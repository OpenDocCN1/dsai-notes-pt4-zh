
| 安装类型与版本 | 还在用旧的 `npm` 安装方式，应该迁移到原生安装器 |

| 自动更新状态 | 当前版本过旧，已知 Bug 在新版中已修复 |

| 配置文件有效性 | `settings.json` 里多了个逗号导致 JSON 解析失败，所有配置项静默失效 |

| MCP 服务器配置 | 某个 `MCP Server` 的路径写错了，或者依赖的 `Node.js` 版本不对 |

| 快捷键配置 | 自定义的快捷键与系统默认键冲突，导致某些操作无响应 |

| 上下文使用情况 | `CLAUDE.md` 文件写了 2000 行，每次对话都白白消耗大量 Token 在加载记忆上 |

| 插件与 Agent 加载 | 安装的某个 Plugin 与当前 Claude Code 版本不兼容 |

![](img/a521261cff8fc129dac28e435441af72.png)

> **💡 提示：**养成一个好习惯——任何时候 Claude Code 的行为不符合预期，先跑一次 `claude doctor`。它相当于 Claude Code 的"体检报告"，80% 的常见问题都能在这里找到线索。在向社区求助或提交 `GitHub Issue` 时，附上 `doctor` 的输出也能显著加速问题定位。

### 3. Fufan-CC Flow：在 Web 界面中安装与验证 Claude Code

对于报名正课的学员，还可以通过本课程的配套项目 Fufan-CC Flow 来完成上述所有操作。Fufan-CC Flow 是我们打造的 Claude Code Web 图形化前端——在 Settings 页面中，安装状态检测、版本更新、连通性验证等操作都可以通过可视化界面一键完成，无需手动输入任何命令。

Settings 页面采用两步验证流程 (Two-Step Checklist) 设计：

**Step 1 — 环境检测**：自动检查 Claude Code 是否已安装、当前版本号、代理连通性。页面底部内嵌 `MiniTerminal`，可直接执行安装和更新命令

**Step 2 — 模型配置**：配置 Anthropic 官方 `API Key` 或切换国产基座模型（后续章节详细介绍）

![](img/256c4e241afce54974b21a4be5f5b046.png)

### 4. 登录验证与计费方式

Claude Code 安装完成后，首次运行需要完成身份认证。Claude Code 支持两种认证方式，对应不同的计费模式：

#### 4.1 认证方式对比

| **维度** | **OAuth 订阅登录** | **API Key 登录** |
| --- | --- | --- |
| **认证方式** | 浏览器跳转 Anthropic 账户授权 | 设置 `ANTHROPIC_API_KEY` 环境变量 |
| **计费模式** | 包含在 Pro/Max 订阅套餐内 | 按 Token 用量付费（后付费） |
| **适用人群** | 个人开发者、轻度使用 | 团队/企业、重度使用、需要精细成本控制 |
| **费用透明度** | 按"消息数"限额，不可见具体 Token 消耗 | 完全按 Token 计费，可精确计算成本 |
| **额度限制** | 滚动 5 小时窗口限额 | 受组织级 TPM/RPM 限制 |

![](img/9daa8e450d34c8e951b941cc03113039.png)

**OAuth 登录流程**：

```
claude
# 首次运行会自动打开浏览器，跳转到 Anthropic 授权页面
# 授权完成后自动返回终端，进入对话
```

![](img/714b312e94ac116842aa2b29e265942a.png)

OAuth 授权完成后，Claude Code 会将获取到的 `OAuth Token`（包含 `Access Token` 和 `Refresh Token`）保存在本地。macOS 上凭证存储在系统钥匙串 (Keychain) 中（加密保护），Linux 和 Windows 上则存储在 `~/.claude/.credentials.json` 文件中。后续启动 Claude Code 时，程序会自动读取本地凭证完成身份验证，无需重复登录——除非 `Token` 过期或主动执行 `/logout`。

**API Key 登录流程**：

```
# 方式一：通过环境变量设置（推荐）
export ANTHROPIC_API_KEY="sk-ant-api03-xxxxxxxxxxxx"
claude

# 方式二：首次运行时选择 API Key 选项，在终端中输入
```

与 OAuth 不同，`API Key` 不涉及浏览器跳转。它的本质是将密钥写入环境变量或本地配置文件。如果通过 `export ANTHROPIC_API_KEY=...` 设置，密钥仅在当前终端会话中有效，关闭终端即失效；如果在首次运行 `claude` 时交互式输入 `API Key`，Claude Code 会将其持久化保存到 `~/.claude/settings.json` 的 `env` 节点中，后续启动自动读取。

> **⚠️ 注意**：`API Key` 等同于你的账户密码，泄露后他人可以用你的账户产生费用。切勿将 `API Key` 提交到 `Git` 仓库或分享给他人。如果不慎泄露，应立即在 `Anthropic Console` 中轮换密钥。

#### 4.2 基础环境依赖：让 Claude Code 完整运行

Claude Code 本体安装完成、认证也通过了，但这并不意味着所有功能都能正常工作。Claude Code 在运行过程中会调用一些系统级工具——如果这些工具缺失，部分功能会静默失败或报错。以下是各平台需要确认的依赖项：

| **依赖** | **用途** | **平台** | **重要性** |
| --- | --- | --- | --- |
| `Git` | `Checkpoint` 检查点（自动保存代码快照）、`/diff` 差异对比、版本控制操作 | 全平台必需 | ⭐ 必装，缺失将导致核心功能不可用 |
| `Xcode Command Line Tools` | 提供 `gcc`、`make` 等编译工具链，部分 `MCP Server` 安装时需要编译原生模块 | macOS 必需 | ⭐ macOS 用户必装 |
| `ripgrep (rg)` | Claude Code 的 `Grep` 工具底层依赖，用于高速代码搜索 | 全平台 | 通常随 Claude Code 自带，一般无需手动安装 |

**快速检查方式**——在终端中逐一验证：

```
git --version          # 应输出 git version 2.x.x
xcode-select -p        # macOS：应输出 /Library/Developer/CommandLineTools（Windows/Linux 跳过）
rg --version           # 应输出 ripgrep 14.x.x（可选检查）
```

如果上述命令提示"命令未找到"，说明对应工具尚未安装，请按照下面的指引完成安装。

##### Git 安装指引

**macOS**：

macOS 通常预装了 `Git`（随 `Xcode Command Line Tools` 一起提供）。如果执行 `git --version` 无输出，最快的方式是安装 `Xcode Command Line Tools`（下一节），它会同时安装 `Git`。也可以通过 `Homebrew` 单独安装：

```
brew install git
```

**Windows**：

访问 Git 官方下载页 `git-scm.com/downloads/win`，下载 `Git for Windows Setup` 安装包。运行安装程序时，关键选项建议如下：

| **安装步骤** | **推荐选择** |
| --- | --- |
| `Default Editor` | 选择你惯用的编辑器（如 `VS Code`） |
| `PATH environment` | 选择 "Git from the command line and also from 3rd-party software"（默认项，确保终端可用） |
| `Line ending` | 选择 "Checkout as-is, commit Unix-style"（适合跨平台开发） |
| 其余选项 | 保持默认即可 |

安装完成后，重新打开终端（`PowerShell` 或 `Git Bash`），执行 `git --version` 确认输出版本号。

**Linux**：

```
# Ubuntu / Debian
sudo apt update && sudo apt install git

# CentOS / RHEL / Fedora
sudo dnf install git
```

##### Xcode Command Line Tools 安装指引（仅 macOS）

`Xcode Command Line Tools` 包含了 `git`、`gcc`、`make`、`clang` 等开发者必备工具。macOS 用户强烈建议安装——不仅 Claude Code 需要，后续安装部分 `MCP Server`（需要编译原生 `Node.js` 模块）时也会依赖它。

```
xcode-select --install
```

执行后系统会弹出安装确认窗口，点击"安装"并等待完成（约 5-10 分钟，下载约 1.5 GB）。安装完成后验证：

```
xcode-select -p
# 应输出：/Library/Developer/CommandLineTools
```

> **💡 提示：**如果你已安装完整版 `Xcode`（从 `App Store`），`Command Line Tools` 已包含在内，无需重复安装。

##### ripgrep 安装指引（通常无需手动安装）

`ripgrep`（`rg`）通常随 Claude Code 自动安装。如果执行 `rg --version` 提示未找到命令，可手动安装：

```
# macOS
brew install ripgrep

# Windows（使用 winget）
winget install BurntSushi.ripgrep.MSVC

# Linux（Ubuntu / Debian）
sudo apt install ripgrep
```

三项依赖中，`Git` 是最关键的——没有 `Git`，Claude Code 的 `Checkpoint` 回滚机制将完全无法工作，`/diff` 命令也无法使用。请务必确认 `Git` 已正确安装后再继续。

#### 4.3 首次对话：验证一切就绪

所有准备工作完成后——Claude Code 已安装、认证已通过、`Git` 等依赖已就绪——是时候发送第一条消息，验证整个链路是否畅通。

进入任意项目目录（或直接在主目录），启动 Claude Code 并输入一句问候：

```
cd ~/my-project    # 进入任意项目目录
claude              # 启动 Claude Code
```

在对话界面中输入：

```
你好，好久不见
```

如果一切配置正确，Claude Code 会在几秒内返回一段自然语言回复——它会根据当前项目上下文做出响应，可能会简单介绍自己的能力，或者主动分析当前目录的项目结构。

![](img/d0ca677f7b14bbc6635c58c9d3bf630c.png)

看到回复，就说明以下环节全部打通：

+   Claude Code 安装正确，可执行文件在系统 `PATH` 中
+   网络代理配置正确，能连接到 Anthropic 服务器
+   身份认证有效（`OAuth Token` 或 `API Key`）
+   模型调用正常，`Claude` 能正确接收和返回消息

> **💡 提示：**如果长时间没有响应或报错，先运行 `claude doctor` 做一次诊断（前文已介绍）。最常见的问题是代理未正确配置——回到前面的"代理设置"章节检查 `https_proxy` 和 `http_proxy` 环境变量是否生效。

首次对话成功，环境搭建阶段正式完成。接下来了解 Claude Code 的计费方式，做到心中有数。

同时，双击 `CTRL+C` 即可退出对话。

退出对话时能看到当前对话 ID。而下次再次进入会话的话，需要在同一文件夹内输入 `resume` 命令：

```
claude --resume f06bc5d1-1523-4e1e-bead-9bf9f522953a
```

或者直接输入 `claude -c` 就能直接恢复上一次停止的会话。

![](img/659303d33ff306a298b15326b2c910f2.png)

#### 4.4 深入理解：会话存储与项目绑定机制

上面的 `-c` 和 `--resume` 操作看似简单，背后却隐藏着 Claude Code 最核心的设计哲学之一：会话与项目目录的绑定关系。理解这一机制，不仅能避免很多初学者常见的困惑（"我的对话怎么找不到了？"），更能帮你建立正确的 Claude Code 工作心智模型。

##### 操作层面：为什么必须在同一文件夹内恢复对话？

尝试一个实验：在 `~/project-A` 目录下与 Claude Code 进行了一次深入的代码重构对话，然后退出。接着 `cd ~/project-B`，再执行 `claude -c`——你会发现，刚才的对话完全消失了，`-c` 恢复的是 `project-B` 目录下的上一次会话（如果有的话），而不是你刚才在 `project-A` 中的那次。

这不是 Bug，而是刻意为之。Claude Code 本质上是一个项目级的开发助手，而非全局聊天工具。它的每一次对话都深度嵌入当前项目的上下文——读取了哪些文件、修改了哪些代码、创建了哪些 `Checkpoint`、遵循了哪些 `CLAUDE.md` 规范。如果允许在 `project-B` 中恢复 `project-A` 的会话，`Claude` 可能会按照 A 项目的规范去修改 B 项目的代码，后果不堪设想。

> **核心认知**：Claude Code 不是"通用对话工具碰巧能写代码"，而是"项目级 Agent 碰巧通过对话交互"。每个项目目录就是一个独立的工作空间，会话、记忆、配置三者在目录维度上完全隔离。

##### 底层机制：会话如何存储在本地？

打开 `~/.claude/projects/` 目录，你会看到 Claude Code 会话存储的全貌。每个项目目录的绝对路径会被编码为一个文件夹名——路径分隔符替换为连字符：

| **项目路径** | **编码后的文件夹名** |
| --- | --- |
| `/Users/alice/code/myapp`（macOS） | `-Users-alice-code-myapp` |
| `C:\Users\Bob\dev\project`（Windows） | `C--Users-Bob-dev-project` |

![](img/a11ac46dabc9a03a097b3c9de3eb58d5.png)

每个编码后的项目文件夹内部结构如下：

```
~/.claude/projects/-Users-alice-code-myapp/
├── a1b2c3d4-e5f6-7890-abcd-ef1234567890.jsonl   # 会话记录（每次对话一个文件）
├── f06bc5d1-1523-4e1e-bead-9bf9f522953a.jsonl   # 另一次会话的记录
├── sessions-index.json                           # 会话索引（元数据）
└── memory/
    └── MEMORY.md                                 # 项目级 Auto Memory
```

![](img/3aff31d51705b79854d61ecd5f8cc4e9.png)

其中，`.jsonl` 文件是会话的完整记录——每一行是一个 `JSON` 对象，记录了每一轮对话的消息内容、时间戳、`Token` 用量、工具调用结果、当前 `Git` 分支等信息。Claude Code 采用追加写入（append-only）模式，每产生一条消息就立即写入磁盘，即使进程崩溃也不会丢失已完成的对话内容。

![](img/c923cdd43741b3e13fcf4ed5a8c803ca.png)

`sessions-index.json` 则是一个轻量级索引文件，记录每个会话的元数据（首条提示词、摘要、消息数量、创建/修改时间、`Git` 分支等），供 `claude --resume` 的交互式选择器快速检索，无需逐一解析完整的 `JSONL` 文件。

> 注意，最新版 `sessions-index.json` 内容已经合并到历史对话记录 `json` 字段中。

理解了这套存储结构，几个常见问题就迎刃而解了：

| **问题** | **原因** |
| --- | --- |
| 移动项目文件夹后，`-c` 找不到之前的对话 | 路径变了，编码后的文件夹名也变了，Claude Code 认为这是一个新项目 |
| 不同电脑之间无法共享会话 | 会话存储在本地 `~/.claude/` 中，不会上传到云端 |
| 删除了 `~/.claude/projects/` 后所有历史记录消失 | 那就是会话数据的唯一存储位置 |

![](img/a991c6234d58971f8799a83c2e153540.png)

##### 延伸思考：Claude Code 能否"多开"？

理解了"会话绑定项目目录"的机制后，一个自然的问题浮现：能不能同时开多个 Claude Code 实例，并行处理不同任务？

**不同项目目录——完全可以多开。** 你可以在一个终端窗口中用 Claude Code 开发前端项目 `~/frontend`，同时在另一个窗口中开发后端项目 `~/backend`。两个实例的会话、记忆、`Checkpoint` 各自独立，互不干扰。这正是"项目级隔离"设计的优势——每个项目目录映射到 `~/.claude/projects/` 下不同的文件夹，数据天然隔离。
