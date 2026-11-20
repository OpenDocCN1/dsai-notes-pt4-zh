
```
{'session_001': Session(id='session_001', app_name='test_app', user_id='user_1', state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text='你好，我叫陈明，好久不见！')], role='user'), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id='e-14ecf65e-58da-4059-b5d0-258f6d63069e', author='user', actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id='MiKceB3V', timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text='你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？')], role='model'), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id='e-14ecf65e-58da-4059-b5d0-258f6d63069e', author='simple_agent', actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id='kpWFucAM', timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text='很高兴认识你，你还记得我叫什么名字么？')], role='user'), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id='e-c76b5a87-154e-4f64-a773-d87dccec320f', author='user', actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id='wXFFrypr', timestamp=1745765160.231687), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text='当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  \n\n很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？')], role='model'), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id='e-c76b5a87-154e-4f64-a773-d87dccec320f', author='simple_agent', actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id='mZTBij0B', timestamp=1745765160.231687)], last_update_time=1745765160.231687)}
```

进一步查看用户`user_1`的`session_001`对话信息：

```
session_service.sessions['test_app']['user_1']['session_001']
```

```
Session(id='session_001', app_name='test_app', user_id='user_1', state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text='你好，我叫陈明，好久不见！')], role='user'), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id='e-14ecf65e-58da-4059-b5d0-258f6d63069e', author='user', actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id='MiKceB3V', timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text='你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？')], role='model'), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id='e-14ecf65e-58da-4059-b5d0-258f6d63069e', author='simple_agent', actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id='kpWFucAM', timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text='很高兴认识你，你还记得我叫什么名字么？')], role='user'), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id='e-c76b5a87-154e-4f64-a773-d87dccec320f', author='user', actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id='wXFFrypr', timestamp=1745765160.231687), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text='当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  \n\n很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？')], role='model'), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id='e-c76b5a87-154e-4f64-a773-d87dccec320f', author='simple_agent', actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id='mZTBij0B', timestamp=1745765160.231687)], last_update_time=1745765160.231687)
```

由此可见ADK本身的对话管理非常完整细致。我们只需要重复调用`await call_agent_async(query, runner, USER_ID, SESSION_ID)`，这个流程本身就是在执行多轮对话。

## 2. ADK会话容器与会话管理器

谷歌ADK的企业级用户会话管理功能的实现，主要依托于`InMemorySessionService`和`Session`两个类，此前我们曾经分别使用过，接下来详细介绍这两个对象的基本功能。

```
from google.adk.sessions import InMemorySessionService, Session
```

```
InMemorySessionService?
```

```
Init signature: InMemorySessionService()
Docstring:      An in-memory implementation of the session service.
File:           c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\google\adk\sessions\in_memory_session_service.py
Type:           ABCMeta
Subclasses: 
```

### 2.1 会话容器 Session

在此前我们首次创建会话管理器`session_service`的时候，曾经使用`create`命令初始化过一个会话容器：

![](img/c507931080709ed3b68d7852184a0776.png)

而伴随着对话的进行，多轮对话信息都保存在`session_service.sessions`中，而最开始创建的会话容器`session`还保留着最初始的状态：

```
session
```

```
Session(id='session_001', app_name='test_app', user_id='user_1', state={}, events=[], last_update_time=1745764835.901281)
```

相比每次对话时都会带入的`session_service.sessions`对象，会话容器`session`更像是一种备份、或者充当保存点的角色，我们可以使用`session_service.get_session`内容创建一个包含当前对话全部信息的会话容器：

```
session_all = session_service.get_session(
    app_name=APP_NAME,
    user_id=USER_ID,
    session_id=SESSION_ID
)
```

此时`session_all`就包含了全部会话信息：

```
session_all
```
