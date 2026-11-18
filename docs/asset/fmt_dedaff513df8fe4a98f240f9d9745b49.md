
### 5.3 配置和测试自定义MCP

#### 在Claude Code中使用

**步骤1：在项目中配置**

在你的项目 `.mcp.json` 中添加：

```
{
  "mcpServers": {
    "my-first-mcp": {
      "command": "node",
      "args": ["/path/to/my-first-mcp/dist/index.js"],
      "env": {}
    }
  }
}
```

> ⚠️ **注意**：替换 `/path/to/my-first-mcp` 为你的实际路径

**步骤2：测试**

```
# 启动Claude Code
claude

# 测试
你：用`hello_world`工具问候"老金"
```

**预期响应**：

```
Claude Code：
[调用 `my-first-mcp.hello_world`]
参数: {"name": "老金"}

Hello, 老金! 这是来自`my-first-mcp`的问候！
```

### 5.4 使用MCP Inspector调试

> **这是什么？** `MCP Inspector`是官方提供的调试工具，可以交互式测试MCP服务器。

**使用方法**：

```
# 调试自己的MCP服务器
npx @modelcontextprotocol/inspector node /path/to/my-first-mcp/dist/index.js

# 调试官方服务器
npx @modelcontextprotocol/inspector npx -y @modelcontextprotocol/server-filesystem ./
```

**访问调试界面**：

启动后访问 `http://127.0.0.1:6274`

**Inspector功能**：

+   ✅ 交互式工具测试

+   ✅ 请求/响应查看

+   ✅ 资源浏览

+   ✅ 协议一致性检查

### 5.5 添加更多功能

#### 添加第二个工具

在 `src/index.ts` 的工具列表中添加：

```
// 在 `ListToolsRequestSchema` 处理器中添加
{
  name: "get_current_time",
  description: "获取当前时间",
  inputSchema: {
    type: "object",
    properties: {
      timezone: {
        type: "string",
        description: "时区（如 'Asia/Shanghai'）",
      },
    },
  },
},
```

在 `CallToolRequestSchema` 处理器中添加：

```
if (name === "get_current_time") {
  const timezone = args?.timezone as string || "Asia/Shanghai";
  const now = new Date().toLocaleString("zh-CN", { timeZone: timezone });
  return {
    content: [
      {
        type: "text",
        text: `当前时间（${timezone}）：${now}`,
      },
    ],
  };
}
```

#### 添加Resources

```
import {
  ListResourcesRequestSchema,
  ReadResourceRequestSchema,
} from "@modelcontextprotocol/sdk/types.js";

// 在`Server`配置中启用resources
const server = new Server(
  { name: "my-first-mcp", version: "1.0.0" },
  {
    capabilities: {
      tools: {},
      resources: {},  // 添加这行
    },
  }
);

// 定义资源列表
server.setRequestHandler(ListResourcesRequestSchema, async () => {
  return {
    resources: [
      {
        uri: "config://app-settings",
        name: "应用配置",
        mimeType: "application/json",
      },
    ],
  };
});

// 处理资源读取
server.setRequestHandler(ReadResourceRequestSchema, async (request) => {
  const { uri } = request.params;

  if (uri === "config://app-settings") {
    return {
      contents: [
        {
          uri,
          mimeType: "application/json",
          text: JSON.stringify({ version: "1.0.0", debug: false }, null, 2),
        },
      ],
    };
  }

  throw new Error(`未知资源: ${uri}`);
});
```

### 5.6 发布MCP服务器

#### 发布到npm

**步骤1：确保`package.json`配置正确**

```
{
  "name": "@your-username/my-first-mcp",
  "version": "1.0.0",
  "description": "我的第一个MCP服务器",
  "main": "dist/index.js",
  "bin": {
    "my-first-mcp": "dist/index.js"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/your-username/my-first-mcp"
  },
  "keywords": ["mcp", "claude", "ai-tools"],
  "license": "MIT"
}
```

**步骤2：登录`npm`**

```
npm login
```

**步骤3：发布**

```
# 构建
npm run build

# 发布
npm publish --access public
```

**步骤4：使用已发布的服务器**

```
{
  "mcpServers": {
    "my-first-mcp": {
      "command": "npx",
      "args": ["-y", "@your-username/my-first-mcp"],
      "env": {}
    }
  }
}
```

---

## 第六部分：故障排查

> **本节目的**：帮助你快速定位和解决MCP配置问题

> ⏱️ **预计时间**：按需查阅

### 6.1 常见问题排查流程

```
问题出现
    │
    ▼
1. 检查配置文件 ─── JSON格式错误？→ 修复JSON
    │
    ▼
2. 检查服务器状态 ─── 服务器启动失败？→ 查看日志
    │
    ▼
3. 检查环境变量 ─── API Key缺失？→ 配置环境变量
    │
    ▼
4. 检查网络 ─── 无法下载包？→ 配置镜像/代理
    │
    ▼
5. 检查版本 ─── 版本不兼容？→ 更新`Node.js`/`SDK`
```

### 6.2 按错误类型排查

#### 错误1：服务器无法启动

**错误信息示例**：

```
Error: MCP server 'xxx' failed to start
```

**排查步骤**：

```
# 1. 检查Node.js版本
node --version
# 需要 v18.x.x 或更高

# 2. 检查npx是否可用
npx --version

# 3. 手动运行服务器命令
npx -y @modelcontextprotocol/server-filesystem ./

# 4. 如果报错，查看具体错误信息
```

**常见原因和解决方案**：

| 原因 | 解决方案 |

| --- | --- |

| `Node.js`版本过低 | 升级到`v18+` |

| `npx`不可用 | 重装`npm`或使用`npm`安装 |

| 网络无法访问`npm` | 配置国内镜像 |

| 包名写错 | 检查包名拼写 |

#### 错误2：配置文件格式错误

**错误信息示例**：

```
SyntaxError: Unexpected token in JSON
```

**排查步骤**：

```
# 1. 验证JSON格式
cat .mcp.json | jq .

# 2. 使用在线工具验证
# 访问 https://jsonlint.com/
```

**常见JSON错误**：

| 错误 | 示例 | 修复 |

| --- | --- | --- |

| 缺少逗号 | `"a": 1 "b": 2` | `"a": 1, "b": 2` |

| 多余逗号 | `"a": 1,}` | `"a": 1}` |

| 使用单引号 | `'key': 'value'` | `"key": "value"` |

| 未闭合的引号 | `"key: "value"` | `"key": "value"` |

#### 错误3：环境变量未设置

**错误信息示例**：

```
Error: GITHUB_PERSONAL_ACCESS_TOKEN is required
Error: Missing required environment variable
```

**排查步骤**：

Windows（PowerShell）：

```
# 检查环境变量
$env:GITHUB_PERSONAL_ACCESS_TOKEN
# 如果为空，说明未设置

# 设置环境变量
[System.Environment]::SetEnvironmentVariable('GITHUB_PERSONAL_ACCESS_TOKEN', 'your-token', 'User')

# 重启终端后验证
$env:GITHUB_PERSONAL_ACCESS_TOKEN
```

**macOS/Linux：**

```
# 检查环境变量
echo $GITHUB_PERSONAL_ACCESS_TOKEN

# 如果为空，添加到shell配置
echo 'export GITHUB_PERSONAL_ACCESS_TOKEN="your-token"' >> ~/.zshrc
source ~/.zshrc
```

#### 错误4：网络超时

**错误信息示例**：

```
npm ERR! code ETIMEDOUT
npm ERR! network request to https://registry.npmjs.org failed
```

**解决方案**：

```
# 方案1：使用国内镜像
npm config set registry https://registry.npmmirror.com

# 方案2：配置代理（如果有代理工具）
npm config set proxy http://127.0.0.1:7890
npm config set https-proxy http://127.0.0.1:7890

# 方案3：增加超时时间
npm config set timeout 60000
```

### 6.3 查看日志

#### Claude Code日志

**macOS：**

```
tail -f ~/Library/Logs/Claude/mcp*.log
```

**Windows：**

```
Get-Content "$env:LOCALAPPDATA\Claude\logs\mcp*.log" -Wait
```

**Linux：**

```
tail -f ~/.local/share/claude/logs/mcp*.log
```

#### 调试模式启动

```
# 开启MCP调试日志
claude --mcp-debug
```

### 6.4 重置MCP配置

如果配置混乱，可以重置：

```
# 删除项目级配置
rm .mcp.json

# 删除用户级配置（谨慎！）
# macOS/Linux
rm ~/.claude.json

# Windows
del %USERPROFILE%\.claude.json
```

---

## 第七部分：FAQ（20个常见问题）

### 基础问题

#### Q1：`MCP`和普通`API`有什么区别？

**A**：`MCP`是协议标准，`API`是具体实现。

| 对比 | `MCP` | 普通`API` |

| --- | --- | --- |

| **定位** | 通用协议标准 | 具体服务接口 |

| **兼容性** | 跨平台、跨AI | 仅特定服务 |

| **学习成本** | 学一次用到处 | 每个API都不同 |

| **类比** | USB标准 | 某品牌的USB设备 |

#### Q2：`MCP`服务器需要一直运行吗？

**A**：不需要。`Claude Code`会在需要时自动启动`MCP`服务器，使用完毕后自动关闭。

#### Q3：配置多个`MCP`服务器会影响性能吗？

**A**：不会。`MCP`服务器是按需启动的，只有被调用时才会运行。配置10个服务器但只用1个，不会有额外开销。

#### Q4：`MCP`服务器的数据安全吗？

**A**：取决于配置。

+   ✅ Filesystem `MCP`只能访问你指定的目录

+   ✅ 所有操作都需要你确认

+   ⚠️ 但`API Key`等敏感信息要妥善保管

+   ⚠️ 不要把敏感Token写在配置文件里

### 配置问题

#### Q5：`.mcp.json`放在哪里？

**A**：项目根目录。和`package.json`、`.git`目录同级。

#### Q6：Local、Project、User作用域怎么选？

**A**：

| 情况 | 选择 |

| --- | --- |

| 包含`API Key` | Local（不会提交Git） |

| 团队共享 | Project |

| 个人常用 | User |

| 临时测试 | Local |

#### Q7：环境变量 `${VAR}` 语法不生效？

**A**：检查以下几点：

1.  环境变量名正确（区分大小写）

1.  环境变量已导出

1.  终端已重启

1.  使用 `echo $VAR` 验证

#### Q8：可以同时用多个数据库`MCP`吗？

**A**：可以，用不同名称：

```
{
  "mcpServers": {
    "sqlite-app": {
      "command": "uvx",
      "args": ["mcp-server-sqlite", "--db-path", "./app.db"]
    },
    "sqlite-analytics": {
      "command": "uvx",
      "args": ["mcp-server-sqlite", "--db-path", "./analytics.db"]
    }
  }
}
```

### 故障问题

#### Q9：`MCP`服务器启动失败怎么办？

**A**：按以下步骤排查：

1.  检查`Node.js`版本（需要18+）

1.  手动运行`npx`命令看报错

1.  检查网络是否能访问`npm`

1.  查看`Claude Code`日志
