
# 通过注册美国公司+水星银行申请Stripe的教程和赚到第一笔Stripe收入的经验分享

> 来源：[https://xuqiwei1986.feishu.cn/docx/FXLSdCyFPolniAxjBXRcwGO4n1b](https://xuqiwei1986.feishu.cn/docx/FXLSdCyFPolniAxjBXRcwGO4n1b)

大家好，我是博鱼。

做出海产品一定绕不过去支付这一环，常见的海外支付平台有：

+   Stripe：`https://stripe.com/`
+   paddle：`https://www.paddle.com/`
+   PayPal：`https://www.paypal.com/`
+   Creem：`https://www.creem.io/`

Stripe是很常用的一个平台，申请Stripe有多种渠道，我跑通的流程是：

1.  注册美国公司
1.  再用公司信息申请美国水星银行
1.  用公司信息和水星银行去申请Stripe

接下来分享整个申请Stripe流程的教程（篇幅70%），和我赚到第一笔Stripe收入的经历经验（篇幅30%）。

## 一、通过Stripe Atlas注册美国公司

有多种方式可以注册美国公司。

+   找万能的淘宝
+   使用firstbase：`https://www.firstbase.io/`
+   使用Stripe Atlas：`https://stripe.com/zh-us/atlas`

我是通过Stripe Atlas（以下简称Atlas）注册的，以下是详细教程，每一步都有截图示例。

### 1、费用

如果直接通过Atlas官网注册，费用是500美元`https://stripe.com/zh-us/atlas`

![](img/6bcdefb21b562bf0c3c9f39058c05d2b.png)

通过这个链接注册`https://stripe.com/zh-sg/atlas/stable`，费用是300美元，立省200美元。这是因为Stable和Atlas有合作，它俩在互相给对方导流，这个链接是Stable的专属链接，通过它去购买Atlas的注册公司服务有优惠。

> Stable是一家提供美国虚拟商业地址和邮件管理服务的平台，你注册公司的时候是需要提供一个公司地址的，Atlas在页面上会提示你可以用Stable的服务，当然你也可以不用。

![](img/93de95f0546f3cdc1887a7a264ef65ef.png)

其它费用：公司注册成功之后，从第二年起每年都会收100美元的管理费，详见`https://support.stripe.com/questions/managing-your-registered-agent-subscription`

### 2、开始注册

进入页面`https://stripe.com/zh-sg/atlas/stable`

点击“立即注册公司”

![](img/0b9b2673ffe94ef7524bb26b6bc7fbe0.png)

填写个人信息

![](img/80dccacd5c31823710de1056228b3c04.png)

验证邮箱

![](img/c5ca46200f60c3e3a31cd6bfc6ee87bf.png)

### 3、填写公司信息

接着会进入到填写公司信息的页面，点击“Let's go”按钮继续往下

![](img/b3e9f828da72cc0d314bfea084af60c7.png)

公司结构：选LLC（有限责任公司）

![](img/62398c875f2a3c621329fd814b375637.png)

给公司起个英文名字

![](img/23936e9abce835178a936b3664e8c39c.png)

填写公司描述和公司网站，网站我填写的是已经上线了的AI图片站

![](img/05d25572fbea87c9c77da03eddd6e6f4.png)

填写**真实的**个人信息，注意`First name`和`Last name`不要写反了

+   `First name`是名
+   `Last name`是姓
+   `Your home address`我填写是现在国内的实际住址

![](img/cda566b7673e046e765ee59e398fc8af.png)

查看所有权信息，点`Next`

![](img/3fc1173063e98df41a3a13dc83dbea6e.png)

选择谁来管理公司，选自己

![](img/cee27a4de0ac4f182c4869fc856ce41a.png)

公司税号Company Tax

![](img/64956a5e33f3eaee107b6a4690d8eb51.png)

> 翻译：Atlas 从美国国税局（`IRS`）获取您的雇主识别号（`EIN`），您需要用它来报税和雇佣员工。为了更快地获取公司的 `EIN`，请提供美国公司地址、美国电话号码和美国社会安全号码。

+   填写公司地址

+   方式1：可以买上面提到的[Stable](`https://www.usestable.com/partners/stripe-atlas-partner`)提供的商业地址。Stable和Atlas有合作，通过Atlas的专属链接去买有优惠：前三个月免费，后边一年每月有50%的折扣（原价每月49美元，优惠后每月24.5美元），一年后恢复原价。我使用的是Stable的地址

![](img/835b1b2d9eb132072613c3f0088b0fc3.png)

+   方式2：填写国内的住址应该也可以，页面上提供了“`Use my home address`”的按钮，据我了解，有人填写国内的注册也注册成功了。

+   电话号码：我填的美国电话号码

+   `SSN`美国社会安全号码：没有，不填

下一步确认公司信息，没问题就继续，有填错的去修改

![](img/28ee020e460c8e9c378dd8c6db796482.png)

最后去付款

![](img/8536487b9eaf61bec720f3f42cc40107.png)

### 4、激活Stripe

Atlas的速度还是很快的，一般1-2个工作日公司就能注册下来，成功后会给你发邮件，然后点击邮件中的“`Accept US card payments`”去激活Stripe，或者也可以从你的 Atlas 管理平台中激活，Atlas 会自动填充你在申请过程中已经填写的信息。

![](img/b6a090128738e4501311892bc32339dc.png)

如果你在公司注册成功后，好几天都没有激活，Stripe还会给你发邮件提醒。注意这个要及时激活的，不然后边你在登录网页端Stripe的时候可能就登录不上了。我当时就遇到了登录问题，联系客服也没解决，最后在`APP`上试了一下，竟然登录成功了，然后完成了激活的步骤。

![](img/4dc17f1f1489f0b6d52a5e153ae910c7.png)

![](img/40ca745e09be4715ca4bb8cdf52ac092.png)

> 到这里，其实已经可以使用Stripe去收款了，可以在你的网站中接入Stripe了。

> Atlas是Stripe官方出的一个代注册美国公司的服务，通过Atlas注册，Stripe允许你在还没有公司`EIN`号，也没有海外银行账户的情况下去收款，只是不能提现，有了`EIN`之后，才能用提现功能把Stripe余额转账到银行账户里。

> 官方文档解释：`Accepting payments before receiving your EIN` `https://support.stripe.com/questions/payments-and-banking-before-receiving-your-ein`

### 5、等待`EIN`号

接下来等待`EIN`号，需要几周的时间

> `EIN`号（`Employer Identification Number`，雇主识别号）是美国国税局（`IRS`）分配给企业或组织的一个九位数的税务识别号，用于税务申报和身份识别，非常重要。它通常以“`XX-XXXXXXX`”的格式显示，例如“`12-3456789`”。

在Atlas后台能看到申请公司的进度就只剩下`EIN`号了

![](img/7935d0d3fa310976e2985b8f8dee285f.png)

### 6、注册过程遇到问题怎么办

（1）英文不好的，可以在浏览器中安装一个“沉浸式翻译”插件，把页面信息翻译成中文，插件链接：`https://chromewebstore.google.com/detail/bpoadfkcbjbfhfodiogcnhhhpibjhbnh`

（2）注册过程中有任何不懂的地方，都可以截图问AI，我注册的时候基本上是每进行一步都要问问AI，防止因为理解错误而填错

（3）看看Atlas的文档`https://support.stripe.com/topics/atlas`

（4）对于自己无法解决的问题，可以联系客服邮箱`support@stripe.com`，清晰地描述你的问题，他们回复很快而且态度很好。

> 我注册的时候有一个小插曲，最后一步支付时提示我的信用卡被拒绝，尝试了多次都无效。于是我直接发邮件联系客服，经过几轮对话，他们直接把我的卡设置成允许支付了，最后我成功支付。

![](img/d314e263d04e5a52b53fea477238f346.png)

![](img/f0a48fb26b411b3e13ae8fb6add802d5.png)

（5）整个注册公司的过程，Atlas会给你发好多邮件，注意及时的经常的看看邮件

### 7、恭喜你获得一个出海省钱小妙招

做出海网站，肯定少不了要买一些海外产品用。在Atlas的特权界面`https://dashboard.stripe.com/atlas/perks`，它提供了很多海外产品的优惠，有几十种，通过Atlas去购买能省钱。

![](img/bf978735237e2ffdf232f322428fd1a9.png)

Stable也有类似的合作伙伴优惠：`https://usestableaddresses.notion.site/572353dfaad54a758bb780a428e8544b?v=74e63ef1e844495b9d2e49c7fd350651` 你看第一个就是通过Stable去买Atlas能省200美元

![](img/ecd0031a368f062d83cbfb69638ae017.png)

最近ElevenLabs也推出了自己的合作伙伴优惠`https://www.aiengineerpack.com/`，如果要用到里边的产品，能实打实的省钱。

![](img/834be11be0e8fa94c274f06061a7ed76.png)

另外，凡是页面上有能填写优惠码的地方，你都可以用AI的`DeepSearch`能力帮你找找有没有能用的优惠码。例如我之前购买`hostinger`的`vps`服务器，让`Grok`去`DeepSearch`帮我找一个优惠码，它真的找到了。

![](img/23eef938d0c9a6f2d4a10fde8f146fd4.png)

![](img/9097a2f38141bef96b1b56e0dd5fe358.png)

## 二、使用Stripe

![](img/dbf54abcf2cf5a053eabd357660343ea.png)

在使用Atlas注册完美国公司后，Stripe账号基本上就可以用了，已经帮你建好了一个商户，可以在代码中写逻辑去接入Stripe支付了。

如果还有信息需要补全，Stripe在`APP`端和网页端都会提醒你，如图所示，根据实际情况填写就可以。例如在你申请完水星银行账户之后，需要在Stripe后台补上银行卡的信息。

![](img/32ce70b42495a59b0ba4b37108ca3a83.png)

### 风险隔离：一个网站一个商户

在使用Stripe的过程中，**尽量一个商户对应一个网站**，这是为了做风险隔离。前面我们使用Atlas注册公司的时候，Stripe默认给我们提供了一个商户，商户名字就是你的公司名字（可以改），这个能直接用。

如果你现在做了一个**新网站要接入支付**，就需要新建商户，点击Stripe后台的`左上角的商户名`，有个下拉列表，然后点击创建-创建账户。

![](img/ec643c4111783570bac91f31a3fec8c7.png)

![](img/57f8fe71379899ed6731842521c0a002.png)

激活新商户

![](img/991b34a59b9e4e16a207da340a11a107.png)

接下来会填写一系列的信息，其中公司信息用的就是上面我们申请好的美国公司。

![](img/700223dc4a08a30a0884757002712fb6.png)

**中间有一个步骤是验证个人身份**，我们没有`SSN`，改为上传一个身份证件，使用护照

![](img/d107250cab9092099af71d5f2b1e2547.png)

添加银行账户，选择水星银行

![](img/a8c5a9856d6aa185e1136e9117654603.png)

审核通过之后，Stripe会发邮件说可以开始收款了。

![](img/4277f294dfc41356a9e12a506a98a513.png)

## 三、申请水星银行`Mercury Bank`

> 美国水星银行（`Mercury Bank`）是一家成立于2017年的数字银行公司，其定位是为初创企业、电商和互联网行业客户提供高效、便捷的金融服务，适合需要绑定支付平台（如`Stripe`、`Paypal`）进行全球收款的企业。支持全程在线开户，非美国居民也可以，但目前水星银行不支持个人账户开户，需通过注册美国公司申请。

等公司成功注册下来之后（Atlas会给你发邮件），就可以去申请水星银行了。

一般来讲，申请银行账户需要你有公司的`EIN`号，但水星银行和Atlas有合作，即使你没有拿到`EIN`号也可以先去申请水星银行的账户。

水星银行官网是`https://mercury.com/`，但我们不要直接在官网申请，而是通过Atlas提供的专属链接点进去申请，这样能把你的一些信息带过去。

先进入到Atlas的页面`https://dashboard.stripe.com/atlas`，找到“`Open a business bank account`”，点击Mercury后边的`Apply`按钮

![](img/e0595e7e0dbaaf85aafec7254589d458.png)

接下来会进入到创建账号的流程，填写一些基础问题

![](img/ef7f8fef71204b4efd9950477eb0adfa.png)

验证邮箱地址

![](img/532b5553473191ace18b2f68215c9085.png)

填写公司名称

![](img/133ec544c04a7159d53f0b1018f899f3.png)

填写Mercury callsign，我写的是公司名

> 这个“`Callsign`”是水星银行用来个性化你的账户的标识，主要用于生成你的专属转账邮箱地址、推荐链接等功能。

+   `Callsign` 是一个简短、独特的标识符，通常是你的公司名、品牌名或一个简洁的代号。

+   水星银行会用它来生成一个专属的转账邮箱地址，例如 `yourcallsign@mercury.com`，用于接收付款或处理其他事务。

+   它类似于一个用户名，必须是唯一的，不能与其他用户重复。

![](img/9c3cfdb808714fd6fde82734512a87fc.png)

问题：你的公司是否属于以下几种？我选的是`None of the Above`

![](img/98ee5defb659d21e0b07e7da56148d47.png)

问题：你对水星银行的哪些功能感兴趣？我选了前4个

![](img/4d150765bbfef36d6173d495c921ffc2.png)

你是否雇佣了以下人员？没有就选`None of the Above`

![](img/33faf62b41078f9c0edd5109258f8052.png)

是否有融资计划？没有就选“`No. I have no immediate plans to raise capital.`”

![](img/8aeb11b0aef50c858b5b9c7097d7bade.png)

接下来会跳转到一个新页面，完善公司信息

### 1、完善公司信息

+   公司名称`Legal business name`
+   注册地址`Country of incorporation`：美国
+   电话号码`Phone numbe`：我填的美国的电话号
+   公司网站`Company website`：没填
+   公司类型`Company type`：LLC
+   行业`Industry`：根据实际情况填写
+   公司描述`Company description`：根据实际情况填写
+   主要投资者`Major investors (optional)`：不填

![](img/341df3d617ec39a1b329fc3b63b6a97f.png)

### 2、公司地址`Company address`

一个是公司的`legal address`，一个是办公地址。

![](img/ca766a2eb0cec08d8645db0213c2d84e.png)

通过Stripe Atlas跳转过来的应该会自动填写好。

如果没有自动填充：

+   公司的`legal address`可以填Atlas提供的那个代理注册的地址，在这里找`https://dashboard.stripe.com/atlas/company`

![](img/c6b626076e7250a696698e6b3edf58db.png)

+   办公地址可以填写你在国内的实际住址，因为它允许填一个国际地址，用英文

![](img/878047ba0277f4fb0d0e1e50f21154d1.png)

### 3、个人信息验证`Ownership details`

+   个人电话
+   出生日期
+   家庭住址：现在你住的地方，按照实际情况填写，因为等会要上传地址证明
+   上传证件，传护照照片
