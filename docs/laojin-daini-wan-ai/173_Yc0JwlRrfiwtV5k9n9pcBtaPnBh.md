
# 拯救AI编程乱码显示！老金一招教你完全避免！Claude Code/Codex/Cursor等通用！

> 来源：https://my.feishu.cn/docx/ILyNdYN98oXhIlxQ7xxcvvLfnye

## 每个AI编程的使用者都遇到过

被软件修改后，文件变成乱码的情况。

Claude Code、Codex、Cursor，一个都跑不了。

老金我测试了网上的N种办法。找到了最有效直接的。

下载1个软件就搞定！

问题不在AI工具，而在终端。

这些工具生成代码时，都是通过终端执行命令来写入文件。

关键在于PowerShell的编码。

不是显示的问题！

---

## 四步彻底解决

### 第一步：检查PowerShell版本

**Windows用户**：

- 按 `Win + R`，输入 `powershell`，回车

**Mac用户**：

- 按 `Cmd + 空格`，输入 `terminal`，回车

在终端输入：

记住，在终端里复制是鼠标右键，不是 `Ctrl+V`！

```
$PSVersionTable.PSVersion
```

如果显示5.x，说明需要升级。

![](img/4ea1430ff2da0a40918f385a13499840.png)

### 第二步：安装PowerShell 7

直接到Github上下载。

> PowerShell 7官方下载：https://github.com/PowerShell/PowerShell/releases

![](img/036fc4c32b9a54faca410917810f2fcf.png)

如果不知道选哪个，甩给AI问一下。

安装后检查：

```
$PSVersionTable.PSVersion
```

显示7.x就对了。

![](img/82d3723bc08ea20374ddc0b13156c88d.png)

### 第三步：配置UTF-8编码

在PowerShell 7里输入：

```
$PROFILE
```

![](img/5dae665d0f83772ff733b338108e0ce5.png)

会显示配置文件路径，打开文件后，然后添加这些内容：

```
强制UTF-8编码
chcp 65001 | Out-Null
[Console]::InputEncoding = [System.Text.UTF8Encoding]::new()
[Console]::OutputEncoding = [System.Text.UTF8Encoding]::new()
$OutputEncoding = [System.Text.UTF8Encoding]::new()

默认文件编码
$PSDefaultParameterValues['Out-File:Encoding'] = 'utf8'
$PSDefaultParameterValues['Set-Content:Encoding'] = 'utf8'
$PSDefaultParameterValues['Add-Content:Encoding'] = 'utf8'
```

保存后重启PowerShell。

### 第四步：设置VS Code使用PowerShell 7

按 `Ctrl+Shift+P`，搜索 `Terminal: Select Default Profile`。

![](img/a023b2d4766951ca2aebdb09b1103b25.png)

选择 `PowerShell`。

![](img/83e91422bc758fc1e886aaaeea9a6a8e.png)

然后在VS Code设置中添加：

```
{
    "files.encoding": "utf8",
    "files.autoGuessEncoding": true,
    "terminal.integrated.defaultProfile.windows": "PowerShell"
}
```

如果你打不开，就直接按照图点击。

![](img/e41e1178c2775da5f347eb70fa89d03e.png)

![](img/0852b3dc6590372d65e59cc1808fe0c8.png)

![](img/a50cb3a458b7b88ec81e91c4742cd7b2.png)

---

如果你想知道原理，在PowerShell中输入：

```
[Console]::OutputEncoding
```

![](img/36b9122dc8f66e3091facd742f642728.png)

![](img/2667a470440b0306a19ed95a1cd14bd5.png)

我们会发现它从gb2312变成了utf-8。

前面的说明你的控制台是GB2312编码，而CLI默认是UTF-8管道输出，此时就会出现乱码。

https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc
