# 白嫖一个AI大模型的微信小助手

> 来源：[https://aiyanxishe.feishu.cn/docx/H6wxdzWdEoXFY3xXa2DcGXSZnWd](https://aiyanxishe.feishu.cn/docx/H6wxdzWdEoXFY3xXa2DcGXSZnWd)

作者：丁先生

原文：[全程白嫖 - 拥有一个AI大模型的微信小助手](https://oj0roee3at.feishu.cn/docx/TYT5dJy4xoWfMmxRNSXc1Cc7nPd)

有问题可以在文章末尾评论区留言

## 这有什么用？

> 一个能帮你解答任何问题的AI机器人，你可以将其接入到微信或群聊中，为用户提供自动答疑服务。你还可以投喂它特定知识，成为你的客服、专业老师或知识备忘录。

+   无需技术知识，小白也能轻松搭建，全程只需复制粘贴

+   可自定义知识库，满足不同需求

+   支持多场景应用，客服、教育、个人知识管理、等等

+   全程免费，白嫖党福音

DIN

## 部署架构图

来个思路图，其实这个搭建全程很简单。不要看图上面显示的很复杂，其实只要完成3点就行。

1.  搭建[OneAPI](https://github.com/songquanpeng/one-api)，这东西是为了汇聚整合多种大模型接口，方便你后面更换使用各种大模型。下面会告诉你怎么去白嫖大模型接口。

1.  搭建[FastGpt](https://fastgpt.in/)，这东西就是个知识库问答系统，你把知识文件放进去，再把上面的大模型接进来，作为分析知识库的大脑，最后回答你问题，这么个系统。如果你不想接到微信去，自己用用，其实到这里搭建完就OK了，他也有问答界面。

1.  接入微信，这里会用到个工具[智能微秘书](https://wechat.aibotk.com/)，就是为了方便配置FastGpt,把知识库问答系统接入到微信，这里建议先用个小号，接入个人微信。因为接企业微信好像要花钱。

完成上面3步就算OK了，那我们正式开始。

![](img/d6c25172d5a0b84a08f7fb552240941f.png)

## 准备工作

### 领免费云服务器

+   [阿里、腾讯对](https://www.aliyun.com/product/ecs)【新用户】[提供免费试用服](https://cloud.tencent.com/act/pro/free)务器的。可以去官网获取，阿里云的服务器ECS、腾讯云的轻量应用服务器（官网提供的试用规格服务器已够用了），到期后想续费也行，他们官网定期会有优惠，这个看大伙需求了。

+   规格配置参考见下图，但要选择【宝塔】系统。后面我都会以这个操作系统来搭建上面说到的工具。

![](img/672cc19557cb8c324e871c8685cf5109.png)

![](img/37fb7e640519061120884a1a5ebf5612.png)

### 领免费大模型接口

+   阿里的[通义千问大模型](https://dashscope.console.aliyun.com/)接口，他也有[文档说明](https://dashscope.console.aliyun.com/)、其实就是注册登录阿里云账号（记得实名哦），然后打开链接，创建个APIkey。下面是我创建的和我已经使用的模型情况。

![](img/b3435c685810b3fb68709d2382dcbace.png)

![](img/1435def7abe76f3728c82a184a14fe88.png)

+   [智谱AI（ChatGLM）](https://open.bigmodel.cn/)、[科大讯飞（SparkDesk）](https://xinghuo.xfyun.cn/sparkapi)，也有免费接口，但是限制了一定的Token额度，超了要花钱。有需要也可以先去官网领取免费的额度。

+   [谷歌的Gemini大模型](https://ai.google.dev/)（gemini 1.0 pro），好处是免费而且还能图片识别分析，坏处是要能链接外网的服务器（我还不会），当然你买了梯子的话就直接注册个谷歌邮箱去登录，去网页上体验下。等我去研究下外网服务器的问题后，再让它来充当我们知识库的大脑吧，哈哈。

## 搭建FsatGpt

我自己之前申请了一台免费的腾讯云，下面我就在腾讯云的服务器上操作，阿里云的菜单位置可能不太一样，对应的找下就行，操作步骤完成一样哦。

1.  给云服务器安装宝塔系统，刚刚领取服务器的时候，应该一并安装好了。

1.  然后在“应用管理”菜单中，点击登录，然后复制命令sudo /etc/init.d/bt default，获取宝塔系统的账号和密码。

![](img/6a3cc1b5d4336707830816b153e07496.png)

![](img/902c642e8279d790a7da0e0a692433ed.png)

1.  这里看见地址里有8888的端口号，我们先去把云服务器的防火墙开下，不然可能打不开网页链接、顺便把后面几个需要用到的端口都打开了。如图操作（防火墙>添加规则）

![](img/9acd40883d82018d27e401807031eccb.png)

![](img/a88d38e5dbc31828e89efe9c4cdb68c9.png)

1.  复制“外网面板地址”登录，然后首次进入会有个快速安装的界面，选择只选Nginx、mysql安装就行，然后去【软件商城】搜索Docker并安装。这些软件都要安装成功哦，这些是部署上面工具的重要基石。

![](img/c0ba8a1cef42ae2d7f3ee67ddc6357cb.png)

![](img/9803db0a2c061b14261a577ae267afb1.png)

1.  打开左侧【终端】，首先验证下Docker是否安装成功。看见版本号，就是安装成功了。

![](img/3b76126e05346f91eb19760480dd0851.png)

1.  然后还在【终端】，复制粘贴下面的4条命令，依次执行。执行完后在【文件】菜[单的root目录下出现](https://github.com/labring/FastGPT/blob/main/projects/app/data/config.json)个fastgpt的文件夹，文件夹里下载下来了2个配置文件docker-compose.yml和config.json。
有时候网络的问题，可手动创建一个fastgpt目录，并下载2个链接的文件，手动上传上去也行的（看截图中有“新建”“上传”按钮，可手动完成）

```
mkdir fastgpt
cd fastgpt
curl -O https://raw.githubusercontent.com/labring/FastGPT/main/projects/app/data/config.json
curl -O https://raw.githubusercontent.com/labring/FastGPT/main/files/deploy/fastgpt/docker-compose.yml
```

![](img/41af8ed1bc9098df1ac77d4524d9eb11.png)

1.  回到【终端】菜单，依次执行下面命令。

```
# 进入项目目录
cd fastgpt
# 创建 mongo 密钥
openssl rand -base64 756 &gt; ./mongodb.key
# 600不行可以用chmod 999
chmod 600 ./mongodb.key
chown 999:root ./mongodb.key
# 启动容器
docker-compose pull
docker-compose up -d
```

执行成功会显示如下图。首次执行docker-compose pull可能会需要点时间，他会下载“mongo”“pg”“fastgpt”这几个项目。

![](img/eacdfecd9b8716eacc186a3d4eeebec8.png)

1.  初始化 Mongo，不然打开FastGPT页面，登录时就会报错。执行如下命令初始化。

```
# 查看 mongo 容器是否正常运行
docker ps
# 进入容器
docker exec -it mongo bash

# 连接数据库
mongo -u myname -p mypassword --authenticationDatabase admin

# 初始化副本集。如果需要外网访问，mongo:27017 可以改成 ip:27017。但是需要同时修改 FastGPT 连接的参数（MONGODB_URI=mongodb://myname:mypassword@mongo:27017/fastgpt?authSource=admin =&gt; MONGODB_URI=mongodb://myname:mypassword@ip:27017/fastgpt?authSource=admin）
rs.initiate({
  _id: &quot;rs0&quot;,
  members: [
    { _id: 0, host: &quot;mongo:27017&quot; }
  ]
})
# 检查状态。如果提示 rs0 状态，则代表运行成功
rs.status()
```

这里执行docker ps时注意下，要查看 mongo 容器是否正常运行。如果有报错就去【Docker】菜单的“容器”按钮，查看下mongo的状态，没有运行的话，在它那栏点击右键，会有启动按钮，启动下就OK，然后去执行上面没完成的命令，最后提示 rs0 就是OK了。如图

![](img/eb58c157a0c28eb3c73dfe82e645e035.png)

![](img/f593eb8953b0b644718178f30186eedc.png)

1.  访问 FastGPT，目前可以通过 公网ip:3000 直接访问(注意防火墙)。登录用户名为 root ，密码1234
其实这里的账号密码是之前下载的docker-compose.yml 环境变量里设置的 DEFAULT_ROOT_PSW，所以后面想修改密码，打开FastGPT页面后也能操作 
这里的ip就是云服务的公网IP，在腾讯云的页面上能看到（如图），最后拼成这样的地址去打开登录000.000.000.000:3000

![](img/85cc4ee9ba2d160311a1f649a218e585.png)

![](img/85a3f48159514bf6dd8141b284b78635.png)

1.  [OK，FastGPT的搭建就](https://doc.fastgpt.in/docs/development/docker/)完成了。如果有什么搭建上的问题，也可以参考fastgpt的官方文档。
现在只是把知识库平台搭建好了，接下来要给知识库去接通大脑，能用AI来回答我们。请看下面。

## 搭建OneAPI

1.  到【终端】菜单，执行以下命令，建议直接用使用 SQLite 的部署命令就可以了。

```
# 使用 SQLite 的部署命令：
docker run --name one-api -d --restart always -p 3001:3000 -e TZ=Asia/Shanghai -v /home/ubuntu/data/one-api:/data justsong/one-api

# 使用 MySQL 的部署命令，在上面的基础上添加 `-e SQL_DSN=&quot;root:123456@tcp(localhost:3306)/oneapi&quot;`，请自行修改数据库连接参数，不清楚如何修改请参见下面环境变量一节。
# 例如：
docker run --name one-api -d --restart always -p 3001:3000 -e SQL_DSN=&quot;root:123456@tcp(localhost:3306)/oneapi&quot; -e TZ=Asia/Shanghai -v /home/ubuntu/data/one-api:/data justsong/one-api
```

其中，-p 3001:3000 中的第一个 3001 是宿主机的端口，可以根据需要进行修改（记得防火墙那也要修改哦），但不要改为3000，因为刚刚FastGpt已经用了，会有冲突的。

如果网络问题导致上面的镜像无法拉取下来，可以尝试使用 GitHub 的 镜像，将命令后面的的[ justson](https://github.com/songquanpeng/one-api)g/one-api 替换为 ghcr.io/songquanpeng/one-api 即可。部署如有问题可以看下OneAPI文档

1.  这时就部署完成，可以公网ip:3001打开oneAPI的页面了。

![](img/1596405f992e702163676e5093ccdd32.png)

1.  [还记得刚刚让你](https://dashscope.console.aliyun.com/)们白嫖的大模型API吗？阿里的通义千问大模型接口，这时要派上用场了，去阿里模型的链接里创建ApiKey，并复制下来。然后在OneAPI的页面，点击【渠道】添加新渠道。

![](img/9e8b45050b3c459c52e582d10423a3e9.png)

添加时，类型选择阿里通义千问，名称自己取个，类型选择好后模型是会默认加进去，你不用删减，还有就把刚刚阿里那复制的ApiKey粘贴到秘钥里去。这样就OK了。后续有其他的大模型也是一样的添加方式。

![](img/6223c724c61592222374fab28adf679e.png)

1.  然后，要创建OneApi的令牌，点击【令牌】添加新令牌

![](img/970fc20ecab64a23015ef4dd503cf10f.png)

这里名称可以自己取个，时间设为永不过期、额度设为无限额度。提交OK。

![](img/bb8f333ab12bcc13282ce02c24c735ba.png)

这时【令牌】中就有你添加的了，点击复制，得到令牌。

![](img/10c0b4fc7e0a96fc1b6f9ca01b621176.png)

1.  OneApi设置的额度有点少，我们手动改下，怕后面因为额度的问题导致不能使用，点击编辑，给自己多加几个0，然后提交OK。见图

![](img/d176ba272be0c895b1fe3c7ef12243da.png)

![](img/b9da60503b3cc33400d791f20f5f188a.png)

1.  OneApi这的部署就完成，现在我们要去给FastGpt去装上大脑

## 配置FsatGpt

1.  回到宝塔系统【文件】菜单，我们要修改2个文件docker-compose.yml 和config.json（文件路径root>fastgpt）

![](img/a7568112152bd69c17903866ce75602a.png)

1.  首先修改docker-compose.yml 中的 OPENAI_BASE_URL （API 地址：改为云服务的内网IP:3001，需要加/v1）和 CHAT_API_KEY （API 凭证，改为刚刚让你在OneApi复制的令牌，也是sk开头的）。其他都不用改，然后保存关闭。内网IP在云服务上能查询到，如图。

![](img/c9dd1a899931ab94ace5887528d0a6e4.png)

![](img/1e7836afd513c33a9702f066d38fc64c.png)

1.  然后就是修改config.json 中的"llmModels"（添加阿里大模型qwen-max） 和"vectorModels"（阿里向量模型text-embedding-v2，使用知识库功能时要用到的），然后保存关闭。如下代码。

```
&quot;llmModels&quot;: [
    {
      &quot;model&quot;: &quot;qwen-max&quot;,
      &quot;name&quot;: &quot;qwen&quot;,
      &quot;maxContext&quot;: 16000,
      &quot;maxResponse&quot;: 4000,
      &quot;quoteMaxToken&quot;: 13000,
      &quot;maxTemperature&quot;: 1.2,
      &quot;charsPointsPrice&quot;: 0,
      &quot;censor&quot;: false,
      &quot;vision&quot;: false,
      &quot;datasetProcess&quot;: false,
      &quot;toolChoice&quot;: false,
      &quot;functionCall&quot;: false,
      &quot;customCQPrompt&quot;: &quot;&quot;,
      &quot;customExtractPrompt&quot;: &quot;&quot;,
      &quot;defaultSystemChatPrompt&quot;: &quot;&quot;,
      &quot;defaultConfig&quot;: {}
    }
  ],
  &quot;vectorModels&quot;: [
    {
      &quot;model&quot;: &quot;text-embedding-ada-002&quot;,
      &quot;name&quot;: &quot;Embedding-2&quot;,
      &quot;charsPointsPrice&quot;: 0,
      &quot;defaultToken&quot;: 700,
      &quot;maxToken&quot;: 3000,
      &quot;weight&quot;: 100
    },
    {
      &quot;model&quot;: &quot;text-embedding-v2&quot;,
      &quot;name&quot;: &quot;qwen-Embedding-2&quot;,
      &quot;charsPointsPrice&quot;: 0,
      &quot;defaultToken&quot;: 700,
      &quot;maxToken&quot;: 3000,
      &quot;weight&quot;: 100
    }
  ],
```

1.  修改好了内容，就要重新启动下，让他生效。直接在【文件】菜单下的fastgpt目录，打开终端，执行启动命令docker-compose up -d，如图

![](img/2fbad8d9c367c4709285c92d8b21f1f5.png)

1.   到这里，我们的知识库就装上AI的大脑了，剩下的就是去接入微信了。
但这之前我们体验下知识库的功能，跟AI对对话，看看是否Ok。打开公网ip:3000进入fastgpt。新建个应用，这里不多说了根据界面来就行。看图示例，OK成功了，用的就是阿里的通义千问AI模型（qwen-max不花钱的接口哦）。

![](img/3193525a384f81615d778a86b1870999.png)

那我想根据我自己的知识库内容来回答问题，要怎么操作？

在知识库这个菜单新建知识库，看到新建时有2个模型选项了吧，在刚刚配置config.json时，我提到向量模型，就是这里的索引模型的选项，没有他知识库功能用不了哦。如图

![](img/47a517e0635502324b40f08396c0d16e.png)

然后我们上传文件或者写入信息都是可以的，我示范下，最后文本状态是“已就绪”就是OK了，我们去验证下

![](img/1ff6451d5dd1e697968c1c5fab97bc0e.png)

![](img/389e814d3f79c4826750efc6374fce95.png)

![](img/29b35dbbabb644a0337be923197d6d75.png)

![](img/55275a5f08f12df55ac90fa3ff180aeb.png)

![](img/d1bcc03b4fc3cd7d68a0ff00bc15629a.png)

1.  回到刚刚创建的应用，关联上创建的知识库。看下没有知识库和有知识库，回答的区别。

![](img/b76781f78f0b64a34ce16eb9a27b485e.png)

![](img/9fd7a8e9634f78792c2f60b51e703f0a.png)

1.  能感受到了吧，大家应该有很多自己想用的场景，可以自己丰富知识库的内容，成为专业的AI助手。好，下面我们把这个拥有知识库能力的AI助手接入到微信。可以帮助我在微信来解答问题。

## 接入微信

1.  因为接入微信要一个网站域名，可能不能接入，如图
（你也可以先尝试下不用域名，直接用公网ip:3000能否接入，直接跳过第1、2步。不行再回来用域名方式接入）

![](img/95ec7d1840f294ebc137227be9fca0da.png)

这里我们可以在腾讯云上花1元买一年的域名（这里也是唯一要花钱的地方），自己挑个域名后缀，选1元1年那种，然后前面随意填写个英文名称，如图。然后购买。

![](img/2df8fdea096bee81c6cd2abee9d97e2f.png)

![](img/a6bdc199e129cffffc6bf652e18895c9.png)

购买好后，点击下添加域名解析，都勾选上，如图。就OK了

![](img/7e6411316326487d19e223897b9f04ce.png)

1.  然后在宝塔系统进入【Docker】点击容器，找到fastgpt后面有个“反向代理”按钮，打开。把刚刚购买并解析好的域名写进去，并设置端口为3000。这样你就可以直接用域名地址打开FastGpt网页了。

![](img/1fdb40b416f9854397235c23c97358a5.png)

1.  然后用域名地址（跳过1、2步的用公网ip:3000）打开FastGpt在新建的应用中，获取个对外的API接口（操作：应用>发布应用>Api访问）。点击“新建”API秘钥，复制API KEY和新建按钮旁边的API根地址2个内容。用于配置接入微信。

![](img/4729e281514a238da7ffab2ebdc4105d.png)

1.  然后打开[智能微信秘书](https://wechat.aibotk.com/dashboard/main)**页面，注册登录****，打开【基础设置****】，如图（天**行api可以随意填、apikey就是在fastgpt应用里新建的API KEY，服务器地址就是API根地址）

![](img/c10f4c5ef469227e9c89a941f8e20e0b.png)

![](img/55c86dc8cb4b306bdc869b6538457aa2.png)

1.  然后在【个人中心】复制微信秘书的APIKEY和APISECRET，后面要用到，如图。

![](img/a656d731c2cd25a37cdcee552f756901.png)

1.  回到宝塔系统进入【Docker】，见下图操作路径：Docker>容器>创建容器>命令创建>复制命令>执行命令（复制命令时，把刚刚微信秘书复制的APIKEY和APISECRET粘贴进去后再执行）

```
docker run -d -e TZ=&quot;Asia/Shanghai&quot; -e AIBOTK_KEY=&quot;微秘书apikey&quot; -e AIBOTK_SECRET=&quot;微秘书apiSecret&quot;  --name=wechatbot aibotk/wechat-assistant
```

![](img/9a8491987fed5b1be362dbb67ed53cb5.png)

1.  好了，所有的部署、配置操作都完成，看下【Docker】容[器中，项目状](https://wechat.aibotk.com/dashboard/main)态是否都在“运行中”。然后去智能微信秘书控制台，首页扫码登录即可。如图。

![](img/1cbdd05d6071a41945a4825ad76ece08.png)

![](img/bf7b7ca984ea40f710fa718353342fc2.png)

1.  最后，我们来看下微信对话效果，是根据我们在fastgpt录入的文档内容来回答的哦。群里要记得@人才能得到回复，如图。

![](img/498d3dee8e9677a692990fc3671b1c5e.png)

![](img/ec4a92580a83653ea8d602fde3bd182a.png)

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>