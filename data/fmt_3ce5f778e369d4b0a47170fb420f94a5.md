
# 白嫖 `NanoBananaPro`：国内免费体验与 API 接入全指南

> 来源：`https://qqs7y1hozd1.feishu.cn/docx/B8cBdtkIAocyShxvQqmceZfdnuK`

本文整理了在国内无需🪜即可在线免费体验 `NanoBananaPro` 的多个网站，并介绍了通过 `OpenRouter` 与 `GRS AI Dashboard` 两个支持国内支付的中转平台调用 `NanoBananaPro` API 示例

官网每天能免费试用3次，但很多人就是用不了🙃

所以这篇就专门说说在国内不🪜，照样免费玩上 `NanoBananaPro`。

## 为什么明明官网免费，我却用不上？

`NanoBananaPro` 本身是可以在官网每天免费体验的，但有两个现实问题：

1. 在线使用的官网需要稳定好用的 VPN

某人每月 50 块钱的🪜，访问 `chatgpt` 都没问题，但一到 `NanoBananaPro` 官网就失败

1. 就算通过 `google AI Studio` 可以调用 `NanoBananaPro` API 但需要国外信用卡

国内卡就算有 `visa` 海外的功能也不行

最终筛出了几家目前在国内网络环境下还算稳定、而且能在线免费体验的站点，以及支持🛰️/支付宝支付的中转 `API`，分享给你。

## 一、无需🪜即可在线免费体验的渠道

下面这些网址都可以在国内网络直连打开，免费白嫖：

+ 在线体验 1：`https://styleai.art/`

![](img/eee18859777457a2b87a593f7404a62f.png)

+ 在线体验 2：`https://www.lovart.ai/`

![](img/892c9dbc9f23a7c9a9d9979c43114237.png)

+ 在线体验 3：`https://banana.listenhub.ai/`

![](img/82163d4c70e27cd9fe0462e026e15650.png)

+ 在线体验 4：`https://grsai.com/zh/dashboard/documents/nano-banana`

![](img/951b24c6eb27d582c125ec6428e0defa.png)

> ⚠️ 小提示：
>
> 这些站点的免费策略不太一样，有的是每天送大概 3 次免费调用机会，有的是注册就送一批积分。具体额度以后平台可能会调整，以页面说明为准。

## 二、API Dashboard（支持国内🛰️ / 支付宝）

如果：

+ 每天使用量远超过免费额度的调用次数

+ 想把 `NanoBananaPro` 接入自己的项目 / 网站 / `Bot` / 小程序

+ 写脚本、做自动化、批量处理内容

+ 把它当成一个真正的“生产力工具”而不是玩具

那就需要上 API 调用 这一层了。

目前比较适合国内用户、又支持 `NanoBananaPro` 的两个平台是：

+ `https://openrouter.ai/`

+ `https://grsai.com/`

它们有一个对国内用户非常友好的点： ✅ 这两个都支持国内的🛰️或支付宝支付

### 1. `OpenRouter`（`openrouter`）

+ 特色：

+ `OpenRouter` 是知名的多模型聚合平台，`NanoBananaPro` 只是其中之一

+ 你可以在同一个 `API Key` 上，选择调用不同家的模型

+ 支持国内支付方式（包括🛰️ / 支付宝）

+ 如果你后面不仅想用 `NanoBananaPro`，还想顺手玩玩其他家模型，这个平台非常适合做“统一中枢”

### 2. `GRS AI Dashboard`（`grs`）

+ 特色：

+ 官网提供试用和简单面板，方便你在写代码前先免费测接口效果

+ 网站本身就有中文界面

+ 支持🛰️ / 支付宝支付，更符合国内用户的使用习惯

## 三、API 调用教程

`OpenRouter` 和 `GRS AI Dashboard` 的 API 获取及调用方法。

### OpenRouter

#### 获取 API Key

按下图获取 `API Key`

![](img/f0ca2386ac3e6e92fc4f03fd0ae94037.png)

按照下图步骤充值（也有免费模型，但 `nano-banana-pro` 是需要充值的）

![](img/8f8104b32c3db44b3f55d06c0841f3e4.png)

#### 调用示例

其他方式可参考官方文档（模型对应的文档中包含所有调用示例）

```
import { OpenRouter } from "@openrouter/sdk";

const openrouter = new OpenRouter({ apiKey: "<OPENROUTER_API_KEY>" });

const result = await openrouter.chat.send({
  model: "google/gemini-3-pro-image-preview",
  messages: [
    {
      role: "user",
      content: "Generate a beautiful sunset over mountains"
    }
  ],
  modalities: ["image", "text"]
});

const message = result.choices[0].message;

if (message.images) {
  message.images.forEach((image, index) => {
    const imageUrl = image.image_url.url;
    console.log(`Generated image ${index + 1}: ${imageUrl.substring(0, 50)}...`);
  });
}
```

### GRS AI Dashboard

#### 获取 API Key

按下图获取 `API Key`

![](img/6cca7e965bb20b38e8b3f72dffa6257d.png)

按照下图步骤充值

![](img/808955e22682a5680ab216d7a7b1af4b.png)

#### 调用示例

其他方式可参考官方文档（在试用处既有对应的调用文档）

```
import time, requests

API_BASE = "https://grsai.dakka.com.cn"
API_KEY = "sk-d13feb96c3944f2190e21e4def0e28a3"  # 换成你的 key

headers = {
    "Content-Type": "application/json",
    "Authorization": f"Bearer " + API_KEY,
}

prompt = "一只可爱的猫咪在草地上玩玩具小车，旁边有一个猫窝，风格卡通"  # 1. 创建任务

create = requests.post(f"{API_BASE}/v1/draw/nano-banana",
    headers=headers,
    json={
        "model": "nano-banana-pro",
        "prompt": prompt,
        "aspectRatio": "16:9",
        "shutProgress": False,
        "webHook": "-1",
    },
).json()

task_id = create["data"]["id"]

# 2. 轮询结果接口，直到成功，打印图片 URL
while True:
    data = requests.post(f"{API_BASE}/v1/draw/result",
        headers=headers,
        json={"id": task_id},
    ).json()["data"]

    if data["status"] == "succeeded":
        print("图片 URL:", data["results"][0]["url"])
        break
    if data["status"] == "failed":
        print("任务失败:", data)
        break

    time.sleep(2)
