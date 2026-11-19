
# RAGapp：使用Agentic RAG最简单的方法

> 来源：[https://geekbang.feishu.cn/docx/CUk7dQFaCoCiVQx0hq4cVOU9n0g](https://geekbang.feishu.cn/docx/CUk7dQFaCoCiVQx0hq4cVOU9n0g)

**板块主理人：尼福财**

·向量光年科技有限公司创始人

·《开源AI项目落地》👸号及社群主理人

专注于AI产品开发落地，移动端、PC端软件快速高质量交付

愿景：加速全行业向AI转变

![](img/ccec019a3c020683a3d67000b28760ae.png)

## 项目简介

RAGapp 是一个高效的企业级应用程序，使得在任何企业中使用 Agentic RAG 变得简单直接。通过 `Docker` 部署在自己的云基础设施中，RAGapp 能够与 `OpenAI` 或 `Gemini` 的托管 AI 模型以及使用 `Ollama` 的本地模型无缝集成。用户可以通过简单的配置，启动 `docker` 容器即可运行 RAGapp，访问管理界面进行设置。此外，RAGapp 提供完全开放的 API 和用户界面，使得自定义和扩展应用成为可能。

**扫码加入AI交流群**

**获得更多技术支持和交流**

**（请注明自己的职业）**

![](img/b950f43cd0e7ea65b8cc34eeb12c8972.png)

## UI

![](img/e1d9b0a3ee32deb87c84a8388ffead8b.png)

## 快速使用

要运行，请使用镜像启动一个 `Docker` 容器：

```
docker run -p 8000:8000 ragapp/ragapp
```

然后，访问 `http://localhost:8000/admin` 来配置你的 RAGapp。

你可以使用 `OpenAI` 或 `Gemini` 的托管 AI 模型，也可以使用 `Ollama` 的本地模型。

注意：为了避免遇到任何错误，建议使用 `Docker` 的最新版本，如有需要，还应使用 `Docker Compose`。

## 端点

`Docker` 容器公开了以下端点：

- `http://localhost:8000/admin`：管理员界面
- `http://localhost:8000`：聊天界面
- `http://localhost:8000/docs`：API 文档

注意：只有在 RAGapp 配置后，聊天界面和 API 才能正常工作。

RAGapp 设计上不包含任何身份验证层。只需在你的云环境中保护 `/admin` 路径，以确保你的 RAGapp 的安全。

## 部署

使用 `Docker Compose`

提供了一个 `docker-compose.yml` 文件，使得在你自己的基础设施中部署带有 `Ollama` 和 `Qdrant` 的 RAGapp 变得容易。

使用 `MODEL` 环境变量，你可以指定使用哪个模型，例如 `llama3`：

```
MODEL=llama3 docker-compose up
```

如果你没有指定 `MODEL` 变量，使用的默认模型是 `phi3`，它的功能比 `llama3` 弱，但下载速度更快。

注意：`docker-compose.yml` 文件中的 `setup` 容器将下载所选模型到 `ollama` 文件夹中——这将花费几分钟时间。

使用 `OLLAMA_BASE_URL` 环境变量，你可以指定使用哪个 `Ollama` 主机。如果你没有指定 `OLLAMA_BASE_URL` 变量，默认指向由 `Docker Compose` 启动的 `Ollama` 实例 (`http://ollama:11434`)。

如果你正在运行本地的 `Ollama` 实例，你可以通过设置 `OLLAMA_BASE_URL` 变量为 `http://host.docker.internal:11434` 来连接它到 RAGapp：

```
MODEL=llama3 OLLAMA_BASE_URL=http://host.docker.internal:11434 docker-compose up
```

如果你在 macOS 上运行 RAGapp，这是必要的，因为 `Docker for Mac` 不支持 GPU 加速。

为了在 Linux 上启用 `Docker` 对 NVIDIA GPUs 的访问，安装 `NVIDIA Container Toolkit`。

## 项目链接

https://github.com/ragapp/ragapp

关注「开源AI项目落地」👸号

与AI时代更靠近一点
