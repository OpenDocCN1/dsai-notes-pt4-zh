
```
from PIL import Image, ImageDraw, ImageFont
import random

# 定义颜色与名称的映射
color_map = {
    "红": "#ff0000",
    "蓝": "#0000ff",
    "黄": "#ffff00",
    "绿": "#00ff00",
    "橙": "#ffa400",
    "紫": "#9f1fef",
    "粉": "#ffc0ca",
    "黑": "#000000",
    "白": "#ffffff",
    "棕": "#a42a2a"
}

# 随机抽取五个不重复的颜色
selected_colors = random.sample(list(color_map.keys()), 5)

# 用户没有输入，默认汉字数量为10
characters_count = 10

# 从抽取的五个颜色中随机抽取指定数量的汉字，确保每个汉字至少出现一次
# 如果汉字数量少于颜色数量，则只选取前n个颜色
if characters_count < len(selected_colors):
    selected_colors = selected_colors[:characters_count]

# 确保每个颜色至少出现一次后，填充剩余位置
characters = random.choices(selected_colors, k=characters_count)

# 定义卡片大小和初始参数
card_width = 285
char_size = 45  # 汉字大小
char_spacing = 10  # 汉字间隔
line_spacing = 20  # 行间距
max_chars_per_line = 5  # 每行最大汉字数

# 计算卡片高度
rows = (characters_count + max_chars_per_line - 1) // max_chars_per_line
card_height = rows * char_size + (rows - 1) * line_spacing + 2 * char_spacing  # 增加顶部和底部间隔

# 创建带有背景颜色的卡片
card = Image.new("RGB", (card_width, card_height), "#333333")
draw = ImageDraw.Draw(card)

# 加载字体
font_path = "/mnt/data/今年也要加油鸭.ttf"
font = ImageFont.truetype(font_path, char_size)

# 计算第一个汉字的起始位置
x = char_spacing
y = char_spacing

for index, character in enumerate(characters):
    # 设置字体颜色
    color = color_map[character]
    
    # 绘制汉字
    draw.text((x, y), character, font=font, fill=color)
    
    # 更新位置
    if (index + 1) % max_chars_per_line == 0:
        # 换行
        x = char_spacing
        y += char_size + line_spacing
    else:
        # 同行下一个位置
        x += char_size + char_spacing

# Display the result
card.show()
```

## 三）难度：中等（中）

提示词

```
利用Python制作一张宽度为285px的小卡片，步骤如下：
1. 从“红、蓝、黄、绿、橙、紫、粉、黑、白和棕”中随机抽取五个不重复的颜色的汉字，放入数组中。
2. 根据用户输入的汉字数量从所数组中随机进行抽取[输入的汉字数量]次，同时也从数组中随机抽取汉字的颜色，按照所抽取的顺序把抽到的汉字和字体颜色一一对应。如果用户没有输入则默认汉字数量为10
3. 比如抽到的字为红色，颜色为“蓝色”则，在小卡片上使用上传的字体写下“红”。然后字体颜色设置为“蓝”。
4. 小卡片上的汉字大小为45px，同时每个汉字之间的间隔为10px。每行最多五个字。如果超过五个字则自动换行，同时行间距为20px。
5. 小卡片的背景颜色为灰黑色。注意：
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

效果展示

![](img/8cd48c9f6e55f80cd2e41486e78a6d2d.png)

Python 代码

```
from PIL import Image, ImageDraw, ImageFont
import random

# Step 1: 随机抽取五个不重复的颜色的汉字
colors = ["红", "蓝", "黄", "绿", "橙", "紫", "粉", "黑", "白", "棕"]
color_codes = {
    "红": "#ff0000",
    "蓝": "#0000ff",
    "黄": "#ffff00",
    "绿": "#00ff00",
    "橙": "#ffa400",
    "紫": "#9f1fef",
    "粉": "#ffc0ca",
    "黑": "#000000",
    "白": "#ffffff",
    "棕": "#a42a2a"
}

# 抽取五个不重复的颜色
selected_colors = random.sample(colors, 5)

# 用户输入的汉字数量（这里假设为默认的10）
num_characters = 10

# Step 2: 随机抽取汉字和汉字的颜色
characters = random.choices(selected_colors, k=num_characters)
character_colors = [random.choice(selected_colors) for _ in range(num_characters)]

# 加载字体
font_path = '/mnt/data/今年也要加油鸭.ttf'
font_size = 45
font = ImageFont.truetype(font_path, font_size)

# 创建画布
card_width = 285
card_height = (font_size + 20) * ((num_characters - 1) // 5 + 1) + 20  # 根据汉字数量计算卡片高度
image = Image.new("RGB", (card_width, card_height), "#333333")
draw = ImageDraw.Draw(image)

# 设置汉字的位置参数
x_start, y_start = 10, 10  # 起始位置
x_offset = font_size + 10  # 汉字间的横向间隔
y_offset = font_size + 20  # 行间距

# Step 3 & 4: 在卡片上绘制汉字
for i, (char, color) in enumerate(zip(characters, character_colors)):
    x = x_start + (i % 5) * x_offset
    y = y_start + (i // 5) * y_offset
    draw.text((x, y), char, font=font, fill=color_codes[color])

# Display the result
image.show()
```

## 四）难度：困难（难）

提示词与上面相同，只有规则不同。

# 三、提示词编写测试

## 一）初始提示词

### Workflow
1. 等待用户输入
    - 获取用户想要是使用的模式
    - 获取用户输入的[汉字数量]，如果客户没有输入，则默认值为10
2. 根据用户输入的模式，调用对应`[Model]`中的模式
3. 每个`[Model]`执行结束后，必须展示一张图片给用户

### Model
- 认识颜色和汉字：
    101. 首先展示图片给用户，让用户对照图片进行学习记忆，之后询问用户是否想要玩一个颜色和文字连线小游戏
    102. 如果用户回复是则使用`python`运行`颜色汉字连线.py`的代码，生成一张python的小卡片，展示给用户
- 难度：简单（易）
    + 游戏规则：随机获取其中的五种颜色，使用颜色和汉字相同的方式，让孩子进行阅读。通过颜色和汉字一一对应的方式，尝试让孩子在脑海中形成记忆。
    + 执行：利用Python制作一张宽度为285px的小卡片，步骤如下：
        201. 从“红、蓝、黄、绿、橙、紫、粉、黑、白和棕”中随机抽取五个不重复的颜色。
        202. 根据用户输入的汉字数量从所抽取的五个汉字中随机进行抽取[输入的汉字数量]次，确保每个汉字和汉字的字体颜色相同。注意：所抽取的每个汉字和颜色至少出现一次，如果用户没有输入则默认汉字数量为10
        203. 比如抽到的字为红色则，在小卡片上使用上传的字体写下“红”。然后字体颜色设置为“红”。字体颜色和汉字必须一一对应。
        204. 小卡片上的汉字大小为45px，同时每个汉字之间的间隔为10px。每行最多五个字。如果超过五个字则自动换行，同时行间距为20px。
        205. 小卡片的背景颜色为灰黑色。
    + 示例代码：
        ```python
        from PIL import Image, ImageDraw, ImageFont

        import random

        # 定义颜色与名称的映射

        color_map = {

            "红": "#ff0000",

            "蓝": "#0000ff",

            "黄": "#ffff00",

            "绿": "#00ff00",

            "橙": "#ffa400",

            "紫": "#9f1fef",

            "粉": "#ffc0ca",

            "黑": "#000000",

            "白": "#ffffff",

            "棕": "#a42a2a"

        }

        # 随机抽取五个不重复的颜色

        selected_colors = random.sample(list(color_map.keys()), 5)

        # 用户没有输入，默认汉字数量为10

        characters_count = 10

        # 从抽取的五个颜色中随机抽取指定数量的汉字，确保每个汉字至少出现一次

        # 如果汉字数量少于颜色数量，则只选取前n个颜色

        if characters_count < len(selected_colors):

            selected_colors = selected_colors[:characters_count]

        # 确保每个颜色至少出现一次后，填充剩余位置

        characters = random.choices(selected_colors, k=characters_count)

        # 定义卡片大小和初始参数

        card_width = 285

        char_size = 45  # 汉字大小

        char_spacing = 10  # 汉字间隔

        line_spacing = 20  # 行间距

        max_chars_per_line = 5  # 每行最大汉字数

        # 计算卡片高度

        rows = (characters_count + max_chars_per_line - 1) // max_chars_per_line

        card_height = rows * char_size + (rows - 1) * line_spacing + 2 * char_spacing  # 增加顶部和底部间隔

        # 创建带有背景颜色的卡片

        card = Image.new("RGB", (card_width, card_height), "#333333")

        draw = ImageDraw.Draw(card)

        # 加载字体

        font_path = "/mnt/data/今年也要加油鸭.ttf"

        font = ImageFont.truetype(font_path, char_size)

        # 计算第一个汉字的起始位置

        x = char_spacing

        y = char_spacing

        for index, character in enumerate(characters):

            # 设置字体颜色

            color = color_map[character]

            # 绘制汉字

            draw.text((x, y), character, font=font, fill=color)

            # 更新位置

            if (index + 1) % max_chars_per_line == 0:

                # 换行

                x = char_spacing

                y += char_size + line_spacing

            else:

                # 同行下一个位置

                x += char_size + char_spacing

        # Display the result

        card.show()

        ```
- 难度：中等（中）
    + 游戏规则：随机获取其中的五种颜色，使用颜色和汉字不同的方式，让孩子根据 **汉字**进行阅读。加深对汉字的含义的印象。
    + 执行：利用Python制作一张宽度为285px的小卡片，步骤如下：
        301. 从“红、蓝、黄、绿、橙、紫、粉、黑、白和棕”中随机抽取五个不重复的颜色的汉字，放入数组中。
        302. 根据用户输入的汉字数量从所数组中随机进行抽取[输入的汉字数量]次，同时也从数组中随机抽取汉字的颜色，按照所抽取的顺序把抽到的汉字和字体颜色一一对应。如果用户没有输入则默认汉字数量为10
        303. 比如抽到的字为红色，颜色为“蓝色”则，在小卡片上使用上传的字体写下“红”。然后字体颜色设置为“蓝”。
        304. 小卡片上的汉字大小为45px，同时每个汉字之间的间隔为10px。每行最多五个字。如果超过五个字则自动换行，同时行间距为20px。
        305. 小卡片的背景颜色为灰黑色。
    + 示例代码：
        ```Python
        from PIL import Image, ImageDraw, ImageFont

        import random

        # Step 1: 随机抽取五个不重复的颜色的汉字

        colors = ["红", "蓝", "黄", "绿", "橙", "紫", "粉", "黑", "白", "棕"]

        color_codes = {

            "红": "#ff0000",

            "蓝": "#0000ff",
        ```
