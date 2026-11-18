
# 把一群大模型关在电脑里替你打工——Dify部署教程

> 来源：[https://datawhaler.feishu.cn/docx/Nv5RdA75joYPepxsYONcShtHnRe](https://datawhaler.feishu.cn/docx/Nv5RdA75joYPepxsYONcShtHnRe)

阅读使人充实，会谈使人敏捷，写作与笔记使人精确。 —— 培根

> 大家都用过大模型吧，也就是俗称的“AI”。通常我们使用大模型都是在网页端，比如[DeepSeek](https://chat.deepseek.com/)、[ChatGPT](https://chatgpt.com/)、[QwenChat](https://chat.qwen.ai/)、[文心一言](https://yiyan.baidu.com/)等等。

> 但是你会发现，有些时候我们在网页端问问题，或者玩角色扮演的时候，模型偶尔会拒绝我们的回答。有时这并不是模型本身的限制，而是来自“系统提示词”：一段在你问出第一个问题之前，就已经叮嘱过大模型的提示。

> 这个系统提示词是无法删掉的，因为一切都在网页后台发生。

> 此外，模型本身其实还具有一些高级参数，可以调整模型的创造力、活跃程度、严谨性和回复长度。这些也是在网页端无法调整的。

> 除此之外，你可能还想让大模型结合你本地的一些文件，比如日记、个人信息与简历、课程设计要求、学校下发的制度与奖学金文件等等内容，来生成个性化的回复。这也是目前的网页端很难做到的。

> 但这些，都能在Dify平台，通过接入API实现！

> 你甚至还可以把来自不同提供商的各类模型一起集成进你的Dify，实现一群大模型为你一个人打工！

> 如果你还想把你搭建的Dify平台分享给舍友，还可以邀请他加入你的工作区，一起使用！

> [Dify](https://dify.ai/zh) 是一款开源的大语言模型(LLM) 应用开发平台。即使你是非技术人员，也能参与到 AI 应用的定义和数据运营过程中，无需编写任何代码！

> Dify 社区版支持个人免费部署到本地！

> Dify 内置了构建 LLM 应用所需的关键技术栈，包括：

1. 对数百个模型的支持
1. 直观的 Prompt 编排界面
1. 高质量的 RAG 引擎
1. 稳健的 Agent 框架
1. 灵活的流程编排

那这么好用的东西，我要怎么在自己的电脑上部署并使用呢？

很简单！只消十分钟即可！

> 本教程无需任何🪜或🪜工具，纯国内网络环境即可实现。

## 在所有事情开始之前想告诉你的

在开始动手之前，我们先用一个简单的比喻，来理解一下这次教程中涉及的东西吧！

**Dify：**就像前文提到的，本次的主角，用来承载和加工调教各个大模型的平台。

**大模型：**会以模型提供商的API的形式，接入Dify。Dify本身是不带模型的，需要接入模型才可以使用。

**`Docker`：**部署Dify的时候用到的实用工具，可以把原先需要配置很久的各类环境打包起来，时间缩短到几分钟。

**`Git`：**用来从Github拉取部署Dify时需要的文件，比如Docker配置。

而我们的流程，大概是这样：先下载`Docker`这个工具本身，然后使用`Git`从Github拉下来Dify的配置文件，再依照这个配置文件，用刚才安装的`Docker`把Dify本体拉下来，然后启动。

拉下来的意思就是下载到本地。

启动之后，就可以给Dify添加大模型了。这部分在结尾。

千里之行，始于足下，现在我们开始吧！

整个流程对于初学者可能有一定难度，但是只要坚持下来跟着教程一步一步走，一定可以成功！

作者想说：你坚持做一件事的样子，真的很迷人！

如果在整个过程中卡在了某个地方，可以试试查看本教程最后一个章节：常见问题与解决方案。

![](img/8cdecb59a34ebd1d94e871a397fb5ee2.png)

## 第一步：安装 Docker for Windows

### 选看：什么是Docker？（点击展开）

1. 先想象一个场景

假设你是一个厨师，你需要做一道菜。这道菜需要很多材料，比如面粉、鸡蛋、调料等等。为了让这道菜在任何地方都能做出来，你会把这些材料打包成一个“食材盒子”，里面包含了所有需要的材料。

这样，无论你去哪个厨房，只要打开这个盒子，就能做出一样的菜，而不需要担心厨房里有没有这些材料。

1. Docker 是什么？

Docker 就像这个“食材盒子”，但它不是用来做菜的，而是用来运行软件的。

+ 在电脑上运行软件时，通常需要安装很多依赖（比如特定的库、环境、配置文件等）。如果换一台电脑，可能因为环境不同，软件就跑不起来了。
+ Docker 把软件和它需要的所有东西（代码、环境、依赖等）打包成一个“盒子”，这个盒子叫做 容器（Container）。
+ 有了这个容器，你就可以在任何电脑上运行这个软件，而不用担心环境问题。

### 另一个关于Docker的比喻（点击展开）

想象一下，你加盟了一家奶茶店，叫蜜雪冰橙。

这家品牌提供好多种饮料！奶茶、茶、果奶、咖啡、冰淇淋，应有尽有~

但是，蜜雪冰橙加盟，只是提供开店指导和品牌使用权，不提供物料！

这意味着，在奶茶粉、咖啡豆、牛奶、椰果、珍珠等等等等的物料上，你需要自己找上游经销商购买！

这就是Docker出来之前，程序员在部署环境时的状况了：不同服务（饮品）依赖的环境版本（原料）都不相同，如果自己慢慢配的话，麻烦不说，还有可能发生冲突（比如进的牛奶只适合做果奶，但打出的冰淇淋没有奶味）。

为了解决这种问题，蜜雪冰城来了！雪王加盟提供全套的技术指导和物料支持~奶茶粉、咖啡豆、牛奶、椰果、珍珠都打包好了，每周按时运到你的店面，你只需要加工就好了！

这也就是Docker做的事情：将某个项目里面需要涉及到的所有服务，以及这种服务需要依赖的环境，全部打包起来，一股脑下载到电脑上，直接运行就好了，省去了自己准备的麻烦！

### 1.1 下载安装Docker

首先，进入Docker官网：

选择 `Download Docker Desktop` -> `Download for Windows - AMD64` ，如图所示下载Docker桌面版：

![](img/f05c70230b12c6644a314a4840980360.png)

在没有🪜的情况下，下载速度有些慢......（250kb/s左右）

如果觉得太慢，可以直接下载下方的安装包！

![](img/c1fac399ef53697cf2775a2a21442327.png)

`Docker Desktop Installer.exe`：[https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/I2Bhb5yUGoIBiDxYiBAcDC0rnPh](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/I2Bhb5yUGoIBiDxYiBAcDC0rnPh)

下载完成后，按如下步骤安装：

![](img/05edc64e1458641969bba12d53e181d8.png)

有的同学在安装时，可能没有这里的第一个复选框，这是正常现象，继续安装流程即可。

![](img/084c4b18ca325aed03edd407cd7c0526.png)

大约3分钟安装时间，具体速度取决于电脑型号和配置~

安装完成后，会显示如下界面：

![](img/8a5797fbe4dd1c0421cd0d3766b3b5ed.png)

重启完成后，Docker的安装就完成了！

据可靠统计，哪怕是计算机专业的学生，也有很多是在大三大四才接触到Docker的，更有甚者大学四年都是只闻其名，而从没有用过（可以尝试问问自己的学长学姐）。所以，学到这里的你，大约已经超过了75%的科班大学生！

不过，在实际使用中，我们通常会在Linux系统上运行Docker。所以如果你有兴趣，也可以在后面尝试自学Linux！

### 1.2 初始化Docker

首先，打开桌面上新安装的`Docker Desktop`软件（相信你之前一定已经重启过了，没有重启过一定要重启，不然这一步有可能会打不开，或出意外错误）。

第一次启动Docker，你会看到：

![](img/ad92c477ee63079cc880998ce72d715e.png)

前面的章鱼和鲸鱼提示卡，是在向你咨询是否同意用户协议，直接点`Accept`接受即可。

垫在后面的那个黑色窗口，是在尝试安装运行Docker时所需要的`WSL`（如果直接重启的话，它会先出现）。选中该黑色窗口直接敲一下回车即可。

![](img/1b1bfa11ccdbcf2f5bd8e28658d4583e.png)

这样就下载完成了。切记：这一步结束之前，不要继续下面的动作。

当黑色`PowerShell`窗口执行结束后，再继续处理Docker的询问：

![](img/c2ab0d64f9bd010c13876660520cd524.png)

点击红框的无需注册直接使用：

![](img/e49bbd6ea568a6a5b772ecebd52c20b4.png)

![](img/bd83186d37d8cce1e751363c33f0b9e5.png)

当你进到这里，说明已经彻底完成Docker安装了！

## 第二步：克隆 Dify 代码仓库

克隆，其实指的就是下载。之所以用克隆这个词，主要有两个原因：

1. 强调这个下载的过程是“在原样的基础上完整复制”。
1. 这个过程通常伴随着`git clone`仓库这个过程，于是就直译clone这个单词了（这个原因最根本）。

而现在我们需要克隆的，是在**GitHub**这个代码托管平台上托管着的 [Dify仓库](https://github.com/langgenius/dify)。

### 选看：什么是 GitHub？（点击展开）

GitHub 是一个基于 `Git` 的代码托管平台，简单来说：

+ 它是一个存放代码的地方（就像网盘存文件一样）。
+ 它可以帮助多人协作开发项目（比如一起写代码）。
+ 它可以记录代码的每一次修改，并随时还原状态（就像写小说的“魔法笔记本”）。

任何成熟的软件工程师都需要使用版本控制工具，而基于`Git`的GitHub是其中最流行的那个。如果你对此有兴趣，可以先注册一个账号，创建一个仓库，上传一些代码试试。慢慢你就会发现它的强大之处！

据可靠数据，至少有80%计算机相关专业的学生，在整个大学都没有真正在GitHub上面上传过一个能跑起来的完整项目。如果假以时日你做到了这一点，真的要恭喜你！

详细说明：

1. 先想象一个场景

假设你和几个朋友一起写一本小说。你们每个人负责写不同的章节，但最后需要把所有人的内容合并成一本书。如果大家都用纸和笔写，可能会遇到这些问题：

+ 即使你们一起聊过，但是你写的剧情和朋友写的还是可能有冲突。
+ 你不知道朋友改了哪些地方，还要一点点找。
+ 如果谁不小心把纸弄丢了，内容就没了！
+ 万一先前某个剧情写的不好，想修改却发现纸上没空改了。

这时候，如果有一个“魔法笔记本”可以帮你们：

+ 随时保存每个人的修改。
+ 自动合并大家的内容。
+ 记录谁改了哪里，什么时候改的。
+ 即使笔记本丢了，内容也不会丢。

这个“魔法笔记本”就是 `GitHub` 的核心功能！

---

1. GitHub 是什么？

GitHub 是一个基于 `Git` 的代码托管平台，简单来说：

+ 它是一个存放代码的地方（就像网盘存文件一样）。
+ 它可以帮助多人协作开发项目（比如一起写代码）。
+ 它可以记录代码的每一次修改，并随时还原状态（就像写小说的“魔法笔记本”）。

---

1. 举个例子

假设你和朋友一起开发一个网站：

+ 你把代码上传到 GitHub，朋友可以下载你的代码。
+ 朋友修改了代码后，把修改的部分上传到 GitHub。
+ 你可以看到朋友改了哪些地方，决定是否接受这些修改。
+ 如果修改有问题，还可以回到之前的版本。

这样，你们就可以高效地一起开发，而不会把代码搞乱。

---

1. GitHub 的核心概念

+ 仓库（`Repository`）：存放代码的地方，可以理解为一个项目的文件夹。
+ 提交（`Commit`）：每次修改代码后，可以保存一次“快照”，这就是提交。提交会记录谁改了哪里。
+ 分支（`Branch`）：可以创建一个独立的分支来开发新功能，不会影响主代码。
+ 合并（`Merge`）：把分支的修改合并到主代码中。
+ 拉取请求（`Pull Request`）：当你修改了代码，可以发起一个拉取请求，让别人审核你的修改，然后合并到主代码中。

---

1. 为什么 GitHub 这么火？

+ 协作方便：多人可以同时开发一个项目，不会互相干扰。
+ 版本控制：可以随时查看历史记录，回到之前的版本。
+ 开源社区：GitHub 上有无数开源项目，你可以学习别人的代码，也可以贡献自己的代码。
+ 备份安全：代码存放在云端，不用担心丢失。

### 2.1 下载Git

克隆代码仓库，首先需要安装能做这件事的工具——`Git`，用来把GitHub上面的仓库克隆下来。

可以到Git官网下载`Git`：

![](img/8019168623b6a00988c421be835b65bc.png)

如果你没有🪜，大概率下不下来或者下载很慢，这时可以直接下载下方的安装包：

![](img/cfa6db6f829dcbfd6878efcc0a1fa5df.png)

`Git-2.48.1-64-bit.exe`：[https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TTOqbqWjqoHpctx5oRQc5aY4ncx](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TTOqbqWjqoHpctx5oRQc5aY4ncx)

下载好后，双击安装，安装界面如下图所示：

![](img/d0818e9676ebd421e49af54e1ecea6ae.png)

看不懂吧？没关系，大家都看不懂，直接一路默认一直点`Next`下一步就好（

除了下面这个界面需要注意一下：

![](img/51913f52d6a32cb2436004fa21f74440.png)

[选看：VS Code安装教程](https://dcndgqtl531e.feishu.cn/docx/SFQ8dBiwNoKsBMx8NvQciCVOn5e?from=from_copylink) 如果电脑上还没有安装`VS Code`，可以参考这篇教程：

安装结束后，请你在桌面或者随便哪个文件夹的空白处，按住`shift`并点击右键，然后在右键菜单中选择“在此处打开`PowerShell`窗口”（win11也可不按`shift`键直接右键，并选择“在终端中打开”）。

![](img/f70e4bab998e4fbf11eda3fd38cd3d7d.png)

一定要按住`Shift`点右键，才会有这个选项！

打开`PowerShell`后，输入`git --version`（注意`git`后面的空格哦）

![](img/007eb8c3f22bcccc9a00873aae145fd6.png)

如果你的显示也类似上图，那么恭喜你安装成功！

### 2.2 克隆仓库

打开一个你方便放Dify代码仓库的文件夹（最好有10G左右的空间），在文件夹的空白处，按住`shift`并点击右键，然后在右键菜单中选择“在此处打开`PowerShell`窗口”。

![](img/50fa7d48bcebdfa8920d75f4e8c9729f.png)

打开后，输入以下指令并回车（建议直接复制）：

```
git clone https://github.com/langgenius/dify.git --branch 0.15.3 # 从GitHub拉取0.15.3版本的Dify（为了和后面的Docker版本对齐）
```

![](img/d4074d5de62cb7b33dab7fefe7b0b889.png)

出现以上回显则为克隆成功！

> 如果此时报错飘红，或者卡住，大概率是因为GitHub在国内的访问不通畅。

> 没关系！你可以手动删除刚才克隆下来的`dify`文件夹，并用下面这个压缩包替换掉。记得解压！

![](img/9e63ba13851c4b75fa45dbcd47e81460.png)

`dify.zip`：[https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TmzebzFPbo728cxEIOrc9RTPnqf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TmzebzFPbo728cxEIOrc9RTPnqf)

完成后，先不要关闭窗口，我们后面还会用到~

## 第三步：拉取并启动Dify的Docker镜像

接下来就是本教程的重头戏了：把`Docker`跑起来！

### 3.1 配置文件

在正式运行之前，我们还需要修改一些配置文件。

在刚才的`PowerShell`窗口中输入以下命令（直接复制粘贴即可）：

```
cd dify                  # 进入刚才克隆下来的dify文件夹
cd docker                # 打开其docker目录
cp .env.example .env     # 复制一份示例的环境配置文件作为正式配置
```

如果你不知道应该在哪里输入指令，或者输入后报错，请点击展开下方的补充说明。

#### 【补充说明】

![](img/e601cff428ef14b1d4678ddb9e6f82ab.png)

而`cd`指令可以在`PowerShell`窗口里进入一个文件夹：

![](img/76e1797abf1a2502e7bc5d9114089d2c.png)

比如在 `C:\示例文件夹\dify`下执行`cd docker`，意思就是打开了这个`dify`文件夹下的`docker`文件夹。

而下面的指令，其实本质上是在`PowerShell`里面打开了刚才下载下来的`dify`文件夹里面的`docker`文件夹，然后把`docker`文件夹下面的`.env.example`文件复制一份，并改名为`.env`。



所以如果我们手动打开`docker`文件夹，然后按住Shift点右键，打开一个新的`PowerShell`窗口，其实和下面在原先位置执行两次`cd`命令的效果是一样的，本质上都是要在`PowerShell`路径为`xxx\dify\docker`的时候执行一句`cp .env.example .env`命令。

同理，既然我们的目的是在`xxx\dify\docker`路径执行一句`cp .env.example .env`命令，那下面的`cd dify`和`cd docker`可以合并为一句：`cd dify/docker`

甚至，你可以不执行这个指令，直接手动打开`dify`中的`docker`文件夹，然后手动复制一份`.env.example`并改名为`.env`，也是一样的效果。

不知道上面的内容对你是否有帮助？

随后，我们还需要改一个细节，为了之后搭建功能强大的Agent智能体铺路

打开`dify\docker\ssrf_proxy\squid.conf.template`文件（使用`VSCode`或记事本），把原先文件中左边的内容改成右边：

```
http_access deny !Safe_ports
http_access deny CONNECT !SSL_ports
http_access allow localhost manager
http_access deny manager
http_access allow localhost
include /etc/squid/conf.d/*.conf
http_access deny all
http_access allow Safe_ports
http_access allow CONNECT SSL_ports
http_access allow localhost manager
http_access deny manager
http_access allow localhost
include /etc/squid/conf.d/*.conf
http_access deny all
```

写给已经在本地的`80`端口部署过网页的同学，如没有部署过可以不看（点击展开）

如果你在本地`80`端口部署过服务，为了使`Dify`和你之前的网页不冲突，你需要修改`Dify`的默认端口。

打开`dify/docker/.env`文件（使用`VSCode`，记事本也可以凑合一下），在大约第`906`行附近找到`EXPOSE_NGINX_PORT=80`，并将`80`改为另一个端口，如`89`，并保存文件。

如果你在这里设置了`89`端口，那上面提到的`dify\docker\ssrf_proxy\squid.conf.template`文件中，你在修改完基础部分后，还需要额外在`acl Safe_ports port 80                # http`这一行后面添加一行：`acl Safe_ports port 89                # http`。

配置文件部分大功告成！

## 3.2 加载并启动项目

接下来就是重头戏：加载并构建镜像

与`GitHub`类似，`Docker`的官方源在国内也基本不可用...

不过没关系，我们可以使用国内的镜像源来拉取`Docker`镜像预先下载好的文件直接从本地加载！

首先下载下方的镜像压缩包，并解压到`dify\docker`目录下：

![](img/a63fd436a8af7d340b325e0184098084.png)

[`docker.zip`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SCPcbcLxioBsdrxOKajcRwExnLf)

解压好的文件结构应该如下图所示。从`docker.zip`解压出的各种`xxx.tar`直接在这个`docker`文件夹下。

![](img/22e1a58e3a71b6c90a38c162d0d32829.png)

随后，在刚才的`PowerShell`窗口中继续执行以下指令：

（这一步会比较久，取决于电脑配置，因为`Docker`在加载大文件，一般会在10分钟内完成~）

```
docker load -i nginx.tar
docker load -i dify_api.tar
docker load -i dify_plugin_daemon.tar
docker load -i dify_web.tar
docker load -i redis.tar
docker load -i postgres.tar
docker load -i squid.tar
docker load -i dify_sandbox.tar
docker load -i weaviate.tar
```

接着使用`docker compose`启动所有服务！

（这里也需要一点时间）

```
docker compose up -d
```

如果没出问题的话，你会看到这样的输出：

![](img/40961d277288a1ccdc645ec7b8d33717.png)

大功告成！

现在你的电脑上正在运行着一组`Docker`镜像，他们互相交织着组成了`Dify`的全套服务。

那我们要如何访问`Dify`呢？

请看——

## 第四步：访问Dify

打开浏览器，输入`http://127.0.0.1/`（前面改过端口的小伙伴需要输入`http://127.0.0.1:89`），回车，你会看到如下界面：

![](img/509615610d139834b0e6f4122bd0c0c6.png)

多么美妙的本地服务啊！现在你可以新建一个管理员账户了，记得不要忘了密码~

新建，登录，你就会看到——

![](img/d6ad49b920fb530687a2bd81ae58021b.png)

一个完整而优雅的`Dify`社区版铺就在眼前！

[接下来，如果你有](https://dcndgqtl531e.feishu.cn/docx/HaPmd0NB7oBe4xxcbDJc3kEmn5g)访问密钥，就可以在右上角的设置中添加大模型供应商了！

进一步教程，请参考其他篇目~

[模型提供商接入`Dify`](https://dcndgqtl531e.feishu.cn/docx/HaPmd0NB7oBe4xxcbDJc3kEmn5g)

## 常见问题与解决方案

### 执行`Docker`相关指令时报错：error during connect:Post........

![](img/7df8b75755909c1b35921526d6533fe9.png)

原因：电脑上安装的`Docker`忘记打开啦！双击桌面图标，打开`Docker`就好了

### 启动`Dify`时报错：x `weaviateError` Get "https://registry-1.docker.io/v2/":net/http:request canceled while wait..

![](img/4662f21e6ab246a082dc71a198a3803b.png)

原因：漏掉了最后一句`docker load -i weaviate.tar`指令~执行这一句之后，再`docker compose up -d`试试？

### 拉取`Git`仓库时报错：fatal: could not create work tree dir '`dify`':Permission denied

![](img/d96b6252bbff10cec73e143147dfde16.png)

原因：没有在桌面或文件夹里按住Shift点右键，而是直接搜出来了管理员版本的`PowerShell`运行，导致权限不足

换一个自己在桌面或者其它盘新建的文件夹，重新按住Shift点鼠标右键，召唤出此处的`PowerShell`，就好咯

### 在初始化`Docker`的过程中，提示 `Docker Desktop-WSL` update failed

原因：

1.  是不是在适用于 Linux 的 Windows 子系统的安装`PowerShell`窗口没有完成任务前，就开启了`Docker`？

1.  国内网络问题...这个比较无解，大多数地区的网络都是可以正常执行这一步的，但是少数地区的网络非常闭塞，就会导致这一步`WSL`更新失败

如果你是问题一，解决方案如下：

首先在任务栏中选中并右键打开菜单关闭`Docker`

![](img/c167f3f3d130fb1fad810be89227c811.png)

随后等待黑色的`PowerShell`窗口正在执行的`WSL`安装任务结束；

结束后，在桌面重新双击打开`Docker`，即可正常启动。

如果你是问题二，解决方案如下（依次执行）：

1.  如果你是笔记本或带无线网卡的台式机，连接你的手机热点试试

1.  在系统的搜索窗口里面搜索`powershell`，并选择以管理员身份打开

![](img/276f389dc01f06101c5a7325bde0a965.png)

1.  输入以下指令：

```
wsl --update
wsl --install
```

可能需要下载一段时间。如果这里没有报错，安装完成后重启电脑，重新打开`Docker`，应该能够解决问题

这样一套操作下来，应该能解决70%问题

[如果](https://blog.csdn.net/qq_64574122/article/details/137267366)还是报错，可以尝试这篇博客的解决方案：windows11运行`Docker Desktop`，报错`Docker Desktop -WSL update failed`_`docker desktop - wsl update failed`-CSD

### 解压要会员怎么办？

这多半是因为电脑上安装了`联想电脑管家`，劫持了你的应用打开方式。

某些华为电脑上的电脑管家也有类似行为

![](img/3d35cc10abf2de6e5d766065d0197b68.png)

解决方案：卸载`联想电脑管家`与`联想应用商店`[（可选），并安装](https://www.bandisoft.com/bandizip/)其它正经的解压缩软件，如`BandiZIP`

下方提供了安装包：

![](img/c1692452183bf9e630fdde1469d76f30.png)

[`BANDIZIP-SETUP-STD-X64.exe`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NLWQbYMjgonU19xLYflcG8tMnhh)

选择安装评价版即可。

### 在启动`Dify`后，打开网页一直无法进入登录界面（`api`和`worker`容器反复失败重启）

![](img/64ca7854df2527ccb5eff2e401b34820.png)

![](img/7e666cd161285b637cff827ec8abba42.png)

原因：暂时未知，该问题并不常见，仅发生一次。

解决方法：尝试卸载并重新安装`Docker`，可能是一些关键组件安装过程中出错。

在此感谢@和乐（陌墨）的反馈！

### 其他错误

其实，在以上的五种错误之外，大部分问题都出在了文件路径上。

比如，解压文件的时候解压在了这个地方，但执行指令的时候，又在另一个地方执行，南辕北辙了

又或者，解压文件的时候选择了系统关键文件夹，没有权限

如果`C`盘空间还充足的话，非常推荐在桌面新建一个文件夹，所有的操作都在这个文件夹里进行

关于`PowerShell`中文件路径与文件夹路径的对应关系讲解，可以参看【3.1 配置文件】下面的【补充说明】

### 总结栏

1.  如果对本教程有建议、意见或者`Bug`反馈，请务必联系：🛰️号（Simuoss）或邮箱（simuoss@163.com）反馈！非常感谢！

1.  2025.2.23感谢HUY同学提出了两个易卡点，已优化

1.  2025.2.26感谢TXY同学提出7个优化思路，已优化

1.  感谢来自SUST的其他五位同学协助测试教程
