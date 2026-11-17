
# Part 2：环境搭建 — 从空白服务器到就绪状态

## part2-02-环境搭建

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/G5Ldd3nyRoasTwxguKfcmpSHnXe](https://kq4b3vgg5b.feishu.cn/docx/G5Ldd3nyRoasTwxguKfcmpSHnXe)

> 地基不牢，地动山摇。本章带你在一台全新的 `A800` 服务器上，从零装好 `Node.js`、`npm` 和 `OpenClaw`，打好后续一切工作的基础。

### 引言

面前是一台全新的 `A800` 服务器——80GB 显存、1.1TB 数据盘，硬件火力拉满。但现在它只有一个光秃秃的终端。

这一章我们要把地基打好：装上 `Node.js`、配好 `npm` 国内镜像、安装 `OpenClaw` 并跑通首次诊断。

看似基础，但每一步都有坑——比如忘记激活 `nvm` 导致 `command not found`，比如 `npm` 不配镜像下载半天超时，比如 `OpenClaw` 的 `models` 配置必须是数组不能逐项 `set`。我已经替你踩过了，跟着走就行。

> **深入理解**：很多人觉得环境搭建是"体力活"，我不这么看。环境理解的深度，决定了你排错的速度。为什么 `Node.js` 装在系统盘没事但模型必须放数据盘？为什么 `nvm` 装完还要手动激活？为什么 `npm` 要配国内镜像？搞懂这些"为什么"，以后遇到新问题你就能举一反三。

**本章学习目标**：

+ 掌握通过 `SSH` 连接 `AutoDL` 服务器的方法

+ 理解 `AutoDL` 服务器的磁盘布局（系统盘 vs 数据盘）

+ 使用 `nvm` 安装并管理 `Node.js` 版本

+ 配置 `npm` 国内镜像加速包安装

+ 安装 `OpenClaw` 并完成初始配置

+ 将环境变量持久化到 `.bashrc`

---

### 2.1 SSH 连接服务器

#### 什么是 SSH？一句话理解

`SSH`（`Secure Shell`）是一种加密的远程登录协议。简单说，它让你在自己的电脑上输入命令，这些命令会被加密传输到远程服务器上执行——就像给服务器装了一条"安全的远程遥控线"。

你的电脑可能在家里或办公室，我们的 `AutoDL` 服务器在北京的数据中心。`SSH` 就是连接两者的桥梁。

#### 连接命令逐字拆解

打开你电脑上的终端（Mac 用"终端"应用，Windows 用 `PowerShell` 或 `Windows Terminal`），输入以下命令：

```
# SSH 连接 AutoDL 服务器
# 注意：端口号和服务器地址请替换为你自己的
ssh -p 40903 root@connect.bjb1.seetacloud.com
```

这条命令的每一个部分都有明确含义：

| 部分 | 含义 | 注意事项 |
| --- | --- | --- |
| `ssh` | 启动 `SSH` 客户端程序 | `Mac`/`Linux` 自带，`Windows 10+` 也内置了 |
| `-p 40903` | 指定端口号 | 每台 `AutoDL` 服务器的端口不同，请查看你自己实例的详情页 |
| `root` | 登录用户名 | `AutoDL` 默认使用 `root`（最高权限用户） |
| `@` | 分隔符 | 把用户名和服务器地址隔开 |
| `connect.bjb1.seetacloud.com` | 服务器地址 | `bjb1` 表示北京一区，你的可能是其他区域 |

> ⚠️ **重要**：本课程使用的端口 `40903` 和地址 `connect.bjb1.seetacloud.com` 是作者的服务器。你必须替换为你自己实例的连接信息——在 `AutoDL` 控制台的实例详情页可以找到。

**输入连接命令**：

按下回车，如果是第一次连接，系统会问你是否信任这台服务器（指纹验证），输入 `yes` 并回车：

连接成功后，你会看到 `Ubuntu` 的欢迎信息和命令提示符 `root@autodl:~#`：

> **看到 `root@autodl:~#`** 就表示你已经成功登录到服务器了。 从此刻起，你输入的所有命令都会在远程服务器上执行，而非你的本地电脑。这一点很重要——后面的所有操作（安装软件、下载模型、启动服务）都发生在远端服务器上。

> **重要提醒**：如果你用 `Windows` 且 `SSH` 报错，可能是 [`PowerShell`](https://mobaxterm.mobatek.net/) 版本太老。建议安装 `Windows Terminal`，或者使用 `MobaXterm` 等第三方 `SSH` 工具。

---

### 2.2 初始环境巡检

登录后，第一件事是"看看这台服务器有什么家底"。就像搬进新房子先检查水电煤气——我们检查三样东西：`GPU` 信息、磁盘空间、已运行的服务。

#### 查看 GPU 信息

```
# 查看 NVIDIA GPU 状态信息
nvidia-smi
```

`nvidia-smi` 是 `NVIDIA` 官方提供的 `GPU` 监控工具，全称 "NVIDIA System Management Interface"。它会显示显卡型号、显存使用情况、温度、正在运行的 `GPU` 进程等关键信息。

**输入命令**：

**输出结果**：

让我们读懂这个输出中的关键信息：

| 信息项 | 值 | 含义 | 对课程的影响 |
| --- | --- | --- | --- |
| `GPU Name` | `NVIDIA A800-SXM4-80GB` | 显卡型号 | `A800` 是 `A100` 的国内版，性能几乎相同 |
| `Memory-Usage` | `4613MiB / 81920MiB` | 已用 `4.6GB` / 总共 `80GB` | 已占 `4.6GB`（`TTS` 服务），还剩 ~`75GB` |
| `GPU-Util` | `0%` | `GPU` 当前空闲 | 没有在计算，好消息 |
| `Temp` | `27°C` | `GPU` 温度 | 很凉爽，满载会到 `60-70°C` |
| `Processes` | `python3` (`4604MiB`) | `TTS` 服务正在运行 | 占了 `4.6GB` 显存，后面可以共存 |

**怎么看这张表**：重点是第二行——`80GB` 显存中 `TTS` 服务 占了 `4.6GB`，还有 ~`75GB` 可用。后面跑 `Qwen3.5-27B` 只需要约 `20GB`（`Ollama` 量化版）或 `58GB`（`vLLM` 全精度版），空间完全充裕。

#### 查看磁盘空间和已运行服务

```
# 查看数据盘使用情况（-h 表示 human-readable，用 GB/TB 显示）
df -h /root/autodl-tmp

# 查看后台运行的 screen 会话
screen -ls
```

**关键信息**：

| 信息项 | 值 | 含义 |
| --- | --- | --- |
| 数据盘总容量 | `1.1TB` | 存模型和大文件的地方 |
| 数据盘可用 | ~`811GB` | 空间非常充足 |
| `tts_api` 会话 | 运行中 | `TTS` 语音合成服务在后台跑着 |

#### 重要概念：系统盘 vs 数据盘

`AutoDL` 服务器有两块磁盘，这个概念非常重要，贯穿整个课程，决定了你"往哪里装东西"：

|  | 系统盘 `/` | 数据盘 `/root/autodl-tmp` |
| --- | --- | --- |
| **容量** | ~`30GB` | **`1.1TB`** |
| **用途** | 操作系统、系统软件 | **模型、数据、大文件** |
| **空间** | 有限，**不要乱放** | **充足，随便用** |
| **持久性** | 重启保留 | 重启保留 |
| **典型内容** | `Ubuntu` 系统、`Node.js`、`OpenClaw` | `Ollama` 模型权重、`vLLM` 环境、`HuggingFace` 模型 |

> ⚠️ **核心原则**：所有大于 `1GB` 的文件（模型权重、虚拟环境、数据集等）必须放到 `/root/autodl-tmp` 数据盘。系统盘只有 `30GB`，一个 `17GB` 的模型就能把它撑爆——这是很多新手必踩的坑。

> **特别注意**：我见过不少学员因为忘记配置模型存储路径，导致 `17GB` 的模型下载到系统盘，系统盘写满后整个服务器变得极度卡顿，`SSH` 都连不上。后面 `Part 3` 安装 `Ollama` 时我会重点强调这个问题。

---

### 2.3 安装 `nvm`（`Node.js` 版本管理器）

#### 为什么不直接装 `Node.js`，要先装 `nvm`？

你可能会想：直接 `apt install nodejs` 不就行了？

不行。三个原因：

| # | 原因 | 详细说明 |
| --- | --- | --- |
| 1 | **`apt` 版本太老** | `Ubuntu 22.04` 默认的 `Node.js` 是 `v12`，`OpenClaw` 要求 ≥ `v22`——差了 `10` 个大版本 |
| 2 | **多版本管理** | 未来你可能需要不同版本的 `Node.js` 运行不同项目，`nvm` 可以一键切换 |
| 3 | **不污染系统** | `nvm` 把 `Node.js` 装在用户目录下（`~/.nvm/`），卸载也干干净净 |

**类比理解**：`nvm` 就像手机上的"应用商店"——你可以通过它浏览、安装、卸载、切换不同版本的 `Node.js`，比手动下载安装方便得多。

#### 安装 `nvm`

执行以下命令：

```
# 下载并执行 nvm 官方安装脚本
# nvm 是 GitHub 上 8 万星标的知名开源项目
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

逐段解释这条命令：

| 部分 | 含义 | 补充说明 |
| --- | --- | --- |
| `curl` | 命令行下载工具 | 从网络获取文件内容 |
| `-o-` | 输出到标准输出（屏幕） | 不保存为文件，直接传给 `bash` |
| `https://raw.githubusercontent.com/...` | `nvm` 官方安装脚本地址 | `GitHub` 上的开源仓库 |
| `\|` | 管道符 | 把前一个命令的输出传给后一个命令 |
| `bash` | 用 `bash` 解释器执行 | 执行下载的安装脚本 |

> **安全说明**：从网络下载脚本并直接执行（`curl \| bash` 模式），通常需要谨慎。但 `nvm` 是 `GitHub` 上 8 万星标的知名项目，安装脚本经过社区审查，可以信任。

**输入安装命令**：

按下回车，`curl` 开始下载安装脚本，`nvm` 开始克隆仓库到本地：

安装完成后，会看到提示信息：

#### 激活 `nvm`（关键步骤，最易遗漏！）

`nvm` 安装完成后，**不能直接使用**——这是很多新手卡住的第一个坑。你需要先执行两行命令来"激活"它：

```
# 设置 nvm 安装目录的环境变量
export NVM_DIR="$HOME/.nvm";
# 如果 nvm.sh 文件存在，则加载它
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh";
```

这两行做了什么？

| 命令 | 含义 | 通俗理解 |
| --- | --- | --- |
| `export NVM_DIR="$HOME/.nvm"` | 告诉系统 `nvm` 安装在哪里 | `$HOME = /root`，即 `/root/.nvm` |
| `[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"` | 检查 `nvm.sh` 是否存在，存在就加载 | "如果 `nvm` 的脚本在，就把它的功能导入到当前终端" |

激活后，验证 `nvm` 是否正常工作：

```
# 验证 nvm 版本
nvm --version
# 预期输出：0.40.1
```

看到 `0.40.1` 就说明 `nvm` 安装成功并已激活。

> ⚠️ **常见问题**：如果忘记激活直接使用 `nvm` 命令，会报 `nvm: command not found`。不用慌——执行上面那两行激活命令就好了。后面我们会把它写入 `.bashrc`，这样每次登录都会自动激活，不用手动操作。

---

### 2.4 安装 `Node.js 22 LTS`

#### 为什么选 `Node.js 22` 而非 `24`？

`OpenClaw` 官方要求 `Node.js` ≥ `22`，文档推荐 `Node 24`。那我们为什么选 `22`？

| 版本 | 类型 | 稳定性 | 维护周期 | 推荐场景 |
| --- | --- | --- | --- | --- |
| `Node.js 22` | **`LTS`**（长期支持） | ⭐⭐⭐⭐⭐ | 维护到 `2027` 年 | **教学、生产环境** |
| `Node.js 24` | `Current`（当前版） | ⭐⭐⭐ | 仅 `8` 个月 | 尝鲜新功能 |

**`LTS`** = `Long Term Support`（长期支持版），意味着 `Node.js` 官方会持续修 `Bug`、打安全补丁。教学场景下，稳定性比新功能更重要，所以我们选 `Node.js 22 LTS`。

> **实用建议**：生产环境也建议用 `LTS` 版本。"能用的新功能"远没有"不会出的 `Bug`"重要。

#### 安装 `Node.js`

一行命令搞定：

```
# 用 nvm 安装 Node.js 22 的最新 LTS 版本
nvm install 22
```

`nvm` 会自动下载并安装 `Node.js` 22 的最新版本（目前是 `v22.22.1`），同时自带 `npm` 包管理器。

**输入安装命令**：

`nvm` 开始从 `Node.js` 官网下载二进制文件：

下载完成、校验通过后，`nvm` 自动安装并设为默认版本：

**关键输出解读**：

| 输出内容 | 含义 |
| --- | --- |
| `Checksums matched!` | 下载文件完整性校验通过，没有损坏或被篡改 |
| `Now using node v22.22.1 (npm v10.9.4)` | 当前正在使用这个版本 |
| `Creating default alias: default -> 22` | 把 `22` 设为默认版本，下次登录自动使用 |

#### 验证安装

运行三个验证命令，确保一切正常：

```
# 查看 Node.js 版本
node --version    # 预期输出：v22.22.1

# 查看 npm 版本
npm --version     # 预期输出：10.9.4

# 查看 Node.js 安装位置
which node        # 预期输出：/root/.nvm/versions/node/v22.22.1/bin/node
```

| 命令 | 作用 | 预期输出 | 说明 |
| --- | --- | --- | --- |
| `node --version` | 查看 `Node.js` 版本 | `v22.22.1` | 主角软件的版本 |
| `npm --version` | 查看 `npm` 版本 | `10.9.4` | `Node.js` 的包管理器 |
| `which node` | `Node.js` 安装在哪 | `/root/.nvm/versions/node/v22.22.1/bin/node` | 在系统盘的 `nvm` 目录下 |

> **关于安装位置**：`nvm` 把 `Node.js` 装在 `/root/.nvm/` 下（系统盘），占用约 `100MB`。对系统盘来说可以接受——别忘了我们说的"大于 `1GB` 才需要放数据盘"。

---

### 2.5 配置 `npm` 淘宝镜像

#### 为什么要配镜像？

`npm` 默认从国外的 `registry.npmjs.org` 下载包。在国内服务器上，这个速度非常不稳定——轻则几十 `KB/s` 慢如蜗牛，重则直接超时失败。

**淘宝镜像**（`npmmirror.com`）是国内最大的 `npm` 镜像，由 `阿里巴巴` 维护，与 `npm` 官方仓库实时同步。配置后，下载速度从几十 `KB/s` 飙升到几十 `MB/s`。

|  | 官方源 | 淘宝镜像 |
| --- | --- | --- |
| 地址 | `registry.npmjs.org` | `registry.npmmirror.com` |
| 服务器位置 | 美国 | 中国 |
| 下载速度（国内） | 极慢或超时 | **几十 `MB/s`** |
| 包同步延迟 | — | 约 `10` 分钟 |
| 维护方 | `npm` 官方 | `阿里巴巴` |

#### 配置命令

```
# 将 npm 的包下载源切换为淘宝镜像
npm config set registry https://registry.npmmirror.com

# 验证配置是否生效
npm config get registry
# 预期输出：https://registry.npmmirror.com/
```

第一条命令告诉 `npm`："以后下载包的时候，不要去国外的 `registry.npmjs.org`，改用国内的 `registry.npmmirror.com`"。

**输入配置命令**：

**配置并验证**：

看到输出 `https://registry.npmmirror.com/` 就表示配置成功。后面用 `npm` 安装任何包，都会走国内镜像，速度起飞。

> **重要提醒**：如果你在公司内网且公司有自己的 `npm` 镜像（如 `Nexus`、`Verdaccio`），应使用公司的镜像地址。淘宝镜像是公共服务，适合个人学习和一般场景。

---

### 2.6 安装 `OpenClaw`

万事俱备，现在来安装我们的主角——`OpenClaw`。

#### 安装命令及每个参数的含义

```
# 全局安装 OpenClaw 最新版本
npm install -g openclaw@latest
```

逐项解释——这里的每个参数都有用：

| 参数 | 含义 | 详细说明 |
| --- | --- | --- |
| `npm install` | `npm` 的安装命令 | 从 `npm` 仓库下载并安装指定的包 |



## 2.6 OpenClaw 安装

| `-g` | **全局安装** | 安装到全局目录后，`openclaw` 命令在**任何路径**都能使用。不加 `-g` 则只安装到当前目录 |

| `openclaw` | 包名 | `npm` 仓库中 OpenClaw 的包名称 |

| `@latest` | 版本标签 | 安装最新发布版。也可以指定版本号如 `@2026.3.13` |

**输入安装命令**：

`npm` 开始下载 OpenClaw 及其所有依赖包：

安装完成：

**输出解读**：

| 信息 | 含义 | 是否需要担心 |

| --- | --- | --- |

| `` `npm warn deprecated node-domexception@1.0.0` `` | 一个依赖包标记为过期 | ❌ 不影响使用 |

| `` `added 539 packages in 21s` `` | 安装了 539 个包，耗时 21 秒 | ✅ 得益于淘宝镜像，速度很快 |

| `` `89 packages are looking for funding` `` | 89 个包的作者希望获得赞助 | ❌ 可忽略 |

> **技术洞察**：21 秒装完 539 个包——这就是配了淘宝镜像的效果。如果没配镜像，同样的操作可能要 5-10 分钟甚至超时失败。所以不要跳过 2.5 节。

### 验证安装

```
# 查看 OpenClaw 版本
openclaw --version
# 预期输出：OpenClaw 2026.3.13 (61d171a)

# 查看安装位置
which openclaw
# 预期输出：/root/.nvm/versions/node/v22.22.1/bin/openclaw
```

**版本信息解读**：

| 信息 | 含义 |

| --- | --- |

| `OpenClaw 2026.3.13` | 版本号——2026 年 3 月 13 日发布 |

| `(61d171a)` | Git 提交哈希——精确定位到源码版本 |

| 安装路径 | 在 `nvm` 管理的 `Node.js` 目录下，因为是通过 `npm` 全局安装的 |

> ⚠️ **可能遇到的问题及解决方案**：

| 问题 | 原因 | 解决方案 |

| --- | --- | --- |

| `` `openclaw: command not found` `` | `npm` 全局 `bin` 目录不在 `PATH` 中 | `` `export PATH="$(npm prefix -g)/bin:$PATH"` `` |

| `sharp` 构建失败 | C 编译器或 `libvips` 问题 | `` `SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install -g openclaw@latest` `` |

| 安装速度极慢 | 没有配淘宝镜像 | 回到 2.5 节配置镜像后重试 |

| `` `EACCES permission denied` `` | 权限不足 | AutoDL 默认是 root，不应出现此问题。如果不是 root 用户，加 `sudo` |

---

## 2.7 OpenClaw 初始配置

### 运行 Doctor 诊断

OpenClaw 自带一个"医生"命令，可以检查当前环境是否就绪：

```
# 运行 OpenClaw 环境诊断
openclaw doctor
```

**输入诊断命令**：

**诊断结果**：

Doctor 会列出所有检查项和状态。常见的输出如下：

| 检查项 | 状态 | 严重程度 | 说明 |

| --- | --- | --- | --- |

| `Node.js version` | ✅ PASS | — | v22.22.1，满足 ≥ 22 的要求 |

| `npm version` | ✅ PASS | — | v10.9.4 |

| Gateway auth is off | ⚠️ WARNING | 低 | 认证未开启，教学环境可暂不处理 |

| Session store dir missing | ❌ CRITICAL | 中 | 会话存储目录缺失，首次启动时会自动创建 |

| Gateway not running | ℹ️ INFO | 低 | 网关还没启动，这是预期的 |

| No model configured | ❌ CRITICAL | 高 | 还没配置模型，Part 3 解决 |

> **特别注意**：不要被诊断结果中的 CRITICAL 吓到。这些问题都是"预期中的"——我们刚安装完 OpenClaw，还没配置模型当然会报 `No model configured`。后面逐步配置，每个问题都会被解决。

### 配置 Gateway 模式

目前最重要的一步是设置 Gateway 模式为 `local`（本地模式）：

```
# 设置 Gateway 为本地模式
# local 模式下 Gateway 只监听 127.0.0.1，不对外暴露
openclaw config set gateway.mode local
```

| 参数 | 含义 | 说明 |

| --- | --- | --- |

| `` `openclaw config set` `` | 修改 OpenClaw 配置 | 写入到 `~/.openclaw/openclaw.json` |

| `gateway.mode` | 配置项名称 | Gateway 的运行模式 |

| `local` | 值 | 只监听本机（`127.0.0.1`），安全的默认选择 |

### OpenClaw 关键路径说明

OpenClaw 安装后，会在你的 home 目录创建一个 `.openclaw/` 文件夹，存放所有配置和数据：

| 路径 | 用途 | 你需要关心吗 |

| --- | --- | --- |

| `~/.openclaw/` | OpenClaw 主目录 | 所有配置文件的根目录 |

| `~/.openclaw/openclaw.json` | 主配置文件（JSON5 格式，支持注释） | ✅ 最重要！模型配置、Agent 配置都在这里 |

| `~/.openclaw/credentials/` | 凭证存储（API Key 等） | ⚠️ 含敏感信息，不要泄露 |

| `~/.openclaw/sessions/` | 对话历史 | 一般不需要手动操作 |

| `~/.openclaw/memory/` | Agent 的长期记忆 | Agent 自动管理 |

| `~/.openclaw/agents/` | Agent 配置和状态 | 高级自定义时会用到 |

**怎么看这张表**：重点记住第二行——`openclaw.json` 是整个 OpenClaw 的"大脑配置中心"。后面 Part 3-6 的核心操作都是围绕这个文件展开的：配 Ollama、配 vLLM、配 DeepSeek、配飞书——全部改这一个文件。

> **推荐做法**：建议用 `nano` 或 `vim` 编辑 `openclaw.json`，不要用 `echo >` 重定向覆盖——万一少个括号，整个配置就废了。`nano` 对新手更友好：`nano ~/.openclaw/openclaw.json`，编辑完 Ctrl+O 保存，Ctrl+X 退出。

---

## 2.8 环境持久化

### 为什么要写入 `.bashrc`？

我们前面设置了 `nvm`、Node.js、环境变量，但这些设置只在当前终端会话中有效。如果你断开 SSH 重新连接——

| 操作 | 会发生什么 | 影响 |

| --- | --- | --- |

| `nvm --version` | `command not found` | `nvm` 不可用 |

| `node --version` | `command not found` | Node.js 也不可用 |

| `openclaw --version` | `command not found` | OpenClaw 更不可用 |

| 启动 Ollama | 模型存错路径 | 系统盘可能被撑爆 |

`.bashrc` 是 Linux 的一个特殊配置文件——每次你通过 SSH 登录或打开新终端时，它会被自动执行。把环境变量写入 `.bashrc`，就等于"让系统记住这些设置"。

### 写入 .bashrc

执行以下命令，把所有需要持久化的配置一次性写入：

```
# 将环境变量追加到 .bashrc（不会覆盖已有内容）
cat >> ~/.bashrc << 'EOF'

# === OpenClaw 课程环境 ===
# nvm（Node.js 版本管理器）
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"

# Ollama 模型存储路径（指向数据盘，避免撑爆系统盘）
export OLLAMA_MODELS=/root/autodl-tmp/ollama-models

# Python（miniconda）
export PATH=/root/miniconda3/bin:$PATH
EOF
```

**命令逐行解释**：

| 行 | 含义 | 为什么需要 |

| --- | --- | --- |

| `` `cat >> ~/.bashrc << 'EOF'` `` | 追加内容到 `.bashrc`（`>>` 是追加，`>` 是覆盖） | `>>` 不会删除已有配置 |

| `` `export NVM_DIR="$HOME/.nvm"` `` | 设置 `nvm` 安装目录 | `nvm` 需要这个变量才能工作 |

| `` `[ -s "$NVM_DIR/nvm.sh" ] && ...` `` | 加载 `nvm` 功能 | 每次登录自动激活 `nvm` |

| `` `export OLLAMA_MODELS=...` `` | Ollama 模型存储到数据盘 | 防止模型下载到系统盘撑爆磁盘 |

| `` `export PATH=/root/miniconda3/bin:$PATH` `` | 让系统找到 Python | `vLLM`（Part 4）需要 Python 环境 |

**写入命令**：

### 验证持久化

```
# 让 .bashrc 立即生效（不用重新登录）
source ~/.bashrc

# 验证所有关键命令
node --version          # 预期：v22.22.1
openclaw --version      # 预期：OpenClaw 2026.3.13 (61d171a)
echo $OLLAMA_MODELS     # 预期：/root/autodl-tmp/ollama-models
```

所有命令都正常输出，表示持久化配置成功。

> **最终验证（可选但推荐）**：断开 SSH 连接（输入 `exit`），然后重新连接服务器。重连后直接运行 `node --version` 和 `openclaw --version`，如果都正常输出版本号，说明环境持久化完全成功——以后再也不用手动设置了。

---

## 2.9 本章环境清单一览

在进入下一章之前，确认你的服务器上已经具备以下全部环境：

| 检查项 | 验证命令 | 预期输出 | 状态 |

| --- | --- | --- | --- |

| SSH 连接正常 | 能看到 `root@autodl:~#` | 命令提示符 | ☐ |

| GPU 可用 | `nvidia-smi` | NVIDIA A800-SXM4-80GB | ☐ |

| 数据盘可用 | `df -h /root/autodl-tmp` | ~1.1TB | ☐ |

| `nvm` 已安装 | `nvm --version` | 0.40.1 | ☐ |

| `Node.js` 已安装 | `node --version` | v22.22.1 | ☐ |

| `npm` 已安装 | `npm --version` | 10.9.4 | ☐ |

| `npm` 镜像已配置 | `npm config get registry` | `https://registry.npmmirror.com/` | ☐ |

| `OpenClaw` 已安装 | `openclaw --version` | OpenClaw 2026.3.13 | ☐ |

| Gateway 模式已设置 | `openclaw config get gateway.mode` | `local` | ☐ |

| `.bashrc` 已持久化 | `echo $OLLAMA_MODELS` | `/root/autodl-tmp/ollama-models` | ☐ |

> **实用建议**：把这张表当作检查清单，逐项打勾。如果某一项不通过，回到对应小节排查。全部通过后再进入 Part 3，否则后面的步骤可能会在意想不到的地方出错。

---

## 本章小结

让我们回顾这一章完成的全部工作：

| 步骤 | 完成情况 | 关键结果 | 耗时参考 |

| --- | --- | --- | --- |

| SSH 连接 | ✅ | 成功登录 AutoDL A800 服务器 | 1 分钟 |

| 环境巡检 | ✅ | GPU 80GB、数据盘 811GB 可用 | 2 分钟 |

| `nvm` 安装 | ✅ | v0.40.1 | 3 分钟 |

| `Node.js` 安装 | ✅ | v22.22.1（LTS） | 2 分钟 |

| `npm` 镜像配置 | ✅ | 淘宝镜像 `npmmirror.com` | 1 分钟 |

| OpenClaw 安装 | ✅ | v2026.3.13（539 packages） | 2 分钟 |

| OpenClaw 配置 | ✅ | Gateway mode = `local` | 1 分钟 |

| 环境持久化 | ✅ | `.bashrc` 写入完成 | 2 分钟 |

**地基打好了！** 你现在有了 `Node.js` 22、`npm` 淘宝镜像、`OpenClaw` 2026.3.13——万事俱备，只欠"大脑"。

下一章，我们给 Agent 装上第一颗"大脑"：用 Ollama 在本地跑一个 270 亿参数 的 Qwen3.5 大模型。听起来很厉害？实际操作只需要两行命令。

> **延伸思考**：环境搭建这一章看起来"基础"，但里面的每一个知识点——SSH 端口、`nvm` 版本管理、`npm` 镜像加速、系统盘 vs 数据盘、环境变量持久化——在后面都会反复出现。如果你对某个步骤的"为什么"还有疑问，现在是回头复习的最佳时机。
