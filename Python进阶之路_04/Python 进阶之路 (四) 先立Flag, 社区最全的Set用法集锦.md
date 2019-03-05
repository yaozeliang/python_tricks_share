


## Set是什么 ##
大家好，在上一期详解tuple元组的用法后，今天我们来看Python里面最后一种常见的数据类型：集合(Set)

与dict类似，set也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。创建一个set，需要提供一个list作为输入集集合，重复元素在set中会被自动被过滤，通过add(key)方法往set中添加元素，重复添加不会有效果。如果现在你发现我讲的很模糊请不要着急。稍后会有海量例子为大家详解。

总而言之,Set具有三个显著特点：
- 无序
- 元素是独一无二的，不允许出现重复的元素
- 可以修改集合本身，但集合中包含的元素必须是不可变类型

现在让我们开启Set奇幻之旅，我希望这篇文章是SegmentFault社区对于Set介绍最全的模范，哈哈！

### 定义一个Set ###
我们有两种方式可以创建一个Set，可以使用内置的**set()**方法，或是使用中括号**{}**
创建模板如下：

```
                    x = set(<iter>)         
                    x = {<obj>, <obj>, ..., <obj>}

```
现在让我们来看例子~
>**set()内置方法创建**

```
x = set(['foo', 'bar', 'baz', 'foo', 'qux'])   # 传入List
print(x)

y = set(('foo', 'bar', 'baz', 'foo', 'qux'))   #传入元组
print(y)

Out: {'qux', 'foo', 'bar', 'baz'}  # 注意到无序了吧~
     {'bar', 'qux', 'baz', 'foo'}
     
```
这里要注意用set()内置方法创建时一定要传递一个可以迭代的参数，还有从输出结果相信大家已经发现set的第一个特点了：无序

字符串也是可迭代的，因此字符串也可以传递给set（）

```
s = 'quux'
a = set(s)
print(a)

Out: {'u', 'q', 'x'}      # 无序，唯一

```
这里又体现了set的第二个特点：元素唯一性

>**{} 方法创建**

```
>>> x = {'foo', 'bar', 'baz', 'foo', 'qux'}
>>> x
{'qux', 'foo', 'bar', 'baz'}
```
这里考虑到之后例子太多，实在不能每次都打print啦，这种形式大家看的更清楚，这个直接用{}创建很简单，只要传递进元素就行啦
>**创建空集合**

Set可以是空的。但是，请记住Python将空花括号{}解释为空字典，因此定义空集的唯一方法是使用set（）函数

```
>>> x = set()
>>> type(x)
<class 'set'>

>>> x = {}
>>> type(x)
<class 'dict'>

```
一个空集合用布尔类型显示为False

```
>>> x = set()
>>> bool(x)
False
>>> x or 1
1
>>> x and 1
set()
```

>**对比小结**

对于这两种方法创建Set，本质区别在于以下两点
1. set（）的参数是可迭代的。它会生成要放入集合中的所有元素组成的List。
2. 花括号 {} 中的对象完整地放入集合中，即使它们是可迭代的。

>**补充说明**

集合中的元素可以是不同类型的对象，不一定非要是同一类型的，可以包含不同类型，比如：

```
>>> x = {42, 'foo', 3.14159, None}
>>> x
{None, 'foo', 42, 3.14159}
```
但同时不要忘记set元素必须是不可变的。例如，元组可以包括在集合中：

```
>>> x = {42, 'foo', (1, 2, 3), 3.14159}
>>> x
{42, 'foo', 3.14159, (1, 2, 3)}
```
但列表和字典是可变的，因此它们不能成为Set的元素：

```
>>> a = [1, 2, 3]
>>> {a}
Traceback (most recent call last):
  File "<pyshell#70>", line 1, in <module>
    {a}
TypeError: unhashable type: 'list'


>>> d = {'a': 1, 'b': 2}
>>> {d}
Traceback (most recent call last):
  File "<pyshell#72>", line 1, in <module>
    {d}
TypeError: unhashable type: 'dict'
```

### Set大小以及成员 ###

len（）函数返回集合中元素的数量，而in和not in运算符可用于测试是否为Set中的元素：

```
>>> x = {'foo', 'bar', 'baz'}
>>> len(x)
3

>>> 'bar' in x
True
>>> 'qux' in x
False
```

### Set基本操作 ###


#### **方法和运算符**

许多可用于Python其他数据类型的操作对集合没有意义。例如，无法对集合建立索引或切片。但是，Python在set对象上提供了运算符，这些操作符其实很多和数学里是一模一样的，相信数学好的朋友们对这部分简直不要太熟悉

所以对于Set的操作除了用普通的内置方法，我们也可以使用运算符，比较方便

#### **Union 并集**  

 - 用法：计算两个或更多集合的并集。 
 - 方法: x1.union(x2[, x3 ...])
 - 运算符：x1 | x2 [| x3 ...]
让我们新建两个Set做测试：

```
>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'baz', 'qux', 'quux'}
```

现在我们想求x1,x2的并集，如下图所示：
![enter image description here](https://lh3.googleusercontent.com/MLemOR9-u30wyOFjmaNgaDFRCP7H9KIXv8BFSejQmr9RJP2jmIzC-NSjCc9rLtYJpw4CzidZihpY)

----------


具体实现方法如下，或是用方法，或是用操作符：
```
>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'baz', 'qux', 'quux'}

>>> x1.union(x2)
{'foo', 'qux', 'quux', 'baz', 'bar'}

>>> x1 | x2
{'foo', 'qux', 'quux', 'baz', 'bar'}
```
如果有两个以上的Set也是没有问题的，原理都是一样的：

```
>>> a = {1, 2, 3, 4}
>>> b = {2, 3, 4, 5}
>>> c = {3, 4, 5, 6}
>>> d = {4, 5, 6, 7}

>>> a.union(b, c, d)
{1, 2, 3, 4, 5, 6, 7}

>>> a | b | c | d
{1, 2, 3, 4, 5, 6, 7}
```


#### **Intersection 交集**  

 - 方法: x1.intersection(x2[, x3 ...])
 - 运算符：x1 & x2 [& x3 ...]
 - 用法：计算两个或更多集合的交集。 

现在还让我们用刚才创建好的两个set，所求部分如下图：

![enter image description here](https://lh3.googleusercontent.com/N4tZ7lsIZbLu9wn5mdQcJSAyQ2CvT1YPc85aqHP9oNC2NnHtf8SMgx7lf0B85B9SiQlp_tAAdox7)
```
实现仍然是两种方法：

>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'baz', 'qux', 'quux'}

>>> x1.intersection(x2)
{'baz'}

>>> x1 & x2
{'baz'}
```
多个集合的情况公示和方法依然有效，结果仅包含所有指定集合中都存在的元素。

```
>>> a = {1, 2, 3, 4}
>>> b = {2, 3, 4, 5}
>>> c = {3, 4, 5, 6}
>>> d = {4, 5, 6, 7}

>>> a.intersection(b, c, d)
{4}

>>> a & b & c & d
{4}
```

#### **Difference 差集**  

 - 方法: x1.difference(x2[, x3 ...])
 - 运算符：x1 - x2 [- x3 ...]
 - 用法：计算两个或更多集合的差集。大白话说就是x1去除x1和x2的共有元素 
下图所示为x1.difference(x2)的目标结果：

![enter image description here](https://lh3.googleusercontent.com/EUqRwlEdtqq8rDwKm86K8LRuPYcBJ649uMg5IynzkS51MrZTLc1ZfwviCd5-llVqKMjuJVNagfr5)

```
    >>> x1 = {'foo', 'bar', 'baz'}
    >>> x2 = {'baz', 'qux', 'quux'}
    
    >>> x1.difference(x2)
    {'foo', 'bar'}
    
    >>> x1 - x2
    {'foo', 'bar'}
```
还是老样子，适用于2个及以上的集合：

```
>>> a = {1, 2, 3, 30, 300}
>>> b = {10, 20, 30, 40}
>>> c = {100, 200, 300, 400}

>>> a.difference(b, c)
{1, 2, 3}

>>> a - b - c
{1, 2, 3}
```
指定多个集合时，操作从左到右执行。在上面的示例中，首先计算a  -  b，得到{1,2,3,300}。然后从该集合中减去c，留下{1,2,3}，具体流程如下图所示：

![enter image description here](https://lh3.googleusercontent.com/vppUaLiFSrmWHvBm0jTuxzatOkIRJuUgIil1WZ8H-XtfCipoEnV3oP8mwNINazd_fYTiKrCw-2zT)


#### **Symmetric Difference 对称差集**  

 - 方法: x1.symmetric_difference(x2)
 - 运算符：x1 ^ x2 [^ x3 ...]
 - 用法：计算两个或更多集合的差集。大白话说就是x1去除x1和x2的共有元素 
下图所示为x1.symmetric_difference(x2)的目标结果：


![enter image description here](https://lh3.googleusercontent.com/2SKdyDWQVM0ljrEpBe7O41ojlsj0Yl52NjKwJqC9I9kh2Rv6_113FSZop7c7jSUElT2bRRjQBLiY)
实现方法如下；

```
>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'baz', 'qux', 'quux'}

>>> x1.symmetric_difference(x2)
{'foo', 'qux', 'quux', 'bar'}

>>> x1 ^ x2
{'foo', 'qux', 'quux', 'bar'}
```
老规矩，支持2个及以上set的连续操作:

```
>>> a = {1, 2, 3, 4, 5}
>>> b = {10, 2, 3, 4, 50}
>>> c = {1, 50, 100}

>>> a ^ b ^ c
{100, 5, 10}
```
当指定多个集合时，操作从左到右执行,奇怪的是，***虽然 ^ 运算符允许多个集合，但.symmetric_difference（）方法不允许***

```
>>> a = {1, 2, 3, 4, 5}
>>> b = {10, 2, 3, 4, 50}
>>> c = {1, 50, 100}

>>> a.symmetric_difference(b, c)
Traceback (most recent call last):
  File "<pyshell#11>", line 1, in <module>
    a.symmetric_difference(b, c)
TypeError: symmetric_difference() takes exactly one argument (2 given)
```

#### **x1.isdisjoint(x2) 判断是否相交**  

 - 方法: x1.isdisjoint(x2) 
 - 用法：确定两个集合是否具有任何共同的元素

```
>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'baz', 'qux', 'quux'}

>>> x1.isdisjoint(x2)
False

>>> x2 - {'baz'}
{'quux', 'qux'}
>>> x1.isdisjoint(x2 - {'baz'})
True
```
从这个栗子可以看出，如果两个Set没有共同元素返回True，如果有返回True，如果返回True同时也意味着
他们之间的交集为空集，这个很好理解：

```
>>> x1 = {1, 3, 5}
>>> x2 = {2, 4, 6}

>>> x1.isdisjoint(x2)
True
>>> x1 & x2
set()
```
***注意：目前还没有运算符对应这个方法***

#### **x1.issubset(x2)   判断x1是否为x2子集**  

 - 方法: x1.issubset(x2)
 - 运算符：x1 <= x2
 - 用法：如果返回True，x1为x2子集，反之返回False


```
>>> x1 = {'foo', 'bar', 'baz'}
>>> x1.issubset({'foo', 'bar', 'baz', 'qux', 'quux'})
True

>>> x2 = {'baz', 'qux', 'quux'}
>>> x1 <= x2
False
```
一个集合本身当然是它自己的子集啦：

```
>>> x = {1, 2, 3, 4, 5}
>>> x.issubset(x)
True
>>> x <= x
True
```
#### **x1<x2   判断x1是否为x2的真子集**  

 - 运算符：x1<x2 
 - 用法：判断x1是否为x2的真子集，如果返回True，x1为x2的真子集，反之返回False

首先。。。让我们回顾一下数学知识：真子集与子集类似，除了集合不能相同。如果x1的每个元素都在x2中，并且x1和x2不相等，则集合x1被认为是另一个集合x2的真子集

换个高大上的说法也可以：如果集合A⊆B，存在元素x∈B，且元素x不属于集合A，我们称集合A与集合B有真包含关系，集合A是集合B的真子集（proper subset）。记作A⊊B（或B⊋A），读作“A真包含于B”（或“B真包含A”）

```
>>> x1 = {'foo', 'bar'}
>>> x2 = {'foo', 'bar', 'baz'}
>>> x1 < x2
True

>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'foo', 'bar', 'baz'}
>>> x1 < x2
False
```
虽然Set被认为是其自身的子集，但它本身并不是自己的真子集：

```
>>> x = {1, 2, 3, 4, 5}
>>> x <= x
True
>>> x < x
False
```

***注意：目前还没有方法对应这个运算符***

#### **x1.issuperset(x2)  判断x1是否为x2的超集**  

 - 方法：x1.issuperset(x2)
 - 运算符：x1 >= x2
 - 用法：判断x1是否为x2的超集，如果是返回True，反之返回False

```
>>> x1 = {'foo', 'bar', 'baz'}

>>> x1.issuperset({'foo', 'bar'})
True

>>> x2 = {'baz', 'qux', 'quux'}
>>> x1 >= x2
False
```
我们刚才已经看到过了一个Set是它自己本身的子集，这里也是一样的，它同时也是自己的超集

```
>>> x = {1, 2, 3, 4, 5}
>>> x.issuperset(x)
True
>>> x >= x
True
```


####  x1 > x2  判断x1是否为x2的真超集

 - 运算符：x1 > x2
 - 用法：判断x1是否为x2的真超集，如果是返回True，反之返回False

真超集与超集相同，除了集合不能相同。如果x1包含x2的每个元素，并且x1和x2不相等，则集合x1被认为是另一个集合x2的真超集。

```
>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'foo', 'bar'}
>>> x1 > x2
True

>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'foo', 'bar', 'baz'}
>>> x1 > x2
False
```
一个集合不是它自己的真超集，和真子集的原理相同

```
>>> x = {1, 2, 3, 4, 5}
>>> x > x
False
```

### 对Set进行修改 ###

虽然集合中包含的元素必须是不可变类型，但可以修改集合本身。与上面的操作类似，可以使用多种运算符和方法来更改集合的内容。

#### x1.update(x2) 通过union修改集合元素 
 - 方法：x1.update(x2[, x3 ...])
 - 运算符：x1 |= x2 [| x3 ...]
 - 用法：通过union修改集合

x1.update(x2) 和 x1 |= x2 作用是向集合x1中添加x2中所有x1不存在的元素。
停下3秒，我仔细读了这句话，觉得我表达的还可以，不知道大家读上去绕不绕，先看例子：

    >>> x1 = {'foo', 'bar', 'baz'}
    >>> x2 = {'foo', 'baz', 'qux'}
    
    >>> x1 |= x2
    >>> x1
    {'qux', 'foo', 'bar', 'baz'}
    
    >>> x1.update(['corge', 'garply'])
    >>> x1
    {'qux', 'corge', 'garply', 'foo', 'bar', 'baz'}

#### x1.intersection(x2) 通过intersection修改集合元素 
 - 方法：x1.intersection_update(x2[, x3 ...])
 - 运算符：x1 &= x2 [& x3 ...]
 - 用法：通过intersection修改集合
x1.intersection_update(x2) 和 x1 &= x2 会让x1只保留x1和x2的交集部分：

```
>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'foo', 'baz', 'qux'}

>>> x1 &= x2
>>> x1
{'foo', 'baz'}

>>> x1.intersection_update(['baz', 'qux'])
>>> x1
{'baz'}
```

#### x1.difference_update(x2) 通过difference修改集合元素 
 - 方法：x1.difference_update(x2[, x3 ...])
 - 运算符：x1 -= x2 [| x3 ...]
 - 用法：通过difference修改集合

x1.difference_update(x2) and x1 -= x2 会让集合x1移除所有在x2出现的属于x1的元素：

```
>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'foo', 'baz', 'qux'}

>>> x1 -= x2
>>> x1
{'bar'}

>>> x1.difference_update(['foo', 'bar', 'qux'])
>>> x1
set()
```

####  x1.symmetric_difference_update(x2) 通过对称差集修改集合元素 
 - 方法：x1.symmetric_difference_update(x2)
 - 运算符：x1 ^= x2
这个我实在用语言解释不清了，看例子容易懂:

```
>>> x1 = {'foo', 'bar', 'baz'}
>>> x2 = {'foo', 'baz', 'qux'}
>>> 
>>> x1 ^= x2
>>> x1
{'bar', 'qux'}
>>> 
>>> x1.symmetric_difference_update(['qux', 'corge'])
>>> x1
{'bar', 'corge'}
```

####  x.add( ) 添加元素

这个就很简单了, 类似List:

```
>>> x = {'foo', 'bar', 'baz'}
>>> x.add('qux')
>>> x
{'bar', 'baz', 'foo', 'qux'}
```

#### x.remove(<elem>) 删除元素

如果删除的元素不存在会抛出异常

```
>>> x = {'foo', 'bar', 'baz'}

>>> x.remove('baz')
>>> x
{'bar', 'foo'}

>>> x.remove('qux')
Traceback (most recent call last):
  File "<pyshell#58>", line 1, in <module>
    x.remove('qux')
KeyError: 'qux'
```
这个时候为了避免出现错误可以用discard方法

```
>>> x = {'foo', 'bar', 'baz'}

>>> x.discard('baz')
>>> x
{'bar', 'foo'}

>>> x.discard('qux')
>>> x
{'bar', 'foo'}
```
利用pop删除随机元素并返回：

```
>>> x = {'foo', 'bar', 'baz'}

>>> x.pop()
'bar'
>>> x
{'baz', 'foo'}

>>> x.pop()
'baz'
>>> x
{'foo'}

>>> x.pop()
'foo'
>>> x
set()
```

利用clear可以清空一个集合：

```
>>> x = {'foo', 'bar', 'baz'}
>>> x
{'foo', 'bar', 'baz'}
>>> 
>>> x.clear()
>>> x
set()
```

## Frozen Sets ##
>**Frozen Sets是什么东西**

Python提供了另一种称为冻结集合Frozen Sets的内置类型，它在所有方面都与集合完全相同，只不过Frozen Sets是不可变的。我们可以对冻结集执行非修改操作，比如：

```
>>> x = frozenset(['foo', 'bar', 'baz'])
>>> x
frozenset({'foo', 'baz', 'bar'})

>>> len(x)
3

>>> x & {'baz', 'qux', 'quux'}
frozenset({'baz'})
```

如果胆敢尝试修改Frozen Sets：

```
>>> x = frozenset(['foo', 'bar', 'baz'])

>>> x.add('qux')
Traceback (most recent call last):
  File "<pyshell#127>", line 1, in <module>
    x.add('qux')
AttributeError: 'frozenset' object has no attribute 'add'

>>> x.pop()
Traceback (most recent call last):
  File "<pyshell#129>", line 1, in <module>
    x.pop()
AttributeError: 'frozenset' object has no attribute 'pop'

>>> x.clear()
Traceback (most recent call last):
  File "<pyshell#131>", line 1, in <module>
    x.clear()
AttributeError: 'frozenset' object has no attribute 'clear'

>>> x
frozenset({'foo', 'bar', 'baz'})
```
>**基本使用举例**

Frozensets在我们想要使用集合的情况下很有用，但需要一个不可变对象。
例如，如果没有Frozen sets我们不能定义其元素也是集合的集合（nested），因为集合元素必须是不可变的，会报错：

```
>>> x1 = set(['foo'])
>>> x2 = set(['bar'])
>>> x3 = set(['baz'])
>>> x = {x1, x2, x3}
Traceback (most recent call last):
  File "<pyshell#38>", line 1, in <module>
    x = {x1, x2, x3}
TypeError: unhashable type: 'set'
```
现在有了 Frozen sets，我们有了解决方案：

```
>>> x1 = frozenset(['foo'])
>>> x2 = frozenset(['bar'])
>>> x3 = frozenset(['baz'])
>>> x = {x1, x2, x3}
>>> x
{frozenset({'bar'}), frozenset({'baz'}), frozenset({'foo'})}
```

## 总结 ##

这一期为大家讲了太多东西，一口老血吐在键盘上，总结不动了
只希望这期Set详解介绍可以帮助到大家，如果帮到了你，就点个赞吧~~
最后再次祝大家猪年大吉！！

