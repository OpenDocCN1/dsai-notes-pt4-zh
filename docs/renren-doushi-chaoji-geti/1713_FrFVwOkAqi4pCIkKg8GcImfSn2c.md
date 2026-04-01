# 小白搭chatgpt代理！0基础实操的无敌详细图文帖

> 来源：[https://xuqiwei1986.feishu.cn/docx/BHa0dG9fxoExg7xvgpScEL9Gnxb](https://xuqiwei1986.feishu.cn/docx/BHa0dG9fxoExg7xvgpScEL9Gnxb)

只需3个环节：需要准备：一个[服务器vps](https://c1n.cn/UnHK0)、自备一个[chatgpt key](https://platform.openai.com/account/api-keys) （不懂如何申请gpt，想白嫖？[戳这里，楼主给你找个插件](https://vw33bncyqrt.feishu.cn/docx/BHa0dG9fxoExg7xvgpScEL9Gnxb#Seisd2YIAoWQyoxSAUZcZlabnYc)）、一个qq号。

（想学搭微信的gpt？我写了另一篇超详细教程： [简单！小白0代码搭微信chatgpt，无敌图文教程！](https://vw33bncyqrt.feishu.cn/docx/PfymdoumHoaBYUxyLyycjTpznqc) ）

本文全程图片介绍，简单清晰，好处：

1、随时随地可用，不担心科学网络问题

**2、手机、电脑均可用**

3、可浏览器访问，也可在QQ中使用

![](img/9cf7cd79b279957ce2512a76ca140e1c.png)

![](img/f7b96e833d6ee71b235a7a1b432e052b.png)

# 第零章：教程说明

Hey hey hey！听说你想学习如何搭建一个真正能和你聊天的AI助手？那你可来对地方了！我刚刚写了一篇神仙级chatgpt搭建教程，让你轻轻松松愉快地玩转这个AI世界！

嗯？不会申请服务器？没关系！我的教程图文并茂、详细易懂，为你从服务器的申请到环境的配置，全方位呈现出每一个细节！能作为一只小白都不必惧怕~

哪怕是原来不想学技术，看了这篇超级吸睛的教程后，相信你也会重新对它心生热爱。更爆炸的是，我还会告诉你如何在QQ上让chatgpt随时随地为你服务！每次的无聊时光如此取消！

来吧，别再拖延了！相信我，用一个下午的时间学习这篇教程，你将打开一扇前所未有的大门，那里有无限可能等待你的探索！

**嗯，这段就是在QQ中用****ChatGPT****生成的**

![](img/8439f65e98a9d3c6cc2b852c48288e66.png)

接下来，进入真正的教程吧~

# 第一章：申请服务器

[**这里以vultr为例，**](https://vw33bncyqrt.feishu.cn/docx/BHa0dG9fxoExg7xvgpScEL9Gnxb#JgiSdMyYUoYUCmxCcM1cI9TznQc)**其他服**[**务器例如 **](https://vw33bncyqrt.feishu.cn/docx/BHa0dG9fxoExg7xvgpScEL9Gnxb#Vs2udUWYGoGgMGxykLhcTPk4nXf)Sugarhost官网 、 腾讯云官网 也可以的（不推荐阿里云，后面有写原因），最低配就能满足了。

**购买服务器地址：****ht****t**ps://c1n.cn/UnHK0 （点链接送100额度，限1个月，可以多开几个服把它撸完）

## 1.1 注册账号

用你常用的邮箱，注册vultr账号，qq邮箱也可以的

![](img/e778f814123b1aa325f8031146eeca21.png)

## 1.2 配置一台服务器

**在Products界面，点+，然后选第**一个 Deploy New Server

![](img/d63d617455f184890c0b3b98d5fb5f1a.png)

型号，选最基础的 Cloud Compute 即可

![](img/41199fdddf61b89f7de37cdb4bd28e46.png)

**地区，可任意选（注意查一下****ChatGPT**不支持的地区），为了访问速度快，建议 Tokyo

![](img/52ffe1db52300ba67eb0eff60833242b.png)

**系统，我用默认的Debian11** （如果你有一定的技术基础，用其它也可）

![](img/709c299f9a81f778d52ab0e8b5d5df56.png)

备份功能对我们没用处，可以关闭，省点钱

![](img/6c09656b3f284a75fc49bd4fd5972f37.png)

**服务器配置，也选最小的**

![](img/0fa793349a6b9719a95c2d07090bed37.png)

**最下面的服务器名，随便你自己填。**如果你要申请很多服务器，可以用来区分用途

然后点击 Deploy Now ，提交申请

![](img/310e0b466d554c32ece63cc0e73013d8.png)

等待1分钟，服务器申请好了，显示 Running 运行中

![](img/676ac7d54e48bde74f009e2781e52b8b.png)

点击进去，可以看到配置，包括 IP地址  服务器账号、密码

![](img/40a64a1668bb3ee5024faaef019df2ba.png)

以上，已经拿到了服务器。（有些ip可能访问有异常，可以ping一下，不会ping也没关系，后面会有其他方法）

## 1.3 ping检查ip地址是否正常

**Win****dows**电脑，点击键盘的windows+R，弹出命令窗口，输入 cmd，点确定

![](img/ea57490df6b8d1d9a2aab1ed5bb5597c.png)

输入 ping + 你的ip地址，回车，看输出信息

**如果界面上显示 “请求超时”，那这个服务器**在国内就访问不到。需要重新申请一个服务器

![](img/bd56905863474751726d0f943e4da584.png)

# 第二章：安装服务器环境配置

## 2.1 XShell安装

这是个免费的工具，打开XShell官网 https://www.xshell.com/zh/xshell/ ，点下载

![](img/c690082ccb7856c5d8cf2ddd41a06f2c.png)

许可类型，找右边的 免费授权页面，点击进去

![](img/d2a7167bf142caaf00a2ca566e3936a0.png)

**输入你的信息，姓名随便填，邮箱能收到邮件**即可。点击下载，会把下载链接发到你的邮箱

![](img/d0bf8e6fd0f27d6a08e41499a288f8b2.png)

**下载后，安装。这个不用教吧**

![](img/2a756518c440b9afe106182caeb6d620.png)

**启动****XShell**，点左上角 + 新建一个会话，准备连上你的服务器

![](img/4483b528ad11a40afda94d13c59404ac.png)

**在配置页面，输入你的服务器ip。 名称随便填**，也是为了区分多个会话的。 然后点 连接

![](img/e37ac0914d473d02408fb6712ea48edc.png)

第一次连接，会弹出一个SSH警告，选 接受并保存

![](img/33926616929a52902d556219fdfe596d.png)

**弹出一个登录框，填入用户名，一般都是 **root。 在你服务器的配置页面上能看到

![](img/75a71ea2d1e537b30f32c098ba2112e8.png)

![](img/4b5d75fb7a02f1b0b27ae3015b56f6da.png)

输入密码，也是在配置页复制过来。点 记住密码， 确定

![](img/c4cead8e09da9f66124645d20bd7079c.png)

**一切顺利的话，就会登录进去了。**

**如果不能登录，或者登录连接很慢，说**[**明ip受限了**](https://vw33bncyqrt.feishu.cn/docx/BHa0dG9fxoExg7xvgpScEL9Gnxb#DoKQdyUICo8smuxSIcxcJZzBnce)**，请直接注销**机器，重新注册（跳转重新注册 成本很低）

![](img/26af09b9432cdebfe56b3027cf83994c.png)

## 2.2 Koishi安装

**Koishi，是国内某大佬开发的开****源工****具**，里面有很多一键部署插件，包括免费AI绘画、免费Chat、资讯监控、QQ转发、Discord转发、飞书转发等，很不错的一个平台。有能力的还可以自己开发插件。

**当然这些需要花时间研究，目前我们用到的插件就**2个，我会直接告诉你。怎么配置，怎么用。

![](img/450627f3fb9eef6fb46647a9f384262e.png)

**为了使用插件，我们要在服务器上安装Ko****ishi（电脑**本地也可以安装，但本地你连ChatGPT需要科学网络，你懂，而服务器已经具备连接ChatGPT的环境了）

**PS：先说一个小tip，****XShell**的界面支持 粘贴，把我列的命令依次粘贴即可

![](img/3273febc084a74466337172f7888146f.png)

**好，接下来，准备进入安装了**

（点击链接 [Koishi安装命令可复制](https://vw33bncyqrt.feishu.cn/docx/Pq0ddFqAvoUhidxGuKUcmnIInVe)）

复制命令，粘贴到服务器命令中，安装所需环境。等一句执行完，再贴下一句：

```
sudo apt-get update
```

```
sudo apt-get install apt-transport-https
```

```
sudo apt-get install ca-certificates
```

```
sudo apt-get install curl
```

```
sudo apt-get install gnupg
```

```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

```
echo &quot;deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable&quot; | sudo tee /etc/apt/sources.list.d/docker.list &gt; /dev/null
```

```
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

如果遇到命令，询问是否继续(y/n)。输入y，回车即可：

![](img/d1a9470ee3957c1998bb5ad72cc6ced4.png)

最后一条命令，安装并启动koishi

```
sudo docker run -p 5140:5140 koishijs/koishi
```

**安装完成后，在浏览器输入 ****你服务器的ip+端口5140**，即可访问。例如: 1.1.1.1:5140

![](img/84692d6b024c37e9c0835dd1fe5e4f82.png)

恭喜完成2/3的教程，马上就能用上了！

# 第三章：Koishi配置

欢迎进入Koishi，你看到的是一个界面控制面板，最左侧有一系列的功能

![](img/9cb5902a6893f05abdf1c49d76e16c1b.png)

## 3.1 安装2个插件

**我们需要点击左侧的市场，安装 davince 、**adapter-onebot 这2个插件

![](img/01f3298ee6f1f4f7422fea126f7298eb.png)

**搜索 ****chatgpt**，会看到第一个 davince-003，点击添加，并安装

![](img/26601cad9ad7395bba2c3a3529183272.png)

![](img/352926f03aa8cceee930a1d7a52fe71f.png)

搜索onebot，同样进行安装（有可能默认已经帮你安装好了）

![](img/96323363cd7b1a4720ec18a8b8225cd7.png)

## 3.2 启动onebot插件

首先启动onebot，是qq的代理（原理是，用你的qq号转发消息）。

点击最侧的配置，列表里找到 adapter-onebot

![](img/ab2a7eeab3754bb206eb24321a0f3a8e.png)

**在配置页，机器人密码就是你的qq密**码，机器人账号就是你的qq号。都填入进去

![](img/10615030ed8ab508a0ecff1a82fe50f3.png)

注意打开 自动创建go-cqhttp子进程

![](img/7d7e3bd48f07fd2982e24cf5df566fea.png)

**点击右上角的 ▷ 进行启动**

![](img/966490f5bb45a2d9b620d56d3bf96e67.png)

**有需要的话， 也可以 √ 重启**

![](img/8b35ffee58796389754ffba2433e794e.png)

**会弹出qq的验证方式，你选一个，**然后验证qq安全（相当于把qq号登录了）

![](img/ecb21f1e2e2c42e7795d27dad414f30b.png)

**验证成功后，点击重新启动：**

![](img/5c015fa7c40ea797f3ac0f0deb9fc04f.png)

**顺利的话，会显示连接成功：**

![](img/699cd3e7e5012dfd9e0eca1b4531c73c.png)

## 3.3 启动 davince 插件

**同样在配置页，****找到davinci插件**，输入你的chatgpt key，点 ▷ 启动

![](img/95fa5884af7ad508901c8231e44620ba.png)

**改配置后记得重启**

![](img/582e10fa8c1f32690f49c5e7d467d7b1.png)

**key的获取地址：**platform.openai.com （这里需要自己解决，买，或者找朋友）

![](img/849db4fcedc5f5e99938df2ae1659a64.png)

**别名，也就是暗号****，是在给****ChatGPT****发消息时的前缀。**

**例如****截图中有3个别**名，你可以发  “ai 请给我写一段小红书介绍我的产品”， 实际ChatGPT收到的是 “请给我写一段小红书介绍我的产品”

![](img/9791a3c7f920354a797288c6707bb472.png)

**你也可以改成 “呼叫大刀”，后续就发 “呼叫大刀 给我来一段r**ap” ，“呼叫大刀 你的大刀有多少米”

![](img/589b68f407a2e689c500cfdc0b07b0db.png)

**另外，配置项可根据需求修改，**

**例如：这里有内容审核功能，可以去百度申**请，价格也不贵 https://ai.baidu.com/solution/censoring

![](img/c9219de730951f39a9f0c5cf0bed6538.png)

# 第四章： 欢迎来到QQ分身ChatGPT

**现在，QQ环境的****ChatGPT****已经准备就绪！**

**再啰嗦一下原理****，前面配置的**qq号，被当做了一个机器人，转发你和ChatGPT之间的消息。

**那怎么用呢？比如 我想自己跟****ChatGPT****聊，或者 我想拉朋友一起玩**

**有两种常用使用方式：**

## 4.1 浏览器直接与ChatGPT聊（不需要科学网络）

在koishi左侧，找到沙盒功能，任意添加一个用户，然后直接聊天

![](img/3823daa9b62c6a50ef036fd0219585f2.png)

![](img/193eb7377242acd51e22b8f009250d7c.png)

如果你想让朋友用，简单的方式是把你的Koishi首页链接转给朋友。

![](img/7fc0567f1e909531e54b2630851660d6.png)

**如果怕被随意传播，可以额外配置一个账**号登录功能，在插件市场搜索安装login

![](img/8e7a6ce2acabcd3d4b68c01bece13da7.png)

**然后配置一个登录账号**

![](img/c7404f080a68d3d6c876599b6a06afac.png)

## 4.2 在QQ中私聊、群聊（不需要科学网络）

**你上面用来配置的qq号，现在承担了一个机器人的**功能。当然，日常的其他功能还是正常用的。

**那么，你的朋**友可以直接发消息给它，记得带上暗号

![](img/e7020adaeb01a81c7875028910dab687.png)

**也可以****建一个群聊**，把你的机器人qq拉进去，任何人发的暗号它都会收到。

**例如，我发了一段暗号“****ai**** 请帮我介绍我的教程，鼓励大家来学习”**，嗯，生成的文本在教程开头，你已经看到了

![](img/e33faf77a372f4d2d4fb0675fd9e696f.png)

# 第五章：其他说明

## 5.1 其他常用服务器

[Dig](https://c1n.cn/dh70p)italOcean、Linode、搬瓦工等，可自行搜索

### 5.1.1 [Sugarhost](https://c1n.cn/A28EL)服务器

也是对国人[操作非常友好的一](https://c1n.cn/A28EL)个服务器品牌，中文界面，流程也很友好清晰（官网链接：戳我直达官网入口）

**打开主页，选 ****VPS****云服务器**** -> Linux云服务器**

![](img/ff7e22f2f3d06596eef52c037c9b0b45.png)

选择北美-月付，这样花钱少。如果想长期用也可选年付

![](img/963c66f943343b169dcd817ee0cf53e9.png)

**主机命名一下，后续登录要用**

![](img/b2e31905113011440ee12d9d95368250.png)

**系统选****Debian**** 11**

![](img/3d920fe7e329c77d5e67eb87a1a3a13b.png)

**点击 结账。后面付款不用教哇**

![](img/43d1098fa8687639e7b37b6b0093feaa.png)

**购买后的服务器，在这里**

![](img/eb8e826772c3a51ef3feff3fcfebae00.png)

**点击进入**

![](img/d333ef8702e578585ffe51242b8093ac.png)

有可能正在创建，那就等一下。 账户名、密码也在这个位置了

![](img/26aecd1aa022a6dfa6f4c26853a83324.png)

**后续，跟前述****的安装过程一**样了 ，拿到 用户名、密码，用XShell登录进行配置

![](img/83e7aa7180f4e6de1c37dd71daca33c6.png)

啊对了，别忘了从检查ip速度开始 戳我直达 ping检查ip教程位置

**服务器配好后，记得****先打开端口防火墙**，否则后面会安装失败。这里以默认5140为例

![](img/b255f12e2146f4982848eb5ce36574ca.png)

![](img/4ee2ca0f34a1e6887be310e690ddfb94.png)

**配置 入 规则，记得启用**

![](img/cb175994cc0b5e55fc4b7c4026789a8f.png)

**配置 出 规则，启用**

![](img/6063bd8ccc4d8ee30788392394fc7906.png)

[**然后就可以按照 之前的命**](https://vw33bncyqrt.feishu.cn/docx/BHa0dG9fxoExg7xvgpScEL9Gnxb#JWSid4uksoI4E6xYTmfcqZ8qn6g)令开始安装了 戳这里回到 安装命令步骤

### 5.1.2 阿里云服务器

**我试了几次，会遇到****DNS****解析的问题，新手还是别忙活了**

### 5.1.3 腾讯云服务器

**腾讯云，国内第二大云服务商，也是有****提供一些 新**[**加坡 东京等地区的服**](https://c1n.cn/qMUEm)务器的，找最便宜的：戳这里直达服务器入口

[**国内服务器，登录、**](https://c1n.cn/qMUEm)支付、找客服都比较方便，我们选 最便宜的轻量服务器

![](img/7596e7178e00667a0ce4157748f9d3be.png)

**进入控制台，点击选购**

![](img/d82969c2864c6793ac9a162b8f791f5e.png)

**创建方式，选 容器镜像**

![](img/82e92a0f5d1482f085038141392f054a.png)

**镜像用默认的即可**

![](img/a324f92571ece649bbd991b603f589c7.png)

地区选 亚太， 新加坡 东京 选一个吧

![](img/8dbe2787880206afcc2cc874f11355a2.png)

**套餐选最便宜的即可**

![](img/30840fe9530c8791bf8fcfd5ff911d90.png)

实例名称随便填，服务器多的时候方便区分的

![](img/31239d056627dcbd70f957a14f3af866.png)

去掉自动续费， 选1个月， 购买

![](img/bfff61c9ab61c9af83e2176000dfb331.png)

**付费**

![](img/e51615867beed15baa72a295803166c1.png)

**找到服务器入口**

![](img/efbd52b28821958a6ed7b585e67055c1.png)

**查看详情**

![](img/13922a31c405db54e1c8164cda7889c5.png)

**进入控制台后，还是先设置防火墙**

![](img/1a843c4d77e392e2c438505d89b1efe7.png)

**把配置填好，确认**

![](img/09ab86cd55f130844065debd7ea315e1.png)

**设置服务器密码**

![](img/4642f40ce22914092c029147b65a2482.png)

**填写你的密码，确认**

![](img/b035908e807e9069a095bcbedbc4d067.png)

**买服务器的时候选了Docker，就免去**了很多安装内容。直接安装koishi吧：

```
sudo docker run -p 5140:5140 koishijs/koishi
```

有服务器的使用问题，也可以看[腾讯帮助文档，或者找客服](https://c1n.cn/EsH2n)

**然后就可以 **[**戳这里回到 服务器配置步骤**](https://vw33bncyqrt.feishu.cn/docx/BHa0dG9fxoExg7xvgpScEL9Gnxb#WkqQdoUsios2AexkzIQcjTQPn6f)

## 5.2 Koishi求助

koishi官方整理了不少文档，可以参考查阅 ：https://koishi.chat/

另外，也可以搜索官方的QQ群 “Koishi 用户交流”， 群号：391463133

早苗插件官方帮助文档：yuque.com/km57bt/hlhnxg/

## 5.3 白嫖GPT服务

gpt服务目前有很多代理，也有很多免费服务。同样，在koishi中也有免费的插件服务，用来跟朋友玩、引流，也足够了。当然受限于免费，它可能不如你自己的plus账号那么好用

在koishi插件市场，搜索 早苗 ，添加这个插件

![](img/d981c527789e3999d97ba69d7d202e15.png)

**在配置中找到它**

![](img/36274394e45839e5a41edd0398551645.png)

根据提示，选定一个角色，在范围内随便选一个编号填入

![](img/3b90a8dc14a6033bcb46af53231165da.png)

点击启动

![](img/ef919483d162f77a12d2066cb7f07ea2.png)

然后在qq中，发送暗号 “早苗on”，即可启动机器人服务

![](img/96d081c1da0f00bcb1cf9288f658d196.png)

**早苗官方帮助文档，**yuque.com/km57bt/hlhnxg/

**发送暗号 ****ai** +  你想问的问题，即可。例如：

![](img/27ea7fde531c69df461cec2cb3048cb9.png)

**早苗****是一款非常优秀且作者非常用心维护的免费插件。它**的用处不仅限于 ai聊天，还包括ai画画、游戏、互动、玩梗、查天气、报时等，好好利用它可以大大的活跃群气氛

![](img/7418e3011488942383265d672231fdfc.png)

## 5.4 其他Tip

a、vultr如遇ip无法连接，或连接很慢，先申请新服务器，再释放旧的（否则很可能给一个相同的ip）

b、sugarhost如遇ip问题，可找到 服务单，发信息跟客服沟通

c、qq会不会被封？ 把控使用节奏，群里真人的聊天信息多，就不容易封。另外插件的功能也可以研究下，有改提示语、控制频率等

d、注意每种主机的权限控制可能稍有不同。 每条命令执行，如果遇到 Y/n  都记得输入 Y

e、http://198.211.33.209:5140/market 我搭的服务，随便玩，玩坏了重启。目前只买了1个月

f、有些服务器默认防火墙比较严格，看到后台上有配置的，都先开启5140端口，再进行安装

g、发暗号，记得加空格

h、[Koishi安装命令 可复制](https://vw33bncyqrt.feishu.cn/docx/Pq0ddFqAvoUhidxGuKUcmnIInVe)  安装命令可以看这里

i、插件尽量都更新到最新版，保证匹配

# 六、如何搭微信的gpt？

> 我写了另一篇详细的教程，同样的无敌实操帖，很多朋友点赞好评：

## 简单！小白0代码搭微信chatgpt，无敌图文教程！

有疑问评论一下，我看到回复，或者大家讨论~

欢迎一起来探索，有好玩的ai社群 求带~~~