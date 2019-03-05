## 神奇的collections ##

大家好，今天想和大家分享一个Python里面非常棒的模快：***Collections***

该模块实现了专门的容器数据类型，为Python的通用内置容器提供了替代方案，如果对源码感兴趣的朋友们可以在 ***Lib/collections/__init__.py*** 路径下找到

基于我目前的学习经验，以下几种类型用的很多：

 - **defaultdict**    (dict子类调用工厂函数来提供缺失值)
 - **counter**        (用于计算可哈希对象的dict子类)
 - **deque**          (类似于列表的容器，可以从两端操作)
 - **namedtuple**     (用于创建具有命名字段的tuple子类的工厂函数)
 - **OrderedDict**    (记录输入顺序的dict)

好啦，看到什么工厂函数，可哈希对象，容器这些词汇不要慌，我第一次看是懵逼并直接跳过的，然而后来发现根本不需要理解，如果大家感兴趣可以自己去查询，这里还是老样子，通过大量实例来一个个讲解！

### defaultdict 

> **基础概念**

“defaultdict”是在名为“collections”的模块中定义的容器。它需要一个函数（默认工厂）作为其参数。默认情况下设置为“int”，即0.如果键不存在则为defaultdict，并返回并显示默认值。

我用人话解释一下: **其实就是一个查不到key值时不会报错的dict**

> **应用实例**

首先我们来看一个用正常dict的例子，如果我们创建了一个叫person的字典，里面存储的key值为name，age，如果这时候尝试调用person['city']，会抛出KeyError错误，因为没有city这个键值：

```
person = {'name':'xiaobai','age':18}
print ("The value of key  'name' is : ",person['name'])
print ("The value of key  'city' is : ",person['city'])

Out: The value of key  'name' is :  xiaobai
Traceback (most recent call last):
  File "C:\Users\E560\Desktop\test.py", line 17, in <module>
    print ("The value of key  'city' is : ",person['city'])
KeyError: 'city'

```
现在如果我们用defaultdict再试试呢？

```
from collections import defaultdict
person = defaultdict(lambda : 'Key Not found') # 初始默认所有key对应的value均为‘Key Not Found’

person['name'] = 'xiaobai'
person['age'] = 18

print ("The value of key  'name' is : ",person['name'])
print ("The value of key  'adress' is : ",person['city'])

Out：The value of key  'name' is :  xiaobai
     The value of key  'adress' is :  Key Not found
```


大家可以发现，这次没有问题了，其实最根本的原因在于当我们创建defaultdict时，首先传递的参数是所有key的默认value值，之后我们添加name，age进去的时候才会有所改变，当我们最终查询时，如果key存在，那就输出对应的value值，如果不存在，就会输出我们事先规定好的值‘Key Not Found’

除此之外外，我们还可以利用defaultdict创建时，传递参数为所有key默认value值这一特性，实现一些其他的功能,比如：

```
from collections import defaultdict
d = defaultdict(list)
d['person'].append("xiaobai")
d['city'].append("paris")
d['person'].append("student")

for i in d.items():
    print(i)

Out: ('person', ['xiaobai', 'student'])
     ('city', ['paris'])

```
一个道理，我们默认所有key对应的是一个list，自然就可以在赋值时使用list的append方法了。再比如下面这个例子：

```
from collections import defaultdict
food = (
    ('jack', 'milk'),
    ('Ann', 'fruits'),
    ('Arham', 'ham'),
    ('Ann', 'soda'),
    ('jack', 'dumplings'),
    ('Ahmed', 'fried chicken'),
)

favourite_food = defaultdict(list)

for n, f in food:
    favourite_food[n].append(f)

print(favourite_food)

Out：defaultdict(<class 'list'>, {'jack': ['milk', 'dumplings'], 'Ann': ['fruits', 'soda'], 'Arham': ['ham'], 'Ahmed': ['fried chicken']})
```
道理和上面差不多，这里大家可以自己拓展，展开想象，相信可能在某个时刻可以用的上defaultdict这个容器

### counter 
> **基础概念**

Counter是dict的子类。因此，它是一个无序集合，其中元素及其各自的计数存储为字典。这相当于其他语言的bag或multiset。

我的理解就是一个计数器，**返回一个字典，key就是出现的元素，value就是该元素出现的次数**
> **应用实例**

计数器没啥可说的，还能干啥，计数呗！

```
from collections import Counter

count_list = Counter(['B','B','A','B','C','A','B','B','A','C'])  #计数list
print (count_list)


count_tuple = Counter((2,2,2,3,1,3,1,1,1))  #计数tuple
print(count_tuple)

Out：Counter({'B': 5, 'A': 3, 'C': 2})
     Counter({1: 4, 2: 3, 3: 2})
```

**Counter一般不会用于dict和set的计数，因为dict的key是唯一的，而set本身就不能有重复元素**

现在我们也可以直接把在defaultdict例子中用过food元组拿来计数：

```
from collections import Counter
food = (
    ('jack', 'milk'),
    ('Ann', 'fruits'),
    ('Arham', 'ham'),
    ('Ann', 'soda'),
    ('jack', 'dumplings'),
    ('Ahmed', 'fried chicken'),
)

favourite_food_count = Counter(n for n,f in food)  #统计name出现的次数
print(favourite_food_count)

Out: Counter({'jack': 2, 'Ann': 2, 'Arham': 1, 'Ahmed': 1})

```
### deque ###

> **基础概念**

在我们需要在容器两端的更快的添加和移除元素的情况下，可以使用deque.
我的个人理解是deque就是一个可以两头操作的容器，类似list但比列表速度更快

> **应用实例**

deque的方法有很多，很多操作和list类似，也支持切片

```
from collections import deque
d = deque()
d.append(1)
d.append(2)
d.append(3)

print(len(d))
print(d[0])
print(d[-1])

Out: 3
     1
     3
```
deque最大的特点在于我们可以从两端操作：

```
d = deque([i for i in range(5)])
print(len(d))
# Output: 5

d.popleft()   # 删除并返回最左端的元素
# Output: 0

d.pop()       # 删除并返回最右端的元素
# Output: 4

print(d)
# Output: deque([1, 2, 3])

d.append(100)  # 从最右端添加元素

d.appendleft(-100) # 从最左端添加元素

print(d)
# Output: deque([-100, 1, 2, 3, 100])

```

除了这些deque的方法实在太多了，比如我再举几个常用的例子，首先我们定义一个deque时可以规定它的最大长度，deque和list一样也支持extend方法，方便列表拼接，但是deque提供双向操作：

```
from collections import deque
d = deque([1,2,3,4,5], maxlen=9)  #设置总长度不变
d.extendleft([0])  # 从左端添加一个list
d.extend([6,7,8])   # 从右端拓展一个list
print(d)

Out：deque([0, 1, 2, 3, 4, 5, 6, 7, 8], maxlen=9)

```
现在d已经有9个元素了，而我们规定的maxlen=9，这个时候如果我们从左边添加元素，会自动移除最右边的元素，反之也是一样：

```
d.append(100)
print(d)
d.appendleft(-100)
print(d)

Out: deque([1, 2, 3, 4, 5, 6, 7, 8, 100], maxlen=9)
     deque([-100, 1, 2, 3, 4, 5, 6, 7, 8], maxlen=9)
```
deque还有很多其他的用法，大家根据各自的需要去自己寻宝吧！
  


### namedtuple ###

> **基础概念**

名称元组。大家一看名字就会感觉和tuple元组有关，没错，我认为它是元组的强化版
namedtuple可以将元组转换为方便的容器。使用namedtuple，我们不必使用整数索引来访问元组的成员。

我觉得可以把namedtuple 视为 ***不可变的*** 字典

> **应用****实例**

首先，让我们先回顾一下普通元组是如何访问成员的：

```
person = ('xiaobai', 18)
print(person[0])

out：xiaobai
```
现在我们看看namedtuple（名称元组）的强大之处：

```
from collections import namedtuple

Person = namedtuple('Person', 'name age city')        # 类似于定义class
xiaobai = Person(name="xiaobai", age=18, city="paris") # 类似于新建对象
print(xiaobai)

Out：Person(name='xiaobai', age=18, city='paris')

```
我们创建namedtuple时非常像定义一个class，这里Person好比是类名，第二个参数就是namedtuple的值的名字了，我感觉很像class里的属性，不过这里不用加逗号分离，下面让我们看看如何访问namedtuple的成员：

```
print(xiaobai.name)
print(xiaobai.age)
print(xiaobai.city)

out：xiaobai
     18
     paris
```

***"爽啊，爽死了"，郭德纲看到这里不禁赞叹***

这种无限接近class调用属性的方式还是非常不错的，在一些实际场景很有用。
最后还有一点千万不要忘了，我们不能修改namedtuple里的值：

```
xiaobai.name = 'laobai'
Out：Traceback (most recent call last):
  File "C:\Users\E560\Desktop\test.py", line 5, in <module>
    xiaobai.name = 'laobai'
AttributeError: can't set attribute

```

### OrderedDict ###

> **基础概念**

“OrderedDict” 本身就是一个dict，但是它的特别之处在于会记录插入dict的key和value的顺序


> **应用实例**

```
from collections import OrderedDict
d = {}
d['a'] = 1
d['b'] = 2
d['c'] = 3
d['d'] = 4
print(d)

Out:{'a': 1, 'c': 3, 'b': 2, 'd': 4}

```
大家可以看到，这是一个普通的dict，因为无序，即使我们依次添加了a,b,c,d 四个键并赋予value，但是输出的顺序并不可控。OrderedDict的出现就是为了解决这个问题：

```
from collections import OrderedDict
d = OrderedDict()
d['a'] = 1
d['b'] = 2
d['c'] = 3
d['d'] = 4
print(d)

Out：OrderedDict([('a', 1), ('b', 2), ('c', 3), ('d', 4)])

```
这回输出时好多了，因为会自动记录插入的顺序，同理，如果我们删除一个key, OrderedDict的顺序不会发生变化：

```
from collections import OrderedDict

print("Before deleting:\n")
od = OrderedDict()
od['a'] = 1
od['b'] = 2
od['c'] = 3
od['d'] = 4

for key, value in od.items():
    print(key, value)

print("\nAfter deleting:\n")
od.pop('c')
for key, value in od.items():
    print(key, value)

print("\nAfter re-inserting:\n")
od['c'] = 3
for key, value in od.items():
    print(key, value) 
    

Out：Before deleting:

    ('a', 1)
    ('b', 2)
    ('c', 3)
    ('d', 4)
    
    After deleting:
    
    ('a', 1)
    ('b', 2)
    ('d', 4)
    
    After re-inserting:
    
    ('a', 1)
    ('b', 2)
    ('d', 4)
    ('c', 3)
```


## 总结 ##

今天为大家简单介绍了collections的一些基础容器类型，包括：
 - **defaultdict**   不会报错的dict
 - **counter**       计数器
 - **deque**         双向操作list
 - **namedtuple**    名称元组


我觉得把它们叫做宝藏感觉还是不过分的，因为这些容器在真实使用场景中非常有用，而且我发现很多教程不会提到，因此衷心希望可以帮到大家，如果我哪里介绍有错误或者遗漏，希望大家留言指出，让我们一起进步！




