
# 多模态标记集提示法的实现与应用

文字的呈现形式还是太弱了，未来的智能体必然是多模态的。随着当前的智能体生态越来越好，实现的门槛也越来越低。人人打造、拥有个人智能体的未来值得期待！

![](img/82d843800ae05f245c32c75759ed72a0.png)

以下是我的联系方式，欢迎关注我们🛰️👸号：云中江树，欢迎添加🛰️交流。

![](img/788a885bde976d220f2717b0686ea03e.png)

## 多模态标记集提示法的实现

`LangGPT` 的代码库中使用当前最好的目标检测模型 `YOLOv10` 和最好的语义分割模型 `SAM` 实现了标记集提示法。

`LangGPT` 中的工程实现相比原版还进行了一些调整优化。相比单纯的目标检测，增加了序号标记。相比无序的序号标记，`LangGPT` 的实现进行了从左到右、从上到下的顺序标记。

有代码能力的朋友可参考相应的代码实现，没有的朋友也不用着急，当前的标记模型也存在着非常多漏标错标的问题，对于简单的需求可以采用手标的方式。

### 使用当前最佳的目标检测模型 `YOLOv10` 标记图片

使用 `YOLO` 模型标记的优势是标记速度很快，对人的检测效果比较好。缺点是只能检测预定义的 80 种类别的物体，对于80种类别之外的物体检测效果不佳。

代码

> `https://github.com/langgptai/LangGPT/blob/main/src/ImagePromptingWithYOLOv10.ipynb`

效果

![](img/eeee1bef136f16a04ba3186fadb7e5eb.png)

### 使用 `SAM` 标记

相比 `YOLO`，使用 `SAM` 标记能够标记的更加精准，没有类别限制，效果更好，缺点是标记速度较慢，消耗`GPU`资源更多。

代码

> `https://github.com/langgptai/LangGPT/blob/main/src/SOMPromptingWithSAM.ipynb`

效果

![](img/34398d0d6e1358274ee8f7dfd5f5ddad.png)

实践下来，图片、视频等视觉AI算法当前在实际使用中还是有非常多的问题，可以作为一种辅助提示手段缓解问题，但是无法从根本上解决问题。

用好多模态 AI，任重道远！多模态智能体，曙光在望~

## 更新：标记提示法的实际应用案例

### PDF 完美解析 （20240705）

[`gptpdf` 项目](https://github.com/CosmosShadow/gptpdf) ：`https://github.com/CosmosShadow/gptpdf/blob/main/README_CN.md`

使用 `GPT4o` 完美解析 PDF。

方法简单但是很有效：

1. 使用 `PyMuPDF` 库，对 PDF 进行解析出所有非文本区域，并做好标记。

2. 然后使用视觉大语言模型（如 `GPT-4o`）将 PDF 解析为 `markdown`。

几乎可以完美地解析排版、数学公式、表格、图片、图表等。
