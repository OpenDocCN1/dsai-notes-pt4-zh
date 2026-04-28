# Clawdbot 上手实录：部署+反代+WebAuth 一步到位

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LcsodjyK6ozdO6xTDiCck01InFh](https://qqs7y1hozd1.feishu.cn/docx/LcsodjyK6ozdO6xTDiCck01InFh)

这两天，**Clawdbot****()** 在技术圈突然爆火，不少**人已经开始在服务器上尝鲜部署**。但真正跑起来之后才发现，Web 控制台、HTTPS、安全访问这些问题一个都绕不开。

这篇文章就简单记录一下 Clawdbot 的部署过程，以及如何通过宝塔面板做反向代理并加一层 Web Auth，让它用起来方便，也更安全。

1.  登录面板，打开SSH终端，或直接打开SSH终端

+   常用系统Debian/Ubuntu/CentOS，可直接指向以下命令安装

```
curl -fsSL https://clawd.bot/install.sh | bash
```

+   部分国产操作系统如OpenCloudOS/Alibaba Cloud Linux，请先到面板-网站-Node项目-Node版本管理器-右上角更新版本列表-安装最新稳定版v24.13.0

+   并设置命令行版本为刚刚安装的稳定版

![](img/79f1336bc2b87e71edf8be37c6050e68.png)

![](img/d9bebc3bd7e65fe6f64c6f49e023f1a8.png)

+   然后终端执行以下命令安装

```
npm install -g clawdbot@latest
```

1.  安装完成后执行以下命令进行初始化，如若脚本安装会自动进入初始化页面，可忽略下面的命令。

```
clawdbot onboard --install-daemon
```

![](img/5d73398118e11623a3aa4d4fc7dd77e1.png)

1.  根据流程提示完成初始化配置：

+   I understand this is powerful and inherently risky. Continue?（我知道这个功能很强，但也存在风险，要继续吗？）按左方向键，选择yes

+   Onboarding mode（引导模式）默认快速开始即可

![](img/254aa8e78942bde069bfd4ff2a7d221f.png)

+   Model/auth provider（模型 / 鉴权提供方），根据自己需要选择并设置，请注意，此处列表里面的Moonshot、Qwen，默认为国际站端点，国内账户可能无法使用。

![](img/a405a6817e30f6ec59c675d133a4bcd5.png)

+   Select channel (QuickStart)（选择通道），根据自己需要选择渠道并按提示完成配置，我这里使用网页对话，直接跳过，大家可以根据自己需要选择

+   Configure skills now? (recommended)（配置技能），配置技能并选择安装技能的方式，默认选择即可。

![](img/a9ec41003403d7e0e7b1b5495fe92c66.png)

+   根据需要选择自己的技能，空格键选中，最后按回车键确认，等待安装即可，如果不想现在安装可以选择第一项跳过。

![](img/0c72f3f950516305f101bc76464393fc.png)

+   剩下的几个API配置我先默认跳过，实际使用时根据自己需要选择

![](img/b314bfb15415e03ecf6531192efb0da2.png)

+   安装完成后会展示Token，注意保存，一会会用到

![](img/91ffd72cd4ee99c43eb6c767358f9dff.png)

1.  安装服务：面板或者腾讯云/阿里云控制台免密登录方式无法完成服务安装，需要使用SSH客户端连接到服务器，并执行以下命令安装服务。

+

```
clawdbot daemon install
```

![](img/39b10f7bcfadb061373fbb9eb61e386a.png)

1.  安装完成执行以下命令启动

+

```
clawdbot daemon start
```

![](img/1caacd55573624bd5ff4be2d2d83be0b.png)

![](img/3414ce00650cc03fc87a51dccde554bc.png)

**配置反向代理**

1.  服务默认监听本地端口，不支持公网访问，因此需要配置反向代理，打开面板-网站-PHP，添加项网站，并设置域名（我这里用IP做演示）

![](img/a48a553ddf4a57cf7bca40b3bb67f69a.png)

1.  打开站点设置-SSL，为站点配置SSL证书

![](img/6f3a1495051fb23610c42b5656378d2b.png)

1.  点击反向代理-添加配置反向代理，并填写以下配置

![](img/f9d79b723b42180f58c1872e7d21dfec.png)

1.  配置完成后找到之前保存的Token

![](img/63512995b9caf5ae8cee6b8a3f965108.png)

替换Token域名为实际域名，例如我这里将

+

```
http://localhost:18789/?token=d275f89c6905729e850634de060e64a30822b5264fe99703
```

替换成

+

```
https://43.133.61.18/?token=d275f89c6905729e850634de060e64a30822b5264fe99703
```

正常应该看到如下提示

![](img/f188ea773f3c069d898603214de94db7.png)

1.  回到终端，执行以下命令，查看待授权的访问

+

```
clawdbot devices list
```

![](img/b955dd4a5222dee9f2ebca0d312f7113.png)

1.  执行以下命令，批准访问

+

```
clawdbot devices approve request-id
```

如：

+

```
clawdbot devices approve 54c0def3-e60a-4074-ad43-19887d222d7f
```

执行成功将看到以下提示：

![](img/16d0f8cda990db8092b304d6718fd71c.png)

页面状态也会变成OK

![](img/5bcd038f1c51dcaf2eb5ab88e14366f5.png)

1.  此时我们就可以试着和他进行对话

![](img/8e33ea78c3bca9a27a32db387cdbcd0b.png)

![](img/597253486ac5293f381a1d1c79b062e1.png)

**配置BasicAuth**

如果想要再加固一层，可以配置BasicAuth，但面板的访问限制无法对反向代理直接生效，因此我们需要手动修改配置：

1.  打开终端，执行以下命令，生成htpasswd文件

+

```
printf &quot;用户名:$(openssl passwd -apr1 密码)\n&quot; &gt; /www/server/nginx/conf/clawd.pass
```

请替换用户名和密码，例如：

+

```
printf &quot;clawd:$(openssl passwd -apr1 clawd123)\n&quot; &gt; /www/server/nginx/conf/clawd.pass
```

![](img/0495a7514ee3761d0daee1e50c92a2e5.png)

生成后执行以下命令修改权限：

+

+

```
chown root:www /www/server/nginx/conf/clawd.pass                                               chmod 640 /www/server/nginx/conf/clawd.pass
```

1.  回到网站-设置-反向代理-配置文件

![](img/4b5fb54372ee71902b261cfb8b031d4e.png)

1.  打开并添加以下内容：

+

+

```
auth_basic &quot;Authorization&quot;;auth_basic_user_file /www/server/nginx/conf/clawd.pass;
```

![](img/2063cd002b1ede78763792bad2d96c58.png)

1.  点击保存，服务会自动重载。

1.  再次访问，应该可以看到需要输入用户名和密码，输入刚才设置的用户名和密码点击登录即可。

![](img/a0b5ce04b9f202015989a03cdbff8b21.png)

到**这里，Clawdb**ot 的部署和访问就算完整跑通了。整体来看，Clawdbot 本身并不复杂，真正容易踩坑的反而是访问方式和安全边界：本地监听、Token 授权、HTTPS、以及如何避免直接暴露在公网。

通过宝塔面板做一层反向代理，再加上 Basic Auth，把这些问题一次性解决，既不影响使用体验，也能让人用得更放心。

当然最终来说，如果你在终端已经完成了所有配置，包括相关的聊天通道，不需要在网页管理和使用，那你完全可以忽略反向代理的步骤，那样也会变得更安全一些。