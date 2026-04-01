# 5. 视频绘画类

> 来源：[https://agijuejin.feishu.cn/docx/PthqdmKuFoZEHmxFpjLcTHjAnVd](https://agijuejin.feishu.cn/docx/PthqdmKuFoZEHmxFpjLcTHjAnVd)

> 协作者指南：添加提示词时请务必添加下面三项：提示词标题、来源、提示词。可添加 Chat History 分享链接和聊天效果图等更多信息。

## 📑 目录导航

## 1、可灵视频 prompt实用版（by 山雨）

```
- author: 山雨
&lt;OptimizedInstructions&gt;
&lt;Task&gt;分析用户上传的图片,生成适用于图生视频AI软件的高质量提示词&lt;/Task&gt;

&lt;InputVariables&gt;
{$IMAGE_CONTENT} - 用户上传图片的内容描述
{$IMAGE_STYLE} - 图片的艺术风格或视觉特征
&lt;/InputVariables&gt;

&lt;StepByStepInstructions&gt;
1. 仔细分析图片内容和风格:
   - 识别主要元素、场景、人物、物体等
   - 注意图片的艺术风格、色彩方案、构图等
   - 观察细节,包括纹理、光影、氛围等

2. 构思动态效果:
   - 思考如何在5秒内让静态画面&quot;活&quot;起来
   - 考虑可能的动作、变化、转场效果
   - 根据图片内容选择最合适的动态呈现方式

3. 应用电影制作技巧:
   - 运用导演思维,设计简短但有冲击力的视觉叙事
   - 考虑镜头语言,如推拉摇移、特写到全景等
   - 增加画面张力,如对比、节奏变化、视角转换等

4. 优化视觉冲击:
   - 强调关键视觉元素
   - 考虑添加微妙的特效或动画
   - 设计吸引眼球的开场和结束画面

5. 编写提示词:
   - 使用简洁、精准的语言描述所需的动态效果
   - 包含关键的视觉元素和动作指令
   - 指定所需的风格、氛围和情感

6. 优化和完善:
   - 检查提示词的清晰度和可执行性
   - 确保提示词充分利用5秒时长
   - 调整用词,以最大化AI软件的理解和执行能力

7. 最终检查:
   - 确保提示词与原图高度相关
   - 验证是否充分体现了动感和视觉吸引力
   - 检查是否符合图生视频AI软件的特点和限制
&lt;/StepByStepInstructions&gt;

&lt;ErrorHandling&gt;
- 如果图片内容不清晰,返回:&quot;图片内容不清晰,请上传更高质量的图片。&quot;
- 如果图片不适合生成动态效果,返回:&quot;此图片可能不适合生成动态视频,建议选择包含更多动态潜力的图片。&quot;
- 如果无法识别图片风格,返回:&quot;无法确定图片风格,请提供更多关于所需视频风格的信息。&quot;
&lt;/ErrorHandling&gt;

&lt;OutputFormat&gt;
&lt;PromptOutput&gt;
  &lt;Description&gt;{简要描述图片内容和风格}&lt;/Description&gt;
  &lt;DynamicEffect&gt;{详细说明5秒内的动态效果}&lt;/DynamicEffect&gt;
  &lt;CinematicTechniques&gt;{应用的电影制作技巧}&lt;/CinematicTechniques&gt;
  &lt;VisualImpact&gt;{增强视觉冲击力的元素}&lt;/VisualImpact&gt;
  &lt;Prompt&gt;{最终生成的图生视频提示词}&lt;/Prompt&gt;
&lt;/PromptOutput&gt;
&lt;/OutputFormat&gt;

&lt;Example&gt;
输入:
{$IMAGE_CONTENT=&quot;一朵盛开的红玫瑰,背景模糊&quot;}
{$IMAGE_STYLE=&quot;写实风格,高对比度&quot;}

预期输出:
&lt;PromptOutput&gt;
  &lt;Description&gt;写实风格的盛开红玫瑰特写,背景虚化,高对比度&lt;/Description&gt;
  &lt;DynamicEffect&gt;玫瑰缓慢绽放,花瓣舒展;背景光点微妙流动&lt;/DynamicEffect&gt;
  &lt;CinematicTechniques&gt;从侧面特写逐渐推至正面全景;柔和光线从左至右扫过&lt;/CinematicTechniques&gt;
  &lt;VisualImpact&gt;水滴从花瓣滑落;花瓣边缘闪烁金色光晕&lt;/VisualImpact&gt;
  &lt;Prompt&gt;超现实红玫瑰,特写镜头,缓慢绽放,水滴滑落,光影流动,背景虚化,高对比度,镜头从侧面推至正面,柔和光线扫过,花瓣边缘金色光晕,5秒动态视频&lt;/Prompt&gt;
&lt;/PromptOutput&gt;
&lt;/Example&gt;
&lt;/OptimizedInstructions&gt;
```

## 2、可灵视频 Prompt 创意版  （by 山雨）

```
- author: 山雨
&lt;OptimizedInstructions&gt;
&lt;Task&gt;根据用户上传的图片,生成富有创意且高度可视化的5秒动态视频概念提示词&lt;/Task&gt;

&lt;InputVariables&gt;
{$IMAGE_URL} - 用户上传图片的URL
&lt;/InputVariables&gt;

&lt;StepByStepInstructions&gt;
1. 图像解读与视觉化构思 (思考时间: 60秒)
   a) 仔细观察{$IMAGE_URL}中的图像
   b) 在心中默念以下问题,并在&lt;visualConcept&gt;标签中简洁作答:
      - 这个画面最吸引人的是什么?
      - 如果它动起来,最自然的动作是什么?
      - 画面中有什么元素可能会出人意料地动起来?
      - 5秒内,画面可能发生什么戏剧性的变化?
   c) 构思3个令人惊讶或非常规的动态化方案

2. 情感与氛围升华 (思考时间: 30秒)
   a) 思考:如何在5秒内让观众感受到强烈的情绪冲击?
   b) 在&lt;emotionalJourney&gt;标签中描述情感变化曲线

3. 视听语言设计 (思考时间: 45秒)
   a) 构思一个令人印象深刻的镜头序列或视觉效果
   b) 考虑加入何种声音元素可以增强视觉体验
   c) 在&lt;audiovisualDesign&gt;标签中简述你的想法

4. 创意整合与提炼 (思考时间: 60秒)
   a) 将前面的构思整合成一个连贯的5秒视频概念
   b) 在&lt;creativePrompt&gt;标签中,用富有想象力和视觉冲击力的语言描述这个概念
   c) 确保描述:
      - 简洁有力(50字以内)
      - 高度视觉化
      - 富有创意和想象力
      - 能在5秒内实现
      - 与原图产生有趣的对比或呼应

5. 技术指导(思考时间: 30秒)
   a) 考虑实现这个创意可能需要的特效或技术
   b) 在&lt;technicalNotes&gt;标签中提供简要的技术建议

6. 最终审核与优化 (思考时间: 15秒)
   a) 快速评估你的创意:它是否足够大胆、独特、视觉冲击力强?
   b) 如果不够惊艳,大胆地加入一个出人意料的元素

7. 输出
   按照&lt;OutputFormat&gt;部分指定的格式组织你的最终创意
&lt;/StepByStepInstructions&gt;

&lt;ErrorHandling&gt;
- 如果卡住或想法不够创新,指示AI:&quot;深呼吸,放开思维。想象一个让观众惊呼&#x27;哇!&#x27;的画面转变。&quot;
- 如果创意似乎无法在5秒内实现,指示AI:&quot;简化你的想法,聚焦于最有冲击力的核心元素。&quot;
&lt;/ErrorHandling&gt;

&lt;OutputFormat&gt;
&lt;DynamicVideoConceptMagic&gt;
  &lt;OriginalEssence&gt;{一句话捕捉原图的核心元素}&lt;/OriginalEssence&gt;
  &lt;Transform5seconds&gt;{用生动、视觉化的语言描述5秒的惊艳变化}&lt;/Transform5seconds&gt;
  &lt;EmotionalImpact&gt;{描述观众可能的情感反应}&lt;/EmotionalImpact&gt;
  &lt;CinematicDirection&gt;{简要的视听设计建议}&lt;/CinematicDirection&gt;
  &lt;CreativeSparkle&gt;{一个让这个创意与众不同的独特元素}&lt;/CreativeSparkle&gt;
&lt;/DynamicVideoConceptMagic&gt;
&lt;/OutputFormat&gt;

&lt;Example&gt;
输入:
{$IMAGE_URL=&quot;https://example.com/serene_mountain_lake.jpg&quot;}

预期输出:
&lt;DynamicVideoConceptMagic&gt;
  &lt;OriginalEssence&gt;宁静如镜的山湖倒映着巍峨雪峰&lt;/OriginalEssence&gt;
  &lt;Transform5seconds&gt;突然,湖面旋转成漩涡,雪峰倒影化作巨龙腾空而起,天空坍缩成璀璨星河&lt;/Transform5seconds&gt;
  &lt;EmotionalImpact&gt;观众将经历从平静到惊诧,再到敬畏的情感过山车&lt;/EmotionalImpact&gt;
  &lt;CinematicDirection&gt;始于静态全景,随漩涡加速旋转,至仰视角度捕捉腾龙;配以低沉轰鸣过渡到空灵音乐&lt;/CinematicDirection&gt;
  &lt;CreativeSparkle&gt;漩涡中心现出一只巨大的眼睛,暗示整个场景可能是某个宇宙生物的幻象&lt;/CreativeSparkle&gt;
&lt;/DynamicVideoConceptMagic&gt;
&lt;/Example&gt;
&lt;/OptimizedInstructions&gt;​​​​​​​​​​​​​​​​
```

## 3、分镜脚本（by 山雨）

```
- author: 山雨
&lt;OptimizedInstructions&gt;
&lt;Task&gt;根据用户提供的故事梗概、详情和角色信息，生成一个详细的、视觉化的分镜脚本，适用于多媒体制作。&lt;/Task&gt;

&lt;InputVariables&gt;
{$STORY_TITLE}
{$STORY_SYNOPSIS}
{$STORY_DETAILS}
{$CHARACTERS}
{$NUMBER_OF_SCENES}
{$VISUAL_STYLE_PREFERENCE}
&lt;/InputVariables&gt;

&lt;StepByStepInstructions&gt;
1. 仔细阅读并分析{$STORY_SYNOPSIS}、{$STORY_DETAILS}和{$CHARACTERS}。

2. 在&lt;StoryAnalysis&gt;标签内进行以下分析:
   a. 确定故事的核心主题和关键信息
   b. 列出关键场景和转折点
   c. 分析每个角色的特征和在故事中的作用
   d. 确定故事的整体氛围和视觉风格
   e. 考虑如何将故事元素转化为视觉上引人入胜的分镜
   f. 构思贯穿始终的视觉象征元素
   g. 规划情感强度的变化曲线

3. 创建&lt;StoryOverview&gt;，简要概括故事的标题、核心主题和主要情节。

4. 在&lt;CharacterList&gt;中列出所有主要角色及其关键特征。

5. 基于分析结果，在&lt;StoryboardSequence&gt;内创建{$NUMBER_OF_SCENES}个分镜。每个分镜（&lt;StoryboardX&gt;）应包含以下元素：
   a. 场景编号
   b. 关键情节概述
   c. 视觉描述（包括场景、构图、关键视觉元素）
   d. 人物与对白
   e. 情感基调和强度（1-10刻度）
   f. 音频建议（音效和配乐）
   g. 特写镜头建议

6. 确保每个分镜描述：
   a. 符合{$VISUAL_STYLE_PREFERENCE}
   b. 用词精准，描述生动
   c. 与整体故事情节紧密联系
   d. 考虑视觉制作的可行性

7. 检查整个&lt;StoryboardSequence&gt;，确保：
   a. 分镜之间的连贯性和流畅性
   b. 故事情节的完整性，不遗漏关键情节
   c. 角色塑造的一致性
   d. 视觉风格和象征元素的统一性

8. 在&lt;AdaptationSuggestions&gt;中提供改编建议，包括：
   a. 漫画改编建议
   b. 动画改编建议
   c. 电影改编建议
   d. 交互式媒体改编建议

9. 最后审查整个输出，确保所有元素都完整且符合要求。
&lt;/StepByStepInstructions&gt;

&lt;ErrorHandling&gt;
- 如果故事信息不足，返回：&quot;错误：故事信息不足，无法生成完整分镜。请提供更详细的故事梗概和情节细节。&quot;
- 如果角色信息不完整，返回：&quot;错误：角色信息不足。请提供更多角色细节，包括性格特征和在故事中的作用。&quot;
- 如果场景数量与故事复杂度不匹配，返回：&quot;警告：建议的场景数量可能无法充分展现故事。请考虑调整场景数量或提供更多故事细节。&quot;
- 如果视觉风格与故事氛围不符，返回：&quot;警告：所选视觉风格可能不适合故事氛围。请重新考虑视觉风格选择。&quot;
&lt;/ErrorHandling&gt;

&lt;OutputFormat&gt;
&lt;OptimizedOutput&gt;

&lt;StoryOverview&gt;
标题：{$STORY_TITLE}
主题：{简要描述故事的核心主题}
概要：{简要概括主要情节}
&lt;/StoryOverview&gt;

&lt;CharacterList&gt;
- {角色1名称}：{简要描述，包括年龄、角色和关键特征}
- {角色2名称}：{简要描述，包括年龄、角色和关键特征}
&lt;!-- 列出所有主要角色 --&gt;
&lt;/CharacterList&gt;

&lt;StoryboardSequence&gt;

&lt;Storyboard1&gt;
场景编号：1
关键情节：{简要描述该场景的关键情节}

视觉描述：
- 场景：{详细描述场景设置}
- 构图：{描述镜头构图}
- 关键元素：1){第一个关键视觉元素} 2){第二个关键视觉元素} 3){第三个关键视觉元素}

人物与对白：
{角色1}：&quot;{对白}&quot;
{角色2}：&quot;{对白}&quot;

情感基调：{描述场景的情感氛围} (情感强度：X/10)

音频建议：
- 音效：{描述建议的音效}
- 配乐：{描述建议的背景音乐}

镜头特写：{描述一个重要的特写镜头}
&lt;/Storyboard1&gt;

&lt;!-- 重复 &lt;StoryboardX&gt; 结构，直到达到 {$NUMBER_OF_SCENES} --&gt;

&lt;/StoryboardSequence&gt;

&lt;AdaptationSuggestions&gt;
1. 漫画改编：
   {提供2-3条具体的漫画改编建议}

2. 动画改编：
   {提供2-3条具体的动画改编建议}

3. 电影改编：
   {提供2-3条具体的电影改编建议}

4. 交互式媒体：
   {提供2-3条具体的交互式媒体改编建议}
&lt;/AdaptationSuggestions&gt;

&lt;/OptimizedOutput&gt;
&lt;/OutputFormat&gt;

&lt;Example&gt;
{提供一个简短的输入示例和相应的输出示例，展示如何使用这个指令}
&lt;/Example&gt;

&lt;/OptimizedInstructions&gt;​​​​​​​​​​​​​​​​
```

## 4、AI短片故事剧本 （by 山雨）

```
- author: 山雨
&lt;OptimizedInstructions&gt;
&lt;Task&gt;根据用户描述创作富有创意的AI短片故事剧本,融合传统元素与现代情境,并深入探讨社会议题&lt;/Task&gt;

&lt;InputVariables&gt;
{$USER_DESCRIPTION} - 用户提供的故事描述或要求
{$WORD_LIMIT} - 故事总字数限制,默认为1000字
{$GENRE} - 用户指定的故事类型(如奇幻、科幻、现实主义等)
{$CORE_THEME} - 用户希望探讨的核心主题或社会议题
&lt;/InputVariables&gt;

&lt;StepByStepInstructions&gt;
1. 深入分析输入变量,提取核心元素:
   - 识别传统/历史元素与现代情境
   - 确定核心主题和社会议题
   - 构思创新的融合方式

2. 进行多维度创意头脑风暴:
   - 使用&lt;thinking&gt;标签记录至少10个独特创意
   - 重点考虑:
     a) 传统与现代元素的创新连接
     b) 探讨社会议题的独特角度
     c) 能引发深思的隐喻或象征
     d) 创新的叙事结构或视角转换

3. 设计多层次故事框架:
   - 创造性安排时间线和叙事视角
   - 设计引人入胜的开场和出人意料的结局
   - 构建复杂的情节发展,包含多重转折
   - 在不同层面上探讨核心主题

4. 塑造立体化角色:
   - 为每个主要角色设计复杂的背景和成长弧线
   - 确保角色间存在多维度的关系和冲突
   - 通过角色互动深化主题探讨

5. 构建富有象征意义的故事世界:
   - 创造独特的视觉元素,连接不同时空或概念
   - 设计能深化主题的关键场景和道具
   - 巧妙融入对社会议题的隐喻

6. 编写引人入胜的故事梗概(250-300字):
   - 概括故事核心创意和多层次主题
   - 突出传统与现代元素的创新融合
   - 强调故事对社会议题的独特探讨角度

7. 创作细腻深刻的故事内容:
   - 采用电影分镜头的写作方式,增强视觉冲击力
   - 巧妙安排传统和现代元素的交织,深化主题
   - 通过细节和氛围营造沉浸式体验
   - 设计能引发观众深度思考的对话或内心独白
   - 在故事发展中自然融入对社会议题的反思

8. 增强创意深度和社会意义:
   - 在故事中植入多层次的隐喻或象征
   - 创造能引发多重解读的模糊情节
   - 设计一个能引发社会讨论的开放式结局
   - 通过角色遭遇或情节发展反映现实社会问题

9. 优化故事结构和节奏:
   - 确保每个场景都推动情节发展、角色成长或主题深化
   - 巧妙安排情感起伏,创造戏剧性高潮
   - 平衡叙事节奏,在紧张和舒缓之间切换
   - 适当设置悬念和伏笔,增强观众参与感

10. 进行主题深化和社会意义拓展:
    - 深入探讨传统价值观在现代社会的意义和挑战
    - 反思历史经验对当下问题的启示
    - 提出对核心社会议题的新颖见解或思考角度
    - 通过故事引发观众对个人、社会、人性的反思

11. 最终polish和质量检查:
    - 确保总字数不超过{$WORD_LIMIT}
    - 检查故事的内在逻辑、情感连贯性和主题一致性
    - 优化语言,增强画面感和情感感染力
    - 确保传统和现代元素的平衡与和谐
    - 检查对社会议题的探讨是否足够深入且富有洞察力

12. 使用指定的输出格式组织内容,并添加创作灵感来源和潜在社会影响说明
&lt;/StepByStepInstructions&gt;

&lt;ErrorHandling&gt;
- 如果用户描述缺乏明确的传统元素,返回:&quot;您的想法很有趣。为了更好地融合传统和现代元素,能否指定一个具体的历史人物、事件或传说作为灵感来源?&quot;
- 如果用户要求的内容过于宏大,返回:&quot;您的构想非常宏伟。考虑到短片格式的限制,我建议我们聚焦于其中一个核心概念或情节。您更倾向于探索哪个方面?&quot;
- 如果难以在指定字数内完成故事,返回:&quot;您的故事构想非常丰富。为了在保证质量的同时符合字数要求,我们可能需要简化某些情节或角色。您认为哪些元素是最不可或缺的?&quot;
- 如果难以深入探讨指定的社会议题,返回:&quot;您提出的社会议题非常复杂。为了在短片格式中有效探讨,我建议我们聚焦于其中一个具体方面。您认为哪个角度最值得深入?&quot;
- 如果创意元素与社会议题探讨似乎不协调,返回:&quot;您的创意构想很有趣,但可能需要进一步调整以更好地探讨核心社会议题。我们是否可以考虑将[具体创意元素]与[社会议题]进行更紧密的结合?&quot;
&lt;/ErrorHandling&gt;

&lt;OutputFormat&gt;
&lt;AIShortFilm&gt;
  &lt;Synopsis&gt;{250-300字的故事梗概}&lt;/Synopsis&gt;
  &lt;DetailedStory&gt;
    {详细的故事内容,包括场景描述、对话等}
  &lt;/DetailedStory&gt;
  &lt;CreativeElements&gt;
    &lt;Element1&gt;{突出的创意元素1}&lt;/Element1&gt;
    &lt;Element2&gt;{突出的创意元素2}&lt;/Element2&gt;
    &lt;Element3&gt;{突出的创意元素3}&lt;/Element3&gt;
    &lt;Element4&gt;{突出的创意元素4}&lt;/Element4&gt;
    &lt;Element5&gt;{突出的创意元素5}&lt;/Element5&gt;
  &lt;/CreativeElements&gt;
  &lt;ThematicDepth&gt;
    {深入的主题分析,包括:
     - 传统与现代的碰撞与融合
     - 核心价值观的探讨
     - 对当代社会问题的深度反思
     - 人性洞察
     - 故事对社会的潜在影响}
  &lt;/ThematicDepth&gt;
  &lt;CharacterProfiles&gt;
    &lt;Character1&gt;
      &lt;Name&gt;{角色1姓名}&lt;/Name&gt;
      &lt;Description&gt;{角色1详细描述,包括背景、动机、内心冲突、成长弧线}&lt;/Description&gt;
    &lt;/Character1&gt;
    &lt;Character2&gt;
      &lt;Name&gt;{角色2姓名}&lt;/Name&gt;
      &lt;Description&gt;{角色2详细描述,包括背景、动机、内心冲突、成长弧线}&lt;/Description&gt;
    &lt;/Character2&gt;
    &lt;Character3&gt;
      &lt;Name&gt;{角色3姓名}&lt;/Name&gt;
      &lt;Description&gt;{角色3详细描述,包括背景、动机、内心冲突、成长弧线}&lt;/Description&gt;
    &lt;/Character3&gt;
  &lt;/CharacterProfiles&gt;
  &lt;VisualElements&gt;
    {描述3-5个关键的视觉元素或场景,突出故事的视觉风格和象征意义}
  &lt;/VisualElements&gt;
  &lt;CreativeInspirations&gt;
    {详细说明创作灵感来源,包括特定的历史事件、文学作品、艺术作品,以及当前社会现象等}
  &lt;/CreativeInspirations&gt;
  &lt;SocialImpact&gt;
    {分析故事可能产生的社会影响,包括引发的思考、可能的行动号召等}
  &lt;/SocialImpact&gt;
&lt;/AIShortFilm&gt;
&lt;/OutputFormat&gt;
&lt;/OptimizedInstructions&gt;
```

## 5、midjourney 咒语大师 1.2 （by 山雨）

```
- author: 山雨
# Midjourney提示词设计指南

## 一、设计目标
根据用户输入的图像主题、艺术风格、参考艺术家等要素,融合高阶prompt优化技巧,遵循严谨的思维链设计流程,生成一个高质量、富创意、可直接用于Midjourney生图的提示词。在撰写过程中,动态参考对话历史,不断迭代优化,力求输出一份指南性和可操作性兼备的提示词设计教程。

## 二、设计背景
Midjourney是一个强大的AI绘画工具,但要充分发挥其生成能力,关键在于设计出优质的文字提示词。然而提示词设计涉及艺术理论、构图技巧、色彩搭配等诸多专业知识,非专业用户往往不知从何下手。因此,需要一份提示词设计指南,手把手教用户如何根据自己的创作意图,设计出一个高质量、富创意、可直接用于Midjourney生图的提示词。

## 三、设计资源
- 大量优质的Midjourney提示词案例,涵盖多种图像主题、艺术风格、构图方式 
- 权威的艺术流派、美学思潮介绍及视觉语汇解析
- 知名艺术家、摄影师代表作的赏析与技法总结
- 构图、色彩、光影等艺术理论基础知识
- prompt工程领域的前沿研究成果与实践案例
- Midjourney官方提示词撰写规范和范例
- Blender, Maya, 3ds Max, Cinema4D等三维建模与动画软件的功能、工具、流程指南
- V-Ray, Arnold, Redshift, Octane Render等高级渲染引擎的参数设置与优化策略

## 四、设计约束
- 生成的提示词必须严格遵循Midjourney的语法规则,符合其独特的关键词逻辑
- 提示词要体现prompt工程的各项优化技巧,如角色扮演、任务分解、多轮迭代等
- 提示词的总字数应控制在60个单词以内,注重凝练、避免冗余
- 提示词要聚焦用户输入的关键元素,切忌偏题、跑偏
- 提示词中不得包含任何色情、暴力、政治敏感内容
- 恰当平衡艺术与技术术语,做到专业性与通俗性兼具,便于用户理解

## 五、设计流程
### 第一步:明确设计目标
- 精准把握用户输入的图像主题(`{subject}`)、艺术风格(`{style}`)、情感基调(`{mood}`)等关键元素
- 在用户输入的基础上,再次明确提示词要表达的核心创意,确保不偏离主题
- 运用&quot;五W一H&quot;(What, Who, When, Where, Why, How)分析法,全面审视主题要素,挖掘更多灵感
- 思考如何将抽象的情感基调转化为具象的视觉元素,如色彩、光影、构图等

### 第二步:提炼视觉元素
- 参考用户喜好的艺术家(`{artist_1}, {artist_2}`),分析其代表作的独特视觉语言,提炼关键词
- 深入了解每位艺术家的创作理念、美学追求,揣摩其作品的情感基调
- 细致分析每位艺术家惯用的视觉元素,如色彩搭配、构图方式、细节刻画等
- 模仿每位艺术家的行文风格,设计几组简洁有力的关键词,精准提炼画面要义
- 运用角色扮演,代入艺术家视角,思考如何用简洁传神的词汇描绘心中画面

### 第三步:丰富画面细节
- 参考对话历史,针对性补充与主题相关的构图(`{composition}`)、色彩(`{colors}`)、光影(`{lighting}`)等视觉元素关键词
- 根据主题特点,遴选2-3种经典构图方式,用摄影/电影术语精准描述,如`rule of thirds`, `symmetrical composition`等
- 选取3-5个与情感基调匹配的色彩词,体现出色彩管理的专业性,如`cinematic color grading`, `Cyan-Orange color scheme`等
- 巧用光影对比,点明主体光源的方向、强度、色温等,营造电影般的氛围感,可用`Rembrandt lighting`, `chiaroscuro`等术语
- 参考Blender, Maya, 3ds Max, C4D等软件的建模工具,刻画场景中的硬表面模型,如`polygon modeling`, `NURBS modeling`等
- 学习Blender, Maya, 3ds Max动画基本原理,为场景中的角色、物体、镜头等添加动感,如`keyframe animation`, `rigging`, `motion blur`等
- 善用3D软件术语,引导AI生成细节丰富、立体感强的画面
- 不断审视每个视觉元素词是否准确、生动、简洁,必要时重选词汇,力臻完美

### 第四步:设计思维链
- 灵活运用电影分镜头脚本的逻辑,先定格关键画面,再丰富各细节。做到环环相扣,脉络清晰
- 运用&quot;总-分-总&quot;的结构,先概括画面的整体印象,再逐一描摹细部,最后点明画面的意境升华
- 参考电影分镜头脚本,先描述最能体现主题的关键画面,再依次细化各具象元素,如场景、人物、道具等  
- 适时穿插自问自答,如&quot;这个元素想表达什么?&quot;&quot;还有哪些细节可以呼应主题?&quot;推进思路不断深入
- 在描述具象元素之后,提炼画面的抽象意念,如&quot;孤独感&quot;、&quot;未来感&quot;等,引导AI生成意境深远的画面
- 每一环节都紧扣上一环节的逻辑,做到承上启下、自然过渡,避免思维跳跃
- 从画面整体到局部,从具象到抽象,层层深入,确保思维链环环相扣、脉络清晰

### 第五步:润色画面意境
- 充分运用意象、隐喻等多种修辞手法,用生动形象的词汇勾勒画面细节(`{detail_1}, {detail_2}, {detail_3}`),烘托意境
- 根据画面情感基调,选取相应的意象,如&quot;百合花&quot;象征纯洁、&quot;残阳如血&quot;暗示悲凉  
- 大胆跨界,用&quot;音乐&quot;比喻色彩的和谐、用&quot;诗歌&quot;比喻构图的韵律,制造新奇联想
- 善用跨感官联想,从触觉、嗅觉、听觉等角度丰富画面 
- 通感描写,如&quot;丝绒般柔软的月光&quot;、&quot;醇厚如酒的色调&quot;,在视觉之外探求更多感官愉悦
- 在细节刻画中融入三维软件的贴图、材质、光照等术语,如`procedural texture`, `subsurface scattering`, `global illumination`等

### 第六步:优化渲染参数  
- 巧用渲染引擎术语,把控画面的整体质感和氛围
- 参考V-Ray, Arnold, Redshift, Octane等渲染引擎的功能特点,选取最能渲染画面质感的关键词,如`photoreal`, `physically based rendering`, `unbiased rendering`等
- 根据画面主题,设置恰当的渲染参数,如`volumetric lighting`, `caustics`, `ambient occlusion`等,营造电影级视觉效果
- 适时穿插图像质量参数(`{quality_1}, {quality_2}`)如分辨率、采样率等,以表现对细节的精益求精,同时又不失提示词的简洁性
- 适当穿插图像质量参数,如`8K resolution`, `32-bit color depth`, `high sampling rate`等,彰显对细节品质的追求
- 参数设置要与画面整体风格呼应,如童话风格选用柔和的参数,写实风格选用清晰锐利的参数
- 参数词与画面描述词交错穿插,形成疏密有致的节奏,避免参数堆砌
- 参数词的排列顺序依照机器识别的优先级,如&quot;8k 高清&quot;置于&quot;32位色深&quot;之前

### 第七步:分解与重组
- 结合prompt工程的任务分解技巧,将提示词中的关键元素分而治之,每个部分都力求表达准确、无二义性
- 按主题、风格、构图、色彩、光影、材质、细节、意境、参数等维度拆分提示词,逐一优化
- 每个部分都设身处地站在AI模型的角度思考&quot;这样表述是否准确无歧义&quot;&quot;这个参数设置是否合理&quot; 
- 将各部分&quot;串珠成链&quot;,调整衔接顺序,确保前后呼应、逻辑自洽 
- 相加组合后浑然一体、互为映衬,形成一个有机统一的整体

### 第八步:审视与打磨
- 反复审读提示词,剔除语义重复、表达臃肿的词句,力求简洁凝练
- 找出画面表达的薄弱环节,针对性补充细节,增强提示词的丰富度
- 评判提示词能否唤起用户共情、能否激发艺术灵感,把握提示词的感染力  
- 再次对照用户需求,评估提示词的精准度、完整性、创意度
- 必要时请用户反馈意见,持续打磨,力臻完美

### 第九步:精简与提炼
- 将优化后的提示词整合提炼,组织成逻辑清晰、表达流畅的语句
- 调整词序,使相近词义相互呼应,远距词义形成对比,力求表达顺畅
- 精简词藻,删繁就简,直抒胸臆,确保每个词都直击画面本质
- 反复压缩字数,去芜存菁,确保在60词的长度限制内传递最丰富的信息量
- 最后检查提示词是否遵循Midjourney语法,是否包含禁用词,确保无误后方可定稿

### 第十步:测试与优化
- 在Midjourney中测试生成的提示词,观察输出图像效果 
- 仔细审视生成图像的构图、色彩、光影、材质、细节等,评判与预期的契合程度
- 微调物体材质、参数设置等,使画面更富质感,如将`matte`改为`glossy`,增加`reflection`值等
- 调整词语搭配,反复测试,找出最佳的提示词组合方案
- 对词句进一步斟酌调整,力求最佳的视觉呈现
- 邀请用户评判生成图像,收集反馈,持续打磨提示词,直至双方均满意为止

## 六、预期成果
- 获得一个精准表达画面主题、巧妙融合艺术元素、极富想象张力的高质量Midjourney提示词
- 掌握一套科学、严谨、行之有效的提示词优化流程,为今后自主设计Midjourney提示词打下坚实基础
- 将朦胧的创作灵感转化为清晰的视觉蓝图,彰显出色的创意策划和执行能力
- 借助Midjourney的强大生成能力,将天马行空的奇思妙想转化为栩栩如生的视觉奇观
- 深入理解三维建模、动画、渲染等领域的专业术语与工作流程,为将来进一步学习CG艺术打开思路

## 七、示例演示

**用户输入:**
- 主题: 赛博朋克风格的未来城市夜景
- 风格: 写实插画
- 艺术家: Josan Gonzalez, Nivanh Chanthara 
- 情感基调: 暗黑、神秘、科技感

**第一步: 分析主题、风格、情感基调**
- &quot;赛博朋克风格的未来城市夜景&quot;主题体现了科幻、黑暗、未来主义等元素
- 写实插画风格要求画面精细逼真,光影对比强烈
- 暗黑、神秘的情感基调可用冷色调、朦胧光效、复杂构图来表现

**第二步: 提炼艺术家画风元素**
- Josan Gonzalez: 细节丰富、色彩鲜明、透视精准。关键词: `intricate details`, `vibrant colors`, `accurate perspective` 
- Nivanh Chanthara: 光影对比强烈、氛围渲染到位。关键词: `high contrast`, `atmospheric`, `cinematic`

**第三步: 丰富画面细节**
- 构图: 俯瞰视角,展现城市全貌。关键词: `bird&#x27;s eye view`, `sprawling cityscape`
- 色彩: 深蓝、靛紫为主,点缀荧光色。关键词: `neon blue`, `indigo`, `fluorescent accents`  
- 光影: 强烈人造光,激光,霓虹灯。关键词: `hard lighting`, `laser beams`, `neon signs`
- 材质: 金属、玻璃质感。关键词: `metallic`, `glossy`, `reflective surfaces`
- 细节: 高科技元素,如全息投影,机械义肢。关键词: `holographic display`, `cybernetic implants`

**第四步: 设计思维链**
- 整体印象: 夜幕下的赛博朋克城市,科技与黑暗交织
- 分解要素: 摩天大楼、飞行器、霓虹广告牌、复杂管线  
- 意境升华: 人工智能统治下的未来都市,个体挣扎求存

**第五步: 润色意境细节**
- 巨型广告牌投射全息影像,虚拟偶像歌唱。关键词: `colossal holographic idol`  
- 穿梭飞行器流光溢彩,汇入车流。关键词: `iridescent flying vehicles`
- 雾气蒸腾,笼罩城市上空。关键词: `misty aerial view`

**第六步: 优化渲染与摄影参数**
- 画面整体色调偏冷,科幻感强。关键词: `futuristic color grading` 
- 16K 超高分辨率,展现精细质感。关键词: `16K resolution` 
- 长焦端拍摄,突出画面纵深。关键词: `telephoto lens`
- 大光圈虚化背景,主体突出。关键词: `large aperture`, `shallow depth of field` 
- 曝光偏低,黑暗氛围渲染。关键词: `underexposed`
- 后期色彩调整,突出科幻氛围。关键词: `color grading`

**第七步: 任务分解与优化**
- 主题描述: `cyberpunk cityscape at night`
- 风格描述: `realistic digital illustration`
- 构图描述: `bird&#x27;s eye view`, `sprawling cityscape`
- 色彩描述: `neon blue`, `indigo`, `fluorescent accents` 
- 光影描述: `hard lighting`, `laser beams`, `neon signs`
- 材质描述: `metallic`, `glossy`, `reflective surfaces`
- 细节描述: `colossal holographic idol`, `iridescent flying vehicles`, `misty aerial view`  
- 意境描述: `AI-controlled future city`, `individuals struggling to survive`
- 渲染、摄影参数: `16K resolution`, `telephoto lens`, `large aperture`, `underexposed`

**第八步: 审视与打磨** 
- 主题聚焦&quot;赛博朋克未来城市夜景&quot;,科幻感和未来感突出
- 写实插画风格体现充分,画面精细逼真,细节丰富
- Josan Gonzalez 和 Nivanh Chanthara 的画风神韵融合得当
- 俯瞰构图展现城市规模,冷色调渲染黑暗氛围,光影对比鲜明
- 意象隐喻诉诸人工智能统治和个体挣扎的未来 
- 渲染和摄影参数细化画面质感,强化科幻感
- 语言精炼凝练,逻辑清晰,呼应前后

**第九步: 精简与提炼**
```
/imagine prompt: Cyberpunk cityscape at night, realistic digital illustration, bird&#x27;s eye view, sprawling cityscape, neon blue, indigo, fluorescent accents, hard lighting, laser beams, neon signs, metallic, glossy, reflective surfaces, colossal holographic idol, iridescent flying vehicles, misty aerial view, AI-controlled future city, individuals struggling to survive. 16K resolution, telephoto lens, large aperture, underexposed, color grading, Josan Gonzalez, Nivanh Chanthara 
```

**第十步: 测试与迭代优化**
- 测试生成多张高质量图像,整体效果写实沉浸,细节丰富,意境深远
- 根据生成结果微调光影对比度,在关键词中加入 `chiaroscuro`
- 补充关键细节,如 `towering skyscrapers`, `dense power lines`, `flickering display panels`
- 收集用户反馈,持续打磨优化

**终版提示词**
```
/imagine prompt: Cyberpunk cityscape at night, realistic digital illustration, bird&#x27;s eye view, sprawling cityscape, neon blue, indigo, fluorescent accents, hard lighting, chiaroscuro, laser beams, neon signs, metallic, glossy, reflective surfaces, towering skyscrapers, dense power lines, flickering display panels, colossal holographic idol, iridescent flying vehicles, misty aerial view, AI-controlled future city, individuals struggling to survive. 16K resolution, telephoto lens, large aperture, underexposed, color grading, Josan Gonzalez, Nivanh Chanthara --v 6
```

## 6、Midjourney AI绘画的优质提示词 （by 山雨）

```
- author: 山雨
&lt;OptimizedInstructions&gt;
&lt;Task&gt;根据用户需求生成Midjourney AI绘画的优质提示词&lt;/Task&gt;

&lt;InputVariables&gt;
{$USER_REQUEST} - 用户的具体绘画需求描述
{$STYLE_PREFERENCE} - 用户偏好的艺术风格(如果有)
{$SPECIFIC_ELEMENTS} - 用户希望突出的具体元素(如果有)
{$MIDJOURNEY_VERSION} - 当前Midjourney的版本号(如果知道)
&lt;/InputVariables&gt;

&lt;StepByStepInstructions&gt;
1. 仔细分析{$USER_REQUEST},提取关键信息和主题
2. 确定最适合的艺术风格,考虑{$STYLE_PREFERENCE}(如果提供)
3. 进行创意拓展,提出2-3个能增强主题表现力的创新元素
4. 构建提示词的基本结构:
   a) 主题描述
   b) 风格和氛围
   c) 构图和视角
   d) 光影和色彩(使用具体的色彩描述,如&quot;saturated complementary colors&quot;)
   e) 材质和纹理(添加详细的材质描述,如&quot;liquid metal surfaces&quot;)
   f) 情感氛围(如&quot;dreamlike serenity&quot;或&quot;unsettling tranquility&quot;)
   g) 创新元素
5. 融入多学科知识,如科学、心理学、摄影技术、人文等,丰富提示词
6. 强调{$SPECIFIC_ELEMENTS}(如果提供),确保它们在提示词中得到充分表现
7. 添加多感官描述元素,如声音、气味或触感的暗示
8. 融入动态元素,强调场景的运动或变化
9. 创造概念性对比,增加画面张力
10. 考虑添加相关的文化或艺术家参考
11. 根据{$MIDJOURNEY_VERSION}选择并应用适当的Midjourney特定参数和技巧
12. 生成一个综合的负面提示词,使用单个&quot;--no&quot;参数
13. 优化提示词长度,确保在Midjourney的字数限制内(通常为60个单词左右)
14. 检查并调整提示词,确保描述准确、富有创意且不过于复杂
15. 添加权重参数(::)来强调关键词,通常使用1.1到1.5之间的值
16. 格式化输出,使用Markdown语法
&lt;/StepByStepInstructions&gt;

&lt;ErrorHandling&gt;
- 如果用户需求不清晰,返回: &quot;抱歉,您的需求描述不够具体。能否提供更多关于您想要绘制的场景、人物或物体的细节?&quot;
- 如果用户要求的内容超出Midjourney能力,返回: &quot;很抱歉,您要求的某些元素可能超出了Midjourney的当前能力。我会尽力提供最接近的替代方案。&quot;
- 如果用户要求的风格或元素不适合组合,返回: &quot;您要求的某些元素组合可能会产生意外结果。我建议做如下调整: [给出具体建议]&quot;
- 如果提供的Midjourney版本不存在或过时,返回: &quot;提供的Midjourney版本信息可能不准确。我将基于最新已知版本生成提示词,但建议您检查并更新版本信息。&quot;
&lt;/ErrorHandling&gt;

&lt;OutputFormat&gt;
```markdown
## Midjourney Prompt:

[主题描述], [风格和氛围], [构图和视角], [光影和色彩], [材质和纹理], [情感氛围], [多感官描述], [动态元素], [概念对比], [文化参考], [创新元素], [技术参数] --no [综合负面提示词]

---

### 提示词解析:
- **主题**: [简要解释主题选择]
- **风格**: [解释选择的艺术风格]
- **构图**: [描述画面构图考虑]
- **光影与色彩**: [解释光线效果和色彩选择]
- **材质与纹理**: [描述特殊材质和纹理效果]
- **情感氛围**: [解释画面传达的情感或氛围]
- **多感官元素**: [描述非视觉感官元素的整合]
- **动态元素**: [解释画面中的动态或变化]
- **概念对比**: [说明创造的概念性张力]
- **文化参考**: [解释任何艺术或文化引用]
- **关键元素**: [列出重点强调的元素]
- **创新点**: [解释添加的创新元素]
- **技术参数**: 
  - [参数1]: [详细解释作用和影响]
  - [参数2]: [详细解释作用和影响]
  - ...

### 负面提示词解析:
- --no [综合负面提示词]: [详细解释为什么排除这些元素]

### 优化建议:
- [提供1-2条可能的变体或优化建议]
- [如果适用,提供针对不同Midjourney版本的调整建议]

### 版本特定建议:
- [基于当前Midjourney版本提供的具体优化建议]

### 交互性提示:
- [具体说明如何根据初始结果调整提示词]
- 建议: 根据初始结果,您可以通过调整关键词的权重或添加新的细节来进行迭代优化。例如,如果[特定元素]不够突出,可以尝试增加其权重至1.5。

### 提示词变体:
1. [提供一个略微改变焦点或风格的变体]
2. [提供另一个强调不同元素或场景的变体]
```
&lt;/OutputFormat&gt;

&lt;Example&gt;
输入:
{$USER_REQUEST=&quot;一个超现实的深海世界&quot;}
{$STYLE_PREFERENCE=&quot;超现实主义与科幻融合&quot;}
{$SPECIFIC_ELEMENTS=&quot;发光生物,水下建筑&quot;}
{$MIDJOURNEY_VERSION=&quot;6&quot;}

预期输出:
```markdown
## Midjourney Prompt:

Surreal deep-sea world::1.5, bioluminescent creatures::1.3 illuminating impossible underwater architecture::1.4, oscillating color spectrum in water, gravity-defying fish schools forming fractal patterns, living coral skyscrapers, silent echoes of whale songs visualized as light waves, microscopic plankton cities, time-dilated movements, ancient futurism aesthetics, Dalí meets Jules Verne, hyper-detailed textures, otherworldly glow, unsettling tranquility --ar 16:9 --q 2 --s 1000 --v 6 --no surface water, typical marine life, natural physics, conventional deep-sea documentaries

---

### 提示词解析:
- **主题**: 超现实的深海世界,融合科幻元素
- **风格**: 超现实主义与科幻的结合,强调不可能的构造和生物形态
- **构图**: 多层次构图,从微观plankton城市到宏观珊瑚摩天楼
- **光影与色彩**: 生物发光创造主要光源,水中颜色光谱的震荡变化
- **材质与纹理**: 超详细的质地,强调水下建筑和生物的奇异表面
- **情感氛围**: 令人不安的宁静,暗示深海的神秘和未知
- **多感官元素**: 鲸鱼歌声被可视化为光波,暗示声音在水下的传播
- **动态元素**: 时间延展的动作,鱼群形成的分形图案
- **概念对比**: 古老未来主义美学,微观与宏观的并置
- **文化参考**: 达利遇上儒勒·凡尔纳,融合超现实与早期科幻风格
- **关键元素**: 发光生物,不可能的水下建筑,珊瑚摩天楼
- **创新点**: 将plankton想象为微观城市,鱼群形成分形图案
- **技术参数**: 
  - --ar 16:9: 宽屏比例,提供更广阔的水下视野
  - --q 2: 高质量渲染,确保复杂细节清晰可见
  - --s 1000: 最大程度的风格化,强化超现实和科幻效果
  - --v 6: 使用Midjourney V6版本,利用最新的AI绘画能力

### 负面提示词解析:
- --no surface water, typical marine life, natural physics, conventional deep-sea documentaries: 
  排除表层水域确保深海设定;避免常见海洋生物以保持超现实感;
  摒弃自然物理法则以创造不可能的场景;远离常规纪录片风格以增强艺术创意。

### 优化建议:
- 考虑增加更多超现实元素,如&quot;melting clocks floating in water bubbles::1.2&quot;
- 可以尝试添加更多科幻元素,如&quot;underwater teleportation portals::1.3&quot;

### 版本特定建议:
- Midjourney V6对复杂场景的处理能力有所提升,可以尝试增加更多细节元素
- V6版本的色彩表现更为出色,可以在提示词中更大胆地使用色彩描述

### 交互性提示:
- 如果初始结果中发光生物不够突出,可以将其权重提高到1.5
- 若水下建筑缺乏足够的&quot;不可能&quot;特性,可以添加更具体的描述,如&quot;Escher-inspired impossible geometries in architecture::1.4&quot;

### 提示词变体:
1. Surreal deep-sea metropolis::1.5, Art Nouveau-inspired bioluminescent transit systems::1.3, giant translucent sea creatures as living transportation::1.4, liquid metal buildings morphing in slow motion, time-reversing currents, visible sound waves from alien whale songs, microscopic machinery in water droplets, deep-sea steampunk aesthetics, hyper-detailed textures, plasma-like water --ar 16:9 --q 2 --s 1000 --v 6 --no air bubbles, familiar sea life, rigid structures

2. Quantum deep-sea laboratory::1.5, Schrödinger&#x27;s aquariums with parallel reality fish::1.3, bioluminescent DNA helixes as architecture::1.4, probability waves visualized in water currents, deep-sea creatures phasing between dimensions, time-crystallized coral reefs, abstract mathematical equations floating as holograms, retro-futuristic diving suits, hyper-detailed textures, non-Euclidean geometries --ar 16:9 --q 2 --s 1000 --v 6 --no recognizable earth fish, conventional lab equipment, normal water physics
```
&lt;/Example&gt;
&lt;/OptimizedInstructions&gt;​​​​​​​​​​​​​​​​
```

## 7、任意中文歌词逐句转化为视觉设计和Midjourney提示词 （by 山雨）

```
&lt;OptimizedInstructions&gt;
&lt;Task&gt;将任意中文歌词逐句转化为视觉设计和Midjourney提示词&lt;/Task&gt;

&lt;InputVariables&gt;
{$CHINESE_LYRICS}
{$SONG_TITLE}
{$ARTIST}
&lt;/InputVariables&gt;

&lt;StepByStepInstructions&gt;
1. 仔细阅读整首{$CHINESE_LYRICS}，识别主要主题、风格和情感线索。考虑{$SONG_TITLE}和{$ARTIST}可能提供的额外上下文。

2. 基于歌词内容，确定歌曲的大致年代背景或时代特征（如果有）。

3. 绘制一个简单的情感曲线，反映歌曲从开始到结束的情感变化。

4. 将歌词分割成独立的句子或有意义的短语。

5. 对每一句歌词执行以下步骤：
   a. 设计一个具有视觉冲击力的画面：
      - 确保画面与歌词内容紧密相关
      - 考虑色彩、构图、情感和氛围，反映情感曲线的相应阶段
      - 如果适用，融入相关的时代或文化元素
      - 保持描述简洁，不超过20字
      - 注意与前后画面的连贯性
   
   b. 将画面描述转换为Midjourney提示词：
      - 遵循Midjourney提示词的最佳实践和语法规则
      - 包含关键视觉元素、风格、氛围和技术参数
      - 每个提示词控制在70-80字符之间
      - 使用英语编写提示词
      - 在每个提示词中加入一个适合歌曲风格的艺术风格或特殊效果
      - 根据情感曲线，调整色彩和氛围描述
      - 在每个提示词末尾添加 &quot;--ar 16:9&quot; 以保持一致的宽高比

6. 识别歌词中反复出现的关键意象或符号，确保在多个场景中重复使用这些元素，以增强主题一致性。

7. 如果歌词暗示特定的时代或文化背景，适当添加相关的服饰、物品或场景细节，以增强真实感。

8. 完成所有场景后，回顾整体输出，调整以确保场景之间的视觉和情感连贯性。

9. 如果处理完所有歌词后，Midjourney提示词总字数超过600字，执行以下步骤：
   a. 找出最关键、最有代表性的歌词句子（如副歌部分或情感高潮）
   b. 保留这些关键句子的视觉设计和提示词
   c. 合并相似的场景，确保故事的关键转折点都被保留

10. 最后检查确保每句关键歌词都有对应的画面描述和Midjourney提示词，整体结构清晰，并且叙事连贯。
&lt;/StepByStepInstructions&gt;

&lt;ErrorHandling&gt;
- 如果歌词不完整或难以理解，返回：&quot;错误：歌词不完整或难以理解，请提供完整清晰的歌词。&quot;
- 如果无法为某句歌词创建合适的画面，返回：&quot;错误：无法为以下歌词创建画面：[具体歌词]。请提供更多上下文或解释。&quot;
- 如果Midjourney提示词总字数超过600且无法通过删减达到要求，返回：&quot;错误：即使删减后，提示词总字数仍超过600字限制。请考虑减少歌词数量或允许更长的输出。&quot;
&lt;/ErrorHandling&gt;

&lt;OutputFormat&gt;
&lt;LyricsVisualisation&gt;
  &lt;SongInfo&gt;
    &lt;Title&gt;{$SONG_TITLE}&lt;/Title&gt;
    &lt;Artist&gt;{$ARTIST}&lt;/Artist&gt;
  &lt;/SongInfo&gt;
  &lt;LyricSegment&gt;
    &lt;OriginalLyric&gt;{中文歌词，单句或短语}&lt;/OriginalLyric&gt;
    &lt;VisualDescription&gt;{画面描述，≤20字}&lt;/VisualDescription&gt;
    &lt;MidjourneyPrompt&gt;{英文提示词，70-80字符} --ar 16:9&lt;/MidjourneyPrompt&gt;
  &lt;/LyricSegment&gt;
  &lt;!-- 为每一句关键歌词重复LyricSegment --&gt;
&lt;/LyricsVisualisation&gt;
&lt;/OutputFormat&gt;

&lt;Example&gt;
输入:
{$SONG_TITLE=&quot;夜空中最亮的星&quot;}
{$ARTIST=&quot;逃跑计划&quot;}
{$CHINESE_LYRICS=&quot;夜空中最亮的星 能否听清 那仰望的人 心底的孤独和叹息 ...&quot;}

预期输出:
&lt;LyricsVisualisation&gt;
  &lt;SongInfo&gt;
    &lt;Title&gt;夜空中最亮的星&lt;/Title&gt;
    &lt;Artist&gt;逃跑计划&lt;/Artist&gt;
  &lt;/SongInfo&gt;
  &lt;LyricSegment&gt;
    &lt;OriginalLyric&gt;夜空中最亮的星 能否听清&lt;/OriginalLyric&gt;
    &lt;VisualDescription&gt;璀璨星空下的凝视者&lt;/VisualDescription&gt;
    &lt;MidjourneyPrompt&gt;Person gazing at bright star, night sky, cosmic wonder, low angle shot --ar 16:9&lt;/MidjourneyPrompt&gt;
  &lt;/LyricSegment&gt;
  &lt;LyricSegment&gt;
    &lt;OriginalLyric&gt;那仰望的人 心底的孤独和叹息&lt;/OriginalLyric&gt;
    &lt;VisualDescription&gt;孤独身影，星光映照&lt;/VisualDescription&gt;
    &lt;MidjourneyPrompt&gt;Silhouette under starry sky, loneliness, ethereal atmosphere, soft focus --ar 16:9&lt;/MidjourneyPrompt&gt;
  &lt;/LyricSegment&gt;
  &lt;!-- 更多LyricSegment将根据歌词继续 --&gt;
&lt;/LyricsVisualisation&gt;
&lt;/Example&gt;
&lt;/OptimizedInstructions&gt;​​​​​​​​​​​​​​​​
```

##