# 该文档介绍了 NLWeb 项目的配置文件及其作用。

> 来源：[https://qianniucity.feishu.cn/docx/DKpZdQakkoyZkDxyriWcgmrcnsc](https://qianniucity.feishu.cn/docx/DKpZdQakkoyZkDxyriWcgmrcnsc)

主要内容总结如下：

+   NLWeb 支持多种向量存储、LLM（大语言模型）和嵌入（embedding）提供商，所有配置都在 config 目录下的 YAML 文件中。

+   配置文件包括：

+   ⁠llm⁠：配置可用的 LLM 及其相关环境变量（如 endpoint、API key），可设置默认模型和不同调用场景下的模型优先级。

+   ⁠embedding⁠：配置嵌入提供商、endpoint 等，嵌入用于向量存储的创建和检索，需保持一致。

+   ⁠retrieval⁠：配置可用的向量存储，目前只查询一个，未来会支持多存储。支持 Restful 向量存储，可将其他 NLWeb 实例作为后端。假设向量存储返回 JSON 格式的数据库条目。

+   ⁠nlweb⁠：指定用于数据上传的 JSON 文件位置，以及与聊天机器人通信时的提示词。

+   ⁠webserver⁠：配置服务器参数，可选择开发或生产模式，开发模式下可用查询参数覆盖部分配置。

这些配置文件为 NLWeb 的灵活部署和扩展提供了基础。