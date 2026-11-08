# Cursor 使用技巧｜持续更新

> 来源：[https://leonzhang.feishu.cn/docx/IAupdZZhfoSGrBxRPMzc7Dm2npq](https://leonzhang.feishu.cn/docx/IAupdZZhfoSGrBxRPMzc7Dm2npq)

### 技巧 & 思路

1.  **需求确认**

1.  新建项目 idea.md 文件，而非 notepad**（支持 M**arsCode 等插件引用），通过与 chat 对话，去确认、完善需求。

1.  后续可以完善，并作为模板使用。

1.  注意 ⚠️：后续封装代码时要考虑删除（毕竟需求都做完了，需要时再新建）。

1.  **需求分拆**

1.  基于需求分拆为可实现的小步骤，并新建 reference.md 文件。

1.  **目的是确保需求**清晰，AI 能够理解你的需求。Cursor 还没有强到分拆、执行这么多需求（上下文长度），几句话让 AI 实现和你想象中一模一样的东西，所以这里核心要确认 AI 理解的需求是否为自己想要的。

1.  **步骤执行**

1.  让 **Cursor**** 去编译项目**，并解决问题（减少从 Xcode 复制错误代码，再给 Cursor 修复的时间）。

1.  用 html、css、js 的方式生成静态页面。修改、完善需求。

1.  **写代码（要耐心）**

1.  写代码 - 改 bug - 调试 - 验证。

1.  **其他 Rules**

1.  如安装依赖时，不断报错。

1.  如果涉及有需要密码验证的环节，请及时打断并详细说明，我应该执行什么步骤。

### 持续学习如何更好与 AI 协作

#### 新建 .cursorrules   .cursorignore

```
请你帮我检索 codebase，并生成 .cursorrules 以及 .cursorignore 文件
```

```
请你在执行操作前，先复述一遍我的需求再进行操作，让我先确认你清楚我的需求

需求正确，请你一步一步帮我执行。
```

#### 需求确认

```
```
想法及要求
```

我想要这个 App 是一个简单的、可本地化运行的 App，请你作为产品专家，帮我完成需求的梳理，并将内容填充到文件 `idea.md` 中。
```

#### 需求分拆

```
现在需要输出高保真、可交互的原型图，并确保这些原型图可以直接用于开发。

1. 用户体验分析：请先分析这个 App 的主要功能与用户需求，确定核心交互逻辑。
2. 产品界面规划：作为产品经理，定义关键界面，确保信息架构合理。
3. 高保真 UI 设计：作为 UI 设计师，设计贴近真实 iOS 设计规范的页面，使用现代化的 UI 元素，使其具有良好的视觉体验。
4. HTML 原型实现：使用 HTML + Tailwind CSS（或 Bootsrap）生成所有原型界面，并使用 FontAwesome（或其他开源 UI 组件）让界面更加精明，解决真实的 App 设计，拆分代码文件，保持结构清晰。
5. 每个界面应作为独立的 HTML 文件存放，比如 home.html, Profile.html, settings.html 等。
    - index.html 作为主入口，不直接写入所有页面的 html 代码，而是使用 iframe 的方式嵌入这些 HTML 片段，并将所有页面直接平铺展示在 index.html 页面中，而不是跳转链接。
    - 真实感增强，界面尺寸应模拟 iPhone 16 Pro Max，并让界面圆角化，使其更像真实的手机页面。
    - 使用真实的 UI 图片，而非占位图片，可以用 Unsplash、Pexels、Apple 官方 UI 资源中选择。
    - 添加顶部状态栏（模拟 iOS 状态栏），并包含 App 导航栏（类似 iOS 底部 Tab Bar）。
    
请按照以上要求生成完整的 HTML 代码，并确保其可用于实际开发。
```

此时，可以随时截图 index.html 页面的原型图使用。

#### **步骤执行**

```
请你参考文件 `prd.md` 中的需求和原型图，帮我完成这个 iOS App 的开发。

当前位置在这个 iOS 开发项目的根目录中，已有文件不需要你重复创建，直接编辑就好，但你可以创建本地没有且你需要的文件。
```

####  Bug 修复

新建文件 bug.md ，将 Xcode 的 bug 复制进去，  让 AI 处理。

#### 使用技巧

> 清晰有逻辑的表达需求、让 AI 复述需求、发送示例参考、发送自己的解决思路给 AI（可选）

1.  Toglle AI  Pane 技巧

1.  **检测**：Save all** 支持先保存并测试，**确认后**再选择 Reject** all 或 Accept all

1.  **回滚**： Checkpoint created。 restore ，点击 restore

1.  **预防**：

1.  让 AI 复述需求指令。请你先复述一遍我的需求再进行答复，让我确认你清楚我的需求｜延伸：可以试试将其更新到 .cursorrules 文件中。

1.   **让这个需求指令足够小，****足够单一化，**不要让 AI 猜你的想法。要有足够的针对性，并且要明确范围，最大程度缩小辐射范围。在修改内容时，要尽可能 @相关文件，并且告诉 AI 要在什么范围内修改。

1.  **需求拆解：**多需求，使用有序/无序列表的方式描述。

1.  把需求拆解为多个功能 feature

1.  **把 AI 当成小孩子**：尽可能逻辑清晰，描述问题要加以引导。

1.  必要时，发送示例参考、发送自己的解决思路给 AI（可选）。

1.  **新项目的准备 步骤**

1.  让 **composer **帮你检索 codebase，然后生成 .cursorrules  文件。可手动添加想要索引忽略掉的文件。

1.  打开「设置-features-Codebase indexing」，点击「Resvnc Index」重新索引。

1.  将文档录入 Docs 中备用，如「需求文档、接口文档、开发相关的技术文档」等。

1.  在项目根目录中，新建.cursorrules 文件，作为 Prompt 前置词。

1.  在 chat 中，@相关文档（项目文档、需求文档）让 AI 检索整个项目，并生成一个 .cursorrules 文件，并适当修改。

1.  比如「请你先复述一遍我的需求再进行答复，让我确认你清楚我的需求」。

1.  如果是版本差异间的 bug，可以 @git ，然后让 chat 检索差异，精准获得 bug 产生原因以及解决思路。

1.  简单 bug 直接让 composer 修复。

1.  复杂 bug 则需要新建 notepads 文件，详细阐述 bug 产生可能性及解决方案。然后在 composer 中 @notepad 让 AI 修复。

1.  项目结束后，可以将 notepad 文件封存，下次就可以让 AI 快速理解这个项目。一定要及时归纳，因为 notepad 是存放在 Cursor 的缓存中，换设备后就丢失了。吧

####  常用文件创建

1.  readme.md  代码简介及代码修改说明。

1.  notepads    与 **composer** 的互通桥梁。

1.  feature.md  功能说明，过程需不断补充。

1.  error.md  错误说明，实时更新。先与 **chat**** 沟通解决方案，**并更新解决方案在当前文件，让 composer 去解决。

1.  .cursorignore  将包含常见的需要忽略的文件和目录。

1.  .cursorrules  只针对这个项目的规则，需要 AI 工作时参考的 Rules。

1.  包括但不限于：前置、项目简介、技术架构、目录结构、代码规范、命名规范、组件规范、样式规范、国际化规范

### IDE 使用技巧

**1**

Xcode 新建项目完毕后，要在 Cursor 中「暂存更改」，方便回溯到刚创建时的文件及文件夹结构。

**2**

在源代码管理工具中，后面的 "U" 通常表示文件的状态。具体来说：

U：表示文件是“未跟踪”（Untracked）。这意味着该文件存在于工作目录中，但尚未被 Git 纳入版本控制。

文件状态的常见标识：

+   U：未跟踪文件

+   M：已修改文件（Modified）

+   A：已添加文件（Added）

+   D：已删除文件（Deleted）

+   R：已重命名文件（Renamed）

### App Store 上架技巧

####  Xcode

1.  删掉 iPad 版本。

1.  最低 iOS 运行版本为 iOS 16。注意，此时要再次编译下，确认是否有问题。

1.  Display Name 即为 App Store 显示名称。

1.  Version 为 1.0.0

1.  创建图标。

1.  可以让 Cursor 创建一个 1024*1024 的 svg 图片作为应用图标，比新建文件 icon.svg 来存储内容。

1.  然后将 icon 导入 Xcode 中的 Assets 中。

1.  最后，在 Xcode 中的打包项目，路径：Product-Archive

1.  Distribute App - App Store Connect - Distribute

1.  Web 页面「App Store Connect」

1.  发布文案可以通过 Cursor 来写。

1.

1.  隐私协议、技术支持文档，可以放在飞书云文档中。

```
我要把 App 发布上架到 App Store，请帮我完成上架信息的填写，包括有
1. 推广文本、描述、关键词
2. 技术支持说明、隐私协议说明
3. 主标题、副标题
```

### 扩展内容

1.  【【Cursor 最直观教程】手把手教小白如何 8 分钟用 Cursor 完成 AI 🛰️小程序的开发上线】 https://www.bilibili.com/video/BV1oF4TegEne/

1.  【我用 Cursor 做了个 Cursor 教程博客网站】 https://www.bilibili.com/video/BV1ia2BYbER7/

1.  【普通人也可以看的 AI 编程指南 | Cursor 教程｜Cursor 使用技巧和思路｜如何免费使用 Cursor｜AI 编程】 https://www.bilibili.com/video/BV1yorUYWEGD/

1.  《LookAI - Cursor AI 编程教程及免费学习资源》: https://www.lookai.top/cn/cursor/instruction/instruction

1.  《Prompting Advice - Devin Docs》: https://docs.devin.ai/learn-about-devin/prompting

1.  【不写代码，30 分钟带你完成 iOS APP 开发全流程】 https://www.bilibili.com/video/BV1K1XNYAEd3/

1.