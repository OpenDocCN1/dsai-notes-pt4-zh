
# 2023-08-03 BLOOMZ-7B - 千帆大模型平台 | 百度智能云文档

> 来源：[https://xuqiwei1986.feishu.cn/docx/IaNOdjwozoUvavxSEC8c7LaWnFf](https://xuqiwei1986.feishu.cn/docx/IaNOdjwozoUvavxSEC8c7LaWnFf)

> 🔗 原文链接：https://cloud.baidu.com/doc/WENXINWORKSHOP/s/Jljcadglj

BLOOMZ-7B是业内知名的大语言模型，由BigScience研发并开源，能够以46种语言和13种编程语言输出文本。本文介绍了千帆相关API。

## 创建chat

调用本接口，发起一次对话请求。

### 注意事项

本文接口返回不支持图片内容，图片内容的请求返回会在后续支持，敬请期待！

### 请求说明

#### 基本信息

请求地址：`https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/bloomz_7b1`

请求方式：`POST`

| 名称 | 值 |
| --- | --- |
| `Content-Type` | application/json |

#### Query参数

| 名称 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| `access_token` | string | 是 | 通过API Key和Secret Key获取的`access_token`，参考 [Access Token获取](https://ai.baidu.com/ai-doc/REFERENCE/Ck3dwjhhu) |

#### Body参数

| 名称 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| `messages` | `List(message)` | 是 | 聊天上下文信息。说明：（1）`messages`成员不能为空，1个成员表示单轮对话，多个成员表示多轮对话（2）最后一个`message`为当前请求的信息，前面的`message`为历史对话信息（3）必须为奇数个成员，成员中`message`的`role`必须依次为`user`、`assistant`（4）最后一个`message`的`content`长度（即此轮对话的问题）不能超过2000个字符；如果`messages`中`content`总长度大于2000字符，系统会依次遗忘最早的历史会话，直到`content`的总长度不超过2000个字符 |
| `stream` | bool | 否 | 是否以流式接口的形式返回数据，默认false |
| `user_id` | string | 否 | 表示最终用户的唯一标识符，可以监视和检测滥用行为，防止接口恶意调用 |

#### message说明

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| `role` | string | 当前支持以下：`user`：表示用户`assistant`：表示对话助手 |
| `content` | string | 对话内容，不能为空 |

### 响应说明

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| `id` | string | 本轮对话的id |
| `object` | string | 回包类型。`chat.completion`：多轮对话返回 |
| `created` | int | 时间戳 |
| `sentence_id` | int | 表示当前子句的序号。只有在流式接口模式下会返回该字段 |
| `is_end` | bool | 表示当前子句是否是最后一句。只有在流式接口模式下会返回该字段 |
| `is_truncated` | bool | 当前生成的结果是否被截断 |
| `result` | string | 对话返回结果 |
| `need_clear_history` | bool | 表示用户输入是否存在安全，是否关闭当前会话，清理历史回话信息 `true`：是，表示用户输入存在安全风险，建议关闭当前会话，清理历史会话信息 `false`：否，表示用户输入无安全风险 |
| `ban_round` | int | 当`need_clear_history`为`true`时，此字段会告知第几轮对话有敏感信息，如果是当前问题，`ban_round`=-1 |
| `usage` | `usage` | token统计信息，token数 = 汉字数+单词数*1.3 （仅为估算逻辑） |

#### usage说明

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| `prompt_tokens` | int | 问题tokens数 |
| `completion_tokens` | int | 回答tokens数 |
| `total_tokens` | int | tokens总数 |

#### 注意

同步模式和流式模式，响应参数返回不同，详细内容参考示例描述。

+ 同步模式下，响应参数为以上字段的完整json包。
+ 流式模式下，各字段的响应参数为 `data: {响应参数}`。

### 示例

#### 请求示例（单轮）

+ bash

```bash
# 步骤一，获取access_token，替换下列示例中的API Key与Secret Key
curl 'https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=[API Key]&client_secret=[Secret Key]'

# 步骤二，调用本文API，使用步骤一获取的access_token，替换下列示例中的”调用接口获取的access_token“
curl -XPOST 'https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/bloomz_7b1?access_token=[步骤一调用接口获取的access_token]' -d '{
   "messages": [
    {"role":"user","content":"介绍一下你自己"}
   ]
}' | iconv -f utf-8 -t utf-8
```

+ Python

```python
import requests
import json

def get_access_token():
    """
    使用 API Key，Secret Key 获取access_token，替换下列示例中的应用API Key、应用Secret Key
    """
        
    url = "https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=[应用API Key]&client_secret=[应用Secret Key]"
    
    payload = json.dumps("")
    headers = {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
    }
    
    response = requests.request("POST", url, headers=headers, data=payload)
    return response.json().get("access_token")


def main():
        
    url = "https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/bloomz_7b1?access_token=" + get_access_token()
    
    payload = json.dumps({
        "messages": [
            {
                "role": "user",
                "content": "介绍一下你自己"
            }
        ]
    })
    headers = {
        'Content-Type': 'application/json'
    }
    
    response = requests.request("POST", url, headers=headers, data=payload)
    
    print(response.text)


if __name__ == '__main__':
    main()
```

#### 响应示例（单轮）

```http
HTTP/1.1 200 OK
Date: Thu, 23 Mar 2023 03:12:03 GMT
Content-Type: application/json;charset=utf-8
Statement: AI-generated

{
  "id": "as-bcmt5ct4iy",
  "object": "chat.completion",
  "created": 1680167072,
  "result": "您好，我是业内知名的大语言模型，由BigScience研发并开源，能够以46种语言和13种编程语言输出文本。",
  "is_truncated":false,
  "need_clear_history": false,
  "usage": {
    "prompt_tokens": 7,
    "completion_tokens": 67,
    "total_tokens": 74
  }
}
```

#### 请求示例（多轮）

+ bash

```bash
# 步骤一，获取access_token，替换下列示例中的API Key与Secret Key
curl 'https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=[API Key]&client_secret=[Secret Key]'

# 步骤二，调用本文API，使用步骤一获取的access_token，替换下列示例中的”调用接口获取的access_token“
curl -XPOST 'https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/bloomz_7b1?access_token=[步骤一调用接口获取的access_token]' -d '{
  "messages": [
    {"role":"user","content":"请介绍一下你自己"},
    {"role":"assistant","content":"业内知名的大语言模型，由BigScience研发并开源，能够以46种语言和13种编程语言输出文本。"},
    {"role":"user","content": "我在上海，周末可以去哪里玩？"},
    {"role":"assistant","content": "上海是一个充满活力和文化氛围的城市，有很多适合周末游玩的地方。以下是几个值得推荐的地方：\n\n1. 上海科技馆：上海科技馆是中国大陆最大的科技馆之一，有许多互动展览和科学实验室，可以让您了解科技的发展历史和最新成果。\n2. 上海博物馆：上海博物馆是一座集艺术、历史和文化于一体的博物馆，有许多精美的艺术品和文物，可以让您深入了解中国和世界的文化历史。\n3. 外滩：外滩是上海的标志性景点之一，有许多高楼大厦和繁华的商业街区，是欣赏黄浦江美景和城市风貌的好去处。\n4. 上海迪士尼乐园：上海迪士尼乐园是世界上最大的迪士尼主题公园之一，有许多精彩的游乐项目和演出，可以让您尽情享受娱乐和欢乐。\n5. 上海野生动物园：上海野生动物园是一个以自然保护为主题的野生动物园，有许多珍稀动物和植物，可以让您近距离接触大自然。\n\n这些地方都是上海周末游玩的好去处，可以根据自己的兴趣和需求选择合适的行程。"},
    {"role":"user","content": "周末这里的天气怎么样？"}
  ]
}'  | iconv -f utf-8 -t utf-8
```

+ Python

```python
import requests
import json


def main():
        
    url = "https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/bloomz_7b1?access_token=[调用鉴权接口获取的token]"
    
    payload = json.dumps({
        "messages": [
            {
                "role": "user",
                "content": "请介绍一下你自己"
            },
            {
                "role": "assistant",
                "content": "业内知名的大语言模型，由BigScience研发并开源，能够以46种语言和13种编程语言输出文本。"
            },
            {
                "role": "user",
                "content": "我在上海，周末可以去哪里玩？"
            },
            {
                "role": "assistant",
                "content": "上海是一个充满活力和文化氛围的城市，有很多适合周末游玩的地方。以下是几个值得推荐的地方：\n\n1. 上海科技馆：上海科技馆是中国大陆最大的科技馆之一，有许多互动展览和科学实验室，可以让您了解科技的发展历史和最新成果。\n2. 上海博物馆：上海博物馆是一座集艺术、历史和文化于一体的博物馆，有许多精美的艺术品和文物，可以让您深入了解中国和世界的文化历史。\n3. 外滩：外滩是上海的标志性景点之一，有许多高楼大厦和繁华的商业街区，是欣赏黄浦江美景和城市风貌的好去处。\n4. 上海迪士尼乐园：上海迪士尼乐园是世界上最大的迪士尼主题公园之一，有许多精彩的游乐项目和演出，可以让您尽情享受娱乐和欢乐。\n5. 上海野生动物园：上海野生动物园是一个以自然保护为主题的野生动物园，有许多珍稀动物和植物，可以让您近距离接触大自然。\n\n这些地方都是上海周末游玩的好去处，可以根据自己的兴趣和需求选择合适的行程。"
            },
            {
                "role": "user",
                "content": "周末这里的天气怎么样？"
            }
        ]
    })
    headers = {
        'Content-Type': 'application/json'
    }
    
    response = requests.request("POST", url, headers=headers, data=payload)
    
    print(response.text)


if __name__ == '__main__':
    main()
```

#### 响应示例（多轮）



## 响应示例（非流式）

```http
HTTP/1.1 200 OK
Date: Mon, 12 Apr 2021 06:27:55 GMT
Content-Type: application/json;charset=utf-8
Statement: AI-generated

{
  &quot;id&quot;: &quot;as-1tbbc0x5af&quot;,
  &quot;object&quot;: &quot;chat.completion&quot;,
  &quot;created&quot;: 1680166551,
  &quot;result&quot;: &quot;2023年4月2日上海气温13~21℃，多云转阴，东风3-4级，空气质量良，空气质量指数55。\n\n\n\n近7日天气信息：\n\n2023-03-29：阴转小雨，11~17℃，东北风&lt;3级，空气质量良。\n\n2023-03-30：小雨转阴，10~14℃，东风3-4级，空气质量良。\n\n2023-03-31：多云，12~18℃，东风3-4级，空气质量优。\n\n2023-04-01：多云转晴，11~20℃，东南风3-4级，空气质量良。\n\n2023-04-02：多云转阴，13~21℃，东风3-4级，空气质量良。\n\n2023-04-03：阴转中雨，15~18℃，东南风4-5级，空气质量良。\n\n2023-04-04：中雨转小雨，10~17℃，南风5-6级，空气质量优。\n\n2023-04-05：阴，9~14℃，西北风3-4级，空气质量优。&quot;,
  &quot;is_truncated&quot;:false,
  &quot;need_clear_history&quot;: false,
  &quot;usage&quot;: {
    &quot;prompt_tokens&quot;: 470,
    &quot;completion_tokens&quot;: 198,
    &quot;total_tokens&quot;: 668
  }
}
```

## 请求示例（流式）

```bash
# 步骤一，获取access_token，替换下列示例中的API Key与Secret Key
curl &#x27;https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&amp;client_id=[API Key]&amp;client_secret=[Secret Key]&#x27;

# 步骤二，调用本文API，使用步骤一获取的access_token，替换下列示例中的”调用接口获取的access_token“
curl -XPOST &#x27;https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/bloomz_7b1?access_token=[步骤一调用接口获取的access_token]&#x27; -d &#x27;{
  &quot;messages&quot;: [
    {&quot;role&quot;:&quot;user&quot;, &quot;content&quot;: &quot;给我推荐一些自驾游路线&quot;}
  ],
  &quot;stream&quot;: true
}&#x27;
```

```python
import requests
import json

def get_access_token():
    &quot;&quot;&quot;
    使用 API Key，Secret Key 获取access_token，替换下列示例中的应用API Key、应用Secret Key
    &quot;&quot;&quot;
        
    url = &quot;https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&amp;client_id=[应用API Key]&amp;client_secret=[应用Secret Key]&quot;
    
    payload = json.dumps(&quot;&quot;)
    headers = {
        &#x27;Content-Type&#x27;: &#x27;application/json&#x27;,
        &#x27;Accept&#x27;: &#x27;application/json&#x27;
    }
    
    response = requests.request(&quot;POST&quot;, url, headers=headers, data=payload)
    return response.json().get(&quot;access_token&quot;)

def main():
        
    url = &quot;https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/bloomz_7b1?access_token=&quot; + get_access_token()
    
    payload = json.dumps({
        &quot;messages&quot;: [
            {
                &quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: &quot;给我推荐一些自驾游路线&quot;
            }
        ],
         &quot;stream&quot;: True
    })
    headers = {
        &#x27;Content-Type&#x27;: &#x27;application/json&#x27;
    }
    
    response = requests.request(&quot;POST&quot;, url, headers=headers, data=payload)
    
    print(response.text)
    

if __name__ == &#x27;__main__&#x27;:
    main()
```

## 响应示例（流式）

```http
HTTP/1.1 200 OK
Date: Mon, 12 Apr 2021 06:27:55 GMT
Content-Type: text/event-stream;charset=utf-8
Cache-Control: no-cache
Statement: AI-generated

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166793,&quot;sentence_id&quot;:0,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;以下是一些适合自驾游的路线推荐：\n\n1.&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:16,&quot;total_tokens&quot;:27}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166794,&quot;sentence_id&quot;:1,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot; 中国大陆最美的景观大道：川藏线，从成都出发，沿着川藏公路一路向西，经过稻城亚丁、理塘、巴塘、芒康等美景，最终到达拉萨。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:50,&quot;total_tokens&quot;:77}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166795,&quot;sentence_id&quot;:2,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;\n2. 丝绸之路：这是一条贯穿中国东西部的公路，从上海出发，经过西安、兰州、乌鲁木齐等城市，最终到达喀什。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:43,&quot;total_tokens&quot;:120}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166796,&quot;sentence_id&quot;:3,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;沿途可以欣赏到中国北方和南方的不同景色。\n3. 西北大环线：从成都出发，经过都江堰、青城山、丹巴、塔公草原、新都桥等景点，最终到达西宁。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:56,&quot;total_tokens&quot;:176}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166799,&quot;sentence_id&quot;:4,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;这条路线可以领略到中国西北地区的壮美风光。\n4. 东北环线：从沈阳出发，经过长春、吉林、延吉等城市，最终到达哈尔滨。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:48,&quot;total_tokens&quot;:224}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166800,&quot;sentence_id&quot;:5,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;沿途可以欣赏到中国东北地区的广阔草原和森林。\n5. 西南环线：从成都出发，经过雅安、康定、理塘、稻城亚丁等景点，最终到达香格里拉。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:54,&quot;total_tokens&quot;:278}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166802,&quot;sentence_id&quot;:6,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;这条路线可以领略到中国西南地区的高山峡谷和民族文化。\n6. 海南环线：从三亚出发，经过海口、文昌、万宁等城市，最终到达三亚。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:52,&quot;total_tokens&quot;:330}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166803,&quot;sentence_id&quot;:7,&quot;is_end&quot;:false,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;这条路线可以欣赏到中国南方的海岸线和热带雨林。\n7. 滇藏线：从昆明出发，经过大理、丽江、香格里拉等景点，最终到达西藏拉萨。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:52,&quot;total_tokens&quot;:382}}

data: {&quot;id&quot;:&quot;as-ywwpgx4dt7&quot;,&quot;object&quot;:&quot;chat.completion&quot;,&quot;created&quot;:1680166804,&quot;sentence_id&quot;:8,&quot;is_end&quot;:true,&quot;is_truncated&quot;:false,&quot;result&quot;:&quot;这条路线可以领略到中国西南地区和西藏地区的自然风光和人文景观。&quot;,&quot;need_clear_history&quot;:false,&quot;usage&quot;:{&quot;prompt_tokens&quot;:11,&quot;completion_tokens&quot;:30,&quot;total_tokens&quot;:412}}
```

## 错误码

如果请求错误，服务器返回的JSON文本包含以下参数。

| 名称 | 描述 |
| --- | --- |
| `error_code` | 错误码 |
| `error_msg` | 错误描述信息，帮助理解和解决发生的错误 |

例如`Access Token`失效返回：

```json
{
  &quot;error_code&quot;: 110,
  &quot;error_msg&quot;: &quot;Access token invalid or no longer valid&quot;
}
```

需要重新获取新的`Access Token`再次请求。

| 错误码 | 错误信息 | 描述 |
| --- | --- | --- |
| 1 | Unknown error | 服务器内部错误，请再次请求， 如[果持续出现](https://ticket.bce.baidu.com/#/ticket/create?useticket=1&amp;productId=279)此类错误，请在百度云控制台内 提交工单 反馈 |
| 2 | Service temporarily unavailable | [服务暂不可](https://ticket.bce.baidu.com/#/ticket/create?useticket=1&amp;productId=279&amp;questionId=1549)用，请再次请求， 如果持续出现此类错误，请在百度云控制台内 提交工单 反馈 |
| 3 | Unsupported openapi method | 调用的API不存在，请检查后重新尝试 |
| 4 | Open api request limit reached | 集群超限额 |
| 6 | No permission to access data | 无权限访问该用户数据 |
| 13 | Get service token failed | 获取token失败 |
| 14 | IAM Certification failed | IAM鉴权失败 |
| 15 | app not exists or create failed | 应用不存在或者创建失败 |



| 17 | Open api daily request limit reached | 每[天请求量超](https://ticket.bce.baidu.com/?_=1683861660309#/ticket/create?productId=279&amp;questionId=1548)限额，已上线计费的接口，请直接在控制台开通计费，调用量不受限制，按调用量阶梯计费；未上线计费的接口，请在百度云控制台内 提交工单 反馈 |
| 18 | Open api qps request limit reached | Q[PS超限额](https://ticket.bce.baidu.com/#/ticket/create?useticket=1&amp;productId=279&amp;questionId=1548)，已上线计费的接口，请直接在控制台开通计费，调用量不受限制，按调用量阶梯计费；未上线计费的接口，请在百度云控制台内 提交工单 反馈 |
| 19 | Open api total request limit reached | 请[求总量超限](https://ticket.bce.baidu.com/#/ticket/create?useticket=1&amp;productId=279&amp;questionId=1548)额，已上线计费的接口，请直接在控制台开通计费，调用量不受限制，按调用量阶梯计费；未上线计费的接口，请在百度云控制台内 提交工单 反馈 |
| 100 | Invalid parameter | 无效的`access_token`参数，请检查后重新尝试 |
| 110 | Access token invalid or no longer valid | `access_token`无效 |
| 111 | Access token expired | `access token`过期 |
| 336000 | Internal error | 服务内部错误 |
| 336001 | Invalid Argument | 入参格式有误，比如缺少必要参数 |
| 336002 | Invalid JSON | 入参`body`不是标准的JSON格式 |
| 336003 | {参数校验不合法的具体原因}说明：参数不同，返回错误信息不同 | 参数校验不合法，`error_msg`中会给出具体原因 |
| 336004 | {权限控制出错的具体原因} | 权限控制出错 |
| 336005 | API name not exist | 定制化模型服务`apiname`不存在 |
| 336100 | 人工智能是一项革命性的技术。与我互动的人过多，请您稍后重新向我提问。 | 人工智能是一项革命性的技术。与我互动的人过多，请您稍后重新向我提问 |
| 336101 | Invalid HTTP Method | 非法的HTTP Method， 当前仅支持`POST`请求 |
