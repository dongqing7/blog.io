---
layout: post
title: 用Python实现对数据中异常值的扫描
date: 2016-11-9
categories: Python Practice
tags: [teaching, Python, practice, project ]
description: 教授这门课不是义务，准备的本身就是报酬。
---

熵是对随机性的测量方法。 这个概念最初起源于热力学的研究。而自从美国科学家香浓（Claude E. Shannon）将其概念应用于数字通信领域中之后，熵在计算和通信科学便大行其道。香浓当时对数字文件的理论最大压缩率非常感兴趣，最后将研究成果发表在1948年的论文“通信的数学理论”上，该文也奠定了信息论的基础。而熵（entropy）成为了信息论的基本概念，其计算公式先请自行百度。

如何理解熵的概念呢？举个不算恰当的例子（*这样说是深受初中政治老师的毒害←_←*），日本漫画《网球优等生》里的主人公丸尾，初学网球时，在地区大赛取得了不错的成绩，那时他已经掌握了直球、上旋球和下旋球三种技法。这个时候，如果要对丸尾的网球技术做一个粗糙的总结，可以根据其在地区比赛中的表现做如下统计：直球（z，355）、上旋球（s，309）、下旋球（x，25）、其他（q，18），共挥拍（t，707）。有了这个数据，就可以通过熵的计算公式，计算丸尾的熵值为**1.33**。先别急着知道这个值的含义，只有通过比较才能清楚了解。那么半年过去之后，丸尾的球技更进一步，他在原有技术的基础上，有学会了“9宫格”的击球方式，这大大的提升了他的攻击和防守能力，那么在全国青少年预决赛中，他的统计数据如下（呃，数据是我编的啦！为简单起见，9宫格简化为左边、中路和右边三种，分别用l、m和r代表）：{l-z：80， l-s：91，l-x：48， m-z：89，m-s：69，m-x：76，r-z：71，r-s：93，r-x：86}，同样用熵的计算公式计算，这时的丸尾的球路熵值为：**3.15**。如果直接去比较的话，那这回的熵值是半年前1.37倍。但一般并不这么比，而是从信息论的角度去解读它：熵又称为自信息（self-information）,可以视为描述一个随机变量的不确定性的数量。这里丸尾的球路就是我们要计算的随机变量，作为网球职业经理人或是网球星探，最想知道的是他能在球场上所发挥的球路到底有多少，换句话说，需要多少信息能够对他的球路进行完整描述。上面的计算，我们使用的是log以2为底的运算，那么该熵值的单位就是二个bit，那么半年前丸尾的每次击球需要至少1.4个bit进行描述，而一年他在预决赛中的球路则需要3.5个bit。换句话说，我们需要更多的信息去表达他现在的水平。

例如下面是地区比赛时描述丸尾球路的编码设计，每表示一种球路，需要1.4个bit。

|  球路  | 直球 z | 上旋球 s | 下旋球 x | 其他 q |
| :--: | :--: | :---: | :---: | :--: |
|  编码  |  10  |  11   |  01   |  00  |

其实另一种更直观的方式是动画里，丸尾对对手球路的整理，可视化效果特别好，类似下面的例子：
![球路描述](http://www.dilidili.com/uploads/150711/1-150G1144553961.jpg "球路描述")

啊，不知不觉又码了这么多，相信应该说明白了。那除了去猜测某个新星，熵的应用可谓如瑞士军刀，短小精悍，看看下面的需求和实现，尽早融会贯通，把瑞士军刀放在你的口袋吧！

下面的这个例子来源于[这里](http://blog.dkbza.org/2007/05/scanning-data-for-entropy-anomalies.html)，[以及他的修正版](https://deadhacker.com/2007/05/13/finding-entropy-in-binary-files/)，感兴趣的同学请移步。
需求：在一段长文本（自然语言，例如英文）中设有一段密文，该密文是由一连串8位字符构成，你需要做的是在成百上千的文本中找到这个密文。想想看，如果你在我党中央情报处当信息员，你该如何找到它们？

时间关系，我先把代码码在下面，下周给大家作进一步的讲解：


首先，我们需要引入一下pakcage，

```python
import math
from pylab import *
import random
import os
from matplotlib.ticker import MultipleLocator, FormatStrFormatter
```

定义用于计算熵值的函数H

```python
import math

def H(data):
  if not data:
    return 0
  entropy = 0
  for x in range(256):
    p_x = float(data.count(chr(x)))/len(data)
    if p_x > 0:
      entropy += - p_x*math.log(p_x, 2)
  return entropy
```

```python
def entropy_scan(data, block_size=256) :
  for block in (data[x:block_size+x]
    for x in range(len(data) - block_size)):
        yield H(block)
```

```python
def plotting(results, filename):
    imgdpi = 100

    majorLocator   = MultipleLocator(0x400)   # mark every 1024 bytes
    majorFormatter = FormatStrFormatter('%X') # change to %d to see decimal offsets

    ax = subplot(111)
    plot(results, linewidth=2.0, antialiased=False)
    subplots_adjust(left=0.02, right=0.99, bottom=0.2)

    #ax.axis([0,filesize,0,8])
    ax.xaxis.set_major_locator(majorLocator)
    ax.xaxis.set_major_formatter(majorFormatter)
    xticks(rotation=315)

    xlabel('block (byte offset)')
    ylabel('entropy')
    title('Entropy levels')

    grid(True)

    img = gcf()
    #img.set_size_inches(imgwidth, 6)
    img.savefig(filename+".png", dpi=imgdpi)

    draw()
    show()
```

```python
if __name__ == '__main__':
    data = ''.join (
      [chr(random.randint(0, 255)) for _ in range(30240)] +
      [chr(random.randint(0, 64)) for _ in range(1024)] +
      [chr(random.randint(0, 255)) for _ in range(32240)] )
    
    results = list(entropy_scan(data))
    plotting(results, r'df.png')
```
