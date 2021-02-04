# Scipy Library

参考[官方文档](https://docs.scipy.org/doc/scipy/reference/tutorial)&nbsp;&nbsp;&nbsp;author:高梓源&nbsp;&nbsp;&nbsp;date:2.4

[点击参阅Numpy Library](numpy.html)

---

[TOC]

## 简介

`SciPy`是基于`Python`的`NumPy`扩展库构建的数学算法和便利函数的集合，解决高等数学问题。

通常导入相关软件包为：

```python
>>> import numpy as np
>>> import matplotlib as mpl
>>> import matplotlib.pyplot as plt
```

内部`subpackages`：

<table class="docutils align-default">
<colgroup>
<col style="width: 25%;" />
<col style="width: 75%;" />
</colgroup>
<thead>
<tr class="row-odd"><th class="head"><p>Subpackage</p></th>
<th class="head"><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even"><td><p><a class="reference internal" href="../cluster.html#module-scipy.cluster" title="scipy.cluster"><code class="xref py py-obj docutils literal notranslate"><span class="pre">cluster</span></code></a></p></td>
<td><p>聚类算法</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="../constants.html#module-scipy.constants" title="scipy.constants"><code class="xref py py-obj docutils literal notranslate"><span class="pre">constants</span></code></a></p></td>
<td><p>物理和数学常数</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="../fftpack.html#module-scipy.fftpack" title="scipy.fftpack"><code class="xref py py-obj docutils literal notranslate"><span class="pre">fftpack</span></code></a></p></td>
<td><p>快速傅立叶变换例程</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="../integrate.html#module-scipy.integrate" title="scipy.integrate"><code class="xref py py-obj docutils literal notranslate"><span class="pre">integrate</span></code></a></p></td>
<td><p>积分和常微分方程求解器</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="../interpolate.html#module-scipy.interpolate" title="scipy.interpolate"><code class="xref py py-obj docutils literal notranslate"><span class="pre">interpolate</span></code></a></p></td>
<td><p>插值和平滑样条</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="../io.html#module-scipy.io" title="scipy.io"><code class="xref py py-obj docutils literal notranslate"><span class="pre">io</span></code></a></p></td>
<td><p>输入输出</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="../linalg.html#module-scipy.linalg" title="scipy.linalg"><code class="xref py py-obj docutils literal notranslate"><span class="pre">linalg</span></code></a></p></td>
<td><p>线性代数</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="../ndimage.html#module-scipy.ndimage" title="scipy.ndimage"><code class="xref py py-obj docutils literal notranslate"><span class="pre">ndimage</span></code></a></p></td>
<td><p>N维图像处理</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="../odr.html#module-scipy.odr" title="scipy.odr"><code class="xref py py-obj docutils literal notranslate"><span class="pre">odr</span></code></a></p></td>
<td><p>正交距离回归</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="../optimize.html#module-scipy.optimize" title="scipy.optimize"><code class="xref py py-obj docutils literal notranslate"><span class="pre">optimize</span></code></a></p></td>
<td><p>优化和寻根例程</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="../signal.html#module-scipy.signal" title="scipy.signal"><code class="xref py py-obj docutils literal notranslate"><span class="pre">signal</span></code></a></p></td>
<td><p>信号处理</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="../sparse.html#module-scipy.sparse" title="scipy.sparse"><code class="xref py py-obj docutils literal notranslate"><span class="pre">sparse</span></code></a></p></td>
<td><p>稀疏矩阵和相关例程</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="../spatial.html#module-scipy.spatial" title="scipy.spatial"><code class="xref py py-obj docutils literal notranslate"><span class="pre">spatial</span></code></a></p></td>
<td><p>空间数据结构和算法</p></td>
</tr>
<tr class="row-odd"><td><p><a class="reference internal" href="../special.html#module-scipy.special" title="scipy.special"><code class="xref py py-obj docutils literal notranslate"><span class="pre">special</span></code></a></p></td>
<td><p>特殊功能</p></td>
</tr>
<tr class="row-even"><td><p><a class="reference internal" href="../stats.html#module-scipy.stats" title="scipy.stats"><code class="xref py py-obj docutils literal notranslate"><span class="pre">stats</span></code></a></p></td>
<td><p>统计分布和功能</p></td>
</tr>
</tbody>
</table>

SciPy子软件包需要单独导入：

```python
>>> from scipy import linalg, optimize
```

## 使用方法

在需要时键入

```python
>>> help(package.name)
```

或点上述链接即可参阅

[点击参阅Numpy Library](numpy.html)