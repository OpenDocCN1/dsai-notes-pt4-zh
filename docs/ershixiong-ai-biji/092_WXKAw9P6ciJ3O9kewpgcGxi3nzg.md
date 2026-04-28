# Cursor MCP Server 配置与使用指南（Window）

> 来源：[https://qianniucity.feishu.cn/docx/ID1sd9fDZoV6plxREricLxOln4b](https://qianniucity.feishu.cn/docx/ID1sd9fDZoV6plxREricLxOln4b)

### 引言

本文将详细介绍如何安装、配置和使用 Cursor MCP Server，包括常见问题的排查和注意事项。通过本文的指导，您可以快速上手并高效使用 MCP Server 提供的强大功能。

### 一、环境准备

在开始安装 MCP Server 之前，请确保您的开发环境满足以下要求：

**卸载旧版本的 Node.js**：如果您的电脑上已经安装了旧版本的 Node.js，建议先卸载以避免版本冲突。

安装最新 LTS 版本的 Node.js：下载并安装 Node.js 的长期支持（LTS）版本，以确保兼容性和稳定性。

**重启电脑**：安装完成后，建议重启电脑以确保环境变量生效。

> 注，版本合适不需要卸载重装

### 二、验证环境

完成环境准备后，需要验证 Node.js 和 npm 是否正确安装。步骤如下：

1.  打开 **CMD**（注意：不是 PowerShell）。

1.  输入以下命令，检查版本号是否正确显示：

```
node --version
npm --version
```

如果能够看到版本号，说明安装成功。

### 三、常见 MCP Server 的安装与配置

以下是三种常见 MCP Server 的安装和 Cursor 配置方法：

1.  Sequential Thinking MCP Server

+   安装命令

```
npm install -g @modelcontextprotocol/server-sequential-thinking
```

+   Cursor 配置

+   名称：sequentialthinking

+   传输方式：**stdio**

+   命令：

```
cmd /c npx @modelcontextprotocol/server-sequential-thinking
```

1.  Code Completion MCP Server

+   安装命令

```
npm install -g @modelcontextprotocol/server-code-completion
```

+   • Cursor 配置

+   名称：**codecompletion**

+   传输方式：**stdio**

+   命令：

```
cmd /c npx @modelcontextprotocol/server-code-completion
```

1.  Code Review MCP Server

+   安装命令

```
npm install -g @modelcontextprotocol/server-code-review
```

+   Cursor 配置

+   名称：**codereview**

+   传输方式：**stdio**

+   命令：

```
cmd /c npx @modelcontextprotocol/server-code-review
```

### 四、使用步骤

完成 MCP Server 的安装和配置后，按照以下步骤使用：

1.  打开 **CMD****（快捷**键：Win + R，输入 cmd）。

1.  输入对应 MCP Server 的运行命令，例如：

```
npx @modelcontextprotocol/[Server 包名]
```

1.  **如果看**到成功启动的消息，请保持 CMD 窗口打开。

1.  打开 Cursor，即可使用对应功能。

### 五、故障排查

如果配置后无法正常工作，请检查以下问题：

1.  确保使用 **CMD** 而不是 PowerShell：MCP Server 的运行依赖 CMD，而非 PowerShell。

1.  检查 **Node.js** 和 npm 的版本：确保您安装的是最新的 LTS 版本。

1.  重新安装 **MCP Server** 包：如果问题仍然存在，可以尝试卸载并重新安装相关包。

1.  检查命令格式是否正确：确保命令中包含 cmd /c 前缀。

1.  5. **重启 Cursor**：配置保存后，需要重启 Cursor 才能生效。

1.  6. **保持 CMD** 窗口运行：MCP Server 的运行窗口不能关闭，否则服务将停止。

### 六、注意事项

在使用 MCP Server 时，请特别注意以下几点：

+   每个 MCP Server 都需要单独的 CMD 窗口运行。

+   必须使用 CMD，而非 PowerShell 或其他终端工具。

+   配置完成后，需重启 Cursor 才能生效。

+   命令中必须包含 cmd /c 前缀，否则可能无法正常运行。

+   MCP Server 的运行窗口不能关闭，否则服务会中断。

### 七、卸载命令

如果需要卸载某个 MCP Server，可以使用以下命令：

```
npm uninstall -g @modelcontextprotocol/[Server 包名]
```

将 [Server 包名] 替换为对应的 MCP Server 名称，例如 server-sequential-thinking。

### 八、清理缓存（可选）

如果遇到异常问题，可以尝试清理 npm 缓存：

```
npm cache clean --force
```

清理完成后，重新安装相关包。

通过以上步骤，您可以轻松完成 Cursor MCP Server 的安装与配置。如果在使用过程中遇到问题，请参考故障排查部分进行解决。祝您使用愉快！