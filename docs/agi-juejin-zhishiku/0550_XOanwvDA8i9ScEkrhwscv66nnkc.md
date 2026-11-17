# AI 编程智能体深度剖析：Cursor 与 Cline 实战对决，谁更胜一筹？

> 来源：[https://agijuejin.feishu.cn/docx/Zm6Tdmr9SotbzUxqcUpczsxkn9d](https://agijuejin.feishu.cn/docx/Zm6Tdmr9SotbzUxqcUpczsxkn9d)

> 作者：欧少泽

> 链接：https://juejin.cn/post/7465031850969858088

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

> 如有侵权，请联系删除。

## 一、前言

这篇文章主要讲的是闭源 AI 编程智能体 Cursor 和开源 AI 编程智能体 Cline ，之所以主要讲这两个工具，是因为测试了好多工具后，发现这两个工具分别是闭源和开源领域中是比较好用的 AI 编程智能体。

对于程序员来说，经常需要做的就是等待编译，包括等待本地编译和等待流水线编译，而有了 AI 编程智能体后，程序员需要等待的事项就多了一个： 等待 AI 编程智能体工作完成 。

### AI 编程智能体

智能体，简单来说，就是你给 AI 越多的工具、资源，它就能帮你解决更多的问题。比如你把访问浏览器的权限给它，它就可以去查资料，把创建和编辑文件的权限给它，它就可以自己创建代码文件。而且智能体有一个规划模块，这意味着它会自己规划实现一个目标所需做的几件任务，然后利用工具逐个执行任务，这样的话就不需要我们自己为完成一个目标不断地去跟 AI 聊天。

AI 编程智能体主打的是写代码，但是因为它是一个能使用特定工具的智能体，这些工具有生成、创建文件、修改文件、读取文件目录、读取文件等，只要你需要做的事情涉及到这些工具，那都可以用 AI 编程智能体来完成，比如写小说、写产品需求文档、写技术文档、写测试用例等工作，都涉及项目级别的上下文，这些工作在 Cursor 的帮助下，Cursor 会读取项目的上下文，从而生成更加可靠的内容。有了读取代码库和特定文档的能力，意味着 AI 生成内容的幻觉会更少，生成的内容更可靠。

而且除了编写代码和生成内容，AI 编程智能体 还能很好地用来学习项目的工作机制，因为 Cursor 能找到代码调用的地方，从而理解代码的执行逻辑，比如我就是通过 Cursor 理解 Cline 的代码的。

**智能体的核心组成部分**

| **模块** | **功能** | **示例** |
| --- | --- | --- |
| **语言模型** | 提供语言理解和生成能力，是Agent的“大脑”。 | GPT-4、Claude、DeepSeek 等模型。 |
| **规划模块** | 将复杂任务拆解为子任务，制定执行步骤。 | 将“开发一个天气App”拆解为UI设计、API调用、数据存储等子任务。 |
| **记忆模块** | 存储历史交互、知识库和上下文信息，支持长期推理。 | 记录用户偏好、项目代码片段、API文档等。 |
| **工具调用** | 通过API、代码执行或外部工具扩展能力（如搜索、计算、数据库操作）。 | 调用Python代码执行数学计算、访问GitHub API获取代码库。 |
| **反馈迭代** | 通过自我反思或外部反馈修正错误，优化结果。 | 若生成的代码运行失败，自动分析报错信息并重新生成。 |

**AI 智能体工具 **、 **AI 问答 **和 **人工编码 **的区别：

| **维度** | **AI 编程智能体 **（如Cursor） | **AI 问答 **（如ChatGPT） | **人工编码** |
| --- | --- | --- | --- |
| **编码质量** | 自动生成高质量的代码片段；生成的代码需要人工进行验证和核查； | 提供代码建议或片段，但需人工验证正确性；对简单问题的处理效果较好，对于依赖代码库的复杂问题的输出质量不稳定； | 由开发者经验决定，而开发者的编码水平是参差不齐的；新手在复杂项目中可能引入低级错误 |
| **编码效率** | 效率较高。通过智能补全、多文件编辑等功能显著提升编码速度 | 即时响应问题，快速提供参考代码；需要人工调整代码以适应具体需求； | 需逐行编写代码，耗时较长。 |
| **上下文理解效率** | 效率较高，能读取代码库的，快速定位任务的相关上下文 | 效率中等，依赖用户描述的清晰度。 | 通过沟通和文档逐步理解需求，耗时但准确性高；对业务背景和隐性需求的理解更深入。 |
| **局限性** | 对复杂逻辑或新兴技术的支持不足，但是可以通过访问文档的形式弥补；生成代码可能缺乏优化或存在隐藏漏洞，需要人工审查 | 无法处理需要多步骤推理的复杂问题；输出结果可能过时（依赖训练数据时间范围）。 | 人力成本高，开发周期长；依赖开发者技能和经验水平，存在主观偏差风险。 |
| **学习成本** | 低（自然语言描述需求） | 中（需学习有效提问技巧） | 高（需掌握特定的编程语言和相关工具） |
| **错误处理** | 自动检测并处理编译和构建错误，部分业务逻辑错误需人工进行调整 | 需人工识别粘贴代码与修复编译和构建错误 | 依赖开发者处理编译和构建问题的经验 |
| **适用场景** | 快速原型开发、缺陷修复、代码补全、代码重构、文档生成。 | 学习编程知识、解决简单问题、获取代码示例。 | 复杂系统开发、性能优化、特定的安全合规问题的处理。 |

**总结**

+   **AI 编程智能体 **（如 Cursor）：适合 **辅助开发 **，提升效率，但需人工校验逻辑；

+   **AI 问答 **：适合 **学习与快速查询 **，但生成的内容缺乏代码环境适配性；

+   **人工编码 **： **完全可控 **，适用于复杂或关键系统，但开发成本高。

三者可结合使用可以提升开发的效率与质量。

### 对客户端开发的影响

由于我对服务端开发的了解较少，这里只说一下我个人认为 API 编程对客户端开发的影响。

| **工作** | **影响度** | **说明** |
| --- | --- | --- |
| 代码编写 | 中 | 对于只需要依葫芦画瓢的业务需求，AI 能轻松学会，影响较高。但是对于部分技术需求，需要工程师自己提出创新的想法，但是落地到具体的代码时可以让 AI 来编写 |
| 代码优化 | 高 | AI 能学会设计模式和整洁代码的写法，对代码进行重构和优化 |
| 单元测试 | 高 | 只需要依葫芦画瓢，AI 能轻松学会 |
| 架构优化 | 低 | AI 写代码很容易，如果代码库很大，但是让 AI 直接思考怎么优化架构的话，AI 给的答案就不一定能令人满意，因为涉及的上下文太大。在做架构优化的时候，你只需要想好具体要怎么改，具体的编码工作可以交给 AI。 |
| 疑难问题解决 | 低 | 这里的疑难问题指的是上下文非常庞大，庞大到 AI 无法理解的程度，比如处理涉及 Framework 层的几个模块的问题 |
| 跨部门协作 | 低 | 涉及到和人的具体沟通交流，无法简单生成内容来解决 |
| 自测 | 低 | 自测的时候往往需要设置一些项目特定的配置，这些配置可能是在云端的，暂时 Cursor 还没有办法做修改云端配置这些操作 |

## 二、工具

### 2.1 工具对比

##### 优劣势对比

| **工具** | **简介** | **使用成本（单人）** | **优势** | **劣势** | **适用场景** |
| --- | --- | --- | --- | --- | --- |
| [Cursor](https://link.juejin.cn/?target=https%3A%2F%2Fwww.cursor.com%2F) | 基于 VS Code 开发的 AI 编程智能体，支持自动编辑文件、访问代码库、运行命令 | 20刀/月 | 1、成熟度高，自动化能力强，支持复杂任务拆解；2、能自动处理编译错误；3、能把代码库转化为嵌入向量，处理上下文的效率较高；4、API 的访问相对稳定；5、修改支持回退（Revert） | 1、不支持自定义模型 API ，如 Ollama Server；2、处理大型项目的编码任务时需要更多的 CPU 和内存资源； | 中小型项目或模块的需求开发、代码优化、单元测试等诉求； |
| [Trae](https://www.trae.ai/) | 字节开发的 对标 Cursor 的 AI 智能体工具，支持 Web 预览 | 限时免费 | 1、限时免费；2、增加了 Web 预览功能； | 1、Agent 的能力不如 Cursor，错误修复能力较弱，还需要进一步优化；2、暂不支持 Windows | 1、有预览诉求的前端应用开发工作；2、使用 MacOS 的用户； |
| [Winsurf](https://link.juejin.cn/?target=https%3A%2F%2Fcodeium.com%2Fwindsurf) | Codium 公司开发的基于 VS Code 的 AI 编程工具 | 15刀/月（有免费额度） | 1、成本相对 Cursor 较低； | 1、模型选择较少，中文支持不完善； | 对价格比较敏感的开发者 |
| [Cline](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fcline%2Fcline) | 开源的 VS Code AI 编程智能体插件 | 取决于使用的 API 或本地部署模型的设备 | 1、开源且支持访问本地部署的模型；2、支持通过 MCP Server 访问外部数据源和工具； | 1、处理大型项目的性能较差；2、不支持 @Doc 、@Notepad 等 Cursor 支持的上下文；3、提示词设置项较少4、不支持自定义模型 API ，如 Kimi；5、时不时会遇到 APi 访问失败的情况； | 1、有信息安全要求的的用户；2、有访问自定义数据或自定义工具诉求的用户 |
| [Roo Code](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FRooVetGit%2FRoo-Code) | 开源的基于 Cline 开发的 AI 编程智能体插件，支持自动补全、AI 修改代码 | 取决于使用的 API 或本地部署模型的设备 | 1、开源且支持本地部署的模型，适合注重信息安全的场景；2、更丰富的提示词设置；3、使用 DeepSeek-R1 时可以看到推理过程；4、有 API 请求失败重试机制 | 1、不支持 Ollama；2、处理大型项目的性能较差；3、不支持 @Doc 、@Notepad 等 Cursor 支持的上下文；4、提示词设置项较少；5、不支持自定义模型 API ，如 Minimax； | 1、有信息安全要求的的用户；2、有访问自定义数据或自定义工具诉求的用户；3、有自定义提示词要求的用户 |
| [Warp](https://link.juejin.cn/?target=https%3A%2F%2Fwww.warp.dev%2F) | 一款专为 macOS 设计的 AI 终端工具，支持自动补全和错误修复 | 未知 | 操作便捷，适合 macOS 用户 | 不支持跨平台 | 推荐 MacOS 使用 |
| [MarsCode](https://www.marscode.cn/) | 字节开发的基于豆包大模型的 AI 编程助手，以 Idea 和 VS Code 插件形式运行 | 免费 |  | 不支持自动编辑文件的代理模式 | 只需要问问题，不需要修改代码、关联代码库的场景 |
| [Devin](https://link.juejin.cn/?target=https%3A%2F%2Fdevin.ai%2F) | 由 Cognition 团队开发的全球首个全自主 AI 软件工程师智能体，旨在通过人工智能技术辅助或独立完成软件开发任务 | 500刀/月 | 自动找出并修复问题 | 价格较高 | 需要 AI 智能体帮忙找出并修复缺陷的用户 |
| [bolt.new](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fstackblitz%2Fbolt.new) | StackBlitz 推出的全栈 Web 编程工具，支持前后端代码编写、运行和部署 | 免费 | 全栈开发一体化，无需本地环境 | 依赖云端，无法离线使用 | 前端项目使用 |
| [v0.dev](https://link.juejin.cn/?target=https%3A%2F%2Fv0.dev%2F) | Vercel 提供的云端 AI 驱动前端开发工具，快速生成可复用 UI 组件。面向前端开发者与设计师，支持实时预览和跨平台适配 | 20刀/月 | 快速生成 UI 组件，提升设计效率 | 功能局限于前端，复杂场景支持不足 | 设计师与前端开发者使用 |

##### 单文件编辑任务处理结果对比

总结：不考虑信息安全和扩展功能的情况下优先选择 Cursor

+   **Cline **：对于以下的测试结果，使用 Cline 时，只有闭源的 Gemini-2.0-Flash-Exp 模型成功处理了任务，其他的开源模型都没有完成任务。对于新项目来说，适合使用 Gemini 模型处理代码上的需求，但是对于比较成熟，比较复杂的项目，不太建议使用 Cline 来处理编码任务，因为处理复杂项目的时候 Cline 存在较大的性能问题，电脑会变得非常卡。如果实在想要使用 Cline 的话，可以考虑把项目模块化，以减少 Cline 处理的上下文的复杂度

+   **Cursor **：对于大型项目、复杂项目来说，推荐使用 Cursor 处理编码需求，但是也需要好一些的电脑，如 i7 及以上的处理器 + 32G 及以上的内存 ，因为我发现 Cursor 加载大项目的时候会需要用到很多内存和 CPU 资源

虽然下面的测试结果主要是基于 Cline 的，但是实际使用的话，和 Cline 相比更推荐 Roo Code ，因为Gemini API 有的时候会报错，而 Roo Code 有错误重试机制。

下面的任务处理结果，对应的诉求是 优化 PlantDetailView 的代码并编写单元测试 ，这个文件是 Android 官方的 Sunflower 项目中的文件。

由于大语言模型的输出不是固定不变的，所以有的模型有的时候能处理成功，以下的测试结果挑选的是同一个模型最坏的测试结果，以免拉高了大家对小型的开源模型的期待值。

| **工具** | **模型** | **主观评分 **（满分5分） | **处理结果截图** | **处理结果** | **处理时间** |
| --- | --- | --- | --- | --- | --- |
| **Cline** | qwen-2.5-coder-14b | 0 | ![](img/3644f3ccd2fed423829ff3f1ad075565.png) | 用 Ollama 上的 [Qwen2.5 Coder ](https://link.juejin.cn/?target=https%3A%2F%2Fqwenlm.github.io%2Fzh%2Fblog%2Fqwen2.5-coder-family%2F)14b 模型，模型输出的内容是针对整个目录的，没有理解我的诉求。 | NA |
| **Cline** | qwen-2.5-coder-32b-instruct | 1 | ![](img/6b39f162fa0e604417799547752143d6.png) | 没有编写单元测试 | 04:38 |
| **Cline** | qwen-2.5-72b-instruct | 0 | ![](img/0afc9d0f050b04f6f7577bc5e63255c9.png) | 出现了编写重复的导入语句的问题 | 02:04 |
| **Cline** | deepseek-r1-distill-llama-70b | 0 | ![](img/d5e3b040d540a2f3030614c68cf100e9.png) | API 请求失败 | NA |
| **Cline** | deepseek-r1 | 0 | ![](img/337ce058532898b76cac7edc8e04826a.png) | 花了 5 分钟，也没有完成优化 | 05:06 |
| **Cline** | deepseek-chat | 0 | ![](img/e0a9ea4fdce5e496c7e017807ab8ce68.png) | 没有理解我的诉求，完成任务 | 01:49 |
| **Cline** | Claude-3.5-Sonnet | 0 | ![](img/1eed520a52f734018db69c3fff44a5fb.png) | API 请求失败 | NA |
| **Cline** | gemini-2.0-flash-exp:free | 5 | ![](img/32f5ba0f287f17a5ee2c3b2784d73b32.png) | 任务处理的说明比较仔细 | 06:36 |
| **Cursor** | Claude-3.5-Sonnet | 4 | ![](img/e8acfa48aabdaac481849659b5e490f8.png) | 任务的处理不如 Cline + Gemini 这么仔细，但是也完成了任务 | 03:37 |
| **Cursor** | DeepSeek-R1 |  | ![](img/2e8a356cecdd6d2c265e21fbf8b35dda.png) | Agent 模式不支持 DeepSeek-R1 | NA |

DeepSeek-R1 在这个任务上的的表现不是很好，可能是因为它是一个推理模型，对于 AI 编程智能体提示词的处理不是特别理想。

##### 多文件编辑任务处理结果对比

下面任务的提示词是“给 PlantDetailsScreen 增加一个删除和更新 Plant 的功能”，这个提示词的预期效果是修改多个文件，包括 PlantDetailView 、PlantDetailViewModel、GardenPlantingRepository ，Roo Code 和 Cursor 都完成了这个任务。

下面是 Roo Code + Gemini 2.0 Flash Exp 的处理结果，在任务执行的过程中中断了一次，但是也完成了任务。

![](img/0513825928b5452526c66b9cf5713419.png)

![](img/264fb5302c8536dfcebea2a2a983d950.png)

![](img/1ea931c1349598085451bbad4914e04d.png)

下面是 Cursor + Claude 的处理结果，也完成了任务。

![](img/e1050fe0a75ca522ca170c3c69228619.png)

##### 复杂文件编辑任务处理结果对比

下面是一个更有挑战性的任务，重构 RecyclerView ，RecyclerView 是一个用于在 Android 中高效显示大型数据集的强大而灵活的视图组件，RecyclerView 一个类的源码就有 14744 行，彻底理解 RecyclerView 对于很多 Android 应用开发者来说都是一个不小的挑战。

Cursor 虽然理解了这个任务需要做哪些事情，经过 18 分钟的处理后，Cursor 做了很多代码拆分的工作，但是最后也没有实现重构这个目标，没有对 RecyclerView 做出实质性的修改。

Cursor 做了下面的事中包含下面这几件事事。

+   把代码挪到源码目录下，以解决编译问题

+   建立构建配置文件 build.gradle

+   把 RecyclerView 中的 ViewHolder 放到 RecyclerViewHolder 文件中

+   把 RecyclerView 中的 LayoutManager 放到 RecyclerViewLayoutManager 文件中

+   把 RecyclerView 中的常量放到 RecyclerViewConstants 中

+   把 RecyclerView 中的 AdapterDataObservable 放到 AdapterDataObservable 文件中

+   把 RecyclerView 中的 ItemAnimator 放到 RecyclerAnimator 中

![](img/851f83333dc017c30aebbf39af655124.png)

由于 Cline 处理这个任务失败了，下面是 Roo Code + Gemini 处理相同的任务的完成情况，花了 15 分钟，也没有给出一个明确的重构方案和结果。

![](img/4a9c6b94b51edc0dae45cc0321aff4b5.png)

在尝试了一下优化提示词后，Gemini 的输出比上一次要好一些，只能说优化提示词有一些作用，但是和 Cursor 一样，最终还是没有完成重构的任务。

![](img/96697daadfeb8e79eef79ca533a0ba1a.png)

##### 编写产品需求文档

该任务的提示词有两个：“为每个页面编写独立的功能需求文档，并放到 prd 目录中，然后完善 gallery 的产品需求文档，并实现相关的代码”。

Cursor + Claude 的组合很好地完成了任务，创建了 GalleryPhoto 等文件。

![](img/63119491ea9957e7c6173062aef5e17f.png)

![](img/fb340ec5732aa450568ab42c42a01f83.png)

Roo Code + Gemini 的组合大致理解了我的诉求，写出来的 PRD 相对 Claude 写的要简陋一些，而且优化方面的改动也更少。

![](img/94e59a836e64e65000fb2023c9951d4b.png)

![](img/6558c66e116adbef199010c01361a45f.png)

### 2.2 Cursor

![](img/01e63da61bb73d399522601ced43ab09.png)

Cursor 是一个基于 VS Code 开发的付费的 AI 代码编辑器，VS Code 编辑器前端开发用得比较多。

Cursor 的早期版本我有用过，那时候觉得 Cursor 就是个玩具，只能根据输入到编辑框里的提示词生成对应的代码，跟我直接与 LLM 对话的效果差不多。但是 Cursor 发展得很快，越来越成熟，当给 AI 添加了修改文件、运行命令这些能力后，Cursor 编写代码的实用性越来越强。

Cursor 的价格是 1 个月 20 刀，可以在电商平台上买到更便宜的 Cursor 账户。

![](img/0043cf94f9d0bbbca3018a41f2278a3e.png)

当我们向 Cursor 说出我们的诉求时，Cursor 会自动查找相关的文件并进行修改，在修改完文件后，Cursor 会运行 Lint 检查编译问题，看下有没有错误，如果有错误的话还会自己想办法修复 Lint 错误，比如处理没有导入类等问题，这不就是程序员平时做的事情之一吗？改代码，然后处理编译问题。

![](img/f811b48d12bdc11a0ad52c01c94ff53b.png)

![](img/c338f59897492903f7d3ef980d978c5c.png)

Currsor 编辑文件时，可以直观地查看修改的内容，这个功能是 Cline 没有的。

![](img/13b6fc06c2ecaf9f3bc684fdc7a3aeba.png)

Cursor 在任务完成后，还会总结一下自己做了哪些修改。

![](img/21d446c321e0bd6c06ca67a817d3b33e.png)

#### 2.2.1 安装

安装 Cursor 时，第一个界面会看到快捷键的设置，比如可以设置 Jetbrains 的快捷键。然后是设置语言以及 Embedding 计算的范围，默认是把整个代码库的代码转化为嵌入向量。

另外还可以把 cursor 添加到命令行，可以选择安装 code 和 cursor 命令行工具，后面在终端里输入 code 或 cursor 后，就会打开 cursor 。

![](img/c1e832957c71f3e8a468d3877b40579e.png)

![](img/a896a7d37f359451029d375c2c539ccc.png)

![](img/a42fdae317856838477ac420dced66f9.png)

做完这个配置后，就需要进行登录，新注册的账户有一些免费的提问额度，免费的额度用完后，可以在电商平台上购买更便宜的 Cursor Pro 账户。

#### 2.2.2 使用

登录完成后，点击左侧面板中的扩展按钮（Extentions），在扩展按钮左侧的是打开源码管理和搜索面板的按钮。要先安装所需的 VS 扩展插件，比如 Kotlin ，这样的话编辑器才可以高亮显示对应文件类型的代码。

![](img/7a79bfaf313aec03ae174573fe176404.png)

![](img/c80dbfa5b8724be498847a6bce5cf6fb.png)

然后选择打开文件夹，比如我下面打开的是 android-architecture-samples 项目。

![](img/9a28e1f4bf6b2e75c32491f7fe588be8.png)

##### Composer 与 Chat 的区别

**Composer**

+   需要自己 @ 对应的文件，也可以通过提示词触发 AI 搜索特定的目录，比如提示词中包含“登录页”则 Cursor 会搜索登录页相关的代码

+   可以同时修改多个文件，点击 Accept All 则确认同时修改多个文件

+   可以根据指令不断地创建文件、执行命令，自动执行

+   Ctrl + i 打开

**Chat**

+   可以 @Codebase ，让 AI 搜索整个代码库

+   需要自己点击应用代码的修改建议

+   Ctrl + L 打开

选中一段代码后，按下 Ctrl + K 可以以这段代码作为上下文，让 Cursor 修改对应的代码。

![](img/be55e6c7082a458034b499f5f6fde52a.png)

![](img/7bc5184bd1ccfd48c01e0866d0893a93.png)

Cursor 也支持通过 Tab 的方式自动补全，MarsCode 等 AI 编程插件也支持类似的功能，这里就不赘述了。

按下 Ctrl + i ，打开 Composer ，在 Compose 面板中，可以切换 normal / agent 模式，也可以上传图片以及切换模型。

在 Composer 中按下 “Ctrl + .” 可以切换 normal / agent 模式。

![](img/fc23847952ff03afa5a0ae8feece04f9.png)

![](img/c815024d06b217dcf5bdac3ce1d58cb5.png)

在 Compose 模式中，可以 @ 特定的文件、文件夹、代码、文档、Git（PR、Commit）、建议的上下文、Web 等内容作为上下文，但是无法 @Codebase。

Chat 功能主要是用于不需要修改代码，只是提问的场景，如果 Cursor 发现你问的问题涉及到代码修改的话，也会编辑代码文件，Chat 模式下按下“Ctrl + 回车”就可以以整个代码库作为对话的上下文。

![](img/0ae049a4829c630e3f597948db5bcc26.png)

![](img/42b2d0421cfd3ddb500120df894187c9.png)

Bug Finder 我自己也没用过，运行的时候 Cursor 会提示你成本非常高。

![](img/b74502986891684b2857d82b917c857f.png)

##### Agent 与 Normal 模式的区别

**Agent 模式**

+   **主动性与自动化 **：Agent 更像一个“智能编程伙伴”，能够自主规划任务、分析需求并执行复杂操作（如跨文件修改、运行终端命令、自动修复错误等），减少用户手动干预。

+   **项目级操作 **：支持多文件协同编辑、自动创建/删除文件、生成完整项目框架（如 Next.js 应用），甚至通过 Yolo 模式实现“全自动”开发（无需用户确认命令执行）。

+   **工具调用能力 **：最多可连续调用 25 个工具（如代码生成、依赖安装、测试运行），适合需要多步骤迭代的任务。

+   **自动上下文提取 **：主动分析项目结构、依赖关系和代码语义，智能选择相关文件（支持 @Suggested 自动推荐），减少手动指定上下文的负担。

+   **多任务协同 **：可并行处理多个文件修改、运行后台命令，并维护任务执行状态，适合复杂重构或功能开发。

+   **长时记忆支持 **：单次会话可保留大量上下文，处理需要多轮迭代的任务（如性能优化或模块重构）

**Normal 模式（普通模式）**

+   **基础交互与代码探索 **：侧重于单文件内的代码补全、问答式交互（如解释代码、修复语法错误）以及简单的文件操作（需手动复制生成的代码）。

+   **可控性优先 **：用户需手动通过 @ 符号或上下文胶囊（Context Pills）指定上下文范围，适合小范围修改或快速调试。

+   **手动上下文管理 **：需用户主动通过 # 符号或 @ 命令添加文件或代码片段，上下文容量有限，易受干扰（如手动添加过多无关文件可能降低生成质量）。

+   **短时记忆 **：对话上下文较短，复杂任务需多次补充信息，更适合快速问答或小范围代码生成。

**Cursor 使用技巧**

+   越是精确地指定相关的上下文（如代码文件、@Doc、@Web）等，Cursor 生成的代码修改建议就越有效

#### 2.2.3 配置

点击右上角的设置按钮，可以进入 Cursor 设置页。

![](img/d7cce8bf829d0a2a51c26ef955a254ad.png)

Cursor 的设置页主要分为几个模块：

+   General ：通用设置，如账户登录、通用提示词（Rules）设置、隐私模式设置等

+   Models ：模型设置，可以勾选特定的模型，这样的话就能在 Chat 和 Composer 中选择对应的模型

+   Features ：特性设置，可以调整 Tab 键、Chat、Composer 、代码库索引、文档、编辑器以及终端的设置

+   Beta ：实验性功能的设置，可以启用 Notepads 、BugFinder 等实验性功能

##### 1、通用设置

在通用设置中可以登录账户或者退出登录，也可以设置通用的 Cursor 提示词，这个提示词会作为隐含的 Chat 和特定代码会话（Ctrl+K）的上下文。

![](img/dedeb6836a3c846a2f40a500b754dbef.png)

此外 Cursor 还支持通过 .cursorrules 文件指定项目特定的提示词，只需要在项目根目录下创建一个 .cursorrules 文件，Cursor 就会读取这个文件的提示词，提示词用于触发 AI 生成与提示词相关的信息。

![](img/952ae8badb963037392811f3cf2d7a45.png)

如果开启了隐私模式，则 Cursor 不会读取项目中的代码，但是这样的话 Cursor 就无法发挥自动关联上下文的作用。

![](img/f19df2c4252fc5adee6760f61cb4f07f.png)

##### 2、模型设置

Cursor 支持的模型类别主要是 Claude 、Gemini、GPT、o1、DeepSeek。

![](img/d80faf5d3eed2b13652b72b8b576ebd8.png)

在 Cursor 中，如果要使用 OpenAI 的 o1 或者 o1-preview 模型的话，价格要另外算，而且使用非 Claude 模型，比如 gpt-4o 的话，Cursor 是无法自动编辑文件的。

![](img/c0c07667142d49804d0a5327c64686d9.png)

##### 3、特性设置

Features 模块的设置主要有下面几个部分：

+   Cursor Tab ：可以启用自动导入、注释上的修改建议等设置项

+   Chat & Composer ：启用自动滚动到底部、Yolo 模式（无需确认自动执行代码修改）、自动修复 Lint（静态代码检查）错误、自动找出上下文、自动运行或者通过按钮确认

+   Codebase Indexing：把代码转化为向量，以便 LLM 更好地理解代码，提升处理上下文的效率

+   Docs ：文档，可以在 Chat 和 Composer 中艾特添加的文档

+   Editor ：可以启用自动解析链接等设置项

+   Terminal ：可以启用终端上的提示等设置项

![](img/3a8801e1f6f8bfd3887a113db605bf83.png)

![](img/889233650398609bf8dc32b24bd3e35e.png)

Cursor 为代码库建立的嵌入向量会存储在云端，但是代码不会存储在云端。

![](img/f7ece5f4b9c53697020ac3431344673c.png)

审查修改的方式有三种：

+   显示按钮

+   自动运行

+   禁用

![](img/4ac89128bbb474b9b560a28aab585fee.png)

在代码库索引这部分的设置中，可以在 Ignore files 中设置不需要 Cursor 去查看的文件。

![](img/728a8daa80bae9e3389d35bee8dd63c9.png)

点击 Configure ignored files 后，Cursor 就会在项目根目录下创建一个 .cursorignore 文件，把想要忽略的目录或文件名输入后，Cursor 就会忽略这些文件夹和文件，可以忽略掉项目的 build、dist 等存储构建结果的文件夹。

![](img/82bc8a71339d158b0b7b86019cedf812.png)

##### 4、实验性功能设置

在 Beta 功能设置模块中，可以设置启用 Notepads 和 Bug Finder ，Bug Finder 可以用于查找项目中的缺陷，但是运行的成本较高。Notepads 可以用于共享 Chat 和 Composer 之间的上下文。

![](img/d891221e3c17457e712530eb0fae48c0.png)

**Notepads 主要作用**

上下文共享：在 Composer 和 Chat 之间无缝共享上下文

文件附件：附加文档和参考文件

动态引用：使用 @ 提及链接到其他资源

**常见用例**

+   动态样板生成：为常见代码模式创建模板、存储项目特定的脚手架规则、在整个团队中保持一致的代码结构

+   架构文档：前端规范、后端设计模式、数据模型文档、系统架构指南（如 DDD ）、开发指南

+   编码标准：项目特定规范、最佳实践、团队惯例

适合在 Notepad 中写什么？

+   项目架构决策

+   开发指南和标准

+   可重复使用的代码模板

+   需要经常参考的文档

+   团队特定的惯例和规则

Notepad 里不适合写什么？

+   临时笔记或草稿

+   属于版本控制的信息（例如 git）

+   敏感数据或凭证

+   高度不稳定且频繁变化的信息

**Notepad 示例：**

```
# API 开发指南## 端点结构- 遵循 RESTful 规范
- 基础 URL：`/api/v1`- 资源命名使用复数形式

## 认证机制- 基于 JWT 的身份验证
- Token 格式：Bearer {token}（中文括号）
- 必须实现令牌刷新机制

## 响应格式
```json
{
  &quot;status&quot;: &quot;success|error&quot;,
  &quot;data&quot;: {},
  &quot;message&quot;: &quot;可选描述信息&quot;
}

## 参考资料
@api-specs.yaml
@auth-flow.md

![](img/0cc164c439b1a87c7e2b10ddd9b682e3.png)


![](img/ccdac0ad822aa72680ffdcc72ebccc6c.png)

![](img/25dd3c8fc9a70c7acec17b64c8d5dad7.png)


![](img/bbd5ab85c4730a49bd7a0b05c1017697.png)

![](img/67589900b1c6c2d7f16e664ce6c03ed1.png)


![](img/60da8404e852be4f41b6acb64a7ea6f8.png)

![](img/ecd3b00d611fb5ad00109852f9be5abb.png)


![](img/90dd63989d1aeb9dd4a76ae677025ff3.png)


![](img/d065d6f5a3aa09b394745c211eda975f.png)

![](img/5727519682fe5cadcd362407cec00174.png)


![](img/ea37221208edf05a4476d172357981c5.png)

![](img/b8463656f8f2deb904b6a53b293b78ef.png)


![](img/96f1256b8b9e7e1a123fb60257032eb3.png)


![](img/05cd68708557ca666011bd578825e430.png)


![](img/3c5230a98f9b9970e84af6a55f57d4c4.png)

![](img/5c9b2b8e3563a4b8177411ede96a4f25.png)


![](img/e70e62ff1234fecf7bbc65261c09dfef.png)


![](img/7e40093e3b681060d8267cbed4004678.png)

![](img/7c7dbfc12ddcc05b0b0e90fd9cc41404.png)


![](img/07879086c0629058f2cc4a49c661f85a.png)


![](img/6c8791679f31480c5614b60fdb6043ac.png)

![](img/4b2fa6fa7de5730d94009b845374c854.png)


![](img/fa8f63d627a34d9a5e1c7839112ab767.png)


![](img/d1bafb7edfb900f2311e536fb635398a.png)


![](img/1ec9410acb78697d8041811428988548.png)


![](img/da6782631230203ec827f2c1b2dede4b.png)

![](img/9c3f9fb18d0e6a94e6104bf51acf8f29.png)


![](img/cccec42030fe5d8a00a696e82f8847cb.png)

![](img/c18ecdc3742075f743f9883727eba87e.png)


![](img/f0045131f0c7b88dd38766010ad186d6.png)


![](img/d03cb44af720ddc7a7b7312307167638.png)


![](img/a071be8393f2f3138ba5972109272437.png)


![](img/5e5c35f9235443e14f1b8600e3c1da46.png)

![](img/68de5c0c20e6dcf17ccc89bd730cbb45.png)


![](img/8e99294f8c9e252a0fd42d2fec7d39b9.png)


![](img/99fd42703542ce4967ce8e6432930c80.png)


![](img/5d9fede54cb8c02c5cd8513a798758e2.png)


![](img/a2c70d24462a35ad1b0938578e96874a.png)


![](img/1afd85f951cab9fe9b0bb716f95a4910.png)


![](img/70d4eb7391f3ad56835f1943bf8760b9.png)

![](img/1722da1d4989520e55576a7cc3a68eb1.png)


![](img/28f23003c5f4b5e6f1905122b1d97953.png)

```

### 2.4 Trae

Trae 字节做的一款是对标 Cursor 的 AI 编辑器，UI 上基于 VS Code 做了一些优化，并且在 Cursor 的基础上增加了 Web 预览的功能，编辑器模块基本上就是沿用的 VS Code 的代码。

[目前只](https://www.trae.ai/)做了 Mac 平台的客户端，打开 官网 就可以下载。
下载安装后，打开应用会看到开始页面，点击开始后可以选择主题和语言，目前只支持中文和英文。
点击继续，可以导入 VS Code 和 Cursor 的配置，导入配置后，可以设置添加 trae 命令行工具，该工具可用于通过终端启动 Trae。
配置好工具后，可以选择登录或跳过，不登录的话是无法正常使用 Trae 的 Chat 和 Builder 功能的。
登录后，进入 Trae 的主界面，点击“打开文件夹”后，和 VS Code 一样，会提示是否信任文件夹的作者，信任的话点信任就可以。
在 Trae 左侧的工具栏上，主要有这几个工具：

+   资源管理器：可以查看和打开项目中的文件

+   搜索：可以搜索项目中的包含特定字符串的文件

+   源代码管理：可以查看 Git 仓库相关的信息

点击右侧面板中的 Builder 标签或者按下 Ctrl + U，就可以看到 Builder 面板中的对话框，对话框中可以切换使用的大语言模型，默认的是 Claude-3.5-Sonnet。
比如说我输入给代码添加注释后，Trae AI 就会开始思考要怎么添加注释。

AI 思考完成后，我就可以开始审查单个文件，或者接受单个文件修改，又或者是点击“全部接受”，一次性修改所有需要修改的文件。
目前 Trae 只支持 Claude-3.5-Sonnet 和 GPT-4o 这两种模型。
在点击对话框中的 # 可以引用特定的代码、文件和文件夹。
遇到错误的时候，可以选择 AI 修复，点击 AI 修复的时候 Trae 会修改文件的内容，点击接受就会修改文件。
当 Trae 需要用到命令的时候，不会自动执行命令，需要点击才会运行。
和 Cursor 一样，Trae 也支持运行命令行，但是无法主动处理运行命令的过程中发生的错误，可以点击终端中的“添加到对话”按钮把错误信息添加到 Builder 的输入框中。
如果运行的是前端应用，还可以使用 Trae 的 Web 预览功能来查看应用运行的效果，这个功能也是 Trae 的一个特色功能。
### 2.5 Windsurf

在 [官网 ](https://link.juejin.cn/?target=https%3A%2F%2Fcodeium.com%2Fwindsurf)下载 Windsurf Editor 并安装后，打开 Windsurf ，首先看到的是导入设置项界面，可以从 Cursor 或 VS Code 中导入设置，然后需要进行登录。
登录后，Windsurf 窗口右侧是 Cascade 面板，可以把它理解为 Cursor 的 Composer ，并且提示了有两个快捷键可以用。

+   Ctrl + L ：唤出 Cascade

+   Ctrl + Shift + P ： 打开命令面板
和 Compoer 一样，当我输入优化代码的时候， Windsurf 就会开始尝试思考后修改代码，我只需要点击 Accept All，不同的是，虽然我输入的是中文，用的也是 Claude 模型，但是输出的内容却是英文的，这个问题应该是与 Windsurf 提供给模型的提示词有关。
Windsurf 可以选择的模型相对 Cursor 来说更少，Cursor 是可以选择 Gemini 模型的。
### 2.6 Cline

#### 2.6.1 用法

Cline 是一个 AI 编程智能体，以 VS Code 扩展插件的形式使用。在 VS Code 中安装 Cline 后，就可以在左侧看到打开 Cline 的按钮。
##### 设置项

首次打开 Cline 的时候，要选择 API 提供者，默认是 OpenRouter，也可以使用 Gemini、DeepSeek、LM Studio以及 Ollama 等。
+   [OpenRouter ](https://link.juejin.cn/?target=https%3A%2F%2Fopenrouter.ai%2F)是一个聚合了多种大型语言模型（LLMs）的API平台，提供了不同模型的访问和成本优化服务

+   [Gemini ](https://link.juejin.cn/?target=https%3A%2F%2Fgemini.google.com)：谷歌开发的大语言模型，不支持中国 IP 访问

+   [DeepSeek ](https://link.juejin.cn/?target=https%3A%2F%2Fchat.deepseek.com%2F)：中国深度求索公司开发的大语言模型

+   [LM Studio ](https://link.juejin.cn/?target=https%3A%2F%2Flmstudio.ai%2F)：本[地大语言](https://link.juejin.cn/?target=https%3A%2F%2Flmstudio.ai%2Fmodels)模型运行工具，可以在本地运行大语言模型，避免信息安全问题，以 GUI 的方式使用，下载的时候要注意，Windows 平台有 ARM 和 x86 两个架构的版本，下载错版本的话，安装后无法打开， 模型目录

+   [Ollama ](https://link.juejin.cn/?target=https%3A%2F%2Follama.com%2F)：与 LM Studio 类似，可以用于在本地设备上运行和管理大型语言模型（LLMs），以命令行的方式调用，尤其适合开发者和研究人员快速部署、测试和自定义模型。

在 OpenRouter 上还可以看到有哪些应用使用了他们的 API 。
Cline 的设置页中可以设置自定义指令，自定义指令会插入到每一个请求的系统提示词的结尾。比如输入“优化代码”，自定义提示词是“说中文”，那系统提示词就是“优化代码xxx说中文”。
相关的 Cline 代码如下。
##### 自动批准

Cline 提供了 Auto Approve 功能，这个功能可以在输入框的上方启用，这个功能启用后，Cline 就会自动做下面这几件事，不会征求你的意见，否则当 Cline 的行动就需要获得你的批准。

+   读取文件与目录

+   修改文件

+   执行命令

+   使用浏览器

+   使用 MCP 服务器：模型上下文协议（MCP，Model Context Protocol）是一种开放协议，允许AI模型通过标准化服务器实现，安全地与本地及远程资源进行交互。这些服务器通过文件访问、数据库连接、API集成及其他上下文服务来扩展AI能力

+   最大请求次数：默认值是 20 ，也就是默认情况下， Cline 执行了 20 次自动的生成请求后，就会停止生成
##### 规划与行动

在 Cline 输入框的下方有 Plan 和 Act 两个选项。
Plan 与 Act 的区别：

+   Plan 模式的提示词主要引导 AI 进行分析和规划

+   Act 模式的提示词包含了具体的执行指令和工具使用权限

Plan 模式下的提示词特点：

+   更侧重于分析和规划

+   会要求 AI 详细列出执行步骤

+   不包含实际执行操作的指令

可能会输出类似大纲或计划的格式

Act 模式下的提示词特点：

+   包含了具体的执行权限和操作指令

+   允许使用各种工具命令，如文件操作、执行命令、浏览器操作

Cline 官方推荐的是用 DeepSeek-R1 进行规划，然后用 Claude-3.5-Sonnet 执行。

##### 切换模型

点击输入框下方的模型名称，可以修改使用的模型。
##### 查看历史

如果想回看之前的任务执行记录，可以点击 View all history 。
##### 插入图片与关联特定上下文

点击输入框中的 @ 可以让模型读取特定的文件夹或文件，也可以通过粘贴 URL 的形式提取 URL 对应的网站的内容。
**@problems 的用途**

+   快速添加工作区的错误和警告信息

+   避免来回询问调试信息

+   让 Cline 能够直接看到并修复代码问题

例如，如果你的代码中有类型错误、缺少导入或其他编译/构建问题，你可以使用 @problems 让 Cline 直接看到这些问题，它就能够主动帮你修复这些错误，而不需要你手动复制粘贴错误信息。

比如下图中 @problems 后，Cline 就尝试处理导包错误了。
点击输入框下方的相机可以插入图片，也可以直接把剪贴板中的图片粘贴进输入框。
#### 2.6.2 MCP 服务器介绍

[模型上下文协议 ](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol)（MCP，Model Context Protocol）是 Anthropic 公司提出一种开放协议，允许AI模型通过标准化服务器实现，安全地与本地及远程资源进行交互。这些服务器通过文件访问、数据库连接、API集成及其他上下文服务来扩展AI能力。MCP 服务器是 Model Context Protocol 中的核心组件，负责为大型语言模型（LLM）提供外部数据源和工具的标准化接入，MCP 服务器通过三种基本原语为 LLM 提供支持：资源、工具、提示。

点击 Cline 中右上角的服务器按钮，可以打开 MCP Server 面板。
在 [Servers ](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers)开源库中，可以看到让 LLM 可以看到这几类 MCP Server。

+   知识检索： AWS KB Retrieval

+   搜索： [Brave Search ](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Fbrave-search)（Web与本地搜索）

+   图像生成： [EverArt](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Feverart)

+   MCP 服务器功能展示： [Everything](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Feverything)

+   网页内容提取： [Fetch](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Ffetch)

+   文件操作： [Filesystem ](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Ffilesystem)（本地）、 [Google Drive ](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Fgdrive)（云端）

+   Git 操作： [Git ](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Fgit)、 [GitHub ](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Fgithub)、 [GitLab](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Fgitlab)

+   数据库： [PostgreSQL ](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Fpostgres)、 [Sqlite](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Fsqlite)

+   浏览器操作： [Puppeteer](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers%2Fblob%2Fmain%2Fsrc%2Fpuppeteer)

+   逐步思考： Sequential Thinking

#### 2.6.3 手动配置 MCP Server

##### Server 介绍

Se**quential Thinki**ng MCP Server 是基于 Model Context Protocol (MCP) 协议实现的专用服务器，旨在为大型语言模型（LLM）提供 结构化、动态化的问题解决框架 。它通过分步骤、可回溯、可分支的思考流程，帮助 AI 智能体（如 Claude、GPT 等）更高效地处理复杂任务，尤其适用于需要逻辑推理、多步骤规划或动态调整的场景。

**核心功能 1 ：动态结构化思考**

+   **问题分解 **：将复杂问题拆解为可管理的子步骤（例如：软件开发任务可拆分为需求分析→架构设计→模块实现→测试）。

+   **动态调整 **：根据推理进展，实时增减总步骤数（ totalThoughts 参数），灵活应对不确定性。

+   **分支推理 **：支持从某一步骤（ branchFromThought ）衍生多个推理路径（通过 branchId 标识），用于探索不同解决方案。

+   **修订机制 **：允许通过 isRevision 和 revisesThought 参数回溯修改特定步骤（例如：发现错误后修正第三步的结论）。

**核心功能 2 ：假设验证与迭代**

+   生成解决方案假设（如“选择算法A”），通过外部工具验证后反馈给思考流程。

+   若验证失败，自动触发修订或分支逻辑（例如：“算法A不适用→尝试算法B”）。

**工具接口 ( **s**equ**ential_thinking ) ：

输入参数：

| 参数 | 类型 | 作用 |
| --- | --- | --- |
| thought | string | 当前步骤的思考内容（如“分析用户需求”） |
| nextThoughtNeeded | boolean | 标记是否需要继续下一步思考（控制流程终止） |
| thoughtNumber | integer | 当前步骤编号（从1开始） |
| totalThoughts | integer | 预估总步骤数（可动态调整） |
| isRevision | boolean | 是否为修订操作（默认false） |
| revisesThought | integer | 指定要修订的步骤编号（需与isRevision=true配合） |
| branchFromThought | integer | 分支起始步骤编号（如从步骤3分出两条路径） |
| branchId | string | 分支标识符（例如：“路径A”、“备选方案1”） |
| needsMoreThoughts | boolean | 请求增加总步骤数（例如：原计划5步，实际需要7步） |

**典型使用场景：**

+   **软件开发 **：逐步设计系统架构 → 发现性能瓶颈 → 回溯修改设计 → 分支探索优化方案。

+   **数据分析 **：假设生成（“销量下降因季节因素”）→ 验证假设（查询历史数据）→ 若被推翻则生成新假设。

+   **项目管理 **：分解任务 → 动态调整优先级 → 处理突发风险（插入新步骤）。

**通过 Docker 部署 **（适合生产环境）：

```
{
  &quot;mcpServers&quot;: {
    &quot;sequentialthinking&quot;: {
      &quot;command&quot;: &quot;docker&quot;,
      &quot;args&quot;: [
        &quot;run&quot;,
        &quot;--rm&quot;,
        &quot;-i&quot;,
        &quot;mcp/sequentialthinking&quot;
      ]
    }
  }
}
```

**使用 Docker 构建镜像：**

克隆或下载 [Servers ](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmodelcontextprotocol%2Fservers)开源库后，进入 servers 目录，执行下面的命令。记住：不要进入 sequentialthinkging 目录执行下面的命令，因为下面的命令指向的就已经是 sequentialthinking 目录中的 Dockerfile。

```
docker build -t mcp/sequentialthinking -f src/sequentialthinking/Dockerfile .

![](img/486c142f13507f481f1bb7f954c892cc.png)

```

构建成功时显示的内容。
构建成功后执行下面的命令。

```
docker run mcp/sequentialthinking

![](img/002edfd3c8a7c19e2dabe61cee2e143f.png)


![](img/667a3c01616c6651fbeca39245ba3ba2.png)

```

##### **使用效果**

当我的提示词是“SequenialThinking 是什么”的时候，Gemini 就会开始尝试用 use_mcp_tool 工具来调用 MCP 服务。
可以勾选自动同意使用这个工具。
#### 2.6.4 自动创建 MCP Server

##### 创建 Server

下面是一个通过 Cline + Gemini 2.0 自动创建一个自定义 MCP Server 的例子。

输入提示词“创建一个 MCP Server，用于从 YouTube 视频中提取字幕。”后，Gemini 会要求执行命令，这个命令用于创建 MCP Server 的源码。

```
cd /Users/oushaoze/Documents/Cline/MCP &amp;&amp; npx @modelcontextprotocol/create-server youtube-transcript-server &amp;&amp; cd youtube-transcript-server &amp;&amp; npm install axios youtube-transcript

![](img/1139bddea269408c7fd26f101dd6335b.png)


![](img/52f3e5107eca1af39b81b2f83714fc1d.png)


![](img/aebc5d8905a7e2650838e8340a035a89.png)


![](img/56979a464bdc0d42b8fba6b6c9698b1e.png)


![](img/bb8157733fc2b7936f933f194ae864c4.png)


![](img/07c88d37f6cf000543005d6268763ace.png)


![](img/08853f8f1f7fda5002902dcd19e6d30c.png)


![](img/c25a4c58cf381672b7743580e76abe69.png)


![](img/f044113100c8cf5d13fa78c6c5667ed4.png)


![](img/a166fe4e378cc5f08cf9bb65147e5390.png)

```
写完项目源码后，Gemin 就会尝试运行 MCP Server。
而且 Gemini 还会修改 Cline 的 MCP Server 配置。
源码所在的位置：/Users/xxx/Documents/Cline/MCP/youtube-transcript-server

##### 使用效果

当我输入“提取 youtube 字幕： www.youtube.com/watch?v=liE… Gemini 就会调用 youtube-transcript-server 工具，
最后并没有处理成功，可能是因为这个获取字幕的 MCP Server 的实现不完整。
通过 Cursor 调整源码后，再用 Cline 获取字幕，效果如下。在遇到错误的时候会尝试调用 SequentialThinking 找出解决方案。
#### 2.6.5 Cline 提示词

下面是执行任务的过程中，Cline 通过读取文件内容和环境信息，一起放到 API 请求中发送给模型的部分内容，点击 API Request 右边的按钮就可以展开查看具体的内容。
从下面的内容中可以看到 Cline 会把打开的标签、模式（act/plan）、文件内容等信息都传给模型，这意味着如果你只是说“优化代码”的话，对于不够智能的模型（比如7b 的模型）来说，可能会先解释项目目录中有哪些内容，再来处理任务。
Cline 是通过下面的提示词让 LLM 使用 MCP Server 的，提示词大约有 12000+ 个 token。
翻译后的提示词如下，提示词的其中一部分就是引导模型输出调用 use_mcp_tool 工具的文本。
在 Cline 的代码中，定义了下面这些工具，其中就包括 use_mcp_tool。

```
export const toolUseNames = [
    &quot;execute_command&quot;,
    &quot;read_file&quot;,
    &quot;write_to_file&quot;,
    &quot;replace_in_file&quot;,
    &quot;search_files&quot;,
    &quot;list_files&quot;,
    &quot;list_code_definition_names&quot;,
    &quot;browser_action&quot;,
    &quot;use_mcp_tool&quot;,
    &quot;access_mcp_resource&quot;,
    &quot;ask_followup_question&quot;,
    &quot;plan_mode_response&quot;,
    &quot;attempt_completion&quot;,
] as const
```

ToolUse 接口的其中一个实现接口是 UseMcpToolToolUse 。

![](img/50950ca8e8c936aed37a8e6e465afd60.png)

### 2.7 Roo Code

Roo Code 是基于 Cline 做的一个 AI 编程智能体插件。

Roo Code 相对于 Cline 的优势：

+   提示词：更细致的提示词设置，如输出的语言、提示词增强、不同的角色（编码、架构优化、提问）

Roo Code 相对于 Cline 的劣势：

+   不支持 Ollama

#### 2.7.1 通用设置

##### 1、API 提供者

Roo Code 的 API 提供者的设置支持多套配置，输入框下方的 default 表示的就是默认的配置，点击 Edit 就可以进入设置页添加另一套配置。

![](img/d2db4eed3ced0bc0605291e859ef2af1.png)

![](img/4e0cd20695c0ff8ec5801c6c789f2fee.png)

在本地大语言模型工具的支持方面，Roo Code 只支持 LM Studio，不支持 Ollama ，而 Cline 是支持的。LM Studio 不支持使用 Intel 芯片的 MacOS 客户端。

如果选择的 API 提供者是 OpenRouter ，那在 Roo Code 中还可以自定义 API 的 baseUrl 。为了让自定义的 baseUrl 可以访问 GLM 等模型，我做了一个简单的示例仓库 OpenRouterRedirect 。 此外选择 OpenRouter 作为 API Provider 的时候， Roo Code 还支持启用提示词压缩，这个设置项可以减少 token 的消耗。

![](img/ce1c8333439883272b1bd93551b2c157.png)

![](img/d295aa25587e236e04e11bafd89fd451.png)

![](img/6347347d7f410f8fdfcc08b78e46b11f.png)

##### 2、自动批准

Cline 只能在对话框里调整 Auto-Approve 的配置，而 Roo Code 可以在设置里调整 Auto-Approve 的配置，Roo Code 的 Auto-Approve 中可以设置自动重试的机制，这样的话当 API 报错的时候就不需要自己点击重试，而且在 Roo Code中还可以设置终端输出的上限。

![](img/d5f895279258b57761f8d4c7a72330a7.png)

![](img/edd2ec7416db82b2e39506a0cf144bc3.png)

##### 3、浏览器视窗

Roo Code 还支持浏览器视窗大小、通知音效、终端输出的行数限制等设置。

![](img/27651fed7789ae65ba2759e2125e6d63.png)

#### 2.7.2 提示词设置

Chat Mode是Roo Code的核心功能之一，为用户提供了多种灵活的工作模式，以适应不同的开发需求。在对话框中可以切换和修改不同模式下的提示词，点击 Edit 可以可以打开提示词设置页。

+   **Code模式 **：这是Roo Code的默认模式，专注于日常开发任务。无论是编写代码、修复问题，还是执行复杂的任务流，Code模式都能提供强大的支持。例如，用户可以直接输入自然语言指令，让Roo Code自动生成代码片段、优化现有代码，甚至执行文件操作。这一模式适合需要快速完成编码任务的开发者。

+   **Architect模式 **：此模式专注于高层次的系统设计与架构分析。通过预设的提示词，Roo Code能够帮助用户规划技术方案、设计系统架构，甚至提出优化建议。与Code模式不同，Architect模式不会直接编写代码或执行命令，而是更注重逻辑分析和技术决策。这种分离的设计使得开发者能够更清晰地思考全局问题。

+   **Ask模式 **：Ask模式是一个知识型助手，适用于研究和技术问题解答。用户可以询问有关代码库、编程概念或技术实现的问题，而Roo Code会基于上下文提供详尽的解答。它特别适合需要深入探讨某些复杂概念或进行代码审查的场景。

![](img/d5f90fff80f519ee0b159d0da053db3b.png)

在提示词设置页中，第一个设置项就是语言。

![](img/b9b54781d6b18b28bb860b5dee945f6f.png)

![](img/5b8ec75983972e4d0bebbd4063787287.png)

Roo Cline 中的提示词分为了通用指示和模式特定的提示词。比如通用提示可以是“你是一个全栈开发者”，点击加号可以添加特定模式的提示词，比如“代码解释”、“代码审查”、“单元测试编写”。可以自定义提示词，意味着把提示词改为中文后，能减少模型输出英文的概率。

在设置中点击 .clinerules 就会看到 Roo Cline 创建了 .clinerules ，这个文件的配置和 .cursorrules 类似，都用于设置项目级别的提示词。

![](img/20dac3248790516675eab286143b7b37.png)

![](img/6cebc06d2480ebfcb76850bf0b10c6d7.png)

特定模式的提示词还可以分别设置不同的自定义提示词。

![](img/9879449e2a8d521fe34fdfcd92f21b1b.png)

点击 “Mode-Specific Prompts”后面的加号，可以创建新的模式，在这里可以设置该模式下可以调用的工具。

![](img/3181aa5e00ae20c8d98cd1a7f05d2058.png)

在 Prompts 设置中可以还可以看到 Roo Code 传给模型的系统提示词，但是不支持编辑系统提示词。

![](img/64b08dc4e899ccf7a6d8dba2ade07e2d.png)

![](img/51878585d193b2e46ed92862b1602809.png)

此外 Roo Code 还支持提示词增强，主要是为了帮助我们优化简单的输入，比如说我输入“优化代码”，Roo Code 会把这个提示词转变为“优化和改进代码以提高性能和可维护性”。

![](img/907197812d107e8c493131164ab0a38d.png)

![](img/3f7d61ebe486ded7d666bded8e23f938.png)

点击对话框中相机左侧的按钮就可以对提示词进行增强。

![](img/4ea87471a4e9660240799d40f9d78fd0.png)

![](img/d25f855bf4cfe99b88b3734d5b36435f.png)

### 2.8 Warp

![](img/4c698920621f4261f202c1fe137044a4.png)

#### **核心功能**

1.  **界面与个性化**

1.  自定义提示符：支持主题库（如 Powerlevel10k、Starship）或通过 GUI 自定义主题配色。

1.  输入框位置：可固定到窗口顶部或底部，支持透明背景和透明度调节。

1.  **现代文本编辑体验**

1.  **IDE 式操作 **：鼠标交互、Vim 键绑定、智能命令补全（支持数百种常见命令）。

1.  **块结构（Blocks） **：将输入和输出分组，便于导航、过滤和分享。

1.  自动纠错：检测拼写错误或缺失参数并提供修正建议。

1.  **AI 增强功能**

1.  **自然语言查询 **：用自然语言搜索命令，替代传统搜索引擎。

1.  **AI 工作流辅助 **：自动生成命令建议、命名并描述工作流（Warp Drive）。

1.  **上下文感知建议 **：根据当前会话和历史命令推荐下一步操作。

1.  协作与云端驱动（Warp Drive）

1.  **团队协作 **：共享云端驱动（Team Drive）、实时会话共享（Beta）和代码块永久链接。

1.  **个人云端库 **：保存参数化命令、环境变量、交互式手册（Notebooks），支持网页端访问。

1.  **生产力工具**

1.  **命令面板 **：快速搜索应用内功能、快捷键和设置。

1.  **历史记录增强 **：查看退出码、目录详情、关联分支等元数据。

1.  Markdown 查看器：直接运行 Markdown 文件中的嵌入命令。

1.  **兼容性 与隐私安全**

1.  **多平台支持 **：兼容 macOS、Linux（Windows 等待名单中），支持 zsh/bash/fish。

1.  **隐私保护 **：默认不收集输入输出数据，禁用分析追踪，企业级 SSO/SAML 集成。

1.  **AI 数据安全 **：用户数据不用于训练 OpenAI/Anthropic 模型，企业版支持零数据保留。

#### 使用示例

下载 Warp 后可以看到一个弹窗，点击确认后则开始启用 AI 功能，终端里还会提示你可以输入自然语言而不是命令。

![](img/8171bbd9d1ed3e62348080a696d8b8dd.png)

![](img/f9a1c37990c1ee12fd3b6d0e7273ffec.png)

比如我输入“安装 python3”，失败后则会提示我改用命令“brew install python3”，这时我按下 Tab 键，就能看到命令已经输入好了。

![](img/906e689deebeb210203e2a9cae908f77.png)

![](img/312b3accdfef90a5c1efc1eb11446d86.png)

在 Warp 中可以切换的模型有 Claude 3.5 sonnet 、haiku 和 gtp-4o。

![](img/4700d1469dbafd489da1c217ee87cc66.png)

Command + i 可以切换为 Agent 模式。

![](img/4c9ee9b9af8ac57e89df74afed3a054a.png)