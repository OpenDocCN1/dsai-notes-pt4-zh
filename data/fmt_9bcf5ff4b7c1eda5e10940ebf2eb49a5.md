
```python
from typing import List, Dict, Tuple
import numpy as np
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import networkx as nx

class KnowledgeRecommender:
    def __init__(self, neo4j_driver, elasticsearch_client):
        self.neo4j = neo4j_driver
        self.es = elasticsearch_client
        self.tfidf_vectorizer = TfidfVectorizer(max_features=5000)
        self.knowledge_graph = self._build_knowledge_graph()
    
    def _build_knowledge_graph(self) -> nx.Graph:
        """构建知识图谱"""
        G = nx.Graph()
        
        # 从Neo4j加载知识图谱数据
        with self.neo4j.session() as session:
            result = session.run("""
                MATCH (n)-[r]->(m) 
                RETURN n.id as source, m.id as target, 
                       type(r) as relation, r.weight as weight
            """)
            
            for record in result:
                G.add_edge(
                    record['source'], 
                    record['target'],
                    relation=record['relation'],
                    weight=record.get('weight', 1.0)
                )
        
        return G
    
    async def recommend(self, intent_result: dict, 
                       context: Optional[DialogState] = None) -> List[Dict]:
        """主推荐函数"""
        intent = intent_result['intent']
        query_text = intent_result['raw_text']
        
        # 多策略推荐
        recommendations = []
        
        # 1. 基于意图的规则推荐
        rule_based = await self._rule_based_recommend(intent, intent_result['slots'])
        recommendations.extend(rule_based)
        
        # 2. 基于语义相似度的推荐
        semantic_based = await self._semantic_recommend(query_text)
        recommendations.extend(semantic_based)
        
        # 3. 基于知识图谱的推荐
        graph_based = await self._graph_based_recommend(intent, context)
        recommendations.extend(graph_based)
        
        # 4. 基于用户历史的个性化推荐
        if context:
            personalized = await self._personalized_recommend(context.user_id, intent)
            recommendations.extend(personalized)
        
        # 去重和排序
        unique_recommendations = self._deduplicate_and_rank(recommendations)
        
        return unique_recommendations[:5]  # 返回top5推荐
    
    async def _rule_based_recommend(self, intent: str, slots: dict) -> List[Dict]:
        """基于规则的推荐"""
        recommendations = []
        
        if intent == 'order_query':
            recommendations.append({
                'type': 'faq',
                'title': '如何查询订单状态',
                'content': '您可以通过订单号在"我的订单"中查看详细状态',
                'score': 0.9,
                'source': 'rule_based'
            })
            
        elif intent == 'return_request':
            recommendations.append({
                'type': 'policy',
                'title': '退换货政策说明',
                'content': '商品在7天内可无理由退货，需保持商品完好',
                'score': 0.95,
                'source': 'rule_based'
            })
            
        elif intent == 'product_inquiry':
            if 'product_keywords' in slots:
                # 基于商品关键词推荐相关FAQ
                product_faqs = await self._get_product_faqs(slots['product_keywords'])
                recommendations.extend(product_faqs)
        
        return recommendations
    
    async def _semantic_recommend(self, query_text: str) -> List[Dict]:
        """基于语义相似度的推荐"""
        # 在Elasticsearch中搜索相似内容
        search_body = {
            "query": {
                "more_like_this": {
                    "fields": ["title", "content"],
                    "like": query_text,
                    "min_term_freq": 1,
                    "max_query_terms": 12
                }
            },
            "size": 10
        }
        
        response = await self.es.search(
            index="knowledge_base",
            body=search_body
        )
        
        recommendations = []
        for hit in response['hits']['hits']:
            recommendations.append({
                'type': 'knowledge',
                'title': hit['_source']['title'],
                'content': hit['_source']['content'],
                'score': hit['_score'] / 10,  # 归一化分数
                'source': 'semantic'
            })
        
        return recommendations
    
    def _deduplicate_and_rank(self, recommendations: List[Dict]) -> List[Dict]:
        """去重和排序"""
        # 基于标题去重
        seen_titles = set()
        unique_recs = []
        
        for rec in recommendations:
            if rec['title'] not in seen_titles:
                seen_titles.add(rec['title'])
                unique_recs.append(rec)
        
        # 按分数排序
        unique_recs.sort(key=lambda x: x['score'], reverse=True)
        
        return unique_recs
```

这个推荐系统结合了多种推荐策略，能够为用户提供精准的知识推荐。

## 系统集成与优化

### 5.1 性能优化策略

![](img/a0069f6ab1dccd62cdf8df1ac88b18ef.png)

### 5.2 A/B测试框架

```python
import random
from enum import Enum
from dataclasses import dataclass
from typing import Dict, Any
 
class TestGroup(Enum):
    CONTROL = "control"
    EXPERIMENT = "experiment"
 
@dataclass
class ABTestConfig:
    test_name: str
    traffic_split: float  # 实验组流量占比
    start_time: int
    end_time: int
    metrics: List[str]
 
class ABTestManager:
    def __init__(self):
        self.active_tests = {}
        self.user_assignments = {}
    
    def assign_user_to_group(self, user_id: str, test_name: str) -> TestGroup:
        """为用户分配测试组"""
        if user_id in self.user_assignments.get(test_name, {}):
            return self.user_assignments[test_name][user_id]
        
        test_config = self.active_tests.get(test_name)
        if not test_config:
            return TestGroup.CONTROL
        
        # 基于用户ID的哈希值进行稳定分组
        hash_value = hash(f"{user_id}_{test_name}") % 100
        group = TestGroup.EXPERIMENT if hash_value < test_config.traffic_split * 100 else TestGroup.CONTROL
        
        # 记录分组结果
        if test_name not in self.user_assignments:
            self.user_assignments[test_name] = {}
        self.user_assignments[test_name][user_id] = group
        
        return group
    
    def log_metric(self, user_id: str, test_name: str, metric_name: str, value: float):
        """记录测试指标"""
        group = self.assign_user_to_group(user_id, test_name)
        # 这里可以集成到数据分析平台
        print(f"Test: {test_name}, Group: {group.value}, Metric: {metric_name}, Value: {value}")
```

## 部署与运维

### 6.1 容器化部署配置
