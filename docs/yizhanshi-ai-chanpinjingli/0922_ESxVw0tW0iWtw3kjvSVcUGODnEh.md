# coze-studio开源版部署教程【Linux】

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BLpkd9nmCogbxixuBivcMjjGn5f](https://v11enp9ok1h.feishu.cn/docx/BLpkd9nmCogbxixuBivcMjjGn5f)

Coze开源地址：https://github.com/coze-dev/coze-studio

# 大半夜的让不让人睡觉了？

大半夜的天塌了，本来已经躺好准备睡觉了，一看朋友圈天塌了

**coze开源了？**

![](img/0aaabe2bae6f5f47b4a20a7609ca4ceb.png)

# coze开源，意味着什么？

开源意味着**免费**

**本地化部署的优势都有所**体现，例如隐私和数据安全得到保障

可以更加深入的学习企业级别的Ai应用的架构设计和系统提示词

可以基于开源版本，**搭建自己想要的Ai应用**

![](img/68c6afd67030acaa71319ad2e8e9d56f.png)

# 部署方法：用云服务器来云部署Coze Studio

接下来就告诉大家如何用云服务器来部署我们的Coze Studio:

## 万物起源：先来个云服务

部署一个项目，我们需要一个云服务器

打开云服务的网站，这里使用阿里云（大家自行注册）

https://www.aliyun.com/product/ecs

找到**产品**中的**云服务器ECS**

![](img/58e7276710c99d4d1299e64ce940a509.png)

![](img/5c0c46a1364422b03e2143b9a98b1852.png)

![](img/8e9e6aa55e15a247d46333291d7e224e.png)

![](img/fa0ba54c33a8257753bfee9463e41fb0.png)

![](img/7655d473300797657d602323ba1c514f.png)

![](img/fff99bba2239ad68876bd935ad856dec.png)

![](img/6b9c6db56228e5576b4faf64b3deb2ac.png)

**充值成功**后返回上个页面点击“确认下单”，等待跳转至如下页面，点击“远程连接”

![](img/2d90b1680b2ad4e99942276ff5bb4f93.png)

![](img/fb0ee04ece05a4d63fae4cc4b8f8a487.png)

![](img/8214e539e5c9c1bd078d9c5bd149ae35.png)

最后等待进入以下页面，云服务就开启成功了

![](img/4a41a05134fe855c4eb7d05a93de3a9c.png)

## 在云服务上部署[coze-studio](https://github.com/coze-dev/coze-studio/tree/main)

直接在云服务上的Linux系统上跑就好了：

将以下代码复制粘贴并回车运行

```
# 更新系统
apt update &amp;&amp; apt upgrade -y

# 安装必要工具
apt install -y curl git vim

# 验证安装
docker --version
docker-compose --version
```

![](img/3d469a62fddfbd8a616752539186eb80.png)

等待运行......

![](img/4c43bb11d12ff3118ce7f617d79ceffe.png)

出现下面类似的内容，左右两个键对着选择：“OK”、“Yes”后，按下回车；或者默认选择第一个，按下回车就可以

![](img/51d50d74bd2e836ef5d1b6a3c9010e39.png)

### 获取 Coze Studio 源码

复制粘贴以下代码，回车运行:

```
git clone https://github.com/coze-dev/coze-studio.git
cd coze-studio
```

![](img/ff558e015d378d12a5ad2de724b5b256.png)

### 配置模型（以火山方舟 doubao-seed-1.6 为例）

```
# 复制模型模板
cp backend/conf/model/template/model_template_ark_doubao-seed-1.6.yaml backend/conf/model/ark_doubao-seed-1.6.yaml

# 编辑配置文件（填入你的 API Key 和 Endpoint ID）
vim backend/conf/model/ark_doubao-seed-1.6.yaml
```

![](img/b9263b9ef784ed0f6b9eac314b923457.png)

1.  按 i 进入编辑模式(英文状态下按键盘i即可)，修改以下字段：

![](img/d3b1e991315e6e76d7a4da722704f2db.png)

1.  id: 填一个非 0 的唯一整数，必须是非 0 的整数，且全局唯一。

1.  meta.conn_config.api_key:[ 在线模型服务的 API K](https://www.volcengine.com/docs/82379/1541594)ey，在本示例中为火山方舟的 API Key，获取方式可参考获取火山方舟 API Key。

1.  meta.conn_config.model: 在线模型服务的 [model ID，在本示例中](https://www.volcengine.com/docs/82379/1099522)为火山方舟 doubao-seed-1.6 模型接入点的 Endpoint ID，获取方式可参考获取 Endpoint ID。

![](img/512fd39be3f7c946c32c1746822c6e8a.png)

![](img/8d7fce25e7603268667df95bfc717fae.png)

![](img/ae7cfa90fbd19ebbdc15753a9d1fdbd3.png)

![](img/a0c8590fb6e3f1a7f778a2fa6bbe4545.png)

![](img/099728f218ea5204e438187655cbd4f2.png)

1.  按 Esc，输入 :wq 保存退出，回到这个页面

![](img/2fdce064cba8aec69397c0fbd26e28ff.png)

### 启动 Coze Studio

复制回车

```
cd docker
cp .env.example .env
sudo docker compose --profile &#x27;*&#x27; up -d
```

![](img/3f4e77003788e2ed21079111d6ab38f0.png)

可能需要稍等有一段时间

![](img/014a28aa9c739bf582f7dc31ba9b9465.png)

重新如下，表示运行成功

![](img/b8050ebeb186d4b47160deea5532d03a.png)

打开阿里云-云服务器安全组

![](img/49c9e6b8a95b7a0c1f5ada09bb646212.png)

点击“管理规则”

![](img/42006cb74c131d1a5b02dd280641c17f.png)

入、出方向，添加规则

![](img/1cb7b88188e6feb3c5cd8e9c12866d92.png)

![](img/855ed16cb748e47dc73f21d0727759e9.png)

![](img/7cc687d2eaeccb40122a9b4d6fd736d1.png)

## 关闭示例，退剩下的金额

大家玩完后，千万记得停止，并且释放，要不会一直计费！！！

![](img/afe869978d3eeb0178fcdef50bed294f.png)

等到一天后，钱就可以提现了

在自己的头像找到“费用与成本”

![](img/cbd0f738250c7f1e0dc4a830679b59ed.png)

找到“提现”

![](img/793ae7fca71ede05c643e779c9be7685.png)

![](img/a6121a86be1acba15427ca29010ca7e2.png)

# 写在后面：自己的感触

这**件事情本身注定会掀起一阵波浪，**从部署到现在凌晨5:00，越到后面越兴奋，就比如其实我们可以在Github上发现，他们开源了coze中的两个核心项目：

+   扣子开发平台：https://github.com/coze-dev/coze-studio

+   扣子罗盘：https://github.com/coze-dev/coze-loop

但是也有一些弊端：

+   不能直接复制粘贴工作流，只能手动重建

+   插件会比线上版要少，但是插件可以自己写，而且核心功能都在，搭建流程也特别顺滑

在README.md中也很贴心的提供了功能清单：

![](img/9fbdfdd787c553e8469f24d3b24d34f9.png)

最后引用Richard Stallman的一句话作为结尾：

** **"自由软件运动的目标是创造一个没有边界的世界，在这个世界中，任何人都可以自由地使用、修改和分发软件。"

OK，这就是用云服务部署coze的具体内容了。

一切都起于对开源和技术的热爱。

我和我的小伙伴一拍即合，于是准备出了详细的从非技术背景的小白视角来写一篇有关于本地化部署Coze Studio的文章，当然同时在后续我们会探索Coze Studio在开源后的更多可能性和玩法，也欢迎大家一起探索和关注这个我们需要重新认识的工作流！