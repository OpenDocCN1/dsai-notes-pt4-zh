
# Tulip Agent：一种利用增删改查让LLM使用大量工具解决复杂任务的新框架！

> 来源：[https://agijuejin.feishu.cn/docx/PtQRdM7HIoUPV1xZibkc7q7SnEd](https://agijuejin.feishu.cn/docx/PtQRdM7HIoUPV1xZibkc7q7SnEd)
>
> 作者：PaperAgent
>
> 原文：https://mp.weixin.qq.com/s/Pdz65V183bwxRRNxxjNP8g

LLM在工具使用方面仍存在限制，如上下文窗口限制和成本问题：

+ 成本问题：工具描述会占用LLM的上下文窗口，增加推理时间和成本。
+ 注意力和工具限制：从大量工具中选择合适工具对LLM来说是一个挑战，类似于“大海捞针”。
+ 静态性问题：工具使用是静态的，仅限于预先定义的工具，限制了自主智能体的适应性和应用范围。

使用郁金香智能体架构解决一个简单数学问题的示例应用。

![](img/1c6d0b2b46538ac8dbd38c2409db9c43.png)

Tulip Agent（郁金香智能体）架构：允许LLM通过具有创建、读取、更新和删除（CRUD）权限的工具库来解决任务，工作流程：

+ 初始化工具库：自动提取工具信息并创建嵌入向量。
+ 接收用户提示：LLM分解任务并搜索工具库以找到合适的工具。
+ 工具搜索：基于子任务描述，使用语义搜索找到最相关的工具。
+ 工具调用：LLM生成工具调用，工具执行器执行工具并返回结果。
+ 结果反馈：LLM根据工具返回的结果进行进一步操作或响应用户。

郁金香智能体架构及其信息流的概览：大语言模型、工具库、工具本身、搜索模块、功能执行模块以及工具自省模块。

![](img/ed80b74a51cb5ac1cc474973ca7bcb26.png)

### 问题阐述

+ 郁金香智能体被设计为一个拥有大量工具集的LLM支持的自主智能体。
+ 当接收到自然语言查询时，代理需要将任务分解为可使用现有工具解决的子任务序列。

### 工具和工具库

+ 工具定义为可执行的函数，具有特定目的并返回结果或状态消息。
+ 通过代码内省自动提取工具信息，生成嵌入向量并存储在工具库中。
+ 工具库可以是任何支持搜索的数据库，这里使用向量存储来实现语义搜索。

### 任务分解和工具检索

+ 郁金香智能体接收自然语言输入，使用LLM将任务分解为更细粒度的子任务。
+ 通过创建子任务的嵌入向量并匹配工具描述的嵌入向量，来搜索适合的工具。
+ 支持递归分解和搜索工具，如果找不到合适的工具，则进一步细化子任务。

### 工具使用

+ 基于子任务和识别的工具，LLM生成工具调用，由工具执行器执行并返回结果。
+ LLM可以根据前一个调用的返回值来生成后续的工具调用。

### 自主工具管理

+ 提供了`AutoTulipAgent`变体，具备创建、读取、更新和删除工具库中工具的能力。
+ 使用LLM的代码生成能力来创建或更新工具，确保新工具的有效性，并动态加载到工具库中。

### 架构变体

+ 郁金香智能体架构可以与其他LLMznt范式结合使用，例如使用CoT（Chain of Thought）提示来指导LLM按固定方式执行任务。

![](img/d9e827f7700f4baf95f2ff7964a54457.png)

在数学任务上的评估和多项消融实验得出了一些结论：

+ 1) 工具对于解决复杂任务至关重要；
+ 2) 使用工具库显著降低了成本；
+ 3) 任务分解改善了工具的使用；
+ 4) 语言模型的性能影响智能体设计的适用性；
+ 5) 嵌入模型的性能（在当前OpenAI模型的高水平上）对工具检索影响很小；
+ 6) 更好的规划允许更精确地搜索工具；
+ 7) 郁金香智能体架构适合于不断创建工具和即时构建工具库。

在几个数学任务上，几个Agent在成本（以美元计）、工具使用精确度、工具使用召回率以及结果正确性方面的表现。设置：语言模型 gpt-3.5-turbo-0125，嵌入模型 text-embedding-3-large，`top_k = 5`，运行5次。

![](img/76fef19d87c834cac4e5b23dd542ae82.png)

`CotTulipAgent` 在模拟环境中控制一个辅助机器人，协助两个人类（穿红色衣服的 Felix 和穿橙色衣服的 Daniel）在具有不同饮品选择的桌面场景中完成各种任务。

![](img/38f1b1726f501c7b1755c49c347d1136.png)

### 附录

Agent的Prompt：

![](img/d966bfccadc4f459afa8cd5cccb6107d.png)

Tool的定义：

![](img/abe9936699406bb1ffb53ea4a3e51709.png)

`NaiveToolAgent`关于“What is 45342 * 23487 + 32478？”的日志

```
INFO:root:Query: What is 45342 * 23487 + 32478?
INFO:base_agent:Usage for chatcmpl-94zcjnURFRF2nyTMlcVzhWJmfjdTZ in tokens: 3960 prompt and 19 completion.
INFO:base_agent:Function multiply returned `1064947554` for arguments {&#x27;a&#x27;: 45342, &#x27;b&#x27;: 23487}.
INFO:base_agent:Usage for chatcmpl-94zcliQksrQsOMn72GamhLxiJRqN3 in tokens: 3990 prompt and 21 completion.
INFO:base_agent:Function add returned `1064980032` for arguments {&#x27;a&#x27;: 1064947554, &#x27;b&#x27;: 32478}.
INFO:base_agent:Usage for chatcmpl-94zcyDN3oBC8rh5hzFNZkn1iXakLJ in tokens: 4022 prompt and 25 completion.
base_res=&#x27;The result of \\(45342 \\times 23487 + 32478\\) is \\(1064980032\\).&#x27;
```

`CotTulipAgent`关于“What is 45342 * 23487 + 32478？”的日志

```
INFO:root:Query: What is 45342 * 23487 + 32478?
INFO:tulip_agent:Usage for chatcmpl-94zjNuHaG36iH6FtEuE4po0gRhTG4 in tokens: 182 prompt and 25 completion.
INFO:root:actions_response_message=ChatCompletionMessage(content=&#x27;1. Multiply 45342 by 23487.\n2. Add the result of
step 1 to 32478.&#x27;, role=&#x27;assistant&#x27;, function_call=None, tool_calls=None)
INFO:tulip_agent:Usage for chatcmpl-94zjPPlK1ySBo6VZJLFMZhPyE0NJK in tokens: 234 prompt and 13 completion.
INFO:root:Tool search for: {&#x27;action_descriptions&#x27;: [&#x27;multiply two numbers&#x27;, &#x27;add two numbers&#x27;]}
INFO:root:Functions for `multiply two numbers`: [...]
INFO:root:Functions for `add two numbers`: [...]
INFO:tulip_agent:Usage for chatcmpl-94zjRk9IrLzy5QKpfNEs5bsrXCGvp in tokens: 413 prompt and 19 completion.
INFO:tulip_agent:Function multiply returned `1064947554` for arguments {&#x27;a&#x27;: 45342, &#x27;b&#x27;: 23487}.
INFO:tulip_agent:Usage for chatcmpl-94zjTXb58ZHJiNYDugqpwpaeffzzB in tokens: 443 prompt and 21 completion.
INFO:tulip_agent:Function add returned `1064980032` for arguments {&#x27;a&#x27;: 1064947554, &#x27;b&#x27;: 32478}.
INFO:tulip_agent:Usage for chatcmpl-94zjVrSx0sLQN9ZqoI8f4z6JW0eVx in tokens: 475 prompt and 25 completion.
tulip_res=&#x27;The result of \\(45342 \\times 23487 + 32478\\) is \\(1064980032\\).&#x27;
```

```
Tulip Agent – Enabling LLM-Based Agents to Solve Tasks Using Large Tool Libraries
https://hri-eu.github.io/tulip_agent/
https://arxiv.org/pdf/2407.21778
https://github.com/HRI-EU/tulip_agent
```

### 推荐阅读

+ 对齐LLM偏好的直接偏好优化方法：DPO、IPO、KTO
+ 一篇搭建AI大模型应用平台架构的全面指南
+ [RAG全景图：从RAG启蒙到高级RAG之36技，再到终章Agentic RAG！](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)
+ [Agent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)

---

欢迎关注我的👸号“PaperAgent”，每天一篇大模型（LLM）文章来锻炼我们的思维，简单的例子，不简单的方法，提升自己。
