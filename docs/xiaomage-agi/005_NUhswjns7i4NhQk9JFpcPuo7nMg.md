
# 如何将 FizzlyCode 的 AI 驱动型 API 集成到您的应用程序中

> 来源：[https://xmgtoagi.feishu.cn/docx/JwG1d8uiQoyI1Sxcl0rcb7rEnbd](https://xmgtoagi.feishu.cn/docx/JwG1d8uiQoyI1Sxcl0rcb7rEnbd)

## 入门

FizzlyCode 提供强大的 API，让您能够访问最先进的 AI 模型，用于代码生成和辅助开发。请按照以下步骤快速入门：

### 1创建您的帐户

注册一个 FizzlyCode 账号即可开始使用。您将自动收到您的第一个 API 密钥。

[创建账户](https://fizzlycode.com/auth/)

### 2获取您的 API 密钥

前往您的控制面板并复制您的 API 密钥。请妥善保管，切勿公开分享。

```
sk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

### 3提出您的第一个请求

使用您的 API 密钥向我们的 API 端点发出您的第一个请求。

## 验证

所有 API 请求都必须在 `Authorization` 标头中包含您的 API 密钥。您的 API 密钥应以“`Bearer`”为前缀。

```
Authorization: Bearer sk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

**安全提示：**切勿在客户端代码或公共代码库中暴露您的 API 密钥。请始终从后端服务器发起 API 调用。

## 克劳德代码集成

本指南将指导您如何安装和配置 Claude Code 以与 FizzlyCode 配合使用。Claude Code 是一款人工智能驱动的代码助手，可帮助您直接在终端中编写、调试和改进代码。

### 使用 CC 开关快速设置

自动配置Claude Code

零配置

无需编辑配置文件

平台智能

自动适应您的操作系统

默认安全

您的API密钥将得到安全处理。

### 手动设置一键设置

从未配置过命令行工具？没问题。

### 系统要求

支持的平台：

+   ✓Windows 10/11（`PowerShell`、`CMD`、`Git Bash`）

+   ✓macOS 10.15+（终端、`iTerm2`）

+   ✓Linux（Ubuntu、Debian、RHEL 等）

要求：

+   •`Node.js 18+`（可从 `nodejs.org` 下载）

+   •`npm`（`Node.js` 自带）

+   •您的 FizzlyCode API 密钥

### 1安装 Node.js

从官方网站下载并安装 `Node.js`。为了保证稳定性，请选择 LTS 版本。

[下载 Node.js](https://nodejs.org/)

• Windows 系统：下载 `.msi` 安装程序并运行它

• macOS：下载 `.pkg` 安装程序或使用 Homebrew：`brew install node`

• Linux：使用软件包管理器或从 `nodejs.org` 下载。

### 2安装 Claude Code

`Node.js` 安装完成后，打开终端并运行以下命令：

```
npm install -g @anthropic-ai/claude-code
```

• Windows：使用 `PowerShell`、`CMD` 或 `Git Bash`

• macOS/Linux：使用终端

• 验证安装：运行 `claude --version`

### 配置

#### 3配置克劳德代码

创建配置文件（适用于所有操作系统）：

创建文件：`~/.claude/settings.json`

```
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your-api-key-here",
    "ANTHROPIC_BASE_URL": "https://fizzlycode.com/api",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1
  },
  "permissions": {
    "allow": [],
    "deny": []
  }
}
```

• 替换 `your-api-key-here` 为您的实际 FizzlyCode API 密钥

• 此方法在 Windows、macOS 和 Linux 系统上均适用

另一种选择：环境变量（点击展开）

#### 4验证安装

测试 Claude Code 是否已正确安装和配置：

```
# Check Claude Code version
claude --version

# Verify configuration
claude doctor

# Start using Claude Code
claude “Hello, can you help me write a Python function?”
```

✓ 如果一切设置正确，Claude Code 将回复您的查询！

#### 故障排除

##### “命令未找到”错误

请确保已正确安装 `Node.js` 和 `npm`。运行 `npm --version` 进行检查。如果缺少 `Node.js`，请从 `nodejs.org` 重新安装。

##### API 连接错误

请检查您的 API 密钥是否正确 `~/.claude/settings.json`。密钥应以您的套餐前缀开头（例如，免费套餐的前缀为 `cr_`）。

##### 未找到设置文件

先创建目录，`mkdir -p ~/.claude` 然后创建 `settings.json` 文件。

##### 安装过程中权限被拒绝

在 macOS/Linux 系统中，切勿使用 `sudo` 命令运行 `npm`。如果出现权限错误，请重新配置 `npm` 以使用用户目录。

#### 专业提示

运行 `claude doctor` 以自动诊断常见问题和配置问题。

#### 专业提示

`Claude Code` 会自动更新到最新版本。运行命令 `claude doctor` 检查您的安装状态和可用更新。

## Gemini CLI 集成

配置 Gemini CLI 使用 FizzlyCode 来增强 Google Gemini 模型的 AI 功能。

### 环境变量配置

设置以下环境变量以配置 Gemini CLI 使用 FizzlyCode。注意：请使用与其他工具相同的 API 密钥格式 (`cr_xxx`)。

```
# Gemini CLI Configuration

export CODE_ASSIST_ENDPOINT="https://fizzlycode.com/gemini"

export GOOGLE_CLOUD_ACCESS_TOKEN="cr_your_api_key_here"

export GOOGLE_GENAI_USE_GCA="true"

# Add to your shell profile (.bashrc, .zshrc, etc.)

echo 'export CODE_ASSIST_ENDPOINT="https://fizzlycode.com/gemini"' >> ~/.bashrc

echo 'export GOOGLE_CLOUD_ACCESS_TOKEN="cr_your_api_key_here"' >> ~/.bashrc

echo 'export GOOGLE_GENAI_USE_GCA="true"' >> ~/.bashrc

source ~/.bashrc

# For Windows PowerShell

env:GOOGLE_CLOUD_ACCESS_TOKEN = "cr_your_api_key_here"

$env:GOOGLE_GENAI_USE_GCA = "true"
```

🔧重要提示：端点 URL 必须完全一致 `https://fizzlycode.com/gemini`（不能是 `/api/gemini`）。请使用与其他工具相同的 API 密钥 (`cr_xxx`)。

## Codex 集成（CLI 和 VSCode）

本文提供使用 FizzlyCode 配置 Codex CLI 和 VSCode 扩展的完整指南。Codex 是一款强大的 AI 代码助手，可将高级推理功能直接引入您的开发环境。

### 使用 CC 开关快速设置

自动配置Codex

零配置

无需编辑配置文件

平台智能

自动适应您的操作系统

默认安全

您的API密钥将得到安全处理。

### 手动设置一键设置

从未配置过命令行工具？没问题。

### 重要提示

FizzlyCode 提供完整的 Codex 支持，包括最新的 `gpt-5-codex` 模型及其增强的推理能力。您的 FizzlyCode API 密钥可与 Codex 无缝协作。

### 快速入门

#### 三步轻松上手

1. 从您的 FizzlyCode 控制面板获取您的 API 密钥。您的 API 密钥以 `cr_` 开头。

2. 安装 Codex CLI：`npm install -g @openai/codex`

3. 使用您的 FizzlyCode API 密钥进行配置（请参阅以下详细步骤）

### 第一部分：Codex CLI 配置

#### 系统要求

环境：

+   ✓`Node.js 22+`（必需）

+   ✓`npm 10+`（必需）

+   ✓网络连接

支持的平台：

+   •Windows 10/11

+   •macOS 11.0 及更高版本

+   •Linux（Ubuntu 20.04+）

#### 1安装 Codex CLI

使用 `npm` 全局安装 Codex：

```
# Install Codex CLI
npm install -g @openai/codex

# For macOS users with Homebrew
brew install codex

# Verify installation
codex --version

# Should show: codex-cli 0.31.0 or higher

# If you get “command not found”, add npm global bin to PATH

export PATH=$(npm config get prefix)/bin:$PATH
```

**注意：**如果遇到权限问题，请配置 `npm` 使用用户级安装目录而不是 `sudo`。

#### 2配置 config.toml

创建配置目录和文件：

**文件位置：**

+   视窗：`C:\Users\[username]\.codex\config.toml`

+   macOS/Linux：`~/.codex/config.toml`

首先创建目录：`mkdir -p ~/.codex`

然后创建 `config.toml` 文件，内容如下：

```
model_provider = "fizzlycode"

model = "gpt-5" # Can be changed to "gpt-5-codex" for enhanced reasoning

model_reasoning_effort = "high"

disable_response_storage = true

[model_providers.fizzlycode]

name = "fizzlycode"

base_url = "https://fizzlycode.com/openai"

wire_api = "responses"

requires_openai_auth = true # IMPORTANT: Add this for model switching support
```

• “模型”`"gpt-5"`代表标准模型，`"gpt-5-codex"`代表增强推理模型

• `model_reasoning_effort`：对于复杂任务，设置为“高”；对于较简单的查询，设置为“中”或“低”。

• `requires_openai_auth`：必须设置为 `true` 才能使模型切换正常工作

• `base_url`：FizzlyCode 的 OpenAI 兼容端点

#### 3配置身份验证

使用您的 FizzlyCode API 密钥创建身份验证文件：

**文件位置：** `~/.codex/auth.json`

创建 `auth.json` 文件，内容如下：

```
{
  "OPENAI_API_KEY": "cr_your_fizzlycode_api_key_here"
}
```

**重要提示：**请将此处的密钥替换 `cr_your_fizzlycode_api_key_here` 为您在控制面板中实际使用的 API 密钥。

文件创建完成后，设置安全权限：`chmod 600 ~/.codex/auth.json`

**安全提示：**切勿泄露您的 API 密钥。配置更改后，请重启终端。

#### 4测试配置

导航至您的项目目录并测试 Codex：

```
# Start Codex
codex

# Test with a simple request
codex “Write a hello world function in Python”

# Test with reasoning effort levels
codex --reasoning-effort high “Explain the time complexity of quicksort”
```

### 第二部分：VSCode 扩展配置

#### 安装官方 Codex 扩展

1. 在 VSCode 中打开扩展：`Ctrl+Shift+X`（Windows/Linux）或`Cmd+Shift+X`（macOS）

2. 搜索“Codex”或“ChatGPT”

3. **重要提示：**请仅安装带有 OpenAI 验证徽章的官方扩展程序。

4. 安装此扩展程序——它将出现在您的侧边栏中。

#### 在 VSCode 中启用模型切换

要在 VSCode 扩展中启用模型选择下拉菜单，您必须将以下代码添加到您的配置中：

```
# ~/.codex/config.toml

[model_providers.fizzlycode]

requires_openai_auth = true # Required to enable the model dropdown
```

**添加此行后：**该扩展程序会显示一个模型下拉菜单，您可以在其中选择 `gpt-5` 或 `gpt-5-codex` 并调整推理难度级别。

#### 启用 gpt-5-codex 模型

`gpt-5-codex` 模型提供了更强大的推理能力。以下是如何启用和使用它：

1. 更新您的 `config.toml` 文件以使用 `gpt-5-codex`：

```
model = "gpt-5-codex"  # Enhanced reasoning model

model_reasoning_effort = "high" # Use high for best performance
```

2. 重启 VSCode 以应用更改

3. 在扩展程序中，使用 `/model` 命令可以在不同模型之间切换。

**模型对比：**

+   • `gpt-5`：标准模型，响应速度快，非常适合大多数编码任务

+   • `gpt-5-codex`：推理能力更强，分析更深入，最适合解决复杂问题

### 常用斜杠命令

#### 内置命令

`/mode`切换审批模式（建议/自动编辑/全自动）

`/model`在 `gpt-5` 和 `gpt-5-codex` 模型之间切换

`/reasoning`调整推理难度（低/中/高）

`/approvals`切换审批要求

`/init`初始化 `AGENTS.md` 以启用自定义提示

`/status`显示当前配置和使用情况

`/diff`显示 Git 差异

`/clear`清晰的对话记录

### 常见问题排查

#### 错误 401：身份验证失败

#### 流错误：连接已断开。

#### VSCode 扩展找不到配置文件

#### VSCode 中不显示“模型”下拉菜单

#### gpt-5-codex 文件编辑问题

#### 如何优化以获得最佳性能？

### 第三方平台集成

FizzlyCode 的 Codex API 与 `new-api` 和 `cherry` 等平台兼容。请使用以下端点：`/v1/chat/completions`

型号名称：`gpt-5`或`gpt-5-codex`

## 费率限制和配额



# API 定价与速率限制指南

API 使用受速率限制和配额约束，具体取决于您的订阅计划。所有计划均包含每日消费限额，以帮助您控制成本。

## 永久免费

每月0美元

- 每分钟10 次请求
- 2 个API密钥
- 每日消费限额1美元
- 社区支持

非常适合体验这项服务

## 基本的

每月12美元

- 每分钟100 个请求
- 3 个API密钥
- 每日消费限额10美元
- 优先支持

非常适合需要可靠 API 访问的小型项目和初创公司。

## 法典

每月15美元

- 每分钟100 个请求
- 2 个API密钥
- 每日消费限额90美元
- 优先支持

非常适合仅使用 GPT 进行开发和模型实验。

## 加

每月39美元

- 每分钟100 个请求
- 2 个API密钥
- 每日消费限额30美元
- 优先支持

非常适合需要更高限额和高级分析功能的成长型团队。

## 专业的

每月70美元

- 每分钟100 个请求
- 2 个API密钥
- 每日消费限额80美元
- 优先支持

对开发者而言，这是无与伦比的价值。

## 企业

每月698美元

- 每分钟1000 次请求
- 20 个API密钥
- 每日消费限额500美元
- 全天候专属支持

大型团队无限量使用

### 速率限制标头

检查响应标头以了解当前使用情况：

```http
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1699564800
```

## 错误处理

API 返回标准 HTTP 状态代码和详细的错误消息，以帮助您调试问题。

### 200 OK

成功

请求成功

### 400 错误请求

客户端错误

无效的请求参数

### 401 未授权

身份验证错误

API密钥无效或缺失

### 429 请求过多

速率限制

速率限制已超出，冷却后重试

### 500 内部服务器错误

服务器错误

服务器错误，请重试或联系客服。

### 错误响应格式

```json
{
  "error": {
    "message": "Invalid API key provided",
    "type": "authentication_error",
    "code": "invalid_api_key"
  }
}
```

## 常见问题解答

### 我能否将 FizzlyCode 与现有的 OpenAI/Anthropic 代码一起使用？

是的！FizzlyCode 与 OpenAI 和 Anthropic API 格式兼容。只需将您的基本 URL 更改为我们的端点并使用您的 FizzlyCode API 密钥即可。

### 如何监控我的 API 使用情况？

查看您的仪表板以获取实时使用统计信息，或使用 `/usage` 端点以编程方式检索您当前的使用情况和剩余配额。

### 有哪些型号可供选择？

我们支持所有最新的 Claude 型号，包括 Claude 3.5 Sonnet (`claude-3-5-sonnet-20241022`)、Claude 3 Opus 和 Claude 3 Haiku。请查看 `/models` 端点以获取完整列表。

### 流媒体响应和非流媒体响应有什么区别？

流式传输会在响应生成后立即实时返回（SSE 格式），而非流式传输则会等待完整的响应。流式传输更适合面向用户的应用程序。

### 同一个账户可以使用多个API密钥吗？

是的，您可以为不同的应用程序或环境创建多个 API 密钥。这有助于跟踪使用情况和安全管理。

### 如何切换模型？

只需更改 API 请求中的 `model` 参数即可。使用 `claude-3-5-sonnet-20241022` 可获得最佳性能，使用 `claude-3-opus-20240229` 可处理复杂任务，使用 `claude-3-haiku-20240307` 可获得更快、更经济的响应。

### 是否有免费版本？

是的！我们的永久免费套餐包含每分钟 10 次请求，每日消费上限为 5 美元，非常适合试用服务或轻度个人使用。

### 如何处理速率限制错误？

实现带有重试逻辑的指数退避算法。检查 `X-RateLimit-Reset` 标头以了解您的速率限制何时重置。如果您经常达到速率限制，请考虑升级您的套餐。
