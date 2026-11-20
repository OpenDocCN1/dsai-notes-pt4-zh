
```
import os
import json # 导入 json 库用于解析和美化打印 JSON
from openai import OpenAI, APIError, BadRequestError

# --- (复用之前的 API Key 获取和 client 初始化代码) ---
api_key = os.getenv(&quot;AOAI_API_KEY&quot;)
if not api_key:
    raise ValueError(&quot;请设置环境变量 AOAI_API_KEY&quot;)

# --- 初始化 OpenAI 客户端，指向 AOAI API ---
client = OpenAI(
    api_key=api_key,
    base_url=&quot;https://api.huaqloud.com/v1&quot;
)

print(&quot;\n\n--- 场景五：强制输出 JSON 格式 (JSON Mode) ---&quot;)

# --- 定义我们期望的 JSON 结构和对模型的要求 ---
# 注意：这里的描述对模型能否正确生成至关重要
system_prompt = &quot;&quot;&quot;
你是一位富有想象力的生物学家，专门研究奇幻生物。
你的任务是根据用户的要求，创造一种生物的描述。
请**严格**按照以下 JSON 格式输出，**不要包含任何 JSON 格式之外的文字或解释**：
{
  &quot;creature_name&quot;: &quot;生物名称 (字符串)&quot;,
  &quot;habitat&quot;: &quot;主要栖息地 (字符串)&quot;,
  &quot;diet&quot;: &quot;主要食物 (字符串)&quot;,
  &quot;appearance_summary&quot;: &quot;外观简述 (字符串, 50字以内)&quot;,
  &quot;special_ability&quot;: {
    &quot;name&quot;: &quot;特殊能力名称 (字符串)&quot;,
    &quot;description&quot;: &quot;能力描述 (字符串)&quot;
  },
  &quot;friendliness_level&quot;: &quot;友好度 (整数, 1-10, 10为最友好)&quot;
}
确保输出是**单个有效**的 JSON 对象。
&quot;&quot;&quot;

user_prompt = &quot;请描述一种生活在云端、以彩虹能量为食的、闪闪发光的小精灵。&quot;

try:
    # --- 发起聊天补全请求，启用 JSON Mode ---
    response = client.chat.completions.create(
        model=&quot;gpt-4o-mini&quot;, # 确保选择支持 JSON Mode 的模型
        messages=[
            {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: system_prompt},
            {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: user_prompt}
        ],
        response_format={&quot;type&quot;: &quot;json_object&quot;}, # &lt;-- 启用 JSON Mode 的关键参数
        temperature=0.8, # 可以稍微提高一点温度增加创意性
        max_tokens=500   # 确保足够生成 JSON 内容
    )

    # --- 获取模型返回的原始字符串 ---
    json_string = response.choices[0].message.content
    print(&quot;模型返回的原始 JSON 字符串:\n&quot;, json_string)

    # --- 尝试将字符串解析为 Python 字典 ---
    try:
        creature_data = json.loads(json_string)
        print(&quot;\n成功解析为 Python 字典:&quot;)
        # 使用 json.dumps 美化打印字典
        print(json.dumps(creature_data, indent=2, ensure_ascii=False))

        # --- (可选) 访问解析后的数据 ---
        print(f&quot;\n小精灵名称: {creature_data.get(&#x27;creature_name&#x27;, &#x27;未知&#x27;)}&quot;)
        if &#x27;special_ability&#x27; in creature_data:
            print(f&quot;特殊能力: {creature_data[&#x27;special_ability&#x27;].get(&#x27;name&#x27;, &#x27;未知能力&#x27;)}&quot;)

    except json.JSONDecodeError as e:
        print(f&quot;\n错误：模型返回的不是有效的 JSON 字符串。错误信息: {e}&quot;)
        print(&quot;请检查模型是否完全遵循了 system prompt 中的格式要求，或者尝试调整 prompt。&quot;)

except BadRequestError as e:
    # 特别捕捉 BadRequestError，因为格式指令错误或模型不支持可能导致 400
    print(f&quot;\nAPI 请求错误 (BadRequest): {e}&quot;)
    print(&quot;请检查：1. 模型 (&#x27;gpt-4o-mini&#x27;) 是否支持 JSON mode。 2. Prompt 中的 JSON 描述是否清晰。&quot;)
except APIError as e:
    print(f&quot;\nAPI 调用出错: {e.status_code} - {e.message}&quot;)
except Exception as e:
    print(f&quot;\n发生意外错误: {e}&quot;)    
```

# 代码解释

1. `import json`： 我们导入了 Python 内置的 `json` 库，用于后续解析模型返回的 JSON 字符串，并能将其格式化输出。

1. `system_prompt`： 这是成功的关键。我们非常明确地告诉模型：

+   它的角色（富有想象力的生物学家）。

+   它的任务（根据用户要求创造生物描述）。

+   必须使用 JSON 格式输出。

+   提供了明确的 JSON 结构，包括字段名和预期的数据类型（以及一些约束，如字符串长度）。

+   强调只输出 JSON，没有额外文字。

1. `response_format={"type": "json_object"}`： 在 `create` 调用中，这个参数是强制启用 JSON Mode 的开关。

1. 获取原始字符串： `response.choices[0].message.content` 会包含模型生成的、理论上应该是 JSON 格式的字符串。

1. `json.loads(json_string)`： 这是核心步骤。我们尝试用 `json.loads()` 将模型返回的字符串解析成 Python 的字典（dictionary）对象。

+   如果字符串是有效的 JSON，解析成功，我们就可以像操作普通 Python 字典一样访问数据了（例如 `creature_data['creature_name']`）。

+   如果字符串不是有效的 JSON（比如多了注释、结尾有逗号、引号错误等），`json.loads()` 会抛出 `json.JSONDecodeError` 异常，我们在 except 块中捕获并提示用户。

1. 美化打印： `json.dumps(creature_data, indent=2, ensure_ascii=False)` 用于将 Python 字典转换回格式化（带缩进）的 JSON 字符串进行打印，`ensure_ascii=False` 确保中文字符能正确显示。

1. 错误处理： 除了通用的 API 错误，我们特别注意捕获 `json.JSONDecodeError` 和 `BadRequestError` （400），后者有时会在模型无法遵循 JSON 格式指令或不支持该模式时发生。

# JSON Mode 的主要应用场景

+   结构化数据提取： 从非结构化文本（如用户评论、文章）中提取关键信息（如姓名、地点、情感倾向）并以 JSON 格式返回。

+   动态生成配置文件： 根据用户需求动态生成 JSON 格式的配置文件。

+   API 集成： 让 LLM 的输出可以直接被其他需要 JSON 输入的系统或服务消费。

+   表单数据填充： 根据自然语言描述自动填充需要 JSON 数据的表单。

+   函数/工具参数构造： （与 Function Calling/Tool Use 结合） 生成调用其他 API 或 工具时所需的 JSON 格式参数。

# 注意事项

+   模型支持： 并非所有模型都支持 JSON Mode，请查阅 AOAI 的模型文档确认您选择的模型（如 `gpt-4o-mini`）是否支持。

+   Prompt 的重要性： 即使开启了 `response_format`， 模型能否成功生成所需结构的 JSON 很大程度上依赖于您在 `messages` 中给出的指令有多清晰和具体。提供清晰的字段名、类型甚至示例非常有帮助。

+   复杂性限制： 对于极其复杂的嵌套 JSON 结构，模型可能会遇到困难。保持结构相对清晰简洁有助于提高成功率。

+   Token 消耗： JSON 结构本身（括号、引号、字段名）也会消耗 Token。

# 使用工具 （Function Calling / Tool Use）

## 引言

大语言模型本身拥有庞大的知识库，但这些知识通常是截至某个训练时间点的，并且它们无法直接与外部世界进行实时交互或执行特定操作（如预订机票、查询数据库、控制智能家居）。"Function Calling" 或 "Tool Use" 功能正是为了弥补这一鸿沟。它允许您在与模型交互时，定义一组可供模型“调用”的工具（函数）。当模型认为需要使用某个工具来回答用户问题时，它不会直接回答，而是输出一个特殊的信号，表明它想调用哪个函数以及需要传入什么参数。您的应用程序接收到这个信号后，执行相应的函数，并将结果返回给模型，模型再根据这个结果生成最终的、更准确或更具操作性的回复。

## 核心概念与流程

调用工具通常涉及以下几个步骤：

1. 定义工具 （Functions/Tools）: 在向 API 发送请求时，您需要提供一个工具列表 （tools）。每个工具定义包括：

+   `type`： 通常是 `"function"`。

+   `function`： 包含函数详细信息的对象：

+   `name`： 函数的名称（例如 `get_current_weather`）。

+   `description`： 函数功能的清晰描述。这对模型决定何时使用该函数至关重要。

+   `parameters`： 一个描述该函数所需参数的 JSON Schema 对象。这定义了参数的名称、类型（如 `string`， `integer`， `boolean`）、是否必需以及描述。

1. 发起 API 请求： 将用户的消息 （messages） 和您定义的 `tools` 列表一起发送给 AOAI API。您还可以使用 `tool_choice` 参数来控制模型的行为：

+   `"auto"` （默认）: 模型自行决定是直接回答还是调用一个或多个工具。

+   `"none"`： 强制模型不调用任何工具，直接生成回复。

+   `{"type": "function", "function": {"name": "my_function"}}`： 强制模型必须调用名为 `my_function` 的特定工具。

1. 模型响应 （请求调用工具）: 如果模型决定使用工具，API 的响应中，`choices[0].message` 会包含一个 `tool_calls` 字段（而不是 `content`）。`tool_calls` 是一个列表，包含一个或多个模型想要调用的工具的信息：

+   `id`： 每个工具调用的唯一标识符 （ `tool_call_id`）。

+   `type`: `"function"`.

+   `function`:

+   `name`： 要调用的函数名称。

+   `arguments`： 一个 JSON 格式的字符串，包含了模型认为需要传递给该函数的参数。

1. 您的代码执行工具： 您的应用程序需要解析 `tool_calls`：

+   识别出要调用的函数名称 （`function.name`）。

+   解析 `function.arguments` 字符串（通常使用 `json.loads()`）。

+   根据函数名和解析出的参数，在您的代码中实际执行对应的函数/工具。例如，调用真实的第三方天气 API，或执行本地的计算函数。

+   获取函数的执行结果（返回值）。

1. 将工具结果返回给模型： 您需要再次调用 AOAI API：

+   在 `messages` 列表中追加两项：

+   第一项是上一步模型返回的包含 `tool_calls` 的原始助手消息 （`role: assistant`， `tool_calls: [...]`）。

+   第二项是一个新的消息，其 `role` 为 `tool`，包含：

+   `tool_call_id`： 对应您执行的那个工具调用的 `id`。

+   `content`： 函数执行后返回的结果（通常是字符串格式）。

+   这次调用时，不再需要传递 `tools` 参数 （因为模型现在需要基于结果来回复，而不是选择工具）。

1. 模型最终响应： 模型接收到工具执行的结果后，会结合之前的对话上下文和工具结果，生成一个最终的、自然的语言回复给用户。这个回复会在 `choices[0].message.content` 中。

## 编程实践：使用 Python 和 openai 库实现工具调用 （天气查询示例）

我们将模拟一个查询天气的功能。注意：下面的 `get_current_weather` 函数是 伪代码/模拟函数，它并不真正查询天气，只是返回一个固定的字符串，用以演示流程。在实际应用中，您需要替换它为调用真实天气服务 API 的代码。
