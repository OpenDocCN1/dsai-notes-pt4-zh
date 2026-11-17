
# 二次开发Open WebUI项目构建专属智能客服系统

> 来源：`https://kq4b3vgg5b.feishu.cn/docx/JoWVduM2KoZnXPxGVaacG2Klneh`

## 《大模型与Agent开发实战》（12月班）

## Agent智能体开发实战体验课 - 从零搭建智能客服系统

本期公开课（连载三天）我们将实现这样一个智能客服系统：

![](img/d0d2ed35792e9501987733dc58cf85c3.png)

在这三天中我给大家准备了非常丰富的资料，在【完课】后可以找助教老师领取~

![](img/2331489fd5309f6b10b570b4634a1843.png)

## Day 3. 二次开发Open WebUI项目构建专属智能客服系统

### 课程目录

1. Open WebUI项目回顾

   - Open WebUI 项目概览
   - Open WebUI 项目的个性化定制方法

1. Ollama项目介绍与核心功能速览

   - Ollama项目介绍
   - Ollama运行所需硬件支持
   - Ollama快速使用方法
   - 如何使用Ollama启动`Qwen2.5`并接入Open WebUI项目

1. 基于Open WebUI 定制化私有智能客服

   - Step 1.不同大模型的接入方法
   - Step 2.通过 `System Message`设定身份背景
   - Step 3.构建私有工具库
   - Step 4.工具的绑定方法
   - Step 5.问答效果测试

1. 大模型与Agent学习路径规划

   - 多样化的大模型怎么选择
   - Single-Agent的局限
   - 学习Multi-Agent的关键
   - Agent开发的不同方式
   - 热门Agent开发框架的分类

### 1. Open WebUI项目回顾

![](img/63c85ad3bafb5bd503798078bc6fe7d6.png)

> Open-webui Github：`https://github.com/open-webui/open-webui`

🚀 轻松安装：通过 Docker 或 Kubernetes（`kubectl`、`kustomize` 或 `helm`）实现无缝安装，支持 `:ollama` 和 `:cuda` 标签镜像，带来无忧体验。

🤝 Ollama/OpenAI API 集成：轻松集成与 OpenAI 兼容的 API，支持与 Ollama 模型的多样化对话。自定义 OpenAI API URL，可与 LMStudio、GroqCloud、Mistral、OpenRouter 等平台对接。

🛡️ 细粒度权限和用户组：管理员可以创建详细的用户角色和权限，确保用户环境的安全性。这种细粒度管理不仅增强了安全性，还允许定制化的用户体验，培养用户的责任感与归属感。

📱 响应式设计：在桌面PC、笔记本和移动设备上都能享受无缝的使用体验。

✒️🔢 完整的 Markdown 和 LaTeX 支持：通过全面的 Markdown 和 LaTeX 支持，提升 LLM（大语言模型）体验，让交互更为丰富。

🎤📹 免提语音/视频通话：享受无缝的语音和视频通话功能，提供更为动态和互动的聊天环境。

🛠️ 模型构建器：通过 Web UI 轻松创建 Ollama 模型。创建并添加自定义角色/代理，定制聊天元素，并通过 Open WebUI 社区集成轻松导入模型。

🐍 原生 Python 函数调用工具：通过内置代码编辑器支持，增强 LLM 功能。通过“Bring Your Own Function” (BYOF) 轻松集成自己的 Python 函数，与 LLM 无缝对接。

📚 本地 RAG 集成：体验未来的聊天互动，开创性的检索增强生成（RAG）支持，让文档互动无缝融入聊天体验。您可以直接将文档加载到聊天中，或通过 `#` 命令将文件添加到文档库，方便在查询时访问。

🔍 Web 搜索集成 RAG：通过 SearXNG、Google PSE、Brave Search、serpstack、serper、Serply、DuckDuckGo、TavilySearch、SearchApi 和 Bing 等搜索引擎提供商执行 Web 搜索，并将结果直接注入到您的聊天体验中。

🌐 Web 浏览功能：通过 `#` 命令加 URL，无缝集成网站内容到聊天体验中，增强互动的丰富性和深度。

🎨 图像生成集成：通过 AUTOMATIC1111 API 或 ComfyUI（本地）以及 OpenAI 的 DALL-E（外部）等选项，轻松集成图像生成功能，为聊天体验增添动态视觉内容。

⚙️ 多模型对话：轻松与多种模型同时互动，利用各自的优势提供最优回应。通过并行利用多种模型，提升聊天效果。

🔐 基于角色的访问控制（RBAC）：确保安全的访问控制，只有授权用户才能访问 Ollama，模型创建/拉取权限仅限管理员。

🌐🌍 多语言支持：体验您喜欢的语言版本的 Open WebUI，我们提供国际化（i18n）支持，积极拓展更多语言！欢迎加入我们成为贡献者！

🧩 管道与 Open WebUI 插件支持：通过管道插件框架，无缝集成自定义逻辑和 Python 库到 Open WebUI。启动您的管道实例，设置 OpenAI URL 为管道 URL，探索无限可能性。功能示例包括函数调用、用户速率限制、使用监控（如 Langfuse）、实时翻译（如 LibreTranslate 多语言支持）、有害信息过滤等。

🌟 持续更新：我们承诺定期更新 Open WebUI，提供修复和新增功能，不断优化用户体验。

这里我们是可以关注到，Open WebUI项目支持接入在线大模型和开源大模型，比如 OpenAI、`GLM4`、`Qwen2.5`等等，均可以通过 Open WebUI 的 `Functions` 功能来灵活的接入，支持一键导入配置，比如：

> Open WebUI Functions：`https://openwebui.com/functions`

![](img/560c1c9a8ea3d3d0707607d3444985a2.png)

在 Open WebUI 项目中，`Functions`（功能）通常是指该项目提供的可以执行特定任务或操作的模块或方法。这些功能可以帮助用户与大模型进行交互、配置模型参数、获取输出结果等。具体来说，这些 `Functions` 包括文本生成、模型参数调节、会话管理功能、API 接口、模型选择和管理等实现细节。

![](img/971d05a992f61aabe3344486418532a2.png)

相较在线模型，开源大模型的部署和使用则会更加复杂，会涉及大模型的私有化部署，再通过命令行、网页端访问和API调用等多种不同的方式与部署的大模型进行交互。但实际上，无论是基于大模型去做任何形式的应用开发过程，仅停留在这个阶段肯定无法满足实际的需求。关于主流的开闭源大模型，在我们的正式课程《大模型与Agent 开发实战课》中均有非常详细的讲解，如果有系统性学习大模型技术需求的小伙伴，可以扫码咨询正式课程。

《大模型与Agent开发实战课》课程原价5999，今天双十二返场秒杀最后4小时，立减2000，木羽老师直播间专属优惠券1000，仅需2999即可入学，【仅限前10名】详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/b2d950ea589cb21399051e6bcf994a54.png)

这里我们就借助`Functions`功能给大家在Open WebUI项目中接入了一个免费可调用的大模型，其`Functions` 详细代码如下图所示：
