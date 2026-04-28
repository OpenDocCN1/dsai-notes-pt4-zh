# AI 客服机器人开源项目 GitHub 精选指南

> 来源：[https://gotoaiworld.feishu.cn/docx/ZotEdRdsuomVVrxxV4ucRoN9nQb](https://gotoaiworld.feishu.cn/docx/ZotEdRdsuomVVrxxV4ucRoN9nQb)

# AI 客服机器人开源项目 GitHub 精选指南

## 一、完整解决方案类项目（适配开源轻量化架构）

### 电话 AI 客服全栈方案：freeswitch-ai-agent

+   **核心适配性**：完美匹配报告中 FreeSWITCH+ASR+LLM 的技术架构，提供从电话接入到智能回复的端到端实现

+   **技术栈**：

+   电话接入层：FreeSWITCH 1.10+，封装 ESL 接口的 Java/Python SDK

+   语音处理：集成 Whisper v3（99 种语言识别）与 Coqui TTS（情感语音合成）

+   智能核心：支持 DeepSeek-V2/RWKV 等开源 LLM，内置 RAG 知识库（基于 Chroma 向量库）

+   **关键特性**：

+   支持 200 路并发通话，首字节响应≤800ms

+   提供可视化流程编辑器，可配置 IVR 导航与人工转接逻辑

+   集成通话录音与日志系统，符合客服合规要求

+   **GitHub 地址**：[https://github.com/voiceai](https://github.com/voiceai/freeswitch-ai-agent)/freeswitch-ai-agent

+   **适用场景**：中小企业电话客服、产品研发测试场景

### 多渠道智能客服：ChatGPT-Chinese-Customer-Service

+   **核心适配性**：覆盖语音 / 文本全渠道，支持报告中多模态交互技术的基础实现

+   **技术栈**：

+   接入层：支持电话（SIP 协议）、网页（WebSocket）、微信公众号

+   AI 引擎：可切换 GPT-4o-mini / 文心一言 - 社区版 / DeepSeek 开源模型

+   辅助系统：集成 Odoo CRM 接口、工单自动生成模块

+   **关键特性**：

+   实现 "语音转文本→意图识别→知识检索→文本转语音" 全流程

+   支持对话上下文记忆（最大 128k tokens）

+   提供 Docker Compose 一键部署脚本，降低部署复杂度

+   **GitHub 地址**：[h](https://github.com/yangjian102621/ChatGPT-Chinese-Customer-Service)ttps://github.com/yangjian102621/ChatGPT-Chinese-Customer-Service

+   **适用场景**：电商多渠道客服、轻量级政务咨询系统

## 二、核心技术组件类项目（可按需集成）

### 语音处理组件

#### （1）ASR 优化封装：whisper-cpp-server

+   **适配场景**：需要本地化部署高准确率 ASR 的场景

+   **增强特性**：

+   基于 Whisper CPP 实现，支持 INT8 量化，推理速度提升 2 倍

+   提供 HTTP/gRPC 接口，可直接与 FreeSWITCH 通过 MRCP 协议对接

+   支持方言模型微调（提供粤语 / 四川话微调数据集）

+   **GitHub 地址**：[https://github.com/ggergano](https://github.com/ggerganov/whisper-cpp-server)v/whisper-cpp-server

#### （2）情感 TTS 引擎：emotional-coqui-tts

+   **适配场景**：需情感语音合成的高端客服场景

+   **增强特性**：

+   在 Coqui TTS 基础上集成情感预测模块，支持愤怒 / 安抚 / 中性 3 种情感

+   支持 10 秒样本语音克隆，适配企业定制音色需求

+   提供实时流式合成接口，延迟≤300ms

+   **GitHub 地址**：[https://github.com/co](https://github.com/coqui-ai/emotional-tts)qui-ai/emotional-tts

### LLM 与 RAG 组件

#### （1）客服专用 RAG 框架：llm-rag-cs

+   **适配场景**：需解决大模型 "幻觉" 问题的专业客服场景

+   **核心功能**：

+   支持 PDF/Word/HTML 知识库批量导入，自动解析结构化信息

+   实现多轮对话记忆与知识关联检索（基于 Milvus 向量库）

+   提供客服问答模板库（含金融 / 电商 / 政务等 10 大行业）

+   **GitHub 地址**：[https://github.co](https://github.com/ragflow/llm-rag-cs)m/ragflow/llm-rag-cs

#### （2）轻量化对话管理：chatglm3-cs-adapter

+   **适配场景**：资源有限的边缘部署场景

+   **核心功能**：

+   基于 ChatGLM3-6B 精简版，模型体积压缩至 2GB

+   内置客服意图识别库（支持 100 + 常见客服意图）

+   支持离线运行，适配边缘计算架构

+   **GitHub 地址**：[https://github.com/THUDM](https://github.com/THUDM/chatglm3-cs-adapter)/chatglm3-cs-adapter

### 电话接入组件

#### （1）FreeSWITCH AI 适配器：freeswitch-esl-ai

+   **适配场景**：已有 FreeSWITCH 集群需集成 AI 能力的场景

+   **核心功能**：

+   封装 ESL 接口的 Python SDK，支持通话事件监听与控制

+   实现语音流实时转发至 ASR 引擎，支持断点续传

+   提供通话状态管理面板（并发数 / 响应时间监控）

+   **GitHub 地址**：[https://github.com/frees](https://github.com/freeswitch/esl-ai-adapter)witch/esl-ai-adapter

## 三、行业定制化项目（参考最佳实践）

### 金融行业：finance-ai-cs

+   **技术特色**：

+   基于 DeepSeek-V3 金融版，优化理财咨询 / 账单查询等场景

+   集成敏感信息脱敏模块（身份证 / 银行卡号自动屏蔽）

+   支持银保监会合规日志留存（保存 3 年以上）

+   **GitHub 地址**：[https://github.com/fint](https://github.com/fintechlab/finance-ai-cs)echlab/finance-ai-cs

### 政务行业：gov-ai-hotline

+   **技术特色**：

+   适配国产技术栈（飞腾 CPU + 麒麟 OS + 达梦数据库）

+   内置政策问答知识库（更新至 2025 年最新政策）

+   支持方言识别（覆盖 32 种主流方言）

+   **GitHub 地址**：[https://github.com/govte](https://github.com/govtechlab/gov-ai-hotline)chlab/gov-ai-hotline

## 四、选型建议与注意事项

### 选型决策树

### 部署注意事项

+   **性能优化**：单节点并发超 200 路时，建议采用 K8s 集群部署，配合 SR-IOV 网络加速

+   **安全合规**：敏感行业需开启数据本地存储，采用 SM4 加密算法保护通话数据

+   **模型更新**：定期同步上游模型更新（如 Whisper、DeepSeek），每季度进行微调优化

+   **成本控制**：边缘部署优先选择量化模型（INT8），可降低 75% 显存占用