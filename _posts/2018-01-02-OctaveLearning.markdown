---
layout:     post
title:      "Common Skill Using Octave"
subtitle:   "Common Usage"
date:       2018-01-02
author:     "Duanrw"
header-img: "img/post-bg-js-module.jpg"
tags:
    - machine_learning
    - course
    - Octave
---

## Common usage for Octave  
1.1 安装教程很简单,具体详情请参见官方网站:  ** [how to install Octave ](https://www.gnu.org/software/octave/ )**   

2.1 基础操作　　
* 行向量 -- 使用空格或者逗号分割
 ```python
 >> v = [1 2 3] % 等效：v = [1, 2, 3]
 v =
    1   2   3
 >> v(2) % 只有一行，所以指定就是列
 ans =  2
 ```
* 列向量 -- 使用分号分割  

   ```python  
   >> v = [1; 2; 3]
   v =
     1
     2
     3
  >> v(2) % 只有一列，所以指定就是行
  ans =  2
   ```  
* 矩阵  
 * 普通矩阵  

   ```Python  
   >>   A = [1, 2; 3, 4] % 一行写完
   A =
      1   2
      3   4
   >> A = [1, 2; % 分行写
   > 3, 4]
   A =
      1   2
      3   4

   ```
 * 创建技巧  
    * a : c --- 从a到c  
    * a : b : c --- 从a，间隔b，到c  
```Python      
  >> A = [1:3; 4:6]
A =
   1   2   3
   4   5   6
>> A = [1:2:5; 2:2:6]
A =
   1   3   5
   2   4   6
```  
* 特殊矩阵  
 * 单位矩阵  
```Python
>> eye(3) % 单位矩阵
ans =
Diagonal Matrix
   1   0   0
   0   1   0
   0   0   1
>> flipud(eye(3))
ans =
Permutation Matrix
   0   0   1
   0   1   0
   1   0   0
```
  * 转置矩阵  
  ```Python  
  A =
   1   2
   3   4
>> A'
ans =
   1   3
   2   4
  ```
 * 逆矩阵  
 ```Python
 A =
   1   2
   3   4
>> pinv(A)
ans =
  -2.00000   1.00000
   1.50000  -0.50000
>> pinv(A) * A
ans =
   1.00000   0.00000
  -0.00000   1.00000
  ```
 * 全1矩阵  
 ```Python  
 >> ones(2, 3)
ans =
   1   1   1
   1   1   1
```
 * 全零矩阵
```Python
>> 0*ones(2, 3)
ans =
   0   0   0
   0   0   0
>> zeros(2, 3)
ans =
   0   0   0
   0   0   0
```
 * 随机矩阵  
 ```Python
 >> rand(1, 3) % 0~1的随机数，1行3列
ans =
   0.99291   0.65946   0.95102
   ```
 * 高斯分布矩阵  
 ```Python  
 >> randn(1, 3) % 高斯分布
ans =
   0.14646   2.02587   1.33266
```

* 查数据  
 * 冒号: 表示所有,当前维度所在的所有的行或者列  
 * 访问元素  
 ```Python
 A =
   1   2   3
   4   5   6
   7   8   9
>> A(2, 2) % 第二行，第二列，先定行再定列
ans =  5
```
  * 访问单行或者列  
  ```python
  >> A(1, :) % 第一行，所有，元素
ans =
   1   2   3
>> A(:, 2) % 第二列，所有，元素
ans =
   2
   5
   8
```

 * 访问多行或者多列   

    ```Python
     >> A(:, [1, 3]) % 第一列和第三列，所有，元素
    ans =

       1   3
       4   6
       7   9

    >> A([1, 3], :) % 第一行和第三行，所有，元素
    ans =

       1   2   3
       7   8   9
    ```
* 合并矩阵  
  * C = [A B] %  把B按列加到A上,生成C  
  * C = [A ; B]  % 把B按照行加到A上,生成C  

* 增加元素  
  * 添加元素  
  ```Python
    >> A = [1]
    A =  1
    >> A = [A, 2] % 把2，按列，添加到A上，再赋给A
    A =
       1   2
    >> A = [A; 3] % A有两列，3只有一列，规模不匹配
    error: vertical dimensions mismatch (1x2 vs 1x1)
  ```
  * 添加行或者列  
  ```Python
    >> A = [A; [3, 4]] % 将向量[3, 4]，作为行，添加到A上，在赋给A
    A =
       1   2
       3   4
    >> A = [A, [5; 6]] % 将向量[5, 6]，作为列，添加到A上，在赋给A
    A =
       1   2   5
       3   4   6
  ```

  * 矩阵连接    

    ```Python
      >> A = [1, 2; 3, 4]
      A =
         1   2
         3   4
      >> B = [5, 6; 7 ,8]
      B =
         5   6
         7   8
      >> [A; B] % 将B，作为行，添加到A上
      ans =
         1   2
         3   4
         5   6
         7   8
      所有的数据放入一个向量中
      >> A
      A =
         1   2
         3   4
      >> A(:)
      ans =
         1
         3
         2
         4
      >> A(:)'
      ans =
         1   3   2   4
    ```
* 赋值  
  * 在访问的基础上,给定相同规模的数据  
  ```Python
    A =
       1   2   3
       4   5   6
       7   8   9
    >> A(3, 3) = 10 % 修改单个元素的值
    A =
        1    2    3
        4    5    6
        7    8   10
    >> A(1, :) = [0, 0, 0] % 修改一行的值
    A =
        0    0    0
        4    5    6
        7    8   10
    >> A(2:3, 2:3)
    ans =
        5    6
        8   10
    >> A(2:3, 2:3) = [0, 0; 0, 0] % 修改指定矩阵的值
    A =
       0   0   0
       4   0   0
       7   0   0
  ```  

---

* 运算  
 * 加减  
 ```Python
    >> A = [1 1; 1 1]
    A =
    1   1
    1   1
    >> B = [2 2; 2 2]
    B =
    2   2
    2   2
    >> A + B
    ans =
    3   3
    3   3
    >> A - B
    ans =
    -1  -1
    -1  -1
    >> A - 1
    ans =
    0  0
    0  0
 ```

 * 乘  
  ```Python
      >> A = [1 2; 3 4]
      A =

         1   2
         3   4

      >> B = [5 6; 7 8]
      B =

         5   6
         7   8

      >> A * B
      ans =

         19   22
         43   50

      >> -A % -1 * A
      ans =

        -1  -2
        -3  -4
  ```

  * 点运算
   * 对应元素运算
    * 维度相同：对应元素相乘
    * 行维度相同：每行对应元素相乘
    * 列维度相同：每列对应元素相乘
    * A .* B = B .* A
    * 乘 *；除 /; 平方 ^；
  * 维度相同
  ```Python
  A =
     1   1
     1   1
  B =
     2   3
     2   3
  >> A .* B
  ans =
     2   3
     2   3
  ```

  * 行维度相同
  ```Python
    A =
       1   1
       1   1
    B =
       5   6
    >> A .* B
    ans =
       5   6
       5   6  
  ```
 * 列维度相同  
 ```Python
  A =
     1   1
     1   1
  B =
     5
     6
  >> A .* B
  ans =
     5   5
     6   6
 ```
 * 点除  
  * 矩阵乘以常数，A * 2，除可以是，A / 2  
  * 反过来，2 * A没问题，2 / A 就不行，要用2 ./ A  
  ```Python
  A =
     1   2
     3   4
  >> 1 ./ A
  ans =
     1.00000   0.50000
     0.33333   0.25000
  ```  
* 逻辑
  * 每个元素作比较,标记为0或者1
  * <,  >,  ==, !=(或者~=),  &&,  ||
* 控制语句  
  * if
  ```python
  i =  1
  >> if i == 1
  >     disp(1)
  >  elseif i == 2
  >     disp(2)
  >  else
  >     disp(3)
  >  end
  ```  
  * for
  ```python
    >> for i = 1:3 % 从1到3
    >     disp(i)
    >  end
     1
     2
     3
  ```
  * while   
  ```python  
    >> while i <= 3
    >     disp(i)
    >     i = i + 1
    >  end
     1
     2
     3
  ```
  * break continue 和其他语言没区别

---

* 函数  

  * size：获取矩阵维度
  * length：获取最大维度
  * who：变量列表
  * whos：变量详情
  * clear 变量名：删除指定变量
  * clear：删除所有变量
  * find：返回符合条件元素的下标
  * log：log以e为底
  * exp：e的多少次方
  * abs：绝对值
  * floor：向下取整
  * ceil：向上取整
  * sum：求和
  * prop：求积
  * max :
    * max(A) 单个向量则返回最大的值
    * max(A, B ) 保留较大的那个
    * max(A,[],1) % 每列的最大值
    * max(A,[],2) % 每行的最大值
  * randperm  生成乱序序列   
      ```python
      >> A = [2, 3, 4 ,5 ,6]
      A =

         2   3   4   5   6
      >> rand_indices = randperm(length(A))
      rand_indices =

         1   2   4   5   3
      >> A(:, rand_indices(1:3))
      ans =

         5   6   2

      ```

---  
绘图

* plot

    * LineWidth：线宽
    * MarkerFaceColor：标记颜色
    * MarkerSize：标记大小

  ```python
  t = [0:0.1:0.98];
  y1 = sin(2*pi*4*t);
  plot(t,y1);
  y2 = cos(2*pi*4t);

  hold on ;  %  使用 hold on 可以将接下来绘制的函数放在一张图里

  plot(t, y2, 'r'); % 第三个参数，表示颜色

  xlabel('time');
  ylabel('value');  %  标记出x和y轴

  legend('sin','cos') %  在图像右上角，用来区分多个函数的图例，依次表示两个曲线所表示的内容

  tiitle('my plot') %  在图像的正上方给图像加上标题

  print -dpng 'myPlot.png'  % 输出图像,保存在'pwd'目录下

  close % 关闭图像

  figure(1); plot(t,y1); % 新开第一个窗口绘制y1
  figure(2); plot(t,y1); % 新开第二个窗口绘制y2

  % pos为向量中为1的行序号组成的向量，neg为向量中为0的行序号组成的向量
  plot(X(pos,1),X(pos,2),'k+','LineWidth',2,'MarkerSize',7);
  plot(X(neg,1),X(neg,2),'ko','markerFaceColor','y','MarkerSize',7);

  ```

其他属性  
  ```Python
  Format arguments:
     linestyle
           '-'  Use solid lines (default).
           '--' Use dashed lines.`
           ':'  Use dotted lines.
           '-.' Use dash-dotted lines.
     markerstyle
           '+'  crosshair
           'o'  circle
           '*'  star
           '.'  point
           'x'  cross
           's'  square
           'd'  diamond
           '^'  upward-facing triangle
           'v'  downward-facing triangle
           '>'  right-facing triangle
           '<'  left-facing triangle
           'p'  pentagram
           'h'  hexagram
     color
           'k'  blacK
           'r'  Red
           'g'  Green
           'b'  Blue
           'm'  Magenta
           'c'  Cyan
           'w'  White

  常用的修改属性有：
  "linestyle",
  "linewidth",
  "color",
  "marker",
  "markersize",
  "markeredgecolor",
  "markerfacecolor".

  ```

* 直方图

  ```Python
  >> w = -6 + sqrt(10)*randn(1, 10000);
  >> hist(w)
  >> hist(w, 30) % 30组
  ```





---
* 其他
  * 格式化输出  
  ```Python
  disp(pi) % 输出：3.1416
  disp(sprintf('pi is %.2f', pi)) % c语言风格
  ```
  * 帮助
  ```Python
  help size % 查看size函数的帮助文档
  help help % 查看help函数的帮助文档
  ```
