
```
from openai import OpenAI
from dotenv import load_dotenv
import os

# 加载环境变量
load_dotenv()
InternLM_api_key = os.getenv("InternLM")

# 初始化客户端
client = OpenAI(
    api_key=InternLM_api_key,  
    base_url="https://chat.intern-ai.org.cn/api/v1/",
)

# 构建包含3轮对话的上下文
messages = [
    # 第一轮对话
    {
        "role": "user",
        "content": "你好，请介绍一下你自己"
    },
    {
        "role": "assistant",
        "content": "你好！我是 InternVL，一个多模态AI助手。我可以理解和分析文本与图像，帮助您处理各种任务，包括图像描述、视觉问答、文档理解等。有什么我可以帮助您的吗？"
    },
    
    # 第二轮对话
    {
        "role": "user",
        "content": "你能处理哪些类型的图像任务？"
    },
    {
        "role": "assistant",
        "content": "我可以处理多种图像任务：\n1. 图像描述：详细描述图片内容\n2. 视觉问答：回答关于图片的问题\n3. 图像对比：分析多张图片的异同\n4. OCR识别：提取图片中的文字\n5. 图表分析：理解图表、图形数据\n6. 场景理解：识别图片中的物体、人物和场景"
    },
    
    # 第三轮对话 - 包含图像
    {
        "role": "user",
        "content": [
            {
                "type": "text",
                "text": "请分析这两张图片，告诉我它们分别展示了什么产品，以及它们的主要特点"
            },
            {
                "type": "image_url",
                "image_url": {
                }
            },
            {
                "type": "image_url",
                "image_url": {
                }
            }
        ]
    }
]

# 调用API获取最终回复
try:
    chat_rsp = client.chat.completions.create(
        model="internvl2.5-latest",
        messages=messages,
        n=1,
        stream=False,
        temperature=0.8,  # 控制回复的创造性
        max_tokens=500    # 限制回复长度
    )
    
    # 打印最终回复
    print("=== API 最终回复 ===")
    for choice in chat_rsp.choices:
        print(choice.message.content)
        
    # 可选：打印整个对话历史
    print("\n=== 完整对话历史 ===")
    for i, msg in enumerate(messages):
        print(f"\n轮次 {i//2 + 1}:")
        if msg["role"] == "user":
            if isinstance(msg["content"], str):
                print(f"用户: {msg['content']}")
            else:
                print(f"用户: {msg['content'][0]['text']}")
                print(f"      (包含 {len(msg['content'])-1} 张图片)")
        else:
            print(f"助手: {msg['content']}")
            
except Exception as e:
    print(f"API调用出错: {str(e)}")
```

Model api计费规则示例：

```
# GPT-4 Turbo 价格（2024年）
输入价格：$0.01 / 1K tokens
输出价格：$0.03 / 1K tokens（输出更贵！）

# 一轮对话
你：“帮我写个Python排序函数”（10 tokens）
AI：“def sort_list(lst):...”（100 tokens）

费用 = 10×0.00001 + 100×0.00003 = $0.0031
```

当然现在的顶级模型api价格感人，书生api免费给大家玩！

![](img/a74ee3594a447e631ec487913ba2dbe1.png)

## 2.2 Browser-Use 简介

Browser-Use 是一款专为 Agent 与浏览器交互设计的工具，旨在通过简单而强大的自动化界面，让 Agent 轻松访问和操作网页。它提供了连接大模型与浏览器的便捷桥梁，使开发者能够快速实现网页自动化任务，无需复杂编码。

## 2.3 开发环境配置

此项目需在本地电脑(os:Windows,macOS)实现,方便观察 Agent 操控操作网页过程~

### 步骤 1.安装 `git`

**Windows:**

+   [下载并安装](https://git-scm.com/download/win)适合您 `Windows` 版本的安装程序：下载地址

+   按照安装向导完成安装。(默认设置安装即可)

+   打开终端，输入指令 `git --version` 检查是否安装成功

**MacOS:**

+   [下载并安装](https://git-scm.com/downloads/mac)适合您 `MacOS` 版本的安装程序：下载地址

+   按照安装向导完成安装。(默认设置安装即可)

+   打开终端，输入指令 `git --version` 检查是否安装成功

### 步骤 2.安装 `uv` 并创建项目环境

这里我们使用 `uv` 进行项目环境管理

首先我们新建一个 `Internlm` 文件夹，并用 `vscode` 打开，启动终端

直接使用 `Python` 自带的 `pip` 安装，兼容性最佳，这里需要 `python` 版本为 `3.8` 及以上：

```
pip install uv  
# 检测是否安装成功，出现版本号则成功
uv --version 
```

如果没安装 `python` 需要去 [Python 官网](https://www.python.org/) 下载安装包，安装时勾选 `Add Python to PATH`（安装向导底部），并选择 自定义安装 → 确保勾选 `Install pip`。

另外，如果网速不行 `pip` 下载包的时候可以加 `-i https://pypi.tuna.tsinghua.edu.cn/simple/` 起到一个换下载源加速的作用。

接着输入以下命令来创建项目环境并进入

```
uv venv Internlm --python 3.11
#Activate the virtual environment:
#Windows (PowerShell):
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\Internlm\Scripts\Activate.ps1
#macOS
source Internlm/bin/activate
```

如下所示，进入了项目环境

![](img/726b1283abee54505d9ea5d16ede2284.png)

### 步骤 3.在项目环境下安装依赖

首先使用 `git` 将 `browser-use` 项目克隆到本地

```
git clone https://github.com/sanjion/Web-ui.git
cd web-ui
```

安装项目依赖

```
uv pip install -r requirements.txt  
playwright install --with-deps chromium
```

`web-ui` 下有个 `.env.example` 文件，这是我们项目的启动配置，复制一份并重命名为 `.env`

```
# Windows (PowerShell):
cp .env.example .env
# macOS/Linux/Windows (PowerShell):
cp .env.example .env
```

## 2.3 启动 Browser-use Web-UI

### 2.3.1 运行 Web-UI

运行 `WebUI`，完成上面的安装步骤后，启动应用程序：

```
python webui.py --ip 127.0.0.1 --port 7788
```

若启动时遇到以下错误

```
ERROR:    Exception in ASGI application
……
TypeError: argument of type 'bool' is not iterable
```

将 `gradio` 升级到最新版本即可解决

```
uv pip install --upgrade gradio  -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

缺相应包 `httpx[socks]` 安装就好了

```
uv pip install "httpx[socks]" -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

启动后，打开 `http://127.0.0.1:7788` 即可看到以下界面

### 2.3.2 Web-UI 选项配置

在 `LLM Settings` 下选择 `Intern`

模型选择 `internvl-latest`

(如果选择 `Internlm`，注意把 `Agent Settings` 中的 `Use Vison` 选项去掉，`Internlm` 是文本模型，不支持图像识别)

`API KEY` 位置填入你的 `API Token`

`API KEY` 也可以直接写入 `.env` 文件中

### 2.3.3 案例一：查看上海天气

在 `Run Agent` 中输入任务，点击 `Run Agent`

例如 `"去百度查一下今天上海的天气，给我结果"` ,模型执行过程如下

![](img/c90873fb153dc0b39a876c291b11e68a.png)

![](img/f3b463e1bf2e360118d8a279f6f69341.png)

在 `Rcordings` 界面可以把 Agent 执行过程下载下来

![](img/e932c9e86d756749a64548aaa4426211.png)

### 2.3.4  案例二：拿书生 api-key

```
step1 访问https://internlm.intern-ai.org.cn/api/tokens这个网站
step2 等我大概一分钟，你再操作
step3  访问https://internlm.intern-ai.org.cn/api/tokens 获取到token 命名test123 。
step4 token结果给我。
```

![](img/784471e22d9aed5e634f7cdf42fda744.png)

更多的有趣的案例请同学们自行探索~
