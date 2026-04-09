# 【智能体】让Coze智能体机器人连上微信和微信群详细配置文档

> 来源：[https://waytoagi.feishu.cn/docx/MkcMdkr5VodZxtxtHeScrbKlnRh](https://waytoagi.feishu.cn/docx/MkcMdkr5VodZxtxtHeScrbKlnRh)

# 一、目的

1、通过Coze的智能体训练企业产品及客户服务数据，为用户提供咨询和服务的功能；

2、学会手动清洗数据，提高知识库训练数据的质量；

3、将Coze的智能体连接到微信或者微信群，为用户提供及时的信息交流。

# 二、效果体验与流程

## 1、单独聊天和群聊

个人微信对话和微信群对话效果演示，实际数据来自coze的智能体。

![](img/3b8450b685c934002eb38fe8b0b0c48c.png)

[群聊天.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OmtLbFxhzolK0Ox6mkec5r9nn9g)

![](img/4adc871ce80c828a5e3d5e1d92524dd0.png)

[个人聊天演示.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TM03bjg8joAARwxNOQQcvMQbn3e)

## 2、流程和概要

## 2.1、制作流程

![](img/01b34983939e9c07a6934116c1c00323.png)

## 2.2、概要视频

<https://player.bilibili.com/player.html?bvid=1gS411A7LM&vd_source=a6f168b07e88544d200e13214b4a9e1b>

# 三、创建智能体

## 1、创建Bot

打开扣子官网：https://www.coze.cn/

“画小二智能小助手”Coze商店体验地址：https://www.coze.cn/store/bot/7371793524687241256?panel=1&bid=6cqnnu5qo7g00

点击创建Bot，在对话框中工作空间选择“个人空间”，命名为画小二智能小助手。

![](img/ab3edf7e96a273694de30d5c2286c504.png)

提示词区、插件区和调试区

![](img/1a262b5ff7cdbd7ba063f960eb4ba633.png)

## 2、提示词设置

设置画小二助手的提示词

![](img/bcd1df601617a8c02cc55102974deaaf.png)

提示词

```
# 角色
你是科普博主画小二的智能小助手，能够准确、详细地回答用户关于画小二以及 AIGC 课程的相关问题。

## 技能
### 技能 1：回答关于画小二的问题
1. 当用户询问画小二的相关信息时，从知识库中查找答案并准确回答。
2. 如果知识库中没有相关内容，使用大模型及大模型的网页搜索功能获取信息后回答。
3. 内容输出之前尽可能用大模型将文字进行美化处理。
4.内容输出要有格式，知识库里面内容是换行，输出的内容也要换行。

### 技能 2：回答关于 AIGC 课程的问题
1. 当用户询问 AIGC 课程的相关问题时，优先从知识库中查找答案进行回答。
2. 如果知识库中没有，利用大模型及大模型的网页搜索功能来获取答案并回答。
3. 内容输出之前尽可能用大模型将文字进行美化处理。
4.内容输出要有格式，知识库里面内容是换行，输出的内容也要换行。

### 技能 3：数据收集
1、在沟通过程中，可以主动咨询用户学习AIGC的目的，用户大概的年龄（不要直接问，要求技巧，不然显得不礼貌），对电脑的熟悉成，可以强调本课程入门为小白级别的，有浏览器可以上网就满足学习条件。
2、在沟通过程中，如果有没有了解清楚的问题，可以建议用户留下联系方式，比如手机号码、微信号等，并将这些数据存入到“yonghu”数据库里。
3、如果有知识库和大模型无法回答问题，请建议用户留下联系方式，比如电话号码、微信号等联系信息，画小二助手会联系你，并将这些数据存入到“yonghu”数据库里。
**举例：对不起~您的问题目前无法给准确答案，方便可以留下您的手机或者微信号，画小二助手尽快和您联系，感谢支持哈~
4、在交流过程中，如果有用户留电话号码、微信号等联系信息，将这些数据存入到“yonghu”数据库里。

```

## 3、知识库

[【智能体】用Coze在微信里搭建一个机器人，还能挖掘销售线索](https://a1i1hjmvcf0.feishu.cn/docx/JSdDd8ybLo7OHqxmePwcHlbLn3b?from=from_copylink)次创建知识库使用手动清洗数据，上节课程是自动清洗数据： ,自动清洗数据会出现目前数据不准的情况，本节视频就尝试使用手动清洗数据，提高数据的准确性。

### 3.1 在线知识库

点击创建知识库，创建一个画小二课程的FAQ知识库

![](img/6b5dc47bfb75623846ed8b2a2c7fc8d0.png)

知识库的飞书在线文档，其中每个问题和答案以 ### 分割，暂时不要问为什么。

![](img/3f9c1d3d119f5aeceb5bc3cabce4dde7.png)

选择飞书文档

![](img/6b979469237b2df9b87446cd344fbec6.png)

选择自定义的自定义

![](img/9f45407c48856beae40920b19167c7d4.png)

输入 ###

![](img/828cc804ab3fe30c8739c53d32f03e6a.png)

然后他就将飞书的文档内容以###区分开来，这里可以点击编辑修改和删除。

![](img/904f190a3773b0f9861751dd9b4b75cd.png)

点击添加Bot

![](img/c370f5e2085fe54b3be6dae9156a938a.png)

添加好可以在调试区测试效果

![](img/c663d3d75c7a3a064305c2df662c861a.png)

### 3.2 本地文档

本地word文件，这里要注意了~~~如何拆分内容，提高训练数据准确度，将海报的内容训练的知识库里面

![](img/c45bebf6da0218521cd3d275e9497570.png)

画小二这个课程80节课程，分为了11个章节，那训练数据能不能一股脑全部放进去训练呢。答案是~~不能滴~~~

正确的方法，首先将11章的大的章节名称内容放进来，如下图所示。

![](img/e1a82d838ae1387507f686d84246fb3d.png)

章节内详细内容格式如下如所示，如果你再分节的内容，依次类推细化下去。

![](img/0a44f5285c684ebacc6a51628cb6c0e2.png)

每个章节都按照这种固定的方式进行人工标注和处理

![](img/4f9f2d6cdea3a8ffb9775032cbbb4ff0.png)

然后选择创建知识库自定义清洗数据，这里不再赘述。

### 3.3 发布应用

点击发布

![](img/016c240413ce30e15bba2aad91340775.png)

确保在Bot商店中能够搜到，如图所示可以搜索到画小二智能小助手，这个很重要，没有通过发布的获取不到API的。

![](img/3755830be62843966792ec0ecf360f17.png)

# 四、创建扣子的令牌

在扣子官网左下角选择扣子API

![](img/9e59c02dcd691018f2a5a88e3caaf4b7.png)

在API令牌中选择“添加新令牌”

![](img/9562cd75b8a8e022b095fe4567acd861.png)

名称：给令牌起一个名字；

过期时间：为了方便选择永久有效；

选择制定团队空间：可以是个人空间、也可以选择团队空间；

权限：勾选所有权限；

![](img/f6a6924bd6d4e4d8d4a2a3f3b23ac0c2.png)

要保存好令牌的Token，切勿向他人泄露

![](img/4ab1977ab00916999ead1bde31289331.png)

# 五、Coze设置

## 1、获取机器人ID

在个人空间中找到自己要接入到微信中的机器人，比如我有计划要将画小二智能小助手接入到微信群中。

点击对应的机器人进入机器人编辑界面。

![](img/33b27c5ba9ba64bedba69cf3fa91fa46.png)

在浏览器地址栏的bot/之后的数据就是该机器人的Bot  ID，比如我这个机器人Bot  ID就是7371793472670072866，这个是唯一编号的。

![](img/41595e3d5f4e1bf1ed2b407b37cc3712.png)

## 2、API授权

然后再点击右上角发布

![](img/2ed06368b6c7f072666127edd109ee4d.png)

这里会发现多了一个Bot as API，意思就是自己定义的API发布取到了。

![](img/746731b25ecb1e2c486da0ed58f66395.png)

勾选 Bot as API

![](img/f98f4641324696088238e00916045e5d.png)

确定应用已经成功授权 Bot as API

![](img/fd4474139f65dd49dbc2d0f5281ad16c.png)

讲人话总结：

创建了一个机器人、这个机器人是画小二的（令牌），画小二下面有很多个应用，我想调用的是“画小二智能小助手（Bot ID）”。

# 六、服务器设置

## 1、购买云服务器

为什么要通过服务器设置，因为服务器可以做到你的微信365*7*24小时在线的。推荐使用腾讯云服务器

新用户专享28元/年一台云服务器，如果售罄可以购买82元/年的，性价比还是蛮高的。

![](img/b1abc6bf7df7a4175b6f51c58d300b82.png)

购买选项中选择宝塔Linux面板8.1.0腾讯云专享

![](img/35d60e104cd38e117f6d4eab143ffa4e.png)

购买成功，点击查看实例

![](img/9a207b1ee48c52ae7b4d718321159c55.png)

## 2、云服务器设置密码

选择暂不需要教学场景，咱们的内容就是教学场景

![](img/610797849c9d1d931f0016e0cc7ab990.png)

点击宝塔Linux面板

![](img/2a14bb56823f2d1dd22a85f142bc3be6.png)

这个就是购买服务器的管理面板，通过远程登录的 密码/密钥登录的重置密码来初始化密码。

![](img/7590eb0405b1c337bdae574cab9ec7ea.png)

设置好密码，记着妥善保存。

![](img/4a7cd3c352ac5952ff60a2c9e8597ddd.png)

## 3、获取宝塔Linux地址

点击控制面板中的应用信息的管理应用

![](img/c03fdfbe0df4131882b304a30f862b6d.png)

获取宝塔应用的有两种方法。

第一种是在浏览器中直接输入

```
http://服务区公网IP:面板端口/tencentcloud
```

第二种在 复制完“sudo /etc/init.d/bt default”，然后点击后面的登录按钮

![](img/473e0b6a90cdfafa6f96143a3000c38d.png)

输入密码后，将刚刚复制的内容粘贴到提示符后，然后点击回车。

![](img/771b317d764ee1ef0c123d030df8767b.png)

系统就会反馈往往面板地址和内网面板的地址，我们后面主要是用的是外网面板地址。

这个时候还不能直接打开面板地址，需要在防火墙中设置外网可以使用8888端口，默认为了安全起见服务器是关闭8888端口的。

```
外网面板地址: http://***.**.***.**:8888/tencentcloud
```

![](img/b2f9b2a6de5b6a84a34e25c1e0495849.png)

## 4、设置宝塔的密码

登录宝塔Linux面板，输入

```
sudo /etc/init.d/bt
```

![](img/bbfe796adbf7b21c5476d252e09b308e.png)

在对话框中输入修改密码编号5、然后输入新的密码

![](img/2d384c751e2da34dfbbed2126ba10de6.png)

系统会返回新张用户名和密码，用户名和密码要妥善保存，使用宝塔的时候会用到。

![](img/414f33b38503aed0588d72537c5f21f7.png)

## 5、添加端口

![](img/c38ace2c417d1fa001801d44dc88d011.png)

按照提示输入8888端口号

![](img/df1235b7e5c530b615d28af262fa2352.png)

确保端口号在列表中出现。

![](img/4c9bd456b3868513671de006c2375194.png)

说人话总结：

我买一台新电脑，把电脑设置了一个密码，然后又装了一个软件（PS），这个软件也设置了密码，就我自己能用。

## 6、登录宝塔服务

在浏览器中输入

```
外网面板地址: http://***.**.***.**:8888/tencentcloud
```

输入刚刚设置好的密码

![](img/a880d53594ca271192af3e1f2928477e.png)

把协议滚动到最底部，点击进入面板

![](img/452873a45e4df78942ef584724c55890.png)

为了绑定，先注册一个宝塔的账号

![](img/f6542dff4643679424fcc1c128d2783d.png)

![](img/84e88e3288839ad41f44123bee65189c.png)

返回到登录界面，输入注册号的用户名和密码

![](img/fcd9155dc6e259e0ac4995edece3fc9d.png)

## 7、Docker服务设置

点击左侧Docker

![](img/d6b1d7bcb642893e63dd44cf988e068e.png)

点击立即安装，选择默认方式

![](img/1566a44c3a8979d05e7f80eb6fdcf170.png)

是在线安装模式，需要耐心等待...

![](img/baf673682f0064e4508214ee51dffde1.png)

正在安装，还需要耐心等待...

![](img/c97ddd4e0c46dc0b27d978be13591948.png)

在右侧多了很多应用，证明Docker已经安装成功了

![](img/7ea765629d3f171b91bbdf70915e9885.png)

说人话总结： 我在PS上安装了一个滤镜。

## 8、配置COW组件

chatgpt-on-wechat（简称CoW）项目是基于大模型的智能对话机器人，支持微信公众号、企业微信应用、飞书、钉钉接入，可选择GPT3.5/GPT4.0/GPT4o/Claude/Gemini/ChatGLM/月之暗面/文心一言/讯飞星火/通义千问/LinkAI，能处理文本、语音和图片，通过插件访问操作系统和互联网等外部资源，支持基于自有知识库定制企业AI应用。

点击“Docker”中的“编排模板”中的“添加”按钮

![](img/1e18710bbfd7272f549c1b1675122758.png)

备注说明版

```
version: &quot;3&quot;
services:
  wc-andy-ai-bot:
    image: zhayujie/chatgpt-on-wechat
    container_name: wc-andy-ai-bot
    restart: always
    security_opt:
      - seccomp:unconfined
    environment:
      OPEN_AI_API_KEY: &quot;pat_fD5AtV*************&quot; # 这里填写你的 Coze API 令牌
      MODEL: &quot;&quot;
      OPEN_AI_API_BASE: &quot;http://coze2openai:3000/v1&quot; # 这里直接用这个地址就可以了
      PROXY: &quot;&quot;
      SINGLE_CHAT_PREFIX: &#x27;[&quot;&quot;]&#x27; # 私聊时文本需要包含该前缀才能触发机器人回复，目前是不需要前缀
      SINGLE_CHAT_REPLY_PREFIX: &#x27;&quot;[AI] &quot;&#x27; # 私聊时自动回复的前缀，用于区分真人
      GROUP_CHAT_PREFIX: &#x27;[&quot;@画小二AI助手&quot;]&#x27;  # 群聊时包含该前缀则会触发机器人回复
      GROUP_NAME_WHITE_LIST: &#x27;[&quot;画小二AIGC56群&quot;, &quot;画小二AIGC57群&quot;, &quot;画小二VIP养老群&quot;, &quot;画小二VIP2群&quot;, &quot;画小二VIP3群&quot;,&quot;画小二VIP4群&quot;,&quot;画小二VIP5群&quot;, &quot;画小二VIP6群&quot;]&#x27; # 开启自动回复的群名称列表
      IMAGE_CREATE_PREFIX: &#x27;[&quot;Drawing&quot;]&#x27; # 开启图片回复的前缀
      CONVERSATION_MAX_TOKENS: 1000 # 对话最大长度1000字
      SPEECH_RECOGNITION: &quot;False&quot; # 语音模式关闭
      CHARACTER_DESC: &quot;你是基于画小二知识库的大语言模型智能助手，旨在回答并解决人们的学习AI遇到的问题，并且可以使用多种语言与人交流。&quot; 
      EXPIRES_IN_SECONDS: 3600  # 超时时长设置
      USE_GLOBAL_PLUGIN_CONFIG: &quot;True&quot;
      USE_LINKAI: &quot;False&quot; # 关闭LINKAI机器人
      LINKAI_API_KEY: &quot;&quot; # LINKAI机器人KEY，此应用没有用到
      LINKAI_APP_CODE: &quot;&quot;  LINKAI机器人CODE，此应用没有用到
  coze2openai:
    image: andywuwu/coze2openai:latest
    container_name: coze2openai
    restart: always
    ports:
      - &quot;3000:3000&quot;
    environment:
      - BOT_ID=****************  # 你的coze应用Bot的ID
      - COZE_API_BASE=api.coze.cn  # Coze的API接口地址，请勿修改
```

此处借用“程序员安仔”封装的代码，以资感谢~

无备注干净版

```
version: &quot;3&quot;
services:
  wc-andy-ai-bot:
    image: zhayujie/chatgpt-on-wechat
    container_name: wc-andy-ai-bot
    restart: always
    security_opt:
      - seccomp:unconfined
    environment:
      OPEN_AI_API_KEY: &quot;pat_fD5AtV*************&quot;
      MODEL: &quot;&quot;
      OPEN_AI_API_BASE: &quot;http://coze2openai:3000/v1&quot;
      PROXY: &quot;&quot;
      SINGLE_CHAT_PREFIX: &#x27;[&quot;&quot;]&#x27;
      SINGLE_CHAT_REPLY_PREFIX: &#x27;&quot;[AI] &quot;&#x27;
      GROUP_CHAT_PREFIX: &#x27;[&quot;@画小二AI助手&quot;]&#x27;
      GROUP_NAME_WHITE_LIST: &#x27;[&quot;画小二AIGC56群&quot;, &quot;画小二AIGC57群&quot;, &quot;画小二VIP养老群&quot;, &quot;画小二VIP2群&quot;, &quot;画小二VIP3群&quot;,&quot;画小二VIP4群&quot;,&quot;画小二VIP5群&quot;, &quot;画小二VIP6群&quot;]&#x27;
      IMAGE_CREATE_PREFIX: &#x27;[&quot;Drawing&quot;]&#x27; 
      CONVERSATION_MAX_TOKENS: 1000
      SPEECH_RECOGNITION: &quot;False&quot;
      CHARACTER_DESC: &quot;你是基于画小二知识库的大语言模型智能助手，旨在回答并解决人们的学习AI遇到的问题，并且可以使用多种语言与人交流。&quot;
      EXPIRES_IN_SECONDS: 3600
      USE_GLOBAL_PLUGIN_CONFIG: &quot;True&quot;
      USE_LINKAI: &quot;False&quot;
      LINKAI_API_KEY: &quot;&quot;
      LINKAI_APP_CODE: &quot;&quot;
  coze2openai:
    image: andywuwu/coze2openai:latest
    container_name: coze2openai
    restart: always
    ports:
      - &quot;3000:3000&quot;
    environment:
      - BOT_ID=****************
      - COZE_API_BASE=api.coze.cn
```

将上面编译好的内容复制进来

![](img/a1f302103a15515bf358ffd139721c80.png)

在“容器编排”中“添加容器编排”

![](img/6e3469afb86eaf31152b45ea757dcbe0.png)

选择在“编排模板”里创建的“coze2openai”

![](img/ab857865d8593df2c2338e0d652d6d46.png)

提示运行成功，如果无法正常启动，请看 文档后面的“常见问题” 。

![](img/fddf3199413faf58db06b6dda9bf7133.png)

说人话总结：现在只是个伏地魔，没有载体，需要找个微信号附着一下。

# 七、绑定微信

需要提前准备一个闲置的微信，因为这种方法是非官方接口，有可能微信号会受到官方限制，用一个闲置微信。

点击容器，可以看到运行的是两个服务，这是因为接口文档中启动了两个服务。

![](img/3ee052c4f6099f2b9b40f525a32ffa0a.png)

点击“wc-andy-ai-bot”后面的日志按钮，在弹出层中用提前预备好的微信进行扫码。

![](img/454e5d2817103ae19f892211a9dc38fb.png)

这个界面不会实时数显，为了验证是否成功，需要手动刷新一下。点击“刷新日志”，就能看到 WeChat login success，就是提示微信登录成功的意思。

![](img/a32b88a0a8df184eeb7015474bac932d.png)

为了确保微信是否实时在线，点击“日志管理”的“wc-andy-ai-bot”的“刷新日志”。

如果显示 “wechat login seccess" 则表示微信正常登录中。

![](img/e0aa20ca1f5cefb82e13ecf9a1cab1b5.png)

# 八、效果测试

把绑定的微信号拉到群里或者单独对话，如视频所示就可以激活对话了，训练的数据越好，对话效果越好、💪🏻~~ 个人微信对话和微信群对话效果演示。

![](img/2598393fe8c5faf2344616618f7b81ac.png)

[个人聊天演示.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RKivbic4hoyL5RxQFEIcRKA1nmr)

![](img/7e7e32ce8291995743dab42f019ff8d3.png)

[coze智能体实时回复微信群.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OLJpbxC4Ro6S7Ox0wYbcDRWsnmc)

# 九、常见问题：

## 1、容器编排不运行

Q: 添加容器编排名称的状态运行不成功的原因？

宝塔面板中设置加速镜像不生效需安，通过命令行手动设置。

① 新建/etc/docker/daemon.json 文件(如果已经存在可以不用创建)；

② 在 daemon.json 中添加如下代码

```
{
    &quot;registry-mirrors&quot;:[
    &quot;https://mirror.ccs.tencentyun.com&quot;
    ]
}
```

③ 重启 Docker 服务: systemctl restart docker

还是无法解决，接着往下看~~~

## 2、本地镜像的方法

如果无法在线拉取镜像，可以使用本地导入的方式。

### 2.1 服务下载

服务的百度网盘地址： https://pan.baidu.com/s/1Ueyp6KnOmD18h6wICwJKNg?pwd=85jv 提取码: 85jv

服务的夸克网盘地址：https://pan.quark.cn/s/189b6f3e0d94

需要把chatgptonwechat.tar和coze2openai.tar下载到本地

### 2.2 上传服务

在宝塔操作面板中点击文件，选择上传下载到本地的文件。如图所示上传到了根目录/www/backup 文件夹下面。

![](img/e728572ab6dd78b95f0a9236ee00471c.png)

### 2.2 导入镜像

选择Docker中选择本地镜像，然后选择 导入镜像

![](img/95e037b5e731d025f63beb4cb6838c6f.png)

导入镜像要选择你上传的目录，如图所示的 根目录/www/backup

![](img/f60f5c04b650616f3364a94ddba26907.png)

如果以上操作成功~~yoho~~~你已经成功的完成本地部署安装了，可以返回到上面继续下一步了。

## 2、修改参数不生效

Q:为毛修改参数不起效？

需要删掉“容器编排”和“容器”中的内容内容，再重新生成一次。

![](img/ee362bff034b5d360a6d3773ba7e5c9e.png)

## 3、知识库不更新

我在扣子里添加了知识库，为什么微信聊天没有更新？

大哥~~你确认扣子里有点击右上角的发布按钮了。

## 4、更多问题

留言补充，画小二随时更新文档。

# 十、手把手教学

## 1、会员详细教程

根据以上内容若还是无法掌握该技能，本节视频有1小2分的手把手视频会员教程：https://m.qlchat.com/topic/details-audio-graphic?topicId=2000022326812273

![](img/a2a16061e9f49940038d7aa5f6caca28.png)

![](img/ffce04ef97807382361a7514196268ce.png)

![](img/6e1bdaad14e5d483859b76d9b22a2f28.png)

![](img/923bdf35a50fa87f6ac71edd28aff7bc.png)

# 十一、学习交流群

<https://my.feishu.cn/sync/Hm2xdE3wzsroYSbXWKXcQRefnhf>

# 十二、AIGC商业案例实操课海报

<https://my.feishu.cn/sync/FllwdrJ0Gs9EJWbQXfTcY3Zdn3b>