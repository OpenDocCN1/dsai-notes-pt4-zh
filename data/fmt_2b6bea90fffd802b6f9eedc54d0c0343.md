
1. 导航到分配的资源组 -> 创建的搜索服务 `aisearchLabinstanceID` -> 搜索管理 -> 索引。

![](img/cbfb8bf54fc8208cf2991540152801f1.png)

1. 如果使用相同的索引名称再次运行该脚本，它会创建同一索引的新版本。

### 任务 3：获取产品文档

接下来，创建一个脚本，用于从搜索索引中获取产品文档。该脚本会在搜索索引中查询与用户问题匹配的文档。

#### 创建脚本以获取产品文档

当聊天收到请求时，它会搜索您的数据以查找相关信息。此脚本使用 `Azure AI SDK` 查询搜索索引，查找与用户问题匹配的文档。然后，它将文档返回给聊天应用。

1. 从 `VS Code` 中，在 `src` 文件夹中创建一个名为 `get_product_documents.py` 的文件。

1. 将以下代码复制并粘贴到文件中。首先导入所需的库，创建项目客户端并配置设置。

```
import os
from pathlib import Path
from opentelemetry import trace
from azure.ai.projects import AIProjectClient
from azure.ai.projects.models import ConnectionType
from azure.identity import DefaultAzureCredential
from azure.core.credentials import AzureKeyCredential
from azure.search.documents import SearchClient
from config import ASSET_PATH, get_logger

# initialize logging and tracing objects
logger = get_logger(name)
tracer = trace.get_tracer(name)

# create a project client using environment variables loaded from the .env file
project = AIProjectClient.from_connection_string(
    conn_str=os.environ["AIPROJECT_CONNECTION_STRING"], credential=DefaultAzureCredential()
)

# create a vector embeddings client that will be used to generate vector embeddings
chat = project.inference.get_chat_completions_client()
embeddings = project.inference.get_embeddings_client()

# use the project client to get the default search connection
search_connection = project.connections.get_default(
    connection_type=ConnectionType.AZURE_AI_SEARCH, include_credentials=True
)

# Create a search index client using the search connection
# This client will be used to create and delete search indexes
search_client = SearchClient(
    index_name=os.environ["AISEARCH_INDEX_NAME"],
    endpoint=search_connection.endpoint_url,
    credential=AzureKeyCredential(key=search_connection.key),
)
```

1. 在 `get_product_documents.py` 中添加函数获取产品文档。

```
from azure.ai.inference.prompts import PromptTemplate
from azure.search.documents.models import VectorizedQuery


@tracer.start_as_current_span(name="get_product_documents")
def get_product_documents(messages: list, context: dict = None) -> dict:
    if context is None:
        context = {}
    overrides = context.get("overrides", {})
    top = overrides.get("top", 5)

    # generate a search query from the chat messages
    intent_prompty = PromptTemplate.from_prompty(Path(ASSET_PATH) / "intent_mapping.prompty")
    intent_mapping_response = chat.complete(
        model=os.environ["INTENT_MAPPING_MODEL"],
        messages=intent_prompty.create_messages(conversation=messages),
        **intent_prompty.parameters,
    )
    search_query = intent_mapping_response.choices[0].message.content
    logger.debug(f"🧠 Intent mapping: {search_query}")

    # generate a vector representation of the search query
    embedding = embeddings.embed(model=os.environ["EMBEDDINGS_MODEL"], input=search_query)
    search_vector = embedding.data[0].embedding
    # search the index for products matching the search query
    vector_query = VectorizedQuery(vector=search_vector, k_nearest_neighbors=top, fields="contentVector")
    search_results = search_client.search(
        search_text=search_query, vector_queries=[vector_query], select=["id", "content", "filepath", "title", "url"]
    )
    documents = [
        {
            "id": result["id"],
            "content": result["content"],
            "filepath": result["filepath"],
            "title": result["title"],
            "url": result["url"],
        }
        for result in search_results]

    # add results to the provided context
    if "thoughts" not in context:
        context["thoughts"] = []

    # add thoughts and documents to the context object so it can be returned to the caller
    context["thoughts"].append(
        {
            "title": "Generated search query",
            "description": search_query,
        }
    )

    if "grounding_data" not in context:
        context["grounding_data"] = []
    context["grounding_data"].append(documents)
    logger.debug(f"📄 {len(documents)} documents retrieved: {documents}")
    return documents
```

1. 最后，添加代码，直接运行脚本来测试功能：

```
if name == "__main__":
    import logging
    import argparse
# set logging level to debug when running this module directly
    logger.setLevel(logging.DEBUG)

    # load command line arguments
    parser = argparse.ArgumentParser()
    parser.add_argument(
        "--query",
        type=str,
        help="Query to use to search product",
        default="I need a new tent for 4 people, what would you recommend?",
    )
    args = parser.parse_args()
    query = args.query
    result = get_product_documents(messages=[{"role": "user", "content": query}])
```

![](img/64becf3e81dfdc1ee171a69c018d44b4.png)

1. 单击`文件`>`全部保存`。

### 任务 4：创建意图映射的提示模板

`get_product_documents.py` 脚本使用提示模板将对话转换为搜索查询。该模板指示如何从对话中提取用户意图。

1. 在运行脚本之前，请先创建提示模板。在 `assets` 文件夹下创建一个名为 `intent_mapping.prompty` 的文件：

![](img/ec3319ba31cbe004213a1c9915db1778.png)

1. 将以下代码复制到 `intent_mapping.prompty` 文件，然后从顶部栏转到文件并单击`全部保存`。

```
---
name: Chat Prompt
description: A prompty that extract users query intent based on the current_query and chat_history of the conversation
model:
    api: chat
    configuration:
        azure_deployment: gpt-4o
inputs:
    conversation:
        type: array
---
system:
# Instructions
- You are an AI assistant reading a current user query and chat_history.
- Given the chat_history, and current user's query, infer the user's intent expressed in the current user query.
- Once you infer the intent, respond with a search query that can be used to retrieve relevant documents for the current user's query based on the intent
- Be specific in what the user is asking about, but disregard parts of the chat history that are not relevant to the user's intent.
- Provide responses in json format

# Examples
Example 1:
With a conversation like below:

 - user: are the trailwalker shoes waterproof?
 - assistant: Yes, the TrailWalker Hiking Shoes are waterproof. They are designed with a durable and waterproof construction to withstand various terrains and weather conditions.
 - user: how much do they cost?

Respond with:
{
    "intent": "The user wants to know how much the Trailwalker Hiking Shoes cost.",
    "search_query": "price of Trailwalker Hiking Shoes"
}

Example 2:
With a conversation like below:

 - user: are the trailwalker shoes waterproof?
 - assistant: Yes, the TrailWalker Hiking Shoes are waterproof. They are designed with a durable and waterproof construction to withstand various terrains and weather conditions.
 - user: how much do they cost?
 - assistant: The TrailWalker Hiking Shoes are priced at $110.
 - user: do you have waterproof tents?
 - assistant: Yes, we have waterproof tents available. Can you please provide more information about the type or size of tent you are looking for?
 - user: which is your most waterproof tent?
 - assistant: Our most waterproof tent is the Alpine Explorer Tent. It is designed with a waterproof material and has a rainfly with a waterproof rating of 3000mm. This tent provides reliable protection against rain and moisture.
 - user: how much does it cost?

Respond with:
{
    "intent": "The user would like to know how much the Alpine Explorer Tent costs.",
    "search_query": "price of Alpine Explorer Tent"
}
user:
Return the search query for the messages in the following conversation:
{{#conversation}}
 - {{role}}: {{content}}
{{/conversation}}
```
