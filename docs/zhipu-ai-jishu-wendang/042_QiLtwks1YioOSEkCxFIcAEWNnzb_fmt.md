
# LangChain-Chatchat

> 来源：[https://ai.feishu.cn/docx/LMR1d4OTnokn9jxbqGXcGSZOnuf](https://ai.feishu.cn/docx/LMR1d4OTnokn9jxbqGXcGSZOnuf)

Github地址: https://github.com/chatchat-space/Langchain-Chatchat

> LangChain-Chatchat 是基于 ChatGLM 等大语言模型与 Langchain 等应用框架实现，开源、可离线部署的检索增强生成(RAG)大模型知识库项目。

本章节将带领开发者在 `LangChain-Chatchat` 中配置 `ChatGLM3-6B` 模型和 `Function Call` 功能。

### 配置 `configs/model_config.py`

在 `configs/model_config.py` 中设置 `ChatGLM3-6B` 的地址，并将 `LLM_MODEL` 修改为 `chatglm3-6b`

```
MODEL_PATH = {
    "llm_model": {
        ...
        "chatglm3-6b": "THUDM/chatglm3-6b",
        ...
    },
}

# LLM 名称
LLM_MODEL = "chatglm3-6b"

# LLM通用对话参数
TEMPERATURE = 0.01 #设置成0.01效果更佳
TOP_P = 0.95
```

### 配置 `configs/server_config.py`

在 `configs/server_config.py` 中添加 `ChatGLM3-6B` 的服务。

```
# 可以如下示例方式更改默认配置
    "chatglm3-6b": { # 使用default中的IP和端口
       "device": "auto",
    },
```

### 配置 `configs/prompt_config.py`

在 `configs/prompt_config.py` 中设置适合于 `ChatGLM3-6B` 的提示词

```
PROMPT_TEMPLATES["agent_chat"] = {
"ChatGLM3":
"""
You can answer using the tools, or answer directly using your knowledge without using the tools.Respond to the human as helpfully and accurately as possible.
You have access to the following tools:
{tools}
Use a json blob to specify a tool by providing an action key (tool name) and an action_input key (tool input).
Valid "action" values: "Final Answer" or  [{tool_names}]
Provide only ONE action per $JSON_BLOB, as shown:

{{{

  "action": $TOOL_NAME,

  "action_input": $INPUT

}}}

Follow this format:
    
Question: input question to answer
Thought: consider previous and subsequent steps
Action:
```

$JSON_BLOB

```
Observation: action result
... (repeat Thought/Action/Observation N times)
Thought: I know what to respond
Action:
```

{{{

  "action": "Final Answer",

  "action_input": "Final response to human"

}}}

Begin! Reminder to ALWAYS respond with a valid json blob of a single action. Use tools if necessary. Respond directly if appropriate. Format is Action:```$JSON_BLOB```then Observation:.

history: {history}

Question: {input}

Thought: {agent_scratchpad}
""",
}
```

在该提示词下，模型能较好的发挥出 `Function Call` 的性能。因此，不要大规模的修改 `Agent` 的提示词模板。

我们欢迎开发者为我们和 `Langchain-Chatchat` 社区提出 `PR`，让模型的支持更上一层。

### 适配效果

当用户适配上框架之后，正常使用 `ChatGLM3-6B` 并调用工具的状态如图所示

![](img/3617bd0ef4ebd365f72ce3ed6a6e4a58.png)
