# Windsurf瞎写想还原怎么办？老金教你Git版本管理

> 来源：[https://my.feishu.cn/docx/Fa4fdgVpXoYfDMxjWJncShGHnye](https://my.feishu.cn/docx/Fa4fdgVpXoYfDMxjWJncShGHnye)

自从老金推荐Windsurf以来，总有小伙伴说改错了怎么还原啊？

所以今儿老金给大家带来了版本管理方案，包你一键还原。

咱这就直接开始。

### 创建本地仓库

首先，创建一个空工程目录（或直接使用已有项目也可以），并使用Windsurf打开文件夹。

![](img/86b02e32fdf43d3d5fc20f84b03fad5d.png)

初始化仓库，链接Git。

Git老金之前有讲过了，安装教学在这。

[Git 安装 - 拉取GIT项目必备](https://tffyvtlai4.feishu.cn/wiki/MF1ywmPKhig7p3kpIqNcFsmPnPe)

> [Git](https://baike.baidu.com/item/%E5%88%86%E5%B8%83%E5%BC%8F/19276232?fromModule=lemma_inlink)[（读音为](https://baike.baidu.com/item/%E7%89%88%E6%9C%AC%E7%AE%A1%E7%90%86/2511538?fromModule=lemma_inlink)/gɪt/）是一个[开源的分布式版本控制系统，可](https://baike.baidu.com/item/Linus%20Torvalds/9336769?fromModule=lemma_inlink)以有效、高速[地处理从很小到](https://baike.baidu.com/item/Linux%E5%86%85%E6%A0%B8/10142820?fromModule=lemma_inlink)非常大的项目版本管理。( [[1])也是](https://baike.baidu.com/item/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6%E8%BD%AF%E4%BB%B6/2617766?fromModule=lemma_inlink)Linus Torvalds为了帮助管理Linux内核开发而开发的一个开放源码的版本控制软件。

![](img/762f2cc2bc6485ec5dd39d1e74be2052.png)

如果是已有项目，先在终端内使用命令，提交到本地仓库。

点击终端，新建终端。

![](img/a80328804256727e18177d139a9a9424.png)

添加文件到暂存区，并提交到本地仓库。

```
#把路径下所有文件添加到暂存区
git add .

#提交到本地仓库
git commit -m 初始化项目
```

![](img/6bdbc4f1a489b1ee7822a7cea891779b.png)

这时候你会发现以上报错，是因为没有登录，让我们进行下Github账号绑定。

### 申请GitHub

申请Github账号，全球最大“基佬”网站，开个玩笑，是代码工程师聚集地。

申请账号过程就不写了，Gmail可以直接注册。

地址：https://github.com/

![](img/15ed7422e0647f8d07274c5a2af23561.png)

直接选择下方蓝色部分。

![](img/b52317253b018cb777c8ae8933ffd01f.png)

选择允许。

![](img/7eff03388377c0607afc345b88c92b67.png)

复制代码，去Github验证。

![](img/7fb1aac23a6b7ac62f22b03cbc0b20e6.png)

网页上如果已登录，则直接继续。

![](img/03203c01512898883f8676585cd94976.png)

这里直接复制Windsurf上的代码。如果不记得没关系，可以返回Windsurf上查看。

左下角会显示。

![](img/305bbd74faaf9c7075baf511ee77694d.png)

进行授权。

![](img/3695c02ea14e33c9b28f92d5f46e4eba.png)

使用手机获取验证码。

![](img/083054b1fb6ec74c393c577aeaa59b71.png)

手机上获取的验证码在网页上输入就搞定了，这样你就连上了Github。

![](img/e6301c015f4f5542cb5a628e4821508e.png)

在GitHub上，创建一个新的仓库，用于远程连接本地的新项目。

![](img/64b11f56fdd685f3427d2419390a365d.png)

然后如图所示，老金把说明都给你们写好了。

![](img/02585bcdf1d6da3e784341714c747921.png)

### 连接本地与远程仓库

重复第二步中的拉取远程仓库，我们会发现刚刚建立的仓库刷新了。

![](img/6d1cb3f46e42d9fff3dc664dd4b590e4.png)

此时会要求建立“分支”，分支的意思简单点说就是备份记录，随便起个分支名称。

![](img/76d610e374709635b54f5a4c35a3e8f6.png)

### 建立完整链接

这里还记得第一步中，实际上是没有添加成功的吧，让我们重新来。

重新执行上述的git add . 和 git commit后，输入Git名字和邮箱。

```
git config --global user.name &quot;您的名字&quot;
git config --global user.email &quot;您的邮箱&quot;
```

![](img/bc0f67ad6299540a60e50740ed936c48.png)

这时候就可以上传本地。

![](img/eed317fa6a6528c0c8715459adae17bb.png)

此时在Github中就可以看见提交记录了，以后在本地开发就再也不怕Windsurf瞎改后没法还原了。

此时本地仓库与远程仓库相关联了，并把本地文件推送到远程仓库中。

### 合并仓库

上传完成后，我们在Github仓库主页上可以看到我们新提交的文件了！

![](img/e499b29ee6519aae8fd5036303a6e141.png)

### 常见问题

你可能会遇见这个错误，无法链接。

![](img/be1fead5612403838d3b254e4d5371dc.png)

这是正常情况，国内链接概率性的无法链接，很正常，过一段时间再试就好。

还可能会存在分支错乱的问题，没关系，交给Windsurf自动进行提交。

![](img/00d96b712e7800409afa58940cdd0b6a.png)

他会自动完成，并告你在Github上怎么操作，甚至你可以交给它来操作。

![](img/73cb6a06c5edb31849d7251b055f38f2.png)

包含整理分支等等。

![](img/c2ed2706951bd8c715ef662abc6d9099.png)

以后就可以再也不用担心它瞎改了，改成屎了，也能还原！

熟练记下这个操作！直接新建个文件夹后，直接拉取，就可以还原到你想要的版本状态了。

GitHub是支持多分支管理的，也就是说你可以给一个项目创建多个版本。

再也不怕AI编程工具改成屎山了！

![](img/108daf24d88bc34cd9bede26de46145d.png)

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>