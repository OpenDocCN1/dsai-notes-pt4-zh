# 魔搭创空间部署教程

> 来源：[https://datawhaler.feishu.cn/docx/BwnbdVTJno6noZxxVOxcSpBknh4](https://datawhaler.feishu.cn/docx/BwnbdVTJno6noZxxVOxcSpBknh4)

# 前言

欢迎来到《魔搭创空间部署教程》！本教程旨在帮助小伙伴们顺利地在魔搭创空间平台上部署和运行自己的项目。

### 适用对象

+   对于希望通过CPU环境来部署项目的小伙伴，本教程提供了全面且详尽的操作指南。

+   魔搭创空间部署教程是按照CPU编写的，使用GPU加速计算的小伙伴也可以从本教程中获得有价值的参考信息，尽管GPU的具体配置和优化步骤可能有所不同，但大部分基础概念和技术框架是通用的。

### 注意事项

+   [Task4：源大模型微调实战-知识点文档](https://datawhaler.feishu.cn/wiki/PLCHwQ8pai12rEkPzDqcufWKnDd?from=from_copylink)程没有讲解基础软件环境以及必要的账户注册流程，默认从 开始

+   教程是基于Task4 微调项目的上线，没有对Task3 RAG做相关测试，但上线的逻辑是相同的。

我们相信，通过本教程的学习，您不仅能够成功部署自己的项目，还能掌握更多实用技能，以便在未来面对更加复杂的任务时也能游刃有余。

# 部署教程

## 1.创建“创空间”

打开[ModelScope 魔搭社区](https://www.modelscope.cn/my/myspace)，然后点击“创空间”

![](img/b4fb581b070e279fd38353441b6c81ea.png)

点击“创建创空间”，一般大家是没有的创建过，所以不一样

![](img/cf4569cd57061c70816d6430032be319.png)

创建方式这里，我们选择“编程式创建应用”

![](img/da2b761a77f05fc77d5bdd383abc25f8.png)

创空间的信息填写

1.  基础信息

1.  空间英文名称

1.  空间中文名称

1.  空间描述

![](img/d475f0cf1fcf99033a8559df5dce7c70.png)

1.  配置

1.  是否公开：建议选择公开，这样大家方便观看

1.  高级配置

1.  接入SDK——Streamlit，如果你的项目是基于Task改的，那就是选择这个SDK

1.  空间云资源——CPU basic / 2v CPU / 16G，建议选择CPU，因为免费

![](img/b2cb6ce16b7ce37c35e01cc93662716d.png)

然后点击“发布应用”

## 上传到空间文件

> 这里以Task4案例：AI简历助手为例

下面我们来演示一下

### 打开PAI的交互式建模（DSW）

打开PAI的交互式建模（DSW）

![](img/16f09e35c8ea7f36375550f4f6b61a00.png)

### 拉取git

打开命令行窗口，输入下面命令（第二行，填写自己注册的创空间地址，以及自己的Token）

```
git lfs install
git clone http://oauth2:&lt;your_access_token&gt;@www.modelscope.cn/studios/&lt;your_studio_path&gt;/&lt;your_studio_name&gt;.git
```

![](img/6df24b26c03c66a0798f0e70c4c5e337.png)

然后cd 进入git到的文件夹

![](img/40dc1da5dbf7d2e2bcddba3a2cf06874.png)

代码解释：

1.  **安装 Git LFS**:

```
git lfs install
```

这条命令用于在你的计算机上安装 Git LFS。Git LFS 是一个 Git 扩展，用于替代 Git 大文件管理方式，使得 Git 仓库更高效地处理大文件。

>

1.  克隆带有 Git LFS 的远程仓库:

```
git clone http://oauth2:&lt;your_access_token&gt;@www.modelscope.cn/studios/&lt;your_studio_path&gt;/&lt;your_studio_name&gt;.git
```

这条命令用来从 ModelScope 中克隆一个包含 Git LFS 的仓库。你需要替换 <your_access_token> 为你的访问令牌，<your_studio_path> 和 <your_studio_name> 分别为你的工作室路径和名称。

>

> 大家可以直接复制自己创空间主页的代码，记得勾选“使用我的Token”

![](img/ee16e75b34b41cd1d211af335d35228a.png)

### 迁移模型文件

迁移模型文件，这是我们的文件目录是这样的：

![](img/520b8554fb5a6e4e02aecf0462c1aa0c.png)

我们选择要把这三个文件迁移到 WaveTrans_Yuan_sl/ 文件夹内，这个文件夹是自己创空间的英文名

![](img/693fdec768e763687faa86bbd795ce9b.png)

迁移后的样子：

![](img/f534c45af33420e5c6b5d761ec654010.png)

### 修改文件

现在我们需要把py源文件改成可以在CPU上使用的，这里使用AI，大佬可以选择手动修改，AI有一定随机性。建议通义千问，因为输入的最大字符为10000，且是免费的。

输入提示词：

```
为了使下面这段代码能够在CPU上运行而不是GPU上，你需要对代码做一些调整，并输出所有代码。
主要的变化包括去掉与CUDA相关的代码，并确保模型和张量都放在CPU上处理，请不要修改其他代码模块！
记得输出所有代码，这对我很重要。最后告诉我，你修改的模块有什么。
# py源代码
```

![](img/0cac2cd9688fae3b343f5a3d88a45434.png)

![](img/61753652c2b7ceb30e4b20a8d21f2c55.png)

然后把py文件的文件名改为app.py，并上传requirements.txt文件（按照大家自己的项目更改）

![](img/bb32ca8e2c96a202d3757c1c7de01190.png)

[requirements.txt](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IwivbUaMqoNGDNxpzY9clLLKnNh)

![](img/196e958cb2aa710489762ce4cc876f21.png)

### 添加文件和文件夹到仓库

使用 git add 命令来添加文件和文件夹，我们来逐一添加每个文件和文件夹，输入下面命令

```
git add IEITYuan
git add output
git add app.py
git add requirements.txt
```

![](img/5eac81ad2d20170a70f8d1b4f89c792d.png)

### 提交更改

假设你的电子邮件地址是 example@example.com，你的全名是 John Doe 输入（这里是可以随便输入的，符合格式要求就行）：

```
git config --global user.email &quot;example@example.com&quot;
git config --global user.name &quot;John Doe&quot;
```

```
git commit -m &quot;Add IEITYuan, output directories, app.py, requirements.txt file&quot;
```

![](img/79447e9704667fbe01e829e13556514b.png)

### 推送更改到远程仓库

最后，将这些更改推送到远程仓库：

```
git push
```

![](img/bd12e7d643473d3f0375d1d9be2f5988.png)

## 设置创空间

打开自己创空间的设置页面

![](img/1de8ffc491197a408b9767ea63ee7dcc.png)

点击“上线空间展示”

![](img/96854b9b9648524cb739a39a346233ff.png)

在主页，等待一会，就完成了：

![](img/62ea34e490a4bb9bf42953b7f9be5145.png)

# 作品提交方式

如果完成了作品开发，可以提交初版作品啦，加油

<https://datawhaler.feishu.cn/share/base/form/shrcnbLAWydDXkK3lFwbIQBecCg?iframeFrom=docx&ccm_open=iframe>

> [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/Gf5DwJNXeiLOjDkMlPhcnwjTnMf)任何想法都可以在学习群里交流或在   进行填写哟~

>

> 本教程主要由   写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 ****署名—非商业性使用**—相同方式共享 4.0 协议国际版许可协议进行许可。