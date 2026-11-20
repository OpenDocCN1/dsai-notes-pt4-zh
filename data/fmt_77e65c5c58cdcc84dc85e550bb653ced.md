
# Learning Hub- AnythingLLM

> 来源：[https://gotoaiworld.feishu.cn/docx/BPy0dTzuAopFdoxhhobcsLO9nyf](https://gotoaiworld.feishu.cn/docx/BPy0dTzuAopFdoxhhobcsLO9nyf)

## 官方核心资料

1. [AnythingLLM 官方文档](https://docs.anythingllm.com/introduction)：作为最权威的入门指南（https://docs.anythingllm.com/introduction），文档涵盖入门介绍、AI 智能体配置、自定义技能开发等核心内容。比如详细拆解了软件的 `frontend`、`server` 等核心模块构成，讲解了工作区（`workspaces`）的使用规则，还针对自定义 `Agent` 技能开发给出了明确要求，像需具备 `NodeJS` 基础、技能需返回字符串类型响应等，适合系统了解技术细节。

1. AnythingLLM 桌面版官方下载页：官网（https://anythingllm.com/desktop）提供 Windows、Mac 等系统的桌面版安装包，下载安装流程和普通软件一致，无需复杂环境配置。新手可通过该页面快速获取安装包，搭配官网指引完成基础部署，避免源码部署的繁琐操作。

1. [AnythingLLM GitHub](https://github.com/zxw626/anything-llm) 开源仓库：开源仓库（https://github.com/zxw626/anything-llm）包含完整源代码和部署脚本，遵循 MIT 开源协议。仓库中 `docker` 目录提供了 Docker 部署的详细指令，`examples` 和 `server` 目录可查看 `NodeJS` 服务端与 LLM、向量数据库的交互逻辑，`frontend` 目录则展示了 `React` 前端的实现代码。`README` 文件清晰说明了项目结构，适合想从源码层面理解框架的学习者。

## 中文实战教程

1. 《【喂饭教程】零基础使用 `Ollama`+DeepSeek+AnythingLLM 搭建私人 AI 知识库》：发布于 51CTO 的保姆级教程，特别适合零基础学习者。教程分五步详细讲解搭建流程，从 `Ollama` 的安装配置、嵌入模型的下载，到 AnythingLLM 的下载安装，再到界面语言切换为中文、上传文档构建知识库，每一步都有清晰指引，还标注了关键注意事项，能快速实现私人知识库的搭建。

1. 《本地 AI 知识库神器 anythingLLM: 工程人设置与使用全攻略》：CSDN 的通俗教程，用工地 “项目经理、技术总工、资料员” 的比喻，生动解释了 AnythingLLM 与 LLM 模型、嵌入模型、向量数据库的关系，还拆解了文档上传、分块、检索到最终问答的完整原理。适合先理解核心逻辑，再动手实操的学习者。

1. 《一键 AI 私有知识库！支持多种大模型、`RAG` 和 `AI Agents`》：CSDN 博客系统梳理了 AnythingLLM 的核心特性，比如多用户权限管理、多种文档格式支持、两种对话模式等。同时介绍了其技术架构的三大核心部分，还补充了 `API` 自定义集成的相关信息，帮助学习者除了基础使用外，初步了解其扩展能力。

1. 《Use Milvus in AnythingLLM》：`Milvus` 官网的专项教程，聚焦向量数据库的适配使用。教程详细讲解了如何在 AnythingLLM 中配置 `Milvus` 作为向量数据库，包含前置条件、配置步骤和文档嵌入存储的操作流程，适合想尝试不同向量数据库适配的学习者，拓宽技术应用场景。

## 辅助学习资料

1. 《AnythingLLM 调用大模型和常用工具：构建智能知识库与应用》：CSDN 博客从应用角度出发，介绍了 AnythingLLM 在企业级文档聊天机器人开发中的应用。内容涵盖安装配置、不同 LLM 模型的调用方法，还分析了应用中的注意事项，适合想将工具用于实际工作场景的学习者。

1. AnythingLLM 使用手册 PDF：第三方整理的技术手册，汇总了基础入门、内置 LLM 配置、AI 智能体使用等内容。手册对比了桌面版和 `Docker` 部署的差异，比如桌面版可直接下载 `Llama - 3` 等热门模型，还说明了模型更新的操作流程，可作为离线参考资料随时查阅。
