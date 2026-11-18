
```
# Build the context for prompt
recall_domain_set = set()
filtered_results = []
for doc, score in results:
    if score > MIN_RELEVANCE_SCORE:
        filtered_results.append((doc, score))
        domain = urlparse(doc.metadata['source']).netloc
        recall_domain_set.add(domain)

    if filtered_results:
        filtered_context = "\n--------------------\n".join([
            f"URL: {doc.metadata['source']}\nRevelance score: {score}\nContent: {doc.page_content}"
            for doc, score in filtered_results
        ])
        context = f"""Documents size: {len(filtered_results)}
Documents(Sort by Relevance Score from high to low):
{filtered_context}"""
    else:
        context = f"""Documents size: 0
No documents found directly related to the current query.
If the query is similar to greeting phrases like ['Hi', 'Hello', 'Who are you?'], including greetings in other languages such as Chinese, Russian, French, Spanish, German, Japanese, Arabic, Hindi, Portuguese, Korean, etc. The bot will offer a friendly standard response, guiding users to seek information or services detailed on the `{site_title}` website.
For other queries, please give the answer "Unfortunately, there is no information available about '{query}' on the `{site_title}` website. I'm here to assist you with information related to the `{site_title}` website. If you have any specific queries about our services or need help, feel free to ask, and I'll do my best to provide you with accurate and relevant answers."
"""
```

### 3.4.3 参考引用

这个更多是一种工具而非检索改进技术，尽管它非常重要。

> 如果我们使用了多个来源来生成答案，可能是由于初始查询的复杂性（我们不得不执行多个子查询，然后将检索到的上下文合并成一个答案），或者因为我们在不同的文档中找到了单个查询的相关上下文，那么就会出现一个问题，即我们是否能准确地反向引用我们的来源。

**有几种方法可以做到这一点：**

1. 将这个引用任务插入到我们的提示中，并要求LLM提及所使用来源的ID。
2. 将生成的响应的部分与我们索引中的原始文本chunk进行匹配。

```
prompt = f"""
Given the documents listed below and the user's query history, please provide accurate and detailed answers in the query's language. The response should be in JSON, with 'answer' and 'source' fields. Answers must be based on these documents and directly relevant to the `{site_title}` website. If the query is unrelated to the documents, inform the user that answers cannot be provided.

User ID: "{user_id}"
Query: "{query}"
User Query History(Sort by request time from most recent to oldest):
{history_context}

{context}

Response Requirements:
- If unsure about the answer, proactively seek clarification.
- Refer only to knowledge related to `{site_title}` website's content.
- Inform users that queries unrelated to `{site_title}` website's content cannot be answered and encourage them to ask site-related queries.
- Ensure answers are consistent with information on the `{site_title}` website.
- Use Markdown syntax to format the answer for readability.
- Responses must be crafted in the same language as the query.

The most important instruction: Please do not provide any answers unrelated to the `{site_title}` website! Regardless of whether the bot know the answer or not! This instruction must be followed!

Please format your response as follows:
{
    "answer": "A detailed and specific answer, crafted in the query's language.",
    "source": ["Unique document URLs directly related to the answer. If no documents are referenced, use an empty list []."]
}
"""
```

### 3.4.5 会话历史

关于构建一个能够针对单一查询多次工作的优秀RAG系统的下一个重大事项是聊天逻辑，这一点与前LLM时代经典聊天机器人中的对话上下文相同。

> 这是为了支持后续问题、指代或与先前对话上下文相关的任意用户命令所必需的。这通过查询压缩技术来解决，同时考虑聊天上下文和用户查询。

### 上下文压缩

一个流行且相对简单的是`ContextChatEngine`，首先检索与用户查询相关的上下文，然后将其连同聊天历史记录从内存缓冲区发送给LLM，以便LLM在生成下一个答案时了解之前的上下文

![](img/c5ff46233c0a1b21d47a990e1484ae27.png)

```
def get_user_query_history(user_id):
    history_key = f"open_kf:query_history:{user_id}"
    history_items = g_diskcache_client.get_list(history_key)[::-1]
    history = [json.loads(item) for item in history_items]
    return history

# Get history session from Cache
history_session = get_user_query_history(user_id)
# Build the history_context for prompt
history_context = "\n--------------------\n".join([f"Previous Query: {item['query']}\nPrevious Answer: {item['answer']}" for item in history_session])
```

## 生成

利用LLM根据用户的查询和检索到的上下文信息生成答案。

### LLM模式

根据场景的不同，LLM的选择可以分为以下两种类型：

云API基础生成器。通过调用第三方LLM的API来使用.

> **优点包括：**
>
> + 没有服务器压力
> + 高并发性
> + 能够使用更强大的模型
>
> **缺点包括：**
>
> + 数据通过第三方，可能引起数据隐私问题
> + 无法调整模型（在绝大多数情况下）

### 3.5.2 响应合成器

这是RAG流程的最后一步，根据我们仔细检索的所有上下文和初始用户查询生成答案。

> 最简单的方法就是将所有获取的上下文（超过某个相关性阈值）与查询一起连接起来，并一次性输入到LLM中。

> 然而，像往常一样，还有其他更复杂的选项，涉及多次LLM调用来优化检索到的上下文并生成更好的答案。

**响应综合的主要方法有：**

1. 通过将检索到的上下文逐chunk送入LLM，迭代地完善答案。
2. 将检索到的上下文进行总结，以适应提示。
3. 基于不同的上下文chunk生成多个答案，然后将它们连接或进行总结。

### 3.5.3 Postprocess Response

如果不是采用流式输出，在获取到LLM生成的结果后，我们可以根据具体业务场景，进行最后干预。

> 比如OpenIM文档网站的网站客服机器人，只需要回答和OpenIM网站相关的问题，如果用户咨询其它问题或者LLM给出了和网站无关的答案，都需要进行结果干预。

![](img/6dff1291945e9b1690e0c7dc2314abb2.png)

![](img/52f0c20b707f58f49017b4ffa4a62c09.png)

```
def postprocess_llm_response(query, answer_json, site_title, recall_domain_set):
    if answer_json['source']:
        answer_json['source'] = list(dict.fromkeys(answer_json['source']))

    is_adjusted = False
    adjust_source = []
    for url in answer_json['source']:
        domain = urlparse(url).netloc
        if domain in recall_domain_set:
            adjust_source.append(url)
        else:
            logger.warning(f"url:'{url}' is not in {recall_domain_set}, it should not be returned!")
            if not is_adjusted:
                is_adjusted = True

    if is_adjusted:
        answer_json['source'] = adjust_source
        logger.warning(f"adjust_source:{adjust_source}")

    if not adjust_source:
        if site_title not in answer_json['answer']:
            adjust_answer = f"Unfortunately, there is no information available about '{query}' on the `{site_title}` website. I'm here to assist you with information related to the `{site_title}` website. If you have any specific queries about our services or need help, feel free to ask, and I'll do my best to provide you with accurate and relevant answers."
            logger.warning(f"adjust_answer:'{adjust_answer}'")
            if not is_adjusted:
                is_adjusted = True
            answer_json['answer'] = adjust_answer
    return is_adjusted
```

# 四、RAG评估

RAG在自然语言处理领域的快速进展和日益普及，将RAG模型的评估推到了LLM社区研究的前沿。

> 评估RAG模型的主要目标是理解和优化其在不同应用场景下的性能。这里主要介绍RAG的主要下游任务、数据集以及如何评估RAG系统。

## 4.1 下游任务

**RAG的核心任务仍然**是问答（QA），包括传统的单跳/多跳QA、多项选择题、领域特定的QA以及适用于RAG的长篇场景。除了QA，RAG还不断扩展到多个下游任务，如信息抽取、对话生成、代码搜索等。

## 4.2 评估目标

> **目前对RAG**模型的评估主要集中在其在特定下游任务中的执行上。这些评估使用适合具体任务的已建立的评估指标。

例如，问答任务的评估可能依赖于EM和F1分数，而事实核查任务通常以准确度为主要指标。

**主要的评估目标包括：**

+ **检索质量：**评估检索质量对于确定检索组件提取的上下文的有效性至关重要。使用来自搜索引擎、推荐系统和信息检索系统领域的标准指标来衡量RAG检索模块的性能。常用的指标包括命中率、MRR和NDCG等。

+ **生成质量：**生成质量的评估关注生成器从检索到的上下文中合成连贯和相关的答案的能力。根据内容的目标，这种评估可以分为无标签内容和有标签内容两种。对于无标签内容，评估包括生成答案的忠实度、相关性和无害性。相反，对于有标签内容，重点是模型生成的信息准确性。此外，检索和生成质量的评估可以通过人工或自动评估方法进行。

综上所述，RAG模型的评估涉及检索质量和生成质量的评估，并使用特定任务的评估指标进行衡量。评估可以采用手动或自动评估方法。

## 4.3 评估方面

RAG模型的评估实践强调三个主要的质量得分和四个基本能力，共同为RAG模型的两个主要目标进行评估：检索和生成。

**质量得分：**质量得分包括上下文相关性、答案忠实度和答案相关性。这些质量得分从不同的角度评估RAG模型在信息检索和生成过程中的效率。

+ 上下文相关性评估检索到的上下文的精确性和特异性，确保相关性并减少与无关内容相关的处理成本。

+ 答案忠实度确保生成的答案与检索到的上下文保持一致，避免矛盾。

+ 答案相关性要求生成的答案与提出的问题直接相关，有效回答核心问题。

**所需能力：**RAG的评估还涵盖了四个能力，这些能力表明其适应性和效率：噪声鲁棒性、负样本拒绝、信息整合和反事实鲁棒性。这些能力对于模型在各种挑战和复杂场景下的性能至关重要，影响着质量得分。

+ 噪声鲁棒性评估模型处理与问题相关但缺乏实质信息的噪声文档的能力。

+ 负样本拒绝评估模型在检索到的文档中不包含回答问题所需知识时的判断能力，避免回答无法回答的问题。

+ 信息整合评估模型从多个文档中合成信息以回答复杂问题的能力。
