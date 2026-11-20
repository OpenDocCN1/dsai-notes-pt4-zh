
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
