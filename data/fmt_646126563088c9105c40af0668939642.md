
# Coze Studio开源AI Agent框架

> 来源：[https://gotoaiworld.feishu.cn/docx/HbmadvG2noASPdxCRDYcHJyinOe](https://gotoaiworld.feishu.cn/docx/HbmadvG2noASPdxCRDYcHJyinOe)

![](img/3efad14fab32180e41bd9c3db96aea83.png)

[Coze Studio](https://github.com/coze-dev/coze-studio/blob/main/README.zh_CN.md#%E4%BB%80%E4%B9%88%E6%98%AF-coze-studio) • [功能清单](https://github.com/coze-dev/coze-studio/blob/main/README.zh_CN.md#%E5%8A%9F%E8%83%BD%E6%B8%85%E5%8D%95) • [快速开始](https://github.com/coze-dev/coze-studio/blob/main/README.zh_CN.md#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B) • [开发指南](https://github.com/coze-dev/coze-studio/blob/main/README.zh_CN.md#%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%97)

[https://github.com/coze-dev/coze-studio](https://github.com/coze-dev/coze-studio)

[English](https://github.com/coze-dev/coze-studio/blob/main/README.md) | 中文

## 什么是 Coze Studio

[Coze Studio](https://www.coze.cn/home) 是一站式 AI Agent 开发工具。提供各类最新大模型和工具、多种开发模式和框架，从开发到部署，为你提供最便捷的 AI Agent 开发环境。

+ 提供 AI Agent 开发所需的全部核心技术：Prompt、RAG、Plugin、Workflow，使得开发者可以聚焦创造 AI 核心价值。

+ 开箱即用，用最低的成本开发最专业的 AI Agent：Coze Studio 为开发者提供了健全的应用模板和编排框架，你可以基于它们快速构建各种 AI Agent，将创意变为现实。

Coze Studio，源自服务了上万家企业、数百万开发者的「扣子开发平台」，我们将它的核心引擎完全开放。它是一个一站式的 AI Agent 可视化开发工具，让 AI Agent 的创建、调试和部署变得前所未有的简单。通过 Coze Studio 提供的可视化设计与编排工具，开发者可以通过零代码或低代码的方式，快速打造和调试智能体、应用和工作流，实现强大的 AI 应用开发和更多定制化业务逻辑，是构建低代码 AI 产品的理想选择。Coze Studio 致力于降低 AI Agent 开发与应用门槛，鼓励社区共建和分享交流，助你在 AI 领域进行更深层次的探索与实践。

Coze Studio 的后端采用 Golang 开发，前端使用 React + TypeScript，整体基于微服务架构并遵循领域驱动设计（DDD）原则构建。为开发者提供一个高性能、高扩展性、易于二次开发的底层框架，助力开发者应对复杂的业务需求。

## 功能清单

## 快速开始

了解如何获取并部署 Coze Studio 开源版，快速构建项目、体验 Coze Studio 开源版。

环境要求：

+ 在安装 Coze Studio 之前，请确保您的机器满足以下最低系统要求：2 Core、4 GB

+ 提前安装 Docker、Docker Compose，并启动 Docker 服务。

部署步骤：

1. 获取源码。

   ### 克隆代码

   ```bash
   git clone https://github.com/coze-dev/coze-studio.git
   ```

1. 配置模型。

   从模板目录复制 `doubao-seed-1.6` 模型的模版文件，并粘贴到配置文件目录。

   ### 复制模型配置模版

   ```bash
   cp backend/conf/model/template/model_template_ark_doubao-seed-1.6.yaml backend/conf/model/ark_doubao-seed-1.6.yaml
   ```

1. 在配置文件目录下，修改模版文件。

   进入目录 `backend/conf/model`。打开复制后的文件 `ark_doubao-seed-1.6.yaml`。

   设置 `id`、`meta.conn_config.api_key`、`meta.conn_config.model` 字段，并保存文件。

   + `id`：Coze Studio 中的模型 ID，由开发者自行定义，必须是非 0 的整数，且全局唯一。智能体或工作流根据模型 ID 来调用模型。对于已上线的模型，请勿修改模型 ID，否则可能导致模型调用失败。

   + [`meta.conn_config.api_key`](https://www.volcengine.com/docs/82379/1541594)：模型服务的 API Key，在本示例中为火山方舟的 API Key，获取方式可参考获取火山方舟 API Key 或获取 Byteplus ModelArk API Key。

   + `meta.conn_config.model`：模型服务的 Model name，在本示例中为火山方舟的 Model ID 或 Endpoint ID，获取方式可参考 [获取火山方舟 Model ID / 获取火山方舟 Endpoint ID](https://www.volcengine.com/docs/82379/1513689)，或者参考获取 BytePlus ModelArk Model ID / 获取 BytePlus ModelArk Endpoint ID。

1. 中国境内用户可选用火山方舟（Volcengine Ark），非中国境内的用户则可用 BytePlus ModelArk。

1. 部署并启动服务。 首次部署并启动 Coze Studio 需要拉取镜像、构建本地镜像，可能耗时较久，请耐心等待。部署过程中，你会看到以下日志信息。如果看到提示 `Container coze-server Started`，表示 Coze Studio 服务已成功启动。

   ### 启动服务

   ```bash
   cd docker
   cp .env.example .env
   docker compose up -d
   ```

1. 启动失败常见问题可参考[常见问题](https://github.com/coze-dev/coze-studio/wiki/9.-%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)。

1. 启动服务后，通过浏览器访问 `http://localhost:8888/` 即可打开 Coze Studio。

> **Warning**
>
> 如果要将 [Coze Studio](https://github.com/coze-dev/coze-studio/wiki/2.-%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B#%E5%85%AC%E7%BD%91%E5%AE%89%E5%85%A8%E9%A3%8E%E9%99%A9) 部署到公网环境，建议在部署前评估整体安全风险，例如账号注册功能、工作流代码节点 Python 执行环境、Coze Server 监听地址配置、SSRF 和部分 API 水平越权的风险，并采取相应防护措施。详细信息可参考快速开始。

## 开发指南

+ 项目配置：

+ [模型配置](https://github.com/coze-dev/coze-studio/wiki/3.-%E6%A8%A1%E5%9E%8B%E9%85%8D%E7%BD%AE)：部署 Coze Studio 开源版之前，必须配置模型服务，否则无法在搭建智能体、工作流和应用时选择模型。

+ [插件配置](https://github.com/coze-dev/coze-studio/wiki/4.-%E6%8F%92%E4%BB%B6%E9%85%8D%E7%BD%AE)：如需使用插件商店中的官方插件，必须先配置插件，添加第三方服务的鉴权秘钥。

+ [基础组件配置](https://github.com/coze-dev/coze-studio/wiki/5.-%E5%9F%BA%E7%A1%80%E7%BB%84%E4%BB%B6%E9%85%8D%E7%BD%AE)：了解如何配置图片上传等组件，以便在 Coze Studio 中使用上传图片等功能。

+ [API 参考](https://github.com/coze-dev/coze-studio/wiki/6.-API-%E5%8F%82%E8%80%83)：Coze Studio 社区版 API 和 Chat SDK 通过个人访问令牌鉴权，提供对话和工作流相关 API。

+ [开发规范](https://github.com/coze-dev/coze-studio/wiki/7.-%E5%BC%80%E5%8F%91%E8%A7%84%E8%8C%83)：

  + [项目架构](https://github.com/coze-dev/coze-studio/wiki/7.-%E5%BC%80%E5%8F%91%E8%A7%84%E8%8C%83#%E9%A1%B9%E7%9B%AE%E6%9E%B6%E6%9E%84)：了解 Coze Studio 开源版的技术架构与核心组件。

  + [代码开发与测试](https://github.com/coze-dev/coze-studio/wiki/7.-%E5%BC%80%E5%8F%91%E8%A7%84%E8%8C%83#%E4%BB%A3%E7%A0%81%E5%BC%80%E5%8F%91%E4%B8%8E%E6%B5%8B%E8%AF%95)：了解如何基于 Coze Studio 开源版进行二次开发与测试。

  + [故障排查](https://github.com/coze-dev/coze-studio/wiki/7.-%E5%BC%80%E5%8F%91%E8%A7%84%E8%8C%83#%E6%95%85%E9%9A%9C%E6%8E%92%E6%9F%A5)：了解如何查看容器状态、系统日志。

## 使用 Coze Studio 开源版

> [关于如何使用 Coze Studio](https://www.coze.cn/open/docs)，可参考扣子开发平台官方文档中心获取更多资料。需要注意的是，音色等部分功能限商业版本使用，开源版与商业版的功能差异可参考功能清单。

+ [快速入门](https://www.coze.cn/open/docs/guides/quickstart)：通过 Coze Studio 快速搭建一个 AI 助手智能体。

+ [开发智能体](https://www.coze.cn/open/docs/guides/agent_overview)：如何创建、编排、发布与管理智能体。你可以使用知识、插件等功能解决模型幻觉、专业领域知识不足等问题。除此之外，Coze Studio 还提供了丰富的记忆功能，使智能体在与个人用户交互时，可根据个人用户的历史对话等生成更准确性的回复。

+ [开发工作流](https://www.coze.cn/open/docs/guides/workflow)：工作流是一系列可执行指令的集合，用于实现业务逻辑或完成特定任务。它为应用/智能体的数据流动和任务处理提供了一个结构化框架。 Coze Studio 提供了一个可视化画布，你可以通过拖拽节点迅速搭建工作流。

+ [插件等资源](https://www.coze.cn/open/docs/guides/plugin)：在 Coze Studio，工作流、插件、数据库、知识库和变量统称为资源。

+ [API & SDK：Coze Studio 支持对话和工作流相关 API](https://github.com/coze-dev/coze-studio/wiki/6.-API-%E5%8F%82%E8%80%83)，你也可以通过 [Chat SDK](https://www.coze.cn/open/docs/developer_guides/web_sdk_overview) 将智能体或应用集成到本地业务系统。

+ [实践教程](https://www.coze.cn/open/docs/tutorial/chat_sdk_web_online_customer_service)：了解如何通过 Coze Studio 实现各种 AI 场景，例如通过 Chat SDK 搭建网页在线客服。

## License

[本项目采用 Apache 2.0 许可证](https://github.com/coze-dev/coze-studio/blob/main/LICENSE-APACHE)。详情请参阅 `LICENSE` 文件。

## 社区贡献

[我们欢迎社区贡献](https://github.com/coze-dev/coze-studio/blob/main/CONTRIBUTING.md)，贡献指南参见 [CONTRIBUTING](https://github.com/coze-dev/coze-studio/blob/main/CONTRIBUTING.md) 和 [Code of conduct](https://github.com/coze-dev/coze-studio/blob/main/CODE_OF_CONDUCT.md)，期待您的贡献！

## 安全与隐私

如果你在该项目中发现潜在的安全问题，[或你认为可能发现了安全问题](https://security.bytedance.com/src)，请通过我们的安全中心 或 [漏洞报告邮箱](mailto:sec@bytedance.com) 通知字节跳动安全团队。请不要创建公开的 GitHub Issue。

## 加入社区

我们致力于构建一个开放、友好的开发者社区，欢迎所有对 AI Agent 开发感兴趣的开发者加入我们！

### 🐛 问题反馈与功能建议

为了更高效地跟踪和解决问题，保证信息透明和便于协同，我们推荐通过以下方式参与：

+ GitHub Issues：[提交 Bug 报告或功能请求](https://github.com/coze-dev/coze-studio/issues)

+ Pull Requests：[贡献代码或文档改进](https://github.com/coze-dev/coze-studio/pulls)

### 💬 技术交流与讨论

加入我们的技术交流群，与其他开发者分享经验、获取项目最新动态：

飞书群聊

使用飞书移动端扫描下方二维码加入：

![](img/6a0d7681343e82b6360caa3f123582b9.png)

Discord 服务器

点击加入：[Coze Community](https://discord.gg/sTVN9EVS4B)

Telegram 群组

点击加入：[Telegram Group Coze](https://t.me/+pP9CkPnomDA0Mjgx)

## 致谢

感谢所有为 Coze Studio 项目做出贡献的开发者和社区成员。特别感谢：

+ [Eino](https://github.com/cloudwego/eino) 框架团队 - 为 Coze Studio 的智能体和工作流运行时、模型抽象封装、知识库索引构建和检索提供了强大的支持

+ [FlowGram](https://github.com/bytedance/flowgram.ai) 团队 - 为 Coze Studio 的工作流画布编辑页提供了高质量的流程搭建引擎

+ [Hertz](https://github.com/cloudwego/hertz) 团队 - 高性能、强扩展性的 Go HTTP 框架，用于构建微服务

+ 所有参与测试和反馈的用户
