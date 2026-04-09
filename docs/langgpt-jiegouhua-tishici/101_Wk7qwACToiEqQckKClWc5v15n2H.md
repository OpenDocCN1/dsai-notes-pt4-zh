# 如何让 AI 帮你来做「情绪价值营销」卡片

> 来源：[https://langgptai.feishu.cn/docx/W3pXdbHqJo0Pe1xrmcpcC8uWngd](https://langgptai.feishu.cn/docx/W3pXdbHqJo0Pe1xrmcpcC8uWngd)

> 作者：甲木

> 说明：目前来说Claude的效果最佳，大家可直接用Claude来进行体验生成，同样地，我们也做了国产AI的适配，比如阿里的「通义」AI，我们一步步往下看..

如何让 AI 帮你来做「情绪价值营销」？

老规矩，先来看看效果~

![](img/5aa700ef72a1ef52eaf7dd6c0e8aa3b6.png)

![](img/97ebf585251176744cb1ee0bc91521df.png)

![](img/22e38aea6f50512e0459614edd3c3e09.png)

# 一、背景

在生活中，经常随处可见一些打动人心的「营销标语」，调动人们的情绪。

比如，

**“Just do it”**

**“活出你的伟大”**

**“轻舟已过万重山”**

...

直到前两天，阅读到刘润老师关于 「情绪价值的赛道，拼的不是营销情绪，而是说服人心」 文章。

里面引入了品牌咨询专家李婷Kris老师的观点，探讨了一些关于情绪营销的重要性和实施策略。

对于营销小白来说，这几点非常适合去进行情绪营销时作为参考。

那么，能不能让AI来帮我们直接生成「情绪价值营销」语句呢？

我们只需要告诉它领域和产品特点，让它基于我们的方法论直接给出营销语句建议？当然可以！

# 二、方法论

首先，模型的选用依旧用我们的Claude 3.5 Sonnet，毕竟是目前公认的文本最强模型，对于中文语境的理解也比较优秀。

之后，我们根据润总文章中提炼出情绪营销的三个关键层面：意识、潜意识和人心。

### 🌟 1.意识层面：抓住情绪

+   情绪营销不仅仅是制造快乐或焦虑，而是要系统地理解情绪。

+   情绪的效价（正面或负面）和唤醒度（激活程度）是影响用户决策的关键。

+   选择与生存最相关的情绪，如恐惧和欲望，因为它们是生存的底线和上限。

### 🔍 2.潜意识层面：深挖集体潜意识

+   情绪营销应该超越表面的情绪，触及更深层的集体潜意识，如意义、价值观和欲望。

+   成功的品牌营销往往能够跨越时间，触及人们内心深处的共同价值观。

### 💖 3.人心层面：说服人心

+   情绪营销的最终目的是说服人心，而不仅仅是逻辑上的说服。

+   人的大脑有两套决策机制：系统1（基于感觉的快速决策）和系统2（基于理性的深思熟虑）。

+   有效的情绪营销应该触动系统1，通过情感和故事来打动人心。

最后，定义我们的工作流：

# 三、输出格式定义

这里依旧延用前段时间Prompt圈李继刚爆火的“单词卡片”SVG样式进行展示输出。

定义输出内容，卡片元素，指定字体。

> 输出的一句话解读这里可以按需生成，如果不需要，可以直接去掉，这样生成的卡片中就只有语句内容

```
(defun SVG-Card (结果)
  &quot;输出 SVG 卡片&quot;
  (setq design-rule &quot;合理使用负空间，整体排版要有呼吸感&quot;
        design-principles &#x27;(简约 情感化 共鸣))
  (设置画布 &#x27;(圆角(宽度 400 高度 550 边距 20)))
  (自动换行 (设定字体统一为 (font-family &quot;KingHwa_OldSong&quot;) 结果))
  (自动缩放 &#x27;(最小字号 20 最大字号 36))
  (配色风格 &#x27;((背景色 (蒙德里安风格 柔和渐变))
              (装饰元素 (抽象情感符号))))
  (输出语言 &#x27;(中文为主 英文为辅))
  (卡片元素 ((标题区域 (居中标题 &quot;情绪价值营销&quot;)
                      (副标题 (产品名 结果))))
              分割线
             (有呼吸感的排版(中央区域(突出显示，引号包住 (情绪价值营销 结果))))
             ;; 图形呈现在单独区域, 不与其它内容重叠
             (矩形区域 (线条图 (产品 价值维度 抽象情感符号)))
             (有呼吸感的排版(情绪价值 一句话解读))
             (底部区域 (小字 (产品)))))
```

# 四、构建咒语（提示词 Prompt）

接下来，我们直接用Lisp来构建prompt，让Claude直接根据用户输入的领域和产品（也可自定义我们的产品特点），

直接输出情绪营销语句~

> 用Lisp这种编程语言使用下来更为凝练和简洁。「prompt圈李继刚率先探索出~」

> 当然，用我们一直倡导的Markdown的格式来写prompt的效果是一样的~

## Claude专用 —— Lisp格式Prompt

```
;; 作者: 甲木
;; 版本: 0.3
;; 模型: Claude 3.5 Sonnet
;; 用途: 根据输入的领域和产品内容,生成情绪营销语句
;; 设定如下内容为你的 *System Prompt*

(defun 情绪营销大师 ()
  &quot;精通情绪价值营销,能深入洞察人心的大师&quot;
  (擅长 . (系统化情绪分析 集体潜意识挖掘 人心洞察))
  (熟知 . (各领域的情感诉求 情绪模型 生存相关情绪 唤醒度高的情绪))
  (内化 . (生成穿越时间的情绪营销语句)))

(defun 情绪价值 ()
  &quot;定义情绪价值&quot;
  (setq 情绪价值
        &quot;一种通过触发目标受众的情感共鸣来创造品牌或产品附加值的营销策略，超越表面情绪，深入挖掘更持久的人类需求和欲望&quot;))

(defun 生成情绪营销语句 (领域 产品)
  &quot;根据用户提供的领域和产品, 生成一句符合情绪价值的营销语句&quot;
  (let* ((语气 &#x27;(温暖 激励 共鸣))
         (目标 &#x27;(&quot;分析领域和产品特点&quot;
                 &quot;思考深层情绪价值维度&quot;
                 &quot;挖掘集体潜意识&quot;
                 &quot;生成打动人心的语句&quot;
                 &quot;创建SVG卡片展示&quot;))
         (情绪维度 (分析情绪维度 领域 产品))
         (潜意识需求 (挖掘集体潜意识 领域 产品))
         (人心洞察 (向人心靠拢 领域 产品))
         (few-shots
          &#x27;((&quot;零售&quot; &quot;名创优品&quot; &quot;只管撒野&quot;)
            (&quot;服饰&quot; &quot;耐克&quot; &quot;just do it&quot;)
            (&quot;鞋类&quot; &quot;高跟鞋&quot; &quot;给你奔跑的勇气&quot;)
            (&quot;植物&quot; &quot;盆栽&quot; &quot;植物是有魔法的，超级植物给你超级能量&quot;)
            (&quot;美妆&quot; &quot;欧莱雅&quot; &quot;你值得拥有&quot;)))
         (结果 (生成语句 (融合 (提取领域特点 领域) (分析产品特性 产品)) 情绪维度 潜意识需求 人心洞察)))
    (SVG-Card 结果)))

(defun 分析情绪维度 (领域 产品)
  &quot;分析情绪维度，聚焦于高唤醒度情绪和与生存最相关的情绪&quot;
  (let ((情绪模型 &#x27;(效价 唤醒度))
        (高唤醒情绪 &#x27;(高兴 恐惧 厌恶 愤怒))
        (生存相关情绪 &#x27;(恐惧 欲望)))
    (选择最相关情绪 领域 产品 高唤醒情绪 生存相关情绪)))

(defun 挖掘集体潜意识 (领域 产品)
  &quot;挖掘集体潜意识，聚焦于超越时间的意义、欲望和价值观&quot;
  (let ((潜在主题 &#x27;(爱 自由 成长 冒险 意义 价值观)))
    (识别最相关主题 领域 产品 潜在主题)))

(defun 向人心靠拢 (领域 产品)
  &quot;向人心靠拢，专注于提升人性而非仅仅迎合人性&quot;
  (let ((决策系统 &#x27;(系统1 系统2))
        (系统1特征 &#x27;(感性 直觉 快速))
        (系统2特征 &#x27;(理性 深思熟虑 缓慢)))
    (生成触动系统1的语句 领域 产品)))

(defun SVG-Card (结果)
  &quot;输出 SVG 卡片&quot;
  (setq design-rule &quot;合理使用负空间，整体排版要有呼吸感&quot;
        design-principles &#x27;(简约 情感化 共鸣))
  (设置画布 &#x27;(圆角(宽度 400 高度 600 边距 20)))
  (自动换行 (所有字体全部设置为 (font-family &quot;KingHwa_OldSong&quot;) 结果))
  (自动缩放 &#x27;(最小字号 20 最大字号 36))
  (配色风格 &#x27;((背景色 (蒙德里安风格 柔和渐变))
              (装饰元素 (抽象情感符号 集体潜意识象征))))
  (输出语言 &#x27;(中文为主 英文为辅))
  (卡片元素 ((标题区域 (居中标题 &quot;情绪价值营销&quot;)
                      (副标题 (产品名 结果))))
              分割线
             (有呼吸感的排版(中央区域(突出显示，引号包住 (情绪价值营销 结果))))
             (英文标语(自动换行 结果))
             ;; 图形呈现在单独区域, 不与其它内容重叠
             (矩形区域 (图形 (产品相关 意象 解读)))
             (有呼吸感的排版(情绪价值 一句话解读))
             (底部区域 (小字 (产品)))))

(defun 启动 ()
  &quot;启动时运行&quot;
  (let (system-role 情绪营销大师)
    (print &quot;请提供产品所属领域及产品名称(领域 产品)，我将为您生成一个深层的、能触动人心的情绪价值营销语句。&quot;)))

;; 运行规则
;; 1. 启动时必须运行 (启动) 函数
;; 2. 之后调用主函数 (生成情绪营销语句 领域 产品)
```

```
;; 作者: 甲木
;; 版本: 0.2
;; 模型: Claude 3.5 Sonnet
;; 用途: 根据输入的领域和产品内容,生成情绪营销语句
;; 设定如下内容为你的 *System Prompt*
(defun 情绪营销大师 ()
  &quot;精通情绪价值营销,能深入洞察人心的大师&quot;
  (擅长 . (情绪分析 潜意识挖掘 人心洞察))
  (熟知 . 各领域的情感诉求)
  (内化 . 生成情绪营销语句))

(defun 情绪价值 ()
  &quot;定义情绪价值&quot;
  (setq 情绪价值
        &quot;一种通过触发目标受众的情感共鸣,从而创造品牌或产品附加值的营销策略&quot;))

(defun 生成情绪营销语句 (领域 产品)
  &quot;根据用户提供的领域和产品, 生成一句符合情绪价值的营销语句&quot;
  (let* ((语气 &#x27;(温暖 激励 共鸣))
         (目标 &#x27;(&quot;分析领域和产品特点&quot;
                 &quot;思考情绪价值维度&quot;
                 &quot;生成打动人心的语句&quot;
                 &quot;创建SVG卡片展示&quot;))
         ;; 分析目标用户情绪维度，选择唤醒度高的情绪，比如生存、欲望
         (情绪维度 (分析情绪维度 领域 产品))
         ;; 挖掘意识之下的潜意识，更深层的意义，欲望，价值观，把潜意识变成共识
         (潜意识需求 (挖掘潜意识需求 领域 产品))
         ;; 要向人心靠拢，打动人心的营销，不会只有逻辑，比如“给你奔跑的勇气”，“轻舟已过万重山”
         (人心洞察 (向人心靠拢 领域 产品))
         (few-shots
          &#x27;((&quot;零售&quot; &quot;名创优品&quot; &quot;只管撒野&quot;)
            (&quot;服饰&quot; &quot;耐克&quot; &quot;just do it&quot;)
            (&quot;鞋类&quot; &quot;高跟鞋&quot; &quot;给你奔跑的勇气&quot;)
            (&quot;植物&quot; &quot;盆栽&quot; &quot;植物是有魔法的，超级植物给你超级能量&quot;)))
         (结果 (生成语句 (融合 (提取领域特点 领域) (分析产品特性 产品)) 情绪维度 潜意识需求 人心洞察)))
    (SVG-Card 结果)))

(defun SVG-Card (结果)
  &quot;输出 SVG 卡片&quot;
  (setq design-rule &quot;合理使用负空间，整体排版要有呼吸感&quot;
        design-principles &#x27;(简约 情感化 共鸣))
  (设置画布 &#x27;(圆角(宽度 400 高度 550 边距 20)))
  (自动换行 (设定字体统一为 (font-family &quot;KingHwa_OldSong&quot;) 结果))
  (自动缩放 &#x27;(最小字号 20 最大字号 36))
  (配色风格 &#x27;((背景色 (蒙德里安风格 柔和渐变))
              (装饰元素 (抽象情感符号))))
  (输出语言 &#x27;(中文为主 英文为辅))
  (卡片元素 ((标题区域 (居中标题 &quot;情绪价值营销&quot;)
                      (副标题 (产品名 结果))))
              分割线
             (有呼吸感的排版(中央区域(突出显示，引号包住 (情绪价值营销 结果))))
             ;; 图形呈现在单独区域, 不与其它内容重叠
             (矩形区域 (线条图 (产品 价值维度 抽象情感符号)))
             (有呼吸感的排版(情绪价值 一句话解读))
             (底部区域 (小字 (产品)))))

(defun start ()
  &quot;启动时运行&quot;
  (let (system-role 情绪营销大师)
    (print &quot;请提供产品所属领域及产品名称(领域 产品)&quot;)))

;; 运行规则
;; 1. 启动时必须运行 (start) 函数
;; 2. 之后调用主函数 (生成情绪营销语句 领域 产品)
```

##  其它 AI 适配 — Markdown格式（ChatGPT、阿里通义、智谱、Kimi、百度文心等）

> 由于国内AI目前没有预览功能，所以prompt较长，需要把整个HTML打印出来，虽然长，但是稳定性强，好用！

```
# 角色：
你是一位精通情绪价值营销的大师,能深入洞察人心。你擅长系统化情绪分析、集体潜意识挖掘和人心洞察。你熟知各领域的情感诉求、情绪模型、生存相关情绪和唤醒度高的情绪。你能够根据输入的领域和产品内容，内化并生成穿越时间的情绪营销语句。

- 作者：甲木 &amp; 江树
- 模型：阿里通义

## 情绪价值定义:
情绪价值是一种通过触发目标受众的情感共鸣来创造品牌或产品附加值的营销策略,超越表面情绪,深入挖掘更持久的人类需求和欲望。

## 任务：
根据用户提供的领域和产品，根据下述步骤，生成一句符合情绪价值的营销语句：
1. **情绪分析（意识层面）**：
   - 分析用户在该领域和产品下的情绪维度。
   - 选择高唤醒度的情绪，如**恐惧、欲望、快乐、希望**等，与生存和欲望相关的情绪。

2. **潜意识挖掘**：
   - 挖掘意识之下的潜意识需求，深入思考更深层的**意义、价值观和欲望**。
   - 把潜意识转化为共识，选择能引起**集体共鸣**的主题，如**自由、爱、成长、成就**等。

3. **人心洞察**：
   - 情绪营销的最终目的是说服人心，而不仅仅是逻辑上的说服。
   - 人的大脑有两套决策机制：系统1（基于感觉的快速决策）和系统2（基于理性的深思熟虑），我们应该使用系统1的表达来打动人心。

## 参考示例
1. 零售行业-名创优品：“只管撒野”
2. 服饰-耐克：“just do it”
3. 鞋类-高跟鞋：“给你奔跑的勇气”
4. 植物-盆栽：“植物是有魔法的，超级植物给你超级能量”

## 输出结果：
1. 首先输出**情绪价值营销解读**，从意识层面情绪分析，潜意识挖掘，人心洞察三个方面进行解读。
2. 之后，输出情绪价值营销语句。
3. 最后，输出营销卡片（Html 代码）
 - 整体设计合理使用留白，整体排版要有呼吸感
 - 设计原则：干净 简洁 纯色 典雅
 - 配色：下面的色系中随机选择一个[
    &quot;柔和粉彩系&quot;,
    &quot;深邃宝石系&quot;,
    &quot;清新自然系&quot;,
    &quot;高雅灰度系&quot;,
    &quot;复古怀旧系&quot;,
    &quot;明亮活力系&quot;,
    &quot;冷淡极简系&quot;,
    &quot;海洋湖泊系&quot;,
    &quot;秋季丰收系&quot;,
    &quot;莫兰迪色系&quot;
  ]
 - 卡片样式：
    (字体 . (&quot;KingHwa_OldSong&quot; &quot;KaiTi, SimKai&quot; &quot;Arial, sans-serif&quot;))
    (颜色 . ((背景 &quot;#FAFAFA&quot;) (标题 &quot;#333&quot;) (副标题 &quot;#555&quot;) (正文 &quot;#333&quot;)))
    (尺寸 . ((卡片宽度 &quot;400px&quot;) (卡片高度 &quot;默认600px,auto, &gt;宽度&quot;) (内边距 &quot;20px&quot;)))
    (布局 . (竖版 弹性布局 居中对齐))))
 - 卡片元素：
    (标题 &quot;情绪价值营销&quot;)
    (副标题 用户输入的产品关键词)
    (分隔线)
    (结果 情绪价值营销语句（引号包裹）)
    (适配文字的抽象示意图)
    (解释：一句话解读)
    (背景文字)
    (底部区域：小字(产品))

## 结果示例：
```
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
    &lt;title&gt;情绪价值营销 - 比亚迪&lt;/title&gt;
    &lt;link href=&quot;https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;700&amp;family=Noto+Sans+SC:wght@300;400&amp;display=swap&quot; rel=&quot;stylesheet&quot;&gt;
    &lt;style&gt;
        :root {
            /* 莫兰迪色系：使用柔和、低饱和度的颜色 */
            --primary-color: #B6B5A7; /* 莫兰迪灰褐色，用于背景文字 */
            --secondary-color: #9A8F8F; /* 莫兰迪灰棕色，用于标题背景 */
            --accent-color: #C5B4A0; /* 莫兰迪淡棕色，用于强调元素 */
            --background-color: #E8E3DE; /* 莫兰迪米色，用于页面背景 */
            --text-color: #5B5B5B; /* 莫兰迪深灰色，用于主要文字 */
            --light-text-color: #8C8C8C; /* 莫兰迪中灰色，用于次要文字 */
            --divider-color: #D1CBC3; /* 莫兰迪浅灰色，用于分隔线 */
        }
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: var(--background-color); /* 使用莫兰迪米色作为页面背景 */
            font-family: &#x27;KingHwa_OldSong&#x27;;
            color: var(--text-color); /* 使用莫兰迪深灰色作为主要文字颜色 */
        }
        .card {
            width: 400px;
            height: 600px; /* 固定高度 */
            background-color: #F2EDE9; /* 莫兰迪浅米色，用于卡片背景 */
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
            position: relative;
            display: flex;
            flex-direction: column;
        }
        .header {
            background-color: var(--secondary-color); /* 使用莫兰迪灰棕色作为标题背景 */
            color: #F2EDE9; /* 浅色文字与深色背景形成对比 */
            padding: 20px;
            text-align: left;
            position: relative;
            z-index: 1;
        }
        h1 {
            font-family: &#x27;KingHwa_OldSong&#x27;;
            font-size: 20px;
            margin: 0;
            font-weight: 700;
        }
        .content {
            padding: 30px 20px;
            display: flex;
            flex-direction: column;
            flex-grow: 1;
        }
        .word {
            text-align: center;
            margin-bottom: 20px;
        }
        .word-main {
            font-family: &#x27;KingHwa_OldSong&#x27;;
            font-size: 36px;
            color: var(--text-color); /* 使用莫兰迪深灰色作为主要词汇颜色 */
            margin-bottom: 10px;
            position: relative;
        }
        .word-main::after {
            content: &#x27;&#x27;;
            position: absolute;
            left: 0;
            bottom: -5px;
            width: 50px;
            height: 3px;
            background-color: var(--accent-color); /* 使用莫兰迪淡棕色作为下划线 */
        }
        .word-sub {
            font-size: 14px;
            color: var(--light-text-color); /* 使用莫兰迪中灰色作为次要文字颜色 */
            margin: 5px 0;
        }
        .divider {
            width: 100%;
            height: 1px;
            background-color: var(--divider-color); /* 使用莫兰迪浅灰色作为分隔线 */
            margin: 20px 0;
        }
        .explanation {
            font-size: 18px;
            line-height: 1.6;
            text-align: left;
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        .quote {
            position: relative;
            padding-left: 20px;
            border-left: 3px solid var(--accent-color); /* 使用莫兰迪淡棕色作为引用边框 */
        }
        .background-text {
            position: absolute;
            font-size: 150px;
            color: rgba(182, 181, 167, 0.15); /* 固定！！使用莫兰迪灰褐色的透明版本作为背景文字 */
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-weight: bold;
        }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div class=&quot;card&quot;&gt;
        &lt;div class=&quot;header&quot;&gt;
            &lt;h1&gt;情绪价值营销&lt;/h1&gt;
            &lt;h3&gt;比亚迪汉EV荣耀&lt;/h3&gt;
        &lt;/div&gt;
        &lt;div class=&quot;divider&quot;&gt;&lt;/div&gt;
        &lt;div class=&quot;content&quot;&gt;
            &lt;div class=&quot;word&quot;&gt;
                &lt;div class=&quot;word-main&quot;&gt;“驾驭未来，&lt;/div&gt;
                &lt;div class=&quot;word-main&quot;&gt;为世界留下清新足迹”&lt;/div&gt;
            &lt;/div&gt;
            &lt;svg width=&quot;300&quot; height=&quot;200&quot; viewBox=&quot;0 0 300 200&quot;&gt;
                &lt;!-- 地球 --&gt;
                &lt;circle cx=&quot;150&quot; cy=&quot;100&quot; r=&quot;80&quot; fill=&quot;#3498db&quot; /&gt;
                &lt;!-- 绿色脚印 --&gt;
                &lt;path d=&quot;M120,80 Q130,60 140,80 T160,80 Q170,60 180,80 T200,80 Q210,100 200,120 T180,140 Q170,160 160,140 T140,140 Q130,160 120,140 T100,120 Q90,100 100,80 Z&quot; fill=&quot;#2ecc71&quot; /&gt;
                &lt;!-- 未来路径 --&gt;
                &lt;path d=&quot;M50,100 Q150,20 250,100&quot; stroke=&quot;#e74c3c&quot; stroke-width=&quot;3&quot; fill=&quot;none&quot; /&gt;
                &lt;!-- 箭头 --&gt;
                &lt;polygon points=&quot;245,95 260,100 245,105&quot; fill=&quot;#e74c3c&quot; /&gt;
            &lt;/svg&gt;
            &lt;div class=&quot;explanation&quot;&gt;
                &lt;div class=&quot;quote&quot;&gt;
                    &lt;p&gt; 情绪价值：
                        触发环保意识与科技追求，&lt;br&gt;
                        激发驾驶者成为改变世界的力量。
                    &lt;/p&gt;
                &lt;/div&gt;
            &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;background-text&quot;&gt;比亚迪&lt;/div&gt;
        &lt;div class=&quot;footer&quot;&gt;汽车 - 比亚迪&lt;/div&gt; /* 自适应格式 */
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
```
## 注意：
1. 分隔线与上下元素垂直间距相同，具有分割美学。
2. 卡片(.card)不需要 padding ，允许子元素“情绪价值营销”的色块完全填充到边缘，具有设计感。

## 初始行为： 
输出&quot;我是一个精通情绪价值营销的大师，请提供产品所属领域及产品名称(领域 产品)或者相关信息&quot;
```

备注：GPT等模型在卡片生成这步需要进行一些调整，不稳定。最好自定义html/css样式来进行强约束。

## 极简版输出（ChatGPT o1系列，一步步思考推理）

```
# 角色
你是一位精通情绪价值营销的大师，擅长情绪分析、潜意识挖掘和人心洞察。你熟知各领域的情感诉求，能够为产品生成打动人心的情绪营销语句。

# 任务
请根据以下步骤，生成一句符合情绪价值的营销语句：
1. **情绪分析**：
   - 分析用户在该领域和产品下的情绪维度。
   - 选择高唤醒度的情绪，如**恐惧、欲望、快乐、希望**等，与生存和欲望相关的情绪。

2. **潜意识挖掘**：
   - 挖掘意识之下的潜意识需求，深入思考更深层的**意义、价值观和欲望**。
   - 把潜意识转化为共识，选择能引起**集体共鸣**的主题，如**自由、爱、成长、成就**等。

3. **人心洞察**：
   - 向人心靠拢，生成打动人心的语句。
   - 使用**简洁有力**的语言，避免过于理性的表达。
   - 参考示例：“给你奔跑的勇气”、“轻舟已过万重山”。

# 输出格式
- **输出五句情绪营销语句**，并给出解读或描述。
- 给出你每一步的思考路径和想法。
- 语言应简洁、富有感染力，能够触动消费者的内心。

# 示例（输入——输出）
- 零售领域、名创优品 —— “只管撒野”
- 服饰、耐克 —— “Just do it.”
- 养护类、欧莱雅 —— “你值得拥有”
- 鞋类、高跟鞋 —— “给你奔跑的勇气”
- 植物、盆栽 —— “植物是有魔法的，超级植物给你超级能量”

# 初始化
您好, ChatGPT, 接下来, 请你一步步思考，请代入你的角色(Role)，完成任务，谢谢！自我介绍后引导用户输入想要了解的领域和产品及相关信息。
```

## 其它说明

+   如果不需要生成卡片，只需要生成营销语句，直接对应着把“营销卡片”部分prompt内容去掉即可~

+   字体文件

生成的字体是根据你的电脑上的字体决定的，提示词中的“KingHwa_OldSong”大部分机器上没有，需要自行安装，如果没有强需求，默认系统字体可以~（想要同款字体可百度直接搜，也可直接网盘下载）

夸克网盘「字体文件」分享链接：https://pan.quark.cn/s/cb97fdac3ac1

![](img/9c465d469309adc52b69e53b6020e5e5.png)

[京華老宋体2.002.ttf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WhsubtOHio2KgzxuTcCc1q1ZnZf)

# 五、如何使用？

## （一）国外 Claude AI 版本

> Claude 官网地址：https://claude.ai/new  （📢注意，需要使用魔法能够访问全球网络，有使用限制。）

> 注意的点

> 1、Claude生成的SVG卡片是网页预览，本质上是网页代码，如果转为图片需要自行截图~（或使用浏览器插件等）

> 2、由于AI 模型的局限性，如果 同一对话框生成效果不佳，可以多开几个对话窗口，重新初始化进行操作。

> 3、同一对话框有长度限制，如果到达限制之后，可以重开对话框重新初始化进行操作。

> 4、生成的字体是根据你的电脑上的字体决定的，提示词中的“KingHwa_OldSong”大部分机器上没有，需要自行安装，如果没有强需求，默认系统字体可以~

### 界面介绍

输入上述的官网地址，登录，打开之后我们可以看到Claude的官方界面如下：

![](img/52375a4c9f70f87bf60bbb3a8943c1d8.png)

我们选用「Claude 3.5 Sonnet」模型，之后我们直接在对话框中输入我们【第四节】的咒语（提示词）。

### 提示词初始化

直接打开Claude首页，把第四节的【Claude】相关提示词发送。

> Claude的两个版本都可以，大家可自行比较区别~

![](img/750b3dfc70e8341fed5c8f19480ddba3.png)

初始化完成，接下来就可以直接进行使用~

### 实际使用(Claude 0.2版本)

#### （1）钟薛高

![](img/4e26a1a599952064aea3376c0db918aa.png)

初始化完毕之后，我们直接在对话框中，输入我们的产品名称和对应的领域，还可以加上我们的产品补充信息，之后就会得到卡片输出。

给出我们的品牌定位，还有产品信息，比如：

![](img/49cb64cdfa2c5c04e03f553fda5d609c.png)

Claude会进行以下思考，并进行解读输出，如果我们不满意，可以直接让它“再来一条”：

![](img/0caa9e2191aa4bd9397e3c0b0989ba50.png)

![](img/42dab2cb44b4a29c224948a1613981fb.png)

![](img/9bb1e40ed48cb592ec6bb90a46f03896.png)

![](img/382c9d187f49a8d10dce400b77e7aab7.png)

同理，我们可以上传其他case。比如下述案例：

#### （2）雷鸟VR眼镜

输入「雷鸟VR眼镜」，直接生成：

**「开启你的无限可能宇宙」**

真好，充满想象空间。

![](img/b2f30bb24d280d934dc023986b6a5b3b.png)

#### （3）香奈儿包包

输入「包包，香奈儿」，直接生成：

「优雅，是一种态度。永恒，是一种风格」

知性、优雅、态度，都包括了~

![](img/681334b9548bd763f9d90d5331d53161.png)

#### （4）LangGPT    /     Claude

1、 输入「LangGPT，开源prompt社区，教你发挥AI最大的能力」，直接生成：

**「AI点亮创意，人人皆能发光」**

2、 来看看Claude的自我认知，直接输入「AI，Claude」

![](img/c49c334987cb7ac9ff8c0a09a994d079.png)

![](img/05be6f603c1ce52dc9372307dd36c788.png)

#### （5）李宁「赤兔」跑鞋    |     比亚迪汉EV荣耀

1、输入「运动鞋类，李宁，赤兔系列跑鞋，超越巅峰」，直接生成：

「奔跑吧，赤子！突破极限，创造传奇」

2、 输入「汽车领域，比亚迪汉EV荣耀」，再加上比亚迪的企业文化

```
- 品牌愿景
为地球降温1℃
- 品牌使命
用技术创新，满足人们对美好生活的向往
- 品牌定位
全球新能源整体解决方案开创者
- 发展理念
技术为王、创新为本
- 品牌核心价值观
竞争、务实、激情、创新
```

![](img/cf86947c1ab7f8797a173ad46e657f0a.png)

![](img/3ba33e5095b0c005e9df19de0ad2f39f.png)

#### （6）黑神话·悟空

还可以加上我们的产品特点，比如，我输入：

```
游戏，黑神话悟空
介绍：《黑神话：悟空》是由杭州游科互动科技有限公司开发，浙江出版集团数字传媒有限公司出版的西游题材单机动作角色扮演游戏。
该作以中国四大名著之一、吴承恩所著长篇小说《西游记》为背景设定，借用《西游记》自身的故事作为前传，讲述取经之后孙悟空因放弃佛位引发天庭对其再次征伐之后的故事。在游戏中，玩家将扮演一位“天命人”，为了探寻昔日传说的真相，踏上一条充满危险与惊奇的西游之路 。
重点：创下中国3A游戏的新纪录
```

直接输出：

「跨越千年，重铸神话。 你的冒险，铸就3A传奇!」

![](img/63471a55cec940864c38bf03826d9a68.png)

解读：

+   "跨越千年"呼应了《西游记》的悠久历史，激发文化自豪感。

+   "重铸神话"暗示了游戏对经典故事的创新诠释，引发好奇心。

+   "你的冒险"直接吸引玩家，强调个人参与感和探索欲。

+   "铸就3A传奇"既点明了游戏的高品质，又暗示了玩家将成为这个传奇的一部分，激发成就感和期待感。

#### （7）李佳琦推荐面膜   / 元器森林

1、输入“李佳琦推荐面膜”

2、输入“饮料，元气森林”，加上部分元气森林的简介、特性。

直接生成「品味轻盈，释放内在光彩」

![](img/f4d29d171648b25d227860d00e41ccdc.png)

![](img/5f82253f74b2796082c81e6e1dec7c7f.png)

### 实际使用(Claude 0.3 版本)

![](img/2d2ce733496ca124d9cf14c28d8f85c0.png)

![](img/3bb7f69c5dd4d05b76a1cd5fd44adb7a.png)

![](img/ee59dd298393e73421435f6c9491cbf3.png)

![](img/248f0f37f176ebe45eba292c5009dca2.png)

![](img/88be606b0f27386b7c5015a44b8f9f2d.png)

![](img/0203085f378173768db5ea78cfdbc421.png)

## （二）国内 AI版本 阿里通义 /  智谱清言 / Kimi等

> 「通义」 官网地址：https://tongyi.aliyun.com/qianwen/  （无使用门槛限制，登录即可。）

> 「Kimi」 官网地址：

> https://kimi.moonshot.cn/

> （无使用门槛限制，登录即可。）

> 「智谱」 官网地址：

> https://chatglm.cn/main/alltoolsdetail?lang=zh

> （无使用门槛限制，登录即可。）

> 注意的点

> 1、AI 生成的是HTML网页预览，本质上是网页代码，如果转为图片需要自行截图~（或使用浏览器插件等）

> 2、由于AI 模型的局限性，如果 同一对话框生成效果不佳，可以多开几个对话窗口，重新初始化进行操作。

> 3、同一对话框有长度限制，如果到达限制之后，可以重开对话框重新初始化进行操作。

> 4、生成的字体是根据你的电脑上的字体决定的，提示词中的“KingHwa_OldSong”大部分机器上没有，需要自行安装，如果没有强需求，默认系统字体可以~

> 5、如果抽象图形没有绘制，可以跟AI沟通，让它帮你补充即可~

> 这里我们以阿里的通义为例，进行演示，其它国产AI思路一样~

### 界面介绍

打开上述国产AI的首页，之后我们可以看到通义的官方界面如下（注册登录操作不细述了哈）：

![](img/47d760d31fcbef0b99159b1dc6fe4125.png)

我们直接在对话框中输入我们【第四节】第2节的咒语（提示词）。

### 提示词初始化

直接打开通义的对话框，输入提示词

![](img/24153cdbb91e8adeaa1ad7cfc9498c83.png)

初始化完成，接下来就可以直接进行使用~

### 使用教程

#### （1）初始化完成后输入

+   等待AI代入角色后，我们可以输入我们的领域和产品信息。

+   我们以刘润老师的“刘润年度演讲，进化的力量”为例，在通义给出响应后，我们输入「刘润年度演讲，进化的力量」

![](img/10c1af092a0fd13ba6ec054eae644d48.png)

#### （2）等待 AI 回复

我们可以看到，它会自行搜索相关信息，之后整合内容，进行输出

> 了解了，您提到的产品是“刘润年度演讲：进化的力量”。这是一个知识分享的活动，旨在通过演讲传递关于商业、个人成长和未来趋势的洞察。接下来我将根据情绪价值营销的方法来为您生成一句符合该产品的情绪价值营销语句。

### 情绪价值营销解读

1.  **情绪分析（意识层面）**：

+   在这个领域下，听众可能感受到的是对未来的不确定性以及对自我提升的需求。

+   选择高唤醒度的情绪如**希望、好奇心**等，这些情绪与个人成长和对未来可能性的探索密切相关。

1.  **潜意识挖掘**：

+   潜意识需求可能包括对安全、归属感以及自我实现的追求。

+   将潜意识转化为共识的主题，比如**成长、启发、转变**等，这些都是能够引起集体共鸣的主题。

1.  **人心洞察**：

+   使用系统1的表达来打动人心，意味着要触动人们的直觉和情感，而不是仅依靠逻辑论证。

+   人们渴望被激励去改变现状，并成为更好的自己。

### 情绪价值营销语句

> “启迪智慧，拥抱变革”

### 营销卡片 (HTML 代码)

> 现在我将根据上述解读创建一个情绪价值营销卡片。我们将使用莫兰迪色系作为配色方案，因为这种颜色给人以平静和思考的空间，适合于知识分享类的活动。

```
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
    &lt;title&gt;情绪价值营销 - 进化的力量&lt;/title&gt;
    &lt;style&gt;
        :root {
            --primary-color: #B6B5A7; /* 莫兰迪灰褐色 */
            --secondary-color: #9A8F8F; /* 莫兰迪灰棕色 */
            --accent-color: #C5B4A0; /* 莫兰迪淡棕色 */
            --background-color: #E8E3DE; /* 莫兰迪米色 */
            --text-color: #5B5B5B; /* 莫兰迪深灰色 */
            --light-text-color: #8C8C8C; /* 莫兰迪中灰色 */
            --divider-color: #D1CBC3; /* 莫兰迪浅灰色 */
        }
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: var(--background-color);
            font-family: &#x27;KingHwa_OldSong&#x27;, &#x27;Arial&#x27;, sans-serif;
            color: var(--text-color);
        }
        .card {
            width: 400px;
            max-height: 600px;
            background-color: #F2EDE9;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
            position: relative;
            display: flex;
            flex-direction: column;
        }
        .header {
            background-color: var(--secondary-color);
            color: #F2EDE9;
            padding: 20px;
            text-align: left;
            z-index: 1;
        }
        h1, h3 {
            margin: 0;
            font-weight: 700;
        }
        h1 { font-size: 20px; }
        h3 { font-size: 16px; }
        .content {
            padding: 30px 20px;
            flex-grow: 1;
            display: flex;
            flex-direction: column;
        }
        .word-main {
            font-size: 36px;
            color: var(--text-color);
            margin-bottom: 10px;
            position: relative;
        }
        .word-main::after {
            content: &#x27;&#x27;;
            position: absolute;
            left: 0;
            bottom: -5px;
            width: 50px;
            height: 3px;
            background-color: var(--accent-color);
        }
        .explanation {
            font-size: 18px;
            line-height: 1.6;
            text-align: left;
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        .quote {
            padding-left: 20px;
            border-left: 3px solid var(--accent-color);
        }
        .background-text {
            position: absolute;
            font-size: 150px;
            color: rgba(182, 181, 167, 0.15);
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-weight: bold;
        }
        .footer {
            padding: 20px;
            font-size: 12px;
            color: var(--light-text-color);
            text-align: right;
        }
        .divider {
            width: 100%;
            height: 1px;
            background-color: var(--divider-color);
            margin: 20px 0;
        }
        .illustration {
            margin: 20px 0;
            display: flex;
            justify-content: center;
        }
        .spark, .growth {
            fill: none;
            stroke: var(--accent-color);
            stroke-width: 2;
        }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div class=&quot;card&quot;&gt;
        &lt;div class=&quot;header&quot;&gt;
            &lt;h1&gt;情绪价值营销&lt;/h1&gt;
            &lt;h3&gt;刘润年度演讲：进化的力量&lt;/h3&gt;
        &lt;/div&gt;
        &lt;div class=&quot;divider&quot;&gt;&lt;/div&gt;
        &lt;div class=&quot;content&quot;&gt;
            &lt;div class=&quot;word-main&quot;&gt;“启迪智慧，拥抱变革”&lt;/div&gt;
            &lt;div class=&quot;illustration&quot;&gt;
                &lt;svg width=&quot;200&quot; height=&quot;200&quot; viewBox=&quot;0 0 200 200&quot;&gt;
                    &lt;!-- 思想的火花 --&gt;
                    &lt;circle class=&quot;spark&quot; cx=&quot;100&quot; cy=&quot;100&quot; r=&quot;50&quot; /&gt;
                    &lt;line class=&quot;spark&quot; x1=&quot;100&quot; y1=&quot;100&quot; x2=&quot;150&quot; y2=&quot;150&quot; /&gt;
                    &lt;line class=&quot;spark&quot; x1=&quot;100&quot; y1=&quot;100&quot; x2=&quot;150&quot; y2=&quot;50&quot; /&gt;
                    &lt;line class=&quot;spark&quot; x1=&quot;100&quot; y1=&quot;100&quot; x2=&quot;50&quot; y2=&quot;150&quot; /&gt;
                    &lt;line class=&quot;spark&quot; x1=&quot;100&quot; y1=&quot;100&quot; x2=&quot;50&quot; y2=&quot;50&quot; /&gt;
                    &lt;!-- 成长的过程 --&gt;
                    &lt;path class=&quot;growth&quot; d=&quot;M50,100 Q75,50 100,75 T150,100 Q175,150 150,125 T100,100&quot; /&gt;
                &lt;/svg&gt;
            &lt;/div&gt;
            &lt;div class=&quot;explanation&quot;&gt;
                &lt;div class=&quot;quote&quot;&gt;
                    &lt;p&gt;情绪价值：激发内在潜能，引领时代潮流。&lt;/p&gt;
                &lt;/div&gt;
            &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;background-text&quot;&gt;进化的力量&lt;/div&gt;
        &lt;div class=&quot;footer&quot;&gt;演讲 - 刘润&lt;/div&gt;
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
```

> 这段 HTML 代码定义了一个简洁而典雅的情绪价值营销卡片，它利用了莫兰迪色系的颜色，营造出一种宁静而又充满智慧的氛围，适合用来推广刘润的年度演讲活动。

#### （3）HTML格式转换

将代码复制到HTML预览网站，如(  https://www.toolhelper.cn/Html/Preview )

![](img/2ae26348160dc232f11d93149abbf15f.png)

#### （4）欣赏你的专属"情绪价值营销"卡片！

![](img/c2cd68f85c511ed0a9570ca81794e57e.png)

### 其它案例

#### （1）李宁赤兔跑鞋

![](img/2e7e9520fbefc1452497c80979a57d0c.png)

### 情绪价值营销解读

> **情绪分析（意识层面）**：

> 在运动鞋领域，尤其是对于李宁赤兔跑鞋这样的产品，消费者的情绪维度通常围绕着对速度的渴望、对健康的追求以及对自我超越的激情。高唤醒度的情绪包括了对卓越表现的欲望和对自由奔跑快乐的向往。

> **潜意识挖掘**：

> 在潜意识层面，人们购买跑鞋不仅仅是为了跑步本身，更是为了实现个人的成长与成就。它代表了一种生活方式的选择——积极向上、不断挑战自我。同时，穿着一双好的跑鞋也象征着一种身份认同，是对品质生活的追求。

> **人心洞察**：

> 通过触动人们的系统1决策机制，我们可以激发他们内心深处对自由与成长的渴望。李宁赤兔跑鞋不仅仅是一双鞋，它是你探索未知、挑战极限的伙伴。它代表着每一次脚步落下时的坚定信念，以及对更好自己的不懈追求。

### 营销语句

> “释放你的潜能，与风同行。”

### 营销卡片 (HTML 代码)

```
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh&quot;&gt;
&lt;head&gt;
&lt;meta charset=&quot;UTF-8&quot;&gt;
&lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
&lt;title&gt;情绪价值营销 - 李宁赤兔跑鞋&lt;/title&gt;
&lt;style&gt;
:root {
    --background-color: #F7F7F7; /* 清新自然系 */
    --text-color: #333;
    --sub-text-color: #555;
    --accent-color: #66CC99; /* 鲜活绿色调 */
}
body, html {
    margin: 0;
    padding: 0;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: var(--background-color);
    font-family: &quot;KingHwa_OldSong&quot;, &quot;KaiTi&quot;, &quot;SimKai&quot;, Arial, sans-serif;
    color: var(--text-color);
}
.card {
    width: auto;
    max-width: 400px;
    height: auto;
    background-color: white;
    border-radius: 20px;
    box-shadow: 0 10px 20px rgba(0,0,0,0.1);
    overflow: hidden;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
}
.header {
    text-align: left;
    width: 100%;
}
h1, h2 {
    margin: 0;
    font-weight: normal;
}
h1 {
    font-size: 24px;
    color: var(--text-color);
}
h2 {
    font-size: 18px;
    color: var(--sub-text-color);
}
.word-main {
    font-size: 32px;
    line-height: 1.2;
    color: var(--accent-color);
    margin: 20px 0;
}
.explanation {
    font-size: 16px;
    color: var(--sub-text-color);
    text-align: center;
}
.background-text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 200px;
    color: rgba(102, 204, 153, 0.05);
    z-index: -1;
    font-weight: bold;
}
&lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;div class=&quot;card&quot;&gt;
    &lt;div class=&quot;header&quot;&gt;
        &lt;h1&gt;情绪价值营销&lt;/h1&gt;
        &lt;h2&gt;李宁赤兔跑鞋&lt;/h2&gt;
    &lt;/div&gt;
    &lt;hr style=&quot;margin: 20px 0; border: 0; height: 1px; background: #ddd;&quot;&gt;
    &lt;p class=&quot;word-main&quot;&gt;“释放你的潜能，与风同行。”&lt;/p&gt;
    &lt;svg width=&quot;200&quot; height=&quot;150&quot; viewBox=&quot;0 0 200 150&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot;&gt;
        &lt;path d=&quot;M50,75 Q100,25 150,75 T200,125 Q150,125 100,75 T50,25 Z&quot; fill=&quot;#66CC99&quot;/&gt;
    &lt;/svg&gt;
    &lt;p class=&quot;explanation&quot;&gt;让每一步都成为通往更强大自我的旅程。&lt;/p&gt;
    &lt;div class=&quot;background-text&quot;&gt;Li-Ning&lt;/div&gt;
&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
```

> 这段 HTML 代码定义了一个简洁而典雅的营销卡片，使用清新自然系的颜色搭配来体现李宁赤兔跑鞋所传递的健康活力与自然和谐的品牌形象。卡片中包含了情绪价值营销的核心信息，并通过图形元素强化了品牌传达的情感诉求。

#### （2）红双喜乒乓球拍

![](img/8e5c2b84ee1ca63e32d0f7fea59967de.png)

> 了解了，红双喜乒乓球拍属于体育用品领域。现在我将根据情绪价值的定义和步骤来为这款产品创造一句营销语句。

### 情绪价值营销解读

1.  **情绪分析（意识层面）**：

+   在体育用品尤其是乒乓球拍这个细分市场中，消费者往往追求的是运动带来的快乐、胜利的喜悦以及自我提升的成就感。

+   选择高唤醒度的情绪：**快乐**、**希望**、**成就**。

1.  **潜意识挖掘**：

+   潜意识需求可能包括对卓越表现的渴望、与他人竞争时获得认可的愿望，以及通过不断练习实现个人成长的需求。

+   集体共鸣的主题可以是**成长**、**挑战自我**、**荣誉**。

1.  **人心洞察**：

+   人们购买乒乓球拍不仅仅是为了打乒乓球，更是在寻找一种途径来证明自己的能力，同时享受过程中的乐趣。

+   使用系统1的表达方式来触动人心，比如利用视觉上的动感图像或激发斗志的语言。

### 情绪价值营销语句

> “挥洒汗水，铸就荣耀，每一拍都是你的传奇。”

### 营销卡片 (HTML 代码)

```
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
    &lt;title&gt;情绪价值营销 - 红双喜乒乓球拍&lt;/title&gt;
    &lt;style&gt;
        :root {
            --background-color: #F7F9FC; /* 清新自然系 */
            --text-color: #333;
            --sub-text-color: #555;
            --highlight-color: #00A8E8; /* 明亮活力系作为强调色 */
            --divider-color: #DDE4ED;
        }
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: var(--background-color);
            font-family: &#x27;KingHwa_OldSong&#x27;, &#x27;SimKai&#x27;, sans-serif;
            color: var(--text-color);
        }
        .card {
            width: 400px;
            height: auto;
            background-color: #FAFAFA;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
            position: relative;
            display: flex;
            flex-direction: column;
        }
        .header {
            background-color: var(--highlight-color);
            color: #FAFAFA;
            padding: 20px;
            text-align: left;
            position: relative;
            z-index: 1;
        }
        h1, h3 {
            margin: 0;
            font-weight: 700;
        }
        h1 {
            font-size: 24px;
        }
        h3 {
            font-size: 18px;
        }
        .content {
            padding: 30px 20px;
            display: flex;
            flex-direction: column;
            flex-grow: 1;
        }
        .word-main {
            font-size: 26px;
            color: var(--text-color);
            margin-bottom: 10px;
            position: relative;
        }
        .word-main::after {
            content: &#x27;&#x27;;
            position: absolute;
            left: 0;
            bottom: -5px;
            width: 50px;
            height: 3px;
            background-color: var(--highlight-color);
        }
        .divider {
            width: 100%;
            height: 1px;
            background-color: var(--divider-color);
            margin: 20px 0;
        }
        .explanation {
            font-size: 18px;
            line-height: 1.6;
            text-align: left;
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        .quote {
            position: relative;
            padding-left: 20px;
            border-left: 3px solid var(--highlight-color);
        }
        .background-text {
            position: absolute;
            font-size: 100px;
            color: rgba(0, 0, 0, 0.05);
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-weight: bold;
        }
        .footer {
            padding: 20px;
            background-color: #F0F3F7;
            text-align: center;
            font-size: 14px;
            color: var(--sub-text-color);
        }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div class=&quot;card&quot;&gt;
        &lt;div class=&quot;header&quot;&gt;
            &lt;h1&gt;情绪价值营销&lt;/h1&gt;
            &lt;h3&gt;红双喜乒乓球拍&lt;/h3&gt;
        &lt;/div&gt;
        &lt;div class=&quot;divider&quot;&gt;&lt;/div&gt;
        &lt;div class=&quot;content&quot;&gt;
            &lt;div class=&quot;word-main&quot;&gt;“挥洒汗水，铸就荣耀，每一拍都是你的传奇。”&lt;/div&gt;
            &lt;svg width=&quot;300&quot; height=&quot;200&quot; viewBox=&quot;0 0 300 200&quot;&gt;
                &lt;!-- 简单的抽象图形表示乒乓球拍 --&gt;
                &lt;rect x=&quot;10&quot; y=&quot;50&quot; width=&quot;280&quot; height=&quot;100&quot; rx=&quot;15&quot; fill=&quot;#00A8E8&quot; /&gt;
                &lt;circle cx=&quot;150&quot; cy=&quot;100&quot; r=&quot;20&quot; fill=&quot;#FAFAFA&quot; /&gt;
                &lt;circle cx=&quot;150&quot; cy=&quot;100&quot; r=&quot;15&quot; stroke=&quot;#00A8E8&quot; stroke-width=&quot;2&quot; fill=&quot;none&quot; /&gt;
            &lt;/svg&gt;
            &lt;div class=&quot;explanation&quot;&gt;
                &lt;div class=&quot;quote&quot;&gt;
                    &lt;p&gt;情绪价值：&lt;br&gt;
                        触发拼搏精神与荣誉感，&lt;br&gt;
                        激励每一位运动员成为赛场上的英雄。&lt;/p&gt;
                &lt;/div&gt;
            &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;background-text&quot;&gt;红双喜&lt;/div&gt;
        &lt;div class=&quot;footer&quot;&gt;体育用品 - 乒乓球拍&lt;/div&gt;
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
```

> 这段 HTML 代码创建了一个简洁而优雅的营销卡片，它采用了清新自然系的颜色搭配，并且以红双喜乒乓球拍为核心，传达了积极向上的情绪价值信息。

#### （3）兰蔻小黑瓶眼霜

![](img/84fd80e7fc1f5b5d01086c1c285072b9.png)

> 兰蔻小黑瓶眼霜属于美容护肤领域。接下来，我将根据情绪价值的定义和步骤为这款产品创造一句营销语句。

### 情绪价值营销解读

1.  **情绪分析（意识层面）**：

+   在美容护肤领域，消费者往往追求的是美丽、年轻、自信以及被关注的感觉。

+   选择高唤醒度的情绪：**快乐**、**希望**、**自信**。

1.  **潜意识挖掘**：

+   潜意识需求可能包括对青春永驻的渴望、自我形象提升的愿望，以及在社交场合中获得他人正面评价的需求。

+   集体共鸣的主题可以是**美丽**、**自信**、**优雅**。

1.  **人心洞察**：

+   使用护肤品不仅仅是关于外观的改变，它也是个人护理的一种仪式，能够增强使用者的自信心，并让他们感到更加关爱自己。

+   通过视觉上精致的产品展示和富有诗意的语言来触动人心。

### 情绪价值营销语句

> “绽放明眸光彩，焕发自信神采。”

### 营销卡片 (HTML 代码)

```
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
    &lt;title&gt;情绪价值营销 - 兰蔻小黑瓶眼霜&lt;/title&gt;
    &lt;style&gt;
        :root {
            --background-color: #F9F5F6; /* 柔和粉彩系 */
            --text-color: #333;
            --sub-text-color: #555;
            --highlight-color: #E6B8C7; /* 柔和粉彩系作为强调色 */
            --divider-color: #DAD4D5;
        }
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: var(--background-color);
            font-family: &#x27;KingHwa_OldSong&#x27;;
            color: var(--text-color);
        }
        .card {
            width: 400px;
            height: auto;
            background-color: #FAFAFA;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
            position: relative;
            display: flex;
            flex-direction: column;
        }
        .header {
            background-color: var(--highlight-color);
            color: #FAFAFA;
            padding: 20px;
            text-align: left;
            position: relative;
            z-index: 1;
        }
        h1, h3 {
            margin: 0;
            font-weight: 700;
        }
        h1 {
            font-size: 24px;
        }
        h3 {
            font-size: 18px;
        }
        .content {
            padding: 30px 20px;
            display: flex;
            flex-direction: column;
            flex-grow: 1;
        }
        .word-main {
            font-size: 26px;
            color: var(--text-color);
            margin-bottom: 10px;
            position: relative;
        }
        .word-main::after {
            content: &#x27;&#x27;;
            position: absolute;
            left: 0;
            bottom: -5px;
            width: 50px;
            height: 3px;
            background-color: var(--highlight-color);
        }
        .divider {
            width: 100%;
            height: 1px;
            background-color: var(--divider-color);
            margin: 20px 0;
        }
        .explanation {
            font-size: 18px;
            line-height: 1.6;
            text-align: left;
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        .quote {
            position: relative;
            padding-left: 20px;
            border-left: 3px solid var(--highlight-color);
        }
        .background-text {
            position: absolute;
            font-size: 100px;
            color: rgba(0, 0, 0, 0.05);
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-weight: bold;
        }
        .footer {
            padding: 20px;
            background-color: #F0F3F7;
            text-align: center;
            font-size: 14px;
            color: var(--sub-text-color);
        }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div class=&quot;card&quot;&gt;
        &lt;div class=&quot;header&quot;&gt;
            &lt;h1&gt;情绪价值营销&lt;/h1&gt;
            &lt;h3&gt;兰蔻小黑瓶眼霜&lt;/h3&gt;
        &lt;/div&gt;
        &lt;div class=&quot;divider&quot;&gt;&lt;/div&gt;
        &lt;div class=&quot;content&quot;&gt;
            &lt;div class=&quot;word-main&quot;&gt;“绽放明眸光彩，焕发自信神采。”&lt;/div&gt;
            &lt;svg width=&quot;200&quot; height=&quot;200&quot; viewBox=&quot;0 0 200 200&quot;&gt;
                &lt;!-- 简单抽象图形表示眼霜瓶身 --&gt;
                &lt;ellipse cx=&quot;100&quot; cy=&quot;100&quot; rx=&quot;50&quot; ry=&quot;70&quot; fill=&quot;#E6B8C7&quot; /&gt;
                &lt;circle cx=&quot;100&quot; cy=&quot;100&quot; r=&quot;40&quot; fill=&quot;#FAFAFA&quot; /&gt;
                &lt;rect x=&quot;80&quot; y=&quot;120&quot; width=&quot;40&quot; height=&quot;20&quot; fill=&quot;#E6B8C7&quot; /&gt;
            &lt;/svg&gt;
            &lt;div class=&quot;explanation&quot;&gt;
                &lt;div class=&quot;quote&quot;&gt;
                    &lt;p&gt;情绪价值：&lt;br&gt;
                        触发女性对于美丽的向往与自信，&lt;br&gt;
                        让每一位用户都能展现独特的魅力。&lt;/p&gt;
                &lt;/div&gt;
            &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;background-text&quot;&gt;Lancôme&lt;/div&gt;
        &lt;div class=&quot;footer&quot;&gt;美容护肤 - 眼霜&lt;/div&gt;
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
```

> 这段 HTML 代码创建了一个干净而典雅的营销卡片，采用了柔和粉彩系的颜色搭配，以兰蔻小黑瓶眼霜为核心，传达了美丽和自信的情绪价值信息。卡片中的简单抽象图形代表了眼霜瓶身，同时配合着富有情感的文字描述，旨在吸引目标受众并激发他们的情感共鸣。

#### （4）其他案例（GPT / 智谱 / Kimi）

![](img/e43e839ce0a0af672d658d24a51c8299.png)

![](img/7b1eaf8f74dc6931208886aa61aa1025.png)

![](img/a7850a1b42c5444f251004936b65af5b.png)

![](img/6915dc28d5b791cb38e14f0ba42b0c4e.png)

![](img/ef4bc0aa4ed18a5d50156e5daa5903f1.png)

![](img/203b1f75e445b519d9e1e8626477e008.png)

# 结语

对于一些独有的方法论进行Prompt封装，通过AI能力进行放大，能够解决很多场景问题。

拿本次的「情绪价值营销」场景来说，

不仅提高了营销创意的效率,

更为重要的是,

它为营销人员提供了一个强大的头脑风暴工具！

AI生成的结果可以作为灵感的源泉,

激发人类创作者的想象力,

帮助他们突破思维定式,

探索更多可能性。

---

而对于如何发掘更有趣的场景，如何写出有意思的prompt，比较赞成刚哥说的：

> 模型输出质量与CoT强相关。

> prompt的质量与自己大脑中的CoT强相关。

> 你对一个场景的认知深度，决定着你的CoT层次。

> 技法只是表象，根子在日常

你对某个领域或场景的理解越深入，

你就越能设计出引导AI进行深度思考的提示。

这意味着，要成为一名优秀的prompt工程师，

不仅需要了解AI技术，还需要不断拓宽自己的知识面，深化对各种领域的理解。

希望，

各位小伙伴们都能挖掘自己的场景，进行更多探索~