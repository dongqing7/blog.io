---
layout: post
title: Python分支与循环
date: 2016-10-15
categories: teaching
tags: [teaching, Programming-Using-Python, 编程, Python ]
description: 教授这门课不是义务，准备的本身就是报酬。
---

本章包含内容如下：

* 1、分支程序
* 2、循环
* 3、动手练习


## 1、 分支程序
在Python中，条件语句的形式是：

```python
if *Boolean expression*:
  # Block of codes
elif:
  # Block of codes
else:
  # Block of codes
```

例如下面的代码，当x除以2的余数是0时，表达式x%2==0的值是True，否则是False。其中，**==**是用来比较的操作符。

```python
if x%2 == 0:
    print('Even')
else:
    print('Odd')
print('Done with conditional')
```

我们看到，缩进在Python的语义中是有意义的，它代表该代码块隶属于上一层代码控制的范围。使用缩进来控制代码的结构在编程语言中并不常见，像C语言使用的是大括号{}来包裹代码。不要小瞧了缩进的使用，它是的代码的视觉结构和实际的程序语义结构是完全一致的！

### 条件语句的嵌套
条件语句的嵌套是指条件语句中的代码块包含另一个条件语句。例如下面的代码：

```python
if x%2 == 0:
    if x%3 == 0:
        print("Divisible by 2 and 3")
    else:
        print("Divisible by 2 and not by 3")
elif x%3 == 0:
    print('Divisible by 3 and not by 2')
```
上面的代码中，elif表示“else if”。

在条件语句中使用复合布尔表达式也是很方便的，例如：

```python
if x<2 and x>0:
    print('x is between 0 and 2')
elif x>2:
    print('x is bigger than 2')
else:
    print('x is smaller than 0')
```

思考一下：什么是：***常数时间***与***计算复杂度***

-----------------------------------------------

试试看：编写一段代码，处理三个变量x、y和z，然后输出它们当中最大的奇数。如果其中没有奇数，输出一条信息来说明。

-----------------------------------------------

## 2、循环
循环是以一个检测开始，如果检测的结果为真（True，C语言中非零的话，程序即进入循环体，然后重新进行检测，直到检测结果为假（False，C语言中为0），循环结束，控制流进入下一段代码。看下面一段代码:

```python
x = 3
ans = 0
left_item = x
while left_item != 0:
    ans = ans + x
    left_item = left_item - 1
print(str(x) + '*' + str(x) + '=' + str(ans))
```

下面的表格展示了每个变量在每次循环开始每个变量的值：

| 测试 # | x | ans | left_item |
|:---:|:---:|:---:|:---:|
| 1 | 3 | 0 | 3 |
| 2 | 3 | 3 | 2 |
| 3 | 3 | 6 | 1 |
| 4 | 3 | 9 | 0 |


## 3、动手练习
结合上面提到的分支与循环以及《Think in Python 2e》第二、五章内容，可以试试下面的练习：

### 3.1 穷举法

```Python
x = int(input('enter an int:'))
ans = 0
while ans**3 < abs(x):
    ans = ans + 1
if ans**3 != abs(x):
    print('%s is not a perfect cube' % (x))
else:
    if x < 0:
        ans = -ans
    print('Cube root of %s is %s' % (x, ans))
```
这段程序如何正确运行的呢：

- 表达式ans**3的值从0开始，每次循环都会变得更大
- 当它到达或者超过abs(x)时，循环终止
- 因为abs(x)总是正数，所以循环的次数必然是有限的

### 3.2 近似解与二分查找

#### 查找平方根的近似解

```python
x = 25
e = 0.01
step = e ** 2
num_Guesses = 0
while abs(ans**2 - x) >= e and ans <= x:
  ans += step
  num_Guesses += 1
print('num_Guesses = %s' % num_Guesses)
if abs(ans**2 -x) >= e:
  print('Failed on square root of %s' % x)
else:
  print('%s is close to square root of %s' % (ans, x))
```

#### 使用二分查找寻找近似平方根

```python
x = 25
e = 0.01
num_Guesses = 0
low = 0
high = max(1.0, x)
ans = (high + low) / 2.0
while abs(ans**2 - x) >= e:
  print('low = %s, high = %s, ans = %s' % (low, high, ans))
  num_Guesses += 1
  if ans**2 < x:
    low = ans
  else:
    high = ans
  ans = (high + low) / 2.0
print('num_Guesses = %s' % num_Guesses)
print('%s is close to square root of %s' % (ans, x))
```

#### 使用牛顿-拉夫逊方法寻找平方根

```python
# 寻找x， 满足x**2 - 24在e 和 0 之间
e = 0.01
k = 24.0
guess = k/2.0
while abs(guess*guess - k) >= e:
  guess = guess - (((guess**2) - k)/(2*guess))
print('square root of %s is about %s' % (k, guess))
```
-----------------------------------------------

试试看：给牛顿-拉夫逊方法的具体实现添加一些代码，跟踪寻找解的循环次数。使用这个次数来比较牛顿-拉夫逊方法和二分查找方法的效率，哪个会**更高**呢？

-----------------------------------------------

-------------------
以上实例codes均来自《编程导论》
