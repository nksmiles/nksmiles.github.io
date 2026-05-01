---
layout: mypost
title: 网络设置自动切换
categories: [Windows]
---

设备调试的网络是手动分配IP地址的，为了方便快速切换，做了脚本，方便切换。


## Powershell 版本

Powershell 脚本，保存为`WLAN切换.ps1`文件。

```powershell
<#
.SYNOPSIS
WLAN网卡配置切换脚本：DHCP模式 / 静态IP模式
#>

# 自动以管理员身份重启脚本
if (!([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)) {
    Start-Process powershell.exe "-NoProfile -ExecutionPolicy Bypass -File `"$PSCommandPath`"" -Verb RunAs
    exit
}

# 配置无线网卡名称（默认：WLAN，如你的网卡名不同请修改这里）
$wlanName = "WLAN"

# 清屏 + 菜单
Clear-Host
Write-Host "==================== WLAN 配置切换工具 ====================" -ForegroundColor Cyan
Write-Host "1. 启用 IPv6 + IPv4 DHCP 自动获取" -ForegroundColor Green
Write-Host "2. 禁用 IPv6 + IPv4 静态地址(172.28.86.85/24)" -ForegroundColor Yellow
Write-Host "=============================================================" -ForegroundColor Cyan

# 获取用户输入
$choice = Read-Host "请输入选择 [1 或 2]"

switch ($choice) {
    1 {
        Write-Host "`n[配置] 启用IPv6 + IPv4 DHCP..." -ForegroundColor Green
        
        # 启用WLAN的IPv6
        Enable-NetAdapterBinding -Name $wlanName -ComponentID ms_tcpip6
        
        # 设置IPv4为DHCP自动获取
        netsh interface ip set address name="$wlanName" source=dhcp
        netsh interface ip set dns name="$wlanName" source=dhcp
        
        Write-Host "[完成] 已切换为 DHCP 模式`n" -ForegroundColor Green
    }
    
    2 {
        Write-Host "`n[配置] 禁用IPv6 + 设置静态IP..." -ForegroundColor Yellow
        
        # 禁用WLAN的IPv6
        Disable-NetAdapterBinding -Name $wlanName -ComponentID ms_tcpip6
        
        # 设置静态IPv4、子网掩码、网关
        netsh interface ip set address name="$wlanName" source=static addr=172.28.86.85 mask=255.255.255.0 gateway=172.28.86.8
        
        # 设置静态DNS
        netsh interface ip set dns name="$wlanName" source=static addr=172.28.86.8 register=primary
        
        Write-Host "[完成] 已切换为 静态IP 模式`n" -ForegroundColor Yellow
    }
    
    default {
        Write-Host "`n[错误] 输入无效，请输入 1 或 2`n" -ForegroundColor Red
    }
}

# 暂停窗口，方便查看结果
Write-Host "按任意键退出..." -ForegroundColor Gray
Read-Host
```

ps1 双击默认是用记事本打开，可以用批处理脚本调用：

```cmd
@echo off
chcp 65001 >nul
:: 以管理员身份调用PowerShell执行同目录下的ps1脚本
powershell -Command "Start-Process powershell -ArgumentList '-NoProfile -ExecutionPolicy Bypass -File ""%~dp0WLAN切换.ps1""' -Verb RunAs"
exit
```

将上面的代码另存为bat或者cmd 批处理文件，这样双击批处理文件，就能调用同目录下的`WLAN切换.ps1`文件了。

## 批处理版本

另外准备了一个纯批处理版本，单文件直接修改网络设置。

```cmd
@echo off
setlocal enabledelayedexpansion

:: 1. 检查管理员权限
net session >nul 2>&1
if %errorLevel% neq 0 (
    echo 正在请求管理员权限...
    powershell -Command "Start-Process '%~f0' -Verb RunAs"
    exit /b
)

:: 2. 设置网卡名称（如果你的网卡不叫 WLAN，请在此修改）
set "INTERFACE_NAME=WLAN"

:MENU
cls
echo ============================================
echo          WLAN 配置一键切换工具
echo ============================================
echo  1. 自动获取 (DHCP) - 开启 IPv6
echo  2. 静态 IP (172.28.86.85) - 关闭 IPv6
echo  3. 退出
echo ============================================
echo.
set /p choice="请输入选项 [1, 2, 3] 然后回车: "

if "%choice%"=="1" goto DHCP
if "%choice%"=="2" goto STATIC
if "%choice%"=="3" exit
goto MENU

:DHCP
echo.
echo [正在执行] 切换至 DHCP 模式并启用 IPv6...
:: 启用 IPv6
powershell -Command "Enable-NetAdapterBinding -Name '%INTERFACE_NAME%' -ComponentID ms_tcpip6"
:: 设置 IPv4 为 DHCP
netsh interface ip set address name="%INTERFACE_NAME%" source=dhcp
netsh interface ip set dns name="%INTERFACE_NAME%" source=dhcp
echo [完成] 已恢复自动获取。
pause
goto MENU

:STATIC
echo.
echo [正在执行] 切换至静态 IP (172.28.86.85) 并禁用 IPv6...
:: 禁用 IPv6
powershell -Command "Disable-NetAdapterBinding -Name '%INTERFACE_NAME%' -ComponentID ms_tcpip6"
:: 设置静态 IP 和网关（对应 172.28.86.x ZeroTier 网段）
netsh interface ip set address name="%INTERFACE_NAME%" source=static addr=172.28.86.85 mask=255.255.255.0 gateway=172.28.86.8
netsh interface ip set dns name="%INTERFACE_NAME%" source=static addr=172.28.86.8 register=primary
echo [完成] 已切换至静态 IP 模式。
pause
goto MENU
```

将文件另存为 `bat` 或者 `cmd` 文件，双击直接运行即可。

