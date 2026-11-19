
# Qwen3接入MCP技术实战（下）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/IIMhdNUmMoJWZyx0mZJcUJVwnQc](https://kq4b3vgg5b.feishu.cn/docx/IIMhdNUmMoJWZyx0mZJcUJVwnQc)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(5月班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(5月班)

![](img/514aa31b938d1b2625aa08c45e2f28be.png)

![](img/801a17f31d3cfe4a1d7dec94f0217d1a.png)

此外，公开课全套学习资料，已上传至网盘

（链接: https://pan.baidu.com/s/11nFqw4w2kIvypKqzBW9N🐧 提取码: 6k24 ）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## Qwen3接入MCP智能体开发实战

## Part 2.Qwen3+Qwen-Agent接入MCP进行智能体开发

### 三、Qwen3+Qwen-Agent接入MCP流程

`Qwen-Agent` 是阿里通义团队推出的开源智能体（Agent）开发框架，基于 `Qwen` 大语言模型（LLM）构建，旨在帮助开发者快速构建具备指令遵循、工具调用、规划与记忆等能力的 AI 应用。它提供了从底层原子组件到高级智能体抽象的完整开发工具链，适用于多种复杂任务场景。

+   官网地址：https://github.com/QwenLM/Qwen-Agent

![](img/679d002598af287a869220498f254977.png)

#### 1. Qwen-Agent 核心功能与技术特点

总的来说，`Qwen-Agent`本质上并不是一个`Multi_Agent`开发框架，而更像是为`Qwen`系列模型量身定制的增强器。借助`Qwen-Agent`，`Qwen`系列模型能够更好的开启`Function calling`功能（包括多步`Function calling`和自动`debug`等）、能够更好的进行模型记忆管理、能够更加便捷的连接`MCP`工具、能够进行长文档检索等，此外`Qwen-Agent`还提供了便捷的前端交互页面。如果是`Qwen`用户的深度使用者，推荐使用`Qwen-Agent`对模型性能进行增强，同时若是希望借助`Qwen3`快速接入`MCP`工具，则`Qwen-Agent`也会是一个非常便捷的工具。

`Qwen-Agent`核心特性如下：

+   更强的工具调用（`Function Calling`）能力:框架支持智能体自动调用外部工具或函数，包括内置的代码解释器、浏览器助手等，也支持开发者自定义工具，扩展智能体的能力。

+   便捷的`MCP`工具接入流程：最新版的`Qwen-Agent`已经集成了`MCP`工具接入流程，我们仅需写入`MCP`配置，即可在`Qwen-Agent`中调用`MCP`工具：

![](img/bb8296e2a956b4d1ec3f67e6341fa152.png)

而这也是本次`Qwen3`模型更新时，官方在更新公告中重点展示的内容。

![](img/76253afdd1a62e90fab67d7a94e2764e.png)

+   规划与记忆能力： 智能体具备任务规划能力，能够根据用户需求自动制定执行步骤。同时，具备上下文记忆功能，能在对话中保持状态，提供连贯的交互体验。

+   长文本处理与 `RA`： `Qwen-Agent` 集成了检索增强生成（`RAG`）机制，支持处理从 8K 到 100 万 tokens 的长文档，通过文档分块和相关性检索，提升上下互与展示

+   `UI`前端交互与展示

![](img/c28c49590b838747467548fcba254598.png)

#### 2. Qwen-Agent下载与创建多轮对话机器人

+   从 `PyPI` 安装稳定版本

在此命令中，可使用双括号指定如下的可选依赖，其中：

+   `[gui]` 用于提供基于 `Gradio` 的 `GUI` 支持；

+   `[rag]` 用于支持 `RAG`；

+   `[code_interpreter]` 用于提供代码解释器相关支持；

+   `[mcp]` 用于支持 `MCP`。

```
! pip install -U "qwen-agent[rag,code_interpreter,gui,mcp]"
```



## 依赖包安装日志

```
Collecting qwen-agent[code_interpreter,gui,mcp,rag]
  Using cached qwen_agent-0.0.20-py3-none-any.whl.metadata (13 kB)
Collecting dashscope>=1.11.0 (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached dashscope-1.23.2-py3-none-any.whl.metadata (6.8 kB)
Requirement already satisfied: eval-type-backport in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (0.2.2)
Requirement already satisfied: json5 in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (0.9.25)
Collecting jsonlines (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached jsonlines-4.0.0-py3-none-any.whl.metadata (1.6 kB)
Requirement already satisfied: jsonschema in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (4.23.0)
Requirement already satisfied: openai in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (1.70.0)
Requirement already satisfied: pydantic>=2.3.0 in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (2.10.6)
Requirement already satisfied: requests in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (2.32.3)
Requirement already satisfied: tiktoken in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (0.7.0)
Requirement already satisfied: anyio>=3.7.1 in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (4.9.0)
Requirement already satisfied: fastapi>=0.103.1 in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (0.115.7)
Requirement already satisfied: jupyter>=1.0.0 in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (1.1.1)
Requirement already satisfied: matplotlib in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (3.9.2)
Requirement already satisfied: numpy in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (1.26.4)
Requirement already satisfied: pandas in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (2.2.3)
Requirement already satisfied: pillow in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (11.1.0)
Requirement already satisfied: seaborn in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (0.13.2)
Requirement already satisfied: sympy in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (1.14.0)
Requirement already satisfied: uvicorn>=0.23.2 in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (0.34.0)
Collecting pydantic>=2.3.0 (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached pydantic-2.9.2-py3-none-any.whl.metadata (149 kB)
Collecting pydantic-core==2.23.4 (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached pydantic_core-2.23.4-cp312-none-win_amd64.whl.metadata (6.7 kB)
Collecting gradio==5.23.1 (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached gradio-5.23.1-py3-none-any.whl.metadata (16 kB)
Collecting gradio-client==1.8.0 (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached gradio_client-1.8.0-py3-none-any.whl.metadata (7.1 kB)
Collecting modelscope-studio==1.1.7 (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached modelscope_studio-1.1.7-py3-none-any.whl.metadata (4.1 kB)
Requirement already satisfied: mcp in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (1.6.0)
Requirement already satisfied: charset-normalizer in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (3.3.2)
Requirement already satisfied: rank-bm25 in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (0.2.2)
Collecting jieba (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached jieba-0.42.1-py3-none-any.whl
Requirement already satisfied: snowballstemmer in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (2.2.0)
Requirement already satisfied: beautifulsoup4 in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (4.12.3)
Requirement already satisfied: pdfminer.six in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (20240706)
Collecting pdfplumber (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached pdfplumber-0.11.6-py3-none-any.whl.metadata (42 kB)
Collecting python-docx (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached python_docx-1.1.2-py3-none-any.whl.metadata (2.0 kB)
Requirement already satisfied: python-pptx in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (1.0.0)
Requirement already satisfied: tabulate in c:\python312\lib\site-packages (from qwen-agent[code_interpreter,gui,mcp,rag]) (0.9.0)
Collecting aiofiles<24.0,>=22.0 (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached aiofiles-23.2.1-py3-none-any.whl.metadata (9.7 kB)
Collecting ffmpy (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached ffmpy-0.5.0-py3-none-any.whl.metadata (3.0 kB)
Collecting groovy~=0.1 (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached groovy-0.1.2-py3-none-any.whl.metadata (6.1 kB)
Requirement already satisfied: httpx>=0.24.1 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (0.27.2)
Requirement already satisfied: huggingface-hub>=0.28.1 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (0.30.2)
Requirement already satisfied: jinja2<4.0 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (3.1.4)
Requirement already satisfied: markupsafe<4.0,>=2.0 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (2.1.5)
Requirement already satisfied: orjson~=3.0 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (3.10.16)
Requirement already satisfied: packaging in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (23.2)
Requirement already satisfied: pydub in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (0.25.1)
Requirement already satisfied: python-multipart>=0.0.18 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (0.0.20)
Requirement already satisfied: pyyaml<7.0,>=5.0 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (6.0.2)
Collecting ruff>=0.9.3 (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached ruff-0.11.7-py3-none-win_amd64.whl.metadata (26 kB)
Collecting safehttpx<0.2.0,>=0.1.6 (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached safehttpx-0.1.6-py3-none-any.whl.metadata (4.2 kB)
Collecting semantic-version~=2.0 (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached semantic_version-2.10.0-py2.py3-none-any.whl.metadata (9.7 kB)
Requirement already satisfied: starlette<1.0,>=0.40.0 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (0.41.3)
Requirement already satisfied: tomlkit<0.14.0,>=0.12.0 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (0.13.2)
Requirement already satisfied: typer<1.0,>=0.12 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (0.12.5)
Requirement already satisfied: typing-extensions~=4.0 in c:\python312\lib\site-packages (from gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (4.12.2)
Requirement already satisfied: fsspec in c:\python312\lib\site-packages (from gradio-client==1.8.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2025.3.0)
Requirement already satisfied: websockets<16.0,>=10.0 in c:\python312\lib\site-packages (from gradio-client==1.8.0->qwen-agent[code_interpreter,gui,mcp,rag]) (14.2)
Requirement already satisfied: annotated-types>=0.6.0 in c:\python312\lib\site-packages (from pydantic>=2.3.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.7.0)
Requirement already satisfied: idna>=2.8 in c:\python312\lib\site-packages (from anyio>=3.7.1->qwen-agent[code_interpreter,gui,mcp,rag]) (3.7)
Requirement already satisfied: sniffio>=1.1 in c:\python312\lib\site-packages (from anyio>=3.7.1->qwen-agent[code_interpreter,gui,mcp,rag]) (1.3.1)
Requirement already satisfied: aiohttp in c:\python312\lib\site-packages (from dashscope>=1.11.0->qwen-agent[code_interpreter,gui,mcp,rag]) (3.11.11)
Requirement already satisfied: websocket-client in c:\python312\lib\site-packages (from dashscope>=1.11.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.8.0)
Requirement already satisfied: notebook in c:\python312\lib\site-packages (from jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (7.2.2)
Requirement already satisfied: jupyter-console in c:\python312\lib\site-packages (from jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (6.6.3)
Requirement already satisfied: nbconvert in c:\python312\lib\site-packages (from jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (7.16.4)
Requirement already satisfied: ipykernel in c:\python312\lib\site-packages (from jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (6.29.5)
Requirement already satisfied: ipywidgets in c:\python312\lib\site-packages (from jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (8.1.5)
Requirement already satisfied: jupyterlab in c:\python312\lib\site-packages (from jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (4.2.5)
Requirement already satisfied: python-dateutil>=2.8.2 in c:\python312\lib\site-packages (from pandas->qwen-agent[code_interpreter,gui,mcp,rag]) (2.9.0.post0)
Requirement already satisfied: pytz>=2020.1 in c:\python312\lib\site-packages (from pandas->qwen-agent[code_interpreter,gui,mcp,rag]) (2024.2)
Requirement already satisfied: tzdata>=2022.7 in c:\python312\lib\site-packages (from pandas->qwen-agent[code_interpreter,gui,mcp,rag]) (2024.2)
Requirement already satisfied: click>=7.0 in c:\python312\lib\site-packages (from uvicorn>=0.23.2->qwen-agent[code_interpreter,gui,mcp,rag]) (8.1.8)
Requirement already satisfied: h11>=0.8 in c:\python312\lib\site-packages (from uvicorn>=0.23.2->qwen-agent[code_interpreter,gui,mcp,rag]) (0.14.0)
Requirement already satisfied: soupsieve>1.2 in c:\python312\lib\site-packages (from beautifulsoup4->qwen-agent[code_interpreter,gui,mcp,rag]) (2.6)
Requirement already satisfied: attrs>=19.2.0 in c:\python312\lib\site-packages (from jsonlines->qwen-agent[code_interpreter,gui,mcp,rag]) (24.2.0)
Requirement already satisfied: jsonschema-specifications>=2023.03.6 in c:\python312\lib\site-packages (from jsonschema->qwen-agent[code_interpreter,gui,mcp,rag]) (2024.10.1)
Requirement already satisfied: referencing>=0.28.4 in c:\python312\lib\site-packages (from jsonschema->qwen-agent[code_interpreter,gui,mcp,rag]) (0.35.1)
Requirement already satisfied: rpds-py>=0.7.1 in c:\python312\lib\site-packages (from jsonschema->qwen-agent[code_interpreter,gui,mcp,rag]) (0.20.1)
Requirement already satisfied: contourpy>=1.0.1 in c:\python312\lib\site-packages (from matplotlib->qwen-agent[code_interpreter,gui,mcp,rag]) (1.3.1)
Requirement already satisfied: cycler>=0.10 in c:\python312\lib\site-packages (from matplotlib->qwen-agent[code_interpreter,gui,mcp,rag]) (0.12.1)
Requirement already satisfied: fonttools>=4.22.0 in c:\python312\lib\site-packages (from matplotlib->qwen-agent[code_interpreter,gui,mcp,rag]) (4.55.0)
Requirement already satisfied: kiwisolver>=1.3.1 in c:\python312\lib\site-packages (from matplotlib->qwen-agent[code_interpreter,gui,mcp,rag]) (1.4.7)
Requirement already satisfied: pyparsing>=2.3.1 in c:\python312\lib\site-packages (from matplotlib->qwen-agent[code_interpreter,gui,mcp,rag]) (3.2.0)
Requirement already satisfied: httpx-sse>=0.4 in c:\python312\lib\site-packages (from mcp->qwen-agent[code_interpreter,gui,mcp,rag]) (0.4.0)
Requirement already satisfied: pydantic-settings>=2.5.2 in c:\python312\lib\site-packages (from mcp->qwen-agent[code_interpreter,gui,mcp,rag]) (2.7.1)
Requirement already satisfied: sse-starlette>=1.6.1 in c:\python312\lib\site-packages (from mcp->qwen-agent[code_interpreter,gui,mcp,rag]) (2.2.1)
Requirement already satisfied: distro<2,>=1.7.0 in c:\python312\lib\site-packages (from openai->qwen-agent[code_interpreter,gui,mcp,rag]) (1.9.0)
Requirement already satisfied: jiter<1,>=0.4.0 in c:\python312\lib\site-packages (from openai->qwen-agent[code_interpreter,gui,mcp,rag]) (0.8.0)
Requirement already satisfied: tqdm>4 in c:\python312\lib\site-packages (from openai->qwen-agent[code_interpreter,gui,mcp,rag]) (4.67.1)
Requirement already satisfied: cryptography>=36.0.0 in c:\python312\lib\site-packages (from pdfminer.six->qwen-agent[code_interpreter,gui,mcp,rag]) (43.0.3)
Collecting pdfminer.six (from qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached pdfminer_six-20250327-py3-none-any.whl.metadata (4.1 kB)
Collecting pypdfium2>=4.18.0 (from pdfplumber->qwen-agent[code_interpreter,gui,mcp,rag])
  Using cached pypdfium2-4.30.1-py3-none-win_amd64.whl.metadata (48 kB)
Requirement already satisfied: lxml>=3.1.0 in c:\python312\lib\site-packages (from python-docx->qwen-agent[code_interpreter,gui,mcp,rag]) (5.3.0)
Requirement already satisfied: XlsxWriter>=0.5.7 in c:\python312\lib\site-packages (from python-pptx->qwen-agent[code_interpreter,gui,mcp,rag]) (3.2.0)
Requirement already satisfied: urllib3<3,>=1.21.1 in c:\python312\lib\site-packages (from requests->qwen-agent[code_interpreter,gui,mcp,rag]) (2.4.0)
Requirement already satisfied: certifi>=2017.4.17 in c:\python312\lib\site-packages (from requests->qwen-agent[code_interpreter,gui,mcp,rag]) (2025.4.26)
Requirement already satisfied: mpmath<1.4,>=1.1.0 in c:\python312\lib\site-packages (from sympy->qwen-agent[code_interpreter,gui,mcp,rag]) (1.3.0)
Requirement already satisfied: regex>=2022.1.18 in c:\python312\lib\site-packages (from tiktoken->qwen-agent[code_interpreter,gui,mcp,rag]) (2024.11.6)
Requirement already satisfied: colorama in c:\python312\lib\site-packages (from click>=7.0->uvicorn>=0.23.2->qwen-agent[code_interpreter,gui,mcp,rag]) (0.4.6)
Requirement already satisfied: cffi>=1.12 in c:\python312\lib\site-packages (from cryptography>=36.0.0->pdfminer.six->qwen-agent[code_interpreter,gui,mcp,rag]) (1.17.1)
Requirement already satisfied: httpcore==1.* in c:\python312\lib\site-packages (from httpx>=0.24.1->gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (1.0.6)
Requirement already satisfied: filelock in c:\python312\lib\site-packages (from huggingface-hub>=0.28.1->gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (3.18.0)
Requirement already satisfied: python-dotenv>=0.21.0 in c:\python312\lib\site-packages (from pydantic-settings>=2.5.2->mcp->qwen-agent[code_interpreter,gui,mcp,rag]) (1.0.1)
Requirement already satisfied: six>=1.5 in c:\python312\lib\site-packages (from python-dateutil>=2.8.2->pandas->qwen-agent[code_interpreter,gui,mcp,rag]) (1.16.0)
Requirement already satisfied: shellingham>=1.3.0 in c:\python312\lib\site-packages (from typer<1.0,>=0.12->gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (1.5.4)
Requirement already satisfied: rich>=10.11.0 in c:\python312\lib\site-packages (from typer<1.0,>=0.12->gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (13.9.4)
Requirement already satisfied: aiohappyeyeballs>=2.3.0 in c:\python312\lib\site-packages (from aiohttp->dashscope>=1.11.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.6.1)
Requirement already satisfied: aiosignal>=1.1.2 in c:\python312\lib\site-packages (from aiohttp->dashscope>=1.11.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.3.2)
Requirement already satisfied: frozenlist>=1.1.1 in c:\python312\lib\site-packages (from aiohttp->dashscope>=1.11.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.6.0)
Requirement already satisfied: multidict<7.0,>=4.5 in c:\python312\lib\site-packages (from aiohttp->dashscope>=1.11.0->qwen-agent[code_interpreter,gui,mcp,rag]) (6.4.3)
Requirement already satisfied: propcache>=0.2.0 in c:\python312\lib\site-packages (from aiohttp->dashscope>=1.11.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.3.1)
Requirement already satisfied: yarl<2.0,>=1.17.0 in c:\python312\lib\site-packages (from aiohttp->dashscope>=1.11.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.20.0)
Requirement already satisfied: comm>=0.1.1 in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.2.2)
Requirement already satisfied: debugpy>=1.6.5 in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.8.7)
Requirement already satisfied: ipython>=7.23.1 in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (8.29.0)
Requirement already satisfied: jupyter-client>=6.1.12 in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (8.6.3)
Requirement already satisfied: jupyter-core!=5.0.*,>=4.12 in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (5.7.2)
Requirement already satisfied: matplotlib-inline>=0.1 in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.1.7)
Requirement already satisfied: nest-asyncio in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.6.0)
Requirement already satisfied: psutil in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (5.9.8)
Requirement already satisfied: pyzmq>=24 in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (26.2.0)
Requirement already satisfied: tornado>=6.1 in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (6.4.1)
Requirement already satisfied: traitlets>=5.4.0 in c:\python312\lib\site-packages (from ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (5.14.3)
Requirement already satisfied: widgetsnbextension~=4.0.12 in c:\python312\lib\site-packages (from ipywidgets->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (4.0.13)
Requirement already satisfied: jupyterlab-widgets~=3.0.12 in c:\python312\lib\site-packages (from ipywidgets->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (3.0.13)
Requirement already satisfied: prompt-toolkit>=3.0.30 in c:\python312\lib\site-packages (from jupyter-console->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (3.0.48)
Requirement already satisfied: pygments in c:\python312\lib\site-packages (from jupyter-console->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.18.0)
Requirement already satisfied: async-lru>=1.0.0 in c:\python312\lib\site-packages (from jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.0.4)
Requirement already satisfied: jupyter-lsp>=2.0.0 in c:\python312\lib\site-packages (from jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.2.5)
Requirement already satisfied: jupyter-server<3,>=2.4.0 in c:\python312\lib\site-packages (from jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.14.2)
Requirement already satisfied: jupyterlab-server<3,>=2.27.1 in c:\python312\lib\site-packages (from jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.27.3)
Requirement already satisfied: notebook-shim>=0.2 in c:\python312\lib\site-packages (from jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.2.4)
Requirement already satisfied: setuptools>=40.1.0 in c:\python312\lib\site-packages (from jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (75.3.0)
Requirement already satisfied: bleach!=5.0.0 in c:\python312\lib\site-packages (from nbconvert->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (6.2.0)
Requirement already satisfied: defusedxml in c:\python312\lib\site-packages (from nbconvert->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.7.1)
Requirement already satisfied: jupyterlab-pygments in c:\python312\lib\site-packages (from nbconvert->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.3.0)
Requirement already satisfied: mistune<4,>=2.0.3 in c:\python312\lib\site-packages (from nbconvert->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (3.0.2)
Requirement already satisfied: nbclient>=0.5.0 in c:\python312\lib\site-packages (from nbconvert->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.10.0)
Requirement already satisfied: nbformat>=5.7 in c:\python312\lib\site-packages (from nbconvert->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (5.10.4)
Requirement already satisfied: pandocfilters>=1.4.1 in c:\python312\lib\site-packages (from nbconvert->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.5.1)
Requirement already satisfied: tinycss2 in c:\python312\lib\site-packages (from nbconvert->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.4.0)
Requirement already satisfied: webencodings in c:\python312\lib\site-packages (from bleach!=5.0.0->nbconvert->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.5.1)
Requirement already satisfied: pycparser in c:\python312\lib\site-packages (from cffi>=1.12->cryptography>=36.0.0->pdfminer.six->qwen-agent[code_interpreter,gui,mcp,rag]) (2.22)
Requirement already satisfied: decorator in c:\python312\lib\site-packages (from ipython>=7.23.1->ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (5.1.1)
Requirement already satisfied: jedi>=0.16 in c:\python312\lib\site-packages (from ipython>=7.23.1->ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.19.1)
Requirement already satisfied: stack-data in c:\python312\lib\site-packages (from ipython>=7.23.1->ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.6.3)
Requirement already satisfied: platformdirs>=2.5 in c:\python312\lib\site-packages (from jupyter-core!=5.0.*,>=4.12->ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (4.3.6)
Requirement already satisfied: pywin32>=300 in c:\python312\lib\site-packages (from jupyter-core!=5.0.*,>=4.12->ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (308)
Requirement already satisfied: argon2-cffi>=21.1 in c:\python312\lib\site-packages (from jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (23.1.0)
Requirement already satisfied: jupyter-events>=0.9.0 in c:\python312\lib\site-packages (from jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.10.0)
Requirement already satisfied: jupyter-server-terminals>=0.4.4 in c:\python312\lib\site-packages (from jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.5.3)
Requirement already satisfied: overrides>=5.0 in c:\python312\lib\site-packages (from jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (7.7.0)
Requirement already satisfied: prometheus-client>=0.9 in c:\python312\lib\site-packages (from jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.21.0)
Requirement already satisfied: pywinpty>=2.0.1 in c:\python312\lib\site-packages (from jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.0.14)
Requirement already satisfied: send2trash>=1.8.2 in c:\python312\lib\site-packages (from jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.8.3)
Requirement already satisfied: terminado>=0.8.3 in c:\python312\lib\site-packages (from jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.18.1)
Requirement already satisfied: babel>=2.10 in c:\python312\lib\site-packages (from jupyterlab-server<3,>=2.27.1->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.15.0)
Requirement already satisfied: fastjsonschema>=2.15 in c:\python312\lib\site-packages (from nbformat>=5.7->nbconvert->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.20.0)
Requirement already satisfied: wcwidth in c:\python312\lib\site-packages (from prompt-toolkit>=3.0.30->jupyter-console->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.2.13)
Requirement already satisfied: markdown-it-py>=2.2.0 in c:\python312\lib\site-packages (from rich>=10.11.0->typer<1.0,>=0.12->gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (3.0.0)
Requirement already satisfied: argon2-cffi-bindings in c:\python312\lib\site-packages (from argon2-cffi>=21.1->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (21.2.0)
Requirement already satisfied: parso<0.9.0,>=0.8.3 in c:\python312\lib\site-packages (from jedi>=0.16->ipython>=7.23.1->ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.8.4)
Requirement already satisfied: python-json-logger>=2.0.4 in c:\python312\lib\site-packages (from jupyter-events>=0.9.0->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.0.7)
Requirement already satisfied: rfc3339-validator in c:\python312\lib\site-packages (from jupyter-events>=0.9.0->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.1.4)
Requirement already satisfied: rfc3986-validator>=0.1.1 in c:\python312\lib\site-packages (from jupyter-events>=0.9.0->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.1.1)
Requirement already satisfied: mdurl~=0.1 in c:\python312\lib\site-packages (from markdown-it-py>=2.2.0->rich>=10.11.0->typer<1.0,>=0.12->gradio==5.23.1->qwen-agent[code_interpreter,gui,mcp,rag]) (0.1.2)
Requirement already satisfied: executing>=1.2.0 in c:\python312\lib\site-packages (from stack-data->ipython>=7.23.1->ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.1.0)
Requirement already satisfied: asttokens>=2.1.0 in c:\python312\lib\site-packages (from stack-data->ipython>=7.23.1->ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.4.1)
Requirement already satisfied: pure-eval in c:\python312\lib\site-packages (from stack-data->ipython>=7.23.1->ipykernel->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (0.2.3)
Requirement already satisfied: fqdn in c:\python312\lib\site-packages (from jsonschema[format-nongpl]>=4.18.0->jupyter-events>=0.9.0->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.5.1)
Requirement already satisfied: isoduration in c:\python312\lib\site-packages (from jsonschema[format-nongpl]>=4.18.0->jupyter-events>=0.9.0->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (20.11.0)
Requirement already satisfied: jsonpointer>1.13 in c:\python312\lib\site-packages (from jsonschema[format-nongpl]>=4.18.0->jupyter-events>=0.9.0->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (3.0.0)
Requirement already satisfied: uri-template in c:\python312\lib\site-packages (from jsonschema[format-nongpl]>=4.18.0->jupyter-events>=0.9.0->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.3.0)
Requirement already satisfied: webcolors>=24.6.0 in c:\python312\lib\site-packages (from jsonschema[format-nongpl]>=4.18.0->jupyter-events>=0.9.0->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (24.8.0)
Requirement already satisfied: arrow>=0.15.0 in c:\python312\lib\site-packages (from isoduration->jsonschema[format-nongpl]>=4.18.0->jupyter-events>=0.9.0->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (1.3.0)
Requirement already satisfied: types-python-dateutil>=2.8.10 in c:\python312\lib\site-packages (from arrow>=0.15.0->isoduration->jsonschema[format-nongpl]>=4.18.0->jupyter-events>=0.9.0->jupyter-server<3,>=2.4.0->jupyterlab->jupyter>=1.0.0->qwen-agent[code_interpreter,gui,mcp,rag]) (2.9.0.20241003)
Using cached gradio-5.23.1-py3-none-any.whl (51.3 MB)
Using cached gradio_client-1.8.0-py3-none-any.whl (322 kB)
Using cached modelscope_studio-1.1.7-py3-none-any.whl (13.9 MB)
Using cached pydantic-2.9.2-py3-none-any.whl (434 kB)
Using cached pydantic_core-2.23.4-cp312-none-win_amd64.whl (1.9 MB)
Using cached dashscope-1.23.2-py3-none-any.whl (1.3 MB)
Using cached jsonlines-4.0.0-py3-none-any.whl (8.7 kB)
Using cached pdfplumber-0.11.6-py3-none-any.whl (60 kB)
Using cached pdfminer_six-20250327-py3-none-any.whl (5.6 MB)
Using cached python_docx-1.1.2-py3-none-any.whl (244 kB)
Using cached qwen_agent-0.0.20-py3-none-any.whl (7.1 MB)
Using cached aiofiles-23.2.1-py3-none-any.whl (15 kB)
Using cached groovy-0.1.2-py3-none-any.whl (14 kB)
Using cached pypdfium2-4.30.1-py3-none-win_amd64.whl (3.0 MB)
Using cached ruff-0.11.7-py3-none-win_amd64.whl (11.6 MB)
Using cached safehttpx-0.1.6-py3-none-any.whl (8.7 kB)
Using cached semantic_version-2.10.0-py2.py3-none-any.whl (15 kB)
Using cached ffmpy-0.5.0-py3-none-any.whl (6.0 kB)
Installing collected packages: jieba, semantic-version, ruff, python-docx, pypdfium2, pydantic-core, jsonlines, groovy, ffmpy, aiofiles, pydantic, safehttpx, pdfminer.six, gradio-client, dashscope, qwen-agent, pdfplumber, gradio, modelscope-studio
  Attempting uninstall: pydantic-core
    Found existing installation: pydantic_core 2.27.2
    Uninstalling pydantic_core-2.27.2:
      Successfully uninstalled pydantic_core-2.27.2
  Attempting uninstall: aiofiles
    Found existing installation: aiofiles 24.1.0
    Uninstalling aiofiles-24.1.0:
      Successfully uninstalled aiofiles-24.1.0
  Attempting uninstall: pydantic
    Found existing installation: pydantic 2.10.6
    Uninstalling pydantic-2.10.6:
      Successfully uninstalled pydantic-2.10.6
  Attempting uninstall: pdfminer.six
    Found existing installation: pdfminer.six 20240706
    Uninstalling pdfminer.six-20240706:
      Successfully uninstalled pdfminer.six-20240706
Successfully installed aiofiles-23.2.1 dashscope-1.23.2 ffmpy-0.5.0 gradio-5.23.1 gradio-client-1.8.0 groovy-0.1.2 jieba-0.42.1 jsonlines-4.0.0 modelscope-studio-1.1.7 pdfminer.six-20250327 pdfplumber-0.11.6 pydantic-2.9.2 pydantic-core-2.23.4 pypdfium2-4.30.1 python-docx-1.1.2 qwen-agent-0.0.20 ruff-0.11.7 safehttpx-0.1.6 semantic-version-2.10.0


  WARNING: Failed to remove contents in a temporary directory 'C:\Python312\Lib\site-packages\~-dantic_core'.
  You can safely remove it manually.
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
firecrawl-py 1.12.0 requires pydantic>=2.10.3, but you have pydantic 2.9.2 which is incompatible.
graphrag 0.5.0 requires aiofiles<25.0.0,>=24.1.0, but you have aiofiles 23.2.1 which is incompatible.
openai-agents 0.0.8 requires pydantic<3,>=2.10, but you have pydantic 2.9.2 which is incompatible.
open-webui 0.6.5 requires pydantic==2.10.6, but you have pydantic 2.9.2 which is incompatible.
unstructured-client 0.32.3 requires aiofiles>=24.1.0, but you have aiofiles 23.2.1 which is incompatible.
unstructured-client 0.32.3 requires pydantic>=2.10.3, but you have pydantic 2.9.2 which is incompatible.

[notice] A new release of pip is available: 25.0.1 -> 25.1
[notice] To update, run: python.exe -m pip install --upgrade pip
```



### 2.1 Qwen-Agent构建多轮对话机器人

Qwen-Agent提供了接入Qwen系列模型，及在线API或者通过Ollama、Vllm启动本地模型等任意兼容OpenAI API接口规范的模型。其中通过`Assistant`组件，可以实现工具调用、Agent编排、MCP接入等等一系列功能。注意: 不同于类似OpenAI Agents SDK，`Assistant` 类仅仅是Qwen-Agent构建不同形式代理的一个子类，主要用于实现`tool_use`和`RAG`的工作流。

```
from qwen_agent.agents import Assistant
from qwen_agent.utils.output_beautify import typewriter_print
```

+   配置所使用的模型服务：

```
# 步骤 2：配置您所使用的 LLM。
llm_cfg = {
    # 使用 ollama 提供的模型服务：
    'model': 'qwen3:8B',
    'model_server': 'http://localhost:11434/v1',
    "api_key": "ollama",

    # （可选） LLM 的超参数：
    'generate_cfg': {
        'temperature': 0.7,
    }
}
```

+   创建一个智能体对象。

`Assistant` 类的构造函数接受以下参数，这些参数允许我们灵活地配置智能体的功能、行为和上下文。

`Assistant`参数介绍

| 参数名 | 类型 | 描述 |
| --- | --- | --- |
| `function_list` | `Optional[List[Union[str, Dict, BaseTool]]]` | 定义助手可以调用的功能列表，可以是字符串、字典或 `BaseTool` 的实例。 |
| `llm` | `Optional[Union[Dict, BaseChatModel]]` | 指定要使用的语言模型配置或实例。 |
| `system_message` | `Optional[str]` | 系统消息，用于定义助手的行为和角色，默认为 `DEFAULT_SYSTEM_MESSAGE`。 |
| `name` | `Optional[str]` | 助手的名称。 |
| `description` | `Optional[str]` | 助手的描述，提供关于助手功能的简要信息。 |
| `files` | `Optional[List[str]]` | 可以传递的文件列表，助手可以访问这些文件。 |
| `rag_cfg` | `Optional[Dict]` | 用于配置检索增强生成（RAG）功能的字典。 |

```
bot = Assistant(llm=llm_cfg,
                system_message="你是一位乐于助人的小助理",
                name="智能助理"
                )
```

+   构建多轮对话聊天

其中`typewriter_print`用于格式化和打印消息，是`Qwen-Agent`内部构造的函数，会处理函数调用和普通对话的不同执行逻辑（`Function Calling`），同时对于推理类模型，会判断消息中是否包含`reasoning_content`，如果存在，则将其添加到`content`列表中，并在前面加上`THOUGHT_S`（表示思考的符号或字符串），从而支持推理类模型和对话模型的不同输入输出形式。核心代码如下所示：

```
TOOL_CALL_S = '[TOOL_CALL]'
TOOL_CALL_E = ''
TOOL_RESULT_S = '[TOOL_RESPONSE]'
TOOL_RESULT_E = ''
THOUGHT_S = '[THINK]'

CODE_INTERPRETER = 'code_interpreter'


def typewriter_print(messages: List[dict], text: str) -> str:
    full_text = ''
    content = []
    for msg in messages:
        if msg['role'] == ASSISTANT:
            if msg.get('reasoning_content'):
                assert isinstance(msg['reasoning_content'], str), 'Now only supports text messages'
                content.append(f'{THOUGHT_S}\n{msg["reasoning_content"]}')
            if msg.get('content'):
                assert isinstance(msg['content'], str), 'Now only supports text messages'
                content.append(msg['content'])
            if msg.get('function_call'):
                if msg['function_call']['name'] == CODE_INTERPRETER:
                    try:
                        _code = json.loads(msg['function_call']['arguments'])['code']
                        content.append(f'{TOOL_CALL_S} {msg["function_call"]["name"]}\n{_code}')
                    except Exception:
                        content.append(
                            f'{TOOL_CALL_S} {msg["function_call"]["name"]}\n{msg["function_call"]["arguments"]}')

                else:
                    content.append(f'{TOOL_CALL_S} {msg["function_call"]["name"]}\n{msg["function_call"]["arguments"]}')
        elif msg['role'] == FUNCTION:
            content.append(f'{TOOL_RESULT_S} {msg["name"]}\n{msg["content"]}')
        else:
            raise TypeError
    if content:
        full_text = '\n'.join(content)
        print(full_text[len(text):], end='', flush=True)

    return full_text
```

```
# 步骤 4：作为聊天机器人运行智能体。
messages = []  # 这里储存聊天历史。
while True:
    # 输入请求 。
    query = input('\n用户请求: 输入“退出”中止对话')
    if query == '退出':
        break
    else:
        # 将用户请求添加到聊天历史。
        messages.append({'role': 'user', 'content': query})
        response = []
        response_plain_text = ''
        
        print('AI 回复:')
        for response in bot.run(messages=messages):
            # 流式输出。
            response_plain_text = typewriter_print(response, response_plain_text)
        # 将机器人的回应添加到聊天历史。
        messages.extend(response)
```

```
用户请求:  你好，请问在吗？
AI 回复:
<think>
好的，用户发来消息："你好，请问在吗？"，我需要以乐于助人的小助理身份回复。首先，要保持亲切友好的态度，用轻松的语气回应。

可以先用"在的！"来确认在线状态，然后用表情符号增添亲切感。接着询问用户有什么需要帮助的，同时保持开放式的提问，让用户有更多选择。可以加入一些表情符号，让回复更生动活泼。

要注意语气温和，避免过于正式。可以适当加入一些感叹词，让回复显得更有活力。同时要保持口语化，避免使用生硬的表达方式。

最后，要让用户感受到被重视和欢迎，可以加入一些鼓励性的话语，比如"随时欢迎你来找我聊天哦！"。整体要保持简洁自然，符合小助理的形象。
</think>

在的！👋 有什么我可以帮你的吗？😊 随时欢迎你来找我聊天哦！
用户请求:  请你介绍一下你自己吧
AI 回复:
<think>
好的，用户让我介绍一下自己。首先，我需要确保回复既友好又有信息量。作为乐于助人的小助理，应该用轻松活泼的语气，同时展示出我的功能和特点。

可以先用表情符号和亲切的问候开头，比如"👋 你好呀！"。然后简单介绍我的身份，比如"我是你的专属小助理"，这样能让人立刻知道我的角色。

接下来要突出我的功能，比如"能陪你聊天、解答问题、提供生活小技巧"，这样用户能清楚知道我能帮他们什么。同时要强调我的特点，比如"性格随和，喜欢和大家做朋友"，让互动更自然。

还要注意保持口语化，避免使用生硬的表达。比如用"有什么想聊的吗？"而不是"你有什么问题需要帮助吗？"。这样更符合小助理的形象。

最后可以加个表情符号，比如"✨"，让回复更有温度。整体要保持简洁，同时传达出热情和乐于助人的态度。这样用户会觉得亲切，也更愿意和我互动。
</think>

👋 你好呀！我是你的专属小助理，性格随和，喜欢和大家做朋友～✨  

我的功能可多了：  
- 能陪你聊天、解忧、讲故事  
- 可以帮你解答各种问题（从学习到生活）  
- 还能提供一些有趣的小技巧和冷知识  
- 最重要的是，我超喜欢和大家聊天！💬  

有什么想聊的吗？或者需要我帮你做什么？😊
```

## 3. 借助Qwen-Agent接入MCP工具

`Assistant`类中的`function_list`参数可以通过`tool_use`的方式传入`MCP`格式的调用示例，就像定义`Function Calling`的`JsonSchema`一样。其中`MCP`调用格式如下所示：

```
tools = [{
    "mcpServers": {
        "sqlite" : {
            "command": "uvx",
            "args": [
                "mcp-server-sqlite",
                "--db-path",
                "test.db"
            ]
        }
    }
}]
```

这个`MCP`服务器配置的功能是启动一个`SQLite`数据库服务器，允许通过`MCP`协议与该数据库进行交互。具体功能包括：

+   数据库连接: 允许其他服务或客户端连接到`SQLite`数据库。
+   执行 SQL 查询: 通过`MCP`协议发送`SQL`查询到`SQLite`数据库并获取结果。
+   数据管理: 允许对数据库中的数据进行增、删、改、查等操作

其中我们配置了三个参数：

+   `mcp-server-sqlite`: 这是指定要启动的`MCP`服务器类型，表示启动一个`SQLite`服务器。
+   `--db-path`: 这是一个选项，后面跟着数据库文件的路径。
+   `test.db`: 这是`SQLite`数据库的文件名，表示要使用的数据库文件。

准备好了`MCP`服务器后，接下来传递给`Assistant`对象。

`Jupyter` 使用特殊的通信架构，它的内核已经运行在一个事件循环中，负责处理前端和后端通信。这个架构使得某些底层系统操作（如创建带特定标志的子进程）无法实现，尽管通过`nest_asyncio.apply()` 可以解决嵌套事件循环问题，但是无法修复底层事件循环本身的实现缺陷。而`Qwen-Agent`底层实现的`MCP`组件管理并没有完全兼容`Jupyter`环境下的子兼容，所以创建 `MCP` 智能体的代码目前无法在`Jupyter` 环境下运行（除非修改底层源码），因此我们可以创建独立的`.py`环境直接运行。

这里我们创建了`qwen3_mcp_sqlite.py`，其核心代码如下：



# Qwen-Agent MCP 实践教程

## 3. 构建基于 MCP 的 SQLite 数据库助手

```
# qwen3_mcp_sqlite.py.py
from qwen_agent.agents.assistant import Assistant
from qwen_agent.utils.output_beautify import typewriter_print

def init_agent_service():
    # 使用 Ollama 运行的模型
    llm_cfg = {
        'model': 'Qwen3:8B',
        'model_server': 'http://localhost:11434/v1',
        'api_key': 'EMPTY',
    }

    system = ('你扮演一个数据库助手，你具有查询数据库的能力')

    tools = [{
        "mcpServers": {
            "sqlite": {
                "command": "uvx",
                "args": [
                    "mcp-server-sqlite",
                    "--db-path",
                    "test.db"
                ]
            }
        }
    }]

    bot = Assistant(
        llm=llm_cfg,
        name='数据库管理员',
        description='你是一位数据库管理员，具有对本地数据库的增删改查能力',
        system_message=system,
        function_list=tools,
    )

    return bot

def run_query(query=None):
    # 定义数据库助手
    bot = init_agent_service()

    # 执行对话逻辑
    messages = []
    messages.append({'role': 'user', 'content': [{'text': query}]})

    # 跟踪前一次的输出，用于增量打印
    previous_text = ""

    print('数据库管理员: ', end='', flush=True)

    for response in bot.run(messages):
        previous_text = typewriter_print(response, previous_text)


# 命令行接口，便于直接运行
if __name__ == '__main__':

    query = '帮我创建一个学生表,表名是students,包含id, name, age, gender, score字段,然后插入一条数据,id为1,name为张三,age为20,gender为男,score为95'

    # 执行查询
    run_query(query)
```

在这段代码中，`run` 方法在 `Agent` 类中负责处理输入消息并生成响应，包括接收消息、确定消息类型和语言、处理系统消息以及通过 LLM 生成响应的整个过程，而`typewriter_print`则用于流式的增量输出。演示视频如下：

```
from IPython.display import Video
```

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/%E6%BC%94%E7%A4%BA1_SqliteMCP%E5%8A%9F%E8%83%BD%E6%BC%94%E7%A4%BA.mkv", width=800, height=400)
```

Your browser does not support the video element.

运行结束后，在脚本执行的同级目录下会生成一个`test.db`的文件，这就是MCP自动创建的数据库文件。

![](img/3b013aa1cd7d7f432854ab7c95dcab65.png)

这里我们可以通过`sqlite3`命令行工具来查看数据库中的数据。

```
# 查看数据库中的数据
import sqlite3

conn = sqlite3.connect('E:\\mcpCourse\\Qwen-Agent-main\\test.db')
cursor = conn.cursor()

# 先查看数据库中有哪些表
cursor.execute("SELECT name FROM sqlite_master WHERE type='table';")
tables = cursor.fetchall()
print("数据库中的表:", tables)

# 如果有表，则查询第一个表的数据
if tables:
    table_name = tables[0][0]
    cursor.execute(f"SELECT * FROM {table_name}")
    print(f"{table_name} 表中的数据:", cursor.fetchall())
else:
    print("数据库中没有表，需要先创建表并插入数据")

conn.close()
```

```
数据库中的表: [('students',)]
students 表中的数据: [(1, '张三', 20, '男', 95)]
```

> 脚本已上传至课件网盘：

![](img/ee9908215b00ef5444a1c7891859e8a8d.png)

> 扫码即可领取：

![](img/ef215029513ff4833af783222e7d636a.png)

## 4. MCP服务器+Qwen-Agent自带工具联合运行

大模型不擅长数学运算、数据可视化等精确计算任务，因此我们一般会使用代码解释器，使智能体编写和运行 Python 程序，从而逐步解决复杂的数据问题。如果智能体编写的代码未能成功运行，它还会不断调整代码，尝试不同的方法，直到代码能够顺利执行。

![](img/d0e2ae55edf91cdd9f84bc828eca9aef.png)

Qwen-Agent内置集成了`code_interpreter`组件，因此需要先执行安装，执行如下命令：

```
pip install "qwen-agent[code_interpreter]"
```

调用`code_interpreter`的方法同样是需要在构建`Assistant`的时候，定义在`tools`列表中，并通过`Function_list`参数传递。

```
tools = [{
    "mcpServers": {
        "sqlite": {
            "command": "uvx",
            "args": [
                "mcp-server-sqlite",
                "--db-path",
                "test.db"
            ]
        },
    },
    # pip install "qwen-agent[code_interpreter]"
    'code_interpreter',
]
```

这里我们创建了`qwen3_mcp_multi.py`文件，核心代码如下：

```
# qwen3_mcp_multi.py
from qwen_agent.agents.assistant import Assistant
from qwen_agent.utils.output_beautify import typewriter_print

def init_agent_service():

    # 使用 Ollama 运行的模型
    llm_cfg = {
        'model': 'Qwen3:8B',
        'model_server': 'http://localhost:11434/v1',
        'api_key': 'EMPTY',
    }

    system = ('你是一个数据分析师，具有对本地数据库的增删改查能力，同时具有使用python代码的能力')

    tools = [{
        "mcpServers": {
            "sqlite": {
                "command": "uvx",
                "args": [
                    "mcp-server-sqlite",
                    "--db-path",
                    "test.db"
                ]
            },
        },
    },
    # pip install "qwen-agent[code_interpreter]"
    'code_interpreter',
    ]

    bot = Assistant(
        llm=llm_cfg,
        name='数据分析师',
        description='可以对本地数据库进行增删改查，也',
        system_message=system,
        function_list=tools,
    )

    return bot

def run_query(query=None):
    # 定义数据库助手
    bot = init_agent_service()

    # 执行对话逻辑
    messages = []
    messages.append({'role': 'user', 'content': [{'text': query}]})

    # 跟踪前一次的输出，用于增量打印
    previous_text = ""

    print('数据库管理员: ', end='', flush=True)

    for response in bot.run(messages):
        previous_text = typewriter_print(response, previous_text)


# 命令行接口，便于直接运行
if __name__ == '__main__':

    query = '帮我随机生成20条学生的成绩数据, 插入到students表中,\
            然后绘制一个折线图展示所有成绩变化趋势'

    # 执行查询
    run_query(query)
```

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/%E6%BC%94%E7%A4%BA2_%E5%A4%9AMCP%E6%89%A7%E8%A1%8C.mp4", width=800, height=400)
```

Your browser does not support the video element.

> 脚本已上传至课件网盘：

![](img/b8fa54de2a9fe6bdd77dea2178670c8b.png)

> 扫码即可领取：

![](img/bb88d164c46871b8a0a1084026cba28c.png)

## 5. 搭建Multi-MCP Agent进行任务执行

接下来构建Multi-MCP `Agent` 多轮对话执行，并接入高德MCP服务器完成旅行规划助理。

首先可以在github上找到公开的高德MCP服务器：https://github.com/sugarforever/amap-mcp-server

![](img/4a9fa5924a878fbf0e59324d05401e22.png)

然后，需要在高德开放中心注册API进行使用：https://lbs.amap.com/

![](img/a7cddcb309faf1d6099dad0574e22168.png)

创建应用并生成一个有效的`key`：

![](img/de6d7d7dc6bc9b994d23a7c9e7b5f194.png)

![](img/24fd94ab8c5d03379e891ade53612abe.png)

然后在`tools`列表中添加高德MCP服务器：

```
tools = [{
    "mcpServers": {
        "sqlite": {
            "command": "uvx",
            "args": [
                "mcp-server-sqlite",
                "--db-path",
                "test.db"
            ]
        },
        "amap-mcp-server": {
            "command": "uvx",
            "args": [
                "amap-mcp-server"
            ],
            "env": {
                "AMAP_MAPS_API_KEY": "9b555d3ff4976"  # 这里替换成注册的高德API
            }
        }
    }
},
'code_interpreter',
]
```

+ `Firecrawl MCP Server`

接下来我们需要找一个能抓取网页数据的MCP，这里可以选择`Firecrawl MCP Server`：https://github.com/mendableai/firecrawl-mcp-server

`Firecrawl MCP Server`实现与 Firecrawl 集成以实现网页抓取功能。支持的功能如下：

+ 抓取、爬取、搜索、提取、深入研究和批量抓取支持

+ 使用 JS 渲染进行网页抓取

+ URL 发现和抓取

+ 带有内容提取的网页搜索

+ .....



# Firecrawl API 注册与 MCP 工具配置

## 注册 Firecrawl API

需要注册Firecrawl的API：`https://www.firecrawl.dev/signin/password_signin`

![](img/b1d09f3b7750c7b645445f853a7927f1.png)

## 创建 API Keys

按照如下位置进行`API_Keys`的创建：

![](img/c61a73a954da99e188c90d7a28cc3b49.png)

## 配置 Tools 列表

同样，需要添加到`Tools`列表中，整体配置如下：

```
tools = [{
    "mcpServers": {
        "sqlite" : {
            "command": "uvx",
            "args": [
                "mcp-server-sqlite",
                "--db-path",
                "test.db"
            ]
        },
        "amap-mcp-server": {
            "command": "uvx",
            "args": [
                "amap-mcp-server"
            ],
            "env": {
                "AMAP_MAPS_API_KEY": "9b555d3ff4976"   # 这里替换成注册的高德API
            }
        },
        
        "firecrawl-mcp": {
            "command": "npx",
            "args": ["-y", "firecrawl-mcp"],
            "env": {
                "FIRECRAWL_API_KEY": "fc-724e84ead993ba1"   # 这里需要替换成注册的 firecrawl Key
            },
        },
    },
},
'code_interpreter',
]
```

## 使用 mcp-server-fetch 与定义 qwen3_mcp_apply.py

除此以外，大家也可以使用 `mcp-server-fetch`：`https://github.com/modelcontextprotocol/servers/tree/main/src/fetch` ，大家可以根据自己的情况灵活选择。最终我们定义`qwen3_mcp_apply.py`文件，如下所示：

![image-20250430175345970](https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/image-20250430175345970.png)
![0ddb8b8061004446f889128b5702d48](https://ml2022.oss-cn-hangzhou.aliyuncs.com/img/0ddb8b8061004446f889128b5702d48.jpg)
![202412191720637](https://muyu20241105.oss-cn-beijing.aliyuncs.com/images/202504301300790.png)
![202412191720637](https://muyu20241105.oss-cn-beijing.aliyuncs.com/images/202504301303817.png)

## Python 代码实现

### 初始化 Agent 服务

```python
def init_agent_service():
    # 使用 Ollama 运行的模型
    llm_cfg = {
        'model': 'Qwen3:8B',
        'model_server': 'http://localhost:11434/v1',
        'api_key': 'EMPTY',
    }

    tools = [{
        "mcpServers": {
            'time': {
                'command': 'uvx',
                'args': ['mcp-server-time', '--local-timezone=Asia/Shanghai']
            },
            # "fetch": {
            #     "command": "uvx",
            #     "args": ["mcp-server-fetch"]
            # },
            
            "sqlite" : {
                "command": "uvx",
                "args": [
                    "mcp-server-sqlite",
                    "--db-path",
                    "test.db"
                ]
            },

            "firecrawl-mcp": {
                "command": "npx",
                "args": ["-y", "firecrawl-mcp"],
                "env": {
                    "FIRECRAWL_API_KEY": "fc-724e84ee49fad993ba1"
                },
            },

            "amap-mcp-server": {
                "command": "uvx",
                "args": [
                    "amap-mcp-server"
                ],
                "env": {
                    "AMAP_MAPS_API_KEY": "9b555d31b15968aff4976"
                }
            }
        },
        
    },
    # pip install "qwen-agent[code_interpreter]"
    'code_interpreter',
    ]

    system = """
    你是一个规划师和数据分析师 \
        你可以调用高德地图规划旅行路线，同时可以提取网页信息进行数据分析
    """

    bot = Assistant(
        llm=llm_cfg,
        name='智能助理',
        description='具备查询高德地图、提取网页信息、数据分析的能力',
        system_message=system,
        function_list=tools,
    )

    return bot
```

### 运行查询

```python
def run_query(query=None):
    # 定义数据库助手
    bot = init_agent_service()

    from qwen_agent.gui import WebUI

    chatbot_config = {
        'prompt.suggestions': [
            '请你帮我写一个关于人工智能的论文',
            "https://github.com/orgs/QwenLM/repositories 提取这一页的Markdown 文档，然后绘制一个柱状图展示每个项目的收藏量",
            '帮我查询从故宫去颐和园的路线',
        ]
    }
    WebUI(
        bot,
        chatbot_config=chatbot_config,
    ).run()
```

## 启动 WebUI 与访问

> 脚本已上传至课件网盘：
> 扫码即可领取：

这里我们要应用到`Qwen-Agent`内置的`WebUI`，其实现了一个基于`Gradio`构建的前端页面，通过执行 `qwen3_mcp_apply`，启动后即可在 `localhost:8000` 访问：

此时通过浏览器访问：`http://localhost:7860/` 即可进行使用。

- 官网查询GitHub项目历史星标项目复现

## 视频演示

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/Qwen-Agent%20%E5%A4%8D%E7%8E%B0%E6%BC%94%E7%A4%BA2.mp4", width=800, height=400)
```

Your browser does not support the video element.

```
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/Qwen-Agent%20%E5%A4%8D%E7%8E%B0%E6%BC%94%E7%A4%BA1.mp4", width=800, height=400)
```

Your browser does not support the video element.
