
# Coze.cn（国内版）

> 来源：[https://my.feishu.cn/docx/BLHNdAOvzoBJuWx1uz8cIsfRnje](https://my.feishu.cn/docx/BLHNdAOvzoBJuWx1uz8cIsfRnje)

## 由于Coze.com已收费（2024/7/3），补充Coze.cn教程。

## 本文在操作代码时，注意缩进和符号

## 本教程使用前提

本教程是在初级🛰️机器人教程的基础上，使其拥有目前最强大能力的升级篇。如果你对基础还不了解，[请移步人人都可以拥有的贾维斯，手把手教你怎么拥有一个初级🛰️机器人！（点击进入）](https://tffyvtlai4.feishu.cn/wiki/AS5pw7JOQi9lHWk56pucqjkinSc)

初级教程关键点总结：

1、申请LLM（语言大模型）的API（接口）

2、注册云服务器（就是让它挂在网络上，不开启随身设备它也能自己运行）

3、配置环境（云服务器的配置）

4、通过COW开源项目，配置🛰️机器人

请确认以上内容都已经学会并完成。

## 教程正文详情

### 一、替换文件

1、`/root/chatgpt-on-wechat`路径下，打开`config.py`文件。

找到`config`类，在`}`前面增加`Coze`配置：

```
# Coze平台配置
"model": "coze",
"coze_api_base": "https://api.coze.cn/open_api/v2",
"coze_api_key": "",
"coze_bot_id": "",
```

![](img/b8b47408e5dae66faaeb7d7b7af138d0.png)

2、`/root/chatgpt-on-wechat/bot`路径下创建一个新文件夹，命名为`bytedance`

`/root/chatgpt-on-wechat/bot/bytedance`路径下，上传`bytedance_coze_bot.py`文件

![](img/dc4f2be42be658adade3856b9476b88d.png)

[`bytedance_coze_bot.py`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FTI1bMS0BoVB1uxyVZTcG8qBnXd)

3、`/root/chatgpt-on-wechat/bot`文件夹下，打开`bot_factory.py`文件。

最下方添加`Coze`配置：

```
elif bot_type == const.COZE:
    from bot.bytedance.bytedance_coze_bot import ByteDanceCozeBot
    return ByteDanceCozeBot()
```

![](img/0107ccceb2e2b726ff249be9584aafc3.png)

4、`/root/chatgpt-on-wechat/common`文件夹下，打开`const.py`文件

`bot_type`中添加`coze`，`COZE = "coze"`，

![](img/e59509290f88eaf20a107c9397832202.png)

`MODEL_LIST`中添加`coze`

![](img/05427d9078e165035562182a7b927e2e.png)

5、`/root/chatgpt-on-wechat/bridge`下，打开`bridge.py`文件

插入`Coze`配置：

```
if model_type in [const.COZE]:
    self.btype["chat"] = const.COZE
```

![](img/bc7a03c3e9f65abe53fbc184f41e0581.png)

### 二、修改配置

路径`/root/chatgpt-on-wechat/`下，打开`config.json`文件进行修改，下面这4行，除了`model`外，其他3行需要手动补充。

+   [※ `COW`主目录Config说明](https://tffyvtlai4.feishu.cn/wiki/Ti7uwsKgiiW6bpk2nmQcVhQ8nnb)如果有报错，可能是更新导致，请查看是否缺少配置。

```
{
  "channel_type": "wx",
  "model": "coze",
  "coze_api_base": "https://api.coze.cn/open_api/v2",
  "coze_api_key": "这里改成你的coze key",
  "coze_bot_id": "这里是你的botid",
  "text_to_image": "dall-e-3",
  "voice_to_text": "openai",
  "text_to_voice": "openai",
  "proxy": "",
  "hot_reload": false,
  "single_chat_prefix": [""],
  "single_chat_reply_prefix": "",
  "group_chat_keyword": ["@机器人大军"],
  "group_chat_prefix": [],
  "group_name_white_list": ["ALL_GROUP"],
  "concurrency_in_session": 1,
  "group_welcome_msg": "",
  "speech_recognition": true,
  "group_speech_recognition": false,
  "voice_reply_voice": false,
  "conversation_max_tokens": 2000,
  "expires_in_seconds": 3600,
  "character_desc": "",
  "temperature": 0.5,
  "subscribe_msg": "",
  "use_linkai": false,
  "linkai_api_key": "",
  "linkai_app_code": ""
}
```

### 三、获取key和id

进入官网：`https://www.coze.cn/home`

#### 1、获取Key

![](img/db78d695c893e1d8ab26342e84782f55.png)

点击`API令牌`

![](img/a1427da2ccb76b561a3cd7dfb6f8a644.png)

这里权限需要说明一下，如果未发布过任何Bot，是不会出现权限选项的。发布后，再回来选上。

![](img/c31e2cdb4ce5c7f7a33c9b959ff07c94.png)

![](img/6e59bd987d5948ba76dbc628e2f0c060.png)

获取到的这个令牌，就是`"coze_api_key"`，像下方一样填入即可。

`"coze_api_key": "pat_diajdkasjdlksajdlksajdasdjlkasjdlas",`

#### 2、获取Bot ID

创建Bot，填写名字，点击确认

![](img/805fb16488501a5df86310091a516a3c.png)

编排后，直接发布。

![](img/785aae0a7c590669748d1cb2943838c0.png)

直接点击跳过。

![](img/01762a26c914205ae23015d58671a650.png)

选择Bot商店和API，然后点击右上角的发布。

![](img/dd4d538dfa1e27e26811a123b5485d3c.png)

找到BOT ID。

![](img/bd4b2fcb6283c64c1085800eb691f1f4.png)

把`bot_id`填入：

`"coze_bot_id": "86787989080",`

发布是否成功，在Bot编辑页面查看。

![](img/36836e91e38d00a2a3db79872b745944.png)

### 四、完成配置并启动

返回[第二步](https://tffyvtlai4.feishu.cn/wiki/LnxAwrDGUi8h6Ykg5L6cY8d2nHh#J2vadFt8go2pazxRUy1c2pahn0f)，配置`coze_api_key`和`coze_bot_id`，然后扫码启动即可。

`https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc`
