
# T8 Comfyui进阶教程(第14集)-Flux最新Controlnet 3大模型详细测评含速度、参数、效果及改版速度提升20倍的工作流，支持显卡分层加载技术

> 来源：https://agijuejin.feishu.cn/docx/TIoIdwRwmoWlx8xLUx7c8rktnTc

## 👀快看👀

> **作者：**[**T8star-Aix**](https://space.bilibili.com/385085361)

> **原文链接：**[**T8 Comfyui进阶教程(第14集)-Flux最新Controlnet 3大**](https://www.bilibili.com/video/BV174WAexEB8/?spm_id_from=333.788&vd_source=5a54577a6bc7d876f6e5df88640c8e9f)模型详细测评含速度、参数、效果及改版速度提升20倍的工作流，支持显卡分层加载技术

## ⚡️摘要⚡️

> 关于 Flux 中最新的 Controlnet V3 模型的详细测评，包括速度、参数、效果的对比，以及如何通过改版工作流提升速度，并介绍了支持显卡分层加载技术。

## 🤖分集🤖

### T8 Comfyui进阶教程(第14集)-Flux最新Controlnet 3大模型详细测评含速度、参数、效果及改版速度提升20倍的工作流，支持显卡分层加载技术

测评 Flux 中三个更新到 V3 的 Controlnet 模型。介绍 `x level` 节点更新，调整工作流测试不同模型，发现 `NF4` 人像还原好但卡通和建筑图像效果待提升。测试 `DEPI` 的 `DEPX` 工作流并改进，测试 `HEDGE` 模型，总结 V3 模型优缺点，提供开关和显卡封存技术建议，将工作流和教程放简介区供下载。

![](img/76cb534bc937b2e05ab89daf213709fa.png)

[T8 Comfyui进阶教程(第14集)-Flux最新Controlnet 3大模型详细测评含速度、参数、效果及改版速度提升20倍的工作流，支持显卡分层加载技术.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VWMWbUjrxoYkuGxEIMgc5Kgcnpe)

## ✏️课代表✏️

+ Controlnet V3 模型在处理写实人物和常用物品时效果显著优于 V1 和 V2。
+ 显卡分层加载技术可以在显存不足时显著提高处理速度。
+ `MF4` 和 `NF4` 模型在处理速度和效果上优于 `FP8`，特别是在人像还原方面。
+ `DEPI` 的 `DEPX` 工作流需要调整以适应不同的显存限制，改用 `depth energy` 节点可以加快处理速度。
+ Controlnet V3 模型在处理卡通和建筑类型图像时效果不佳，需要进一步的优化。
+ `HEDGE` 模型在较低分辨率下效果较好，但强度值需要适当调整以避免过拟合。
+ 用户应根据自己的显存大小选择是否开启显卡封存技术，并避免在使用过程中频繁开关以防止错误。
