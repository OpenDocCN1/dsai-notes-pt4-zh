
# GLM-4V 图像理解

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EgVIddpIHoq1iNx5cWpcwnN8n1c](https://v11enp9ok1h.feishu.cn/docx/EgVIddpIHoq1iNx5cWpcwnN8n1c)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

模型编码：`glm-4v`

根据输入的自然语言指令和图像信息完成任务，推荐使用 SSE 或同步调用方式请求接口

模型编码：`glm-4v-plus`

提升版本 `GLM-4V-Plus` 具备卓越的图像理解能力，并具备基于时间感知的视频理解能力。

## 非流调用

### 请求示例

```
"url": "https://ts1.cn.mm.bing.net/th/id/R-C.3997eb10814025183c8fbbc6b5373713?rik=fSPsSA1rrKXAzw&riu=http%3a%2f%2fdpic.tiankong.com%2fau%2fpw%2fQJ6612208802.jpg&ehk=c%2f0WAvYPgW2uwrHdpUenuYb4L%2f0oQkiOmmJoSawaX4A%3d&risl=&pid=ImgRaw&r=0"
```

```
from zhipuai import ZhipuAI

client = ZhipuAI(api_key="")

response = client.chat.completions.create(
    model="glm-4v",
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": "图里有几个人，他们可能要去哪里"
                },
                {
                    "type": "image_url",
                    "image_url": {
                    }
                }
            ]
        }
    ],
)
print(response.choices[0].message)
```

### 响应示例

```
{
  "created": 1703487403,
  "id": "8239375684858666781",
  "model": "glm-4v",
  "request_id": "8239375684858666781",
  "choices": [
      {
          "finish_reason": "stop",
          "index": 0,
          "message": {
              "content": "图中有一片蓝色的海和蓝天，天空中有白色的云朵。图片的右下角有一个小岛或者岩石，上面长着深绿色的树木。",
              "role": "assistant"
          }
      }
  ],
  "usage": {
      "completion_tokens": 37,
      "prompt_tokens": 1037,
      "total_tokens": 1074
  }
}
```

## 流式调用

### 请求示例

```
from zhipuai import ZhipuAI
client = ZhipuAI(api_key="") 
response = client.chat.completions.create(
    model="glm-4v",
    messages=[
        {"role": "user",
          "content": [
          {
            "type": "text",
            "text": "图里有什么"
          },
          {
            "type": "image_url",
            "image_url": {
                "url" : "https://img1.baidu.com/it/u=1369931113,3388870256&fm=253&app=138&size=w931&n=0&f=JPEG&fmt=auto?sec=1703696400&t=f3028c7a1dca43a080aeb8239f09cc2f"
            }
          }
        ]
        },
    ],
    stream=True,
)
for chunk in response:
    print(chunk.choices[0].delta)
```

### 响应示例

```
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"delta":{"role":"assistant","content":"图"}}]}
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"delta":{"role":"assistant","content":"中"}}]}
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"delta":{"role":"assistant","content":"图片"}}]}
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"delta":{"role":"assistant","content":"的"}}]}
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"delta":{"role":"assistant","content":"右"}}]}
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"delta":{"role":"assistant","content":"下"}}]}
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"delta":{"role":"assistant","content":"角"}}]}
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"delta":{"role":"assistant","content":"有一个"}}]}
... ...
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"delta":{"role":"assistant","content":"树木"}}]}
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"delta":{"role":"assistant","content":"。"}}]}
data: {"id":"8305986882425703351","created":1705476637,"model":"glm-4v","choices":[{"index":0,"finish_reason":"stop","delta":{"role":"assistant","content":""}}],"usage":{"prompt_tokens":1037,"completion_tokens":37,"total_tokens":1074}}
```

## Cookbook教学（仅供参考）

教程可以解决如下问题：

1. 如何生成 `base64`
2. 返回的格式

地址：`https://github.com/zRzRzRzRzRzRzR/glm-cookbook/blob/main/vision/glm-v_pysdk.ipynb`
