# Embedding-3 向量模型

> 来源：[https://ai.feishu.cn/docx/FijFdgJ3lozdZRx03tpcMOL4nDc](https://ai.feishu.cn/docx/FijFdgJ3lozdZRx03tpcMOL4nDc)

> Embedding-3 是文本向量模型，将输入的文本信息进行向量化表示，以便于结合向量数据库为大模型提供外部知识库，模型支持自定义向量维度，建议选择256、512、1024或2048维度。相比 Embedding-2，除了基本的文本向量化功能，Embedding-3 还支持自定义向量维度（如 256、512、1024 或 2048），提供更灵活的外部知识库。更高的维度会提供更高的准确性以及不同场景下更具适应性，适合需要高精度和丰富语义表示的场景。

>

> **模型编码：**embedding-3

## Embedding-3 Cookbook 调用教程

## 创建向量模型同步请求

### 接口请求

| **传输方式** | https |
| --- | --- |
| **请求地址** | https://open.bigmodel.cn/api/paas/v4/embeddings |
| **调用方式** | 同步调用，等待模型执行完成并返回最终结果 |
| **字符编码** | UTF-8 |
| **接口请求格式** | JSON |
| **响应格式** | JSON |
| **接口请求类型** | POST |
| **开发语言** | 任意可发起 HTTP 请求的开发语言 |

### 接口请求参数

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| model | String | 是 | 模型编码 |
| input | String or Array | 是 | 输入需要向量化的文本，支持字符串数组。 |
| dimensions | Integer | 否 | 向量维度默认为 2048。模型支持自定义向量维度，建议选择256、512、1024或2048维度。 |

### 接口响应参数

| **参数名称** | **类型** | **参数说明** |
| --- | --- | --- |
| model | String | 模型编码 |
| object | Long | 结果类型，目前为 list |
| data | List&lt;Object&gt; | 模型生成的数组结果 |
| index | Object | 结果下标 |
| object | String | 结果类型，目前为 embedding |
| embedding | ListObject | embedding 的处理结果，返回向量化表征的数组 |
| usage | Object | 本次模型调用的 tokens 数量统计 |
| prompt_tokens | Integer | 用户输入的 tokens 数量 |
| completion_tokens | Integer | 模型输出的 tokens 数量 |
| total_tokens | Integer | 总 tokens 数量 |

## Python 调用示例

### 请求示例

```
from zhipuai import ZhipuAI

client = ZhipuAI(api_key=&quot;your api key&quot;) 
response = client.embeddings.create(
    model=&quot;embedding-3&quot;, #填写需要调用的模型编码
     input=[
        &quot;美食非常美味，服务员也很友好。&quot;,
        &quot;这部电影既刺激又令人兴奋。&quot;,
        &quot;阅读书籍是扩展知识的好方法。&quot;
    ],
)
print(response)
```

### 响应示例

```
{
    &quot;model&quot;: &quot;embedding-3&quot;,
    &quot;data&quot;: [
        {
            &quot;embedding&quot;: [
                -0.02675454691052437,
                0.019060475751757622,
                ...... 
                -0.005519774276763201,
                0.014949671924114227
            ],
            &quot;index&quot;: 0,
            &quot;object&quot;: &quot;embedding&quot;
        },
        ...
        {
            &quot;embedding&quot;: [
                -0.02675454691052437,
                0.019060475751757622,
                ...... 
                -0.005519774276763201,
                0.014949671924114227
            ],
            &quot;index&quot;: 2,
            &quot;object&quot;: &quot;embedding&quot;
        }
    ],
    &quot;object&quot;: &quot;list&quot;,
    &quot;usage&quot;: {
        &quot;completion_tokens&quot;: 0,
        &quot;prompt_tokens&quot;: 100,
        &quot;total_tokens&quot;: 100
    }
}
```