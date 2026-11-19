
# 突发！OpenAI官方开源多智能体框架「`Swarm`」

> 来源：[https://v11enp9ok1h.feishu.cn/docx/NFsBd9pI6oWvyyxwRYLcayNhnTc](https://v11enp9ok1h.feishu.cn/docx/NFsBd9pI6oWvyyxwRYLcayNhnTc)

一句话总结：

**OpenAI终于自己下场做多智能体**框架了，虽然还只是实验性的，但是很有用。

今天早上，OpenAI实施团队的 @shyamal在Github上开源了`Swarm`这个OpenAI官方的多智能体框架。

`Swarm`里面定义了两个核心「`Agent`」和「`Handoffs`」。但是一个「`Handoffs`」，就把多智能体的关键，点的透透的。

大概过了一下文章和代码，简单看了下examples，不得不说，官方下场，就是不一样，下面就来带大家看看不一样在什麼地方，地址放在最后面。

![](img/43dd08fab0de921deae9280d570b7a99.png)

## 为啥需要多智能体

我在之前的血泪史系列文章里有断断续续提到过很多关于`Agent` `Tools`调用和编排相关的内容，有兴趣的同学可以先读读我之前的一些内容：

+ AI `Agent`产品经理血泪史：一年来我摸过的那些石头【`Tools`篇】

+ [AI `Agent` 产品经理血泪史（二）-](http://mp.weixin.qq.com/s?__biz=MzIzMjAxMzYyNQ==&mid=2653601515&idx=1&sn=cdc56dd546f9d276176201104cd228af&chksm=f34574bdc432fdab4e3cc0ee81d673c406b76c1a03cf815408c1f5f0b2ec7d0ee8efe9e8c86d&scene=21#wechat_redirect)欲知方圆，则必规矩【`Workflow`篇】

这两篇内容讲到，如何构建不同类型的`Agent`来满足不同的需求，复杂度从易到难，分别是：

1. 纯`Prompt`对话

1. 单个工具调用

1. 多个工具调用

1. 通过`workflow`来执行复杂流程

1. `Multi-Agent`多智能体协同

虽然单个`Agent`配上多个`Tools`，`Workflow`之后也能处理不同的问题，但是一个`Agent`的`System Prompt`是只有一个，能携带的`Tools`也是有数量限制的。

如果我们需要处理很多不同类别的复杂问题，要怎麼办？

相信大家都有打过10086客服电话的经验，或者电商客服咨询的经验：

如果你只是简单的问个常规问题，接线的客服直接就给你回答了，但是如果你问了一些其它业务线的问题时，比如宽带，那客服小姐姐就会温柔地跟你说“请稍等，我帮你转接到宽带部门”，一阵美妙的音乐声之后，另外一位专业的客服小姐姐接起了电话...

术业有专攻，智能体也一样。不能逮住一个牛马就使劲薅。

## 为啥用`Swarm`的多智能体

![](img/09830612b2036b05285023d3e51a11fc.png)

脚趾头：

因为是OpenAI出的。（君不见现在大多数大模型都在遵循OpenAI的接口规范...他们对自己做的东西更了解）

脑门：

其实单`Agent`这块，没有啥花里胡哨的东西，简单业务，OpenAI就一个 `/api/completions` 接口。但是「`Handoffs`」这块，`Swarm`的确做的非常优雅。（这里不得不给自己吹个牛，年初我就写了`Swarm`类似的多智能体了。）

多智能体的核心难题其实是不同智能体之间的通信问题。怎麼传递，传哪些信息，这些都很重要。

其实之前很多多智能体开源框架，走的都是Room/Group的思路，就是把各个智能体都扔到一个大空间里，然后每个智能体都接收信息，每个智能体都存储信息。不说效率低下，光`token`的消耗都扛不住。

**实际上，多智能体**，也只要在必要的时候被`call`起就可以，回到我们上文 10086客服 的例子。

当接线小姐姐识别到这是个宽带问题需要转接的时候，她需要做2个事情：

1. 找到宽带部门的小姐姐，把会话权限交接过去；

1. 把记录「`Messages`」和我的问题「`Query`」交接过去（实际上会自动记录，共享查阅）

那如果我们需要构建这样的一个客服多智能体，是不是只需要准备两个`Agent`：一个普通接线客服，一个宽带客服。

`Swarm`的「`Handoffs`」处理了交接的逻辑。下面我用官方的例子魔改一下客服例子，方便大家理解。

```
from swarm import Swarm, Agent

client = Swarm()

# 定义一个交接函数，作为普通客服的工具
def transfer_to_agent_kuandai():
    return agent_kuandai

# 定义一个普通客服小姐姐
little_sister_a = Agent(
    name="普通客服小姐姐",
    instructions="你是10086的客服小姐姐，你负责处理基础的业务问题",
    # 这里就给普通客服一个转接宽带客服的工具，当需要处理宽带业务的时候会被调用
    functions=[transfer_to_agent_kuandai],
)

little_sister_kuandai = Agent(
    name="宽带客服小姐姐",
    instructions="你负责处理宽带业务问题",
)

response = client.run(
    agent=little_sister_a,
    messages=[{"role": "user", "content": "我想要咨询一下宽带问题"}],
)

print(response.messages[-1]["content"])
```

执行这段代码，打印出来的对话记录就可能会是

```
用户：你好，我想要咨询一下宽带问题
普通客服：请稍等，我帮你转接（转给宽带客服）
宽带客服：你好，请问有什麼宽带问题需要查询咨询？
```

是不是就变得非常清晰了？

总得来说就是：

1. `Agent`部分其实大同小异；

1. `handoffs`通过封装一个`tools`给到`agent`，当用户的`query`意图触发的时候，调用这个工具，处理交接的一系列逻辑（ 可以理解是把另外一个`agent`的`prompt`，`tools`，拿过来，结果了`a`的上下文记录，再重新请求一遍 ）

好了，剩下的自己看吧，标黑的都是重点。

`https://github.com/openai/swarm`
