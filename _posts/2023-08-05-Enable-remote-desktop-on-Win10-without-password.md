---
layout: mypost
title: 在 Windows 10 上，为未设置密码的本地账户启用远程桌面
categories: [Windows]
---

## 说明

使用过各种第三方远程桌面，用来用去，还是Windows 自带的远程桌面最好用。

> Windows 远程桌面是一种使用 RDP 协议的远程控制功能，它可以让您在本地计算机上访问和操作远程计算机的桌面和应用。Windows 远程桌面相比其他第三方远程桌面软件有以下几个优点：
> - Windows 远程桌面是 Windows 系统自带的功能，您不需要额外下载或安装其他软件，只需在远程和本地计算机上进行简单的设置，就可以开始使用。
> - Windows 远程桌面支持多用户多会话的远程控制，这意味着您可以在同一台远程计算机上创建多个账户，并且每个账户都可以同时进行独立的远程会话，而不会影响其他用户的操作。
> - Windows 远程桌面支持集成式 RemoteApp 会话，这是一种将单个远程应用集成到本地桌面的方式，让您感觉就像在本地运行该应用一样。这种方式可以节省网络带宽，提高运行效率，并且可以与本地应用进行交互。
> - Windows 远程桌面支持动态分辨率和智能调整大小的功能，这意味着您可以根据本地显示器的分辨率和方向，以及客户端窗口的大小，自动调整远程桌面的显示效果，从而获得最佳的视觉体验。
> - Windows 远程桌面支持多重身份验证，这是一种增强远程连接安全性的方式，要求您在输入用户名和密码之外，还需要提供其他形式的验证信息，例如手机验证码、指纹扫描等。

当然，IPv4 如果没有公网 IP，非局域网访问就变得很麻烦，这是 Windows 自带的远程桌面不方便的一点。但也可以用 Zerotier 等做内网穿透使用。

## 办法

默认设置下，如果本地账户没有设置密码，启用远程桌面后也无法连接，下面是解决办法：

1. 在要被远程访问的计算机上，打开“设置”-“系统”-“远程桌面”，并开启“启用远程桌面”选项。

![image](remoteDesktop1.png)

2. 在同一台计算机上，按下Win+R键，输入secpol.msc命令，打开本地安全策略管理器。

3. 在左侧列表中，依次展开“本地策略”-“安全选项”，在右侧窗口中，找到“账户：使用空密码的本地账户只允许进行控制台登录”这一项，双击它，选择“已禁用”，然后点击“确定”。

![image](remoteDesktop2.png)

4. 这样就完成了设置，您可以使用空密码登录远程桌面了。
