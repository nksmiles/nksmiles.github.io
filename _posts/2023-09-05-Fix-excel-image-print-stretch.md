---
layout: mypost
title: 修复 Excel 打印或者转为 PDF 时的图片的拉伸问题
categories: [Excel]
---

## 问题

Excel 中的图片在打印或者转为 PDF 后看起来都在宽度方向上被拉长了。遇到这个问题很久了，之前也用百度、必应、Google 搜索中解决办法，但是中文搜索结果中大多是推荐各种软件、驱动的，并没有真的解决问题，这一直这么凑合着用。

今天突然想起来，Excel 这么多年一直有这个问题，也许国外也有讨论，就用英文搜索了一下，果然，在国外这个 Excel 论坛找到了：

[https://www.excelforum.com/excel-general/889534-excel-will-not-print-images-at-correct-height-width.html](https://www.excelforum.com/excel-general/889534-excel-will-not-print-images-at-correct-height-width.html)

## 解决方法

### 办法 1：简单办法

最简单的办法是更换主题或者字体主题：

在 Excel 页面布局选项卡的“主题”下面，选择“大都会”主题，或者“都会”字体主题：

![image](theme1.png)

如果是较早版本的 Excel，有 Metro （都会）字体主题的话，可以直接选择字体主题。
如果是 Office 365 （Microsoft 365），字体主题下面没有 Metro（都会），可以选择“主题”下面的 Metropolis（大都会）主题。

### 办法 2：自定义字体主题

将主题改为“大都会”后发现，字体主题不是列表中的几个，选择自定义主题，发现西文字体被设置为 Calibri Light，中文字体被设置为 宋体。

![image](theme2.png)

个人觉得用 宋体 显示数字和字母实在太难看：

![image](font1.png)

根据上面帖子提到的，将字体设置为 Segoe UI 也可以正常比例打印图像，于是自定义将中文字体设置为 Segoe UI。
行、列的数字和字母显示和打印、转PDF的图片比例问题就都解决了。

![image](theme3.png)
