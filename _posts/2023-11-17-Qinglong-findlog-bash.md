---
layout: mypost
title: 青龙面板依赖缺失查找
categories: [Linux]
---

通过查找青龙面板的日志来搜索缺失的依赖。缺失的依赖仍然需要手动安装。


```bash
# 用于查找青龙面板日志文件中的依赖缺失报错。
# 找到后需要手动添加/安装依赖。
# 遍历/root/ql/data/log目录及其所有子目录中的.log文件
for file in `find /root/ql/data/log -name "*.log"`; do
    # 在每个.log文件中搜索"Cannot find module"字符串，并输出匹配的行
    grep -n "Cannot find module" $file
    result=`grep -n "Cannot find module" $file`
    # 如果有匹配结果，则显示当前.log文件的完整路径
    if [ $? -eq 0 ]; then
        echo "File path: $file"
    fi
done
echo "End."
```

[findlog.sh](findlog.sh)
