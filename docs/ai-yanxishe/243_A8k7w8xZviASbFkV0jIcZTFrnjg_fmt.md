
# AI+金融

> 来源：[https://aiyanxishe.feishu.cn/docx/OjL3dpwCkoDFJWxO7RzcaQhVnOb](https://aiyanxishe.feishu.cn/docx/OjL3dpwCkoDFJWxO7RzcaQhVnOb)

![](img/96f3b53599f5695f54386a40cb27e8f1.png)

## Cornucopia（聚宝盆）

基于中文金融知识的`LLaMA`模型。

- 地址：[https://github.com/jerry1993-tech/Cornucopia-LLaMA-Fin-Chinese](https://github.com/jerry1993-tech/Cornucopia-LLaMA-Fin-Chinese)
- 简介：开源了经过中文金融知识指令精调/指令调节(Instruct-tuning)的`LLaMA-7B`模型。通过中文金融公开数据+爬取的金融数据构建指令数据集，并在此基础上对`LLaMA`进行了了指令扭矩，提高了`LLaMA`在金融领域的问答效果。基于相同的数据，提升还将利用`GPT3.5 API`构建高质量的数据集，另外在中文知识图谱-金融上进一步增加高质量的指令数据集。

## BBT-FinCUGE-应用

- 地址：[https://github.com/ssymmetry/BBT-FinCUGE-Applications](https://github.com/ssymmetry/BBT-FinCUGE-Applications)
- 简介：开源了中文金融领域开源料培训库`BBT-FinCorpus`，中文金融领域知识增强型预语言模型`BBT-FinT5`及中文金融领域自然语言处理金融基准`CFLEB`。

## 轩辕（轩辕）

首个千亿级中文金融对话模型。

- 地址：[https://github.com/Duxiaoman-DI/XuanYuan](https://github.com/Duxiaoman-DI/XuanYuan)
- 简介：轩辕是国内首个开源的千亿级中文对话大模型，同时也是第一个针对中文金融领域优化的千亿级开源对话大模型。轩辕在`BLOOM-176B`的基础上针对中文通用领域和金融领域进行了舆情的预训练，它不仅可以解决通用领域的问题，还可以解答与金融相关的人群问题，为用户提供准确、全面的金融信息和建议。

## 芬GPT

- 地址：[https://github.com/AI4Finance-Foundation/FinGPT](https://github.com/AI4Finance-Foundation/FinGPT)
- 简介：该项目开源了多个金融大模型，包括`ChatGLM-6B/ChatGLM2-6B+LoRA`和`LLaMA-7B+LoRA`的金融大模型，收集了包括金融新闻、社交媒体、财报等中英文训练数据。

## DISC金融法学硕士

- 地址：[https://github.com/FudanDISC/DISC-FinLLM](https://github.com/FudanDISC/DISC-FinLLM)
- 简介：该项目由复旦大学数据智能与社会计算实验室（Fudan-DISC）开发并开源，项目中开源的资源包括：`DISC-FinLLM-SFT`训练数据样本，`DISC-FinLLM`模型参数（基于`Baichuan-13B- Chat`训练），`DISC-Fin-Eval-Benchmark`等。

## 统一财经

- 地址：[https://modelscope.cn/models/TongyiFinance/Tongyi-Finance-14B](https://modelscope.cn/models/TongyiFinance/Tongyi-Finance-14B)
- 简介：该模型是针对金融行业推出的大语言模型，基于义千问通基础模型进行行业语料增量学习，强化金融领域知识和场景应用能力，覆盖金融知识问答、文本分类、信息抽取、文本创作、阅读理解、逻辑推理、多模态、编码等能力象限。具有以下特点：行业语料增量学习：使用`200B`优质金融行业语料进行增量学习，并进行金融行业词表扩展，覆盖丰富的数据类型，支持更大上下（16k）输入和完整的语义表达。行业能力强化：自研`SFT`质量和多样性分析工具，筛选高质量`SFT`数据，解决大语言模型的对齐问题。行业后序列优化：借助多代理框架，实现知识库增强和工具`API`调用。
