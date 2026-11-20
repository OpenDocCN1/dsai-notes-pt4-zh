
而如果出现调用工具报错，则一般会尝试2-3次之后停止尝试。此外，关于调用工具报错信息的输出格式、以及控制报错响应流程，都可以使用稍后介绍的中间件来完成。

## 4. 搭建基于DeepSeek-V3.2 reasoner的Agent流程

+ 正常情况下调用会报错，因为目前`LangChain 1.1`还未兼容DeepSeek-V3.2的推理调用工具流程

```
model = ChatDeepSeek(model="deepseek-reasoner")

agent = create_agent(
    model=model,
    tools=[get_weather],
    system_prompt="你是一名多才多艺的智能助手，可以调用工具帮助用户解决问题。"
)
```

```
result = agent.invoke(
    {"messages": [{"role": "user", "content": "请问北京今天天气如何？"}]}
)
```
