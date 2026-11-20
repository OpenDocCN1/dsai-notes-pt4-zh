
# 建筑文档智能审查系统

## 误差分析模块

```python
class ErrorAnalyzer:
    def __init__(self, llm):
        self.llm = llm
    
    def analyze_errors(self, document_chunk: str, query: str, retrieved_knowledge: List[str]) -> Dict[str, Any]:
        
        analysis_prompt = f"""
Task: Your task is to conduct an error analysis on a given review document, based on a provided review query and relevant reference specifications. This analysis MUST strictly adhere to the provided reference and focus specifically on reviewing and analyzing the original descriptive sections within the review document.

Review document: {document_chunk}
Query: {query}
Reference: {chr(10).join([f"{i+1}. {ref}" for i, ref in enumerate(retrieved_knowledge)])}
Analysis:
"""
        
        analysis = self.llm.predict(analysis_prompt)
        
        return {
            "analysis": analysis,
            "reference_support": retrieved_knowledge
        }
```

## 修订建议生成

误差辨析模块完成后，系统将输出标记偏差区域以及相关知识佐证。随后，系统进入修订策略生成模块。该模块依据误差分析结果和知识参考，对标记区域进行针对性的修订建议生成，最终实现对原文的知识驱动型自动修正。

```python
class RevisionGenerator:
    def __init__(self, llm):
        self.llm = llm
    
    def generate_revisions(self, document_chunk: str, analysis: Dict[str, Any]) -> Dict[str, str]:     
        revision_prompt = f"""
Task: Your task is to review and revise the provided document based on the given analysis and corresponding reference specifications. STRICT adherence to the provided reference specifications is required. If the review document aligns with the analysis and reference specifications WITHOUT discrepancies, revision is not necessary.

Review document: {document_chunk}
Analysis: {analysis['analysis']}
Reference: {chr(10).join([f"- {ref}" for ref in analysis['reference_support']])}
Revision:
"""
        
        revision = self.llm.predict(revision_prompt)
        
        return {
            "original_text": document_chunk,
            "revision_suggestions": revision,
            "modified_regions": analysis.get("error_regions", []),
            "confidence": self._calculate_confidence(analysis)
        }
    
    def _calculate_confidence(self, analysis: Dict[str, Any]) -> float:
        ref_count = len(analysis.get("reference_support", []))
        error_count = len(analysis.get("error_regions", []))
        
        confidence = min(0.9, 0.5 + (ref_count * 0.1) + (error_count * 0.05))
        return confidence
```

## 完整审查流程

将上述所有模块整合，形成完整的文档审查流程。系统首先生成审查问题，然后进行知识引导检索，接着执行错误分析，最后生成修订建议。

```python
def complete_review_process(document_chunk: str, 
                          gkgr_framework: GKGRRetriever, 
                          error_analyzer: ErrorAnalyzer,
                          revision_generator: RevisionGenerator) -> Dict[str, Any]:    
    review_queries = generate_review_queries(gkgr_framework.llm, document_chunk)
    
    results = {}
    for query in review_queries[:3]:
        retrieved_docs, augmented_query = gkgr_framework.retrieve(query)
        
        knowledge_refs = retrieved_docs
        analysis = error_analyzer.analyze_errors(document_chunk, query, knowledge_refs)
        
        revision = revision_generator.generate_revisions(document_chunk, analysis)
        
        results[query] = {
            "retrieved_knowledge": retrieved_docs,
            "augmented_query": augmented_query,
            "analysis": analysis,
            "revision": revision
        }
    
    return results
```

至此，我们就完成了建筑文档智能审查系统的核心实现。

## 实际应用示例

让我们通过一个完整的示例来展示系统的使用：

```python
# 初始化系统组件
llm = DeepSeekLLM(
    model_name='deepseek-chat',
    api_key='your-api-key',
    base_url='https://api.deepseek.com/v1'
)

embedding = BGEEmbedding(model_name="BAAI/bge-m3")
key_extractor = KeyInfoExtractor(llm)

# 从markdown文档构建知识库
processor = DocumentProcessor()
documents = processor.load_documents("./construction_standards")

# 对文档进行动态语义分块
chunker = DynamicSemanticChunker()
knowledge_base = []
for doc in documents:
    chunks = chunker.split_text(doc)
    knowledge_base.extend(chunks.values())

# 初始化检索器
gkgr_retriever = GKGRRetriever(
    knowledge_base=knowledge_base,
    embedding_model=embedding,
    key_info_extractor=key_extractor,
    llm=llm
)

# 初始化分析器
error_analyzer = ErrorAnalyzer(llm)
revision_generator = RevisionGenerator(llm)

# 待审查的文档内容
sample_document = """
钢筋混凝土柱的施工应符合以下要求：
1. 混凝土强度等级不低于C25
2. 钢筋保护层厚度为25mm
3. 混凝土浇筑应连续进行，间歇时间不超过1小时
4. 养护期间应保持混凝土表面湿润
"""

# 执行审查
result = complete_review_process(
    sample_document, 
    gkgr_retriever, 
    error_analyzer, 
    revision_generator
)

# 查看审查结果
for query, analysis in result.items():
    print(f"审查问题: {query}")
    print(f"修订建议: {analysis['revision']['revision_suggestions']}")
    print("-" * 50)
```

## 源码获取

[本项目的源码以及实](https://github.com/Hongru0306/CDDRS)例数据存放在 GitHub 仓库。
