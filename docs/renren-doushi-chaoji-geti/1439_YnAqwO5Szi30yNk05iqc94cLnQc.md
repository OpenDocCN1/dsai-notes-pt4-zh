
# 2023-10-15 OpenAI 最新文生图模型 DALL-E 3，4 种使用方法

> 来源：[https://xuqiwei1986.feishu.cn/docx/QglHdd3Uioop1rxHpdQcDcBHncc](https://xuqiwei1986.feishu.cn/docx/QglHdd3Uioop1rxHpdQcDcBHncc)

> 🔗 原文链接：`https://mp.weixin.qq.com/s/caHrXvXDMjoGrJNFAhSa...`

收录于合集

OpenAI 74 个

微软 121 个

专注 AIGC 领域的专业社区，关注 OpenAI、百度文心一言等大语言模型（LLM）的发展和应用落地，关注 LLM 的基准评测和市场研究，欢迎关注！

DALL·E-3 是 OpenAI 在今年 9 月 21 日最新推出的文生图模型，在语义理解、颗粒处理、图像质量等方面，比 2022 年 3 月 25 日发布的 DALL·E-2 实现巨大提升。

DALL·E-3 可生成写实、二次元、平面、创意、朋克、3D 等多种类型，图片质量可媲美 Midjourney，文本提示理解甚至超过了它。

目前，DALL·E-3 处于测试阶段，可以通过 ChatGPT Plus、企业版用户或通过 API、OpenAI 实验室等方式进行体验。

但上述功能可能比较麻烦，最简单的方法就是在微软的 Bing Chat 和 Bing Image Create 中使用 DALL·E-3，并且免费支持中文提示。

Bing Image Create 地址：`https://www.bing.com/images/create`

Bing Chat 地址：`https://www.microsoft.com/zh-cn/edge/launch/bing-chat-3p?form=MY02CJ&OCID=MY02CJ&q`

![](img/b698db3538e18dff357520bcfd58f12f.png)

由 DALL·E-3 生成

下面「AIGC 开放社区」为大家介绍 4 种使用方法。ChatGPT Plus 和企业版用户可直接在提示中使用，这里就不介绍了。

## 一、在微软 Bing Chat 中使用 DALL·E-3

Bing Chat 的使用方法非常简单，打开 Bing 的聊天对话功能，直接在对话框中输入文本提示。

例如，生成一条东方巨龙，飞舞在天空中，满天云彩。很快就能获得 4 张精美图片。

![](img/4885db60cc50eac7f306be8fd97fcbd9.png)

![](img/4a9cdfc7bf3eef4b9a95fab241bf8185.png)

## 二、在 Bing Image Create 中使用

打开网址 `https://www.bing.com/images/create` 直接在对话框中输入内容即可。

![](img/d3bf7be67eb13a8ef5542466ece22f1c.png)

例如，一个多彩的冰淇淋，水果很多，色彩斑斓，3D 风格。

![](img/1a9fb64fe72fbbfac12a3d8fd7c9903c.png)

![](img/5bb3ed924cf0fbda015d23e3dd817a67.png)

需要注意的是，微软每周免费给 25 个生成币，用完后下周会补满。如果使用量大的话，可以多申请几个账号。

## 三、通过 OpenAI 实验室使用 DALL-E-3

打开地址 `labs.openai.com`。

登录 OpenAI 帐户。如果没有，创建一个新帐户。

账号注册完成后，在对话框中输入描述然后点击 `Generate` 即可，使用方法与 ChatGPT 一样。例如，顶部有一个大橘子的汽车。

![](img/0b3f1d035949e94f6f2a1147920f0abd.png)

通常会生成 4 张图片，如果对效果不满意可以继续生成。

![](img/3b41957a1e24012605b72476159b6228.png)

点击任意图片，可以进行编辑、共享和保存等操作。

![](img/fa8e9e29962092f2847c66842069c1b9.png)

## 四、通过 API 使用 DALL-E-3

开发人员可以通过 API 的方法将 DALL·E-3 集成到自己的应用程序中。下面是演示流程。

注册 OpenAI API，可以在 `https://platform.openai.com/account/api-keys` 进行注册。

安装 OpenAI Python 库，以使用 `pip` 安装 OpenAI Python 库。

![](img/53609e5ec1388172dad9a7de9cf77fb1.png)

导入库并进行 API 调用：安装库和 API 密钥后，就可以开始使用 DALL-E 3 了。以下是如何调用 DALL-E 生成图像的示例：

![](img/56d4722faf33aabf28b23b5e62a733ad.png)

注意，确保替换 `YOUR_API_KEY_HERE` 为你的 OpenAI API 密钥。可以自定义 `prompt` 以指定希望 DALL-E 生成哪种图像。

检索并使用生成的图像：该 `response` 对象包含生成的图像的 URL。可以根据需要使用此 URL 显示或下载图像。

## DALL·E-3 局限性

为了保证 DALL·E-3 生成的内容符合标准，OpenAI 禁止 DALL·E 3 生成暴力、仇恨、成人等非法内容；

禁止生成偏见、歧视、丑化公众人物的图片。同时内置了内容来源分类器，用于识别 DALL·E 3 生成的图片，用于非法用途。

此外，可以选择 DALL·E 3 是否可以提供现在艺术家风格图片：默认是允许生成；如果艺术家不允许可以申请将其作品，从训练数据中剔除。

本文素材来源微软 Bing Chat、网络等，如有侵权请联系删除

END

![](img/10745981040184bceb5b8909c133e197.png)

![](img/3ec0c76bec836840326c7878f9ded150.png)
