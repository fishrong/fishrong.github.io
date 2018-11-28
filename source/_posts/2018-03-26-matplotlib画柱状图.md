---
layout: posts
title: 使用matplot画柱状图
categories: python学习笔记
tags: 
    - python
    - matplot
---
## 代码示例
```
import matplotlib.pyplot as plt

data = [5, 20, 15, 25, 10]
labels = ['Tom', 'Dick', 'Harry', 'Slim', 'Jim']#横坐标标记
xx=range(len(data))
plt.bar(range(len(data)), data, tick_label=labels)
# bar(left, height, width=0.8, bottom=None, hold=None, **kwargs)  
# 绘制柱形图  
# left:柱形图的x坐标  
# height柱形图的高度，以0.0为基准  
# width:柱形图的宽度，默认0.8  
# facecolor:颜色  
# edgecolor:边框颜色n  
# bottom:表示底部从y轴的哪个刻度开始画  
# yerr:应该是对应的数据的误差范围，加上这个参数，柱状图头部会有一个蓝色的范围标识,标出允许的误差范围,在水平柱状图中这个参数为xerr  
for x,y in zip(xx,data):#用来在显示每一条的上方显示y值
    plt.text(x,y+0.05,'%.2f' % y,ha='center',va='bottom')
    #x, y+0.05表示在每一柱子对应x值、y值上方0.05处标注文字说明， '%.0f' % y,代表标注的文字，即每个柱子对应的y值， ha='center', va= 'bottom'代表horizontalalignment（水平对齐）、verticalalignment（垂直对齐）的方式
plt.title('学生成绩')
plt.savefig(r'F:\QQzone_spider\\' +r'\\'+name+ ".png")#图片保存路径,在show()之前
plt.show()
```
## 效果如图:
<!-- more -->
![柱状图](/images/bar1.png)

## 解决matplotlib中文乱码问题
进入Python安装目录下的Lib\site-packages\matplotlib\mpl-data目录，打开matplotlibrc文件，删除font.family和font.sans-serif两行前的#，并在font.sans-serif后添加字体（SimHei），代码如下：
```
font.sans-serif     : SimHei,Microsoft YaHei,DejaVu Sans, Bitstream Vera Sans, Computer Modern Sans Serif, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif
```
