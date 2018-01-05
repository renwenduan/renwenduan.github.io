---
layout:     post
title:      "Pandas Usage (2)"
subtitle:   "Pandas Common Usage"
date:       2018-01-05
author:     "Duanrw"
header-img: "img/post-bg-kuaidi.jpg"
catalog: true
header-mask: 0.3
tags:
    - machine_learning
    - course
    - pandas
    - matplotlib
--- 

## pandas 数据排序

> **.sort_index()** 在指定轴上根据索引进行排序，索引排序后内容会跟随排序

```Python

b
Out[78]: 
    0   1   2   3   4
c   0   1   2   3   4
a   5   6   7   8   9
d  10  11  12  13  14
b  15  16  17  18  19

b.sort_index()
Out[79]: 
    0   1   2   3   4
a   5   6   7   8   9
b  15  16  17  18  19
c   0   1   2   3   4
d  10  11  12  13  14

b.sort_values()
Out[80]: 
<bound method DataFrame.sort_values of     0   1   2   3   4
c   0   1   2   3   4
a   5   6   7   8   9
d  10  11  12  13  14
b  15  16  17  18  19>

b.sort_index(ascending=False)
Out[81]: 
    0   1   2   3   4
d  10  11  12  13  14
c   0   1   2   3   4
b  15  16  17  18  19
a   5   6   7   8   9

b.sort_index(axis=0, ascending=False) #按行标排序，ascending：False为降序

b.sort_index(axis=1, ascending=False) #按列标排序

```

* Series.sort_values(axis=0,ascending=True)
* DataFrame.sort_values(by,axis=0,ascending=True)
    * by：axis轴上的某个索引或索引列表

```Python
df
Out[86]: 
                   A         B         C         D
2013-01-01 -0.351042  1.125637  1.053039 -0.630667
2013-01-02  0.326019  0.122807  0.668532 -2.274802
2013-01-03 -0.387315 -0.274692  1.420606  0.314601
2013-01-04 -1.461497  1.003460 -0.147864  0.869765
2013-01-05  0.575583 -1.955573  2.149040  0.092377
2013-01-06  0.107741  0.693995  0.729713  2.523184
2013-01-07 -3.104186 -0.395048  0.329386  0.624749
2013-01-08 -0.477212  1.423984  1.193984 -0.301437
2013-01-09  0.734418 -0.109489  0.732805  0.979757
2013-01-10  0.611182  1.115951  0.165254 -1.970597

c = df.sort_values('B')

c = df.sort_values('B',ascending = False)

c
Out[89]: 
                   A         B         C         D
2013-01-08 -0.477212  1.423984  1.193984 -0.301437
2013-01-01 -0.351042  1.125637  1.053039 -0.630667
2013-01-10  0.611182  1.115951  0.165254 -1.970597
2013-01-04 -1.461497  1.003460 -0.147864  0.869765
2013-01-06  0.107741  0.693995  0.729713  2.523184
2013-01-02  0.326019  0.122807  0.668532 -2.274802
2013-01-09  0.734418 -0.109489  0.732805  0.979757
2013-01-03 -0.387315 -0.274692  1.420606  0.314601
2013-01-07 -3.104186 -0.395048  0.329386  0.624749
2013-01-05  0.575583 -1.955573  2.149040  0.092377

c = df.sort_values('2013-01-01',axis=1,ascending=False) #指定1轴排序,这里指定1轴a行为基准排序

c
Out[91]: 
                   B         C         A         D
2013-01-01  1.125637  1.053039 -0.351042 -0.630667
2013-01-02  0.122807  0.668532  0.326019 -2.274802
2013-01-03 -0.274692  1.420606 -0.387315  0.314601
2013-01-04  1.003460 -0.147864 -1.461497  0.869765
2013-01-05 -1.955573  2.149040  0.575583  0.092377
2013-01-06  0.693995  0.729713  0.107741  2.523184
2013-01-07 -0.395048  0.329386 -3.104186  0.624749
2013-01-08  1.423984  1.193984 -0.477212 -0.301437
2013-01-09 -0.109489  0.732805  0.734418  0.979757
2013-01-10  1.115951  0.165254  0.611182 -1.970597

```

**NaN空值统一放在排序末尾**


## Pandas 绘图


Pandas的绘图方法封装了Matplotlib的pyplot方法，可以提供简单的绘图功能,对于DataFrame来说，.plot是一种将所有列及其标签进行绘制的简便方法

不常用，实际应用中，一般仍使用Matplotlib绘图

Jupyter notebook中如不显示Pandas绘制图像，解决方法:

    * 载入import Matplotlib.pyplot as plt，Pandas绘图代码最后加 plt.show()
    
    * 或者直接载入IPython魔术命令 %matplotlib inline，或%pylab inline(不推荐)（非IPython的py文档载入 from pylab import *）

```python 
import numpy as np
import pandas as pd
%matplotlib inline

ts = pd.Series(np.random.randn(1000), index=pd.date_range('1/1/2000', periods=1000))

ts = ts.cumsum()
ts.plot()
#plt.show()

```

![图片](https://ws1.sinaimg.cn/large/b3c7bdb6gy1fn5qrom7e0j20ae07bweq.jpg)

在DataFrame中，**plot()** 可以绘制所有带有标签的列

```Python
df = pd.DataFrame(np.random.randn(1000, 4), index=ts.index,columns=['A', 'B', 'C', 'D'])

df = df.cumsum()
df.plot()
#plt.show()
```

![DataFrame](https://ws1.sinaimg.cn/large/b3c7bdb6gy1fn5qtu8n99j20ak07b74x.jpg)

>df.plot(kind='line',figsize=(15,10),grid=True)  
> plt.show()  

**kind** 参数可以绘制的图形：

    ‘line’ : line plot (default)
    ‘bar’ : vertical bar plot
    ‘barh’ : horizontal bar plot
    ‘hist’ : histogram
    ‘box’ : boxplot
    ‘kde’ : Kernel Density Estimation plot
    ‘density’ : same as ‘kde’
    ‘area’ : area plot
    ‘pie’ : pie plot
    ‘scatter’ : scatter plot
    ‘hexbin’ : hexbin plot


## Pandas 缺失值的处理

Pandas用 np.nan 代表缺失数据  

**reindex() 可以修改索引, 会返回一个数据的副本:**


```python 
df1 = df.reindex(index=dates[0:4], columns=['A','B','C','D','E'])
df1

df1 = df.reindex(index=dates[0:4], columns=['A','B','C','D']+['E'])
df1

df1 = df.reindex(index=dates[0:4], columns=list(df.columns) + ['E'])
df1

df1.loc[dates[0]:dates[1],'E'] = 1
df1

```

**对缺失值进行填充--fillna():**

```python 

df1.fillna(value=5)

df1['E'] = df1['E'].fillna(value=5)
df1

```

**丢掉含有缺失项的行:**

```Python
df1.dropna(how='any')

```

**对缺失值进行布尔赋值:**

```Python
pd.isnull(df1)
```

## Pandas 数据操作

**遍历DataFrame**

```Python
a = pd.DataFrame(np.arange(12).reshape(3,4),index=[1,2,3])

a
Out[94]: 
   0  1   2   3
1  0  1   2   3
2  4  5   6   7
3  8  9  10  11

for index, row in a.iterrows():
    print(index)
    
1
2
3

for index, row in a.iterrows():
    print(row)
    
0    0
1    1
2    2
3    3
Name: 1, dtype: int32
0    4
1    5
2    6
3    7
Name: 2, dtype: int32
0     8
1     9
2    10
3    11
Name: 3, dtype: int32

```

**字符串方法** 

Series对象在其str属性中配备了一组字符串处理的方法,可以很容易的应用到每个元素中去.\

```Python

t
Out[99]: 
0    a_b_c_d
1      c_d_e
2        NaN
3      f_g_h
dtype: object

t.str.cat(['A','B','C','D'],sep=',') #拼接字符串
Out[100]: 
0    a_b_c_d,A
1      c_d_e,B
2          NaN
3      f_g_h,D
dtype: object

t.str.cat(['A','B','C','D'],sep=',') #拼接字符串
Out[101]: 
0    a_b_c_d,A
1      c_d_e,B
2          NaN
3      f_g_h,D
dtype: object

t.str.split('_') #切分字符串
Out[102]: 
0    [a, b, c, d]
1       [c, d, e]
2             NaN
3       [f, g, h]
dtype: object

t.str.get(0) #获取指定位置的字符串
Out[103]: 
0      a
1      c
2    NaN
3      f
dtype: object

t.str.replace("_", ".") #替换字符串
Out[104]: 
0    a.b.c.d
1      c.d.e
2        NaN
3      f.g.h
dtype: object

t.str.pad(10, fillchar="?") #左补齐
Out[105]: 
0    ???a_b_c_d
1    ?????c_d_e
2           NaN
3    ?????f_g_h
dtype: object

t.str.pad(10, side="right", fillchar="?") #右补齐
Out[106]: 
0    a_b_c_d???
1    c_d_e?????
2           NaN
3    f_g_h?????
dtype: object

t.str.center(10, fillchar="?") #中间补齐
Out[107]: 
0    ?a_b_c_d??
1    ??c_d_e???
2           NaN
3    ??f_g_h???
dtype: objec

```

**数据转秩(行列转换)**

>df.T  # 行列转换


**对数据应用function**

> df.apply(np.cumsum)#cumsum 累加

**频率**

> s.value_counts()  # 计算出现的次数,类似直方图


## Pandas库的数据运算

#### 算数运算法则

* 根据行列索引,补齐运算(不同索引不运算,行列索引相同才运算),默认产生浮点数
* 补齐时默认填充NaN空值
* 二维和一维,一维和0维之间采用广播运算(低维元素与每一个高维元素运算)
* 采用 +-*/符号的二元运算会产生新的对象

```Python
a = pd.DataFrame(np.arange(12).reshape(3,4))
a

b = pd.DataFrame(np.arange(20).reshape(4,5))
b

# 维度相同,行列内元素个数不同的运算,自动补齐,缺项NaN
a + b
a * b

```

除了使用+-*/,也可使用方法形式,好处是可以增加可选参数  


* .add(d,**argws) 类型间加法运算,可选参数
* .sub(d,**argws) 类型间减法运算,可选参数
* .mul(d,**argws) 类型间乘法运算,可选参数
* .div(d,**argws) 类型间除法运算,可选参数

```Python
b.add(a,fill_value = 100) #将a和b之间的缺失元素用100补齐并参加与运算
a.mul(b,fill_value = 0)

```

**不同维度运算**

```Python

b = pd.DataFrame(np.arange(20).reshape(4,5))
b
c = pd.Series(np.arange(4))
c

c - 10
b - c #b的每一行都与c运算一遍,二维和一维运算默认在轴1（行）发生
b.sub(c,axis=0) #指定用 列 参与运算

```

**比较运算法则**

* 二维和一维/一维和零维间为广播运算
* 比较运算只能比较相同索引的元素,不进行补齐(尺寸不同会报错)
* 采用>< >= <= -- !=等符号进行的二元运算产生布尔对象

```Python
a = pd.DataFrame(np.arange(12).reshape(3,4))
a
d = pd.DataFrame(np.arange(12,0,-1).reshape(3,4))
d

a > d #bool值表
a == d


b = pd.DataFrame(np.arange(12).reshape(3,4))
b
c = pd.Series(np.arange(4))
c

a > c
c > 0

```


## 三维数据类型 Panel

Panel 面板数据，平行数据，3维数据容器

* 3维版的DataFrame
* 类似DataFrame的列是Series对象一样，Panel中的每一项都是一个DataFrame对象
* 可用一个DataPFrame对象组成的字典或一个三维ndarray创建Panel对象
* Panel就是给DataFrame加了index和columns

**三个轴的语义含义：**

* items: 0轴， 每个项目对应其中的一个DataFrame
* major_axis: 1轴，它是每个DataFrame的index行
* minor_axis: 2轴，它是每个DataFrame的column列

## pandas 数据规整

#### 合并

**连接**

连接pandas对象 **.concat()**

```Python

df = pd.DataFrame(np.random.randn(10, 4))

df
Out[109]: 
          0         1         2         3
0 -2.070164 -2.104048  0.423111 -0.202614
1  0.449024 -1.003771 -1.077943  1.201632
2  0.170547  0.067125  0.293196 -1.499813
3  0.247998 -1.158033  0.206656 -0.279089
4 -0.597541  0.445353  1.870879 -0.906847
5 -1.410490 -0.939774  0.255985  0.696811
6 -0.991901 -1.102371 -0.220248  0.105564
7  0.532598 -0.730126 -0.664180  0.144844
8  0.297871  0.698161  0.870107  1.140705
9  0.097863  0.067722 -1.558896 -0.106901

pieces = [df[:2], df[3:5], df[7:]]

pd.concat(pieces)
Out[111]: 
          0         1         2         3
0 -2.070164 -2.104048  0.423111 -0.202614
1  0.449024 -1.003771 -1.077943  1.201632
3  0.247998 -1.158033  0.206656 -0.279089
4 -0.597541  0.445353  1.870879 -0.906847
7  0.532598 -0.730126 -0.664180  0.144844
8  0.297871  0.698161  0.870107  1.140705
9  0.097863  0.067722 -1.558896 -0.106901

```

追加 **.append()**

```Python
df = pd.DataFrame(np.random.randn(8, 4), columns=['A','B','C','D'])

df
Out[113]: 
          A         B         C         D
0  0.071887 -0.766916  0.067371  0.690515
1  0.346996  0.072076  0.214704  0.640955
2  1.537361 -0.400454  1.348213 -1.815034
3  0.382017 -2.282443  0.165918  0.895235
4  1.982529  0.759425 -0.739336 -0.499609
5 -2.321614  0.867844  0.544679 -1.898956
6 -0.241947  0.796973  0.056873  0.919893
7 -1.013292  0.091175 -1.894975  0.759243

s = df.iloc[3]

s
Out[115]: 
A    0.382017
B   -2.282443
C    0.165918
D    0.895235
Name: 3, dtype: float64

df.append(s, ignore_index=True)
Out[116]: 
          A         B         C         D
0  0.071887 -0.766916  0.067371  0.690515
1  0.346996  0.072076  0.214704  0.640955
2  1.537361 -0.400454  1.348213 -1.815034
3  0.382017 -2.282443  0.165918  0.895235
4  1.982529  0.759425 -0.739336 -0.499609
5 -2.321614  0.867844  0.544679 -1.898956
6 -0.241947  0.796973  0.056873  0.919893
7 -1.013292  0.091175 -1.894975  0.759243
8  0.382017 -2.282443  0.165918  0.895235

```

#### 分组

**groupby():** 一般指以下一个或多个操作步骤

* Splitting 将数据分组
* Applying 对每个分组应用不同的function
* Combining 使用某种数据结果展示结果

```Python

df = pd.DataFrame({
    'A' : ['foo', 'bar', 'foo', 'bar','foo', 'bar', 'foo', 'foo'],
    'B' : ['one', 'one', 'two', 'three','two', 'two', 'one', 'three'],
    'C' : np.random.randn(8),
    'D' : np.random.randn(8)
})

df
Out[118]: 
     A      B         C         D
0  foo    one  0.619536  0.357982
1  bar    one -1.124971 -0.384746
2  foo    two -0.310408 -1.478254
3  bar  three  0.106225 -0.501906
4  foo    two  1.270072  0.508074
5  bar    two -0.346177 -1.708633
6  foo    one  1.452838  0.096211
7  foo  three -1.550773  1.026659

#分组后sum求和:
a = df.groupby('A').sum()

a = df.groupby('A',as_index=False).sum()

a
Out[121]: 
     A         C         D
0  bar -1.364923 -2.595285
1  foo  1.481265  0.510671


#对多列分组后sum:
b = df.groupby(['A','B']).sum()

b = df.groupby(['A','B'],as_index=False).sum()

b
Out[124]: 
     A      B         C         D
0  bar    one -1.124971 -0.384746
1  bar  three  0.106225 -0.501906
2  bar    two -0.346177 -1.708633
3  foo    one  2.072374  0.454192
4  foo  three -1.550773  1.026659
5  foo    two  0.959665 -0.970180


```

