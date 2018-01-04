---
layout:     post
title:      "Pandas Usage"
subtitle:   "Pandas Common Usage"
date:       2018-01-05
author:     "Duanrw"
header-img: "img/post-bg-rwd.jpg"
tags:
    - machine_learning
    - course
    - pandas
    - matplotlib
---  

## Pandas 简介  
* pandas 是基于numpy搭建的,支持numpy中的大部分计算  
* 专业用于数据分析的Python第三方库,最适用于处理大型的机构化表格数据  

## Pandas的数据类型:  
* Series 一维数据  
* DataFrame 二维数据,Series的容器,这是最常用的数据  
* Panel 三维数据,DataFrame的容器  

## Series   

```python  
import pandas as pd

s = pd.Series([1,2,3,4])

s
Out[7]: 
0    1
1    2
2    3
3    4
dtype: int64  
```  


* Series 对象: 一维的,带标签的数组  
* Series对象是ndarry数组派生类型  
* Series对象本质上由两个数组构成, 一个数组构成对象的键(index,索引,标签), 一个数组构成对象的值(values),键 ->值  
* Numpy 中数组的运算均可用于Series对象  
* Python 字典的部分操作也可以用于Series对象  


### 创建  

* Python list 创建Series


```python  

# 不设置index时使用默认索引0 1 2 3...，位置
a = pd.Series([100,25,50,90,60])

a
Out[24]: 
0    100
1     25
2     50
3     90
4     60
dtype: int64

# 自定义索引，标签
b = pd.Series([100,25,59,90,61],index=['ming','hua','hong','huang','bai'])

b
Out[26]: 
ming     100
hua       25
hong      59
huang     90
bai       61
dtype: int64

# 可存储不同数据类型
c = pd.Series([True,18,59.5,'小红'],index=['sex','age','grade','name'])

c
Out[28]: 
sex      True
age        18
grade    59.5
name       小红
dtype: object

```

* Python 字典创建Series

```python

d = pd.Series({'hong':50,'huang':90,'qing':60})

dv = {'hong':50,'huang':90,'qing':60}

di = ['hong','huang','lan']

e = pd.Series(dv,index=di)

d
Out[40]: 
hong     50
huang    90
qing     60
dtype: int64

e
Out[41]: 
hong     50.0
huang    90.0
lan       NaN
dtype: float64


```  

### 查询  

```python  

d
Out[42]: 
hong     50
huang    90
qing     60
dtype: int64

# 值数据，输出类型为array，还是ndarray数组
d.values
Out[43]: array([50, 90, 60])

# 索引，输出index类型（Pandas独有的索引类型）,本质上就是ndarray

d.index
Out[44]: Index(['hong', 'huang', 'qing'], dtype='object')

d.index[2]
Out[45]: 'qing'

d.index.values
Out[46]: array(['hong', 'huang', 'qing'], dtype=object)

```

### 索引查询  

```Python

d['qing'] 
Out[49]: 60

d.qing  # 方法调用写法
Out[53]: 60

d[1]
Out[50]: 90

d[[1,2]]
Out[51]: 
huang    90
qing     60
dtype: int64

d[['qing','hong']]  # 注意双括号
Out[52]: 
qing    60
hong    50
dtype: int64

# b[[1,'hua','huang']] #错误，两套索引并存,但不能混用


```    

### 切片  

```python  

# 索引切片,默认左开右闭
b[:2]
Out[54]: 
ming    100
hua      25
dtype: int64

# 标签切片,自定义索引  
b[:'hong']
Out[55]: 
ming    100
hua      25
hong     59
dtype: int64

# 步长
b[::-1]  # 逆序
Out[56]: 
bai       61
huang     90
hong      59
hua       25
ming     100
dtype: int64
```

### 其他操作  
* 类似Python的字典操作  
    * 保留in操作  
    * 保留.get() 方法  

    ```Python  

    'c' in b # 判断此键在不在b的索引中
    # 0 in b # 错误，in不会判断自动索引

    b.get('zi',60) #从b中提取索引zi的值,如果存在就取出,不存在用60代替

    ```


### 增加  --append链接其他Series

```Python  

p = pd.Series(['88','66'],index=['xin','xin2'])

b.append(p)
Out[58]: 
ming     100
hua       25
hong      59
huang     90
bai       61
xin       88
xin2      66
dtype: object

b.append(p,ignore_index=True) # 不使用连接对象的索引
Out[59]: 
0    100
1     25
2     59
3     90
4     61
5     88
6     66
dtype: object

```

### 修改 --查询后赋值就可以修改了


```python 

b['ming'] = 0

b['hua','hong'] = 55 # b[['hua','hong']] = 20

b
Out[62]: 
ming      0
hua      55
hong     55
huang    90
bai      61
dtype: int64

b['hua','hong'] = [35,55]

b
Out[64]: 
ming      0
hua      35
hong     55
huang    90
bai      61
dtype: int64

# 索引修改
b2 = b.copy() # 复制而非引用，类似深拷贝

b2
Out[66]: 
ming      0
hua      35
hong     55
huang    90
bai      61
dtype: int64

b2.index = ['xiaoming','xiaohua','xiaohong','xiaohuang','xiaobai']

b2
Out[68]: 
xiaoming      0
xiaohua      35
xiaohong     55
xiaohuang    90
xiaobai      61
dtype: int64

# 修改索引除了赋值还有一个reindex()方法见后

```


### 删除   
> 删除的是视图,不会修改对象值  b3 = b.drop('ming')


### **检测缺失值**  

* NaN: 非数字, Not a Number, Pandas 中它表示缺失值或者NA值  
* isnull 和 notnull 函数可以检测缺失数据 


```python  

e
Out[70]: 
hong     50.0
huang    90.0
lan       NaN
dtype: float64

pd.isnull(e) #谁是空值
Out[71]: 
hong     False
huang    False
lan       True
dtype: bool

pd.notnull(e) #谁不是空值
Out[72]: 
hong      True
huang     True
lan      False
dtype: bool

# Series 可以使用方法的方式来检测
e.isnull()
Out[73]: 
hong     False
huang    False
lan       True
dtype: bool

e.notnull()
Out[74]: 
hong      True
huang     True
lan      False
dtype: bool

```



### Series 类型的name属性  
> Series对象和它的索引都有一个name属性,该数据和Pandas其他功能关系密切  


```Python   

c
Out[87]: 
sex      True
age        18
grade    59.5
name       小红
dtype: object

c.name

c.name = 'C班级成绩表'

c.name
Out[90]: 'C班级成绩表'

c
Out[91]: 
sex      True
age        18
grade    59.5
name       小红
Name: C班级成绩表, dtype: object

c.index.name = '姓名'

c
Out[93]: 
姓名
sex      True
age        18
grade    59.5
name       小红
Name: C班级成绩表, dtype: object

```

