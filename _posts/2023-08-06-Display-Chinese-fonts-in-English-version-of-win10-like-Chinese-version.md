---
layout: mypost
title: Windows 10 英文版下中文字体如何像 Windows 中文版一样显示？
categories: [Windows]
---

具体问题可以参考这篇文章：
[完美解决中文在英文Windows上显示高矮不一的问题](https://shajisoft.com/shajisoft_wp/cn/%E5%AE%8C%E7%BE%8E%E8%A7%A3%E5%86%B3%E4%B8%AD%E6%96%87%E5%9C%A8%E8%8B%B1%E6%96%87windows%E4%B8%8A%E6%98%BE%E7%A4%BA%E9%AB%98%E7%9F%AE%E4%B8%8D%E4%B8%80%E7%9A%84%E9%97%AE%E9%A2%98/)

这个问题可以使修改 FontLink 相关注册表文件来解决，但修改起来比较麻烦。参考这篇文章，
[win10英文版下中文字体如何像中文版一样显示？](https://www.zhihu.com/question/58055812)
在英文 Windows 10 下，可以通过更改语言的方式让系统来自动修改 FontLink。 

在区域设置中，直接启用UTF-8支持特性就可以了：

控制面板-区域与语言-区域，非Unicode（non Unicode）选项

![01](01.png)

修复后的效果

![02](02.png)

建议重点：<font color=red>先选择 Chinese(Traditional)，重启后修改回 Chinese(Simplified)，再次重启</font>后成功修复。
