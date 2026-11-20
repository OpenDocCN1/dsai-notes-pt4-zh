
# GPTs 实战：新春贺卡制作（中文版本）

> 来源：[https://agijuejin.feishu.cn/docx/N2ycdYwY2oYyVhx0TRKc2PZnnOd](https://agijuejin.feishu.cn/docx/N2ycdYwY2oYyVhx0TRKc2PZnnOd)

> 作者：景淮AI探索之旅

> 原文链接：https://mp.weixin.qq.com/s/cT-WIMZoA17z6u6nvq8F9Q

嘿，大家好呀，我是景淮，一个加拿大的程序员，沉迷 AI 不能自拔中。

转眼 2023 年就剩下最后的几天啦，距离我发出圣诞贺卡已经过去十多天啦~ 很多小伙伴都询问了我圣诞贺卡有没有中文版本，很抱歉那时候因为太匆忙，并没有做中文版。但随着元旦的来临！中文版的元旦贺卡，他来啦~

今天的内容其实和上次圣诞贺卡的内容比较类似，我会细致的陪大家走一遍，这种贺卡制作的过程，希望大家在过程中了解这种做图是什么原理，怎么样能调整到更稳定的效果。还是一样的，写文章的初衷还是希望大家动手试试，希望大家喜欢~

今天是我持续分享内容的第 60 篇，主要是分享关于：

**GPT 辅助制作元旦贺卡**

本文会根据以下内容顺序进行：

+ 图片调试

+ 文案生成

+ 生成文字图片调试

+ 提示词编写、测试

+ GPTs 制作

+ 总结

## 一、图片调试

虽然已经合理的按照我的要求把文字加进去了，但是我们可以发现对于一张贺卡来说，这种图还是太过于复杂，内容过多。

然后，我们可以看到图片还是挺好看的，但 D3 展示文字这块确实效果不太好。所以我们再调整一下，让它多加一些内容，减少文字的整体占比。

![](img/c4dca8006fad2e5a745f237bd767211b.png)

获得了一个我觉得不错的效果，其实也是抽卡抽出来的，如果效果不好，可以使用提示词进行调整，或者持续抽卡的方式，对于我们来说最关键的是稳定生成这种效果的图片，所以图片的提示词就是很重要的一环。

绘画部分的提示词如下

> A clean and minimalist New Year's greeting card for 2024 in a 9:16 format. The card has a solid color background, offering a sleek and modern look. Centered on the card are the words "2024" and "Happy New Year" in a simple, elegant font. Surrounding the text, there are subtle, stylized icons representing New Year's celebrations, like fireworks, a champagne glass, and a clock approaching midnight, all depicted in a minimalistic style that complements the overall design. These elements are arranged to enhance the festive feel without overcrowding the card.

现在我们加上一些限制的内容，限制的内容主要是为了让生成的图片是一整张贺卡，而不是现在这样把贺卡摆在中间。

删减后

> A clean and minimalist New Year's greeting card for 2024 in a 9:16 format. The card has a solid color background, offering a sleek and modern look. All are depicted in a minimalistic style

这边限制的推演过程就略过了，也是差不多的流程，用提示词控制不停的画图，然后拆解符合要求图片的提示词的一部分，最后找到生效的内容。

> A designed to cover the entire image with no border or empty space around the edges, in a 9:16 ratio, featuring a single, consistent solid `[color]` background.

然后结合两部分提示词，在一个新的页面直接输入提示词，看看生成的效果。

> A clean and minimalist New Year's greeting image, covers the entire image with no border or empty space around the edges, in a 9:16 format, featuring a single, consistent solid `[color]` background. All are depicted in a minimalistic style

> 固定使用上述提示词做为开头（放在最前面，可以直接使用），只可根据 `[关键词]` 填充修改 `[color]`，其他内容不可更改。然后以 `[关键词]` 帮我写一段描述图片中间内容的绘画提示词的描述，要求画面简洁清晰，并确保绘画内容覆盖整张图片，放在开头的后面进行补充，画风：现代和简约风格. 同时用英文标注 "2024" and "Happy New Year"，生成后使用 Dalle-3 进行画图. 关键词：New Year

**Tips**：调整提示词的时候需要注意，不能使用 `Card` 这种单词在绘画提示词中，不然生成的图片大多数都是中间一张卡片的形式，所以我在生成图片时进行了单词替换。

最后生成的效果图

![](img/cdfc7e3c1219a44c5c88db2356477d45.png)

## 二、文案生成

我们可以先从最简单的生成元旦文案开始测试。看看如果不增加任何限制，生成效果是什么样的。

![](img/70efda61cafcb21af4f6b02360ca431f.png)

这段祝福词对我们来说有点长，也不太符合我想要的效果（后面调整后的要求），增加字数限制，换行符，每句话的限制。

生成效果一般，而且不太可控，所以我又给了一些少样本。

![](img/c48dd37ed9d3ab8af134c9cc4bd1356e.png)

效果明显有所好转

## 三、生成文字图片调试

因为之前的圣诞贺卡部分我们已经有了成熟的图片生成提示词，我们这边拿过来修改一下，主要展示如何进行调整。首先，我们在展示之前，对提示词中的文字进行修改，主要内容是因为 “圣诞贺卡” => “元旦贺卡”

贺卡补充设计：首先要求用户提供字体包，等待用户提供字体包后，利用 Python 生成贺卡，深吸一口气，然后慢慢的一步一步执行以下步骤：

401. 获取所生成图片的背景色，并展示 RGB 数值给用户。
402. 以获取到的 RGB 数值为背景的背景颜色，在生成 `[贺卡绘制]` 的右边补充一块与 `[贺卡绘制]` 相同大小的图片（同样的高度和宽度）
403. 结合下面的方法论，把 `[贺卡文案]` 的所生成的文本（保持原格式）结果，放到所补充的图片上
   - 选择合适的字体和大小：
     - 使用 PIL 库加载所需字体。在本例中，我们使用了大小为 85 的字体，这样的大小既确保了文本的清晰可读，又不会过大占据过多空间。
   - 确定行间距：
     - 设置适当的行间距（例如 30 像素），以确保文本的可读性和美观性。
   - 调整段落间距：
     - 将每个段落之间的距离设置为正常行间距的 3 倍。这样做不仅有助于区分不同的段落，还能保持整体的美观性。
   - 文本的垂直居中与左对齐：
     - 计算文本区域的总高度，并据此确定文本的起始垂直位置，以确保文本在垂直方向上居中。
     - 保持文本的左对齐格式，以符合常规阅读习惯。
   - 保持左右边距一致：
     - 在计算文本的最大宽度时，考虑左右边距，以确保文本左右两边的空白区域保持一致。
   - 确保文本内容适当展示与换行：
     - 确保所有文字内容均展示在图片中，避免超出画面。
     - 在单词或句子达到最大宽度限制时必须进行换行。
   - 图像的最终组合：
     - 将处理过的文本区域图片放在原始图片的右侧组成新的图片
   - 使用 `python` 执行上述内容时，务必按照以下顺序创建代码
     1. 创建一个图片，确定所创建的图片背景色为 `[401]` 的执行结果
     2. 使用 PIL 库加载所需字体，设定字体为用户上传的字体，并设置字体的字号为 85px
     3. 录入 `[贺卡文案]` 时，注意保持原有格式，每段内容应该为两个 `\n`
     4. 然后设定文本与图片边缘的距离，根据计算获取文本的具体范围
     5. 当段落内容过长，超出具体的步骤 4 中计算出的文本范围时，应该在不影响单词显示的情况下，进行自动换行（增加 `\n`）。避免超出具体的文本范围。
     6. 设置行高为 30px，行距，字体颜色为白色等信息。
     7. 根据计算文本整体的内容高度，调整文本整体位置，使文本整体垂直高度居中，并且确保文本依然保持原有的格式，同时整体文本居中。

```python
# Create a new image with the same size as the original image

new_image_size = image.size

new_image = Image.new("RGB", new_image_size, background_color)

# Load the provided font

font_path = '/mnt/data/DancingScript-VariableFont_wght.ttf'

font_size = 85

font = ImageFont.truetype(font_path, font_size)

# The Christmas message

message = (

    "Dear [贺卡接收人],\n\n"

    "[第一段话]\n\n "

    "[第二段话]\n\n"

    "[第三段话]\n\n"

    "[表达祝福],\n"

    "[姓名]]"

)

# Recalculating the text position to ensure vertical centering

# First, we need to calculate the total height of the text block

# Initialize variables to calculate total text height

total_text_height = 0

max_line_width = width - 2 * margin

# Calculate the total height of the text block

for line in message.split('\n'):

    line_width, line_height = draw.textsize(line, font=font)

    if line_width > max_line_width:

        words = line.split(' ')

        current_line = ''

        for word in words:

            # Check if adding the word exceeds the line width

            if draw.textsize(current_line + word + ' ', font=font)[0] <= max_line_width:

                current_line += word + ' '

            else:

                # Add the height of the line to the total height

                total_text_height += line_height + line_spacing

                current_line = word + ' '

        # Add the height of the last line of the split

        total_text_height += line_height + line_spacing

    else:

        # Add the height of the line to the total height

        total_text_height += line_height + line_spacing

# Recreate the image to reset the drawing

new_image = Image.new("RGB", new_image_size, background_color)

draw = ImageDraw.Draw(new_image)

# Calculate the starting height for vertical centering

start_height = (height - total_text_height) // 2

# Redraw the text with vertical centering

current_height = start_height

for line in message.split('\n'):

    line_width, line_height = draw.textsize(line, font=font)

    if line_width > max_line_width:

        words = line.split(' ')

        current_line = ''

        for word in words:

            if draw.textsize(current_line + word + ' ', font=font)[0] <= max_line_width:

                current_line += word + ' '

            else:

                # Draw the line and reset for a new line

                draw.text(((width - draw.textsize(current_line, font=font)[0]) / 2, current_height), current_line, font=font, fill=text_color)

                current_height += line_height + line_spacing

                current_line = word + ' '

        # Draw the last line of the split

        draw.text(((width - draw.textsize(current_line, font=font)[0]) / 2, current_height), current_line, font=font, fill=text_color)

        current_height += line_height + line_spacing

    else:

        # Center the line horizontally and draw it
```
