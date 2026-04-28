# Ollama

> 来源：[https://gotoaiworld.feishu.cn/docx/G1KOdja60o3ulExgBFdc2D1kn2d](https://gotoaiworld.feishu.cn/docx/G1KOdja60o3ulExgBFdc2D1kn2d)

# 一、Ollama概述

Ollama 是一款开源的大语言模型（LLM）本地部署工具，专注于简化开源大模型的运行流程，让用户能在本地设备（包括消费级电脑、边缘设备）上轻松部署和运行大模型

Ollama 是一款开源的大语言模型（LLM）本地部署工具，专注于简化开源大模型的运行流程，让用户能在本地设备（包括消费级电脑、边缘设备）上轻松部署和运行大模型（如 Llama、Mistral、Gemini 等），无需复杂的环境配置或依赖高端硬件。其核心目标是 “让大模型本地化运行像安装软件一样简单”。

### 核心定位

Ollama 是连接普通用户与开源大模型的 “桥梁”，通过封装底层技术细节（如模型加载、显存优化、推理加速），让开发者和非技术用户都能一键启动大模型，实现本地对话、内容生成等功能，同时保证数据隐私（无需上传至云端）。

### 核心功能与特点

1.  跨平台与零配置部署

+   支持 Windows、macOS、Linux 系统，安装仅需一条命令（如 curl https://ollama.ai/install.sh | sh），无需手动配置 Python 环境、CUDA 驱动等依赖。

+   内置模型下载、缓存和管理机制，运行模型时自动适配本地硬件（CPU/GPU），例如在有 NVIDIA GPU 的设备上会自动启用 CUDA 加速。

1.  丰富的模型支持

+   原生集成主流开源模型，包括 Llama 3（8B/70B）、Mistral、Phi-2、Gemini 开源版、Qwen（通义千问）等，可通过简单命令调用（如 ollama run llama3 启动 Llama 3）。

+   支持社区量化模型（GGUF 格式），如 4bit/8bit 量化版本，在消费级 GPU（如 RTX 3060）或仅 CPU 设备上也能流畅运行 7B-70B 参数模型。

1.  简单的交互方式

+   命令行交互：通过终端直接与模型对话，支持多轮聊天、长文本生成（如写代码、总结文档）。

+   API 接口：提供 REST API（兼容 OpenAI 格式），可轻松集成到应用程序中（如用 Python 调用 http://localhost:11434/api/chat 接口实现对话功能）。

+   自定义模型：通过 Modelfile 配置文件，可基于基础模型微调（如添加系统提示词、绑定工具调用能力），生成专属模型（如 “法律问答专用版 Llama 3”）。

1.  轻量与高效

+   安装包体积小（约 100MB），运行时占用资源可控，支持动态调整模型参数（如最大上下文长度、温度系数）以平衡性能和显存消耗。

+   基于 llama.cpp 优化的推理引擎，支持 KV 缓存复用、批处理等技术，推理速度比原生 Python 实现快 2-5 倍。

### 典型应用场景

+   本地开发测试：开发者无需依赖云端 API，在本地快速验证模型效果（如测试自定义提示词、工具调用逻辑）。

+   隐私敏感场景：处理医疗、法律等涉密数据时，模型在本地运行，避免数据上传风险（如企业内部文档分析）。

+   边缘设备部署：在 Jetson Nano、树莓派等边缘硬件上运行轻量模型（如 7B 参数模型），实现本地化智能交互（如工业设备故障诊断）。

+   教育与演示：学生或研究者可低成本体验大模型原理，无需配置复杂环境。

### 基础使用示例

1.  安装后，在终端运行以下命令启动模型：

1.  bash

```
# 运行 Llama 3 8B 模型（自动下载约 4GB 量化文件）
ollama run llama3

# 运行 Mistral 7B 模型
ollama run mistral
```

1.  与模型对话：

1.  plaintext

```
&gt; 请解释什么是大语言模型？
大语言模型是一种基于深度学习的人工智能系统，通过海量文本数据训练...
```

1.  通过 API 调用（Python 示例）：

1.  python

1.  运行

```
import requests

response = requests.post(&quot;http://localhost:11434/api/chat&quot;,
    json={&quot;model&quot;: &quot;llama3&quot;,&quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;写一首关于春天的诗&quot;}]})print(response.json()[&quot;message&quot;][&quot;content&quot;])
```

### 优势总结

+   易用性：零配置部署，一行命令启动模型，大幅降低本地运行大模型的技术门槛。

+   隐私性：数据本地处理，适合对数据安全敏感的场景。

+   兼容性：支持主流开源模型和硬件，兼顾性能与资源消耗。

+   开源免费：代码完全开源（Apache 2.0 协议），社区活跃，持续更新对新模型的支持。

### 获取资源

+   官网：https://ollama.ai/（提供安装指南、模型列表和 API 文档）

+   GitHub 仓库：https://github.com/ollama/ollama（源码、示例和社区讨论）

总之，Ollama 是本地运行开源大模型的 “首选工具”，尤其适合希望低成本、低门槛体验大模型，或需要本地化部署保障数据隐私的用户。