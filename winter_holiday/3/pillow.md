# Pillow

参考[官方文档](https://pillow.readthedocs.io/en/stable/)&nbsp;&nbsp;&nbsp;author:高梓源&nbsp;&nbsp;&nbsp;date:2.25

[点击参阅Matplotlib Library](Matplotlib.html)

---

[TOC]

# Quick Start

## 打开保存图像

`Python Imaging Library`(PIL)

```python
>>> from PIL import Image
>>> im = Image.open("F://Pictures/background/1.jpg")
```

如果成功，此函数将返回一个Image对象。可以使用实例属性来检查文件内容：

```python
>>> print(im.format, im.size, im.mode)
JPEG (1920, 1080) RGB
```

如果无法打开文件，OSError则会引发异常。

- `format`属性标识图像的来源。如果未从文件读取图像，则将其设置为”None"。
- `size`属性是一个包含宽度和高度（以像素为单位）的2元组。
- `mode`属性定义图像中条带的数量和名称，以及像素类型和深度。常见模式是灰度图像的"L"，真彩色图像的"RGB"和印前图像的"CMYK"。

显示刚刚加载的图像：

```python
>>> im.show()
```

`open()`方法打开文件，`save()`方法保存文件（保有格式，可用于文件转换）


### 将文件转换为JPEG
注：输入`python -u "d:\pycode\vscode\pillow.py" F://Pictures/background/1.jpg`以运行

```python
import os, sys
from PIL import Image

for infile in sys.argv[1:]:
    f, e = os.path.splitext(infile)
    outfile = f + ".jpg"
    if infile != outfile:
        try:
            with Image.open(infile) as im:
                im.save(outfile)
        except OSError:
            print("cannot convert", infile)
```

### 创建JPEG缩略图

```python
from PIL import Image

size = (128, 128)

for infile in sys.argv[1:]:
    outfile = os.path.splitext(infile)[0] + ".thumbnail"
    if infile != outfile:
        try:
            with Image.open(infile) as im:
                im.thumbnail(size)
                im.save(outfile, "JPEG")
        except OSError:
            print("cannot create thumbnail for", infile)
```

### 识别图像文件

```python
import sys
from PIL import Image

for infile in sys.argv[1:]:
    try:
        with Image.open(infile) as im:
            print(infile, im.format, f"{im.size}x{im.mode}")
    except OSError:
        pass
```

### 剪切，粘贴和合并图像

#### 从图像复制子矩形

```python
box = (100, 100, 400, 400)
region = im.crop(box)
```
该区域由一个四元组定义，坐标为（左，上，右，下），以像素为单位，**注意PIL使用的坐标系的左上角为$(0，0)$**

#### 处理子矩形，然后将其粘贴回

```python
region = region.transpose(Image.ROTATE_180)
im.paste(region, box)
```
粘贴区域时，区域的大小必须与给定区域完全匹配。
但是，原始图像的模式和区域不需要匹配。在粘贴之前会自动转换区域。

#### 例：滚动图像

```python
def roll(image, delta):
    """Roll an image sideways."""
    xsize, ysize = image.size

    delta = delta % xsize
    if delta == 0: return image

    part1 = image.crop((0, 0, delta, ysize))
    part2 = image.crop((delta, 0, xsize, ysize))
    image.paste(part1, (xsize-delta, 0, xsize, ysize))
    image.paste(part2, (0, 0, xsize-delta, ysize))

    return image
```

#### 拆分颜色通道

```python
r, g, b = im.split()
im = Image.merge("RGB", (b, g, r))
```
对于单波段图像，split()返回图像本身。要使用各个色带，您可能需要先将图像转换为`RGB`。
`PIL.Image.merge(mode, bands)`参数为模式和一个包含每个波段的序列，返回值为一个`Image`对象。

### 几何变换

#### 简单

```python
out = im.resize((128, 128))
out = im.rotate(45) # degrees counter-clockwise
```

**注意`rotate()`参数单位为$\degree$，且为逆时针**

#### 转置影像

```python
out = im.transpose(Image.FLIP_LEFT_RIGHT)
out = im.transpose(Image.FLIP_TOP_BOTTOM)
out = im.transpose(Image.ROTATE_90)
out = im.transpose(Image.ROTATE_180)
out = im.transpose(Image.ROTATE_270)
```

可以通过该transform()方法执行图像变换的更一般形式 。

### 颜色转换

#### 模式间转换（可以直接另存？）

该库支持每种支持的模式与`L`和`RGB`模式之间的转换。要在其他模式之间转换，您可能必须使用中间图像（通常是`RGB`图像）。

```python
from PIL import Image
with Image.open("hopper.ppm") as im:
    im = im.convert("L")
```

### 影像增强

#### 筛选器

```python
from PIL import ImageFilter
out = im.filter(ImageFilter.DETAIL)
```
#### 点操作
`point()`方法可以用于转换图像的像素值（如图像对比度操纵）

```python
# multiply each pixel by 1.2
out = im.point(lambda i: i * 1.2)
```
使用`lamda`表达式

#### 例子

```python
# split the image into individual bands
source = im.split()

R, G, B = 0, 1, 2

# select regions where red is less than 100
mask = source[R].point(lambda i: i < 100 and 255)

# process the green band
out = source[G].point(lambda i: i * 0.7)

# paste the processed band back, but only where red was < 100
source[G].paste(out, None, mask)

# build a new multiband image
im = Image.merge(im.mode, source)
```

#### 增强影像

可以通过`ImageEnhance`模块调整对比度，亮度，色彩平衡和清晰度。

```python
from PIL import ImageEnhance

enh = ImageEnhance.Contrast(im)
enh.enhance(1.3).show("30% more contrast")
```

## 图像序列（动画格式）处理

支持的序列格式包括FLI/FLC,GIF,TIFF
当您打开序列文件时，PIL将自动加载序列中的第一帧

顺序翻阅：

```python
from PIL import Image

with Image.open("animation.gif") as im:
    im.seek(1) # skip to the second frame

    try:
        while 1:
            im.seek(im.tell()+1)
            # do something to im
    except EOFError:
        pass # end of sequence
```

若要使用for语句遍历序列：

```python
from PIL import ImageSequence
for frame in ImageSequence.Iterator(im):
    # ...do something to frame...
```

## 读取图像（几种方式）

```python
from PIL import Image
with Image.open("hopper.ppm") as im:
    ...

#从打开的文件中读取
from PIL import Image
with open("hopper.ppm", "rb") as fp:
    im = Image.open(fp)

#从二进制数据读取
from PIL import Image
import io
im = Image.open(io.BytesIO(buffer))
```