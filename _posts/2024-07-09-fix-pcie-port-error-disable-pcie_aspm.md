---
layout: mypost
title: pcie port aer 错误 故障修复（禁用pcie_aspm）
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

## 说明

主​​​​​​​动​​​​​​​式​​​​​​​电​​​​​​​源​​​​​​​管​​​​​​​理​​​​​​​（ASPM）在​​​​​​​ ​​​​PCIe（Peripheral Component Interconnect Express）子​​​​​​​系​​​​​​​统​​​​​​​中​​​​​​​的​​​​​​​节​​​​​​​电​​​​​​​，其​​​​​​​原​​​​​​​理​​​​​​​为​​​​​​​当​​​​​​​设​​​​​​​备​​​​​​​连​​​​​​​接​​​​​​​的​​​​​​​ PCI 连​​​​​​​接​​​​​​​没​​​​​​​有​​​​​​​处​​​​​​​于​​​​​​​使​​​​​​​用​​​​​​​状​​​​​​​态​​​​​​​时​​​​​​​将​​​​​​​其​​​​​​​设​​​​​​​定​​​​​​​为​​​​​​​低​​​​​​​功​​​​​​​率​​​​​​​状​​​​​​​态​​​​​​​。​​​​​​​ASPM 可​​​​​​​同​​​​​​​时​​​​​​​在​​​​​​​终​​​​​​​端​​​​​​​和​​​​​​​连​​​​​​​接​​​​​​​中​​​​​​​控​​​​​​​制​​​​​​​电​​​​​​​源​​​​​​​状​​​​​​​态​​​​​​​，并​​​​​​​在​​​​​​​连​​​​​​​接​​​​​​​终​​​​​​​端​​​​​​​的​​​​​​​设​​​​​​​备​​​​​​​处​​​​​​​于​​​​​​​满​​​​​​​电​​​​​​​状​​​​​​​态​​​​​​​时​​​​​​​仍​​​​​​​可​​​​​​​在​​​​​​​连​​​​​​​接​​​​​​​中​​​​​​​节​​​​​​​电​​​​​​​。​​​​​​​

设置 `pcie_aspm=off` 后，禁​​​​​​​用​​​​​​​ ASPM 则​​​​​允​​​​​​​许​​​​​​​ PCIe 链​​​​​​​接​​​​​​​以​​​​​​​最​​​​​​​佳​​​​​​​性​​​​​​​能​​​​​​​操​​​​​​​作​​​​​​​。​​​​​​​

### 参考链接

[3.5. 主​​​​​​​动​​​​​​​式​​​​​​​电​​​​​​​源​​​​​​​管​​​​​​​理​​​​​​​](https://docs.redhat.com/zh_hans/documentation/red_hat_enterprise_linux/6/html/power_management_guide/aspm)

[pcieport 0000:00:1d.0: AER: Corrected error received: 0000:04:00.0](https://askubuntu.com/questions/1401726/pcieport-0000001d-0-aer-corrected-error-received-00000400-0)
