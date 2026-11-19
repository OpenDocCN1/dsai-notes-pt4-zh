
# 开源版Coze快速部署使用指南【官方优化版】

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Jb3pdJemzo1gh0xZXxCc5ystntf](https://kq4b3vgg5b.feishu.cn/docx/Jb3pdJemzo1gh0xZXxCc5ystntf)

+ 项目主页：https://github.com/coze-dev/coze-studio

![](img/cf1444f525eb0c4cdbccc36f42d3f0f3.png)

## 开源版Coze VS 在线版Coze

| 功能模块 | 开源版 Coze Studio | 官方 Coze 在线版（https://www.coze.cn） |

| --- | --- | --- |

| Agent 工作流编排 | ✅ 支持基本功能（LLM/判断/调用节点） | ✅ 更丰富的节点类型（如多模态/内存/知识图谱） |

| 插件系统 | ❌ 不含插件市场（需手动配置 HTTP 插件） | ✅ 可一键接入上千个插件（天气/股票/系统控制等） |

| 知识库系统 | ⚠️ 仅支持本地静态 JSON Schema + Milvus | ✅ 云端存储，支持文档上传、PDF分析、OCR |

| 模型支持 | ✅ 支持 OpenAI/火山/通义/Qwen 本地配置 | ✅ 内置 Coze API/多语言模型/免费额度 |

| 多模态输入 | ❌ 不支持语音/图像/视频输入 | ✅ 支持语音对话、图文问答、图像分析等 |

| 对话内存管理 | ❌ 暂无长程记忆/上下文剪辑能力 | ✅ 有完整历史追踪 + 用户画像 |

| 权限与组织协作 | ❌ 不支持组织结构/多用户协作 | ✅ 团队管理/权限设置/邀请协作 |

| 接口开放 | ✅ 支持 API 调用（需配置 OpenAPI） | ✅ 提供统一 SaaS API，支持 Agent as API |

| 评估监控系统 | ✅ Coze Loop 提供基础评测/Trace 追踪 | ✅ 可视化 A/B 测试、日志监控、模型对比 |

| 数据存储 | ✅ 本地 MySQL + Redis + Minio + Milvus | ✅ 云原生存储，支持分布式调度 |

| 插件安全审计 | ❌ 需开发者自己把关 | ✅ 插件沙箱隔离、安全自动审计 |

## 开源版Coze VS Dify

| 对比维度 | 🧠 Coze（开源版） | ⚙️ Dify 平台 |

| --- | --- | --- |

| 平台定位 | 🎯 可私有化部署、灵活扩展的 AI Agent 开发引擎 | 🧩 快速构建智能应用的低代码平台 |

| 可视化体验 | ⭐⭐⭐⭐☆流程结构清晰，偏开发者向 | ⭐⭐⭐⭐⭐拖拽式组件，非开发者也可轻松上手 |

| 插件/工具接入 | ⭐⭐☆☆☆支持 HTTP 工具节点，需手动配置 | ⭐⭐⭐⭐☆内置数百插件 + Function Calling 支持 |

| 模型支持 | ⭐⭐⭐⭐☆支持接入 OpenAI、火山、Qwen 等本地模型 | ⭐⭐⭐⭐⭐支持 HuggingFace、OpenRouter、DeepSeek 等上百模型 |

| RAG 能力 | ⭐⭐⭐☆☆支持自接 Milvus、JSON schema，需手动搭建 | ⭐⭐⭐⭐⭐原生支持文档上传、分片、向量搜索，一键构建知识库 |

| 对话记忆 / 用户画像 | ⭐⭐☆☆☆基础记忆可定制，默认未提供高级能力 | ⭐⭐⭐⭐☆内置用户上下文、历史摘要管理 |

| 多模态支持（语音/图像） | ⭐☆☆☆☆默认不支持（可集成） | ⭐⭐⭐⭐☆图文问答、多模态输入体验优秀 |

| 调试与监控能力 | ⭐⭐⭐⭐☆借助 Coze Loop 可视化链路追踪、评估分析 | ⭐⭐⭐⭐☆支持 Helicone 等 observability 工具集成 |

| 协作与权限控制 | ⭐⭐☆☆☆无多用户组织/权限体系 | ⭐⭐⭐⭐☆团队共享、权限分配、版本管理等支持良好 |

| 部署方式 | ⭐⭐⭐⭐⭐Apache 2.0 开源协议，支持本地/内网/企业私有部署 | ⭐⭐⭐☆支持开源自托管，功能最全需云端账户 |

| 多 Agent 支持 | ⭐⭐⭐⭐☆支持复合 agent 流程，含生命周期管理系统（Loop） | ⭐⭐☆☆☆偏单 Agent 场景，暂不擅长多 Agent 调度 |

| 文档与社区支持 | ⭐⭐⭐☆☆社区新起步，依赖开发者自摸索 | ⭐⭐⭐⭐☆GitHub star 多、文档丰富、中文资源多 |

| 扩展性 / 可定制性 | ⭐⭐⭐⭐⭐微服务 + DDD 架构，适合深度二次开发 | ⭐⭐⭐☆偏平台封闭逻辑，业务适配需 hack 或等更新 |

| 数据隐私 / 安全性 | ⭐⭐⭐⭐⭐本地控制数据，可接私有模型、安全可控 | ⭐⭐☆☆☆默认云端服务，数据需上传 |

| 适合人群 | 🧑💻 高级开发者、企业级场景、私有部署需求者 | 👩💼 产品经理、业务团队、原型验证、非技术人员 |

| 商业可用性 | ✅ 支持，开源无商业限制 | ✅ 商业支持完善，云端可付费扩展 |

## 什么是 Coze Studio

[Coze Studio](https://www.coze.cn/home) 是一站式 AI Agent 开发工具。提供各类最新大模型和工具、多种开发模式和框架，从开发到部署，为你提供最便捷的 AI Agent 开发环境。上万家企业、数百万开发者正在使 Coze Studio。

+ 提供 AI Agent 开发所需的全部核心技术：Prompt、RAG、Plugin、Workflow，使得开发者可以聚焦创造 AI 核心价值。

+ 开箱即用，用最低的成本开发最专业的 AI Agent：Coze Studio 为开发者提供了健全的应用模板和编排框架，你可以基于它们快速构建各种 AI Agent，将创意变为现实。

Coze Studio，源自服务了上万家企业、数百万开发者的「扣子开发平台」，我们将它的核心引擎完全开放。它是一个一站式的 AI Agent 可视化开发工具，让 AI Agent 的创建、调试和部署变得前所未有的简单。

通过 Coze Studio 提供的可视化设计与编排工具，开发者可以通过零代码或低代码的方式，快速打造和调试智能体、应用和工作流，实现强大的 AI 应用开发和更多定制化业务逻辑，是构建低代码 AI 产品的理想选择。Coze Studio 致力于降低 AI Agent 开发与应用门槛，鼓励社区共建和分享交流，助你在 AI 领域进行更深层次的探索与实践。

Coze Studio 的后端采用 Golang 开发，前端使用 React + TypeScript，整体基于微服务架构并遵循领域驱动设计（DDD）原则构建。为开发者提供一个高性能、高扩展性、易于二次开发的底层框架，助力开发者应对复杂的业务需求。

## Coze Studio 能做什么

借助扣子提供的可视化设计与编排工具，你可以通过零代码或低代码的方式，快速搭建出基于大模型的各类 AI 项目，满足个性化需求、实现商业价值。

+ 搭建智能体：智能体是基于对话的 AI 项目，它通过对话方式接收用户的输入，由大模型自动调用插件或工作流等方式执行用户指定的业务流程，并生成最终的回复。智能客服、虚拟伴侣、个人助理、英语外教都是智能体的典型应用场景。

+ 搭建 AI 应用：应用是指利用大模型技术开发的应用程序。扣子中搭建的 AI 应用具备完整业务逻辑和可视化用户界面，是一个独立的 AI 项目。通过扣子开发的 AI 应用有明确的输入和输出，可以根据既定的业务逻辑和流程完成一系列简单或复杂的任务，例如 AI 搜索、翻译工具、饮食记录等。

+ 搭建工作流：扣子的工作流功能可以用来处理逻辑复杂，且有较高稳定性要求的任务流。扣子提供了大量灵活可组合的节点包括大语言模型 LLM、自定义代码、判断逻辑等，无论你是否有编程基础，都可以通过拖拉拽的方式快速搭建一个工作流。例如创建一个撰写行业研究报告的工作流，让智能体写一份 20 页的报告。

## 快速开始

参考[快速开始](https://github.com/coze-dev/coze-studio/wiki/2.-%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)，了解如何获取并部署 Coze Studio 开源版，快速构建项目、体验 Coze Studio 开源版。

## 功能清单

| 功能模块 | 功能点 |

| --- | --- |

| 模型服务 | 管理模型列表，可接入OpenAI、火山方舟 等在线模型服务 |

| 搭建智能体 | 编排、发布、管理智能体；支持配置工作流、知识库等资源 |

| 搭建应用 | 创建、发布应用；通过工作流搭建业务逻辑 |

| 搭建工作流 | 创建、修改、发布、删除工作流 |

| 开发资源 | 支持创建并管理以下资源：插件、知识库、数据库、提示词 |

| API 与 SDK | 创建会话、发起对话等 OpenAPI |

| | 通过 Chat SDK 将智能体或应用集成到自己的应用 |

## 环境要求

在参考本文安装 Coze Studio 之前，确保您的软硬件环境满足以下要求：

| 项目 | 说明 |

| --- | --- |

| CPU | 2 Core |

| RAM | 4 GiB |

| Docker | 提前安装 `Docker`、`Docker Compose`，并启动 `Docker` 服务，详细操作请参考 [Docker 文档](https://docs.docker.com/engine/install/) 和 [Docker Compose 安装指南](https://docs.docker.com/compose/install/)： * macOS：推荐使用 Docker Desktop 安装，参考 Docker Desktop For Mac 安装指南。 * Linux：参考 Docker 安装指南 和 Docker Compose 安装指南。 * Windows：推荐使用 Docker Desktop 安装，参考 Docker Desktop For Windows 安装指南。 |

## 安装 Coze Studio

### 步骤一：获取源码

在本地项目中执行以下命令，获取 Coze Studio 最新版本的源码。

```
# 克隆代码 
git clone https://github.com/coze-dev/coze-studio.git
```

### 步骤二：配置模型

Coze Studio 是基于大语言模型的 AI 应用开发平台，首次部署并启动 Coze Studio 开源版之前，你需要先在 Coze Studio 项目里配置模型服务，否则创建智能体或者工作流时，无法正常选择模型。

本文档以火山方舟模型为例，演示如何为 Coze Studio 配置模型服务。如果你准备使用 OpenAI 等其他在线模型服务，应参考模型配置文档正确填写配置文件。

1. 从模板目录复制 doubao-seed-1.6 模型的模版文件，并粘贴到配置文件目录。

```
cd coze-studio
# 复制模型配置模版
cp backend/conf/model/template/model_template_ark_doubao-seed-1.6.yaml backend/conf/model/ark_doubao-seed-1.6.yaml
```

1. 在配置文件目录下，修改模版文件。

1. 进入目录 `backend/conf/model`。打开文件 `ark_doubao-seed-1.6.yaml`。

1. 设置 `id`、`meta.conn_config.api_key`、`meta.conn_config.model` 字段，并保存文件。

![](img/ed9b6913317a69873e823cd6bcaba60f.png)

+ `id`：Coze Studio 中的模型 ID，由开发者自行定义，必须是非 0 的整数，且全局唯一。模型上线后请勿修改模型 id。

+ [`meta.conn_config.api_key`](https://www.volcengine.com/docs/82379/1541594)：在线模型服务的 API Key，在本示例中为火山方舟的 API Key，获取方式可参考获取火山方舟 API Key。

+ `meta.conn_config.model`：在线模型服务的 model ID，在本示例中为火山方舟 doubao-seed-1.6 模型接入点的 Endpoint ID，获取方式可参考获取 Endpoint ID。

### 步骤三：部署并启动服务

首次部署并启动 Coze Studio 需要拉取镜像、构建本地镜像，可能耗时较久，请耐心等待。如果看到提示 "Container coze-server Started"，表示 Coze Studio 服务已成功启动。

```
# 启动服务
cd docker
cp .env.example .env
docker compose --profile '*/*' up -d
```

### 步骤四：登录访问

启动服务后，通过浏览器访问 http://localhost:8888/ 即可打开 Coze Studio。其中 8888 为后端监听端口。

至此，你已成功部署 Coze Studio，可以根据页面提示注册账号、体验 Coze Studio 的各项功能与服务。

![](img/6b92793bd97ef314d629180e92a3f25d.png)

Coze Studio 是基于大语言模型的 AI 应用开发平台，首次部署运行 Coze Studio 开源版之前，你需要先克隆到本地的项目中，配置所需要的模型。项目正常运行过程中，也可以随时按需添加新的模型服务、删除不需要的模型服务。

## 模型列表

Coze Studio 支持的模型服务如下：

+ Ark（火山方舟）

+ OpenAI

+ DeepSeek

+ Claude

+ Ollama

+ Qwen

+ Gemini

## 模型配置说明

在 Coze Studio 开源版中，模型配置统一放在 `backend/conf/model` 目录中，目录下存在多个 yaml 文件，每个文件对应一个可访问的模型。

为方便开发者快速配置，Coze Studio 在 `backend/conf/model/template` 目录下提供了一些模板文件，覆盖了常见的模型类型，例如火山方舟、OpenAI 等。开发者可以找到对应厂商的模型模板，复制到 `backend/conf/model` 目录，根据模板注释设置各个参数。

## 注意事项

在填写模型配置文件之前，请确保你已经了解了以下注意事项：

+ 保证每个模型文件 `id` 唯一，且配置上线后不修改 `id`。`id` 是模型配置在系统中流转的标识，修改模型 `id` 可能导致已有的智能体功能出现问题。

+ 在删除模型之前，请确保此模型已无线上流量。

+ 智能体或工作流根据模型 ID 来调用模型。对于已上线的模型，请勿修改模型 ID，否则可能导致模型调用失败。

## 为 Coze Studio 配置模型

Coze Studio 是基于大语言模型的 AI 应用开发平台，首次部署并启动 Coze Studio 开源版之前，你需要先在 Coze Studio 项目里配置模型服务，否则创建智能体或者工作流时，无法正常选择模型。

首次部署并设置方舟模型服务，可直接参考快速开始完成配置；如需添加更多模型，或改为其他模型服务，可以参考以下步骤。

### 步骤一：修改模型配置文件

1. 复制模板文件。

1. 在 `backend/conf/template` 目录下，根据要添加的模型名称找到对应的模版 yaml 文件，例如 OpenAI 模型的配置文件模型为 `model_template_openai.yaml`。

1. 将模板文件复制到 `backend/conf/model` 目录下。

1. 修改模型配置。

1. 进入目录 `backend/conf/model`，打开步骤 1 中复制的文件。

1. 修改文件中的 `id`、`meta.conn_config.api_key`、`meta.conn_config.model` 字段，保存文件。

+ `id`：Coze Studio 中的模型 ID，由开发者自行定义，必须是非 0 的整数，且全局唯一。智能体或工作流根据模型 ID 来调用模型。对于已上线的模型，请勿修改模型 ID，否则可能导致模型调用失败。

+ [`meta.conn_config.api_key`](https://www.volcengine.com/docs/82379/1541594)：模型服务的 API Key，获取方式可参考对应的模型服务，例如获取火山方舟 API Key。

+ `meta.conn_config.model`：模型服务的 `model ID`，获取方式可参考对应的模型服务，例如获取火山方舟模型 ID 可参考 [火山方舟模型列表](https://www.volcengine.com/docs/82379/1330310)。

其他参数可维持默认配置，你也可以按需修改。以方舟模型为例，配置方式可参考 火山方舟模型列表。

![](img/37b0bbd9e0ac34fec372514d04bcdcd9.png)

![](img/b742d9282529fb25136f189fbff74469.png)

### 步骤二：重新启动服务

修改配置文件之后，执行以下命令重启服务，使配置生效。

```
docker compose --profile '*/*' restart coze-server
```

服务启动成功之后，打开agent编辑页面，在模型下拉列表里选择已配置的模型。

![](img/c0c7916f5eaa2c0eec6101a4030f1be2.png)



# 字段说明

## 模型信息

模型元信息文件中描述了模型基础能力和模型连接信息。

基础模型元信息模板见 `backend/conf/model/template/model_template_basic.yaml`。

各模型的元信息模板见 `backend/conf/model/template` 下的对应模型名称文件。

以下为完整的模型配置字段，其中标明了各字段的含义：

| 字段名称 | 是否必选 | 示例 | 参数描述 |
| --- | --- | --- | --- |
| `id` | 是 | `0` | 模型 id |
| `name` | 是 | `test_model` | 模型平台展示名称 |
| `icon_uri` | 否 | `default_icon/doubao_v2.png` | 模型展示图片 uri，uri 为 `docker/volumes/minio` 下的相对路径；模板文件中填写的 uri 已经默认上传，可直接使用。 |
| `icon_url` | 否 | `test_icon_url` | 模型展示图片 url，可配置静态链接，展示时优先使用 url 配置，url 为空时使用 uri |
| `description` | 否 | `-` | 模型默认描述 |
| `description.zh` | 否 | `这是模型描述信息` | 中文版模型描述，用于平台展示 |
| `description.en` | 否 | `This is model description` | 英文版模型描述，用于平台展示 |
| `default_parameters` | 否 | `-` | 模型参数列表，列表中元素整体可以参考模板文件 |
| `default_parameters.name` | 是 | `temperature` | 模型参数名称，枚举值：`temperature`, `top_p`, `top_k`, `max_tokens`, `response_format`, `frequency_penalty`, `presence_penalty` |
| `default_parameters.label` | 是 | `-` | 模型参数平台展示名称 |
| `default_parameters.label.zh` | 是 | `生成随机性` | 模型参数平台展示名称-中文 |
| `default_parameters.label.en` | 是 | `Temperature` | 模型参数平台展示名称-英文 |
| `default_parameters.desc` | 是 | `-` | 模型参数平台展示描述 |
| `default_parameters.desc.zh` | 是 | `temperature`: 调高温度会使得模型的输出更多样性和创新性 | 模型参数平台展示描述-中文 |
| `default_parameters.desc.en` | 是 | `Temperature`: When you increase this value, the model outputs more diverse and innovative content | 模型参数平台展示描述-英文 |
| `default_parameters.type` | 是 | `int` | 字段值类型，枚举值：`int`, `float`, `boolean`, `string` |
| `default_parameters.min` | 否 | `'0'` | （数值类型时）字段最小值 |
| `default_parameters.max` | 否 | `'1'` | （数值类型时）字段最大值 |
| `default_parameters.default_val` | 是 | `-` | 精准 / 平衡 / 创意 / 自定义模式下的默认值 |
| `default_parameters.default_val.default_val` | 是 | `'1.0'` | 自定义模式下的默认值 |
| `default_parameters.default_val.creative` | 否 | `'1.0'` | 创意模式下的默认值 |
| `default_parameters.default_val.balance` | 否 | `'0.8'` | 平衡模式下的默认值 |
| `default_parameters.default_val.precise` | 否 | `'0.3'` | 精准模式下的默认值 |
| `default_parameters.precision` | 否 | `2` | （类型为 `float` 时）精度 |
| `default_parameters.style` | 是 | `-` | 展示类型 |
| `default_parameters.style.widget` | 是 | `slider` | 展示样式，枚举值：`slider`（滑块）, `radio_button`（按钮） |
| `default_parameters.style.label` | 是 | `-` | 归类 |
| `default_parameters.style.label.zh` | 是 | `生成多样性` | 归类标签名称-中文 |
| `default_parameters.style.label.en` | 是 | `Generation diversity` | 归类标签名称-英文 |
| `meta` | 是 | `-` | 模型元信息 |
| `meta.name` | 是 | `test_model_name` | 模型名称，用于记录，不展示 |
| `meta.protocol` | 是 | `test_protocol` | 模型连接协议 |
| `meta.capability` | 是 | `-` | 模型基础能力 |
| `meta.capability.function_call` | 否 | `true` | 模型是否支持 function call |
| `meta.capability.input_modal` | 否 | `["text", "image", "audio", "video"]` | 模型输入支持模态 |
| `meta.capability.input_tokens` | 否 | `1024` | 输入 token 上限 |
| `meta.capability.output_modal` | 否 | `["text", "image", "audio", "video"]` | 模型输出支持模态 |
| `meta.capability.output_tokens` | 否 | `1024` | 输出 token 上限 |
| `meta.capability.max_tokens` | 否 | `2048` | 最大 token 数量 |
| `meta.capability.json_mode` | 否 | `true` | 是否支持 json mode |
| `meta.capability.prefix_caching` | 否 | `false` | 是否支持 prefix caching |
| `meta.capability.reasoning` | 否 | `false` | 是否支持 reasoning |
| `meta.conn_config` | 是 | `-` | 模型连接参数 |
| `meta.conn_config.base_url` | 是 | `https://localhost:1234/chat/completion` | 模型服务基础URL |
| `meta.conn_config.api_key` | 是 | `qweasdzxc` | API密钥 |
| `meta.conn_config.timeout` | 否 | `100` | 超时时间（纳秒） |
| `meta.conn_config.model` | 是 | `model_name` | 模型名称 |
| `meta.conn_config.temperature` | 否 | `0.7` | 默认 `temperature` |
| `meta.conn_config.frequency_penalty` | 否 | `0` | 默认 `frequency_penalty` |
| `meta.conn_config.presence_penalty` | 否 | `0` | 默认 `presence_penalty` |
| `meta.conn_config.max_tokens` | 否 | `2048` | 默认 `max_tokens` |
| `meta.conn_config.top_p` | 否 | `0` | 默认 `top_p` |
| `meta.conn_config.top_k` | 否 | `0` | 默认 `top_k` |
| `meta.conn_config.enable_thinking` | 否 | `false` | 是否启用思考过程 |
| `meta.conn_config.stop` | 否 | `["bye"]` | 停止词列表 |
| `meta.conn_config.openai` | 否 | `-` | OpenAI专用配置 |
| `meta.conn_config.openai.by_azure` | 否 | `true` | 是否使用Azure |
| `meta.conn_config.openai.api_version` | 否 | `2024-10-21` | API版本 |
| `meta.conn_config.openai.response_format.type` | 否 | `text` | 响应格式类型 |
| `meta.conn_config.claude` | 否 | `-` | Claude专用配置 |
| `meta.conn_config.claude.by_bedrock` | 否 | `true` | 是否使用Bedrock |
| `meta.conn_config.claude.access_key` | 否 | `bedrock_ak` | Bedrock访问密钥 |
| `meta.conn_config.claude.secret_access_key` | 否 | `bedrock_secret_ak` | Bedrock密钥 |
| `meta.conn_config.claude.session_token` | 否 | `bedrock_session_token` | Bedrock会话令牌 |
| `meta.conn_config.claude.region` | 否 | `bedrock_region` | Bedrock地区 |
| `meta.conn_config.ark` | 否 | `-` | Ark专用配置 |
| `meta.conn_config.ark.region` | 否 | `region` | 地区 |
| `meta.conn_config.ark.access_key` | 否 | `ak` | 访问密钥 |
| `meta.conn_config.ark.secret_key` | 否 | `sk` | 密钥 |
| `meta.conn_config.ark.retry_times` | 否 | `123` | 重试次数 |
| `meta.conn_config.ark.custom_header` | 否 | `{"key": "val"}` | 自定义请求头 |
| `meta.conn_config.deepseek` | 否 | `-` | Deepseek专用配置 |
| `meta.conn_config.deepseek.response_format_type` | 否 | `text` | 响应格式类型 |
| `meta.conn_config.qwen` | 否 | `-` | Qwen专用配置 |
| `meta.conn_config.qwen.response_format` | 否 | `-` | 返回格式化 |
| `meta.conn_config.gemini` | 否 | `-` | Gemini专用配置 |
| `meta.conn_config.gemini.backend` | 否 | `0` | Gemini 后端配置，`0`：默认，`1`：GeminiAPI，`2`：VertexAI |
| `meta.conn_config.gemini.project` | 否 | `test_project` | GCP Project ID for Vertex AI，`backend=2` 时必填 |
| `meta.conn_config.gemini.location` | 否 | `test_loc` | GCP Location/Region for Vertex AI，`backend=2` 时必填 |
| `meta.conn_config.gemini.api_version` | 否 | `v1beta` | api 版本 |
| `meta.conn_config.headers` | 否 | `-` | http headers |
| `meta.conn_config.timeout_ms` | 否 | `-` | http 超时时间 |
| `meta.conn_config.include_thoughts` | 否 | `true` | 返回是否包含 thinking 内容 |
| `meta.conn_config.thinking_budget` | 否 | `123` | thinking token 消耗预算 |
| `meta.status` | 否 | `1` | 模型状态，`0`: 未配置时的默认状态，等同于 `1`，`1`: 应用中，可使用可新建，`5`: 待下线，可使用不可新建，`10`: 已下线，不可使用不可新建 |

## 配置文件示例

各模型的完整配置示例及字段说明可参考 `backend/conf/model/template/model_template_basic.yaml`。

你也可以参考以下最简配置，大部分字段配置基本相同，差异主要在于 `protocol` 与 `conn_config`。

### Ark 火山方舟



## `Doubao Model`

```
id: 2002
name: Doubao Model
icon_uri: default_icon/doubao_v2.png
icon_url: &quot;&quot;
description:
    zh: 豆包模型简介
    en: doubao model description
default_parameters:
    - name: temperature
      label:
        zh: 生成随机性
        en: Temperature
      desc:
        zh: &#x27;- **temperature**: 调高温度会使得模型的输出更多样性和创新性，反之，降低温度会使输出内容更加遵循指令要求但减少多样性。建议不要与“Top p”同时调整。&#x27;
        en: &#x27;**Temperature**:\n\n- When you increase this value, the model outputs more diverse and innovative content; when you decrease it, the model outputs less diverse content that strictly follows the given instructions.\n- It is recommended not to adjust this value with \&quot;Top p\&quot; at the same time.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        balance: &quot;0.8&quot;
        creative: &quot;1&quot;
        default_val: &quot;1.0&quot;
        precise: &quot;0.3&quot;
      precision: 1
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: max_tokens
      label:
        zh: 最大回复长度
        en: Response max length
      desc:
        zh: 控制模型输出的Tokens 长度上限。通常 100 Tokens 约等于 150 个中文汉字。
        en: You can specify the maximum length of the tokens output through this value. Typically, 100 tokens are approximately equal to 150 Chinese characters.
      type: int
      min: &quot;1&quot;
      max: &quot;4096&quot;
      default_val:
        default_val: &quot;4096&quot;
      options: []
      style:
        widget: slider
        label:
            zh: 输入及输出设置
            en: Input and output settings
    - name: top_p
      label:
        zh: Top P
        en: Top P
      desc:
        zh: &#x27;- **Top p 为累计概率**: 模型在生成输出时会从概率最高的词汇开始选择，直到这些词汇的总概率累积达到Top p 值。这样可以限制模型只选择这些高概率的词汇，从而控制输出内容的多样性。建议不要与“生成随机性”同时调整。&#x27;
        en: &#x27;**Top P**:\n\n- An alternative to sampling with temperature, where only tokens within the top p probability mass are considered. For example, 0.1 means only the top 10% probability mass tokens are considered.\n- We recommend altering this or temperature, but not both.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        default_val: &quot;0.7&quot;
      precision: 2
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: response_format
      label:
        zh: 输出格式
        en: Response format
      desc:
        zh: &#x27;- **文本**: 使用普通文本格式回复\n- **Markdown**: 将引导模型使用Markdown格式输出回复\n- **JSON**: 将引导模型使用JSON格式输出&#x27;
        en: &#x27;**Response Format**:\n\n- **Text**: Replies in plain text format\n- **Markdown**: Uses Markdown format for replies\n- **JSON**: Uses JSON format for replies&#x27;
      type: int
      min: &quot;&quot;
      max: &quot;&quot;
      default_val:
        default_val: &quot;0&quot;
      options:
        - label: Text
          value: &quot;0&quot;
        - label: Markdown
          value: &quot;1&quot;
        - label: JSON
          value: &quot;2&quot;
      style:
        widget: radio_buttons
        label:
            zh: 输入及输出设置
            en: Input and output settings
meta:
    name: Doubao
    protocol: ark
    capability:
        function_call: true
        input_modal:
            - text
            - image
        input_tokens: 128000
        json_mode: false
        max_tokens: 128000
        output_modal:
            - text
        output_tokens: 16384
        prefix_caching: false
        reasoning: false
        prefill_response: false
    conn_config:
        base_url: &quot;&quot;
        api_key: &quot;&quot;
        timeout: 0s
        model: &quot;&quot;
        temperature: 0.1
        frequency_penalty: 0
        presence_penalty: 0
        max_tokens: 4096
        top_p: 0.7
        top_k: 0
        stop: []
        openai: null
        claude: null
        ark:
            region: &quot;&quot;
            access_key: &quot;&quot;
            secret_key: &quot;&quot;
            retry_times: null
            custom_header: {}
        deepseek: null
        qwen: null
        gemini: null
        custom: {}
    status: 0
```

## `Claude-3.5-Sonnet`

```
id: 2006
name: Claude-3.5-Sonnet
icon_uri: default_icon/claude_v2.png
icon_url: &quot;&quot;
description:
    zh: claude 模型简介
    en: claude model description
default_parameters:
    - name: temperature
      label:
        zh: 生成随机性
        en: Temperature
      desc:
        zh: &#x27;- **temperature**: 调高温度会使得模型的输出更多样性和创新性，反之，降低温度会使输出内容更加遵循指令要求但减少多样性。建议不要与“Top p”同时调整。&#x27;
        en: &#x27;**Temperature**:\n\n- When you increase this value, the model outputs more diverse and innovative content; when you decrease it, the model outputs less diverse content that strictly follows the given instructions.\n- It is recommended not to adjust this value with \&quot;Top p\&quot; at the same time.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        balance: &quot;0.8&quot;
        creative: &quot;1&quot;
        default_val: &quot;1.0&quot;
        precise: &quot;0.3&quot;
      precision: 1
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: max_tokens
      label:
        zh: 最大回复长度
        en: Response max length
      desc:
        zh: 控制模型输出的Tokens 长度上限。通常 100 Tokens 约等于 150 个中文汉字。
        en: You can specify the maximum length of the tokens output through this value. Typically, 100 tokens are approximately equal to 150 Chinese characters.
      type: int
      min: &quot;1&quot;
      max: &quot;4096&quot;
      default_val:
        default_val: &quot;4096&quot;
      options: []
      style:
        widget: slider
        label:
            zh: 输入及输出设置
            en: Input and output settings
meta:
    name: Claude-3.5-Sonnet
    protocol: claude
    capability:
        function_call: true
        input_modal:
            - text
            - image
        input_tokens: 128000
        json_mode: false
        max_tokens: 128000
        output_modal:
            - text
        output_tokens: 16384
        prefix_caching: false
        reasoning: false
        prefill_response: false
    conn_config:
        base_url: &quot;&quot;
        api_key: &quot;&quot;
        timeout: 0s
        model: &quot;&quot;
        temperature: 0.7
        frequency_penalty: 0
        presence_penalty: 0
        max_tokens: 4096
        top_p: 1
        top_k: 0
        stop: []
        openai: null
        claude:
            by_bedrock: false
            access_key: &quot;&quot;
            secret_access_key: &quot;&quot;
            session_token: &quot;&quot;
            region: &quot;&quot;
        ark: null
        deepseek: null
        qwen: null
        gemini: null
        custom: {}
    status: 0
```

## `Deepseek`



## DeepSeek-V3

```
id: 2004
name: DeepSeek-V3
icon_uri: default_icon/deepseek_v2.png
icon_url: &quot;&quot;
description:
    zh: deepseek 模型简介
    en: deepseek model description
default_parameters:
    - name: temperature
      label:
        zh: 生成随机性
        en: Temperature
      desc:
        zh: &#x27;- **temperature**: 调高温度会使得模型的输出更多样性和创新性，反之，降低温度会使输出内容更加遵循指令要求但减少多样性。建议不要与“Top p”同时调整。&#x27;
        en: &#x27;**Temperature**:\n\n- When you increase this value, the model outputs more diverse and innovative content; when you decrease it, the model outputs less diverse content that strictly follows the given instructions.\n- It is recommended not to adjust this value with \&quot;Top p\&quot; at the same time.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        balance: &quot;0.8&quot;
        creative: &quot;1&quot;
        default_val: &quot;1.0&quot;
        precise: &quot;0.3&quot;
      precision: 1
      options: []
      style:
        widget: slider
        label:
            zh: 生成随机性
            en: Generation diversity
    - name: max_tokens
      label:
        zh: 最大回复长度
        en: Response max length
      desc:
        zh: 控制模型输出的Tokens 长度上限。通常 100 Tokens 约等于 150 个中文汉字。
        en: You can specify the maximum length of the tokens output through this value. Typically, 100 tokens are approximately equal to 150 Chinese characters.
      type: int
      min: &quot;1&quot;
      max: &quot;4096&quot;
      default_val:
        default_val: &quot;4096&quot;
      options: []
      style:
        widget: slider
        label:
            zh: 输入及输出设置
            en: Input and output settings
    - name: response_format
      label:
        zh: 输出格式
        en: Response format
      desc:
        zh: &#x27;- **文本**: 使用普通文本格式回复\n- **JSON**: 将引导模型使用JSON格式输出&#x27;
        en: &#x27;**Response Format**:\n\n- **Text**: Replies in plain text format\n- **Markdown**: Uses Markdown format for replies\n- **JSON**: Uses JSON format for replies&#x27;
      type: int
      min: &quot;&quot;
      max: &quot;&quot;
      default_val:
        default_val: &quot;0&quot;
      options:
        - label: Text
          value: &quot;0&quot;
        - label: JSON Object
          value: &quot;1&quot;
      style:
        widget: radio_buttons
        label:
            zh: 输入及输出设置
            en: Input and output settings
meta:
    name: DeepSeek-V3
    protocol: deepseek
    capability:
        function_call: false
        input_modal:
            - text
        input_tokens: 128000
        json_mode: false
        max_tokens: 128000
        output_modal:
            - text
        output_tokens: 16384
        prefix_caching: false
        reasoning: false
        prefill_response: false
    conn_config:
        base_url: &quot;&quot;
        api_key: &quot;&quot;
        timeout: 0s
        model: &quot;&quot;
        temperature: 0.7
        frequency_penalty: 0
        presence_penalty: 0
        max_tokens: 4096
        top_p: 1
        top_k: 0
        stop: []
        openai: null
        claude: null
        ark: null
        deepseek:
            response_format_type: text
        qwen: null
        gemini: null
        custom: {}
    status: 0
```

## Ollama

```
id: 2003
name: Gemma-3
icon_uri: default_icon/ollama.png
icon_url: &quot;&quot;
description:
    zh: ollama 模型简介
    en: ollama model description
default_parameters:
    - name: temperature
      label:
        zh: 生成随机性
        en: Temperature
      desc:
        zh: &#x27;- **temperature**: 调高温度会使得模型的输出更多样性和创新性，反之，降低温度会使输出内容更加遵循指令要求但减少多样性。建议不要与“Top p”同时调整。&#x27;
        en: &#x27;**Temperature**:\n\n- When you increase this value, the model outputs more diverse and innovative content; when you decrease it, the model outputs less diverse content that strictly follows the given instructions.\n- It is recommended not to adjust this value with \&quot;Top p\&quot; at the same time.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        balance: &quot;0.8&quot;
        creative: &quot;1&quot;
        default_val: &quot;1.0&quot;
        precise: &quot;0.3&quot;
      precision: 1
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: max_tokens
      label:
        zh: 最大回复长度
        en: Response max length
      desc:
        zh: 控制模型输出的Tokens 长度上限。通常 100 Tokens 约等于 150 个中文汉字。
        en: You can specify the maximum length of the tokens output through this value. Typically, 100 tokens are approximately equal to 150 Chinese characters.
      type: int
      min: &quot;1&quot;
      max: &quot;4096&quot;
      default_val:
        default_val: &quot;4096&quot;
      options: []
      style:
        widget: slider
        label:
            zh: 输入及输出设置
            en: Input and output settings
meta:
    name: Gemma-3
    protocol: ollama
    capability:
        function_call: true
        input_modal:
            - text
        input_tokens: 128000
        json_mode: false
        max_tokens: 128000
        output_modal:
            - text
        output_tokens: 16384
        prefix_caching: false
        reasoning: false
        prefill_response: false
    conn_config:
        base_url: &quot;&quot;
        api_key: &quot;&quot;
        timeout: 0s
        model: &quot;&quot;
        temperature: 0.6
        frequency_penalty: 0
        presence_penalty: 0
        max_tokens: 4096
        top_p: 0.95
        top_k: 20
        stop: []
        openai: null
        claude: null
        ark: null
        deepseek: null
        qwen: null
        gemini: null
        custom: {}
    status: 0
```

## OpenAI



```
id: 2001
name: GPT-4o
icon_uri: default_icon/openai_v2.png
icon_url: &quot;&quot;
description:
    zh: gpt 模型简介
    en: Multi-modal, 320ms, 88.7% MMLU, excels in education, customer support, health, and entertainment.
default_parameters:
    - name: temperature
      label:
        zh: 生成随机性
        en: Temperature
      desc:
        zh: &#x27;- **temperature**: 调高温度会使得模型的输出更多样性和创新性，反之，降低温度会使输出内容更加遵循指令要求但减少多样性。建议不要与“Top p”同时调整。&#x27;
        en: &#x27;**Temperature**:\n\n- When you increase this value, the model outputs more diverse and innovative content; when you decrease it, the model outputs less diverse content that strictly follows the given instructions.\n- It is recommended not to adjust this value with \&quot;Top p\&quot; at the same time.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        balance: &quot;0.8&quot;
        creative: &quot;1&quot;
        default_val: &quot;1.0&quot;
        precise: &quot;0.3&quot;
      precision: 1
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: max_tokens
      label:
        zh: 最大回复长度
        en: Response max length
      desc:
        zh: 控制模型输出的Tokens 长度上限。通常 100 Tokens 约等于 150 个中文汉字。
        en: You can specify the maximum length of the tokens output through this value. Typically, 100 tokens are approximately equal to 150 Chinese characters.
      type: int
      min: &quot;1&quot;
      max: &quot;4096&quot;
      default_val:
        default_val: &quot;4096&quot;
      options: []
      style:
        widget: slider
        label:
            zh: 输入及输出设置
            en: Input and output settings
    - name: top_p
      label:
        zh: Top P
        en: Top P
      desc:
        zh: &#x27;- **Top p 为累计概率**: 模型在生成输出时会从概率最高的词汇开始选择，直到这些词汇的总概率累积达到Top p 值。这样可以限制模型只选择这些高概率的词汇，从而控制输出内容的多样性。建议不要与“生成随机性”同时调整。&#x27;
        en: &#x27;**Top P**:\n\n- An alternative to sampling with temperature, where only tokens within the top p probability mass are considered. For example, 0.1 means only the top 10% probability mass tokens are considered.\n- We recommend altering this or temperature, but not both.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        default_val: &quot;0.7&quot;
      precision: 2
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: frequency_penalty
      label:
        zh: 重复语句惩罚
        en: Frequency penalty
      desc:
        zh: &#x27;- **frequency penalty**: 当该值为正时，会阻止模型频繁使用相同的词汇和短语，从而增加输出内容的多样性。&#x27;
        en: &#x27;**Frequency Penalty**: When positive, it discourages the model from repeating the same words and phrases, thereby increasing the diversity of the output.&#x27;
      type: float
      min: &quot;-2&quot;
      max: &quot;2&quot;
      default_val:
        default_val: &quot;0&quot;
      precision: 2
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: presence_penalty
      label:
        zh: 重复主题惩罚
        en: Presence penalty
      desc:
        zh: &#x27;- **presence penalty**: 当该值为正时，会阻止模型频繁讨论相同的主题，从而增加输出内容的多样性&#x27;
        en: &#x27;**Presence Penalty**: When positive, it prevents the model from discussing the same topics repeatedly, thereby increasing the diversity of the output.&#x27;
      type: float
      min: &quot;-2&quot;
      max: &quot;2&quot;
      default_val:
        default_val: &quot;0&quot;
      precision: 2
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: response_format
      label:
        zh: 输出格式
        en: Response format
      desc:
        zh: &#x27;- **文本**: 使用普通文本格式回复\n- **Markdown**: 将引导模型使用Markdown格式输出回复\n- **JSON**: 将引导模型使用JSON格式输出&#x27;
        en: &#x27;**Response Format**:\n\n- **Text**: Replies in plain text format\n- **Markdown**: Uses Markdown format for replies\n- **JSON**: Uses JSON format for replies&#x27;
      type: int
      min: &quot;&quot;
      max: &quot;&quot;
      default_val:
        default_val: &quot;0&quot;
      options:
        - label: Text
          value: &quot;0&quot;
        - label: Markdown
          value: &quot;1&quot;
        - label: JSON
          value: &quot;2&quot;
      style:
        widget: radio_buttons
        label:
            zh: 输入及输出设置
            en: Input and output settings
meta:
    name: GPT-4o
    protocol: openai
    capability:
        function_call: true
        input_modal:
            - text
            - image
        input_tokens: 128000
        json_mode: false
        max_tokens: 128000
        output_modal:
            - text
        output_tokens: 16384
        prefix_caching: false
        reasoning: false
        prefill_response: false
    conn_config:
        base_url: &quot;&quot;
        api_key: &quot;&quot;
        timeout: 0s
        model: &quot;&quot;
        temperature: 0.7
        frequency_penalty: 0
        presence_penalty: 0
        max_tokens: 4096
        top_p: 1
        top_k: 0
        stop: []
        openai:
            by_azure: true
            api_version: &quot;&quot;
            response_format:
                type: text
                jsonschema: null
        claude: null
        ark: null
        deepseek: null
        qwen: null
        gemini: null
        custom: {}
    status: 0
```

## Qwen



```
id: 2005
name: Qwen3-32B
icon_uri: default_icon/qwen_v2.png
icon_url: &quot;&quot;
description:
    zh: 通义千问模型
    en: qwen model description
default_parameters:
    - name: temperature
      label:
        zh: 生成随机性
        en: Temperature
      desc:
        zh: &#x27;- **temperature**: 调高温度会使得模型的输出更多样性和创新性，反之，降低温度会使输出内容更加遵循指令要求但减少多样性。建议不要与“Top p”同时调整。&#x27;
        en: &#x27;**Temperature**:\n\n- When you increase this value, the model outputs more diverse and innovative content; when you decrease it, the model outputs less diverse content that strictly follows the given instructions.\n- It is recommended not to adjust this value with \&quot;Top p\&quot; at the same time.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        balance: &quot;0.8&quot;
        creative: &quot;1&quot;
        default_val: &quot;1.0&quot;
        precise: &quot;0.3&quot;
      precision: 1
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: max_tokens
      label:
        zh: 最大回复长度
        en: Response max length
      desc:
        zh: 控制模型输出的Tokens 长度上限。通常 100 Tokens 约等于 150 个中文汉字。
        en: You can specify the maximum length of the tokens output through this value. Typically, 100 tokens are approximately equal to 150 Chinese characters.
      type: int
      min: &quot;1&quot;
      max: &quot;4096&quot;
      default_val:
        default_val: &quot;4096&quot;
      options: []
      style:
        widget: slider
        label:
            zh: 输入及输出设置
            en: Input and output settings
    - name: top_p
      label:
        zh: Top P
        en: Top P
      desc:
        zh: &#x27;- **Top p 为累计概率**: 模型在生成输出时会从概率最高的词汇开始选择，直到这些词汇的总概率累积达到Top p 值。这样可以限制模型只选择这些高概率的词汇，从而控制输出内容的多样性。建议不要与“生成随机性”同时调整。&#x27;
        en: &#x27;**Top P**:\n\n- An alternative to sampling with temperature, where only tokens within the top p probability mass are considered. For example, 0.1 means only the top 10% probability mass tokens are considered.\n- We recommend altering this or temperature, but not both.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        default_val: &quot;0.95&quot;
      precision: 2
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
meta:
    name: Qwen3-32B
    protocol: qwen
    capability:
        function_call: true
        input_modal:
            - text
        input_tokens: 128000
        json_mode: false
        max_tokens: 128000
        output_modal:
            - text
        output_tokens: 16384
        prefix_caching: false
        reasoning: false
        prefill_response: false
    conn_config:
        base_url: &quot;&quot;
        api_key: &quot;&quot;
        timeout: 0s
        model: &quot;&quot;
        temperature: 0.7
        frequency_penalty: 0
        presence_penalty: 0
        max_tokens: 4096
        top_p: 1
        top_k: 0
        stop: []
        openai: null
        claude: null
        ark: null
        deepseek: null
        qwen:
            response_format:
                type: text
                jsonschema: null
        gemini: null
        custom: {}
    status: 0
```

## Gemini

```
id: 2007
name: Gemini-2.5-Flash
icon_uri: default_icon/gemini_v2.png
icon_url: &quot;&quot;
description:
    zh: gemini 模型简介
    en: gemini model description
default_parameters:
    - name: temperature
      label:
        zh: 生成随机性
        en: Temperature
      desc:
        zh: &#x27;- **temperature**: 调高温度会使得模型的输出更多样性和创新性，反之，降低温度会使输出内容更加遵循指令要求但减少多样性。建议不要与“Top p”同时调整。&#x27;
        en: &#x27;**Temperature**:\n\n- When you increase this value, the model outputs more diverse and innovative content; when you decrease it, the model outputs less diverse content that strictly follows the given instructions.\n- It is recommended not to adjust this value with \&quot;Top p\&quot; at the same time.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        balance: &quot;0.8&quot;
        creative: &quot;1&quot;
        default_val: &quot;1.0&quot;
        precise: &quot;0.3&quot;
      precision: 1
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: max_tokens
      label:
        zh: 最大回复长度
        en: Response max length
      desc:
        zh: 控制模型输出的Tokens 长度上限。通常 100 Tokens 约等于 150 个中文汉字。
        en: You can specify the maximum length of the tokens output through this value. Typically, 100 tokens are approximately equal to 150 Chinese characters.
      type: int
      min: &quot;1&quot;
      max: &quot;4096&quot;
      default_val:
        default_val: &quot;4096&quot;
      options: []
      style:
        widget: slider
        label:
            zh: 输入及输出设置
            en: Input and output settings
    - name: top_p
      label:
        zh: Top P
        en: Top P
      desc:
        zh: &#x27;- **Top p 为累计概率**: 模型在生成输出时会从概率最高的词汇开始选择，直到这些词汇的总概率累积达到Top p 值。这样可以限制模型只选择这些高概率的词汇，从而控制输出内容的多样性。建议不要与“生成随机性”同时调整。&#x27;
        en: &#x27;**Top P**:\n\n- An alternative to sampling with temperature, where only tokens within the top p probability mass are considered. For example, 0.1 means only the top 10% probability mass tokens are considered.\n- We recommend altering this or temperature, but not both.&#x27;
      type: float
      min: &quot;0&quot;
      max: &quot;1&quot;
      default_val:
        default_val: &quot;0.7&quot;
      precision: 2
      options: []
      style:
        widget: slider
        label:
            zh: 生成多样性
            en: Generation diversity
    - name: response_format
      label:
        zh: 输出格式
        en: Response format
      desc:
        zh: &#x27;- **文本**: 使用普通文本格式回复\n- **JSON**: 将引导模型使用JSON格式输出&#x27;
        en: &#x27;**Response Format**:\n\n- **JSON**: Uses JSON format for replies&#x27;
      type: int
      min: &quot;&quot;
      max: &quot;&quot;
      default_val:
        default_val: &quot;0&quot;
      options:
        - label: Text
          value: &quot;0&quot;
        - label: JSON
          value: &quot;2&quot;
      style:
        widget: radio_buttons
        label:
            zh: 输入及输出设置
            en: Input and output settings
meta:
    name: Gemini-2.5-Flash
    protocol: gemini
    capability:
        function_call: true
        input_modal:
            - text
            - image
            - audio
            - video
        input_tokens: 1048576
        json_mode: true
        max_tokens: 1114112
        output_modal:
            - text
        output_tokens: 65536
        prefix_caching: true
        reasoning: true
        prefill_response: true
    conn_config:
        base_url: &quot;&quot;
        api_key: &quot;&quot;
        timeout: 0s
        model: gemini-2.5-flash
        temperature: 0.7
        frequency_penalty: 0
        presence_penalty: 0
        max_tokens: 4096
        top_p: 1
        top_k: 0
        stop: []
        openai: null
        claude: null
        ark: null
        deepseek: null
        qwen: null
        gemini:
            backend: 0
            project: &quot;&quot;
            location: &quot;&quot;
            api_version: &quot;&quot;
            headers:
                key_1:
                    - val_1
                    - val_2
            timeout_ms: 0
            include_thoughts: true
            thinking_budget: null
        custom: {}
    status: 0
```

插件工具可以扩展 LLM 的能力，比如联网搜索、科学计算或绘制图片，赋予并增强了 LLM 连接外部世界的能力。Coze Studio 提供了两种插件工具类型，即官方内置工具和自定义工具。官方内置工具由后台统一配置，支持 Coze Studio 开源版中所有用户使用；自定义工具的使用范围为当前工作空间。

部署 Coze Studio 开源版之后，你可以参考本文档配置官方插件工具，否则这些插件工具可能无法正常运行。

## 官方插件工具列表

登录 Coze Studio 开源版之后，你可以在左侧导航栏单击探索，并在插件页面查看所有官方插件工具。这些插件可通过 API 的方式调用第三方服务。

插件页面中，如果插件带有未授权的标签，表示此插件需要授权，且尚未配置授权信息。配置方式可参考配置官方插件工具。没有此标签的插件可以直接使用。



# Coze Studio 开源版官方插件工具配置指南

![](img/557c7994044b46235daf32189eb34c08.png)

Coze Studio 开源版提供的官方插件工具列表如下：

| 插件名称 | 插件描述 |
| --- | --- |
| Wolfram Alpha | 强大的数学工具，可计算各种数学表达式。 |
| 搜狐热闻 | 帮助用户获取搜狐网上的每日热闻。 |
| 什么值得买 | 帮助用户查询商品优惠信息，提供价格、购买渠道和性价比推荐。 |
| 板栗看板 | 任务拆解工具，根据用户提问拆解任务，生成任务看板。 |
| 文库搜索 | 根据文库文档标题关键字，搜索豆柴文库内容和网页URL。 |
| 高德地图 | 路线规划、位置查询、地理信息转换等功能 |
| 飞书电子表格 | 对飞书电子表格进行创建、编辑和查询等操作。 |
| 博查搜索 | 从全网搜索任何网页信息和网页链接，结果准确、摘要完整，更适合AI使用。 |
| 图片压缩 | 上传图片链接，返回压缩后的base64格式图片。 |
| 飞书认证及授权 | 提供飞书应用的认证及授权功能。 |
| 飞书多维表格 | 支持创建和管理飞书多维表格，进行数据操作和搜索。 |
| 飞书日历 | 在飞书日历上创建、更新、删除、查询日程信息。 |
| 飞书云文档 | 支持创建文档、添加内容、获取文档信息和搜索文档。 |
| 飞书消息 | 支持发送飞书消息和获取消息历史记录。 |
| 飞书任务 | 调用飞书开放平台任务相关API，创建、更新、删除和查询任务。 |
| 飞书知识库 | 搜索飞书知识库Wiki、获取Wiki全部子节点列表。 |
| 天眼查 | 利用天眼查API检索企业信息，查询企业基本信息、新闻舆情等。 |

## 配置官方插件工具

如果你需要直接使用 Coze Studio 提供的官方内置插件工具，在使用前，应参考以下步骤配置相应插件的授权凭证。

此处以“飞书云文档”插件为例，其他官方插件请参考对应第三方服务的 API 文档获取授权凭证。

1. 进入 `backend/conf/plugin` 目录。
2. 打开 `plugin_meta.yaml` 文件。
3. 找到你想要使用的内置工具，例如搜索插件名称“飞书云文档”。
4. 找到对应插件 `payload` 字段。
5. 修改 `client_id`、`client_secret` 字段，配置为应用凭证 App ID 和 App Secret。可在飞书开放平台开发者后台 > 应用详情页 > 凭证与基础信息 > 应用凭证 区域获取。

关于插件的其他参数配置说明，可参考插件配置 schema；`payload` 的其他字段说明，可参考授权配置 Payload。

![](img/6172ef65fd28516d0c4649ae7e462023.png)

6. 保存文件。
7. 执行以下命令重新启动服务。

```
#  重启服务
docker compose --profile '*' restart coze-server
```

服务启动之后，在智能体编辑页面，选择飞书云文档插件，就可以正常使用了。

![](img/d7594b44905ea4f3d6283a8180f92fd0.png)

## 插件配置 schema

以飞书云文档插件为例，其配置 schema 详细字段说明如下。你可以根据业务需求修改各个可选参数配置。关于 `payload` 部分的详细字段说明，可参考授权配置 Payload。

```
- plugin_id: 19 #必填，类型int。插件唯一标识ID。
  product_id: 7395041536909574154 #可忽略。对应coze平台公开的产品ID，未来可能会废弃。
  deprecated: false #可选，类型bool。插件是否已废弃，当官方插件需要从商店下架时，设置为true。
  version: v1.0.0 #必填，类型string。插件版本，格式必须为vX.X.X，每一次插件升级都应该升级版本号。
  openapi_doc_file: lark_docx.yaml #必填，类型string。描述插件工具API的OpenAPI3文档。
  plugin_type: 1 #必填，类型int。目前固定为1，表示插件类型为HTTP。
  manifest: #必填，类型object。插件配置元信息。
    schema_version: v1 #必填，类型string。插件配置元信息的版本号，目前固定为v1。
    name_for_model: 飞书云文档 #必填，类型string，暂时无用。插件名称，提供给大模型理解。
    name_for_human: 飞书云文档 #必填，类型string。插件公开展示的名称。
    description_for_model: 这是一个飞书云文档 #必填，类型string，暂时无用。插件描述，提供给大模型理解。
    description_for_human: 这是一个飞书云文档 #必填，类型string。插件公开展示的描述。
    auth: #必填，类型object。插件鉴权配置。
      type: oauth #必填，类型string。插件鉴权类型，可选none(无鉴权)；oauth(标准oauth2.0)；service_http(简单鉴权)。
      sub_type: authorization_code #可选，类型string。插件鉴权子类型。鉴权类型为none时无效；鉴权类型为oauth时，仅支持authorization_code(授权码模式)；授权类型为service_http时，仅支持token/api_key(APIToken或者APIKey等单字段鉴权信息)；
      payload: '{...}' #可选，类型string。鉴权的具体配置，json格式，具体字段根据鉴权类型和子类型而定。
      logo_url: official_plugin_icon/plugin_lark_docx.png #必填，类型string。插件图标URI，需要提前在./docker/volumes/minio/official_plugin_icon配置。
      api: #必填，类型object。插件额外描述信息。
        type: openapi #必填，类型string。插件类型，目前固定为openapi，表示插件类型为HTTP，与plugin_type含义相同。
      common_params: #必填，类型object。工具HTTP请求的通用参数。
        header: #可选，类型array。工具HTTP请求的通用头参数。User-Agent 固定为 Coze/1.0。
          - name: User-Agent
            value: Coze/1.0
        path: [ ] #可选，类型array。工具HTTP请求的通用路径参数。
        query: [ ] #可选，类型array。工具HTTP请求的通用查询参数。
        body: [ ] #可选，类型array。工具HTTP请求的通用body参数。
  tools: #必填，类型array。插件工具列表。
    - tool_id: 190001 #必填，类型int。工具唯一标识ID。
      deprecated: false #可选，类型bool。工具是否已废弃，当需要从商店下架工具时，设置为true。
      method: post #必填，类型string。工具HTTP请求方法。
      sub_url: /document/create_document #必填，类型string。工具HTTP请求子路径。
    - tool_id: 190002
      deprecated: false
      method: get
      sub_url: /document/get_document_content
```

## 授权配置 Payload

以飞书云文档插件为例，其授权配置 payload 详细字段说明如下。你可以根据业务需求修改各个可选参数配置。

```
type AuthOfAPIToken struct {
    // Location is the location of the parameter.
    // It can be "header" or "query".
    Location HTTPParamLocation `json:"location"`
    // Key is the name of the parameter.
    Key string `json:"key"`
    // ServiceToken is the simple authorization information for the service.
    ServiceToken string `json:"service_token"`
}

type OAuthAuthorizationCodeConfig struct {
    ClientID     string `json:"client_id"`
    ClientSecret string `json:"client_secret"`
    // ClientURL is the URL of authorization endpoint.
    ClientURL string `json:"client_url"`
    // Scope is the scope of the authorization request.
    // If multiple scopes are requested, they must be separated by a space.
    Scope string `json:"scope,omitempty"`
    // AuthorizationURL is the URL of token exchange endpoint.
    AuthorizationURL string `json:"authorization_url"`
    // AuthorizationContentType is the content type of the authorization request, and it must be "application/json".
    AuthorizationContentType string `json:"authorization_content_type"`
}
```

部署 Coze Studio 开源版之后，如需使用图片上传功能，或知识库相关功能，应参考本文档配置功能相关的基础组件。这些组件通常依赖火山引擎等第三方服务，配置组件时需要填写第三方服务的账号密钥等鉴权配置。

## 上传组件

在多模态对话场景，往往需要在对话中上传图片、文件等信息，例如在智能体调试区域中发送图片消息：

![](img/3161392fe49563a7d94154218865da74.png)

此功能由上传组件提供。Coze Studio 上传组件目前支持以下三种，你可以任选一种作为上传组件。

+ `（默认）minio`：图片和文件自动上传至本地主机的 `minio` 服务，通过指定端口号可访问。但本地主机必须配置公网域名，否则已上传的图片和文件只能生成内网访问链接，无法被大模型读取和识别。
+ `火山引擎对象存储 TOS`：图片和文件自动上传至指定的火山引擎对象存储 `TOS` 服务，并生成一个公网可访问的 URL。如果选择 `TOS`，必须先开通 `TOS` 并在 Coze Studio 中配置火山密钥。
+ `火山引擎 ImageX`：图片和文件自动上传至指定的火山引擎 `ImageX`，并生成一个公网可访问的 URL。如果选择 `ImageX`，必须先开通 `ImageX` 并在 Coze Studio 中配置火山密钥。

上传组件的配置方式如下：

1. 设置上传组件类型。

在 `docker` 目录中打开 `.env` 文件，配置项 `FILE_UPLOAD_COMPONENT_TYPE` 的值表示上传组件类型。

+ `storage`（默认）：表示使用 `STORAGE_TYPE` 配置的存储组件，`STORAGE_TYPE` 默认为 `minio`，也支持配置为 `tos`。
+ `imagex`：表示使用火山 `ImageX` 组件。

```
# This Upload component used in Agent / workflow File/Image With LLM  , support the component of imagex / storage
# default: storage, use the settings of storage component
# if imagex, you must finish the configuration of <VolcEngine ImageX> 
export FILE_UPLOAD_COMPONENT_TYPE="storage"
```

2. 为上传组件添加密钥等配置。

同样在 `docker` 目录的 `.env` 文件中，根据组件类型填写以下配置。

| 组件类型 | 配置方式 | 示例 |
| --- | --- | --- |
| `minio`（默认） | 1. 在 Coze studio 项目的 `docker/.env` 文件中，`FILE_UPLOAD_COMPONENT_TYPE` 设置为 `storage`。 2. 在 Storage component 区域，设置 `STORAGE_TYPE` 为 `minio`。 3. 在 MiniO 区域，维持默认配置即可。 如果你选择在火山引擎等公共云上部署 Coze Studio，则需要提前为你的云服务器开放 8888、8889 端口的访问权限。 | ![](img/5f615ef3f5f4fe20d4d714ed1ad1cfb2.png) |
| `tos` | 1. 开通火山引擎 TOS 产品。 2. 在 Coze studio 项目的 `docker/.env` 文件中，`FILE_UPLOAD_COMPONENT_TYPE` 设置为 `storage`。 3. 在 Storage component 区域，设置 `STORAGE_TYPE` 为 `tos`。 4. 在 TOS 区域，填写以下参数： * `TOS_ACCESS_KEY`：火山引擎密钥 AK。获取方式可参考获取火山方舟 API Key。 * `TOS_SECRET_KEY`：火山引擎密钥 SK。获取方式可参考获取火山方舟 API Key。 * `TOS_ENDPOINT`：TOS 服务的 Endpoint，获取方式可参考地域和访问域名。 * `TOS_REGION`：TOS 服务所在的地域，获取方式可参考地域和访问域名。 | |
| `imagex` | 1. 开通火山引擎 veImageX 产品，并创建素材托管服务。操作步骤可参考火山 veImageX 官方文档。注意创建素材托管服务时需要填写域名，建议提前获取一个可用的公开域名。 2. 在 Coze studio 项目的 `docker/.env` 文件中，`FILE_UPLOAD_COMPONENT_TYPE` 设置为 `imagex`。 3. 在 VolcEngine ImageX 区域，填写以下参数： * `VE_IMAGEX_AK`：火山引擎密钥 AK。获取方式可参考获取火山方舟 API Key。 * `VE_IMAGEX_SK`：火山引擎密钥 SK。获取方式可参考获取火山方舟 API Key。 * `VE_IMAGEX_SERVER_ID`：火山引擎 veImageX 产品服务管理页面展示的服务 ID。 * `VE_IMAGEX_DOMAIN`：创建服务时指定的域名。 * `VE_IMAGEX_TEMPLATE`：图片处理配置的模板名称。 | |

3. 执行以下命令重启服务，使以上配置生效。

```
docker compose --profile '*' up -d --force-recreate --no-deps coze-server
```

## 知识库相关组件

### 文字识别（OCR）

在创建知识库时，如果上传的本地文件是扫描件，往往需要开启 `ORC` 功能来识别图片或文件中的文字信息。配置示例如下：



# Coze Studio 知识库配置

## OCR 配置

![](img/a363f1fde7237c450c7b45e2a7f0e278.png)

Coze Studio 知识库的 `ORC` 能力由火山引擎 `OCR` 产品提供服务。在 Coze Studio 开源版中开启 `OCR` 功能前，应先开通火山引擎 `OCR` 产品，并在本地项目中填写 `ORC` 配置，将 `OCR` 组件的 `AK`、`SK` 配置为火山引擎的密钥对。详细操作步骤如下：

1. 开通火山引擎[通用文字识别](https://console.volcengine.com/ai/ability/info/3)产品。

   你可以根据页面提示完成开通。开通产品后，通用文字识别产品页面显示接入状态为正式开通，示例如下：

   ![](img/fd84fec8d74cb0e4084322ed2a9bb47d.png)

2. 获取火山账号的 [`AK` 和 `SK`](https://www.volcengine.com/docs/6291/65568)，后续步骤中需要使用。

   获取方式可参考火山官方文档。

3. 在本地项目中填写 `ORC` 配置。

4. 进入 `docker/` 目录。

5. 打开文件 `.env`，修改 `VE_OCR_AK`、`VE_OCR_SK` 配置项。

   + `VE_OCR_AK`：配置为步骤二中获取的火山账号 `AK`。
   + `VE_OCR_SK`：配置为步骤二中获取的火山账号 `SK`。

6. 保存配置。

```
# Settings for OCR
# If you want to use the OCR-related functions in the knowledge base feature，You need to set up the OCR configuration. 
# Currently, Coze Studio has built-in Volcano OCR.
# ocr_type: default type `ve`
export OCR_TYPE="ve"
# ve ocr
export VE_OCR_AK=""
export VE_OCR_SK=""
```

7. 执行以下命令重启服务。

```
docker compose --profile '*' up -d --force-recreate --no-deps coze-server
```

## 向量化存储（VectorStore）

Coze Studio 知识库功能依赖向量化存储组件，目前支持以下两种组件：

+ `milvus`：开源的高性能向量数据库，专注于解决大规模向量相似度搜索（如图片、视频、文本的语义检索）问题。`milvus` 未内置 `embedding` 模型，需要另外添加本地部署的 `embedding` 模型。

+ `vikingdb`：火山引擎提供的新一代分布式向量数据库，专为超大规模向量数据管理与检索设计。`vikingdb` 内置多个可选的向量化模型，目前可选的 `embedding` 模型列表可参考 `VikingDB` 模型列表，你也可以选择本地部署的 `embedding` 模型。

配置流程如下：

1. 进入 `docker/` 目录，打开文件 `.env`，找到 `vectorstore` 配置模块。

   以下部分为 `vectorstore` 配置模块。

   ```
   # Settings for VectorStore
   # VectorStore type: milvus / vikingdb
   # If you want to use vikingdb, you need to set up the vikingdb configuration.
   export VECTOR_STORE_TYPE="milvus"
   # milvus vector store
   export MILVUS_ADDR="localhost:19530"
   # vikingdb vector store for Volcengine
   export VIKING_DB_HOST=""
   export VIKING_DB_REGION=""
   export VIKING_DB_AK=""
   export VIKING_DB_SK=""
   export VIKING_DB_SCHEME=""
   export VIKING_DB_MODEL_NAME=""  # if vikingdb model name is not set, you need to set Embedding settings
   ```

2. 选择你需要的向量库类型，修改对应配置项。

   + `milvus`：填写以下配置

     + `VECTOR_STORE_TYPE` 设置为 `milvus`。
     + `MILVUS_ADDR` 设置为 `localhost:19530`。

   + `vikingdb`：开通火山引擎方舟平台的知识库模块，获取火山账号的密钥（`AK`、`SK`），并填写以下配置

     + [`VIKING_DB_HOST`](https://www.volcengine.com/docs/84313/1254488#%E5%88%9D%E5%A7%8B%E5%8C%96-sdk)：方舟知识库的域名。详细说明可参考火山官方文档。
     + `VIKING_DB_REGION`：方舟知识库所在地域，华北：`cn-beijing`，华东：`cn-shanghai`，柔佛：`ap-southeast-1`。
     + [`VIKING_DB_AK`](https://www.volcengine.com/docs/6291/65568)：火山引擎账号 `AK`。获取方式可参考火山官方文档。
     + [`VIKING_DB_SK`](https://www.volcengine.com/docs/6291/65568)：火山引擎账号 `SK`。获取方式可参考火山官方文档。
     + `VIKING_DB_SCHEME`：`http` 或 `https`。
     + `VIKING_DB_MODEL_NAME`：可选的 `embedding` 模型列表可参考 [`VikingDB` 模型列表](https://www.volcengine.com/docs/84313/1254542#%E6%A8%A1%E5%9E%8B%E5%88%97%E8%A1%A8)。如需选用本地部署的 `embedding` 模型，此处留空，并填写 `Embedding` 模型部分的配置。详细说明可参考下文向量化模型（`Embedding`）。

3. 保存文件。

4. 执行以下命令重启服务。

```
docker compose --profile '*' up -d --force-recreate --no-deps coze-server
```

## 向量化模型（Embedding）

Coze Studio 开源版支持自定义设置知识库向量化依赖的 `Embedding` 模型，使知识库的向量化环节效果更符合指定场景的业务需求。

+ 如果向量化存储模块使用 `milvus`，则必须参考本文档设置 `Embedding` 模型。

+ `VikingDB` 向量库自带 `Embedding` 功能，如果向量化存储模块使用 [`milvus`](https://www.volcengine.com/docs/84313/1254542#%E6%A8%A1%E5%9E%8B%E5%88%97%E8%A1%A8)，则可以自行选择使用预置的 `VikingDB` 模型或者使用 `OpenAI` 或其他模型。

Coze Studio 支持三种方式接入，`openai`、`ark`、`http`（本地部署的模型服务）。

配置流程如下：

1. 进入 `docker/` 目录，打开文件：`.env`，找到 `Embedding` 配置模块。

2. 选择向量化模型类型，并填写对应的配置参数。

   详细配置如下：

   ```
   # Settings for Embedding
   # The Embedding model relied on by knowledge base vectorization does not need to be configured 
   # if the vector database comes with built-in Embedding functionality (such as VikingDB). Currently, 
   # Coze Studio supports three access methods: openai, ark, and custom http. Users can simply choose one of them when using
   # embedding type: openai / ark / http
   export EMBEDDING_TYPE="openai"
   # openai embedding
   export OPENAI_EMBEDDING_BASE_URL="https://search.bytedance.net/gpt/openapi/online/v2/crawl"
   export OPENAI_EMBEDDING_MODEL=""
   export OPENAI_EMBEDDING_API_KEY=""
   export OPENAI_EMBEDDING_BY_AZURE=true
   export OPENAI_EMBEDDING_API_VERSION=""
   export OPENAI_EMBEDDING_DIMS=1024
   # ark embedding
   export ARK_EMBEDDING_MODEL=""
   export ARK_EMBEDDING_AK=""
   export ARK_EMBEDDING_DIMS=""
   # http embedding
   export HTTP_EMBEDDING_ADDR="http://127.0.0.1:6543"
   ```

3. 保存文件。

4. 执行以下命令重启服务。

```
docker compose --profile '*' up -d --force-recreate --no-deps coze-server
```

## AI 生成模型（Model）

Coze Studio 知识库提供文本转 SQL（`NL2SQL`）、一句话生成 Query（`Message to Query`）、图像标注（`Image Annotation`）功能，这些功能统一依赖于 Coze Studio 预配置的 AI 生成模型。在 Coze Studio 开源版中使用这些功能前，你需要先配置这个 AI 生成模型。

+ 此处配置的 AI 生成模型只在 `NL2SQL`、`Message to Query`、`Image Annotation` 三个场景中生效。

+ 如果需要在不同场景中使用不同模型，你可以通过添加前缀来针对特定场景应用特定配置，例如图片标注场景需要配置视觉模型，其余场景不需要，则可以新增 `export IA_BUILTIN_CM_OPENAI_MODEL="xxx"` 来配置图片标注场景专用的视觉模型。

配置步骤如下：

1. 进入 `docker/` 目录。

2. 打开文件：`.env`，找到 `Model for knowledge` 配置模块。修改以下配置：

   + 设置模型类型：通过参数 `BUILTIN_CM_TYPE` 设置 `NL2SQL` 等 AI 生成场景的模型。支持设置为 `openai`、`ark`、`deepseek`、`ollama` 或 `qwen`。

   + 设置模型鉴权等参数：不同的模型对应不同的参数配置，你可以在配置文件中找到各个模型的配置参数区域。例如 `ark` 模型应设置以下参数：

     + `BUILTIN_CM_ARK_API_KEY`：[`火山方舟 API Key`](https://www.volcengine.com/docs/82379/1541594)，获取方式可参考获取火山方舟 API Key。

     + [`BUILTIN_CM_ARK_MODEL`](https://www.volcengine.com/docs/82379/1330310)：方舟模型 ID，可参考 `火山方舟模型列表`。

   ```
   # Settings for Model
   # Model for knowledge nl2sql, messages2query (rewrite), image annotation
   # add prefix to assign specific model, downgrade to default config when prefix is not configured:
   # 1. nl2sql:            NL2SQL_ (e.g. NL2SQL_BUILTIN_CM_TYPE)
   # 2. messages2query:    M2Q_    (e.g. M2Q_BUILTIN_CM_TYPE)
   # 3. image annotation:  IA_     (e.g. IA_BUILTIN_CM_TYPE)
   # supported chat model type: openai / ark / deepseek / ollama / qwen
   export BUILTIN_CM_TYPE="openai"
   # type openai
   export BUILTIN_CM_OPENAI_BASE_URL=""
   export BUILTIN_CM_OPENAI_API_KEY=""
   export BUILTIN_CM_OPENAI_BY_AZURE=true
   export BUILTIN_CM_OPENAI_MODEL=""

   # type ark
   export BUILTIN_CM_ARK_API_KEY = ""
   export BUILTIN_CM_ARK_MODEL = ""

   # type deepseek
   export BUILTIN_CM_DEEPSEEK_BASE_URL = ""
   export BUILTIN_CM_DEEPSEEK_API_KEY = ""
   export BUILTIN_CM_DEEPSEEK_MODEL = ""

   # type ollama
   export BUILTIN_CM_OLLAMA_BASE_URL = ""
   export BUILTIN_CM_OLLAMA_MODEL = ""

   # type qwen
   export BUILTIN_CM_QWEN_BASE_URL = ""
   export BUILTIN_CM_QWEN_API_KEY = ""
   export BUILTIN_CM_QWEN_MODEL = ""
   ```

3. 保存文件。

4. 执行以下命令重启服务。

```
docker compose --profile '*' up -d --force-recreate --no-deps coze-server
```

## Workflow 代码运行沙箱（Sandbox）

沙箱功能提供了一个安全隔离的沙箱环境，供 Workflow 代码节点运行用户配置的 `python` 代码。

默认情况下，代码会在服务环境的 `Python` 虚拟环境（`venv`）中直接运行。如果你有隔离需求，可以通过修改下面的环境变量来配置：

1. 进入 `docker/` 目录。

2. 打开文件：`.env`，找到 `CODE_RUNNER_TYPE` 配置项，将其修改为 `sandbox`，并配置 `sandbox` 的依赖配置项，保存



```
# CODE_RUNNER_TYPE 支持 sandbox / local，默认为 local
export CODE_RUNNER_TYPE="sandbox"

# 注意：下面的配置仅在 CODE_RUNNER_TYPE=sandbox 时生效

export CODE_RUNNER_ALLOW_ENV=""   # 限制环境变量读取，用逗号分隔，例如: "PATH,USERNAME"
export CODE_RUNNER_ALLOW_READ=""  # 读权限，用逗号分隔，例如: "/tmp,./data"
export CODE_RUNNER_ALLOW_WRITE="" # 写权限，用逗号分隔，例如："/tmp,./data"
export CODE_RUNNER_ALLOW_RUN=""   # 运行权限，用逗号分隔，例如："python,git"

# 网络访问权限，用逗号分隔，例如："api.test.com,api.test.org:8080"
# 默认配置的 CDN 支持下载运行 pyodide 所需的包，如果删除的话沙箱可能无法正常运行。
export CODE_RUNNER_ALLOW_NET="cdn.jsdelivr.net"

export CODE_RUNNER_ALLOW_FFI=""        # 外部函数接口，用逗号分隔，例如："/usr/lib/libm.so"
export CODE_RUNNER_NODE_MODULES_DIR="" # deno modules 路径，默认使用项目路径，例如："/tmp/path/node_modules"
export CODE_RUNNER_TIMEOUT_SECONDS=""  # 代码执行超时时间，默认 60s，例如："2.56"
export CODE_RUNNER_MEMORY_LIMIT_MB=""  # 代码运行内存限制，默认 100MB，例如："256"
```

1. 执行以下命令重启服务。

```
docker compose --profile '*' up -d --force-recreate --no-deps coze-server
```

# 准备工作

在调用 Coze Studio 社区版 API 前，确保你已经将智能体发布为了 API 服务，并通过不同的授权方式获取了访问令牌。

## 发布智能体为 API 服务

智能体发布为 API 服务之后，才能通过调用 API 的方式使用这个智能体，例如查看智能体的基本设置、发起一个智能体对话等。

操作步骤如下：

1. 登录 Coze Studio 社区版。
1. 在项目开发页面，选择智能体。
1. 在页面右上角，单击发布。
1. 在发布页面，选择 API 选项，然后单击发布。

![](img/f01b278095add1647fa1dd31919d3169.png)

## 获取个人访问令牌

调用扣子 API 时，你需要在 `Header` 中通过 `Authorization` 参数指定访问令牌（Access token），扣子服务端会根据访问令牌验证调用方的操作权限。调用 API 之前，你需要先获得访问令牌。

Coze Studio 社区版仅支持通过个人访问令牌鉴权，暂不支持 OAuth 等鉴权方式。

获取个人访问令牌的操作步骤如下：

1. 登录 Coze Studio 社区版。
1. 在页面左下角单击个人头像，并选择 API 授权。

![](img/edc684e7c6b33ea4b12ac531beaa76a4.png)

1. 在个人访问令牌页面中，单击添加新令牌。
1. 在弹出的页面完成以下配置，然后单击确定。

| 配置项 | 说明 |

| --- | --- |

| 名称 | 个人访问令牌的名称。 |

| 过期时间 | 个人访问令牌的有效期时长。令牌过期后将会失效，无法继续用它来调用扣子 API。 生成令牌后，无法修改过期时间。 |

1. 复制并妥善保存个人访问令牌。

生成的令牌仅在此时展示一次，请即刻复制并保存。

# 创建会话

创建一个会话。

会话是智能体和用户之间的基于一个或多个主题的问答交互，一个会话包含一条或多条消息。创建会话时，扣子会同时在会话中创建一个空白的上下文片段，用于存储某个主题的消息。后续发起对话时，上下文片段中的消息是模型可见的消息历史。

你可以在创建会话时同步写入消息，消息默认写入到最新的一个上下文片段中，对话时将作为上下文传递给模型。

## 基础信息

| 请求方式 | POST |

| --- | --- |

| 请求地址 | `{{host}}/v1/conversation/create` |

| 接口说明 | 创建一个会话。 |

## 请求参数

### Header

| 参数 | 取值 | 说明 |

| --- | --- | --- |

| `Authorization` | Bearer `$Access_Token` | 用于验证客户端身份的个人访问令牌。你可以在扣子平台中生成个人访问令牌，详细信息，参考【准备工作】。 |

| `Content-Type` | `application/json` | 解释请求正文的方式。 |

### Body

| 参数 | 类型 | 是否必选 | 示例 | 说明 |

| --- | --- | --- | --- | --- |

| `bot_id` | `String` | 可选 | 730454116184516* | 会话对应的智能体 ID。创建会话时指定智能体 ID，后续才能通过 查看会话列表 查看指定智能体 ID 对应的会话列表。 |

| `connector_id` | `String` | 可选 | 1024 | 该会话在哪个渠道创建。目前支持如下渠道： API：（默认）1024  ChatSDK：999 |

## 返回参数

| 参数 | 类型 | 示例 | 说明 |

| --- | --- | --- | --- |

| `data` | Object of `ConversationData` | 详见返回示例 | 会话的基础信息。详细说明可参考`ConversationData`。 |

| `code` | `Long` | 0 | 调用状态码。0 表示调用成功，其他值表示调用失败，你可以通过 `msg` 字段判断详细的错误原因。 |

| `msg` | `String` | "Success" | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

### ConversationData

| 参数 | 类型 | 示例 | 说明 |

| --- | --- | --- | --- |

| `id` | `String` | 737999610479815**** | Conversation ID，即会话的唯一标识。 |

| `created_at` | `Long` | 1718289297 | 会话创建的时间。格式为 10 位的 Unixtime 时间戳，单位为秒。 |

| `last_section_id` | `String` | 7495664347616952360 | 会话中最新的一个上下文片段 ID。 |

## 示例

### 请求示例

创建空会话：

```
curl --location --request POST '{{host}}/v1/conversation/create'\
--header 'Authorization: Bearer pat_xitq9LWlowpX3qGCih1lwpAdzvXNqgmpfhpV28HLWFypY37xR5Uaj2GioN****' \
--header 'Content-Type: application/json' \
```

创建会话，并在会话中携带消息内容。

```
curl --location --request POST 'https://api.coze.cn/v1/conversation/create'\
--header 'Authorization: Bearer pat_xitq9LWlowpX3qGCih1lwpAdzvXNqgmpfhpV28HLWFypY37xR5Uaj2GioN****' \
--header 'Content-Type: application/json' \
--data-raw '{
    "meta_data": {
        "uuid": "newid1234"
    },
    "messages": [
        {
            "role": "user",
            "content": "[{\"type\":\"text\",\"text\":\"你好，这是我的图片\"},{\"type\":\"image\",\"file_id\":\"{{FILE_ID}}\"}]",
            "content_type": "object_string"
        },
        {
            "role": "assistant",
            "content": "你好我是一个bot",
            "content_type": "text"
        }
    ]
}'
```

### 返回示例

```
{
    "code": 0,
    "data": {
        "created_at": 1742820175,
        "id": "748535563872637****",
        "last_section_id": "748535563872637****",
    },
    "msg": ""
}
```

# 查看会话列表

查看指定智能体的会话列表。

+ 仅支持通过此 API 查看智能体在 API 或 Chat SDK 渠道产生的会话。

+ 仅支持查询本人创建的会话。

## 基础信息

| 请求方式 | GET |

| --- | --- |

| 请求地址 | `{{host}}/v1/conversations` |

| 接口说明 | 获取指定智能体的会话列表。 |

## 请求参数

### Header

| 参数 | 取值 | 说明 |

| --- | --- | --- |

| `Authorization` | Bearer `$Access_Token` | 用于验证客户端身份的个人访问令牌。你可以在扣子平台中生成个人访问令牌，详细信息，参考【准备工作】。 |

| `Content-Type` | `application/json` | 解释请求正文的方式。 |

### Query

| 参数 | 类型 | 是否必选 | 示例 | 说明 |

| --- | --- | --- | --- | --- |

| `bot_id` | `String` | 必选 | 73428668***** | 智能体 ID，获取方法如下： 进入智能体的开发页面，开发页面 URL 中 `bot` 参数后的数字就是智能体 ID。例如`https://www.coze.cn/space/341****/bot/73428668*****`，智能体 ID 为`73428668*****`。 |

| `page_num` | `Integer` | 可选 | 1 | 页码，从 1 开始，默认为 1。 |

| `page_size` | `Integer` | 可选 | 40 | 每一页的数据条目个数，默认为 50，最大为 50。 |

| `sort_order` | `String` | 可选 | ASC | 会话列表的排序方式： ASC：按创建时间升序排序，最早创建的会话排序最前。 DESC：（默认）按创建时间降序排序，最近创建的会话排序最前。 |

| `connector_id` | `String` | 可选 | 999 | 发布渠道 ID，用于筛选指定渠道的会话。仅支持查看以下渠道的会话： （默认）API 渠道，渠道 ID 为 1024。 Chat SDK 渠道，渠道 ID 为 999。 |

## 返回参数

| 参数 | 类型 | 示例 | 说明 |

| --- | --- | --- | --- |

| `code` | `Long` | 0 | 调用状态码。0 表示调用成功，其他值表示调用失败，你可以通过 `msg` 字段判断详细的错误原因。 |

| `msg` | `String` | Success | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

| `data` | Object of `ListConversationData` | [ { "created_at": 1731575569, "id": "123456789123456789", "meta_data": {}, } ] | 会话列表的详细。 |

### ListConversationData

| 参数 | 类型 | 示例 | 说明 |

| --- | --- | --- | --- |

| `has_more` | `Boolean` | false | 是否还有更多会话未在本次请求中返回。 true：还有更多未返回的会话。 false：已返回符合筛选条件的全部会话。 |

| `conversations` | Array of `ConversationData` | { "created_at": 1731575569, "id": "12345456789*****", "meta_data": {}, } | 会话的详细信息。 |

### ConversationData

| 参数 | 类型 | 示例 | 说明 |

| --- | --- | --- | --- |

| `id` | `String` | 737999610479815**** | Conversation ID，即会话的唯一标识。 |

| `created_at` | `Long` | 1718289297 | 会话创建的时间。格式为 10 位的 Unixtime 时间戳，单位为秒。 |

| `last_section_id` | `String` | 7495664347616952360 | 会话中最新的一个上下文片段 ID。 |

## 示例

### 请求示例

```
curl --location --request curl -X GET {{host}}/v1/conversations?bot_id=73428668*****&page_num=1&page_size=20 \
  -H "Authorization: Bearer pat_OYDacMzM3WyOWV3Dtj2bHRMymzxP****"
```

### 返回示例

```
{
  "code": 0,
  "msg": "",
  "data": {
    "has_more": false,
    "conversations": [
      {
        "created_at": 1731575569,
        "id": "12345***3456789",
      }
    ]
  },
  "detail": {
    "logid": "202412345678901234567890"
  }
}
```

# 发起对话

调用此接口发起一次对话，支持添加上下文和流式响应。

## 接口说明

发起对话接口用于向指定智能体发起一次对话，支持在对话时添加对话的上下文消息，以便智能体基于历史消息做出合理的回复。

Coze Studio 社区版发起对话 API 仅支持流式响应，智能体在生成回复的同时，将回复消息以数据流的形式逐条发送给客户端。处理结束后，服务端会返回一条完整的智能体回复。

## 创建会话 API 和发起对话 API 的区别

+ 创建会话：主要用于初始化一个新的会话环境。

+ 发起对话：

+ 用于在已经存在的会话中发起一次对话。

+ 支持添加上下文和流式响应。

+ 可以基于历史消息进行上下文关联，提供更符合语境的回复。



# 接口文档

## 基础信息

| 请求方式 | `POST` |

| --- | --- |

| 请求地址 | `{{host}}/v3/chat` |

| 接口说明 | 调用此接口发起一次对话，支持添加上下文和流式响应。 |

## Header

| 参数 | 取值 | 说明 |

| --- | --- | --- |

| `Authorization` | `Bearer $Access_Token` | 用于验证客户端身份的个人访问令牌。你可以在扣子平台中生成个人访问令牌，详细信息，参考【准备工作】。 |

| `Content-Type` | `application/json` | 解释请求正文的方式。 |

## Query

| 参数 | 类型 | 是否必选 | 说明 |

| --- | --- | --- | --- |

| `conversation_id` | String | 可选 | 标识对话发生在哪一次会话中。会话是智能体和用户之间的一段问答交互。一个会话包含一条或多条消息。对话是会话中对智能体的一次调用，智能体会将对话中产生的消息添加到会话中。  * 可以使用已创建的会话，会话中已存在的消息将作为上下文传递给模型。  * 对于一问一答等不需要区分 `conversation` 的场合可不传该参数，系统会自动生成一个会话。 |

## Body

| 参数 | 类型 | 是否必选 | 说明 |

| --- | --- | --- | --- |

| `bot_id` | String | 必选 | 要进行会话聊天的智能体ID。  进入智能体的 开发页面，开发页面 URL 中 `bot` 参数后的数字就是智能体ID。例如 `https://www.coze.cn/space/341****/bot/73428668*****`，智能体ID 为 `73428668*****`。 |

| `user_id` | String | 必选 | 标识当前与智能体对话的用户，由使用方自行定义、生成与维护。`user_id` 用于标识对话中的不同用户，不同的 `user_id`，其对话的上下文消息、数据库等对话记忆数据互相隔离。如果不需要用户数据隔离，可将此参数固定为一个任意字符串，例如 `123`，`abc` 等。 |

| `additional_messages` | Array of object | 可选 | 对话输入信息，你可以通过此字段传入本次对话中用户的问题。  * `additional_messages` 仅支持 `role=user` 的记录，以免影响模型效果。 |

| `stream` | Boolean | 可选 | 是否启用流式返回，目前仅支持流式响应。  * `true`：“流式响应”将模型的实时响应提供给客户端，类似打字机效果。你可以实时获取服务端返回的对话、消息事件，并在客户端中同步处理、实时展示，也可以直接在 `completed` 事件中获取智能体最终的回复。 |

| `shortcut_command` | Object | 可选 | 快捷指令信息。你可以通过此参数指定此次对话执行的快捷指令，必须是智能体已绑定的快捷指令。  消息结构可参考 `ShortcutCommandDetail` Object。  调用快捷指令，会自动根据快捷指令配置信息生成本次对话中的用户问题，并放入 `additional_messages` 最后一条消息作为本次对话的用户输入。 |

## EnterMessage Object

| 参数 | 类型 | 是否必选 | 说明 |

| --- | --- | --- | --- |

| `role` | String | 必选 | 发送这条消息的实体。取值：  * `user`：代表该条消息内容是用户发送的。  * `assistant`：代表该条消息内容是智能体发送的。 |

| `type` | String | 可选 | 消息类型。默认为 `question`。  * `question`：用户输入内容。  * `answer`：智能体返回给用户的消息内容，支持增量返回。如果工作流绑定了消息节点，可能会存在多 `answer` 场景，此时可以用流式返回的结束标志来判断所有 `answer` 完成。  * `function_call`：智能体对话过程中调用函数（function call）的中间结果。  * `tool_response`：调用工具 （function call）后返回的结果。  * `follow_up`：如果在 智能体上配置打开了用户问题建议开关，则会返回推荐问题相关的回复内容。不支持在请求中作为入参。  * `verbose`：多 `answer` 场景下，服务端会返回一个 `verbose` 包，对应的 `content` 为 JSON 格式，`content.msg_type = generate_answer_finish` 代表全部 `answer` 回复完成。不支持在请求中作为入参。  仅发起会话（v3）接口支持将此参数作为入参，且：  * 如果 `autoSaveHistory=true`，`type` 支持设置为 `question` 或 `answer`。  * 如果 `autoSaveHistory=false`，`type` 支持设置为 `question`、`answer`、`function_call`、`tool_output`/`tool_response`。  其中，`type=question` 只能和 `role=user` 对应，即仅用户角色可以且只能发起 `question` 类型的消息。 |

| `content` | String | 可选 | 消息的内容，支持纯文本、多模态（文本、图片、文件混合输入）、卡片等多种类型的内容。  * `content_type` 为 `object_string` 时，`content` 为 `object_string` object 数组序列化之后的 JSON String，详细说明可参考 `object_string` object。  * 当 `content_type = text` 时，`content` 为普通文本，例如 `&quot;content&quot; :&quot;Hello!&quot;`。 |

| `content_type` | String | 可选 | 消息内容的类型，支持设置为：  * `text`：文本。  * `object_string`：多模态内容，即文本和文件的组合、文本和图片的组合。  * `card`：卡片。此枚举值仅在接口响应中出现，不支持作为入参。  `content` 不为空时，此参数为必选。 |

## object_string object

| 参数 | 类型 | 是否必选 | 说明 |

| --- | --- | --- | --- |

| `type` | String | 必选 | 多模态消息内容类型，支持设置为：  * `text`：文本类型。  * `file`：文件类型。  * `image`：图片类型。  * `audio`：音频类型。 |

| `text` | String | 可选 | 文本内容。 |

| `file_url` | String | 可选 | 文件、图片或语音文件的在线地址。必须是可公共访问的有效地址。  在 `type` 为 `file`、`image` 或 `audio` 时，`file_id` 和 `file_url` 应至少指定一个。 |

+ 一个 `object_string` 数组中最多包含一条 `text` 类型消息，但可以包含多个 `file`、`image` 类型的消息。

+ 当 `object_string` 数组中存在 `text` 类型消息时，必须同时存在至少 1 条 `file` 或 `image` 消息，纯文本消息（仅包含 `text` 类型）需要使用 `content_type: text` 字段直接指定，不能使用 `object_string` 数组。

+ 支持发送纯图片或纯文件消息，但每条纯图片或纯文件消息的前一条或后一条消息中，必须包含一条 `content_type: text` 的纯文本消息，作为用户查询的上下文。例如， `"content": "[{\"type\":\"image\",\"file_id\":\"{{file_id_1}}\"}]"` 为一条纯图片消息，该纯图片消息的前一条或后一条消息必须是一条纯文本消息，否则接口会报 `4000` 参数错误。

例如，以下数组是一个完整的多模态内容：

序列化前：

```
[
    {
        &quot;type&quot;: &quot;text&quot;,
        &quot;text&quot;: &quot;你好我有一个帽衫，我想问问它好看么，你帮我看看&quot;
    }, 
        {
        &quot;type&quot;: &quot;file&quot;,
        &quot;file_url&quot;: &quot;{{file_url_1}}&quot;
    }
]
```

序列化后：

```
&quot;[{\&quot;type\&quot;:\&quot;text\&quot;,\&quot;text\&quot;:\&quot;你好我有一个帽衫，我想问问它好看么，你帮我看看\&quot;},{\&quot;type\&quot;:\&quot;file\&quot;,\&quot;file_url\&quot;:\&quot;{{file_url_1}}\&quot;}]&quot;
```

## ShortcutCommandDetail Object

| 参数 | 类型 | 是否必选 | 说明 |

| --- | --- | --- | --- |

| `command_id` | String | 必选 | 对话要执行的快捷指令 ID，必须是智能体已绑定的快捷指令。 |

| `parameters` | Map<String, String> | 可选 | 用户输入的快捷指令组件参数信息。  自定义键值对，其中键（`key`）为快捷指令组件的名称，值（`value`）为组件对应的用户输入，为 `object_string` object 数组序列化之后的 JSON String，详细说明可参考 `object_string` object。 |

## 流式响应

在流式响应中，服务端不会一次性发送所有数据，而是以数据流的形式逐条发送数据给客户端，数据流中包含对话过程中触发的各种事件（`event`），直至处理完毕或处理中断。处理结束后，服务端会通过 `conversation.message.completed` 事件返回拼接后完整的模型回复信息。各个事件的说明可参考流式响应事件。

流式响应允许客户端在接收到完整的数据流之前就开始处理数据，例如在对话界面实时展示智能体的回复内容，减少客户端等待模型完整回复的时间。

流式响应的整体流程如下：

+ 流式响应流程：

```
######### 整体概览 （chat, MESSAGE 两级）
# chat - 开始
# chat - 处理中
#   MESSAGE - 知识库召回
#   MESSAGE - function_call
#   MESSAGE - tool_output
#   MESSAGE - answer is card
#   MESSAGE - answer is normal text
#   MESSAGE - 多 answer 的情况下，会继续有 message.delta
#   MESSAGE - verbose （多 answer、Multi-agent 跳转等场景）
#   MESSAGE - suggestion
# chat - 完成
# 流结束 event: done
#########
```

+ 流式响应示例：



# 流式响应事件示例与对象结构

## chat - 开始

```
event: conversation.chat.created
data: {"id": "123", "conversation_id": "123", "bot_id": "222", "created_at": 1710348675, compleated_at: null, "last_error": null, "meta_data": {}, "status": "created", "usage": null}
```

## chat - 处理中

```
event: conversation.chat.in_progress
data: {"id": "123", "conversation_id": "123", "bot_id": "222", "created_at": 1710348675, compleated_at: null, "last_error": null, "meta_data": {}, "status": "in_progress", "usage": null}
```

## MESSAGE - 知识库召回

```
event: conversation.message.completed
data: {"id": "msg_001", "role": "assistant", "type": "knowledge", "content": "---\nrecall slice 1:xxxxxxx\n", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}
```

## MESSAGE - function_call

```
event: conversation.message.completed
data: {"id": "msg_002", "role": "assistant", "type": "function_call", "content": "{"name":"toutiaosousuo-search","arguments":{"cursor":0,"input_query":"今天的体育新闻","plugin_id":7281192623887548473,"api_id":7288907006982012986,"plugin_type":1}}", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}
```

## MESSAGE - toolOutput

```
event: conversation.message.completed
data: {"id": "msg_003", "role": "assistant", "type": "tool_output", "content": "........", "content_type": "card", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}
```

## MESSAGE - answer is card

```
event: conversation.message.completed
data: {"id": "msg_004", "role": "assistant", "type": "answer", "content": "{{card_json}}", "content_type": "card", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}
```

## MESSAGE - answer is normal text

```
event: conversation.message.delta
data: {"id": "msg_005", "role": "assistant", "type": "answer", "content": "以下", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}

event: conversation.message.delta
data: {"id": "msg_005", "role": "assistant", "type": "answer", "content": "是", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}

...... {{ N 个 delta 消息包}} ......

event: conversation.message.completed
data: {"id": "msg_005", "role": "assistant", "type": "answer", "content": "{{msg_005 完整的结果。即之前所有 msg_005 delta 内容拼接的结果}}", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}
```

## MESSAGE - 多 answer 的情况，会继续有 message.delta

```
event: conversation.message.delta
data: {"id": "msg_006", "role": "assistant", "type": "answer", "content": "你好你好", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}

...... {{ N 个 delta 消息包}} ......

event: conversation.message.completed
data: {"id": "msg_006", "role": "assistant", "type": "answer", "content": "{{msg_006 完整的结果。即之前所有 msg_006 delta 内容拼接的结果}}", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}
```

## MESSAGE - Verbose （流式 plugin, 多 answer 结束，Multi-agent 跳转等场景）

```
event: conversation.message.completed
data: {"id": "msg_007", "role": "assistant", "type": "verbose", "content": "{"msg_type":"generate_answer_finish","data":""}", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}
```

## MESSAGE - suggestion

```
event: conversation.message.completed
data: {"id": "msg_008", "role": "assistant", "type": "follow_up", "content": "朗尼克的报价是否会成功？", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}
event: conversation.message.completed
data: {"id": "msg_009", "role": "assistant", "type": "follow_up", "content": "中国足球能否出现？", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}
event: conversation.message.completed
data: {"id": "msg_010", "role": "assistant", "type": "follow_up", "content": "羽毛球种子选手都有谁？", "content_type": "text", "chat_id": "123", "conversation_id": "123", "bot_id": "222"}
```

## chat - 完成

```
event: conversation.chat.completed （chat完成）
data: {"id": "123", "chat_id": "123", "conversation_id": "123", "bot_id": "222", "created_at": 1710348675, compleated_at: 1710348675, "last_error": null, "meta_data": {}, "status": "compleated", "usage": {"token_count": 3397, "output_tokens": 1173, "input_tokens": 2224}}

event: done （stream流结束）
data: [DONE]
```

## chat - 失败

```
event: conversation.chat.failed
data: {
    "code": 701231,
    "msg": "error"
}
```

## 返回的事件消息体结构

| 参数 | 类型 | 说明 |
| --- | --- | --- |
| `event` | `String` | 当前流式返回的数据包事件。详细说明可参考 流式响应事件。 |
| `data` | `Object` | 消息内容。其中，`chat` 事件和 `message` 事件的格式不同。`chat` 事件中，`data` 为 `Chat Object`。`message`、`audio` 事件中，`data` 为 `Message Object`。 |

## 流式响应事件

| 事件（event）名称 | 说明 |
| --- | --- |
| `conversation.chat.created` | 创建对话的事件，表示对话开始。 |
| `conversation.chat.in_progress` | 服务端正在处理对话。 |
| `conversation.message.delta` | 增量消息，通常是 `type=answer` 时的增量消息。 |
| `conversation.audio.delta` | 增量语音消息，通常是 `type=answer` 时的增量消息。只有输入中包含语音消息时，才会返回 `audio.delta`。 |
| `conversation.message.completed` | `message` 已回复完成。此时流式包中带有所有 `message.delta` 的拼接结果，且每个消息均为 `completed` 状态。 |
| `conversation.chat.completed` | 对话完成。工具类型 |
| `conversation.chat.failed` | 此事件用于标识对话失败。 |
| `conversation.chat.requires_action` | 对话中断，需要使用方上报工具的执行结果。 |
| `error` | 流式响应过程中的错误事件。 |
| `done` | 本次会话的流式返回正常结束。 |

## 接口响应参数

| 参数 | 类型 | 说明 |
| --- | --- | --- |
| `data` | `Object` | 本次对话的基本信息。详细说明可参考 `Chat Object`。 |
| `code` | `Integer` | 状态码。 `0` 代表调用成功。 |
| `msg` | `String` | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

## Message Object

| 参数 | 类型 | 说明 |



## 消息字段

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `id` | `String` | Message ID，即消息的唯一标识。 |
| `conversation_id` | `String` | 此消息所在的会话 ID。 |
| `bot_id` | `String` | 编写此消息的智能体 ID。此参数仅在对话产生的消息中返回。 |
| `chat_id` | `String` | Chat ID。此参数仅在对话产生的消息中返回。 |
| `meta_data` | `Map` | 创建消息时的附加消息，获取消息时也会返回此附加消息。 |
| `role` | `String` | 发送这条消息的实体。取值：`user`：代表该条消息内容是用户发送的；`assistant`：代表该条消息内容是智能体发送的。 |
| `content` | `String` | 消息的内容，支持纯文本、多模态（文本、图片、文件混合输入）、卡片等多种类型的内容。 |
| `content_type` | `String` | 消息内容的类型，取值包括：`text`：文本。`object_string`：多模态内容，即文本和文件的组合、文本和图片的组合。`card`：卡片。此枚举值仅在接口响应中出现，不支持作为入参。`audio`：音频。此枚举值仅在接口响应中出现，不支持作为入参。仅当输入有 `audio` 文件时，才会返回此类型。当 `content_type` 为 `audio` 时，`content` 为 `base64` 后的音频数据。音频的编码根据输入的 `audio` 文件的不同而不同：<br> * 输入为 `wav` 格式音频时，`content` 为采样率 24kHz，`raw` 16 bit, 1 channel, `little-endian` 的 `pcm` 音频片段 `base64` 后的字符串<br> * 输入为 `ogg_opus` 格式音频时，`content` 为采样率 48kHz，1 channel，10ms 帧长的 `opus` 格式音频片段 `base64` 后的字符串 |
| `created_at` | `Integer` | 消息的创建时间，格式为 10 位的 `Unixtime` 时间戳，单位为秒（s）。 |
| `updated_at` | `Integer` | 消息的更新时间，格式为 10 位的 `Unixtime` 时间戳，单位为秒（s）。 |
| `type` | `String` | 消息类型。<br> * `question`：用户输入内容。<br> * `answer`：智能体返回给用户的消息内容，支持增量返回。如果工作流绑定了 `message` 节点，可能会存在多 `answer` 场景，此时可以用流式返回的结束标志来判断所有 `answer` 完成。<br> * `function_call`：智能体对话过程中调用函数（`function call`）的中间结果。<br> * `tool_response`：调用工具（`function call`）后返回的结果。<br> * `follow_up`：如果在智能体上配置打开了用户问题建议开关，则会返回推荐问题相关的回复内容。<br> * `verbose`：多 `answer` 场景下，服务端会返回一个 `verbose` 包，对应的 `content` 为 `JSON` 格式，`content.msg_type =generate_answer_finish` 代表全部 `answer` 回复完成。<br> 仅发起会话（v3）接口支持将此参数作为入参，且：<br> * 如果 `autoSaveHistory=true`，`type` 支持设置为 `question` 或 `answer`。<br> * 如果 `autoSaveHistory=false`，`type` 支持设置为 `question`、`answer`、`function_call`、`tool_response`。<br> 其中，`type=question` 只能和 `role=user` 对应，即仅用户角色可以且只能发起 `question` 类型的消息。 |
| `section_id` | `String` | 上下文片段 ID。每次清除上下文都会生成一个新的 `section_id`。 |
| `reasoning_content` | `String` | `DeepSeek-R1` 模型的思维链（`CoT`）。模型会将复杂问题逐步分解为多个简单步骤，并按照这些步骤逐一推导出最终答案。  该参数仅在使用 `DeepSeek-R1` 模型时才会返回。 |

## 示例

### 流式响应

#### Request

```
curl --location --request POST &#x27;https://api.coze.cn/v3/chat?conversation_id=7374752000116113452&#x27; \
--header &#x27;Authorization: Bearer pat_OYDacMzM3WyOWV3Dtj2bHRMymzxP****&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data-raw &#x27;{
    &quot;bot_id&quot;: &quot;734829333445931****&quot;,
    &quot;user_id&quot;: &quot;123456789&quot;,
    &quot;stream&quot;: true,
    &quot;auto_save_history&quot;:true,
    &quot;additional_messages&quot;:[
        {
            &quot;role&quot;:&quot;user&quot;,
            &quot;content&quot;:&quot;2024年10月1日是星期几&quot;,
            &quot;content_type&quot;:&quot;text&quot;
        }
    ]
}&#x27;
```

#### Response

```
event:conversation.chat.created
// 在 chat 事件里，data 字段中的 id 为 Chat ID，即会话 ID。
data:{&quot;id&quot;:&quot;7382159487131697202&quot;,&quot;conversation_id&quot;:&quot;7381473525342978089&quot;,&quot;bot_id&quot;:&quot;7379462189365198898&quot;,&quot;completed_at&quot;:1718792949,&quot;last_error&quot;:{&quot;code&quot;:0,&quot;msg&quot;:&quot;&quot;},&quot;status&quot;:&quot;created&quot;,&quot;usage&quot;:{&quot;token_count&quot;:0,&quot;output_count&quot;:0,&quot;input_count&quot;:0}}

event:conversation.chat.in_progress
data:{&quot;id&quot;:&quot;7382159487131697202&quot;,&quot;conversation_id&quot;:&quot;7381473525342978089&quot;,&quot;bot_id&quot;:&quot;7379462189365198898&quot;,&quot;completed_at&quot;:1718792949,&quot;last_error&quot;:{&quot;code&quot;:0,&quot;msg&quot;:&quot;&quot;},&quot;status&quot;:&quot;in_progress&quot;,&quot;usage&quot;:{&quot;token_count&quot;:0,&quot;output_count&quot;:0,&quot;input_count&quot;:0}}

event:conversation.message.delta
data:{&quot;id&quot;:&quot;7382159494123470858&quot;,&quot;conversation_id&quot;:&quot;7381473525342978089&quot;,&quot;bot_id&quot;:&quot;7379462189365198898&quot;,&quot;role&quot;:&quot;assistant&quot;,&quot;type&quot;:&quot;answer&quot;,&quot;content&quot;:&quot;2&quot;,&quot;content_type&quot;:&quot;text&quot;,&quot;chat_id&quot;:&quot;7382159487131697202&quot;}

event:conversation.message.delta
data:{&quot;id&quot;:&quot;7382159494123470858&quot;,&quot;conversation_id&quot;:&quot;7381473525342978089&quot;,&quot;bot_id&quot;:&quot;7379462189365198898&quot;,&quot;role&quot;:&quot;assistant&quot;,&quot;type&quot;:&quot;answer&quot;,&quot;content&quot;:&quot;0&quot;,&quot;content_type&quot;:&quot;text&quot;,&quot;chat_id&quot;:&quot;7382159487131697202&quot;}

//省略模型回复的部分中间事件event:conversation.message.delta
......

event:conversation.message.delta
data:{&quot;id&quot;:&quot;7382159494123470858&quot;,&quot;conversation_id&quot;:&quot;7381473525342978089&quot;,&quot;bot_id&quot;:&quot;7379462189365198898&quot;,&quot;role&quot;:&quot;assistant&quot;,&quot;type&quot;:&quot;answer&quot;,&quot;content&quot;:&quot;星期三&quot;,&quot;content_type&quot;:&quot;text&quot;,&quot;chat_id&quot;:&quot;7382159487131697202&quot;}

event:conversation.message.delta
data:{&quot;id&quot;:&quot;7382159494123470858&quot;,&quot;conversation_id&quot;:&quot;7381473525342978089&quot;,&quot;bot_id&quot;:&quot;7379462189365198898&quot;,&quot;role&quot;:&quot;assistant&quot;,&quot;type&quot;:&quot;answer&quot;,&quot;content&quot;:&quot;。&quot;,&quot;content_type&quot;:&quot;text&quot;,&quot;chat_id&quot;:&quot;7382159487131697202&quot;}

event:conversation.message.completed
data:{&quot;id&quot;:&quot;7382159494123470858&quot;,&quot;conversation_id&quot;:&quot;7381473525342978089&quot;,&quot;bot_id&quot;:&quot;7379462189365198898&quot;,&quot;role&quot;:&quot;assistant&quot;,&quot;type&quot;:&quot;answer&quot;,&quot;content&quot;:&quot;2024 年 10 月 1 日是星期三。&quot;,&quot;content_type&quot;:&quot;text&quot;,&quot;chat_id&quot;:&quot;7382159487131697202&quot;}

event:conversation.message.completed
data:{&quot;id&quot;:&quot;7382159494123552778&quot;,&quot;conversation_id&quot;:&quot;7381473525342978089&quot;,&quot;bot_id&quot;:&quot;7379462189365198898&quot;,&quot;role&quot;:&quot;assistant&quot;,&quot;type&quot;:&quot;verbose&quot;,&quot;content&quot;:&quot;{\&quot;msg_type\&quot;:\&quot;generate_answer_finish\&quot;,\&quot;data\&quot;:\&quot;\&quot;,\&quot;from_module\&quot;:null,\&quot;from_unit\&quot;:null}&quot;,&quot;content_type&quot;:&quot;text&quot;,&quot;chat_id&quot;:&quot;7382159487131697202&quot;}

event:conversation.chat.completed
data:{&quot;id&quot;:&quot;7382159487131697202&quot;,&quot;conversation_id&quot;:&quot;7381473525342978089&quot;,&quot;bot_id&quot;:&quot;7379462189365198898&quot;,&quot;completed_at&quot;:1718792949,&quot;last_error&quot;:{&quot;code&quot;:0,&quot;msg&quot;:&quot;&quot;},&quot;status&quot;:&quot;completed&quot;,&quot;usage&quot;:{&quot;token_count&quot;:633,&quot;output_count&quot;:19,&quot;input_count&quot;:614}}

event:done
data:&quot;[DONE]&quot;
```

# 查看消息列表

查看指定会话的消息列表。

`查看消息列表` API 与 `查看对话消息详情` API 的区别在于：

+ `查看消息列表` API 用于查询指定会话（`conversation`）中的消息记录，不仅包括开发者在会话中手动插入的每一条消息和用户发送的 `Query`，也包括调用发起对话 API 得到的 `type=answer` 的智能体回复，但不包括 `type=function_call`、`tool_response` 和 `follow-up` 类型的对话中间态消息。

+ `查看对话消息详情` API 通常用于非流式对话场景中，查看某次对话（`chat`）中 `type=answer` 的智能体回复及 `type=function_call`、`tool_response` 和 `follow-up` 类型的对话中间态消息。不包括用户发送的 `Query`。

## 基础信息

| 请求方式 | POST |

| --- | --- |

| 请求地址 | `{{host}}/v1/conversation/message/list` |

| 接口说明 | 调用接口查看指定会话的消息列表。 |

## 请求参数

### Header

| 参数 | 取值 | 说明 |

| --- | --- | --- |

| `Authorization` | `Bearer $AccessToken` | 用于验证客户端身份的个人访问令牌。你可以在扣子平台中生成个人访问令牌，详细信息，参考【准备工作】。 |

| `Content-Type` | `application/json` | 解释请求正文的方式。 |

### Query

| 参数 | 类型 | 是否必选 | 示例 | 说明 |

| --- | --- | --- | --- | --- |

| `conversation_id` | `Integer` | 必选 | `737363834493434****` | Conversation ID，即会话的唯一标识。可以在发起对话接口 `Response` 中查看 `conversation_id` 字段。 |

### Body

| 参数 | 类型 | 是否必选 | 示例 | 说明 |

| --- | --- | --- | --- | --- |

| `order` | `String` | 可选 | `desc` | 消息列表的排序方式。  * `desc`：（默认）按创建时间降序排序，最新的消息排序最前。  * `asc`：按创建时间升序排序，最早的消息排序最前。 |

| `chat_id` | `String` | 可选 | `737999610479815****` | 筛选指定 Chat ID 中的消息列表。发起对话接口 `Response` 中 Chat 事件的 `data.id` 字段即为 Chat ID。 |



# 获取会话消息列表

## 请求参数

| 参数 | 类型 | 是否必选 | 示例 | 说明 |
| --- | --- | --- | --- | --- |
| `before_id` | String | 可选 | `737363834493437****` | 查看指定位置之前的消息。默认为 `0`，表示不指定位置。如需向前翻页，则指定为返回结果中的 `first_id`。 |

| `after_id` | String | 可选 | `737363834493437****` | 查看指定位置之后的消息。默认为 `0`，表示不指定位置。如需向后翻页，则指定为返回结果中的 `last_id`。 |

| `limit` | Long | 可选 | `30` | 每次查询返回的数据量。默认为 `50`，取值范围为 `1~50`。 |

## 返回参数

| 参数 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |

| `data` | Array of `OpenMessageApi` | `[ { &quot;bot_id&quot;: &quot;&quot;, &quot;chat_id&quot;: &quot;&quot;, &quot;content&quot;: &quot;你的名字叫什么&quot;, &quot;content_type&quot;: &quot;text&quot;, &quot;conversation_id&quot;: &quot;737363834493434****&quot;, &quot;created_at&quot;: 1716809829, &quot;id&quot;: &quot;737363834493437****&quot;, &quot;meta_data&quot;: {}, &quot;role&quot;: &quot;user&quot;, &quot;type&quot;: &quot;&quot;, &quot;updated_at&quot;: &quot;1716809829&quot; } ]` | 消息详情。 |

| `has_more` | Boolean | `true` | 是否已返回全部消息。  * `true`：未返回全部消息，可再次调用此接口查看其他分页。  * `false`：已返回全部消息。 |

| `first_id` | String | `737363834493437****` | 返回的消息列表中，第一条消息的 Message ID。 |

| `last_id` | String | `737363834493440****` | 返回的消息列表中，最后一条消息的 Message ID。 |

| `code` | Long | `0` | 调用状态码。`0` 表示调用成功，其他值表示调用失败，你可以通过 `msg` 字段判断详细的错误原因。 |

| `msg` | String | `Success` | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

### `OpenMessageApi`

| 参数 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |

| `id` | String | `738130009748252****` | Message ID，即消息的唯一标识。 |

| `conversation_id` | String | `737999610479815****` | 此消息所在的会话 ID。 |

| `bot_id` | String | `747363834493437****` | 编写此消息的智能体 ID。此参数仅在对话产生的消息中返回。 |

| `chat_id` | String | `757363834493437****` | Chat ID。此参数仅在对话产生的消息中返回。  不同的对话中，系统会生成新的 `chat_id`。同一个用户在第一次对话和第二次对话时，`chat_id` 不一样。 |

| `meta_data` | JSON Map | `{}` | 创建消息时的附加消息，获取消息时也会返回此附加消息。 |

| `role` | String | `user` | 发送这条消息的实体。取值：  * `user`：代表该条消息内容是用户发送的。  * `assistant`：代表该条消息内容是 Bot 发送的。 |

| `content` | String | 早上好，今天星期几？ | 消息的内容，支持纯文本、多模态（文本、图片、文件混合输入）、卡片等多种类型的内容。 |

| `content_type` | String | `text` | 消息内容的类型，取值包括：  * `text`：文本。  * `object_string`：多模态内容，即文本和文件的组合、文本和图片的组合。  * `card`：卡片。此枚举值仅在接口响应中出现，不支持作为入参。 |

| `created_at` | Long | `1718592898` | 消息的创建时间，格式为 10 位的 Unixtime 时间戳，单位为秒（s）。 |

| `updated_at` | Long | `1718592898` | 消息的更新时间，格式为 10 位的 Unixtime 时间戳，单位为秒（s）。 |

| `type` | String | `question` | 消息类型。  * `question`：用户输入内容。  * `answer`：智能体返回给用户的消息内容，支持增量返回。如果工作流绑定了 `message` 节点，可能会存在多 `answer` 场景，此时可以用流式返回的结束标志来判断所有 `answer` 完成。  * `function_call`：智能体对话过程中调用函数（`function call`）的中间结果。  * `tool_response`：调用工具（`function call`）后返回的结果。  * `follow_up`：如果在 Bot 上配置打开了用户问题建议开关，则会返回推荐问题相关的回复内容。  * `verbose`：多 `answer` 场景下，服务端会返回一个 `verbose` 包，对应的 `content` 为 JSON 格式，`content.msg_type = generate_answer_finish` 代表全部 `answer` 回复完成。  仅发起会话（v3）接口支持将此参数作为入参，且：  * 如果 `autoSaveHistory=true`，`type` 支持设置为 `question` 或 `answer`。  * 如果 `autoSaveHistory=false`，`type` 支持设置为 `question`、`answer`、`function_call`、`tool_output`/`tool_response`。  其中，`type=question` 只能和 `role=user` 对应，即仅用户角色可以且只能发起 `question` 类型的消息。 |

| `section_id` | String | `757363834493437****` | 上下文片段 ID。每次清除上下文都会生成一个新的 `section_id`。 |

| `reasoning_content` | String | 好的，我现在需要给一个13岁的大学生提供学习建议。首先，我得考虑用户的情况…… | DeepSeek-R1 模型的思维链（CoT）。模型会将复杂问题逐步分解为多个简单步骤，并按照这些步骤逐一推导出最终答案。  该参数仅在使用 DeepSeek-R1 模型时才会返回。 |

## 示例

### 请求示例

```
curl --location --request POST {{host}}/v1/conversation/message/list?&amp;conversation_id=737363834493434**** \
--data-raw &#x27;{
    &quot;limit&quot;: null,
    &quot;order&quot;: &quot;asc&quot;,
    &quot;chat_id&quot;: &quot;737363834493437****&quot;,
    &quot;before_id&quot;: &quot;737363834493437****&quot;
}&#x27;
```

### 返回示例

```
{
    &quot;code&quot;: 0,
    &quot;data&quot;: [
        {
            &quot;bot_id&quot;: &quot;737363834493434****&quot;,
            &quot;chat_id&quot;: &quot;747363834493434****&quot;,
            &quot;content&quot;: &quot;你的名字叫什么&quot;,
            &quot;content_type&quot;: &quot;text&quot;,
            &quot;conversation_id&quot;: &quot;737363834493434****&quot;,
            &quot;created_at&quot;: 1716809829,
            &quot;id&quot;: &quot;737363834493437****&quot;,
            &quot;meta_data&quot;: {},
            &quot;role&quot;: &quot;user&quot;,
            &quot;type&quot;: &quot;&quot;,
            &quot;updated_at&quot;: &quot;1716809829&quot;
        },
        {
            &quot;bot_id&quot;: &quot;737363834493434****&quot;,
            &quot;chat_id&quot;: &quot;747363834493434****&quot;,
            &quot;content&quot;: &quot;我的名字叫bot&quot;,
            &quot;content_type&quot;: &quot;text&quot;,
            &quot;conversation_id&quot;: &quot;737363834493434****&quot;,
            &quot;created_at&quot;: &quot;1716809829&quot;,
            &quot;id&quot;: &quot;737363834493440****&quot;,
            &quot;meta_data&quot;: {},
            &quot;role&quot;: &quot;assistant&quot;,
            &quot;type&quot;: &quot;&quot;,
            &quot;updated_at&quot;: &quot;1716936779&quot;
        }
    ],
    &quot;first_id&quot;: &quot;737363834493437****&quot;,
    &quot;has_more&quot;: true,
    &quot;last_id&quot;: &quot;737363834493440****&quot;,
    &quot;msg&quot;: &quot;&quot;

}
```

# 清除上下文

清除指定会话中的上下文。

## 接口说明

在智能体对话场景中，上下文指模型在处理当前输入时所能参考的历史消息、对话记录，它决定了模型对当前任务的理解深度和连贯性，直接影响输出结果的准确性和相关性。多轮对话中，如果需要开启新的话题，可以调用此 API 清除上下文。清除上下文后，对话中的历史消息不会作为上下文被输入给模型，后续对话不再受之前历史消息的影响，避免信息干扰，确保对话的准确性和连贯性。

会话中的消息存储在上下文段落（`section`）中，每次调用此 API 清除上下文时，系统会自动删除旧的上下文段落，并创建新的上下文段落用于存储新的消息。再次发起对话时，新分区中的消息会作为新的上下文传递给模型参考。

+   清除上下文 API 只是清空模型可见的消息历史，不会实际删除会话中的消息，通过查看消息列表等 API 仍能查看到消息内容。

+   仅支持清除本人创建的会话的上下文。

+   [会话、对话](https://www.coze.cn/open/docs/developer_guides/coze_api_overview#fed4a54c)、消息和上下文段落的术语解释请参见基础概念。

![](img/4938a916cc4d79658fdc8890ed46396c.png)

## 基础信息

| 请求方式 | `POST` |

| --- | --- |

| 请求地址 | `{{host}}/v1/conversations/:conversation_id/clear` |

| 接口说明 | 清除指定会话中的上下文。 |

## 请求参数

### Header

| 参数 | 取值 | 说明 |
| --- | --- | --- |

| `Authorization` | `Bearer $Access_Token` | 用于验证客户端身份的个人访问令牌。你可以在扣子平台中生成个人访问令牌，详细信息，参考【准备工作】。 |

| `Content-Type` | `application/json` | 解释请求正文的方式。 |

### Path

| 参数 | 类型 | 是否必选 | 示例 | 说明 |
| --- | --- | --- | --- | --- |

| `conversation_id` | String | 必选 | `737989918257****` | [待清除上下文](https://www.coze.cn/docs/developer_guides/chat_v3)的会话 ID。你可以在发起对话接口的 `Response` 中通过 `conversation_id` 字段获取会话 ID。 |

## 返回参数

| 参数 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |

| `code` | Long | `0` | 调用状态码。`0` 表示调用成功，其他值表示调用失败，你可以通过 `msg` 字段判断详细的错误原因。 |

| `msg` | String | `"Success"` | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

| `data` | Object of `Section` | `{ "id": "1234567890123456789", "conversation_id": "1234567890123456789" }` | 上下文段落（`section` ）的详细信息。  `Section` 是一个独立的上下文段落，用于分隔不同的对话阶段或主题。`Section` 中包括上下文消息，当用户清除上下文时，系统会创建一个新的 `Section`，从而确保新的对话不受历史消息的影响。 |

### `Section`

| 参数 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |

| `id` | String | `1234567890123456789` | Session ID，即清除上下文后新创建的上下文段落（`section`）的唯一标识符。  每个上下文段落对应一批独立的上下文消息。每次清除上下文时，系统会新建一个上下文段落用于存储新的上下文消息。 |

| `conversation_id` | String | `737999610479815****` | Conversation ID，即会话的唯一标识。 |

## 示例

### 请求示例

```
curl --location --request POST {{host}}/v1/conversations/:conversation_id/clear \
--header &#x27;Authorization: Bearer pat_OYDacMzM3WyOWV3Dtj2bHRMymzxP****&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data-raw &#x27;{}&#x27;
```

### 返回示例

```
{
    &quot;code&quot;: 0,
    &quot;msg&quot;: &quot;&quot;,
    &quot;data&quot;: {
        &quot;id&quot;: &quot;12345678****56789&quot;,
        &quot;conversation_id&quot;: &quot;1234567****456789&quot;
    }
}
```

# 执行工作流

执行已发布的工作流。

## 接口说明

此接口为非流式响应模式，对于支持流式输出的节点，应使用接口 `执行工作流（流式响应）` 获取流式响应。调用接口后，你可以从响应中获得 `debug_url`，访问链接即可通过可视化界面查看工作流的试运行过程，其中包含每个执行节点的输入输出等详细信息，帮助你在线调试或排障。

## 限制说明

| 限制项 | 说明 |
| --- | --- |

| 工作流发布状态 | 必须为已发布。执行未发布的工作流会返回错误码 [`4200`](https://www.coze.cn/docs/guides/use_workflow)。创建并发布工作流的操作可参考使用工作流。 |

| 节点限制 | 工作流中不能包含消息节点、开启了流式输出的结束节点、问答节点。 |



# 执行工作流

| 关联智能体 | 调用此 API 之前，应先在扣子平台中试运行此工作流，如果试运行时需要关联智能体，则调用此 API 执行工作流时，也需要指定智能体ID。通常情况下，执行存在数据库节点、变量节点等节点的工作流需要关联智能体。 |

| 请求大小上限 | 20 MB，包括输入参数及运行期间产生的消息历史等所有相关数据。 |

| 超时时间 | [未开启工作流](https://www.coze.cn/docs/guides/workflow_limits)异步运行时，工作流整体超时时间为 10 分钟，建议执行时间控制在 5 分钟以内，否则不保障执行结果的准确性。详细说明可参考工作流使用限制。开启工作流异步运行后，工作流整体超时时间为 24 小时。 |

## 基础信息

| 请求方式 | `POST` |

| --- | --- |

| 请求地址 | `https://{{host}}/v1/workflow/run` |

| 接口说明 | 执行已发布的工作流。 |

## 请求参数

### Header

| 参数 | 取值 | 说明 |

| --- | --- | --- |

| `Authorization` | `Bearer $Access_Token` | 用于验证客户端身份的个人访问令牌。你可以在扣子平台中生成个人访问令牌，详细信息，参考【准备工作】。 |

| `Content-Type` | `application/json` | 解释请求正文的方式。 |

### Body

| 参数 | 类型 | 是否必选 | 示例 | 说明 |

| --- | --- | --- | --- | --- |

| `workflow_id` | `String` | 必选 | `73664689170551*****` | 待执行的 Workflow ID，此工作流应已发布。进入 Workflow 编排页面，在页面 URL 中，`workflow` 参数后的数字就是 Workflow ID。例如 `https://www.coze.com/work_flow?space_id=42463***&workflow_id=73505836754923***`，Workflow ID 为 `73505836754923***`。 |

| `parameters` | `Map[string]any` | 可选 | `{ "user_id":"12345", "user_name":"George" }` | [工作流开始节点](https://www.coze.cn/open/docs/developer_guides/upload_files)的输入参数及取值，你可以在指定工作流的编排页面查看参数列表。如果工作流输入参数为 `Image` 等类型的文件，可以调用上传文件 API 获取 `file_id`，在调用此 API 时，在 `parameters` 中以序列化之后的 JSON 格式传入 `file_id`。例如 `"parameters" : { "input" : "{\"file_id\": \"xxxxx\"}" }`。 |

| `bot_id` | `String` | 可选 | `73428668*****` | 需要关联的智能体 ID。部分工作流执行时需要指定关联的智能体，例如存在数据库节点、变量节点等节点的工作流。 |

| `app_id` | `String` | 可选 | `749081945898306****` | 该工作流关联的应用的 ID |

## 返回参数

| 参数 | 类型 | 示例 | 说明 |

| --- | --- | --- | --- |

| `code` | `Long` | `0` | 调用状态码。`0` 表示调用成功。其他值表示调用失败。你可以通过 `msg` 字段判断详细的错误原因。 |

| `msg` | `String` | `Success` | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

| `data` | `String` | | 工作流执行结果，通常为 JSON 序列化字符串，部分场景下可能返回非 JSON 结构的字符串。 |

| `execute_id` | `String` | `741364789030728****` | 异步执行的事件 ID。 |

| `token` | `Long` | | 预留字段，无需关注。 |

| `cost` | `String` | `0` | 预留字段，无需关注。 |

| `debug_url` | `String` | | 工作流试运行调试页面。访问此页面可查看每个工作流节点的运行结果、输入输出等信息。 |

## 示例

### 请求示例

```
curl --location --request POST &#x27;https://{{host}}/v1/workflow/run&#x27; \
--header &#x27;Authorization: Bearer pat_hfwkehfncaf****&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data-raw &#x27;{
    &quot;workflow_id&quot;: &quot;73664689170551*****&quot;,
    &quot;parameters&quot;: &quot;{\&quot;user_id\&quot;:\&quot;12345\&quot;,\&quot;user_name\&quot;:\&quot;George\&quot;}&quot;
}&#x27;
```

### 返回示例

```
{
    &quot;code&quot;: 0,
    &quot;cost&quot;: &quot;0&quot;,
    &quot;data&quot;: &quot;{\&quot;output\&quot;:\&quot;北京的经度为116.4074°E，纬度为39.9042°N。\&quot;}&quot;,
    &quot;debug_url&quot;: &quot;https://www.coze.cn/work_flow?execute_id=741364789030728****&amp;space_id=736142423532160****&amp;workflow_id=738958910358870****&quot;,
    &quot;msg&quot;: &quot;Success&quot;,
    &quot;token&quot;: 98
}
```

# 执行工作流（流式响应）

执行已发布的工作流，响应方式为流式响应。

## 接口说明

调用 API 执行工作流时，对于支持流式输出的工作流，往往需要使用流式响应方式接收响应数据，例如实时展示工作流的输出信息、呈现打字机效果等。

在流式响应中，服务端不会一次性发送所有数据，而是以数据流的形式逐条发送数据给客户端，数据流中包含工作流执行过程中触发的各种事件（`event`），直至处理完毕或处理中断。处理结束后，服务端会通过 `event: Done` 事件提示工作流执行完毕。

目前支持流式响应的工作流节点包括输出节点、问答节点和开启了流式输出的结束节点。对于不包含这些节点的工作流，可以使用执行工作流接口一次性接收响应数据。

## 限制说明

+ 通过 API 方式执行工作流前，应确认此工作流已发布，执行从未发布过的工作流时会返回错误码 `4200`。

+ 调用此 API 之前，应先在扣子平台中试运行此工作流。

+ 如果试运行时需要关联智能体，则调用此 API 执行工作流时，也需要指定 `bot_id`。通常情况下，执行存在数据库节点、变量节点等节点的工作流需要关联智能体。

+ 执行应用中的工作流时，需要指定 `app_id`。

+ 请勿同时指定 `bot_id` 和 `app_id`，否则 API 会报错 `4000`，表示请求参数错误。

+ 此接口为同步接口，如果工作流整体或某些节点运行超时，智能体可能无法提供符合预期的回复，建议将工作流的执行时间控制在 5 分钟以内。

+ 工作流支持的请求大小上限为 20MB，包括输入参数以及运行期间产生的消息历史等所有相关数据。

## 基础信息

| 请求方式 | `POST` |

| --- | --- |

| 请求地址 | `https://{{host}}/v1/workflow/stream_run` |

| 接口说明 | 执行已发布的工作流，响应方式为流式响应。 |

### Header

| 参数 | 取值 | 说明 |

| --- | --- | --- |

| `Authorization` | `Bearer $Access_Token` | 用于验证客户端身份的个人访问令牌。你可以在扣子平台中生成个人访问令牌，详细信息，参考【准备工作】。 |

| `Content-Type` | `application/json` | 解释请求正文的方式。 |

### Body

| 参数 | 类型 | 是否必选 | 说明 |

| --- | --- | --- | --- |

| `workflow_id` | `String` | 必选 | 待执行的 Workflow ID，此工作流应已发布。进入 Workflow 编排页面，在页面 URL 中，`workflow` 参数后的数字就是 Workflow ID。例如 `https://www.coze.com/work_flow?space_id=42463***&workflow_id=73505836754923***`，Workflow ID 为 `73505836754923***`。 |

| `parameters` | `Map[string]any` | 可选 | 工作流开始节点的输入参数及取值，你可以在指定工作流的编排页面查看参数列表。 |

| `bot_id` | `String` | 可选 | 需要关联的智能体ID。部分工作流执行时需要指定关联的智能体，例如存在数据库节点、变量节点等节点的工作流。 |

| `app_id` | `String` | 可选 | 工作流所在的应用 ID。你可以通过应用的业务编排页面 URL 中获取应用 ID，也就是 URL 中 `project-ide` 参数后的一串字符，例如 `https://www.coze.cncom/space/739174157340921****/project-ide/743996105122521****/workflow/744102227704147****` 中，应用的 ID 为 `743996105122521****`。仅运行扣子应用中的工作流时，才需要设置 `app_id`。智能体绑定的工作流、空间资源库中的工作流无需设置 `app_id`。 |

## 返回结果

在流式响应中，开发者需要注意是否存在丢包现象。

+ 事件 ID（`id`）默认从 `0` 开始计数，以包含 `event: Done` 的事件为结束标志。开发者应根据 `id` 确认响应消息整体无丢包现象。

+ `Message` 事件的消息 ID 默认从 `0` 开始计数，以包含 `node_is_finish : true` 的事件为结束标志。开发者应根据 `node_seq_id` 确认 `Message` 事件中每条消息均完整返回，无丢包现象。

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `id` | `Integer` | 此消息在接口响应中的事件 ID。以 `0` 为开始。 |

| `event` | `String` | 当前流式返回的数据包事件。包括以下类型：`Message`：工作流节点输出消息，例如消息节点、结束节点的输出消息。可以在 `data` 中查看具体的消息内容。`Error`：报错。可以在 `data` 中查看 `error_code` 和 `error_message`，排查问题。`Done`：结束。表示工作流执行结束，此时 `data` 中包含 debug URL。`Interrupt`：中断。表示工作流中断，此时 `data` 字段中包含具体的中断信息。`PING`：心跳信号。表示工作流执行中，消息内容为空，用于维持连接。 |

| `data` | `Object` | 事件内容。各个 `event` 类型的事件内容格式不同。 |

### Message 事件

Message 事件中，`data` 的结构如下：

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `content` | `String` | 流式输出的消息内容。 |

| `node_title` | `String` | 输出消息的节点名称，例如消息节点、结束节点。 |

| `node_seq_id` | `String` | 此消息在节点中的消息 ID，从 `0` 开始计数，例如消息节点的第 5 条消息。 |

| `node_is_finish` | `Boolean` | 当前消息是否为此节点的最后一个数据包。 |

| `ext` | `Map[String]String` | 额外字段。 |

| `cost` | `String` | 预留字段，无需关注。 |

### Interrupt 事件

Interrupt 事件中，`data` 的结构如下：

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `interrupt_data` | `Object` | 中断控制内容。 |

| `interrupt_data.event_id` | `String` | 工作流中断事件 ID，恢复运行时应回传此字段。 |

| `interrupt_data.type` | `Integer` | 工作流中断类型，恢复运行时应回传此字段。 |

| `node_title` | `String` | 输出消息的节点名称，例如“问答”。 |

### Error 事件

Error 事件中，`data` 的结构如下：

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `error_code` | `Integer` | 调用状态码。`0` 表示调用成功。其他值表示调用失败。你可以通过 `error_message` 字段判断详细的错误原因。 |

| `error_message` | `String` | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

## 示例

### 请求示例

```
curl --location --request POST &#x27;https://{{host}}/v1/workflow/stream_run&#x27; \
--header &#x27;Authorization: Bearer pat_fhwefweuk****&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data-raw &#x27;{
    &quot;workflow_id&quot;: &quot;73664689170551*****&quot;,
    &quot;{\&quot;user_id\&quot;:\&quot;12345\&quot;,\&quot;user_name\&quot;:\&quot;George\&quot;}&quot;
}&#x27;
```

### 响应示例

+ `Message`事件



# 工作流流式响应事件

```
id: 0
event: Message
data: {"content":"msg","node_is_finish":false,"node_seq_id":"0","node_title":"Message"}

id: 1
event: Message
data: {"content":"为","node_is_finish":false,"node_seq_id":"1","node_title":"Message"}

id: 2
event: Message
data: {"content":"为什么小明要带一把尺子去看电影？\n因","node_is_finish":false,"node_seq_id":"2","node_title":"Message"}

id: 3
event: Message
data: {"content":"为他听说电影很长，怕","node_is_finish":false,"node_seq_id":"3","node_title":"Message"}

id: 4
event: Message
data: {"content":"坐不下！","node_is_finish":true,"node_seq_id":"4","node_title":"Message"}

id: 5
event: Message
data: {"content":"{\"output\":\"为什么小明要带一把尺子去看电影？\\n因为他听说电影很长，怕坐不下！\"}","cost":"0.00","node_is_finish":true,"node_seq_id":"0","node_title":"","token":0}

id: 6
event: Done
data: {}
```

## Error 事件

```
id: 0
event: Error
data: {"error_code":4000,"error_message":"Request parameter error"}
```

## Interrupt 事件

```
// 流式执行工作流，触发问答节点，Bot提出问题
id: 0
event: Message
data: {"content":"请问你想查看哪个城市、哪一天的天气呢","content_type":"text","node_is_finish":true,"node_seq_id":"0","node_title":"问答"}

id: 1
event: Interrupt
data: {"interrupt_data":{"data":"","event_id":"7404830425073352713/2769808280134765896","type":2},"node_title":"问答"}
```

# 恢复运行工作流

恢复运行已中断的工作流。

## 接口说明

执行包含问答节点的工作流时，智能体会以指定问题向用户提问，并等待用户回答。此时工作流为中断状态，开发者需要调用此接口回答问题，并恢复运行工作流。如果用户的响应和智能体预期提取的信息不匹配，例如缺少必选的字段，或字段数据类型不一致，工作流会再次中断并追问。如果询问 3 次仍未收到符合预期的回复，则判定为工作流执行失败。

## 限制说明

+   最多调用此接口恢复运行 3 次，如果第三次恢复运行时智能体仍未收到符合预期的回复，则判定为工作流执行失败。

+   恢复运行后，`index` 和节点 `index` 都会重置。

+   恢复运行工作流也会产生 `token` 消耗，且与`执行工作流（流式响应）`时消耗的 `token` 数量相同。

## 基础信息

| 请求方式 | `POST` |

| --- | --- |

| 请求地址 | `https://{{host}}/v1/workflow/stream_resume` |

| 接口说明 | 恢复运行已中断的工作流。 |

### Header

| 参数 | 取值 | 说明 |

| --- | --- | --- |

| Authorization | `Bearer $Access_Token` | 用于验证客户端身份的`个人访问令牌`。你可以在扣子平台中生成个人访问令牌，详细信息，参考【准备工作】。 |

| Content-Type | `application/json` | 解释请求正文的方式。 |

### Body

| 参数 | 类型 | 是否必选 | 说明 |

| --- | --- | --- | --- |

| `workflow_id` | `String` | 必选 | 待执行的 Workflow ID，此工作流应已发布。  进入 `Workflow` 编排页面，在页面 URL 中，`workflow` 参数后的数字就是 Workflow ID。例如 `https://www.coze.com/work_flow?space_id=42463***&workflow_id=73505836754923***`，Workflow ID 为 `73505836754923***`。 |

| `event_id` | `String` | Required | 工作流执行中断事件 ID。你可以从`执行工作流（流式响应）`的响应信息中获得中断事件 ID。 |

| `resume_data` | `String` | Required | 恢复执行时，用户对智能体指定问题的回复。  如果是问答节点导致的中断，回复中应包含问答节点中的必选参数，否则工作流会再次中断并提问。 |

| `interrupt_type` | `Integer` | Required | 中断类型，你可以从`执行工作流（流式响应）`的响应信息中获得中断时间的中断类型。 |

## 返回结果

在流式响应中，开发者需要注意是否存在丢包现象。

+   事件 ID（`id`）默认从 0 开始计数，以包含 `event: Done` 的事件为结束标志。开发者应根据 `id` 确认响应消息整体无丢包现象。

+   `Message` 事件的消息 ID 默认从 0 开始计数，以包含 `node_is_finish : true` 的事件为结束标志。开发者应根据 `node_seq_id` 确认 `Message` 事件中每条消息均完整返回，无丢包现象。

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `id` | `Integer` | 此消息在接口响应中的事件 ID。以 0 为开始。 |

| `event` | `String` | 当前流式返回的数据包事件。包括以下类型：  * `Message`：工作流节点输出消息，例如消息节点、结束节点的输出消息。可以在 `data` 中查看具体的消息内容。  * `Error`：报错。可以在 `data` 中查看 `error_code` 和 `error_message`，排查问题。  * `Done`：结束。表示工作流执行结束，此时 `data` 中包含 debug URL。  * `Interrupt`：中断。表示工作流中断，此时 `data` 字段中包含具体的中断信息。  * `PING`：心跳信号。表示工作流执行中，消息内容为空，用于维持连接。 |

| `data` | `Object` | 事件内容。各个 `event` 类型的事件内容格式不同。 |

### Message 事件

`Message` 事件中，`data` 的结构如下：

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `content` | `String` | 流式输出的消息内容。 |

| `node_title` | `String` | 输出消息的节点名称，例如消息节点、结束节点。 |

| `node_seq_id` | `String` | 此消息在节点中的消息 ID，从 0 开始计数，例如消息节点的第 5 条消息。 |

| `node_is_finish` | `Boolean` | 当前消息是否为此节点的最后一个数据包。 |

| `ext` | `Map[String]String` | 额外字段。 |

| `cost` | `String` | 预留字段，无需关注。 |

### Interrupt 事件

`Interrupt` 事件中，`data` 的结构如下：

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `interrupt_data` | `Object` | 中断控制内容。 |

| `interrupt_data.event_id` | `String` | 工作流中断事件 ID，恢复运行时应回传此字段。 |

| `interrupt_data.type` | `Integer` | 工作流中断类型，恢复运行时应回传此字段。 |

| `node_title` | `String` | 输出消息的节点名称，例如“问答”。 |

### Error 事件

`Error` 事件中，`data` 的结构如下：

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `error_code` | `Integer` | 调用状态码。  * `0` 表示调用成功。  * 其他值表示调用失败。你可以通过 `error_message` 字段判断详细的错误原因。 |

| `error_message` | `String` | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

## 示例

工作流执行中断场景下，以查看天气工作为例，完整的接口调用示例如下。

1.  调用接口`执行工作流（流式响应）`，要求查看天气。

请求示例如下：

```
curl --location 'https://{{host}}/v1/workflow/stream_run' \
--header 'Authorization: Bearer pat_vTG1****' \
--header 'Content-Type: application/json' \
--data '{
    "workflow_id": "739739507914235****",
    "parameters": "{\"BOT_USER_INPUT\":\"查看天气\"}"
}'
```

1.  触发问答节点，工作流中断，响应信息中返回智能体提出的问题，要求用户提供城市和日期。

返回示例如下：

```
id: 0
event: Message
data: {"content":"请问你想查看哪个城市、哪一天的天气呢","content_type":"text","node_is_finish":true,"node_seq_id":"0","node_title":"问答"}

id: 1
event: Interrupt
data: {"interrupt_data":{"data":"","event_id":"7404831988202520614/6302059919516746633","type":2},"node_title":"问答"}
```

1.  调用接口恢复运行工作流，回复智能体城市和日期。

请求示例如下：

```
curl --location 'https://{{host}}/v1/workflow/stream_resume' \
--header 'Authorization: Bearer pat_vTG1****' \
--header 'Content-Type: application/json' \
--data '{
    "event_id":"740483727529459****/433802199567434****",
    "interrupt_type":2,
    "resume_data":"杭州，2024-08-20",
    "workflow_id":"739739507914235****"
}'
```

1.  工作流执行完毕，完成天气查询，返回工作流输出消息。

返回示例如下：

```
id: 0
event: Message
data: {"content":"{\"output\":[{\"condition\":\"中到大雨\",\"humidity\":72,\"predict_date\":\"2024-08-20\",\"temp_high\":35,\"temp_low\":26,\"weather_day\":\"中到大雨\",\"wind_dir_day\":\"西风\",\"wind_dir_night\":\"西风\",\"wind_level_day\":\"3\",\"wind_level_night\":\"3\"}]}","content_type":"text","cost":"0","node_is_finish":true,"node_seq_id":"0","node_title":"End","token":386}

id: 1
event: Done
data: {}
```

# 项目架构

Coze Studio 基于领域驱动设计（DDD）原则架构设计，项目整体结构如下：

```
├── backend/              # 后端服务
│   ├── api/              # API 处理器和路由
│   ├── application/      # 应用层，组合领域对象和基础设施实现
│   ├── conf/             # 配置文件
│   ├── crossdomain/      # 跨领域防腐层
│   ├── domain/           # 领域层，包含核心业务逻辑
│   ├── infra/            # 基础设施实现层
│   ├── pkg/              # 无外部依赖的工具方法
│   └── types/            # 类型定义
├── common/               # 公共组件
├── docker/               # Docker 配置
├── frontend/             # 前端应用
│   ├── apps/             # 应用程序
│   ├── config/           # 配置文件
│   ├── infra/            # 基础设施
│   └── packages/         # 包
├── idl/                  # 接口定义语言文件
```

+   API 层（`/backend/api`）：实现 HTTP 端点，使用 `Hertz` 服务器、处理请求/响应处理、包含中间件组件

+   应用层（`/backend/application`）：组合各种领域对象和基础设施实现、提供 API 服务、封装 API

+   领域层（`/backend/domain`）：包含核心业务逻辑、定义领域实体和值对象、实现业务规则和工作流

+   跨领域防腐层（`/backend/crossdomain`）：定义跨领域接口、实现跨领域接口、防止领域间直接依赖



+ 基础设施契约层 (`/backend/infra/contract`)：定义所有外部依赖的接口、作为领域逻辑和基础设施之间的边界、包括存储系统、缓存机制、消息队列、配置管理等接口

+ 基础设施实现层 (`/backend/infra/impl`)：实现基础设施契约层定义的接口、提供具体实现，如数据库操作、缓存机制、消息队列操作等

+ 工具包 (`/backend/pkg`)：无外部依赖的工具方法、可以被任何层直接使用

+ `docker`：Docker 配置

+ 前端应用(`frontend`)：前端应用实现

+ `idl`：后端接口定义

# 代码开发与测试

在 Coze Studio 中，前后端代码均需要遵守本文档提供的编码风格与规范。

## 代码结构

```
├── backend/              # 后端服务
│   ├── api/              # API 处理器和路由
│   │   ├── handler/      # 处理器
│   │   ├── internal/     # 内部工具
│   │   ├── middleware/   # 中间件组件
│   │   ├── model/        # API 模型定义
│   │   └── router/       # 路由定义
│   ├── application/      # 应用层，组合领域对象和基础设施实现
│   │   ├── app/          # 应用服务
│   │   ├── conversation/ # 会话应用服务
│   │   ├── knowledge/    # 知识应用服务
│   │   ├── memory/       # 内存应用服务
│   │   ├── modelmgr/     # 模型管理应用服务
│   │   ├── plugin/       # 插件应用服务
│   │   ├── prompt/       # 提示词应用服务
│   │   ├── search/       # 搜索应用服务
│   │   ├── singleagent/  # 单一代理应用服务
│   │   ├── user/         # 用户应用服务
│   │   └── workflow/     # 工作流应用服务
│   ├── conf/             # 配置文件
│   │   ├── model/        # 模型配置
│   │   ├── plugin/       # 插件配置
│   │   └── prompt/       # 提示词配置
│   ├── crossdomain/      # 跨领域防腐层
│   │   ├── contract/     # 跨领域接口定义
│   │   ├── impl/         # 跨领域接口实现
│   │   └── workflow/     # 工作流跨领域实现
│   ├── domain/           # 领域层，包含核心业务逻辑
│   │   ├── agent/        # 代理领域逻辑
│   │   ├── app/          # 应用领域逻辑
│   │   ├── conversation/ # 会话领域逻辑
│   │   ├── knowledge/    # 知识领域逻辑
│   │   ├── memory/       # 内存领域逻辑
│   │   ├── modelmgr/     # 模型管理领域逻辑
│   │   ├── plugin/       # 插件领域逻辑
│   │   ├── prompt/       # 提示词领域逻辑
│   │   ├── search/       # 搜索领域逻辑
│   │   ├── user/         # 用户领域逻辑
│   │   └── workflow/     # 工作流领域逻辑
│   ├── infra/            # 基础设施实现层
│   │   ├── contract/     # 基础设施接口定义
│   │   └── impl/         # 基础设施接口实现
│   ├── pkg/              # 无外部依赖的工具方法
│   │   ├── ctxcache/     # 上下文缓存工具
│   │   ├── errorx/       # 错误处理工具
│   │   ├── goutil/       # Go 语言工具
│   │   ├── logs/         # 日志工具
│   │   └── safego/       # 安全 Go 工具
│   └── types/            # 类型定义
│       ├── consts/       # 常量定义
│       ├── ddl/          # 数据定义语言
│       └── errno/        # 错误码定义
├── frontend/             # 前端应用
│   ├── apps/             # 应用程序
│   │   └── coze-studio/  # Coze Studio 应用
│   ├── config/           # 配置文件
│   │   ├── eslint-config/# ESLint 配置
│   │   ├── postcss-config/# PostCSS 配置
│   │   ├── rsbuild-config/# RSBuild 配置
│   │   ├── stylelint-config/# StyleLint 配置
│   │   ├── tailwind-config/# Tailwind 配置
│   │   ├── ts-config/    # TypeScript 配置
│   │   └── vitest-config/# Vitest 配置
│   ├── infra/            # 基础设施
│   │   ├── eslint-plugin/# ESLint 插件
│   │   ├── idl/          # 接口定义语言
│   │   ├── plugins/      # 插件
│   │   └── utils/        # 工具
└── packages/         # 包
    ├── agent-ide/    # 代理 IDE
    ├── arch/         # 架构
    ├── common/       # 公共组件
    ├── components/   # UI 组件
    ├── data/         # 数据
    ├── foundation/   # 基础
    ├── studio/       # Studio
    └── workflow/     # 工作流
├── idl/                  # 接口定义语言文件
```

## 基础组件

+ 后端框架：Hertz ( Cloudwego 高性能 HTTP 框架)

+ 数据存储：MySQL：结构化数据存储

+ Redis：缓存和临时数据

+ MinIO：对象存储

+ Imagex：图像、文档等各类素材上传

+ RocketMQ：消息队列

+ 容器化：Docker 和 Docker Compose

## 开发调试

当你对coze studio进行二次开发保存后，需要本地开发调试时，可以按照以下命令启动本地服务，确保在项目根目录下执行，更多命令可以参考`coze-studio/Makefile`文件。

环境要求：

`docker`、`go`（≥1.24）、`make`、`node`(>=22) 、`npm`、`rush`

调试步骤：

1. 首次在本地启动调试时，需要执行以下命令：

   ```
   # 确保 docker 是正常运行的
   make debug
   ```

2. 后面修改代码保存，直接重新编译启动`coze-server`服务，执行一下命令即可：

   ```
   make server
   ```

3. 除上述外，如果还需要调试前端代码，重新打开Terminal 程序，启动前端工程：

   ```
   bash scripts/setup_fe.sh
   cd frontend/apps/coze-studio
   npm run dev
   ```

之后，访问命令行输出的地址即可，如下图所示的 `http://localhost:8080`：

![](img/d6c0ed4f794459a39fb3ffa0f3a336f3.png)

## 后端规范

### Go 规范

Go 语言的代码规范可参考[Google规范](https://google.github.io/styleguide/go/best-practices.html)。建议使用 `gofmt` 等格式化工具进行代码格式化。

### IDL 规范

| 类别 | 说明 |
| --- | --- |
| `Service` 定义 | * 服务命名采用驼峰命名<br>* 一个`Thrift`文件只定义一个`Service`, `extends`聚合除外 |
| `Method` 定义 | * 接口命名采用驼峰命名<br>* 接口只能拥有一个参数和一个返回值，且是自定义`Struct`类型<br>* 入参须命名为`{Method}Request`，返回值命名为`{Method}Response`<br>* 每个`Request`类型须包含`Base`字段，类型`base.Base`，字段序号为255，`optional`类型<br>* 每个`Response`类型须包含`BaseResp`字段，类型`base.BaseResp`，字段序号为255 |
| `Struct` 定义 | * 结构体命名采用驼峰命名<br>* 字段命名采用蛇形命名<br>* 新增字段设置为`optional`，禁止`required`<br>* 禁止修改现有字段的`ID`和类型 |
| 枚举定义 | * 推荐使用`typedef`来定义枚举值<br>* 枚举值命名采用驼峰命名，类型和名字之间用下划线连接 |
| API定义 | * 使用Restful风格定义API<br>* 参考现有模块的API定义，风格保持一致 |
| 注解定义 | * 可参考[Hertz](https://www.cloudwego.cn/zh/docs/hertz/tutorials/toolkit/annotation/#%E6%94%AF%E6%8C%81%E7%9A%84-api-%E6%B3%A8%E8%A7%A3)支持的注解 |

### 单测规范

| 类别 | 规范说明 |
| --- | --- |
| UT 函数命名 | * 普通函数命名为`Test{FunctionName}(t *testing.T)`<br>* 对象方法命名为`Test{ObjectName}{MethodName}(t *testing.T)`<br>* 基准测试函数命名为`Benchmark{FunctionName}(b *testing.B)`<br>* 基准测试对象命名为`Benchmark{ObjectName}{MethodName}(b *testing.B)` |
| 文件命名 | 测试文件与被测试文件同名，后缀为`_test.go`，处于同一目录下 |
| 测试设计 | * 推荐使用 Table-Driven 的方式定义输入/输出，覆盖多种场景<br>* 使用`github.com/stretchr/testify`简化断言逻辑<br>* 使用`github.com/uber-go/mock`生成`Mock`对象，尽量避免`Patch`打桩的方式 |

## 前端开发规范

### 语言规范

Coze Studio 前端部分整体使用 Typescript、React、Tailwind 等框架实现，仓库内已内置丰富的代码规范检测体系，在提交代码、合入 PR 时会执行自动检测，开发者只需确保提交时未出现 Lint 报错即可。

### 单测规范

仓库中选用 `Vitest` 作为主要前端测试框架，测试文件需遵循如下规范：

+ 在 `pakcage` 根目录下设立 `__tests__` 文件夹，与 `src` 同级。

+ `Package` 内所有的测试用例，都保存在上一步创建的文件夹中。

+ 为 `src` 目录每一个源码模块 `foo.ts`，创建对应同名测试模块 `foo.test.ts`，且测试代码的目录结构与源码保持一致，方便对应。

+ 所有单测文件名均以 `test.ts` 结束。

最终形成如下结构：

![](img/9eaa43fe089a9b0b167d800f322d57e8.png)

```
infra/flags-devtool/
├── README.md
├── OWNERS
├── vitest.config.ts
├── package.json
├── src/
│   ├── index.tsx
│   ├── root.tsx
│   ├── indicator.tsx
│   ├── global.d.ts
│   ├── index.module.less
│   ├── hooks/
│   ├── utils/
│   └── config-panel/
├── stories/
├── setup/
└── __tests__/
    ├── root.test.tsx
    ├── index.test.tsx
    ├── indicator.test.tsx
    ├── hooks/
    ├── utils/
    └── config-panel/
```

上述示例中：

+ `src/root.tsx` 相关单测代码集中在 `__tests__/root.test.tsx` 中.

+ `src/config-panel/foo.tsx` 则集中在 `__tests__/config-panel/foo.test.tsx` 中，单测目录结构与源码目录结构保持一致。

另外，源码文件应与单测文件一一对应，若出现某些测试文件代码行数过多时，请不要拆解出多个单测文件，而应该优先判断对应源码模块的逻辑是否过于复杂，是否应该做进一步模块拆解。

其次，单元测试本质上就是“在可控环境中，模拟触发代码逻辑，验证执行结果”的过程，一个标准的单测用例通常包含如下要素：

+ `arrange`：调用 `vi.mock` 等接口模拟上下文状态，构建“可控”的测试环境；

+ `act`：调用测试目标代码，触发执行效果；

+ `assert`：检测，验证 `act` 的响应效果是否符合预期，注意，单测中务必包含足够完整的 `assert`，否则无法达成验证效果的目标。

建议后续 UT 代码均 `AAA(Arrange-Act-Assert)` 结构组织代码，遵循如下结构要求：

+ 除 `vi.importActual` 等特殊语句外，所有 `import` 语句均保存到文件开头；

+ `import` 语句之后，放置全局 `vi.mock` 调用，原则上应 mock 掉所有下游模块；

![](img/5d60a1aa92f0249243249fb5e797a574.png)

+ `Mock` 语句之后放置 `describe` 测试套件函数，函数内原则上不可嵌套多个 `describe`；函数内应包含多个 `it` 用例；

+ `it` 用例内部遵循 `arrange => act => asset` 顺序，例如：

![](img/a207505a6f2d03f4a7862bcffdf0a1c9.png)

完整示例：

```
import { describe, it, expect, vi, beforeEach } from 'vitest';
import { exec } from 'shelljs';

import { ensureNotUncommittedChanges } from '@/utils/git';
// 导入被mock的模块，以便我们可以访问mock函数

import { env } from '@/ai-scripts';

// arrange
// Mock shelljs
vi.mock('shelljs', () => ({
  exec: vi.fn(),
}));

// Mock ../ai-scripts
vi.mock('@/ai-scripts', () => ({
  env: vi.fn(),
}));

describe('git utils', () => {
  it('应该在 BYPASS_UNCOMMITTED_CHECK 为 true 时直接返回 true', async () => {
    // arrange
    // mock
    vi.mocked(env).mockReturnValue('true');

    // act
    const result = await ensureNotUncommittedChanges('/fake/path');

    // assert
    expect(result).toBe(true);
    expect(exec).not.toHaveBeenCalled();
  });
});
```

### 依赖管理

请尽量遵循现有依赖定义，避免引入重复依赖项，例如：

+ 目前仓库主要使用 `Rspack`、`Rollup` 作为构建工具，分别对应 Web 应用与 Node Package 场景，应避免引入 `Webpack` 等构建工具。

+ 使用 `Vitest` 作为测试框架，请勿引入 `Jest`、`Mocha` 等。

+ 使用 `@coze/coze-design` 作为 UI 框架，请勿引入其它框架。

+ 其它依赖同理。

## 测试流程

### 功能测试

本地部署后可以打开平台，测试各模块的功能是否正常。

# 故障排查

## 查看容器状态



# 容器启动与状态排查

项目启动时，会运行如下几个容器：

```
coze-server: 后端服务
coze-mysql: mysql数据库
coze-redis: redis缓存
coze-milvus: 向量数据库
coze-rocketmq-namesrv: rocketmq nameserver
coze-rocketmq-broker: rocketmq broker
coze-elasticsearch: es
coze-kibana: kibana
coze-minio: 对象存储
coze-etcd: etcd
```

你可以参考以下步骤排查容器状态。

## 排查步骤

### 检查容器启动状态

查看是否所有容器都已经启动并处于 `healthy` 状态。

```
docker ps -a
```

### 查看容器日志

如果有服务处于 `unhealty` 状态，可以查看对应组件的日志并定位报错原因。

```
docker logs coze-server #后端服务
docker logs coze-mysql #mysql数据库
docker logs coze-redis #redis缓存
docker logs coze-milvus #向量数据库
docker logs coze-rocketmq-namesrv #rocketmq nameserver
docker logs coze-rocketmq-broker #rocketmq broker
docker logs coze-elasticsearch #es
docker logs coze-kibana #kibana
docker logs coze-minio #对象存储
docker logs coze-etcd #etcd
```

### 查看基础组件数据

对于基础组件，可以参考 `docker-compose.yaml` 文件中的说明进入基础组件查看数据。

例如：

```
# mysql，查询user表的数据
docker exec -it coze-mysql mysql -u root -proot
SHOW DATABASES;
USE opencoze;
SHOW TABLES;
SELECT * FROM user LIMIT 10;
```

## 服务日志

所有容器都正常启动之后，主要关注点就是后端服务的运行日志，如果出现接口报错等情况，我们可以查看服务容器对应的日志，查看是否有错误日志：

```
docker logs coze-server
```

页面如果接口报错，可以通过F12进入浏览器控制台，查看对应报错的请求，从相应头中获取`LogID`，位于`x-log-id`，拿到`logid`后再去容器内查看日志，如果接口正常，可能会搜索不到对应日志。

![](img/79f73a01f1186abd879cd274cd8d4baa.png)

去容器内查看日志：

```
docker logs coze-server | grep {logid}
```

## 错误码

服务端的错误定义在 `backend/types/errno` 目录下，若请求返回了错误码，可以在项目中搜索错误码来简单定位问题，然后再通过以上日志查询的方式定位具体问题。
