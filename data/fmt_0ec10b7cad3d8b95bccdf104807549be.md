
# 第二节 图数据建模与Neo4j集成

> 来源：[https://gotoaiworld.feishu.cn/docx/TlCzdJ8o3o6Bm0xMKggcM151nKb](https://gotoaiworld.feishu.cn/docx/TlCzdJ8o3o6Bm0xMKggcM151nKb)

> [本节完整代码](https://github.com/datawhalechina/all-in-rag/blob/main/code/C9/rag_modules/graph_data_preparation.py)

## [一、数据来源与转换](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e4%b8%80%e3%80%81%e6%95%b0%e6%8d%ae%e6%9d%a5%e6%ba%90%e4%b8%8e%e8%bd%ac%e6%8d%a2)

### 1.1 从Markdown到图数据的转换

本章的图数据来源于第八章中使用的Markdown格式菜谱数据。为了构建知识图谱，笔者用AI开发了一个简单的Agent，通过LLM将结构化的Markdown菜谱数据转换为CSV格式的图数据。

转换流程：

1. 读取Markdown菜谱：从第八章的数据源加载菜谱文件
1. LLM解析提取：使用大语言模型识别和提取实体及关系
1. 结构化输出：生成`nodes.csv`和`relationships.csv`文件
1. 图数据导入：通过Cypher脚本导入Neo4j数据库

### [1.2 图数据文件结构](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_12-%e5%9b%be%e6%95%b0%e6%8d%ae%e6%96%87%e4%bb%b6%e7%bb%93%e6%9e%84)

转换后的图数据包含两个核心文件：

```
data/C9/cypher/
├── nodes.csv          # 节点数据（菜谱、食材、步骤等）
├── relationships.csv  # 关系数据（菜谱-食材、菜谱-步骤等）
└── neo4j_import.cypher # 数据导入脚本
```

## [二、图数据模型设计](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e4%ba%8c%e3%80%81%e5%9b%be%e6%95%b0%e6%8d%ae%e6%a8%a1%e5%9e%8b%e8%ae%be%e8%ae%a1)

### [2.1 实际数据结构分析](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_21-%e5%ae%9e%e9%99%85%e6%95%b0%e6%8d%ae%e7%bb%93%e6%9e%84%e5%88%86%e6%9e%90)

基于LLM转换后的实际图数据，知识图谱包含以下核心实体类型。如果你有游戏逆向经验，可以把这些实体类型想象成虚幻引擎烹饪游戏中的对象类，节点间的关系就像对象间的指针引用：

核心实体类型：

+   `Recipe` (菜谱)：具体的菜品，包含难度、菜系、时间等属性

+   `Ingredient` (食材)：制作菜品所需的原料，包含分类、用量、单位等

+   `CookingStep` (烹饪步骤)：详细的制作步骤，包含方法、工具、时间估计

+   `CookingMethod` (烹饪方法)：如炒、煮、蒸、炸等烹饪技法

+   `CookingTool` (烹饪工具)：如炒锅、蒸锅、刀具等

+   `DifficultyLevel` (难度等级)：一星到五星的难度分级

+   `RecipeCategory` (菜谱分类)：素菜、荤菜、水产、早餐等分类

实际数据特点：

+   统一编码体系：使用`nodeId`进行唯一标识（如201000001）

+   多语言支持：包含`preferredTerm`、`fsn`等多语言字段

+   丰富属性：每个实体包含详细的属性信息

+   层次化结构：从抽象概念到具体实例的层次化组织

### [2.2 实际节点模型](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_22-%e5%ae%9e%e9%99%85%e8%8a%82%e7%82%b9%e6%a8%a1%e5%9e%8b)

基于实际数据的图数据模型：

节点类型说明：

+   🍽️ `Recipe` (菜谱节点): 核心实体，包含菜谱的完整信息

+   🥬 `Ingredient` (食材节点): 制作菜谱所需的食材信息

+   👨‍🍳 `CookingStep` (烹饪步骤节点): 详细的制作步骤和方法

+   📂 `RecipeCategory` (菜谱分类节点): 菜品分类（素菜、荤菜、水产等）

+   ⭐ `DifficultyLevel` (难度等级节点): 制作难度分级（一星到五星）

### [2.3 实际关系模型](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_23-%e5%ae%9e%e9%99%85%e5%85%b3%e7%b3%bb%e6%a8%a1%e5%9e%8b)

基于实际数据的关系结构：

关系类型说明：

关系特点：

+   虚线箭头：表示带有丰富属性的关系（如`REQUIRES`、`CONTAINS_STEP`）

+   实线箭头：表示简单的分类关系

+   层次化结构：`Root`节点作为概念层次的顶层节点

## [三、Neo4j数据导入](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e4%b8%89%e3%80%81neo4j%e6%95%b0%e6%8d%ae%e5%af%bc%e5%85%a5)

### [3.1 数据准备脚本](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_31-%e6%95%b0%e6%8d%ae%e5%87%86%e5%a4%87%e8%84%9a%e6%9c%ac)

系统通过`GraphDataPreparationModule`来处理图数据的加载和管理：

```
class GraphDataPreparationModule:
    def __init__(self, neo4j_config: dict):
        """
        初始化图数据准备模块
        
        Args:
            neo4j_config: Neo4j连接配置
        """
        self.driver = GraphDatabase.driver(
            neo4j_config['uri'],
            auth=(neo4j_config['user'], neo4j_config['password']))

    def load_graph_data(self) -> List[Dict]:
        """
        从Neo4j加载图数据
        
        Returns:
            包含菜谱信息的字典列表
        """
        query = """
        MATCH (r:Recipe)
        OPTIONAL MATCH (r)-[:REQUIRES]->(i:Ingredient)
        OPTIONAL MATCH (r)-[:HAS_STEP]->(s:Step)
        OPTIONAL MATCH (r)-[:BELONGS_TO]->(c:Category)
        RETURN r, collect(DISTINCT i) as ingredients, 
               collect(DISTINCT s) as steps,
               collect(DISTINCT c) as categories
        ORDER BY r.name
        """
        with self.driver.session() as session:
            result = session.run(query)
        return [record for record in result]
```

### [3.2 实际CSV数据格式](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_32-%e5%ae%9e%e9%99%85csv%e6%95%b0%e6%8d%ae%e6%a0%bc%e5%bc%8f)

转换后的CSV文件格式（基于实际数据）：

`nodes.csv`结构：

```
nodeId,labels,name,preferredTerm,fsn,conceptType,synonyms,category,difficulty,cuisineType,prepTime,cookTime,servings,tags,filePath,amount,unit,isMain,description,stepNumber,methods,tools,timeEstimate
```

实际数据示例：

```
201000184,Recipe,干煎阿根廷红虾,干煎阿根廷红虾,,Recipe,"[{'term': '干pan-fried阿根廷红虾', 'language': 'zh'}]",水产,3.0,,提前1天冷藏解冻+10分钟,约5分钟,1人,"趁热吃,柠檬可增酸提味",dishes\aquatic\干煎阿根廷红虾\干煎阿根廷红虾.md,,,,,,,,
201000185,Ingredient,阿根廷红虾,阿根廷红虾,,Ingredient,,蛋白质,,,,,,,,2-3,只,True,,,,,
201000196,CookingStep,步骤1,步骤1,,CookingStep,,,,,,,,,,,,,阿根廷红虾提前1天从速冻取出放到冷藏里自然解冻,1.0,解冻,冰箱,24小时
```

`relationships.csv`结构：

```
startNodeId,endNodeId,relationshipType,relationshipId,amount,unit,step_order
```

实际关系示例：

```
201000184,201000185,801000001,R_000001,2-3,只,
201000184,201000196,801000003,R_000010,,,1.0
201000184,720000000,801000002,R_000020,,,
```

## [四、图数据查询与检索](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e5%9b%9b%e3%80%81%e5%9b%be%e6%95%b0%e6%8d%ae%e6%9f%a5%e8%af%a2%e4%b8%8e%e6%a3%80%e7%b4%a2)

### [4.1 基础查询模式](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_41-%e5%9f%ba%e7%a1%80%e6%9f%a5%e8%af%a2%e6%a8%a1%e5%bc%8f)

#### [简单实体查询](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e7%ae%80%e5%8d%95%e5%ae%9e%e4%bd%93%e6%9f%a5%e8%af%a2)

```
MATCH (r:Recipe)
WHERE r.category = "水产"
RETURN r.name, r.difficulty, r.prepTime, r.cookTime

MATCH (r:Recipe)-[:REQUIRES]->(i:Ingredient)
WHERE i.name CONTAINS "虾"
RETURN r.name, r.difficulty, i.name, i.amount, i.unit

CALL db.index.fulltext.queryNodes("recipe_fulltext_index", "川菜 OR 辣椒")
YIELD node, score
RETURN node.name, node.category, score
ORDER BY score DESC
```

#### [多跳关系查询](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e5%a4%9a%e8%b7%b3%e5%85%b3%e7%b3%bb%e6%9f%a5%e8%af%a2)

```
MATCH (r:Recipe)
WHERE r.difficulty = 3.0
RETURN r.name, r.category, r.prepTime, r.cookTime, r.difficulty

MATCH (r:Recipe {name: "干煎阿根廷红虾"})-[:CONTAINS_STEP]->(s:CookingStep)
RETURN r.name, s.stepNumber, s.description, s.methods, s.tools
ORDER BY s.stepNumber
```

### [4.2 复杂推理查询](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=_42-%e5%a4%8d%e6%9d%82%e6%8e%a8%e7%90%86%e6%9f%a5%e8%af%a2)

#### [基于约束的菜谱推荐](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e5%9f%ba%e4%ba%8e%e7%ba%a6%e6%9d%9f%e7%9a%84%e8%8f%9c%e8%b0%b1%e6%8e%a8%e8%8d%90)

```
MATCH (r:Recipe)
WHERE r.difficulty <= 2.0
  AND r.stepCount <= 5
RETURN r.name, r.difficulty, r.stepCount, r.category
ORDER BY r.difficulty, r.stepCount

MATCH (r:Recipe)
WHERE r.prepTime IS NOT NULL AND r.cookTime IS NOT NULL
  AND r.prepTime CONTAINS "分钟" AND r.cookTime CONTAINS "分钟"
RETURN r.name, r.prepTime, r.cookTime, r.category
ORDER BY r.name
```

#### [菜谱组合推荐](https://datawhalechina.github.io/all-in-rag/#/chapter9/02_graph_data_modeling?id=%e8%8f%9c%e8%b0%b1%e7%bb%84%e5%90%88%e6%8e%a8%e8%8d%90)
