
# Meta（`Llama3`）

> 来源：[`https://agispace.feishu.cn/docx/XIandeBHkoxsorxlImPcE48hnQU`](https://agispace.feishu.cn/docx/XIandeBHkoxsorxlImPcE48hnQU)

## 最新消息

北京时间：2024年7月23日23点，Meta正式发布了其最新的开源模型——`Llama 3.1`，包含`8B`、`70B`和`405B`三个尺寸，最大上下文提升到了`128k`。

1. `Llama 3.1`的发布：2024年7月23日，Meta发布了`Llama 3.1`，包括`8B`、`70B`和`405B`三个尺寸，其中`405B`是迄今最强大的模型。

1. 性能和上下文：`Llama 3.1`的最大上下文提升到了`128k`，`405B`模型在性能上超过了`GPT-4 0125`，与`Claude 3.5`不相上下。

1. 数据训练：`Llama 3`使用了超过15 T `token`的公开数据进行训练，采用了超过1.6万个`H100` `GPU`。

1. 开源的重要性：文章强调了开源`AI`的重要性，认为它是未来的发展方向。扎克伯格在致辞中提到，开源`AI`将使更多人受益于`AI`技术，避免权力集中在少数公司手中。

1. `Llama 3.1`的应用：`Llama 3.1`被设计为易于微调和定制，适用于各种规模的公司和研究机构。

1. 生态系统的构建：Meta正在内部建立团队，与开发者和合作伙伴合作，推动`Llama`模型在生态系统中的应用。

其中，`405B`是其拓展最强大的模型，从评分上看，超过了`GPT-4 0125`，和`Claude 3.5`不相上下。

![](img/0cb1742218f88aa1a21e8d200b3f2456.png)

### 相关知名媒体消息：

+   赛博禅心：最大`405B`：`Llama-3.1`发布，第一时间详解

+   [`AI沃茨：3.1个羊驼顶1个`GPT4`，`](https://mp.weixin.qq.com/s/WPoEN4zbuG8ZfWmDB7jZrg)评测`Llama 405B` 10大实用场景

> 北京时间2024年4月18日，Meta开源了其最新版本的大语言模型`LLaMA-3。LLaMA-3`提供了三个版本的模型：参数量分别为80亿、700亿和4000亿的预训练模型和指令微调模型。注意的是，`LLaMA-3`的所有版本目前均采用`Dense`架构，并未如外界预期的那样使用类似`GPT-4`的`MoE`(`Mixture of Experts`)架构。

> `LLaMA 3` 的亮点和特性：

> ▪ 基于超过 15T `token` 训练，大小相当于 `LLaMA 2` 数据集的 7 倍还多；

> ▪ 训练效率比`LLaMA 2` 高 3 倍；

> ▪ 支持 `8K` 长文本，改进的 `tokenizer` 具有 `128K` `token` 的词汇量，可实现更好的性能；

> ▪ 在大量重要基准测试中均具有最先进性能；

> ▪ 增强推理和代码能力；

> ▪ 安全性有重大突破，带有 `LLaMA Guard`，`Code Shield` 和 `CyberSec Eval 2` 的新版信任和安全工具，还能比 `LLaMA 2` 有更好“错误拒绝表现”。

> 官方网址：`https://llama.meta.com/`

> 下载链接：`https://llama.meta.com/llama-downloads/`

> Github网址：`https://github.com/meta-llama/`

## 使用 `Llama 3`进行快速工程

除此之外，Meta 此次和大模型一起发布的还有自己的`AI`助手，`Meta AI`。

`Meta AI` 网址：`https://www.meta.ai/`

![](img/66cd7cdaa40986db89d7241a2323920a.png)

`Meta AI`由最新的`Llama 3`模型提供支持，它不仅将被整合到Meta 旗下的`Instagram`、`WhatsApp`、`Facebook`和`Messenger`的搜索框中，还将首次以独立网站`Meta.ai`的形式对外提供服务。
