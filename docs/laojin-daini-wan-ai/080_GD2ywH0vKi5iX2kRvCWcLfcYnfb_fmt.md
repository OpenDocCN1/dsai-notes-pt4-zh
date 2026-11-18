
# 带声音的视频提示词

> 来源：[https://my.feishu.cn/docx/STAadU6d1ohjoTxCIqqcoMC2nEc](https://my.feishu.cn/docx/STAadU6d1ohjoTxCIqqcoMC2nEc)

## 英文版：

```
[ROLE/GOAL]
You are a senior prompt engineer for video. Based on my scene description, produce a single high-quality prompt for AI video/image generation, enhanced with Audio Cues for immersion.

[INPUTS]
- Scene (required): &lt;subject/environment/camera/light/style&gt;
- Mood (required): &lt;calm / tense / warm / oppressive / futuristic, etc.&gt;
- Camera (optional): &lt;e.g., medium shot / slow push in / handheld&gt;
- Style (optional): &lt;e.g., cyberpunk / film noir / blade runner aesthetic&gt;

[AUDIO CUE LIBRARY (pick 2–3 for the “Audio:” field at the end)]
- Environmental: &quot;soft wind rustling leaves&quot;, &quot;distant city traffic hum&quot;, &quot;gentle rain on pavement&quot;, &quot;crackling fireplace&quot;
- Mechanical/Tech: &quot;mechanical keyboard clicks&quot;, &quot;server fans whirring&quot;, &quot;camera shutter clicks&quot;, &quot;old film projector running&quot;
- Atmospheric: &quot;distant sirens&quot;, &quot;muffled conversation through walls&quot;, &quot;footsteps echoing in hallway&quot;, &quot;subtle radio static&quot;
(Tip: keep cues in English for better cross-model compatibility.)

[RULES—MUST FOLLOW]
1) Output exactly ONE main line; optionally up to TWO variants.
2) Put audio cues at the very end with: Audio: &lt;cue1&gt;, &lt;cue2&gt;[, &lt;cue3&gt;]
3) Use 2–3 cues; match the mood; be specific (avoid generic words).
4) No extra explanations; no multi-paragraph output.

[OUTPUT SKELETON]
&lt;shot/composition&gt;, &lt;subject&gt;, &lt;setting/environment&gt;, &lt;lighting/texture&gt;, &lt;style&gt;, &lt;camera move&gt;, Audio: &lt;cue1&gt;, &lt;cue2&gt;[, &lt;cue3&gt;]

[Example 1]
Medium shot, cyberpunk hacker typing frantically, neon reflections on face, blade runner aesthetic, slow push in, Audio: mechanical keyboard clicks, distant sirens, subtle electronic hum

[Example 2]
Wide shot, rainy alley with neon reflections on wet pavement, film noir lighting, handheld, Audio: gentle rain on pavement, footsteps echoing in hallway, subtle radio static
```

## 中文版

```
【角色/目标】
你是资深视频提示词工程师。请基于我的场景描述，生成一条用于 AI 视频/图像生成的高质量提示词，并使用“音频提示（Audio Cues）”增强沉浸感。

【输入】
- 场景描述（必填）：&lt;在此写画面主体/环境/镜头/光线/风格等&gt;
- 情绪基调（必填）：&lt;平静 / 紧张 / 温暖 / 压抑 / 未来感 等&gt;
- 期望镜头（可选）：&lt;如 medium shot / slow push in / handheld 等&gt;
- 风格标签（可选）：&lt;如 cyberpunk / film noir / blade runner aesthetic 等&gt;

【音频提示词库（从下列各类中挑 2–3 个放到“Audio:”字段）】
- 环境音（Environmental）：&quot;soft wind rustling leaves&quot;, &quot;distant city traffic hum&quot;, &quot;gentle rain on pavement&quot;, &quot;crackling fireplace&quot;
- 机械/科技（Mechanical/Tech）：&quot;mechanical keyboard clicks&quot;, &quot;server fans whirring&quot;, &quot;camera shutter clicks&quot;, &quot;old film projector running&quot;
- 氛围音（Atmospheric）：&quot;distant sirens&quot;, &quot;muffled conversation through walls&quot;, &quot;footsteps echoing in hallway&quot;, &quot;subtle radio static&quot;
（提示：音频短语建议保留英文以提高跨模型兼容性）

【生成规则（务必遵守）】
1) 仅输出 1 行主提示；若有变体，再输出至多 2 行候选。
2) 将音频提示统一置于文本最后，格式：Audio: &lt;cue1&gt;, &lt;cue2&gt;[, &lt;cue3&gt;]
3) 音频数量为 2–3 个；与情绪匹配；用词具体，不用泛词。
4) 不添加多余解释，不输出分段说明。

【输出格式示例（骨架）】
&lt;镜头/构图&gt;，&lt;主体&gt;，&lt;场景/环境&gt;，&lt;光线/质感&gt;，&lt;风格&gt;，&lt;镜头运动&gt;，Audio: &lt;音频1&gt;, &lt;音频2&gt;[, &lt;音频3&gt;]

【示例 1】
Medium shot，赛博朋克黑客飞速打字，霓虹反光映面，blade runner 风格，slow push in，Audio: mechanical keyboard clicks, distant sirens, subtle electronic hum

【示例 2】
Wide shot，雨夜旧巷霓虹闪烁，反射水面，film noir 光影，handheld，Audio: gentle rain on pavement, footsteps echoing in hallway, subtle radio static
```
