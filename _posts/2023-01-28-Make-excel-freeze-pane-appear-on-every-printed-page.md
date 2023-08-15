---
layout: mypost
title: 如何让 excel 冻结窗格在打印的每页出现
categories: [Excel VBA]
---

Excel 打印时可以设置标题行可以在每一页都显示，新版 Excel 在“页面布局”选项卡，“页面设置”部门，选择“打印标题”，在弹出的对话框中，选择“顶端标题行” 或者 “左端标题行”，右边点有红色小箭头的按扭，出来一个黑色的小箭头，然后再标题行或者标题列上点一下，确定，OK。


使用宏代码实现：
```vb
    With ActiveSheet.PageSetup
        .PrintTitleRows = "$1:$3"
        .PrintTitleColumns = ""
    End With
```
