# 利用DeepSeek修改月报

> 来源：[https://datawhaler.feishu.cn/docx/UNimdzjk0o31g6xZEL4ckemXndv](https://datawhaler.feishu.cn/docx/UNimdzjk0o31g6xZEL4ckemXndv)

本文目标对象：需要**写月报**的打工人，且月报格式和内容相对固定，部分月报内容来自excel数据和其他word文档，且excel和word格式都相对固定。

# Step1：整理月报模板

方法1（手动版）：将每个月需要修改的部分，如月份、销售额、增长率等等，用占位符来表示。

方法2（自动版）：把某月月报发送给deepseek，告诉它哪些数据或文本需要替换成占位符。（注意上传前删掉保密数据。）

例如，我这里有一份月报模板：

[月报模板](https://ci7ta5wy9u0.feishu.cn/docx/Qu2LdHun7ojxF7xGsBsctPeunoh)

月份和销售额等数据在表格[2025-02](https://ci7ta5wy9u0.feishu.cn/sheets/B8L9srpHqhTpI3tSnQicUjr8nEd?from=from_copylink)中。

情况通报在word文档[情况通报](https://ci7ta5wy9u0.feishu.cn/docx/UqEtdzZImo3gO0xkTWqcnVVEnPg?from=from_copylink)中。

接下来，我需要把这个月需要的数据整理好。

# Step2：整理占位符和数据来源的关系

[月报模板](https://ci7ta5wy9u0.feishu.cn/docx/Qu2LdHun7ojxF7xGsBsctPeunoh)中的{{月份}}、{{销售额}}、{{增长率}}和{{转化率}}分别对应[2025-02](https://ci7ta5wy9u0.feishu.cn/sheets/B8L9srpHqhTpI3tSnQicUjr8nEd?from=from_copylink)中的对应数据。

[月报模板](https://ci7ta5wy9u0.feishu.cn/docx/Qu2LdHun7ojxF7xGsBsctPeunoh)中的{{情况通报}}对应[情况通报](https://ci7ta5wy9u0.feishu.cn/docx/UqEtdzZImo3gO0xkTWqcnVVEnPg?from=from_copylink)的所有文字。

# Step3：利用DeepSeek写VBA代码

打开https://chat.deepseek.com/，输入以下文字：

“我有3个文件：1. 月报模板.docx；2. 月度销售额.xlsx；3. 月度情况通报.docx。其中月报模板的{{月份}}、{{销售额}}、{{增长率}}和{{转化率}}需要分别替换为“月度销售额.xlsx”的sheet1中单元格A2：D2的数据；{{情况通报}}需要替换为“月度情况通报.docx”的所有内容。”

DeepSeek即会输出可执行的VBA代码。

```
Sub GenerateMonthlyReport()
    Dim wordApp As Object
    Dim wordDoc As Object
    Dim excelApp As Object
    Dim excelWorkbook As Object
    Dim excelWorksheet As Object
    Dim data As Object
    Dim month As String
    Dim outputPath As String
    
    &#x27; 设置目标月份
    month = &quot;2025/2/1&quot; &#x27; 确保与 Excel 中的数据格式一致
    
    &#x27; 打开 Excel 数据源
    On Error Resume Next
    Set excelApp = CreateObject(&quot;Excel.Application&quot;)
    Set excelWorkbook = excelApp.Workbooks.Open(&quot;E:\月度销售额.xlsx&quot;) &#x27; Excel 文件路径
    If excelWorkbook Is Nothing Then
        MsgBox &quot;无法打开 Excel 文件，请检查路径！&quot;
        Exit Sub
    End If
    On Error GoTo 0
    
    &#x27; 获取默认工作表（Sheet1）
    Set excelWorksheet = excelWorkbook.Sheets(1)
    If excelWorksheet Is Nothing Then
        MsgBox &quot;未找到默认工作表！&quot;
        Exit Sub
    End If
    
    &#x27; 读取数据
    Dim lastRow As Long
    lastRow = excelWorksheet.Cells(excelWorksheet.Rows.Count, 1).End(-4162).Row
    Dim found As Boolean
    found = False
    
    Dim i As Long
    For i = 2 To lastRow
        &#x27; 检查月份是否匹配
        If CDate(excelWorksheet.Cells(i, 1).Value) = CDate(month) Then
            Set data = excelWorksheet.Rows(i)
            found = True
            Exit For
        End If
    Next i
    
    If Not found Then
        MsgBox &quot;未找到 &quot; &amp; month &amp; &quot; 的数据！&quot;
        Exit Sub
    End If
    
    &#x27; 打开 Word 模板
    On Error Resume Next
    Set wordApp = CreateObject(&quot;Word.Application&quot;)
    Set wordDoc = wordApp.Documents.Open(&quot;E:\月报模板.docx&quot;) &#x27; Word 模板路径
    If wordDoc Is Nothing Then
        MsgBox &quot;无法打开 Word 模板，请检查路径！&quot;
        Exit Sub
    End If
    On Error GoTo 0
    
    &#x27; 替换数据占位符
    Dim range As Object
    Set range = wordDoc.Content
    With range.Find
        .Text = &quot;{{月份}}&quot;
        .Replacement.Text = Format(CDate(month), &quot;yyyy年M月&quot;) &#x27; 格式化月份
        .Execute Replace:=2
        
        .Text = &quot;{{销售额}}&quot;
        .Replacement.Text = data.Cells(2).Value
        .Execute Replace:=2
        
        .Text = &quot;{{增长率}}&quot;
        .Replacement.Text = data.Cells(3).Value
        .Execute Replace:=2
        
        .Text = &quot;{{转化率}}&quot;
        .Replacement.Text = data.Cells(4).Value
        .Execute Replace:=2
    End With
    
    &#x27; 插入月度情况通报
    Dim noticeDoc As Object
    Set noticeDoc = wordApp.Documents.Open(&quot;E:\月度情况通报.docx&quot;) &#x27; 通报文件路径
    If noticeDoc Is Nothing Then
        MsgBox &quot;无法打开月度情况通报文件，请检查路径！&quot;
        Exit Sub
    End If
    
    &#x27; 查找占位符位置并插入通报内容
    With wordDoc.Content.Find
        .Text = &quot;{{情况通报}}&quot;
        .Execute
        If .Found Then
            .Parent.InsertAfter vbNewLine
            noticeDoc.Content.Copy
            .Parent.Paste
        Else
            MsgBox &quot;未找到 {{情况通报}} 占位符！&quot;
        End If
    End With
    
    &#x27; 保存生成的报告
    outputPath = &quot;E:\月报_202502.docx&quot; &#x27; 生成的月报路径
    wordDoc.SaveAs2 outputPath
    
    &#x27; 关闭文档和应用程序
    noticeDoc.Close False
    wordDoc.Close
    wordApp.Quit
    excelWorkbook.Close
    excelApp.Quit
    
    &#x27; 释放对象
    Set noticeDoc = Nothing
    Set wordDoc = Nothing
    Set wordApp = Nothing
    Set excelWorksheet = Nothing
    Set excelWorkbook = Nothing
    Set excelApp = Nothing
    
    MsgBox &quot;月报已生成：&quot; &amp; outputPath
End Sub
```

# Step4：执行VBA代码

这一步也可以利用DeepSeek帮忙，问问它你该怎么执行代码即可。以下是DeepSeek的回答：

---

## **操作步骤**

1.  **准备文件**：

+   将 月报模板.docx、月度销售额.xlsx 和 月度情况通报.docx 放在 **E盘根目录**（可以根据个人具体情况修改） 下。

+   确保 Excel 文件中的数据格式正确，例如：

1.  **运行宏**：

+   打开 Word，按 Alt + F11 打开 VBA 编辑器。

+   将代码粘贴到模块中，按 F5 运行。

1.  **检查结果**：

+   生成的月报会保存为 E:\月报_202502.docx。

+   打开文件，检查占位符是否被正确替换，通报内容是否插入到正确位置。

---

# Step5：出现错误怎么办？

利用好DeepSeek老师，出错了问问它该怎么办。当然，请注意尽可能多提供信息。

教程到此结束，如果你有更好的解决方案，欢迎评论交流！