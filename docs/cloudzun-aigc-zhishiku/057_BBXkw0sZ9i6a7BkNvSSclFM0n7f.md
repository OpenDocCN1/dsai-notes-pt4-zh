# 自动发送不同内容到不同信箱

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/XzcXdsmxPoQuy8xHqSicDU6gnxf](https://zsyhjtnsa5.feishu.cn/docx/XzcXdsmxPoQuy8xHqSicDU6gnxf)

这个问题很有意思！在 Excel 中，原生功能本身并不直接支持“按行自动发送不同内容到不同邮箱地址”，但通过一些方法和工具，可以实现这个需求。以下是几种可行的解决方案，供你参考：

---

### 方法 1：使用 VBA 宏代码

如果你对 Excel 的 VBA（Visual Basic for Applications）稍有了解，可以通过编写宏代码实现这个功能。假设你的 Sheet 中每一行包含一个邮箱地址（比如在列 A），以及需要发送的内容（比如列 B 或整个行），可以用 VBA 自动遍历每一行并发送邮件。

#### 示例步骤：

1.  **按 Alt + F11** 打开 VBA 编辑器。

1.  点击“插入 > 模块”，然后粘贴以下代码：

```
Sub SendEmailsByRow()
    Dim OutApp As Object
    Dim OutMail As Object
    Dim ws As Worksheet
    Dim rng As Range
    Dim cell As Range
    Dim email As String
    Dim content As String

    &#x27; 设置工作表
    Set ws = ThisWorkbook.Sheets(&quot;Sheet1&quot;) &#x27; 替换为你的Sheet名称
    Set rng = ws.Range(&quot;A2:A&quot; &amp; ws.Cells(ws.Rows.Count, &quot;A&quot;).End(xlUp).Row) &#x27; 假设邮箱地址从 A2 开始

    &#x27; 创建 Outlook 应用程序对象
    Set OutApp = CreateObject(&quot;Outlook.Application&quot;)

    &#x27; 遍历每一行
    For Each cell In rng
        If cell.Value Like &quot;*@*.*&quot; Then &#x27; 检查是否为有效邮箱地址
            email = cell.Value
            content = cell.Offset(0, 1).Value &#x27; 假设内容在旁边一列（B列）

            &#x27; 创建新邮件
            Set OutMail = OutApp.CreateItem(0)
            With OutMail
                .To = email
                .Subject = &quot;来自 Excel 的自动邮件&quot;
                .Body = &quot;您好，&quot; &amp; vbNewLine &amp; content &#x27; 邮件正文
                .Send &#x27; 直接发送（或用 .Display 先预览）
            End With
            Set OutMail = Nothing
        End If
    Next cell

    &#x27; 清理
    Set OutApp = Nothing
    MsgBox &quot;邮件发送完成！&quot;
End Sub
```

1.  **运行代码**：

+   保存文件为 .xlsm 格式（启用宏）。

+   确保 Outlook 已安装并配置好邮箱账户。

+   按 F5 运行宏，Excel 会逐行读取邮箱地址并发送邮件。

#### 注意事项：

+   需要调整代码中的 Sheet 名称和列号以匹配你的数据。

+   如果需要发送整行内容，可以用 cell.EntireRow.Value 并格式化。

+   Outlook 需已打开，或代码会自动启动它。

---

### 方法 2：结合 Word 的邮件合并功能

如果你不想写代码，可以用 Excel 作为数据源，结合 Word 的邮件合并功能发送个性化邮件。

#### 步骤：

1.  **准备 Excel 数据**：

+   确保 Sheet 中有列标题，比如 Email（邮箱地址）、Content（邮件内容）。

+   保存文件。

1.  **在 Word 中操作**：

+   打开 Word，新建文档。

+   转到“邮件”选项卡 > “选择收件人” > “使用现有列表”，选择你的 Excel 文件。

+   插入合并字段，比如 «Email» 和 «Content»。

+   点击“完成并合并” > “发送电子邮件”，设置 To 为邮箱字段，输入主题和正文。

1.  **发送**：

+   Word 会通过 Outlook 按行发送邮件，每封邮件对应一行数据。

#### 优点：

+   无需编程，适合小白用户。

+   可以设计复杂的邮件格式。

#### 缺点：

+   不能直接发送整行或附件，需要手动配置。

---

### 方法 3：使用第三方工具或插件

如果 VBA 和 Word 都不够方便，可以试试以下工具：

+   Power Automate（Microsoft 365）：

+   创建一个自动化流程，读取 Excel 表格中的行，循环发送邮件。

+   需要订阅 Microsoft 365，并设置在线 Excel 文件。

+   Kutools for Excel：

+   这款插件有“发送邮件”功能，可以按行批量发送，但需要购买。

---

### 方法 4：Python 脚本（高级用户）

如果你熟悉编程，可以用 Python 的 pandas 和 smtplib 库实现：

1.  用 pandas 读取 Excel 文件。

1.  遍历每一行，提取邮箱和内容。

1.  用 smtplib 通过你的邮箱服务器发送邮件。

#### 示例代码：

```
import pandas as pd
import smtplib
from email.mime.text import MIMEText

# 读取 Excel
df = pd.read_excel(&quot;your_file.xlsx&quot;, sheet_name=&quot;Sheet1&quot;)

# 邮箱配置
smtp_server = &quot;smtp.gmail.com&quot;
smtp_port = 587
sender_email = &quot;your_email@gmail.com&quot;
password = &quot;your_password&quot;

# 发送邮件
server = smtplib.SMTP(smtp_server, smtp_port)
server.starttls()
server.login(sender_email, password)

for index, row in df.iterrows():
    receiver_email = row[&quot;Email&quot;]
    content = row[&quot;Content&quot;]
    msg = MIMEText(content)
    msg[&quot;Subject&quot;] = &quot;自动邮件&quot;
    msg[&quot;From&quot;] = sender_email
    msg[&quot;To&quot;] = receiver_email
    server.sendmail(sender_email, receiver_email, msg.as_string())

server.quit()
print(&quot;邮件发送完成！&quot;)
```

#### 注意：

+   需要配置邮箱的 SMTP 设置（比如 Gmail 需要应用专用密码）。

+   适合批量处理大数据。

---

### 总结与建议

+   **简单需求**：用 VBA（方法 1）最直接，适合小规模使用。

+   **无编程经验**：试试 Word 邮件合并（方法 2）。

+   **自动化需求**：用 Power Automate 或 Python（方法 3/4）。

具体用哪种方法，取决于你的技术水平和需求复杂度。你可以告诉我你的 Excel 数据结构（比如列名、内容样式）和发送频率，我再帮你细化方案！怎么样，诸位老大觉得哪个靠谱？