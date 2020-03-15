```python
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif']=['SimHei']  #用来正常显示中文标签
plt.rcParams['axes.unicode_minus']=False    #用来正常显示负号
```

# 函数

## 绘制不同种类的图

```
plot([x], y, [fmt], ...)
    绘制折线图，省略x则按照1, 2, ...，fmt参数可以调整绘制格式(fmt同MATLAB的格式)
    重复多组x, y, fmt/多个plot函数，能够在一张图上绘制多个函数
    fmt常用格式：rgbyk表示颜色，-直线，--虚线，-.点划线，.o+s^不同形状的点
    返回matplotlib.lines.Line2D对象
scatter(x, y, ...)
    绘制散点图（可以给不同的点赋予不同颜色）
hist
    绘制直方图(histogram)
bar 绘制柱状图
contour, contourf
    等高线图
imshow
    灰度图
pie 饼状图
quiver
    矢量场图
plot_surface
    3D图
errorbar(x, y, yerr, xerr, fmt, ecolor, elinewidth, capsize, barsabove)
    误差杆
    设置误差杆的线：eb[-1][0].set_linestyle(fmt)，eb是Errorbar Container
```



## 绘图参数

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



## 轴参数

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



## 其他函数

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

## 绘图区域相关

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



# 类

## Figure类

```
matplotlib.figure.Figure(figsize=None, dpi=None, facecolor=None,
                         edgecolor=None, linewidth=0.0,frameon=None,
                         subplotpars=None, tight_layout=None,
                         constrained_layout=None)
    The top level container for all the plot elements
    Attributes
        （略）
    methods
        （略）
```



## Axes类

```
matplotlib.axes.Axes(fig, rect, facecolor=None, frameon=True,
                     sharex=None, sharey=None, label='',
                     xscale=None, yscale=None, **kwargs)
    包含了大部分图的内容（Axis, Tick, Line2D, Text, Polygon, etc.）以及坐标
```

