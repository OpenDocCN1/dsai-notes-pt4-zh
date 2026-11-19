
# DIFY 场景演练

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/FOaRdeBePoucByxTz5fc66EPnPh](https://zsyhjtnsa5.feishu.cn/docx/FOaRdeBePoucByxTz5fc66EPnPh)

> Dify 是一个开源的 LLM 应用开发平台。其直观的界面结合了 AI 工作流、RAG 管道、Agent、模型管理、可观测性功能等，让您可以快速从原型到生产。

## 环境准备

### 安装 Docker 和 `docker-compose`

### 安装 Golang

1. [从 Go 的官方网站](https://golang.org/dl/) 下载版本为 1.22.2 的二进制文件。你可以在此处找到所有已发布版本的下载链接。

使用 `wget` 命令下载文件：

```
wget https://golang.org/dl/go1.22.2.linux-amd64.tar.gz
```

1. 下载完成后，解压压缩文件到 `/usr/local` 目录。

```
sudo tar -xvf go1.22.2.linux-amd64.tar.gz -C /usr/local
```

1. 将 Go 的路径添加到你的 `PATH` 环境变量中。你可以在你的 `~/.bashrc` 或 `~/.profile` 文件中执行此操作。

使用如下命令打开 `.bashrc` 文件：

```
nano ~/.bashrc
```

在文件的末尾添加如下行：

```
export PATH=$PATH:/usr/local/go/bin
```

保存并关闭文件。

1. 让 `.bashrc` 文件中的更改立即生效。

```
source ~/.bashrc
```

1. 最后，你应该可以运行以下命令来验证 Go 是否已经成功安装：

```
go version
```

如果 Go 安装成功，那么这个命令会输出你现在已经安装的 Go 版本。

## Lab 1：基本功能演练

### 创建 DIFY 堆栈

Clone 项目 Repo

```
git clone https://gitee.com/cloudzun/dify-with-wordpress
```

启动 DIFY Stack

```
cd dify-with-wordpress/
docker-compose up -d
```

```
version: '3'
services:
  # API service
  api:
    image: langgenius/dify-api:0.6.4
    restart: always
    env_file:
      - ./config/api.env
      - ./config/middleware.env
    depends_on:
      - db
      - redis
    volumes:
      # Mount the storage directory to the container, for storing user files.
      - ./volumes/app/storage:/app/api/storage
    # uncomment to expose dify-api port to host
    # ports:
    #   - "5001:5001"

  # worker service
  # The Celery worker for processing the queue.
  worker:
    image: langgenius/dify-api:0.6.4
    restart: always
    env_file:
      - ./config/worker.env
      - ./config/middleware.env
    depends_on:
      - db
      - redis
    volumes:
      # Mount the storage directory to the container, for storing user files.
      - ./volumes/app/storage:/app/api/storage

  # Frontend web application.
  web:
    image: langgenius/dify-web:0.6.4
    restart: always
    environment:
      EDITION: SELF_HOSTED
      # The base URL of console application api server, refers to the Console base URL of WEB service if console domain is
      # different from api or web app domain.
      # example: http://cloud.dify.ai
      CONSOLE_API_URL: ''
      # The URL for Web APP api server, refers to the Web App base URL of WEB service if web app domain is different from
      # console or api domain.
      # example: http://udify.app
      APP_API_URL: ''
      # The DSN for Sentry error reporting. If not set, Sentry error reporting will be disabled.
      SENTRY_DSN: ''
    # uncomment to expose dify-web port to host
    # ports:
    #   - "3000:3000"

  # The postgres database.
  db:
    image: postgres:15-alpine
    restart: always
    environment:
      PGUSER: postgres
      # The password for the default postgres user.
      POSTGRES_PASSWORD: difyai123456
      # The name of the default postgres database.
      POSTGRES_DB: dify
      # postgres data directory
      PGDATA: /var/lib/postgresql/data/pgdata
    volumes:
      - ./volumes/db/data:/var/lib/postgresql/data
    # uncomment to expose db(postgresql) port to host
    # ports:
    #   - "5432:5432"
    healthcheck:
      test: ["CMD", "pg_isready"]
      interval: 1s
      timeout: 3s
      retries: 30

  # The redis cache.
  redis:
    image: redis:6-alpine
    restart: always
    volumes:
      # Mount the redis data directory to the container.
      - ./volumes/redis/data:/data
    # Set the redis password when startup redis server.
    command: redis-server --requirepass difyai123456
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
    # uncomment to expose redis port to host
    # ports:
    #   - "6333:6333"

  # The Weaviate vector store.
  weaviate:
    image: semitechnologies/weaviate:1.19.0
    restart: always
    volumes:
      # Mount the Weaviate data directory to the container.
      - ./volumes/weaviate:/var/lib/weaviate
    environment:
      # The Weaviate configurations
      # You can refer to the [Weaviate](https://weaviate.io/developers/weaviate/config-refs/env-vars) documentation for more information.
      QUERY_DEFAULTS_LIMIT: 25
      AUTHENTICATION_ANONYMOUS_ACCESS_ENABLED: 'false'
      PERSISTENCE_DATA_PATH: '/var/lib/weaviate'
      DEFAULT_VECTORIZER_MODULE: 'none'
      CLUSTER_HOSTNAME: 'node1'
      AUTHENTICATION_APIKEY_ENABLED: 'true'
      AUTHENTICATION_APIKEY_ALLOWED_KEYS: 'WVF5YThaHlkYwhGUSmCRgsX3tD5ngdN8pkih'
      AUTHENTICATION_APIKEY_USERS: 'hello@dify.ai'
      AUTHORIZATION_ADMINLIST_ENABLED: 'true'
      AUTHORIZATION_ADMINLIST_USERS: 'hello@dify.ai'
    # uncomment to expose weaviate port to host
    # ports:
    #  - "8080:8080"

  # The DifySandbox
  sandbox:
    image: langgenius/dify-sandbox:latest
    restart: always
    cap_add:
    # Why is sys_admin permission needed?
    # https://docs.dify.ai/getting-started/install-self-hosted/install-faq#id-16.-why-is-sys_admin-permission-needed
      - SYS_ADMIN
    environment:
      # The DifySandbox configurations
      API_KEY: dify-sandbox
      GIN_MODE: release
      WORKER_TIMEOUT: 15

  # Qdrant vector store.
  # uncomment to use qdrant as vector store.
  # (if uncommented, you need to comment out the weaviate service above,
  # and set VECTOR_STORE to qdrant in the api & worker service.)
  # qdrant:
  #   image: langgenius/qdrant:v1.7.3
  #   restart: always
  #   volumes:
  #     - ./volumes/qdrant:/qdrant/storage
  #   environment:
  #     QDRANT_API_KEY: 'difyai123456'
  #   # uncomment to expose qdrant port to host
  #   # ports:
  #    - "6333:6333"

  # The nginx reverse proxy.
  # used for reverse proxying the API service and Web service.
  nginx:
    image: nginx:latest
    restart: always
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - api
      - web
    ports:
      - "8082:80"
      #- "443:443"


  wordpress:
    image: soulteary/wp-api:6.5.2-sqlite
    restart: always
    ports:
      - 8083:80
    volumes:
      - ./wordpress:/var/www/html
```

> 这个 `docker-compose.yml` 文件定义了 10 个服务，每个服务都会运行在一个独立的容器中。以下是每个服务的简单描述和它们之间的关系：

1. `api`：这是主 API 服务，使用了 `langgenius/dify-api:0.6.4` 镜像。它依赖于 `db` 和 `redis` 服务，并将 `./volumes/app/storage` 目录挂载到容器的 `/app/api/storage` 目录。

1. `worker`：这是一个 Celery worker 服务，用于处理队列任务。它使用了同样的镜像，并且也依赖于 `db` 和 `redis` 服务。它也将同样的存储目录挂载到了容器中。

1. `web`：这是前端 Web 应用服务，使用了 `langgenius/dify-web:0.6.4` 镜像。

1. `db`：这是 Postgres 数据库服务，使用了 `postgres:15-alpine` 镜像。它将 `./volumes/db/data` 目录挂载到容器的 `/var/lib/postgresql/data` 目录。

1. `redis`：这是 Redis 缓存服务，使用了 `redis:6-alpine` 镜像。它将 `./volumes/redis/data` 目录挂载到容器的 `/data` 目录。

1. `weaviate`：这是 Weaviate 向量存储服务，使用了 `semitechnologies/weaviate:1.19.0` 镜像。它将 `./volumes/weaviate` 目录挂载到容器的 `/var/lib/weaviate` 目录。

1. `sandbox`：这是 DifySandbox 服务，使用了 `langgenius/dify-sandbox:latest` 镜像。

1. `nginx`：这是 Nginx 反向代理服务，使用了 `nginx:latest` 镜像。它依赖于 `api` 和 `web` 服务，并将 `./nginx.conf` 文件挂载到容器的 `/etc/nginx/nginx.conf` 文件。

1. `wordpress`：这是 WordPress 服务，使用了 `soulteary/wp-api:6.5.2-sqlite` 镜像。它将 `./wordpress` 目录挂载到容器的 `/var/www/html` 目录。

1. `qdrant`：这是 Qdrant 向量存储服务，目前被注释掉了。如果启用，需要在 `api` 和 `worker` 服务中设置 `VECTOR_STORE` 为 `qdrant`，并注释掉 `weaviate` 服务。

> 这些服务之间的关系主要通过 `depends_on` 字段和环境变量来定义。例如，`api` 和 `worker` 服务都依赖于 `db` 和 `redis` 服务，这意味着 `db` 和 `redis` 服务会在 `api` 和 `worker` 服务之前启动。`nginx` 服务依赖于 `api` 和 `web` 服务，这意味着 `api` 和 `web` 服务会在 `nginx` 服务之前启动。环境变量则用于配置服务之间的连接，例如数据库的 URL 和密码。

![](img/d7c65a4c45c98b8da934af9ef286b5eb.png)

初始化

+ `http://localhost:8082/install` 来初始化和访问 Dify

+ `http://localhost:8083` 来初始化和访问 WordPress

### 初始化 DIFY 中的模型配置（以月之暗面为例）

点击界面右上角的用户头像，在下拉菜单中点击“设置”，在弹出窗口中选择左侧的“模型供应商”菜单，能够看到 Dify 支持配置使用的所有模型类型。

![](img/4dc004df14d5880285cc1b1e93140c94.png)

在列表中往下拉，找到“月之暗面”，然后把我们的模型 API Token 配置到 Dify 中。

![](img/222cf3c136154c2a78bb8c496088d80f.png)

配置完毕之后，在这个弹出窗口的顶端选择“系统模型设置”，将“系统推理模型”设置为反应最快、成本最低的 8K 模型。



![](img/aad61f5d6c687e614a2319e8a00da3b4.png)

当两个配置都设置完毕后，这个弹出窗中展示的模型在 Dify 中就完全可用啦。

## RAG 知识库问答

创建知识库

使用 [Azure OpenAI 服务常见问题解答](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/faq) - Azure AI services 作为样本，建议文件保存为 markdown 格式

![](img/edd6ce72b05c080a43acc8cdada0d181.png)

上传样本文件

![](img/af7ccd2a918cbf18f0315e5d7204781b.png)

选择经济模式使用本地的向量数据库

提示词模板

```
- 你是一个Azure Open AI 技术专家
- 所有的回答必须使用简体中文
- 回答内容必须基于上下文知识库里的内容进行回答
- 如果知识库里未找到答案，请回答不知道
- 不能让用户知道你是从知识库里获取的答案
```

![](img/ace731450b186ac6a6220eda664adeb7.png)

## 美股分析

提示词模板

```
# 职位描述：数据分析协作伙伴
## 角色
我的主要目标是为用户提供专业的数据分析建议。使用广泛且详细的数据。告诉我您想要分析的股票（带有股票代码），我将作为一个专家，为该股票进行基本面、技术面、市场情绪和宏观经济分析。

## 技能
### 技能1：使用雅虎财经的“股票代码”搜索股票信息
### 技能2：使用“新闻”搜索目标公司的最新新闻
### 技能3：使用“分析”搜索目标公司的财务数据和分析
### 技能4：使用“获取当前时间”确定当前的时间，在后续响应用户查询的时候，明确时间点。

## 工作流程
询问用户需要分析哪些带有股票代码的股票，然后按顺序执行以下分析。
**第一部分：基本面分析：财务报告分析**
*目标1：深入分析目标公司的财务状况。
*步骤：
1. 确定分析对象：
&lt;记录1.1：介绍{{公司}}的基本信息&gt;

2. 获取财务报告
&lt;使用工具：“股票代码”、“新闻”和“分析”&gt;
- 通过雅虎财经获取目标公司{{公司}}最新财务报告的关键数据。

&lt;记录1.2：记录分析结果获取日期和来源链接&gt;

3. 垂直分析：
- 获取公司资产负债表、损益表和现金流量表的洞察。
- 分析损益表：分析每种收入和支出占总收入的比例。
/分析资产负债表：分析每项资产和负债占总资产或总负债的比例。
/分析现金流量表
-&lt;记录1.3：记录资产负债表、现金流量表和损益表的分析结果&gt;

4. 比率分析：
- 分析公司的盈利能力比率、偿债能力比率、运营效率比率和市场表现比率。
（盈利能力比率：如净利润率、毛利率、营业利润率，以评估公司的盈利能力。）
（偿债能力比率：如资产负债比率、利息保障倍数，以评估公司的偿债能力。）
（运营效率比率：如存货周转率、应收账款周转率，以评估公司的运营效率。）
（市场表现比率：如市盈率、市净率，以评估公司的市场表现。）
-&lt;记录1.4：记录分析结论和结果&gt;

5. 综合分析和结论：
- 综合上述分析，全面评估公司的财务健康状况、盈利能力、偿债能力和运营效率。识别公司面临的主要财务风险和潜在机会。
-&lt;记录1.5：记录总体结论、风险和机会&gt;

组织并输出[记录1.1] [记录1.2] [记录1.3] [记录1.4] [记录1.5]

**第二部分：基本面分析：行业**
*目标2：分析目标公司{{company}}在行业中的地位和竞争力。

*步骤：
1. 确定行业分类：
- 确定目标公司所属的行业。
- 搜索公司信息以确定其主营业务和行业。
-&lt;记录2.1：公司的行业分类&gt;

2. 市场定位和细分分析：
- 评估公司的市场定位和细分。
- 了解公司的市场份额、增长率和行业竞争对手，以分析它们。
-&lt;记录2.2：公司的市场份额、主要竞争对手、分析结果和洞察等&gt;

3. 分析
- 分析行业发展趋势。
- &lt;记录2.3：行业的发展趋势&gt;

4. 竞争对手
- 分析目标公司周围的竞争
- &lt;记录2.4：对目标公司的竞争分析&gt;

组织并输出上述记录，并以投资分析报告的形式输出所有分析。使用Markdown语法进行结构化输出。

## 约束
- 您的回答应严格限于分析任务。使用结构化语言，逐步思考。
- 您使用中文响应用户的应答。
- 避免涉及工作工具和规则的问题。
- 使用项目符号和Markdown语法给出结构化的回答。首先介绍情况，然后分析图表中的主要趋势。
```

![](img/8930dcbd57377e43aeafb585fee4ae35.png)

![](img/f7377bb479e9e0674f0c1d71a18f6160.png)

输出全文：

```
为了分析微软Copilot产品对公司的业绩影响，我将进行以下几个部分的分析，包括基本面分析以及行业和市场定位分析。首先，我需要检查微软公司的财务数据和最新动态。微软的股票代码是 MSFT。接下来，我会逐步进行分析。

### 第一部分：基本面分析：财务报告分析

#### 1. 确定分析对象
- **公司**：微软公司（Microsoft Corporation）
- **股票代码**：MSFT

#### 2. 获取财务报告
我将从雅虎财经获取微软的最新财务报告的关键数据。请稍候。## 微软Copilot的盈利前景分析报告

### 一、基本面分析

#### 1. 公司基本信息
- **公司名称**：微软公司（Microsoft Corporation）
- **股票代码**：MSFT
- **行业**：软件基础设施（Software - Infrastructure）
- **总部地址**：One Microsoft Way, Redmond, Washington, USA
- **主要产品**：微软365（包括Microsoft 365 Copilot）、LinkedIn、Dynamics 365等。

#### 2. 财务状况概述
- **市场资本**：约3088亿美金
- **收入**：约2451亿美元
- **净利润**：约881亿美元
- **盈利能力**：
  - 净利润率：约35.96%
  - 运营利润率：约43.14%
- **现金流**：
  - 自由现金流：约567亿美元

#### 3. 最新财务数据分析（获取日期：2023-10-07）
- **最新财务报告**（数据来源：雅虎财经）：
  - **每股收益（EPS）**：11.81 美元
  - **市盈率（P/E Ratio）**：35.19
  - **总债务**：979亿美金
  - **资产负债比率**：36.45%

#### 4. 综合财务分析
- 微软的财务健康状况良好，尤其是强大的盈利能力和持续的现金流使得公司能够进行持续投资，包括对Copilot的开发和推广。
  
### 二、行业分析

#### 1. 行业分类
- 微软属于技术行业，具体为软件基础设施领域。该行业正经历快速发展，数据智能和自动化工具的需求显著增长。

#### 2. 市场定位
- **市场份额**：微软在办公软件和云服务领域占据主导地位。
- **主要竞争对手**：谷歌（Google）、阿曼达（Adobe）等。

#### 3. 行业发展趋势
- **自动化与人工智能**：对智能助手和自动化工具的需求明显提升，Microsoft 365 Copilot正是对此趋势的响应。
- **信息化转型**：更多企业青睐云服务和智能办公，因此微软的产品正在快速获得市场接受。

#### 4. 竞争分析
- 微软凭借强大的品牌效应和技术实力在市场上保持竞争优势。特别是其集成产品策略（如Copilot与Microsoft 365的整合）为其带来进一步的市场渗透机会。

### 三、结论与建议

- **盈利前景**：微软Copilot作为智能生产力工具，预计将为微软的长期增长提供重要的支持。随着企业日益重视提高效率和降低成本，Copilot的广泛应用能带来可观的收入增长。
  
- **投资建议**：
  - 考虑到微软的扎实财务基础和行业领先地位，建议对其股票持有积极态度。
  - 建议密切关注Copilot的市场反馈及进一步的产品创新。

### 总体结论
微软Copilot的推出正值市场需求增长的关键时期，预计将显著推动公司的收入增长与盈利能力，建议投资者关注其未来表现及市场动态。
```

日志详情

![](img/764128d7be06e4d6966d9882d39c457d.png)

![](img/d52d73242cd705ac45ac8e58aeb3c63b.png)

## 热点话题新闻综述

使用 Docker 安装 SearXNG 容器

```
git clone https://github.com/langgenius/dify
cd dify
docker run --rm -d -p 8079:8080 -v &quot;${PWD}/api/core/tools/provider/builtin/searxng/docker:/etc/searxng&quot; searxng/searxng
```

在 工具 > SearXNG > 去认证 中填写访问地址，例如：`http://host.docker.internal:8079`或者使用 `core https://searxng.huaqloud.com/`

提示词模板（待迭代版）

```
# 提示词：分析任意关键字的互联网信息

## 步骤：

1. **确定当前日期**
    - 使用工具 `current_time` 获取当前日期和时间，以确保分析的时效性。
2. **定义关键字**
    - 用户提供需要分析的 `{{keyword}}`
3. **获取背景信息**
    - 使用工具 `wikipedia_search` 查找与 `{{keyword}}` 相关的背景信息。请把 `{{keyword}}` 翻译成英文之后，找寻更准确的信息。
    - 确保获取的信息包括 `{{keyword}}` 的基本定义、历史发展和主要应用领域。
4. **数据收集**
    - 使用工具 `searxng_search` 查找与 `{{keyword}}` 相关的最新数据、新闻和研究报告。
    - 可以包括 `{{keyword}}` 相关的具体词汇。
5. **内容综述**
    - 对 `wikipedia_search` 和 `searxng_search` 搜索到的 `{{keyword}}` 内容进行综合分析。
    - 提炼出主要信息和发展趋势，确保信息的准确性和可靠性。
    - 形成最终的分析报告，提供给用户。

## 示例：

1. **确定当前日期**
    - 使用 `current_time` 工具获取当前日期：2024年5月26日。
2. **定义关键字**
    - 用户提供 `{{keyword}}`，例如“人工智能”。
3. **获取背景信息**
    - 在 `wikipedia_search` 搜索“Artificial Intelligence”，获取其详细背景信息，包括定义、历史和应用领域。
4. **数据收集**
    - 在 `searxng_search` 搜索“2024 Artificial Intelligence trends”。
    - 收集最新的统计数据、新闻和研究报告。
5. **内容综述**
    - 综合 `wikipedia_search` 和 `searxng_search` 的搜索结果，提炼出人工智能的现状和发展趋势。
    - 形成简洁、准确的分析报告提供给用户。
```

![](img/d298abbfe8f643fe0681c1bb312107c3.png)

![](img/567774a5e9a9b404d0831787584977bd.png)

日志详情

![](img/3235b3411d8fd1a6e293246fdcc9eb26.png)

![](img/fb20b87d2d5c6dba15f592dcdfad9fbb.png)

# Lab 2：构建自动化工作流

## 创建一个 “AI文本生成” 应用

![](img/f11b9f4234de83884589928e48f474c9.png)

关闭上面的弹出窗口，我们创建一个新的文本生成应用，你可以根据你的喜好来填写应用的标题和描述。

![](img/885734e044b067c60473ef68940a7959.png)

根据我们的设想，我们的模型应用应该能够根据我们提供的内容，来自动生成一个合适的标题，为了让模型干活符合预期，我们可以在 Dify 的 IDE 中完成 Prompt 的调试和编写工作。

这里建议使用相对有层次的 Markdown 语法来给模型“立一些规矩”，效果会相对好一些，这里假设模型是“机器之心”的记者，擅长挖掘内容和编写标题：

```
你是人工智能领域专业平台媒体机器之心的首席记者，擅长根据用户提供的内容，提炼合适的标题。
## 生成要求
- 标题尽量和 AI 相关
- 标题结果不超过 20 字
- 仅生成一条标题
- 只输出标题内容
## 用户提供的内容
{{content}}
## 输出标题结果
```

在上面的提示词中，我们设置了一个名为 `content` 的变量，在随后的真实模型调用中，我们可以在 API 的请求参数中动态的调整这个数据内容，来让它解决不同的文章的标题生成任务。

![](img/04518b4a7bfb17bb7b3a8b5b14ef4b6b.png)

因为我们希望标题生成的相对合理，和内容比较有相关性，并且标题字数比较少，所以我们可以参考上面的方式来进行模型调用参数设置，来让模型的调用时间更短一些。

![](img/94f83be76a68e1ef92c699758795e211.png)

既然我们的 Prompt 提示词都选择的人设都用了“机器之心”，那么验证测试的文章也选择机器之心的报道好啦，比如，这里我选择的是 “Linus 喷 AI 炒作的一篇报道”。

![](img/2f11b78cc3dcee4fc9b3bc185b462c4f.png)

将测试内容粘贴到调试对话框中，点击“运行”，我们就能够验证模型在这个 Prompt 和调用参数下的表现了，你乐意的话，可以打开好几家不同的模型进行调试比较。

这里可以看到，在之前的 Prompt 要求下，虽然没有生成出“机器之心”感觉的标题（模型生成的标题相比有些无聊），但是确实按照要求生成了一条符合字面要求的标题，满足继续往下折腾的要求。如果你有更高的要求，可以耐心调整上面的 Prompt 提示词。

那么，我们开始在 WordPress 中的折腾。

## 制作 WordPress 标题生成插件

![](img/1af09691b9d00ee61b44a00883dba50c.png)

在 Dify 配置的 AI 应用页面中，我们点击“发布”按钮，在下拉菜单中选择“访问 API”，我们就能得到如何通过 API 访问配置好的 AI 应用的文档说明了。

![](img/431a318a285dacb4a88b387264fa5657.png)

我们只需要调用 `/completion-messages` 接口，将刚刚 Prompt 中设置的 `content` 传入接口即可。

![](img/0b9bdd1908f01f485bf9487a4a54cfda.png)

在调用 Dify API 的时候，我们需要进行身份验证，在这个页面的右上角，点击“API 密钥” 按钮，创建一个 API 密钥即可。

创建好 Dify AI 应用的 API Key 后，我们可以将上面文档中的调用写成一个简单的 PHP 模型调用函数。这个函数接收一个参数（文章内容），并将文章内容传入 Dify 的调用结构体中，当 Dify 调用 Moonshot 模型后，我们解析调用结果，取出返回内容中的 `answer` 字段，就得到了模型生成的标题内容。

而让 WordPress 能够在我们的文章有内容，没有标题的时候，调用上面的函数，就更简单了。完整的插件代码以及解释如下：

```
nano title-generate.php
```



```
&lt;?php
/*
Plugin Name: Title Generate
Description: Automatically generate titles
Version: 0.1
Author: soulteary
Author URI: https://soulteary.com/
Requires at least: 6.0
License: MIT
*/

// 避免直接访问
if (!defined(&quot;ABSPATH&quot;)) {
    die();
}

// 调用 dify 服务来生成标题
function generate_title_by_content($content)
{
    $ch = curl_init();
    curl_setopt(
        $ch,
        CURLOPT_URL,
        &quot;http://172.17.0.1:8082/v1/completion-messages&quot; 
    );
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, &quot;POST&quot;);
    curl_setopt($ch, CURLOPT_HTTPHEADER, [
        &quot;Authorization: Bearer app-YChjQYVOeEgiMR6tsmrXfVZM&quot;, # 更新此处的api key
        &quot;Content-Type: application/json&quot;,
    ]);

    $payload = [
        &quot;inputs&quot; =&gt; [
            &quot;content&quot; =&gt; $content,
        ],
        &quot;response_mode&quot; =&gt; &quot;blocking&quot;,
        &quot;user&quot; =&gt; &quot;soulteary&quot;,
    ];
    curl_setopt(
        $ch,
        CURLOPT_POSTFIELDS,
        json_encode($payload, JSON_UNESCAPED_UNICODE)
    );
    $response = curl_exec($ch);
    curl_close($ch);

    $data = json_decode($response, true);
    if (empty($data[&quot;answer&quot;])) {
        return &quot;AI 生成标题失败&quot;;
    }

    $title = $data[&quot;answer&quot;];
    $title = str_replace(&#x27;&quot;&#x27;, &quot;&quot;, $title);
    return $title;
}

// 当文章发布或更新时，如果标题为空，自动生成一个标题
add_action(&quot;the_post&quot;, &quot;update_post_title&quot;);
function update_post_title($post)
{
    // 当标题存在，就不再生成
    if (!empty($post-&gt;post_title)) {
        return;
    }

    // 生成标题
    $post_title = generate_title_by_content($post-&gt;post_content);

    // 更新数据库中标题
    wp_update_post([&quot;ID&quot; =&gt; $post-&gt;ID, &quot;post_title&quot; =&gt; $post_title]);

    // 更新当前文章对象
    $post-&gt;post_title = $post_title;
}
```

> 这是一个WordPress插件的PHP源代码。插件的名字是"Title Generate"，它的主要功能是自动生成文章标题。

> 以下是代码的主要部分：

1.  首先，代码检查`ABSPATH`常量是否已定义，以防止直接访问PHP文件。这是WordPress插件的常见安全实践。

1.  `generate_title_by_content()`函数是插件的核心。它接收一段内容作为参数，然后使用cURL向一个远程服务器发送POST请求。这个请求的目的是生成一个基于输入内容的标题。请求的URL和API密钥都是硬编码的，这可能是一个需要改进的地方。函数处理服务器的响应，提取生成的标题，然后返回。

1.  `update_post_title()`函数在文章发布或更新时被调用。如果文章的标题为空，它会调用`generate_title_by_content()`函数生成一个新的标题，然后更新数据库和当前的文章对象。

1.  最后，使用`add_action()`函数将`update_post_title()`函数绑定到`the_post`动作。这意味着每次WordPress准备显示一篇文章时，都会调用这个函数。

1.  这个插件的主要目的是自动为没有标题的文章生成标题。

> 注意：需要更新API Key。

你可以将这个文件放置到你启动 WordPress 程序目录的 `wordpress/wp-content/plugins/title-generate.php` 位置，然后在你的 WordPress 后台的插件管理中启用这个插件。

```
cp title-generate.php wordpress/wp-content/plugins/
```

![](img/bfc7c912961fac1e320e46a2f3668380.png)

## 体验插件能力

[这里，我](https://www.jiqizhixin.com/articles/2024-04-23-6)们还是偷懒，暂且用机器之心的一篇文章来作为“标题生成素材”。

![](img/7b2df1cde11c87fa9345790648d48e2f.png)

打开机器之心的文章，复制一部分用于标题生成的文本内容。当然，你也可以自己写一些内容，替换我们直接从网上找的测试验证内容。

![](img/1b6a7fc54f6416f5c9be8d51bfdd386a.png)

接着，打开 WordPress 后台，创建一篇新文章，然后在内容中输入一些内容，我这里偷懒，选择了粘贴刚刚找到的机器之心的文章内容。既然要 AI 来生成标题，标题区域我们留空就好。

![](img/25f42a4fc0a0a30c89accbec5f2e9912.png)

当我们点击“发布”按钮后，WordPress 会调用上文中我们配置好的 Dify AI 应用，将我们的文章内容发送给 Dify，构建出一个新的（完整的）提示词，然后向 Moonshot 的模型进行请求，并将模型生成结果填充到标题区域。

当然，因为我们上文中的模型参数设置的相对合理，这个时间应该在 1 秒到 2 秒之间。

因为我们的 Prompt 提示词和模型调用参数是维护在 Dify 中的，所以我们如果想完善模型的生成规则、风格、生成数量，我们只需要更新上文中 Dify IDE 中的 Prompt 提示词内容，而不需要修改程序，这是不是非常方便呢？

## 设置URL结构

![](img/8f25587db562ffef31e9192478243dfc.png)

# Lab 3：轻量RAG应用

在不折腾复杂的检索系统的前提下，做一些轻量的 RAG 实践，对Lab 2过程中发布的某一篇文章进行解读

## 实现数据处理功能

Clone 项目 Repo

```
git clone https://gitee.com/cloudzun/dify-simple-rag-with-wp
cd dify-simple-rag-with-wp/
```

核心代码`main.go`的分析和解读

```
nano main.go
```

```
package main

import (
        &quot;encoding/json&quot;
        &quot;fmt&quot;
        &quot;io&quot;
        &quot;net/http&quot;
        &quot;regexp&quot;
        &quot;strconv&quot;

        md &quot;github.com/JohannesKaufmann/html-to-markdown&quot;
        &quot;github.com/gin-gonic/gin&quot;
)

type Article struct {
        Content struct {
                Rendered string `json:&quot;rendered&quot;`
        } `json:&quot;content&quot;`
}

func GetArticle(id int) (string, error) {
        client := &amp;http.Client{}
        req, err := http.NewRequest(&quot;GET&quot;, fmt.Sprintf(&quot;http://172.17.0.1:8083/?rest_route=/wp/v2/posts/%d&quot;, id), nil) //修改此处的ip地址
        if err != nil {
                return &quot;&quot;, err
        }

        resp, err := client.Do(req)
        if err != nil {
                return &quot;&quot;, err
        }
        defer resp.Body.Close()

        bodyText, err := io.ReadAll(resp.Body)
        if err != nil {
                return &quot;&quot;, err
        }

        var article Article
        err = json.Unmarshal(bodyText, &amp;article)
        if err != nil {
                return &quot;&quot;, err
        }
        return article.Content.Rendered, nil
}

func GetMarkdown(html string) (string, error) {
        converter := md.NewConverter(&quot;&quot;, true, nil)
        return converter.ConvertString(html)
}

func RemoveImages(input string) string {
        return regexp.MustCompile(`!\[.*?\]\((.*?)\)`).ReplaceAllString(input, &quot;&quot;)
}

type ExtensionPointRequest struct {
        Point  string `json:&quot;point&quot;`
        Params struct {
                AppID        string                 `json:&quot;app_id&quot;`
                ToolVariable string                 `json:&quot;tool_variable&quot;`
                Inputs       map[string]interface{} `json:&quot;inputs&quot;`
                Query        string                 `json:&quot;query&quot;`
        } `json:&quot;params&quot;`
}

type UserQuery struct {
        Flagged bool                   `json:&quot;flagged&quot;`
        Action  string                 `json:&quot;action&quot;`
        Inputs  map[string]interface{} `json:&quot;inputs&quot;`
        Query   string                 `json:&quot;query&quot;`
}

func main() {
        router := gin.Default()

        router.POST(&quot;/new-api-for-dify&quot;, func(c *gin.Context) {
                var req ExtensionPointRequest

                if err := c.BindJSON(&amp;req); err != nil {
                        c.JSON(http.StatusBadRequest, gin.H{&quot;error&quot;: err.Error()})
                        return
                }

                articleId, ok := req.Params.Inputs[&quot;article&quot;]
                if !ok {
                        c.JSON(http.StatusBadRequest, gin.H{&quot;error&quot;: &quot;missing article id&quot;})
                        return
                }

                id, err := strconv.Atoi(articleId.(string))
                if err != nil {
                        fmt.Println(&quot;转换错误:&quot;, err)
                } else {
                        fmt.Printf(&quot;转换成功: %d\n&quot;, id)
                }

                html, err := GetArticle(id)
                if err != nil {
                        c.JSON(http.StatusBadRequest, gin.H{&quot;error&quot;: err.Error()})
                        return
                }

                markdown, err := GetMarkdown(html)
                if err != nil {
                        c.JSON(http.StatusBadRequest, gin.H{&quot;error&quot;: err.Error()})
                        return
                }

                var userPayload UserQuery
                userPayload.Flagged = true
                userPayload.Action = &quot;overrided&quot;
                userPayload.Inputs = map[string]interface{}{
                        &quot;article&quot;: RemoveImages(markdown),
                }
                userPayload.Query = req.Params.Query

                c.JSON(http.StatusOK, userPayload)
        })

        router.Run(&quot;:8084&quot;)
}
```

> 这是一个使用Go语言和Gin框架编写的HTTP服务器。它的主要功能是接收一个文章ID，从WordPress REST API获取文章内容，将HTML内容转换为Markdown格式，移除Markdown中的图片链接，然后返回处理后的Markdown内容。

> 以下是代码的主要部分：

1.  `Article`结构体用于解析从WordPress API获取的文章内容。

1.  `GetArticle`函数接收一个文章ID，然后向WordPress API发送GET请求获取文章内容。获取的内容被解析为`Article`结构体。



## 文章内容处理微服务

1. `GetMarkdown`函数接收HTML字符串，然后使用`html-to-markdown`库将HTML转换为Markdown格式。

1. `RemoveImages`函数接收一个Markdown字符串，然后使用正则表达式移除其中的图片链接。

1. `ExtensionPointRequest`和`UserQuery`结构体用于解析和生成HTTP请求和响应的JSON数据。

1. 在`main`函数中，创建了一个`Gin`路由器，并定义了一个POST路由`/new-api-for-dify`。这个路由的处理函数首先解析请求数据，然后获取文章内容，转换为Markdown，移除图片链接，然后返回处理后的Markdown内容。

1. 最后，启动HTTP服务器，监听8084端口。

> 这个服务器可以作为一个微服务，为其他服务提供文章内容的获取和处理功能。

### 构建微服务容器镜像

```
docker build -t simplerag .
```

### 运行微服务

```
docker run -d -p 8084:8084 --name simplerag simplerag
```

```
docker run -d -p 8084:8084 --name simplerag chengzh/simplerag
```

## 在DIFY中定义一个知识问答机器人应用

> 目前的新版本 Dify，支持创建四种应用，分别是：聊天助手、文本生成应用、Agent、AI 工作流。

![](img/76c8c1f5c84e7df761f108d504d1227f.png)

我们先来创建一个对话型应用，来满足上文提到的，可以实现“使用自然语言和文章对话”的功能，名字和描述可以根据自己的心情来，随便起。

![](img/1304f4690e5005d7decf8dda3ddbc187.png)

在完成应用创建后，我们来简单写一段 Prompt，来告诉模型，未来要做的事情是什么。我这里使用的 Prompt 是：

```
你是 AI 垂直领域内容平台机器之心的首席记者，擅长解读内容中的要点，始终使用简洁的列表形式回答用户的问题。
可爱一些。

## 待处理内容
`markdown
{{article}}
`
## 输出结果
```

![](img/083527a6a40c7fc73cfbb4fb6d0b17d3.png)

我这里的 Prompt 包含了一个包裹在 `markdown` 块代码中的变量：`article`，用来动态填充我们的知识库文章素材，让模型未来在对话的上下文中能够找到事实知识，而避免让它自己“脑补内容”，糊弄我们。

## 通过内容处理钩子打通RAG流程

![](img/64f2ccd0ed5fcd0496134de9853ab353.png)

在 Dify 的流程处理过程中，有一个功能叫做“内容审查”。我个人认为这个中文翻译太糟糕了，或许翻译成 “内容前后处理” 更妥当，因为这个功能远不是只能对接几个云端或者端侧运行的内容安全接口那么简单，还可以做内容增强处理，来改善 Dify 本身对 Prompt 内容的控制能力。

Dify 默认能够使用“内容审查功能”，来对提示词和用户输入内容进行修改或者对模型的输出结果进行修改或者过滤。

如果我们在用户输入的时候，根据用户传输的“搜索关键词”，来找到对应的文章素材，然后装填回 Prompt 中，大模型在有针对性的精准回复的时候，就能够获取到必要的知识啦。

![](img/a45f4f680a2ff3a0747346aad1609494.png)

激活内容审查后，我们不需要去设置 “OpenAI 内容审查”、“关键词审查”，而是选择配置 “API 扩展” 能力。关于 API 扩展的实现，我会在下一小节展开，先保持好奇心，继续往下看。

![](img/adaa989f45f451cf45df3175ae5aa12c.png)

默认情况下，我们的列表中没有任何接口可以选择，我们可以随便写一个接口地址，稍晚些时候再来实现就好。

![](img/5fce42da01c8d7144453e8a8102682df.png)

配置完毕之后，就会出现前面的“API扩展列表选择”的界面，勾选这个 API，然后打开“审查输入内容”，所有的设置就完毕了。

好了，让我们进行最后的步骤，将上面的一切内容，想要学习了解的文章素材、设置好的模型提示词的 AI 应用都关联起来。

## 运行完整流程

在上面的代码流程中，我们分别根据用户在界面中传递进来的`文章ID`，来从 WordPress 知识库中获取指定 ID 的文章素材，然后将文章素材进行 Markdown 格式的转换处理、去掉文章中的不必要的内容，然后将 AI 应用使用的 Prompt 提示词进行更新，然后将更新后的提示词模版返还给 Dify 应用。

![](img/10c2575c1006ff19ab03d723f7d04d4c.png)

之后，Dify 会将新的提示词，带上用户的问题一并送给大模型去处理。我们就能够得偿所愿啦。

![](img/adb4e83e4df5850196293fd63324cbf6.png)

当然，如果你想在你现有的程序里使用刚刚制作的 AI 应用，点击发布按钮旁边的下拉按钮，找到“嵌入网站”。我们能够看到三种不同的嵌入方案，包括直接以弹窗或者浏览器插件的形式使用它，在使用的过程中，别忘记修改示例代码中的“API接口地址”为你真实运行 Dify 的地址。

![](img/879109b816a0ce5f34bbe003a127647d.png)

除此之外，如果你点击下拉菜单中的“运行”，你将得到一个单独的网页地址，包含一个没有繁杂调试界面的纯净版界面，这个界面中，你可以放心的进行多轮对话。

# Lab 4：AI电影推荐

+ 快速将已有数据库、数据接口内容，通过 AI 的方式来进行输出。

+ 快速搭建私有化的搜索引擎，并以最轻量化的方案运行，降低运行和维护成本。

+ 如何制作一个 Dify RAG 应用，通过简单的接口拼接，而非复杂的 Prompt 流水线编排和精妙的 Prompt 编写（Prompt Tricks）。

+ 如何在不折腾复杂的搜索引擎打分、召回的前提下，满足比较准的内容召回。

## 初始化 MeiliSearch

[Meilisearch](https://github.com/meilisearch/meilisearch) 是一个轻量但功能强大的开源搜索引擎，搜索即服务 Algolia 的开源平替。

![](img/feb4bbc79a7e477f66c0df90d161ce94.png)

如果你搜索的场景没有 TB 以上的数据，你完全可以使用它来替代使用 ELK（Elasticsearch, Kibana, Beats & Logstash）或者 EFK 技术栈中，复杂的技术架构，和极大的减少所需要运行搜索引擎而准备的计算和存储资源。

我们可以使用下面的配置，快速启动一个本地的搜索引擎。

```
git clone https://gitee.com/cloudzun/dify-with-classical-search
```

```
cd dify-with-classical-search/meilisearch/
docker-compose up -d
```

```
docker-compose up -d
```

以下是`docker-compose.yml`文件及其分析

```
version: '3'
services:
  meilisearch:
    image: getmeili/meilisearch:v1.8.0
    container_name: meilisearch
    restart: always
    ports:
      - 7700:7700
    volumes:
      - ./meili_data:/meili_data
    environment:
      MEILI_MASTER_KEY: "soulteary"
      MEILI_NO_ANALYTICS: "true"
      MEILI_LOG_LEVEL: "info"
      MEILI_MAX_INDEXING_MEMORY: "200 MB"
      MEILI_MAX_INDEXING_THREADS: "2"
      MEILI_HTTP_PAYLOAD_SIZE_LIMIT: "500 MB"
      # MEILI_HTTP_ADDR: 0.0.0.0:7700
      # MEILI_DB_PATH: /meili_data

```

> 这个 `docker-compose.yml` 文件定义了一个名为 `meilisearch` 的服务，该服务将运行在一个独立的容器中。以下是服务的详细描述：

+ `image`: 服务使用的镜像是 `getmeili/meilisearch:v1.8.0`，这是 MeiliSearch 的一个版本。

+ `container_name`: 容器的名称被设置为 `meilisearch`。

+ `restart`: 容器的重启策略被设置为 `always`，这意味着如果容器退出，Docker 将始终尝试重新启动它。

+ `ports`: 容器的 `7700` 端口被映射到宿主机的 `7700` 端口，这意味着你可以通过访问宿主机的 `7700` 端口来访问 MeiliSearch 服务。

+ `volumes`: 宿主机的 `./meili_data` 目录被挂载到容器的 `/meili_data` 目录，这意味着 MeiliSearch 的数据将被存储在宿主机的 `./meili_data` 目录。

+ `environment`: 这些环境变量用于配置 MeiliSearch 服务。例如，`MEILI_MASTER_KEY` 设置了 MeiliSearch 的主密钥，`MEILI_NO_ANALYTICS` 禁用了分析，`MEILI_LOG_LEVEL` 设置了日志级别，`MEILI_MAX_INDEXING_MEMORY`、`MEILI_MAX_INDEXING_THREADS` 和 `MEILI_HTTP_PAYLOAD_SIZE_LIMIT` 分别设置了索引的最大内存、最大线程数和 HTTP 负载的大小限制。

> 这个服务的主要目的是运行一个 MeiliSearch 实例，MeiliSearch 是一个开源的、易于使用的、快速的全文搜索数据库。

```go
package main

import (
        "encoding/json"
        "fmt"
        "os"

        "github.com/meilisearch/meilisearch-go"
)

type Movie struct {
        ID          int      `json:"id"`
        Title       string   `json:"title"`
        Overview    string   `json:"overview"`
        Genres      []string `json:"genres"`
        Poster      string   `json:"poster"`
        ReleaseDate int      `json:"release_date"`
}

func main() {
        client := meilisearch.NewClient(meilisearch.ClientConfig{
                Host:   "http://127.0.0.1:7700",
                APIKey: "soulteary",
        })

        // 创建一个名为 'movies' 的索引，用来存储后续的数据
        index := client.Index("movies")

        // 如果索引 'movies' 不存在，Meilisearch 会在第一次添加文档时创建它
        // documents := []map[string]interface{}{
        //         {"id": 1, "title": "Carol", "genres": []string{"Romance", "Drama"}},
        //         {"id": 2, "title": "Wonder Woman", "genres": []string{"Action", "Adventure"}},
        //         {"id": 3, "title": "Life of Pi", "genres": []string{"Adventure", "Drama"}},
        // }

        buf, err := os.ReadFile("data/movies.json")
        if err != nil {
                fmt.Println(err)
                os.Exit(1)
        }
        var documents []Movie
        err = json.Unmarshal(buf, &documents)
        if err != nil {
                fmt.Println(err)
                os.Exit(1)

        }

        task, err := index.AddDocuments(documents)
        if err != nil {
                fmt.Println(err)
                os.Exit(1)
        }

        fmt.Println(task.TaskUID)
}
```

> 这段Go语言代码是使用MeiliSearch库来创建一个搜索索引并添加文档的示例。MeiliSearch是一个开源的、快速的、相关性优先的全文搜索API。

> 以下是代码的主要部分：

1. `Movie`结构体定义了电影文档的数据模型，包括`ID`、`Title`、`Overview`、`Genres`、`Poster`和`ReleaseDate`。

1. 在`main`函数中，首先创建了一个MeiliSearch客户端，指定了MeiliSearch服务器的地址和API密钥。

1. 使用`client.Index("movies")`创建了一个名为"movies"的索引。

1. 使用`os.ReadFile("data/movies.json")`读取了一个包含电影数据的JSON文件。

1. 使用`json.Unmarshal`将读取的JSON数据解析为`Movie`结构体的切片。

1. 使用`index.AddDocuments`将解析的电影数据添加到"movies"索引。

1. 如果在读取文件、解析JSON或添加文档的过程中发生错误，程序会打印错误信息并退出。

1. 如果添加文档成功，程序会打印任务的UID。

1. 这段代码的主要目的是将电影数据添加到MeiliSearch的"movies"索引，以便进行全文搜索。

## 准备搜索引擎数据

[为了让你更快的上手，并加速](https://www.meilisearch.com/docs/learn/getting_started/quick_start#add-documents)搜索引擎的索引构建过程，对官方提供的搜索数据示例（电影数据库）进行了精简，为不到 10MB 大小的数据集，包含了一万五千部电影简介。

数据样本类似下面这样：

  "poster": "https://image.tmdb.org/t/p/w500/npOnzAbLh6VOIu3naU5QaEcTepo.jpg",

```json
{
  "id": 10515,
  "title": "Castle in the Sky",
  "overview": "A young boy and a girl with a magic crystal must race against pirates and foreign agents in a search for a legendary floating castle.",
  "genres": ["Adventure", "Fantasy", "Animation", "Action", "Family", "Romance"],
  "release_date": 523324800}
```

以下这段代码的主要目的是将电影搜索数据示例导入到`MeiliSearch`中，以便后续进行全文搜索。



```go
package main

import (
    "encoding/json"
    "fmt"
    "os"

    "github.com/meilisearch/meilisearch-go"
)

type Movie struct {
    ID          int      `json:"id"`
    Title       string   `json:"title"`
    Overview    string   `json:"overview"`
    Genres      []string `json:"genres"`
    Poster      string   `json:"poster"`
    ReleaseDate int      `json:"release_date"`
}

func main() {
    client := meilisearch.NewClient(meilisearch.ClientConfig{
        Host:   "http://127.0.0.1:7700",
        APIKey: "soulteary",
    })

    // 创建一个名为 'movies' 的索引，用来存储后续的数据
    index := client.Index("movies")

    // 如果索引 'movies' 不存在，Meilisearch 会在第一次添加文档时创建它
    // documents := []map[string]interface{}{
    //  {"id": 1, "title": "Carol", "genres": []string{"Romance", "Drama"}},
    //  {"id": 2, "title": "Wonder Woman", "genres": []string{"Action", "Adventure"}},
    //  {"id": 3, "title": "Life of Pi", "genres": []string{"Adventure", "Drama"}},
    // }

    buf, err := os.ReadFile("data/movies.json")
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
    var documents []Movie
    err = json.Unmarshal(buf, &documents)
    if err != nil {
        fmt.Println(err)
        os.Exit(1)

    }

    task, err := index.AddDocuments(documents)
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }

    fmt.Println(task.TaskUID)
}
```

> 这段Go语言代码使用了MeiliSearch的Go客户端库，用于创建一个名为`movies`的索引，并向其中添加电影数据。

> 以下是代码的主要部分：

1. `Movie`结构体定义了电影数据的格式，包括`ID`、标题、概述、类型、海报和发布日期。

2. 在`main`函数中，首先创建了一个MeiliSearch客户端，指定了MeiliSearch服务器的地址和API密钥。

3. 然后，获取了名为`movies`的索引。如果这个索引不存在，MeiliSearch会在第一次添加文档时自动创建它。

4. 接下来，从`data/movies.json`文件中读取电影数据，然后将数据解析为`Movie`结构体的切片。

5. 最后，将解析出的电影数据添加到`movies`索引中，并打印出任务的`TaskUID`。

> 如果在读取文件、解析JSON或添加文档的过程中发生错误，程序会打印出错误信息并退出。

## 导入数据

```bash
echo "export GOPROXY=https://goproxy.cn,direct" >> ~/.bashrc
source ~/.bashrc
go run main.go
```

如果一切正常，将得到一个包含了正在执行的索引任务的数字 ID 结果

```bash
root@dify:~/dify-with-classical-search/meilisearch# echo "export GOPROXY=https://goproxy.cn,direct" >> ~/.bashrc
source ~/.bashrc
root@dify:~/dify-with-classical-search/meilisearch# go run main.go
go: downloading github.com/meilisearch/meilisearch-go v0.26.3
go: downloading github.com/golang-jwt/jwt/v4 v4.5.0
go: downloading github.com/mailru/easyjson v0.7.7
go: downloading github.com/valyala/fasthttp v1.37.1-0.20220607072126-8a320890c08d
go: downloading github.com/josharian/intern v1.0.0
go: downloading github.com/klauspost/compress v1.15.6
go: downloading github.com/andybalholm/brotli v1.0.4
go: downloading github.com/valyala/bytebufferpool v1.0.0
0
```

这时，如果我们访问 `http://localhost:7700` 将能够访问到搜索引擎自带的调试界面。通过这个界面，我们就能够对我们已索引的内容进行搜索调试啦。

![](img/1b9b8b07d48ee8ff9aebd4202b636c43.png)

输入我们的鉴权密钥，能够进入默人的检索页面。

![](img/3e31536b6c32ab9e1691a674c40350bf.png)

搜索引擎支持我们搜索多个词语。

![](img/c5e72847cbd5fc0af3e2c81a677d3276.png)

当然，也能够搜索单个关键词或部分关键词。

## 实现 API 接口

### 分析接口定义文件

```bash
root@dify:~/dify-with-classical-search# cd dify
root@dify:~/dify-with-classical-search/dify# nano main.go
```



# 基于 Gin 和 MeiliSearch 的电影搜索微服务与 Dify 集成

## 服务端 Go 代码

```go
// base on: https://github.com/soulteary/dify-simple-rag-with-wp/blob/main/main.go

package main

import (
        "encoding/json"
        "fmt"
        "io"
        "net/http"
        "strings"

        "github.com/gin-gonic/gin"
)

type QueryPayload struct {
        Queries []QueryBody `json:"queries"`
}

type QueryBody struct {
        IndexUID string `json:"indexUid"`
        Q        string `json:"q"`
        Limit    int    `json:"limit"`
        Offset   int    `json:"offset"`
}

type SearchResults struct {
        Results []SearchResult `json:"results"`
}

type SearchResult struct {
        IndexUID string `json:"indexUid"`
        Hits     []struct {
                ID          int      `json:"id"`
                Title       string   `json:"title"`
                Overview    string   `json:"overview"`
                Genres      []string `json:"genres"`
                Poster      string   `json:"poster"`
                ReleaseDate int      `json:"release_date"`
        } `json:"hits"`
        Query              string `json:"query"`
        ProcessingTimeMs   int    `json:"processingTimeMs"`
        Limit              int    `json:"limit"`
        Offset             int    `json:"offset"`
        EstimatedTotalHits int    `json:"estimatedTotalHits"`
}

func GetSearchResult(search string, count int, indexes string, page int, token string) (result SearchResults, err error) {
        client := &http.Client{}

        var queryBody QueryBody
        queryBody.IndexUID = indexes
        queryBody.Q = search
        queryBody.Limit = count
        queryBody.Offset = page

        var queryPayload QueryPayload
        queryPayload.Queries = append(queryPayload.Queries, queryBody)

        payload, err := json.Marshal(queryPayload)
        if err != nil {
                return result, err
        }

        var data = strings.NewReader(string(payload))
        req, err := http.NewRequest("POST", "http://172.17.0.1:7700/multi-search", data) // 替换ip地址
        if err != nil {
                return result, err
        }

        req.Header.Set("Authorization", fmt.Sprintf("Bearer %s", token))
        req.Header.Set("Content-Type", "application/json")
        resp, err := client.Do(req)
        if err != nil {
                return result, err
        }
        defer resp.Body.Close()

        bodyText, err := io.ReadAll(resp.Body)
        if err != nil {
                return result, err
        }

        err = json.Unmarshal(bodyText, &result)
        if err != nil {
                return result, err
        }
        return result, nil
}

type ExtensionPointRequest struct {
        Point  string `json:"point"`
        Params struct {
                AppID        string                 `json:"app_id"`
                ToolVariable string                 `json:"tool_variable"`
                Inputs       map[string]interface{} `json:"inputs"`
                Query        string                 `json:"query"`
        } `json:"params"`
}

type ExtensionPointResponse struct {
        Result string `json:"result"`
}

func main() {
        router := gin.Default()

        router.POST("/new-api-for-dify", func(c *gin.Context) {
                var req ExtensionPointRequest

                if err := c.BindJSON(&req); err != nil {
                        c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
                        return
                }

                if req.Point == "ping" {
                        c.JSON(http.StatusOK, ExtensionPointResponse{Result: "pong"})
                        return
                }

                if req.Point != "app.external_data_tool.query" {
                        c.JSON(http.StatusBadRequest, gin.H{"error": "invalid point"})
                        return
                }

                keywords, exist := req.Params.Inputs["keywords"]
                if !exist {
                        c.JSON(http.StatusBadRequest, gin.H{"error": "missing keyword"})
                        return
                }

                s := strings.TrimSpace(fmt.Sprintf("%s", keywords))
                if s == "" {
                        c.JSON(http.StatusBadRequest, gin.H{"error": "empty keyword"})
                        return
                }

                movies, err := GetSearchResult(s, 3, "movies", 0, "soulteary")
                if err != nil {
                        c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
                        return
                }

                var result string
                for _, movie := range movies.Results {
                        for _, hit := range movie.Hits {
                                result += fmt.Sprintf("- 标题：%s\n", hit.Title)
                                result += fmt.Sprintf("- 简介：%s\n\n", hit.Overview)
                        }
                }

                c.JSON(http.StatusOK, ExtensionPointResponse{Result: result})
        })

        router.Run(":8084")
}
```

> 这段Go语言代码是一个使用`Gin`框架和MeiliSearch创建的HTTP服务器。它接收一个关键词，然后在MeiliSearch的`movies`索引中搜索匹配的电影，返回搜索结果。

> 以下是代码的主要部分：

1. `QueryPayload`、`QueryBody`、`SearchResults`、`SearchResult`、`ExtensionPointRequest`和`ExtensionPointResponse`结构体用于解析和生成HTTP请求和响应的JSON数据。
2. `GetSearchResult`函数接收一个关键词、结果数量、索引名、页码和API密钥，然后向MeiliSearch服务器发送POST请求进行搜索。搜索结果被解析为`SearchResults`结构体。
3. 在`main`函数中，创建了一个Gin路由器，并定义了一个POST路由`/new-api-for-dify`。这个路由的处理函数首先解析请求数据，然后获取关键词，调用`GetSearchResult`函数获取搜索结果，然后返回处理后的搜索结果。
4. 如果在解析请求、获取关键词或搜索的过程中发生错误，服务器会返回一个状态码为400的响应，响应体包含一个`error`字段，值为错误信息。
5. 如果搜索成功，遍历搜索结果，生成一个包含电影标题和简介的字符串，然后返回一个状态码为200的响应，响应体包含一个`Result`字段，值为生成的字符串。
6. 最后，启动HTTP服务器，监听8084端口。

> 这个服务器可以作为一个微服务，为其他服务提供电影搜索功能。

## 构建微服务镜像

```bash
docker build -t movie .
```

## 运行容器化实例

```bash
docker run -d -p 8084:8084 --name movie movie
```

或者直接运行现成的容器

```bash
docker run -d -p 8084:8084 --name movie chengzh/movie
```

## 在DIFY中配置外部数据接口

打开 Dify，然后在右上角用户图标的下拉菜单中选择“设置”选项。

![](img/bbcad266c1996eef83616f19113e1e80.png)

在打开的弹出窗口中选择“新增 API 扩展”。

![](img/950bbcbe9e258b6c06dea56d4d688649.png)

在新的弹出窗口中填入我们上面完成的 “API 框架” 的基础信息，你可以根据你的需求来，我这里就起名为“电影检索”啦。

+   名称：`api`
+   API Endpoint：`http://172.17.0.1:8084/new-api-for-dify`
+   API-key：`soulteary`

![](img/12bd53edae3ceb9f2a83d11c10ad135e.png)

点击保存，如果一切顺利，你的 Dify 界面中将出现我们自定义的 API，让 Dify 访问我们自定义的 API 就完成啦。

![](img/375bc7fb1f6e2f162138eaf5ad0055d3.png)

## 创建调用外部数据 API 的 AI 应用

![](img/2f3e98f17e9d52424ef11e0588d57310.png)

返回 Dify 的首页，选择 “创建空白应用”，根据你的喜好给应用起个名字，选个图标。

![](img/a24a463dc06564bd09bdb8968a348d1d.png)

接着，在新应用的界面中，根据自己的具体情况更新模型参数。这里我们希望模型有一定的“自由发挥空间”，所以将温度设置为了 0.3，如果你希望模型能够多说一些，那么可以适当将“最大标记”调整到更高的数值。

![](img/95e8916afe8ca448a79335e91ed40394.png)

然后，我们点击“变量”模块的“添加”按钮，在下拉菜单中选择“基于 API 的变量”。

![](img/5d0696159957c9390c2a19306456069c.png)

在新弹出的窗口中，完成“基于 API 的变量”的配置。

![](img/476a4dc5ae3675b60403f2fd858e1cfb.png)

当我们配置完毕，就能够在编写 Prompt 的时候，调用这个 API 变量啦。

如果我们想在请求中调用外部 API 数据（搜索引擎结果），可以编写一个类似下面的简单的 Prompt：

```
你是一个资深的电影影评人，擅长根据“参考信息”来生成充满趣味性的回答，始终使用中文输出内容。
## 参考信息
{{movies_data}}

## 最终输出
```

因为我们一般会使用关键词来减少搜索引擎的数据查找范围，来更快的获取更准确的信息，所以我们可以再创建一个新的变量，来传递给搜索引擎 API。

![](img/550d69b9ab7e001753e994cda5b2f4ae.png)

还是在“变量”模块选择“添加”，这次添加一个“文本变量”即可。

![](img/397a3350e5b3d9160306191705c69595.png)

变量添加完毕，我们就能够在对话的过程中，在界面中预先定义要搜索的内容是什么了，这个变量不需要在 Prompt 中体现，所以我们不需要修改 Prompt 的内容。

## 验证程序

![](img/d9b54c067a96f5bffa222bd28af733e5.png)

好了，一切就绪，让我们来重新使用这个 AI 应用。再次在关键词中搜索`sky`，并问模型应用“为什么推荐这几部电影”。



![](img/404f5ba1a0883f83b4a8174e780964cf.png)

模型会将搜索引擎中得到的前三条电影填充到 `Prompt` 中，并提交到模型服务中，进行结果推理。因为设置了 `Prompt` 始终使用中文输出，所以即便是搜索引擎搜索回的内容都是英文数据，也还是被翻译为了不错的中文结果。

![](img/6c08f06459dc13479821eb03e6a4341f.png)

让我们来验证下多个关键词的检索能力，修改搜索关键词为 `sky cas`（我只拼了一部分的“城堡”单词），然后重新提问。

稍等片刻，我们能够看到，模型应用在后台更新了搜索词，输出了包含天空之城在内的新的搜索结果的答案，并给出了一段还可以的总结。

![](img/62c39788c6057ad0f8c9d1b313c9479e.png)

因为我们创建的是一个 `Dify` 聊天应用，所以我们可以对模型进行追问，来了解自己更想了解的内容。相比完全让模型“抹黑”生成内容，在提供了必要的准确上下文后，模型的输出结果明显更靠谱了，对吧？

![](img/33f2763d15a119b2f06c0545cc5be545.png)

到这里，我们就完成了整个 AI 应用的基础搭建，你可以根据你自己的具体情况来做进一步的优化和完善。

![](img/df8e793135e51ac8aa833bdd51f51747.png)

当我们完成了应用调整后，访问这个 AI 应用的“概览页面”，找到模型应用的“预览”按钮。点击它，浏览器将打开一个新窗口，在这个新窗口中，我们能够访问我们配置好的 `Chat` 应用。

![](img/4d5fe3f55cd1d8813d736e3c6adb8708.png)

在这个界面中，我们就可以使用自然语言的方式，随意的向我们的搜索引擎提问啦。

# Lab 5：创建自定义工具实现 AI 信息检索

## 实现 API 接口

`Clone` 项目 `Repo`

```
git clone https://gitee.com/cloudzun/dify-with-ai-agent
cd dify-with-ai-agent/dify/
```

分析 `main.go`

```
nano main.go
```

```
// base on: https://github.com/soulteary/dify-simple-rag-with-wp/blob/main/main.go

package main

import (
        "encoding/json"
        "fmt"
        "io"
        "net/http"
        "strings"

        "github.com/gin-gonic/gin"
)

type QueryPayload struct {
        Queries []QueryBody `json:"queries"`
}

type QueryBody struct {
        IndexUID string `json:"indexUid"`
        Q        string `json:"q"`
        Limit    int    `json:"limit"`
        Offset   int    `json:"offset"`
}

type SearchResults struct {
        Results []SearchResult `json:"results"`
}

type SearchResult struct {
        IndexUID string `json:"indexUid"`
        Hits     []struct {
                ID          int      `json:"id"`
                Title       string   `json:"title"`
                Overview    string   `json:"overview"`
                Genres      []string `json:"genres"`
                Poster      string   `json:"poster"`
                ReleaseDate int      `json:"release_date"`
        } `json:"hits"`
        Query              string `json:"query"`
        ProcessingTimeMs   int    `json:"processingTimeMs"`
        Limit              int    `json:"limit"`
        Offset             int    `json:"offset"`
        EstimatedTotalHits int    `json:"estimatedTotalHits"`
}

func GetSearchResult(search string, count int, indexes string, page int, token string) (result SearchResults, err error) {
        client := &http.Client{}

        var queryBody QueryBody
        queryBody.IndexUID = indexes
        queryBody.Q = search
        queryBody.Limit = count
        queryBody.Offset = page

        var queryPayload QueryPayload
        queryPayload.Queries = append(queryPayload.Queries, queryBody)

        payload, err := json.Marshal(queryPayload)
        if err != nil {
                return result, err
        }

        var data = strings.NewReader(string(payload))
        req, err := http.NewRequest("POST", "http://172.17.0.1:7700/multi-search", data)
        if err != nil {
                return result, err
        }

        req.Header.Set("Authorization", fmt.Sprintf("Bearer %s", token))
        req.Header.Set("Content-Type", "application/json")
        resp, err := client.Do(req)
        if err != nil {
                return result, err
        }
        defer resp.Body.Close()

        bodyText, err := io.ReadAll(resp.Body)
        if err != nil {
                return result, err
        }

        err = json.Unmarshal(bodyText, &result)
        if err != nil {
                return result, err
        }
        return result, nil
}

type ExtensionPointRequest struct {
        Point  string `json:"point"`
        Params struct {
                AppID        string                 `json:"app_id"`
                ToolVariable string                 `json:"tool_variable"`
                Inputs       map[string]interface{} `json:"inputs"`
                Query        string                 `json:"query"`
        } `json:"params"`
}

type ExtensionPointResponse struct {
        Result string `json:"result"`
}

func main() {
        router := gin.Default()

        router.GET("/new-api-for-dify", func(c *gin.Context) {
                s := strings.TrimSpace(c.Query("keyword"))
                if s == "" {
                        c.JSON(http.StatusBadRequest, ExtensionPointResponse{Result: "empty keyword"})
                        return
                }

                movies, err := GetSearchResult(s, 3, "movies", 0, "soulteary")
                if err != nil {
                        c.JSON(http.StatusBadRequest, ExtensionPointResponse{Result: err.Error()})
                        return
                }

                var result string
                for _, movie := range movies.Results {
                        for _, hit := range movie.Hits {
                                result += fmt.Sprintf("- 标题：%s\n", hit.Title)
                                result += fmt.Sprintf("- 简介：%s\n\n", hit.Overview)
                        }
                }

                c.JSON(http.StatusOK, ExtensionPointResponse{Result: result})
        })

        router.Run(":8084")
}
```

> 这段 `Go` 语言代码是一个使用 `Gin` 框架和 `MeiliSearch` 创建的 `HTTP` 服务器。它接收一个关键词，然后在 `MeiliSearch` 的 `movies` 索引中搜索匹配的电影，返回搜索结果。

> 以下是代码的主要部分：

1. `QueryPayload`、`QueryBody`、`SearchResults`、`SearchResult`、`ExtensionPointRequest` 和 `ExtensionPointResponse` 结构体用于解析和生成 HTTP 请求和响应的 JSON 数据。

1. `GetSearchResult` 函数接收一个关键词、结果数量、索引名、页码和 API 密钥，然后向 `MeiliSearch` 服务器发送 `POST` 请求进行搜索。搜索结果被解析为 `SearchResults` 结构体。

1. 在 `main` 函数中，创建了一个 `Gin` 路由器，并定义了一个 `GET` 路由 `/new-api-for-dify`。这个路由的处理函数首先解析请求数据，然后获取关键词，调用 `GetSearchResult` 函数获取搜索结果，然后返回处理后的搜索结果。

1. 如果在解析请求、获取关键词或搜索的过程中发生错误，服务器会返回一个状态码为 400 的响应，响应体包含一个 `Result` 字段，值为错误信息。

1. 如果搜索成功，遍历搜索结果，生成一个包含电影标题和简介的字符串，然后返回一个状态码为 200 的响应，响应体包含一个 `Result` 字段，值为生成的字符串。

1. 最后，启动 HTTP 服务器，监听 8084 端口。

> 这个服务器可以作为一个微服务，为其他服务提供电影搜索功能。

创建容器化微服务应用

```
docker build -t aiagent .
```

```
docker run -d -p 8084:8084 --name aiagent aiagent
```

```
docker run -d -p 8084:8084 --name aiagent chengzh/aiagent
```

## 创建 `Dify` AI Agent 应用

在首页点击创建空白应用，然后在弹出窗口中选择 `Agent` 类型的应用，根据你的喜好填写应用名称和描述：

![](img/5a701086596b2fbd9587cd2afd618edf.png)

在右侧模型列表中选择我们要使用的模型：

![](img/04ab8978b001537d9209223889d308a5.png)

然后，根据自己的需求，对模型超参数进行调整：

![](img/9c6f70fbaf7c2dfb0c358ac8e5f08f10.png)

从 `Dify` 顶部菜单选择“工具”，在工具页面点击左侧的“创建自定义工具”。

![](img/2a1fa896d5583e512b5d2e9a78cb08be.png)

结合我们上面创建的接口信息，在弹出窗口填写适当的配置，就能够完成工具创建啦。参考上面的我们实现的接口，可以轻松实现一个 `JSON` 配置：

```
{
  "openapi": "3.1.0",
  "info": {
    "title": "电影搜索",
    "description": "从本地电影数据库中获取电影信息",
    "version": "v1.0.0"
  },
  "servers": [
    {
      "url": "http://172.17.0.1:8084" 
    }
  ],
  "paths": {
    "/new-api-for-dify": {
      "get": {
        "description": "根据关键词获取电影信息",
        "operationId": "GetMoviesByKeyword",
        "parameters": [
          {
            "name": "keyword",
            "in": "query",
            "description": "想要搜索的关键词",
            "required": true,
            "schema": {
              "type": "string"
            }
          }
        ],
        "deprecated": false
      }
    }
  },
  "components": {
    "schemas": {}
  }
}
```

> 这段代码是一个 `OpenAPI` 规范的 `JSON` 文档，也被称为 `Swagger` 文档。`OpenAPI` 规范是一个 `API` 描述格式，用于描述 `RESTful` `API` 的结构和行为。

> 以下是代码的主要部分：



## OpenAPI 文档字段说明

1. `openapi`字段指定了OpenAPI规范的版本，这里是3.1.0。
1. `info`字段包含了API的基本信息，包括标题、描述和版本。
1. `servers`字段定义了API服务器的URL。
1. `paths`字段定义了API的路径和操作。这里定义了一个GET操作，路径是`/new-api-for-dify`。这个操作的描述是"根据关键词获取电影信息"，操作ID是`GetMoviesByKeyword`。
1. GET操作有一个参数，名字是`keyword`，位置在查询字符串中，描述是"想要搜索的关键词"，是必需的，类型是字符串。
1. `components`字段用于定义复用的模式、参数、响应等，这里没有定义任何组件。
1. 这个文档的主要目的是描述一个电影搜索API，客户端可以根据这个文档了解API的使用方法。

当我们把配置复制粘贴到弹出窗口中，界面中“可用工具”会出现可以调用的接口，我们可以点击“测试”按钮，来验证接口的可用性。

![](img/5bfd1ca4f1537ed467423effef3bbcba.png)

举个例子，我们在弹出窗口中输入“Titanic”（泰坦尼克），能够清楚看到工具是否能够正常工作：

![](img/9bc674b508a896c686c996321a35961e.png)

## 完善DIFY AI Agent 应用并验证

我们再次打开第二步中完成的 AI Agent 应用，开始完善它的具体实现，先设置一个合适的 Prompt。

```
你是一个专业影评人，善于帮助结合用户需求点评影片中的精彩之处，始终使用中文输出。
```

### 更新 Agent Prompt

接着，在界面左侧点击“添加工具”，将我们刚刚创建好的“电影搜索”工具添加到界面中。

![](img/0254767d24c68777817f95398772702d.png)

因为 Agent 可以调用多个工具，考虑到演示的实用性，我这里添加了两个应用，我们创建的“电影搜索助手”和“飞书发送群消息”。

![](img/e3031cad4bf6d8c76df33fa3b3331cfd.png)

和上一篇文章一样，我们可以根据自己的需求，搜索具体的电影名称，或者电影类型、概况中的关键字，电影的关键词等等，我这里搜索了“狮子王”，然后要求 Agent 将原本搜索出来的英文接口结果翻译为中文，发送给我设置好的飞书群里。

当然，我是懒人，所以上面的内容，我只是写了一句话：“搜索 Lion King，将电影为什么值得看翻译为中文，结果发飞书。”

![](img/0aa5068e4a407139194f535041c12e04.png)

当我们将鼠标移动到对话消息上面，我们能够打开具体的消息日志。在界面的左侧，我们能够看到所有的调用详情，方便我们查看模型某一步为什么没有符合预期。我们换一个能够搜索到多部内容的关键词，再次进行验证：

![](img/395682c1b8b80e5c7d27cde5df21096d.png)

![](img/864d37f55e15df12a9dad9a887480a4c.png)
