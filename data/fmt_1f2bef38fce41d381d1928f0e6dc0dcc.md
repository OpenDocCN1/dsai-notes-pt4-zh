
# 发布 · 终于！`GLM-4-9B` 支持 `Ollama` 部署

> 来源：[`https://v11enp9ok1h.feishu.cn/docx/FU4BdNz4VooROFxygP2cJw8rnn7`](https://v11enp9ok1h.feishu.cn/docx/FU4BdNz4VooROFxygP2cJw8rnn7)

![](img/f2cc50585cc9bc776fc85c9a6f5bc85c.png)

`GLM-4-9B` 及 `CodeGeeX4-ALL-9B` 现已支持 `Ollama` 部署。

## 模型介绍

`GLM-4-9B` 是智谱 AI 推出的最新一代预训练模型 `GLM-4` 系列中的开源版本。在语义、数学、推理、代码和知识等多方面的数据集测评中，`GLM-4-9B` 在各项能力上均表现出卓越的能力。

具体可参见 Github Repo：`https://github.com/THUDM/GLM-4`

`CodeGeeX4-ALL-9B` 是最新的 `CodeGeeX4` 系列模型的开源版本。该模型是在 `GLM-4-9B` 基础上持续训练的多语言代码生成模型，显著提升了代码生成能力。

具体可参见 Github Repo：`https://github.com/THUDM/CodeGeeX4`

## Ollama 简介

`Ollama` 是一个专为在本地环境中运行和定制大型语言模型而设计的工具。它提供了一个简单而高效的接口，用于创建、运行和管理这些模型，同时还提供了一个丰富的预构建模型库，可以轻松集成到各种应用程序中。`Ollama` 支持多种操作系统，包括 macOS、Windows、Linux 以及 Docker，适用性广泛。通过 `Ollama`，用户可以方便地部署和运行 `GLM-4-9B` 等开源的大语言模型。此外，`Ollama` 还提供了包括网页、桌面应用和终端界面在内的多种互动方式，方便用户使用和管理这些模型。

欢迎大家体验！

## 模型地址

+   `https://ollama.com/library/glm4`
+   `https://ollama.com/library/codegeex4`

```
#执行以下命令即可部署运行

ollama run glm4

ollama run codegeex4
```

![](img/a8d48f188da013e48f94575844891571.png)

## 注意事项

注意：此模型需要 `Ollama 0.2` 或更高版本，目前处于预发布阶段。当前支持 `ollama` 部署的仅为语言模型，多模态模型尚不支持 `ollama` 部署。
