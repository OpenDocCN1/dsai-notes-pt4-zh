
# 技术交流画布

> 来源：[https://datawhaler.feishu.cn/docx/JlvOdzfmGoOlIXxcXFEcxMdMnOg](https://datawhaler.feishu.cn/docx/JlvOdzfmGoOlIXxcXFEcxMdMnOg)

## 近期技术问题汇总

## 3月26日技术交流会

> Agentscope多智能体框架技术讨论会

> 时间：3月26日 20:00 ~ 21:30

> 主讲人：王熠明，长梦项目负责人

1. 熠明分享`agentscope`开发经验 & 长梦项目设计思路

1. 自由讨论和交流经验

> 会前推荐阅读：《Agentscope应用开发入门》`https://datawhaler.feishu.cn/wiki/GInXwJYKlibkSwk04qrcfOJBnge`

> 录制文件：`https://meeting.tencent.com/user-center/shared-record-info?id=04c63d12-7368-43cf-a400-08ae117e7740`

![](img/5c2f0dcdfe1ffb21e8f46c302f2ad767.png)

![](img/6aeaff4e44cd5af47458c61cb34ad5c4.png)

使用`input()`获取用户互动

![](img/5b426bc3cbcdd04f2698f67a479facec.png)

![](img/355ba665057ae9a51d10c28ea407343f.png)

可以获取单个`agent`的对话记录，`user`的`content`是输入给他的`Prompt`

### 开发思路：

1. 确定一个应用创作方向：问你自己想做个啥

1. 解决UI设计问题：确定一个想模仿的UI，直接copy其UI代码

1. 拆分功能：确定好UI各个按钮和组件分别是用来做啥的、预期效果是啥

1. 实现功能Demo：东抄抄西抄抄，用AI帮忙写代码，把基础功能能力实现出来，跑通最小MVP、实现可运行和展示的demo

1. 迭代和优化效果：跑通了demo之后，通过设计更好的`Prompt`、设计更多容错机制、不断测试和迭代效果

1. 优化项目：通过更多的用户体验获得反馈，优化应用设计、优化UI

![](img/a5901f532d60044d35721f60965df62a.png)

![](img/b10ed4ce31fc2ed128de5d1f8a5aaf23.png)

---

### 交流话题

1. 上下文太长的解决思路？

1. 做一个总结`Agent`，`Langchain`就是这样的方案，设计总结的字数？

1. 只获取关键信息？

1. `agentscope`新上线的@功能大家有使用过了吗？

1. 多人对话开始说话的人能指定吗

1. 队列是空的会随机选人？

1. 请问这个`NPC1`是如何拿到旁白的输出的？看起来旁白和`NPC1`是两个独立的`agent`，并且不共享`memory`

1. 直接用代码拼接就行

1. `agentscope service`能丝滑调用`modelscope`的`tools`吗？好像`agentscope`还没有引入`ReAct`机制？

1.

1. `agentscope`如何支持任意`http`的大模型调用方式？

1. 事实上，我们用`langchain`自己二次重写一个`Agent`类，任意接入模型。所以我觉得`AgentScope`也应该提供任意形态`API`的大模型调用

1. 如何调用语音`tts`？

1. 框架里的`example`，有语音输入的功能，需要测试一下

1. 看一下工具源码

1. 在创空间部署的多人对话即使页面关闭了，也不会停下来，消耗了大量`tokens`

1. 增加终结代码，注意不要消耗太多

1. 智能体记忆能否可选是否使用，每次调用记忆耗费大量`token`且耗时

1. 直接把`memory`清空，存储在另一个隐藏的`agent`里

1. `memory.delete()`

1. `userAgent`是干啥的？`uid`好像可以不用？

1. `Prompt`设计 & 测试的思路？

1. 直接在一个`Notebook`进行测试？工作站？

1. 任务导向，按照自己的想法去调，调不出来拆分为多个`Agent`？可能不是`Prompt`问题，而是基座模型能力
