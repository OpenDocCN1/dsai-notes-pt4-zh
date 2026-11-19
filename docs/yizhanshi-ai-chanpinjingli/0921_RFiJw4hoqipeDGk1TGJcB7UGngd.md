
# coze-studio开源版部署教程【Linux】

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EtnLdvaGgoKNVBxsYa2cq3lQnqg](https://v11enp9ok1h.feishu.cn/docx/EtnLdvaGgoKNVBxsYa2cq3lQnqg)

## 写在前面

感谢Datawhale 张浩远提供的帮助！

Coze开源地址：https://github.com/coze-dev/coze-studio

## 大半夜的让不让人睡觉了？

大半夜的天塌了，本来已经躺好准备睡觉了，一看朋友圈天塌了

**coze开源了？**

![](img/70692422a51b5eb10a3177a22cb68aff.png)

很多人也许会觉得这没什么，但对我们俩来说是巨大的消息，我们两个一拍即合，快速的用了Github上的内容，下载，部署，配置，测试一个落的跑了一遍流程。与此同时，我们也把coze studio本地化部署的完整步骤和使用体验给记录了下来。

先来聊一下我们俩对这件事以及对字节的直观感受：

![](img/72f951f2041e03eb0fc382d9ae96ad07.png)

我是那么评价的：

![](img/0c4a35b107083a5b59e10a260c0e4a81.png)

是的，因为开源本身就是一个很伟大的行为，同时他们用的是Apache2.0协议，你可以理解为允许代码修改和再发布（无论是作为开源还是商业软件）。伟大无需多言

同时，一个商业价值极高的商业产品开源是有自信和多有魄力！

## coze开源，这意味着什么？

对于我们创作者而言，这次的开源就有以下几点：

开源意味着**免费**

本地化部署的优势都有所体现，例如隐私和数据安全得到保障

可以更加深入的学习企业级别的Ai应用的架构设计和系统提示词

可以基于开源版本，**搭建自己想要的Ai应用**

![](img/e3182a32591612efb0b77a52280ebf20.png)

## 部署方法：用云服务器来云部署Coze Studio

让我们先把这份惊喜放一边

我们需要做的就是迈出第一步，我们来告诉大家如何用云服务器来部署我们的Coze Studio:

### 万物起源：先来个云服务

部署一个项目，我们需要一个云服务器

打开云服务的网站，这里使用阿里云（大家自行注册）

https://www.aliyun.com/product/ecs

找到**产品**中的**云服务器ECS**

选择“**按量付费**”、“**新加坡**”

网络及可用区选择“**默认**”

（可自行优化）选择x86计算中的**2vCPU**、**4.0GiB**

系统选择**Ubuntu**，同时在下面的拓展程序选择**Ducker社区版**

在网络和安全组的带宽峰值调到**100Mbps**

登录密码，我建议直接使用“登录密码”，快速一点，有安全需求的建议密钥对

检查无误之后，“**确认下单**”

跳转充值100元，**后面可以退**

等待跳转，点击“**远程连接**”

点击“**立即登录**”

![](img/e1b24d93f7c81d3dfcab46c31047fb05.png)

建议选择“**终端连接**”

等待进入这个页面

### 在云服务上部署 coze-studio

直接在云服务上的Linux系统上跑就好了：

```
# 更新系统
apt update && apt upgrade -y

# 安装必要工具
apt install -y curl git vim

# 验证安装
docker --version
docker-compose --version
```

下面是运行的过程

![](img/cff826a9ee5d6d0c2637677bc2d4fcb9.png)

出现下面类似的内容，左右两个键对着选择：“OK”、“Yes”后，按下回车；或者默认选择第一个，按下回车就可以

![](img/76bdd7980f656cef97bd001f06ce68d6.png)

#### 获取 Coze Studio 源码

```
git clone https://github.com/coze-dev/coze-studio.git
cd coze-studio
```

#### 配置模型（以火山方舟 doubao-seed-1.6 为例）

```
# 复制模型模板
cp backend/conf/model/template/model_template_ark_doubao-seed-1.6.yaml backend/conf/model/ark_doubao-seed-1.6.yaml

# 编辑配置文件（填入你的 API Key 和 Endpoint ID）
vim backend/conf/model/ark_doubao-seed-1.6.yaml
```

![](img/2665db8f31d850b11c3d8b45f02f99ac.png)

1. 按 `i` 进入编辑模式，修改以下字段：

![](img/e908a3bb439050cda4b3aad8db7b5d29.png)

1. `id`: 填一个非 0 的唯一整数，必须是非 0 的整数，且全局唯一。

1. `meta.conn_config.api_key`: [在线模型服务的 API Key](https://www.volcengine.com/docs/82379/1541594)，在本示例中为火山方舟的 API Key，获取方式可参考获取火山方舟 API Key。

1. `meta.conn_config.model`: 在线模型服务的 [model ID，在本示例中](https://www.volcengine.com/docs/82379/1099522)为火山方舟 doubao-seed-1.6 模型接入点的 Endpoint ID，获取方式可参考获取 Endpoint ID。

![](img/0558b5115866e3813803f9426c871011.png)

1. 按 `Esc`，输入 `:wq` 保存退出，回到这个页面

#### 启动 Coze Studio

复制回车

```
cd docker
cp .env.example .env
sudo docker compose --profile '*/*' up -d
```

可能需要稍等有一段时间

重新如下，表示运行成功

![](img/fbd4bb5a5b0ff874a684db923d6102af.png)

打开阿里云-云服务器安全组

点击“管理规则”

入、出方向，添加规则

### 下面很重要！除非你钱多

大家玩完后，记得停止，并且释放

![](img/c654744493cca1e3db318757012a58ab.png)

等到一天后，钱就可以提现了

在自己的头像找到“费用与成本”

![](img/4477a74befa1e0767656a18ffaa7e923.png)

找到“提现”

![](img/30841ab2f5c4e5e9d97e228cae7da105.png)

## 写在后面：自己的感触

这件事情本身注定会掀起一阵波浪，从部署到现在凌晨5:00，越到后面越兴奋，就比如其实我们可以在Github上发现，他们开源了coze中的两个核心项目：

+ 扣子开发平台：https://github.com/coze-dev/coze-studio

+ 扣子罗盘：https://github.com/coze-dev/coze-loop

但是也有一些弊端：

+ 不能直接复制粘贴工作流，只能手动重建

+ 插件会比线上版要少，但是插件可以自己写，而且核心功能都在，搭建流程也特别顺滑

在`README.md`中也很贴心的提供了功能清单：

![](img/3d6f0ce06e349b3d5faa6afe25714ca8.png)

最后引用Richard Stallman的一句话作为结尾：

> "自由软件运动的目标是创造一个没有边界的世界，在这个世界中，任何人都可以自由地使用、修改和分发软件。"

OK，这就是用云服务部署coze的具体内容了。

一切都起于对开源和技术的热爱。

我和我的小伙伴一拍即合，于是准备出了详细的从非技术背景的小白视角来写一篇有关于本地化部署Coze Studio的文章，当然同时在后续我们会探索Coze Studio在开源后的更多可能性和玩法，也欢迎大家一起探索和关注这个我们需要重新认识的工作流！
