## 前情回顾 ##
大家好，我又回来了。今天我会继续和大家分享itertools这个神奇的自带库，首先，让我们回顾一下上一期结尾的时候我们讲到的3个方法：

 - **combinations()**
 - **combinations_with_replacement()**
 - **permutations()**

让我们对这3个在排列组合中经常会使用到的函数做个总结

#### combinations()

> 基础概念

 - 模板：combinations(iterable, n)
 - 参数：iterable为可迭代的对象（list,tuple...）, n为想要的组合包含的元素数
 - 返回值： 返回在iterable里n个元素组成的tuple的全部组合（不考虑顺序，元素自身不可重复）

> 应用实例

```python

import itertools as it
lst = [1,2,3]
result = list(it.combinations(lst,2))
print(result)

Out: [(1, 2), (1, 3), (2, 3)]

```
这里我们从lst这个list里面选取所有由两个元素组成的组合，得到结果如图所示，这里没有考虑顺序，因此我们不会看到（1，2）和（2，1）被算作两种组合，元素自身不可重复，所以没有（1，1），（2，2），（3，3）的组合出现


#### combinations_with_replacement()

> 基础概念

 - 模板：combinations_with_replacement(iterable, n)
 - 参数：iterable为可迭代的对象（list,tuple...）, n为想要的组合包含的元素数
 - 返回值： 返回在iterable里n个元素组成的tuple的全部组合（不考虑顺序，元素自身可重复）

> 应用实例


```python

import itertools as it
lst = [1,2,3]
result = list(it.combinations_with_replacement(lst,2))
print(result)

Out: [(1, 1), (1, 2), (1, 3), (2, 2), (2, 3), (3, 3)]


```
和刚才的区别是多了（1，1），（2，2），（3，3）的组合，也就是说允许每个元素自己和自己组合


#### permutations()

> 基础概念

 - 模板：permutations(iterable, n=None)
 - 参数：iterable为可迭代的对象（list,tuple...）, n为想要的组合包含的元素数
 - 返回值： 返回在iterable里n个元素组成的tuple的全部组合（考虑顺序，元素自身不可重复）

> 应用实例


```python

import itertools as it
lst = [1,2,3]
result = list(it.permutations(lst,2))
print(result)

Out: [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]



```

我们用permutations得到的结果是自身元素不能重复的情况下，一个iterable里面由n个元素构成的全部组合，考虑顺序

#### 不同点汇总

我们这里可以简单汇总一下三个函数的不同点，汇总一张精华满满的表格送个大家，希望大家如果日后有一天需要用到的话可以回来我这里看看，顺便给勤劳的博主点个赞也是好的👍


|  函数      | 组合的元素个数|  示例 list   | 输出结果  | 特点 |  
|---|---|---|---|---|
| combinations() |   2  | [1,2,3]   | (1, 2), (1, 3), (2, 3)  | 元素自身不能重复，不考虑顺序    | 
| combinations_with_replacement() |  2   |  [1,2,3]    | (1, 1), (1, 2), (1, 3), (2, 2), (2, 3), (3, 3)  | 元素自身能重复，不考虑顺序  | 
| permutations() |  2   |  [1,2,3]   | (1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)  | 元素自身不能重复，考虑顺序   | 



## 数字序列 ##

在完美的收尾了上一期的内容后我们可以继续前进了，使用itertools，我们可以轻松地在无限序列上生成迭代器。接下来我们主要看看和数字序列相关的方法和功能。

首先我们来看一个生成奇数偶数的例子，如果不知道itertools的情况下，我们利用生成器的解决方案如下：

```python
def evens():
    """Generate even integers, starting with 0."""
    n = 0
    while True:
        yield n
        n += 2

def odds():
    """Generate odd integers, starting with 1."""
    n = 1
    while True:
        yield n
        n += 2


evens = evens()
even_numbers = list(next(evens) for _ in range(5))
print(even_numbers)

odds = odds()
odd_numbers = list(next(odds) for _ in range(5))
print(odd_numbers)

Out：[0, 2, 4, 6, 8]
     [1, 3, 5, 7, 9]

```
现在我们可以利用itertools里面的it.count()方法进行优化：

```python
import itertools as it

evens = it.count(step=2)
even_numbers = list(next(evens) for _ in range(5))
print(even_numbers)

odds = it.count(start=1, step=2)
odd_numbers = list(next(odds) for _ in range(5))
print(odd_numbers)

Out：[0, 2, 4, 6, 8]
     [1, 3, 5, 7, 9]
```
itertools.count（）这个方法主要就是用来计数，默认从0开始，我们可以通过设置start关键字参数从任何数字开始计数，默认为0.同样也可以设置step关键字参数来确定从count（）返回的数字之间的间隔，默认为1。

再来看其它两个用到itertools count方法的例子：

```python
>>> count_with_floats = it.count(start=0.5, step=0.75)
>>> list(next(count_with_floats) for _ in range(5))
[0.5, 1.25, 2.0, 2.75, 3.5]
```

可以用来计算float类型

```python
>>> negative_count = it.count(start=-1, step=-0.5)
>>> list(next(negative_count) for _ in range(5))
[-1, -1.5, -2.0, -2.5, -3.0]
```

或是负数也没有问题

> 在某些方面，count（）类似于内置range（）函数，但count（）总是返回无限序列。
> 无限序列的好处在于它不可能完全迭代

count()函数甚至模拟了内置的enumrate（）功能：

```python
import itertools as it
print(list(zip(it.count(), ['a', 'b', 'c'])))

Out:[(0, 'a'), (1, 'b'), (2, 'c')]

```
#### 其他有意思的方法

> ***repeat(object, times=1)***

首先让我们看一下itertools里面的repeat放法，它的功能是返回一个值的无限序列：

```python
all_ones = it.repeat(1)  # 1, 1, 1, 1, ...
all_twos = it.repeat(2)  # 2, 2, 2, 2, ...
```

如果我们希望可以指定返回序列的长度，我们可以在方法的第二个参数加上想要的序列长度即可：

```python
five_ones = it.repeat(1, 5)  # 1, 1, 1, 1, 1
three_fours = it.repeat(4, 3)  # 4, 4, 4
```


> ***cycle(iterable)***


接着估计你可能想到了，那如果我们想要不断循环两个数呢？答案是itertools的cycle方法：

```
alternating_ones = it.cycle([1, -1])  # 1, -1, 1, -1, 1, -1, ...
```

如果你想要输出上面的alternating_ones是不可能的，因为这是一个无限序列，你会收到下面的错误：

```
Traceback (most recent call last):
  File "C:\Users\Desktop\itertools.py", line 48, in <module>
    alternating_ones = list(it.cycle([0, 1]))
MemoryError
```

> ***accumulate(iterable, func=operator.add)***


itertools.accumulate（）函数, 这个函数有些特殊，它接受两个参数 ：

-  一个可迭代的输入
-  一个二进制函数func（即一个具有两个输入的函数）

并返回一个迭代器，用于将func应用于输入元素的累积结果。看一个小栗子：

```python
>>> import operator
>>> list(it.accumulate([1, 2, 3, 4, 5], operator.add))
[1, 3, 6, 10, 15]
```

accumulate（）返回的迭代器中的第一个值始终是输入序列中的第一个值。在这个例子中，是1,因为1是 [1,2,3,4,5]中的第一个值。
输出迭代器中的下一个值是输入序列的前两个元素的总和：add（1,2）= 3，所以操作模式如下：

 - ***add(3, 3) = add(add(1, 2), 3) = 6***

以此类推，最终得到最后的答案。实际上accumulate（）的第二个参数本身就是默认为operator.add（），因此前面的示例可以简化为：

```python
>>> list(it.accumulate([1, 2, 3, 4, 5]))
[1, 3, 6, 10, 15]

```

我们也可以自己添加别的方法到第二个参数里：

```python
>>> list(it.accumulate([9, 21, 17, 5, 11, 12, 2, 6], min))
[9, 9, 9, 5, 5, 5, 2, 2]
```

好啦，itertools 有关于数字序列方面的方法我就简单介绍到这里啦，有需要的朋友可以自己去看看，其实还是非常实用的，不是类似lambda那些花哨的方法

## 对List和字符串的相关操作


> itertools.product  实现交叉组合

```python

>>> product([1, 2], ['a', 'b'])
(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')

```
此处实现两个可迭代序列的元素组合。


> itertools.tee 从一个输入序列生成任意数量的生成器

```
>>> iter1, iter2 = it.tee(['a', 'b', 'c'], 2)
>>> list(iter1)
['a', 'b', 'c']
>>> list(iter2)
['a', 'b', 'c']
```

注意这里的iter1和iter2相互不会一影响。是一个深复制

> islice(iterable, stop) islice(iterable, start, stop, step=1) 切片

```

>>> islice([1, 2, 3, 4], 3)
1, 2, 3

>>> islice([1, 2, 3, 4], 1, 2)
2, 3
```
这里和list最大的区别在于返回对象是一个迭代器，并不是一个list，islice(iterable, stop)中stop是切片截至的index，和list切片一样，并不会包括stop本身的值。如果想要指定切片起始位置和不长，就使用islice(iterable, start, stop, step=1)

> chain(*iterables) 

```
>>> chain('abc', [1, 2, 3])  #<type 'itertools.chain'> 
'a', 'b', 'c', 1, 2, 3


```
返回一个链对象，其__next __（）方法返回第一个iterable中的元素，直到它耗尽，然后是来自下一个iterable的元素，直到所有的iterables都用完为止。


这里有一点需要注意，chain（）函数有一个类方法.from_iterable（），它将一个iterable作为参数。迭代的元素本身必须是可迭代的，因此效应是chain.from_iterable（）某种程度上可以实现类似 list.extend() 或者 list.append() 的功能， 我们一起看一个混合了很多itertools中其他方法的综合小栗子：

```
import itertools as it
cycle = it.chain.from_iterable(it.repeat('abc'))
result = list(it.islice(cycle,8))
print(result)

Out: ['a', 'b', 'c', 'a', 'b', 'c', 'a', 'b']

```

这里其实it.chain.from_iterable里面甚至可以放进一个无限序列，不一定是定长的。

## 总结 ##

这一期的内容没有那么长，我们简单了解了一下itertools的基础好用的方法，下一期就是简单实战了，我自己在网上找了一些非常不错的案例，照猫画虎练习了一下，打算在下一期和大家一起分享。这次的文章就到这里啦，我们下期见！！！
 









