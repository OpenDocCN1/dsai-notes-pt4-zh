
# 模型调用次数和 Token 数量

```
print(
    f"LLM calls: {result1.llm_calls}. Prompt tokens: {result1.prompt_tokens}. Output tokens: {result1.output_tokens}."
)
```

```
LLM calls: 2. Prompt tokens: 7183. Output tokens: 1086.
```

## Step 18. 大文本下知识图谱可视化

```
def convert_entities_to_dicts(df):
    """Convert the entities dataframe to a list of dicts for yfiles-jupyter-graphs."""
    nodes_dict = {}
    for _, row in df.iterrows():
        # Create a dictionary for each row and collect unique nodes
        node_id = row["title"]
        if node_id not in nodes_dict:
            nodes_dict[node_id] = {
                "id": node_id,
                "properties": row.to_dict(),
            }
    return list(nodes_dict.values())
```

```
def convert_relationships_to_dicts(df):
    """Convert the relationships dataframe to a list of dicts for yfiles-jupyter-graphs."""
    relationships = []
    for _, row in df.iterrows():
        # Create a dictionary for each row
        relationships.append({
            "start": row["source"],
            "end": row["target"],
            "properties": row.to_dict(),
        })
    return relationships
```

```
w = GraphWidget()    # 创建GraphWidget对象
w.directed = True    # 设置图形为有向图
w.nodes = convert_entities_to_dicts(entity_df) # 将实体数据转换为节点
w.edges = convert_relationships_to_dicts(relationship_df) # 将关系数据转换为边
```

```
w.node_label_mapping = "title"  # 设置节点标签显示
```

```
# 社区到颜色的映射
def community_to_color(community):
    """Map a community to a color."""
    colors = [
        "crimson",
        "darkorange",
        "indigo",
        "cornflowerblue",
        "cyan",
        "teal",
        "green",
    ]
    try:
        return colors[int(community) % len(colors)] if community is not None else "lightgray"
    except (ValueError, TypeError):
        # 如果 community 不是整数或其他错误，返回默认颜色
        return "lightgray"
```

```
def edge_to_source_community(edge):
    """Get the community of the source node of an edge."""
    source_node = next(
        (entry for entry in w.nodes if entry["properties"]["title"] == edge["start"]),
        None,
    )
    source_node_community = source_node["properties"]["community"]
    return source_node_community if source_node_community is not None else None
```

```
w.node_color_mapping = lambda node: community_to_color(
    node["properties"].get("community", None)  # 使用 .get 方法获取属性，避免 KeyError
)
w.edge_color_mapping = lambda edge: community_to_color(edge_to_source_community(edge))

w.node_scale_factor_mapping = lambda node: 0.5 + node["properties"].get("size", 1) * 1.5 / 20

w.edge_thickness_factor_mapping = "weight"
```

```
w.circular_layout()
```

```
display(w)
```

```
GraphWidget(layout=Layout(height='800px', width='100%'))
```

### 五、GraphRAG提问测试

```
# 本地检索引擎
search_engine_local
```

```
<graphrag.query.structured_search.local_search.search.LocalSearch at 0x7f8c3c50bd90>
```

```
# 全局检索引擎
search_engine
```

```
<graphrag.query.structured_search.global_search.search.GlobalSearch at 0x7f8c5c97ff10>
```

#### 总结性质问题

```
# 本地检索
result = await search_engine_local.asearch("请问文档中总共介绍了几种决策树算法？")
```

```
display(Markdown(result.response))
```

抱歉，我无法回答这个问题，因为没有提供相关的数据表或文档内容。如果您能提供更多信息或数据，我将很乐意帮助您分析。

```
# 全局检索
result = await search_engine.asearch("请问文档中总共介绍了几种决策树算法？")
```

```
display(Markdown(result.response))
```

文档中总共介绍了四种决策树算法，包括CART、ID3、C4.5和C5.0。这些算法在不同的背景下进行了讨论，强调了它们在机器学习领域中的作用和贡献 [Data: Reports (0, 6, 7, 3, +more)]。

```
result.context_data["reports"]
```

|  | id | title | occurrence weight | content | rank |

| --- | --- | --- | --- | --- | --- |

| 0 | 8 | Scikit-Learn and Decision Tree Models | 1.000000 | # Scikit-Learn and Decision Tree Models\n\nThe... | 8.5 |

| 1 | 5 | Python Ecosystem for Scientific Computing and ... | 0.777778 | # Python Ecosystem for Scientific Computing an... | 8.5 |

| 2 | 3 | CART and Sklearn in Machine Learning | 0.777778 | # CART and Sklearn in Machine Learning\n\nThe ... | 8.5 |

| 3 | -1 | Classification and Regression Trees and Random... | 0.666667 | # Classification and Regression Trees and Rand... | 8.5 |

| 4 | 6 | C4.5 and Decision Tree Algorithms | 0.666667 | # C4.5 and Decision Tree Algorithms\n\nThe com... | 8.5 |

| 5 | 0 | CART and ID3 Decision Trees in Machine Learning | 0.444444 | # CART and ID3 Decision Trees in Machine Learn... | 7.5 |

| 6 | 7 | Ross Quinlan and Decision Tree Algorithms | 0.222222 | # Ross Quinlan and Decision Tree Algorithms\n\... | 8.5 |

| 7 | 9 | Scikit-Learn Hyperparameter Tuning Tools | 0.222222 | # Scikit-Learn Hyperparameter Tuning Tools\n\n... | 7.5 |

| 8 | 10 | Keras and TensorFlow Integration | 0.111111 | # Keras and TensorFlow Integration\n\nThe comm... | 8.0 |

| 9 | 4 | CART Regression Trees and Statistical Learning | 0.111111 | # CART Regression Trees and Statistical Learni... | 4.0 |

#### 综合评价类问题

```
# 本地检索
result = await search_engine_local.asearch("你觉得文档中介绍的决策树算法，哪个算法最有应用前景？")
display(Markdown(result.response))
```

很抱歉，我无法回答这个问题，因为没有提供相关的文档或数据表来支持对决策树算法的分析和比较。如果你能提供更多具体的信息或数据，我将很乐意帮助你分析和讨论这些算法的应用前景。

```
# 全局检索
result = await search_engine.asearch("你觉得文档中介绍的决策树算法，哪个算法最有应用前景？")
display(Markdown(result.response))
```

在文档中介绍的决策树算法中，CART（Classification and Regression Trees）和C4.5算法都展现了强大的应用前景。CART算法因其处理连续变量的能力以及在流行库如Scikit-Learn中的应用而备受关注。这种算法的多功能性使其在分类和回归任务中都具有很高的应用潜力 [Data: Reports (3, 0, 4)]。

C4.5算法由Ross Quinlan开发，被IEEE评为顶级数据挖掘算法之一。它能够处理分类和连续数据，并采用剪枝技术，因而在各种应用中表现出色 [Data: Reports (6, 7)]。此外，C4.5和CART算法的集成到广泛使用的库和软件套件中，如Scikit-Learn和SAS，进一步证明了它们的实用性和广泛应用的潜力 [Data: Reports (5, 7, 9)]。

此外，随机森林（Random Forests）算法也因其在处理大型数据集时的准确性和稳健性而广受欢迎。这种集成方法在实践中被广泛使用，显示出其强大的应用潜力 [Data: Reports (3, -1)]。

综上所述，CART和C4.5算法由于其在处理不同类型数据和集成到流行工具中的能力，展现了显著的应用前景，而随机森林算法则因其在实际应用中的表现而备受推崇。

```
result.context_data["reports"]
```

|  | id | title | occurrence weight | content | rank |

| --- | --- | --- | --- | --- | --- |

| 0 | 8 | Scikit-Learn and Decision Tree Models | 1.000000 | # Scikit-Learn and Decision Tree Models\n\nThe... | 8.5 |

| 1 | 5 | Python Ecosystem for Scientific Computing and ... | 0.777778 | # Python Ecosystem for Scientific Computing an... | 8.5 |

| 2 | 3 | CART and Sklearn in Machine Learning | 0.777778 | # CART and Sklearn in Machine Learning\n\nThe ... | 8.5 |

| 3 | -1 | Classification and Regression Trees and Random... | 0.666667 | # Classification and Regression Trees and Rand... | 8.5 |

| 4 | 6 | C4.5 and Decision Tree Algorithms | 0.666667 | # C4.5 and Decision Tree Algorithms\n\nThe com... | 8.5 |

| 5 | 0 | CART and ID3 Decision Trees in Machine Learning | 0.444444 | # CART and ID3 Decision Trees in Machine Learn... | 7.5 |

| 6 | 7 | Ross Quinlan and Decision Tree Algorithms | 0.222222 | # Ross Quinlan and Decision Tree Algorithms\n\... | 8.5 |

| 7 | 9 | Scikit-Learn Hyperparameter Tuning Tools | 0.222222 | # Scikit-Learn Hyperparameter Tuning Tools\n\n... | 7.5 |

| 8 | 10 | Keras and TensorFlow Integration | 0.111111 | # Keras and TensorFlow Integration\n\nThe comm... | 8.0 |

| 9 | 4 | CART Regression Trees and Statistical Learning | 0.111111 | # CART Regression Trees and Statistical Learni... | 4.0 |

#### 主观评价类问题

```
# 本地检索
result = await search_engine_local.asearch("你觉得这篇文档写得如何？")
display(Markdown(result.response))
```

很抱歉，我无法评估这篇文档的写作质量，因为没有提供任何具体的文档内容或数据表供我参考。如果您能提供更多的细节或上下文，我会很乐意帮助您分析或提供建议。

```
# 全局检索
result = await search_engine.asearch("你觉得这篇文档写得如何？")
display(Markdown(result.response))
```
