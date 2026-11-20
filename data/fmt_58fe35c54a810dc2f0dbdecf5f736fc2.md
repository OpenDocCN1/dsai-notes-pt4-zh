
# OpenAI主动公开自家爬虫，只为撇清窃取数据之嫌

> 来源：[https://jalicvoygc0.feishu.cn/docx/Vi8Hdj6YroDUtxxwQWrcJjAJnnd](https://jalicvoygc0.feishu.cn/docx/Vi8Hdj6YroDUtxxwQWrcJjAJnnd)

作者：三易菌

AI大模型需要海量的数据来“喂养”，这是毋庸置疑的，ChatGPT之所以表现得如此优秀，背后是OpenAI拿出了超过1750亿个参数。更多的数据等于更强的性能，让数据的价值在大数据时代之后又一次被放在聚光灯下。那么，AI大模型所需的数据从何而来呢？

![](img/315e64d1c296c74e29f1d3c79cbc3b2a.png)

摆在明面的方式是通过API向推特、Reddit等内容平台付费购买数据，摆在台面下的方式就多了，比如说不合规的网络爬虫。没错，OpenAI就被指控互联网抓取的信息来训练ChatGPT时，从互联网上窃取了3000亿个单词，它们来自“书籍、文章、网站和帖子——包括未经同意获得的个人信息”，严重侵犯了大量用户的版权和隐私。

事实上，使用网络爬虫获取数据在过去二十余年来在全球都属于一个灰色地带，合法性始终备受争议，因为爬虫到底有没有获取有版权的内容、有没有爬取非公开数据、有没有侵犯个人隐私，在没有被抓住证据之前都处于“薛定谔状态”。为了打消外界的顾虑，OpenAI在最近公开了自己的爬虫(`GPTBot`)以及IP地址网段，以供有需求的网站屏蔽。

![](img/1d33c53767e3431c6d2f8ab6b97c5298.png)

根据OpenAI的说法，`GPTBot`遵守网站提供的`robots.txt`协议，同时会过滤掉付费才能访问的内容。并且，基于OpenAI的相关政策，如果网页中包含个人身份信息或其他违规内容，则`GPTBot`也会删除这部分网页。`GPTBot`使用的网段为`40.83.2.64/28`，网站站长可以使用`nslookup`命令来判断`GPTBot`爬虫的真伪。如果想禁止`GPTBot`抓取网站的内容，可以在`robots.txt`中写入指令，爬虫检测到该指令后会按照指令要求操作。

`robots.txt`也被称为robots协议，这是一种存放于网站根目录下的ASCII编码的文本文件，它的唯一作用就是告诉网络爬虫，该网站中的哪些内容是不对爬虫开放，哪些内容又可以被爬取。robots协议是控制网站被搜索的内容的一种策略，该文件一般会放在网站的根目录里，在网站域名后面加上`/robots.txt`，就可以直接访问到该网站的robots协议页面。

![](img/ded9c4e01d30cebba9f7c7e365c3ad69.png)

这里我们以淘宝网的`https://www.taobao.com/robots.txt`为例，这家著名的电商网站采用的robots协议非常简单，`User-agent`主要作用是会告诉网站服务器，访问者是通过什么工具来请求的，后面的`Baiduspider`就是大名鼎鼎的百度搜索引擎爬虫，最后的`Disallow: /`，按照robots协议的规则，是禁止被描述的百度的爬虫访问网站。

没错，百度搜索引擎被淘宝在2008年9月的时候屏蔽，依靠的就是这简简单单的几行代码，让淘宝在战略层面掌握了竞争的主动权，避免了流量被百度搜索引擎拿走，也避免了平台内的商家要给百度竞价排名付费的可能，间接催生了淘宝的站内竞价排名体系。

![](img/5c4d5d149239337e90f60bc968d699e6.png)

robots协议本质上来说就是运营方写的一贯规则书，它的书写非常简单、逻辑也非常直白，允许什么搜索引擎爬虫访问什么内容都可以清晰的写出来。需要注意的是，robots协议本身是一个“君子协议”，只是一个搜索引擎和网站之间的共识，它是没有任何法律效力的，也没有任何技术约束，单纯是一个道德层面的约束。换句话来说，robots协议从技术层面上是完全对抗不了爬虫的。

故而，robots协议对于非法的爬虫是毫无约束力的，但是对于重视商誉的大公司来说反而具有效力。毕竟，以百度的技术力想要强行获取淘宝的数据并不难，但就是简单的四行代码就愣是让百度爬虫在过去十余年里对淘宝“秋毫无犯”。同理，这一次OpenAI也是用主动公开自家爬虫的IP地址网段，就是以品牌形象为担保，将自己的爬虫行为一切摆在台面上。

![](img/94fd2f39858ecd1083b207d7143ed02e.png)

对于OpenAI而言，这也是其在面临窃取数据非议时，最简单、最直接的证明自己确实合规的措施。毕竟，对于OpenAI这样对于数据有海量需求、高技术力且追求“盈利上限”的公司，一边是合规获取海量数据带来的高昂成本，一边是寻求外部资金支持需要拿出更好的预期，要自证清白确实不容易。

引用：

`https://mp.weixin.qq.com/s/OoYSqmsYsaaadqYViHhN_Q`

---

- [🎪首页](https://jalicvoygc0.feishu.cn/wiki/DdEvwxYsSidd36kAIIVcnROwnmb)
- [🤔学习AI工作流](https://jalicvoygc0.feishu.cn/wiki/W15IwochvimsqvkkuCPcZuBxnqg)
- [🤖查找AI工具](https://jalicvoygc0.feishu.cn/wiki/Bu8GwqYu8i5LBRkyVKocUvX5n97)
- [📊阅读AI文章](https://jalicvoygc0.feishu.cn/wiki/SIywwqrVji2lZkkqFFpcEHZ8nNc)

扫码加入AI Flow Genius社群，探讨使用AI来提升生产力的方法，成为超级个体

超过200需手动拉人 **备注：加群**

![](img/92008f94d4b5f833e825a70d531f8f8d.png)
