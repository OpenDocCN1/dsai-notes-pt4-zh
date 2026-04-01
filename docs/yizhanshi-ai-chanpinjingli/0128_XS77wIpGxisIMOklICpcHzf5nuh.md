# 模型微调

> 来源：[https://v11enp9ok1h.feishu.cn/docx/KA7ldoaA3oHPuwxVuVJcoSOBn7c](https://v11enp9ok1h.feishu.cn/docx/KA7ldoaA3oHPuwxVuVJcoSOBn7c)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

管理您的微调任务以根据您的特定训练数据定制专属模型。

相关指南: [模型微调](https://open.bigmodel.cn/dev/howuse/finetuning)

## 概要

微调可以显著提升您的模型推理效果。通过训练使模型学习大量的样本数据，从而取得比prompt更好的效果。一方面，您不需要在prompt中提供大量的few-shots示例，能够有效节省成本；另一方面，由于训练的样本数量远多于prompt所能提供的样本数量，因此能取得比prompt调优更好的推理结果。概括来说，微调过程包括以下步骤：

+   准备并上传训练数据

+   训练一个新的微调模型

+   部署模型进行推理

+   评估结果，如有需要返回到步骤1

API 列表

## 微调Cookbook教程

## 接口介绍

| 传输方式 | https |
| --- | --- |
| 请求地址 | https://open.bigmodel.cn/api/paas/v4/fine_tuning/jobs |
| 调用方式 | 同步调用 |
| 字符编码 | UTF-8 |
| 接口请求格式 | JSON |
| 响应格式 | JSON |
| 接口请求类型 | POST |
| 开发语言 | 任意可发起 http 请求的开发语言 |

## 创建微调任务

### 接口请求参数

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| model | String | 是 | 要微调的模型的model-id，目前仅支持 chatglm3-6b 模型 |
| training_file | String | 是 | 包含训练数据的上传文件的 ID。您的数据集必须格式化为 JSONL 文件。此外，您必须传参文件用途为fine-tune的文件ID |
| validation_file | String | 否 | 包含验证数据的上传文件的 ID。  如果您提供此文件，则数据将用于在微调期间定期生成验证指标。这些指标可以在微调结果文件中查看。相同的数据不应同时出现在训练文件和验证文件中。  您的数据集必须格式化为 JSONL 文件。您必须传参文件用途为fine-tune的文件ID |
| hyperparameters | Object | 否 | 微调超参 |
| learning_rate_multiplier | Double or String | 否 | 学习率倍数。较小的学习率可能有助于避免过度拟合。  默认&quot;auto&quot; 为 1.0，数值范围(0, 5] |
| batch_size | Integer or String | 否 | 每批中的训练样本数。批量大小越大意味着模型参数的更新频率越低，但方差越小。  默认&quot;auto&quot; 为8，数值范围[4, 32] |
| n_epochs | Integer or String | 否 | 训练模型的 epochs 数。一个 epoch 指的是训练数据集的一个完整周期。  默认&quot;auto&quot; 为3，数值范围[1, 10] |
| suffix | String | 否 | 用于对模型做简明标识，最长18个字符，该信息将添加到您的微调模型名称中。默认为空  举例：微调后的模型名称结构为: ft:{base-model}:{suffix}:{random-id}。suffix为test时模型名称为ft:chatglm3-6b:test:5bhn64mh |
| request_id | String | 否 | 客户请求ID |

### 接口响应参数

| **参数名称** | **类型** | **参数说明** |
| --- | --- | --- |
| id | String | 任务id |
| request_id | String | 客户请求id |
| fine_tuned_model | String | 微调模型id，后续调用时候使用 |
| status | String | 模型微调任务的处理状态，包括以下状态值：  · create（已提交）  · validating_files（文件验证中）  · queued（排队中）  · running（训练中）  · succeeded（训练完成）  · failed（任务失败）  · cancelled（已取消） |
| object | String | fine_tuning.job |
| training_file | String | 训练数据对应的file id |
| validation_file | String | 验证数据对应的file id |

### 调用示例

使用curl 调用

```
curl &#x27;https://open.bigmodel.cn/api/paas/v4/fine_tuning/jobs&#x27; \
-X POST \
--header &#x27;Authorization: Bearer &lt;token&gt;&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data &#x27;{
    &quot;model&quot;: &quot;chatglm3-6b&quot;,
    &quot;training_file&quot;: &quot;file-xxx&quot;,
    &quot;validation_file&quot;: &quot;file-yyy&quot;,
    &quot;suffix&quot;: &quot;&lt;self-defined&gt;&quot;
}&#x27;
# example response
{
  &quot;fine_tuned_model&quot;: &quot;ft:chatglm3-6b:test:5bhn64mh&quot;,
  &quot;id&quot;: &quot;ftjob-20240115170023472-jss2f&quot;,
  &quot;model&quot;: &quot;chatglm3-6b&quot;,
  &quot;object&quot;: &quot;fine_tuning.job&quot;,
  &quot;status&quot;: &quot;created&quot;,
  &quot;training_file&quot;: &quot;file-20240114063454130-bnm25&quot;,
  &quot;validation_file&quot;: &quot;&quot;
}
```

使用 python sdk 调用

```
from zhipuai import ZhipuAI
 
client = ZhipuAI(api_key=&quot;&quot;) 
 
job = client.fine_tuning.jobs.create(
    model=&quot;chatglm3-6b&quot;,
    training_file=&quot;file-xxx&quot;,   
    validation_file=&quot;file-yyy&quot;,
    suffix=&quot;&lt;self-defined&gt;&quot;,
)
job_id = job.id
 
print(job_id)
```

## 查询微调任务事件

| 传输方式 | https |
| --- | --- |
| 请求地址 | https://open.bigmodel.cn/api/paas/v4/fine_tuning/jobs/&lt;job_id&gt;/events?limit=50 |
| 调用方式 | 同步调用 |
| 字符编码 | UTF-8 |
| 请求格式 | JSON |
| 响应格式 | JSON |
| 接口请求类型 | GET |
| 开发语言 | 任意可发起 HTTP 请求的开发语言 |

### 接口请求参数

| **参数名称** | **类型** | **是否必填** | **参数说明** |  |
| --- | --- | --- | --- | --- |
| job_id | String | 是 | Path | 路径参数，要查询事件的微调任务对应的任务id |
| limit | Integer | 否 | Query | limit |
| after | String | 否 | Query | after event id |
| hyperparameters | Object | 否 | 微调超参 |  |
| learning_rate_multiplier | Double or String | 否 | 学习率倍数。较小的学习率可能有助于避免过度拟合。  默认&quot;auto&quot; 为 1.0，数值范围(0, 5] |  |
| batch_size | Integer or String | 否 | 每批中的训练样本数。批量大小越大意味着模型参数的更新频率越低，但方差越小。  默认&quot;auto&quot; 为8，数值范围[4, 32] |  |
| n_epochs | Integer or String | 否 | 训练模型的 epochs 数。一个 epoch 指的是训练数据集的一个完整周期。  默认&quot;auto&quot; 为3，数值范围[1, 10] |  |
| suffix | String | 否 | 用于对模型做简明标识，最长18个字符，该信息将添加到您的微调模型名称中。默认为空  举例：微调后的模型名称结构为: ft:{base-model}:{suffix}:{random-id}。suffix为test时模型名称为ft:chatglm3-6b:test:5bhn64mh |  |
| request_id | String | 否 | 客户请求ID |  |

### 接口响应参数

| **参数名称** | **类型** | **参数说明** |
| --- | --- | --- |
| object | String | list |
| has_more | Boolean | 是否还有更多 |
| data | List | 任务id |
| object | String | fine_tuning.job.event |
| id | String | 事件id |
| type | String | metrics或message |
| created_at | Integer | 创建时间timestamp |

message类型说明

| **参数名称** | **类型** |
| --- | --- |
| level | 日志级别 info |
| message | 当前事件日志内容 |

metric类型中的data说明

| **参数名称** | **类型** |
| --- | --- |
| data |  |
| epoch | 当前轮数 |
| current_steps | 当前步数 |
| total_steps | 训练总步数 |
| elapsed_time | 已花费时间 (seconds) |
| remaining_time | 预计剩余时间 (seconds) |
| trained_tokens | 已花费token数 |
| loss | 训练集loss |
| eval_loss | 评估集loss |
| acc | 训练集准确率 |
| eval_acc | 评估价准确率 |
| learning_rate | 当前学习率 |

### 调用示例

Curl 调用

```
curl &#x27;https://open.bigmodel.cn/api/paas/v4/fine_tuning/jobs/&lt;job_id&gt;/events?limit=50&#x27; --header &#x27;Authorization: Bearer &lt;token&gt;&#x27;
 
# example response
{
  &quot;data&quot;: [
    {
      &quot;level&quot;: &quot;info&quot;,
      &quot;created_at&quot;: 1705220854,
      &quot;id&quot;: &quot;ftevent-20240114162734889-6btwz&quot;,
      &quot;type&quot;: &quot;message&quot;,
      &quot;message&quot;: &quot;Files validated, moving job to queued state&quot;,
      &quot;object&quot;: &quot;fine_tuning.job.event&quot;
    },
    {
      &quot;data&quot;: {
        &quot;acc&quot;: 0.11607143,
        &quot;loss&quot;: 0.91015625,
        &quot;current_steps&quot;: 1,
        &quot;remaining_time&quot;: &quot;0:00:04&quot;,
        &quot;elapsed_time&quot;: &quot;0:00:02&quot;,
        &quot;total_steps&quot;: 3,
        &quot;epoch&quot;: 1,
        &quot;trained_tokens&quot;: 102,
        &quot;learning_rate&quot;: 0.0001
      },
      &quot;created_at&quot;: 1705220886,
      &quot;id&quot;: &quot;ftevent-20240114162806148-ghg9k&quot;,
      &quot;type&quot;: &quot;metrics&quot;,
      &quot;object&quot;: &quot;fine_tuning.job.event&quot;
    }
  ],
  &quot;has_more&quot;: false,
  &quot;object&quot;: &quot;list&quot;
}
```

python sdk

```
from zhipuai import ZhipuAI
 
client = ZhipuAI(api_key=&quot;&quot;) 
 
job = client.fine_tuning.jobs.create(
    model=&quot;chatglm3-6b&quot;,
    training_file=&quot;file-xxx&quot;,   
    validation_file=&quot;file-yyy&quot;, 
    suffix=&quot;&lt;self-defined&gt;&quot;,
)
job_id = job.id
 
print(job_id)
```

## 查询微调任务

### 接口请求

| 传输方式 | https |
| --- | --- |
| 请求地址 | https://open.bigmodel.cn/api/paas/v4/fine_tuning/jobs/&lt;job_id&gt; |
| 调用方式 | 同步调用 |
| 字符编码 | UTF-8 |
| 请求格式 | JSON |
| 响应格式 | JSON |
| 接口请求类型 | GET |
| 开发语言 | 任意可发起 HTTP 请求的开发语言 |

### 接口请求参数

| **参数名称** | **类型** | **是否必填** | **参数说明** |  |
| --- | --- | --- | --- | --- |
| job_id | String | 是 | Path | 路径参数，要查询事件的微调任务对应的任务id |
| limit | Integer | 否 | Query | 查询条数 |
| after | String | 否 | Query | After event id |

### 接口响应参数

| **参数名称** | **类型** | **参数说明** |
| --- | --- | --- |
| request_id | String | 客户请求id |
| fine_tuned_model | String | 微调模型id，后续调用时候使用 |
| status | String | create,validating_files, queued, running, succeeded, failed, cancelled |
| object | String | fine_tuning.job |
| training_file | String | 训练数据对应的file id |
| validation_file | String | 校验数据对应的file id |
| error | Object | 当failed时会有错误信息 |
| code | String | 参考错误码文档 |
| message | String | 参考错误码文档 |

### 调用示例

使用curl请求，需要提供 job_id 进行查询

```
curl &#x27;https://open.bigmodel.cn/api/paas/v4/fine_tuning/jobs/&lt;job_id&gt;&#x27; --header &#x27;Authorization: Bearer &lt;token&gt;&#x27;
 
# example response
{
  &quot;fine_tuned_model&quot;: &quot;ft:chatglm3-6b:cpc:gb6k6bmm&quot;,
  &quot;id&quot;: &quot;ftjob-20240114162722549-6hlwm&quot;,
  &quot;model&quot;: &quot;chatglm3-6b&quot;,
  &quot;object&quot;: &quot;fine_tuning.job&quot;,
  &quot;result_files&quot;: [
    &quot;file-20240114082807774-sdtfc&quot;
  ],
  &quot;status&quot;: &quot;succeeded&quot;,
  &quot;training_file&quot;: &quot;file-20240112074237286-n8kcn&quot;,
  &quot;validation_file&quot;: &quot;&quot;
}
```

python sdk

```
from zhipuai import ZhipuAI
 
client = ZhipuAI(api_key=&quot;&quot;) 
 
fine_tuning_job = client.fine_tuning.jobs.retrieve(
    fine_tuning_job_id = job_id, 
)
print(fine_tuning_job)
```

## 查询个人微调任务

### 接口请求

| 传输方式 | https |
| --- | --- |
| 请求地址 | https://open.bigmodel.cn/api/paas/v4/fine_tuning/jobs |
| 调用方式 | 同步调用 |
| 字符编码 | UTF-8 |
| 请求格式 | JSON |
| 响应格式 | JSON |
| 接口请求类型 | GET |
| 开发语言 | 任意可发起 HTTP 请求的开发语言 |

### 接口请求参数

| **参数名称** | **类型** | **是否必填** | **参数说明** |  |
| --- | --- | --- | --- | --- |
| limit | Integer | 否 | Query | 查询条数 |
| after | String | 否 | Query | After job id |

### 接口响应参数

| **参数名称** | **类型** | **参数说明** |
| --- | --- | --- |
| object | String | list |
| data | Object | 参考创建任务返回对象 |
| fine_tuned_model | String | 微调模型id，后续调用时候使用 |
| status | String | create,validating_files, queued, running, succeeded, failed, cancelled |
| object | String | fine_tuning.job |
| training_file | String | 训练数据对应的file id |
| validation_file | String | 校验数据对应的file id |
| error | Object | 当failed时会有错误信息 |
| code | String | 参考错误码文档 |
| message | String | 参考错误码文档 |

### 调用示例

使用curl请求

```
curl &#x27;https://open.bigmodel.cn/api/paas/v4/fine_tuning/jobs&#x27; --header &#x27;Authorization: Bearer &lt;token&gt;&#x27;

# example response
{
  &quot;data&quot;: [
    {
      &quot;training_file&quot;: &quot;file-20240112074237286-n8kcn&quot;,
      &quot;result_files&quot;: [
        &quot;file-20240114082807774-sdtfc&quot;
      ],
      &quot;fine_tuned_model&quot;: &quot;ft:chatglm3-6b:cpc:gb6k6bmm&quot;,
      &quot;validation_file&quot;: &quot;&quot;,
      &quot;model&quot;: &quot;chatglm3-6b&quot;,
      &quot;id&quot;: &quot;ftjob-20240114162722549-6hlwm&quot;,
      &quot;object&quot;: &quot;fine_tuning.job&quot;,
      &quot;status&quot;: &quot;succeeded&quot;
    }
  ],
  &quot;object&quot;: &quot;list&quot;
}
```

Python sdk

```
from zhipuai import ZhipuAI
client = ZhipuAI(api_key=&quot;&quot;)
client.fine_tuning.jobs.list()
```