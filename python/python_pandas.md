---
layout: post 
---

- [引言](#引言)
- [数据结构](#数据结构)
- [常用属性及方法](#常用属性及方法)
  - [Series](#series)
  - [DataFrame](#dataframe)
- [参考资料](#参考资料)


# 引言
**Pandas**具有超强的数据处理能力，似乎是目前处理日常**csv**和**Excel**文件中最好用的工具包。结合**Matplotlib**工具包，基本可以满足上述两种文件的处理和显示工作。 

# 数据结构
我们介绍Pandas中的两种主要数据结构：**Series**与**DataFrame**，其示意图如下所示：

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC1jODQxYWI0Y2I3ZmY4OWY4LnBuZw?x-oss-process=image/format,png">
    <div class="figcaption">Series数据结构</div>
</div>

从上图可以看出，**Series**由一个一维数组（values）和与之对应的索引（index）组成。这个结构看起来和python中的字典（dict）类似，但两者有很多不同：
1. 字典是**无序**的数据结构，Series相当于**定长有序**的字典；
2. 字典中key和value是**绑定**的，Series中values和index是**独立**的；
3. 字典中key值**不可变**，Series中index是**可变**的

Series的定义可以从[官网](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html)查看，下面以一个简单的例子介绍Series的初始化：

```
In [23]: import pandas as pd

In [24]: data = pd.Series([1,2,3,4], index=['a', 'b', 'c', 'd'])

In [25]: data
Out[25]:
a    1
b    2
c    3
d    4
dtype: int64
```

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC01MGRjOWQ1MDdjYTViMTAwLnBuZw?x-oss-process=image/format,png">
    <div class="figcaption">DataFrame数据结构</div>
</div>

从上图可以看出，**DataFrame**有点像打开Excel文件的样子：一个二维数据加上行（index）和列（columns）标签。还可以将**DataFrame**看作是同一index的不同Series集合。有趣的是，DataFrame中每一列的数据类型还可以不同。

我们继续从[官网](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html)学习DataFrame的定义以及初始化：
```
# 这里直接copy了官网上的一种初始化方式感受下
In [27]: import numpy as np

In [28]: df2 = pd.DataFrame(np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]]),^M
    ...: ...                    columns=['a', 'b', 'c'])

In [29]: df2
Out[29]:
   a  b  c
0  1  2  3
1  4  5  6
2  7  8  9
```

# 常用属性及方法
## Series
在我的应用中，基本接触不到Series，因此这里就不介绍了

## DataFrame

函数|描述
:-:|:-:
df.to_csv('text.csv') | 保存dataframe到csv文件中
df = pd.read_csv('text.csv') | 读csv文件到dataframe中


# 参考资料

[数据分析三剑客之pandas](https://www.cnblogs.com/peng104/p/10398490.html)
[pandas用法总结](https://blog.csdn.net/yiyele/article/details/80605909)
