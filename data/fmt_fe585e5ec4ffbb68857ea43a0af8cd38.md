
# OpenAI 模型列表与 API 调用

## 模型列表

```
wtr5tv'), Model(id='ft:gpt-3.5-turbo-1106:acmr::8NGu7dB7', created=1700557216,
object='model', owned_by='acmr-wtr5tv'), Model(id='ft:gpt-3.5-turbo-
1106:acmr::8NGecRZn', created=1700556254, object='model', owned_by='acmr-
wtr5tv'), Model(id='ft:gpt-3.5-turbo-1106:acmr::8NGoWxGp', created=1700556868,
object='model', owned_by='acmr-wtr5tv'), Model(id='ft:gpt-3.5-turbo-
1106:acmr::8NcxfjOE', created=1700642003, object='model', owned_by='acmr-
wtr5tv'), Model(id='ft:gpt-3.5-turbo-1106:acmr::8NISonhF', created=1700563210,
object='model', owned_by='acmr-wtr5tv'), Model(id='ft:gpt-3.5-turbo-
1106:acmr::8NIBwIXY', created=1700562164, object='model', owned_by='acmr-
wtr5tv'), Model(id='ft:gpt-3.5-turbo-1106:acmr::8NIJWEAY', created=1700562634,
object='model', owned_by='acmr-wtr5tv'), Model(id='ft:gpt-3.5-turbo-
1106:acmr::8NIZytXn', created=1700563654, object='model', owned_by='acmr-
wtr5tv'), Model(id='ft:babbage-002:acmr::8ONnv6dd', created=1700822067,
object='model', owned_by='acmr-wtr5tv'), Model(id='ft:gpt-3.5-turbo-
1106:acmr::8PSAdThJ', created=1701077179, object='model', owned_by='acmr-
wtr5tv'), Model(id='ft:gpt-3.5-turbo-1106:acmr::8Okwb8qA', created=1700911017,
object='model', owned_by='acmr-wtr5tv'), Model(id='ft:gpt-3.5-turbo-
1106:acmr::8Onym3eo', created=1700922684, object='model', owned_by='acmr-
wtr5tv'), Model(id='ft:gpt-3.5-turbo-1106:acmr::8P3jjlnu', created=1700983255,
object='model', owned_by='acmr-wtr5tv'), Model(id='ft:gpt-3.5-turbo-
1106:acmr::8P7Wp9ZW', created=1700997831, object='model', owned_by='acmr-
wtr5tv'), Model(id='ft:gpt-3.5-turbo-1106:acmr::8P8ICr9h', created=1701000768,
object='model', owned_by='acmr-wtr5tv'), Model(id='ft:gpt-3.5-turbo-
1106:acmr::8P8JcJOS', created=1701000856, object='model', owned_by='acmr-
wtr5tv'), Model(id='ft:gpt-3.5-turbo-1106:acmr::8P8MZq4B', created=1701001039,
object='model', owned_by='acmr-wtr5tv'), Model(id='ft:gpt-3.5-turbo-
1106:acmr::8P8eJWFu', created=1701002139, object='model', owned_by='acmr-
wtr5tv'), Model(id='ft:gpt-3.5-turbo-1106:acmr::8P8jDYPa', created=1701002444,
```

```
17 gpt-4-turbo-preview
18 omni-moderation-latest
19 gpt-4o-2024-05-13
20 omni-moderation-2024-09-26
21 tts-1-hd-1106
22 chatgpt-4o-latest
23 gpt-4
24 gpt-4-0613
25 o1-mini
26 gpt-3.5-turbo
27 gpt-3.5-turbo-0125
28 o1-preview
29 o1-preview-2024-09-12
30 gpt-4-turbo
31 tts-1-hd
32 text-embedding-ada-002
33 gpt-4-turbo-2024-04-09
34 gpt-3.5-turbo-1106
35 gpt-4o-audio-preview
36 gpt-4o-audio-preview-2024-10-01
37 tts-1
38 tts-1-1106
39 gpt-4o-2024-08-06
40 gpt-3.5-turbo-instruct-0914
41 text-embedding-3-large
42 gpt-4o-realtime-preview-2024-12-17
43 gpt-4o
44 gpt-4o-mini-realtime-preview
45 gpt-4o-mini-realtime-preview-2024-12-17
46 gpt-4o-2024-11-20
47 gpt-4o-audio-preview-2024-12-17
48 gpt-4o-mini-audio-preview
49 gpt-4o-mini-audio-preview-2024-12-17
50 ft:gpt-3.5-turbo-0613:acmr:recipe-ner:7rOJnQow
51 ft:babbage-002:acmr::8LjwL7pi
52 ft:gpt-3.5-turbo-1106:acmr::8NFcWDKd
53 ft:gpt-3.5-turbo-1106:acmr::8NG1DFJy
54 ft:gpt-3.5-turbo-1106:acmr::8NGu7dB7
55 ft:gpt-3.5-turbo-1106:acmr::8NGecRZn
56 ft:gpt-3.5-turbo-1106:acmr::8NGoWxGp
57 ft:gpt-3.5-turbo-1106:acmr::8NcxfjOE
58 ft:gpt-3.5-turbo-1106:acmr::8NISonhF
59 ft:gpt-3.5-turbo-1106:acmr::8NIBwIXY
60 ft:gpt-3.5-turbo-1106:acmr::8NIJWEAY
61 ft:gpt-3.5-turbo-1106:acmr::8NIZytXn
62 ft:babbage-002:acmr::8ONnv6dd
63 ft:gpt-3.5-turbo-1106:acmr::8PSAdThJ
```

获取模型列表的代码示例：

```
model_list = client.models.list().data
for usable_model_name in model_list:
    ...
```

如果我们想与上述列出的这些模型进行对话时，就需要向OpenAI提供的某个端点发送一个 HTTP 请求。简单来说，API 端点就像一个“门”，它连接了你和某个服务。你通过这个“门”向服务发送请求，并从它那里获得你需要的信息或结果。对于 OpenAI 的模型来说，这个端点就是你向模型发送请求、获取回复的地方。

[OpenAI Chat Completion EndPoint](https://platform.openai.com/docs/api-reference/chat/create)

![](img/354c0c501c75083ffeed8721f1cab58a.png)

如上所示，OpenAI提供的 API 端点地址，用于和它的模型（比如 GPT-4）进行对话。当你发送一条消息或问题时，会通过这个端点处理你的请求，并返回一个由模型生成的回答。其过程如下：

• Step 1. 发送请求：我们向发送一个请求，告诉模型你想要什么样的回答。这个请求会包含你给模型的输入（比如你问的问题或者你要进行的对话内容）。

• Step 2. 模型处理请求：OpenAI 的模型会收到你的请求，理解你要的内容，并生成相应的回答。

• Step 3. 获取响应：模型生成的回答会通过这个端点返回给你，你就能看到模型给出的响应了。

这个过程看起来可能比较复杂，因为它涉及到向 OpenAI 的服务器发送 HTTP 请求、处理请求的响应、进行身份验证等一系列操作，但实际上，OpenAI 提供了非常方便的工具来简化这一过程，调用封装好的方法，就能轻松与OpenAI的模型进行交互。

OpenAI提供了官方的，封装在 `client.chat.completions.create` 方法中，所以我们在使用Python编程语言的时候，则可以直接通过：

[OpenAI Chat Completion EndPoint](https://platform.openai.com/docs/api-reference/chat/create)

![](img/e508f334d15370e67c505064cfa71b79.png)

而在使用 SDK 的过程中我们要明确的是：虽然 SDK 简化了使用过程，但在构建这些 SDK 时，还是需要一些基础的参数支撑，比如 API 密钥、模型选择、消息格式等，这些参数确保了请求的正确性和有效性。这里我们先关注下必须要传入的参数，如下所示：

| 层级 | 描述 | 必填项 | 说明 |
| --- | --- | --- | --- |
| `model` | 要使用的模型 ID | 必填 | 指定模型的 ID，查看模型端点兼容表以获取支持的模型类型。 |
| `messages` | 所有消息的集合 | 必填 | 包含整个对话的消息数 |
| Developer message | 开发者提供的指令，模型需要遵循这些指令，无论用户发送什么消息。 | `content` (消息内容) | `content`：开发者消息内容 |
| System message | 开发者提供的指令，通常用于配置模型行为。 | `content` (消息内容), `role` (角色: system) | `content`：系统消息内容 |
| User message | 用户发送的消息，包含提示或额外的上下文信息。 | `content` (消息内容), `role` (角色: user) | `content`：用户消息内容 |
| Assistant message | 模型对用户消息的回应 | `content` (消息内容), `role` (角色: assistant) | `content`：模型回应的消息内容 |
| `tool_calls` | 模型调用的工具，通常用于外部函数调用 | `id` (工具调用 ID), `type` (工具类型) | `id`：唯一标识符 |
| `function` | 模型调用的函数 | `name` (函数名) | `name`：调用的函数名 |
| Tool message | 工具消息，通常用于模型与外部工具交互的响应 | `role` (角色: tool) | `role`：固定为 `"tool"` |

在使用OpenAI时，必须传入两个关键参数：

1. `model`：这个参数指定了要使用的模型的 ID。例如，你可以选择 GPT-4 或其他 OpenAI 提供的模型。模型的 ID 确定了你将与哪个版本的模型进行交互，因此它是必填项。
2. `messages`：这个参数包含了整个对话的消息集合。它是一个消息数组，每个消息都包含一个 `role`（角色，表示消息的发送者，如开发者、系统、用户或助手）和 `content`（消息内容）。

参数是与模型进行有效对话的基础，确保模型能根据上下文正确生成回应。

这两个参数是 SDK 必须传入的核心内容，`model` 确定了你选择的模型，而 `messages` 则提供了对话的上下文，模型根据这些信息来生成相应的输出。

当我们通过大模型的API进行会话交互时，数据交换的格式如下图所示。

上图中展示的 JSON 结构是API交互中常用的数据格式。不仅仅是 OpenAI 的 GPT 模型，其他在线模型或者开源模型均遵循这种规范。在这个例子中，我们看到有一个对象包含两个键值对：`"role": "user"`和`"content": "your question"`。这表示交互中用户的角色被标识为`"user"`，并且用户的输入内容为`"your question"`。这种格式化的数据结构允许API清晰地解析出发送者的角色和对话内容，从而进行有效的处理和响应。

要发送到 GPT 模型的问题必须包含在字典列表中。我们使用OpenAI的Python SDK来调用GPT 模型以获取聊天回复，来看代码实现：

```
ChatCompletionMessage(content='你好！我是ChatGPT，由OpenAI开发的人工智能语言模型。我的主要功能是理解和生成自然语言文本，可以帮助回答各种问题、提供建议、撰写文章、翻译语言等')
```

当大模型处理完用户的输入，它会返回一个响应对象。此时我们便可以从响应体中提取模型生成的消息内容。

```
completion
```

上述的交互过程如下图所示。

![](img/5c72cae0a6022d917ac2a865764e4322.png)

```
completion.choices[0].message
```

```
ChatCompletionMessage(content='你好！我是ChatGPT，由OpenAI开发的人工智能语言模型。我的主要功能是理解和生成自然语言文本，可以帮助回答各种问题、提供建议、撰写文章、翻译语言等')
```

```
print(completion.choices[0].message.content)
```

你好！我是ChatGPT，由OpenAI开发的人工智能语言模型。我的主要功能是理解和生成自然语言文本，可以帮助回答各种问题、提供建议、撰写文章、翻译语言等等。无论你是在学习、工作还是日常生活中遇到任何问题，都可以随时向我咨询。我会尽力为你提供准确、有用的信息和帮助。很高兴认识你！

接下来，围绕这种一问一答的逻辑，我们就可以构建非常简单的 `While` 循环来生成一个持续对话的交互程序，代码如下所示：

```
2(H - y) + 4y = L \\
2H - 2y + 4y = L \\
2H + 2y = L \\
2y = L - 2H \\
y = \frac{L - 2H}{2}
\]

- 有了 `y`，再代回 `x = H - y` 可得 `x`。
```

## 示例

### 题目

笼子里有35个头，94条腿，问有多少只鸡和兔？

### 解答

1. 设鸡的数量为 `x` 只，兔的数量为 `y` 只。
2. 根据头的数量：

   $$
   x + y = 35
   $$

3. 根据腿的数量：

   $$
   2x + 4y = 94
   $$

4. 解第一个方程得：

   $$
   x = 35 - y
   $$

5. 将 `x = 35 - y` 代入第二个方程：

   $$
   2(35 - y) + 4y = 94 \\
   70 - 2y + 4y = 94 \\
   70 + 2y = 94 \\
   2y = 24 \\
   y = 12
   $$

6. 将 `y = 12` 代入 `x = 35 - y`：

   $$
   x = 35 - 12 = 23
   $$

**答案**：笼中有23只鸡和12只兔。

## 小结
