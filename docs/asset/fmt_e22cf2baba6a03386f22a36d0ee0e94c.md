
## 官网样例

```
import os
import torch
from transformers import AutoTokenizer, AutoModel

model_path = "/mnt/workspace/chatglm2/models/chatglm2-6b"
tokenizer = AutoTokenizer.from_pretrained(model_path, trust_remote_code=True)
model = AutoModel.from_pretrained(model_path, trust_remote_code=True).half().cuda()
model = model.eval()
response, history = model.chat(tokenizer, "你好", history=[])
print(response)
```

你好👋!我是人工智能助手 ChatGLM-6B,很高兴见到你,欢迎问我任何问题。

### 2.4.4.2 命令行

```
python cli_demo.py # 在命令提示符中运行
```

### 2.4.4.3 运行 web 演示

在运行之前，需要先修改 `web_demo.py` 文件中的模型地址，具体为第 5 行与第 6 行，具体如下：

![](img/131e927f716b0842ea4c7ae97d3d03fc.png)

从清华镜像加载

```
# web_demo.py
# 1. 新增 mirror='https://mirrors.tuna.tsinghua.edu.cn/hugging-face-models', 下载模型使用清华源
tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True, mirror='https://mirrors.tuna.tsinghua.edu.cn/hugging-face-models')
model = AutoModel.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True, mirror='https://mirrors.tuna.tsinghua.edu.cn/hugging-face-models').half().quantize(4).cuda()
# 2. 增加 server_name 和 server_port 参数
demo.queue().launch(share=True, server_name="0.0.0.0", server_port=7860)
```

从本地加载

```
# 更改一下脚本里面的模型加载路径，改为从本地加载（上面通过 git 在 huggingface 上已经做了下载），hugging face 有时候网络环境不太好
# 修改 web_demo.py 文件
tokenizer = AutoTokenizer.from_pretrained("/mnt/workspace/chatglm2/models/chatglm2-6b", trust_remote_code=True)
model = AutoModel.from_pretrained("/mnt/workspace/chatglm2/models/chatglm2-6b", trust_remote_code=True).half().quantize().cuda()
# 如果想要本地访问，需要修改此处，如需要公网访问，还需要将 web_demo.py 中修改：share=True
demo.queue().launch(share=True, inbrowser=True, server_name='0.0.0.0', server_port=7860)
```

**温馨提示**，根据实际显卡显存，可以更改第 6 行关于 `model` 运行方式：

```
# 6G 显存可以 4 bit 量化
model = AutoModel.from_pretrained("model", trust_remote_code=True).half().quantize(4).cuda()

# 10G 显存可以 8 bit 量化
model = AutoModel.from_pretrained("model", trust_remote_code=True).half().quantize(8).cuda()

# 14G 以上显存可以直接不量化，博主显存为 16G 选择的就是这个
model = AutoModel.from_pretrained("model", trust_remote_code=True).half().cuda()
```

```
# Web 模式启动
# 官方推荐用 Streamlit 启动会更流畅一些，但受限于 PAI 平台没有分配弹性公网，所以还是用老的 gradio 启动吧。

# 安装 gradio 用于启动图形化 web 界面
pip install gradio

# 运行
python web_demo.py
# Running on local URL:
http://127.0.0.1:7860
# 直接点击日志弹出的 url，dsw 会自动进行公网代理的跳转，很丝滑
```

![](img/ba1186d670ce9aeb67751ac21d4e3f64.png)

访问界面

![](img/8355ddd61a4c5864d5367dd66d3b128b.png)

streamlit 方式启动，更加流畅

```
# 修改 web_demo2.py 中 model 位置的代码

# 然后执行启动命令：
streamlit run web_demo2.py

# 运行时模型以 FP16 精度加载，占用 GPU 显存为：13161 MiB
```

> 注意：确保 `transformers` 的版本为 4.30.2，否则会报错：`ImportError: cannot import name 'GenerationConfig' from 'transformers.generation.utils'`。

### 2.4.4.4 作为 API 部署

首先需要安装额外的依赖

```
pip install fastapi uvicorn
```

然后运行仓库中的 `api.py`：

```
python api.py
```

默认部署在本地的 8000 端口，通过 POST 方法进行调用

```
curl -X POST "http://127.0.0.1:8000" \
     -H 'Content-Type: application/json' \
     -d '{"prompt": "你好", "history": []}'
```

得到的返回值为

```
{
  "response": "你好👋！我是人工智能助手 ChatGLM-6B，很高兴见到你，欢迎问我任何问题。",
  "history": [["你好", "你好👋！我是人工智能助手 ChatGLM-6B，很高兴见到你，欢迎问我任何问题。"]],
  "status": 200,
  "time": "2023-03-23 21:38:40"
}
```

## 2.4.5 基于 P-Tuning 微调 ChatGLM2-6B

> ChatGLM2-6B 环境已经有了，接下来开始模型微调，这里我们使用官方的 P-Tuning v2 对 ChatGLM2-6B 模型进行参数微调，P-Tuning v2 将需要微调的参数量减少到原来的 0.1%，再通过模型量化、`Gradient Checkpoint` 等方法，最低只需要 7GB 显存即可运行。

目前我们实现了针对以下高效微调方法的支持：

+   [LoRA](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2106.09685)：仅微调低秩适应器。

+   [P-Tuning V2](https://link.zhihu.com/?target=https%3A//github.com/THUDM/P-tuning-v2)：仅微调前缀编码器。

+   [Freeze](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2012.14913)：仅微调后几层的全连接层。

微调代码就在 `THUDM/ChatGLM-6B` 的 `ptuning` 目录下

> **安装依赖**

```
# 运行微调需要 4.27.1 版本的 transformers
pip install transformers==4.27.1
pip install rouge_chinese nltk jieba datasets
```

> **禁用** `W&B`

```
# 禁用 W&B，如果不禁用可能会中断微调训练，以防万一，还是禁了吧
export WANDB_DISABLED=true
```

> **准备数据集**

经过前面的折腾，我们知道格式是 `{“content”:"","summary":""}`（看教程广告数据训练都是这个结构），一个是 input，一个是 output，我们按照这个格式造数据即可（经过 zy 网友指导，这里对于同一个 input 要做到同义句泛化，要使用大量同义句，微调的效果才好）

> **提前准备一下泛化数据**（懒得泛化的，可以直接手写自己的数据 `{“content”:"","summary":""}` 格式大概是这样），Self instruction 这个貌似要单独学习一下，基于 GPT 自动泛化，作为新手这里暂时就手动自己造数据好了

```
cd /mnt/workspace/chatglm2/ChatGLM2-6B/ptuning
mkdir mydata
vim mydata/dev.json
vim mydata/train.json
```

训练数据写入到 `dev.json`（随便一条），`train.json`（全部）

```
{"content": "你叫什么名字？", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你可以告诉我你的名字吗？", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你是GPT吗", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你能告诉我你的名字吗？", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你是机器人吗？", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你能告诉我你的名字吗？", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你知道我叫啥名字吗？", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "介绍一下你自己", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "介绍自己", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你是ChatGPT吗", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "自我介绍一下", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你是？", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你知道我叫啥名字吗？", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你是谁", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你是", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你可以告诉我你的名字吗", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你是哪位", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你叫啥", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你叫什么", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
{"content": "你是谁", "summary": "你好，我是小君，很高兴认识你，有什么可以帮你？"}
```

这里为了简化，我们使用官方提供的数据集

```
$ cd /mnt/workspace/chatglm2/ChatGLM2-6B/ptuning
# fine-tune 数据集下载
$ wget -O AdvertiseGen.tar.gz https://cloud.tsinghua.edu.cn/f/b3f119a008264b1cabd1/?dl=1
# 解压
$ tar -xzvf AdvertiseGen.tar.gz
```

+   修改 `train.sh` 和 `evaluate.sh` 中的 `train_file`、`validation_file` 和 `test_file` 为你自己的 JSON 格式数据集路径。

+   并将 `prompt_column` 和 `response_column` 改为 JSON 文件中输入文本和输出文本对应的 KEY。

+   可能还需要增大 `max_source_length` 和 `max_target_length` 来匹配你自己的数据集中的最大输入输出长度。

+   并将模型路径 `THUDM/chatglm2-6b` 改为你本地的模型路径。

> 3、**训练**

改一下 `train.sh` 的内容

![](img/2af72bbbc0cc248db86040ed3e15bb5a.png)

+   **`train.sh` 文件修改**
