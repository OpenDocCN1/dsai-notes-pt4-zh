# skills

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/BJ9IdkzG2o7czixZqPEcDTtSnbe](https://xatc0v8uz5m.feishu.cn/docx/BJ9IdkzG2o7czixZqPEcDTtSnbe)

## 推荐一些Skills

Anthropic官方Skills

> https://github.com/anthropics/skills

ComposioHQ 提供的Awesome Skills列表

> https://github.com/ComposioHQ/awesome-claude-skills/tree/master

不过，上面的太大路货了。

再给几个我用过觉得不错的。

### **生图自动化内容创作与发布**

https://github.com/JimLiu/baoyu-skills

主要目标是**自动化内容创作与发布，让 Claude Code 具备“从**文本直接生成专业视觉内容并发布”的能力。

内容生成和发布技能。

baoyu-skills 是给 Claude 配了一个“美工团队”和一个“新媒体运营团队”。

主要增量价值确实在于图像化和发布操作，而不是教 Claude 如何写文章。

```
**JimLiu (Ba****oyu) 分享。它的****主要目标****是自动**化内容创作与发布，极大地提高工作效率。
简单来说，它能让 Claude Code 具备“从文本直接生成专业视觉内容并发布”的能力。
以下是它包含的主要能力模块：
1. 视觉内容生成 (Content Creation)
这是该 Skill 最核心的功能，支持多种格式的专业图片生成：
小红书/社交媒体配图 (baoyu-xhs-images)：
将 Markdown 文章或短文本自动转换为小红书风格的系列图片（1-10张）。
支持多种布局（如：清单体、对比图、流程图）和风格（如：可爱、极简、科技感）。
专业信息图表 (baoyu-infographic)：
将复杂内容转化为可视化图表。
支持 20 种布局（如：漏斗图、鱼骨图、金字塔、思维导图）和 17 种视觉风格。
文章封面图 (baoyu-cover-image)：
根据文章内容自动生成手绘风格或设计感强的封面图。
PPT 幻灯片 (baoyu-slide-deck)：
从 Markdown 文章生成完整的 PPT 大纲和对应的幻灯片图片。
最后可自动合并导出为 .pptx 文件。
知识漫画 (baoyu-comic)：
将枯燥的知识或故事转化为多格漫画（支持日漫风格、美漫风格等）。
适合制作教育类或叙事类内容。
文章自动配图 (baoyu-article-illustrator)：
智能分析长文章，在合适的位置自动插入上下文相关的插图。
2. 自动化发布 (Publishing Automation)
生成内容后，它可以直接帮你发布到社交平台（通过调用本地 Chrome 浏览器）：
发布到 X (Twitter) (baoyu-post-to-x)：支持发推文、带图推文以及长文章 (X Articles)。
发布到微信公众号 (baoyu-post-to-wechat)：支持发布图文消息（多图模式）和富文本文章。
3. 底层工具与实用程序
AI 绘图后端 (baoyu-image-gen)：集成了 OpenAI (DALL-E) 和 Google (Imagen) 的 API，支持高质量绘图、参考图生图。
实用工具：包括 URL 转 Markdown、图片压缩等功能，辅助内容处理。
总结： 如果你是内容创作者或开发者，这个 Skill 相当于给你的 Claude Code 装上了一个**“全能设计部 + 社交媒体运营部”**。你只需要写好文字草稿，它就能帮你画图、排版、做 PPT，甚至直接发帖。
```

### 视频剪辑

https://github.com/remotion-dev/skills/blob/main/skills/remotion/SKILL.md

**媒体资源管理**：音频、视频、图像、GIF、Lottie 动画及字体的导入与处理（含裁剪、变速、音量控制）。

**动画与渲染**：基础动画、Three.js (3D) 集成、缓动插值 (Interpolation/Springs)、场景转场及文本动画。

**布局与样式**：集成 TailwindCSS、动态计算元数据 (Metadata)、测量 DOM 节点与文本尺寸、图表数据可视化。

**时间轴控制**：序列编排 (Sequencing)、修剪 (Trimming)、帧提取以及 Composition（合成）配置。

**字幕与音频处理**：SRT 字幕导入、音频转录生成字幕、TikTok 风格字幕展示及音频时长检测。

### Superpowers

1.6万Star的Skill精选，从脑暴、写需求文档、开发、测试全包含，口碑相当好。

> https://github.com/obra/superpowers

### Planning-with-files

作者Ahmad看 Manus 被20亿美元收购，参考Manus的Agent方法写的Skill。

很适合多步骤任务，用这个Skill指导其他Skill工作也挺好。

AI也需要有计划性，写TodoList，完成Check，不止于遗忘和跑偏。

### X-article-publisher-skill

王树义老师写的X文章发布Skill。

调用Chrome浏览器，自动把文字和图片粘贴到编辑器，还能自动化根据文本定位插入图片。

> https://github.com/wshuyi/x-article-publisher-skill

这个Skill非常值得学习，改造成各种自动发布工具。

### NotebookLM skill

也是控制Chrome浏览器，自动上传PDF、Youtube链接到NotebookLM，程序化提问获取答案，对学习相当有帮助。

最强的是，还能记住登录信息，非常值得研究。

> https://github.com/PleasePrompto/notebooklm-skill