
# YouTube Shorts 运营与赛道分析

## 频道数据列表

| 频道名称 | 频道链接 | 视频链接 | 日期 | 数量 | 类别 | 内容形式 | 音乐 | 趋势 | 备注1 | 备注2 | 备注3 | 备注4 | 更新频率 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| littleclassics | `https://www.youtube.com/@littleclassics7988/shorts` | `https://www.youtube.com/shorts/ygtdtfBgHsQ` | 10-14 | 170 | 儿童+时尚 | 小动画 | 配乐 | 下降 | | | | | 日均4更 |
| ClassyBaby | `https://www.youtube.com/@ClassyBaby` | `https://www.youtube.com/shorts/3fYyOJ1CyGk` | 10-21 | 130 | 儿童+时尚 | 小动画 | 配乐 | 波动 | | | | | |
| Baby Viral Trend | `https://www.youtube.com/@BabyViralTrend/shorts` | `https://www.youtube.com/shorts/70oReHshcN4` | 10-27 | 120 | 儿童+时尚 | 小动画 | 配乐 | 保持 | | | | | |
| AI Việt Nam | `https://www.youtube.com/@aivietnamvuinhon/shorts` | `https://www.youtube.com/shorts/Rc1RI1NEU_w` | 10-20 | 270 | 农村生活+搞笑 | 图片 | 大量语音+简单配乐 | 波动 | | | | | 日均6更，泰国语 |
| GRB world | `https://www.youtube.com/@grbworlds` | `https://www.youtube.com/shorts/y4s4dRh5HOY` | 10-28 | 150 | 猫咪+正能量 | 图片 | 大量语音+简单配乐 | 保持 | | | | | 日更，印度语 |
| VSL Neuz | `https://www.youtube.com/@vslneuzofficial` | `https://www.youtube.com/shorts/war_Ax9KCFQ` | 10-17 | 210 | 猫咪+正能量 | 图片 | 大量语音+简单配乐 | 波动 | | | | | 日更，印度语？ |
| Cat Series Official Channel | `https://www.youtube.com/@catseries85` | `https://www.youtube.com/shorts/9ppyaX4JRC8` | 10-12 | 170 | 猫咪+反面教材 | 图片 | 大量语音+简单配乐 | 略降 | | | | | 日更，英语 |
| Feline funnies | `https://www.youtube.com/@Felinefunnies-y` | `https://www.youtube.com/shorts/h4GJb_TyYG4` | 10-27 | 230 | 猫咪+搞笑 | 微动画 | 合成语音+简单配乐 | 保持 | | | | | 英语 |
| SmolCat Ai | `https://www.youtube.com/@SmolCat-Ai` | `https://www.youtube.com/shorts/KgjqputJij4` | 10-17 | 120 | 猫咪+斗坏人 | 小动画 | 配乐+简单配音 | 波动 | | | | | 类似Lute Catkin |
| Mrk Pop Pix | `https://www.youtube.com/@MrkPopPix` | `https://www.youtube.com/shorts/aShHDg2jfDE` | 10-27 | 160 | 猫咪+妈妈临盆有惊无险 | 小动画 | 配乐 | 保持 | | | | | |

## 数据分析小结

受限于原始数据，有些数据分析的想法没能灵活且高效的实现，上面从大佬的原始数据中做的筛选很多也是近似估计，但也足够向下推进了。早期还是先把流程跑通，后面再回来仔细挖掘更多有机会的细分领域。

## 定位选择与对标账号

又看了`@老马内容出海`(`https://wx.zsxq.com/footprint/818515184445882`)的文章 如何选择适合自己的匿名Shorts赛道，收获非常大，重点启发如下：

- 个人初期（YPP之前）也可以少量矩阵，选2~3个赛道，每个赛道3~5个号（自己估算），分散风险，保持信心。
- 对标账号的收益估算很重要
- 货币化查询工具：`https://ytlarge.com/youtube/monetization-checker/`
- 手动估算公式：`EstimatedRevenue=(TotalViews-10,000,000)*0.0001`，单位美元，保守估计（仅Shorts收入，按 `RPM=$0.1` 计算）
- 对新趋势的判断，不能只看货币化查询工具的结果
- 选题思路：
- 简单粗暴型：播放量从高到低，一比一复刻
- 一鸭多吃型：同样的内容脚本，换不同的形式，多个垂直频道同时做

再结合自己前面的数据分析，整理如下：

### 赛道汇总

| 赛道 | 需求层面 | 供给层面 | 实现层面 | 总结 |
| --- | --- | --- | --- | --- |
| 猫咪类 | 1. 心灵抚慰：被感动，被激励等 2. 娱乐：搞笑，荒诞 | 1. 红海，大量频道 2. 大量货币化案例 3. 大多数播放量下降或断更 4. 少数保持的频道，还是靠独特的内容 | 1. 较复杂，故事剧本-分镜-图片-视频-配乐配音-剪辑等 | 1. 可能是今年AI类最大赛道，靠新技术的表现形式大火，但审美疲劳之后走下坡路 2. 也可以做，要从内容上下功夫，持续产出能吸引观众的内容 3. 形式上能否做些变化，例如猫改成熊猫、兔子、鸭子等 |
| 萌娃时尚走秀 | 1. 萌 | 1. 大概从7、8月开始，频道不是很多 2. 尚未货币化 3. 趋势略有下降 | 1. 较简单，图片提取prompt - 生成图片 - 生成视频 - 配乐 - 剪辑 | 1. 视频新技术带来的新表现形式 2. 可以做，但由于缺少内容，感觉难以持久 3. 关注头部频道的货币化进展，存疑 |
| 图片风格转绘 | 1. 娱乐：搞笑，荒诞 | 1. 频道数量介于前两个之间 2. 大量货币化案例 3. 趋势略有下降 | 1. 较简单，文生图 - 风格转绘 - 转场 - 配乐 | 1. 比较成熟的图片技术，主要靠风格转绘的反差和意外产生笑点 2. 可以做，但由于缺少内容，感觉难以持久 |
| 名人或卡通搞笑 | 1. 娱乐：搞笑，荒诞 | 1. 红海，大量频道 2. 大量货币化案例 3. 趋势略有下降 | 1. 技术较简单，以图片为主 2. 内容较复杂，搞笑的剧本和分镜，以及大量的配音 | 1. 特定的文化背景，纯粹搞笑荒诞的内容 2. 自己确实不擅长这类内容产出，暂时不做 |

### 赛道1：猫咪类（频道太多，按趋势不同选取几个代表性的频道）

| 对标账号 | URL | 初更时间 | 视频数 | 播放量 /亿 | 平均播放量/万 | 上次百万时间 | 趋势 | 总收入估算 /万美元 | 内容脚本 | 画面 | 声音 | 分析 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| GingerCatTV | `https://www.youtube.com/@gingercat-tv` | 2024-3-5 | 62 | 3.1 | 500 | 10-24 | 保持 | 全绿 3 | 正能量故事 | 动画 | 配乐 | |
| StoryCATAI | `https://www.youtube.com/@storyCATAXS` | 2024-5-2 | 62 | 2 | 322 | 10-1 | 略降 | 全绿 1.9 | 应勇搏斗的故事 | 图片 | 配乐 | |
| Mpminds | `https://www.youtube.com/@Mpminds_` | 2024-2-10 | 65 | 1.4 | 215 | 10-12 | 波动 | 全红 X | 悲伤励志的故事 | 动画 | 配乐 | |
| | | | | | | | | | | | | |
| Meow WORLD | `https://www.youtube.com/@MeowWRLD` | | 74 | 1.8 | | 10-11 | 下降 | 全绿 1.7 | | | | |
| Cute Cat Games | `https://www.youtube.com/@CuteCatGames-fc` | | 60 | 1.6 | | 10-4 | 下降 | 全绿 1.5 | | | | |
| AI Catverse | `https://www.youtube.com/@aicatverse` | | 83 | 1.1 | | 10-6 | 下降 | 全绿 1 | | | | |
| | | | | | | | | | | | | |
| Lute Catkin | `https://www.youtube.com/@LuteCatkin` | 2024-8-16 | 26 | 2.2 | 846 | 9-26 | 略降 10-4停更 | 全绿 2.1 | 无厘头猫咪智斗恶人 | 动画 | 配乐 | 成绩不错，有差异化，为何停更？ |
| Cat Realm AI | `https://www.youtube.com/@CatRealmAI` | 2024-3-15 | 36 | 1.9 | 528 | 4-19 | 下降 7-8停更 | 全红 X | 无统一主题的故事 | 动画 | 配乐 | 怀疑是抄袭过重，过不了YPP后停更 |

### 赛道2：萌娃时尚走秀

| 对标账号 | URL | 初更时间 | 视频数 | 播放量 /亿 | 平均播放量/万 | 上次百万时间 | 趋势 | 总收入估算 /万美元 | 内容脚本 | 画面 | 声音 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Baby Viral Trend | `https://www.youtube.com/@BabyViralTrend` | 2024-9-23 | 48 | 1 | 200 | 10-27 | 保持 | 全红 X | 萌娃时尚走秀 | 动画 | 配乐 |
| Carter Sophia | `https://www.youtube.com/@CarterSophia` | 2024-10-15 | 24 | 0.11 | 46 | 10-19 | 略降 | 全红 X | 萌娃时尚走秀 | 动画 | 配乐 |
| ClassyBaby | `https://www.youtube.com/@ClassyBaby` | 2024-9-29 | 48 | 0.23 | 48 | 10-21 | 波动 | 全红 X | 萌娃时尚走秀 | 动画 | 配乐 |
| littleclassics | `https://www.youtube.com/@littleclassics7988/shorts` | 2024-8-20 | 215 | 0.94 | 44 | 10-14 | 下降 | 全红 X | 萌娃时尚走秀 | 动画 | 配乐 |
| AloAiTec | `https://www.youtube.com/@AloAiTec` | 2024-8-19 | 56 | 0.14 | 25 | 9-20 | 下降 | 全红 X | 萌娃时尚走秀 | 动画 | 配乐 |

### 赛道3：图片风格转绘

| 对标账号 | URL | 初更时间 | 视频数 | 播放量 /亿 | 平均播放量/万 | 上次百万时间 | 趋势 | 总收入估算 /万美元 | 内容脚本 | 画面 | 声音 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Mr. Majesty 2 | `https://www.youtube.com/@MrMajesty2` | 2024-5-16 | 725 | 15 | 200 | 10-1 | 下降 | 全绿 15 | 蜘蛛侠转绘 | 图片 | 配乐 |
| TeknoCat Pro | `https://www.youtube.com/@TeknoCatPro` | 2023-9-15 | 404 | 1.4 | 35 | 10-26 | 波动 | 全绿 1.3 | 蜘蛛侠转绘 | 图片 | 配乐 |
| Ballu Wala | `https://www.youtube.com/@BalluWala` | 2024-8-1 | 832 | 0.88 | 10 | 11-2 | 波动 | 全绿 0.78 | 蜘蛛侠转绘 | 图片 | 配乐 |
| Sonja Briana | `https://www.youtube.com/@sonjabriana3135` | 2024-9-10 | 173 | 1.4 | 80 | 10-28 | 波动 | 全绿 1.3 | 卡通转绘 | 图片 | 配乐 |
| MIAW MILKY | `https://www.youtube.com/@MiawMilky` | 2024-7-20 | 241 | 2.5 | 100 | 9-25 | 下降 | 全绿 2.4 | 卡通转绘 | 图片 | 配乐 |

### 赛道4：名人或卡通搞笑

| 对标账号 | URL | 初更时间 | 视频数 | 播放量 /亿 | 平均播放量/万 | 上次百万时间 | 趋势 | 总收入估算 /万美元 | 内容脚本 | 画面 | 声音 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
