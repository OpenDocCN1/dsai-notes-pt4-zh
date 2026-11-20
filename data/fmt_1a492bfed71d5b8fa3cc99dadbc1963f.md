
# 75% 成本和时间削减：优化微软 GraphRAG 索引的秘密

> 来源：[https://agijuejin.feishu.cn/docx/UYjHdGwxTomkF4xYgaWcvKzengh](https://agijuejin.feishu.cn/docx/UYjHdGwxTomkF4xYgaWcvKzengh)

> 作者：还就可 | 深入LLM Agent应用开发

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/L7WYW3Vet1dL-1Gx7PYQMg)qq.com/s/L7WYW3Ve...

书接上回《 [实战微软新一代RAG：GraphRA](http://mp.weixin.qq.com/s?__biz=MzI0OTAzNTEwMw==&mid=2247484079&idx=1&sn=b79a12567425ef19304bd79667be71a4&chksm=e996e29fdee16b89113018d874b95040c71f3ab8fbde75be8a3687ec1b42dd600577d5ded38b&scene=21#wechat_redirect)G强大的全局理解能力，碾压朴素RAG？ 》，想必大家都知道我最近在测试`GraphRAG`，由于其惊人的理解能力，我一直在探索。当然遇到的问题也比较多，比如`community_reports`不生效，`tokens per minute`和`requests per minute`不生效，全局搜索经常`JSONDecodeError`，以及高昂的成本和时间。这4个问题，我提交了4个PR去修复。本篇将围绕这4个问题和PR说明，最主要的是如何大幅降低成本和时间进行说明，修改很简单，但是精度上升了。

## 1. 社区报告配置失效

因为社区报告通常总结内容较大，如果配置的全局`llm`的`context windows`较小的话，在`community reports`阶段可能会失败。因此我在`settings.yaml`中尝试配置覆盖全局`llm`。

```
community_report:
  llm:
    api_key: ${GROQ_API_KEY}
    type: openai_chat # or azure_openai_chat
    model: qwen2:72b
    model_supports_json: false # recommended if this is available for your model.
    api_base: http://localhost:11434/v1
    max_tokens: 32768
    concurrent_requests: 1 # the number of parallel inflight requests that may be made
    tokens_per_minute: 15000 # set a leaky bucket throttle
    requests_per_minute: 30 # set a leaky bucket throttle
    top_p: 0.99
  ## parallelization: override the global parallelization settings for this task
  ## async_mode: override the global async_mode settings for this task
  prompt: "prompts/community_report.txt"
  max_length: 2000
  max_input_length: 8000
```

这个配置之所以不生效是因为代码中其实采用的`community_reports`，而配置中缺少`s`。因此修改生成模板的代码，即可通过 PR [1] ，此PR已经被合并，拉取主分支使用即可。

## 2. TPM和RPM失效

当你如上面文档配置全局每秒的每分钟最大`Token`数量和每分钟最大请求时候，也会发现不生效。原因是代码中其实读取的变量是`tpm`和`rpm`，通过 PR [2] 可以修复，但是这个被其他人提前修复，因此被关闭了，拉取主分支使用即可。

![](img/8c0e77b8db80b99ff2d0f797d4a9ca7d.png)

## 3. 全局搜索`JSONDecodeError`

当我使用`gemma-9b-it`模型的时候，我使用全局查询询问整个故事在讲什么的时候，我发现它经常报`JSONDecodeError`，但是结果也能输出，只是缺少一半，因为那一半返回的不是`JSON`格式，因此直接被忽略了。

```
poetry run poe query --root . --method global "What is this story mainly tell"
```

如果你看代码，你会发现全局查询调用的 `graphrag/query/structured_search/global_search/search.py` 中的 `search` 方法，其中它会使用文件同目录下 `map_system_prompt.py` 的 `MAP_SYSTEM_PROMPT` 这个`Prompt`提示词。这个`Prompt`非常长，而且我发现`GraphRAG`里的`Prompt`提示词好多都有重复。以下面 `MAP_SYSTEM_PROMPT` 为例，它设定了

+   角色说明`Role`

+   任务目标`Goal`，包含对如何解决任务、输入是什么、输出是什么、以及一系列输出的`JSON`字段解释和小的示例，这个`example`不是整个任务的示例，而是输出中某个`description`的示例。

+   输入的数据表

+   任务目标`Goal`，重复一遍

![](img/de10030b6217c19520730a457eadc928.png)

看过文章《 敲黑板！吴恩达LLM Agent工作流Prompt精华全解析 》的朋友们都知道，一个`Prompt`基本上包含五要素。`GraphRAG`中的`Prompt`中已经包含了任务的方法、任务的输入和输出、没有示例、用户输入，但是没有示例，而且重复任务目标。

+   **解决任务的方法**

+   **任务的输入和输出**

+   任务的`Example`，3到5个左右。

+   **任务的历史纪录，如果有的话**

+   **用户输入的问题。**

我们按照这个提示词优化这个`Prompt`来尝试稳定地输出`JSON`结果。为了加入示例，我将这个`Prompt`拆为`System Prompt`和`User Prompt`，系统`Prompt`就是设定一下角色，然后在`User Prompt`中加入示例。

```
MAP_SYSTEM_PROMPT = """
You are a helpful assistant responding to questions about data in the tables provided.
"""
```

`User Prompt`设定还是按照上述`Prompt`的5个要素来设定，任务设定和输入输出说明和上面截图一致，然后加入完整示例，从用户问题、输入的数据表到输出。然后再次按照这个格式，组织用户的问题和数据表给到`LLM`，使之能够模仿输出。通过这次修改，我发现我的全局查询稳定多了，可以稳定输出`JSON`。其中也有遇到生成的`JSON`回复有问题的情况，但是`JSON`中多了一个引号导致解析错误，看起来是模型过小。具体的 PR [3] 尚未合并，但对于我个人测试来说效果好多了，你如果遇到此类可以尝试一下。此外这个PR我删除了冗余的`Goal`设定，其实局部搜索中也有类似的`Prompt`我觉得都是需要优化的。

```
MAP_USER_PROMPT = """
Generate a response consisting of a list of key points that responds to the user's question, summarizing all relevant information in the input data tables.
...
**Do not list more than 5 record ids in a single reference**. Instead, list the top 5 most relevant record ids and add "+more" to indicate that there are more.

===============
For example:
user question: Is Person X currently under investigation for alleged illegal activities or unethical behavior?
---Data Tables---
| id | title                                | occurrence weight | content | rank |
|----|--------------------------------------|-------------------|---------|------|
| 1  | Allegations against Person X   | 1                 | Allegations of financial misconduct           |   4.0   |
| 2  | Allegations against Person X   | 0.3                 | Allegations of unethical business practices   |   4.0   |
| 3  | Allegations against Person X   | 0.8               | Allegations of workplace harassment          |   3.0   |
| 4  | CEO of company X               | 1                 | Person X is CEO of Company X             |   3.0   |
| 5  | owner of company Y              | 1                 | Person X is the owner of Company Y     |   3.0   |

answer: 
{
    "points": [
        {"description": "Person X is the owner of Company Y and subject to many allegations of wrongdoing [Data: Reports (1, 2, 3, 4)].", "score": 85},
        {"description": "He is also CEO of company X [Data: Reports (6)]", "score": 75}
    ]
}

==============
user question: {user_question}

---Data tables---
{context_data}

answer: 
"""
```

## 4. 大幅缩短索引时间和成本

有很多朋友在我的文章《 [实战微软新一代RAG：GraphRA](http://mp.weixin.qq.com/s?__biz=MzI0OTAzNTEwMw==&mid=2247484079&idx=1&sn=b79a12567425ef19304bd79667be71a4&chksm=e996e29fdee16b89113018d874b95040c71f3ab8fbde75be8a3687ec1b42dd600577d5ded38b&scene=21#wechat_redirect)G强大的全局理解能力，碾压朴素RAG？ 》中评论索引时间过长，动辄几十分钟到几个小时。我在尝试优化`Prompt`时候我发现，实体关系提取的`Prompt`非常大，大约 `2224` `tokens` ，位于 `prompts/entity_extraction.txt` 。它的调用次数和文本分割的大小有关系。每个分割的文本，都需要使用 `entity_extraction` 提取一次实体关系。而`GraphRAG`设定的默认`chunk size`为`300`，`overlap`是`100`，这意味着什么？《仙逆》前10章大约26000个字符，按照这种方式分割，大约在实体提取阶段就需要65万`Tokens`，当然这只是一个粗略的估算，实际没有那么多，但也不会相差太远。

```
26000/(300-2*100)=260 chunks
260 * (2224+300) = 656240 tokens
```

所以两个思路，一是精简实体提取的`Prompt`，二是增加`chunk size`。前者可以考虑缩减`Prompt`的示例为一个，删除其他几个。后者可以参考`LLamaIndex`，它的默认分割大小为`1024`，`overlap`为`20`，因此我改变`settings.yaml`中这项参数，重新进行`index`。

```
chunks:
  size: 1024
  overlap: 20
  group_by_columns: [id] # by default, we don't allow chunks to cross documents
```

我只等待[了大约5分-7分左右就全部完成了，而](http://mp.weixin.qq.com/s?__biz=MzI0OTAzNTEwMw==&mid=2247484079&idx=1&sn=b79a12567425ef19304bd79667be71a4&chksm=e996e29fdee16b89113018d874b95040c71f3ab8fbde75be8a3687ec1b42dd600577d5ded38b&scene=21#wechat_redirect)这在此之前起码半小时起步。我再次查看`Groq`上我消耗的`Token`数量，惊喜的发现`Token`消耗大幅下降，之前所需54万`Token`，而现在只需要151640，大约下降了3/4，时间也变得可以接受。请求数量从283到106，下降了大约63%。你或许会问，精度牺牲了吗？我们仍然以《 实战微软新一代RAG：GraphRAG强大的全局理解能力，碾压朴素RAG？ 》中使用的问题为例进行说明。

![](img/afc5059db86ceaa2b3bcc9f303894c78.png)

问题1：这个故事主要讲述了什么？

> SUCCESS: Global Search Response: 这个故事主要讲述了王家内部的复杂冲突。

> 冲突的核心是继承争端、个人恩怨以及不同成员的抱负。王卓表现出一种优越感，与铁柱存在冲突，因为他对铁柱的评价很低，并渴望被认可为更强大的修炼者 [Data: Reports (2)]. 王天山与王卓站在同一阵营，对抗铁柱，这表明他们之间存在着共同的敌意或策略联盟 [Data: Reports (2)].

> 铁柱的家庭也面临着内部矛盾和外部压力。他的父亲与王天山有冲突，而他的叔叔则试图缓和局势 [Data: Reports (1)].

> 总的来说，这个故事展现了家族内部的权力斗争、个人野心以及利益冲突，最终导致了复杂的人际关系和紧张的局势。

使用局部查询：王林的主要经历

> SUCCESS: Local Search Response: # 王林的主要经历

> 1 出生与成长 [Data: 第一卷 平庸少年 (1)]

+   **出生背景**：王林，小名铁柱，出生于一个木匠家庭，在周围几个村落中属于大姓“王”氏。他的父亲是家族庶出次子，定居在一个村庄，以精湛的木匠手艺维持小康生活。

> 2 教育与梦想 [Data: 第一卷 平庸少年 (1)]

+   **教育经历**：从小聪明好学，喜欢读书，被村里人誉为神童。父母对他的期望很高，让他在家专心读书，而不是像其他孩子一样下地干活。

> 3 离家出走 [Data: 第一卷 平庸少年 (1)]

+   **离乡原因**：王林对山村外的世界充满向往，但因未能被恒岳派选中成为弟子而感到失望。他渴望改变命运，追求更高的学识和更广阔的世界。

> 4 恒岳派的遭遇 [Data: 第一卷 平庸少年 (1)]

+   **测试失败**：王林参加了恒岳派的入门测试，但因灵根不足未能被选中，这让他深感挫败。

> 5 家人的寻找与回归 [Data: 第一卷 平庸少年 (1)]

+   **家人的担忧**：离家出走后，家人非常担心他的安危。通过家族联名上访恒岳派，最终在附近的一处洞穴中被找到。

> 6 重返恒岳派 [Data: 第一卷 平庸少年 (1)]

+   **意外的转机**：虽然最初测试失败，但王林因一次意外被再次带到恒岳派。这次经历可能为他提供了重新评估的机会。

> 通过这些主要经历，我们可以看到王林从一个渴望知识和改变命运的孩子成长为面对挫折、家庭责任与个人梦想之间挣扎的年轻人。他的故事反映了追求理想与现实之间的冲突，以及在逆境中寻找希望的勇气。

> 7 结论

> 王林的故事是关于成长、失望与希望的交织。尽管面临挑战，他仍然保持着对更广阔世界的渴望和探索精神。

+   问题2：谁是王林 他的主要人际关系是什么样

> SUCCESS: Local Search Response: ## 王林的身份与背景

> 王林，本名铁柱，是中国网络小说《仙逆》中的主角。他出生于一个平凡的山村家庭，自幼便对修仙充满了向往和渴望[Data: Characters (1)]。

> 1 **主要人际关系**

> 1.1 **家庭成员**
