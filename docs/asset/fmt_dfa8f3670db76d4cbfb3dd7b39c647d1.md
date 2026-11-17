
# GPTs Action 实战：彩色汉字小游戏

> 来源：[https://agijuejin.feishu.cn/docx/MdrkdYIHhofkKPxKLCpcwUX5nvh](https://agijuejin.feishu.cn/docx/MdrkdYIHhofkKPxKLCpcwUX5nvh)

> 作者：景淮AI探索之旅

> 原文链接：https://mp.weixin.qq.com/s/62gizq2etl06D6o0vXv1pw

嘿，大家好呀，我是景淮，一个加拿大的程序员，沉迷 AI 不能自拔中。春节时期不打烊~ 持续输出新的内容给大家~

这篇内容算是压箱底的货啦，春节时候写的~才发出来，因为上周还是情感波动比较大，所以发了两篇与提示词内容不大，但与自己思考息息相关的内容。感谢大家的支持和赞赏~🫶

好啦，现在让我们聊聊我们今天的主题。今天是我要更新孩子王系列内容的第 6 篇

### GPT 制作彩色汉字小游戏

本文会根据以下内容顺序进行：

+ 需求分析
+ 分步实现需求
+ 提示词编写测试
+ GPTs 使用链接
+ 总结

# 一、需求分析

## 一）什么是彩色汉字小游戏

游戏如图所示：

![](img/be04e74150b74b5e3930ba347a009bc4.png)

### 1、彩色汉字

这个小游戏是一种认知挑战游戏，基于斯特鲁普效应设计，旨在测试和训练玩家的认知控制能力和注意力切换能力。游戏中展示的汉字是不同颜色名称的词语，但每个词语的呈现颜色与其实际意义不匹配。玩家的任务是忽略汉字的字面含义，而是要尽快准确地说出每个汉字的显示颜色。

### 2、对孩子的好处

1. 认知灵活性：认知灵活性是指个体在适应新的规则或环境时，从一个思维模式转换到另一个思维模式的能力。在这个游戏中，孩子需要忽略他们阅读汉字的自动反应，转而关注汉字的颜色，这有助于提高他们在遇到变化时适应和转换的能力。

2. 注意力控制：游戏要求孩子专注于特定的任务目标（汉字的颜色），而忽略干扰信息（汉字的字面意义）。这有助于提高孩子的注意力控制能力，即在众多信息中快速识别并专注于关键信息的能力。

3. 执行功能：执行功能是一系列高级认知过程，包括计划、决策、错误检测、冲动控制和任务切换等。这个游戏通过要求玩家同时处理相互冲突的信息（汉字的字面意义与颜色），促进了执行功能的发展。

4. 处理速度：玩家需要迅速识别颜色并做出反应，这种练习有助于提高孩子的信息处理速度。

5. 心理弹性：在游戏中，孩子可能会面临挑战和失败（如错误地说出汉字而不是颜色），这有助于他们学习如何在面对困难时保持尝试，并从错误中恢复。

## 二）游戏设计

游戏过程中我们可以为不同年龄的孩子设置三种不同的难度。（"难"、“中”、“易”）

根据不同难度，每次可以从红色、蓝色、黄色、绿色、橙色、紫色、粉色、黑色、白色和棕色，十种颜色中随机选择生成不同的难度的游戏。

### 1、难度：简单（易）

随机获取其中的五种颜色，使用 `颜色和汉字相同` 的方式，让孩子进行阅读。通过颜色和汉字一一对应的方式，尝试让孩子在脑海中形成记忆。

如：`"红色" => 红色`

### 2、难度：中等（中）

随机获取其中的五种颜色，使用 `颜色和汉字不同` 的方式，让孩子根据 `汉字` 进行阅读。加深对汉字的含义的印象。

如：`“红色” => 红色`

### 3、难度：困难（难）

随机获取其中的五种颜色，使用 `颜色和汉字随机匹配` 的方式，让孩子根据 `颜色` 阅读。

如：`“红色” => 黄色`

### 4、生成的汉字数量

默认设定每次生成的汉字数量为 10 个，即 `5 * 2`，每行5个汉字，一共生成 2 排。

用户可以在选择难度的同时输入想要生成汉字的数量，但要求是 5 的倍数。

### 5、认识颜色和汉字

把十种颜色的汉字和颜色对应的卡片制作出来展示给小朋友，并提供一个可以连线练习的小游戏。

### 6、输出

使用 Python 绘制图片的方式，把对应难度的游戏内容书写在小卡片上。

## 三）需求整合

![](img/1e7cf9a366344ef6adf1739241e12051.png)

# 二、分步实现需求

## 一）认识颜色和汉字

首先，要制作一张颜色和汉字一一对应的卡片，这张卡片可以在后续使用中直接放在 `Knowledge` 中进行使用。并不需要重复的制作。

> 提示词：使用 Python 帮我生成一张图片，这张图片中要实现中文颜色的汉字和对应的颜色一一对应起来。格式如下：一个长度为 40px 宽度为10px的长方形，用红色进行填充。 后面对应汉字“红色”，依次生成“红色、蓝色、黄色、绿色、橙色、紫色、粉色、黑色、白色和棕色”十种颜色。同时每个颜色中间留出足够的空隙，不要挨得太近。放在同一张小卡片中，以供孩子学习。

当然我们也可以用 PS 等绘图软件进行制作，以个人方便的方式进行生成就好。

生成效果

![](img/2882899fea24b4c6ae8b35104cf91afc.png)

每个颜色相对应的代码如下：

+ 红色 (Red): `#ff0000`
+ 蓝色 (Blue): `#0000ff`
+ 黄色 (Yellow): `#ffff00`
+ 绿色 (Green): `#00ff00`
+ 橙色 (Orange): `#ffa400`
+ 紫色 (Purple): `#9f1fef`
+ 粉色 (Pink): `#ffc0ca`
+ 黑色 (Black): `#000000`
+ 白色 (White): `#ffffff`
+ 棕色 (Brown): `#a42a2a`

### 连线小游戏

提示词

```
使用 Python 帮我生成一张图片。格式如下：一个长度为40px 宽度为10px的长方形，用红色进行填充。 然后依次生成“蓝色、黄色、绿色、橙色、紫色、粉色、黑色、白色和棕色”总共十种颜色。同时每个颜色中间留出足够的空隙，不要挨得太近。在每个颜色块的后面随机从十种颜色中选择一个写在长方形后面，中间保留30px间距。 给长方形增加一个1px的黑色边框，同时把所有内容居中 注意：每个颜色都只能使用一次，不能重复使用。放在同一张小卡片中，以供孩子学习。
```

生成效果

![](img/456da19b185ef4b25a07056ac13b6dbb.png)

### 代码实现

```python
from PIL import Image, ImageDraw, ImageFont
import os
import random

# Define the colors to be used
colors = [
    ("red", "红色"),
    ("blue", "蓝色"),
    ("yellow", "黄色"),
    ("green", "绿色"),
    ("orange", "橙色"),
    ("purple", "紫色"),
    ("pink", "粉色"),
    ("black", "黑色"),
    ("white", "白色"),
    ("brown", "棕色")
]

# Load the uploaded font
font_path = '/mnt/data/Mengshen-Handwritten.ttf'
font_size = 20  # Adjust font size as needed
font = ImageFont.truetype(font_path, font_size)

# Calculate total image size needed
# Assuming 30px vertical spacing between rectangles, 1px border, and 30px for text height
rect_height = 10  # Adjusted for horizontal orientation as in Code 2
rect_width = 40   # Adjusted width as in Code 2
border_size = 1
spacing = 30
text_height = 30  # Approximation
num_colors = len(colors)

# Image width: width of rectangles + maximum text width (approximation) + spacing
# Calculate maximum text width
max_text_width = max([font.getsize(color[1])[0] for color in colors]) + spacing

image_width = rect_width + max_text_width + (border_size * 2) + spacing * 2
image_height = (rect_height + text_height + spacing * 3) * num_colors

# Shuffle the texts to not match the colors directly
shuffled_colors = colors[:]
while True:
    random.shuffle(shuffled_colors)
    if all(shuffled_color[1] != color[1] for shuffled_color, color in zip(shuffled_colors, colors)):
        break

# Create a new image with white background
image = Image.new("RGB", (image_width, image_height), "white")
draw = ImageDraw.Draw(image)

# Draw each color rectangle and corresponding shuffled text
y_offset = spacing
for (color, _), (_, text) in zip(colors, shuffled_colors):
    # Calculate x coordinates for rectangle and text to center them
    rect_x1 = (image_width - rect_width - max_text_width - spacing) // 2
    rect_x2 = rect_x1 + rect_width
    text_x = rect_x2 + spacing * 2  # Leave some space after the rectangle

    # Rectangle coordinates and border
    draw.rectangle([rect_x1-border_size, y_offset-border_size, rect_x2+border_size, y_offset+rect_height+border_size], outline="black")
    draw.rectangle([rect_x1, y_offset, rect_x2, y_offset + rect_height], fill=color)

    # Text position: vertically centered with the rectangle
    text_y = y_offset + (rect_height - font.getsize(text)[1]) // 2
    draw.text((text_x, text_y), text, fill="black", font=font)

    # Update y_offset for the next color
    y_offset += rect_height + text_height + spacing * 3

# show the updated image
output_path_shuffled = '/mnt/data/color_rectangles_with_shuffled_text.png'
image = Image.open(output_path_shuffled)
image.show()
```

## 二）难度：简单（易）

### 提示词

```
利用Python制作一张宽度为285px的小卡片，步骤如下：
1. 从“红、蓝、黄、绿、橙、紫、粉、黑、白和棕”中随机抽取五个不重复的颜色。
2. 根据用户输入的汉字数量从所抽取的五个汉字中随机进行抽取[输入的汉字数量]次，确保每个汉字和汉字的字体颜色相同。注意：所抽取的每个汉字和颜色至少出现一次，如果用户没有输入则默认汉字数量为10
3. 比如抽到的字为红色则，在小卡片上使用上传的字体写下“红”。然后字体颜色设置为“红”。字体颜色和汉字必须一一对应。
4. 小卡片上的汉字大小为45px，同时每个汉字之间的间隔为10px。每行最多五个字。如果超过五个字则自动换行，同时行间距为20px。
5. 小卡片的背景颜色为灰黑色。
- 红 (Red): #ff0000
- 蓝 (Blue): #0000ff
- 黄 (Yellow): #ffff00
- 绿 (Green): #00ff00
- 橙 (Orange): #ffa400
- 紫 (Purple): #9f1fef
- 粉 (Pink): #ffc0ca
- 黑 (Black): #000000
- 白 (White): #ffffff
- 棕 (Brown): #a42a2a
```

### 效果展示

![](img/c62bee7a91915a5c835bb5b530deccdd.png)

### Python 代码
