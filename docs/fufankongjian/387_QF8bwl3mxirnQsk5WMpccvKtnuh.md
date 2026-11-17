# Part 1.ADK快速入门调用

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/OrlEd6Sgso1Tt4xKQmJcqeuSnld](https://kq4b3vgg5b.feishu.cn/docx/OrlEd6Sgso1Tt4xKQmJcqeuSnld)

## 部分项目成果演示⬇️

```
from IPython.display import Video
```

+   **MateGen项目演示**

```
Video(&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/4.MateGen%20Pro%20%E9%A1%B9%E7%9B%AE%E5%8A%9F%E8%83%BD%E6%BC%94%E7%A4%BA.mp4&quot;, width=800, height=400)
```

+   **智能客服项目演示**

```
Video(&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/%E6%99%BA%E8%83%BD%E5%AE%A2%E6%9C%8D%E6%A1%88%E4%BE%8B%E6%BC%94%E7%A4%BA.mp4&quot;, width=800, height=400)
```

+   **Dify项目演示**

```
Video(&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/2f1b47f42c65fd59e8d3a83e6cb9f13b_raw.mp4&quot;, width=800, height=400)
```

+   LangChain&LangGraph搭建Multi-Agnet

```
Video(&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/%E5%8F%AF%E8%A7%86%E5%8C%96%E6%95%B0%E6%8D%AE%E5%88%86%E6%9E%90Multi-Agent%E6%95%88%E6%9E%9C%E6%BC%94%E7%A4%BA%E6%95%88%E6%9E%9C.mp4&quot;, width=800, height=400)
```

---

## 《2025大模型Agent智能体开发实战》体验课

## 谷歌开源Agent框架ADK快速入门调用指南

## 一、谷歌 Agent Development Kit (ADK) 框架功能特性详解

谷歌 Agent Development Kit (ADK) 是 Google 在 2025 年发布的开源 AI 多代理开发框架。它旨在让开发者像进行软件开发一样来创建和部署自主智能体系统，使构建从简单任务到复杂工作流的多代理应用更为容易。ADK 同时也是支撑 Google 内部 Agentspace 和客户互动套件（CES）代理的底层框架，并已通过开源惠及社区。下面我们从架构设计、API 风格、扩展性、安全性、任务管理、多代理协调等方面，对 ADK 的框架特性进行详细说明。

## 1.架构设计与多代理协作

ADK 天生支持多代理 (multi-agent) 架构，在设计上鼓励**将应用拆分为多个专职**的代理，并通过层次化的管理和协作来完成复**杂任****务。典型模式是**采用层次化的主从代理模型：可以定义一个主代**理（**如“调**度/主管****”代理）****负责统筹，根据用**户请求的意图将子任务委**派给不同功能特长的子代理处理。**这**种设计允许代理间自动分工与**协调：在处理用户消息时，主代理的底层大模型会参考自己及子代理的能力描述，判断是否将工作转交（handoff）给某个子代理。例如，一个 WeatherAgent 主管可根据用户输入动态决定是自己调用天气查询工具回答天气问题，还是将对话移交给 GreetingAgent 来处理问候语。为此，每个代理在定义时都需要提供清晰的角色说明和能力描述，以帮助大模型决策职责路由。这种通过 LLM 推断驱动的代理转移机制使 ADK 的多代理系统更具自适应性，能够根据对话内容灵活调用最合适的代理处理。除此之外，ADK 还支持多代理并行和循环等复杂编排模式：内置了顺序 (Sequential)、并行 (Parallel)、**循环 (Loo...

## 2.模型与工具生态的支持

**模型支持方面**，ADK 采用**模型无关、插件式**设计。虽然针对 Goo**gle 自家**的 Gemini 大**模型和 **V**ertex**** AI 平台**做了优化，但并不局限于此。通过内**置的 LiteLLM 适配**层，ADK 可以**无缝对接 Ve****rtex AI **模型园中各种模型，甚至包括 Anthropic、Meta、Mistral、AI21 等多家提供商的大模型。这意味着企业可根据需求选择最适合的模型或使用私有模型，而无需更改 ADK 的使用方式，避免供应商锁定。工具扩展方面，ADK 拥有丰富的内置工具和第三方工具整合能力。框架自带了诸如网络搜索、代码执行等常用工具模块，并通过开放 API支持开发者注册任意 Python 函数作为自定义工具。ADK 特别提供了模型上下文协议 (MCP) 工具接口和对 OpenAPI 服务的集成，方便调用外部系统。ADK 还能够把其他代理作为工具使用：例如可以将一个由 LangChain 或 CrewAI 构建的代理包装为 ADK 工具供另一个代理调用，从而充分利用社区已有成果。通过这种插件化设计，ADK 的功能可**无限扩...

+   ADK项目主页：https://github.com/google/adk-python

![](img/b1722a3b2b2e8de92dabf918788e973e.png)

+   ADK项目说明文档：https://google.github.io/adk-docs/

![](img/7cd3dd2591011899606c76b36060f2dd.png)

## 二、LiteLlm快速使用指南

在介绍ADK之前，我们需要系统介绍ADK默认的大模型调用工具——LiteLlm的使用方法。在后续使用ADK时，选择基础模型是最基础的环节，而ADK目前只支持使用LiteLlm进行模型调用，因此我们先详细介绍国内外在线模型、以及ollama和vLLM驱动下的开源模型如何接入LiteLlm，然后再讲解如何上手使用ADK。

### 1.**LiteLLM 项目概述**

+   项目官网：https://github.com/BerriAI/litellm

![](img/02e5b13f11b3118d952dc865aeb79a60.png)

+   项目参考文档：https://docs.litellm.ai/docs/

![](img/69efe60e6a856c4a9b11725576fa8536.png)

**LiteLLM** 是一个轻量级的 Python 库，旨在简化与各种大语言模型（LLM）进行交互的过程。它特别适用于开发者在多种云服务提供商（如 OpenAI、DeepSeek）**或本地部署模型（如 Ollam**a）上运行大语言模型的需求。通过提供简洁的 API 和灵活的配置选项，LiteLLM 使得开发者可以轻松调用和集成各种不同的 LLM，同时支持模型的定制化扩展和高效调度。LiteLLM 核心功能如下：

+   **跨平台模型兼容性**：LiteLLM 使开发者能够通过统一接口调用多个大语言模型，无论是云端提供的 API（如 OpenAI）还是本地托管的模型（如 Ollama）。它内置支持常见的大型模型和多模型集成，如 GPT-3.5、GPT-4、DeepSeek 等，也能支持定制模型或自己托管的 LLM。

+   **简化的 API 调用**：LiteLLM 提供了一个简单直观的接口，使得开发者无需深入了解底层 API 细节即可实现与模型的交互。用户只需关注模型名称和输入输出内容，LiteLLM 会自动处理与模型的连接、API 调用以及响应解析工作。

+   **多模型支持**：LiteLLM 支持通过配置文件和代码直接切换不同的模型。它支持从 OpenAI、Anthropic、Meta、DeepSeek 等多个模型提供商调用，还允许开发者轻松集成自定义模型。例如，您可以在一个项目中同时使用 OpenAI 的 GPT 模型和 DeepSeek 的推理模型，而无需额外的集成工作。

+   **灵活的代理支持**：LiteLLM 可以配置为代理模型的 API，通过代理层进行调用，支持本地化部署和代理服务，例如与 Ollama 等工具结合，运行本地大语言模型。这使得 LiteLLM 在需要进行模型切换或构建跨平台服务时，能够提供极大的灵活性。

+   自定义 API 路由（Base URL）：LiteLLM 允许用户设置自定义的 base_url，以支持通过代理或内部 API 调用模型。这为需要在多个环境中运行或通过私有部署调用 LLM 的企业应用提供了便利。

+   **流式响应与事件处理**：LiteLLM 支持流式响应（streaming responses），允许开发者在模型生成回复的同时进行处理。通过这种方式，开发者能够实时接收和处理模型输出，适用于需要低延迟响应的应用场景，如实时对话或多轮交互。

+   **工具和任务管理**：在集成外部工具（如搜索引擎、数据库查询等）时，LiteLLM 通过简洁的配置支持多任务执行。开发者可以将外部工具与模型结合，形成复杂的自动化任务流，例如通过模型生成的查询请求，调用外部 API 完成任务并返回结果。

+   **灵活的配置和扩展性**：LiteLLM 提供了多种配置选项，允许开发者根据具体需求调优模型的行为。用户可以通过传入配置文件来调整模型的推理设置、响应格式、温度参数等，此外，LiteLLM 还支持通过插件扩展其功能，适应更多的使用场景。

不难看出LiteLLM 是一款强大的模型调用工具，它的核心优势在于跨平台支持、简洁的 API 和灵活的配置选项。无论是调用云端大模型，还是集成本地托管的模型，LiteLLM 都能提供高效且简化的解决方案，使得开发者能够快速集成并灵活调度大语言模型。它特别适用于需要多模型集成、代理服务、任务管理和流式响应的场景，具有极高的可扩展性和适应性。通过 LiteLLM，开发者可以专注于业务逻辑和任务设计，而将模型调用、数据处理和通信细节交给框架来处理，从而大大提高开发效率并降低技术门槛。

### 2.LiteLlm安装部署调用流程

我们可以使用如下命令进行安装

```
pip install litellm==1.67.2
```

这里需要注意的是，尽管1.67.2不是最新LiteLlm稳定版，但却是可以最好的和ADK兼容的版本，目前最新的LiteLlm库在使用ADK的时候会报错，具体问题详见：https://github.com/BerriAI/litellm/issues/10353 。因此公开课中统一使用1.67.2版本。

![](img/157b90ee22d44a6ce6ba5ea71f80b225.png)

安装完成后即可尝试调用DeepSeek模型在线API进行测试。

```
from litellm import completion
```

```
completion?
```

```
[31mSignature:[39m
completion(
    model: str,
    messages: List = [],
    timeout: Union[float, str, openai.Timeout, NoneType] = [38;5;28;01mNone[39;00m,
    temperature: Optional[float] = [38;5;28;01mNone[39;00m,
    top_p: Optional[float] = [38;5;28;01mNone[39;00m,
    n: Optional[int] = [38;5;28;01mNone[39;00m,
    stream: Optional[bool] = [38;5;28;01mNone[39;00m,
    stream_options: Optional[dict] = [38;5;28;01mNone[39;00m,
    stop=[38;5;28;01mNone[39;00m,
    max_completion_tokens: Optional[int] = [38;5;28;01mNone[39;00m,
    max_tokens: Optional[int] = [38;5;28;01mNone[39;00m,
    modalities: Optional[List[Literal[[33m&#x27;text&#x27;[39m, [33m&#x27;audio&#x27;[39m]]] = [38;5;28;01mNone[39;00m,
    prediction: Optional[openai.types.chat.chat_completion_prediction_content_param.ChatCompletionPredictionContentParam] = [38;5;28;01mNone[39;00m,
    audio: Optional[openai.types.chat.chat_completion_audio_param.ChatCompletionAudioParam] = [38;5;28;01mNone[39;00m,
    presence_penalty: Optional[float] = [38;5;28;01mNone[39;00m,
    frequency_penalty: Optional[float] = [38;5;28;01mNone[39;00m,
    logit_bias: Optional[dict] = [38;5;28;01mNone[39;00m,
    user: Optional[str] = [38;5;28;01mNone[39;00m,
    reasoning_effort: Optional[Literal[[33m&#x27;low&#x27;[39m, [33m&#x27;medium&#x27;[39m, [33m&#x27;high&#x27;[39m]] = [38;5;28;01mNone[39;00m,
    response_format: Union[dict, Type[pydantic.main.BaseModel], NoneType] = [38;5;28;01mNone[39;00m,
    seed: Optional[int] = [38;5;28;01mNone[39;00m,
    tools: Optional[List] = [38;5;28;01mNone[39;00m,
    tool_choice: Union[str, dict, NoneType] = [38;5;28;01mNone[39;00m,
    logprobs: Optional[bool] = [38;5;28;01mNone[39;00m,
    top_logprobs: Optional[int] = [38;5;28;01mNone[39;00m,
    parallel_tool_calls: Optional[bool] = [38;5;28;01mNone[39;00m,
    deployment_id=[38;5;28;01mNone[39;00m,
    extra_headers: Optional[dict] = [38;5;28;01mNone[39;00m,
    functions: Optional[List] = [38;5;28;01mNone[39;00m,
    function_call: Optional[str] = [38;5;28;01mNone[39;00m,
    base_url: Optional[str] = [38;5;28;01mNone[39;00m,
    api_version: Optional[str] = [38;5;28;01mNone[39;00m,
    api_key: Optional[str] = [38;5;28;01mNone[39;00m,
    model_list: Optional[list] = [38;5;28;01mNone[39;00m,
    thinking: Optional[litellm.types.llms.anthropic.AnthropicThinkingParam] = [38;5;28;01mNone[39;00m,
    **kwargs,
) -&gt; Union[litellm.types.utils.ModelResponse, litellm.litellm_core_utils.streaming_handler.CustomStreamWrapper]
[31mDocstring:[39m
Perform a completion() using any of litellm supported llms (example gpt-4, gpt-3.5-turbo, claude-2, command-nightly)
Parameters:
    model (str): The name of the language model to use for text completion. see all supported LLMs: https://docs.litellm.ai/docs/providers/
    messages (List): A list of message objects representing the conversation context (default is an empty list).

    OPTIONAL PARAMS
    functions (List, optional): A list of functions to apply to the conversation messages (default is an empty list).
    function_call (str, optional): The name of the function to call within the conversation (default is an empty string).
    temperature (float, optional): The temperature parameter for controlling the randomness of the output (default is 1.0).
    top_p (float, optional): The top-p parameter for nucleus sampling (default is 1.0).
    n (int, optional): The number of completions to generate (default is 1).
    stream (bool, optional): If True, return a streaming response (default is False).
    stream_options (dict, optional): A dictionary containing options for the streaming response. Only set this when you set stream: true.
    stop(string/list, optional): - Up to 4 sequences where the LLM API will stop generating further tokens.
    max_tokens (integer, optional): The maximum number of tokens in the generated completion (default is infinity).
    max_completion_tokens (integer, optional): An upper bound for the number of tokens that can be generated for a completion, including visible output tokens and reasoning tokens.
    modalities (List[ChatCompletionModality], optional): Output types that you would like the model to generate for this request.. You can use `[&quot;text&quot;, &quot;audio&quot;]`
    prediction (ChatCompletionPredictionContentParam, optional): Configuration for a Predicted Output, which can greatly improve response times when large parts of the model response are known ahead of time. This is most common when you are regenerating a file with only minor changes to most of the content.
    audio (ChatCompletionAudioParam, optional): Parameters for audio output. Required when audio output is requested with modalities: [&quot;audio&quot;]
    presence_penalty (float, optional): It is used to penalize new tokens based on their existence in the text so far.
    frequency_penalty: It is used to penalize new tokens based on their frequency in the text so far.
    logit_bias (dict, optional): Used to modify the probability of specific tokens appearing in the completion.
    user (str, optional):  A unique identifier representing your end-user. This can help the LLM provider to monitor and detect abuse.
    logprobs (bool, optional): Whether to return log probabilities of the output tokens or not. If true, returns the log probabilities of each output token returned in the content of message
    top_logprobs (int, optional): An integer between 0 and 5 specifying the number of most likely tokens to return at each token position, each with an associated log probability. logprobs must be set to true if this parameter is used.
    metadata (dict, optional): Pass in additional metadata to tag your completion calls - eg. prompt version, details, etc.
    api_base (str, optional): Base URL for the API (default is None).
    api_version (str, optional): API version (default is None).
    api_key (str, optional): API key (default is None).
    model_list (list, optional): List of api base, version, keys
    extra_headers (dict, optional): Additional headers to include in the request.

    LITELLM Specific Params
    mock_response (str, optional): If provided, return a mock completion response for testing or debugging purposes (default is None).
    custom_llm_provider (str, optional): Used for Non-OpenAI LLMs, Example usage for bedrock, set model=&quot;amazon.titan-tg1-large&quot; and custom_llm_provider=&quot;bedrock&quot;
    max_retries (int, optional): The number of retries to attempt (default is 0).
Returns:
    ModelResponse: A response object containing the generated completion and associated metadata.

Note:
    - This function is used to perform completions() using the specified language model.
    - It supports various optional parameters for customizing the completion behavior.
    - If &#x27;mock_response&#x27; is provided, a mock completion response is returned for testing or debugging.
[31mFile:[39m      c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\litellm\main.py
[31mType:[39m      function
```

```
api_base = &quot;https://api.deepseek.com&quot;
api_key = &quot;YOUR_DEEPSEEK_API_KEY&quot;
```

```
# 使用 LiteLLM 调用 DeepSeek 模型
response = completion(
    model=&quot;deepseek/deepseek-chat&quot;,  
    messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，好久不见！&quot;}],
    api_base=api_base,
    api_key=api_key
)
```

```
print(response)
```

```
ModelResponse(id=&#x27;1aeee833-ee0e-48e8-a540-629e9aabd3ac&#x27;, created=1745740153, model=&#x27;deepseek-chat&#x27;, object=&#x27;chat.completion&#x27;, system_fingerprint=&#x27;fp_8802369eaa_prod0425fp8&#x27;, choices=[Choices(finish_reason=&#x27;stop&#x27;, index=0, message=Message(content=&#x27;你好呀！确实好久不见～最近过得怎么样？有什么新鲜事想分享，或者需要帮忙的地方吗？我随时在这里哦！😊  \n\n（如果这时候你突然想聊天气、电影、或者突然冒出的奇怪问题……我也完全接得住！）&#x27;, role=&#x27;assistant&#x27;, tool_calls=None, function_call=None, provider_specific_fields=None))], usage=Usage(completion_tokens=55, prompt_tokens=8, total_tokens=63, completion_tokens_details=None, prompt_tokens_details=PromptTokensDetailsWrapper(audio_tokens=None, cached_tokens=0, text_tokens=None, image_tokens=None), prompt_cache_hit_tokens=0, prompt_cache_miss_tokens=8))
```

```
print(response.choices[0].message.content)
```

```
你好呀！确实好久不见～最近过得怎么样？有什么新鲜事想分享，或者需要帮忙的地方吗？我随时在这里哦！😊  

（如果这时候你突然想聊天气、电影、或者突然冒出的奇怪问题……我也完全接得住！）
```

由于后续我们主要借助ADK来调用LiteLlm，因此LiteLlm更多进阶功能就暂不介绍了，大家感兴趣的话可以参考官方说明文档。

### 3.各类大模型接入LiteLlm流程

接下来我们尝试使用LiteLlm调用更多模型，以及本地ollama、vLLM驱动的开源模型。

+   多模型API-KEY及Base_URL管理方法

在实际开发过程中，如果涉及到多个模型调用，可以考虑在当前项目文件内创建.env文件，用于保存不同模型的API-KEY和调用请求地址Base_URL，例如

![](img/a02b0837679dc4fcb980126566587100.png)

然后写入如下内容：

![](img/541bb2315751f5cee03e653e84c7eb61.png)

也就是DeepSeek、OpenAI、Gemini等模型的API-KEY和Base_URL，之后即可使用如下方式进行导入：

```
import os
from dotenv import load_dotenv
load_dotenv(override=True)
```

```
True
```

```
# 读取模型们的API-KEY
DS_API_KEY = os.getenv(&quot;DS_API_KEY&quot;)
OPENAI_API_KEY = os.getenv(&quot;OPENAI_API_KEY&quot;)
GEMINI_API_KEY = os.getenv(&quot;GEMINI_API_KEY&quot;)
```

```
# 读取模型们的BASE_URL
DS_BASE_URL = os.getenv(&quot;DS_BASE_URL&quot;)
OPENAI_BASE_URL = os.getenv(&quot;OPENAI_BASE_URL&quot;)
GEMINI_BASE_URL = os.getenv(&quot;GEMINI_BASE_URL&quot;)
```

+   OpenAI/GPT-4.1模型调用流程

接下来测试GPT-4.1模型在LiteLlm中调用流程，在LiteLlm统一的模型调用框架下，同样也是只需要输入api_key和base_url即可调用:

```
openai_response = completion(
    model=&quot;openai/gpt-4.1&quot;,  
    messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，好久不见！&quot;}],
    api_base=OPENAI_BASE_URL,
    api_key=OPENAI_API_KEY
)
```

```
print(openai_response.choices[0].message.content)
```

```
你好！很高兴再次见到你，有什么可以帮忙的吗？
```

反向代理地址：

+   **ollama+开源模型调用流程**

接下来继续测试使用LiteLlm调用本地开源模型，例如以Gemma-27B模型为例，首先需要确认模型已经下载并可以在ollama中顺利调用：

![](img/9d3bd1a9382e41c66bc76f1798b91be4.png)

![](img/cdc71b3d74cad447b7c04c1fc3a42482.png)

然后测试使用LiteLlm进行调用：

```
# 使用 LiteLLM 调用 Ollama 模型
ollama_response = completion(
    model=&quot;ollama/gemma3:27b&quot;,  # Ollama 模型
    messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，好久不见！&quot;}],
    api_base=&quot;http://localhost:11434&quot;  # 本地 Ollama API
)
```

```
print(ollama_response.choices[0].message.content)
```

```
你好！我很好，很高兴和你聊天。你今天感觉怎么样？有什么想聊的话题吗？
```

+   **vllm+开源模型调用流程**

最后尝试使用vLLM调用本地QwQ-32B模型，并接入LiteLlm。首先需要启动vLLM：

![](img/48cee6304bfb784d1bdeb87be0f96433.png)

然后输入如下代码进行测试：

```
# 使用 LiteLLM 调用 vLLM 模型
vllm_response = completion(
    model=&quot;vllm/QwQ-32B&quot;,  # vLLM 模型
    messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，好久不见！请介绍下你自己。&quot;}],
    api_base=&quot;http://localhost:8000&quot;  # 本地 vLLM API
)
```

```
print(vllm_response.choices[0].message.content)
```

```
你好！真的好久不见啦，很高兴又和你聊天。最近怎么样？有什么新鲜事吗？ 😊
```

能够顺利接入各类模型后，接下来即可进一步使用ADK来创建Agent了。

---

### 更多LiteLlm使用方法详见正课

**LiteLlm还支持独立设置代理地址，借助国内中转或反向代理方法，即可零网络门槛使用GPT、Claude和****Gemini等模型，上手全球顶尖大模型&完**整使用ADK Web Dev UI的实时语音和视频交互等功能，更多进阶方法详见《2025大模型Agent智能体开发实战》(5月班)付费课程。详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/7796990f1ede1cc360fddf53326ee3ac.png)

![](img/b44a69b27e3a0cc731fa95c2572b2a1a.png)

---

### 三、ADK（Agent Development Kit）调用流程详解

+   ADK项目主页：https://github.com/google/adk-python

![](img/4fe3753204f6036dd8db794c58fcc84e.png)

+   ADK项目说明文档：https://google.github.io/adk-docs/

![](img/1585f084cd7c6f08e9059c44aac49197.png)

目前ADK处于快速迭代中，我们使用如下命令即可安装最新稳定版：

```
pip install google-adk 
```

然后导入下列基础库：

```
import os
import asyncio
from google.adk.agents import Agent
from google.adk.models.lite_llm import LiteLlm 
from google.adk.sessions import InMemorySessionService
from google.adk.runners import Runner
from google.genai import types 
```

接下来即可简单进行测试和调用。作为工业级Agent开发框架，ADK的一个基本调用流程如下：

### **Step 1.配置模型**

首先我们需要创建一个模型，也被称作ADK的模型调用适配器，这里需要采用ADK中封装的LiteLlm库来完成，需要注意的是，这个适配器负责配置好 model、认证、连接参数，然后统一管理模型调用接口。

```
LiteLlm?
```

```
[31mInit signature:[39m
LiteLlm(
    model: str,
    *,
    llm_client: google.adk.models.lite_llm.LiteLLMClient = &lt;factory&gt;,
) -&gt; [38;5;28;01mNone[39;00m
[31mDocstring:[39m     
Wrapper around litellm.

This wrapper can be used with any of the models supported by litellm. The
environment variable(s) needed for authenticating with the model endpoint must
be set prior to instantiating this class.

Example usage:
```
os.environ[&quot;VERTEXAI_PROJECT&quot;] = &quot;your-gcp-project-id&quot;
os.environ[&quot;VERTEXAI_LOCATION&quot;] = &quot;your-gcp-location&quot;

agent = Agent(
    model=LiteLlm(model=&quot;vertex_ai/claude-3-7-sonnet@20250219&quot;),
    ...
)
```

Attributes:
  model: The name of the LiteLlm model.
  llm_client: The LLM client to use for the model.
  model_config: The model config.
[31mInit docstring:[39m
Initializes the LiteLlm class.

Args:
  model: The name of the LiteLlm model.
  **kwargs: Additional arguments to pass to the litellm completion api.
[31mFile:[39m           c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\google\adk\models\lite_llm.py
[31mType:[39m           ModelMetaclass
[31mSubclasses:[39m     
```

其参数解释如下：

| 参数名 | 类型 | 是否必填 | 说明 |
| --- | --- | --- | --- |
| model | str 字符串 | ✅ 必填 | 指定要调用的模型名称（例如 openai/gpt-4o, deepseek/deepseek-chat, ollama/qwen2.5） |
| llm_client | LiteLLMClient对象 | ❎ 可选 | 默认自动创建一个内部的客户端；高级用法允许你传自己定制的客户端（一般开发初期用不到） |

我们可以采用如下方法，创建一个ADK能够识别的DeepSeek模型：

```
model = LiteLlm(
    model=&quot;deepseek/deepseek-chat&quot;,  
    api_base=DS_BASE_URL,
    api_key=DS_API_KEY
)
```

需要注意的是，适配器创建模型的流程和使用completion函数调用模型类似，模型名称和参数都要相互匹配。

### Step 2.定义一个极简Agent

接下来创建一个Agent对象。和一般Multi Agent框架类似，ADK也是通过Agent类来创建一个个能够独立完成一些功能的代理，而在实际工作中，会通过创建多个代理来协同完成复杂任务。

Agent是 Google ADK 中管理 LLM推理+工具使用+多Agent协作的标准组件，其功能概述如下：

+   封装一个模型（例如 LiteLlm 连接的模型）

+   定义这个Agent的**指令说明**（Instruction）

+   注册可以调用的**工具（Tools）**

+   支持**子Agent管理**（比如自己不能完成就转交子Agent）

+   支持**调用前后回调**（比如修改请求、监控日志）

+   支持**输入输出Schema校验**（结构化输入输出）

+   可以**嵌套在更大系统中**（比如成为子Agent）

```
Agent?
```

```
[31mInit signature:[39m
Agent(
    *,
    name: str,
    description: str = [33m&#x27;&#x27;[39m,
    parent_agent: Optional[google.adk.agents.base_agent.BaseAgent] = [38;5;28;01mNone[39;00m,
    sub_agents: list[google.adk.agents.base_agent.BaseAgent] = &lt;factory&gt;,
    before_agent_callback: Optional[Callable[[google.adk.agents.callback_context.CallbackContext], Optional[google.genai.types.Content]]] = [38;5;28;01mNone[39;00m,
    after_agent_callback: Optional[Callable[[google.adk.agents.callback_context.CallbackContext], Optional[google.genai.types.Content]]] = [38;5;28;01mNone[39;00m,
    model: Union[str, google.adk.models.base_llm.BaseLlm] = [33m&#x27;&#x27;[39m,
    instruction: Union[str, Callable[[google.adk.agents.readonly_context.ReadonlyContext], str]] = [33m&#x27;&#x27;[39m,
    global_instruction: Union[str, Callable[[google.adk.agents.readonly_context.ReadonlyContext], str]] = [33m&#x27;&#x27;[39m,
    tools: list[typing.Union[typing.Callable, google.adk.tools.base_tool.BaseTool]] = &lt;factory&gt;,
    generate_content_config: Optional[google.genai.types.GenerateContentConfig] = [38;5;28;01mNone[39;00m,
    disallow_transfer_to_parent: bool = [38;5;28;01mFalse[39;00m,
    disallow_transfer_to_peers: bool = [38;5;28;01mFalse[39;00m,
    include_contents: Literal[[33m&#x27;default&#x27;[39m, [33m&#x27;none&#x27;[39m] = [33m&#x27;default&#x27;[39m,
    input_schema: Optional[type[pydantic.main.BaseModel]] = [38;5;28;01mNone[39;00m,
    output_schema: Optional[type[pydantic.main.BaseModel]] = [38;5;28;01mNone[39;00m,
    output_key: Optional[str] = [38;5;28;01mNone[39;00m,
    planner: Optional[google.adk.planners.base_planner.BasePlanner] = [38;5;28;01mNone[39;00m,
    code_executor: Optional[google.adk.code_executors.base_code_executor.BaseCodeExecutor] = [38;5;28;01mNone[39;00m,
    examples: Union[list[google.adk.examples.example.Example], google.adk.examples.base_example_provider.BaseExampleProvider, NoneType] = [38;5;28;01mNone[39;00m,
    before_model_callback: Optional[Callable[[google.adk.agents.callback_context.CallbackContext, google.adk.models.llm_request.LlmRequest], Optional[google.adk.models.llm_response.LlmResponse]]] = [38;5;28;01mNone[39;00m,
    after_model_callback: Optional[Callable[[google.adk.agents.callback_context.CallbackContext, google.adk.models.llm_response.LlmResponse], Optional[google.adk.models.llm_response.LlmResponse]]] = [38;5;28;01mNone[39;00m,
    before_tool_callback: Optional[Callable[[google.adk.tools.base_tool.BaseTool, dict[str, Any], google.adk.tools.tool_context.ToolContext], Optional[dict]]] = [38;5;28;01mNone[39;00m,
    after_tool_callback: Optional[Callable[[google.adk.tools.base_tool.BaseTool, dict[str, Any], google.adk.tools.tool_context.ToolContext, dict], Optional[dict]]] = [38;5;28;01mNone[39;00m,
) -&gt; [38;5;28;01mNone[39;00m
[31mDocstring:[39m      LLM-based Agent.
[31mInit docstring:[39m
Create a new model by parsing and validating input data from keyword arguments.

Raises [`ValidationError`][pydantic_core.ValidationError] if the input data cannot be
validated to form a valid model.

`self` is explicitly positional-only to allow `self` as a field name.
[31mFile:[39m           c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\google\adk\agents\llm_agent.py
[31mType:[39m           ModelMetaclass
[31mSubclasses:[39m     
```

其核心参数解释如下：

| 参数名 | 类型 | 说明 | 中文解释 |
| --- | --- | --- | --- |
| name | str | Agent的名称，必须唯一 | 代理的名字（必填） |
| description | str | Agent的简短描述，通常在多Agent系统中用于路由/选择 | 代理的简短介绍 |
| parent_agent | BaseAgent or None | 父Agent（如果这个Agent是子代理的话） | 父代理，默认无 |
| sub_agents | list[BaseAgent] | 这个Agent下面挂载的子Agent们 | 子代理列表 |
| before_agent_callback | Callable or None | 在Agent处理输入前，进行预处理的回调 | 处理前的自定义函数 |
| after_agent_callback | Callable or None | 在Agent完成任务后，进行后处理的回调 | 处理后的自定义函数 |
| model | str or BaseLlm | 关联的模型，可以是字符串名或模型实例（如LiteLlm） | 使用的模型（必填） |
| instruction | str or Callable | 给Agent的详细指令，可以是固定文本，也可以是函数动态生成 | 指令说明（必填或推荐填写） |
| global_instruction | str or Callable | 在整个Agent树范围共享的指令（全局指令） | 全局指令（少用） |
| tools | list[Callable or BaseTool] | 这个Agent可以调用的外部工具列表 | 工具列表 |
| generate_content_config | GenerateContentConfig or None | 控制模型生成内容时的一些配置，比如最大Token数、温度等 | 生成设置（选填） |
| disallow_transfer_to_parent | bool | 禁止把任务转给父Agent（防止死循环） | 禁止转移到父代理 |
| disallow_transfer_to_peers | bool | 禁止把任务转给同级Agent | 禁止转移到同级代理 |
| include_contents | &#x27;default&#x27; or &#x27;none&#x27; | 是否在提示中包含历史对话内容 | 是否包含上下文 |
| input_schema | pydantic.BaseModel or None | 限定输入必须符合某种结构（Pydantic模型） | 输入校验模型 |
| output_schema | pydantic.BaseModel or None | 限定输出必须符合某种结构（Pydantic模型） | 输出校验模型 |
| output_key | str or None | 指定模型输出结果中的哪个字段作为最终输出 | 输出字段名 |
| planner | BasePlanner or None | 自定义计划器（比如动态规划步骤） | 规划器（进阶功能） |
| code_executor | BaseCodeExecutor or None | 允许Agent执行代码（如Python沙盒环境） | 代码执行器（进阶功能） |
| examples | list[Example] or BaseExampleProvider or None | 示例对话或行为示范，帮助模型更好完成任务 | 示例数据 |
| before_model_callback | Callable or None | 在发送给模型推理前，修改请求内容 | 模型请求前回调 |
| after_model_callback | Callable or None | 在收到模型响应后，修改或处理响应 | 模型响应后回调 |
| before_tool_callback | Callable or None | 在调用工具前修改参数或做拦截 | 工具调用前处理 |
| after_tool_callback | Callable or None | 在工具返回结果后做处理 | 工具调用后处理 |

其中四个核心参数：

+   model 是必须指定的（不指定Agent就不会工作）

+   instruction 和 description 是推荐填写的（帮助大模型更好理解自己要干什么）

+   tools 是选填的（如果需要调用外部API、数据库等，就注册工具）

+   sub_agents 是选填的（需要多Agent协作时再用）

一个极简的Agent对象创建方法如下：

```
agent = Agent(
    name=&quot;simple_agent&quot;,
    model=model,
    instruction=&quot;你是一个乐于助人的中文助手。&quot;,
    description=&quot;回答用户的问题。&quot;
)
```

### Step 3.创建一个Session

紧接着创建一个Session对象，用于保存Agent运行过程中的多轮对话内容。ADK提供了一个InMemorySessionService类，其核心作用如下：

+   保存每轮用户输入（user messages）

+   保存每轮模型输出（assistant messages）

+   保存工具调用过程中的中间数据（如果有工具的话）

+   提供历史对话上下文，让模型能基于完整对话继续推理，而不是每次都从空白开始

```
InMemorySessionService?
```

```
[31mInit signature:[39m InMemorySessionService()
[31mDocstring:[39m      An in-memory implementation of the session service.
[31mFile:[39m           c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\google\adk\sessions\in_memory_session_service.py
[31mType:[39m           ABCMeta
[31mSubclasses:[39m     
```

在前期测试时，我们可以创建一个InMemorySessionService()实例:

```
session_service = InMemorySessionService()
```

这个对话管理示例实际会在内存中进行历史会话信息管理。而具体的会话信息会保存在.sessions属性里，默认为空：

```
session_service.sessions
```

```
{}
```

在正式开启对话之前，我们需要创建一个初始session对象，为了便于区分不同的会话，该对象需要设置APP名称、用户ID和对话ID，方便后续进行查找和调用：

```
APP_NAME = &quot;test_app&quot;
USER_ID = &quot;user_1&quot;
SESSION_ID = &quot;session_001&quot;
```

```
session = session_service.create_session(
    app_name=APP_NAME,
    user_id=USER_ID,
    session_id=SESSION_ID
)
print(f&quot;Session created: App=&#x27;{APP_NAME}&#x27;, User=&#x27;{USER_ID}&#x27;, Session=&#x27;{SESSION_ID}&#x27;&quot;)
```

```
Session created: App=&#x27;test_app&#x27;, User=&#x27;user_1&#x27;, Session=&#x27;session_001&#x27;
```

需要注意的是，执行完上面的代码后，我们就获得了一个包含APP名称、用户ID和对话ID的session对象：

```
session
```

```
Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[], last_update_time=1745764835.901281)
```

```
session.app_name
```

```
&#x27;test_app&#x27;
```

```
session.user_id
```

```
&#x27;user_1&#x27;
```

```
session.id
```

```
&#x27;session_001&#x27;
```

目前session对象是空的，而在后续执行过程中，伴随着上下文生成器存储了更多的会话信息，我们也可以将更多的上下文信息保存在session中。

不过这里需要注意的是，在创建session对象的同时，对话生成器session_service的.sessions也发生了变化，其内容与session基本一致：

```
session_service.sessions
```

```
{&#x27;test_app&#x27;: {&#x27;user_1&#x27;: {&#x27;session_001&#x27;: Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[], last_update_time=1745764835.901281)}}}
```

而需要注意的是，在后续对话中，我们实际上是带入session_service进行对话，并读取session_service.sessions里面的历史对话信息作为上下文。这里可以将单独创建的session对象视作一个中间对象或者一个备份。

### Step 4.创建智能体执行器（Runner）

接下来进一步创建Runner对象，简单说，Runner 是 ADK 中真正让 Agent“动起来”的引擎，其核心功能和Agents SDK中的Runner类似，具体职责如下：

| 职责 | 说明 |
| --- | --- |
| 会话管理 | 自动读取/写入 SessionService，维护历史 |
| Agent调用 | 调用指定的 Agent 完成推理和工具调用 |
| 输入输出流转 | 把用户输入交给Agent，把Agent输出返回 |
| 流程控制 | 支持多轮对话、子Agent委托、工具调用等 |
| 生命周期管理 | 处理每一次对话流程的完整生命周期 |

也就是说**Runner：Agent的管理者**，负责组织每一次对话交互的完整生命周期。

```
Runner?
```

```
[31mInit signature:[39m
Runner(
    *,
    app_name: [33m&#x27;str&#x27;[39m,
    agent: [33m&#x27;BaseAgent&#x27;[39m,
    artifact_service: [33m&#x27;Optional[BaseArtifactService]&#x27;[39m = [38;5;28;01mNone[39;00m,
    session_service: [33m&#x27;BaseSessionService&#x27;[39m,
    memory_service: [33m&#x27;Optional[BaseMemoryService]&#x27;[39m = [38;5;28;01mNone[39;00m,
)
[31mDocstring:[39m     
The Runner class is used to run agents.

It manages the execution of an agent within a session, handling message
processing, event generation, and interaction with various services like
artifact storage, session management, and memory.

Attributes:
    app_name: The application name of the runner.
    agent: The root agent to run.
    artifact_service: The artifact service for the runner.
    session_service: The session service for the runner.
    memory_service: The memory service for the runner.
[31mInit docstring:[39m
Initializes the Runner.

Args:
    app_name: The application name of the runner.
    agent: The root agent to run.
    artifact_service: The artifact service for the runner.
    session_service: The session service for the runner.
    memory_service: The memory service for the runner.
[31mFile:[39m           c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\google\adk\runners.py
[31mType:[39m           type
[31mSubclasses:[39m     InMemoryRunner
```

其核心参数解释如下：

| 参数名 | 类型 | 说明 | 中文翻译 |
| --- | --- | --- | --- |
| app_name | str | 应用程序的名称，用于标识不同的应用实例 | 应用名称 |
| agent | BaseAgent | 要运行的根 Agent，负责执行实际的任务和推理 | 要运行的主代理 |
| artifact_service | Optional[BaseArtifactService] | 可选的文件存储服务，用于处理生成的工件（如文件、模型） | 文件存储服务（可选） |
| session_service | BaseSessionService | 必需的会话服务，用于管理和维护对话历史 | 会话服务（必须） |
| memory_service | Optional[BaseMemoryService] | 可选的内存服务，用于长期和短期记忆管理 | 内存服务（可选） |

而Runner类有个直观重要的方法，Runner.run_async，其核心解释如下：

```
Runner.run_async?
```

```
[31mSignature:[39m
Runner.run_async(
    self,
    *,
    user_id: [33m&#x27;str&#x27;[39m,
    session_id: [33m&#x27;str&#x27;[39m,
    new_message: [33m&#x27;types.Content&#x27;[39m,
    run_config: [33m&#x27;RunConfig&#x27;[39m = RunConfig(speech_config=[38;5;28;01mNone[39;00m, response_modalities=[38;5;28;01mNone[39;00m, save_input_blobs_as_artifacts=[38;5;28;01mFalse[39;00m, support_cfc=[38;5;28;01mFalse[39;00m, streaming_mode=&lt;StreamingMode.NONE: [38;5;28;01mNone[39;00m&gt;, output_audio_transcription=[38;5;28;01mNone[39;00m, max_llm_calls=[32m500[39m),
) -&gt; [33m&#x27;AsyncGenerator[Event, None]&#x27;[39m
[31mDocstring:[39m
Main entry method to run the agent in this runner.

Args:
  user_id: The user ID of the session.
  session_id: The session ID of the session.
  new_message: A new message to append to the session.
  run_config: The run config for the agent.

Yields:
  The events generated by the agent.
[31mFile:[39m      c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\google\adk\runners.py
[31mType:[39m      function
```

runner.run_a**sync 是** Runner 类的核**心方法之一**，用于以异步方式执行 Agent 的任务。这个方法接收用户输入的消息，并将其添加到会话中，随后启动 Agent 来处理该消息，并生成一系列的事件（Event）。

具体参数解释如下：

1.  **user_id** (str):

+   该参数表示**用户的唯一标识**，用于标记当前会话是属于哪个用户的。每个用户可以有多个会话实例。

+   示例：user_id="user_123"

1.  **session_id** (str):

+   该参数表示**会话的唯一标识**，用于区分不同用户或同一用户的不同对话会话。每次创建会话时，都会为其分配一个唯一的 session_id。

+   示例：session_id="session_456"

1.  **new_message** (types.Content):

+   这是 **ADK** 中的一个核心对象，表示传递给 Agent 的新消息。types.Content 是一个包含了消息的具体内容的对象，通常它包含 role（角色）、text（消息文本）等字段。

+   示例：new_message=types.Content(role="user", parts=[types.Part(text="今天天气如何？")])

1.  **run_config** (RunConfig, 可选):

+   这是一个配置对象，用于**定制执行时的参数设置**。可以根据需求设置一些如语音配置、响应方式、最大模型调用次数等参数。其默认值为 RunConfig()，这是一个内置的默认配置类。

1.  **RunConfig**** 的核心字段解释**：

+   speech_config: 用于配置语音识别和语音输出的相关设置（例如语音转文本、语音输出等）。通常与语音助手相关。

+   response_modalities: 配置响应的方式，例如，返回文本、语音等。

+   save_input_blobs_as_artifacts: 是否将输入文件保存为工件（例如，将用户上传的图片保存为文件以供后续处理）。默认值为 False。

+   support_cfc: 是否支持 CFC（Custom Function Calling），即是否允许 Agent 调用自定义函数。

+   streaming_mode: 设置流模式（例如是否开启流式输出），用于处理长时间生成的响应。

+   output_audio_transcription: 配置输出的音频转录设置（如果支持语音输出）。

+   max_llm_calls: 配置在一次会话中最大可以调用 LLM（大语言模型）次数的上限。默认值为 500。

接下来我们创建Runner对象，而Runner.run_async方法会在之后用到。这里需要带入此前创建的session_service对象：

```
session_service
```

```
&lt;google.adk.sessions.in_memory_session_service.InMemorySessionService at 0x1afb8c9a790&gt;
```

```
runner = Runner(
    agent=agent, # The agent we want to run
    app_name=APP_NAME,   # Associates runs with our app
    session_service=session_service # Uses our session manager
)
```

```
print(f&quot;Runner created for agent &#x27;{runner.agent.name}&#x27;.&quot;)
```

```
Runner created for agent &#x27;simple_agent&#x27;.
```

> ADK的runner和Agents SDK runner功能类似，本质上都是完成一个任务的循环，稍后会具体介绍。

### **Step 5.执行对话**

接下来我们需要创建一个名为call_agent_async的函数来获得Agent响应，其代码如下：

```
async def call_agent_async(query: str, runner, user_id, session_id):
  &quot;&quot;&quot;Sends a query to the agent and prints the final response.&quot;&quot;&quot;
  print(f&quot;\n&gt;&gt;&gt; User Query: {query}&quot;)

  # Prepare the user&#x27;s message in ADK format
  content = types.Content(role=&#x27;user&#x27;, parts=[types.Part(text=query)])

  final_response_text = &quot;Agent did not produce a final response.&quot; # Default

  # Key Concept: run_async executes the agent logic and yields Events.
  # We iterate through events to find the final answer.
  async for event in runner.run_async(user_id=user_id, session_id=session_id, new_message=content):
      # You can uncomment the line below to see *all* events during execution
      print(f&quot;  [Event] Author: {event.author}, Type: {type(event).__name__}, Final: {event.is_final_response()}, Content: {event.content}&quot;)

      # Key Concept: is_final_response() marks the concluding message for the turn.
      if event.is_final_response():
          if event.content and event.content.parts:
             # Assuming text response in the first part
             final_response_text = event.content.parts[0].text
          elif event.actions and event.actions.escalate: # Handle potential errors/escalations
             final_response_text = f&quot;Agent escalated: {event.error_message or &#x27;No specific message.&#x27;}&quot;
          # Add more checks here if needed (e.g., specific error codes)
          break # Stop processing events once the final response is found

  print(f&quot;&lt;&lt;&lt; Agent Response: {final_response_text}&quot;)
```

这段代码定义了一个 call_agent_async 异步函数，用来：

1.  接收用户的查询（query）

1.  调用 **Agent** 执行推理

1.  获取并返回最终的 **Agent** 回复

其核心运行流程是：

+   将用户查询转换为 ADK 需要的格式

+   通过 runn**er.ru**n_async() 运行 Agent

+   通过 **事件驱动机制** 获取和处理每一轮的消息、工具调用、回复

+   当 **最终响应** 到达时，停止循环并返回 Agent 的最终回答。

完整代码解释如下：

Part 1. **导入必要模块**

```
from google.genai import types # For creating message Content/Parts
```

+   这个 types 模块是用于在 ADK 中创建消息内容的。它允许你定义消息的“角色”（如 use**r 或 **assistant），以及消息的内容部分。

Part 2. **定义 ****cal**l_agent_async 函数

```
async def call_agent_async(query: str, runner, user_id, session_id):
  &quot;&quot;&quot;Sends a query to the agent and prints the final response.&quot;&quot;&quot;
  print(f&quot;\n&gt;&gt;&gt; User Query: {query}&quot;)
```

+   query: 用户输入的查询问题。

+   runner: 你之前创建的 **Runner** 对象，负责启动 Agent 并管理会话。

+   user_id 和 session_id: 用于标识特定用户和会话，确保模型能够维持会话上下文。

```
content = types.Content(role=&#x27;user&#x27;, parts=[types.Part(text=query)])
```

+   这里将用户的查询（query）格式化为 **ADK 消息格式**，并通过 types.Content 和 types.Part 封装成可以传递给 **Agent** 的内容。role='user' 表明消息是由用户发送的。

Part 3. 准备获取 Agent 的最终回复

```
final_response_text = &quot;Agent did not produce a final response.&quot; # Default
```

+   默认情况下，**final_respo**nse_text 被初始化为一个错误信息字符串，表明Agent没有最终响应。后续会被更新为真正的回答。

Part 4. **调用 Agent 并监听事件**

```
async for event in runner.run_async(user_id=user_id, session_id=session_id, new_message=content):
```

+   runner.run_a**sync() 是一**个异步生成器，**负责执行Agent推**理并生成一系列 事件（Events），每个事件代表了交互中的一步操作（如用户消息、模型回复、工具调用等）。

+   你可以通过 async for 循环来**迭代所有事件**，并逐一检查它们的内容。

Part 5. **检查是否是最终响应**

```
if event.is_final_response():
```

+   关键点：event.is_fin**al_r**esponse() 用来判断当前事件是否是最终回复。这个判断标志着Agent已经完成了对用户问题的处理，可以停止进一步处理。

```
if event.content and event.content.parts:
    final_response_text = event.content.parts[0].text
```

+   如果事件的 content 存在，并且包含了 parts（模型回复的文本部分），就将 **最终回复** 保存到 final_response_text 变量中。

```
elif event.actions and event.actions.escalate:
    final_response_text = f&quot;Agent escalated: {event.error_message or &#x27;No specific message.&#x27;}&quot;
```

+   如果 event.actio**ns.escala**te 为 True，表示 错误发生或需要升级，比如模型不能处理问题，或者需要人工干预。这时，我们会将相应的错误信息记录在 final_response_text。

```
break  # Stop processing events once the final response is found
```

+   一旦获取到最终的响应，break 语句会**停止事件处理**，不再继续监听其他的事件。

Part 6. **打印最终响应**

```
print(f&quot;&lt;&lt;&lt; Agent Response: {final_response_text}&quot;)
```

+   最后将 **Agent** 的最终回复输出到控制台。

**这里有以下几点需要注意：***

+   为什么要定义这个辅助函数**？在这段代****码中，我**们通过 call_**agent**_async 这个辅助函数来实现与 Agent 的交互。它的作用不仅是简单地给用户回复答案，而是通过 事件驱动 机制，来处理与 Agent 的复杂交互和状态管理。

+   为什么不直接调用 **runner.ru**n_async？这里的设计其实是为了让我们能够更方便地处理和监控整个过程，尤其是在多轮对话或复杂交互中。将这些复杂逻辑封装到一个函数里，使得代码更加清晰、可维护，并且容易扩展。

也就是说，我们可以把call**_agen**t_async 想象成一个管控器，它负责管理这个异步对话的过程，并确保每一部分顺利完成。这个过程很复杂，因为：

+   它不是**一次性任务**，可能涉及**多步推理**、**外部工具调用**、**错误处理**等环节。

+   我们需要**逐步监控每一个事件**（可能有很多中间状态、工具调用等）。

+   只有当所有步骤完成，最终的模型回复才算“最终回应”。

实际运行流程如下：

```
query=&#x27;你好，我叫陈明，好久不见！&#x27;
```

```
USER_ID, SESSION_ID
```

```
(&#x27;user_1&#x27;, &#x27;session_001&#x27;)
```

```
runner
```

```
&lt;google.adk.runners.Runner at 0x1afbafcd810&gt;
```

```
await call_agent_async(query, runner, USER_ID, SESSION_ID)
```

```
&gt;&gt;&gt; User Query: 你好，我叫陈明，好久不见！
  [Event] Author: simple_agent, Type: Event, Final: True, Content: parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？&#x27;)] role=&#x27;model&#x27;
&lt;&lt;&lt; Agent Response: 你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？
```

此时输出内容如下：

+   **Author**：simple_agent — 这个事件是由 **simple_agent** 发出的，表示当前响应来自于我们定义的 Agent。

+   **Type**：Event — 表明这是一个事件对象，表示某个状态发生。

+   **Final**：True — True 表示这是**最终的响应**，模型已经完成任务并准备返回最终结果。

+   **Content**：content.parts=[Part(...)] — 这里包含了模型回复的实际内容。它是一个 Part 对象，其中 text 字段就是最终的文本回答：

```
&#x27;你好！我是你的智能助手，专门用来回答你的问题、提供帮助或陪你聊天。我可以帮你查询信息、解答疑问、提供建议，或者只是随便聊聊。有什么我可以帮你的吗？ 😊&#x27;
```

+   **role**：'model' — 这个角色是 **模型**，意味着这条消息是从模型生成的，不是用户输入。

也就是说，在普通的问答过程中，事件流会经过以下几个关键步骤：

1.  **用户输入**：

+   用户提交一个查询请求（例如：“你好，好久不见，请介绍下你自己。”）

1.  **生成事件**：

+   系统触发一个 **事件**，表示用户输入已到达，准备交给 Agent 处理。

1.  **Agent 处理**：

+   **Agent** 接收用户的输入，开始进行推理（基于 instruction 和模型的推理能力），并生成相应的回答。

1.  **工具调用**（可选）：

+   如果 Agent 需要外部工具（例如查询天气、数据库、搜索引擎等），会触发工具调用事件。

1.  **最终响应**：

+   **最终响应**（如 final=True）被标记为完成，且不会有更多的事件生成。此时，Agent 将答复用户，结束本轮对话。

![](img/595bd77fcee750e840b7967e6e27d25e.png)

## 四、ADK历史对话信息保存机制与构建多轮对话机器人

### 1.ADK历史对话保存机制

需要注意的是，我们无需任何额外设置，ADK的历史对话信息就保存在session_service.sessions对象中：

```
session_service.sessions
```

```
{&#x27;test_app&#x27;: {&#x27;user_1&#x27;: {&#x27;session_001&#x27;: Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，我叫陈明，好久不见！&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;MiKceB3V&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;kpWFucAM&#x27;, timestamp=1745765051.476204)], last_update_time=1745765051.476204)}}}
```

因此无需任何额外操作，我们直接再进行新的对话，就会带入历史上下文进行对话：

```
query=&#x27;很高兴认识你，你还记得我叫什么名字么？&#x27;
```

```
USER_ID, SESSION_ID
```

```
(&#x27;user_1&#x27;, &#x27;session_001&#x27;)
```

```
await call_agent_async(query, runner, USER_ID, SESSION_ID)
```

```
&gt;&gt;&gt; User Query: 很高兴认识你，你还记得我叫什么名字么？
  [Event] Author: simple_agent, Type: Event, Final: True, Content: parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  \n\n很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？&#x27;)] role=&#x27;model&#x27;
&lt;&lt;&lt; Agent Response: 当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  

很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？
```

而新的会话信息会继续存储在session_service.sessions中。

```
session_service.sessions
```

```
{&#x27;test_app&#x27;: {&#x27;user_1&#x27;: {&#x27;session_001&#x27;: Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，我叫陈明，好久不见！&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;MiKceB3V&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;kpWFucAM&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;很高兴认识你，你还记得我叫什么名字么？&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;wXFFrypr&#x27;, timestamp=1745765160.231687), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  \n\n很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;mZTBij0B&#x27;, timestamp=1745765160.231687)], last_update_time=1745765160.231687)}}}
```

据此查看test_app里面的对话信息：

```
session_service.sessions[&#x27;test_app&#x27;]
```

```
{&#x27;user_1&#x27;: {&#x27;session_001&#x27;: Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，我叫陈明，好久不见！&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;MiKceB3V&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;kpWFucAM&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;很高兴认识你，你还记得我叫什么名字么？&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;wXFFrypr&#x27;, timestamp=1745765160.231687), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  \n\n很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;mZTBij0B&#x27;, timestamp=1745765160.231687)], last_update_time=1745765160.231687)}}
```

进一步查看用户user_1的对话信息：

```
session_service.sessions[&#x27;test_app&#x27;][&#x27;user_1&#x27;]
```

```
{&#x27;session_001&#x27;: Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，我叫陈明，好久不见！&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;MiKceB3V&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;kpWFucAM&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;很高兴认识你，你还记得我叫什么名字么？&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;wXFFrypr&#x27;, timestamp=1745765160.231687), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  \n\n很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;mZTBij0B&#x27;, timestamp=1745765160.231687)], last_update_time=1745765160.231687)}
```

进一步查看用户user_1的session_001对话信息：

```
session_service.sessions[&#x27;test_app&#x27;][&#x27;user_1&#x27;][&#x27;session_001&#x27;]
```

```
Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，我叫陈明，好久不见！&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;MiKceB3V&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;kpWFucAM&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;很高兴认识你，你还记得我叫什么名字么？&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;wXFFrypr&#x27;, timestamp=1745765160.231687), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  \n\n很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;mZTBij0B&#x27;, timestamp=1745765160.231687)], last_update_time=1745765160.231687)
```

由此可见ADK本身的对话管理非常完整细致。我们只需要重复调用await call_agent_async(query, runner, USER_ID, SESSION_ID)，这个流程本身就是在执行多轮对话。

### 2.ADK会话容器与会话管理器

谷歌ADK的企业级用户会话管理功能的实现，主要依托于InMemorySessionService和Session两个类，此前我们曾经分别使用过，接下来详细介绍这两个对象的基本功能。

```
from google.adk.sessions import InMemorySessionService, Session
```

```
InMemorySessionService?
```

```
[31mInit signature:[39m InMemorySessionService()
[31mDocstring:[39m      An in-memory implementation of the session service.
[31mFile:[39m           c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\google\adk\sessions\in_memory_session_service.py
[31mType:[39m           ABCMeta
[31mSubclasses:[39m     
```

#### 2.1 会话容器Session

在此前我们首次创建会话管理器session_service的时候，曾经使用create命令初始化过一个会话容器：

![](img/c507931080709ed3b68d7852184a0776.png)

而伴随着对话的进行，多轮对话信息都保存在session_service.sessions中，而最开始创建的会话容器session还保留着最初始的状态：

```
session
```

```
Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[], last_update_time=1745764835.901281)
```

相比每次对话时都会带入的session_service.sessions对象，会话容器session更像是一种备份、或者充当保存点的角色，我们可以使用session_service.get_session内容创建一个包含当前对话全部信息的会话容器：

```
session_all = session_service.get_session(
    app_name=APP_NAME,
    user_id=USER_ID,
    session_id=SESSION_ID
)
```

此时session_all就包含了全部会话信息：

```
session_all
```

```
Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，我叫陈明，好久不见！&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;MiKceB3V&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;kpWFucAM&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;很高兴认识你，你还记得我叫什么名字么？&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;wXFFrypr&#x27;, timestamp=1745765160.231687), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  \n\n很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;mZTBij0B&#x27;, timestamp=1745765160.231687)], last_update_time=1745765160.231687)
```

而我们现在就有两个会话容器了，其中session是个空的容器，而session_all是一个包含了好几轮对话信息的容器：

```
session
```

```
Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[], last_update_time=1745764835.901281)
```

而如果我们想要回到最初始的状态（清空全部历史消息），则可以使用如下方法恢复到session保留的记忆：

```
session_service.sessions[&#x27;test_app&#x27;][&#x27;user_1&#x27;][&#x27;session_001&#x27;]
```

```
Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，我叫陈明，好久不见！&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;MiKceB3V&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;kpWFucAM&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;很高兴认识你，你还记得我叫什么名字么？&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;wXFFrypr&#x27;, timestamp=1745765160.231687), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  \n\n很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;mZTBij0B&#x27;, timestamp=1745765160.231687)], last_update_time=1745765160.231687)
```

```
session_service.sessions[&#x27;test_app&#x27;][&#x27;user_1&#x27;][&#x27;session_001&#x27;] = session
```

此时我们会话生成器的历史对话就和session同步了：

```
session_service.sessions
```

```
{&#x27;test_app&#x27;: {&#x27;user_1&#x27;: {&#x27;session_001&#x27;: Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[], last_update_time=1745764835.901281)}}}
```

此时再进行对话，则不会有任何历史对话信息：

```
query = &#x27;你好，你还记得我叫什么名字么？&#x27;
```

```
await call_agent_async(query, runner, USER_ID, SESSION_ID)
```

```
&gt;&gt;&gt; User Query: 你好，你还记得我叫什么名字么？
  [Event] Author: simple_agent, Type: Event, Final: True, Content: parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好！作为AI助手，我无法记住之前的对话内容或个人信息。每次新的对话开始时，我都不会保留之前的聊天记录。不过你可以随时告诉我你的名字，我会很高兴认识你！😊 有什么我可以帮你的吗？&#x27;)] role=&#x27;model&#x27;
&lt;&lt;&lt; Agent Response: 你好！作为AI助手，我无法记住之前的对话内容或个人信息。每次新的对话开始时，我都不会保留之前的聊天记录。不过你可以随时告诉我你的名字，我会很高兴认识你！😊 有什么我可以帮你的吗？
```

而本次对话信息会继续保留在session_service.sessions中：

```
session_service.sessions
```

```
{&#x27;test_app&#x27;: {&#x27;user_1&#x27;: {&#x27;session_001&#x27;: Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，你还记得我叫什么名字么？&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-d6d3b179-17ff-4dd1-9707-2af4787c3777&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;efarDm72&#x27;, timestamp=1745766363.459974), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好！作为AI助手，我无法记住之前的对话内容或个人信息。每次新的对话开始时，我都不会保留之前的聊天记录。不过你可以随时告诉我你的名字，我会很高兴认识你！😊 有什么我可以帮你的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-d6d3b179-17ff-4dd1-9707-2af4787c3777&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;IpGsef5Z&#x27;, timestamp=1745766363.460605)], last_update_time=1745766363.460605)}}}
```

而如果我们想要恢复此前session_all的会话内容，则可以使用如下方法：

```
session_service.sessions[&#x27;test_app&#x27;][&#x27;user_1&#x27;][&#x27;session_001&#x27;] = session_all
```

```
session_service.sessions
```

```
{&#x27;test_app&#x27;: {&#x27;user_1&#x27;: {&#x27;session_001&#x27;: Session(id=&#x27;session_001&#x27;, app_name=&#x27;test_app&#x27;, user_id=&#x27;user_1&#x27;, state={}, events=[Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，我叫陈明，好久不见！&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;MiKceB3V&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;你好，陈明！很高兴再次见到你！最近过得怎么样？有什么我可以帮你的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-14ecf65e-58da-4059-b5d0-258f6d63069e&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;kpWFucAM&#x27;, timestamp=1745765051.476204), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;很高兴认识你，你还记得我叫什么名字么？&#x27;)], role=&#x27;user&#x27;), grounding_metadata=None, partial=None, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;user&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;wXFFrypr&#x27;, timestamp=1745765160.231687), Event(content=Content(parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;当然记得呀！你刚刚告诉过我，你叫**陈明**~ （*＾-＾*）  \n\n很高兴认识你，陈明！有什么想聊的或者需要帮忙的吗？&#x27;)], role=&#x27;model&#x27;), grounding_metadata=None, partial=False, turn_complete=None, error_code=None, error_message=None, interrupted=None, custom_metadata=None, invocation_id=&#x27;e-c76b5a87-154e-4f64-a773-d87dccec320f&#x27;, author=&#x27;simple_agent&#x27;, actions=EventActions(skip_summarization=None, state_delta={}, artifact_delta={}, transfer_to_agent=None, escalate=None, requested_auth_configs={}), long_running_tool_ids=None, branch=None, id=&#x27;mZTBij0B&#x27;, timestamp=1745765160.231687)], last_update_time=1745765160.231687)}}}
```

然后测试模型对话记忆：

```
query = &#x27;你好，你还记得我叫什么名字么？&#x27;
```

```
await call_agent_async(query, runner, USER_ID, SESSION_ID)
```

```
&gt;&gt;&gt; User Query: 你好，你还记得我叫什么名字么？
  [Event] Author: simple_agent, Type: Event, Final: True, Content: parts=[Part(video_metadata=None, thought=None, code_execution_result=None, executable_code=None, file_data=None, function_call=None, function_response=None, inline_data=None, text=&#x27;哈哈，当然记得啦！你叫**陈明**嘛～（聊天记录可是有“小本本”记着的✨）  \n\n需要我帮你记住其他信息吗？或者今天想聊点什么？ 😄&#x27;)] role=&#x27;model&#x27;
&lt;&lt;&lt; Agent Response: 哈哈，当然记得啦！你叫**陈明**嘛～（聊天记录可是有“小本本”记着的✨）  

需要我帮你记住其他信息吗？或者今天想聊点什么？ 😄
```

由此不难发现对话容器的核心用途，便于进行更加灵活的会话管理。

> 此外我们还可以通过session_id、user_id和app_name来进行会话管理。更多进阶内容会在付费课程中进行讲解。

#### 2.2 会话生成器session_service

从此前的介绍中不难看出，会话生成器session_service的核心作用是伴随着会话进程，时刻保存会话信息。而实际上会话生成器还有更多的会话生命周期管理的功能：

```
session_service?
```

```
[31mType:[39m        InMemorySessionService
[31mString form:[39m &lt;google.adk.sessions.in_memory_session_service.InMemorySessionService object at 0x000001AFB8C9A790&gt;
[31mFile:[39m        c:\programdata\anaconda3\envs\jupyter_env\lib\site-packages\google\adk\sessions\in_memory_session_service.py
[31mDocstring:[39m   An in-memory implementation of the session service.
```

总的来说ADK的会话生成器有三种，其一是我们前文介绍的InMemorySessionService类：

```
from google.adk.sessions import InMemorySessionService
```

该类实例化的会话生成器，是基于内存的会话生成与保存，使用更加轻量便捷，但并不能持久化保存，一旦环境重启（或进程终止），历史会话就会消失。因此ADK还提供了另外两种能够持久化保存会话信息的方法，分别是使用数据库（PostgreSQL、MySQL、SQLite）进行保存的库DatabaseSessionService：

```
from google.adk.sessions import DatabaseSessionService
```

以及可以在谷歌云平台Vertex AI上进行存储的VertexAiSessionService类：

```
from google.adk.sessions import VertexAiSessionService
```

若采用持久化的会话生成器，则每次生成的会话内容（包括events流）则都会自动存储在指定数据库中。整个过程要求新进行数据库安装下载配置，并创建响应的存储表。以DatabaseSessionService+MySQL为例，基本执行流程如下：

+   **第一步：设计数据库表结构**

根据 ADK 的要求，我们需要在 MySQL 中创建四张表：sessions、events、app_states 和 user_states。

+   **第二步：安装和配置 ADK**

紧接着在 Python 环境中安装 Google ADK：

```
pip install google-adk
```

然后，安装支持 MySQL 的扩展：

```
pip install pymysql
```

+   **第三步：配置 **DatabaseSessionService

然后在 Python 代码中，使用以下方式配置 DatabaseSessionService：

```
from google.adk.sessions import DatabaseSessionService

mysql_url = &quot;mysql+pymysql://username:password@localhost/adk_sessions&quot;
session_service = DatabaseSessionService(db_url=mysql_url)
```

这里需要将 username、password 和 localhost 替换为您的 MySQL 数据库的实际用户名、密码和主机地址。

+   **第四步：创建会话并添加事件**

在创建会话时，您可以初始化会话状态并添加事件：


&lt;center&gt;&lt;img src=&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/b9ccfe4155bb650c83c4a061f3da131.jpg&quot; alt=&quot;b9ccfe4155bb650c83c4a061f3da131&quot; style=&quot;zoom: 15%;&quot; /&gt;

&lt;center&gt;&lt;img src=&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/01dad5b6f552883b19da9b5863c9970.png&quot; alt=&quot;01dad5b6f552883b19da9b5863c9970&quot; style=&quot;zoom:50%;&quot; /&gt;


&lt;center&gt;&lt;img src=&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/7e1032040d5c3bd8447894629c9361e.png&quot; alt=&quot;7e1032040d5c3bd8447894629c9361e&quot; style=&quot;zoom:50%;&quot; /&gt;


&lt;center&gt;&lt;img src=&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/image-20250428112014361.png&quot; style=&quot;zoom: 50%;&quot; /&gt;

```
from google.adk.sessions import Session
from google.adk.events import UserMessage, AgentResponse

# 创建会话
session = session_service.create_session(
    app_name=&quot;MySQL_test&quot;,
    user_id=&quot;user_1&quot;,
    session_id=&quot;session_1&quot;,
    state={&quot;history&quot;: []}
)

之后的对话就和此前无异。

---

### **&lt;center&gt; 更多ADK会话信息持久化方法详见正课**

&amp;emsp;&amp;emsp;**更多关于持久化具体实操流程，以及更多ADK进阶内容讲解，详见[《2025大模型Agent智能体开发实战》(5月班)](https://ix9mq.xetslk.com/s/3u765N)付费课程。&lt;span style=&quot;color:red;&quot;&gt;详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&amp;查看课程详情👇&lt;/span&gt;**
---

## 五、ADK内置前端调用流程

### 1.ADK内置前端功能介绍

&amp;emsp;&amp;emsp;ADK作为最新一代Agent开发框架，不仅功能特性非常领先，而且还内置了非常多的工具，包括Gemini系列模型自带的谷歌搜索、文件检索和代码解释器等功能，同时ADK还拥有内置对话前端，方便开发者更加直观的感受Agent的对话流程，并且实时追踪Agent的events流。而如果Agent中存在外部工具调用，则在内置前端中，还能进一步观察到Agent调用外部工具完整流程，以及各环节调用信息，方便开发者即时debug。如果是使用Gemini模型，这个前端还支持语音和视频实时交互，基本展示效果如下：
### 2.ADK内置前端开启流程

&amp;emsp;&amp;emsp;而ADK内置前端开启流程也非常简单，这里我们需要创建一个项目文件，如创建一个ADK_Chat文件夹：
然后创建基本文件结构：（注意，如果是Windows系统，官方推荐手动创建项目文件基本结构，而不是采用IDE如Cursor等进行创建，以避免出现字符乱码情况）

```css
ADK_Chat/
├── test_agent/
│   ├── agent.py
│   ├── __init__.py
│   ├── .env
├── __init__.py
└── ...

![](img/49f435bc762945c060c7a01ee87f801d.png)


![](img/014a131568217989174775e72541f049.png)

```
然后使用cursor或者其他IDE打开项目，准备写入代码。（也可以直接使用文本编辑器将代码拷贝进去）

+   **init.py**

写入如下内容：from . import agent
+   **.env**

然后在.env中写入如下内容：

```
DS_BASE_URL=https://api.deepseek.com
DS_API_KEY=YOUR_API_KRY

![](img/03a14a138787305f9cfe54de270679c3.png)

```
+   **agent.py**

写入如下内容：

```
from google.adk.agents import Agent
from google.adk.models.lite_llm import LiteLlm

import os
from dotenv import load_dotenv
load_dotenv(override=True)


DS_API_KEY = os.getenv(&quot;DS_API_KEY&quot;)
DS_BASE_URL = os.getenv(&quot;DS_BASE_URL&quot;)

model = LiteLlm(
    model=&quot;deepseek/deepseek-chat&quot;,  
    api_base=DS_BASE_URL,
    api_key=DS_API_KEY
)

root_agent = Agent(
    name=&quot;simple_agent&quot;,
    model=model,
    instruction=&quot;你是一个乐于助人的中文助手。&quot;,
    description=&quot;回答用户的问题。&quot;
)

![](img/017f35c58589df88ae1977d445b114db.png)

```
然后再次确认当前环境是否安装adk和litellm两个库：

```
pip install litellm==1.67.2
pip install google-adk 
```

然后使用adk命令先进行Agent测试。我们在根目录下输入：

```
adk run test_agent

![](img/06bc3b67607ad9ba562c5253bf8a523b.png)

```

若能顺利进行对话，则说明Agent使用正常：
由此不难看出，adk其实是伴随着安装包一起安装的调用测试命令。在使用adk命令时，我们无需单独设置主函数，只需要按照要求创建一个名为root_agent的主agent，即可顺利开启对话。同时当前项目结构中，.env文件用于保存一些关键变量信息，而__init__.py则负责将当前项目文件test_agent包装为一个可执行的Python文件。这也就是为何我们可以直接输入adk run test_agent的原因。

接下来即可开启前端了，我们在项目根目录下输入：

```
adk web --port 8002

![](img/b1bac1d038065f43b0182ea5c3cb5b82.png)


![](img/d2a2ce974e659a2f7ebb343f5378899d.png)


![](img/15c36842611415a67408d817f990df55.png)

```

> 注意选择一个未被使用的端口号
然后在浏览器输入：http://0.0.0.0:8002/即可开启对话：
在对话页面能够看到完整的历史对话消息，并且在左侧还能看到事件流的基本情况，以及每个事件的详细信息：
并且此时Agent也是能支持多轮对话的。具体演示效果如下：

```
from IPython.display import Video
```

```
Video(&quot;https://ml2022.oss-cn-hangzhou.aliyuncs.com/2025-04-28%2012-12-22.mp4&quot;, width=800, height=400)
```

Your browser does not support the video element.

不难看出，尽管单独使用ADK代码环境创建Agent流程较为复杂，但如果是借助ADK run或者web等工具，则可以不进行任何额外其他设置，快速调用已经创建好的Agent并进行功能测试，期间关于会话ID、用户ID和APP ID等参数都会自动设置，且支持多轮对话并能打印详细的每个event信息，可以说是非常高效的开发工具了。

至此第一部分内容结束，下一部分我们将继续讲解ADK如何接入外部工具及MCP工具流程。

---

### 更多ADK会话信息持久化方法详见正课

**更多关于持久化具体实操流程，以及更多ADK**进阶内容讲解，详见《2025大模型Agent智能体开发实战》(5月班)付费课程。详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/f0a39e4ce16deee6e48c187cc2d55274.png)

![](img/6652067a3675cc8be7778bc74cbf26fe.png)

---