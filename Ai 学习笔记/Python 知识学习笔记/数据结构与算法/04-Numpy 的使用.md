# Numpy 基础

&emsp;&emsp;Numpy 是 Python 中科学计算的基础包，它是一个 Python 库，提供多维数组对象、各种派生对象（例如：掩码数组和矩阵）以及用于对数组进行快速操作的各种方法（包括：数学、逻辑、形状操作、排序、选择、I/O 、离散傅里叶变换、基本线性代数、基本统计运算、随机模拟等）。

&emsp;&emsp;numpy的部分功能如下：

+ ndarray 一个具有矢量算术运算和复杂广播能力的快速且节省空间的多维数组
+ 用于对整组数据进行快速运算的标准数学函数（无需编写循环）
+ 用于读写磁盘数据的工具以及用于操作内存映射文件的工具
+ 线性代数、随机数生成以及傅里叶变换功能
+ 用于集成由 C、C++、Fortran 等语言编写的代码的 API

&emsp;&emsp;大多数 Numpy 数组都有一些限制：

+ 数组的所有元素必须具有相同的数据类型
+ 一旦创建，数组的总大小就不能改变
+ 形状必须是 "矩形" 而不是 "锯齿状"，例如二维数组的每一行必须具有相同的列数

&emsp;&emsp;可以通过 `conda install numpy` 命令下载安装：

```python
import numpy as np # 导入 numpy  
# 创建一个二维数组  
a = np.array([[1, 2, 3], [4, 5, 6]])  
print(a)  
print(a.ndim) # 维度  
print(a.shape) # 形状  
print(a.size) # 元素个数  
print(a.dtype) # 数据类型  
print(a.itemsize)  # 每个元素字节大小
```

# ndarray 的创建方式

<font color=orachid>**array() 与 asarray()**</font>

+ array()：将输入数据转换为 ndarray，会进行复制
+ asarray()：将输入数据转换为 ndarray，如果输入本身是 ndarray 则不会进行复制

```python
import numpy as np # 导入 numpy  
data = [1, 2, 3]  
print(f"元数据的地址为：{id(data)}")  
  
print("-" * 20)  
arr1 = np.array(data)  
print(f"arr1 地址为：{id(arr1)}")  
print(f"arr1 的数据为：{arr1}")  
  
print("-" * 20)  
arr2 = np.array(arr1)  
print(f"arr1 地址为：{id(arr2)}")  
print(f"arr1 的数据为：{arr2}")  
  
print("-" * 20)  
arr3 = np.asarray(data)  
print(f"arr3 地址为：{id(arr3)}")  
print(f"arr3 的数据为：{arr3}")  
  
print("-" * 20)  
arr4 = np.asarray(arr3)  
print(f"arr4 地址为：{id(arr4)}")  
print(f"arr4 的数据为：{arr4}")
```

<font color=orachid>**zeros()、ones()、empty()与zeros_like()、ones_like()、empty_like()**</font>

+ zeros()：返回给定形状和类型的新数组并用 0 填充
+ ones()：返回给定形状和类型的新数组并用 1 填充
+ empty()：返回给定形状和类型的未初始化的新数组。

><font color=orange>注意：</font>
>+ `np.empty` 并不保证数组元素被初始化为 0，它只是分配内存空间，数组中的元素值是未初始化的，可能是内存中的任意值。
>+ 上述 3 个方法创建的数组元素类型默认都是 float64

+ zeros_like()：返回与给定数组具有相同形状和类型的 0 新数组
+ ones_like()：返回与给定数组具有相同形状和类型的 1 新数组
+ empty_like()：返回与给定数组具有相同形状和类型的未初始化的新数组

```python
arr1 = np.zeros((2, 5))  
# 注意：这里元素间的分隔符是空格，而不是小数点.  
# [[0. 0. 0. 0. 0.]  
#  [0. 0. 0. 0. 0.]]  
print(arr1)  
  
arr2 = np.ones_like(arr1)  
# [[1. 1. 1. 1. 1.]  
#  [1. 1. 1. 1. 1.]]  
print(arr2)  
  
# [[ 1.79769313e+308 -1.79769313e+308  2.00000000e+000]  
#  [ 2.22044605e-016  2.22507386e-308  4.94065646e-324]]  
arr3 = np.empty((2, 3))  
print(arr3)
```

<font color=orachid>**full() 与 full_like()**</font>

+ full()：返回给定形状和类型的新数组，用指定的值填充
+ full_like()：返回与给定数组具有相同形状和类型的用指定值填充的新数组

```python
arr1 = np.full((2, 3), 6)
# [[6 6 6]
#  [6 6 6]]
print(arr1)

arr2 = np.full_like(arr1, 5)
# [[5 5 5]
#  [5 5 5]]
print(arr2)
```

<font color=orachid>**arange()**</font>

+ arange()：返回在给定范围内用均匀间隔的值填充的一维数组

```python
arr1 = np.arange(0, 10, 2)  
# [0 2 4 6 8]  
print(arr1)
```

<font color=orachid>**linspace() 与 logspace()**</font>

+ linspace()：返回指定范围和元素个数的等差数列，数组元素类型为浮点型
+ logspace()：返回指定指数范围、元素个数、底数的等比数列

```python
arr1 = np.linspace(0, 10, 5)  
# [ 0.   2.5  5.   7.5 10. ]  
print(arr1)  
  
arr2 = np.linspace(0, 10, 5, endpoint=False)  
# [0. 2. 4. 6. 8.]  
print(arr2)
```

+ `endpoint = True`：如果把 0 到 10 看作一条线段，相当于用 5 个点将这条线段分成了 4 段，要计算每段的长度（即相邻元素的间隔），用总长度 `(stop - start) / (num - 1)` ，得到间隔为 `10-0 / 4 = 2.5`。这样从起始点 0 开始，每次加上间隔 2.5 就能依次得到序列中的元素：0、2.5、5、7.5、10 
+ `endpoint = False`：意味着 stop 这个值不包含在生成的序列中，此时 `[start, stop)` 区间相当于一条右端点空心（不包含 stop 这个点）的线段。我们在这条线段上放置 num 个点进行划分，每一个点都会划分出一个新的区间段。比如，放 1 个点会把线段分成 1 段，放 2 个点会分成 2 段，放 num 个点就会分成 num 段，段数就等于点数 num，计算间隔的公式就变为 `(stop - start) / num` 

```python
arr = np.logspace(2, 5, 5, base=2)  
# [ 4.     6.72717132  11.3137085  19.02731384  32.   ]  
print(arr)
```

<font color=orachid>**创建随机数数组**</font>

+ random.rand()：返回给定形状的数组，用 `[0, 1)` 上均匀分布的随机样本填充
+ random.randint()：返回给定形状的数组，用从低位(包含)到高位(不包含)上均匀分布的随机整数填充
+ random.uniform()：返回给定形状的数组，用从低位(包含)到高位(不包含)上均匀分布的随机浮点数填充
+ random.randn()：返回给定形状的数组，用标准正态分布(均值为0，标准差为1)的随机样本填充

```python
arr1 = np.random.rand(2, 3)
# [[0.77112868 0.97415392 0.25668864]
#  [0.49946961 0.23491874 0.40514576]]

arr2 = np.random.randint(0, 10, (2, 3))
# [[7 8 2]
#  [1 2 3]]

arr3 = np.random.uniform(3, 6, (2, 3))
# [[5.69275495 3.84857937 3.2899215 ]
#  [5.32035519 3.7460973  3.33859905]]

arr4 = np.random.randn(2, 3)
# [[-2.03654925 -0.50146561  0.4362483 ]
#  [-1.90585739  0.94797017 -0.77026926]]
```

<font color=orachid>**matrix()**</font>

&emsp;&emsp;matrix 为 ndarray 的子类，只能生成二维的矩阵：

```python
arr1 = np.matrix("1 2; 3 4")
# [[1 2]
#  [3 4]]

arr2 = np.matrix([[1, 2], [3, 4]])
# [[1 2]
#  [3 4]]
```

# ndarray 的数据类型

| **数据类型**                                                   | **类型代码**                         | **说明**                                                                              |
| ---------------------------------------------------------- | -------------------------------- | ----------------------------------------------------------------------------------- |
| bool                                                       | ?                                | 布尔类型                                                                                |
| int8、uint8<br>int16、uint16<br>int32、uint32<br>int64、uint64 | i1,u1<br>i2,u2<br>i4,u4<br>i8,u8 | 有符号、无符号的8位（1字节）整型<br>有符号、无符号的16位（2字节）整型<br>有符号、无符号的32位（4字节）整型<br>有符号、无符号的64位（8字节）整型 |
| float16<br>float32<br>float64                              | f2<br>f4或f<br>f8或d               | 半精度浮点型<br>单精度浮点型<br>双精度浮点型                                                          |
| complex64<br>complex128                                    | c8<br>c16                        | 用两个32位浮点数表示的复数<br>用两个64位浮点数表示的复数                                                    |
&emsp;&emsp;创建数组时可以使用 dtype 参数指定元素类型：

```python
arr1 = np.array([1, 2, 3], dtype=np.float64)
# [1. 2. 3.]

arr2 = np.array([0.2, 2.5, 4.8], dtype="i8")
# [0 2 4]
```

&emsp;&emsp;也可以使用 `ndarray.astype()` 方法转换数组的元素类型：

```python
arr1 = np.array([1, 2, 3], dtype=np.float64)
# [1. 2. 3.]

arr2 = arr1.astype(np.int64)
# [1 2 3]
```

# ndarray 切片和索引

&emsp;&emsp;ndarray 对象的内容可以通过索引或切片来访问和修改，与  Python 中 list 的切片操作一样。可以通过内置的 `slice` 函数或者冒号设置start, stop 及 step 参数进行切片，从原数组中切割出一个新数组：

```python
import numpy as np

arr = np.arange(10)
print(arr)
# [0 1 2 3 4 5 6 7 8 9

#获取索引为2的数据
print(arr[2])
# 2

# 从索引 2开始到索引9(不包含)停止，间隔为2
print(arr[slice(2,9,2)])
# [2 4 6 8]

# 从索引2开始到索引9(不包含)停止，间隔为2
print(arr[2:9:2])
# [2 4 6 8]

# 从索引2开始到最后(不包含)，默认间隔为1
print(arr[2:])
# [2 3 4 5 6 7 8 9]

# 从索引2开始到索引9(不包含)结束，默认间隔为1
print(arr[2:9])
# [2 3 4 5 6 7 8]
```

# Numpy 常用函数

## 基本函数

| **函数**                    | **说明**                                                                                                              |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| np.abs()                  | 元素的绝对值，参数是 number 或 array                                                                                           |
| np.ceil()                 | 向上取整，参数是 number 或 array                                                                                             |
| np.floor()                | 向下取整，参数是 number 或 array                                                                                             |
| np.rint()                 | 四舍五入，参数是 number 或 array                                                                                             |
| np.isnan()                | 判断元素是否为NaN(Not a Number) ，参数是 number 或 array                                                                        |
| np.multiply()             | 元素相乘，参数是 number 或 array。如果第二个参数传递的是number，原数组中所有元素乘以这个数字，返回新的数组；如果第二个参数也是一个数组，是将两个数组中对应位置的元素相乘，返回一个新的数组，其形状与输入数组相同。 |
| np.divide()               | 元素相除，参数是 number 或 array                                                                                             |
| np.where(condition, x, y) | 三元运算符，x if condition else y                                                                                         |
```python
arr = np.random.randn(2, 3)  
#  [[ 0.19821279 -0.60262422 -0.04369977]  
#  [ 1.02966974  0.658359    0.46406156]]  
print("-"*30,"\n",arr)  
# [[0.19821279 0.60262422 0.04369977]  
#  [1.02966974 0.658359   0.46406156]]  
print("-"*30,"\n",np.abs(arr))  
#  [[ 1. -0. -0.]  
#  [ 2.  1.  1.]]  
print("-"*30,"\n",np.ceil(arr))  
#  [[ 0. -1. -1.]  
#  [ 1.  0.  0.]]  
print("-"*30,"\n",np.floor(arr))  
#  [[ 0. -1. -0.]  
#  [ 1.  1.  0.]]  
print("-"*30,"\n",np.rint(arr))  
#  [[False False False]  
#  [False False False]]  
print("-"*30,"\n",np.isnan(arr))  
#  [[ 0.39642558 -1.20524845 -0.08739954]  
#  [ 2.05933948  1.31671799  0.92812313]]  
print("-"*30,"\n",np.multiply(arr,2))  
#  [[1. 1. 1.]  
#  [1. 1. 1.]]  
print("-"*30,"\n",np.divide(arr,arr))  
#  [[1 0 0]  
#  [1 1 1]]  
print("-"*30,"\n",np.where(arr>0,1,0))
```

## 统计函数

| **函数**       | **说明**                    |
| ------------ | ------------------------- |
| np.mean()    | 所有元素的平均值                  |
| np.sum()     | 所有元素的和                    |
| np.max()     | 所有元素的最大值                  |
| np.min()     | 所有元素的最小值                  |
| np.std()     | 所有元素的标准差                  |
| np.var()     | 所有元素的方差                   |
| np.argmax()  | 最大值的下标索引值                 |
| np.argmin()  | 最小值的下标索引值                 |
| np.cumsum()  | 返回一个一维数组，每个元素都是之前所有元素的累加和 |
| np.cumprod() | 返回一个一维数组，每个元素都是之前所有元素的累乘积 |
&emsp;&emsp;多维数组在计算时默认计算全部维度，可以使用 axis 参数指定按某一维度为轴心统计，axis=0 按列统计、axis=1 按行统计：

```python
arr = np.random.randint(1, 5, (2, 3))  
#  [[2 4 2]  
#  [3 3 4]]  
print("-"*30,"\n",arr)  
#  3.0  
print("-"*30,"\n",np.mean(arr))  
# 18  
print("-"*30,"\n",np.sum(arr))  
# 4  
print("-"*30,"\n",np.max(arr))  
# 2  
print("-"*30,"\n",np.min(arr))  
#  0.816496580927726  
print("-"*30,"\n",np.std(arr))  
# 0.6666666666666666  
print("-"*30,"\n",np.var(arr))  
# 1  
print("-"*30,"\n",np.argmax(arr))  
# 0  
print("-"*30,"\n",np.argmin(arr))  
# [ 2  6  8 11 14 18]  
print("-"*30,"\n",np.cumsum(arr))  
#  [  2   8  16  48 144 576]  
print("-"*30,"\n",np.cumprod(arr))  
#  [[ 2  8 16]  
#  [ 3  9 36]]  
print("-"*30,"\n",np.cumprod(arr, axis=1))
```

## 比较函数

| **函数**   | **说明**                |
| -------- | --------------------- |
| np.any() | 至少有一个元素满足指定条件，就返回True |
| np.all() | 所有的元素都满足指定条件，才返回True  |
```python
arr = np.array([1, 2, 3, 4, 5])  
print(np.any(arr > 3)) # True  
print(np.all(arr > 3)) # False
```

## 排序函数

&emsp;&emsp;`ndarray.sort()` 直接修改原数组：

```python
arr = np.random.randint(0, 10, (3, 3))  
# [[3 3 7]  
#  [8 5 5]  
#  [0 1 3]]  
print(arr)  
arr.sort()  
# [[3 3 7]  
#  [5 5 8]  
#  [0 1 3]]  
print(arr)  
arr.sort(axis=0)  
# [[0 1 3]  
#  [3 3 7]  
#  [5 5 8]]  
print(arr)
```

&emsp;&emsp;`axis` 用来指定排序的轴。默认值为 -1，表示沿着最后一个轴进行排序；在二维数组中，axis = 0 表示按列排序，axis = 1 表示按行排序。

&emsp;&emsp;在 NumPy 中，轴是对数组维度的一种抽象描述。对于多维数组，每个维度都对应一个轴，轴的编号从 0 开始。对于二维数组，它有两个轴：

+ 轴 0：代表垂直方向，也就是行的方向。可以把二维数组想象成一个表格，轴 0 就像是表格中从上到下的行索引方向对列数据排序，所以 axis=0 表示按列排序
+ 轴 1：代表水平方向，也就是列的方向。就像是表格中从左到右的列索引方向对行数据进行排序，所以 axis=1 表示按行排序

&emsp;&emsp;`np.sort()` 会返回排序后的副本，即创建新的数组：

```python
arr = np.random.randint(0, 10, (3, 3))    
print(np.sort(arr))
print(arr)
```

## 去重函数

&emsp;&emsp;np.unique() 计算唯一值并返回有序结果：

```python
arr = np.random.randint(0, 5, (3, 3))  
# [[1 3 4]  
#  [1 3 3]  
#  [1 3 3]]  
print(arr)  
# [1 3 4]  
print(np.unique(arr))
```

# 基本运算

&emsp;&emsp;Numpy 中的数组不用编写循环即可执行批量运算，称之为矢量化运算，大小相等的数组之间的任何算术运算都会将运算应用到元素级：

```python
arr1 = np.array([[1, 2, 3], [4, 5, 6]])  
arr2 = np.array([[7, 8, 9], [10, 11, 12]])  
# [[ 8 10 12]  
#  [14 16 18]]  
print(arr1 + arr2)  
# [[-6 -6 -6]  
#  [-6 -6 -6]]  
print(arr1 - arr2)  
# [[ 7 16 27]  
#  [40 55 72]]  
print(arr1 * arr2)  
# [[0.14285714 0.25       0.33333333]  
#  [0.4        0.45454545 0.5       ]]  
print(arr1 / arr2)
```

&emsp;&emsp;数组与标量的算术运算会将标量值传播到各个元素，不同大小的数组之间的运算叫做广播。

```python
arr1 = np.array([[1, 2, 3], [4, 5, 6]])
print(arr1 + 100)
print(arr1 - 100)
print(arr1 * 100)
print(arr1 / 100)
```

&emsp;&emsp;广播机制是 NumPy 中一个强大的特性，它允许在不同形状的数组之间进行元素级运算。广播机制的规则如下：

+ 规则 1：如果俩个数组的维度数不相同，那么小维度数组的形状将会在最左边补 1

```python
import numpy as np

# 一维数组
arr1 = np.array([1, 2, 3])  # 形状为 (3,)
# 二维数组
arr2 = np.array([[4], [5], [6]])  # 形状为 (3, 1)

# 对 arr1 应用规则 1，在其形状最左边补 1，变为 (1, 3)  [[1,2,3]]
# 此时 arr1 形状 (1, 3) 和 arr2 形状 (3, 1) 满足广播条件
result = arr1 + arr2
print("规则 1 示例结果：\n", result)
```

+ 规则 2：如果俩个数组的形状在任何一个维度上都不匹配，那么数组的形状会沿着维度大小（元素个数）为 1 的维度开始扩展 ，（维度必须是1开始）直到所有维度都一样， 以匹配另一个数组的形状

```python
import numpy as np

# 二维数组
arr3 = np.array([[1, 2, 3]])  # 形状为 (1, 3)

# 二维数组
arr4 = np.array([[4], [5], [6]])  # 形状为 (3, 1)

# arr3 沿着第0个维度扩展,将原有的一行数据复制成3行,为 (3, 3)=>[[1,2,3], [1,2,3], [1,2,3]]
# arr4 沿着第1个维度扩展, (3, 3)=>[[4,4,4], [5,5,5], [6,6,6]]
result = arr3 + arr4
print("规则 2 示例结果：\n", result)
```

+ 规则 3：如果俩个数组的形状在任何一个维度上都不匹配，并且没有任何一个维度大小等于1，那么会引发异常

```python
import numpy as np

# 一维数组
arr5 = np.array([1, 2, 3])  # 形状为 (3,)
# 一维数组
arr6 = np.array([4, 5])  # 形状为 (2,)

try:
    result = arr5 + arr6
    print(result)
except ValueError as e:
    print(f"规则 3 示例错误信息：{e}")
```

# 矩阵乘法

&emsp;&emsp;通过运算符和 `np.multiply()` 对两个数组相乘进行的是对位乘法而非矩阵乘法运算：

```python
arr1 = np.array([[1, 2, 3], [4, 5, 6]])
arr2 = np.array([[6, 5, 4], [3, 2, 1]])

print(arr1 * arr2)
print(np.multiply(arr1, arr2))
```

&emsp;&emsp;使用 `np.dot()`、`ndarray.dot()`、`@` 可以进行矩阵乘法运算：

```python
arr1 = np.array([[1, 2, 3], [4, 5, 6]])
arr2 = np.array([[6, 5], [4, 3], [2, 1]])

#对于矩阵乘法来说，要求第一个矩阵的列数等于第二个矩阵的行数
print(arr1)
print(arr2)
print(arr1.shape, arr2.shape)

print(np.dot(arr1, arr2))
print(arr1.dot(arr2))
print(arr1 @ arr2)

# 一个二维数组跟一个大小合适的一维数组的矩阵点积运算之后将会得到一个一维数组
arr3 = np.array([6, 5, 4])
print(arr1 @ arr3)
```

&emsp;&emsp;矩阵乘法的规则是：结果矩阵中第 i 行第 j 列的元素等于第一个矩阵的第 i 行与第二个矩阵的第 j 列对应元素乘积之和。

+ 结果矩阵第一行第一列的元素：计算 arr1 的第一行 `[1, 2, 3]` 与 arr2 的第一列 `[6, 4, 2]` 对应元素乘积之和，即 `1*6 + 2*4 + 3*2 = 6 + 8 + 6 = 20`
+ 结果矩阵第一行第二列的元素：计算 arr1 的第一行 `[1, 2, 3]` 与 arr2 的第二列 `[5, 3, 1]` 对应元素乘积之和，即 `1*5 + 2*3 + 3*1 = 5 + 6 + 3 = 14`
+ 结果矩阵第二行第一列的元素：计算 arr1 的第二行 `[4, 5, 6]` 与 arr2 的第一列 `[6, 4, 2]` 对应元素乘积之和，即 `4*6 + 5*4 + 6*2 = 24 + 20 + 12 = 56`
+ 结果矩阵第二行第二列的元素：计算 arr1 的第二行 `[4, 5, 6]` 与 arr2 的第二列 `[5, 3, 1]` 对应元素乘积之和，即 `4*5 + 5*3 + 6*1 = 20 + 15 + 6 = 41`

&emsp;&emsp;所以手动计算得到的结果矩阵是 `[[20, 14], [56, 41]]`。