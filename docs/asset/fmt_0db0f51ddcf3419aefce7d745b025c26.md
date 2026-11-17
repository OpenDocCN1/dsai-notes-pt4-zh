
# Python 代码示例

```python
from dotenv import load_dotenv
import os
import time # time模块在此版本中未直接使用，但保留以防未来需要
from PIL import Image, ImageDraw
from matplotlib import pyplot as plt

# Import namespaces
from azure.ai.vision.imageanalysis import ImageAnalysisClient
from azure.ai.vision.imageanalysis.models import VisualFeatures
from azure.core.credentials import AzureKeyCredential
from azure.core.exceptions import HttpResponseError # 导入用于错误处理

def main():
    global cv_client # 将 cv_client 声明为全局变量，以便 GetTextRead 可以访问

    try:
        # Get Configuration Settings
        load_dotenv()
        ai_endpoint = os.getenv('AI_SERVICE_ENDPOINT')
        ai_key = os.getenv('AI_SERVICE_KEY')

        if not ai_endpoint or not ai_key:
            print("错误：环境变量 AI_SERVICE_ENDPOINT 或 AI_SERVICE_KEY 未设置。")
            print("请确保您的 .env 文件包含这些值。")
            return

        # Authenticate Azure AI Vision client
        cv_client = ImageAnalysisClient(
            endpoint=ai_endpoint,
            credential=AzureKeyCredential(ai_key)
        )

        # Menu for text reading functions
        print('\n1: Use Read API for image (Lincoln.jpg)\n2: Read handwriting (Note.jpg)\nAny other key to quit\n')
        command = input('Enter a number:')
        if command == '1':
            image_file = os.path.join('images', 'Lincoln.jpg')
            if not os.path.exists(image_file):
                print(f"错误：图片文件 '{image_file}' 未找到。")
                return
            GetTextRead(image_file)
        elif command == '2':
            image_file = os.path.join('images', 'Note.jpg')
            if not os.path.exists(image_file):
                print(f"错误：图片文件 '{image_file}' 未找到。")
                return
            GetTextRead(image_file)
        # else: # 任何其他键退出
        #     print("程序退出。")

    except Exception as ex:
        print(f"发生错误: {ex}")

def GetTextRead(image_file):
    print(f'\n正在从 {image_file} 读取文本...')

    # Open image file
    with open(image_file, "rb") as f:
        image_data = f.read()

    result = None # 初始化 result

    try:
        # Use Analyze image function to read text in image
        result = cv_client.analyze(
            image_data=image_data,
            visual_features=[VisualFeatures.READ]
        )
    except HttpResponseError as e:
        print(f"--- 调用 API 时发生错误 ---")
        print(f"状态码: {e.status_code}")
        print(f"原因: {e.reason}")
        if hasattr(e, 'error') and hasattr(e.error, 'message'):
            print(f"错误消息: {e.error.message}")
        return # 如果API调用失败则退出函数
    except Exception as general_ex:
        print(f"--- 分析过程中发生未知错误 ---")
        print(general_ex)
        return

    # Display the image and overlay it with the extracted text
    if result and result.read is not None: # 确保 result 和 result.read 都有效
        print("\nText:")

        # Prepare image for drawing
        image = Image.open(image_file)
        # fig = plt.figure(figsize=(image.width/100, image.height/100)) # 创建与图片大小匹配的figure
        # plt.axis('off') # 关闭坐标轴
        draw = ImageDraw.Draw(image)
        color = 'cyan' # 边界框颜色

        for line in result.read.blocks[0].lines: # 假设文本在第一个 block 中
            # Return the text detected in the image
            print(f"  {line.text}")

            # drawLinePolygon = True # 这个变量在手册中用于C#控制是否画线框，Python中可以直接画

            # Convert Azure's BoundingPolygon to PIL/Pillow format
            # line.bounding_polygon is a list of Point objects like {'x': 10, 'y': 20}
            line_bounding_polygon_points = []
            for point in line.bounding_polygon:
                line_bounding_polygon_points.append((point.x, point.y))

            # Return the position bounding box around each line
            print("   Bounding Polygon: {}".format(line_bounding_polygon_points)) # 打印转换后的多边形点

            # Draw line bounding polygon (手册中先画线，再画词，这里我们只画词的框，如手册最终效果)
            # if drawLinePolygon: # 如果要画线的框，取消注释下面这行
            #     draw.polygon(line_bounding_polygon_points, outline=color, width=3)

            # Return each word detected in the image and the position bounding box around each word with the confidence level of each word
            for word in line.words:
                word_bounding_polygon_points = []
                for point in word.bounding_polygon:
                    word_bounding_polygon_points.append((point.x, point.y))

                print(f"    Word: '{word.text}', Bounding Polygon: {word_bounding_polygon_points}, Confidence: {word.confidence:.4f}")

                # Draw word bounding polygon
                # drawLinePolygon = False # 手册C#中用于切换，Python中直接绘制
                draw.polygon(word_bounding_polygon_points, outline=color, width=3) # 绘制每个单词的边界框

        # Save image (使用Pillow保存，因为我们用Pillow绘制的)
        outputfile = 'text.jpg'
        image.save(outputfile)
        print(f'\n  带有文本高亮的图片已保存至: {outputfile}')

        # 如果想用 matplotlib 显示和保存（如手册中的fig.savefig）：
        # 需要在循环前创建 fig 和 plt.axis('off')
        # 并在循环后执行以下操作：
        # plt.imshow(image)
        # plt.tight_layout(pad=0)
        # outputfile_mpl = 'text_mpl.jpg'
        # fig.savefig(outputfile_mpl)
        # print(f'\n  Matplotlib 保存的图片: {outputfile_mpl}')
        # plt.show() # 可选，直接显示图片

    elif result and result.read is None:
        print("\n未在图片中检测到文本。")
    else:
        print("\n分析未返回有效结果。")

if __name__ == "__main__":
    main()
```

# 环境配置与初始化（`main()` 函数的开头部分）

## 功能
此模块负责设置程序运行所需的环境。

## 具体操作
- 加载 `.env` 文件中的环境变量，主要是 Azure AI Vision 服务的 API 端点（`AI_SERVICE_ENDPOINT`）和密钥（`AI_SERVICE_KEY`）。这是为了安全地存储和访问敏感信息。
- 使用获取到的端点和密钥，初始化并认证 `ImageAnalysisClient` 对象（`cv_client`）。这个 `cv_client` 是后续与 Azure 服务进行所有通信的接口。

## 与输出的关联
这部分操作在后台完成，用户直接看到的输出始于菜单。如果这里配置错误（例如，API Key 无效），`GetTextRead()` 中的 API 调用会失败。

# 用户交互与任务分发（`main()` 函数的菜单部分）

## 功能
提供一个简单的命令行菜单，让用户选择要执行的操作（是分析印刷体图像还是手写体图像）。

## 具体操作
- 向用户显示选项："1: Use Read API for image (Lincoln.jpg)" 和 "2: Read handwriting (Note.jpg)"。
- 接收用户的输入。
- 根据用户的选择（在您的例子中是 '1'），确定要处理的图像文件路径（`images\Lincoln.jpg`）。
- 调用核心的文本读取函数 `GetTextRead()`，并将选定的图像文件路径传递给它。

## 与输出的关联
您的输出清晰地显示了菜单和您选择的 '1'：

```
      1: Use Read API for image (Lincoln.jpg)
2: Read handwriting (Note.jpg)
Any other key to quit

Enter a number:1
```

# 图像数据准备与 API 调用（`GetTextRead()` 函数的开头部分）

## 功能
负责读取本地图像文件，并调用 Azure AI Vision 服务进行文本识别。

## 具体操作
- 打印提示信息，告知用户正在处理哪个文件（例如，正在从 `images\Lincoln.jpg` 读取文本...）。
- 以二进制格式打开并读取指定的图像文件，将其内容存入 `image_data`。
- 使用之前初始化的 `cv_client` 对象，调用其 `analyze()` 方法。关键参数是：
  - `image_data`：包含图像二进制内容。
  - `visual_features=[VisualFeatures.READ]`：明确指示 Azure 服务执行文本读取（OCR）功能。
- API 调用成功后，结果会存储在 `result` 对象中。

## 与输出的关联
```
正在从 images\Lincoln.jpg 读取文本...
```
这行输出直接对应了此模块开始处理图像的动作。后续的文本输出都来源于 `result` 对象中的数据。

# 文本结果解析与控制台输出（`GetTextRead()` 函数中处理 `result.read` 的部分）

## 功能
解析从 Azure 服务返回的结构化文本识别结果，并在控制台友好地显示这些信息。

## 具体操作
- 检查 `result` 和 `result.read` 是否有效，确保服务成功返回了文本信息。
- 遍历识别到的文本行（`line in result.read.blocks[0].lines`）。
- 对于每一行：
  - 打印该行的完整文本（`line.text`）。
  - 打印该行在图像中的边界多边形坐标（`line.bounding_polygon`）。
  - 进一步遍历该行中的每一个单词（`word in line.words`）。
- 对于每一个单词：
  - 打印单词的文本内容（`word.text`）。
  - 打印单词在图像中的边界多边形坐标（`word.bounding_polygon`）。
  - 打印单词识别的置信度（`word.confidence`）。

## 与输出的关联
这部分代码直接生成了您提供的大部分控制台输出。

# 图像处理与结果可视化（`GetTextRead()` 函数中使用 `Pillow` 绘图和保存的部分）
