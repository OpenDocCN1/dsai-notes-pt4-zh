
```python
import torch
from torch import nn

from labml_nn.sampling import Sampler


class NucleusSampler(Sampler):
    """
    ## Nucleus Sampler
    """
    def __init__(self, p: float, sampler: Sampler):
        """
        :param p: is the sum of probabilities of tokens to pick $p$
        :param sampler: is the sampler to use for the selected tokens
        """
        self.p = p
        self.sampler = sampler
        # Softmax to compute $P(x_i | x_{1:i-1})$ from the logits
        self.softmax = nn.Softmax(dim=-1)

    def __call__(self, logits: torch.Tensor):
        """
        Sample from logits with Nucleus Sampling
        """

        # Get probabilities $P(x_i | x_{1:i-1})$
        probs = self.softmax(logits)

        # Sort probabilities in descending order
        sorted_probs, indices = torch.sort(probs, dim=-1, descending=True)

        # Get the cumulative sum of probabilities in the sorted order
        cum_sum_probs = torch.cumsum(sorted_probs, dim=-1)

        # Find the cumulative sums less than $p$.
        nucleus = cum_sum_probs < self.p

        # Prepend ones so that we add one token after the minimum number
        # of tokens with cumulative probability less that $p$.
        nucleus = torch.cat([nucleus.new_ones(nucleus.shape[:-1] + (1,)), nucleus[..., :-1]], dim=-1)

        # Get log probabilities and mask out the non-nucleus
        sorted_log_probs = torch.log(sorted_probs)
        sorted_log_probs[~nucleus] = float('-inf')

        # Sample from the sampler
        sampled_sorted_indexes = self.sampler(sorted_log_probs)

        # Get the actual indexes
        res = indices.gather(-1, sampled_sorted_indexes.unsqueeze(-1))

        #
        return res.squeeze(-1)
```

> 如果 `top-k` 和 `top-p` 同时启用，则 `top-p` 在 `top-k` 之后起作用。

> `Temperature` 采样：调节输出概率分布的参数

高 `temperature` 对差异进行平滑，低 `temperature` 对差异进行放大

设模型的原始输出 `logits`（未归一化的预测值）为 ，词汇表大小为 ，温度参数为

控制分布的尖锐程度，接近 0，概率分布趋向于贪婪选择（仅最高概率的 `token` 概率接近 1，其余接近 0）

趋近无穷大，概率分布趋向于均匀分布（所有 `token` 概率接近相等）

![](img/8d6b5d201c60aa55eaf9d4df8d59d279.png)

```python
import torch
from torch.distributions import Categorical

from labml_nn.sampling import Sampler


class TemperatureSampler(Sampler):
    """
    ## Sampler with Temperature
    """
    def __init__(self, temperature: float = 1.0):
        """
        :param temperature: is the temperature to sample with
        """
        self.temperature = temperature

    def __call__(self, logits: torch.Tensor):
        """
        Sample from logits
        """

        # Create a categorical distribution with temperature adjusted logits
        dist = Categorical(logits=logits / self.temperature)

        # Sample
        return dist.sample()
```

### 部署类 `OpenAI` 服务

+ 使用 `lmdeploy` CLI 工具

```bash
lmdeploy serve api_server /root/share/new_models/InternVL3/InternVL3-1B --server-port 23333
```

![](img/7d80ec9f3bb6c80c5e753f55318a8e77.png)

+ 使用类 `OpenAI` 方式

(i) 不启用权限鉴别 (`api-key`)

```python
from openai import OpenAI
client = OpenAI(
    api_key='none', # 若未启用鉴权，可填任意值（如 "none"）
    base_url="http://0.0.0.0:23333/v1"
)
model_name = client.models.list().data[0].id
response = client.chat.completions.create(
  model=model_name,
  messages=[
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": " provide three suggestions about time management"},
  ],
    temperature=0.8,
    top_p=0.8
)
print(response)
```

![](img/17852faf36e278ac398c3d8be7de3d7c.png)

(ii) 启用权限鉴别 (`api-key`)

```
1. 启动服务

lmdeploy serve api_server /root/share/new_models/internlm3/internlm3-8b-instruct --server-port 23333 --api-keys "token"

2. 授权访问

from openai import OpenAI
client = OpenAI(
    api_key='token',
    base_url="http://0.0.0.0:23333/v1"
)
model_name = client.models.list().data[0].id
response = client.chat.completions.create(
  model=model_name,
  messages=[
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": " provide three suggestions about time management"},
  ],
    temperature=0.8,
    top_p=0.8
)
print(response)
```

![](img/885c0f48c751da62c4790f34b95993d6.png)

(iii) 异步访问 (`api-key`)

```python
import asyncio
from openai import AsyncOpenAI

async def main():
    client = AsyncOpenAI(api_key='token',
                         base_url='http://0.0.0.0:23333/v1')
    model_cards = await client.models.list()._get_page()
    response = await client.chat.completions.create(
        model=model_cards.data[0].id,
        messages=[
            {
                'role': 'system',
                'content': 'You are a helpful assistant.'
            },
            {
                'role': 'user',
                'content': ' provide three suggestions about time management'
            },
        ],
        temperature=0.8,
        top_p=0.8)
    print(response)

asyncio.run(main())
```

![](img/64a87c45d9f30cbe94362b8da5222fc2.png)

### `APIClient` 接口

1. 启动服务

```bash
lmdeploy serve api_server /root/share/new_models/internlm3/internlm3-8b-instruct --server-port 23333 --api-keys "token"
```

1. 使用 `APIClient` 进行推理

```python
from lmdeploy.serve.openai.api_client import APIClient

client = APIClient(
    api_server_url="http://localhost:23333",
    api_key="token"
)
model_name = client.available_models[0]

messages = [{"role": "user", "content": "你好"}]
for item in client.chat_completions_v1(model=model_name, messages=messages):
    print(item)
```

![](img/438acb27bd79181c4f0dea300ec1aa45.png)

### `Requests` 调用（了解）

1. 启动服务

```bash
lmdeploy serve api_server /root/share/new_models/internlm3/internlm3-8b-instruct --server-port 23333 --api-keys "token"
```

2. `requests` 发送请求

```python
import requests

url = "http://localhost:23333/v1/chat/completions"
headers = {
    "Authorization": "Bearer token",  # Bearer + Token
    "Content-Type": "application/json"
}
data = {
    "model": "/root/share/new_models/internlm3/internlm3-8b-instruct",
    "messages": [{"role": "user", "content": "你好"}]
}

response = requests.post(url, headers=headers, json=data)
print(response.json()["choices"][0]["message"]["content"])
```

![](img/d5abc88e9d7638b665fd2eaec25ea3a0.png)

### 使用 `CURL` 命令（了解）

![](img/2d0df95d3f95ea1c56e4c67de8a7c019.png)

## 视觉-语言模型 (VLMs) 部署

> 部署思路和文本模型相似

### 离线部署

```python
image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg')
```

```python
from lmdeploy import pipeline
from lmdeploy.vl import load_image

pipe = pipeline('/root/share/new_models/InternVL3/InternVL3-1B')
response = pipe(('describe this image', image))

print(response)
```

![](img/32631d0720af60c1a6dd6309f1a7da72.png)

### 多卡并行（了解即可）

```python
from lmdeploy import pipeline, TurbomindEngineConfig, PytorchEngineConfig
from lmdeploy.vl import load_image

pipe = pipeline('root/share/new_models/InternVL3/InternVL3-1B',
                backend_config=PytorchEngineConfig(tp=2))

image = load_image('tiger.jpeg')
response = pipe(('describe this image', image))
print(response)
```

### 图片 token

```python
IMAGE_TOKEN = '<IMAGE_TOKEN>'
```

```python
image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg')
```
