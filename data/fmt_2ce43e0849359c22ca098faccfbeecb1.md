
# 安装教程

> 来源：[https://deepwisdom.feishu.cn/docx/DOn0d363BolKJyxKvYJccsABnmh](https://deepwisdom.feishu.cn/docx/DOn0d363BolKJyxKvYJccsABnmh)
>
> 推荐 `python` 版本>=3.9
>
> 保姆级安装教程：[保姆级教程 Windows](https://deepwisdom.feishu.cn/wiki/KecHwCGq4i8EjqktzsTcIU23n5b?from=from_copylink)

# 系统支持

| 系统版本 | `Python` 版本 | 是否支持 |
| --- | --- | --- |
| macOS 13.x | `Python` 3.9 | ✔ |
| Windows 11 | `Python` 3.9 | ✔ |
| Ubuntu 22.04 | `Python` 3.9 | ✔ |

# pip 安装

## 安装稳定版本（推荐）

可以像使用任何 `python` 包一样导入 MetaGPT 的组件，使用各种内置的角色、动作、环境等，并构建自己的智能体来服务各种场景。

```
pip install metagpt
```

### 安装子模块

+ `RAG`：使用`pip install 'metagpt[rag]'`安装。用途：用于基于RAG（检索增强生成）的系统，结合多个`LLM`（大型语言模型）和向量存储技术。

+ `OCR`：使用`pip install 'metagpt[ocr]'`安装。用途：用于光学字符识别（`OCR`）任务，从图像中识别和提取文本。

+ `search-ddg`：使用`pip install 'metagpt[search-ddg]'`安装。用途：用于`DuckDuckGo`搜索功能。

+ `search-google`：使用`pip install 'metagpt[search-google]'`安装。用途：用于与`Google API`（如`Google`搜索`API`）交互。

+ `selenium`：使用`pip install 'metagpt[selenium]'`安装。用途：用于自动化浏览器操作和网页抓取。

## 安装最新的开发版本

推荐给那些想要根据自己的独特需求定制框架、尝试新的想法或者利用框架创建复杂功能（如新颖的记忆机制）的开发者和研究者。

```
git clone https://github.com/geekan/MetaGPT.git
cd MetaGPT
pip install -e .
```

# Docker 安装

使用默认的 MetaGPT 镜像

```
# 第1步：下载metagpt官方镜像并准备config2.yaml
docker pull metagpt/metagpt:latest
mkdir -p /opt/metagpt/{config,workspace}
docker run --rm metagpt/metagpt:latest cat /app/metagpt/config/config2.yaml > /opt/metagpt/config/config2.yaml
vim /opt/metagpt/config/config2.yaml # 修改配置

# 第2步：使用容器运行metagpt demo
docker run --rm \
    --privileged \
    -v /opt/metagpt/config/config2.yaml:/app/metagpt/config/config2.yaml \
    -v /opt/metagpt/workspace:/app/metagpt/workspace \
    metagpt/metagpt:latest \
    metagpt "Write a cli snake game"

# 你也可以启动一个容器并在其中执行命令
docker run --name metagpt -d \
    --privileged \
    -v /opt/metagpt/config/config2.yaml:/app/metagpt/config/config2.yaml \
    -v /opt/metagpt/workspace:/app/metagpt/workspace \
    metagpt/metagpt:latest

docker exec -it metagpt /bin/bash
$ metagpt "Write a cli snake game"
```

`docker run`命令做了以下事情：

+ 以特权模式运行，以获得运行浏览器的权限
+ 将主机配置文件`/opt/metagpt/config/config2.yaml`映射到容器`/app/metagpt/config/config2.yaml`
+ 将主机目录`/opt/metagpt/workspace`映射到容器`/app/metagpt/workspace`
+ 执行 demo 命令`metagpt "Write a cli snake game"`

## 自行构建镜像

```
git clone https://github.com/geekan/MetaGPT.git
cd MetaGPT && docker build -t metagpt:custom .
```

# 安装全部功能

适用于生成`象限图`、`系统设计图`、`顺序流程图`等图表，如果运行`metagpt/software_company.py`，它们将作为中间结果提供给你。

## Mermaid

Mermaid 是一种使用文本生成流程图、饼图、甘特图和其他图表的语言。MetaGPT 提供 `nodejs`、`pyppeteer`、`playwright`、`ink` 这四种 Mermaid 引擎将 Mermaid 文本转换为流程图、序列图和甘特图等图表：

### nodejs

你需要先安装 `nodejs`，然后使用 `npm` 进行安装：

```
npm install -g @mermaid-js/mermaid-cli
```

### pyppeteer

`pyppeteer` 是一个用 `Python` 实现的 web 自动化测试工具，可以执行 `JavaScript` 脚本。因此，使用 `pyppeteer` + `mermaidjs` 可以将 Mermaid 文本转换为图表。你可以使用 `pip` 安装 `pyppeteer`：

```
pip install pyppeteer
```

这种方法安装相对简单，没有平台限制，并支持输出 `png`/`svg`/`pdf` 格式。然而，它需要依赖于浏览器，所以你需要先安装一个浏览器，并在运行时设置浏览器路径：

```
export PUPPETEER_EXECUTABLE_PATH=/path/to/your/chromium  # 或者edge或chrome
```

（注意：`pyppeteer` 已经不再维护）

### playwright

由于 `pyppeteer` 已经不再维护，它推荐使用 `playwright-python` 作为替代。使用 `playwright` 运行 mermaid 的原理与 `pyppeteer` 相同。然而，`playwright-python` 需要安装其自己提供的浏览器，不能使用已经安装的浏览器。官方只支持以下平台：

+ `Windows 10+`、`Windows Server 2016+`或 `Windows Subsystem for Linux`（`WSL`）。
+ `MacOS 12 Monterey` 或 `MacOS 13 Ventura`。
+ `Debian 11`、`Debian 12`、`Ubuntu 20.04` 或 `Ubuntu 22.04`。

```
pip install playwrightplaywright install --with-deps chromium
```

### ink

`Mermaid.ink` 项目提供了将 `Mermaid` 文本在线转换为图表的功能，请参阅 `Mermaid.ink`。

然而，此方法仅支持生成 `PNG` 和 `SVG`，不支持 `PDF`，并且在操作过程中需要访问 `Mermaid.ink`，可能会受到网络和其他因素的影响。

### 综合对比

| Mermaid 引擎 | `nodejs` | `pyppeteer` | `playwright` | `ink` |
| --- | --- | --- | --- | --- |
| 安装简易度 | ★ | ★★★ | ★★ | ★★★★★ |
| 平台兼容性 | ★★★★★ | ★★★★ | ★★★ | ★★★★★ |
| 生成 `png` | ✔ | ✔ | ✔ | ✔ |
| 生成 `svg` | ✔ | ✔ | ✔ | ✔ |
| 生成 `pdf` | ✔ | ✔ | ✔ | ✘ |
| 离线运行 | ✔ | ✔ | ✔ | ✘ |

# What Next!

+ 成功安装后，[就可以开始](https://deepwisdom.feishu.cn/wiki/VKwOwffAUitxPOk5wvMcNJBLndf)为各个智能体配置大模型：大模型配置
