
# Learning Hub- RAGFlow

> 来源：`https://gotoaiworld.feishu.cn/docx/UtgbdxCGdoNjwoxP875cPWlsnBg`

## 官方核心资料

1. RAGFlow 官方网站 `ragflow.org`：作为入门核心权威资料（`https://ragflow.org/index.html`），网站清晰介绍了平台核心功能、系统架构和快速部署流程。不仅明确了 `CPU`≥4 核、内存≥16GB 等硬件要求，还提供了完整的 `Docker` 部署命令，同时标注了镜像拉取失败时的华为云、阿里云镜像替代方案，此外还能了解到代码执行器、跨语言查询等最新功能，适合搭建基础认知。

1. RAGFlow 官方英文文档 `ragflow.io`：针对需要细致部署指导的学习者，官方英文文档（`https://ragflow.io/docs/dev/`）补充了不同系统下的适配细节。比如 `macOS` 系统中设置 `vm.max_map_count` 的特殊命令、`ARM` 架构下自行构建 `Docker` 镜像的教程，还细化了 `Elasticsearch` 组件的配置原理，可解决跨系统部署的差异化问题。

1. RAGFlow GitHub 开源仓库（`https://github.com/infiniflow/ragflow`）是获取源码和部署脚本的核心渠道。`README` 文件包含完整的启动步骤，`docker` 目录下的配置文件支持 `CPU` 和 `GPU` 两种启动模式，同时可在 `Issues` 区查询部署时的常见报错解决方案，适合想通过源码理解底层架构的学习者。

## 中文实战教程

1. 《十分钟吃透 ragflow: 从入门到精通》：发布于 `CSDN` 的保姆级教程，内容覆盖 RAGFlow 的核心特点、应用场景和完整部署流程。不仅详细讲解了 `vm.max_map_count` 参数的临时修改与永久配置方法，还给出了镜像源切换的具体操作，甚至补充了不同版本镜像的差异对比，后续还涉及知识库创建和文档处理的基础操作，适合零基础快速完成首次部署。

1. 《从 0 到 1: 基于 `Qwen3 Embedding` 的 RAG 智能问答系统搭建指南》：掘金上的实战文章，聚焦 RAGFlow 与具体模型的集成应用。教程以 `Qwen3 Embedding` 为例，演示了如何将第三方模型接入 RAGFlow 搭建智能问答系统，可帮助学习者掌握 RAGFlow 与外部模型协作的核心逻辑，适配进阶的集成开发需求。

1. 《手把手带你搭建 RAGflow，专业 AI 知识库的开源工具》：`CSDN` 的零基础入门教程，简化了部署和知识库搭建的复杂步骤。文中通过通俗的语言解释了文本切片、引用追溯等核心概念，同时提供了大模型学习思维导图、手册等配套资料，适合新手快速理解 RAGFlow 的实际应用价值。

## 视频课程

1. 《Ragflow-Plus 项目简介与操作指南》（`https://www.bilibili.com/video/BV1UJLezaEEE`）：B 站配套视频的图文补充教程，聚焦 `Ragflow-Plus` `v0.2.0` 版本的操作逻辑。视频通过实操演示了知识库的进阶配置，适合想了解 RAGFlow 扩展版本功能的学习者。

1. 《RAGFlow：知识库「终极引擎」》（`https://bibigpt.co/video/BV1VBsFe5E9b`）：B 站的实操演示视频，被总结为 “终极 RAG 引擎” 部署指南。视频详细讲解了 `Docker` 部署的每一步操作，包括文件夹创建、`Git` 克隆仓库等，还演示了模型配置、文档切块设置、`RAPTOR` 功能开启等关键操作，同时讲解了检索效果测试方法，适合偏好可视化学习的初学者。

1. 《大模型应用开发：`Dify`+`RAGFlow`+`Xinf`+`Java`》：`CSDN` 程序员研修院的实战课程，虽偏向综合应用，但包含 RAGFlow 的核心实战内容。课程从基础的 `Docker` 安装讲起，逐步深入 RAGFlow 的知识库搭建、文档向量化处理，最后还讲解了 `Dify` 接入 RAGFlow 外部知识库的方法，适合想掌握多平台协同开发的学习者。

## 开源教材与辅助资源

1. 大模型配套开源资料：多篇 `CSDN` 教程提及可免费领取大模型学习资料包，包含大模型经典书籍、640 套行业报告和系列视频教程。这些资料虽非 RAGFlow 专属，但能补充 `RAG` 技术的基础理论，帮助理解检索增强生成的核心逻辑，适配零基础学习者构建完整的知识体系。

1. `ChinaTextbook 开源教材库`（`https://github.com/TapXWorld/ChinaTextbook/`）：该 `GitHub` 仓库（`https://github.com/TapXWorld/ChinaTextbook/`）包含从小学到大学的各类 `PDF` 教材。可用于测试 RAGFlow 对不同格式、不同篇幅文档的解析能力，适配知识库搭建环节的实操练习，验证文本切片、引用追溯等功能的实际效果。
