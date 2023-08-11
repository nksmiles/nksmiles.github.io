---
layout: mypost
title: KiwiVM面板不显示RAM、SWAP信息的解决方法
categories: [VPS,Linux]
---

在使用 BandwagonHost 的 KiwiVM 面板时发现，新安装的 Linux 系统可以正常显示 RAM、SWAP 信息，但是重启后，这两项信息就无法显示了，而且 Root shell 等功能也无法正常使用。 
（在 Debian 11、AlmaLinux 9 上均复现此问题。）

测试发现，是因为 `qemu-kvm_ga` 脚本没有运行，手动运行后，
```bash
/usr/bin/qemu-kvm_ga
```
可以在 KiwiVM 面板查看RAM、SWAP信息了。
下面我们通过添加后台服务的方式让 qemu-kvm_ga 脚本自动运行。

新建一个service文件：
```bash
nano /lib/systemd/system/qemu-kvm.service
```

将以下内容复制到文件中：

```
[Unit]
Description=QEMU-KVM
After=network.target

[Service]
Type=forking
ExecStart=/bin/bash /usr/bin/qemu-kvm_ga
Restart=always
RestartSec=1

[Install]
WantedBy=multi-user.target
```

Ctrl+X 保存后，启用服务，并启动：
```bash
systemctl enable qemu-kvm
systemctl start qemu-kvm
```
