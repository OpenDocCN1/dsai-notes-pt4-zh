
# 一句话做了个Coze？Gemini 3 Pro让我懵了

> 来源：[https://my.feishu.cn/docx/BXrsddcEZocxWux7la3cJ1cMnOZ](https://my.feishu.cn/docx/BXrsddcEZocxWux7la3cJ1cMnOZ)

那天晚上睡不着，打开Google AI Studio瞎玩。

想试试Gemini 3 Pro到底有多牛，就随手敲了一句：

```
做一个类似Coze的LLMOps，可以编排各个节点，并可在右侧直接预览结果。
节点你都要考虑你可以使用什么，尽可能的多。
```

说完我就去倒水。

回来一看，愣住了。

屏幕上出现了一个完整的低代码编排平台。

左边是节点面板，中间是可视化画布，右边是实时预览区。

我点了下预览，输入"画个小可爱"。

3秒后，一只萌猫出现在右边。

这玩意儿，真能跑。

**文章结尾奉上反推的提示词。**

![](img/edd81f6a8f8507109f762400a91ccac7.png)

## 它给我生成了什么

让我给你说说这个"GeminiFlow Ops"到底长什么样。

![](img/782b0b75dfd4f27b70dfcc7b271c1796.png)

### 左边：节点工具箱

**逻辑核心**：

开始节点、智能路由Agent、条件判断`IF`、`JS`代码执行、结束节点。

**Google工具集**：

地图Maps、YouTube、Gmail、日历Calendar、网盘Drive、旅行助手。

**生成模型**：

Gemini文本、Imagen 3图片生成、Veo 3.1视频生成、语音合成。

### 中间：`React Flow`画布

拖节点，连线，配参数。

跟Coze、Dify一模一样的操作逻辑。

但这玩意儿是AI用5分钟写出来的。

### 右边：实时预览

输入内容，点运行，直接看结果。

底部还有状态显示：`[start]` running → `[router]` running → `[llm]` running → `[image_gen]` running → `[end]` running。

整个流程可视化，一目了然。

## Google全生态，一句话全接入

这才是最狠的地方。

Gemini自己知道它能调用什么，所以节点库里全是Google服务。

**Workspace全家桶**：

Gmail收发邮件、Calendar日程管理、Drive文件存储、Docs文档编辑、Sheets表格处理。

**地图和旅行**：

Maps地图定位、Flights航班查询、Hotels酒店预订、Travel旅行规划。

**媒体服务**：

YouTube视频搜索、Google Search实时搜索。

**AI生成能力**：

Gemini文本生成、Nano Banana Pro图片生成（4K，中文完美）、Veo 3.1视频生成、语音合成TTS。

你在Coze里接一个服务，要配`API Key`，要调试，要处理报错。

这里？

一句话，全给你接好了。

## 对比一下就知道有多离谱

用Coze/Dify从零搭建一个LLMOps平台：

学习平台操作，熟悉节点配置，接入各种`API`，调试数据流转，处理报错。

保守估计：3-5天。

用`Gemini 3 Pro` + `Canvas`：

说一句话，等5分钟，直接用。

实际花费：5分钟。

而且这还不是最狠的。

最狠的是，你可以继续用自然语言改它。

"把`YouTube`节点删掉。"

删了。

"加一个语音输入节点。"

加了。

"把整个流程改成先搜索再生成图片。"

改了。

也可以让他记录上下文，类似这样。

这里我是丢个图进去，它居然就懂我意思了。

![](img/493d1c758848dd8a37082be4aacb2277.png)

增删改查，全用嘴说。

**这才是真正的低代码。**

**不对，这是零代码。**

## 这说明什么

老金我琢磨了一晚上，想明白一件事。

以前我们说"AI编程"，是AI帮你写代码。

现在不一样了。

AI直接帮你做产品。

你说需求，它出成品。

不是代码片段，是完整的、能跑的、有界面的产品。

Coze、`Dify`这些平台，本质上还是工具。

你得学会用它，才能做出东西。

但`Gemini`这个玩法，工具本身都是AI生成的。

你不需要学任何平台。

**你只需要会说人话。**

## 怎么玩

打开 `https://aistudio.google.com/`

选`Gemini 3 Pro`，进`Canvas`模式。

![](img/4f9a79af74b1b9170bc9c4253e716016.png)

说一句类似的话：

"做一个类似Coze的LLMOps平台，可以编排节点，右侧实时预览，节点尽可能多。"

等3-5分钟，看它表演。

生成完可以继续用自然语言调整。

**注意：**

`Veo 3.1`视频生成需要单独的`API Key`。

## 写在最后

那天晚上我盯着屏幕看了很久。

以前折腾Coze、`Dify`，光是搞明白怎么用就花了好几天。

现在一句话，5分钟，平台就有了。

而且还自动接入了Google全家桶。

这不是"AI辅助开发"。

这是"AI替你开发"。

工具都不用学了，直接说要什么。

这才是AI该有的样子。

## 彩蛋：

我让他生成了简要版的提示词，大家可以直接拿去用。

```
请帮我构建一个基于 `React 19`、`Tailwind CSS` 和 `React Flow` 的中文版 可视化 LLMOps 编排平台。

核心功能要求：
1.  节点编排：支持拖拽 `Start`、`End`、`LLM`（`Gemini`）、`Code`（`JS`执行）、`Router`（智能意图路由）、`Condition`（条件判断）以及 Google 全家桶工具节点（`Maps`, `YouTube`, `Gmail`, `Calendar`, `Drive`, `Veo`视频, `Imagen`绘图）。
2.  智能执行引擎：
    *   实现基于图遍历的工作流执行。
    *   `Router` 节点需作为大脑，根据用户输入和多轮对话历史自动分发任务。
    *   针对媒体生成（音/视/图），实现双路路由逻辑：既支持通过 `Planner LLM` 优化提示词，也支持直接生成。
3.  界面交互：
    *   左右侧边栏可折叠。
    *   节点配置面板需支持查看详细运行日志（输入/输出/耗时）。
    *   中间画布连线需带删除按钮。
4.  Chat 预览：
    *   右侧实时对话框，支持 `Markdown` 渲染。
    *   特色功能：自动解析文本中的 `YouTube` 链接并渲染为带有封面的视频卡片。
5.  技术细节：
    *   使用 `@google/genai` SDK，并封装重试机制处理网络错误。
    *   所有 AI 回复强制设定为简体中文。
    *   使用 `LocalStorage` 保存工作流状态。

请生成完整的项目代码，包含 `App.tsx`, `geminiService.ts` 等关键文件。
```

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
