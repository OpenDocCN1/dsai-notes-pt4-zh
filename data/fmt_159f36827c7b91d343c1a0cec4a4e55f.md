
# 前置准备

> 来源：[https://gotoaiworld.feishu.cn/docx/MmxZdjMu4o2c3txBdBUc7O6Bnnf](https://gotoaiworld.feishu.cn/docx/MmxZdjMu4o2c3txBdBUc7O6Bnnf)

> 首先注册火山方舟账号：[**火山方舟注册链接**](https://console.volcengine.com/ark/region:ark+cn-beijing/overview?briefPage=0&briefType=introduce&type=new&utm_campaign=2025H2&utm_content=gongjuxiang&utm_medium=xianshang&utm_source=LBM-zhyx&utm_term=dyjx_wendang)

## 开通模型

> 点击查看选型建议 [了解火山方舟大模型](https://bytedance.larkoffice.com/wiki/AxwJwyb7XiIP1GkQwd9ccC7dnMc)

进入[火山方舟-开通管理](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false&utm_campaign=2025H2&utm_content=gongjuxiang&utm_medium=xianshang&utm_source=LBM-zhyx&utm_term=dyjx_wendang)，模型开通不涉及任何费用

![](img/db4e4e003e947e18fe8aab60c77463a6.png)

![](img/4e5e5bf76ad212e08d28299e409a284b.png)

## `API key` 获取

> 提醒：注意`API key`的保密，不要随意泄漏

![](img/fa1999e659b074ababd3289280e73bbc.png)

![](img/39f2ae9d742c6b0affce416dd74e1ef5.png)

## 接入点id（`endpoint_id`）获取

> 火山方舟提供两种类型的模型接入点

### 预置推理接入点

使用`model_id`作为`endpoint_id`，将会自动使用预置推理接入点，无需创建

| 模态 | 模型名称 | `model_id` |
| --- | --- | --- |
| 多模态理解 | Doubao-Seed-1.6 | `doubao-seed-1-6-250615` |
| | Doubao-Seed-1.6-thinking | `doubao-seed-1-6-thinking-250715` |
| | | `doubao-seed-1-6-thinking-250615` |
| | Doubao-Seed-1.6-flash | `doubao-seed-1-6-flash-250715` |
| | | `doubao-seed-1-6-flash-250615` |
| | Doubao-Seed-1.6-vision | `doubao-seed-1-6-vision-250815` |
| | Doubao-1.5-thinking-vision-pro | `doubao-1-5-thinking-vision-pro-250428` |
| 文本生成 | Doubao-1.5-pro-32k | `doubao-1-5-pro-32k-250115` |
| | | `doubao-1-5-pro-32k-character-250715` |
| | | `doubao-1-5-pro-32k-character-250228` |
| | Doubao-1.5-lite-32k | `doubao-1-5-lite-32k-250115` |
| | Doubao-1.5-thinking-pro | `doubao-1-5-thinking-pro-250415` |
| 视觉理解 | Doubao-1.5-vision-pro | `doubao-1.5-vision-pro-250328` |
| | Doubao-1.5-vision-lite | `doubao-1.5-vision-lite-250315` |
| 视频生成 | Doubao-Seedance-1.0-pro | `doubao-seedance-1-0-pro-250528` |
| | Doubao-Seedance-1.0-lite-i2v | `doubao-seedance-1-0-lite-i2v-250428` |
| | Doubao-Seedance-1.0-lite-t2v | `doubao-seedance-1-0-lite-t2v-250428` |
| 图像生成 | Doubao-SeedEdit-3.0-i2i | `doubao-seededit-3-0-i2i-250628` |
| | Doubao-Seedream-3.0-t2i | `doubao-seedream-3-0-t2i-250415` |

### 自定义推理接入点

![](img/d18ad42e489520d84d5cb42a7b26d1bf.png)

![](img/b8e67c8559a57510cf1f79615bb5b39c.png)

## `AK`、`SK`获取

> 提醒：注意`AK`、`SK`的保密，不要随意泄漏

+ 进入[火山方舟控制台](https://console.volcengine.com/ark/region:ark+cn-beijing/overview?briefPage=0&briefType=introduce&type=new&utm_campaign=2025H2&utm_content=gongjuxiang&utm_medium=xianshang&utm_source=LBM-zhyx&utm_term=dyjx_wendang)，点击右上角个人中心，选择「API访问密钥」
+ 点击「新建密钥」，获取`AK`（Access Key ID）、`SK`（Secret Access Key）

[参考文档](https://www.volcengine.com/docs/84313/1254467)

![](img/51051db36286086900980ee9620bd210.png)
