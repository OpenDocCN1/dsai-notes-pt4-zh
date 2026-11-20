
```
from haystack import Document, Pipeline
from haystack.components.retrievers import InMemoryBM25Retriever
from haystack.components.generators import OpenAIGenerator
from haystack.components.builders import PromptBuilder
from haystack.document_stores import InMemoryDocumentStore

class HaystackRAGPipeline:
    def __init__(self):
        self.document_store = InMemoryDocumentStore()
        self.setup_pipeline()

    def setup_pipeline(self):
        """设置RAG管道"""
        # 检索器
        retriever = InMemoryBM25Retriever(document_store=self.document_store)

        # 提示构建器
        template = """
        Given the following information, answer the question.
        Context: {% for document in documents %}
            {{ document.content }}
        {% endfor %}
        Question: {{ question }}
        Answer:
        """
        prompt_builder = PromptBuilder(template=template)

        # 生成器
        generator = OpenAIGenerator(model="gpt-3.5-turbo")

        # 构建管道
        self.pipeline = Pipeline()
        self.pipeline.add_component("retriever", retriever)
        self.pipeline.add_component("prompt_builder", prompt_builder)
        self.pipeline.add_component("llm", generator)

        # 连接组件
        self.pipeline.connect("retriever", "prompt_builder.documents")
        self.pipeline.connect("prompt_builder", "llm")

    def add_documents(self, texts):
        """添加文档"""
        documents = [Document(content=text) for text in texts]
        self.document_store.write_documents(documents)

    def query(self, question):
        """执行查询"""
        return self.pipeline.run({
            "retriever": {"query": question},
            "prompt_builder": {"question": question}
        })
```

## 性能对比与测试结果

### 3.1 测试环境与数据集

我使用了标准化的测试环境对五个框架进行了全面评估：

![](img/54df83d7b6198e66ca270b1648cc8b7e.png)

### 3.2 详细测试代码

```
import time
import psutil
import numpy as np
from typing import List, Dict, Any

class RAGBenchmark:
    def __init__(self):
        self.test_queries = [
            "什么是机器学习？",
            "深度学习的主要应用领域有哪些？",
            "如何优化神经网络的性能？"
        ] * 100  # 扩展到300个查询

    def measure_setup_complexity(self, framework_class) -> Dict[str, Any]:
        """测量搭建复杂度"""
        start_time = time.time()
        try:
            framework = framework_class()
            setup_time = time.time() - start_time
            return {
                "setup_time": setup_time,
                "success": True,
                "complexity_score": self._calculate_complexity_score(framework_class)
            }
        except Exception as e:
            return {
                "setup_time": float('inf'),
                "success": False,
                "error": str(e)
            }

    def measure_query_performance(self, rag_system, queries: List[str]) -> Dict[str, float]:
        """测量查询性能"""
        response_times = []
        memory_usage = []
        cpu_usage = []

        for query in queries:
            # 记录资源使用情况
            process = psutil.Process()
            cpu_before = process.cpu_percent()
            memory_before = process.memory_info().rss / 1024 / 1024  # MB

            # 执行查询
            start_time = time.time()
            try:
                response = rag_system.query(query)
                response_time = time.time() - start_time
                response_times.append(response_time)
            except Exception as e:
                response_times.append(float('inf'))

            # 记录资源使用情况
            cpu_after = process.cpu_percent()
            memory_after = process.memory_info().rss / 1024 / 1024

            cpu_usage.append(cpu_after - cpu_before)
            memory_usage.append(memory_after - memory_before)

        return {
            "avg_response_time": np.mean(response_times),
            "p95_response_time": np.percentile(response_times, 95),
            "avg_memory_usage": np.mean(memory_usage),
            "avg_cpu_usage": np.mean(cpu_usage)
        }

    def _calculate_complexity_score(self, framework_class) -> int:
        """计算复杂度评分（1-5分，5分最复杂）"""
        # 基于代码行数、依赖数量等因素计算
        import inspect
        source_lines = len(inspect.getsource(framework_class).split('\n'))

        if source_lines < 50:
            return 1
        elif source_lines < 100:
            return 2
        elif source_lines < 200:
            return 3
        elif source_lines < 300:
            return 4
        else:
            return 5
```

## 框架选择决策树

基于我的实战经验，我总结了一套RAG框架选择的决策方法：

![](img/199e0d04200e9d7f925f6ba439a12284.png)

## 最佳实践与优化策略

### 5.1 通用优化策略

无论选择哪个框架，以下优化策略都能显著提升RAG系统性能：

```
class RAGOptimizer:
    def __init__(self):
        self.chunk_strategies = {
            "fixed": self._fixed_chunking,
            "semantic": self._semantic_chunking,
            "hybrid": self._hybrid_chunking
        }

    def optimize_chunking(self, documents, strategy="hybrid"):
        """优化文档分块策略"""
        return self.chunk_strategies[strategy](documents)

    def _semantic_chunking(self, documents):
        """语义分块：基于句子相似度分割"""
        from sentence_transformers import SentenceTransformer

        model = SentenceTransformer('all-MiniLM-L6-v2')
        chunks = []

        for doc in documents:
            sentences = doc.split('.')
            embeddings = model.encode(sentences)

            # 基于相似度阈值分块
            current_chunk = []
            for i, sentence in enumerate(sentences):
                current_chunk.append(sentence)

                if i > 0:
                    similarity = np.dot(embeddings[i], embeddings[i-1])
                    if similarity < 0.7:  # 相似度阈值
                        chunks.append('.'.join(current_chunk))
                        current_chunk = []

            if current_chunk:
                chunks.append('.'.join(current_chunk))

        return chunks

    def implement_reranking(self, retrieved_docs, query):
        """实现重排序机制"""
        from sentence_transformers import CrossEncoder

        # 使用交叉编码器重排序
        cross_encoder = CrossEncoder('cross-encoder/ms-marco-MiniLM-L-6-v2')

        pairs = [[query, doc.page_content] for doc in retrieved_docs]
        scores = cross_encoder.predict(pairs)

        # 按分数排序
        scored_docs = list(zip(retrieved_docs, scores))
        scored_docs.sort(key=lambda x: x[1], reverse=True)

        return [doc for doc, score in scored_docs]

    def add_query_expansion(self, original_query):
        """查询扩展：生成相关查询"""
        expansion_prompt = f"""
        基于原始查询："{original_query}"
        生成3个相关的查询变体，用于提高检索召回率：
        1.
        2.
        3.
        """

        # 这里可以调用LLM生成扩展查询
        # expanded_queries = llm.generate(expansion_prompt)
        return [original_query]  # 简化示例
```

### 5.2 性能监控与调优
