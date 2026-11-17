# AI+知识管理体系 —— Obsidian入门及 AI 插件配置

> 来源：[https://v11enp9ok1h.feishu.cn/docx/XjA2dWukzo0ZRsxDDPvcV9P9nig](https://v11enp9ok1h.feishu.cn/docx/XjA2dWukzo0ZRsxDDPvcV9P9nig)

### 引言

**Obsidian** 是一款非常优秀的笔记工具，也是个人知识管理体系的有效助手：https://obsidian.md/

![](img/5f36f2308d00f097592bdfee8c867df3.png)

作为一款优秀的知识管理工具，obsidian 开放的插件生态有很多优秀的大模型插件和 RAG 工具，便于我们直接把笔记体系作为大模型的上下文来进行知识管理，大模型的引入对obsidian可以说是如虎添翼，非常建议 obsidian 用户用起来。

本文涉及以下几部分内容：（对obsidian非常熟悉的朋友可以直接跳到第五节）

1.  熟悉基础界面

1.  设置外观

1.  使用插件市场

1.  配置工作区

1.  嵌入 AI 辅助

1.  进阶用法：笔记仓库嵌入大模型（Copilot）

1.  笔记内容作为 RAG 嵌入大模型（Smart Conections）

1.  笔记内使用大模型编写内容 (Text Generator/Tars/Companion )

接下来会逐一截图标注及说明：

---

### 第一步：熟悉基础界面

（本文所有配图可点击放大查阅）

![](img/656bab7f31469983582052da3dd7dd37.png)

#### **全局界面：**

+   最左侧（粉色）是 Obsidian 的功能按钮，包含一些方便的快捷功能和核心插件，鼠标移动到图标上可以看到具体说明。

+   上方（黄色）是顶部功能按钮，包括查看笔记列表、搜索、书签、收起和展开侧边栏。这些功能按钮都可以拖动到你习惯的位置形成单独的设置窗口。

+   上方（绿色）是笔记功能按钮，包括新建笔记、新建文件夹、笔记排序设置、展开和收起全部文件夹。

+   左下方（紫色）点击切换不同的笔记库（库的概念我们后面会讲到）

+   左下方（蓝色）是 Obsidian 的全局设置入口，在这里可以设置 Obsidian 的很多核心功能。

+   中心区上方（红色）是当前笔记的阅读模式，可以切换为源码模式和阅览模式。

+   中心区上方（绿色）是当前笔记的设置菜单，可以对笔记进行各类操作。（如图1.2）

![](img/23a6d8b68c7d2d510fc0c4c991893f6d.png)

+   右上方（紫色）是关系图谱的设置入口（打开后如图1.3 关系图谱设置）

![](img/d0b8334efe0292dba84a3dede63bfed5.png)

以上就是刚安装好的Obsidian的基础界面样式，大家稍作熟悉即可。在接下来的内容中，我们会帮助大家理解Obsidian的核心功能和重要设置。

### 第二步：外观设置

#### 外观管理

![](img/502b1f590a351721166e5c0ee830b45f.png)

+   点击左下角设置（小齿轮）1️⃣

+   在弹出菜单中选择“外观” 2️⃣

+   右侧点击“管理”按钮进入外观设置 3️⃣

#### 选择主题

![](img/1ad60e2345768ac9018a7ee0dc815237.png)

#### 安装主题

![](img/bfbca6c04a39548b845943997a341493.png)

#### 字体管理

![](img/3017b4cbe79eb74d3c224b81b713740c.png)

+   点击左下角设置（小齿轮）1️⃣

+   在弹出菜单中选择“外观” 2️⃣

+   如图，选择你喜欢的字体到对应位置 3️⃣ 4️⃣ 5️⃣

注意：选择的字体必须和你系统中的字体名称准确对应，否则 Obsidian无法识别

![](img/08dfe3026d4082cafb76f857e9d373c6.png)

上图中的字体采用的是仓耳玄三的二号字和三号字，他们的字体名称叫“仓耳玄三”，如图2.6

![](img/92c6db0dfc2503d9d3c624e85e2c57c7.png)

但他们的字体文件在系统中的名称是：

TsangerXuanSan04-W03

TsangerXuanSan04-W02

如图2.7

![](img/f6c0f41238bef2dd4580205b8c26f6f6.png)

#### 字体设置

所以我们要在设置中填入字体的文件名称才能正确对应字体，如图2.8及2.9

![](img/da0597cb07ae20aa99fb51d8ce232b34.png)

![](img/1340d4c83d59d2ac97195e87a77a3420.png)

### 第三步：插件安装

#### 第三方插件市场


+   点击左下角设置（小齿轮）1️⃣

+   在弹出菜单中选择“第三方插件” 2️⃣

+   右侧点击“浏览”按钮进入第三方插件市场 3️⃣

#### 搜索插件名称

![](img/673f77a33cde2267c03bf855b609c92e.png)

+   搜索想要的插件名称

+   这里以安装自定义样式设置插件 “style settings”为例（推荐安装这一插件）

#### 安装插件

![](img/ee8eee4c9c02f66ce6a7dfd69487f320.png)

+   在插件市场搜索到的结果列表中选中所需插件 1️⃣

+   点击安装 2️⃣

#### 启用插件

![](img/8638b7fbcb01df946e82dab69482ce65.png)

+   软件右上角会提示插件安装完成

+   随后直接在原界面点击启用插件 1️⃣

#### 插件选项

![](img/6315cf8bc2b9f33d98c3f999fdeb6e42.png)

+   点击启用后，直接在原界面点击插件选项 1️⃣

#### 设置插件选项

![](img/aea4203a14bd805f66c784ca24c380a1.png)

+   在 style settings 插件设置中可以详细设定相关选项，大家可以根据喜好自行探索。

注意：萌新不建议深度设置太多不熟悉的选项，建议研究一下字体样式、文件名和标题即可。

### 第四步：配置工作区

#### 打开核心插件

![](img/8cf9795a990bef9fd1d8e733e1f97b67.png)

+   点击左下角设置（小齿轮）1️⃣

+   在弹出菜单中选择“核心插件” 2️⃣

+   右侧打开“工作区”的滑动按钮 3️⃣

#### 工作区入口

![](img/f16caa5f94202cf519865d4946b2dd90.png)

+   在左侧功能按钮区找到工作区布局按钮

+   点击打开

#### 工作区选项

![](img/a08c020a67470463b9412624f5f9e496.png)

#### 工作区划分

如图4.4，这是一个工作区划分示例（实际上要根据自己的笔记习惯自己配置）

![](img/955d25bd1dd8ea09ac92043eb3d90f62.png)

+   左上角（粉色）为笔记目录区

+   左侧下方（蓝色）为笔记大纲区

+   中心（绿色）为笔记内容区

+   右上（紫色）为日记或便签区

+   右下（深蓝）为工具配置区

> 注意：在 Obsidian 中可以通过：

1.  拖拽区域上方小按钮，移动到对应位置

1.  点击区域菜单栏（三个小点）选择划分分栏的方式

> 自由设置不同位置的面板，同学们可以自行尝试一下。

自由拖动工作区示例：

![](img/6f65d9d2ff9b40ea444d4477c0cc92a6.png)

[Export-1722756115709.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ClkDbFCyWokVwWxnOmvcaIdpnsd)

#### 切换不同工作区


#### 长文编写双排工作区示例


### 第五步：嵌入 AI 辅助工具

我们通过使用第三方插件：custom frames 来实现最简单的大模型嵌入笔记系统

#### 打开插件市场


#### 搜索 custom frames 插件


#### 安装插件

![](img/c211e2d9e889bdb0cd0573e7ea286153.png)

#### 启用插件

![](img/03de486ea667d05a6fd37915520fd455.png)

#### 配置插件

![](img/be8b17814990a73a974de17ece96b535.png)

#### 新增窗口

![](img/ae1699788c2f22aaf7e3b1d202115d3e.png)

#### 设置新增的窗口

![](img/aa7d9e39c91e64d76432be855b75d96d.png)

如图5.7.1，在这里点击 show settings 来配置我们想要新增的 KimiChat 窗口 1️⃣

![](img/bc2f1cdedba368efe3604519b8bd46e4.png)

如图 5.7.2

+   1️⃣ 在 Display name （显示名称）处填写想要显示的名称：Kimi

+   2️⃣ 在 URL（网址）处填写对应的完整网页地址（要带http://）：http://kimi.moonshot.cn

![](img/84a0b0c6e8cd71248256c0d0f31f66ac.png)

如图 5.7.3

+   在 Icon 选项中，点击 Lucide icons 链接，会在原处弹出浏览器窗口，在弹出的浏览器中进行操作 1️⃣

![](img/77b754e7dd9d3915ac8103e601de2ac5.png)

如图 5.7.4

+   在弹出的浏览器页面 Lucide icons 中点击搜索框 1️⃣

![](img/5e3d5f2186705d04606dc685f99eb46c.png)

如图 5.7.5

1.  搜索合适的图标（用英文搜索）1️⃣

1.  选中喜欢的图标 2️⃣

1.  在下方的复制按钮处点击，复制图标名称 3️⃣

![](img/adaae80c416b79a3c208ebf0ecf5b4a8.png)

如图 5.7.6

+   关闭浏览器  1️⃣

+   回到Obsidian填写图标名称 2️⃣

#### 重启Obsidian

配置完成后需要 完全退出Obsidian 再重新打开

#### 打开新面板

![](img/defe834908a3222749a4c0cd225a395c.png)

1.  用命令面板打开刚才创建的页面，默认快捷键 Ctrl+P 1️⃣

1.  在弹出的搜索框中输入插件名称：custom frames 2️⃣

1.  在下拉选项中选择 OpenKimi 3️⃣

1.  打开刚才设置好的Kimi窗口

#### 开始使用

![](img/175313f8718bb2f055d74efd9f9c716d.png)

以上是最简单的在obsidian中通过内置网页嵌入AI类型工具的方法。

接下来为大家介绍在Obsidian 中使用 AI 工具的进阶配置方法。

> Obsidian 与 AI 组合使用的几种思路：

1.  内嵌网页+ AI（Custom frame）

1.  笔记仓库嵌入大模型（Copilot）

1.  笔记内容作为 RAG 嵌入大模型（Smart Conections）

1.  笔记内使用大模型编写内容 (Tars/Text Generator )

其中，“内嵌网页+ AI（Custom frame）”已经在上一篇教程为大家分享过了，本篇讲一下2-4三类工具的配置方法：

## 进阶用法：Copilot for Obsidian

⭐️⭐️⭐️⭐️⭐️

### 简介

集成ObsidianUI，支持AI 笔记阅读，支持一键保存生成内容为 Markdown，支持长笔记阅读，支持移动端，支持本地库 QA 获得引用答复（需要本地大模型支持），支持几乎市面上所有主流大模型和本地大模型嵌入。功能完整，简单易用。

项目地址：https://github.com/logancyang/obsidian-copilot

本地大模型设置指南：https://github.com/logancyang/obsidian-copilot/blob/master/local_copilot.md

```
# Obsidian Copilot 使用指南

## 简介
Obsidian Copilot是一款强大的Obsidian插件,它为您的知识管理系统带来了人工智能助手的功能。这款插件完全免费和开源,设计简洁,易于使用。它能让您直接在Obsidian中与ChatGPT等大型语言模型进行对话,大大提升您的工作效率和创造力。

## 主要特点
1. 内置聊天界面:在Obsidian内部直接与AI助手对话,无需切换到其他应用。
2. 多种AI模型支持:可使用OpenAI, Anthropic, Azure OpenAI, Google Gemini等多家公司的AI模型。
3. 本地模型支持:可通过LM Studio和Ollama使用本地AI模型,保护隐私。
4. 笔记问答功能:可以针对您的笔记内容进行问答,AI会引用相关来源。
5. 自定义提示词:可以创建、编辑和应用您自己的AI指令模板。
6. 多种实用命令:如简化文本、添加表情、总结、翻译、改变语气等。
7. 灵活设置:可自定义AI参数如温度、最大令牌数等。
8. 隐私保护:使用本地向量存储,不会将您的数据发送到云端。

## 使用方法
### 安装
1. 在Obsidian中打开&quot;社区插件&quot;设置页面。
2. 点击&quot;浏览&quot;按钮,搜索&quot;Copilot&quot;。
3. 找到&quot;Copilot for Obsidian&quot;插件并安装。
4. 安装完成后,启用该插件。

### 基本设置
1. 在Obsidian设置中找到&quot;Copilot&quot;选项。
2. 选择您想使用的AI模型提供商(如OpenAI)。
3. 输入相应的API密钥。
4. 根据需要调整其他参数,如温度、最大token数等。
5. 点击&quot;保存并重新加载&quot;以应用设置。

### 开始使用
1. 点击Obsidian左侧边栏的聊天图标,打开Copilot聊天面板。
2. 在聊天框中输入您的问题或指令,按发送键与AI对话。
3. 使用模式选择下拉菜单切换不同功能模式:
   - Chat:普通聊天模式
   - QA:针对当前笔记的问答模式
   - Vault QA:针对整个知识库的问答模式

### 常用功能
1. 复制消息:点击消息旁的复制图标,将内容复制为Markdown格式。
2. 保存对话:点击&quot;Save as Note&quot;按钮,将整个对话保存为笔记。
3. 使用Copilot命令:
   - 在Obsidian命令面板中搜索&quot;Copilot&quot;
   - 选择所需命令,如&quot;Simplify&quot;、&quot;Summarize&quot;、&quot;Translate&quot;等
   - 命令将应用于当前选中的文本

4. 自定义提示词:
   - 在设置中添加、编辑或删除自定义提示词模板
   - 在聊天时使用这些模板快速生成特定类型的内容

5. 长文本问答:
   - 打开一篇长笔记
   - 切换到&quot;QA&quot;模式
   - 开始针对该笔记内容提问
   
6. 知识库问答:
   - 切换到&quot;Vault QA&quot;模式
   - 提出问题,AI将搜索整个知识库并给出带引用的回答

## 注意事项
- 聊天历史默认不保存,请使用&quot;Save as Note&quot;功能保存重要对话。
- 使用&quot;New Chat&quot;会清除当前所有聊天记录。
- 使用GPT-4等高级模型时,请注意API使用成本。
- 首次使用QA模式时,需要等待系统建立索引。
- 确保API密钥正确且有足够的使用额度。

## 结语
Obsidian Copilot为您的知识管理带来了AI的力量。通过与您的笔记互动,它可以帮助您更好地理解、组织和扩展您的知识。随着使用,您会发现越来越多令人兴奋的应用方式。祝您使用愉快,让AI成为您强大的学习和创作伙伴!
```

### 安装配置

#### 2.1 在插件市场搜索并安装 Copilot 插件

![](img/1aa35d70c199e6e00e443716ca3767c1.png)

#### 2.2 启用 Copilot 插件

![](img/4377f47f6d37dc4d3e781fa253bae854.png)

#### 2.3 配置插件

![](img/2e561b47a32ac62b636686d13d4c50d7.png)

> 所有设置后点击“Save and Reload”保存重载设置

![](img/010dd353afb8768546ab0524e38e3dbd.png)

#### 2.4 配置大模型 Key

##### 2.4.1 使用第三方 Key

![](img/a72052e354db1ab73fbfd37e874fcc01.png)

##### 2.4.2 使用官方大模型 Key

![](img/3051154bb0dbdfe21634e29074b1a530.png)

#### 2.5 大模型默认参数设置

![](img/ef2e71fb6af3c2562458571210ccf500.png)

#####

## 进阶用法：Smart Connection插件

⭐️⭐️⭐️⭐️⭐️

### 简介

自动给出笔记的关联性，根据库中的笔记内容进行对话，快速组织自己的笔记内容。功能和 Copilot 相似，但是更强大，需要一定的配置能力。

项目地址：https://github.com/brianpetro/obsidian-smart-connections

参考视频介绍：https://www.bilibili.com/video/BV1JD421G7gD/

### 安装配置

#### 在插件市场搜索并安装插件

![](img/59139d587dcfe596e20cb63ed605a2ae.png)

#### 启用插件

![](img/43c8b51941fc49a088c387824bafb348.png)

#### 配置插件

##### 2.3.1 配置 Embedding 模型

![](img/5db98ab5d3c3b636fdf87dcaed11a7c0.png)

##### 2.3.2 智能对话模型模型选择

![](img/a5d82a1fbe2abc2ac310bebdb51cd027.png)

#####

## 进阶用法：**Text Generator**

⭐️⭐️⭐️

### 简介

文本生成工具，通过使用指令沉浸式在笔记中生成内容，功能与 NotionAI类似，无单独 UI，支持的模型较多。

项目地址：https://github.com/nhaouari/obsidian-textgenerator-plugin

![](img/8da6da1b94d740ec91099024f86107e0.png)

### 安装配置

#### 在插件市场搜索并安装插件

![](img/acd7085b73fae48050725cfe25de2cad.png)

#### 启用插件

![](img/384265a100493e07392649521a767adf.png)

#### 配置插件

##### 大模型设置

![](img/5048c83e5fe330f8171742b1c21db4e3.png)

##### 行内建议文本生成设置

![](img/4f101eb6c7ecab26dbed8c8bfeff6f54.png)

## 进阶用法：**Companion**

⭐️⭐️

### 简介

根据写作上下文接收有关完成单词、短语甚至整个句子的建议，从而更快速、更轻松地书写笔记。轻便好用。支持的模型较多。

项目地址：https://github.com/rizerphe/obsidian-companion

![](img/73e2a9d877d85492121ce2cd76cd0bf5.png)

### 安装配置

#### 在插件市场搜索并安装

#### 启用插件

![](img/f91062a3b7323edc6009ead712669dc2.png)

#### 配置插件

![](img/374ea3c991063d536e21f70b6b9f9f98.png)

## **Tars**

⭐️⭐️⭐️

### 简介

Tars 是一个 Obsidian 插件，基于标签建议进行文本生成，支持 Claude、OpenAI、Kimi、豆包、阿里千问、智谱、深度求索、百度千帆等。

项目地址:https://github.com/TarsLab/obsidian-tars

![](img/6a52c1805fefb571e8d35fcfa184040a.png)

### 安装配置

#### 启用插件

![](img/7382051ee979ef0dee01ac6c3f8105e3.png)

#### 配置插件

![](img/bc5a384c835601dcd5b47d38aadf1cfd.png)