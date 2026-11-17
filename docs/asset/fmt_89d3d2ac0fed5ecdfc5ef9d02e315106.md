
# 获取会话消息列表

## 请求参数

| 参数 | 类型 | 是否必选 | 示例 | 说明 |
| --- | --- | --- | --- | --- |
| `before_id` | String | 可选 | `737363834493437****` | 查看指定位置之前的消息。默认为 `0`，表示不指定位置。如需向前翻页，则指定为返回结果中的 `first_id`。 |

| `after_id` | String | 可选 | `737363834493437****` | 查看指定位置之后的消息。默认为 `0`，表示不指定位置。如需向后翻页，则指定为返回结果中的 `last_id`。 |

| `limit` | Long | 可选 | `30` | 每次查询返回的数据量。默认为 `50`，取值范围为 `1~50`。 |

## 返回参数

| 参数 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |

| `data` | Array of `OpenMessageApi` | `[ { &quot;bot_id&quot;: &quot;&quot;, &quot;chat_id&quot;: &quot;&quot;, &quot;content&quot;: &quot;你的名字叫什么&quot;, &quot;content_type&quot;: &quot;text&quot;, &quot;conversation_id&quot;: &quot;737363834493434****&quot;, &quot;created_at&quot;: 1716809829, &quot;id&quot;: &quot;737363834493437****&quot;, &quot;meta_data&quot;: {}, &quot;role&quot;: &quot;user&quot;, &quot;type&quot;: &quot;&quot;, &quot;updated_at&quot;: &quot;1716809829&quot; } ]` | 消息详情。 |

| `has_more` | Boolean | `true` | 是否已返回全部消息。  * `true`：未返回全部消息，可再次调用此接口查看其他分页。  * `false`：已返回全部消息。 |

| `first_id` | String | `737363834493437****` | 返回的消息列表中，第一条消息的 Message ID。 |

| `last_id` | String | `737363834493440****` | 返回的消息列表中，最后一条消息的 Message ID。 |

| `code` | Long | `0` | 调用状态码。`0` 表示调用成功，其他值表示调用失败，你可以通过 `msg` 字段判断详细的错误原因。 |

| `msg` | String | `Success` | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

### `OpenMessageApi`

| 参数 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |

| `id` | String | `738130009748252****` | Message ID，即消息的唯一标识。 |

| `conversation_id` | String | `737999610479815****` | 此消息所在的会话 ID。 |

| `bot_id` | String | `747363834493437****` | 编写此消息的智能体 ID。此参数仅在对话产生的消息中返回。 |

| `chat_id` | String | `757363834493437****` | Chat ID。此参数仅在对话产生的消息中返回。  不同的对话中，系统会生成新的 `chat_id`。同一个用户在第一次对话和第二次对话时，`chat_id` 不一样。 |

| `meta_data` | JSON Map | `{}` | 创建消息时的附加消息，获取消息时也会返回此附加消息。 |

| `role` | String | `user` | 发送这条消息的实体。取值：  * `user`：代表该条消息内容是用户发送的。  * `assistant`：代表该条消息内容是 Bot 发送的。 |

| `content` | String | 早上好，今天星期几？ | 消息的内容，支持纯文本、多模态（文本、图片、文件混合输入）、卡片等多种类型的内容。 |

| `content_type` | String | `text` | 消息内容的类型，取值包括：  * `text`：文本。  * `object_string`：多模态内容，即文本和文件的组合、文本和图片的组合。  * `card`：卡片。此枚举值仅在接口响应中出现，不支持作为入参。 |

| `created_at` | Long | `1718592898` | 消息的创建时间，格式为 10 位的 Unixtime 时间戳，单位为秒（s）。 |

| `updated_at` | Long | `1718592898` | 消息的更新时间，格式为 10 位的 Unixtime 时间戳，单位为秒（s）。 |

| `type` | String | `question` | 消息类型。  * `question`：用户输入内容。  * `answer`：智能体返回给用户的消息内容，支持增量返回。如果工作流绑定了 `message` 节点，可能会存在多 `answer` 场景，此时可以用流式返回的结束标志来判断所有 `answer` 完成。  * `function_call`：智能体对话过程中调用函数（`function call`）的中间结果。  * `tool_response`：调用工具（`function call`）后返回的结果。  * `follow_up`：如果在 Bot 上配置打开了用户问题建议开关，则会返回推荐问题相关的回复内容。  * `verbose`：多 `answer` 场景下，服务端会返回一个 `verbose` 包，对应的 `content` 为 JSON 格式，`content.msg_type = generate_answer_finish` 代表全部 `answer` 回复完成。  仅发起会话（v3）接口支持将此参数作为入参，且：  * 如果 `autoSaveHistory=true`，`type` 支持设置为 `question` 或 `answer`。  * 如果 `autoSaveHistory=false`，`type` 支持设置为 `question`、`answer`、`function_call`、`tool_output`/`tool_response`。  其中，`type=question` 只能和 `role=user` 对应，即仅用户角色可以且只能发起 `question` 类型的消息。 |

| `section_id` | String | `757363834493437****` | 上下文片段 ID。每次清除上下文都会生成一个新的 `section_id`。 |

| `reasoning_content` | String | 好的，我现在需要给一个13岁的大学生提供学习建议。首先，我得考虑用户的情况…… | DeepSeek-R1 模型的思维链（CoT）。模型会将复杂问题逐步分解为多个简单步骤，并按照这些步骤逐一推导出最终答案。  该参数仅在使用 DeepSeek-R1 模型时才会返回。 |

## 示例

### 请求示例

```
curl --location --request POST {{host}}/v1/conversation/message/list?&amp;conversation_id=737363834493434**** \
--data-raw &#x27;{
    &quot;limit&quot;: null,
    &quot;order&quot;: &quot;asc&quot;,
    &quot;chat_id&quot;: &quot;737363834493437****&quot;,
    &quot;before_id&quot;: &quot;737363834493437****&quot;
}&#x27;
```

### 返回示例

```
{
    &quot;code&quot;: 0,
    &quot;data&quot;: [
        {
            &quot;bot_id&quot;: &quot;737363834493434****&quot;,
            &quot;chat_id&quot;: &quot;747363834493434****&quot;,
            &quot;content&quot;: &quot;你的名字叫什么&quot;,
            &quot;content_type&quot;: &quot;text&quot;,
            &quot;conversation_id&quot;: &quot;737363834493434****&quot;,
            &quot;created_at&quot;: 1716809829,
            &quot;id&quot;: &quot;737363834493437****&quot;,
            &quot;meta_data&quot;: {},
            &quot;role&quot;: &quot;user&quot;,
            &quot;type&quot;: &quot;&quot;,
            &quot;updated_at&quot;: &quot;1716809829&quot;
        },
        {
            &quot;bot_id&quot;: &quot;737363834493434****&quot;,
            &quot;chat_id&quot;: &quot;747363834493434****&quot;,
            &quot;content&quot;: &quot;我的名字叫bot&quot;,
            &quot;content_type&quot;: &quot;text&quot;,
            &quot;conversation_id&quot;: &quot;737363834493434****&quot;,
            &quot;created_at&quot;: &quot;1716809829&quot;,
            &quot;id&quot;: &quot;737363834493440****&quot;,
            &quot;meta_data&quot;: {},
            &quot;role&quot;: &quot;assistant&quot;,
            &quot;type&quot;: &quot;&quot;,
            &quot;updated_at&quot;: &quot;1716936779&quot;
        }
    ],
    &quot;first_id&quot;: &quot;737363834493437****&quot;,
    &quot;has_more&quot;: true,
    &quot;last_id&quot;: &quot;737363834493440****&quot;,
    &quot;msg&quot;: &quot;&quot;

}
```

# 清除上下文

清除指定会话中的上下文。

## 接口说明

在智能体对话场景中，上下文指模型在处理当前输入时所能参考的历史消息、对话记录，它决定了模型对当前任务的理解深度和连贯性，直接影响输出结果的准确性和相关性。多轮对话中，如果需要开启新的话题，可以调用此 API 清除上下文。清除上下文后，对话中的历史消息不会作为上下文被输入给模型，后续对话不再受之前历史消息的影响，避免信息干扰，确保对话的准确性和连贯性。

会话中的消息存储在上下文段落（`section`）中，每次调用此 API 清除上下文时，系统会自动删除旧的上下文段落，并创建新的上下文段落用于存储新的消息。再次发起对话时，新分区中的消息会作为新的上下文传递给模型参考。

+   清除上下文 API 只是清空模型可见的消息历史，不会实际删除会话中的消息，通过查看消息列表等 API 仍能查看到消息内容。

+   仅支持清除本人创建的会话的上下文。

+   [会话、对话](https://www.coze.cn/open/docs/developer_guides/coze_api_overview#fed4a54c)、消息和上下文段落的术语解释请参见基础概念。

![](img/4938a916cc4d79658fdc8890ed46396c.png)

## 基础信息

| 请求方式 | `POST` |

| --- | --- |

| 请求地址 | `{{host}}/v1/conversations/:conversation_id/clear` |

| 接口说明 | 清除指定会话中的上下文。 |

## 请求参数

### Header

| 参数 | 取值 | 说明 |
| --- | --- | --- |

| `Authorization` | `Bearer $Access_Token` | 用于验证客户端身份的个人访问令牌。你可以在扣子平台中生成个人访问令牌，详细信息，参考【准备工作】。 |

| `Content-Type` | `application/json` | 解释请求正文的方式。 |

### Path

| 参数 | 类型 | 是否必选 | 示例 | 说明 |
| --- | --- | --- | --- | --- |

| `conversation_id` | String | 必选 | `737989918257****` | [待清除上下文](https://www.coze.cn/docs/developer_guides/chat_v3)的会话 ID。你可以在发起对话接口的 `Response` 中通过 `conversation_id` 字段获取会话 ID。 |

## 返回参数

| 参数 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |

| `code` | Long | `0` | 调用状态码。`0` 表示调用成功，其他值表示调用失败，你可以通过 `msg` 字段判断详细的错误原因。 |

| `msg` | String | `"Success"` | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

| `data` | Object of `Section` | `{ "id": "1234567890123456789", "conversation_id": "1234567890123456789" }` | 上下文段落（`section` ）的详细信息。  `Section` 是一个独立的上下文段落，用于分隔不同的对话阶段或主题。`Section` 中包括上下文消息，当用户清除上下文时，系统会创建一个新的 `Section`，从而确保新的对话不受历史消息的影响。 |

### `Section`

| 参数 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |

| `id` | String | `1234567890123456789` | Session ID，即清除上下文后新创建的上下文段落（`section`）的唯一标识符。  每个上下文段落对应一批独立的上下文消息。每次清除上下文时，系统会新建一个上下文段落用于存储新的上下文消息。 |

| `conversation_id` | String | `737999610479815****` | Conversation ID，即会话的唯一标识。 |

## 示例

### 请求示例

```
curl --location --request POST {{host}}/v1/conversations/:conversation_id/clear \
--header &#x27;Authorization: Bearer pat_OYDacMzM3WyOWV3Dtj2bHRMymzxP****&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data-raw &#x27;{}&#x27;
```

### 返回示例

```
{
    &quot;code&quot;: 0,
    &quot;msg&quot;: &quot;&quot;,
    &quot;data&quot;: {
        &quot;id&quot;: &quot;12345678****56789&quot;,
        &quot;conversation_id&quot;: &quot;1234567****456789&quot;
    }
}
```

# 执行工作流

执行已发布的工作流。

## 接口说明

此接口为非流式响应模式，对于支持流式输出的节点，应使用接口 `执行工作流（流式响应）` 获取流式响应。调用接口后，你可以从响应中获得 `debug_url`，访问链接即可通过可视化界面查看工作流的试运行过程，其中包含每个执行节点的输入输出等详细信息，帮助你在线调试或排障。

## 限制说明

| 限制项 | 说明 |
| --- | --- |

| 工作流发布状态 | 必须为已发布。执行未发布的工作流会返回错误码 [`4200`](https://www.coze.cn/docs/guides/use_workflow)。创建并发布工作流的操作可参考使用工作流。 |

| 节点限制 | 工作流中不能包含消息节点、开启了流式输出的结束节点、问答节点。 |
