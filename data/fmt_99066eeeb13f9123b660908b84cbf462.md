
# 202411｜解决问题的方式之一：拖延一下

> 来源：[https://leonzhang.feishu.cn/docx/TMw7dvSlWoHTQ0x0ja2ceXWinSd](https://leonzhang.feishu.cn/docx/TMw7dvSlWoHTQ0x0ja2ceXWinSd)

![](img/ed4014cf94933698a24b9ec0cb431781.png)

> 做正确的事和获得好的结果，有时候并不是同一路径。

## 最近经历

### 生活

**1**

Melisa：不要跳出自己的舒适圈，而是扩大自己的舒适圈。不要修改你的缺点，而是去放大你的优点。

金妮：人的缺点和优点，它就是相辅相成的，就是缺点只是优点的立面而已。人的精力也是有那种季节性律动的，大自然那种起伏的那种感觉，所以就是人累的话不妨去休息一下。

**2**

当我看见花开的时候，花就在枯萎了。

### 工作

**1**

复制 Web 文字的小 Tips：

通过浏览器开发者工具（`F12`）复制页面文字 `Console` 的 `copy(document.body.innerText)` 通过开发者工具复制整个页面的所有文本内容。

通过 `document.body.innerText` 获取页面全部文字

1. 按下 `F12`，打开浏览器的开发者工具。
1. 切换到 `Console`（控制台）标签。
1. 在控制台输入以下代码并按回车：`copy(document.body.innerText)` 这个命令会将页面上所有的纯文本内容复制到剪贴板。
1. 打开任意文本编辑器，粘贴内容。

**2**

听到一句很当头一棒的话，「公司的员工不开心，做出的产品能有多么的快乐？」

想到每次硬件产品这边被我拒掉的需求，会重新出现在我 +1 这里，项目负责人甚至更高 Level 的人来施压。嗯，有些卑鄙。他们没有意识到一个最基础的事实，功能是做不完的，产品是永远无法达到完美状态的。因为想法只会越做越多，越多越显得心里没有底气，然后不断用堆功能的方式来掩盖内心的不安，进而负向循环。

**3**

之前在公司做某硬件项目时，涉及 Wi-Fi & BLE 模块选型。因为有很多 IoT 设备为了降低产品整体 BOM 成本，会选用更常见的仅支持 2.4Ghz 而不支持 5Ghz 频段的模块，比如很多米家的 IoT 设备，即便在 2024 年，还是会有很多这样的产品。

> 我个人曾购买过很多米家生态的电子产品，小到插线板、充电宝，大到路由器、小爱音箱、蓝牙温湿度计、空调伴侣、踢脚线电暖器 1S、体脂秤、甚至电饭煲等等。列这么多数量的米家产品，是想佐证自己对 IoT 设备的使用体验有一定的话语权。

正因为我之前多次遇到购买仅支持 2.4Ghz 的 IoT 设备后，无法连接 Wi-Fi 进而产生退货的情况，甚至竞品中也有很多因为无法连接 Wi-Fi 而导致产品退货。因此，我多次向硬件产品同学反馈该选型风险，甚至还在群里、线下都有和项目负责人提过，建议谨慎考虑该模块，无果。

不得已，只能在后续的软件开发过程中，协同开发及软件测试同学进行多次测试，并委托硬件测试同学寻找屏蔽房进行多设备测试，以确保设备连接 Wi-Fi 的稳定性，力求最大限度降低设备在 Wi-Fi 连接上的潜在问题和风险。结果产品上市后还是出现了很多因为 Wi-Fi 连接不上，进而导致退货的事件发生。退货理由很简单，“软件难用，连接不上 Wi-Fi”，甚至在项目前期时，内部某高层 Leader 也认为是软件问题。如此急匆匆投向市场的行为，最后结果注定反噬。

## 最近观察和思考

![](img/ead8a4d8d3f99905122ff3e8e7a0514b.png)

小米穿戴手势控制中的「响指操作」这个真是太聪明了，每次看到都会觉得妙～ ❤️

用声音来弥补算法的不足（即便是 Apple Watch 也“时灵时不灵”），大幅度动作 + 响指声音，立马就能捕捉到用户意图， 真是棒呀！！

## 最近在听、在看

![](img/06f61f4c97ebeee193ea1bb386c1fbb2.png)

### 播客

**1**

> 三五环｜No.171 撕不开的 AI 入口，从 AI 耳机聊起

生态很重要，拥有自己的硬件入口很重要。对于国内安卓手机厂商来说，是个很好的机会，对他们而言是现成的 AI 入口，比如最近的 OPPO 被戏称为“OPhone”，这个就是“窗口期”，要不是 ChatGPT 无法进入国内，Apple 也不可能选择百度这个烂泥一起合作。

先不说我身边朋友一听说这个消息，以后买设备要买港版或者日版，单从实现上，百度肯定是没办法实现 Apple 想要的技术能力或者效果，不然也不至于拖这么久还几乎没有任何消息。

### 视频

**1**

> 1124 酱｜四年了，种子开花了_哔哩哔哩_bilibili

**感谢没有中途放弃的自己，才没有那句，“要是坚持到终点就好了”。不到终点那一刻，永远不知道会发生什么，所以，请先竭尽全力坚持到终点。真心为她开心。**

**2**

> 1124 酱｜连续跑步五年，身体会发生什么变化？_哔哩哔哩_bilibili

24 酱在视频中说，自身的感受是随时在线的体检机，也是她觉得自己健康的底气，而且体检是她每年最不怕的活动。

我个人也是因为 24 酱的视频，促使我开始运动，从早上骑车到早上跑步，甚至还在天气转冷后报名了一个月的健身房，因为我实在没机会像她一样在浙大的校园或者围着西湖跑步。还有一条好消息是，只要坚持锻炼是一定有效果的，Apple 健康还在最后一周提醒我，平均体重、体脂、BMI，包括平均心率都有明显下降。

1. **强化练习还是比较明显的**。最直观的感受就是，跑步后的体重下降涨幅不明显，而且前一天吃一些东西后，第二天还是会恢复到原来的体重，我喜欢这种感觉。

1. **今年的跑步基本结束，年后继续**。因为今年气温变低了，为了避免生病，接下来就不去跑步了，预计年后变暖后继续。加油，朝着 75kg 继续！！不敢想象自己把这么多脂肪减下去，状态会变得多好，哈哈！！

1. 再次感谢 24 酱，你的视频激发了我。

**3**

> 老梁：疯狂内卷 天意不可违：`https://www.youtube.com/watch?v=cw7i0ws27Fk`

有句话叫，「火烧眉毛，且顾眼下。」 人在多数情况下，是考虑「眼前利益大于长远利益」。当一个人做出反常规的事情，一定是必有原因的。

全世界任何一个地方的政府，都是用低成本方式管理社会，而且是「有效管理」。人群越集中，相对越容易管理，比如美团外卖与骑手、出租车公司与司机等等，一定程度上的“维稳”与“效率”兼具。

1. 「成本降低」，市场一定，随着人员规模的扩大，则收入降低。

1. 「利益交换」，因为下面管理者承担了管理责任，所以让下面的管理者获得相对更高的收入。当然也会警惕无序扩张，所谓「一张一弛，文武之道」。

1. 「矛盾转移」，本来是「制度设计不合理、以及劳资与平台之间的矛盾」，将劳资矛盾转化为了「人民内部矛盾」。这是一种非常高明的「驯化手段/管理手段」，历史上如《商鞅变法》的“连坐制”。

如何面对内卷？顺势而为，无为而治。躺平就好！

**4**

> [余华谈盗版：穷人也想读书！我鼓励我的书就应该被盗版…_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1PWDdYYECK/)

人们的奋斗为了什么？“就是为了躺平，我现在奋斗的最终目标就是为了躺平。”就是为了过上一种不被闹钟吵醒的生活。人生奋斗的目标很简单，就希望生活里没有闹钟，所以先奋斗，后躺平。

**5**

> [如果你觉得生活苦，请看看王立群老师的人生经历_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1NhyeYdEd9/?share_source=copy_web&vd_source=672ec070780ce29c890256a5fabe799b)

《孟子》倡导的强势人格的关键点就是「自强不息」。经典往往不是你读懂的，经典是你人生经历与经典相碰撞、共鸣后的感悟。

### Live 现场

<https://leonzhang.feishu.cn/sync/Iv7ZdMGaBsyuHLbBRRDcqS2InBc>

1. [2024.05.25 林俊杰 胡彦斌《黑夜问白天》（官摄）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV131421q7Xv?spm_id_from=333.1387.favlist.content.click)
1. [【4K60 帧】林俊杰&黄贯中《海阔天空》官摄 经典永存_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1HGCmYpEqz?spm_id_from=333.1387.favlist.content.click)
1. [【4K60 帧】林俊杰生涯神曲首唱《愿与愁》官摄_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1vZ1YYGEZp?spm_id_from=333.1387.favlist.content.click)
1. 林忆莲&林俊杰 - 江南（ live）_哔哩哔哩_bilibili
1. [【4K 60FPS 修复】林俊杰《我们的爱》LIVE JJ20 演唱会东京站-超清官摄_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1UomjYREUV?spm_id_from=333.1387.favlist.content.click)
1. 淦！邓紫棋唱《曹操》也太好听了吧！_哔哩哔哩_bilibili
1. [超爱！黄霄雲北京演唱会翻唱林俊杰歌曲合集《那些你很冒险的梦 》《手心的蔷薇 》《不潮不用花钱》_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1NNDVY1EVs?spm_id_from=333.1387.favlist.content.click)
1. [油管 7 千万播放的暗恋神曲《Crush》，说不出口的悸动，恋爱的感觉！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1M5411p7PB?spm_id_from=333.1387.favlist.content.click)
1. [[HiRes/4K]这画质音质官摄馋哭了，张靓颖王力宏《另一个天堂》，4192 天的等待，900mm 长焦录制_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1QZ421M72v?spm_id_from=333.1387.favlist.content.click)
1. [【🧣之夜】飞云之下 - 单依纯丨容祖兒丨周笔畅丨欧阳娜娜_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Z84y137cA?spm_id_from=333.1387.favlist.content.click)
1. [【超高清】信乐团-《死了都要爱》 神级现场 LIVE_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1wJ411H7x9?spm_id_from=333.1387.favlist.content.click)
1. [杨宗纬《其实都没有》2024/01/05。辛选演唱会现场。官方 8M 高清无损源码_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV13i4y1z76V?spm_id_from=333.1387.favlist.content.click)
1. 杨宗纬《我想要》心碎到了极致的嗓音！_哔哩哔哩_bilibili
1. [「歌手 2024」第四期 纯净版 孙楠《拯救》_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1J1421C75g?spm_id_from=333.1387.favlist.content.click)
1. [【时光音乐会】《愿与愁》 | 尚雯婕“你我终会沦为尘埃漂流”_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1JYDdYxE4h?spm_id_from=333.1387.favlist.content.click)
1. [【4K 顶级现场】李健《贝加尔湖畔》神级现场！一开口便无法超越，此曲只应天上有！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1u44y1Q7LE?spm_id_from=333.1387.favlist.content.click)
1. 吉他弹唱，假如爱有天意_哔哩哔哩_bilibili
1. 《她说》_哔哩哔哩_bilibili
1. 一路向北_哔哩哔哩_bilibili

## 相关阅读

1. [AI 和 AR 是一件事？从 Google Glass 到 Rokid Glasses](https://leonzhang.feishu.cn/wiki/QrKswAhYMiPuI5khoq2c4WGEnSd?from=from_copylink)｜https://mp.weixin.qq.com/s/CqYaOhluSKFtZWrrtQupkQ
1. [Notes｜应用来了 - 百度世界大会 2024 ](https://leonzhang.feishu.cn/wiki/YycHwKlOdi7ttRkG1LBcw8zQnqb?from=from_copylink)

以上，

祝你早安、午安、晚安～
