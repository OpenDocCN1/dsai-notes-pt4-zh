
# 使用智谱 GLM-4-9B 和 SiliconCloud 云服务快速构建一个编码类智能体应用

> 来源：[https://agispace.feishu.cn/docx/Xd76dKtxEoiDLcxlzX9c1aPOno3](https://agispace.feishu.cn/docx/Xd76dKtxEoiDLcxlzX9c1aPOno3)
>
> 原创：是莫尔索
>
> 👸号：莫尔索随笔
>
> 原文链接：https://mp.weixin.qq.com/s/Ym6QYBfouJLuyELEqAAeTw

## GLM-4-9B 有多强

智谱家 `GLM-4-9B` 模型的发布，可以称得上大模型开源领域的又一个里程碑事件，除了开源行为本身值得肯定，我觉得开源出来的模型可以接入线上应用直接使用，可能对我们做应用层的开发者意义更大。话不多说，看 `GLM-4-9B` 的介绍：

首先 `GLM-4-9B` 模型结构与 `GLM-3-9B` 变化不大，主要是模型层数由 28 增加到 40，词表大小由 65024 扩充到 151552、支持的上下文长度支持从 32K、128K 扩展到 128K、1M（`GLM-4-9B-Chat-1M`），做应用最关注的就是长上下文（多轮对话记忆保持、各种阅读助手、长文本理解等常见场景）能力和 `Function Call()` 能力（工具调用，构建智能体应用的基础）。

1M 的上下文长度（约 200 万中文字符）方面，`GLM-4-9B` 在大海捞针([1])测试中全绿。

![](img/78a068af77b01209be770a8c202e2023.png)

大海捞针测试

不过根据英伟达研究团队最近新提出的名为 `RULER()` 的新基准，这里选用的测试方法（测试的 `LWM` 声称上下文长度 1M，实际不到 4K）测出来的结果有水分，这个我还会自己测试下。

![](img/2f7a1d931caa4579a0e8ed51b9085250.png)

很多大模型上下文长度虚标严重

`Function Call` 能力也属于 `gpt-4-turbo` 级别，使用 `Berkeley Function-Calling Leaderboard`([2]) 测试集，这个对我来说比较有说服力，有兴趣的可以看看测试集设计，不过毕竟测试集公开，混在训练集里也不好说，这个我也按照相同思路设计了对应的私有中文测试集，引入之前也会再跑一次作为交叉验证。

虽然开源，但也存在限制，就是学术研究免费，商业用途需要登记且必须遵守相关条款和条件，详见 `Github` 项目介绍：https://github.com/THUDM/GLM-4

## SiliconCloud 有多好用

`SiliconCloud` 是硅基流动推出的 `GenAI` 云服务，这是国内同类产品中我体验最好的一家，便宜且推理速度还快，国外已经有很多类似服务了，比如 `Banana`([3])，`Replicate`([4])，`Beam`([5])，`Modal`([6])、`OctoAI`([7])、`ModelZ`([8])、`BentoML`([9])等，这类服务主要用于私有模型或常见开源模型托管，通过它们自研的推理加速引擎、大模型基础设施优化能力，大幅降低大模型的部署及推理成本，降低 `AI` 应用的成本，加速 `AI` 产品的落地。

![](img/49dced707312ad0fa308fed82f645fae.png)

推理价格

`SiliconCloud` 便宜又快的原因就在于硅基流动自研的 `LLM` 推理加速引擎 `SiliconLLM`，支持 `Llama3`、`Mixtral`、`DeepSeek`、`Baichuan`、`ChatGLM()`、`Falcon`、`01-ai`（零一万物开源的系列模型）、`GPT-NeoX` 等模型加速，下面是 `SiliconLLM` 与推理框架 `vLLM`([10])（伯克利大学 LMSYS 组织开源）、`Tensorrt-LLM`([11])（英伟达开源）的性能比较。

![](img/023e9bb4b8be843e42d3a9465edbb7fd.png)

SiliconLLM与vLLM、Tensorrt-LLM性能比较

最后补充一组我自己本地随手测试的 API 服务数据：

1. `首次响应时间与最快响应时间差`（`Latency saved 0.03s`）：表示首次响应时间与最快响应时间之间的差异，这个指标可以反映出大模型 API 服务在处理请求时的波动。

2. 优化后的首 token 响应时间（`Optimized TTFT 0.36s`）：是指在多次请求中，最快的一次首 token 响应时间。

3. `首 token 响应时间中位数`（`Median TTFT 0.38s`）：是指在所有请求中，首 token 响应时间的中位数，即一半的请求首 token 响应时间比这个值快，另一半比这个值慢。中位数可以提供一个更稳健的性能指标，因为它不受极端值的影响。

4. `生成的 token 数量`（`Tokens: 19`）：表示在请求过程中生成的 token 总数。

5. `token 生成速率`（`79 tokens/sec`）：表示每秒生成的 Token 数量，这是衡量大模型 API 服务处理能力的一个指标。

6. `总时间`（`Total time: 0.59s`）：表示从开始发送 HTTP 请求到接收到最后一个 token 的时间，这是整个请求处理过程的总耗时

API吞吐

## 构建编码类智能体应用

首先前往官网 👉 https://cloud.siliconflow.cn/auth/login 注册 `SiliconCloud` 账号，无需手机号，邮箱注册即可（值得一提，新用户注册可以得到 42 元免费额度用于体验，相当于 3 亿 tokens），按流程注册，保存好生成的 `API-KEY`。

生成API密钥

开始之前先设置好 `SiliconCloud` 的 `GLM-4-9B-Chat` 模型，`API` 调用方式也与 `OpenAI` 兼容，所以可以直接使用 `OpenAI SDK`（`langchain_openai`）来访问 `SiliconCloud` 上的任意模型。

```
import os
from langchain_openai import ChatOpenAI
sc_api_key = os.getenv("SC_API_KEY")
llm = ChatOpenAI(base_url="https://api.siliconflow.cn/v1",
    api_key=sc_api_key,
    model="zhipuai/glm4-9B-chat")
```

后续代码和这篇文章 [`DeepSeek-V2` 到底有多强？写一个 `AI`](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247485039&idx=1&sn=cddc45babe6dfba911e7971d2c0a0d08&scene=21#wechat_redirect) 编码 `Agent` 测测看（附详细代码）基本一致，也有着详细解释过程，这里不再赘述，后台回复 `DeepSeek` 可获取完整代码，点击阅读原文，获取更好阅读体验。

![](img/663728e4ee501774822b4db7f19fa08a.png)

## 莫尔索随笔

### 跟踪 `LLM`（大语言模型）最新动态，分享第一手 `LLM` 实践经验，致力于打造个人 `LLM` 应用开发全栈指南！

### 74篇原创内容

### 👸号

### 我写过的 `LangChain` 相关文章合集

+   从零开始学 `LangChain`（1）：入门介绍
+   [从零开始学 `LangChain`（2）：`Model I/O` 模块](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247483866&idx=1&sn=3b50f450f67367c3024c1193223a88a0&scene=21#wechat_redirect) 和 `Data Connection` 模块
+   [从零开始学 `LangChain`（3）](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247483890&idx=1&sn=f1bc87d4e4087df24121c32a7e6a85ee&scene=21#wechat_redirect)：`Memory` 模块和 `Chain` 模块
+   [从零开始学 `LangChain`（4）：`Agents` 模块和 `Callbacks` 模块](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247483894&idx=1&sn=bb3ab52b45ffe3c6d18fce41ded748a4&scene=21#wechat_redirect)
+   [`LLM` 应用开发框架 `Semantic Kernel` 和 `LangChain` 比较（上篇）](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247484170&idx=1&sn=044f341d858934e7e4b8343fdba53cff&scene=21#wechat_redirect)
+   [`LLM` 应用开发框架 `Semantic Kernel` 和 `LangChain` 比较（下篇）](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247484176&idx=1&sn=f1e46b697571c195fc3bb499cb390449&scene=21#wechat_redirect)
+   轻装上阵，加速商业化！`LangChain` 0.1 预发布看点！
+   [`LangChain` 在多轮对话中让 `Agent` 保持长期记忆的 8 种方式](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247484590&idx=1&sn=7b5acda8ed4cef7a593c02a694470539&scene=21#wechat_redirect)
+   如何使用 `LangChain` 实现 `Agent` 的 `ReAct` 模式
+   `LangChain` 创始人最新访谈：开源 `LLM` 中间件的未来与挑战
+   [刚刚！`LangChain` 宣布推出 `LangChain v0.2` 预发布版本！](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247485046&idx=1&sn=b0e8e3c0dbee8c3ebf21c50c329ce74e&scene=21#wechat_redirect)
+   [使用 `DeepSeek-V2`+`LangGraph` 编写了一个编码助手类 `AI Agent`](https://mp.weixin.qq.com/s?__biz=Mzg2OTk1NDQ4Ng==&mid=2247485039&idx=1&sn=cddc45babe6dfba911e7971d2c0a0d08&scene=21#wechat_redirect)
