## 有关字典的更多用法 ##
大家好，上一期为大家梳理了一些List的进阶用法，今天我们来看字典Dict的相关技巧，我个人在编程中对字典的使用非常频繁，其实对于不是非常大的数据存储需求，字典是一个不错的选择，比List要快的多，我在基础篇里面讲过了一些关于dict的基础方法，如果没有看过的朋友们可以点击链接[Python 基础起步 (八) 字典实用技巧大全][1] ，好啦，闲话少说，现在让我们一起来看看今天的进阶技巧吧~

## 字典进阶方法汇总 ##

### 创建字典

这里介绍最常见的几种方式,直接上例子:

```
first = {}           # 创建空字典
second = dict()      # 创建空字典

keys = ['Name','Age','Job','Salary']
values = ['White',50,'cook',10000]
third=dict(zip(keys,values))    # Zip创建

fouth = dict(Name='White',Age=50,Job='cook',Salary=10000)  # 等号创建

fifth = {1: {'name': 'John', 'age': '27', 'sex': 'Male'},
         2: {'name': 'Marie', 'age': '22', 'sex': 'Female'}}    # 创建一个嵌套字典

print(first)
print(second)
print(third)
print(fouth)
print(fifth[1])


Out: {}
     {}
     {'Name': 'White', 'Age': 50, 'Job': 'cook', 'Salary': 10000}
     {'Name': 'White', 'Age': 50, 'Job': 'cook', 'Salary': 10000}
     {'name': 'John', 'age': '27', 'sex': 'Male'}

```

这里我们可以直接用{}或者dict()创建空的字典，或者直接为字典以key:value的形式赋值，Zip和等号直接赋值也很方便，如果需要多层nested也可以很简单的实现，有关创建就说这么多啦

### 字典排序

有关字典排序，我们有两种选择，第一是根据字典的key值排序，第二是根据Value值排序，让我们一个个来看，首先让我们新建一个字典用于测试：
```
final_result= dict(Math=80,Chinese=78,English=96,Science=60,Art=75)
print(final_result.items())

Out: dict_items([('Math', 80), ('Chinese', 78), ('English', 96), ('Science', 60), ('Art', 75)])

```
#### **根据Key值排序** ####
这里我们创建一个字典final_result,key值是科目的名字，value值是分数，首先根据Key值进行排序，首先让我们根据Key值升序，可选的方法很多，比如sorted, operator, lamba ：

```
print(sorted(final_result.items())) # 根据key的值升序
Out:[('Art', 75), ('Chinese', 78), ('English', 96), ('Math', 80), ('Science', 60)]

```

```
import operator
print(sorted(final_result.items(),key=operator.itemgetter(0)))
Out:[('Art', 75), ('Chinese', 78), ('English', 96), ('Math', 80), ('Science', 60)]

```

```
print(sorted(final_result.items(),key=lambda x:x[0]))
Out:[('Art', 75), ('Chinese', 78), ('English', 96), ('Math', 80), ('Science', 60)]
```
根据key值降序只要加个reverse=True就好了，因为sorted函数默认reverse=False，看下结果：

```
print(sorted(final_result.items(),reverse=True))   # 根据key的值降序
Out：[('Science', 60), ('Math', 80), ('English', 96), ('Chinese', 78), ('Art', 75)]

```
```
import operator
print(sorted(final_result.items(),key=operator.itemgetter(0),reverse=True))
Out:[('Science', 60), ('Math', 80), ('English', 96), ('Chinese', 78), ('Art', 75)]


```

```
print(sorted(final_result.items(),key=lambda x:x[0],reverse=True))
Out:[('Science', 60), ('Math', 80), ('English', 96), ('Chinese', 78), ('Art', 75)]

```
有关lamba函数实在有太多可以总结的，我会在之后专门拿一期来讲，和filter reduce简直是神器，当我逐渐使用的多了后终于感受到了一点点pythonic的感觉，哈哈


### **根据Value值排序** ####

其实大家看到了根据key的排序，也猜到了如何根据value 排序，让我们先看升序：

```
print(sorted(final_result.items(),key=lambda x:x[1])) #根据Value升序
Out：[('Science', 60), ('Art', 75), ('Chinese', 78), ('Math', 80), ('English', 96)]

```

```
import operator
print(sorted(final_result.items(),key=operator.itemgetter(1)))

Out：[('Science', 60), ('Art', 75), ('Chinese', 78), ('Math', 80), ('English', 96)]

```
降序也一样，无非就是加上reverse=True，这里不一一举例了:

```
print(sorted(final_result.items(),key=lambda v:v[1],reverse=True))
Out：[('English', 96), ('Math', 80), ('Chinese', 78), ('Art', 75), ('Science', 60)]
```

### 字典合并(Merge)

在Python 3.5以上可以直接用**，是一个常用的小技巧，在此对于2.7的用户说一声对不起，技术一直说是喜新厌旧呀，让我们看一个小栗子：

```
def Merge(dict1, dict2):
    res = {**dict1, **dict2}
    return res

dict1 = {'a': 10, 'b': 8,'c':2}
dict2 = {'d': 6, 'c': 4}
dict3 = Merge(dict1, dict2)
print(dict3)

Out：{'a': 10, 'b': 8, 'c': 4, 'd': 6}


```
这里顺序很重要，大家一定要看好是谁覆盖了谁，如果我们交换一下顺序就会变成这样：

```
def Merge(dict1, dict2):
    res = {**dict2, **dict1} #  交换了顺序
    return res

dict1 = {'a': 10, 'b': 8,'c':2}
dict2 = {'d': 6, 'c': 4}
dict3 = Merge(dict1, dict2)
print(dict3)
Out:{'d': 6, 'c': 2, 'a': 10, 'b': 8}

```

对于Python2的朋友们不用担心，自然有解决方案，那就是用update函数，也很方便，上代码：

```
dict1 = {'a': 10, 'b': 8,'c':2}
dict2 = {'d': 6, 'c': 4}

dict2.update(dict1)
print(dict2)

Out：{'d': 6, 'c': 2, 'a': 10, 'b': 8}

```
### 利用Json.dumps()美化输出dict

我们如果碰到以下这种情况的dict，如果按照常规print输出会这样：

```
my_mapping = {'a': 23, 'b': 42, 'c': 0xc0ffee}
print(my_mapping)
Out:{'a': 23, 'b': 42, 'c': 12648430}

```
但是如果我们能引用json库里的dumps方法会得到好的多的效果：

```
import json
print(json.dumps(my_mapping, indent=4, sort_keys=True))
Out:{
    "a": 23,
    "b": 42,
    "c": 12648430
    }

```

### 字典参数解包

Python里面方便神奇的方法很多，比如下面这个，可以实现解包字典：

```
def unpack(k1,k2,k3):
    print(k1,k2,k3)

my_dict = {'k1':'value1','k2':'value2','k3':'value3'}
unpack(**my_dict)

Out: value1 value2 value3

```
顺便提一下哈，有关 args和kwargs的方法我会专门在后面的一期讲，敬请期待！

### 字典推导式

这个我写的比较纠结，因为咨询了我的主管，他推荐我尽量不要用，我也不太懂其中原因，不知道有没有大神可以出来解答一下哈，具体用法和List的推导式一样，上代码：

```
import json
first = {x:'A'+str(x) for x in range(8)}
print(json.dumps(first,indent=4, sort_keys=True))     # 这种情况用json输出好看些
 
Out：{             
    "0": "A0",
    "1": "A1",
    "2": "A2",
    "3": "A3",
    "4": "A4",
    "5": "A5",
    "6": "A6",
    "7": "A7"
     }
```
或者可以这么用：
```
second={v:k for k,v in first.items()}
print(json.dumps(second,indent=4))
Out：{
    "A0": 0,
    "A1": 1,
    "A2": 2,
    "A3": 3,
    "A4": 4,
    "A5": 5,
    "A6": 6,
    "A7": 7
     }

```
至于其他乱七八糟的用法大家可以自己去想哈哈

## 总结 ##

今天系统地为大家梳理了几点：

 - 创建字典不同方法
 - 字典排序
 - 字典合并
 - 字典解包
 - json优化输出
 - 字典推导式

希望可以帮到大家，后续如果我发想有什么有意思的方法和技巧我会加上，再次祝大家新年快乐！！！！
  [1]: https://segmentfault.com/a/1190000018050011
