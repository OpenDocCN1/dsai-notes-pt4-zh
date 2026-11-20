
# Langchain —— 聊天机器人

> 来源：[https://v11enp9ok1h.feishu.cn/docx/NgjwdW7MHomRo7xyizjcY5f9nTl](https://v11enp9ok1h.feishu.cn/docx/NgjwdW7MHomRo7xyizjcY5f9nTl)

## 目录

+ [**快速入门**](https://python.langchain.com/v0.1/docs/use_cases/chatbots/quickstart/)：建议从此处开始。以下许多指南假定你完全了解快速入门中显示的体系结构。

+ [**内存管理**](https://python.langchain.com/v0.1/docs/use_cases/chatbots/memory_management/)：本节介绍聊天机器人可以用来处理先前对话轮次中的信息的各种策略。

+ [**检索**](https://python.langchain.com/v0.1/docs/use_cases/chatbots/retrieval/)：本节介绍如何使您的聊天机器人能够使用外部数据源作为上下文。

+ [**工具使用**](https://python.langchain.com/v0.1/docs/use_cases/chatbots/tool_usage/)：本节介绍如何通过添加使用工具与其他系统和 API 交互的功能，将聊天机器人转变为对话代理。

## 架构

> 设计聊天机器人需要考虑具有不同优势和权衡的各种技术，具体取决于您期望它处理的问题类型。
>
> 例如
>
> 聊天机器人通常使用[检索增强生成](https://python.langchain.com/v0.1/docs/use_cases/question_answering/)（RAG）来处理私有数据，以更好地回答特定领域的问题。您还可以选择在多个数据源之间路由，以确保它仅使用最热门的上下文进行最终问题解答，或者选择使用更专业类型的聊天历史记录或内存，而不仅仅是来回传递消息。

![](img/5a347028ba7b7df872fd6b6ed3f13ec9.png)

## 快速入门

聊天模型，输出为：`AIMessage`

```
from langchain_core.messages import HumanMessage

chat.invoke(
    [
        HumanMessage(
            content="Translate this sentence from English to French: I love programming."
        )
    ]
)
```
