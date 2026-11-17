# GLM-4 语言模型

> 来源：[https://v11enp9ok1h.feishu.cn/docx/H1iRdgoFqobRqhxIWlAcZWXRndg](https://v11enp9ok1h.feishu.cn/docx/H1iRdgoFqobRqhxIWlAcZWXRndg)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

## GLM-4 Cookbook调用教程

Python SDK请求

工具调用

## 接口请求

| 传输方式 | https |
| --- | --- |
| 请求地址 | https://open.bigmodel.cn/api/paas/v4/chat/completions |
| 调用方式 | 同步调用，等待模型执行完成并返回最终结果或 SSE 调用 |
| 字符编码 | UTF-8 |
| 接口请求格式 | JSON |
| 响应格式 | JSON 或标准 Stream Event |
| 接口请求类型 | POST |
| 开发语言 | 任意可发起 http 请求的开发语言 |

### 接口请求参数

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| model | String | 是 | 所要调用的模型编码。 |
| messages | List&lt;Object&gt; | 是 | 调用语言模型时，将当前对话信息列表作为提示输入给模型; 按 总长度超过模型最长输入限制后会自动截断，需按时间由旧到新排序；模型可接受的消息类型包括System message、User message、Assistant message和Tool message。可接受的格式为json数组。见下方message消息字段说明 |
| request_id | String | 否 | 由用户端传参，需保证唯一性；用于区分每次请求的唯一标识，用户端不传时平台会默认生成。 |
| do_sample | Boolean | 否 | do_sample为true 时启用采样策略，do_sample为false 时采样策略temperature、top_p将不生效 |
| stream | Boolean | 否 | 使用同步调用时，此参数应当设置为Fasle或者省略。表示模型生成完所有内容后一次性返回所有内容。 |
| temperature | Float | 否 | 采样温度，控制输出的随机性，必须为正数  取值范围是：(0.0,1.0]，不能等于 0,默认值为 0.95  值越大，会使输出更随机，更具创造性；值越小，输出会更加稳定或确定  建议您根据应用场景调整 top_p 或 temperature 参数，但不要同时调整两个参数 |
| top_p | Float | 否 | 用温度取样的另一种方法，称为核取样  取值范围是：(0.0, 1.0) 开区间，不能等于 0 或 1，默认值为 0.7  模型考虑具有 top_p 概率质量tokens的结果  例如：0.1 意味着模型解码器只考虑从前 10% 的概率的候选集中取tokens  建议您根据应用场景调整 top_p 或 temperature 参数，但不要同时调整两个参数 |
| max_tokens | Interger | 否 | 模型最大输出 tokens |
| stop | List | 否 | 停止词，目前仅支持单个停止词，格式为[&quot;stop_word1&quot;] |
| tools | List |  | 模型可以调用的工具列表 |
| type | String | 是 | 工具类型，目前仅支持function |
| function | Object | 是 |  |
| name | String | 是 | 函数名称。 |
| description | String | 是 | 用于描述函数功能。模型会根据这段描述决定函数调用方式。 |
| parameters | Object | 否 | parameter字段需要传入一个 Json Schema 对象，以准确地定义函数所接受的参数。若调用函数时不需要传入参数，省略该参数即可。样例： |
| tool_choice | String或Object | 否 | 控制模型是否调用定义的函数。 |

模型可接受的消息类型包括System message、User message、Assistant message和Tool message，不同的消息类型格式不同，具体如下：

1.  System message

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| role | String | 是 | 消息的角色信息，此时应为system |
| content | String | 是 | 消息内容 |

1.  User message

+   纯文本格式：

调用glm-4、glm-3-turbo 模型时，使用此格式的 User message 。

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| role | String | 是 | 消息的角色信息，此时应为user |
| content | String | 是 | 消息内容。内容仅为文字消息时类型为string。 |

+   内容分块数组：

仅在调用glm4-v模型时，使用此格式的 User message 。

此时content是一个数组，其中的元素是一个内容块，一个内容块是文本类型或图片类型。

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| role | String | 是 | 消息的角色信息，此时应为user |
| content | String或List | 是 | 消息内容。List中包括消息的各个部分。各部分可以分别为文字或图片消息。以下为文字部分和图片部分的传参格式。 |
|  | Object |  | 文字类型 |
| type | String | 是 | 此时应为text |
| text | String | 是 | 消息文字内容 |
|  | Object |  | 图片类型 |
| type | String | 是 | 此时应为image_url |
| image_url | String | 是 | 图片链接或图片的base64编码 |

内容分块数组的示例：

                &quot;url&quot;: &quot;https://ts1.cn.mm.bing.net/th/id/R-C.3997eb10814025183c8fbbc6b5373713?rik=fSPsSA1rrKXAzw&amp;riu=http%3a%2f%2fdpic.tiankong.com%2fau%2fpw%2fQJ6612208802.jpg&amp;ehk=c%2f0WAvYPgW2uwrHdpUenuYb4L%2f0oQkiOmmJoSawaX4A%3d&amp;risl=&amp;pid=ImgRaw&amp;r=0&quot;

```
{
    &quot;role&quot;: &quot;user&quot;,
    &quot;content&quot;: [
        {
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;图里有几个人，他们可能要去哪里&quot;
        },
        {
            &quot;type&quot;: &quot;image_url&quot;,
            &quot;image_url&quot;: {
            }
        }
    ]
}
```

1.  Assistant message

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| role | String | 是 | 消息的角色信息，此时应为assistant |
| content | String | 否 | 消息内容。如果消息中包括了tool_calls字段，content字段为空。 |
| tool_calls | List | 否 | 模型产生的工具调用消息 |
| id | String | 需要填写tool_calls时必填 | 模型调用的工具 |
| type | String | 需要填写tool_calls时必填 | 模型调用的工具类型，目前只支持function |
| function | Object | 需要填写tool_calls时必填 | 模型调用的函数 |
| name | String | 需要填写tool_calls时必填 | 模型调用的函数名称 |
| arguments | String | 需要填写tool_calls时必填 | 模型调用函数时传递的参数，为json格式。 |

1.  Tool message

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| role | String | 是 | 消息的角色信息，此时应为tool |
| content | String | 是 | tool message消息内容，为工具被调用后返回的结果。 |
| tool_call_id | String | 是 | 产生此消息的函数的唯一标识。 |

### 模型同步调用响应内容

| **参数名称** | **类型** | **参数说明** |
| --- | --- | --- |
| id | String | 任务ID |
| created | Long | 请求创建时间，是以秒为单位的Unix时间戳。 |
| model | String | 模型名称 |
| choices | List | 当前对话的模型输出内容 |
| index | Integer | 结果下标 |
| finish_reason | String | 模型推理终止的原因。 |
| message | Object | 模型返回的文本信息 |
| role | String | 当前对话的角色，目前默认为 assistant（模型） |
| content | String | 当前对话的内容。命中函数时此字段为null，未命中函数时返回模型推理结果。 |
| tool_calls | List&lt;Object&gt; | 模型生成的应调用函数的名称和参数。 |
| function | Object |  |
| name | String | 模型生成的应调用函数的名称。 |
| arguments | Object | 模型生成的 JSON 格式的函数调用参数。请注意，模型生成的 JSON 并不总是有效的，可能会出现函数模式未定义的参数。在调用函数之前，请在代码中验证参数。 |
| id | String | 命中函数的唯一标识符。 |
| type | String | 模型调用工具的类型，目前仅支持function。 |
| usage | Object | 结束时返回本次模型调用的 tokens 数量统计 |
| prompt_tokens | Integer | 用户输入的 tokens 数量 |
| completion_tokens | Integer | 模型输出的 tokens 数量 |
| total_tokens | Integer | 总 tokens 数量 |

### 流式响应块内容

| **参数名称** | **类型** | **参数说明** |
| --- | --- | --- |
| id | String | 智谱AI开放平台生成的任务订单号，调用请求结果接口时请使用此订单号 |
| created | Long | 请求创建时间，是以秒为单位的Unix时间戳。 |
| choices | List | 当前对话的模型输出内容 |
| index | Integer | 结果下标 |
| finish_reason | String | 模型推理终止的原因。 |
| delta | Object | 模型增量返回的文本信息 |
| role | String | 当前对话的角色，目前默认为 assistant（模型） |
| content | String | 当前对话的内容 |
| tool_calls | List&lt;Object&gt; | 模型生成的应调用函数的名称和参数。 |
| function | Object |  |
| name | String | 模型生成的应调用函数的名称 |
| arguments | Object | 模型生成的 JSON 格式的函数调用参数。请注意，模型生成的 JSON 并不总是有效的，可能会出现函数模式未定义的参数。在调用函数之前，请在代码中验证参数。 |
| id | String | 此消息调用的tool id。 |
| type | String | 模型调用工具的类型，目前只可能是function |
| usage | Object | 本次模型调用的 tokens 数量统计 |
| prompt_tokens | Integer | 用户输入的 tokens 数量 |
| completion_tokens | Integer | 模型输出的 tokens 数量 |
| total_tokens | Integer | 总 tokens 数量 |

## 非流式调用

#### 请求示例

```
from zhipuai import ZhipuAI
client = ZhipuAI(api_key=&quot;&quot;) 
response = client.chat.completions.create(
    model=&quot;glm-4&quot;,  
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好&quot;},
        {&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: &quot;我是人工智能助手&quot;},
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你叫什么名字&quot;},
        {&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: &quot;我叫chatGLM&quot;},
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你都可以做些什么事&quot;}
    ],
)
print(response.choices[0].message)

```

#### 响应示例

```
{
  &quot;created&quot;: 1703487403,
  &quot;id&quot;: &quot;8239375684858666781&quot;,
  &quot;model&quot;: &quot;glm-4&quot;,
  &quot;request_id&quot;: &quot;8239375684858666781&quot;,
  &quot;choices&quot;: [
      {
          &quot;finish_reason&quot;: &quot;stop&quot;,
          &quot;index&quot;: 0,
          &quot;message&quot;: {
              &quot;content&quot;: &quot;\n作为一个人工智能助手，我可以帮助你完成多种任务，包括但不限于：\n\n1. 回答问题......&quot;,
              &quot;role&quot;: &quot;assistant&quot;
          }
      }
  ],
  &quot;usage&quot;: {
      &quot;completion_tokens&quot;: 217,
      &quot;prompt_tokens&quot;: 31,
      &quot;total_tokens&quot;: 248
  }
}
```

## 流式调用

#### 请求示例

```
from zhipuai import ZhipuAI

client = ZhipuAI(api_key=&quot;&quot;) # 请填写您自己的APIKey

tools = [
    {
        &quot;type&quot;: &quot;function&quot;,
        &quot;function&quot;: {
            &quot;name&quot;: &quot;query_train_info&quot;,
            &quot;description&quot;: &quot;根据用户提供的信息，查询对应的车次&quot;,
            &quot;parameters&quot;: {
                &quot;type&quot;: &quot;object&quot;,
                &quot;properties&quot;: {
                    &quot;departure&quot;: {
                        &quot;type&quot;: &quot;string&quot;,
                        &quot;description&quot;: &quot;出发城市或车站&quot;,
                    },
                    &quot;destination&quot;: {
                        &quot;type&quot;: &quot;string&quot;,
                        &quot;description&quot;: &quot;目的地城市或车站&quot;,
                    },
                    &quot;date&quot;: {
                        &quot;type&quot;: &quot;string&quot;,
                        &quot;description&quot;: &quot;要查询的车次日期&quot;,
                    },
                },
                &quot;required&quot;: [&quot;departure&quot;, &quot;destination&quot;, &quot;date&quot;],
            },
        }
    }
]
messages = [
    {
        &quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: &quot;你能帮我查询2024年1月1日从北京南站到上海的火车票吗？&quot;
    }
]
response = client.chat.completions.create(
    model=&quot;glm-4&quot;, 
    messages=messages,
    tools=tools,
    tool_choice=&quot;auto&quot;,
)
print(response.choices[0].message)
```

#### 响应示例

```
{
  &quot;id&quot;: &quot;8231168139794583938&quot;,
  &quot;model&quot;: &quot;glm-4&quot;,
  &quot;request_id&quot;: &quot;8231168139794583938&quot;,
  &quot;created&quot;: 1703490288,
  &quot;choices&quot;: [
      {
          &quot;finish_reason&quot;: &quot;tool_calls&quot;,
          &quot;index&quot;: 0,
          &quot;message&quot;: {
              &quot;role&quot;: &quot;assistant&quot;,
              &quot;tool_calls&quot;: [
                  {
                      &quot;id&quot;: &quot;call_8231168139794583938&quot;,
                      &quot;index&quot;: 0,
                      &quot;type&quot;: &quot;function&quot;,
                      &quot;function&quot;: {
                          &quot;arguments&quot;: &quot;{&quot;date&quot;: &quot;2024-01-01&quot;,&quot;departure&quot;: &quot;北京南站&quot;,&quot;destination&quot;: &quot;上海&quot;}&quot;,
                          &quot;name&quot;: &quot;query_train_info&quot;
                      }
                  }
              ]
          }
      }
  ],
  &quot;usage&quot;: {
      &quot;completion_tokens&quot;: 31,
      &quot;prompt_tokens&quot;: 120,
      &quot;total_tokens&quot;: 151
  }
}
```

## 异步调用

### 请求示例

```
from zhipuai import ZhipuAI
 
client = ZhipuAI(api_key=&quot;&quot;) # 请填写您自己的APIKey
response = client.chat.asyncCompletions.create(
    model=&quot;glm-4&quot;,  # 填写需要调用的模型名称
    messages=[
        {
            &quot;role&quot;: &quot;user&quot;,
            &quot;content&quot;: &quot;请你作为童话故事大王，写一篇短篇童话故事，故事的主题是要永远保持一颗善良的心，要能够激发儿童的学习兴趣和想象力，同时也能够帮助儿童更好地理解和接受故事中所蕴含的道理和价值观。&quot;
        }
    ],
)
print(response)
```

### 响应示例

```
id=&#x27;123456789&#x27; request_id=&#x27;654321&#x27; model=&#x27;glm-4&#x27; task_status=&#x27;PROCESSING&#x27;
```

## 异步任务结果查询

### 请求示例

```
import time
from zhipuai import ZhipuAI

client = ZhipuAI(api_key=&quot;&quot;) # 请填写您自己的APIKey

response = client.chat.asyncCompletions.create(
    model=&quot;glm-4&quot;,  # 填写需要调用的模型名称
    messages=[
        {
            &quot;role&quot;: &quot;user&quot;,
            &quot;content&quot;: &quot;请你作为童话故事大王，写一篇短篇童话故事，故事的主题是要永远保持一颗善良的心，要能够激发儿童的学习兴趣和想象力，同时也能够帮助儿童更好地理解和接受故事中所蕴含的道理和价值观。&quot;
        }
    ],
)
task_id = response.id
task_status = &#x27;&#x27;
get_cnt = 0

while task_status != &#x27;SUCCESS&#x27; and task_status != &#x27;FAILED&#x27; and get_cnt &lt;= 40:
    result_response = client.chat.asyncCompletions.retrieve_completion_result(id=task_id)
    print(result_response)
    task_status = result_response.task_status

    time.sleep(2)
    get_cnt += 1
    
```

### 响应示例

```
{&quot;id&quot;:&quot;123456789&quot;,&quot;request_id&quot;:&quot;123123123&quot;,&quot;model&quot;:null,&quot;task_status&quot;:&quot;PROCESSING&quot;}
{&quot;id&quot;:&quot;123456789&quot;,&quot;request_id&quot;:&quot;123123123&quot;,&quot;model&quot;:null,&quot;task_status&quot;:&quot;PROCESSING&quot;}

... ...

{&quot;id&quot;:&quot;123456789&quot;,&quot;request_id&quot;:&quot;123123123&quot;,&quot;model&quot;:null,&quot;task_status&quot;:&quot;PROCESSING&quot;}
{
    &quot;id&quot;: &quot;123456789&quot;,
    &quot;request_id&quot;: &quot;123123123&quot;,
    &quot;model&quot;: &quot;chatglm3_beta&quot;,
    &quot;task_status&quot;: &quot;SUCCESS&quot;,
    &quot;choices&quot;: [
        {
            &quot;index&quot;: 0,
            &quot;finish_reason&quot;: &quot;stop&quot;,
            &quot;message&quot;: {
                &quot;content&quot;: &quot;从前，有一个美丽的村庄，村子里的孩子们都喜欢一起玩耍、学习和探索。其中，有一个叫小明的小男孩，他有一颗善良的心，总是乐于助人。

有一天，小明在森林里捡到了一只小鸟，它的翅膀受伤了，无法飞翔。小明心疼地把小鸟带回家，用自己的温暖和关爱照顾它。在小明的精心照料下，小鸟的翅膀渐渐康复，它开始在小明的房间里飞翔。

看到小鸟康复得如此神奇，小明产生了浓厚的兴趣，他想知道更多关于小鸟的知识。于是，他开始阅读关于鸟类的书籍，了解它们的习性和生活习惯。通过学习，小明变得越来越了解小鸟，他和小鸟也建立了深厚的友谊。

有一天，小明在森林里散步时，发现了一只小兔子被困在了一个猎人设下的陷阱里。小明毫不犹豫地走过去，帮助小兔子解开了陷阱。小兔子感激地看着小明，告诉他森林里有一个神秘的宝藏，那是一颗能实现愿望的神奇宝石。

小明充满好奇心，决定去寻找那颗宝石。他带着小鸟和小兔子一起踏上了探险之旅。在探险过程中，他们遇到了各种困难，但小明始终保持着善良的心，勇敢地面对挑战。他不仅学会了如何与森林里的动物相处，还掌握了许多生存技巧。

经过一段时间的努力，小明终于找到了那颗神奇的宝石。宝石散发着光芒，把小明和他的朋友们带到了一个美丽的世界。在那里，他们遇到了一位智慧的老者。老者告诉小明，这颗宝石的力量源于一个人的善良之心。只有拥有善良之心的人，才能激发宝石的力量，实现自己的愿望。

小明明白了这个道理，他感激地和老者告别，带着宝石回到了现实世界。他把宝石的力量用在了帮助别人上，让村子变得更加美好。而小明也成为了村子里的榜样，他用自己的行动告诉孩子们，要永远保持一颗善良的心。

从此，小明和村民们过上了幸福快乐的生活。而那些听过小明故事的孩子们，也明白了善良之心的重要性。他们纷纷向小明学习，努力成为一个有爱心、有责任感的人。在这个过程中，他们的学习兴趣和想象力也被激发，成长为了一个个优秀的孩子。

这个故事告诉我们，要永远保持一颗善良的心，用自己的行动去影响身边的人。只有拥有善良之心的人，才能激发自己的潜能，实现自己的梦想。让我们一起努力，成为那个拥有善良之心的人吧！&quot;,
                &quot;role&quot;: &quot;assistant&quot;,
                &quot;tool_calls&quot;: null
            }
        }
    ],
    &quot;usage&quot;: {
        &quot;prompt_tokens&quot;: 52,
        &quot;completion_tokens&quot;: 470,
        &quot;total_tokens&quot;: 522
    }
}
```

## 工具调用

ChatGLM 的函数调用功能可以增强模型推理效果或进行其他外部操作，包括信息检索、数据库操作、知识图谱搜索与推理、操作系统、触发外部操作等工具调用场景。

本教程将介绍如何使用 ChatGLM 的函数调用功能，实现对模型与外部函数库的连接。

tools 是内容生成 API 中的可选参数，用于向模型提供函数定义。通过此参数，模型能够生成符合用户所提供规范的函数参数。请注意，API 实际上不会执行任何函数调用，仅返回调用函数所需要的参数。开发者可以利用模型输出的参数在应用中执行函数调用。

本部分将带领开发者了解以下3个部分的内容：

1.  如何使用 Chat Completion 接口向模型描述外部函数。

1.  如何与模型交互，触发模型对函数的调用。

1.  如何使用模型生成的结果调用外部函数。

### 描述外部函数

假设我们要创建一个具备查询航班功能的聊天机器人。我们定义如下两个外部函数供模型选择调用：

1.  查询两地之间某日航班号函数：get_flight_number(departure: str, destination: str, date: str)

1.  查询某航班某日票价函数：get_ticket_price(flight_number: str, date: str)

#### 描述函数功能

为了向模型描述外部函数库，需要向 tools 字段传入可以调用的函数列表。参数如下表：

Python 示例代码

```
tools = [
    {
        &quot;type&quot;: &quot;function&quot;,
        &quot;function&quot;: {
            &quot;name&quot;: &quot;get_flight_number&quot;,
            &quot;description&quot;: &quot;根据始发地、目的地和日期，查询对应日期的航班号&quot;,
            &quot;parameters&quot;: {
                ......
            },
        }
    },
    {
        &quot;type&quot;: &quot;function&quot;,
        &quot;function&quot;: {
            &quot;name&quot;: &quot;get_ticket_price&quot;,
            &quot;description&quot;: &quot;查询某航班在某日的票价&quot;,
            &quot;parameters&quot;: {
                ......
            },
        }
    },
]
```

此处先省略parameters参数，我们将在下一节介绍如何描述函数所需参数。

#### 编写函数参数列表的 JSON 描述

为了准确定义函数的参数列表，在编写参数列表的 JSON Schema 时建议最少包含以下字段：

+   description ：说明函数方法的用途。

+   type ：定义 JSON 数据的数据类型约束。

+   properties：一个Object，其中的每个属性代表要定义的 JSON 数据中的一个键。

+   required：指定哪些属性在数据中必须被包含。

+   enum：如果一个属性是枚举类型，则此字段应当设置为枚举值的数组。

则完整的tools字段设置为：

```
tools = [
    {
        &quot;type&quot;: &quot;function&quot;,
        &quot;function&quot;: {
            &quot;name&quot;: &quot;get_flight_number&quot;,
            &quot;description&quot;: &quot;根据始发地、目的地和日期，查询对应日期的航班号&quot;,
            &quot;parameters&quot;: {
                &quot;type&quot;: &quot;object&quot;,
                &quot;properties&quot;: {
                    &quot;departure&quot;: {
                        &quot;description&quot;: &quot;出发地&quot;,
                        &quot;type&quot;: &quot;string&quot;
                    },
                    &quot;destination&quot;: {
                        &quot;description&quot;: &quot;目的地&quot;,
                        &quot;type&quot;: &quot;string&quot;
                    },
                    &quot;date&quot;: {
                        &quot;description&quot;: &quot;日期&quot;,
                        &quot;type&quot;: &quot;string&quot;,
                    }
                },
                &quot;required&quot;: [ &quot;departure&quot;, &quot;destination&quot;, &quot;date&quot; ]
            },
        }
    },
    {
        &quot;type&quot;: &quot;function&quot;,
        &quot;function&quot;: {
            &quot;name&quot;: &quot;get_ticket_price&quot;,
            &quot;description&quot;: &quot;查询某航班在某日的票价&quot;,
            &quot;parameters&quot;: {
                &quot;type&quot;: &quot;object&quot;,
                &quot;properties&quot;: {
                    &quot;flight_number&quot;: {
                        &quot;description&quot;: &quot;航班号&quot;,
                        &quot;type&quot;: &quot;string&quot;
                    },
                    &quot;date&quot;: {
                        &quot;description&quot;: &quot;日期&quot;,
                        &quot;type&quot;: &quot;string&quot;,
                    }
                },
                &quot;required&quot;: [ &quot;flight_number&quot;, &quot;date&quot;]
            },
        }
    },
]
```

#### **函数调用选择**

在 tools参数中，如果填写了 functions 参数，则默认情况下模型将决定何时适合使用其中一个函数。  如果要控制模型如何选择函数调用，需要设置 tool_choice  参数。

参数默认值为auto，此时模型根据上下文信息自行选择是否返回函数调用。

将其设置为 {"name":  "your_function_name"} 时，可以强制 API 返回特定函数的调用。

还可以通过将 tool_choice 参数设置为  none 来强制 API 不返回任何函数的调用。

### Function Call 流程实践

本节将以上文定义的具备查询航班功能的聊天机器人为例，介绍如何与模型对话完成函数调用。

#### 构建工具和调用

+   首先，我们需要初始化函数定义和客户端：

```
from zhipuai import ZhipuAI
client = ZhipuAI(api_key=&quot;&quot;)
messages = []
tools = [
    {
        &quot;type&quot;: &quot;function&quot;,
        &quot;function&quot;: {
            &quot;name&quot;: &quot;get_flight_number&quot;,
            &quot;description&quot;: &quot;根据始发地、目的地和日期，查询对应日期的航班号&quot;,
            &quot;parameters&quot;: {
                &quot;type&quot;: &quot;object&quot;,
                &quot;properties&quot;: {
                    &quot;departure&quot;: {
                        &quot;description&quot;: &quot;出发地&quot;,
                        &quot;type&quot;: &quot;string&quot;
                    },
                    &quot;destination&quot;: {
                        &quot;description&quot;: &quot;目的地&quot;,
                        &quot;type&quot;: &quot;string&quot;
                    },
                    &quot;date&quot;: {
                        &quot;description&quot;: &quot;日期&quot;,
                        &quot;type&quot;: &quot;string&quot;,
                    }
                },
                &quot;required&quot;: [ &quot;departure&quot;, &quot;destination&quot;, &quot;date&quot; ]
            },
        }
    },
    {
        &quot;type&quot;: &quot;function&quot;,
        &quot;function&quot;: {
            &quot;name&quot;: &quot;get_ticket_price&quot;,
            &quot;description&quot;: &quot;查询某航班在某日的票价&quot;,
            &quot;parameters&quot;: {
                &quot;type&quot;: &quot;object&quot;,
                &quot;properties&quot;: {
                    &quot;flight_number&quot;: {
                        &quot;description&quot;: &quot;航班号&quot;,
                        &quot;type&quot;: &quot;string&quot;
                    },
                    &quot;date&quot;: {
                        &quot;description&quot;: &quot;日期&quot;,
                        &quot;type&quot;: &quot;string&quot;,
                    }
                },
                &quot;required&quot;: [ &quot;flight_number&quot;, &quot;date&quot;]
            },
        }
    },
]
messages = []
```

+   我们向模型提供这个信息来查询2024年1月20日从北京前往上海的航班。

```
messages = []
messages.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;帮我查询从2024年1月20日，从北京出发前往上海的航班&quot;})
response = client.chat.completions.create(
    model=&quot;glm-4&quot;,  # 填写需要调用的模型名称
    messages=messages,
    tools=tools,
)
print(response.choices[0].message)
messages.append(response.choices[0].message.model_dump())
```

+   此时模型成功触发对get_flight_numbe函数的调用，参数为：

date="2024-01-20",departure="北京",destination="上海"

```
此时模型成功触发对get_flight_number函数的调用，参数为：date=&quot;2024-01-20&quot;,departure=&quot;北京&quot;,destination=&quot;上海&quot; 
```

+   现在，清空消息历史。我们尝试提供信息，触发模型对get_ticket_price函数的调用。

```
messages = []
messages.append({&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;不要假设或猜测传入函数的参数值。如果用户的描述不明确，请要求用户提供必要信息&quot;})
messages.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;帮我查询1234航班的票价&quot;})
response = client.chat.completions.create(
    model=&quot;glm-4&quot;,  # 填写需要调用的模型名称
    messages=messages,
    tools=tools,
    tool_choice={&quot;type&quot;: &quot;function&quot;, &quot;function&quot;: {&quot;name&quot;: &quot;get_ticket_price&quot;}},
)
print(response.choices[0].message)
messages.append(response.choices[0].message.model_dump())
```

+   此时模型被强制触发对get_ticket_price函数的调用，参数为：

date="2022-01-01",flight_number="1234"。

> 注意到此时模型假设了一个date。 这会导致得到错误的答案。

我们也可以强制模型不调用函数。需要设置tool_choice为none。

#### 使用模型生成的参数调用函数

将所需的函数实现

```
def get_flight_number(date:str , departure:str , destination:str):
    flight_number = {
        &quot;北京&quot;:{
            &quot;上海&quot; : &quot;1234&quot;,
            &quot;广州&quot; : &quot;8321&quot;,
        },
        &quot;上海&quot;:{
            &quot;北京&quot; : &quot;1233&quot;,
            &quot;广州&quot; : &quot;8123&quot;,
        }
    }
    return { &quot;flight_number&quot;:flight_number[departure][destination] }
def get_ticket_price(date:str , flight_number:str):
    return {&quot;ticket_price&quot;: &quot;1000&quot;}
```

定义处理 Function call 的函数：

```
def parse_function_call(model_response,messages):
    # 处理函数调用结果，根据模型返回参数，调用对应的函数。
    # 调用函数返回结果后构造tool message，再次调用模型，将函数结果输入模型
    # 模型会将函数调用结果以自然语言格式返回给用户。
    if model_response.choices[0].message.tool_calls:
        tool_call = model_response.choices[0].message.tool_calls[0]
        args = tool_call.function.arguments
        function_result = {}
        if tool_call.function.name == &quot;get_flight_number&quot;:
            function_result = get_flight_number(**json.loads(args))
        if tool_call.function.name == &quot;get_ticket_price&quot;:
            function_result = get_ticket_price(**json.loads(args))
        messages.append({
            &quot;role&quot;: &quot;tool&quot;, 
            &quot;content&quot;: f&quot;{json.dumps(function_result)}&quot;,
            &quot;tool_call_id&quot;:tool_call.id
        })
        response = client.chat.completions.create(
            model=&quot;glm-4&quot;,  # 填写需要调用的模型名称
            messages=messages,
            tools=tools,
        )
        print(response.choices[0].message)
        messages.append(response.choices[0].message.model_dump())
```

查询北京到广州的航班：

```
# 清空对话
messages = []

messages.append({&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;不要假设或猜测传入函数的参数值。如果用户的描述不明确，请要求用户提供必要信息&quot;})
messages.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;帮我查询1月23日，北京到广州的航班&quot;})

response = client.chat.completions.create(
    model=&quot;glm-4&quot;,  # 填写需要调用的模型名称
    messages=messages,
    tools=tools,
)
print(response.choices[0].message)
messages.append(response.choices[0].message.model_dump())

parse_function_call(response,messages)
```

返回

```
content=None role=&#x27;assistant&#x27; tool_calls=[CompletionMessageToolCall(id=&#x27;call_8282666790542042140&#x27;, function=Function(arguments=&#x27;{&quot;date&quot;:&quot;2023-01-23&quot;,&quot;departure&quot;:&quot;北京&quot;,&quot;destination&quot;:&quot;广州&quot;}&#x27;, name=&#x27;get_flight_number&#x27;), type=&#x27;function&#x27;)]
content=&#x27;根据您的要求，我已经查询到了1月23日从北京到广州的航班号，航班号为8321。&#x27; role=&#x27;assistant&#x27; tool_calls=None
```

查询1234航班票价：

```
messages.append({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;这趟航班的价格是多少？&quot;})
response = client.chat.completions.create(
    model=&quot;glm-4&quot;,  # 填写需要调用的模型名称
    messages=messages,
    tools=tools,
)
print(response.choices[0].message)
messages.append(response.choices[0].message.model_dump())

parse_function_call(response,messages)
```

返回

```
content=None role=&#x27;assistant&#x27; tool_calls=[CompletionMessageToolCall(id=&#x27;call_8282666893621289712&#x27;, function=Function(arguments=&#x27;{&quot;date&quot;:&quot;2023-01-23&quot;,&quot;flight_number&quot;:&quot;8321&quot;}&#x27;, name=&#x27;get_ticket_price&#x27;), type=&#x27;function&#x27;)]
content=&#x27;这趟航班的票价为1000元。&#x27; role=&#x27;assistant&#x27; tool_calls=None
```

到这里，我们就完成了使用API进行基础的工具调用