
# 【对外接口文档】聊天接口API

> 来源：[https://xuqiwei1986.feishu.cn/docx/Mq8Hdh1YqoZorEx2cgqcnGOPnof](https://xuqiwei1986.feishu.cn/docx/Mq8Hdh1YqoZorEx2cgqcnGOPnof)

## 简介

ZelinAI应用聊天请求接口，用于应用聊天交互。

## 更新历史

### V1.2.1 版本

**发布时间**

该版本发布于 2023 年 5 月 15 日。

**发布内容**

| 类型 | 说明 |
| --- | --- |
| 新增 | ZelinAI接口文档 |

## 调用方法

### 3.1 前提条件

暂无

### 3.2 请求结构

**服务地址：**`https://zelinai.com/`

**通信协议：**`HTTPS`

### 3.3 公共参数

**公共请求参数**

暂无

**公共返回参数**

| 名称 | 类型 | 说明 |
| --- | --- | --- |
| `status` | `int` | 200-正常；非200-异常 |
| `message` | `string` | `status`状态说明 |
| `data` | `object` | 返回的内容 |

### 3.4 鉴权机制

+ 为了确保接口调用的安全，每个接口在请求的时候，都要附带签名`signature`字段；鉴权逻辑：

+ `signature`签名一致；

+ `timestamp`相差在30s以内（正负30s）；

+ 在请求的`Header`中添加如下参数

| 名称 | 类型 | 是否必填 | 说明 |
| --- | --- | --- | --- |
| `appkey` | `string(32)` | 是 | 业务方`appkey` |
| `nonce` | `string(16)` | 是 | 长度为16的随机字符串 |
| `timestamp` | `long` | 是 | 当前时间戳，单位秒 |
| `signature` | `string` | 是 | 签名字符串，加签方法见下文 |

+ 注：在构造签名的字符串中，除了上述`appkey`、`nonce`、`timestamp`之外，可能还包括其他参数，具体看各个接口请求参数的“签名参数”列，值为“是”的都需要纳入进来一起加签；

+ 计算签名的方法如下：

+ 签名字段包含`appkey`、`nonce`和`timestamp`，还有接口包含的其他业务参数；

+ 所有待签名参数按照字段名的ASCII 码从小到大排序（字典序），然后使用 URL 键值对的格式（即`key1=value1&key2=value2…`）拼接成字符串`s1`。（注：值为空的不计入签名；）

+ 在 `s1` 字符串后面直接拼接 `appsecret` 作为签名字符串，即`s1{appsecret}`；

+ 对签名字符串调用`MD5`签名，生成`signature`字符串，`MD5(s1{appsecret})`

+ 注意：`md5`结果值需要转换成小写

+ 注：`appkey`、`appsecret`是业务方唯一标示，由平台生成，请在控制台后台-系统设置里面获取；

## 相关接口

### 4.1 聊天接口（同步返回）

**功能简述**

+ 聊天接口

+ 注1：由于`LLM`接口返回时长可能比较长，建议网络请求时`connectTimeout`设置尽量长一点，比如300s等；

+ 注2：聊天接口，不支持绘图模型，请确保对应的应用所选模型不是绘图模型。

**请求方式**

+ `POST /biz/v1/app/chat/sync`

**请求参数**

+ **Header**

| 名称 | 是否必填 | 说明 |
| --- | --- | --- |
| `Content-Type` | 是 | `application/json;` |

+ **Body**

| 名称 | 类型 | 是否必填 | 说明 | 签名参数 |
| --- | --- | --- | --- | --- |
| `app_id` | `String` | 是 | 应用唯一标示，在`ZelinAI`创建应用完成之后，可复制生成的`app_id`。 | 是 |
| `request_id` | `String` | 是 | 本次请求的唯一标示，业务方生成，最多32位；相同`request_id`会查询上次请求的结果，所以不同的请求需要确保`request_id`不一样； | 是 |
| `uid` | `String` | 是 | 业务方用户唯一标示，最多32位，用于区分用户；为了防刷，我们在用户维度增加了一些限制，每个用户同一时间只能有一个提问，只有等提问回复了，才能继续提下一个问题； | 是 |
| `content` | `String` | 是 | 本次提问的问题 | 是 |
| `indexes` | `String[]` | 否 | 训练集唯一标识列表 | 否 |
| `var_content` | `Object` | 否 | 变量列表 | 否 |
| `img_urls` | `String[]` | 否 | 本次上传的图片列表，最多支持3张。ps: 仅在智能体中模型型号选择4v 、4o模型支持，其他模型上传图片不会生效 | 否 |

**响应参数**

| 名称 | 类型 | 说明 |
| --- | --- | --- |
| `status` | `int` | 200-正常；非200-异常 |
| `message` | `string` | `status`状态说明 |
| `data` | `object` | 返回的内容 |
| + `aid` | `string(32)` | 本次消息的唯一标示 |
| + `ai_content` | `string` | 回复的内容 |

**请求示例**

```
{
    "app_id": "xxx",
    "request_id": "xxx",
    "uid": "xxx",
    "content": "你好",
    "var_content": {
        "身份": "张三"
    },
}
```

**响应示例**

```
{
    "status": 200,
    "message": "",
    "data": {
        "aid": "0h947saXr2oGqS1NbY8o50",
        "ai_content": "你好，有什么我可以帮助你的吗？"
    }
}
```

**错误码**

见公共错误码

### 4.2 聊天接口（SSE流式）

**功能简述**

+ 聊天接口，`SSE`流式返回

+ 注：聊天接口，不支持绘图模型，请确保对应的应用所选模型不是绘图模型。

**请求方式**

+ `POST /biz/v1/app/chat/sync/sse`

**请求参数**

+ **Header**

| 名称 | 是否必填 | 说明 |
| --- | --- | --- |
| `Content-Type` | 是 | `application/json;` |

+ **Body**

| 名称 | 类型 | 是否必填 | 说明 | 签名参数 |
| --- | --- | --- | --- | --- |
| `app_id` | `String` | 是 | 应用唯一标示，在`ZelinAI`创建应用完成之后，可复制生成的`app_id`。 | 是 |
| `request_id` | `String` | 是 | 本次请求的唯一标示，业务方生成，最多32位；相同`request_id`会查询上次请求的结果，所以不同的请求需要确保`request_id`不一样； | 是 |
| `uid` | `String` | 是 | 业务方用户唯一标示，最多32位，用于区分用户；为了防刷，我们在用户维度增加了一些限制，每个用户同一时间只能有一个提问，只有等提问回复了，才能继续提下一个问题； | 是 |
| `content` | `String` | 是 | 本次提问的问题，最多256个字符 | 是 |
| `indexes` | `String[]` | 否 | 训练集唯一标识列表 | 否 |
| `var_content` | `Object` | 否 | 变量列表 | 否 |
| `img_urls` | `String[]` | 否 | 本次上传的图片列表，最多支持3张。ps: 仅在智能体中模型型号选择4v 、4o模型支持，其他模型上传图片不会生效 | 否 |

**响应示例**

![](img/e0c208b4a57c1a6b8cc2260ec19080bd.png)

| type | data |
| --- | --- |
| `message` | `{ "id": "chatcmpl-92hAlEziJA2HMGfwzPM2A9jX1t9T8", "object": "chat.completion", "created": 1710429639, "model": "gpt-35-turbo", "system_fingerprint": "fp_68a7d165bf", "choices": [ { "index": 0, "message": { "role": "assistant", "content": "{content}" }, "finish_reason": "stop" } ] }` |
| `function` | `// type为request { "name": "bingSearch", "type": "request", "request": { "name": "bingSearch", "arguments": "{\"query\": \"OpenAI latest news\"}" } } // type为response { "name": "bingSearch", "type": "response", "response": "{response}" }` |
| `error` | `{ "content": "{content}" }` |
| `close` | `{ "retrieve": [ { "uid": "mgKiHcKyQCArHWNBkEzZeR", "idx": "test_cnq4nVVCrX2vFFs3lMBld0", "key": "qa_gN1dwkQ_rN2sZOW_7sVJ33", "fileName": "", "embeddingStr": "ZelinAI的联系电话是多少？", "textStr": "400-001-5866", "type": 1, "score": 0.9291201 }, { "uid": "mgKiHcKyQCArHWNBkEzZeR", "idx": "test_heEUTGvnt1A62ynZJyX_71", "key": "cjwla_Cqt6EtNT7Mcq3ad9", "fileName": "robot_GDP_USA1031.txt", "embeddingStr": "xxxxxxx", "textStr": "xxxxxx", "type": 0, "score": 0.8207875 } ] }` |
| `healthy` | 探活消息，`data`是`{}` |

**错误码**

见公共错误码

## 公共错误码

| status | msg | 说明 |
| --- | --- | --- |
| 200 | 操作成功 | 接口调用成功 |
| 120001 | 模型不存在 | 应用关联的模型不存在 |
| 140001 | 应用不存在 | |
| 496101 | `appKey`不存在 | |
| 496102 | 缺少参数值 | 校验请求参数 |
| 496103 | `signature`校验失败 | |
| 496104 | `timestamp`校验失败 | |
| 496105 | `nonce`校验失败 | |
| 50001 | 调用`QPS`超限 | |
| 50002 | 调用次数超限 | |
| 50003 | 无空闲账号 | |
| 54000 | 调用失败 | 非如下明细报错时的统一报错 |
| 54010 | 账单未激活 | `[Type]billing_not_active` |
| 54020 | 配额不足 | `[Type]insufficient_quota` |
| 54030 | 无效请求错误 | `[Type]invalid_request_error` |
| 54031 | 帐户已停用 | `[Type]invalid_request_error` |
| 54032 | 上下文长度超出 | `[Type]invalid_request_error` |
| 54033 | 无效的`api_key` | `[Type]invalid_request_error` |
| 54040 | `RPM`达到速率限制 | `[Type]requests` |
| 54050 | 服务器错误 | `[Type]server_error` |
| 54060 | `TPM`达到速率限制 | `[Type]tokens` |
| 54071 | `prompt`内容受限 | `[Code]content_filter` |
| 54072 | 超时 | `[Code]Timeout` |
| 54080 | 身份验证子请求错误 | `[Type]auth_subrequest_error` |
| 54500 | 结果内容受限 | `[finish_reason]content_filter` |
| 55000 | 调用失败 | 文心一言调用失败 |

## 常见问题



| 问题 | 可能原因 |
| --- | --- |
| `appKey`不存在 | 请确保`appkey`在请求的`Header`中存在； |
| `signature`校验失败 | 注意，验签的逻辑，是服务器会重新计算一遍签名；所以客户端和服务器端，要使用相同的加签算法和参数来计算，才能匹配，所以对于所有参与加签的参数，都需要确保一致，包括`nonce`、`timestamp`、`request_id`等字段； |

```
md.update(str.getBytes("utf-8"));
```

# 7.参考代码

| 语言 | 示例代码 | 备注 |
| --- | --- | --- |
| Python |  |  |
| Java |  |  |

![](`img/6ae5cb896949aee4572b1c3661ae1725.png`)

[`zelinai_api_demo.py`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ESpVbFc2ZoulMIxC8qBcMlDBnbh)

![](`img/6c4c01ca2d9f4e87f3b37ff56b18a555.png`)

[`Test.java`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HvUtbQ8ycowqMpx1YOZcWdBMnLK)
