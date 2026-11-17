
# 工作流流式响应事件

```
id: 0
event: Message
data: {"content":"msg","node_is_finish":false,"node_seq_id":"0","node_title":"Message"}

id: 1
event: Message
data: {"content":"为","node_is_finish":false,"node_seq_id":"1","node_title":"Message"}

id: 2
event: Message
data: {"content":"为什么小明要带一把尺子去看电影？\n因","node_is_finish":false,"node_seq_id":"2","node_title":"Message"}

id: 3
event: Message
data: {"content":"为他听说电影很长，怕","node_is_finish":false,"node_seq_id":"3","node_title":"Message"}

id: 4
event: Message
data: {"content":"坐不下！","node_is_finish":true,"node_seq_id":"4","node_title":"Message"}

id: 5
event: Message
data: {"content":"{\"output\":\"为什么小明要带一把尺子去看电影？\\n因为他听说电影很长，怕坐不下！\"}","cost":"0.00","node_is_finish":true,"node_seq_id":"0","node_title":"","token":0}

id: 6
event: Done
data: {}
```

## Error 事件

```
id: 0
event: Error
data: {"error_code":4000,"error_message":"Request parameter error"}
```

## Interrupt 事件

```
// 流式执行工作流，触发问答节点，Bot提出问题
id: 0
event: Message
data: {"content":"请问你想查看哪个城市、哪一天的天气呢","content_type":"text","node_is_finish":true,"node_seq_id":"0","node_title":"问答"}

id: 1
event: Interrupt
data: {"interrupt_data":{"data":"","event_id":"7404830425073352713/2769808280134765896","type":2},"node_title":"问答"}
```

# 恢复运行工作流

恢复运行已中断的工作流。

## 接口说明

执行包含问答节点的工作流时，智能体会以指定问题向用户提问，并等待用户回答。此时工作流为中断状态，开发者需要调用此接口回答问题，并恢复运行工作流。如果用户的响应和智能体预期提取的信息不匹配，例如缺少必选的字段，或字段数据类型不一致，工作流会再次中断并追问。如果询问 3 次仍未收到符合预期的回复，则判定为工作流执行失败。

## 限制说明

+   最多调用此接口恢复运行 3 次，如果第三次恢复运行时智能体仍未收到符合预期的回复，则判定为工作流执行失败。

+   恢复运行后，`index` 和节点 `index` 都会重置。

+   恢复运行工作流也会产生 `token` 消耗，且与`执行工作流（流式响应）`时消耗的 `token` 数量相同。

## 基础信息

| 请求方式 | `POST` |

| --- | --- |

| 请求地址 | `https://{{host}}/v1/workflow/stream_resume` |

| 接口说明 | 恢复运行已中断的工作流。 |

### Header

| 参数 | 取值 | 说明 |

| --- | --- | --- |

| Authorization | `Bearer $Access_Token` | 用于验证客户端身份的`个人访问令牌`。你可以在扣子平台中生成个人访问令牌，详细信息，参考【准备工作】。 |

| Content-Type | `application/json` | 解释请求正文的方式。 |

### Body

| 参数 | 类型 | 是否必选 | 说明 |

| --- | --- | --- | --- |

| `workflow_id` | `String` | 必选 | 待执行的 Workflow ID，此工作流应已发布。  进入 `Workflow` 编排页面，在页面 URL 中，`workflow` 参数后的数字就是 Workflow ID。例如 `https://www.coze.com/work_flow?space_id=42463***&workflow_id=73505836754923***`，Workflow ID 为 `73505836754923***`。 |

| `event_id` | `String` | Required | 工作流执行中断事件 ID。你可以从`执行工作流（流式响应）`的响应信息中获得中断事件 ID。 |

| `resume_data` | `String` | Required | 恢复执行时，用户对智能体指定问题的回复。  如果是问答节点导致的中断，回复中应包含问答节点中的必选参数，否则工作流会再次中断并提问。 |

| `interrupt_type` | `Integer` | Required | 中断类型，你可以从`执行工作流（流式响应）`的响应信息中获得中断时间的中断类型。 |

## 返回结果

在流式响应中，开发者需要注意是否存在丢包现象。

+   事件 ID（`id`）默认从 0 开始计数，以包含 `event: Done` 的事件为结束标志。开发者应根据 `id` 确认响应消息整体无丢包现象。

+   `Message` 事件的消息 ID 默认从 0 开始计数，以包含 `node_is_finish : true` 的事件为结束标志。开发者应根据 `node_seq_id` 确认 `Message` 事件中每条消息均完整返回，无丢包现象。

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `id` | `Integer` | 此消息在接口响应中的事件 ID。以 0 为开始。 |

| `event` | `String` | 当前流式返回的数据包事件。包括以下类型：  * `Message`：工作流节点输出消息，例如消息节点、结束节点的输出消息。可以在 `data` 中查看具体的消息内容。  * `Error`：报错。可以在 `data` 中查看 `error_code` 和 `error_message`，排查问题。  * `Done`：结束。表示工作流执行结束，此时 `data` 中包含 debug URL。  * `Interrupt`：中断。表示工作流中断，此时 `data` 字段中包含具体的中断信息。  * `PING`：心跳信号。表示工作流执行中，消息内容为空，用于维持连接。 |

| `data` | `Object` | 事件内容。各个 `event` 类型的事件内容格式不同。 |

### Message 事件

`Message` 事件中，`data` 的结构如下：

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `content` | `String` | 流式输出的消息内容。 |

| `node_title` | `String` | 输出消息的节点名称，例如消息节点、结束节点。 |

| `node_seq_id` | `String` | 此消息在节点中的消息 ID，从 0 开始计数，例如消息节点的第 5 条消息。 |

| `node_is_finish` | `Boolean` | 当前消息是否为此节点的最后一个数据包。 |

| `ext` | `Map[String]String` | 额外字段。 |

| `cost` | `String` | 预留字段，无需关注。 |

### Interrupt 事件

`Interrupt` 事件中，`data` 的结构如下：

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `interrupt_data` | `Object` | 中断控制内容。 |

| `interrupt_data.event_id` | `String` | 工作流中断事件 ID，恢复运行时应回传此字段。 |

| `interrupt_data.type` | `Integer` | 工作流中断类型，恢复运行时应回传此字段。 |

| `node_title` | `String` | 输出消息的节点名称，例如“问答”。 |

### Error 事件

`Error` 事件中，`data` 的结构如下：

| 参数名 | 参数类型 | 参数描述 |

| --- | --- | --- |

| `error_code` | `Integer` | 调用状态码。  * `0` 表示调用成功。  * 其他值表示调用失败。你可以通过 `error_message` 字段判断详细的错误原因。 |

| `error_message` | `String` | 状态信息。API 调用失败时可通过此字段查看详细错误信息。 |

## 示例

工作流执行中断场景下，以查看天气工作为例，完整的接口调用示例如下。

1.  调用接口`执行工作流（流式响应）`，要求查看天气。

请求示例如下：

```
curl --location 'https://{{host}}/v1/workflow/stream_run' \
--header 'Authorization: Bearer pat_vTG1****' \
--header 'Content-Type: application/json' \
--data '{
    "workflow_id": "739739507914235****",
    "parameters": "{\"BOT_USER_INPUT\":\"查看天气\"}"
}'
```

1.  触发问答节点，工作流中断，响应信息中返回智能体提出的问题，要求用户提供城市和日期。

返回示例如下：

```
id: 0
event: Message
data: {"content":"请问你想查看哪个城市、哪一天的天气呢","content_type":"text","node_is_finish":true,"node_seq_id":"0","node_title":"问答"}

id: 1
event: Interrupt
data: {"interrupt_data":{"data":"","event_id":"7404831988202520614/6302059919516746633","type":2},"node_title":"问答"}
```

1.  调用接口恢复运行工作流，回复智能体城市和日期。

请求示例如下：

```
curl --location 'https://{{host}}/v1/workflow/stream_resume' \
--header 'Authorization: Bearer pat_vTG1****' \
--header 'Content-Type: application/json' \
--data '{
    "event_id":"740483727529459****/433802199567434****",
    "interrupt_type":2,
    "resume_data":"杭州，2024-08-20",
    "workflow_id":"739739507914235****"
}'
```

1.  工作流执行完毕，完成天气查询，返回工作流输出消息。

返回示例如下：

```
id: 0
event: Message
data: {"content":"{\"output\":[{\"condition\":\"中到大雨\",\"humidity\":72,\"predict_date\":\"2024-08-20\",\"temp_high\":35,\"temp_low\":26,\"weather_day\":\"中到大雨\",\"wind_dir_day\":\"西风\",\"wind_dir_night\":\"西风\",\"wind_level_day\":\"3\",\"wind_level_night\":\"3\"}]}","content_type":"text","cost":"0","node_is_finish":true,"node_seq_id":"0","node_title":"End","token":386}

id: 1
event: Done
data: {}
```

# 项目架构

Coze Studio 基于领域驱动设计（DDD）原则架构设计，项目整体结构如下：

```
├── backend/              # 后端服务
│   ├── api/              # API 处理器和路由
│   ├── application/      # 应用层，组合领域对象和基础设施实现
│   ├── conf/             # 配置文件
│   ├── crossdomain/      # 跨领域防腐层
│   ├── domain/           # 领域层，包含核心业务逻辑
│   ├── infra/            # 基础设施实现层
│   ├── pkg/              # 无外部依赖的工具方法
│   └── types/            # 类型定义
├── common/               # 公共组件
├── docker/               # Docker 配置
├── frontend/             # 前端应用
│   ├── apps/             # 应用程序
│   ├── config/           # 配置文件
│   ├── infra/            # 基础设施
│   └── packages/         # 包
├── idl/                  # 接口定义语言文件
```

+   API 层（`/backend/api`）：实现 HTTP 端点，使用 `Hertz` 服务器、处理请求/响应处理、包含中间件组件

+   应用层（`/backend/application`）：组合各种领域对象和基础设施实现、提供 API 服务、封装 API

+   领域层（`/backend/domain`）：包含核心业务逻辑、定义领域实体和值对象、实现业务规则和工作流

+   跨领域防腐层（`/backend/crossdomain`）：定义跨领域接口、实现跨领域接口、防止领域间直接依赖
