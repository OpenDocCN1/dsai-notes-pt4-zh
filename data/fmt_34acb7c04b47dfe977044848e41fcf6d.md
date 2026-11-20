
# 飞书--多维表格API授权

> 来源：[https://zinxkclpk97.feishu.cn/docx/BMihdLZNWoPH4wxgjopcbQHqn6S](https://zinxkclpk97.feishu.cn/docx/BMihdLZNWoPH4wxgjopcbQHqn6S)

## 飞书--多维表格API授权

### 使用场景

![](img/dfe39540c58d43148291efe4700ea84f.png)

![](img/0cba36c93ce2feebb8d9410d770bee7c.png)

Bot（应用）中工作流需要向多维表格写入数据，这里的多维表格为应用方（我）提供的公共表格，所以需要进行应用的访问授权。

像上述截图，可以使用官方或三方封装的授权插件+飞书文档插件，实现文档读写。

目前官方的多个插件是不支持使用`tenant_access_token`，可以使用其它开发者提供的飞书插件或自己按需封装。

`tenant_access_token`与`user_access_token`，其二者授权范围不同，前者是对应用整体授权，后者是为用户独立授权。

具体可以参照如下文档和介绍来了解，两种授权方式区别。

+ `tenant_access_token`

如果你的业务逻辑不需要操作用户的数据资源，仅需操作应用自身拥有的资源，例如在应用自身的文档目录空间下操作文档，则推荐使用`tenant_access_token`，无需额外申请授权。

+ `user_access_token`

如果你的业务逻辑需要操作用户的数据资源，例如需要在用户的文档目录空间下创建云文档，则推荐使用`user_access_token`，无需额外申请授权。

该情况下如果使用`tenant_access_token`，则需额外在资源层面为应用添加相应的授权。例如，如果使用`tenant_access_token`调用通讯录，则需在飞书开发者后台的权限管理页面配置应用的通讯录权限范围；而使用`user_access_token`则无需单独配置通讯录权限范围，遵循`user_access_token`所属用户的通讯录权限范围。

### 使用示例

为Bot授权飞书多维表格权限

登录飞书开发者后台：[飞书开放平台](https://open.feishu.cn/app?lang=zh-CN)

找到我们创建的Bot应用，点进去查看详情

![](img/26c969af23a7ec55bf3816598fc7070c.png)

在这里可以看到应用的`appId`和`appSecret`信息，提前记录下来，后续在获取`access_token`时会用到。

![](img/baf94798db1ba9908aeceef5378ed973.png)

#### 授权方式一：

点击开发配置==>权限管理==>选择多维表格 ，对需要的API进行批量授权

![](img/e662b3107d4ba24f97adb4bf929051ba.png)

详细官方API授权介绍：接入指南 - 服务端 API - 开发文档 - 飞书开放平台

授权之后我们可以在API调试控制台进行简单测试，

#### 授权方式二：

服务端API介绍：新增记录 - 服务端 API - 开发文档 - 飞书开放平台

我们也可以在这个页面中对特定的功能API进行权限配置（和授权方式一效果一样，这里更直观，按需授权）

![](img/c598679fa130e7aba185bd9f4c9842e2.png)

![](img/51c1584a45e6cfaed1e64468257fd84f.png)

如何调用多维表格新增记录的API，每个参数的含义和获取方式，这里就不多介绍，大家可以看官方API文档。在coze中也有非常多的飞书插件可供选择，我们把用到的参数格式搞清楚即可。

接入指南 - 服务端 API - 开发文档 - 飞书开放平台

### 其他

未涉及的其它飞书API访问授权也是同样的方式，按需尝试。

coze工作流+飞书结合的场景非常多，大家可以多多探索。

![](img/6603f7ef128540cbdc20d7245660a877.png)

## 飞书-多维表格仪表盘

多维表格提供了仪表盘功能，在这个场景下，可以结合日常运营收集到的用户信息，制作对应的分析图表，用于日常运营分析。

具体仪表盘如何使用大家可以参考如下文档，点一点即可配置，也比较简单，同时飞书多维表格也提供了非常丰富的功能，可以把每个表格当成应用使用也不为过。

[使用多维表格仪表盘](https://www.feishu.cn/hc/zh-CN/articles/161059314076-%E4%BD%BF%E7%94%A8%E5%A4%9A%E7%BB%B4%E8%A1%A8%E6%A0%BC%E4%BB%AA%E8%A1%A8%E7%9B%98)

`https://sf3-scmcdn-cn.feishucdn.com/obj/tos-cn-v-826391/o4eIgDWh9a5IlCpbxpO8TaopUghnmBeDC5OAc6`
