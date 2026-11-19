
# 食用 · 清言 API

> 来源：[https://v11enp9ok1h.feishu.cn/docx/NGzbdZ02woGziQxA2PNcpuV1njc](https://v11enp9ok1h.feishu.cn/docx/NGzbdZ02woGziQxA2PNcpuV1njc)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

![](img/b50c5ec16d28772d0cae6bd6d44aa73a.png)

> 获取API KEY地址：https://chatglm.cn/developersPanel

> github入门代码：https://github.com/MetaGLM/glm-cookbook/blob/main/glms/api/glms_api.ipynb

# 简介

具备开发能力的创作者可以[通过API](https://chatglm.cn/developersPanel)调用的方式使用智能体，方便将智能体应用于更多业务场景中。API基本覆盖清言C端页面所有功能，包含文本对话，文生图，图片解读，联网搜索，文档解析，Python代码执行，外部API调用等。接入前请通过创作者中心获取`api_key`及`api_secret`。当前仅支持通过API方式调用创作者自己创建的智能体。

# 基础信息

服务根路径：`https://chatglm.cn/chatglm/assistant-api/v1/`

鉴权方式：header中发送 `Authorization: Bearer {access_token}`。Token获取方式参考下方Access Token获取接口。

请求体格式：JSON

返回格式（除`stream`接口外）：JSON

返回结构体：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `status` | int | Y | 是否成功，0 -> 成功，其他代表失败，默认错误返回1。具体错误码见接口说明。 |
| `message` | string | N | 错误信息 |
| `result` | `Dict` | N | 返回数据 |

# 用量控制

为避免API调用占用过多资源，系统限制每个开发者（非`API_KEY`）的并发数量。默认并发数为2。并发限制范围包括上传文件和会话调用API接口。

此外，我们对每个开发者调用会话接口的次数也设置了上限，目前为一天500次，总量5000次。

# 接口列表

## Access Token获取

请求路径和方式：`POST /get_token`

输入：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `api_key` | string | Y | 开发者ID |
| `api_secret` | string | Y | 开发者秘钥 |

输出数据：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `access_token` | string | Y | 接口鉴权token |
| `expires_in` | int | Y | token过期时间,单位秒。一次授权有效期为10天。请注意在token过期前更新。 |

异常说明：

| 异常情况 | http码 | status码 |
| --- | --- | --- |
| `api_key`被禁用 | 403 | 1001 |
| `api_key`或`api_secret`验证失败 | 401 | 1002 |

## Assistant 会话调用

请求路径和方式：`POST /stream`

输入：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `assistant_id` | string | Y | 智能体 ID，浏览器打开智能体对话页后，可通过地址栏查看。下图红框中即为ID。 |
| `conversation_id` | string | N | 会话 ID，不传默认创建新会话。需要继续之前的会话时，传入流式输出`Result`中的`conversation_id`值。对话轮数和上下文长度无限制，需要注意上下文过大会导致响应耗时增加。 |
| `prompt` | string | Y | prompt文本 |
| `file_list` | `List<Dict>` | N | 文件列表，传入文件上传接口返回的`file_id`。`[{"file_id": "xxx"}]` |
| `meta_data` | `Dict` | N | 元信息，拓展字段 |

输出：

SSE 流式输出

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `message` | `Result` | Y | SSE返回json格式消息体. |

### 输出Result结构

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `history_id` | string | Y | 历史记录ID |
| `conversation_id` | string | Y | 对话上下文ID |
| `message` | `Message` | Y | 输出消息体, 每次输出一个`message`对象，前一个`message`结束后，输出下一个`message` |
| `created_at` | string | Y | 创建时间，格式：“2023-11-23 18:00:00” |
| `status` | string | Y | `init` -> 初始态, `processing` -> 生成中 `finish` -> 生成结束,`error` -> 生成异常。 |
| `last_error` | dict | N | 异常信息 `{"error_code": "", "error_msg": ""}` |
| `meta_data` | dict | N | `{}` |

### 输出Message结构

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `role` | string | Y | assistant为模型输出，tool为工具输出 |
| `content` | `Content` | Y | 输出内容，参考下方Content结构。 |
| `status` | string | Y | `init`/`processing`/`finish`/`error`，状态流转逻辑与外层Result相同。 |
| `created_at` | string | Y | 时间戳。格式参考：“2023-11-23 18:00:00” |
| `meta_data` | dict | N | `{}` |

### 输出Content结构

根据不同的Tool调用情况，返回的content结构不同，参考如下：

| Tool使用情况 | 返回结构说明 |
| --- | --- |
| 文本 | 返回 `{"type": "text", "text": "你好"}` |
| CogView画图 | 返回包含 `type`、`image` 及 `image_url` 的图片结构 |
| Python代码执行 | 返回代码执行开始、成功或失败的结构 |
| OPEN-URL链接打开 | 返回浏览器工具调用开始、成功或失败的结构 |

文本返回结构：

```json
{
  "type": "text",
  "text": "你好"
}
```

CogView画图返回结构：

```json
{
  "type": "image",
  "image": [
    {
      "image_url": "https://sfile.chatglm.cn/..."
    }
  ]
}
```

Python代码执行返回结构：

```
代码执行-开始 {
  "type": "code",
  "code": "# Calculating the square of 10\n10 ** 2"
}
代码执行-成功，结果返回 {
  "type": "execution_output",
  "content": "100"
}
代码执行-失败 {
  "type": "system_error",
  "content": "sandbox error code: 504"
}
```

OPEN-URL链接打开返回结构：

```
OPEN-URL开始 {
  "type": "tool_calls",
  "tool_calls": {
    "name": "browser",
    "arguments": "open_url(\"https://www.chatglm.com/\")"
  }
}
OPEN-URL结束-成功 {
  "type": "browser_result",
  "content": "{\"title\": \" Split string with multiple delimiters in Python - Stack Overflow\", \"url\": \" https://stackoverflow.com/questions/4998629/split-string-with-multiple-delimiters-in-python\"}
}
OPEN-URL结束-失败 {
  "type": "system_error",
  "content": "Error when executing command "}
],
  "meta_data":{
    "failedCommand": "open_url(\"https://www.chatglm.com/\")"
  }
}
```



## Search 联网搜索

### Search 开始
```
{
  "type": "tool_calls",
  "tool_calls": {
    "name": "browser",
    "arguments": "search(\"河南河北山西霰雪预警 2024\", recency_days=30)"
  }
}
```

### Search 结束-成功
```
{
  "type": "browser_result",
  "content": "{}",
  "meta_data": {
    "metadata_list": [
      {
        "type": "webpage",
        "title": "2024年1-2月份山西省进出口情况分析 - 昆明海关",
        "url": "http://kunming.customs.gov.cn/taiyuan_customs/zfxxgk50/3023433/3023453/4676174/5755479/index.html",
        "text": "比去年同期（下同）增长26.8%。全省活跃企业数明显增加前2个月。占全省进出口总值的40.3%。国家",
        "pub_date": ""
      }
    ]
  }
}
```

### Search 结束-失败
```
{
  "type": "system_error",
  "content": "Error when executing command "],
  "meta_data": {
    "failedCommand": "search(\"河南河北山西霰雪预警 2024\", recency_days=30)"
  }
}
```

### mclick 搜索结果页面打开 开始
`mclick` 中参数为打开链接序号列表，关联搜索结果。

```
{
  "type": "tool_calls",
  "tool_calls": {
    "name": "browser",
    "arguments": "mclick([0, 1, 3])"
  },
  "meta_data": {
    "metadata_list": [
      {
        "type": "webpage",
        "title": "2024年1-2月份山西省进出口情况分析 - 昆明海关",
        "url": "http://kunming.customs.gov.cn/taiyuan_customs/zfxxgk50/3023433/3023453/4676174/5755479/index.html",
        "text": "比去年同期（下同）增长26.8%。全省活跃企业数明显增加前2个月。占全省进出口总值的40.3%。国家",
        "pub_date": ""
      }
    ]
  }
}
```

### mclick 搜索结果页面打开 成功
所有链接均完成后，返回 `quote_result`。

```
{
  "type": "quote_result",
  "content": "{}",
  "meta_data": {
    "metadata_list": [
      {
        "type": "webpage",
        "title": "2024年1-2月份山西省进出口情况分析 - 昆明海关",
        "url": "http://kunming.customs.gov.cn/taiyuan_customs/zfxxgk50/3023433/3023453/4676174/5755479/index.html",
        "text": "比去年同期（下同）增长26.8%。全省活跃企业数明显增加前2个月。占全省进出口总值的40.3%。国家",
        "pub_date": ""
      }
    ]
  }
}
```

### mclick 搜索结果页面打开 失败
每个 URL 失败返回一次该事件，可能会出现部分失败。

```
{
  "type": "system_error",
  "content": "",
  "meta_data": {
    "failedURL": "https://xxx.xx"
  }
}
```

## 外部 API 调用

### 请求 API 的参数内容
```
{
  "type": "tool_calls",
  "tool_calls": {
    "name": "generate",
    "arguments": "```python\ntool_call(Content-Type='application/json', title='Hello World', type='post', platform='wordpress')\n```",
    "host": "ppt-test-v101.wxbjq.top"
  }
}
```

### 请求 API 的返回内容
```
{
  "type": "function_result",
  "content": "{\"status\":0,\"message\":\"success\",\"result\":{\"count\":818},\"rid\":\"2030521d-b4ea-4c38-854f-f3f189a3ebb3\"}"
}
```

## 知识库检索（需在智能体配置中打开“显示相关的知识库段落”）

### 检索开始
```
{
  "type": "rag_slices",
  "content": []
}
```

### 检索结果
```
{
  "type": "rag_slices",
  "content": [
    {
      "text": "知识库内容abcd", # 只有 rag 模式会返回
      "document_name": "1.pdf"
    }
  ]
}
```

## 异常说明

| 异常情况 | http 码 | status 码 |
| --- | --- | --- |
| `api_key`被禁用 | 403 | 10004 |
| `api_key`已删除 | 400 | 10003 |
| `FileList` 中文件不存在 | 400 | 10005 |
| 当日会话调用次数超限 | 403 | 10008 |
| 实时并发数超限 | 403 | 10007 |
| 智能体被删除 | 403 | 10010 |
| 无权限调用智能体 | 403 | 10018 |

## 流式异常说明

| 异常情况 | `error_code` 码 |
| --- | --- |
| 安全风控拦截 | 10031 |
| 生成中断（网络原因） | 10024 |
| 生成错误（模型原因） | 10027 |
| 生成异常（系统原因） | 10025 |
| 生成图片失败 | 10028 |

## Assistant 会话调用（非流式输出）

请求路径和方式：`POST /stream_sync`

### 输入

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `assistant_id` | `string` | Y | 智能体 ID，浏览器打开智能体对话页后，可通过地址栏查看。下图红框中即为 ID。 |
| `conversation_id` | `string` | N | 会话 ID，不传默认创建新会话。需要继续之前的会话时，传入流式输出 `Result` 中的 `conversation_id` 值。对话轮数和上下文长度无限制，需要注意上下文过大会导致响应耗时增加。 |
| `prompt` | `string` | Y | `prompt` 文本 |
| `file_list` | `List<Dict>` | N | 文件列表，传入文件上传接口返回的 `file_id`。`[{"file_id":"xxx"}]` |
| `meta_data` | `Dict` | N | 元信息，拓展字段 |

### 输出

HTTP response JSON 输出

`https://zhipu-ai.feishu.cn/sync/Bz6sdk1QbsIbnjbX3nBcLOSMn8b`

### 输出 Result 结构

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `history_id` | `string` | Y | 历史记录 ID |
| `conversation_id` | `string` | Y | 对话上下文 ID |
| `output` | `List<Part>` | Y | 输出消息体列表，每个 `Part` 对应流式中的一个 `Message` |
| `status` | `string` | Y | `init` -> 初始态, `processing` -> 生成中 `finish` -> 生成结束, `error` -> 生成异常。 |

### 输出 Part 结构

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `role` | `string` | Y | `assistant` 为模型输出，`tool` 为工具输出 |
| `content` | `Content` | Y | 输出内容，参考下方 `Content` 结构。 |
| `status` | `string` | Y | `init`/`processing`/`finish`/`error`，状态流转逻辑与外层 `Result` 相同。 |
| `created_at` | `string` | Y | 时间戳。格式参考：“2023-11-23 18:00:00” |
| `meta_data` | `dict` | N | `{}` |

### 输出 Content 结构

根据不同的 `Tool` 调用情况，返回的 `content` 结构不同，参考如下：

| Tool 使用情况 | 返回结构 |
| --- | --- |
| | ```
{
  "type": "text",
  "text": "你好"
}
``` |

| `CogView` 画图 | ```
{
  "type": "image",
  "image": [
    {
      "image_url": "https://sfile.chatglm.cn/..."
    }
  ]
}
``` |

| `Python` 代码执行 | ```
代码执行-开始
{
  "type": "code",
  "code": "# Calculating the square of 10\n10 ** 2"
}
代码执行-成功，结果返回
{
  "type": "execution_output",
  "content": "100"
}
代码执行-失败
{
  "type": "system_error",
  "content": "sandbox error code: 504"
}
``` |



# API 接口与工具调用格式说明

## 工具调用

### OPEN-URL 链接打开

```
OPEN-URL开始 {
    "type": "tool_calls",
    "tool_calls": {
        "name": "browser",
        "arguments": "open_url(\"https://www.chatglm.com/\")"
    }
}
OPEN-URL结束-成功 {
    "type": "browser_result",
    "content": "{\"title\": \" Split string with multiple delimiters in Python - Stack Overflow\", \"url\": \" https://stackoverflow.com/questions/4998629/split-string-with-multiple-delimiters-in-python\"}",
}
OPEN-URL结束-失败 {
    "type": "system_error",
    "content": "Error when executing command "}],
    "meta_data": {
        "failedCommand": "open_url(\"https://www.chatglm.com/\")"
    }
}
```

### Search 联网搜索

```
Search 开始 {
    "type": "tool_calls",
    "tool_calls": {
        "name": "search",
        "arguments": "search(\"河南河北山西霰雪预警 2024\", recency_days=30)"
    }
}
Search 结束-成功 {
    "type": "browser_result",
    "content": "{}",
    "meta_data": {
        "metadata_list": [
            {
                "type": "webpage",
                "title": "2024年1-2月份山西省进出口情况分析 - 昆明海关",
                "url": "http://kunming.customs.gov.cn/taiyuan_customs/zfxxgk50/3023433/3023453/4676174/5755479/index.html",
                "text": "比去年同期（下同）增长26.8%。全省活跃企业数明显增加前2个月。占全省进出口总值的40.3%。国家",
                "pub_date": ""
            }
        ]
    }
}
Search 结束-失败 {
    "type": "system_error",
    "content": "Error when executing command "}],
    "meta_data": {
        "failedCommand": "search(\"河南河北山西霰雪预警 2024\", recency_days=30)"
    }
}
mclick 搜索结果页面打开 开始. mclick中参数为打开链接序号列表，关联搜索结果 {
    "type": "tool_calls",
    "tool_calls": {
        "name": "browser",
        "arguments": "mclick([0, 1, 3])"
    },
    "meta_data": {
        "metadata_list": [
            {
                "type": "webpage",
                "title": "2024年1-2月份山西省进出口情况分析 - 昆明海关",
                "url": "http://kunming.customs.gov.cn/taiyuan_customs/zfxxgk50/3023433/3023453/4676174/5755479/index.html",
                "text": "比去年同期（下同）增长26.8%。全省活跃企业数明显增加前2个月。占全省进出口总值的40.3%。国家",
                "pub_date": ""
            }
        ]
    }
}
mclick 搜索结果页面打开 成功. 所有链接均完成后，返回 quote_result. {
    "type": "quote_result",
    "content": "{}",
    "meta_data": {
        "metadata_list": [
            {
                "type": "webpage",
                "title": "2024年1-2月份山西省进出口情况分析 - 昆明海关",
                "url": "http://kunming.customs.gov.cn/taiyuan_customs/zfxxgk50/3023433/3023453/4676174/5755479/index.html",
                "text": "比去年同期（下同）增长26.8%。全省活跃企业数明显增加前2个月。占全省进出口总值的40.3%。国家",
                "pub_date": ""
            }
        ]
    }
}
mclick 搜索结果页面打开 失败. 每个URL失败返回一次该事件，可能会出现部分失败. {
    "type": "system_error",
    "content": ""，
    "meta_data": {
        "failedURL": "https://xxx.xx"
    }
}
```

### 外部 API 调用

请求 API 的参数内容：

````
{
    "type": "tool_calls",
    "tool_calls": {
        "name": "generate",
        "arguments": "```python\ntool_call(Content-Type='application/json', title='Hello World', type='post', platform='wordpress')\n```",
        "host": "ppt-test-v101.wxbjq.top"
    }
}
````

请求 API 的返回内容：

```
{
    "type": "function_result",
    "content": "{\"status\":0,\"message\":\"success\",\"result\":{\"count\":818},\"rid\":\"2030521d-b4ea-4c38-854f-f3f189a3ebb3\"}"
}
```

### 知识库检索

> 注：需在智能体配置中打开“显示相关的知识库段落”。

检索开始：

```
{
    "type": "rag_slices",
    "content": []
}
```

检索结果：

```
{
    "type": "rag_slices",
    "content": [
        {
            "text": "知识库内容abcd",
            "document_name": "1.pdf"
        }
    ]
}
```

## 异常说明

| 异常情况 | http 码 | status 码 |
| --- | --- | --- |
| `api_key` 被禁用 | 403 | 10004 |
| `api_key` 已删除 | 400 | 10003 |
| `FileList` 中文件不存在 | 400 | 10005 |
| 当日会话调用次数超限 | 403 | 10008 |
| 实时并发数超限 | 403 | 10007 |
| 智能体被删除 | 403 | 10010 |
| 无权限调用智能体 | 403 | 10018 |

## Assistant 对话文件上传

上传文件，输入为文件流，输出 `file_id` 用于会话调用时使用。文件大小限制为 100MB。目前支持的业务场景与文件类型对应参考下表：

| 业务场景 | 文件类型 |
| --- | --- |
| 图片解析 | `jpg`, `png`, `jpeg`, `webp`, `gif` |
| 文档解读 | `pdf`, `docx`, `doc`, `txt`, `pptx`, `md`, `epub`, `epub.zip` |
| 代码执行（`Code Interpreter`） | 上传其他所有类型文件，或在图片与文档文件混合使用时。 |

请求路径和方式：`POST` form-data `/file_upload`

输入：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `file` | file | Y | 文件流。仅支持上传单一文件。 |

输出：

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `file_id` | string | Y | 文件 ID |
| `file_name` | String | Y | 文件名 |

### 异常说明

| 异常情况 | http 码 | status 码 |
| --- | --- | --- |
| `api_key` 被禁用 | 403 | 10004 |
| `api_key` 已删除 | 400 | 10003 |
| 实时并发数超限 | 403 | 10006 |
| 文件上传失败 | 500 | 11003 |
| 文档解析失败 | 500 | 11004 |
| 文档字数超限（上限 12 万 Token） | 400 | 11005 |
| 文件大小超限 | 400 | 11006 |
| 文件处理失败 | 500 | 11009 |

## 下一步问题建议

请求路径和方式：

`POST` `chatglm/assistant-api/v1/suggest/prompts`

输入：

```
{
    "conversation_id": "66cd9eea71105488bddd5d5f",
    "log_id": "66cd9eea71105488bddd5d63"
}
```

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `conversation_id` | string | Y | 会话 ID，默认会给该会话的最后一条记录创建下一步问题建议 |
| `log_id` | string | N | 获取之前某条 `log_id` 的生成结果，如果未生成过，会返回空，如果传了该值，会走缓存的问题建议 |

输出：

HTTP response JSON 输出

```
{
    "message": "success",
    "result": {
        "list": [
            "谢谢老板",
            "我知道了",
            "已经搞定了，请你放心。"
        ]
    },
    "status": 0
}
```

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `list` | string | Y | 下一步问题建议的列表 |

### 异常说明

| 异常情况 | http 码 | status 码 |
| --- | --- | --- |
| `api_key` 被禁用 | 403 | 10004 |
| `api_key` 已删除 | 400 | 10003 |
| Conversation 未找到 | 400 | 12001 |
| Conversation 不属于该用户 | 401 | 12002 |
