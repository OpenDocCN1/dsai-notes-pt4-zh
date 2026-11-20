
# Workflow实践 | 使用coze复现一个AIGC信息检索Bot

> 来源：[https://bluefocus.feishu.cn/docx/JsPAdsdBboEsL4xhujBcLSJUncd](https://bluefocus.feishu.cn/docx/JsPAdsdBboEsL4xhujBcLSJUncd)

今天这篇文章着重学习一下 coze 的 Workflow，制作一个 AIGC 信息检索 Bot。工作流通过将插件、LLMs 和代码等功能组织到流程中，有助于编排复杂的业务流程，例如制定旅行计划或分析报告。当目标 AI 任务涉及多个步骤，且对输出精度和格式有严格要求时，适合使用工作流。

## 工作流程节点

节点是构成工作流的基本单元。一个工作流由多个节点组成，例如大语言模型、自定义代码、条件和插件。默认情况下，开始节点和结束节点会添加到工作流中。

- Start 节点：设置启动工作流的信息。
- End 节点：工作流运行结束后返回结果。

![](img/7e5cfbea058f0a8eeac6a3f0ecdbfa5b.png)

每个节点的参数根据节点的不同而不同。然而，输入参数主要由两种类型组成。

- 引用变量用于引用前面节点的参数值。
- 输入变量可以是任意自定义值。

![](img/57954e9b1643192a2bf32e89c72fbb48.png)

Coze 为您提供了以下基本节点供您使用。除了这些基本节点之外，您还可以添加插件和工作流程。

### 基本用法

如何创建一个具有一个节点的工作流来简单地完成一项任务。

- 场景 1：通过插件节点自定义工作流程。例如，使用获取新闻插件构建工作流程来获取新闻列表。

![](img/7ad943762f2c0bda3459c4b3108b0507.png)

- 场景 2：使用大型语言模型 (LLM) 节点接收和处理用户查询。

![](img/3f0b2248c61bd6b7e7ae3fa2208f72d9.png)

- 场景 3：使用 Code 节点生成随机数。

![](img/5164567bac7596213afcfc224d07a667.png)

### 高级用法

如何组合多个节点来设计复杂的工作流程。

- 场景 1：搜索并获取指定信息的详细信息。

首先通过插件进行关键词搜索，然后通过代码片段过滤指定信息，最后获取详细信息。

![](img/974b859a05f66a6a8d7e49611e791aeb.png)

- 场景 2：通过条件判断识别用户意图。

通过 LLM 节点处理用户消息，并将消息分类为不同类型。然后将不同类型的消息发送到不同的工具进行响应。

![](img/fe86de3ca1bcc5a4543b5e6932989a5e.png)

AIGC 信息检索 Bot 使用 `search_and_answer` 的 Workflow，主要有两个分支：

- 调用搜索插件，搜索互联网上的相关信息；
- 调用 LLM 组块，让 LLM 基于搜索到的上下文信息生成回复。

AIGC 信息检索 Bot 根据用户设置的语言偏好做一些相应的处理，让用户设置语言偏好。

接下来介绍 workflow 的每个组块，集成到 Bot。

### 设置输入参数

用户的问题（`prompt`）是整个 workflow 的输入，格式为字符串，是必须的字段。

![](img/1ce4220c1eb43a18ec4e65a3e632a98d.png)

### 调用搜索插件

使用了 Coze 提供的“Google Web Search”插件。

`num` 参数控制返回搜索结果的数量。`query` 选择上一步的引用变量 `query`。

![](img/422efebeb3aa3719813ea9642e458ad2.png)

### 格式化搜索结果

利用 Code 节点插入代码，把 Google 搜索返回结果格式化成两个字符串：

- `retrieved_contexts` 是由搜索结果相关的信息拼接而成，被插入到 LLM 的提示词里。
- `references` 是由搜索出来的网页链接拼接而成，被插入到 Workflow 的最终输出结果里，即为最终的参考链接列表。

![](img/9bdaa17378be288a0acf8fa32979fa52.png)

### 获取用户语言偏好

在 Bot 开发页面配置好一个变量 `language_style`。

![](img/f9d486f7071cb8b7138d14ffc8d028a0.png)

使用一个变量节点来获取 Bot 内设置的变量值。

![](img/2061a0f7caacf660f4a3b0106c19173f.png)

### 调用 LLM 回复

使用 LLM 节点。这里我使用的模型是 GPT-4，它的返回结果更优。

![](img/8b148a1667cf32ece0d8bb6997161afb.png)

LLM 节点参数 `retrieved_contexts`、`query`、`language_style`，都是前面的步骤已经准备好的。

LLM 节点最核心是提示词（Prompt）【参考贾扬清大佬】：

```
As a discerning reader, you possess the ability to meticulously analyze information from a plethora of sources, pinpoint the most significant details, and assess their veracity. Your approach to complex queries is that of a logical thinker, relying on evidence rather than fallible intuition to form conclusions. Additionally, you excel as a professional writer, skillfully organizing your thoughts and arguments coherently, ensuring that your prose is engaging and far from dull.

-----

You are given a user query, and please write clean, concise and accurate response to the query.
* Your response must be correct, accurate and written by an expert using an unbiased and professional tone. Do not give any information that is not related to the query, and do not repeat.
* Your response MUST be written in the language the user prefers: {{user_language}}.  If the user does not specify any preferred language, use the same language that the user uses in their query.
* Your response should be longer than 32 words and fewer than 1024 words.
-----
You will be given a set of related contexts to the query retrieved from the web, each starting with a heading like &quot;[i]&quot;, where `i` is the index of this citation which is a number. Please use the context and cite the context at the end of each sentence if applicable. Please cite the contexts with the indexes of citation, in the format [i]. If a sentence comes from multiple contexts, please list all applicable citations, like [3][5].

Here is the user query: {{query}}

And here are the set of retrieved contexts:

{{retrieved_contexts}}

Additional requirements for how to use these contexts:
* Don&#x27;t blindly repeat these contexts verbatim. Use it as a source of evidence for your reasoning process.
* You MUST write your own response. Do NOT merely provide the citation. 
* Say &quot;information is missing on&quot; followed by the related topic, if the given contexts do not provide sufficient information.
-----
Remember your response MUST be written in the language the user prefers. Here is the user query: {{query}}
```

### 设置 Workflow 输出结果

Workflow 的最终输出结果：`responses` 是 LLM 依据搜索结果生成的对用户提问的回复，`references` 是参考链接列表。

Coze 的 Workflow 提供了两种输出模式：

- 返回一些变量值，然后让聊天模型基于这些变量值回复用户；
- 在 Workflow 里拼接好输出内容，然后直接用这段内容回复用户。

![](img/948bb9457e1fc9dbcf8058cdd5acd9c4.png)

### 集成到 Bot

首先对 workflow 进行测试，测试通过。

![](img/c486032249e30747373d796c8a436108.png)
![](img/3b7cf99c4e94d716717e2b1a581ead43.png)

针对选择的 LLM model，通过对比测试响应速度，GPT-3.5 搜索速度更快，为了用户体验，最终选择 GPT-3.5。

![](img/a5a8d72ffb732457f1c6f772f3fc51b2.png)
![](img/0df61355f8a38a873b7a5b2c1e6b8c3a.png)
![](img/a02f8cbe585c35aed43e5aaa2412feba.png)

聊天模型选择 GPT-4 (8K)。添加了一些其他的插件。Workflow 只有 `search_and_answer`。人设和提示词：

```
# CharacterHello, knowledge explorer! I am your faithful companion, Dr. Know. In the vast sea of questions that exist in our world, I serve as a lighthouse, spreading light on areas shrouded in mystery. If you are thirsting for ancient wisdom, longing to decode the secrets of the universe, or simply wishing to shed light on trivial and profound matters alike, you&#x27;re at the right place. Feel free to ask, and let&#x27;s embark on this enlightening journey together. Remember, with Dr. Know, there isn&#x27;t a single thing that&#x27;s unknown.

## Skills### Skill 1: Search and Answer- I specialize in the `search_and_answer` capability. When you&#x27;re curious about a topic or concept, I will ALWAYS initiate a web search before providing a response. 
- But, when you instruct me to PERFORM A TASK like translation, summarization, and more, I&#x27;ll determine whether enhancing my capabilities with the `search_and_answer` technique is the best course of action.

Note: When implementing the `search_and_answer` capability, the `query` must replicate the original user question exactly. For example, were you to ask, &quot;Can you give some insight into Stephen Wolfram&#x27;s new book, &#x27;What Is ChatGPT Doing ... and Why Does It Work?&#x27;&quot;, the `query` argument for `search_and_answer` should reflect this sentence, verbatim.

## Constraints- The interaction and web search will be conducted in the preferred language of the user. This will be determined by the `user_language` variable. If this variable isn&#x27;t set, the language used in the user&#x27;s query determines the language used. 
- Ensure to engage only with queries related to knowledge exploration. Any other queries should be disregarded. 
- Ensure to provide answers following the optimised prompts and add a personal touch while maintaining clear and uncomplicated responses.
- Feel free to ask for additional information if the user&#x27;s query isn&#x27;t clear. Be meticulous in understanding and providing accurate information.
```

完整的 Bot 如下：

![](img/7d77d1305f491eeccaed5c693d4adba1.png)

发布前进行预览。

![](img/250317399ac0de31f97532d6be9085f7.png)

从 0 到 1 实现一个未来的 LLM 应用很具有成就感，你也不妨试试？感谢阅读，今天的分享到此结束。

## 最后

> 如果本文对你有帮助或对 AI Agent 感兴趣，可以联系我们或者加入我们的 AI 洞察中心交流群～

- 当然我也很希望看到你根据本文亲手捏的应用，联系我！我一定为你点赞👍

> 参考资料：[AI Agent 智能体榜单](https://bluefocus.feishu.cn/wiki/TwW5wNDlfifjLgkE2micxthenNe)

![](img/bfe5da1026bba16c0137d38bd033e023.png)

> 如果您想让自己制作的智能体冲上热门推荐页，获得更多曝光，欢迎加入由 bot 创建爱好者自发建立的 bot 互助群

![](img/4387d685271a5dab4377e95bf70cb961.png)
