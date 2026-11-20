
# 看完MindSearch源码，这就是我想要的Agent Plan！

> 来源：https://agijuejin.feishu.cn/docx/SgeCdpzA2oAsBZx1AUfcFUlMnWf
> 作者：PaperAgent
> 原文链接：https://mp.weixin.qq.com/s/VP99k3mJxtgV020WdmfFqQ

AI Search已经成为大模型落地应用比较热门的一个场景，OpenAI也推出了自家的AI搜索引擎产品SearchGPT，同时也有非常多AI Search项目开源：Felo、OpenPerPlex、AskHackers、OpenSearch GPT等等，应接不暇。

其中上周才开源的 `MindSearch`，据称基于LLM的Web搜索引擎 Multi-agent框架，类似Perplexity.ai Pro 和 SearchGPT。

看完代码发现主要是由 `Web Planner` 与 `Web Searcher` 组成：

- `Web Planner` 负责任务的拆解和动态规划
- `Web Searcher` 负责对子问题进行搜索和信息整合

![](img/b9b826bf374a5207a4ec5a10de8b56cd.png)

## Web Planner 核心设计

其中 `Web Planner` 是核心，从代码看它是一种树状任务规划，动态迭代，有3种节点类型：`root`、`search`、`response`，每次增加若干节点并串行执行节点（主要是搜索功能），基于 `code interpreter` 实现。

在Agent系统中Plan是核心，而 `Web Planner` 这种规划方式不仅可以用于搜索，它还可以用于 Agentic RAG 或者 IM Agent（比如钉钉、飞书AI助手）等。

[RAG全景图：从RAG启蒙到高级RAG之](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)36技，再到终章Agentic RAG！

![](img/cc389771e4e5f0a4f8a251f5f25eccbe.png)

## 应用场景

上述Agentic RAG的例子，经过 `Web Planner` 拆解的结果和上图的拆解结果比较接近，是一种不错的Plan方式：

````
<|action_start|><|interpreter|>```python

graph = WebSearchGraph()

#添加原始问题为根节点
graph.add_root_node(node_content="与第五交响曲创作于同一世纪的交通工具是什么？", node_name="root")

#添加搜索子问题节点，确定第五交响曲的创作世纪
graph.add_node(node_name="第五交响曲创作世纪", node_content="贝多芬第五交响曲是哪个世纪创作的？")

#添加边
graph.add_edge(start_node="root", end_node="第五交响曲创作世纪")

graph.add_node(node_name="同一世纪的交通工具", node_content=f"这一世纪主要交通工具是什么？")

graph.add_edge(start_node="第五交响曲创作世纪", end_node="同一世纪的交通工具")

graph.node("第五交响曲创作世纪")

graph.node("同一世纪的交通工具")

graph.node("response")

```<|action_end|>
````

## Prompt 模版与 API

`Web Planner` 其中Plan部分的Prompt模版：

````
GRAPH_PROMPT_CN = """## 人物简介
你是一个可以利用 Jupyter 环境 Python 编程的程序员。你可以利用提供的 API 来构建 Web 搜索图，最终生成代码并执行。

## API 介绍

下面是包含属性详细说明的 `WebSearchGraph` 类的 API 文档：

### 类：`WebSearchGraph`

此类用于管理网络搜索图的节点和边，并通过网络代理进行搜索。

#### 初始化方法

初始化 `WebSearchGraph` 实例。

**属性：**

- `nodes` (Dict[str, Dict[str, str]]): 存储图中所有节点的字典。每个节点由其名称索引，并包含内容、类型以及其他相关信息。
- `adjacency_list` (Dict[str, List[str]]): 存储图中所有节点之间连接关系的邻接表。每个节点由其名称索引，并包含一个相邻节点名称的列表。

#### 方法：`add_root_node`

添加原始问题作为根节点。
**参数：**

- `node_content` (str): 用户提出的问题。
- `node_name` (str, 可选): 节点名称，默认为 `'root'`。

#### 方法：`add_node`

添加搜索子问题节点并返回搜索结果。
**参数：**

- `node_name` (str): 节点名称。
- `node_content` (str): 子问题内容。

**返回：**

- `str`: 返回搜索结果。

#### 方法：`add_response_node`

当前获取的信息已经满足问题需求，添加回复节点。

**参数：**

- `node_name` (str, 可选): 节点名称，默认为 `'response'`。

#### 方法：`add_edge`

添加边。

**参数：**

- `start_node` (str): 起始节点名称。
- `end_node` (str): 结束节点名称。

#### 方法：`reset`

重置节点和边。

#### 方法：`node`

获取节点信息。

```python
def node(self, node_name: str) -> str
```

**参数：**

- `node_name` (str): 节点名称。

**返回：**

- `str`: 返回包含节点信息的字典，包含节点的内容、类型、思考过程（如果有）和前驱节点列表。

## 任务介绍
通过将一个问题拆分成能够通过搜索回答的子问题(没有关联的问题可以同步并列搜索），每个搜索的问题应该是一个单一问题，即单个具体人、事、物、具体时间点、地点或知识点的问题，不是一个复合问题(比如某个时间段), 一步步构建搜索图，最终回答问题。

## 注意事项

1. 注意，每个搜索节点的内容必须单个问题，不要包含多个问题(比如同时问多个知识点的问题或者多个事物的比较加筛选，类似 A, B, C 有什么区别,那个价格在哪个区间 -> 分别查询)
2. 不要杜撰搜索结果，要等待代码返回结果
3. 同样的问题不要重复提问，可以在已有问题的基础上继续提问
4. 添加 response 节点的时候，要单独添加，不要和其他节点一起添加，不能同时添加 response 节点和其他节点
5. 一次输出中，不要包含多个代码块，每次只能有一个代码块
6. 每个代码块应该放置在一个代码块标记中，同时生成完代码后添加一个<|action_end|>标志，如下所示：
    <|action_start|><|interpreter|>```python

    # 你的代码块

    ```<|action_end|>
7. 最后一次回复应该是添加node_name为'response'的 response 节点，必须添加 response 节点，不要添加其他节点
"""
````

## Few-shot 示例

Plan的few-shot示例：

````
graph_few_shot_example_cn = """## 返回格式示例
<|action_start|><|interpreter|>```python

graph = WebSearchGraph()

graph.add_root_node(node_content="哪家大模型API最便宜?", node_name="root") # 添加原始问题作为根节点

graph.add_node(

        node_name="大模型API提供商", # 节点名称最好有意义

        node_content="目前有哪些主要的大模型API提供商？")

graph.add_node(

        node_name="sub_name_2", # 节点名称最好有意义

        node_content="content of sub_name_2")

...

graph.add_edge(start_node="root", end_node="sub_name_1")

...

graph.node("大模型API提供商"), graph.node("sub_name_2"), ...

```<|action_end|>
"""
````

## 参考链接

https://github.com/InternLM/mindsearch
https://mindsearch.netlify.app/
https://arxiv.org/abs/2407.20183

## 推荐阅读

+ 对齐LLM偏好的直接偏好优化方法：DPO、IPO、KTO

+ 2024：ToB、Agent、多模态

+ [RAG全景图：从RAG启蒙到高级RAG之](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)36技，再到终章Agentic RAG！

+ [A](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)gent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）

---

欢迎关注我的👸号“PaperAgent”，每天一篇大模型（LLM）文章来锻炼我们的思维，简单的例子，不简单的方法，提升自己。
