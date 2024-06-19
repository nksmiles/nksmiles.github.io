---
layout: mypost
title: Windows 显示语言设置为英文时简体中文显示异常的解决办法
categories: [Windows]
---

Windows 显示语言设置为英文时简体中文显示异常，尤其是 Edge 的标题栏，辣眼睛，没法看……

网上有很多介绍修改 FontLink 注册表的，Windows 11 22H2 实测无效。

按以下步骤操作解决问题：

1. 备份注册表 `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts`
2. 删除 Microsoft JhengHei （繁中）开头的 3 个键、Malgun（韩文）开头的 3 个键和 Yu Gothic（日文） 开头的 4 个键
3. 重启

这个方法会导致部分繁体中文和日、韩文显示异常。不过日常用不到，就这样解决了。

## 参考

[Windows 显示语言设置为英文时简体中文以日文或繁体中文字形显示的解决办法](https://zhuanlan.zhihu.com/p/502139239)
