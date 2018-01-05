---
layout:     post
title:      "Pandas Usage"
subtitle:   "Pandas Common Usage"
date:       2018-01-05
author:     "Duanrw"
header-img: "img/post-bg-rwd.jpg"
catalog: true
header-mask: 0.3
tags:
    - machine_learning
    - course
    - pandas
    - matplotlib
---  

# Pandas 简介  
* pandas 是基于numpy搭建的,支持numpy中的大部分计算  
* 专业用于数据分析的Python第三方库,最适用于处理大型的机构化表格数据  

# Pandas的数据类型:  
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


#### 创建  

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

#### 查询  

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

* 索引查询  

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

#### 切片  

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

#### 其他操作  
* 类似Python的字典操作  
    * 保留in操作  
    * 保留.get() 方法  

    ```Python  

    'c' in b # 判断此键在不在b的索引中
    # 0 in b # 错误，in不会判断自动索引

    b.get('zi',60) #从b中提取索引zi的值,如果存在就取出,不存在用60代替

    ```


#### 增加  --append链接其他Series

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

#### 修改 --查询后赋值就可以修改了


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


#### 删除   
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



#### Series 类型的name属性  
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


## DataFrame


```python

import pandas as pd 

d = pd.DataFrame([[1,2,3],[9,8,7]],index=['x1','x2'],columns=['y1','y2','y3'])

d
Out[96]: 
    y1  y2  y3
x1   1   2   3
x2   9   8   7

```

* DataFrame 对象是Pandas最常用的数据类型  
    * 表格数据机构, 类似于Excel或者关系型数据库中的二维表的  
    * 是Series 对象的容器, 可视为: 二维的带标签的数组,或者Series组成的字典  
    * DataFrame 对象是由多个Series对象最为列组成的表格数据类型,每行Series值都增加了一个公共的索引  

* DataFrame 对象既有行索引,又有列索引    
    * 行索引, 表明不同行,横向索引,叫 index, 0轴, axis = 0   
    * 列索引, 表明不同列, 纵向索引,叫 columns, 1轴, axis=1  

* 其他情况:  

    * 基本操作类似于Series, 依据 行列索引进行操作  
    * 内存中以一个或者多个二维块存放,而不是列表,字典或者别的一维数据结构  
    * 常用语表达二维数据,但是也可以用于表达对位数据(层次化索引的表格型结构--没用过,不讨论)  


#### 创建  

* Python list 列表创建DataFrame  


```Python  

# 二维列表,内层是行,列索引默认0,1,2....   
a = pd.DataFrame([['小明','male',18,170,60,3000,90],['小华','female',28,160,50,8000,60]])

a
Out[98]: 
    0       1   2    3   4     5   6
0  小明    male  18  170  60  3000  90
1  小华  female  28  160  50  8000  60


# 自定义行列索引

bv = [['小明','male',18,170,60,3000,90],['小华','female',28,160,50,8000,60]]

b = pd.DataFrame(bv,index=[1,2],columns=['name','sex','age','heigh','weight','salary','looks'])

b
Out[101]: 
  name     sex  age  heigh  weight  salary  looks
1   小明    male   18    170      60    3000     90
2   小华  female   28    160      50    8000     60

```


* 等长列表或numpy数组组成的字典创建DataFrame, 注意必须等长  


```Python

# 字典索引是列,内部列表是行,行索引默认是0,1,2,3....
cv = {
    'name':['小明','小华','小红','小白','小兰'],
    'sex':[1,0,0,1,0],
    'age':[18,28,38,48,8]
}

cv
Out[103]: 
{'age': [18, 28, 38, 48, 8],
 'name': ['小明', '小华', '小红', '小白', '小兰'],
 'sex': [1, 0, 0, 1, 0]}

c = pd.DataFrame(cv)

c
Out[105]: 
   age name  sex
0   18   小明    1
1   28   小华    0
2   38   小红    0
3   48   小白    1
4    8   小兰    0

d = pd.DataFrame(cv,columns=['name','sex','age']) # 指定列排序

d
Out[107]: 
  name  sex  age
0   小明    1   18
1   小华    0   28
2   小红    0   38
3   小白    1   48
4   小兰    0    8

e = pd.DataFrame(cv,index=[1,2,3,4,5],columns=['name','sex','age','heigh']) # 传入列在数据中找不到，会产生NA值

e
Out[109]: 
  name  sex  age heigh
1   小明    1   18   NaN
2   小华    0   28   NaN
3   小红    0   38   NaN
4   小白    1   48   NaN
5   小兰    0    8   NaN


```

* 嵌套字典创建  

> 字典组成的字典,创建DataFrame,不等长也可以  


```Python

# 生成的DataFrame，外层字典键是列索引，内层键是行索引

fv = {
    'name':{1:'小明',2:'小华',3:'小红',4:'小白',5:'小兰'},
    'sex':{1:1,2:0,3:0,4:1,5:0},
    'age':{2:28,3:38,4:48,5:8} # 少一个值自动填充为Nan
}

f = pd.DataFrame(fv)

f
Out[112]: 
    age name  sex
1   NaN   小明    1
2  28.0   小华    0
3  38.0   小红    0
4  48.0   小白    1
5   8.0   小兰    0

# 内层字典键被合并、排序以形成最终索引，可以显式指定索引

g = pd.DataFrame(fv,index=[2,3,4,6])

g
Out[114]: 
    age name  sex
2  28.0   小华  0.0
3  38.0   小红  0.0
4  48.0   小白  1.0
6   NaN  NaN  NaN


```

* Series 组成的字典 创建DataFrame,同嵌套字典  

* ndarray 数组创建DataFrame  

```python 
j = pd.DataFrame(np.arange(10).reshape(2,5)) #自动生成行/列索引


k = pd.DataFrame(np.random.randn(6,4),index=[1,2,3,4,5,6],columns=['a','b','c','d']) #自定义行列索引

```

**注意** 由 **列表,元祖或者numpy数组组成的字典** 创建DataFrame,必须等长  

