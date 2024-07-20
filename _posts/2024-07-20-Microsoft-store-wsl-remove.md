---
layout: mypost
title: 新版 Microsoft Store 中的 Windows Subsystem for Linux 彻底移除办法
categories: [前端]
---

此前 Windows 10、11可以在 `启用或关闭 Windows 功能` 中通过取消勾选 `适用于 Windows 的 Linux 子系统`来彻底移除wsl。

但近期（2024年7月）在Windows 10和11上都发现无法通过上述方法彻底删除wsl。取消勾选应用后桌面仍有一个企鹅Linux图标。
进入Microsoft Store，选择左侧下部的“库”，能看到 `Windows Subsystem for Linux`，通过Microsoft Store无法删除。

实测可以通过以下办法彻底移除：

1. 以管理员身份运行CMD或者POWERSHELL；
2. 运行wmic
```
wmic
```
3. 在wmic中运行命令显示程序名：
```
product get name
```
4. 如果软件名中有`Windows Subsystem for Linux`，运行以下命令移除：
```
product where name="Windows Subsystem for Linux" call uninstall
```
5. 输入Y，确认删除软件
```
Y
```
6. 成功移除后会显示成功的提示：
```
Method execution successful.
```
也可以通过再次运行命令查看程序名中是否还有`Windows Subsystem for Linux`：
```
product get name
```
通过以上操作彻底移除wsl后，桌面上的企鹅Linux图标就删除了。
