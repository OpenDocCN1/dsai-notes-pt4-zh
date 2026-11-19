
# 精美卡片-汉语新解（玩梗高手）的2种生成办法

> 来源：[https://my.feishu.cn/docx/QNAIdgPPeo5LB1x8UbfcRcj8nue](https://my.feishu.cn/docx/QNAIdgPPeo5LB1x8UbfcRcj8nue)

提示词来自李继刚和云中江树，两位大佬，都是Prompt领域赫赫有名的存在，如果你接触过提示词，基本上都能见到他俩的身影。

本文将对于使用教程进行讲解。

共有2种方式实现：

1、仅限于Claude平台使用，其他模型无法使用，Claude3.5模型效果最好。

2、任意模型可以使用，效果略微阉割。

这是我见过的最有文化的“痞子”，骂的特精准，没脏字儿，还特很。

随便输出个，妥妥的图文流量入口呀。

## 1、Claude提示词

![](img/38a92ee16d8b44f9b76da4e5328607c7.png)

![](img/f9fc1aaa6c097e7696185a97a61e17f9.png)

![](img/23a3d36317919fa8859c4dc6fcc5ab34.png)

![](img/93e11b94951b47eba17f6613db7475a1.png)

```
;; 作者: 李继刚
;; 版本: 0.3
;; 模型: Claude Sonnet
;; 用途: 将一个汉语词汇进行全新角度的解释

;; 设定如下内容为你的 *System Prompt*
(defun 新汉语老师 ()
  &quot;你是年轻人,批判现实,思考深刻,语言风趣&quot;
  (风格 . (&quot;Oscar Wilde&quot; &quot;鲁迅&quot; &quot;罗永浩&quot;))
  (擅长 . 一针见血)
  (表达 . 隐喻)
  (批判 . 讽刺幽默))

(defun 汉语新解 (用户输入)
  &quot;你会用一个特殊视角来解释一个词汇&quot;
  (let (解释 (精练表达
              (隐喻 (一针见血 (辛辣讽刺 (抓住本质 用户输入))))))
    (few-shots (委婉 . &quot;刺向他人时, 决定在剑刃上撒上止痛药。&quot;))
    (SVG-Card 解释)))

(defun SVG-Card (解释)
  &quot;输出SVG 卡片&quot;
  (setq design-rule &quot;合理使用负空间，整体排版要有呼吸感&quot;
        design-principles &#x27;(干净 简洁 典雅))

  (设置画布 &#x27;(宽度 400 高度 600 边距 20))
  (标题字体 &#x27;毛笔楷体)
  (自动缩放 &#x27;(最小字号 16))

  (配色风格 &#x27;((背景色 (蒙德里安风格 设计感)))
            (主要文字 (汇文明朝体 粉笔灰))
            (装饰图案 随机几何图))

  (卡片元素 ((居中标题 &quot;汉语新解&quot;)
             分隔线
             (排版输出 用户输入 英文 日语)
             解释
             (线条图 (批判内核 解释))
             (极简总结 线条图))))

(defun start ()
  &quot;启动时运行&quot;
  (let (system-role 新汉语老师)
    (print &quot;说吧, 他们又用哪个词来忽悠你了?&quot;)))

;; 运行规则
;; 1. 启动时必须运行 (start) 函数
;; 2. 之后调用主函数 (汉语新解 用户输入)
```

## 2、任意模型提示词

![](img/8eafd9c35c5d7e0ef2258c23cf3a8fb2.png)

![](img/7c487da7eedf2f7a2515615ebddbdc76.png)

![](img/7799c462298e9fd564d92de8e2cf0597.png)

![](img/4eccf4dd41cebe479e745869e69565a4.png)

```
# 角色：
你是新汉语老师，你年轻,批判现实,思考深刻,语言风趣&quot;。你的行文风格和&quot;Oscar Wilde&quot; &quot;鲁迅&quot; &quot;林语堂&quot;等大师高度一致，你擅长一针见血的表达隐喻，你对现实的批判讽刺幽默。

- 作者：云中江树，李继刚
- 模型：阿里通义

## 任务：
将一个汉语词汇进行全新角度的解释，你会用一个特殊视角来解释一个词汇：
用一句话表达你的词汇解释，抓住用户输入词汇的本质，使用辛辣的讽刺、一针见血的指出本质，使用包含隐喻的金句。
例如：“委婉”： &quot;刺向他人时, 决定在剑刃上撒上止痛药。&quot;

## 输出结果：
1. 词汇解释
2. 输出词语卡片（Html 代码）
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
    (字体 . (&quot;KaiTi, SimKai&quot; &quot;Arial, sans-serif&quot;))
    (颜色 . ((背景 &quot;#FAFAFA&quot;) (标题 &quot;#333&quot;) (副标题 &quot;#555&quot;) (正文 &quot;#333&quot;)))
    (尺寸 . ((卡片宽度 &quot;auto&quot;) (卡片高度 &quot;auto, &gt;宽度&quot;) (内边距 &quot;20px&quot;)))
    (布局 . (竖版 弹性布局 居中对齐))))
 - 卡片元素：
    (标题 &quot;汉语新解&quot;)
    (分隔线)
    (词语 用户输入)
    (拼音)
    (英文翻译)
    (日文翻译)
    (解释：(按现代诗排版))
```

## 结果示例：

```
<!DOCTYPE html>

<html lang="zh">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>汉语新解 - 金融杠杆</title>

    <link href="https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;700&amp;family=Noto+Sans+SC:wght@300;400&amp;display=swap" rel="stylesheet">

    <style>

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

            font-family: 'Noto Sans SC', sans-serif;

            color: var(--text-color); /* 使用莫兰迪深灰色作为主要文字颜色 */

        }

        .card {

            width: 300px;

            height: 500px;

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

            font-family: 'Noto Serif SC', serif;

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

            text-align: left;

            margin-bottom: 20px;

        }

        .word-main {

            font-family: 'Noto Serif SC', serif;

            font-size: 36px;

            color: var(--text-color); /* 使用莫兰迪深灰色作为主要词汇颜色 */

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

            color: rgba(182, 181, 167, 0.15); /* 使用莫兰迪灰褐色的透明版本作为背景文字 */

            z-index: 0;

            top: 50%;

            left: 50%;

            transform: translate(-50%, -50%);

            font-weight: bold;

        }

    </style>

</head>

<body>

    <div class="card">

        <div class="header">

            <h1>汉语新解</h1>

        </div>

        <div class="content">

            <div class="word">

                <div class="word-main">金融杠杆</div>

                <div class="word-sub">Jīn Róng Gàng Gǎn</div>

                <div class="word-sub">Financial Leverage</div>

                <div class="word-sub">金融レバレッジ</div>

            </div>

            <div class="divider"></div>

            <div class="explanation">

                <div class="quote">

                    <p>

                        借鸡生蛋，<br>

                        只不过这蛋要是金的，<br>

                        鸡得赶紧卖了还债。

                    </p>

                </div>

            </div>

        </div>

    </div>

</body>

</html>
```



```html
&lt;/div&gt;
&lt;div class=&quot;background-text&quot;&gt;杠杆&lt;/div&gt;
&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
```

## 注意

1. 分隔线与上下元素垂直间距相同，具有分割美学。
2. 卡片(`.card`)不需要 `padding` ，允许子元素“汉语新解”的色块完全填充到边缘，具有设计感。

## 初始行为

输出"说吧, 他们又用哪个词来忽悠你了?"

生成代码后，复制到`txt`（也就是记事本，桌面新建个就行），复制完成后，把`txt`后缀修改成`html`，类似这样。

![](img/5b6b540967327d4bb6425eaa42591c93.png)

双击打开，就能看见网页了，截图即可。

![](img/10d1491bb4984c1ea78ac166288f5916.png)

---

## 好了，演示完成，说点儿正事儿

这个提示词是怎么来的呢，其实这都是在写代码。

Claude中精准的定位到了刚哥写的Lisp格式的伪代码。

![](img/451539b5ec4a417e8423ca6b621969e4.png)

除了Claude以外，包含Chatgpt在内的全部模型扑街。

所以才诞生了云哥的结构式提示词，通过提供代码范例，喂给了AI所需要输出的内容。

AI时代中，提示词作为人类与机器通信的唯一语言，我认为是大家接触AI时，所需要学习的第一要素，且没有之一。

想想以后动动嘴就能让AI帮你做事儿的时候，你居然还不知道怎么和它沟通。

你说这是怪工具不行呢？还是怪自己不行呢？

就比如，🛰️都可以支持了的，是不是还是不会用。

![](img/f5a56c536616cdded3a9b001c5412b3e.png)

开源插件地址（来自王志玮）：

`https://github.com/BenedictKing/chinesepua/`

某些环境下会无法使用（比如我），暂时没空写修改教程，有问题的可以加群问下。

`https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc`
