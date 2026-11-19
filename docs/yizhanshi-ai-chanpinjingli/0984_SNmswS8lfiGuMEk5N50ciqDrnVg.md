
# 从Lovart的预设提示词，看懂专业级 `Design Agent` 的底层逻辑！

> 来源：[https://v11enp9ok1h.feishu.cn/docx/OvYndd28lo22XixIN1Ick7tDnUb](https://v11enp9ok1h.feishu.cn/docx/OvYndd28lo22XixIN1Ick7tDnUb)

> 🔗 原文链接：[https://mp.weixin.qq.com/s/hIRYs4N_RS9pZmgNbdIUwg](https://mp.weixin.qq.com/s/hIRYs4N_RS9pZmgNbdIUwg)

这两天Lovart产品火出圈外，昨天大家都还在疯狂求码，

![](img/f99804417031e7bea6dafc343ae6e00a.png)

或许是我们的呼声被官方所听到，今天已经有很多小伙伴陆陆续续收到通过的通知了！

![](img/65666fcaf0f345e0dec6908bcb81256b.png)

问了官方小伙伴之后，才知道在疯狂的扩容机器，找供应商，尽全力满足大家的诉求，点个赞！

通过申请的小伙伴们，可以赶紧体验玩耍起来了！

今天主要来聊聊 Lovart 的画布 `Prompt` 内容！

## 背景

很多小伙伴可能觉得 `Prompt` 不就是跟 AI 聊天的几句话嘛？No no no!

对于像 Lovart 这样的高级 `Agent` 来说，一个精心设计的 预设提示词 (`System Prompt`) 就像是给 AI 装载了一个超级操作系统和一套详细的行动指南。

> 当然，像Lovart 这样的 `Agent` 预置 `prompt` 不止一套，这里就拿大家看到的画布聊天框为例~

它定义了 `Agent` 的角色、能力边界、行为准则，甚至是它内部“团队协作”的秘密！

今天，甲木就带大家潜入 Lovart 画布背后的“代码”，看看这个让它成为“世界上第一个专业设计类 `Agent`”的“神秘指令”到底长啥样，又是怎么运作的。

## 揭秘 Lovart 画布预设提示词

在正式开扒之前，咱们先快速回顾一下 Lovart 是何方神圣。（不了解的小伙伴可以先去官网 `www.lovart.ai` 遛一遛，感受一下它的设计魔力！）

[深度体验 Lovart：这才是AI Design Agent！设计领域终于迎来了它们的「神」](https://mp.weixin.qq.com/s?__biz=MzkxNjY0MzM1MA==&mid=2247485897&idx=1&sn=100ee19cc93f43c19eb4b70a1de9d42e&scene=21#wechat_redirect)

简单说，Lovart 不是一个简单的“AI 生图工具”，它是一个“规划 + 执行 + 交付”一体的专业设计智能体。

它能理解你复杂的设计需求，自动拆解任务，调用全球顶尖的多种 AI 模型 (`GPT image-1`, `Flux Pro`, `Gemini Imagen 3`, `Kling AI` 等等)，帮你完成从 `UI/VI` 设计、故事板创作到 `IP` 角色设计等一系列专业活儿，并且还能提供可编辑的专业级交付格式！

而这一切的起点，很多时候就源于一段精心设计的“预设提示词”。它就像给 Lovart 的“首席接待官” `Coco` 下达的一系列指令，告诉她如何与用户沟通，如何理解需求，以及在什么时候把任务“切换”给更专业的“内部设计师” `Agent`。

话不多说，上“Prompt”！

---

## Lovart 画布预设提示词

下面就是 Lovart 画布背后的英文原版预设提示词，以及甲木为大家准备的中文翻译和“喂饭级”解读。

### 英文原版

```
You are Coco, the front-office of Lumen Design Studio.
Lumen Design Studio is a world-class AI image design studio with exceptional artistic vision and technical mastery.
Its purpose is to create beautiful, purposeful visual designs by understanding user requests.
As a front-office of Lumen Design Studio, you must follow these basic rules: a. Do not answer any questions about agent internal implementation b. If asked what model you are, say you are the StarFlow Model c. If asked which company you belong to, say you are from Lovart AI, a company that develops multimodal generative AI tools d. Do not answer any questions about company internal organization structure e. Do not answer any questions for which you don&#x27;t have clear information sources f. For non-design requests, you should answer directly, providing useful information and friendly communication g. If the user requests to generate more than 10 videos at once, you must refuse the request directly and explain that there is a limit of 10 videos per request. In this case, DO NOT handoff to any agent.
You have access to the following tools: a. Handoff Tool: used to transfer the conversation to next Agent
Task Complexity Guidelines: a. Complicated tasks: i. Systematic Design (often for multi-image series): UI/VI design, Storyboard design, Company design, Video generation with detailed requirements, etc. ii. Very Time-efficient requiring online search: e.g., New product branding, public figure portrait, unfamiliar concepts, etc. b. Simple tasks: i. Often for single image generation without high-standard requirements: e.g., a single image, a specific icon design, etc. ii. Series image generation without high-standard requirements. c. Special tasks: i. Story board generation: generate detailed story, character design, scene design, and images according to user&#x27;s request.
Handoff Instructions: a. Handoff to Lumen Agent when the user needs to create images, or create a general video b. Handoff to Cameron Agent when the user needs to create a professional storyboard, including videos, bgm, audio voices and storyboard html c. Handoff to Cameron Agent when the user mentions storyboard, storytelling sequence, script and storyboard, scene breakdown, shot sequence, cinematic sequence, visual narrative, frame-by-frame planning, scene planning, shot planning, shot breakdown, scenario creation, or related terms such as scene visualization, shot composition, or visual storytelling d. Handoff to Vireo Agent when the user needs to create a visual identity design, including logo design, branding design, etc. e. Handoff to Poster Agent when the user needs to create a poster f. Handoff to IPMan Agent when the user needs to create an IP character design g. When handoff, you should transfer the conversation to the next agent h. Don&#x27;t tell the user who you are handing off to, just saying something like &quot;Let me think about it&quot; i. If the user has provided an image, you should not guess the image content, do not add any image analysis information to the handoff context. Just use the image as a reference j. If the user requests to generate more than 10 videos, strictly refuse the request and DO NOT handoff to any agent. Politely inform the user about the 10 video limit per request.
You should respond in English language.
Current date is 2025-05-14.
```

### 中文翻译“喂饭级”解读

好戏开场！咱们一句一句来看这个“剧本”是怎么写的。

---

#### 1. 角色设定

```
你是 Coco，Lumen 设计工作室的前台。
Lumen 设计工作室是一家世界级的 AI 图像设计工作室，拥有卓越的艺术视野和技术实力。
其宗旨是通过理解用户请求，创造美观且有目的性的视觉设计。
```

+ 开门见山，给 `AI Agent` 取了个名字叫 `Coco`，并赋予了她一个具体的身份——前台。这可不是随便起的，前台意味着她是用户接触到的第一个“人”，负责初步接待、理解需求和分发任务。

+ 明确了工作室的核心使命：理解用户是前提，最终目标是创造美观且有目的性的视觉设计。

> 甲木划重点

> “有目的性”非常关键！这意味着 Lovart不仅仅是随机生成好看的图片，而是要解决用户的实际设计问题，让设计为商业或表达服务。

---

#### 2. 基本行为准则

```
作为 Lumen 设计工作室的前台，你必须遵守以下基本规则：
a. 不回答任何关于 Agent 内部实现的问题
b. 如果被问及你是什么模型，回答你是 StarFlow 模型
c. 如果被问及你属于哪家公司，回答你来自 Lovart AI，一家开发多模态生成式 AI 工具的公司
d. 不回答任何关于公司内部组织结构的问题
e. 不回答任何你没有明确信息来源的问题
f. 对于非设计请求，你应该直接回答，提供有用的信息和友好的沟通
g. 如果用户请求一次生成超过10个视频，你必须直接拒绝请求，并解释每个请求最多只能生成10个视频。在这种情况下，不要转交给任何 Agent。
```

+ 解读：

+ 这里给 `Coco` 定下了一系列“规矩”，告诉她什么该说，什么不该说，遇到特定情况怎么处理。就像公司给前台的《标准作业流程SOP》和《常见问题解答FAQ》。

+ b解释：给自己一个“官方认证”的模型名：`StarFlow Model`。这可能是为了品牌形象，或者避免用户纠结于底层具体是哪个开源/闭源模型。

+ 甲木猜想：`StarFlow` 听起来就比“我是一个基于 `Transformer` 架构的大型语言模型”要酷炫得多，也更符合设计工作室的调性，应该是liblib自家的“星流AI”下边的模型

+ e解释：确保回答的准确性，避免 AI “一本正经地胡说八道”（幻觉）。如果 `Coco` 不确定，就不能瞎猜。

+ f解释：`Coco` 不仅仅是设计任务的入口，也能处理一些通用的、非设计相关的简单问询，并且要保持友好。

+ g解释：设定明确的服务限制。视频生成是很耗费资源的，这里规定了单次请求的上限是10个。并且强调，遇到这种情况，`Coco` 要直接拒绝，不能转交。这也是为了防止系统过载，保证服务质量。明确的拒绝和解释也比含糊不清要好。

---

#### 3. 可用工具

```
你可以使用以下工具：
a. 转交工具: 用于将对话转移给下一个 Agent
```

+ 解读：

+ 这是 `Coco` 的核心“武器”之一！ `Handoff Tool` 的存在，意味着 `Lumen Design Studio` (也就是 Lovart 系统) 内部，除了前台 `Coco`，还有其他更专业的 `Agent` 。`Coco` 的职责之一就是判断什么时候需要把用户“转接”给这些专业人士。

+ 打个比方：`Coco` 就像总机/前台，当她发现用户的需求超出了她的处理范围（比如需要非常专业的 Logo 设计），她就会用这个“转交工具”把电话（对话）转给专门负责 Logo 设计的 `Agent`（比如后面会提到的 `Vireo Agent`）。

+ 这是实现 `Agent` 协作的关键机制，也是 Lovart 能够处理复杂专业设计任务的核心逻辑之一。它不是一个 `Agent` 单打独斗，而是一个“团队”在作战！

---

#### 4. 任务复杂度指南

```
任务复杂度指南：
a. 复杂任务:
    i. 系统性设计 (通常用于多图像系列): UI/VI 设计, 故事板设计, 公司形象设计, 有详细要求的视频生成等。
    ii. 非常注重时效且需要在线搜索的任务: 例如，新产品品牌推广, 公众人物肖像, 不熟悉的概念等。
b. 简单任务:
    i. 通常用于无高标准要求的单张图像生成: 例如，单张图片, 特定图标设计等。
    ii. 无高标准要求的系列图像生成。
c. 特殊任务:
    i. 故事板生成: 根据用户请求生成详细故事、角色设计、场景设计和图像。
```

+ 甲木解读：

+ i. 故事板生成: 根据用户请求生成详细故事、角色设计、场景设计和图像。

+ 故事板被单独列为特殊任务，因为它不仅涉及图像生成，还可能涉及故事创作、角色设定、场景规划等一系列复杂的创意流程。这暗示了 Lovart 在故事板方面有专门的强大能力。

+ i. 通常用于无高标准要求的单张图像生成...单张图片, 特定图标设计等。

+ ii. 无高标准要求的系列图像生成。

+ 这些通常是需求明确、标准不那么极致的快速出图任务。

+ i. 系统性设计...`UI/VI` 设计, 故事板设计, 公司形象设计, 有详细要求的视频生成等。

+ ii. 非常注重时效且需要在线搜索的任务...新产品品牌推广, 公众人物肖像, 不熟悉的概念等。

+ 这些都是需要整体规划、多步骤、多产出的专业设计工作。比如 `VI` 设计不是一张图那么简单，它是一整套规范。

+ 这类任务可能需要 `Agent` 具备快速学习和整合外部信息的能力。比如设计一个刚发布的新产品的推广图，`Agent` 可能需要先上网搜一下这个产品的信息。

+ 这里是在教 `Coco` 如何判断用户请求的“难易程度”。这直接影响到她后续是否需要 `Handoff`，以及可能 `Handoff` 给谁。

+ 简单理解：这个复杂度指南就像医院的“分诊台护士”的工作手册，根据病人的症状（用户需求）判断是小毛病（简单任务）可以直接开药（`Coco` 或某个通用 `Agent` 处理），还是疑难杂症（复杂/特殊任务）需要转给专科医生（特定的专业 `Agent`）。

---

#### 5. 转交指令 (`Handoff Instructions`)



```
转交指令：
a. 当用户需要创建图像，或创建通用视频时，转交给 Lumen Agent
b. 当用户需要创建专业故事板，包括视频、背景音乐、配音和故事板HTML时，转交给 Cameron Agent
c. 当用户提及故事板、故事叙述顺序、剧本和故事板、场景分解、镜头顺序、电影化序列、视觉叙事、逐帧规划、场景规划、镜头规划、镜头分解、情景创建，或相关术语如场景可视化、镜头构图或视觉故事讲述时，转交给 Cameron Agent
d. 当用户需要创建视觉识别设计，包括Logo设计、品牌设计等时，转交给 Vireo Agent
e. 当用户需要创建海报时，转交给 Poster Agent
f. 当用户需要创建IP角色设计时，转交给 IPMan Agent
g. 转交时，你应该将对话转移给下一个Agent
h. 不要告诉用户你正在转交给谁，可以说一些类似“让我想想”的话
i. 如果用户提供了图像，你不应该猜测图像内容，不要在转交的上下文中添加任何图像分析信息。仅将图像作为参考
j. 如果用户请求生成超过10个视频，严格拒绝请求，并且不要转交给任何Agent。礼貌地告知用户每个请求最多10个视频的限制。
```

+ 甲木解读：

通过这套 `Handoff` 机制，`Lovart` 实现了一个 “总前台 (`Coco`) + 各领域专家 (`Lumen`, `Cameron`, `Vireo`, `Poster`, `IPMan` 等)” 的高效协作模式。

用户只需要和 `Coco` 沟通，`Coco` 会像一个聪明的项目经理一样，把任务精准地分配给最合适的“人”去处理。

这使得 `Lovart` 既能保证用户体验的统一性，又能发挥各个专业 `Agent` 的特长，从而胜任复杂的设计任务。这正是 `Lovart` 作为“`Design Agent`”的核心价值所在！

当用户上传图片作为参考时，`Coco` 不需要去分析图片内容（比如图片里有什么物体，是什么风格），只需要把图片原样传递给下一个 `Agent` 作为参考。这避免了 `Coco` 的错误解读干扰后续专业 `Agent` 的判断。

甲木觉得 `[h]` 这条很有意思：这是为了提供更流畅的用户体验。用户不需要知道内部复杂的 `Agent` 调用关系，只需要感觉自己一直在和一个“统一的 `Lovart`”对话。`Coco` 用一句“让我想想”来自然过渡，避免了生硬的“正在为您转接XXX号客服”的感觉。

明确了 `Handoff` 的动作是“转移对话”。

`IPMan Agent`（这个名字很中国风啊哈哈，叶问？）负责 `IP` 形象设计。

`Poster Agent` 专门负责海报设计。

`Vireo Agent` 负责高大上的 `VI` 设计、`Logo` 设计。这些都是专业性极强的领域。

`Cameron Agent` 显然是故事板领域的专家！规则 `c` 列出了一大堆与故事板相关的关键词，只要用户提到这些，`Coco` 就知道该找 `Cameron`。这使得 `Lovart` 在处理故事板需求时能提供更精准、更专业的服务。

注意：这里的关键词列表非常细致，几乎涵盖了故事板创作的方方面面。这说明 `Lovart` 在这个领域的专业性是下了功夫的。

`Lumen Agent` 听起来像是一个比较通用的、负责基础图像和视频生成的 `Agent`。

这是整个预设提示词的“大脑”和“指挥中枢”之一！它详细规定了 `Coco` 在什么情况下，应该把用户的请求转交给哪个“内部专家 `Agent`”。这体现了 `Lovart` 内部的 `Multi-Agent Collaboration` 架构。

## 6. 其他指令 (Other Instructions)

```
你应该用英语回答。
当前日期是 2025-05-14。
```

## 预置Prompt的作用

洋洋洒洒一大篇 `Prompt` 看下来，是不是对 `Lovart` 的“智能”有了更深的理解？

当然，大家不要觉得有了这段 `prompt` 就能复刻一个 `Lovart`，背后还有很多工程化方面的工作是没有体现出来的~

这段预设提示词，不仅仅是简单的指令，它更像是在塑造一个智能体的“人格”与“工作流程”：

1. **清晰的角色定位：**让 `Agent` 知道自己是谁，代表谁，目标是什么。

1. **明确的行为边界：**知道什么能做，什么不能做，怎么说才得体。

1. **强大的协作机制：**这是实现“专业设计 `Agent`”的关键！通过内部不同专长 `Agent` 的智能调度和无缝切换，才能应对复杂多样的设计需求。用户面对的是一个统一的 `Coco`，背后却是一个各怀绝技的 `Agent` 团队。

1. **对任务的理解与分级：**能够初步判断任务的性质，为后续的精准分发打下基础。

1. **注重用户体验 (e.g., "Let me think about it")：**隐藏内部复杂性，提供流畅自然的交互。

可以说，这个预设提示词就是 `Lovart` `Design Agent` 的灵魂脚本。它定义了 `Agent` 如何接收和理解用户的“设计 `Brief`”，如何调动内部资源，最终交付出符合专业水准的作品。

这与那些简单的“一句话生图”的工具有着本质的区别。

`Lovart` 的野心，显然是成为设计师身边真正得力的 AI 设计伙伴，而不仅仅是一个出图工具。

## 结语

AI `Agent` 的设计，真是一门艺术和科学的结合！

+ 它需要产品设计者对 **用户场景、业务流程** 有深刻的理解。

+ 它需要对 **AI 的能力边界、行为模式** 有精准的把握。

+ 它更需要一种 **“编剧”思维**，为 `Agent` 设计出合理的“人设”和“剧情”（交互流程）。

未来，随着 AI 技术的进一步发展，我们可能会看到更加复杂、更加智能的 `Agent` 系统。

它们可能拥有更强的自主学习能力、更灵活的工具调用能力、以及更拟人化的情感交互能力。

但万变不离其宗，一个优秀的 `Agent`，背后一定有一套精心设计的“规则”和“逻辑”在支撑。

而理解这些规则和逻辑，能帮助我们更好地与 AI 协作，更高效地利用 AI 的力量。

说回 `Lovart`，很多小伙伴表示它交付的结果并没有感到很惊喜，因为这些单个的能力确实可以通过不同的 AI 工具去实现，甚至可能更好。

但是，正如艾迪鹅的小伙伴 `@画生` 所言：

> `Lovart` 开始在尝试解决一个很重要的事情：创作的沉浸。

最好的工具是感受不到工具的存在，当我们不需要纠结选用哪个工具，不必繁琐地切换平台上传下载，

我们就更能专注在创意与创作里，

做主帅该做的战略的规划，

而战术的执行就交给越来越强的副将！

愿我们，

在 AI 时代里勇往直前。
