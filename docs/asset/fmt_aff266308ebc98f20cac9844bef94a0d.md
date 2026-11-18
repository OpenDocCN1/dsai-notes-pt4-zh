
`RefuelLLM-2`是为数据标签化、清洗和丰富专门构建的大型语言模型的新版本，比所有现有顶尖模型都要优秀，包括在约30个数据标签任务中超越了`GPT-4-Turbo`和`Claude-3-Opus`。该模型基于`Mixtral-8x7B`基模型，训练了一个覆盖多种任务（如分类、阅读理解、结构化属性提取和实体解析）的2750多个数据集。`RefuelLLM-2`小型版也展示了优越性，被开源并可通过`Hugging Face`获取。`Refuel`采用自动标签库`Autolabel`对这些模型进行了测试，显示出在非公开数据集上的出色泛化能力，特别是在处理长文本输入时的强大表现。

![](img/f06b7387e2f4b407029457e5cef3b819.png)

## `Unsloth` x `Qwen2`，提速47.32%，节省39.13%显存，最少仅需8.43GB显存

贡献人：

`https://mp.weixin.qq.com/s/x2N3p1qgJy_RyRsO2PHS_A`

`Unsloth`是一个训练框架，通过优化梯度计算、链式矩阵乘法等手段，大幅提升训练速度，降低显存占用。最新版本支持`Qwen2`模型，可在一张`1080Ti`上训练`Qwen1.5-7B`，提速47.32%，显存占用减少39.13%。通过`Firefly`项目和`Unsloth`对`Qwen1.5-7B`的SFT和DPO两阶段训练，在🤗`Open LLM Leaderboard`上取得优异表现。未来，开源社区有望进一步完善`Unsloth`，推动大模型训练成本和速度的进一步提升。

![](img/24fad67d9aef6871d079653fec98e653.png)

## 使用`GGML`和`LLAMA-CPP`对`LLAMA`模型进行量化

贡献人：

`https://towardsdatascience.com/quantize-llama-models-with-ggml-and-llama-cpp-3612dfbcc172`

`GGML`和`LLAMA-CPP`是两个工具，可用于对`LLAMA`模型进行量化，从而减小模型尺寸、提高推理速度。`GGML`提供了一种简单的方法，可以将`PyTorch`模型转换为`TFLite`格式，并在`TFLite`模型上运行`Post Training Quantization（PTQ）`流程。`LLAMA-CPP`是一个用`C++`实现的`LLAMA`推理引擎，支持`INT8`量化推理，并提供用于加载和运行`LLAMA`模型的`API`。通过`GGML`和`LLAMA-CPP`的结合使用，用户可以方便地将`LLAMA`模型转换为`INT8`量化版本，并在嵌入式设备上进行高效推理。

![](img/e11bcad30ed13e15dcf17bb7fefddd7e.png)

## `飞腾`、`鲲鹏`、`海光`、`龙芯`、`兆芯`、`申威`国产`CPU`处理器

贡献人：

`https://zhuanlan.zhihu.com/p/609496571?utm_psn=1771518998435684352`

国产`CPU`处理器领域中，`飞腾`、`鲲鹏`、`海光`、`龙芯`、`兆芯`、`申威`等厂商正推动“中国芯”的发展。`飞腾`基于`ARM`架构，提供高性能服务器和桌面`CPU`，支持`PK`生态；`鲲鹏`为华为旗下，以`ARM V8`架构实现端边云算力同构；`海光`与`AMD`合作，开发`X86`架构`CPU`，性能领先；`龙芯`完全自主研发，基于`MIPS`架构；`兆芯`是`X86`架构下的合资企业，产品性能较好；`申威`基于`Alpha`架构，专注于超级计算。各厂商正通过技术创新和生态建设，提升产品性能，增强信息安全，逐步减少对外部技术的依赖。

![](img/95a754e2d651a7b255e33a5250673f43.png)

## 优化微调语言模型的`GPU`利用率

贡献人：

`https://jillanisofttech.medium.com/optimizing-gpu-utilization-for-fine-tuned-language-models-a-comprehensive-guide-d681743a4a00`

该文章详细介绍了如何优化大型语言模型（`LLMs`）的`GPU`利用率，特别是通过适配器模块和低秩适配器（`LoRA`）技术，以减少计算成本并提高模型性能。`LoRA`技术通过对模型权重进行小规模且易于管理的矩阵修改来实现这一目标，有效减少了计算开销，同时保持了模型性能。通过在单一基础模型中集成多个适配器，可以在共享`GPU`资源上实现高效的多租户部署，智能地将请求路由到适当的适配器，从而优化资源使用，减少成本，并保持灵活性和可扩展性。此外，文章还强调了动态资源分配和适配器的监控调整在维持运营效率和高客户满意度中的重要性。

![](img/f7bcdc5ae48eb03f093d5236b504ec0a.png)

## 单节点8x`A800`跑起来`HuggingFace` `DeepSeek V2`踩坑

贡献人：

`https://zhuanlan.zhihu.com/p/696792195?utm_psn=1771933567855411200`

该文章是关于在单节点8x`A800`上运行`HuggingFace` `DeepSeek V2`时遇到的一些问题和解决方法的总结。作者首先介绍了在运行`DeepSeek V2`时遇到的两个报错情况，以及对应的解决方法。其次，作者分享了在使用`transformers`库进行模型推理时遇到的程序卡死问题，并给出了优化代码的解决方案。文章还提到了在单节点`A800`推理时需要限制输出长度以避免内存不足的问题，并给出了相应的调试建议和实测结果。最后，作者提到了如果想在单节点8卡`A100`/`A800`上加速推理并且输出更长长度的文本，可以使用`vllm`的实现。文章突出了技术细节和解决问题的方法，对于在实际应用中遇到类似问题的读者具有一定的参考价值。

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

`ChatGPT`；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（`HuggingFace`）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报知识库】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/d8e7c1036ac717062a12b8b347042b96.png)
