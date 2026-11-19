
# 时间序列预测 | Autoformer 简介

> 来源：`https://agijuejin.feishu.cn/docx/WwPGdoXuaoQLlax0grVc0bZenef`

## 👀快看👀

> 作者：[NobleAI](https://space.bilibili.com/3546595134015939)
> 原文链接：时间序列预测 | Autoformer 简介

## ⚡️摘要⚡️

> `Autoformer` 的时间序列预测模型，它通过引入 `Decomposition` 和 `Auto-Correlation` 两个创新模块，改进了传统的 `Transformer` 架构，以适应长期时间序列数据的预测任务。

## 🤖分集🤖

### 时间序列预测 | Autoformer 简介

`Autoformer` 针对长期时间序列预测，在 `Transformer` 基础上引入 `Decomposition` 和 `Auto-Correlation` 模块，分别捕捉数据周期性和改进自注意力机制，提高预测准确性和计算效率，实验证明其在多个数据集上性能优异。

![](img/22fc366b3571da2d1727ddd25cdd860d.png)

[`时间序列预测 _ Autoformer 简介.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NWYabiZZNobR3uxW4aXcyZvSntc)

## ✏️课代表✏️

+ 长期时间序列预测的重要性：传统的 `Transformer` 模型在处理长期时间序列数据时会遇到计算效率和预测准确性的挑战，`Autoformer` 通过特定的设计解决了这些问题。

+ `Decomposition` 模块的作用：通过将时间序列数据分解为周期性和趋势性两部分，可以更准确地捕捉数据的内在规律，从而提高预测的准确性。

+ `Auto-Correlation` 模块的优势：相比于传统的自注意力机制，`Auto-Correlation` 模块能够更有效地处理长序列依赖问题，并且通过快速傅里叶变换（`FFT`）等技术降低了计算复杂度。

+ 实验结果验证：通过在多个数据集上的实验，`Autoformer` 展现了在不同时间跨度和数据特性下的优异性能，尤其在长序列预测任务中表现出色。

+ 周期性分析的重要性：`Autoformer` 的设计考虑了时间序列数据的周期性，通过选取 top k 的周期性序列来提高模型对周期性强的数据的预测能力。

+ 计算复杂度的降低：`Autoformer` 的模块设计不仅提高了模型的性能，同时也通过使用 `FFT` 等算法降低了计算复杂度，使得模型在长序列预测任务中更加高效。
