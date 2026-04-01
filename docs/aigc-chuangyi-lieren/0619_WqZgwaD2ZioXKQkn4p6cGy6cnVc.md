# Midjourney常用参数-直达

> 来源：[https://aigcreative.feishu.cn/docx/Xjz3dCndYod49xxuQgAcAM1Insh](https://aigcreative.feishu.cn/docx/Xjz3dCndYod49xxuQgAcAM1Insh)

# Midjourney 参数用于控制图像生成过程，可以细分为以下几类：

## **通用参数**

+   **--seed**：指定随机种子，范围为 0 到 4294967295。

+   **--chaos**：控制图像的混乱程度，范围为 0 到 100，数值越高，图像越混乱。

+   **--quality**：控制图像质量，范围为 0.25 到 1，数值越高，图像质量越高，但渲染时间越长。

+   **--aspect**：控制图像宽高比，默认值为 1:1，可以设置为其他比例，例如 16:9 或 4:3。

+   **--style**：控制图像风格，可以指定具体的艺术家或风格名称，例如 "梵高" 或 "蒸汽朋克"。

+   **--no**：排除某些元素，例如 "--no 人脸" 可以生成没有人脸的图像。

## **高级参数**

+   **--iw**：控制图像权重，范围为 0 到 2，数值越高，图像权重越高。

+   **--fast**：使用快速模式生成图像，但质量可能较低。

+   **--repeat**：重复生成图像，可以指定重复次数。

+   **--relax**：使用放松模式生成图像，可以提高图像质量，但渲染时间更长。

+   **--stylize**：控制图像风格化程度，范围为 0 到 1，数值越高，图像风格化程度越高。

+   **--mask**：使用遮罩生成图像，可以指定遮罩图片路径。

## **其他参数**

+   **--prompt**：指定图像描述，这是最重要的参数，可以包含文本、表情符号、特殊符号等。

+   **--v**：指定模型版本，目前最新版本为 v5。

+   **--upscaler**：指定图像放大器，默认值为 "ESRGAN"。

+   --prompt_weights：指定文本权重，可以为不同的文本块指定不同的权重。

+   **--cfg_scale**：控制配置文件缩放比例，可以调整图像的整体风格。

# 以下是一些常用的 Midjourney 参数示例

+   **--seed 12345**: 使用随机种子 12345 生成图像

+   **--chaos 50**: 生成具有中等混乱程度的图像

+   **--quality 1**: 生成最高质量的图像

+   **--aspect 16:9**: 生成宽高比为 16:9 的图像

+   **--style 梵高**: 生成梵高风格的图像

+   **--no 人脸**: 生成没有人脸的图像

# 有关 Midjourney 参数的更多信息，请参阅以下资源:

+   [Midjourney 官方文档: https://do](https://docs.midjourney.com/docs/parameter-list)cs.midjourney.com/docs/parameter-list

+   [M](https://tenten.co/learning/midjourney-beginner-prompt/)idjourney 中文教程: https://tenten.co/learning/midjourney-beginner-prompt/

# **注意:**

+   Midjourney 参数区分大小写

+   参数可以组合使用

+   参数的具体效果取决于图像描述和设置

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>