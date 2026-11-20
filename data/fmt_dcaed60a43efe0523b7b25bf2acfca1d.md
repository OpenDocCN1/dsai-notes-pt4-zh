
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
