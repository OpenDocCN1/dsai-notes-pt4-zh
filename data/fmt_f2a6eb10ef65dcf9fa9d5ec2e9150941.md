
# DeepSeek API 使用指南

## client.chat.completions.create

```python
Signature:
client.chat.completions.create(
    *,
    messages: 'Iterable[ChatCompletionMessageParam]',
    model: 'Union[str, ChatModel]',
    audio: 'Optional[ChatCompletionAudioParam] | NotGiven' = NOT_GIVEN,
    frequency_penalty: 'Optional[float] | NotGiven' = NOT_GIVEN,
    function_call: 'completion_create_params.FunctionCall | NotGiven' = NOT_GIVEN,
    functions: 'Iterable[completion_create_params.Function] | NotGiven' = NOT_GIVEN,
    logit_bias: 'Optional[Dict[str, int]] | NotGiven' = NOT_GIVEN,
    logprobs: 'Optional[bool] | NotGiven' = NOT_GIVEN,
    max_completion_tokens: 'Optional[int] | NotGiven' = NOT_GIVEN,
    max_tokens: 'Optional[int] | NotGiven' = NOT_GIVEN,
    metadata: 'Optional[Dict[str, str]] | NotGiven' = NOT_GIVEN,
    modalities: 'Optional[List[ChatCompletionModality]] | NotGiven' = NOT_GIVEN,
    n: 'Optional[int] | NotGiven' = NOT_GIVEN,
    parallel_tool_calls: 'bool | NotGiven' = NOT_GIVEN,
    prediction: 'Optional[ChatCompletionPredictionContentParam] | NotGiven' = NOT_GIVEN,
    presence_penalty: 'Optional[float] | NotGiven' = NOT_GIVEN,
    response_format: 'completion_create_params.ResponseFormat | NotGiven' = NOT_GIVEN,
    seed: 'Optional[int] | NotGiven' = NOT_GIVEN,
    service_tier: "Optional[Literal['auto', 'default']] | NotGiven" = NOT_GIVEN,
    stop: 'Union[Optional[str], List[str]] | NotGiven' = NOT_GIVEN,
    store: 'Optional[bool] | NotGiven' = NOT_GIVEN,
    stream: 'Optional[Literal[False]] | Literal[True] | NotGiven' = NOT_GIVEN,
    stream_options: 'Optional[ChatCompletionStreamOptionsParam] | NotGiven' = NOT_GIVEN,
    temperature: 'Optional[float] | NotGiven' = NOT_GIVEN,
    tool_choice: 'ChatCompletionToolChoiceOptionParam | NotGiven' = NOT_GIVEN,
    tools: 'Iterable[ChatCompletionToolParam] | NotGiven' = NOT_GIVEN,
    top_logprobs: 'Optional[int] | NotGiven' = NOT_GIVEN,
    top_p: 'Optional[float] | NotGiven' = NOT_GIVEN,
    user: 'str | NotGiven' = NOT_GIVEN,
    extra_headers: 'Headers | None' = None,
    extra_query: 'Query | None' = None,
    extra_body: 'Body | None' = None,
    timeout: 'float | httpx.Timeout | None | NotGiven' = NOT_GIVEN
) -> 'ChatCompletion | Stream[ChatCompletionChunk]'
Docstring: <no docstring>
File: ~/anaconda3/lib/python3.12/site-packages/openai/resources/chat/completions.py
Type: method
```

## DeepSeek 支持的模型范围

```python
models_list = client.models.list()
```

```python
models_list.data
```

```python
[Model(id='deepseek-chat', created=None, object='model', owned_by='deepseek'),
 Model(id='deepseek-coder', created=None, object='model', owned_by='deepseek')]
```

## DeepSeek v3 模型参数说明

DeepSeek v3 模型属于聊天补全（chat completion）模型，模型提供了多种参数，帮助开发者定制化模型的生成行为。以下是对主要参数的详细解释，以及如何在 Python 中使用这些参数进行请求。

### messages (必填)

`messages` 参数是 DeepSeek v3 模型 API 中必填的参数之一，用于定义聊天上下文，包括用户的输入、系统的指令、助手的回复等。通过 `messages` 数组，模型可以理解当前对话的背景，从而生成更加连贯的响应。根据不同的使用场景，`messages` 包含多种类型的消息，例如 `system message`、`user message` 和 `assistant message`。下面是对 `messages` 参数及其各个子类型的详细解释。

#### System message

`system message` 用于设置系统消息，通常由开发者设定，以指导模型如何进行对话。这类消息可以定义规则或约束，并提供有关对话的背景信息。

+ `content` (必填)：系统消息的内容，可以是字符串或数组。如果是数组，可能包含多个类型的内容（如文本、图像）。
+ `role` (必填)：此处角色为 `system`，表明这是系统发出的消息。
+ `name` (可选)：提供系统消息发送者的名称，尤其适用于区分多个具有相同角色的参与者。

**示例**：

```python
system_message = {
    "role": "system",
    "content": "你是一位助人为乐的助手。"
}
```

```python
# 以下代码的注释由 MateGen 生成:
# 定义一个字典，键为 'system_message'，用于存储系统消息
system_message = {
    # 'role' 键对应的值是一个字符串 "system"，表示消息的发送者是系统
    "role": "system",
    # 'content' 键对应的值是一个字符串，表示系统消息的内容
    "content": "你是一位大学教授。"
}
```

#### User message

`user message` 表示用户发给模型的消息，是对话的核心部分之一。它定义了用户的输入内容，模型根据这些内容生成响应。

+ `content` (必填)：用户消息的内容，通常为文本或图像链接的数组。对于支持图像输入的模型，如 DeepSeek v2.5，还可以传递图像。
+ 文本内容：单纯的字符串形式，用户输入的文本。
+ 数组形式的内容：由文本或图像链接组成的数组，可以同时传递多张图像或多段文本内容。
+ `role` (必填)：角色为 `user`，表示该消息来自用户。
+ `name` (可选)：可以为用户指定一个名称，用于区分多个具有相同角色的用户。

**示例**：

```python
user_message = {
    "role": "user",
    "content": "你好，请介绍下你自己。"
}
```

如果传递多个内容（如图像和文本）：

```python
"url": "https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/image-20241009205714055.png",
```

```python
user_message_with_image = {
    "role": "user",
    "content": [
        {"type": "text", "text": "你能帮我介绍下这张图片里面的内容么？"},
        {"type": "image_url", 
         "image_url": {
                    }
        }
    ]
}
```
