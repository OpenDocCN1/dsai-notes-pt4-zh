
# 官方推荐Dify小工具

> 来源：[https://yunyinghui.feishu.cn/docx/YbHNd2q5Qofx0RxDfv5cpvkenVf](https://yunyinghui.feishu.cn/docx/YbHNd2q5Qofx0RxDfv5cpvkenVf)

---

## 1、Google搜索

![](img/1c42e3b6cebd3ebf6eb533bde8091cf9.png)

Google 搜索工具能够帮助你在使用 LLM 应用的时候，获取联网搜索结果。以下是在 Dify 中配置和使用 Google 搜索工具的步骤。

1. 申请 Serp API Key

请在 [Serp 平台](https://serpapi.com/dashboard)申请 API Key。

1. 在 Dify 内填写配置

在 Dify 导航页内轻点 工具 > Google > 去授权 填写 API Key。

1. 使用工具

你可以在以下应用类型中使用 Google 工具。

+   Chatflow / Workflow 应用

Chatflow 和 Workflow 应用均支持添加 Google 工具节点。

+   Agent 应用

在 Agent 应用内添加 Google 工具，然后输入在线搜索指令，调用此工具。

## 2、Perplexity Search

![](img/e2303256371926932a35dd348578dace.png)

Perplexity 是一个基于 AI 的搜索引擎，能够理解复杂的查询并提供准确、相关的实时答案。以下是在 Dify 中配置和使用 Perplexity Search 工具的步骤。

1. 申请 Perplexity API Key

请在 [Perplexity](https://www.perplexity.ai/settings/api)申请 API Key，并确保账户内有足够的 Credits。

1. 在 Dify 内填写配置

在 Dify 导航页内轻点 工具 > Perplexity > 去授权 填写 API Key。

1. 使用工具

你可以在以下应用类型中使用 Perplexity Search 工具。

+   Chatflow / Workflow 应用

Chatflow 和 Workflow 应用均支持添加 Perplexity 工具节点。将用户输入的内容通过变量传递至 Perplexity 工具节点内的“查询”框中，按照需求调整 Perplexity 工具的内置参数，最后在“结束”节点的回复框中选中 Perplexity 工具节点的输出内容。

## 3、Stable Diffusion

![](img/9634ee294da9f2c7f2ab918a1364e2fa.png)

Stable Diffusion 是一种基于文本提示生成图像的工具，Dify 已经实现了访问 Stable Diffusion WebUI API 的接口，因此你可以直接在 Dify 中使用它。以下是在 Dify 中集成 Stable Diffusion 的步骤。

1. 初始化本地环境

推荐使用装有较强 GPU 的机器来安装和驱动 Stable Diffusion，但这并不是必须的，你也可以使用 CPU 来生成图像，但速度可能会很慢。

1. 安装并启动 Stable Diffusion WebUI

1. 从[官方仓库](https://github.com/AUTOMATIC1111/stable-diffusion-webui)克隆 Stable Diffusion WebUI 仓库

```
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui
```

1. 运行命令启动 Stable Diffusion WebUI。

克隆仓库后，切换到仓库目录。根据系统的不同，可能需要使用不同的命令来启动 WebUI。

### Windows

```
cd stable-diffusion-webui./webui.bat --api --listen
```

### Linux

```
cd stable-diffusion-webui./webui.sh --api --listen
```

1. 准备模型

现在你可以根据终端中显示的地址在浏览器中访问 Stable Diffusion WebUI，但模型还不可用。你需要从 HuggingFace 或其他来源下载模型，并将其放在 `stable-diffusion-webui` 的 `models` 目录中。例如，我们使用 pastel-mix 作为模型，使用 `git lfs` 下载模型并将其放在 `stable-diffusion-webui` 的 `models` 目录中。

```
git clone https://huggingface.co/JamesFlare/pastel-mix
```

1. 获取模型名称

现在你可以在模型列表中看到 pastel-mix，但我们仍然需要获取模型名称，访问 `http://your_id:port/sdapi/v1/sd-models`，你将看到如下的模型名称。

```
[    {        &quot;title&quot;: &quot;pastel-mix/pastelmix-better-vae-fp32.ckpt [943a810f75]&quot;,        &quot;model_name&quot;: &quot;pastel-mix_pastelmix-better-vae-fp32&quot;,        &quot;hash&quot;: &quot;943a810f75&quot;,        &quot;sha256&quot;: &quot;943a810f7538b32f9d81dc5adea3792c07219964c8a8734565931fcec90d762d&quot;,        &quot;filename&quot;: &quot;/home/takatost/stable-diffusion-webui/models/Stable-diffusion/pastel-mix/pastelmix-better-vae-fp32.ckpt&quot;,        &quot;config&quot;: null    },]
```

`model_name` 就是我们需要的，这个例子中是 `pastel-mix_pastelmix-better-vae-fp32`。

1. 在 Dify 集成 Stable Diffusion

在 `工具 > StableDiffusion > 去认证` 中填写你在之前步骤中获取的认证信息和模型配置。

1. 完成

+   Chatflow / Workflow 应用

Chatflow 和 Workflow 应用均支持添加 Stable Diffusion 工具节点。添加后，需要在节点内的 “输入变量 → 提示词” 内填写变量引用用户输入的提示词，或者是上一节点生成的内容。最后在 “结束” 节点内使用变量引用 Stable Diffusion 输出的图像。

+   Agent 应用

在 Agent 应用内添加 Stable Diffusion 工具，然后在对话框内发送图片描述，调用工具生成 AI 图像。

## 4、AlphaVantage 股票分析

![](img/a05373b6a882ef182747bf191cf41aa1.png)

AlphaVantage 一个在线平台，它提供金融市场数据和 API，便于个人投资者和开发者获取股票报价、技术指标和股票分析。本

1. 申请 AlphaVantage API Key

请在 [AlphaVantage](https://www.alphavantage.co/support/#api-key)申请 API Key。

1. 在 Dify 内填写配置

在 Dify 导航页内轻点 工具 > AlphaVantage > 去授权 填写 API Key。

1. 使用工具

+   Chatflow / Workflow 应用

Chatflow 和 [Workflow](https://docs.dify.ai/zh-hans/user-guide/build-app/flow-app/variables) 应用均支持添加 AlphaVantage 工具节点。添加后，需要在节点内的 “输入变量 → 股票代码” 通过变量引用用户输入的查询内容。最后在 “结束” 节点内使用变量引用 AlphaVantage 节点输出的内容。

+   Agent 应用

在 Agent 应用内添加 AlphaVantage 工具，然后在对话框内发送股票代码或大致描述，调用工具得到准确的金融数据。

## 5、ComfyUI

![](img/40dc037ad9ff8ac28deaba4ec54d9a55.png)

[ComfyUI](https://www.comfy.org/) 是一个强大的、模块化的生成图形的平台。现在你可以在 Dify 中使用它， 传入图片的提示词， 并得到生成的图片。

1. 确保已能正常运行 ComfyUI 的工作流。

请参考其[官方文档](https://docs.comfy.org/get_started/gettingstarted)，确保 ComfyUI 可以正常运行并生成图片。

1. 导出工作流的 API 文件。

如图所示，在工具栏中选择 `Save(API Format)`，如果没有该选择，则需要在设置中开启 Dev Mode。

1. 在 Dify 中集成 ComfyUI

在 `工具 > ComfyUI > 去认证` 中填写访问地址，如果您使用的 `docker` 部署的 Dify，这个地址一般是 `http://host.docker.internal:8188`。

1. 在 Dify 中使用 ComfyUI

打开其 工作流 工具，在 `WORKFLOW JSON` 中填入刚刚导出的 API 文件中的内容，就可以正常生成了。
