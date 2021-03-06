﻿## 前言 ##

大家好，今天想和大家分享一下我的itertools学习体验及心得，itertools是一个Python的自带库，内含多种非常实用的方法，我简单学习了一下，发现可以大大提升工作效率，在sf社区内没有发现十分详细的介绍，因此希望想自己做一个学习总结。也和朋友们一起分享一下心得

首先，有关itertools的详细介绍，我参考的是Python 3.7官方文档：[itertools — Functions creating iterators for efficient looping][1]，大家感兴趣可以去看看，目前还没有中文版本，十分遗憾，这里不得不吐槽一句，为啥有日语，韩语，中文的版本没有跟上呢？


![没有中文文档](https://lh3.googleusercontent.com/mVs73y97tE1BoC5JgSJnIbgakzLasu8Y4Jp15lHLECGIOwjcNf8tFH2GMtSyj2VRXmIxDORicnm9 )


书规正传，itertools 我个人评价是Python3里最酷的东西! 如果你还没有听说过它，那么你就错过了Python 3标准库的一个最大隐藏宝藏，是的，我很快就抛弃了刚刚分享的collections模块：[Python 进阶之路 (七) 隐藏的神奇宝藏：探秘Collections][2]，毕竟男人都是大猪蹄子

网上有很多优秀的资源可用于学习itertools模块中的功能。但对我而言，官方文档本身总是一个很好的起点。学会做甜点之前，总是要会最基础的面包。这篇文章便是基本基于文档归纳整理而来。

我在学习后的整体感受是，关于itertools只知道它包含的函数是远远不够的。真正的强大之处在于组合这些功能以创建**快速，占用内存效率极少,漂亮优雅**的代码。

在这篇很长的文章里，我会全面复盘我的学习历程，争取全面复制每一个细节，在开始之前，如果朋友们还不太知道迭代器和生成器是什么，可以参考以下科普扫盲：

 - [菜鸟教程  迭代器生成器][3]
 - [廖雪峰的迭代器讲解][4]
 - [廖雪峰的生成器讲解][5]

## 神奇的itertools##

好啦，坐好扶稳，我们准备上车了，根据官方文档的定义：

> This module implements a number of iterator building blocks inspired by constructs from APL, Haskell, and SML. Each has been recast in a form suitable for Python.

翻译过来大概就是它是一个实现了许多迭代器构建的模块，它们受到来自APL，Haskell和SML的构造的启发......可以提高效率啥的，

这主要意味着itertools中的函数是在迭代器上“操作”以产生更复杂的迭代器。
例如，考虑内置的zip（）函数，该函数将任意数量的iterables作为参数，并在其相应元素的元组上返回迭代器：

```
print(list(zip([1, 2, 3], ['a', 'b', 'c'])))
Out：[(1, 'a'), (2, 'b'), (3, 'c')]

```
这里让我们思考一个问题，这个zip函数到底是如何工作的？

与所有其他list一样，[1,2,3] 和 ['a'，'b'，'c'] 是可迭代的，这意味着它们可以一次返回一个元素。
从技术上讲，任何实现：

 - ***.__ iter __（）***

 - 或**.__ getitem __（）**

方法的Python对象都是可迭代的。如果对这方面有疑问，大家可以看前言部分提到的教程哈

其实有关iter（）这个内置函数，当在一个list或其他可迭代的对象 x 上调用时，会返回x自己的迭代器对象：

```
iter([1, 2, 3, 4])  
iter((1,2,3,4))
iter({'a':1,'b':2})

Out：<list_iterator object at 0x00000229E1D6B940>
     <tuple_iterator object at 0x00000229E3879A90>
     <dict_keyiterator object at 0x00000229E1D6E818>
```

实际上，zip（）函数通过在每个参数上调用iter（），然后使用next（）推进iter（）返回的每个迭代器并将结果聚合为元组来实现。 zip（）返回的迭代器遍历这些元组

而写到这里不得不回忆一下，之前在 [Python 进阶之路 (五) map, filter, reduce, zip 一网打尽][6]我给大家介绍的神器map（）内置函数，其实某种意义上也是一个迭代器的操作符而已，它以最简单的形式将单参数函数一次应用于可迭代的sequence的每个元素:

 - 模板： map(func,sequence)

```
list(map(len, ['xiaobai', 'at', 'paris']))
Out: [7, 2, 5]
```

参考map模板，不难发现：map（）函数通过在sequence上调用iter（），使用next（）推进此迭代器直到迭代器耗尽，并将func 应用于每步中next（）返回的值。在上面的例子里，在['xiaobai', 'at', 'paris']的每个元素上调用len（），从而返回一个迭代器包含list中每个元素的长度

由于迭代器是可迭代的，因此可以用 zip（）和 map（）在多个可迭代中的元素组合上生成迭代器。
例如，以下对两个list的相应元素求和：

```
a = [1, 2, 3]
b = [4, 5, 6]
list(map(sum, zip(a,b)))

Out: [5, 7, 9]

```
这个例子很好的解释了如何构建itertools中所谓的 ***“迭代器代数”*** 的函数的含义。我们可以把itertools视为一组构建砖块，可以组合起来形成专门的“数据管道”，就像这个求和的例子一样。

> *其实在Python 3里，如果我们用过了map() 和 zip() ，就已经用过了itertools，因为这两个函数返回的就是迭代器！*

我们使用这种 itertools 里面所谓的 ***“迭代器代数”*** 带来的好处有两个:

 1. **提高内存效率  (lazy evaluation)**
 2. **提速**

可能有朋友对这两个好处有所疑问，不要着急，我们可以分析一个具体的场景：

> 
现在我们有一个list和正整数n，编写一个将list 拆分为长度为n的组的函数。为简单起见，假设输入list的长度可被n整除。例如，如果输入= [1,2,3,4,5,6] 和 n = 2，则函数应返回 [（1,2）,（3,4）,（5,6）]。

我们首先想到的解决方案可能如下：

```
def naive_grouper(lst, n):
    num_groups = len(lst) // n
    return [tuple(lst[i*n:(i+1)*n]) for i in range(num_groups)]
```
我们进行简单的测试，结果正确：

```
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
naive_grouper(nums, 2)
Out: [(1, 2), (3, 4), (5, 6), (7, 8), (9, 10)]

```
**但是问题来了，如果我们试图传递一个包含1亿个元素的list时会发生什么？我们需要大量内存！即使有足够的内存，程序也会挂起一段时间，直到最后生成结果**

这个时候如果我们使用itertools里面的迭代器就可以大大改善这种情况：

```
def better_grouper(lst, n):
    iters = [iter(lst)] * n
    return zip(*iters)
```
这个方法中蕴含的信息量有点大，我们现在拆开一个个看，表达式 [iters（lst）] * n 创建了对同一迭代器的n个引用的list:

```
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
iters = [iter(nums)] * 2
list(id(itr) for itr in iters)    # Id 没有变化，就是创建了n个索引

Out: [1623329389256, 1623329389256]

```
接下来，zip（* iters）在 iters 中的每个迭代器的对应元素对上返回一个迭代器。当第一个元素1取自“第一个”迭代器时，“第二个”迭代器现在从2开始，因为它只是对“第一个”迭代器的引用，因此向前走了一步。因此，zip（）生成的第一个元组是（1,2）。

此时，iters中的所谓 “两个”迭代器从3开始，所以当zip（）从“first”迭代器中拉出3时，它从“second”获得4以产生元组（3,4）。这个过程一直持续到zip（）最终生成（9,10）并且iters中的“两个”迭代器都用完了：

> **注意： 这里的"第一个"，"第二个" ，"两个"都是指向一个迭代器，因为id没有任何变化！！**

最后我们发现结果是一样的：

```
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
list(better_grouper(nums, 2))

Out: [(1, 2), (3, 4), (5, 6), (7, 8), (9, 10)]
```
但是，这里我做了测试，发现二者的消耗内存是天壤之别，而且在使用iter+zip()的组合后，执行速度快了500倍以上，大家感兴趣可以自己测试，把 nums 改成 xrange(100000000) 即可

现在让我们回顾一下刚刚写好的better_grouper(lst, n) 方法，不难发现，这个方法存在一个明显的缺陷：如果我们传递的n不能被lst的长度整除，执行时就会出现明显的问题：

```
>>> nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> list(better_grouper(nums, 4))
[(1, 2, 3, 4), (5, 6, 7, 8)]
```
我们发现分组输出中缺少元素9和10。发生这种情况是因为一旦传递给它的最短的迭代次数耗尽，zip（）就会停止聚合元素。而我们想要的是不丢失任何元素。因此解决办法是我们可以使用 **itertools.zip_longest（）** 它可以接受任意数量的 iterables 和 fillvalue 这个关键字参数，默认为None。我们先看一个简单实例

```
>>> import itertools as it
>>> x = [1, 2, 3, 4, 5]
>>> y = ['a', 'b', 'c']

>>> list(zip(x, y))                     # zip总是执行完最短迭代次数停止
[(1, 'a'), (2, 'b'), (3, 'c')]

>>> list(it.zip_longest(x, y))          
[(1, 'a'), (2, 'b'), (3, 'c'), (4, None), (5, None)]

```
这个例子已经非常清晰的体现了zip（）和 zip_longest()的区别，现在我们可以优化 better_grouper 方法了：

```
import itertools as it


def grouper(lst, n, fillvalue=None):
    iters = [iter(lst)] * n
    return it.zip_longest(*iters, fillvalue=fillvalue)  #  默认就是None
    
```

我们再来看优化后的测试：

```
>>> nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> print(list(grouper(nums, 4)))
[(1, 2, 3, 4), (5, 6, 7, 8), (9, 10, None, None)]
```
已经非常理想了，各位老铁们可能还没有意识到，***我们刚刚所做的一切就是创建itertools 里面grouper方法的全过程！***

现在让我们看看真正的 [官方文档][7] 里面所写的grouper方法：

![enter image description here](https://lh3.googleusercontent.com/zOBdfwpw4MDIvL7CQm8tFDNxfPYrI0pY93fxCcOZGD3f1PcDs5P1zpZ-lgOWYAGj0vFmcGSQddjV)

和我们写的基本一样，除了可以接受多个iterable 参数，用了*args

最后心满意足的直接调用一下：


![enter image description here](https://lh3.googleusercontent.com/EfzrpaOPnpZaBI4niqk5BK0rzKMu7BZEu2dgIQ_MhxHtqd79YtsPl1cqwwarercge9AAAAJhecwV)

输出结果如下：

![enter image description here](https://lh3.googleusercontent.com/hSqzo7NNt0KPKyKeB3DbJIz5rbcJYBKEkvMkeq5u_IbTEqk14CQ82oDWGhFGj2DKHV_-6LXVg9b4)



## 暴力求解(brute force) 

首先基础概念扫盲，所谓暴力求解是算法中的一种，简单来说就是 ***利用枚举所有的情况，或者其它大量运算又不用技巧的方式，来求解问题的方法。***
我在看过暴力算法的广义概念后，首先想到的居然是盗墓笔记中的王胖子 

> 如果有看过盗墓笔记朋友，你会发现王胖子其实是一个推崇暴力求解的人，在无数次遇到困境时祭出的”枚举法“，就是暴力求解，例如我印象最深的是云顶天宫中，一行人被困在全是珠宝的密室中无法逃脱，王胖子通过枚举排除所有可能性，直接得到”身边有鬼“ 的最终解。

>**PS: 此处致敬南派三叔，和那些他填不上的坑**

扯远了，回到现实中来，我们经常会碰到如下的经典题目：

> 你有三张20美元的钞票，五张10美元的钞票，两张5美元的钞票和五张1美元的钞票。可以通过多少种方式得到100美元？

为了暴力破解这个问题，我们只要把所有组合的可能性罗列出来，然后找出100美元的组合即可，首先，让我们创建一个list，包含我们手上所有的美元：

```
bills = [20, 20, 20, 10, 10, 10, 10, 10, 5, 5, 1, 1, 1, 1, 1]

```
这里itertools会帮到我们。 **itertools.combinations（）** 接受两个参数 
  - 一个可迭代的input
  - 正整数n  

最终会在 input中 n 个元素的所有组合的元组上产生一个迭代器。

```
import  itertools as it

bills = [20, 20, 20, 10, 10, 10, 10, 10, 5, 5, 1, 1, 1, 1, 1]

result =list(it.combinations(bills, 3))
print(len(result))  # 455种组合
print(result)

Out: 455
     [(20, 20, 20), (20, 20, 10), (20, 20, 10), ... ]
```

我仅剩的高中数学知识告诉我其实这个就是一个概率里面的 C 15(下标)，3(上标)问题，好了，现在我们拥有了各种组合，那么我们只需要在各种组合里选取总数等于100的，问题就解决了：

```
makes_100 = []
for n in range(1, len(bills) + 1):
    for combination in it.combinations(bills, n):
        if sum(combination) == 100:
            makes_100.append(combination)
```

这样得到的结果是包含重复组合的，我们可以在最后直接用一个set过滤掉重复值，最终得到答案：

```
import  itertools as it

bills = [20, 20, 20, 10, 10, 10, 10, 10, 5, 5, 1, 1, 1, 1, 1]

makes_100 = []
for n in range(1, len(bills) + 1):
    for combination in it.combinations(bills, n):
        if sum(combination) == 100:
            makes_100.append(combination)

print(set(makes_100))

Out：{(20, 20, 10, 10, 10, 10, 10, 5, 1, 1, 1, 1, 1),
      (20, 20, 10, 10, 10, 10, 10, 5, 5),
     (20, 20, 20, 10, 10, 10, 5, 1, 1, 1, 1, 1),
     (20, 20, 20, 10, 10, 10, 5, 5),
     (20, 20, 20, 10, 10, 10, 10)}
     
```
所以最后我们发现一共有5种方式。 现在让我们把题目换一种问法，就完全不一样了：

> 现在要把100美元的钞票换成零钱，你可以使用任意数量的50美元，20美元，10美元，5美元和1美元钞票，有多少种方法？

在这种情况下，我们没有预先设定的钞票数量，因此我们需要一种方法来使用任意数量的钞票生成所有可能的组合。为此，我们需要用到**itertools.combinations_with_replacement（）**函数。

它就像combination（）一样，接受可迭代的输入input 和正整数n，并从输入返回有n个元组的迭代器。不同之处在于combination_with_replacement（）允许元素在它返回的元组中重复，看一个小栗子：

```
>>> list(it.combinations_with_replacement([1, 2], 2))   #自己和自己的组合也可以
[(1, 1), (1, 2), (2, 2)]

```

对比 itertools.combinations（）：

```
>>> list(it.combinations([1, 2], 2))   #不允许自己和自己的组合
[(1, 2)]
```
所以针对新问题，解法如下：

```
bills = [50, 20, 10, 5, 1]
make_100 = []
for n in range(1, 101):
    for combination in it.combinations_with_replacement(bills, n):
        if sum(combination) == 100:
            makes_100.append(combination)
```

最后的结果我们不需要去重，因为这个方法不会产生重复组合：

```
>>> len(makes_100)
343
```
如果你亲自运行一下，可能会注意到输出需要一段时间。那是因为它必须处理96,560,645种组合！这里我们就在执行暴力求解

另一个“暴力” 的itertools函数是permutations（），它接受单个iterable并产生其元素的所有可能的排列（重新排列）：

```
>>> list(it.permutations(['a', 'b', 'c']))
[('a', 'b', 'c'), ('a', 'c', 'b'), ('b', 'a', 'c'),
 ('b', 'c', 'a'), ('c', 'a', 'b'), ('c', 'b', 'a')]
```
任何三个元素的可迭代对象（比如list）将有六个排列，并且较长迭代的对象排列数量增长得非常快。实际上，长度为n的可迭代对象有n！排列：

![enter image description here](https://lh3.googleusercontent.com/9xsL9ehezwGrpva3jp5dyNC9zp--4hi3jwG03d9rWMYhv4ylNx4mFDU8EWrHZUXVxpLNZDa0mYbH)


只有少数输入产生大量结果的现象称为组合爆炸，在使用combination（），combinations_with_replacement（）和permutations（）时我们需要牢记这一点。

说实话，通常最好避免暴力算法，但有时我们可能必须使用（比如算法的正确性至关重要，或者必须考虑每个可能的结果）
## 小结 ##

由于篇幅有限，我先分享到这里，这篇文章我们主要深入理解了以下函数的基本原理：

 - map（）
 - zip（）
 - itertools.combinations
 - itertools.combinations_with_replacement 
 - itertools.permutations 

在下一篇文章我会先对最后三个进行总结，然后继续和大家分享itertools里面各种神奇的东西

  [1]: https://docs.python.org/3.7/library/itertools.html
  [2]: https://segmentfault.com/a/1190000018145641
  [3]: http://www.runoob.com/python3/python3-iterator-generator.html
  [4]: https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143178254193589df9c612d2449618ea460e7a672a366000
  [5]: https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014317799226173f45ce40636141b6abc8424e12b5fb27000
  [6]: https://segmentfault.com/a/1190000018114755
  [7]: https://docs.python.org/3.6/library/itertools.html#itertools-recipes



