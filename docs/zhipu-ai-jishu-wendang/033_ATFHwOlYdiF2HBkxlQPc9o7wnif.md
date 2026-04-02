# 提示词和超参数

> 来源：[https://ai.feishu.cn/docx/ZIMUdT0CwoNJPcxKi6rcVdqgnZc](https://ai.feishu.cn/docx/ZIMUdT0CwoNJPcxKi6rcVdqgnZc)

本文档将带领用户熟悉ChatGLM3-6B的提示词和超参数设置

## ChatGLM3 对话格式

为了避免用户输入的注入攻击，以及统一 Code Interpreter，Tool & Agent 等任务的输入，ChatGLM3 采用了全新的对话格式。

### 整体结构

ChatGLM3 对话的格式由若干对话组成，其中每个对话包含对话头和内容，一个典型的多轮对话结构如下

```
&lt;|system|&gt;
You are ChatGLM3, a large language model trained by Zhipu.AI. Follow the user&#x27;s instructions carefully. Respond using markdown.
&lt;|user|&gt;
Hello
&lt;|assistant|&gt;
Hello, I&#x27;m ChatGLM3. What can I assist you today?
```

### 对话头

对话头占完整的一行，格式为

```
&lt;|role|&gt;{metadata}
```

其中 <|role|> 部分使用 special token 表示，无法从文本形式被 tokenizer 编码以防止注入。metadata 部分采用纯文本表示，为可选内容。

+   <|system|>**：系统信息，设计上可穿插于**对话中，但目前规定仅可以出现在开头

+   <|user|>：用户

+   不会连续出现多个来自 <|user|> 的信息

+   <|assistant|>：AI 助手

+   在出现之前必须有一个来自 <|user|> 的信息

+   <|observation|>：外部的返回结果

+   必须在 <|assistant|> 的信息之后

### 样例场景

为提升可读性，下列样例场景中表示角色的 special token 前均额外添加了一个换行符。实际使用及 tokenizer 实现中均无需额外添加这一换行。

#### 多轮对话

+   有且仅有 <|user|>、<|assistant|>、<|system|> 三种 role

```
&lt;|system|&gt;
You are ChatGLM3, a large language model trained by Zhipu.AI. Follow the user&#x27;s instructions carefully. Respond using markdown.
&lt;|user|&gt;
Hello
&lt;|assistant|&gt;
Hello, I&#x27;m ChatGLM3. What can I assist you today?
```

#### 工具调用

```
&lt;|system|&gt;
Answer the following questions as best as you can. You have access to the following tools:
[
    {
        &quot;name&quot;: &quot;get_current_weather&quot;,
        &quot;description&quot;: &quot;Get the current weather in a given location&quot;,
        &quot;parameters&quot;: {
            &quot;type&quot;: &quot;object&quot;,
            &quot;properties&quot;: {
                &quot;location&quot;: {
                    &quot;type&quot;: &quot;string&quot;,
                    &quot;description&quot;: &quot;The city and state, e.g. San Francisco, CA&quot;,
                },
                &quot;unit&quot;: {&quot;type&quot;: &quot;string&quot;},
            },
            &quot;required&quot;: [&quot;location&quot;],
        },
    }
]
&lt;|user|&gt;
今天北京的天气怎么样？
&lt;|assistant|&gt;
好的，让我们来查看今天的天气
&lt;|assistant|&gt;get_current_weather
tool_call(location=&quot;beijing&quot;, unit=&quot;celsius&quot;)

&lt;|observation|&gt;
{&quot;temperature&quot;: 22}
&lt;|assistant|&gt;
根据查询结果，今天北京的气温为 22 摄氏度。
```

#### 代码执行

+   有 <|user|>、<|assistant|>、<|system|>、<|observation|> 四种 role。其中 <|assistant|> 的 metadata 只有 interpreter。

```
&lt;|system|&gt;
你是一位智能AI助手，你叫ChatGLM3，你连接着一台电脑，但请注意不能联网。在使用Python解决任务时，你可以运行代码并得到结果，如果运行结果有错误，你需要尽可能对代码进行改进。你可以处理用户上传到电脑上的文件，文件默认存储路径是/mnt/data/。
&lt;|user|&gt;
#File: /mnt/data/metadata.jsonl
#Size: 35380
#File uploaded
文件中是否存在缺失值或异常值？
&lt;|assistant|&gt;
首先，我将读取您提供的文件，并查看其内容以确定是否存在缺失值或异常值。
我们从加载文件开始：
&lt;|assistant|&gt;interpreter
import json

# Load the uploaded file
data = []
with open(&quot;/mnt/data/metadata.jsonl&quot;, &#x27;r&#x27;) as file:
    for line in file:
        data.append(json.loads(line))

# Display the first few records to get a sense of the data
data[:5]

&lt;|observation|&gt;
[{&#x27;file_name&#x27;: &#x27;aceinthehole.png&#x27;,
  &#x27;name&#x27;: &#x27;Ace in the Hole&#x27;,
  &#x27;type&#x27;: &#x27;survivor&#x27;,
  &#x27;description&#x27;: &#x27;Lady Luck always seems to be throwing something good your way.&#x27;},
 {&#x27;file_name&#x27;: &#x27;adrenaline.png&#x27;,
  &#x27;name&#x27;: &#x27;Adrenaline&#x27;,
  &#x27;type&#x27;: &#x27;survivor&#x27;,
  &#x27;description&#x27;: &#x27;You are fuelled by unexpected energy when on the verge of escape.&#x27;},
 {&#x27;file_name&#x27;: &#x27;aftercare.png&#x27;,
  &#x27;name&#x27;: &#x27;Aftercare&#x27;,
  &#x27;type&#x27;: &#x27;survivor&#x27;,
  &#x27;description&#x27;: &quot;Unlocks potential in one&#x27;s aura reading ability.&quot;},
 {&#x27;file_name&#x27;: &#x27;agitation.png&#x27;,
  &#x27;name&#x27;: &#x27;Agitation&#x27;,
  &#x27;type&#x27;: &#x27;killer&#x27;,
  &#x27;description&#x27;: &#x27;You get excited in anticipation of hooking your prey.&#x27;},
 {&#x27;file_name&#x27;: &#x27;alert.png&#x27;,
  &#x27;name&#x27;: &#x27;Alert&#x27;,
  &#x27;type&#x27;: &#x27;survivor&#x27;,
  &#x27;description&#x27;: &#x27;Your acute senses are on high alert.&#x27;}]

&lt;|assistant|&gt;
该文件看起来包含有关某些条目的元数据，每个条目有以下字段：
file_name: 文件名称
name: 名称
type: 类型（例如 &quot;survivor&quot; 或 &quot;killer&quot;）
description: 描述
接下来，我将检查这些字段是否存在缺失值。
&lt;|assistant|&gt;interpreter
# Check for missing values in each column
missing_values = {key: sum(1 for item in data if key not in item) for key in data[0].keys()}

missing_values

&lt;|observation|&gt;
{&#x27;file_name&#x27;: 0, &#x27;name&#x27;: 0, &#x27;type&#x27;: 0, &#x27;description&#x27;: 0}

&lt;|assistant|&gt;
每个字段都没有缺失值。这意味着每个条目都完整。
接下来，我们可以检查是否存在任何潜在的异常值。这通常需要更深入的分析和对数据的了解，但我可以从一些基本的统计和分布开始进行检查。为了这样做，我们可以查看“type”字段的值分布，看看是否有任何意外的类别。
&lt;|assistant|&gt;interpreter
# Check the distribution of values in the &#x27;type&#x27; column
type_distribution = {key: sum(1 for item in data if item[&#x27;type&#x27;] == key) for key in set(item[&#x27;type&#x27;] for item in data)}

type_distribution

&lt;|observation|&gt;
{&#x27;survivor&#x27;: 116, &#x27;killer&#x27;: 103}

&lt;|assistant|&gt;
“type”字段有两个值：“survivor”和“killer”。其中，“survivor”有116个条目，而“killer”有103个条目。
从这个初步的检查中，我们没有发现任何明显的异常值。
如果您有任何特定的标准或其他字段需要检查，我可以为您提供更详细的分析。您还希望我检查什么呢？
&lt;|user|&gt;
帮我画一个爱心
&lt;|assistant|&gt;interpreter
import numpy as np
import matplotlib.pyplot as plt

# 爱心形状的参数方程
def heart(t):
    x = 16 * np.sin(t) ** 3
    y = 13 * np.cos(t) - 5 * np.cos(2 * t) - 2 * np.cos(3 * t) - np.cos(4 * t)
    return x, y

t = np.linspace(0, 2 * np.pi, 1000)
x, y = heart(t)

plt.figure(figsize=(6, 6))
plt.plot(x, y, color=&#x27;red&#x27;)
plt.axis(&#x27;equal&#x27;)
plt.axis(&#x27;off&#x27;)
plt.show()
```

## 超参数设置

ChatGLM3-6B共有以下参数可以设置

+   max_length: 模型的总token限制，包括输入和输出的tokens

+   temperature: 模型的温度。温度只是调整单词的概率分布。其最终的宏观效果是，在较低的温度下，我们的模型更具确定性，而在较高的温度下，则不那么确定。

+   top_p: 模型采样策略参数。在每一步只从累积概率超过某个阈值 p 的最小单词集合中进行随机采样，而不考虑其他低概率的词。只关注概率分布的核心部分，忽略了尾部部分。

对于以下场景，我们推荐使用这样的参数进行设置

| Use Case | temperature | top_p | 任务描述 |
| --- | --- | --- | --- |
| 代码生成 | 0.2 | 0.1 | 生成符合既定模式和惯例的代码。 输出更确定、更集中。有助于生成语法正确的代码 |
| 创意写作 | 0.7 | 0.8 | 生成具有创造性和多样性的文本，用于讲故事。输出更具探索性，受模式限制较少。 |
| 聊天机器人回复 | 0.5 | 0.5 | 生成兼顾一致性和多样性的对话回复。输出更自然、更吸引人。 |
| 调用工具并根据工具的内容回复 | 0.0 | 0.7 | 根据提供的内容，简洁回复用户的问题。 |
| 代码注释生成 | 0.1 | 0.2 | 生成的代码注释更简洁、更相关。输出更具有确定性，更符合惯例。 |
| 数据分析脚本 | 0.2 | 0.1 | 生成的数据分析脚本更有可能正确、高效。输出更确定，重点更突出。 |
| 探索性代码编写 | 0.6 | 0.7 | 生成的代码可探索其他解决方案和创造性方法。输出较少受到既定模式的限制。 |