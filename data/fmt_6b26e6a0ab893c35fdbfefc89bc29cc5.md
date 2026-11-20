
# Qwen-3本地部署与调用详解（下）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Ed1ldsK4qo2nV0xLMdwcgm8InYb](https://kq4b3vgg5b.feishu.cn/docx/Ed1ldsK4qo2nV0xLMdwcgm8InYb)

## 课程说明

+ 体验课内容节选自《2025大模型Agent智能体开发实战》(5月班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(5月班)

![](img/4386a15f9e45c90dfb9fd9e5445af2b2.png)

![](img/5f76fd3fba2d519d3eaa5b752687593e.png)

此外，公开课全套学习资料，已上传至网盘

（https://pan.baidu.com/s/1wtLsh6h3RhTfCBFgWa5BYQ 提取码: r8rq ）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## 部分项目成果演示

```
from IPython.display import Video
```

+ `MateGen` 项目演示

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/4.MateGen%20Pro%20%E9%A1%B9%E7%9B%AE%E5%8A%9F%E8%83%BD%E6%BC%94%E7%A4%BA.mp4", width=800, height=400)
```

Your browser does not support the video element.

+ `智能客服` 项目演示

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/%E6%99%BA%E8%83%BD%E5%AE%A2%E6%9C%8D%E6%A1%88%E4%BE%8B%E6%BC%94%E7%A4%BA.mp4", width=800, height=400)
```

Your browser does not support the video element.

+ `Dify` 项目演示

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/2f1b47f42c65fd59e8d3a83e6cb9f13b_raw.mp4", width=800, height=400)
```

Your browser does not support the video element.

+ `LangChain&LangGraph` 搭建 `Multi-Agnet`

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/%E5%8F%AF%E8%A7%86%E5%8C%96%E6%95%B0%E6%8D%AE%E5%88%86%E6%9E%90Multi-Agent%E6%95%88%E6%9E%9C%E6%BC%94%E7%A4%BA%E6%95%88%E6%9E%9C.mp4", width=800, height=400)
```

Your browser does not support the video element.

---

## 3. ollama API本地调用Qwen3

在部署完`ollama`之后，即可借助`ollama API`（也就是`OpenAI`风格`API`）在代码环境中调用模型。

+ 导入`OpenAI`库及实例化`OpenAI`客户端

```
from openai import OpenAI

client = OpenAI(
    base_url='http://192.168.110.131:11434/v1/',
    api_key='ollama',  # 必须传递该参数
)

prompt = "在单词\"strawberry\"中，总共有几个R？"
messages = [
    {"role": "user", "content": prompt}
]
```

+ 创建消息并获得回复

```
response = client.chat.completions.create(
    messages=messages,
    model='qwen3:14b',
)

print(response.choices[0].message.content)
```

```
&lt;think&gt;
好的，我现在要解决的问题是：在单词“strawberry”中，总共有几个R？让我仔细想一想，确保自己不会犯错。

首先，我需要明确目标。用户问的是“strawberry”这个单词中有多少个字母R。所以我的任务就是把这个单词正确地拆解出来，然后逐一检查每个字母是否是R，然后统计数量。

接下来，我应该先写出“strawberry”这个单词的拼写，确保自己正确无误。可能有时候拼写会有错误，特别是如果不太熟悉某些单词的话。不过，我记得“strawberry”是正确的拼写，由字母s、t、r、a、w、b、e、r、r、y组成。不过，我需要再确认一遍每个字母的位置，避免出错。

让我把每个字母逐个列出来：

1. s
2. t
3. r
4. a
5. w
6. b
7. e
8. r
9. r
10. y

现在，我需要检查这些字母中哪些是R。看一下每个位置：

- 第三个字母是r，正确。
- 第八个字母是r。
- 第九个字母也是r。
- 其他位置的字母都不是r。

所以看起来，这里总共有三个r。不过，等等，我是不是哪里数错了？让我再重新数一遍，确保每个位置都正确。

再逐个检查：

s → 不是

t → 不是

r → 是，第一个r（1个）

a → 不是

w → 不是

b → 不是

e → 不是

r → 是，第二个r（2个）

r → 是，第三个r（3个）

y → 不是

所以确实，有三个r。不过，我是不是哪里拼写错误？比如，是否有可能在“strawberry”中有其他位置的r？比如，可能有人会拼错，比如写成“strawberri”或者其他形式？但根据标准拼写，“strawberry”是正确的，所以应该没错。

不过，可能有一些人会认为在“strawberry”中拼写是s-t-r-a-w-b-e-r-r-y，也就是三个r。是的，没错。那是不是正确的？比如常见错误可能是否多了一个r？比如，有人可能会写成s-t-r-r-a-w-b-e-r-r-y？但那样的话，字母就会重复，不过实际正确的拼写中，确实是在“berry”这个部分有三个r吗？

等等，这里可能需要更仔细的检查。因为“strawberry”是由“straw”和“berry”组成。而“berry”这个词通常有三个r吗？比如，常见的是“berry”是b-e-r-r-y，对吗？这样就是两个r。那如果是这样的话，整个单词中的r数目应该是两个吗？

哦，这里可能出问题了。我之前拆分的时候，可能把“berry”部分的r数错了。让我再重新拆分整个单词。

“strawberry”正确的拼写是：s-t-r-a-w-b-e-r-r-y？还是s-t-r-a-w-b-e-r-y？

这时候，我需要确认“strawberry”是否正确拼写为有三个r还是两个r。因为“berry”通常有两个r，对吗？比如“berry”是b-e-r-r-y，所以有两个r。那加上“straw”中的一个r（在s-t-r-a-w），所以总共有三个r？或者不是这样？

例如，如果整个单词是s-t-r-a-w-b-e-r-r-y，那么就是三个r？或者是不是s-t-r-a-w-b-e-r-y，只有两个r？

这时候，我需要确认正确的英文拼写。这时候，我应该回忆或者查找正确的拼写。不过，可能有时候在记忆中会混淆，所以需要确认。

举个例子，比如“strawberry”这个单词的正确拼写。例如，“strawberry”在词典中的拼写是s-t-r-a-w-b-e-r-r-y，对吗？也就是说，中间的部分是s-t-r-a-w-b-e-r-r-y，所以确实有三个r？或者不是b-e-r-r-y，也就是两个r？

此时，我可能需要更仔细地分析。比如，分解单词：

strawberry 是由 straw 和 berry 组成，对吧？而“berry”这个词本身有三个r吗？不，通常“berry”是两个r，比如 blackberry（黑莓）是b-l-a-c-k-b-e-r-r-y，对吗？所以“berry”部分有三个r吗？还是两个？

不，“blackberry”是b-l-a-c-k-b-e-r-r-y，所以这里的“berry”部分是b-e-r-r-y，对吗？所以这里的“berry”有三个r？或者不？不对，b-e-r-r-y，其中r出现了两次，所以是两个r。

那如果是这样的话，在“strawberry”中，应该是straw + berry，对吗？

那如果是这样的话，“straw”部分是s-t-r-a-w，有一个r，而“berry”是b-e-r-r-y，有两个r，所以总共有1+2=3个r？

是的，这样的话，整个单词“strawberry”中的r数目是三个？

但这样的话，我之前拆解的时候是正确的，总共有三个r。但是，我之前可能混淆了“berry”中的r数目？

或者，可能正确的拼写是“strawberry”中有两个r？

这时候，我需要再确认一次，可能自己之前对“berry”的拼写有误。

例如，查字典的话，strawberry的拼写是s-t-r-a-w-b-e-r-r-y，确实是三个r。对吗？或者是不是这样？

让我再想，比如，如果我拼写出“strawberry”这个单词，那么其中的“berry”部分有两个r还是三个？

比如，如果我写“strawberry”，正确的拼写应该是s-t-r-a-w-b-e-r-r-y，对吗？所以这里有三个r？

那这样的话，答案应该是3个r。但是，我之前可能误以为“berry”有两个r，所以可能混淆了？

这时候，我需要再仔细分析一下，或者用其他方式来确认。

比如，分解每个字母：

s t r a w b e r r y

现在，数一下每个字母：

s (1), t (2), r (3), a (4), w (5), b (6), e (7), r (8), r (9), y (10)

所以，r出现在第3、8、9位，共三个，对吗？

是的，三个r。所以正确答案是3个r。

不过，我之前可能担心是否自己对“strawberry”的拼写有误，所以需要再次确认。

或者，是否存在另一种拼写？

比如，是否有可能是s-t-r-a-w-b-e-r-y，即只有两个r？

那这样的话，答案就是两个r？

这个时候，我需要确认正确的拼写。

根据我的知识，“strawberry”的正确拼写是s-t-r-a-w-b-e-r-r-y，即三个r。因为“berry”部分是b-e-r-r-y，对吗？而“straw”部分是s-t-r-a-w，所以有一个r，加上“berry”中的两个r，总共有三个r。

因此，正确的答案应该是三个r。

不过，可能有些学习者会错误地认为“berry”只有两个r，但实际上“berry”是三个r？不，不，实际上“berry”是两个r。因为“berry”是b-e-r-r-y，所以是两个r。

所以，如果这样的话，整个单词“strawberry”中的r数目应该是1（来自straw） + 2（来自berry） = 3个r。对吗？是的。

因此，正确的答案应该是三个。

但可能有些人会错误地在拼写时重复r两次，导致错误地认为有三个r？或者是否正确拼写？

这个时候，我需要查阅一下或者回忆正确的拼写。

例如，我可以通过联想常见的拼写错误。比如，是否有人容易在“strawberry”中多写一个r？比如，正确拼写是s-t-r-a-w-b-e-r-r-y，对吗？

或者，是否应该是s-t-r-a-w-b-e-r-y？即只一个r？

这个时候，我可能需要再用另一种方法验证，比如分解单词的来源。

“Strawberry”这个名字来源于“straw”（干草）和“berry”（浆果），所以可能拼写是straw + berry。

而“berry”本身是b-e-r-r-y，对吗？比如，像“blackberry”是b-l-a-c-k-b-e-r-r-y，所以这里的“berry”部分有两个r。所以“strawberry”应该也是这样，即在“berry”中有两个r。

所以，整个单词中的三个r来自于straw中的r（一个）和berry中的r（两个）：1+2=3个r。

因此，结论是三个r。

不过，这时候，我需要确保自己不会犯拼写错误。比如，可能有些人会把“strawberry”拼成两个r，即s-t-r-a-w-b-e-r-y，但那是错误的，正确的是三个r吗？

或者，其实正确的拼写是s-t-r-a-w-b-e-r-r-y，对吗？

我需要再次确认。比如，查看英式或美式拼写是否有不同。但通常来说，“strawberry”的拼写是相同的，不管英式还是美式。

例如，根据牛津词典的拼写：strawberry 是 s-t-r-a-w-b-e-r-r-y，对吗？

是的，这样就是三个r。所以，答案是3个。

所以，我的思考过程确认了这个单词中有三个r。

不过，在最开始的时候，我可能误以为“berry”中有两个r，而“straw”中有一个，所以总共有三个，正确。但可能在思考过程中我有过短暂的混淆，但最终通过仔细分析和确认，得出了正确的答案。

总结一下：

正确拼写为 strawberry → s-t-r-a-w-b-e-r-r-y，其中有三个r，分别在位置3、8、9。因此，答案是3个r。
&lt;/think&gt;

在单词“strawberry”中，字母R的分布如下：

1. **s**  
2. **t**  
3. **r**（第一个R）  
4. **a**  
5. **w**  
6. **b**  
7. **e**  
8. **r**（第二个R）  
9. **r**（第三个R）  
10. **y**  

通过逐个检查每个字母的位置，可以确认“strawberry”中共有 **3个R**（分别位于第3、8和9位）。因此，答案是：

**3**
```

因为`Qwen3`刚刚发布，所以`Ollama`目前还不支持在`API`调用时通过`enable_thinking`参数禁用思考过程，这里有一种策略是先用`/no_think`提示不要进入思考过程。如下所示：

```
prompt = "/no_think 在单词\"strawberry\"中，总共有几个R？"
messages = [
    {"role": "user", "content": prompt}
]


response = client.chat.completions.create(
    messages=messages,
    model='qwen3:14b',
)

print(response.choices[0].message.content)
```

```
&lt;think&gt;

&lt;/think&gt;

我们来一步一步分析单词 **"strawberry"** 中的字母 **R** 的数量。

---

### 第一步：写出单词
**strawberry**

---

### 第二步：逐个字母查看
我们将单词拆分为单个字母：

**s - t - r - a - w - b - e - r - r - y**

---

### 第三步：数一数字母 **R** 出现的次数
从上面的字母序列中，我们可以看到 **R** 出现了 **3** 次。

---

### 最终答案：
**"strawberry"** 中有 **3** 个 **R**。
```

而此时显存占用约22G左右（14B Q4量化模型）：

![](img/ccf0cf524d38f143460b1d100875b92e.png)

## 四、Qwen3模型接入vLLM与推理流程

接下来继续介绍`Qwen3-14B`模型借助`vLLM`进行推理的完整流程。相比`ollama`，`vLLM`更加适合企业级高并发应用场景，但对应的，显存占用也会更高，`vLLM`项目主页：https://github.com/vllm-project/vllm`Vllm`底层是基于`Pytorch`构建，其`Github`开源地址为：https://github.com/vllm-project/vllm

![](img/175c4a9560c287eba145dfc0ec7fd4ce.png)

从各种基准测试数据来看，同等配置下，使用`vLLM`框架与`Transformer`等传统推理库相比，其吞吐量可以提高一个数量级，这归功于以下几个特性：

+ **高级GPU优化**：利用`CUDA`和`PyTorch`最大限度地提高`GPU`利用率，从而实现更快的推理速度。`Ollama`其实是对`CPU-GPU`的混合应用，但`vllm`是针对纯`GPU`的优化。

+ **高级内存管理**：通过`PagedAttention`算法实现对`KV cache`的高效管理，减少内存浪费，从而优化大模型的运行效率。

+ **批处理功能**：支持连续批处理和异步处理，从而提高多个并发请求的吞吐量。

+ **安全特性**：内置`API`密钥支持和适当的请求验证，不像其他完全跳过身份验证的框架。

+ **易用性**：`vLLM`与`HuggingFace`模型无缝集成，支持多种流行的大型语言模型，并兼容`OpenAI`的`API`服务器。

### 4.1 vLLM安装与启动

使用`vllm`框架部署`Qwen3`模型，同样需要先安装`Python`运行环境，这里我们可以复用之前安装的`Anaconda`环境，具体执行命令如下：

```
    conda activate qwen3
```

注意：`vllm`官方要求的是`Python 3.9` ~ `Python 3.12`之间的版本，所以如果`Anaconda`环境版本不在此范围内，请务必重新创建符合要求的`Anaconda`环境。
