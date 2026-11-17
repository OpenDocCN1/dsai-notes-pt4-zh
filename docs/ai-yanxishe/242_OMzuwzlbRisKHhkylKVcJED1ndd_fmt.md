
# AI+医疗

> 来源：[https://aiyanxishe.feishu.cn/docx/RWQ2dziDZozygsxKYJ2crFNln6c](https://aiyanxishe.feishu.cn/docx/RWQ2dziDZozygsxKYJ2crFNln6c)

<https://aiyanxishe.feishu.cn/sync/H4bBd6rOtsCpG9bjI0ZcDHxinnc>

![](img/2522b6c6febef5227547f2b1f812ea25.png)

![](img/bf198028f03fe6c7bde1d3e6a4f6d83e.png)

## AI+医疗应用

### GLM医生

+ 地址：[https://github.com/xionghonglin/DoctorGLM](https://github.com/xionghonglin/DoctorGLM)
+ 简介：基于`ChatGLM-6B`的中文问诊模型，通过中文医疗对话数据集进行参数，实现了包括`lora`、`p-tuningv2`等参数及配置

### 本草

+ 地址：[https://github.com/SCIR-HI/Huatuo-Llama-Med-Chinese](https://github.com/SCIR-HI/Huatuo-Llama-Med-Chinese)
+ 简介：开源了经过中文医学指令精调/指令调节(`Instruct-tuning`)的`LLaMA-7B`模型。通过医学知识图谱和`GPT3.5 API`构建了中文医学指令数据集，并在此基础上对`LLaMA`进行了指令扭矩，提高了`LLaMA`在医疗领域的快速效果。

![](img/4b68ece9a932e8e9a9395941afa24d8f.png)

### 扁鹊

+ 地址：[https://github.com/scutcyr/BianQue](https://github.com/scutcyr/BianQue)
+ 简介：一个经过指令与多轮问询对话联合调整的医疗对话大模型，基于`ClueAI/ChatYuan-large-v2`作为基础，使用中文医疗问答指令与多轮问询对话混合数据集进行调整。

![](img/7a288a9680836672418de25aa42bfd42.png)

### 华佗GPT

+ 地址：[https://github.com/FreedomIntelligence/HuatuoGPT](https://github.com/FreedomIntelligence/HuatuoGPT)
+ 简介：开源经过中文医学指令精调/指令调整(`Instruct-tuning`)的一个类似GPT的模型

![](img/cf4fdd8cd7b8ee1ce64bf0c1ae60ef28.png)

### Med-ChatGLM

+ 地址：[https://github.com/SCIR-HI/Med-ChatGLM](https://github.com/SCIR-HI/Med-ChatGLM)
+ 简介：基于中文医学知识的`ChatGLM`模型扭矩，扭矩数据与`BenTsao`相同。

### 七珍GPT

+ 地址：[https://github.com/CMKRG/QiZhenGPT](https://github.com/CMKRG/QiZhenGPT)
+ 简介：该项目利用启真医学知识库构建的中文医学指令数据集，并在此基础上在`LLaMA-7B`模型上进行指令精调，大幅提高了模型在中文医疗场景下的效果，首先针对药品知识问答发布了体育数据集，后续计划优化疾病、手术、检验等方面的问答效果，并针对医患问答、病历自动生成等应用拓展拓展。

### 聊天医学

+ 地址：[https://github.com/michael-wzhu/ChatMed](https://github.com/michael-wzhu/ChatMed)
+ 简介：该项目推出`ChatMed`系列中文医疗大规模语言模型，模型主干为`LlaMA-7b`并采用`LoRA`专用，具体包括`ChatMed-Consult`:基于中文医疗在线问诊数据集`ChatMed_Consult_Dataset`的50w+在线问诊回复诊+`ChatGPT`作为训练集；`ChatMed-TCM`:基于中医药指令数据集`ChatMed_TCM_Dataset`，以开源的中医药知识图谱为基础，采用以实体为中心的自指令方法(`entity-centric self-instruct`)，调用`ChatGPT`得到2.6w+的围绕中医药的指令数据训练得到。

### XrayGLM

首个会看监狱X光片的中文多模态医学大模型。

+ 地址：[https://github.com/WangRongsheng/XrayGLM](https://github.com/WangRongsheng/XrayGLM)
+ 简介：该项目为促进中文领域医学多模态大模型的研究发展，发布了`XrayGLM`数据集及模型，其在医学影像诊断和多轮交互对话上显示出非凡的潜力。

### MeChat

中文健康心理支持对话大模型。

+ 地址：[https://github.com/qiuhuachuan/smile](https://github.com/qiuhuachuan/smile)
+ 简介：该项目的中文心理健康支持通用开源模型由`ChatGLM-6B LoRA 16-bit`指令调节得到。数据集通过调用`gpt-3.5-turbo API`扩展真实的心理互助 QA 为多轮的心理健康支持多轮对话，提高了通用语言大模型在心理健康支持领域的表现，更加符合在长程多轮对话的应用场景。

### 医疗GPT

+ 地址：[https://github.com/shibing624/MedicalGPT](https://github.com/shibing624/MedicalGPT)
+ 简介：训练医疗大模型，实现包括二次预训练、有监督建模、强化学习训练。发布中文医疗`LoRA`模型`shibing624/ziya-llama-13b-medical-lora`，基于`Ziya-LLaMA-13B- v1`模型，`SFT`一个版本医疗模型，医疗问答效果有提升，发布后的`LoRA`权重。

### 孙思邈

+ 地址：[https://github.com/thomas-yanxin/Sunsimiao](https://github.com/thomas-yanxin/Sunsimiao)
+ 简介：`Sunsimiao`是一个开源的中文医疗大模型，该模型基于`baichuan-7B`和`ChatGLM-6B`基础模型在十万级高质量的中文医疗数据中驱动而得。

### 神农中医法学硕士

+ 地址：[https://github.com/michael-wzhu/ShenNong-TCM-LLM](https://github.com/michael-wzhu/ShenNong-TCM-LLM)
+ 简介：该项目开源了神农中医药大规模语言模型，该模型以`LlaMA`为基础，采用`LoRA (rank=16)`调节器。调节代码与`ChatMed`代码库相同。此外该项目还开源了中医药指令剂量数据集。

### 灵魂聊天

+ 地址：[https://github.com/scutcyr/SoulChat](https://github.com/scutcyr/SoulChat)
+ 简介：该项目了经过百万规模心理咨询领域中文长文本指令与多轮共情对话数据联合指令调节的心理健康大模型灵心（`SoulChat`），该模型以`ChatGLM-6B`作为初始化模型，进行了了全部参数的参数配置。

### 护理GPT

+ 地址：[https://github.com/WangRongsheng/CareGPT](https://github.com/WangRongsheng/CareGPT)
+ 简介：该项目提供了开放可用的医疗护理数据集和开放可用的医疗大语言模型，包含LLM的培训、评估、部署等以促进医疗`LLM`快速发展。

### DISC医学法学硕士

+ 地址：[https://github.com/FudanDISC/DISC-MedLLM](https://github.com/FudanDISC/DISC-MedLLM)
+ 简介：该项目是由复旦大学发布的针对医疗健康对话式场景并设计的医疗领域大模型与数据集，该模型由基于`Baichuan-13B-Base`指令得到的`DISC-Med-SFT`数据集。

### 太乙-法学硕士

+ 地址：[https://github.com/DUTIR-BioNLP/Taiyi-LLM](https://github.com/DUTIR-BioNLP/Taiyi-LLM)
+ 简介：该项目由大连理工大学信息检索训练研究室开发的中英双语医学大模型“太一”，收集整理了丰富的中英双语生物医学自然语言处理（`BioNLP`）语料，总共包含38个中文数据集，通过丰富的中英文任务指令数据（超过100W条样本）进行大模型（`Qwen-7B-base`）指令配置，使模型具备了出色的中英文生物医学智能问答、医患对话、报告生成、信息抽取、机器翻译、标题生成、文本分类等多种`BioNLP`能力。

### 无线NGPT

+ 地址：[https://github.com/winninghealth/WiNGPT2](https://github.com/winninghealth/WiNGPT2)
+ 简介：`WiNGPT`训练是一个基于`GPT`的医疗垂直领域大模型，基于`Qwen-7b1`作为基础预模型，这里技术上进行了继续预训练，指令调节等，该项目具体开源了`WiNGPT2-7B-Base`与`WiNGPT2 -7B-聊天模型`。

### 和黄医药-GPT

+ 地址：[https://github.com/synlp/ChiMed-GPT](https://github.com/synlp/ChiMed-GPT)
+ 简介：`ChiMed-GPT`是一个开源中文医学大语言模型，通过在中文医学数据上持续训练`Ziya-v2`构建而成，其中涵盖了预训练、有监督角色（`SFT`）和来自人类反馈的强化学习（`RLHF`）等训练过程。

### 心灵聊天

+ 地址：[https://github.com/XD-Lab/MindChat](https://github.com/XD-Lab/MindChat)
+ 简介：心理大模型——漫谈(`MindChat`)希望从心理咨询、心理评估、心理诊断、心理治疗四个维度帮助人们解心理压力与解决心理困惑，为用户提供隐私、温暖、安全、及时、方便`MindChat`是一个基于`Qwen`作为基础的预训练模型，并在此基础上进行指令以获得的心理垂域大模型。

## AI问诊机器人

可以帮助医院和诊所提高医疗服务质量和效率，减轻医生的工作压力，同时也为患者提供了更便捷、高效的医疗服务体验。具体的应用场景可能会因医院和诊所的需求和特点而有所不同。

+ **智能问诊**：患者可以通过语音或文字与 AI 问诊机器人交流，描述症状、疾病等问题，获取初步的诊断建议和治疗方案。

+ **辅助诊断**：医生可以参考 AI 问诊机器人提供的信息，结合自己的专业知识进行更准确的诊断。

+ **患者教育**：AI 问诊机器人可以向患者提供疾病预防、治疗、康复等方面的知识，提高患者的健康意识和自我管理能力。

+ **药物咨询**：患者可以咨询药物的用法、剂量、副作用等信息，确保用药安全。

+ **预诊分诊**：AI 问诊机器人可以根据患者的症状和病情，为其提供合适的科室和医生推荐，提高就诊效率。

+ **医疗知识库**：医生可以通过 AI 问诊机器人访问丰富的医疗知识库，获取最新的医学知识和临床经验。

## AI+医疗公司

![](img/108c2a347c7926d1a9bcef72326b7d64.png)

![](img/9c36c6b8ced66b70914dd4fa936a2c50.png)

![](img/88429dce1f08125b43c56e65b54460d3.png)

家庭健康医生

`detail.tmall.com`

左手医生

左手医生-打造主动式AI 让优质医疗触手可及。

![](img/a8b7167f7da4af7c5651cb18115ea6a1.png)

![](img/eb348d5a6e427521592c89ff9b82bda4.png)

易普森

易普森官网-智能医疗服务机器人-智慧医疗AI解决方案。

穿山甲智能AI问诊导诊机器人

穿山甲智能AI问诊导诊机器人C位出道

![](img/c0f6cd5b657ef5e3644927035497fdec.png)

![](img/babeb117075279dd3104022ea2f70648.png)

**泰迪智慧科技有限公司**

联系人：刘秋琦

手机：13264708894

妙伴AI智能机器人健康机器人在线问诊视屏通

AI皮肤检测

https://item.taobao.com/item.htm?abbucket=16&id=663768004827&ns=1&spm=a21n57.1.0.0.3707523cexKfo8

![](img/67ec996050bbaa3f3d389fffb477faa6.png)

![](img/999647e81476ffd4f449821423a3d1be.png)

AI人工智能问诊

👆 扫码加入AI研习社 👆

踩在人工智能浪潮最前沿

欢迎积极入群，踩在浪潮最前沿

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>
