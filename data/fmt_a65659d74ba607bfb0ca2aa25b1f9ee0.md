
# 混合语言文本分块与检索测试

文本分块时的语言检测：我们会计算文本中中文和英文的比例。如果中文占比高，就认为它是中文文本；如果英文占比高，就认为它是英文文本。

分词时的语言检测：只要文本中有中文，无论英文占比多少，我们都会将其视为中文文本，并使用中文分词器。

## 测试使用

```python
# 混合语言测试
mixed_corpus = [
    "this is a sample document about 机器学习",
    "machine learning is fascinating and useful",
    "这是一个关于深度学习的样本文档",
    "another sample about 人工智能"
]
mixed_query = "机器学习"

# 创建并保存为 JSON
bm25_mixed_json = create_bm25(mixed_corpus, 'mixed')
bm25_mixed_json.save(os.path.join(output_dir, 'bm25_mixed.json'))

# 从 JSON 加载并搜索
loaded_bm25_mixed_json = load_bm25(os.path.join(output_dir, 'bm25_mixed.json'), mixed_corpus)
print("混合语言查询（JSON加载):", mixed_query)
results_json = loaded_bm25_mixed_json.search(mixed_query, top_k=3)
for doc_id, score in results_json:
    print(f"文档ID: {doc_id}, 得分: {score:.4f}, 文本: {mixed_corpus[doc_id]}")

# 创建并保存为 Pickle
bm25_mixed_pkl = create_bm25(mixed_corpus, 'mixed')
bm25_mixed_pkl.save(os.path.join(output_dir, 'bm25_mixed.pkl'))

# 从 Pickle 加载并搜索
loaded_bm25_mixed_pkl = load_bm25(os.path.join(output_dir, 'bm25_mixed.pkl'), mixed_corpus)
print("\n混合语言查询（Pickle加载):", mixed_query)
results_pkl = loaded_bm25_mixed_pkl.search(mixed_query, top_k=3)
for doc_id, score in results_pkl:
    print(f"文档ID: {doc_id}, 得分: {score:.4f}, 文本: {mixed_corpus[doc_id]}")
```

```
混合语言查询（JSON加载）: 机器学习
文档ID: 0, 得分: 2.0847, 文本: this is a sample document about 机器学习
文档ID: 2, 得分: 0.5458, 文本: 这是一个关于深度学习的样本文档
文档ID: 1, 得分: 0.0000, 文本: machine learning is fascinating and useful

混合语言查询（Pickle加载）: 机器学习
文档ID: 0, 得分: 2.0847, 文本: this is a sample document about 机器学习
文档ID: 2, 得分: 0.5458, 文本: 这是一个关于深度学习的样本文档
文档ID: 1, 得分: 0.0000, 文本: machine learning is fascinating and useful
```

从上面我们可以看到中英文混合的文本块也能实现对应的检索并排序了，如果用英文的BM25检索器就没有办法检索到 this is a sample document about 机器学习，总的来说，我们通过上面的改进处理，初步实现了混合语言的BM25检索算法，当然他也同样存在一些局限性，比如中英文意思一样是没有办法BM25算法检索到的，只能是相同的词才能被检索到。

当然，我们可以尝试将文本翻译一下，但是这里就会得不偿失了，因为翻译本身就不好做，而且翻译的词语也不一定和目标的一致，所以呢，不是很好的解决方案。

所以，业界还有另外一种处理的办法，也就是向量检索，向量检索在通过训练得到不同语言之间的关系，能够建立正确的联系，所以可以消除语言之间的壁垒，但是由于向量检索需要引入另外一个模型，我上面说过了，会提升系统的复杂性，所以不引入，这里提到只是让大家知道有这个东西。

## 🦆三、构建简易 RAG 系统

我们的RAG系统主要分成三个部分，检索前的数据提取模块、文本分块模块，其中文本分块只有markdown分块功能，检索时的BM25检索，检索后的生成部分由deepseek这类大模型提供支持。

![](img/d0d07c87bc4d9c18316fe92baeea4230.png)

### 3.1 数据提取模块：支持多种格式，灵活应对不同需求

RAG系统的第一步是数据提取，我们支持PDF和TXT两种常见文件格式。特别是针对PDF文件，我们提供了三种不同的提取方式，满足不同场景的需求：

+   `PyMuPDF`：适用于常规PDF提取，速度快（超级快）。
+   `PyMuPDF4LLM`：专为大型语言模型优化，提取的文本更加结构化，便于后续处理。
+   `MinerU`：针对复杂PDF文件，能够更精准地提取文本和布局信息，缺点是速度慢，性能要求高，具体来说，如果使用`cpu`运行，10s大概是处理一个页面。

### 3.2 文本分块模块：Markdown分块，提升信息处理效率

上面已经提到过了，我们的系统目前只支持markdown格式的文本进行分块。主要是由于最终的内容都会转换成markdown格式，目前我们也没有复杂的分块需求，先简单实现一个，后续再采用类似“迟分”等等的高级分块模式。或者说使用小模型进行分块，但是由于那些操作比较高级了，我们暂时先不接触他们，先从基础的开始。虽然langchain等的一些框架也支持很多种分块的方式，但是为了我们说好的原则（不使用langchain这种框架构建一个极简的RAG系统），所以只能手动实现了。。。

后续我们也会慢慢引入其他的一些分块策略，但是这里先暂时这样吧。

至于为什么要进行分块，是由于大模型的对话上下文空间是有限制的，比如早期的模型，类似chatgpt，chatglm等模型，对话上下文只有4K、8K这些，虽然现在已经有类似128K、1M上下文的模型了，但是总的来说，还是有限制的，还有一点是，模型对话的上下文越长，模型输出内容的速度就会越慢，忠实度也会下降。所以我们给模型提供干净的对话上下文进行输出是有必要的。进行分块的目的就是让我们检索的时候能够检索回来与问题相关的文本块。

### 3.3 检索生成模块：基于BM25算法的文本块召回与大模型内容生成

上面我们说过RAG应用的核心其实在于检索，能否检索到相关文本直接决定了大模型生成内容的质量高低。

我们这里实现仅仅依赖BM25检索算法检索其实效果不会太好，因为我们的提问当中不一定能够包含对应内容的关键词，所以其实检索效果总体来说命中率会比较低，但是内容一旦命中有效率是很高的，相比向量检索而言。后续我们会实现两者结合的混合检索器，好奇的同学可以先自己探索下如何实现两者之间的混合排序哈！

生成模块主要是指使用大模型对检索会来的内容进行总结或者描述性质的回答。

这个阶段只需要结合提示词让模型参考检索回来的内容进行回答。具体的提示词可以参考如下内容：

```python
"""
<system>
    [当前日期] {CURRENT_DATE}
    [角色] 您是一名专业的文档分析助理，擅长从技术文档中提取关键信息
    [背景] 
    {file_info}
    用户将会向你提问关于这些文档的问题，系统将会根据问题检索对应的文本块内容，并拼接成参考信息文档。
    [参考信息文档]
    【{retrieved_info}】
    [核心任务]
    1. 当用户提问时，优先从参考信息文档中寻找答案
    2. 对复杂问题进行分步骤思考
    3. 不要生成参考信息文档没有提供的信息
    4. 保持专业且易懂的语气
    5. 不确定时，可以请求用户提供更多信息
    [交互要求]
    - 保持专业且易懂的语气
    - 关键数据用**加粗**显示
    - 代码块使用```包裹

</system>

"""

```

langchain默认提供给RAG使用的提示词也可以简单参考下，提示词如下：

```
You are an assistant for question-answering tasks. Use the following pieces of retrieved context to answer the question. If you don't know the answer, just say that you don't know. Use three sentences maximum and keep the answer concise.

Question: {question}

Context: {context}

Answer:

---

翻译版本：

你是一个用于回答问题的助手。请使用以下检索到的内容来回答问题。如果你不知道答案，只需说明你不知道。回答请保持简洁，不超过三句话。

问题：{问题}

上下文：{上下文}

答案：

```

应用内检索代码逻辑实现部分：

```python
def retrieved_information(query=""):
    """
    功能描述: 根据已处理的文件内容生成参考信息文档。

    参数:
        file_metadata: 文件元数据
        processed_content: 已处理的文件内容

    返回值:
        str: 生成的参考信息文档字符串
    """
    uploaded_content = st.session_state.uploaded_content

    if not uploaded_content:
        return ""

    documents = []

    for u in uploaded_content:
        content = u.get("content")
        # metadata = u.get("metadata")
        initial_chunks = split_markdown_by_headers(content)

        if not initial_chunks:
            print("有未提取文件出现")
            continue

        # 测试自动检测语言
        merged_chunks_auto = merge_markdown_chunks(initial_chunks, 
                                                   chunk_size=1000, 
                                                   chunk_overlap=100)

        # {header:"",content:"",level:""}

        # todo 给块上面都添加序号，方便后续进行同文件内容重排序

        for chunk_dict in merged_chunks_auto:
            # d = Document(content=chunk_dict.get("content"),metadata=metadata)
            # documents.append(d)
            # todo 将document换成Document类
            documents.append(chunk_dict.get("content"))

    results = bm25_search(corpus=documents,query=query,top_k=20)

    # 将去掉分数，提取序号
    relate_texts =[doc for doc_id, score,doc in results]

    retrieval_context_length = st.session_state.retrieval_context_length

    # 根据检索内容长度限制获取合适数量的文本
    selected_texts = []
    current_length = 0

    for text in relate_texts:
        content_length = len(text)
        if current_length + content_length <= retrieval_context_length:
            selected_texts.append(text)
            current_length += content_length
        else:
            break

    # 将选中的文本内容拼接成字符串返回
    if selected_texts:
        print("检索到的文本内容：",selected_texts)
        return "---" * 5 +"\n\n".join(text for text in selected_texts)
```
