
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



![](img/ad9a9dfa59d35086fbe7b6047b706c0d.png)

然后，安装`vLLM`框架，具体执行命令如下：

```
pip install vllm
```

![](img/ca3ae9520f1f55ca3cbca943703a0a1b.png)

此时耐心等待安装完成即可。待安装完成后，可以使用`pip show vllm`命令查看`vllm`框架的安装信息，可以明确看到当前安装的`vllm`版本号。如下图所示：

![](img/e6a1371df22462488dc448e466e2a024.png)

目前`vLLM`已支持`Qwen3`模型调用，可以在模型支持列表中查看模型关键字：https://docs.vllm.ai/en/latest/models/supported_models.html

![](img/71e2035141457a3cc87b394eccda2bbe.png)

接下来即可按照如下流程进行调用。

## 4.2 OpenAI风格API响应模式

需要注意的是，随着模型上下文越长，所需要占用的显存也越大。根据测试，14B模型在32K上下文时，运行需要约30G显存。实际运行命令类似如下形式：

```
vllm serve Qwen3-8B --max-model-len 32768 --host 192.168.110.131 --port 8000
```

![](img/90c2d9c0faa46eef1d68afc7b7aba6ad.png)

启动`vLLM`时候需要谨慎的设置最大上下文和对应的运行GPU数量。

+   单GPU运行命令

```
cd /root/autodl-tmp
vllm serve ./qwen3-8B --max-model-len 32768
```

+   双GPU运行命令

```
cd /root/autodl-tmp
CUDA_VISIBLE_DEVICES=0,1 vllm serve ./qwen3-8B --tensor-parallel-size 2
```

启动成功后，即可在`Jupyter`中进行调用。

+   导入`OpenAI`库并实例化`OpenAI`客户端

```
from openai import OpenAI

openai_api_key = "EMPTY"
openai_api_base = "http://192.168.110.131:8000/v1"

client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)
```

+   创建消息获得回复

```
prompt = "在单词\"strawberry\"中，总共有几个R？"
messages = [
    {"role": "user", "content": prompt}
]
```

```
response = client.chat.completions.create(
    model="Qwen3:8B",
    messages=messages,
)

print(response.choices[0].message.content)
```

```
<think>
嗯，用户问的是在单词“strawberry”中有几个字母R。首先，我需要确认单词的正确拼写，可能有时候会有拼写错误或者变体，但“strawberry”应该是正确的拼写。

接下来，我应该把单词分解开来，逐个字母检查。先写下单词：S-T-R-A-W-B-E-R-R-Y。或者可能我记错了，应该再仔细拼一遍。S-T-R-A-W-B-E-R-R-Y？或者是不是中间有其他字母？比如，strawberry的正确拼写是S-T-R-A-W-B-E-R-R-Y，对吗？是的，没错，正确的拼写是S-T-R-A-W-B-E-R-R-Y，也就是“strawberry”。

现在，我需要数一下里面有几个R。先从头开始，每个字母依次检查：

S - 不是R
T - 不是
R - 是第一个R
A - 不是
W - 不是
B - 不是
E - 不是
R - 第二个R
R - 第三个R
Y - 不是

所以看起来有三个R？但可能我哪里数错了。或者是不是中间的拼写有误？

或者可能用户的问题有陷阱？比如，是否在拼写中有错误？比如，正确的拼写是strawberry，其中R出现的位置是第三个字母，然后是第8个和第9个字母？或者是不是中间的拼写有误？

让我再仔细拼一遍：S-T-R-A-W-B-E-R-R-Y。对的，S是第一个字母，然后T，R，A，W，B，E，R，R，Y。所以R出现在第3、8、9的位置，也就是三个R？

或者是不是我数错了？比如，第三个字母是R，然后在第八个和第九个字母也是R？所以总共有三个R？

但可能用户可能有不同的答案，或者我哪里出错了？

或者是不是在单词中R出现两次？比如，strawberry的正确拼写是S-T-R-A-W-B-E-R-R-Y，对吗？所以第三个字母是R，然后第八和第九是R，所以三个R？

或者是不是在拼写中R只出现两次？比如，可能用户认为是strawberry中R出现两次？比如，可能我记错了拼写？

或者是不是应该拆分单词为straw + berry？straw是S-T-R-A-W，berry是B-E-R-R-Y，所以整个单词是strawberry，所以R在straw中的第三个字母，然后在berry中的第三个和第四个字母？所以总共有三个R？

是的，这样的话，strawberry中的R出现三次。所以答案应该是三个R？

不过，可能用户会误以为是两次，所以需要确认正确拼写和字母的位置。

或者可能用户的问题有其他陷阱？比如，是否在单词中有其他R？或者是否在拼写中存在错误？

比如，strawberry的正确拼写是S-T-R-A-W-B-E-R-R-Y，对吗？是的。所以R的位置是第三个、第八个和第九个字母，也就是三个R。因此，正确的答案应该是3个R。
</think>
```

在单词`"strawberry"`中，字母`R`出现了 **3 次**。具体位置如下：

1. 第3个字母：`R`（strawberry → S-T-R-A-W-B-E-R-R-Y）  
2. 第8个字母：`R`  
3. 第9个字母：`R`  

因此，总共有 **3 个 R**。

![](img/4912db1dc93ea2a6ce79da6cba738475.png)

## 五、基于llama.cpp的Qwen3模型CPU推理

Qwen3系列模型目前也是可以使用`llama.cpp`进行纯CPU推理或者CPU+GPU混合推理的。接下来介绍如何使用`llama.cpp`调用模型权重进行推理和对话。

### 1. llama.cpp下载与编译

+   `llama.cpp`项目主页：https://github.com/ggml-org/llama.cpp

![](img/ef5f96fd3280cfb041f8ee376cfbc15a.png)

由于`llama.cpp`是个C语言项目，因此实际调用过程需要先构建项目，然后设置参数进行编译，然后最终创建可执行文件（类似于脚本），再运行本地大模型。借助`llama.cpp`可以实现纯CPU推理、纯GPU推理和CPU+GPU混合推理。

+   依赖下载

```
apt-get update
apt-get install build-essential cmake curl libcurl4-openssl-dev -y
```

![](img/8f8fff373a098f236082118140612961.png)

这条命令安装了一些常用的构建和开发工具，具体的每个部分的含义如下：

+   `build-essential`：安装一组构建必需的工具和库，包括：

+   编译器（如`GCC`）

+   `make` 工具

+   其他一些常见的构建工具，确保你的系统能进行编译。

+   `cmake`：安装`CMake`工具，它是一个跨平台的构建系统，允许你管理项目的编译过程。

+   `curl`：安装`cURL`工具，它是一个命令行工具，用于通过 URL 发送和接收数据。它在很多开发场景中都很有用，尤其是与网络交互时。

+   `libcurl4-openssl-dev`：安装 libcurl 库的开发版本。它是`cURL`的一个库文件，允许你在编程中通过`cURL`发送 HTTP 请求。`libcurl4-openssl-dev` 是与 OpenSSL 配合使用的版本，提供了 SSL/TLS 加密支持，用于安全的 HTTP 请求。

+   `llama.cpp`源码下载

若是AutoDL服务器，可以先开启学术加速：

```
source /etc/network_turbo
```

如果是其他服务器或者本地服务器，则可以直接进行源码下载：

```
git clone https://github.com/ggerganov/llama.cpp
```

也可以直接在课件网盘中找到代码文件，直接上传服务器并解压缩：

![](img/86962fd89483c837b26714de5cbf5a7b.png)

![](img/7d5c5575ca7f9c778134a18a0aa183e2.png)

准备好后，即可在服务器中看到`llama.cpp`项目文件夹：

![](img/59048bde205c5fdd148005b408f732e1.png)

接下来，即可开始进行项目构建与编译。

+   项目构建与编译

```
cmake llama.cpp -B llama.cpp/build \
    -DBUILD_SHARED_LIBS=OFF -DGGML_CUDA=ON -DLLAMA_CURL=ON
```

+   `cmake`：运行 CMake 工具，用于配置和生成构建文件。

+   `llama.cpp`：指定项目的源代码所在的目录。在这个例子中，`llama.cpp` 是项目的根目录。

+   `-B llama.cpp/build`：指定生成构建文件的目录。`-B` 参数表示构建目录，`llama.cpp/build` 是生成的构建目录。这是 CMake 将生成的文件存放的地方（例如 Makefile 或 Ninja 构建文件）。

+   同时还指定了一些编译选项：

+   禁用共享库（`-DBUILD_SHARED_LIBS=OFF`），生成静态库。

+   启用 CUDA 支持（`-DGGML_CUDA=ON`），以便在有 GPU 的情况下使用 GPU 加速。

+   启用 CURL 库支持（`-DLLAMA_CURL=ON`），以便支持网络请求。

![](img/d6f54e70cbbebd25260d3c7538f5d4f4.png)

然后需要进一步进行编译：

```
cmake --build llama.cpp/build --config Release -j --clean-first --target llama-quantize llama-cli llama-gguf-split
```

+   `--build llama.cpp/build`：告诉 CMake 使用 `llama.cpp/build` 目录中的构建文件来执行构建过程。这个目录是在之前运行 `cmake llama.cpp -B llama.cpp/build` 命令时生成的，包含了所有构建所需的文件（例如 Makefile 或 Ninja 构建文件）。

+   `--config Release`：指定构建的配置为 `Release` 配置。

+   `Release` 配置通常意味着启用更多的 **优化**，生成的程序运行速度较快，适合发布。

+   在 CMake 中，通常有两种常见的构建配置：

+   `Debug`：用于调试版本，包含调试信息且没有做过多优化。

+   `Release`：优化后的发布版本，去除调试信息，运行时性能更高。

+   `-j`：表示并行构建，允许 CMake 使用多个 CPU 核心来加速构建过程。

+   如果没有指定数字，CMake 会使用默认的并行级别，通常是可用的所有 CPU 核心。你也可以指定并行的作业数，例如 `-j 8` 表示使用 8 个并行作业进行编译。

+   `--clean-first`：表示在构建之前先清理掉之前的构建结果。这可以确保每次构建时都是从一个干净的状态开始，避免由于缓存或中间文件引起的编译错误。

+   如果你之前运行过构建并且有问题，或者希望重新构建而不使用任何缓存文件，这个选项非常有用。

+   `--target`：指定构建的目标（target）。通常，一个项目会定义多个目标（比如库、可执行文件等），通过这个参数可以告诉 CMake 只编译特定的目标。

+   `llama-quantize`：可能是与模型量化相关的目标。量化（quantization）是将模型的精度从浮点数降低到整数，从而减少内存占用和提高推理速度。

+   `llama-cli`：可能是一个命令行工具，用于运行模型或与用户交互。

+   `llama-gguf-split`：可能是一个用于拆分模型文件的目标，通常用于将一个大模型文件拆分成多个小文件，方便存储和加载。

![](img/b61f7b10fd3f0d96c48b1f3be2a769a3.png)

+   复制可执行文件

```
cp llama.cpp/build/bin/llama-* llama.cpp
```

+   将 **所有生成的可执行文件** 从构建目录 `llama.cpp/build/bin/` 复制到项目的根目录 `llama.cpp` 下。这样可以更方便地在项目根目录下执行这些可执行文件，而无需每次都进入构建目录。

![](img/5e0aadd3ab070b4378c4db6a9ff33b4f.png)

在准备完成后，接下来即可进行调用和推理测试了。

### 2. 借助llama.cpp运行Qwen3模型

+   纯CPU推理流程【1token/s】

首先是纯CPU推理测试。此时系统只调用内存+CPU进行计算，此时不会用到GPU。

![](img/92d2a5ba3dcf96f30cd045b827f11d51.png)

此时模型运行门槛很低，但运行速度非常慢，只有不到1tokens/s。具体推理代码实现流程如下：

```
cd ./llama.cpp

./llama-cli --model /home/08_vllm/Qwen3-14B-GGUF/Qwen3-14B-Q4_K_M.gguf --cache-type-k q4_0 --threads 64 --prio 2 --temp 0.6 --ctx-size 512 --seed 3407 --n-gpu-layers 0 -no-cnv --prompt "<｜User｜>你好，好久不见，请介绍下你自己。<｜Assistant｜>"
```

![](img/f944774212e49d7197973dabd9bacb4a.png)

其中命令行核心参数说明：

+   `--threads`：CPU 核心数；

+   `--ctx-size`：输出的上下文长度；

+   `--n-gpu-layers`：需要卸载到 GPU 的层数，设置为 0 时代表完全使用 CPU 进行推理；

+   `--temp`：模型温度参数；

+   `-no-cnv`：不进行多轮对话；

+   `--cache-type-k`：K 缓存量化为 4bit；

+   `--seed`：随机数种子；

实际运行效果如下所示：

![](img/032136d11302d1381055816e073b572c.png)

+   CPU+GPU混合推理

接下来进一步尝试CPU+GPU混合推理，我们只需要合理的设置`--n-gpu-layers`参数，即可灵活的将模型的部分层加载到GPU上进行运行。并且无需手动设置，`llama.cpp`会自动识别当前GPU数量以及可以分配的显存，自动将模型权重加载到各个不同的GPU上。例如，我们这里考虑将30层加载到GPU上，运行效果如下所示：

```
./llama-cli \
    --model /home/08_vllm/Qwen3-14B-GGUF/Qwen3-14B-Q4_K_M.gguf \
    --cache-type-k q4_0 \
    --threads 64 \
    --prio 2 \
    --temp 0.6 \
    --ctx-size 512 \
    --seed 3407 \
    --n-gpu-layers 30 \
    -no-cnv \
    --prompt "<｜User｜>你好，好久不见，请介绍下你自己。<｜Assistant｜>"
```

![](img/efd92ff69a6fcecee66e074940a11c90.png)

此时显存占用不到10G：

![](img/c27eaef32f0486d34afde8cf37d8cdd4.png)

推理速度略微有所提升，能达到接近2tokens/s

+   纯GPU推理

最后，我们更进一步，尝试把全部的模型权重都放在GPU上进行推理。

```
./llama-cli \
    --model /home/08_vllm/Qwen3-14B-GGUF/Qwen3-14B-Q4_K_M.gguf \
    --cache-type-k q4_0 \
    --threads 64 \
    --prio 2 \
    --temp 0.6 \
    --ctx-size 512 \
    --seed 3407 \
    --n-gpu-layers 64 \
    -no-cnv \
    --prompt "<｜User｜>你好，好久不见，请介绍下你自己。<｜Assistant｜>"
```

此时GPU占用约11G：



![](img/f1953c24e8e3b53427e56d65eba798d3.png)

推理速度则能达到14tokens/s。

## 六、Qwen3 接入OpenWeb-UI

### 1. Open-WebUI部署流程

首先需要安装Open-WebUI，官网地址如下：`https://github.com/open-webui/open-webui`。

![](img/ed8521da59a8cee87e961f41fe5ed6f4.png)

我们可以直接使用`pip`命令快速完成安装：

```
    pip install open-webui
```

![](img/425d211a74ef41da053b0e6c434a0c39.png)

可以直接使用在GitHub项目主页上直接下载完整代码包，并上传至服务器解压缩运行：

![](img/501a8d75736d1aee6731d02b48945d17.png)

此外，也可以在课件网盘中领取完整代码包，并上传至服务器解压缩运行：

![](img/4fec41b524b0954e19df40943da7e608.png)

![](img/05c7aa98a7b3aa52daf8a9220388140e.png)

在确保`ollama`正常运行的情况下，进行后续操作。

### 2. Open-WebUI启动与对话流程

在准备好了Open-WebUI和一系列模型权重后，接下来我们尝试启动Open-WebUI，并借助本地模型进行问答。

首先需要设置离线环境，避免Open-WebUI启动时自动进行模型下载：

```
    export HF_HUB_OFFLINE=1
```

然后启动Open-WebUI

```
    open-webui serve
```

![](img/14c7c97f3736ff778d4b464b9151899b.png)

需要注意的是，如果启动的时候仍然报错显示无法下载模型，是Open-WebUI试图从huggingface上下载embedding模型，之后我们会手动将其切换为本地运行的Embedding模型。

![](img/982192cc6e017808364ec436e6262956.png)

然后在本地浏览器输入地址:8080端口即可访问：

![](img/0d8a05dc1ada9c93ef450f95ddefaf4e.png)

> 若使用AutoDL，则需要使用SSH隧道工具进行地址代理

![](img/838651c44899bd70f8b4e8f85331fe80.png)

> 更多AutoDL相关操作详见公开课：《AutoDL快速入门与GPU租赁使用指南》|https://www.bilibili.com/video/BV1bxB7YYEST/

![](img/11bb43b12cc0644d57cf9833d38bdf86.png)

然后首次使用前，需要创建管理员账号：

![](img/f59cd0d92764ecb55f28930c715d4bbf.png)

然后点击登录即可。需要注意的是，此时Open-WebUI会自动检测后台是否启动了`ollama`服务，并列举当前可用的模型。稍等片刻，即可进入到如下页面：

![](img/3876f89f7e8559529600666d29d0ddb0.png)

接下来即可进入到对话页面：

![](img/28174d94eb4471500bdb02d1091c182d.png)

对话效果如下所示：

![](img/0dcd6150b786a4c92f4188b0a4bd7e2e.png)

### 3. 本地知识库检索

![](img/a8e91edd44301cb29b25cdd7d0c22cb4.png)

![](img/fe298b9b71dac2889810c45ed505abed.png)

![](img/7b1f9daa5d7c79524bef773abfafb951.png)

### 4. 代码解释器

![](img/b19227b61573d1618eebc79608078d1f.png)

![](img/7c94a7ad6358f2899d59058d13cf520f.png)

### 5. 调用外部工具

Qwen3全系列支持Function calling，因此我们可以基于此完成Open-WebUI的外部工具调用工作。

+   Open-WebUI工具调用功能实现

![](img/81e285c14210fe04a4f5aa9026d0319d.png)

然后编写天气查询函数：

```
    import requests
    import json
    from fastapi import Request
    from open_webui.models.users import Users

    class Tools:
        def __init__(self):
            pass

        async def get_weather(
            self, loc: str, __request__: Request, __user__: dict, __event_emitter__=None
        ) -&gt; str:
            &quot;&quot;&quot;
            获取指定城市的即时天气信息。

            :param loc: 城市名称（如果是非英文城市名称，请先将其翻译为英文城市名称再输入）。
            :param __request__: HTTP 请求对象（来自 FastAPI）。
            :param __user__: 用户信息（可以用于个性化或记录请求）。
            :param __event_emitter__: 事件发射器，用于将实时状态更新发送到前端。
            :return: 格式化后的天气数据，作为字符串返回。
            
            说明：
            如果输入的是非英文城市名称，请先将其翻译为对应的英文城市名称再输入。
            比如，输入“中国，北京”，需要转为 &quot;Beijing&quot;。
            &quot;&quot;&quot;

            # Step 1. 通知用户正在获取天气数据
            await __event_emitter__(
                {
                    &quot;type&quot;: &quot;status&quot;,
                    &quot;data&quot;: {&quot;description&quot;: f&quot;正在获取 {loc} 的天气数据...&quot;, &quot;done&quot;: False},
                }
            )

            try:
                # Step 2. 构建请求的 URL 和查询参数
                url = &quot;https://api.openweathermap.org/data/2.5/weather&quot;
                params = {
                    &quot;q&quot;: loc,
                    &quot;appid&quot;: &quot;YOUR_API_KEY&quot;,  # 你的 API 密钥
                    &quot;units&quot;: &quot;metric&quot;,  # 使用摄氏度
                    &quot;lang&quot;: &quot;zh_cn&quot;,  # 输出简体中文
                }

                # Step 3. 向 OpenWeather API 发送 GET 请求
                response = requests.get(url, params=params)

                # Step 4. 解析响应数据
                data = response.json()

                # Step 5. 提取并格式化天气描述
                weather_data = data.get(&quot;weather&quot;, [])
                if weather_data:
                    main_weather = weather_data[0].get(&quot;main&quot;, &quot;&quot;)
                    description = weather_data[0].get(&quot;description&quot;, &quot;&quot;)
                    weather_info = f&quot;当前天气：{main_weather} - {description}&quot;
                else:
                    weather_info = &quot;天气信息不可用。&quot;

                # Step 6. 通知用户天气数据已经成功获取
                await __event_emitter__(
                    {
                        &quot;type&quot;: &quot;status&quot;,
                        &quot;data&quot;: {&quot;description&quot;: f&quot;成功获取 {loc} 的天气数据&quot;, &quot;done&quot;: True},
                    }
                )

                # Step 7. 返回格式化后的天气信息并发送到前端
                await __event_emitter__(
                    {
                        &quot;type&quot;: &quot;message&quot;,
                        &quot;data&quot;: {&quot;content&quot;: f&quot;{loc} 的天气信息: {weather_info}&quot;},
                    }
                )

                # 返回格式化后的天气数据作为字符串
                return f&quot;{loc} 的天气信息: {weather_info}&quot;;

            except Exception as e:
                # Step 8. 如果发生错误，通知用户
                await __event_emitter__(
                    {
                        &quot;type&quot;: &quot;status&quot;,
                        &quot;data&quot;: {&quot;description&quot;: f&quot;发生错误: {str(e)}&quot;, &quot;done&quot;: True},
                    }
                )

                # 返回错误信息
                return f&quot;获取天气数据时发生错误：{str(e)}&quot;;
```

并将函数进行写入：

![](img/7e6ecb7bcc75f20fd50ee4821dcf56a4.png)

完成后即可看到新的工具：

![](img/0e6378ef45fea024247e8dc88277eca7.png)

在对话时可以开启天气查询函数：

![](img/5ef44530dd03e90169a7ad05e9670b85.png)

![](img/75d228b328be36f003767a35cccda764.png)

至此，我们就详细介绍了QwQ模型的各类本地部署与调用方法。
