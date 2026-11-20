
# ChatGLM Papers

> 来源：[https://learning.feishu.cn/docx/XHBZdNyRpokWWuxhn8vcr7Sgn4c](https://learning.feishu.cn/docx/XHBZdNyRpokWWuxhn8vcr7Sgn4c)

## Links

`https://huggingface.co/THUDM/chatglm-6b`

`https://github.com/THUDM/ChatGLM2-6B`

基座模型 `https://github.com/THUDM/GLM`

GLM: General Language Model Pretraining with Autoregressive Blank Infilling

`https://arxiv.org/abs/2103.10360`

GLM-130B: An Open Bilingual Pre-trained Model

`https://arxiv.org/abs/2210.02414`

有 2 个模型

`ChatGLM-6B`
`ChatGLM2-6B`

+ 使用了 [`GLM`](https://github.com/THUDM/GLM) 的混合目标函数
+ [`FlashAttention`](https://github.com/HazyResearch/flash-attention) 把 Context Length 从 2K 扩展到了 32K。对于更长的上下文，我们发布了 [`ChatGLM2-6B-32K`](https://huggingface.co/THUDM/chatglm2-6b-32k) 模型
+ Multi-Query Attention 推理速度提升 42%，INT4 量化下，6G 显存支持的对话长度由 1K 提升到了 8K。

**[2023/07/31]** 发布 [`ChatGLM2-6B-32K`](https://huggingface.co/THUDM/chatglm2-6b-32k) 模型，提升对于长文本的理解能力。

**[2023/07/25]** 发布 [`CodeGeeX2`](https://github.com/THUDM/CodeGeeX2) 模型，基于 ChatGLM2-6B 加入代码预训练实现，代码能力全面提升。

**[2023/07/04]** 发布 [`P-Tuning v2`](https://github.com/THUDM/ChatGLM2-6B/blob/main/ptuning) 与 全参数微调脚本，参见 P-Tuning。

对 ChatGLM2 进行加速的开源项目：

+ [`fastllm`](https://github.com/ztxz16/fastllm/): 全平台加速推理方案，单GPU批量推理每秒可达10000+token，手机端最低3G内存实时运行（骁龙865上约4~5 token/s）
+ [`chatglm.cpp`](https://github.com/li-plus/chatglm.cpp): 类似 `llama.cpp` 的 CPU 量化加速推理方案，实现 Mac 笔记本上实时对话
+ [`ChatGLM2-TPU`](https://github.com/sophgo/ChatGLM2-TPU): 采用TPU加速推理方案，在算能端侧芯片BM1684X（16T@FP16，内存16G）上实时运行约5 token/s

基于或使用了 ChatGLM2-6B 的开源项目：

+ [`Chuanhu Chat`](https://github.com/GaiZhenbiao/ChuanhuChatGPT): 为各个大语言模型和在线模型API提供美观易用、功能丰富、快速部署的用户界面，支持ChatGLM2-6B。

支持 ChatGLM-6B 和相关应用在线训练的示例项目：

+ ChatGLM2-6B 的部署与微调教程

## Paper Reading

Pretrain model 的 3 个 framework

+ Autoencoding: Bert
+ Autoregressive: GPT
+ Encoder-decoder models: T5

本文是 autoregressive，2 点改进：

1. Adding 2D positional encodings
1. Allowing an arbitrary order to predict spans

Model architecture，基于 transformer 的 3 个改变

1. Rearrange the order of `layer norm` and residual connection. Critical to avoid numerical errors.
1. Single linear layer for output token prediction
1. Replace `ReLU` with `GeLUs`

2D positional encoding

Each token is encoded with 2 positional ids.

1. Xxx
1. Xxx

Projected into 2 vectors via learnable embedding tables, which are both added to the input token embeddings.

Masked token 数量是未知的，不会泄漏信息。 XLnet, SpanBERT 都会泄漏。

模型里面看到的大量的 `diff` & `reshape`，是在做这个 positional embedding？

## 结构推导

从代码上看，`SelfAttention` 结构如下，里面先做了 position encoding，然后是 attention。

`onnx` 图里的一大坨，可能就是 position encoding 的东西。chatGLM 的 position encoding 比较特殊，是 2d 的，其他模型是 1d。

![](img/edec66c0c6f1b23e7ab42ad7c81730c0.png)
