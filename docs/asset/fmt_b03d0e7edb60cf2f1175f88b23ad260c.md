
# 00-课程介绍和学习环境设置

> 来源：[https://aigcreative.feishu.cn/docx/RiszdagrromcIAxGaZBcfmqNn63](https://aigcreative.feishu.cn/docx/RiszdagrromcIAxGaZBcfmqNn63)

我们非常高兴您能够开始学习本课程，希望您能通过学习使用生成式 AI 构建有趣的应用！

为了让您提升学习效率，我们创建了该页面，概述了所有关于本课程需要设置的步骤、相关技术要求以及如何获取帮助。

## [安装步骤](https://microsoft.github.io/generative-ai-for-beginners/#/00-course-setup/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e5%ae%89%e8%a3%85%e6%ad%a5%e9%aa%a4)

要开始学习本课程，您需要完成以下设置。

### 1. Fork this Repo

[Fork 这个完整的 repo](https://github.com/microsoft/generative-ai-for-beginners/fork?WT.mc_id=academic-105485-koreyst) 到您自己的 GitHub 账号下，以便您能完成代码的修改和完成相关的挑战。您也可以给该 repo star（🌟），让您更容易找到它和相关的 Repo。

### 2. 创建 GitHub Codespaces

为了避免运行代码时出现任何依赖性问题，我们建议在 GitHub Codespace 中运行本课程的相关例子。

这可以通过选择该 Repo 版本上的“Code”选项并选择 Codespaces 选项来创建。

### 3. 存储您的 API Keys

在构建任何类型的应用程序时，确保 API Keys 的安全非常重要。我们建议您不要将任何 API 密钥直接存储在您正在使用的代码中，因为将这些详细信息提交到公共存储库可能会导致不必要的费用成本和问题。

![](img/3d5440a023d9fd3d8cb28ce4d4b61adb.png)

## [在您的设备上本地运行](https://microsoft.github.io/generative-ai-for-beginners/#/00-course-setup/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e5%9c%a8%e6%82%a8%e7%9a%84%e8%ae%be%e5%a4%87%e4%b8%8a%e6%9c%ac%e5%9c%b0%e8%bf%90%e8%a1%8c)

+ [要在本地运行代码，](https://conda.io/en/latest/miniconda.html?WT.mc_id=academic-105485-koreyst)您需要安装某个版本的 Python。个人建议安装 miniconda，这是相当轻量级的安装，支持不同 Python 虚拟环境的 conda 包管理器。

安装 miniconda 后，您需要克隆存储库并创建一个用于本课程的 Python 虚拟环境：

```
git clone http://github.com/microsoft/ai-for-beginners
cd ai-for-beginners
conda env create --name ai4beg --file .devcontainer/environment.yml
conda activate ai4beg
```

### 使用 Visual Studio Code Python 插件

学习本课程，建议使用 [Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python&WT.mc_id=academic-77998-bethanycheum) 的 Python 插件。

> 注意：在 VS Code 中克隆并打开该目录后，会自动建议您安装 Python 插件。还必须按照上述要求安装 miniconda。

> 注意：如果 VS Code 建议您在容器中重新打开 Repo，您需要拒绝此操作以使用本地 Python 安装的环境。

### [在浏览器中使用 Jupyter](https://microsoft.github.io/generative-ai-for-beginners/#/00-course-setup/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e5%9c%a8%e6%b5%8f%e8%a7%88%e5%99%a8%e4%b8%ad%e4%bd%bf%e7%94%a8-jupyter)

您还可以直接从自己计算机上的浏览器使用 Jupyter 环境。实际上，经典的 Jupyter 和 JupyterHub 都提供了相当便捷的开发环境，具有代码自动完成、代码高亮等功能。

要在本地启动 Jupyter，请转到课程目录，然后执行：

```
jupyter notebook
```

或

```
jupyterhub
```

然后，您可以导航到任何 `.ipynb` 文件，打开它们进行学习。

### [在容器中运行](https://microsoft.github.io/generative-ai-for-beginners/#/00-course-setup/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e5%9c%a8%e5%ae%b9%e5%99%a8%e4%b8%ad%e8%bf%90%e8%a1%8c)

我们也可以在容器中运行代码。由于我们的 Repo 包含特殊的 `.devcontainer` 文件夹，该文件夹指示如何为此 Repo 创建容器，因此 VS Code 将允许您重新打开容器中的代码。这需要安装 Docker，而且会比较复杂，所以我们推荐给更有经验的用户。

使用 GitHub Codespaces 时确保 API Secrets 安全的最佳方法之一是使用 Codespace Secrets。请按照本指南了解如何管理 Codespace Secrets。

## [相关课程和技术要求](https://microsoft.github.io/generative-ai-for-beginners/#/00-course-setup/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e7%9b%b8%e5%85%b3%e8%af%be%e7%a8%8b%e5%92%8c%e6%8a%80%e6%9c%af%e8%a6%81%e6%b1%82)

该课程有 6 节基础课和 6 节相关的编程课。

对于编程课，我们使用 Azure OpenAI Service。您将需要访问 [Azure OpenAI Service](https://customervoice.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR7en2Ais5pxKtso_Pz4b1_xUOFA5Qk1UWDRBMjg0WFhPMkIzTzhKQ1dWNyQlQCN0PWcu&culture=en-us&country=us?WT.mc_id=academic-105485-koreyst) 和 API Key 才能运行此代码。您可以通过 [此申请](https://customervoice.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR7en2Ais5pxKtso_Pz4b1_xUOFA5Qk1UWDRBMjg0WFhPMkIzTzhKQ1dWNyQlQCN0PWcu&culture=en-us&country=us?WT.mc_id=academic-105485-koreyst) 来申请访问权限。

当您等待审核时，每个编码课程还包含一个 `README.md` 文件，您可以在里面查看代码和相关内容。

## 首次使用 Azure OpenAI Service

如果这是您第一次使用 Azure OpenAI Service，请按照本指南了解如何创建和部署一个 Azure OpenAI Service 资源。

## [找到志同道合的人](https://microsoft.github.io/generative-ai-for-beginners/#/00-course-setup/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e6%89%be%e5%88%b0%e5%bf%97%e5%90%8c%e9%81%93%e5%90%88%e7%9a%84%e4%ba%ba)

我们在官方 AI Community Discord server 中创建了学习频道，用于结识其他学习者。这是与其他志同道合的企业家、学生以及任何希望在生成式人工智能领域提升水平的人建立联系的方式。

项目团队也将在这个 Discord server 上为任何学习者提供帮助。

## [贡献该内容](https://microsoft.github.io/generative-ai-for-beginners/#/00-course-setup/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e8%b4%a1%e7%8c%ae%e8%af%a5%e5%86%85%e5%ae%b9)

[本课程是一项开源计划](https://github.com/microsoft/generative-ai-for-beginners/pulls?WT.mc_id=academic-105485-koreyst)。如果您发现需要改进的地方或问题，请创建 Pull Request 或记录 GitHub 问题。

课程项目团队将跟踪所有贡献，为开源做出贡献是在生成人工智能领域建立职业生涯的绝佳方式。

大多数贡献都要求您遵循 [贡献者许可协议 (CLA)](https://cla.microsoft.com/?WT.mc_id=academic-105485-koreyst)，声明您有权并且实际上授予我们使用您的贡献的权利。有关详细信息，请访问 CLA，贡献者许可协议网站。

重要提示：翻译此存储库中的文本时，请确保不使用机器翻译。我们将通过社区验证翻译，因此请用您熟悉的语言进行翻译。

当您提交拉取请求时，CLA-bot 将自动确定您是否需要提供 CLA 并适当地描述 PR（例如标签、评论）。只需按照机器人提供的说明进行操作即可。您只需使用我们的 CLA 在所有存储库中执行一次此操作。

该项目采用了微软开源行为准则。有关更多信息，请阅读 [行为准则常见问题](https://microsoft.github.io/generative-ai-for-beginners/#/00-course-setup/translations/cn/opencode@microsoft.com)，或联系 opencode@microsoft.com 提出任何其他问题或意见。

## [我们一起开始进入学习](https://microsoft.github.io/generative-ai-for-beginners/#/00-course-setup/translations/cn/README?wt.mc_id=academic-105485-koreyst&id=%e6%88%91%e4%bb%ac%e4%b8%80%e8%b5%b7%e5%bc%80%e5%a7%8b%e8%bf%9b%e5%85%a5%e5%ad%a6%e4%b9%a0)

现在您已经完成了本课程所需的设置步骤，让我们开始进入生成式人工智能和 LLMs 简介。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
