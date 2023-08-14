---
layout: mypost
title: Word VBA 更新所有域的宏代码
categories: [Word VBA]
---

当Word中插入的域很多时，更新域就变得极为繁琐，利用Word里面的宏功能，通过下面的VBA代码可以一键快速更新文档中的所有域。高效、方便。

Word中用于更新所有域的宏代码： 

```
    Dim aField As Field
    Dim aStory As Range
    ''' Update all fields in the document
    For Each aStory In ActiveDocument.StoryRanges
       For Each aField In aStory.Fields
          aField.Update
       Next aField
    Next aStory
```
