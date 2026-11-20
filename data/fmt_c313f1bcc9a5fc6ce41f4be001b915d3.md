
# 浦语工具链玩转 Llama 3.1

> 来源：[https://aicarrier.feishu.cn/docx/FPJZduu9CohNLSxgWF0cUsH1nbU](https://aicarrier.feishu.cn/docx/FPJZduu9CohNLSxgWF0cUsH1nbU)

![](img/20f77c2a33c33ad06c5587bd8878278a.png)

标题：全网首发，Llama 3.1 微调部署及 `Function Call`

前言：Meta 重磅发布 Llama 3.1，分别为 8B、70B 和 405B 三个尺寸的模型～原生支持 `Function Calling` 功能。

128K 上下文、非 `MoE` 模型、卷合成数据、原生支持 `Function Call`、官方支持多语言不包含中文、纯语言模型。

Llama 3.1 开源发布了，虽然模型提前被泄露了，但 Meta 正式发布并提供细节后还是发现一些有意思的点。

![](img/9a9a3d6b09751449f90dfe793ea4d0cc.png)

官网：https://ai.meta.com/blog/meta-llama-3-1/

论文地址：https://ai.meta.com/research/publications/the-llama-3-herd-of-models/

## 模型下载

首先申请权限

```python
import os
import sys

repo = sys.argv[1]
os.environ['HF_ENDPOINT'] = 'https://hf-mirror.com'
from huggingface_hub import snapshot_download
token = 'YOUR_TOKEN'
snapshot_download(repo_id=repo, local_dir=f'/share/new_models/{repo}', local_dir_use_symlinks=False, max_workers=1, resume_download=True, token=token, ignore_patterns=["*consolidated*"])
```

## LMDeploy 量化部署

https://github.com/internLM/lmdeploy

欢迎 Star

### LMDeploy 实践教程

#### 环境配置

```bash
conda create -n llama3 python=3.10
conda activate llama3
conda install pytorch==2.1.2 torchvision==0.16.2 torchaudio==2.1.2 pytorch-cuda=12.1 -c pytorch -c nvidia
conda install git
git-lfs install
```

安装 `LMDeploy` 与其他工具

```bash
cd ~
pip install lmdeploy
pip install triton>=2.1.0
```

#### LMDeploy 量化

`LMDeploy` 支持一键量化和部署，只需要一行命令即可将模型进行量化。`LMDeploy` 支持两种量化方式，`INT4` 量化和 `W8A8` 量化，经量化后的模型可以直接进行使用。

1. `INT4` 量化

```bash
lmdeploy lite auto_awq /root/share/new_models/meta-llama/Meta-Llama-3.1-8B-Instruct --work-dir ./Meta-Llama-3.1-8B-Instruct-4bit
```

> 注：经 `INT4` 量化后在推理时加入 `--model-format awq`，如：
>
> `lmdeploy chat ./Meta-Llama-3.1-8B-Instruct-4bit --model-format awq --model-name llama3`

1. `W8A8` 量化

```bash
lmdeploy lite smooth_quant /root/share/new_models/meta-llama/Meta-Llama-3.1-8B-Instruct --work-dir ./Meta-Llama-3.1-8B-Instruct-w8
```

#### LMDeploy 推理

在本节我们将使用 `LMDeploy` 进行模型推理。`LMDeploy` 支持多种推理方式，从终端 demo 到 API 部署都能一键部署，十分方便快捷。

首先我们需要进入模型目录下的 `config.json`，然后将 `rope_scaling` 参数设为 `null`:

![](img/a0b5771c871c5bde709183aa6b01b323.png)

##### 命令行推理

执行以下命令开启推理服务(需要将模型目录改为下载的路径)：

```bash
lmdeploy chat ./Meta-Llama-3.1-8B-Instruct --model-name llama3
```

之后我们就能在终端与模型对话，比如我们可以让 `Llama3.1` 写一个快速排序的代码：

![](img/63d9a59b42fceec630df53837d9cf96d.png)

在此基础上可以添加其他参数以满足更多对话需求：

```bash
lmdeploy chat ./Meta-Llama-3.1-8B-Instruct \
--meta-instruct 'You are an assistant' \  # 设置system rompt
--cache-max-entry-count 0.1 # 申请 k/v cache 比例
--quant-policy {0,4,8} # 是否量化KV. 0: no quant; 4: 4bit kv; 8: 8bit kv. Default: 0. Type: int
```

##### 使用 `pipeline` 进行推理

`lmdeploy` 提供 `python` 函数接口 `pipeline`，使用 `pipeline` 可以集成到 `python` 项目中。以下是使用 `OpenAI` 格式的输入代码：

```python
from lmdeploy import pipeline, GenerationConfig, TurbomindEngineConfig, ChatTemplateConfig

backend_config = TurbomindEngineConfig(cache_max_entry_count=0.2)  # 推理引擎 config 设置
gen_config = GenerationConfig(top_p=0.8,
                              top_k=40,
                              temperature=0.8,
                              max_new_tokens=1024)  # 生成超参数 config 设置
pipe = pipeline('./Meta-Llama-3.1-8B-Instruct',
                # model_name='llama3',
                chat_template_config=ChatTemplateConfig(model_name='llama3',
                                                        meta_instruction='You are an useful assistant,your name is chatbot.'
                                                        ),
                backend_config=backend_config)

prompts = [[{
    'role': 'user',
    'content': 'Hi, pls intro yourself'
}], [{
    'role': 'user',
    'content': 'please write a small story'
}]]
response = pipe(prompts,
                gen_config=gen_config)
print(response)
```

将以上代码封装为 `pipeline.py`，然后在终端执行以下命令：

```bash
python pipeline.py
```

得到如下的 `response`，可在此基础上进行进一步开发:

![](img/94ad984df619efc3c2f5619545982956.png)

同时 `pipeline` 支持模型的流式推理，只需使用以下代码即可：

```python
from lmdeploy import pipeline, GenerationConfig, TurbomindEngineConfig, ChatTemplateConfig

backend_config = TurbomindEngineConfig(cache_max_entry_count=0.2)  # 推理引擎 config 设置
gen_config = GenerationConfig(top_p=0.8,
                              top_k=40,
                              temperature=0.8,
                              max_new_tokens=1024)  # 生成超参数 config 设置
pipe = pipeline('./Meta-Llama-3.1-8B-Instruct',
                # model_name='llama3',
                chat_template_config=ChatTemplateConfig(model_name='llama3',
                                                        meta_instruction='You are an useful assistant,your name is chatbot.'
                                                        ),
                backend_config=backend_config)

prompts = [[{
    'role': 'user',
    'content': 'Hi, pls intro yourself'
}], [{
    'role': 'user',
    'content': 'please write a small story'
}]]
for item in pipe.stream_infer(prompts, gen_config=gen_config):
    print(item)
```

##### 使用 `gradio` 界面进行推理

`LMDeploy` 提供了封装好的友好界面推理模式，只需要使用以下命令

```bash
lmdeploy serve gradio ./Meta-Llama-3.1-8B-Instruct --model-name llama3
```

接下来即可在 `http://0.0.0.0:6006` 访问到界面进行模型推理：

![](img/4c52369e29feffd5a1a64a96ece4dffc.png)

##### 使用 `api_server` 进行推理

`LMDeploy` 支持使用 `API` 进行部署，以方便地进行模型访问和各类应用场景的部署支持。

首先启动模型 `API` 服务

```bash
lmdeploy serve api_server ./Meta-Llama-3.1-8B-Instruct --model-name llama3 --server-port 23333
```

`api_server` 启动时的参数可以通过命令行 `lmdeploy serve api_server -h` 查看。比如，`--tp` 设置张量并行，`--session-len` 设置推理的最大上下文窗口长度，`--cache-max-entry-count` 调整 `k/v cache` 的内存使用比例等等。

接下来就可以利用 `openai` 接口进行 `api` 访问然后进行模型推理，`LMDeploy` 的 `RESTful API` 兼容了 `OpenAI` 以下 3 个接口：1、`/v1/chat/completions`；2、`/v1/models`；3、`/v1/completions`。这里以 `/v1/chat/completions` 为例：

```python
from openai import OpenAI
client = OpenAI(
    api_key='YOUR_API_KEY',
    base_url="http://0.0.0.0:23333/v1"
)
model_name = client.models.list().data[0].id
response = client.chat.completions.create(
  model=model_name,
  messages=[{"role": "user", "content": "Who are you"}
  ],
    temperature=0.8,
    top_p=0.8
)
print(response.choices[0].message.content)
```

![](img/ec520fe0bf492fb516851ab3f5fdb0e0.png)

## Web Demo 部署

## XTuner 微调

https://github.com/internLM/xtuner

欢迎 Star

## Lagent

## 其他
