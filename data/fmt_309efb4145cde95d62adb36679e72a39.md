
# 动手学大模型应用开发

> 来源：[https://my.feishu.cn/docx/GssvdhztFoOH89x8SoScVOVNnIe](https://my.feishu.cn/docx/GssvdhztFoOH89x8SoScVOVNnIe)

> 内容来源：https://datawhalechina.github.io/llm-universe/#/

## [项目简介](https://datawhalechina.github.io/llm-universe/#/?id=%e9%a1%b9%e7%9b%ae%e7%ae%80%e4%bb%8b)

本项目是一个面向小白开发者的大模型应用开发教程，旨在基于阿里云服务器，结合个人知识库助手项目，通过一个课程完成大模型开发的重点入门，主要内容包括：

1. 大模型简介，何为大模型、大模型特点是什么，`LangChain` 是什么，如何开发一个 LLM 应用，针对小白开发者的简单介绍；

1. 如何调用大模型 API，本节介绍了国内外知名大模型产品 API 的多种调用方式，包括调用原生 API、封装为 `LangChain` LLM、封装为 `Fastapi` 等调用方式，同时将包括百度文心、讯飞星火、智谱AI等多种大模型 API 进行了统一形式封装；

1. 知识库搭建，不同类型知识库文档的加载、处理，向量数据库的搭建；

1. 构建 RAG 应用，包括将 LLM 接入到 `LangChain` 构建检索问答链，使用 `Streamlit` 进行应用部署

1. 验证迭代，大模型开发如何实现验证迭代，一般的评估方法有什么；

本项目主要包括三部分内容：

1. LLM 开发入门。V1 版本的简化版，旨在帮助初学者最快、最便捷地入门 LLM 开发，理解 LLM 开发的一般流程，可以搭建出一个简单的 Demo。

1. LLM 开发技巧。LLM 开发更进阶的技巧，包括但不限于：Prompt Engineering、多类型源数据的处理、优化检索、召回精排、Agent 框架等

1. LLM 应用实例。引入一些成功的开源案例，从本课程的角度出发，解析这些应用范例的 Idea、核心思路、实现框架，帮助初学者明白其可以通过 LLM 开发什么样的应用。

目前，第一部分已经完稿，欢迎大家阅读学习；第二、三部分正在创作中。

目录结构说明：

```
requirements.txt：官方环境下的安装依赖
notebook：Notebook 源代码文件
docs：Markdown 文档文件
figures：图片
data_base：所使用的知识库源文件
```

## [项目意义](https://datawhalechina.github.io/llm-universe/#/?id=%e9%a1%b9%e7%9b%ae%e6%84%8f%e4%b9%89)

LLM 正逐步成为信息世界的新革命力量，其通过强大的自然语言理解、自然语言生成能力，为开发者提供了新的、更强大的应用开发选择。随着国内外井喷式的 LLM API 服务开放，如何基于 LLM API 快速、便捷地开发具备更强能力、集成 LLM 的应用，开始成为开发者的一项重要技能。

目前，关于 LLM 的介绍以及零散的 LLM 开发技能课程已有不少，但质量参差不齐，且没有很好地整合，开发者需要搜索大量教程并阅读大量相关性不强、必要性较低的内容，才能初步掌握大模型开发的必备技能，学习效率低，学习门槛也较高。

本项目从实践出发，结合最常见、通用的个人知识库助手项目，深入浅出逐步拆解 LLM 开发的一般流程、步骤，旨在帮助没有算法基础的小白通过一个课程完成大模型开发的基础入门。同时，我们也会加入 RAG 开发的进阶技巧以及一些成功的 LLM 应用案例的解读，帮助完成第一部分学习的读者进一步掌握更高阶的 RAG 开发技巧，并能够通过对已有成功项目的借鉴开发自己的、好玩的应用。

## [项目受众](https://datawhalechina.github.io/llm-universe/#/?id=%e9%a1%b9%e7%9b%ae%e5%8f%97%e4%bc%97)

所有具备基础 Python 能力，想要掌握 LLM 应用开发技能的开发者。

本项目对学习者的人工智能基础、算法基础没有任何要求，仅需要掌握基本 Python 语法、掌握初级 Python 开发技能即可。

考虑到环境搭建问题，本项目提供了阿里云服务器学生免费领取方式，学生读者可以免费领取阿里云服务器，并通过阿里云服务器完成本课程的学习；本项目同时也提供了个人电脑及非阿里云服务器的环境搭建指南；本项目对本地硬件基本没有要求，不需要 GPU 环境，个人电脑及服务器均可用于学习。

注：本项目主要使用各大模型厂商提供的 API 来进行应用开发，如果你想要学习部署应用本地开源 LLM，欢迎学习同样由 Datawhale 出品的 Self LLM ｜ 开源大模型食用指南，该项目将手把手教你如何速通开源 LLM 部署微调全链路！

注：考虑到学习难度，本项目主要面向初学者，介绍如何使用 LLM 来搭建应用。如果你想要进一步深入学习 LLM 的理论基础，并在理论的基础上进一步认识、应用 LLM，欢迎学习同样由 Datawhale 出品的 So Large LM | 大模型基础，该项目将为你提供全面而深入的 LLM 理论知识及实践方法！

## [项目亮点](https://datawhalechina.github.io/llm-universe/#/?id=%e9%a1%b9%e7%9b%ae%e4%ba%ae%e7%82%b9)

1. 充分面向实践，动手学习大模型开发。相较于其他从理论入手、与实践代差较大的类似教程，本教程基于具有通用性的个人知识库助手项目打造，将普适的大模型开发理念融合在项目实践中，帮助学习者通过动手搭建个人项目来掌握大模型开发技能。

1. 从零开始，全面又简短的大模型教程。本项目针对个人知识库助手项目，对相关大模型开发理论、概念和基本技能进行了项目主导的重构，删去不需要理解的底层原理和算法细节，涵盖所有大模型开发的核心技能。教程整体时长在数小时之内，但学习完本教程，可以掌握基础大模型开发的所有核心技能。

1. 兼具统一性与拓展性。本项目对 GPT、百度文心、讯飞星火、智谱GLM 等国内外主要 LLM API 进行了统一封装，支持一键调用不同的 LLM，帮助开发者将更多的精力放在学习应用与模型本身的优化上，而不需要花时间在繁琐的调用细节上；同时，本教程拟上线 奇想星球 | AIGC共创社区平台，支持学习者自定义项目为本教程增加拓展内容，具备充分的拓展性。

## [在线阅读地址](https://datawhalechina.github.io/llm-universe/#/?id=%e5%9c%a8%e7%ba%bf%e9%98%85%e8%af%bb%e5%9c%b0%e5%9d%80)

[LLM Universe V2](https://datawhalechina.github.io/llm-universe/#/)

## [内容大纲](https://datawhalechina.github.io/llm-universe/#/?id=%e5%86%85%e5%ae%b9%e5%a4%a7%e7%ba%b2)

### [第一部分 LLM 开发入门](https://datawhalechina.github.io/llm-universe/#/?id=%e7%ac%ac%e4%b8%80%e9%83%a8%e5%88%86-llm-%e5%bc%80%e5%8f%91%e5%85%a5%e9%97%a8)

负责人：邹雨衡

1. [LLM 介绍](https://datawhalechina.github.io/llm-universe/#/./C1/) @高立业

1. [LLM 的理论介绍](https://datawhalechina.github.io/llm-universe/#/./C1/1.%E5%A4%A7%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B%20LLM%20%E7%90%86%E8%AE%BA%E7%AE%80%E4%BB%8B)

1. 什么是 RAG，RAG 的核心优势

1. [什么是 LangChain](https://datawhalechina.github.io/llm-universe/#/./C1/3.LangChain%20%E7%AE%80%E4%BB%8B)

1. [开发 LLM 应用的整体流程](https://datawhalechina.github.io/llm-universe/#/./C1/4.%E5%BC%80%E5%8F%91%20LLM%20%E5%BA%94%E7%94%A8%E7%9A%84%E6%95%B4%E4%BD%93%E6%B5%81%E7%A8%8B)

1. [阿里云服务器的基本使用](https://datawhalechina.github.io/llm-universe/#/./C1/5.%E9%98%BF%E9%87%8C%E4%BA%91%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)

1. GitHub Codespaces 的基本使用（选修）

1. [环境配置](https://datawhalechina.github.io/llm-universe/#/./C1/7.%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE)

1. [使用 LLM API 开发应用](https://datawhalechina.github.io/llm-universe/#/./C2/) @毛雨

1. [基本概念](https://datawhalechina.github.io/llm-universe/#/./C2/1.%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)

1. [使用 LLM API](https://datawhalechina.github.io/llm-universe/#/./C2/2.%20%E4%BD%BF%E7%94%A8%20LLM%20API)

   + ChatGPT

   + 文心一言

   + 讯飞星火

   + 智谱 GLM

1. Prompt Engineering

1. [搭建知识库](https://datawhalechina.github.io/llm-universe/#/./C3/) @娄天奥

1. [词向量及向量知识库介绍](https://datawhalechina.github.io/llm-universe/#/./C3/1.%E8%AF%8D%E5%90%91%E9%87%8F%E5%8F%8A%E5%90%91%E9%87%8F%E7%9F%A5%E8%AF%86%E5%BA%93%E4%BB%8B%E7%BB%8D)

1. 使用 `Embedding` API

1. [数据处理：读取、清洗与切片](https://datawhalechina.github.io/llm-universe/#/./C3/3.%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86)

1. [搭建并使用向量数据库](https://datawhalechina.github.io/llm-universe/#/./C3/4.%E6%90%AD%E5%BB%BA%E5%B9%B6%E4%BD%BF%E7%94%A8%E5%90%91%E9%87%8F%E6%95%B0%E6%8D%AE%E5%BA%93)

1. [构建 RAG 应用](https://datawhalechina.github.io/llm-universe/#/./C4/) @徐虎

1. 将 LLM 接入 `LangChain`

   + ChatGPT

   + 文心一言

   + 讯飞星火

   + 智谱 GLM

1. 基于 `LangChain` 搭建检索问答链

1. 基于 `Streamlit` 部署知识库助手

1. [系统评估与优化](https://datawhalechina.github.io/llm-universe/#/./C5/) @邹雨衡

1. [如何评估 LLM 应用](https://datawhalechina.github.io/llm-universe/#/./C5/1.%E5%A6%82%E4%BD%95%E8%AF%84%E4%BC%B0%20LLM%20%E5%BA%94%E7%94%A8)

1. [评估并优化生成部分](https://datawhalechina.github.io/llm-universe/#/./C5/2.%E8%AF%84%E4%BC%B0%E5%B9%B6%E4%BC%98%E5%8C%96%E7%94%9F%E6%88%90%E9%83%A8%E5%88%86)

1. [评估并优化检索部分](https://datawhalechina.github.io/llm-universe/#/./C5/3.%E8%AF%84%E4%BC%B0%E5%B9%B6%E4%BC%98%E5%8C%96%E6%A3%80%E7%B4%A2%E9%83%A8%E5%88%86)

### 第二部分 进阶 RAG 技巧（正在创作）

负责人：高立业

1. 背景

1. 架构概览

1. 存在的问题

1. 解决方法

1. 数据处理

1. 多类型文档处理

1. 分块优化

1. 向量模型的选择

1. 微调向量模型（进阶）

1. 索引层面

1. 索引结构

1. 混合检索

1. 假设性问题

1. 检索阶段

1. `query` 过滤

1. 对齐 `query` 和 文档

1. 对齐检索和 LLM

1. 生成阶段

1. 后处理

1. 微调 LLM（进阶）

1. 参考引用

1. 增强阶段

1. 上下文增强

1. 增强流程

1. RAG 工程化评估

### 第三部分 开源 LLM 应用解读

负责人：徐虎

1. ChatWithDatawhale——个人知识库助手解读

1. 天机——人情世故大模型解读

---

## 作者简介

我是文森策，一个10年+品牌策划人，AI提示词&提效玩家。

现阶段热衷于AI辅助内容营销探索，欢迎交流分享AI辅助创作思考/技法/实用工作流，一起用AI打开更多创作可能。

⭐ **提交反馈与交流合作请直接戳我**👇

![](img/229b1d3774a0059e12c954d5f2c0a9d4.png)

![](img/e6664ce0f44fd1f0b8f4c6ee297eadc9.png)
