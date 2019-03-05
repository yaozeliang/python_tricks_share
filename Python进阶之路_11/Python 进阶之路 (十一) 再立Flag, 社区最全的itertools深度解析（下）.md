


## **简单实战** ##


大家好，我又来了，在经过之前两篇文章的介绍后相信大家对itertools的一些常见的好用的方法有了一个大致的了解，我自己在学完之后仿照别人的例子进行了真实场景下的模拟练习，今天和大家一起分享，有很多部分还可以优化，希望有更好主意和建议的朋友们可以留言哈，让我们一起进步

## 实战:分析标准普尔500指数 ##

### 数据源及目标 
在这个例子中，我们首先尝试使用itertools来操作大型数据集：标准普尔500指数的历史每日价格数据。 我会在这个部分的最后附上下载链接和py文件，这里的数据源来自[雅虎财经][1]

> 目标： 找到标准普尔500指数的单日最大收益，最大损失（百分比），和最长的增长周期

首先我们手上得到了 SP500.csv ，让我们对数据有个大概的印象，前十行的数据如下：

```python
Date,Open,High,Low,Close,Adj Close,Volume
1950-01-03,16.660000,16.660000,16.660000,16.660000,16.660000,1260000
1950-01-04,16.850000,16.850000,16.850000,16.850000,16.850000,1890000
1950-01-05,16.930000,16.930000,16.930000,16.930000,16.930000,2550000
1950-01-06,16.980000,16.980000,16.980000,16.980000,16.980000,2010000
1950-01-09,17.080000,17.080000,17.080000,17.080000,17.080000,2520000
1950-01-10,17.030001,17.030001,17.030001,17.030001,17.030001,2160000
1950-01-11,17.090000,17.090000,17.090000,17.090000,17.090000,2630000
1950-01-12,16.760000,16.760000,16.760000,16.760000,16.760000,2970000
1950-01-13,16.670000,16.670000,16.670000,16.670000,16.670000,3330000
```

为了实现目标，具体思路如下：
 - 读取csv文件，并利用 Adj Close这一列转换为每日百分比变化的序列，代表收益，命名为gain
 - 找到gain这一序列中的最大值和最小值,并且找到对应的日期，当然，有可能会出现对应多个日期的情况，我们这里选取日期最近的就好。
 - 定义一个sequence叫做growth_streaks，其中包含了所有 gain中出现的连续为正值的元素组成的tuple，我们要找到这些tuples中长度最长的一个，从而定位其对应的开始时间和结束时间,当然这里也是一样，有可能出现最大长度一样的的情况，这种情况下，我们还是选择日期最近的。

这里有关百分比的计算公式如下：

![enter image description here](https://lh3.googleusercontent.com/m6HynNinsY1r-Wxzyvv01nV9639C0H1hrjoO5NSSJrzaEn5ZuesOeMyr-oA8jKBVTVIt-BPz7Hjf)



### 分步实现 

首先在这里，我们会经常处理日期，为了方便后续操作，这里我们引入collections模块的namedtuple来实现对日期的相关操作：

```python
from collections import namedtuple


class DataPoint(namedtuple('DataPoint', ['date', 'value'])):
    __slots__ = ()

    def __le__(self, other):
        return self.value <= other.value

    def __lt__(self, other):
        return self.value < other.value

    def __gt__(self, other):
        return self.value > other.value
```
这里有很多小技巧，之后我会再系统的开一个Python OOP笔记，会为大家都讲到，这里面涉及的小知识点如下：

 - slots ：这是一个节省变量内存的好东西，__slot__后面一般都是跟class中 init 方法里面用到的变量，好处在于能够大量节省内存
 - namedtuple：可以实现类似属性一样调用tuple里面的元素，我在collections里面详细说过，大家可以看看：[Python 进阶之路 (七) 隐藏的神奇宝藏：探秘Collections][2]
 - le:运算符重载，可以得到class中一个变量的长度，必须是整数，也就是说如果传入的是list，dict，tuple，set这些一定没有问题，因为这些序列的长度一定是整数，这里面传递的是tuple（）
 - lt：运算符重载（less than ）：可以实现利用 < 比较一个class的不同对象中的值大小的比较
 - gt：运算符重载（greater than）：可以实现利用 > 比较一个class的不同对象中的值大小的比较


下面为了唤醒大家的记忆，我这里快速举一个有关于namedtuple，le，lt，gt的小栗子：

```
from collections import namedtuple
class Person(namedtuple('person', ['name', 'age','city','job'])):

    def __le__(self):
        return len(self)

    def __lt__(self,other):
        return self.age < other.age

    def __gt__(self,other):
        return self.age > other.age


xiaobai = Person('xiaobai', 18, 'paris','student')
laobai = Person('Walter White',52, 'albuquerque','cook')


print('Infomation for first person: ', xiaobai)     # 显示全部信息
print('Age of second person is: ', laobai.age)    # 根据name得到tuple的数据
print(len(xiaobai))
print(xiaobai > laobai)
print(xiaobai < laobai)


Out: Infomation for first person:  Person(name='xiaobai', age=18, city='paris',job='student')
     Age of second person is:  52
     4
     False
     True
```
如果大家对这个例子中的一些地方还有疑问，不用担心，我会在下一个专栏Python OOP学习笔记中和大家慢慢说的
。好的，现在回到刚才的实战：

```
from collections import namedtuple

class DataPoint(namedtuple('DataPoint', ['date', 'value'])):
    __slots__ = ()

    def __le__(self, other):
        return self.value <= other.value

    def __lt__(self, other):
        return self.value < other.value

    def __gt__(self, other):
        return self.value > other.value
```

这里我们的DataPoint类有两个主要属性，一个是datetime类型的日期，一个是当天的标普500值

接下来让我们读取csv文件,并将每行中的Date和Adj Close列中的值存为DataPoint的对象，最后把所有的对象组合为一个sequence序列：

```
import csv
from datetime import datetime


def read_prices(csvfile, _strptime=datetime.strptime):
    with open(csvfile) as infile:
        reader = csv.DictReader(infile)
        for row in reader:
            yield DataPoint(date=_strptime(row['Date'], '%Y-%m-%d').date(),
                            value=float(row['Adj Close']))


prices = tuple(read_prices('SP500.csv'))
```

read_prices（）生成器打开 SP500.csv 并使用 csv.DictReader（）读取数据的每一行。DictReader（）将每一行作为 OrderedDict 返回，其中key是每行中的列名。

对于每一行，read_prices（）都会生成一个DataPoint对象，其中包含“Date”和“Adj Close”列中的值。 最后，完整的数据点序列作为元组提交给内存并存储在prices变量中



> **Ps： Ordereddict是我在collections中漏掉的知识点，我马上会补上，大家可以随时收藏[Python 进阶之路 (七) 隐藏的神奇宝藏：探秘Collections][3]，我会继续更新**

接下来我们要把prices这个转变为表达每日价格变化百分比的序列，利用的公式就是刚才提到的，如果忘了的朋友可以往回翻~

```
gains = tuple(DataPoint(day.date, 100*(day.value/prev_day.value - 1.))
                for day, prev_day in zip(prices[1:], prices))
```

为了得到标普500单日最大涨幅，我们可以用一下方法：

```
max_gain = DataPoint(None, 0)
for data_point in gains:
    max_gain = max(data_point, max_gain)

print(max_gain)   # DataPoint(date='2008-10-28', value=11.58)
```

我们可以把这个方法用之前提到过的reduce简化一下：

```
import functools as ft

max_gain = ft.reduce(max, gains)

print(max_gain)  # DataPoint(date='2008-10-28', value=11.58)
```
这里有关reduce 和 lambda的用法，我们可以通过一个小栗子来回忆一下：

```
import functools as ft
x = ft.reduce(lambda x,y:x+y,[1, 2, 3, 4, 5])
print(x)

Out: 15
```
当然，如果求和在实际场景直接用sum就好，这里只是为了让大家有个印象，如果回忆不起来的老铁们也没有关系，轻轻点击以下链接立刻重温：

 - [Python 进阶之路 (五) map, filter, reduce, zip 一网打尽][4]
 - [Python 进阶之路 (六) 九浅一深 lambda，陈独秀你给我坐下！][5]

好了，书规正传，我们发现用reduce改进了for循环后得到了同样的结果，单日最大涨幅的日期也一样，但是这里需要注意的是reduce和刚才的for循环完全不是一回事

我们可以想象一下，假如CSV文件中的数据每天都是跌的话。 max_gain最后到底是多少？

在 for 循环中，首先设置max_gain = DataPoint（None，0），因此如果没有涨幅，则最终的max_gain值将是此空 DataPoint 对象。但是，reduce（）解决方案会返回最小的单日跌幅，这不是我们想要的，可能会引入一个难以找到的bug

这就是itertools可以帮助到我们的地方。 itertools.filterfalse（）函数有两个参数：一个返回True或False的函数，和一个可迭代的输入。它返回一个迭代器，是迭代结果都为False的情况。这里是个小栗子：

```python
import itertools as it
only_positives = it.filterfalse(lambda x: x <= 0, [0, 1, -1, 2, -2])
print(list(only_positives))


Out：[1, 2]

```
所以现在我们可以用 itertools.filterfalse（）去除掉gains中那些小于0或者为负数的值，这样reduce会仅仅作用在我们想要的正收益上：

```
max_gain = ft.reduce(max, it.filterfalse(lambda p: p <= 0, gains))

```
这里我们默认为gains中一定存在大于0的值，这也是事实，但是如果假设gains中没有的话，我们会报错，因此在使用itertools.filterfalse（）的实际场景中要注意到这一点。

针对这种情况，可能你想到的应对方案是在合适的情况下添加TryExpect捕获错误，但是reduce有个更好的解决方案，reuce里面可以传递第三个参数，用做reduce返回结果不存在时的默认值，这一点和字典的get方法有异曲同工之妙，如果对get有疑问的朋友可以回顾我之前的文章：[Python 进阶之路 (二) Dict 进阶宝典，初二快乐！][6]，还是看一个小栗子：

```
>>> ft.reduce(max, it.filterfalse(lambda x: x <= 0, [-1, -2, -3]), 0)
0
```
这回很好理解了，因此我们应用到我们标准普尔指数的实战上：

```python
zdp = DataPoint(None, 0)  # zero DataPoint
max_gain = ft.reduce(max, it.filterfalse(lambda p: p.value <= 0, diffs), zdp)
```

同理，对于标普500单日最大跌幅我们也照猫画虎：

```
max_loss = ft.reduce(min, it.filterfalse(lambda p: p.value > 0, gains), zdp)

print(max_loss)  # DataPoint(date='2018-02-08', value=-20.47)
```
根据我们的数据源是2018年2月8号那一天，我没有谷歌查询那一天发生了什么，大家感兴趣可以看看哈，但是应该是没有问题的，因为数据源来自雅虎财经

现在我们已经得到了标普500历史上的单日最大涨跌的日期，我们接下来要找到它的最长时间段，其实这个问题等同于在gains序列中找到最长的连续为正数的点的集合，itertools.takewhile（）和itertools.dropwhile（）函数非常适合处理这种情况。

itertools.takewhile（）接受两个参数，一个为判断的条件，一个为可迭代的序列，会返回第一个判断结果为False时之前的迭代过的所有元素，下面的小栗子很好的解释了这一点
```
it.takewhile(lambda x: x < 3, [0, 1, 2, 3, 4])  # 0, 1, 2
```
itertools.dropwhile() 则恰恰相反：

```
it.dropwhile(lambda x: x < 3, [0, 1, 2, 3, 4])  # 3, 4

```

因此我们可以创建一下方法来实现在gains中找到连续为正数的序列：

```
def consecutive_positives(sequence, zero=0):
    def _consecutives():
        for itr in it.repeat(iter(sequence)):
            yield tuple(it.takewhile(lambda p: p > zero,
                                     it.dropwhile(lambda p: p <= zero, itr)))
    return it.takewhile(lambda t: len(t), _consecutives())
    
growth_streaks = consecutive_positives(gains, zero=DataPoint(None, 0))
longest_streak = ft.reduce(lambda x, y: x if len(x) > len(y) else y,
                           growth_streaks)
```

最后让我们看一下完整的代码：

```python
from collections import namedtuple
import csv
from datetime import datetime
import itertools as it
import functools as ft


class DataPoint(namedtuple('DataPoint', ['date', 'value'])):
    __slots__ = ()

    def __le__(self, other):
        return self.value <= other.value

    def __lt__(self, other):
        return self.value < other.value

    def __gt__(self, other):
        return self.value > other.value


def consecutive_positives(sequence, zero=0):
    def _consecutives():
        for itr in it.repeat(iter(sequence)):
            yield tuple(it.takewhile(lambda p: p > zero,
                                     it.dropwhile(lambda p: p <= zero, itr)))
    return it.takewhile(lambda t: len(t), _consecutives())


def read_prices(csvfile, _strptime=datetime.strptime):
    with open(csvfile) as infile:
        reader = csv.DictReader(infile)
        for row in reader:
            yield DataPoint(date=_strptime(row['Date'], '%Y-%m-%d').date(),
                            value=float(row['Adj Close']))


# Read prices and calculate daily percent change.
prices = tuple(read_prices('SP500.csv'))
gains = tuple(DataPoint(day.date, 100*(day.value/prev_day.value - 1.))
              for day, prev_day in zip(prices[1:], prices))

# Find maximum daily gain/loss.
zdp = DataPoint(None, 0)  # zero DataPoint
max_gain = ft.reduce(max, it.filterfalse(lambda p: p.value <= zdp, gains))
max_loss = ft.reduce(min, it.filterfalse(lambda p: p.value > zdp, gains), zdp)


# Find longest growth streak.
growth_streaks = consecutive_positives(gains, zero=DataPoint(None, 0))
longest_streak = ft.reduce(lambda x, y: x if len(x) > len(y) else y,
                           growth_streaks)

# Display results.
print('Max gain: {1:.2f}% on {0}'.format(*max_gain))
print('Max loss: {1:.2f}% on {0}'.format(*max_loss))

print('Longest growth streak: {num_days} days ({first} to {last})'.format(
    num_days=len(longest_streak),
    first=longest_streak[0].date,
    last=longest_streak[-1].date
))
```

最终结果如下：

```
Max gain: 11.58% on 2008-10-13
Max loss: -20.47% on 1987-10-19
Longest growth streak: 14 days (1971-03-26 to 1971-04-15)
```

数据源可以点击[这里][7]下载


## 总结 ##

这次我为大家梳理一个利用itertools进行了简单实战的小栗子，这里我们旨在多深入了解itertools，但是真实的生活中，遇到这种问题，哪有这么麻烦，一个pandas包就搞定了，我以后会和大家分享和pandas有关的知识，这一次接连三期的itertools总结希望大家喜欢。

itertools深度解析至此全剧终。


  [1]: https://finance.yahoo.com/quote/%5EGSPC?p=%5EGSPC&guccounter=1&guce_referrer=aHR0cHM6Ly9yZWFscHl0aG9uLmNvbS9weXRob24taXRlcnRvb2xzLw&guce_referrer_sig=zWyw2jQagm5AfJpcL0XEuPudKH5TSk2liQKEqwDG-0k
  [2]: https://segmentfault.com/a/1190000018145641
  [3]: https://segmentfault.com/a/1190000018145641
  [4]: https://segmentfault.com/a/1190000018114755
  [5]: https://segmentfault.com/a/1190000018121906
  [6]: https://segmentfault.com/a/1190000018102693
  [7]: https://raw.githubusercontent.com/realpython/materials/master/itertools-in-python3/SP500.csv
