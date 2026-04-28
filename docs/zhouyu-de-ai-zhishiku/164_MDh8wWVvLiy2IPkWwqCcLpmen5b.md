# AI coding作业

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/Aw4rdUDjVomYs5xUKQbcpwfGnde](https://xatc0v8uz5m.feishu.cn/docx/Aw4rdUDjVomYs5xUKQbcpwfGnde)

### 项目核心文档 (MVP Core Docs)

这份文档整合了PRD、TRD等的核心要素，简洁明了。

#### 产品与市场方向 (MRD/PRD)

+   **产品名称**: Web Piano MVP

+   **目标用户**: 对音乐感兴趣，希望在网页上快速体验弹琴乐趣的普通用户。

+   核心用户故事 (User Story):

+   **作为用户**，我希望在浏览器中看到一个虚拟钢琴键盘。

+   **作为用户**，当我按下电脑键盘上的指定按键时（如'q', 'w', 'e'等），我希望听到对应音高的声音。

+   **作为用户**，当我按下按键时，我希望能看到虚拟钢琴上对应的琴键有“被按下”的视觉反馈。

#### 技术与设计方案 (TRD/DRD)

+   **技术架构**: 纯前端，无后端依赖。

+   **语言**: HTML5, CSS3, JavaScript (ES6+)。

+   **框架**: 无。使用原生Web API。

+   **音频**: 使用浏览器内置的 **Web Audio API** 的 OscillatorNode 实时合成声音。

+   **部署**: 可托管在任何静态网站平台（如GitHub Pages, Netlify, Vercel）。

+   **设计要求 (MVP版)**:

+   **布局**: 必须严格按照 virtualpiano.net 的按键映射关系。

+   **样式**: **功能优先**。仅需实现基础的黑白键区分，以及一个简单的“按下”状态（例如改变背景色或边框），无需追求高级质感、阴影和渐变。

#### 核心功能流程图 (Flowchart)

```
(用户) --- 按下键盘按键 (e.g., &#x27;w&#x27;) ---&gt; [程序: keydown事件触发]
                                            |
                                            v
                               [1. 查询 &#x27;w&#x27; 是否在按键映射表中?]
                                            |
                               (是) --------v-------- (否)
                                            |           |
                                            v           v
                          [2a. 播放对应频率的声音]      [结束]
                                            |
                                            v
                          [2b. 对应琴键div添加 &#x27;pressed&#x27; class]
                                            |
                                            v
(用户) --- 松开键盘按键 (e.g., &#x27;w&#x27;) ---&gt; [程序: keyup事件触发]
                                            |
                                            v
                               [1. 查询 &#x27;w&#x27; 是否在按键映射表中?]
                                            |
                               (是) --------v-------- (否)
                                            |           |
                                            v           v
                           [2a. 停止播放对应的声音]       [结束]
                                            |
                                            v
                           [2b. 对应琴键div移除 &#x27;pressed&#x27; class]
```

---

### 交付物二：AI Coding全套提示词 (Prompts)

请按顺序，一步步将以下提示词喂给您的AI编程助手。

#### **Prompt 1: 项目初始化**

> 请为我创建一个Web虚拟钢琴项目。首先，创建三个文件：index.html, style.css, script.js。 在 index.html 中，完成以下任务：

1.  设置基本的HTML5文档结构。

1.  在<head>中链接style.css和script.js (使用 defer 属性)。

1.  在<body>中，创建一个<div id="piano-container"></div>作为钢琴的容器。除此之外，不需要任何其他内容。

#### Prompt 2: 核心数据与HTML动态生成

> 现在我们来处理 script.js。

1.  创建一个名为 PIANO_KEYS 的常量数组。这个数组包含了所有琴键的数据。每个元素都是一个对象，必须包含 keyboardKey, type ('white' 或 'black'), note, 和 frequency 四个属性。请根据 virtualpiano.net 的键盘布局（从'1'/'!'到'm'），为我完整地生成这个包含所有琴键的数据。

1.  编写一个名为 generatePianoKeys 的函数。这个函数应该： a. 获取 piano-container 元素。 b. 遍历 PIANO_KEYS 数组。 c. 为数组中的每个对象，创建一个 div 元素。 d. 给这个 div 添加 key 和 white/black 两个类。 e. 给这个 div 设置一个 data-key 属性，值为 keyboardKey。 f. 将这个 div 添加到 piano-container 中。

1.  在脚本的最后，调用 generatePianoKeys() 函数。

#### Prompt 3: 基础功能性CSS样式

> 接下来是 style.css 文件。请为钢琴添加基础的、功能性的样式，不需要精美的细节。

1.  给 #piano-container 设置 display: flex，并给一个深色背景。

1.  为 .key.white 设置一个固定的宽度、高度、白色背景和黑色边框。

1.  为 .key.black 设置一个更小的宽度、高度、黑色背景和白色边框。使用 position: absolute 和 z-index: 2 让它能浮在白键上。你只需要提供一个大概的定位逻辑，不需要像素级精确。

1.  创建一个简单的 .key.pressed 类，它只需要改变一下背景颜色或者边框即可，用来提供视觉反馈。

#### Prompt 4: 实现完整的JavaScript交互逻辑

> 回到 script.js。现在我们要让钢琴“活”起来。请在 generatePianoKeys 函数的下方，添加完整的交互逻辑：

1.  初始化 AudioContext 和一个 activeOscillators Map，用于管理音频。

1.  创建一个 playNote(keyData) 函数。这个函数接收 PIANO_KEYS 中的一个对象作为参数，然后使用 Web Audio API 创建并播放一个对应频率的 OscillatorNode。将创建的 oscillator 存入 activeOscillators Map 中。

1.  创建一个 stopNote(keyData) 函数。这个函数从 activeOscillators Map 中找到对应的 oscillator，并调用 .stop() 方法立即停止它，然后从 Map 中删除。

1.  添加 keydown 事件监听器： a. 当键盘按下时，根据 event.key 找到 PIANO_KEYS 中的对应数据。 b. 找到页面上对应的琴键 div 元素，并给它添加 .pressed 类。 c. 调用 playNote() 函数。 d. 要防止按键长按时重复触发 playNote。

1.  添加 keyup 事件监听器： a. 当键盘松开时，找到对应的数据和div元素。 b. 移除 .pressed 类。 c. 调用 stopNote() 函数。