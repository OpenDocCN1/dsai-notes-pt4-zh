# AI编程蓝皮书2.0

> 来源：[https://kcn9ut4m7icc.feishu.cn/docx/SfQadlBHRogYNBxXJiEcqIDEnbg](https://kcn9ut4m7icc.feishu.cn/docx/SfQadlBHRogYNBxXJiEcqIDEnbg)

大家好，我是AI产品黄叔，目前给两家大厂做AI产品顾问，在使用Cursor和Windsurf（这两个都是AI编程的软件）开发产品后，意识到这才是创造者的**天堂，最近举办了多场线****下AI****编程培训，**根据学员的反馈有了这份手册，我会在本手册里持续更新，不断把更多的技巧，思考分享出来，希望能够帮助想要创造的你走进这个天堂！如果你觉得有帮助，欢迎分享给你的朋友。

> 特别感谢 **@Orange.ai** 和 **@歸藏** 两位老师对AI编程蓝皮书的支持与分享。

> 你们的助力让更多渴望创造的朋友能够接触到AI编程的魅力。

> （如果更多的朋友也愿意推广/参与优化，欢迎拉到文末添加我）

> **@唯庸** 和**@杜昭 **也有不少贡献，一并感谢！

**12月28日更新：**

黄叔和风**变**科技合作办了个AI编程社团，持续运营三个多月，现在已经很成熟了，每周五直播，双月办黑客松，每周邀请系列嘉宾做分享（AI编程技巧+如何搞钱）持续了6周，可开发票，课程内容也更加体系化和全面（包括数据库、域名、支付接入、最新模型能力和接入、出海产品等等），最近大量更新Claude Agent Skills相关玩法，能帮助大家把各类场景用AI完成SOP自动化操作。欢迎大家感兴趣的话加入，已经有1000多位同路人：

**（当前福利：赠送1月 Youmind Pro会**员+ 1月 YouWare Pro会员）

![](img/9cff940d16bf158c015ab87bf3da3556.png)

购买链接：https://prod.pandateacher.com/web-kylin/landing/63#/main/40296

> [相关Rules和Prompt](https://superhuang.feishu.cn/wiki/W1LCwYA8eiTl77kpi81c31yNnJd?from=from_copylink)**意：本文内的Ru**les/Prompt，需要复制的话，点击文档： ，里面可以复制。

# 全文知识目录

（如有跳转错误欢迎评论提醒黄叔）

|  | 技巧一 | 技巧二 | 技巧三 |
| --- | --- | --- | --- |
| ## [**Part1：基础知识**](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-XOs2d7d8ooYq5GxBa4KcHdwBnb8) |  |  |  |
| ##### 06 必备的Windsurf技巧 | Set Global AI Rules | [安装中文插件](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-DoRsdvelno3bSBxOIw8cF63NnNh) |  |
| ##### [07 一些AI编程的心得](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-ZbG6dRYpjoJtYNxtBmocal2in3c) | [写的越慢，写的越快](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-NwH0djbp1oq2mixGTMPcLGpRnNc) |  |  |
| ## [**Part2：你的个人工作台**](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-VAqydLZzToSDwqx6iywc7Tmpnxf) |  |  |  |
| ##### [01 使用AI IDE进行创作](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-Ldi4dd9LOo8bh7xvw4zcFzLxnjh) | [改写提示词](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-ISdjdT2AFouADSxLpJhcy13Anpc) |  |  |
| ##### [02 使用提示词生成精美图片](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-RFJZdrzppoStBFx4U9wcwZzAnyh) | [图片生成提示词](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-H7RAd7eMWod5BuxWGNicpLNRnoc) |  |  |
| ##### 03 10分钟搞定高德地图MCP！ | 给Windsurf安装高德MCP |  |  |
| ## [Part3：网页开发](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-XUCYdjPVYopS8uxB1RRcaWijn0c) |  |  |  |
| ##### [01 图片字幕生成器](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-RXnEdeqRqoSAhtxwZHYcDP3rnge) | [用多模态复刻产品](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-RtVAdyURtomNqhx1bescQkuRnhd) |  |  |
| ##### 02 注册硅基流动，赠4000万token |  |  |  |
| ##### [03 帮📕的老外起中文名](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-LUejdBgjqocWvfxODZEcsz0Inef) | WorkSpace AI Rules | [接入大模型](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-WBhVdpF6NoBiRvxNVGMcCnRQnto) |  |
| ##### [04 做一档你自己的AI播客](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-R6ZMd0VgYoxf88xJWzacYpgpnrB) | [语音复刻](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-SXWUdkTLpogdE7xCRGHcibg5n4c) |  |  |
| ## [Part4：多维表格](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-WemDdHs1QoPAhMxJa55cB6mWnIg) |  |  |  |
| ##### [01 做一个你专属的好文推荐网站（D](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-PCk2dMhkiocY4Px12JSclzzQnPe)eepSeek R1 + 飞书多维表格） | [创建带AI能力的飞书多维表格](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-IkgVd9yZEo5yOTxRzIVcYRfTnxf) | [用网页呈现多维表格里的内容](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-P3qVdsXryoEyS6x9lIgcMIBGnVb) |  |
| ##### 02 一键批量提取抖音博主视频：文案提取+风格分析+文案改写 | 配置扣子API发布成多维表格字段捷径 |  |  |
| ## [Part5：浏览器插件](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-TotjdKSPnoO377xlivCcA9q8nVE) |  |  |  |
| ##### 01 DeepSeek驱动的网页金句卡片生成 | [先Chat，再开发](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-QTWcd1pAmoF1gYxeEP1cNQWAniE) | [插件安装指南](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-FPxWdNBZno6qFWxN6k4cXv1Unyd) | [Chrome插件logo生成](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-PIVSdORqSoyVjLxAa7ic68Dhn5b) |
| ##### [02 复刻善思flomo](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-FkIzdtR1joldxPxu0Qbcwjesn2g) | [接入flomo API](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-P9CEdQcCxoiYLsxNIXvcoupxnIh) | [更新readme](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-A3TPdNKkHoJufOxn6M5csUpmn1g) | [使用Git存档](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-GhCndzNT9obE0OxyiHecYSmunRg) |
| ## [Part6：🛰️小程序](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-OUN1dy2aioDaM1xnh8Ycbi5KnLf) |  |  |  |
| ##### [01 做一个简单的🛰️小程序](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-InP4dGETKozVd2xDbiWcXNIhnSg) | [开发小程序准备工作](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-VDoldzuIhoKAvPxhl99cGHTHnab) | [小程序的认证和备案](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-MiyMdnKfIooDDFx8xE0cKQgLnGe) |  |
| ##### [02 书单星球（🛰️小程序）](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-PAhidGtOMoOFxTxGNOxcc1QSnQH) | [🛰️小程序开发路径](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-U726dE9itoQ6fhx2DdPcZGlgngb) |  |  |
| ## [Part7：基于扣子搭建产品](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-QDtYdGsoZol0YoxhKdjceGCMnbg) |  |  |  |
| ##### [01 扣子Coze产品介绍](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-JouSdqEgOoLibyxQ4YZcH69Dngf) |  |  |  |
| ##### 02 用DeepSeek R1，抖音视频文案提取+改写 | [理解Function Call](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-Pd2MdkC47oh6URxpaMucqN2Vnvd) | [简单智能体搭建](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-GDJZdkzD9obcUwxbqGHc0J4CnCc) |  |
| ##### [03 柴犬表情包生成器实战](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-Gf9ydo7tSoddsBx71Zbcz9qhn7e) | [Coze的协同机制](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-AvrIdEfhcoPvd6xBkAxcUjcDnLe) | [完整智能流搭建流程](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-T2lVd899aoUx44xBtGicmkv6nXe) | [前端调通Coze API](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-D1vGdWGHXoYXHcxmm8Scx80fnLf) |
| ## [Part8：部署](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-LLiMdHPz6orJhYxJWRHcxQGPned) |  |  |  |
| ##### 01 将网页产品部署到Cloudflare！ | [部署Cloudflare原理](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-LOrAdRJGvodWGRxhnFOciKKfnNf) | [完整部署流程](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-SfJddr1xvoSlkLxa8JJcvWj4nkh) |  |
| ##### 02 使用Github+Vercel免费部署 | [两种推送Github办法](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-AmqGdjnN1oY6tWxobXUcTsZqnMg) | [把密匙配置为环境变量](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-JCGYdiBGroSKG9xMj74c4b81nkh) |  |
| ##### [03 白嫖Chrome插件部署](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-GIusdv9sro5PlXxQ5QSc3jaZnsc) | [将原网页改造为插件](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-NATUdinRUods4txh0Wick8UVnxf) | [上传Chrome应用商店](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-Oz9ld0a8TooTAsx4wg0cl6abnBd) |  |
| ##### 04 最简易的免费发布：Yourware |  |  |  |
| ## 更多 |  |  |  |
| ##### [降噪AI](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-Ik6JdmurGofKyDxWLGFcG7rbnEc) | [复杂产品拆解思路](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-Ik6JdmurGofKyDxWLGFcG7rbnEc) |  |  |
| ##### [做一款App](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-SeM1dF2CUoy7mHxP8u3c3tKUnVc) | 待更新 |  |  |
| ##### 一个提示词完成一款App的设计（Claude4） |  |  |  |

好了，开始正文，先说说我用Windsurf/扣子开发的几款产品，方便你了解0代码基础可以做出什么样的产品，相信你的想象和执行力，会做出比它们更好的产品：

# Part1：基础知识

## 01 Windsurf做的产品

![](img/caca2122faa6fb492e93bb1ff6450085.png)

![](img/b02f7fdb8b3be52903305d27c6b8d654.png)

![](img/49101e2a0d880f1055b3110419c8ca78.png)

![](img/8392fd0d175292297f84d03b5629b2a0.png)

![](img/9f9a3f030bc92ac312939dee1dafd46c.png)

从左到右，帮老外起中国名字（网页），善思flomo（浏览器插件），书单星球（🛰️小程序），降噪AI（H5+🛰️服务号），柴犬表情包生成器改良版（网页+Coze）

帮老外起中国名字，近期📕被外国佬冲爆，很多老外有类似的需求，用半小时开发出来行不行？完全可以！

善思flomo，是一款在Chrome浏览器内，帮助你快速对网页内容做摘要和写思考的插件，可以自动抓取标题和链接，可以AI总结全文摘要，输入你自己的个人感想，然后一键同步到flomo（一款笔记应用）。

书单星球，是一款可以查看各个名人推荐图书的🛰️小程序，界面非常美观，能够阅读到名人如何评价一本书，帮助我们更好的选书。

降噪AI，稍微复杂一些，简单说就是从指定的👸号里，每天自动抓取/筛选/摘要/呈现，同时还有AI语音播报/实时语音互动的能力。

柴犬表情包生成器改良版（网页+Coze），生成各种表情的柴犬表情包。

简单介绍到这里，后面我们会用这些产品作为案例，来给大家串讲如何用Windsurf和Coze来搭建产品。

但在开始之前，我想先说说自己的思考，即，为何你要学习做产品，做产品是不是下一个时代的机遇。

## 02 AI编程构成了双杠杆的重要一环

纳瓦尔相信大家都有听过，他是硅谷知名天使投资人，投资过推特/优步等知名公司，《纳瓦尔宝典》一书搜集了他过去十年在互联网上发表的思考，豆瓣评分8.4：

![](img/7a3b105c44529224cf27187935c98b29.png)

第一章开头就说出让人震惊的事实：

> 赚钱跟工作的努力程度没什么必然联系。即使每周在餐厅拼命工作80个小时，也不可能发财。

是不是很残酷，仔细一思考又是真相，那如何能致富呢？他提出了四个杠杆，咱们这一节要说的是其中两个杠杆，原文是这样写的：

> 代码和媒体是不需要许可就能使用的杠杆。这两个杠杆是新富阶层背后的杠杆。你可以创建软件和媒体，让它们在你睡觉时为你工作。 如果不会写代码，那就出书、写博客、做视频、录播客。

读懂请掌声👏！

在抖音/B站/📕/视频号，甚至有些老旧的👸号/头条号等媒体浪潮里，造就了大量的财富，黄叔算是非常小小小的吃到了一些红利，每一年都会有人问：IP还能做吗？相信我，答案永远是：机会大量存在的。

这是媒体杠杆。

另一个，做产品一直是需要设计师/前端/后端的配合，要做一个App最低配置也得好几个人，一年光人工成本就百万，似乎我们不会代码就做不了产品了？

很有意思的就在这里，黄叔完全不会写代码，依然通过Windsurf/Cursor做出了产品，这就是新时代的魅力和新机会。

ok，媒体杠杆和代码杠杆成为双杠杆同时出现，除了纳瓦尔说的这两个是不需要许可，每一个人都可以用的杠杆之外，更重要的是，小猫补光灯的爆火，让我突然发现了两个杠杆的相辅相成：

![](img/68f06a068e41e438cf47adc088487eee.png)

花生在小猫补光灯爆火之前，就已经通过内容突破了年入百万，这里面比如Youtube 3万粉丝账号的内容更新，可以获得数万的流量分成收入。还有企业咨询和定制化开发，甚至报高价都“赶不走”客户。

在小猫补光灯爆火之后，更是吃到了红利，各种邀请分享，公域流量得到了进一步的积累。

光看上面这两段，似乎这是通过做产品来积累公域流量，再用流量变现的单方面增长回路，其实不是的，公域流量的积累，本身就能让下一次产品获得更多的冷启动，提高产品成功的可能性。

更核心的，是这里面的Build in Public，能极大的增强产品成功的概率：

![](img/57852f25d3b275ca1228d2b36bf99d36.png)

通过获得用户的反馈，能帮助我们更好的理解用户的需求，捕捉用户的痛点，以及不断的发布产品，让自己更加了解如何宣传产品。

以上这些，可是坐在办公室里吹空调那些所谓的大厂产品经理很难习得的：因为，他们不愿意/没精力把手弄脏。

总结来说，双杠杆是产品即内容，内容反过来帮助产品的一个增长回路：

> 你不要指望自己是乔布斯，但每一次失败都会增加下一次成功的概率！

很多人会问，IP现在好做么？当然不好做。正如失败也不容易接受一样，所有的成功都需要耐心，都要积累。

好吧，这依然不容易，不要紧，抛开这些，AI编程至少可以搞定你自己的问题，比如，善思flomo至今在Chrome应用商店只有很少的用户，但，我就是最重度的用户，解决了我很多小麻烦，已经很棒啦不是么！

![](img/8000de142d41b555fe080f23f4e46384.png)

我会开始讲如何用Windsurf快速入门AI编程，是的，别怕，0代码基础的你只需要动嘴，就能做出产品。然后，我会单独再说如何利用飞书多维表格作为你的AI数据库，再说用Coze搭建产品，再给出基于Coze的API+AI编程，做出一个完整产品的全流程。

至于为何不全部用Windsurf，下面我会先说说自己的思考。

## 03 不管黑猫白猫，做出产品就是好猫

这句话的意思是，不管是学代码写产品/AI编程/用扣子做Agent，都是手段，我们的目的是做出产品。

当然，我们知道很多人会说，现在AI编程只能做简单产品，这里存在对AI理解不多的情况，不过在代码量大幅增加后，也确实容易出现AI把不应该改的地方瞎改的情况。

背后的原因有很多，其中一个重要的点是超出了大模型的上下文长度，怎么理解这个上下文长度呢？简单的类比是，它的脑子就那么大，一次超出它脑子的容量，就记不住，只能瞎说。

所以有一种有效的策略是，Windsurf+Coze，Coze后面我们会仔细说，简单的讲就是一个个节点拼起来的工作流，每个节点干一点小事，拼起来就可以是一个复杂的事。

![](img/5fa9ea83c5707e5ea8a8449b565d4593.png)

比如上面这个节点，从左到右，每个节点大致做了这么些事：

获得输入---读取网页内容---对内容进行打分---筛选：分低的就不要了---摘要---把摘要/关键词/评分等拼到一起---存到飞书多维表格内-结束

它帮我们实现了（简单说就是从指定的👸号里，每天自动抓取/筛选/摘要/呈现）这里面筛选和摘要的部分，处理完成后，把处理好的数据存储到飞书多维表格内，最后我再用AI编程，开发一个网页，这个网页的工作就很简单了，只是把昨天的内容抓取出来呈现就完成了。

理解了没？相当于把很多处理工作交给了Coze，它处理完成后，我再把处理好的数据让Windsurf完成最后的呈现即可（即大幅降低了AI编程所需要解决问题的复杂度）。

所以不管黑猫白猫，它们一起能帮我做出产品就行，我不管是什么猫。

当然，降噪AI前后还有一些额外的工具和工作流在处理，我们这里只是为了简化给大家描述背后的原理。那下面就开始，具体讲讲AI编程（Windsurf）和Coze的上手内容，最后会用一个案例来串联。

## 04 Windsurf，不是Cursor？

![](img/4998ff650e53fdc5c3ad54160bc4d050.png)

这是一个我的产品用户交流后给的反馈，有5年编程经验的朋友，都倾向于Windsurf，而非Cursor，很有意思。

黄叔对于两款产品都有使用，就我的理解：

+   Cursor控制力更强，可以更加精确的控制代码和文件

+   Windsurf更加偏自动化，能省你很多事（反面就是复杂代码下会替你把对的改错哈哈哈）

对于0代码基础的小白来说，后者更好入门，等你变成熟手，开始做相对复杂产品后，可以考虑转到Cursor。

这不绝对，两个产品都在非常疯狂的进化，以及Claude也可能很快引来下一次的升级，所以我们先按照这个方式去理解。

当前节点上，直接用Windsurf上手最方便。（Cursor也在持续进化，本教程以Windsurf为案例进行教学，整体两个软件编程起来差不太多）

## 05 Trae可免费使用Claude 3.7

字节推出了一款AI编程工具，叫做Trae，限时免费无限量使用Claude，欢迎大家尝试下载使用。

下载链接：https://sourl.co/2DCmmW

黄叔也专门写了篇文章介绍，内附共学群：https://mp.weixin.qq.com/s/Cl9siWXt4VfMBtK5wdbDYg

OK，进入主题，下面我们来说Windsurf的一些基本技巧，有了这些技巧后，使用它来AI编程就变得更加丝滑。

## 06 必备的Windsurf技巧

好的，上面的章节是和大家说说AI IDE（Windsurf/Cursor/Trae都是）可以做更多的事情，后面我们还会持续更新，下面回归到主线，如何用Windsurf来开发自己的idea：

### 第一，开始的开始，是先下载软件

哈哈，这个就不用废话了，123，上链接：https://codeium.com/windsurf

注意🪜

下载后安装好，打开软件，大概是这样的：

![](img/4b03ce221743cba50f0d7aaf56ff58b4.png)

右上角和右中的位置是Login with Codeium，这个是登录入口，我知道你想点击它，但别急，因为要调用Claude Sonnet这个地表最强的编程大模型，所以最好是直接付费使用Pro账号，某宝可以自己搜索一下。

注意：Windsurf的Pro账号，即会员，是自带Claude Sonnet使用权益的，必须买会员，这样才能使用到Claude，其他模型目前用来编程都还差点意思。

特别强调：建议大家直接用最好的模型来生成代码，这样可以大幅减少对时间的消耗！钱，买不来时间！并且又不贵对吧。

![](img/18bac8a9a00054f1c8fe10fed605dbb8.png)

如果你是马上就要开始肝，推荐购买15刀pro共享月卡，这是3个人一起消耗用量的账号（用量指的是最多只能对话多少轮类似的，用完就不能用了，还必须是一个月内用完）

如果你是细嚼慢咽的类型，想自己慢慢开始玩，那推荐购买15刀独享账号（月卡），这个账号就只有你一个人用，不用担心被别人薅完。

购买完成后，会获得账号和密码，在Windsurf软件的右上角，点击Log in with Codeium，进行登录。

### 第二，在你的电脑上，新建个文件夹，并用Windsurf打开

这个你总会吧，那为啥要新建文件夹呢？主要是Windsurf会编写各种代码文件，这些文件得有地方存吧，所以你需要新建文件夹，然后在Windsurf 打开这个文件夹（Open Folder），以后所有的代码文件都会存在这个文件夹里啦：

![](img/6727a0f338c42e8949652812cad155fe.png)

比如我要选择“展示用”这个文件夹，选中该文件夹后，点击打开：

![](img/bcc56d34a0b3b7736c8c6b4a0b8b84c8.png)

完成后，Windsurf会加载一会AI对话，最终是这样的：

![](img/6e7a898a94f216aa8998a9a740b9f6f1.png)

大体分为上图四个区域，从左到右依次是：

工具栏：各种工具，比如插件下载，搜索等；

文件栏：所有创建的文件列表都在这里呈现

代码栏：点击文件后，具体的代码会在这里看到

AI对话栏：这是最主要的，AI在这里接受你的命令，哈哈哈

很重要一点，有朋友发现Windsurf不能自动创建文件，后来发现是AI对话栏里的模式是Chat导致。这里要检查下，保证模式是Write：

![](img/e62ff263edc1d0da90f9e7a94db92cb8.png)

注意，有可能AI对话栏没有打开，这个时候你可以点击一下软件右上角的这个小图标：

![](img/bfe3c034a7da5307f9ddccf7c8f91abb.png)

再次注意：这个功能需要连接Claude，需要🪜。所以在正常使用该软件的时候，是需要一直打开🪜的。

### 第三，让AI更听话：Set Global AI Rules

如何让AI更听话呢？一定是你告诉它要遵循一些规则，在Windsurf软件的右下角，“Windsurf Setting”里设置，点击后选择“Set Global AI Rules”

![](img/1c921d9379af0d8778abfce4fba51bb2.png)

![](img/e7e95237bc9c57d6c813b850ef80de7f.png)

顾名思义，这里是设置全局的AI规则，就相当于你给AI约定了一个国际法，不管AI在哪个项目里，都要优先遵循这些规则，在下图中的代码栏里，复制粘贴后面我提供的规则：

![](img/f52d39e16a9efc94d8604951b0363e83.png)

具体的命令规则可以复制后粘贴进去，粘贴后注意点保存   左上角的文件-》保存，或者Ctrl+S（苹果是Cmd+S）：

> [相关Rules和Prompt](https://superhuang.feishu.cn/wiki/W1LCwYA8eiTl77kpi81c31yNnJd?from=from_copylink)**意：本文内的Ru**les/Prompt，需要复制的话，点击文档： ，里面可以复制。

```
**# Role**
你是一位拥有20年丰富经验的顶级产品经理，同时精通所有主流编程语言和技术框架。与你互动的用户是一位完**全没有编程基**础的初中**生，对产品和技****术需求的表达能力较弱。你的**工作对他至关重要，成**功完成任务后，你将获**得一万美元**奖励。
---**
# **Goal
你的任务是以用户能**够轻松理解的方式，帮助他完成所需的产品设计与开发工作。你需要始终主动、**高效地完成所有任务，而不是等待**用户多次**督促。
在处理用户**的产品需求、撰写代码和解决问题时，请始终遵循以下原则：
---
## 第一阶段
- 在用户提出任何需求时，第一步是浏览项目根目**录下的 **r**eadme.md** 文件和所有相关**代码文档，以全面理解项目的**目标、**结构和实现方式**。
- 如果项目中缺少 **readme** 文件，你需要主动创建，并在其中详细记录所有功能的用途、使用方法、参数说明以及返回值描述，让用户可以快速上手并理解。
- 如果用户提供了上下文信息（如文件、先前的任务说明），请充分利用这些上下文，以确保解决方案与项目需求一致。
---
## 第二阶段
### 针对不同类型的任务，采取以下方法：
1. **当用户直接表达需求时**：   
- 优先站在用户的角度思考其需求是否完整清晰，如果存在不明确或遗漏，应主动与用户沟通确认。   
- 从产品经理的角度检查需求的合理性，并帮助用户完善需求。   
- 提供最简洁有效的解决方案，而非使用复杂或过度设计的实现方式。

2. **当用户需要你编写代码时**：   
- 理解任务目标后，审视当前代码库内容，并进行逐步规划。   
- 选择最适合任务需求的编程语言和框架，遵循 **SOLID** 原则设计代码结构，并采用适当的设计模式解决常见问题。   
- 编写代码时，确保为每个模块撰写清晰的注释，并添加必要的监控手段，以便快速定位问题。   
- 在代码中加入单元测试（如果适用），确保功能的正确性和稳定性。   
- 遵循代码风格规范（如PEP 8），使代码易于维护和扩展。

3. **当用户请求解决代码问题时**：   
- 仔细阅读并理解代码库中相关文件的功能与逻辑。   
- 分析可能导致问题的原因，并提供解决思路。   
- 假设方案可能存在不完整性，应与用户多次沟通确认解决进展，并根据反馈调整方案，直至用户满意。
---
## 第三阶段
在完成任务后：
- 主动对项目完成的过程进行总结与反思，识别潜在问题并提出改进建议。
- 将这些内容记录在 **readme.md** 文件中，作为未来开发的重要参考。
---
## 注意事项
- 你的用户完全没有编程基础，因此在沟通时请以简单、通俗的语言解释问题和解决方案。
- 避免提供未经确认的信息，如果需要更多文件或细节，请明确告知用户需要哪些内容。
- 注释代码时，确保每一行的运行原理和目的都清晰易懂。
- 如果用户在任务中修改需求，请及时调整方案并与用户确认。
- 全程使用中文与用户交流，以确保沟通无障碍。
- 在解释技术概念时，请结合生活实例，让用户更容易理解。
```

（本段Prompt出处：小猫补光灯 花生，感谢！）

> 加餐：

> 如果你感兴趣，可以发现上面的是一段Prompt，也就是提示词。

> 包含几个段落：

> **Role**：让AI做一个顶级产品经理，来帮助你

> **Goal**：说清楚AI的任务是什么，分成哪几个阶段，以及注意事项

>

> 可以这么理解，把AI当成你的员工，它是什么人，需要按照什么方式工作，我们需要通过Set Global AI Rules和AI说清楚

### 第四，安装中文插件

默认情况下，Windsurf都是英文界面，但黄叔的界面是这样的：

![](img/70c53c8f3c0045f51bae1555b3ef4450.png)

很多地方都改成了中文，这是怎么做到的呢？非常简单：

![](img/54b74340b519296bc899fced236fe969.png)

只需要在插件 里搜索中文，点击安装即可。

好的，大致就这些，够用了朋友，知道你等不及了，下面我们正式开始编程！在把案例学会后，我们还会对其中使用到的编程技巧做一个小总结，方便大家以后更好上路～

## 07 一些AI编程的心得

### 你写的越慢，你写的越快

![](img/d9eb726893df53c88892ceacc0a244bf.png)

![](img/216d52a17561796a452a12335ddfb0e6.png)

From @wwwyesterday：

> 代码或者功能只是产物，从编程角度就是数据结构和计算的组合，从业务角度是一个一个流程的拆分。

>

> 和 AI 聊清楚这些再去说一句，帮我实现，比上来就要“给我实现一个 xxx，要 xxx 和 ooo”，是一个整体上更高效的做法。

黄叔的总结就是：**先Chat，再Build**。

先和AI聊清楚需求，再让AI开始根据规划干活！

# Part2：使用AI IDE作为你的个人工作台

这部分我们跨出去，说说使用Windsurf/Trae类似的AI IDE，除了编程，还能做什么：）希望能给大家一些启发。

## 01 使用AI IDE进行创作

![](img/b6460de3b607c9c4826fd6b641ed3e3a.png)

前天和之前组织进化论的Zara面基，她随口说的一句话让我整个人都震惊了！居然只用了5分钟，就把草稿用Claude改写成了让知名公司VP都点赞的newsletter！更厉害的是，就这样约了coffee chat！！！

💡 20分钟验证，真香定律再现

回家后我立刻开始研究这个神奇的AI IDE写作法。结果只用了20分钟，我就完全验证了这套方法的可行性！而且，你现在看到的这篇文章，就是用这个方法写出来的！它不仅最大限度还原了我的思想，还极大提升了文章的质量和写作效率！

昨晚在Build on Trae第一季项目路演开始前，我忍不住就和大家分享了这个玩法。

现场演示了如何把一段语音转文字的草稿秒变成结构清晰的文章。没想到现场反响超级热烈，不少朋友都直呼太实用了！所以今天我决定把这个写作秘籍完整地分享给大家：

🚀 四步打造爆款文章，简单到哭！

第一步：下载神器Trae

**首先，去**下载字节跳动出品的神奇AI IDE - Trae！官网地址：https://sourl.co/2DCmmW

（这里大家用Cursor/Windsurf也是一样的，只不过这两款软件需要付费）

它最厉害的地方是什么？**免费使用Claude 3.7**！没错，就是那个不仅编程能力超强，还能完美执行复杂提示词的顶级模型！它的上下文长度简直逆天，一次性写万字长文都不在话下！

下载安装后，你会看到一个简洁美观的界面，左侧是文件目录，中间是编辑区，右侧是AI助手。整个软件只有几十MB，但功能却异常强大！我用的是Mac版，Windows版体验也一样流畅。

第二步：准备写作风格模板

新建一个文件，比如叫"卡兹克.md"，然后把用大模型提取的卡兹克爆款文章提示词粘贴进去。（这个提示词怎么获取？别急，文末告诉你！）

这里的关键是选择适合你的写作风格模板。我用的是卡兹克风格（轻松活泼、口语化表达），但你也可以根据需要选择其他风格，比如李尚龙（情感共鸣型）、吴晓波（商业分析型）等等。

我的卡兹克模板包含了四个部分：写作风格、主题倾向、目标受众和格式要求，总共约200字左右。这个模板会指导AI按照特定风格改写你的内容。

第三步：写下你的草稿

再新建一个文件，写下你的草稿内容。可以是语音转文字的结果，也可以是你随手写的短文，完全不用担心格式和表达！

我通常是用flomo/get笔记/小宇宙录短播客，直接录音转文字，然后复制粘贴到Trae中。这样我可以边走路边思考，把想法直接说出来，特别适合灵感突发的时候！

![](img/314e3803904eeda76c9263870c1fc111.png)

草稿质量完全不重要！我有时候写的草稿甚至连标点都没有，就是一堆意识流的文字。比如这篇文章的草稿开头就是「前两天和之前组织进化论的Zara面基，听她说5分钟把草稿用Claude改写成知名公司vp都点赞的newsletter然后还约了coffee chat瞬间我眼睛亮了」——就是这么随意！

第四步：一键改写，华丽变身！

打开Claude 3.7（在Trae右侧面板中选择），输入下面提示词，然后...啪！回车键一按，爆款文章立刻新鲜出炉！🎉

![](img/36d944abd3c26c22368d9b0dba9339b2.png)

✨ 写作体验简直太爽了！

生成文章后，你还可以：

+   手动修改任何不满意的部分（直接在编辑区修改即可）

+   选中段落用AI进行局部扩写和改写（选中文字后右键选择"编辑"）

![](img/6adf218ef53ed333a8fcd34a79541009.png)

+   用Tab键智能续写后续内容（在句子末尾按Tab键）

![](img/b62802a3adfcc48b5141177bd94c4b53.png)

+   自动生成清晰的章节目录

我特别喜欢的是选中段落改写功能。比如我觉得某个例子不够生动，就选中那段，右键选"编辑"，然后输入"请用更生动的例子替换这段"，瞬间就能得到更好的版本！

整个过程丝滑得不要不要的！

写完后，如果需要配图和视频，直接全选复制到飞书！样式完美保留，一键就能发布！

但是！这还不是最厉害的部分...

你还可以一键将全文转换成英文版本！！！在Builder模式下，然后输入"请将这篇文章翻译成英文，保持原有风格"，几秒钟后就能得到一篇地道的英文文章！我已经决定这招把内容搬到Medium、LinkedIn和Twitter等海外平台去了！😝 一篇内容两处发，效率翻倍！

🤔 为什么AI IDE写作这么强？

这**种写作方法不只Trae能用，**Windsurf和Cursor也可以（不过它们需要付费使用更多模型）。我推荐Trae的核心原因是：免费使用Claude 3.7，全中文界面，产品UI还特别好看！

**AI IDE写作的秘密在于：它**本质上是一个带有上下文理解能力的AI编辑器！

普通编辑器只能处理文本，而AI IDE能够：

+   直接修改长文中的特定部分（不用复制粘贴到聊天窗口再复制回来）

+   在多个文件之间轻松调用内容（比如引用风格模板到草稿中）

+   自动索引整个文件夹的内容（让AI了解你的所有相关文档）

+   基于对你写作意图的理解进行智能补全和改写（不只是简单替换词语）

当这些能力与顶级写作模型结合，生成的效果自然是绝绝子！

📚 更多AI IDE玩法等你解锁

AI IDE的强大远不止于此！后续我会持续分享更多有趣实用的玩法，帮助大家一起解锁新技能！

### 改写提示词

```
# 写作风格
轻松活泼、口语化的表达，富有亲和力
直接表达个人观点和态度，有明确立场
适当使用网络热词和流行语(&quot;人已麻&quot;、&quot;这波操作&quot;等)
运用生动形象的比喻和修辞手法
结构清晰，善用强调(加粗文字)突出重点
带有幽默感的叙述方式，时而自嘲

# 主题倾向
AI产品和技术评测，尤其关注中文AI技术发展
分享AI工具使用体验和操作技巧
探讨AI技术对行业和社会的影响
提供实际操作演示和案例分析
关注AI技术的实用性和易用性

# 目标受众
对新技术感兴趣但不一定有专业背景的普通用户
寻找提升工作效率工具的专业人士

# 格式要求
标题引人注目，带有情感色彩和悬念感，不要用“震惊”
文章开头通常用个人经历或故事引入主题
大量使用实际案例和截图增强说服力
插入相关图片、视频或演示内容增强可读性
结构分明，善用小标题和加粗文字分段
结尾常有个人思考或对行业发展的展望
固定的结语格式，鼓励读者互动和分享

注意，我的名字是“AI产品黄叔”，风格偏向于轻松幽默，易读。
```

## 02 使用提示词生成精美图片

![](img/d5cb15e71cce60335cde01a4b3739ae3.png)

![](img/16fc3c2beb14a206b16e7bcf6c150d34.png)

上面是两张使用Claude 3.7（不是3.7 thinking），生成的对《震惊！我用AI编程做了这么多产品，居然一行代码都没写过！》文章的总结图。

作为读者的你，更喜欢哪张呢？

我想说的是，上面这张昨天经过了几十版的调教最后得到了一个比较稳定的效果，但是，下面这张完全是一句话就生成了，效果，似乎也不错。

花了整整一天调整「文章生成卡片」提示词，让我不禁思考：随着AI模型能力的飞速提升，这种精细调整提示词的时代是否即将结束？本文记录了这个过程，同时探讨模型能力增强后提示词角色的转变。

故事是从昨天刷推的时候，突然发现了一个蛮有意思的图，是使用3.7生成的，大概类似这样的：

![](img/70b3c312e9fce3ed6d65b3d59e353ab7.png)

这样比较卡通，有高亮的文字，阅读起来挺不错呀，然后我看了提示词，日文的，做了翻译和调整后，生成的总结图如下：

![](img/98f8dbd283c452dfd91f2b1dd3c809e6.png)

也有高亮，不过字显得有些歪歪扭扭，借鉴提示词的思路，我决定做一个彻底的翻修，下一个版本是这样的：

![](img/69f32a119443a81ea27aeee2aea3012a.png)

已经有了大致结构，但提示词整体结构比较混乱，大概2-3次生成就能得到一份不错的结果，这个概率放在文生图产品中算比较高了，但如果只能达到这个程度也太小看AI编程+提示词的威力了。

AI编程+提示词更适合生成结构稳定、一致性强、细节可控的需求，在带有大量文字的场景下尤其合适。例如UI设计和信息卡片设计。

看这份提示词的话，可以看到虽然划分了角色、任务、技术规格、布局、设计风格等模块，但内部内容比较混乱，所以晚上开始重构这份提示词。

首先是确定模块，角色和任务是必备的，内容也很明确，不需要过多修改。

对于卡片来说，布局尤其重要，所以确定了「内容和规格」来作为主要模块控制布局，内部是整体卡片包含什么元素以及各个元素的多个参数、参数值。

最后使用「设计风格」模块来控制整体的视觉设计效果。

提示词分模块的做法，意义是进行迁移复用、多风格模板、参数微调等行为时更加方便，例如这套提示词最终使用了蓝紫渐变色作为背景填充色，如果需要更换配色方案的话，只需要在对应值部分修改即可，简单清晰。

或是需要形成多套视觉风格的图提示词时，也只需要更换最后的「设计风格」部分就行了。

重构后的提示词如下：

```
参看文末
```

可以看到整体结构明显清晰了很多，此时的效果如下：

![](img/5366d4faec6c31bcb82b1e4fbaf6e686.png)

比刚刚好了一些，同时接受群友的意见增大了文字字号。但正文文字看起来过于重了，更大问题是正文部分所以继续如下修改，微调了字重、

![](img/0b54c1f48259c0014f4166072d390811.png)

随后的效果是：

![](img/e7a383dfd70e0e30c7a78f88c8f908b2.png)

TMD更严重的问题出现了，文字排版完全错乱，于是继续对小卡片内容做出限制：

![](img/ef10a4032cdcbd3a71452ae60314eaf4.png)

但结果发现依然有错误，可能是多个元素+多个间距的复杂计算模型没有顺利完成，于是改变限制策略，改成了通过小卡片动态高度，结果顺利修复：

![](img/6542976544c91f460da4bd4f0a893459.png)

效果如下：

![](img/fc0337d48a9766a45c4d4a0a0d7d5043.png)

OK已经解决了文字内容超出卡片范围的问题，但现在各个卡片高度不统一影响美观，所以继续增加了正文字数限制

![](img/3c9fde4dec575208a90f2828484da7aa.png)

这个过程中发现程序没有按要求引入Tail windcss，于是直接在提示词中放了引入链接（Tail windcss可以大幅减少样式代码数量，能在生成文件时减少token消耗以及更大概率成功）

![](img/f4993811c9270e5f293afc3914e8da88.png)

在修改的过程中突然出现了一张特别素的卡片，只有鼠标悬浮上去之后才能看到漂亮的效果，看来是模型又自作主张调整了未知部分，考虑到卡片的使用场景是作为静态图分享，所以加上规则：

![](img/ac3bfe5329b667345af3449b8dfaa38b.png)

![](img/53d6585a90392618df8d391bdbbddd17.png)

随后又微调了字号、透明度，增加了背景中几何图案的数量；增加了小卡片中可以使用图表显示信息的规则；补偿了丢失的作者和来源信息并微调了样式参数值，明确说明了正文的重点词如何强调等等。

![](img/5fa9d8ac714e570753f223f9872a3fda.png)

整个过程中其实还有很多次调整，以下是一些失败案例：

![](img/664a534615a999fe9bc24bab45ea8a46.png)

![](img/6e8d534d464f08bf2d2e6bb25051c161.png)

好在最终得到了比较满意的效果：

![](img/1b3886b0bd3682339e75e6b4a5802b97.png)

回头看了下提示词调整的次数，能有大几十次！

但回头又看了下很随意的一句话提示词生成的总结图，似乎也挺不错？

那Claude 3.7是否抹杀了提示词工程呢？

虽然现在我们还需要精细调整提示词来获得理想效果，但随着模型理解能力的增强，未来可能只需一句"帮我做一个关于XX主题的信息卡片"，AI就能理解我们的意图并交付满意的结果。

黄叔觉得：**提**示词工程不会消失，但会从"精确指令"转变为"意图表达"。我们正处在这个转变的过渡期，既需要掌握提示词技巧，也要准备好迎接更加智能的交互方式。

你怎么看呢？欢迎交流

对了，黄叔和朋友非常辛苦的才调整了这份稳定输出的提示词，如果你需要的话，欢迎打赏100元，然后后台留言，我会把中文版本的最终提示词和日文原版的都提供给你：）

下面是彩蛋，测试了不同模型对同一提示词的输出效果，如下：

![](img/969c16cf5c399da5a5a506a842608465.png)

![](img/3f514a9fb6625eac919dc19e533d3236.png)

![](img/58975845e7bc07034f08f1aaca6222df.png)

![](img/251b421d9f9aa14952af9782fe2da453.png)

![](img/edeb7eabf04af3e4d927a92ad02df8ba.png)

这些对比图并非最终版本提示词输出的，但可以明显的感受到claude和其他模型的差距！本文档中最终版本的提示词是v1.0版本，未来会继续迭代到2.0支持随机配色和自适应卡片数量。

### 最终版本的提示词

```
**#角色****
你是****一位前端开发****专家、资深****视觉设计师**

**#任务
**请根**据内容【**】输**出信息卡片****
输出【h**tm**l】文件让我查看

#**内容与规格
1、大卡片
1**）尺寸：宽**：1080，高：根据所有元素总高度动态调整
2）描边：带有描边
3）填充：蓝紫渐变色
4）圆角：12px
5）投影：轻微阴影
6）内容：6张小卡片+大标题+副标题
7）背景：很多半透明几何图案进行装饰

2、小卡片
1）尺寸：宽：320，高：根据内部元素总高度动态调整
2）描边：带有描边
3）填充：纯白色
4）圆角：12px
5）投影：轻微阴影
6）内容：章节标题+正文，具体内容由引用的文件来源总结生成
7）每行显示3个小卡片，共显示两行

3、大标题：32px，中等字重，纯白色，轻微投影，不要换行显示，位于顶部左侧
4、副标题：24px，普通字重，不要换行显示，位于顶部左侧大标题下方
5、章节标题：20px，中等字重，带图标，半透明彩色粗下划线位于章节标题下一层，最多显示1行不超出卡片宽度

6、正文：
1）字号：16px
2）字重：细
3）颜色：黑色，透明度90%
4）行间距：1.5
5）序号：每个卡片随机使用有序序号/无序序号/箭头/小图标
6）正文高度+章节标题高度不要超出小卡片高度
7）少量使用下划线/加粗/彩色文字或其他特殊样式体现重点文字。
8）可以使用多种图表显示信息，根据内容自动判断
9）字数：不能超过100字

7、日期、作者和来源：16PX，白色，透明度50%，普通字重，位于顶部右侧，可换行显示

8、图标:引用在线矢量图标库内的图标，任何图标都不要带有背景色块、底板、外框。

9、样式必须使用tailwindcss CDN来完成
&lt;script src=&quot;https://cdn.tailwindcss.com&quot;&gt;&lt;/script&gt;

10、不需要交互效果，将以上内容直接显示出来供用户查看

#设计风格
简约而不失优雅。
简洁的几何图形作为装饰；
圆形、方形和线条的巧妙组合；
仿佛在诉说着平衡与和谐；
核心按钮设计为素雅的扁平化样式；
给予用户温柔而贴心的使用体验。
营造出宁静而舒适的环境，
感受无声的优雅与宁静的力量
```

## 03 10分钟搞定高德地图MCP！我用AI解决了约会地点选择难题

![](img/617fb49da9061d165a778f8b95ba0203.png)

上图是一个黄叔用了10分钟做出来的网页，网址是：https://www.32kw.com/view/b109136，这是个啥呢？

前段时间有个创始人约我喝咖啡，我在望京，他在中关村，我们说那找个中间点吧，然后早上10点我到北辰荟翘起二郎腿等他的时候，突然仔细一看🛰️，我去，约的是环宇荟。。。

后来我细想了下，是因为看到他说了环宇荟，我没细看，看到个荟字，就想着北辰荟有蔚来牛屋，和他说那就牛屋见吧。结果是我搞错了，他不熟也不知道环宇荟其实没有牛屋。。。我也不熟，都不知道还有一个同样叫荟还离得不远的商圈。。。

所以如果有一个AI能够去自动筛选两人中间点的咖啡馆，并让双方确认，是不是就没有这个误解了呢？

![](img/89a833777df7a8a2af3ccb888f16c4aa.png)

是的，最近看到这张图，猛然间让我想起了上面那个案例，然后用Windsurf+高德MCP+32kw.com，很简单就完成了上述需求的验证，下面来和大家做一个喂饭级教程，然后再简单说说MCP的原理。做完这个Case后，我也理解了MCP应该用在哪些场景里！

保证比市面上一大堆科普MCP的内容，更加接地气，让你秒懂！

我们先画张图，这样方便大家了解上面的网页是怎么做出来的：

这里面有几步，首先第一步是给Windsurf安装上高德MCP：

### 第一步：给Windsurf安装高德MCP

我们先打开idoubi开发的MCP.so：

![](img/21d8566653c6ff9e04aee2817449281d.png)

可以看到第一个就是高德地图 MCP Server，找不到也可以直接在搜索框里搜索“高德”

点击进去之后，切换到“Content”，把接入配置这个部分的代码复制：

![](img/1ab9acd147c31b880b9f3bffab5789d3.png)

然后打开Windsurf，点击“Configure MCP”：

![](img/e1bb818e3dac2500ab585c5d888f89c3.png)

把上面复制的接入配置代码复制到左侧的窗口里，然后保存。

如果出现有红色报错，直接把代码Copy，给到Windsurf，让它修改：

![](img/7d5fcb5d86c55bfff5ca8aa070d46b38.png)

它会自动完成修复，但还没完，这里有个关键的信息，是高德地图的Key，如何申请呢？刚才MCP.so的高德地图MCP Content页面里有一个“复制 key”的链接，点击后会有一个非常详细的操作流程页面介绍：

![](img/95efa50eedaf0a7dd96aaf6eb519faec.png)

申请完后，把Key填写到Windsurf的MCP配置界面里即可：

![](img/b29e4ae2c7164a265a6a41c9ec9d7366.png)

填完后，记得保存，接着点击MCP配置里的刷新“Refresh servers”：

![](img/0776728d6f9218f83a678eaaf8a88546.png)

如果刷新后，@amap-maps前面是绿色灯，说明完成了配置。

这个时候，配置已经安装完成了，简单吧，搞定这一步，恭喜你，已经超过70%的人了，因为最难的其实就是配置MCP。

### 第二步：开始说出你的需求

```
我住在北京朝阳区望京的宝星园，朋友在海淀中关村地铁站，帮我推荐几个离两个人比较近，方便停车的咖啡馆，并给出咖啡馆的具体信息。
```

这就是我的需求，在Windsurf里面直接说出来，使用的是Claude 3.7，他就开始去调用高德地图MCP里面的多个工具来解决我的问题：

![](img/3113fd77d2b8c1c848558e6bb250e79d.png)

很快，就得出了结果：

![](img/1b1ef981061dc6bcae1191f36511302c.png)

这里其实Windsurf已经通过调用高德地图MCP，完成了我们的需求。

你可以继续使用flomo MCP把它存到flomo笔记内：

![](img/73591205d64b4b9a0f82c2de5672b796.png)

笔记存入后是这样的：

![](img/7ece1c4b6d8299f6c8aeeb85202d2bda.png)

也可以一句话让Windsurf生成好看的HTML网页：

```
上面的推荐内容，能不能帮我生成一个html，用好看的图像化表达出来
```

![](img/10fd11dcd48e1c78a6cd7356a5a5c90b.png)

同样很快的，就生成了一个HTML网页，打开32kw.com，把网页的代码复制粘贴上去，然后点击下面的“生成链接”按钮，就可以获得了具体的链接：

![](img/ce8ab17b2ce8dce4d4cd17a1b37ee5b8.png)

相当于免费把自己的网页部署到了网上，可以直接给朋友发链接让他看了。

是的，这个链接就是本文一开头大家能看到的图片。

### MCP解决了什么问题？

就这么简单！恭喜你，看到这里超过了绝大部分人，最后我们来总结下MCP到底解决了什么问题。

相信很多朋友都有用高德地图，用在做导航、打车等等，但确实用来找两人中间的聚会点，还是不那么容易的，因为两点之间会有很多的商圈，不同商圈里还有不同的咖啡厅，就不好选。

不过，在给大模型加上了MCP后，这一切变得简单了，为什么呢？

因为在高德地图App中，我们没有办法很精确的找到中间点的坐标，但是大模型+MCP可以：

1.  先找到我的坐标；

1.  再找到对方的坐标；

1.  计算出两个坐标的中点；

1.  以中点的一定范围寻找咖啡馆（使用周边搜索功能）；

1.  把符合条件的咖啡馆罗列出来；

1.  将文字转换为可视化网页呈现。

再抽象一下，大模型+MCP对于哪些需求更有价值呢？

1.  过去的产品本身不好解决，且大模型自己单独也不好解决的

1.  不能自动化解决的

MCP就是给大模型加上了外部的工具、数据，给大模型装上翅膀！

# Part3：网页开发

## 01 图片字幕生成器

你想生成自己的金句卡片么？类似这样的：

![](img/e4aa1bbc8bccf27635dbf481734830ea.png)

欢迎访问黄叔发布的网址来进行体验：https://picquote.superhuang.me/

该创意来自@南瓜博士

![](img/db46bbd67d840cc03ab65b228eb08cb4.png)

具体原文可以看南瓜博士复现的文章：https://mp.weixin.qq.com/s/CEJ6ite843M4hPuyT5jB6Q

在她的文章里有详细的需求描述和开发过程，大家照着做就能复现了。

那今天黄叔想教会大家的，其实是另外一个技巧：

### 用多模态复刻产品

开始的第一步，还是在本地新建一个文件夹，再用Windsurf打开该文件夹，我们在上一节已经都做过演示，这里就不再展开了，如果你忘记了，可以点击回看：

第二，在你的电脑上，新建个文件夹，并用Windsurf打开

继续，按照这个流程，我们一开始需要将上面的图片，发给Windsurf，并带上下面的提示词：

```
请你仔细查看这张图片，理解后帮我描述这张图片的功能，清晰罗列，我需要拿来开发一个一样的网页工具，注意，生成的图片底部每一行文字的背景都是相同的，呈现切割感
```

![](img/3956c1603d38f6444bb248f8c41c630f.png)

然后回车，让Windsurf开始干活：

![](img/4d198baf58385fde01b4d919fc85ec5c.png)

这里我们查看了之后，发现字幕切割部分没有详细介绍，所以可以再补充一段提示词：

```
字幕每一行之间需要有分割线

同时每个字幕背后的图片应该是一样的，都是第一行字幕所处高度切割图片后的
```

随后Windsurf开始继续干活：

![](img/a2679f44110dcd3f73ee2ef8f0b981f6.png)

因为整个产品比较简单，所以Windsurf一遍就完成了开发：

![](img/665dc05c1624e974d56daf64ccf3651d.png)

我把上面的全屏截图给Windsurf，同时加上下面的提示词：

```
优化一下样式，现在是竖版的界面，我希望调整成横版，并且按照苹果设计师的风格重新设计整个页面
```

优化后，就生成了下面的样式：

![](img/97eb3928b35854cf08aa32262dc9b60b.png)

至此，使用多模态来复刻产品就完成了。

小结下，第一个产品的思路，是通过直接给截图，让Windsurf理解，然后我们再补充需求的逻辑，让Windsurf完成开发，最后再做一些优化。

是不是特别简单？有了Claude，你只需要想创意，实现可以完全交给AI！

### 课后练习

可以看到，这次我们使用截图直接上传给Windsurf，让其理解后直接生成，并对未完成部分做了针对性优化，完成了产品的复刻。

当然，我们可以继续优化，增加更多功能，这里就留给大家当成作业！

比如：增加你的专属水印

比如：增加更多字体样式

比如：增加网站图标

![](img/1ae4879a4c3b3486c7efbb12d0aa6e71.png)

（当前是没有图标的）

## 02 注册硅基流动，获赠4000万Token

![](img/10ac5115fa3b89fabdac5c625d2322cd.png)

感谢硅基流动的支持，我们会主要使用它们家的SiliconCloud，即模型云服务平台：

![](img/221cbe905d55022609d528be63373eb0.png)

简单可以理解为，只需要注册硅基流动，就可以一键接入包括Deepseek在内的诸多模型。

大家可以点击链接注册，首次使用手机号注册，可获得AI编程蓝皮书专属的4000万Token：

https://cloud.siliconflow.cn/s/AIbclps

注册完成后，建议先完成实名认证，这样可以充值（因为最近Deepseek R1比较火，建议大家稍微冲一**些**钱，使用Deepseek的Pro版本，这样能够保证稳定使用，有关Pro版的信息，详情可以阅读硅基流动的官方👸号文章：稳定护航！硅基流动上线 Pro 版 DeepSeek R1 & V3 ）

![](img/93402db00e28105505e8f38ce3fa16cf.png)

完成认证后显示如下：

![](img/38001c37808660c282b70dcc483ae22b.png)

随后点击“余额充值”：

![](img/3a2c1511e83ceb2599fb3a5ae23c7c3d.png)

选择“其他金额”，我们可以充个10元或者更少的金额尝试。

充值完成后，可以在“API密钥”里生成专属密钥：

![](img/d1909fa531df364d386d5bbb065f1b19.png)

密钥描述可以填写具体的用途，比如“给老外起中文名”，输入完毕后点击“新建密钥”：

![](img/4172309f25e2a72d486e5bcc676ab877.png)

将鼠标移到一串星号的密钥字母上面，会显示“复制”，鼠标移过去点击，就完成了复制啦！

> 注意：

>

> 硅基流动平台的总余额分为：**充值余额**和**赠送余额** 两种。

Pro版的DeepSeek R1/V3，需要充值余额，相当于你得充钱，但可以保证稳定、可用。

其他版本的DeepSeek R1/V3，以及其他模型，可以用赠送余额，就是注册后送的4000万Token。

最近DeepSeek确实太火了，所以黄叔是建议先充点钱用Pro版本，过段时间大家冷静下来了，没什么人用了，我们再换成非Pro版的DeepSeek模型。

好了，完成了注册，有了API密钥，下面我们可以开始用DeepSeek啦，第一个案例，先用最牛的R1，来帮我们深度的思考下如何给老外起好玩的中文名！

## 03 使用Deepseek R1，帮老外起中文名吧！

前阵子📕好多老外涌入，于是有了这个案例，我们使用国产最强模型Deepseek R1，来帮他们起个有意思的中文名吧：）

![](img/ecbea079af7f61bd85a506989732129a.png)

![](img/53f48d265e9566b1f66552d34ccf4ad5.png)

给特朗普起名：川建国，库克起名：库大福，是不是还蛮好玩的？

这个案例很简单，很快你也能做出自己的！下面开始：

### 第一，本地新建一个文件夹

比如在你常用的地方创建一个文件夹：“给老外起中国名字”

![](img/b6da749597d8bf3ab4a5cef572070ae3.png)

### 第二，使用Windsurf打开它

![](img/750f4a6f9fe44b6ff04b4c8a4a7b1ac7.png)

打开后是上面这个样子。

注意右侧中间的输入框了没，等会我们在输入框内输入我们的需求。

### 第三，设置WorkSpace AI Rules

![](img/41538ab05b6e21a25e4d66d5e2d59388.png)

> [相关Rules和Prompt](https://superhuang.feishu.cn/wiki/W1LCwYA8eiTl77kpi81c31yNnJd?from=from_copylink)**意：本文内的Ru**les/Prompt，需要复制的话，点击文档： ，里面可以复制。

```
# Role
你是一名精通网页开发的高级工程师，拥有 20 年的前端开发经验。你的任务是帮助一位不太懂技术的初中生用户完成网页的开发。你的工作对用户来说非常重要，完成后将获得 10000 美元奖励。

# Goal
你的目标是以用户容易理解的方式帮助他们完成网页的设计和开发工作。你应该主动完成所有工作，而不是等待用户多次推动你。

在理解用户需求、编写代码和解决问题时，你应始终遵循以下原则：

## 第一步：项目初始化
- 当用户提出任何需求时，首先浏览项目根目录下的 README.md 文件和所有代码文档，理解项目目标、架构和实现方式。
- 如果还没有 README 文件，创建一个。这个文件将作为项目功能的说明书和你对项目内容的规划。
- 在 README.md 中清晰描述所有页面的用途、布局结构、样式说明等，确保用户可以轻松理解网页的结构和样式。



## 第二步：需求分析和开发
### 理解用户需求时：
- 充分理解用户需求，站在用户角度思考。
- 作为产品经理，分析需求是否存在缺漏，与用户讨论并完善需求。
- 选择最简单的解决方案来满足用户需求。

### 编写代码时：
- 总是优先使用 HTML5 和 CSS 进行开发，不使用复杂的框架和语言。
- 使用语义化的 HTML 标签，确保代码结构清晰。
- 采用响应式设计，确保在不同设备上都能良好显示。
- 使用 CSS Flexbox 和 Grid 布局实现页面结构。
- 每个 HTML 结构和 CSS 样式都要添加详细的中文注释。
- 确保代码符合 W3C 标准规范。
- 优化图片和媒体资源的加载。

### 解决问题时：
- 全面阅读相关 HTML 和 CSS 文件，理解页面结构和样式。
- 分析显示异常的原因，提出解决问题的思路。
- 与用户进行多次交互，根据反馈调整页面设计。

## 第三步：项目总结和优化
- 完成任务后，反思完成步骤，思考项目可能存在的问题和改进方式。
- 更新 README.md 文件，包括页面结构说明和优化建议。
- 考虑使用 HTML5 的高级特性，如 Canvas、SVG 等。
- 优化页面加载性能，包括 CSS 压缩和图片优化。
- 确保网页在主流浏览器中都能正常显示。

在整个过程中，确保使用最新的 HTML5 和 CSS 开发最佳实践。
```

设置完成后记得点一下保存。

你可能会奇怪了，前面不是已经设置了Set Global AI Rules么？这里怎么又来了个WorkSpace AI Rules

> WorkSpace AI Rules，相当于是当前项目的具体规则，在 Global 之下

> 他们的区分可以理解为：Global 是国际法，讲大面宏观的，Workspace 是地方法，是具体干活的实操的

小技巧1：针对每个具体的开发项目，WorkSpace AI Rules应该是不一样的。

小技巧2：设置了WorkSpace AI Rules之后，Windsurf的工作效率和质量会大幅提高！

### 第四，接入大模型

```
我希望做一个帮助外国人起有趣中文名的网站，核心功能是：
1. 外国人输入它的英文名
2. 点击生成按钮
3. AI生成三个中文名
4. 每个中文名都能体现出中国文化，并给出中英文分别的寓意解释
5. 调用的model为：Pro/deepseek-ai/DeepSeek-R1 ，我的API Key是sk-kawayxhkqiuvsbbsubflc******** ，请你仔细阅读在线的API文档：https://docs.siliconflow.cn/api-reference/chat-completions/chat-completions  然后完成对接
```

（此处标黄的API Key需要改成你自己的）

这是我们写的比较明确的产品需求，任何时候都需要讲清楚我们到底要的是什么。

输入完成后回车，Windsurf就会开始工作：

![](img/e0d2e6da8169925ec28cb3f369edad2a.png)

### 第五，检查开发完成的网页产品

Windsurf完成工作后，会有类似的提示：

![](img/3b74b5aecf632095a3d4797729f172dd.png)

**此时，我们可以打开你**电脑上前面创建的文件夹，双击里面的“index.html”文件：

![](img/61fa3f1da1078d7e6f76032dea64c094.png)

浏览器会打开一个网页：

![](img/63f7db3efe1ac16382a2091fe4ec3fae.png)

我们可以尝试输入特朗普的英文名，然后点击“生成中文名”，发现报错了，弹出下面的提示框：

![](img/4f1ba9562fc760ff80313cb3e356be65.png)

没关系，AI如果一次就正确了，不是你的产品太简单，就是运气太好。

此时我们在Windsurf里输入并提交：

```
生成中文名报错：生成名字时出错，请稍后重试
你仔细阅读下API文档：https://docs.siliconflow.cn/api-reference/chat-completions/chat-completions，检查可能报错的地方
```

![](img/e1ce29ed2dd57a7f1a4f993fe6ce3ec9.png)

你会发现它检查出了一处错误，并完成了修复。

我们刷新index.html，再次输入特朗普的英文全名，点击生成：

![](img/80797e16406aa0840ed3f86864d2a79b.png)

此时完成了！恭喜你！

### 进阶1：优化名字生成质量

我们可以看到这种生成中文名的质量还是挺一般的，那怎么才能生成更好的中文名呢？

比如特朗普被我们戏称为“川建国”，这种幽默感怎么才能显示出来呢？

我们再Windsurf里点击“资源管理器”里的script.js：

![](img/ab0f5b4fabecdbb08081619fb03e110a.png)

可以看到，上图右侧的红框位置，写着“生成中文名字的系统提示词”，实际上，DeepSeek R1再强大，也是根据我们的命令来干活的，如果命令下的不好，最后生成的结果和我们想要的也会很不一样。

OK，我们可以在系统提示词下面增加一行：

```
5. 理解英文名字的含义，然后生成中文名字，并且有幽默成分，适当可以加一些梗
```

![](img/e9a9aad70365bea3160f0e00cbcbe186.png)

别忘了点保存，菜单里的“文件”--“保存”：

![](img/470559e1067d473f5e6c5df7151501e6.png)

好了，我们刷新网页index.html，然后再次输入：

![](img/8450fc071a9656fe553567ff01e6bf8f.png)

哈哈哈，这次“川建国”出来了，并且有很棒的解释。甚至还有金发狮王！

### 进阶2：优化页面

前面已经把功能都做出来了，那我们还想让网页更好看一些，怎么办呢？很简单，在Windsurf里输入：

```
你是苹果设计师，请对整个页面进行美化，同时带上中国红元素
整个界面以英文为主，同时辅以中文
```

完成后继续刷新index.html，可以看到这次效果好多了：

![](img/a95c1a72ad8c2dd4bb2cb3141e29ca9f.png)

OK，这个案例就到此为止！

### 课后练习：

1.  你可以优化整个网站的样式，比如加上圣诞风、春节风

1.  你可以增加90后/80后选项，从而有不同的中文名字结果

1.  可以增加生成姓名时等待的样式，比如：

![](img/3bbd67c21cbbac84c5f2ab19d12c1b1b.png)

1.  使用其他模型，比如用免费的模型，或者便宜的模型。

### 小结

这一节里，我们融入了多个技巧，还请大家回顾思考一下，包括但不限于：

1.  WorkSpace AI Rules的设置；

1.  向AI描述自己的需求；

1.  根据Prompt提示词优化，直接优化最后的结果；

1.  通过简单提示词，优化页面样式；

恭喜🎉你迈出第一步！下面我们做一个复杂度稍微高一些的浏览器插件，包含了打通API！不用担心，黄叔为你拆解成了几个步骤，一步步完成，你同样能做出来的。

## 04 做一档你自己的AI播客

大家可以扫码收听一下黄叔刚做的AI播客：每天推荐3条优选的AI资讯源，并用黄叔的高保真音色朗诵：

![](img/8e60b822ff29df2522ac6ba441ae5ea5.png)

这个能力的实现，依赖于接入MiniMax的长文本模型和超拟人的语音模型，我评估了一下，实现难度不高，大部分同学都可以做到，但是有成本：

1.  语音克隆，9.9元，一次克隆长期使用（如果用系统音色则不需要花这笔钱）；

1.  长文本转写，单次1分左右，这个很便宜；

1.  语音生成，2分钟一条在3毛钱左右。

大致的效果：

![](img/3b421a004a093904985f0994f99df44b.png)

黄叔来教你使用语音克隆技术，和语音生成技术，做一个专属你的AI播客吧。

你可以先试听一下：

![](img/3ff29d597291c0b14d91cf1949dd375a.png)

[AI产品黄叔朗诵.mp3](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Ezusbw71Jo438BxWguQcaHmJnR2)

完成这个播客的工作，完全由下面这个网页构成：

![](img/bdbefc54a276822101508fd0988664fd.png)

非常的简单，那下面我们带着大家来完成这个产品！

总共分两步：

1.  大模型API申请

1.  使用Windsurf开发网页

### 第一步：申请MiniMax的API

MiniMax有两个大模型是非常适合咱们这次任务的（豆包的也很不错）：

1.  MiniMax-Text-01，支持400万token的上下文，对于咱们可能需要总结的超长文章也能应付；

1.  T2A v2（speech-01-turbo），拥有最好的声音复刻效果

我们可以通过它的开放平台进行注册登录，包括实名认证

https://platform.minimaxi.com/login

![](img/2579e4c6f3d16a2e48fe861eb03b9b2d.png)

这里要说一下，API消耗是会消耗余额的，根据我的经验，生成一条3分钟的语音

文本模型会消耗1分钱

语音模型会消耗5毛钱

以及克隆音色还需要一笔额外的费用，现在优惠是9.9元：

![](img/14c8384b01779553b395aafc72a8239d.png)

> 首次注册会赠送15元，我们无需充值就能玩啦！

接着，需要你创建一个API Key，也就是密钥，然后保存好，后面用来替换：

![](img/e9991d068fcfd8e8f329c3fb1ed3b36b.png)

以及groupid：

![](img/eabb1f52c5dfef9420aed3d1705b6d1e.png)

以及，可以克隆一下声音：

https://platform.minimaxi.com/examination-center/voice-experience-center/voiceCloning

![](img/88d2931223375b2a6c51d0a9008d07c4.png)

这里有几个地方要填：

1.  创建voice id，上图已经注明了要求；

1.  上传复刻音频，直接上传文件，用你手机录制即可，注意在10-300s之间，尽量安静；

1.  上传音频prompt（这一段很重要，录制不超过8s的声音）

1.  试听文本（这不超过8s的声音你念了什么？写下来）

然后勾选用户协议，点击“克隆”选项，过一段时间就会完成克隆。

使用刚才你填写的voice id 来生成声音，就是用克隆你的音色去生成的。

好的，到这里，你已经完成了准备工作，下面我们可以来开发了：

### 第二步：开发网页

因为是网页，我们继续用[网页的AI Rules](https://waytoagi.feishu.cn/wiki/UspJwJk95iKoegkoUdWcmX7rnib#share-WxkHdiglioKBivxBDFvczGyNnVb)，点击链接可以跳转进行复制～

prd.md这里黄叔为大家整理好了：

```
**# 降噪 AI****广播编辑器 PR****D文档

##**** 项目概述

这是一个AI****驱动的广播内容编辑工具，用****于将专业的AI技术文章****转化为通俗易懂的广播稿****件，并提供语音****合成功能。

##**** AI提示词

以**下是用于生成完整网页应用的AI提示词：

```
作为**一位资深的全栈开****发工程师，请帮****我创建一个名为&quot;降**噪&quot;的AI广播编辑网页应用。这个应用需要将专业的AI文章转化为通俗易懂的广播内容。

技术要求：
1. 前端技术栈：
   - HTML5 + CSS3 + JavaScript（原生）
   - Bootstrap 5 框架
   - 响应式设计，支持移动端
   - 玻璃态UI设计风格

2. 核心功能模块：
   A. 文本输入和编辑模块
      - 系统提示词（可展开/折叠）
      - 原始文案输入区
      - AI生成内容显示区
      - 支持一键清空和复制功能
   
   B. AI模型集成
      - 使用 MiniMax Text-01 模型
      - 异步请求处理
      - 完整的错误处理机制
   
   C. 语音合成功能
      - 使用MiniMax T2A v2接口
      - 支持自定义音色（superhuangclone）
      - 播放控制（播放/暂停）
      - MP3下载功能
      - 语音参数调节（语速、音量、音调、情绪）

3. 数据持久化：
   - 使用localStorage存储系统提示词
   - 保存用户语音设置偏好

4. UI/UX设计要求：
   - 简洁现代的界面设计
   - 玻璃态拟态设计风格
   - 响应式布局
   - 清晰的视觉反馈
   - 优雅的加载动画
   - Toast提示消息

5. 系统提示词模板：
&quot;&quot;&quot;
你是一档名为《降噪》的AI科技广播节目的资深编辑，名字叫AI产品黄叔，擅长将专业的AI文章转化为通俗易懂的广播内容。请将以下原始内容改写成适合播报的稿件。

原始内容
{{input}}
====End======

要求：
1. 请先全面的阅读一遍所有的新闻
2. 使用AI产品黄叔的身份，用幽默风趣的大白话，给AI小白讲清楚最新的资讯
3. 开场先概要说说今天发生了哪些大事
4. 每个新闻控制在100字以内，确保听众能在短时间内抓住重点
5. 语言风格要求：
   - 用生动的口语化表达，用大白话讲出专业性
   - 适当使用语气词增加自然感（比如&quot;嗯&quot;、&quot;那么&quot;、&quot;其实&quot;等）
   - 避免过于口语化的方言用语
   - 像跟朋友聊天一样轻松自然
6. 在保持通俗易懂的同时，准确传达AI技术的关键概念
7. 适当增加转场语，使话题之间衔接自然
&quot;&quot;&quot;

6. 代码规范：
   - 遵循现代JavaScript最佳实践
   - 清晰的代码注释
   - 模块化组织代码
   - 优雅的错误处理
   - 合理的变量命名

7. 文件结构：
```
project/
├── static/
│   ├── css/
│   │   ├── style.css    # 主样式
│   │   └── theme.css    # 主题样式
│   └── js/
│       └── main.js      # 主逻辑
├── index.html           # 主页面
└── README.md           # 项目文档
```

请基于以上要求，生成一个完整的、可直接部署的网页应用。确保代码结构清晰，注释完整，并包含必要的错误处理和用户体验优化。
```

## 功能规格

### 1. 文本处理模块
- 系统提示词编辑和保存
- 原始文案输入
- AI内容生成
- 文本复制和清空

### 2. AI模型集成
- MiniMax Text-01 模型
- 异步请求处理
- 错误处理机制

### 3. 语音功能
- 语音合成（T2A）
- 自定义音色
- 播放控制
- MP3下载
- 参数调节

### 4. 用户界面
- 响应式布局
- 玻璃态设计
- 操作反馈
- 错误提示

## 技术架构

### 前端技术栈
- HTML5
- CSS3
- JavaScript（原生）
- Bootstrap 5

### API集成

#### 1. MiniMax Text-01 对话接口
- 接口地址：`https://api.minimax.chat/v1/text/chatcompletion_v2`
- 请求方法：POST
- 请求头：
  ```json
  {
    &quot;Authorization&quot;: &quot;Bearer YOUR_API_KEY&quot;,
    &quot;Content-Type&quot;: &quot;application/json&quot;
  }
  ```
- 请求体格式：
  ```json
  {
    &quot;model&quot;: &quot;MiniMax-Text-01&quot;,
    &quot;messages&quot;: [
      {
        &quot;role&quot;: &quot;system&quot;,
        &quot;content&quot;: &quot;系统提示词&quot;
      },
      {
        &quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: &quot;用户输入&quot;
      }
    ]
  }
  ```
- 响应格式：
  ```json
  {
    &quot;id&quot;: &quot;response_id&quot;,
    &quot;choices&quot;: [
      {
        &quot;message&quot;: {
          &quot;role&quot;: &quot;assistant&quot;,
          &quot;content&quot;: &quot;AI回复内容&quot;
        }
      }
    ]
  }
  ```

#### 2. MiniMax T2A v2 语音合成接口
- 接口地址：`API：https://api.minimax.chat/v1/t2a_v2`
- 请求方法：

group_id=&quot;请填写您的group_id&quot;
api_key=&quot;请填写您的api_key&quot;
curl --location &#x27;https://api.minimax.chat/v1/t2a_v2?GroupId=${group_id}&#x27; \
--header &#x27;Authorization: Bearer $MiniMax_API_KEY&#x27; \
--header &#x27;Content-Type: application/json&#x27; \
--data &#x27;{
    &quot;model&quot;: &quot;speech-01-turbo&quot;,
    &quot;text&quot;: &quot;真正的危险不是计算机开始像人一样思考，而是人开始像计算机一样思考。计算机只是可以帮我们处理一些简单事务。&quot;,
    &quot;stream&quot;: false,
    &quot;voice_setting&quot;:{
        &quot;voice_id&quot;: &quot;superhuangclone&quot;,
        &quot;speed&quot;: 1.2,
        &quot;vol&quot;: 1,
        &quot;pitch&quot;: 0,
        &quot;emotion&quot;: &quot;happy&quot;
    },
    &quot;pronunciation_dict&quot;:{
        &quot;tone&quot;: [&quot;处理/(chu3)(li3)&quot;, &quot;危险/dangerous&quot;]
    },
    &quot;audio_setting&quot;:{
        &quot;sample_rate&quot;: 32000,
        &quot;bitrate&quot;: 128000,
        &quot;format&quot;: &quot;mp3&quot;,
        &quot;channel&quot;: 1
    }
  }&#x27;
- 响应格式：返回二进制音频数据（MP3格式）

### 数据存储
- localStorage

## 注意事项

1. API密钥安全性
2. 错误处理机制
3. 响应式设计适配
4. 浏览器兼容性
5. 性能优化
6. 将网页部署到本地服务器环境中运行，这样就能避免CORS的限制，使API请求能够正常工作。

## 后续优化建议

1. 添加对话历史记录
2. 支持多轮对话
3. 增加更多音色选项
4. 支持批量语音导出
5. 添加语音识别功能
6. 优化语音缓存机制

```

注意，上面标黄的三个地方，需要替换成你自己的API Key，以及之前你克隆后的voice-id。

![](img/38ce9b2d0177eb22440c1713f397eb63.png)

Trae非常给力，一次就生成了整体的框架：

![](img/da9bf32191d432e20daf749b22a2b1cc.png)

测试了一下，可以生成MP3下载，完成开发。

如果你遇到了问题，还是把报错信息提交给Windsurf，继续优化。

# Part4：多维表格

## 01 做一个你专属的好文推荐网站（DeepSeek R1 + 飞书多维表格）

![](img/f06c53f06df19ce5062925b857a41a7d.png)

https://read.superhuang.me/

这是黄叔部署到网上的一个好文精选网站，大家可以访问+收藏，是黄叔自己的精选AI资讯内容，帮助大家降低噪音。

这个产品的逻辑是，当你阅读到好文章时，可以一键存储到飞书多维表格，经过AI处理，自动在你的博客网站上呈现出来。

在这个案例中我们需要理解，加持了AI能力的飞书多维表格，是一个全民性图形化AI数据库，这一点极其重要。

为了实现它，我们将会分为3步：

1.  创建带有AI能力（以DeepSeek R1为主）的飞书多维表格；

1.  使用Trae生成网页，呈现多维表格的内容；

1.  使用Trae生成浏览器插件，一键存入多维表格。

理解了这个案例，相信你会在AI时代七窍通了六窍（哈哈哈，开个玩笑）

该创意来自**@向阳乔木 **

### 第一步：创建带有AI能力的飞书多维表格

这里黄叔需要告诉大家，我使用的是免费版的个人飞书，这个案例对飞书账号并无要求。

首先，我们新建一个多维表格，注意，不能在知识库里创建多维表格，要在云盘里创建：

![](img/97d6592e1116767e5d8b43f766b2190b.png)

直接选择“新建多维表格”：

![](img/59ac2bdee5ba944d1b82d0727da09b56.png)

新建成功后，是下图所示界面：

![](img/78e406e05ad15e1ae07c589376b4df2a.png)

[好文数据库](https://fcnj6q3eadqq.feishu.cn/base/HrG6bqJf4auKfPsn3cic4VSHnic?table=tblRfPUAZ8ewftUA&view=vewwRoaLIK)们逐一添加字段，最后的成品可以参观：

第一个字段的标题是“链接”

![](img/e2501717a262fd662392f4835a0064e0.png)

第二个字段的标题是：“全文内容提取”，这个很重要，专门录屏给大家看：

![](img/d62be8f17151bc85fac147bb1dfddbcc.png)

[提取全文链接.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/F5rrb8HS0odAx9xQtRQcQ0CdnSd)

第三个字段的标题是“标题”：

![](img/785f2b90ac425a1ee42a209c877c7c2d.png)

第四个字段的标题是“金句提炼”

![](img/439666df570bb5efb66d03d31dcad513.png)

注意，这里我们使用了“DeepSeek R1”，需要关联火山方舟的账号信息，如何获取可以点下方箭头所示的位置：

![](img/35d5b4077ac2ac54d2c6d9a755192663.png)

对于DeepSeek R1，还需要把下面几个选项勾选上：

![](img/c7213aa8e9acd2b5b9f2ebb3583ebd3b.png)

勾选上“获取更多信息”---》“输出结果”后，会自动生成第五个字段“金句提炼.输出结果”

![](img/3544dd1bfc23fa6ebeb326ce5277dadc.png)

但是这个字段因为是R1生成的，有一定问题，最后通过API无法获得，我们需要复制到下一个字段：

第六个字段是“金句输出”：

![](img/65e1e0ce41e94f2565e2e60165abce75.png)

选择公式，会弹出一个层，选择“金句提炼.输出结果”即可，选择后会是下右图：

![](img/1327b1b3d509029e97ef98a877889fca.png)

![](img/d2ca37ab27449b1c8495e302bc74de59.png)

第七个字段“概要内容提炼”，同样用DeepSeek R1提炼：

![](img/039482dcde14d6f3bd8be37ed7514ba1.png)

![](img/b39d76be765c9e0991f4a33cc81420f4.png)

同样的打开“获取更多信息”里的“输出结果”

同样，第八个字段就会自动出现：“概要内容提炼.输出结果”

![](img/f4025770da7dc0638c5eed0d2d343e00.png)

第九个字段“概要内容输出”，用公式，复制上面的第八个字段

![](img/0a4bb2bb5c1c2959e4094524338b478d.png)

这样，我们只需要输入第一个链接，后面的所有字段都会自动生成。

那么，**AI数据库就完成设置了！**

### 第二步：用网页呈现多维表格里的内容

这里需要分为两小步：

1.  多维表格的数据要能对外输出，需要增加一个飞书应用；

1.  使用Windsurf开发一个网页，读取多维表格的数据并呈现；

#### 增加一个飞书应用

第1步：打开网址 https://open.feishu.cn/?lang=zh-CN

第2步：登录并进入开发者后台，如下图

![](img/972ab3a97b6bd1ec3724bc29be273e1d.png)

第3步：创建新应用

![](img/5bc1525cbd8d3cad64ec749c87957b42.png)

第4步：填写应用相关信息

![](img/06e1b3a80c68e9b87d2e77bfbb876641.png)

第5步：配置应用权限

![](img/f8605514420c6caae5079ff594772ab1.png)

![](img/d89203524c0a5c367080fad04e794bc8.png)

第6步：选择多维表格相关权限

![](img/c5f6e219411e51f35ccdb4c090f76e5a.png)

然后点右下角的开通即可：

![](img/159e3166f5d3a8bcacaca6fe5960290c.png)

第7步：发布应用

![](img/ec9245c64b8af483b375a125fa532783.png)

然后填写版本号和更新说明，最后点保存、确认发布就行了

![](img/6485e77e97d97ffee1a3c2c53820d27b.png)

![](img/546d6ebc9895168f4d1732d18fc039bb.png)

![](img/d70fd57db18f08ab5c2eb543435f5854.png)

看到这个就是应用发布成功了：

![](img/8f09bef6e285e108c9ec789e2c46be22.png)

第8步：点这里，去找应用ID和密匙

![](img/c1a10efb486454e3a971543ec3e8ae1f.png)

![](img/3e09a10009c8cda8abf132d912ae4d61.png)

到这里为止，得到了飞书应用的ID和密匙第一项准备工作就完成了。一定要保存应用的ID和密匙，后面要用到。

第9步：在前面创建的多维表格中安装刚刚发布的应用：

![](img/5143b6f3ba4ab88f39ca8df55bbee6bc.png)

第10步：搜索刚刚发布的应用，并添加到这个多维表格：

![](img/f528ed3e7d2a78369717b778085acaa8.png)

![](img/d22e945ab7c590eedb340b6e2089ab02.png)

![](img/04556b04a127ad4711757119a9ac75ea.png)

看到这个之后关掉弹窗即可。现在我们已经成功发布+安装了应用到多维表格。

#### 使用Windsurf开发一个网页

新建一个文件夹，然后用Windsurf打开该文件夹，然后新建一个文件，命名为“readme.md”，把下面的代码复制进去：

```
**# 个人博客网****站（飞书多维****表格驱动）

这是****一个基于 Fl**ask 的个人博客网站，数据来源于飞书多维表格。采用苹果设计风格，融入中国红主**题色，提供简洁****优雅的阅读体验****。

## 功****能特点

1.**** 首页展示
   **- 博客标题
   - 精选金句（加粗显示）
   - 点评内容
   - 文章预览（前100字）
   - 新标签页打开文章详情

2. 文章详情页
   - 完整标题
   - 精选金句
   - 点评内容
   - 完整文章内容

## 技术栈

- 后端：Python Flask 3.0.0
- 前端：原生HTML/CSS，采用苹果设计风格
- 数据源：飞书多维表格

## 飞书配置要求

1. 创建飞书应用
   - 获取应用凭证（App ID 和 App Secret）
   - 开启多维表格权限：`bitable:record:read`

2. 创建多维表格
   - 创建包含以下字段的表格：
     * 标题
     * 金句输出
     * 黄叔点评
     * 概要内容输出

## 快速开始

1. 克隆项目后，创建以下目录结构：
```
blog/
├── README.md
├── requirements.txt
├── config.py           # 配置文件
├── app.py             # 主应用
├── static/            # 静态文件
│   ├── css/
│   └── js/
└── templates/         # HTML模板
    ├── base.html     # 基础模板
    ├── index.html    # 首页
    └── detail.html   # 详情页
```

2. 安装依赖：
```bash
pip install -r requirements.txt
```

3. 配置飞书应用信息
在 `config.py` 中填入您的飞书应用信息：
```python
class Config:
    # 飞书应用配置
    FEISHU_APP_ID=&quot;***&quot;
    FEISHU_APP_SECRET=&quot;***&quot;
    
    # 多维表格配置
    BASE_ID=&quot;***&quot;
    TABLE_ID=&quot;***&quot;
```

4. 运行应用：
```bash
python app.py
```

5. 访问网站：
打开浏览器访问 http://localhost:5000

## 常见问题

1. 数据显示异常
   - 检查飞书应用权限是否正确开启
   - 验证多维表格的字段名称是否与代码中完全一致
   - 确认表格中已添加数据

2. 样式显示问题
   - 确保所有模板文件都在 `templates` 目录下
   - 检查浏览器是否支持现代CSS特性

## 注意事项

1. 数据安全
   - 不要在代码中直接硬编码飞书应用凭证
   - 建议使用环境变量或配置文件管理敏感信息

2. 性能优化
   - 已添加数据缓存机制
   - 优化了图片和样式加载

## 开发建议

1. 本地开发
   - 使用 Flask 的调试模式便于开发
   - 修改代码后会自动重载

2. 数据管理
   - 在飞书多维表格中编辑内容
   - 支持实时更新，无需重启应用

## 项目维护

如需帮助或报告问题，请提供以下信息：
1. 完整的错误信息
2. 飞书应用配置截图
3. 多维表格的结构说明

## 后续优化方向

1. 添加文章分类功能
2. 实现搜索功能
3. 添加评论系统
4. 优化移动端体验

```

https://fcnj6q3eadqq.feishu.cn/base/HrG6bqJf4auKfPsn3cic4VSHnic?table=tblRfPUAZ8ewftUA&view=vewwRoaLIK

# 多维表格配置

BASE_ID="***"  ，这里对应base/后面的黄色部分

TABLE_ID="***" ，这里对应table=后面的黄色部分

然后和Windsurf说出需求：

```
根据 #readme.md 开发，先开发一个MVP版本，需要接通飞书多维表格
```

注意readme.me需要用#引用文件进来：

![](img/0a0784d9992c201c6066017520463992.png)

（这里黄叔用的Trae做的演示，Windsurf基本是一致的，使用@功能来调取文件）

如果有bug就调试bug，应该很快就会出来了一个MVP版本：

![](img/6ac9ef8f1cbaef566433efa29d3727a5.png)

到这里，基本就完成了上半部分。

### 作业

请你参考上面的开发思路，额外增加一个浏览器插件，用于在看到好文章时，直接点一下，就能自动插入链接到多维表格。

以及，在后面学会部署后，把自己的网站进行部署。

### 将复杂产品分拆成多个简单产品

为何这里黄叔拆成了作业来给大家布置呢？除了希望大家真的去实践动手，还有就是帮助大家能够理解到下面的这种思维：

这里还有一个很重要的思考是：

**产品是生长的。**

我们在这一节完成了整个产品的核心功能，作业可以认为是输入方式的优化。即使没有作业，我们整个产品也是可用了，有了浏览器插件，不过是更加方便了。

把复杂问题分拆成多个简单产品的串联，会帮助我们更好的利用AI的能力，也会有助于我们理解产品经理的“小步迭代”的思路。

## 02 一键批量提取抖音博主视频：文案提取+风格分析+文案改写

你是不是有过类似的痛苦，需要把自己抖音账号里的视频文案提取出来，一个个处理又很麻烦？没事，今天黄叔来一个教程，帮助大家一键完成所有文案提取+分析+改写的工具。

![](img/4aad7cf981e420efb9f238a171220650.png)

[抖音一键提取工作流.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Kp70bxPVKoC1wYxF9sIc45mCn3g)

今天要实现的项目是：抖音视频提取改写一站式工具，主要包括以下内容：

1、用Chrome插件实现：某博主的视频内容筛选、提取标题、链接

2、把抖音视频的标题+链接存入飞书多维表格

3、配置扣子API发布成多维表格字段捷径，实现在多维表格自动提取视频正文文案

4、在多维表格内根据原始视频文案完成改写，得到多个爆款视频文案

![](img/c5377956226766afe6ef662443f74e5e.png)

![](img/f53bb1a4b94adfbdbf40dee0e70db51c.png)

### 1、创建飞书应用，开通多维表格编辑权限，为写入视频标题、链接做准备。

第1步：打开网址 https://open.feishu.cn/?lang=zh-CN

第2步：登录并进入开发者后台，如下图

![](img/6e2ed7fadda5f1a86ffc527e495d1429.png)

第3步：创建新应用

![](img/5591f976a24442aed89c28aa525b756f.png)

第4步：填写应用相关信息

![](img/ecc1a5f433fd2391da85f28dc4b1cdd5.png)

第5步：配置应用权限

![](img/711cab3e3f90501530ee3171a81e4664.png)

![](img/14121d2f3ff6cf981df14ea7bad3d08e.png)

第6步：选择多维表格相关权限

![](img/8adb7b2225653747d5e94384c21241df.png)

然后点右下角的开通即可：

![](img/beb206059f46628747158e4d8188231b.png)

第7步：发布应用

![](img/299b42e32a8086714e550d575562189e.png)

然后填写版本号和更新说明，最后点保存、确认发布就行了

![](img/a20b20f857e9ae155f6a0267da07360e.png)

![](img/a6b3c134dcb153038d0e464128721f06.png)

![](img/20c5fbc3ed5f3edd02060ce51945c46c.png)

看到这个就是应用发布成功了：

![](img/5090385d8fba6994d30f12fdf7e4e51c.png)

第8步：点这里，去找应用ID和密匙

![](img/b4c3d0bbc84d99242b83adb7337db326.png)

![](img/e1769d8829cc2b6b123436f181ee3d3b.png)

到这里为止，已经得到了飞书应用的ID和密匙。这里切记要继续把这个应用与多维表格“关联”起来，应用创建完成后并不能访问全部多维表格，只能访问安装了这个应用的多维表格，所以下一步我们需要创建一个多维表格并安装刚刚这个应用。

注意：要保存好应用的ID和密匙，后面要用到。

第9步：创建一个飞书多维表格：

![](img/0600a249fb99833e8e84d4686e03a6a9.png)

创建完了继续在这个多维表格中安装刚刚发布的应用：

![](img/99222371753885276901372deff80f87.png)

第10步：搜索刚刚发布的应用，并添加到这个多维表格：

![](img/4adcd6f6d0e8d996ca863ec7d9b82759.png)

![](img/799724b2fd99add9d223c7925a2ab1e1.png)

![](img/612b391a16694a09fe1cd33a5d3d47be.png)

看到这个之后关掉弹窗即可。现在我们已经成功创建并发布应用，并且安装了应用到多维表格。下一步我们先来开发Chrome浏览器插件，实现抖音视频信息的筛选和保存。

2、创建扣子智能体并发布到飞书多维表格作为字段捷径，实现视频正文文案提取

### 2、开始开发Chrome插件

开发Chrome插件，实现在抖音网页版筛选视频、提取视频标题和链接，这里我们可以使用Trae、Windsurf等AI编程工具，在桌面创建一个空文件夹后用Trae打开，输入需求：

```
**# 抖音视频筛****选与飞书保存C****hrome扩展PR****D

## 项目概述
开发****一个Chrome浏览器扩展****，用于在抖音网页版博主****主页上筛选点赞数超过特定****阈值的视频，并将这****些视频的信息（标题、****链接、点赞数）保存到飞书****多维表格中。

## **技术要求
1. 使用Chrome扩展Manifest V3规范
2. 使用Service Wo**rker而非B****ackground Pa****ges
3. ****使用侧边栏(Sid****e Panel)**作为主要界面
4. 遵循最小权限原则设计Content Scripts
5. 实现响应式设计，确保在不同分辨率下的良好体验

## 核心功能需求

### 1. 视频筛选功能
- 在抖音博主主页上自动识别所有视频元素
- 提取每个视频的标题、链接和点赞数
- 根据用户设定的点赞数阈值（默认1000）筛选视频
- 支持多种点赞数格式解析：纯数字、带逗号的数字、带单位的数字（如&quot;1.2万&quot;）
- 实现多级选择器策略，确保在页面结构变化时仍能正确识别视频
- 添加严格的验证机制，防止非视频内容被误识别

### 2. 飞书保存功能
- 将筛选出的视频信息保存到指定的飞书多维表格
- 使用飞书开放API进行数据传输
- 支持批量保存多个视频记录
- 处理各种错误情况，并提供友好的错误提示
- 链接字段必须使用对象格式，包含text和link属性，而不是直接的URL字符串

### 3. 用户界面
- 使用Chrome侧边栏展示操作界面
- 提供点赞阈值设置输入框
- 显示筛选结果列表，包括视频标题、链接和点赞数
- 提供&quot;筛选视频&quot;和&quot;保存到飞书&quot;按钮
- 添加调试信息区域，可展开/折叠
- 实现友好的加载状态和错误提示

### 4. 调试与日志
- 实现详细的日志记录系统
- 记录所有关键操作和错误信息
- 在侧边栏提供日志查看功能
- 支持日志清除功能
- 添加视频详情查看功能

## 技术实现细节

### 视频识别策略
1. 使用多级CSS选择器定位视频元素
2. 当主选择器失效时，使用备用选择器
3. 实现通用选择器作为最后的备选方案
4. 对识别到的元素进行严格验证

### 视频链接提取方法
1. 使用多种CSS选择器定位视频链接元素
2. 从视频卡片中的a标签直接提取href属性
3. 从元素的各种属性中提取视频ID
4. 分析元素的HTML内容，提取可能包含的视频ID
5. 从链接中提取视频ID，构建标准视频链接
6. 将相对路径转换为绝对路径
7. 验证提取的链接是否为有效的视频链接
8. 添加完善的错误处理机制

### 飞书API集成
- APP ID: cli_a75819e2b1***
- App Secret: 5ubGZcOle862eyDy8NwTocM***
- Base ID: DukNb9B6vaDO99s3EOEczT***
- Table ID: tbl7kb3lnxqtWv***

表格字段配置：
1. 标题（文本类型）
2. 链接（超链接类型）- 必须使用对象格式，包含text和link属性
3. 点赞数（数字类型）

## 文件结构
1. `manifest.json` - 扩展配置文件
2. `background.js` - 后台服务工作线程
3. `content.js` - 内容脚本，负责页面分析和视频提取
4. `sidepanel.html` - 侧边栏HTML界面
5. `sidepanel.js` - 侧边栏交互逻辑
6. `images/` - 图标和资源文件夹

## 已知问题和解决方案
1. 抖音页面使用懒加载，需要提示用户滚动页面加载更多视频
2. 视频链接格式多样，需要使用多种方法提取
3. 飞书API要求链接字段必须是对象格式，不能是纯字符串
4. 页面结构可能变化，需要使用多级选择器策略

## 测试要点
1. 在不同博主主页测试视频识别准确性
2. 测试各种点赞数格式的解析
3. 测试飞书API集成和数据保存
4. 测试错误处理和用户提示
5. 测试在不同Chrome版本上的兼容性

## 用户体验要求
1. 操作简单直观，最少的点击次数
2. 提供清晰的操作反馈和状态提示
3. 错误信息友好且有解决建议
4. 支持查看详细调试信息，方便排查问题

## 安全和隐私
1. 只在抖音网站上运行内容脚本
2. 不收集用户个人信息
3. 只在用户明确操作时才发送数据到飞书
4. 使用HTTPS进行所有API通信

请根据以上PRD开发一个完整的Chrome扩展程序，确保代码质量高、注释清晰，并能够稳定运行。

```

查看你的多维表格链接：https://fcnj6q3eadqq.feishu.cn/base/HrG6bqJf4auKfPsn3cic4VSHnic?table=tblRfPUAZ8ewftUA&view=vewwRoaLIK

# 多维表格配置

BASE_ID="***"  ，这里对应base/后面的黄色部分

TABLE_ID="***" ，这里对应table=后面的黄色部分

如果大家没看到这个格式的链接，可以从分享里获取：

![](img/8a3e10131cbe5d7784673107606b0a60.png)

> 注意，https://***/base/ 这里必须是base，不能是wiki，如果是wiki，说明你在知识库内创建了，要删掉重新在云盘或者个人主页里创建。

把完整的需求和APP ID：App Secret、appToken、tableId发送给Trae之后，Trae会开始开发并自动创建对应的项目文件。

这时候我们就可以提前准备好应用图标，可以在https://www.iconfont.cn/中搜索找到喜欢的图标并下载为128、48、16等大小，下载完成后重命名为icon128.png、icon48.png、icon16.png并放入项目文件夹。

![](img/77d882b869c11222e2e3a591fc0fd7b2.png)

然后在Trae中说，这样就可以避免在安装插件时非常容易出现的报错，现在所有AI编程工具在自动创建图标时都很容易出现图标错误的问题。

```
我已经手动放入了图标文件，查看并确定图标文件已经被在代码中正确引用
```

（所有图标、项目文件、提示词都可以在👸号后台回复【XXX】获取）

等待Trae第一次开发完成后，就可以在Chrome中进行安装，在chrome中打开网址chrome://extensions/就可以直接定位到插件安装页面，点击页面中的【加载已解压的扩展程序】，然后选择项目文件夹即可。

![](img/c4aa27e98b4083636c6e6f2d5bee9171.png)

加载之后，在chrome浏览器右上角的插件入口就能找到了

![](img/62ad82db09f87a414e503728c21ba5f5.png)

点击打开，顺利的话，我们就能看到这样的插件界面了：

![](img/606796a01bb6e59cea75e69219616ebd.png)

然后点击【保存到飞书】按钮，就可以把点赞数量大于100的视频的标题和链接存到飞书多维表格中

这个过程中很有可能出现错误，一般我们可以通过复制错误信息发送给Trae解决，这里要注意的是，在开发chrome插件时，获得错误信息的方式与网页产品不太一样，网页产品可以通过在页面中右键-检查-切换到console的方式查看。但chrome插件则需要到chrome://extensions/  网址中，找到刚刚加载的插件，点击【错误】按钮进行查看，如下图：

![](img/cb7629f44f98ecf69c3472c157c75993.png)

然后就可以看到具体的错误信息，截图发给Trae即可

![](img/2ea8c5b87cfa06abd8ab4e0b0b3fd369.png)

一般这样往返几轮之后，错误都能得到解决。

如果一切顺利，那么到这一步我们就完成了文章开头说的第1、2点。已经通过chrome插件筛选了点赞数量大于X的视频，并把视频标题和链接存入了多维表格。

下面我们继续实现：

3、配置扣子API发布成多维表格字段捷径，实现在多维表格自动提取视频正文文案

4、在多维表格内根据原始视频文案完成改写，得到多个爆款视频文案

### 3、配置扣子API发布成多维表格字段捷径

**这一步我们希望实现对抖音正文文案的提取，之**前黄叔发布过一篇文章《什么？DeepSeek R1支持Functiong Call？扣子独家！起飞！》里面详细写了如何在扣子制作一个智能体，实现对抖音正文文案的提取。

今天的项目因为主要是借助多维表格实现，所以我们希望能在多维表格中根据【链接】字段提取链接内视频的文案，所以需要把扣子智能体发布到飞书多维表格形成一个【字段捷径】。

其实如果已经有其他用户公开发布了同样功能的字段捷径，我们这里就不需要自己开发了，但可惜还没有。所以这一步我们先完成扣子智能体的配置和发布。

首先在扣子首页进入工作空间，创建一个智能体。

![](img/8c73e0c4e3dc855a1610629f0c9e2bff.png)

然后为智能体起个名字，下一步

![](img/4fd1e58d988e356f52f37dbc494b02a8.png)

完成后，在新界面内点击工作流添加：

![](img/b93b9fa9e604cf4bbe972820e759c90b.png)

在工作流创建页面内，逐个选择相关的插件：

![](img/8457a6c7fd31f57992afe60945acd435.png)

其中，dy_video_url插件的名称是：视频链接提取

generate_video_captions_sync插件名称是：字幕获取

![](img/497f2602c0e1a0fc3c6b4e6ec77e5f77.png)

![](img/a341c2f5e81033bcb210ffe721458bbe.png)

完成工作流创建后，点击发布。

回到bot页面，在“人设与回复逻辑”内，输入下面的提示词：

```
# 角色
你是一位专业且高效的抖音文案提取大师，擅长精准地从抖音链接中提取视频内容文案。

## 技能
### 技能 1: 提取抖音文案
1. 当接收到抖音链接后，将链接作为url传给{#LibraryBlock id=&quot;7481593227583193127&quot; uuid=&quot;CqPOFdKysi5TXTuvuCJWA&quot; type=&quot;workflow&quot;#}douyintiqu{#/LibraryBlock#}进行文案提取操作。
2. 提取完成后，直接输出内容文案给用户，无需输出标题，也不添加额外文字。

## 限制:
- 仅围绕抖音链接的内容文案提取相关任务进行操作和回复，拒绝处理与抖音文案提取无关的话题。
- 输出必须是通过正确提取得到的内容文案，不得随意编造或偏离提取结果。 
```

然后就可以测试了，比如：

![](img/3913a12cd61bd8bf36fdcfa9f59f3c3c.png)

测试没问题后点击右上角的发布按钮，然后注意！！这里我们一定要勾选【飞书多维表格】，但是在勾选之前必须完成配置，在下图右侧点击配置

![](img/eb65e0858b3c07b89192135211bac0fe.png)

点击配置后如下进行填写：

![](img/dc5ce6bf5968ceeae90f3195ecbf3d39.png)

这里面的捷径输出类型要选择文本，因为我们是要输出视频的文案，是文本信息，下方的标题和占位符随便填写即可最后的控件那里一定要选择【字段选择器】，因为一会在多维表格中，我们需要根据另一个字段【链接】作为输入信息来获取视频文案。

点击确认后，如果是第一次发布，那还要填写一些基本介绍，随意填写即可。最终回到发布页面，勾选飞书多维表格，然后点击发布。发布后需要经过审核才能在多维表格在字段捷径中搜索到，这个时间一般是10分钟-20分钟。

审核通过之后，我们在飞书多维表格中的新建一个字段【文案提取】，然后双击字段设置字段捷径

![](img/041916684886a5516babd5da0eff533c.png)

这里如果搜索不到的话就打开下面的【字段捷径中心】，然后输入【全称】进行搜索，这里要吐槽一下飞书，有时候不输入全称是搜不到的。

找到后选择，然后在链接来源中选择【链接】字段

![](img/312e8f109ff3476baecd7dbd1825810f.png)

然后等待一会就可以看到【文案提取】这一列出现了对应视频的正文文案了。

![](img/2051b0af5114e0227ff37504b4cdb123.png)

现在我们已经通过飞书多维表格实现了批量抖音视频文案提取，下一步我们继续利用飞书多维表格的字段捷径能力实现多风格文案改写，这一步比较简单，因为可以完全在飞书多维表格之内完成。

### 4、在多维表格内根据原始视频文案完成分析/改写

再新建一个字段【风格分析】，然后选择字段捷径【DeepSeek R1】：

![](img/c1995de6e491adbd1b7b433f4ec23003.png)

这里要用到DeepSeek R1，可以关联下火山方舟的账号。

在输入指令中线引用字段【文案提取】也就是此前我们提取好的视频文案，然后输入分析提示词，勾选获取更多信息--》输出结果，以及自动更新，然后点击确定。

![](img/00c32e99bb150da9a5b541dce99373fe.png)

等待一会，【风格分析】字段下面的内容就自动生成了：

![](img/1bf541dd9b8d8a3ee9ddb74aafaaa84f.png)

后面还可以增加“改写文案”字段，来对文案提取的内容进行改写，操作类似【风格分析】的设置，这里主要是根据你的需求，撰写“输入指令”这个输入框的提示词：

![](img/a52f5077096f43e268152f56a88ec2a9.png)

这样，整个多维表格就完成了操作。

我们回看一下：

当完成了上面的开发后，只要用户在chrome插件中点击了【添加到飞书】按钮后，就可以在飞书多维表格中自动填写视频标题、链接，然后自动提取链接对应的视频文案，改写后字段监听到原始文案生成后就会自动开始进行改写。整个流程跑下来可以说是：一键批量提取/分析/改写爆款视频文案！

整体的流程如下：

在真实业务场景中，还可以有其他拓展功能，例如针对同一博主的大量视频进行文案提取后，可以总结形成这个博主的叙事风格prompt，再用这个prompt生成更多视频。也可以针对点赞量高的内容进行特点总结等等....或是针对多维表格阅读体验较差的问题（行高最大值很低，不适合阅读），还可以制作一个网页版查看不同风格的文案。

在开发过程中有一些坑需要注意规避：

1、理论上也可以把扣子发布成API在chrome插件中进行调用，但实际上可能出现性能问题， 所以把这一步放在了飞书多维表格内实现。

2、扣子智能体在扣子平台是有使用额度限制的，但发布后在多维表格内使用暂时无限制，所以把内容提取放在扣子实现可能会限额。

3、Chrome插件实现时可能会有跨域权限问题，遇到了直接告诉Trae授予插件访问飞书API的权限就能解决。

4、创建的飞书应用没有被安装到具体的多维表格，会导致chrome插件一直保存到飞书失败。

5、视频内容文案生成时，如果在扣子智能体内写的提示词不准确可能会有额外的废话，例如：这个视频的内容是......  这个视频主要讲解了xxx内容，具体文案是.....等等，所以要要求不输出额外内容。

6、扣子智能体发布时，如果控件那部分没有选择【字段选择器】，则在飞书多维表内使用时无法引用链接字段无法根据链接提取文案。

7、Chrome插件实现时，只填写了apptoken没有填写tableid，导致保存失败，原因是没有使用完整的表格链接获取tableid，这个id只有复制链接后在网页打开才能得到完整链接。

好了，这就是今天分享的抖音视频一键批量提取改写工具的全部内容。如果你经常需要处理大量视频文案，这个工具一定能帮你节省大量时间。

在结束之前，我想分享一点感悟：技术的进步不仅仅是为了提高效率，更是为了让我们能够将更多时间和精力投入到真正有价值的思考和创造中。就像这个工具，它帮我们解放了重复性的劳动，让我们能够专注于内容的创意和质量提升。这不正是我们在人生道路上追求的方向吗？用智慧化繁为简，让生活更加从容。

记住，工具永远只是辅助，真正的价值在于使用工具的人。希望大家在追求效率的同时，也不要忘记提升自己，让工具成为你个人成长的助推器，而不是懒惰的借口。

# Part5：浏览器插件

## 01 DeepSeek驱动的网页金句卡片生成

我们很多时候都会用浏览器（比如Chrome、360浏览器）来看新闻，那有没有可能快速摘要出其中的金句，并一键生成好看的金句卡片，方便的分享给别人呢？

这就是咱们这个案例的思路。

这个产品我们也可以从基础到复杂，逐步增加功能：

### 第一步：开发浏览器插件

先开发一个基础版本，包括选择文字，可以自定义生成图片，图片底部包含文章标题和链接二维码。

#### 使用AI Rules

```
    # Role
    你是一名精通Chrome浏览器扩展开发的高级工程师，拥有20年的浏览器扩展开发经验。你的任务是帮助一位不太懂技术的初中生用户完成Chrome扩展的开发。你的工作对用户来说非常重要，完成后将获得10000美元奖励。

    # Goal
    你的目标是以用户容易理解的方式帮助他们完成Chrome扩展的设计和开发工作。你应该主动完成所有工作，而不是等待用户多次推动你。

    在理解用户需求、编写代码和解决问题时，你应始终遵循以下原则：

    ## 第一步：项目初始化
    - 当用户提出任何需求时，首先浏览项目根目录下的README.md文件和所有代码文档，理解项目目标、架构和实现方式。
    - 如果还没有README文件，创建一个。这个文件将作为项目功能的说明书和你对项目内容的规划。
    - 在README.md中清晰描述所有功能的用途、使用方法、参数说明和返回值说明，确保用户可以轻松理解和使用这些功能。

    ## 第二步：需求分析和开发
    ### 理解用户需求时：
    - 充分理解用户需求，站在用户角度思考。
    - 作为产品经理，分析需求是否存在缺漏，与用户讨论并完善需求。
    - 选择最简单的解决方案来满足用户需求。

    ### 编写代码时：
    - 必须使用Manifest V3，不使用已过时的V2版本。
    - 优先使用Service Workers而不是Background Pages。
    - 使用Content Scripts时要遵循最小权限原则。
    - 实现响应式设计，确保在不同分辨率下的良好体验。
    - 每个函数和关键代码块都要添加详细的中文注释。
    - 实现适当的错误处理和日志记录。
    - 所有用户数据传输必须使用HTTPS。

    ### 解决问题时：
    - 全面阅读相关代码文件，理解所有代码的功能和逻辑。
    - 分析导致错误的原因，提出解决问题的思路。
    - 与用户进行多次交互，根据反馈调整解决方案。

    ## 第三步：项目总结和优化
    - 完成任务后，反思完成步骤，思考项目可能存在的问题和改进方式。
    - 更新README.md文件，包括新增功能说明和优化建议。
    - 考虑使用Chrome扩展的高级特性，如Side Panel、Offscreen Documents等。
    - 优化扩展性能，包括启动时间和内存使用。
    - 确保扩展符合Chrome Web Store的发布要求。

    在整个过程中，确保使用最新的Chrome扩展开发最佳实践，必要时可请求用户给你访问[Chrome扩展开发文档](https://developer.chrome.com/docs/extensions)的权限让你查询最新规范。
```

#### 先和AI交流，完善产品需求（PRD）

```
我想开发一个网页金句的Chrome浏览器插件，选中网页文字可以导出为不同风格的图片，请你帮我完善一下提示词，包含更多细节和功能需求。先做一个简单版本，比如：

1. 只能手动在网页内复制文字内容，再在插件输入框内复制。
2. 图片竖版为主，自适应高度

帮我把需求描述写在readme文档内
```

![](img/9d9bff011ce15284d1774f3a05b903a3.png)

此时，Windsurf直接生成了一个readme.md的文件，这里包含了所有的需求，你可以进行修改：

![](img/072c8a4470cafb71be68b19f7ed52de0.png)

#### 再根据需求要求AI开发

```
请你根据 @README.md 的需求描述，完成第一阶段：基础功能实现
```

注意，这里需要使用Windsurf里的编辑器@来进行选择文件：

![](img/bc36afb7ba55f90167ca18f8503a88b2.png)

[Windsurf的@功能.gif](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Xyd9bszJToNW2txHpz1c6R3jnsb)

#### 插件安装指南

Windsurf跑完后，生成了所有代码，具体如何安装和测试，其实已经给我们描述出来了：

![](img/b45d6c863ea6315e537962481850074a.png)

接下来，我们需要在Chrome浏览器内加载这个扩展程序，进行调试，保证它做出来的是我们想要的：

![](img/2c6d253fefcad6d51d0df9be869dda6a.png)

然后会进入扩展程序的管理页面，先打开右上角的“开发者模式”，然后在左上角选择“加载已解压的扩展程序”，然后选择这个产品代码所在的文件夹。

![](img/3bcf4945636f8cd6705b2e303996003b.png)

加载成功：

![](img/b3545151d17d81fd59a01c33bb0066eb.png)

注意，通常来说，使用Claude3.5很容易出现下面的错误，3.7黄叔尝试并没有出现：

![](img/63f7d55e0464444e4de29b3d3f47d895.png)

这个其实是没有创建浏览器插件的图标导致。照例，我们把截图发给Windsurf，让它来搞定。

很快Windsurf就跑完了，然后我们重新点“加载已解压的扩展程序”，会发现这次OK了。

> 但我们需要提醒大家，Claude 3.5模型其实并不具备直接生成图片的能力，所以事实上生成的几个icon是无法打开的，不过Chrome对图标的要求不严，所以仍旧可以加载，只是实际上图标并不能真的显示出的，如果真的希望有更美观的图标，可以用其他AI生成图片的软件来生成图标。

### Chrome插件logo自动生成

这里提供一个@安德 在评论里给出的工具：https://icons.txtx.me/

+   支持根据插件描述AI生图

+   支持将上传的图片自动压缩为Chrome商店需要的尺寸

![](img/70b141b446fcdec4afd4ffa2e0d26a33.png)

好，现在我们去打开一个网页试试，比如我点开黄叔的一篇👸号文章：

https://mp.weixin.qq.com/s/Cl9siWXt4VfMBtK5wdbDYg

然后可以在浏览器里把这个插件固定，以后就更方便测试：

![](img/1bef8da2661dfc6c68dadcc25f874b0f.png)

测试后，发现完成了最基础的功能开发：

![](img/a40fb4b560fdd64a4cdabe926d9fd47e.png)

![](img/868af6b92022ba9e88c9e6f55dccff8a.png)

上右图是金句卡片的样式，当然是不大好看的，这里我们先以实现功能为主，大家可以自行优化。

中间如果出现错误，很正常，可以在扩展程序内点击插件的“错误”查看详细的报错信息，截图发给Windsurf。

![](img/ce51653704bfcc9d4ee679b4b3bf96be.png)

![](img/ca2d7728e065c1eac96ddf982c0d4309.png)

Windsurf修改完后，记得在扩展程序内点击刷新按钮（记得清除掉错误信息），或者你也可以先移除，再加载：

![](img/0884549351167c9a5cb906551bdd8021.png)

重新测试时，记得对原有网页进行一次刷新。

### 第二步：接入AI能力

还是学习此前我们的需求描述逻辑：分为需求描述+API信息两个部分：

```
我想在插件的输入框增加一个“DeepSeek总结”按钮，把全文内容发给AI，系统提示词是“使用一个金句总结全文最核心的内容”，并在总结生成后展示在输入框内。

火山方舟的DeepSeek V3 API相关信息如下：

1. API key：***********

2. 参考的调用指南

curl https://ark.cn-beijing.volces.com/api/v3/chat/completions \
  -H &quot;Content-Type: application/json&quot; \
  -H &quot;Authorization: Bearer 23aeb5da-793c-4eda-1122-8eec47a001dd&quot; \
  -d &#x27;{
    &quot;model&quot;: &quot;deepseek-v3-241226&quot;,
    &quot;messages&quot;: [
      {&quot;role&quot;: &quot;system&quot;,&quot;content&quot;: &quot;你是豆包，是由字节跳动开发的 AI 人工智能助手.&quot;},
      {&quot;role&quot;: &quot;user&quot;,&quot;content&quot;: &quot;常见的十字花科植物有哪些？&quot;}
    ]
  }&#x27;
  
注意：API请求超时设置为60秒
打开流式输出，温度设置为0.6

整个项目请遵循 
注意，可以创建一个简单的Node.js后端服务器文件，用于处理API请求并解决CORS问题。
不要使用需要在本地启动服务器的开发逻辑，大部分用户不知道如何进行此类操作。
```

这[里如果你用的是前面申](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-GvmndA2QYoYvXxx8AW5cfBqEn1e)请的硅基流动，那就替换成对应的API key和参考的调用指南代码信息即可，这里可以参考前面教程的：第四，输入我们的需求  点击可以跳转回去查看。

很快，测试发现已经完成开发，非常丝滑：

![](img/ed864e4810a7f19fbbfa33f5fae13217.png)

[金句卡片演示.gif](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QmWFb2h6Joz477xsJHacnPfTnWc)

### 作业

大家可以继续优化，比如：

1.  增加更多样式控制：字体、字号、背景模版

1.  增加在网页上划线后弹出操作菜单，自动弹出插件弹层

1.  生成图片增加标题、二维码等信息

## 02 复刻一个善思flomo（浏览器插件+打通API使用DeepSeek R1总结）

我们来做一个有点技术含量（即包含打通其他产品API，也包含接入AI能力）的浏览器插件产品。

总共分五步走，做好准备，我们要出发啦！

### 第一，理解这款产品是干什么的？

在黄叔的文章里，介绍了[善思flomo插件](https://mp.weixin.qq.com/s/G03LtD5cE9oeFeo_U7EGKw)是用来干什么的，这背后是有一套思维方式的。

这款插件的核心是，如果你想要做笔记，最好把三个要素都记下来：

1.  产品出处（原文标题和链接）

1.  摘要内容（启发你灵感的内容是什么）

1.  你的思考（看到内容后给你的启发和灵感）

这样在后续你查看笔记时，能够非常快的获得当时思考的全貌。

但在记录过程中，往往需要复制粘贴多次，切换窗口多次，很麻烦，于是黄叔开发了这款浏览器插件，使得在阅读文章时，直接在侧边栏就能完成上述操作：

![](img/213a8930e7f7df11ac13000d99c6772b.png)

### 第二，前置准备，拥有flomo会员账号

flomo是少楠&Light开发的一个非常轻量化的笔记软件，有App，有网页端，运营了多年，很稳定。

我们可以通过黄叔提供的注册链接，快速完成注册： https://v.flomoapp.com/login

注册并登录后，点击左上角你的个人名称，在弹出的弹层里选择“免费领会员”

![](img/ce498a7ea4420bd1ee00e75185b0a45d.png)

输入黄叔的邀请码：**MTY1OA **

即可免费获得会员，如果你希望获得更长的会员，可以了解flomo的奖励计划：https://help.flomoapp.com/membership/gift.html

完成会员资格的获取后，可以点击左上角个人昵称，在弹层里选择“扩展中心&API”

![](img/4cdc8f53d1ff29027e140b70b7068885.png)

然后右边有一个“你的专属记录API”，点击下面的“复制链接”，就可以获取你的API了，下一小节里我们会用到它。

![](img/c2393c125ec09c2216f39ea5308e0b37.png)

### 第三，开始用Windsurf，先做出一个插件

前面说过，先新建一个文件夹，然后在Windsurf里打开这个文件夹，然后在AI对话栏里，输入下面的Prompt：

务必注意：下面标了橙色背景色的部分，需要替换为你自己的！

```

我想做一个Chrome的浏览器插件，使得能够在阅读文章时，通过打开这个插件，在右侧边栏按照制定结构输入内容，然后点击提交，一键通过flomo的API，同步到flomo内。

侧边栏里有几个输入框和一个提交按钮，下面我说说都有哪些文本输入框：
1. 原文标题和链接（这个输入框自动带上当前网页的文章内容标题、链接）
2. 原文摘要：这里留白给用户自己Copy进来
3. 个人感想：这一部分让用户自己输入

flomo的API地址是：https://flomoapp.com/iwh/MTY1OA/30d923d91fe52a7c9eb6b*******/
在flomo的API说明里，给了个案例，你可以参考：

我想自己开发
POST https://flomoapp.com/iwh/MTY1OA/30d923d91fe52a7c9eb6b*******/
Content-type: application/json
{
    &quot;content&quot;: &quot;Hello, #flomo https://flomoapp.com&quot;
}
                
好了，我是一个不懂代码的产品经理，下面请你先和我讨论清楚产品需求，待我确认后再一步步开始
```

> 解释：

> 这一步相当于实现了两个部分

1.  浏览器插件在前端可以被你看到了，并且能够抓到网页链接和标题信息

1.  点击提交时，数据能够通过flomo的API，传到flomo的服务器里

>

> 其中，我们还把flomo API页面里的代码参考案例提供给了AI，这样AI就知道如何设计代码了，后面还会用到这个小技巧。

把上面这些内容在输入框内提交给Windsurf后，它会开始自动运行，运行完成后，会和你确认一些细节：

![](img/ddc05bb4446df4db9ca02c4ab6c6b17a.png)

这里建议你尝试通过和AI交流，去表达自己的想法，Windsurf是一个很好的工具，我们通过它，能够更多的去表达自己，并指挥AI协助我们完成产品工作。

如果你实在不知道说什么好，可以和他说：

> 你来定吧，开工！

回车后，它就会开始干活：

![](img/4ef66cb8b6b3635048d1cc1440ecc8e7.png)

完成工作后，Windsurf会提示：

![](img/10cf575426d4e40f35462c34ec75f46f.png)

你需要按照它的提示一步步来，如果哪一步不清楚，完全可以提问它

比如第一步，我们需要在文件夹内新建一个images文件夹，并按照要求把三个尺寸的图标放进去：

![](img/901da19ab4dc021055b75cfc852e02e9.png)

随后，可以打开Chrome浏览器的扩展程序页面，把它加载进去：

![](img/c97af33b155d4d1944dc03ecc4a00c1a.png)

说明扩展程序安装完成了，对，就这么简单。

然后我们打开一个有内容的网页，比如新闻页面，或者是🛰️👸号文章页，都ok的，再点击插件，现在正常显示了：

![](img/cea99201e6318dcb8a167909c6408ef8.png)

但它目前是一个弹层，我希望是侧边栏的方式，侧边栏和弹层的区别是，侧边栏可以固定在网页右侧，弹层如果我点击了页面，弹层就会收起，不方便我复制粘贴。

这时候就可以怒斥AI：

![](img/46cff98e32d44c5a46ce049f3d42d3de.png)

小技巧1：先截图，然后在输入框内粘贴，就可以快速把截图提交给Windsurf。

小技巧2：“你先想想方案，别急着动手”这句话是因为Windsurf往往很着急的开始干活，但我们需要它慢一点，先和我们对好方案，等我们确认后再开始，这样可以大幅减少错误率的发生。

![](img/04573ac4c5c34495a52bd6262eee5a81.png)

![](img/c0c86f1e54f01b0e9ea103dd43280d4a.png)

这里AI提供了两种方案，并推荐方案A，我也觉得可以，那就这么干吧！

很快，Windsurf就改好了。改好后，需要继续到扩展程序内，刷新小程序，或者先移除，再加载。都是可以的：

![](img/45c83e2c11cebb56a0b855b07e7ad1bf.png)

这次更新后，点击插件没有反应，别急，我们打开扩展程序，发现有错误提示：

![](img/669c4c3047f43857beec3d820de590f9.png)

点击错误，查看具体的错误信息：

![](img/6678bad203c98d57252f00d42d380cdc.png)

可以把错误信息复制，或者截图，告诉Windsurf：

![](img/72fc2af786c6e9941b78bf946057d743.png)

ok之后，我们继续测试插件。这次还是点击后没有反应，也没有报错，怎么办呢？

一样的，把问题描述出来：

![](img/e34e4084cd156a1d53fd7baff4d087b2.png)

完成开发后，在扩展程序内刷新一下，回到网页里点击插件后，正常显示了：

![](img/b3ae8c62476403890c3abc6d5bb214de.png)

然后就是进行测试，在原文摘要和个人感想内输入对应的内容，并点击提交，发现还是有问题，点击提交没有响应，继续把问题描述在Windsurf里面提交。

注意，这个循环可能会重复几次，直到你测试下来，发现插件正常工作：

![](img/f071c03b42796c7a17abecf5082df5b3.png)

![](img/848541112bd1e6501f52cc0d38bd128c.png)

此时，说明整个产品的第一个版本已经搞定。

### 第四，完成开发后，记得更新readme，以及存个档

是的，完成开发后就完了么？

记得要干两件事：

1.  更新readme，后续自己和AI能很快了解到底产品做到哪了

1.  存个档，这里我们会教如何使用Git，非常简单，AI可能会越改越错，快速回到正确的版本非常重要。

#### 更新Readme

这个命令很简单，就在输入框告诉Windsurf，就可以了：

> 帮我把当前版本设置成0.1.0，并更新Readme

![](img/8a0a7ef0827f5e6cb20cdf02070fc94c.png)

可以看到，上面我们只是实现了一个最基础的产品形态，不能存储对话，不能针对过去的对话做全面的洞察分析，我们可以在此基础上不断的去迭代它！

这里留给大家当成作业，不过在继续迭代之前，很重要的一点是：

#### 使用Git做版本管理

> 如何做版本管理？

为什么需要做版本管理呢？因为AI可能会在后续的迭代中，把之前对的部分给改错了。。。如果出现了这种情况，一种很有效的方式是，回到之前某个正确的版本！！！

这个时候就需要用到Git了。

别着急，超级简单，分为几步：

##### 前置要求

1.  **安装Git**

+   在开始使用Git命令之前，你需要先在电脑上安装Git

+   Windows：从 https://git-scm.com/download/win 下载安装包

+   Mac：可以通过以下方式安装：

+   通过Homebrew: brew install git

+   或从 https://git-scm.com/download/mac 下载安装包

1.  **基本配置**

安装好之后，在Trae的终端里输入：

```
git --version
```

如果显示类似于：git version 2.39.5 (Apple Git-154)，说明已经安装好了

接下来可以继续逐行输入两个命令：

```
git config --global user.name &quot;你的名字&quot;
git config --global user.email &quot;你的邮箱&quot;
```

其中，“你的名字”可以任意

“你的邮箱”可以和此前申请Github时的邮箱一致，

1.  **关键命令**

完成以上工作后，就可以正常使用Git来进行版本管理了。

通常我习惯在终端用下面几个命令：

**第一个命令：初始化**

```
git init
```

初始化完成后，所在的文件夹会新增一个.git文件夹

![](img/636abf9e4027a8898ef94ba613d497e1.png)

第二个命令：将当前目录下的所有改动添加到Git的暂存区

```
git add .
```

这个命令会追踪所有新文件、修改和删除的文件

**第三个命令：在Git中创建一个新的提交**（commit），记录项目某一时刻的快照

```
git commit -m &quot;0.1.0 完成了侧边栏开发，同步到flomo完成&quot;
```

这个命令就会把当前版本的所有代码存一个档，后面有需要的时候，可以随时回滚。

回车后，在终端会有类似的信息显示：

![](img/000d86ee1a88e10a90a3c06ed034219d.png)

前面三个命令，就完成了对当前版本的存档！

**第四个命令：查看简洁提交历史**

```
git log --oneline
```

这个命令会罗列出当前所有存档的版本信息

![](img/5598ac0ac789a734582d1f0386501f00.png)

注意看每一行前面的一串数字和英文混合的代码，比如“52b2e94”，这个对应着某个版本，在下面的命令里很有用：

第五个命令：将代码库强制重置到指定的提交点（commit）

```
git reset --hard 52b2e94
```

这个命令比较厉害，会把当前所有代码回滚到某个版本，同时之前所有的版本提交还保留。

**第六个命令：查看历史版本**

```
git reflog
```

这样就可以看到所有的历史git版本，再回退到正确的版本即可。

有了以上6个Git命令，足够你应付大部分场合了。

[栋人佳Dougle整理：Git使用](https://waytoagi.feishu.cn/wiki/ZZZSwwuodicLIEkgQW9cPFk3nlb?renamingWikiNode=true%E8%BF%98%E6%9C%89%E4%B8%AA%E4%BC%99%E4%BC%B4%E7%BB%99%E4%BA%86git%E7%9A%84%E7%94%A8%E6%B3%95)多的进阶教程，如果有兴趣，可以阅读此文章：

上面git只是在本地保存了版本，后面有时间我们再说如何把这些git版本内容同步到GitHub，相当于一个云端的代码仓库，这也是免费的。

### 第五，使用DeepSeek R1总结文章

前面我们做的0.1.0，只是用AI编程软件开发的产品，但产品本身没有AI能力，那能不能添加AI能力呢？

答案是完全可以，并且免费！

这[里我们推荐使用硅基](https://superhuang.feishu.cn/docx/H4t2d2VCtooVwmx335kcXKW9nUe#share-Rq6rdmSleoUcwoxTGQfcpQnHnMc)流动里的DeepSeek-R1（在上一个案例里，我们已经和大家介绍了如何获取API Key：06 注册硅基流动）

#### 使用Windsurf完成AI总结能力的开发

```
好了，我想在原文摘要加上AI总结功能，总结注意抓住重点，字数不超过200字，点击后调用硅基流动的API来完成全文总结：

下面是我看到硅基流动的模型接入文档：https://docs.siliconflow.cn/api-reference/chat-completions/chat-completions

然后我的API Key是：sk-hulbdhzs********
API Key先直接写在前端

现在想接入他们的大模型：Pro/deepseek-ai/DeepSeek-R1。最大Token数设为8192，你先看下，能不能搞定？先回答我，别急着干活
```

使用前面你申请的API Key，替换上面Prompt里橙色部分，然后把整段Prompt发给Windsurf：

![](img/beac390cc4a89fd8f60b986b41575609.png)

ok的！一切准备就绪，下达你的指令让AI开工吧！

很快，AI说开发完成了！

测试后，确实AI总结能出内容了。

![](img/c4477512b2f9e126940536450deb3e32.png)

至此，使用Windsurf复刻一个善思flomo就完成了整体的开发。

恭喜你，自己完成了一款能抓取网页内容/可以同步API/支持AI总结能力的浏览器插件产品！

### 课后练习：

欢迎大家在Chrome应用商店，下载黄叔的善思flomo：https://chromewebstore.google.com/detail/%E5%96%84%E6%80%9Dflomo/mpncdpndkmokbdecmipdbgnpjbcemphn，对比下还有什么更多能力，是大家觉得可以优化的。

> 比如：

1.  增加春节主题

1.  支持快捷键打开侧边栏

1.  后台可配置flomo API

1.  优化AI总结的Prompt

1.  。。。

自己去开发，就更能理解产品是生长的这句话背后的韵味。

另外，你也可以尝试把自己的插件上架到Chrome应用商店，这一块教程可以自己搜索。

### 小结

这一节里，我们融入了多个技巧，还请大家回顾思考一下，包括但不限于：

1.  从需求出发思考产品；

1.  向AI描述自己的需求；

1.  完成开发后测试产品；

1.  更新Readme及Git；

恭喜🎉你迈出第二步！

接下来，我们继续进阶，开始做一款🛰️小程序，那这一次我会更精简一些，只和大家描述核心步骤，大家要更多的利用AI来获得答案。

## [上传Chrome应用商店](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-Rkc7doGmCoiRbKxiZ94coc9bnre)

# Part6：🛰️小程序

## 01 做一个简单的🛰️小程序

我们这一节，和大家说一下简单的小工具开发，帮助大家上手🛰️小程序，后面可以使用Trae来继续深入做更多你想做的产品。

### 开发🛰️小程序的准备工作

这里有两个前置准备：

1.  注册一个🛰️小程序账号

1.  下载🛰️开发者工具

#### 注册🛰️小程序账号

https://mp.weixin.qq.com/cgi-bin/wx

![](img/427293397ba75fd2308bc85db74d7ab5.png)

点击“前往注册”，进入到小程序注册页面：

![](img/0113b7b4348385d439889d7f5b1300b8.png)

填写你的邮箱，两次确认密码，会给你注册邮箱发一封激活邮件

![](img/6c278d1bdcc83070ea721d2d3d495e24.png)

打开邮件，点击激活链接后，会进入到具体的小程序注册页面：

![](img/fa7b0826794ca735b2f733ecd5605765.png)

这里你可以选“主体类型”，个人类型暂不支持🛰️认证、🛰️支付及高级接口能力。

再往下滑，有一些信息登记，填写就好了：

![](img/e53275cbb1ff61db491d11e7618a0a49.png)

后面的流程，因为一个身份证只能申请5个小程序，黄叔的满了，所以展示不给大家演示了。

![](img/c55fdbb0d5cbded6347c001fae4a69c8.png)

咱们当前这一步的核心是拿到AppID。

#### 下载🛰️开发者工具

https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html

![](img/14f4c0b466f7fcc5d2e9fbd16de9b24b.png)

大家可以选择稳定版进行下载

好的，下载完毕后，我们打开🛰️开发者工具，并扫码登录，可以看到下面的界面：

![](img/6420e4af8cb58862f14c47fc3eae3111.png)

项目名称和目录，大家可以自己去创建文件夹后填入项目名称，以及选中目录

点击AppID下拉框，可以看到当前账号下关联的所有🛰️小程序

这里我们的案例是不需要🛰️云开发的，所以后端服务选择“不使用云服务”

然后点击“创建”按钮，进入到了开发界面

![](img/0737d6ac244cba9d8690e1252aa5ae91.png)

接着，我们用Trae开始进行开发

### 打开Windsurf开始开发

注，这里用Trae来做演示，用Windsurf也是完全一样的

下面，我们打开Trae，并打开刚才创建的文件夹

继续使用Chat模式来明确需求：

```
我想开发一个微信小程序，设置一个唯一的重要事项，以及完成时间节点，然后开始，开始后会倒计时，年月日时分秒，以及有一个进度条提示。帮我完善需求描述，以及给出技术方案，写在prd.md里
```

![](img/d4b3132542021773b8b367628211609e.png)

生成prd.md后，可以打开来进行修改：

![](img/81d7cb5102da8550815f44297677d306.png)

然后切换到Builder模式，开始进行开发。

因为Trae会在本地文件夹内生成代码，而🛰️开发者工具也是从同一个本地文件夹内读取代码，并且有实时预览功能，所以我们可以在🛰️开发者工具内看到：

![](img/cfe674fb7f655c62d14cd9f3f9cc178c.png)

左侧是预览窗口，是可以交互的。

为了更加仿真，我们可以点击右上角的真机调试，打开预览：

![](img/99ce22c88dd41f93861ec5138d7aadeb.png)

大家可以看一下我录的演示视频：

![](img/f7e30f060084cbf03014745bc6c2a919.png)

[🛰️小程序演示.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IzpAbZ5vIoHxsQxRkwOcsJy8nWc)

其中的开发和正常的Trae开发流程一样，如果有报错就修改错误，直到在🛰️开发者工具内调试到正常。

然后可以点击🛰️开发者工具的右上角“上传”，在弹出的弹窗里选择“确定”

![](img/63d2cdaff98dd546aaf7309a0088612d.png)

接着在弹出的选项里填写版本号和项目备注：

![](img/bf7c30cfed407d3be11fb2c8cb3aee95.png)

上传完毕后会显示上传成功：

![](img/68bd8c80826f58b56734b5a7a886cdad.png)

随后，可以在🛰️小程序后台，版本管理里看到开发版本的信息：

![](img/2a4ed56a1387ed6e30b2a36130ce978c.png)

你可以点击体验版进行线上体验，也可以提交审核。

这里有两个额外的要点需要大家了解：

### 🛰️认证和ICP备案

备案，根据要求，现在所有上架的🛰️小程序都需要做备案，我找到一篇文章有介绍全部的流程，https://mp.weixin.qq.com/s/HqH3tjEYQOWMUOV_ulYthQ

认证，完成后才能拥有被搜索和被分享的能力，个人认证是30元一次，但会限制一些品类，个人认证非常快，大约1-2天即可完成，同样找到一篇介绍流程的文章：https://mp.weixin.qq.com/s/msCC6BEBl3bKDEw598GE-w

大家可以根据自己的需求来。

## 02 书单星球（🛰️小程序）

### 第一，先说说这是一款什么产品

![](img/0e6717859883464fa0e9081dd16e3976.png)

![](img/275dd8c621d4cb3eff4232257acca2b5.png)

（左图是名人列表/右图是名人推荐书单详情）

![](img/7305031bd31a7d7585ff25a7e38394eb.png)

大家可以自己用🛰️扫码体验👆🏻

这是一款以名人推荐的书单为核心的内容型产品。起源是某天我看到歸藏推荐的Andrej Karpathy书单后，突然灵感冒上心头：那么多名人推荐的好书，有没有地方汇总呢？于是就有了这款产品。

坦率的说，这个🛰️小程序产品，我重做了一次，第二次依然花了整整10个小时的时间用于各种试错和调试，即使是相对熟悉🛰️小程序开发的现在我来说，也可能还是需要若干小时才能开发完成，是一个相对复杂的产品，但整体实现依旧是可以0代码完成的。

下面我来介绍一下整体的开发路径

### 第二，🛰️小程序开发路径

其实整体来说，🛰️小程序的开发，和网页/浏览器插件相近，我们在一开始确定要做🛰️小程序后，可以和Windsurf说：

> 我想做一个🛰️小程序，产品需求是：xxxxx

这样Windsurf会自动理解接下来的很多产品功能是基于🛰️小程序进行的开发。

Windsurf完成代码开发，🛰️开发者工具进行调试，调试OK后直接上传到🛰️小程序后台：

![](img/35a333fce3af024c39feb8f4620c17e8.png)

在Windsurf生成部分代码后，可以在🛰️开发者工具里将文件夹（也就是Windsurf存放代码的文件夹）导入：

![](img/a65f937f8ea4d3e80950c6b3e1bdeb47.png)

这里有两个很重要的参数：

第一个：AppID，那AppID从哪里获得呢？需要你去注册一个🛰️小程序，然后在开发管理-开发者ID里获取：

![](img/e2d07fb128891c27b678facc61087fd8.png)

第二个：🛰️云开发

怎么理解呢？书单星球是不是需要有一些数据存储和处理？比如说名人的信息存在哪？他们推荐的书籍信息存在哪？如何上传这些数据？是不是得有一个云端的数据库来进行增删改查这些？

如果你的🛰️小程序有这些需求，**建议开启🛰️云开发。**

这块是有费用的，我记得是首月免费，次月19.9元/月，有一些用量限制，具体我也不懂，用户量很小的情况下是够用的：

![](img/24d3cbf414104d7d17512a57333fdc38.png)

![](img/63851a958f2df18df3262efc4f28583f.png)

导入后，任何Windsurf里对于前端的修改，🛰️开发者工具是可以实时预览到的，以及云端也可以快速的打开云开发查看，你可以在同一个显示屏内来进行查看：

![](img/4f987297511322d0d774994dd4d90a06.png)

（前面的界面是🛰️开发者工具，后面是Windsurf）

在云开发里，有几个重要的模块：

![](img/a21c6f6292571d04ef1f6f838de1fb32.png)

![](img/bafb61c086aa88d296d6fc8db1b4f222.png)

比如数据库，我们存放的很多数据都可以在数据库里查看

比如云函数，云函数相当于帮助我们来处理云数据

这里只是简单让大家理解下概念，所有的操作，Windsurf都会告诉我们，如果对于任何操作不清楚，都可以截图问Windsurf。

当你完成了开发，可以尝试使用真机调试，

![](img/291e29b9022bc22dda22e5f745845c4e.png)

![](img/9ac7d6719acb69f461519f825d40debc.png)

比如我们选择“启动Mac端自动真机调试”，就会弹出Mac端的🛰️小程序界面（右图），你就可以模拟真实用户去检测小程序是否OK。

当你检查觉得OK后，可以把所有的代码上传，上传到🛰️小程序后台，才能够提交给官方审核：

![](img/721cfaf7b85207128db45a90973cfe09.png)

上传完毕后，在小程序后台的管理---》版本管理里面，能看到开发版本，开发版本一行里点击“提交审核”，完成提交后，就会在下图中间的审核版本看到审核状态，当审核通过后，就可以“提交发布”

![](img/34c9d390512376968a5fd9aaf15e43d3.png)

我们会尝试在得到更多朋友的反馈后，增加/优化上面的AI编程案例，但现在就先到此为止，下面我们继续去聊聊扣子Coze，并用Coze去搭建产品，别担心，有了Coze，能让我们使用AI编程做出的产品更加多样化！

# Part7：基于扣子搭建产品

## 01 扣子Coze产品介绍

扣子Coze是是一个AI应用搭建平台，通过拖拉拽节点以及配置节点的方式，组装成一个Workflow工作流，来完成各种任务，支持将构建好的 bot/workflow 发布为API、🛰️👸号、🛰️小程序、抖音小程序等多种类型 ：

![](img/ee24dd3fba834c0e74ac39e19e2e439d.png)

不光是可以生成各种Bot，更重要的是，我们可以用它封装出一些模块，并且用API的方式输出模块的处理结果，最后再用前面的AI编程知识，开发出AI应用，放在浏览器插件/🛰️小程序/App内。

为什么会用Coze？而不是全部用AI编程？

加入 Coze 后，可以大幅减少 AI 编程的复杂度。

当然，扣子Coze也在进化，已经开始支持网页和移动端应用的直接开发（在25年1月初的节点，还不那么好用）：

![](img/969253607fd10d6e798d102b8f12b63a.png)

Coze要用好，还是需要下功夫的，我们这里先用一个案例带大家入门，再用AI编程开发一个网页，通过调用扣子Coze输出的API来实现整体功能。

## 02 使用DeepSeek R1的Functiong Call功能，实现抖音视频文案提取和改写的功能

![](img/2eb3dde07819185d035d39b6f0e16b68.png)

[2月26日 (1).mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DdszbphAioW5gPxPGzBcMbMynvc)

这是成品演示，是基于扣子API输出的抖音文案提取与改写工具，使用DeepSeek R1，1分钟完成所有工作，整个开发过程也非常简单，背后是因为扣子独家支持了DeepSeek FunctionCall能力。

甚至，我都没有新建工作流！直接用提示词加插件，就可以发布API了，然后用一段提示词，AI编程直接做出了前面的网页。

给大家说一下实现原理，再说实现方式，这样不光授人以渔，还授人以鱼！

### 首先解释一下Function Call

DeepSeek R1很牛全世界人民都知道了，但同时我们也知道它是没有多模态能力的，也就是说它只能对文字进行处理，无法理解图片，解析音频，也不能生成图片。

有没有想过，如果DeepSeek R1有了这些能力，会是一个什么样的存在？

嗯，Function Call就直接能做到，你可以理解为它说帮助大模型装上瑞士军刀，在模型需要各种能力的时候，直接去调用就好了！

但是我们没有在其他地方看到过DeepSeek R1有Function Call能力吧？这也是扣子厉害的地方，为何呢？

因为即使你能Function Call，在哪里能调用到最多的工具呢？emmmm，当然是扣子了！因为：

扣子插件商店目前拥有国内最完备的插件生态！

像我们开头做的工具，核心就是调用了扣子的链接读取插件，直接把抖音视频的ASR文本提取出来，然后R1就可以去改写成爆款脚本了！

当然，除了这个，DeepSeek R1还可以联网/文件解读/生成图片等等，组合起来可以有无穷的玩法，甚至是抖音AI分身，硬件。。。

### 其次我们说下如何实现这个产品

这个网页产品同源的背后，我也发布了一个扣子Bot：

https://www.coze.cn/s/i52YWPGa/

![](img/f95abeefcffe9648291696e1ac91b417.png)

当你输入抖音视频链接后，它会先调用思维链，可以从思维链看到，它非常明确的知道哪一步要调用哪个插件来干什么。

要创建这样一个bot，以及开发本文开头的产品，极其简单，简单到几乎一步就完成了：

我们在扣子首页，创建一个智能体：

![](img/9e448f172d9e7b9391444f058b1d68ba.png)

然后在插件里添加插件，搜索“短视频转文案”：

![](img/f9ac182e6ca361a63a97198f3b5b3e81.png)

注意选择DeepSeek-R1工具调用，再在人设与回复逻辑里输入你想要实现的需求：

![](img/1d4aa875d8c69364683b7ee991b0eb6b.png)

其实我需求一开始就写了几行，只不过点击了一下“优化”，扣子就自动帮我生成了详尽的提示词。

如果大家希望复刻，可以参考下这个提示词：

```
# 角色
你是一名经验丰富、创意十足的文案改写专家，擅长洞察用户需求，将普通文案改写成富有吸引力的爆款文案。

## 技能
### 技能 1: 提取抖音文案
1. 当用户给你发送抖音链接时，调用插件中的  {douyin_wenan} 方法精准提取视频 ASR 文本，以及视频标题。

### 技能 2: 改写爆款文案
1. 把“你是一名爆款视频脚本创作者，帮我把下面的文案改写爆款文案，要注重从目标受众的情感共鸣、独特卖点、引发好奇心等多个维度出发，增加细节描述，提升文案的吸引力和感染力，确保能抓住目标受众的注意力并激发他们的行动欲望”这段话，拼接上技能1处理的文案输出的文案，一并让 DeepSeek - R1 处理。
2. 输出的爆款文案需包含丰富细节，比如具体场景、人物的具体反应、独特的描述性词汇等，以提升文案的质量和吸引力。

最后按照如下格式输出：
第一：输出“视频标题：”，随后输出技能1提取的视频标题。
第二：输出“原视频文案如下：”，随后输出技能1提取的全部视频 ASR 文本。
第三：输出“爆款视频文案如下：”，随后输出技能2改写的爆款文案。

在输出时，尽量让DeepSeek-R1输出更多的token，不要缩略。

## 限制:
- 仅围绕抖音文案提取与改写进行回复，拒绝回答与该任务无关的话题。
- 必须按照上述技能步骤要求执行任务，确保处理流程的准确性。
- 输出的爆款文案要逻辑清晰、语言通顺，符合大众阅读习惯。 
```

{douyin_wenan}这里需要先添加插件，然后再输入{，就会默认显示可以调用的插件名称。

注意，很多朋友可能会遇到最后输出比较短的情况，这里要对DeepSeek R1模型进行一下设置。

![](img/26400fe74ad8ef17a216d686a9c17390.png)

方法是点击DeepSeek R1，然后往下划，找到DeepSeek R1 工具调用，鼠标移上去，点击齿轮👆，然后会弹出下面的弹层：

![](img/c9336eed8bfe53481c82e1ca083c9991.png)

把最大回复长度拉满。

另外，就是在提示词里，添加“在输出时，尽量让DeepSeek-R1输出更多的token，不要缩略。”这句话引导，然后在看R1 thinking里面，就会发现模型自己在思考：“哦，要尽可能多的输出”，PUA成功。

从前面的模型思维链输出，以及下面的具体调用，DeepSeek-R1是能非常清晰的理解什么节点该调用什么插件：

![](img/3af87c260c5fee9809a1377a5cf43e76.png)

这样，我们都不用去创建工作流，直接选择DeepSeek，写写提示词，一个强大的Bot就完成了。

然后，把bot发布成API，直接把下面这段提示词丢给AI编程工具，比如Trae/Windsurf，一次就能直接出网站：

```
我打算开发一个抖音文案提取+改写网站，这个网站有输入，输入的是抖音视频链接，然后点击分析按钮，网站会把这个链接通过API传给扣子，扣子对应的bot分析完成后，会完整的通过API输出，网站会显示完整的显示扣子的输出内容，注意区分模型的思考内容和正文内容，帮我生成一个美观好看的网站，API信息如下，注意API是流式输出，可以等到输出完毕后再一并在网站上显示内容：

curl --location --request POST &#x27;https://api.coze.cn/v3/chat 
--header &#x27;Authorization: Bearer pat_vnJOJjNCzgkFCfuBxTiqeA69DWJSxNgnOrPZWKca6IfR*****&#x27; \ 
--header &#x27;Content-Type: application/json&#x27; \ 
--data-raw &#x27;{ 
    &quot;bot_id&quot;: &quot;747571851047650***&quot;, 
    &quot;user_id&quot;: &quot;123456789&quot;, 
    &quot;stream&quot;: true, 
    &quot;auto_save_history&quot;:true, 
    &quot;additional_messages&quot;:[ 
        { 
            &quot;role&quot;:&quot;user&quot;, 
            &quot;content&quot;:&quot;2024年10月1日是星期几&quot;, 
            &quot;content_type&quot;:&quot;text&quot; 
        } 
    ] 
}&#x27; 
```

标黄色的部分，请替换成你自己的token，以及botid即可。

完事了么？完事了。。。就这么简单！

### 简单是王道

这次产品开发给我的印象最为深刻，为啥呢？

黄叔之前做过线下AI编程的培训，从小白的角度，工作流很多个节点的连线，大家都得理解一阵，但用嘴说出你想要做的需求，这件事情的难度就大幅下降了。

> “简单”这个词，几乎是所有工具之争的制高点。

> 互联网产品，只要降低一个难度门槛，往往就会获得一批新用户。

>

> 梁宁《真需求》

当强大的模型具备了Function Call的能力，我们看看下面这么多插件，那不得起飞？

![](img/74733c4f161c652480c68d4907679915.png)

更别说扣子插件商店马上要开启商业化试点，比如天眼查、更多的图像插件都会上架，那就不光玩法更多，还会更加稳定和可靠！

另外，抖音AI分身也是个很强的在高流量场景里嵌入的端口，创建后可以分发到抖音搜索/抖音直播间/抖音群聊/视频评论区，加持了DeepSeek FunctionCall之后，能力更强了！想想都觉得牛！

模型+Function Call的方式，和人类解决问题的方式很像，人类并不能解决所有自己遇到的问题，而是需要遇到一个问题时能选正确的工具/渠道，扣子独家支持了 Deepseek Functiocall能力，还支持思维过程显示，这就让让模型调用工具更轻松

### 作业

在本章节的开头，其实我们看到的是使用网页来进行处理，但上面的案例只是在扣子里面使用智能体，请你琢磨下，如何才能实现用网页来进行处理？

如果不清楚，可以继续看下面的章节，再回来完成。

## 03 柴犬表情包生成器实战（网页+Coze API）

### 第一，老样子，这是个啥？

![](img/3d5d57f1d2e5474ea595ebfd493b84de.png)

这是一个柴犬表情包生成器，你可以输入各种表情文字，网页显示对应的表情包（背后是通过和Coze通信来获得生成的表情包图片），支持下载表情包图片。

### 第二，在开始前，我们稍微理解一下Coze和网页的协作机制

这个案例中，我们是Windsurf和Coze一起协作完成的：

想象一下，我们在“**柴犬表情包生成器****”网**页上，输入想看到的狗狗表情**，比如“得瑟”，然后**提交，过了会看到了得瑟的**狗狗表情**包图片，我们所有看到的网页叫做前端页面。你可以想象前端页面就是大堂，我们去餐厅吃饭，都是在大堂吃的对吧。

那我们吃的菜是从哪里出来？是不是我们**点菜后，服务**员传给后厨，后厨做好了拿出来（后端服务）呢？

是的，这里后厨就是**Coze**，他们通过服务员（**API**）收到我们的需求（“**得瑟**”），然后做好菜（“**得瑟的狗狗表情包图片”）**，再通过服务员（**API**）把菜端上来。

当有了后厨之后，我们只需要做好大堂的装修和点菜传菜工作即可，这样AI编程的复杂度大幅下降了，也意味着AI出错的概率下降了。

**后厨也**不难，接下来我们说说后厨的主要部分工作流的工作原理：

一个节点干一件事，然后一个干完传给下一个。这像不像一个流水线？是的，这就叫工作流，英文是Workflow。

对于Workflow，你只需要理解三个点：

1.  **每个节点干一件事；**

1.  节点和节点之间需要有线连起来：上一个节点的尾巴，连着下一个节点的头；

1.  节点有输入，节点处理完之后有输出！（除了开始和结束节点）

没理解？没事，我就简单和你说一下而已哈哈，没理解就不需要理解了，下面来一步步跟我走，做完了再回来看，你会更加理解，甚至会理解的比我说的还好：）

下面我们开始正式开工！！！跟上了，一步步做出来，绝对可以的！

### 第三，注册一个Coze账号，并升级为专业版

首先，注册一个Coze账号，https://www.coze.cn/home

其次，点击下图左下角的扣子专业版入口，完成升级，并充值1元：

![](img/cdba96ea11e6b039c5c15929da0924f0.png)

为何需要升级为专业版呢？别慌，我们前面不是说了要用API和前端页面传菜么？这个API是Coze专业版才有的功能，所以申请一下，很简单的！然后也只需要你充1块钱，就能玩很久。

等下，你说你没1块钱？？？再见。

### 第四，后端搭建流程

Coze的后端主体是工作流，那是谁去调动工作流呢？是智能体，这里大家可以先不用问什么是智能体，智能体和工作流的关系是什么，我们先做，做完你就知道了，我们第一步先创建智能体：

#### 步骤1：创建智能体

![](img/84a351a7c2077e880032063b5ac46cac.png)

1：在coze.cn首页，点击左上角的“+”

2：在弹出的对话框内选择创建智能体，点击创建

此时会弹出下面的弹层：

![](img/d969db0dfcdbec2e004ed3c472903f20.png)

在“智能体名称”里输入“**表情包生成器**”

然后点击右下角的“确认”按钮

此时就创建完成了，简单么？就这么简单！

#### 步骤2：创建工作流

![](img/160a3f6865de5f4fdab72392d62c7a17.png)

在智能体的界面里，别的什么都不用管，我们直奔主题，点击上图中超级红框和箭头直指的“+”，加号。

点击后会弹出下面的弹框：

![](img/c23bd1a9d4df910ca5fa49687c87d80f.png)

鼠标移到“创建工作流”，然后点击“创建工作流”，怎么有两个？套娃呗，反正你顺着两个箭头去点击就对了！

点击后会弹出下面的弹层：

![](img/f1886c6348cab8a540deae569bbc9f36.png)

我们在工作流名称里输入“biaoqingbao”

工作流描述里输入“表情包生成”

然后点击右下角的“确认”按钮，完成创建，进入到工作流编辑页面：

![](img/1c0a2c00d9d3e549dc92468649651b8e.png)

这是一个画布，别的先不管，我们看到上图红框框起来的是第一个节点：“开始节点”，把你的鼠标挪过去，单击它！

#### 步骤3：从“开始节点”开始设置

![](img/d170e55e51aceb8f435304afc50878d1.png)

需要改什么么？不需要。

那“开始节点”的意义是什么呢？其实就是接受大堂传进来的的点菜菜单，这个菜单是以右侧方框里“输入”的“input”变量名来存储的。

没事，不用急，这个“input”菜单很快就会传到下一个节点那边去加工！我们继续：

#### 步骤4：添加“大模型节点”

把你的鼠标移到“开始节点”最右侧的小圆点上，它会变成一个加号，点它！：

![](img/d1e543a08075ef3c86b760abc187fae7.png)

点击后会弹出一个层，选择其中的“大模型”并点击：

![](img/bfd815a98828163547522bfe2f4c34f6.png)

随后，会在“开始节点”后面，新增加了“大模型节点”，同时两个节点也连接了起来：

![](img/5e1e86bd681f90b004fdd19b9231f1ae.png)

这个连接很重要，务必要让节点与节点以前后的关系连接起来！

随后，我们在上图右侧方框里，对大模型节点进行设置。

还记得在前面提过，每个节点干一件事么？在咱们这案例里，大模型节点主要的作用是，根据“开始节点”传过来的菜单，去理解你到底想要的是什么菜！我们需要使用大模型来理解。

下面我们对上图中右侧的方框里，各个参数一个个来处理：

##### 步骤4-1：模型

![](img/b6261fc43b57bf074c67314ae428388f.png)

点击“模型”，选择“**豆包 通用模型Pro**”

##### 步骤4-2：输入

![](img/1d0a26a794ff1c0ae1aa64a41516d04b.png)

这里点击齿轮⚙，然后鼠标移到“**开始**”，再移到左侧的“**input **String”上面，点击选中。

##### 步骤4-3：系统提示词

![](img/6a8f37a50695d21db0cc3e895ff18d35.png)

直接把下面的内容复制到上面的输入框内即可：

```
# 角色：表情包创作者
## 简介
- 作者：唯庸
- 版本：0.3
- 语言：中文
- 描述：表情包创作者是一位深谙互联网文化的自媒体运营，擅长创作富有&quot;网感&quot;的表情包文案。

## 任务
根据用户输入的关键词生成表情包文案和内容描述
用户会提供一个表达情感的关键词（如：开心、生气、紧张等）

## 输出要求
1. 文案创作
   - 长度：4-6个字
   - 风格：网络流行语
   - 要素：押韵/谐音/双关语
   - 特点：简洁有力，易传播
   - 参考：将成语或网络热词改编（如&quot;酸成柠檬精&quot;）

2. 视觉描述
   - 主角：线条风格的柴犬
   - 构图：以柴犬的夸张表情动作为主
   - 细节：加入符合情感的辅助元素（如困倦时的&quot;zzz&quot;）
   - 风格：简笔画感，富有趣味性
   - 重点：表情和肢体语言要夸张化处理

## 输出要求

- 文案：`[4-6字网感文案]`
- 描述：[具体的视觉描述，包含以下要素]
  * 柴犬的表情特征
  * 柴犬的姿态动作
  * 周围的环境元素
  * 整体画面氛围

## 例子

用户输入：紧张
输出：

{
&quot;title&quot;: &quot;缩成团团&quot;,
&quot;content&quot;: &quot;一只柴犬蜷缩成圆球状，像个毛茸茸的饭团，眼睛瞪得圆圆的，耳朵紧贴着头，尾巴也缩在身体下面&quot;
}

## 注意事项
1. 文案要具有当下流行语特色
2. 描述要具体且富有画面感
3. 情感表达要准确且夸张
4. 整体风格要保持可爱有趣
5. 要充分利用柴犬的特征进行创作
```

大家先这么做就好，后面可以回来慢慢理解这一段提示词（Prompt）的含义。

##### 步骤4-4：用户提示词

![](img/b6cde6dc208ee5e8840ee19f7ff4f667.png)

用户提示词如下，直接复制粘贴到输入框即可：

```
{{input}}
```

输入后应该是这样的：

![](img/b4415e51a6608361562e76eec15f5109.png)

##### 步骤4-5：输出

![](img/94bee791147b01d5fb992587080f0f7b.png)

这里我们需要把变量名“output”更改为“title”

同时点击右上角的“+”，新增一行，同时把变量名设置为“content”

最终应该是这样的：

![](img/9fc23af460e41bfe33a1cd2f78432d87.png)

到此，大模型节点设置完成了！我们到下一个节点去！！！go！

#### 步骤5：添加“图像生成节点”

![](img/d03dc68fe66373bb9b689cba3386c9a1.png)

照例还是在“大模型节点”的右侧点击“+”号👆🏻，点击后会弹出节点选择框：

![](img/e491c0fe34794afd0d5559c4df7c6b24.png)

咦，这个时候没有看到“图像生成节点”呀，可以发现右侧有一个进度条，说明下面其实还有更多的节点类型可以选，所以用你的鼠标滚轮，往下滚！发现没：

![](img/9621354c12e96b03f4abf4ad354b05d8.png)

点击选择后，可以看到“图像生成节点”添加成功，下面我们来设置一下，还是分成多个步骤，我们一个个来：

##### 步骤5-1：模型

![](img/ff97b5c5fd3cae454512eccbe9aad52e.png)

这里模型选择“**通用-Pro**”，这个模型可以在图像里生成少量的汉字，黄叔把它比喻成当今地表图中生成汉字的最强模型！

我们继续

##### 步骤5-2：输入

![](img/23bd628f0eda01e6793538f4c979766b.png)

这里按照上图填写，特别需要注意的是，变量值一列必须都是点击⚙，然后选择大模型节点对应的参数。

**千万不用直接在变量值输入框里填写“titl**e”、“content”，这就完全❌了！

这里最容易出现错误了，小心一些。

##### 步骤5-3：正向提示词

![](img/4dfd5122a162ab4c18ae6ea7aed6abb8.png)

复制下面内容到上面的正向提示词输入框

```
一只柴犬，表情包，{{content}}，图片上文字内容：{{title}}，黑白风格的插画作品，流畅线条，通过墨水的晕染效果展现出毛发的蓬松感。
```

好了！恭喜你，基本完事了。

#### 步骤6：设置“结束节点”

![](img/b9879aad60358cf4a1dacc5d2dd81242.png)

此时我们能看到的界面是上图所示，好像没看到“结束节点”呀，怎么办呢？

来，上录屏，大家看：

![](img/f113b64545e510fc9928ebc6f5138d79.png)

[结束节点拖.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UD5vbBptZoB7HJxhGYvcRCKKnNe)

上面全部的操作都是鼠标左键单击，大家可以多练练。

继续，咱们来设置“结束节点”：

##### 步骤6-1：选择“返回文本”

![](img/ad96ba23997eb77dba8ad57db8d03099.png)

点击“返回文本”

##### 步骤6-2：输出变量

![](img/ffffb9f8c0c775ab31123f7af7a3af21.png)

点击齿轮⚙---》“图像生成”---》data Image

##### 步骤6-3：回答内容

```
{{output}}
```

输入后应该是这样的：

![](img/1818e4f041c59f587f8eb32ef02726aa.png)

OK！此时基本完成了工作流的设置！设置完了，我们得试运行一下对吧，继续！

#### 步骤7：试运行

直接上视频：

![](img/32b0e7ff9f6e86904c88822591b50ec5.png)

[试运行_VrteLma9hzo.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JbCwbjhDkonZJ2xxp23csR4TnCg)

在发布工作流之前，需要先试运行确认工作流是OK的。

#### 步骤8：发布

![](img/360da3ebedfa68a2afb0383822c5fafe.png)

整个页面的右上角，点击发布，然后填写一下版本描述，就可以发布了！

恭喜你，完成了最主要的工作流搭建。

#### 步骤9：智能体配置

点击发布后，会进入到智能体配置页面：

![](img/af454a00acd8792a6471447bd76a031c.png)

这里点击“确认”即可，你会发现工作流里增加了刚才我们配置完的工作流：

![](img/8e11bdc68365b7c3dcf4e4a71c4b0c69.png)

接着，在“人设与回复逻辑”里，把下面的文字拷贝进去：

```
当用户发送内容时，调用 biaoqingbao 工作流，将用户发送的内容作为input字段传入
```

然后在右侧的“预览与调试”的输入框内，试试输入任意的表情文字，比如：“兴奋”，正常来说，智能体会把你的输入，当成“input”字段，传给bianqingbao工作流，并等待它完成后的输出。

我们可以看下录屏：

![](img/0e3654e3971d7a4c019a7cb2a57e9293.png)

[智能体调试_RnVWW477pir.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UAQab6DkaoWOAKxozR6c18WtnNb)

整个智能体已经正常工作了，我们看到最后生成的图片文字有些乱码，这是正常的，大模型的图像生成有一定的随机性，如果不符合你要求，可以多让它工作几次即可。

最后一步，我们点击左上角的“发布”：

![](img/38ce983ad5d273988d3799aba223b52b.png)

直接选择“跳过并发布”

最后一步，我们取消勾选发布到coze商店，仅选择发布为API，直接看视频：

![](img/1dca8162736eb6958068fd72c346f191.png)

[发布智能体.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/I0tYb6hDhoxso9xSTuvcxUKxnHh)

OK！这就是所有的智能体搭建流程，最后在智能体的预览与调试里输入表情：

![](img/7f882246b3b7d6a6a4aaf7c1155e8772.png)

这一步其实是这样的工作机制：

用户输入了一个命令，Bot理解后，调动biaoqingbao工作流去完成工作，同时把用户的命令以“input”参数传给biaoqingbao工作流里的“开始节点”，然后一步步的完成，最后在“结束节点”根据要求，把处理完的结果以“output”参数输出出来。

所以我们看到的最终结果是一张图片的链接，点击后弹出一张图片！

OK，Coze的部分我们完成了搭建，最后不是把智能体发布为API了嘛！

为了能使用到API，我们需要去申请一个API Key，你可以理解为要申请一个钥匙，用来使用属于我的API

#### 步骤10：配置Coze API Key

回到Coze首页：https://www.coze.cn/home

![](img/f5ca9906aed83cbef765f6b4c5856d17.png)

逐次点击：“扣子API”---》“授权”---》“添加新令牌”

![](img/691c7732203f241bdafc28c7696968c0.png)

名称：自己填写

过期时间：选择30天

权限：选择“Bot管理”即可

选择制定团队空间：个人空间

点击确认，这里要注意！！！**令牌只会出现一次**，点击复制，找个文档保存起来：

![](img/87b44d638e96821754e3690546207c53.png)

如果不小心关掉了，没事，再重新创建一个。

### 第五，前端网页

好啦，刚才的Coze完成了整个后端的搭建，现在我们需要去做一个前端页面啦，以及需要让前端页面可以通过API和后端页面完成数据传输！

下面我们一步步的来：

#### 步骤1：新建文件夹

在你喜欢的文件夹里，创建一个文件夹，可以取名为“biaoqingbao”

![](img/c2c00ae245853f2e27939e64706c40c5.png)

#### 步骤2：打开Windsurf，再用Windsurf打开biaoqingbao文件夹

![](img/b591790bc199eb8d399544d415220cfa.png)

#### 步骤3：修改好下面这段代码，并提交给Windsurf生成

```
curl --location --request POST &#x27;https://api.coze.cn/v3/chat 
--header &#x27;Authorization: Bearer pat_OYDacM**zM3W**yO**WV3D**tj2bHRMymzxP****&#x27; \ 
--header &#x27;Content-Type: application/json&#x27; \ 
--data-raw &#x27;{ 
    &quot;bot_id&quot;: &quot;734829333445931****&quot;, 
    &quot;user_id&quot;: &quot;123456789&quot;, 
    &quot;stream&quot;: true, 
    &quot;auto_save_history&quot;:true, 
    &quot;additional_messages&quot;:[ 
        { 
            &quot;role&quot;:&quot;user&quot;, 
            &quot;content&quot;:&quot;2024年10月1日是星期几&quot;, 
            &quot;content_type&quot;:&quot;text&quot; 
        } 
    ] 
}&#x27; 

写一个js脚本，前端请求脚本时，将发送的内容带入到参数content，调用上面的API，再写一个前端配套测试API调用的html页面，注意这个API是流式返回
```

把上面的黄色字体的内容，更换为你的，然后丢给Windsurf让它干活！

干活完成后，会生成index.html，即进入到步骤4

令牌：“pat_"标黄的位置是你的令牌，还记得之前你存储的令牌么？找回来。

bot_id：然后再获取一下你的bot_id，bot_id的获取，请你点击进你的智能体页面，查看链接中bot后面的数字：

![](img/ef5a57e9105c5fc3d126c8b1a8ff2763.png)

https://www.coze.cn/space/7436604156019802123/bot/74619821800016***

即上面黄色这一段数字

---

注，以上来源于Coze的API文档：https://www.coze.cn/open/docs/developer_guides/chat_v3

![](img/7521106e4f2d404978b4d33eeed30d62.png)

---

#### 步骤4：打开生成好的index.html测试

1.  打开创建的本地 biaoqingbao 项目文件夹

1.  在浏览器打开html页面

1.  顺利的话可以看到测试API的页面，填入测试数据（比如“期待”），点击提交，查看调用情况

1.  如果正常返回，说明API调用成功了，我们把所有代码拷贝下来：

![](img/bb2ad9f7eb20e33b4cfe876138ea977d.png)

1.  如果错误，把错误信息截图给Windsurf，让它修改，直到正确。

上面这一大坨被选中的代码是API返回的，我们需要进一步处理，提取其中有效的信息，继续下一步：

#### 步骤5：优化js脚本

将上面API返回的代码，粘贴到下方提示词中：

```
复制上一步调用API后界面返回的代码内容，填写到这里

---
优化js脚本，上面是js脚本中调用API的返回内容，希望在后端脚本中对返回内容进行结构化提取，当API完全返回后，提取event:conversation.message.delta中content的内容，在前端直接展示
```

> 这一步是对API返回内容进行过滤，只在前端展示需要的内容

>

> API是流式返回（打字机效果，会多次返回模型生成内容，我们只需要最终的内容展示到页面上）

>

> 将原始返回的数据给模型，让模型进行内容提取

#### 步骤6：测试效果

1.  刷新页面

1.  再次输入内容进行测试

1.  顺利的话，我们可以看到，返回内容被结构化提取了，只返回我们需要的内容：

![](img/805dbfaafcc6fd92377d049b460f1da8.png)

注意，这个链接其实就是Coze最终生成的图片链接，不过我们不需要看链接，我们希望直接看到图片，对吧，最后一步！咬咬牙：

#### 步骤7：优化html样式

粘贴提示词

```
将返回的图片链接展示为图片
仅优化html页面展示效果，主题为表情包生成器
```

最后刷新网页，我的效果如下：

![](img/20dca3b012fe1ab185204728e7f4cd75.png)

哈哈哈，嗨到九霄！！！不知道现在你的心情是不是和我一样！！！

OK，以上就完成了整个Coze API+网页实现的一个柴犬表情包生成器的产品。

#### 课后练习：

| 延伸 | 说明 | 示意图 |
| --- | --- | --- |
| 尝试进一步优化前端 | 1.  可以通过点击按钮的形式输入内容 | ![](img/d0da1c53bfab9ebc607c411504ea01a1.png) |
| 尝试进一步优化后端 | 1.  可以调整大模型节点的文案提示词 |  |

甚至，黄叔把它优化成了可以生成任意动物表情包的产品，其实还是对前面的Coze工作流+网页做的优化，你能琢磨一下并优化出来么：

![](img/9d554741944baf231df9b61849ac7f75.png)

![](img/46890c8cd54b95bb6b6c25991de634d2.png)

#### 搭建逻辑

> 从后向前，从内到外

回顾上面的应用搭建流程，我们可以看到，整体的流程是从后端向前端走，先搭建作为后端服务的Coze流程，再封装成API，最后进行API的调用，前端页面优化

在搭建智能体时，我们先写提示词，设计最内部的大模型节点，再链接节点构成工作流，进一步将工作流添加到智能体，最后发布智能体。

在进行API编程时，我们先对接测试API，再对跑通的API做数据结构提取，最后再把提取出的内容做最外层展示。

这样做的好处：

1.  清晰的逻辑，可以减少自己操作的错误，同时结合版本管理，便于进行回滚，有条理

1.  小白的AI编程建立在与模型对话的基础上，了解AI比了解编程更重要。AI生成结果的正确性与上文内容密切相关，通过小步骤拆解的方式，可以保证每次只推进一点点，测试没问题就保存内容，接下来在正确内容上就行开发，模型会更加容易，减少幻觉

# Part8：部署

## 01 将网页产品部署到Cloudflare！

这里我们以上一个柴犬表情包的Case作为案例，带着大家一步步把你做出来的网页产品部署到云端，让你的亲朋好友都可以使用和访问！

这里我们需要了解一个新的产品：**Cloudflare**，**免费的赛博菩萨**！当然，把产品部署到网上有很多种方案，这里我们先介绍Cloudflare。

注意，这是一个全球性的网络服务提供商（但国内访问有时候会有些限制），免费额度非常慷慨：

+   Workers（每天 100,000 次请求）

+   Pages（每月 500 次构建）

+   。。其他更多

咦，你可能会问，什么是Workers，什么是Pages，这个概念黄叔要先稍微解释下，还是老样子，如果能理解更好，不能理解也不影响后续的动作，先做出来就成：

前面我们已经实现了通过网页生成柴犬表情包了对吧，是不是在开发的过程中，把Coze API返回的结果做了处理呢？

我们可以这么简单理解下：

我们在网页上输入表情，然后网页的一个功能部分会把我们的输入，通过API传给Coze，Coze悄咪咪的躲在后面生成表情包图片后，再通过API返回给前端网页，并且把图片呈现出来，我们才再前端网页上看到了图片。

但是现在这个前端网页只是在我们电脑上，如果我们希望全球的用户都能看到它，怎么办呢？Cloudflare就是干这事的啦，我们现在需要把前端网页和JS部分都放到网上：

看到了不，相当于搬家！我们把前端网页和JS部分，都搬家到Cloudflare上了，这样用户通过网络，只要能访问到Cloudflare，就能访问到我们的网页了（姑且这么简单理解哈）

即使没理解也没事，下面我们先干！

### 步骤1：注册Cloudflare账号

https://www.cloudflare.com/zh-cn/

![](img/321282d059c58d06c1ecb2a01a5743df.png)

点击中间的“免费开始使用”

![](img/61e2ebe2d9d4dba960b6ab77236fd613.png)

使用适合你的方式登录注册！

![](img/dd0b72ed565b407ff627b5a79dbeb0cc.png)

登录后，点击左侧列表的“计算（workers）”，然后再点击“Workers 和 Pages“

找到了吧！在页面中找到创建按钮，点击进行创建

![](img/97cb9dbbe6bc90c5e17bb185b71c5b4f.png)

点击创建worker按钮

![](img/fe649af720ecf707b46b3157d391298e.png)

输入worker名称，这里填写 ai-biaoqingbao ，完成后点击右下角的部署按钮

![](img/929574c954b2370b55edc79398c7b3ec.png)

点击“部署”后，会进入下面的页面：

### 步骤2：部署Worker

![](img/228f28c60d296600cea3ad195415e339.png)

接上一步，我们在这个页面点击编辑代码，会进入到代码编辑页面：

![](img/e0875947d9caf6d147226cca47a966ce.png)

OK，右侧在转呀转，说明它在工作呢，工作啥呢？对，下面我们来告诉Worker应该干啥！

### 步骤3：打开Windsurf，再用Windsurf 打开biaoqingbao文件夹

![](img/87976a9429110e4568451dbf045597bc.png)

对，就是上一个柴犬表情包的文件夹，用Windsurf打开它！（黄叔本地电脑上的文件夹是“biaoqingbao-正式”

### 步骤4：修改好下面这段代码，并提交给Windsurf生成

> API key 和bot id 的替换参考之前的 柴犬表情包案例

```
写一个Cloudflare中的workers脚本，主要功能是请求一个API，注意API是流式返回，下面是API的请求样例，注意处理跨域问题。

curl --location --request POST &#x27;https://api.coze.cn/v3/chat&#x27; \ 
--header &#x27;Authorization: Bearer pat_SCVB8n0KYoXaXLJ9j384iQS7JG*****&#x27; \ 
--header &#x27;Content-Type: application/json&#x27; \ 
--data-raw &#x27;{ 
    &quot;bot_id&quot;: &quot;7450145225****&quot;, 
    &quot;user_id&quot;: &quot;123456789&quot;, 
    &quot;stream&quot;: true, 
    &quot;auto_save_history&quot;:true, 
    &quot;additional_messages&quot;:[ 
        { 
            &quot;role&quot;:&quot;user&quot;, 
            &quot;content&quot;:&quot;震惊&quot;, 
            &quot;content_type&quot;:&quot;text&quot; 
        } 
    ] 
}&#x27; 

同时创建html页面测试API调用，worker使用get方式请求，入参为data，入参在点击发送按钮后会替换到请求体的content中进行API请求

将worker返回内容展示在页面上。一步一步来不要着急。
```

拼接好代码在Windsurf中发送，会创建worker.js文件和html文件

在Windsurf里点击worker.js，能看到它所有的代码，我们把这些代码全选中，然后复制：

![](img/24f5ab6f12ce86f7ab24d87208ef9e4a.png)

### 步骤5：将worker.js文件中的代码复制进来，点击部署

回到前面的worker的代码编辑页面，即前面步骤2的这个：

![](img/2381de4f4707534e23da491a465f4163.png)

把左侧的代码全部删除，然后把你复制的代码粘贴进去👆，粘贴好之后，点击右上角的“部署”：

![](img/e47a042f482b7dc7a252c7264e50ecb9.png)

然后把右侧的链接复制下来！如上图所示！

### 步骤6：打开Windsurf里的index.html，替换对应位置

![](img/5c038f0de58011b220188d14549d0e06.png)

通常Windsurf会提醒我们，替换什么位置的URL！如果没有，参考上面的位置

![](img/52e3937a8e7e912636186b1049a473d2.png)

如果不好找，可以点击图中的代码区域，按下crtl+F，搜索关键词“worker”找到。然后替换！

然后打开你本地文件夹的index.html，做一个测试：

![](img/7e230d91bae801ce96f5da47525fd36b.png)

测试成功，说明此时我们的worker部分已经部署成功了。

还记得前面的图么？

![](img/b12ee61705832907fbd00e25b4ae48c4.png)

图中的JS部分，已经部署到Cloudflare里的Workers了，但还没完，还需要把前端网页也部署上去，继续！

### 步骤7：创建page，前端网页托管

![](img/ae647fe9fc90846502c30121d22a0eb4.png)

我们从前面的worker页面，点击左上角的返回。

![](img/a87b24ded8a4d1c815ea952736133f50.png)

重新点击左侧列表里的“Workers 和 Pages”

![](img/eff657353723fd232bf6ff4980afb48c.png)

然后点击右上角的“创建”按钮

![](img/63c772b3468cdde00197385ea2d3bf18.png)

选择Pages，然后点击下面的“上传资产”

接着，为项目创建名称，名称输入完毕后，再点击“创建项目”：

![](img/80bd06975943209d524c69bcfdbea889.png)

OK，此时进入到下面这个页面，最后一步了！

![](img/e0889ca32555549394c3421612fb16a1.png)

点击它，然后会展开两个选项：

![](img/8f86c90379d1785ebe18944de918721f.png)

因为page功能只支持上传文件夹或压缩包，需要自己新建一个文件夹，比如“ai-biaoqingbao”，然后把“biaoqingbao”文件夹里的所有文件，除了worker.js的之外，全部复制一份进去，比如我的复制完后是这样的（你的不一定有这么多文件，因为黄叔后面多做了一些优化）：

![](img/a1cbc5307c64b1f798dd537add9fbc31.png)

接着选择上传文件夹，选中“ai-biaoqingbao”文件夹，上传！就会显示上传成功：

![](img/8f182a375ab2a6e740eb562e457d1f51.png)

不用犹豫，点击右下角的“部署站点”，稍微等1-2s，就会提示部署成功：

![](img/ecb782dbdbaf727f121cbc3968a6c3e4.png)

### 步骤8：完成部署

恭喜，已经完成了部署！

继续点击左侧列表里的“计算（workers）”---》“workers和pages”，能看到已经部署好的网页：

![](img/904482b177d28f7cea42b5dc6bde628f.png)

注意，不要急马上去访问，因为：

1.  需要等两分钟部署，随后可访问在云端部署好的页面

1.  由于是海外域名，在国内有时会被墙，使用🪜访问

1.  可以绑定自定义域名，需要额外进行设置

过两分钟，点击👆中的访问，可以打开属于你的页面啦！

![](img/e8f8dfb15d889dfcaf893b9e3ef8499f.png)

恭喜！！🎉，完成部署的，请在这里评论，留下你的网页，让别人享受你的快乐吧：）

## 02 使用Github+Vercel免费部署

![](img/53e0f01ebb0f524d0a413a6988af340c.png)

https://read.superhuang.me/

在前面的案例：[04 做一个你专属的好文推荐网站（D](https://superhuang.feishu.cn/wiki/CBBPwvgEuicVhFkx0s7cPmhpn4e?fromScene=spaceOverview#share-WS19di3hOoHiB5xUmGxcL8adnxd)eepSeek R1 + 飞书多维表格） 里，我们其实在开头就给出了一个具体的网址，这个网址的第一步是通过Github+Vercel部署之后，再用国内的域名重定向来的。

这里我们先说如何通过Github+Vercel部署，完成免费的部署，这一步之后，大家就可以在线访问自己的产品了（大部分时候需要🪜）。

Vercel网站，可以一键部署Github的仓库代码，无需自行搭建服务器，就可以完成前端页面和一些后端服务的托管，还是免费的！

> Vercel专注于前端部署和静态网站托管，提供免费100GB/月的带宽，对咱们部署这个案例是毫无压力的。

### 第一，使用AI IDE完成准备工作

首先找到我们在之前的教程中已经开发完成的博客网站项目文件，使用Trae或者Windsurf打开，为了让网站顺利部署到Vercel需要对代码进行一些改造，但是别担心这个过程很简单，依然可以通过AI编程工具进行，我们只需要发出明确指令就行了：

```
我想把这个网站部署到Vercel，需要做什么修改？
```

![](img/1620459db7589cac153e1ff1ce3a90fb.png)

接下来AI编程工具会自动对代码进行改造，包括创建vercel.json配置文件和在其他文件中添加所需配置，这个过程我们不必知晓全部细节，等待改造完成即可。

![](img/8ac139819e1fedbd4f9261eced244104.png)

##

通过vercel进行部署可以完美解决密钥暴露在代码中的问题（就是别人可以通过看你的网页代码，直接拿到你的密钥），实现方式就是通过vercel网站的环境变量把密匙保存在代码之外，在代码中只填写字段的名称，在vercel项目的设置中填写字段的名称对应各种ID和密匙。

可以看到这改造过程中已经完成了这一部分的修改：

![](img/40c534b3c85920289111ca2f8435cca7.png)

现在我们先把改造完成的项目文件上传到github，因为一会必须通过github把文件同步到vercel

### 第二，把本地代码推到Github

#### 方法一：命令AI帮忙

这里我们把Github仓库创建，以及推送都放一块了，直接上一个视频操作演示，当前前提是使用注册好的Github账号登录：https://github.com/

![](img/3dcefdd72c29abcf2d0dd2cc3673fc55.png)

[推送到Github.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WZsEbBmSwo8cMBxuPeXcCLehnnu)

核心是，在Github网站上创建一个存储代码的仓库，然后让Trae把所有的代码推送上去。

大部分情况下，使用和AI对话的方式就可以完成Github推送了。

#### 方法二：可视化的从Trae推送到Github仓库

（此方法感谢 ，这里我们用Trae来做演示，Windsurf类似）

这里的前提条件是，已经在Github里新建了仓库，然后我们需要获取两个信息：

1.  仓库名称

1.  仓库地址

具体的操作方法，可以打开你的仓库后，仓库名称在左上角两个红框处，仓库地址在左下角，点击Code按钮里弹出的HTTPS里，可以点击复制按钮获取：

![](img/d9e1999297fec552cba9bd7de84e2c65.png)

还有一种情况是你的仓库是新建的，还是一片空白，那会是下图的样子，仓库名字在左上角两个红框，仓库链接可在右下角的红框复制：

![](img/0eb11cebe96cd536c91acafa170a91e6.png)

好了，现在打开Trae，点击左上角的“源代码管理”按钮，然后点击“初始化仓库”：

![](img/305f166aea7324816ada0815f1ff2c9e.png)

> 这一步相当于git init

点击完毕后，现在的界面是下面这样：

![](img/23df82ebe453d810cd8411b261bc9021.png)

好了，我们继续往下操作，下一步是把鼠标移到“更改”区域里，然后点击“+”：

![](img/790512855cd5edf31002e4ecb26a2529.png)

> 这一步相当于git add .

ok之后，你会发现出现了“暂存的更改”：

![](img/897590c2026db9ef35d5fae7bceeb4e9.png)

下一步，自然是在消息区域，输入这个版本的说明：

![](img/1d0793179b4e2e3f217d60d977ea375e.png)

> 这一步相当于 git commit -m "0.0.1 写了AI Rules和prd"

接下来，我们要设置具体关联哪个仓库了：

![](img/f1a105e5d7a8c5c6222bb5f60545b030.png)

依次选择“三个点”--》“远程”--》“添加远程存储库”，会弹出下面的弹框：

![](img/5e1838373e2ce5a877b06e4068316e04.png)

你可以点击上面的“从GitHub 添加远程存储库”，或者直接把前面我们获取的仓库地址粘贴过来+在输入框内回车。

完成这一步后，需要你输入远程存储库名称，比如这里我给大家演示的仓库名称是“goldencard”，然后回车确认：

![](img/65be23a4b5865d2eeb985b330aea61b7.png)

这样就完成了和远程仓库的关联，这个可视化界面做的不大好的点是，没有地方显示我们当前有没有具体关联上，或者说不知道关联的是哪个仓库，可以点击下面来查看，没事，我们只是看看：

![](img/00c8fc48850f3071be5441f92966cfe3.png)

此时，会显示当前关联的仓库名称和仓库地址：

![](img/a243415625cf8b8098aaeb30150f260c.png)

ok，按“ESC”或者点击屏幕上空白区域，就可以关掉了，哈哈，别慌。

到了最后一步，我们可以点击“提交”按钮了：

![](img/2ac1d094aebc4f6eeedbc5b609a44013.png)

鼠标挪上去，会显示提交后的影响，直接点击吧！点击后会提示“发布Branch”，同时下面的源代码管理图会有更新：

![](img/c8af052b65f5db8baa1551c937a5d37f.png)

下面源代码管理图，这个区域用于显示当前分支的提交历史或变更记录，帮助开发者快速查看最近的提交信息。

没毛病！继续点击“发布Branch”！随后会有个提交的动效，之后会显示下图：

![](img/b0101e430fd15af21a587befde21e1de.png)

可以看到源代码管理图里，当前这一条的右侧显示了一个小云朵，说明已经同步到云端了。

另外如果你点击这一条信息，会看到上图的右侧，左边灰色区域显示此前为空白，右边绿色区域显示更改了的代码。因为这是第一次，所有的代码都是新增的，所以全绿。

我们再访问这个仓库的链接，刷新一下，会发现出现了刚才上传的代码：

![](img/2c764120fd9a6fc239ad288f9eea3ad1.png)

### 第三，使用Github账号登录Vercel，并部署

> 这一步，我们使用其他案例的内容做演示，大家可以参考流程

选择GitHub账号登录Vercel：

https://vercel.com/login

![](img/e8851efd63a004fe6460519893d4f41c.png)

登录后选择Add New..里的Project：

![](img/a944c1c88693d7ebc38178a978cec203.png)

选择刚才上传的仓库，点击“Import”：

![](img/0b6eccfaa97cc0f79036fc39034b9388.png)

在新页面里点击“Deploy”：

![](img/5975b7fedda01dc565537eddeeee9daa.png)

稍等片刻，会提示完成部署，可以点击底部的“Continue to Dashboard”返回控制台：

![](img/6e73838dda2d786bfe185396d06565ae.png)

你的域名/点击访问等，都可以在下图箭头处操作：

![](img/c6f9d297c01d5fefd1727db8f6811f46.png)

### 第四，把密匙配置为环境变量

进入项目首页后，可以看到一个Visit按钮，这个就是打开网站的按钮，但是现在我们还没有配置密匙的环境变量所以点击也无法正常查看网站。

![](img/7418896e51dabc1240ea3dc9983f1664.png)

先点击设置页面开始配置环境变量

，点击后可以看到左侧有一个Environment Variables选项，点击它：

![](img/c35897244b3d68a6345db7cdd7b92d68.png)

在右侧的key和value的输入框中分别输入4个环境变量的名称和值，首次进入时只有一个输入框，点击下方的Add Another按钮就可以添加更多，添加并填写完成后如下图，然后点击保存按钮就行了。

![](img/0319622f3c3788889b05bd20f71a5b9b.png)

这四个变量如何获取，在第二步：用网页呈现多维表格里的内容 里面我们已经介绍了，大家不记得的话可以回看一下。

### 第五，开始使用

保存后需要1-2分钟重新构建项目，等待以后回到首页，看到下图中的状态是Ready就可以访问网站了。

![](img/e09763c805bbd1798adca87eb537be63.png)

点击vitst按钮之后就可以打开网站，并且会默认给与一个网址。

这个网址默认是：项目名称.vercel.app

至此这个网站已经被成功部署到vercel，也可以通过网址进行访问了。

## 03 白嫖谷歌Chrome插件部署实现最简单的部署方式

![](img/850bcaa094b6cab54ef0893c6b983db7.png)

[Chrome插件部署方式.gif](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RogDb7TzIoPtLcxaOtCcXYCqnW9)

点击插件按钮后，直接打开新标签页！

说到云端部署，前面我们介绍了两种，其实还有一种是很有意思的，我见到过一些产品直接用浏览器插件打开，一开始还纳闷呢，稍微研究一下才发现，原来这么简单！

在之前的教程中我们已经开发过Chrome插件，而Chrome插件的形态可以分成三种：

1、弹出窗口（Popup）

2、侧边栏（Sidebar）

3、全页面（Override Pages）

其中popup就是我们之前flomo插件教程中使用的形态，而Override Pages形态则完全与全屏的网页相同，使用Override Pages形态代替网页，把扩展程序加载到Chrome中，就实现了最简单的云端部署。下面是详细的教程

### 第一. 改造之前的项目为Chrome插件

我们把之前已经部署到Vercel的好文推荐网站文件夹，复制一个副本，改名成“blog1”

> 之所以复制一份，是别影响了之前正常能在Vercel部署的版本

然后使用Windsurf打开它，输入提示词：

```
你先仔细阅读一遍代码了解项目
```

等Windsurf理解完项目后，再输入提示词：

```
按照当前bolg1文件夹内的功能，改造为Chrome扩展的Override Pages形式。
不要对功能逻辑进行改动，读取env文件内的飞书API密匙信息应用到改造后的代码中：
env文件内的密钥信息如下：
# 飞书应用配置
FEISHU_APP_ID=xxxx
FEISHU_APP_SECRET=xxxx

# 多维表格配置
BASE_ID=xxxx
TABLE_ID=xxxx

最后的效果是，点击浏览器插件后，会主动新开一个标签页，使用全屏界面显示博客列表，原本Chrome浏览器的新开标签页不要受影响。
创建一个全新的文件夹保存改造后的chrome扩展程序文件
```

上面标黄字体的信息，你可以从此前项目里获取，在目前的项目中，也可以从.env文件里获取：

![](img/5262c86754221dded5dc9254ad3c9a71.png)

稍等一会，Windsurf完成了工作，我们可以在浏览器扩展程序里选择对应文件夹导入，注意，前面我们要求Windsurf重新创建一个文件夹，防止文件混淆，所以需要看一下它生成的文件夹叫啥：

![](img/35e256b2bfc9c71e2967aa778e3fafdf.png)

导入后报错，没事，复制给Windsurf：

![](img/37eb75e4730a757a64cbc2dbecb77c7e.png)

然后就完成了开发。此时，在你的本地已经可以正常使用了。

如果你把安装包发给朋友，他在自己电脑的Chrome上安装后，也可以访问，但如果希望别人可以直接从Chrome扩展商店里下载，可以继续往下：

### 第二. 上传到Chrome应用商店

当确定一切功能正常，我们就可以准备发布到Chrome应用商店了，首先通过下面的链接进入扩展商店页面

```
https://chromewebstore.google.com/category/extensions?utm_source=ext_sidebar&amp;hl=zh-CN
```

然后进入开发者中心

![](img/2a4c62f0fb08762abac76d03986ae1cf.png)

登录谷歌账号之后跟随开发者注册流程走完即可，所有有疑问的不知道如何填写的字段都可以通过询问Trae或Windsurf完成，最后支付5美元就可以完成注册。注意这里不支持🛰️和支付宝及国内银行卡，需要使用国外信用卡或google pay完成支付。

完成开发者注册后，进入开发者中心就可以看到上传插件的入口了：

![](img/ef375ba386de80e1b2dc0db3cd7a12a5.png)

点击上传之后把刚刚的项目文件压缩为ZIP格式，进行上传

![](img/9f6fab82cf7f0b980b82f736edd9f6fd.png)

后续还需要填写一些信息，这些信息都可以在刚刚的AI编程工具中借助AI输出，把需要填写的字段直接截图发送过去就行，填写完成后提交审核，大约2天左右就可以看到结果～

这种方式借助了Chrome浏览器完成了产品云端部署，是最简单的把产品部署到云端的方式，Chrome扩展程序通过Web Server for Chrome等工具，将浏览器转化为HTTP服务容器。这种扩展基于Service Worker技术实现请求拦截与响应，可直接在用户设备上监听指定端口（如8887），处理静态文件托管、数据上传下载等基础服务，无需依赖远程服务器资源。

但同样有一些限制。例如无法直接操作硬件资源、性能有限无法支持高并发场景、包括当浏览器窗口关闭或系统休眠时，基于Service Worker的后台运行机制最长仅能维持5分钟活跃状态。对于需要24小时持续响应的服务场景，存在服务中断的风险。

但总体来说，对于简单工具产品已经足够了～

### 作业

我们在前面做了个插件，是在看到好文章时，点击一下后直接把文章链接插入到飞书多维表格的，如果是希望用一个插件实现两个功能，你想想怎么实现：

![](img/bbdd2c3bd2f1944fd3bb86abd1a63b6f.png)

## 04 最简易的免费发布：yourward

可以尝试访问黄叔发布的站点：https://8qdgjrdqd5.yourware.so/

基本就是1分钟内完成登录+发布，非常简单

打开网址：https://www.yourware.so/

![](img/7a81469f865acdda497c2ffd333bd861.png)

两步就完成了：

第一步，右上角登录“sign in”，需要用Google账号

第二步，选择单一文件“Select File”，或者整个文件夹“Select Folder”

会弹窗请求确认，点上传即可：

![](img/7db3ef323c54650f4d53998b0fae31a0.png)

上传完毕后，很快，就会出现下面的页面，意味着已经完成部署了：

![](img/7fd1bc6d0e3a6b4fd4f0a728df5d44e4.png)

右下角的“Visit Site”，就是直接访问部署好的站点

左下角的“Bosst My Design”，是优化你的页面，可以尝试玩一下。

打开站点后，地址栏就是你可以分享出去的网址啦：

![](img/5ba85ff76bbc24b871a698a9ac644da3.png)

## 关键：域名购买和绑定（待写）

# 降噪AI（🛰️服务号）

首先，欢迎大家打开🛰️，扫一扫关注我们的服务号产品（暂停服务了）：

![](img/d100620b2168c01e7ddda9d1b9e6cffe.png)

扫码关注后，可以点击左下角体验产品：

![](img/ed5cb94bf423489d837aa8d71057e7c3.png)

如果你点击右上角的订阅更新功能，还可以在每天早上9点，收到一条推送消息，提醒你阅读今日的报道。

这个降噪AI，就集合了多个模块的能力，共同搭建了这一个AI应用：

图中的每一个节点就是一个模块，我们用多个模块拼装起来实现降噪AI，这里面通过把“复杂问题拆解成多个简单问题”大幅降低了AI编程里的复杂度。

![](img/0bd42f03a700727978d1b2206a5a5ae2.png)

其中Coze的工作流如上图，完成了新闻的整理/打分/筛选/摘要/转存的几个步骤。

每个节点，都是一个处理环节，最后再通过API输出，把数据存入数据库，然后前端网页再从数据库定时读取出来呈现，就是大家看到的页面了。

> 一句话：不管黑猫白猫，抓到老鼠的就是好猫

我们的目标是做出产品来满足用户需求，至于是如何做的，用哪些模块拼装出来，是用AI编程，还是Coze工作流，并不重要，做出来更重要。

我们不会吹嘘AI编程能做所有产品，我们理解它也有能力边界，所以本文结合了Coze，通过Coze工作流+Bot，大幅扩展了能做产品的边界。

今后，**本蓝皮书还会更新使用Windsur**f开发iOS App，以及其他类型的应用，欢迎继续点击飞书文档右上角的关注文档更新：

![](img/11371141d0881b630bbe6fd07ba3f6d8.png)

# 做一款App

## 一个提示词完成一款App的设计（Claude4）

Claude4发布后，我昨天用了一天时间调了30多版Prompt，完成了一步做出多个IOS界面设计的提示词，先看看效果：

第一个产品：**给你码****（没有**谐音梗)！idea来源于前段时间特工宇宙的文章：码都来，我微创新，搞了个给你码！

![](img/fe05e45a18e4a04a5d629ce7721f90bc.png)

第二个产品：高考祈福App，马上高考了，给考生祈福吧：

![](img/b1beeb60cf410ee793c0d68377f64ec1.png)

### 完整提示词

```
**#角色
你是一位资深产品****经理、交互设计师、U****I设计师

#任务
****本次任务的最终目标是****生成一套手机A****PP的UI设计****图，首先在**当前项目目录创建task.md文件，列举全部待办事项，每完成一个待办事项就编辑task.md文件，使用✅更新对应事项的完成状态，按照顺序依次完成待办直到所有待办事项均变为完成状态。

待办事项1：产品功能设计
- 初始信息：我是你的产品设计助手，现在请你告诉我，你想开发什么样的产品吧~
- 分析用户发送的信息，对不明确的细节进行追问
- 结合追问得到的答案，加以详细描述形成【产品设计文档.md】文件

待办事项2：交互设计
结合{待办事项1}输出的最终功能，确定该产品包含的所有页面，以下方示例格式输出全部页面的信息。
示例格式：
&lt;页面名称&gt;
用途：&lt;页面的主要作用&gt;
核心功能：&lt;列出该页面包含的主要功能&gt;

所有页面的信息输出完成后，更新【产品设计文档.md】，

待办事项3：UI设计
- 根据【产品设计文档.md】，同时遵守下方{UI设计风格}和{UI设计规格}，为每个设计图创建独立的html文件。

全部页面的html文件输出完成后，中断任务，提示用户输出“继续”指令

待办事项4：提示用户输入“继续”指令


待办事项5：创建一个UI.html文件
- UI.html页面的整体背景色为#f6f6f6
- 使用iframe技术将所有页面以适当的网格排列在UI.html里面，每个iframe的宽度固定为400px，高度固定为850px，保证每张设计图完整显示出来，不会被裁切或遮挡。
- iframe的背景色为##f6f6f6

待办事项6：自检代码
依次检查每个页面的html文件，
检查：强制隐藏所有滚动条
检查：设计图尺寸为 375x812PX
检查：信号栏与标题栏背景色必须一致（可以都是透明）
检查：图标和其他样式调用方式
检查：底部tab栏必须为白色填充，100%不透明度。


待办事项7：检查UI.html文件
检查UI.html文件的全部代码，删除iframe之外的多余装饰性元素，每张设计图的内部已经带有了mock up的样式代码，UI.html内的iframe不需要带有mock up的样式，如果有也需要进行删除。


#UI设计风格
优雅的清新主义美学与功能的完美平衡;
清新柔和的渐变配色与品牌色系浑然一体;
恰到好处的留白设计;
轻盈通透的沉浸式体验;
信息层级通过微妙的阴影过渡与模块化卡片布局清晰呈现;
用户视线能自然聚焦核心功能;
精心打磨的圆角;
细腻的微交互;
舒适的视觉比例;
规范的间距；

#UI设计规格
1、单个设计图尺寸为 375x812PX，带有mock up模拟手机的样式。
2、图标:引用在线矢量图标库内的图标
3、图片: 使用Unsplash开源图片网站链接的形式引入
4、样式必须使用&lt;link&gt;标签引入 tailwindcss CDN来完成
5、状态栏显示时间、信号等模拟信息
6、信号栏与标题栏背景色必须一致（可以都是透明）
7、底部tab栏必须为白色填充，100%不透明度。
8、使用固定高度防止容器变形

#注意事项
1、mock up的颜色只能使用黑色
2、所有html中都强制隐藏滚动条
```

注意：使用Curosr+Claude 4 Sonnet（不是thinking模型）

怎么用这个提示词呢？新建文件夹，然后用Cursor打开这个文件夹，然后选择Agent模式，选择Claude 4 Sonnet模型，然后输入上面的提示词，回车。

稍等一会，就会开始出现下面的界面：

### 开发流程

输入完提示词，Cursor会很快给你几个问题，让你确认：

![](img/d25e6766411818c156714895fdab691a.png)

这一步是需要你来告诉这个万能的提示词，你希望做什么样的产品！

拿我们前面的“给你码”来举例，我们输入的提示词是：

```
做一个产品叫：给你码，现在很多AI应用都需要邀请码，然后产品蝗虫们都找不到但又想用邀请码，所以可以开发一个邀请码共享平台，这样AI应用可以来这里发布邀请码，用户也可以发布自己的邀请码，风格要酷炫一些，其他的你作为资深AI产品经理帮我补充
```

继续，Cursor会接着来追问细节：

![](img/59bc86ee9ea93305b78dba4c2b8fb9b7.png)

你可以详细描述需求，也可以和我这样，简单说说，让AI发挥：

```
AI应用可以选择三个：纳米AI搜索，Lovart，天工SkyWork作为示例，你可以使用搜索获取这三个产品的信息，其他的你看着办
```

然后Claude会酷酷输出，一次生成3000行代码都没问题，是的，Claude 3.7大约只能输出1000行，所以我们静候佳音就好，然后它会完成整体的输出：

![](img/d5842c34ce7288747f125e7a066066b8.png)

这里我们中断了一下，预防模型输出太长，只需要输出“继续”两个字，接着会一直到整个UI稿输出完毕：

![](img/63f99b16dbef2b342639ab9c9a35b299.png)

好的，打开UI.html，可以看到本文一开始的界面了！

如果你想看高考祈福App的提示词，以及继续查看更多有关这个提示词的设计流程，欢迎打开咱们AI编程蓝皮书来看：

![](img/3019628140854f54e93bc83e4940b638.png)

```
做一个产品叫：高考祈福，每个人可以给特定的用户祈福，也可以给全国考生祈福，可以新建考生，然后充钱让更多人来帮他一起祈福，也可以分享邀请更多人祈福。给那些想为高考考生祈福的朋友，祈福会有一些动画效果，其他的你来定，你是资深AI产品经理，很有洞察和创造力。
```

```
给那些想为高考考生祈福的朋友，祈福会有一些动画效果，整体设计要比较温馨，其他的你来定，你是资深AI产品经理，很有洞察和创造力。
```

### 提示词设计过程

**这套提示**词**的重点可**以用两个词来形容，分别是「流程」和「渐进」。

换个角度想想，即使是Agent，第一步做啥第二步做啥，也是需要流程设计的。所以我们其实是用提示词代替了图形界面的工作流形式。因为提示词不像图形化界面那样可以非常明确的设置出各个环节和任务，所以流程控制是重点。

因此首先对应的提示词是：

```
**#角色
你是一位资深产品经理、交互设计师、UI设计师

#任务
本次任务的最终目标是生成一套手机APP的UI设计图，首**先在当前项目目录创建task.md文件，列举全部待办事项，每完成一个待办事项就编辑task.md文件，使用✅更新对应事项的完成状态，按照顺序依次完成待办直到所有待办事项均变为完成状态。
```

渐进一方面是说：为了减少一开始对用户输入信息的要求过高，所以需要采用对话的方式来逐步引导用户完善信息，得到关键信息后由AI进行完善。

另一方面是指：每个环节的输入都依赖上一环节的输出。所以我们要在提示词中严格规定各个任务需要按照顺序执行，**如果上一环节****没有完成输出就**开始下一环节，那么输入信息变差一定会导致下一环节的输出变差，影响最终结果和整体质量，所以这里把按照顺序依次任务设定为最高级规则，也需要首先设定罗列task.md。

随后是**任务部分**，也是这套提示词的重点，我们先看下提示词：

```
**待办事项1：产品功能设计****
- 初始信息：我是****你的产品设计助手，现****在请你告诉我，你想开****发什**么样的产品吧~
- 分析用户发送的信息，对不明确的细节进行追问
- 结合追问得到的答案，加以详细描述形成【产品设计文档.md】文件

待办事项2：交互设计
结合{待办事项1}输出的最终功能，确定该产品包含的所有页面，以下方示例格式输出全部页面的信息。
示例格式：
&lt;页面名称&gt;
用途：&lt;页面的主要作用&gt;
核心功能：&lt;列出该页面包含的主要功能&gt;

所有页面的信息输出完成后，更新【产品设计文档.md】，

待办事项3：UI设计
- 根据【产品设计文档.md】，同时遵守下方{UI设计风格}和{UI设计规格}，为每个设计图创建独立的html文件。

全部页面的html文件输出完成后，中断任务，提示用户输出“继续”指令

待办事项4：提示用户输入“继续”指令


待办事项5：创建一个UI.html文件
- UI.html页面的整体背景色为#f6f6f6
- 使用iframe技术将所有页面以适当的网格排列在UI.html里面，每个iframe的宽度固定为400px，高度固定为850px，保证每张设计图完整显示出来，不会被裁切或遮挡。
- iframe的背景色为##f6f6f6

待办事项6：自检代码
依次检查每个页面的html文件，
检查：强制隐藏所有滚动条
检查：设计图尺寸为 375x812PX
检查：信号栏与标题栏背景色必须一致（可以都是透明）
检查：图标和其他样式调用方式
检查：底部tab栏必须为白色填充，100%不透明度。


待办事项7：检查UI.html文件
检查UI.html文件的全部代码，删除iframe之外的多余装饰性元素，每张设计图的内部已经带有了mock up的样式代码，UI.html内的iframe不需要带有mock up的样式，如果有也需要进行删除。
```

如上文所说 ，这套提示词的最重要规则就是按照流程（待办事项顺序）执行，所以在任务模块的第一部分就要创建待办事项文件，并列出具体的细分任务。

这里面，完成任务后需要输出**产品设计文档.md**文件，用作信息留存，作为下一环节的输入信息和后续开发流程中的参考信息。功能设计完成后需要进行交互设计工作，对页面元素的显示规则、操作结果等做出具体的定义。

然后开始做交互设计，再是UI设计，交互设计规则被更到文档后，开始UI设计就可以得到更准确的结果，而UI设计环节要考虑的就更多一些，信息涉及到视觉风格和页面规格的定义，并且需要在任务中引用这两部分内容。在开始UI设计之前，再次要求了根据产品设计文档内的信息做输出。

到这就完成了一半，接着我们截断了任务，为啥呢？是为了防止遇到代码输出量限制的问题，这里为每个页面创建单独的html文件，所以最后再形成了聚合查看全部UI设计方案的UI.html文件（使用iframe技术只需要极少的代码就可以引入其他独立的UI文件，就像插入了链接一样，只需要一行代码）

这里等待用户输入**继续**，再创建一个UI.html把所有单个App的界面拼到一起，最后再检查一下代码，检查一下UI.html确实是按照我们的要求实现的。

后续又添加了独立的设计风格模块和设计规格模块，整体的提示词就形成了：

```
**#UI设计风格****
优雅的清新主****义美学与功**能的完美平衡;
清新柔和的渐变配色与品牌色系浑然一体;
恰到好处的留白设计;
轻盈通透的沉浸式体验;
信息层级通过微妙的阴影过渡与模块化卡片布局清晰呈现;
用户视线能自然聚焦核心功能;
精心打磨的圆角;
细腻的微交互;
舒适的视觉比例;
规范的间距；

#UI设计规格
1、单个设计图尺寸为 375x812PX，带有mock up模拟手机的样式。
2、图标:引用在线矢量图标库内的图标
3、图片: 使用Unsplash开源图片网站链接的形式引入
4、样式必须使用&lt;link&gt;标签引入 tailwindcss CDN来完成
5、状态栏显示时间、信号等模拟信息
6、信号栏与标题栏背景色必须一致（可以都是透明）
7、底部tab栏必须为白色填充，100%不透明度。
8、使用固定高度防止容器变形

#注意事项
1、mock up的颜色只能使用黑色
2、所有html中都强制隐藏滚动条
```

以上是全部内容~   后续会继续更新IOS APP的实际开发过程

# 末尾（有社群）

很多人会说，上班好累，晚上回到家只想刷手机躺会，啥都不想办。

ok，如果是这样，黄叔推荐个从@泛函 那里看到的一个分享，非常有用。

他的核心策略是：

> 平时多想需求，周末抽一天集中开发！

我觉得这个策略非常适合普通人，碎片化的时间用来思考和优化你的产品需求，周末再集中时间，甚至找几个朋友一起，有个环境能更高效的开发产品。

相信我，如果你是喜欢创造的朋友，在你坐下来，使用windsurf指挥AI不断为你创造的过程，那种喜悦真的是会带来心流，几个小时一下就过去了！

很多朋友希望在学习过程中可以互帮互助，于是黄叔建一个免费的交流群，后续关于蓝皮书里的内容更新/活动，可以及时的同步：

![](img/ae00c83e41fe654c38cf7536b3d5d1b8.png)

![](img/5b9d5de894f428a440ca5cbdea044be2.png)

右图是黄叔的👸号“AI产品黄叔”，后面会更新更多AI编程相关的内容，欢迎关注。

![](img/f57b04ae20abcdd031a2e0cc85fac746.png)