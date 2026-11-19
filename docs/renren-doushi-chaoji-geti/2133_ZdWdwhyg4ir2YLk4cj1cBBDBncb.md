
# 2023-08-11搞台虚拟机玩玩！

> 来源：[https://xuqiwei1986.feishu.cn/docx/R1mldKePtoGYx5x6mIIc7mFKnWg](https://xuqiwei1986.feishu.cn/docx/R1mldKePtoGYx5x6mIIc7mFKnWg)

> 🔗 原文链接：https://mp.weixin.qq.com/s/zOvx89QBhIPj5Cicbo90sA

原创 鱼皮 程序员鱼皮

2023-08-11 18:19:27

精确发文时间由壹伴提供

2023-08-11 18:19 发表于 上海

手机阅读

![](img/d2798d3e36b7d8d56bc4b4fef3926a26.png)

收录于合集

#计算机 217 个

#程序员 234 个

#互联网 58 个

#编程 220 个

#技术 42 个

> Linux 虚拟机 + 远程开发保姆级教程，轻松搭建高效开发环境！

大家好，我是鱼皮。最近我正在带大家开发`OJ`系统，需要用`Docker`容器技术来实现隔离的代码沙箱。

随之而来的是一个难题：很多同学的电脑都是`Windows`的，直接在`Windows`上安装`Docker`容器比较麻烦，需要依赖`WSL`（`Windows Subsystem for Linux`），很容易出现各种报错。

比如 `WSL` 内核版本太低：

![](img/3fab5c2658f17980752cea7aa8b68204.png)

缺少 `WSL`：

![](img/4908bce2ffdf0e76bec1175072b77cf6.png)

所以，我们不妨直接搞一台 `Linux` 系统的服务器来安装 `Docker` 吧，不仅不会影响现有的系统，以后开发和部署项目也都会更方便。

那么如何拥有一台 `Linux` 服务器呢？

比较常见的有 2 种方式：

+ 购买第三方云服务商的云服务器

+ 使用虚拟机软件，在自己电脑上额外运行 `Linux` 系统

第一种方式要花钱，就不多说了。本篇文章我会带大家实践第 2 种方式，只需几分钟的时间，手把手带大家在 `Windows` 上安装 `Linux` 虚拟机，并且实践 2 种远程开发方法，让大家像使用 `Windows` 一样轻松地在 `Linux` 上开发程序。

⭐️ 本文对应视频教程：https://www.bilibili.com/video/BV1h94y1k7Jf/（讲解会更细节，建议配合本文使用）

## 一、虚拟机安装

### 1、下载安装

首先我们要下载虚拟机软件，这里比较推荐 `VMware Workstation Player`，个人不商业使用是 免费 的。

直接到 `VMware` 官网下载：

指路：https://www.vmware.com/cn/products/workstation-player.html

![](img/c3c970dcba62207d4711515d5aac606b.png)

这里有条件的朋友也可以使用 `VMware Workstation Pro`，功能更全，但是要付费。

在官网点击免费下载后，即可进入安装流程。

这里要注意勾选 `自动安装 Windows Hypervisor Platform` 和 `将 VMware Workstation 控制台工具添加到系统 PATH` 两个选项：

![](img/42968ecd680c0649ad75871ea3a7d37c.png)

![](img/c5274f4ebcd2ff3a60895867a7d3e5e3.png)

选择免费、非商用版本：

![](img/60b518d8a1a68d04d141af3ae8b11ae9.png)

然后就安装成功了，比起在 `Windows` 上折腾 `WSL`，安装虚拟机可方便太多了~

### 2、安装系统

虚拟机软件安装完成后，我们打开软件，接下来要先新建虚拟机来安装一个 `Linux` 操作系统：

![](img/20637b9967c152a4a6cf2517098f6c6e.png)

点击创建新虚拟机，然后它让我们提供一个 `Linux` 操作系统的镜像：

![](img/630d41f3f16bf6008af616a3117fd328.png)

问题是，`Linux` 操作系统有很多的发行版，我们安装哪个版本呢？

我学生时代的时候，特别喜欢用 `CentOS`，但可惜的是，`CentOS` 已经停止维护了。。。

所以这里我们选择 `Linux Ubuntu 18.04 LTS` 版本来给大家演示，有图形界面，会更能照顾到新手同学。

`Ubuntu` 镜像下载：https://releases.ubuntu.com/18.04/

拉到底部，找到 `.iso` 后缀的文件下载即可：

![](img/7b85d4d1a261f114a7899980ff7a8fe4.png)

当然，萝卜青菜，各有所爱，会玩 `Linux` 的同学可以自由选择版本~

指定完操作系统后，我们继续新建虚拟机，可以根据实际的电脑情况来自定义虚拟机的硬件。如果你的电脑配置还不错，内存和 `CPU` 可以分配多一点：

![](img/dfffec925e72917d29eecdcc1c6fa44e.png)

点击完成，然后稍等片刻，系统就安装完成并启动啦！

![](img/08d764a4f0e9093ec0863c8e0befc02a.png)

### 3、修改配置

像购买一台新电脑一样，我们有了新的操作系统后，可以自行修改一些配置，比如修改语言为中文。

#### 修改语言为中文

按 `windows` 键，弹出功能搜索按钮，输入 `language`，打开 `语言支持`：

我这台虚拟机是已经改完语言的，所以截图中已经是中文了

![](img/0b4791fcba5d7221667f79148a821da6.png)

然后点击添加语言，选择中文简体，并且把汉语拖到语言列表的最上面，最后点击应用到整个系统：

![](img/71325799b73c16e706336250e1cab8da.png)

稍等片刻，中文语言包、输入法等内容就安装好了，点击右上角重启虚拟机：

![](img/728bdc226df9707d43a383f6c19f507d.png)

重启后，同样是按 `windows` 键，弹出功能搜索按钮，输入 `language`，打开 `区域和语言`：

![](img/b07dd10c38ead26dbc2a65d371e0655f.png)

在 `区域和语言` 设置中，新增输入源，添加已安装的中文输入法：

![](img/100cafa4993c28f90873246895ac7618.png)

#### 修改分辨率

用同样的方法，我们可以调整系统的分辨率。

先打开显示设置：

![](img/ecd61ffe5a40d5616f2227642d7bce69.png)

然后可以调整分辨率和字体大小：

![](img/beb82696a64281460fa24c539d714f70.png)

#### 修改时间

默认情况下系统的时区可能是有问题的，我们可以打开 `日期和时间` 设置，选择时区为 `中国上海`：

![](img/c2a04442ed79d1e75960f8a9fc48edc2.png)

### 4、安装软件

基本配置修改完成后，我们就可以给 `Linux` 系统安装软件并使用了。

和其他操作系统一样，`Linux Ubuntu` 也有自己的应用商店，能够可视化一键安装软件，比如防火墙之类的，适合新手操作：

![](img/3aeb15451a4f5ac5073d6bbbb0a99676.png)

当然，大家用 `Linux` 更多地还是要学习它的终端命令行操作，所以这里我们以 `Docker` 为例，使用命令行来快速安装。

按 `Ctrl + Alt + T` 快速打开终端，输入以下命令来一键安装 `Docker`：

`apt` 命令是软件包管理工具，能够让我们方便地安装软件

```
sudo apt install docker.io
```

如下图：

![](img/b6839abfab5eeba3ddfa0604276cbbcd.png)

安装完成后，输入 `docker -v` ，查看版本号，如下图：

![](img/a7f81f9e92b33b6a3fd8772729f41182.png)

然后输入 `docker run` 命令，测试 `Docker` 能否正常运行：

```
sudo docker run hello-world
```

能看到如下信息，表示 `Docker` 安装成功：

![](img/9895369041fdc94bf2d728232a8ba0d2.png)

怎么样，比在 `Windows` 安装方便多了吧？

## 二、Java 远程开发

有了 `Linux` 系统后，我们怎么在 `Linux` 上开发呢？

是直接在 `Linux` 上安装常用的开发工具，然后在虚拟机里编写代码么？

No、No、No，这样不仅要改变你原本的编码习惯，还可能会出现卡顿。如果你的项目有的在 `Windows`、有的在 `Linux`，需要同时开发的话，来回切换系统可太麻烦了。

所以这里我会教大家使用远程开发，直接在自己的 `Windows` 电脑上操作 `Linux` 服务器，和之间的开发方式完全一致！

两个字：“丝滑”！

下面我会以 `Java` 开发为例，带大家掌握 2 种远程开发的方式：`远程部署` 和 `纯远程开发`，大家可以根据实际情况选择合适的方式。

### 1、前置准备

在学习远程开发前，我们要先做一些准备工作。

#### 1）网络连通

要确保我们本地的 `Windows` 电脑能够连接到 `Linux` 虚拟机。

首先在设置中搜索网络，点击查看网络设置：

![](img/2363e3ff6eb3c530ecd1befef6a3ae1f.png)

获取到虚拟机的 `IPv4` 地址，这个地址一定要记好了：

![](img/ebbc0006c9e478c3077871aeb96fc65e.png)

在自己的 `Windows` 电脑上 `ping` 这个 `IP` 地址，测试能否 `ping` 通。

如下图，表示网络正常连通：

![](img/69207c1089c217274f1c47de716e196c.png)

#### 2）开启 SSH

光能 `ping` 通虚拟机还不够，很多远程开发工具都是通过 `SSH` 协议连接远程服务器的，所以我们要在 `Linux` 虚拟机上开启 `SSH`。

在 `Linux` 虚拟机上打开终端，输入下列命令安装 `SSH` 服务器：

```
sudo apt-get install openssh-server
```

安装后，输入下列命令检查 `SSH` 是否已开启：

```
ps -e |grep ssh
```

如下图，则表示 `SSH` 开启成功：

![](img/abdb23002c59f6f00f9dc270ec25d7cf.png)

如果未启动，执行 `sudo service ssh start` 手动启动服务试试

#### 3）安装 Java

因为我们要演示 `Java` 远程开发，所以 `Java JDK` 的安装是必不可少的。

`Linux` 下安装 `JDK` 还是很方便的，首先打开终端，输入命令来更新软件源信息：

```
sudo apt update
```

然后执行下列命令来安装 `Java 8`（经典版本）：

```
sudo apt install openjdk-8-jdk
```

安装完成后，输入命令检查 `Java` 版本：

```
java -version
```

看到如下输出，表示 `Java` 安装成功：

![](img/fabed4b92d5259c9217c989fbbce6f8a.png)

#### 4）安装 Maven

除了安装 `Java` 外，我们还要安装 `Java` 的项目依赖管理工具 `Maven`，便于接下来我们测试 `Spring Boot` 项目的远程开发。

打开终端，输入如下命令安装 `Maven`：

```
sudo apt install maven
```

安装完成后，输入命令来查看 `Maven` 的版本号：

```
mvn -v
```

看到如下输出，表示 `Maven` 安装成功：

![](img/6c8bdd14387adf955d1e82ca6583ca3e.png)

环境准备完毕，接下来开始远程开发之旅~

### 2、远程部署

第一种远程开发方式是 `远程部署`，就像我们真实做项目时，先在本地用代码编辑器完成开发，再把代码放到 `Linux` 服务器上去部署一样。`远程部署` 的思路是在本地编写代码，然后把本地的代码文件定期同步到 `Linux` 服务器，再用本地电脑操作远程服务器完成部署和运行。

如下图：

![](img/25a690cbf996d1aeafd36861abc96302.png)

这种方式更多地是为了提高将代码放到服务器部署的效率，不算是真正意义上的远程开发，但可以达到接近的效果。

远程开发主要分为 6 个阶段：编码、文件同步、运行、编译构建、部署、调试。接下来让我们以一个最简单干净的 `Spring Boot` 项目为例，实战上述几个操作。

#### 0）编码

假设我们本地已经有了一个示例项目，仅提供一个 `checkHealth` 接口，用于测试服务能否正常访问，端口为 `8080`。

核心代码如下：

```
@RestController("/")
public class MainController {

    @GetMapping("/health")
    public String checkHealth() {
        return "ok";
    }
}
```

#### 1）文件同步

打开 `JetBrains IDEA` 开发工具，点击 `Tools => Development => Configuration`：

![](img/c4fd28d0a16e3c4825cda87144bb9728.png)

进入到如下操作界面，需要先来配置 `SSH`，和服务器建立连接：

![](img/0e05a67093228727722c7a9a1711d1be.png)

进入 `SSH` 配置，输入自己实际的服务器 `IP`！不一定和下图的 `IP` 地址相同！

![](img/fe290466b6fa2d6dd21c23f5f8c2546d.png)

点击测试连接，能看到连接成功的弹窗；如果报错，可能是 `IP` 地址、用户名或密码输错了。

![](img/f6d167a17a5a8c4365f0fb45cc63c76d.png)

回到部署界面，点击 `Mappings` 来配置路径映射，就是把本地电脑的文件同步到远程服务器的指定路径：

![](img/d876d4626602bcfc1c2fc1be9413d5be.png)

保存，在 `IDEA` 右侧就能看到我们虚拟机的所有文件列表啦：

![](img/5c8c8470a8222eee14cbf8c3f328aaf5.png)

然后点击 `Tools => Development => Automatic Upload`，开启自动同步：

![](img/0fc0552c13945bf711309848b679d90f.png)

现在，尝试在自己电脑的代码文件夹中新建一个文件，该文件就会自动同步到 `Linux` 虚拟机~

但目前如果你删除了自己电脑的文件，`Linux` 虚拟机的对应文件不会删除，所以我们还要进入 `Options` 配置：

![](img/fd17f2713edb39f3edb9b7100f7f2dee.png)

勾选上文件删除的同步：

![](img/9dcb62c504621d933148aa031e9219af.png)

至此，你在本地编写的代码都会自动同步到服务器上了。

#### 2）运行项目

可以直接在 `IDEA` 的终端中快速创建一个能够远程访问远程服务器的终端：

![](img/ff52a343d0fb0868dd4d04ad661a951c.png)

然后使用 `cd` 命令进入到代码目录：

![](img/f1811abd91f13f4514cfd5ccbd6166b0.png)

输入 `Maven` 命令来运行项目：

```
mvn clean
mvn spring-boot:run
```

能够看到项目成功启动：

![](img/1983d996124d4b312bb9d3dfd609af4d.png)

然后访问远程服务器的接口地址（比如 `http://192.168.170.132:8080/health`），能够看到如下输出：

![](img/9f2c3bb39016778a62a11d6409b19858.png)

表示我们已经能够正常访问到 `Linux` 服务器上运行的项目了。

还可以在 `Deployment` 界面中配置 `web server url`，相当于一个快捷访问方式：

![](img/37b051543ccf0fbb3de0bfec1f2a1eb6.png)

#### 3）构建项目

和上面运行项目的方式相同，在自己的电脑打开终端访问远程虚拟机，然后执行 `Maven` 打包命令：

```
mvn package
```

看到如下输出，表示打包成功：

![](img/0c3a758f307ac88b5103882fc9f0e1e2.png)

![](img/de41def141887e3a25804bcf2e6d7f9a.png)

然后在服务器上就能看到生成的可执行 `jar` 包：

![](img/a0c2ac29e37523b101e6e7953a2c5ce1.png)

#### 4）部署项目

现在项目可执行 `jar` 包已经放到了服务器上，我们只需启动即可。

输入下列命令，以生产环境运行 `jar` 包：



```
java -jar /home/yupi/code/target/yuoj-code-sandbox-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod
```

看到如下信息，启动成功：

![](img/dc042af4f036aa28eef05e09aa332226.png)

## 5）远程调试

现在程序已经在 Linux 虚拟机上启动了，那么我们如何调试远程的程序呢？比如打断点、查看变量信息等。

首先强调一点，远程调试不要乱用，可以在开发时调试，但千万别给线上打断点！影响正常用户的访问。

开启远程调试的方法很简单，首先在 IDEA 右上角找到编辑配置：

![](img/3a5e7d7dfc8a52098c1599c8c5bf1232.png)

然后新建一个远程 JVM Debug 配置：

![](img/2b1fa74371823109aba7560f78af3110.png)

更改配置，修改自己虚拟机的 IP、希望占用的远程调试端口、JDK 版本等，IDEA 会自动生成一段远程调试参数：

![](img/0b07e2f484bb3d85fdb0eee32ce79fdc.png)

在启动项目时，给启动命令追加上述界面生成的 command line 参数，注意要加到 `jar` 包路径之前。

示例命令如下：

```
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar /home/yupi/code/target/yuoj-code-sandbox-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod
```

启动项目后，在右上角开启 Debug：

![](img/af79cdb6c497c3616c93433a6674570c.png)

然后给代码打个断点：

![](img/0b8f5fa8afc3567de5efede33d5e341e.png)

再访问对应的端口，可以看到 Debug 已经生效：

![](img/fc4abc0991c61631ae586dc88949ee0a.png)

现在，你就可以把 Linux 虚拟机当做自己的电脑进行远程开发啦~

## 3、纯远程开发

除了上面的方式外，还有一种更简单的远程开发方法。

使用 IDEA 自带的远程开发功能，可以直接将本地的编译、构建、调试、运行等工作全部都放在远程服务器上执行！而本地仅运行客户端软件连接服务器，像之前一样编写代码、进行其他开发操作即可。

这种方式就很像云游戏，自己的电脑不存放代码、不负责程序的运行，只需要运行一个操作界面即可，而 “脏活累活”都交给服务器。适合本地电脑性能差（服务器性能强）、开发环境多人协作开发、需要统一开发环境的场景。

如下图：

![](img/d1e961e7c8cc818ea80bc72e75826a63.png)

这种方式也更简单，进入 IDEA 主页，找到 SSH 选项，点击新建项目：

![](img/a5e6889f959ca93372ab9cc20c5d0207.png)

配置 SSH 连接：

![](img/36ef0b45d622839783c07a87595bebf8.png)

指定远程开发的代码路径：

![](img/e8666de64ab35c9fd64c6021234fccd5.png)

首次使用时，需要等待下载 JetBrains Client 客户端：

![](img/832a9dc94bad9e3f1fe3f27b51fba9a3.png)

服务器上也会自动安装对应的远程开发后端：

![](img/0c8323bf3902b8ba0dbcea8eccd0e228.png)

然后进入远程开发：

![](img/82914a5f5dcab2648bf94fe0d3dd0bdf.png)

打开任务管理器，可以看到 JetBrains 的 client 非常 “轻量”：

![](img/2fa7df2d7172daecf3c56955beb5d083.png)

进入客户端界面，软件会自动加载 Maven 项目，但是需要手动指定服务器上已安装的 JDK：

![](img/d870fd0016f69f01ac216963336307f1.png)

然后直接以 Debug 模式启动项目：

![](img/000597fa8c3797d83e394e950ab08159.png)

还可以配置请求转发，像访问本地服务一样访问远程端口：

![](img/e07c840f7fb918e11dcc633823665b8d.png)

![](img/777a70ecddd3325356b5d1e48767ce32.png)

接下来访问本机地址 `http://127.0.0.1:8080/health`，就能够访问到项目啦，而且直接支持 Debug！无须做任何多余的配置！

![](img/db0f4c73ca71dbea443c75face290ef1.png)

---

至此，使用 Linux 虚拟机 + 远程开发的教程就结束了，个人更喜欢第二种远程开发方式，非常简单、而且丝滑，接下来大家就可以愉快地学习 Linux、Docker 等技术啦~

原创不易，大家如果学会了的话，希望能点赞收藏支持下 🌹

往期推荐

[新项目又来咯！](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247548613&idx=1&sn=b33eac9c819f03fd7f94784d985d4cd5&chksm=e9c2d932deb55024b361627a09b03593483f80d39e357fa0b2a9b20a9a4aea21dee95966b674&scene=21#wechat_redirect)

看完这个，我直接把 SQL 刷通了！

[一次很意外的网站故障经历。](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247548725&idx=1&sn=b5911b11254e32253d4dc63152d73bd6&chksm=e9c2d8c2deb551d4927cfd30e55c4361a51c80e47ad5faa14478359b04996254da0edeaefaf9&scene=21#wechat_redirect)

[实习天天crud，很迷茫...](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247548680&idx=1&sn=5ef3f10f3d9a4beb4a4e0c2b8f18d0fa&chksm=e9c2d8ffdeb551e9ee13b6eec5b6aa95ad8d04f4deb14ecff0ac2bc53209a83e2da6f56b5f78&scene=21#wechat_redirect)

最近面试了一些人，有些槽点跟大家说说。

[有自信了，再战阿里！](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247548764&idx=1&sn=b34e5023b209cd420708a8d54514b63b&chksm=e9c2d8abdeb551bdd80c347e62d1f43d3cbfeed31b639a4d46c24cf3de7452f804b8b9386c8a&scene=21#wechat_redirect)
