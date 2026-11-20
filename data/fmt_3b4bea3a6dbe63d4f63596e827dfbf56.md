
![](img/671c86b28eb785518be18bc57efe478a.png)

### 4. 借助MCP Inspector进行功能测试

在完成了项目的源码开发后，接下来就可以使用MCP官方提供的Inspector工具进行功能测试了。

+   Inspector项目地址：https://github.com/modelcontextprotocol/inspector

![](img/17214a371b2d31256d0d7c11f62796e8.png)

这里需要注意的是，由于当前MCP服务器脚本是使用MCP SDK进行的开发，并且MCP服务器的核心功能是为MCP客户端提供服务，因此如果不采用官方提供的Inspector进行debug，就需要自己手写一个MCP client并尝试与MCP server进行通信，才能进行debug。

换而言之就是官方提供的Inspector实际上是一个通用的MCP Client，能够测试MCP server实际运行效果。具体debug流程如下：

+   Step 1.运行MCP服务器脚本

```
# 回到项目主目录
# cd /root/autodl-tmp/MCP/mcp-server-deepresearch

# 运行Inspector
npx -y @modelcontextprotocol/inspector uv run ./src/mcp_server_deepresearch/server.py --openai_api_key YOUR_KEY
```

![](img/d8b10b87e7f9b7883685e31902b31217.png)

> 注意此处要输入`OpenAI-API-KEY`

启动完成后能够看到，此时服务器端口号为6277，而Inspector端口号为6274，如果是在本地运行，则可以直接在浏览器中输入`localhost:6274`进入Inspector页面，而如果是在AutoDL上运行，则需要使用SSH隧道工具，将两个端口号都映射到本地：

![](img/76da5ed983af5aeb2cb0e939c093675b.png)

然后再在`localhost:6274`进入Inspector页面。

+   Step 2.设置Inspector相应时间

首次进入Inspector页面时需要设置响应时间，由于当前外部工具运行时间较长，Inspector默认只给出10秒响应窗口，超时未响应则会报错，因此这里需要在Configuration中进行响应时间设置：

然后点击Connect即可连接MCP Server。

> 超时响应报错示例：

+   Step 4.在线运行

当连接上MCP Server之后，接下来点击Tools、再点击List Tools，选择deepresearch工具，并在右侧输入问题，例如你好，我想深入了解下到底什么是MCP（Model Context Protocol）技术，接下来MCP服务器会开始进行运行：

![](img/fbd394c4d4baf3c434bb2bfe13e2cf17.png)

运行后即可得到完整调研报告结果：

由此即完成了完整的MCP Server脚本debug。接下来即可考虑将项目进行部署上线了。

## 三、MCP服务器部署流程

对于MCP服务器来说，一般有两种部署方式，分别是在线发布和离线拷贝运行。

+   **MCP服务器在线发布**

    所谓在线发布，指的是我们可以将开发好的MCP服务器打包上传到`pypi`平台或者`npm`平台进行云平台托管，一旦上传成功，后续用户即可使用`uvx`或者`npx`进行下载和使用。我们在3. MCP工具标准接入流程一节中所介绍的各主流MCP客户端接入MCP工具的标准流程，实际上就是实时下载MCP工具，然后在本地进行运行。而各大MCP工具平台，如Smithery，本质上也是在`pipy`平台或者`npm`平台上托管的库基础上进行的进一步维护。

+   `pipy`：https://pypi.org/

+   `npm`：https://www.npmjs.com/

![](img/c0bea0ec71a9bf49a33f3d0453d50a1d.png)

+   **MCP服务器离线拷贝运行**

    而所谓的离线部署运行，就好理解的多，指的是将打包好的库直接拷贝到服务器上或者给他人进行使用，没有在云平台托管的这个环节，适合非公开MCP服务器进行部署。

接下来围绕我们创建的DeepResearch MCP服务器，详细介绍这两种部署方法。

### 1. DeepResearch项目完善

尽管此前我们已经完成了DeepResearch项目的核心脚本debug，但其仍然不算是一个结构完整的项目。核心脚本的调用关系并不明确，同时项目说明也不够完善。因此这里我们首先需要先完善项目的主体内容，再考虑进行部署或上线发布。

#### 1.1 `src layout`项目结构

其实在此之前，我们将代码都放在`src`内的某个文件夹里，这种项目结构也被称作`src layout`项目结构，这是一种非常通用、同时也便于代码维护的项目结构，基本示意图如下：

```
your-project/
├── src/               ← ✅ 所有业务代码都集中在这里
│   └── your_package/  ← Python 包的实际内容（含 __init__.py）
├── dist/              ← 构建后的分发包（如 wheel）
├── .venv/             ← 虚拟环境（本地依赖隔离）
├── .git/              ← Git 管理
├── pyproject.toml     ← 项目配置
├── README.md          ← 说明文件
├── .python-version    ← Python版本声明
├── .gitignore         ← Git 忽略规则
└── uv.lock            ← uv 的依赖锁文件
```

接下来我们还需要在`src/mcp_server_deepresearch`中创建两个`py`脚本，其一是`__init__.py`，使当前文件夹可以作为Python的一个库进行导入，需要在`__init__.py`写入如下代码：

```
from .server import main
```

同时再创建一个`__main__.py`，用于实际执行主函数调用流程：

```
from mcp_server_deepresearch import main

main()
```

最终`src/mcp_server_deepresearch`中代码文件如下所示：

![](img/c21ba71f685365fbb3261aeb502a2d49.png)

整个项目的脚本文件可以在网盘中领取：

![](img/6344a1d36e50b04445650a2dea5e2ccb.png)

![](img/7043ee48e1db3da88e28105cd80e3e9d.png)

#### 1.2 修改`pyproject.toml`

创建完基本项目结构后，让我们回到当前项目主目录下，删除`main.py`（如果有的话），然后修改项目配置文件`pyproject.toml`:

```
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "mcp-server-deepresearch"
version = "0.1.2"
description = "实现类deepresearch功能的MCP工具"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "asyncio>=3.4.3",
    "mcp>=1.6.0",
    "openai>=1.75.0",
    "openai-agents>=0.0.12",
    "pydantic>=2.11.3",
    "typing>=3.10.0.0",
]

[project.scripts]
mcp-server-deepresearch = "mcp_server_deepresearch:main"

[tool.setuptools]
package-dir = {"" = "src"}

[tool.setuptools.packages.find]
where = ["src"]
```

具体配置解释如下：

`[build-system]` 段

```
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"
```

告诉 Python 构建工具（如 `pip`、`uv`、`build`）：

+   要使用 `setuptools` 来构建项目

+   同时依赖 `wheel`，因为你要构建 `.whl` 包

`[project]` 段

```
[project]
name = "mcp-server-deepresearch"
version = "0.1.2"
description = "实现类deepresearch功能的MCP工具"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "asyncio>=3.4.3",
    "mcp>=1.6.0",
    "openai>=1.75.0",
    "openai-agents>=0.0.12",
    "pydantic>=2.11.3",
    "typing>=3.10.0.0",
]
```

定义项目的**元数据**，包括：

+   项目名称、版本、描述

+   Python 最低版本要求

+   自动安装的依赖项（相当于`requirements.txt`）

`[project.scripts]`：命令行入口

```
[project.scripts]
mcp-server-deepresearch = "mcp_server_deepresearch:main"
```

定义一个**可执行命令行脚本**，等价于在终端中直接运行：

```
mcp-server-deepresearch --openai_api_key=...
```

它会自动调用你包内的：

```
mcp_server_deepresearch/main.py 里的 main() 函数
```

也支持：

```
mcp_server_deepresearch/__init__.py 中的 main() 函数
```

只要是 `main()` 函数即可。

```
[tool.setuptools]
package-dir = {"" = "src"}

[tool.setuptools.packages.find]
where = ["src"]
```

明确告诉 `setuptools`：

+   你的项目源代码都在 `src/` 目录下

+   请去 `src/` 中查找 Python 包（即包含 `__init__.py` 的目录）

| 区块 | 作用 | 评价 |
| --- | --- | --- |
| `[build-system]` | 告诉构建工具如何构建项目 | ✅ 非常规范 |
| `[project]` | 定义项目基本信息和依赖 | ✅ 可以删掉 `asyncio` 和 `typing` |
| `[project.scripts]` | 定义命令行工具 | ✅ 可直接用 `mcp-server-deepresearch` 命令运行 |
| `[tool.setuptools]` | 指定源码位置 | ✅ 与 `src layout` 完美配合 |

然后对整个项目进行打包。首先需要安装打包（和上传）工具：

```
uv pip install build twine
```

![](img/2540a929ce4ebb723ae904b0c2b3024c.png)

然后使用如下命令进行打包：

```
# 回到项目主目录
# cd /root/autodl-tmp/MCP/mcp-server-deepresearch

# 打包
python -m build
```

![](img/afcd9a1d88b13bfc14bc873d961df44b.png)

打包完成后会在主目录下新增一个`dist`文件夹：

![](img/f1cab1a028a23e630f45535e8ea8e514.png)

以及在`src`目录下会创建一个`.egg-info`文件夹，

![](img/d3425b20f0c313d91ed2f63d434a91e3.png)

这两个文件夹内容解释如下：

| 文件类型 | 说明 |
| --- | --- |
| `.whl` 文件 | Python wheel 格式的二进制分发包，最常用 |
| `.tar.gz` 文件 | 源代码包（source distribution） |

而在离线部署时候，别人安装时只要 `pip install xxx.whl` 就可以了（无需源码），同时上传到 `PyPI`、内网 `PyPI`、AI 模型平台等地方部署。

+   `<project-name>.egg-info/` 文件夹：元数据文件夹，这个是 `setuptools` 在构建或打包时生成的一个“项目元信息目录”。比如会看到这样的结构：

```
mcp_server_deepresearch.egg-info/
├── PKG-INFO
├── SOURCES.txt
├── top_level.txt
├── dependency_links.txt
├── requires.txt
```

`PKG-INFO`：你项目的基本信息（名称、版本、作者、依赖等），`requires.txt`：列出依赖项，`SOURCES.txt`：列出所有打包进 `.whl` 的文件，`top_level.txt`：顶层包名（比如 `mcp_server_deepresearch`）。

### 2. 项目离线拷贝

接下来继续介绍离线拷贝流程。假设我们现在需要从服务器A上把项目拷贝到服务器B上，最简单的方法首先是在服务器A上进行项目压缩：

```
# 回到主目录 
# cd /root/autodl-tmp/MCP/mcp-server-deepresearch
tar --exclude='.venv' -czvf mcp-server-deepresearch.tar.gz *
```

压缩后会创建一个名为`mcp-server-deepresearch.tar.gz`的文件：

![](img/71889776151e847ea74696a703b87124.png)

将其下载后即可上传到服务器B上，例如将其上传到B服务器的`/root/autodl-tmp/MCP/MCP-Test`路径下：

![](img/7b72381ccd3b9d03906113d3068455b9.png)

然后即可使用如下命令进行解压缩：

```
tar -xzvf mcp-server-deepresearch.tar.gz
```

![](img/293cbbda8e581b5a158d0a468cb1b8c6.png)

然后即可激活虚拟环境并相关依赖：

```
# 创建虚拟环境
uv venv

# 激活虚拟环境
source .venv/bin/activate

# 使用uv（推荐）或pip安装
uv pip install .
```

![](img/f0f0d073a6ab1747cffadc5cacac22cf.png)

然后即可运行Inspector进行测试：

```
# 运行Inspector
npx -y @modelcontextprotocol/inspector uv run ./src/mcp_server_deepresearch/server.py --openai_api_key YOUR_KEY
```

![](img/38fa583e56dcc67fbab8a905c00ae004.png)

![](img/a998aa47c7018c799e9ad6e80aab005a.png)

完整项目压缩包在网盘中领取：

![](img/869d59eddf4b0a2ec02d04933d088bc0.png)

![](img/32e30b535ffdf222530fcf58b3d967ba.png)

### 3. 项目发布到`pipy`平台

除了离线拷贝之外，我们也可以把自己开发完成的MCP Server发布到一些托管平台上方便其他人下载使用，如果是`js`开发的MCP Server可以发布在`npm`平台上，而如果是Python项目，则可以发布在`pypi`上。我们这里以`pypi`发布流程为例进行演示。首先需要先在`pypi`：https://pypi.org/上进行账号注册，然后获取`API-KEY`作为身份验证，

![](img/becb2d0e51b4006779f6c7c27f847d2c.png)

然后需要再次确认打包完成，并开启上传
