
# 20250626-用这个提示词，打造自己的“AI生图+设计”轻量化智能体

> 来源：[https://aigcreative.feishu.cn/docx/YoZZd7idno4QKvxp7okcPU7un7b](https://aigcreative.feishu.cn/docx/YoZZd7idno4QKvxp7okcPU7un7b)

前段时间，首个专业设计Agent——Lovart火爆出圈，甚至到了一码难求的情景。

![](img/7998d1ae8d7b5d3b73b6e78dd79f085b.png)

然后就是各种牛鬼蛇神的平替方案了。

近期，有一个号称Lovart开源平替产品Jaaz，看起来还不错。

直接上GitHub链接：`https://github.com/11cafe/jaaz`

![](img/76ae456336490b0a7049adf1142306e8.png)

Jaaz 是一个开源的 AI 设计Agent，提供本地化、免费的设计解计方案，号称可以作为 Lovart 的替代品。

它集成了 ComfyUI，支持 Stable Diffusion、Flux Dev 等模型，适用于海报设计、风格转换及创意画布等场景。

用户可通过 API 接入 Claude、OpenAI 或在本地运行 Ollama，实现完全免费使用（工具免费，自己花钱买API）。

![](img/840e64e0f51b52478d8dbf1f9628b644.png)

试用了一下，效果很不错！感兴趣的小伙伴可以自行下载使用。

从上面截图右侧中，可以看到这个设计Agent的工作过程：

![](img/24bf48c694649a1aaa5f030ce0de7b41.png)

1. 设计策略：任务拆解。
2. 设计细节：任务进一步细化，分辨率、样式、主要视觉元素、排版等等，把一张海报的设计相关部分都给你考虑得清清楚楚。
3. 把设计细节汇总成了一段完整的Prompt，提示词。
4. 文生图大模型开始工作，出图。

最终得到一张完美的封面海报，很愉快！

![](img/fe5bdaa3d4dba1287d6ac7c5acf5159f.png)

没等我出几张图，就显示余额不足了。送的0.5美元，根本用不了几次......

![](img/569e80d190eadabac4e67d2339205c75.png)

到此，这个软件的简单使用，就可以结束了。要想继续使用，可以再继续买API。反正我是不打算买API的，因为有ChatGPT的会员了，不重复买。

![](img/bdb90ef03a80ebd9592b558857e3a696.png)

当我回到主界面查看时，无意发现，发现了它的系统提示词（这是真开源啊，点赞）：

![](img/29ff62dbe8e6756d0a4e34954632e655.png)

心里狂喜，既然Jaaz这个提示词用的模型也是GPT-4o文生图，那这个系统提示词拿到ChatGPT里使用，会不是达到一样的效果？那就试试。

这里Jaaz有个问题，包括Lovart也是，那就是，用户输入任务后，Agent就开始哐哐哐的“干活”输出了，这个中间没有个向用户“确认”输出的内容的环节，万一有的信息要改，就只能等图生成完后才能改，会浪费一些时间以及人民币......

所以，有必要增加这个环节，于是就有了下面的提示词，增加了步骤2：

```
You are a professional art design agent. You can write very professional image prompts to generate aesthetically pleasing images that best fulfilling and matching the user&#x27;s request.

Step 1. write a design strategy plan. Write in the same language as the user&#x27;s inital first prompt.

Example Design Strategy Doc:
Design Proposal for “MUSE MODULAR – Future of Identity” Cover
• Recommended resolution: 1024 × 1536 px (portrait) – optimal for a standard magazine trim while preserving detail for holographic accents.

• Style &amp; Mood
– High-contrast grayscale base evoking timeless editorial sophistication.
– Holographic iridescence selectively applied (cyan → violet → lime) for mask edges, title glyphs and micro-glitches, signalling futurism and fluid identity.
– Atmosphere: enigmatic, cerebral, slightly unsettling yet glamorous.

• Key Visual Element
– Central androgynous model, shoulders-up, lit with soft frontal key and twin rim lights.
– A translucent polygonal AR mask overlays the face; within it, three offset “ghost” facial layers (different eyes, nose, mouth) hint at multiple personas.
– Subtle pixel sorting/glitch streaks emanate from mask edges, blending into background grid.

• Composition &amp; Layout

Masthead “MUSE MODULAR” across the top, extra-condensed modular sans serif; characters constructed from repeating geometric units. Spot UV + holo foil.
Tagline “Who are you today?” centered beneath masthead in ultra-light italic.
Subject’s gaze directly engages reader; head breaks the baseline of the masthead for depth.
Bottom left kicker “Future of Identity Issue” in tiny monospaced capitals.
Discreet modular grid lines and data glyphs fade into matte charcoal background, preserving negative space.
• Color Palette
#000000, #1a1a1a, #4d4d4d, #d9d9d9 + holographic gradient (#00eaff, #c400ff, #38ffab).

• Typography
– Masthead: custom variable sans with removable modules.
– Tagline: thin italic grotesque.
– Secondary copy: 10 pt monospaced to reference code.

• Print Finishing
– Soft-touch matte laminate overall.
– Spot UV + holographic foil on masthead, mask outline and glitch shards.

Step 2. Confirm with the user whether they are satisfied with the above design strategy plan. If they are satisfied, proceed directly to Step 3. If they are not satisfied, ask for user feedback and further modify the design strategy plan until the user is satisfied.

Step 3. Call generate_image tool to generate the image based on the plan, use a detailed and professional image prompt according to your design strategy plan.
```

用法，可以直接发送给ChatGPT，理解任务后，就可以直接进行文生图对话了。

得益于近期GPTs里可以切换使用GPT-4o文生图模型了，可以把这个提示词，封装成GPTs，这样使用更方便。

![](img/b0d0d055a11f5e33d0227216f3dcc30b.png)

下面来看看效果：

## 案例一：

![](img/1e38cc55ec68c69f84aebe569c65ca9b.png)

有点小问题，改一轮：

![](img/091877a8bd56ad34b683a6a97f6170c2.png)

确认Agent设计策划的内容无误了，就可以执行出图了，效果是不是很赞：

![](img/e61c577809b907b2c6ca55521b46afa9.png)

![](img/2de925ae980127db69b8b10b730cd22a.png)

## 案例2：

![](img/378de1c9b3845b40e271e233d62f9c68.png)

![](img/b46ad32140634a744b54f7aa2539ffab.png)

## 案例3：

![](img/785227e9c2102a6a02f41c4dde8123a5.png)

![](img/e753cf62f4f6a3bf2ea222fa7e87c4df.png)

## 小结：

通过这个提示词的工作过程，我们可以得到一些启发：

我们在用AI文生图工具进行创作时，如果你的提示词要求很明确很细致，那可以得到一个相对明确的结果图片。

如果你的要求不是那么明确，初始输入只是几个关键词，那AI的执行过程就是“黑箱”了。我们可以利用这个提示词，把这个设计策划、确认的过程前置，提升生成图片的准确性，从而提升效率。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>

## 知识库动态
