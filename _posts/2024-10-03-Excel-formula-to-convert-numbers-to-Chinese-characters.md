---
layout: mypost
title: Excel 用公式将数字转为汉字大写/英文单词的几种方法
categories: [Excel]
---

由于工作需要，经常要将Excel中的数字转为中文大写，或者英文单次，网上有很多使用VBA实现的方法，但是VBA方法通用性不强，待宏的excel文件也容易被拦截禁止运行。

下面是网上搜集的集中使用公式转换的方法，虽然有的公式比较繁琐，但是通用性强，在此分享出来。

## 隐藏函数：NUMBERSTRING

NUMBERSTRING 不是官方的 Excel 函数，它只是为了与旧的 Lotus123 电子表格兼容。所以NUMBERSTRING函数再Excel中不会提示，在微软的官方帮助文档中也查找不到任何信息，实测函数有2个参数，第一个参数为待转换的数字，第二个参数为转换的格式，参数有3种：1，2，3对应的分别为中文大写，会计大写和数学大写。

下面的示例中，A1单元格中的数字为1234

|公式                 |结果           |
| ------------------- | -----------: |
|=NUMBERSTRING(A1,1)  |一千二百三十四 |
|=NUMBERSTRING(A1,1)  |壹仟贰佰叁拾肆 |
|=NUMBERSTRING(A1,1)  |一二三四      |

实测使用NUMBERSTRING函数不能转换带小数的数字，在最新的Microsoft 365 Excel中仍然可用。




## 中文大写组合公式（SUBSTITUTE、TEXT、INT、MID、FIND...）

```
=SUBSTITUTE(SUBSTITUTE(TEXT(INT(A1),"[DBNum2][$-804]G/通用格式元"&IF(INT(A1)=A1,"整",""))&TEXT(MID(A1,FIND(".",A1&".0")+1,1),"[DBNum2][$-804]G/通用格式角")&TEXT(MID(A1,FIND(".",A1&".0")+2,1),"[DBNum2][$-804]G/通用格式分"),"零角","零"),"零分","")
```

下面的示例中，A1单元格中的数字为1234.56

|公式                 |结果           |
| ------------------- | -----------: | 
|=SUBSTITUTE(SUBSTITUTE(TEXT(INT(A1),"[DBNum2][$-804]G/通用格式元"&IF(INT(A1)=A1,"整",""))&TEXT(MID(A1,FIND(".",A1&".0")+1,1),"[DBNum2][$-804]G/通用格式角")&TEXT(MID(A1,FIND(".",A1&".0")+2,1),"[DBNum2][$-804]G/通用格式分"),"零角","零"),"零分","")  |壹仟贰佰叁拾肆元伍角陆分 |


这个组合函数因为使用了Excel的 [DBNum2]数字格式，所以依赖于系统的语言设置。需要系统语言设置为简体中文才能生效。
函数支持小数点后2位。


## 英文单词组合公式（CHOOSE、LEFT、TEXT、MID、IF...）
```
=CHOOSE(LEFT(TEXT(A1,"000000000.00"))+1,,"One","Two","Three","Four","Five","Six","Seven","Eight","Nine")
&IF(--LEFT(TEXT(A1,"000000000.00"))=0,,IF(AND(--MID(TEXT(A1,"000000000.00"),2,1)=0,--MID(TEXT(A1,"000000000.00"),3,1)=0)," Hundred"," Hundred and "))
&CHOOSE(MID(TEXT(A1,"000000000.00"),2,1)+1,,,"Twenty ","Thirty ","Forty ","Fifty ","Sixty ","Seventy ","Eighty ","Ninety ")
&IF(--MID(TEXT(A1,"000000000.00"),2,1)<>1,CHOOSE(MID(TEXT(A1,"000000000.00"),3,1)+1,,"One","Two","Three","Four","Five","Six","Seven","Eight","Nine"),
CHOOSE(MID(TEXT(A1,"000000000.00"),3,1)+1,"Ten","Eleven","Twelve","Thirteen","Fourteen","Fifteen","Sixteen","Seventeen","Eighteen","Nineteen"))
&IF((--LEFT(TEXT(A1,"000000000.00"))+MID(TEXT(A1,"000000000.00"),2,1)+MID(TEXT(A1,"000000000.00"),3,1))=0,,IF(AND((--MID(TEXT(A1,"000000000.00"),4,1)+MID(TEXT(A1,"000000000.00"),5,1)+MID(TEXT(A1,"000000000.00"),6,1)+MID(TEXT(A1,"000000000.00"),7,1))=0,(--MID(TEXT(A1,"000000000.00"),8,1)+RIGHT(TEXT(A1,"000000000.00")))>0)," Million and "," Million "))
&CHOOSE(MID(TEXT(A1,"000000000.00"),4,1)+1,,"One","Two","Three","Four","Five","Six","Seven","Eight","Nine")
&IF(--MID(TEXT(A1,"000000000.00"),4,1)=0,,IF(AND(--MID(TEXT(A1,"000000000.00"),5,1)=0,--MID(TEXT(A1,"000000000.00"),6,1)=0)," Hundred"," Hundred and"))
&CHOOSE(MID(TEXT(A1,"000000000.00"),5,1)+1,,," Twenty"," Thirty"," Forty"," Fifty"," Sixty"," Seventy"," Eighty"," Ninety")
&IF(--MID(TEXT(A1,"000000000.00"),5,1)<>1,CHOOSE(MID(TEXT(A1,"000000000.00"),6,1)+1,," One"," Two"," Three"," Four"," Five"," Six"," Seven"," Eight"," Nine"),CHOOSE(MID(TEXT(A1,"000000000.00"),6,1)+1," Ten"," Eleven"," Twelve"," Thirteen"," Fourteen"," Fifteen"," Sixteen"," Seventeen"," Eighteen"," Nineteen"))
&IF((--MID(TEXT(A1,"000000000.00"),4,1)+MID(TEXT(A1,"000000000.00"),5,1)+MID(TEXT(A1,"000000000.00"),6,1))=0,,IF(OR((--MID(TEXT(A1,"000000000.00"),7,1)+MID(TEXT(A1,"000000000.00"),8,1)+MID(TEXT(A1,"000000000.00"),9,1))=0,--MID(TEXT(A1,"000000000.00"),7,1)<>0)," Thousand "," Thousand and "))
&CHOOSE(MID(TEXT(A1,"000000000.00"),7,1)+1,,"One","Two","Three","Four","Five","Six","Seven","Eight","Nine")
&IF(--MID(TEXT(A1,"000000000.00"),7,1)=0,,IF(AND(--MID(TEXT(A1,"000000000.00"),8,1)=0,--MID(TEXT(A1,"000000000.00"),9,1)=0)," Hundred "," Hundred and "))&
CHOOSE(MID(TEXT(A1,"000000000.00"),8,1)+1,,,"Twenty ","Thirty ","Forty ","Fifty ","Sixty ","Seventy ","Eighty ","Ninety ")
&IF(--MID(TEXT(A1,"000000000.00"),8,1)<>1,CHOOSE(MID(TEXT(A1,"000000000.00"),9,1)+1,,"One","Two","Three","Four","Five","Six","Seven","Eight","Nine"),CHOOSE(MID(TEXT(A1,"000000000.00"),9,1)+1,"Ten","Eleven","Twelve","Thirteen","Fourteen","Fifteen","Sixteen","Seventeen","Eighteen","Nineteen"))
```

下面的示例中，A1单元格中的数字为1234

|公式                 |结果                                  |
| ------------------- | -----------------------------------: |
|=CHOOSE(LEFT(TEXT(A1,"000000000.00"))+1,,"One","Two","Three","Four","Five","Six","Seven","Eight","Nine")&IF(--LEFT(TEXT(A1,"000000000.00"))=0,,IF(AND(--MID(TEXT(A1,"000000000.00"),2,1)=0,--MID(TEXT(A1,"000000000.00"),3,1)=0)," Hundred"," Hundred and "))&CHOOSE(MID(TEXT(A1,"000000000.00"),2,1)+1,,,"Twenty ","Thirty ","Forty ","Fifty ","Sixty ","Seventy ","Eighty ","Ninety ")&IF(--MID(TEXT(A1,"000000000.00"),2,1)<>1,CHOOSE(MID(TEXT(A1,"000000000.00"),3,1)+1,,"One","Two","Three","Four","Five","Six","Seven","Eight","Nine"),CHOOSE(MID(TEXT(A1,"000000000.00"),3,1)+1,"Ten","Eleven","Twelve","Thirteen","Fourteen","Fifteen","Sixteen","Seventeen","Eighteen","Nineteen"))&IF((--LEFT(TEXT(A1,"000000000.00"))+MID(TEXT(A1,"000000000.00"),2,1)+MID(TEXT(A1,"000000000.00"),3,1))=0,,IF(AND((--MID(TEXT(A1,"000000000.00"),4,1)+MID(TEXT(A1,"000000000.00"),5,1)+MID(TEXT(A1,"000000000.00"),6,1)+MID(TEXT(A1,"000000000.00"),7,1))=0,(--MID(TEXT(A1,"000000000.00"),8,1)+RIGHT(TEXT(A1,"000000000.00")))>0)," Million and "," Million "))&CHOOSE(MID(TEXT(A1,"000000000.00"),4,1)+1,,"One","Two","Three","Four","Five","Six","Seven","Eight","Nine")&IF(--MID(TEXT(A1,"000000000.00"),4,1)=0,,IF(AND(--MID(TEXT(A1,"000000000.00"),5,1)=0,--MID(TEXT(A1,"000000000.00"),6,1)=0)," Hundred"," Hundred and"))&CHOOSE(MID(TEXT(A1,"000000000.00"),5,1)+1,,," Twenty"," Thirty"," Forty"," Fifty"," Sixty"," Seventy"," Eighty"," Ninety")&IF(--MID(TEXT(A1,"000000000.00"),5,1)<>1,CHOOSE(MID(TEXT(A1,"000000000.00"),6,1)+1,," One"," Two"," Three"," Four"," Five"," Six"," Seven"," Eight"," Nine"),CHOOSE(MID(TEXT(A1,"000000000.00"),6,1)+1," Ten"," Eleven"," Twelve"," Thirteen"," Fourteen"," Fifteen"," Sixteen"," Seventeen"," Eighteen"," Nineteen"))&IF((--MID(TEXT(A1,"000000000.00"),4,1)+MID(TEXT(A1,"000000000.00"),5,1)+MID(TEXT(A1,"000000000.00"),6,1))=0,,IF(OR((--MID(TEXT(A1,"000000000.00"),7,1)+MID(TEXT(A1,"000000000.00"),8,1)+MID(TEXT(A1,"000000000.00"),9,1))=0,--MID(TEXT(A1,"000000000.00"),7,1)<>0)," Thousand "," Thousand and "))&CHOOSE(MID(TEXT(A1,"000000000.00"),7,1)+1,,"One","Two","Three","Four","Five","Six","Seven","Eight","Nine")&IF(--MID(TEXT(A1,"000000000.00"),7,1)=0,,IF(AND(--MID(TEXT(A1,"000000000.00"),8,1)=0,--MID(TEXT(A1,"000000000.00"),9,1)=0)," Hundred "," Hundred and "))&CHOOSE(MID(TEXT(A1,"000000000.00"),8,1)+1,,,"Twenty ","Thirty ","Forty ","Fifty ","Sixty ","Seventy ","Eighty ","Ninety ")&IF(--MID(TEXT(A1,"000000000.00"),8,1)<>1,CHOOSE(MID(TEXT(A1,"000000000.00"),9,1)+1,,"One","Two","Three","Four","Five","Six","Seven","Eight","Nine"),CHOOSE(MID(TEXT(A1,"000000000.00"),9,1)+1,"Ten","Eleven","Twelve","Thirteen","Fourteen","Fifteen","Sixteen","Seventeen","Eighteen","Nineteen")) | One Thousand Two Hundred and Thirty Four |


上述函数仅支持整数数字，不支持小数。

## 参考链接

[教你Excel数字怎么转大写金额](https://cloud.tencent.com/developer/news/931688)

[NUMBERSTRING FUNCTION](https://answers.microsoft.com/en-us/msoffice/forum/all/numberstring-function/972bd253-f238-4cfc-ba8f-b9d0185f121a)

[Convert Numbers to Words in Excel (without VBA)](https://www.youtube.com/watch?v=2PonSV_7PI4)

[excel阿拉伯数字转中文大写](https://www.cnblogs.com/POTUS/p/14328748.html)
