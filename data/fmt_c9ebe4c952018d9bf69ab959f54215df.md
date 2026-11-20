
# 如何使用 NLWeb 的数据库加载工具 db_load.py 进行数据加载和管理

> 来源：[https://qianniucity.feishu.cn/docx/QgtVdgRtYo9XX4xYvFdcAepInmC](https://qianniucity.feishu.cn/docx/QgtVdgRtYo9XX4xYvFdcAepInmC)

主要内容总结如下：

## 工具位置与功能

`db_load.py` 位于 `code/tools` 目录下，可用于计算嵌入并将数据加载到向量数据库、删除数据库中的文件或站点相关文件。

## 数据格式要求

支持多种数据格式，包括：

1. 每行两个用 tab 分隔的列（URL 和 JSON）
2. 仅 JSON（URL 从 JSON 中提取）
3. 带表头的 CSV
4. RSS/Atom feed
5. 指向上述类型的 URL

## 加载数据流程

1. 指定数据源（本地或 URL）
2. 工具根据配置计算嵌入
3. 将嵌入加载到向量数据库（需有写权限）示例命令：`python -m tools.db_load <file-path> <site-name>`

## 删除数据库条目

可通过参数删除某个站点的数据库条目：`python -m tools.db_load --only-delete delete-site <site-name>`

## 可选参数

- 指定不同数据库：`--database <db-name>`
- 批量加载 URL：`--url-list`
- 修改批处理大小：`--batch-size <number>`

该工具支持灵活的数据加载、删除和批量处理，适用于多种数据源和数据库配置。
