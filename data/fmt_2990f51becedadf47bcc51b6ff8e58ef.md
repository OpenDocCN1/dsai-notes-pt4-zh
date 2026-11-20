
# 微学习05 | Streamlit+PyMuPDF：解析PDF论文

> 来源：[https://datawhaler.feishu.cn/docx/Fmm4dpfgioktssxt3Oncwwdun0g](https://datawhaler.feishu.cn/docx/Fmm4dpfgioktssxt3Oncwwdun0g)

## 大模型应用极简开发快速入门

✨你好，我是筱可，欢迎来到「筱可AI研习社」

🚀 标签关键词：| AI实战派开发者 | 技术成长陪伴者 | RAG前沿探索者 |

本次的主题是：

入门《基于 `Streamlit` 和 `DeepSeek` 的智能文档助手开发实战指南》

当然了，主要的目的是主题上写的，本篇文章也不仅仅是介绍这些库，还有其他惊喜哦！

（我们赶快进入本次的重点吧！）

通过本文你将收获：

+ 🛠️ 环境变量管理的艺术

+ 🔥 `OpenAI SDK` 高阶用法

+ 💡 `Streamlit` 交互魔法

+ 📄 PDF 解析技巧

下篇文章预告：

主题是，业界前沿的pdf提取技术！

---

### ❓前言：来自读者的求助

上一期我们通过《基于 `Streamlit` 和 `DeepSeek` 的智能文档助手开发实战指南》带着大家体验如何在本地构建一个具有基础文档交互能力的智能文档助手，不过有的读者觉得我写的不够详细，但是由于当时篇幅已经很长了，我也不好写太细了，所以本次开一篇文章具体介绍我们上次使用到的一些开发生态库当中的功能，以及用法，也会提到为什么用这些库的原因，还有这些生态库都有哪些缺点和优点，后续我们应该使用什么方式去改进。

### 🔐一、环境变量管理（`python-dotenv`）

环境变量一般长这样（`.env`）：

![](img/a29e26261f67c679e3194d5075a50d30.png)

```
import os

def find_dotenv_path(dir_name: str = ""):
    # 首先在当前工作目录下查找 .env 文件
    current_working_dir = os.getcwd()
    env_path = os.path.join(current_working_dir, ".env")
    if os.path.exists(env_path):
        return env_path

    # 如果没有指定目录名，且当前工作目录下没有 .env 文件，则返回 None
    if not dir_name:
        return None

    # 从当前脚本所在目录开始向上查找指定目录
    current_dir = os.path.dirname(os.path.abspath(__file__))
    while True:
        # 检查当前目录是否为指定目录
        if os.path.basename(current_dir) == dir_name:
            # 在指定目录下查找 .env 文件
            env_path_in_specified_dir = os.path.join(current_dir, ".env")
            if os.path.exists(env_path_in_specified_dir):
                return env_path_in_specified_dir
            else:
                return None
        # 到达根目录时停止查找
        if current_dir == os.path.dirname(current_dir):
            break
        # 向上移动一层目录
        current_dir = os.path.dirname(current_dir)

    # 如果没有找到指定目录，返回 None
    return None
```

上面我们主要是定义了一个用来找到环境变量文件的地址的函数。

方面后续调用找到加载对应的环境变量，避免文件配置混乱,主要还是因为方便拉取我仓库的人能快速配置好环境变量就直接跑。

上面的代码我们也优化了上次展示的获取环境变量文件地址的逻辑，如果没有找到就返回`None`，后续我们在接收返回值的部分可以通过检查是否是字符串判断环境变量地址是不是被找到了。

🚨 安全规范：

+ 🚫 禁止硬编码敏感信息

+ 📌 强制`.gitignore`添加`.env`

+ 🔄 使用`dotenv`自动加载环境变量

---

### ⚙️二、`OpenAI SDK` 深度解析

#### 介绍

`OpenAI SDK`是`openai`公司开源的一个默认用来调用他们公司的模型的一个库，但是他支持配置`base_url`（我猜是为了让大家都支持他的规范，扩大业界影响力，去年和前年`openai`确实遥遥领先，当然现在也是领先吧，只是没那么多了，那个时候确实是业界规范制定者），所以我们还可以通过其他的模型服务商通过这个库进行调用模型服务供应商的大语言模型。

他的作用主要是简化我们调用模型的代码，底层也是通过类似`requests`发送http请求的形式向模型服务端请求服务，我看了源码，是通过`httpx`封装的。

![](img/83ac75902d97d30ee34cdb8f4eaa218a.png)

#### 参数介绍(以阿里云的文档举例)

参考资料地址：

+ https://help.aliyun.com/zh/model-studio/developer-reference/use-qwen-by-calling-api#b30677f6e9437

| 参数名称 | 参数类型 | 是否必选 | 说明 |

| --- | --- | --- | --- |

| `model` | string | 是 | 模型名称，支持通义千问系列及数学、代码模型等。通义千问Audio仅支持DashScope方式 |

| `messages` | array | 是 | 历史对话消息列表 |

| `stream` | boolean | 否 | 是否流式输出，`false`为一次性返回，`true`为边生成边输出，默认`false` |

| `stream_options` | object | 否 | 流式输出时，设置`{"include_usage": true}`可显示Token数，仅`stream`为`true`时生效 |

| `temperature` | float | 否 | 采样温度，控制文本多样性，范围[0, 2)，与`top_p`选其一设置 |

| `top_p` | float | 否 | 核采样概率阈值，控制文本多样性，范围(0,1.0]，与`temperature`选其一设置 |

| `presence_penalty` | float | 否 | 控制内容重复度，范围[-2.0, 2.0] ，正数减少、负数增加重复度 |

| `response_format` | object | 否 | 返回格式，可选`{"type": "text"}`或`{"type": "json_object"}`，默认`{"type": "text"}` |

| `max_tokens` | integer | 否 | 最大Token数，超量返回截断内容，默认和最大为模型最大输出长度 |

| `n` | integer | 否 | 生成响应个数，范围1 - 4，仅`qwen-plus`支持，传入`tools`时固定为1，默认1 |

| `seed` | integer | 否 | 使文本生成更确定，范围0到231−1 |

| `stop` | string/array | 否 | 生成文本含指定内容时停止，`array`时不能同时含`token_id`和字符串 |

| `tools` | array | 否 | 模型调用工具数组，不支持通义千问VL/Audio及数学、代码模型 |

| `tool_choice` | string/object | 否 | 工具选择策略，可选`"auto"` `"required"` `"none"` 或指定工具，默认`"auto"` |

| `parallel_tool_calls` | boolean | 否 | 是否开启并行工具调用，默认`false` |

| `translation_options` | object | 否 | 翻译模型配置参数，Python `SDK`调用时通过`extra_body`配置 |

| `enable_search` | boolean | 否 | 是否用互联网搜索参考，`true`启用、`false`关闭，默认`false`，仅部分模型支持 |

#### 具体示例代码

```
from openai import OpenAI

import fitz
import openai
import os
import dotenv

env_path = find_dotenv_path()

dotenv.load_dotenv(dotenv_path=env_path)

api_key = os.getenv("API_KEY")

client = OpenAI(api_key=api_key,
                base_url="https://api.siliconflow.cn/v1")
model = "Pro/deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B"
messages = []

# 使用非流式输出测试下api是否可用
response = client.chat.completions.create(
    model=model,
    messages=[{"role": "system", "content": "你好"}],
)
print(response.choices[0].message.content, end='', flush=True)
print("\n")
while True:
    user_input = input("你: ")
    if user_input.lower() in ["exit", "quit"]:
        break

    messages.append({"role": "user", "content": user_input})
    
    response = client.chat.completions.create(
        model=model,
        messages=messages,
        stream=True
    )
    index = 0
    for chunk in response:
        chunk_message = chunk.choices[0].delta.content
        if not chunk_message:
            continue
        if index == 0:
            index += 1
            print(f"AI: {chunk_message}", end='', flush=True)
        else:
            print(chunk_message, end='', flush=True)
    
    messages.append({"role": "assistant", "content": chunk_message})
    print()  # 换行
```

上面我们主要是分别使用`openai`构建了流式输出和非流式输出的示例，在流式输出部分我们通过`messages`构建了一个支持对话历史形式的终端对话脚本，刚入门的小伙伴可以试试，这是使用最简单的形式构建与大模型进行多轮对话了。（调用其他的库除外哈，仅限于我们使用`openai sdk`构建多轮对话）

`print`函数的高级用法在上面有使用到哦，不知道有没有同学注意到，嘿嘿，`flush`是细节！

感兴趣的同学可以试试去掉或者改成`False`，你会发现不一样的结果！

**流式 vs 非流式对比：**

+ 流式：实时性高，适合面向用户对话场景的长文本生成，及时响应，可以大大减轻用户的等待焦虑，提升用户体验。

+ 非流式：响应完整，适合构建工作流或者需要模型进行多次加工的时候使用。

### 🎨三、`Streamlit` 交互魔法
