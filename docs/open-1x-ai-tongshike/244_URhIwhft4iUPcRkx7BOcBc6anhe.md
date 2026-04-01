# 零代码动手创建ModelScope Agent

> 来源：[https://datawhaler.feishu.cn/docx/BLG6d50C1oPppbxhgqBceUHfnZe](https://datawhaler.feishu.cn/docx/BLG6d50C1oPppbxhgqBceUHfnZe)

> 目前Agent创建项目已有一定更新，界面可能存在差异，但整体步骤没有大变动

> 另外魔搭创空间已支持直接进行Agent应用创建，本教程可能存在一定的滞后，期待你能一起共创更新

> 有意维护可联系 （wx：aomferni）

### [创建通义千问API](https://dashscope.console.aliyun.com/overview) (新注册用户有一定的限时免费额度)

<https://datawhaler.feishu.cn/sync/FpUmdMXkAs2mg3bFmeacI69kndg>

### 使用 [魔搭Agent创建器](https://modelscope.cn/studios/modelscope/AgentFabric/summary) 快速创建Agent

打开魔搭社区的 [AgentFabric](https://modelscope.cn/studios/modelscope/AgentFabric/summary)，或者 [创空间-交互式创建应用](https://modelscope.cn/studios/agent)

![](img/5819bde69ccf6dbf437772d6099fa1c7.png)

![](img/a7dceebbc868de81fc9f756c810f2c8b.png)

左边点开【create】是一个聊天界面，只需打字就能零代码创建一个Agent，右边是即时体验界面

#### 创建天气小助手

1.  **在左侧聊天框中发送你想要的功能**

这里我给出一个 设计天气查询Agent 的example

```
我想做一个能查询天气的agent，名字就叫“Datawhale&amp;Modelscopeの天気予報”，要求能查询我指定城市今天的天气情况和温度信息，并画一幅图向我形象的展示出来。
```

可以看到，它在回复的同时，多了2行字，分别是

“✅Config Updated!

✅Logo Updated!    ”

这是在调整Agent的配置，以及logo，可以在右边查看到具体的修改。

此外它还会反问一些问题，比如城市和展示风格等。

可以通过回复来进行进一步的配置自动调整，但也可以不予理会，右边可以实时体验调整后的效果。

1.  **检查配置&提供Tool**

点进左上角的【Configure】可以查看 Agent 的配置，其中的字段均可任意修改做Prompt Enginerring

下方的【Capabilities】是这个Agent可调用的Tool，目前可用的内置Tools有限，可按需勾选

此处勾选了用于画画的“Wanx Image Generation”，和用于查天气的“高德天气”

![](img/d7a794e118488b080fb9b587910fd5c4.png)

1.  体验Agent，做Prompt Engineering

1.  右边的聊天框里随时可以体验 Agent，可自由根据实施效果进行Prompt修改，但修改后记得点击【Update Configuration】更新配置，否则效果不会有变化

1.  还可以 点开▶，查看调用详情

![](img/fe19e7fd7f57c769bb20f6e22a56134d.png)

1.  **Agent能力体现**：将指令自动拆分成了“天气查询”和“画图”，并成功的调用了对应的工具完成任务

#### 创建宠物模拟器

> Prompt：我想做一个 宠物模拟器，让用户可以在线给自己选一只宠物，进行起名和进行喂养、玩游戏、出门遛弯等行动，并每次都能输出多张合适的图片，并在完成5次操作后，生成markdown文档记录这些行动

![](img/fe22d2c1439f23f0858674919189fe95.png)

![](img/b3729fbd55afba6ca537eaa7b0002813.png)

![](img/c4a9253856b107ddc4c769517e621e60.png)

![](img/0d80ccfcdfb64fec06bbc0acaab7fdcf.png)

![](img/575aeac969e6973fc296f7cad3842ee9.png)

![](img/971b5635af7c76cd13256b14592fd51c.png)

![](img/15ae1fc1cb82d3df5241d7d4ad004274.png)

![](img/cacf1a7a2a1f1197615b4df060a03d95.png)

### 发布 属于自己的Agent

1.  创建Agent并实验效果满意后，点开【发布】，点击【构建】，构建完成后点击【发布】

![](img/b76f83ac478157e8a09809ac035dae5f.png)

![](img/b2285dbf41797a6c829e63cd84eb701a.png)

1.  **跳转到创建创空**间，修改创空间信息，填入前面创建的 API-KEY，然后【复制创空间】

![](img/f14ea0c3aa7ec634b6bb108531bbad6a.png)

![](img/a9e1f31fddeda0ad6c9f6df1f6e82dff.png)

1.  打开相应的创空间，进入【设置】、【上线空间展示】

![](img/43904880868f1f68a147ff2a3ad3445e.png)

![](img/de9a2e85ab206b7f0e2f4d2e55595fa4.png)

![](img/181417c64905f873ebb0e18ae56e55ff.png)

![](img/749be9c257ff1192be76497e12190940.png)

![](img/39baaccffa917f20544b5a34d6b86e95.png)

![](img/9e29c760f30c22a1723b861e62928fd8.png)

![](img/abfeba6e4395836869664e8b664e32e5.png)

![](img/cd62728e7589c61e665139cb486745c1.png)

![](img/84a75a9dab9fc3e2071e0e021573f4e0.png)

1.  等待发布成功即可，祝大家都能享受Agent创建之旅~ Agent is ALL YOU NEED！~

![](img/7515a26df21951a6092f68e709145b91.png)

![](img/bbf9b31ab724e80750c7f0d615c7b23c.png)

![](img/cf45f58483f2ed67c2053c4c9dcdb872.png)

![](img/83c0fab841a70b0a9265f55d2153cfab.png)

### 可选：深入代码魔改 (仅简单介绍)

1.  在github上查看 [魔搭开源Agent框架](https://github.com/modelscope/modelscope-agent)，获取更多文档

1.  将github上的 [魔搭开源Agent框架](https://github.com/modelscope/modelscope-agent) 下载下来，进行深度魔改后发布

1.  将自己发布的Agent的代码下载下来，进行深度魔改后发布

[点开相应的创空间，在设](https://modelscope.cn/studios/modelscope/AgentFabric/summary)置处，可在环境变量中可查看自己通过 AgentFabric 创建并发布的Agent的代码，AGENR_URL，可直接复制链接，粘贴到浏览器地址栏下载

![](img/7bf7163a80723caedfeac2c26fe25441.png)

![](img/b3f86fff65cd325930fd1e9c0528c3d3.png)

1.  可以fork自己感兴趣的有源码的创空间，然后git clone到本地，进行深度魔改后发布

![](img/e5b448bd5afc09ad9dcab4485dbe4987.png)

![](img/d2c6dbc8f2ee4cfab4afb370093ba45f.png)

![](img/7e33ee4ed81ecd9229ceb8c618404422.png)

1.  发布前，需要有一个创空间，并git clone到本地，将修改好的文件复制到对应文件夹，并push即可

![](img/f04f0d7367814969d500ad773bad5576.png)

![](img/2e7a4dd1880e0a95c4888680cc3269a9.png)

1.  更详细的本地开发步骤可参考agent-tutorial/Agent原理深入+快速搭建demo 或 modelscope Agent 新人开发教程 @Tintin

1.  更多教程还可见[魔搭Agent大本营](https://modelscope.cn/brand/view/agent)

![](img/d0855f167bd1e852efea6687dba74fa7.png)