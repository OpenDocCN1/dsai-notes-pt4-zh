# 如何免费自动化寻找蓝海市场：用AI Agent+N8N工作流在Reddit上发现真实痛点

> 来源：[https://xuqiwei1986.feishu.cn/docx/Y2kkdmdPFolDJlxkLlKcY4Sunjd](https://xuqiwei1986.feishu.cn/docx/Y2kkdmdPFolDJlxkLlKcY4Sunjd)

# 背景

我和很多小伙伴最近正在参加 **AI Web 大航海活动**，而建站的第一步就是——**找到明确的需求**。

考**虑到我们主要是面向**海外用户出海，而老外的思维方式与我们确实存在一些差异，为了尽快定位他们真实的痛点和需求，我用上了一个超高效的工具：n8n 自动化流程。

大家可以翻阅我之前的帖子（里头有详细教程），可以直接白嫖搭建一个 n8n 的在线工作流平台。

https://scys.com/articleDetail/xq_topic/1524485511885422

![](img/fd7221fa055ec5e2e48aff34b305a474.png)

## 视频讲解一下0成本部署N8N操作平台

![](img/987d6b8165ae2e950315fb01e300a100.png)

[6月2日.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FDZZbsvCUoWuMQxT46kcZJUwn0C)

## 结果

这里是N8N工作流的。

![](img/eaa516bf4cfe9819da6101904b17032e.png)

## 结果数据展示

这里是google sheet的数据

![](img/67613081e68d3a088c582e98cda9520b.png)

## 为什么选择Reddit ?

**Reddit 是一个基**于兴趣和社区的内容聚合与讨论平台，它拥有全球数亿级别的活跃用户，在北美、欧洲及全球英语国家尤为活跃，被称为“美版知乎 + 🧣 + 豆瓣”。

+   💬 用户可以在不同主题的 S**ubreddit（子论坛） **中提问、发帖、投票、讨论

+   👥 内容分类明确：如 r/smallbusiness, r/startups, r/freelance, r/SaaS, r/ecommerce, r/marketing 等等

+   🔍 用户发帖多为求**助、吐槽、经验分享、资源推荐 **→ 真实、高粘性、高痛点

+

在这个流程里，我接入了 Reddit 作为数据源

**通过 n8n + Reddit +** AI 的组合，我们可以自动抓取海外用户的真实问题、生成摘要、判断是否为商业机会，并自动归档进表格，让“找需求”变得不再拍脑袋，全靠数据说话。

# 基本概念了解

## 🟥 1. R**eddit 节点**

### ✅ 作用：从 Reddit 获取数据

+   用于抓取 Reddit 上指定 subreddit 中的帖子（如 r/smallbusiness）。

+   你可以使用关键词进行搜索，例如 “recommendation”, “looking for solution”。

+   支持返回内容包括：发帖时间、点赞数（ups）、正文内容（selftext）等。

👉 目**标：**获取 Reddit 上真**实用户发布的问题或需求作**为数据源。

---

## 🟨 2. I**F 节点**

### ✅ 作用：**筛选符合条件的数据**

+   是一个逻辑判断模块，用于过滤不需要的内容。

+   常见条件：

+   ups > 2：至少有一定点赞数（代表真实需求）

+   selftext is not empty：帖子正文不为空

+   created within 180 days：只保留半年内的数据

👉 目**标：**确保输入的数据是近期、有互动、有实质内容的真实问题。

---

## 🟩 3. A**I Agent 节点**

### ✅ 作用：调用 AI 判断帖子内容是否是“商业问题”

+   使用DeepSeek或其他大模型（如 GPT-4）对 Reddit 帖子进行分类。

+   Prompt 示例：

+   “判断这个 Reddit 帖子是否在描述一个具体商业问题或需求，仅回答 Yes 或 No。”

👉 目**标：**自动识别哪些内容是真的“痛点”或“机会”，跳过聊天、废话、情绪宣泄类帖子。

---

## 🟦 4. M**erge 节点**

### ✅ 作用：**整合多个来源的数据**

+   将来自不同节点（如原始帖子数据、AI判断结果、摘要内容、解决方案）的输出合并成一个对象。

+   可配置字段优先级和数据匹配方式。

👉 目**标：****将“问题 +** 摘要 + AI生成的解决方案”等信息整合成一个完整数据结构，用于后续写入表格或发送通知。

---

## 🟧 5. F**ilter 节点**

### ✅ 作用：**进一步筛选合格数据**

+   通常用于**只保留 AI 判断为 ****yes**** 的结果**。

+   即使 AI Agent 处理完，也要再次确认是否真正命中你的筛选条件。

👉 目**标：**精炼数据，只保留真正值得生成商业方案的帖子内容。

---

## 🟪 6. Summarization Chain 节点

### ✅ 作用：**自动摘要原始帖子内容**

+   使用 AI（如 OpenAI）生成精炼的帖子摘要。

+   适合将 Reddit 上较长的问题描述，压缩为一句话概括。

+   示例输出：

+   “创业者正在寻找跨平台的库存管理方案。”

👉 目**标：**为每个问题提供一个可**视化、快速阅读的概括，**便于整理归档或输入到 Google 表格。

## 🟪 7. G**oogle Sheet**

### ✅ 作用：**保存数据**

# 实操

## 1：**触发启动工作流的节点**

![](img/2e64a10d3879a5f7111ae0f57499e61f.png)

## Reddit 节点（search: post）

第一步需要创建reddit节点，我们需要到reddit上获取数据,Post Action 是 “search for a post”

![](img/64f85845205826630b3dd7cd2976a8ed.png)

这一步需要接入API,就相当于reddit授权给你，相当于给了你一把钥匙，允许你从我的平台获取数据。这里我们记住要填入Oauth Redirect URL , Client ID, Client Secret

![](img/89998c932b1f6926274307c4cbb4ab52.png)

文档参考 https://docs.n8n.io/integrations/builtin/credentials/reddit/?utm_source=n8n_app&utm_medium=credential_settings&utm_campaign=create_new_credentials_modal

我们进入文档链接 点击如图“third-party app”

![](img/622028b043e790ab614a2d186ffb9554.png)

https://www.reddit.com/prefs/apps

要求填入，其中 redirect uri 是 <你的域名>/rest/oauth2-credential/callback

![](img/218a9c5aebdf476a9914ae3ca7942af1.png)

填入之后就会给你Client ID  ,Client Secret ，这里我们红色标注，将得到值回到之前那一步填入即可.

![](img/97d37c49a8625fb8e679886462d39b29.png)

这一步我们需要将reddit设置明明白白的 ，我需要探索需求频道，通过关键词找“look for a sulution”来进行查找。

https://www.reddit.com/r/smallbusiness/  这个社区足够的活跃

![](img/fb18708c5f4e5728e91f0d47aaa5c920.png)

到这里我们先测试一下 看是否返回数据，可以看到有数据。

### 测试

![](img/0afec29c53972fb4a704e7196a9020ef.png)

### 说明

1.  Credential   用于连接你的 Reddit 账户。

1.  Resource：Post   表示你要操作的数据类型是帖子（Reddit Post）。

1.  Operation：Search  表示要进行的是“搜索帖子”的操作。

也可选其它操作，如 Get Post（获取指定帖子）、Submit（发帖）等。

1.  Location：Subreddit  指定搜索范围是某一个具体的 Subreddit（子论坛）。

如：只在 r/smallbusiness 中搜索。

1.  Subreddit：smallbusiness

填你要搜索的目标社区，这里是 r/smallbusiness，是 Reddit 上关于小企业经营的活跃社区。

1.  Keyword：looking for a solution

这是你要搜索的关键词。

示例中填写的是 “looking for a solution”，代表你在查找那些明确表达出老外遇到商业问题、希望得到解决方案的帖子。

1.  Return All：Off

若打开（On）：会返回匹配的所有帖子（不建议，量太大）。

当前关闭（Off）：你只会得到 Limit 指定数量的结果。

1.  Limit：5

返回最多 5 条搜索结果。推荐前期调试设置为 5～10，确认结果准确后再增大。

1.  Additional Fields（附加字段）

如你想按时间/热度排序结果，可以展开这里设置：

Sort: Hot / New / Top / Relevance / Comments

通常建议设置为 Hot，获取最新的火热的问题。

### 结果

![](img/ace75a08d3e4c569c3cfff7a21987633.png)

## 3 IF节点 筛选有效数据

从这里我收集到数据 需要判断筛选

![](img/2d2ff2980c15551dbd8aa6161e1700ea.png)

### 说明

条件1：{{ $json.ups }} is greater than 2

**含义**：这个帖子必须有至少 **3个 upvotes（点赞）**。

**ups**** 是什么？**

ups 是 Reddit API 返回的数据字段，表示帖子当前的点赞数量（upvotes）。

+   类似于🧣的“赞”数量，用户点击“↑”代表觉得这个帖子有价值。

---

条件2：{{ $json.selftext }} is not empty

**含义**：帖子正文不能为空。

**selftext** 是帖子内容主体（不是标题），如果是纯链接帖或空内容就会被过滤掉。

---

条件3：

{{ DateTime.fromSeconds($json.created).toISO() }}

is after or equal to

{{ $today.minus({days: 180}).toISO() }}

+   **含义**：只保留**180天内发布的帖子**。

+   created 是帖子创建的时间戳（秒），通过 .fromSeconds() 转换为 ISO 格式日期

+   $today.minus({days: 180}) 是当前日期往前推180天

还是老惯例，我们调试一下

![](img/bbd58f1fa32a72f24175f8333fb8095e.png)

### 结果

![](img/2d84f2adebbd15f7c2776c10f8a0cc70.png)

## 4.Edit Fields（手动设定字段）

这一步我们需要过滤一下字段 ，发现好多字段，好烦呀，同理我们需要设置一下，重新整理之后的字段 ，我们就清晰多了。

![](img/eba4d2f5a5e24358faf4f226822a9529.png)

### 结果

![](img/77de543ecb37771482f0cf2ee1db7059.png)

## 5.AI Agent（+ DeepSeek Chat Model）

接下就用到我们的AI agent 分析了 ,这里选用我们的deepseek，这里选用的Model是deepseek-chat即可。(也可以选择Open AI)

![](img/d32cac66b0e13533c48d53b27a7fac68.png)

### 说明

+   deepseek-chat

✅ 适合用途：自然语言对话，问答、摘要、翻译，prompt 工程、日常聊天机器人、n8n 工作流中的基本 AI Agent 任务

🎯 特点：

类似 ChatGPT 的风格，偏向“对话体验优化”，回复简洁、速度快，比较适合“判断类型、生成摘要、生成简答型文案”等任务

+    deepseek-reasoner

✅ 适合用途：

数学计算、复杂逻辑推理、代码生成

多步骤推导（例如：链式思维、判断链、因果关系）

🎯 特点：

加强了思维链推理能力（Chain-of-Thought Reasoning）

更偏“研究型”思维，适合长文本推理、分析任务

![](img/7abaa8c154d6f5cf292f89dd9f10428b.png)

这里根据提示词，筛选符合我们要求的Reddit帖子。用Yes或者No进行标注。方便我们后续筛选。

![](img/45b65db23307bcf6246078b642684fd4.png)

提示词是：

Reddit帖子是在描述一个商业相关的问题或者需要一个解决方案。这篇帖子应该提到具体的要求，正尝试积极解决它。

Reddit帖子：{{ $json.selftext }}

这篇文章是一个商业解决方案吗？只输出Yes或No

### 结果

![](img/27646bcf0978b21c97ebde6198e2219d.png)

## 6.Merge

接下来我们将 yes or no的结果 同前面的数据合在一起，需要用到了 merge 节点 ，将 AI Agent 的判断结果 与 原始帖内容合并。

![](img/5156fbf2f7e8f780d4d84d9d253808d4.png)

### 说明

🔹 Mode：Combine  表示采用“合并”模式。

”yes or no的结果“同”前面的数据“合在一起。

🔹 Combine By：Position

表示合并是按位置对应进行的。

📝 举例说明：

输入1（原始帖子）输出了 3 个 item

输入2（AI判断结果）也输出了 3 个 item

Merge 将第1个帖子与第1个判断结果组合成一个对象

🔹 Number of Inputs：2

表示这个 Merge 节点将接收 2 个输入端口的数据。

### 结果

![](img/30cd62e99243789bd10f2fb826f8071a.png)

## 7.筛选

我们最想要的是yes，yes代表这用户需要寻求一种商业解决方案，我们用 “Filter” 节点筛选一下

![](img/d0080542cc658e1975ae3a5090de4dbd.png)

### 结果

![](img/5e32e6754c7dde128c10516908f81659.png)

## 8.Summarization Chain

在 **n8n** 中，Summari**zation Chain **是一个用于 将长文本内容**压缩成简洁摘要 的 AI 节**点，属于 Advanced AI 功能的一部分。你可以用它对 Reddit 帖子、用户评论、产品评价、客服对话 等冗长内容进行总结，用一句话或一小段话清楚表达“这段内容在说什么”。

![](img/f69f44fb23f28d2354c78f35f32d2677.png)

### 说明：

🔹 Data to Summarize：Use Node Input (JSON)

表示从上一个节点传入的 JSON 数据中提取要摘要的内容。

🔹 Chunking Strategy：Simple (Define Below) 指定文本的“分块方式”。

Simple 意味着使用手动设置的字符数进行分段。

用于避免传入大段文本超过 LLM 模型 token 限制，或提升总结准确性。

🔹 Characters Per Chunk：1000

每块文本的最大长度是 1000 个字符。

如果一段 Reddit 帖子超过 1000 字符，就会被分割成多块分别摘要。

🔹 Chunk Overlap (Characters)：200

每个块与下一个块之间重叠的字符数是 200。

好处是避免信息“被切断”，保持语义连贯。

常用于确保段落在分块时不会丢失上下文。

### 结果

![](img/81ca98fad8846ae0b8961d559388fee6.png)

## 9.将数据存到Google Sheets中

我们想将内容存储到文档中

![](img/878f700bb5483c147280865ba04c66a3.png)

我们要想存储到google的文档中 我需要google给我们把钥匙。参考文档https://docs.n8n.io/integrations/builtin/credentials/google/oauth-single-service/?utm_source=n8n_app&utm_medium=credential_settings&utm_campaign=create_new_credentials_modal

![](img/051a1f0304ab44452cc3a9a84c36438c.png)

![](img/88557bbc8535d6c64d0f6604b8e632c5.png)

授权之后 我们需要创建sheet，供N8N输出的数据到这输入

![](img/2e2d665e1db34a93df6b0028719688f3.png)

回到N8N界面我们从Document,From List 就可以看到我们创建的sheet了

![](img/5d54fc51b6c7f41479b077f06aa1a200.png)

我们将之前的数据，拖拽到google对应的字段中 点击右上角的 test step

![](img/01e4f0b5580a867a8ba51d49cbd96d37.png)

还老惯例，我们调试下结果。

![](img/0184c2bbb4718438cf225edbc0558366.png)

这样我们就可以得到老外的想法了。如果需要得到更多的数据 ，修改Reddit节点的limit值，重新触发工作流即可。

![](img/daeb4078b8989bdc289477d3152ab755.png)

### 结果

![](img/0ab39ac9b31b677acfc90ab79e89c4ad.png)

# 结尾

+   希望大家多多反馈，毕竟我身为能熟练操作N8N自动化工作流的人来写教程。

+   还有r/smallbusiness只是来抛砖引玉，如果还有其他想法，也可以寻找对应的频道。

+   文中的N8N是免费部署搭建的，所以没有做汉化处理。

+   由于是海外内容，需要设置网络

+   注意要小步快跑，每一步调试一下。