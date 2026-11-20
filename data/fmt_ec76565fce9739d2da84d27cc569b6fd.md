
# DeepSeek官网太卡？这3大替代方案请收好，分分钟丝滑用上满血版R1

> 来源：`https://my.feishu.cn/docx/QP1Dd3d2DopiTCxIHQTcDBGFn9d`

文章来源：`https://mp.weixin.qq.com/s/s_WLQL-kW_TOvuoOf1ZBcA`

朋友们开工大吉啊，新的一年要愉快搞钱、多多搞钱！

最新数据，DeepSeek R1 上线 16 天，日活跃用户（DAU）数已破 2000 万。已经超过 ChatGPT，成为全球增速最快的 AI 应用。

因为服务器压力太大，以及一些外在网络安全问题，加上春节放假的缘故，火爆全球的“国产之光”DeepSeek 近期官网拥堵严重，官网和 API 都暂停了对外开放注册，只有早期已注册用户才能用到。

DeepSeek 官方应该会在节后解决问题后重新开放注册，但可能还需要等待一段时间。

那么，这么好用的 ai 助手，对于心急的想用来节后开工开个挂的朋友们来说，怎么才能愉快体验到呢？是否有其他的可替代应急办法，可以用到强大纯正的 DeepSeek R1？今天就来仔细捋一捋！

![](img/b69c3f01f9a163b782df1bf49086a87d.png)

（配图由谷歌 ImageFX 生成）

## 01

DeepSeek 的开源模型，倒是可以尝试在本地运行，比如使用 LM Studio、Ollama 等工具。这些方法可以让用户在本地使用 DeepSeek 的功能，而且数据保留在本地，隐私性更好。但本地部署对于电脑终端的软硬件配置都要求较高，大部分朋友们的轻量办公笔记本，部署起来都比较麻烦，有一定上手门槛。因此，不太建议如我这般的科技小白们来折腾本地化部署这个方案。

相比更轻松一些的解决方案是，可以直接使用第三方网页对话，或者结合一些第三方 API 部署的工具来使用，基本也能体验到官方效果差不多的 DeepSeek R1 推理模型。

**说到第三方网页，海外主流 AI 应用工具** `Poe`、`Perplexity`、`together ai`、`groq` 等都接入了 `R1` 推理模型，以及国内秘塔搜索、360 的纳米搜索也已增加了 `R1` 的调用服务，都可以用到“开箱即用”的 DeepSeek R1。优点方便省心，优先推荐 `Poe`、`together ai`，体验下来这个 `R1` 比较纯正，速度也快，和官方的能力效果差不多。

**但是，免费体验次数有限**，`Poe` 免费账户一天 10 次。（`Poe` 网址：`https://poe.com/`）

![](img/476487a0efe6ccb5c5c66be057aac662.png)

（Poe 测试效果）

`Together` 可以在线体验（`https://api.together.ai/playground/chat/deepseek-ai/DeepSeek-R1`）或者调用 API 服务。

![](img/82ee95f8a977e215feda10ed2f49d1f3.png)

（Together.ai 测试效果）

号称“全球速度最快”的大模型集合平台 `Groq`，也已推出免费的 `R1`，不过是个 70b 精调版本，效果上虽不如官方原生，但比大多数套壳好很多，而且胜在速度快。感兴趣的可以体验一下：`https://chat.groq.com/?model=deepseek-r1-distill-llama-70b`

![](img/4bcfbe6b19a60b48370f387e25c9c2d3.png)

（Groq 测试效果）

## 02

其实，如果常用 ai 辅助日常的话，我个人更倾向于调用 API，网络上有不少公益免费 API 可用，也陆续有可靠的第三方 API 集合平台已经支持了 `R1`。有免费体验额度，自己也可以即充即用，DeepSeek“AI 界拼多多”价格也不高，充个一二十块都能用好长一阵子了。

这几天国产推理引擎 x 国产芯片 x 国产模型强强联合，`Siliconflow` 硅基流动与华为云昇腾云服务合作，推出了满血版 DeepSeek-R1 大模型 API，注册即送 2000 万 Tokens。

![](img/ab5ea39aece79b2691c12a478e7c6ca1.png)

（硅基流动联合华为云推出 R1）

### 具体教程如下：

API + 第三方部署工具：在 `DeepSeek API` 暂停开放注册的情况下，可以通过电脑端的 `Cherry Studio` 和手机端的 `ChatBot`/`ChatBox` 来调用 API 使用 DeepSeek。

如果想先轻度体验，连 app 都不想安装，这里也有快速体验通道：

DeepSeek R1 在线体验：`https://cloud.siliconflow.cn/open/models?target=deepseek-ai/DeepSeek-R1`

![](img/60b72a7abc0ea519d37b49474c770163.png)

（硅基流动官网模型广场）

![](img/f2053a658cf1ba1be7888c128c0ea681.png)

（硅基流动中的 DeepSeek R1 在线测试效果）

## 03 硅基流动 API 电脑端使用教程

### （电脑端推荐使用 Cherry Studio）

#### 第一步：获取免费 DeepSeek API

+ 首先，通过硅基流动官网注册通道：`https://cloud.siliconflow.cn/i/a1uVG82a`，进入硅基流动官网，进行账号注册，目前能领取到平台赠送的 14 元体验金（2000 万 Tokens 额度）。

![](img/e3a3ac41e659403b1b617ec475edd505.png)

（硅基流动官网）

+ 获取 `DeepSeek API Key` ：如果已经注册了 `DeepSeek` 账号，登录 `DeepSeek` 官网，进入 API 接入页面，创建 `API Key` 。

![](img/649fe0007efb3cb923f7dbcfe63b4cef.png)

（硅基流动 API 界面）

![](img/e9cf2af363debe36b3da6fc619bf1d1f.png)

（硅基流动 API 界面）

#### 第二步：电脑端下载 Cherry Studio 神器

### Cherry Studio+Siliconflow 硅基流动，完美调用 deepseek API

`Cherry Studio` 这个软件挺好用的，我用了近一年了，借助它可以使用不止 `Siliconflow` 硅基流动一家的 API，各家官方以及第三方 API 站点都可以轻松接入，分分钟在一个工具里用到 `ChatGPT`、`Claude`、`Gemini` 御三家先进模型，还能够备份本地聊天数据、建立个人知识库，妥妥的实用生产力工具，大家可以多多探索！🚀

![](img/fc209bcc5696b4f13816a6a8bfc50e9e.png)

![](img/9125ab5f3daf3054d5e450dcf31044d6.png)

### （Cherry Studio 官网页面）

### 安装流程指南：

+ 安装和配置 `Cherry Studio` ：从官方网站（`https://cherry-ai.com`）下载并安装 `Cherry Studio`。安装完成后，打开软件，按照提示进行初始配置。

+ **配置 `API`** ：在 `Cherry Studio` 中，找到 API 配置选项（左下角⚙设置按钮），选择「硅基流动」并将其勾选为“开启”状态，然后依次输入 API 地址（即 `DeepSeek` 的 `Base URL`：`https://api.deepseek.com`）、`API` 密钥（即你前面获取的 `API Key`）、选择模型 `deepseek-ai/DeepSeek-R1`。

（Cherry Studio 电脑客户端界面）

+ **设置「智能体」**

这里，特别讲解一下「智能体」功能模块，支持预设 `System Prompt` 来配置各种满足不同任务或扮演特定角色的智能体，可玩性很强，可以在日常聊天之外，进阶摸索一下。

![](img/6719805adb32166afd8e0c78e6ed0d1c.png)

（Cherry Studio 智能体配置界面）

这里的 `system Prompt` 采用了宝玉老师逆向的 deepseek 官方提示词，提示词也一并放到下面：

```
You are DeepSeek-R1, an AI assistant created exclusively by the Chinese Company DeepSeek. You&#x27;ll provide helpful, harmless, and detailed responses to all user inquiries. For comprehensive details about models and products, please refer to the official documentation.

# Key Guidelines:
1. **Identity &amp; Compliance**
   - Clearly state your identity as a DeepSeek AI assistant in initial responses.
   - Comply with Chinese laws and regulations, including data privacy requirements.

2. **Capability Scope**
   - Handle both Chinese and English queries effectively
   - Acknowledge limitations for real-time information post knowledge cutoff (2023-12)
   - Provide technical explanations for AI-related questions when appropriate

3. **Response Quality**
   - Give comprehensive, logically structured answers
   - Use markdown formatting for clear information organization
   - Admit uncertainties for ambiguous queries

4. **Ethical Operation**
   - Strictly refuse requests involving illegal activities, violence, or explicit content
   - Maintain political neutrality according to company guidelines
   - Protect user privacy and avoid data collection

5. **Specialized Processing**
   - Use &lt;think&gt;...&lt;/think&gt; tags for internal reasoning before responding
   - Employ XML-like tags for structured output when required

Knowledge cutoff: {{current_date}}
```

+ **开始使用** ：配置完成后，就可以在左上角「助手-默认助手」对话界面选择 `R1` 模型聊天啦！

![](img/750120a2aaf2629aa0d2160546a2ec13.png)

（Cherry Studio 聊天对话界面）

对，就是这么简单，分分钟就可以实现在 `Cherry Studio` 中通过 API 调用 DeepSeek 的功能，如进行对话聊天、代码生成、文本生成、知识库管理等。

亲测对话速度还是挺快的，比之前买第三方 API token 丝滑太多了，相见恨晚呐～

另外左侧边栏还有强大的「`智能体`、`图片生成`、`翻译`、`知识库`」等丰富功能，甚至有 `小程序` 专区，可以直达任意 AI 大模型的官方网页直接开聊。

![](img/d9e4369ea13466b4e122de0cb4ea5a2b.png)

（Cherry Studio 小程序界面）

超级实用的 AI 客户端，真心推荐，更多功能朋友们可以慢慢体验。

## 04 硅基流动 API 手机端使用教程

### （手机端推荐使用 ChatBot）

#### 第一步：下载 Chatbot 手机 APP

**获取 `硅基流动 API`** 后，手机下载 `ChatBot` APP（地址：`https://github.com/fanenr/flutter-chatbot`）

这个是我经常用的，考虑到手机端的轻便性，这个 app 也不复杂繁重，是一个个人开发者搞的，很方便实用。

界面很简单，和前面 `Cherry Studio` 原理类似。

#### 第二步：部署教程

+ 点击「⚙设置」- 接口 - 新接口；

+ 再依次填入 API 地址、密钥、拉取和选择模型；

+ 然后，回到设置中的「配置」一栏，选择好前面部署的硅基流动接口以及 `deepseek-ai/DeepSeek-R1` 模型。

（Chatbot 手机 APP 模型接口界面）

除此之外，这个 APP 的设置栏还提供了「角色」项，其实也就是智能体。可以将承担各种不同任务处理或对话聊天角色的 `Prompt` 填入进去，给智能体预设一个 `System Prompt`（与前面的 `Cherry Studio` 智能体设置同理），这样对话起来就更高效实用了，感兴趣的朋友可以后期慢慢摸索体验～

（Chatbot 手机 APP 智能体配置界面）

![](img/46909c5b77e7026a4e6ebd29be3fc286.png)

（Chatbot 手机 APP 基础功能配置界面）

这时候，就可以回到主界面的对话窗口，用 `R1` 畅聊啦！

![](img/48194bc9f83f818c4858532910729304.png)

（Chatbot 手机 APP 聊天演示界面）

如果需要用到更复杂功能的话呢，这里推荐另外一个手机端 APP，也是很受欢迎、很多人用的 `Chatbox`（`https://chatboxai.app`）。UI 和功能设计上，相比前面的 `chatbot` 更丰富一些，并且已同时提供电脑端和手机端，不想下载 app 还可以直接使用网页端，也很主流的 AI 配置工具。

（Chatbox 官网界面）

到这里，就可以日常在电脑和手机上使用到满血版 `DeepSeek-R1` 啦，作为 DeepSeek 官网网页和 API 的补充或替代，是目前很不错的解决方案。

最后补充一点，硅基流动获取的 API，除了前面这样和聊天工具结合用，还有个绝妙的用处：

可以用 `DeepSeek-V3` 或者 `Qwen2.5-7B-Instruct` / `glm-4-9b-chat` 的免费 API（这两个分别是同属国内第一阵营的阿里通义千问与智谱清言的 AI 大模型），来接入火爆全网的沉浸式翻译（官网地址：`https://immersivetranslate.com`）

![](img/d1d77b1fd1f20477154541a26340601e.png)

（沉浸式翻译官网界面）

翻译效果很不错，大模型做翻译有比原先的传统机器翻译相当大的优势，这里就不多做赘述，回头再开一篇来详细说明。

等不及的朋友，可以自己先操练起来，这种集合工具站，用习惯了都不会想念官网了～

以上，不管是 `第三方网页`，还是 `电脑端、手机端` 自配置，这 `三大替代方案`，都可以轻松用上如德芙般丝滑的满血版 `DeepSeek R1`，从此实现 DeepSeek 自由，告别卡顿焦虑！

最后，再次祝大家新春愉快、开工大吉！让我们乘着这波 AI 东风，借助 AI 超强 buff，专业技能狂飙升级，2025 在职场如游龙顺势，风生水起、所向披靡！

---

—— THE END ——

作者简介：文森特，营销策划人，野生 `Promptor` 一枚。关注 AIGC 场景化落地应用、内容生产提效，致力于 AI 与品牌营销的创新实践，在此分享一些有关商业洞察、营销思考、内容创作及 AI 应用技巧等方面的所见、所得、所感。

既然看到这里了，如果觉得我的内容还不错，不妨顺手点个赞、在看、转发三连吧～想要第一时间收到推送，也可以给我个星标⭐欢迎点击下方👸号关注我👇

![](img/c33abb32bf5ca1a13d6d4fbeedd1f168.png)

![](img/e0e0bff0735535687d97e5523d8f3998.png)
