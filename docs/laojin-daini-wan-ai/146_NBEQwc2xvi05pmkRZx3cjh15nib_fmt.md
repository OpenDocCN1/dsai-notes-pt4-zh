
# Coze.com（国际版）

> 来源：[https://my.feishu.cn/docx/Luwed72BNossaKxmxQdcxKMln7f](https://my.feishu.cn/docx/Luwed72BNossaKxmxQdcxKMln7f)

## Coze海外版官方已开始收费，此方案作废，仅供参考

---

## 本教程使用前提

本教程是在初级🛰️机器人教程的基础上，使其拥有目前最强大能力的升级篇。如果你对基础还不了解，请移步人人都可以拥有的贾维斯，手把手教你怎么拥有一个初级🛰️机器人！（点击进入）

初级教程关键点总结：

1、申请LLM（语言大模型）的API（接口）
2、注册云服务器（就是让它挂在网络上，不开启随身设备它也能自己运行）
3、配置环境（云服务器的配置）
4、通过`COW`开源项目，配置🛰️机器人

请确认以上内容都已经学会，再开始本教程，否则将遇到很多卡点。

## 最强机器人教程概况

首先先介绍本教程的重点要用到的工具，大家先要知道他们的作用：

### 1、海外Coze

`Coze.com`具备免费的Chatgp3.5/4/4o/4 turbo，Gemini Flash/Pro 1.5等国际顶尖LLM，并搭配了知识库、插件（画图、搜索等你能想到的大部分功能需求它都有）、工作流等超强的落地集合。如果用过FastGPT/DIFY等平台，Coze可以轻易使用。

### 2、Discord

海外爆火的聊天软件，类似我们的🛰️，消息通过此平台进行转发。

我们需要创建2个Discord机器人（1个Discord账号即可），用于发送消息和监听消息，分别转发给🛰️和Coze。

### 3、🛰️

一个进行过实名认证的个人🛰️新号（这里很多小伙伴问过我一些上个教程卡在最后一步无法登录，都是因为没实名认证，切记要使用新号，如果不小心触发某些机制，机器人会有封号风险）

### 4、一台海外云服务器

国内的云服务器可以白嫖试用，通过代理访问海外，此方法可见张梦飞的原文，但是缺点是转发多导致的慢，以及不稳定，后期接入MJ等代理时，频繁掉线。

为了减少转发次数，以及代理的不稳定性，我在后来直接转成了海外云服务器，使用下来稳定了很多。

### 5、Coze-Discord-Proxy项目

一个开源项目，支持在Discord上发送和监听Coze消息的项目，本教程中消息转发的最核心的工具。

最精简的流程总结：🛰️（云服务器）↔Discord↔Coze

## 最强机器人教程正文

### 一、购买海外服务器

这里我只介绍腾讯云，因为我用习惯了，其他各个平台的云服务器也都可以，但根据各个平台不同，可能会有我未知的情况发生，需要自行询问客服处理。

腾讯云官网：https://cloud.tencent.com/

右上角搜索轻量应用服务器

![](img/c46b56eb4a4ea74730e53b145fd1811a.png)

直接点击“立即选购”

![](img/b87f4c4e26d3e2e5dcd574f4d14d0674.png)

按照下面的去做选择，选择第1个，30元/月的也是可以用的，我无非想网速快些选择了第2个。

![](img/0f68a89aef6056a9d72f1560160806c9.png)

付费完成后，服务器购买完成，服务器环境配置请参考初级教程（点击进入），不再复述。

服务器环境包含，请逐个参考初级教程进行安装：

1、获取服务器公网地址与账号密码
2、绑定宝塔账号
3、腾讯云API接入
4、Nginx与MySQL安装
5、防火墙开放对应端口

到此为止，先放一边，最后我们安装Docker，并配置一下就行了。

#### 搁置的浏览器窗口1，放着，下文用的时候我会提。

### 二、注册Coze（魔法环境）

打开`Coze.com`进行注册，老生常谈的`Gmail`就行，不懂得参考Gmail注册教程（点击查看）。进入后创建一个机器人。

![](img/5b7ebae3d7af21fabca600a497ac3a26.png)

创建后按照下面继续点击5区域，1-4先不用管，回头自己按照需求慢慢搞功能就行了。

这里虽说慢慢搞，但是机器人的好坏都来自于这的设定，是机器人最重要的组成部分。

![](img/42048915f8567a9d38e5f344cd32e742.png)

点击发布后，弹出这个框，什么也不用填，直接选跳过。

![](img/9bc05beb9be10e1f99d8dc211c96edc1.png)

进入最后的发布页，停在这个页面。

![](img/43818cceb6a272ffaceb99eecb7b3313.png)

#### 搁置的浏览器窗口2，放着，下文用的时候我会提。

### 三、注册Discord（魔法环境）

打开官网：https://discord.com/

![](img/8f3d71974f3126fe536e1724f65ed198.png)

同理，`Gmail`注册：

![](img/8b3f9e0c3c0f96cb727d76c27ed81e89.png)

创建服务器与频道，获取服务器`ID`与频道`ID`，记录好。在Discord聊天页面中按`F12`，然后随便说句话，查看以下信息，也记录好。

![](img/61893548645616d3aa58d64283ae2061.png)

![](img/0729ec62b00175d9bdd15ecd375d3e40.png)

**总结：**这里我们获得了服务器`ID`、频道`ID`、`Authorization`

打开Discord的开发者地址，创建机器人接口。https://discord.com/developers/applications机器人建议与我的同名，方便区分。

这里`BOT1`是🛰️发送给Coze用的，`BOT2`是监听Coze返回信息给🛰️用的。

![](img/a30d4683c30e621c71358f83ae557633.png)

2个机器人流程一致，都要获取 `APPLICATION ID`和`Token`，注意`Token`只能看到一次，再次查看需要重置，谨慎保留记好。

![](img/9fe942b72b6b03746b88125dc9750309.png)

![](img/eb449d04d04d64820a9adce8796c8138.png)

把2个机器人都设置成管理员模式。

![](img/cd1c0354c62871a68881bf4dcbabf03f.png)

还是这个页面，拉倒最下面，会生成一个邀请地址，先别着急操作，需要先打开权限，记着这个地方，操作完下一步之后，返回来复制邀请地址邀请机器人。

打开`BOT`页面，把这3个打开。

![](img/eb9a31f13d446c48b3604b692a7b873b.png)

然后返回刚刚的链接页面复制链接，打开邀请页面。授权为管理员。

![](img/e0ab7b31efb9dbcb2e6a0bfd284f7a8d.png)

2个机器人都这样操作一遍。邀请完成后，在Discord的聊天界面中，获取机器人的用户`ID`。

**总结：**这里我们获得了服务器`ID`、频道`ID`、`Authorization`、2个机器人的用户`ID`、`APPLICATION ID`和`Token`。

### 四、发布Coze机器人到Discord

把`BOT1`的`Token`，复制到搁置的浏览器窗口2，然后点上Discord前面的对号，进行发布。`BOT1`的名字在Discord中会变成Coze中`BOT`的名字。

![](img/72e6e0d0bd724c2147700c94b86870ea.png)

### 五、配置机器人

返回搁置的浏览器窗口1，也就是已经下好`COW`的云服务器那。接着进行操作：

1、下载Docker环境

点击立即安装，选择第一个阿里云镜像即可。

![](img/e9c27f2cc167c870bd4d1a1b1a24847d.png)

![](img/0a5c2a85c52e1d3c0cdcc3e11d06963b.png)

完成后刷新界面，看到如下界面则安装完成。

![](img/b04e419608e265d3dfc09a36ca8b4369.png)

选择左侧列表中的【终端】，输入以下代码：

```
mkdir -p /data/coze-discord-proxy && cd $_
```

![](img/c79300d6bad27cbeaeb20a3ea316623b.png)

找到这里，创建Docker配置文件，新建文件，命名为`docker-compose.yml`。

![](img/198bcc89802122cacf35e408be0ec9f3.png)

![](img/1890a82adfcf063a543cdf64990de4f3.png)

双击打开文件，把下方代码，全部粘贴进去。然后把该替换的ID和`token` 替换进去并保存！

```
version: '3.4'

services:
  coze-discord-proxy:
      image: deanxv/coze-discord-proxy:latest
      # 如果你的网络环境访问docker hub不稳定的话，可以把上面的镜像地址改为下面的地址进行下载镜像 
     #image: dockerproxy.com/deanxv/coze-discord-proxy:latest 
     container_name: coze-discord-proxy
     restart: always
     ports:      
     - "7077:7077"    
     volumes:      
     - ./data:/app/coze-discord-proxy/data    
     environment:      
     - USER_AUTHORIZATION=MTA5OTg5N************uIfytxUgJfmaXUBHVI  这里替换成我刚才让你保存的那个AUTHORIZATION      
     - BOT_TOKEN=MTE5OTk2xxxxxxxxxxxxxxrwUrUWNbG63w    这里替换成 Coze_bot2 的token      
     - GUILD_ID=119xxxxxxxx796       这里替换成 刚才保存的服务器ID      
     - COZE_BOT_ID=119xxxxxxxx7   这里替换成 Coze_bot1 的  ID  ，注意是ID       
     - CHANNEL_ID=119xxxxxx24        这里替换成 保存的频道ID      
     - PROXY_SECRET=sk-123456        这里不变！      
     - TZ=Asia/Shanghai
```

保存后，在这个文件位置的上方，点击终端。一行一行粘贴以下代码：

```
sudo systemctl restart docker
docker-compose up -d
```

![](img/e045c68c7ba90e54117ef3d2ecd352bc.png)

进入`COW`项目文件夹，如果你完成了初级教程，它的路径在`root/chatgpt-on-wechat`。

![](img/199ea26a44a2a33ba372742526bfdefd.png)

把前三行内容配置并替换。

+   [※ `COW`主目录`Config`说明](https://tffyvtlai4.feishu.cn/wiki/Ti7uwsKgiiW6bpk2nmQcVhQ8nnb) 如果有报错，可能是更新导致，请查看是否缺少配置。

```
"model": "GPT-4",
"open_ai_api_key": "sk-123456",
"open_ai_api_base": "http://这里是你的服务器地址:7077/v1",        把文字部分替换成你的服务器地址
```

参考如下（这步仔细检查，很多小伙伴这段配置的字段名和`key`是错的）：

![](img/1ec3bdd0a21d291039b2f377ca3ec0eb.png)

### 六、开放防火墙

增加`7077`端口，并开放，是`coze-discord-proxy`所用的。

如果按照梦飞的国内代理方法，则需要增加`9999`端口。

![](img/28b39d162e6e54ef66f52f2e379e87cd.png)

### 七、完成&扫码登录

宝塔界面，右侧选中文件，`root/chatgpt-on-wechat`下，选择【终端】，输入以下代码运行扫码登录即可。

```
nohup python3 app.py & tail -f nohup.out
```

![](img/962994d7702fcc9e0e9c7c36ffc22988.png)

以上，全部教程完成。

恭喜你拥有的宇宙最强的🛰️机器人！

## F&Q

1、如果无法联通，请仔细检查`Coze-Discord-Proxy`和`COW`中的`Config`中的配置。

2、本项目因Discord限制，每日有使用次数，GPT4系列每天100次对话，次日早9点重置次数。

3、可以在Discord中，先@下机器人看看是否有反应，有反应的话，请仔细检查`BOT1`和`BOT2`的配置。

4、提示词走Coze中的配置，插件调用也是，因此Coze的提示词尤为重要，绘画、搜索等等都是通过语义或者工作流调用。

5、如果所有配置检查没问题，那么检查 【腾讯云】和【宝塔】两个地方的防火墙是否增加了`7077`，并确认开启。

6、如果使用的是国内服务器以及代理形式，会有很多不稳定的麻烦发生，总价一年也就差个100多元钱。

7、Discord白嫖每天限次，因此需要多账号实现负载均衡，详情参考Discord账号多配置方法。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
