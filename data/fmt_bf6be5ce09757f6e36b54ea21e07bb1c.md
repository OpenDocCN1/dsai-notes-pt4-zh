
# GPT-5 模型介绍

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Ie85dALNXoJCrHxy2BsceTFfn8e](https://kq4b3vgg5b.feishu.cn/docx/Ie85dALNXoJCrHxy2BsceTFfn8e)

![](img/569f1e6fdfedbcc9b492fa0423bc1884.png)

> 这是OpenAI迄今为止最智能、、最快、最实用的模型，具有内置思维，可将专家级智能交到每个人手中。

当地时间2025年8月7日上午10点，OpenAI推出了GPT-5。OpenAI认为这是迄今为止最优秀的人工智能系统。GPT-5 的智能性能远超OpenAI之前的所有模型，在编码、数学、写作、健康、视觉感知等领域均拥有卓越的性能。它是一个统一的系统，能够识别何时快速响应，何时需要更长时间的思考以提供专家级的响应。GPT-5 面向所有用户开放，Plus 会员可获得更多使用量，Pro 会员则可访问 GPT-5 Pro 版本，该版本具有扩展推理能力，可提供更全面、更准确的答案。

## 一个统一的系统

GPT-5 是一个统一的系统，它包含一个能够解答大多数问题的智能高效模型、一个能够解决更复杂问题的更深层次的推理模型（GPT-5 思维），以及一个实时路由器，可以根据对话类型、复杂度、工具需求以及你的明确意图（例如，如果你在提示中说“认真思考一下”）。路由器会根据真实信号持续训练，包括用户切换模型的时间、响应偏好率以及测量的正确率，并随着时间的推移不断改进。一旦达到使用限制，每个模型的mini版本将处理剩余的查询。在不久的将来，OpenAI计划将这些功能集成到一个模型中。

## 更智能、更广泛适用的模型

GPT-5 不仅在基准测试中超越了之前的模型，回答问题的速度也更快，而且最重要的是，它对现实世界的查询更加有用。在减少幻觉、提高指令执行能力和减少谄媚方面取得了显著进展，同时提升了 GPT-5 在 ChatGPT 最常见的三个用途（写作、编程和健康）中的表现。

### 编程Coding

OpenAI 认为，GPT-5 是 OpenAI 迄今为止最强大的编码模型。它在复杂的前端生成和大型代码库的调试方面表现出色。它通常只需一次提示就能创建美观且响应迅速的网站、应用程序和游戏，并兼具美感，直观而优雅地将创意转化为现实。早期测试人员也注意到了它的设计选择，对间距、排版和留白等方面有了更深入的理解。

#### GPT-5编程示例&prompt

##### （1）滚球小游戏prompt

![](img/3e5c7ff86e86d6a12dc20d197dfcdb88.png)

**Prompt: **Create a single-page app in a single HTML file with the following requirements:

- Name: Jumping Ball Runner
- Goal: Jump over obstacles to survive as long as possible.
- Features: Increasing speed, high score tracking, retry button, and funny sounds for actions and events.
- The UI should be colorful, with parallax scrolling backgrounds.
- The characters should look cartoonish and be fun to watch.
- The game should be enjoyable for everyone.

**提示词：** 在单个 HTML 文件中创建单页应用程序，要求如下：

- 名称：跳球跑者
- 目标：跳过障碍物，尽可能长时间地生存。
- 特点：提高速度、跟踪高分、重试按钮以及动作和事件的有趣声音。
- 用户界面应色彩丰富，并具有视差滚动背景。
- 角色应该具有卡通风格并且有趣。
- 每个人都应该享受游戏的乐趣。

##### （2）像素艺术prompt

![](img/2498e9e8f90bd2a820c8b226e21d6e43.png)

**Prompt: **Create a single-page app, in a single HTML file, that provides a retro pixel painting experience.

- Canvas: fixed pixel grid with zoom; tools for pencil, eraser, fill, line, rectangle, circle; grid toggle.
- Palette: 16-color swatches with two custom slots; eyedropper; foreground/background swap.
- Editing: undo/redo, copy/paste selection, flip/rotate selection, clear canvas; status bar with cursor coords.
- UI shell: faux OS window (’90s style) with draggable title bar, toolbar icons, tooltip hints.
- Import/Export: import PNG (quantize to palette) and export PNG/SpriteSheet + JSON; save/load from `localStorage`.
- Shortcuts: number keys for tools, +/- for zoom; accessible labels and focus order.
- Responsive layout; no uploads to servers.

**提示词：** 在单个 HTML 文件中创建一个单页应用程序，提供复古像素绘画体验。

- 画布：可缩放的固定像素网格；铅笔、橡皮擦、填充、线条、矩形、圆形工具；网格切换。
- 调色板：16 色样本，带有两个自定义插槽；吸管；前景/背景交换。
- 编辑：撤消/重做、复制/粘贴选择、翻转/旋转选择、清除画布；带有光标坐标的状态栏。
- UI 外壳：仿 OS 窗口（90 年代风格），带有可拖动的标题栏、工具栏图标、工具提示。
- 导入/导出：导入 PNG（量化为调色板）并导出 PNG/SpriteSheet + JSON；从 `localStorage` 保存/加载。
- 快捷方式：工具的数字键，缩放的+/-；可访问的标签和焦点顺序。
- 响应式布局；无需上传到服务器。

##### （3）打字游戏prompt

![](img/3ef090f028ac6ccbc1a4194e6f6d36b6.png)

**Prompt: **Create a single-page app in a single HTML file with the following requirements:

- Name: Typing Speed Race
- Goal: Test WPM and accuracy in a timed typing challenge.
- Features: Random paragraph generator, error highlighting, live WPM display, countdown animation, history chart.
- The UI should be clean, with high-contrast text and a large typing area.

**提示词：** 在单个 HTML 文件中创建单页应用程序，要求如下：

- 名称：打字速度竞赛
- 目标：在定时打字挑战中测试 WPM 和准确性。
- 功能：随机段落生成器、错误突出显示、实时 WPM 显示、倒计时动画、历史图表。
- 用户界面应该简洁，具有高对比度的文本和较大的打字区域。

##### （4）鼓模拟器prompt

![](img/e88804b85f812fb361d28967afb23907.png)

**Prompt: **Create a single-page app in a single HTML file with the following requirements:

- Name: Virtual Drum Kit
- Goal: Play a drum kit using keyboard or clicks.
- Features: Multiple drum sounds, record and playback mode.
- The UI should be music-studio themed, polished, modern. Make it as beautiful as possible.

**提示词：** 在单个 HTML 文件中创建单页应用程序，要求如下：

- 名称：虚拟鼓组
- 目标：使用键盘或点击演奏一组鼓。
- 特点：多种鼓声、录音和播放模式。
- 用户界面应以音乐工作室为主题，精致、现代。尽可能美观。

##### （5）可视化工具prompt

![](img/44c23f5af5350b585414f91d3686e9fc.png)

**Prompt:** Generate `React` + `Canvas` “Lo-Fi Visualiser” that animates bars and waves to a vaporwave track (no file uploads, use a bundled tone); provide 3 visual styles (bars, dots, grid) and sliders for speed, density, and glow; surround with a Windows-’96 chrome (reminiscent of the file explorer interface), pixel buttons, and a hue wheel to recolour the scene.

**提示词：** 生成一个 `React` + `Canvas`“Lo-Fi Visualiser”，将条形和波浪动画化为蒸汽波轨道（无需上传文件，使用捆绑音调）；提供 3 种视觉样式（条形、点、网格）和滑块，用于控制速度、密度和辉光；周围环绕着 Windows-'96 镀铬（让人联想到文件资源管理器界面）、像素按钮和色相轮，为场景重新着色。

![](img/792d29b4bef2b34fe345dbda96beeafc.png)

### 创造性表达和写作

GPT-5 是OpenAI迄今为止最强大的写作协作工具，能够帮助用户将粗略的想法转化为引人入胜、富有文学深度和节奏感的文字作品 。它能够更可靠地处理结构模糊的写作，例如持续不押韵的抑扬格五音步或流畅自然的自由诗，将对形式的尊重与清晰的表达相结合。这些改进的写作功能意味着 ChatGPT 能够更好地帮助用户完成日常任务，例如起草和编辑报告、电子邮件、备忘录等。

### 健康

[OpenAI认为，GPT-5](https://openai.com/index/healthbench/) 是迄今为止针对健康相关问题的最佳模型，它使用户能够了解并倡导自己的健康。该模型在 HealthBench 上的得分明显高于任何先前的模型。HealthBench 是OpenAI今年早些时候基于现实场景和医生定义的标准发布的评估。与之前的模型相比，它更像是一个积极的思想伙伴，主动标记潜在顾虑并提出问题以提供更有用的答案。该模型现在还可以提供更精确、更可靠的响应，适应用户的背景、知识水平和地理位置，使其能够在各种场景中提供更安全、更有用的响应。

## 模型评测

GPT-5 的全面智能化程度显著提升，这体现在它在学术和人工评估基准测试中的表现上，尤其是在数学、编码、视觉感知和健康领域。它在数学（AIME 2025 无需工具测试得分 94.6%）、真实世界编码（SWE-bench Verified 得分 74.9%，Aider Polyglot 得分 88%）、多模态理解（MMMU 得分 84.2%）和健康（HealthBench Hard 得分 46.2%）方面均创下了新的最高水平 ——这些提升在日常使用中得到了充分体现。凭借 GPT-5 Pro 的扩展推理能力，该模型还在 GPQA 上创下了新的最高水平，无需工具测试得分高达 88.4%。

### 数学能力

![](img/2b28924a1ab2ca07efb841c8c613b377.png)

![](img/91bd10837523850c6aaa845b73f14db5.png)

![](img/4319508304011ae0580fec8cbb66dee8.png)

![](img/f777478cc12d4bc36ab6199e37914775.png)

![](img/52a178cc8d8ff0a60bc57d0fb760b39b.png)

### 编程能力Coding

![](img/f0f759c7741e9f1c42b699920141553f.png)

### 遵循指令和代理工具使用

![](img/906e4020d8f29d87e4ba245e940bb374.png)

![](img/df2752827e037c267fea4be28e09de2f.png)

### 多模态能力

该模型在一系列多模态基准测试中表现出色，涵盖视觉推理、视频推理、空间推理和科学推理。更强大的多模态性能意味着 ChatGPT 能够更准确地对图像和其他非文本输入进行推理——无论是解读图表、总结演示文稿照片，还是回答有关图表的问题。

![](img/3af7f322b6ad028a7f16b690995301f3.png)

![](img/bdc74d2de3e101e400f4fe641cbfd669.png)

### 健康

![](img/71f4a92d22e13154b42e8ecfb45c2267.png)

![](img/490ede59977f22b2c0671c87806f3bbb.png)

### 经济价值

GPT-5 也是OpenAI内部基准测试中表现最佳的模型，该基准测试衡量了模型在复杂且具有经济价值的知识工作中的表现。在推理方面，GPT-5 在大约一半的案例中与专家相当或更胜一筹，同时在涵盖法律、物流、销售和工程等 40 多个职业的任务中，其表现优于 o3 和 ChatGPT Agent。

![](img/d5afd7e6465de0b0d15d048bf93a97fb.png)

> 上述评估方法：GPT-4o 的结果反映了截至 2025 年 8 月 ChatGPT 中该模型的最新版本。所有模型均在高“推理努力”设置下进行评估。在 ChatGPT 中，推理努力程度可能有所不同，“高”表示用户使用该模型时可能体验到的上限。

## 更快、更高效的思考

GPT-5 用更少的思考时间实现了更大的价值。在评估中，GPT-5（具备思考能力）的表现优于 OpenAI o3，在视觉推理、代理编码和研究生水平的科学问题解决等功能上，输出 token 数量减少了 50% 至 80%。

![](img/80468e7b96c9e27d3f2987ae019d1aa5.png)

![](img/b43015253c38b89085a3f518918f1504.png)

![](img/20565a1266f02cc3425d41e60cce5084.png)

GPT-5 在 Microsoft Azure AI 超级计算机上进行了训练。

## 构建更强大、更可靠、更有用的模型

### 减少模型幻觉

GPT-5 产生幻觉的可能性显著低于OpenAI之前的模型。在 ChatGPT 生产流量中代表匿名提示的网页搜索中，GPT-5 的响应包含事实错误的可能性比 GPT-4o 低约 45%；在思考时，GPT-5 的响应包含事实错误的可能性比 OpenAI o3 低约 80%。

OpenAI表示在提高模型在推理复杂开放式问题时的可靠性方面投入了特别多精力。比如，添加了新的评估指标，以对开放式事实性进行压力测试。测量了 GPT-5 在思考开放式事实探究提示时的幻觉率，这些幻觉来自两个公开的事实性基准：LongFact（概念和对象）和 FActScore。在所有这些基准中，“GPT-5 思维”的幻觉率均显著下降——大约比 o3 少六倍——这标志着在生成一致准确的长篇内容方面取得了显著的飞跃

![](img/ef0ef3df153ee3a4901bc5c569cf9049.png)

![](img/ebe2f651717fb7b13f7d6bf4177231ed.png)

### 更诚实的回应

除了事实性增强之外，GPT-5（具备思考能力）还能更诚实地向用户传达其操作和能力——尤其是在处理那些不可能完成、具体要求不明确或缺少关键工具的任务时。为了在训练中获得高额奖励，推理模型可能会学会谎报自己成功完成了任务，或者对不确定的答案过于自信。例如，为了测试这一点，从多模态基准测试 CharXiv 的提示中删除了所有图像，结果发现 OpenAI o3 仍然能够以 86.7% 的概率对不存在的图像给出自信的答案，而 GPT-5 的这一比例仅为 9%。
