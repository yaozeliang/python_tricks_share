## 开启变身模式 ##

大家好, 从这一期开始，我们会从小白变身为中等小白，在基础起步阶段有太多的东西我没有讲到，但是俗话说的好，无他，但手熟尔，只要多多练习，时间会是最好的证明，相信我们终有一天会成为高手，因此从这一系列开始，让我们一起更上一层楼，还是和往常一样，我也是水平不高，如果有大神发现文章中的错误一定要指出哈~

先皮一下，看个欧阳修的<<卖油翁>>：
>陈康肃公尧咨善射，当世无双，公亦以此自矜。尝射于家圃，有卖油翁释担而立，睨之，久而不去。见其发矢十中八九，但微颔之。

>康肃问曰：“汝亦知射乎？吾射不亦精乎？”翁曰：“无他，但手熟尔。”康肃忿然曰：“尔安敢轻吾射！”翁曰：“以我酌油知之。”乃取一葫芦置于地，以钱覆其口，徐以杓酌油沥之，自钱孔入，而钱不湿。因曰：**“我亦无他，惟手熟尔。**”康肃笑而遣之。
                                 
## List的进阶用法 ##
这里我将会详细介绍一些我认为非常不错的List的使用方法，至于list 自带的一些基础用法，这里不再说明，感兴趣的朋友们可以看看我的基础教程: [Python 基础起步 (五) 一定要知道的数据类型：初识List][1] 和 [Python 基础起步 (六) List的实用技巧大全][2], 好啦，闲话少说，让我们开始吧

### **把其他类型数据结构转化为List类型**

利用**list(target)**即可实现把其他类型的数据结构转化为List类型，十分实用，我们可以把字符串，元组等数据结构转化为List,也可以直接创建,像下图一样：

```

print(list())                            # 创建空List

vowelString = 'aeiou'                    # 把字符串转化为List
print(list(vowelString))

vowelTuple = ('a', 'e', 'i', 'o', 'u')  # 从元组tuple转化为List
print(list(vowelTuple))

vowelList = ['a', 'e', 'i', 'o', 'u']   # 从List到List
print(list(vowelList))

vowelSet = {'a', 'e', 'i', 'o', 'u'}    #  从Set到List
print(list(vowelSet))

Out: []
     ['a', 'e', 'i', 'o', 'u']
     ['a', 'e', 'i', 'o', 'u']
     ['a', 'e', 'i', 'o', 'u']
     ['a', 'e', 'i', 'o', 'u']

```

可能平时用的比较多的一般是从一个dict中提取 keys 和 values ：

```
person = {
    'name':'Peppa Pig',
    'age':15,
    'country':'bratian'}

person_keys = list(person.keys())
person_values = list(person.values())

print(list(person))  # 如果直接转化一个字典只会把keys提取出来
print(list(person.keys()))
print(list(person.values()))

Out:['name', 'age', 'country']    
    ['name', 'age', 'country']             # 把字典中的Keys提出
    ['Peppa Pig', 15, 'bratian']           # 把字典中的Values提出
 
```

这里大家稍微注意下，如果直接用list(person)会默认等同于person.keys()

### **List排序方法汇总**

总体来说，有两种方法最为便捷，**List.sort **或者 **sorted(List)**,具体来看如何实现，先看升序：

```
vowels = ['e', 'a', 'u', 'o', 'i']   
vowels.sort()        
print('Sorted list Acending:', vowels)  # 使用sort
Out: Sorted list Acending: ['a', 'e', 'i', 'o', 'u']
```

```
vowels = ['e', 'a', 'u', 'o', 'i']   
print('Sorted list Acending:', sorted(vowels))  # 使用sorted
Out：Sorted list Acending: ['a', 'e', 'i', 'o', 'u']

```

再来看降序：

```
vowels = ['e', 'a', 'u', 'o', 'i']
vowels.sort(reverse=True)     # 使用sort
print('Sorted list (in Descending):', vowels)
Out: Sorted list (in Descending): ['u', 'o', 'i', 'e', 'a']

```

```
vowels = ['e', 'a', 'u', 'o', 'i']
print('Sorted list (in Descending):', sorted(vowels,reverse=True))  # 使用sorted方法
Out:Sorted list (in Descending): ['u', 'o', 'i', 'e', 'a']
```
其实这里里面还有一个关键的参数是key，我们可以自定义一些规则排序，下面看个例子，用的是默认的len函数
，让我们根据List中每个元素的长度来排序，首先是升序：

```
vowels = ['I', 'live', 'at', 'Paris', 'I','love','Python']
# 使用sort
vowels.sort(key=len)  
print('Sorted by lenth Ascending:', vowels)
Out:Sorted by lenth: ['I', 'I', 'at', 'live', 'love', 'Paris', 'Python']

```

```
vowels = ['I', 'live', 'at', 'Paris', 'I','love','Python']
# 使用 sorted
print('Sorted by lenth Ascending:', sorted(vowels,key=len))
Out：Sorted by lenth Ascending: ['I', 'I', 'at', 'live', 'love', 'Paris', 'Python']

```
降序也差不多啦：

```
vowels = ['I', 'live', 'at', 'Paris', 'I','love','Python']
vowels.sort(key=len,reverse=True)
print('Sorted by lenth Descending:', vowels)
Out：Sorted by lenth Descending: ['Python', 'Paris', 'live', 'love', 'at', 'I', 'I']


```

```
vowels = ['I', 'live', 'at', 'Paris', 'I','love','Python']
print('Sorted by lenth Descending:', sorted(vowels,reverse=True))
Out：Sorted by lenth Descending: ['Python', 'Paris', 'live', 'love', 'at', 'I', 'I']

```

有关这个key我们可以自己定义，请看下面的大栗子：

```
def takeSecond(elem):
    return elem[1]

random = [(2, 2), (3, 4), (4, 1), (1, 3)]
random.sort(key=takeSecond) # sort list with key
print('Sorted list:', random)

Out： [(4, 1), (2, 2), (1, 3), (3, 4)]
```
这里因为列表中的元素都是元组，所以我们规定按照元组的第二个数排序，所以结果如上，默认依然是reverse=False ，也就是升序，至于其他有意思的方法大家可以自己开发

### **逆序输出List**

如果我们想要逆序输出一个List，简单来说有两种方法比较好用，切片和reverse，来看例子：

```
    vowels = ['I', 'live', 'at', 'Paris', 'I','love','Python']
    print(list(reversed(vowels)))
    print(list[::-1])
Out:['Python', 'love', 'I', 'Paris', 'at', 'live', 'I']   
    ['Python', 'love', 'I', 'Paris', 'at', 'live', 'I']   
```



### **利用Range生成List**

Python里面为我们提供了一个超好用的方法range()，具体的用法很多，不在这里一一讲解啦，但是大家可以看一下它如何和List结合在一起的：

```
five_numbers=list(range(5))  #创建0~4的List
print(five_numbers)

odd_numbers=list(range(1,50,2))   #创建0~50所有的奇数的List
print(odd_numbers)

even_numbers=list(range(0,50,2))  #创建0~50所有的偶数的List
print(even_numbers)
```

其实很简单，用的最多的就是range(start, stop, hop)这个结构，其他的大家可以自行查询哈
### **List列表推导式**

其实这个东西无非就是为了减少代码书写量，比较Pythonic的东西，基础模板如下：
```
variable = [out_exp for out_exp in input_list if out_exp == 2]
```
大家直接看比较麻烦，还是直接上代码吧：

```
S = [x**2 for x in range(8)]      # 0~7每个数的平方存为List
V = [2**i for i in range(8)]      # 2的 0~7次方
M = [x for x in S if x % 2 == 0]  #找出在S里面的偶数

print(S)
print(V)
print(M)


Out:   Square of 0 ~7:  [0, 1, 4, 9, 16, 25, 36, 49]
       The x power of 2,(0<x<7) :  [1, 2, 4, 8, 16, 32, 64, 128]
       The even numbers in S:  [0, 4, 16, 36]

```

通过这个小栗子大家估计已经明白用法啦，推导式还可以这么用：

```
verbs=['work','eat','sleep','sit']
verbs_with_ing=[v+'ing' for v in verbs]   # 使一组动词成为现在分词
print(verbs_with_ing)

Out：['working', 'eating', 'sleeping', 'siting']

```

或者加上查询条件if ：

```
all_numbers=list(range(-7,9))
numbers_positive = [n for n in all_numbers if n>0 ]
numbers_negative = [n for n in all_numbers if n<0]
devided_by_two = [n for n in all_numbers if n%2==0]

print("The positive numbers between -7 to 9 are :",numbers_positive)
print("The negative numbers between -7 to 9 are :",numbers_negative )
print("The numbers deveided by 2 without reminder are :",devided_by_two)```

Out：The positive numbers between -7 to 9 are : [1, 2, 3, 4, 5, 6, 7, 8]
     The negative numbers between -7 to 9 are : [-7, -6, -5, -4, -3, -2, -1]
     The numbers deveided by 2 without reminder are : [-6, -4, -2, 0, 2, 4, 6, 8]

```
## 总结 ##

其实客观来讲，list在实际项目中所用的地方有限，特别是处理数据量较大的情况下，因为速度极慢，但是在一些边边角角的地方我个人用的还挺多，尤其是不求速度的地方，总的来讲List是我个人比较喜欢的Python
数据结构，简单好用！！！！！

好啦，我要去吃大餐啦，祝大家新年快乐呀！！！


  [1]: https://segmentfault.com/a/1190000018002749
  [2]: https://segmentfault.com/a/1190000018016421
