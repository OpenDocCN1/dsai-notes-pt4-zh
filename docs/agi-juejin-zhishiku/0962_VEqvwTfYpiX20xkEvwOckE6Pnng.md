# 从0开始学RAG之RAG-Fusion

> 来源：[https://agijuejin.feishu.cn/docx/Mbaxd3QUQogC2axlIc5ciWFqnxb](https://agijuejin.feishu.cn/docx/Mbaxd3QUQogC2axlIc5ciWFqnxb)

>

> ji

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/684994205)hihu.com/p/684994...

## [基本原理](https://www.zhihu.com/search?q=%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)

RAG-Fusion可以认为是 MultiQueryRetriever 的进化版，如下图所示，RAG-Fusion首先根据原始question从不同角度生成多个版本的新question，用以提升question的质量；然后针对每个question进行向量检索，到此步为止都是MultiQueryRetriever的功能；与之不同的是，RAG-Fusion在喂给LLM生成答案之前增加了一个排序的步骤。

![](img/f2ee9e4952ca92b16311fdd448f733bd.png)

[排序包含两个动作，](https://www.zhihu.com/search?q=question&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)一是独立对每个 question 检索返回的内容根据相似度排序，确定每个返回chunk在各自候选集中的位置，相似度越高排名越靠前。二是对所有question 返回的内容利用RRF（Reciprocal Rank Fusion）综合排序，RRF排序原理如下图所示。

![](img/d291ee221c7a692d4ab4674c5b1df26e.png)

RRF score的计算公式非常简单：

score=1rank+k score=\frac{1}{rank+k}

其中，rank是按照距离排序的文档在各自集合中的排名，k是常数[平滑因子](https://www.zhihu.com/search?q=%E6%A3%80%E7%B4%A2%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)，一般取k=60。RRF将不同 检索器 的结果综合评估得到每个chunk的统一得分。

## 代码实践

首先，导入必要的packages

```
import torch
from langchain.load import dumps, loads
from langchain.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.vectorstores import Chroma
from langchain import PromptTemplate
from langchain import HuggingFacePipeline
from transformers import AutoTokenizer, pipeline, AutoModelForCausalLM
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.chains.llm import LLMChain
from langchain_core.output_parsers import StrOutputParser
```

然后做一些准备工作，编写加载embedding模型、LLM和文件的函数。

```
def load[_embedding(](https://www.zhihu.com/search?q=torch_dtype&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)embe[d_path):
   ](https://www.zhihu.com/search?q=texts_chunks&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D) embeddings = HuggingFaceEmbeddings(
        model_name=embed_path,
        model_kwargs={&quot;device&quot;: &quot;cuda&quot;},
        encode_kwargs={&quot;normalize_embeddings&quot;: True},
    )
    return embeddings


def load_llm(model_path):
    tokenizer = AutoTokenizer.from_pretrained(
        model_path,
        device_map=&quot;auto&quot;,
        trust_remote_code=True,
        torch_dtype=torch.float16
    )

    model = AutoModelForCausalLM.from_pretrained(
        model_path,
        torch_dtype=torch.float16,
        trust_remote_code=True,
        device_map=&quot;auto&quot;,
    )

    model_pipeline = pipeline(
        &quot;text-generation&quot;,
        model=model,
        tokenizer=tokenizer,
        return_full_text=True,
    )

    llm = HuggingFacePipeline(pipeline=model_pipeline)
    return llm


def load_data(data_file):
    loader = PyPDFLoader(data_file)
    documents = loader.load_and_split()
    text_splitter = RecursiveCharacterTextSplitter(separators=[&quot;。&quot;], chunk_size=512, chunk_overlap=32)
    texts_chunks = text_splitter.split_documents(documents)
    return texts_chunks
```

编写retrieval_and_rank()函数，对query利用similarity_search_with_score()在向量库中检索，返回候选chunk和相应的score。对每个query返回的内容，根据score降序排列，最终得到list[list]结构的候选答案集。

```
def retrieval_and_rank(queries):
    all_results = {}
    for query in queries:
        if query:
            search_results = vectorstore.similarity_search_with_score(query)
            results = []
            for res in search_results:
                content = res[0].page_content
                score = res[1]
                results.append((content, score))
            all_results[query] = results

    document_ranks = []
    for query, doc_score_list in all_results.items():
        ranking_list = [doc for doc, _ in sorted(doc_score_list, key=lambda x: x[1], reverse=True)]
        document_ranks.append(ranking_list)
    return document_ranks
```

编写reciprocal_rank_fusion()函数，利用上面的RRF score计算公式计算每个候选chunk的融合得分。

```
def reciproc[al_rank](https://www.zhihu.com/search?q=doc_str&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)_fusion(document_ranks, k=60):
    fused_scores = {}
    for docs in document_ranks:
        for rank, doc in enumerate(docs):
            doc_str = dumps(doc)
            if doc_str not in fused_scores:
                fused_scores[doc_str] = 0
            fused_scores[doc_str] += 1 / (rank + k)
    reranked_results = [
        (loads(doc), score)
        for doc, score in sorted(fused_scores.items(), key=lambda x: x[1], reverse=True)
    ]
    return reranked_results
```

编写main()函数。指定模型和文件路径，依次加载embedding模型、LLM和文件。创建向量库vectorstore，用于后续检索。

基于prompt模板，利用LLM为query生成3个相关问题。典型实现是把生成的相关问题作为输入调用retrieval_and_rank()函数，也可以连同原始query一起扔给 retrieval_and_rank()函数 ，视实际场景效果验证是否增加原始query。内部排序过的结果用RRF算法再统一计算得分排序。

接下来是常规操作，将 [RRF排序 ](https://www.zhihu.com/search?q=RRF%E6%8E%92%E5%BA%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)后的内容作为上下文，调用LLMChain验证回答的效果。

```
[if __n](https://www.zhihu.com/search?q=%E5%85%AC%E5%BC%80%E5%8F%91%E8%A1%8C%E6%96%B0%E8%82%A1&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)ame_[_ == &quot;__](https://www.zhihu.com/search?q=template&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)[main_](https://www.zhihu.com/search?q=%E4%B8%8D%E9%9C%80%E8%A6%81%E5%9B%9E%E7%AD%94&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)_&quot;:
    data_file = &quot;../data/中华人民共和国证券法(2019修订).pdf&quot;
    model_path = &quot;/data/models/Baichuan2-13B-Chat&quot;
    embed_path = &quot;/data/models/bge-large-zh-v1.5&quot;

    embeddings = load_embedding(embed_path)
    llm = load_llm(model_path)

    docs = load_data(data_file)
    vectorstore = Chroma.from_documents(docs, embeddings)

    template = &quot;&quot;&quot;You are a helpful assistant that generates multiple search queries based on a single input query. \n
        Generate multiple search queries related to: {question} \n
        Output (3 queries):&quot;&quot;&quot;

    prompt_rag_fusion = PromptTemplate.from_template(template)

    generate_query_chain = (
            prompt_rag_fusion
            | llm
            | StrOutputParser()
            | (lambda x: x.split(&quot;\n&quot;))
    )

    query = &quot;公司首次公开发行新股，应当符合哪些条件？&quot;
    queries = generate_query_chain.invoke({&quot;question&quot;: query})
    all_results = retrieval_and_rank(queries)
    reranked_results = reciprocal_rank_fusion(all_results)

    # ----------------- 构造提示模板 ----------------- #
    template = &quot;&quot;&quot;你是一名智能助手，根据上下文回答用户的问题，不需要回答额外的信息或捏造事实。

        已知内容：
        {context}

        问题：
        {question}
        &quot;&quot;&quot;
    prompt = PromptTemplate(template=template, input_variables=[&quot;context&quot;, &quot;question&quot;])
    # ----------------- 验证效果 ----------------- #
    chain = LLMChain(llm=llm, prompt=prompt)
    result = chain.run(context=reranked_results, question=query)
    print(result)
```

## 写在最后

RAG-Fusion虽然可以通过生成多个相关query改善qu[ery质量](https://www.zhihu.com/search?q=%E5%85%B3%E9%94%AE%E5%85%83%E7%B4%A0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)，个人理解对缺少上下文信息或 关键元素 的短query作用比较大，对表述相对完整的query实[属鸡肋。反而可能](https://www.zhihu.com/search?q=%E5%8E%9F%E5%A7%8Bquery&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A684994205%7D)会因为改写query检索到其他角度的信息，造成答案无效信息增多显得冗长，甚至得出错误的答案。此时，加入 原始query 会对生成答案的正确性有一定改善。

RAG各种技巧、方法层出不穷，每种技巧有其适用的场景，需要根据实际应用场景灵活选择、大胆魔改！☺

## 参考资料

+   https://github.com/Raudaschl/rag-fusion

+   https://github.com/langchain-ai/langchain/blob/master/cookbook/rag_fusion.ipynb?ref=blog.langchain.dev

+   https://github.com/langchain-ai/rag-from-scratch/blob/main/rag_from_scratch_5_to_9.ipynb

+   https://arxiv.org/html/2402.03367v2

+   https://safjan.com/rag-fusion-enhancing-information-retrieval-in-large-language-models/