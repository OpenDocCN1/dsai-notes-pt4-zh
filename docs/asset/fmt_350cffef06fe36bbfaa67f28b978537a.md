
```
def _execute_sql(self, sql: str) -> Tuple[bool, Any]:
    """执行SQL语句"""
    # 添加LIMIT限制，防止大量数据返回
    if sql.strip().upper().startswith('SELECT') and 'LIMIT' not in sql.upper():
        sql = f"{sql.rstrip(';')} LIMIT {self.max_result_rows}"
    # 结构化结果返回
    if sql.strip().upper().startswith('SELECT'):
        columns = [desc[0] for desc in cursor.description]
        rows = cursor.fetchall()
        
        results = []
        for row in rows:
            result_row = {}
            for i, value in enumerate(row):
                result_row[columns[i]] = value
            results.append(result_row)
        return True, {"columns": columns, "rows": results, "count": len(results)}
```

## [3.4 完整流程模拟](https://datawhalechina.github.io/all-in-rag/#/chapter4/13_text2sql?id=_34-%e5%ae%8c%e6%95%b4%e6%b5%81%e7%a8%8b%e6%a8%a1%e6%8b%9f)

以查询"年龄大于30的用户有哪些"为例，演示框架三个核心模块的完整协作过程：

### [3.4.1 模拟数据](https://datawhalechina.github.io/all-in-rag/#/chapter4/13_text2sql?id=_341-%e6%a8%a1%e6%8b%9f%e6%95%b0%e6%8d%ae)

假设数据库中的`users`表包含以下用户数据：

### 3.4.2 Step 1: 知识库检索

用户输入："年龄大于30的用户有哪些"

检索过程：

1. BGE-M3模型将查询文本转换为768维向量

2. Milvus在知识库中进行语义相似度搜索

3. 返回最相关的5条知识，按相似度排序

检索结果：

DDL知识 (相似度: 0.85)

+ 表名：`users`

+ 结构：包含`id`、`name`、`email`、`age`、`city`字段

+ 约束：`id`为主键，`email`唯一

Q-SQL示例 (相似度: 0.82)

+ 问题："查询年龄超过25岁的用户"

+ SQL：`SELECT * FROM users WHERE age > 25`

+ 这是检索到的相似示例，最终SQL会基于用户实际问题调整为`age > 30`

表描述 (相似度: 0.78)

+ `age`字段：用户年龄，整数类型

+ `name`字段：用户姓名，文本类型

### 3.4.3 Step 2: SQL生成

上下文构建： 系统将检索到的知识整理成结构化的上下文信息：

表结构信息

+ 表名：`users`

+ DDL定义：完整的`CREATE TABLE`语句

+ 字段约束：主键、唯一性等

表和字段描述

+ `age`字段：用户年龄，`INTEGER`类型

+ `name`字段：用户姓名，`TEXT`类型

查询示例

+ 相似问题：查询年龄超过25岁的用户

+ 参考SQL：`SELECT * FROM users WHERE age > 25`

SQL生成过程：

1. DeepSeek分析用户问题的意图：查询满足年龄条件的用户

2. 识别关键信息：`age`字段、比较操作（大于）、阈值（30）

3. 参考示例模式：从`WHERE age > 25`学习到`WHERE age > 数值`的模式

4. 模式应用：将用户的实际数值30替换示例中的25

5. 生成目标SQL：`SELECT * FROM users WHERE age > 30`

### 3.4.4 Step 3: SQL执行与结果处理

安全处理：

+ 原始SQL：`SELECT * FROM users WHERE age > 30`

+ 自动添加限制：`SELECT * FROM users WHERE age > 30 LIMIT 100`

数据库执行： SQLite引擎逐行检查`users`表中的数据：

结果处理：

+ 筛选出2条符合条件的记录

+ 转换为结构化JSON格式

+ 包含字段名称和数据类型信息

最终输出：

```
{
    "success": true,
    "error": null,
    "sql": "SELECT * FROM users WHERE age > 30 LIMIT 100",
    "results": {
        "columns": ["id", "name", "email", "age", "city"],
        "rows": [
            {"id": 2, "name": "李四", "email": "lisi@email.com", "age": 32, "city": "上海"},
            {"id": 4, "name": "赵六", "email": "zhaoliu@email.com", "age": 35, "city": "深圳"}
        ],
        "count": 2
    },
    "retry_count": 0
}
```

通过这个语义理解 → 结构化查询 → 数据过滤 → 结果输出的完整流程，框架成功将用户的自然语言问题转换为精确的数据库查询结果。

## [3.5 代码运行](https://datawhalechina.github.io/all-in-rag/#/chapter4/13_text2sql?id=_35-%e4%bb%a3%e7%a0%81%e8%bf%90%e8%a1%8c)

如果你想测试这个Text2SQL框架，可以通过以下方式进行：

快速体验：运行演示程序

```
python code/C4/03_text2sql_demo.py
```

> 完整演示代码：`03_text2sql_demo.py`

核心代码获取：三个核心模块的完整实现

+ `knowledge_base.py` - 知识库模块

+ `sql_generator.py` - SQL生成模块

+ `text2sql_agent.py` - 代理协调模块

> 源码地址：`code/C4/text2sql/`

数据资源：框架使用的JSON知识数据

+ `ddl_examples.json` - DDL结构示例

+ `qsql_examples.json` - 问题-SQL对示例

+ `db_descriptions.json` - 表和字段描述

> 数据文件：`code/C4/text2sql/data/`

## 3.6 为什么不直接使用封装好的框架？

> 因为淋过雨，所以想为你撑把伞🤪

市面上确实有很多成熟的Text2SQL框架，但这些高度封装的工具往往存在黑盒问题——当查询结果不符合预期时，很难定位是检索环节、SQL生成环节还是执行环节出了问题。正如上一节LangChain示例中遇到的查询异常，我们很难深入到框架内部进行精确调试和优化。这一点在索引优化那节中也提到过。

## [参考文献](https://datawhalechina.github.io/all-in-rag/#/chapter4/13_text2sql?id=%e5%8f%82%e8%80%83%e6%96%87%e7%8c%ae)

[LangChain Docs: Text to SQL](https://datawhalechina.github.io/all-in-rag/#fnref-1) ↩

[RAGFlow Blog: Implementing Text2SQL with RAGFlow](https://ragflow.io/blog/implementing-text2sql-with-ragflow) ↩

[DDL](https://datawhalechina.github.io/all-in-rag/#fnref-3)（Data Definition Language）是数据定义语言，用于定义数据库结构，如`CREATE TABLE`语句。 ↩
