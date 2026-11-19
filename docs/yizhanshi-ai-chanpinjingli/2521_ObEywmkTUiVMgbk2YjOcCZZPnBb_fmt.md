
# 6🈷️02日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/ZjqodzwP5oNKDdxBidqcBP7vnTg](https://v11enp9ok1h.feishu.cn/docx/ZjqodzwP5oNKDdxBidqcBP7vnTg)

> 🔔AI情报助手早报「06月02日」

> ✨✨✨✨✨✨✨✨

## 1️⃣

🤖

Mamba核心作者新作：取代DeepSe

+ Mamba核心作者之一Tri Dao等普林斯顿大学学者提出GTA和GLA两种专为推理定制的注意力机制。
+ GTA与GQA质量相当，但KV缓存用量减少约50%；GLA与MLA质量匹配，解码速度某些情况快2倍。
+ 实验表明，GTA、GLA在不牺牲模型质量前提下，显著提升推理效率与硬件资源利用率。
+ 研究针对模型推理阶段难题，重新设计注意力机制，优化内存使用与计算逻辑。
+ 论文作者表示这是迈向推理“理想”架构第一步。

**关键词：** 注意力机制，推理效率，GTA，GLA，大语言模型

🔗 文中引用的链接

+ [论文](https://arxiv.org/abs/2505.21487)
+ [代码](https://github.com/Dao-AILab/grouped-latent-attention)
+ [1](https://x.com/tri_dao/status/1928170648863473892)
+ [2](https://x.com/gm8xx8/status/1927572103806554262)
+ [3](https://x.com/tedzadouri/status/1928167296821854363)

## 2️⃣

🤖

揭开大模型“伪遗忘”，港理工等团队：结构不变就是没忘

+ 港理工等团队构建诊断工具，区分大模型“可逆性遗忘”与“灾难性不可逆遗忘”，揭示遗忘现象背后表示结构变化规律。
+ 研究在Yi-6B、Qwen2.5-7B模型实验，对比不同方法和指标，探究遗忘场景下性能波动。
+ 可视化诊断发现可逆与不可逆遗忘在表示空间主方向、分布中心等方面差异。
+ 研究得出持续遗忘风险高、真正遗忘是结构漂移等结论。
+ 结构诊断工具为实现安全遗忘机制奠定基础。

**关键词：** 大模型，机器遗忘，结构变化，可逆性，诊断工具

🔗 文中引用的链接

+ [论文地址](https://arxiv.org/abs/2505.16831)
+ [Github地址](https://github.com/XiaoyuXU1/Representational_Analysis_Tools.git)

## 3️⃣

🎯

挑战强化学习后训练霸权！全新无监督方法仅

+ Ubiquant团队提出无监督方法One Shot熵最小化（EM），仅用1条无标签数据、10步训练，就能提升LLM性能，或可打破强化学习（RL）微调瓶颈。
+ EM通过最小化标记级熵减少模型预测不确定性，依赖筛选出的高信息量示例，在多个数学推理任务上效果显著。
+ 研究发现，EM使模型`Logits`分布右移，强化自信；RL则使其左移，受真实信号引导。
+ EM存在“过度自信”陷阱，训练后期性能可能下降，且具随机性。
+ EM适合基础模型、资源有限场景，未来可研究与现有技术融合等方向。

**关键词：** 熵最小化，强化学习，大语言模型，数学推理，无监督方法

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/3TE_mFq-pjj2Ebv3W1XNKw)

## 4️⃣

🤖

「平价 Model Y」曝光；OpenA

+ 谷歌推出“AI Edge Gallery”应用，可在手机本地运行AI模型。
+ Anthropic年化收入突破30亿美元，其“AI即服务”模式受企业采纳。
+ Opera公布Neon AI浏览器技术细节，内嵌Operator模型可自动化操作网页。
+ Meta计划让AI接管90%产品风险评估，引发员工担忧。
+ 谷歌DeepMind推出最强手语翻译模型SignGemma，打破沟通壁垒。

**关键词：** AI，模型，浏览器，风险评估，手语翻译

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/apW6YHPmOazOEZ4nYUU1ww)

## 5️⃣

🤖

图解Vllm V1系列4：加载模型权重

+ 文章探讨`vllm`加载模型权重，在`rlhf`训练中，了解其运作流很重要。
+ `load_model`整体流程为`Executor -> Worker.load_model() -> ModelRunner.load_model()`。
+ 核心函数`get_model`默认使用`DefaultModelLoader`，其`load_model`分初始化模型架构和实际加载权重两步。
+ 初始化模型架构时，`vllm`根据模型找到其在`vllm`中的实现并实例化。
+ 加载模型权重时，`_get_all_weights`生成权重迭代器，`model.load_weights`根据权重名和分布式配置读取权重。

**关键词：** `vllm`，模型权重，加载流程，`DefaultModelLoader`

🔗 文中引用的链接

+ [multiproc_executor.py#L245](https://github.com/vllm-project/vllm/blob/refs/tags/v0.8.2/vllm/v1/executor/multiproc_executor.py#L245)
+ [gpu_worker.py#L136](https://github.com/vllm-project/vllm/blob/refs/tags/v0.8.2/vllm/v1/worker/gpu_worker.py#L136)
+ [gpu_model_runner.py#L1173](https://github.com/vllm-project/vllm/blob/refs/tags/v0.8.2/vllm/v1/worker/gpu_model_runner.py#L1173)
+ [model_loader/__init__.py#L12](https://github.com/vllm-project/vllm/blob/refs/tags/v0.8.2/vllm/model_executor/model_loader/__init__.py#L12)
+ [loader.py#L435](https://github.com/vllm-project/vllm/blob/refs/tags/v0.8.2/vllm/model_executor/model_loader/loader.py#L435)
+ [loader.py#L110](https://github.com/vllm-project/vllm/blob/refs/tags/v0.8.2/vllm/model_executor/model_loader/loader.py#L110)
+ [registry.py#L528](https://github.com/vllm-project/vllm/blob/refs/tags/v0.8.2/vllm/model_executor/models/registry.py#L528)
+ [loader.py#L406](https://github.com/vllm-project/vllm/blob/refs/tags/v0.8.2/vllm/model_executor/model_loader/loader.py#L406)
+ [qwen.py#L297](https://github.com/vllm-project/vllm/blob/refs/tags/v0.8.2/vllm/model_executor/models/qwen.py#L297)

## 6️⃣

🤖

华为余承东：有些车卖爆了但产品不太好，尊

+ 余承东在第三届未来汽车先行者大会分享华为对智能汽车思考，称汽车行业在电动化、智能化等方面发生极大变化。
+ 华为可在智能化、电动、网联化等方面赋能车企，且在六大智能技术领域有信心做到最强。
+ 华为与车企合作有磨合，现鸿蒙智行已完成75万累计交付量，新发布的尊界S800或成现象级产品。
+ 尊界S800带动221家车企、零部件厂商高端化，销售对标100 - 300万车型，设计对标1000万以上产品。
+ 鸿蒙智行各“界”品牌需时间成长，相信都能成功。

**关键词：** 华为，智能汽车，鸿蒙智行，尊界S800，汽车行业变革

🔗 文中引用的链接

+ [原文链接](https://mp.weixin.qq.com/s/UJF2UeJhKYT4dGEzDEwaZA)
