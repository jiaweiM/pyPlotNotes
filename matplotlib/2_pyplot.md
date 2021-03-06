# Pyplot

- [Pyplot](#pyplot)
  - [简介](#简介)
    - [样式设置](#样式设置)
  - [关键字字符串绘图](#关键字字符串绘图)
  - [分类变量](#分类变量)
  - [线段属性设置](#线段属性设置)

2020-07-01, 20:58
***

## 简介

`matplotlib.pyplot` 包含一系列类似 MATLAB 的绘图函数。每个 `pyplot` 函数对 figure 进行一些修改，如创建 figure，在 figure 中创建 plot，在 plot 中添加线段，添加标签等。

`matplotlib.pyplot` 在不同函数调用时，状态保持不变，它记录了当前 figure 和 plotting area，以及对当前 axes 作用的绘图函数。

pyplot 主要用于交互式绘图以及较为简单的绘图，对复杂的绘图，还是建议为面向对象 API。且 `pyplot` 的大部分函数 `Axes` 对象也有。

使用 pyplot 生成 figure 很快：

```py
import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4])
plt.ylabel("some numbers")
plt.show()
```

如果只为 `plot()` 函数提供单个列表或数组时，matplotlib 假定其为 y 值，并以索引作为 x 值，所以此处 x 为 [0, 1, 2, 3]。

![line](images/2020-07-01-21-07-41.png)

`plot()` 是一个通用命令，可以使用多个参数。例如：

```py
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
```

![line](images/2020-07-01-21-53-35.png)

### 样式设置

对每对 x, y 参数，都有一个可选的格式字符串，用于设置线条类型和颜色。格式字符串来自 MATLAB。默认为 `'b-'`，表示蓝色实线。

例如，下面绘制红色圆圈：

```py
plt.plot([1, 2, 3, 4], [1, 4, 9, 16], 'ro')
plt.axis([0, 6, 0, 20])
plt.show()
```

`axis()` 用于指定范围，格式为 `[xmin, xmax, ymin, ymax]`。

![scatter](images/2020-07-01-21-57-02.png)

matplotlib 内部使用 numpy 存储数据，提供的其它类型数据在内部也被转换为 ndarray。

下面使用一个命令绘制不同样式的多条线：

```py
import numpy as np

# evenly sampled time at 200ms intervals
t = np.arange(0., 5., 0.2)

# red dashes, blue squares and green triangles
plt.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
plt.show()
```

`np.arange(start, end, step)` 生成一个 `ndarray`，然后使用 `plot()` 函数绘制 $y=x$, $y=x^2$, $y=x^3$。

'r--' 对应红色虚线，'bs' 对应红色方块，'g^' 对应绿色三角。

![line](images/2020-07-01-22-09-29.png)

## 关键字字符串绘图

有些数据类型可以通过变量名称访问数据，例如 `numpy.recarray` 和 `pandas.DataFrame`。

Matplotlib 允许通过 `data` 关键字参数提供这类数据，然后通过特定变量的字符串绘图。

```py
data = {'a': np.arange(50),
        'c': np.random.randint(0, 50, 50),
        'd': np.random.randn(50)}
data['b'] = data['a'] + 10 * np.random.randn(50)
data['d'] = np.abs(data['d']) * 100

plt.scatter('a', 'b', c='c', s='d', data=data)
plt.xlabel('entry a')
plt.ylabel('entry b')
plt.show()
```

![scatter](images/2020-07-01-22-22-05.png)

## 分类变量

Matplotlib 可以直接使用分类变量绘图：

```py
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

`subplot(131)` 表示1行3列的第一个。

![subplot](images/2020-07-01-22-24-24.png)

## 线段属性设置

线段的属性包括：linewidth, dash style, antialiased 等；线段由 [`matplotlib.lines.Line2D`](https://matplotlib.org/api/_as_gen/matplotlib.lines.Line2D.html#matplotlib.lines.Line2D) 类表示。

设置 Line2D 属性的方法有三种：

1. 使用关键字参数

```py
plt.plot(x, y, linewidth=2.0)
```

2. 使用 `Line2D` 的 `setter` 方法

`plot()` 函数返回 `Line2D` 对象列表，例如 `line1, line2 = plot(x1, y1, x2, y2)` 
