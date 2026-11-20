
# DeepSeek+RagFlow+Dify：本地知识库检索（Windows）

> 来源：[https://datawhaler.feishu.cn/docx/NaywdsIXKoJRkXx1VXRcolG7ngg](https://datawhaler.feishu.cn/docx/NaywdsIXKoJRkXx1VXRcolG7ngg)

> 上一次我们学习了如何部署ragflow，本次我们学习如何使用ragflow+dify搭建本地问答系统。

> 为什么要和dify结合呢，是因为dify的智能体功能非常强大，ragflow中虽然有类似的功能，但是并没有dify那么强大；但是dify做rag比较麻烦，ragflow做rag很方便。

> 最后，俺是个写教程的新手，如果大家有疑问和建议非常欢迎批评指正！

> 面向人群：计算机小白

> 阅读时间：10分钟

## 安装dify

+   确保电脑上安装了`docker`，`git`，`vscode`三个软件

+   Windows Docker 安装 | 菜鸟教程

+   `git`：https://cloud.tencent.com/developer/article/2099150

+   `vscode`：https://zhuanlan.zhihu.com/p/264785441

+   开始安装`dify`

首先在文件夹中点击鼠标右键，`open git bash here`，之后执行命令：`git clone https://github.com/langgenius/dify.git --branch 0.15.3`。完成后关闭`git bash`窗口，可以看到有一个`dify`的文件夹，打开这个文件夹并进入到`docker`文件夹下，右键菜单栏点击在终端中打开。

![](img/0a4d776b8111442fd14c8889d2c1684a.png)

![](img/72e9d584f40cddde0ffe656d21d4074f.png)

之后在终端中逐行执行以下命令：

```
copy .env.example .env
# 复制环境文件
copy middleware.env.example middleware.env
# 复制环境文件
docker compose -p dify up -d
# 启动为dify名称，避免compose冲突
```

![](img/43b00e07ef89822604d6f54b92d5ed22.png)

看到9个容器都在运行中即是部署成功了。此时打开`docker`容器可以看到`dify`后端运行情况：

![](img/971eb540dd3cf4b0150cdec38837ddd8.png)

### 解决端口冲突

找到上次存放`ragflow`的文件夹，进入`docker`中，用`vscode`打开`docker-compose.yml`文件，修改其中的端口。

![](img/9cc3d416aff88804d6e378cadcaa3627.png)

![](img/69c17b14fe7279eae114462baf18cd41.png)

之后在此文件夹（路径一定要正确，相当于在特定的房间里启动这台机器）下右键打开命令行，输入：`docker compose up -d`

可以看到`ragflow`服务也成功启动了，避免了端口冲突。当然这里也可以命名为`ragflow`服务`docker compose -p ragflow up -d`

![](img/14833028d345561d0008ce5a5e13723a.png)

## 搭建本地问答系统

### 创建ragflow知识库+ragflow api

这里我们可以继续选择使用`deepseek`的`api`来搭建，也可以设置为本地或者其他服务的接口。

输入网址：`localhost:8080` 打开`ragflow`界面，填入注册的账号和密码登录。

![](img/6801bb31586d639cd42628451efd9406.png)

![](img/f39fc4b63a5fcdb9da765227c83d0dff.png)

![](img/1ebd7aea6477404b1c1ed8dcb073a8e2.png)

![](img/ac42f95231723e4d4628e1efe9f17122.png)

> 文档解析会非常慢，请大家耐心等候，目前没有找到批处理的方案，手动一个一个点击解析吧（据我测试一次解析3个比较稳妥）

![](img/879a17228a3be6ff748abb2acb4f72f8.png)

这一步我们创建`ragflow`的`API`，以便之后接入`dify`中。

### ragflow进行本地问答

+   进行检索测试，测试检索命中准确度如何

![](img/46ba14073087203bab3747500cc8529b.png)

+   直接使用搜索功能查询名词，或者给出解释。

![](img/f2a7a342db1d8ef5bdf0833dd41da5ee.png)

+   聊天这里创建简易的智能体

![](img/5bc6901eabddbc7353c470ffdf217457.png)

![](img/fccc357d13aaefbc8e27b32a0578a9f1.png)

![](img/639acb0b4b75bce156fd8cf3b730693b.png)

+   这里我们可以邀请团队成员一起使用这个知识库

![](img/dab5326b2fd1349b24f4cfc03d6732cb.png)

### Dify与ragflow联通

同样的我们在浏览器地址栏输入：`localhost:80` 即可打开`Dify`

![](img/28b7a533b9695f23fa27677575c3d1fb.png)

![](img/85906dbd2864c828271aeeb9824ce493.png)

图中第4步骤需要填本地`ip`地址，`http://本地IP:8080/api/v1/dify`

`apikey`为上一步创建的`ragflow api`，复制填入即可。

本地`ip` 查看方法：打开`powershell`，输入 `ipconfig`，找到下图对应的`IPv4`地址：

![](img/f83794fb575d168a1b575ebe6a267b6b.png)

之后我们点击下图所示的

![](img/ed10db0360913d32590a8d750aa431c7.png)

![](img/9c4e68ad419e0ed2123faa50565cd68f.png)

![](img/429fd5afb563543efa9a05560b22fccc.png)

![](img/392ceb1f3ffc29aa362390f3b9b02bad.png)

可以看到我们的知识库是调用成功了。

## Dify的使用

![](img/f8bb8a9e40f35fa078e6bffd4ed7bf50.png)

![](img/446cfdb26a1eb010944a2cf1cc753e25.png)

![](img/f91dc6615163d1c89563c81c17d8568e.png)

我们可以看到这里有非常多的模型配置选择，在`ragflow`中有`embedding`模型和`rerank`模型，不过它们都没有发布为`api`，无法调用。这里我们可以去硅基流动平台或者阿里百炼平台或者火山引擎，它们都提供了多种模型的`api`服务，申请`api`后填入`key`即可，个人觉得其中硅基流动的`api`使用起来比较方便。

![](img/4bc8f58d96592c8bbacf3667481eddfe.png)

在探索页面有非常多的应用开发模板提供，按照自己的应用场景选择合适的即可，比如我想要一个社会学领域知识深度搜索的应用，就选择`DeepResearch`模板。

![](img/4c671e751143aad4406b2467b667b034.png)

最后我们开发好工作流应用就可以啦，把配置好的应用发布为服务或者`API`都可以。

关于`Dify`的使用放在后续的教程中哦。`Dify`与`coze`非常[微学习17 | Coze：生成书籍推荐卡片](https://datawhaler.feishu.cn/wiki/U2N0w3qNSinGlLkd2o6ckBSTnBc)，想学习的小伙伴可先行移步`coze`教程

## 应用启动与更新

### 应用启动

打开`docker`界面，把对应的容器服务全部开启。

![](img/bec334d67c5f9cfd0c1687c365dbc077.png)

浏览器分别输入：`localhost:80` 与 `localhost:8080` 打开`dify` 和 `ragflow`。

### 应用更新

在对应的文件夹位置打开命令行，更新哪个就在哪个文件夹（`docker`文件夹）下打开，并键入以下命令：

```
docker compose down
git pull origin main
docker compose pull
docker compose up -d
```

同时，请更新环境变量配置：

+   如果 `.env.example` 文件有更新，请同步更新您本地的 `.env` 文件。

+   检查 `.env` 文件中的所有配置项，确保它们与您的实际运行环境相匹配。您可能需要将 `.env.example` 文件中的新变量和更新后的值添加到 `.env` 文件中。

## To do list
