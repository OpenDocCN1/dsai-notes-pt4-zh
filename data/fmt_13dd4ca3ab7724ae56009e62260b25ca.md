
# 按键精灵-脚本

> 来源：[https://yunyingmenghai.feishu.cn/docx/Nfrvd3684obUO7x17ZQcx2KBn6c](https://yunyingmenghai.feishu.cn/docx/Nfrvd3684obUO7x17ZQcx2KBn6c)

快手批量删除视频：`https://cp.kuaishou.com/article/manage/video`

```
For i=1 To 10
    //这里的脚本可以循环10次
IfColor 608, 832, &quot;FFFFFF&quot;, 1 Then
        //颜色不一样
        MoveTo 2937, 970
        Delay 126
        LeftClick 1
        Delay 10
        LeftUp 1

        MoveTo 1810, 1129
        
        Delay 2377
        LeftDown 1
        Delay 30
        LeftClick 1
        Delay 20
        LeftUp 1
        Delay 2377
Else    
    //颜色一样
    MoveTo 188, 125
    Delay 12
        LeftClick 1
        Delay 23
        LeftUp 1
        Delay 1200
    
End If
Next
```
