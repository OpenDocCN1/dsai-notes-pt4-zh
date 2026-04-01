# 【对外接口文档】聊天接口API

> 来源：[https://xuqiwei1986.feishu.cn/docx/Mq8Hdh1YqoZorEx2cgqcnGOPnof](https://xuqiwei1986.feishu.cn/docx/Mq8Hdh1YqoZorEx2cgqcnGOPnof)

# 简介

ZelinAI应用聊天请求接口，用于应用聊天交互。

---

# 更新历史

## V1.2.1 版本

**发布时间**

该版本发布于 2023 年 5 月 15 日。

**发布内容**

| **类型** | **说明** |
| --- | --- |
| 新增 | ZelinAI接口文档 |

---

# 调用方法

## **3.1 前提条件**

暂无

## 3.2 请求结构

**服务地址：****h**ttps://zelinai.com/

**通信协议：HTTPS**

## 3.3 公共参数

**公共请求参数**

暂无

**公共返回参数**

| **名称** | **类型** | **说明** |
| --- | --- | --- |
| status | int | 200-正常；非200-异常 |
| message | string | status状态说明 |
| data | object | 返回的内容 |

## 3.4 鉴权机制

+   为了确保接口调用的安全，每个接口在请求的时候，都要附带签名signature字段；鉴权逻辑：

+   signature签名一致；

+   timestamp相差在30s以内（正负30s）；

+   在请求的Header中添加如下参数

| **名称** | **类型** | **是否必填** | **说明** |
| --- | --- | --- | --- |
| appkey | string(32) | 是 | 业务方appkey |
| nonce | string(16) | 是 | 长度为16的随机字符串 |
| timestamp | long | 是 | 当前时间戳，单位秒 |
| signature | string | 是 | 签名字符串，加签方法见下文 |

+   注：在构造签名的字符串中，除了上述appkey、nonce、timestamp之外，可能还包括其他参数，具体看各个接口请求参数的“签名参数”列，值为“是”的都需要纳入进来一起加签；

+   计算签名的方法如下：

+   签名字段包含appkey、nonce和timestamp，还有接口包含的其他业务参数；

+   所有待签名参数按照字段名的ASCII 码从小到大排序（字典序），然后使用 URL 键值对的格式（即key1=value1&key2=value2…）拼接成字符串s1。（注：值为空的不计入签名；）

+   在 s1 字符串后面直接拼接 appsecret 作为签名字符串，即s1{appsecret}；

+   对签名字符串调用MD5签名，生成signature字符串，MD5(s1{appsecret})

+   注意：md5结果值需要转换成小写

+   注：appkey、appsecret是业务方唯一标示，由平台生成，请在控制台后台-系统设置里面获取；

---

# 相关接口

## 4.1 聊天接口（同步返回）

**功能简述**

+   聊天接口

+   注1：由于LLM接口返回时长可能比较长，建议网络请求时connectTimeout设置尽量长一点，比如300s等；

+   注2：聊天接口，不支持绘图模型，请确保对应的应用所选模型不是绘图模型。

**请求方式**

+   POST /biz/v1/app/chat/sync

**请求参数**

+   **Header**

| **名称** | **是否必填** | **说明** |
| --- | --- | --- |
| Content-Type | 是 | application/json; |

+   **Body**

| **名称** | **类型** | **是否必填** | **说明** | 签名参数 |
| --- | --- | --- | --- | --- |
| app_id | String | 是 | 应用唯一标示，在zelinAI创建应用完成之后，可复制生成的app_id。 | 是 |
| request_id | String | 是 | 本次请求的唯一标示，业务方生成，最多32位；相同request_id会查询上次请求的结果，所以不同的请求需要确保request_id不一样； | 是 |
| uid | String | 是 | 业务方用户唯一标示，最多32位，用于区分用户；为了防刷，我们在用户维度增加了一些限制，每个用户同一时间只能有一个提问，只有等提问回复了，才能继续提下一个问题； | 是 |
| content | String | 是 | 本次提问的问题 | 是 |
| indexes | String[] | 否 | 训练集唯一标识列表 | 否 |
| var_content | Object | 否 | 变量列表 | 否 |
| img_urls | String[] | 否 | 本次上传的图片列表，最多支持3张。ps: 仅在智能体中模型型号选择4v 、4o模型支持，其他模型上传图片不会生效 | 否 |

**响应参数**

| **名称** | **类型** | **说明** |
| --- | --- | --- |
| status | int | 200-正常；非200-异常 |
| message | string | status状态说明 |
| data | object | 返回的内容 |
| +   aid | string(32) | 本次消息的唯一标示 |
| +   ai_content | string | 回复的内容 |

**请求示例**

```
{
    &quot;app_id&quot;: &quot;xxx&quot;,
    &quot;request_id&quot;: &quot;xxx&quot;,
    &quot;uid&quot;: &quot;xxx&quot;,
    &quot;content&quot;: &quot;你好&quot;,
    &quot;var_content&quot;: {
        &quot;身份&quot;: &quot;张三&quot;
    },
}
```

**响应示例**

```
{
    &quot;status&quot;: 200,
    &quot;message&quot;: &quot;&quot;,
    &quot;data&quot;: {
        &quot;aid&quot;: &quot;0h947saXr2oGqS1NbY8o50&quot;,
        &quot;ai_content&quot;: &quot;你好，有什么我可以帮助你的吗？&quot;
    }
}
```

**错误码**

见公共错误码

## 4.2 聊天接口（SSE流式）

**功能简述**

+   聊天接口，SSE流式返回

+   注：聊天接口，不支持绘图模型，请确保对应的应用所选模型不是绘图模型。

**请求方式**

+   POST /biz/v1/app/chat/sync/sse

**请求参数**

+   **Header**

| **名称** | **是否必填** | **说明** |
| --- | --- | --- |
| Content-Type | 是 | application/json; |

+   **Body**

| **名称** | **类型** | **是否必填** | **说明** | 签名参数 |
| --- | --- | --- | --- | --- |
| app_id | String | 是 | 应用唯一标示，在zelinAI创建应用完成之后，可复制生成的app_id。 | 是 |
| request_id | String | 是 | 本次请求的唯一标示，业务方生成，最多32位；相同request_id会查询上次请求的结果，所以不同的请求需要确保request_id不一样； | 是 |
| uid | String | 是 | 业务方用户唯一标示，最多32位，用于区分用户；为了防刷，我们在用户维度增加了一些限制，每个用户同一时间只能有一个提问，只有等提问回复了，才能继续提下一个问题； | 是 |
| content | String | 是 | 本次提问的问题，最多256个字符 | 是 |
| indexes | String[] | 否 | 训练集唯一标识列表 | 否 |
| var_content | Object | 否 | 变量列表 | 否 |
| img_urls | String[] | 否 | 本次上传的图片列表，最多支持3张。ps: 仅在智能体中模型型号选择4v 、4o模型支持，其他模型上传图片不会生效 | 否 |

**响应示例**

![](img/e0c208b4a57c1a6b8cc2260ec19080bd.png)

| type | data |
| --- | --- |
| message | ``` {     &amp;quot;id&amp;quot;: &amp;quot;chatcmpl-92hAlEziJA2HMGfwzPM2A9jX1t9T8&amp;quot;,     &amp;quot;object&amp;quot;: &amp;quot;chat.completion&amp;quot;,     &amp;quot;created&amp;quot;: 1710429639,     &amp;quot;model&amp;quot;: &amp;quot;gpt-35-turbo&amp;quot;,     &amp;quot;system_fingerprint&amp;quot;: &amp;quot;fp_68a7d165bf&amp;quot;,     &amp;quot;choices&amp;quot;: [         {             &amp;quot;index&amp;quot;: 0,             &amp;quot;message&amp;quot;: {                 &amp;quot;role&amp;quot;: &amp;quot;assistant&amp;quot;,                 &amp;quot;content&amp;quot;: &amp;quot;{content}&amp;quot;             },             &amp;quot;finish_reason&amp;quot;: &amp;quot;stop&amp;quot;         }     ] } ``` |
| function | ``` // type为request {     &amp;quot;name&amp;quot;: &amp;quot;bingSearch&amp;quot;,  ## 工具名     &amp;quot;type&amp;quot;: &amp;quot;request&amp;quot;,     ## 调用类型，request/response     &amp;quot;request&amp;quot;: {           ## 调用类型为request时，是结构化数据         &amp;quot;name&amp;quot;: &amp;quot;bingSearch&amp;quot;,         &amp;quot;arguments&amp;quot;: &amp;quot;{\&amp;quot;query\&amp;quot;: \&amp;quot;OpenAI latest news\&amp;quot;}&amp;quot;     } }  // type为response {     &amp;quot;name&amp;quot;: &amp;quot;bingSearch&amp;quot;,     &amp;quot;type&amp;quot;: &amp;quot;response&amp;quot;,     &amp;quot;response&amp;quot;: &amp;quot;{response}&amp;quot;   ## 调用类型为response时，是string字符串 } ``` |
| error | ``` {     &amp;quot;content&amp;quot;: &amp;quot;{content}&amp;quot; } ``` |
| close | ``` {     &amp;quot;retrieve&amp;quot;: [         {           &amp;quot;uid&amp;quot;: &amp;quot;mgKiHcKyQCArHWNBkEzZeR&amp;quot;,           &amp;quot;idx&amp;quot;: &amp;quot;test_cnq4nVVCrX2vFFs3lMBld0&amp;quot;,           &amp;quot;key&amp;quot;: &amp;quot;qa_gN1dwkQ_rN2sZOW_7sVJ33&amp;quot;,           &amp;quot;fileName&amp;quot;: &amp;quot;&amp;quot;,           &amp;quot;embeddingStr&amp;quot;: &amp;quot;ZelinAI的联系电话是多少？&amp;quot;,           &amp;quot;textStr&amp;quot;: &amp;quot;400-001-5866&amp;quot;,    //文本内容           &amp;quot;type&amp;quot;: 1,                    //类型，0-默认；1-标准QA；           &amp;quot;score&amp;quot;: 0.9291201            //相似度分值，retrieve已按照score从高到底排序         },         {             &amp;quot;uid&amp;quot;: &amp;quot;mgKiHcKyQCArHWNBkEzZeR&amp;quot;,             &amp;quot;idx&amp;quot;: &amp;quot;test_heEUTGvnt1A62ynZJyX_71&amp;quot;,             &amp;quot;key&amp;quot;: &amp;quot;cjwla_Cqt6EtNT7Mcq3ad9&amp;quot;,             &amp;quot;fileName&amp;quot;: &amp;quot;robot_GDP_USA1031.txt&amp;quot;,             &amp;quot;embeddingStr&amp;quot;: &amp;quot;xxxxxxx&amp;quot;,             &amp;quot;textStr&amp;quot;: &amp;quot;xxxxxx&amp;quot;,             &amp;quot;type&amp;quot;: 0,             &amp;quot;score&amp;quot;: 0.8207875         }     ] } ``` |
| healthy | 探活消息，data是{} |

**错误码**

见公共错误码

---

# 公共错误码

| **status** | **msg** | **说明** |
| --- | --- | --- |
| 200 | 操作成功 | 接口调用成功 |
| 120001 | 模型不存在 | 应用关联的模型不存在 |
| 140001 | 应用不存在 |  |
| 496101 | appKey不存在 |  |
| 496102 | 缺少参数值 | 校验请求参数 |
| 496103 | signature校验失败 |  |
| 496104 | timestamp校验失败 |  |
| 496105 | nonce校验失败 |  |
| 50001 | 调用QPS超限 |  |
| 50002 | 调用次数超限 |  |
| 50003 | 无空闲账号 |  |
| 54000 | 调用失败 | 非如下明细报错时的统一报错 |
| 54010 | 账单未激活 | [Type]billing_not_active |
| 54020 | 配额不足 | [Type]insufficient_quota |
| 54030 | 无效请求错误 | [Type]invalid_request_error |
| 54031 | 帐户已停用 | [Type]invalid_request_error |
| 54032 | 上下文长度超出 | [Type]invalid_request_error |
| 54033 | 无效的api_key | [Type]invalid_request_error |
| 54040 | RPM达到速率限制 | [Type]requests |
| 54050 | 服务器错误 | [Type]server_error |
| 54060 | TPM达到速率限制 | [Type]tokens |
| 54071 | prompt内容受限 | [Code]content_filter |
| 54072 | 超时 | [Code]Timeout |
| 54080 | 身份验证子请求错误 | [Type]auth_subrequest_error |
| 54500 | 结果内容受限 | [finish_reason]content_filter |
| 55000 | 调用失败 | 文心一言调用失败 |

---

# 常见问题

| 问题 | 可能原因 |
| --- | --- |
| appKey不存在 | +   请确保appkey在请求的Header中存在； |
| signature校验失败 | +   注意，验签的逻辑，是服务器会重新计算一遍签名；所以客户端和服务器端，要使用相同的加签算法和参数来计算，才能匹配，所以对于所有参与加签的参数，都需要确保一致，包括nonce、timestamp、request_id等字段； |
|  |  |

+   md.update(str.getBytes("utf-8"));

# 7.参考代码

| 语言 | 示例代码 | 备注 |
| --- | --- | --- |
| Python |  |  |
| Java |  |  |

![](img/6ae5cb896949aee4572b1c3661ae1725.png)

[zelinai_api_demo.py](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ESpVbFc2ZoulMIxC8qBcMlDBnbh)

![](img/6c4c01ca2d9f4e87f3b37ff56b18a555.png)

[Test.java](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HvUtbQ8ycowqMpx1YOZcWdBMnLK)