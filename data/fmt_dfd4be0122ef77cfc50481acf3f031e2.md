
## DataFrame API 基本操作

+ **代码功能**： 这部分使用 `DataFrame` 的原生方法进行数据分析。

+ `df['CustomerName', 'Email']` 或 `df.select(...)`： 选择特定的列。

+ `.distinct().count()`： 计算不重复的记录数。

+ `.where(df['Item']=='...')`： 根据条件过滤行。

+ **学习要点**：

+ **DataFrame API**: 掌握 Spark `DataFrame` 最基础、最核心的操作，包括选择（Projection）和过滤（Filter）。

+ **不可变性**： 理解 Spark 中 `DataFrame` 的转换（Transformation）操作（如 `select`， `where`）不会修改原始 `DataFrame`，而是返回一个新的 `DataFrame`。

+ **数据聚合与分组**：

+ **代码功能**：

+ `df.select(...).groupBy("Item").sum()`： 按商品（`Item`）分组，计算每个商品的总销售量。

+ `df.select(year(col("OrderDate")).alias("Year")).groupBy("Year").count()`： 使用 Spark SQL 内置函数 `year` 从日期中提取年份，重命名为 `"Year"`，然后按年份分组统计订单数量。

+ **学习要点**：

+ **聚合操作**： 学习数据分析中最常见的模式之一：拆分-应用-合并（Split-Apply-Combine），通过 `groupBy` 和聚合函数（`sum`， `count`）实现。

+ **使用内置函数**： 了解如何从 `pyspark.sql.functions` 导入强大的函数来处理和转换数据，例如日期处理、字符串操作等。

## 数据转换与持久化

> 这部分聚焦于更复杂的数据处理（数据工程任务），并将清理、转换后的结果保存下来，为后续分析做准备。

### 转换数据（特征工程）

+ **代码功能**：

+ `.withColumn("Year", year(col("OrderDate")))`： 基于现有列（`OrderDate`）创建新列（`Year`， `Month`）。

+ `.withColumn("FirstName", split(col("CustomerName"), " ").getItem(0))`： 使用 `split` 函数拆分 `CustomerName` 列，提取出姓和名。

+ **学习要点**：

+ **数据规整与丰富**： 这是数据准备的关键步骤。学生学习如何根据业务需求，从现有数据中派生出新的、更有价值的特征列。

### 保存为 Parquet 格式

+ **代码功能**： `transformed_df.write.mode("overwrite").parquet('Files/transformed_data/orders')` 这段代码将转换后的 `DataFrame` 以 Parquet 格式保存到湖屋的 `Files` 目录中。

+ **学习要点**：

+ **列式存储**： 了解 `Parquet` 格式的优势。它是一种高效的列式存储格式，非常适合大规模数据分析，能显著提升查询性能并节省存储空间。

+ **数据生命周期**： 理解数据处理的完整流程：原始数据 -> 转换后的中间数据 -> 最终应用。保存中间结果是数据工程中的常见实践。

### 分区保存 （Partitioning）

+ **代码功能**： `orders_df.write.partitionBy("Year","Month")...parquet(...)` 这段代码在保存 Parquet 文件时，额外指定了按“年”和“月”进行分区。

+ **学习要点**：

+ **性能优化**： 这是 Spark 中最重要的数据布局优化技巧之一。通过分区，数据被物理地存储在 `Year=2021/Month=12` 这样的子文件夹结构中。当查询带有 `WHERE Year = 2021` 这样的条件时，Spark 可以直接跳过所有其他年份的文件夹，极大地减少了需要读取的数据量，从而实现查询性能的飞跃。

## 使用 SQL 和表

> 这部分内容旨在告诉学生，除了使用 Python API，还可以利用他们已有的 SQL 技能在 Spark 中进行数据分析，体现了 Lakehouse 的核心理念。

### 创建 Delta 表

+ **代码功能**： `df.write.format("delta").saveAsTable("salesorders")` 这段代码将 `DataFrame` 保存为一个由 Spark Metastore 管理的 Delta 表。

+ **学习要点**：

+ **文件到表的抽象**： 理解 Spark 如何在文件系统（数据湖）之上提供一个关系型数据库的抽象层。分析师不再需要关心底层是 Parquet 还是 CSV 文件，只需像查询数据库表一样查询 `salesorders`。

+ **Delta Lake 的价值**： 实验强调了使用 Delta 格式是 Fabric 中的首选，因为它为数据湖带来了事务性、可靠性和版本控制等数据库功能。

### 执行 SQL 查询

+ **代码功能**：

+ `spark.sql("SELECT ...")`： 在 PySpark 代码中嵌入并执行一条 SQL 语句，其返回结果是一个 `DataFrame`。

+ `%%sql`： 使用 Notebook 的“魔法命令”将整个代码单元格的语言切换为 SQL，可以直接编写和运行 SQL 查询。

+ **学习要点**：

+ **语言灵活性**： 学生可以根据任务需求和个人偏好，在同一个 Notebook 中无缝切换 Python 和 SQL，极大地提高了开发效率。

## 数据可视化

> 最后一部分将分析结果以图表的形式呈现，让数据洞察更加直观。

+ **代码功能**： 实验展示了三种可视化方法：

### 内置图表工具

直接在 SQL 查询结果上点击按钮生成简单的图表。

### Matplotlib

使用经典的 Python 可视化库。这需要一个关键步骤：`df_spark.toPandas()`，将 Spark `DataFrame` 转换为 Pandas `DataFrame`。

### Seaborn

在 `Matplotlib` 基础上构建的更高级的库，可以用更简洁的代码生成更美观的图表。

+ **学习要点**：

+ **结果呈现**： 数据分析的最后一公里是将数字和表格转化为有意义的视觉故事。

+ Spark 与 Python 生态的集成： 理解 Spark 主要负责大规模分布式计算，而当数据聚合到较小规模（可以装入单机内存）时，通常会将其转换为 Pandas `DataFrame`，以便利用 Python 生态中丰富的可视化和数据科学库（如 `Matplotlib`， `Seaborn`， `Scikit-learn`）。这是一个非常重要且常见的实践模式。

> **总结：实验核心功能点**

> 该实验旨在让学生全面掌握在 Fabric 中使用 Spark 进行数据分析的核心技能，包括：

### 端到端的数据流程

体验从数据加载、清洗、转换、聚合到存储和可视化的完整过程。

### Spark DataFrame API

熟练使用 `select`， `filter`， `groupBy`， `withColumn` 等核心 API 对数据进行操作。

### 性能优化基础

通过学习 `Parquet` 格式和数据分区，了解提升大数据查询性能的关键技术。

### SQL 与代码的融合

理解 Lakehouse 架构的优势，能够在代码和 SQL 之间灵活切换，满足不同角色的分析需求。

### 生态系统集成

掌握如何将 Spark 与 Pandas、`Matplotlib` 等标准 Python 数据科学生态库结合使用。

## 在 Microsoft Fabric 中使用 Delta Lake 表 （DP-601）

Microsoft Fabric 湖屋中的表基于 Apache Spark 中常用的 Linux 基础 Delta Lake 表格式。 Delta Lake 是 Spark 的开源存储层，它支持用于批处理和流式处理数据的关系数据库功能。 通过使用 Delta Lake，可以在 Spark 中实现湖屋体系结构以支持基于 SQL 的支持事务和架构强制实施的数据操作语义。 结果是一个分析数据存储，它提供关系数据库系统的许多优势，以及数据湖中数据文件存储的灵活性。

虽然无需直接使用 Delta Lake API 即可在 Fabric 湖屋中使用表，但了解 Delta Lake 元存储体系结构并熟悉一些更专业的 Delta 表操作可以极大地扩展你在 Microsoft Fabric 上构建高级分析解决方案的能力。

### 什么是 Delta Lake？

Delta Lake 是一个开源存储层，它将关系数据库语义添加到基于 Spark 的数据湖处理中。它能够为 Spark 提供类似关系数据库的事务支持，包括 `ACID` 事务、数据版本控制和时间旅行，以及对批处理和流式处理数据的支持。在 Microsoft Fabric 湖屋中，所有表都是 Delta 表，并以三角形 Delta （Δ） 图标表示。

### Delta 表如何存储数据和元数据？

Delta 表是对以 Delta 格式存储的数据文件的架构抽象。对于每个 Delta 表，湖屋会存储一个包含 `Parquet` 数据文件的文件夹和一个名为 `_delta_log` 的文件夹，后者以 `JSON` 格式记录事务详情。这种结构允许 Delta Lake 提供 `ACID` 事务、数据版本控制和时间旅行等功能。

### 使用 Delta 表有哪些主要优势？

使用 Delta 表的主要优势包括：

+ 支持关系型操作：支持 `CRUD`（创建、读取、更新和删除）操作，允许像在关系数据库中一样处理数据行。

+ `ACID` 事务：通过实现事务日志和强制执行并发操作的可序列化隔离，确保原子性、一致性、隔离性和持久性。

+ 数据版本控制和时间旅行：由于所有事务都记录在事务日志中，因此可以跟踪每个表行的多个版本，并使用时间旅行功能检索某个行的先前版本。

+ 支持批处理和流式处理数据：Delta Lake 表可以用作 Spark 结构化流式处理数据的接收器（目标）和源。

+ 标准格式和互操作性：底层数据以 `Parquet` 格式存储，并且可以通过 Microsoft Fabric 湖屋的 SQL 分析终结点进行 SQL 查询。

### Delta Lake 如何解决“小文件问题”并优化读取性能？

Delta Lake 通过以下功能解决“小文件问题”并优化读取性能：

+ `OptimizeWrite` 函数：在数据写入时减少文件数量，通过写入更少、更大的文件来防止“小文件问题”。在 Microsoft Fabric 中，此功能默认启用。

+ `OPTIMIZE` 命令：表维护功能，可以将小型 `Parquet` 文件合并为更少、更大的文件，从而提高压缩率和跨节点的有效数据分布。

+ `V-Order` 函数：优化运行时的可选功能，专为 Fabric 中的 `Parquet` 文件格式设计。`V-Order` 通过应用特殊的排序、行组分布、字典编码和压缩，实现闪电般的快速读取，显著提高 Microsoft Fabric 计算引擎（如 Power BI、`SQL`、`Spark` 等）的读取速度。

### 什么是 V-Order，它如何提高性能？

`V-Order` 是 Delta Lake 在 Microsoft Fabric 中的一项优化功能，旨在最大化 `Parquet` 文件的读取速度。它通过对 `Parquet` 文件应用特殊的排序、行组分布、字典编码和压缩来实现这一点。`V-Order` 在写入数据时默认启用，虽然会带来大约 15% 的小写入开销，但能使 Microsoft Fabric 计算引擎（如 Power BI 和 `SQL` 引擎利用 `Verti-Scan` 技术，`Spark` 和其他引擎也能受益）的读取速度大幅提升。

### VACUUM 命令在 Delta 表维护中扮演什么角色？

`VACUUM` 命令用于移除 Delta 表中旧的、不再被事务日志引用的数据文件。每次更新或删除操作都会创建新的 `Parquet` 文件并保留旧文件以支持时间旅行，导致文件随时间累积。运行 `VACUUM` 会永久删除超过指定保留期（默认 7 天）的数据文件，从而释放存储空间。需要注意的是，一旦文件被 `VACUUM` 移除，时间旅行就不能回溯到该保留期之前。

### 何时应该对 Delta 表进行分区？

对 Delta 表进行分区是一种优化技术，通过基于元数据跳过不相关的数据对象（称为“数据跳过”）来提高查询性能。您应该在以下情况下考虑分区：

+ 数据量非常大：当数据集庞大时，分区能显著提升性能。

+ 表可以拆分为几个大型分区：分区列应该能将数据有效分割成数量适中、大小合理的分区。

不建议在以下情况下进行分区：

+ 数据量较小：分区会增加文件数量，可能加剧“小文件问题”。

+ 分区列基数较高：基数过高会导致创建大量小分区，反而降低性能。

+ 分区列将导致多个级别：多级别分区可能使数据管理和查询复杂化。

分区是固定的数据布局，因此在决定分区策略时应考虑数据的使用方式和粒度。

### Delta 表如何支持流式处理数据？

Delta 表可以无缝地作为 Spark 结构化流式处理的源和接收器。这意味着：

+ 作为流式处理接收器：可以从 `IoT` 设备或其他实时数据源捕获数据流，并将数据流直接写入 Delta 表。

+ 作为流式处理源：可以从 Delta 表读取数据，从而在表中添加新数据时几乎实时地报告和处理这些新数据。

Spark 结构化流式处理 `API` 允许用户像处理批处理数据一样处理流式处理数据，Delta Lake 的事务日志特性确保了流处理过程中的数据一致性和可靠性。

### 练习 - 在 Apache Spark 中使用 Delta 表

Microsoft Fabric 湖屋中的表基于开源 Delta Lake 格式。 Delta Lake 添加了对批处理和流式数据的关系语义的支持。 在本练习中，你将创建 Delta 表并使用 SQL 查询浏览数据。

https://microsoftlearning.github.io/mslearn-fabric.zh-cn/Instructions/Labs/03-delta-lake.html

>

#### 环境准备

首先，创建一个新的 Fabric 工作区和一个湖屋（Lakehouse）作为实验环境。 （略）

#### 数据上传与读取

将一个名为 `products.csv` 的数据文件上传到湖屋中，然后使用 Spark Notebook 将其读取为数据帧（`DataFrame`）。 （略）

#### 创建 Delta 表

> 这部分旨在让学生理解 Delta Lake 中最核心的表格式概念，特别是托管表和外部表之间的区别。

##### 创建托管表 （Managed Table）

+ **代码功能**： `df.write.format("delta").save("Tables/products")` 这段代码将之前读取的 `products.csv` 文件的数据帧（`DataFrame`）以 Delta 格式保存到湖屋的 `Tables` 目录中，并自动创建一个名为 `products` 的 SQL 表。

+ **学习要点**：

+ **简便性**： 这是在 Fabric 中创建 Delta 表最直接的方式。

+ **统一管理**： Fabric 会同时管理表的元数据（表结构、名称等）和底层的数据文件（`Parquet` 文件）。 如果你用 SQL 命令 `DROP TABLE`，元数据和数据会一起被删除。

##### 创建外部表 （External Table）

+ **代码功能**： `df.write.format("delta").save(f"{lakehouse_path}/products_external")` 先将数据以 Delta 格式保存到湖屋的 `Files` 目录下的一个指定位置。 接着，`spark.sql(f"CREATE TABLE products_external_sql ...")` 这段代码在 Fabric 的元数据中注册一个表，但告诉它数据实际存储在之前指定的 `Files` 路径下。

+ **学习要点**：

+ **灵活性**： 实现了数据和元数据的分离。 你可以删除这个表（`DROP TABLE`），但底层的数据文件会保留下来，可以被其他表或服务重新引用。

+ **数据共享**： 这种方式更适合于多个计算引擎或团队需要访问同一份底层数据的场景。

#### 使用 Delta Lake 表的版本控制（时间旅行）

> 这部分是 Delta Lake 的核心亮点功能，展示了其如何自动管理数据版本并允许用户访问历史数据。

##### 更新数据

+ **代码功能**： `new_df.write.format("delta").mode("overwrite").save("Tables/products")` 这段代码用一个新的数据帧 `new_df` 的内容覆盖 `products` 表。

+ **学习要点**： 演示了对 Delta 表进行更新操作（在这里是覆盖写）。 Delta Lake 会将这次更改记录为一个新的版本，而不会直接删除旧数据。

##### 查看历史记录

+ **代码功能**： `spark.sql("DESCRIBE HISTORY products").show()` 这条命令会查询并显示 `products` 表的事务日志。 输出会包含版本号、操作类型（如 `WRITE`， `CREATE`）、时间戳和操作用户等信息。

+ **学习要点**：

+ **数据可审计性**： Delta Lake 提供了清晰的数据血缘和审计追踪能力，每一次变更都有记录。

+ **事务日志**： 这是 Delta Lake 实现 `ACID` 事务和版本控制的基础。

##### 查询旧版本数据（时间旅行）
