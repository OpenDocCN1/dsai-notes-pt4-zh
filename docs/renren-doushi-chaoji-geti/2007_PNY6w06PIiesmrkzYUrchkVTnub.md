# 搭建Z-Library的tg机器人以及找电子书

> 来源：[https://xuqiwei1986.feishu.cn/docx/Xi4udHExMovbjfxszOzcNkaenFf](https://xuqiwei1986.feishu.cn/docx/Xi4udHExMovbjfxszOzcNkaenFf)

**什么是****Z-Library****？**

Z-Library（简称z-lib）是一个图书馆和文件分享计划，用户可在此一网站上下载期刊文章以及各种类型的书籍。根据Z-Library的说法，截至2022年6月12日，其共收录了10,456,034本书和84,837,646篇文章。Z-Library在其电子书搜索页面上宣称自己是“全球最大的数字图书馆”，并在文章搜索页面上称自身是“全球最大的科学文章存储库”。

![](img/a891e3ad85391eea951eeb301cf55b36.png)

可以称之为最大的电子书库

**

什么是tg的机器**人

Telegram是类似于微信、钉钉一样的即时聊天工具，tg机器人也和企业微信或者钉钉的群机器人一样，可以通过一些指令让机器人回复或者处理某些事情。但是tg的机器人不一定要加入到群里，可以单独作为一个好友让你去订阅或者添加它，类似于公众号一样，你对它输入一些指令，它会返回处理后的结果给你。

具体的说明，可以看下面两篇介绍。

http://www.dingchen.com/myrouter/wenzhang/Telegramjieshao.html

https://appmaster.io/zh/blog/telegram-mo-kuai-chuang-jian-liao-tian-ji-qi-ren-he-pin-dao

# **具体怎么操作？**

1点[击此链](https://t.me/BotFather)接或在 Telegram 上搜索 “@BotFather” 机器人

2向 @BotFather 发送创建机器人命令：/newbot

3按提示，给你的聊天机器人指定一个名称（name），可随意命名。

![](img/38e807cecb2896ae1caeb54e216ae89a.png)

4为机器人选择一个用户名（username），必须以 bot 结尾，例如：bookbot

![](img/f25b31f1292ef5415c3ba4d38fa734a6.png)

5从 BotFather 复制 HTTP API，是一串长长的字符。

![](img/1359c76672cbdf10eb4a1a7137b75f82.png)

6在浏览器上访问 Z-Library 的登录页面（需要张良计）：https://singlelogin.me/ 登录你的 Z-Library 账号。然后进入“编辑个人资料”的页面，找到Personal Telegram bot（现在TG机器人只能私有了），点击Try it now，会给出对应的指引。

![](img/5695b99775a50d5ee030ec2d8cb56507.png)

![](img/7156f1d10332a233670b9f2e695e1d4d.png)

7现在，搜索并打开你的 Z-Library 机器人，即可和机器人对话，进行图书检索和下载了。每个私有化的机器人每天只能下载10本书，如果只是给自己用那还是绰绰有余了的。

![](img/fa3b22c0fd0ba06d39d0c5bda3f46666.png)