
# 模型微调

> 来源：[https://ai.feishu.cn/docx/BhlIdbxPzoDqUSxyfEictU0rn0e](https://ai.feishu.cn/docx/BhlIdbxPzoDqUSxyfEictU0rn0e)

为了能将`CogVideoX`视频生成相关生态社区更加活跃，优化生成模型的效果是一个非常重要的方向，我们将介绍`CogVideoX`模型微调的场景，进一步增加分发自由度，对没有编程基础的用户进行一些有好的封装。

微调`CogVideoX`需要的资源,按照微调方式分为`SFT[`Lora`微调`CogVideoX`-风格对齐](https://zhipu-ai.feishu.cn/wiki/OjIDwMEKniIby1kHQa4cMKibnhP)` `lora`微调，同时我们为保证结果复现，公开了一些数据处理的脚本，在更加垂类的场景上完成某些风格对齐，增加消融实验指导用户在创建不同样本的微调任务中，减少复现微调任务的难度。

## Diffusers相关

🤗 `Diffusers`框架是huggface在扩散模型相关的开源方案，通过模块工具，可以做到方便快捷的集成自定义框架，在模型训练方向`Diffusers`有`accelerate`加速支持，兼容常见的推理框架

+   使用手册 [h](https://huggingface.co/docs/diffusers/using-diffusers/cogvideox)ttps://huggingface.co/docs/diffusers/using-diffusers/cogvideox

+   训练手册 [h](https://huggingface.co/docs/diffusers/training/cogvideox)ttps://huggingface.co/docs/diffusers/training/cogvideox

在视频评测维度的实际测试发现，我们的`CogVideoX`在主题一致、动态信息一致、背景一致、物体信息一致、运动流畅、色彩、场景、外观风格、时间风格等维度上取得了不错的效果，但在人体动作、空间关系、多物体等维度上无法取得很好的效果。

通过[`Lora`微调`Diffusers` 示例](https://zhipu-ai.feishu.cn/wiki/XWdgwnzu2iHXJmkLv4wc6vRXnyg)微调训练弥补这方面的差距，对效果欠缺的数据集通过`Diffusers`框架进行训练。

## SAT相关

`sat` ( [`S`](https://github.com/THUDM/SwissArmyTransformer)wissArmyTransformer ) 是一个灵活而强大的库，可用于开发您自己的 `Transformer` 变体。

[`SAT` 进行模型微调](https://zhipu-ai.feishu.cn/wiki/ZluWwNNC1iB46TkrWoMc7IGfnud)
