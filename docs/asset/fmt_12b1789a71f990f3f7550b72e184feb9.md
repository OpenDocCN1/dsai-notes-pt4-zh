
# GraphRAG-Local - 基于 GraphRAG 支持本地的聊天

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LPXedeRmTogPY7xGL11cVTpOnCh](https://qqs7y1hozd1.feishu.cn/docx/LPXedeRmTogPY7xGL11cVTpOnCh)

## 一、关于 GraphRAG-Local

GraphRAG-Local，是带有索引/提示-调整和查询/聊天！该项目是微软GraphRAG的改编，专为支持本地模型而定制，并具有全面的交互式用户交互界面生态系统。

特点：

以API为中心的架构：强大的基于`FastAPI`的服务器（`api.py`），作为GraphRAG操作的核心。

专用索引和提示调整用户界面：一个单独的基于图形的界面（`index_app.py`），用于管理索引和提示调整过程。

本地模型支持：利用本地模型进行LLM和嵌入，包括与Ollama和OpenAI兼容的API的兼容性。

经济高效：通过使用您自己的本地模型消除对昂贵的基于云的模型的依赖。

交互式UI：用于管理数据、运行查询和可视化结果（主应用程序）的用户友好界面。

实时图形可视化：使用`Ploly`（主应用程序）以2D或3D形式可视化您的知识图。

文件管理：直接从UI上传、查看、编辑和删除输入文件。

设置管理：通过UI轻松更新和管理您的GraphRAG设置。

输出探索：浏览和查看索引输出和工件。

日志记录：用于更好调试和监控的实时日志记录。

灵活查询：支持具有可自定义参数的全局、本地和直接聊天查询（主应用程序）。

可定制的可视化：调整图形布局、节点大小、颜色等以适应您的偏好（主应用程序）。

## 二、安装和设置

按照以下步骤设置和运行GraphRAG本地生态系统：

1、创建并激活一个新的`conda`环境：

```
conda create -n graphrag-local -y
conda activate graphrag-local
```

2、安装所需的包：

首先从此存储库安装`GraphRAG`目录（Microsoft存储库中不存在更改）：

```
pip install -e ./graphrag
```

然后安装其余的依赖项：

```
pip install -r requirements.txt
```

3、启动API服务器：

```
python api.py --host 0.0.0.0 --port 8012 --reload
```

4、如果使用Ollama进行嵌入，请启动嵌入代理：

```
python embedding_proxy.py --port 11435 --host http://localhost:11434
```

注意：有关将Ollama嵌入与GraphRAG一起使用的详细说明，请参阅`EMBEDDING_PROXY_README.md`文件。

5、启动索引和提示调整UI：

```
gradio index_app.py
```

6、启动主交互式UI（旧版应用程序）：

```
gradio app.py
```

或者

```
python app.py
```

## 三、使用入门🚀

GraphRAG专为灵活性而设计，允许您快速创建和初始化自己的索引目录。按照以下步骤设置您的环境：

### 1、创建索引目录

此repo带有一个预制的索引文件夹，但您可能想制作自己的索引文件夹，因此以下是步骤。首先，为输入数据和索引结果创建所需的目录结构：

```
mkdir -p ./indexing/input
```

该目录将存储：

+ 输入`.txt`文件进行索引
+ 输出结果
+ 提示及时调整

### 2、添加示例数据（可选）

如果您想从示例数据开始，请将其复制到您的新输入目录：

```
cp input/* ./indexing/input
```

您还可以将自己的`.txt`文件添加到此目录以进行索引。

### 3、初始化索引文件夹

运行以下命令以使用所需文件初始化`./index`文件夹：

```
python -m graphrag.index --init --root ./indexing
```

### 4、配置设置

将预先配置好的`settings.yaml`文件移动到索引目录：

```
mv settings.yaml ./indexing
```

此文件包含主要配置，预设用于本地模型。

### 5、定制

您可以通过修改以下环境变量来自定义设置：

+ `ROOT_DIR`：指向您的主索引目录
+ `INPUT_DIR`：指定输入文件的位置

## 四、GraphRAG应用生态系统 🖥️

GraphRAG Local UI生态系统由三个主要组件组成，每个组件在知识图谱创建和查询过程中都有特定用途：

### 1、核心API（`api.py`）

该`api.py`文件作为GraphRAG系统的主干，提供了一个强大的基于`FastAPI`的服务器来处理所有核心操作。

主要特点：

+ 管理索引和提示调整过程
+ 处理各种查询类型（本地、全局和直接聊天）
+ 与本地LLM和嵌入模型集成
+ 为文件管理和系统配置提供端点

用法：

```
python api.py --host 0.0.0.0 --port 8012 --reload
```

注意：如果使用Ollama进行嵌入，请确保在`api.py`旁边运行嵌入代理（`embedding_proxy.py`）。有关详细说明，请参阅`EMBEDDING_PROXY_README.md`。

### 2、索引和提示调整UI（`index_app.py`）

工作流集成

启动Core API（`api.py`）以启用后端功能。

如果使用Ollama进行嵌入，请启动嵌入代理（`embedding_proxy.py`）。

使用索引和提示调整UI（`index_app.py`）准备数据并微调系统。

（可选）使用主交互式UI（`app.py`）进行可视化和遗留功能。

这种模块化方法为GraphRAG系统提供了更大的灵活性和更容易的维护。随着开发的继续，`app.py`的功能将逐渐集成到与核心应用编程接口互的新的专用接口中。

### 2、索引和提示调整UI（`index_app.py`）

该`index_app.py`文件提供了一个用户友好的Gradio界面，用于管理索引和提示调整过程。

主要特点：

+ 配置和运行索引任务
+ 设置并执行提示调整
+ 管理输入文件并探索输出数据
+ 调整LLM和嵌入设置

用法：

```
python index_app.py
```

### 3、Main Interactive UI (Legacy App) (`app.py`)

该`app.py`文件是预先存在的主应用程序，正在逐步淘汰，但仍提供有用的功能。

主要特点：

+ 以2D或3D形式可视化知识图
+ 运行查询并查看结果
+ 管理GraphRAG设置
+ 探索索引数据

用法：

```
python app.py
```

或者

```
gradio app.py
```

工作流集成

启动Core API（`api.py`）以启用后端功能。

使用索引和提示调整UI（`index_app.py`）准备数据并微调系统。

（可选）使用主交互式UI（`app.py`）进行可视化和遗留功能。

这种模块化方法为GraphRAG系统提供了更大的灵活性和更容易的维护。随着开发的继续，`app.py`的功能将逐渐集成到与核心应用编程接口互的新的专用接口中。

## 五、其它

### 📚引用

+ 微软的原始GraphRAG存储库：GraphRAG
+ 这个项目获得了灵感，并使用win4r的`GraphRAG4OpenWebUI`存储库作为API实现的起点。

### 故障排除

如果您在使用新API或索引UI时遇到任何问题，请检查控制台日志以获取详细的错误消息。

对于主应用程序，如果您无法运行`gradio app.py`，请尝试运行`pip install --upgrade gradio`，然后退出并启动一个新终端。然后它应该作为Gradio应用程序正常加载和启动。

在Windows上，如果遇到编码/UTF错误，可以在YAML设置菜单中将其更改为正确的格式。

对于任何问题或功能请求，请在GitHub存储库上打开一个问题。快乐的知识图表！
