
### Step 3: 执行主逻辑

```python
async def chat_loop(root_agent, exit_stack, runner, user_id, session_id) -> None:
    print("\n🤖 ADK+MCP对话已启动！输入 'quit' 退出。")

    while True:
        query = input("\n你: ").strip()
        if query.lower() == "quit":
            break
        try:
            print(f"\n>>> User Query: {query}")
            content = types.Content(role='user', parts=[types.Part(text=query)])
            final_response_text = "Agent did not produce a final response."

            async for event in runner.run_async(user_id=user_id, session_id=session_id, new_message=content):
                print(f"  [Event] Author: {event.author}, Type: {type(event).__name__}, Final: {event.is_final_response()}, Content: {event.content}")

                if event.is_final_response():
                    if event.content and event.content.parts:
                        final_response_text = event.content.parts[0].text
                    elif event.actions and event.actions.escalate:
                        final_response_text = f"Agent escalated: {event.error_message or 'No specific message.'}"
                    break

            print(f"<<< Agent Response: {final_response_text}")
        except Exception as e:
            print(f"\n⚠️  调用过程出错: {e}")
```

`chat_loop`：这是一个交互式的对话循环函数，模拟多轮对话。用户输入的查询会被传递给代理，代理生成的响应会被输出。通过 `runner.run_async` 执行代理的逻辑，并迭代事件，直到获得最终的响应。

`is_final_response()`：用于判断当前事件是否为最终响应，一旦获得最终响应就停止继续处理。

```python
async def async_main():
  root_agent, exit_stack = await get_agent_async()
  session_service = InMemorySessionService()

  app_name='mcp_filesystem_app'
  user_id = "user_1"
  session_id = "session_001"

  session = session_service.create_session(
      app_name=app_name,
      user_id=user_id,
      session_id=session_id
  )

  runner = Runner(
      app_name=app_name,
      agent=root_agent,
      session_service=session_service,
  )
  await chat_loop(root_agent, exit_stack, runner, user_id, session_id)

  print("Closing MCP server connection...")
  await exit_stack.aclose()
  print("Cleanup complete.")
```

`async_main`：主逻辑函数，首先通过 `get_agent_async` 获取一个代理，并创建一个会话（`session_service.create_session`）。然后，通过 `Runner` 启动代理并运行多轮对话（`chat_loop`）。

在对话完成后，关闭 MCP 服务器连接并进行清理工作。

## 二、使用 ADK 工具搭建 MCP 服务器（MCP 服务器暴露 ADK）

接下来我们继续尝试将一个ADK Agent封装为MCP服务器，方便其他应用程序使用。接下来我们将使用 `model-context-protocol` 库创建一个标准的 Python MCP 服务器应用程序。并在此服务器中试下你如下功能：

1. 创建一个联网搜索的 ADK 工具；

2. 实现 MCP 服务器的 `@app.list_tools` 处理程序来宣传 ADK 工具，并使用 `adk_to_mcp_tool_type` 将 ADK 工具定义转换为 MCP 模式；

3. 实现 MCP 服务器的 `@app.call_tool` 处理程序以接收来自 MCP 客户端的请求，识别请求是否针对您包装的 ADK 工具，执行 ADK 工具的 `.run_async()` 方法，并将结果格式化为符合 MCP 的响应；

还是类似的，我们采用 `uv` 进行库的管理，我们仍然还在相同的项目文件内进行操作，需要使用如下命令安装必要的虚拟环境：

```
uv add mcp bs4 lxml
```

然后创建 `server.py` 脚本，

![](img/2899050d0f1102a02cfef4f65b807ae9.png)

并写入如下内容：
