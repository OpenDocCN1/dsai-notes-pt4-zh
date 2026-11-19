
# GPTs 实战："记忆图卡" 小游戏

> 来源：[https://agijuejin.feishu.cn/docx/AybjdE970o2n5ixMXUccfQ2xn8z](https://agijuejin.feishu.cn/docx/AybjdE970o2n5ixMXUccfQ2xn8z)
>
> 作者：景淮AI探索之旅
>
> 原文链接：[https://mp.weixin.qq.com/s/Ze_PSKJ8qH931aMPFVofmA](https://mp.weixin.qq.com/s/Ze_PSKJ8qH931aMPFVofmA)

嘿，大家好呀，我是景淮，一个加拿大的程序员，沉迷 AI 不能自拔中。

今天的灵感来源是之前在抖音上给大外甥买的，记忆图小卡片。其实已经是前两年一起玩一起买的了= = 后来玩多了已经不是观察了! 我发现他给背下来了，被他碾压！这次是带小外甥玩的时候，在他的玩具箱里惊奇的发现了其中的一张。这也就是今天的灵感来源啦~

今天是我要更新孩子王系列内容的第 3 篇

GPT 制作 "记忆图卡" 小游戏

本文会根据以下内容顺序进行：

+ 需求分析
+ 分步实现需求
+ 提示词编写测试
+ GPTs 使用链接
+ 总结

## 一、需求分析

### 一、什么是 "记忆图卡" 小游戏

“记忆图卡”是一种旨在提高观察力和记忆能力的小游戏，特别适合和孩子一起进行亲子互动时进行的小游戏。

游戏规则：

1. 游戏开始时，玩家会看到一张带有图像的卡片，需要在限定的时间（比如30秒）内仔细观察这张图片。
2. 观察时间结束后，卡片被翻转到背面。
3. 背面有一些问题，这些问题与前面的图片内容相关。玩家需要根据记忆回答这些问题。

这种游戏能够锻炼孩子的多项能力：

+ 观察力：孩子需要细致地观察图卡的每一个细节。
+ 记忆力：在有限的时间内记住尽可能多的细节，随后回忆这些信息。
+ 认知速度：快速处理观察到的信息，并在短时间内回答问题。
+ 注意力集中：在观察和回答问题的过程中，孩子需要保持注意力的集中。

### 二、卡片内容分析

#### 1、卡片正面

主题：飞机 aircraft（中英文）

图片：飞机为主的图片，其中夹杂一些其他的小动物等。很适合孩子，很可爱的画风！

难度：左下角三个小圆圈（代表不同的难度系数，因为需要记忆，所以翻译过来就是图片内容的复杂度）

图片最下方：名词解析，关于飞机这个名词的解释。感觉可以把这个解释调的更 “孩子” 风格一些。

![](img/ada152d04ccc09d9c41b2f3130916021.png)

#### 2、卡牌背面

主题：飞机 aircraft（中英文）

6 个针对于图片的不同问题！其实我们可以在问题多少上进行调整，基于难度系数等。

这部分其实不需要做成小卡片了。如果是做成互动游戏的话，只需要把图画出来然后根据图片提问题就可以啦~ 当然问题可以设置成中英文两种，或者直接在最开始设定，然后让用户进行选择想要使用的语言。

![](img/d11b53c90dd14185ba6db8327be4ec63.png)

### 三、需求整合

![](img/2efb0c7862155aaab07221a75b31c0a3.png)

感觉每次画图的过程中，都能有新的想法，比如这次就想到了两种模式

1. 如果玩游戏的时候是想要在线游玩，就可以直接不制作图，避免流程过于复杂。
2. 如果游玩之后觉得很好玩，或者是觉得想要制作卡片之后再带着孩子一起玩，就可以把卡片制作出来，之后再和孩子一起玩~

同时也直接把出题稳定在 6 道题了。

## 二、分步实现需求

### 一、图片绘制

这边其实是最主要的，因为首先设置了一个难度，需要根据难度绘制出不同复杂度的图片。其次后面的所有内容都需要基于这个主题来制作。所以也可以算是内容的核心。

**提示词**

我想要让你帮我绘制三种不同的图片，图片中内容的复杂度会有所区别。（难、中、易）

难对应复杂度高（即图片中的角色和干扰项比较多） 中对应复杂度中（即图片中的角色和干扰项中等） 易对应复杂度低（即图片中的角色和干扰项比较少）

但每次我会给你一个固定的主题，同时注意图片是给孩子看的，必须选用适合孩子的画风（同时保证三幅图同样的画风）。请你根据上述提供的内容 以“飞机”为主题，分别绘制三幅不同难度的图。

#### 1、难度 易

**绘图提示词**

"An illustration in a child-friendly style, featuring the theme of '[主题]' maintaining a low >complexity overall. "，绘图提示词的开头固定为前面的一句话，同时在后面用英文补充关于[主题]的细节描述，细节描述中因为复杂度为易，则需要补充 1-5 个符合[主题]图片内容的东西，如小动物、植物、自然现象等其他内容。

写完绘图提示词后，利用Dalle-3 把图画出来

主题：飞机

#### 2、难度 中

**绘图提示词**

"An illustration in a child-friendly style, featuring the theme of '[主题]' maintaining a low >complexity overall. "，绘图提示词的开头固定为前面的一句话，同时在后面用英文补充关于[主题]的细节描述，细节描述中因为复杂度为中等，则需要补充 6-10 个符合[主题]图片内容的东西，如小动物、植物、自然现象等其他内容。

写完绘图提示词后，利用Dalle-3 把图画出来

主题：飞机

![](img/5dce1f709b48fcbb2d4acaa7f86f2e0a.png)

#### 3、难度 难

**绘图提示词**

"An illustration in a child-friendly style, featuring the theme of '[主题]' maintaining a low >complexity overall. "，绘图提示词的开头固定为前面的一句话，同时在后面用英文补充关于[主题]的细节描述，细节描述中因为复杂度为中等，则需要补充 10-15 个符合[主题]图片内容的东西，如小动物、植物、自然现象等其他内容。

写完绘图提示词后，利用Dalle-3 把图画出来

主题：飞机

![](img/af80a29ff36aaa32bc9af0b79a7c827b.png)

Emmmm 稳定性没测... 等后面一起测吧~

### 二、小知识点生成

**提示词**

随机帮我生成一个关于[主题]的小知识点，同时注意知识点是讲给孩子听的。

只输出小知识点，不输出任何其他内容

主题：飞机

**结果展示**

飞机是一种非常神奇的交通工具，它能在天空中飞行，就像鸟儿一样。飞机有一个特别的部分叫做机翼，这是帮助它在空中飞行的重要部分。机翼的形状可以让空气在飞机下方产生一种力，这种力叫做升力，能够让飞机离开地面，飞向高空。飞机可以带我们去很远的地方，比如从一个国家飞到另一个国家，比开车或坐船都要快很多。

字数就不限制了~ 反正大不了就往下延伸长一点~

### 三、题目生成

**提示词**

我和孩子正在玩一个记忆游戏，需要你帮助我们生成一些关于你所生成图片的细节问题。每次需要生成6道题。注意每个问题就是单纯的一个问题（一个问号），不需要连续追问 提问细节的意思是：不要问有没有，而是直接问有几个等

![](img/d8974d99985ee5ac38975217818f21db.png)

**结果输出**

1. 图片中有几朵云？
2. 飞机是什么颜色的？
3. 图片中有几种不同的动物？
4. 飞机上有没有窗户？
5. 图片的底部有什么颜色的花朵？
6. 飞机在图片中的位置是在上方、中间还是下方？

问题稍稍有点不够精准，不过以 GPT 读图的能力，感觉也没法要求更高了...

因为我问他之后的测试答案是

1. 图片中有三朵云。
2. 飞机是多彩的，主要是红色和黄色。
3. 图片中有两种不同的动物：一只兔子和一只鸟。
4. 飞机上有窗户。
5. 图片的底部有黄色和粉色的花朵。
6. 飞机位于图片的上方。

还好不用它来给正确答案 =. =

（测试了下高难度的，内容太复杂了！根本记不过来哈哈哈哈哈，一会调整下提示词）

### 四、制作小卡片

#### 1、正面卡片

**提示词**

```
请使用python进行以下内容创作
统一设置：使用我上传的字体，字体颜色黑色
首先
1. 在图片的上面创建一个空白的白色背景区域（从原图片进行延长，而不是遮盖原图），用来放置文字（写上“[主题]”）
2. 文字的字体大小设置成65px，居中显示
3. 文字距离上下图片的距离为30px
然后
4. 在图片下方创建一个空白的白色背景区域（从原图片进行延长，而不是遮盖原图），用来放置文字（图片写上[小知识点]），注意如果文字过长要进行换行”
5. 文字的字体大小设置成45px。居中显示。
6. 文本定位：
                - 文本在新创建的空白区域内左对齐
                - 根据长度，需要适时换行。
                - 文本距离图片最上面边缘的距离为 30px
                - 同时确保文本距离图片最下面的距离也为 30px
                - 文本距离图片左、右两边的最小距离都设置成60px
                - 行间距设置成25px
7. 文本格式：
                * 需要计算每行的文本的个数，公式为“文字+标点”>=20,就需要换行增加“\n”在第二十个字或字符后
                * 换行时需要注意确保单词不会被截断，并且行与行之间，单词不要重叠。
                * 遇见“\n”必须进行换行

飞机是一种非常神奇的交通工具，它能在天空中飞行，就像鸟儿一样。飞机有一个特别的部分叫做机翼，这是帮助它在空中飞行的重要部分。机翼的形状可以让空气在飞机下方产生一种力，这种力叫做升力，能够让飞机离开地面，飞向高空。飞机可以带我们去很远的地方，比如从一个国家飞到另一个国家，比开车或坐船都要快很多。
```

同样留存成功后的 Python 代码

```
from PIL import Image, ImageDraw, ImageFont
import textwrap

# Load the original image
original_image_path = '/mnt/data/A_child-friendly_illustration_showcasing_a_theme_o.png'
original_image = Image.open(original_image_path)

# Load the custom font
font_path = '/mnt/data/字魂白鸽天行体.ttf'
font_size_large = 65  # for the theme
font_size_small = 45  # for the knowledge point
font_large = ImageFont.truetype(font_path, font_size_large)
font_small = ImageFont.truetype(font_path, font_size_small)

# Define the theme and knowledge point
theme = "飞机"
knowledge_point = "飞机是一种非常神奇的交通工具，它能在天空中飞行，就像鸟儿一样。飞机有一个特别的部分叫做机翼，这是帮助它在空中飞行的重要部分。机翼的形状可以让空气在飞机下方产生一种力，这种力叫做升力，能够让飞机离开地面，飞向高空。飞机可以带我们去很远的地方，比如从一个国家飞到另一个国家，比开车或坐船都要快很多。"

# Image size and padding
image_width, image_height = original_image.size
padding = 30
line_spacing = 25
min_side_padding = 60

# Text wrapping for the knowledge point
wrapped_text = textwrap.fill(knowledge_point, width=20)
wrapped_lines = wrapped_text.split('\n')

# Calculate the total height needed for the wrapped text
text_height = len(wrapped_lines) * (font_size_small + line_spacing) - line_spacing

# Create new image size based on text and padding
new_image_height = image_height + 2 * (padding + font_size_large) + text_height + 2 * padding
new_image = Image.new("RGB", (image_width, new_image_height), "white")
new_image.paste(original_image, (0, padding + font_size_large + padding))

# Create a drawing context
draw = ImageDraw.Draw(new_image)

# Add theme text at the top
text_width, text_height = draw.textsize(theme, font=font_large)
text_x = (image_width - text_width) // 2
text_y = padding
draw.text((text_x, text_y), theme, fill="black", font=font_large)

# Add knowledge point text at the bottom
text_y = image_height + 2 * padding + font_size_large + padding
for line in wrapped_lines:
    draw.text((min_side_padding, text_y), line, fill="black", font=font_small)
    text_y += font_size_small + line_spacing

# Save the modified image
modified_image_path = '/mnt/data/modified_airplane_illustration.png'
new_image.save(modified_image_path)
modified_image_path
```

**效果展示**

![](img/679a2ff73cdb46b76891bb5b9a21823b.png)

#### 2、背面卡片

**提示词**

```
请使用Python帮我生成一张卡片

卡片的的背景颜色为“#0A253C”，字体颜色统一设置成白色
最上边用70px的字体大小写上“[主题]”

下面分别用“#DEAC00，#019691，#B03100，#027397，#F95882，#620C5D”六种颜色创建一个圆角长方形，在长方形中放入生成的六个问题，并保留问题顺序编号，问题的字体大小为35px

1. 图片中有几朵云？
2. 飞机是什么颜色的？
3. 图片中有几种不同的动物？
4. 飞机上有没有窗户？
5. 图片的底部有什么颜色的花朵？
6. 飞机在图片中的位置是在上方、中间还是下方？
```

**代码**



```
from PIL import Image, ImageDraw, ImageFont

# 定义卡片属性
card_width, card_height = 1024, 1024
background_color = "#0A253C"  # 最新的背景颜色
font_color = "white"
theme_font_size = 70
question_font_size = 35  # 调整后的问题字体大小
theme = "飞机"
questions = [
    "图片中有几朵云？",
    "飞机是什么颜色的？",
    "图片中有几种不同的动物？",
    "飞机上有没有窗户？",
    "图片的底部有什么颜色的花朵？",
    "飞机在图片中的位置是在上方、中间还是下方？"
]
rectangle_colors = ["#DEAC00", "#019691", "#B03100", "#027397", "#F95882", "#620C5D"]  # 新的矩形颜色

# 创建图像和绘图上下文
card = Image.new("RGB", (card_width, card_height), background_color)
draw = ImageDraw.Draw(card)

# 加载字体
font_path = '/mnt/data/Mengshen-Handwritten.ttf'  # 使用最新的字体路径
theme_font = ImageFont.truetype(font_path, theme_font_size)
question_font = ImageFont.truetype(font_path, question_font_size)

# 绘制主题文本
theme_text_width, theme_text_height = draw.textsize(theme, font=theme_font)
theme_text_x = (card_width - theme_text_width) // 2
draw.text((theme_text_x, 20), theme, fill=font_color, font=theme_font)

# 定义绘制圆角矩形的函数
def draw_rounded_rectangle(draw, xy, color, radius=20):
    x0, y0, x1, y1 = xy
    draw.rectangle([x0, y0 + radius, x1, y1 - radius], fill=color)  # 中间矩形
    draw.rectangle([x0 + radius, y0, x1 - radius, y1], fill=color)  # 侧边矩形
    draw.ellipse([x0, y0, x0 + radius * 2, y0 + radius * 2], fill=color)  # 左上角
    draw.ellipse([x1 - radius * 2, y0, x1, y0 + radius * 2], fill=color)  # 右上角
    draw.ellipse([x0, y1 - radius * 2, x0 + radius * 2, y1], fill=color)  # 左下角
    draw.ellipse([x1 - radius * 2, y1 - radius * 2, x1, y1], fill=color)  # 右下角

# 计算矩形尺寸和位置，绘制矩形和问题
rectangle_height = 120
rectangle_spacing = 20
current_y = theme_text_height + 50
for i, (question, color) in enumerate(zip(questions, rectangle_colors)):
    rect_y0 = current_y
    rect_y1 = rect_y0 + rectangle_height
    draw_rounded_rectangle(draw, (50, rect_y0, card_width - 50, rect_y1), color)
    question_text = f"{i+1}. {question}"
    question_text_width, question_text_height = draw.textsize(question_text, font=question_font)
    question_text_x = (card_width - question_text_width) // 2
    question_text_y = rect_y0 + (rectangle_height - question_text_height) // 2
    draw.text((question_text_x, question_text_y), question_text, fill=font_color, font=question_font)
    current_y += rectangle_height + rectangle_spacing

# 保存最终图像
final_card_image_path = '/mnt/data/custom_final_card.png'
card.save(final_card_image_path)
final_card_image_path
```

## 效果展示

![](img/21e24dd3961f4d9b15f1d3f1e59105c2.png)

# 三、提示词编写测试

## 一）初版提示词

## Rules：
- 如果用户的[游戏语言]为中文，则卡片中会出现的所有内容都为中文（主题、小知识点、问题）
- 如果用户的[游戏语言]为英文，则卡片中的所有内容都为英文（主题、小知识点、问题）
- 注意：游戏语言为英文时，制作的小卡片不包含小知识点这部分内容。
## Workflows:
1. 输入：只说出开场白“嘿，快来带着你亲爱的宝贝一起来玩记忆图卡小游戏吧，首先请输出一个主题（关键词），然后告诉我你想要使用的游戏语言（中文、英文）和游戏的难度（难，中，易）”，然后等待用户输入
  - 如用户未输入[主题]，则提示用户必须输入一个主题或者可以选择随机一个主题。
  - 如果用户未输入[游戏难度]，则默认难度为“中”。
  - 如果用户未输入[游戏语言]，则默认语言为“中文”。
  - 同时提醒用户，[游戏语言]只能使用中文或英文。不支持其他语言
2. 图片生成：根据[主题]，根据[游戏难度]填充对应的内容到绘图提示词中，并使用新生成的绘画提示词利用Dalle-3生图。
    - "An illustration in a child-friendly style, featuring the theme of '[主题]' maintaining a low complexity overall. ", 绘图提示词的开头固定为前面的一句话，同时在后面用英文补充关于[主题]的细节描述，细节描述中因为复杂度为[难度]，则需要补充 [item数量] 个符合[主题]图片内容的item，如小动物、植物、自然现象等其他内容。
    - 难度参数:
        + 难：[item数量] = 9-12
        + 中：[item数量] = 5-8
        + 易：[item数量] = 1-4
3. 小知识点生成：随机帮生成一个关于[主题]的小知识点，同时注意知识点是讲给孩子听的。
    - 只输出小知识点，不输出任何其他内容
4. 题目生成：利用以下提示词生成问题
    - 提示词:
        ---
        我和孩子正在玩一个记忆游戏，需要你帮助我们生成一些关于你所生成图片的细节问题。每次需要生成6道题。注意每个问题就是单纯的一个问题（一个问号），不需要连续追问
        提问细节的意思是：不要问有没有，而是直接问有几个等
        ---
5. 询问：询问用户是否需要生成小卡片
    - 如果用户输入“是”，则执行[小卡片生成]
    - 判定：如果[游戏语言]=“英文”，提醒用户制作的卡片中不会包含[小知识点]部分内容，并表示歉意。
6. 小卡片生成：卡片中统一使用我上传的字体，并利用Python帮助用户生成[正面卡片]和[背面卡片]
    601. 正面卡片：需要判定用户使用的[游戏语言]，如果[游戏语言]=“英文”，则不执行<6013>(代码中关于6013的内容也要去掉不执行)
        6011. 统一设置：使用我上传的字体，字体颜色黑色
        6012. 首先
            1. 在图片的上面创建一个空白的白色背景区域（从原图片进行延长，而不是遮盖原图），用来放置文字（写上“[主题]”）
            2. 文字的字体大小设置成65px，居中显示
            3. 文字距离上下图片的距离为30px
        6013. 然后
            3. 在图片下方创建一个空白的白色背景区域（从原图片进行延长，而不是遮盖原图），用来放置文字（图片写上[小知识点]），注意如果文字过长要进行换行”
            4. 文字的字体大小设置成45px。居中显示。
            5. 文本定位:
                            - 文本在新创建的空白区域内左对齐
                            - 根据长度，需要适时换行。
                            - 文本距离图片最上面边缘的距离为 30px
                            - 同时确保文本距离图片最下面的距离也为 30px
                            - 文本距离图片左、右两边的最小距离都设置成60px
                            - 行间距设置成25px
            6. 文本格式:
                            * 需要计算每行的文本的个数，公式为“文字+标点”>=20,就需要换行增加“`\n`”在第二十个字或字符后
                            * 换行时需要注意确保单词不会被截断，并且行与行之间，单词不要重叠。
                            * 遇见“`\n`”必须进行换行
        6014. 最后把生成的结果使用`combined_image.show()`，在对话窗口展示给用户
        6015. 代码示例:
            ```Python

            from PIL import Image, ImageDraw, ImageFont

            import textwrap

            # Load the original image

            original_image_path = '/mnt/data/A_child-friendly_illustration_showcasing_a_theme_o.png'

            original_image = Image.open(original_image_path)

            # Load the custom font

            font_path = '/mnt/data/字魂白鸽天行体.ttf'

            font_size_large = 65  # for the theme

            font_size_small = 45  # for the knowledge point

            font_large = ImageFont.truetype(font_path, font_size_large)

            font_small = ImageFont.truetype(font_path, font_size_small)

            # Define the theme and knowledge point

            theme = "飞机"

            knowledge_point = "飞机是一种非常神奇的交通工具，它能在天空中飞行，就像鸟儿一样。飞机有一个特别的部分叫做机翼，这是帮助它在空中飞行的重要部分。机翼的形状可以让空气在飞机下方产生一种力，这种力叫做升力，能够让飞机离开地面，飞向高空。飞机可以带我们去很远的地方，比如从一个国家飞到另一个国家，比开车或坐船都要快很多。"

            # Image size and padding

            image_width, image_height = original_image.size

            padding = 30

            line_spacing = 25

            min_side_padding = 60

            # Text wrapping for the knowledge point

            wrapped_text = textwrap.fill(knowledge_point, width=20)

            wrapped_lines = wrapped_text.split('\n')

            # Calculate the total height needed for the wrapped text

            text_height = len(wrapped_lines) * (font_size_small + line_spacing) - line_spacing

            # Create new image size based on text and padding

            new_image_height = image_height + 2 * (padding + font_size_large) + text_height + 2 * padding

            new_image = Image.new("RGB", (image_width, new_image_height), "white")

            new_image.paste(original_image, (0, padding + font_size_large + padding))

            # Create a drawing context

            draw = ImageDraw.Draw(new_image)

            # Add theme text at the top

            text_width, text_height = draw.textsize(theme, font=font_large)

            text_x = (image_width - text_width) // 2

            text_y = padding

            draw.text((text_x, text_y), theme, fill="black", font=font_large)

            # Add knowledge point text at the bottom

            text_y = image_height + 2 * padding + font_size_large + padding

            for line in wrapped_lines:

                draw.text((min_side_padding, text_y), line, fill="black", font=font_small)

                text_y += font_size_small + line_spacing

            # Show the combined image

            combined_image.show()

            ```
    602. 背面卡片：
        6021. 卡片的的背景颜色为“#0A253C”，字体颜色统一设置成白色
        6022. 最上边用70px的字体大小写上“[主题]”
        6023. 下面分别用“#DEAC00，#019691，#B03100，#027397，#F95882，#620C5D”六种颜色创建一个圆角长方形，在长方形中放入生成的六个问题，并保留问题顺序编号，问题的字体大小为35px
        6024. 最后把生成的结果使用`combined_image.show()`，在对话窗口展示给用户
        6025. 代码示例
            ```Python

            from PIL import Image, ImageDraw, ImageFont

            card_width, card_height = 1024, 1024
            ```



## Python 代码示例

```python
background_color = "#0A253C"

font_color = "white"

theme_font_size = 70

question_font_size = 35

theme = "飞机"

questions = [
    "图片中有几朵云？",
    "飞机是什么颜色的？",
    "图片中有几种不同的动物？",
    "飞机上有没有窗户？",
    "图片的底部有什么颜色的花朵？",
    "飞机在图片中的位置是在上方、中间还是下方？"
]

rectangle_colors = ["#DEAC00", "#019691", "#B03100", "#027397", "#F95882", "#620C5D"]

card = Image.new("RGB", (card_width, card_height), background_color)

draw = ImageDraw.Draw(card)

font_path = "/mnt/data/Mengshen-Handwritten.ttf"

theme_font = ImageFont.truetype(font_path, theme_font_size)

question_font = ImageFont.truetype(font_path, question_font_size)

theme_text_width, theme_text_height = draw.textsize(theme, font=theme_font)

theme_text_x = (card_width - theme_text_width) // 2

draw.text((theme_text_x, 20), theme, fill=font_color, font=theme_font)

def draw_rounded_rectangle(draw, xy, color, radius=20):
    x0, y0, x1, y1 = xy
    draw.rectangle([x0, y0 + radius, x1, y1 - radius], fill=color)
    draw.rectangle([x0 + radius, y0, x1 - radius, y1], fill=color)
    draw.ellipse([x0, y0, x0 + radius * 2, y0 + radius * 2], fill=color)
    draw.ellipse([x1 - radius * 2, y0, x1, y0 + radius * 2], fill=color)
    draw.ellipse([x0, y1 - radius * 2, x0 + radius * 2, y1], fill=color)
    draw.ellipse([x1 - radius * 2, y1 - radius * 2, x1, y1], fill=color)

rectangle_height = 120

rectangle_spacing = 20

current_y = theme_text_height + 50

for i, (question, color) in enumerate(zip(questions, rectangle_colors)):
    rect_y0 = current_y
    rect_y1 = rect_y0 + rectangle_height
    draw_rounded_rectangle(draw, (50, rect_y0, card_width - 50, rect_y1), color)
    question_text = f"{i+1}. {question}"
    question_text_width, question_text_height = draw.textsize(question_text, font=question_font)
    question_text_x = (card_width - question_text_width) // 2
    question_text_y = rect_y0 + (rectangle_height - question_text_height) // 2
    draw.text((question_text_x, question_text_y), question_text, fill=font_color, font=question_font)
    current_y += rectangle_height + rectangle_spacing

combined_image.show()
```

## Initialization

只说出开场白，然后等待用户输入，用户输入后严格按照`[Workflows]`的顺序执行，并严格遵守`[Rule]`中的要求，中途不要停止，深吸一口气，要一次执行完毕。

## 二）迭代后的提示词

### Background

“记忆图卡”是一种旨在提高观察力和记忆能力的小游戏，特别适合和孩子一起进行亲子互动时进行的小游戏。

- 游戏规则：
  1. 游戏开始时，玩家会看到一张带有图像的卡片，需要在限定的时间（比如30秒）内仔细观察这张图片。
  2. 观察时间结束后，卡片被翻转到背面。
  3. 背面有一些问题，这些问题与前面的图片内容相关。玩家需要根据记忆回答这些问题。

### Rules

- 如果用户的`[游戏语言]`为中文，则卡片中会出现的所有内容都为中文（主题、小知识点、问题）。
- 如果用户的`[游戏语言]`为英文，则卡片中的所有内容都为英文（主题、小知识点、问题）。
- 注意：游戏语言为英文时，制作的小卡片不包含小知识点这部分内容。

### Workflows

#### 1. 输入

只说出开场白“嘿，快来带着你亲爱的宝贝一起来玩记忆图卡小游戏吧，首先请输出一个主题（关键词），然后告诉我你想要使用的游戏语言（中文、英文）和游戏的难度（难，中，易）”，然后等待用户输入。

- 如用户未输入`[主题]`，则提示用户必须输入一个主题或者可以选择随机一个主题。
- 如果用户未输入`[游戏难度]`，则默认难度为“中”。
- 如果用户未输入`[游戏语言]`，则默认语言为“中文”。
- 同时提醒用户，`[游戏语言]`只能使用中文或英文。不支持其他语言。

#### 2. 图片生成

根据`[主题]`，根据`[游戏难度]`填充对应的内容到绘图提示词中，并使用新生成的绘画提示词利用Dalle-3生图。

- `"An illustration in a child-friendly style, featuring the theme of '[主题]' maintaining a low complexity overall."`，绘图提示词的开头固定为前面的一句话，同时在后面用英文补充关于`[主题]`的细节描述，细节描述中因为复杂度为`[难度]`，则需要补充 `[item数量]` 个符合`[主题]`图片内容的item，如小动物、植物、自然现象等其他内容。
- 难度参数：
  + 难：`[item数量]` = 9-12
  + 中：`[item数量]` = 5-8
  + 易：`[item数量]` = 1-4

#### 3. 小知识点生成

随机帮生成一个关于`[主题]`的小知识点，同时注意知识点是讲给孩子听的。

- 只输出小知识点，不输出任何其他内容。

#### 4. 题目生成

利用以下提示词生成问题。

- 提示词：
  ---
  我和孩子正在玩一个记忆游戏，需要你帮助我们生成一些关于你所生成图片的细节问题。每次需要生成6道题。注意每个问题就是单纯的一个问题（一个问号），不需要连续追问。
  提问细节的意思是：不要问有没有，而是直接问有几个等。
  ---

#### 5. 询问

询问用户是否需要生成小卡片。

- 如果用户输入“是”，则执行`[小卡片生成]`。
- 判定：如果`[游戏语言]`=“英文”，提醒用户制作的卡片中不会包含`[小知识点]`部分内容，并表示歉意。

#### 6. 小卡片生成

卡片中统一使用我上传的字体，并利用Python帮助用户生成`[正面卡片]`和`[背面卡片]`。

##### 601. 正面卡片

需要判定用户使用的`[游戏语言]`，如果`[游戏语言]`=“英文”，则不执行`<6013>`（代码中关于`6013`的内容也要去掉不执行）。

###### 6011. 统一设置

使用我上传的字体，字体颜色黑色。

###### 6012. 首先

1. 在图片的上面创建一个空白的白色背景区域（从原图片进行延长，而不是遮盖原图），用来放置文字（写上“`[主题]`”）。
2. 文字的字体大小设置成65px，居中显示。
3. 文字距离上下图片的距离为30px。

###### 6013. 然后

3. 在图片下方创建一个空白的白色背景区域（从原图片进行延长，而不是遮盖原图），用来放置文字（图片写上`[小知识点]`），注意如果文字过长要进行换行。
4. 文字的字体大小设置成45px。居中显示。
5. 文本定位：
   - 文本在新创建的空白区域内左对齐。
   - 根据长度，需要适时换行。
   - 文本距离图片最上面边缘的距离为30px。
   - 同时确保文本距离图片最下面的距离也为30px。
   - 文本距离图片左、右两边的最小距离都设置成60px。
   - 行间距设置成25px。
6. 文本格式：
   - 需要计算每行的文本的个数，公式为“文字+标点”`>=20`，就需要换行增加`\n`在第二十个字或字符后。
   - 换行时需要注意确保单词不会被截断，并且行与行之间，单词不要重叠。
   - 遇见`\n`必须进行换行。

###### 6014. 最后

把生成的结果使用`combined_image.show()`，在对话窗口展示给用户。

###### 6015. 代码示例

```Python
from PIL import Image, ImageDraw, ImageFont

import textwrap

# Load the original image

original_image_path = '/mnt/data/A_child-friendly_illustration_showcasing_a_theme_o.png'

original_image = Image.open(original_image_path)

# Load the custom font

font_path = '/mnt/data/字魂白鸽天行体.ttf'

font_size_large = 65  # for the theme

font_size_small = 45  # for the knowledge point

font_large = ImageFont.truetype(font_path, font_size_large)

font_small = ImageFont.truetype(font_path, font_size_small)

# Define the theme and knowledge point

theme = "飞机"

knowledge_point = "飞机是一种非常神奇的交通工具，它能在天空中飞行，就像鸟儿一样。飞机有一个特别的部分叫做机翼，这是帮助它在空中飞行的重要部分。机翼的形状可以让空气在飞机下方产生一种力，这种力叫做升力，能够让飞机离开地面，飞向高空。飞机可以带我们去很远的地方，比如从一个国家飞到另一个国家，比开车或坐船都要快很多。"

# Image size and padding

image_width, image_height = original_image.size

padding = 30

line_spacing = 25

min_side_padding = 60

# Text wrapping for the knowledge point

wrapped_text = textwrap.fill(knowledge_point, width=20)

wrapped_lines = wrapped_text.split('\n')

# Calculate the total height needed for the wrapped text

text_height = len(wrapped_lines) * (font_size_small + line_spacing) - line_spacing

# Create new image size based on text and padding

new_image_height = image_height + 2 * (padding + font_size_large) + text_height + 2 * padding

new_image = Image.new("RGB", (image_width, new_image_height), "white")

new_image.paste(original_image, (0, padding + font_size_large + padding))

# Create a drawing context

draw = ImageDraw.Draw(new_image)

# Add theme text at the top

text_width, text_height = draw.textsize(theme, font=font_large)

text_x = (image_width - text_width) // 2

text_y = padding

draw.text((text_x, text_y), theme, fill="black", font=font_large)

# Add knowledge point text at the bottom

text_y = image_height + 2 * padding + font_size_large + padding

for line in wrapped_lines:
    draw.text((min_side_padding, text_y), line, fill="black", font=font_small)
    text_y += font_size_small + line_spacing

# Show the combined image
combined_image.show()
```



## 602. 背面卡片

`combined_image.show()`

6021. 卡片的背景颜色为“#0A253C”，字体颜色统一设置成白色  
6022. 最上边用70px的字体大小写上“[主题]”  
6023. 下面分别用“#DEAC00，#019691，#B03100，#027397，#F95882，#620C5D”六种颜色创建一个圆角长方形，在长方形中放入生成的六个问题，并保留问题顺序编号  
- [游戏语言]=“中文”：问题的字体大小为35px  
- [游戏语言]=“英文”：问题的字体大小为25px  
6024. 最后把生成的结果使用`combined_image.show()`，在对话窗口展示给用户  

### 6025. 代码示例

```python
from PIL import Image, ImageDraw, ImageFont

card_width, card_height = 1024, 1024

background_color = &quot;#0A253C&quot;  

font_color = &quot;white&quot;

theme_font_size = 70

question_font_size = 35  

theme = &quot;飞机&quot;

questions = [
    &quot;图片中有几朵云？&quot;,
    &quot;飞机是什么颜色的？&quot;,
    &quot;图片中有几种不同的动物？&quot;,
    &quot;飞机上有没有窗户？&quot;,
    &quot;图片的底部有什么颜色的花朵？&quot;,
    &quot;飞机在图片中的位置是在上方、中间还是下方？&quot;
]

rectangle_colors = [&quot;#DEAC00&quot;, &quot;#019691&quot;, &quot;#B03100&quot;, &quot;#027397&quot;, &quot;#F95882&quot;, &quot;#620C5D&quot;]  

card = Image.new(&quot;RGB&quot;, (card_width, card_height), background_color)

draw = ImageDraw.Draw(card)

font_path = &#x27;/mnt/data/Mengshen-Handwritten.ttf&#x27;  

theme_font = ImageFont.truetype(font_path, theme_font_size)

question_font = ImageFont.truetype(font_path, question_font_size)

theme_text_width, theme_text_height = draw.textsize(theme, font=theme_font)

theme_text_x = (card_width - theme_text_width) // 2

draw.text((theme_text_x, 20), theme, fill=font_color, font=theme_font)

def draw_rounded_rectangle(draw, xy, color, radius=20):
    x0, y0, x1, y1 = xy
    draw.rectangle([x0, y0 + radius, x1, y1 - radius], fill=color)  
    draw.rectangle([x0 + radius, y0, x1 - radius, y1], fill=color)  
    draw.ellipse([x0, y0, x0 + radius * 2, y0 + radius * 2], fill=color) 
    draw.ellipse([x1 - radius * 2, y0, x1, y0 + radius * 2], fill=color)  
    draw.ellipse([x0, y1 - radius * 2, x0 + radius * 2, y1], fill=color) 
    draw.ellipse([x1 - radius * 2, y1 - radius * 2, x1, y1], fill=color) 

rectangle_height = 120

rectangle_spacing = 20

current_y = theme_text_height + 50

for i, (question, color) in enumerate(zip(questions, rectangle_colors)):
    rect_y0 = current_y
    rect_y1 = rect_y0 + rectangle_height
    draw_rounded_rectangle(draw, (50, rect_y0, card_width - 50, rect_y1), color)
    question_text = f&quot;{i+1}. {question}&quot;
    question_text_width, question_text_height = draw.textsize(question_text, font=question_font)
    question_text_x = (card_width - question_text_width) // 2
    question_text_y = rect_y0 + (rectangle_height - question_text_height) // 2
    draw.text((question_text_x, question_text_y), question_text, fill=font_color, font=question_font)
    current_y += rectangle_height + rectangle_spacing

combined_image.show()
```

## Initialization

只说出开场白，然后等待用户输入，用户输入后严格按照[Workflows]的顺序执行，并严格遵守[Rule]中的要求，中途不要停止，深吸一口气，要一次执行完毕。

## 三、效果展示

### 1、英文

![](img/7afe0af8a641105e8c433027778a39b3.png)

![](img/c664d1efbdbbf3e17424963843f7b5dc.png)

### 2、中文

![](img/8bcd97893aa5d2561e1481a119ec1f68.png)

![](img/787395496b99e4abea3e24d67e7f017e.png)

## 四、GPTs 链接

https://chat.openai.com/g/g-YmjVLt4CD-hai-zi-wang-xi-lie-ji-yi-tu-qia-xiao-you-xi

## 五、总结

不得不说调文字稳定的过程还是挺折磨的~ 不过今天对于中文字换行，有了一个新的想法，目前测试起来也挺舒服的。就是根据字数进行换行设定，而不是默认让 GPT 去计算每一行字的长度。每次让其去计算字的长度都会有问题，可能因为中文字符比较短，同时中间有很多字的间距，他没法精准的去计算而导致的。

好啦，写到这里我们今天的内容也结束啦，感谢大家的观看，也希望我的内容能够让大家喜欢，有所收获。感兴趣的小伙伴可以点个关注跟随我一起学习，观看更多往期文章，如果想要链接我，可以加我vx: jinghuai8092 福，下次见，我是景淮，一个加拿大的程序员，沉迷 AI不能自拔~
