
# Stable Diffusion 提示词与 ControlNet 使用指南

## 提示词语法与轮转

例如：`[fantasy:cyberpunk:16] landscape` 在一开始，读入的提示词为：`the model will be drawing a fantasy landscape.` 在第 16 步之后，提示词将被替换为：`a cyberpunk landscape`，它将继续在之前的图像上计算。

又例如，对于提示词为：`fantasy landscape with a [mountain:lake:0.25] and [an oak:a christmas tree:0.75][ in foreground::0.6][ in background:0.25][shoddy:masterful:0.5]`，100 步采样，一开始。提示词为：`fantasy landscape with a mountain and an oak in foreground shoddy` 在第 25 步后，提示词为：`fantasy landscape with a lake and an oak in foreground in background shoddy` 在第 50 步后，提示词为：`fantasy landscape with a lake and an oak in foreground in background masterful` 在第 60 步后，提示词为：`fantasy landscape with a lake and an oak in background masterful` 在第 75 步后，提示词为：`fantasy landscape with a lake and a christmas tree in background masterful`

提示词还可以轮转，比如

`[cow|horse] in a field`

在第一步时，提示词为“cow in a field”；在第二步时，提示词为"horse in a field."；在第三步时，提示词为"cow in a field"，以此类推。

![](img/9177688f226ff5f260cd16a8749b2b9e.png)

## Token

实际上，程序是将输入的关键词以 Token 的形式传入模型进行计算的：

“ (Medieval astronomer using a telescope with a cosmic starry sky in the background.sketch, hand draw style, con, uncomplicated background )”转换为 Token ID 即：263, 10789, 40036, 1996, 320, 19037, 593, 320, 18304, 30963, 2390, 530, 518, 5994, 8, 11, 263, 5269, 267, 2463, 4001, 1844, 267, 5646, 267, 569, 16621, 5994, 264 。

一个单词可能对应一个或多个 Token，多个单词也可能对应同一个 Token。

![](img/528b86a237a06965cb40bab91b022a4e.png)

## 提示词模板

可参考 [Civitai | Stable Diffusion models, embeddings, hypernetworks and more](https://link.zhihu.com/?target=https%3A//civitai.com/) 中优秀作品的提示词作为模板。

类似的网站还有：

+ [MajinAI | Home](https://link.zhihu.com/?target=https%3A//majinai.art/index.php)
+ 词图 PromptTool - AI 绘画资料管理网站
+ [black_lily](https://link.zhihu.com/?target=http%3A//heizicao.gitee.io/novelai/%23/book)
+ [Danbooru 标签超市](https://link.zhihu.com/?target=https%3A//tags.novelai.dev/)
+ [魔咒百科词典](https://link.zhihu.com/?target=https%3A//aitag.top/)
+ [AI 词汇加速器 Accelerator Prompt](https://link.zhihu.com/?target=https%3A//thereisnospon.github.io/NovelAiTag/)
+ NovelAI 魔导书
+ [鳖哲法典](https://link.zhihu.com/?target=http%3A//tomxlysplay.com.cn/%23/)
+ [Tag Groups Wiki | Danbooru (donmai.us)](https://link.zhihu.com/?target=https%3A//danbooru.donmai.us/wiki_pages/tag_groups)
+ AIBooru: Anime Image Board

## 五、Controlnet

Controlnet 允许通过线稿、动作识别、深度信息等对生成的图像进行控制。

请注意，在使用前请确保 ControlNet 设置下的路径与本地 Stable Diffusion 的路径同步。

![](img/a055f42f5c73b71b5511fc0ae1d5b878.png)

![](img/b49b7d02ca889b891abc91fc696bcaa6.png)

### 基本流程

+ 点击 `Enable` 启用该项 ControlNet

+ `Preprocessor` 指预处理器，它将对输入的图像进行预处理。如果图像已经符合预处理后的结果，请选择 `None`。譬如，图中导入的图像已经是 OpenPose 需要的骨架图，那么 `preprocessor` 选择 `none` 即可。

+ 在 `Weight` 下，可以调整该项 ControlNet 的在合成中的影响权重，与在 prompt 中调整的权重类似。`Guidance strength` 用来控制图像生成的前百分之多少步由 Controlnet 主导生成，这点与 `[:]` 语法类似。

+ `Invert Input Color` 表示启动反色模式，如果输入的图片是白色背景，开启它。

+ `RGB to BGR` 表示将输入的色彩通道信息反转，即 `RGB` 信息当做 `BGR` 信息解析，只是因为 `OpenCV` 中使用的是 `BGR` 格式。如果输入的图是法线贴图，开启它。

+ `Low VRAM` 表示开启低显存优化，需要配合启动参数 `--lowvram`。

+ `Guess Mode` 表示无提示词模式，需要在设置中启用基于 CFG 的引导。

+ `Model` 中请选择想要使用解析模型，应该与输入的图像或者预处理器对应。请注意，预处理器可以为空，但模型不能为空。

### 可用预处理/模型

+ `canny` 用于识别输入图像的边缘信息。

![](img/8c15ca25935440a71e0b5bd8252804e4.png)

+ `depth` 用于识别输入图像的深度信息。

![](img/0de908b546c1ebfba0ec525507d8fc75.png)

+ `hed` 用于识别输入图像的边缘信息，但边缘更柔和。

![](img/88f049ebd8e38ebedf2dddbba3e78b1a.png)

+ `mlsd` 用于识别输入图像的边缘信息，一种轻量级的边缘检测。它对横平竖直的线条非常敏感，因此更适用于室内图的生成。

![](img/da8c55c1418ab660c0203abad8e7af44.png)

+ `normal` 用于识别输入图像的法线信息。

![](img/97f8ea29389d7aedd143ec027c80b8d8.png)

+ `openpose` 用于识别输入图像的动作信息。

![](img/ce0f045d8cbb95f68743ff3c33c504b5.png)

OpenPose Editor 插件可以自行修改姿势，导出到文生图或图生图。

![](img/7b79d49b406bd93771e7ad68a9ed2480.png)

+ `scribble` 将输入图像作为线稿识别。如果线稿是白色背景，务必勾选“`Invert Input Color`”

![](img/39d2f19d327c67faba513791049b146a.png)

+ `fake_scribble` 识别输入图像的线稿，然后再将它作为线稿生成图像。

![](img/7fd160acf1ea62bd8b1bb407633336d4.png)

+ `segmentation` 识别输入图像各区域分别是什么类型的物品，再用此构图信息生成图像。

如果想绘制一张符合 [segmentation](https://link.zhihu.com/?target=https%3A//docs.google.com/spreadsheets/d/1se8YEtb2detS7OuPE86fXGyD269pMycAWe2mtKUj2W8/edit%23gid%3D0) 规范的图像，可以使用以下色表绘制。`color_coding_semantic_segmentation_classes` - Google 表格

![](img/9fb70ae9247772ac124abe27650f83b4.png)

### 多 ControlNet 合成

在 ControlNet 的设置下，可以调整可用 ControlNet 的数量。

![](img/2ee21be1b4ae7f484170f1f577aec5c5.png)

在多个 ControlNet 模式下，结果会将输入的信息合并生成图像：

![](img/c50247e2667f69b84f7634c9ef431dbf.png)

## 六、更多模型

### 模型下载

模型能够有效地控制生成的画风和内容。常用的模型网站有：

+ [Civitai | Stable Diffusion models, embeddings, hypernetworks and more](https://link.zhihu.com/?target=https%3A//civitai.com/)
+ [Stable Diffusion models](https://link.zhihu.com/?target=https%3A//www.123114514.xyz/models/ckpt)
+ [models, embeddings, hypernetworks - AI 绘图指南 wiki](https://link.zhihu.com/?target=https%3A//aiguidebook.top/index.php/model/)
+ Models - Hugging Face
+ SD - WebUI 资源站
+ 元素法典 AI 模型收集站 - AI 绘图指南 wiki (aiguidebook.top)
+ AI 绘画模型博物馆 (subrecovery.top)

### 模型安装

下载模型后需要将之放置在指定的目录下，请注意，不同类型的模型应该拖放到不同的目录下。模型的类型可以通过 Stable Diffusion 法术解析检测。

+ 大模型（`Ckpt`）：放入 `models\Stable-diffusion`

![](img/1bec501497789e8f21a4fa561cff585e.png)

+ `VAE` 模型：一些大模型需要配合 `vae` 使用，对应的 `vae` 同样放置在 `models\Stable-diffusion` 或 `models\VAE` 目录，然后在 webui 的设置栏目选择。

![](img/fd55d63063815f05328b974575649f28.png)

![](img/ac983609323586e80cfd73a351a5cebe.png)

+ `Lora`/`LoHA`/`LoCon` 模型：放入 `extensions\sd-webui-additional-networks\models\lora`，也可以在 `models/Lora` 目录

![](img/e9a380295f3ef08cef666fd6d503fe72.png)

![](img/2b122a185dc5b710b41216e63c264561.png)

+ `Embedding` 模型：放入 `embeddings` 目录

![](img/38c0c63f488ac6641ac14eeee673d25c.png)

### 模型使用

+ `Checkpoint`(`ckpt`)模型 对效果影响最大的模型。在 webui 界面的左上角选择使用。

![](img/33af673745505be4f4861831ef7823e8.png)

一些模型会有触发词，即在提示词内输入相应的单词才会生效。

+ `Lora` 模型 / `LoHA` 模型 / `LoCon` 模型

对人物、姿势、物体表现较好的模型，在 `ckpt` 模型上附加使用。在 webui 界面的 `Additional Networks` 下勾选 `Enable` 启用，然后在 `Model` 下选择模型，并可用 `Weight` 调整权重。权重越大，该 `Lora` 的影响也越大。不建议权重过大（超过 1.2），否则很容易出现扭曲的结果。

![](img/92874b36788c583c4cdeef196de5a917.png)

多个 `lora` 模型混合使用可以起到叠加效果，譬如一个控制面部的 `lora` 配合一个控制画风的 `lora` 就可以生成具有特定画风的特定人物。因此可以使用多个专注于不同方面优化的 `Lora`，分别调整权重，结合出自己想要实现的效果。

`LoHA` 模型是一种 `LORA` 模型的改进。

`LoCon` 模型也一种 `LORA` 模型的改进，泛化能力更强。

+ `Embedding`

对人物、画风都有调整效果的模型。在提示词中加入对应的关键词即可。大部分 `Embedding` 模型的关键词与文件名相同，譬如一个名为 `SomeCharacter.pt` 的模型，触发它的关键词检索 `SomeCh`
