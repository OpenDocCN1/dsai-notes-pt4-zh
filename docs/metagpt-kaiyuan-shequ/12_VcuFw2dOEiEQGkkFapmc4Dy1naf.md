# 文档贡献

> 来源：[https://deepwisdom.feishu.cn/docx/LgytdqE6IoIiLJxRgVdcU42DnWg](https://deepwisdom.feishu.cn/docx/LgytdqE6IoIiLJxRgVdcU42DnWg)

官方文档网站地址：https://docs.deepwisdom.ai

目前，该文档网站主要包括入门和基础教程、单/多代理示例、高级指南等内容。同时，该文档网站目前主要支持中文和英文版本，因此我们期望您提交的文档也包含中文和英文两个版本。

此外，网站还包含 API 文档，但 API 文档主要由 MetaGPT 维护和自动更新，不属于文档贡献流程的范围。

提交文档也遵循 fork 和 pull request 的方式。

# 本地设置

支持文档站点部署环境的本地安装和预览，便于检查渲染结果是否符合预期。

## 前提条件

+   [Node.js](https://nodejs.org/en) 版本 18 或更高。

+   支持 [Markdown](https://en.wikipedia.org/wiki/Markdown) 语法的文本编辑器。

+   推荐使用 [VSCode](https://code.visualstudio.com/)，并安装官方的 [Vue](https://marketplace.visualstudio.com/items?itemName=Vue.volar)。 扩展。

## 启动步骤

按照以下步骤启动开发服务器：

1.  启用 pnpm（如果尚未安装）：corepack enable pnpm

1.  或者参考 [pnpm 安装指南](https://pnpm.io/installation) 进行安装。

1.  安装依赖项：pnpm i

1.  运行开发服务器：npm run docs:dev

开发服务器应在 http://localhost:5173 上运行。在浏览器中访问该 URL，即可查看您的新站点实际效果！

# 文档标准

## 文档结构标准

文档网站上的文档目前包含几个模块：入门指南、教程、用例和深入指南。

+   入门指南：提供快速安装和配置文件设置教程，帮助用户快速上手体验。

+   教程：简要介绍单代理和多代理，以及一些基本使用功能，包括人工干预和工具使用。

+   用例：提供不同场景下单代理和多代理的代码示例和效果介绍，帮助用户直观了解 MetaGPT 的功能。

+   深入指南：主要面向开发者，提供包括增量开启、通信机制、断点恢复等高级功能，帮助用户深化对场景的理解。

不同的模块有不同的内容重点，但结构基本遵循以下规范：

对于不同类型的文档，请参考相应现有文档的内容结构来补充内容，并将其保存为 Markdown 文件。

+   英文文档位于src/en目录，并在相应目录中创建。例如，教程文档位于src/en/guide/tutorials目录。

+   中文文档位于src/zh目录，并在相应目录中创建。例如，教程文档位于src/zh/guide/tutorials目录。

+   媒体文件（如图片和视频）位于src/public目录，存储位置必须与文档位置相对应。例如，涉及图片的教程文档放置在src/public/image/guide/tutorials目录，并为每个文档创建一个新的子目录。文档中的相应访问方式为：![img](../../../public/image/guide/tutorials/inc_req_and_fixbug/6d081360d0c74bb48794b9f8a2b0a23e.png)。需要注意相对路径。

+   添加和修改文档的侧边栏（导航目录）需要在src/.vitepress/config.mts中的locales.themeConfig.sidebar或locales.zhcn.themeConfig.sidebar进行配置。

+   对于其他需要在文档中引用的文档、图片等资源，可以为中文和英文文档指定相同的路径。

添加文档后，在提交 PR 之前确保其正确性。PR 批准后，新文档将自动在线更新。

## 文档内容标准

在编写文档内容时，需要考虑一些基本规范，以便整个文档网站保持一致的风格。

+   使用主动语态和现在时态来描述内容，并保持整个文档的清晰度和语调一致。

+   避免术语重复，尽量在前页定义术语，并引用而非重复定义。

+   尽量通过文档相对路径进行引用，以减少 URL 变化的影响。

+   文档使用清晰的层次结构（标题、副标题）来组织信息，便于快速浏览。列出章节概览，帮助用户了解内容流程。

+   尽量使用图表等形式来突出结果或解释复杂过程，这可以大大减少文本内容。

+   提供真实案例或示例，帮助用户更好地理解应用场景。

+   如果添加代码，请使用代码高亮和格式化工具，并添加适当的注释以提高可读性。

+   将复杂过程分解为小步骤，并引导用户逐步完成任务。在每个步骤提供反馈，确保用户了解自己的进度。

+   在教程结束时总结要点，帮助用户巩固学习内容。

如果在使用过程中遇到任何问题，请前往 Discord 频道进行交流。我们期待您的参与！