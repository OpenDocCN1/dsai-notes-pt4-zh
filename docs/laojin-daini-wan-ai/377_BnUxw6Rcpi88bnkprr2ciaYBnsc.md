
# WaytoAGI 机器人活动共创篇

> 来源：[https://my.feishu.cn/docx/GghGdhpUCo09Bfxq5mJcp5XinEg](https://my.feishu.cn/docx/GghGdhpUCo09Bfxq5mJcp5XinEg)

为了良好的参与《谁是人类？》比赛，请各位参赛选手把自己的机器人按照如下方式调整，避免出现马脚。

## 1、如何关闭进群欢迎语

实现群中新进人时，没有欢迎语。

A、管理员认证 `#auth 【你的密码】` - 密码请查看`Plugins/godcmd/config.json`

B、`#disablep hello`，关闭`hello`插件。

C、`#enablep hello`，赛后如需重启，使用该指令。

![](img/4ece362e85ca72ab002a44e8a4f5b53e.png)

## 2、如何在群里回答时不 @对方

私聊🛰️机器人第一行，出现安装成功后，输入第二行。

```
#installp https://github.com/wangxyd/ipartment.git
#scanp
```

如果出现安装失败，则在插件目录下，右键删除后，重新安装即可。

![](img/5b8b6c44c69fc84094c07c45c1126f40.png)

配置修改：

配置文件为`plugins/ipartment/config.json`，可以自行修改，示例如下：

```
{
  "group_at_probability": 0,
  "add_quoter_nickname": false,
  "reply_reference_query": true
}
```

在以上配置项中：

+ `group_at_probability`: 设置群聊中机器人回复消息时@对方的概率。取值范围0~1，0每次都不@对方，1每次都@对方；

+ `add_quoter_nickname`: 是否开启识别对方身份的功能。值为`true`时会将对方的🛰️昵称添加到对话内容中，以便机器人识别对方的身份并做出有针对性的回答；

+ `reply_reference_query`: 是否开启优化引用消息的功能。值为`true`时会告诉机器人，对话内容中哪部分是引用的消息，哪部分是新的问题。

`备注`

`chatgpt-on-wechat`默认忽略引用的消息，需要修改源代码才会响应引用的消息，然后才可以开启优化引用消息的功能。

需要修改的源代码文件为`channel/chat_channel.py`，注释掉或者直接删除下图中红色方框中的4行代码，然后重启`chatgpt-on-wechat`即可：

![](img/9a9ef09fcbd4bbe161ccddbfa965dcbb.png)

## 3、如何修改`config.json`中的触发词为“问题”

找到配置文件，路径如下：

![](img/9055ef5ecdce3a9ef66232d2b3690c87.png)

修改`"group_chat_prefix": ["问题"]`，注意符号均为英文符号。

![](img/fdf749bf9f55846d7a4cf3a470ddbcfd.png)

达成效果：

![](img/ebf1db49103e9d302b0aee079e44be56.png)

以上全部内容完成后，重新扫码登录🛰️机器人即可。

---

以上分享来自 --- 金永勋。

如果想要看其他内容，可联系我加AI知识分享群。

![](img/258e7891bbad7433045858411be581f2.png)
