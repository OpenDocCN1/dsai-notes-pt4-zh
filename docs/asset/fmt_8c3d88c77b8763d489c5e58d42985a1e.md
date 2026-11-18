
# 2023-08-04 看完这个，我直接把 SQL 刷通了！

> 来源：https://xuqiwei1986.feishu.cn/docx/DlREdqCfXoIUUxxETTqc87uAnOe
>
> 🔗 原文链接：https://mp.weixin.qq.com/s/ZWG_vjARi1LgJ0xjsnE5lQ

原创 鱼皮 程序员鱼皮 2023-08-04 12:08 发表于 上海

收录于合集

#程序员 212 个
#数据库 1 个
#编程 198 个
#项目 47 个
#计算机 195 个

大家好，我是鱼皮。

相信很多学编程的同学都学习过 SQL 吧？SQL 作为数据库查询语言，实在是太重要了，可以说是程序员、产品经理、数据分析同学的必备技能。

为了帮助大家自学 SQL，这段时间，我一个人做了个免费的闯关式 SQL 自学网，并且决定把代码完全开源给大家学习！

如果你还不知道什么是 SQL，那么相信这个网站可以帮助你轻松入门；如果你已经学过了 SQL，那么不妨来挑战一下，看看自己的 SQL 水平是否过关。

![](img/b5db3c1d63a1c7f10b7c2233ecc4924f.png)

某 B 友的评价

项目演示视频：https://www.bilibili.com/video/BV1pV4y1i7LW（推荐先看这个）

> 开源地址：https://github.com/liyupi/sql-mother

## 项目介绍

这个 SQL 自学网的名称为 SQL 之母，寓意是像妈妈一样贴心地带你入门 SQL~

> 了解我的朋友应该知道，我曾经做了个 SQL 代码生成器叫 SQL 之父，寓意是 “生 SQL” 的

![](img/eafe45622099306cf0a11995e9c7ac4b.png)

我个人认为，比起看教程学 SQL，实战闯关练习的方式更容易带大家入门，所以给系统设计了一系列关卡（总共 30 多关，都是我自己做的）。

怎么使用网站来自学呢？步骤如下：

1. 在主页左侧，是关卡的教程题目区域，包括了知识讲解、示例和题目，请先完整阅读

2. 然后呢，在右上角的做题区域，编写代码并提交，尝试返回题目中要求的数据

3. 如果觉得题目太难了，可以使用右下角助手区域的查看提示、查看建表语句功能来帮助你答题，甚至你也可以直接查看答案

4. 如果你写的 SQL 语句和预期的结果一致，那么就可以进入下一关啦

也可以直接看下面这张图：

![](img/68623d98b51a9cdda7ec6046377ce69a.png)

### SQL 之母使用教程

除了按照顺序过关外，大家还可以进入关卡页面自由选择关卡、自由学习，不用担心卡在一道题目上过不去、也可以不做自己觉得简单的题目。

![](img/b1a28b64f43a8fa38592766e0fb01220.png)

### 选择关卡

在关卡页面，我还提供了几个自定义关卡。毕竟时间和想象力有限，剧情可能比较水，大家别介意：

![](img/b4a00f97faa4cd4a000efcea11f78a46.png)

### 自定义关卡

除了闯关外，我还提供了一个 SQL 广场功能，可以在小小的编辑器里随意输入 SQL 语句来练习~

![](img/63f6d137097a8157dd0c19fe0f399981.png)

### SQL 练习广场

## 项目开源

大家可能会好奇这个网站是怎么实现的？其实很简单，我把代码完全开源了，大家可以自行阅读，源码之前没有秘密！当然，拿去学习练手、或者做毕设也是 OK 的~

开源地址：https://github.com/liyupi/sql-mother

很关键的一点是，这个项目没有后端！甚至没有用到数据库！是纯前端实现的项目！

有同学要问了：为什么不用后端呢？

鱼皮：当然是为了减少被攻击的风险啊！！！

![](img/996334923187abe004a977accce53698.png)

虽然项目是纯前端实现，而且我自己是主后端开发的，都觉得这个项目不难。

首先框架用的是 Vue3，然后站在巨人的肩膀上，核心的 SQL 执行部分使用了 `sql.js` 开源库，通过 webassembly 实现了浏览器环境内 SQL 的执行。

这个项目主要的麻烦之处在于关卡的设计，相当于梳理并编写了一套 SQL 的教程，很花时间。

更多的项目信息我都写在开源文档里了，希望项目代码能给大家一些启发。我还写了一些项目贡献指南和扩展思路，希望能让更多同学有机会参与进来成为贡献者，一起让这个项目更好。

![](img/421ca275af90ca704438a53f7b34a619.png)

开源文档

最后，希望这个网站能帮助大家顺利入门 SQL，欢迎大家体验并且在评论区打出你通关的时间、或者是倒在第几关了，便于我们去改良网站。

制作不易，也希望朋友们可以点赞支持下，谢谢大家~

## 往期推荐

[我的学习小圈子](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247539132&idx=2&sn=45af016dee0c03491750f76ba8fdbd25&chksm=e9c2be4bdeb5375d3253155b4053263109a631620b7cb9074e2fe1b4a5b1604ef92c522b606e&scene=21#wechat_redirect)

[手写 OJ 系统，真的很难么？](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247548629&idx=1&sn=ec262099ca945f4fdea8837178ac8f87&chksm=e9c2d922deb550344b9aa7807ba7092d32209bbdc23fa77914f9fc244df9f8fd273692efb546&scene=21#wechat_redirect)

[拿下鹅厂一面！](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247546598&idx=2&sn=5b16b3b8cbaf7cc9abfa483ed36edbd3&chksm=e9c2d111deb558078a284158c0bf931e5227113c89597e72857632e054ee9ed29bfb73c0664b&scene=21#wechat_redirect)

[鱼皮 C++ 学习路线一条龙！](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247546121&idx=1&sn=3bd30fd4a653a427361d1a2b68aa3877&chksm=e9c2d2fedeb55be8797c9705251e1ac2fdbe8b3716a93d97aba8ab821040869d42daf2b72d06&scene=21#wechat_redirect)

[给学弟妹们的秋招建议！](https://mp.weixin.qq.com/s?__biz=MzI1NDczNTAwMA==&mid=2247545076&idx=1&sn=e34cd7b138d43dae18d7fdd26114979f&chksm=e9c2d703deb55e1584187e679d84ac47a0778f6bd0e072c3af82941bd8ddb564cddab2125374&scene=21#wechat_redirect)

除了 MySQL，这些数据库你都认识么？
