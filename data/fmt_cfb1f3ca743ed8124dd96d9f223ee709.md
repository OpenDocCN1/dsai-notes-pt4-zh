
# 二本院校 + 计算机科学与技术专业学生12个月成长计划

> 来源：https://gotoaiworld.feishu.cn/docx/EDDqdTcTZoRSFjxtnsncSJGYnfc

## 一、规划总览

- **目标岗位**：大模型应用开发工程师、RAG 系统开发工程师、AI Agent 开发工程师

- **核心能力**：`Python` 工程开发、大模型微调与部署、`RAG`/`Agent` 技术落地、云原生部署

- **薪资预期**：应届生 15-25K（一线城市）、3 年经验后 25-35K

## 二、分月学习规划表

### 第一阶段：基础能力构建（1-3 月）

| 月份 | 学习重点 | 实战项目 | 技能认证准备 |
| --- | --- | --- | --- |
| 1 月 | 1. `Python` 核心语法（函数、类、异常处理）2. 数据处理库（`NumPy`/`Pandas`/`Matplotlib`）3. 基础 `Linux` 命令与 `Git` 操作 | `Python` 自动化工具开发：“PDF 简历解析器”，提取关键信息生成结构化表格：`PyPDF2`+`Pandas` + 正则表达式） | 1. 注册 `GitHub` 账号，学习版本控制基础 2. 开始 `Codecademy` “`Python` 开发者路径” 课程 |
| 2 月 | 1. 数学基础（线性代数矩阵运算、概率分布） > 2. 机器学习基础（逻辑回归、决策树） > 3. 经典深度学习模型（`CNN`/`RNN` 原理） | `文本分类器开发`：基于 `IMDB` 影评数据集，用 `Scikit-learn` 实现情感分类，要求：准确率≥85%，输出混淆矩阵） | 1. 推进 `Coursera` “吴恩达机器学习” 课程 2. 准备 `PCEP` 认证（入门级，提升简历可信度） |
| 3 月 | 1. `Transformer` 架构核心原理（自注意力、位置编码）. `Hugging Face` 基础使用（模型加载、`tokenizer`） 大模型 `API` 调用（`OpenAI`/`ChatGLM`） | `API 调用实战`：“多模态内容生成器”，输入文本生成配图描述 + 文案，技术栈：`ChatGLM API`+`Stable Diffusion API` | 1. 完成 `PCEP` 认证考试 2. 订阅 `Hugging Face` 博客，跟踪模型更新动态 |

### 第二阶段：核心技术进阶（4-6 月）

| 月份 | 学习重点 | 实战项目 | 技能认证准备 |
| --- | --- | --- | --- |
| 4 月 | 1. 大模型微调基础（`LoRA` 原理、参数高效微调）2. `PyTorch` 核心操作（张量、自动微分、模型定义）3. 数据集处理（`Hugging Face Datasets` 库） | `小模型微调实践`：`RA` 微调 `BERT-base`，实现医疗问答意图识别 >（数据来源：`MedQA` 公开数据集，微调后 `F1` 值≥80%） | 1. 开始 `PyTorch` 官方 “深度学习入门” 教程 > 2. 了解 `AWS Certified Machine Learning` 认证大纲 |
| 5 月 | 1. `RAG` 核心技术（检索引擎、向量数据库）. `LangChain` 框架基础（`Chains`/`Agents`/`Tools`）. 向量数据库使用（`Chroma`/`Milvus` 入门） | `本地知识库问答系统`：搭建 “计算机专业知识库”，支持 `PDF` 文档上传与语义问答，栈：`LangChain`+`Chroma`+`GPT-3.5-turbo` | 1. 完成 `Hugging Face` “`Transformers` 工程师” 专项课程。准备 `TensorFlow Developer` 认证实践题 |
| 6 月 | 1. 模型部署基础（`FastAPI` 搭建 `API`、`Swagger` 文档）2. 推理优化入门（`INT8` 量化、模型蒸馏）. 前端基础（`Streamlit` 快速可视化） | `API 服务化部署`：将 5 月知识库系统封装为 `API`，开发 `Streamlit` 前端界面 >（支持功能：文档上传、提问、历史记录查询） | 1. 报名 `TensorFlow Developer` 认证考试 整理前 6 个月项目代码，优化 `GitHub` 仓库结构 |

### 第三阶段：工程化实战（7-9 月）

| 月份 | 学习重点 | 实战项目 | 技能认证准备 |
| --- | --- | --- | --- |
| 7 月 | 1. `AI Agent` 开发（`AutoGen` 框架、多智能体协作）2. 工具调用技术（函数调用、第三方工具集成） > 3. 复杂流程编排（`LangChain StateGraph`） | `电商智能导购 Agent`：实现 “产品推荐 + 比价 + 下单咨询” 多智能体系统，栈：`AutoGen`+`LangChain` + 电商 `API` 模拟 | 1. 学习 `AWS SageMaker` 基础（模型部署平台）2. 研究 `MLOps` 工程师核心技能要求 |
| 8 月 | 1. 容器化技术（`Docker` 镜像构建、容器管理）2. 云原生基础（`K8s` 核心概念、`Pod` 调度） > 3. 模型监控（`Prometheus`+`Grafana`） | `容器化部署项目`：> 将 `RAG` 系统与 `Agent` 打包为 `Docker` 镜像，编写部署脚本实现功能：一键启动、资源监控、日志输出 | 1. 推进 `AWS Certified Machine Learning` 课程 > 2. 完成 `Docker` 官方 “容器化应用开发” 认证 |
| 9 月 | 1. 大模型评测（困惑度、`BLEU` 分数、人工评估）2. 性能优化（显存占用优化、响应速度提升）. 开源项目贡献（`Issue` 修复、文档完善） | `系统优化实战`：月部署项目，将响应延迟从 3s 降至 1s 内，手段：模型量化 + 缓存策略 + 异步处理 | 1. 提交 `Hugging Face Transformers` 库文档 `PR` 准备 `AWS` 认证模拟考试（侧重模型部署模块） |

### 第四阶段：求职冲刺（10-12 月）

| 月份 | 学习重点 | 实战项目 | 技能认证与求职准备 |
| --- | --- | --- | --- |
| 10 月 | 1. 面试核心考点（`Transformer` 推导、微调原理）. 项目复盘方法论（`STAR` 法则、技术难点拆解） 行业解决方案（金融 / 医疗 `RAG` 案例） | `企业级项目整合`：“智能办公助手”，整合 `RAG`+`Agent` + 云部署：文档问答、会议纪要生成、任务调度 | 1. 参加 `Kaggle` 大模型相关竞赛（积累排名） > 2. 完成 `AWS Certified Machine Learning` 认证 |
| 11 月 | 1. 简历优化（量化成果、突出技术栈）. 技术博客撰写（`Medium` / 知乎分享项目）3. 模拟面试（算法题 + 项目答辩） | `技术沉淀输出`：> 撰写 3 篇技术博客：《`LoRA` 微调实战：医疗问答模型训练》2.《`RAG` 系统优化指南：从 3s 到 1s 的实践》.《`AI Agent` 开发：电商导购场景落地》 | 1. `GitHub` 项目 `Star` 数突破 500+2. 内推大厂 `AI` 应用开发岗（重点投中小 `AI` 企业） |
| 12 月 | 1. `Offer` 谈判技巧（薪资构成、晋升路径）2. 入职准备（云平台使用、团队协作工具）3. 前沿技术跟踪（多模态 `Agent`、`MoE` 架构） | `求职作品集整理`：制作在线作品集，包含 4 个核心项目的演示视频 + 代码链接（附加材料：技术博客合集、性能测试报告） | 1. 获得 2-3 个目标岗位 `Offer` . 制定入职后学习计划（聚焦行业垂直领域） |

## 三、关键资源推荐

1. **课程资源**：

+   基础：李沐《动手学深度学习》（`B` 站）、`Codecademy` `Python` 专项

+   进阶：`Hugging Face` 官方教程、`AWS Machine Learning` 专项

+   实战：`LangChain` 文档、`AutoGen` 官方示例库

2. **工具与平台**：

+   开发工具：`VS Code`+`Jupyter Notebook`、`PyCharm`

+   云平台：`AWS Free Tier`、阿里云开发者计划

+   数据集：`Hugging Face Datasets`、`Kaggle` 公开数据集

3. **社区与交流**：

+   技术社区：`GotoAI` 中国技术社区、`Reddit` `r/LangChain`

+   求职社群：大模型应用开发交流群（🛰️ / 🐧）

+   开源社区：`Hugging Face Discussions`、`GitHub Issues`
