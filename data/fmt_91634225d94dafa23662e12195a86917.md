
# Claude Code 安装使用指南

> 来源：https://xodnytdcaw.feishu.cn/docx/B7rKdZAlvoFJs8xWJKkcQUThnUg

你是否厌倦了在 IDE 和 ChatGPT 之间来回切换？渴望一个真正懂你代码的 AI 助手？

这几天，Claude Code 彻底火了，网上都在夸赞这个由 Anthropic 出品的命令行 AI 编码工具。不同于那些花哨的 AI 插件，Claude Code 直接住进你的终端，像个老司机一样帮你写代码、修 bug、重构项目。

我也测试了一圈下来，确实牛批！免费、不限量、速度快，关键是它真的能理解你的整个项目结构。

今天，我就给大家整理一份保姆级教程，让你 5 分钟上手 Claude Code。

## 一. 准备工作

1. Node.js 18 或更高版本

> 请前往 Node.js 官方网站安装最新 LTS 版本，确保 Claude Code 能正常运行。
> https://nodejs.org/en/download/

2. Git（Windows 用户必需）

> 如果你使用 Windows，请提前安装 Git for Windows，否则后续依赖安装可能失败。
> https://git-scm.com/install/windows

## 二. 安装 Claude Code（卸载方法：[点此](https://www.i7dc.com/docs/getting-started/claude-code-uninstall)）

```bash
npm install -g @anthropic-ai/claude-code
```

安装完成后，输入下面的命令查看是否安装成功。只要能看到版本号，就表示已经就绪：

```bash
claude --version
```

![](img/6d7d36b686f8a5287155bf30a990df82.png)

### 启动 Claude Code

重新打开一个新的终端窗口，进入你的项目目录，运行 `claude`。

```bash
cd test
claude
```

![](img/1d2f2de1a66d1b539daa19e08225e700.png)

> 启动后会要求你授权，为了节约成本，这里推荐接入 API 的方法使用，你可以订阅智谱 GLM Coding Lite，然后将 API 通过 `CC-switch` 进行管理启动，供 `Claude Code` 使用，当然你有 `gemini pro` 会员，也可以借助 `Antigravity Tools` 反代 API，通过 `CC-switch` 进行管理启动供 `Claude Code` 使用。

## 三. 配置 Claude Code 的 API

### 1. 购买 API KEY

[点此领优惠券购买](https://www.bigmodel.cn/glm-coding?ic=PUCQNADSPM) 3 个月只要 48.6

![](img/ae16612e502c53ea31d69b7c96e50cca.png)

### 2. 创建 API key

![](img/14e9ca43e95e66a626f06b3f673519df.png)

### 3. 配置 API 三种方法：

#### 方法 1：CC-switch [点此直达](https://github.com/farion1231/cc-switch) （推荐）

> 一款基于 Rust + Tauri 开发的跨平台 AI 工具配置管理工具，核心功能是一键切换 `Claude Code`、`Codex`、`Gemini CLI` 等 AI 编程工具的多套 API / 供应商配置，还具备可视化管理、技能扩展与云端同步能力，能替代手动编辑 JSON 配置的繁琐操作，大幅提升 AI 开发效率。

#### 方法 2：Coding Tool Helper

`win + r` 输入 `cmd`，粘贴以下命令可以直接将 `GLM` 的 API 服务器配置到 `Claude Code`

```bash
npx @z_ai/coding-helper
```

进入这个界面，选择第三个回车，然后粘贴 API

![](img/b02326dae9a3fb5e6f583b6ab9c173bb.png)

![](img/33bdf8626cf68224d6c6eef1556791f0.png)

#### 方法 3：`Claude-code-router`

可以把任意的大模型接入到 `Claude Code`

安装命令  视频教程：[点此直达](https://www.bilibili.com/video/BV1jGtEzNEAv/?spm_id_from=333.1387.upload.video_card.click&vd_source=206031f494850e57fd6c92ace02b1bed)

```bash
npm install -g @musistudio/claude-code-router
```

创建配置文件

> `~/.claude-code-router/config.json`

配置文件 `config.json` 样例

```json
{
  "Providers": [
    {
      "name": "modelscope",
      "api_base_url": "http://127.0.0.1:8045/v1beta/models",
      "api_key": "sk-",
      "models": ["gemini-3-flash", "gemini-3-pro-high"],
      "transformer": {
        "use": [
          [
            "maxtoken",
            {
              "max_tokens": 65536
            }
          ],
          "enhancetool"
        ],
        "gemini-3-pro-high": {
          "use": ["reasoning"]
        }
      }
    }
  ],
  "Router": {
    "default": "modelscope,gemini-3-pro-high" // <--- 修改这里
  }
}
```

在桌面新建 `test` 文件，打开 `cmd`，输入以下命令启动

```bash
ccr code
```

## 四. 上手 `claude` Skills

`Skills` 是给 Claude 装的"专业技能包"。就像 RPG 游戏里给角色学技能，一个 `Skill` 让 Claude 在特定领域变成专家。`Skills` 是打包好的指令、脚本和资源，Claude 会自动发现并按需加载，让它在特定任务上表现更好。

目前 `skills` 支持在 `Claude.ai`、`Claude Code` 以及 API 中使用，`Skills` 有两个位置

```
~/.claude/skills/              # 全局 Skills  (所有项目可用)
project/.claude/skills/        # 项目 skills (仅当前项目，可 git 共享)
```

> 案例

> [使用 Claude Code 的 Slash Command 编排写作工作流](https://xodnytdcaw.feishu.cn/wiki/TN6OwOsijiyDL9kjWQ1cw6a9nxe)

> GitHub 上 1400 星的 `Skills` 清单,老金给你扒个明白 - 知乎

### 理解 `CLAUDE.md`

存储位置：项目根目录 `./CLAUDE.md`

![](img/4aeaa7b0bba9f0874d76916310cbb88e.png)

### 理解 `Claude Code` 的 Slash Commands（自定义斜杠命令）

```bash
mkdir -p ~/.claude/commands
```

```bash
touch ~/.claude/commands/search-web.md
```

`search-web.md` 内容

```markdown
---
allowed-tools: Bash(*)
description: 使用 Gemini CLI 搜索特定站点并进行 UltraThink 总结
argument-hint: [搜索关键词]
---

# 任务目标
在互联网上搜索关于 "$ARGUMENTS" 的内容，并挖掘最靠谱、最能解答疑问的信息。

# 搜索要求
1. **限定范围**：重点关注以下站点的资料和动态：
   - `anthropic.com`
   - `x.com`
   - `reddit.com`
   - `linux.do`
2. **时效性**：搜索当前月份（`!`date "+%Y年%m月"`）的最新消息 [1]。
3. **执行工具**：必须调用本地的 `gemini cli` 工具，并利用其 `GoogleSearch` 功能执行查询。
4. **并行处理**：请启动多个并行的子任务（同时调用多次 `Bash` 工具）分别检索上述不同站点，以加快执行速度。
5. **总结逻辑**：在给出最终回复前，请开启 **UltraThink (Extended Thinking)** 模式。深度分析各方资料的冲突、真实性和深度，最后提供一份结构严密的总结。

# 搜索指令示例（供你参考执行）
- `gemini search "site:linux.do $ARGUMENTS"`
- `gemini search "site:anthropic.com $ARGUMENTS"`
```

使用示例

```
/search-web  Claude Code 上手教程
```

## 六. 常见问题

### 1. PowerShell 运行不了命令 `claude`

以管理员身份运行 `PowerShell`，执行：`Set-ExecutionPolicy RemoteSigned`（该策略允许运行本地签名脚本、远程可信签名脚本），按提示输入 `Y` 确认。

![](img/daa3942efc46fa1d3668dff0273360c1.png)

### 2. 网络问题

`windows` 下的 `cmd` 默认不支持 🪜，需要在环境变量下设置，并让 🪜 走全局代理 ''

![](img/726143f8d723c15f1f930715c44662e0.png)

![](img/daef88c369f59e6eed37e7aeb71f4adc.png)

![](img/1992f1b44692a6bdbb695f24b12dd891.png)

+ 检查这个文件 API 正确 `C:\Users\Administrator\.claude\settings.json`

+ 检查 `C:\Users\Administrator\.claude.json`

最后的结局方案：

我自己用的 `OpenWrt` 系统路由器，给电脑 `direct` 标签（可选），电脑使用客户端解决

配置环境变量 创建文件夹 `D:\Tools` 里面 `cl.bat` 端口改你自己

```batch
@echo off
set HTTP_PROXY=http://127.0.0.1:3067
set HTTPS_PROXY=http://127.0.0.1:3067
echo [Claude CLI] 代理隧道已建立 (Port:3067)...
for /f "delims=" %%i in ('where claude') do (
    "%%i" %*
    goto :finish
)

echo.
echo [Error] 未在系统路径中找到 'claude' 程序。
echo 请确保你已经正确安装了 `claude-cli` 并且它在你的系统环境变量 `PATH` 中。
goto :finish

:finish
:: 脚本结束，临时设置的环境变量会自动销毁，不影响系统其他程序
echo.
```

然后执行 `sysdm.cpl` 进入配置环境变量，两种方法都可以

方法 1：用户变量 --- `Path` --- 编辑 --- 新建粘贴 `D:\Tools`

方法 2：系统变量 --- 新建

> 变量名：`http_proxy` 变量值：`http://127.0.0.1:1080`

> 变量名：`https_proxy` 变量值：`http://127.0.0.1:1080`

## 七. 写在最后

`Claude Code` 确实是目前最好用的 AI 编程工具之一，它不像其他工具那样花哨，而是专注于真正提升你的编程效率。当然，一些简单的任务基本用不到 AI，反而会拖累你的效率，日常可以根据自己的实际情况，是否使用 AI 辅助。以上，既然看到这里了，如果觉得不错，随手点个赞、在看、转发三连吧，如果想第一时间收到推送，也可以给我个星标 ⭐～

谢谢你看我的文章，我们，下次再见。

PS. 如有网络不便下载，回复暗号「`ClaudeCode`」获取安装包网盘下载地址。

### 补充：参考资料

[Claude Code 从入门到狂飙](https://ai.feishu.cn/wiki/JAjhwcIlHixsf1kDlI8cHAfanyd)

```
36233@r2
```

`Claude Code` 超级小白入门指南

https://mp.weixin.qq.com/s/pEPweAhpzZb1ef5oMvaDDA
