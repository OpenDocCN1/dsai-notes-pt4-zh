# Claude Code 和 Codex 接入 Figma MCP 保姆级教程

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/IfF5d2FvZor6rHxvLrHcLF51nUf](https://qqs7y1hozd1.feishu.cn/docx/IfF5d2FvZor6rHxvLrHcLF51nUf)

## Claude Code 接入配置

### 方式一：接入 Desktop MCP（本地）

在终端中执行以下命令：

```
claude mcp add --transport http figma-desktop http://127.0.0.1:3845/mcp
```

**参数说明：**

+   --transport http：使用 HTTP 协议连接

+   figma-desktop：MCP 服务器标识符（可自定义）

+   http://127.0.0.1:3845/mcp: Desktop MCP 本地地址

**管理命令：**

```
# 查看所有 MCP 服务器
claude mcp list

# 删除 MCP 服务器
claude mcp remove figma-desktop

# 查看配置详情
claude mcp info figma-desktop
```

### 方式二：接入 Remote MCP（云端）

```
claude mcp add --transport http figma-remote https://mcp.figma.com/mcp
```

**使用示例：**

连接成功后，在 Claude Code 中可以这样提问：

> “请读取我在 Figma Desktop 中当前选中的 Frame，分析其布局和样式，然后为我的 React 项目生成一个对应的组件。”

## Codex 接入配置

Codex by OpenAI 对 Figma MCP 的支持非常完整！

### 接入 Desktop MCP（本地）

在 Codex 的 MCP 配置中添加：

```
{
  &quot;mcpServers&quot;: {
    &quot;figma-desktop&quot;: {
      &quot;url&quot;: &quot;http://127.0.0.1:3845/mcp&quot;,
      &quot;transport&quot;: &quot;http&quot;
    }
  }
}
```

配置保存后，重启 Codex 即可使用！

### Codex 支持的能力

+   Desktop MCP server

+   Remote MCP server

+   Code to canvas（Remote 独有）

+   Skills（预设工作流）

### 接入 Remote MCP（推荐）

1.  打开 Codex 的 MCP 或 Tools/Servers 配置界面

1.  新增一个 MCP server

1.  URL 填写：https://mcp.figma.com/mcp

1.  保存配置并重启/刷新 Codex

**Remote MCP 的优势：**

+   不依赖 Desktop 本地运行

+   支持「Code to Canvas」- 将 Web 界面回写到 Figma（⚠️ 仅在 Claude Code 中可用，使用 generate_figma_design 工具）

+   更适合云端协作和长时间运行的工作流

+   ⚠️ 注意：有速率限制（免费 6 次/月，付费 Dev seat 10 次/分钟）

---

## 典型使用场景和 Prompt

### 场景 1：获取设计并生成组件

**操作流程：**

1.  在 Figma 中选中组件或复制节点链接

1.  在 AI 工具中发起请求：

> “请使用已连接的 Figma MCP 服务器读取当前选中的 Frame，分析其布局、颜色、字体和组件结构，并为我的 React 项目生成一个可复用的组件代码。”

### 场景 2：配合 Code Connect 重用组件

> 开启 Code Connect 后，AI 会优先使用你现有的组件库：

> “请基于 Figma MCP 提供的设计上下文和 Code Connect 映射，生成使用我们现有组件库的实现代码，而不是重新写一个全新的组件。”

### 场景 3：利用 Skills（预设工作流）

> 在支持 Skills 的客户端中，可以直接调用：

+   Implement Design - 实现设计

+   Code Connect Components - 连接组件

+   Create Design System Rules - 创建设计系统规则

---

## 推荐实践流程

### 完整配置顺序

1.  **启用 Desktop MCP**

+   在 Figma Desktop 中开启 MCP server

+   获得本地地址：http://127.0.0.1:3845/mcp

1.  **配置 Claude Code**

```
claude mcp add --transport http figma-desktop http://127.0.0.1:3845/mcp
```

1.  **配置 Codex**

+   添加 Remote MCP:https://mcp.figma.com/mcp

+   可选添加 Desktop MCP

+   启用 Figma 相关 Skills

1.  **开始使用**

+   通过 selection-based 或 link-based 方式

+   把 Figma 设计上下文交给 AI

+   生成与现有代码库对齐的实现代码