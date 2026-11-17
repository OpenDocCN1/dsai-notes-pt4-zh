
# VPS 部署 ClewdR+酒馆

> 来源：[https://sqivg8d05rm.feishu.cn/docx/KxT6dPOSPo9NEkxlxuUcsqOFnsg](https://sqivg8d05rm.feishu.cn/docx/KxT6dPOSPo9NEkxlxuUcsqOFnsg)

作者: 水秋喵

旧版1Panel教程：[VPS 部署 Clewd+酒馆（1Panel版）](https://sqivg8d05rm.feishu.cn/wiki/A7U3w7aPZidNlLkPcJecuOgon8s)

旧版宝塔Clewd教程教程：[VPS 部署 Clewd+酒馆 副本](https://sqivg8d05rm.feishu.cn/wiki/GnoQw7nT6io7sSkKvDYctV0Yndb)

---

使用宝塔搭建 [SillyTavern](https://github.com/SillyTavern/SillyTavern) 与 [Clewd修改版](https://github.com/teralomaniac/clewd) by [teralomaniac](https://github.com/teralomaniac)

优点：

+   PC / MAC / Android / IOS / HarmonyOS NEXT全平台皆可访问

+   数据云端储存，全平台同步

+   在外网架设后，日常使用无需魔法

缺点：

+   服务器要钱

+   使用的服务器较差可能会产生性能问题

---

## 使用宝塔部署 Clewd+酒馆

### （更新教程ing，有什么没写清楚或者需要补充的地方，可以群里@或者私聊我喵喵喵喵）

---

## BUG修复

hostvds 被迫更换ip后如果无法连接，尝试更换安全组

![](img/c8df5ccdcde87a812b80b45d939ad399.png)

---

## 萌新建议

> 萌新咪可创建一个文件并复制以下内容，水秋喵会在下文提示萌新咪保存需要的信息。

```
* 教程：https://sqivg8d05rm.feishu.cn/wiki/RVQ0wka6FiGGWXkc40jc2GHOnyc
* 请保存并保护好下列信息，不要外泄
-----服务器信息-----
服务器厂商（如HostVDS）账号密码：
服务器IP：
服务器Root密码：
------面板信息------
外网面板地址: 
内网面板地址: 
username: 
password: 
------项目信息------
酒馆地址：
ClewdR地址: 
API Password: 
Web Admin Password: 
```

## 设置VPS

### 选购VPS

（选购VPS的原作者为慢性鼻炎，小修改）

如果你不知道买什么，可以看看 [hostvds](https://hostvds.com/?affiliate_uuid=1b6cab9c-05f5-4f6d-a1c9-b3dad79c5278) 或 [hostyun](https://www.hostyun.com/)

无广，我[随便选的，主要](https://hostvds.com/?affiliate_uuid=1b6cab9c-05f5-4f6d-a1c9-b3dad79c5278)是便宜。当然，你自己挑着买也行，有便宜好用的记得踢我一下。

下面我用 `hostvds` 做例子(因为这个最便宜，0.99刀一个月，算下来一个月几块钱吧

> 注册后保存咪的账号与密码

1.  开始充钱，选择支付宝，萌新充1刀即可（充5刀以上免手续费）。

因为这个是实时计费的，所以你可以看到我剩$0.9104。

![](img/4d061149b2b1bdf4c1fc7a7ddeadd53a.png)

1.  氪金结束之后，我们就可以回来买服务器了。

![](img/8c977bccec305636d459f8e0d52d9b02.png)

1.  VPS的名字随便，地点两个美国的随便选，我选默认的第一个。系统选择`Ubuntu 22.04`或更高。

![](img/fa664a408d958528e239d94f0196a8fa.png)

1.  按需选择配置后点击部署。我选择第一个，1核1G，10g空间，50兆速度，0.5T流量。

![](img/abb2cfa5aa5e08ee83f72b7663d9bc5a.png)

1.  部署后稍等一会启动成功，显示你的服务器IP。

注意！保存好你的服务器IP，并尽量不泄露出去。

![](img/e5f32fe58b74d43faa44021bc1987290.png)

> 保存咪的IP地址

1.  修改VPS密码，点击VPS - 操作 - 安全。

![](img/068d4602473694bd209e0398c99f7993.png)

1.  点击更改`root`密码，服务器要创建两分钟后才能修改密码，稍等片刻。

![](img/8accf989c576986b591010949c8f0d78.png)

1.  设置服务器`root`密码。

![](img/683454aa45476fe7c4125b53ee1cfe8a.png)

> 保存咪的服务器`root`密码

### 连接VPS

1.  查看你的VPS服务器ip地址

![](img/744008c90732cfed874a0cad170f0307.png)

1.  [使用`SSH`工具进行](https://github.com/RipplePiam/MobaXterm-Chinese-Simplified)连接，不知用什么的可使用 `MobaXterm`（点击下载）

![](img/4dd8ddaff6c694991185ef312f29518f.png)

![](img/4e783f4d4ee824da512afc826eba9a89.png)

输入`root`，回车，输入密码，回车（注：密码不显示，打错重来即可）

![](img/0a61e0d9187428a29b5999da3922c898.png)

这样就成功登录了

![](img/8492fc80b5561372eec875aef4c4c6a1.png)

## 安装环境

### 安装宝塔面板

1.  [输入以下指令并回车](https://www.bt.cn/new/download.html)（右键粘贴）（宝塔官网安装方法）：

```
if [ -f /usr/bin/curl ];then curl -sSO https://download.bt.cn/install/install_panel.sh;else wget -O install_panel.sh https://download.bt.cn/install/install_panel.sh;fi;bash install_panel.sh ed8484bec
```

1.  提示 Do you want to install Bt-Panel to the `/www` directory now?(y/n)，输入 `y` 回车。

![](img/94166576370bf1cf404f36ccfdfc4ca2.png)

1.  稍等片刻安装成功，**自行保存登录消息**（重要！需要自行保存并保护好）。

![](img/579bd40dbd0049d846f8e332e6f78e99.png)

> 保存咪的面板账户登录信息:面板地址 `username` `password`

面板地址加入收藏夹，后续维护只要进面板就行。

1.  进入面板地址，输入账号和密码（上图的`username`和`password`），接受并同意用户协议。

![](img/9bbdb6b432474bd1a347b8e99c01e94d.png)

关闭或点击不再显示推荐。

![](img/ab91aff52fc77bc4c078a28814c2b62e.png)

### 安装Node版本管理器

1.  点击网站 - Node项目 - 立即安装

![](img/dafc93cb0ec76b96cb02834e5e5faf93.png)

1.  安装后点击**Node版本管理器。**

![](img/0e7eee6393146edbccc0d10d31239266.png)

先点击右上角的更新版本列表，然后选择最新稳定版（发布日期2024及以后），或自行考虑。

安装完选择命令行版本为你安装的版本。

![](img/dd638db47a1101c4fd2612ce73437f39.png)

### 下载项目

![](img/70800087aa6eb9eb64ebfa221997d34e.png)

1.  输入以下指令下载酒馆

```
git clone https://github.com/SillyTavern/SillyTavern && npm install --prefix SillyTavern
```

1.  输入以下指令下载并运行ClewdR

```
curl -LO "https://github.com/Xerxes-2/clewdr/releases/latest/download/clewdr-musllinux-x86_64.zip" && unzip clewdr-musllinux-x86_64.zip -d . && chmod +x clewdr && ./clewdr
```

![](img/79ad4feb605700ccb13cdf0c67ab2305.png)

> 保存咪的ClewdR信息:`API Password` `Web Admin Password`

## 配置酒馆

### 配置酒馆

1.  点击文件，找到 `/root/SillyTavern` 中的 `config.yaml` 并打开。

![](img/8e5c59dfd2125859325e076f16da0c4f.png)

**如果找不到**，就把 `/root/SillyTavern/default` 中的 `config.yaml` 复制到 `/root/SillyTavern` 中

（注意是复制过去，不要打开这里的修改，已经有的就别复制了！）

![](img/b08dc4858f6a6fb6722cb83b4df35e28.png)

1.  打开 `/root/SillyTavern` 中的 `config.yaml` 进行修改。

+   `listen` 外网访问。默认`false`，改为`true`。

+   `whitelistMode` 白名单验证。默认`true`，改为`false`。

+   `basicAuthMode` 账号密码验证。默认`false`，改为`true`。

+   `username` 用户名。自行设置，不用和我的一样，用`""`包裹。

+   `password` 密码。自行设置，不用和我的一样，用`""`包裹。

+   ！`port` 酒馆端口。默认8000，本喵强烈建议修改！但要记得你改过后的端口

建议服务器的 `username` 和 `password` 都设置复杂一点，并修改端口号。

你也不想有一天截图发群友时，网址不小心露了出来，被群友用123456这样的密码把你的XP全都看光光吧？！

![](img/aed2b02cb193133e57788573aa2b4183.png)

### 添加酒馆

1.  点击 网站 - Node项目 - 添加Node项目。

![](img/41bdd1a3534c8f7bce19a9ede9cfe3e8.png)

1.  添加酒馆（SillyTavern）。

+   项目目录为 `/root/SillyTavern`

+   启动选项自定义，填`bash start.sh`

+   包管理器选 `npm` 并勾选 不安装`node_module`

+   运行用户选`root`

+   端口：默认8000，并放行端口（上面改过 `port` 酒馆端口的这里改成你的设置）

![](img/a544bf34f6373dc3f234b628cb5e465c.png)

### 启动酒馆

1.  添加后，如果未启动，点击启动。

![](img/5c634efee0e153843c555f97709671be.png)

### 进入酒馆

1.  浏览器新建标签页，输入 您的服务器IP:酒馆端口 访问SillyTavern酒馆。

[默认用户：`user` 默认密码：`password` 默认端口：8000]（改过`port`端口把8000改成你的设置）。

```
http://服务器IP:酒馆端口 如:http://123.456.1.789:8000
```

![](img/c283ebb4a6cd42bc43580fd122472edd.png)

进不去的话：

+   检查防火墙是否已经添加8000端口（改过`port`端口把8000改成你的设置）。

![](img/a9968e2b9f32b5ea3344e2e69ec240d7.png)

+   检查项目日志是否显示 `Go to: http://127.0.0.1:8000/ to open SillyTavern`，没有显示的话，点击模块管理-一键安装项目模块，然后重启酒馆项目。

![](img/2ab14d43c3fde2562a6e82b81931602f.png)

1.  首次启动会弹出欢迎页，输入你想成为的角色名字（别跟着输入水秋喵喵-_-）。

![](img/2d5441d8daa0b08ebcb32dcb538b192c.png)

！酒馆已经安装完成，如需使用API的到这一步即可根据API填写教程使用：

[连接第三方API（OpenAI格式）](https://sqivg8d05rm.feishu.cn/wiki/TS8zwRkqyiZk8SkCGRTcNMg6nMd)

下方为ClewdR➕Cookie教程

## 配置ClewdR

### 配置ClewdR

1.  进入文件 `/root` 打开`clewdr.toml`

+   `ip`改为 `0.0.0.0`

+   （ClewdR端口可修改，本喵也建议修改，修改后把8484替换为你修改的端口，下文默认未修改）

记得改完后保存

![](img/86c6015f344d968edcfc2e487acc098c.png)

### 添加ClewdR

1.  进入其他项目

1.  添加通用项目

+   项目执行文件 `/root/clewdr`

+   项目端口默认8484，勾选放行端口

+   执行命令 `/root/clewdr`

+   运行用户 `root`

+   开机启动

![](img/649195c9cb41b0e8167c705ea73e57d1.png)

### 进入ClewdR

1.  浏览器新建标签页，输入 您的服务器IP:ClewdR端口 访问ClewdR管理页。

```
http://服务器IP:ClewdR端口 如:http://123.456.1.789:8484
```

进不去的话：

+   检查防火墙是否已经添加8484端口（改过`port`端口把8484改成你的设置）。

![](img/0c7b84955d95d1b32946cbbff72e628f.png)

1.  进入后输入之前保存的`Web Admin Password` （忘记截图了，你们应该都懂吧！）

### 添加Cookie

1.  输入多个cookie，每行一个。

单个Cookie的形式是`sessionKey=sk-ant-sid01-xxxxx-AA`

可使用`https://clewd.pages.dev/`整理格式（将Cookie全部粘贴到输入框 - 提取并显示 - 点击复制）。

![](img/22f067f294848b4aa5032029b807c52c.png)

1.  添加完检查Cookie状态，查看是否被成功添加。

![](img/5a4f5d42b918d17d12e199dbd6aaa6c6.png)

## 酒馆启动

### 配置AI反代（ClewdR）

在反向代理中填入内网clewdR的地址（默认为 `http://127.0.0.1:8484/v1` ）。

代理密码填写`API Password`

模型可选择OpenAI的`gpt3.5`或者`gpt4`，ClewdR会自动选择正确的Claude模型.

![](img/80ddfe63d930502ceaa2c7033324c73d.png)

### 与AI对话

进入[酒馆入门（必修）](https://sqivg8d05rm.feishu.cn/wiki/J8NDwc06JiuHXmk5cP2csfP6nmd)

## 其他（非必须，瞎写的，研究中）

### 清理空间

使用日志清理工具

![](img/c2ecd5f1e71b546084eede5c6069070b.png)

### 更新酒馆

点击终端 先输入 `cd SillyTavern`

然后输入

`git pull --rebase --autostash`

### 宝塔助手

### 增加虚拟内存

没必要，硬盘也就10g吧。安装`Linux`工具箱

![](img/88a0dce6925b564c0cba76a26786c9d6.png)

![](img/5e7ef10c8226754f8279aaf7368e294d.png)

![](img/044275f3b7e19a297a0877e3f754e6bb.png)
