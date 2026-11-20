
# 完整卸载 OpenClaw（Windows/macOS/Linux/npm/pnpm）

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/T2JqdvVSNoEN2RxAPzicQ8B9nQs](https://qqs7y1hozd1.feishu.cn/docx/T2JqdvVSNoEN2RxAPzicQ8B9nQs)

平台支持：🪟 Windows PowerShell · ⌨️ Windows CMD · 🍎 macOS / Linux · 📦 npm · ⚡ pnpm

![](img/5a86273bac5112db05cada7dcb3f8e3b.png)

---

## 卸载前的准备工作

在开始卸载之前，建议先做几件事情，确保卸载后不留残余文件。

**步骤 1 — 停止正在运行的 OpenClaw 守护进程（daemon）**

如果你使用了 Gateway 服务，先手动停止它，否则可能有残留进程占用端口或文件。

```
# 停止守护进程
openclaw daemon stop

# 或者停止 Gateway 服务
openclaw gateway stop
```

**步骤 2 — （可选）备份配置文件**

如果你不确定是否需要保留已有配置，建议先备份：

+   macOS / Linux：`~/.openclaw/`
+   Windows：`%USERPROFILE%.openclaw`

⚠️ 注意：卸载 OpenClaw 本体不会自动删除你的配置目录（`~/.openclaw/`），如需彻底清除，需要手动删除该目录。

## Windows — PowerShell 安装的卸载方法

**安装命令：** `iwr -useb https://openclaw.ai/install.ps1 | iex`

PowerShell 脚本通过 `npm install -g openclaw` 全局安装 OpenClaw，并将其写入 Windows 的用户 PATH（通常是 `%APPDATA%\npm`）。

**步骤 1 — 以管理员身份打开 PowerShell，执行 npm 卸载命令**

```
npm uninstall -g openclaw
```

**步骤 2 — 验证卸载成功**

```
Get-Command openclaw -ErrorAction SilentlyContinue
# 如果没有任何输出，说明已卸载成功
```

**步骤 3 — （彻底清除）删除配置目录与残留文件**

```
# 删除配置目录
Remove-Item -Recurse -Force "$env:USERPROFILE.openclaw"

# 如果有遗留的 git 安装（~/openclaw 目录），一并删除
Remove-Item -Recurse -Force "$env:USERPROFILE\openclaw"

# 删除 .local\bin 中的包装脚本（git 方式安装时创建）
Remove-Item -Force "$env:USERPROFILE.local\bin\openclaw.cmd" -ErrorAction SilentlyContinue
```

💡 如果 PowerShell 安装时使用了 `-InstallMethod git` 参数，还需额外删除 `$env:USERPROFILE\openclaw`（git 克隆目录）和 `$env:USERPROFILE\.local\bin\openclaw.cmd`（包装脚本）。

## Windows — CMD 安装的卸载方法

**安装命令：** `curl -fsSL https://openclaw.ai/install.cmd -o install.cmd && install.cmd && del install.cmd`

CMD 脚本的底层安装机制与 PowerShell 脚本相同，最终都是通过 `npm install -g openclaw` 完成安装，因此卸载方式一致。

步骤 1 — 打开命令提示符（CMD），执行卸载命令

```
npm uninstall -g openclaw
```

**步骤 2 — 验证卸载结果**

```
where openclaw
:: 若显示"无法找到文件"则卸载成功
```

步骤 3 — （彻底清除）删除配置与遗留文件

```
:: 删除配置目录
rmdir /s /q "%USERPROFILE%.openclaw"

:: 删除 git 源码目录（如果存在）
rmdir /s /q "%USERPROFILE%\openclaw"

:: 删除 .local\bin 包装脚本
del /f "%USERPROFILE%.local\bin\openclaw.cmd"
```

✅ CMD 与 PowerShell 安装的卸载核心命令完全相同，区别只在于删除残留文件时 CMD 使用 `rmdir /s /q`，而 PowerShell 使用 `Remove-Item -Recurse -Force`。

## macOS / Linux 安装的卸载方法

**安装命令：** `curl -fsSL https://openclaw.ai/install.sh | bash`

安装脚本默认使用 `npm install -g openclaw`（npm 方式），也可以以 git 源码方式安装（`--install-method git`）。两种方式的卸载步骤略有不同。

### A. 默认 npm 安装方式卸载

步骤 1 — 卸载 npm 全局包

```
npm uninstall -g openclaw
```

步骤 2 — 确认命令已从 PATH 移除

```
which openclaw
# 应该没有任何输出，或提示"not found"
```

步骤 3 — （可选）删除遗留的 npm 包目录和符号链接

```
# 找到 npm 全局包根目录
npm root -g

# 手动删除残留目录（通常 npm uninstall 已自动处理）
rm -rf "$(npm root -g)/openclaw"

# 删除可能遗留的 Homebrew 软链接（macOS）
rm -f /opt/homebrew/bin/openclaw /usr/local/bin/openclaw
```

步骤 4 — （彻底清除）删除配置目录

```
# 主配置目录
rm -rf ~/.openclaw

# 兼容旧版本遗留目录（如有）
rm -rf ~/.clawdbot ~/.moltbot ~/.moldbot
```

### B. git 源码安装方式卸载（`--install-method git`）

使用 `--install-method git` 安装时，脚本会将 OpenClaw 克隆到 `~/openclaw`，并在 `~/.local/bin/openclaw` 创建包装脚本。

**步骤 1 — 删除包装脚本**

```
rm -f ~/.local/bin/openclaw
```

**步骤 2 — 删除源码目录（默认路径为 `~/openclaw`）**

```
rm -rf ~/openclaw

# 如果你使用了自定义路径，替换为你当时指定的目录
# 例如：rm -rf ~/my-openclaw-src
```

**步骤 3 — （可选）清理 shell 配置文件中的 PATH 配置**

安装脚本可能向 `~/.bashrc` 或 `~/.zshrc` 追加了以下行，可手动打开文件删除：

```
# 以下两行如存在则删除
export PATH="$HOME/.local/bin:$PATH"
export PATH="$HOME/.npm-global/bin:$PATH"
```

然后重新加载配置：

```
source ~/.bashrc   # bash 用户
source ~/.zshrc    # zsh 用户
```

**步骤 4 — 删除配置目录**

```
rm -rf ~/.openclaw
```

⚠️ 安装脚本在某些情况下会向 `~/.bashrc` / `~/.zshrc` 写入 PATH 配置行（如 Linux 上配置 `~/.npm-global`）。如需彻底清除，记得检查并手动删除这些行，然后执行 `source ~/.bashrc`（或 `source ~/.zshrc`）使改动生效。

## npm 全局安装的卸载方法

**安装命令：** `npm i -g openclaw`，然后 `openclaw onboard`

这是最简洁的安装方式，卸载也同样简单，只需一条命令。

步骤 1 — 执行 npm 全局卸载

```
npm uninstall -g openclaw
```

**步骤 2 — 验证卸载结果**

```
# macOS / Linux
which openclaw          # 应无输出
npm list -g openclaw --depth=0   # 应显示 "(empty)" 或报错
```

```
# Windows
where openclaw   # 应提示"无法找到"
```

步骤 3 — （彻底清除）删除配置目录

```
# macOS / Linux
rm -rf ~/.openclaw
```

```
# Windows PowerShell
Remove-Item -Recurse -Force "$env:USERPROFILE.openclaw"
```

✅ npm 全局安装是五种方式里卸载最干净的：`npm uninstall -g openclaw` 会自动移除二进制文件、符号链接和包目录，无需手动清理 `node_modules`。

## pnpm 全局安装的卸载方法

**安装命令：** `pnpm add -g openclaw`，然后 `openclaw onboard`

pnpm 全局安装将 OpenClaw 写入 pnpm 的全局存储目录（通常是 `~/.local/share/pnpm`，或通过 `pnpm root -g` 查看）。

步骤 1 — 执行 pnpm 全局卸载

```
pnpm remove -g openclaw

# 或者使用 rm 别名
pnpm rm -g openclaw
```

**步骤 2 — 验证卸载结果**

```
# macOS / Linux
which openclaw    # 应无输出
pnpm list -g      # openclaw 应不在列表中
```

```
# Windows PowerShell
pnpm list -g
# openclaw 应不在列表中
```

步骤 3 — （可选）清理 pnpm 全局存储中的残留缓存

```
# 查看 pnpm 全局包根目录
pnpm root -g

# 手动清理（通常 pnpm remove 已处理）
rm -rf "$(pnpm root -g)/openclaw"

# 清理 pnpm 全局 bin（如有残留链接）
rm -f "$(dirname $(pnpm root -g))/bin/openclaw"
```

步骤 4 — （彻底清除）删除配置目录

```
# macOS / Linux
rm -rf ~/.openclaw
```

```
# Windows PowerShell
Remove-Item -Recurse -Force "$env:USERPROFILE.openclaw"
```

💡 pnpm 的全局 bin 目录通常是 `~/.local/share/pnpm`（Linux）或 `~/Library/pnpm`（macOS）。如果 `pnpm remove -g` 后 `openclaw` 命令仍然存在，可以手动检查并删除该目录下的 `openclaw` 链接文件。

## 卸载方式汇总对照表
