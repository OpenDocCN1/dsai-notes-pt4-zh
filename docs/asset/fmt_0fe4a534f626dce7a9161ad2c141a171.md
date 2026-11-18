
# 《MetaGPT智能体开发入门》FAQ 常见问题

> 来源：[https://deepwisdom.feishu.cn/docx/WFtJdRUsaosTx0x1JgacEK3Znge](https://deepwisdom.feishu.cn/docx/WFtJdRUsaosTx0x1JgacEK3Znge)

## FAQ

遇到了哪些技术相关问题

### Q1. 有些代码有`>>> `，复制后没法直接跑

A: 去掉`>>> `

### Q2. 完整的代码没法直接跑

A: 创建完整目录，包含依赖包、配置文件和运行说明

### Q3. `Exception: Project root not found.`

A: 同Q2

### Q4. 运行后没输出结果

A: 代码定时写死9点执行，可改每分钟执行，并有说明

### Q5. 爬取github网页，有时会报错`aiohttp.client_exceptions.ServerDisconnectedError: Server disconnected`

A: 改成不需要🪜的网页，或是有网络代理说明，代码层面最好加上重试机制

### Q6. `KeyError: 'WXPUSHER_TOKEN'`

A: 没有设置WX，因为只设置的Discord，最好是有参数选择推送渠道

### Q7. `openai.error.APIConnectionError: Error communicating with OpenAI`

A: 同Q2

### Q8. Cannot connect to host `discord.com:443`

A: 设置网络代理说明

### Q9. `discord.errors.Forbidden: 403 Forbidden (error code: 50001): Missing Access`

A: 需要设置bot发消息的权限

### Q10. `aiohttp.client_exceptions.InvalidURL: #%20Today's%20TrendsToday,%20Python%20and%2...`

A: 看着是代码问题，把爬取的结果直接放memory了，导致下一次获取的值有问题

### Q11: 使用MetaGPT 0.5.0版本在Google Colab上执行`await main()`时没有结果，这是怎么回事？

A11: MetaGPT 0.5.0版本在发布后发现有一些兼容性问题，特别是在Google Colab环境中。这个问题已经被开发者识别并通过提交修复（pull request）进行了解决。然而，在这个修复被合并并发布新版本之前，建议用户使用旧版本MetaGPT 0.4.0来完成相关教程或项目。这是因为0.4.0版本在兼容性方面表现更为稳定。

### Q11: 如何配置`GLOBAL_PROXY`来解决网络连接问题？

A11: `GLOBAL_PROXY`配置用于设置网络代理，特别是在需要通过特定网络路径（例如🪜）访问外部资源时。要配置`GLOBAL_PROXY`，您需要提供有效的代理服务器IP地址和端口。这个配置通常放在`key.yaml`文件中，格式如下：

```
GLOBAL_PROXY: "代理服务器IP:端口"
```

确保替换代理服务器IP和端口为您实际的代理服务器信息。一旦配置完成，MetaGPT或相关应用将通过指定的代理服务器进行网络连接。

### Q12:使你的Agent能自动生成总结内容的目录，然后根据二级标题进行分块，每块内容做出对应的总结，形成一篇资讯文档”——这里指的二级标题划分的标准/类型是？

![](img/272f3bbf101e9b43fecbcae5aaa625e3.png)

A12：这里面的就是按照二级标题分类的你可以详细参考这个prompt是怎么写的

![](img/0c468743cd91c264c497dc4153af478d.png)

### Q13:代码中的`_rc`到底是个啥，我看教程这一块看的有点懵，对`_rc`没有一个整体的认识，哪位大佬帮忙解释一下

A13:

![](img/69a880cc46155224104ac1f20e58a5f3.png)

### Q14:metagpt生成的`cli_blackjack`代码报错，是需要自己手动调试吗，还是需要用metagpt重新生成一次？metagpt是否支持自动调试修改？

A14:可以生成软件项目的时候把`run-tests`打开，命中率会更高；或者可以切`env_refactor`分支，这个分支有增量开发功能，但还没合主干，可以用它来给信息、改bug；分支上可以提增量需求，自动bug-fix的能力这两天也会合

目前如果用gpt3.5比较容易出现运行报错的问题，如果是小问题，可以自己调试下。重新生成、代码运行可以关注一下社区的进展，这个功能已到pr阶段，可以看下这个 https://github.com/geekan/MetaGPT/pull/552

### Q15:

![](img/384ad2e5d087887d46c73c1ec45a454a.png)

A15:`export`是Linux下的命令，windows不支持，可以用`config.yaml`来配置

![](img/f2bfd993e160128a8ce7976005238b71.png)

![](img/660caf5f2ec182f4b5305c7b860c5000.png)

### Q16:想请教下，运行第一章示例代码时，日志有warning：

```
2023-12-11 17:03:23.965 | WARNING  | metagpt.utils.mermaid:mermaid_to_file:37 - RUN npm install -g @mermaid-js/mermaid-cli to install mmdc, or consider changing MERMAID_ENGINE to playwright, pyppeteer, or ink.
```

A16:这是个warning，就是pyppeteer不再维护了，现在一般是转向使用playwright了，但playwright要安装步骤会多一些

### Q17:

```
await startup(idea="write a cli blackjack game") # blackjack: 二十一点
```

^

`SyntaxError: 'await' outside function`

A17:

![](img/bb558a230613e861f926b835e4a3f05f.png)

## 未解决的疑问

task3刚才完成了，但是在做第二个多智能体任务的时候，为什么使用`subprocess`经常获取不到输出的结果或者打印的结果呢？请问这个该怎么解决

![](img/fa1fa3fa2ba2836e2f68f7dadf244a40.png)

## 更多MetaGPT faq

[MetaGPT-Index/FAQ等](https://deepwisdom.feishu.cn/wiki/MsGnwQBjiif9c3koSJNcYaoSnu4)

## 你的代码解释助手 （用于debug和代码分析）

GPTs 上传了本教程PDF的课程助教 如果有不明白的问题可以在群内交流也可以请教课程助教，注意将回答丢到群中，这样你可能可以得到一个更多角度的判断

> Q:为什么对话`chatbot-agent`智能体框架要用python异步编程的架构实现

> A:首先，我们需要了解异步编程的概念。在传统的同步编程中，代码按照顺序执行，一个任务的完成必须等待前一个任务完成。这种方式在处理I/O密集型操作（如网络请求）时效率较低，因为CPU在等待I/O操作完成时，会处于闲置状态。

> 异步编程允许程序在等待一个任务完成时继续执行其他任务，这对于I/O密集型的应用特别有用。在聊天机器人的场景中，异步编程使得系统能够同时处理多个聊天会话，而不需要为每个会话独立等待响应。

> 使用Python实现异步编程的一个重要原因是Python的异步支持已经相当成熟。Python的`asyncio`库提供了一套易于理解和使用的API，用于编写单线程的并发代码。这使得Python成为开发异步聊天机器人的理想选择。

> 现在，我们将这个概念简化一下：

> 想象一个服务员（程序）在一个忙碌的餐厅（服务器）里工作。在同步编程模式下，服务员每次只能服务一个顾客，即使顾客只是在等待他们的饮料。服务员必须站在那里，等待饮料准备好，然后才能去服务下一个顾客。这很低效，因为服务员在等待时无事可做。

> 在异步模式下，服务员点完一个顾客的饮料后，可以去服务其他顾客，而不是等待。当饮料准备好时，服务员会得到通知，然后再继续服务那个顾客。这样，服务员可以同时处理多个任务，提高效率。

> 这就是为什么使用Python异步编程架构来构建对话`chatbot-agent`智能体是有益的：它允许聊天机器人同时处理多个对话，而不是一次一个，从而提高效率和响应速度。

## 优秀笔记：如果有常见问题找不到的错误可以浏览优秀笔记看看是否有解决方案

[MetaGPT学习笔记，初遇...](https://rwv2x8ajkma.feishu.cn/docx/HIdpdtJ7Qo3Lu3x9g5ecnzovnJd)

《MetaGPT智能体开发入门》学习手册 第四章课程任务-CSDN博客

[MetaGPT智能体开发入门课程_学习笔记](https://damg37uvq9k.feishu.cn/docx/LZOjdUpORoLlWMxG0ThcgroJnCb)

[MetaGPT框架学习-task3&task4](https://xxfttvlwtj0.feishu.cn/docx/ExMNd1OeaoO8HpxKz21ct7WDndd)

[Meta GPT Agent DIY course 笔记](https://viufqt3dj6s.feishu.cn/docx/I5E8d3UoqooeiWxTKGncwlDnnm2)

[Metagpt - task 1 & 2](https://cvp9iqe9khg.feishu.cn/wiki/ZxGlwdNHWiMOvAkIVNrc7v9fngh)

公测优秀笔记：

[⭐](https://zhuanlan.zhihu.com/p/678360263)[MetaGPT Task04 & Task05—实现订阅智能体](https://iizita3h9dk.feishu.cn/wiki/QexZwrkNliUSEAkxcD3cMkwRnHe)群 root  LLM Agent（三）：基于MetaGPT实现订阅机器人，从HuggingFace上获取Papers并分析总结

LLM Agent（三）：基于MetaGPT实现订阅机器人，从HuggingFace上获取Papers并分析总结

⭐3群 huzixia ：

MetaGPT Task04 & Task05—实现订阅智能体

⭐3群 杜金水：

一个拿来主义者的MetaGPT学习笔记 https://flqcdentjk.feishu.cn/docx/

⭐3群的宏伟（散人）：

Me[MetaGPT 学习笔记](https://snq6prgid0.feishu.cn/docx/POMMdclW6omCuhxCpsncFqhLnug)aGPT 学习笔记

⭐3群的高山为task3专门录制了一期[b](https://blog.csdn.net/Attitude93/article/details/135625879?spm=1001.2014.3001.5502)站视频，里面有很多自己的感悟和理解特别棒，推荐给在task3上遇到困难的同学：MetaGPT课程3作业 MetaGPT课程3作业_哔哩哔哩_bilibili

⭐1群的同学小张：【AI的未来 - AI Agent系列】【MetaGPT】3. 实现一个订阅智能体，订阅消息并打通🛰️和邮件

【AI的未来 - AI Agent系列】【MetaGPT】3. 实现一个订阅智能体，订阅消息并打通🛰️和邮件-CSDN博客
