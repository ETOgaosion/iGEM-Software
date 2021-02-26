# Matplotlib Library

参考[官方文档](https://matplotlib.org/stable/users/index.html)&nbsp;&nbsp;&nbsp;author:高梓源&nbsp;&nbsp;&nbsp;date:2.25

[点击参阅pillow Library](pillow.html)

---

[TOC]

# Quick Start

Matplotlib在窗口部件上绘图，first:

```python
import matplotlib.pyplot as plt
import numpy as np
```

## 绘图

在matplotlib中，您有两个选项：

创建您的图并在最后绘制它们：
```python
import matplotlib.pyplot as plt

plt.plot(x, y)
plt.plot(z, t)
plt.show()
```
创建图并在创建后立即绘制它们：

```python
import matplotlib.pyplot as plt
from matplotlib import interactive
interactive(True)

plt.plot(x, y)
raw_input('press return to continue')

plt.plot(z, t)
raw_input('press return to end')
```
**交互模式也可以通过`matplotlib.pyplot.ion()`和`matplotlib.pyplot.ioff()`来开关。**

在大多数交互式后端上，如果通过面向对象的界面更改了图形窗口，它也会被更新。例如，获取对该Axes实例的引用，并调用该 实例的方法：
```python
ax = plt.gca()
ax.plot([3.1, 2.2])
```

数据点的绘制：

```python
#创建图
fig, ax = plt.subplots()  # Create a figure containing a single axes.
ax.plot([1, 2, 3, 4], [1, 4, 2, 3])  # Plot some data on the axes.

#直接绘制
plt.plot([1, 2, 3, 4], [1, 4, 2, 3])  # Matplotlib plot.
```

## 面向对象的接口和pyplot接口
使用Matplotlib基本上有两种方法：

- 显式创建图形和轴，并在其上调用方法（“面向对象（OO）样式”）。
- 依靠pyplot自动创建和管理图形和轴，并使用pyplot函数进行绘图。

OO风格:
```python
x = np.linspace(0, 2, 100)

# Note that even in the OO-style, we use `.pyplot.figure` to create the figure.
fig, ax = plt.subplots()  # Create a figure and an axes.
ax.plot(x, x, label='linear')  # Plot some data on the axes.
ax.plot(x, x**2, label='quadratic')  # Plot more data on the axes...
ax.plot(x, x**3, label='cubic')  # ... and some more.
ax.set_xlabel('x label')  # Add an x-label to the axes.
ax.set_ylabel('y label')  # Add a y-label to the axes.
ax.set_title("Simple Plot")  # Add a title to the axes.
ax.legend()  # Add a legend.
```

`pyplot`风格：
```python
x = np.linspace(0, 2, 100)

plt.plot(x, x, label='linear')  # Plot some data on the (implicit) axes.
plt.plot(x, x**2, label='quadratic')  # etc.
plt.plot(x, x**3, label='cubic')
plt.xlabel('x label')
plt.ylabel('y label')
plt.title("Simple Plot")
plt.legend()
```

绘制相同的图，但是使用不同的数据集，可以编写专门的函数来进行绘制：

```python
def my_plotter(ax, data1, data2, param_dict):
    """
    Parameters
    ----------
    ax : Axes
    data1 : array
       The x data
    data2 : array
       The y data
    param_dict : dict
       Dictionary of kwargs to pass to ax.plot
    out : list
        list of artists added
    """
    out = ax.plot(data1, data2, **param_dict)
    return out
```

调用时可直接：

```python
data1, data2, data3, data4 = np.random.randn(4, 100)
fig, ax = plt.subplots(1, 1)
my_plotter(ax, data1, data2, {'marker': 'x'})
```
当想绘制两幅组成的子图：

```python
fig, (ax1, ax2) = plt.subplots(1, 2)
my_plotter(ax1, data1, data2, {'marker': 'x'})
my_plotter(ax2, data3, data4, {'marker': 'o'})
```

## 绘图（函数）

目的|函数
:-:|:-:
Line Plot|[plot()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html#matplotlib.pyplot.plot)
Multiple subplots in one figure|[subplot()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.subplot.html#matplotlib.pyplot.subplot)
Images|[imshow()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.imshow.html#matplotlib.pyplot.imshow)
轮廓和伪彩色|[pcolormesh()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.pcolormesh.html#matplotlib.pyplot.pcolormesh)<br>[contour()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.contour.html#matplotlib.pyplot.contour)
直方图|[hist()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.hist.html#matplotlib.pyplot.hist)
路径|[matplotlib.path](https://matplotlib.org/stable/api/path_api.html#module-matplotlib.path)模块
三维绘图|[mplot3d工具包](https://matplotlib.org/stable/gallery/index.html#mplot3d-examples-index)
流线图|[streamplot()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.streamplot.html#matplotlib.pyplot.streamplot)
椭圆|[Arc](https://matplotlib.org/stable/api/_as_gen/matplotlib.patches.Arc.html#matplotlib.patches.Arc)
条形图|[bar()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.bar.html#matplotlib.pyplot.bar)
饼图|[pie()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.pie.html#matplotlib.pyplot.pie)
文本表添加到轴|[table()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.table.html#matplotlib.pyplot.table)
散点图|[scatter()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.scatter.html#matplotlib.pyplot.scatter)
GUI小部件|[matplotlib.widgets](https://matplotlib.org/stable/api/widgets_api.html#module-matplotlib.widgets)
充满曲线|[fill()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.fill.html#matplotlib.pyplot.fill)
自定义刻度线格式器|[matplotlib.ticker](https://matplotlib.org/stable/api/ticker_api.html#module-matplotlib.ticker)和[matplotlib.dates](https://matplotlib.org/stable/api/dates_api.html#module-matplotlib.dates)
对数图|[semilogx()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.semilogx.html#matplotlib.pyplot.semilogx),[semilogy()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.semilogy.html#matplotlib.pyplot.semilogy)和[loglog()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.loglog.html#matplotlib.pyplot.loglog)
极坐标图|[polar()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.polar.html#matplotlib.pyplot.polar)
自动生成图形图例|[legend()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.legend.html#matplotlib.pyplot.legend)


## 绘图（例子）

### Style

更改特定线条的宽度或颜色，引入`style`样式库

```python
from matplotlib import pyplot as plt
from matplotlib import style
 
style.use('ggplot')
x = [5,8,10]
y = [12,16,6]
x2 = [6,9,11]
y2 = [6,15,7]
plt.plot(x,y,'g',label='line one', linewidth=5)
plt.plot(x2,y2,'c',label='line two',linewidth=5)
plt.title('Epic Info')
plt.ylabel('Y axis')
plt.xlabel('X axis')
plt.legend()
plt.grid(True,color='k')
plt.show()
```

### 条形图
`plt.bar`

```python
plt.bar([0.25,1.25,2.25,3.25,4.25],[50,40,70,80,20],
label="BMW",width=.5)
plt.bar([.75,1.75,2.75,3.75,4.75],[80,20,20,50,60],
label="Audi", color='r',width=.5)
```

### 直方图
先列出统计数据
用`bin`是指划分为一系列间隔的值的范围
`plt.hist`

```python
population_age = [22,55,62,45,21,22,34,42,42,4,2,102,95,85,55,110,120,70,65,55,111,115,80,75,65,54,44,43,42,48]
bins = [0,10,20,30,40,50,60,70,80,90,100]
plt.hist(population_age, bins, histtype='bar', rwidth=0.8)
```

### 散点图
`plt.scatter`

```python
x = [1,1.5,2,2.5,3,3.5,3.6]
y = [7.5,8,8.5,9,9.5,10,10.5]
x1=[8,8.5,9,9.5,10,10.5,11]
y1=[3,3.5,3.7,4,4.5,5,5.2]
plt.scatter(x,y, label='high income low saving',color='r')
plt.scatter(x1,y1,label='low income high savings',color='b')
```

### 面积图

`plt.stackplot`

```python
days = [1,2,3,4,5]
sleeping =[7,8,6,11,7]
eating = [2,3,4,3,2]
working =[7,8,7,2,2]
playing = [8,5,7,8,13]
plt.plot([],[],color='m', label='Sleeping', linewidth=5)
plt.plot([],[],color='c', label='Eating', linewidth=5)
plt.plot([],[],color='r', label='Working', linewidth=5)
plt.plot([],[],color='k', label='Playing', linewidth=5)
  
plt.stackplot(days, sleeping,eating,working,playing, colors=['m','c','r','k'])
```

### 饼图
`plt.pie`

类似上例：

```python
days = [1,2,3,4,5]
sleeping =[7,8,6,11,7]
eating = [2,3,4,3,2]
working =[7,8,7,2,2]
playing = [8,5,7,8,13]
slices = [7,2,2,13]
activities = ['sleeping','eating','working','playing']
cols = ['c','m','r','b']
 
plt.pie(slices,
  labels=activities,
  colors=cols,
  startangle=90,
  shadow= True,
  explode=(0,0.1,0,0),
  autopct='%1.1f%%')
```

# Pyplot

简例：
```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4])
plt.ylabel('some numbers')
plt.show()
```

`plot`是一种多功能函数，它将接受任意数量的参数。

格式化绘图样式，`plt.plot()`中加入格式参数，默认为`b-`，参数同`matlab`，例：

```python
plt.plot([1, 2, 3, 4], [1, 4, 9, 16], 'ro')
plt.axis([0, 6, 0, 20])
plt.show()
```

> `plt.axis([xmin,xmax,ymin,ymax])`
> `ax.set(xlim=(xmin, xmax), ylim=(ymin, ymax))`

参数如下：

<center>
<table style="position:center;width:100%;">
<td>
<div class="table-responsive"><table class="table table-condensed"><colgroup><col class="tcol1" width="50%"><col class="tcol2" width="50%"></colgroup><thead><tr><th>线型</th><th>说明</th></tr></thead><tbody><tr><td><code class="literal remove_text_wrapping">-</code></td><td>实线（默认）</td></tr><tr><td><code class="literal remove_text_wrapping">--</code></td><td>虚线</td></tr><tr><td><code class="literal">:</code></td><td>点线</td></tr><tr><td><code class="literal remove_text_wrapping">-.</code></td><td>点划线</td></tr></tbody></table></div>
</td>
<td>
<div class="table-responsive"><table class="table table-condensed"><colgroup><col class="tcol1" width="50%"><col class="tcol2" width="50%"></colgroup><thead><tr><th>标记</th><th>说明</th></tr></thead><tbody><tr><td><code class="literal">o</code></td><td>圆圈</td></tr><tr><td><code class="literal">+</code></td><td>加号</td></tr><tr><td><code class="literal">*</code></td><td>星号</td></tr><tr><td><code class="literal">.</code></td><td>点</td></tr><tr><td><code class="literal">x</code></td><td>叉号</td></tr><tr><td><code class="literal">s</code></td><td>方形</td></tr><tr><td><code class="literal">d</code></td><td>菱形</td></tr><tr><td><code class="literal">^</code></td><td>上三角</td></tr><tr><td><code class="literal">v</code></td><td>下三角</td></tr><tr><td><code class="literal">&gt;</code></td><td>右三角</td></tr><tr><td><code class="literal">&lt;</code></td><td>左三角</td></tr><tr><td><code class="literal">p</code></td><td>五角形</td></tr><tr><td><code class="literal">h</code></td><td>六角形</td></tr></tbody></table></div>
</td>
<td>
<div class="table-responsive"><table class="table table-condensed"><colgroup><col class="tcol1" width="50%"><col class="tcol2" width="50%"></colgroup><thead><tr><th>颜色</th><th>说明</th></tr></thead><tbody><tr><td><p id="bt2rawq-2"><code class="literal">y</code></p></td><td>黄色</td></tr><tr><td><p id="bt2rawq-5"><code class="literal">m</code></p></td><td>品红色</td></tr><tr><td><p id="bt2rawq-8"><code class="literal">c</code></p></td><td>青蓝色</td></tr><tr><td><p id="bt2rawq-11"><code class="literal">r</code></p></td><td>红色</td></tr><tr><td><p id="bt2rawq-14"><code class="literal">g</code></p></td><td>绿色</td></tr><tr><td><p id="bt2rawq-17"><code class="literal">b</code></p></td><td>蓝色</td></tr><tr><td><p id="bt2rawq-20"><code class="literal">w</code></p></td><td>白色</td></tr><tr><td><p id="bt2rawq-23"><code class="literal">k</code></p></td><td>黑色</td></tr></tbody></table></div>
</td></table>
</center>

- 参数可以不是列表，而是`np.array`，画图格式基本与`matlab`相同，即`plot(x1,y1,form1,x2,y2,form2,...)`绘制在一张图上。

- 参数可以是分类变量：

```python
names = ['group_a', 'group_b', 'group_c']
values = [1, 10, 100]

plt.figure(figsize=(9, 3))

plt.subplot(131)
plt.bar(names, values)
plt.subplot(132)
plt.scatter(names, values)
plt.subplot(133)
plt.plot(names, values)
plt.suptitle('Categorical Plotting')
plt.show()
```

- 也可以使用`setp`在之后设置格式，类似`matlab`，键值对和属性列举都可以：
```python
lines = plt.plot(x1, y1, x2, y2)
# use keyword args
plt.setp(lines, color='r', linewidth=2.0)
# or MATLAB style string value pairs
plt.setp(lines, 'color', 'r', 'linewidth', 2.0)
```

- `clf`清除当前图形，`cla`清除当前轴

- `matplotlib`在任何文本表达式中接受$TeX$方程表达式。

- 在图上添加文字注释：

```python
plt.annotate('local max', xy=(2, 1), xytext=(3, 1.5),
             arrowprops=dict(facecolor='black', shrink=0.05),
             )
```

- `matplotlib.pyplot`不仅支持线性轴刻度，还支持对数和对数刻度。如果数据跨多个数量级，则通常使用此方法。更改轴的比例很容易：`plt.xscale('log')`，`plt.yscale('symlog', linthresh=0.01)`，`plt.yscale('logit')`