
# 如何在本地快速搭建和运行 NLWeb 服务器的步骤

> 来源：[https://qianniucity.feishu.cn/docx/ImKLd7yYPodCgoxjaPQcxOSOnch](https://qianniucity.feishu.cn/docx/ImKLd7yYPodCgoxjaPQcxOSOnch)

主要内容总结如下：

## 前提条件

需要本地安装 Python 3.10 及以上版本。

## 安装与配置流程

1. 克隆或下载 NLWeb 代码仓库，并进入项目目录。
1. 创建并激活 Python 虚拟环境。
1. 进入 `code` 目录，安装依赖（包括本地向量数据库）。
1. 复制 `.env.template` 为 `.env`，并填写 LLM（大语言模型）API key，本地 Qdrant 配置已默认设置。
1. 根据实际情况修改 `code/config` 目录下的配置文件（如 `LLM`、`embedding`、`retrieval` 的 `provider`）。
1. 使用 `db_load` 工具加载测试数据（如示例 RSS 源），可加载多个“站点”数据，支持多源检索。

## 启动与访问

1. 在 `code` 目录下运行 `python app-file.py` 启动服务器。
1. 在浏览器访问 `http://localhost:8000/`，即可体验本地搜索和对话功能。

## 补充说明

1. 可通过修改 `config_nlweb.yaml` 配置检索范围。
1. 支持加载多种数据格式和多站点数据，便于扩展和测试。

整体流程简单明了，适合新用户快速上手 NLWeb 项目。
