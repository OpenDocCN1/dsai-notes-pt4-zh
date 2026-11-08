# 一个更小、更快、更干净的GraphRAG！

> 来源：[https://agijuejin.feishu.cn/docx/VcMGd8wFWoZ4QvxYjCVcS59en2g](https://agijuejin.feishu.cn/docx/VcMGd8wFWoZ4QvxYjCVcS59en2g)

> 作者：PaperAgent

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/gdWINeUR2Qq3Rz_7TuhBIw)qq.com/s/gdWINeUR...

**nano-GraphRAG **一个简单，易于修改的GraphRAG实现

![](img/41594f0cd8d1215a3f2864c03efb04d0.png)

😭 GraphRAG很好很强大，但是官方实现很难/痛苦地阅读或修改。

😊 这个项目提供了一个更小、更快、更干净的GraphRAG，同时保留了核心功能（见基准测试和问题）。

🎁 除了测试和提示，nano-graphrag大约有800行代码。

👌 小而可扩展，异步且完全类型化。

```
pip install nano-graphrag
```

下载查尔斯·狄更斯的《圣诞颂歌》副本

```
curl https://raw.githubusercontent.com/gusye1234/nano-graphrag/main/tests/mock_data.txt &gt; ./book.txt
```

使用下面的Python代码片段

```
from nano_graphrag import GraphRAG, QueryParam
graph_func = GraphRAG(working_dir=&quot;./dickens&quot;)

with open(&quot;./book.txt&quot;) as f:
    graph_func.insert(f.read())

# Perform global graphrag search
print(graph_func.query(&quot;What are the top themes in this story?&quot;))

# Perform local graphrag search (I think is better and more scalable one)
print(graph_func.query(&quot;What are the top themes in this story?&quot;, param=QueryParam(mode=&quot;local&quot;)))
```

下次你从相同的工作目录初始化GraphRAG时，它将自动重新加载所有上下文。

**增量插入**

nano-graphrag 支持增量插入，不会添加任何重复的计算或数据

```
with open(&quot;./book.txt&quot;) as f:
    book = f.read()
    half_len = len(book) // 2
    graph_func.insert(book[:half_len])
    graph_func.insert(book[half_len:])
```

nano-graphrag 使用内容的 md5 哈希作为键，因此不会有重复的区块。

然而，每次你进行插入操作时，图的社区将被重新计算，社区报告也将被重新生成

nano-GraphRAG与微软Graphrag 效果对比参数设置：

```
使用罗贯中的《三国演义》作为基准语料库。
使用了官方GraphRAG实现的61b5eea34783c58074b3c53f1689ad8a5ba6b6ee提交版本。
GraphRAG和nano-graphrag都使用OpenAI的嵌入和gpt-4o。
两者都不使用缓存。
在同一设备和网络连接上。
GraphRAG的最大异步API请求数：25 
nano-graphrag的最大异步API请求数：16
```

**Index对比**

都超过10分钟

## **Local Search对比**

+   **GraphRAG**

### "大乔和曹操的关系"

```
大乔与曹操之间的关系在历史和文学中被描绘为复杂且多维的，尽管直接的互动可能并不频繁。大乔，作为东吴孙策的妻子，以及其美貌和与三国时期重要人物的关联而闻名。曹操，作为三国时期的关键政治和军事领导者，其与大乔的关系主要通过他与东吴的关系以及对大乔及其家族的兴趣来体现。

首先，大乔的美貌在三国时期广为人知，甚至引起了曹操的注意。据记载，曹操曾有意俘获大乔以供己用，这一点体现了曹操对大乔的个人兴趣，尽管这种兴趣可能更多地是基于她的美貌和象征意义，而非两人之间有实质性的个人关系 [Data: Entities (266)]。

其次，大乔作为孙策的妻子，自然成为了孙家与东吴势力中的一员。孙策和孙权是曹操在政治和军事上的对手，这使得大乔与曹操之间存在着间接的敌对关系。曹操对东吴的军事行动，包括对孙权的攻击，间接地影响到了大乔及其家族的安全和利益 [Data: Entities (266)]。

最后，尽管直接的文献记录可能不多，大乔的地位和她所处的时代背景使她与曹操之间的关系充满了历史和文化上的意义。她的故事和曹操的野心相交织，展现了三国时期女性角色与政治军事活动之间的复杂联系。

综上所述，大乔与曹操之间的关系主要是通过她的家庭背景、曹操的政治军事活动，以及文化记忆中对她美貌的追捧来定义的。这种关系更多地体现在历史和文学的叙述中，而不是基于两人之间有直接的个人互动。
```

+   **nano-graphrag**

### "大乔和曹操的关系"

```
# 大乔和曹操的关系
## 大乔的背景
大乔（Da Qiao）是江东乔公的长女，以其极美的容貌闻名于世。她最为著名的身份是孙策（Sun Ce）的妻子，这一身份在三国时期的政治和军事格局中也占有重要地位。周瑜特别提到她，显示她在周瑜与曹操局势中的重要性 [Data: Entities (1)]。
## 曹操的欲望
曹操（Cao Cao）对大乔的渴望主要源自其好色本性。他希望得到大乔和她的妹妹小乔，以满足个人欲望，并且认为获得她们可以帮助他提高政治地位和享乐。曹操曾发誓希望能得到江东二乔（大乔和小乔），并将她们置于铜雀台上，以乐晚年 [Data: Relationships (6, 7); Entities (1, 2)]。
## 谋略与政治
在三国时期的权谋斗争中，诸葛亮曾提议使用美人计，将大乔和小乔送给曹操，以此来使曹操班师回朝，减少对东吴的威胁。这一提议显示出大乔在权谋中的重要角色以及她可能对大局产生的影响 [Data: Sources (0); Relationships (12)]。
## 曹操的策略和行动
尽管曹操对大乔有很大的兴趣，但实际行动中的证据有限。周瑜得知曹操对大乔的意图后，表达了强烈的反对，并与诸葛亮商议对策，以确保她和小乔的安全 [Data: Sources (2)]。周瑜明确表示大乔是孙策的正妻，而小乔是他的妻子，拒绝将她们送给曹操。
## 总结
总体而言，曹操和大乔没有直接的互动记录，但曹操对大乔的欲望及其可能带来的政治影响却是不容忽视的。大乔作为江东乔公的长女，其美貌和身份使她成为多方势力争夺的对象，但她与曹操的关系主要体现在曹操的欲望和诸葛亮、周瑜等人的权谋计划中。
曹操对大乔的欲望和权谋体现了三国时期复杂的政治动态，同时也揭示了美人计在当时战略中的重要性 [Data: Relationships (6, 12, 13); Entities (1); Sources (0, 2)].
```

## **Global Search对比**

+   GraphRAG

### "大乔与曹操的关系"

```
### 大乔与曹操的关系

大乔（Da Qiao）是乔公的女儿，以其非凡的美貌而闻名。这一特质使她在社区中成为一个重要人物，尤其是在曹操（Cao Cao）对她的渴望背景下 [Data: Reports (41)]。

#### 曹操的计划

曹操计划在统一江南后，将大乔安置在铜雀台（Tongque Tai）。铜雀台是曹操在叶县张河上建造的一座宏伟平台，象征着好运和庆祝。曹操打算将包括大乔在内的美丽女子安置在铜雀台中 [Data: Reports (269)]。这一计划不仅反映了曹操的野心和愿景，也显示了他对大乔的特别关注。

#### 乔公与曹操的关系

乔公是大乔的父亲，同时也是曹操的朋友。这层关系不仅突显了乔公的影响力，还暗示了由于曹操对大乔的渴望，可能会产生潜在的杠杆作用或冲突 [Data: Reports (41)]。

### 结论

综上所述，大乔与曹操的关系主要体现在曹操对她的渴望以及他计划将她安置在铜雀台的意图上。此外，乔公与曹操的友谊也为这一关系增添了复杂性，可能会在未来引发更多的动态变化 [Data: Reports (41, 269)]。
```

+   **nano-graph**

"大乔与曹操的关系"

```
## 曹操与大乔的关系

曹操对大乔展现了显著的政治和战略兴趣，这部分是由于大乔的美貌以及她在战略上的重要性。在那个时期，美丽并非单纯的外貌特征，还往往与政治资源和战略权力相关联。曹操认为拥有大乔会对他的政治野心有所帮助，并为其权势扩大增添筹码 [Data: Reports (6)]。

大乔作为孙策的妻子，再加上她的地位和美貌，使得她在权力动态中成为一个关键的目标。曹操对大乔的重视，不仅因为她个人的吸引力，更因为她所代表的权力象征。如果能将大乔纳入自己的势力范围，曹操将可能通过这种方式削弱敌对势力，同时增强自己的影响力 [Data: Reports (6)]。

总之，曹操和大乔的关系不仅仅停留在个人层面，更深层次的是政治策略和权力博弈的体现。曹操对她的兴趣，反映了他在权力斗争中的精明与野心。
```

```
https://github.com/gusye1234/nano-graphrag
```

**推荐阅读**

+   对齐LLM偏好的直接偏好优化方法：DPO、IPO、KTO

+   2024：ToB、Agent、多模态

+   [RAG全景图：从RAG启蒙到高级RAG之](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)36技，再到终章Agentic RAG！

+   [A](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)gent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）

---

欢迎关注我的👸号“ **PaperAgent **”， 每天一篇大模型（LLM）文章来锻炼我们的思维，简单的例子，不简单的方法，提升自己。