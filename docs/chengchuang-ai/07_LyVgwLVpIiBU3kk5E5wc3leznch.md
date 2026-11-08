# ✍精通Stable Diffusion画图，理解LoRA、Dreambooth、Hypernetworks四大模型差异

> 来源：[https://chengchaungai.feishu.cn/docx/Jzu9d00bgop5PLx2izucKLzNn5e](https://chengchaungai.feishu.cn/docx/Jzu9d00bgop5PLx2izucKLzNn5e)

![](img/917f0ff9958fd0208d6fddb583123c41.png)

————**选择人工智能，链接无限未来**————

本文编辑:**JUGO** **  ****  **                                                         👸号：橙创AI

![](img/965c3fdc5b56a7cb684d7a91ee5c032d.png)

![](img/42ce4d0042149196112d3495f967ebbb.png)

**进入👉 **[🍊橙创AI知识库（首页）](https://ix9q8jjj4ug.feishu.cn/wiki/DOsXwmRDaibJVUkDSHKcYbhmn0f)**获取更多A****IGC内****容👈**

> **查阅、整理和输出教程属实**[**不易**](https://ki6j1b0d92h.feishu.cn/wiki/wikcnOTLuqHXX6ZJ1oWEBGbd9zf?from=from_copylink)，觉得这篇教程对你有所帮助的话，不妨分享给更多需要的人呗😊

随着生成型AI技术的能力提升，越来越多的同行开始将注意力放在了通过AI模型提升研发效率上。业内比较火的AI模型有很多，比如画图神器Midjourney、用途多样的Stable Diffusion，以及OpenAI此前刚刚迭代的DALL-E 2，除了后者使用人数有限之外，前两个都有很多的开发者尝试。

不过，对于研发团队而言，尽管Midjourney功能强大且不需要本地安装，但它对于硬件性能的要求较高，甚至同一个指令每次得到的结果都不尽相同。相对而言，功能多、开源、运行速度快，且能耗低内存占用小的Stable Diffusion成为了更理想的选择。

最近，甚至有人用Stable Diffusion和Dreambooth训练出了一个可以模仿人类插画师风格的AI，仅用了32张作品，就训练出了和插画师Hollie Mengert一模一样风格的艺术作品。

目前，训练Stable Diffusion模型的方法主要有四种，它们分别是：Dreambooth、Textual Inversion、LoRA和Hypernetworks。那么，这些模型的特点是什么？哪一个更适合开发者使用呢？

![](img/d5901041aeb4d2b86323dcf6ac3c7349.png)

# **Dreambooth**

| DreamBooth是谷歌推出的一个主题驱动的AI生成模型，它可以微调文本到图像扩散模型或新图像的结果。Dreambooth可以做一些其他扩散模型不能或者不擅长的事情，比如DALL-E 2、Midjourney以及Stable Diffusion等模型都对主题缺乏情景化。 | ![](img/e7090a8bf51e15e6d2b43c2467dd3802.png) |
| --- | --- |

## **工作原理**

| 只要有少量图片作为输入（通常3-5张），Dreambooth就可以在调整后的Imagen和其他一些扩散模型的帮助下，生成具有不同背景的基于主题的个性化图像。一旦有图片输入，调整后的Imagen和其他扩散模型就找到唯一标识符，并将其与主题联系起来。在推理时，唯一标识符被用于合成不同上下文中的主题。 | ![](img/2ce91a4b133b7ddacee33ff27c7aacca.png) |
| --- | --- |

## **使用方法**

1）准备输入图片：如果想将你自己变成AI美术，最少准备五张清晰的照片，并且按照后续步骤上传至Colab notebook。输入照片越多越好，如果给出的数量较少，那么代码本身就会生成一些输入图片用于训练。

由于上传图片数量不限，所以你可以输入任何数字的图片。拍一些中等尺寸照片和不同角度不同光照的全尺寸照片，另外，不要上传光线较差或者太暗的照片。当然，你也可以用明星照片训练Dreambooth。

2）前往谷歌Colab Notebook：目前，能用Stable Diffusion运行Dreambooth的Colab Notebook有三个：Hugging Face、ShivamShirao和TheLastBen。

考虑到速度问题和pf VRAM的用途，这里我们暂时用TheLastBen Colab notebook训练和生成图片。在你的电脑上打开TheLastBen Colab notebook，点击“File”和“Save a copy in Drive”。

![](img/12682b0d6d0f4616e46c7849f7651279.png)

3）从Hugging Face获得访问标记：要使用任何与Dreambooth相关的谷歌Colab，你需要从Hugging Face获得访问标记。

前往Hugging Face网站并用电子邮件地址注册，使用企业邮箱可以帮你找到同事并加入团队。然后到通过点击“Profile icon”进入设置页面，点击“Access Token”，然后点击“New Token”创建你的访问标记。

创建标记的时候，你必须选择“Write”角色，然而，你可以任意对标记命名，使用与你要访问的平台相关的名称是一种很好的做法，这里的平台是Colab notebook。最后，复制你创建的标记。

![](img/3f34319f6650a0ab58121631c3581aac.png)

4）运行Colab notebook：打开了复制的TheLastBen的Colab notebook之后，在“Downloading the model”部分点击Hugging Face链接，接受条款，随后点击“Access repository”。

![](img/992c0a29fe0cb14492c69c1126b6659a.png)

现在，你就可以在“Downloading the model”下看到“Huggingface_Token”区域。粘贴第三步复制的标记。然后你需要逐个运行cell，即运行第一个cell并等待绿色对号之后开始下一个cell。

![](img/c1a8d4a5fe671d6e08ef8681b4c5bb75.png)

运行完第一个cell之后，你会看到一个来自Colab的连接你的Google Drive文件的许可请求，点击“Connect to Google Drive”即可。

![](img/d9489b1f462b4c71497fa05ffe8d2895.png)

运行“Setting up”cell之前，确保输入主题名字、实例名，并提到你要训练或上传的图片数量。

![](img/d1c2c498de4c38176fae8e1b2beab550.png)

然后开始运行这个cell，点击“Choose files”按钮，如果你上传的图片数量较少，可以点击这个按钮，如果数量较大，在“Instance_DIR_Optional”区域提到文件夹URL即可。

将第七个cell设置为可选项，接下来运行第八个cell“Start Dreambooth”，最后一个cell将耗时30分钟至90分钟完成。

5）在Google Drive里检查输出图片：最后，在你的Google Drive里检查AI生成的图片即可。

| ![](img/0f25b71c52d34df2baa287798de6fcbe.png) | ![](img/19b952e032e3e17f1017f1b165b17626.png) | ![](img/bed61e061a929b4ad2710bbfd73e29a1.png) |
| --- | --- | --- |

# **Textual ****Inversion**

| Textual Inversion是一种从少量示例图像中捕捉新概念的技术，随后可以用来控制文本到图像的管道，它通过在管道的文本编码器的embedding空间中学习新的“单词”来做到这一点。然后，这些特殊的单词可以在文本提示中使用，以实现对生成图像的细粒度控制。 | ![](img/a982377f028a23b584a6bbf567adb4f0.png) |
| --- | --- |

## 工作原理

![](img/5a62ccbfe74a1a08f885daf140fe34f2.png)

在一个文字指令被用于扩散模型之前，它必须首先被处理成数值化呈现（numerical representation），这通常包括对文本进行标记，将每个标记转换为embedding，然后通过模型（通常是转换器）进行嵌入，该模型的输出将用作扩散模型的条件。

Textual Inversion学习一个新的标志embedding（即上图中的V*）。一个指令（包括将被映射到该新embedding的标志）与一个或多个训练图像的噪声版本结合使用，作为生成器模型的输入，生成器模型试图预测图像的去噪版本。embedding是根据模型在这项任务中的表现来优化的，更好地捕捉训练图像所显示的对象或风格的embedding将为扩散模型提供更多有用的信息，从而降低去噪损失。经过许多步骤（通常是几千步）和各种指令与图像变体之后，学习到的embedding应该有望捕捉到新概念的本质。

除了使用你自己训练的概念之外，新的Stable Diffusion公开概念库还有社区创作的textual inversion训练模型，你同样可以使用。随着时间的推移，更多的示例加入，会让它变成非常有用的资源。

2、示例：本地运行

这里的textual_inversion.py脚本展示了如何实现训练过程，并使其适应Stable Diffusion。

运行脚本之前，确保先安装该库的training dependencies

pip install diffusers[training] accelerate transformers

然后用accelerate config初始化一个Accelerate环境

3、猫玩具示例

在下载或使用权重之前，你需要接受模型授权，这个案例中我们使用v1-4，这样你就需要访问其卡片（card），阅读授权然后勾选同意授权。

你必须是Hugging Face Hub的注册用户，还需要获得一个访问标记才能让代码工作。运行以下指令验证你的标记：

huggingface-cli login

下载三四张图片作为训练数据，然后用以下代码训练：

export MODEL_NAME=”runwayml/stable-diffusion-v1-5″

export DATA_DIR=”path-to-dir-containing-images”

accelerate launch textual_inversion.py \

–pretrained_model_name_or_path=$MODEL_NAME \

–train_data_dir=$DATA_DIR \

–learnable_property=”object” \

–placeholder_token=”” –initializer_token=”toy” \

–resolution=512 \

–train_batch_size=1 \

–gradient_accumulation_steps=4 \

–max_train_steps=3000 \

–learning_rate=5.0e-04 –scale_lr \

–lr_scheduler=”constant” \

–lr_warmup_steps=0 \

–output_dir=”textual_inversion_cat”

在一个V100 GPU上运行一次完整的训练大概需要一个小时。

![](img/e88128c84081d7d56915b4849f9ed7e3.png)

拿破仑照片

推导：一旦用上述指令训练了一个模型，那么使用StableDiffusionPipeline推导就会比较简单。确保在你的指令中加入placeholder_token。

from diffusers import StableDiffusionPipeline

model_id = “path-to-your-trained-model”

pipe = StableDiffusionPipeline.from_pretrained(model_id, torch_dtype=torch.float16).to(“cuda”)

prompt = “A backpack”

image = pipe(prompt, num_inference_steps=50, guidance_scale=7.5).images[0]

image.save(“cat-backpack.png”)

# **LoRA**

| LoRA的全称是Low-Rank Adaptation，即大型语言模型的低阶自适应。 | ![](img/d01d6c19089e1b6db6298ad71e5b0395.png) |
| --- | --- |

## 工作原理

LoRA通过学习rank-decompostion matrices来减少可训练参数的数量，同时冻结原始权重。这大大降低了适用于特定任务的大型语言模型的存储需求，并在部署期间实现了高效的任务切换，而不会带来推导延迟的问题。LoRA还优于其他几种自适应方法，比如适配器、前缀调整和微调。

## 快速上手

1）安装loralib非常简单：

```
pip install loralib
# Alternatively
# pip install git+https://github.com/microsoft/LoRA
```

2）你可以选择通过将某些层替换为在loralib中实现的对应层，进行调整，目前只支持nn.Lineral、nn.Embedding和nn.Conv2d。对于单个nn.Lineral表示多个层的情况，我们也支持MergedLinear，例如在attention qkv映射的一些实现中。

```
===== Before =====
# layer = nn.Linear(in_features, out_features)
===== After ======
import loralib as lora
# Add a pair of low-rank adaptation matrices with rank r=16
layer = lora.Linear(in_features, out_features, r=16)
```

3）训练开始之前，只标记LoRA参数为可训练

```
import loralib as lora
model = BigModel()
# This sets requires_grad to False for all parameters without the string “lora_” in their names
lora.mark_only_lora_as_trainable(model)
# Training loop
for batch in dataloader:
…
```

4）当存储一个checkpoint的时候，生成一个只包含LoRA参数的state_dict。

```
===== Before =====
# torch.save(model.state_dict(), checkpoint_path)
# ===== After =====
torch.save(lora.lora_state_dict(model), checkpoint_path)
```

5）当使用load_state_dict加载一个checkpoint的时候，确保设置strict=False。

```
Load the pretrained checkpoint first
model.load_state_dict(torch.load(‘ckpt_pretrained.pt’), strict=False)
# Then load the LoRA checkpoint
model.load_state_dict(torch.load(‘ckpt_lora.pt’), strict=False)
```

随后，训练就可以正常进行了。

# **Hypernetwork**

Hypernetwork最初是Novel AI研发的 一个微调技术，它是一个连接到Stable Diffusion模型上的小型神经网络，用于修改其样式。它是Stable Diffusion模型中最关键的部分：噪声预测器（noise predictor）UNet的cross-attention模块。

![](img/310676ca3139630fd29f5de7fa4fc211.png)

Hypernetwork通常是一个简单的神经网络：一个具有丢弃和激活的完全连接的线性网络，就像你在神经网络入门课程中学到的一样。他们通过插入两个网络来转换密钥和查询向量，从而劫持了cross-attention模块。对比下面的原始模型体系结构和被劫持的模型体系结构：

![](img/950717dcd7b1cf3d8d8cea4e3d1fae68.png)

训练期间，Stable Diffusion模型被锁定，但附在上面的Hypernetwork是可以改变的。因为Hypernetwork比较小，训练很快速而且需要的资源有限，而且可以在最普通的电脑上进行训练。

快速训练和比较小的文件是Hypernetwork最主要的吸引力。

需要注意的是，这与普通机器学习中的hypernetwork并不相同，这是一个为其他网络生成权重的网络，与2016年的不同。

Hypernetwork的文件大小通常在200MB以下，而且无法单独工作，它需要与一个checkpoint模型一起生成图片。

Hypernetwork与LoRA很像，它们都很小且仅修改cross-attention模块，区别在于后者是通过改变权重修改，而Hypernetwork则是通过插入额外的网络改动cross-attention模块。LoRA是一个数据存储方法，它不定义训练过程，Hypernetwork可以定义训练。

## 如何使用

这里介绍在AUTOMATIC1111 Stable Diffusion GUI使用Hypernetwork的方法，你可以在Windows、Mac或者Google Colab使用该GUI。

1）安装一个Hypernetwork模型：要在AUTOMATIC1111 webui安装Hypernetwork模型，将模型文件放在下列文件夹中：stable-diffusion-webui/models/hypernetworks

2）使用一个Hypernetwork模型：要使用Hypernetwork，将下列短语放到指令中：<hypernet:filename:multiplier>

> filename是Hypernetwork的文件名，去掉了扩展名（如.pt，.bin等）。multiplier是应用到这个Hypernetwork模型的权重，默认值是1，设置为0则关闭该模型。

如何确定文件名的正确呢？你需要在“Generate”按钮下点击模型按钮，而不是写下这个短语。

![](img/8e3c62464650cd096eded16f7541c8c3.png)

点击Hypernetwork标签，可以看到被安装的Hypernetwork列表，点击你想要使用的那一个，hypernet短语就会被插入指令中。

![](img/d38e3de2be1bd4efb0c4f435e3ee7e5e.png)

需要注意的是，hypernet短语并不被当做指令的一部分，它只是指引使用哪些hypernetwork，在Hypernetwork被应用之后，这个短语就会被去除，所以你不能对它们使用 [keyword1:keyword2:0.5] 这样的指令语法。

3）测试并用该模型生成美术作品：为了有更大的成功率获得预期的美术风格，刚开始可以将它与训练过的模型一起使用。但不要止步于此，一些Hypernetwork需要特别的指令，或者只适用于某些主题，所以一定要查看模型页面上的指令示例，看什么最有效。

这里给出一个建议是：如果你发现图片有些过于饱和，或许是因为需要调整multiplier，这很容易搞定。有时候Stable Diffusion会干扰颜色饱和度以达到目标，但降低multiplier可以回到平衡状态。

| ![](img/f52686d05dff22f3ce8edc5aa7cc72c0.png) | ![](img/5fcf4f1b08c71913e84cddaac0a92f8f.png) |
| --- | --- |

# 对比

对于该使用哪个方法，有一位开发者通过对四种模型的深度对比，讲述了不同模型之间的区别，比较了各自之间的优劣势。以下是Gamelook听译的完整内容：

在LoRA、Dreambooth、Textual Inversion和Hypernetworks之间，你应该使用哪一个？为了回答这个问题，我阅读了所有的论文，并且了解了人们对这些模型喜欢和不喜欢的地方，我做了一个表格和很漂亮的简图，然后回答这个问题。

![](img/3db562b1c620d88d40491851b04b83d5.png)

对于每个模型，我们需要回答两个问题：方法是什么、它们是如何共工作的？根据各自的优劣势，它们所做的权衡是什么？

这四种方法的工作方式都很相似，但我们先从Dreambooth开始，因为这或许是最为直接的一个，它的方式实际上就是更改模型本身的结构。在Dreambooth里，你有两种输入：第一个是你想要训练的概念，这里我们给出的是你想要训练的Corgi（柯基狗）的照片，现实中你可能有五个或者更多的图片要训练；另一个就是这个句子，就是带有唯一标识符的那个，这个案例中是SKS。

Dreambooth的整个想法是，你教模型将这个唯一标识符SKS与概念Corgi联系起来。进一步来说，就是将这句话转化为文本embedding，每个单词都通过一个矢量（也就是一串数字，就像是浮点数字）表示，每一个单词都有唯一的矢量，每个矢量都包含这个单词相关的一些语义学信息。

这里我们不会深入探究embedding，不过简单来说，有些矢量包含艺术相关的信息、有些是关于照片，还有些是非常随机的。

![](img/92c89bce63a41708a55192396e4d55ac.png)

我们的方法是进入文本embedding，然后对样本图片应用大量的噪点，然后还应用少量的噪点。比如，我们或许会对进入的那个应用10步的噪点，另一个则应用9步，我们要让模型使用10步噪点的那个，然后输出9步的那一个。我们让Stable Diffusion对图片去噪，让它回到原本的样子。

一开始，由于模型不知道你发送的图片，它们或许会做的很糟糕，输出的可能是差别很大的结果。然后你要做的就是将结果和应该创作出的9步噪点进行对比，然后进行梯度更新定义什么是丢失，如果丢失过高，则惩罚模型，如果丢失很低则奖励模型，这样循环数次之后，模型就会记住如何处理这样的指令。

这样，你就得到了一个模型：你输入噪点图片（SKS），然后将它变成一个干净的图片Corgi，这就是你最终将得到的结果。

这就是Dreambooth的工作方式，解释起来很费时间，但希望我说的足够简单，这样我们对比其他技巧的时候理解起来会更容易。在Dreambooth里，你实际做的是创作了一个全新的模型，你对初始模型的内部结构进行改动，直到它理解这个概念为止，因此，这或许是在Stable Diffusion里训练特定概念最有效的训练方法。

![](img/7dd48e1f028ff5700ed969948293454e.png)

不过，它在存储方面的效率不高，因为每次使用Dreambooth就会产生一个全新模型，比如训练Corgi可能会产生2GB的数据量，然后训练猫模型，就又需要2GB空间，带着这么大的数据分享很不方便。同一个模型训练多个概念也是可以的，但有时候会上这个模型令人困惑，除此之外，Dreambooth就是最有效的方法。

接下来说Textual Inversion，第一眼看上去，设定几乎是一样的，我们仍然有SKS，依然有Corgi，并且还是尝试最后产出Corgi这个结果，依然需要做去噪工作然后对比，比较不同的是，Textual Inversion并不在结果出错的时候进行梯度更新惩罚模型，而是会更新一个矢量，最终得到想要的结果。

有趣的是，Dreambooth是个非常复杂的模型，它可以理解成千上万个概念，所以它很聪明。对于Textual Inversion，我们只是创作了一个非常特殊而完美的矢量，用它来告诉模型Corgi的概念，结果我们发现Textual Inversion得出的结果非常好。

Textual Inversion的优势在于，你不用重新创作一个新模型，它只是一个12KB的微型embedding，你可以将它上传到网上，所有人都可以下载并运用到自己的模型，得到同样的柯基。

然后是LoRA，也就是Low-rank Adaption，不过为了理解它的工作方式，我们需要了解扩散模型本身的内部以及它是如何工作的。

![](img/a799a16f410b1b9c5089805a2fb750b1.png)

当今神经网络的工作方式，是设定一系列连续的层，这个案例中有三个，但实际运用中通常有数百个之多，你得到输入，通常是一个庞大的数字矩阵，把它传递给第一层，第一层会对这个数字矩阵做一些计算然后得出另一个矩阵。新的矩阵会传递给下一层，然后得出另一个变形的矩阵，到了最后一层，你得到了输出结果。

它的想法是，随着这些权重在这些模型之间通过，模型会学到越来越多和输入结构有关的东西，最后它完全理解了输入是什么，然后给出你想要的结果，这就是神经网络的基本运作方式。

那么，在这个过程中，LoRA在哪一步呢？它实际上试图解决的是Dreambooth问题，在后者当中，你试图训练模型理解一个概念，然后创造新模型不断迭代，这会导致巨大的存储占用，LoRA的出现就是想要解决这个问题，它希望在不做完整模型拷贝的情况下，让模型理解这个概念。

由于Stable Diffusion并不是超级大的模型，所以Dreambooth还是可以接受的，你能够使用它。不过，LoRA最初用于大型语言模型，后者通常有数十亿参数，所以每次训练的时候做一个副本是不现实的。

![](img/96ded148603ba53a4222e43a4628bb99.png)

LoRA所做的是给这个模型插入新的层，最初模型看起来像图片里的下方，现在它有了两个额外的层，第一层处理的结果不会直接传递给第二层，而是到了LoRA层，它得出第二个结果，然后传递给第二层。这些都是很小的层，基本上来说，当LoRA训练开始之后，它们完全不会影响模型。

随着训练的进行，你更新这些中间层，慢慢地，这些中间层越来越有观点，只要训练足够多，它通常也会得到Dreambooth那样的结果。所以它的方式与Dreambooth有些相似，但只是更新了已经存在的权重，插入新的权重并更新这些新权重，直到实现同样的效果。

LoRA的训练过程和Dreambooth很像，但与之对比，LoRA训练快很多，而且使用的内存很低，LoRA模型很小，你可以加到不同的模型中，通常它的大小在150MB左右。

![](img/1f54717af6d0ef9d1c1ad5bb6d0405ef.png)

最后是Hypernetwork，基本上来说，它与LoRA一样，目前为止还没有一分和它相关的官方论文，不过，我通过阅读AUTOMATIC 111代码库发现，这就是它的运行方式。

不过，这个方法并没有直接更新中间层并进行优化，而是有了一个输出中间层的hypernetwork，就像扩散模型会输出数字矩阵并翻译成图片那样，Hypernetwork会输出多个数字矩阵，后者随后被用在扩散模型中当做中间层。

与LoRA的想法一模一样：插入中间层，不断更新并且变得更好，最终得到你想要的结果。只不过，你不是直接更新整个层、对比丢失，而是更新一个学会如何创造这些层的网络，然后这个网络不断更新，直到得到理想的结果。

虽然Hypernetwork有很多相关的研究，但我的直觉（不一定对）是，它只是LoRA的一个最糟糕版本，因为LoRA当中有很多聪明数学计算让它易于优化和训练，但是，通过一个网络进行间接训练，我怀疑它的效率会低很多，结果也不会有那么好。不过，它有着和LoRA同样的优势，那就是只占150MB左右的空间。

![](img/e0c6f9fd34f9c477a05e9733aab6a9ab.png)

做了定性分析之后，我们再来看定量分析，这张表包含了我对每个训练技巧研究的重要事实，比如训练占用多少RAM、用时多久等等。令人惊讶的是，它们训练占用的RAM几乎相同，但使用的训练时间差别很大，输出结果的大小也各不相同，Textual Inversion是占存储空间最小的。

这个表单包含了大量从civitai下载的数据，这些都是有关人们对不同模型喜好的数据，通过统计，我发现最受欢迎的模型是Dreambooth，它的下载量、评分和喜欢人数都是最高的。这并不一定代表着Dreambooth就是最好的模型，但意味着很多人在使用，意味着就有更多的相关资源。

如果我要教一个模型理解一个概念，我也会使用Dreambooth，因为我知道它会有更好的新手教学、浪费更少的时间逛论坛，而且似乎会有更好的效果，毕竟很多人都在用。

![](img/30fb6c05216d65737701832a70538c48.png)

评分方面，Dreambooth和Textual Inversion得分相同，从实际与人沟通来看，似乎Dreambooth略胜一筹，但从civitai数据来看，人们对这两个模型都很喜欢。其余两个模型的评分低很多，这对于Hypernetwork显然是个坏消息，再加上比较低的下载量，或许Hypernetwork是应该避开的那个，除非你别无选择。

统计结果对LoRA也不利，不过它相对比较新，而且统计当中也只有11个LoRA模型，因此这些数据或许并不能完全代表LoRA的潜力。

# 结尾

**总的来**说，或许直接使用Dreambooth是最好的选择，用的人很多，而且对它的评价也很高。只是需要注意两点，其一是这个模型很大，如果对存储空间占用介意，那么可以选择Textual Inversion，当然，LoRA也是不错的，毕竟它的训练时间最短。

> 关注**👸号**，加入**AI****实战交流群**，掌握全面专业的**行业资讯**、**工具分享**、**变现指南**和**实战教程**，开启你的AI智慧之旅👇👇

联系🛰️**JUGO737Y****（备注来意）**

![](img/f47dad6e375bd6ff3215e8e3bbd94cb3.png)

👸号：**橙创****AI**

![](img/747244dc17c8ecd93db9bfaf79fa48de.png)

**群聊二维码**：

![](img/68190e4a4d00fe186a1e2a046e3f0a9f.png)