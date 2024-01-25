---
layout: mypost
title: Linux 常用命令
categories: [Linux]
---

## 查看端口占用

lsof (list open files)是一个列出当前系统打开文件的工具。它可以显示进程名、进程ID、所属用户、文件描述符、文件类型、设备ID、文件大小、Inode号和文件路径等信息。

lsof 查看端口占用的语法格式是：

```
lsof -i:端口号
```

例如：
```bash
$ lsof -i:443
COMMAND    PID  USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
nginx   708088  root    8u  IPv4 2488628      0t0  TCP *:https (LISTEN)
nginx   708089 nginx    8u  IPv4 2488628      0t0  TCP *:https (LISTEN)
nginx   708090 nginx    8u  IPv4 2488628      0t0  TCP *:https (LISTEN)
```
