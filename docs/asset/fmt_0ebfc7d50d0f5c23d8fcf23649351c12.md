
# 火山引擎提示语优化代码

> 来源：[`https://aiwarts101.feishu.cn/docx/RXRzdkocgoGJq6xMTSbcB5PHnqc`](https://aiwarts101.feishu.cn/docx/RXRzdkocgoGJq6xMTSbcB5PHnqc)

```
import agent_pilot as ap
from volcenginesdkarkruntime import Ark
import os
import time

generated_prompt = &quot;&quot;
Usage = None
print(&quot;Streaming response：&quot;)
for chunk in ap.generate_prompt_stream(
    task_description=&quot;&quot;&quot;使用vefass将网页发布，并返回可以访问的地址&quot;&quot;&quot;,
    model_name=&quot;doubao-1.5-pro-32k-250115&quot;,
    temperature=1.0,
    top_p=0.7,
    api_key=&quot;4bf7b9c3-8196-4dd8-870d-51e84dcfa3f9&quot;,
    api_url=&quot;https://sd0roa2mjbbc44uo99850.apigateway-cn-beijing.volceapi.com&quot;,
):
    # 处理流式输出角色：你是一个智能航班
    generated_prompt += chunk.data.content
    print(chunk.data.content, end=&quot;&quot;,flush=True)
    if chunk.event == &quot;usage&quot;:
        usage = chunk.data.usage

print(f&quot;【Generated prompt】:{str(generated_prompt)}&quot;)
print(f&quot;【Usage】:{usage}&quot;)
```

## 火山引擎官方🔗

`https://www.volcengine.com/docs/82379/1587837`

## 感谢关注！

![](img/051ab1427b31689b726cea5163f3ee1a.png)

![](img/279220fe6317152e5561c3156cf9661e.png)
