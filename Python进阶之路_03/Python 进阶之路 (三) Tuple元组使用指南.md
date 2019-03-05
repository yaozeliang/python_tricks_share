## 比List更安全的数据类型 ##
大家好，今天为大家介绍一种更为安全的Python内置数据类型:tuple（元组），以及它的基础用法
##元组是什么##
元组(tuple)是另一种有序的数据类型，与list比较类似。主要不同的一点是tuple被创建后就不能对其进行修改。所以，tuple与list不同，没有append(),pop(),insert()这些方法可以使用。获取元素的方法和list是一样的，可以通过索引来访问（也是从0开始的），只不过不能赋值成为其他的元素。

**因为 tuple不可变，所以代码更安全。如果可以的话，我们尽量使用tuple代替list。**

### 创建元组

```
# 定义一个空的tuple
t = ()
print(t)
Out:()
```
只有1个元素的元组在进行定义的时候，需要加一个逗号 , 来消除歧义，否则定义的就不是一个元组而是元素本身
```
t1 = (5)
t2 = (5, )
print(t1)
print(t2)

Out: 5
    (5,)
```

```
tup4 = (1, 2, 3, 4, 5 );          # 创建时直接赋值
tup5 = "a", "b", "c", "d";        # 创建时直接赋值
print(tup4)
print(tup5)

Out：(1, 2, 3, 4, 5)
     ('a', 'b', 'c', 'd')
```
一旦创建完，比如tup4 和 tup5 这两个tuple不能变了，它也没有append()，insert()这样的方法。其他获取元素的方法和List是一样的，我们可以正常地使用tup4[0]，tup5[-1]，但不能赋值成另外的元素


### 访问元组

访问元组和List基本一样，我们可以用切片很容易的查看元组中的元素，这里不多说，看个小栗子:

```
tup4 = (1, 2, 3, 4, 5 );
tup5 = "a", "b", "c", "d";
print(tup4[0])
print(tup5[1:3])
print(tup5[::-1])
print(sorted(tup5,reverse=True))   # 使用sorted结果变成了List

Out: 1
    ('b', 'c')
    ('d', 'c', 'b', 'a')
    ['d', 'c', 'b', 'a']

```
>理解元组的不可变

上面已经说过了，元组是不可变的，让我们来看下面这个小栗子：

```
    test=('a','b',[1,2,3])
    print(test)
    test[2][0]=100
    print(test)

Out: ('a', 'b', [1, 2, 3])
     ('a', 'b', [100, 2, 3])

```
不知道有没有朋友会有疑问？ 你不是说元组不能变嘛，这里怎么回事，test元组的第三个元素是List，这里我们修改了List的值结果不是变了吗？

这里要给大家说明一下，tuple不可变指的是指向不变，也就是说test[2]永远指向List[1,2,3]，这里是因为List可变，所以我们才能修改为[100,2,3],但是改变前后test[2]的指向没有发生任何变化，如果我们想要直接改变test[2]的值，就会发现如下错误

```
test[2]=[100,2,3]
Out: TypeError: 'tuple' object does not support item assignment

```
理解了“指向不变”后，如果我们需要创建一个内容也不变的tuple怎么做？那就必须保证tuple的每一个元素本身也不能变。

### 元组的连接

如前面所说，元组是不可改的，但是可以连接，我们可以使用 + 对元组进行连接：

```
t1 = (2,3,5)
t2 = ('ricequant','python')
t3 = t1 + t2
print(t3)
Out：(2, 3, 5, 'ricequant', 'python')

```
### 元组的删除

元组中的元素不能被删除，但是我们可以使用 **del** 删除整个元组，删除后可以重新定义,非常简单，不多说啦

```
person = ('xiaobai',18,'paris')
print(person)
del person
print(person)

Out: ('xiaobai', 18, 'paris')
     NameError: name 'person' is not defined

```

### 元组的解包
这里是比较有意思的地方，假设我们有一个元组t如下:

```
t = ('foo', 'bar', 'baz', 'qux')
```
当我们创建 t 时，实际上就是一个打包，过程展示如下图：

![enter image description here](https://lh3.googleusercontent.com/xvPerbm_Mekia9m-yJDSNQUtqjYo0lCqGVffEDaod8NN3CumyuhhvrSxWzZcSnw-EsX-QhgeeROk)
那如果是解包呢？ 换过来就行了呀

```
t = ('foo', 'bar', 'baz', 'qux')
(s1, s2, s3, s4) = t
print(s1,s2,s3,s4)

Out：foo bar baz qux
```
当我们执行(s1, s2, s3, s4) = t的时候，实际发生的情况如下：

![enter image description here](https://lh3.googleusercontent.com/0jX7S5f-rehj6ujAiHkqsLrY6wAWCmwsuwDoVKyw_hxTWvdXRRmCzkijttALVofYslI3otgOzIE-)
这里注意一点，如果我们尝试解包一个元祖是传递的变量和元组实际元素数量不相符时会产生错误：

```
(s1, s2, s3) = t
ValueError: too many values to unpack (expected 3)

(s1, s2, s3, s4, s5) = t
ValueError: not enough values to unpack (expected 5, got 4)

```


### 元组的互换swap

其实Python里面还有一种非常简单的创建元组的方法，那就是逗号，我们如果用逗号分隔一些元素，会自动生成一个元组：

```
a = 'foo'
b = 'bar'
x= a, b
print(x)

Out：('foo', 'bar')

```
如果做一个简单的互换很容易，只要这样就可以了：
```
x= b,a
print(x)
Out：('bar', 'foo')
```

### 元组的常用方法汇总

 - tup.index(x, [start, [stop]])) 返回元组中start到stop索引中第一个值为 x的元素在整个列表中的索引。如果没有匹配的元素就会返回一个错误。
   
 - tup.count(x) 返回 x 在元组中出现的次数。
 - cmp(tuple1, tuple2) 比较元组中两个元素。
 - len(tuple) 计算元组元素个数。
 - max(tuple) 返回元组中元素最大值。
 - min(tuple) 返回元组中元素最小值。
 - tuple(seq) 将列表转换为元组。
 - 元组不提供字符串、列表和字典中的方法。如果相对元组排序，通常先得将它转换为列表并使其成为一个可变对象，才能获得使用排序方法，或使用sorted内置方法。

## 总结 ##

今天为大家讲解了我知道的有关tuple的一切，也为大家展示了一些常规操作，希望能够帮助到大家，马上就要到初五了，迎财神，吃饺子！！！ 希望大家在2019大吉大利，大发横财！！







