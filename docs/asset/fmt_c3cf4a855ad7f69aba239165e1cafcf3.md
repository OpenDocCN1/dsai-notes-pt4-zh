
```python
# interactive_search.py
import os
import json
import requests # 导入 requests 库
from openai import OpenAI, APIError
import traceback # 用于打印详细错误

# --- (复用之前的 API Key 获取和 client 初始化代码) ---
api_key = os.getenv("AOAI_API_KEY")
if not api_key:
    raise ValueError("请设置环境变量 AOAI_API_KEY")

client = OpenAI(api_key=api_key, base_url="https://api.huaqloud.com/v1")

# --- SearXNG 配置 ---
SEARXNG_BASE_URL = "https://searxng.huaqloud.com" # 您提供的 SearXNG 实例地址
MAX_SEARCH_RESULTS = 3 # 只返回前 N 条结果给模型，避免信息过载和 Token 消耗

# --- 步骤 1 & 5: 定义并实现 SearXNG 搜索函数 ---
def perform_searxng_search(query: str) -> str:
    """
    使用指定的 SearXNG 实例执行网页搜索并返回格式化的结果。

    Args:
        query: 用户输入的搜索关键词。

    Returns:
        一个包含前 N 条搜索结果（标题、URL、摘要）的格式化字符串，
        或者在出错时返回包含错误信息的 JSON 字符串。
    """
    print(f"--- 工具执行：正在使用 SearXNG 搜索 '{query}' ---")
    headers = {
        # 设置一个 User-Agent 是个好习惯
        'User-Agent': 'AOAIInteractiveApp/1.0 (Tool Call; +https://your-project-url)'
    }
    params = {
        'q': query,
        'format': 'json' # 请求 JSON 格式的输出
        # 根据 SearXNG API 文档，可以添加 'language', 'time_range', 'safesearch' 等参数
    }

    try:
        # 设置超时时间 (连接超时5秒, 读取超时10秒)
        response = requests.get(SEARXNG_BASE_URL + "/", params=params, headers=headers, timeout=(5, 10))
        # 检查 HTTP 状态码，如果不是 200-299，则抛出异常
        response.raise_for_status()

        data = response.json()
        results = data.get('results', [])

        if not results:
            print("--- SearXNG 返回了空结果 ---")
            # 返回一个信息性 JSON 字符串，告知模型没有结果
            return json.dumps({"info": f"SearXNG did not return any results for the query: '{query}'."})

        # 格式化前 N 条结果
        formatted_results = []
        for i, result in enumerate(results[:MAX_SEARCH_RESULTS]):
            title = result.get('title', 'N/A')
            url = result.get('url', 'N/A')
            # 'content' 字段通常包含摘要，可能需要根据实际情况调整字段名
            content = result.get('content', 'N/A')
            # 移除摘要中的换行符，简化输出
            content = content.replace('\n', ' ').replace('\r', ' ') if content else 'N/A'
            formatted_results.append(f"{i+1}. Title: {title}\n   URL: {url}\n   Snippet: {content}\n")

        # 最终返回给模型的字符串
        final_output = f"Web search results for '{query}':\n\n" + "\n".join(formatted_results)
        # 打印部分结果用于调试，避免控制台输出过多
        print(f"--- SearXNG 工具执行结果 (格式化后):\n{final_output[:300]}{'...' if len(final_output) > 300 else ''} ---")
        return final_output # 直接返回格式化字符串

    except requests.exceptions.Timeout:
        print(f"--- SearXNG 请求超时 ---")
        return json.dumps({"error": f"SearXNG request timed out for query: '{query}'."})
    except requests.exceptions.RequestException as e:
        print(f"--- SearXNG 请求失败: {e} ---")
        # 返回包含具体请求错误的 JSON 字符串
        return json.dumps({"error": f"Failed to connect to SearXNG or request failed: {str(e)}"})
    except json.JSONDecodeError as e:
        print(f"--- 解析 SearXNG 响应失败: {e} ---")
        # 尝试打印部分响应文本帮助调试 SearXNG 返回的非 JSON 内容
        response_text = response.text if 'response' in locals() else 'N/A'
        print(f"--- Response Text (first 500 chars): {response_text[:500]} ---")
        return json.dumps({"error": f"Failed to parse JSON response from SearXNG: {str(e)}. Response started with: {response_text[:100]}"})
    except Exception as e:
        print(f"--- SearXNG 工具执行中发生未知错误: {e} ---")
        # 记录未知错误
        return json.dumps({"error": f"An unexpected error occurred during the search: {str(e)}"})


# --- 步骤 1 (续): 定义工具的描述 ---
tools = [
    {
        "type": "function",
        "function": {
            "name": "web_search",
            # description 对模型能否正确使用工具至关重要
            "description": "当需要获取关于当前事件、最新发展、特定网站的实时内容或任何模型自身知识库可能未包含的信息时，使用此工具执行网页搜索。适用于回答例如“今天有什么科技新闻？”、“某某公司的最新财报怎么样？”、“请总结一下[某个URL]页面的主要内容”等问题。",
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {
                        "type": "string",
                        "description": "用于网页搜索的具体、明确的关键词或问题。",
                    },
                },
                "required": ["query"], # query 参数是必须的
            },
        }
    }
]

print("\n\n--- 场景六 (交互式升级版): 使用 SearXNG 进行网页搜索 ---")
print("你好！我是可以上网搜索的 AOAI 助手。")
print("输入你想查询的内容。如果需要最新信息，我会尝试使用 SearXNG 搜索。")
print("输入 'quit' 退出程序。")

# --- 开始交互式循环 ---
while True:
    try:
        user_input = input("\n你 (输入 'quit' 退出): ")
        if user_input.lower() == 'quit':
            print("\n机器人: 感谢使用，再见！")
            break
        if not user_input.strip(): # 跳过空输入或只有空格的输入
            continue

        # --- 为当前交互初始化消息列表 ---
        # 注意：当前实现为单轮工具调用优化，不保留长期对话历史。
        # 如需多轮记忆，需在循环外维护 history 列表，并在循环内添加消息。
        messages = [{"role": "user", "content": user_input}]

        # --- 第一次 API 调用 (尝试让模型决策是否使用工具) ---
        print("--- 机器人思考中 (可能需要联网搜索)... ---")
        response = client.chat.completions.create(
            model="gpt-4o-mini", # 确保所选模型支持 Tool Use
            messages=messages,
            tools=tools,
            tool_choice="auto", # 让模型自动决定
        )
        response_message = response.choices[0].message

        # --- 检查模型是否请求调用工具 ---
        tool_calls = response_message.tool_calls
        final_response = None # 初始化最终回复变量

        if tool_calls:
            print("--- 模型决定使用 SearXNG 进行搜索 ---")
            # **必须**将模型的工具调用请求添加到消息历史中
            messages.append(response_message)

            # 通常一个请求可能包含多个 tool_calls，这里简化处理第一个
            tool_call = tool_calls[0]
            function_name = tool_call.function.name
            tool_call_id = tool_call.id
            function_args = {} # 初始化参数字典

            # 健壮地解析参数 JSON 字符串
            try:
                function_args = json.loads(tool_call.function.arguments)
                print(f"  搜索查询参数: {function_args}")
            except json.JSONDecodeError as e:
                print(f"错误：解析模型生成的参数时出错: {e}")
                print(f"  原始参数字符串: {tool_call.function.arguments}")
                # 即使参数解析失败，也需要将错误信息作为工具结果返回给模型
                messages.append({
                    "tool_call_id": tool_call_id, "role": "tool",
                    "content": json.dumps({"error": "Invalid arguments format provided by the model."})
                })
                function_name = None # 标记为无需执行实际函数

            # 如果函数名正确且参数解析成功，则执行工具
            if function_name == "web_search":
                print(f"  工具调用 ID: {tool_call_id}")
                search_query = function_args.get("query")
                if search_query:
                    # --- 执行真实的 SearXNG 搜索 ---
                    function_response_content = perform_searxng_search(query=search_query)
                else:
                    print("错误：模型调用 web_search 时未提供必需的 'query' 参数！")
                    function_response_content = json.dumps({"error": "Missing required parameter 'query' from model."})

                # --- 将工具执行结果添加到消息历史 ---
                messages.append(
                    {
                        "tool_call_id": tool_call_id,
                        "role": "tool",
                        "content": function_response_content, # SearXNG 返回的格式化结果或错误信息
                    }
                )

            elif function_name: # 如果有函数名但不是我们定义的 web_search
                print(f"警告：模型请求调用未定义的工具 '{function_name}'")
                messages.append({
                    "tool_call_id": tool_call_id, "role": "tool",
                    "content": json.dumps({"error": f"Attempted to call unknown tool: {function_name}"})
                })
            # else: function_name is None due to parsing error, already handled.

            # --- 如果进行了工具调用（即使是返回错误），都需要第二次 API 调用 ---
            print("--- 机器人正在整合信息... ---")
            second_response = client.chat.completions.create(
                model="gpt-4o-mini",
                messages=messages, # 发送包含用户问题、模型请求、工具结果的完整历史
            )
            final_response = second_response.choices[0].message.content

        else:
            # --- 模型未调用工具，直接使用其第一轮回复 ---
            print("--- 模型直接回答 (未使用搜索工具) ---")
            final_response = response_message.content

        # --- 打印最终回复给用户 ---
        if final_response:
            print(f"\n机器人:\n{final_response}")
        else:
             # 兜底，理论上不应发生，除非API调用失败且未被捕获
             print("\n机器人: 抱歉，似乎出了点问题，无法生成回复。")

    # --- 统一的错误处理 ---
    except APIError as e:
        print(f"\n[API 调用出错 ({e.status_code}): {e.message}]")
        print("机器人: 抱歉，与 AOAI 服务通信时遇到问题，请稍后再试。")
    except requests.exceptions.ConnectionError as e:
        # 特别处理 SearXNG 连接失败，避免程序崩溃
        print(f"\n[网络错误: 无法连接到 SearXNG 实例 {SEARXNG_BASE_URL}] - {e}")
        print("机器人: 抱歉，无法连接到网络搜索服务，请检查网络或稍后再试。")
    except Exception as e:
        print(f"\n[发生意外错误: {type(e).__name__} - {e}]")
        traceback.print_exc() # 打印完整的错误堆栈信息以供调试
        print("机器人: 抱歉，我遇到了一些内部技术问题，暂时无法处理您的请求。")
```
