# 第二章：Hermes Agent安装

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/WiAXd5IOJoVpn1xGcz7csMbpned](https://qqs7y1hozd1.feishu.cn/docx/WiAXd5IOJoVpn1xGcz7csMbpned)

# 安装

使用单行安装程序在两分钟内启动并运行 Hermes Agent，或按照手动步骤进行完全控制。

## 快速安装

### Linux / macOS / WSL2

```
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

### 安卓 / Termux

现在，Hermes也提供了一个支持Termux的安装路径：

```
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

安装程序会自动检测Termux并切换到经过测试的安卓流程：

+   使用 Termux 软件包来处理系统依赖项（git、python、nodejs、ripgrep、ffmpeg、构建工具）

+   创建虚拟环境与python-m venv

+   自动导出 ANDROID_API_LEVEL 用于 Android wheel 构建

+   安装一个精心挑选的[termux]额外组件，使用pip

+   默认跳过未经测试的浏览器/WhatsApp引导程序

[如果您需要完整的](https://hermes-agent.nousresearch.com/docs/getting-started/termux)详细路径，请遵循专门的Termux指南。

Windows

原生Windows**不支持**。请安装[WSL2](https://learn.microsoft.com/en-us/windows/wsl/install)并从那里运行Hermes Agent。上述安装命令在WSL2中有效。

### 安装程序的作用

安装程序会自动处理一切——所有依赖项（Python、Node.js、ripgrep、ffmpeg）、仓库克隆、虚拟环境、全局hermes命令设置以及大语言模型（LLM）提供商配置。完成后，你就可以开始聊天了。

### 安装后

重新加载你的shell并开始聊天：

```
source ~/.bashrc   # or: source ~/.zshrc
hermes             # Start chatting!
```

若要稍后重新配置个别设置，请使用专用命令：

```
hermes model          # Choose your LLM provider and model
hermes tools          # Configure which tools are enabled
hermes gateway setup  # Set up messaging platforms
hermes config set     # Set individual config values
hermes setup          # Or run the full setup wizard to configure everything at once
```

---

## 先决条件

唯一的先决条件是**Git**。安装程序会自动处理其他所有事项：

+   **UV**（快速Python包管理器）

+   **Python 3.11**（通过 UV，无需 sudo）

+   **Node.js v22**（用于浏览器自动化和WhatsApp桥接）

+   **ripgrep**（快速文件搜索）

+   **ffmpeg**（用于TTS的音频格式转换）

信息

你**无需**手动安装 Python、Node.js、ripgrep 或 ffmpeg。安装程序会检测缺少的内容并为你安装。只需确保git可用（git --version）。

Nix用户

如果您使用 Nix（在 NixOS、macOS 或 Linux 上），有一个专门的设置路径，包含 Nix flake、声明式 NixOS 模块和可选的容器模式。请参阅《Nix 和 NixOS 设置》指南。

---

## 手动安装

如果您希望完全掌控安装过程，请按照以下步骤操作。

### 步骤1：克隆仓库

使用--recurse-submodules克隆以拉取所需的子模块：

```
git clone --recurse-submodules https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
```

如果您已经克隆但没有使用--recurse-submodules：

```
git submodule update --init --recursive
```

### 步骤2：安装UV并创建虚拟环境

```
# Install uv (if not already installed)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create venv with Python 3.11 (uv downloads it if not present — no sudo needed)
uv venv venv --python 3.11
```

提示

你**不需要**激活虚拟环境来使用hermes。入口点有一个硬编码的shebang指向虚拟环境的Python，因此一旦创建符号链接，它就可以全局使用。

### 步骤3：安装Python依赖项

```
# Tell uv which venv to install into
export VIRTUAL_ENV=&quot;$(pwd)/venv&quot;

# Install with all extras
uv pip install -e &quot;.[all]&quot;
```

如果您只需要核心代理（不支持Telegram/Discord/cron）：

```
uv pip install -e &quot;.&quot;
```

**可选附加项明细**

### 步骤4：安装可选子模块（如有需要）

```
# RL training backend (optional)
uv pip install -e &quot;./tinker-atropos&quot;
```

两者都是可选的 —— 如果您跳过它们，相应的工具集将不会可用。

### 步骤5：安装Node.js依赖项（可选）

仅适用于**浏览器自动化****（由Browserb**ase驱动）和WhatsApp桥接：

```
npm install
```

### 步骤6：创建配置目录

```
# Create the directory structure
mkdir -p ~/.hermes/{cron,sessions,logs,memories,skills,pairing,hooks,image_cache,audio_cache,whatsapp/session}

# Copy the example config file
cp cli-config.yaml.example ~/.hermes/config.yaml

# Create an empty .env file for API keys
touch ~/.hermes/.env
```

### 步骤7：添加您的API密钥

打开~/.hermes/.env并至少添加一个大语言模型（LLM）提供商密钥：

```
# Required — at least one LLM provider:
OPENROUTER_API_KEY=sk-or-v1-your-key-here

# Optional — enable additional tools:
FIRECRAWL_API_KEY=fc-your-key          # Web search &amp; scraping (or self-host, see docs)
FAL_KEY=your-fal-key                   # Image generation (FLUX)
```

或者通过CLI设置它们：

```
hermes config set OPENROUTER_API_KEY sk-or-v1-your-key-here
```

### 步骤 8：将hermes添加到您的 PATH 环境变量中

```
mkdir -p ~/.local/bin
ln -sf &quot;$(pwd)/venv/bin/hermes&quot; ~/.local/bin/hermes
```

如果~/.local/bin不在您的PATH中，请将其添加到您的shell配置中：

```
# Bash
echo &#x27;export PATH=&quot;$HOME/.local/bin:$PATH&quot;&#x27; &gt;&gt; ~/.bashrc &amp;&amp; source ~/.bashrc

# Zsh
echo &#x27;export PATH=&quot;$HOME/.local/bin:$PATH&quot;&#x27; &gt;&gt; ~/.zshrc &amp;&amp; source ~/.zshrc

# Fish
fish_add_path $HOME/.local/bin
```

### 步骤9：配置您的提供商

```
hermes model       # Select your LLM provider and model
```

### 步骤10：验证安装

```
hermes version    # Check that the command is available
hermes doctor     # Run diagnostics to verify everything is working
hermes status     # Check your configuration
hermes chat -q &quot;Hello! What tools do you have available?&quot;
```

---

## 快速参考：手动安装（精简版）

对于那些只想知道命令的人：

```
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Clone &amp; enter
git clone --recurse-submodules https://github.com/NousResearch/hermes-agent.git
cd hermes-agent

# Create venv with Python 3.11
uv venv venv --python 3.11
export VIRTUAL_ENV=&quot;$(pwd)/venv&quot;

# Install everything
uv pip install -e &quot;.[all]&quot;
uv pip install -e &quot;./tinker-atropos&quot;
npm install  # optional, for browser tools and WhatsApp

# Configure
mkdir -p ~/.hermes/{cron,sessions,logs,memories,skills,pairing,hooks,image_cache,audio_cache,whatsapp/session}
cp cli-config.yaml.example ~/.hermes/config.yaml
touch ~/.hermes/.env
echo &#x27;OPENROUTER_API_KEY=sk-or-v1-your-key&#x27; &gt;&gt; ~/.hermes/.env

# Make hermes available globally
mkdir -p ~/.local/bin
ln -sf &quot;$(pwd)/venv/bin/hermes&quot; ~/.local/bin/hermes

# Verify
hermes doctor
hermes
```

---

## 故障排除

如需更多诊断信息，请运行hermes doctor — 它将准确告诉你缺少什么以及如何修复。