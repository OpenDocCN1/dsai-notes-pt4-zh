# Claude Code 完全指南

> 来源：[https://my.feishu.cn/docx/K9zSdkkmWoS7EExCCGHc7VQpnkf](https://my.feishu.cn/docx/K9zSdkkmWoS7EExCCGHc7VQpnkf)

> 终端里的 AI 编程助手，不是帮你聊天，是帮你干活。

---

# 一、Claude Code 是什么？

**一句话**：Anthropic 官方出的 AI 编程工具，在终端里直接跑，不依赖任何 IDE。

和 Cursor、Copilot 的区别：

Claude Code 能做到的事：

+   读写你整个项目的代码

+   执行 shell 命令

+   跑测试、装依赖、改配置

+   理解项目结构，不是只看当前文件

+   真正"动手干活"，不是只给建议

**不能做的事**：

+   没有图形界面（纯终端）

+   不能直接操作浏览器（但可以通过 MCP 扩展）

---

# 二、为什么用 Claude Code？

## 适合什么人

+   经常用终端的开发者

+   需要改老项目、重构代码的人

+   想让 AI 真正"干活"而不是"建议"的人

+   追求效率，愿意学新工具的人

## 适合什么场景

+   **新项目初始化**：一条命令搞定脚手架

+   **代码审查**：让 AI 过一遍，找出潜在问题

+   **Bug 调试**：给它错误信息，它帮你定位

+   **老项目测试覆盖**：给没有测试的代码补测试

+   **日常开发**：写代码、改代码、跑测试

## 国内使用的难点

+   官方需要 Anthropic API，贵且要🪜

+   但可以接入国内模型（智谱、DeepSeek 等），便宜且不需要🪜

---

# 三、安装教程

## 3.1 前置条件

## 3.2 Windows 安装

```
# 1. 安装 Node.js（去 https://nodejs.org 下载安装包）

# 2. 安装 Git（去 https://git-scm.com 下载安装包）

# 3. 以管理员身份打开 PowerShell，运行：
npm install -g @anthropic-ai/claude-code

# 4. 验证安装
claude --version
```

## 3.3 Mac 安装

```
# 1. 安装 Node.js（没有 Homebrew 的话先装）
brew install node@20

# 2. 安装 Claude Code
npm install -g @anthropic-ai/claude-code

# 3. 验证安装
claude --version
```

## 3.4 验证安装成功

```
# 查看版本
claude --version

# 启动 Claude Code
claude
```

如果能启动并看到 Claude 的提示符，就装好了。

---

# 四、跳过官方登录，接入国内模型

这是国内用户最关心的部分。官方 Anthropic API 贵且要🪜，但你可以接入国内模型。

## 方案对比

## 4.1 智谱 GLM-5 配置（推荐）

**为什么推荐**：国内直连、便宜、中文好、官方支持 Claude Code 接入。

**第一步：获取 API Key**

1.  访问 https://www.bigmodel.cn/glm-coding?ic=HQSELTFKYT

1.  注册登录（手机号就行）

1.  进入「API Keys」页面

1.  创建新密钥，复制保存

第二步：配置 Claude Code

配置文件位置：

+   Windows: C:\Users\你的用户名\.claude\settings.json

+   Mac/Linux: ~/.claude/settings.json

如果文件不存在，手动创建。写入以下内容：

```
{
  &quot;env&quot;: {
    &quot;ANTHROPIC_AUTH_TOKEN&quot;: &quot;你的智谱API Key&quot;,
    &quot;ANTHROPIC_BASE_URL&quot;: &quot;https://open.bigmodel.cn/api/paas/v4&quot;
  },
  &quot;permissions&quot;: {
    &quot;allow&quot;: [
      &quot;Bash(npm *)&quot;,
      &quot;Bash(npx *)&quot;,
      &quot;Bash(node *)&quot;,
      &quot;Bash(git *)&quot;,
      &quot;Read&quot;,
      &quot;Write&quot;,
      &quot;Edit&quot;
    ]
  }
}
```

**第三步：验证**

```
# 启动 Claude Code
claude

# 发一条消息测试
你好，介绍一下你自己
你好，你是什么模型
```

如果能正常回复，说明配置成功。

**智谱套餐参考**（2026年3月）：

## 4.2 DeepSeek 配置

**第一步：获取 API Key**

1.  访问 https://platform.deepseek.com/

1.  注册登录

1.  进入「API Keys」页面

1.  创建新密钥

**第二步：配置**

```
{
  &quot;env&quot;: {
    &quot;ANTHROPIC_AUTH_TOKEN&quot;: &quot;你的DeepSeek API Key&quot;,
    &quot;ANTHROPIC_BASE_URL&quot;: &quot;https://api.deepseek.com&quot;
  }
}
```

## 4.3 硅基流动配置

硅基流动提供多种模型，包括免费的。

**第一步：获取 API Key**

1.  访问 https://siliconflow.cn/

1.  注册登录

1.  进入控制台创建 API Key

**第二步：配置**

```
{
  &quot;env&quot;: {
    &quot;ANTHROPIC_AUTH_TOKEN&quot;: &quot;你的硅基流动 API Key&quot;,
    &quot;ANTHROPIC_BASE_URL&quot;: &quot;https://api.siliconflow.cn/v1&quot;
  }
}
```

---

# 五、常用命令和快捷键

## 5.1 启动和退出

```
# 启动（在项目目录下运行）
claude

# 启动并指定目录
claude /path/to/project

# 退出
# 按 Ctrl+D 或输入 exit
```

## 5.2 核心命令

在 Claude Code 内部输入：

## 5.3 快捷键速查

## 5.4 三种模式

按 Shift+Tab 循环切换：

---

# 六、实际使用场景

## 6.1 新项目初始化

```
# 在项目目录下
claude

# 输入
/init
```

Claude 会分析项目结构，生成 CLAUDE.md 文件，以后每次打开项目它都会读这个文件，了解项目信息。

**CLAUDE.md 建议写入**：

+   项目是什么

+   常用命令（构建、测试、部署）

+   目录结构说明

+   编码规范

## 6.2 代码审查

```
帮我审查一下 src/ 目录下的代码，重点关注：
1. 安全问题
2. 性能问题
3. 代码风格
```

## 6.3 Bug 调试

```
跑测试报错了，错误信息是：
[粘贴错误信息]

帮我定位问题并修复
```

## 6.4 老项目测试覆盖

```
给 src/utils.js 补单元测试，用 Jest
```

## 6.5 日常开发

```
帮我实现用户登录功能，要求：
1. 支持手机号 + 验证码
2. 支持 JWT
3. 写入 src/auth/ 目录
```

---

# 七、Claude Code + Obsidian 联动

这是我最喜欢的用法：**人和 AI 一起写文档**。

## 7.1 为什么爽？

+   Obsidian 是本地 Markdown 知识库

+   Claude Code 能读写本地文件

+   两者结合 = AI 直接参与你的知识管理

## 7.2 怎么用？

**场景一：让 AI 帮你整理笔记**

```
读取我的 Obsidian 仓库 E:\Obsidian仓库\万航宇的第二大脑\
帮我整理「编程」相关的笔记，按主题分类
```

**场景二：AI 帮你写文档**

```
帮我写一篇关于 Docker 的入门教程
保存到 E:\Obsidian仓库\万航宇的第二大脑\Docker入门.md
```

**场景三：知识库搜索**

```
在我的 Obsidian 仓库里搜索关于「API 设计」的内容
汇总成一份清单
```

## 7.3 Skills 管理

Claude Code 的 Skills 可以放在 Obsidian 里管理：

```
我的 Skills 存在 E:\Obsidian仓库\skills\
帮我创建一个新的 Skill，用于代码审查
```

## 7.4 人机协作写文档的感觉

1.  你告诉 AI 想写什么

1.  AI 出初稿

1.  你在 Obsidian 里修改

1.  让 AI 继续完善

1.  来回迭代，文档越来越好

**这种感觉很奇妙**：不是 AI 替你写，是你和 AI 一起写。你的思路 + AI 的执行力 = 高效产出。

---

# 八、进阶技巧

## 8.1 CLAUDE.md 配置

每个项目根目录放一个 CLAUDE.md，Claude 每次启动都会读。

**模板**：

```
# 项目名称

## 是什么
一句话描述

## 常用命令
- 构建：npm run build
- 测试：npm test
- 启动：npm run dev

## 目录结构
- src/ - 源代码
- tests/ - 测试
- docs/ - 文档

## 编码规范
- 使用 TypeScript
- 遵循 ESLint 规则
```

## 8.2 权限配置

在 settings.json 里配置权限：

```
{
  &quot;permissions&quot;: {
    &quot;allow&quot;: [
      &quot;Bash(npm *)&quot;,
      &quot;Bash(git *)&quot;,
      &quot;Read&quot;,
      &quot;Write&quot;
    ],
    &quot;deny&quot;: [
      &quot;Bash(rm -rf *)&quot;  // 禁止危险命令
    ]
  }
}
```

## 8.3 并行多开

可以同时开多个终端窗口，每个跑一个 Claude Code 实例。

```
# 终端 1：处理项目 A
cd /path/to/projectA
claude

# 终端 2：处理项目 B
cd /path/to/projectB
claude
```

## 8.4 MCP 服务器

MCP（Model Context Protocol）可以扩展 Claude Code 的能力，比如联网搜索、数据库操作等。

智谱 GLM Pro 套餐自带联网搜索 MCP，配置后可以直接用。

---

# 九、常见问题

**Q: 安装后输入 ****claude**** 提示命令不存在？**

A: 关掉终端重新打开试试。还不行就检查 PATH：

```
# 查看 npm 全局路径
npm config get prefix

# 确保这个路径在 PATH 里
```

**Q: 模型连接不上？**

A:

1.  检查 API Key 是否正确

1.  检查 API Key 是否有余额

1.  检查 ANTHROPIC_BASE_URL 是否正确

1.  用官方 API 的话检查网络（需要🪜）

**Q: Token 消耗太快？**

A:

1.  用 /compact 压缩上下文

1.  用 /clear 清除历史

1.  换更便宜的模型（GLM-5 比 Claude 便宜很多）

1.  不要把大文件整个扔进去

**Q: AI 改错了代码怎么办？**

A:

1.  按 Esc Esc 打开重放菜单

1.  选择回退到之前的状态

1.  重新描述你的需求

**Q: 想换模型怎么办？**

A: 直接改 settings.json 里的配置，然后重启 Claude Code。

---

# 十、相关资源

## 官方资源

## 国内教程

## API 提供商

## 相关工具

---

# 十一、快速上手 Checklist

按这个顺序来，30 分钟内搞定：

---

最后更新：2026年3月9日