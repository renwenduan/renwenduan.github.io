---
layout:     post
title:      "Pandas Usage (1)"
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


#### **检测缺失值**  

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
j = pd.DataFrame(np.arange(10).reshape(2,5))  # 自动生成行/列索引


k = pd.DataFrame(np.random.randn(6,4),index=[1,2,3,4,5,6],columns=['a','b','c','d'])  # 自定义行列索引

```

**注意** 由 **列表,元祖或者numpy数组组成的字典** 创建DataFrame,必须等长  

#### 增加  

为不存在的行或者列赋值会创建新的行列  


```Python

bv = [['小明','male',18,170,60,3000,90],['小华','female',28,160,50,8000,60]]

b = pd.DataFrame(bv,index=[1,2],columns=['name','sex','age','heigh','weight','salary','looks'])

b
Out[5]: 
  name     sex  age  heigh  weight  salary  looks
1   小明    male   18    170      60    3000     90
2   小华  female   28    160      50    8000     60

m = b.copy()

m
Out[7]: 
  name     sex  age  heigh  weight  salary  looks
1   小明    male   18    170      60    3000     90
2   小华  female   28    160      50    8000     60

m['name']  # 查询列
Out[8]: 
1    小明
2    小华
Name: name, dtype: object

m.loc[1]  # 查询行
Out[9]: 
name        小明
sex       male
age         18
heigh      170
weight      60
salary    3000
looks       90
Name: 1, dtype: object

# 增加列
m['address'] = '北京'

m
Out[11]: 
  name     sex  age  heigh  weight  salary  looks address
1   小明    male   18    170      60    3000     90      北京
2   小华  female   28    160      50    8000     60      北京

# 顺序增加一列多行
m['address'] = ['北京','上海']

m
Out[13]: 
  name     sex  age  heigh  weight  salary  looks address
1   小明    male   18    170      60    3000     90      北京
2   小华  female   28    160      50    8000     60      上海

# 增加行
m.loc[3] = '新增'

m
Out[15]: 
  name     sex age heigh weight salary looks address
1   小明    male  18   170     60   3000    90      北京
2   小华  female  28   160     50   8000    60      上海
3   新增      新增  新增    新增     新增     新增    新增      新增
# 增加行
m.loc['a'] = ['小红','female',38,178,60,30000,95,'东京']

m
Out[17]: 
  name     sex age heigh weight salary looks address
1   小明    male  18   170     60   3000    90      北京
2   小华  female  28   160     50   8000    60      上海
3   新增      新增  新增    新增     新增     新增    新增      新增
a   小红  female  38   178     60  30000    95      东京

```

append连接,默认沿着列进行(axis = 0,列对齐)  

```python

m2 = m.copy()

m2 = m2.append(m)  # 类似于Python列表的拼接

m2
Out[20]: 
  name     sex age heigh weight salary looks address
1   小明    male  18   170     60   3000    90      北京
2   小华  female  28   160     50   8000    60      上海
3   新增      新增  新增    新增     新增     新增    新增      新增
a   小红  female  38   178     60  30000    95      东京
1   小明    male  18   170     60   3000    90      北京
2   小华  female  28   160     50   8000    60      上海
3   新增      新增  新增    新增     新增     新增    新增      新增
a   小红  female  38   178     60  30000    95      东京

```

#### 查询(索引,切片,过滤) 

整体情况查询,和DataFrame常用属性

``Python
e
Out[26]: 
  name  sex  age heigh
1   小明    1   18   NaN
2   小华    0   28   NaN
3   小红    0   38   NaN
4   小白    1   48   NaN
5   小兰    0    8   NaN

e.head() # 显示头部几行

e.tail() # 显示末尾几行

e.info() # 相关信息概览
<class 'pandas.core.frame.DataFrame'>
Int64Index: 5 entries, 1 to 5
Data columns (total 4 columns):
name     5 non-null object
sex      5 non-null int64
age      5 non-null int64
heigh    0 non-null object
dtypes: int64(2), object(2)
memory usage: 200.0+ bytes

e.shape # 行数 列数
Out[30]: (5, 4)

e.dtypes # 列数据类型
Out[31]: 
name     object
sex       int64
age       int64
heigh    object
dtype: object

e.index # 获取行索引
Out[32]: Int64Index([1, 2, 3, 4, 5], dtype='int64')

e.columns # 获取列索引
Out[33]: Index([u'name', u'sex', u'age', u'heigh'], dtype='object')

e.values # 获取值
Out[34]: 
array([['\xe5\xb0\x8f\xe6\x98\x8e', 1L, 18L, nan],
       ['\xe5\xb0\x8f\xe5\x8d\x8e', 0L, 28L, nan],
       ['\xe5\xb0\x8f\xe7\xba\xa2', 0L, 38L, nan],
       ['\xe5\xb0\x8f\xe7\x99\xbd', 1L, 48L, nan],
       ['\xe5\xb0\x8f\xe5\x85\xb0', 0L, 8L, nan]], dtype=object)

e.index.values
Out[35]: array([1, 2, 3, 4, 5], dtype=int64)

e.index.values.tolist
Out[36]: <function tolist>

e.index.values.tolist()  # 可以再使用列表的方法来取出各个值
Out[37]: [1L, 2L, 3L, 4L, 5L]


```

类似于list/ndarray的查询方式

```Python
m

# 查询单列
m['name']
m.name # .写法容易与其他预留关键字产生冲突,不推荐

# 查询多列
m[['name','address']] # 双中括号

# 查询单值
m['name'][2] # 先列后行

```

#### Pandas专用的查询方式

索引,切片,过滤三种查询方式

**索引查询** : 用于连续或者不连续(行列有间隔的)行列区块查询

有标签索引和位置索引查询两种方式,都是先行后列  

* m.loc[行,列]  # 标签索引,自定义索引

* m.iloc[行,列]  # 位置索引,默认索引


查询单行

```Python
m
Out[45]: 
  name     sex age heigh weight salary looks address
1   小明    male  18   170     60   3000    90      北京
2   小华  female  28   160     50   8000    60      上海
3   新增      新增  新增    新增     新增     新增    新增      新增
a   小红  female  38   178     60  30000    95      东京

m.loc['a',:] # 标签索引
Out[46]: 
name           小红
sex        female
age            38
heigh         178
weight         60
salary      30000
looks          95
address        东京
Name: a, dtype: object

m.loc['a'] # 标签索引,简写
Out[47]: 
name           小红
sex        female
age            38
heigh         178
weight         60
salary      30000
looks          95
address        东京
Name: a, dtype: object

m.iloc[3] # 位置索引，从0开始
Out[48]: 
name           小红
sex        female
age            38
heigh         178
weight         60
salary      30000
looks          95
address        东京
Name: a, dtype: object

```

查询单列

```Python
m.loc[:,'name'] # 标签索引
Out[49]: 
1    小明
2    小华
3    新增
a    小红
Name: name, dtype: object

m.iloc[:,0] # 位置索引
Out[50]: 
1    小明
2    小华
3    新增
a    小红
Name: name, dtype: object

```

查询多行,双中括号

```Python

# 这里不一一演示了,有兴趣的可以复制然后做一下
m.loc[[1,'a'],:] # 标签索引
m.loc[[1,'a']] # 标签索引，简写
m.iloc[[0,3]] # 位置索引
```

查询单个单元格

```Python

m.loc[1,'name'] # 标签索引，先行后列，一行一列
m.iloc[0,0] # 位置索引

```

查询多个单元格

```Python

m.loc[1,['name','address']] # 一行多列
m.loc[[1,'a'],'name'] # 多行一列
m.loc[[1,'a'],['name','address']] # 多行多列，不连续
# m.loc[[1,2],['name','address']] # 注意：多行多列情况下，loc纯数字参数会被解析为iloc默认索引

m.iloc[0,[0,7]]
m.iloc[[0,3],0]
m.iloc[[0,3],[0,7]]

```

**切片查询**　：　用于连续的行列区块查询　　

* 比索引查询点单方便,但是不能查询费连续的行列区块  
* 单个或者不连续的行列区块使用索引查询,连续的行列区块使用切片查询  

切片查询的区间：

* 默认索引是左开右闭区间（包含起始元素，不包含结束元素）
* 标签索引是左开右开区间（因为标签索引不包含位置信息，使用时很难知道索引前后是什么）

```python

#切片选取连续区块。行，列。左开右闭
m.iloc[1:3,3:6]
Out[51]: 
  heigh weight salary
2   160     50   8000
3    新增     新增     新增

m.loc['1':'3','heigh':'salary'] #loc参数如果是纯数字会出错
Out[52]: 
Empty DataFrame
Columns: [heigh, weight, salary]
Index: []

n
Out[55]: 
  name     sex age heigh weight salary looks address
a   小明    male  18   170     60   3000    90      北京
b   小华  female  28   160     50   8000    60      上海
c   新增      新增  新增    新增     新增     新增    新增      新增
d   小红  female  38   178     60  30000    95      东京


#切片选取连续行
n.loc[:'c',:]
Out[56]: 
  name     sex age heigh weight salary looks address
a   小明    male  18   170     60   3000    90      北京
b   小华  female  28   160     50   8000    60      上海
c   新增      新增  新增    新增     新增     新增    新增      新增

n.loc['b':'c',:] # 左闭右闭
Out[57]: 
  name     sex age heigh weight salary looks address
b   小华  female  28   160     50   8000    60      上海
c   新增      新增  新增    新增     新增     新增    新增      新增

n.loc['a':'c',:] # 左闭右闭
Out[58]: 
  name     sex age heigh weight salary looks address
a   小明    male  18   170     60   3000    90      北京
b   小华  female  28   160     50   8000    60      上海
c   新增      新增  新增    新增     新增     新增    新增      新增

n.iloc[1:3,:] #左开右闭
Out[59]: 
  name     sex age heigh weight salary looks address
b   小华  female  28   160     50   8000    60      上海
c   新增      新增  新增    新增     新增     新增    新增      新增


#切片选取连续列
n.loc[:,'heigh':'salary']
Out[60]: 
  heigh weight salary
a   170     60   3000
b   160     50   8000
c    新增     新增     新增
d   178     60  30000

n.iloc[:,3:6]
Out[61]: 
  heigh weight salary
a   170     60   3000
b   160     50   8000
c    新增     新增     新增
d   178     60  30000


#切片选取连续行列，连续单元格  

n.loc['b':'c','heigh':'salary']
Out[62]: 
  heigh weight salary
b   160     50   8000
c    新增     新增     新增

n.iloc[1:3,3:6]
Out[63]: 
  heigh weight salary
b   160     50   8000
c    新增     新增     新增

```

**过滤查询** 

* 不通过索引而是通过值来进行查询  
* 用于结果索引不确定的查询  
* 通过运算所得布尔值对结果进行过滤  

通过布尔值过滤

```Python
n
# n.loc[:,'heigh'] > 165 # 出错，列值不一致

# 生成新对象
p = n.loc[['a','b','d']].copy()
p.loc['e'] = ['小白','male',28,166,100,50000,5,'广州']
p.loc['f'] = ['小黄','male',18,206,70,10000,80,'杭州']
p.loc['g'] = ['小兰','female',22,167,55,11000,90,'深圳']
p
```

```Python
# 通过某列值过滤数据，返回布尔值
p['looks'] >= 80
p.loc[:,'looks'] >= 80

# 布尔值做DataFrame参数，返回Dataframe对象
p[p['looks'] >= 80]
p.loc[p.loc[:,'looks'] >= 80]

# 多重条件过滤，逻辑运算 & 且，| 或，- 非(或用 != 判断)
(p['looks'] >= 80) & (p['sex'] == 'female')
(p.loc[:,'looks'] >= 80) & (p.loc[:,'sex'] == 'female')

p[(p['looks'] >= 80) & (p['sex'] == 'female')]
p.loc[(p.loc[:,'looks'] >= 80) & (p.loc[:,'sex'] == 'female')]

```

通过**where**过滤  

```Python

# p[p > 60] # 有非数值数据，无效

# 创建纯数值对象
r = p.loc[:,'age':'looks']

# 通过where选择数据
s = r[r > 60]
s
s.loc['d','weight']

```

通过 **isin()** 过滤，查询某列中包含某值的所有行

```Python

p['address'].isin(['北京','深圳'])
p[p['address'].isin(['北京','深圳'])]

```

#### 修改

凡是能通查询得到的值,直接对其进行赋值就能修改

#### 删除

* drop()只能用标签索引定位行列

* 复杂的删除需求，可以使用查询将结果出来赋予新变量即可

```Python

# 默认删除行，默认只改动视图
w.drop('a') # 删除单行
w.drop(['a','e']) # 删除多行

# 删除列
# axis=1删除列，默认axis=0删除行
w.drop('address',axis=1)
# inplace=True 改动原数据，默认inplace=False 只改动视图
w.drop('address',axis=1,inplace=True)

```


## Pandas 数据费存取

> pandas能存取各种格式的类型数据,比如内存,文本,csv,json,html,Excel,hdf5,sql等

```Python

df = pd.DataFrame(np.random.randn(1000, 4),columns=['A', 'B', 'C', 'D'])

df
Out[66]: 
            A         B         C         D
0   -0.019372 -0.187205 -1.878929 -0.927317
1    0.290658 -0.606108 -2.196774 -2.244475
...
999 -0.695898 -0.256264  0.413073  0.597988

```

#### pandas 存取csv

写入csv

```python 

df.to_csv('foo.csv')

df.to_csv('foo.csv',index=False) #不保存行索引

```

读取csv

```Python

pd.read_csv(
    'aaa.csv', #文件名
    sep=',', #指定分隔符，默认逗号
    usecols=[0,1,2,4], #读取指定列
    nrows=10, #读取前几行
    encoding='GBK' #编码，根据文本编码修改，默认utf-8,可以指定为GBK
)

'''
data,time,name,age
20100101,000000,"张三",18
20100101,230000,"李,四",28
'''

x = pd.read_csv(
    'aaa.csv',
    parse_dates={'timestamp': ['data','time']}, #将两列合并解析为时间格式
    index_col='timestamp' #将时间设为行索引
)

```

*  csv文件内有汉字等特殊符号时，csv文件编码应为utf-8(无BOM)可默认正常读取，如果编码是ANSI，加参数encoding='GBK'
*  数据内有逗号时，左右加英文半角双引号，可以正常解析

#### Pandas 读取Excel(xlsx)

写入Excel文件

> df.to_excel(''foo.xlsx', sheet_name='Sheet1'')

从Excel中读取文件  

> pd.read_excel('foo.xlsx', 'Sheet1', index_col=None, na_values=['NA'])  


## Pandas 统计分析  

Pandas数据的基本统计分析 和 numpy 的函数相似  

```python  

dates = pd.date_range('20130101',periods=10)
dates
df = pd.DataFrame(np.random.randn(10,4),index=dates,columns=['A','B','C','D'])
df

df.describe() #快速统计结果
df.mean() # 按列求平均值
df.mean(1) # 按行求平均值

```  

#### 基本的统计分析函数  


* .describe() 针对0轴(列)的统计汇总，计数/平均值/标准差/最小值/四分位数/最大值

* .sum() 计算数据的总和,按0轴计算(各行计算),下同,要按列计算参数1

* .count() 非NaN值数量

* .mean() .median() .mode() 计算数据的算数平均值/算数中位数/众数

* .var() .std() 计算数据的方差/标准差

* .min() .max() 计算数据的最小值/最大值

只适用于Series:


* .argmin(),.argmax() 计算数据最大值/最小值所在位置的索引位置(自动索引,用她是因为很容易切片等操作)

* .idxmin(),.idxmax() 计算数据最大值/最小值所在位置的索引(自定义索引)


```python 

a = pd.Series([9,8,7,6],index=['a','b','c','d'])
a
b = pd.DataFrame(np.arange(20).reshape(4,5),index=['c','a','d','b'])
b

a.describe()
type(a.describe()) #series对象
a.describe()['count']

b.describe() #默认0轴运算
type(b.describe()) #dataframe对象

#返回横行数据,series
b.describe().loc['max']
b.describe().iloc[7]

#返回一列值,这里第2列
b.describe()[2]
#b.describe()[2]
b.describe().loc[:,2]

```

#### 数据的累计统计分析  

* 对序列的前1-n个数累计运算

* 可减少for循环的使用

累计统计分析函数,适用于series和dataframe类型

* .cumsum() 依次给出前1/2/.../n个数的和

* .cumprod() 依次给出前1/2/.../n个数的积

* .cummax() 依次给出前1/2/.../n个数的最大值

* .cummin() 依次给出前1/2/.../n个数的最小值


```Python

b = pd.DataFrame(np.arange(20).reshape(4,5),index=['c','a','d','b'])
b

b.cumsum() #列的累加和
b.cumprod() #列的累加积

```

#### 滚动计算(窗口计算)函数

适用series/dataframe

* .rolling(w).sum() 依次计算相邻w个元素的和

* .rolling(w).mean() 依次计算相邻w个元素的算数平均值

* .rolling(w).var() 依次计算相邻w个元素的方差

* .rolling(w).std() 依次计算相邻w个元素的标准差

* .rolling(w).min .max() 依次计算相邻w个元素的最小值/最大值

```Python

b.rolling(2).sum() #纵向列,以两个元素为单位,做求和运算
b.rolling(3).sum()

```


## 剩余内容见下一篇......
