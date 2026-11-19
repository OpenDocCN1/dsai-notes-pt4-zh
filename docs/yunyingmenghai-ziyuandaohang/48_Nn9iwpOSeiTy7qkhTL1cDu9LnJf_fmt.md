
# chatgpt-on-wechat基于大模型搭建的聊天机器人

> 来源：[https://yunyingmenghai.feishu.cn/docx/QK4Pd4NxYo6oFfxkyrfcznGznFb](https://yunyingmenghai.feishu.cn/docx/QK4Pd4NxYo6oFfxkyrfcznGznFb)

## 整合包

链接：https://pan.quark.cn/s/f57352cf5745

基于大模型搭建的聊天机器人，同时支持 🛰️👸号、企业🛰️应用、飞书、钉钉 等接入，可选择GPT3.5/GPT-4o/GPT-o1/ Claude/文心一言/讯飞星火/通义千问/ Gemini/GLM-4/Claude/Kimi/LinkAI，能处理文本、语音和图片，访问操作系统和互联网，支持基于自有知识库进行定制企业智能客服。

使用需谨慎，容易封号，我个人实现了一个🛰️GPT聊天机器人

[基于ChatGPT](https://github.com/openai/openai-python)的🛰️聊天机器人，通过 [ChatGPT](https://github.com/littlecodersh/ItChat) 接口生成对话内容，使用`itchat`实现🛰️消息的接收和自动回复。已实现的特性如下：

+   **文本对话：** 接收私聊及群组中的🛰️消息，使用ChatGPT生成回复内容，完成自动回复

+   **规则定制化：** 支持私聊中按指定规则触发自动回复，支持对群组设置自动回复白名单

+   **多账号：** 支持多🛰️账号同时运行

+   **图片生成：** 支持根据描述生成图片，并自动发送至个人聊天或群聊

+   **上下文记忆**：支持多轮对话记忆，且为每个好友维护独立的上下会话

+   **语音识别：** 支持接收和处理语音消息，通过文字或语音回复

+   **插件化：** 支持个性化功能插件，提供角色扮演、文字冒险游戏等预设插件

## 快速开始

### 本地开发环境

本地开发环境支持 MacOS、Windows、Linux 系统，需要安装`python`环境，推荐的版本在 3.7.1 ~ 3.9.X 之间，可前往 [官网](https://www.python.org/downloads/) 下载。

### 提示

在命令行执行`python3 -V`，如果能显示正确的版本则表示安装成功。需要注意的是，`python3` 只是一个指向`python`解释器的引用，有可能在你的系统里这个别名是`python`或者`python3.8`，那么在后面你就需要用`python`或`python3.8`来代替`python3`运行程序。同样的，执行`pip3 -V`来确认已经成功安装包管理软件`pip`。

### 下载源码

在命令行使用`git`克隆项目并进入目录：

```
git clone https://github.com/zhayujie/chatgpt-on-wechat
cd chatgpt-on-wechat/
```

### 安装依赖

```
pip3 install -r requirements.txt                必选依赖
pip3 install -r requirements-optional.txt       可选依赖，语音、tool插件等功能需要
```

### 配置

复制项目中的模板文件`config-template.json`，来生成最终起效果的配置文件`config.json`，你可以通过执行以下命令完成：

```
cp config-template.json config.json
```

然后打开 [`config.json` 文件](https://docs.link-ai.tech/cow/quick-start/config)添加所需配置，具体配置含义参考 配置说明。

以下是对默认配置的说明，可根据需要进行自定义修改（在实际json文件中要去掉注释哦）：

支持的模型:

```
MODEL_LIST = [
              GPT35, GPT35_0125, GPT35_1106, &quot;gpt-3.5-turbo-16k&quot;,
              O1, O1_MINI, GPT_4o, GPT_4O_0806, GPT_4o_MINI, GPT4_TURBO, GPT4_TURBO_PREVIEW, GPT4_TURBO_01_25, GPT4_TURBO_11_06, GPT4, GPT4_32k, GPT4_06_13, GPT4_32k_06_13,
              WEN_XIN, WEN_XIN_4,
              XUNFEI,
              ZHIPU_AI, GLM_4, GLM_4_PLUS, GLM_4_flash, GLM_4_LONG, GLM_4_ALLTOOLS, GLM_4_0520, GLM_4_AIR, GLM_4_AIRX,
              MOONSHOT, MiniMax,
              GEMINI, GEMINI_PRO, GEMINI_15_flash, GEMINI_15_PRO,GEMINI_20_flash_exp,
              CLAUDE_3_OPUS, CLAUDE_3_OPUS_0229, CLAUDE_35_SONNET, CLAUDE_35_SONNET_1022, CLAUDE_35_SONNET_0620, CLAUDE_3_SONNET, CLAUDE_3_HAIKU, &quot;claude&quot;, &quot;claude-3-haiku&quot;, &quot;claude-3-sonnet&quot;, &quot;claude-3-opus&quot;, &quot;claude-3.5-sonnet&quot;,
              &quot;moonshot-v1-8k&quot;, &quot;moonshot-v1-32k&quot;, &quot;moonshot-v1-128k&quot;,
              QWEN, QWEN_TURBO, QWEN_PLUS, QWEN_MAX,
              LINKAI_35, LINKAI_4_TURBO, LINKAI_4o
            ]
```

config.json文件内容示例

```
{
  &quot;open_ai_api_key&quot;: &quot;YOUR API KEY&quot;,                          填入上面创建的 OpenAI API KEY
  &quot;model&quot;: &quot;gpt-3.5-turbo&quot;,                                   模型名称。当use_azure_chatgpt为true时，其名称为Azure上model deployment名称
  &quot;proxy&quot;: &quot;&quot;,                                                代理客户端的ip和端口
  &quot;single_chat_prefix&quot;: [&quot;bot&quot;, &quot;@bot&quot;],                      私聊时文本需要包含该前缀才能触发机器人回复
  &quot;single_chat_reply_prefix&quot;: &quot;[bot] &quot;,                       私聊时自动回复的前缀，用于区分真人
  &quot;group_chat_prefix&quot;: [&quot;@bot&quot;],                              群聊时包含该前缀则会触发机器人回复
  &quot;group_name_white_list&quot;: [&quot;ChatGPT测试群&quot;, &quot;ChatGPT测试群2&quot;], 开启自动回复的群名称列表
  &quot;group_chat_in_one_session&quot;: [&quot;ChatGPT测试群&quot;],              支持会话上下文共享的群名称
  &quot;image_create_prefix&quot;: [&quot;画&quot;, &quot;看&quot;, &quot;找&quot;],                   开启图片回复的前缀
  &quot;conversation_max_tokens&quot;: 1000,                            支持上下文记忆的最多字符数
  &quot;speech_recognition&quot;: false,                                是否开启语音识别
  &quot;group_speech_recognition&quot;: false,                          是否开启群组语音识别
  &quot;use_azure_chatgpt&quot;: false,                                 是否使用Azure ChatGPT service代替openai ChatGPT service. 当设置为true时需要设置 open_ai_api_base，如 https://xxx.openai.azure.com/
  &quot;azure_deployment_id&quot;: &quot;&quot;,                                  采用Azure ChatGPT时，模型部署名称
  &quot;character_desc&quot;: &quot;你是ChatGPT, 一个由OpenAI训练的大型语言模型, 你旨在回答并解决人们的任何问题，并且可以使用多种语言与人交流。&quot;,  人格描述
  订阅消息，公众号和企业微信channel中请填写，当被订阅时会自动回复，可使用特殊占位符。目前支持的占位符有{trigger_prefix}，在程序中它会自动替换成bot的触发词。
  &quot;subscribe_msg&quot;: &quot;感谢您的关注！\n这里是ChatGPT，可以自由对话。\n支持语音对话。\n支持图片输出，画字开头的消息将按要求创作图片。\n支持角色扮演和文字冒险等丰富插件。\n输入{trigger_prefix}#help 查看详细指令。&quot;,
  &quot;use_linkai&quot;: false,                                        改为true后将使用LinkAI的服务，无需海外网络环境和OpenAI账号。&quot;
  &quot;linkai_api_key&quot;: &quot;&quot;,                                       LinkAI平台的api key
  &quot;linkai_app_code&quot;: &quot;&quot;                                       LinkAI平台创建的应用的code
}
```

#### 配置说明

##### 1. 个人聊天

+   个人聊天中，需要以`bot`或`@bot`为开头的内容触发机器人，对应配置项`single_chat_prefix` (如果不需要以前缀触发可以填写`"single_chat_prefix": [""]`)

+   机器人回复的内容会以`"[bot] "`作为前缀，以区分真人，对应的配置项为`single_chat_reply_prefix` (如果不需要前缀可以填写`"single_chat_reply_prefix": ""`)

##### 2. 群组聊天

+   群组聊天中，群名称需配置在`group_name_white_list`中才能开启群聊自动回复。如果想对所有群聊生效，可以直接填写`"group_name_white_list": ["ALL_GROUP"]`

+   默认只要被人`@`就会触发机器人自动回复；另外群聊天中只要检测到以`@bot`开头的内容，同样会自动回复（方便自己触发），这对应配置项`group_chat_prefix`

+   可选配置:`group_name_keyword_white_list`配置项支持模糊匹配[群名称，group_chat_keyword](https://github.com/evolay)，`group_chat_keyword`配置项则支持模糊匹配群消息内容，用法与上述两个配置项相同。（Contributed by evolay)

+   `group_chat_in_one_session`：使群聊共享一个会话上下文，配置`["ALL_GROUP"]`则作用于所有群聊

##### 3. 语音识别

+   添加`"speech_recognition": true`将开启语音识别，默认使用openai的whisper模型识别为文字，同时以文字回复，该参数仅支持私聊 (注意由于语音消息无法匹配前缀，一旦开启将对所有语音自动回复，支持语音触发画图)；

+   添加`"group_speech_recognition": true`将开启群组语音识别，默认使用openai的whisper模型识别为文字，同时以文字回复，参数仅支持群聊 (会匹配`group_chat_prefix`和`group_chat_keyword`, 支持语音触发画图)；

+   添加`"voice_reply_voice": true`将开启语音回复语音（同时作用于私聊和群聊），但是需要配置对应语音合成平台的key，由于`itchat`协议的限制，只能发送语音mp3文件，若使用`wechaty`则回复的是🛰️语音。

##### 4. 其他配置

+   `model`: 模型名称，目前支持 gpt-3.5-turbo, gpt-4o, gpt-4o-mini, gpt-4-turbo, gpt-4, wenxin , claude , gemini, glm-4, xunfei, moonshot

+   `temperature`,`frequency_penalty`,`presence_penalty`: Chat API接口参数，详情参考OpenAI官方文档。

+   `proxy`: 由于目前 openai 接口国内无法访问，需配置代理客户端的地址，详情参考 #351

+   对于图像生成，在满足个人或群组触发条件外，还需要额外的关键词前缀来触发，对应配置`image_create_prefix`

+   关于OpenAI对话及图片接口的参数配置（内容自由度、回复字数限制、图片大小等），可以参考 [对话接口](https://github.com/zhayujie/chatgpt-on-wechat/blob/master/config.py) 和 图像接口 文档，在`config.py`中检查哪些参数在本项目中是可配置的。

+   `conversation_max_tokens`：表示能够记忆的上下文最大字数（一问一答为一组对话，如果累积的对话字数超出限制，就会优先移除最早的一组对话）

+   `rate_limit_chatgpt`，`rate_limit_dalle`：每分钟最高问答速率、画图速率，超速后排队按序处理。

+   `clear_memory_commands`: 对话内指令，主动清空前文记忆，字符串数组可自定义指令别名。

+   `hot_reload`: 程序退出后，暂存🛰️扫码状态，默认关闭。

+   `character_desc` [配置中保存](https://github.com/zhayujie/chatgpt-on-wechat/issues/43)着你对机器人说的一段话，他会记住这段话并作为他的设定，你可以为他定制任何人格 (关于会话上下文的更多内容参考该 issue)

+   `subscribe_msg`：订阅消息，👸号和企业🛰️channel中请填写，当被订阅时会自动回复， 可使用特殊占位符。目前支持的占位符有`{trigger_prefix}`，在程序中它会自动替换成`bot`的触发词。

#### 注意

1.  全量配置可在 [`config.py`](https://github.com/zhayujie/chatgpt-on-wechat/blob/master/config.py) 中查看，但该文件仅做配置的说明，直接修改是无效的，`config.json` 文件才是最终生效的配置文件。

1.  如果启动程序失败并报错`JSONDecodeError`，[就是`config.json`](https://www.json.cn/)中配置文件的 json 格式错误，报错中一般会提示哪一行出现问题。也可以去 [在线JSON校验](https://www.json.cn/) 网站进行验证，同时需要注意`openai_key`的安全。

### 运行

一切准备就绪，可以启动程序了，直接在项目目录下执行：

```
python3 app.py
```

扫描输出的二维码即可完成登录。如果你使用的是 pycharm 等IDE，就更方便了，点击运行按钮就可以直接运行。

## 三、使用

成功运行之后日志中会输出`"Start auto replying"`，这表示你用于扫码登录的账号已经变身为机器人了，让你的好友给你发消息 或在群聊@你 都会触发自动回复，效果如下图：

常见问题



# 常见问题排查

1. 如果发送消息没有回复，对于私聊场景确认下消息是否携带了触发词，对应配置`single_chat_prefix`，对于群聊场景要确认该群名称是否配置在群组白名单`group_name_white_list`中。

1. 如果机器人回复 "我现在有点累了" 或 "我连接不到你的网络" 等，一般是访问openAI接口失败，需查看日志中的具体报错信息来定位，可能是网络不通、openAI账号额度不足、访问频率超限等原因。

1. 可能存在一定账号被限制的风险，建议仅用于技术研究或娱乐，不要过高频率使用。

常见问题查看 [FAQs](https://github.com/zhayujie/chatgpt-on-wechat/wiki/FAQs)，或直接与 [在线项目小助手](https://link-ai.tech/app/Kv2fXJcH) 对话。
