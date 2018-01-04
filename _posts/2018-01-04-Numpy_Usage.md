---
layout:     post
title:      "numpy&pandas&matplotlib usage"
subtitle:   "Common Usage"
date:       2018-01-04
author:     "Duanrw"
header-img: "img/post-bg-universe.jpg"
tags:
    - machine_learning
    - course
    - numpy
    - pandas
    - matplotlib
---  

## numpy 矩阵和数组的转换
> np.array(matrix)  # 将矩阵转换成数组  
> np.matrix(array)  # 将数组转换为矩阵  

```python
A
Out[100]: 
array([[ 1.,  2.,  3.],
       [ 4.,  5.,  6.]])

D = np.matrix(A)

D
Out[102]: 
matrix([[ 1.,  2.,  3.],
        [ 4.,  5.,  6.]])

B = np.array(D)

B
Out[104]: 
array([[ 1.,  2.,  3.],
       [ 4.,  5.,  6.]])
```  

## ndarray-数组的操作(增查改删)  

* 查询  

    * 索引: 获取数组中特定位置的元素
    * 切片: 获取数组中特定元素的子集    

    * 一维数组的索引和切片和Python的列表类似
    ```Python
    a = np.array([1,2,3,4,5,6,7])
    # 索引
    a[2]
    Out[70]: 3
    # 切片,起始位置,终止位置,步长
    a[1:5:2]
    Out[71]: array([2, 4])
    ```  

    * 多维数组的索引和切片--每个逗号分隔一个维度,每个维度可以使用切片操作来进行取索引或者切片
    ```Python
    a = np.arange(24).reshape((2,3,4))
    a[1,:] # 取第一行的所有列  
    a[:,1] # 取所有行的第一列
    ```  

    * 布尔型索引  

        ```Python
        names = np.array(['Bob','Joe','Will','Bob','Will','Joe','Joe'])
        data = np.arange(28).reshape(7,4)

        # 先对姓名做比较运算产生布尔型数组
        names == 'Bob' 
        # Out: array([ True, False, False,  True, False, False, False], dtype=bool)

        # 这个布尔型数组可之间用于索引
        data[names == 'Bob']
        #Out: 
        array([[ 0,  1,  2,  3],
            [12, 13, 14, 15]]) 

        # 可以将布尔型数组跟切片、整数、整数序列混合试用
        data[names == 'Bob',2:]
        # Out: 
        array([[ 2,  3],
            [14, 15]])

        ```  

    * 逻辑运算  

      组合条件,逻辑运算符: ：& 且，| 或，非（!= 或 ~） 
        ```python 
        mask = (names == 'Bob') | (names == 'Will')
        data[mask]

        # 非
        data[names != 'Bob']
        data[~(names == 'Bob')]
        ```  

    * 返回指定条件元素所在位置索引  


        ```python
        a = np.array([[1,2,3,4],[5,4,7,8]])
        b = np.array([[[1,2,3,4],[5,4,7,8]],[[1,4,3,2],[5,2,1,8]]])

        a > 2
        # Out[91]:
        array([[False, False,  True,  True],
            [ True,  True,  True,  True]], dtype=boo
        np.where(a > 2)
        # Out[92]: 
        # (array([0, 0, 1, 1, 1, 1], dtype=int64),array([2, 3, 0, 1, 2, 3], dtype=int64))
        np.where(a > 4)
        # (array([1, 1, 1], dtype=int64), array([0, 2, 3], dtype=int64))
        np.where(b == 4)
        # Out[94]: (array([0, 0, 1], dtype=int64),array([0, 1, 0], dtype=int64),array([3, 1, 1], dtype=int64))  

        ```

    >  np.where 函数也是三元表达式 x if condition else y的矢量化版本  
    > result = np.where(cond,xarr,yarr)  
    > 当符合条件时是x，不符合是y，常用于根据一个数组产生另一个新的数组   

* 增加
    > np.concatenate((a1,a2,...), axis=0)，ndarray数组拼接    



    ```Python
    a = np.array([[1,2,3],[4,5,6],[7,8,9]])
    b = np.array([[10,11,12]])

    np.concatenate((a,b),axis=0) #按行拼接
    np.concatenate((a,b.T),axis=1) # 按列拼接

    ```   

* 修改  
    > 使用切片或者索引进行选中后来进行修改

* 删除  
    > np.delete(arr, obj, axis=None)   


    ```python  
    a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
    a

    #删除行
    b1 = np.delete(a, -1, axis=0) 
    #删除列
    b2 = np.delete(a, -1, axis=1)  
    #删除多列
    b3 = np.delete(a, [1,2], axis=1)
    ```  

## Numpy统计  
> 大部分函数都有参数axis = None 默认不输入此参数对数据进行计算,设定轴可以对此轴上的数据进行计算   

*  常用的统计函数  


    * .sum(a,axis=None)：数组a求和运算，根据给定轴axis计算数组a相关元素之和，axis整数或元组，轴、维度可以指定
    * .mean(a,axis=None)：根据给定轴axis计算数组a相关元素的期望（算数平均数），axis整数或元组
    average(a,axis=None,weights=None)：根据给定轴axis计算数组a相关元素的加权平均值，weights加权值，不设为等权重
    * .var(a,axis=None)：根据给定轴axis计算数组a相关元素的方差，方差：各数与平均数之差的平方的平均数
    * .std(a,axis=None)：根据给定轴axis计算数组a相关元素的标准差，标准差:方差平方根

* 其他统计函数  
    
    * .min(a,axis=None) max(a,axis=None)：计算数组a中元素的最小值、最大值
    * .argmin(a,axis=None) argmax(a,axis=None)：计算数组a中元素最小值、最大值的降1维后下标（寻找某元素，得到它的 一维扁平化 后的位置）
    * .ptp(a,axis=None)：计算数组a中元素最大值与最小值的差
    * .median(a,axis=None)：计算数组a中元素的中位数（中值）

## Numpy 数据的存取

* 存  
    > np.save('文件名.npy', 变量) # **变量** 存为.npy文件  
    > np.savez("y.npz", ar0 = a, ar1 = b)  # 多个数组存入一个.npz压缩包
    c = np.load('x.npy') # 读取数据 无论是压缩包还是数组都用这一个接口
* Numpy 存储csv文件  
    将ndarray 数组写到csv文件中:
    > np.savetxt(frame,array,fmt='%.18e',delimiter=None)  
    
    * frame 存储文件、字符串或产生器的名字，可以是.gz或.bz2的压缩文件，对大型数据有用，压缩后存储或读取，节省存储资源
    * array 存入文件的数组
    * fmt 写入文件中每个元素的字符串格式，例如
        * %s (ASCII字符)
        * %d （整数）
        * %.2f（2位小数的浮点数）
        * %.18e（科学计数法，常用）
            * np各类型元素存储到CSV中都是字符串，字符串显示的格式，默认%.18e，科学计数法，保留18位小数的浮点数形式存储数据，需要根据情况修改
    * delimiter 分隔字符串，默认是任何空格，需要改为 逗号  


   ```Python
    a = np.arange(100).reshape(5,20)
    np.savetxt('a.csv',a,fmt='%d',delimiter=',') #整数
    np.savetxt('a.csv',a,fmt='%.1f',delimiter=',') #一位小数的浮点数

    b = np.array([['a','b','c','d'],['11','12','13','14']])
    np.savetxt('b.csv',b,fmt='%s',delimiter=',') #ASCII字符，不能存储非ASCII字符串  
    ```  

## numpy 读取csv文件   

> np.loadtxt(frame,dtype=np.float,delimiter=None skiprows=0,usecols=None,unpack=False)  

* frame 文件、字符串或产生器，可以是.gz或bz2压缩文件
* dtype 数据类型，可选，CSV的字符串以什么数据类型读入数组中，默认np.float 浮点数
* delimiter 分隔字符串，默认是任何空格，改为 逗号
* skiprows 跳过前x行，一般跳过第一行表头
* usecols 读取指定的列，索引，元组类型  
* unpack 如果True，读入属性将分别写入不同数组变量，False 读入数据只写入一个数组变量，默认False

```python  
b = np.loadtxt('a.csv', delimiter=',') # 默认浮点型
b = np.loadtxt('a.csv', dtype=np.int, delimiter=',') #数据为整型

#b = np.loadtxt('a.txt', dtype=np.str, delimiter=',') #数据为字符串，输出默认带 b，要去掉用下面方式输出：
b = np.loadtxt('a.txt', dtype=bytes, delimiter=',').astype(str)
b = np.loadtxt('a.txt', dtype=bytes, delimiter=',',skiprows=1,usecols=(2,3)).astype(str) #跳过第一行，读入第3、4列  
```   
