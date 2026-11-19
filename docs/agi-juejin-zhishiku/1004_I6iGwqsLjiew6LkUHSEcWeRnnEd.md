
# 多智能体RAG超级机器人：GraphRAG + AutoGen + Ollama + Chainlit

> 来源：[https://agijuejin.feishu.cn/docx/LZWCd1CSLomkP9xEgExc68efnMf](https://agijuejin.feishu.cn/docx/LZWCd1CSLomkP9xEgExc68efnMf)

> 作者：PaperAgent

> 原文：[https://mp.weixin.qq.com/s/1CmTx1sfPzZrjvAp6btVEA](https://mp.weixin.qq.com/s/1CmTx1sfPzZrjvAp6btVEA)

开源项目 `Autogen_GraphRAG_Ollama`：一个集成微软 `GraphRAG` + `AutoGen` + `Ollama` + `Chainlit` 的完全本地且免费的多智能体 `RAG` 超级机器人。

这项目简介是真的强，囊括了大模型领域里面比较火的应用，什么 `GraphRAG`、`Multi-Agent` 统统拿下。

![](img/7fa0fcbd16a380e866b0793d54cb6915.png)

## 项目特点

+ `Agentic-RAG`：通过函数调用将 `GraphRAG` 的知识搜索方法与 `AutoGen` 智能体集成。
+ `离线 LLM 支持`：配置 `GraphRAG`（本地和全局搜索）以支持来自 `Ollama` 的本地模型进行推理和嵌入。
+ `非 OpenAI 函数调用`：扩展 `AutoGen` 以支持通过 `Lite-LLM` 智能体服务器从 `Ollama` 使用非 `OpenAI` `LLM` 进行函数调用。
+ `交互式 UI`：部署 `Chainlit` UI 来处理连续对话、多线程和用户输入设置。

## 效果展示

![](img/3008e81703d6b12e8cbea7ed953691f8.png)

![](img/3c5d0201273a34c302e9644fb78ea464.png)

## 代码说明

`AssistantAgent` 与 `ChainlitUserProxyAgent` 定义：

```
retriever   = AssistantAgent(
       name="Retriever", 
       llm_config=llm_config_autogen, 
       system_message="""Only execute the function query_graphRAG to look for context. 
                    Output 'TERMINATE' when an answer has been provided.""",
       max_consecutive_auto_reply=1,
       human_input_mode="NEVER", 
       description="Retriever Agent"
    )
user_proxy = ChainlitUserProxyAgent(
        name="User_Proxy",
        human_input_mode="ALWAYS",
        llm_config=llm_config_autogen,
        is_termination_msg=lambda x: x.get("content", "").rstrip().endswith("TERMINATE"),
        code_execution_config=False,
        system_message='''A human admin. Interact with the retriever to provide any context''',
        description="User Proxy Agent"
    )
```

`agents` 的注册

```
async def query_graphRAG(
          question: Annotated[str, 'Query string containing information that you want from RAG search']
                          ) -> str:
        if LOCAL_SEARCH:
            print(LOCAL_SEARCH)
            result = run_local_search(INPUT_DIR, ROOT_DIR, COMMUNITY, RESPONSE_TYPE, question)
        else:
            result = run_global_search(INPUT_DIR, ROOT_DIR, COMMUNITY, RESPONSE_TYPE, question)
        await cl.Message(content=result).send()
        return result

for caller in [retriever]:
        d_retrieve_content = caller.register_for_llm(
            description="retrieve content for code generation and question answering.", api_style="function"
        )(query_graphRAG)

for agents in [user_proxy, retriever]:
        agents.register_for_execution()(d_retrieve_content)
```

`AutoGen` 的聊天对话逻辑

```
groupchat = autogen.GroupChat(
    agents=[user_proxy, retriever],
    messages=[],
    max_round=MAX_ITER,
    speaker_selection_method=state_transition,
    allow_repeat_speaker=True,
)
manager = autogen.GroupChatManager(groupchat=groupchat,
                                    llm_config=llm_config_autogen, 
                                    is_termination_msg=lambda x: x.get("content", "") and x.get("content", "").rstrip().endswith("TERMINATE"),
                                    code_execution_config=False,
                                    )    

# -------------------- Conversation Logic. Edit to change your first message based on the Task you want to get done. ----------------------------- # 
if len(groupchat.messages) == 0: 
    await cl.make_async(user_proxy.initiate_chat)( manager, message=CONTEXT, )
elif len(groupchat.messages) < MAX_ITER:
    await cl.make_async(user_proxy.send)( manager, message=CONTEXT, )
elif len(groupchat.messages) == MAX_ITER:  
    await cl.make_async(user_proxy.send)( manager, message="exit", )
```

```
https://github.com/karthik-codex/Autogen_GraphRAG_Ollama
```

## 推荐阅读

+ 对齐 `LLM` 偏好的直接偏好优化方法：`DPO`、`IPO`、`KTO`

+ 一篇搭建 `AI` 大模型应用平台架构的全面指南

+ [`RAG`全景图：从 `RAG` 启蒙到高级 `RAG` 之 `36` 技，再到终章 `Agentic RAG`！](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)

+ [`Agent` 到多模态 `Agent` 再到多模态 `Multi-Agents` 系统的发展与案例讲解（1.2万字，20+文献，27张图）](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)

---

欢迎关注我的👸号 "`PaperAgent`"，每天一篇大模型（`LLM`）文章来锻炼我们的思维，简单的例子，不简单的方法，提升自己。
