
# Qwen1.5模型高效微调

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/G1c0dB1veoxpcWxXJkccASMin1g](https://kq4b3vgg5b.feishu.cn/docx/G1c0dB1veoxpcWxXJkccASMin1g)

## 实操部分

```
from modelscope import snapshot_download
from transformers import AutoModelForCausalLM, AutoTokenizer
```

```
2024-04-18 19:16:45,535 - modelscope - INFO - PyTorch version 2.1.2+cu121 Found.
2024-04-18 19:16:45,537 - modelscope - INFO - TensorFlow version 2.14.0 Found.
2024-04-18 19:16:45,537 - modelscope - INFO - Loading ast index from /mnt/workspace/.cache/modelscope/ast_indexer
2024-04-18 19:16:45,537 - modelscope - INFO - No valid ast index found from /mnt/workspace/.cache/modelscope/ast_indexer, generating ast index from prebuilt!
2024-04-18 19:16:45,856 - modelscope - INFO - Loading done! Current index file version is 1.13.3, with md5 55e7043102d017111a56be6e6d7a6a16 and a total number of 972 components indexed
/opt/conda/lib/python3.10/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
  from .autonotebook import tqdm as notebook_tqdm
```

```
qwen18_dir=snapshot_download("Qwen/Qwen1.5-1.8B-Chat")
```

```
Downloading: 100%|██████████| 662/662 [00:00<00:00, 4.81MB/s]
Downloading: 100%|██████████| 51.0/51.0 [00:00<00:00, 293kB/s]
Downloading: 100%|██████████| 206/206 [00:00<00:00, 1.42MB/s]
Downloading: 100%|██████████| 7.11k/7.11k [00:00<00:00, 19.1MB/s]
Downloading: 100%|██████████| 1.59M/1.59M [00:00<00:00, 63.9MB/s]
Downloading: 100%|█████████▉| 3.42G/3.42G [00:08<00:00, 425MB/s]
Downloading: 100%|██████████| 4.15k/4.15k [00:00<00:00, 12.4MB/s]
Downloading: 100%|██████████| 6.70M/6.70M [00:00<00:00, 212MB/s]
Downloading: 100%|██████████| 1.26k/1.26k [00:00<00:00, 12.5MB/s]
Downloading: 100%|██████████| 2.65M/2.65M [00:00<00:00, 34.8MB/s]
```

```
from transformers import AutoModelForCausalLM, AutoTokenizer
device = "cuda" # the device to load the model onto

model = AutoModelForCausalLM.from_pretrained(
    qwen18_dir,
    torch_dtype="auto",
    device_map="auto"
)
tokenizer = AutoTokenizer.from_pretrained(qwen18_dir)
```

```
Special tokens have been added in the vocabulary, make sure the associated word embeddings are fine-tuned or trained.
```

```
prompt = "你好，请介绍下你自己。"
messages = [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": prompt}
]
text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True
)
model_inputs = tokenizer([text], return_tensors="pt").to(device)
```

```
generated_ids = model.generate(
    model_inputs.input_ids,
    max_new_tokens=512
)
generated_ids = [
    output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
]

response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
```

```
print(response)
```

```
您好！我是一个大型语言模型，由阿里云开发和维护。我被设计为一个自然语言处理（NLP）语言模型，能够理解和生成人类语言的文本。我被集成到阿里云的超大规模语言模型平台，可以回答各种问题、写故事、撰写代码、翻译文本等多种任务，并具有强大的知识库和精准的上下文理解能力。

我可以进行以下方面的工作：

1. 问答：根据用户的提问，如天气预报、股票行情、电影推荐等，通过预训练的深度学习模型，快速生成准确的答案或提供相关信息。

2. 文本生成：基于用户提供的输入，我可以自动生成与之相关的高质量文本，如新闻报道、科技文章、小说情节等。

3. 编码和转换：支持多种编程语言的代码自动完成，例如Python、Java、C++等，也可以将文本从一种语言转译成另一种语言。

4. 文本摘要：对较长的文章或文档进行简短的概括，提取出关键信息和主题观点，帮助用户快速了解文章内容。

5. 文本分类：将文本根据其主题、情感倾向或其他属性进行分类，如新闻事件、人物评述、情绪分析等。

6. 自然语言处理对话：能够模拟人类对话，回答用户的指令和问题，具备良好的语义理解能力和交互体验。

7. 多轮对话系统：支持多轮交互，能够理解并回答用户提出的复杂问题，提高系统的响应速度和灵活性。

8. 语音识别：对于音频数据，可以实现文本转语音的功能，帮助用户通过语音与计算机进行交互。

9. 舆情分析：通过对社交媒体上文本的分析，获取用户情绪、热点话题、趋势等内容，帮助企业了解市场动态和社会舆情。

10. 知识图谱构建：通过构建知识图谱，提供知识检索、推理、解析等功能，支持用户在海量信息中快速找到所需的知识点。

以上是我作为一个人工智能助手的主要功能和应用场景，随着技术的进步和数据的增长，我的能力将会不断提升，为用户提供更高效、更个性化的服务。如果您有任何关于我或者NLP领域的问题，欢迎随时向我提出！
```

### 一、大模型微调概念入门介绍

在完成了Qwen1.5模型的快速部署之后，接下来我们尝试围绕Qwen1.5大模型进行微调。

所谓微调，通俗理解就是围绕大模型进行参数修改，从而永久性的改变模型的某些性能。而大模型微调又分为全量微调和高效微调两种，所谓全量微调，指的是调整大模型的全部参数，而高效微调，则指的是调整大模型的部分参数，目前常用的高效微调方法包括LoRA、QLoRA、p-Tunning、Prefix-tunning等。而只要大模型的参数发生变化，大模型本身的性能和“知识储备”就会发生永久性改变。在通用大模型往往只具备通识知识的当下，为了更好的满足各类不同的大模型开发应用场景，大模型微调已几乎称为大模型开发人员的必备基础技能。

> 本节体验课以微调实操为主，更多微调基本概念介绍可以参考我的公开课：https://www.bilibili.com/video/BV1Yc411g78a

![](img/18acfa9cad789995fd53d67e70524f62.png)

> [在《大模型技术实战课》（第3期](https://appZe9inzwc2314.h5.xiaoeknow.com)）课程中，我们也会详细介绍各微调算法的基本原理。

而在过去的一年，伴随着大语言模型技术的飞速发展，微调这一原本诞生于深度学习技术领域的方法，在大模型技术领域的应用得到了长足的发展。简单来说，目前微调的应用面得到了极大程度拓展、微调的门槛大幅降低、同时微调的各项基本数据集支持也更加友好。

+   大模型微调应用

![](img/1747f45085af00f4efba6c9c85b0306f.png)

+   微调技术门槛大幅降低

![](img/bc03e005fa47bd2a496f6e91a9984083.png)

> LlaMa-Factory项目地址：https://github.com/hiyouga/LLaMA-Factory

> SWIFT项目地址：https://github.com/modelscope/swift

🍻现开设了大**模型学习交流群，**扫描下👇码，来遇见更多志同道合的小伙伴~

![](img/47289776f8c1987c6d9b928536194556.png)

海量硬核独家技术**干货内容**+无门槛**技术交流**+不定期开设**硬核干货&前沿技术公开课**，**扫码**👆即刻入群！

### 二、基于LlaMa-Factory的高效微调流程

#### 1. LLaMA-Factory项目介绍

LLaMA Factory是一个在GitHub上开源的项目，该项目给自身的定位是：提供一个易于使用的大语言模型（LLM）微调框架，支持LLaMA、Baichuan、Qwen、ChatGLM等架构的大模型。更细致的看，该项目提供了从预训练、指令微调到RLHF阶段的开源微调解决方案。截止目前（2024年3月1日）支持约120+种不同的模型和内置了60+的数据集，同时封装出了非常高效和易用的开发者使用方法。而其中最让人喜欢的是其开发的LLaMA Board，这是一个零代码、可视化的一站式网页微调界面，它允许我们通过Web UI轻松设置各种微调过程中的超参数，且整个训练过程的实时进度都会在Web UI中进行同步更新。

简单理解，通过该项目我们只需下载相应的模型，并根据项目要求准备符合标准的微调数据集，即可快速开始微调过程，而这样的操作可以有效地将特定领域的知识注入到通用模型中，增强模型对特定知识领域的理解和认知能力，以达到“通用模型到垂直模型的快速转变”。

LLaMA-Factory目前支持微调的模型及对应的参数量：

| Model | Sizes |
| --- | --- |
| Baichuan2 | 7B/13B |
| BLOOM | 560M/1.1B/1.7B/3B/7.1B/176B |
| BLOOMZ | 560M/1.1B/1.7B/3B/7.1B/176B |
| ChatGLM3 | 6B |
| DeepSeek (MoE) | 7B/16B/67B |
| Falcon | 7B/40B/180B |
| Gemma | 2B/7B |
| InternLM2 | 7B/20B |
| LLaMA | 7B/13B/33B/65B |
| LLaMA-2 | 7B/13B/70B |
| Mistral | 7B |
| Mixtral | 8x7B |
| Phi-1.5/2 | 1.3B/2.7B |
| Qwen | 1.8B/7B/14B/72B |
| Qwen1.5 | 0.5B/1.8B/4B/7B/14B/72B |
| XVERSE | 7B/13B/65B |
| Yi | 6B/34B |
| Yuan | 2B/51B/102B |

可以看到，当前主流的开源大模型，包括ChatGLM3、Qwen的第一代以及最新的1.5测试版本，还有Biachuan2等，已经完全支持不同规模的参数量。针对LLaMA架构的系列模型，该项目已经基本实现了全面的适配。而其支持的训练方法，也主要围绕（增量）预训练、指令监督微调、奖励模型训练、PPO 训练和 DPO 训练展开，具体情况如下：

| 方法 | 全参数训练 | 部分参数训练 | LoRA | QLoRA |
| --- | --- | --- | --- | --- |
| 预训练（Pre-Training） | ✅ | ✅ | ✅ | ✅ |
| 指令监督微调（Supervised Fine-Tuning） | ✅ | ✅ | ✅ | ✅ |
| 奖励模型训练（Reward Modeling） | ✅ | ✅ | ✅ | ✅ |
| PPO 训练（PPO Training） | ✅ | ✅ | ✅ | ✅ |
| DPO 训练（DPO Training） | ✅ | ✅ | ✅ | ✅ |

+   **预训练**

在大模型的早期阶段，也就是诞生的过程中，往往不会经过Supervised finetuning（监督式微调） 的过程，它在训练时仅仅是去学习大量的语言基本规律和知识理解，其训练数据是类似这种形式：

```
山西警方扫黑除恶行动集中收网打掉涉黑涉恶犯罪组织和团伙54个【消息】 本报太原2月14日讯（记者 雷清明）2月13日，山西省公安厅对外发布消息称，扫黑除恶专项行动启动以来，全省各级公安机关迅速对一大批在侦在办的涉黑涉恶案件实施了集中收网。截至目前，全省公安机关共打掉涉黑涉恶犯罪组织和团伙54个，其中，打掉涉嫌黑社会性质组织4个，打掉恶势力犯罪组织50个。在54个涉黑涉恶犯罪组织态势，不断形成对黑恶势力违法犯罪的凌厉攻势；将进一步摸排深挖线索，进一步增强主动发现犯罪的能力和水平；将进一步扩大扫黑除恶的社会宣传覆盖面，营造对黑恶势力同仇敌忾、人人喊打的浓厚氛围。

近年来，单片机以其体积小、价格廉、面向控制等独特优点，在各种工业控制、仪器仪表、设备、产品的自动化、智能化方面获得了广泛的应用。与此同时，单片机应用系统的可靠性成为人们越来越关注的重要课题。影响可靠性的因素是多方面的，如构成系统的元器件本身的可靠性、系统本身各部分之间的相互耦合因素等。其中系统的抗干扰性能是系统可靠性的重要指标。 ·加电、掉电以及供电电压下降情况下的复位输出，复位脉冲宽度典型值为200 ms。 ·独立的看门狗输出，如果看门狗输入在1．6 s内未被触发，其输出将变为高

...
```

> 上述数据截取自：Skywork：https://huggingface.co/datasets/Skywork/SkyPile-150B （注：需要开启🪜才可以访问）

预训练数据通常来自于多样化的文本来源，以覆盖尽可能广泛的主题、语境和语言风格。这些数据可以是结构化的文本，也可以是非结构化的，比如新闻文章、科学文献等等，对于基于这样训练数据训练得到的模型，很难听懂人类下达的具体指令从而去完成复杂的任务，更多的是使用其文本的生成能力。

**  我们所一直提及的微调**，从参数规模来看，大体上可以分为全参数微调和高效参数微调。全参数微调通常以预训练模型的初始权重为基础，在特定数据集上继续训练，更新模型的所有参数。相比之下，高效参数微调旨在使用更少的资源来更新模型参数，这包括仅更新部分参数或通过对参数施加某些结构化约束来实现。后者就涵盖了我们在之前课程中提到的高效微调技术，如PEFT、LoRA和QLoRA等方法。

另一方面，如果按照在大模型哪个阶段使用微调，或者根据模型微调的目标来区分，也可以从提示微调、指令微调、有监督微调的方式来具体分类。

+   **指令微调**

指令微调，**是**一种通过在由（指令，输出）对组成的数据集上进一步训练LLMs的过程。其中，指令代表模型的人类指令，输出代表遵循指令的期望输出。这个过程有助于弥合LLMs的下一个词预测目标与用户让LLMs遵循人类指令的目标之间的差距。它可以被视为有监督微调（Supervised Fine-Tuning，SFT）的一种特殊形式。但是，它们的目标依然有差别。SFT是一种使用标记数据对预训练模型进行微调的过程，以便模型能够更好地执行特定任务。而指令微调是一种通过在包括（指令，输出）对的数据集上进一步训练大语言模型（LLMs）的过程，以增强LLMs的能力和可控性。指令微调的特殊之处在于其数据集的结构，即由人类指令和期望的输出组成的配对。这种结构使得指令微调专注于让模型理解和遵循人类指令。
