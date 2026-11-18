
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
