
# Cursor Chat 与 Composer

> 来源：[https://v11enp9ok1h.feishu.cn/docx/AONZdmk55oiZoCxaApGcwOFanLb](https://v11enp9ok1h.feishu.cn/docx/AONZdmk55oiZoCxaApGcwOFanLb)

Cursor AI 聊天窗口中有 Chat 和 Composer 两个功能，它们是两种不同功能的 AI 辅助编程模式：

+ Chat 是一个交互式的 AI 助手，你可以用自然语言与它对话，提出问题、请求代码解释、进行代码审查、查找错误等。它更侧重于交互式的问答和代码辅助，适合快速获取解决方案、调试代码或学习新概念。

+ Composer 是一个强大的代码生成工具，主要用于生成全新的代码。你可以提供需求描述或示例代码，让 Composer 生成完整的代码片段或文件。它更侧重于代码的生成，适合构建新功能、优化代码结构或处理复杂的编程任务。

**简单**来说 Chat 适合快速问答和简单任务，Composer 适合复杂代码生成和跨文件操作，平时开发我们可以先用 Composer 生成代码，再用 Chat 对代码进行解释或优化。

![](img/8cf289d141b5459b609f553d93bb2b3d.png)

选择 Chat 还是 Composer 取决于任务需求：

+ 如果需要快速问答、即时解决问题，Chat 是更好的选择。

+ 如果需要生成复杂代码、处理多个文件或进行精细调整，Composer 更适合。

## Cursor Chat 介绍

Cursor Chat 是一个集成在代码编辑器中的聊天工具，它允许开发者在编写代码的同时与团队成员进行实时沟通。通过 Cursor Chat，开发者可以在不离开编辑器的情况下讨论代码问题、分享代码片段或寻求帮助。

### 主要功能

+ 对话式交互：用户可以像与真人对话一样，用自然语言提问或请求代码片段。

+ 快速响应：适合即时问答，如代码调试、技术概念解释等。

+ 上下文感知：Chat 会记住之前的对话，支持逐步深入提问。

+ 代码引用：可以直接引用代码片段，让 AI 解释或修改

## 实例

```
expo**rt def**ault function AboutPage() {
    return &lt;h1&gt;关于我们，RUNOOB.COM&lt;/h1&gt;;
}
```

接下来我们在 Chat 对话窗口，让 AI 添加注释。

![](img/7283c85988ab97401823c92643c1395b.png)

然后 点击 `Apply` 按钮，就可以应用到代码文件中了：

![](img/0736a5e27b6a5b89e0837f01694a4ab9.png)

## Composer 介绍

Composer 可以根据自然语言描述，Composer 可以生成完整的代码片段或文件，甚至构建整个应用程序，同时支持同时编辑多个文件，能够理解文件之间的调用关系，适合复杂项目的开发。

Composer 能够理解整个项目结构和现有代码，生成更符合逻辑、更高质量的代码。

Composer 生成代码后，用户可以通过额外指令进一步优化代码。

比如我们可以创建一个目录，编写需求文档：

![](img/c294cea4e954c23a17611b81e02ff1dc.png)

在 Composer 对话窗口就可以让他根据需求文档生成代码：

![](img/b15c1f30dc7cf0c63c0433b723dbc556.png)
