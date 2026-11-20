
# AI 从来没有真正「听过」你说话

> 来源：[https://chatmemo.feishu.cn/docx/YVi4dOqRqo8DK1xMHHpcVTv5n9g](https://chatmemo.feishu.cn/docx/YVi4dOqRqo8DK1xMHHpcVTv5n9g)

阿里千问团队发布了 Qwen3.5-Omni 全模态模型。

我认为从 AI 产品设计的角度，它值得更高的讨论热度。

近几个月，大模型领域的注意力大多集中在 Vibe Coding 与 Agent 上。但全模态模型线也不断加速，3 月以来，小米发 MiMo-V2-Omni，美团发 LongCat-Next。

而作为国内 Omni 模型的主要开源贡献者，千问也在前两天发布了最新的 Qwen3.5-Omni。

![](img/8ff8c8144e9d0d9201ad30fb726adf0b.png)

---

## 聊全模态，先从 AI 语音通话聊起

不知道你是否还记得 ChatGPT 高级语音模式？

彼时，GPT 凭借随时可打断，真人音效的优势——挂着语音，开车聊、走路问，成了很多人日常使用 AI 的自然方式。（国内豆包也把语音通话做成了核心体验）

![](img/be51cea99afc9196b5223a84c2c2ae59.png)

但体验不是一开始就如此自然。OpenAI 在发布 GPT-4o 时的原话是：

+ 在 4o 之前，语音模式需要通过「你说话 → 语音识别转成文字 → 模型理解文字并生成回复 → 文字转语音念给你听」的旧管线
+ 作为主要智能源的 AI，在旧管线中，并不能直接观察语气、说话者人数，也不能输出情感。

![](img/3063199ab0a1110613b1c51a1cfafb26.png)

这也是大多数 AI 产品的语音交互的旧方案：

![](img/527b07db3832a7622522b506032f28aa.png)

不难发现，管线的每一层都可能损耗信息：

+ **语音识别（ASR）**：语气、情绪、停顿节奏、声调变化、环境音、说话人特征全部丢失。你阴阳怪气说了一句话，转成文字后意思变得很平和，模型完全感知不到你的语气
+ **级联错误**：ASR 转录错了，后面所有环节跟着错。一个识别失误逐级放大，没有纠错机会
+ **语音合成（TTS）**：模型只能输出文字，不能输出语气。你所听到的语调，只是 TTS 从字面推断出来的语气

一个测试，用不同的语气、节奏，说「哎，好的，没问题」，拿同系列的 Qwen3.5 的非全模态模型与 Omni-Plus 全模态模型做直观的对比：

![](img/56f07e61a8af4bb66e5c34e57a20664f.png)

[`TX02_MIC007_20260401_082306_orig.wav`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GIrVbrOCDo0dm7xZgaDczzp4nNg)

![](img/6641748a17f9d6d6348887ed58498867.png)

![](img/dd097947da2209df4f2cf061c63eeb07.png)

前者无法辨别语气，后者则能捕捉到说话人的心情与含义。

非全模态方案，声音抵达模型智能前，就损失了大量信息，这就是「模型从来没有听过你说话」。

---

## 全模态加速人 AI 交互的效率

语音通话的信息损耗，只是人 AI 交互的一种问题。

现实任务中，AI 还要处理图像、视频、音频的理解 or 输出。

**每次中间环节转换，不仅**丢失信息，还会大大降低人机交互的效率与准确性。

（比如部分模型依赖 OCR 识别照片内容后，才能交给 AI 推理，但无法理解照片内的文字笔锋、颜色深浅）

全模态模型要做的，是让模型**直接听音频、看图像、看视频，并通过端到端**输出语音、图像等模态，减少中间转换的损耗。（多模态在行业语境下，更侧重多模态理解，尤其指文本、图像理解）

例如，在 Qwen3.5-Omni 中，任何用户都可以对着 Excel 数据表，口述数据分析要求：

![](img/c1d9ed9267b160e9dbcec886cc5b4cb5.png)

[`电商分析.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/F5J9bdGWGoowK9x4a20ckfU9nfd)

视频演示：对着数据表口述数据分析要求

AI 自动理解整个视频内的要求，进行指标计算：

![](img/026bd2756202d8e5aa77a73b10058eb5.png)

并将结果保存到了新的 Sheet 中。

![](img/84c9c9b1ff3338ea6e3feccdd4c0fde9.png)

模型能端到端处理的模态越完善，AI 就越理解人的需求。人与 AI 的交互方式也越接近「让真实人类做事」的方式。

---

## Qwen3.5-Omni，你所需知道的一切

话说回来，还是先速通 Qwen3.5-Omni 的发布信息。

Qwen3.5-Omni 是千问最新一代的全模态大模型，支持以下模态的端到端处理：

![](img/0080c884b4ac129b32c55c0335fa2919.png)

+ 输入：文本、图片、音频、视频理解（视觉能力与 Qwen3.5-Plus 对齐）
+ 输出：文本、音频

在模型规格方面：

+ **三种尺寸**：Plus、Flash、Light（线上暂无 light 版）
+ **API 版本：**分为 Offline、Realtime 两个版本。离线版支持长音视频文件处理，实时版可进行实时音视频通话
+ **架构**：Thinker-Talker 架构，均采用 Hybrid-Attention MoE
+ **上下文**：支持 256k 长上下文

核心能力提升明显，主打：

+ **长音视频处理：**目前在线 API 支持 3 小时音频、1 小时视频处理
+ **语音输出：**支持音色克隆，端到端情绪、音量、语速控制
+ **多语种：**113 种语言和方言语音识别，36 种语音生成
+ **语义打断：**区分「真实插话」与「附和 / 背景噪音」，不会被"嗯"一声或咳嗽误触发
+ **实时工具调用：**实时通话时，支持联网搜索 + `Function Call`，模型自判断是否触发工具

> 注：语种、部分参数 Plus 与 Flash 有一定差异

官方也给出了新模型与 Gemini 3.1 Pro 等海外顶级模型在音视频、图像理解、语音生成的榜单情况，得分靠前，详情见 👉 「全模态Qwen3.5-Omni上线」

---

## 全模态，为什么值得更高的讨论度？

Omni 类模型，虽然还没大规模应用于 AI 产品，但真的挺好玩的。

我找了些使用案例与你分享，在这个过程中，可以多多关注对 AI 用法、产品设计的一些启发。

---

### 音视频实时通话：AI 产品的 Vibe 交互方式

AI时代的产品现在被分为 GUI 和 LUI 的两种模式。

GUI 依赖人学会使用按钮，框选与点击。

![](img/348fb59b48ddcb9ad361a50ee148d114.png)

LUI 需要人用文本，相对准确地描述需求，等待 AI 回应。

![](img/9e1f93c9604d30eb6d761bf2c12934f9.png)

那如果让 Omni 模型全程监控人在 AI 产品内的光标活动、说话声音呢？（当然简单一点，也可以是摄像头同时对着人和电脑屏幕）

⬇️

是不是就能更进一步降低 AI 产品的使用门槛，让更多不熟悉电脑操作、未经精准训练语言表达的用户，体验到 AI 时代的 Vibe 操作便利？

比如，**随便写写画画、指指点点， Qwen3.5-Omni-Plus** 更进一步拉低 AI Coding 的难度（左边是对着线框光标指点，右边是最终 Omni 的 Coding 效果。为了测试方便，采用了录屏；实际应用可以直接结合纸笔、手指指点来录像）

![](img/7d0daed75f3af8f79f5f76a84d1578e2.png)

[`chatdesign.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UT3mbiROdok52ZxpFBkc1ET2nFc)

![](img/fdb97e9a0b4ee32ed5f03f47bc9f95f6.png)

![](img/99e85c8b5aca505120662334d291f53b.png)

甚至扩大到更广泛的非技术 APP：比如前文的 Vibe Excel 操作，如果 Excel or 系统通用 Agent 支持观察应用内活动，，自然就能更加流畅（无脑）地调用 Agent 智能。

![](img/58cb43b2b95d3c83bd9b7188a55c8a57.png)

---

### 长音频处理：LifeLog 精细化识别（情绪、场景音）

第二个是个人全天录音的识别，提取声音内容、建立个人日志。

前段时间买了大疆 mic3 ，打算用来记录个人的全天录音。早前设计了两套转录分析方案：

+ 一套偏管线：`Qwen3-ASR-Flash` 转写 + `pyannote` 说话人分离 + 后置 LLM 分析
+ 一套依赖海外全模态模型：Gemini 3 Flash 直接从录音原件总结当天日志

前者优势在于能够建立当天的文字记录，但无法识别声音情绪、背景信息，只能从语义进行推理。

后者能自然区分说话人，结合人声音调、环境背景推理更多 lifelog 信息。

恰好 Qwen3.5-Omni-Plus 模型发布，支持长音频识别，可分析音频内的声音细节与效果。

我给了一段 50 分钟的音频，是前两天在 GDPS 的 OPC 论坛上的实录：

![](img/1ae3e6bdb79b7a10e17ba916daaff82e.png)

Omni 模型根据录音中的主持人串词与嘉宾的发言，在 5 秒内就流式返回了当时的分享记录 ⬇️

![](img/9ac18e5ac9dda9f22b3dfc0fbb33582f.png)

特别的，还成功识别了过程中的人物语速、语调特征，更大程度地保留了 lifelog 信息。（实测对比。该任务表现中 Qwen 不弱于 Gemini ）

如果特化要求，细化语调情绪分析，甚至能做到「识别吐字、重音、音调走向」这种程度：

![](img/d1a8d20a2311699d747e3b1a1a14697f.png)

若要求 AI 进一步分析环境背景音，比如在这样 GDPS 这样的场子，就能依靠全模态模型，得到以下的结果：

![](img/3d531867f3fc7ae5e85412b4441328c2.png)

成功推测了出整体所处的环境空间大小。

---

### 多语种端到端语音：外语对练（多音色）

这个能力真的很好用。

Qwen3.5-Omni 既支持实时语音通话，又支持大量国外语言，还能端到端音频输出。

你可以在 Qwen Chat 的语音通话中进行体验：

![](img/a8d1642ac78cc044d4befa385ac27e72.png)

这是我跟Qwen 的语音过程，测试了她在中英日德等多语种会话下的表现，语音切换自如，音色自然真实。

![](img/9636cd6dea4319f99502a9aee0413b4c.png)

[`多语言.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ItJJbWqwNoNBO9x5jfocFGsGnge)

而且支持切换不同的音色（别的我也听不懂，但这英音是真英啊🤣）：

![](img/68a69bc3d3f589027b288aabb816272e.png)

[`多音色切换.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TviGbhL1XoZFYFxo6Asc0xc3nVf)

每种音色也支持了多国语言，为出海产品提供了更多选择。

![](img/2af75a1b644026be626d09582477d86e.png)

---

## 🎐 写在最后

除了以上用法，还有一些能力没有展示，比如实时通话里自动触发的联网检索、音色克隆等。

总得来说，全模态模型发展到如今，我觉得是时候进入 AI 产品设计的考量了：

![](img/ae50485914b416f38c5bae3939c16328.png)

+ 论设备，手机、车载、眼镜——这些设备的交互本来就不是纯文字的
+ 论用户习惯，更多人并不擅长文本准确描述，更习惯口述+简单指点

当模型能直接处理语气、画面和环境，不需要先想办法把用户的直觉反应“有损翻译”为文本，产品设计的空间就不一样了。

在全模态模型的加持下，人与 AI 的交互方式会越接近「让真实人类做事」的形式。

而兼具了全模态与 Agent Coding 能力的 Qwen3.5-Omni 是个开始，往后能做什么，越来越取决于产品侧的想象力。

---

### 👉 如果想上手体验？

+ **普通用户：**请使用 Qwen Chat：`https://chat.qwen.ai/`

![](img/f2f3280afb56a8083e8d0944d58866ed.png)

+ **开发者：**可通过阿里百炼 API 调用

```
【离线】https://help.aliyun.com/zh/model-studio/qwen-omni
【实时】https://help.aliyun.com/zh/model-studio/realtime
```

![](img/e29f0188f7b64999ed561ed8c7e57920.png)
