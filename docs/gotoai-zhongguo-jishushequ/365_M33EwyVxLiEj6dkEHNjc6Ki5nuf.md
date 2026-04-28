# 第二节 图数据建模与Neo4j集成

> 来源：[https://gotoaiworld.feishu.cn/docx/TlCzdJ8o3o6Bm0xMKggcM151nKb](https://gotoaiworld.feishu.cn/docx/TlCzdJ8o3o6Bm0xMKggcM151nKb)

> [本节完整代码](https://github.com/datawhalechina/all-in-rag/blob/main/code/C9/rag_modules/graph_data_preparation.py)

## [一、数据来源与转换](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e4%b8%80%e3%80%81%e6%95%b0%e6%8d%ae%e6%9d%a5%e6%ba%90%e4%b8%8e%e8%bd%ac%e6%8d%a2)

### 1.1 从Markdown到图数据的转换

本[章的图数据](https://github.com/datawhalechina/all-in-rag/tree/main/code/C9/agent(%E4%BB%A3%E7%A0%81%E7%B3%BBai%E7%94%9F%E6%88%90))来源于第八章中使用的Markdown格式菜谱数据。为了构建知识图谱，笔者用AI开发了一个简单的Agent，通过LLM将结构化的Markdown菜谱数据转换为CSV格式的图数据。

转换流程：

1.  读取Markdown菜谱：从第八章的数据源加载菜谱文件

1.  LLM解析提取：使用大语言模型识别和提取实体及关系

1.  结构化输出：生成nodes.csv和relationships.csv文件

1.  图数据导入：通过Cypher脚本导入Neo4j数据库

### [1.2 图数据文件结构](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_12-%e5%9b%be%e6%95%b0%e6%8d%ae%e6%96%87%e4%bb%b6%e7%bb%93%e6%9e%84)

转换后的图数据包含两个核心文件：

```
data/C9/cypher/
├── nodes.csv          # 节点数据（菜谱、食材、步骤等）
├── relationships.csv  # 关系数据（菜谱-食材、菜谱-步骤等）
└── neo4j_import.cypher # 数据导入脚本Copy to clipboardErrorCopied
```

## [二、图数据模型设计](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e4%ba%8c%e3%80%81%e5%9b%be%e6%95%b0%e6%8d%ae%e6%a8%a1%e5%9e%8b%e8%ae%be%e8%ae%a1)

### [2.1 实际数据结构分析](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_21-%e5%ae%9e%e9%99%85%e6%95%b0%e6%8d%ae%e7%bb%93%e6%9e%84%e5%88%86%e6%9e%90)

基于LLM转换后的实际图数据，知识图谱包含以下核心实体类型。如果你有游戏逆向经验，可以把这些实体类型想象成虚幻引擎烹饪游戏中的对象类，节点间的关系就像对象间的指针引用：

核心实体类型：

+   Recipe (菜谱)：具体的菜品，包含难度、菜系、时间等属性

+   Ingredient (食材)：制作菜品所需的原料，包含分类、用量、单位等

+   CookingStep (烹饪步骤)：详细的制作步骤，包含方法、工具、时间估计

+   CookingMethod (烹饪方法)：如炒、煮、蒸、炸等烹饪技法

+   CookingTool (烹饪工具)：如炒锅、蒸锅、刀具等

+   DifficultyLevel (难度等级)：一星到五星的难度分级

+   RecipeCategory (菜谱分类)：素菜、荤菜、水产、早餐等分类

实际数据特点：

+   统一编码体系：使用nodeId进行唯一标识（如201000001）

+   多语言支持：包含preferredTerm、fsn等多语言字段

+   丰富属性：每个实体包含详细的属性信息

+   层次化结构：从抽象概念到具体实例的层次化组织

### [2.2 实际节点模型](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_22-%e5%ae%9e%e9%99%85%e8%8a%82%e7%82%b9%e6%a8%a1%e5%9e%8b)

基于实际数据的图数据模型：

节点类型说明：

+   🍽️ Recipe (菜谱节点): 核心实体，包含菜谱的完整信息

+   🥬 Ingredient (食材节点): 制作菜谱所需的食材信息

+   👨‍🍳 CookingStep (烹饪步骤节点): 详细的制作步骤和方法

+   📂 RecipeCategory (菜谱分类节点): 菜品分类（素菜、荤菜、水产等）

+   ⭐ DifficultyLevel (难度等级节点): 制作难度分级（一星到五星）

### [2.3 实际关系模型](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_23-%e5%ae%9e%e9%99%85%e5%85%b3%e7%b3%bb%e6%a8%a1%e5%9e%8b)

基于实际数据的关系结构：

关系类型说明：

关系特点：

+   虚线箭头：表示带有丰富属性的关系（如REQUIRES、CONTAINS_STEP）

+   实线箭头：表示简单的分类关系

+   层次化结构：Root节点作为概念层次的顶层节点

## [三、Neo4j数据导入](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e4%b8%89%e3%80%81neo4j%e6%95%b0%e6%8d%ae%e5%af%bc%e5%85%a5)

### [3.1 数据准备脚本](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_31-%e6%95%b0%e6%8d%ae%e5%87%86%e5%a4%87%e8%84%9a%e6%9c%ac)

系统通过 GraphDataPreparationModule 来处理图数据的加载和管理：

```
class GraphDataPreparationModule:def __init__(self, neo4j_config: dict):&quot;&quot;&quot;
        初始化图数据准备模块
        
        Args:
            neo4j_config: Neo4j连接配置
        &quot;&quot;&quot;
        self.driver = GraphDatabase.driver(
            neo4j_config[&#x27;uri&#x27;],
            auth=(neo4j_config[&#x27;user&#x27;], neo4j_config[&#x27;password&#x27;]))def load_graph_data(self) -&gt; List[Dict]:&quot;&quot;&quot;
        从Neo4j加载图数据
        
        Returns:
            包含菜谱信息的字典列表
        &quot;&quot;&quot;
        query = &quot;&quot;&quot;
        MATCH (r:Recipe)
        OPTIONAL MATCH (r)-[:REQUIRES]-&gt;(i:Ingredient)
        OPTIONAL MATCH (r)-[:HAS_STEP]-&gt;(s:Step)
        OPTIONAL MATCH (r)-[:BELONGS_TO]-&gt;(c:Category)
        RETURN r, collect(DISTINCT i) as ingredients, 
               collect(DISTINCT s) as steps,
               collect(DISTINCT c) as categories
        ORDER BY r.name
        &quot;&quot;&quot;with self.driver.session() as session:
            result = session.run(query)return [record for record in result]Copy to clipboardErrorCopied
```

### [3.2 实际CSV数据格式](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_32-%e5%ae%9e%e9%99%85csv%e6%95%b0%e6%8d%ae%e6%a0%bc%e5%bc%8f)

转换后的CSV文件格式（基于实际数据）：

nodes.csv结构：

```
nodeId,labels,name,preferredTerm,fsn,conceptType,synonyms,category,difficulty,cuisineType,prepTime,cookTime,servings,tags,filePath,amount,unit,isMain,description,stepNumber,methods,tools,timeEstimateCopy to clipboardErrorCopied
```

实际数据示例：

```
201000184,Recipe,干煎阿根廷红虾,干煎阿根廷红虾,,Recipe,&quot;[{&#x27;term&#x27;: &#x27;干pan-fried阿根廷红虾&#x27;, &#x27;language&#x27;: &#x27;zh&#x27;}]&quot;,水产,3.0,,提前1天冷藏解冻+10分钟,约5分钟,1人,&quot;趁热吃,柠檬可增酸提味&quot;,dishes\aquatic\干煎阿根廷红虾\干煎阿根廷红虾.md,,,,,,,,
201000185,Ingredient,阿根廷红虾,阿根廷红虾,,Ingredient,,蛋白质,,,,,,,,2-3,只,True,,,,,
201000196,CookingStep,步骤1,步骤1,,CookingStep,,,,,,,,,,,,,阿根廷红虾提前1天从速冻取出放到冷藏里自然解冻,1.0,解冻,冰箱,24小时Copy to clipboardErrorCopied
```

relationships.csv结构：

```
startNodeId,endNodeId,relationshipType,relationshipId,amount,unit,step_orderCopy to clipboardErrorCopied
```

实际关系示例：

```
201000184,201000185,801000001,R_000001,2-3,只,
201000184,201000196,801000003,R_000010,,,1.0
201000184,720000000,801000002,R_000020,,,Copy to clipboardErrorCopied
```

## [四、图数据查询与检索](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e5%9b%9b%e3%80%81%e5%9b%be%e6%95%b0%e6%8d%ae%e6%9f%a5%e8%af%a2%e4%b8%8e%e6%a3%80%e7%b4%a2)

### [4.1 基础查询模式](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_41-%e5%9f%ba%e7%a1%80%e6%9f%a5%e8%af%a2%e6%a8%a1%e5%bc%8f)

#### [简单实体查询](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e7%ae%80%e5%8d%95%e5%ae%9e%e4%bd%93%e6%9f%a5%e8%af%a2)

```
// 查找所有水产类菜谱
MATCH (r:Recipe)
WHERE r.category = &quot;水产&quot;
RETURN r.name, r.difficulty, r.prepTime, r.cookTime

// 查找包含特定食材的菜谱
MATCH (r:Recipe)-[:REQUIRES]-&gt;(i:Ingredient)
WHERE i.name CONTAINS &quot;虾&quot;
RETURN r.name, r.difficulty, i.name, i.amount, i.unit

// 使用全文搜索查找菜谱
CALL db.index.fulltext.queryNodes(&quot;recipe_fulltext_index&quot;, &quot;川菜 OR 辣椒&quot;)
YIELD node, score
RETURN node.name, node.category, score
ORDER BY score DESCCopy to clipboardErrorCopied
```

#### [多跳关系查询](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e5%a4%9a%e8%b7%b3%e5%85%b3%e7%b3%bb%e6%9f%a5%e8%af%a2)

```
// 查找某个难度等级的所有菜谱（基于属性查询）
MATCH (r:Recipe)
WHERE r.difficulty = 3.0
RETURN r.name, r.category, r.prepTime, r.cookTime, r.difficulty

// 查找菜谱的完整制作流程
MATCH (r:Recipe {name: &quot;干煎阿根廷红虾&quot;})-[:CONTAINS_STEP]-&gt;(s:CookingStep)
RETURN r.name, s.stepNumber, s.description, s.methods, s.tools
ORDER BY s.stepNumberCopy to clipboardErrorCopied
```

### [4.2 复杂推理查询](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_42-%e5%a4%8d%e6%9d%82%e6%8e%a8%e7%90%86%e6%9f%a5%e8%af%a2)

#### [基于约束的菜谱推荐](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e5%9f%ba%e4%ba%8e%e7%ba%a6%e6%9d%9f%e7%9a%84%e8%8f%9c%e8%b0%b1%e6%8e%a8%e8%8d%90)

```
// 查找适合新手的简单菜谱（低难度、步骤少）
MATCH (r:Recipe)
WHERE r.difficulty &lt;= 2.0
  AND r.stepCount &lt;= 5
RETURN r.name, r.difficulty, r.stepCount, r.category
ORDER BY r.difficulty, r.stepCount

// 查找制作时间短的菜谱
MATCH (r:Recipe)
WHERE r.prepTime IS NOT NULL AND r.cookTime IS NOT NULL
  AND r.prepTime CONTAINS &quot;分钟&quot; AND r.cookTime CONTAINS &quot;分钟&quot;
RETURN r.name, r.prepTime, r.cookTime, r.category
ORDER BY r.nameCopy to clipboardErrorCopied
```

#### [菜谱组合推荐](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e8%8f%9c%e8%b0%b1%e7%bb%84%e5%90%88%e6%8e%a8%e8%8d%90)

```
// 查找同一分类下的不同菜谱
MATCH (r1:Recipe), (r2:Recipe)
WHERE r1.category = r2.category
  AND r1.category = &quot;水产&quot;
  AND r1.nodeId &lt;&gt; r2.nodeId
RETURN r1.name, r2.name, r1.category
LIMIT 5

// 查找包含相同食材的不同菜谱
MATCH (r1:Recipe)-[:REQUIRES]-&gt;(i:Ingredient)&lt;-[:REQUIRES]-(r2:Recipe)WHERE r1.nodeId &lt;&gt; r2.nodeId
  AND i.name = &quot;阿根廷红虾&quot;
RETURN r1.name, r2.name, i.nameCopy to clipboardErrorCopied
```

## [五、图数据到文档的转换](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e4%ba%94%e3%80%81%e5%9b%be%e6%95%b0%e6%8d%ae%e5%88%b0%e6%96%87%e6%a1%a3%e7%9a%84%e8%bd%ac%e6%8d%a2)

### [5.1 结构化文档构建](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_51-%e7%bb%93%e6%9e%84%e5%8c%96%e6%96%87%e6%a1%a3%e6%9e%84%e5%bb%ba)

```
def build_recipe_documents(self, graph_data: List[Dict]) -&gt; List[Document]:&quot;&quot;&quot;将图数据转换为结构化文档&quot;&quot;&quot;

    documents = []for record in graph_data:
        recipe = record[&#x27;r&#x27;]
        ingredients = record[&#x27;ingredients&#x27;]
        steps = record[&#x27;steps&#x27;]
        categories = record[&#x27;categories&#x27;]# 构建结构化文档内容
        content_parts = [f&quot;# {recipe[&#x27;name&#x27;]}&quot;,f&quot;分类: {&#x27;, &#x27;.join([c[&#x27;name&#x27;] for c in categories])}&quot;,f&quot;难度: {recipe[&#x27;difficulty&#x27;]}星&quot;,# ... 时间、份量等基本信息&quot;&quot;,&quot;## 所需食材&quot;]# 添加食材列表for i, ingredient in enumerate(ingredients, 1):
            content_parts.append(f&quot;{i}. {ingredient[&#x27;name&#x27;]}&quot;)

        content_parts.extend([&quot;&quot;, &quot;## 制作步骤&quot;])# 添加制作步骤（按顺序排序）
        sorted_steps = sorted(steps, key=lambda x: x.get(&#x27;order&#x27;, 0))for step in sorted_steps:
            content_parts.extend([f&quot;### 第{step[&#x27;order&#x27;]}步&quot;,
                step[&#x27;description&#x27;],&quot;&quot;])# 创建Document对象
        document = Document(
            page_content=&quot;\n&quot;.join(content_parts),
            metadata={&#x27;recipe_name&#x27;: recipe[&#x27;name&#x27;],&#x27;node_id&#x27;: recipe.get(&#x27;nodeId&#x27;),  # 关键：保持与图节点的关联&#x27;difficulty&#x27;: recipe.get(&#x27;difficulty&#x27;, 0),&#x27;categories&#x27;: [c[&#x27;name&#x27;] for c in categories],&#x27;ingredients&#x27;: [i[&#x27;name&#x27;] for i in ingredients]# ... 其他元数据})
        documents.append(document)return documentsCopy to clipboardErrorCopied
```

> 为什么不直接读取原始Markdown文件？

> 虽然第八章中HowToCook项目的Markdown格式是统一的，但图RAG的价值在于提供更丰富的信息：

> 原始Markdown的特点：

+   格式统一：HowToCook项目有良好的Markdown结构（#、##、###层级）

+   信息完整：包含菜品名称、原料、制作步骤等基本信息

+   元数据推断：可以从文件路径推断分类，从★★★★★符号推断难度

> 图数据构建文档的额外价值：

1.  关系信息丰富：包含食材间的替代关系、菜谱间的相似性等图关系

1.  结构化查询：可以通过图关系快速获取相关信息（如"包含鸡肉的所有菜谱"）

1.  动态内容生成：根据图关系动态生成推荐内容（如"相似菜谱"、"替代食材"）

1.  语义增强：图数据库可以存储更丰富的语义信息和计算结果

1.  查询优化：图查询在复杂关系检索上比文本搜索更高效

### [5.2 图RAG中的分块策略](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_52-%e5%9b%berag%e4%b8%ad%e7%9a%84%e5%88%86%e5%9d%97%e7%ad%96%e7%95%a5)

在图RAG系统中，分块策略与上个项目有所不同，主要体现在数据来源和上下文获取方式的差异：

图RAG vs 传统RAG的分块对比：

图RAG分块的特点：

1.  保持图关联：每个chunk通过parent_id与图节点关联

1.  语义优先分块：优先按章节分块，保持语义完整性

1.  丰富的元数据：直接从图节点获取结构化信息

1.  双重上下文：既有文本块关系，又有图关系信息

### [5.3 实际分块实现](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_53-%e5%ae%9e%e9%99%85%e5%88%86%e5%9d%97%e5%ae%9e%e7%8e%b0)

在图RAG系统中，采用的实际分块策略：

```
def chunk_documents(self, chunk_size: int = 500, chunk_overlap: int = 50) -&gt; List[Document]:&quot;&quot;&quot;图RAG文档分块：结合图结构优势的智能分块策略&quot;&quot;&quot;

    chunks = []for doc in self.documents:
        content = doc.page_content

        if len(content) &lt;= chunk_size:# 短文档：保持完整，避免破坏语义
            chunk = Document(
                page_content=content,
                metadata={**doc.metadata,&quot;parent_id&quot;: doc.metadata[&quot;node_id&quot;],  # 关键：保持与图节点的关联&quot;chunk_index&quot;: 0,&quot;doc_type&quot;: &quot;chunk&quot;})
            chunks.append(chunk)else:# 长文档：智能分块策略
            sections = content.split(&#x27;\n## &#x27;)if len(sections) &lt;= 1:# 无章节结构：按长度分块（带重叠）
                total_chunks = (len(content) - 1) // (chunk_size - chunk_overlap) + 1for i in range(total_chunks):
                    start = i * (chunk_size - chunk_overlap)
                    end = min(start + chunk_size, len(content))# ... 创建chunk，保持parent_id关联else:# 有章节结构：按语义分块（推荐）for i, section in enumerate(sections):
                    chunk_content = section if i == 0 else f&quot;## {section}&quot;# ... 创建chunk，包含section_title信息return chunksCopy to clipboardErrorCopied
```

图RAG的分块策略在保持语义完整性的基础上，充分利用图数据库的结构化优势。与第八章直接读取Markdown文件不同，这里从图数据库构建标准化文档，每个chunk通过parent_id与原始Recipe节点保持关联，既继承了传统的父子文档映射关系，又能通过图关系遍历获取更丰富的上下文信息。在具体实现上，采用智能分块策略：短文档保持完整避免破坏语义，长文档优先按##标题进行章节分块，必要时才进行长度分块，同时为每个chunk提供丰富的元数据（如chunk_id、chunk_index、total_chunks等），确保后续处理的灵活性和可追溯性。