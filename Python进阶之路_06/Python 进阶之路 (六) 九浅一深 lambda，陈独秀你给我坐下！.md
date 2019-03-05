## **lambda是什么** ##

大家好，今天给大家带来的是有关于Python里面的lambda表达式详细解析。lambda在Python里面的用处很广，但说实话，我个人认为有关于lambda的讨论不是如何使用的问题，而是该不该用的问题。接下来还是通过大量实例和大家分享我的学习体验，可能最后你也会得出和我一样的结论。

好啦，首先让我们先搞明白基础定义，lambda到底是什么？

>**Lambda表达了Python中用于创建匿名函数的特殊语法。我们将lambda语法本身称为lambda表达式，从这里得到的函数称之为lambda函数。**

其实总结起来，lambda可以理解为一个小的匿名函数，***lambda函数可以使用任意数量的参数，但只能有一个表达式。***估计有JavaScript ES6经验的朋友们听上去会很亲切，具体函数表达式如下：

 - 模板： lambda argument: manipulate(argument)
 - 参数：argument就是这个匿名函数传入的参数，冒号后面是我们对这个参数的操作方法

让我们参考上面的定义模板和参数, 直接看一个最简单的例子:

```
add_one = lambda x:x+1       # 1个参数，执行操作为+1
add_nums = lambda x,y:x+y    # 2个参数，执行操作为相加

print(add_one(2))            # 调用add_one
print(add_nums(3,7))         # 调用add_nums

>>>   3 
     10
```
相比大家已经发现lambda匿名函数的特点了，就是对于较为简单的功能，无需自己def一个了，单行就可以写下，传参和执行方法一气呵成



## **lambda用法详解** ##

接下来让我们看看lambda的实际应用，就我自己使用lambda的体验来说，从来没有单独用过，lambda一般情况下是和map，filter，reduce这些超棒的内置函数以及dict，list,tuple,set 等数据结构混用，这样才能发挥它的最大效果，如果有朋友还不太熟悉这些内置函数，可以看一下我上一篇文章 [Python 进阶之路 (五) map, filter, reduce, zip 一网打尽][1]

好了，闲话少说，下面让我们一个个来看

#### **lambda + map** ####
首先出场的是lambda+map的组合，先看下面这个例子：

```
numbers = [1,2,3,4,5]
add_one = list(map(lambda n:n+1,numbers))  #map(fun,sequence)

print(list(add_one))
print(tuple(add_one))

Out: [2, 3, 4, 5, 6]
     (2, 3, 4, 5, 6)
```
这个是我们上一期的例子，实现一个数组（元组）每个元素+1，让我们回忆一下map的用法
map(fun,sequence)，fun是传递的方法，sequence是一个可迭代的序列，这里我们的fun就是匿名函数
lambda n:n+1，这里非常完美的解释了lambda的设计初衷，因为如果没有lambda，我们的解决方案是这样:

```
def add(num):
    return num+1

numbers = [1,2,3,4,5]
add_one = list(map(add,numbers))
print(add_one)
print(tuple(add_one))
```
显然易见，这里的add方法有点多余，所以用lambda代替是个好的选择。让我们再看下一个例子,这是我自己备份日志时写的一小段代码,命名不是很规范：

```
from datetime import datetime as dt
logs = ['serverLog','appLog','paymentLog']
format ='_{}.py'.format(dt.now().strftime('%d-%m-%y'))
result =list(map(lambda x:x+format,logs))   # 利用map+lambda 实现字符串拼接
print(result)

Out:['serverLog_11-02-19.py', 'appLog_11-02-19.py', 'paymentLog_11-02-19.py']

```
这里和刚才的加1例子差不多，但是换成了字符串的拼接，然而我这里用lambda并不是很好的解决方案，最后我们会说，现在大家应该对map + lambda 有一些感觉了，让我们再来个和dict字典互动的例子：

```
person =[{'name':'Lilei',
          'city':'beijing'},
         {'name':'HanMeiMei',
          'city':'shanghai'}]

names=list(map(lambda x:x['name'],person))
print(names)

Out：['Lilei', 'HanMeiMei']

```
好了，看到这里对于map+lambda的用法大家已经很清楚了应该~

#### **lambda + filter** ####

lambda和filter的组合也很常见，用于特定筛选条件下，现在让我们来看上篇文章filter的例子，就应该很好理解了：

```
numbers = [0, 1, 2, -3, 5, -8, 13]

# 提取奇数
result = filter(lambda x: x % 2, numbers)
print("Odd Numbers are :",list(result))

# 提取偶数
result = filter(lambda x: x % 2 == 0, numbers)
print("Even Numbers are :",list(result))

#提取正数
result = filter(lambda x: x>0, numbers)
print("Positive Numbers are :",list(result))

Out：Odd Numbers are : [1, -3, 5, 13]
     Even Numbers are : [0, 2, -8]
     Positive Numbers are : [1, 2, 5, 13]
```

这里无非就是我们把filter(fun,sequence)里面的fun换成了我们的lambda，只是lambda的函数部分（x%2,x%2==0,x>0）都是可以返回True或者False来判断的，符合fiter的要求，用刚才李雷和韩梅梅的例子也是一个道理：

```
person =[{'name':'Lilei',
          'city':'beijing'},
         {'name':'HanMeiMei',
          'city':'shanghai'}]

names=list(filter(lambda x:x['name']=='Lilei',person)) # 提取李雷的信息
print(names)

Out：[{'name': 'Lilei', 'city': 'beijing'}]

```

#### **lambda + reduce** ####
还是让我们看一下上篇文章的例子：

    from functools import reduce          # Only Python 3
    numbers = [1,2,3,4]
    result_multiply = reduce((lambda x, y: x * y), numbers)
    result_add = reduce((lambda x,y: x+y), numbers)
    
    print(result_multiply)
    print(result_add)
    
    Out：24
         10

这个例子用lambda和reduce的配合实现了list求累积和和累积乘法。
有意思的是这个例子具有两面性，一方面展示了lambda和reduce如何一起使用，另一方面也引出了接下来我想说的重点：lambda真的值得用吗？到底应该怎么用？

## **避免过度使用lambda** ##

通过上面的例子大家已经看到了lambda的实际应用场景，但是这里我想和大家分享一下我的看法:**我认为lambda的缺点略多于优点，应该避免过度使用lambda**

首先，这仅仅是我的个人看法哈，希望大家理解，我为什么这么说呢，首先让我们拿lambda方法和常规def做个对比，我发现lambda和def的主要不同点如下：

 - 可以立即传递（无需变量）
 - 只需一行代码，简洁（未必高效）
 - 可以会自动返回，无需return
 - lambda函数没有函数名称

有关优点大家都可以看到，我主要想说一下它的缺点，首先，**从真正需求出发，我们在大多数时候是不需要lambda的，因为总可以找到更好的替代方法**，现在我们一起看一下刚才lambda+reduce 的例子，我们用lambada实现的结果如下：

```
from functools import reduce          # Only Python 3
    numbers = [1,2,3,4]
    result_multiply = reduce((lambda x, y: x * y), numbers)
    result_add = reduce((lambda x,y: x+y), numbers)
```
这里用lambda并没有实现简单高效的目的，因为我们有现成的sum和mul方法可以用：

```

from functools import reduce
from operator import mul

numbers = [1,2,3,4]
result_add = sum(numbers)
result_multiply =reduce(mul,numbers)

print(result_add)
print(result_multiply)

Out: 10
     24

```
结果是一样的，但是显然用sum和mul的方案更加高效。再举个常见的例子说明，假如我们有一个list存储了各种颜色，现在要求把每个颜色首字母大写，如果用lambda写出是这样：

```
colors = ['red','purple','green','blue']
result = map(lambda c:c.capitalize(),colors)
print(list(result))

Out：['Red', 'Purple', 'Green', 'Blue']

```
看着似乎不错，挺简洁的，但是我们有更好的方法：

```

colors = ['red','purple','green','blue']
result = [c.capitalize() for c in colors]
print(result)

Out：['Red', 'Purple', 'Green', 'Blue']

```
用sorted还能处理首字母不规范的情况，连排序都省了：

```
colors = ['Red','purple','Green','blue']
print(sorted(colors,key=str.capitalize))

Out:['blue', 'Green', 'purple', 'Red']

```
还有一个主要原因就是: **lambda函数没有函数名称**。所以在代码交接，项目移植的场景中会给团队带来很多困难，多写个函数add_one()没什么坏处，因为大家都很容易理解，知道它是执行+1的功能，但是如果团队里你在自己负责的模块使用了很多lambda，会给其他人理解带来很多麻烦

## **适合lambda的场景** ##

话又说回来，存在即合理，那么真正需要我们使用lambda的是哪些场景呢：

 1. 你需要的方法是很简单的(+1，字符串拼接等),该函数不值得拥有一个名字
 2. 使用lambda表达式，会比我们能想到的函数名称更容易理解
 3. 除了lambda，没有任何python提供的函数可以实现目的
 4. 团队中所有成员都掌握lambda，大家同意你用
 

还有一种场景非常适用，就是在给其他人制造自己很专业的错觉时，比如：


> **哎呀，小老弟，听说你学了Python，知道lambda不？ 没听过？不行啊，白学了！来来来，让我给你讲讲。。。此处省略1万字**


## **总结** ##

今天为大家九浅一深地讲解了lambda的用法和使用场景，所谓九浅一深，就是90%情况下用于创建简单的匿名函数，10%的情况稍微复杂（我这个借口找的太好了）

总而言之就是，任何事情都具有两面性，我们在使用lambda之前应该先停下来，问问自己是不是真的需要它。

当然，如果需要和别人忽悠的时候都是正反一张嘴，lambda是好是坏全看我们自己怎么说，吹牛时请遵守如下原则，屡试不爽：

> **如果你说一个女大学生晚上卖淫就是可耻，但如果改成一个妓女利用业余时间努力学习就励志多了**！

lambda也是如此



  [1]: https://segmentfault.com/a/1190000018114755?_ea=6883963




