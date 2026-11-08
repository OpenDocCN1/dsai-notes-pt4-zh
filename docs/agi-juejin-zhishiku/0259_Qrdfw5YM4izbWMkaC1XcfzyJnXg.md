# 覆盖 7 百万问答数据，上海 AI Lab 发布 ChemLLM，专业能力比肩 GPT-4

> 来源：[https://agijuejin.feishu.cn/docx/HWnYdkLm3oNTRWxftDKcdJyunqc](https://agijuejin.feishu.cn/docx/HWnYdkLm3oNTRWxftDKcdJyunqc)

![](img/397e05b9475a3d66b1a9b7dafb74cb15.png)

> 作者：李姝    [阅读原文](https://mp.weixin.qq.com/s/6tlKJmKQWAuOBdXTEtYazg)

随着人工智能技术的飞速发展，大语言模型 (LLMs) 凭借其强大的自然语言处理能力，在生命科学、海洋学、材料化学等科学研究中被广泛应用。虽然，LLMs 在分子特性预测、分子生成和实验方案设计等化学相关任务上表现良好，但在处理各种化学下游任务时表现欠佳。

究其原因，直接将化学知识集成到语言模型中，主要面临三大挑战：首先，大多数化学信息和知识存储在结构化数据库中，直接使用这些数据训练 LLMs 可能会损害模型处理自然语言的能力，使得模型的对话和逻辑推理能力发生退化；其次，在化学信息学中，分子用特殊符号表示，如 SMILES。然而这类数据往往不符合自然语言的规范，因此常规的语言模型难以正确理解和生成这种符号；最后，化学数据和任务种类繁多，设计一个灵活并能泛化到多种化学任务上的训练流程十分困难。

**

针对于此，上海人工智能实验室发布了化学大语言模型 ChemLLM。ChemLL**M 擅长通过流畅的对话交互执行化学学科的各种任务，在核心任务上的性能与 GPT-4 相当，并在一般场景中表现出了与类似规模的 LLM 相媲美的性能。ChemLLM 为化学研究的探索开辟了新的道路，并且研究团队将结构化化学知识融入对话系统的方法，为开发各科学领域的 LLM 树立了新的标准。

相关研究以「ChemLLM: A Chemical Large La**n**guage Model」为题，已发表于 arXiv，成果已开源，并提供免费商用。目前 HyperAI超神经官网 (hyper.ai) 已上线「一键部署化学大模型 ChemLLM-7B-chat」，分步教程详见文末～

**

研究亮点**：

+   创建并开源大规模化学数据集 ChemData 以及 ChemPref-10K 的中英文版本数据集、C- MHChem 数据集、 ChemBench4K 化学能力评测基准数据集

+   创建并开源包含 4,100 道选择题、9 项特定任务的大规模化学基准测试 ChemBench

+   通过定量、定性的评估测试，ChemLLM 表现出良好的化学专业性和多功能性

![](img/cda985b2e57446cc9b07c1fa2d59f6a2.png)

**论文地址：**

https://arxiv.org/abs/2402.06852

关注👸号，后台回复「ChemLLM」获取完整 PDF

**化学大模型 ChemLLM-7B-chat 教程已在 h**yper.ai 上线，点击链接一键部署：

https://go.hyper.ai/r31KV

ChemData 化学任务数据集下载地址：

https://go.hyper.ai/zMJEl

**开源项目「awesome-ai4s」汇集了百余篇 AI**4S 论文解读，并提供海量数据集与工具：

https://github.com/hyperai/awesome-ai4s

## **ChemData 数据集: 涵盖** 700 万条问答数据的大规模化学数据集

研究人员从众多在线资源库中收集了包括 PubChem、ChEMBL、ChEBI、ZINC 等在内的化学数据，并在此基础上创建了一个用于微调 ChemLLM 的大规模数据集 ChemData。

**C**hemData 数据集利用基于模板的指令构建方法，将结构化的化学数据转换为适合训练 LLM 的自然对话形式。该数据集包含 700 万条用于指令微调的问答数据，涵盖了广泛的化学领域知识，并且该问答数据类别与分子、反应和其他与化学相关的任务类别一致。

其中**，与**分子相关的任务包括名称转换 (Name Conversion)、Caption2Mol、Mol2Caption 和分子性质预测 (Molecular Property Prediction)，主要目的是调整语言模型对化学分子的感知。

与反应相关的任务涉及化学反应的各个方面，包括逆合成 (Retrosynthesis)、产物预测 (Product Prediction)、产量预测 (Yield Prediction)、温度预测 (Temperature Prediction) 和溶剂预测 (Solvent Prediction)。除可明确分类的数据外，所有其他数据都归入特定类型的任务，从而增强 ChemLLM 对整个化学空间的理解。下图展示了这三类任务所包含数据分别占的比重。

![](img/e3daff5afcb815181efcd64d480541e7.png)

ChemData 数据集构成占比

## **ChemLLM 模型架构：以 InternLM2-Bas**e-7B 为基底，进行两个阶段的指令微调

**化学**大语言模型 ChemLLM 是在 InternLM2-Base-7B 模型训练基础上，通过两个阶段的指令微调方法训练得到的，不仅实现了多种化学能力，还保留了完整的自然语言能力。

如下图所示，在第一阶段，研究团队使用 Multi-Corpus（一个从 Hugging Face 收集的包含 170 万个问答对的综合语料库），提升了模型的通用语言能力，并将第一阶段获得的模型命名为 InternLM2-Chat-7B 。

![](img/7b26826d2095cf8f2e79082b4a50eb86.png)

ChemLLM 两个阶段的指令微调流程示意图

在第二阶段，研究团队使用 ChemData 和 Multi-Corpus 的混合数据集对模型进行微调，其中 ChemData 用于增强模型的化学知识，而 Multi-Corpus 用于保留模型的通用能力。经过两个阶段的指令微调训练，提高了 ChemLLM 在化学领域的通用性。

## ChemBench 基准测试：减少语言模型输出风格对评估结果的影响

现有的化学大模型基准测试多以问答形式呈现，并使用 BLEU 和 ROUGE 作为评估标准。然而，这类评估容易受到语言模型输出风格的影响，不适合强调科学事实正确性的场景。

**基**于此，研究团队构建了与当前主流评估集 MMLU、C-Eval 相类似的化学基准测试 ChemBench。ChemBench 包括 9 项关于化学分子和反应的任务，并且与 ChemData 数据集中的任务相同。除此之外，ChemBench 还包含 4,100 道选择题，每道题对应一个正确答案，旨在最大程度地减少语言模型输出风格对评估结果的影响。

值得一提的是，该基准测试已经在 OpenCompass 开源项目上线。下图展示了 ChemBench 基准测试 9 项任务的分布情况。

![](img/b526e2bea07a81cb23be5a7c76fa9f30.png)

ChemBench 基准测试 9 项任务分布情况

## **研究结果：ChemLLM 模型化学专业能力与 GPT-4** 相当，明显优于类似规模的一般 LLMs

**研究团队从定量与定性两个维度，对化学大语**言模型 ChemLLM 的性能进行评估。定量评估包括化学能力和通用能力评估，定性评估主要通过与化学相关 NLP（自然语言处理）任务中的表现进行评估。

**

在化学能力评估中**，ChemBench 作为评估核心化学能力的基准，通过 9 项不同任务测试该模型的专业性。如下图所示，ChemLLM 明显优于类似规模的一般大语言模型 (LLMs)，全面超越了 GPT-3.5。与 InternLM2-Chat-7B 相比，ChemLLM 在化学方面的能力有了显著提高，说明第二阶段的化学能力训练具有显著效果。与 GPT-4 相比，ChemLLM 在 9 项任务中有 6 项得分比 GPT-4 高。

![](img/82873e0e4a045c02927f6138260e88ca.png)

ChemLLM 化学性能评估得分

**在通用能力评估中，**研究团队使用 MMLU、C-Eval、GSM8K、C-MHChem 4 个数据集对 ChemLLM 进行评估。其中，MMLU 是一个涵盖 STEM（科学、技术、工程和数学）、人文和社会科学等跨学科科目的基准测试，对跨学科知识进行广泛评估；C-Eval 是一个全面的中文基准测试，涵盖多个学科并分为 4 个难度等级；GSM8K 是一个用于测试语言模型数学能力的基准测试，要求通过 2-8 步的基本数学运算来解决问题；C-MHChem 是一个用于评估模型基本化学概念的数据集，主要涉及初高中化学测试。

如下图所示，在英文的 MMLU 和中文的 C-Eval 基准测试中，ChemLLM 的准确率分别为 65.6 和 64.1，说明它在更广泛的学科和多语言场景中表现出色。

在 GSM8K 数据集测试中，ChemLLM 的准确率达到 67.2，结果表明在化学数据上进行微调，在一定程度上增强了模型的推理能力。

在 C-MHChem 数据集测试中，ChemLLM 准确率为 76.4，超过了 GPT-4，展示了 ChemLLM 在中国初高中考试中的能力。

![](img/3649ef99b99fe08ed79d4811d712b27b.png)

ChemLLM 通用性能评估得分

**

在定性评估中**，研究团队通过化学诗歌创作、文本提取、化学文献翻译、伦理回答等与化学相关的 NLP（自然语言处理） 任务对 ChemLLM 进行评估。结果显示，ChemLLM 在各种 NLP 任务中都能够对化学知识进行更深入地理解和创造性运用。下图列举了 ChemLLM 在部分 NLP 任务上的表现：

![](img/825d24cf8f007b21ff1b2e3de52ca47c.png)

ChemLLM 化学诗歌创作

![](img/577ef76f7fdc6ddba04bfd38a7273105.png)

ChemLLM 化学信息提取

以上研究结果显示，ChemLLM 能够通过实时对话处理各种化学任务，其化学能力与 GPT-4 相当，并在其他领域表现出色。

**

目**前，ChemLLM 已经完成新一轮升级，ChemLLM-1.5 接入 RAG 功能，不仅支持对化学文献进行深度挖掘和理解以及联网搜索，而且支持与 ChemLLM 直接对话探讨文章内容。ChemLLM 的发展为科学领域的 LLMs 树立了先例，进一步加速了 AI 时代化学研究的进程。

**H**yperAI超神经官网 (hyper.ai) 已经上线了「一键部署化学大模型 ChemLLM-7B-chat」，以下是分步教程与效果展示，快跟着小编一起探索吧～

## 一键部署化学大模型 ChemLLM-7B-chat

**Demo 运行**

1.  登录 hyper.ai，在「教程」页面，选择「一键部署浦科化学大模型 ChemLLM-7B-chat Demo」，点击「在线运行此教程」。

![](img/074b36d50feade8478c1b6941a4b8a83.png)

![](img/20766c77e223aaec683f4a20567ace5c.png)

1.  页面跳转后，点击右上角「克隆」，将该教程克隆至自己的容器中。

![](img/b1fd79b38355a42e33b768fe6339f273.png)

1.  点击右下角「下一步：选择算力」。

![](img/1081d88728e67989b497270164d2b049.png)

1.  页面跳转后，选择「NVIDIA GeForce RTX 4090」，点击「下一步：审核」。新用户使用下方邀请链接注册，可获得 4 小时 RTX 4090 + 5 小时 CPU 的免费时长！

HyperAI超神经专属邀请链接（直接复制到浏览器打开）：

https://openbayes.com/console/signup?r=6bJ0ljLFsFh_Vvej

![](img/f15132d3fd2363c9885ac1a5d8e593b5.png)

1.  点击「继续执行」，等待分配资源，首次克隆需等待 2 分钟左右的时间。当状态变为「运行中」后，点击「API 地址」边上的跳转箭头，即可跳转至「一键部署浦科化学大模型 ChemLLM-7B-chat Demo」的页面。请注意，用户需在实名认证后才能使用 API 地址访问功能。

若超过 10 分钟仍处于「正在分配资源」状态，可尝试停止并重启容器；若重启仍无法解决，请在官网联系平台客服。

![](img/079a4b5d35e7c63f8ab9a1c3d64e4aa9.png)

![](img/2ca1fbe2fad5f4c7803b5a935e2fcd32.png)

![](img/2162bd75322c71691bbd68ec153f044b.png)

**效果预览**

### 以药物研发中的伦理困境问题进行测试

![](img/6f01c1b85b4dba74daef705413cb6139.png)

**参考资料：**

1.[ https://mp.weixin.qq.com/s/C](https://mp.weixin.qq.com/s?__biz=MzA3Mzg1NzkxMQ==&mid=2650101798&idx=1&sn=8e93e0e6d061b430e427658b585dea1c&scene=21#wechat_redirect)_aF[YbzLlQySmTDarWWRkA

2. https:/](https://mp.weixin.qq.com/s?__biz=MzA3ODkxNTMyMw==&mid=2247483865&idx=1&sn=ec53c8e48111381619a2c172471e5113&scene=21#wechat_redirect)/mp.weixin.qq.com/s/b9T9LxAkv4gnJMfBs2AW5Q