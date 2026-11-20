
```
"llmModels": [
    {
      "model": "qwen-max",
      "name": "qwen",
      "maxContext": 16000,
      "maxResponse": 4000,
      "quoteMaxToken": 13000,
      "maxTemperature": 1.2,
      "charsPointsPrice": 0,
      "censor": false,
      "vision": false,
      "datasetProcess": false,
      "toolChoice": false,
      "functionCall": false,
      "customCQPrompt": "",
      "customExtractPrompt": "",
      "defaultSystemChatPrompt": "",
      "defaultConfig": {}
    }
  ],
  "vectorModels": [
    {
      "model": "text-embedding-ada-002",
      "name": "Embedding-2",
      "charsPointsPrice": 0,
      "defaultToken": 700,
      "maxToken": 3000,
      "weight": 100
    },
    {
      "model": "text-embedding-v2",
      "name": "qwen-Embedding-2",
      "charsPointsPrice": 0,
      "defaultToken": 700,
      "maxToken": 3000,
      "weight": 100
    }
  ],
```

1. 修改好了内容，就要重新启动下，让他生效。直接在【文件】菜单下的`fastgpt`目录，打开终端，执行启动命令`docker-compose up -d`，如图

![](img/2fbad8d9c367c4709285c92d8b21f1f5.png)

2. 到这里，我们的知识库就装上AI的大脑了，剩下的就是去接入🛰️了。

但这之前我们体验下知识库的功能，跟AI对对话，看看是否Ok。打开公网ip:3000进入`fastgpt`。新建个应用，这里不多说了根据界面来就行。看图示例，OK成功了，用的就是阿里的通义千问AI模型（`qwen-max`不花钱的接口哦）。

![](img/3193525a384f81615d778a86b1870999.png)

那我想根据我自己的知识库内容来回答问题，要怎么操作？

在知识库这个菜单新建知识库，看到新建时有2个模型选项了吧，在刚刚配置`config.json`时，我提到向量模型，就是这里的索引模型的选项，没有他知识库功能用不了哦。如图

![](img/47a517e0635502324b40f08396c0d16e.png)

然后我们上传文件或者写入信息都是可以的，我示范下，最后文本状态是“已就绪”就是OK了，我们去验证下

![](img/1ff6451d5dd1e697968c1c5fab97bc0e.png)

![](img/389e814d3f79c4826750efc6374fce95.png)

![](img/29b35dbbabb644a0337be923197d6d75.png)

![](img/55275a5f08f12df55ac90fa3ff180aeb.png)

![](img/d1bcc03b4fc3cd7d68a0ff00bc15629a.png)

3. 回到刚刚创建的应用，关联上创建的知识库。看下没有知识库和有知识库，回答的区别。

![](img/b76781f78f0b64a34ce16eb9a27b485e.png)

![](img/9fd7a8e9634f78792c2f60b51e703f0a.png)

4. 能感受到了吧，大家应该有很多自己想用的场景，可以自己丰富知识库的内容，成为专业的AI助手。好，下面我们把这个拥有知识库能力的AI助手接入到🛰️。可以帮助我在🛰️来解答问题。

## 接入🛰️

1. 因为接入🛰️要一个网站域名，可能不能接入，如图

（你也可以先尝试下不用域名，直接用公网ip:3000能否接入，直接跳过第1、2步。不行再回来用域名方式接入）

![](img/95ec7d1840f294ebc137227be9fca0da.png)

这里我们可以在腾讯云上花1元买一年的域名（这里也是唯一要花钱的地方），自己挑个域名后缀，选1元1年那种，然后前面随意填写个英文名称，如图。然后购买。

![](img/2df8fdea096bee81c6cd2abee9d97e2f.png)

![](img/a6bdc199e129cffffc6bf652e18895c9.png)

购买好后，点击下添加域名解析，都勾选上，如图。就OK了

![](img/7e6411316326487d19e223897b9f04ce.png)

2. 然后在宝塔系统进入【Docker】点击容器，找到`fastgpt`后面有个“反向代理”按钮，打开。把刚刚购买并解析好的域名写进去，并设置端口为3000。这样你就可以直接用域名地址打开FastGpt网页了。

![](img/1fdb40b416f9854397235c23c97358a5.png)

3. 然后用域名地址（跳过1、2步的用公网ip:3000）打开FastGpt在新建的应用中，获取个对外的API接口（操作：应用>发布应用>Api访问）。点击“新建”API秘钥，复制API KEY和新建按钮旁边的API根地址2个内容。用于配置接入🛰️。

![](img/4729e281514a238da7ffab2ebdc4105d.png)

4. 然后打开[智能🛰️秘书](https://wechat.aibotk.com/dashboard/main)页面，注册登录，打开【基础设置】，如图（天行api可以随意填、`apikey`就是在`fastgpt`应用里新建的`API KEY`，服务器地址就是API根地址）

![](img/c10f4c5ef469227e9c89a941f8e20e0b.png)

![](img/55c86dc8cb4b306bdc869b6538457aa2.png)

5. 然后在【个人中心】复制🛰️秘书的`APIKEY`和`APISECRET`，后面要用到，如图。

![](img/a656d731c2cd25a37cdcee552f756901.png)

6. 回到宝塔系统进入【Docker】，见下图操作路径：Docker>容器>创建容器>命令创建>复制命令>执行命令（复制命令时，把刚刚🛰️秘书复制的`APIKEY`和`APISECRET`粘贴进去后再执行）

```
docker run -d -e TZ="Asia/Shanghai" -e AIBOTK_KEY="微秘书apikey" -e AIBOTK_SECRET="微秘书apiSecret"  --name=wechatbot aibotk/wechat-assistant
```

![](img/9a8491987fed5b1be362dbb67ed53cb5.png)

7. 好了，所有的部署、配置操作都完成，看下【Docker】容器中，项目状态是否都在“运行中”。然后去智能🛰️秘书控制台，首页扫码登录即可。如图。

![](img/1cbdd05d6071a41945a4825ad76ece08.png)

![](img/bf7b7ca984ea40f710fa718353342fc2.png)

8. 最后，我们来看下🛰️对话效果，是根据我们在`fastgpt`录入的文档内容来回答的哦。群里要记得@人才能得到回复，如图。

![](img/498d3dee8e9677a692990fc3671b1c5e.png)

![](img/ec4a92580a83653ea8d602fde3bd182a.png)

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>
