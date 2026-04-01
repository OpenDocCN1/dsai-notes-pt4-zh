# Voice - 语音识别与发送语音

> 来源：[https://my.feishu.cn/docx/HlNKdOE2coz0vAxt4rtcVYEunzg](https://my.feishu.cn/docx/HlNKdOE2coz0vAxt4rtcVYEunzg)

![](img/cf83c3a1732b74d388264afe2e831ddf.png)

## 因cow是基于itchat的web协议制作，发送语音是.mp3文件，而非语音条

# 1、音频处理工具 - ffmpeg

点击地址，进行安装[音频 - ffmpeg安装](https://tffyvtlai4.feishu.cn/wiki/ZxcowYub6iE5GNkEsUjcW9vznMf)

# 2、讯飞APIkey申请

对比来对比去，觉得讯飞的最合适。

免费一年，而且量很大，足够用。

在讯飞上申请一个API Key。

https://www.xfyun.cn/services/rtasr

进入控制台

![](img/c1b69f682527da23b46e5daa46a4ef3d.png)

创建应用

![](img/b27f8c7cf8ce849d534ee5638505c832.png)

这里这3个就是要用到的Key。

![](img/3e98060a38f5bff6707082a4083c77a9.png)

# 3、配置Cow

**打开根目录的confi**g.json，这里是总开关，复制时请移除所有汉字。

```
# config.json文件内容示例
{
  &quot;model&quot;: &quot;gpt-3.5-turbo&quot;,                                   # 模型名称, 支持 gpt-3.5-turbo, gpt-4, gpt-4-turbo, wenxin, xunfei, glm-4, claude-3-haiku, moonshot
  &quot;open_ai_api_key&quot;: &quot;YOUR API KEY&quot;,                          # 如果使用openAI模型则填入上面创建的 OpenAI API KEY
  &quot;proxy&quot;: &quot;&quot;,                                                # 代理客户端的ip和端口，国内环境开启代理的需要填写该项，如 &quot;127.0.0.1:7890&quot;
  &quot;single_chat_prefix&quot;: [&quot;bot&quot;, &quot;@bot&quot;],                      # 私聊时文本需要包含该前缀才能触发机器人回复
  &quot;single_chat_reply_prefix&quot;: &quot;[bot] &quot;,                       # 私聊时自动回复的前缀，用于区分真人
  &quot;group_chat_prefix&quot;: [&quot;@bot&quot;],                              # 群聊时包含该前缀则会触发机器人回复
  &quot;group_name_white_list&quot;: [&quot;ChatGPT测试群&quot;, &quot;ChatGPT测试群2&quot;], # 开启自动回复的群名称列表
  &quot;group_chat_in_one_session&quot;: [&quot;ChatGPT测试群&quot;],              # 支持会话上下文共享的群名称  
  &quot;image_create_prefix&quot;: [&quot;画&quot;, &quot;看&quot;, &quot;找&quot;],                   # 开启图片回复的前缀
  &quot;conversation_max_tokens&quot;: 1000,                            # 支持上下文记忆的最多字符数
  &quot;voice_to_text&quot;: &quot;xunfei&quot;，                                 # 语音转文字模型
  &quot;text_to_voice&quot;: &quot;xunfei&quot;，                                 # 文字转语音模型
  &quot;speech_recognition&quot;: false,                                # 是否开启语音识别
  &quot;group_speech_recognition&quot;: false,                          # 是否开启群组语音识别
  &quot;voice_reply_voice&quot;: false,                                 # 是否使用语音回复语音
  &quot;character_desc&quot;: &quot;你是基于大语言模型的AI智能助手，旨在回答并解决人们的任何问题，并且可以使用多种语言与人交流。&quot;,  # 人格描述
  # 订阅消息，公众号和企业微信channel中请填写，当被订阅时会自动回复，可使用特殊占位符。目前支持的占位符有{trigger_prefix}，在程序中它会自动替换成bot的触发词。
  &quot;subscribe_msg&quot;: &quot;感谢您的关注！\n这里是ChatGPT，可以自由对话。\n支持语音对话。\n支持图片输出，画字开头的消息将按要求创作图片。\n支持角色扮演和文字冒险等丰富插件。\n输入{trigger_prefix}#help 查看详细指令。&quot;,
  &quot;use_linkai&quot;: false,                                        # 是否使用LinkAI接口，默认关闭，开启后可国内访问，使用知识库和MJ
  &quot;linkai_api_key&quot;: &quot;&quot;,                                       # LinkAI Api Key
  &quot;linkai_app_code&quot;: &quot;&quot;                                       # LinkAI 应用或工作流code
}
```

按需配置，开启是true，关闭是false

因发送语音不能是语音条，所以我只开了语音识别。

配置讯飞的key，进入/chatgpt-on-wechat/voice/xunfei下的config.json，把对应的key配置进来即可。

![](img/e334c7b57de6f5543cc7fba303f7b0bd.png)

配置完成，重新扫码登录。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>