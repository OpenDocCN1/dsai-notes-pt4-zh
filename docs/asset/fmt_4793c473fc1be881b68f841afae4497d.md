
# `CodeGeeX4-ALL-9B` 技术文档

> 来源：[https://ai.feishu.cn/docx/CCORdlRfPo2wU4xYnOgcXJoCnVg](https://ai.feishu.cn/docx/CCORdlRfPo2wU4xYnOgcXJoCnVg)

## 模型介绍

`C[`odeGeeX4`](https://github.com/THUDM/GLM-4)-ALL-9B`，是最新的 `CodeGeeX4` 系列模型的开源版本。`CodeGeeX4-ALL-9B`模型在 `GLM-4-9B` 基础上持续训练的多语言代码生成模型，显著提升了代码生成能力。使用单个 `CodeGeeX4-ALL-9B` 模型，可以支持代码补全与生成、代码解释、联网搜索、函数调用、仓库级代码问答等多种功能，覆盖了软件开发的各个场景。

`[`CodeGeeX4-AL`](https://huggingface.co/datasets/bigcode/bigcodebench)L-9B` 在 `BigCodeBench` 和 `NaturalCodeBench` 等公开基准测试中取得了极具竞争力的表现。它是目前参数量少于 100 亿的最强代码生成模型，甚至超越了更大的通用模型，在推理速度和模型性能方面达到了最佳平衡。

`CodeGeeX4-ALL-9B`上下文处理能力达到了128K，在“大海捞针”（Needle In A Haystack, NIAH）评估中，python语言环境达到了 100% 的检索准确率。`CodeGeeX4-ALL-9B`也是目前唯一一个实现`Function Call`的代码大模型。

![](img/b8be4db9c9b1910c7c8c6ab28755325a.png)

+ **Github项目地址：**https://github.com/THUDM/CodeGeeX4
+ [HuggingFace模型权重下载：https:/](https://huggingface.co/THUDM/codegeex4-all-9b)/huggingface.co/THUDM/codegeex4-all-9b
+ **ModelScope：**[h](https://modelscope.cn/models/ZhipuAI/codegeex4-all-9b)ttps://modelscope.cn/models/ZhipuAI/codegeex4-all-9b
+ **WiseModel：**[h](https://wisemodel.cn/models/ZhipuAI/codegeex4-all-9b)ttps://wisemodel.cn/models/ZhipuAI/codegeex4-all-9b

任**何相关技术问题，请**到 Github Issue 提问。如果您喜欢我们的项目并认为它对您有帮助，请在GitHub上为`CodeGeeX4`点一个⭐️ Star！

## 开源仓库项目更新

+ 2024/7/16: `CodeGeeX4-ALL-9B`在[HuggingFace上线GGUF权重：https://hu](https://huggingface.co/THUDM/codegeex4-all-9b-GGUF)ggingface.co/THUDM/codegeex4-all-9b-GGUF
+ 2024/7/16: `CodeGeeX4-ALL-9B`在`ModelScope`上线GGUF权重：https://modelscope.cn/models/ZhipuAI/codegeex4-all-9b-GGUF
+ 2024/7/8: `CodeGeeX4-ALL-9B`适配`Ollama`上线。
+ 2024/7/5: `CodeGeeX4-ALL-9B`正式开源！

## 文档目录

针对以下内容进行了详细的介绍，您可以点击进入并查看具体的技术文档。

+ [评测结果](https://zhipu-ai.feishu.cn/wiki/QCFEwwgB4ixMPPkXmcnczIUUnFb)
+ `CodeGeeX4-ALL-9B` 开源模型评测结果
+ [快速使用](https://zhipu-ai.feishu.cn/wiki/PWSQwJxCvi7lbOkiivwcsYOqnnd)
+ `Ollama`
+ `HuggingFace Transformers`
+ `vLLM`
+ [进阶指南](https://zhipu-ai.feishu.cn/wiki/ThXRwtMdfi4uYykStJbcLnwdnad)
+ Chat&通用模式
+ 代码注释
+ 代码解释
+ 单元测试
+ 文件问答
+ 多轮对话
+ [常见问题](https://zhipu-ai.feishu.cn/wiki/IV0PwzEyTi2EuJkWT4dcxYr7nCh)
+ 使用 `CodeGeeX4-ALL-9B` 开源模型遇见的常见问题
+ 如何为项目做出贡献
+ 项目更新计划

## 📚 信息汇总

官方网站：https://www.codegeex.cn

Github仓库：https://github.com/THUDM/CodeGeeX

## 👍 API开放平台：

---

## 🏠 👸号

![](img/6e5b60664a35f0d481b753dec660f73d.png)

## ❤️ 交流群

![](img/74a4e2bceaf7f9e931ec5dce56f62578.png)
---
