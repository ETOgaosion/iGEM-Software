# Numpy Library

参考[官方文档](https://numpy.org/doc/stable/user/quickstart.html)&nbsp;&nbsp;&nbsp;author:高梓源&nbsp;&nbsp;&nbsp;date:2.2

[点击参阅Scipy Library](scipy.html)

---

[TOC]

## Basic Knowledge

Numpy的研究对象主要是齐次多维数组`ndarray(numpy.array)`，如二维List：

`[[ 1., 0., 0.],`
`[ 0., 1., 2.]]`

有横纵两个轴，因为`List[2][3]`，即纵轴为第一个轴，横轴为第二个轴，长度分别为`2,3`

ndarray对象的重要属性：

名称|解释
:--:|:--:
ndarray.ndim|数组的轴数
ndarray.shape|数组的尺寸。这是一个整数元组，指示每个维度中数组的大小
ndarray.size|数组元素的总数。这等于shape元素的乘积
ndarray.dtype|描述数组中元素类型的对象。标准Python类型,numpy.int32,numpy.int16,numpy.float64
ndarray.itemsize|数组中每个元素的大小(单位:Byte)。等同于ndarray.dtype.itemsize。
ndarray.data|包含数组实际元素的缓冲区。

示例：

```python
>>>import numpy as np
>>>a = np.arange(15).reshape(3, 5)#简便写法，0~14分成3行5列
>>>a
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
>>>a.shape
(3, 5)
>>>a.ndim
2
>>>a.dtype.name
'int64'
>>>a.itemsize
8
>>>a.size
15
>>>type(a)
<class 'numpy.ndarray'>
>>>b = np.array([6, 7, 8])#低级做法，列表值赋给array对象
>>>b
array([6, 7, 8])
```

## 创建数组

### np.array()

利用`np.array()`函数传递列表参数，将列表转换为`ndarray`，列表内的元组转换为列表；`np.array()`包含可选参数给`dtype`：

```python
>>> c = np.array([[1,2],[3,4]], dtype=complex)
>>> c
array([[1.+0.j, 2.+0.j],
       [3.+0.j, 4.+0.j]])
```

### zeros(), ones(), empty()

快速创建已知大小的`ndarray`，注意传递的参数本质上为`ndarray.shape`即一个元组，可以利用0占位：

```python
>>> np.zeros((3, 4))#3行4列
array([[0., 0., 0., 0.],
       [0., 0., 0., 0.],
       [0., 0., 0., 0.]])
```

1占位：

```python
>>> np.ones((2,3,4), dtype=np.int16)
```

空位：

```python
>>> np.empty((2,3))
array([[6.23042070e-307, 1.42417221e-306, 1.37961641e-306],
       [1.69113762e-306, 1.33511562e-306, 3.06320700e-322]])#may vary
#but
>>> np.empty((2,2))
array([[0., 0.],
       [0., 0.]])
```

若已经创建了一个数组，想要快速创建`np.shape`相同的数组，使用：

```python
>>> np.zeros_like(x)
>>> np.empty_like(x)
>>> np.ones_like(x)
>>> np.full_like(x,fill_value,d_type)#创建np.shape相同的数组，填充fill_value，数据类型d_type
```

### arange(), linespace()

创建等差的数字序列可以使用`arange()`，类似`range()`，参数为`start`,`end`,`step`：

```python
>>> np.arange(10, 30, 5)
array([10, 15, 20, 25])
>>> np.arange(0, 2, 0.3)
array([0. , 0.3, 0.6, 0.9, 1.2, 1.5, 1.8])
```

面对可能出现浮点数的情况建议使用`linespace()`，**注意第二个参数`end`被包括，第三个参数为`number`**，即等差数列长度：

```python
>>> np.linspace(0, 2, 9)
array([0.  , 0.25, 0.5 , 0.75, 1.  , 1.25, 1.5 , 1.75, 2.  ])
```

若需要用圆周率`\pi`，引入即可：

```python
>>> from numpy import pi
>>> x=np.linespace(0,2*pi,100)#在函数横坐标集可用
>>> f=np.sin(x)
```

## 基本运算

类似`matlab`中矩阵，`+`,`-`,`*`,`/`,`np.sin()`,`<`可用于元素运算，矩阵乘法是`*`元素对应相乘，`@`或`A.dot(B)`为正常乘法

不同类型数组运算时，结果取更精确的数组(向上转换)

一元运算：`a.sum()`,`a.min()`,`a.max()`，可选参数`axis`，指定轴（维度）来运算

```python
>>> b = np.arange(12).reshape(3,4)
>>> b
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>> b.sum(axis=0)                            # sum of each column
array([12, 15, 18, 21])
>>>
>>> b.min(axis=1)                            # min of each row
array([0, 4, 8])
>>> b.cumsum(axis=1)                         # cumulative sum along each row
array([[ 0,  1,  3,  6],
       [ 4,  9, 15, 22],
       [ 8, 17, 27, 38]])
```

cumsum为对list[i][j]，用`\sum\limits_{k=0}^{j}list[i][k]`替换，一次整体操作

`ndarray`的索引，切片，循环都和标准`python`类似，多维数组可以按照轴（维度）索引，方式为逗号分隔：`b[0:3,1]`,`b[1:3,:]`；索引数少于轴数时缺失索引视为完整切片，且允许用`...`省略已有轴之间或两边的轴索引：

```python
>>> def f(x,y):
...     return 10*x+y
...
>>> b = np.fromfunction(f,(5,4),dtype=int)
array([[ 0,  1,  2,  3],
       [10, 11, 12, 13],
       [20, 21, 22, 23],
       [30, 31, 32, 33],
       [40, 41, 42, 43]])
>>> b[-1]#等价于b[-1,:]
array([40, 41, 42, 43])
>>> b[1,...]#等价于b[1,:]
array([10, 11, 12, 13])
>>> b[...,1]#等价于b[:,1]
array([ 1, 11, 21, 31, 41])
```

若对数组中每个元素进行操作，使用`flat`属性作为迭代器

```python
>>> for element in b.flat:
...     print(element)
```

## 通用函数

对数组调用通用函数将使所有元素改变：

```python
>>> B = np.arange(3)
>>> B
array([0, 1, 2])
>>> np.exp(B)
array([1.        , 2.71828183, 7.3890561 ])
>>> np.sqrt(B)
array([0.        , 1.        , 1.41421356])
>>> C = np.array([2., -1., 4.])
>>> np.add(B, C)
array([2., 0., 6.])
```

其他函数，可点击访问链接：

<div class="admonition seealso">
<p class="admonition-title">See also</p>
<p><a class="reference internal" href="../reference/generated/numpy.all.html#numpy.all" title="numpy.all"><code class="xref py py-obj docutils literal notranslate"><span class="pre">all</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.any.html#numpy.any" title="numpy.any"><code class="xref py py-obj docutils literal notranslate"><span class="pre">any</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.apply_along_axis.html#numpy.apply_along_axis" title="numpy.apply_along_axis"><code class="xref py py-obj docutils literal notranslate"><span class="pre">apply_along_axis</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.argmax.html#numpy.argmax" title="numpy.argmax"><code class="xref py py-obj docutils literal notranslate"><span class="pre">argmax</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.argmin.html#numpy.argmin" title="numpy.argmin"><code class="xref py py-obj docutils literal notranslate"><span class="pre">argmin</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.argsort.html#numpy.argsort" title="numpy.argsort"><code class="xref py py-obj docutils literal notranslate"><span class="pre">argsort</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.average.html#numpy.average" title="numpy.average"><code class="xref py py-obj docutils literal notranslate"><span class="pre">average</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.bincount.html#numpy.bincount" title="numpy.bincount"><code class="xref py py-obj docutils literal notranslate"><span class="pre">bincount</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.ceil.html#numpy.ceil" title="numpy.ceil"><code class="xref py py-obj docutils literal notranslate"><span class="pre">ceil</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.clip.html#numpy.clip" title="numpy.clip"><code class="xref py py-obj docutils literal notranslate"><span class="pre">clip</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.conj.html#numpy.conj" title="numpy.conj"><code class="xref py py-obj docutils literal notranslate"><span class="pre">conj</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.corrcoef.html#numpy.corrcoef" title="numpy.corrcoef"><code class="xref py py-obj docutils literal notranslate"><span class="pre">corrcoef</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.cov.html#numpy.cov" title="numpy.cov"><code class="xref py py-obj docutils literal notranslate"><span class="pre">cov</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.cross.html#numpy.cross" title="numpy.cross"><code class="xref py py-obj docutils literal notranslate"><span class="pre">cross</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.cumprod.html#numpy.cumprod" title="numpy.cumprod"><code class="xref py py-obj docutils literal notranslate"><span class="pre">cumprod</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.cumsum.html#numpy.cumsum" title="numpy.cumsum"><code class="xref py py-obj docutils literal notranslate"><span class="pre">cumsum</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.diff.html#numpy.diff" title="numpy.diff"><code class="xref py py-obj docutils literal notranslate"><span class="pre">diff</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.dot.html#numpy.dot" title="numpy.dot"><code class="xref py py-obj docutils literal notranslate"><span class="pre">dot</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.floor.html#numpy.floor" title="numpy.floor"><code class="xref py py-obj docutils literal notranslate"><span class="pre">floor</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.inner.html#numpy.inner" title="numpy.inner"><code class="xref py py-obj docutils literal notranslate"><span class="pre">inner</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.invert.html#numpy.invert" title="numpy.invert"><code class="xref py py-obj docutils literal notranslate"><span class="pre">invert</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.lexsort.html#numpy.lexsort" title="numpy.lexsort"><code class="xref py py-obj docutils literal notranslate"><span class="pre">lexsort</span></code></a>,
<a class="reference external" href="https://docs.python.org/dev/library/functions.html#max" title="(in Python v3.10)"><code class="xref py py-obj docutils literal notranslate"><span class="pre">max</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.maximum.html#numpy.maximum" title="numpy.maximum"><code class="xref py py-obj docutils literal notranslate"><span class="pre">maximum</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.mean.html#numpy.mean" title="numpy.mean"><code class="xref py py-obj docutils literal notranslate"><span class="pre">mean</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.median.html#numpy.median" title="numpy.median"><code class="xref py py-obj docutils literal notranslate"><span class="pre">median</span></code></a>,
<a class="reference external" href="https://docs.python.org/dev/library/functions.html#min" title="(in Python v3.10)"><code class="xref py py-obj docutils literal notranslate"><span class="pre">min</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.minimum.html#numpy.minimum" title="numpy.minimum"><code class="xref py py-obj docutils literal notranslate"><span class="pre">minimum</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.nonzero.html#numpy.nonzero" title="numpy.nonzero"><code class="xref py py-obj docutils literal notranslate"><span class="pre">nonzero</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.outer.html#numpy.outer" title="numpy.outer"><code class="xref py py-obj docutils literal notranslate"><span class="pre">outer</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.prod.html#numpy.prod" title="numpy.prod"><code class="xref py py-obj docutils literal notranslate"><span class="pre">prod</span></code></a>,
<a class="reference external" href="https://docs.python.org/dev/library/re.html#module-re" title="(in Python v3.10)"><code class="xref py py-obj docutils literal notranslate"><span class="pre">re</span></code></a>,
<a class="reference external" href="https://docs.python.org/dev/library/functions.html#round" title="(in Python v3.10)"><code class="xref py py-obj docutils literal notranslate"><span class="pre">round</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.sort.html#numpy.sort" title="numpy.sort"><code class="xref py py-obj docutils literal notranslate"><span class="pre">sort</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.std.html#numpy.std" title="numpy.std"><code class="xref py py-obj docutils literal notranslate"><span class="pre">std</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.sum.html#numpy.sum" title="numpy.sum"><code class="xref py py-obj docutils literal notranslate"><span class="pre">sum</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.trace.html#numpy.trace" title="numpy.trace"><code class="xref py py-obj docutils literal notranslate"><span class="pre">trace</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.transpose.html#numpy.transpose" title="numpy.transpose"><code class="xref py py-obj docutils literal notranslate"><span class="pre">transpose</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.var.html#numpy.var" title="numpy.var"><code class="xref py py-obj docutils literal notranslate"><span class="pre">var</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.vdot.html#numpy.vdot" title="numpy.vdot"><code class="xref py py-obj docutils literal notranslate"><span class="pre">vdot</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.vectorize.html#numpy.vectorize" title="numpy.vectorize"><code class="xref py py-obj docutils literal notranslate"><span class="pre">vectorize</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.where.html#numpy.where" title="numpy.where"><code class="xref py py-obj docutils literal notranslate"><span class="pre">where</span></code></a></p>
</div>


## 改变数组形状

数组形状由`ndarray.shape`确定，以下三个命令返回修改后的数组但不会覆盖：

```python
>>> a = np.floor(10*rg.random((3,4)))
>>> a
array([[3., 7., 3., 4.],
       [1., 4., 2., 2.],
       [7., 2., 4., 9.]])
>>> a.ravel()#平铺所有元素
array([3., 7., 3., 4., 1., 4., 2., 2., 7., 2., 4., 9.])
>>> a.reshape(6,2)#重排成6行2列
>>> a.T#转置
```

需要说明的是：`ravel`采用`C-style`存储方式，即行优先顺序（字典访问顺序），从0开始索引，对比`Fortran`列优先顺序，从1开始索引，可以通过参数转变索引方式

直接改变原数组的变形方式是：

```python
>>> a.resize(6,2)
```

## 堆叠数组

### stack

根本上都是从此，可设置轴即合并方向

### column_stack

将一维数组堆叠为二维数组，数组横行变纵列，横向合并

```python
>>> a = np.array((1,2,3))
>>> b = np.array((2,3,4))
>>> np.column_stack((a,b))
array([[1, 2],
       [2, 3],
       [3, 4]])
```

### hstack

直接横向合并数组，按行合并，索引方式是对于每一行先全部索引一个数组

```python
>>> a = np.array((1,2,3))
>>> b = np.array((2,3,4))
>>> np.hstack((a,b))
array([1, 2, 3, 2, 3, 4])
>>> a = np.array([[1],[2],[3]])
>>> b = np.array([[2],[3],[4]])
>>> np.hstack((a,b))
array([[1, 2],
       [2, 3],
       [3, 4]])
```

### vstack

纵向直接堆叠，索引方式是每列先全部索引一个数组

```python
>>> a = np.array([1, 2, 3])
>>> b = np.array([2, 3, 4])
>>> np.vstack((a,b))
array([[1, 2, 3],
       [2, 3, 4]])
>>> a = np.array([[1], [2], [3]])
>>> b = np.array([[2], [3], [4]])
>>> np.vstack((a,b))
array([[1],
       [2],
       [3],
       [2],
       [3],
       [4]])

>>> np.column_stack is np.hstack
False
>>> np.row_stack is np.vstack
True
```
**row_stack与vstack是完全等价的**

### dstack

沿深度方向堆积

### concatenate

通常参数形式为`concatenate((a,b),axis=num/None)`，即提供轴进行合并操作，**注意轴数（维度）越大代表在`List[][]`中索引越靠后**，`None`代表全部打散为1D，按行优先索引；

```python
>>> a = np.array([[1, 2], [3, 4]])
>>> b = np.array([[5, 6]])
>>> np.concatenate((a, b), axis=0)
array([[1, 2],
       [3, 4],
       [5, 6]])
>>> np.concatenate((a, b.T), axis=1)
array([[1, 2, 5],
       [3, 4, 6]])
>>> np.concatenate((a, b), axis=None)
array([1, 2, 3, 4, 5, 6])
```

## 拆分数组

### split

以下函数参数基本与此相同，`numpy.split(ndarray，indexs_or_sections，axis = 0)`，参数：

参数|说明
:--:|--
ndarray|
index_or_sections<br>int或一维数组|**`\cdot`** 如果indexs_or_sections是整数N，则该数组将沿axis轴向划分为**N个相等的数组(must)**。<br>**`\cdot`** 如果indexs_or_sections是**一维已排序整数的数组**(可以是`List`,`tuple`,`nparray`），则条目指示该数组沿轴的分割位置。<br>**`\cdot`** 如果**索引沿轴超出数组的维数**，则将相应返回一个空的子数组。
axisint, optional|The axis along which to split, default is 0.

### array_split

类似`split`，可指定分割轴，且可用于数组轴向大小无法整除拆分结果个数的情况，即允许不平分指定轴。对长度为 `l` 的数组要拆分成`n`份，他返回 `l\%n` 个大小 `l//n+1` 的子数组，与其余长度 `l//n` 的子数组：

```python
>>> x = np.arange(8.0)
>>> np.array_split(x, 3)
[array([0.,  1.,  2.]), array([3.,  4.,  5.]), array([6.,  7.])]

>>> x = np.arange(9)
>>> np.array_split(x, 4)
[array([0, 1, 2]), array([3, 4]), array([5, 6]), array([7, 8])]
```

### hsplit

沿数组的水平轴横向拆分数组

## 复制

简单的`=`赋值不会创建新`object`

```python
>>> a = np.array([[ 0,  1,  2,  3],
...               [ 4,  5,  6,  7],
...               [ 8,  9, 10, 11]])
>>> b = a            # no new object is created
>>> b is a           # a and b are two names for the same ndarray object
True
```
使用`a.view()`可以创建一个共享数据的对象，改变形状不会互相影响但改变数据是对双方的，直接切片的效果相同

```python
>>> c = a.view()
>>> c is a
False
>>> c.base is a                        # c is a view of the data owned by a
True
>>> c.flags.owndata
False
>>> c = c.reshape((2, 6))                      # a's shape doesn't change
>>> a.shape
(3, 4)
>>> c[0, 4] = 1234                      # a's data changes
>>> a
array([[   0,    1,    2,    3],
       [1234,    5,    6,    7],
       [   8,    9,   10,   11]])

#对于切片
>>> s = a[ : , 1:3]
>>> s[:] = 10
>>> a
array([[   0,   10,   10,    3],
       [1234,   10,   10,    7],
       [   8,   10,   10,   11]])
```

而`a.copy()`的方法能够对数组数据全部拷贝到新数组中

```python
>>> d = a.copy()                          # a new array object with new data is created
>>> d is a
False
>>> d.base is a                           # d doesn't share anything with a
False
>>> d[0,0] = 9999
>>> a
array([[   0,   10,   10,    3],
       [1234,   10,   10,    7],
       [   8,   10,   10,   11]])
```

有时如果不再需要原来的数组，进行切片后调用copy：`b = a[:100].copy()`。

## 函数与方法

<div class="section" id="functions-and-methods-overview">
<h3>Functions and Methods Overview<a class="headerlink" href="#functions-and-methods-overview" title="Permalink to this headline">¶</a></h3>
<p>Here is a list of some useful NumPy functions and methods names
ordered in categories. See <a class="reference internal" href="../reference/routines.html#routines"><span class="std std-ref">Routines</span></a> for the full list.</p>
<dl class="simple">
<dt>Array Creation</dt><dd><p><a class="reference internal" href="../reference/generated/numpy.arange.html#numpy.arange" title="numpy.arange"><code class="xref py py-obj docutils literal notranslate"><span class="pre">arange</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.array.html#numpy.array" title="numpy.array"><code class="xref py py-obj docutils literal notranslate"><span class="pre">array</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.copy.html#numpy.copy" title="numpy.copy"><code class="xref py py-obj docutils literal notranslate"><span class="pre">copy</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.empty.html#numpy.empty" title="numpy.empty"><code class="xref py py-obj docutils literal notranslate"><span class="pre">empty</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.empty_like.html#numpy.empty_like" title="numpy.empty_like"><code class="xref py py-obj docutils literal notranslate"><span class="pre">empty_like</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.eye.html#numpy.eye" title="numpy.eye"><code class="xref py py-obj docutils literal notranslate"><span class="pre">eye</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.fromfile.html#numpy.fromfile" title="numpy.fromfile"><code class="xref py py-obj docutils literal notranslate"><span class="pre">fromfile</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.fromfunction.html#numpy.fromfunction" title="numpy.fromfunction"><code class="xref py py-obj docutils literal notranslate"><span class="pre">fromfunction</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.identity.html#numpy.identity" title="numpy.identity"><code class="xref py py-obj docutils literal notranslate"><span class="pre">identity</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.linspace.html#numpy.linspace" title="numpy.linspace"><code class="xref py py-obj docutils literal notranslate"><span class="pre">linspace</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.logspace.html#numpy.logspace" title="numpy.logspace"><code class="xref py py-obj docutils literal notranslate"><span class="pre">logspace</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.mgrid.html#numpy.mgrid" title="numpy.mgrid"><code class="xref py py-obj docutils literal notranslate"><span class="pre">mgrid</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.ogrid.html#numpy.ogrid" title="numpy.ogrid"><code class="xref py py-obj docutils literal notranslate"><span class="pre">ogrid</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.ones.html#numpy.ones" title="numpy.ones"><code class="xref py py-obj docutils literal notranslate"><span class="pre">ones</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.ones_like.html#numpy.ones_like" title="numpy.ones_like"><code class="xref py py-obj docutils literal notranslate"><span class="pre">ones_like</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.r_.html#numpy.r_" title="numpy.r_"><code class="xref py py-obj docutils literal notranslate"><span class="pre">r_</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.zeros.html#numpy.zeros" title="numpy.zeros"><code class="xref py py-obj docutils literal notranslate"><span class="pre">zeros</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.zeros_like.html#numpy.zeros_like" title="numpy.zeros_like"><code class="xref py py-obj docutils literal notranslate"><span class="pre">zeros_like</span></code></a></p>
</dd>
<dt>Conversions</dt><dd><p><a class="reference internal" href="../reference/generated/numpy.ndarray.astype.html#numpy.ndarray.astype" title="numpy.ndarray.astype"><code class="xref py py-obj docutils literal notranslate"><span class="pre">ndarray.astype</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.atleast_1d.html#numpy.atleast_1d" title="numpy.atleast_1d"><code class="xref py py-obj docutils literal notranslate"><span class="pre">atleast_1d</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.atleast_2d.html#numpy.atleast_2d" title="numpy.atleast_2d"><code class="xref py py-obj docutils literal notranslate"><span class="pre">atleast_2d</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.atleast_3d.html#numpy.atleast_3d" title="numpy.atleast_3d"><code class="xref py py-obj docutils literal notranslate"><span class="pre">atleast_3d</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.mat.html#numpy.mat" title="numpy.mat"><code class="xref py py-obj docutils literal notranslate"><span class="pre">mat</span></code></a></p>
</dd>
<dt>Manipulations</dt><dd><p><a class="reference internal" href="../reference/generated/numpy.array_split.html#numpy.array_split" title="numpy.array_split"><code class="xref py py-obj docutils literal notranslate"><span class="pre">array_split</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.column_stack.html#numpy.column_stack" title="numpy.column_stack"><code class="xref py py-obj docutils literal notranslate"><span class="pre">column_stack</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.concatenate.html#numpy.concatenate" title="numpy.concatenate"><code class="xref py py-obj docutils literal notranslate"><span class="pre">concatenate</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.diagonal.html#numpy.diagonal" title="numpy.diagonal"><code class="xref py py-obj docutils literal notranslate"><span class="pre">diagonal</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.dsplit.html#numpy.dsplit" title="numpy.dsplit"><code class="xref py py-obj docutils literal notranslate"><span class="pre">dsplit</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.dstack.html#numpy.dstack" title="numpy.dstack"><code class="xref py py-obj docutils literal notranslate"><span class="pre">dstack</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.hsplit.html#numpy.hsplit" title="numpy.hsplit"><code class="xref py py-obj docutils literal notranslate"><span class="pre">hsplit</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.hstack.html#numpy.hstack" title="numpy.hstack"><code class="xref py py-obj docutils literal notranslate"><span class="pre">hstack</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.ndarray.item.html#numpy.ndarray.item" title="numpy.ndarray.item"><code class="xref py py-obj docutils literal notranslate"><span class="pre">ndarray.item</span></code></a>,
<a class="reference internal" href="../reference/constants.html#numpy.newaxis" title="numpy.newaxis"><code class="xref py py-obj docutils literal notranslate"><span class="pre">newaxis</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.ravel.html#numpy.ravel" title="numpy.ravel"><code class="xref py py-obj docutils literal notranslate"><span class="pre">ravel</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.repeat.html#numpy.repeat" title="numpy.repeat"><code class="xref py py-obj docutils literal notranslate"><span class="pre">repeat</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.reshape.html#numpy.reshape" title="numpy.reshape"><code class="xref py py-obj docutils literal notranslate"><span class="pre">reshape</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.resize.html#numpy.resize" title="numpy.resize"><code class="xref py py-obj docutils literal notranslate"><span class="pre">resize</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.squeeze.html#numpy.squeeze" title="numpy.squeeze"><code class="xref py py-obj docutils literal notranslate"><span class="pre">squeeze</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.swapaxes.html#numpy.swapaxes" title="numpy.swapaxes"><code class="xref py py-obj docutils literal notranslate"><span class="pre">swapaxes</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.take.html#numpy.take" title="numpy.take"><code class="xref py py-obj docutils literal notranslate"><span class="pre">take</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.transpose.html#numpy.transpose" title="numpy.transpose"><code class="xref py py-obj docutils literal notranslate"><span class="pre">transpose</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.vsplit.html#numpy.vsplit" title="numpy.vsplit"><code class="xref py py-obj docutils literal notranslate"><span class="pre">vsplit</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.vstack.html#numpy.vstack" title="numpy.vstack"><code class="xref py py-obj docutils literal notranslate"><span class="pre">vstack</span></code></a></p>
</dd>
<dt>Questions</dt><dd><p><a class="reference internal" href="../reference/generated/numpy.all.html#numpy.all" title="numpy.all"><code class="xref py py-obj docutils literal notranslate"><span class="pre">all</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.any.html#numpy.any" title="numpy.any"><code class="xref py py-obj docutils literal notranslate"><span class="pre">any</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.nonzero.html#numpy.nonzero" title="numpy.nonzero"><code class="xref py py-obj docutils literal notranslate"><span class="pre">nonzero</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.where.html#numpy.where" title="numpy.where"><code class="xref py py-obj docutils literal notranslate"><span class="pre">where</span></code></a></p>
</dd>
<dt>Ordering</dt><dd><p><a class="reference internal" href="../reference/generated/numpy.argmax.html#numpy.argmax" title="numpy.argmax"><code class="xref py py-obj docutils literal notranslate"><span class="pre">argmax</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.argmin.html#numpy.argmin" title="numpy.argmin"><code class="xref py py-obj docutils literal notranslate"><span class="pre">argmin</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.argsort.html#numpy.argsort" title="numpy.argsort"><code class="xref py py-obj docutils literal notranslate"><span class="pre">argsort</span></code></a>,
<a class="reference external" href="https://docs.python.org/dev/library/functions.html#max" title="(in Python v3.10)"><code class="xref py py-obj docutils literal notranslate"><span class="pre">max</span></code></a>,
<a class="reference external" href="https://docs.python.org/dev/library/functions.html#min" title="(in Python v3.10)"><code class="xref py py-obj docutils literal notranslate"><span class="pre">min</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.ptp.html#numpy.ptp" title="numpy.ptp"><code class="xref py py-obj docutils literal notranslate"><span class="pre">ptp</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.searchsorted.html#numpy.searchsorted" title="numpy.searchsorted"><code class="xref py py-obj docutils literal notranslate"><span class="pre">searchsorted</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.sort.html#numpy.sort" title="numpy.sort"><code class="xref py py-obj docutils literal notranslate"><span class="pre">sort</span></code></a></p>
</dd>
<dt>Operations</dt><dd><p><a class="reference internal" href="../reference/generated/numpy.choose.html#numpy.choose" title="numpy.choose"><code class="xref py py-obj docutils literal notranslate"><span class="pre">choose</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.compress.html#numpy.compress" title="numpy.compress"><code class="xref py py-obj docutils literal notranslate"><span class="pre">compress</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.cumprod.html#numpy.cumprod" title="numpy.cumprod"><code class="xref py py-obj docutils literal notranslate"><span class="pre">cumprod</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.cumsum.html#numpy.cumsum" title="numpy.cumsum"><code class="xref py py-obj docutils literal notranslate"><span class="pre">cumsum</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.inner.html#numpy.inner" title="numpy.inner"><code class="xref py py-obj docutils literal notranslate"><span class="pre">inner</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.ndarray.fill.html#numpy.ndarray.fill" title="numpy.ndarray.fill"><code class="xref py py-obj docutils literal notranslate"><span class="pre">ndarray.fill</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.imag.html#numpy.imag" title="numpy.imag"><code class="xref py py-obj docutils literal notranslate"><span class="pre">imag</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.prod.html#numpy.prod" title="numpy.prod"><code class="xref py py-obj docutils literal notranslate"><span class="pre">prod</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.put.html#numpy.put" title="numpy.put"><code class="xref py py-obj docutils literal notranslate"><span class="pre">put</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.putmask.html#numpy.putmask" title="numpy.putmask"><code class="xref py py-obj docutils literal notranslate"><span class="pre">putmask</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.real.html#numpy.real" title="numpy.real"><code class="xref py py-obj docutils literal notranslate"><span class="pre">real</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.sum.html#numpy.sum" title="numpy.sum"><code class="xref py py-obj docutils literal notranslate"><span class="pre">sum</span></code></a></p>
</dd>
<dt>Basic Statistics</dt><dd><p><a class="reference internal" href="../reference/generated/numpy.cov.html#numpy.cov" title="numpy.cov"><code class="xref py py-obj docutils literal notranslate"><span class="pre">cov</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.mean.html#numpy.mean" title="numpy.mean"><code class="xref py py-obj docutils literal notranslate"><span class="pre">mean</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.std.html#numpy.std" title="numpy.std"><code class="xref py py-obj docutils literal notranslate"><span class="pre">std</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.var.html#numpy.var" title="numpy.var"><code class="xref py py-obj docutils literal notranslate"><span class="pre">var</span></code></a></p>
</dd>
<dt>Basic Linear Algebra</dt><dd><p><a class="reference internal" href="../reference/generated/numpy.cross.html#numpy.cross" title="numpy.cross"><code class="xref py py-obj docutils literal notranslate"><span class="pre">cross</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.dot.html#numpy.dot" title="numpy.dot"><code class="xref py py-obj docutils literal notranslate"><span class="pre">dot</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.outer.html#numpy.outer" title="numpy.outer"><code class="xref py py-obj docutils literal notranslate"><span class="pre">outer</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.linalg.svd.html#numpy.linalg.svd" title="numpy.linalg.svd"><code class="xref py py-obj docutils literal notranslate"><span class="pre">linalg.svd</span></code></a>,
<a class="reference internal" href="../reference/generated/numpy.vdot.html#numpy.vdot" title="numpy.vdot"><code class="xref py py-obj docutils literal notranslate"><span class="pre">vdot</span></code></a></p>
</dd>
</dl>
</div>

## 高级索引trick

### 使用数字数组索引

两个`ndarray`可以相互充当索引，维度不限，当被索引数组为多维时，索引数组作用在第一个维度：

```python
#索引数组一维
>>> a = np.arange(12)**2
>>> i = np.array([1, 1, 3, 8, 5])
>>> a[i] 
array([ 1,  1,  9, 64, 25])

#索引数组二维
>>> j = np.array([[3, 4], [9, 7]])
>>> a[j]
array([[ 9, 16],
       [81, 49]])

#被索引数组二维
>>> palette = np.array([[0, 0, 0],         # black
...                     [255, 0, 0],       # red
...                     [0, 255, 0],       # green
...                     [0, 0, 255],       # blue
...                     [255, 255, 255]])  # white
>>> image = np.array([[0, 1, 2, 0],        # 只索引第i行，即一种颜色
...                   [0, 3, 4, 0]])
>>> palette[image]                         # the (2, 4, 3) color image
array([[[  0,   0,   0],
        [255,   0,   0],
        [  0, 255,   0],
        [  0,   0,   0]],

       [[  0,   0,   0],
        [  0,   0, 255],
        [255, 255, 255],
        [  0,   0,   0]]])
```

索引不同维度可以传递形如`a[np.array1,np.array2]`的切片，`np.array1`,`np.array2`必须具有相同的形状，索引方式为对不同维度进行点组合，对应填在形状中即可，若存在其他组合，填在新的维度上，原维度的`shape`不变：

```python
>>> a = np.arange(12).reshape(3,4)
>>> a
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>> i = np.array([[0, 1],                     # indices for the first dim of a
...               [1, 2]])
>>> j = np.array([[2, 1],                     # indices for the second dim
...               [3, 3]])
>>>
>>> a[i, j]                                   # i and j must have equal shape
array([[ 2,  5],
       [ 7, 11]])
>>>
>>> a[i, 2]
array([[ 2,  6],
       [ 6, 10]])
>>>
>>> a[:, j]                                     # i.e., a[ : , j]
array([[[ 2,  1],
        [ 3,  3]],

       [[ 6,  5],
        [ 7,  7]],

       [[10,  9],
        [11, 11]]])
```

因为`arr[i,j]==arr[(i,j)]`，因此可将元组赋值`l=(i,j)`，而后直接索引`a[l]`即可，但是不可以放在`List`中直接使用，而要`a[tuple(List)]`

#### 应用

##### 便于提取最大最小值点

首先是`argmax`函数`numpy.argmax(a, axis=None, out=None)`，作用是返回沿着给定或默认轴方向的最大值地址：

```python
>>> a = np.arange(6).reshape(2,3) + 10
>>> a
array([[10, 11, 12],
       [13, 14, 15]])
>>> np.argmax(a)#不指定轴时按照平铺方式筛选，优先索引行
5
>>> np.argmax(a, axis=0)#相当于纵向压缩，纵向寻找每列的最大值
array([1, 1, 1])
>>> np.argmax(a, axis=1)#横向压缩，横向寻找每行最大值
array([2, 2])
```

其次是`numpy.all(a, axis=None, out=None, keepdims=<no value>, *, where=<no value>)`用来沿某个轴进行逻辑规约：

```python
>>> np.all([[True,False],[True,True]])
False
>>> np.all([[True,False],[True,True]], axis=0)
array([ True, False])
>>> np.all([-1, 4, 5])
True
>>> np.all([1.0, np.nan])#np.nan为NaN的IEEE 754浮点表示
True
```

回到最大最小值点：

```python
>>> time = np.linspace(20, 145, 5)                 # time scale
>>> data = np.sin(np.arange(20)).reshape(5,4)      # 4 time-dependent series
>>> time
array([ 20.  ,  51.25,  82.5 , 113.75, 145.  ])
>>> data
array([[ 0.        ,  0.84147098,  0.90929743,  0.14112001],
       [-0.7568025 , -0.95892427, -0.2794155 ,  0.6569866 ],
       [ 0.98935825,  0.41211849, -0.54402111, -0.99999021],
       [-0.53657292,  0.42016704,  0.99060736,  0.65028784],
       [-0.28790332, -0.96139749, -0.75098725,  0.14987721]])

# index of the maxima for each series
>>> ind = data.argmax(axis=0)
>>> ind
array([2, 0, 3, 1])

# times corresponding to the maxima
>>> time_max = time[ind]
>>>
>>> data_max = data[ind, range(data.shape[1])]#就是把每一列最大值取出

>>> time_max
array([ 82.5 ,  20.  , 113.75,  51.25])
>>> data_max
array([0.98935825, 0.84147098, 0.99060736, 0.6569866 ])

>>> np.all(data_max == data.max(axis=0))
True
```

#### 通过索引赋值数组

索引列表包含重复项时，以最后一次赋值为准，但`+=`则只会赋值一次：

```python
>>> a = np.arange(5)
>>> a
array([0, 1, 2, 3, 4])
>>> a[[1,3,4]] = 0
>>> a
array([0, 0, 2, 0, 0])
#重复索引进行赋值
>>> a[[0,0,2]]=[1,2,3]
>>> a
array([2, 1, 3, 3, 4])
#重复索引应用+=
>>> a[[0,0,2]]+=1
>>> a
array([1, 1, 3, 3, 4])
```

### 使用布尔数组索引

**核心在于不索引`False`值**

#### 一维情况

添加条件，形成和原数组相同`shape`的新布尔数组`b`，而后传递索引数组`b`，可以只取满足条件，即布尔值为`True`位置的值：

```python
>>> a = np.arange(12).reshape(3,4)
>>> b = a > 4
>>> b
array([[False, False, False, False],
       [False,  True,  True,  True],
       [ True,  True,  True,  True]])
>>> a[b]#返回1D
array([ 5,  6,  7,  8,  9, 10, 11])
```

一个漂亮的应用，而目前还无法理解：
>生成生成Mandelbrot集

```python
import numpy as np
import matplotlib.pyplot as plt
def mandelbrot( h,w, maxit=20 ):
    """Returns an image of the Mandelbrot fractal of size (h,w)."""
    y,x = np.ogrid[ -1.4:1.4:h*1j, -2:0.8:w*1j ]
    c = x+y*1j
    z = c
    divtime = maxit + np.zeros(z.shape, dtype=int)

    for i in range(maxit):
        z = z**2 + c
        diverge = z*np.conj(z) > 2**2            # who is diverging
        div_now = diverge & (divtime==maxit)  # who is diverging now
        divtime[div_now] = i                  # note when
        z[diverge] = 2                        # avoid diverging too much

    return divtime
plt.imshow(mandelbrot(400,400))
```

#### 二维情况

记住布尔数组索引的核心

```python
>>> a = np.arange(12).reshape(3,4)
>>> b1 = np.array([False,True,True])             # first dim selection
>>> b2 = np.array([True,False,True,False])       # second dim selection
>>>#只索引True值
>>> a[b1,:]                                   # selecting rows
array([[ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>>
>>> a[b1]                                     # same thing
array([[ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>>
>>> a[:,b2]                                   # selecting columns
array([[ 0,  2],
       [ 4,  6],
       [ 8, 10]])
>>>
>>> a[b1,b2]                                  # a weird thing to do
array([ 4, 10])
```

### ix_()

`ix_()`函数可用于组合不同的向量，以获得每个n片段的结果。例如，如果您要计算从向量a，b和c中选取的所有三元组的所有a + b * c：

```python
>>> a = np.array([2,3,4,5])
>>> b = np.array([8,5,4])
>>> c = np.array([5,4,6,8,3])
>>> ax,bx,cx = np.ix_(a,b,c)
>>> ax
array([[[2]],
       [[3]],
       [[4]],
       [[5]]])
>>> bx
array([[[8],
        [5],
        [4]]])
>>> cx
array([[[5, 4, 6, 8, 3]]])
>>> ax.shape, bx.shape, cx.shape
((4, 1, 1), (1, 3, 1), (1, 1, 5))
>>> result = ax+bx*cx
>>> result
array([[[42, 34, 50, 66, 26],
        [27, 22, 32, 42, 17],
        [22, 18, 26, 34, 14]],
       [[43, 35, 51, 67, 27],
        [28, 23, 33, 43, 18],
        [23, 19, 27, 35, 15]],
       [[44, 36, 52, 68, 28],
        [29, 24, 34, 44, 19],
        [24, 20, 28, 36, 16]],
       [[45, 37, 53, 69, 29],
        [30, 25, 35, 45, 20],
        [25, 21, 29, 37, 17]]])
>>> result[3,2,4]
17
>>> a[3]+b[2]*c[4]
17
```

## trick

### reshape

要更改数组的尺寸，您可以忽略其中一个尺寸（使用`-1`），程序自动推断尺寸：

```python
>>> b = a.reshape((2, -1, 3))  # -1 means "whatever is needed"
```

# 友链

上述所有都是在`python`中广泛应用的`Numpy`库，`Matlab`中也可使用，参看：
https://numpy.org/doc/stable/user/numpy-for-matlab-users.html

[点击参阅Scipy Library](scipy.htmL)