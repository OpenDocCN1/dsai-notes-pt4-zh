
# MCP 使用指南

## 第二部分：MCP基础验证

### 预期响应

```
Claude Code：
[调用 filesystem.list_directory]
路径: .

目录内容：
- .mcp.json (配置文件)
- package.json
- src/
- node_modules/
...
```

### 更多测试

```
你：读取package.json文件内容

你：在src目录下创建一个test.txt文件，内容是"Hello MCP"

你：搜索所有.js文件
```

### 2.2 验证MCP工作正常

### 完整验证清单

### 2.3 第一个MCP工具调用

**恭喜你！** 如果上面的测试都成功了，你已经完成了MCP的基础配置。

### 你刚才完成了什么？

1. ✅ 创建了MCP配置文件
1. ✅ 连接了Filesystem MCP服务器
1. ✅ 通过Claude Code调用了MCP工具
1. ✅ 验证了MCP正常工作

### 接下来可以

+ 继续学习更多MCP服务器（第三部分）
+ 了解MCP工作原理（第四部分）
+ 开发自己的MCP服务器（第五部分）

---

## 第三部分：常用MCP服务器配置（实战为主）

> **本节目的**：学会配置10+最常用的MCP服务器

> ⏱️ **预计时间**：1-2小时

### 3.1 配置方式详解

#### 方式1：JSON配置文件（推荐）

##### 创建 `.mcp.json` 文件

```
{
  "mcpServers": {
    "服务器名称1": {
      "command": "启动命令",
      "args": ["参数1", "参数2"],
      "env": {
        "环境变量名": "值"
      }
    },
    "服务器名称2": {
      ...
    }
  }
}
```

#### 配置字段说明

| 字段 | 必需 | 说明 | 示例 |
| --- | --- | --- | --- |
| `command` | ✅ | 启动命令 | `npx`, `node`, `python` |
| `args` | ✅ | 命令参数数组 | [`-y`, `@modelcontextprotocol/server-xxx`] |
| `env` | ❌ | 环境变量对象 | {`API_KEY`: `xxx`} |
| `timeout` | ❌ | 超时时间(毫秒) | 60000 |

#### 方式2：CLI命令（快速添加）

```
# 添加MCP服务器
claude mcp add <服务器名> <命令> [参数...]

# 示例：添加filesystem服务器
claude mcp add filesystem npx -y @modelcontextprotocol/server-filesystem ./data

# 列出所有MCP服务器
claude mcp list

# 查看特定服务器详情
claude mcp get filesystem

# 删除服务器
claude mcp remove filesystem
```

#### CLI添加的作用域

```
# 默认添加到local作用域
claude mcp add my-server npx -y xxx

# 指定添加到project作用域
claude mcp add --scope project my-server npx -y xxx

# 指定添加到user作用域（全局）
claude mcp add --scope user my-server npx -y xxx
```

#### 方式对比

| 对比维度 | JSON配置文件 | CLI命令 |
| --- | --- | --- |
| 适合场景 | 团队项目、版本控制 | 快速测试、个人配置 |
| 可读性 | 高（结构清晰） | 中 |
| 批量配置 | 方便（一个文件） | 不便（逐个添加） |
| 版本控制 | 支持 | 不支持 |
| 推荐度 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |

### 3.2 GitHub MCP服务器

> **功能**：完整的GitHub仓库管理能力，包括创建仓库、管理Issue、PR等

#### 配置步骤

##### 步骤1：获取GitHub Token

1. 登录 GitHub.com
1. 点击右上角头像 → Settings
1. 左侧菜单最下方 → Developer settings
1. Personal access tokens → Tokens (classic)
1. Generate new token (classic)
1. 勾选权限：
+ ✅ `repo`（完整仓库访问）
+ ✅ `workflow`（如需管理Actions）
1. 点击 Generate token
1. **立即复制保存Token**（只显示一次！）

> ⚠️ **安全提醒**：Token只显示一次，关闭页面就看不到了！

##### 步骤2：配置环境变量

###### Windows（PowerShell 7）

```
# 永久设置环境变量
[System.Environment]::SetEnvironmentVariable('GITHUB_PERSONAL_ACCESS_TOKEN', 'ghp_你的Token', 'User')

# 验证
$env:GITHUB_PERSONAL_ACCESS_TOKEN
# 应显示你的Token
```

###### macOS/Linux

```
# 添加到shell配置文件
echo 'export GITHUB_PERSONAL_ACCESS_TOKEN="ghp_你的Token"' >> ~/.zshrc
source ~/.zshrc

# 验证
echo $GITHUB_PERSONAL_ACCESS_TOKEN
```

##### 步骤3：添加MCP配置

在 `.mcp.json` 中添加：

```
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_PERSONAL_ACCESS_TOKEN}"
      }
    }
  }
}
```

> 💡 **说明：**`${GITHUB_PERSONAL_ACCESS_TOKEN}` 会自动读取环境变量，不用把Token直接写在配置文件里

##### 步骤4：验证配置

```
# 重启Claude Code
claude

# 测试
你：查看我的GitHub仓库列表
你：创建一个Issue到xxx仓库，标题是"测试MCP"
```

#### 提供的工具

| 工具名 | 功能 | 参数 |
| --- | --- | --- |
| `create_repository` | 创建仓库 | `name`, `description`, `private` |
| `get_file_contents` | 获取文件内容 | `owner`, `repo`, `path` |
| `push_files` | 推送文件 | `owner`, `repo`, `branch`, `files` |
| `create_issue` | 创建Issue | `owner`, `repo`, `title`, `body` |
| `create_pull_request` | 创建PR | `owner`, `repo`, `title`, `head`, `base` |
| `fork_repository` | Fork仓库 | `owner`, `repo` |
| `create_branch` | 创建分支 | `owner`, `repo`, `branch` |
| `search_repositories` | 搜索仓库 | `query` |
| `search_code` | 搜索代码 | `query` |
| `search_issues` | 搜索Issues | `query` |

### 3.3 数据库MCP服务器

#### SQLite（本地数据库）

> **功能**：本地SQLite数据库的完整访问，无需安装数据库软件

##### 配置方法

```
{
  "mcpServers": {
    "sqlite": {
      "command": "uvx",
      "args": ["mcp-server-sqlite", "--db-path", "./data/app.db"],
      "env": {}
    }
  }
}
```

> 💡 **说明：**最后的参数是数据库文件路径，不存在会自动创建。SQLite MCP是Python包，需要用 `uvx` 而非 `npx`

##### 提供的工具

| 工具名 | 功能 | 参数 |
| --- | --- | --- |
| `read_query` | 执行SELECT查询 | `query` |
| `write_query` | 执行INSERT/UPDATE/DELETE | `query` |
| `create_table` | 创建表 | `query` |
| `list_tables` | 列出所有表 | - |
| `describe_table` | 获取表结构 | `table_name` |
| `append_insight` | 添加分析洞察 | `insight` |

##### 使用示例

```
你：创建一个users表，包含id、name、email字段
你：插入一条记录：张三，zhangsan@example.com
你：查询所有用户
```

#### PostgreSQL（生产数据库）

> **功能**：连接PostgreSQL数据库，支持查询和结构检查

##### 配置方法

```
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://user:password@localhost:5432/database"]
    }
  }
}
```

> ⚠️ **安全建议**：
+ 使用只读数据库用户
+ 不要在配置文件中硬编码密码
+ 连接字符串通过 args 传递，可使用环境变量替代硬编码：`postgresql://${PGUSER}:${PGPASSWORD}@localhost:5432/database`

##### 提供的工具

| 工具名 | 功能 | 参数 |
| --- | --- | --- |
| `query` | 执行SQL查询 | `sql` |

> 💡 **说明：**`query` 是唯一的Tool。表结构信息（schemas、tables、columns）通过MCP的Resources机制自动暴露，无需手动调用工具即可获取

### 3.4 三作用域配置体系

> 🎯 **关键点：**理解配置的作用域和优先级，是正确使用MCP的关键！

#### 三大作用域

| 作用域 | 存储位置 | 优先级 | 适用场景 |
| --- | --- | --- | --- |
| `Local` | `~/.claude.json` 项目条目 | 最高 | 个人私有配置、含API Key |
| `Project` | 项目根目录 `.mcp.json` | 中等 | 团队共享、版本控制 |
| `User` | `~/.claude.json` 全局部分 | 最低 | 个人常用工具 |

> 💡 **Windows用户路径说明：**
+ `~/.claude.json` 在Windows上对应：`C:\Users\你的用户名\.claude.json`
+ 可以在资源管理器地址栏输入 `%USERPROFILE%` 快速跳转到用户目录

> **作用域名称说明**：
+ **Local**（本地）：虽然叫"本地"，但实际是"项目级个人私有配置"——只对当前项目生效，且不提交到Git
+ **Project**（项目）：项目级共享配置，整个团队都能看到
+ **User**（用户）：全局个人配置，所有项目都能用

#### 优先级合并规则

```
Local > Project > User
```

#### 示例场景

假设三个作用域都配置了 `github` 服务器：

```
User作用域：GITHUB_PERSONAL_ACCESS_TOKEN = "user-token"
Project作用域：GITHUB_PERSONAL_ACCESS_TOKEN = "${GITHUB_PERSONAL_ACCESS_TOKEN}"
Local作用域：GITHUB_PERSONAL_ACCESS_TOKEN = "local-override-token"

最终生效：Local作用域的配置（local-override-token）
```

#### 作用域选择建议

| 场景 | 推荐作用域 | 原因 |
| --- | --- | --- |
| 包含API密钥的配置 | Local | 安全，不会提交到Git |
| 团队必需的工具 | Project | 团队共享，可版本控制 |
| 个人常用工具 | User | 全局可用 |
| 临时测试配置 | Local | 不影响其他配置 |
| CI/CD使用 | Project | 可自动化部署 |

#### 配置方法

##### Local作用域（修改 `~/.claude.json`）

```
{
  "mcpServers": {
    "/Users/me/project1": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx_local_override"
        }
      }
    }
  }
}
```

##### Project作用域（项目根目录 `.mcp.json`）

```
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_PERSONAL_ACCESS_TOKEN}"
      }
    }
  }
}
```

##### User作用域（使用CLI）

```
claude mcp add --scope user github npx -y @modelcontextprotocol/server-github
```

### 3.5 更多常用服务器配置

#### Brave Search（网页搜索）

> **功能**：隐私优先的Web搜索，免费层每月2000次查询

##### 获取API Key

1. 访问 https://brave.com/search/api/
1. 注册账号
1. 创建API Key
1. 免费层：2000次/月

##### 配置方法

```
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "${BRAVE_API_KEY}"
      }
    }
  }
}
```

#### Context7（技术文档）
