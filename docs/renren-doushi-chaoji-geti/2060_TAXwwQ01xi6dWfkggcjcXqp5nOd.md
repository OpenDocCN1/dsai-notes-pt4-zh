# 2023-08-11搞台虚拟机玩玩！

> 来源：[https://xuqiwei1986.feishu.cn/docx/OZtqdbRhxo1ssExa1acccmconNe](https://xuqiwei1986.feishu.cn/docx/OZtqdbRhxo1ssExa1acccmconNe)

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/zOvx89QBhIPj5Cicbo90sA)q.com/s/zOvx89QB...

>

原创 鱼皮 程序员鱼皮 2023-08-11 18:19 发表于 上海

> Linux 虚拟机 + 远程开发保姆级教程，轻松搭建高效开发环境！

**大家好，我是鱼皮。最近我**正在带大家开发 OJ 系统，需要用 Docker 容器技术 来实现隔离的代码沙箱。

随之而来的是一个难题：很多同学的电脑都是 Windows 的，直接在 Windows 上安装 Docker 容器比较麻烦，需要依赖 WSL（Windows Subsystem for Linux），很容易出现各种报错。

比如 WSL 内核版本太低：

![](img/31cf922f6bbcda89ffc80ff92ebad0d2.png)

缺少 WSL：

![](img/cfc431833d3293212cd6f35fa7e31e13.png)

所以，我们不妨直接搞一台 Linux 系统的服务器来安装 Docker 吧，不仅不会影响现有的系统，以后开发和部署项目也都会更方便。

那么如何拥有一台 Linux 服务器呢？

比较常见的有 2 种方式：

+   购买第三方云服务商的云服务器

+   使用虚拟机软件，在自己电脑上额外运行 Linux 系统

第一种方式要花钱，就不多说了。本篇文章我会带大家实践第 2 种方式，只需几分钟的时间，手把手带大家在 Windows 上安装 Linux 虚拟机，并且实践 2 种远程开发方法，让大家像使用 Windows 一样轻松地在 Linux 上开发程序。

⭐️ 本文对应视频教程：https://www.bilibili.com/video/BV1h94y1k7Jf/（讲解会更细节，建议配合本文使用）

## ** ****一、虚拟机安装 **

### **  ****1、下载安装 **

首**先我们**要下载虚拟机软件，这里比较推荐 VMware Workstation Player，个人不商业使用是 免费 的。

直接到 VMware 官网下载：

指路：https://www.vmware.com/cn/products/workstation-player.html

![](img/654b9ec367e2f70d450c8a9543083ca1.png)

这里有条件的朋友也可以使用 VMware Workstation Pro，功能更全，但是要付费。

在官网点击免费下载后，即可进入安装流程。

这里要注意勾选 “自动安装 Windows Hypervisor Platform” 和 “将 VMware Workstation 控制台工具添加到系统 PATH” 两个选项：

![](img/6468c40f40c6b9642162240f08202499.png)

![](img/667f25ea9fd396f0945cd604594f3224.png)

选择免费、非商用版本：

![](img/10884d09ad20c666cf4d341274f91ebd.png)

然后就安装成功了，比起在 Windows 上折腾 WSL，安装虚拟机可方便太多了~

### **  ****2、安装系统 **

虚拟机软件安装完成后，我们打开软件，接下来要先新建虚拟机来安装一个 Linux 操作系统：

![](img/434c1927fc8801718de11bd795975598.png)

点击创建新虚拟机，然后它让我们提供一个 Linux 操作系统的镜像：

![](img/75d19b22a91135d240474026bd236cf0.png)

问题是，Linux 操作系统有很多的发行版，我们安装哪个版本呢？

我学生时代的时候，特别喜欢用 CentOS，但可惜的是，CentOS 已经停止维护了。。。

所以这里我们选择 Linux Ubuntu 18.04 LTS 版本来给大家演示，有图形界面，会更能照顾到新手同学。

Ubuntu 镜像下载：https://releases.ubuntu.com/18.04/

拉到底部，找到 .iso 后缀的文件下载即可：

![](img/ab24a1e1539d61a95ae38930cd1222b0.png)

当然，萝卜青菜，各有所爱，会玩 Linux 的同学可以自由选择版本~

指定完操作系统后，我们继续新建虚拟机，可以根据实际的电脑情况来自定义虚拟机的硬件。如果你的电脑配置还不错，内存和 CPU 可以分配多一点：

![](img/dc249541d25fa9be1ec41bef05825959.png)

点击完成，然后稍等片刻，系统就安装完成并启动啦！

![](img/df064cf8afe53ef9c41fb7214a3292e9.png)

### **  ****3、修改配置 **

像购买一台新电脑一样，我们有了新的操作系统后，可以自行修改一些配置，比如修改语言为中文。

#### ** 修改语言为中文 **

按 windows 键，弹出功能搜索按钮，输入 "language"，打开 "语言支持"：

我这台虚拟机是已经改完语言的，所以截图中已经是中文了

![](img/fdb209e655a26eab6c0e7c76909cf8cf.png)

然后点击添加语言，选择中文简体，并且把汉语拖到语言列表的最上面，最后点击应用到整个系统：

![](img/a75562cc85f6a449ae45e7b7fb82a75d.png)

稍等片刻，中文语言包、输入法等内容就安装好了，点击右上角重启虚拟机：

![](img/d5852742511b1dbb61336a00a4516e0d.png)

重启后，同样是按 windows 键，弹出功能搜索按钮，输入 "language"，打开 "区域和语言"：

![](img/7d500a47dfebf5025dc24cec5fbf412c.png)

在区域和语言设置中，新增输入源，添加已安装的中文输入法：

![](img/b7a36b245ca374e98689a2bad4612e98.png)

#### ** 修改分辨率 **

用同样的方法，我们可以调整系统的分辨率。

先打开显示设置：

![](img/9bb930b9e97d78eccd42a265f3fe288e.png)

然后可以调整分辨率和字体大小：

![](img/a468078722320a90ddc14200fe155f46.png)

#### ** 修改时间 **

默认情况下系统的时区可能是有问题的，我们可以打开“日期和时间”设置，选择时区为“中国上海”：

![](img/1904f87b50929224066137e903f145b8.png)

### **  ****4、安装软件 **

基本配置修改完成后，我们就可以给 Linux 系统安装软件并使用了。

和其他操作系统一样，Linux Ubuntu 也有自己的应用商店，能够可视化一键安装软件，比如防火墙之类的，适合新手操作：

![](img/87ef9075ef9c2e054330dfb921d68c67.png)

当然，大家用 Linux 更多地还是要学习它的终端命令行操作，所以这里我们以 Docker 为例，使用命令行来快速安装。

按 Ctrl + Alt + T 快速打开终端，输入以下命令来一键安装 Docker：

apt 命令是软件包管理工具，能够让我们方便地安装软件

```
sudo apt install docker.io
```

如下图：

![](img/d723f8e762aab51f2fc4299f6e33216d.png)

安装完成后，输入 docker -v ，查看版本号，如下图：

![](img/17ed3e98269888028529a23971c195c3.png)

然后输入 docker run 命令，测试 Docker 能否正常运行：

```
sudo docker run hello-world
```

能看到如下信息，表示 Docker 安装成功：

![](img/78ed7eec33aeac7ff02c3e6dd227df70.png)

怎么样，比在 Windows 安装方便多了吧？

## ** ****二、Java 远程开发 **

有了 Linux 系统后，我们怎么在 Linux 上开发呢？

是直接在 Linux 上安装常用的开发工具，然后在虚拟机里编写代码么？

No、No、No，这样不仅要改变你原本的编码习惯，还可能会出现卡顿。如果你的项目有的在 Windows、有的在 Linux，需要同时开发的话，来回切换系统可太麻烦了。

所以这里我会教大家使用远程开发，直接在自己的 Windows 电脑上操作 Linux 服务器，和之间的开发方式完全一致！

两个字：“丝滑”！

**下面我会以** J**ava 开发**为例，带大家掌握 2 种远程开发的方式： 远程部署 和 纯远程开发 ，大家可以根据实际情况选择合适的方式。

### **  ****1、前置准备 **

在学习远程开发前，我们要先做一些准备工作。

#### ** 1）网络连通 **

要确保我们本地的 Windows 电脑能够连接到 Linux 虚拟机。

首先在设置中搜索网络，点击查看网络设置：

![](img/ec792f0219d1b839abfb5f7d5c2a2188.png)

获取到虚拟机的 IPv4 地址，这个地址一定要记好了：

![](img/c624cec0d2d82e22d815e5a3dc0f0de0.png)

在自己的 Windows 电脑上 ping 这个 IP 地址，测试能否 ping 通。

如下图，表示网络正常连通：

![](img/eb0053e2b5bdfd1af3008b6c341b6532.png)

#### ** 2）开启 SSH **

光能 ping 通虚拟机还不够，很多远程开发工具都是通过 SSH 协议连接远程服务器的，所以我们要在 Linux 虚拟机上开启 SSH。

在 Linux 虚拟机上打开终端，输入下列命令安装 SSH 服务器：

```
sudo apt-get install openssh-server
```

安装后，输入下列命令检查 SSH 是否已开启：

```
ps -e |grep ssh
```

如下图，则表示 SSH 开启成功：

![](img/5b4ed8048d9949ec318d4094d48ba7c9.png)

如果未启动，执行 sudo service ssh start 手动启动服务试试

#### ** 3）安装 Java **

因为我们要演示 Java 远程开发，所以 Java JDK 的安装是必不可少的。

Linux 下安装 JDK 还是很方便的，首先打开终端，输入命令来更新软件源信息：

```
sudo apt update
```

然后执行下列命令来安装 Java 8（经典版本）：

```
sudo apt install openjdk-8-jdk
```

安装完成后，输入命令检查 Java 版本：

```
java -version
```

看到如下输出，表示 Java 安装成功：

![](img/0cc7b0447bfdb0c4339942e3225207e1.png)

#### ** 4）安装 Maven **

除了安装 Java 外，我们还要安装 Java 的项目依赖管理工具 Maven，便于接下来我们测试 Spring Boot 项目的远程开发。

打开终端，输入如下命令安装 Maven：

```
sudo apt install maven
```

安装完成后，输入命令来查看 Maven 的版本号：

```
mvn -v
```

看到如下输出，表示 Maven 安装成功：

![](img/98bbf346b0734c0843e0fd46bc9af05c.png)

环境准备完毕，接下来开始远程开发之旅~

### **  ****2、远程部署 **

第一种远程开发方式是 **远程部署 **，就像我们真实做项目时，先在本地用代码编辑器完成开发，再把代码放到 Linux 服务器上去部署一样。远程部署的思路是在本地编写代码，然后把本地的代码文件定期同步到 Linux 服务器，再用本地电脑操作远程服务器完成部署和运行。

如下图：

![](img/f151c09e0dcd09a827d6e864b6e25907.png)

这种方式更多地是为了提高将代码放到服务器部署的效率，不算是真正意义上的远程开发，但可以达到接近的效果。

远程开发主要分为 6 个阶段：编码、文件同步、运行、编译构建、部署、调试。接下来让我们以一个最简单干净的 Spring Boot 项目为例，实战上述几个操作。

#### ** 0）编码 **

假设我们本地已经有了一个示例项目，仅提供一个 checkHealth 接口，用于测试服务能否正常访问，端口为 8080。

核心代码如下：

```
@RestController(&quot;/&quot;)
public class MainController {

    @GetMapping(&quot;/health&quot;)
    public String checkHealth() {
        return &quot;ok&quot;;
    }
}
```

#### ** 1）文件同步 **

打开 JetBrains IDEA 开发工具，点击 Tools => Development => Configuration：

![](img/0be73b8c06c12271f37179a74cd0e1da.png)

进入到如下操作界面，需要先来配置 SSH，和服务器建立连接：

![](img/2dd15355804002733c76cad043c64543.png)

进入 SSH 配置，输入自己实际的服务器 IP！不一定和下图的 IP 地址相同！

![](img/356a34f7b19a7ff2b2e492c010965a6d.png)

点击测试连接，能看到连接成功的弹窗；如果报错，可能是 IP 地址、用户名或密码输错了。

![](img/0f1d508635c6e15214ea5cf54ffdbc8e.png)

回到部署界面，点击 Mappings 来配置路径映射，就是把本地电脑的文件同步到远程服务器的指定路径：

![](img/27675893933bdbbcf1b34dee5198809a.png)

保存，在 IDEA 右侧就能看到我们虚拟机的所有文件列表啦：

![](img/838d183eefac70576d3c958c760c7e5d.png)

然后点击 Tools => Development => Automatic Upload，开启自动同步：

![](img/07e26416279748e96fa1a2361c27ef40.png)

现在，尝试在自己电脑的代码文件夹中新建一个文件，该文件就会自动同步到 Linux 虚拟机~

但目前如果你删除了自己电脑的文件，Linux 虚拟机的对应文件不会删除，所以我们还要进入 Options 配置：

![](img/fca1f4def48ac986b5c04f8106180263.png)

勾选上文件删除的同步：

![](img/e45c0cce7c3e62c683c6cffa4ff9d53a.png)

至此，你在本地编写的代码都会自动同步到服务器上了。

#### ** 2）运行项目 **

可以直接在 IDEA 的终端中快速创建一个能够远程访问远程服务器的终端：

![](img/9903213bab35df13adb0783520f255aa.png)

然后使用 cd 命令进入到代码目录：

![](img/d070c1307f7afacfb4397dde501e98be.png)

输入 Maven 命令来运行项目：

```
mvn clean
mvn spring-boot:run
```

能够看到项目成功启动：

![](img/18327b78a96d2fffc811e394a04816d3.png)

然后访问远程服务器的接口地址（比如 http://192.168.170.132:8080/health），能够看到如下输出：

![](img/feeb7a2e097aaeb07e612111020330e4.png)

表示我们已经能够正常访问到 Linux 服务器上运行的项目了。

还可以在 Deployment 界面中配置 web server url，相当于一个快捷访问方式：

![](img/dacbbf8d7187058bb24602c7701b1564.png)

#### ** 3）构建项目 **

和上面运行项目的方式相同，在自己的电脑打开终端访问远程虚拟机，然后执行 Maven 打包命令：

```
mvn package
```

看到如下输出，表示打包成功：

![](img/e5d781027627653414199e0f15e610cb.png)

![](img/489f74301a2f4ecfc8fbc20580badab5.png)

然后在服务器上就能看到生成的可执行 jar 包：

![](img/3b8ba11c5bc43d28f29c732fac3d3d5a.png)

#### ** 4）部署项目 **

现在项目可执行 jar 包已经放到了服务器上，我们只需启动即可。

输入下列命令，以生产环境运行 jar 包：

```
java -jar /home/yupi/code/target/yuoj-code-sandbox-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod
```

看到如下信息，启动成功：

![](img/4ed6450106224d81a30a93f8b21ea8d5.png)

#### ** 5）远程调试 **

现在程序已经在 Linux 虚拟机上启动了，那么我们如何调试远程的程序呢？比如打断点、查看变量信息等。

首先强调一点，远程调试不要乱用，可以在开发时调试，但千万别给线上打断点！影响正常用户的访问。

开启远程调试的方法很简单，首先在 IDEA 右上角找到编辑配置：

![](img/0f68111d2a8b12083b9e442f43423fb7.png)

然后新建一个远程 JVM Debug 配置：

![](img/a4162565cd1f64f9d3f0721ade65fa16.png)

更改配置，修改自己虚拟机的 IP、希望占用的远程调试端口、JDK 版本等，IDEA 会自动生成一段远程调试参数：

![](img/c0ae4a5631bb5a69bc457b9add110527.png)

在启动项目时，给启动命令追加上述界面生成的 command line 参数， 注意要加到 jar 包路径之前 。

示例命令如下：

```
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar /home/yupi/code/target/yuoj-code-sandbox-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod
```

启动项目后，在右上角开启 Debug：

![](img/c799ea350e1ecec8c68503a6c1a56f5b.png)

然后给代码打个断点：

![](img/faeb3458349898a74bf429ab84059956.png)

再访问对应的端口，可以看到 Debug 已经生效：

![](img/9690d84a540312393f2c489afa371aab.png)

现在，你就可以把 Linux 虚拟机当做自己的电脑进行远程开发啦~

### **  ****3、纯远程开发 **

除了上面的方式外，还有一种更简单的远程开发方法。

使用 IDEA 自带的远程开发功能，可以直接将本地的编译、构建、调试、运行等工作全部都放在远程服务器上执行！而本地仅运行客户端软件连接服务器，像之前一样编写代码、进行其他开发操作即可。

这种方式就很像云游戏，自己的电脑不存放代码、不负责程序的运行，只需要运行一个操作界面即可，而 “脏活累活”都交给服务器。适合本地电脑性能差（服务器性能强）、开发环境多人协作开发、需要统一开发环境的场景。

如下图：

![](img/4059c5f043f918d20c795998bd853d99.png)

这种方式也更简单，进入 IDEA 主页，找到 SSH 选项，点击新建项目：

![](img/89e40e21eed22104643f059ff0bad164.png)

配置 SSH 连接：

![](img/b3e9c94ba78b0f17bb099c701556c850.png)

指定远程开发的代码路径：

![](img/e8f9521e8cad0f17dbbb0853f9b0e791.png)

首次使用时，需要等待下载 JetBrains Client 客户端：

![](img/a8fee8ee7303f873824c70b75912c3f8.png)

服务器上也会自动安装对应的远程开发后端：

![](img/657db50953cd28ce5622466a5140f01e.png)

然后进入远程开发：

![](img/181bd3cf566d91e8612630e4ee1ee1c5.png)

打开任务管理器，可以看到 JetBrains 的 client 非常 “轻量”：

![](img/ce9936c19bff6a767769f8ba87b0ffbf.png)

进入客户端界面，软件会自动加载 Maven 项目，但是需要手动指定服务器上已安装的 JDK：

![](img/71a3a108abf8a5fdafdb10f9c9dccd93.png)

然后直接以 Debug 模式启动项目：

![](img/1487fd6604096a700dd9ae38d20fc1f7.png)

还可以配置请求转发，像访问本地服务一样访问远程端口：

![](img/4e8ecc51f64fc794b2d11453e956df8b.png)

![](img/52e7d2c3ae68be57d31948f071db2897.png)

接下来访问本机地址 http://127.0.0.1:8080/health，就能够访问到项目啦，而且直接支持 Debug！无须做任何多余的配置！

![](img/fb47e1d1379101ea9d9ca181c8ce6209.png)

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