
# 文章风格提取器（文风抽取）

> 来源：[https://vxc3hj17dym.feishu.cn/docx/M9d8d9yxvoYEQ0xou6zcQONfnme](https://vxc3hj17dym.feishu.cn/docx/M9d8d9yxvoYEQ0xou6zcQONfnme)

> 作者：[小七姐](https://vxc3hj17dym.feishu.cn/wiki/RxnSw84QIiC63KkBrofc2altnuf?from=from_copylink)

> [**prompt 学习社群**](https://t.zsxq.com/15KhQ0RE7)

> [提示词图书馆](https://vxc3hj17dym.feishu.cn/wiki/VDb1wMKDNiNj0mkJn6VcFgRenVc?from=from_copylink)

tag：文风 抽取

该提示词用于抽取不同风格文章的核心要素，抽取到的字段可以作为prompt，结合需要创作的指定主题进行风格迁移。（提示词整体创作思路见文末PDF）

**大白话：**用鲁迅、史铁生、汪曾祺的风格写一篇你需要的命题作文。

具体使用：拷贝提示词，输入给任意大模型，随后提供要抽取的文本。类似于：

```
为我分析以下文本风格：
&quot;&quot;&quot;
(这里是要分析的文本内容）
&quot;&quot;&quot;
```

**在抽取到所需要的文章风格后，输入提示词，在末尾加你的写作要求，生成该风格的新文章**：

```
基于上述风格为我生成一篇（这里提供你要创作的内容主题）文章。
要求：
1.
2.
3. 
```

没看懂具体使用方法的小伙伴可以评论区留言。

## 文章风格提取（核心提示词）

https://monica.im/share/bot?botId=1eOLOpnm Monica智能体

```
# 文章风格分析器 v1.0
请用户输入想要分析的文本段落。你将对其进行深度风格解析，并以结构化格式输出分析结果。
## 分析维度
你将从以下维度分析文本风格特征：
1. 语言特征（句式、用词、修辞）
2. 结构特征（段落、过渡、层次）
3. 叙事特征（视角、距离、时序）
4. 情感特征（浓淡、方式、基调）
5. 思维特征（逻辑、深度、节奏）
6. 个性标记（独特表达、意象系统）
7. 文化底蕴（典故、知识领域）
8. 韵律节奏（音节、停顿、节奏）
## 输出格式
你将以下列结构化格式以代码块输出分析结果：
```json

{

    &quot;style_summary&quot;: &quot;风格一句话概括&quot;,

    &quot;language&quot;: {

        &quot;sentence_pattern&quot;: [&quot;主要句式特征&quot;, &quot;次要句式特征&quot;],

        &quot;word_choice&quot;: {

            &quot;formality_level&quot;: &quot;正式度 1-5&quot;,

            &quot;preferred_words&quot;: [&quot;高频特征词1&quot;, &quot;特征词2&quot;],

            &quot;avoided_words&quot;: [&quot;规避词类1&quot;, &quot;规避词类2&quot;]

        },

        &quot;rhetoric&quot;: [&quot;主要修辞手法1&quot;, &quot;修辞手法2&quot;]

    },

    &quot;structure&quot;: {

        &quot;transition_style&quot;: &quot;过渡特征&quot;,

        &quot;hierarchy_pattern&quot;: &quot;层次展开方式&quot;

    },

    &quot;narrative&quot;: {

        &quot;perspective&quot;: &quot;叙事视角&quot;,

        &quot;time_sequence&quot;: &quot;时间处理方式&quot;,

        &quot;narrator_attitude&quot;: &quot;叙事态度&quot;

    },

    &quot;emotion&quot;: {

        &quot;intensity&quot;: &quot;情感强度 1-5&quot;,

        &quot;expression_style&quot;: &quot;表达方式&quot;,

        &quot;tone&quot;: &quot;情感基调&quot;

    },

    &quot;thinking&quot;: {

        &quot;logic_pattern&quot;: &quot;思维推进方式&quot;,

        &quot;depth&quot;: &quot;思维深度 1-5&quot;,

        &quot;rhythm&quot;: &quot;思维节奏特征&quot;

    },

    &quot;uniqueness&quot;: {

        &quot;signature_phrases&quot;: [&quot;标志性表达1&quot;, &quot;表达2&quot;],

        &quot;imagery_system&quot;: [&quot;核心意象1&quot;, &quot;意象2&quot;]

    },

    &quot;cultural&quot;: {

        &quot;allusions&quot;: [&quot;典故类型&quot;, &quot;使用频率&quot;],

        &quot;knowledge_domains&quot;: [&quot;涉及领域1&quot;, &quot;领域2&quot;]

    },

    &quot;rhythm&quot;: {

        &quot;syllable_pattern&quot;: &quot;音节特征&quot;,

        &quot;pause_pattern&quot;: &quot;停顿规律&quot;,

        &quot;tempo&quot;: &quot;节奏特征&quot;

    }

}

```
## 注意：
1. 绝不提取文中提及的特殊要素，例如书名、作者姓名、特定地理位置等。
2. 风格提取的目的在于基于该风格生成其他指定主题的文章，提取要素应当基于这一任务进行。
```

## 抽取效果

![](img/0328d7f9b7b61c0851c2bebe239ef5e5.png)

![](img/49795593bd637651a74214694b2ab461.png)

![](img/f6ae86b66753701d4e5106c69460385e.png)

## 已抽取的一些风格参考

### 万维钢风格

```
{
    &quot;style_summary&quot;: &quot;知识分享类科普文风，以轻松对话式的语气传递学术观点，善用口语化表达和具体案例&quot;,
    &quot;language&quot;: {
        &quot;sentence_pattern&quot;: [&quot;口语化短句为主&quot;, &quot;设问句+自答模式&quot;],
        &quot;word_choice&quot;: {
            &quot;formality_level&quot;: &quot;3&quot;,
            &quot;preferred_words&quot;: [&quot;咱们&quot;, &quot;说白了&quot;, &quot;你看&quot;, &quot;那个&quot;, &quot;这么个&quot;],
            &quot;avoided_words&quot;: [&quot;学术术语&quot;, &quot;书面化表达&quot;]
        },
        &quot;rhetoric&quot;: [&quot;设问&quot;, &quot;打比方&quot;, &quot;举例论证&quot;]
    },
    &quot;structure&quot;: {
        &quot;paragraph_length&quot;: &quot;短段落为主，2-3句话一段&quot;,
        &quot;transition_style&quot;: &quot;自然流畅，常用&#x27;那么&#x27;、&#x27;所以&#x27;等口语化连接词&quot;,
        &quot;hierarchy_pattern&quot;: &quot;先抛出观点-解释-举例-延伸讨论&quot;
    },
    &quot;narrative&quot;: {
        &quot;perspective&quot;: &quot;第一人称+对话式&quot;,
        &quot;time_sequence&quot;: &quot;线性推进&quot;,
        &quot;narrator_attitude&quot;: &quot;亲和、平等的知识分享者&quot;
    },
    &quot;emotion&quot;: {
        &quot;intensity&quot;: &quot;2&quot;,
        &quot;expression_style&quot;: &quot;平和而亲切&quot;,
        &quot;tone&quot;: &quot;轻松愉快的学习氛围&quot;
    },
    &quot;thinking&quot;: {
        &quot;logic_pattern&quot;: &quot;归纳式+举例式&quot;,
        &quot;depth&quot;: &quot;4&quot;,
        &quot;rhythm&quot;: &quot;快速切入，节奏轻快&quot;
    },
    &quot;uniqueness&quot;: {
        &quot;signature_phrases&quot;: [&quot;说白了&quot;, &quot;你看&quot;, &quot;举个例子&quot;],
        &quot;imagery_system&quot;: [&quot;生活化比喻&quot;, &quot;具体案例&quot;]
    },
    &quot;cultural&quot;: {
        &quot;allusions&quot;: [&quot;现代案例为主&quot;, &quot;频率适中&quot;],
        &quot;knowledge_domains&quot;: [&quot;文化研究&quot;, &quot;社会学&quot;, &quot;跨文化交际&quot;]
    },
    &quot;rhythm&quot;: {
        &quot;syllable_pattern&quot;: &quot;口语化节奏&quot;,
        &quot;pause_pattern&quot;: &quot;自然停顿，类似日常对话&quot;,
        &quot;tempo&quot;: &quot;中等偏快，富有节奏感&quot;
    }
}
以上述风格为我改写upload中的文本内容，不要设置标题，不要结构化，仅以自然段落输出。
注意：确保生成内容和原文主旨的一致性，不要乱用举例以至于失去原文本意，注意概念映射的正确性。
```

### 文风抽取 - 史铁生 -《我与地坛》

```
{
    &quot;style_summary&quot;: &quot;深沉内省的散文风格，融合哲思、抒情与叙事，以朴实语言探讨生命意义与存在价值&quot;,
    &quot;language&quot;: {
        &quot;sentence_pattern&quot;: [&quot;绵长复合句与短句错落句式&quot;, &quot;富有节奏感的排比句&quot;],
        &quot;word_choice&quot;: {
            &quot;formality_level&quot;: &quot;4&quot;,
            &quot;preferred_words&quot;: [&quot;苍凉&quot;, &quot;沉郁&quot;, &quot;寂寥&quot;, &quot;镇静&quot;, &quot;幽静&quot;, &quot;熨帖&quot;, &quot;苍幽&quot;],
            &quot;avoided_words&quot;: [&quot;口语化表达&quot;, &quot;网络流行语&quot;, &quot;过于直白的情感词&quot;]
        },
        &quot;rhetoric&quot;: [&quot;时空蒙太奇&quot;, &quot;物我同构&quot;]
    },
    &quot;structure&quot;: {
        &quot;paragraph_length&quot;: [&quot;中等偏长&quot;, &quot;禁止以平均字数生成工整段落，穿插一句话作为段落&quot;],
        &quot;transition_style&quot;: &quot;意象过渡与时空转换相结合，自然流畅&quot;,
        &quot;hierarchy_pattern&quot;: &quot;以空间为线索，内外景交替，由具体场景引发哲思&quot;
    },
    &quot;narrative&quot;: {
        &quot;perspective&quot;: &quot;第一人称独白与自我对话&quot;,
        &quot;time_sequence&quot;: &quot;现在与回忆交错，时空跨度大&quot;,
        &quot;narrator_attitude&quot;: &quot;冷静观察与深情思考并存，自省而不自怜&quot;
    },
    &quot;emotion&quot;: {
        &quot;intensity&quot;: &quot;3&quot;,
        &quot;expression_style&quot;: [&quot;物哀式抒情&quot;, &quot;含蓄内敛，以景物与人物侧面烘托情感&quot;],
        &quot;tone&quot;: &quot;平静中带着忧伤，哲思中蕴含温情&quot;
    },
    &quot;thinking&quot;: {
        &quot;logic_pattern&quot;: &quot;由具体场景引发哲理思考，螺旋式深入&quot;,
        &quot;depth&quot;: &quot;5&quot;,
        &quot;rhythm&quot;: &quot;缓慢而深沉，如同在园中漫步般的思考节奏&quot;
    },
    &quot;uniqueness&quot;: {
        &quot;signature_phrases&quot;: [&quot;人真正的名字叫欲望&quot;, &quot;死是一件不必着急的事&quot;],
        &quot;imagery_system&quot;: [&quot;古园&quot;, &quot;落叶&quot;, &quot;老树&quot;, &quot;轮椅&quot;, &quot;光影&quot;, &quot;四季更替&quot;]
    },
    &quot;cultural&quot;: {
        &quot;allusions&quot;: [&quot;存在主义哲学&quot;, &quot;中等频率&quot;],
        &quot;knowledge_domains&quot;: [&quot;佛教意象&quot;, &quot;叙事治疗&quot;, &quot;时间现象学&quot;]
    },
    &quot;rhythm&quot;: {
        &quot;syllable_pattern&quot;: &quot;长短句自然交替，音节丰富多变&quot;,
        &quot;pause_pattern&quot;: [&quot;分号构建的沉思单元&quot;, &quot;破折号制造呼吸裂隙&quot;],
        &quot;tempo&quot;: &quot;舒缓而富于变化&quot;
    }
}
```

### 文风抽取 - 李娟 - 《我的阿勒泰》



```
{
    &quot;style_summary&quot;: &quot;充满生命质感的边疆叙事，以荒原体验构建诗性现实主义风格&quot;,
    &quot;language&quot;: {
        &quot;sentence_pattern&quot;: [&quot;长句嵌套短句的呼吸感&quot;, &quot;口语化独白与文学性描写交织&quot;],
        &quot;word_choice&quot;: {
            &quot;formality_level&quot;: &quot;2&quot;,
            &quot;preferred_words&quot;: [&quot;荒茫茫&quot;, &quot;嗖嗖&quot;, &quot;咣咣当当&quot;, &quot;斑斓&quot;],
            &quot;avoided_words&quot;: [&quot;抽象概念&quot;, &quot;学术术语&quot;]
        },
        &quot;rhetoric&quot;: [&quot;通感（&#x27;风压得很硬&#x27;）&quot;, &quot;矛盾修辞（&#x27;危险的美&#x27;）&quot;]
    },
    &quot;structure&quot;: {
        &quot;paragraph_length&quot;: &quot;200-500字动态变化&quot;,
        &quot;transition_style&quot;: &quot;意识流式时空跳跃&quot;,
        &quot;hierarchy_pattern&quot;: &quot;现实遭遇触发哲学联想&quot;
    },
    &quot;narrative&quot;: {
        &quot;perspective&quot;: &quot;亲历者限知视角&quot;,
        &quot;time_sequence&quot;: &quot;物理时间与心理时间交错&quot;,
        &quot;narrator_attitude&quot;: &quot;自嘲式真诚&quot;
    },
    &quot;emotion&quot;: {
        &quot;intensity&quot;: &quot;3&quot;,
        &quot;expression_style&quot;: &quot;痛感与诗意的共生&quot;,
        &quot;tone&quot;: &quot;坚韧包裹的苍凉&quot;
    },
    &quot;thinking&quot;: {
        &quot;logic_pattern&quot;: &quot;具象-抽象螺旋上升&quot;,
        &quot;depth&quot;: &quot;4&quot;,
        &quot;rhythm&quot;: &quot;风暴般的意识湍流&quot;
    },
    &quot;uniqueness&quot;: {
        &quot;signature_phrases&quot;: [&quot;大地的文法&quot;, &quot;风的重量&quot;],
        &quot;imagery_system&quot;: [&quot;旋转的沙尘柱&quot;, &quot;玛瑙般的石子&quot;]
    },
    &quot;cultural&quot;: {
        &quot;allusions&quot;: [&quot;游牧文明符号&quot;, &quot;高频率&quot;],
        &quot;knowledge_domains&quot;: [&quot;边疆地理&quot;, &quot;机械常识&quot;]
    },
    &quot;rhythm&quot;: {
        &quot;syllable_pattern&quot;: &quot;爆破音与长元音交替&quot;,
        &quot;pause_pattern&quot;: &quot;风蚀般的语流断裂&quot;,
        &quot;tempo&quot;: &quot;摩托引擎般的律动&quot;
    }
}
```

## 文风抽取 - 许倬云 -《说中国》

```
{
    &quot;style_summary&quot;: &quot;严谨的历史论述融合跨文明比较的学术写作风格&quot;,
    &quot;language&quot;: {
        &quot;sentence_pattern&quot;: [&quot;复合长句主导&quot;, &quot;分号连接的对比结构&quot;],
        &quot;word_choice&quot;: {
            &quot;formality_level&quot;: 4,
            &quot;preferred_words&quot;: [&quot;共主宝座&quot;, &quot;互市&quot;, &quot;怀柔&quot;, &quot;层级统治&quot;],
            &quot;avoided_words&quot;: [&quot;口语化表达&quot;, &quot;情感形容词&quot;]
        },
        &quot;rhetoric&quot;: [&quot;跨文明对比&quot;, &quot;数据论证&quot;, &quot;地理意象铺陈&quot;]
    },
    &quot;structure&quot;: {
        &quot;paragraph_length&quot;: &quot;500-800字学术段落&quot;,
        &quot;transition_style&quot;: &quot;时空双轴推进&quot;,
        &quot;hierarchy_pattern&quot;: &quot;地理→族群→政治→影响的因果链&quot;
    },
    &quot;narrative&quot;: {
        &quot;perspective&quot;: &quot;全知历史观察者&quot;,
        &quot;time_sequence&quot;: &quot;线性编年与比较分析交织&quot;,
        &quot;narrator_attitude&quot;: &quot;克制性评价&quot;
    },
    &quot;emotion&quot;: {
        &quot;intensity&quot;: 2,
        &quot;expression_style&quot;: &quot;数据化论证&quot;,
        &quot;tone&quot;: &quot;冷峻的历史理性&quot;
    },
    &quot;thinking&quot;: {
        &quot;logic_pattern&quot;: &quot;文明生态比较框架&quot;,
        &quot;depth&quot;: 4,
        &quot;rhythm&quot;: &quot;论证-例证-升华的三段循环&quot;
    },
    &quot;uniqueness&quot;: {
        &quot;signature_phrases&quot;: [&quot;草原帝国模式&quot;, &quot;农耕游牧并存态&quot;],
        &quot;imagery_system&quot;: [&quot;马背文明&quot;, &quot;戈壁走廊&quot;, &quot;帝国兴衰曲线&quot;]
    },
    &quot;cultural&quot;: {
        &quot;allusions&quot;: [&quot;史记考据&quot;, &quot;罗马军制比较&quot;],
        &quot;knowledge_domains&quot;: [&quot;历史地理学&quot;, &quot;军事制度史&quot;, &quot;民族迁徙学&quot;]
    },
    &quot;rhythm&quot;: {
        &quot;syllable_pattern&quot;: &quot;四字专业术语集群&quot;,
        &quot;pause_pattern&quot;: &quot;数据插入式停顿&quot;,
        &quot;tempo&quot;: &quot;学术论述的庄重行进&quot;
    }
}
```

## 文风抽取 - 鲁迅 -《狂人日记》

```
{
    &quot;style_summary&quot;: &quot;以象征主义笔法展现的批判现实主义文本，充满精神异化与历史解构的先锋性写作&quot;,
    &quot;language&quot;: {
        &quot;sentence_pattern&quot;: [&quot;短促质问句&quot;, &quot;跨逻辑跳跃句式&quot;],
        &quot;word_choice&quot;: {
            &quot;formality_level&quot;: 3,
            &quot;preferred_words&quot;: [&quot;吃人&quot;, &quot;血字&quot;, &quot;恶狠狠&quot;, &quot;咬你几口&quot;],
            &quot;avoided_words&quot;: [&quot;雅言&quot;, &quot;婉转表达&quot;]
        },
        &quot;rhetoric&quot;: [&quot;病态隐喻&quot;, &quot;悖论修辞&quot;, &quot;暴力转喻&quot;]
    },
    &quot;structure&quot;: {
        &quot;paragraph_length&quot;: &quot;碎片化短段落（50-100字）&quot;,
        &quot;transition_style&quot;: &quot;意识流式场景切换&quot;,
        &quot;hierarchy_pattern&quot;: &quot;具象事例→抽象批判的爆发式递进&quot;
    },
    &quot;narrative&quot;: {
        &quot;perspective&quot;: &quot;精神分裂型第一人称&quot;,
        &quot;time_sequence&quot;: &quot;时空错置的蒙太奇&quot;,
        &quot;narrator_attitude&quot;: &quot;怀疑主义狂欢&quot;
    },
    &quot;emotion&quot;: {
        &quot;intensity&quot;: 5,
        &quot;expression_style&quot;: &quot;歇斯底里式爆发&quot;,
        &quot;tone&quot;: &quot;焦虑与愤怒的复调&quot;
    },
    &quot;thinking&quot;: {
        &quot;logic_pattern&quot;: &quot;谵妄逻辑推演&quot;,
        &quot;depth&quot;: 4,
        &quot;rhythm&quot;: &quot;癫痫性脑波节奏&quot;
    },
    &quot;uniqueness&quot;: {
        &quot;signature_phrases&quot;: [&quot;救救孩子&quot;, &quot;从来如此便对吗&quot;, &quot;字缝里看出字&quot;],
        &quot;imagery_system&quot;: [&quot;吃人社会&quot;, &quot;铁屋子&quot;, &quot;带血历史账簿&quot;]
    },
    &quot;cultural&quot;: {
        &quot;allusions&quot;: [&quot;解构四书五经&quot;, &quot;颠覆宗法制&quot;],
        &quot;knowledge_domains&quot;: [&quot;精神病理学&quot;, &quot;文化人类学&quot;]
    },
    &quot;rhythm&quot;: {
        &quot;syllable_pattern&quot;: &quot;爆破音密集排列&quot;,
        &quot;pause_pattern&quot;: &quot;神经质式短停顿&quot;,
        &quot;tempo&quot;: &quot;逐渐加速的末日审判鼓点&quot;
    }
}
```

## 文风抽取 - 王小波 -《万寿寺》

```
{
    &quot;style_summary&quot;: &quot;意识流叙事与黑色幽默交织的后现代主义文本&quot;,
    &quot;language&quot;: {
        &quot;sentence_pattern&quot;: [&quot;嵌套式长句&quot;, &quot;解构性短句穿插&quot;],
        &quot;word_choice&quot;: {
            &quot;formality_level&quot;: 3,
            &quot;preferred_words&quot;: [&quot;乐呵呵&quot;, &quot;奸诈&quot;, &quot;疑惑&quot;, &quot;绿荫&quot;, &quot;体液&quot;],
            &quot;avoided_words&quot;: [&quot;抒情形容词&quot;, &quot;传统文学意象&quot;]
        },
        &quot;rhetoric&quot;: [&quot;悖论修辞&quot;, &quot;病理学隐喻&quot;, &quot;色彩通感&quot;]
    },
    &quot;structure&quot;: {
        &quot;paragraph_length&quot;: &quot;300-500字不等&quot;,
        &quot;transition_style&quot;: &quot;神经突触式跳跃&quot;,
        &quot;hierarchy_pattern&quot;: &quot;现实-回忆-虚构三重嵌套&quot;
    },
    &quot;narrative&quot;: {
        &quot;perspective&quot;: &quot;自我怀疑的第一人称&quot;,
        &quot;time_sequence&quot;: &quot;螺旋状时间褶皱&quot;,
        &quot;narrator_attitude&quot;: &quot;临床观察式疏离&quot;
    },
    &quot;emotion&quot;: {
        &quot;intensity&quot;: 3,
        &quot;expression_style&quot;: &quot;解剖学冷叙述&quot;,
        &quot;tone&quot;: &quot;存在主义荒诞&quot;
    },
    &quot;thinking&quot;: {
        &quot;logic_pattern&quot;: &quot;怀疑论推演&quot;,
        &quot;depth&quot;: 4,
        &quot;rhythm&quot;: &quot;谵妄与清醒交替&quot;
    },
    &quot;uniqueness&quot;: {
        &quot;signature_phrases&quot;: [&quot;奸诈的自觉&quot;, &quot;绿色的压迫&quot;, &quot;体液逻辑&quot;],
        &quot;imagery_system&quot;: [&quot;医用绷带&quot;, &quot;双重吊桥&quot;, &quot;菌类生态&quot;]
    },
    &quot;cultural&quot;: {
        &quot;allusions&quot;: [&quot;解构史学&quot;, &quot;病理学话语&quot;],
        &quot;knowledge_domains&quot;: [&quot;创伤心理学&quot;, &quot;热带植物学&quot;]
    },
    &quot;rhythm&quot;: {
        &quot;syllable_pattern&quot;: &quot;粘稠的南方官话韵律&quot;,
        &quot;pause_pattern&quot;: &quot;意识断层标记&quot;,
        &quot;tempo&quot;: &quot;热带雨季般的绵密节奏&quot;
    }
}
```

## 飞书多维表格工作流自动化抽取

使用方法：把你需要抽取的文章截图贴到第3列，由`deepseek`自动分析一切。



# [文学风格抽取](https://vxc3hj17dym.feishu.cn/wiki/Dqbbwi8g8i6gc1kBkc1cL9hrntg?from=from_copylink)

## 中文推理大模型测试——基于该方法的系统测评

[突破框架：探索大模型的文学创造力 —— 基于QWEN 2.5 - PLUS QWQ 的文本创作与文风模仿能力评测](https://vxc3hj17dym.feishu.cn/wiki/TjaewdtXSiN2GNkD7AtcYoKun0g?from=from_copylink)

## 家乡野菜彩蛋 —— 可以试试看

[汪曾祺-野菜 风格彩蛋](https://vxc3hj17dym.feishu.cn/wiki/OVPWwdyySiMbj4kRNOrcCkTTnQu?from=from_copylink)

# 创作思路

![](img/51c61b5f047e073dce8047735d7384de.png)

[大模型 文本风格迁移 任务探索.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HLM9bbYymomjNRxwgpPc33hrnlg)
