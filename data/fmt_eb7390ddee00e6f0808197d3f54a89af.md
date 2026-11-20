
# 安卓系统上使用Termux的Hermes

> 来源：https://qqs7y1hozd1.feishu.cn/docx/HAELdVAB6oTl3px22Q4cz5rqn0b

这是通过 `Termux` 直接在安卓手机上运行 `Hermes Agent` 的测试路径。

它为你在手机上提供一个可用的本地 `CLI`，再加上目前已知可在安卓系统上顺利安装的核心附加组件。

## 测试路径中支持哪些内容？

经过测试的 `Termux` 捆绑包安装情况如下：

+ `Hermes CLI`
+ `cron` 支持
+ `PTY`/后台终端支持
+ `MCP` 支持
+ `Honcho` 内存支持
+ `ACP` 支持

具体而言，它映射到：

```
python -m pip install -e '.[termux]' -c constraints-termux.txt
```

## 哪些部分尚未纳入测试路径？

有几个功能仍然需要桌面/服务器风格的依赖项，这些依赖项尚未针对安卓系统发布，或者尚未在手机上进行验证：

+ `.[all]` 目前在安卓系统上不受支持
+ 额外的被阻止，并且不发布安卓 `wheels` `voicefaster-whisper` -> `ctranslate2` `ctranslate2`
+ 在 `Termux` 安装程序中跳过自动浏览器/ `Playwright` 引导
+ 基于 `Docker` 的终端隔离在 `Termux` 内部不可用

这并不妨碍 `Hermes` 作为原生手机 `CLI` 代理正常工作——这只是意味着推荐的移动设备安装范围有意比桌面/服务器安装范围更窄。

---

## 选项1：单行安装程序

`Hermes` 现在提供了一个支持 `Termux` 的安装路径：

```
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

在 `Termux` 上，安装程序会自动执行以下操作：

+ 系统使用包 `pkg`
+ 创建 `venv` 与 `python -m venv`
+ 安装与 `.[termux]` `pip`
+ 链接到其中，以便它保留在你的 `Termux PATH` `hermes` `$PREFIX/bin`
+ 跳过未经测试的浏览器/ `WhatsApp` 引导程序

如果您需要明确的命令或需要调试安装失败的问题，请使用下面的手动路径。

---

## 选项2：手动安装（完全明确）

### 更新 `Termux` 并安装系统软件包

```
pkg update
pkg install -y git python clang rust make pkg-config libffi openssl nodejs ripgrep ffmpeg
```

### 为什么是这些包？

+ `Python` — 运行时 + 虚拟环境支持
+ `git` — 克隆/更新仓库
+ `clang` `rust` `make` `pkg-config` `libffi` `openssl` — 在 `Android` 上构建一些 `Python` 依赖项所需的工具
+ `nodejs` — 用于测试核心路径之外实验的可选 `Node` 运行时
+ `ripgrep` — 快速文件搜索
+ `ffmpeg` — 媒体/TTS 转换

### 克隆 `Hermes`

```
git clone --recurse-submodules https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
```

如果您已经克隆了但没有克隆子模块：

```
git submodule update --init --recursive
```

### 创建虚拟环境

```
python -m venv venv
source venv/bin/activate
export ANDROID_API_LEVEL="$(getprop ro.build.version.sdk)"
python -m pip install --upgrade pip setuptools wheel
```

`ANDROID_API_LEVEL` 对于基于 `Rust`/`maturin` 的包很重要，例如 `.jiter`

### 安装经过测试的 `Termux` 捆绑包

```
python -m pip install -e '.[termux]' -c constraints-termux.txt
```

如果你只需要最小核心代理，这也可行：

```
python -m pip install -e '.' -c constraints-termux.txt
```

### 安装你的 `Termux PATH` `hermes`

```
ln -sf "$PWD/venv/bin/hermes" "$PREFIX/bin/hermes"
```

`$PREFIX/bin` 已经在 `Termux` 的 `PATH` 环境变量中，因此这使得命令在新的 `shell` 会话中持续生效，而无需每次都重新激活虚拟环境。`hermes`

### 验证安装

```
hermes version
hermes doctor
```

### 启动 `Hermes`

```
hermes
```

---

## 建议的后续设置

### 配置模型

```
hermes model
```

或者直接在 `~/.hermes/.env` 中设置密钥

### 稍后重新运行完整的交互式安装向导

```
hermes setup
```

### 手动安装可选的 `Node` 依赖项

经过测试的 `Termux` 路径会故意跳过 `Node`/浏览器的引导过程。如果您之后想尝试使用浏览器工具：

```
pkg install nodejs-lts
npm install
```

浏览器工具会自动将 `Termux` 目录（`/data/data/com.termux/files/usr/bin`）包含在其 `PATH` 搜索中，因此无需任何额外的 `PATH` 配置即可找到和 `agent-browser` `npx`。

在另有文档说明之前，将安卓系统上的浏览器/ `WhatsApp` 工具视为实验性工具。

---

## 故障排除

### 未找到解决方案在安装 `.[all]` 时

请使用经过测试的 `Termux` 捆绑包代替：

```
python -m pip install -e '.[termux]' -c constraints-termux.txt
```

当前的阻碍因素是额外的声音

+ 语音拉取更快的低语
+ `faster-whisper` 依赖于 `ctranslate2`
+ `ctranslate2` 不发布安卓版本的 `wheel` 包

### `uv` `pip install` 在安卓系统上失败

请使用带有标准库虚拟环境的 `Termux` 路径 + 来代替：`pip`

```
python -m venv venv
source venv/bin/activate
export ANDROID_API_LEVEL="$(getprop ro.build.version.sdk)"
python -m pip install --upgrade pip setuptools wheel
python -m pip install -e '.[termux]' -c constraints-termux.txt
```

### `jiter` / `maturin` 抱怨 `ANDROID_API_LEVEL`

在安装前明确设置 API 级别：

```
export ANDROID_API_LEVEL="$(getprop ro.build.version.sdk)"
python -m pip install -e '.[termux]' -c constraints-termux.txt
```

### `hermes doctor` 提示缺少 `ripgrep` 或 `Node`

使用 `Termux` 软件包安装它们：

```
pkg install ripgrep nodejs
```

### 安装 `Python` 包时构建失败

确保已安装构建工具链：

```
pkg install clang rust make pkg-config libffi openssl
```

然后重试：

```
python -m pip install -e '.[termux]' -c constraints-termux.txt
```

---

## 手机上已知的限制

+ `Docker` 后端不可用
+ 本地语音转录通过是不可用的测试路径更快-耳语
+ 浏览器自动化设置被安装程序有意跳过
+ 某些可选附加功能可能有效，但目前仅记录为经过测试的安卓捆绑包 `.[termux]`

如果遇到新的特定于安卓的问题，请在 `GitHub` 上创建一个问题，并提供以下内容：

+ 您的安卓版本
+ `termux-info`
+ `python --version`
+ 爱马仕医生
+ 确切的安装命令和完整的错误输出
