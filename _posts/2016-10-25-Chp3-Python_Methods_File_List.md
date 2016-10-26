---
layout: post
title: 3-Python函数使用方法与常见数据结构
date: 2016-10-26
categories: Introducing Python
tags: [teaching, Python, Methods, List, Tuple, Dictionary ]
description: 教授这门课不是义务，准备的本身就是报酬。
---

本章包含内容如下：

* 1、函数、作用域和规范抽象
* 2、列表、元组和字典

## 1. 函数、作用域和规范抽象

```python
# 通过二分法求数值25的近似平方根
x = 25
epsilon = 0.01
numGuesses = 0
low = 0.0
high = max(1.0,x)
ans = (high + low)/2.0
while abs(ans**2 - x) >= epsilon:
    numGuesses += 1
    if ans**2 < x:
        low = ans
    else:
        high = ans
    ans = (high + low)/2.0
    print 'numGuess = ',numGuess
    print ans,'is close to square root of'.x
```

这段代码只能处理变量x和epsilon所指定的值，并不具备通用性。如果想要重用它，需要复制整段代码，修改变量名，然后粘贴到需要使用的地方。因此很难在其他更复杂的应用中**重复使用**这段代码。

如果我们要计算立方根而不是平方根，那么需要修改代码。如果想要编写一个既可以计算平方根又可以计算立方根的代码，那么程序会包含很多段完全相同的代码。一个程序包含的代码越多，就越有可能出错，也就越难维护。

高级编程语言的机制都提供了很多特性，能使得代码更具有通用性和可重用性，其中最常见的就是函数。

### 1.1 函数和作用域

#### 函数定义

Python中，函数的定义形式：

```python
def name of function(liat of formal parameters):
    
    body of function
```

例如，

```python
def max(x,y):
    if x > y:
        return x
    else:
        return y
```

def关键字告诉Python即将定义一个函数。

函数名（max）用来指代函数。

函数名后括号中的序列(x,y)是函数的形参。

调用函数时，实参（通常指代函数调用时的参数）被绑定（就像赋值语句一样）到形参。

例如：`max（3,4）`

##### 1.2) 关键字参数和默认值

```python
def printName(firstName, lastName, reverse):
    if reverse:
        print lastName + ',' + firsrName
    else:
        print firstName,lastName
```

关键字参数：形参根据名称绑定到实参。上面例子中reverse的默认值是True.

在定义函数时，可以设定参数的默认值，例如

```python
def printName(firstName, lastName, reverse = False):
    if reverse:
        print lastName + ',' + firstName
    else:
        print firstName,lastName
```
如果使用默认值，程序员在调用函数是就可以不传入所有参数。

### 1.1.3 作用域

```python
def f(x):
    y = 1
    x = x + y
    print 'x =',x
    return x
	
x = 3
y = 2
z = f(x)
print 'z = ',z
print 'x = ',x
print 'y = ',y
```

运行后会输出：

x = 4
z = 4
x = 3
y = 2

调用 f 时，形参x在局部被绑定到了实参x的值。需要注意的是虽然形参和实参的名称相同，但是他们并不是同一个变量。每个函数都会定义一个新的命名空间，也被称为作用域。f 中的形参x和局部变量y只存在于f 定义的作用域中。函数体中的赋值语句x = x + y将局部名称x绑定到对象4，但是对于f 作用域外的名称x和y的绑定没有任何影响。

**思考一下**：如果关于求平凡根的需求变了呢，比如我想计算数值8219的近似平方根，同时精度要达0.00005；或者计算（23、96、137、337、963..到10333的平凡根，而且，数值越大，所需要的精度曾递减，比如从0.01、0.001、0.00001、0.000003等，那你如果更新上面的代码以满足此类需求？

-----------------------------------------------
试试看：将上述需求，通过函数进行包装，配合调用方的代码，就可以轻松完成以上任务，例如可以这样写：
```python
def cal_approximate_root(tvalue, epsilon=0.001):
    numGuesses = 0
    low = 0.0
    high = max(1.0,x)
    ans = (high + low)/2.0
    # ....
    # code block
    # ....
    return ans
```
-----------------------------------------------
如果我的需求又变了呢，比如我想在不同的条件下求平方根或立方根的近似值，如果更改以上代码，一起满足需求？

### 1.2 全局变量

```python
NUM_FIB_CALLS
def fib(x):
    """假定x是正整数
    返回第x个斐波那契数"""
    global NUM_FIB_CALLS
    NUM_FIB_CALLS += 1
    if x == 0 or x == 1:
        return 1
    else:
        return fib(x-1) + fib(x-2)
		
def test_fib(n):
    for i in range(n+1):
        global NUM_FIB_CALLS
        NUM_FIB_CALLS = 0
        print 'fib of', i, '=',fib(i)
        print 'fib called',NUM_FIB_CALLS,'time.'
```

在这两个函数中，**global NUM_FIB_CALLS** 这行代码告诉Python这个名称应该被定义在模块的最外层的作用域中，而不是在本行代码坐在的函数作用域中定义——尽管在fib和test_fib 函数中**NUM_FIB_CALLS**都出现在赋值语句左侧。（我们并没有使用语句global NUM_FIB_CALLS,所以NUM_FIB_CALLS在fib和test_fib中都是局部变量。）函数fib和test_fib都可以随意访问变量NUM_FIB_CALLS指向的对象。函数test_fib每次调用fib时都会将NUM_FIB_CALLS绑定到0，fib每次被执行时都会将NUM_FIB_CALLS的值加fib。

### 1.3 模块

一个模块是一个包含Python定义和语句的.py文件。例如，创建一个包含下面内容的.py文件circle.py。

```python
pi  = 3.14159
def area(radius):
    return pi * (radius ** 2)
	
def circumference(radius):
    return 2 *pi*radius
	
def sphereSurface(radius):
    return 4.0*area(radius)
    
if __name__ == '__main__':
    print(area(3))
```

程序可以通过import语句来访问一个模块。

```python
import circle
print circle.pi
print circle.area(3)
print circle.circumference(3)
print circle.sphereSurface(3)
```

输出结果为：

3.14159
28.27431
18.84954
113.09724

模块一般单独储存在文件中，每个模块有自己的私有符号表。

Python标准库中有很多有用的模块。

##  2文件的读写与操作

### 2.1 打开文件

`fileobj = open(filename,mode)`

1、fileobj是open（）返回的文件对象

2、filename是该文件的字符串名

3、mode是指文件类型和操作的字符串。

mode的第一个字母表示对其的操作：

1、r表示读模式

2、w表示写模式。如果文件不存在则新创建，如果文件存在则重写新内容。

3、x表示在文件不存在的情况下新创建并写文件

4、a表示如果文件存在，在文件末尾追加写内容。

mode的第二个字母是文件类型：

1、t(或者省略)代表文本类型

2、b代表二进制文件

### 2.2 写文本文件

```python
f.write(s)将字符串s写到文件结尾
f.writelines(s) s是一个字符串序列，这条语句会将s的每一个元素都写入文件
```

### 2.3 读文本文件

```python
f.read()返回一个包含文件内容的字符串
f.readlines()返回一个列表，其中的一个元素就是文件的一行
f.close()关闭文件
```

## 3 列表、元组和字典

### 3.1 列表

创建列表

```python
1 empty_list = []
2 empty_list = list()
```

使用list()将其他数据类型转换成列表

```python
list('cat') ————> ['c', 'a', 't']

a_tuple = ('ready','fire','aim')    #元组转化为列表
list(a_tuple) ————> ['ready', 'fire', 'aim']
```

列表操作

```python
list.append(e) #将对象e添加到list的结尾
list.account(e) #返回list中e出现的次数
list.insert(i,e) #将对象e插入到list中下标为i的地方
list.extend(list1) #将list1中的元素添加到list的结尾
list.remove(e) #从list中删除第一次出现的e
list.index(e) #返回list中第一次出现e的下标。
list.pop(i) #删除并返回下标为i的元素。如果i值被省略，默认值是-1
list.sort() #有副作用的排序L中的元素
list.reverse() #有副作用的翻转列表list
```

### 3.2  元组

创建元组

```python
empty_tuple = ()
max_tuple = ('Groucho','Chico','Harpo')
```

tuple() 函数可以用其他类型的数据来创建元组

```python
max_list = ['Groucho','Chico','Harpo']
tuple(max_list)
```

### 3.3 字典

创建字典

```python
empty_dict = {}
```

使用dict{}转换为字典

```python
lol = [['a','b'],['c','d'],['e','f']]
dict(lol) ————> {'a': 'b', 'c': 'd', 'e': 'f'}
```

常用的字典操作

```python
len(d) #返回d中项的数量
d.keys() #返回一个列表，包含d中的键
d.values() #返回一个列表，包含d中的值
k in d #如果键k在d中，返回True
d[k] #返回d中键为k的项
d.get(k,v) #如果k在d中返回d[k]，否则返回v
d[k] = v #将值v和键k关联起来。如果键k已经关联到其他值，使用新值代替k
del d[k] #从d中删除键k
for k in d #遍历d中的键
```