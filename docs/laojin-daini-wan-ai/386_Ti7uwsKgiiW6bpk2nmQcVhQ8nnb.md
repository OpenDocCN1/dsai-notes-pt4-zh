# ※ COW主目录Config说明

> 来源：[https://my.feishu.cn/docx/Fv2rdRMFioUWDexkB3XcVkzFnIh](https://my.feishu.cn/docx/Fv2rdRMFioUWDexkB3XcVkzFnIh)

配置截至Github中COW的官方说明：https://github.com/zhayujie/chatgpt-on-wechat/blob/master/config.py

**复制时请移除所有汉字。**

```
# 将所有可用的配置项写在字典里, 请使用小写字母
# 此处的配置值无实际意义，程序不会读取此处的配置，仅用于提示格式，请将配置加入到config.json中
available_setting = {
    # openai api配置
    &quot;open_ai_api_key&quot;: &quot;&quot;,  # openai api key
    # openai apibase，当use_azure_chatgpt为true时，需要设置对应的api base
    &quot;open_ai_api_base&quot;: &quot;https://api.openai.com/v1&quot;,
    &quot;proxy&quot;: &quot;&quot;,  # openai使用的代理
    
    # chatgpt模型， 当use_azure_chatgpt为true时，其名称为Azure上model deployment名称
    &quot;model&quot;: &quot;gpt-3.5-turbo&quot;,  # 可选择: gpt-4o, pt-4o-mini, gpt-4-turbo, claude-3-sonnet, wenxin, moonshot, qwen-turbo, xunfei, glm-4, minimax, gemini等模型，全部可选模型详见common/const.py文件
    &quot;bot_type&quot;: &quot;&quot;,  # 可选配置，使用兼容openai格式的三方服务时候，需填&quot;chatGPT&quot;。bot具体名称详见common/const.py文件列出的bot_type，如不填根据model名称判断，
    &quot;use_azure_chatgpt&quot;: False,  # 是否使用azure的chatgpt
    &quot;azure_deployment_id&quot;: &quot;&quot;,  # azure 模型部署名称
    &quot;azure_api_version&quot;: &quot;&quot;,  # azure api版本
    
    # Bot触发配置
    &quot;single_chat_prefix&quot;: [&quot;bot&quot;, &quot;@bot&quot;],  # 私聊时文本需要包含该前缀才能触发机器人回复
    &quot;single_chat_reply_prefix&quot;: &quot;[bot] &quot;,  # 私聊时自动回复的前缀，用于区分真人
    &quot;single_chat_reply_suffix&quot;: &quot;&quot;,  # 私聊时自动回复的后缀，\n 可以换行
    &quot;group_chat_prefix&quot;: [&quot;@bot&quot;],  # 群聊时包含该前缀则会触发机器人回复
    &quot;no_need_at&quot;: False,  # 群聊回复时是否不需要艾特
    &quot;group_chat_reply_prefix&quot;: &quot;&quot;,  # 群聊时自动回复的前缀
    &quot;group_chat_reply_suffix&quot;: &quot;&quot;,  # 群聊时自动回复的后缀，\n 可以换行
    &quot;group_chat_keyword&quot;: [],  # 群聊时包含该关键词则会触发机器人回复
    &quot;group_at_off&quot;: False,  # 是否关闭群聊时@bot的触发
    &quot;group_name_white_list&quot;: [&quot;ChatGPT测试群&quot;, &quot;ChatGPT测试群2&quot;],  # 开启自动回复的群名称列表
    &quot;group_name_keyword_white_list&quot;: [],  # 开启自动回复的群名称关键词列表
    &quot;group_chat_in_one_session&quot;: [&quot;ChatGPT测试群&quot;],  # 支持会话上下文共享的群名称
    &quot;nick_name_black_list&quot;: [],  # 用户昵称黑名单
    &quot;group_welcome_msg&quot;: &quot;&quot;,  # 配置新人进群固定欢迎语，不配置则使用随机风格欢迎
    &quot;trigger_by_self&quot;: False,  # 是否允许机器人触发
    &quot;text_to_image&quot;: &quot;dall-e-2&quot;,  # 图片生成模型，可选 dall-e-2, dall-e-3
    
    # Azure OpenAI dall-e-3 配置
    &quot;dalle3_image_style&quot;: &quot;vivid&quot;, # 图片生成dalle3的风格，可选有 vivid, natural
    &quot;dalle3_image_quality&quot;: &quot;hd&quot;, # 图片生成dalle3的质量，可选有 standard, hd
    # Azure OpenAI DALL-E API 配置, 当use_azure_chatgpt为true时,用于将文字回复的资源和Dall-E的资源分开.
    &quot;azure_openai_dalle_api_base&quot;: &quot;&quot;, # [可选] azure openai 用于回复图片的资源 endpoint，默认使用 open_ai_api_base
    &quot;azure_openai_dalle_api_key&quot;: &quot;&quot;, # [可选] azure openai 用于回复图片的资源 key，默认使用 open_ai_api_key
    &quot;azure_openai_dalle_deployment_id&quot;:&quot;&quot;, # [可选] azure openai 用于回复图片的资源 deployment id，默认使用 text_to_image
    &quot;image_proxy&quot;: True,  # 是否需要图片代理，国内访问LinkAI时需要
    &quot;image_create_prefix&quot;: [&quot;画&quot;, &quot;看&quot;, &quot;找&quot;],  # 开启图片回复的前缀
    &quot;concurrency_in_session&quot;: 1,  # 同一会话最多有多少条消息在处理中，大于1可能乱序
    &quot;image_create_size&quot;: &quot;256x256&quot;,  # 图片大小,可选有 256x256, 512x512, 1024x1024 (dall-e-3默认为1024x1024)
    &quot;group_chat_exit_group&quot;: False,
    
    # chatgpt会话参数
    &quot;expires_in_seconds&quot;: 3600,  # 无操作会话的过期时间
    
    # 人格描述
    &quot;character_desc&quot;: &quot;你是ChatGPT, 一个由OpenAI训练的大型语言模型, 你旨在回答并解决人们的任何问题，并且可以使用多种语言与人交流。&quot;,
    &quot;conversation_max_tokens&quot;: 1000,  # 支持上下文记忆的最多字符数
    
    # chatgpt限流配置
    &quot;rate_limit_chatgpt&quot;: 20,  # chatgpt的调用频率限制
    &quot;rate_limit_dalle&quot;: 50,  # openai dalle的调用频率限制
    
    # chatgpt api参数 参考https://platform.openai.com/docs/api-reference/chat/create
    &quot;temperature&quot;: 0.9,
    &quot;top_p&quot;: 1,
    &quot;frequency_penalty&quot;: 0,
    &quot;presence_penalty&quot;: 0,
    &quot;request_timeout&quot;: 180,  # chatgpt请求超时时间，openai接口默认设置为600，对于难问题一般需要较长时间
    &quot;timeout&quot;: 120,  # chatgpt重试超时时间，在这个时间内，将会自动重试
    
    # Baidu 文心一言参数
    &quot;baidu_wenxin_model&quot;: &quot;eb-instant&quot;,  # 默认使用ERNIE-Bot-turbo模型
    &quot;baidu_wenxin_api_key&quot;: &quot;&quot;,  # Baidu api key
    &quot;baidu_wenxin_secret_key&quot;: &quot;&quot;,  # Baidu secret key
    &quot;baidu_wenxin_prompt_enabled&quot;: False,  # Enable prompt if you are using ernie character model
    
    # 讯飞星火API
    &quot;xunfei_app_id&quot;: &quot;&quot;,  # 讯飞应用ID
    &quot;xunfei_api_key&quot;: &quot;&quot;,  # 讯飞 API key
    &quot;xunfei_api_secret&quot;: &quot;&quot;,  # 讯飞 API secret
    &quot;xunfei_domain&quot;: &quot;&quot;,  # 讯飞模型对应的domain参数，Spark4.0 Ultra为 4.0Ultra，其他模型详见: https://www.xfyun.cn/doc/spark/Web.html
    &quot;xunfei_spark_url&quot;: &quot;&quot;,  # 讯飞模型对应的请求地址，Spark4.0 Ultra为 wss://spark-api.xf-yun.com/v4.0/chat，其他模型参考详见: https://www.xfyun.cn/doc/spark/Web.html
    
    # claude 配置
    &quot;claude_api_cookie&quot;: &quot;&quot;,
    &quot;claude_uuid&quot;: &quot;&quot;,
    
    # claude api key
    &quot;claude_api_key&quot;: &quot;&quot;,
    
    # 通义千问API, 获取方式查看文档 https://help.aliyun.com/document_detail/2587494.html
    &quot;qwen_access_key_id&quot;: &quot;&quot;,
    &quot;qwen_access_key_secret&quot;: &quot;&quot;,
    &quot;qwen_agent_key&quot;: &quot;&quot;,
    &quot;qwen_app_id&quot;: &quot;&quot;,
    &quot;qwen_node_id&quot;: &quot;&quot;,  # 流程编排模型用到的id，如果没有用到qwen_node_id，请务必保持为空字符串
    
    # 阿里灵积(通义新版sdk)模型api key
    &quot;dashscope_api_key&quot;: &quot;&quot;,
    # Google Gemini Api Key
    &quot;gemini_api_key&quot;: &quot;&quot;,
    
    # wework的通用配置
    &quot;wework_smart&quot;: True,  # 配置wework是否使用已登录的企业微信，False为多开
   
    # 语音设置
    &quot;speech_recognition&quot;: True,  # 是否开启语音识别
    &quot;group_speech_recognition&quot;: False,  # 是否开启群组语音识别
    &quot;voice_reply_voice&quot;: False,  # 是否使用语音回复语音，需要设置对应语音合成引擎的api key
    &quot;always_reply_voice&quot;: False,  # 是否一直使用语音回复
    &quot;voice_to_text&quot;: &quot;openai&quot;,  # 语音识别引擎，支持openai,baidu,google,azure,xunfei,ali
    &quot;text_to_voice&quot;: &quot;openai&quot;,  # 语音合成引擎，支持openai,baidu,google,azure,xunfei,ali,pytts(offline),elevenlabs,edge(online)
    &quot;text_to_voice_model&quot;: &quot;tts-1&quot;,
    &quot;tts_voice_id&quot;: &quot;alloy&quot;,
  
    # baidu 语音api配置， 使用百度语音识别和语音合成时需要
    &quot;baidu_app_id&quot;: &quot;&quot;,
    &quot;baidu_api_key&quot;: &quot;&quot;,
    &quot;baidu_secret_key&quot;: &quot;&quot;,
 
    # 1536普通话(支持简单的英文识别) 1737英语 1637粤语 1837四川话 1936普通话远场
    &quot;baidu_dev_pid&quot;: 1536,
  
    # azure 语音api配置， 使用azure语音识别和语音合成时需要
    &quot;azure_voice_api_key&quot;: &quot;&quot;,
    &quot;azure_voice_region&quot;: &quot;japaneast&quot;,
  
    # elevenlabs 语音api配置
    &quot;xi_api_key&quot;: &quot;&quot;,  # 获取ap的方法可以参考https://docs.elevenlabs.io/api-reference/quick-start/authentication
    &quot;xi_voice_id&quot;: &quot;&quot;,  # ElevenLabs提供了9种英式、美式等英语发音id，分别是“Adam/Antoni/Arnold/Bella/Domi/Elli/Josh/Rachel/Sam”
  
    # 服务时间限制，目前支持itchat
    &quot;chat_time_module&quot;: False,  # 是否开启服务时间限制
    &quot;chat_start_time&quot;: &quot;00:00&quot;,  # 服务开始时间
    &quot;chat_stop_time&quot;: &quot;24:00&quot;,  # 服务结束时间

    # 翻译api
    &quot;translate&quot;: &quot;baidu&quot;,  # 翻译api，支持baidu
 
    # baidu翻译api的配置
    &quot;baidu_translate_app_id&quot;: &quot;&quot;,  # 百度翻译api的appid
    &quot;baidu_translate_app_key&quot;: &quot;&quot;,  # 百度翻译api的秘钥

    # itchat的配置
    &quot;hot_reload&quot;: False,  # 是否开启热重载

    # wechaty的配置
    &quot;wechaty_puppet_service_token&quot;: &quot;&quot;,  # wechaty的token

    # wechatmp的配置
    &quot;wechatmp_token&quot;: &quot;&quot;,  # 微信公众平台的Token
    &quot;wechatmp_port&quot;: 8080,  # 微信公众平台的端口,需要端口转发到80或443
    &quot;wechatmp_app_id&quot;: &quot;&quot;,  # 微信公众平台的appID
    &quot;wechatmp_app_secret&quot;: &quot;&quot;,  # 微信公众平台的appsecret
    &quot;wechatmp_aes_key&quot;: &quot;&quot;,  # 微信公众平台的EncodingAESKey，加密模式需要

    # wechatcom的通用配置
    &quot;wechatcom_corp_id&quot;: &quot;&quot;,  # 企业微信公司的corpID
 
    # wechatcomapp的配置
    &quot;wechatcomapp_token&quot;: &quot;&quot;,  # 企业微信app的token
    &quot;wechatcomapp_port&quot;: 9898,  # 企业微信app的服务端口,不需要端口转发
    &quot;wechatcomapp_secret&quot;: &quot;&quot;,  # 企业微信app的secret
    &quot;wechatcomapp_agent_id&quot;: &quot;&quot;,  # 企业微信app的agent_id
    &quot;wechatcomapp_aes_key&quot;: &quot;&quot;,  # 企业微信app的aes_key

    # 飞书配置
    &quot;feishu_port&quot;: 80,  # 飞书bot监听端口
    &quot;feishu_app_id&quot;: &quot;&quot;,  # 飞书机器人应用APP Id
    &quot;feishu_app_secret&quot;: &quot;&quot;,  # 飞书机器人APP secret
    &quot;feishu_token&quot;: &quot;&quot;,  # 飞书 verification token
    &quot;feishu_bot_name&quot;: &quot;&quot;,  # 飞书机器人的名字

    # 钉钉配置
    &quot;dingtalk_client_id&quot;: &quot;&quot;,  # 钉钉机器人Client ID 
    &quot;dingtalk_client_secret&quot;: &quot;&quot;,  # 钉钉机器人Client Secret
    &quot;dingtalk_card_enabled&quot;: False,
    
    # chatgpt指令自定义触发词
    &quot;clear_memory_commands&quot;: [&quot;#清除记忆&quot;],  # 重置会话指令，必须以#开头
 
    # channel配置
    &quot;channel_type&quot;: &quot;&quot;,  # 通道类型，支持：{wx,wxy,terminal,wechatmp,wechatmp_service,wechatcom_app,dingtalk}
    &quot;subscribe_msg&quot;: &quot;&quot;,  # 订阅消息, 支持: wechatmp, wechatmp_service, wechatcom_app
    &quot;debug&quot;: False,  # 是否开启debug模式，开启后会打印更多日志
    &quot;appdata_dir&quot;: &quot;&quot;,  # 数据目录

    # 插件配置
    &quot;plugin_trigger_prefix&quot;: &quot;$&quot;,  # 规范插件提供聊天相关指令的前缀，建议不要和管理员指令前缀&quot;#&quot;冲突
 
    # 是否使用全局插件配置
    &quot;use_global_plugin_config&quot;: False,
    &quot;max_media_send_count&quot;: 3,  # 单次最大发送媒体资源的个数
    &quot;media_send_interval&quot;: 1,  # 发送图片的事件间隔，单位秒
  
    # 智谱AI 平台配置
    &quot;zhipu_ai_api_key&quot;: &quot;&quot;,
    &quot;zhipu_ai_api_base&quot;: &quot;https://open.bigmodel.cn/api/paas/v4&quot;,
    &quot;moonshot_api_key&quot;: &quot;&quot;,
    &quot;moonshot_base_url&quot;: &quot;https://api.moonshot.cn/v1/chat/completions&quot;,
 
    # LinkAI平台配置
    &quot;use_linkai&quot;: False,
    &quot;linkai_api_key&quot;: &quot;&quot;,
    &quot;linkai_app_code&quot;: &quot;&quot;,
    &quot;linkai_api_base&quot;: &quot;https://api.link-ai.tech&quot;,  # linkAI服务地址
    &quot;Minimax_api_key&quot;: &quot;&quot;,
    &quot;Minimax_group_id&quot;: &quot;&quot;,
    &quot;Minimax_base_url&quot;: &quot;&quot;,
    &quot;web_port&quot;: 9899,
```

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>