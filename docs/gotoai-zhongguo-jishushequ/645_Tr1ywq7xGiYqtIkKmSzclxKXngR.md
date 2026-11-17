# DeepSeek+RagFlow+Dify：本地知识库检索

> 来源：[https://gotoaiworld.feishu.cn/docx/G13Xd7GQnouHK2xF0uHcxf8JnPb](https://gotoaiworld.feishu.cn/docx/G13Xd7GQnouHK2xF0uHcxf8JnPb)

原创：墙上的斑点

[DeepSeek+RagFlow+Dify：本地知识库检索（Windows）](https://datawhaler.feishu.cn/wiki/Fzsrw8SsriPK5Vk4SnmcXDVynrg)

> 面向人群：计算机小白

> 阅读时间：10分钟

# 安装dify

+   确保电脑上安装了docker，git，vscode三个软件

+   Windows Docker 安装 | 菜鸟教程

+   git：https://cloud.tencent.com/developer/article/2099150

+   vscode：https://zhuanlan.zhihu.com/p/264785441

+   开始安装dify

首先在文件夹中点击鼠标右键，open git bash here，之后执行命令：git clone https://github.com/langgenius/dify.git --branch 0.15.3。完成后关闭git bash窗口，可以看到有一个dify的文件夹，打开这个文件夹并进入到docker文件夹下，右键菜单栏点击在终端中打开。

![](img/fdb9b79c617e05e05329d6a137a9eaa1.png)

![](img/bfcba475d642f88f446508016da51bd7.png)

之后在终端中逐行执行以下命令：

```
copy .env.example .env
# 复制环境文件
copy middleware.env.example middleware.env
# 复制环境文件
docker compose -p dify up -d
# 启动为dify名称，避免compose冲突
```

![](img/49d73bf35c864cf990afbfef14181692.png)

看到9个容器都在运行中即是部署成功了。此时打开docker容器可以看到dify后端运行情况：

![](img/bfe6c91cbe2bf365e84bf649e311cd18.png)

+   解决端口冲突

找到上次存放ragflow的文件夹，进入docker中，用vscode打开docker-compose.yml文件，修改其中的端口。

![](img/66d345865f44015a73a764b0fd80a6c1.png)

![](img/f5bf97728b79cca6204c0d57851e81d2.png)

之后在此文件夹（路径一定要正确，相当于在特定的房间里启动这台机器）下右键打开命令行，输入： docker compose up -d

可以看到ragflow服务也成功启动了，避免了端口冲突。当然这里也可以命名为ragflow服务docker compose -p ragflow up -d

![](img/8c5362dc83ea2de1367a33887c410130.png)

# 搭建本地问答系统

## 创建ragflow知识库+ragflow api

这里我们可以继续选择使用deepseek的api来搭建，也可以设置为本地或者其他服务的接口。

输入网址：localhost:8080 打开ragflow界面，填入注册的账号和密码登录。

![](img/443d35a0a71c3ee7fcae6bf19871c798.png)

![](img/eb466467aa1cac33afe4c8c4ac3a60cb.png)

![](img/792b847fc177500b5961032634662a7f.png)

![](img/b82b34f2bccf5d40e1ccf70a0b2c4130.png)

> 文档解析会非常慢，请大家耐心等候，目前没有找到批处理的方案，手动一个一个点击解析吧（据我测试一次解析3个比较稳妥）

![](img/892243904afef6528b0f1d35ad32a462.png)

这一步我们创建ragflow的API，以便之后接入dify中。

## ragflow进行本地问答

+   进行检索测试，测试检索命中准确度如何

![](img/25712d301402aaa79db4c18742a03f4b.png)

+   直接使用搜索功能查询名词，或者给出解释。

![](img/05da8ad9bae048e562d859a38c10c0f8.png)

+   聊天这里创建简易的智能体

![](img/f1049350ab7cb8c8cee048324828d4cc.png)

![](img/dcacb7039b6dc9e37b1af6813f53067a.png)

![](img/639c8f4083eb94ead15197eeaa55b623.png)

+   这里我们可以邀请团队成员一起使用这个知识库

![](img/b3db10e4d91a28cae6bb28388b4fb2da.png)

# Dify与ragflow联通

同样的我们在浏览器地址栏输入：localhost:80 即可打开Dify

![](img/6cc48b16340987814c11e356f2b2a0ea.png)

![](img/a619324038ad8e6ff9059421e1169689.png)

**图中第4步骤需要填本地ip地址，http://本**地IP:8080/api/v1/dify

apikey为上一步创建的ragflow api，复制填入即可。

本地ip 查看方法：打开powershell，输入 ipconfig，找到下图对应的IPv4地址：

![](img/e00e4dbaf1fe3cb53587f3c28b3482ac.png)

之后我们点击下图所示的

![](img/9b439cc24c2c50db4cbdce8cac0fa5c2.png)

![](img/c7192c0a4d041823380d5173e60a52fc.png)

![](img/e2ebbe19b09caac9786ed6e6cd656c60.png)


可以看到我们的知识库是调用成功了。

# Dify的使用

![](img/ab177ac80b4367f4b81f3decff038c29.png)

![](img/51977d82968799cd2714185db4c69271.png)

![](img/7cbc3e545fc0dbe2724eef8667db1f3b.png)

我们可以看到这里有非常多的模型配置选择，在ragflow中有embedding模型和rerank模型，不过它们都没有发布为api，无法调用。这里我们可以去硅基流动平台或者阿里百炼平台或者火山引擎，它们都提供了多种模型的api服务，申请api后填入key即可，个人觉得其中硅基流动的api使用起来比较方便。

![](img/e222713f5e9e8b4a7506aa9ff71c7807.png)

在探索页面有非常多的应用开发模板提供，按照自己的应用场景选择合适的即可，比如我想要一个社会学领域知识深度搜索的应用，就选择DeepResearch模板。

![](img/a83fb2c88cf89f3241df47fefe17011d.png)

最后我们开发好工作流应用就可以啦，把配置好的应用发布为服务或者API都可以。

关于Dify的使用放在后续的教程中哦。Dify与coze非常[微学习17 | Coze：生成书籍推荐卡片](https://datawhaler.feishu.cn/wiki/U2N0w3qNSinGlLkd2o6ckBSTnBc)，想学习的小伙伴可先行移步coze教程

# 应用启动与更新

1.  应用启动：打开docker界面，把对应的容器服务全部开启。

![](img/5945bbb57e9b2413a72d75ba44a4a878.png)

浏览器分别输入：localhost:80 与 localhost:8080 打开dify 和 ragflow。

1.  应用更新：
在对应的文件夹位置打开命令行，更新哪个就在哪个文件夹（docker文件夹）下打开，并键入以下命令：

```
docker compose down
git pull origin main
docker compose pull
docker compose up -d
```

同时，请更新环境变量配置：

+   如果 .env.example 文件有更新，请同步更新您本地的 .env 文件。

+   检查 .env 文件中的所有配置项，确保它们与您的实际运行环境相匹配。您可能需要将 .env.example 文件中的新变量和更新后的值添加到 .env 文件中。

# To do list