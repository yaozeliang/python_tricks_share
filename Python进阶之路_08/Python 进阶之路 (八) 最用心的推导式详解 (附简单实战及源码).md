## 什么是推导式 ##

大家好，今天为大家带来问我最喜欢的Python推导式使用指南，让我们先来看看定义~

推导式（comprehensions）是Python的一种独有特性，推导式是可以从一个数据序列构建另一个新的数据序列的结构体。一般有三种使用最多的推导式：

 - 列表推导式（list comprehensions）
 - 字典推导式（dict comprehensions） 
 - 集合推导式（set comprehensions）

使用推导式可以简化代码的同时提高效率，在我的个人使用场景中，用的最多的还是列表推导式，接下来我会一一介绍这三种常见的推导式，最后通过一个简单实战发现推导式的高效之处

### 列表推导式（list comprehensions）

> 模板

首先，让我们看看使用列表推导式的基础模板：

 - ***[ expression for item in list if conditional ]***
简单来说，遍历一个list，得到每一个元素item，我们相对item进行哪些操作，写在expression里就可以，如果对筛选有什么条件，可以放在if后面

下面可以通过大量实例帮助大家理解

> 使用实例

先看第一个小栗子，在这里我们用for循环常规遍历一个字符串‘human’，把每一字母作为元素放在一个叫h_letters的数组里面：

```
h_letters = []
for letter in 'human':
    h_letters.append(letter)

print(h_letters)![enter image description here](https://lh3.googleusercontent.com/DMMlAkdvzqzK_qr9zOEa6tGcVgSwEeWbBojvqOvBX7ps3nmVBmbzVOvstN-UYFN-CcFQDhU8kxtx)

Out：['h', 'u', 'm', 'a', 'n']

```
如果我们根据列表推导式的定义模板，可以简化如下：

```
h_letters = [ letter for letter in 'human' ]
print( h_letters)

Out: ['h', 'u', 'm', 'a', 'n']
```
这样的话便捷了很多，看上去也很容易理解，这里我们在expression部分什么都没有写，只是提出了每个元素而已，运行时的python执行方式如下：
![enter image description here](https://lh3.googleusercontent.com/DMMlAkdvzqzK_qr9zOEa6tGcVgSwEeWbBojvqOvBX7ps3nmVBmbzVOvstN-UYFN-CcFQDhU8kxtx)

我们可以在expression的部分进行很多操作，比如:

```
h_letters = [ letter.upper() for letter in 'human' ]
print( h_letters)

Out: ['H', 'U', 'M', 'A', 'N']
```
这样我们可以很容易的实现字母的大小写转化

同样的，我们可以在if后面写出筛选条件，比如这里，我们想要提出从-20 ~ 20中所有能被3整除的正数：

```
result = [num for num in range(-20,20)        
          if num %3==0 and num > 0]   

print(result)                   #多个条件可以用and连接

Out: [3, 6, 9, 12, 15, 18]

```
列表推导式的实际应用场景十分广泛，它和lambda不同，是真正好理解，提高效率的python特性之一，这里相信聪明的你已经想到了更多用法！



### 字典推导式（dict comprehensions ) 

> 模板

让我们看先来看使用字典推导式的基础模板：

 - ***{ key:value for key,value in existing_data_structure } ***

这里和list有所不同，因位dict里面有两个关键的属性，key 和 value，但大同小异，我们现在的expression部分可以同时对 key 和 value 进行操作

下面来看最常见的应用

> 使用实例

首先最实用的功能莫过于可以实现一个dict的key，value值互换：

```
person = {'name':'xiaobai','city':'paris'}
person_reverse = {v:k for k,v in person.items()}   #简单互换key和value的值即可

print(person_reverse)
Out: {'xiaobai': 'name', 'paris': 'city'}

```
这里就非常方便了用字典推导式，不然的话如果用for循环，会稍微麻烦一些。
让我们再看下一个很巧的例子：

```
nums = {'a':10,'b':20,'A':5,'B':3,'d':4}

num_frequency  = {k.lower():nums.get(k.lower(),0) + nums.get(k.upper(),0)
                  for k in nums.keys() }

print(num_frequency)

Out: {'a': 15, 'b': 23, 'd': 4}

```
这里使用的就比较灵活，我们有一个数据，key是字母的大小写混在一起，我们想统计同一个key（大小写都包括）所对应的数字出现总和，所以在新建的num_frequency 字典用使用了推导式，这里我们遍历的是dict.keys()配合dict.get()方法，当然，如果仅仅是为了实现这个功能，我们有更好的办法，这里只是为了介绍推导式

再比如下面的例子：

```
fruit = ['apple','banana','organge','mango','peach']

fruit_len = {f:len(f) for f in fruit}
print(fruit_len)

Out：{'apple': 5, 'banana': 6, 'organge': 7, 'mango': 5, 'peach': 5}

```
我们有一个fruit的list，现在想要得到每一种水果的单词长度，就可以通过图中所示的方法实现，非常容易

最后再来看一个字典推导式配合枚举（enumerate）的例子:

```
fruit = ['apple','organge','banana','mango','peach']

fruit_positon = {v:i for i,v in enumerate(fruit)}
print(fruit_positon)

Out: {'apple': 0, 'organge': 1, 'banana': 2, 'mango': 3, 'peach': 4}

```
还是用刚才的list，这次我们得到的key是fruit的每个元素，value则是该元素在fruit所在的index


### 集合推导式（Set comprehensions）

> 模板

让我们看先来看使用集合推导式的基础模板：

- ***{ expression for item in Sequence if conditional }***

其实集合推导式和list的推导式很像，但是既然是集合，肯定会配合利用Set的特有属性来实现我们的目的，如果你还对Set这种数据结构不够了解，可以参考我之前的文章：[Python 进阶之路 (四) 先立Flag, 社区最全的Set用法集锦][1]

下面来看最常见的应用

> 使用实例

首先，我们来看一个根据Set值唯一的特性的例子，我们有一个list叫names，用来存储名字，其中的数据很不规范，有大写，小写，还有重复的，我们想要去重并把名字的格式统一为首字母大写，实现方法便是用Set推导式：

```
names = [ 'Bob', 'JOHN', 'alice', 'bob', 'ALICE', 'James', 'Bob','JAMES','jAMeS' ]
names_standard = { n[0].upper()+n[1:].lower() for n in names}

print(names_standard)
Out: {'John', 'Bob', 'James', 'Alice'}

```
这里就不再举很多的其他例子了，因为使用的方式多种多样，剩下的就靠广大人民群众的智慧自行开发即可！


### 简单实战

现在让我们来看一个比较综合的例子！我们现在手里有一个英文字典的dictionary.txt文件，包含从A~Z的单词

具体需求：我们想要找到长度大于5的正反拼写都具有实际含义的单词

我们现在会通过各种推导式来实现这个目标，我会在文章最后把txt文件及Python文件下载链接附上，这样大家如果先要练习可以自行下载

首先，我们的初始目录结构如下：

![enter image description here](https://lh3.googleusercontent.com/7zEd9ERPMxXzNcgpgXfk-I2f6y4gLtKEksnWB7vTkjYrniVlBKyo2Kt37l5H6v0D5BkwfHxM_OQC)
这里我新建了一个test文件夹，把dictionary.txt 文件和python文件放在一起方便读取，开始之前，先大概看下txt文件长什么样子：


![enter image description here](https://lh3.googleusercontent.com/glYXJeEJfrmN572-cntgFHRhlgHlT7mMACScJMZofzJtGIHq8TXcZsSVX3pJI-X65czaqIvaXrvE)

#### **第一步：读取dictionary.txt中的单词，选出长度大于5的**

```

with open('dictionary.txt') as dictionary_file:
    words = (line.rstrip() for line in dictionary_file)
    words_over_five_letters = [w for w in words if len(w)>5 ]
    
```

这里通过列表推导式words_over_five_letters 用来存储所有长度大于5的单词

#### **第二步：将上一步选出的单词全部以倒序的方式存储在一个集合里**

```python
reversed_words ={
    word[::-1]
    for word in words_over_five_letters
    }
```
通过set推导式来实现

#### **第三步：通过 if 条件筛选得出结果**


```python
reversible_words = [
    word
    for word in words_over_five_letters
    if word in reversed_words
]

for word in reversible_words[0:20]:
    print(word)
    
    
Out：
    abrood
    agenes
    amaroid
    amunam
    animal
    animes
    bruted
    darter
    decart
    decurt
    deedeed
    deflow
    degami
    degener
    degged
    deified
    deifier
    deliver
    denier
```

这里最后共有203个结果，我们只看了前20个，验证方法就是只要长度大于5的单词同时存在于reversed_words和words_over_five_letters即可

完整代码如下：

```python

with open('dictionary.txt') as dictionary_file:
    words = (line.rstrip() for line in dictionary_file)
    words_over_five_letters = [w for w in words if len(w)>5 ]


reversed_words ={
    word[::-1]
    for word in words_over_five_letters
     }

reversible_words = [
    word
    for word in words_over_five_letters
    if word in reversed_words
]

for word in reversible_words[0:20]:
    print(word)
```


> **资料下载**

 - [dictionary.txt][2]
 - [dictionary.py][3]

## 总结 ##

这次为大家总结了python里面常见的三种推导式相关用法以及最后的小实战环节，希望大家喜欢，双击666点个赞吧！！


  [1]: https://segmentfault.com/a/1190000018109634?_ea=7068836
  [2]: http://yaozeliang.com/dictionary.txt
  [3]: http://yaozeliang.com/dictionary.py



