
# Agent 能力体验与微调实战：以Llama 3为例

> 来源：[https://v11enp9ok1h.feishu.cn/docx/PHjfdjnpcokMfexMbYVcw8NanWh](https://v11enp9ok1h.feishu.cn/docx/PHjfdjnpcokMfexMbYVcw8NanWh)

2024 年 4 月 18 日 Meta 发布了最新的开源大模型 Llama 3。机智流和书生·浦语社区在第一时间进行了分享，并制作了系列课程帮助大家了解 Llama 3。今天带来的就是 Llama 3 最新的 Agent 体验和微调教程，大家一起来上手应用吧！

## Llama3 ReAct Demo

首先我们先来使用基于 [Lagent](https://github.com/InternLM/lagent) 的 Web Demo 来直观体验一下 Llama3 模型在 `ReAct` 范式下的智能体能力。我们让它使用 `ArxivSearch` 工具来搜索 InternLM2 的技术报告。

从图中可以看到，`Llama3-8B-Instruct` 模型并没有成功调用工具。原因在于它输出了 `query=InternLM2 Technical Report` 而非 `{'query': 'InternLM2 Technical Report'}`，这也就导致了 `ReAct` 在解析工具输入参数时发生错误，进而导致调用工具失败。

![](img/9e2fc784e73aa7275ddb4273ed2221e2.png)

## 微调过程

接下来我们带大家使用 [XTuner](https://github.com/InternLM/xtuner) 在 `Agent-FLAN` 数据集上微调 `Llama3-8B-Instruct`，以让 `Llama3-8B-Instruct` 模型获得智能体能力。

[Agent-FLAN](https://github.com/InternLM/Agent-FLAN) 数据集是上海人工智能实验室 InternLM 团队所推出的一个智能体微调数据集，其通过将原始的智能体微调数据以多轮对话的方式进行分解，对数据进行能力分解并平衡，以及加入负样本等方式构建了高效的智能体微调数据集，从而可以大幅提升模型的智能体能力。

## 环境配置

我们先来配置相关环境。使用如下指令便可以安装好一个 `python=3.10` `pytorch=2.1.2+cu121` 的基础环境了。

```
conda create -n llama3 python=3.10
conda activate llama3
conda install pytorch==2.1.2 torchvision==0.16.2 torchaudio==2.1.2 pytorch-cuda=12.1 -c pytorch -c nvidia
```

接下来我们安装 `XTuner`。

```
cd ~
git clone -b v0.1.18 https://github.com/InternLM/XTuner
cd XTuner
pip install -e .
```

## 模型准备

在微调开始前，我们首先来准备 `Llama3-8B-Instruct` 模型权重。

+ InternStudio

```
cd ~
ln -s /root/new_models/meta-llama/Meta-Llama-3-8B-Instruct .
```

+ 非 InternStudio

我们选择从 OpenXLab 上下载 `Meta-Llama-3-8B-Instruct` 的权重。

```
cd ~
git lfs install
git clone https://code.openxlab.org.cn/MrCat/Llama-3-8B-Instruct.git Meta-Llama-3-8B-Instruct
```

## 数据集准备

由于 HuggingFace 上的 `Agent-Flan` 数据集暂时无法被 `XTuner` 直接加载，因此我们首先要下载到本地，然后转换成 `XTuner` 直接可用的格式。

+ InternStudio

如果是在 InternStudio 上，我们已经准备好了一份转换好的数据，可以直接通过如下脚本准备好：

```
cd ~
ln -s /root/new_models/internlm/Agent-Flan .
```

+ 非 InternStudio

首先先来下载数据：

```
cd ~
git lfs install
git clone https://huggingface.co/datasets/internlm/Agent-FLAN
```

我们已经在 `SmartFlowAI/Llama3-Tutorial` 仓库中已经准备好了相关转换脚本，更多细节可以访问 https://github.com/SmartFlowAI/Llama3-Tutorial 。

```
cd ~
git clone https://github.com/SmartFlowAI/Llama3-Tutorial
python ~/Llama3-Tutorial/tools/convert_agentflan.py ~/Agent-Flan/data
```

在显示下面的内容后，就表示已经转换好了。转换好的数据位于 `~/Agent-Flan/data_converted`

```
Saving the dataset (1/1 shards): 100%|███████████████████████████████████████████| 34442/34442
```

## 微调启动

我们已经为大家准备好了可以一键启动的配置文件，主要是修改好了模型路径、对话模板以及数据路径。

我们使用如下指令以启动训练：

```
mkdir -p ~/project/llama3-ft
cd ~/project/llama3-ft
xtuner train ~/Llama3-Tutorial/configs/llama3-agentflan/llama3_8b_instruct_qlora_agentflan_3e.py --work_dir ~/project/llama3-ft/agent-flan
```

在训练完成后，我们将权重转换为 HuggingFace 格式，并合并到原权重中。

```
# 转换权重
xtuner convert pth_to_hf ~/Llama3-Tutorial/configs/llama3-agentflan/llama3_8b_instruct_qlora_agentflan_3e.py
    ~/project/llama3-ft/agent-flan/iter_18516.pth \
    ~/project/llama3-ft/agent-flan/iter_18516_hf
# 合并权重
export MKL_SERVICE_FORCE_INTEL=1
xtuner convert merge ~/Meta-Llama-3-8B-Instruct \
    ~/project/llama3-ft/agent-flan/iter_18516_hf
    ~/project/llama3-ft/agent-flan/merged
```

## Llama3+Agent-FLAN ReAct Demo

在合并权重后，我们再次使用 Web Demo 体验一下它的智能体能力吧~

可以看到，经过 `Agent-FLAN` 数据集的微调后，`Llama3-8B-Instruct` 模型已经可以成功地调用工具了，其智能体能力有了很大的提升。

![](img/8a531b02ec2aae26a54a040ebed24e59.png)

---
