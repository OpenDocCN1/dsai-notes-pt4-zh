
# 第二章：接入国内大模型

> 来源：https://qqs7y1hozd1.feishu.cn/docx/PC7qdQyCFoe5eYxGHQEcok4knAf

## 2.1 为什么要选择国内大模型

使用国内大模型有很多优势，特别适合中国用户。本文将从多个维度详细分析为什么选择国内模型。

### 一、访问速度优势

国内模型服务器部署在中国境内，访问速度更快：

+ 响应时间更短，通常在1-2秒内就能得到回复
+ 网络延迟更低，使用体验更流畅
+ 不受国际网络波动影响，稳定性更好

### 二、成本考虑

国内模型通常价格更实惠：

+ `API` 调用费用更低，比海外模型普遍低30%-50%
+ 提供免费额度或试用机会，适合个人学习和小型项目
+ 适合频繁使用的场景，长期使用成本优势明显
+ 支付方式更方便，支持支付宝、🛰️支付等国内常用支付方式

### 三、中文处理能力

国内模型对中文的理解和处理更加精准：

+ 更好地理解中文语境和文化内涵
+ 处理中文成语、俗语、歇后语等特殊表达更准确
+ 生成中文内容更自然流畅，符合中文表达习惯
+ 支持更多中文方言和表达方式，覆盖更广泛的用户群体

### 四、数据安全与合规

使用国内模型更符合国内法规要求：

+ 数据存储在国内，符合《中华人民共和国数据安全法》
+ 隐私保护更符合国内标准，严格遵守《个人信息保护法》
+ 不受跨境数据传输限制，避免了数据跨境带来的安全风险
+ 更容易满足企业内部的数据安全审计要求

### 五、本地化服务支持

国内模型提供更好的本地化服务：

+ 客服支持中文沟通，响应速度更快
+ 提供完善的中文技术文档和教程，便于理解和使用
+ 定期举办线上线下技术交流活动，促进用户学习和分享
+ 与国内主流开发工具、框架和平台深度集成

### 六、支持Claude Code的国内模型厂商

目前，已有多家国内AI厂商提供了支持 `Claude Code` 的服务：

#### 智谱AI（GLM）

+ 代表模型：`GLM-4`、`GLM-3`
+ 优势：中文理解能力强，代码生成质量高
+ 适用场景：代码开发、文档生成、智能问答

#### 通义千问（Qwen）

+ 代表模型：`Qwen Plus`、`Qwen Max`
+ 优势：多模态能力强，支持图像和代码
+ 适用场景：代码补全、代码解释、系统设计

#### DeepSeek

+ 代表模型：`DeepSeek-R1`、`DeepSeek-Coder`
+ 优势：专注于代码领域，代码生成准确率高
+ 适用场景：代码生成、代码修复、代码优化

#### 豆包

+ 代表模型：豆包4.0、豆包Code
+ 优势：用户界面友好，集成度高
+ 适用场景：代码学习、辅助开发、代码评审

#### Kimi

+ 代表模型：`Kimi-Coder`、`Kimi-Max`
+ 优势：上下文窗口大，支持长文本处理
+ 适用场景：大型项目开发、文档理解、代码重构

### 七、最佳实践

#### 选择合适的模型

根据不同的使用场景选择合适的模型：

+ 代码生成：优先选择 `DeepSeek-Coder`、`Kimi-Coder` 等专注于代码的模型
+ 文档生成：推荐使用 `GLM-4`、`Qwen Plus` 等综合性模型
+ 长文本处理：选择 `Kimi-Max` 等大上下文窗口模型

#### 优化API使用

+ 合理设置 `maxTokens` 值，避免不必要的消耗
+ 使用流式输出，提高响应速度
+ 缓存常用请求的结果，减少重复调用

#### 安全使用API密钥

+ 不要将 `API Key` 硬编码到代码中
+ 定期更换 `API Key`
+ 限制 `API Key` 的使用范围和权限
+ 使用环境变量或配置文件管理 `API Key`

### 八、总结

选择国内大模型具有诸多优势，包括更快的访问速度、更低的成本、更好的中文处理能力、更强的数据安全性以及更完善的本地化服务。

随着国内AI技术的不断发展，国内模型在性能和功能上已经逐渐赶上甚至超越海外模型，成为越来越多开发者的首选。建议开发者根据自己的需求和使用场景，选择适合的国内模型，充分利用AI技术提升开发效率和质量。

## 2.2 配置 GLM

`GLM`（智谱AI）是国内领先的大语言模型之一，由智谱AI开发，对中文理解能力出色，支持多种应用场景。

以下是配置 `GLM` 到 `Claude Code` 的详细步骤。

### 注册与获取 API

#### 步骤 1：注册智谱AI账户

1. 访问智谱AI开放平台：
2. 点击页面左上方的"注册"按钮
3. 输入手机号并进行验证，完成注册
4. 完成实名认证（实名认证后可获得更多免费额度）

#### 步骤 2：获取免费额度

1. 注册后自动获得 100 万 `tokens` 免费额度
2. 完成实名认证后额外获得 400 万 `tokens`（有效期一个月）
3. 新用户总共可获得 500 万 `tokens` 的免费额度

> `GLM` 推出了编码套餐，可以 `https://www.bigmodel.cn/glm-coding?ic=5AH7ATEZSC`，配合 `Claude Code`，完成你的工作需求。

#### 步骤 3：获取 API Key

1. 登录智谱AI开放平台
2. 点击右上角的"查看API Key"或进入"API管理"
3. 点击"添加新的Key"或"创建新Key"
4. 填写Key名称（可选），点击确认
5. 系统会生成 `API Key`
6. 重要：妥善保存 `API Key`，建议复制到安全的地方

### 配置 Claude Code

配置 `GLM` 到 `Claude Code` 有三种方式，选择适合你的一种即可。

#### 方式一：使用自动化助手（推荐）

##### 步骤 1：运行自动化助手

```
npx @z_ai/coding-helper
```

##### 步骤 2：按照提示操作

自动化助手会引导你完成：

+ 工具安装
+ 套餐配置
+ `MCP` 服务器管理

这是最简单快捷的方式，适合大多数用户。

#### 方式二：使用自动化脚本（仅限 macOS 和 Linux）

如果你使用 `macOS` 或 `Linux` 系统，可以使用自动化脚本快速配置。

##### 步骤 1：下载并运行脚本

在终端中执行以下命令：

```
curl -O "https://cdn.bigmodel.cn/install/claude_code_env.sh" && bash ./claude_code_env.sh
```

##### 步骤 2：脚本自动配置

脚本会自动修改 `~/.claude/settings.json` 文件，配置以下环境变量：

```
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "your_zhipu_api_key",
        "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic",
        "API_TIMEOUT_MS": "3000000",
        "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1
    }
}
```

注意：请将 `your_zhipu_api_key` 替换为你实际获取的 `API Key`。

#### 方式三：手动配置

如果你需要手动配置，请根据你的操作系统选择以下方式之一。

##### macOS 和 Linux

1. 创建或编辑配置文件 `~/.claude/settings.json`
2. 添加以下内容（替换 `your_zhipu_api_key` 为你的 `API Key`）：

```
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "your_zhipu_api_key",
        "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic",
        "API_TIMEOUT_MS": "3000000",
        "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1
    }
}
```

##### Windows（Cmd）

在 `Cmd` 中运行以下命令（替换 `your_zhipu_api_key` 为你的 `API Key`）：

```
setx ANTHROPIC_AUTH_TOKEN your_zhipu_api_key
setx ANTHROPIC_BASE_URL https://open.bigmodel.cn/api/anthropic
setx CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC 1
```

##### Windows（PowerShell）

在 `PowerShell` 中运行以下命令（替换 `your_zhipu_api_key` 为你的 `API Key`）：

```
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_AUTH_TOKEN', 'your_zhipu_api_key', 'User')
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_BASE_URL', 'https://open.bigmodel.cn/api/anthropic', 'User')
[System.Environment]::SetEnvironmentVariable('CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC', '1', 'User')
```

重要提示：配置完成后，请关闭所有终端窗口，打开一个新的终端窗口以使配置生效。

#### 测试连接

##### 步骤 1：启动 Claude Code

```
claude
```

##### 步骤 2：信任文件夹

首次启动时，`Claude Code` 会询问是否信任当前文件夹，选择 `Yes`。

##### 步骤 3：发送测试消息

在 `Claude Code` 的聊天界面中，输入一条简单的测试消息，例如：

```
你好，请用一句话介绍一下你自己。
```

##### 步骤 4：验证结果

如果看到 `GLM` 的回复，说明配置成功！

+ 回复应该流畅自然
+ 响应时间应该在几秒内
+ 没有错误提示

##### 步骤 5：检查模型状态（可选）

在 `Claude Code` 中输入 `/status` 命令，可以查看当前使用的模型状态和配置信息。

### GLM 模型

`GLM Coding Plan` 默认使用智谱 AI 的最新模型，通过环境变量映射到 `Claude Code` 的模型层级。

#### 默认模型映射

配置成功后，`GLM` 模型会自动映射到 `Claude Code` 的模型层级：

+ `Claude Opus` → `GLM-4.7`（最强大的模型）
+ `Claude Sonnet` → `GLM-4.7`（平衡性能与速度）
+ `Claude Haiku` → `GLM-4.5-Air`（快速响应模型）

提示：默认情况下，你在 `Claude Code` 界面上看到的是 `Claude` 模型名称，但实际使用的是 `GLM` 模型。这是服务端的模型映射，无需手动配置。

#### GLM-4.7

+ 特点：智谱最新的旗舰模型，综合能力最强
+ 适用场景：复杂任务、深度代码生成、需要高质量输出的场景
+ 性能：在代码生成、逻辑推理、问题解决等方面表现优异

#### GLM-4.5-Air

+ 特点：轻量级模型，响应速度快
+ 适用场景：快速对话、简单任务、需要实时响应的场景
+ 性能：响应速度快，适合大量使用

#### 手动切换模型（高级用户）

如果你需要手动切换模型，可以编辑 `~/.claude/settings.json` 文件，添加以下配置：

```
{
  "env": {
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "glm-4.5-air",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "glm-4.7",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "glm-4.7"
  }
}
```

注意：一般不建议手动调整模型映射，因为硬编码后当 `GLM Coding Plan` 的模型更新升级时，不方便自动更新到最新模型。

### 常见问题

Q：`API Key` 泄露了怎么办？

A：立即在智谱AI开放平台删除旧密钥，创建新的密钥，并更新你的配置。

Q：配置后提示"认证失败"怎么办？

A：检查以下几点：

+ `API Key` 是否正确，注意不要有多余的空格
+ `Base URL` 是否填写为 `https://open.bigmodel.cn/api/anthropic`
+ 配置后是否重新打开了新的终端窗口
+ 配置文件的 `JSON` 格式是否正确

Q：免费额度用完了怎么办？

A：可以在智谱AI开放平台购买付费套餐，价格比较实惠。`GLM Coding Plan` 还提供以更低价格获得 3 倍用量的套餐。

Q：如何确认配置是否生效？

A：启动 `Claude Code` 后，输入 `/status` 命令查看模型状态和配置信息。

Q：配置后没有生效怎么办？

A：尝试以下排查步骤：

1. 关闭所有 `Claude Code` 窗口
2. 重新打开一个新的命令行窗口
3. 再次运行 `claude` 启动
4. 如果问题仍然存在，可以删除 `~/.claude/settings.json` 文件，然后重新配置
5. 确认配置文件的 `JSON` 格式是否正确，可以使用在线 `JSON` 校验工具进行检查

Q：`GLM` 的中文能力如何？

A：`GLM` 是国内领先的大模型之一，中文理解能力非常出色，适合中文用户使用。

Q：可以在多个项目中使用同一个 `API Key` 吗？

A：可以，一个 `API Key` 可以在多个项目中使用，但建议为不同项目创建不同的 `Key` 以便于管理。

Q：推荐使用哪个版本的 `Claude Code`？

A：建议使用最新版本的 `Claude Code`。你可以通过以下命令检查当前版本和升级：

```
# 检查当前版本
claude --version

# 升级到最新版本
claude update
```

我们在 `Claude Code 2.0.14` 等版本验证通过，建议使用此版本或更新版本。

Q：如何切换使用不同的 `GLM` 模型？

A：默认情况下，`GLM` 模型会自动映射到 `Claude Code` 的模型层级。如果需要手动切换，可以编辑 `~/.claude/settings.json` 文件，修改 `ANTHROPIC_DEFAULT_HAIKU_MODEL`、`ANTHROPIC_DEFAULT_SONNET_MODEL`、`ANTHROPIC_DEFAULT_OPUS_MODEL` 环境变量。但一般不建议手动调整，以便自动更新到最新模型。

Q：配置文件修改后不生效怎么办？

A：确保：

1. `JSON` 格式正确，检查变量名称、逗号等
2. 关闭所有 `Claude Code` 窗口，重新打开新的终端
3. 如果问题持续，可以删除配置文件重新配置

## 2.3 配置通义千问

`通义千问` 是阿里巴巴推出的中文大模型，在中文理解和生成方面表现优秀。阿里云百炼提供了 `Anthropic API` 兼容服务，可以方便地在 `Claude Code` 中接入 `通义千问` 系列模型。

### 前提条件



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



# 2.3 配置通义千问

+   不支持思考模式
+   适合开发场景

## 通义千问 VL 系列[#](https://claudecode.tangshuang.net/easy-tutorial/2.3%20%E9%85%8D%E7%BD%AE%E9%80%9A%E4%B9%89%E5%8D%83%E9%97%AE#%E9%80%9A%E4%B9%89%E5%8D%83%E9%97%AE%20VL%20%E7%B3%BB%E5%88%97)

+   支持视觉理解
+   可以处理图像和文本
+   不支持思考模式
+   适合多模态任务

## 重要提示[#](https://claudecode.tangshuang.net/easy-tutorial/2.3%20%E9%85%8D%E7%BD%AE%E9%80%9A%E4%B9%89%E5%8D%83%E9%97%AE#%E9%87%8D%E8%A6%81%E6%8F%90%E7%A4%BA)

1.  旧版接口已弃用：旧版兼容接口 `https://dashscope.aliyuncs.com/api/v2/apps/claude-code-proxy` 仅支持调用 `qwen3-coder-plus` 模型，建议迁移至新版接口。

1.  免费额度限制：Claude Code 无法使用 Qwen Code 每日 2000 次免费额度，使用百炼的 API 兼容服务会按照百炼的计费规则计费。

1.  地域限制：本文档仅适用于中国内地版（北京地域）。

1.  思考模式：只有 `qwen3-max-preview` 支持思考模式，其他模型不支持。

## 常见问题[#](https://claudecode.tangshuang.net/easy-tutorial/2.3%20%E9%85%8D%E7%BD%AE%E9%80%9A%E4%B9%89%E5%8D%83%E9%97%AE#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)

Q：API Key 泄露了怎么办？

A：立即在阿里云百炼控制台删除旧密钥，创建新的密钥，并更新环境变量。

Q：为什么提示"鉴权失败"？

A：检查 API Key 是否正确，注意不要有多余的空格。同时确认 `ANTHROPIC_BASE_URL` 设置正确。

Q：如何选择合适的模型版本？

A：新手建议使用 `qwen-turbo` 或 `qwen-flash`，如果发现能力不足再升级到 `qwen-plus` 或 `qwen3-max`。编程任务推荐使用 `qwen3-coder-plus`。

Q：通义千问和文心一言哪个更好？

A：两个模型都很优秀，建议都配置好，根据具体任务选择使用。通义千问在编程和多模态方面有优势。

Q：如何查看模型参数及计费规则？

A：请访问阿里云百炼控制台的模型列表页面查看详细信息。

Q：环境变量配置后不生效怎么办？

A：确保已执行 `source` 命令使环境变量生效，并打开新的终端窗口进行测试。

# 2.4 使用 AICodeSwitch 工具

AICodeSwitch 是一个便捷的工具，可以让你快速配置Claude Code或Codex这类本地编程工具，并且可以在不同的大模型之间快速切换，无需每次都去设置中修改配置。

## 什么是 AICodeSwitch

AICodeSwitch 是一个模型路由工具，它的主要功能包括：

+   集中管理多个 AI 模型的配置
+   内置模型适配器，可以把`GPT`类的模型也作为`Claude Code`的驱动模型
+   一键切换不同的路由
+   根据任务类型，在路由规则中自动选择合适的模型
+   丰富的日志信息，显示各模型的使用统计信息

简单来说，它就像一个"模型切换器"，让你在使用 Claude Code 时可以随时GLM、GPT、Qwen等模型之间切换。

## 安装与配置

### 步骤 1：安装 AICodeSwitch

AICodeSwitch 通常以 `npm` 包的形式提供。

1.  打开终端（命令行）

1.  输入以下命令安装：

```
npm install -g aicodeswitch
```

1.  如果是MacOS或Linux用户，可能需要使用 `sudo` 命令：`sudo npm install -g aicodeswitch`。

1.  等待安装完成

### 步骤 2：配置模型信息

1.  在终端中输入：

```
aicos ui
```

1.  这会打开一个图形化界面，帮助你配置模型信息。`aicos` 命令是 `aicodeswitch` 命令的别名。

1.  配置供应商和路由

1.  激活路由

具体可以参考这个视频：<https://www.bilibili.com/video/BV1a865B8ErA/>

<https://player.bilibili.com/player.html?bvid=1a865B8ErA&t=0.0>

你不需要自己手动对 Claude Code 或 Codex 进行配置修改，AICodeSwitch 会自动修改它们的配置文件，并接管它们的模型请求。

## 随时随地切换大模型[#](https://claudecode.tangshuang.net/easy-tutorial/2.4%20%E4%BD%BF%E7%94%A8%20AICodeSwitch%20%E5%B7%A5%E5%85%B7#%E9%9A%8F%E6%97%B6%E9%9A%8F%E5%9C%B0%E5%88%87%E6%8D%A2%E5%A4%A7%E6%A8%A1%E5%9E%8B)

你可以在 AICodeSwitch 的图形化界面中，随时切换不同的模型。

你只需要在路由列表中，点击“激活”对应的路由。

路由规则中，你可以根据实际情况，配置不同的大模型，激活路由后，就切换到了规则里面配置的大模型。

## 常见问题[#](https://claudecode.tangshuang.net/easy-tutorial/2.4%20%E4%BD%BF%E7%94%A8%20AICodeSwitch%20%E5%B7%A5%E5%85%B7#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)

Q：AICodeSwitch 是免费的吗？

A：AICodeSwitch 工具本身是免费的，但使用各个模型仍需支付相应的 API 费用。

Q：可以同时配置多个国内模型吗？

A：可以，你可以配置GLM、通义千问等多个国内模型，随时切换使用。

Q：切换模型会影响对话历史吗？

A：不会，对话历史会保留，但不同模型可能对上下文的理解略有差异。

# 2.5 使用免费的大模型

在本章节中，我们将介绍如何将免费的大模型接入到Claude Code中，主要通过以下几种方式：使用免费大模型提供商的服务、本地搭建免费大模型，以及通过ClaudeCodeRouter实现接入。

## 一、寻找免费大模型提供商[#](https://claudecode.tangshuang.net/easy-tutorial/2.5%20%E4%BD%BF%E7%94%A8%E5%85%8D%E8%B4%B9%E7%9A%84%E5%A4%A7%E6%A8%A1%E5%9E%8B#%E4%B8%80%E3%80%81%E5%AF%BB%E6%89%BE%E5%85%8D%E8%B4%B9%E5%A4%A7%E6%A8%A1%E5%9E%8B%E6%8F%90%E4%BE%9B%E5%95%86)

### 1.1 OpenRouter[#](https://claudecode.tangshuang.net/easy-tutorial/2.5%20%E4%BD%BF%E7%94%A8%E5%85%8D%E8%B4%B9%E7%9A%84%E5%A4%A7%E6%A8%A1%E5%9E%8B#1.1%20OpenRouter)

`OpenRouter`是一个聚合了多种大模型的平台，其中包含一些免费或提供免费额度的模型。

#### 步骤 1：注册`OpenRouter`账户

1.  访问`OpenRouter`官网：`https://openrouter.ai/`

1.  点击右上角的`Sign Up`按钮

1.  使用邮箱或`GitHub`账户完成注册

1.  登录后进入控制台

#### 步骤 2：获取免费模型

1.  在控制台中点击`Models`标签

1.  筛选`Free`模型，查看可用的免费模型

1.  选择适合的模型，例如：

+   `Llama 3 8B Instruct`
+   `Mistral 7B Instruct`
+   `Gemma 7B Instruct`

#### 步骤 3：获取API密钥

1.  在控制台中点击`API Keys`标签

1.  点击`Create API Key`按钮

1.  为密钥命名，选择访问权限

1.  复制生成的API密钥，妥善保存

### 1.2 阿里云通义千问[#](https://claudecode.tangshuang.net/easy-tutorial/2.5%20%E4%BD%BF%E7%94%A8%E5%85%8D%E8%B4%B9%E7%9A%84%E5%A4%A7%E6%A8%A1%E5%9E%8B#1.2%20%E9%98%BF%E9%87%8C%E4%BA%91%E9%80%9A%E4%B9%89%E5%8D%83%E9%97%AE)

阿里云提供的通义千问模型也提供一定的免费额度。

#### 步骤 1：注册阿里云账户

1.  访问阿里云官网：`https://www.aliyun.com/`

1.  点击右上角的`免费注册`按钮

1.  使用手机号完成注册和实名认证

#### 步骤 2：开通通义千问服务

1.  搜索`通义千问`，进入产品页面

1.  点击`立即开通`，按照提示完成开通

1.  进入通义千问控制台

#### 步骤 3：寻找免费模型

1.  在官网文档中，寻找可免费使用的大模型列表

1.  将它们保存起来

#### 步骤 4：获取API密钥

1.  在控制台中进入`API密钥管理`页面

1.  点击`创建AccessKey`按钮

1.  复制生成的`AccessKey ID`和`AccessKey Secret`，妥善保存

## 二、本地搭建Ollama运行免费大模型

`Ollama`是一个可以在本地运行大模型的工具，支持多种开源免费模型。

### 2.1 安装Ollama[#](https://claudecode.tangshuang.net/easy-tutorial/2.5%20%E4%BD%BF%E7%94%A8%E5%85%8D%E8%B4%B9%E7%9A%84%E5%A4%A7%E6%A8%A1%E5%9E%8B#2.1%20%E5%AE%89%E8%A3%85Ollama)

#### macOS

1.  访问`Ollama`官网：`https://ollama.com/`

1.  点击`Download for macOS`按钮

1.  打开下载的DMG文件，将`Ollama`拖拽到`Applications`文件夹

1.  打开终端，运行`ollama --version`验证安装成功

#### Windows

1.  访问`Ollama`官网：`https://ollama.com/`

1.  点击`Download for Windows`按钮

1.  运行下载的安装程序，按照提示完成安装

1.  打开命令提示符或`PowerShell`，运行`ollama --version`验证安装成功

#### Linux

1.  打开终端，运行以下命令：

```
curl -fsSL https://ollama.com/install.sh | sh
```

1.  安装完成后，运行`ollama --version`验证安装成功

### 2.2 下载并运行免费大模型[#](https://claudecode.tangshuang.net/easy-tutorial/2.5%20%E4%BD%BF%E7%94%A8%E5%85%8D%E8%B4%B9%E7%9A%84%E5%A4%A7%E6%A8%A1%E5%9E%8B#2.2%20%E4%B8%8B%E8%BD%BD%E5%B9%B6%E8%BF%90%E8%A1%8C%E5%85%8D%E8%B4%B9%E5%A4%A7%E6%A8%A1%E5%9E%8B)

1.  打开终端，运行以下命令下载并运行`Llama 3 8B`模型：

```
ollama run llama3
```

1.  `Ollama`会自动下载模型并启动交互式会话

1.  测试模型：输入你好，请介绍一下自己，查看模型回复

1.  退出交互式会话：输入`/bye`

### 2.3 启动Ollama API服务[#](https://claudecode.tangshuang.net/easy-tutorial/2.5%20%E4%BD%BF%E7%94%A8%E5%85%8D%E8%B4%B9%E7%9A%84%E5%A4%A7%E6%A8%A1%E5%9E%8B#2.3%20%E5%90%AF%E5%8A%A8Ollama%20API%E6%9C%8D%E5%8A%A1)

1.  打开终端，运行以下命令启动`Ollama` API服务：

```
ollama serve
```

1.  API服务默认运行在`http://localhost:11434`

1.  测试API服务：

```
curl http://localhost:11434/api/chat -d &#x27;{&quot;model&quot;: &quot;llama3&quot;, &quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好&quot;}]}&#x27;
```

1.  如果返回模型回复，则说明API服务运行正常

## 三、将免费大模型接入到`ClaudeCodeRouter`

`ClaudeCodeRouter`是一个中间层服务，可以将多种大模型接入到`Claude Code`中。

### 3.1 安装`ClaudeCodeRouter`

请参考上一节内容。

### 3.2 配置`ClaudeCodeRouter`

1.  创建配置文件`router.config.json`：

```
{
  &quot;port&quot;: 3000,
  &quot;models&quot;: [
    {
      &quot;name&quot;: &quot;llama3&quot;,
      &quot;type&quot;: &quot;ollama&quot;,
      &quot;baseUrl&quot;: &quot;http://localhost:11434&quot;,
      &quot;model&quot;: &quot;llama3&quot;
    },
    {
      &quot;name&quot;: &quot;openrouter-llama3&quot;,
      &quot;type&quot;: &quot;openrouter&quot;,
      &quot;apiKey&quot;: &quot;your_openrouter_api_key&quot;,
      &quot;model&quot;: &quot;meta-llama/llama-3-8b-instruct:free&quot;
    }
  ]
}
```

1.  配置说明：

+   `port`：`ClaudeCodeRouter`服务端口
+   `models`：模型列表，支持多种模型配置
+   `type`：模型类型，支持`ollama`、`openrouter`、`aliyun`等
+   `baseUrl`：模型API地址（针对本地模型）
+   `apiKey`：模型API密钥（针对云服务）
+   `model`：模型名称

### 3.3 启动`ClaudeCodeRouter`服务

打开终端，运行以下命令：

```
ccr start
```

## 四、将`Claude Code`接入到`ClaudeCodeRouter`

### 4.1 配置`Claude Code`

1.  打开`Claude Code`的配置文件，不同操作系统的位置：

+   macOS/Linux：`~/.claude/settings.json`
+   Windows：`%USERPROFILE%\.claude\settings.json`

1.  编辑配置文件，添加以下内容：

```
{
  &quot;env&quot;: {
    &quot;ANTHROPIC_BASE_URL&quot;: &quot;http://localhost:3456&quot;,
    &quot;ANTHROPIC_AUTH_TOKEN&quot;: &quot;sk-anything&quot;,
    &quot;ANTHROPIC_DEFAULT_OPUS_MODEL&quot;: &quot;llama3&quot;,
    &quot;ANTHROPIC_DEFAULT_SONNET_MODEL&quot;: &quot;llama3&quot;,
    &quot;ANTHROPIC_DEFAULT_HAIKU_MODEL&quot;: &quot;llama3&quot;
  }
}
```

1.  配置说明：

+   `ANTHROPIC_BASE_URL`：`ClaudeCodeRouter`服务地址
+   `ANTHROPIC_AUTH_TOKEN`：可以填写任意值，`ClaudeCodeRouter`暂不验证密钥



## 四、配置免费模型

### 4.1 配置环境变量

+   `ANTHROPIC_DEFAULT_*_MODEL`：将Claude模型映射到免费模型

### 4.2 重启Claude Code

1. 关闭所有Claude Code窗口

1. 打开一个新的终端窗口

1. 进入代码工作目录，运行`claude`命令启动Claude Code

### 4.3 验证配置

1. 在Claude Code的聊天界面中，输入测试消息：

```
你好，请介绍一下自己。
```

1. 如果看到免费模型的回复，说明配置成功

1. 输入`/status`命令，查看当前使用的模型状态

1. 可以看到模型名称显示为配置的免费模型

## 五、进阶配置与优化

### 5.1 配置多个免费模型

在`ClaudeCodeRouter`配置文件中，可以添加多个免费模型，实现模型切换：

```
{
  "port": 3000,
  "models": [
    {
      "name": "llama3",
      "type": "ollama",
      "baseUrl": "http://localhost:11434",
      "model": "llama3"
    },
    {
      "name": "mistral",
      "type": "ollama",
      "baseUrl": "http://localhost:11434",
      "model": "mistral"
    },
    {
      "name": "gemma",
      "type": "ollama",
      "baseUrl": "http://localhost:11434",
      "model": "gemma:7b"
    }
  ]
}
```

### 5.2 模型切换

在Claude Code中，可以通过修改配置文件中的`ANTHROPIC_DEFAULT_*_MODEL`值来切换不同的免费模型：

```
{
  "env": {
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "mistral",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "mistral",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "mistral"
  }
}
```

### 5.3 性能优化

+   对于本地运行的模型，可以调整Ollama的配置以提高性能：

```
export OLLAMA_NUM_GPU=1  # 使用GPU加速
ollama serve
```

对于`ClaudeCodeRouter`，可以调整日志级别和超时时间：

```
{
  "port": 3000,
  "logLevel": "info",
  "timeout": 60000,
  "models": [
    // 模型配置
  ]
}
```

## 六、常见问题及解决方案

### 6.1 `ClaudeCodeRouter`启动失败

+   检查端口是否被占用：使用`lsof -i :3000`查看端口占用情况

+   检查配置文件格式是否正确：使用JSON验证工具检查配置文件

+   检查Node.js版本是否符合要求：建议使用v18或更高版本

### 6.2 Claude Code无法连接到`ClaudeCodeRouter`

+   检查`ClaudeCodeRouter`服务是否正在运行

+   检查配置文件中的`ANTHROPIC_BASE_URL`是否正确

+   检查网络连接是否正常

+   尝试重启Claude Code和`ClaudeCodeRouter`

### 6.3 模型回复速度慢

+   对于本地模型，检查电脑配置是否足够：建议至少8GB RAM，最好有GPU加速

+   对于云服务模型，检查网络连接是否稳定

+   尝试使用更小尺寸的模型，例如7B参数的模型

### 6.4 模型回复质量不佳

+   尝试使用更适合代码生成的模型，例如DeepSeek-Coder、CodeLlama等

+   优化提示词，提供更清晰的指令

+   调整模型参数，例如`temperature`、`top_p`等

## 七、总结

通过以上步骤，我们可以将免费的大模型接入到Claude Code中，主要包括以下几种方式：

1. 使用OpenRouter、阿里云等免费大模型提供商的服务

1. 本地搭建Ollama运行免费大模型

1. 通过`ClaudeCodeRouter`将多种模型统一管理

1. 将Claude Code配置为使用`ClaudeCodeRouter`提供的免费模型

这种方式不仅可以节省API调用费用，还可以根据需求选择不同的模型，提高开发效率。随着开源大模型的不断发展，免费模型的性能和功能也在不断提升，为开发者提供了更多的选择。

建议开发者根据自己的需求和使用场景，选择合适的免费模型接入方式。对于个人学习和小型项目，本地搭建Ollama是一个不错的选择；对于需要更高性能的场景，可以考虑使用OpenRouter等平台提供的免费模型；而对于需要管理多个模型的团队，可以使用`ClaudeCodeRouter`实现统一管理。
