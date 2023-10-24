---
layout: mypost
title: 在 OneDrive 中禁用“使用 Office 应用程序同步我打开的 Office 文件”
categories: [OneDrive]
---

OneDrive 老版本中有一个文件协作设置选项，可以禁用“使用 Office 应用程序同步我打开的 Office 文件”。但在新版的 OneDrive 中，如果之前这个选项是禁用的，那么新版本会提示你启用该功能，一旦该功能被启用，在新版本的 OneDrive 中这个选项就消失了……在新版本就找不到禁用的选项了。

![image](oldOneDrive.png)

新版本中，一旦该功能被启用，这个选项就会消失：

![image](newOneDrive1.png)
![image](newOneDrive2.png)

# 解决办法 1

找到了下面这个帖子：
[新版 OneDrive 如何使 “使用 office 同步 office 文件” 或” 文件协作 “ 保持关闭？](https://answers.microsoft.com/zh-hans/msoffice/forum/all/%E6%96%B0%E7%89%88onedrive%E5%A6%82%E4%BD%95/13b6132a-7548-41d1-a35c-d3022c5a9b66)

目前的解决办法是彻底卸载新版本OneDrive，安装旧版本OneDrive，然后禁用该功能。

这样升级到新版本后会 同步并备份 - 高级设置 中会出现“文件协作”，并且是关闭的。

请注意，这里如果启用文件协作，这个选项会直接消失。

# 解决办法 2 ???

今天在查看 OneDrive 文档的时候发现了微软如下说明：

[Coauthor and share in Office desktop apps](https://learn.microsoft.com/en-us/sharepoint/use-group-policy?redirectSourcePath=%252farticle%252f8a409b0c-ebe1-4bfa-a08e-998389a9d823#coauthor-and-share-in-office-desktop-apps)

```
[HKCU\SOFTWARE\Policies\Microsoft\OneDrive] "EnableAllOcsiClients"=dword:00000000
```
可惜搜遍注册表，并没有找到……

# 为什么要禁用这个功能

这个功能会导致每次保存Office文件时都需要联网存储，经常遇到网络环境不佳的时候，导致打开、存储文件异常缓慢。

这个功能禁用后，本地保存的 Office 文件，OneDrive 还是会同步到网盘，就如其他非 Office 文件一样。
