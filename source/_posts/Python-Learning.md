---
title: Python-Learning
date: 2023-01-10 17:44:16
categories: [Python]
tags: [Language learning]
---

# Python基础

## 输入和输出

输入：``input``

```python
name=input()
```

用input（）进行输入的时候输入的是一个字符串类型，要是想输入一个数字可以这么操作：

```python
age =input('your age=')
age1=(int)age
if age>18:
    print('adult')
else:
    print('not adult')
```

<!--more-->

python中可以像如下方式一样输入数据：

```python
>>> u, v, w = [int(x) for x in input().split()]
1 2 4
>>> print(u,v,w)
1 2 4
```

输出：``print``

```python
print("114514")
print('The quick brown fox', 'jumps over', 'the lazy dog')
The quick brown fox jumps over the lazy dog
```

在python中可以连续输入字符串，输出时，变成空格。

## 数据类型

python中无需定义数据类型，一切皆对象。

``**``表示方幂，同时也可以使用python的内置函数``pow(a,b,mod)``实现**快速幂**。

```python
3**4  #81
pow(2,3,5)  #3
```

python中没有double这种数据类型，只有float，对应着c/c++中的双精度数据类型。同时需要注意浮点数的运算不能直接判等，经典例子：

```python
0.1 + 0.2 == 0.3  #False
```

同时注意一下，python中**True**和**False**都是首字母大写。

python中字符串既允许使用单引号``''``也允许使用双引号``""``；

对于某些特定字符打印时我们需要用到转义字符，但同时有些时候我们就想让它直接显示结果，这时候我们可以用上``r''``来表示r内的字符不转义，例如：

```python
print(r'\\\t\\\')
```

输出结果为\\\\\\t\\\\\\；(~~md 怎么typora也有转义~~)

如果你想写多行字符串你可以写成：

``` python
print('''line1
line2
line3''')
```

### 字符串的格式化：

Python中格式化字符串有多种方法；

（1）与c语言相同，用%进行占位；

例如：

```python
print('%2d-%02d' % (3, 1))
print('%.2f' % 3.1415926)
```

同样，如果想输出中含有``%``，可以写成转义字符形式``%%``；

（2）format：

```python
'Hello {0}'.format('World')
```

（3）f-string：

```python
r=1
s=2.2342532
print(f'the num of r is={r}',f'the num of s is={s:.2f}')
```

输出结果为：the num of r is=1 the num of s is=2.23

就是用f写在字符串前面，然后遇见相应的变量替换掉。

空值：``None``；

### list 列表类型：

list内部可以放各种各样的数据类型，与数据结构中广义表的定义类似，非常的方便。

```python
classmates=['shino','xv','Goblin']
```

可以看作数组，len（）获取长度，可以用索引访问元素；

呃，也可以倒着取，比如``classmate[-1]``就输出Goblin，反正倒过来你也不能越界。

list是可以变长的数组，比较好，一些方法如下：

```python
list.append('123')#在末尾直接添加元素
list.insert(1,'123')#在相应的位置添加元素
list.pop()#删除末尾元素
list.pop(1)#删除指定位置的元素
```

list还可以容纳不同数据类型的元素；

```python
list=['shino',123,True,['Goblin',114]]
```

如果要检测某个list里面有没有某个元素，可以采取这样的方式：

```python
lst = [1,'2',33233,'114']
1 in lst  #True
```

但是不能检测是否存在子序列，例如：

```python
[1,'2'] in lst  #False
```

### tuple列表类型：

```python
classmates=('shino','xv','Goblin')
```

与list不同的是，tuple一旦确定就不能更改了

呃，但是有一点要注意：

```python
t=(1)
#此时表示的是t=1，因为这里把（）当小括号看了而不是tuple
t=(1,)
#这样就ok了 显示的是t=（1）
```

### 条件判断与循环

其实和c之类的差不多，但是要注意python的缩进规则，与c，java写法上有点不一样

```python
age=3
if age>18:
    print('adult')
    print('your age is',age)
else:
    print('child')
```

反正python就是按缩进来写语句的，自己看着办写（~~md，怎么判断条件里面连括号都不用写~~）

哦，不要忘记写冒号``:``

c里面的``else if``在python里面写成是``elif``

```python
if age>18:
    print('ohhhh')
elif age=24:
    print('aaaaaaa')
else:
    print('11111')
```

对于循环，python中提供两种方法，一种是for...in...循环方式：

```python
names=['xv','shino','Goblin']
for name in names:
    print(name)
```

python中提供一个特殊的函数叫做``range()``，可以用来提供从0-n-1的整数列，然后用list格式化为列表变量，例如我们可以用range函数实现0-100的加法：

```python
sum=0
for x in range(101):
    sum=sum+x
print(sum)
```

第二种是while循环：

```python
n=99
while n>0:
    print(n)
    n=n-2
```

然后，python里面也有和c，java一样的``continue``与``break``语句，就不再赘述。

### dict与set

python内置了字典（dict），采用键-值的搜索方式（kay-value），例如：

```python
d={'xv':10,'shino':20,'Goblin':30}
print(d['xv'])
```

此时此刻应该输出10；

运用字典查找速度较快，省去了list中从前向后遍历的时间，直接根据键-值来确定

除了这种方法，还可以直接添加数据：

```python
d['sjh']=10
```

由于key-value的一一对应性，所以如果对一个key放入多个value，后面的value会覆盖前面的value：

```python
d['xv']=10
d['xv']=20
print(d['xv'])
```

key不存在时，如果进行了访问，系统会报错，为了防止这种事情发生我们可以采用一下两种方法：

```python
'xv' in d
```

返回一个bool类型的值（True或者是False）

或者用dict自带的get方法：

```python
d.get('xv')
d.get('xv',-1)
```

如果不提供返回值将返回一个None，提供返回值便会返回你提供的数值（例如上面的-1）

删除元素用pop函数：

```python
d.pop('xv')
```

set也是一组key的集合，但是不存储value，set的key也不能重复

要创建set，必须提供一个list：

```python
s=set({1,2,3})
print(s)
```

输出结果为{1，2，3}

重复元素在set中会被过滤掉，例如：

``s=set([1,1,2,2,3,3])``实际上就是``s=set([1,2,3])``

增添，删减元素：

```python
s.add(4)
s.remove(3)
```

set可以看成数学上的集合，因此可以进行交并运算：

```python
s1=set([1,2,3])
s2=set([2,3,4])
s1 & s2
s1 | s2
```

分别就对应{2, 3}，{1, 2, 3, 4}

## 函数

### 自定义函数

格式如下：

```python
def my_abs(x):
    if x>0:
        return x
    else:
        return -x    
```

如果你想使用别的文件里的函数，可以这么操作：

```python
from abstest import my_abs
	#fileName 		#fuctionName
```

空函数：

```python
def nop():
    pass
```

pass一般就用于占位，等到什么时候这个函数需要被使用时在进行相应的改变。

返回多个值

```python
def returnTest(x,y,z):
    return x+y,y+z
```

可以发现python允许返回多个值，我们可以试着接受一下：

```python
a,b=returnTest(1,2,3)
print(a,b)
c=returnTest(1,2,3)
print(c)
```

输出结果：

3 5
(3, 5)

不难发现，实际上这个函数是return了一个tuple而不是返回了两个值。

### 函数参数

#### 位置参数

```python
def power(x,n):
    s=1
    while n>0:
        n=n-1
        s=s*x
    return s
```

这里的x，n就叫做位置变量

#### 默认参数

我们通常习惯计算n=2的情况，便可以把函数写成这样：

```python
def power(x,n=2):
    s=1
    while n>0:
        n=n-1
        s=s*x
    return s
print(power(2))
```

这里便默认n=2，就是计算平方项，如果要计算别的次方的话可以写成``power(2,3)``的形式

但是默认参数有个重要的一点就在于，python函数在被定义的时候，默认参数就被计算出来了，如果改变了这个默认参数的量，以后调用的话也会产生影响：

```python
def add_end(L=[]):
    L.append('END')
    return L
```

当你使用默认参数调用时，一开始是对的，但是多调用几次之后就不对了：

```python
print(add_end())
print(add_end())
print(add_end())
```

输出结果如下：

['END']
['END', 'END']
['END', 'END', 'END']

这是因为L原本设定的时候就是[]；你进行调用的时候向它指向的list中添加了'END'数据，为了避免这种事情发生，可以这么操作：

```python
def add_end(L=None):
    if L==None:
        L=[]
    L.append('END')
	return L
```

#### 可变参数

比如我们需要计算n个数字的平方和，可以这么写：

```python
def cal(numbers):
    sum=0
    for n in numbers:
        sum=sum+n*n
    return sum
```

此时就需要传入一个list或者tuple，就会写成：

```python
cal([1,2,3])或者是cal((1,2,3))
```

如果运用可变参数，就可以写成这样：

```python
cal(1,2,3)
```

怎么写呢，就这么写：

```python
def cal(*numbers):
	sum=0
    for n in numbers:
        sum=sum+n*n
    return sum    
```

这样使你传入的0-多个参数在函数内部自动组成一个tuple

#### 关键字参数

关键词参数允许你传入0-多个参数，在函数内部自动组成一个dict

```python
def person(name,age,**kw)
	print('name:',name,'age:',age,'other:',kw)
```

这样允许你在除了name和age参数之外传更多的参数，例如这样：

```python
extra={'city':'Peking','job':'worker'}
person('sjh',13,**extra)
```

## 高级特性

### 切片

切片可以用来截取一个list,tuple或者字符串。例如：

```python
L[:3]#截取L前三个元素
L[1:4]#截取下标1-4的元素
(1,2,3,4)[:3]
(1,2,3)
s = '12345'
s[::-1]  #反转整个字符串中的元素 输出：'54321'r
```

### 装饰器（Decorator）

什么是装饰器decorator呢，就是一种能在动态给函数添加功能的东西。装饰器是一种返回函数的函数，例如下面这个例子：

```python
def now():
    print("hello")
```

这里定义了一个``now()``函数，要是像在该函数上添加一些新功能怎么办呢，我们当然可以选择直接在函数里面直接写，但是python给我们提供了一些新的做法：

```python
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```

这里log就是一个装饰器，因为它是一个返回函数的函数，我们可以把这个装饰器放在``now()``函数的定义处，即这样：

```python
@log
def now():
    print("hello")
```

这样我们调用``now()``函数的时候，不仅调用其本身，而且你会得到这样的结果：

```python
>>>now()
#  call now():
#  hello
```



## OOP

### 封装继承多态

```python
class Student(object):
    def __init__(self,name,score):
        self.__name=name
        self.__score=score
    def print_score(self):
        print('%s: %s'%(self.__name,self.__score))
```

类后面的（）表示继承，类里面的函数第一个参数必须为self，表示类自身，构造方法\_\_init\_\_（**注意\_\_\_init_\_前后都有两个下划线！**）

如果要进行类内部数据的封装，可以在变量名前面加上**两个下划线__**

然后你可以提供set get方法

\_\_xv__这种变量不是私有变量，可以直接访问

```python
class pupil(Student):
    pass
```

这种就是继承，多态和java相似，**但是python可以多继承**



## 异常

格式：

```python
import logging

try:
    r=10/0
    print(r)
except ZeroDivisionError as e:
    logging.exception(e)
finally:
    print('finally')
print('END')
```

抛出异常：

```python
raise ...
```

## IO编程

### 文件读写

```python
f=open('/Users/123/text.txt','r')#打开文件
f.read()#一次性读取文件中全部内容
f.close()#读取完之后要关闭文件
f=open('Users/234/text.txt','w')
f.write('hello')#写文件，这样会直接覆盖之前的内容，不想之前的内容被覆盖可以在后面加一个参数'a'
f.close()
```



Python是一种非常神奇的语言，这里也仅仅列举了一些常见的知识，系统的学习可以转至廖雪峰的python教学网站：[Python教程 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1016959663602400)
