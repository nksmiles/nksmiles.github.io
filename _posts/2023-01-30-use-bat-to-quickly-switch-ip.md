---
layout: mypost
title: Windows 使用批处理快速切换IP地址
categories: [Windows,Network]
---

因为调试原因，笔记本要经常在不同的 IP 配置之间来回使用，两边都是使用静态IP地址，除了 DNS 都要重新设置，很不方便，就做了这个批处理，可以用于 IP 地址切换。

已在 XP 和 Server 2003 操作系统下测试过，其他 Windows 系统应该兼容。

以下是批处理文件的内容：

```cmd
@echo off
title IP地址快速切换
::color 1f
::变量默认设置
set INTERFACE=本地连接
:menu
cls
echo    ┏━━━━━━━━━━━━━━━━━┓
echo    ┃ IP 地址快速切换 ┃
echo    ┗━━━━━━━━━━━━━━━━━┛
echo.
echo   1. 切换IP地址为 10.10.112.150   (调试用)
echo.
echo   2. 切换IP地址为 202.113.227.142 (上网用)
echo.
echo   3. 退出
echo.
set /p key= 请输入您的选择[1/2/3]: 
if "%key%" == "" goto error
if "%key%" == "1" goto dormIP
if "%key%" == "2" goto expIP
if "%key%" == "3" exit
goto error
:error
echo.
echo 对不起，您的输入有误!
echo 请按任意键返回...
pause >nul
goto menu
:dormIP
echo.
echo 正在设置IP地址，请稍等...
netsh interface ip set address name="%INTERFACE%" source=static addr=10.10.112.144 mask=255.255.255.0
echo 正在设置默认网关，请稍等...
netsh interface ip set address name="%INTERFACE%" source=static gateway=10.10.112.1 gwmetric=0
echo 设置完毕，请按任意键返回...
pause > nul
goto menu
:expIP
echo.
echo 正在设置IP地址，请稍等...
netsh interface ip set address name="%INTERFACE%" source=static addr=202.113.227.142 mask=255.255.255.192
echo 正在设置默认网关，请稍等...
netsh interface ip set address name="%INTERFACE%" source=static gateway=202.113.227.129 gwmetric=0
echo 设置完毕，请按任意键返回...
pause > nul
goto menu

```
将以上内容使用记事本保存为.bar或者.cmd文件，以管理员身份运行即可。 

![ip](ip.png)

当然，如果想设置动态IP，只要相应的更改为下面的命令就可以了：

```cmd
netsh interface ip set address name="%INTERFACE%" source=dhcp
netsh interface ip set address name="%INTERFACE%" gateway=none
```

这里的INTERFACE变量是“本地连接”，在上面的批处理文件开始的地方已经定义，这样如果有“本地连接2”或者其它名称的连接也比较容易修改。


除了用批处理文件运行netsh命令外，还有用netsh dump保存网络配置的方法：

```cmd
netsh interface dump > NetConfig.cfg
```

然后用netsh exec恢复网络设置：

```cmd
netsh exec NetConfig.cfg
```

## 另一个解决方案

[IP配置工具_2.6__一键切换IP、改Mac、计算机名](https://www.52pojie.cn/thread-1368052-1-1.html)
