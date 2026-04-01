# Python Numpy 入门

> 来源：[https://v11enp9ok1h.feishu.cn/docx/ES3Fd0CGgo2FFAxo2kocEtJanZe](https://v11enp9ok1h.feishu.cn/docx/ES3Fd0CGgo2FFAxo2kocEtJanZe)

# Numpy 简介

NumPy(Numerical Python) 是 Python 语言的一个扩展程序库，支持大量的维度数组与矩阵运算，此外也针对数组运算提供大量的数学函数库。

NumPy 是一个运行速度非常快的数学库，主要用于数组计算，包含：

+   一个强大的N维数组对象 ndarray

+   广播功能函数

+   整合 C/C++/Fortran 代码的工具

+   线性代数、傅里叶变换、随机数生成等功能

在安装好python3，有pip后，可一行命令直接安装numpy

pip install numpy

# Numpy 基本操作

## 0. 库导入

在导入numpy包时，可以这样写python中的import函数：

```
**import** numpy **as** np
```

为了更好的代码可读性，一般在import时加以一个别名np。这是一个被广泛采用的约定，这样您的代码对所**有人都更可读**。因此，建议在**导入**时始终使用import numpy as np。

## nparray对象

数组是下标（index）和值（value）组成的序对的集合。在数组中，每个有定义的下标都与一个值对应，这个值称作数组元素。

在深度学习中，大量的数据是由多维数组存储的。nparray就是一个存储多维数据的解决方法，也是numpy中最重要的一个概念。

### 1.1 创建第一个nparray

我们可以用np.array()函数初始化一个n维数组：只需传入一个或多个python列表即可。

```
import numpy as np 

# 一维  
a = np.array([1,2,3])  
print (a) # [1,2,3]

# 多于一个维度，这里以二维举例
b = np.array([[1,  2],  [3,  4]])  
print (b)
# [[1  2] 
#  [3  4]]
```

当然，您也可以使用np.asarray()从python列表、元组获得nparray；

甚至从torch.tensor中转换到nparray！

```
import numpy as np 

x = [1,2,3] 
y = (1,2,3) 
z = torch.tensor([1, 2, 3])

a = np.asarray(x)   # 从列表转换到 nparray
b = np.asarray(y)   # 从元组转换到 nparray
c = z.cpu().numpy() # numpy仅支持cpu，因而如果tensor在gpu需先转至cpu。
print(a, b, c)

# a/b/c的输出都是
# [1,2,3]
```

### 1.2 创建特殊数组（全零、全一等）

+   **numpy**.empty 函数：创建指定形状的数组，不初始化。

+   **numpy.zeros **函数：创建指定形状的数组，数组元素以 0 来填充。

+   **numpy.ones 函**数：创建指定形状的数组，数组元素以 1 来填充。

+   **numpy.ey**e 函数：创建一个矩阵数组，对角线元素为 1，其他位置为零。

+   **numpy.zeros_li**ke 函数：创建一个与给定数组具有相同形状的数组，数组元素以 0 来填充。

+   **numpy.ones_lik**e 函数：创建一个与给定数组具有相同形状的数组，数组元素以 1 来填充。

代码示例：

```
import numpy as np

x = np.empty([3,2])
print(x)
# [[6.23042070e-307, 4.67296746e-307],
#  [1.69121096e-306, 9.34601642e-307],
#  [2.22523004e-307, 1.78020440e-306]]

# 默认为浮点数
y = np.zeros([1,5]) 
print(y) # [0. 0. 0. 0. 0.]
z = np.ones([1,5]) 
print(z) # [1. 1. 1. 1. 1.]
u = np.eye(3,4)
print(u)
# [[1. 0. 0. 0.]
#  [0. 1. 0. 0.]
#  [0. 0. 1. 0.]]

# 创建一个 3x3 的二维数组
arr = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
zeros_arr = np.zeros_like(arr)
print(zeros_arr)
# [[0 0 0]
#  [0 0 0]
#  [0 0 0]]

ones_arr = np.ones_like(arr)
print(ones_arr)
# [[1 1 1]
#  [1 1 1]
#  [1 1 1]]
```

### 1.3 数据类型

数据类型也就是n维数组中，数组元素在计算机中存储的格式。

常用的有如下类型：

|  | 整数 | 无符号整数 | 浮点数 |
| --- | --- | --- | --- |
| 8位 | int8 | uint8 |  |
| 16位 | int16 | uint16 | float16 |
| 32位 | int32 | uint32 | float32 |
| 64位 | int64 | uint64 | float64 |

简单来说，位数越多，能表达的数的范围就越广。

运算效率上uint>int>float，但精度上uint<int<float。

[了解数据](https://www.runoob.com/numpy/numpy-dtype.html)类型的更为详细的知识，可以参考拓展阅读。

代码中，可以像这样查询和变更nparray的数据类型：

```
import numpy as np

arr = np.array([10, 20, 30])
print(arr.dtype) # int32

# 用astype函数变更nparray的数据类型。
arr = arr.astype(np.float32)
print(arr.dtype) # float32
```

### 1.4 数组属性

常用的数组属性有：

代码示例：

```
import numpy as np

a = np.arange(24)
print(a.ndim) # 1
print(a.shape) # (24, )
print(a.size) # 24

b = a.reshape(2, 3, 4)
print(b.ndim) # 3
print(b.shape) # (2, 3, 4)
print(b.size) # 24
```

## 基础切片和索引

您可以和python列表一样对nparray进行切片和索引。

```
import numpy **a**s np

a = np.arange(10) # [0 1 2 3 4 5 6 7 8 9]
print(a[2:]) # [2 3 4 5 6 7 8 9]
print(a[2:5]) # [2 3 4]

# 切片还可以包括省略号 …，来使选择元组的长度与数组的维度相同。
# 如果在行位置使用省略号，它将返回包含行中元素的 ndarray。
b = np.array([[1,2,3],[3,4,5],[4,5,6]])  
print(b[...,1])   # 第2列元素 [2 4 5]
print(b[1,...])   # 第2行元素 [3 4 5]
print(b[...,1:])  # 第2列及剩下的所有元素
# [[2 3]
#  [4 5]
#  [5 6]]
```

## 常用数组操作

+   numpy.reshape 函数：可以在不改变数据的条件下修改形状

+   numpy.transpose 函数：用于对换数组的维度

+   numpy.squeeze 函数：从给定数组的形状中删除一维的条目

```
import numpy as np 
a = np.arange(10) # [0 1 2 3 4 5 6 7 8 9]

# reshape 函数可以在不改变数据的条件下修改nparray的形状
b = a.reshape(2, 5)
print(b)
# [[0 1 2 3 4]
#  [5 6 7 8 9]]

# transpose 函数对换数组的维度，也就是转置矩阵。
c = np.transpose(b)
print(c)
# [[0 1]
#  [2 3]
#  [4 5]
#  [6 7]
#  [8 9]]

d = a.reshape(1, 2, 5)
print(d.shape) # (1, 2, 5)

d = d.squeeze()
print(d.shape) # (2, 5)

```

## 统计函数/条件筛选

### 4.1 最大最小值（索引）

+   **nump**y**.am**in() 函数：计算数组沿指定轴的最小值。

+   **nump**y**.am**ax() 函数：计算数组沿指定轴的最大值。

+   **numpy**.ar**gm**in() 函数：沿给定轴最小元素的索引。

+   **numpy**.ar**gm**ax() 函数：沿给定轴最大元素的索引。

```
import numpy as np 
 
a = np.array([[3,7,5],[8,4,3],[2,4,9]])  
print(a)
# [[3 7 5]
#  [8 4 3]
#  [2 4 9]]

print(np.amin(a, 1)) # [3 3 2]
print(np.amin(a, 0)) # [2 4 3]
print(np.amax(a))    # 9
print(np.amax(a, 0)) # [8 7 9]

print(np.argmin(a, 1)) # [0 2 0]
print(np.argmin(a, 0)) # [2 1 1]
print(np.argmax(a))    # 8
print(np.argmax(a, 0)) # [1 0 2]
```

### 4.2 条件筛选

+   **nump**y**.n**onzero() 函数：返回输入数组中非零元素的索引。

+   **numpy.whe**r**e(**) 函数：返回输入数组中满足给定条件的元素的索引。

```
import numpy as np

a = np.array([[30,40,0],[0,20,10],[50,0,60]])  
print(a)
# [[30 40  0]
#  [ 0 20 10]
#  [50  0 60]]
print(a.nonzero())
# (array([0, 0, 1, 1, 2, 2]), array([0, 1, 1, 2, 0, 2]))


# 筛选a中大于30的元素的索引
idx = np.where(a &gt; 30)
print(idx)
# (array([0, 2, 2]), array([1, 0, 2]))
# 使用这些索引来获取满足条件的元素：
print(a[idx])
# [40 50 60]
```

## 数学运算/位运算

### 5.1 三角函数/反三角函数

+   np.sin()、np.cos()、np.tan() 函数：正弦、余弦、正切。

+   np.arcsin()、np.arccos()、np.arctan() 函数：反正弦、反余弦、反正切。

```
import numpy as np

a = np.array([0,30,45,60,90])
sin_value = np.sin(a*np.pi/180) # 换算成弧度
print(sin_value) 
# [0.         0.5        0.70710678 0.8660254  1.        ]
print(np.arcsin(sin_value)*180/np.pi)
# [ 0. 30. 45. 60. 90.]

cos_value = np.cos(a*np.pi/180) # 换算成弧度
print(cos_value)
# [1.00000000e+00 8.66025404e-01 7.07106781e-01 5.00000000e-01 6.12323400e-17]
print(np.arcsin(cos_value)*180/np.pi)
# [ 0. 30. 45. 60. 90.]

tan_value = np.tan(a*np.pi/180) # 换算成弧度
print(tan_value)
# [0.00000000e+00 5.77350269e-01 1.00000000e+00 1.73205081e+00 1.63312394e+16]
print(np.arcsin(tan_value)*180/np.pi)
# [ 0. 30. 45. 60. 90.]

```

### 5.2 舍入函数

+   numpy.around() 函数：返回指定数字的四舍五入值。

+   numpy.floor() 函数：**返回小于**或者等于指定表达式的最大整数，即向下取整。

+   numpy.ceil() 函数：**返回大于**或者等于指定表达式的最小整数，即向上取整。

### 5.3 位运算

位运算是一种在二进制数字的位级别上进行操作的一类运算，它们直接操作二进制数字的各个位，而不考虑数字的整体值。

位运算在计算机科学中广泛应用于优化和处理底层数据。

常见的位运算有：

1.  **与运算（&）：** 对应位上的两个数字都为1时，结果为1；否则，结果为0。

例如：1010 & 1100 = 1000

对应numpy函数：np.bitwise_and()

1.  **或运算（|）：** 对应位上的两个数字有一个为1时，结果为1；否则，结果为0。

例如：1010 | 1100 = 1110

对应numpy函数：np.bitwise_or()

1.  **异或运算（^）：** 对应位上的两个数字相异时，结果为1；相同时，结果为0。

例如：1010 ^ 1100 = 0110

对应numpy函数：np.bitwise_xor()

1.  **取反运算（~）：** 对数字的每个位取反，即0变为1，1变为0。

例如：~1010 = 0101

对应numpy函数：np.bitwise_not(), np.invert()

1.  **左移运算（<<）：** 将数字的所有位向左移动指定的位数，右侧用0填充。

例如：1010 << 2 = 101000

对应numpy函数：np.left_shift()

1.  **右移运算（>>）：** 将数字的所有位向右移动指定的位数，左侧根据符号位或补零。

例如：1010 >> 2 = 0010

对应numpy函数：np.right_shift()

代码示例：

```
import numpy as np

arr1 = np.array([True, False, True], dtype=bool)
arr2 = np.array([False, True, False], dtype=bool)

result_and = np.bitwise_and(arr1, arr2)
result_or = np.bitwise_or(arr1, arr2)
result_xor = np.bitwise_xor(arr1, arr2)
result_not = np.bitwise_not(arr1)

print(&quot;AND:&quot;, result_and)  # [False, False, False]
print(&quot;OR:&quot;, result_or)    # [True, True, True]
print(&quot;XOR:&quot;, result_xor)  # [True, True, True]
print(&quot;NOT:&quot;, result_not)  # [False, True, False]

# 按位取反
arr_invert = np.invert(np.array([1, 2], dtype=np.int8))
print(&quot;Invert:&quot;, arr_invert)  # [-2, -3]

# 左移位运算
arr_left_shift = np.left_shift(5, 2)
print(&quot;Left Shift:&quot;, arr_left_shift)  # 20

# 右移位运算
arr_right_shift = np.right_shift(10, 1)
print(&quot;Right Shift:&quot;, arr_right_shift)  # 5
```

## 矩阵运算/线性代数

+   numpy.dot() 函数：

+   对于两个一维的数组，计算的是这两个数组对应下标元素的乘积和(数学上称之为向量点积)；

+   对于二维数组，计算的是两个数组的矩阵乘积；

+   对**于多维数组，它的通用计算公式如下，即结果数组中的每**个元素都是：数组a的最后一维上的所有元素与数组b的倒数第二位上的所有元素的乘积和： dot(a, b)[i,j,k,m] = sum(a[i,j,:] * b[k,:,m])。

+   numpy.vdot() 函数：是两个数组的点积。

+   numpy.matmul() 函数：返回两个数组的矩阵乘积。

+   虽然它返回二维数组的正常乘积，但如果任一参数的维数大于2，则将其视为存在于最后两个索引的矩阵的栈，并进行相应广播。

+   另一方面，如果任一参数是一维数组，则通过在其维度上附加 1 来将其提升为矩阵，并在乘法之后被去除。

+   对于二维数组，它就是矩阵乘法：

+   numpy.linalg.det() 函数：计算输入矩阵的行列式。

+   numpy.linalg.inv() 函数：计算矩阵的乘法逆矩阵。

```
import numpy as np

a = np.array([[1,2],[3,4]])
b = np.array([[11,12],[13,14]])
print(np.dot(a, b))
# [[1*11+2*13, 1*12+2*14],[3*11+4*13, 3*12+4*14]]
# [[37  40] 
#  [85  92]]

print(np.matmul(a, b)) # 同上
print(np.vdot(a, b)) # 130
# 1*11 + 2*12 + 3*13 + 4*14 = 130

print(np.linalg.det(a)) # -2.0
print(np.linalg.inv(a))
# [[-2.   1. ]
#  [ 1.5 -0.5]]
```

# 菜鸟教程:NumPy

<https://www.runoob.com/numpy/numpy-tutorial.html>

# Numpy 1.26 User Guide

<https://numpy.org/doc/1.26/user/index.html>