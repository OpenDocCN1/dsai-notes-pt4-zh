
# Mobile-Agent 技术详解

> 来源：https://datawhaler.feishu.cn/docx/GKINdAUAToNGrmxT7lucCVC8nwf
> 恭喜你已经跑通了我们基础的 baseline 部分
> 接下来我们将逐步解析 mobile agent 的核心技术与内容
>
> 别忘了组队辅助表：`https://docs.qq.com/sheet/DTW50em50a2FCSnlX?tab=BB08J2`

## Mobile-Agent简介

![](img/3c71fab554fc5c28e94803b9a4f745b3.png)

`Moblie-Agent-v2`是阿里巴巴与北京交通大学共同提出的多代理协作有效导航的移动设备操作助手，该框架在`ocr`、开放式目标检测技术与多模态模型的基础上构建如上流程的多智能体系统来执行用户指令。

## 初始信息获取

### 代码讲解

我们先从`run.py`看起

```
iter = 0
while True:
    iter += 1
    if iter == 1: # 如果为首轮执行则获取屏幕信息
        screenshot_file = "./screenshot/screenshot.jpg" # 设置保存路径
        perception_infos, width, height = get_perception_infos(
            adb_path, screenshot_file # get_perception_infos获取abd路径以及保存路径
        ) # 执行完毕后返回perception_infos描述信息、width屏幕宽度、height屏幕高并保存截图
        shutil.rmtree(temp_file) # 删除temp文件夹及其文件
        os.mkdir(temp_file)# 创建temp文件夹

        keyboard = False # keyboard表示当前屏幕有无键盘
        keyboard_height_limit = 0.9 * height # 设置键盘高度限制
        for perception_info in perception_infos: # 遍历描述信息
            # 如果目标高度小于keyboard_height_limit则直接遍历下个元素
            if perception_info["coordinates"][1] < keyboard_height_limit: 
                continue
            # 如果ADB Keyboard在描述信息中则将keyboard置为True
            if "ADB Keyboard" in perception_info["text"]:
                keyboard = True
                break
```

在这段代码的逻辑中，首先我们将先获取当前屏幕截屏的信息，这个操作通过`get_perception_infos`函数完成

```
def get_perception_infos(adb_path, screenshot_file):
    # 截图屏幕
    get_screenshot(adb_path)
    # 获取屏幕宽、高
    width, height = Image.open(screenshot_file).size
    # 获取屏幕中文字及位置信息
    text, coordinates = ocr(screenshot_file, ocr_detection, ocr_recognition)
    # 若文本块位置相近则合并文本块
    text, coordinates = merge_text_blocks(text, coordinates)
    # 获取文本块位置中心
    center_list = [
        [(coordinate[0] + coordinate[2]) / 2, (coordinate[1] + coordinate[3]) / 2]
        for coordinate in coordinates
    ]
    # 将几何中心画上红色圆点并保存到screenshot_file
    draw_coordinates_on_image(screenshot_file, center_list)
    # 将text及coordinates存放到perception_infos
    perception_infos = []
    for i in range(len(coordinates)):
        perception_info = {"text": "text: " + text[i], "coordinates": coordinates[i]}
        perception_infos.append(perception_info)
    # 使用groundingdino_model检测屏幕中的图标，并返回图标位置
    coordinates = det(screenshot_file, "icon", groundingdino_model)
    # 将图标位置存入perception_infos
    for i in range(len(coordinates)):
        perception_info = {"text": "icon", "coordinates": coordinates[i]}
        perception_infos.append(perception_info)
    # 获取图标坐标框及id
    image_box = []
    image_id = []
    for i in range(len(perception_infos)):
        if perception_infos[i]["text"] == "icon":
            image_box.append(perception_infos[i]["coordinates"])
            image_id.append(i)
    # 截取坐标框内容，并存为id.jpg
    for i in range(len(image_box)):
        crop(screenshot_file, image_box[i], image_id[i])
    # 获取图标文件夹下所有文件，返回列表
    images = get_all_files_in_folder(temp_file)
    if len(images) > 0:
        # 对图标截图按id进行排序
        images = sorted(images, key=lambda x: int(x.split("/")[-1].split(".")[0]))
        # 获取图标id
        image_id = [int(image.split("/")[-1].split(".")[0]) for image in images]
        # 使用多模态大模型对图标进行描述，并将结果存入icon_map
        icon_map = {}
        prompt = "This image is an icon from a phone screen. Please briefly describe the shape and color of this icon in one sentence."
        if caption_call_method == "local":
            for i in range(len(images)):
                image_path = os.path.join(temp_file, images[i])
                icon_width, icon_height = Image.open(image_path).size
                if (
                    icon_height > 0.8 * height
                    or icon_width * icon_height > 0.2 * width * height
                ):
                    des = "None"
                else:
                    des = generate_local(tokenizer, model, image_path, prompt)
                icon_map[i + 1] = des
        else:
            for i in range(len(images)):
                images[i] = os.path.join(temp_file, images[i])
            icon_map = generate_api(images, prompt)
        # 将icon_map中信息整合到perception_infos
        for i, j in zip(image_id, range(1, len(image_id) + 1)):
            if icon_map.get(j):
                perception_infos[i]["text"] = "icon: " + icon_map[j]
    # 将锚框转为几何中心
    for i in range(len(perception_infos)):
        perception_infos[i]["coordinates"] = [
            int(
                (
                    perception_infos[i]["coordinates"][0]
                    + perception_infos[i]["coordinates"][2]
                )
                / 2
            ),
            int(
                (
                    perception_infos[i]["coordinates"][1]
                    + perception_infos[i]["coordinates"][3]
                )
                / 2
            ),
        ]

    return perception_infos, width, height
```

在`get_perception_infos`函数中，首先我们将通过`get_screenshot`这个函数获取当前的屏幕截图，这个方法通过使用`adb`执行相关指令即可拿到当前屏幕的截图

```
def get_screenshot(adb_path):
    command = adb_path + " shell rm /sdcard/screenshot.png"
    subprocess.run(command, capture_output=True, text=True, shell=True)
    time.sleep(0.5)
    command = adb_path + " shell screencap -p /sdcard/screenshot.png"
    subprocess.run(command, capture_output=True, text=True, shell=True)
    time.sleep(0.5)
    command = adb_path + " pull /sdcard/screenshot.png ./screenshot"
    subprocess.run(command, capture_output=True, text=True, shell=True)
    image_path = "./screenshot/screenshot.png"
    save_path = "./screenshot/screenshot.jpg"
    image = Image.open(image_path)
    image.convert("RGB").save(save_path, "JPEG")
    os.remove(image_path)
```

在这个函数中，我们将会先通过命令`adb shell rm /sdcard/screenshot.png`删除我们设备上的旧的截图，由于我们使用`subprocess`执行这个命令，为了确保在删除再继续执行命令，我们使用`time.sleep(0.5)`强制等待上一个任务完成，删除后我们使用命令`adb shell screencap -p /sdcard/screenshot.png`在该路径下重新获取新的屏幕截图，完成后我们将使用命令`adb pull /sdcard/screenshot.png ./screenshot`将在手机上存储的截图拉取到我们的设备上，接着我们将其转为`RGB`模式，并保存为`JPEG`格式的文件

```
width, height = Image.open(screenshot_file).size

text, coordinates = ocr(screenshot_file, ocr_detection, ocr_recognition)
```

接着我们将获取屏幕的长高宽，以及屏幕上的信息（这里即指含有文字的地方），`ocr_detection`用于检测图像中的文本区域，而`ocr_recognition`用于识别这些区域中的文本，`text_data`和`coordinate`，分别用于存储识别到的文本内容和对应的文本区域的坐标

```
def ocr(image_path, ocr_detection, ocr_recognition):
    text_data = []
    coordinate = []
    
    image_full = cv2.imread(image_path)
    det_result = ocr_detection(image_full)
    det_result = det_result['polygons'] 
    for i in range(det_result.shape[0]):
        pts = order_point(det_result[i])
        image_crop = crop_image(image_full, pts)
        
        try:
            result = ocr_recognition(image_crop)['text'][0]
        except:
            continue

        box = [int(e) for e in list(pts.reshape(-1))]
        box = [box[0], box[1], box[4], box[5]]
        
        text_data.append(result)
        coordinate.append(box)
        
    else:
        return text_data, coordinate
```

在`ocr`这个函数中，主要实现了两个任务，首先我们将通过`ocr_detection(image_full)`对整个图像进行文本区域检测，检测完成后我们将对该区域进行`ocr`识别，获取文字信息，同时我们还将获取文本区域的左上角与右下角坐标`[box[0], box[1], box[4], box[5]]`

```
text, coordinates = merge_text_blocks(text, coordinates)
```

接着我们将进行合并操作，在OCR识别过程中，同一行的文字有可能被识别为不同的块，所以我们这里将位置接近的块做合并，最后返回合并后的文本以及他们的坐标

```
center_list = [
        [(coordinate[0] + coordinate[2]) / 2, (coordinate[1] + coordinate[3]) / 2]
        for coordinate in coordinates
    ]
    draw_coordinates_on_image(screenshot_file, center_list)
```

接着我们在图上画出文本框的中心



![](img/8952e31d752e7862aea746a514783fb7.png)

# 图标感知

接着我们需要获取所有图标，这里我们使用 `GroundingDINO` 来获取当前截图中的图标

```
coordinates = det(screenshot_file, "icon", groundingdino_model)
```

同样的，我们使用 `GroundingDINO` 获取所有图标的位置

```
def det(input_image_path, caption, groundingdino_model, box_threshold=0.05, text_threshold=0.5):
    image = Image.open(input_image_path)
    size = image.size

    caption = caption.lower()
    caption = caption.strip()
    if not caption.endswith('.'):
        caption = caption + '.'
    
    inputs = {
        'IMAGE_PATH': input_image_path,
        'TEXT_PROMPT': caption,
        'BOX_TRESHOLD': box_threshold,
        'TEXT_TRESHOLD': text_threshold
    }

    result = groundingdino_model(inputs)
    boxes_filt = result['boxes']

    H, W = size[1], size[0]
    for i in range(boxes_filt.size(0)):
        boxes_filt[i] = boxes_filt[i] * torch.Tensor([W, H, W, H])
        boxes_filt[i][:2] -= boxes_filt[i][2:] / 2
        boxes_filt[i][2:] += boxes_filt[i][:2]

    boxes_filt = boxes_filt.cpu().int().tolist()
    filtered_boxes = remove_boxes(boxes_filt, size)  # [:9]
    coordinates = []
    for box in filtered_boxes:
        coordinates.append([box[0], box[1], box[2], box[3]])

    return coordinates
```

接着我们将所有的图标取出来，存放在 `temp` 目录下

```
image_box = []
    image_id = []
    for i in range(len(perception_infos)):
        if perception_infos[i]["text"] == "icon":
            image_box.append(perception_infos[i]["coordinates"])
            image_id.append(i)

    for i in range(len(image_box)):
        crop(screenshot_file, image_box[i], image_id[i])
```

```
def crop(image, box, i):
    image = Image.open(image)
    x1, y1, x2, y2 = int(box[0]), int(box[1]), int(box[2]), int(box[3])
    if x1 >= x2 - 10 or y1 >= y2 - 10:
        return
    cropped_image = image.crop((x1, y1, x2, y2))
    cropped_image.save(f"./temp/{i}.jpg")
```

完成图标的存取后，我们将图标送给多模态大模型，让其生成对应的描述

对应的 `prompt` 如下："This image is an icon from a phone screen. Please briefly describe the shape and color of this icon in one sentence."

```
images = get_all_files_in_folder(temp_file)
if len(images) > 0:
    images = sorted(images, key=lambda x: int(x.split("/")[-1].split(".")[0]))
    image_id = [int(image.split("/")[-1].split(".")[0]) for image in images]
    icon_map = {}
    prompt = "This image is an icon from a phone screen. Please briefly describe the shape and color of this icon in one sentence."
    if caption_call_method == "local":
        for i in range(len(images)):
            image_path = os.path.join(temp_file, images[i])
            icon_width, icon_height = Image.open(image_path).size
            if (
                icon_height > 0.8 * height
                or icon_width * icon_height > 0.2 * width * height
            ):
                des = "None"
            else:
                des = generate_local(tokenizer, model, image_path, prompt)
            icon_map[i + 1] = des
    else:
        for i in range(len(images)):
            images[i] = os.path.join(temp_file, images[i])
        icon_map = generate_api(images, prompt)
    for i, j in zip(image_id, range(1, len(image_id) + 1)):
        if icon_map.get(j):
            perception_infos[i]["text"] = "icon: " + icon_map[j]
for i in range(len(perception_infos)):
    perception_infos[i]["coordinates"] = [
        int(
            (
                perception_infos[i]["coordinates"][0]
                + perception_infos[i]["coordinates"][2]
            )
            / 2
        ),
        int(
            (
                perception_infos[i]["coordinates"][1]
                + perception_infos[i]["coordinates"][3]
            )
            / 2
        ),
    ]
```

最后，所有的信息都被整理为

`perception_infos` 代表文字/图标描述信息及中心坐标

`width`, `height` 代表屏幕宽高

```
return perception_infos, width, height
```

现在我们就拿到了我们所需的所有信息

## 决策

### 代码讲解

决策阶段由我们的多模态模型做出回答

```
prompt_action = get_action_prompt(
        instruction,
        perception_infos,
        width,
        height,
        keyboard,
        summary_history,
        action_history,
        summary,
        action,
        add_info,
        error_flag,
        completed_requirements,
        memory,
    )
chat_action = init_action_chat()
chat_action = add_response("user", prompt_action, chat_action, screenshot_file)
def get_action_prompt(instruction, clickable_infos, width, height, keyboard, summary_history, action_history, last_summary, last_action, add_info, error_flag, completed_content, memory):
    '''
```

其中 `instruction` 为我们设定的指令

`clickable_infos` `width` `height` 为前一步我们获取的指定位置的相关信息

`keyboard` 为当前键盘是否可用的信息

`summary_history`, `action_history` 为之前的操作以及这些操作的总结

`last_summary`, `last_action` 为过去行动的信息，当 `error_flag` 不为空时，则会获取最近一次的操作以及情况总结

`add_info` 为当前完成任务需要的额外信息，

`completed_content` 为当前任务的进度

`memory` 为需要的记忆



```
def get_action_prompt(instruction, clickable_infos, width, height, keyboard, summary_history, action_history, last_summary, last_action, add_info, error_flag, completed_content, memory):
    prompt = &quot;### Background ###\n&quot;
    prompt += f&quot;This image is a phone screenshot. Its width is {width} pixels and its height is {height} pixels. The user\&#x27;s instruction is: {instruction}.\n\n&quot;
    
    prompt += &quot;### Screenshot information ###\n&quot;
    prompt += &quot;In order to help you better perceive the content in this screenshot, we extract some information on the current screenshot through system files. &quot;
    prompt += &quot;This information consists of two parts: coordinates; content. &quot;
    prompt += &quot;The format of the coordinates is [x, y], x is the pixel from left to right and y is the pixel from top to bottom; the content is a text or an icon description respectively. &quot;
    prompt += &quot;The information is as follow:\n&quot;

    for clickable_info in clickable_infos:
        if clickable_info[&#x27;text&#x27;] != &quot;&quot; and clickable_info[&#x27;text&#x27;] != &quot;icon: None&quot; and clickable_info[&#x27;coordinates&#x27;] != (0, 0):
            prompt += f&quot;{clickable_info[&#x27;coordinates&#x27;]}; {clickable_info[&#x27;text&#x27;]}\n&quot;
    
    prompt += &quot;Please note that this information is not necessarily accurate. You need to combine the screenshot to understand.&quot;
    prompt += &quot;\n\n&quot;
    
    prompt += &quot;### Keyboard status ###\n&quot;
    prompt += &quot;We extract the keyboard status of the current screenshot and it is whether the keyboard of the current screenshot is activated.\n&quot;
    prompt += &quot;The keyboard status is as follow:\n&quot;
    if keyboard:
        prompt += &quot;The keyboard has been activated and you can type.&quot;
    else:
        prompt += &quot;The keyboard has not been activated and you can\&#x27;t type.&quot;
    prompt += &quot;\n\n&quot;
    
    if add_info != &quot;&quot;:
        prompt += &quot;### Hint ###\n&quot;
        prompt += &quot;There are hints to help you complete the user\&#x27;s instructions. The hints are as follow:\n&quot;
        prompt += add_info
        prompt += &quot;\n\n&quot;
    
    if len(action_history) &gt; 0:
        prompt += &quot;### History operations ###\n&quot;
        prompt += &quot;Before reaching this page, some operations have been completed. You need to refer to the completed operations to decide the next operation. These operations are as follow:\n&quot;
        for i in range(len(action_history)):
            prompt += f&quot;Step-{i+1}: [Operation: &quot; + summary_history[i].split(&quot; to &quot;)[0].strip() + &quot;; Action: &quot; + action_history[i] + &quot;]\n&quot;
        prompt += &quot;\n&quot;
    
    if completed_content != &quot;&quot;:
        prompt += &quot;### Progress ###\n&quot;
        prompt += &quot;After completing the history operations, you have the following thoughts about the progress of user\&#x27;s instruction completion:\n&quot;
        prompt += &quot;Completed contents:\n&quot; + completed_content + &quot;\n\n&quot;
    
    if memory != &quot;&quot;:
        prompt += &quot;### Memory ###\n&quot;
        prompt += &quot;During the operations, you record the following contents on the screenshot for use in subsequent operations:\n&quot;
        prompt += &quot;Memory:\n&quot; + memory + &quot;\n&quot;
    
    if error_flag:
        prompt += &quot;### Last operation ###\n&quot;
        prompt += f&quot;You previously wanted to perform the operation \&quot;{last_summary}\&quot; on this page and executed the Action \&quot;{last_action}\&quot;. But you find that this operation does not meet your expectation. You need to reflect and revise your operation this time.&quot;
        prompt += &quot;\n\n&quot;
    
    prompt += &quot;### Response requirements ###\n&quot;
    prompt += &quot;Now you need to combine all of the above to perform just one action on the current page. You must choose one of the six actions below:\n&quot;
    prompt += &quot;Open app (app name): If the current page is desktop, you can use this action to open the app named \&quot;app name\&quot; on the desktop.\n&quot;
    prompt += &quot;Tap (x, y): Tap the position (x, y) in current page.\n&quot;
    prompt += &quot;Swipe (x1, y1), (x2, y2): Swipe from position (x1, y1) to position (x2, y2).\n&quot;
    if keyboard:
        prompt += &quot;Type (text): Type the \&quot;text\&quot; in the input box.\n&quot;
    else:
        prompt += &quot;Unable to Type. You cannot use the action \&quot;Type\&quot; because the keyboard has not been activated. If you want to type, please first activate the keyboard by tapping on the input box on the screen.\n&quot;
    prompt += &quot;Home: Return to home page.\n&quot;
    prompt += &quot;Stop: If you think all the requirements of user\&#x27;s instruction have been completed and no further operation is required, you can choose this action to terminate the operation process.&quot;
    prompt += &quot;\n\n&quot;
    
    prompt += &quot;### Output format ###\n&quot;
    prompt += &quot;Your output consists of the following three parts:\n&quot;
    prompt += &quot;### Thought ###\nThink about the requirements that have been completed in previous operations and the requirements that need to be completed in the next one operation.\n&quot;
    prompt += &quot;### Action ###\nYou can only choose one from the six actions above. Make sure that the coordinates or text in the \&quot;()\&quot;.\n&quot;
    prompt += &quot;### Operation ###\nPlease generate a brief natural language description for the operation in Action based on your Thought.&quot;
    
    return prompt
```

在决策时，我们最终产生的 `prompt` 如下：



## Background
This image is a phone screenshot. Its width is 1080 pixels and its height is 2400 pixels. The user's instruction is: Open Calendar to see what day it is today.

## Screenshot information
In order to help you better perceive the content in this screenshot, we extract some information on the current screenshot through system files. This information consists of two parts: coordinates; content. The format of the coordinates is [x, y], x is the pixel from left to right and y is the pixel from top to bottom; the content is a text or an icon description respectively. The information is as follow:
[930, 63]; text: LTE
[110, 63]; text: 3:08
[224, 291]; text: Thu, Aug 22
[162, 1655]; text: Play Store
[419, 1654]; text: Gmail
[658, 1654]; text: Photos
[909, 1655]; text: YouTube
[372, 1654]; text: G
[167, 2202]; text: G
[909, 1968]; icon: The Android logo, which looks like a robot with two dots for eyes, is green on a white background inside a gray circle.
[662, 1526]; icon: The app logo, which takes up most of the space on the left side of the circle, has four green petals pointing clockwise starting at the top right corner. The background is white with black border.
[415, 1526]; icon: The logo for Gmail, which takes up most of the space on the circle, has three letters "M" stacked vertically with alternating colors (green, blue, red). The background is white.
[662, 1969]; icon: The icon has a circle design with three main colors, red on top, blue in the middle, green at the bottom.
[168, 1526]; icon: The Google Play logo has three triangles, colored yellow, blue, and red respectively, inside a white circle with black border.
[415, 1968]; icon: The icon features two overlapping blue chat bubbles with a white background, enclosed within a larger circle.
[909, 1526]; icon: The icon is round, with a red play button outlined in white placed slightly to the left on a white background.
[168, 1969]; icon: The icon is shaped like a blue telephone with white background.
[162, 2205]; icon: The Google logo, which appears as a white "G" within a red outlined circle on a blue background.
[915, 2206]; icon: The icon is a circle with a camera graphic inside, all in shades of blue and green.
[789, 2207]; icon: The microphone icon on the phone screen has a rounded square shape with black lines outlining it, and a red circle filled-in at its center.
[165, 1968]; icon: The icon is a blue telephone with white outline, depicting classic design for making voice calls on mobile devices.
[171, 1526]; icon: The Google Play logo is an octagon with rounded corners, filled with red-orange to yellow gradient.
[417, 1524]; icon: The Gmail app logo on my iPhone has a blue "M" with a white background, bordered by a thin orange line.
[910, 1972]; icon: The Android logo, which appears as a green robot with its arms raised against a white background outlined in black.
[416, 1975]; icon: The icon is shaped like two blue speech bubbles overlapping each other, with the top bubble slightly larger than the bottom one.
[662, 1968]; icon: The Google app logo on my home screen has three stacked circles, each smaller than the previous, with colors blue to yellow to red.
Please note that this information is not necessarily accurate. You need to combine the screenshot to understand.

## Keyboard status
We extract the keyboard status of the current screenshot and it is whether the keyboard of the current screenshot is activated.
The keyboard status is as follow:
The keyboard has not been activated and you can't type.

## Hint
There are hints to help you complete the user's instructions. The hints are as follow:
If you want to tap an icon of an app, use the action `Open app`. If you want to exit an app, use the action `Home`.

## Response requirements
Now you need to combine all of the above to perform just one action on the current page. You must choose one of the six actions below:
`Open app` (app name): If the current page is desktop, you can use this action to open the app named "app name" on the desktop.
`Tap` (x, y): Tap the position (x, y) in current page.
`Swipe` (x1, y1), (x2, y2): Swipe from position (x1, y1) to position (x2, y2).
`Unable to Type`. You cannot use the action `Type` because the keyboard has not been activated. If you want to type, please first activate the keyboard by tapping on the input box on the screen.
`Home`: Return to home page.
`Stop`: If you think all the requirements of user's instruction have been completed and no further operation is required, you can choose this action to terminate the operation process.

## Output format
Your output consists of the following three parts:
### Thought
Think about the requirements that have been completed in previous operations and the requirements that need to be completed in the next one operation.
### Action
You can only choose one from the six actions above. Make sure that the coordinates or text in the "()".
### Operation
Please generate a brief natural language description for the operation in Action based on your Thought.

接下来就到了我们具体与大模型交互的环节

### 初始化 `chat_action`

```
# 初始化chat_action
chat_action = init_action_chat()
# 将prompt_action与截图整合到chat_action
chat_action = add_response("user", prompt_action, chat_action, screenshot_file)
```

我们通过使用 `inference_chat` 方法，从我们指定的 LLM 获取我们当前情况下 agent 需要的决策

```
output_action = inference_chat(chat_action, 'gpt-4o', API_url, token)
```

### 修改推理模型

实际上，我们可以通过修改官方提供的 `inference_chat` 接口，来实现更换我们推理的模型（注意，请保证数据格式对齐）

```
def inference_chat(chat, model, api_url, token):    
    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {token}"
    }

    data = {
        "model": model,
        "messages": [],
        "max_tokens": 2048,
        'temperature': 0.0,
        "seed": 1234
    }

    for role, content in chat:
        data["messages"].append({"role": role, "content": content})

    while True:
        try:
            res = requests.post(api_url, headers=headers, json=data)
            res_json = res.json()
            res_content = res_json['choices'][0]['message']['content']
        except:
            print("Network Error:")
            try:
                print(res.json())
            except:
                print("Request Failed")
        else:
            break
    
    return res_content
```

由于 `glm-4` 完全兼容 `openai` API，实际上我们填入 `glm` 的 api 与 key 也能顺利运行

如果修改为 `qwen`，则可以借鉴我们提供的 `run.py` 的写法（需要注意的是，`qwen-vl-puls` 与 `qwen-vl-v1` 的返回格式不同，如果使用需要进行修改）

```
def call_with_local_file(chat, api_key, model):
    response = MultiModalConversation.call(model=model, messages=chat, api_key=api_key)
    return response.output.choices[0].message.content[0]["text"]
```

修改后替换即可

### LLM 最后输出的决策信息如下

```
Thought: We successfully identified the date as Thursday, August 22nd using the location bar at the top of the screenshot. There are several apps available including Gmail, Photos, and YouTube. However, there was no specific task mentioned regarding these apps so I assume they were included for general browsing purposes.
Action: Home
Operation: To return to the home page since there wasn't any specific task related to opening certain apps or tapping icons.
```

完成决策后，我们将对应的聊天记录保存起来



```
thought = (
        output_action.split("### Thought ###")[-1]
        .split("### Action ###")[0]
        .replace("\n", " ")
        .replace(":", "")
        .replace("  ", " ")
        .strip()
    )
summary = (
    output_action.split("### Operation ###")[-1]
    .replace("\n", " ")
    .replace("  ", " ")
    .strip()
)
action = (
    output_action.split("### Action ###")[-1]
    .split("### Operation ###")[0]
    .replace("\n", " ")
    .replace("  ", " ")
    .strip()
)
chat_action = add_response("system", output_action, chat_action)
status = "#" * 50 + " Decision " + "#" * 50
print(status)
print(output_action)
print("#" * len(status))
```

mobile agent 引入了类似于关注重点的机制，我们在行动时可能当前会有我们需要关注的非常重要的信息，这里我们使用大模型来判断当前是否有关于 `insight` 的重要信息，如果有，我们将这些内容放入我们当前的记忆中，同时我们会将这部分信息保存在 `memory` 中，这将影响我们后续的决策

```
if memory_switch:
    prompt_memory = get_memory_prompt(insight)
    chat_action = add_response("user", prompt_memory, chat_action)
    output_memory = call_with_local_file(
        chat_action, api_key=qwen_api, model="qwen-vl-plus"
    )
    chat_action = add_response("system", output_memory, chat_action)
    status = "#" * 50 + " Memory " + "#" * 50
    print(status)
    print(output_memory)
    print("#" * len(status))
    output_memory = (
        output_memory.split("### Important content ###")[-1]
        .split("\n\n")[0]
        .strip()
        + "\n"
    )
    if "None" not in output_memory and output_memory not in memory:
        memory += output_memory
```

接着就是具体的行动部分，由于我们已经知道了当前屏幕上的信息以及他们对应的坐标，我们只需要通过 `adb` 对指定左边坐标操作即可，同样的 ocr 精度会影响我们执行的过程，接着我们强制休眠，等待指令完成

```
if "Open app" in action:
    app_name = action.split("(")[-1].split(")")[0]
    text, coordinate = ocr(screenshot_file, ocr_detection, ocr_recognition)
    tap_coordinate = [0, 0]
    for ti in range(len(text)):
        if app_name == text[ti]:
            name_coordinate = [
                int((coordinate[ti][0] + coordinate[ti][2]) / 2),
                int((coordinate[ti][1] + coordinate[ti][3]) / 2),
            ]
            tap(
                adb_path,
                name_coordinate[0],
                name_coordinate[1] - int(coordinate[ti][3] - coordinate[ti][1]),
            ) #

elif "Tap" in action:
    coordinate = action.split("(")[-1].split(")")[0].split(", ")
    x, y = int(coordinate[0]), int(coordinate[1])
    tap(adb_path, x, y)

elif "Swipe" in action:
    coordinate1 = action.split("Swipe (")[-1].split("), (")[0].split(", ")
    coordinate2 = action.split("), (")[-1].split(")")[0].split(", ")
    x1, y1 = int(coordinate1[0]), int(coordinate1[1])
    x2, y2 = int(coordinate2[0]), int(coordinate2[1])
    slide(adb_path, x1, y1, x2, y2)

elif "Type" in action:
    if "(text)" not in action:
        text = action.split("(")[-1].split(")")[0]
    else:
        text = action.split(' "')[-1].split('"')[0]
    type(adb_path, text)

elif "Back" in action:
    back(adb_path)

elif "Home" in action:
    home(adb_path)

elif "Stop" in action:
    break

time.sleep(5)
```

## 反思

### 代码讲解

当我们完成行动后，我们将会保存最近的信息操作以及截图，这些信息将被用于反思阶段，我们将重复之前的信息获取操作，获取行动后目前的情况

```
# 保存执行动作前屏幕信息
last_perception_infos = copy.deepcopy(perception_infos)
# 定义执行动作前截图路径
last_screenshot_file = "./screenshot/last_screenshot.jpg"
# 保存执行动作前是否有键盘
last_keyboard = keyboard
if os.path.exists(last_screenshot_file):
    os.remove(last_screenshot_file)
os.rename(screenshot_file, last_screenshot_file)
# 获取执行动作后屏幕信息
perception_infos, width, height = get_perception_infos(adb_path, screenshot_file)
# 删除临时文件夹及子文件
shutil.rmtree(temp_file)
# 创建临时文件夹，该文件夹保存grounding_dino识别的图标
os.mkdir(temp_file)
# 获取当前设备中是否有键盘
keyboard = False
for perception_info in perception_infos:
    if perception_info["coordinates"][1] < keyboard_height_limit:
        continue
    if "ADB Keyboard" in perception_info["text"]:
        keyboard = True
        break
```

完成当前情况的检测后，我们将历史情况与当前情况同时传给多模态模型，让其观察操作是否合理以及屏幕信息是否发生变化

```
if reflection_switch:
    # 将信息填充到reflect_prompt模版
    prompt_reflect = get_reflect_prompt(
        instruction,
        last_perception_infos,
        perception_infos,
        width,
        height,
        last_keyboard,
        keyboard,
        summary,
        action,
        add_info,
    )
    # 初始化反思聊天
    chat_reflect = init_reflect_chat()
    # 增添执行动作前后状态对比
    chat_reflect = add_response_two_image(
        "user",
        prompt_reflect,
        chat_reflect,
        [last_screenshot_file, screenshot_file],
    )
    # 获取反思结果
    output_reflect = call_with_local_file(
        chat_action, api_key=qwen_api, model="qwen-vl-plus"
    )
    # 提取反思结果
    reflect = output_reflect.split("### Answer ###")[-1].replace("\n", " ").strip()
    chat_reflect = add_response("system", output_reflect, chat_reflect)
    # 在控制栏中输出反思结果
    status = "#" * 50 + " Reflcetion " + "#" * 50
    print(status)
    print(output_reflect)
    print("#" * len(status))
```

在 `prompt` 中，我们向大模型提供了前后两张图片以及他们的 ocr 图标检测的全部信息，同时提供了我们做出当前操作的思路以及具体的动作内容，我们要求大模型按照我们给定的情况输出它对当前情况的判断

分别为：

A：操作动作的结果符合我对操作思路的预期操作想法

B：操作动作导致页面错误，我需要返回上一页。

C：操作动作未产生任何变化。

```
prompt += "Whether the result of the \"Operation action\" meets your expectation of \"Operation thought\"?\n";
prompt += "A: The result of the \"Operation action\" meets my expectation of \"Operation thought\".\n";
prompt += "B: The \"Operation action\" results in a wrong page and I need to return to the previous page.\n";
prompt += "C: The \"Operation action\" produces no changes.";
prompt += "\n\n";
```



```
def get_reflect_prompt(instruction, clickable_infos1, clickable_infos2, width, height, keyboard1, keyboard2, summary, action, add_info):
    prompt = f"These images are two phone screenshots before and after an operation. Their widths are {width} pixels and their heights are {height} pixels.\n\n"
    
    prompt += "In order to help you better perceive the content in this screenshot, we extract some information on the current screenshot through system files. "
    prompt += "The information consists of two parts, consisting of format: coordinates; content. "
    prompt += "The format of the coordinates is [x, y], x is the pixel from left to right and y is the pixel from top to bottom; the content is a text or an icon description respectively "
    prompt += "The keyboard status is whether the keyboard of the current page is activated."
    prompt += "\n\n"
    
    prompt += "### Before the current operation ###\n"
    prompt += "Screenshot information:\n"
    for clickable_info in clickable_infos1:
        if clickable_info['text'] != "" and clickable_info['text'] != "icon: None" and clickable_info['coordinates'] != (0, 0):
            prompt += f"{clickable_info['coordinates']}; {clickable_info['text']}\n"
    prompt += "Keyboard status:\n"
    if keyboard1:
        prompt += f"The keyboard has been activated."
    else:
        prompt += "The keyboard has not been activated."
    prompt += "\n\n"
            
    prompt += "### After the current operation ###\n"
    prompt += "Screenshot information:\n"
    for clickable_info in clickable_infos2:
        if clickable_info['text'] != "" and clickable_info['text'] != "icon: None" and clickable_info['coordinates'] != (0, 0):
            prompt += f"{clickable_info['coordinates']}; {clickable_info['text']}\n"
    prompt += "Keyboard status:\n"
    if keyboard2:
        prompt += f"The keyboard has been activated."
    else:
        prompt += "The keyboard has not been activated."
    prompt += "\n\n"
    
    prompt += "### Current operation ###\n"
    prompt += f"The user's instruction is: {instruction}. You also need to note the following requirements: {add_info}. In the process of completing the requirements of instruction, an operation is performed on the phone. Below are the details of this operation:\n"
    prompt += "Operation thought: " + summary.split(" to ")[0].strip() + "\n"
    prompt += "Operation action: " + action
    prompt += "\n\n"
    
    prompt += "### Response requirements ###\n"
    prompt += "Now you need to output the following content based on the screenshots before and after the current operation:\n"
    prompt += "Whether the result of the \"Operation action\" meets your expectation of \"Operation thought\"?\n"
    prompt += "A: The result of the \"Operation action\" meets my expectation of \"Operation thought\".\n"
    prompt += "B: The \"Operation action\" results in a wrong page and I need to return to the previous page.\n"
    prompt += "C: The \"Operation action\" produces no changes."
    prompt += "\n\n"
    
    prompt += "### Output format ###\n"
    prompt += "Your output format is:\n"
    prompt += "### Thought ###\nYour thought about the question\n"
    prompt += "### Answer ###\nA or B or C"
    
    return prompt
```

对于不同的情况，我们做出不同的处理，当出现 BC 时，我们认为操作失败，所以设置 `error_flag = True`，同时对于`B`情况，我们可以使用 `adb` 来撤销我们的上一条指令

```
    if "A" in reflect:
        thought_history.append(thought)
        summary_history.append(summary)
        action_history.append(action)

        prompt_planning = get_process_prompt(
            instruction,
            thought_history,
            summary_history,
            action_history,
            completed_requirements,
            add_info,
        )
        chat_planning = init_memory_chat()
        chat_planning = add_response("user", prompt_planning, chat_planning)
        output_planning = call_with_local_file(
            chat_action, api_key=qwen_api, model="qwen-vl-plus"
        )
        chat_planning = add_response("system", output_planning, chat_planning)
        status = "#" * 50 + " Planning " + "#" * 50
        print(status)
        print(output_planning)
        print("#" * len(status))
        completed_requirements = (
            output_planning.split("### Completed contents ###")[-1]
            .replace("\n", " ")
            .strip()
        )

        error_flag = False

elif "B" in reflect:
    error_flag = True
    back(adb_path)

elif "C" in reflect:
    error_flag = True

else:
    thought_history.append(thought)
    summary_history.append(summary)
    action_history.append(action)

    prompt_planning = get_process_prompt(
        instruction,
        thought_history,
        summary_history,
        action_history,
        completed_requirements,
        add_info,
    )
    chat_planning = init_memory_chat()
    chat_planning = add_response("user", prompt_planning, chat_planning)
    output_planning = call_with_local_file(
        chat_action, api_key=qwen_api, model="qwen-vl-plus"
    )
    chat_planning = add_response("system", output_planning, chat_planning)
    status = "#" * 50 + " Planning " + "#" * 50
    print(status)
    print(output_planning)
    print("#" * len(status))
    completed_requirements = (
        output_planning.split("### Completed contents ###")[-1]
        .replace("\n", " ")
        .strip()
    )
```

## 计划

### 代码讲解

对于`A`情况，当当前任务执行成功时，我们将成功的经历放入我们的 `thought_history`、`summary_history`以及`action_history` 中

```
if "A" in reflect:
    thought_history.append(thought)
    summary_history.append(summary)
    action_history.append(action)

    prompt_planning = get_process_prompt(
        instruction,
        thought_history,
        summary_history,
        action_history,
        completed_requirements, # 已经完成的操作，初始为""
        add_info,
    )
```

接下来我们需要大模型判断，当前任务的完成情况，在这一步中，我们需要将之前的操作步骤与行动想法汇总给大模型，让其判断我们当前的任务进行到了什么阶段，最后我们将已完成的内容存在 `completed_requirements` 中



```
def get_process_prompt(instruction, thought_history, summary_history, action_history, completed_content, add_info):
    prompt = "&quot;### Background ###\n&quot;"
    prompt += f&quot;There is an user\&#x27;s instruction which is: {instruction}. You are a mobile phone operating assistant and are operating the user\&#x27;s mobile phone.\n\n&quot;
    
    if add_info != &quot;&quot;:
        prompt += &quot;### Hint ###\n&quot;
        prompt += &quot;There are hints to help you complete the user\&#x27;s instructions. The hints are as follow:\n&quot;
        prompt += add_info
        prompt += &quot;\n\n&quot;
    
    if len(thought_history) &gt; 1:
        prompt += &quot;### History operations ###\n&quot;
        prompt += &quot;To complete the requirements of user\&#x27;s instruction, you have performed a series of operations. These operations are as follow:\n&quot;
        for i in range(len(summary_history)):
            operation = summary_history[i].split(&quot; to &quot;)[0].strip()
            prompt += f&quot;Step-{i+1}: [Operation thought: &quot; + operation + &quot;; Operation action: &quot; + action_history[i] + &quot;]\n&quot;
        prompt += &quot;\n&quot;
        
        prompt += &quot;### Progress thinking ###\n&quot;
        prompt += &quot;After completing the history operations, you have the following thoughts about the progress of user\&#x27;s instruction completion:\n&quot;
        prompt += &quot;Completed contents:\n&quot; + completed_content + &quot;\n\n&quot;
        
        prompt += &quot;### Response requirements ###\n&quot;
        prompt += &quot;Now you need to update the \&quot;Completed contents\&quot;. Completed contents is a general summary of the current contents that have been completed based on the ### History operations ###.\n\n&quot;
        
        prompt += &quot;### Output format ###\n&quot;
        prompt += &quot;Your output format is:\n&quot;
        prompt += &quot;### Completed contents ###\nUpdated Completed contents. Don\&#x27;t output the purpose of any operation. Just summarize the contents that have been actually completed in the ### History operations ###.&quot;
        
    else:
        prompt += &quot;### Current operation ###\n&quot;
        prompt += &quot;To complete the requirements of user\&#x27;s instruction, you have performed an operation. Your operation thought and action of this operation are as follows:\n&quot;
        prompt += f&quot;Operation thought: {thought_history[-1]}\n&quot;
        operation = summary_history[-1].split(&quot; to &quot;)[0].strip()
        prompt += f&quot;Operation action: {operation}\n\n&quot;
        
        prompt += &quot;### Response requirements ###\n&quot;
        prompt += &quot;Now you need to combine all of the above to generate the \&quot;Completed contents\&quot;.\n&quot;
        prompt += &quot;Completed contents is a general summary of the current contents that have been completed. You need to first focus on the requirements of user\&#x27;s instruction, and then summarize the contents that have been completed.\n\n&quot;
        
        prompt += &quot;### Output format ###\n&quot;
        prompt += &quot;Your output format is:\n&quot;
        prompt += &quot;### Completed contents ###\nGenerated Completed contents. Don\&#x27;t output the purpose of any operation. Just summarize the contents that have been actually completed in the ### Current operation ###.\n&quot;
        prompt += &quot;(Please use English to output)&quot;
        
    return prompt
```

```
    # 初始计划对话：
    # {&#x27;role&#x27;: &#x27;system&#x27;,&#x27;content&#x27;: [{&#x27;text&#x27;: &quot;You are a helpful AI mobile phone operating assistant.&quot;}]}
    chat_planning = init_memory_chat()
    # prompt_planning为get_process_prompt生成的prompt，chat_planning
    chat_planning = add_response(&quot;user&quot;, prompt_planning, chat_planning)
    # 获取大模型反馈
    output_planning = call_with_local_file(
        chat_action, api_key=qwen_api, model=&quot;qwen-vl-plus&quot;
    )
    chat_planning = add_response(&quot;system&quot;, output_planning, chat_planning)
    # 输出历史操作
    status = &quot;#&quot; * 50 + &quot; Planning &quot; + &quot;#&quot; * 50
    print(status)
    print(output_planning)
    print(&quot;#&quot; * len(status))
    completed_requirements = (
        output_planning.split(&quot;### Completed contents ###&quot;)[-1]
        .replace(&quot;\n&quot;, &quot; &quot;)
        .strip()
    )

    error_flag = False
```

最后我们将重新回到第一步信息获取，继续执行我们的相关任务

## 提分要点

+ 提高 ocr 的精度
+ 提高 icon 获取的精度（部分icon的识别精度目前不高）
+ 修改生成 icon 描述的 prompt ，达到更高的准确性
+ 打开老年人模式（
+ 当我们使用国产多模态模型时，目前国产多模态模型在普通的推理任务上效果其实不如我们的语言模型（多模态模型的尺寸往往比较小），在确保ocr 与图标检测的精度够高时，我们可以考虑使用纯语言模型来处理整个流程，因为我们拿到的信息已经相当丰富了
+ 在处理 ocr 模型时，我们可以考虑将坐标位置重新排序，让重要的信息聚拢在一起，比如图标与图标下的信息，我们可以考虑硬编码来解决，也可以考虑增加一次 LLM 的推理过程来优化这部分信息
+ 动态改变不同任务的所需前置信息，在我们使用谷歌模拟器时，如果出现了跨页面，或需要呼出所有图标的页面，实际上，模型在决策时缺少这样的先验知识，我们可以考虑让模型先获取每个页面的信息与跨页面手段，接着做决策
+ 调整每个阶段关键的 prompt

## [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/I0sywipHNiwyCJkXSsfcH71Mnsb)！

> 本文档主要由    写作贡献，
>
> 融合了多位助教的内测体验和宝贵建议。
>
> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~
>
> ### 另外别忘了写笔记打卡呀！！~
>
> ### 及时思考、沉淀和产出能让你的学习事半功倍！
>
> 注：本文档采用 **署名—非商业性使用**—相同方式共享 4.0 协议国际版许可协议进行许可。
