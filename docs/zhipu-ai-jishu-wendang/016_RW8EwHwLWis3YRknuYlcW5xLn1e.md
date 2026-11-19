
# GLM-4-Voice技术文档

> 来源：[https://ai.feishu.cn/docx/OknSdJwStoT1AfxZBjMc9FnHn3g](https://ai.feishu.cn/docx/OknSdJwStoT1AfxZBjMc9FnHn3g)

+   Github 官方仓库：https://github.com/THUDM/GLM-4-Voice

+   技术支持：请到 [github issue](https://github.com/THUDM/GLM-4-voice/issues) 提问

## 模型介绍

GLM-4-Voice 是智谱推出的端到端语音模型。GLM-4-Voice 能够直接理解和生成中英文语音，进行实时语音对话，并且能够遵循用户的指令要求改变语音的情感、语调、语速、方言等属性。

### 模型架构

![](img/bcfa24b17b2c7a0f410772f8357edf68.png)

GLM-4-Voice 由三个部分组成：

+   [GLM-4-Voice-Tokenizer](https://github.com/openai/whisper): 通过在 Whisper 的 Encoder 部分增加 Vector Quantization 并在 ASR 数据上有监督训练，将连续的语音输入转化为离散的 token。每秒音频平均只需要用 12.5 个离散 token 表示。

+   [GLM-4-Voice-Decoder](https://github.com/FunAudioLLM/CosyVoice): 基于 CosyVoice 的 Flow Matching 模型结构训练的支持流式推理的语音解码器，将离散化的语音 token 转化为连续的语音输出。最少只需要 10 个语音 token 即可开始生成，降低端到端对话延迟。

+   [GLM-4-Voice-9B](https://github.com/THUDM/GLM-4): 在 GLM-4-9B 的基础上进行语音模态的预训练和对齐，从而能够理解和生成离散化的语音 token。

预训练方面，为了攻克模型在语音模态下的智商和合成表现力两个难关，我们将 Speech2Speech 任务解耦合为“根据用户音频做出文本回复”和“根据文本回复和用户语音合成回复语音”两个任务，并设计两种预训练目标，分别基于文本预训练数据和无监督音频数据合成语音-文本交错数据以适配这两种任务形式。GLM-4-Voice-9B 在 GLM-4-9B 的基座模型基础之上，经过了数百万小时音频和数千亿 token 的音频文本交错数据预训练，拥有很强的音频理解和建模能力。

对齐方面，为了支持高质量的语音对话，我们设计了一套流式思考架构：根据用户语音，GLM-4-Voice 可以流式交替输出文本和语音两个模态的内容，其中语音模态以文本作为参照保证回复内容的高质量，并根据用户的语音指令要求做出相应的声音变化，在最大程度保留语言模型智商的情况下仍然具有端到端建模的能力，同时具备低延迟性，最低只需要输出 20 个 token 便可以合成语音。

更详细的技术报告将在之后公布。

## 快速阅览

我们在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### [快速使用](https://zhipu-ai.feishu.cn/wiki/H7yhwWsl5iMZuMkQP7JcsCU4nEc)

+   环境配置，模型下载
+   基础的模型推理

### [常见问题](https://zhipu-ai.feishu.cn/wiki/C5QDwoelfiwCJukYil2clj3HnLf)

+   使用 GLM-4-Voice 开源模型遇见的常见问题。
+   如何为项目做出贡献。
+   项目更新计划

## 📚 信息汇总

官方网站：https://www.zhipuai.cn/

智谱清言：https://chatglm.cn

API开放平台：https://bigmodel.cn/

Github仓库：https://github.com/THUDM/GLM-4-Voice

## 👍 GLM-4-Voice 体验： https://bigmodel.cn/trialcenter

---

## 🏠 👸号

![](img/7b26847a032092abef78f1455a6092f3.png)

## ❤️ 交流群

![](img/4217698dacd75637fb84b6a6ed094988.png)

---
