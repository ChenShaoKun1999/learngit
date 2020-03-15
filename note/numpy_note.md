numerical python

# 数据类型

| type              | description                                |
| ----------------- | ------------------------------------------ |
| int8/16/32/64     | 有符号x位整数                              |
| uint8/16/32/64    | 无符号                                     |
| float32           | 单精度浮点数（和C的float兼容）             |
| float64           | 双精度浮点数（和C的double，py的float兼容） |
| float128          | 扩展精度浮点数                             |
| complex64/128/256 | 实虚部分别用x/2位浮点数表示的复数          |

另外还有默认数据类型`int_`, `float_`, `complex_`

# ndarrray

N维定长数组，表示一个标量/矢量/矩阵/张量
要求元素数据类型统一，可以进行优化过的矩阵操作

## indexing

```
index
    一维同list；高维数组可以用类似array[1, -1]的方式引用
bool indexing
    用一个同shape布尔数组来索引，e.g.
    bool_arr = [True, False, True]
    array = [0, 1, 2]
    array[bool] = [0, 2]
    获取布尔数组的方法例子：bool_arr = (array>=1)
fancy indexing
    用一个array-like object进行索引， e.g.
    array[[0, 2, 4]]            #第0，2，4行
    array[[[i1,i2],[j1,j2]]]    #(i1, j1)和(i2, j2)元素
    用ndarray做fancy indexing好像会有奇怪的错误
slice
   切片的部分不会被复制！
    一维的切片同list，多维数组可以分别切片，e.g. array[0:5, ::-1]
```

## 初始化

```python
#一般的
array(object, dtype=None)
   返回根据object(array-like)构建的ndarray
asarray(object, dtype=None)
    和array差不多（少几个可选参数）
    当object为ndarray时，不复制，返回原对象

#一维的
arange([start, ]stop[, step], dtype=None)
    和range函数效果相同，不过返回的是ndarray
linspace(start, stop, num=50, endpoint=True, dtype=None)
    返回等差数列列表
    start, stop     序列起止点
    num             样本点个数
    endpoint        若endpoint=True，则把stop作为最后一个元素，公差为Δ/num
                    否则，不包含stop，公差为Δ/(num-1)
logspace(start, stop, num=50, endpoint=True, base=10.0, dtype=None)
    返回以base^start为首项的等比数列列表
    start, stop     float, 序列起止点

#在整个array填充指定值
ones(shape, dtype=None, order='C')
    返回指定形状的填满1的数组
ones_like(a, dtype=None, order='K')
    返回形状和a相同的填满1的数组
zeros       同ones，填0
zeros_like  同ones_like，填0
empty       同ones，不初始化
empty_like  同ones_like，不初始化
full        同ones，shape后面要一个fill_value，用它填满
full_like   同ones_like，a后面要一个fill_value，用它填满

#单位矩阵
eye(N, M=None, k=0, dtype=<class 'float'>, order='C')
    在指定“对角线”填1，其他填0
    N, M    行列数
    k       k=0为主对角线，正值上移，负值下移
identity(n, dtype=None)
    返回指定大小单位方阵

#特殊的
>>> arr = np.arange(0, 5)
>>> gt = arr>3
array([False, False, False, False,  True])
```

## 属性

    shape       ndarray的形状，即有几个维度、每个维度有多长
                ()          0维数组（标量）
                (5,)        长度为5的一维数组
                (3, 8, 8)   容量为3*8*8的三维数组
    dtype       数据类型
## 方法

    reshape(shape, order='C')
        修改ndarray的shape，但是总长度不能变
        shape参数可以装在tuple里，也可以写成多个参数
    astype(dtype, order, casting, subok, copy)
        返回一个数组，内容为类型转换后的self
        即使dtype和self的类型相同，同样会返回一个拷贝
    a.copy(order='C')
        返回自身的一个拷贝
## 运算

### 常量运算

常量与ndarray运算，两个shape相同的nearray进行运算，或者数学函数作用于ndarray，相当于其中的逐个元素进行运算

```python
>>> import numpy as np
>>> arr1 = np.array([1, 2, 3, 4, 5])
>>> arr2 = arr1**2
>>> arr2
array([ 1,  4,  9, 16, 25], dtype=int32)
>>>
>>> arr1 * arr2		#注意两个array相乘是逐个元素相乘，而不是矩阵乘法
array([  1,   8,  27,  64, 125])
>>>
>>> np.sqrt(arr2)
array([1., 2., 3., 4., 5.])
```

### 矩阵运算

```
#矩阵乘法。以下两种方法等价，不改变原矩阵
np.dot(A, B)
A.dob(B)

#矩阵转置
A.T
```

# 函数

```python
np.argmax(A)        #返回第一个最大值在扁平化数组（相当于A.reshape((n,))）中的index
fill_diagonal(A, v) #对角元填充指定值
triu(A, k)          #返回A的上三角阵
tril(A, k)          #下三角阵
trace(A)            #迹
diagonal(a, offset=0, axis1=0, axis2=1)
                    #对角元
set_printoptions	#设置打印格式（全局），参数举例：
                    #precision=3, suppress=True 小数点后三位，不使用科学记数法
                    #formatter={'float': '{: 0.3f}'.format}
                    #linewidth=90 一行的长度，到了这么多字符就自动换行
```

