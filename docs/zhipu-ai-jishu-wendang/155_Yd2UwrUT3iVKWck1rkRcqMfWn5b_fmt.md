
# CodeGeeX 代码辅助模型

> 来源：[https://ai.feishu.cn/docx/HfqwdKPmdoTdfzxLFbIcBmcansd](https://ai.feishu.cn/docx/HfqwdKPmdoTdfzxLFbIcBmcansd)
>
> CodeGeeX 是一款先进的人工智能编程助手。它通过智能问答和代码补全两种功能来提高用户的编码效率，并且支持多种编程语言。
>
> 模型编码：`codegeex-4`
>
> API调用接口地址：`https://bigmodel.cn/dev/api#codegeex-4`

## CodeGeeX Cookbook 调用教程

### 智能问答

#### sse调用

##### 接口请求

| 传输方式 | https |
| --- | --- |
| 请求地址 | `https://open.bigmodel.cn/api/paas/v4/chat/completions` |
| 调用方式 | 同步调用，等待模型执行完成并返回最终结果或 SSE 调用 |
| 字符编码 | UTF-8 |
| 接口请求格式 | JSON |
| 响应格式 | JSON 或标准 Stream Event |
| 接口请求类型 | POST |
| 开发语言 | 任意可发起 HTTP 请求的开发语言 |

##### 接口请求参数

| 参数名称 | 类型 | 是否必填 | 参数说明 |
| --- | --- | --- | --- |
| `model` | `String` | 是 | 模型编码 |
| `messages` | `List<Object>` | 是 | 调用模型时，将当前对话信息列表作为提示输入给模型，按照 `{"role": "user", "content": "你好"}` 的 JSON 数组形式进行传参；可能的消息类型包括 System message、User message和Assistant message。 |
| `request_id` | `String` | 否 | 由用户端传参，需保证唯一性；用于区分每次请求的唯一标识，用户端不传时平台会默认生成。 |
| `do_sample` | `Boolean` | 否 | `do_sample` 为 `true` 时启用采样策略，`do_sample` 为 `false` 时采样策略 `temperature`、`top_p` 将不生效。默认值为 `true`。 |
| `stream` | `Boolean` | 否 | 使用同步调用时，此参数应当设置为 `false` 或者省略。表示模型生成完所有内容后一次性返回所有内容。默认值为 `false`。 如果设置为 `true`，模型将通过标准 Event Stream ，逐块返回模型生成内容。Event Stream 结束时会返回一条 `data: [DONE]` 消息。 注意：在模型流式输出生成内容的过程中，我们会分批对模型生成内容进行检测，当检测到违法及不良信息时，API会返回错误码（1301）。开发者识别到错误码（1301），应及时采取（清屏、重启对话）等措施删除生成内容，并确保不将含有违法及不良信息的内容传递给模型继续生成，避免其造成负面影响。 |
| `temperature` | `Float` | 否 | 采样温度，控制输出的随机性，必须为正数 取值范围是：`[0.0, 1.0]`，默认值为 `0.95`，值越大，会使输出更随机，更具创造性；值越小，输出会更加稳定或确定 建议您根据应用场景调整 `top_p` 或 `temperature` 参数，但不要同时调整两个参数 |
| `top_p` | `Float` | 否 | 用温度取样的另一种方法，称为核取样 取值范围是：`[0.0, 1.0]`，默认值为 `0.7` 模型考虑具有 `top_p` 概率质量 tokens 的结果 例如：`0.1` 意味着模型解码器只考虑从前 10% 的概率的候选集中取 tokens 建议您根据应用场景调整 `top_p` 或 `temperature` 参数，但不要同时调整两个参数 |
| `max_tokens` | `Integer` | 否 | 模型输出最大 tokens，最大输出为8192，默认值为1024 |
| `stop` | `List` | 否 | 模型在遇到 `stop` 所指定的字符时将停止生成，建议为 `["<|endoftext|>", "<|user|>", "<|assistant|>", "<|observation|>"]` |
| `user_id` | `String` | 否 | 终端用户的唯一ID，协助平台对终端用户的违规行为、生成违法及不良信息或其他滥用行为进行干预。ID长度要求：最少6个字符，最多128个字符。了解更多 |

##### message 字段说明

模型可接受的消息类型包括 System message、User message和Assistant message ，不同的消息类型格式有所差异。具体如下：

###### System Message 格式

| 参数名称 | 类型 | 是否必填 | 参数说明 |
| --- | --- | --- | --- |
| `role` | `String` | 是 | 消息的角色信息，此时应为 `system` |
| `content` | `String` | 是 | 消息内容 |

###### User Message 格式

| 参数名称 | 类型 | 是否必填 | 参数说明 |
| --- | --- | --- | --- |
| `role` | `String` | 是 | 消息的角色信息，此时应为 `user` |
| `content` | `String` | 是 | 消息内容 |

###### Assistant Message 格式

| 参数名称 | 类型 | 是否必填 | 参数说明 |
| --- | --- | --- | --- |
| `role` | `String` | 是 | 消息的角色信息，此时应为 `assistant` |
| `content` | `String` | 是 | 消息内容 |

##### 模型同步调用响应内容

| 参数名称 | 类型 | 参数说明 |
| --- | --- | --- |
| `id` | `String` | 任务 ID |
| `created` | `Long` | 请求创建时间，是以秒为单位的 Unix 时间戳 |
| `model` | `String` | 模型名称 |
| `choices` | `List` | 当前对话的模型输出内容 |
| `index` | `Integer` | 结果下标 |
| `finish_reason` | `String` | 模型推理终止原因。 `stop` 代表推理自然结束或触发停止词。 `tool_calls` 代表模型命中函数。 `length` 代表到达 tokens 长度上限。 `sensitive` 代表模型推理内容被安全审核接口拦截。请注意，针对此类内容，请用户自行判断并决定是否撤回已公开的内容。 `network_error` 代表模型推理异常。 |
| `message` | `Object` | 模型返回的文本信息 |
| `role` | `String` | 当前对话的角色，目前默认为 `assistant`（模型） |
| `content` | `String` | 当前对话的内容。命中函数时此字段为 `null`，未命中函数时返回模型推理结果。 |
| `usage` | `Object` | 结束时返回本次模型调用的 tokens 数量统计 |
| `prompt_tokens` | `Integer` | 用户输入的 tokens 数量 |
| `completion_tokens` | `Integer` | 模型输出的 tokens 数量 |
| `total_tokens` | `Integer` | 总 tokens 数量 |
| `content_filter` | `List` | 返回内容安全的相关信息。 |
| `role` | `String` | 安全生效环节，包括 `role = assistant` 模型推理，`role = user` 用户输入，`role = history` 历史上下文 |
| `level` | `Integer` | 严重程度 `level` 0-3，`level` 0表示最严重，3表示轻微 |

##### 内容生成流式响应块内容

| 参数名称 | 类型 | 参数说明 |
| --- | --- | --- |
| `id` | `String` | 智谱 AI 开放平台生成的任务订单号，调用请求结果接口时请使用此订单号 |
| `created` | `Long` | 请求创建时间，是以秒为单位的 Unix 时间戳 |
| `choices` | `List` | 当前对话的模型输出内容 |
| `index` | `Integer` | 结果下标 |
| `finish_reason` | `String` | 模型推理终止的原因。 |
| `delta` | `Object` | 模型增量返回的文本信息 |
| `role` | `String` | 当前对话的角色，目前默认为 `assistant`（模型） |
| `content` | `String` | 当前对话的内容 |
| `usage` | `Object` | 结束时返回本次模型调用的 tokens 数量统计 |
| `prompt_tokens` | `Integer` | 用户输入的 tokens 数量 |
| `completion_tokens` | `Integer` | 模型输出的 tokens 数量 |
| `total_tokens` | `Integer` | 总 tokens 数量 |
| `content_filter` | `List` | 返回内容安全的相关信息。 |
| `role` | `String` | 安全生效环节，包括 `role = assistant` 模型推理，`role = user` 用户输入，`role = history` 历史上下文 |
| `level` | `Integer` | 严重程度 `level` 0-3，`level` 0表示最严重，3表示轻微 |

### Python 调用示例

#### 同步调用

##### 请求示例

```
from zhipuai import ZhipuAI
client = ZhipuAI(api_key="")  # 填写您自己的APIKey
response = client.chat.completions.create(
    model="codegeex-4",  # 填写需要调用的模型名称
    messages=[
        {
            "role": "system",
            "content": "你是一位智能编程助手，你叫CodeGeeX。你会为用户回答关于编程、代码、计算机方面的任何问题，并提供格式规范、可以执行、准确安全的代码，并在必要时提供详细的解释。任务：请为输入代码提供格式规范的注释，包含多行注释和单行注释，请注意不要改动原始代码，只需要添加注释。 请用中文回答。"
        },
        {
            "role": "user",
            "content": "写一个快速排序函数"
        }
    ],
    top_p=0.7,
    temperature=0.9,
    max_tokens=1024,
    stop=["<|endoftext|>", "<|user|>", "<|assistant|>", "<|observation|>"]
)
print(response.choices[0].message)
```

##### 响应示例

```
{
    "created": 1719929866,
    "id": "8804608774531416088",
    "model": "codegeex-4",
    "request_id": "8804608774531416088",
    "choices": [
        {
            "finish_reason": "stop",
            "index": 0,
            "message": {
                "content": "```python\n# 快速排序函数\ndef quick_sort(arr):\n    # 如果数组长度小于等于1，则直接返回\n    if len(arr) <= 1:\n        return arr\n    # 选择数组中的第一个元素作为基准值\n    pivot = arr[0]\n    # 将数组中小于基准值的元素放入left数组，大于基准值的元素放入right数组\n    left = [x for x in arr[1:] if x <= pivot]\n    right = [x for x in arr[1:] if x > pivot]\n    # 递归调用quick_sort函数对left和right数组进行排序，并将结果与基准值拼接起来\n    return quick_sort(left) + [pivot] + quick_sort(right)\n```\n",

                "role": "assistant"

            }

        }

    ],

    "usage": {

        "completion_tokens": 157,
        "prompt_tokens": 92,
        "total_tokens": 249

    }

}
```

#### 流式调用

##### 请求示例

```
from zhipuai import ZhipuAI

client = ZhipuAI(api_key="")  # 填写您自己的APIKey

response = client.chat.completions.create(

    model="codegeex-4",  # 填写需要调用的模型名称

    messages=[

        {

            "role": "system",

            "content": "你是一位智能编程助手，你叫CodeGeeX。你会为用户回答关于编程、代码、计算机方面的任何问题，并提供格式规范、可以执行、准确安全的代码，并在必要时提供详细的解释。任务：请为输入代码提供格式规范的注释，包含多行注释和单行注释，请注意不要改动原始代码，只需要添加注释。 请用中文回答。"

        },

        {

            "role": "user",

            "content": "写一个快速排序函数"

        }

    ],

    top_p=0.7,

    temperature=0.9,

    max_tokens=1024,

    stream=True,

    stop=["<|endoftext|>", "<|user|>", "<|assistant|>", "<|observation|>"]

)

for chunk in response:

    print(chunk.choices[0].delta)

```

##### 响应示例



```python
# 快速排序
... ...
quick_sort(right)
```

# 异步调用

## 接口请求

| 传输方式 | https |
| --- | --- |
| 请求地址 | `https://open.bigmodel.cn/api/paas/v4/async/chat/completions` |
| 调用方式 | 异步调用，需通过查询接口获取结果 |
| 字符编码 | UTF-8 |
| 接口请求格式 | JSON |
| 响应格式 | JSON |
| 接口请求类型 | POST |
| 开发语言 | 任意可发起 HTTP 请求的开发语言 |

## 接口请求参数

与同步调用的接口请求参数相同。

## 接口响应参数

| 参数名称 | 类型 | 参数说明 |
| --- | --- | --- |
| `request_id` | String | 用户在客户端请求时提交的任务编号或者平台生成的任务编号 |
| `id` | String | 智谱 AI 开放平台生成的任务订单号，调用请求结果接口时请使用此订单号 |
| `model` | String | 本次调用的模型名称 |
| `task_status` | String | 处理状态，`PROCESSING`（处理中），`SUCCESS`（成功），`FAIL`（失败）。需通过查询获取结果 |

## Python 调用示例

### 请求示例

```python
from zhipuai import ZhipuAI

client = ZhipuAI(api_key="") # 请填写您自己的APIKey

response = client.chat.asyncCompletions.create(

    model="codegeex-4",  # 填写需要调用的模型名称

    messages=[

        {

            "role": "system",

            "content": "你是一位智能编程助手，你叫CodeGeeX。你会为用户回答关于编程、代码、计算机方面的任何问题，并提供格式规范、可以执行、准确安全的代码，并在必要时提供详细的解释。任务：请为输入代码提供格式规范的注释，包含多行注释和单行注释，请注意不要改动原始代码，只需要添加注释。 请用中文回答。"

        },

        {

            "role": "user",

            "content": "写一个快速排序函数"

        }

    ],

    top_p=0.7,

    temperature=0.9,

    max_tokens=2000,

    stop=["<|endoftext|>", "<|user|>", "<|assistant|>", "<|observation|>"]

)

print(response)
```

### 响应示例

```
id='850817101390499798804608774531416095' request_id='8804608774531416094' model='codegeex-4' task_status='PROCESSING'
```

## 任务结果查询

### 接口请求

| 传输方式 | https |
| --- | --- |
| 请求地址 | `https://open.bigmodel.cn/api/paas/v4/async-result/{id}` |
| 调用方式 | 同步调用，等待模型执行完成并返回最终结果 |
| 字符编码 | UTF-8 |
| 接口请求格式 | JSON |
| 响应格式 | JSON |
| 接口请求类型 | GET |
| 开发语言 | 任意可发起 HTTP 请求的开发语言 |

### 请求参数

| 参数名称 | 类型 | 是否必填 | 参数说明 |
| --- | --- | --- | --- |
| `id` | String | 是 | 任务 ID |

### 接口响应参数

| 参数名称 | 类型 | 参数说明 |
| --- | --- | --- |
| `model` | String | 模型名称 |
| `choices` | List | 当前对话的模型输出内容 |
| `index` | Integer | 结果下标 |
| `finish_reason` | String | 模型推理终止的原因。 |
| `message` | Object | 模型增量返回的文本信息 |
| `role` | String | 当前对话的角色，目前默认为 `assistant`（模型） |
| `content` | String | 当前对话的内容。命中函数时此字段为 `null`，未命中函数时返回模型推理结果。 |
| `task_status` | String | 处理状态，`PROCESSING`（处理中），`SUCCESS`（成功），`FAIL`（失败） 注：处理中状态需通过查询获取结果 |
| `request_id` | String | 用户在客户端请求时提交的任务编号或者平台生成的任务编号 |
| `id` | String | 智谱 AI 开放平台生成的任务订单号，调用请求结果接口时请使用此订单号 |
| `usage` | Object | 结束时返回本次模型调用的 tokens 数量统计 |
| `prompt_tokens` | Integer | 用户输入的 tokens 数量 |
| `completion_tokens` | Integer | 模型输出的 tokens 数量 |
| `total_tokens` | Integer | 总 tokens 数量 |
| `content_filter` | List | 返回内容安全的相关信息。 |
| `role` | String | 安全生效环节，包括 `role = assistant` 模型推理，`role = user` 用户输入，`role = history` 历史上下文 |
| `level` | Integer | 严重程度 `level` 0-3，`level` 0表示最严重，3表示轻微 |

### Python 调用示例

#### 请求示例

```python
import time

from zhipuai import ZhipuAI

client = ZhipuAI(api_key="") # 请填写您自己的APIKey

response = client.chat.asyncCompletions.create(

    model="codegeex-4",  # 填写需要调用的模型名称

    messages=[

        {

            "role": "system",

            "content": "你是一位智能编程助手，你叫CodeGeeX。你会为用户回答关于编程、代码、计算机方面的任何问题，并提供格式规范、可以执行、准确安全的代码，并在必要时提供详细的解释。任务：请为输入代码提供格式规范的注释，包含多行注释和单行注释，请注意不要改动原始代码，只需要添加注释。 请用中文回答。"

        },

        {

            "role": "user",

            "content": "写一个快速排序函数"

        }

    ],

    top_p=0.7,

    temperature=0.9,

    max_tokens=2000,

    stop=["<|endoftext|>", "<|user|>", "<|assistant|>", "<|observation|>"]

)

task_id = response.id

task_status = ''

get_cnt = 0

while task_status != 'SUCCESS' and task_status != 'FAILED' and get_cnt <= 40:

    result_response = client.chat.asyncCompletions.retrieve_completion_result(id=task_id)

    print(result_response)

    task_status = result_response.task_status

    time.sleep(2)

    get_cnt += 1
```

#### 响应示例

```
{"id":"850817101390499798804608774531416097","request_id":"8804608774531416096","model":null,"task_status":"PROCESSING"}

{"id":"850817101390499798804608774531416097","request_id":"8804608774531416096","model":null,"task_status":"PROCESSING"}

... ...

{"id":"850817101390499798804608774531416097","request_id":"8804608774531416096","model":null,"task_status":"PROCESSING"}

{

    "created": 1719930768,

    "id": "850817101390499798804608774531416097",

    "model": "codegeex-4",
```



```json
{
    "request_id": "8804608774531416096",

    "task_status": "SUCCESS",

    "choices": [

        {

            "finish_reason": "stop",

            "index": 0,

            "message": {

                "content": "```python\n# 快速排序函数\ndef quick_sort(arr):\n    # 如果数组长度小于等于1，则直接返回数组\n    if len(arr) <= 1:\n        return arr\n    # 选择基准值，这里选择第一个元素\n    pivot = arr[0]\n    # 定义左右两个子数组\n    left = []\n    right = []\n    # 遍历数组，将小于基准值的元素放入左子数组，大于基准值的元素放入右子数组\n    for i in range(1, len(arr)):\n        if arr[i] < pivot:\n            left.append(arr[i])\n        else:\n            right.append(arr[i])\n    # 递归调用快速排序函数，对左右两个子数组进行排序\n    return quick_sort(left) + [pivot] + quick_sort(right)\n```\n",
                "role": "assistant"
            }
        }
    ],
    "usage": {
        "completion_tokens": 173,
        "prompt_tokens": 92,
        "total_tokens": 265
    }
}
```

# 代码补全

## SSE 调用

| 传输方式 | https |
| --- | --- |
| 请求地址 | `https://open.bigmodel.cn/api/paas/v4/chat/completions` |
| 调用方式 | 同步调用，等待模型执行完成并返回最终结果或 SSE 调用 |
| 字符编码 | UTF-8 |
| 接口请求格式 | JSON |
| 响应格式 | JSON 或标准 Stream Event |
| 接口请求类型 | POST |
| 开发语言 | 任意可发起 HTTP 请求的开发语言 |

### 接口请求参数

| 参数名称 | 类型 | 是否必填 | 参数说明 |
| --- | --- | --- | --- |
| `model` | `String` | 是 | 模型编码 |
| `request_id` | `String` | 否 | 由用户端传参，需保证唯一性；用于区分每次请求的唯一标识，用户端不传时平台会默认生成。 |
| `do_sample` | `Boolean` | 否 | `do_sample` 为 true 时启用采样策略，`do_sample` 为 false 时采样策略 `temperature`、`top_p` 将不生效。默认值为 true。 |
| `stream` | `Boolean` | 否 | 使用同步调用时，此参数应当设置为 fasle 或者省略。表示模型生成完所有内容后一次性返回所有内容。默认值为 false。  如果设置为 true，模型将通过标准 Event Stream ，逐块返回模型生成内容。Event Stream 结束时会返回一条`data: [DONE]`消息。  注意：在模型流式输出生成内容的过程中，我们会分批对模型生成内容进行检测，当检测到违法及不良信息时，API会返回错误码（1301）。开发者识别到错误码（1301），应及时采取（清屏、重启对话）等措施删除生成内容，并确保不将含有违法及不良信息的内容传递给模型继续生成，避免其造成负面影响。 |
| `temperature` | `Float` | 否 | 采样温度，控制输出的随机性，必须为正数  取值范围是：[0.0, 1.0]，默认值为 0.95，值越大，会使输出更随机，更具创造性；值越小，输出会更加稳定或确定  建议您根据应用场景调整 `top_p` 或 `temperature` 参数，但不要同时调整两个参数 |
| `top_p` | `Float` | 否 | 用温度取样的另一种方法，称为核取样  取值范围是：[0.0, 1.0]，默认值为 0.7  模型考虑具有 `top_p` 概率质量 tokens 的结果  例如：0.1 意味着模型解码器只考虑从前 10% 的概率的候选集中取 tokens  建议您根据应用场景调整 `top_p` 或 `temperature` 参数，但不要同时调整两个参数 |
| `max_tokens` | `Integer` | 否 | 模型输出最大 tokens，最大输出为8192，默认值为1024 |
| `stop` | `List` | 否 | 模型在遇到`stop`所指定的字符时将停止生成，建议为`["<|endoftext|>","<|user|>", "<|assistant|>", "<|observation|>"]` |
| `extra` | `Object` | 否 | 代码补全相关的参数 |
| `target` | `Object` | 否 | 补全的内容参数 |
| `path` | `String` | 否 | 文件路径 |
| `language` | `String` | 否 | 代码语言类型，支持Python、Java、C、C++等 100 多种编程语言 |
| `code_prefix` | `String` | 否 | 补全位置的前文 |
| `code_suffix` | `String` | 否 | 补全位置的后文 |
| `contexts` | `String` | 否 | 附加代码文件上下文，一个`context`表示一个附加文件 |
| `path` | `String` | 否 | 附加代码文件的路径 |
| `code` | `String` | 否 | 附加的代码内容 |
| `user_id` | `String` | 否 | 终[端用户的](https://bigmodel.cn/dev/howuse/securityaudit)唯一ID，协助平台对终端用户的违规行为、生成违法及不良信息或其他滥用行为进行干预。ID长度要求：最少6个字符，最多128个字符。了解更多 |

### 模型同步调用响应内容

| 参数名称 | 类型 | 参数说明 |
| --- | --- | --- |
| `id` | `String` | 任务 ID |
| `created` | `Long` | 请求创建时间，是以秒为单位的 Unix 时间戳 |
| `model` | `String` | 模型名称 |
| `choices` | `List` | 当前对话的模型输出内容 |
| `index` | `Integer` | 结果下标 |
| `finish_reason` | `String` | 模型推理终止原因。  `stop`代表推理自然结束或触发停止词。  `tool_calls` 代表模型命中函数。  `length`代表到达 tokens 长度上限。  `sensitive` 代表模型推理内容被安全审核接口拦截。请注意，针对此类内容，请用户自行判断并决定是否撤回已公开的内容。  `network_error` 代表模型推理异常。 |
| `message` | `Object` | 模型返回的文本信息 |
| `role` | `String` | 当前对话的角色，目前默认为 `assistant`（模型） |
| `content` | `String` | 当前对话的内容。命中函数时此字段为`null`，未命中函数时返回模型推理结果。 |
| `usage` | `Object` | 结束时返回本次模型调用的 tokens 数量统计 |
| `prompt_tokens` | `Integer` | 用户输入的 tokens 数量 |
| `completion_tokens` | `Integer` | 模型输出的 tokens 数量 |
| `total_tokens` | `Integer` | 总 tokens 数量 |
| `content_filter` | `List` | 返回内容安全的相关信息。 |
| `role` | `String` | 安全生效环节，包括 `role = assistant` 模型推理，`role = user` 用户输入，`role = history` 历史上下文 |
| `level` | `Integer` | 严重程度 `level` 0-3，`level` 0表示最严重，3表示轻微 |

### 内容生成流式响应块内容

| 参数名称 | 类型 | 参数说明 |
| --- | --- | --- |
| `id` | `String` | 智谱 AI 开放平台生成的任务订单号，调用请求结果接口时请使用此订单号 |
| `created` | `Long` | 请求创建时间，是以秒为单位的 Unix 时间戳 |
| `choices` | `List` | 当前对话的模型输出内容 |
| `index` | `Integer` | 结果下标 |
| `finish_reason` | `String` | 模型推理终止的原因。 |
| `delta` | `Object` | 模型增量返回的文本信息 |
| `role` | `String` | 当前对话的角色，目前默认为 `assistant`（模型） |
| `content` | `String` | 当前对话的内容 |
| `usage` | `Object` | 结束时返回本次模型调用的 tokens 数量统计 |
| `prompt_tokens` | `Integer` | 用户输入的 tokens 数量 |
| `completion_tokens` | `Integer` | 模型输出的 tokens 数量 |
| `total_tokens` | `Integer` | 总 tokens 数量 |
| `content_filter` | `List` | 返回内容安全的相关信息。 |
| `role` | `String` | 安全生效环节，包括 `role = assistant` 模型推理，`role = user` 用户输入，`role = history` 历史上下文 |
| `level` | `Integer` | 严重程度 `level` 0-3，`level` 0表示最严重，3表示轻微 |

## Python 调用示例

### 同步调用

#### 请求示例

```python
from zhipuai import ZhipuAI
client = ZhipuAI(api_key="") # 请填写您自己的APIKey
response = client.chat.completions.create(
    model="codegeex-4", # 填写需要调用的模型名称
    messages=[],
    extra={
        "target": {
            "path": "quick_srot.py",
            "language": "Python",
            "code_prefix": "def quick_sort(arr):\n    ",
            "code_suffix": ""
        },
        "contexts": []
    },
    top_p=0.7,
    temperature=0.9,
    max_tokens=1024,
    stop=["<|endoftext|>", "<|user|>", "<|assistant|>", "<|observation|>"]
)
print(response.choices[0].message)
```

#### 响应示例

```json
{
    "created": 1720008093,
    "id": "8807577524653187075",
    "model": "codegeex-4",
    "request_id": "8807577524653187075",
    "choices": [
        {
            "finish_reason": "stop",
            "index": 0,
            "message": {
                "content": "    # If the list is empty or contains only one element, it is already sorted\n    if len(arr) <= 1:\n        return arr\n",
                "role": "assistant"
            }
        }
    ],
    "usage": {
        "completion_tokens": 32,
        "prompt_tokens": 28,
        "total_tokens": 60
    }
}
```

### 流式调用

#### 请求示例

```python
from zhipuai import ZhipuAI
client = ZhipuAI(api_key="") # 请填写您自己的APIKey
response = client.chat.completions.create(
    model="codegeex-4", # 填写需要调用的模型名称
    messages=[],
    extra={
        "target": {
            "path": "quick_srot.py",
            "language": "Python",
            "code_prefix": "def quick_sort(arr):\n    ",
            "code_suffix": ""
        },
        "contexts": []
    },
    top_p=0.7,
    temperature=0.9,
    stream=True,
    max_tokens=1024,
    stop=["<|endoftext|>", "<|user|>", "<|assistant|>", "<|observation|>"]
)
for chunk in response:
    print(chunk.choices[0].delta)
```

#### 响应示例



```
{"id":"8807577524653187084","created":1720008235,"model":"codegeex-4","choices":[{"index":0,"delta":{"role":"assistant","content":"  "}}]}
{"id":"8807577524653187084","created":1720008235,"model":"codegeex-4","choices":[{"index":0,"delta":{"role":"assistant","content":"#"}}]}
{"id":"8807577524653187084","created":1720008235,"model":"codegeex-4","choices":[{"index":0,"delta":{"role":"assistant","content":"quick"}}]}
{"id":"8807577524653187084","created":1720008235,"model":"codegeex-4","choices":[{"index":0,"delta":{"role":"assistant","content":"sort"}}]}
... ...

{"id":"8807577524653187084","created":1720008235,"model":"codegeex-4","choices":[{"index":0,"delta":{"role":"assistant","content":"list"}}]}
{"id":"8807577524653187084","created":1720008235,"model":"codegeex-4","choices":[{"index":0,"delta":{"role":"assistant","content":"\n   "}}]}
{"id":"8807577524653187084","created":1720008235,"model":"codegeex-4","choices":[{"index":0,"delta":{"role":"assistant","content":"pass"}}]}
{"id":"8807577524653187084","created":1720008235,"model":"codegeex-4","choices":[{"index":0,"finish_reason":"stop","delta":{"role":"assistant","content":""}}],"usage":{"prompt_tokens":28,"completion_tokens":13,"total_tokens":41}}
```

## 异步调用

### 接口请求

| 传输方式 | `https` |
| --- | --- |
| 请求地址 | `https://open.bigmodel.cn/api/paas/v4/async/chat/completions` |
| 调用方式 | 异步调用，需通过查询接口获取结果 |
| 字符编码 | UTF-8 |
| 接口请求格式 | JSON |
| 响应格式 | JSON |
| 接口请求类型 | `GET` |
| 开发语言 | 任意可发起 HTTP 请求的开发语言 |

### 接口请求参数

与同步调用的接口请求参数相同。

### 接口响应参数

| 参数名称 | 类型 | 参数说明 |
| --- | --- | --- |
| `request_id` | `String` | 用户在客户端请求时提交的任务编号或者平台生成的任务编号 |
| `id` | `String` | 智谱 AI 开放平台生成的任务订单号，调用请求结果接口时请使用此订单号 |
| `model` | `String` | 本次调用的模型名称 |
| `task_status` | `String` | 处理状态，`PROCESSING`（处理中），`SUCCESS`（成功），`FAIL`（失败）。需通过查询获取结果 |

### Python 调用示例

#### 请求示例

```
from zhipuai import ZhipuAI
client = ZhipuAI(api_key="") # 请填写您自己的APIKey
response = client.chat.asyncCompletions.create(
    model="codegeex-4", # 填写需要调用的模型名称
    messages=[],
    extra={
        "target": {
            "path": "quick_srot.py",
            "language": "Python",
            "code_prefix": "def quick_sort(arr):\n    ",
            "code_suffix": ""
        },
        "contexts": []
    },
    top_p=0.7,
    temperature=0.9,
    max_tokens=1024,
    stop=["<|endoftext|>", "<|user|>", "<|assistant|>", "<|observation|>"]
)
print(response)
```

#### 响应示例

```
id='850817101390499798804608774531416095' request_id='8804608774531416094' model='codegeex-4' task_status='PROCESSING'
```

### 任务结果查询

#### 接口请求

| 传输方式 | `https` |
| --- | --- |
| 请求地址 | `https://open.bigmodel.cn/api/paas/v4/async-result/{id}` |
| 调用方式 | 同步调用，等待模型执行完成并返回最终结果 |
| 字符编码 | UTF-8 |
| 接口请求格式 | JSON |
| 响应格式 | JSON |
| 接口请求类型 | `GET` |
| 开发语言 | 任意可发起 HTTP 请求的开发语言 |

#### 请求参数

| 参数名称 | 类型 | 是否必填 | 参数说明 |
| --- | --- | --- | --- |
| `id` | `String` | 是 | 任务 ID |

#### 接口响应参数

| 参数名称 | 类型 | 参数说明 |
| --- | --- | --- |
| `model` | `String` | 模型名称 |
| `choices` | `List` | 当前对话的模型输出内容，目前只返回一条 |
| `index` | `Integer` | 结果下标 |
| `finish_reason` | `String` | 模型推理终止的原因。 |
| `message` | `Object` | 模型增量返回的文本信息 |
| `role` | `String` | 当前对话的角色，目前默认为 `assistant`（模型） |
| `content` | `String` | 当前对话的内容。命中函数时此字段为 `null`，未命中函数时返回模型推理结果。 |
| `task_status` | `String` | 处理状态，`PROCESSING`（处理中），`SUCCESS`（成功），`FAIL`（失败） 注：处理中状态需通过查询获取结果 |
| `request_id` | `String` | 用户在客户端请求时提交的任务编号或者平台生成的任务编号 |
| `id` | `String` | 智谱 AI 开放平台生成的任务订单号，调用请求结果接口时请使用此订单号 |
| `usage` | `Object` | 结束时返回本次模型调用的 tokens 数量统计 |
| `prompt_tokens` | `Integer` | 用户输入的 tokens 数量 |
| `completion_tokens` | `Integer` | 模型输出的 tokens 数量 |
| `total_tokens` | `Integer` | 总 tokens 数量 |
| `content_filter` | `List` | 返回内容安全的相关信息。 |
| `role` | `String` | 安全生效环节，包括 `role = assistant` 模型推理，`role = user` 用户输入，`role = history` 历史上下文 |
| `level` | `Integer` | 严重程度 `level` 0-3，`level` 0 表示最严重，3 表示轻微 |

#### Python 调用示例

##### 请求示例

```
import time
from zhipuai import ZhipuAI
client = ZhipuAI(api_key="") # 请填写您自己的APIKey
response = client.chat.asyncCompletions.create(
    model="codegeex-4", # 填写需要调用的模型名称
    messages=[],
    extra={
        "target": {
            "path": "quick_srot.py",
            "language": "Python",
            "code_prefix": "def quick_sort(arr):\n    ",
            "code_suffix": ""
        },
        "contexts": []
    },
    top_p=0.7,
    temperature=0.9,
    max_tokens=1024,
    stop=["<|endoftext|>", "<|user|>", "<|assistant|>", "<|observation|>"]
)
task_id = response.id
task_status = ''
get_cnt = 0
while task_status != 'SUCCESS' and task_status != 'FAILED' and get_cnt <= 40:
    result_response = client.chat.asyncCompletions.retrieve_completion_result(id=task_id)
    print(result_response)
    task_status = result_response.task_status

    time.sleep(2)
    get_cnt += 1
```

##### 响应示例

```
{"id":"850817101390499798807577524653187104","request_id":"8807577524653187103","model":null,"task_status":"PROCESSING"}
{"id":"850817101390499798807577524653187104","request_id":"8807577524653187103","model":null,"task_status":"PROCESSING"}
... ...
{"id":"850817101390499798807577524653187104","request_id":"8807577524653187103","model":null,"task_status":"PROCESSING"}
{
    "created": 1720008757,
    "id": "850817101390499798807577524653187104",
    "model": "codegeex-4",
    "request_id": "8807577524653187103",
    "task_status": "SUCCESS",
    "choices": [
        {
            "finish_reason": "stop",
            "index": 0,
            "message": {
                "content": "   if len(arr) <= 1:\n        return arr\n    pivot = arr[len(arr) // 2]\n    left = [x for x in arr if x < pivot]\n    middle = [x for x in arr if x == pivot]\n    right = [x for x in arr if x > pivot]\n    return quick_sort(left) + middle + quick_sort(right)",
                "role": "assistant"
            }
        }
    ]
    "usage": {
        "completion_tokens": 80,
        "prompt_tokens": 28,
        "total_tokens": 108
    }
}
```
