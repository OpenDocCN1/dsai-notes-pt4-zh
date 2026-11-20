
# CogVideoX 视频模型

来源：https://v11enp9ok1h.feishu.cn/docx/HuCqdJvHooGFmVxRYhWcYFvQnHg

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

## CogVideoX模型介绍

`CogVideoX`是智谱开放平台中最新上线的视频模型，现已支持文生视频、图生视频多个能力，让用户可以在开放平台使用和调用视频模型能力，轻松高效地完成艺术视频创作。体验中心支持多种生成方式，包括文本生成视频、图片生成视频，可应用于广告制作、电影剪辑、短视频制作等领域。

模型编码：`cogvideox`

`CogVideoX`体验中心地址：`https://zhipuaishengchan.datasink.sensorsdata.cn/t/Eo`

API调用接口地址：`https://bigmodel.cn/dev/api#cogvideox`

### 技术特点

针对内容连贯性问题，智谱AI自主研发了一套高效的三维变分自编码器结构（`3D VAE`）。该结构能够将原始视频数据压缩至原始大小的2%，显著降低了视频扩散生成模型的训练成本和难度。结合`3D RoPE`位置编码模块，该技术有效提升了在时间维度上对帧间关系的捕捉能力，从而建立了视频中的长期依赖关系。

在可控性方面，智谱AI打造了一款端到端的视频理解模型，该模型能够为大量视频数据生成精确且内容相关的描述。这一创新增强了模型对文本的理解和对指令的遵循能力，确保生成的视频更加符合用户的输入需求，并能够处理超长且复杂的`prompt`指令。

我们的模型采纳了一种将文本、时间、空间三维一体融合的`transformer`架构。该架构摒弃了传统的`cross attention`模块，创新性地设计了`Expert Block`以实现文本与视频两种不同模态空间的对齐，并通过`Full Attention`机制优化模态间的交互效果。

## CogVideoX Cookbook调用教程

## 创建视频生成任务

### 接口请求

| 传输方式 | `https` |

| --- | --- |

| 请求地址 | `https://open.bigmodel.cn/api/paas/v4/videos/generations` |

| 调用方式 | 异步调用，需通过查询接口获取结果 |

| 字符编码 | `UTF-8` |

| 接口请求格式 | `JSON` |

| 响应格式 | `JSON` |

| 接口请求类型 | `POST` |

| 开发语言 | 任意可发起 `http` 请求的开发语言 |

#### 接口请求参数

| 参数名称 | 类型 | 是否必填 | 参数说明 |

| --- | --- | --- | --- |

| `model` | `String` | 是 | 模型编码 |

| `prompt` | `String` | 是 | 视频的文本描述，最大支持 500 Tokens 输入 |

| `image_url` | `String` | 否 | 不上传图片进行文生视频，上传图片则进行图生视频，支持通过 `URL` 或 `Base64` 编码传入图片。图片要求如下： |

| `request_id` | `String` | 否 | 由用户端传参，需保证唯一性；用于区分每次请求的唯一标识，用户端不传时平台会默认生成。 |

| `user_id` | `String` | 否 | 终端用户的唯一 ID，协助平台对终端用户的违规行为、生成违法及不良信息或其他滥用行为进行干预。ID 长度要求：最少 6 个字符，最多 128 个字符。了解更多 |

#### 接口响应参数

| 参数名称 | 类型 | 参数说明 |

| --- | --- | --- |

| `request_id` | `String` | 用户在客户端请求时提交的任务编号或者平台生成的任务编号 |

| `id` | `String` | 智谱 AI 开放平台生成的任务订单号，调用请求结果接口时请使用此订单号 |

| `model` | `String` | 本次调用的模型名称 |

| `task_status` | `String` | 处理状态，`PROCESSING`（处理中），`SUCCESS`（成功），`FAIL`（失败）。需通过查询获取结果 |

### Python调用示例

#### 请求示例

```
from zhipuai import ZhipuAI
client = ZhipuAI()  # 填写您自己的APIKey

response = client.videos.generations(
    model="cogvideox",
    prompt="比得兔开小汽车，游走在马路上，脸上的表情充满开心喜悦。"
)
print(response)
```

#### 响应示例

```
id='8868902201637896192' request_id='654321' model='cogvideox' task_status='PROCESSING'
```

## 视频生成任务结果查询

### 接口请求

| 传输方式 | `https` |

| --- | --- |

| 请求地址 | `https://open.bigmodel.cn/api/paas/v4/async-result/{id}` |

| 调用方式 | 同步调用，等待模型执行完成并返回最终结果 |

| 字符编码 | `UTF-8` |

| 接口请求格式 | `JSON` |

| 响应格式 | `JSON` |

| 接口请求类型 | `GET` |

| 开发语言 | 任意可发起 `http` 请求的开发语言 |

#### 请求参数

| 参数名称 | 类型 | 是否必填 | 参数说明 |

| --- | --- | --- | --- |

| `id` | `String` | 是 | 任务 id |

#### 响应参数

| 参数名称 | 类型 | 参数说明 |

| --- | --- | --- |

| `model` | `String` | 模型名称 |

| `video_result` | `List` | 视频生成结果 |

| `url` | `String` | 视频url |

| `cover_image_url` | `String` | 视频封面url |

| `task_status` | `String` | 处理状态，`PROCESSING`（处理中），`SUCCESS`（成功），`FAIL`（失败）。注：处理中状态需通过查询获取结果 |

| `request_id` | `String` | 用户在客户端请求时提交的任务编号或者平台生成的任务编号 |

| `id` | `String` | 智谱 AI 开放平台生成的任务订单号，调用请求结果接口时请使用此订单号 |

### Python调用示例

#### 请求示例

```
from zhipuai import ZhipuAI
client = ZhipuAI()  # 填写您自己的APIKey

response = client.videos.retrieve_videos_result(
    id="8868902201637896192"
)
print(response)
```

#### 响应示例

```
{
    "model": "cogvideox",
    "request_id": "8868902201637896192",
    "task_status": "SUCCESS",
    "cover_image_url": "https://sfile.chatglm.cn/testpath/video_cover/4d3c5aad-8c94-5549-93b7-97af6bd353c6_cover_0.png",
    "video_result": [
        {
            "url": "https://sfile.chatglm.cn/testpath/video/4d3c5aad-8c94-5549-93b7-97af6bd353c6_0.mp4"
        }
    ]
}
```

## 社群开发者调用反馈

深度测评国产视频生成模型`CogVideoX`，开发者的体验到底如何？

突发！`Maas`开放平台推出全新视频生成大模型，首调用，一手评测来了！

`Mass`开放平台发布视频生成模型`CogVideoX`，开放 API 调用
