
### 图片文字绘制与合成代码

```python
draw.text(((width - line_width) / 2, current_height), line, font=font, fill=text_color)

current_height += line_height + line_spacing

# Combine the original image and the new text image again

combined_image = Image.new("RGB", (width * 2, height), background_color)

combined_image.paste(image, (0, 0))

combined_image.paste(new_image, (width, 0))

# Save and display the final combined image with vertical centering

final_image_centered_path = '/mnt/data/final_new_year_card_centered.png'

combined_image.save(final_image_centered_path)

# Display the final image with vertical centering

plt.imshow(combined_image)

plt.axis('off')

plt.show()

final_image_centered_path
```

运行之后，我发现的问题是图片中的文字不能很好的进行分割。

![](img/86ca40a84b04f9cbd0026eacb3994354.png)

经过我多次对话调整后，我发现不能完整的测量图片中文字的长度，跟英文有所区别，所以为了避免这种情况，我们可以尝试对生成的文本进行限制，如在每一句话的末尾加上固定的`\n`换行符，同时确保文本整齐。

（次数到了，换了个账号，所以图片也不一样了，但生成流程是一样的）

最后调整了`python`代码的一些内容，还有文字大小等。生成效果如下

![](img/3a94a1a9e872ae9a9c1e20dbba5cc8d6.png)

# 四、提示词编写测试

## 一）初始提示词

因为已经做过一个类似的圣诞节贺卡了，所以这边直接给予圣诞节贺卡和上面的方法进行整合调整

### Workflows:
1. 输入：只说出开场白“你好，想要获取只属于你自己的专属圣诞贺卡嘛？快输入你想要送出专属贺卡的人和你的关系。还有你想要贺卡中绘制的内容，请用关键词进行描述1个”，然后等待用户输入
    - 如用户未输入收贺卡人和用户的关系，则提醒用户输入
    - 如用户未输入贺卡绘制关键词，则随机生成1个与元旦相关的关键词
2. 元旦贺卡文案：根据提供的`[关系]`，写一封不超过 40 词（`Maxtoken` 40）的中文元旦贺词。同时在每一句话后面添加换行符`\n`，每句话不超过8个字
    - 中文元旦贺词的意思是，用中文进行书写。
    - 格式：
        `[四字词语]`，`[四字词语]`；
        `[四字词语]`，`[四字词语]`。
        `[四字词语]`，`[四字词语]`；
        `[四字词语]`，`[四字词语]`。
        + 示例1：
            家人闲坐，灯火可亲；
            新年伊始，万象更新。
            所念之人，平安喜乐；
            所想之事，顺心如意
        + 示例2：
            日有小暖，岁有小安。
            辞慕尔尔，烟火年年。
            日子滚烫，人间可爱。
3. 贺卡绘制：
    - “A clean and minimalist New Year's greeting image, covers the entire image with no border or empty space around the edges, in a 9:16 format, featuring a single, consistent solid `[color]` background. All are depicted in a minimalistic style”
    - - 固定使用上述提示词做为开头（放在最前面，可以直接使用），只可根据`[关键词]`填充修改`[color]`，其他内容不可更改。然后以`[关键词]`帮我写一段描述图片中间内容的绘画提示词的描述，要求画面简洁清晰，并确保绘画内容覆盖整张图片，放在开头的后面进行补充，画风：现代和简约风格。同时用英文标注`"2024"` and `"Happy New Year"`，生成后使用`Dalle-3`进行画图。
4. 贺卡补充设计：首先要求用户提供字体包，等待用户提供字体包后，利用`Python`生成贺卡，深吸一口气，然后慢慢的一步一步执行以下步骤
  401. 获取所生成图片的背景色，并展示`RGB`数值给用户。
  402. 以获取到的`RGB`数值为背景的背景颜色，在生成`[贺卡绘制]`的右边补充一块与`[贺卡绘制]`相同大小的图片（同样的高度和宽度）
  403. 结合下面的方法论，把`[贺卡文案]`的所生成的文本（保持原格式）结果，放到所补充的图片上
    - 选择合适的字体和大小:
      * 使用`PIL`库加载所需字体。在本例中，我们使用了大小为`85px`的字体，这样的大小既确保了文本的清晰可读，又不会过大占据过多空间。
    - 确定行间距:
      * 设置适当的行间距（例如`30px`），以确保文本的可读性和美观性。
    - 调整段落间距:
      * 将每个段落之间的距离设置为正常行间距的3倍。这样做不仅有助于区分不同的段落，还能保持整体的美观性。
    - 文本的垂直居中与左对齐:
      * 计算文本区域的总高度，并据此确定文本的起始垂直位置，以确保文本在垂直方向上居中。
      * 保持文本的左对齐格式，以符合常规阅读习惯。
    - 保持左右边距一致:
      * 在计算文本的最大宽度时，考虑左右边距，以确保文本左右两边的空白区域保持一致。
    - 确保文本内容适当展示与换行:
      * 确保所有文字内容均展示在图片中，避免超出画面。
      * 在单词或句子达到最大宽度限制时必须进行换行。
    - 图像的最终组合:
      * 将处理过的文本区域图片放在原始图片的右侧组成新的图片 
    + 使用`python`执行上述内容时，务必按照以下顺序创建代码
      1. 创建一个图片，确定所创建的图片背景色为`[401]`的执行结果
      2. 使用`PIL`库加载所需字体，设定字体为用户上传的字体，并设置字体的字号为`85px`
      3. 录入`[贺卡文案]`时，注意保持原有格式，每段内容应该为两个`\n`
      4. 然后设定文本与图片边缘的距离，根据计算获取文本的具体范围
      5. 当段落内容过长，超出具体的步骤4中计算出的文本范围时，应该在不影响单词显示的情况下，进行自动换行（增加`\n`）。避免超出具体的文本范围。
      6. 设置行高为`30px`，行距，字体颜色为白色等信息。
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

        draw.text(((width - line_width) / 2, current_height), line, font=font, fill=text_color)

        current_height += line_height + line_spacing

# Combine the original image and the new text image again

combined_image = Image.new("RGB", (width * 2, height), background_color)

combined_image.paste(image, (0, 0))

combined_image.paste(new_image, (width, 0))

# Save and display the final combined image with vertical centering
```
