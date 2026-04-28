# Ollama：开源平台

> 来源：[https://qianniucity.feishu.cn/docx/ZduRdbBCXopSyTxlPipcczaPnRd](https://qianniucity.feishu.cn/docx/ZduRdbBCXopSyTxlPipcczaPnRd)

![](img/c9a1a3df71167a408801560585f6797a.png)

> **Ollama** 是一个开源的大语言模型本地运行平台，它让你可以 轻松在本地加载、管理并运行各种大语言模型（LLMs），就像运行 Docker 容器一样简单。Ollama 的出现，大大降低了普通用户使用本地 AI 模型的门槛，成为当前最受欢迎的本地 AI 工具之一。

## **🔧 核心功能**

### **🧱 一键拉取 & 运行模型**

Ollama 支持用类似于 docker run 的命令方式拉取和启动模型。例如：

```
ollama run llama3
```

只需一条命令，你就能在本地运行 Meta 最新发布的 LLaMA 3 模型。

### **📦 模型打包与管理**

Ollama 提供一种类似于 Dockerfile 的 Modelfile 格式，让你可以自定义构建模型，包括使用 LoRA 微调、添加系统提示等。

```
FROM llama3
SYSTEM &quot;You are a helpful assistant.&quot;
```

### **🔁 本地 API 接口**

Ollama 启动后会暴露一个 RESTful API 服务，支持 POST /api/generate 等接口，开发者可以非常方便地将本地模型集成进自己的应用中。

### **🧠 支持众多模型**

Ollama 官方和社区支持的模型种类丰富，包括但不限于：

+   LLaMA / LLaMA 2 / LLaMA 3

+   Mistral / Mixtral

+   **Gemma**

+   **Codellama**

+   **Neural Chat**

+   **Phi / TinyLLaMA /** Dolphin / StarCoder 等

## **⚙️ 安装方式**

安装 Ollama 十分简单，支持 macOS、Windows 和 Linux 系统。你只需访问官网下载安装程序即可：

👉 https://ollama.com

也可以使用命令行方式安装：

```
curl -fsSL https://ollama.com/install.sh | sh
```

安装后，你就可以通过命令行拉取模型、运行对话或集成到应用里。

## **🔒 数据隐私友好**

**Ollama** 所有模型运行、数据处理 都在本地完成，不依赖任何云端资源。这意味着你的数据不会上传，也不会被记录或分析，非常适合隐私要求较高的用户或企业使用。

## **🧩 常见集成场景**

+   搭配 **Open WebUI** 构建本地 ChatGPT

+   与 **Cline** 等 AI 编程助手集成，提供智能补全

+   与 Notion AI、Obsidian 搭建本地知识助手

+   嵌入浏览器插件、Shell 脚本、移动设备等

---

## 🆚 Ollama vs 其他平台对比

**Ollama** 就像是本地大模型的 Docker，它让部署、运行、切换、管理模型变得轻而易举。对于想要在本地构建私有化 AI 助手、内容生成系统、自动化工具的开发者来说，Ollama 是当前最佳的选择之一。

✅ 支持中文界面

✅ 社区活跃，持续更新

✅ 跨平台支持，轻量好用

---

如你需要我为这部分内容配套写一篇更“公众号风格”的文章、配图教程、或做成视频脚本，也欢迎继续发消息给我，我可以帮你完整整理一套内容~