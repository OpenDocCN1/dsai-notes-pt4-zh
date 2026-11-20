
# 用 Cursor 四步搞定产品原型图

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/EdOQdwYa8o85Udxn9ZOc2x4ingy](https://qqs7y1hozd1.feishu.cn/docx/EdOQdwYa8o85Udxn9ZOc2x4ingy)

## 核心提示词模板

```
你是一名精通 UI 设计和产品规划的产品经理，你的目标是完成一个&quot;健身普拉提&quot;iOS App 的开发。

你的核心任务是输出一套完整的APP原型图（HTML页面形式）来辅助后续的开发任务。

核心执行点：

- 明确功能与页面： 请你构思并确定&quot;健身普拉提&quot;App的核心功能模块。基于这些模块，规划出需要设计的HTML页面清单。
- 产品与UI/UX设计：
  - 以产品经理的视角规划APP的关键功能、页面流程和交互逻辑。
  - 以设计师的视角输出符合现代iOS App风格的、美观且用户友好的UI/UX。

技术规范：

- 使用 HTML5、Font Awesome、Tailwind CSS 和必要的 JavaScript（用于基础交互）。
- 图片素材请使用 Unsplash。
- 代码应简洁，注重可读性。

输出要求：

- 创建一个包含多个 HTML 页面的原型。
- 主页面命名为 `index.html`，它可以整合或跳转到其他页面。
- 非主页面HTML文件使用其对应的核心功能名称进行命名（英文，例如 `courses.html`, `profile.html`）。
- 每个页面均需采用 iOS App 的风格生成。
- `index.html` 中，每行展示两个主要功能模块的入口或页面预览。
- 所有输出（包括代码内注释和页面文本）永远用简体中文。
- 请以顶级UX的眼光和审美标准，创造令人满意的设计。

请直接开始设计并输出上述要求的HTML原型页面代码，从 `index.html` 开始，然后是其他你规划的核心功能页面。
```

这个提示词模板可应用于任何你想设计的 APP。特别适合以下场景：

1. **快速验证产品创意**：当你只有一个点子，但不知如何推进时，让 AI 结合最佳实践帮你设计初稿

1. **高效制作原型**：生成的原型不仅视觉完整，还带有基础交互功能

1. **客户/领导汇报**：需要提供 POC 时，可以快速将 HTML 文件部署到网络，或直接发送本地 zip 包供浏览器查看

## 效果图

![](img/064802e22c43e10fb37576f987829844.png)

![](img/07363164193bcb9c1402e5667e28b676.png)

![](img/af7b92ad72b48cf9259ee30762e15a12.png)

![](img/1135aee1edaa20c7a3eee2ac1c378924.png)

![](img/9de2360f94c92ae86ab0b47cec307d42.png)

# Cursor+Figma联动，实现可编辑产品原型图

## 步骤一：项目初始化

首先先创建文件夹，然后使用cursor打开

之后创建`mdc`文件：

![](img/5f4ccf8477e207b9cb75f0ae20bd9da8.png)

**内容如下：**

```
# 作用
能够按照用户的需求，生成APP UI图

# 步骤：
1.结合用户需求，以产品经理的视角去规划APP的功能、页面和交互；
2.结合产品规划，以设计师的视角去输出完整的UI/UX；

# 要求
以上全部页面都在同一个html文件中展示。
```

## 步骤二：html生成

首先右键`app-ui-generate.mdc`文件，

点击`Add files to cursor chat`将该rule添加到对话中

![](img/75605fca8720cab3df8328fa921fb5da.png)

**之后执行下面这些操作**

+ 使用`Agent`模式

+ 选择`claude-3.7.sonnet`算法

+ 并输入你要的需求，我这里让其生成一个心情记录APP UI

![](img/5e54bdbd1e6d628fea80cbe206ac55aa.png)

html生成完成后，可以使用浏览器在本地打开看看效果

![](img/6bd17fa41980692297293c890c21ea88.png)

## 步骤三：使用figma插件转html为原型图

### 创建设计稿

+ 点击`Design`

![](img/4c7acac17874f1e5c453746555662a3f.png)

有的电脑版本可能在这里，注意找到`Design`就可以了

![](img/34a3fe4fe43c94e57475e1f69fa99d9e.png)

### 搜索并引用插件

+ 点击插件

搜索`html.to.design`

![](img/6a9b164f671eebe4242f4eadce2db51e.png)

![](img/5f67909508fff577bf254e0688ae1de8.png)

点击下面的搜索的图标

点击`save` 进行保存

![](img/02dcad44c60bcd7df0b6776f1537dd44.png)

![](img/cc91d9fbb85904bd0b17cfcbab3d20d7.png)

点击该插件

![](img/3417afb4fa76964b972dee5b202cca86.png)

### 导入html

这里有很多种导入方式，包括使用链接导入，或者文件导入等，也有MCP的方式（不过没有去研究）

注意：有可能得🪜才能使用，所以这里得自己去想办法啦~

+ 点击`File`，并将生成的html拖进去

之后便等待生成，可能中间会遇到这个页面：

![](img/ae9b2ee11294cc71ad19a704cfad3e88.png)

![](img/9dc7f2f6a76926bd795417098ce5522d.png)

生成效果

![](img/398016b149aabe92ab9397bdde46c327.png)

可以对内容进行编辑。

# Cursor+Claude3.7生成原型图保姆级教程

## 第一步，整理提示词。

```
你是一名精通 UI 设计和产品规划的产品经理，拥有 20 年的 UX 设计经验。你的任务是帮助一位初中生用户完成一个“健身普拉提”iOS App 的开发。
现在你的目标是输出一套完整的APP原型图来辅助后续的开发任务，请遵循以下原则：
- 模拟真实用户使用普拉提类APP的真实场景和需求； 
- 结合用户需求，以产品经理的视角去规划APP的功能、页面和交互； 
- 结合产品规划，以设计师的视角去输出完整的UI/UX； 
- 引入tailwindcss来完成style样式，图片使用unsplash 
- 以上全部页面都在同一个html文件中展示。
```

## 第二步：使用 Cursor + Claude 3.7 Sonnet，在 `Ask` 模型中提问。

如果直接使用 `Edit` 模式进行提问，会发现代码上下文过长导致中断，改成 `Ask` 模式。

![](img/5643d9c974634e430e8d958407dce7e0.png)

![](img/11778ac8ee81a152c7d1dca83543e8b9.png)

![](img/f7e0dd2d8288facf9f8c492f1aabbe43.png)

## 第三步，在中断后，发送几轮“继续”之后，就可以生成多段 HTML 代码。将其拷贝到一个 HTML 文件即可。

整个生成 UI 原型图的过程非常的简单，同时审美也很在线。

在提示词中并没有加入太多功能描述的前提下，Claude 3.7也能自行丰富，如果在提示词中加入更多的规则来约束的话，能更好的满足部分复杂项目的诉求。

![](img/2b7356fd4cdeaba8c187aa75d172b852.png)
