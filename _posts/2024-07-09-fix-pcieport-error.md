---
layout: mypost
title: pcieport 000:00:1d.0: aer: error of this agent is reported first故障修复
categories: [Linux]
---

## 起因

新买了BY51的迷你PC，安装Ubuntu和Debian都会报错：
```
pcieport 000:00:1d.0: aer: error of this agent is reported first
```

并且这个报错会导致systemd-journald.service服务一直CPU高占用。

## 解决

修改grub启动参数：
```bash
sudo nano /etc/default/grub
```

找到GRUB_CMDLINE_LINUX_DEFAULT这一行：

```
GRUB_CMDLINE_LINUX_DEFAULT="pcie_aspm=off"
```

然后更新grub并重启：

```
sudo update-grub
sudo reboot
```

### 参考链接

[pcieport 0000:00:1d.0: AER: Corrected error received: 0000:04:00.0](https://askubuntu.com/questions/1401726/pcieport-0000001d-0-aer-corrected-error-received-00000400-0)
