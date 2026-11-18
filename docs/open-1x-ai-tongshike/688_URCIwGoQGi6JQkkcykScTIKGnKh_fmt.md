
# 用DeepSeek生成科技文章Markdown

> 来源：[https://datawhaler.feishu.cn/docx/E2Znd7vqDoTQT8xzN6SccMKOnCe](https://datawhaler.feishu.cn/docx/E2Znd7vqDoTQT8xzN6SccMKOnCe)

## 使用指南

### 1. 程序简介

**主要功能：**

+ 从指定的网站收集信息。
+ 基于收集到的信息，获得文章主题建议。
+ 根据你选的主题，得到吸引人的标题和大纲选项。
+ 自动撰写文章的各个段落。
+ 对生成的内容进行初步的语言润色。
+ 最终输出一篇结构化的 Markdown 格式文章，方便后续编辑。

**目标用户：**

+ 需要快速生成科技文章初稿的内容创作者。
+ 想体验 AI 写作应用的学习者。
+ 对自动化工具感兴趣的任何人。

**重要提醒：** 本工具生成的是 **初稿**。AI 的内容可能需要你进行事实核查、观点补充和文字修改，才能达到最终发布标准。此外，现有版本在网络爬虫技术上有待进一步优化，上下文生成内容在逻辑连贯性方面有待进一步提高，这些都是后续程序进一步优化方向。

### 2. 预期运行效果

当你按照本指南成功设置并在 PyCharm 或 VS Code 中运行此程序后，你将体验到：

1. 在 IDE 内部交互：程序会在 PyCharm 或 VS Code 下方的 `Terminal` 或 `Run` 窗口中启动。你会看到文字提示，并需要在此窗口中输入你的选择（比如输入数字或文字后按 Enter 键）。

2. 智能建议：程序会调用 Deepseek AI，在终端窗口显示文章主题、标题和大纲的建议。

3. 自动化流程：程序根据你的选择，自动完成内容抓取、分析、撰写和润色，并在终端显示进度。

4. 最终成果：

+ 程序运行结束后，你会在 IDE 的终端窗口看到生成的完整文章内容。
+ 同时，在你存放项目的文件夹内，会自动创建一个名为 `articles` 的子文件夹（如果原来没有）。
+ 在这个 `articles` 文件夹里，会有一个 `.md` 文件。文件名是你之前选定或输入的文章标题。这个文件就是 Markdown 格式的文章初稿。
+ 你可以直接在 PyCharm 或 VS Code 中双击打开这个 `.md` 文件查看和编辑，它们都支持 Markdown 预览。

### 3. 运行前的准备

在你开始之前，请确保你的电脑已准备好以下几样东西：

1. 安装 Python 3：这是程序运行的基础。

+ 如何检查/安装：
+ 访问 Python 官网：`https://www.python.org/downloads/`
+ 下载适合你操作系统 (Windows/macOS) 的最新 Python 3 版本。
+ Windows 用户特别注意：在安装过程中，**务必勾选 "Add Python to PATH"** 或类似的选项，这会让 PyCharm/VS Code 更容易找到 Python。然后点击 "Install Now"。
+ macOS 用户：下载 `.pkg` 安装包并按提示安装。macOS 通常自带 Python，但建议安装官网最新版以获得更好兼容性。

1. 安装 IDE (集成开发环境)：你需要选择 PyCharm 或 VS Code 中的一个。它们是强大的代码编辑器，能让运行 Python 程序更方便。

+ PyCharm：
+ 访问 JetBrains 官网：`https://www.jetbrains.com/pycharm/download/`
+ 下载并安装 Community 版本（免费）。

+ Visual Studio Code (VS Code)：
+ 访问 VS Code 官网：`https://code.visualstudio.com/download`
+ 下载并安装适合你操作系统的版本。

1. 获取 Deepseek API 密钥：这是使用 AI 功能的“钥匙”。

+ 访问 Deepseek 官方平台 (`https://platform.deepseek.com/` 或类似官方地址) 并注册账号。
+ 登录后，找到 API 管理或密钥生成的部分，创建一个新的 API 密钥 (通常以 `sk-` 开头)。
+ **非常重要：** 复制并**妥善保存**这个密钥，不要分享给他人。使用 API 可能产生费用，请了解 Deepseek 的定价策略。

1. **稳定的网络连接：** 程序需要联网获取网页内容和调用 AI 服务。

### 4. 详细使用步骤

**步骤 1：准备项目文件夹和代码**

1. 在你的电脑上（比如“文档”或“桌面”）创建一个新的文件夹，专门用于存放这个项目。可以命名为 `ArticleGenerator`。
1. 将你获得的 `main.py` 文件放入这个 `ArticleGenerator` 文件夹中。

**步骤 2：配置 Deepseek API 密钥**

这一步需要在 IDE 中完成。

1. 打开项目文件夹：

+ PyCharm：启动 PyCharm。选择 "Open" (打开)，然后找到并选择你创建的 `ArticleGenerator` 文件夹。
+ VS Code：启动 VS Code。点击菜单栏 "File" (文件) -> "Open Folder..." (打开文件夹...)，然后找到并选择 `ArticleGenerator` 文件夹。

1. 找到 `main.py`：在 IDE 左侧的文件浏览器 (Project / Explorer) 中，你应该能看到 `main.py` 文件。双击它在编辑器中打开。

1. 修改代码：在 `main.py` 文件的开头，找到这行：

```
DEEPSEEK_API_KEY = "替换成你的API Key" # 请替换成你的实际 API Key
```

1. 粘贴你的密钥：将引号 "" 内的示例密钥 (替换成你的API Key) **完全删除**，然后粘贴你自己的、真实的 Deepseek API 密钥。确保你的密钥仍然被英文双引号包裹。例如：

```
DEEPSEEK_API_KEY = "你的真实Deepseek密钥粘贴在这里" # 请替换成你的实际 API Key
```

1. 保存文件：按 Ctrl+S (Windows) 或 Cmd+S (macOS) 保存你对 `main.py` 的修改。

**步骤 3：安装程序依赖库**

程序需要一些额外的 Python 工具包才能运行。你需要在 IDE 内置的终端中安装它们。

1. 打开 IDE 内的终端：

+ PyCharm：通常在 IDE 窗口的底部会有一个 "Terminal" (终端) 标签，点击它。如果没有，可以从菜单栏 "View" (视图) -> "Tool Windows" (工具窗口) -> "Terminal" 打开。
+ VS Code：点击菜单栏 "Terminal" (终端) -> "New Terminal" (新建终端)。一个终端窗口会出现在 IDE 底部。

1. 安装依赖：在打开的终端窗口中，**输入**以下命令，然后按 Enter 键：

```
pip install requests beautifulsoup4 markdown tqdm
```

+ **注意：** 如果你的系统（尤其是 macOS 或 Linux 安装了多个 Python 版本）提示 `pip` 命令找不到或关联的是旧版 Python，请尝试使用 `pip3`：

```
pip3 install requests beautifulsoup4 markdown tqdm
```

+ 这个命令会自动下载并安装程序运行所需的 `requests`, `beautifulsoup4`, `markdown`, 和 `tqdm` 库。等待安装完成，期间需要保持网络连接。你会看到下载和安装的进度信息。

**步骤 4 (仅限 VS Code)：安装 Python 扩展**

如果你是第一次在 VS Code 中使用 Python，需要安装官方的 Python 扩展。

1. 点击 VS Code 左侧边栏的扩展 (Extensions) 图标（看起来像几个方块）。
1. 在搜索框中输入 `Python`。
1. 找到由 Microsoft 发布的那个 `Python` 扩展，点击 Install (安装)。

**步骤 5：运行程序**

现在一切准备就绪，可以运行脚本了！

+ **PyCharm：**

1. 确保 `main.py` 文件在编辑器中是打开并激活的状态。
1. 找到编辑器右上角的绿色三角形 ▶️ (Run) 按钮，点击它。
+ 或者，在 `main.py` 编辑器窗口内右键单击，选择 "Run 'main'"。
1. 程序的输出和交互提示会出现在 IDE 底部名为 "Run" 或 "Terminal" 的窗口中。

+ **VS Code：**

1. 确保 `main.py` 文件在编辑器中是打开并激活的状态。
1. 通常在编辑器右上角会有一个绿色三角形 ▶️ (Run Python File) 按钮，点击它。
+ 或者，在 `main.py` 编辑器窗口内右键单击，选择 "Run Python File in Terminal"。
1. 程序的输出和交互提示会出现在 IDE 底部的 `Terminal` (终端) 窗口中。

**步骤 6：按照程序提示进行交互**

程序运行后，它会在 IDE 的终端/运行窗口中显示欢迎语和步骤说明。你需要：

1. 阅读提示：仔细看程序要求你做什么。
1. 输入信息：根据提示，在终端/运行窗口中输入相应的数字（如选择项的序号）或文字（如网址、自定义主题、标题等）。
1. 按 Enter：每次输入完成后，按 Enter 键确认。
1. 耐心等待：在某些步骤（如内容收集、AI 生成），程序需要一些时间处理，你会看到进度条。请耐心等待它完成。
1. 完成流程：按照提示一步步完成内容收集 -> 主题选择 -> 标题选择 -> 大纲选择 -> (自动撰写) -> (自动润色) -> (自动保存) -> 内容展示。
1. 反馈：最后程序会显示生成的文章，并询问你的反馈。输入“满意”结束程序，或输入“修改”（程序会提示你需要重新运行）。

**步骤 7：查看结果**

+ 终端输出：在 IDE 的终端/运行窗口可以看到最终生成的文章文本。
+ Markdown 文件：

+ 在 IDE 左侧的文件浏览器 (Project / Explorer) 中，展开你的 `ArticleGenerator` 文件夹。
+ 你会看到一个名为 `articles` 的新文件夹。
+ 展开 `articles` 文件夹，里面就是以你文章标题命名的 `.md` 文件。
+ **双击** 这个 `.md` 文件，可以在 PyCharm/VS Code 中直接打开预览和编辑。

### 5. 重要提示与限制

+ **网页抓取质量：** 程序提取网页文本的方式比较基础，可能混入广告、导航等无关内容，影响后续 AI 处理效果。复杂网站效果可能打折扣。
+ **AI 内容可靠性：** AI 生成的内容仅供参考，务必进行人工审核、事实核查和修改，不能直接作为最终稿发布。
+ **API 密钥安全与费用：** 再次强调，保管好你的 API 密钥。使用 Deepseek API 可能产生费用，请关注其计费规则和你的账户用量。
+ **网络依赖：** 网络不稳定或目标网站访问限制可能导致程序出错。
+ **错误处理：** 程序虽有基本错误处理，但无法覆盖所有异常情况。如遇问题，可尝试重新运行或检查网络/API 密钥。

### 6. 简单故障排除

+ 命令找不到 (`pip`/`python`)：确保 Python 安装正确，特别是 Windows 下勾选了 "Add to PATH"。在 IDE 终端尝试 `python --version` 或 `python3 --version` 看是否能正确显示版本。
+ **依赖安装失败：** 检查网络连接。尝试使用 `pip3` 代替 `pip`。
+ **API 错误/认证失败：** 仔细检查 `main.py` 中的 API 密钥是否正确复制粘贴，没有多余的空格或字符。确认你的 Deepseek 账户和密钥状态正常且有额度。
+ **程序卡住或无响应：** 可能是网络慢或 AI 处理时间长，请稍等。如果长时间无响应，可能需要停止程序（通常是点击终端旁边的红色方块 ⏹️ 或按 Ctrl+C）并重试。

---

> 本文作者：@
<https://datawhaler.feishu.cn/sync/LfRzdAExlsoykRbUZymc0Jrfngh>

## 欢迎投稿参与共建、下次微学习就学你的内容！

<https://datawhaler.feishu.cn/sync/CPNrdTvwgshasbb2UjecLzVinPe>
