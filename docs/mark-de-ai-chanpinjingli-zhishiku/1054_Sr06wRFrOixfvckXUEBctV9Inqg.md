
# Hermes Agent更新与卸载

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/FTHwduzhKoAYnqx05NDcCmUanSg](https://qqs7y1hozd1.feishu.cn/docx/FTHwduzhKoAYnqx05NDcCmUanSg)

## 正在更新

使用单个命令更新到最新版本：

```
hermes update
```

此操作会拉取最新代码、更新依赖项，并提示您配置自上次更新以来添加的任何新选项。

提示

`hermes update`会自动检测新的配置选项，并提示您添加这些选项。如果您跳过了该提示，可以手动运行以查看缺失的选项，然后以交互方式添加它们。`hermes config check` `hermes config migrate`

### 更新期间会发生什么

当你运行`hermes update`时，会发生以下步骤：

1. `git pull` — 从分支拉取最新代码并更新子模块

1. 依赖安装 — 运行`uv pip install -e ".[all]"`以获取新的或更改的依赖项

1. 配置迁移 — 检测自您的版本以来添加的新配置选项，并提示您进行设置

1. 网关自动重启 — 如果网关服务正在运行（Linux 上的 systemd，macOS 上的 launchd），则在更新完成后自动重启，以便新代码立即生效

预期输出如下：

```
$ hermes update
Updating Hermes Agent...
📥 Pulling latest code...
Already up to date.  (or: Updating abc1234..def5678)
📦 Updating dependencies...
✅ Dependencies updated
🔍 Checking for new config options...
✅ Config is up to date  (or: Found 2 new options — running migration...)
🔄 Restarting gateway service...
✅ Gateway restarted
✅ Hermes Agent updated successfully!
```

### 建议的更新后验证

`hermes`更新处理主要更新路径，但快速验证确认所有内容都已顺利完成：

1. `git status --short` — 如果工作树意外地处于脏状态，请在继续操作前进行检查

1. `hermes doctor` — 检查配置、依赖项和服务健康状况

1. `hermes --version` — 确认版本按预期升级

1. 如果您使用网关：`hermes网关状态`

1. 如果报告`npm`审计问题：在标记的目录中运行`npm audit fix`

更新后工作树有未提交的更改

如果在更新后显示意外更改，请在继续之前停止并检查它们。这通常意味着本地修改在更新后的代码之上被重新应用，或者依赖步骤刷新了锁定文件。`git status --short` `hermes update`

### 正在检查您的当前版本

```
hermes version
```

与[GitHub发布页面](https://github.com/NousResearch/hermes-agent/releases)上的最新版本进行比较，或检查可用更新：

```
hermes update --check
```

### 从消息平台更新

您也可以通过发送以下内容直接从 Telegram、Discord、Slack 或 WhatsApp 进行更新：

```
/update
```

此操作将拉取最新代码、更新依赖项并重启网关。重启期间，机器人将短暂离线（通常为5 - 15秒），随后恢复运行。

### 手动更新

如果您是手动安装（而非通过快速安装程序）：

```
cd /path/to/hermes-agent
export VIRTUAL_ENV="$(pwd)/venv"

# Pull latest code and submodules
git pull origin main
git submodule update --init --recursive

# Reinstall (picks up new dependencies)
uv pip install -e ".[all]"
uv pip install -e "./tinker-atropos"

# Check for new config options
hermes config check
hermes config migrate   # Interactively add any missing options
```

### 回滚说明

如果更新引入问题，你可以回滚到之前的版本：

```
cd /path/to/hermes-agent

# List recent versions
git log --oneline -10

# Roll back to a specific commit
git checkout <commit-hash>
git submodule update --init --recursive
uv pip install -e ".[all]"

# Restart the gateway if running
hermes gateway restart
```

要回滚到特定的发布标签：

```
git checkout v0.6.0
git submodule update --init --recursive
uv pip install -e ".[all]"
```

警告

回滚可能会导致配置不兼容，如果添加了新选项。回滚后运行，遇到错误时移除未识别的选项。`hermes config check` `config.yaml`

### 给Nix用户的提示

如果你是通过 Nix Flake 安装的，更新会通过 Nix 包管理器来管理：

```
# Update the flake input
nix flake update hermes-agent

# Or rebuild with the latest
nix profile upgrade hermes-agent
```

Nix安装是不可变的——回滚由Nix的生成系统处理：

```
nix profile rollback
```

更多详情请参阅[Nix设置](https://hermes-agent.nousresearch.com/docs/getting-started/nix-setup)。

---

## 正在卸载

```
hermes uninstall
```

卸载程序提供了保留配置文件（`~/.hermes/`）以便将来重新安装的选项。

### 手动卸载

```
rm -f ~/.local/bin/hermes
rm -rf /path/to/hermes-agent
rm -rf ~/.hermes            # Optional — keep if you plan to reinstall
```

信息

如果您将网关作为系统服务安装，请先停止并禁用它：

```
hermes gateway stop
# Linux: systemctl --user disable hermes-gateway
# macOS: launchctl remove ai.hermes.gateway
```
