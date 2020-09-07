---
title: 凿刻文字效果
date: 2016-10-14 20:11:48
cover: https://i.loli.net/2020/02/19/1qzWI9Dr4JAKXx6.jpg
categories: 
     - 设计
---

前些日子给博客随意做了一个 `Logo`，上图是素模渲染效果。文字使用凿刻加挤压的复合效果，细节丰富，层次感强烈。凿刻文字效果曾困扰过笔者不少时日，现在分享一下个人摸索的制作方法。

<!-- more -->

> **凿刻文字（Chiseled Text）**是比较常见的一种文字效果，多用于电影、海报和广告的标题。这种效果具有视觉冲击力的同时又能彰显出高贵优雅的气质，一般搭配字面和胸线较大的字体使用，其中 [无衬线体](https://zh.wikipedia.org/wiki/%E6%97%A0%E8%A1%AC%E7%BA%BF%E4%BD%93) 和 [黑体](https://zh.wikipedia.org/wiki/%E9%BB%91%E4%BD%93_(%E5%AD%97%E4%BD%93)) 制作起来比较简单。电影[《霍比特人》](https://zh.wikipedia.org/wiki/%E5%93%88%E6%AF%94%E4%BA%BA)的标题就采用了这种效果。

平面的凿刻文字效果非常简单，借助 `Photoshop` 和 `Illustrator` 即可轻易完成。三维立体的凿刻文字效果相对而言麻烦很多，但是得到的效果却是惊人的，接下来笔者就来简述一下流程。

***

首先在 `Illustrator` 中绘制出文字的骨架线，然后再以此为依据绘制出 *{% ruby 外轮廓|Outer Lines %}*。由于骨架线不是闭合曲线，所以需要对其 *{% ruby 轮廓化描边|Outline Stroke %}* 以获得闭合曲线，称之为 *{% ruby 内轮廓|Inner Lines %}*。（如果使用字体直接获得 *{% ruby 外轮廓|Outer Lines %}*，则需要 *{% ruby 扩展|Expend %}* 来转曲，并自行勾勒骨架线。）获得内外轮廓线后需要统筹好点的数量和分布位置，做到一一对应。最后保存一份当前版本的 `.ai` 文件备用，另存为一份 `Illustrator 8` 版本的 `.ai` 文件供三维软件使用。

![Chiseled_Text_Basic_Lines.png](https://i.loli.net/2020/02/19/tCjOs27eHKR9BQz.png)

在 `Cinema 4D` 中导入 `Illustrator 8` 版本的 `.ai` 文件后，将中心点重置到几何中心，并检验内外轮廓是否独立。检验无误后进入点编辑模式，**将内外轮廓的起始点设置为对应的点，并保证点序列的方向一致**。添加一个 *{% ruby 放样曲面|Loft Nurbs %}* 作为内外轮廓的父级，内轮廓置于上方，并沿法线方向平移一定距离，即可完成效果。最后调整系列参数直到满意为止。

![m_chiseled_final.jpg](https://i.loli.net/2020/02/19/F3HrotMRZQ6zN2K.jpg)
***

> 附 [工程文件](http://meroco.d.pr/111tM) 供参考学习。
