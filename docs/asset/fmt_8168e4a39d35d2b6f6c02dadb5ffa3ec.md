
# LiteLLM 与 ADK Agent 开发指南

## LiteLLM completion 函数

```python
extra_headers: Optional[dict] = None,
functions: Optional[List] = None,
function_call: Optional[str] = None,
base_url: Optional[str] = None,
api_version: Optional[str] = None,
api_key: Optional[str] = None,
model_list: Optional[list] = None,
thinking: Optional[litellm.types.llms.anthropic.AnthropicThinkingParam] = None,
**kwargs,
) -> Union[litellm.types.utils.ModelResponse, litellm.litellm_core_utils.streaming_handler.CustomStreamWrapper]
```

### Docstring

Perform a `completion()` using any of `litellm` supported llms (example `gpt-4`, `gpt-3.5-turbo`, `claude-2`, `command-nightly`)

### Parameters

- `model` (`str`): The name of the language model to use for text completion. see all supported LLMs: https://docs.litellm.ai/docs/providers/
- `messages` (`List`): A list of message objects representing the conversation context (default is an empty list).

#### OPTIONAL PARAMS

- `functions` (`List`, optional): A list of functions to apply to the conversation messages (default is an empty list).
- `function_call` (`str`, optional): The name of the function to call within the conversation (default is an empty string).
- `temperature` (`float`, optional): The temperature parameter for controlling the randomness of the output (default is 1.0).
- `top_p` (`float`, optional): The top-p parameter for nucleus sampling (default is 1.0).
- `n` (`int`, optional): The number of completions to generate (default is 1).
- `stream` (`bool`, optional): If True, return a streaming response (default is False).
- `stream_options` (`dict`, optional): A dictionary containing options for the streaming response. Only set this when you set `stream: true`.
- `stop` (`string`/`list`, optional): Up to 4 sequences where the LLM API will stop generating further tokens.
- `max_tokens` (`integer`, optional): The maximum number of tokens in the generated completion (default is infinity).
- `max_completion_tokens` (`integer`, optional): An upper bound for the number of tokens that can be generated for a completion, including visible output tokens and reasoning tokens.
- `modalities` (`List[ChatCompletionModality]`, optional): Output types that you would like the model to generate for this request. You can use `["text", "audio"]`
- `prediction` (`ChatCompletionPredictionContentParam`, optional): Configuration for a Predicted Output, which can greatly improve response times when large parts of the model response are known ahead of time. This is most common when you are regenerating a file with only minor changes to most of the content.
- `audio` (`ChatCompletionAudioParam`, optional): Parameters for audio output. Required when audio output is requested with modalities: `["audio"]`
- `presence_penalty` (`float`, optional): It is used to penalize new tokens based on their existence in the text so far.
- `frequency_penalty`: It is used to penalize new tokens based on their frequency in the text so far.
- `logit_bias` (`dict`, optional): Used to modify the probability of specific tokens appearing in the completion.
- `user` (`str`, optional): A unique identifier representing your end-user. This can help the LLM provider to monitor and detect abuse.
- `logprobs` (`bool`, optional): Whether to return log probabilities of the output tokens or not. If true, returns the log probabilities of each output token returned in the content of message
- `top_logprobs` (`int`, optional): An integer between 0 and 5 specifying the number of most likely tokens to return at each token position, each with an associated log probability. `logprobs` must be set to true if this parameter is used.
- `metadata` (`dict`, optional): Pass in additional metadata to tag your completion calls - eg. prompt version, details, etc.
- `api_base` (`str`, optional): Base URL for the API (default is None).
- `api_version` (`str`, optional): API version (default is None).
- `api_key` (`str`, optional): API key (default is None).
- `model_list` (`list`, optional): List of api base, version, keys
- `extra_headers` (`dict`, optional): Additional headers to include in the request.

#### LITELLM Specific Params

- `mock_response` (`str`, optional): If provided, return a mock completion response for testing or debugging purposes (default is None).
- `custom_llm_provider` (`str`, optional): Used for Non-OpenAI LLMs, Example usage for bedrock, set `model="amazon.titan-tg1-large"` and `custom_llm_provider="bedrock"`
- `max_retries` (`int`, optional): The number of retries to attempt (default is 0).

### Returns

- `ModelResponse`: A response object containing the generated completion and associated metadata.

### Note

- This function is used to perform `completions()` using the specified language model.
- It supports various optional parameters for customizing the completion behavior.
- If `mock_response` is provided, a mock completion response is returned for testing or debugging.

File: `c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\litellm\main.py`

Type: `function`

## 配置 DeepSeek API 凭据

```python
api_base = "https://api.deepseek.com"
api_key = "YOUR_DEEPSEEK_API_KEY"
```

## 使用 LiteLLM 调用 DeepSeek 模型

```python
response = completion(
    model="deepseek/deepseek-chat",
    messages=[{"role": "user", "content": "你好，好久不见！"}],
    api_base=api_base,
    api_key=api_key
)
```

```python
print(response)
```

```text
ModelResponse(id='1aeee833-ee0e-48e8-a540-629e9aabd3ac', created=1745740153, model='deepseek-chat', object='chat.completion', system_fingerprint='fp_8802369eaa_prod0425fp8', choices=[Choices(finish_reason='stop', index=0, message=Message(content='你好呀！确实好久不见～最近过得怎么样？有什么新鲜事想分享，或者需要帮忙的地方吗？我随时在这里哦！😊  \n\n（如果这时候你突然想聊天气、电影、或者突然冒出的奇怪问题……我也完全接得住！）', role='assistant', tool_calls=None, function_call=None, provider_specific_fields=None))], usage=Usage(completion_tokens=55, prompt_tokens=8, total_tokens=63, completion_tokens_details=None, prompt_tokens_details=PromptTokensDetailsWrapper(audio_tokens=None, cached_tokens=0, text_tokens=None, image_tokens=None), prompt_cache_hit_tokens=0, prompt_cache_miss_tokens=8))
```

```python
print(response.choices[0].message.content)
```

```
你好呀！确实好久不见～最近过得怎么样？有什么新鲜事想分享，或者需要帮忙的地方吗？我随时在这里哦！😊  

（如果这时候你突然想聊天气、电影、或者突然冒出的奇怪问题……我也完全接得住！）
```

## 使用 Google ADK 构建 Agent

使用 Gooogle ADK 构建 Agent 其基本的实现形式如下所示。这里我们使用 `litellm` 接入 DeepSeek 模型，注意需要在同级目录下创建 `.env` 文件，并配置 `DS_API_KEY` 和 `DS_BASE_URL`。同时，对于 Agent 的构建，我们使用最简模式，即只设置 `name` 和 `model`。（我们在下一小节课程再详细介绍 Agent 组件的高阶应用）

```python
from google.adk.agents import Agent
from google.adk.models.lite_llm import LiteLlm
import os
from dotenv import load_dotenv

load_dotenv(override=True)

DS_API_KEY = os.getenv("DS_API_KEY")
DS_BASE_URL = os.getenv("DS_BASE_URL")

# 1. 通过 litellm 接入 DeepSeek 模型
model = LiteLlm(
    model="deepseek/deepseek-reasoner",
    api_base=DS_BASE_URL,
    api_key=DS_API_KEY
)

# 2. 使用最简模式构建一个 Agent 代理
init_agent = Agent(
    name="chatbot",
    model=model,
    instruction="你是一个乐于助人的中文助手。",
)
```

![](img/8b06b30dfc025dfafcebe9b3d9ab0889.png)

![](img/326487f5f6b7d36a5f6a91f80c6ffbf5.png)

ADK 作为最新一代 Agent 开发框架，不仅功能特性非常领先，而且还内置了非常多的工具，包括 Gemini 系列模型自带的谷歌搜索、文件检索和代码解释器等功能，同时 ADK 还拥有内置对话前端，方便开发者更加直观的感受 Agent 的对话流程，并且实时追踪 Agent 的 `events` 流。而如果 Agent 中存在外部工具调用，则在内置前端中，还能进一步观察到 Agent 调用外部工具完整流程，以及各环节调用信息，方便开发者即时 `debug`。如果是使用 Gemini 模型，这个前端还支持语音和视频实时交互，基本展示效果如下：

## 6.3 ADK 内置前端开启流程

而 ADK 内置前端开启流程也非常简单，这里我们需要创建一个项目文件，如创建一个 `ADK_Chat` 文件夹：
然后创建基本文件结构：（注意，如果是 Windows 系统，官方推荐手动创建项目文件基本结构，而不是采用 IDE 如 Cursor 等进行创建，以避免出现字符乱码情况）

```text
ADK_Chat/
├── test_agent/
│   ├── agent.py
│   ├── __init__.py
│   ├── .env
├── __init__.py
└── ...
```
