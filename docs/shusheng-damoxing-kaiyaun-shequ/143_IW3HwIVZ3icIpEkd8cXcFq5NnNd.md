# L1G3-LMDeploy 课程

> 来源：[https://aicarrier.feishu.cn/docx/Lm3ydVNPBozXp7xUTGFcAsPQnXb](https://aicarrier.feishu.cn/docx/Lm3ydVNPBozXp7xUTGFcAsPQnXb)

> 文档尚处于草稿阶段，完善中

LMDeploy 是一个高效且友好**的 LLMs 模**型部署工具箱，功能涵盖了量化、推理和服务

## 安装

1.  pip 安装

conda简单使用

```
conda create -n lmdeploy python=3.10
conda activate lmdeploy
pip install lmdeploy
pip install timm==1.0.15 #安装本地推理需要的依赖
```

## 快速上手

1.  本地推理

```
import lmdeploy
from lmdeploy import GenerationConfig

pipe = lmdeploy.pipeline(&quot;/root/share/new_models/InternVL3/InternVL3-1B&quot;)
response = pipe(prompts=[&quot;Hi, pls intro yourself&quot;, &quot;Shanghai is&quot;],
                gen_config=GenerationConfig(max_new_tokens=1024,
                                            top_p=0.8,
                                            top_k=40,
                                            temperature=0.6))
print(response)
```

![](img/57fc52256943044063278ee0d48ecb05.png)

1.  指定推理引擎

> 在构造 pipeline 时，如果没有指定使用 TurboMind 引擎或 PyTorch 引擎进行推理，默认优先使用 TurboMind 引擎，实际上，LMDeploy 将根据提供的检查点自动确定使用哪种引擎。

```
### TurbomindEngineConfig推理引擎

from lmdeploy import pipeline, TurbomindEngineConfig
from lmdeploy import GenerationConfig

pipe = pipeline(&#x27;/root/share/new_models/InternVL3/InternVL3-1B&#x27;,
                backend_config=TurbomindEngineConfig(
                    max_batch_size=32,
                    enable_prefix_caching=True,
                    cache_max_entry_count=0.8,
                    session_len=8192,
                ))
response = pipe(prompts=[&quot;Hi, pls intro yourself&quot;, &quot;Shanghai is&quot;],
                gen_config=GenerationConfig(max_new_tokens=1024,
                                            top_p=0.8,
                                            top_k=40,
                                            temperature=0.6))
print(response)

### PytorchEngineConfig推理引擎


from lmdeploy import pipeline, PytorchEngineConfig
from lmdeploy import GenerationConfig

pipe = pipeline(&#x27;/root/share/new_models/InternVL3/InternVL3-1B&#x27;,
                backend_config=PytorchEngineConfig(
                    max_batch_size=32,
                    enable_prefix_caching=True,
                    cache_max_entry_count=0.8,
                    session_len=8192,
                ))
                
response = pipe(prompts=[&quot;Hi, pls intro yourself&quot;, &quot;Shanghai is&quot;],
                gen_config=GenerationConfig(max_new_tokens=1024,
                                            top_p=0.8,
                                            top_k=40,
                                            temperature=0.6))
print(response)
```

![](img/babbcf9e24099826586e7146bc55ef29.png)

1.  VLM 推理(Visual Language Models

```
from lmdeploy import pipeline, VisionConfig
from lmdeploy.vl import load_image

pipe = pipeline(&#x27;/root/share/new_models/InternVL3/InternVL3-1B&#x27;,
                vision_config=VisionConfig(
                    max_batch_size=8
                ))

image = load_image(&#x27;https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg&#x27;)
response = pipe((&#x27;describe this image&#x27;, image))
print(response)
```

## 大语言模型(LLMs)部署

#### 离线部署

```
import lmdeploy
from lmdeploy import GenerationConfig
pipe = lmdeploy.pipeline(&quot;/root/share/new_models/internlm3/internlm3-8b-instruct&quot;)
response = pipe(prompts=[&quot;Hi, pls intro yourself&quot;, &quot;Shanghai is&quot;],
                gen_config=GenerationConfig(max_new_tokens=1024,
                                            top_p=0.8,
                                            top_k=40,
                                            temperature=0.6))
print(response)
```

![](img/48fc4c1d2a0bd14c69fbb4e3f2a252ff.png)

> 注意：

+   LMDeploy的KV cache 策略对显存提出了较高的要求；LMDeploy在服务启动时就会预先一次性分配KV cache空间，如果加载模型出现OOM，可能需要考虑降低 cache_max_entry_count 的值，表示加载模型权重后 K/V 缓存占用的空闲 GPU 内存的比例。

+   v0.2.0 <= lmdeploy <= v0.2.1，默认比例为 0.5，表示 GPU总显存的 50% 被分配给 k/v cache。lmdeploy > v0.2.1，分配策略改为从空闲显存中按比例为 k/v cache 开辟空间。默认比例值调整为 0.8。

#### 设置多卡并行（此部分不需要复现）

> TurbomindEngineConfig和Py**tor**chEngineConfig的参数tp是设置GPU并行的，解释如下：

> tp (int): the number of GPU cards used in tensor parallelism, default to 1

```
from lmdeploy import pipeline, TurbomindEngineConfig
from lmdeploy import pipeline, PytorchEngineConfig

### TurbomindEngineConfig推理引擎
pipe = pipeline(&#x27;/root/share/new_models/internlm3/internlm3-8b-instruct&#x27;,
                backend_config=TurbomindEngineConfig(
                    tp=2,
                    max_batch_size=32,
                    enable_prefix_caching=True,
                    cache_max_entry_count=0.8,
                    session_len=8192,
                ))
### PytorchEngineConfig推理引擎

pipe = pipeline(&#x27;/root/share/new_models/internlm3/internlm3-8b-instruct&#x27;,
                backend_config=PytorchEngineConfig(
                    tp=2,
                    max_batch_size=32,
                    enable_prefix_caching=True,
                    cache_max_entry_count=0.8,
                    session_len=8192,
                ))
```

####  3. 生成核心参数介绍

> 常见解码策略：

+   贪心解码（Greedy Decoding）：直接选择概率最高的单词。这种方法简单高效，但是可能会导致生成的文本过于单调和重复。

+   随机采样（Random Sampling）：按照概率分布随机选择一个单词。这种方法可以增加生成的多样性，但是可能会导致生成的文本不连贯和无意义。

+   集束搜索（Beam Search）：维护一个大小为 k 的候选序列集合，每一步从每个候选序列的概率分布中选择概率最高的 k 个单词，然后保留总概率最高的 k 个候选序列。这种方法可以平衡生成的质量和多样性，但是可能会导致生成的文本过于保守和不自然；由于需要维护k个序列，开销比较大。

>

> 以上方法都有各自的问题，而 top-k 采样和 top-p 采样是介于贪心解码和随机采样之间的方法，也是目前大模型解码策略中常用的方法。

> Top-k: 在每一步，只从概率最高的 k 个单词中进行随机采样，而不考虑其他低概率的单词。

>

> **缺点：**

1.  结果对**K参数**比较敏感，生成的结果更关注高频词，多样性丢失，可能会导致生成的回复过于简单，趋同性高；

1.  回复可能缺乏个性，更偏向“安全”回复

1.  每一步只关注当前概率高的词，可能会导致生成的文本不符合常识或逻辑；

```
import torch
from labml_nn.sampling import Sampler

# Top-k Sampler
class TopKSampler(Sampler):
    # k is the number of tokens to pick
    # sampler is the sampler to use for the top-k tokens
    # sampler can be any sampler that takes a logits tensor as input and returns a token tensor; e.g. `TemperatureSampler`.
    def __init__(self, k: int, sampler: Sampler):
        self.k = k
        self.sampler = sampler

    # Sample from logits
    def __call__(self, logits: torch.Tensor):
        # New logits filled with −∞; i.e. zero probability
        zeros = logits.new_ones(logits.shape) * float(&#x27;-inf&#x27;)
        # Pick the largest k logits and their indices
        values, indices = torch.topk(logits, self.k, dim=-1)
        # Set the values of the top-k selected indices to actual logits.
        # Logits of other tokens remain −∞
        zeros.scatter_(-1, indices, values)
        # Sample from the top-k logits with the specified sampler.
        return self.sampler(zeros)

```

![](img/91f599663d39b218f5bd714619de3ceb.png)

> Top-p: 在每一步，只从**累积概率**超过某个阈值 p 的最小单词集合中进行随机采样，而不考虑其它低概率的单词。这种方法也被称为核采样（nucleus sampling），因为它只关注概率分布的核心部分，而忽略了尾部部分。

>

> **缺点：**

1.  参数敏感性，p值调优比较难

1.  Top-p 仅依赖概率分布筛选候选词，但缺乏对已生成内容的显式约束（如重复惩罚机制），可能导致重复片段，倾向于重复高频词

1.  生成结果偏向“安全”选项，牺牲个性。

>

![](img/b4fdf2437626881faf98ba61c1d2b7d4.png)

```
import torch
from torch import nn

from labml_nn.sampling import Sampler


class NucleusSampler(Sampler):
    &quot;&quot;&quot;
    ## Nucleus Sampler
    &quot;&quot;&quot;
    def __init__(self, p: float, sampler: Sampler):
        &quot;&quot;&quot;
        :param p: is the sum of probabilities of tokens to pick $p$
        :param sampler: is the sampler to use for the selected tokens
        &quot;&quot;&quot;
        self.p = p
        self.sampler = sampler
        # Softmax to compute $P(x_i | x_{1:i-1})$ from the logits
        self.softmax = nn.Softmax(dim=-1)

    def __call__(self, logits: torch.Tensor):
        &quot;&quot;&quot;
        Sample from logits with Nucleus Sampling
        &quot;&quot;&quot;

        # Get probabilities $P(x_i | x_{1:i-1})$
        probs = self.softmax(logits)

        # Sort probabilities in descending order
        sorted_probs, indices = torch.sort(probs, dim=-1, descending=True)

        # Get the cumulative sum of probabilities in the sorted order
        cum_sum_probs = torch.cumsum(sorted_probs, dim=-1)

        # Find the cumulative sums less than $p$.
        nucleus = cum_sum_probs &lt; self.p

        # Prepend ones so that we add one token after the minimum number
        # of tokens with cumulative probability less that $p$.
        nucleus = torch.cat([nucleus.new_ones(nucleus.shape[:-1] + (1,)), nucleus[..., :-1]], dim=-1)

        # Get log probabilities and mask out the non-nucleus
        sorted_log_probs = torch.log(sorted_probs)
        sorted_log_probs[~nucleus] = float(&#x27;-inf&#x27;)

        # Sample from the sampler
        sampled_sorted_indexes = self.sampler(sorted_log_probs)

        # Get the actual indexes
        res = indices.gather(-1, sampled_sorted_indexes.unsqueeze(-1))

        #
        return res.squeeze(-1)

```

> 如果 top-k 和top-p 同时启用，则top-p在 top-k之后起作用。

> Temperature采样: 调节输出概率分布的参数

> 高**temperature对差异进行平滑**，低temperature对差异进行放大

设模型的原始输出 logits（未归一化的预测值）为  , 词汇表大小为   ，温度参数为

控制分布的尖锐程度, 接近0，概率分布趋向于贪婪选择（仅最高概率的 token 概率接近 1，其余接近 0）

趋近无穷大，概率分布趋向于均匀分布（所有 token 概率接近相等）

![](img/8d6b5d201c60aa55eaf9d4df8d59d279.png)

```
import torch
from torch.distributions import Categorical

from labml_nn.sampling import Sampler


class TemperatureSampler(Sampler):
    &quot;&quot;&quot;
    ## Sampler with Temperature
    &quot;&quot;&quot;
    def __init__(self, temperature: float = 1.0):
        &quot;&quot;&quot;
        :param temperature: is the temperature to sample with
        &quot;&quot;&quot;
        self.temperature = temperature

    def __call__(self, logits: torch.Tensor):
        &quot;&quot;&quot;
        Sample from logits
        &quot;&quot;&quot;

        # Create a categorical distribution with temperature adjusted logits
        dist = Categorical(logits=logits / self.temperature)

        # Sample
        return dist.sample()

```

#### 部署类OpenAI 服务

+   使用 lmdeploy cli 工具

```
lmdeploy serve api_server /root/share/new_models/InternVL3/InternVL3-1B --server-port 23333
```

![](img/7d80ec9f3bb6c80c5e753f55318a8e77.png)

+   使用类openai方式

(i) 不启用权限鉴别(api-key)

```
from openai import OpenAI
client = OpenAI(
    api_key=&#x27;none&#x27;,# 若未启用鉴权，可填任意值（如 &quot;none&quot;）
    base_url=&quot;http://0.0.0.0:23333/v1&quot;
)
model_name = client.models.list().data[0].id
response = client.chat.completions.create(
  model=model_name,
  messages=[
    {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a helpful assistant.&quot;},
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot; provide three suggestions about time management&quot;},
  ],
    temperature=0.8,
    top_p=0.8
)
print(response)
```

![](img/17852faf36e278ac398c3d8be7de3d7c.png)

(ii) 启用权限鉴别(api-key)

```
1. 启动服务

lmdeploy serve api_server /root/share/new_models/internlm3/internlm3-8b-instruct --server-port 23333 --api-keys &quot;token&quot;

2. 授权访问

from openai import OpenAI
client = OpenAI(
    api_key=&#x27;token&#x27;,
    base_url=&quot;http://0.0.0.0:23333/v1&quot;
)
model_name = client.models.list().data[0].id
response = client.chat.completions.create(
  model=model_name,
  messages=[
    {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a helpful assistant.&quot;},
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot; provide three suggestions about time management&quot;},
  ],
    temperature=0.8,
    top_p=0.8
)
print(response)
```

![](img/885c0f48c751da62c4790f34b95993d6.png)

(iii) 异步访问(api-key)

```
import asyncio
from openai import AsyncOpenAI

async def main():
    client = AsyncOpenAI(api_key=&#x27;token&#x27;,
                         base_url=&#x27;http://0.0.0.0:23333/v1&#x27;)
    model_cards = await client.models.list()._get_page()
    response = await client.chat.completions.create(
        model=model_cards.data[0].id,
        messages=[
            {
                &#x27;role&#x27;: &#x27;system&#x27;,
                &#x27;content&#x27;: &#x27;You are a helpful assistant.&#x27;
            },
            {
                &#x27;role&#x27;: &#x27;user&#x27;,
                &#x27;content&#x27;: &#x27; provide three suggestions about time management&#x27;
            },
        ],
        temperature=0.8,
        top_p=0.8)
    print(response)

asyncio.run(main())
```

![](img/64a87c45d9f30cbe94362b8da5222fc2.png)

#### APIClient 接口

1.   启动服务

```
lmdeploy serve api_server /root/share/new_models/internlm3/internlm3-8b-instruct --server-port 23333 --api-keys &quot;token&quot;
```

1.  使用APIClient进行推理

```
from lmdeploy.serve.openai.api_client import APIClient

client = APIClient(
    api_server_url=&quot;http://localhost:23333&quot;,
    api_key=&quot;token&quot;
)
model_name = client.available_models[0]

messages = [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好&quot;}]
for item in client.chat_completions_v1(model=model_name, messages=messages):
    print(item)

```

![](img/438acb27bd79181c4f0dea300ec1aa45.png)

#### Requests调用（了解）

```
1. 启动服务

lmdeploy serve api_server /root/share/new_models/internlm3/internlm3-8b-instruct --server-port 23333 --api-keys &quot;token&quot;

2. request发送请求

import requests

url = &quot;http://localhost:23333/v1/chat/completions&quot;
headers = {
    &quot;Authorization&quot;: &quot;Bearer token&quot;,  # Bearer + Token
    &quot;Content-Type&quot;: &quot;application/json&quot;
}
data = {
    &quot;model&quot;: &quot;/root/share/new_models/internlm3/internlm3-8b-instruct&quot;,
    &quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好&quot;}]
}

response = requests.post(url, headers=headers, json=data)
print(response.json()[&quot;choices&quot;][0][&quot;message&quot;][&quot;content&quot;])
```

![](img/d5abc88e9d7638b665fd2eaec25ea3a0.png)

#### 使用CURL命令(了解）

![](img/2d0df95d3f95ea1c56e4c67de8a7c019.png)

## 视觉-语言模型(VLMs)部署

> 部署思路和文本模型相似

#### 离线部署

```
from lmdeploy import pipeline
from lmdeploy.vl import load_image

pipe = pipeline(&#x27;/root/share/new_models/InternVL3/InternVL3-1B&#x27;)

image = load_image(&#x27;https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg&#x27;)

response = pipe((&#x27;describe this image&#x27;, image))

print(response)
```

![](img/32631d0720af60c1a6dd6309f1a7da72.png)

#### 多卡并行(了解即可）

```
from lmdeploy import pipeline, TurbomindEngineConfig,PytorchEngineConfig
from lmdeploy.vl import load_image

pipe = pipeline(&#x27;root/share/new_models/InternVL3/InternVL3-1B&#x27;,
                backend_config=PytorchEngineConfig(tp=2))

image = load_image(&#x27;tiger.jpeg&#x27;)
response = pipe((&#x27;describe this image&#x27;, image))
print(response)
```

#### 图片 token

```
IMAGE_TOKEN = &#x27;&lt;IMAGE_TOKEN&gt;&#x27;
```

```
from lmdeploy import pipeline, VisionConfig
from lmdeploy.vl import load_image
from lmdeploy.vl.constants import IMAGE_TOKEN

pipe = pipeline(&#x27;/root/share/new_models/InternVL3/InternVL3-1B&#x27;,
                vision_config=VisionConfig(
                    max_batch_size=8
                ))

image = load_image(&#x27;https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg&#x27;)
response = pipe((f&#x27;describe this image{IMAGE_TOKEN}&#x27;, image))
print(response)

```

> IMAGE_TOKEN是一个特殊的token，在文本序列中插入 IMAGE_TOKEN 可以明确标注图像的位置，帮助模型区分文本和图像的边界。

#### 对话模板

> 对话模板（Conversation Template）是一种预设的输入输出格式规则，用于规范用户与模型的交互方式。它的核心作用是通过结构化对话内容，提升模型的响应质量、可控性和适用性。

>

> 推理时，LMDeploy 会根据模型路径匹配内置的对话模板，并把对话模板应用到输入的提示词上。如果用户使用的是本地模型，并且模型文件夹名字与官方模型不一致时，需要手动指定对话模版。

**自定义对话模板**

```
from lmdeploy.model import MODELS, BaseChatTemplate


@MODELS.register_module(name=&#x27;customized_model&#x27;)
class CustomizedModel(BaseChatTemplate):
    &quot;&quot;&quot;A customized chat template.&quot;&quot;&quot;

    def __init__(self,
                 system=&#x27;&lt;|im_start|&gt;system\n&#x27;,
                 meta_instruction=&#x27;You are a robot developed by LMDeploy.&#x27;,
                 user=&#x27;&lt;|im_start|&gt;user\n&#x27;,
                 assistant=&#x27;&lt;|im_start|&gt;assistant\n&#x27;,
                 eosys=&#x27;&lt;|im_end|&gt;\n&#x27;,
                 eoh=&#x27;&lt;|im_end|&gt;\n&#x27;,
                 eoa=&#x27;&lt;|im_end|&gt;&#x27;,
                 separator=&#x27;\n&#x27;,
                 stop_words=[&#x27;&lt;|im_end|&gt;&#x27;, &#x27;&lt;|action_end|&gt;&#x27;]):
        super().__init__(system=system,
                         meta_instruction=meta_instruction,
                         eosys=eosys,
                         user=user,
                         eoh=eoh,
                         assistant=assistant,
                         eoa=eoa,
                         separator=separator,
                         stop_words=stop_words)


from lmdeploy import ChatTemplateConfig, pipeline

messages = [{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;who are you?&#x27;}]
pipe = pipeline(&#x27;/root/share/new_models/InternVL3/InternVL3-1B&#x27;,
                chat_template_config=ChatTemplateConfig(&#x27;customized_model&#x27;))
for response in pipe.stream_infer(messages):
    print(response.text, end=&#x27;&#x27;)
```

#### 视觉模型参数

> 可通过设置 VisionConfig 修改视觉模型默认参数

```
from lmdeploy import pipeline, VisionConfig
from lmdeploy.vl import load_image
vision_config=VisionConfig(max_batch_size=16)
pipe = pipeline(&#x27;/root/share/new_models/InternVL3/InternVL3-1B&#x27;, vision_config=vision_config)
image = load_image(&#x27;https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg&#x27;)
response = pipe((&#x27;describe this image&#x27;, image))
print(response)


###VisionConfig 源码

@dataclass
class VisionConfig:
    &quot;&quot;&quot;Vison model configs.

    Args:
        max_batch_size (int): the max image size passed to the model, since
            some models will use image patch, the actual running batch could
            be larger than this value.
        thread_safe (bool): Specifies whether the engine instance is
            thread-safe. Please set it to True when using the pipeline
            in a multi-threaded environment.
    &quot;&quot;&quot;
    max_batch_size: int = 1
    thread_safe: bool = False
```

#### 多图推理

```
from lmdeploy import pipeline, TurbomindEngineConfig
from lmdeploy.vl import load_image

pipe = pipeline(&#x27;/root/share/new_models/InternVL3/InternVL3-1B&#x27;,
                backend_config=TurbomindEngineConfig(session_len=8192))

image_urls=[
    &#x27;https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg&#x27;,
    &#x27;https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg&#x27;
]

images = [load_image(img_url) for img_url in image_urls]
response = pipe((&#x27;describe the two images&#x27;, images))
print(response)
```

#### 多轮对话

> 使用pipeline 进行多轮对话有两种方式，一种是按照 openai 的格式来构造 messages，另外一种是使用 pipeline.chat 接口。

```
from lmdeploy import pipeline, TurbomindEngineConfig, GenerationConfig
from lmdeploy.vl import load_image

pipe = pipeline(&#x27;/root/share/new_models/InternVL3/InternVL3-1B&#x27;,  # 注意加了 `/`
                backend_config=TurbomindEngineConfig(session_len=8192))

image = load_image(&#x27;https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg&#x27;)
gen_config = GenerationConfig(top_k=40, top_p=0.8, temperature=0.6)

# 第一句图文对话
sess = pipe.chat((&#x27;describe this image&#x27;, image), gen_config=gen_config)
print(sess.response.text)

# 第二轮多轮对话
sess = pipe.chat(&#x27;What is the woman doing?&#x27;, session=sess, gen_config=gen_config)
print(sess.response.text)
```

> 视觉语言模型的部署和文本几乎相似，其余请参考文本模型的部署

## 模型量化

#### 量化模型

> LMDeploy 量化模块目前仅支持 AWQ 量化算法。

> 量化是指将高精度数字转换为低精度数字。低精度实体可以存储在磁盘上很小的空间内，从而减少内存需求。

![](img/57cc3c002eb619fede0be4ee6c53f8b0.png)

```
conda activate lmdeploy
pip uninstall -y datasets &amp;&amp; pip install --no-cache-dir &quot;datasets==2.19.2&quot; 
```

量化internlm2-chat-7b的命令：

```
export HF_MODEL=/root/share/model_repos/internlm2-chat-7b
export WORK_DIR=/root/internlm2-chat-7b-4bit

lmdeploy lite auto_awq $HF_MODEL   --calib-dataset &#x27;ptb&#x27;   --calib-samples 128   --calib-seqlen 2048   --w-bits 4   --w-group-size 128   --batch-size 1   --work-dir $WORK_DIR
```

量化InternVL3-1B的命令：

```
lmdeploy lite auto_awq /root/share/new_models/InternVL3/InternVL3-1B \
  --work-dir /root/InternVL3-1B-4bit
```

#### W8A8量化

LMDeploy 提供了使用 8-bit 整数(INT8)和浮点数(FP8)对神经网络模型进行量化和推理的功能。

```
pip install lmdeploy[all]
```

lmdeploy 提供了命令行工具 lmdeploy lite smooth_quant 实现量化功能。并且其中命令行参数 --quant-dtype 可以用来控制是进行8-bit整数还是浮点数类型的量化。更多命令行工具使用方式，请执行 lmdeploy lite smooth_quant --help 查看。

```
export HF_MODEL=/root/share/model_repos/internlm2-chat-7b
export WORK_DIR=/root/internlm2-chat-7b-4bit

lmdeploy lite smooth_quant \
  $HF_MODEL \
  --work-dir $WORK_DIR \
  --quant-dtype int8
```

```
lmdeploy lite smooth_quant $HF_MODEL --work-dir $WORK_DIR --quant-dtype fp8
```

#### Key-Value(KV) Cache 量化

**自 **v0.4.0 起，LMDeploy 支持在线 kv cache int4/int8 量化，量化方式为 per-head per-token 的非对称量化。

**通过 LMDeploy **应用 kv 量化非常简单，只需要设定 quant_policy 参数即可。

```
from lmdeploy import pipeline, TurbomindEngineConfig
engine_config = TurbomindEngineConfig(quant_policy=8)
pipe = pipeline(&quot;/root/share/model_repos/internlm2-chat-7b&quot;, 
            backend_config=engine_config)
response = pipe([&quot;Hi, pls intro yourself&quot;, &quot;Shanghai is&quot;])
print(response)
```

**LMDeploy 规定 qant_po**licy=4 表示 kv int4 量化

quant_policy=8 表示 kv int8 量化。

## 闯关任务

复现 InternVL 和 InternLM 的部署和量化提交为在线飞书文档。

提交地址：https://aicarrier.feishu.cn/share/base/form/shrcnkSJ97ykqRSz4fgyCJbvCsb