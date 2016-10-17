## 分支程序
在Python中，条件语句的形式是：
```{python}
if *Boolean expression*:
  # Block of codes
elif:
  # Block of codes
else:
  # Block of codes
```

例如下面的代码，当x除以2的余数是0时，表达式x%2==0的值是True，否则是False。其中，**==**是用来比较的操作符。
```{python}
if x%2 == 0:
    print('Even')
else:
    print('Odd')
print('Done with conditional')
```

我们看到，缩进在Python的语义中是有意义的，它代表该代码块隶属于上一层代码控制的范围。使用缩进来控制代码的结构在编程语言中并不常见，像C语言使用的是大括号{}来包裹代码。不要小瞧了缩进的使用，它是的代码的视觉结构和实际的程序语义结构是完全一致的！

### 条件语句的嵌套
条件语句的嵌套是指条件语句中的代码块包含另一个条件语句。例如下面的代码：
```{python}
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
```{python}
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

## 循环
循环是以一个检测开始，如果检测的结果为真（True，C语言中非零的话，程序即进入循环体，然后重新进行检测，直到检测结果为假（False，C语言中为0），循环结束，控制流进入下一段代码。看下面一段代码:
```{python}
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


