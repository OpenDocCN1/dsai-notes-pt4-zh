
```python
def write_file_now(content: str) -> str:
    """
    Writes the specified content to a local file.

    Args:
        content (str): The content to be written to the file.

    Returns:
        str: A message indicating whether the content was successfully written to the file.
             In this case, it returns "已成功写入本地文件" if the writing is successful.
    """

    return "输入信息格式不对，未能成果写入本地"
```

```python
# 重新创建基础会话信息和会话存储器
session_service = InMemorySessionService()

APP_NAME = "test_app"
USER_ID = "user_3"
SESSION_ID = "session_001"

session = session_service.create_session(
    app_name=APP_NAME,
    user_id=USER_ID,
    session_id=SESSION_ID
)
print(f"Session created: App='{APP_NAME}', User='{USER_ID}', Session='{SESSION_ID}'")
```

```
Session created: App='test_app', User='user_3', Session='session_001'
```

```python
# 重新创建Agent
write_file_agent = Agent(
    name="write_file_agent",
    model=model, 
    description="用于进行信息记录的智能体",
    instruction="你是一个乐于助人的助手。"
                "当用户询需要将一些信息记录到本地的时候，"
                "使用 'write_file_now' 将指定内容记录到本地"    
                "如果工具返回错误，礼貌地告知用户。",
    tools=[write_file_now], 
)

print(f"Agent '{multi_tool_agent.name}' created using model '{model}'.")
```

```
Agent 'multi_tool_agent' created using model 'model='deepseek/deepseek-chat' llm_client=<google.adk.models.lite_llm.LiteLLMClient object at 0x000001F7313C3C10>'.
```

```python
runner = Runner(
    agent=write_file_agent, 
    app_name=APP_NAME,   
    session_service=session_service 
)
```

然后进行对话：

```python
query='请帮我围绕智能体编写一首俳句，并写入本地文档。'
```

```python
await call_agent_async(query, runner, USER_ID, SESSION_ID)
```

````
>>> User Query: 请帮我围绕智能体编写一首俳句，并写入本地文档。
  [Event] Author: write_file_agent, Type: Event, Final: False, Content: parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=FunctionCall(id='call_0_b2e879ba-406e-4e83-844e-0ddc1df0ba5b', args={'content': '智能体如风，\n代码流转似清泉，\n智慧藏其中。'}, name='write_file_now'), function_response=None, inline_data=None, text=None)] role='model';
  [Event] Author: write_file_agent, Type: Event, Final: False, Content: parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=FunctionResponse(id='call_0_b2e879ba-406e-4e83-844e-0ddc1df0ba5b', name='write_file_now', response={'result': '输入信息格式不对，未能成果写入本地'}), inline_data=None, text=None)] role='user';
  [Event] Author: write_file_agent, Type: Event, Final: False, Content: parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text='看起来写入文件时出现了一些问题，可能是格式不符合要求。我再尝试一次，确保内容正确写入。'), Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=FunctionCall(id='call_0_1cd79330-0e23-4b76-b050-cc77dc981d3e', args={'content': '智能体如风，\n代码流转似清泉，\n智慧藏其中。'}, name='write_file_now'), function_response=None, inline_data=None, text=None)] role='model';
  [Event] Author: write_file_agent, Type: Event, Final: False, Content: parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=FunctionResponse(id='call_0_1cd79330-0e23-4b76-b050-cc77dc981d3e', name='write_file_now', response={'result': '输入信息格式不对，未能成果写入本地'}), inline_data=None, text=None)] role='user';
  [Event] Author: write_file_agent, Type: Event, Final: True, Content: parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text='抱歉，我尝试了两次但未能成功将俳句写入本地文件。可能是工具对输入格式有特殊要求。以下是您需要的俳句内容，您可以手动保存：\n\n```
智能体如风，
代码流转似清泉，
智慧藏其中。
```

如果您有其他需求，请随时告诉我！')] role='model';
````

通过观察事件流不难发现，ADK在调用外部函数报错时，总共会尝试三次，属于正常范围，可以放心使用。

## 三、ADK+外部工具的前端调用与测试流程

### 1. ADK基础外部工具调用流程

接下来我们进一步尝试在Web端调用包含外部工具的ADK开发的Agent。借助Dev UI，我们能够更加直观的观察到Agent调用外部哦工具的事件流，并进行更高效率的开发。

顺着上一小节的前端项目继续，我们回到`ADK_Chat`文件夹中：

![](img/3431079e2b76af1db8922d62a1e2cf4e.png)

修改`agent.py`脚本：

```
ADK_Chat/

├── test_agent/

│   ├── agent.py

│   ├── __init__.py

│   ├── .env

├── __init__.py

└── ...

```

删除原有内容，写入如下新的内容：

```
from google.adk.agents import Agent

from google.adk.models.lite_llm import LiteLlm

import os

from dotenv import load_dotenv

load_dotenv(override=True)

import requests,json

DS_API_KEY = os.getenv("DS_API_KEY")

DS_BASE_URL = os.getenv("DS_BASE_URL")

model = LiteLlm(
    model="deepseek/deepseek-chat",  
    api_base=DS_BASE_URL,
    api_key=DS_API_KEY
)

def get_weather(city: str) -> str:

    """

    Retrieves the current weather report for a specified city.

    Args:

        city (str): The name of the city (e.g., "Beijing", "Shanghai").

                   Note: For cities in China, use the city's English name (e.g., "Beijing").

    Returns:

        dict: A dictionary containing the weather information.

              Includes a 'status' key ('success' or 'error').

              If 'success', includes a 'report' key with weather details.

              If 'error', includes an 'error_message' key.

    """

    # Step 1.构建请求

    url = "https://api.openweathermap.org/data/2.5/weather"

    # Step 2.设置查询参数

    params = {

        "q": city,               

        "appid": os.getenv("OPENWEATHER_API_KEY"),    # 输入自己的API key

        "units": "metric",            # 使用摄氏度而不是华氏度

        "lang":"zh_cn"                # 输出语言为简体中文

    }

    # Step 3.发送GET请求

    response = requests.get(url, params=params)

    # Step 4.解析响应

    data = response.json()

    city = data.get("name", "未知")

    country = data.get("sys", {}).get("country", "未知")

    temp = data.get("main", {}).get("temp", "N/A")

    humidity = data.get("main", {}).get("humidity", "N/A")

    wind_speed = data.get("wind", {}).get("speed", "N/A")

    weather_list = data.get("weather", [{}])

    description = weather_list[0].get("description", "未知")

    return (

        f"🌍 {city}, {country}\n"

        f"🌡 温度: {temp}°C\n"

        f"💧 湿度: {humidity}%\n"

        f"🌬 风速: {wind_speed} m/s\n"

        f"🌤 天气: {description}\n"

    )

def write_file(content: str) -> str:

    """

    Writes the specified content to a local file.

    Args:

        content (str): The content to be written to the file.

    Returns:

        str: A message indicating whether the content was successfully written to the file.

             In this case, it returns "已成功写入本地文件" if the writing is successful.

    """

    return "已成功写入本地文件。"

root_agent = Agent(

    name="multi_tool_agent",

    model=model, 

    description="用于进行天气查询和记录的智能体",
