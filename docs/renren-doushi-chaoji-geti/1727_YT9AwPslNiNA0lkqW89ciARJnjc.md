
# 2023-10-26人人都能学会的 ChatGPT + 🛰️机器人部署教程

> 来源：https://xuqiwei1986.feishu.cn/docx/E9qBdFRw7o8abQx7ETccoDhFnrf  
> 🔗 原文链接：https://articles.zsxq.com/id_o5hg4vw48weg.html

大家好，我是苏小瓶，一名 AI 工程师，正在职场搞副业，努力完成自己的闭环之路。今天分享的是实操课程，手把手带你做一个AI🛰️客服（助手）。

最近接二连三有朋友问我，有没有🛰️小程序可以自动回复的，还能收集资料，要是语音输入也能回复就好了。

这个 ChatGPT + 🛰️，还能写诗，还能作画，功能简直不要太齐全。完完全全可以作为私人助理了！

![](img/9c3e7c589e3b13669da11381d2fc46a0.png)

![](img/7f4bc3964cfbade5b04753d75990c79c.png)

## 资源&环境

github上有非常多的开源代码，而且操作非常简单，自己动手基本都能搞定。

这里选择第一个：`https://github.com/zhayujie/chatgpt-on-wechat`

![](img/06bbbd9ea6f4969e0c483c51bcb6977e.png)

下载源码，开源直接下载，也可以使用命令下载：

`git clone https://github.com/zhayujie/chatgpt-on-wechat`

![](img/064b04333cb859fb44624465ab0fa0df.png)

这是下载完成后的情况。

接着安装环境，本人习惯使用conda

`conda create -n wechat python=3.8 #创建专属 🛰️ChatGPT环境`

`source activate wechat #进入该环境中`

接下来就是 安装 `chatgpt-on-wechat` 目录下 `requirements.txt` 文件提前准备好的依赖安装包，执行上面命令后，默认会一个一个帮我安装在本地。这几个依赖包一定要安装好，不然 ChatGPT 🛰️机器人程序跑不起来。

`pip3 install -r requirements.txt`

和上面的差不多的意思，还是继续安装 ChatGPT 🛰️机器人程序会要用到的程序。

`pip3 install -r requirements-optional.txt`

如果你需要你的 ChatGPT 机器人具备语音识别能力的话，需要安装下面这个安装包。

`pip3 install azure-cognitiveservices-speech`

以上，基本环境搭建完成！，接下来就要见证奇迹的时刻了。看看🛰️ChatGPT 如何动起来！

![](img/f24b3cefec16d025024e14c8a97b7acd.png)

## 配置

错了，重来！

忘记配置这回事了！

### 一）ChatGPT API key 获取

🛰️ ChatGPT 调用的其实是 ChatGPT API 的接口，因此，我们需要去获取API keys。

首先，需要注册个 ChatGPT 账号，如果你不会，来找我，手把手教你！接着就去获取 API keys。

获取网址是：`https://platform.openai.com/account/api-keys`

![](img/b0bc97a643e538c30df06cbbb5704ab6.png)

### 二）配置表说明

配置文件的模板在根目录的 `config-template.json` 中，需复制该模板创建最终生效的 `config.json` 文件：

`cp config-template.json config.json`

配置详情如下表：

```json
{
 "open_ai_api_key": "YOUR API KEY", # 填入上面创建的 OpenAI API KEY
 "model": "gpt-3.5-turbo", # 模型名称。当use_azure_chatgpt为 true 时，其名称为Azure上model deployment名称
 "proxy": "127.0.0.1:7890", # 代理客户端的ip和端口
 "single_chat_prefix": ["bot", "@bot"], # 私聊时文本需要包含该前缀才能触发机器人回复
 "single_chat_reply_prefix": "[bot] ", # 私聊时自动回复的前缀，用于区分真人
 "group_chat_prefix": ["@bot"], # 群聊时包含该前缀则会触发机器人回复
 "group_name_white_list": ["ChatGPT测试群", "ChatGPT测试群2"], # 开启自动回复的群名称列表
 "group_chat_in_one_session": ["ChatGPT测试群"], # 支持会话上下文共享的群名称
 "image_create_prefix": ["画", "看", "找"], # 开启图片回复的前缀
 "conversation_max_tokens": 1000, # 支持上下文记忆的最多字符数
 "speech_recognition": false, # 是否开启语音识别
 "group_speech_recognition": false, # 是否开启群组语音识别
 "use_azure_chatgpt": false, # 是否使用Azure ChatGPT service代替openai ChatGPT service. 当设置为 true 时需要设置 open_ai_api_base，如 https://xxx.openai.azure.com/
 "azure_deployment_id": "", # 采用Azure ChatGPT时，模型部署名称
 "character_desc": "你是ChatGPT, 一个由OpenAI训练的大型语言模型, 你旨在回答并解决人们的任何问题，并且可以使用多种语言与人交流。", # 人格描述 # 订阅消息，👸号和企业🛰️channel中请填写，当被订阅时会自动回复，可使用特殊占位符。目前支持的占位符有{trigger_prefix}，在程序中它会自动替换成bot的触发词。
 "subscribe_msg": "感谢您的关注！\n这里是ChatGPT，可以自由对话。\n支持语音对话。\n支持图片输出，画字开头的消息将按要求创作图片。\n支持角色扮演和文字冒险等丰富插件。\n输入{trigger_prefix}#help 查看详细指令。",
 "use_linkai": true, "linkai_api_key": "", "linkai_app_code": ""
}
```

其中 `open_ai_api_key` 是前面获取的 API keys;

个人聊天中，需要以 `"bot"`或`"@bot"` 为开头的内容触发机器人，对应配置项 `single_chat_prefix` ;

机器人回复的内容会以 `" [bot] "` 作为前缀， 以区分真人，对应的配置项为 `single_chat_reply_prefix` ;

群组聊天中，群名称需配置在 `group_name_white_list` 中才能开启群聊自动回复；

以上标红的内容都可以根据需要进行更改。

以下是针对绘画需求的配置表：

```json
{
"group_app_map": { # 群聊 和 应用编码 的映射关系
"测试群名称1": "default", # 表示在名称为 "测试群名称1" 的群聊中将使用app_code 为 default 的应用
"测试群名称2": "Kv2fXJcH"
},
"midjourney": {
"enabled": true, # midjourney 绘画开关
"auto_translate": true, # 是否自动将提示词翻译为英文
"img_proxy": true, # 是否对生成的图片使用代理，如果你是国外服务器，将这一项设置为false会获得更快的生成速度
"max_tasks": 3, # 支持同时提交的总任务个数
"max_tasks_per_user": 1, # 支持单个用户同时提交的任务个数
"use_image_create_prefix": true # 是否使用全局的绘画触发词，如果开启将同时支持由`config.json`中的 image_create_prefix 配置触发
},
"summary": {
"enabled": true, # 文档总结和对话功能开关
"group_enabled": true, # 是否支持群聊开启
"max_file_size": 5000 # 文件的大小限制，单位KB，默认为5M，超过该大小直接忽略
}
}
```

注意：如果你无法使用ChatGPT, 那也可以直接使用 LinkAI 。

### 三）LinkAI 配置

LinkAI API key获取如下：

创建API Key地址：`https://chat.link-ai.tech/app/UpuwJTmj?share=bCLsDR`

![](img/04d5445149142907b00419dcb611b5f0.png)

![](img/e41b23eab0fe10aacd7a5fb2dc399214.png)

![](img/9840b0cf2211f2c69614d99a25dca74c.png)

![](img/cd26ba90b54c563d0f9f9c5f7e813930.png)

## 运行

修改完配置文件后，我们运行下面命令，把我们的 ChatGPT 🛰️机器人项目在本地跑起来，也就是 `chatgpt-on-wechat` 这个项目。

`python3 app.py`

[或命令：`nohup python app.py &` #后台运行程序]

![](img/dc604597725cd003fcc68208ec161e65.png)

到此程序部署完成。用我们的个人🛰️，去扫描这个二维码，此时你的🛰️就变成了 ChatGPT 机器人了，命令行也是能接收到你的登录信息，以及消息的接收和发送都能看到，还挺有意思的。

看看效果：

![](img/ccd2e66fc4e59c3e8234eedf7ab5ae06.png)

![](img/707be6fe55cd864e55ad75e604d9b759.png)

![](img/828a39bb750d07553511c711326a7dd7.png)

![](img/8c7be5482d82e33740a75a8f021c7ad8.png)

以上，有启发点个赞哈，跟着实操了留个评论呀

![](img/96803fb9826c33eadb4ec2804a5f4cfb.png)

扫码加入星球

查看更多优质内容

`https://wx.zsxq.com/mweb/views/joingroup/join_group.html?group_id=15552545485212`
