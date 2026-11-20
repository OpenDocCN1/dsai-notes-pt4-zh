
# Intern-S1-mini 部署实践 （SGLang 版）

> 来源：[https://aicarrier.feishu.cn/docx/Kvrhd0rFfod5ETxW6EScSsn9nig](https://aicarrier.feishu.cn/docx/Kvrhd0rFfod5ETxW6EScSsn9nig)

本文将带你快速上手 InternLM/Intern-S1-mini 在书生开发机上的用SGLang进行部署与调用。

![](img/ed28b56697ede22a18254a924685156b.png)

继 7 月 26 日开源『书生』科学多模态大模型 Intern-S1 之后，上海人工智能实验室（上海AI实验室）今天推出了轻量化版本 Intern-S1-mini。

凭借领先的通用与专业科学能力，Intern-S1 上线后连续多日霸榜 HuggingFace 多模态 Trending 全球第一，并在开源社区引发了广泛关注。作为 8B 参数的“迷你模型”，Intern-S1-mini 同样兼具通用能力与专业科学能力，且更加适合快速部署和二次开发。Intern-S1-mini 性能速览：

+ 通用能力稳居同量级第一梯队：在 MMLU-Pro、AIME2025、MMMU 等多项权威基准上表现卓越，展现出兼具稳定性与竞争力的综合实力；
+ 科学专业能力优异：在化学、材料等任务表现尤为突出，在 SmolInstruct、ChemBench、MatBench 等基准测试中显著领先；在物理、地球、生物等学科任务中也保持第一梯队水平，体现出扎实的科学理解与跨领域泛化能力；
+ 减身材不减实力，助力科研、开发、教育多种场景。

![](img/283949ee6c39e3a46a024b3877eacdb2.png)

Intern-S1-mini 体验页面：

https://chat.intern-ai.org.cn/

GitHub 链接：

https://github.com/InternLM/Intern-S1

HuggingFace 链接：

https://huggingface.co/internlm/Intern-S1-mini

ModelScope 链接：

https://modelscope.cn/models/Shanghai_AI_Laboratory/Intern-S1-mini

欢迎大家点赞、下载来玩👏

## 开发环境配置

在创建开发机界面选择镜像为`Cuda11.7-conda`，并选择 GPU 为30% A100 or 50% A100。

![](img/8e31283be5b4d0d12e36f9e0a9e3fa8c.png)

```
conda create -n sglang python=3.10 -y
conda activate sglang
```

```
uv pip install "sglang[all]>=0.5.2rc1" -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## 开始推理

```
python3 -m sglang.launch_server \
  --model-path /root/share/new_models/Intern-S1-mini \
  --trust-remote-code \
  --grammar-backend none \
  --attention-backend triton \
  --port 30001 \
  --max-total-tokens 2048 \
  --max-running-requests 4 \
  --chunked-prefill-size 512 \
  --disable-radix-cache \
  --cpu-offload-gb 4
```

```
import openai
client = openai.Client(
    base_url="http://127.0.0.1:30001/v1", api_key="EMPTY")

# Chat completion
response = client.chat.completions.create(
    model="default",
    messages=[
        {"role": "system", "content": "You are a helpful AI assistant"},
        {"role": "user", "content": "List 3 countries and their capitals."},
    ],
    temperature=0,
    max_tokens=64,
)
print(response)
```

![](img/9491add9845e3db893b178f8ca650d8a.png)

![](img/cbecb99072f58e6403cb80fdd8109b49.png)
