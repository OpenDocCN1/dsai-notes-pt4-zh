
# LangChain 链的使用与序列化

## 代码示例

```
chat_prompt_template = ChatPromptTemplate.from_messages([human_message_prompt])

chat = ChatOpenAI(temperature=0.9)

chain = LLMChain(llm=chat, prompt=chat_prompt_template)

print(chain.run("colorful socks"))

    Rainbow Socks Co.
```

### 链的序列化，保存链到磁盘，从磁盘加载链：

Serialization | ️ Langchain

### 总结

`LangChain` 为特定用例提供了多种组件，例如个人助理、文档问答、聊天机器人、查询表格数据、与 `API` 交互、提取、评估和汇总。通过提供模块化和灵活的方法简化了构建高级语言模型应用程序的过程。通过了解组件、链、提示模板、输出解析器、索引、检索器、聊天消息历史记录和代理等核心概念，可以创建适合特定需求的自定义解决方案。`LangChain` 能够释放语言模型的全部潜力，并在广泛的用例中创建智能的、上下文感知的应用程序。

编辑于 2023-07-02 18:25 ・IP 属地广东
