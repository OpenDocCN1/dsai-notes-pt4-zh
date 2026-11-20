
## 步骤 1：开通阿里云百炼服务

1. 登录阿里云百炼控制台：`https://bailian.console.aliyun.com/`
1. 若页面顶部显示需要开通，请点击开通阿里云百炼的模型服务
1. 首次开通后可领取新人免费额度（有效期：90天）
1. 超出额度或期限将产生费用，建议开启"免费额度用完即停"功能

## 步骤 2：获取 API Key

1. 进入阿里云百炼控制台
1. 在左侧菜单中找到"API-KEY管理"
1. 点击"创建新的API-KEY"按钮
1. 系统会生成一个 API Key
1. 重要：妥善保存这个密钥，它只会显示一次

## 安装 Claude Code

### macOS 安装

在终端中执行以下命令：

```
npm install -g @anthropic-ai/claude-code
```

### Windows 安装

在 Windows 上使用 Claude Code，需要先安装 WSL 或 Git for Windows，然后在 WSL 或 Git Bash 中执行：

```
npm install -g @anthropic-ai/claude-code
```

## 配置环境变量

要通过兼容 Anthropic API 的方式接入阿里云百炼的模型服务，需要配置以下两个环境变量：

+ `ANTHROPIC_BASE_URL`：设置为 `https://dashscope.aliyuncs.com/apps/anthropic`
+ `ANTHROPIC_API_KEY` 或 `ANTHROPIC_AUTH_TOKEN`：设置为阿里云百炼 API Key

### macOS 配置

在终端中执行以下命令，查看默认 Shell 类型：

```
echo $SHELL
```

Zsh 配置：

```
# 用百炼 API Key 替换 YOUR_DASHSCOPE_API_KEY
echo 'export ANTHROPIC_BASE_URL="https://dashscope.aliyuncs.com/apps/anthropic"' >> ~/.zshrc
echo 'export ANTHROPIC_API_KEY="YOUR_DASHSCOPE_API_KEY"' >> ~/.zshrc
source ~/.zshrc
```

Bash 配置：

```
# 用百炼 API Key 替换 YOUR_DASHSCOPE_API_KEY
echo 'export ANTHROPIC_BASE_URL="https://dashscope.aliyuncs.com/apps/anthropic"' >> ~/.bash_profile
echo 'export ANTHROPIC_API_KEY="YOUR_DASHSCOPE_API_KEY"' >> ~/.bash_profile
source ~/.bash_profile
```

打开一个新的终端，执行以下命令验证环境变量：

```
echo $ANTHROPIC_BASE_URL
echo $ANTHROPIC_API_KEY
```

### Windows 配置

CMD 配置：

```
REM 用百炼 API Key 替换 YOUR_DASHSCOPE_API_KEY
setx ANTHROPIC_API_KEY "YOUR_DASHSCOPE_API_KEY"
setx ANTHROPIC_BASE_URL "https://dashscope.aliyuncs.com/apps/anthropic"
```

打开新的 CMD 窗口，运行以下命令检查：

```
echo %ANTHROPIC_API_KEY%
echo %ANTHROPIC_BASE_URL%
```

PowerShell 配置：

```
# 用百炼 API Key 替换 YOUR_DASHSCOPE_API_KEY
[Environment]::SetEnvironmentVariable("ANTHROPIC_API_KEY", "YOUR_DASHSCOPE_API_KEY", [EnvironmentVariableTarget]::User)
[Environment]::SetEnvironmentVariable("ANTHROPIC_BASE_URL", "https://dashscope.aliyuncs.com/apps/anthropic", [EnvironmentVariableTarget]::User)
```

打开新的 PowerShell 窗口，运行以下命令检查：

```
echo $env:ANTHROPIC_API_KEY
echo $env:ANTHROPIC_BASE_URL
```

## 支持的模型

百炼提供的 Anthropic API 兼容服务支持以下通义千问系列模型：

### 通义千问 Max（仅 qwen3-max-preview 支持思考模式）

+ qwen3-max
+ qwen3-max-2025-09-23
+ qwen3-max-preview

### 通义千问 Plus

+ qwen-plus
+ qwen-plus-latest
+ qwen-plus-2025-09-11

### 通义千问 Flash

+ qwen-flash
+ qwen-flash-2025-07-28

### 通义千问 Turbo

+ qwen-turbo
+ qwen-turbo-latest

### 通义千问 Coder（不支持思考模式）

+ qwen3-coder-plus
+ qwen3-coder-plus-2025-09-23
+ qwen3-coder-flash

### 通义千问 VL（不支持思考模式）

+ qwen-vl-max
+ qwen-vl-plus

## 测试连接

### 步骤 1：启动 Claude Code

在终端中执行：

```
claude-code
```

### 步骤 2：配置模型

在 Claude Code 对话期间，执行 `/mode` 命令，然后选择或输入你想使用的通义千问模型名称，例如：

```
/mode qwen-plus
```

### 步骤 3：发送测试消息

输入一条简单的测试消息：

```
你好，请用一句话介绍一下你自己。
```

### 步骤 4：验证结果

如果看到通义千问的回复，说明配置成功！

+ 回复应该流畅自然
+ 响应时间应该在几秒内
+ 没有错误提示

## 模型版本说明

### 通义千问 Max 系列

+ 最强大的版本
+ qwen3-max-preview 支持思考模式
+ 适合复杂任务和专业场景
+ 价格较高

### 通义千问 Plus 系列

+ 能力较强
+ 理解深入
+ 适合大多数复杂任务
+ 价格适中

### 通义千问 Flash 系列

+ 响应速度快
+ 成本较低
+ 适合日常对话和简单任务
+ 性价比高

### 通义千问 Turbo 系列

+ 响应最快
+ 成本最低
+ 适合快速对话和简单任务
+ 适合新手使用

### 通义千问 Coder 系列

+ 专为编程任务优化
+ 代码生成和理解能力强
