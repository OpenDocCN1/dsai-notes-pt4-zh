
# 如何为 NLWeb 添加新的大语言模型（LLM）或检索（向量数据库）提供商的集成方法和所需步骤

> 来源：[https://qianniucity.feishu.cn/docx/GpXbdirIpoZ09ux7PH8cdZQznSg](https://qianniucity.feishu.cn/docx/GpXbdirIpoZ09ux7PH8cdZQznSg)

## 主要内容总结

NLWeb 支持开放标准，允许用户和开发者集成新的模型或检索后端。

### 添加新 LLM 提供商的步骤

1. 在 `config/config_llm.yaml` 中添加 `provider` 配置，包括 API key、`endpoint` 变量名，以及高/低模型名。
2. 在 `code/env.template` 中补充相关环境变量。
3. 在 `code/llm/` 目录下实现 `LLMProvider` 接口。
4. 在 `code/llm/llm.py` 注册新 `provider`。
5. 在 `docs/` 目录下添加该模型的文档说明。

### 添加新检索（向量数据库）提供商的步骤

1. 在 `config/config_retrieval.yaml` 中添加 `endpoint` 配置，包括数据库路径或云服务的 API key、`endpoint` 变量名等。
2. 在 `code/env.template` 中补充相关环境变量。
3. 在 `code/retrieval/` 目录下实现 `VectorDBClientInterface`。
4. 在 `code/retrieval/retriever.py` 中注册新 `provider`。
5. 在 `tools/` 目录下添加数据加载脚本。
6. 在 `docs/` 目录下添加该检索器的文档说明。

### 目的

通过上述 checklist，确保新模型或检索后端能被 NLWeb 平滑集成和使用，方便社区贡献和扩展。

整体上，文档为开发者提供了清晰的集成指引，支持 NLWeb 的可扩展性和开放性。
