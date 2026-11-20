
# SQL-Agent：实现SQL生成、翻译

> 来源：[https://v11enp9ok1h.feishu.cn/docx/F2FQdEkuRo51uMxz8DrclG9bneb](https://v11enp9ok1h.feishu.cn/docx/F2FQdEkuRo51uMxz8DrclG9bneb)

## 实现案例

![](img/339da4e649d209be1a6f0a3968b94642.png)

## 实现数据SQL-Agent

这里我们实现了一个简单的SQL助手，由于我们的目的主要是SQL生成、翻译、优化，所以我们制定的Action是一个空Action。

```
"""SQL Assistant Agent.
Use RAG and Schema-Linking, which can generate,translate,
and correct SQL based on user questions.
"""

import asyncio
from dbgpt.agent import BlankAction, ConversableAgent, DynConfig, ProfileConfig

class SQLAssistantAgent(ConversableAgent):
    """SQL Agent Use RAG and Schema-Linking."""

    profile: ProfileConfig = ProfileConfig(
        name=DynConfig(
            "kevin",
            category="agent",
            key="dbgpt_agent_expand_sql_expert_name",),
        ),
    role=DynConfig(
        "SQLExpert", category="agent", key="dbgpt_agent_expand_sql_expert_name_role",
    ),
    goal=DynConfig(
        "SQL generation, SQL translate, SQL diagnostics",
        category="agent",
        key="dbgpt_agent_expand_sql_expert_name_goal",
    ), constraints=DynConfig(
        [
            """You are a SQL expert who can generate, translate,and correct SQL based on user questions.If the user's input is not related to SQL,you need to prompt the user to output SQL-related content."""
        ], category="agent", key="dbgpt_agent_expand_sql_expert_profile_constraints",), desc=DynConfig("""You are a SQL expert who can generate, translate,and correct SQL based on user questions.""", category="agent", key="dbgpt_agent_expand_sql_expert_name_desc",),)
    def __init__(self, **kwargs):"""Create a new SQLAssistantAgent."""super().__init__(**kwargs)self._init_actions([BlankAction])
```

SQL Agent 还需要一个数据库连接作为依赖的资源，这里我们创建一个SQLite表：

```
def _create_temporary_connector():
    """Create a temporary database connector for testing."""
    from dbgpt.datasource.rdbms.conn_sqlite import SQLiteTempConnector

    connector = SQLiteTempConnector.create_temporary_db()
    connector.create_temp_tables(
        {
            "user": {
                "columns": {
                    "id": "INTEGER PRIMARY KEY",
                    "name": "TEXT",
                    "age": "INTEGER",
                },
                "data": [
                    (1, "Tom", 10),
                    (2, "Jerry", 16),
                    (3, "Jack", 18),
                    (4, "Alice", 20),
                    (5, "Bob", 22),
                ],
            }
        }
    )
    return connector
```

构建Agent，绑定资源并做初始化。

```
import asyncio
from dbgpt.agent import AgentContext, AgentMemory, UserProxyAgent, LLMConfig
from dbgpt.agent.resource import RDBMSConnectorResource
from dbgpt.model.proxy import OpenAILLMClient
# 假设 _create_temporary_connector 函数在同一作用域或已正确导入，这里补充下函数定义（如果之前没完整提供）
def _create_temporary_connector():
    """Create a temporary database connector for testing."""
    from dbgpt.datasource.rdbms.conn_sqlite import SQLiteTempConnector

    connector = SQLiteTempConnector.create_temporary_db()
    connector.create_temp_tables(
        {
            "user": {
                "columns": {
                    "id": "INTEGER PRIMARY KEY",
                    "name": "TEXT",
                    "age": "INTEGER",
                },
                "data": [
                    (1, "Tom", 10),
                    (2, "Jerry", 16),
                    (3, "Jack", 18),
                    (4, "Alice", 20),
                    (5, "Bob", 22),
                ],
            }
        }
    )
    return connector
async def main():
    from dbgpt.agent import SQLAssistantAgent  # 补充导入，原代码里用了但没显示导入
    agent_memory = AgentMemory()

    llm_client = OpenAILLMClient(model_alias="gpt-3.5-turbo")
    context: AgentContext = AgentContext(conv_id="sql_agent")

    connector = _create_temporary_connector()
    sqlite_resource = RDBMSConnectorResource("SQLite Database", connector=connector)
    user_proxy = await UserProxyAgent().bind(agent_memory).bind(context).build()
    sql_agent = (
        await SQLAssistantAgent()
       .bind(context)
       .bind(LLMConfig(llm_client=llm_client))
       .bind(sqlite_resource)
       .bind(agent_memory)
       .build()
    )

    await user_proxy.initiate_chat(
        recipient=sql_agent,
        reviewer=user_proxy,
        message="What is the name and age of the user with age less than 18",
    )

    print(await agent_memory.gpts_memory.one_chat_completions("sql_agent"))

if __name__ == "__main__":
    asyncio.run(main())
```

本Agent中，我们准备了两类资源。1. 知识库  2. 数据库。知识库里面主要对一些字段与上下文进行描述。

![](img/d45a340a57f837a6982fd7dc1e76e2a9.png)

![](img/ebe94fb31769f3f636714415e2035e9d.png)
