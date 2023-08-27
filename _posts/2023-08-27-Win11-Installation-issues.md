---
layout: mypost
title: Windows 11 安装过程中遇到的问题及解决办法汇总
categories: [Windows]
---

## 问题1：这台电脑无法运行 Windows 11

![image](win11-1.png)

在这个界面同时按下 SHIFT + F10，打开命令行界面。输入 `regedit` 进入注册表。

进入：
```
计算机\HKEY_LOCAL_MACHINE\SYSTEM\Setup
```

新建项，注意大小写：
```
LabConfig
```

在新建的项下新建 DWORD 值：
```
BypassTPMCheck = 1
BypassSecureBootCheck = 1
```

回到上一步，然后继续安装即可。

## 问题2：让我们为你连接到网络

