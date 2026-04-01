# Youtube运营基础之住宅ISP搭建保姆级教程

> 来源：[https://xuqiwei1986.feishu.cn/docx/TXsadzLkXog9QixeoGtccwKWnxe](https://xuqiwei1986.feishu.cn/docx/TXsadzLkXog9QixeoGtccwKWnxe)

## 第一步：注册野卡（虚拟消费卡）

**注意：野卡有一个开卡费，大概80元左右，这个钱相当于一个服务费是不算在余额里面的，所以大家可以**组团拼一个号，或者找已经开了的人帮忙买，给人家点幸苦费就行了，成本肯定比开卡低。

地址：https://yeka.ai/

点击【登录/注册】

![](img/4474ef7798ddc00f455662a969f9d1b4.png)

用自己的手机号注册就行了

![](img/ba7891dbdc1ffea33eef80bdf1eb057d.png)

选择左边的【野卡订阅服务】

![](img/ddcd37d428f88c03978629c929afaf6d.png)

开通会员

![](img/efea906d8f7cedcf28a3bb8c7776d8c4.png)

下一步

![](img/163fc50eba0f8a5d780fb08587f98a99.png)

选择【1年】，填写邀请码【YYVR0SV2】，然后点击【下一步】。（填写邀请码后会优惠一美元开卡费用）

![](img/fe2b1ca9b6b8984e8018bfa2ce9b213f.png)

一直下一步，直到【填写姓名】页面，按自己的情况填写就行了，然后【下一步】

![](img/09fc7902376abe50c41603b0a1b2fe43.png)

用支付宝完成实名认证，由于这里我已经认证过了，没法继续截图后面的步骤了。

![](img/170fa5217419f038c3dd0aaa95609701.png)

认证成功后，就是交手续费就行了。

## 第二步：Webshare购买住宅ISP

地址：https://www.webshare.io/?referral_code=pvgth5375260

我是直接用【google邮箱】登录的，点击【SIGN IN WITH GOOGLE】就行了。

![](img/fde2cce90e3de326f622db493eb68644.png)

这个网站一进来会送你10个体验IP，不过我感觉没啥用，下面是购买步骤

点击【Subscription】下面的【Browse Plans】

![](img/4e671a101944b4402bda82dbaa88fe7c.png)

代理的选择，我们选【Static Residential】静态住宅代理，然后下面选【Private】因为买一个的价格它和【Shared】是一样的，都是一美金，那肯定选更好的。

至于独享我觉得没必要，太贵了，而且待会有办法筛选IP，要是IP被污染了，我们换就行了。

![](img/b0ef2c0e903bab55fb2eade1230fc3bf.png)

购买IP数量的选择，我们要滑动到最下面，然后选【Custom】

![](img/5bce086a1e9325afbb5fa6df5f8b4f6c.png)

然后它会出现一个框，我就买1个所以我填【1】，大家想买几个就填几，然后点【Save】就好啦

![](img/846a5aad92a4a10f867253b9aa8e86d4.png)

流量我选的是一个月【250G】具体看你自己情况，我做的是音乐号，一个视频5,6G感觉是有点不够用了。

![](img/bc3e6b76f66db36d634b60d04a13e932.png)

这里是选你要哪个地区的IP，我用的是美国，所以选【US】，然后输入1(我买的IP数量)

![](img/100c8ad4f160a5e4648b6986ae238304.png)

选好后点击右边的【Continue】

![](img/fafea8588e58bc641796431cc762da33.png)

这个框不必理会，直接【Dontinue】

![](img/e900fb0b0ddd84dc3c44e48e1a3312f8.png)

付款页面，需要绑定虚拟卡

![](img/8b2de398007a4b3f091d6ade4d5ae5fb.png)

到我们刚才弄好的野卡里面，选择【订阅服务】->【Webshare】->【充值】

![](img/78a234447ed09980c617f26674894ca5.png)

把野卡中的信息，填到对应的框里面，然后点击【Confirm Subscription】就行了。

**注意：你的野卡里面要有钱，下面这个位置不是0，要是你没钱**然后去买，这种情况发生2-3次它会冻结。

![](img/85266597887c5e96aa67f6a810a0116f.png)

![](img/bcd944114e6ed46eaf1d9b15a14f1a94.png)

这里我已经买了，就不买了。

## 第三步：代理配置

买好了之后，到【Proxy List】中就可以查看到你买的IP了。

![](img/3a19d6883ea30cbd835960e9c292de87.png)

这里我就用它送的10个IP来演示如何配置。

由于我们买的不是完全独享的IP，所以我建议每次使用前，先做个测试，没问题，我们再用，有问题我们就换

地址：https://ipjiance.com/

直接把我们买的IP，复制过来检测。

![](img/f641d87c72114adce1f6f3cc6a39e41a.png)

它会给你出一个报告，由于这里是我送的IP，所以是合格，正常情况下你买的IP应该是【优良】，如果不是就参考【如何更换IP】里面的内容，换IP，直到检查的是优良为止。

![](img/3b3ef1e6cc0f87ede45739b610616a6e.png)

IP检测没问题后，打开【比特浏览器】，点击【创建窗口】

![](img/ee24a7876caa7190ab1d0eb02905a886.png)

滚动到代理配置这里，按照图中的提示配置就行了。

![](img/b708dd98666e77adae32b4317be56526.png)

配置好后，重点来了。

你的电脑也得开魔法，不然就算这里配置好了，你的窗口也打不开Youtube。

这里我就不放图了，文字说明。

你的魔法软件要打开TUN模式，系统代理，全局模式。

然后就可以打开刚才创建的窗口了。

## 问题解答

### 如何更换IP

选择你要更换的IP，然后点【Replace】

![](img/0fc5adf91b806362e081dffa302d7528.png)

![](img/9bff504418ade08046c827235c1986a5.png)

![](img/cbeb9b4b7d8ae46ac2db5c6769e4a471.png)