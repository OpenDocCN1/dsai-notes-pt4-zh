
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
