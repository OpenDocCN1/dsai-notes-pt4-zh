
# 开源 · 多模态 VisualGLM-6B，最低只需 8.7G 显存

> 来源：[https://ai.feishu.cn/docx/RNbOd8hxSopgfMxH9xacvCwvnub](https://ai.feishu.cn/docx/RNbOd8hxSopgfMxH9xacvCwvnub)

![](img/48d2ed4978a84c6678f92372bff2fc69.png)

OpenAI 的`GPT-4`样例中展现出令人印象深刻的多模态理解能力，但是能理解图像的中文开源对话模型仍是空白。

近期，智谱AI 和清华大学 KEG 实验室开源了基于 `ChatGLM-6B` 的多模态对话模型 `VisualGLM-6B`，供大家使用。

`Github` 链接：`https://github.com/THUDM/VisualGLM-6B`

`Huggingface` 链接：`https://huggingface.co/THUDM/visualglm-6b`

`VisualGLM-6B` 可以进行图像的描述及相关知识的问答：

![](img/6a16185756ffc84386999736a3126bad.png)

也能结合常识或提出有趣的观点：

![](img/15c047f6942cd5bb51435dd272a99afd.png)

![](img/e739409cdd5551c09e79f4d71bb77d2d.png)

## 一、简介

`VisualGLM-6B` 是一个开源的，支持图像、中文和英文的多模态对话语言模型，语言模型基于 `ChatGLM-6B`，具有 62 亿参数；图像部分通过训练 `BLIP2-Qformer` 构建起视觉模型与语言模型的桥梁，整体模型共 78 亿参数。

`VisualGLM-6B` 依靠来自于 `CogView` 数据集的30M高质量中文图文对，与 300M 经过筛选的英文图文对进行预训练，中英文权重相同。该训练方式较好地将视觉信息对齐到 `ChatGLM` 的语义空间；之后的微调阶段，模型在长视觉问答数据上训练，以生成符合人类偏好的答案。

`VisualGLM-6B` 由 `SwissArmyTransformer` (简称 `sat` ) 库训练，这是一个支持 `Transformer` 灵活修改、训练的工具库，支持 `Lora`、`P-tuning` 等参数高效微调方法。本项目提供了符合用户习惯的 `huggingface` 接口，也提供了基于`sat`的接口。

不过，由于 `VisualGLM-6B` 仍处于`v1`版本，目前已知其具有相当多的局限性，如图像描述事实性/模型幻觉问题，图像细节信息捕捉不足，以及一些来自语言模型的局限性。请大家在使用前了解这些问题，评估可能存在的风险。在`VisualGLM`之后的版本中，将会着力对此类问题进行优化。

结合模型量化技术，用户可以在消费级的显卡上进行本地部署（`INT4`量化级别下最低只需8.7G显存）。

## 二、使用

### 模型推理

使用`pip`安装依赖

```
pip install -r requirements.txt
```

此时默认会安装`deepspeed`库（支持`sat`库训练），此库对于模型推理并非必要，同时部分`Windows`环境安装此库时会遇到问题。如果想绕过`deepspeed`安装，我们可以将命令改为

```
pip install -r requirements_wo_ds.txt
pip install --no-deps 'SwissArmyTransformer>=0.3.
```

如果使用`Huggingface transformers`库调用模型，可以通过如下代码（其中图像路径为本地路径）：

```
from transformers import AutoTokenizer, AutoModel
tokenizer = AutoTokenizer.from_pretrained("THUDM/visualglm-6b", trust_remote_code=True)
model = AutoModel.from_pretrained("THUDM/visualglm-6b", trust_remote_code=True).half().cuda()
image_path = "your image path"
response, history = model.chat(tokenizer, image_path, "描述这张图片。", history=[])
print(response)
response, history = model.chat(tokenizer, "这张图片可能是在什么场所拍摄的？", history=history)
print(response)
```

如果使用`SwissArmyTransformer`库调用模型，方法类似，可以使用环境变量`SAT_HOME`决定模型下载位置。在本仓库目录下

```
>>> import argparse
>>> from transformers import AutoTokenizer
>>> tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True)
>>> from model import chat, VisualGLMModel
>>> model, model_args = VisualGLMModel.from_pretrained('visualglm-6b', args=argparse.Namespace(fp16=True, skip_init=True))
>>> from sat.model.mixins import CachedAutoregressiveMixin
>>> model.add_mixin('auto-regressive', CachedAutoregressiveMixin())
>>> image_path = "your image path or URL"
>>> response, history, cache_image = chat(image_path, model, tokenizer, "描述这张图片。", history=[])
>>> print(response)
>>> response, history, cache_image = chat(None, model, tokenizer, "这张图片可能是在什么场所拍摄的？", history=history, image=cache_image)
>>> print(response)
```

使用 `sat` 库也可以轻松进行参数高效微调。

请注意，`Huggingface`模型的实现位于`Huggingface`的仓库中，`sat`模型的实现包含于本仓库中。

## 三、部署工具

### 命令行 Demo

```
python cli_demo.py
```

程序会自动下载`sat`模型，并在命令行中进行交互式的对话，输入指示并回车即可生成回复，输入 `clear` 可以清空对话历史，输入 `stop` 终止程序。

![](img/7f6cc07e148c1eabe7e39bd9e1c10490.png)

程序提供如下超参数控制生成过程与量化精度：

```
usage: cli_demo.py [-h] [--max_length MAX_LENGTH] [--top_p TOP_P] [--top_k TOP_K] [--temperature TEMPERATURE] [--english] [--quant {8,4}]
optional arguments:  -h, --help            show this help message and exit
  --max_length MAX_LENGTH                        max length of the total sequence
  --top_p TOP_P         top p for nucleus sampling
  --top_k TOP_K         top k for top k sampling
  --temperature TEMPERATURE                        temperature for sampling
  --english             only output English
  --quant {8,4}         quantization bits
```

我们也提供了继承自`ChatGLM-6B`的打字机效果命令行工具，此工具使用`Huggingface`模型：

```
python cli_demo_hf.py
```

### 网页版 Demo

![](img/86029da652c215d500e3c26ddee7b596.png)

我们提供了一个基于 `Gradio` 的网页版 `Demo`，首先安装 `Gradio`：`pip install gradio`。然后下载并进入本仓库运行`web_demo.py`：

```
git clone https://github.com/THUDM/VisualGLM-6B
cd VisualGLM-6B
python web_demo.py
```

程序会自动下载`sat`模型，并运行一个 Web Server，并输出地址。在浏览器中打开输出的地址即可使用。`--quant 4`使用4比特量化减少显存占用。

我们也提供了继承自`ChatGLM-6B`的打字机效果网页版工具，此工具使用`Huggingface`模型：

```
python web_demo_hf.py
```

### API部署

首先需要安装额外的依赖 `pip install fastapi uvicorn`，然后运行仓库中的 `api.py`：

```
python api.py
```

程序会自动下载`sat`模型，默认部署在本地的 `8080` 端口，通过 `POST` 方法进行调用。下面是用`curl`请求的例子，一般而言可以也可以使用代码方法进行`POST`。

```
echo "{\"image\":\"$(base64 path/to/example.jpg)\",\"text\":\"描述这张图片\",\"history\":[]}" > temp.json
curl -X POST -H "Content-Type: application/json" -d @temp.json http://127.0.0.1:8080
```

得到的返回值为

```
{    "response":"这张图片展现了一只可爱的卡通羊驼，它站在一个透明的背景上。这只羊驼长着一张毛茸茸的耳朵和一双大大的眼睛，它的身体是白色的，带有棕色斑点。",    "history":[('描述这张图片', '这张图片展现了一只可爱的卡通羊驼，它站在一个透明的背景上。这只羊驼长着一张毛茸茸的耳朵和一双大大的眼睛，它的身体是白色的，带有棕色斑点。')],    "status":200,    "time":"2023-05-16 20:20:10"  }
```

## 四、模型量化

在`Huggingface`实现中，模型默认以 `FP16` 精度加载，运行上述代码需要大概 15GB 显存。如果你的 `GPU` 显存有限，可以尝试以量化方式加载模型。使用方法如下：

```
按需修改，目前只支持 4/8 bit 量化。下面将只量化ChatGLM，ViT 量化时误差较大
model = AutoModel.from_pretrained("THUDM/visualglm-6b", trust_remote_code=True).quantize(8).half().cuda()
```

在`sat`实现中，需先传参将加载位置改为`cpu`，再进行量化。方法如下，详见`cli_demo.py`：

```
from sat.quantization.kernels import quantize
model = quantize(model.transformer, args.quant).cuda()
指定 model.transformer 只量化 ChatGLM，ViT 量化时误差较大
```

## 五、局限性

本项目正处于`V1`版本视觉和语言模型的参数、计算量都较小，我们总结了如下主要存在的改进方向：

+   图像描述事实性/模型幻觉问题。在生成图像长描述的时候，距离图像较远时，语言模型的将占主导，有一定可能根据上下文生成并不存在于图像的内容。

+   属性错配问题。在多物体的场景中，部分物体的某些属性，经常被错误安插到其他物体上。

+   分辨率问题。本项目使用了224*224的分辨率，也是视觉模型中最为常用的尺寸；然而为了进行更细粒度的理解，更大的分辨率和计算量是必要的。
