# GLM-4V 图像理解

> 来源：[https://ai.feishu.cn/docx/CnVideMmcowC08xdCGwcLNrYnah](https://ai.feishu.cn/docx/CnVideMmcowC08xdCGwcLNrYnah)

模型编码：glm-4v

根据输入的自然语言指令和图像信息完成任务，推荐使用 SSE 或同步调用方式请求接口

模型编码：glm-4v-plus

提升版本 GLM-4V-Plus 具备卓越的图像理解能力，并具备基于时间感知的视频理解能力。

## 接口信息

## 接口参数

## 非流调用

### 请求示例

```
from zhipuai import ZhipuAI

client = ZhipuAI(api_key=&quot;&quot;)

response = client.chat.completions.create(
    model=&quot;glm-4v&quot;,
    messages=[
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
                        &quot;url&quot;: &quot;https://ts1.cn.mm.bing.net/th/id/R-C.3997eb10814025183c8fbbc6b5373713?rik=fSPsSA1rrKXAzw&amp;riu=http%3a%2f%2fdpic.tiankong.com%2fau%2fpw%2fQJ6612208802.jpg&amp;ehk=c%2f0WAvYPgW2uwrHdpUenuYb4L%2f0oQkiOmmJoSawaX4A%3d&amp;risl=&amp;pid=ImgRaw&amp;r=0&quot;
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
  &quot;created&quot;: 1703487403,
  &quot;id&quot;: &quot;8239375684858666781&quot;,
  &quot;model&quot;: &quot;glm-4v&quot;,
  &quot;request_id&quot;: &quot;8239375684858666781&quot;,
  &quot;choices&quot;: [
      {
          &quot;finish_reason&quot;: &quot;stop&quot;,
          &quot;index&quot;: 0,
          &quot;message&quot;: {
              &quot;content&quot;: &quot;图中有一片蓝色的海和蓝天，天空中有白色的云朵。图片的右下角有一个小岛或者岩石，上面长着深绿色的树木。&quot;,
              &quot;role&quot;: &quot;assistant&quot;
          }
      }
  ],
  &quot;usage&quot;: {
      &quot;completion_tokens&quot;: 37,
      &quot;prompt_tokens&quot;: 1037,
      &quot;total_tokens&quot;: 1074
  }
}
```

## 流式调用

### 请求示例

```
from zhipuai import ZhipuAI
client = ZhipuAI(api_key=&quot;&quot;) 
response = client.chat.completions.create(
    model=&quot;glm-4v&quot;,
    messages=[
        {&quot;role&quot;: &quot;user&quot;,
          &quot;content&quot;: [
          {
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;图里有什么&quot;
          },
          {
            &quot;type&quot;: &quot;image_url&quot;,
            &quot;image_url&quot;: {
                &quot;url&quot; : &quot;https://img1.baidu.com/it/u=1369931113,3388870256&amp;fm=253&amp;app=138&amp;size=w931&amp;n=0&amp;f=JPEG&amp;fmt=auto?sec=1703696400&amp;t=f3028c7a1dca43a080aeb8239f09cc2f&quot;
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
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;图&quot;}}]}
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;中&quot;}}]}
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;图片&quot;}}]}
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;的&quot;}}]}
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;右&quot;}}]}
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;下&quot;}}]}
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;角&quot;}}]}
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;有一个&quot;}}]}
... ...
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;树木&quot;}}]}
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;。&quot;}}]}
data: {&quot;id&quot;:&quot;8305986882425703351&quot;,&quot;created&quot;:1705476637,&quot;model&quot;:&quot;glm-4v&quot;,&quot;choices&quot;:[{&quot;index&quot;:0,&quot;finish_reason&quot;:&quot;stop&quot;,&quot;delta&quot;:{&quot;role&quot;:&quot;assistant&quot;,&quot;content&quot;:&quot;&quot;}}],&quot;usage&quot;:{&quot;prompt_tokens&quot;:1037,&quot;completion_tokens&quot;:37,&quot;total_tokens&quot;:1074}}
```

## Cookbook教学（仅供参考）

教程可以解决如下问题：

1.  如何生成base64

1.  返回的格式

地址：https://github.com/zRzRzRzRzRzRzR/glm-cookbook/blob/main/vision/glm-v_pysdk.ipynb