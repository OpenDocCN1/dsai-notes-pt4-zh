# 第三天：LLM和多模态模型高效推理实践

> 来源：[https://seigy6zzam.feishu.cn/docx/DyFFdna0PoIcaWxNZHCc4CPknx4](https://seigy6zzam.feishu.cn/docx/DyFFdna0PoIcaWxNZHCc4CPknx4)

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=MzkxNTM5NTg2OA==&mid=2247489961&idx=1&sn=4b585053de23b9aca3238b8336fb05b7&chksm=c15e9ddaf62914cc2d1bd3f2748996dd806e8926e2c13f666e8eb45fef7eeab40d0c80a95824&cur_album_id=3322557210689912835&scene=190#rd)q.com/s?__biz=Mz...

> ⏰ 剪存时间：2024-02-24 03:56:19 (UTC+8)

> ✂️ 本文档由 [飞书剪存 ](https://www.feishu.cn/hc/zh-CN/articles/606278856233?from=in_ccm_clip_doc)一键生成

原创 魔搭官方 魔搭ModelScope社区 2024-02-11 22:49  广西

![](img/20b3704219ed9022267bfdb52be57a5b.png)

# **内容摘要**

本课程以Qwen系列模型为例，主要介绍在魔搭社区如何高效推理LLM和多模态模型，主要包括如下内容：

+   **LLM和多模态大模型的推理**

+   使用ModelScope NoteBook免费GPU推理Qwen-1.8B-Chat-int4

+   使用ModelScope NoteBook免费GPU推理Qwen-VL-Chat-int4

+   使用ModelScope NoteBook免费GPU推理Qwen-audio-Chat

+   **推理加速和多端推理**

+   推理加速：vLLM+fastchat加速推理

+   多端推理适配-Qwen.cpp和LLaMa.cpp转化模型为gguf或者ggml，并结合Xinference在本地笔记本部署。

+   LLM的应用场景，RAG&Agent

+   使用llama index和langchain打造基于本地知识库的ChatBot

# **多模态大模型推理**

**LLM的推理流程：**

![](img/45c64b9ab99d3f0fc4cb1c3a6b794b67.png)

**多模态的LLM的原理：**

![](img/b379dfb27c65eb9f84da6afc2de730f7.png)

**代码演示：**

使用ModelScope NoteBook完成语言大模型，视觉大模型，音频大模型的推理

## **环境配置与安装**

以下主要演示的模型推理代码可在魔搭社区免费实例PAI-DSW的配置下运行（显存24G） ：

1.  点击模型右侧Notebook快速开发按钮，选择GPU环境：

![](img/6e050a6de4f8ef53a8a94e27455a665c.png)

1.  打开Python 3 (ipykernel)：

![](img/819e35625e32ad054eb06f46fe7297a1.png)

## **示例代码 ** **  **

### 语言大模型推理示例代码

```
#通义千问1_8B LLM大模型的推理代码示例
#通义千问1_8B:https://modelscope.cn/models/qwen/Qwen-1_8B-Chat/summary
from modelscope import AutoModelForCausalLM, AutoTokenizer, GenerationConfig

# Note: The default behavior now has injection attack prevention off.
tokenizer = AutoTokenizer.from_pretrained(&quot;Qwen/Qwen-1_8B-Chat&quot;, revision=&#x27;master&#x27;, trust_remote_code=True)

# use bf16
# model = AutoModelForCausalLM.from_pretrained(&quot;Qwen/Qwen-1_8B-Chat&quot;, device_map=&quot;auto&quot;, trust_remote_code=True, bf16=True).eval()
# use fp16
# model = AutoModelForCausalLM.from_pretrained(&quot;Qwen/Qwen-1_8B-Chat&quot;, device_map=&quot;auto&quot;, trust_remote_code=True, fp16=True).eval()
# use cpu only
# model = AutoModelForCausalLM.from_pretrained(&quot;Qwen/Qwen-1_8B-Chat&quot;, device_map=&quot;cpu&quot;, trust_remote_code=True).eval()
# use auto mode, automatically select precision based on the device.
model = AutoModelForCausalLM.from_pretrained(&quot;Qwen/Qwen-1_8B-Chat&quot;, revision=&#x27;master&#x27;, device_map=&quot;auto&quot;, trust_remote_code=True).eval()

# Specify hyperparameters for generation. But if you use transformers&gt;=4.32.0, there is no need to do this.
# model.generation_config = GenerationConfig.from_pretrained(&quot;Qwen/Qwen-1_8B-Chat&quot;, trust_remote_code=True) # 可指定不同的生成长度、top_p等相关超参

# 第一轮对话 1st dialogue turn
response, history = model.chat(tokenizer, &quot;你好&quot;, history=None)
print(response)
# 你好！很高兴为你提供帮助。

# 第二轮对话 2nd dialogue turn
response, history = model.chat(tokenizer, &quot;给我讲一个年轻人奋斗创业最终取得成功的故事。&quot;, history=history)
print(response)
# 这是一个关于一个年轻人奋斗创业最终取得成功的故事。
# 故事的主人公叫李明，他来自一个普通的家庭，父母都是普通的工人。从小，李明就立下了一个目标：要成为一名成功的企业家。
# 为了实现这个目标，李明勤奋学习，考上了大学。在大学期间，他积极参加各种创业比赛，获得了不少奖项。他还利用课余时间去实习，积累了宝贵的经验。
# 毕业后，李明决定开始自己的创业之路。他开始寻找投资机会，但多次都被拒绝了。然而，他并没有放弃。他继续努力，不断改进自己的创业计划，并寻找新的投资机会。
# 最终，李明成功地获得了一笔投资，开始了自己的创业之路。他成立了一家科技公司，专注于开发新型软件。在他的领导下，公司迅速发展起来，成为了一家成功的科技企业。
# 李明的成功并不是偶然的。他勤奋、坚韧、勇于冒险，不断学习和改进自己。他的成功也证明了，只要努力奋斗，任何人都有可能取得成功。

# 第三轮对话 3rd dialogue turn
response, history = model.chat(tokenizer, &quot;给这个故事起一个标题&quot;, history=history)
print(response)
# 《奋斗创业：一个年轻人的成功之路》

# Qwen-1.8B-Chat现在可以通过调整系统指令（System Prompt），实现角色扮演，语言风格迁移，任务设定，行为设定等能力。
# Qwen-1.8B-Chat can realize roly playing, language style transfer, task setting, and behavior setting by system prompt.
response, _ = model.chat(tokenizer, &quot;你好呀&quot;, history=None, system=&quot;请用二次元可爱语气和我说话&quot;)
print(response)
# 你好啊！我是一只可爱的二次元猫咪哦，不知道你有什么问题需要我帮忙解答吗？

response, _ = model.chat(tokenizer, &quot;My colleague works diligently&quot;, history=None, system=&quot;You will write beautiful compliments according to needs&quot;)
print(response)
# Your colleague is an outstanding worker! Their dedication and hard work are truly inspiring. They always go above and beyond to ensure that 
# their tasks are completed on time and to the highest standard. I am lucky to have them as a colleague, and I know I can count on them to handle any challenge that comes their way.
```

输出结果：

![](img/1abe23f5e58215e3860f8453ad5f5f27.png)

### 视觉大模型推理示例代码

```
#Qwen-VL 是阿里云研发的大规模视觉语言模型（Large Vision Language Model, LVLM）。Qwen-VL 可以以图像、文本、检测框作为输入，并以文本和检测框作为输出。Qwen-VL 系列模型性能强大，具备多语言对话、多图交错对话等能力，并支持中文开放域定位和细粒度图像识别与理解。
from modelscope import (
    snapshot_download, AutoModelForCausalLM, AutoTokenizer, GenerationConfig
)
from auto_gptq import AutoGPTQForCausalLM

model_dir = snapshot_download(&quot;qwen/Qwen-VL-Chat-Int4&quot;, revision=&#x27;v1.0.0&#x27;)

import torch
torch.manual_seed(1234)

# Note: The default behavior now has injection attack prevention off.
tokenizer = AutoTokenizer.from_pretrained(model_dir, trust_remote_code=True)

# use cuda device
model = AutoModelForCausalLM.from_pretrained(model_dir, device_map=&quot;cuda&quot;, trust_remote_code=True,use_safetensors=True).eval()

# 1st dialogue turn
query = tokenizer.from_list_format([
    {&#x27;image&#x27;: &#x27;https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-VL/assets/demo.jpeg&#x27;},
    {&#x27;text&#x27;: &#x27;这是什么&#x27;},
])
response, history = model.chat(tokenizer, query=query, history=None)
print(response)
# 图中是一名年轻女子在沙滩上和她的狗玩耍，狗的品种可能是拉布拉多。她们坐在沙滩上，狗的前腿抬起来，似乎在和人类击掌。两人之间充满了信任和爱。

# 2nd dialogue turn
response, history = model.chat(tokenizer, &#x27;输出&quot;狗&quot;的检测框&#x27;, history=history)
print(response)

image = tokenizer.draw_bbox_on_latest_picture(response, history)
if image:
  image.save(&#x27;1.jpg&#x27;)
else:
  print(&quot;no box&quot;)
```

输出结果：

![](img/9ebfc2f519a76948ef47563af0e22e95.png)

![](img/6afb7d773b8a763699f157dbe33532b5.png)

### 音频大模型推理示例代码

```
from modelscope import (
    snapshot_download, AutoModelForCausalLM, AutoTokenizer, GenerationConfig
)
import torch
model_id = &#x27;qwen/Qwen-Audio-Chat&#x27;
revision = &#x27;master&#x27;

model_dir = snapshot_download(model_id, revision=revision)
torch.manual_seed(1234)

tokenizer = AutoTokenizer.from_pretrained(model_dir, trust_remote_code=True)
if not hasattr(tokenizer, &#x27;model_dir&#x27;):
    tokenizer.model_dir = model_dir

# Note: The default behavior now has injection attack prevention off.
tokenizer = AutoTokenizer.from_pretrained(model_dir, trust_remote_code=True)

# use bf16
# model = AutoModelForCausalLM.from_pretrained(model_dir, device_map=&quot;auto&quot;, trust_remote_code=True, bf16=True).eval()
# use fp16
# model = AutoModelForCausalLM.from_pretrained(model_dir, device_map=&quot;auto&quot;, trust_remote_code=True, fp16=True).eval()
# use cpu only
# model = AutoModelForCausalLM.from_pretrained(model_dir, device_map=&quot;cpu&quot;, trust_remote_code=True).eval()
# use cuda device
model = AutoModelForCausalLM.from_pretrained(model_dir, device_map=&quot;cuda&quot;, trust_remote_code=True).eval()

# 1st dialogue turn
query = tokenizer.from_list_format([
    {&#x27;audio&#x27;: &#x27;https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Audio/1272-128104-0000.flac&#x27;}, # Either a local path or an url
    {&#x27;text&#x27;: &#x27;what does the person say?&#x27;},
])
response, history = model.chat(tokenizer, query=query, history=None)
print(response)
# The person says: &quot;mister quilter is the apostle of the middle classes and we are glad to welcome his gospel&quot;.

# 2nd dialogue turn
response, history = model.chat(tokenizer, &#x27;Find the start time and end time of the word &quot;middle classes&quot;&#x27;, history=history)
print(response)
# The word &quot;middle classes&quot; starts at &lt;|2.33|&gt; seconds and ends at &lt;|3.26|&gt; seconds.
```

输出结果：

![](img/0f08fbcb7bcbc2084c970810b0316b5c.png)

# vLLM+FastChat高效推理实战

FastChat是一个开放平台，用于训练、服务和评估基于LLM的ChatBot。

FastChat的核心功能包括：

+   优秀的大语言模型训练和评估代码。

+   具有Web UI和OpenAI兼容的RESTful API的分布式多模型服务系统。

vLLM是一个由加州伯克利分校、斯坦福大学和加州大学圣迭戈分校的研究人员基于操作系统中经典的虚拟缓存和分页技术开发的LLM服务系统。他实现了几乎零浪费的KV缓存，并且可以在请求内部和请求之间灵活共享KV高速缓存，从而减少内存使用量。

**FastChat开源链接：**https://github.com/lm-sys/FastChat

**vLLM开源链接：**https://github.com/vllm-project/vllm

## **实战演示**

安装FastChat最新包

```
git clone https://github.com/lm-sys/FastChat.git
cd FastChat
pip install .
```

## **环境变量设置**

在vLLM和FastChat上使用魔搭的模型需要设置两个环境变量：

```
export VLLM_USE_MODELSCOPE=True
export FASTCHAT_USE_MODELSCOPE=True
```

## 使用FastChat和vLLM实现发布model worker(s)

可以结合FastChat和vLLM搭建一个网页Demo或者类OpenAI API服务器，首先启动一个controller：

```
python -m fastchat.serve.controller
```

然后启动vllm_worker发布模型。如下给出单卡推理的示例，运行如下命令：

千问模型示例：

```
#以qwen-1.8B为例，在A10运行python -m fastchat.serve.vllm_worker --model-path qwen/Qwen-1_8B-Chat --trust-remote-code --dtype bfloat16
```

启动vLLM优化worker后，本次实践启动页面端demo展示：

```
python -m fastchat.serve.gradio_web_server --host 0.0.0.0 --port 8000
```

让我们体验极致推理优化的效果吧!

中文案例（千问1.8B） ** **

TODO

# **多端部署实战 **

魔搭社区和Xinference合作，提供了模型 GGML的部署方式， 以qwen+个人Mac电脑为例 。

Xinference支持大语言模型，语音识别模型，多模态模型的部署，简化了部署流程，通过一行命令完成模型的部署工作。并支持众多前沿的大语言模型，结合GGML技术，支持多端部署。

![](img/7b25cc91b6e45ac31f46431a0c1ced13.png)

使用Qwen.cpp实现通义千问的多端部署：

多端部署以1.8B模型为例，

第一步：使用qwen.cpp将pytorch格式的千问模型转为GGML格式。

+

```
python3 qwen_cpp/convert.py -i qwen/Qwen-1_8-Chat -t q4_0 -o qwen-1_8b-ggml.bin
```

第二步：在Xinference上launch模型，并部署到Mac笔记本实现推理。

关闭

**观看更多**

更多

退出全屏

切换到横屏模式

[🎦 点击观看视频](https://mpvideo.qpic.cn/0bc3ryabkaaammaltngj3nsvbdwdcwhaafia.f10002.mp4?dis_k=9f11f0fa6cdf4a2fb8c13ac1b03c6bcb&dis_t=1708775776&play_scene=10120&auth_info=cq7imLJfOSgdzciBmVlcG2xOZB8DdWFHCkZqJTMAeRFJJFt+dFBkR384Qm0zeEshZg==&auth_key=51f5f39cbffe1c4a64e48cb3262c6755&vid=wxv_3323961597702291459&format_id=10002&support_redirect=0&mmversion=false)

> 无法获取音频链接

视频详情

# **LLM的应用场景：RAG**

LLM会产生误导性的 “ **幻觉 **”，依赖的信息可能 **过时 **，处理特定知识时 **效率不高 **， **缺乏专业领域 **的深度洞察，同时在推理能力上也有所欠缺。

正是在这样的背景下，检索增强生成技术（Retrieval-Augmented Generation，RAG）应时而生，成为 AI 时代的一大趋势。

RAG 通过在语言模型生成答案之前，先从广泛的文档数据库中检索相关信息，然**后利用这些信息来**引导生成过**程，极大地提升了**内容的准确性和相关性。**RAG 有****效地 缓解了**幻觉问题 ，提高了 知识更新的速度 ，并增强了内容生成的 可追溯性 ，使得大型语言模型在实际应用中变得更加 实用和可信 。

一个典型的RAG的例子：

![](img/417fb798f9cf40f30d65db7fc5a6bdc8.png)

这里面主要包括包括三个基本步骤：

1. 索引 — 将文档库分割 成较短的 Chunk，并通过编码器构建向量索引。

2. 检索 — 根据问题和 chunks 的相似度检索相关文档片段。

3. 生成 — 以检索到的上下文为条件，生成问题的回答。

RAG（开卷考试）VS. Finetune（专业课程学习）

![](img/dd88e0b8892b51862cdefdd60492e679.png)

**示例代码： ** https://github.com/modelscope/modelscope/blob/master/examples/pytorch/application/qwen_doc_search_QA_based_on_langchain_llamaindex.ipynb

点击 [**阅读原文**](https://www.bilibili.com/video/BV1RQ4y1w7MQ/)** **，直达本期课程视频学习