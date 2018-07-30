# matplotlib

## 中文显示问题

每次编代码时都进行参数设置如下：

```python
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif']=['SimHei'] #用来正常显示中文标签
plt.rcParams['axes.unicode_minus']=False #用来正常显示负号
```



##折线图

title

xlabel

ylabel

####设置刻度标记的大小

tick_params(axis='', labelsize=) axis参数是指`x轴y轴`或者两者 labelsize是指刻度大小



## 散点图

scatter(x, y, c='',  edgecolor='', s ='') c是颜色 edgecolor是点的轮廓颜色 s是点的大小

颜色渐变： plt.scatter(x, y, c=y, cmap=plt.cm.Blues)

 ## 柱状图

 plt.bar(x, y)

## 饼图

plt.pie(x)

## 直方图

## 产生多个图

plt.figure(num)给当前图编号

plt.subplot(221)表示当前绘图位置是4个块中的第一个

每次切换位置需要声明 使用plt.subplot()