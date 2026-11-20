
# L2 OpenCompass 评测书生大模型实践

> 来源：[https://aicarrier.feishu.cn/docx/GJOgdmYY8ofPtkxOTpKctJ8mnod](https://aicarrier.feishu.cn/docx/GJOgdmYY8ofPtkxOTpKctJ8mnod)

本课程将带您掌握 `OpenCompass` 大模型评测平台的核心应用，从环境配置、数据集下载，到用配置脚本评测 `Intern-s1` 模型的 `C-Eval` 选择题，同时还会教学多模态模型评测工具 `VLMEvalKit`，对 `InternVL3_5-1B` 在 `MME` 评测实操，让您从理论到实战全面落地模型评估。

# OpenCompass 概述

`OpenCompass` 是由上海人工智能实验室开发的开源大模型评测平台，旨在全面评估大语言模型和多模态模型的能力。

![](img/c88dae2a9029e0c069ae1d07c5dd8f58.png)

**在 `OpenCompass` 中评估一个模型通常包括以下几个阶段：配置 -> 推理 -> 评估 -> 可视化。**

**配置**：这是整个工作流的起点。您需要配置整个评估过程，选择要评估的模型和数据集。此外，还可以选择评估策略、计算后端等，并定义显示结果的方式。

**推理与评估**：在这个阶段，`OpenCompass` 将开始对[模型和数据集](https://opencompass.readthedocs.io/zh-cn/latest/get_started/faq.html#id6)进行推理和评估。推理阶段主要是让模型从数据集产生输出，而评估阶段则是衡量这些输出与标准答案的匹配程度。这两个过程会被拆分为多个同时运行的“任务”以提高效率，但请注意，如果计算资源有限，这种策略可能会使评测变得更慢。如果需要了解该问题及解决方案，可以参考 FAQ: 效率。

**可视化**：评估完成后，`OpenCompass` 将结果整理成易读的表格，并将其保存为 `CSV` 和 `TXT` 文件。你也可以激活飞书状态上报功能，此后可以在飞书客户端中及时获得评测状态报告。

对于其他模型，请参考 [`configs` 目录](https://github.com/open-compass/opencompass/tree/main/configs) 中提供的其他示例。

# 评测环境配置

## 开发机的选择

在创建开发机界面选择镜像为 `Cuda12.8-conda`，并选择 `GPU` 为 30% `A100`。

如无算力配额，申请助教老师开通。开通后还是爆显存，调低 `batch size===1`。

![](img/df94efabdfd71cd7af192fb0e1503bba.png)

## 2.2 conda 环境安装

```
conda create -n opencompass10 python=3.10
conda activate opencompass10

# 注意：一定要先 cd /root
cd /root
git clone -b 0.5.0 https://gh.llkk.cc/https://github.com/open-compass/opencompass.git opencompass
cd opencompass
pip install -e .
pip install opencompass[api]
pip install jmespath
```

# 评测不同类型的题目

## 3.0 数据集的下载

`OpenCompass` 支持的数据集主要包括三个部分：

1. `Huggingface` 数据集：`Huggingface Dataset` 提供了大量的数据集，这部分数据集运行时会自动下载。

1. `ModelScope` 数据集：`ModelScope OpenCompass Dataset` 支持从 `ModelScope` 自动下载数据集。

要启用此功能，请设置环境变量：`export DATASET_SOURCE=ModelScope`，可用的数据集包括（来源于 `OpenCompassData-core.zip`）：`humaneval`, `triviaqa`, `commonsenseqa`, `tydiqa`, `strategyqa`, `cmmlu`, `lambada`, `piqa`, `ceval`, `math`, `LCSTS`, `Xsum`, `winogrande`, `openbookqa`, `AGIEval`, `gsm8k`, `nq`, `race`, `siqa`, `mbpp`, `mmlu`, `hellaswag`, `ARC`, `BBH`, `xstory_cloze`, `summedits`, `GAOKAO-BENCH`, `OCNLI`, `cmnli`

1. 自建以及第三方数据集：`OpenCompass` 还提供了一些第三方数据集及自建中文数据集。运行以下命令手动下载解压。

为了方便评测，我们首先将数据集下载到本地：

```
apt-get update && apt-get install -y unzip
cd /root/opencompass
wget https://ghfast.top/https://github.com/open-compass/opencompass/releases/download/0.2.2.rc1/OpenCompassData-core-20240207.zip
unzip OpenCompassData-core-20240207.zip
```

将会在 `OpenCompass` 下看到 `data` 文件夹，里面包含的数据集如下图所示：

![](img/8e01de4d14d194d6e098dd87220bedaf.png)

## 使用 Intern-s1 评测 C-Eval 选择题

评测数据集为 `C-Eval`，格式为 `csv`，摘出部分内容如下所示：

有两种方式运行，命令行 `&&` 配置脚本运行。但如果要更高定制化需求，需要些配置脚本运行更合适。故下文用配置脚本进行教学。

### 3.1.1 写配置脚本运行

`OpenCompass` 允许用户在配置文件中编写实验的完整配置脚本，并通过 `run.py` 直接运行它。

配置文件就像一个指令集，告诉评测系统用什么模型 (`model`) 测什么数据 (`datasets`)。

用 `read_base()` 直接复用已有配置，不用重复编写。比如代码中通过 `with read_base()` 块，直接引入了 `ceval` 数据集和其他模型配置，再组合成当前所需的完整配置。

在 `opencompass/opencompass/configs` 文件夹下创建 `eval_tutorial_demo1.py`：

```
cd /root/opencompass/opencompass/configs
touch eval_tutorial_demo1.py
```

打开 `eval_tutorial_demo1.py` 贴入以下代码：

```
from mmengine.config import read_base
from opencompass.models import OpenAISDK

# 配置模型
models = [
    dict(
        type=OpenAISDK,
        path='intern-s1',  # 明确模型名称
        key='sk-xxx',  # 你的API密钥
        openai_api_base='https://chat.intern-ai.org.cn/api/v1',  # API地址
        rpm_verbose=True,
        query_per_second=1,  # 根据 rpm 限制,进行调整.rpm==30
        max_out_len=512,
        max_seq_len=4096,
        temperature=0.01,
        batch_size=10,# 根据 rpm 限制,进行调整.rpm==30
        retry=5,  # 增加重试次数
    )
]

# 配置数据集（只取每个子数据集的1个样本）
with read_base():
    from .datasets.ceval.ceval_gen import ceval_datasets

#datasets=ceval_datasets #测试完整的数据集

# 缩小数据集规模为五个子数据集，每个子数据集仅保留10个样本,缩短测评时间
datasets = []
for d in ceval_datasets[:5]:
    # 添加前缀标识这是演示用的精简数据集
    d['abbr'] = 'demo_' + d['abbr']
    # 仅使用第1个样本（索引0）
    d['reader_cfg']['test_range'] = '[0:10]'
    datasets.append(d)
```

因此，运行任务时，我们只需将配置文件的路径参数传递给 `run.py`：

```
cd /root/opencompass
python run.py opencompass/configs/eval_tutorial_demo1.py --debug
```

### 3.1.2 结果

因为只取了小部分数据集进行测试，5min 左右就会出现以下结果：

![](img/204eefa3eb1811bd3dca1c9f60f5326a.png)

所有运行输出将定向到 `outputs/` 目录，结构如下（下面为完整数据集测试输出图）：

![](img/6c42fb270c9e188f1322c94c690370eb.png)

打印评测结果的过程可被进一步定制化，用于输出一些数据集的平均分（例如 `MMLU`, `C-Eval` 等）。

[关于评测](https://opencompass.readthedocs.io/zh-cn/latest/user_guides/summarizer.html) 结果输出的更多介绍可阅读 结果展示。

## 使用 Intern-s1 评测自建数据集进行

这里我们评测一下论文分类比赛的数据，`newformat_sft_test_data.csv` 是 A 榜测试数据，将其放在 `/root/opencompass/newformat_sft_test_data.csv` 位置：

![](img/cc87edce671f1da5daba0ef3686151d9.png)

[newformat_sft_test_data.csv](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YOQUb7NRso3tEqxBUELc5UWYnhL)

在 `configs` 文件夹下创建 `eval_tutorial_demo2.py`：

```
cd /root/opencompass/opencompass/configs
touch eval_tutorial_demo2.py
```

```
from opencompass.models import OpenAISDK

# 配置模型
models = [
    dict(
        type=OpenAISDK,
        path='intern-s1',  # 明确模型名称
        key='sk-xxxx',  # 你的API密钥
        openai_api_base='https://chat.intern-ai.org.cn/api/v1',  # API地址
        rpm_verbose=True,
        query_per_second=1,  # 根据 rpm 限制,进行调整.rpm==30
        max_out_len=512,
        max_seq_len=4096,
        temperature=0.01,
        batch_size=8,# 根据 rpm 限制,进行调整.rpm==30
        retry=5,  # 增加重试次数
    )
]

# 配置数据集
datasets = [
    dict(
        path='/root/opencompass/newformat_sft_test_data.csv',
        data_type='mcq',
        infer_method='gen'
    )
]
```

运行：

```
# 在 `opencompass` 目录下
python run.py opencompass/configs/eval_tutorial_demo2.py --debug
```

如果一切正常，大概 5min 将会看到：

![](img/9238be650a774673276a3a6c62402c0d.png)

## 评测 MME 多模态题——InternVL3_5-1B 评测实践

`VLMEvalKit` 是一个专为大型视觉语言模型评测设计的开源工具包。它支持在各种基准测试上对大型视觉语言模型进行一键评估，无需进行繁重的数据准备工作，使评估过程更加简便。`VLMEvalKit` 适用于图文多模态模型的评测，支持单对图文输入或是任意数量的图文交错输入。它通过实现 70 多个基准测试，覆盖了多种任务，包括但不限于图像描述、视觉问答、图像字幕生成等。

![](img/80613d40be57b495b23546e116d0be56.png)

`VLMEvalKit` 的主要用途包括以下几个方面：

1. **多模态模型评估**：`VLMEvalKit` 旨在为研究人员和开发人员提供一个用户友好且全面的框架，以评估现有的多模态模型并发布可重复的评估结果。它支持多种多模态模型和评测集，包括闭源和开源模型，以及多个多模态基准数据集。

1. **模型性能比较**：通过 `VLMEvalKit`，用户可以轻松比较不同多模态模型在各种任务上的性能。该工具包提供了详细的评测结果，并支持将结果发布在 `OpenCompass` 的多模态整体榜单上。

1. **便捷的一站式评测**：`VLMEvalKit` 支持一键式评测，无需手动进行数据预处理。用户只需一条命令即可完成对多个多模态模型和评测集的评测。

1. **易于扩展**：`VLMEvalKit` 框架支持轻松添加新的多模态模型和评测集。用户可以根据需要添加新的模型或评测集，并确保原有的评测集和模型仍然适用于新的模型或评测集。

1. **支持多种模型和评测集**：`VLMEvalKit` 支持超过三十个开源多模态模型和十余个开源多模态评测集，包括 `GPT-4v`、`GeminiPro`、`QwenVLPlus` 等主流模型和 `MME`、`MMBench`、`SEEDBench` 等评测集。

1. **提供定量与定性结果**：`VLMEvalKit` 不仅提供定量评测结果，还支持对模型的定性分析，帮助用户了解模型在特定任务上的表现和不足之处。

使用 `VLMEvalKit` 进行评测需要进行以下步骤：

### 环境准备

用以下命令安装依赖：

```
cd /root/opencompass
git clone https://gh.llkk.cc/https://github.com/open-compass/VLMEvalKit.git
conda create -n VLMEvalKit1 python=3.10
conda activate VLMEvalKit1
cd VLMEvalKit
pip install -e .
pip install datasets transformers==4.56.1
```

> **注意：**
>
> 某些 `VLM` 可能无法在某些特定的 `transformers` 版本下运行，参考以下设置来评估对应的 `VLM`：
>
> + 请用 `transformers==4.33.0` 来运行: `Qwen series`, `Monkey series`, `InternLM-XComposer Series`, `mPLUG-Owl2`, `OpenFlamingo v2`, `IDEFICS series`, `VisualGLM`, `MMAlaya`, `ShareCaptioner`, `MiniGPT-4 series`, `InstructBLIP series`, `PandaGPT`, `VXVERSE`.
>
> + 请用 `transformers==4.37.0` 来运行: `LLaVA series`, `ShareGPT4V series`, `TransCore-M`, `LLaVA (XTuner)`, `CogVLM Series`, `EMU2 Series`, `Yi-VL Series`, `MiniCPM-[V1/V2]`, `OmniLMM-12B`, `DeepSeek-VL series`, `InternVL series`, `Cambrian Series`, `VILA Series`, `Llama-3-MixSenseV1_1`, `Parrot-7B`, `PLLaVA Series`.
>
> + 请用 `transformers==4.40.0` 来运行: `IDEFICS2`, `Bunny-Llama3`, `MiniCPM-Llama3-V2.5`, `360VL-70B`, `Phi-3-Vision`, `WeMM`.
>
> + 请用 `transformers==latest` 来运行: `LLaVA-Next series`, `PaliGemma-3B`, `Chame3leon series`, `Video-LLaVA-7B-HF`, `Ovis series`, `Mantis series`, `MiniCPM-V2.6`, `OmChat-v2.0-13B-sinlge-beta`, `Idefics-3`, `GLM-4v-9B`, `VideoChat2-HD`.

### 修改文件

在 `$VLMEvalKit/vlmeval/config.py` 文件中设置在 `VLMEvalKit` 中支持的 `VLM` 名称，以及模型路径。

如果你的电脑上面没有该模型的模型文件，则需要自己下载，然后更改模型路径，也可以不修改，在运行模型评测命令的时候会自动下载模型文件。
