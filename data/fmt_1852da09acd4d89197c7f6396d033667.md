
# LIama3+Groq 最强组合，800 tokens/s！推理速度真的太快了！

> 来源：[https://agispace.feishu.cn/docx/G3HKdkMAqoWKWtxuvnQcfpB4nXe](https://agispace.feishu.cn/docx/G3HKdkMAqoWKWtxuvnQcfpB4nXe)

> 作者： 是莫尔索

> 👸号： 莫尔索随笔

> 原文链接：https://mp.weixin.qq.com/s/NYHT9etA8qlK4R7kOtBe3Q

北京时间 4 月 19 日凌晨，Meta 在官网上官宣了 Llama3，作为继 Llama1、Llama2 和 CodeLlama 之后的第三代模型，Llama3 在多个基准测试中实现了全面领先，性能优于业界同类最先进的模型，你有没有第一时间体验上呢，这篇文章就分享下如何在 Groq 上以超过 800 tokens/s 的恐怖推理速度体验 Llama3，会同时分享 Web 端、移动端、API 方式以及集成到 LangChain 中 4 种体验方案。

[欢迎加入我的会员计划](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247484894&idx=1&sn=4379fe3a2c67f3ac808f4913cb2a2c90&scene=21#wechat_redirect)，获取 AI 落地方面的优质信息和见解，节省你的时间。

莫尔索随笔

跟踪LLM(大语言模型)最新动态，分享第一手LLM 实践经验，致力于打造个人LLM 应用开发全栈指南！

62篇原创内容

👸号

## Groq 有多快

先看两组数据

![](img/de19db326cd4de05045683a1ad4389e7.png)

Llama3 8B不同平台的推理速度

![](img/329fe66f634aa86e01cbbc817361ebcf.png)

Llama3 70B不同平台的推理速度

Llama3 8B 每秒钟 876 tokens 的输出速度，人眼基本跟不上模型的输出速度了，要知道 Llama3 8B 的质量与 GPT-3.5 和 Llama2 70B 相似，可以显著提升一些常见的 AI 应用场景的用户体验；RAG 的性能瓶颈不再是 LLM，而是 Retrieval，什么 HyDE（假设文档嵌入，利用 LLMs 生成假设性答案，以增强文档检索的准确性）、LLM 重排序器（对检索到的文档进行重排序，以优先选择最相关和上下文适当的信息）不再是 RAG 链路速度瓶颈...

初看数据，我以为是个噱头，本着务实的态度，我自己实际体验了一把，大家自己看 👇

Llama3 8B实测

![](img/79796572e0dbfb23724e014615638175.png)

Llama3 70B实测

结果 Llama3 70B 的实际体验结果比测评数据还高。

## Groq 为什么这么快

源于 Groq 开发出了一种新的 AI 处理器 ——LPU（Language Processing Unit），其推理速度相较于英伟达 GPU 提高了 10 倍。

1. 专业优化：LPU 专门针对语言模型推理任务进行了优化，特别是在序列处理方面。

2. 创新架构：LPU 采用了一种新的计算模式，能够高效地按顺序处理任务，而不是并行处理。

3. 软件先行：Groq 在硬件开发前就创新了软件和编译器，以确保芯片间的高效通信。

4. 内存与处理单元的整合：LPU 的设计使得数据流局部性得到更好的利用。

5. 针对性能和成本的优化：LPU 在设计时就注重了性能提升和成本降低。

详细科普请前往[这里查看](https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650909778&idx=4&sn=6e6d183c997878b640b88a0a2dd9bcd5&scene=21#wechat_redirect)

## 4 种 Groq 体验方案

### 1. Web 端

无需登录，即可直接在网页版进行尝试，地址指路 👉`groq.com`([1])，当前支持的模型有 Llama3 8B-4k 、 Llama3 70B-8k 、 Llama2 70B-8k 、 Mixtral 8X7B-32k 、 Gemma 7B-it

### 2. 移动端

Gorq 的 iOS 应用已经推出，目前不需要登录即可使用，通过下面的 TestFlight 安装地址：`testflight.apple.com`([2])，支持的模型有 Llama3 8B 、 Llama3 70B 、 Llama2 70B 、 Mixtral 8X7B 、 Gemma 7B

![](img/001ef2cb4938d0f4ba105e5bc8fd3b39.png)

Gorq iOS 应用

### 3. API 调用

先前往这个地址 `https://console.groq.com/keys` 申请好 `API-KEY`。

![](img/14e12f3a901dcfccdcb13b298902901f.png)

Groq API-Key申请

安装依赖库

`pip install groq`

调用

```
import os
from groq import Groq

client = Groq(
    api_key=os.environ.get("GROQ_API_KEY"),
)
llm = client.chat.completions.create(
    messages=[
        {
            "role": "user",
            "content": "编写一篇中国神话故事，篇幅500～800字，必须使用中文输出",
        }
    ],
    model="llama3-70b-8192",
)

print(llm.choices[0].message.content)
```

### 4. LangChain 中使用

安装依赖库

`pip install langchain-groq`

使用

```
from langchain_core.prompts import ChatPromptTemplate
from langchain_groq import ChatGroq

llm = ChatGroq(temperature=0, model_name="llama3-70b-8192")
human = "{text}"
prompt = ChatPromptTemplate.from_messages([("human", human)])

chain = prompt | llm

response = chain.invoke({"text": "编写一篇中国神话故事，篇幅500～800字，必须使用中文输出"})
print(response.content)
```

## 更多体验方式

如果由于网络原因你还是无法访问，请使用我在这篇文章推荐的5 个免费稳定的 Llama 3 在线体验地址([3])

莫尔索随笔

跟踪LLM(大语言模型)最新动态，分享第一手LLM 实践经验，致力于打造个人LLM 应用开发全栈指南！

62篇原创内容

👸号

不可错过 👉：我创建了【AI 产品】和 【AI 开发】交流群，用于 AI 产品构建和分享交流，如果你负责的工作和这个领域强相关并且感兴趣，欢迎后台点击【一起交流】 ，备注信息务必做简单自我介绍，否则不予通过！如果觉得内容不错，欢迎点个关注，分享和在看～

## 参考资料

[1]

groq.com: https://groq.com/

[2]

testflight.apple.com: https://testflight.apple.com/join/Y9X0wGsi

[3]

5 个免费稳定的 Llama 3 在线体验地址: https://liduos.com/the-memeber-newsletter-02.html
