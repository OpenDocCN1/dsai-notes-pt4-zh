# 文件管理

> 来源：[https://v11enp9ok1h.feishu.cn/docx/RmdHdVGfRoWkLwxj4Aecf2CYneg](https://v11enp9ok1h.feishu.cn/docx/RmdHdVGfRoWkLwxj4Aecf2CYneg)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

文件管理用于上传在知识库、模型微调等功能中所需的文件。

## 上传文件

上传用于模型微调、知识库、评测中等功能所使用的文件。当前单个文件的大小最大可为 100 MB 。请注意，Fine-tuning API 仅支持.jsonl文件。

### 接口介绍

| 传输方式 | https |
| --- | --- |
| 请求地址 | [https://open.bigmodel.](https://open.bigmodel.cn/api/paas/v4/files)cn/api/paas/v4/files |
| 调用方式 | 同步调用 |
| 字符编码 | UTF-8 |
| 接口请求格式 | JSON |
| 响应格式 | JSON 或标准 Stream Event |
| 接口请求类型 | POST |
| 开发语言 | 任意可发起 http 请求的开发语言 |

### 接口请求参数

| **参数名称** | **类型** | **是否必填** | **参数位置** | **参数说明** |
| --- | --- | --- | --- | --- |
| file | File | 是 | FromData | 要上传的文件路径 |
| purpose | String | 是 | FromData | 您所上传文件用途，当前仅支持文件用途为fine-tune的文件上传，知识库及评测中心等功能[的文件上](https://open.bigmodel.cn/dev/api#fine-tuning)传将在未来进行支持。  文件用途为fine-tune时，仅支持上传.jsonl文件，且文件中语料格式需满足微调指南中所描述的格式。有关更多详细信息，请参阅微调指南 |

### 接口响应参数

| **参数名称** | **类型** | **参数说明** |
| --- | --- | --- |
| id | String | 文件id |
| filename | String | 文件名称 |
| purpose | String | 该文件的用途 |

### 调用示例

使用 Curl

```
curl &#x27;https://open.bigmodel.cn/api/paas/v4/files&#x27; \
-X POST \
--header &#x27;Authorization: Bearer &lt;api-key&gt;&#x27; \
--form &#x27;file=@&quot;&lt;path-to-file.jsonl&gt;&quot;&#x27; \
--form &#x27;purpose=&quot;fine-tune&quot;&#x27;
```

使用Python SDK

```
from zhipuai import ZhipuAI
 
client = ZhipuAI(api_key=&quot;&quot;) 
result = client.files.create(
    file=open(&quot;&lt;path-to-file.jsonl&gt;&quot;, &quot;rb&quot;),
    purpose=&quot;fine-tune&quot;
)
print(result.id)
```

## 查询文件列表

返回属于用户的文件列表，按照上传时间倒序排列

### 接口介绍

| 传输方式 | https |
| --- | --- |
| 请求地址 | [https://open.bigmodel.](https://open.bigmodel.cn/api/paas/v4/files)cn/api/paas/v4/files |
| 调用方式 | 同步调用 |
| 字符编码 | UTF-8 |
| 接口请求格式 | JSON |
| 响应格式 | JSON 或标准 Stream Event |
| 接口请求类型 | POST |
| 开发语言 | 任意可发起 http 请求的开发语言 |

### 接口请求参数

| **参数名称** | **类型** | **是否必填** | **参数位置** | **参数说明** |
| --- | --- | --- | --- | --- |
| purpose | String | 否 | Query | 上传文件用途 |
| limit | Integer | 否 | Query | 查询文件列表数，默认10 |
| after | String | 否 | Query | 查询指定fileID之后的文件列表 |
| order | String | 否 | Query | 排序规则，可选值[desc，asc]，默认desc |

### 接口响应参数

| **参数名称** | **类型** | **参数说明** |
| --- | --- | --- |
| object | String | 任务对象 |
| data | List |  |
| id | String | 文件ID |
| bytes | Long | 文件大小 |
| filename | String | 文件名称 |
| purpose | String | 上传文件用途 |
| created_at | Long | 创建时间戳，10位Unix时间戳 |
| object | String | 对象类型 |

### 调用示例

使用 Curl

```
curl https://open.bigmodel.cn/api/paas/v4/files \
  -H &#x27;Authorization: Bearer &lt;token&gt;&#x27;
```

使用Python SDK

```
from zhipuai import ZhipuAI
 
client = ZhipuAI(api_key=&quot;&quot;) 
 
client.files.list()
```