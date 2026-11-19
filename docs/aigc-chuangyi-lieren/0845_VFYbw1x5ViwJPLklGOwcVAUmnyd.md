
# OpenCode + Oh My OpenCode 配置和开发教程（新手小白专用）

> 来源：https://aigcreative.feishu.cn/docx/DdeLdXZrUoX3GLxUuN1ci2ulnbb

大家好！如果你是编程新手，对AI辅助开发工具感兴趣，但又不知道从哪里入手，这个教程就是为你准备的。我们会一步一步走，从零开始安装、配置，到实际开发使用。整个过程像聊天一样简单，不需要高深的编程知识。

首先，简单介绍一下：

+ `OpenCode`：这是一个开源的AI编程助手，像Claude Code的免费替代品。它可以帮你分析代码、生成功能、修改项目，支持多种AI模型（如Claude、ChatGPT、Gemini）。它运行在终端里，超级方便。

+ `Oh My OpenCode`（也叫`Oh My Opencode`）：这是OpenCode的增强插件，像“Oh My Zsh”一样，添加了更多强大功能。比如多AI模型协作、智能体系统（Agents）、提示词优化等，让开发效率翻倍。安装它后，OpenCode会变得更智能。

教程基于官方文档和GitHub README，假设你是Windows、macOS或Linux用户。如果卡壳了，可以参考视频教程（如YouTube上的OpenCode入门视频）。

## 第一步：准备前提条件

在开始前，确保你的电脑满足这些：

+ 一个现代终端（命令行窗口）。推荐：

+ [macOS/Linux](https://alacritty.org/)：用Terminal或安装Alacritty。

+ [Windows](https://wezterm.org/)：用Windows Terminal（系统自带）或WezTerm。

+ 网络连接（安装时需要下载）。

+ 可选：ChatGPT Plus、Claude Pro或Gemini订阅（免费模型也能用，但付费的更强）。如果你有这些账号，准备好API密钥。

+ Node.js或Bun（Oh My OpenCode需要）。如果没有，安装脚本会帮你处理。

如果你是纯小白，先打开你的终端（命令行）：

+ Windows：搜索“cmd”或“PowerShell”。

+ macOS：搜索“Terminal”。

+ Linux：Ctrl+Alt+T。

## 第二步：安装OpenCode

OpenCode安装超级简单，一行命令搞定。

1. 打开终端，运行这个安装脚本：

```
curl -fsSL https://opencode.ai/install | bash
```

+ 如果你是Windows，用PowerShell运行：

```
Invoke-WebRequest -Uri https://opencode.ai/install -UseBasicParsing | Invoke-Expression
```

+ 其他方式（如果curl不行）：

+ 用npm（需先安装Node.js）：

```
npm install -g opencode-ai
```

+ macOS用Homebrew：

```
brew install anomalyco/tap/opencode
```

2. 验证安装：运行

```
opencode --version
```

3. 如果显示版本号（如1.0.150或更高），就成功了！如果报错，检查网络或重启终端。

## 第三步：配置OpenCode

配置主要是设置AI模型的API密钥，让OpenCode能调用AI。

1. 运行OpenCode：

```
opencode
```

2. 这会打开一个终端界面（TUI，像聊天窗口）。

3. 在界面里输入命令`/connect`，选择提供商（如`opencode`、`anthropic`、`openai`、`google`）。

+ 推荐新手用`opencode`（官方免费试用）：访问opencode.ai/auth，登录，复制API密钥，粘贴到终端。

+ 如果用Claude：选anthropic，输入你的API密钥（从Claude官网获取）。

+ ChatGPT：选openai，输入OpenAI API密钥。

+ Gemini：选google，类似。

4. 配置好后，退出（Ctrl+C），重启opencode测试。如果看到欢迎界面，就OK了。

高级配置（可选，新手可跳过）：编辑配置文件`~/.config/opencode/opencode.json`（Windows是`%APPDATA%\opencode\opencode.json`）。比如添加主题或键位。

## 第四步：安装Oh My OpenCode

这个插件让OpenCode升级为“多AI团队”。安装基于你的订阅情况。

1. 先确认你的订阅（用于配置参数）：

+ Claude Pro/Max：用`--claude=yes`或`--claude=max20`（max20是高级模式）。

+ ChatGPT Plus：用`--chatgpt=yes`。

+ Gemini：用`--gemini=yes`。

+ 没有就用`no`。

2. 安装Bun（如果没装，Oh My需要）：

```
curl -fsSL https://bun.sh/install | bash
```

3. 重启终端。

4. 一键安装Oh My OpenCode：

```
bunx oh-my-opencode install --no-tui --claude=yes --chatgpt=yes --gemini=yes
```

+ 替换参数根据你的订阅。比如只有Claude：

```
bunx oh-my-opencode install --no-tui --claude=yes --chatgpt=no --gemini=no
```

+ 如果Bun有问题，用npx（需Node.js）：

```
npx oh-my-opencode install --no-tui --claude=yes ...
```

5. 验证：运行

```
cat ~/.config/opencode/opencode.json | grep "oh-my-opencode"
```

6. 如果看到插件名，就安装成功了。

7. 完成认证（如果用付费模型）：

+ 运行`opencode auth login`，选提供商，浏览器登录OAuth。

+ 对于Gemini/ChatGPT，可能需额外插件（如`opencode-antigravity-auth`），加到`opencode.json`的`plugin`数组里。

## 第五步：配置Oh My OpenCode

配置在JSON文件里，新手用默认就行，但我们可以简单调整。

1. 配置文件位置：

+ 全局：`~/.config/opencode/oh-my-opencode.json`

+ 项目级：在项目文件夹下创建`.opencode/oh-my-opencode.json`

2. 打开文件（用记事本或VS Code），添加基本配置。例如：

```
{
  "google_auth": false,  // 如果用外部Gemini认证
  "agents": {
    "frontend-ui-ux-engineer": { "model": "google/gemini-3-pro-high" },  // 自定义模型
    "oracle": { "model": "openai/gpt-5.2" }  // 添加新Agent
  },
  "sisyphus_agent": { "disabled": false },  // 启用主编排器
  "background_task": { "defaultConcurrency": 5 }  // 并发任务数
}
```

+ 新手提示：复制这个，改成你的模型。保存后，重启opencode。

3. 如果想禁用某些功能，加`"disabled_agents": ["某个agent名"]`。

## 第六步：进入开发状态（实际使用）

现在，你可以开发了！OpenCode + Oh My像一个AI团队，帮你写代码。

1. 进入项目文件夹：

```
cd /path/to/your/project  // 替换成你的项目路径
opencode
```

2. 初始化项目：

输入`/init`，它会分析项目，生成`AGENTS.md`文件（定义编码规则）。

3. 基本开发示例：

+ 提问代码：输入“How does authentication work in main.js?”（Oh My的Agents会帮分析）。

+ 添加功能：切换到Plan模式（按Tab键，右下角显示Plan）。

输入： “Add a delete button for notes, flag as deleted in DB, and create a recovery screen.”

AI会生成计划。反馈后，切换回Build模式（Tab），输入“Go ahead!”让它执行代码修改。

+ 用Oh My的超级功能：输入“ulw Add a REST API”（ulw触发多Agent协作），AI团队会自动分工（一个写前端，一个写后端）。

+ 自定义Agent：输入“让 @oracle 设计数据库 schema。”（@oracle是Oh My的智能体）。

4. 其他命令：

+ `/undo`：撤销修改。

+ `/redo`：重做。

+ `/share`：分享对话。

新手Tips：

+ 多用自然语言描述需求，像和朋友聊天。

+ 如果卡住，输入ulw让Oh My全自动处理复杂任务。

+ 测试代码：修改后，在终端运行你的项目（比如`node app.js`），看效果。

+ 学习更多：看Oh My的`Agents.md`文件，了解每个智能体的作用。

恭喜！你现在进入开发状态了！从简单项目开始练手，比如一个TODO列表App。遇到问题，搜索YouTube“OpenCode教程”或GitHub issue。

https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg

# 知识库动态
