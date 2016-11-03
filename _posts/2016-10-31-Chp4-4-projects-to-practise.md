---
layout: post
title: 通过几个小项目练习使用Python
date: 2016-10-31
categories: Introducing Python
tags: [teaching, Python, practice, project ]
description: 教授这门课不是义务，准备的本身就是报酬。
---


4个小项目如下：

* 1、模拟掷骰子
* 2、猜数字
* 3、疯狂的故事大王（Mad Libs Generator）
* 4、大冒险（文字版）


说明，练习项目的信息来源于：http://knightlab.northwestern.edu/2014/06/05/five-mini-programming-projects-for-the-python-beginner/

子曰：学而时习之，不亦说乎！
如要真正掌握一门编程语言，唯有通过大量的练习和不断的思维才能实现。在**knight lab**发现的这个5个小小项目，正好可以让大家把已学到的知识得以应用。这里只涉及了前4个，个人觉得已基本够用，手快的同学可以移步原始网站去完成最后一个。

## 项目1. 模拟掷骰子
目标和需求：用程序模拟骰子游戏，当程序启动时，它将会自动从指定的1-6个数字中随机选择一个数字返回，然后输出，用户可以继续投骰子，直到用户输入退出（exit）的命令，结束程序。
涉及的概念：

- 随机 （import random）
- 整数（int）
- 打印（print）
- 判断（if）
- 循环 （while）
- 处理用户输入 （input）

这可能是最简单不过的练习了，程序虽小，但也五脏俱全了。试试看吧！（括号里面是Python关键字，用来实现需求的所需要用到的技术）

下手前是不是茫然无措？那就是了，学再多的理论也不可能一下就学会骑车。然后，编程还是有捷径可以走的，一个是**逻辑草稿**，另一个就是自己的**代码库**。目前为止，相信才入门的同学肯定还没有开始积累自己的代码库，那就从逻辑草稿开始吧！
思考上面的问题，可以拿着笔在草稿纸上写下你理解的内容，括号（）里是可能要思考的内容：

- 自动是个什么鬼。。。？
- 1-6个数字选择其中一个（是要把每个数字记录在不同的变量里么？还是有更简单些的方法？）
- 随机选择（这要如何实现？我要到哪儿去找可以实现的代码？）
- 输出（大概就是print吧）
- 用户？
- 如何让用户告诉程序，再来一遍投骰子的过程

把要做的都写下来的时候，再整理一遍思路，这里有一个**要点**，在拿到需求后，可以先不用考虑性能，即先以自己最熟悉的方法去实现它，等所有功能安装既定目标达成之后 ，如果这时发现性能（时间或空间）需要优化，再开始优化。比如，上面的思路整理之后应该能转变为下面的逻辑：

- 0，程序开始运行后，自动进行下面的代码（也就是说不等待用户输入）
- 1，随机从1-6个数字中选中一个
- 2，打印选择的数字
- 3，等待用户输入，如果输入继续或c，则运行1；如果输入结束或e，则运行下一步
- 4，结束程序

嗯，差不多就是这样，因为不熟练，所以需要纸笔辅助思考，再熟练以后，可以直接用脑思维完成上述过程，因为Python的语法特点和代码结构本就很直观，所以可以很容易的写出代码，比如说下面的代码：

```python

import random

flag = True
while flag == True:
    chosen_digit = random.randrange(1, 6)
    print('The num %s was randomly chosen!' % (chosen_digit))
    
    flag_continue = ''
    while flag_continue != 'c' and flag_continue != 'e':
        print('If continue, please input \'c\', or \'e\' to exit the program')
        flag_continue = input()
         
    if flag_continue == 'c':
        flag = True
    elif flag_continue == 'e':
        flag = False
        
print('The programming is closing...')
```

嗯，我的经验是，大凡你一开始觉得比较容易的实现，在真正进入调试程序的时候才发现，实现难度往往要比你想的要大一些，这一点尤其在你从未实现过的功能上体现的很显著。比如上面的代码中，用来实现检验用户输入的部分，因为很少做这样的事情，所以在现实中走了一些弯路。
无论如何，在顺利完成一个任务后，回顾一下刚实现的代码，是不是觉的有点成就感啊！还记得之前说的两个**要点**么——**代码库**，在上面的这段代码中，random.randrange()以及检验用户输入是否和期望相符的代码就是我们代码库里要安置的内容，我们并不用去刻意记忆具体实现的方法，我们只要知道，我知道我能干什么，并且具体怎么做（代码库！）。这样，随着自己的经验积累，若干年后，你一定会产生很大的自信。趁热打铁，看看下面的项目如何快速实现吧！


## 项目2. 猜数字
目标和需求：与上个项目相似，该项目也需要用到Python里的随机模块（random module）。程序首先自动选中一个数字，然后由用户去猜。如果没猜中，程序需要进行适当的提示，以帮助用户进行下一次的猜测（比如，太大了，或太小了等等），如果猜中了，则输出成功信息。需要注意的是，需要判断用户输入的是否为整数（int）。

涉及的概念：

- 随机（imprt random）
- 变量 （variable）
- 输入和打印（input，print）
- 判断（if）
- 循环 （while）

```python
import random

count = 6
chosen_digit = random.randrange(1, 100)
print('Guess what I have? Your have %s times' % count)

while count > 0:
    guess_num = int(input())
    if guess_num == chosen_digit:
        print('Great! You get it at %s guess!' % count)
        break
    elif guess_num > chosen_digit:
        print('No, too big! %s times left, my dear!' % (count))
    elif guess_num < chosen_digit:
        print('No, too small! %s times left, my dear!' % (count))
    
    count -= 1
    
print('What a pity, you have missed the %s million $' % chosen_digit)
```

## 项目3. 疯狂的故事大王（Mad Libs Generator）
目标和需求：程序一开始提示用户，需要用户输入一系列的词汇，例如，一个名词、一个形容词等等。然后，一旦所有信息输入完成以后，程序根据已存储好的故事架构，把输入的词汇放置其中，然后打印出来。

涉及的概念：

- 字符串（str）
- 变量 （variable）
- 字符串连接（+）
- 打印（print）

非常好玩的小项目，实现的困难可能在于如何处理用户的输入以保证符合要求。与之前的项目相比，本项目更加注重对字符串的处理和使用。试试看，能不能编一些意想不到的故事来？！

```python

```


## 项目4. 大冒险（文字版）
目标和需求：有没有过历险的经历？小说或者电影里的历险场景？现在，我们自己来构建一个基本的冒险游戏吧！程序里存储着一个神秘空间，里面有大大小小含有未知事物的神秘房间和可能触发的事件，每个房间都有对应的描述和房间号。程序可以让用户查看每个房间的描述，在用户选择好要移动的房间后，触发事件并对用户进行记录，并规定游戏结束条件，例如，再完成探索5个房间后，然后活着的；或者提前找到钥匙的。可选的项目：房间是有固定结构的，比如遇到墙壁，则要通知用户无法前进，需要选择其他方向。

涉及的概念：

- 字符串（str）
- 变量 （variable）
- 字符串连接（+）
- 判断（if）
- 列表（List）
- 字典（dict）
- 打印（print）

这仅仅是一个基本的冒险类文本游戏，游戏可以设计的更为精细，但相应的代码量和难度也会随之增加。先来实现一个最基本的吧。


```python

```
