# 基础知识

## pyplot与pylab

pyplot是matplotlib的子模块，官方文档建议使用plt创建Figure，然后用面向对象方法绘图；pylab是另一个子模块，目前已经废弃，不建议使用

## 导入

```python
import matplotlib.pyplot as plt

plt.rcParams['font.sans-serif']=['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus']=False    # 用来正常显示负号
```



## Coding Style

plt中有所谓的"current figure"和"current axes"，使用模块级别的函数时，就是操作当前对象。这种方法与MATLAB的绘图一致，被称为MATLAB style

```python
plt.plot([1, 2, 3])
plt.title('example')
plt.show()
```

当然，也可以先显式创建figure和axes，然后对它们进行操作，称作Object Oriented style

```python
fig, ax = plt.subplots()
ax.plot([1, 2, 3])
ax.set_title('example')
plt.show()
```

两种方法相比，显然OO更好。Explicit is better than implicit



# 图片元素

![](./images/parts_of_figure.webp)

## Figure

Figure包含了图像的所有元素，包括Axes，少数特殊的Artists，以及Canvas（注意用户一般不会直接绘制Canavs，而是通过其他对象来操作Canvas）

```python
fig = plt.figure()
fig.suptitle('example')
```

## Axes

Axes是主要的图像内容，包含了数据图线等元素。一个Figure中可以包含若干个Axes，一个Axes只能属于一个Figure。一个Axes中包含若干个Axis（二维图2个，三维图则3个），与每条Axis对应的label（`set_xlabel, set_ylabel`）以及一个title（`set_title`）

```python
# 使用figure.add_subplot建立axes
fig = figure()
ax1 = fig.add_subplot(1, 2, 1)  # 一行两列，一共给fig加了两个Axes，返回是第一个
ax2 = fig.add_subplot(1, 2, 2)  # 返回第二个Axes（Axes的排序方式是从左到右，从上到下）

ax1.set_title('axes 1')   # 注意每个Axes有自己的标题，figure还有一个大标题

# 使用plt.subplots建立axes
fig, ax = plt.subplots(3, 3)  # ax是Axes的array
```



## Axis

注意：英语中Axis是Axes的单数形式，但Axis和Axes是完全不同的两种对象

Axis是图像的轴，负责图像取值范围（可以用`axes.set_xlim`的方法从Axis所属Axes设置），刻度（tick，刻度位置由Locator对象决定）和刻度标签（ticklabel，刻度标签格式由Formatter对象确定）

## Artist

Artist包括了几乎所有图像元素。Figure, Axes, Axis对象也都是Artist，但多数Artist都被绑定至Axes对象，而不能被多个Axes共享。绘制图像时，所有Artist被画到Canvas上



# 绘制不同种类的图

## 折线图

plot([x], y, [fmt], ...)

绘制折线图，省略x则按照1, 2, ...，fmt参数可以调整绘制格式(fmt同MATLAB的格式)

重复多组x, y, fmt/多个plot函数，能够在一张图上绘制多个函数

fmt常用格式：rgbyk表示颜色，-直线，--虚线，-.点划线，.o+s^不同形状的点

## 散点图

（可以给不同的点赋予不同颜色）

plt.scatter(x, y)

## 直方图

plt.hist

## 柱状图

plt.bar

## 等高线图
plt.contour
plt.contourf

## 灰度图
plt.imshow

## 饼状图
plt.pie

## 矢量场图
plt.quiver

## 3D图
plt.plot_surface

## 误差杆
plt.errorbar

errorbar(x, y, yerr, xerr, fmt, ecolor, elinewidth, capsize, barsabove)

设置误差杆的线：eb[-1][0].set_linestyle(fmt)，eb是Errorbar Container



# 绘图参数

```
setp        修改图线参数(粗细，颜色，etc.)，并返回其当前参数
            example:
            line = plt.plot([1, 2, 3])
            plt.setp(line, linesidth=2.0)
            也可以调用line的方法调整
xlabel, ylabel
            坐标轴的标签
xscale, yxcale
            对数坐标等，"linear", "log", "symlog", "logit", ...
title       标题
legend      添加图例，在需要图例的plot函数添加label参数
annotate(s, xy, *args, **kwargs)
            添加注释
            s       str, 注释内容
            xy      (float, float), 注释图线上哪个点
            xytext  (float, float), 注释的位置，默认在xy
            还有很多调整显示格式的参数
text(x, y, s)
            在(x, y)处插入文字s，返回instance of Text
            xlabel, title等其实都是text的wrapper
```



# 轴参数

```
axis(*v, **args)
    *v是字符串指令列表，**args = [xmin, xmax, ymin, ymax]调整spines的上下限
    返回(xmin, xmax, ymin, ymax)
xlim(left, right)
ylim(left, right)
    设置坐标上下限，返回当前上下限
xticks(ticks=None, labels=None)
yticks(ticks=None, labels=None)
    ticks, labels都是array-like，标识坐标轴上那些位置显示值、显示什么
    e.g. np.xticks([np.pi/2, np.pi], ['$\frac{\pi}{2}$', '$\pi$'])
    返回当前的ticks, label. 如xticks([])可以清除当前设置
```



# 其他函数

```
show        显示图形
savefig     保存为文件，参数filename, format, dpi等
gca         get current axes
gcf         get current figure
cla         clear current axes
clf         clear current figure (注意：figure.close()才能完全释放内存)
sca         set current axes，把指定的Axes实例设置为活跃区的Axes
grid        绘制网格
```

# 绘图区域相关

```
figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None,
       frameon=True, FigureClass=<class 'matplotlib.figure.Figure'>,
       clear=False, **kwargs)
    create a new figure
    num figure的编号，int, string, etc. 默认为递增整数，可以通过fig.number访问
        若给的num已经存在，returns a reference to it;
        否则，创建一个新figure并返回
    figsize (float, float)，figure大小(inches)
    dpi int
    clear   如果图已经存在，且clear=True则清除其内容

subplot(*args, **kwargs)
    给当前figure加一个subplot，wrapper of Figure.add_subplot
    *args
        三位整数或者三个整数，表示子图的位置(nrols, ncols, index)
        nrols, ncols表示行列各有几张子图
        index表示摆放次序，取1, 2, ..., nrols*ncols
    projection
        取什么坐标系，None, 'aitoff', 'hammer', 'lambert', 'mollweide',
        'polar', 'rectilinear'
    polar
        polar=True等于projection='polar'
    sharex, sharey
        axes, 使用提供的sxes
    label
        str, 返回axes的标签
    自己的理解：把当前绘图活跃区换到指定的位置

subplots(nrows=1, ncols=1, sharex=False, sharey=False, squeeze=True,
         subplot_kw=None, gridspec_kw=None, **fig_kw)
    创建一个新figure以及它的若干subplot
    返回Figure, Axes或Axes列表
    自己的理解：创建新的绘图区域，指定有几个子图。figure(figsize=(6.8, 8))等可以调尺寸，画完了之后用tight_layout可以自动排版
```


