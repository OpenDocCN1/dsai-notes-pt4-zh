
# L2 书生大模型 Browse Use 实践

> 来源：[https://aicarrier.feishu.cn/docx/Kpo2dBjgOofTWGxVhIgc0qOlnah](https://aicarrier.feishu.cn/docx/Kpo2dBjgOofTWGxVhIgc0qOlnah)

## 0. 课程开头

欢迎大家来到 `Intern-S1-Pro` 玩转 `BrowseUse`

本文将介绍如何利用 `书生 API` `Intern-S1-Pro`，实现控制浏览器完成查询天气、搜索小红书信息等。当然结合 `Intern-S1-Pro` 还有更多的有趣的案例，等待同学们自行探索~

![](img/f14f68197699084f92e04376116409b2.png)

## 2. 课程内容

### 2.1 书生 API 介绍及实践

`书生 API` 是上海人工智能实验室推出的核心产品之一，旨在为开发者提供便捷、高效的大模型调用能力。作为书生大模型全链路开源体系的重要组成部分，其依托 Intern 系列模型（如 `Intern-S1-Pro`、`InternLM3`、`InternVL3.5` 等）及工具链生态（`XTuner`、`LMDeploy` 等），支持文本交互、多模态处理及智能体开发。

本课程仅对 `API` 调用作基础讲解~，更多详细内容请参考 `https://internlm.intern-ai.org.cn/api/document`

#### 2.1.1 环境搭建

`API` 部分可以在本地or开发机上操作都可

##### 步骤 1. 安装依赖库

```
conda activate base
pip install requests openai dotenv
```

##### 步骤 2. 获取API Token

+   访问 `书生 AI` 官网 `https://internlm.intern-ai.org.cn/` 注册并申请 API 权限。

+   进入到API页面（`https://internlm.intern-ai.org.cn/api/tokens`）之后，点击获取个人密钥，接着创建 `API Token`。

+   给你的 `API Token` 取个名字，接着就能得到一串字符，注意，`API Token` 只能复制一次，点击确认之后，便不可见，妥善保管自己的 `API Token`。且在使用过程中，不要显式地暴露在代码中。

![](img/bc7bdc6a82f47b3eb317dac2f0b5538f.png)

##### 步骤 3. python代码调用

在 `/root/Internlm` 目录下

+   使用 `openai` python sdk

```
from openai import OpenAI
from dotenv import load_dotenv
import os

InternLM_api_key = os.getenv("InternLM", load_dotenv())
client = OpenAI(
    api_key=InternLM_api_key,  
    base_url="https://chat.intern-ai.org.cn/api/v1/",
)

chat_rsp = client.chat.completions.create(
     model="intern-s1-pro",
     messages=[{
            "role": "user",         #role 支持 user/assistant/system/tool
            "content": "你知道刘慈欣吗？"
    }, {
            "role": "assistant",
            "content": "为一个人工智能助手，我知道刘慈欣。他是一位著名的中国科幻小说家和工程师，曾经获得过多项奖项，包括雨果奖、星云奖等。"
    },{
            "role": "user",
            "content": "他什么作品得过雨果奖？"
    }],
    stream=False
)

for choice in chat_rsp.choices:
    print(choice.message.content)
#若使用流式调用：stream=True，则使用下面这段代码
#for chunk in chat_rsp:
#    print(chunk.choices[0].delta.content)
```

+   原生调用

```
import requests
import json
from dotenv import load_dotenv
import os
InternLM_api_key = os.getenv("InternLM", load_dotenv())

url = 'https://chat.intern-ai.org.cn/api/v1/chat/completions'
header = {
    'Content-Type': 'application/json',
    "Authorization": "Bearer " + InternLM_api_key,
}
data = {
    "model": "intern-s1-pro",  
    "messages": [{
        "role": "user",
        "content": "你好~"
    }],
    "n": 1,
    "temperature": 0.8,
    "top_p": 0.9
}

res = requests.post(url, headers=header, data=json.dumps(data))
print(res.status_code)
print(res.json())
print(res.json()["choices"][0]['message']["content"])
```

### 2.1.2 多模态交互

+   使用 `openai` python sdk

`"url": "https://static.openxlab.org.cn/internvl/demo/visionpro.png"` #支持互联网公开可访问的图片 url 或图片的 base64 编码

`"url": "https://static.openxlab.org.cn/puyu/demo/000-2x.jpg"`

```
from openai import OpenAI
from dotenv import load_dotenv
import os

InternLM_api_key = os.getenv("InternLM", load_dotenv())
client = OpenAI(
    api_key=InternLM_api_key,  
    base_url="https://chat.intern-ai.org.cn/api/v1/",
)

chat_rsp = client.chat.completions.create(
     model="intern-s1-pro",
     messages=[
        {
            "role": "user",
            "content": "你好"
        },
        {
            "role": "assistant",
            "content": "你好，我是 internvl"
        },
        {
            "role": "user",
            "content": [                                    #用户的图文提问内容，数组形式
                {
                    "type": "text",                        # 支持 text/image_url
                    "text": "Describe these two images please"
                },
                {
                    "type": "image_url",
                    "image_url": {
                    }
                },
                {
                    "type": "image_url",                                                     # 单轮对话支持上传多张图片
                    "image_url": {
                    }
                }
            ]
        }
    ],
    n=1,
    stream=False
)

for choice in chat_rsp.choices:
    print(choice.message.content)
```

### 2.1.3 三轮对话示例代码

`"url": "https://static.openxlab.org.cn/internvl/demo/visionpro.png"`

`"url": "https://static.openxlab.org.cn/puyu/demo/000-2x.jpg"`

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
        "content": "我可以处理多种图像任务：\\n1. 图像描述：详细描述图片内容\\n2. 视觉问答：回答关于图片的问题\\n3. 图像对比：分析多张图片的异同\\n4. OCR识别：提取图片中的文字\\n5. 图表分析：理解图表、图形数据\\n6. 场景理解：识别图片中的物体、人物和场景"
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
        model="intern-s1-pro",
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
    print("\\n=== 完整对话历史 ===")
    for i, msg in enumerate(messages):
        print(f"\\n轮次 {i//2 + 1}:")
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
你："帮我写个Python排序函数"（10 tokens）
AI："def sort_list(lst):..."（100 tokens）

费用 = 10×0.00001 + 100×0.00003 = $0.0031
```

当然现在的顶级模型api价格感人 ——然而 `书生api` 免费给大家玩！

![](img/eb1b001194290e8d1917a6210a642096.png)

### 2.2 Browser-Use 简介



Browser-Use 是一款专为 `Agent` 与浏览器交互而设计的工具，通过简洁而高效的自动化接口，使 `Agent` 能够轻松访问并操作网页。它为大模型与浏览器之间搭建了便捷的桥梁，帮助开发者快速实现网页自动化任务，而无需编写复杂代码。

其运行机制基于模型的多模态能力，并通过固定的 `JSON` 输出格式与浏览器交互。因此，模型的输出稳定性与能力水平直接决定了工具的整体效果。

## 2.3 开发环境配置

`Browser-use` 部分需在本地电脑 (os: Windows, macOS) 实现，方便观察 `Agent` 操控操作网页过程。

### 步骤 1. 安装 git

**Windows:**

+ [下载并安装](https://git-scm.com/download/win) 适合您 Windows 版本的安装程序：下载地址

+ 按照安装向导完成安装。（默认设置安装即可）

+ 打开终端，输入指令 `git --version` 检查是否安装成功

**MacOS:**

+ [下载并安装](https://git-scm.com/downloads/mac) 适合您 MacOS 版本的安装程序：下载地址

+ 按照安装向导完成安装。（默认设置安装即可）

+ 打开终端，输入指令 `git --version` 检查是否安装成功

安装成功后，使用 `git` 将 `browser-use` 项目克隆到本地

```
git clone https://github.com/fak111/Web-ui.git
cd Web-ui
```

### 步骤 2. 安装 uv 并创建项目环境

这里我们使用 `uv` 进行项目环境管理，直接使用 Python 自带的 `pip` 安装，兼容性最佳，这里需要 Python 版本为 3.8 及以上：

```
pip install uv  
# 检测是否安装成功，出现版本号则成功
uv --version 
```

![](img/864eaff976573f90298c665c2224c11f.png)

如果没安装 Python 需要去 [Python 官网](https://www.python.org/) 下载安装包，安装时勾选 “Add Python to PATH”（安装向导底部），并选择 自定义安装 → 确保勾选 “Install pip”。

另外，如果网速不行 `pip` 下载包的时候可以加 `-i https://pypi.tuna.tsinghua.edu.cn/simple/` 起到一个换下载源加速的作用。

接着输入以下命令来创建项目环境并进入

```
# 第一步：创建环境（只需执行一次）
uv venv Internlm --python 3.11

# 第二步：根据你的操作系统激活环境（每次使用项目时都需要）
# 如果是 Windows：
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\Internlm\Scripts\Activate.ps1

# 如果是 macOS：
source Internlm/bin/activate
```

如下所示，安装了 `uv` 环境：

![](img/4026062dde612a3037ad4edea6a590b7.png)

### 步骤 3. 在项目环境下安装依赖

安装项目依赖

```
uv pip install -r requirements.txt  
uv pip install --upgrade gradio  -i https://pypi.tuna.tsinghua.edu.cn/simple/
uv pip install "httpx[socks]" -i https://pypi.tuna.tsinghua.edu.cn/simple/
playwright install --with-deps chromium
playwright install
```

`web-ui` 下有个 `.env.example` 文件，这是我们项目的启动配置，复制一份并重命名为 `.env`，然后填写我们的 key

```
# macOS/Linux/Windows (PowerShell):
cp .env.example .env
```

![](img/cc4871493b91139dbe87ed60d471ef46.png)

## 2.4 启动 Browser-use Web-UI

### 2.4.1 运行 Web-UI

运行 WebUI，完成上面的安装步骤后，启动应用程序：

```
python webui.py --ip 127.0.0.1 --port 7788
```

启动后，打开 `http://127.0.0.1:7788` 即可看到以下界面

### 2.4.2 Web-UI 选项配置

在 `LLM Settings` 下选择 `Intern`

模型选择 `intern-s1`

`API KEY` 位置填入你的 `API Token`

`API KEY` 也可以直接写入 `.env` 文件中

### 2.4.3 案例一：查看上海天气

在 `Run Agent` 中输入任务，点击 `Run Agent`

例如 "去百度查一下今天上海的天气，给我结果" ，模型执行过程如下

在 `Recordings` 界面可以把 `Agent` 执行过程下载下来

![](img/e70d952e6b6cb11ba05eb16903327b9c.png)

![](img/246fefa0f4e2fbb6304d15ecbd625105.png)

后台显示如图：

![](img/79fa730bd1f8514167c587f76725adfd.png)

### 2.4.4 案例二：去📕搜索东方明珠

![](img/79397e998d19b6bf24b3090c2e5c1358.png)

[agent_history (1).gif](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SiU5bBB0uoingrxkQ2OcsCQ0n4d)

在执行该任务时，`GUI Agent` 在登录界面卡住。我手动完成了扫码登录操作，但它在输入框中输入“东方明珠”后并未点击搜索按钮，因此我代为完成了点击操作。这也反映出此类技术仍存在诸多挑战，未来各位开发者可以在这一方向上还有广阔的探索与突破空间。

## 闯关任务

1. 构建一个包含 3 轮上下文的对话 `&&` 发送一张图片并获取描述。

2. 自行探索，并使用 `Browser-use Web-UI` 自带的录制功能，将书生大模型操作浏览器过程录制下来，放到作业中。

作业提交地址：https://aicarrier.feishu.cn/share/base/form/shrcnbguDXSzIZV5Dkapw39BJ0f

本文参考：

[L1G1-玩转书生大模型 API 之 Browser-Use 实践](https://aicarrier.feishu.cn/wiki/XhoNw2qzPir8qmkVNYyc5YAunrc)
