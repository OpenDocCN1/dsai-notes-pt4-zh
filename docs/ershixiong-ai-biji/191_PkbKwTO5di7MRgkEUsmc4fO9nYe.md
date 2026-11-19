
# MCP 实战：MCP+SQLite 用自然语言操作数据库

> 来源：[https://qianniucity.feishu.cn/docx/GhTAdqYvloztIqx81pqcH4n4nXw](https://qianniucity.feishu.cn/docx/GhTAdqYvloztIqx81pqcH4n4nXw)

## 开源项目介绍

该项目提供了一个通过Model Context Protocol（MCP）对SQLite数据库进行安全只读访问的MCP服务器，其使用FastMCP框架构建，使大语言模型（LLMs）能够探索和查询SQLite数据库，具有内置安全特性和查询验证功能。

### 项目地址

`sqlite-explorer-fastmcp-mcp-server`：https://github.com/hannesrudolph/sqlite-explorer-fastmcp-mcp-server

### 项目概述

+ 功能特性：为SQLite数据库提供安全的只读访问，允许LLMs通过MCP探索和查询数据库，具备查询验证、参数绑定、行限制执行等安全特性，查询结果格式化为字典，可列出数据库所有表名及获取特定表的详细架构信息。

+ 应用场景：适用于需要LLMs与SQLite数据库进行交互的场景，如数据分析、信息检索等，通过安全的方式为LLMs提供数据库访问能力，保障数据安全的同时实现数据利用。

### 系统要求与依赖

+ 系统要求：Python 3.6及以上版本，需指定SQLite数据库文件路径（通过环境变量）。

+ 依赖安装：使用`pip`安装`requirements.txt`中指定的所有依赖项，主要依赖fastmcp框架用于构建MCP服务器。

### 工具与功能

+ MCP工具

+ `read_query`：执行SELECT查询，有安全验证、参数绑定、行限制及格式化结果等功能。

+ `list_tables`：列出数据库所有可用表名。

+ `describe_table`：获取特定表的详细架构信息，包括列名、类型、约束、默认值和主键等。

+ 安装选项

+ Claude Desktop：使用`fastmcp install sqlite_explorer.py --name "SQLite Explorer" -e SQLITE_DB_PATH=/path/to/db`命令安装，需替换为实际数据库文件路径。

+ Cline VSCode插件：在VSCode的Cline插件中配置相关参数，包括命令、参数和环境变量，需指定仓库路径和数据库文件路径。

### 安全特性与文档

+ 安全特性：提供只读访问，进行查询验证和清理、参数绑定、行限制执行，抑制进度输出以获得干净的JSON响应。

+ 开发文档：包含`mcp-documentation.txt`，详细记录MCP服务器实现和FastMCP框架使用方法，为开发功能提供上下文信息，并可辅助LLMs进行开发。

### 环境变量

需设置`SQLITE_DB_PATH`为要探索的SQLite数据库文件的完整路径。项目主要使用Python语言开发，涵盖的主题包括python、sqlite、mcp、数据库工具、LLM工具和LLM集成等。

## 执行安装

> 以 Macos 为例

### 数据库安装与创建

```
## 安装 sqlite3
brew install sqlite3

## 查看版本号
sqlite3 --version

## 在当前目录下 创建数据库
sqlite3 test.db
```

```
-- 创建表 products
CREATE TABLE products (
        id INTEGER PRIMARY KEY,
        name TEXT,
        price REAL
);

-- 插入数据
INSERT INTO products (name,price) VALUES
('Widget',19.99),
('Gadget', 29.99),
('Gizmo',39.99),
('Smart Watch',199.99),
('Wireless Earbuds',89.99),
('Portable Charger',24.99),
('Bluetooth Speaker',79.99),
('Phone Stand', 15.99),
('Laptop Sleeve', 34.99),
('Mini Drone', 299.99),
('LED Desk Lamp', 45.99),
('Keyboard',129.99),
('Mouse Pad',12.99),
('USB Hub', 49.99),
('Webcam',69.99),
('Screen Protector',9.99),
('Travel Adapter',27.99),
('Gaming Headset', 159.99),
('Fitness Tracker',119.99),
('Portable SSD', 179.99);
```

### 项目安装执行

```
## 下载
git clone https://github.com/hannesrudolph/sqlite-explorer-fastmcp-mcp-server.git
cd sqlite-explorer-fastmcp-mcp-server

##Using pip 安装依赖
pip install -r requirements.txt
```

### 使用 配置 MCP

```
{
  "mcpServers": {
    "sqlite-explorer": {
      "command": "uv",
      "args": [
        "run",
        "--with",
        "fastmcp",
        "fastmcp",
        "run",
        "/Volumes/ExtremeSSD/Github/sqlite-explorer-fastmcp-mcp-server/sqlite_explorer.py"
      ],
      "env": {
        "SQLITE_DB_PATH": "/Volumes/ExtremeSSD/Github/sqlite-explorer-fastmcp-mcp-server/test.db"
      }
    }
  }
}
```

### 服务启动效果

![](img/363375309098e4167764bba503d03dc7.png)

## 参考链接

+ SQLite官网：https://www.sqlite.org/download.html

+ SQLite 教程：https://www.runoob.com/sqlite/sqlite-tutorial.html
