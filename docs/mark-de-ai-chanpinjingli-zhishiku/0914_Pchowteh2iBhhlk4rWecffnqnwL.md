# Claude Code 集成 GLM-5 构建互动小说创作工具

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/OKr8dXfCXoux0axOV21c9XeHn8f](https://qqs7y1hozd1.feishu.cn/docx/OKr8dXfCXoux0axOV21c9XeHn8f)

#

一、摘要

在生成式AI快速演进的今天，互动小说（Interactive Fiction）作为一种融合叙事性与用户参与感的数字内容形式，正迎来新的创作范式。传统互动小说依赖作者手动编写大量分支剧情，开发成本高、迭代慢。而借助大语言模型（LLM）的上下文理解与生成能力，配合流式响应机制，可以显著降低创作门槛并提升内容动态性。

本文围绕如何使用Claude Code与 GLM-5 API 构建一个轻量级但功能完整的互动小说创作工具展开。Claude Code 是Anthropic推出的AI编程助手，支持在终端、IDE中进行自然语言编程，实现从灵感到代码的高效创作；而 GLM-5 作为智谱新一代的旗舰基座模型，在叙事生成、角色一致性与上下文记忆方面表现同样优异。

本文将带你从零开始，利用 Claude Code AI编程工具，结合 智谱 AI 提供的 GLM-5 大模型API，亲手打造一个智能小说生成器。它不仅能根据你的开头智能生成多个精彩故事分支、自定义标题与开头输入、创作历史管理（包括查看、继续创作与一键删除），以及对已生成内容进行局部调整和重新续写，堪称你的私人AI小说创作助手。

二、Claude Code与智谱GLM-5模型介绍

2.1 Claude Code 工具介绍

Claude - 官方

Claude Code 是Anthropic 推出的AI编程 助手，支持在终端、VS Code、JetBrains 等 IDE 中使用。它能够理解自然语言指令，直接生成、修改和调试代码。

告别传统”搜索-复制-粘贴”的编程模式，Claude Code 本质上是 AI 驱动的智能编程伙伴。通过自然语言对话，Claude Code 能够准确理解开发意图并生成高质量代码，支持文件读取、编辑、执行命令等操作，实现 所想即所得的编程体验。在这种 AI 辅助的开发模式中，开发者可以专注于创意和逻辑，而将繁琐的编码工作交给 AI。

Claude Code 最强大的能力在于其深度上下文理解，能够分析整个项目结构，理解代码依赖关系，并给出符合项目风格的代码建议。同时支持多文件编辑、运行测试、Git 操作等，让开发效率大幅提升！

2.2 智谱 GLM-5 模型介绍

智谱AI - 官方

GLM-5 是智谱新一代的旗舰基座模型，面向 Agentic Engineering 打造，能够在复杂系统工程与长程 Agent 任务中提供可靠生产力。在 Coding 与 Agent 能力上，GLM-5 取得开源 SOTA 表现，在真实编程场景的使用体感逼近 Claude Opus 4.5，擅长复杂系统工程与长程 Agent 任务，是通用 Agent 助手的理想基座。GLM-5 从 355B（激活 32B）扩展至 744B（激活 40B），预训练数据从 23T 提升至 28.5T，更大规模的预训练算力显著提升了模型的通用智能水平，具备六大核心能力：

高级编码能力：在公开基准与真实编程任务中，GLM-5 的代码能力对齐 Claude Opus 4.5，是国内已知的最好的 Coding 模型。

上下文长度：上下文窗口由 128K→200K，适应更长的代码和智能体任务。

推理能力：推理能力提升，并支持在推理过程中调用工具。

搜索能力：增强了模型在工具调用和搜索智能体上的表现，在智能体框架中表现更好。

写作能力：在文风、可读性与角色扮演场景中更符合人类偏好。

多语言翻译：进一步增强跨语种任务的处理效果。

使用智谱 GLM-5 模型来赋予Ai小说创作能力那是非常合适！

三、应用效果展示

3.1 欢迎页

首页（用户未配置智谱GLM-5模型）：

3.2 故事创建

故事创作页（核心，树形结构清晰展示整个故事的发展路径与分支概览）：

3.4 创作时光

创作时光（历史查看及故事回溯界面）：

3.5 API配置管理

API配置页面（后续还可进行优化 配置更多模型 丰富小说生成图片能力）：

四、Claude code 集成 GLM-5

4.1 质谱模型API Key获取

1.  首先我们去智谱官网注册账号并进行实名认证，然后创建自己的的API Key：智谱API Key创建

1.  查看智谱GLM-5 API开发文档，AI小说创作能力的强弱和使用的模型能力关系非常大。

API 调用实例（后续替换成个人API Key）：

curl -X POST "https://open.bigmodel.cn/api/paas/v4/chat/completions" \

-H "Content-Type: application/json" \

-H "Authorization: Bearer your-api-key" \

-d '{

"model": "glm-5",

"messages": [

{

"role": "user",

"content": "作为一名营销专家，请为我的产品创作一个吸引人的口号"

},

{

"role": "assistant",

"content": "当然，要创作一个吸引人的口号，请告诉我一些关于您产品的信息"

},

{

"role": "user",

"content": "智谱AI 开放平台"

}

],

"thinking": {

"type": "enabled"

},

"max_tokens": 65536,

"temperature": 1.0

}'

一键获取完整项目代码

curl -X POST "https://open.bigmodel.cn/api/paas/v4/chat/completions" \

-H "Content-Type: application/json" \

-H "Authorization: Bearer your-api-key" \

-d '{

"model": "glm-5",

"messages": [

{

"role": "user",

"content": "作为一名营销专家，请为我的产品创作一个吸引人的口号"

},

{

"role": "assistant",

"content": "当然，要创作一个吸引人的口号，请告诉我一些关于您产品的信息"

},

{

"role": "user",

"content": "智谱AI开放平台"

}

],

"thinking": {

"type": "enabled"

},

"stream": true,

"max_tokens": 65536,

"temperature": 1.0

}'

一键获取完整项目代码

4.2 小白上手配置教程

方式一：质谱官方提供安装助手安装

这里我们使用质谱官方提供的一键安装助手来接入Claude Code工具，通过这个安装助手可以非常方便的集成各种开发工具。

未安装Claude code时候可以通过这个脚本一键安装（这里我已经安装，所有通过其他编码工具展示）

方式二：命令方式安装

Claude Code - 智谱AI开放文档

安装Claude code之前需要确保已安装

Node.js 18或更高版本

Git

安装命令：

npm install -g @anthropic-ai/claude-code

一键获取完整项目代码

配置 GLM Coding Plan

支持 MacOS & Linux & Windows, 注意不同系统配置文件路径不一样。注意需保证修改的 JSON 文件格式正确性(比如多或少,)

# 编辑或新增 settings.json 文件

# MacOS & Linux 为 ~/.claude/settings.json

# Windows 为用户目录/.claude/settings.json

# 新增或修改里面的 env 字段

# 注意替换里面的 your_zhipu_api_key 为您上一步获取到的 API Key

{

"env": {

"ANTHROPIC_AUTH_TOKEN": "your_zhipu_api_key",

"ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic",

"API_TIMEOUT_MS": "3000000",

"CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1

}

}

# 再编辑或新增 .claude.json 文件

# MacOS & Linux 为 ~/.claude.json

# Windows 为用户目录/.claude.json

# 新增 hasCompletedOnboarding 参数

{

"hasCompletedOnboarding": true

}

一键获取完整项目代码

开始使用 Claude Code

配置完成后，进入一个您的代码工作目录，在终端中执行 claude 命令即可开始使用 Claude Code，启动后选择信任 Claude Code 访问文件夹里的文件。

确认后可以正常使用 Claude Code 进行开发

如果想要自己配置模型的话可以在配置文件里面进行配置

编辑 C:\Users\用户名称\.claude\settings.json：

{

"env": {

"ANTHROPIC_DEFAULT_HAIKU_MODEL": "glm-5",

"ANTHROPIC_DEFAULT_SONNET_MODEL": "glm-5",

"ANTHROPIC_DEFAULT_OPUS_MODEL": "glm-5"

}

}

一键获取完整项目代码

五、应用开发 实战

5.1 提示词搭建

输入提示词，最好是提供结构化的提示词，最开始我以日常对话式进行coding，发现最终效果没有结构化提示词的好，后续我基本通过结构化的需求形式让Claude code进行开发

我需要开发一款Ai小说创作平台，名称为智能小说创作家，流程是用户提供故事的标题和50-200字左右的小说故事开头，提供完应用会调用GLM-4.6模型，生成后续五个情节分支概览，用户可以任意选择某一分支。选择分支之后立马调用GLM-4.6生成具体的情节，大约300-500百字左右。未被选择的分支废弃，同时用户还可以手动修改分支内容创作，选择的分支使用树形结构一级一级往下展示，但是始终展示当前的分支，持续重复。采用React技术，风格使用新粗野风格，多用白色、橙色、绿色、蓝色、灰色等，流畅的交互设计。无需登录，只需要几个页面即可，首页核心创作功能，API Key 的配置页面（保存到本地的local storage中）。

GLM-5调用示例：

curl -X POST "https://open.bigmodel.cn/api/paas/v4/chat/completions" \

-H "Content-Type: application/json" \

-H "Authorization: Bearer your-api-key" \

-d '{

"model": "glm-5",

"messages": [

{

"role": "user",

"content": "作为一名营销专家，请为我的产品创作一个吸引人的口号"

},

{

"role": "assistant",

"content": "当然，要创作一个吸引人的口号，请告诉我一些关于您产品的信息"

},

{

"role": "user",

"content": "智谱AI 开放平台"

}

],

"thinking": {

"type": "enabled"

},

"max_tokens": 65536,

"temperature": 1.0

}'

一键获取完整项目代码

5.2 Vibe Coding 交互编程

可以看到，这里Claude Code 开始理解需求并生成代码

这里还会生成相应的文档，如果我们想要调整的话，可以进行编辑或删除；对于应用开发，技术文档就是领头羊，防止脱离实际需求。

在我体验很多产品后发现，技术文档越是完善，应用开发效果就越完美。

同时，作为程序猿的小伙伴们，如果觉得Ai提供的页面不太完善，同时我们想自己上手操作的话，可以手动修改源码调整，这样就能完全由我们自己控制偏差。

在我开发途中，出现了一个小插曲。当我提出大改样式的需求后，他将所有页面背景图片调整成同一个，但是我希望使用不同背景来使样式更加丰富，这个时候也可与通过提供大致想要的背景图片，让他去修改。

质谱提供了四个比较常用的MCP工具，体验下来非常不错，同样也可与通过助手一键安装。

六、总结与展望

6.1 总结

通过 Claude Code 集成 智谱GLM-5 模型，我构建了一个高效、可扩展的互动小说创作工具原型。该工具不仅验证了流式 LLM 在动态叙事中的可行性，也为内容创作者提供了”AI 协同创作”的新范式。同时我们成功将「AI小说创作」从概念落地为可交互的生产力工具。

核心亮点：

1.  开发效率革命：传统开发需 1 周 → Claude Code 集成质谱最强GLM 5模型实现极速开发构建

1.  创作体验革新：从「手动写分支」到「AI 动态生成树形叙事」

1.  技术可扩展性：架构支持无缝接入语音、多角色记忆等新能力

6.2 实践展望

在实际开发中，我深刻体会到：技术选型必须服务于用户体验——Claude Code 的智能代码生成能力有效提升了开发效率，而 GLM-5 模型提供的中文叙事能力则大幅提升了剧情质量。未来，该工具可进一步集成语音输入、多角色对话记忆、剧情质量评估（如一致性检测）等功能。

更重要的是，这种”状态 + 流式生成”的架构可迁移至教育、游戏、客服等多个领域。技术的价值不在于炫技，而在于解决真实问题。希望本文能为探索生成式 AI 应用落地的同行提供有价值的参考。互动小说只是起点，人机共创的叙事革命才刚刚开始。