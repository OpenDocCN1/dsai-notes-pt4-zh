
# AIbot

> 来源：[https://my.feishu.cn/docx/J5ADdmL2IoapiWxZp5fc31Vmn3A](https://my.feishu.cn/docx/J5ADdmL2IoapiWxZp5fc31Vmn3A)

## 简介

> 本项目属于`chatgpt-on-wechat`的 PC Windows 端个人🛰️版，基于`WeChat-AIChatbot-WinOnly`，由于跃迁大佬停更，所以备份一下，同时丰富一下原`ntchat`消息通道监听类型，方便开发对应类型插件。

> 本项目仅供学习和技术研究，请勿用于非法用途，如有任何人凭此做何非法事情，均于作者无关，特此声明。

+ 2024.4月开始🛰️限制低版本登录，为提高本项目使用门槛，故不提供低版本登录解决方案，请自行解决 *

+ 只能在Win平台运行项目！只能在Win平台运行项目！只能在Win平台运行项目！（重要的事情说三遍）

+ 项目支持功能如下：

+ `Wechat`：[PC端的个微](https://github.com/billyplus/ntchat)消息通道，依赖`ntchat`项目，最高支持Python3.10环境版本，限`WeChat3.6.0.18`版本，

+ 发送消息：文本/图片/视频/文件/群聊@/链接卡片/GIF/XML

+ 接收消息：几乎涵盖所有消息类型

+ 其他功能：同意加好友请求/创建群/添加好友入群/邀请好友入群/删除群成员/修改群名/修改群公告

+ 短板缺陷：无法发送语音条信息

## 详细功能列表

+ 聊天对话：私聊、群聊对话，支持`fastgpt`、`coze`、`linkai`、`openai`、`azure`、文心一言，通义千问，讯飞星火，`ChatGLM-4`，`MiniMax`，`deepspeek`，`gemini`，`claudeapi`，`Kimi`对话模型通道

+ 语音对话：语音消息可选文字或语音回复，支持`Azure`，`Openai`，`Google`等语音模型

+ 插件：`chatgpt-on-wechat`项目的插件可以复制到本项目来使用，也可自行开发插件

+ 高度定制：依赖`fastgpt`接口，可实现每个群聊对应不同的应用知识库

## 快速开始

### 准备

#### 1. 运行环境

仅支持Windows系统同时需安装`Python`。

> 建议`Python`版本在 3.7.1~3.10 之间。

#### (1) 下载项目代码：

`git clone https://github.com/Tishon1532/chatgpt-on-wechat-win`

`cd chatgpt-on-wechat-win/`

#### (2) 安装核心依赖（必选）：

`pip3 install -r requirements.txt`

#### (3) 拓展依赖（可选，建议安装）：

`pip3 install -r requirements-optional.txt`

> 如果某项依赖安装失败请注释掉对应的行再继续。

其中`tiktoken`要求`Python`版本在3.8以上，它用于精确计算会话使用的`tokens`数量，强烈建议安装。

使用`google`或`baidu`语音识别需安装`ffmpeg`，

默认的`openai`语音识别不需要安装`ffmpeg`。

参考[#415](https://github.com/zhayujie/chatgpt-on-wechat/issues/415)

使用`azure`语音功能需安装依赖，并参考[文档](https://learn.microsoft.com/en-us/azure/cognitive-services/speech-service/quickstarts/setup-platform?pivots=programming-language-python&tabs=linux%2Cubuntu%2Cdotnet%2Cjre%2Cmaven%2Cnodejs%2Cmac%2Cpypi)的环境要求。：

`pip3 install azure-cognitiveservices-speech`

### 配置

配置文件的模板在根目录的`config-template.json`中，需复制该模板创建最终生效的`config.json`文件：

`cp config-template.json config.json`

然后在`config.json`中填入配置，以下是对默认配置的说明，可根据需要进行自定义修改（请去掉注释）：[config.py](https://github.com/Tishon1532/chatgpt-on-wechat-win/blob/master/config.py) 更多可选配置项可以参考`config.py`。

```
# config.json文件内容示例
{
  &quot;open_ai_api_key&quot;: &quot;YOUR API KEY&quot;,                          # 填入上面创建的 OpenAI API KEY
  &quot;model&quot;: &quot;gpt-4o-mini&quot;,                                     # 模型名称。当use_azure_chatgpt为true时，其名称为Azure上model deployment名称
  &quot;proxy&quot;: &quot;&quot;,                                                # 代理客户端的ip和端口，国内环境开启代理的需要填写该项，如 &quot;127.0.0.1:7890&quot;
  &quot;single_chat_prefix&quot;: [&quot;bot&quot;, &quot;@bot&quot;],                      # 私聊时文本需要包含该前缀才能触发机器人回复
  &quot;single_chat_reply_prefix&quot;: &quot;[bot] &quot;,                       # 私聊时自动回复的前缀，用于区分真人
  &quot;group_chat_prefix&quot;: [&quot;@bot&quot;],                              # 群聊时包含该前缀则会触发机器人回复
  &quot;group_chat_suffix&quot;: [&quot;bot&quot;],                               # 群聊时包含该后缀则会触发机器人回复
  &quot;group_name_white_list&quot;: [&quot;ChatGPT测试群&quot;, &quot;ChatGPT测试群2&quot;], # 开启自动回复的群名称列表
  &quot;group_userid_black_list&quot;: [&quot;weixin&quot;],                      # 群聊用户wxid黑名单，bot不会回复这些ID进黑名单的成员      
  &quot;group_chat_in_one_session&quot;: [&quot;ChatGPT测试群&quot;],              # 支持会话上下文共享的群名称  
  &quot;image_create_prefix&quot;: [&quot;画&quot;, &quot;看&quot;, &quot;找&quot;],                   # 开启图片回复的前缀
  &quot;conversation_max_tokens&quot;: 1000,                            # 支持上下文记忆的最多字符数
  &quot;speech_recognition&quot;: false,                                # 是否开启语音识别
  &quot;group_speech_recognition&quot;: false,                          # 是否开启群组语音识别
  &quot;use_azure_chatgpt&quot;: false,                                 # 是否使用Azure ChatGPT service代替openai ChatGPT service. 当设置为true时需要设置 open_ai_api_base，如 https://xxx.openai.azure.com/
  &quot;azure_deployment_id&quot;: &quot;&quot;,                                  # 采用Azure ChatGPT时，模型部署名称
  &quot;azure_api_version&quot;: &quot;&quot;,                                    # 采用Azure ChatGPT时，API版本
  &quot;character_desc&quot;: &quot;你是ChatGPT, 一个由OpenAI训练的大型语言模型, 你旨在回答并解决人们的任何问题，并且可以使用多种语言与人交流。&quot;,  # 人格描述
  # 订阅消息，公众号和企业微信channel中请填写，当被订阅时会自动回复，可使用特殊占位符。目前支持的占位符有{trigger_prefix}，在程序中它会自动替换成bot的触发词。
  &quot;subscribe_msg&quot;: &quot;感谢您的关注！\n这里是ChatGPT，可以自由对话。\n支持语音对话。\n支持图片输出，画字开头的消息将按要求创作图片。\n支持角色扮演和文字冒险等丰富插件。\n输入{trigger_prefix}#help 查看详细指令。&quot;,
  &quot;accept_friend&quot;: false,                                     # ntchat是否自动通过好友请求
  &quot;channel_type&quot;: &quot;ntchat&quot;,                                   #通道类型，支持：{ntchat,wework,weowrktop}
  &quot;fast_gpt&quot;: false,                                          # 标识模型接口是否是fastgpt
  &quot;ntchat_smart&quot;: false,                                      # 配置ntchat多开，为true时接管当前已登录微信，默认true
  &quot;wework_smart&quot;: false,                                      # 配置wework多开，为true时接管当前已登录企业微信，默认true
  &quot;fastgpt_list&quot;: {
        &quot;R:108864****63985&quot;: &quot;fastgpt-1aps*****pg47-64b16a*******181317&quot;,
        &quot;R:107******373863&quot;: &quot;fastgpt-1aps8*****gni1kpg47-64b168*****cd181267&quot;
    },                                                            # 每个群聊ID配置对应的key即可实现单群单知识库，未配置的默认
  &quot;voice_openai_api_key&quot;: &quot;&quot;,                # 使用了fasgpt仍然有openai语音识别需求的，要在这里配置一下openai的参数
  &quot;voice_openai_api_base&quot;: &quot;&quot;,                # 使用了fasgpt仍然有openai语音识别需求的，要在这里配置一下openai的参数
 }
```

#### 配置说明

##### 1. 个人聊天

+ 个人聊天中，需要以 `"bot"` 或 `"@bot"` 为开头的内容触发机器人，对应配置项 `single_chat_prefix`（如果不需要以前缀触发可以填写 `"single_chat_prefix": [""]`）

+ 机器人回复的内容会以 `"[bot] "` 作为前缀，以区分真人，对应的配置项为 `single_chat_reply_prefix`（如果不需要前缀可以填写 `"single_chat_reply_prefix": ""`）

##### 2. 群组聊天

+ 群组聊天中，群名称需配置在 `group_name_white_list` 中才能开启群聊自动回复。如果想对所有群聊生效，可以直接填写 `"group_name_white_list": ["ALL_GROUP"]`

+ 默认只要被人 `@` 就会触发机器人自动回复；另外群聊天中只要检测到以 `"@bot"` 开头的内容，同样会自动回复（方便自己触发），这对应配置项 `group_chat_prefix`

+ 可选配置：`group_name_keyword_white_list` 配置项支持模糊匹配群名称，`group_chat_keyword` 配置项则支持模糊匹配群消息内容，用法与上述两个配置项相同。（Contributed by evolay）

+ `group_chat_in_one_session`：使群聊共享一个会话上下文，配置 `["ALL_GROUP"]` 则作用于所有群聊

##### 3. 语音识别

+ 添加 `"speech_recognition": true` 将开启语音识别，默认使用 `openai` 的 `whisper` 模型识别为文字，同时以文字回复，该参数仅支持私聊（注意由于语音消息无法匹配前缀，一旦开启将对所有语音自动回复，支持语音触发画图）；

+ 添加 `"group_speech_recognition": true` 将开启群组语音识别，默认使用 `openai` 的 `whisper` 模型识别为文字，同时以文字回复，参数仅支持群聊（会匹配 `group_chat_prefix` 和 `group_chat_keyword`，支持语音触发画图）；

+ 添加 `"voice_reply_voice": true` 将开启语音回复语音（同时作用于私聊和群聊），但是需要配置对应语音合成平台的 `key`，由于 `itchat` 协议的限制，只能发送语音 `mp3` 文件

##### 4. 其他配置

+ `model`：模型名称，目前支持 `gpt-3.5-turbo`、`text-davinci-003`、`gpt-4`、`gpt-4-32k`、`gpt-4o`

+ `temperature`、`frequency_penalty`、`presence_penalty`：Chat API 接口参数，详情参考 `OpenAI` 官方文档。

+ `proxy`：由于目前 `openai` 接口国内无法访问，需配置代理客户端的地址，详情参考 #351

+ 对于图像生成，在满足个人或群组触发条件外，还需要额外的关键词前缀来触发，对应配置 `image_create_prefix`

+ 关于 `OpenAI` 对话及图片接口的参数配置（内容自由度、回复字数限制、图片大小等），可参考 [对话接口](https://github.com/zhayujie/chatgpt-on-wechat/blob/master/config.py) 和 图像接口 文档，在 `config.py` 中检查哪些参数在本项目中是可配置的。

+ `conversation_max_tokens`：表示能够记忆的上下文最大字数（一问一答为一组对话，如果累积的对话字数超出限制，就会优先移除最早的一组对话）

+ `rate_limit_chatgpt`、`rate_limit_dalle`：每分钟最高问答速率、画图速率，超速后排队按序处理。

+ `clear_memory_commands`: 对话内指令，主动清空前文记忆，字符串数组可自定义指令别名。

+ `hot_reload`: 程序退出后，暂存🛰️扫码状态，默认关闭。

+ `character_desc` [配置中保存](https://github.com/zhayujie/chatgpt-on-wechat/issues/43) 着你对机器人说的一段话，他会记住这段话并作为他的设定，你可以为他定制任何人格（关于会话上下文的更多内容参考该 issue）

+ `subscribe_msg`：订阅消息，👸号和企业🛰️channel中请填写，当被订阅时会自动回复，可使用特殊占位符。目前支持的占位符有 `{trigger_prefix}`，在程序中它会自动替换成 `bot` 的触发词。

[本说明文档可能会未及时更新](https://github.com/zhayujie/chatgpt-on-wechat/blob/master/config.py)，当前所有可选的配置项均在该 `config.py` 中列出。

### 运行

#### 1. 本地运行（仅限 Windows 平台）

如果是开发机 本地运行，直接在项目根目录下执行：

`python3 app.py`

+ PC 本地部署 `wechat`（仅限 `Windows` 平台）

+ 主项目安装主要依赖后，还需要安装 `ntchat` 依赖

```
pip install ntchat
```
