# Claude账号注册与API-KEY获取流程

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/CJoWdhX0Foq7DFxqAAScXbxjnVd](https://kq4b3vgg5b.feishu.cn/docx/CJoWdhX0Foq7DFxqAAScXbxjnVd)

一、SMS-active账号注册（虚拟手机号）

二、Claude 辅助邮箱注册流程   1.开启网络工具，使用全局代理 2.微软outlook邮箱注册

1.开启网络工具，使用全局代理

2.微软outlook邮箱注册

三、Claude账号注册与API-KEY获取方法 1.Claude-Chat账号注册

1.Claude-Chat账号注册

2.ClaudeAPI模型账户绑定

3.ClaudeAPI账户升级与API-Key创建

4.Claude模型API的本地代码环境调用测试

## **二**、SMS-active账号注册

在稍后的Claude 注册流程中，会反复涉及手机号验证的环节，这里我们就需要使用代理服

务，来使用其他国家地区的手机号来接收验证码，进而完成账户注册。这里推荐使用SMS进行其他  国家地区手机号的验证和激活，登录https://sms-activate.org/，即可完成相关服务的购买。点击进 入sms-active：

![](img/781558133effbc48147b9a114cf8f1bb.png)

首次登录时点击左上方小人图标，进行注册，这里可以使用国内QQ邮箱或者outlook邮箱进行注 册，大家自行完成注册即可：

![](img/f0103854de8e792ca20500ec0aaeff9e.png)

注册完成后，需要注意，该网站提供的手机号是根据不同服务需求来匹配和分发的的，而在申请手   机号之前，我们需要往这个账户里面充值，这里可以使用支付宝进行充值，并且只需要充值2美金即 可使用较长一段时间：

![](img/c01fe2e470ab6e0db07ece94bdbe3312.png)

点击支付宝进行充值：

![](img/26a2f696db35cf76ae09b62ec53e0324.png)

然后选择支付2美金：

![](img/14babb04184934740d334269db2f2bc4.png)

然后扫码完成支付即可。

早年OpenAI未火之时，最低充值额度是1美金~

至此我们就完成了SMS账号注册与储值，接下来我们将借助SMS平台来完成手机号验证。

## **三**、 Claude 辅助邮箱注册流程

接下来开始进入到Claude 注册流程中。需要注意的是， Claude平台并不是传统意义的通过账号  和密码登录，而是通过注册邮箱+邮箱验证码的形式进行登录，因此需要首先完成邮箱注册，且根据现阶  段使用情况来看，微软outlook邮箱和hotmail邮箱相对稳定，这里推荐使用outlook邮箱进行Claude账户

注册，并且在邮箱注册过程中使用国外虚拟手机号进行验证。

### **1.开启网络工具，使用全局代理**

### **2.微软outlook邮箱注册**

接下来介绍outlook邮箱注册流程：

·**  ** Step 1.开启无痕浏览模式

为了确保能顺利完成注册，推荐使用谷歌浏览器，并开启无痕浏览模式：

![](img/5a8a5ce9d3f5cdb453156150c4bff154.png)

![](img/03942cb191fedb7f5b378ab3a785d5e7.png)

.  Step 2.进行outlook邮箱注册

然后在网址栏输入： [www.outlook.com](http://www.outlook.com/) ，进入outlook官网，并点击“创建免费账户”：

![](img/adcf43cef48b239d7b1467558710b88a.png)

然后点击同意并继续

![](img/360eb590561c70ef03f620a0082d9207.png)

然后选择输入的邮箱名称，自定义名称即可：

![](img/6ab3641c5a739d6d81bb5ebdd887ccf2.png)

然后输入密码：

![](img/d36fb647600e5f088cbfeea88742ae95.png)

填写姓名，随意填写即可：

![](img/024757a181b627cded52de2733243e4e.png)

设置国家和地区。在全局代理开启的情况下，默认地区就是代理地址。这里推荐选择美国、英格兰 或者巴西均可，方便后面使用对应地区虚拟手机号进行验证：

![](img/7b3c6da0a8caf8ba8581d4e6442187a4.png)

接下来就进入到手机号验证环节。需要注意的是，尽管这里可以使用中国地区手机号进行验证，但 为了保险起见，最好还是使用其他国家地区的虚拟手机号完成验证。这里需要借助SMS-active平台 进行手机号验证。

·**  **Step 3.虚拟手机号验证

接下来回到SMS-Active页面，在页面左侧的服务选择中输入Microsoft，找到能够用于辅助 Microsoft产品注册的虚拟手机号：

![](img/ceeb7fbdde39b33648dc8afbe7d71e3a.png)

点击下方Microsoft，即可查看到一系列不同地区的虚拟手机号，这里可以选择巴西，然后点击右侧 红色购物车按钮，即可租赁一个虚拟手机号：

![](img/8d4c2a7bcf39815ed5418231c7914556.png)

在页面的中间能看到租赁到的虚拟手机号，其中+55是国家编号，之后的数字就是具体的手机号：

![](img/971205e670a088806cad838c844f82a5.png)

复制手机号并输入到outlook邮箱注册页面，点击发送验证码：

![](img/1e2c5adc6497452ba2d501afce8cc5a8.png)

然后回到SMS-Active页面，稍等片刻即可查看到当前手机号接收到的验证码：

![](img/33686106c811b5a6e3a5f2e76d6f0b0d.png)

将验证码输入到outlook邮箱注册页面，即可完成注册。

![](img/07570837fce2d2725d69d8c47c26a173.png)

.  Step 4.登录outlook邮箱

需要注意的是，如果完成注册后页面没有自动跳转，则在浏览器地址栏再次输入outlook.com 进行登录即可。登陆后邮箱页面如下：

![](img/6df274658fda4d75c940b22bc21f1a7a.png)

至此，我们就完成了outlook邮箱注册全流程，接下来我们将使用该邮箱完成Claude账号注册。

## **四**、 Claude账号注册与API-KEY获取方法

和OpenAI产品架构类似， Claude的账号体系同样也是分为Claude-Chat账号和Claude API账号，就 类似于ChatGPT账号和GPT API账号。在注册Claude时需要同时注册Claude-Chat账号和Claude API账号才可稳定运行。因此这里先进行Claude-Chat账号注册，然后再进行Claude API账号绑定。尽管看起来像是两次注册，但流程基本一致，整个过程并不复杂。

### 1.Claude-Chat账号注册

[C](https://claude.ai/)[laude-Chat是一个基于Claude模型的网页](https://claude.ai/)端聊天机器人，功能定位和ChatGPT类似。点击链接： https://claude.ai/即可进入到Claude-Chat登录页面。在登录页面中输入刚才注册的outlook邮箱地址：

![](img/ad8c2e41dab9b6b6dfa3d4b5383c6cdb.png)

然后点击发送验证码。回到outlook邮箱页面接收验证码：

![](img/86701bdc8b1fc7ae1edc6f41246034ad.png)

将验证码输入到Claude-Chat注册页面中：

![](img/32321fdb8970023d69afbee38cd90964.png)

![](img/53ee39dd7244e7090b532a1ac6c00d36.png)

在完成邮箱验证之后，还需要绑定手机号。这里同样是需要使用SMS-Active借助虚拟手机号完成验证：

![](img/633a20e1a63515fc185a256a959d15cb.png)

回到SMS-Active页面，点击左侧服务选择，在搜索栏输入Claude，点击Claude搜索结果：

![](img/c97f848d27b12a49ef29869e21317b75.png)

即可查看目前全部可以用于Claude账户注册的各地区手机号。这里可以使用英格兰地区手机号，点击右 侧购物车按钮，即可租赁英格兰地区虚拟手机号：

![](img/d28dd84669911a2fc39abf8a0e8ecff2.png)

租赁完成后在页面中央查看并复制手机号：

![](img/85c5e304a80a8866cf9d3ea60d17465e.png)

将手机号输入到Claude-Chat注册页面中：

![](img/aa682f48f8a9a9e1720ad38ec30e9e79.png)

并点击发送验证码。回到SMS-Active页面查看验证码：

![](img/91d578eb9b495f4cb4b5dcf048e792ce.png)

将验证码输入到Claude-Chat注册页面中即可完成注册。：

![](img/42c702ca2ebbbd6188053bfecdd25fae.png)

注册完成之后，会自动弹出一个简单的欢迎页面，其中会介绍Claude模型基本情况，以及需要用户输入 一个合适的对用户自己的称呼：

![](img/c947643f85623485d401649d0e3a23fa.png)

![](img/d7f2f77b6812eb74aaab94375cb013a4.png)

一路点击，即可最终进入到Claude-Chat聊天页面：

![](img/aa738e915121a59253ac4392354753d0.png)

至此， Claude-Chat账户注册全部完成。需要注意的是，下次登录时还会需要通过邮箱验证码进行登录， 但无需再进行手机号验证。同时，再默认的免费模式下，  Claude-Chat只能调用Sonnet模型（Claude  的中号模型）进行对话，若想调用Opus（ Claude 的大号模型），则需要进行付费订阅， 20刀/月升级  到Pro版。在当前页面中，点击下方的upgrade to Claude Pro，绑定银行卡（国内用户需要使用虚拟信  用卡）即可完成升级。

### 2.Claude API模型账户绑定

在完成了Claude-Chat账户注册之后，接下来继续完成Claude API账户绑定（绑定到Claude-Chat账  户中）。点击右侧链接： https://console.anthropic.com/ 进入到Claude模型控制台页面。首次进入会提 示完成注册，这里使用和Claude-Chat账户相同的邮箱即可完成账号绑定，无需再次进行手机号验证：

![](img/c8dc6ccf33dc7cf34019a7140f07ad58.png)

这里输入此前注册的outlook邮箱即可完。然后回到outlook邮箱主页查看收到的验证码：

![](img/f587fcf2a4ccc73b7afd4e37d70a24e5.png)

将该验证码输入到Claude API账户注册页面即可完成账号绑定。由于该账号已经注册了Claude-Chat，因 此无需再次进行手机号验证。接下来会直接进入到一个设置组织名称（所在公司名称）的环节，这里随   便输入一个公司名称即可：

![](img/06a095e84fc48871e38409c858e3496b.png)

然后点击创建账户，即可进入到Claude API账户控制台（主页）：

![](img/62f42f39ecf62bdf29f2c49a2844967d.png)

至此便完成了Claude API账户注册流程。

### 3.Claude API账户升级与API-Key创建

· Claude API账户升级

刚注册完的Claude API账户是免费账户，需要领取初始免费赠送的额度才可顺利调用Claude 系列模 型。这里点击页面上方的Claim按钮免费领取5刀额度：

![](img/e55ac739de935704160e59530fee2be0.png)

点击之后会要求进行手机号验证。

![](img/f49f46769e0f97cec9b245ce3d013216.png)

这里我们再次使用SMS-Active进行手机号验证。这里可以继续使用此前租赁的手机号（如果没过期的    话，刷新即可再次接收验证码），也可以再租赁一个新的手机号都可以。这里重新租赁了一个新的手机 号，注意还是需要选择用于支持Claude的手机号：

![](img/d1c7bed67760a12ab2255e5e597946f9.png)

输入手机号：

![](img/9ec38299c7ec0f6382df491ac3360d55.png)

接收验证码：

![](img/a3c646008c6013ced9a876838cfe9a66.png)

输入验证码：

![](img/31338023c02b97e1623a4807758f3b3d.png)

再次回到主页面时即代表已经完成5刀免费额度领取：

![](img/c36b57884f2469f265676f1b667162bb.png)

此时我们点击右上方个人头像—>Plans&Billing，即可查看当前账户剩余额度：

![](img/6d61b03ea798dd19806befa0b202c252.png)

能够看到目前已成功领取5刀免费额度，至此便完成了账号升级，接下来可以尝试创建Claude 模型API- KEY并在本地环境进行调用了。

· Claude模型API-KEY创建方法

在当前页面，点击左侧API Keys，然后点击Create Key，即可创建一个新的API-Key：

![](img/d1e51d3937c5eb4790dcefcfbef13ee4.png)

输入新的API-Key名称（随便取名即可），点击Create Key：

![](img/ac9c89fdc36e56e92ff9deca8c76b1c4.png)

创建完成后会返回完整Key字符串列表，点击复制，并在安全位置保存即可：

![](img/411af1ac7bc5ddb22b027beba8f36d40.png)

至此就完成了API-Key创建。

### 4.Claude模型API的本地代码环境调用测试

· Step 1.设置代理环境

最后，我们需要在代码环境中简单验证当前API-Key是否能顺利调用。在开启全局代理模式的情况   下，确定当前代理的代理端口，然后打开Jupyter （或者其他Python编程IDE），先输入以下代码开启代 理环境：

| import os |
| --- |

代码中的 PROXY 需要换成自己的代理端口，例如我的代理端口是10080，则运行如下代码：

![](img/86bd90611bca4f7ae247a5ab43659fd8.png)

此外，也可以在cmd环境中启动Jupyter，并在启动Jupyter之前通过set命令设置Jupyter运行的默 认代理环境。

.  Step 2.anthropic库安装与导入

anthropic库是用于支持Claude模型调用的SDK，在Jupyter页面中可以按照如下方式进行安装：

| ! pip install anthropic |
| --- |

此外也可以在cmd中进行安装。

![](img/57eab78dfe1f414e9dbea8ee8c088e79.png)

安装完成后即可导入anthropic库：

| import anthropic |
| --- |

![](img/f552e0a55f51884acfe05bd74ebd4b4c.png)

· Claude 模型API本地调用

完成了一系列准备工作之后，接下来即可尝试在代码环境中调用Claude 3 opus模型。首先我 们需要先实例化一个Claude模型客户端（Client），并输入API-Key用于身份验证：

| client = anthropic.Anthropic( |
| --- |

上面的 YOUR_API_KEY 需要替换为自己的API-Key字符串。

然后尝试调用Claude 3 opus模型：

| message = client.messages.create( | , |
| --- | --- |
| &quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;你好。&quot; |  |

其中 claude-3-5-sonnet-20240620是Claude 3 Opus模型编号，从调用代码来看， Claude 3.5模 型调用整体风格和OpenAI GPT系列模型完全一样。若上述代码能返回打印结果，则说明调用成

功：

![](img/70fd29368d4a5db1b190554479549360.png)

至此我们就完成了Claude账户注册与API调用的全流程。

![](img/1c72e019b4c1dbae9476752115e8b00e.png)

📍更**多大模型技术内容学习**

**扫码添加助理****英英**，回复“大模型”，了解更多大模型技术详情哦👇

![](img/a10c956cfc4a0ea9aa3342741511f3fa.png)

此外，**扫码回复“入群”**，即可加入**大模型技术社群****：海量硬核独家技术****干货内容****+无门槛****技术交流****！**

![](img/1ba35db3e2e77c7e6a9a6d92db89ffdd.png)