
# DeepSeek v3 API接入指南

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/UsF2dzGWHoJ6JXxDSUOcVIxGnjf](https://kq4b3vgg5b.feishu.cn/docx/UsF2dzGWHoJ6JXxDSUOcVIxGnjf)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》完整版付费课程。

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》。

![](img/79c1c7e7d314005a8d0443a5cab67fdd.png)

![](img/7bc5d664e5d3b7b73fb6ead87594d096.png)

此外，ds公开课训练项目代码、数据及训练完的模型已上传至课件网盘，更体系深入学习大模型，扫码联系助教，即可咨询正课详情。

### 《2025大模型Agent智能体开发实战》体验课

### Ch 1. DeepSeek v3 API接入指南

```
import os
from IPython.display import display, Code, Markdown
import requests
import json
```

#### DeepSeek v3账号注册与API获取

DeepSeek官网：https://www.deepseek.com/

![](img/e59241c3844f788876743d6aa47e9e2b.png)

![](img/77288ff5ffe2e713c9d0e0d640029cd5.png)

新用户注册即赠送10元额度，约500万token额度：

![](img/a5db4fddd3ba5d7be8935b3b647497d8.png)

对比GPT4o价格，约降低90%以上：输入价格为GPT4o的6%，输出价格围殴GPT4o的3%

![](img/b928a68bf60b06bd901a14a907b9755a.png)

且API调用不限速：

![](img/73dee7b8ad6c2e16e55a36ed555c7b07.png)

最关键的是，调用风格和OpenAI完全一致：Function calling、提示词缓存、Json Output等功能完全相同：

![](img/9bb5ab73ca093a84f32a5bf27521de6e.png)

![](img/f2a4ad2a97d5b57d80f49e151bff1a54.png)

#### DeepSeek v3调用流程

对比OpenAI GPT4o调用流程：

```
!pip install openai
```

Requirement already satisfied: openai in /root/anaconda3/lib/python3.12/site-packages (1.57.2)
Requirement already satisfied: anyio<5,>=3.5.0 in /root/anaconda3/lib/python3.12/site-packages (from openai) (4.2.0)
Requirement already satisfied: distro<2,>=1.7.0 in /root/anaconda3/lib/python3.12/site-packages (from openai) (1.9.0)
Requirement already satisfied: httpx<1,>=0.23.0 in /root/anaconda3/lib/python3.12/site-packages (from openai) (0.28.1)
Requirement already satisfied: jiter<1,>=0.4.0 in /root/anaconda3/lib/python3.12/site-packages (from openai) (0.8.2)
Requirement already satisfied: pydantic<3,>=1.9.0 in /root/anaconda3/lib/python3.12/site-packages (from openai) (2.5.3)
Requirement already satisfied: sniffio in /root/anaconda3/lib/python3.12/site-packages (from openai) (1.3.0)
Requirement already satisfied: tqdm>4 in /root/anaconda3/lib/python3.12/site-packages (from openai) (4.66.4)
Requirement already satisfied: typing-extensions<5,>=4.11 in /root/anaconda3/lib/python3.12/site-packages (from openai) (4.11.0)
Requirement already satisfied: idna>=2.8 in /root/anaconda3/lib/python3.12/site-packages (from anyio<5,>=3.5.0->openai) (3.7)
Requirement already satisfied: certifi in /root/anaconda3/lib/python3.12/site-packages (from httpx<1,>=0.23.0->openai) (2024.6.2)
Requirement already satisfied: httpcore==1.* in /root/anaconda3/lib/python3.12/site-packages (from httpx<1,>=0.23.0->openai) (1.0.7)
Requirement already satisfied: h11<0.15,>=0.13 in /root/anaconda3/lib/python3.12/site-packages (from httpcore==1.*->httpx<1,>=0.23.0->openai) (0.14.0)
Requirement already satisfied: annotated-types>=0.4.0 in /root/anaconda3/lib/python3.12/site-packages (from pydantic<3,>=1.9.0->openai) (0.6.0)
Requirement already satisfied: pydantic-core==2.14.6 in /root/anaconda3/lib/python3.12/site-packages (from pydantic<3,>=1.9.0->openai) (2.14.6)
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
```

```
import openai
```

```
openai.__version__
```

```
'1.57.2'
```

```
from openai import OpenAI
```

```
api_key = 'YOUR_API_KEY'
```

```
# 实例化客户端
client = OpenAI(api_key=api_key, 
                base_url="https://ai.devtool.tech/proxy/v1")
```

```
# 调用 GPT-4o-mini 模型
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "user", "content": "你好，好久不见!"}
    ]
)
```

上述代码中 `model` 参数指定了使用的模型（`gpt-4o-mini`），`messages` 列表定义了对话内容，其中 `role` 为 `user` 表示用户的输入内容。

```
# 输出生成的响应内容
print(response.choices[0].message.content)
```

```
你好！很高兴再次见到你！你最近怎么样？
```

```
response
```

```
ChatCompletion(id='chatcmpl-AmyljFoOE3EPBcKgrc2MxlUTs4Nfe', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='你好！很高兴再次见到你！你最近怎么样？', refusal=None, role='assistant', audio=None, function_call=None, tool_calls=None))], created=1736236703, model='gpt-4o-mini-2024-07-18', object='chat.completion', service_tier=None, system_fingerprint='fp_d02d531b47', usage=CompletionUsage(completion_tokens=15, prompt_tokens=13, total_tokens=28, completion_tokens_details=CompletionTokensDetails(accepted_prediction_tokens=0, audio_tokens=0, reasoning_tokens=0, rejected_prediction_tokens=0), prompt_tokens_details=PromptTokensDetails(audio_tokens=0, cached_tokens=0)))
```

DeepSeekv3调用流程:

首先在官网申请API-KEY：https://platform.deepseek.com/api_keys

![](img/7cac3f5a2c58d0a11d8b9c82ad7257ba.png)

![](img/728a6431abec732b7d049c6eca9ce072.png)

```
ds_api_key = 'YOUR_DS_API_KEY'
```

```
# 实例化客户端
client = OpenAI(api_key=ds_api_key, 
                base_url="https://api.deepseek.com")
```

```
# 调用 deepseekv3 模型
response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "user", "content": "你好，好久不见!"}
    ]
)
```

```
# 输出生成的响应内容
print(response.choices[0].message.content)
```

```
你好！好久不见！最近过得怎么样？有什么想聊的吗？
```

```
response
```

```
ChatCompletion(id='1fdbc70b-acbf-4c9a-a895-d26bd69ad531', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='你好！好久不见！最近过得怎么样？有什么想聊的吗？', refusal=None, role='assistant', audio=None, function_call=None, tool_calls=None))], created=1736238514, model='deepseek-chat', object='chat.completion', service_tier=None, system_fingerprint='fp_3a5770e1b4', usage=CompletionUsage(completion_tokens=14, prompt_tokens=8, total_tokens=22, completion_tokens_details=None, prompt_tokens_details=None, prompt_cache_hit_tokens=0, prompt_cache_miss_tokens=8))
```

一样的SDK，代表DeepSeek模型采用和GPT模型完全的参数体系：

```
client.chat.completions.create?
```
