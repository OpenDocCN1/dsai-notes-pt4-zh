# 快速使用

> 来源：[https://ai.feishu.cn/docx/S1stdQPoioyUO2xJnZAcIxq4nLc](https://ai.feishu.cn/docx/S1stdQPoioyUO2xJnZAcIxq4nLc)

本章节将带领开发者如何快速上手智谱AI 最新的 Python SDK

## 使用Python 进行 SDK 调用

## 安装SDK

```
pip install zhipuai&gt;=2.0.1
```

## 非流式输出调用

现在，我们只要用如下代码进行演示，就可以获得完整的回答

```
from zhipuai import ZhipuAI

client = ZhipuAI(api_key=&quot;your api key&quot;)
response = client.chat.completions.create(
    model=&quot;glm-4&quot;,
    messages=[
        {
            &quot;role&quot;: &quot;user&quot;,
            &quot;content&quot;: &quot;请你作为童话故事大王，写一篇短篇童话故事，故事的主题是要永远保持一颗善良的心，要能够激发儿童的学习兴趣和想象力，同时也能够帮助儿童更好地理解和接受故事中所蕴含的道理和价值观。&quot;
        }
    ],
    top_p=0.7,
    temperature=0.9,
    stream=False,
    max_tokens=2000,
)
print(response.choices[0].message.content)
```

## 流式输出调用

当然，我们的python sdk进行简单的流式输出调用，以下是简单的代码

```
from zhipuai import ZhipuAI
import sys
client = ZhipuAI(api_key=&quot;your api key&quot;)
response = client.chat.completions.create(
    model=&quot;glm-4&quot;,
    messages=[
        {
            &quot;role&quot;: &quot;user&quot;,
            &quot;content&quot;: &quot;请你作为童话故事大王，写一篇短篇童话故事，故事的主题是要永远保持一颗善良的心，要能够激发儿童的学习兴趣和想象力，同时也能够帮助儿童更好地理解和接受故事中所蕴含的道理和价值观。&quot;
        }
    ],
    top_p=0.7,
    temperature=0.9,
    stream=True,
    max_tokens=2000,
)

if response:
    for chunk in response:
        #每次输出都会在同一行的末尾添加，并且在输出后立即刷新缓冲区，从而立即看到结果。
        sys.stdout.write(chunk.choices[0].delta.content) 
        sys.stdout.flush()
    print()
```

现在，你已经初步学会使用智谱AI V4版本了，开始在文档进行尽情的探索吧。