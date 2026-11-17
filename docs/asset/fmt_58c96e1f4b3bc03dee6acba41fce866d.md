
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
