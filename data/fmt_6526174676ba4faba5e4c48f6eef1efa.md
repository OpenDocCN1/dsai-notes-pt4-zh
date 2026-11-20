
# 情绪价值营销卡片

```html
&lt;!DOCTYPE html&gt;
&lt;html lang="zh"&gt;
&lt;head&gt;
    &lt;meta charset="UTF-8"&gt;
    &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
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
            font-family: 'KingHwa_OldSong', 'Arial', sans-serif;
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
            content: '';
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
    &lt;div class="card"&gt;
        &lt;div class="header"&gt;
            &lt;h1&gt;情绪价值营销&lt;/h1&gt;
            &lt;h3&gt;刘润年度演讲：进化的力量&lt;/h3&gt;
        &lt;/div&gt;
        &lt;div class="divider"&gt;&lt;/div&gt;
        &lt;div class="content"&gt;
            &lt;div class="word-main"&gt;“启迪智慧，拥抱变革”&lt;/div&gt;
            &lt;div class="illustration"&gt;
                &lt;svg width="200" height="200" viewBox="0 0 200 200"&gt;
                    &lt;!-- 思想的火花 --&gt;
                    &lt;circle class="spark" cx="100" cy="100" r="50" /&gt;
                    &lt;line class="spark" x1="100" y1="100" x2="150" y2="150" /&gt;
                    &lt;line class="spark" x1="100" y1="100" x2="150" y2="50" /&gt;
                    &lt;line class="spark" x1="100" y1="100" x2="50" y2="150" /&gt;
                    &lt;line class="spark" x1="100" y1="100" x2="50" y2="50" /&gt;
                    &lt;!-- 成长的过程 --&gt;
                    &lt;path class="growth" d="M50,100 Q75,50 100,75 T150,100 Q175,150 150,125 T100,100" /&gt;
                &lt;/svg&gt;
            &lt;/div&gt;
            &lt;div class="explanation"&gt;
                &lt;div class="quote"&gt;
                    &lt;p&gt;情绪价值：激发内在潜能，引领时代潮流。&lt;/p&gt;
                &lt;/div&gt;
            &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class="background-text"&gt;进化的力量&lt;/div&gt;
        &lt;div class="footer"&gt;演讲 - 刘润&lt;/div&gt;
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
```

> 这段 HTML 代码定义了一个简洁而典雅的情绪价值营销卡片，它利用了莫兰迪色系的颜色，营造出一种宁静而又充满智慧的氛围，适合用来推广刘润的年度演讲活动。

## HTML格式转换

将代码复制到HTML预览网站，如`https://www.toolhelper.cn/Html/Preview`

![](img/2ae26348160dc232f11d93149abbf15f.png)

## 欣赏你的专属"情绪价值营销"卡片！

![](img/c2cd68f85c511ed0a9570ca81794e57e.png)

## 其它案例

### 李宁赤兔跑鞋

![](img/2e7e9520fbefc1452497c80979a57d0c.png)

## 情绪价值营销解读

### 情绪分析（意识层面）

在运动鞋领域，尤其是对于李宁赤兔跑鞋这样的产品，消费者的情绪维度通常围绕着对速度的渴望、对健康的追求以及对自我超越的激情。高唤醒度的情绪包括了对卓越表现的欲望和对自由奔跑快乐的向往。

### 潜意识挖掘

在潜意识层面，人们购买跑鞋不仅仅是为了跑步本身，更是为了实现个人的成长与成就。它代表了一种生活方式的选择——积极向上、不断挑战自我。同时，穿着一双好的跑鞋也象征着一种身份认同，是对品质生活的追求。

### 人心洞察

通过触动人们的`系统1`决策机制，我们可以激发他们内心深处对自由与成长的渴望。李宁赤兔跑鞋不仅仅是一双鞋，它是你探索未知、挑战极限的伙伴。它代表着每一次脚步落下时的坚定信念，以及对更好自己的不懈追求。

## 营销语句

> “释放你的潜能，与风同行。”

## 营销卡片 (HTML 代码)
