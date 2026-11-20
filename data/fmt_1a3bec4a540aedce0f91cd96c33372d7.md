
# Agents SDK+DeepSeek+MCP智能体开发实战（下）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Uvwmdabi3oLdAdxzWbScpdXlnWQ](https://kq4b3vgg5b.feishu.cn/docx/Uvwmdabi3oLdAdxzWbScpdXlnWQ)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(春季班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(春季班)

![](img/d8d904157864b842b73dd2ff19baa7df.png)

![](img/0d3c3216712ed53c5e9c86a2c70b887d.png)

此外，公开课全套学习资料，已上传至网盘

（https://pan.baidu.com/s/1fHbHoyjCpqRIDwsqtujKdw?pwd=xarb）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## 《2025大模型Agent智能体开发实战》体验课

### OpenAI开源Agents SDK + DeepSeek + MCP智能体开发实战（下）

```python
import openai
```

```python
openai.__version__
```

```python
'1.70.0'
```

```python
from openai import OpenAI
import os
from dotenv import load_dotenv
load_dotenv(override=True)
from openai import AsyncOpenAI
from agents import OpenAIChatCompletionsModel, Agent, Runner, set_default_openai_client
from agents.model_settings import ModelSettings
from contextlib import AsyncExitStack
```

```text
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: max retries reached, giving up on this batch.
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: max retries reached, giving up on this batch.
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: max retries reached, giving up on this batch.
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: max retries reached, giving up on this batch.
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: max retries reached, giving up on this batch.
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: max retries reached, giving up on this batch.
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: max retries reached, giving up on this batch.
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: max retries reached, giving up on this batch.
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: max retries reached, giving up on this batch.
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: max retries reached, giving up on this batch.
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
[non-fatal] Tracing: request failed: [Errno 99] Cannot assign requested address
```

```python
import openai
from openai import OpenAI
import os
from dotenv import load_dotenv
load_dotenv(override=True)

from openai import AsyncOpenAI
from agents import OpenAIChatCompletionsModel, Agent, Runner, set_default_openai_client
from agents.model_settings import ModelSettings
from agents import function_tool
import requests, json
import pymysql

from agents import (
    Agent,
    HandoffOutputItem,
    ItemHelpers,
    MessageOutputItem,
    RunContextWrapper,
    Runner,
    ToolCallItem,
    ToolCallOutputItem,
    TResponseInputItem,
    function_tool,
    handoff,
    trace,
)
from agents.extensions.handoff_prompt import RECOMMENDED_PROMPT_PREFIX

from IPython.display import display, Code, Markdown, Image
```

```python
import os
import openai
import glob
import shutil

import numpy as np
import pandas as pd
import pymysql

import json
import io
import inspect
import requests
import re
import random
import string
import base64

from bs4 import BeautifulSoup
import dateutil.parser as parser
import tiktoken
from lxml import etree

import sys
from dotenv import load_dotenv
from openai import OpenAI

import matplotlib
import matplotlib.pyplot as plt
import seaborn as sns
from IPython.display import display, Code, Markdown, Image
from IPython import get_ipython
```

+ DeepSeek API调用流程回顾

```python
# 读取模型API-KEY
API_KEY = os.getenv("API_KEY")

# 实例化客户端
client = OpenAI(api_key=API_KEY, 
                base_url="https://api.deepseek.com")

# 调用 deepseekv3 模型
response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "user", "content": "你好，好久不见!请介绍下你自己。"}
    ]
)

# 输出生成的响应内容
print(response.choices[0].message.content)
```

```text
你好呀！确实好久不见～很高兴再次和你聊天！😊  

我是 **DeepSeek Chat**，由深度求索公司打造的智能AI助手。我的最新版本是 **DeepSeek-V3**，知识更新至 **2024年7月**，拥有 **128K 上下文记忆**，可以处理超长文本，还能阅读并分析 **PDF、Word、Excel、PPT** 等文件。  

✨ **我的特点**：  
- **免费使用**：目前不收费，放心提问！  
- **超强理解力**：能解答技术、学习、生活、娱乐等各种问题。  
- **文件阅读**：上传文档，我可以帮你总结、提取信息或分析内容。  
- **持续进步**：团队在持续优化我，让我更聪明、更贴心！  

你可以问我任何问题，比如最新科技动态、学习建议、写作灵感，或者单纯聊聊天～ 😃 最近有什么我可以帮你的吗？
```

+ Agents SDK基本调用流程回顾

```python
external_client = AsyncOpenAI(
    base_url = "https://api.deepseek.com",
    api_key=API_KEY, 
)

set_default_openai_client(external_client)

deepseek_model = OpenAIChatCompletionsModel(
    model="deepseek-chat",
    openai_client=external_client)
```

```python
agent = Agent(name="Assistant", 
              instructions="你是一名助人为乐的助手。",
              model=deepseek_model)

result = await Runner.run(agent, "请写一首关于编程中递归的俳句。") 

print(result.final_output)
```

```text
函数自调用，  
层层深入栈深处，  
基线终有归。
```

## 一、MCP入门介绍与接入Agents SDK基本流程

在3月27号，Agents SDK正式官宣支持MCP使用，这也使得Agents SDK的实际应用场景得到待拓展：

![](img/8dfa381bd91eae4a11a552920ea03d7a.png)
