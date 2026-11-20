
# 用DeepSeek自动处理Excel表格，拆分、合并一秒完成！

> 来源：[https://geekbang.feishu.cn/docx/KFzhdaRn5oQKXmxe7xycGQhPnWc](https://geekbang.feishu.cn/docx/KFzhdaRn5oQKXmxe7xycGQhPnWc)

文章来自：Excel 精英培训

原文地址：https://mp.weixin.qq.com/s/YdgoGG1ETdu8R_kLnq2BMA

昨天兰色分享了让Deepseek设置公式的方法，顺利完成了要求。今天再试一个更难的表格处理。

如下图表中，要求按A列项目内容拆分成多个表格并另存为单独的excel文件，文件名为A列的值。

![](img/fd0bd839a71d5b5e6cd4fea185bcf47e.png)

要求拆分后果：

![](img/78e7af2d90e8de0747c1a4644d69503d.png)

## 具体操作步骤

### 1、让DeepSeek生成代码

打开`https://www.deepseek.com/`，点别针先把表格文件上传（上传表格截图也可以）

![](img/eecc4b5e922949551a10d9165aa30e1b.png)

把问题提交给deepSeek（一定要描述清楚）

![](img/4fef4be2f2c8a528794cf63d84c9dd0d.png)

很快思路和VBA代码就生成了，而且提供了代码的详细使用说明

```
Sub SplitDataByProject()    Dim dict As Object    Dim wsSource As Worksheet    Dim lastRow As Long, i As Long    Dim key As Variant    Dim newWb As Workbook    Dim newWs As Worksheet    Dim savePath As String
    &#x27;设置字典用于存储唯一项目值    Set dict = CreateObject(&quot;Scripting.Dictionary&quot;)    Set wsSource = ThisWorkbook.Sheets(&quot;A表&quot;) &#x27;根据实际表名修改
    &#x27;获取数据最后一行    lastRow = wsSource.Cells(wsSource.Rows.Count, &quot;A&quot;).End(xlUp).Row
    &#x27;收集唯一项目值（排除标题行）    For i = 2 To lastRow        If Not dict.exists(wsSource.Cells(i, 1).Value) Then            dict.Add wsSource.Cells(i, 1).Value, Nothing        End If    Next i
    &#x27;设置保存路径（默认为当前工作簿路径）    savePath = ThisWorkbook.Path &amp; &quot;\&quot;
    Application.ScreenUpdating = False    Application.DisplayAlerts = False
    &#x27;循环处理每个项目    For Each key In dict.keys        &#x27;创建新工作簿        Set newWb = Workbooks.Add        Set newWs = newWb.Sheets(1)
        &#x27;复制标题行        wsSource.Rows(1).Copy newWs.Rows(1)
        &#x27;筛选并复制数据        wsSource.Range(&quot;A1:F&quot; &amp; lastRow).AutoFilter Field:=1, Criteria1:=key        wsSource.UsedRange.Offset(1).SpecialCells(xlCellTypeVisible).Copy newWs.Range(&quot;A2&quot;)
        &#x27;清除筛选        wsSource.AutoFilterMode = False
        &#x27;保存文件        newWb.SaveAs savePath &amp; key &amp; &quot;.xlsx&quot;        newWb.Close    Next key
    &#x27;恢复设置    Application.ScreenUpdating = True    Application.DisplayAlerts = True
    MsgBox &quot;已拆分生成 &quot; &amp; dict.Count &amp; &quot; 个文件！&quot;End Sub
```

![](img/ce86c7d46a5bde2fb1d3ad7d5a935e4e.png)

### 2、按上面说明执行代码

按`Alt+f11`打开VBE编程器（WPS需要安装WPS VBA插件）

插入菜单-添加模块后，把代码粘贴的右侧空白处中，点三角运行

![](img/d7d5ce22d364cf567f1c14bb6722a024.png)

**只需1秒**，在当前文件夹下就生成4个新文件

![](img/f2fe5ed57a524a9d82241364b899782b.png)

打开后正是拆分后的表格，DeepSeek任务顺利完成。

如果代码需要保存在excel工作簿内，文件需要另存为启用宏的工作簿

![](img/2e778955d20c3ea315420e414aa8e7e4.png)

兰色说：有了VBA加持，DeepSeep可以帮我们完成很多复杂工作，除了本文的拆分，合并多个excel文件等操作也可以搞定。
