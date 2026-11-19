
# GPTs 实战：叠在一起的小动物

> 来源：[https://agijuejin.feishu.cn/docx/BOIJdRduzo6honxY🐧OcTEHfnrf](https://agijuejin.feishu.cn/docx/BOIJdRduzo6honxY🐧OcTEHfnrf)

> 作者：景淮AI探索之旅

> 原文链接：https://mp.weixin.qq.com/s/6hmRRtkq1zvuISI-Q6CK8A

嘿，大家好呀，我是景淮，一个在加拿大的朋友，每天陪你一起玩转 AI。

今天，又是一个忙碌的周末，有时候真的感觉周末比平时还忙哈哈哈，可能因为要陪家人出去，要一起带孩子玩吧~

虽然忙碌，但是也是很快乐的一天呀~

想起来，年初立的 Flag 今年要更新孩子王系列 50 篇，数了一下才更新不到 15 篇。然而今年已经过了一半多了，所以要开始加速更新啦~

今天我们的孩子王系列内容主题如下

+   需求分析
+   分步实现需求
+   提示词编写测试
+   GPTs 使用链接
+   总结

## 一、需求分析

### 一）什么是"叠在一起的小动物"小游戏呢？

其实灵感来源很有趣，是看到了如图一个场景

![](img/5a84f8d14762b28f8082c302dd092d88.png)

很可爱的一幅图，我们看到了三只小猫咪和小鸡堆在了一起，形成了我们看到的图。在这个过程中，我和我外甥就开始分析了！

究竟这三只猫是怎么样纠缠在一起的呢？怎么叠在一起的呢？

冷不丁一看，有点乱，但仔细看看，可以发现因为颜色是有对比的，所以还是能比较清楚的分辨出来。

然后，我就想到了，如果我用简笔画的方式把一些小动物，叠在一起，使用不同的颜色来针对不同的小动物的简笔画，那我们就可以制作一个又可以考眼力，分辨小动物的、又可以锻炼孩子空间感的游戏！

这也就是这个游戏设计的初衷。

### 二）思路拆解

要完成这样的一幅图片。需要完成以下步骤：

1.  GPT 随机生成几个小动物，包含小动物的简单介绍。

1.  利用 `Dalle-3` 绘制小动物的简笔画

1.  利用 `Python` 把图片线条上色，每只小动物使用不同的颜色

1.  把图片用随机的顺序叠加在一起，上一层和下层重叠的部分保持上层颜色。

### 三）思路图解

![](img/01e609079ad8c37a29f2ede7fb2ffa37.png)

## 二、分步实现需求

### 一）GPT 随机生成几个小动物，包含小动物的简单介绍。

#### 1、提示词

```
根据用户输入的难度来调整，随机生成的小动物个数，难度简单生成3只小动物、难度中等生成5只小动物、难度困难生成7只小动物。

小动物的生成要求：随机选择、每次选择的小动物不可以重复、同时给每种小动物配备一种颜色（红色、橙色、黄色、绿色、蓝色、靛色、紫色）

在输出小动物的同时，用一句话给出小动物的介绍，要求有趣，容易理解。可以帮助大家认识这种动物的特点、习性等。

输出格式：
【动物】（【颜色】】）：【介绍】
```

#### 2、效果展示

![](img/ca5736f8620bfa4e9cef5ff9bab63b65.png)

### 二）利用 `Dalle-3` 依次绘制小动物的简笔画

#### 1、提示词

```
根据上面生成的小动物，按照上面的顺序，依次对下述的绘画提示词中的[关键词]进行补充，不用在意颜色的描述，关键词只使用上述关键词的英文，如：猴子就在关键词处填入Monkey，每种动物调用 Dalle-3 进行依次生图，有几种动物生成几张图。深吸一口气，可以一个一个来。
- 绘画提示词：
    + &quot;A cute, simple line drawing of a symmetrical [关键词]. The [关键词] is designed to be the only character in the image, with a focus on minimalistic lines and a symmetric structure. All auxiliary lines are removed to emphasize the simplicity of the drawing.&quot;根据用户选择的关键词填入上述提示词中[关键词]的部分，同时不要修改其他内容的情况下。最后根据最新的提示词生成图片。
```

#### 2、效果展示

![](img/18e8a755791e6026059e0e7e57de887d.png)

### 三）利用 `Python` 调整原图中线条的颜色

#### 1、提示词

```
利用 Python 结合最开始生成的小动物相对应的颜色，来调整刚才生成图片中所有线条的颜色。例如，最开始生成的内容为 猴子（红色）则把 Dalle-3 生成的对应图片中所有线条换成猴子对应的红色。
    - 如果有小动物的图片有进行调整或重新生成，则使用最后生成的对应小动物图片。
    - 代码示例：
        ```Python

        from PIL import Image, ImageOps

        import numpy as np

        def process_image(image_path, output_path, line_color):

            # 加载图像

            image = Image.open(image_path)

            # 如果图像不是RGBA模式，则转换为RGBA

            if image.mode != &#x27;RGBA&#x27;:

                image = image.convert(&#x27;RGBA&#x27;)

            # 将白色背景变为透明

            datas = image.getdata()

            newData = []

            for item in datas:

                if item[0] &gt; 200 and item[1] &gt; 200 and item[2] &gt; 200:

                    # 替换为透明

                    newData.append((255, 255, 255, 0))

                else:

                    newData.append(item)

            image.putdata(newData)

            # 保存透明背景的图像

            transparent_image_path = output_path + &quot;/transparent_image.png&quot;

            image.save(transparent_image_path, &quot;PNG&quot;)

            # 加载透明背景的图像

            transparent_image = Image.open(transparent_image_path)

            # 如果图像不是RGBA模式，则转换为RGBA

            if transparent_image.mode != &#x27;RGBA&#x27;:

                transparent_image = transparent_image.convert(&#x27;RGBA&#x27;)

            # 将图像转换为numpy数组以便于操作

            data = np.array(transparent_image)

            # 定义检测黑色线条的阈值

            threshold = 50

            # 将所有黑色线条替换为红色线条

            red = list(line_color) + [255]  # 添加不透明度

            black_indices = np.all(data[:, :, :3] &lt; threshold, axis=-1)

            data[black_indices] = red

            # 转换回图像

            red_lines_image = Image.fromarray(data, mode=&#x27;RGBA&#x27;)

            # 保存红色线条的图像

            red_lines_image_path = output_path + &quot;/red_lines_image.png&quot;

            red_lines_image.save(red_lines_image_path, &quot;PNG&quot;)

            return red_lines_image_path

        # 定义图像路径和输出路径

        image_path = &quot;/mnt/data/image.png&quot;

        output_path = &quot;/mnt/data&quot;

        # 定义线条颜色为红色 (猴子对应的颜色)

        line_color = (255, 0, 0)

        # 调用处理函数

        processed_image_path = process_image(image_path, output_path, line_color)

        print(f&quot;Processed image saved at: {processed_image_path}&quot;)

        ```
```

#### 2、效果展示

![](img/5d61a393174cdac4ee9d697be20fbfa4.png)

> 可以看到明显熊猫处理的有些问题，这个需要单独调整。

### 四）利用 `Python` 随机顺序进行重叠，随机调整角度和距离中心的距离。

#### 1、提示词

```
利用python处理上述所以处理好的图片。
在最底层增加一个原图（长+1200px，宽+1200px）的白色底板。
要求按照随机顺序把所有图片一层一层的进行重叠在一起，确保所有图片的长、高都完全相同。所有图片的摆放中心点与白色底板图的中心点相同。然后除了最下面的一张图片以外。每张图片随机旋转（0-360）度之间的任意角度（），同时向着（随机从0,60,120,180,240,300度中选择一个，但只可以使用一次，可以不全使用）方向，随机前进（400-600）px，把最后生成的重叠图，展示给用户。同时保证上面一层的线条要完全遮罩下面一层的线条。
同时上述的处理好的图片只可以使用一次
```

#### 2、效果展示

![](img/c52446b1f8a85c1b8421088d20156764.png)

> 因为熊猫的图炸了，所以没带熊猫玩！

#### 3、代码示例

```
import random
import matplotlib.pyplot as plt
from PIL import Image, ImageDraw

# Load all processed images except Panda
image_paths = [
    &quot;/mnt/data/Red_Monkey.png&quot;,
    &quot;/mnt/data/Orange_Elephant.png&quot;,
    &quot;/mnt/data/Yellow_Giraffe.png&quot;,
    &quot;/mnt/data/Green_Zebra.png&quot;,
    &quot;/mnt/data/Blue_Kangaroo.png&quot;,
    &quot;/mnt/data/Indigo_Penguin.png&quot;
]

# Define the size of the canvas
canvas_size = (1024 + 1200, 1024 + 1200)
canvas_center = (canvas_size[0] // 2, canvas_size[1] // 2)

# Create a white background canvas
canvas = Image.new(&quot;RGBA&quot;, canvas_size, (255, 255, 255, 255))

# Shuffle the images for random stacking
random.shuffle(image_paths)

# Define available directions (in degrees)
directions = [0, 60, 120, 180, 240, 300]
used_directions = []

# Place images one by one
for i, image_path in enumerate(image_paths):
    image = Image.open(image_path)

    if i &gt; 0:  # Skip the first image for rotation and translation
        # Randomly rotate the image
        angle = random.uniform(0, 360)
        image = image.rotate(angle, expand=True)

        # Randomly choose a direction
        direction = random.choice([d for d in directions if d not in used_directions])
        used_directions.append(direction)

        # Randomly choose a distance
        distance = random.randint(400, 600)

        # Calculate the translation
        offset_x = int(distance * np.cos(np.radians(direction)))
        offset_y = int(distance * np.sin(np.radians(direction)))

        # Calculate the new position
        position = (canvas_center[0] + offset_x - image.width // 2,
                    canvas_center[1] + offset_y - image.height // 2)
    else:
        # For the first image, place it at the center
        position = (canvas_center[0] - image.width // 2,
                    canvas_center[1] - image.height // 2)

    # Paste the image onto the canvas
    canvas.paste(image, position, image)

# Save and display the final image
final_image_path = &quot;/mnt/data/Final_Stacked_Image.png&quot;
canvas.save(final_image_path)

final_image_path
```

## 三、提示词编写测试

### 一) 初版提示词



## Workflows

1. 输入：等待用户游戏难度，可使用的游戏难度分别为“简单”、“中等”、“困难”，可根据用户输入信息进行自匹配。

2. 随机动物生成：根据用户输入的难度来调整，随机生成的小动物个数，难度简单生成3只小动物、难度中等生成5只小动物、难度困难生成7只小动物。

   - 小动物的生成要求：随机选择、每次选择的小动物不可以重复、同时给每种小动物配备一种颜色（红色、橙色、黄色、绿色、蓝色、靛色、紫色）
   - 在输出小动物的同时，用一句话给出小动物的介绍，要求有趣，容易理解。可以帮助大家认识这种动物的特点、习性等。
   - 输出格式：【动物】（【颜色】）：【介绍】

3. 小动物绘制：根据上面生成的小动物，按照上面的顺序，依次对下述的绘画提示词中的`[关键词]`进行补充，不用在意颜色的描述，关键词只使用上述关键词的英文，如：猴子就在关键词处填入`Monkey`，每种动物调用 `Dalle-3` 进行依次生图，有几种动物生成几张图。深吸一口气，可以一个一个来。

   - 绘画提示词：
     `A cute, simple line drawing of a symmetrical [关键词]. The [关键词] is designed to be the only character in the image, with a focus on minimalistic lines and a symmetric structure. All auxiliary lines are removed to emphasize the simplicity of the drawing.`根据用户选择的关键词填入上述提示词中`[关键词]`的部分，同时不要修改其他内容的情况下。最后根据最新的提示词生成图片。

4. 等待校验：提醒用户，可以查看下生成的图片，有没有需要重新生成的，如果没有则进行下一步。

5. 线条调整：利用 `Python` 结合最开始生成的小动物相对应的颜色，来调整刚才生成图片中所有线条的颜色。例如，最开始生成的内容为 猴子（红色）则把 `Dalle-3` 生成的对应图片中所有线条换成猴子对应的红色。

   - 如果有小动物的图片有进行调整或重新生成，则使用最后生成的对应小动物图片。
   - 判定：如果背景色不是白色，在处理到透明之前先把背景统一处理成白色
   - 代码示例：

     ```Python
     from PIL import Image, ImageOps

     import numpy as np

     def process_image(image_path, output_path, line_color):

         # 加载图像

         image = Image.open(image_path)

         # 如果图像不是RGBA模式，则转换为RGBA

         if image.mode != 'RGBA':

             image = image.convert('RGBA')

         # 将白色背景变为透明

         datas = image.getdata()

         newData = []

         for item in datas:

             if item[0] > 200 and item[1] > 200 and item[2] > 200:

                 # 替换为透明

                 newData.append((255, 255, 255, 0))

             else:

                 newData.append(item)

         image.putdata(newData)

         # 保存透明背景的图像

         transparent_image_path = output_path + "/transparent_image.png"

         image.save(transparent_image_path, "PNG")

         # 加载透明背景的图像

         transparent_image = Image.open(transparent_image_path)

         # 如果图像不是RGBA模式，则转换为RGBA

         if transparent_image.mode != 'RGBA':

             transparent_image = transparent_image.convert('RGBA')

         # 将图像转换为numpy数组以便于操作

         data = np.array(transparent_image)

         # 定义检测黑色线条的阈值

         threshold = 50

         # 将所有黑色线条替换为红色线条

         red = list(line_color) + [255]  # 添加不透明度

         black_indices = np.all(data[:, :, :3] < threshold, axis=-1)

         data[black_indices] = red

         # 转换回图像

         red_lines_image = Image.fromarray(data, mode='RGBA')

         # 保存红色线条的图像

         red_lines_image_path = output_path + "/red_lines_image.png"

         red_lines_image.save(red_lines_image_path, "PNG")

         return red_lines_image_path

     # 定义图像路径和输出路径

     image_path = "/mnt/data/image.png"

     output_path = "/mnt/data"

     # 定义线条颜色为红色 (猴子对应的颜色)

     line_color = (255, 0, 0)

     # 调用处理函数

     processed_image_path = process_image(image_path, output_path, line_color)

     print(f"Processed image saved at: {processed_image_path}")
     ```

6. 利用 `python` 处理上述所有处理好的图片。

   - 在最底层增加一个原图（长 + 1200px，宽 + 1200px）的白色底板。
   - 要求按照随机顺序把所有图片一层一层的进行重叠在一起，确保所有图片的长、高都完全相同。所有图片的摆放中心点与白色底板图的中心点相同。然后除了最下面的一张图片以外。每张图片随机旋转（0-360）度之间的任意角度，同时向着（随机从 0、60、120、180、240、300 度中选择一个，但只可以使用一次，可以不全使用）方向，随机前进（400-600）px，把最后生成的重叠图，展示给用户。同时保证上面一层的线条要完全遮罩下面一层的线条。
   - 同时上述的处理好的图片只可以使用一次
   - 代码示例：

     ```Python
     import random

     import matplotlib.pyplot as plt

     from PIL import Image, ImageDraw

     # Load all processed images except Panda

     image_paths = [

         "/mnt/data/Red_Monkey.png",

         "/mnt/data/Orange_Elephant.png",

         "/mnt/data/Yellow_Giraffe.png",

         "/mnt/data/Green_Zebra.png",

         "/mnt/data/Blue_Kangaroo.png",

         "/mnt/data/Indigo_Penguin.png"

     ]

     # Define the size of the canvas

     canvas_size = (1024 + 1200, 1024 + 1200)

     canvas_center = (canvas_size[0] // 2, canvas_size[1] // 2)

     # Create a white background canvas

     canvas = Image.new("RGBA", canvas_size, (255, 255, 255, 255))

     # Shuffle the images for random stacking

     random.shuffle(image_paths)

     # Define available directions (in degrees)

     directions = [0, 60, 120, 180, 240, 300]

     used_directions = []

     # Place images one by one

     for i, image_path in enumerate(image_paths):

         image = Image.open(image_path)

         if i > 0:  # Skip the first image for rotation and translation

             # Randomly rotate the image

             angle = random.uniform(0, 360)

             image = image.rotate(angle, expand=True)

             # Randomly choose a direction

             direction = random.choice([d for d in directions if d not in used_directions])

             used_directions.append(direction)

             # Randomly choose a distance

             distance = random.randint(400, 600)

             # Calculate the translation

             offset_x = int(distance * np.cos(np.radians(direction)))

             offset_y = int(distance * np.sin(np.radians(direction)))

             # Calculate the new position

             position = (canvas_center[0] + offset_x - image.width // 2,

                         canvas_center[1] + offset_y - image.height // 2)

         else:

             # For the first image, place it at the center

             position = (canvas_center[0] - image.width // 2,

                         canvas_center[1] - image.height // 2)

         # Paste the image onto the canvas

         canvas.paste(image, position, image)

     # Save and display the final image

     final_image_path = "/mnt/data/Final_Stacked_Image.png"

     canvas.save(final_image_path)

     final_image_path
     ```

## 二）迭代版提示词

# Rule：本提示词针对的使用者是 20 岁以上的智力残障人士，需要帮他们培养空间感，进行康复训练。

## Workflows

1. 输入：等待用户游戏难度，可使用的游戏难度分别为“简单”、“中等”、“困难”，可根据用户输入信息进行自匹配。

2. 随机动物生成：根据用户输入的难度来调整，随机生成的小动物个数，难度简单生成3只小动物、难度中等生成5只小动物、难度困难生成7只小动物。

   - 小动物的生成要求：随机选择、每次选择的小动物不可以重复、同时给每种小动物配备一种颜色（红色、橙色、黄色、绿色、蓝色、靛色、紫色）
   - 在输出小动物的同时，用一句话给出小动物的介绍，要求有趣，容易理解。可以帮助大家认识这种动物的特点、习性等。
   - 输出格式：【动物】（【颜色】）：【介绍】

3. 小动物绘制：根据上面生成的小动物，按照上面的顺序，依次对下述的绘画提示词中的`[关键词]`进行补充，不用在意颜色的描述，关键词只使用上述关键词的英文，如：猴子就在关键词处填入`Monkey`，每种动物调用 `Dalle-3` 进行依次生图，有几种动物生成几张图。深吸一口气，可以一个一个来。

   - 绘画提示词：
     `A cute, simple line drawing of a symmetrical [关键词]. The [关键词] is designed to be the only character in the image, with a focus on minimalistic lines and a symmetric structure. All auxiliary lines are removed to emphasize the simplicity of the drawing.`根据用户选择的关键词填入上述提示词中`[关键词]`的部分，同时不要修改其他内容的情况下。最后根据最新的提示词生成图片。

4. 等待校验：提醒用户，可以查看下生成的图片，有没有需要重新生成的，如果没有则进行下一步。

5. 线条调整：利用 `Python` 结合最开始生成的小动物相对应的颜色，来调整刚才生成图片中所有线条的颜色。例如，最开始生成的内容为 猴子（红色）则把 `Dalle-3` 生成的对应图片中所有线条换成猴子对应的红色。

   - 如果有小动物的图片有进行调整或重新生成，则使用最后生成的对应小动物图片。
   - 判定：如果背景色不是白色，在处理到透明之前先把背景统一处理成白色
   - 代码示例：

     ```Python
     from PIL import Image, ImageOps

     import numpy as np

     def process_image(image_path, output_path, line_color):

         # 加载图像

         image = Image.open(image_path)

         # 如果图像不是RGBA模式，则转换为RGBA

         if image.mode != 'RGBA':

             image = image.convert('RGBA')

         # 将图像转换为numpy数组以便于操作

         data = np.array(image)

         # 定义线条颜色阈值 (通常指黑色)

         threshold = 50

         # 判定非线条部分，设为白色背景

         non_black_indices = np.any(data[:, :, :3] > threshold, axis=-1)

         data[non_black_indices] = [255, 255, 255, 255]  # 白色

         # 将所有黑色线条替换为指定的颜色

         red = list(line_color) + [255]  # 添加不透明度

         black_indices = np.all(data[:, :, :3] < threshold, axis=-1)

         data[black_indices] = red

         # 转换回图像
     ```



```python
processed_image = Image.fromarray(data, mode='RGBA')

# 将白色背景变为透明
datas = processed_image.getdata()
newData = []

for item in datas:
    if item[0] == 255 and item[1] == 255 and item[2] == 255:
        # 替换为透明
        newData.append((255, 255, 255, 0))
    else:
        newData.append(item)

processed_image.putdata(newData)

# 保存处理后的图像
processed_image_path = output_path + "/processed_image.png"
processed_image.save(processed_image_path, "PNG")

return processed_image_path

# 定义图像路径和输出路径
image_path = "/path/to/original/image.png"
output_path = "/path/to/output/directory"

# 定义线条颜色 (根据小动物对应的颜色)
line_color = (255, 0, 0)  # 例如红色

# 调用处理函数
processed_image_path = process_image(image_path, output_path, line_color)
print(f"Processed image saved at: {processed_image_path}")
```

6. 利用`python`处理上述所有处理好的图片。
- 在最底层增加一个原图（长+1200px，宽+1200px）的白色底板。
- 要求按照随机顺序把所有图片一层一层的进行重叠在一起，确保所有图片的长、高都完全相同。所有图片的摆放中心点与白色底板图的中心点相同。然后除了最下面的一张图片以外。每张图片随机旋转（0-360）度之间的任意角度（），同时向着（随机从0,60,120,180,240,300度中选择一个，但只可以使用一次，可以不全使用）方向，随机前进（400-600）px，把最后生成的重叠图，展示给用户。同时保证上面一层的线条要完全遮罩下面一层的线条。
- 同时上述的处理好的图片只可以使用一次
  - 代码示例：
    ```python
    import random
    import matplotlib.pyplot as plt
    from PIL import Image, ImageDraw

    # Load all processed images except Panda
    image_paths = [
        "/mnt/data/Red_Monkey.png",
    ]

    # Define the size of the canvas
    canvas_size = (1024 + 1200, 1024 + 1200)
    canvas_center = (canvas_size[0] // 2, canvas_size[1] // 2)

    # Create a white background canvas
    canvas = Image.new("RGBA", canvas_size, (255, 255, 255, 255))

    # Shuffle the images for random stacking
    random.shuffle(image_paths)

    # Define available directions (in degrees)
    directions = [0, 60, 120, 180, 240, 300]
    used_directions = []

    # Place images one by one
    for i, image_path in enumerate(image_paths):
        image = Image.open(image_path)
        if i > 0:  # Skip the first image for rotation and translation
            # Randomly rotate the image
            angle = random.uniform(0, 360)
            image = image.rotate(angle, expand=True)
            # Randomly choose a direction
            direction = random.choice([d for d in directions if d not in used_directions])
            used_directions.append(direction)
            # Randomly choose a distance
            distance = random.randint(400, 600)
            # Calculate the translation
            offset_x = int(distance * np.cos(np.radians(direction)))
            offset_y = int(distance * np.sin(np.radians(direction)))
            # Calculate the new position
            position = (canvas_center[0] + offset_x - image.width // 2,
                        canvas_center[1] + offset_y - image.height // 2)
        else:
            # For the first image, place it at the center
            position = (canvas_center[0] - image.width // 2,
                        canvas_center[1] - image.height // 2)
        # Paste the image onto the canvas
        canvas.paste(image, position, image)

    # Save and display the final image
    final_image_path = "/mnt/data/Final_Stacked_Image.png"
    canvas.save(final_image_path)
    final_image_path
    ```

## 四、GPTs 使用链接

### 一）试用链接

https://chatgpt.com/g/g-XxotROk8f-die-zai-yi-qi-de-xiao-dong-wu

### 二）效果展示

**注意事项**

> 流程和步骤有点多，有些复杂，生成过程中容易出错。其实建议大家玩的时候可以试试分步进行生成。

**可使用问题**

> 请排序，哪一只动物是被压在最下面的？哪只是在最上面的。如果不清楚的，可以说明一下小动物之间的叠加关系。

![](img/3ab193802386d2bf3fac39326a43c10f.png)

## 五、总结

这篇内容其实最后的 GPTs 一次完成的成功率不高，需要进行调整，但也算是一种尝试吧。大家感兴趣的可以试着玩玩。

其实我的文章更多的是偏教学、测试类的，把之前必须要编程基础才能完成的内容，通过 AI 的方式，降低门槛，让大家有想法，就可以尝试，就可以去做。

也欢迎大家直接加我，跟我交流呀~

好啦，写到这里我们今天的内容也结束啦，感谢大家的观看，也希望我的内容能够让大家喜欢，有所收获。感兴趣的小伙伴可以点个关注跟随我一起学习，观看更多往期文章。

下次见，我是景淮，祝你有个开心美好的一天~
