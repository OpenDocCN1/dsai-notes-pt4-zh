# 🏞️图像 Token 化：视觉数据转换的关键技术

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Woj3dVn95o7Toyxm9Ytcx89XnVc](https://v11enp9ok1h.feishu.cn/docx/Woj3dVn95o7Toyxm9Ytcx89XnVc)

# 🏞️  图像 Token 化：视觉数据转换的关键技术

+   随着 **GPT-4o**的发布，多模态模型真正惠及普罗大众。多模态**模型在单一模型中同时处理**不同类型的数据，做到不同模态之间的端到端。而多模型模型中的第一步（也是非常重要的一步）往往就是利用不同类型的 Tokenizer 把数据各自表达为离散的 token 序列。

+   **很多场景下使**用离散表示更方**便，比如文本中的 to**kens 都是离散的，Transformer 模型也是针对离散数据发明的。

+   **本文介绍图**像或视频数据中常用的 token 化模型。这些模型都符合以下结构，主要包含 3 个部分：

![](img/c6178511259b284916d96358263639fd.png)

1.  **Encoder****：把原**始**图片压缩成维度更低的序**列**向量。**常用的模型包括：CNN、Transformer、MLP等，逻辑上只要模型可以降维、压缩都 ok。

1.  Vector Quant**izer (VQ**)：把上面得到的向量**对应到Codeb**ook 中的向量，获得在 Codebook中的索引值，这样就可以用离散的序列索引值表示原始的图片。

1.  **Decoder****：把序**列索引值**映射回像素空间**，还原出RGB **图像。Deco**der 一般都使用和 Encoder 对称的模型结构。

# 下面具体介绍各种模型

## VQ-VAE

+   [**Understanding VQ-VAE (DAL**](https://mlberkeley.substack.com/p/vq-vae)L-E Explained Pt. 1)

+   **Code: **[**h**](https://github.com/apple/ml-4m/blob/main/fourm/vq/vqvae.py)ttps://github.com/apple/ml-4m/blob/main/fourm/vq/vqvae.py

**VAE**使用**连续向量表示隐状态**，而 **VQ-VAE**** (VQ: vecto**r quantization) 使用离散方式表示隐状态。

> 思路其实也挺简单的，**encoder**的输出**还是连续向量，但**会维护一个大小为  的 cod**eb**ook  （连续向**量集合），然后把** K 这个连续向量对应到codebook中离它距离最近的向量  ，对应的序号就变成了离散的隐状态了。 传给 decoder 的输入就是向量  。

![](img/3d4ee1effd0178cd8528e9d8ecd36541.png)

+   除了模型参数外，codebook中的向量集合也是通过训练获得的。

![](img/99b6cd9e0588cd6ad2000198aec63094.png)

> 如果一个输入的图片只用**codebook****中的一个向量表示的话，表****达****能力一般****是不够的（decoder 只能****看到  个向量****）。但可**以让一个输入输出一**个向量序列，表达能力就够了。**比如把一张图片按横竖线划分为 32 x 32 个网格，然后每个网格对应codebook中的一个向量，这个表达能力就足够了 。

![](img/e5e9e342751f293340f9ea5ecc64e087.png)

**训练 loss：**

![](img/ab0feaebb3f81b87bba3566d003ef83f.png)

其中 **sg[⋅]**表示 **stop gradient** ，即不进行求导。

> 上面第二项是期望调整**codebook**以尽量与**encoder**输出接近，第三项（codebo**ok comm**itment** loss）是期**望调整encoder输出以尽量与codebook接近。如果输出一个序列的向量，最后两项会对序列所有向量的结果进行平均。

### 先验分布

> **训练的时候是假设先验分布 **** **** 是均匀分布而忽略的**。但实际**推断使用时，是可以基****于**应用数据获得先验分布   的。比如有一堆应用数据   ，可以通过VQ-VAE计算获得它们对应的离散表示   ，其中每个   是一个离散序列，类似文本中的一句话。有了这些离散序列，就可以类似语言模型一样，训练一个语言模型，然后用它来预测一个离散序列 的概率值。

![](img/efe49c2121947e36790caa671f31ecc5.png)

训练得到先验分布后**，就可以先从先验**分布**中抽取得到   **，然后再通过 VQ-VAE 的 decoder 生成图片。

## discrete VAE (dVAE)

+   [**How is it so good ? (DAL**](https://mlberkeley.substack.com/p/dalle2)L-E Explained Pt. 2)

+   **Code: **[**DALLE-pytorch/dalle_pytorch/dalle_pytor**](https://github.com/lucidrains/DALLE-pytorch/blob/main/dalle_pytorch/dalle_pytorch.py#L101)ch.py at main · lucidrains/DALLE-pytorch

> **VQ-VAE**** 每次只对应到 ****Codebook ****中的一个元素**，而 **dVAE** 则每次计算与 Codebo**ok 各元素的对****应分布  ，****然后从此分布中****抽取一个** Codebook 元素作为 Decoder 的输入。

![](img/dc5134be651a6fff049788e92248da72.png)

**下面具体介绍每个部分：**

#### Encoder

> 常见的 **Encoder**就是 **CNN**，它把维度为   的原始图片，转换成维度为   的输出，其中的    为** Codeboo**k 中向量数量。其中   为高度维度上 patch 化后的 patch 数量。比如原始图片高度  ，每个 patch 高度为 1616，那  。   类似，是宽度维度上 patch 化后的 patch 数量。

当然，现在较大模型 **Encoder** 经常使用 CNN + T**r****ans**former 的结构，比如 ViT 。

#### Gumbel-Softmax 离散化

> **Encoder** 输出结果的维度为   ，也就是每个 **patch** 对应一个长度为  的向量。  中的每个元素对应着 **Codebook** 中的各个向量，例如第  个元素的值可以理解成是选择** Codeboo**k 中第  个向**量  的 log**it，做下 softmax 变换后就可以认为是选择 Codebook 中第    个向量   的概率值。

> 那怎么从这个包含   个类别的多项式分布中抽取出类别（Codebook 中哪个向量）呢。在推断时当然好做，但是在训练时要保证可微性就不能直接抽样。这时候就用到了一种叫 Gumbel-Softmax Trick 的方法。这里不细说 Gumbel-Softmax Trick，只要知道用了它之后我们在训练时也可以做到从多项式分布中抽样（或近似抽样）且不破坏训练过程的可微性。

**抽样过程包括两种模式：**

1.  **hard 模式**：这种模式得到的分布是取值为 0 或 1，抽中的那个类值为 1，其他类为 0。比如选中第 2 个类，那通过 Gumbel Softmax 处理后得到的向量是  。

1.  **soft 模式****：**这种模式类似带 temperature 的 softmax 操作，它让之前概率高的类概率值更高，之前概率低的类概率值更低，可以认为是 hard 模式的一种 soft 近似。比如之前第 2 个类的概率值就很高，那通过 Gumbel Softmax 处理后得到的向量可能是

![](img/2d0fd10db853f8cc5eaf55095e14fe79.png)

> 所以维度为 的 **Encoder **输出，通过**Gumbel-Softmax** 离散化后，其维度仍是  **，只是在最后**一个维度上的含义从 logit**s 变成了概率值**。这个结果再与 Codebook矩阵相乘就得到了最终的抽样向量，维度为   ：

![](img/a09387fc44f6f494a06d6b68054c609b.png)

**代码示例：**

```
logits = self.encoder(img)
one_hot = F.gumbel_softmax(logits, tau = temp, dim = 1, hard = self.straight_through)
sampled = einsum(&#x27;b k h w, k d -&gt; b d h w&#x27;, one_hot, self.codebook.weight)
out = self.decoder(sampled)
```

#### Decoder

**Decoder** 通常就是转置 **CNN** （transposed CNN），它把输入维度为  的张量恢复成与原始图片同样大小，即  大小的张量。

**转置 CNN 原理如下图**，具体可见：[c](https://github.com/vdumoulin/conv_arithmetic/blob/master/README.md)onv_arithmetic/README.md at master · vdumoulin/conv_arithmetic 。

![](img/7c0b56bd3ac0f3b4df35d78f1534cbe5.png)

当然，**Decoder** 也可以使用 **Transformer** 架构。

#### 训练 Loss

**dVAE** 的训练 **loss** 和 **VQ-VAE**类似，只是使用了KL距离来让分布尽量分散：

![](img/6ee369acc611888ddfc056747ad5c2a7.png)

**其中 **** ****利****用 Gum**bel-Softmax 从 中抽样得到，  是个等概率的多项式分布。

+   上面第一项**loss** 为 L1，L2 或者 Smooth L1（靠近 0 为 L2，否则为 L1）。

+   第二项**loss** 是 **Encoder** 输出的 logits 和 uniform 分布的 KL 距离，示例代码如下。

```
logits = self.encoder(img)
logits = rearrange(logits, &#x27;b k h w -&gt; b (h w) k&#x27;)
log_qy = F.log_softmax(logits, dim = -1)
log_uniform = torch.log(torch.tensor([1. / num_tokens], device = device))
kl_div = F.kl_div(log_uniform, log_qy, None, None, &#x27;batchmean&#x27;, log_target = True)
```

## VQGAN

+   [T](https://compvis.github.io/taming-transformers/)aming Transformers for High-Resolution Image Synthesis

+   **Code**: [https://github.com/CompVis/taming-transfor](https://github.com/CompVis/taming-transformers/tree/master?tab=readme-ov-file#training-on-custom-data)mers/tree/master#training-on-custom-data

+   **VQGAN:** [https://github.com/CompVis/taming-transfo](https://github.com/CompVis/taming-transformers/blob/master/taming/models/vqgan.py)rmers/blob/master/taming/models/vqgan.py

+   **Loss 实现**: [ht](https://github.com/CompVis/taming-transformers/blob/master/taming/modules/losses/vqperceptual.py)tps://github.com/CompVis/taming-transformers/blob/master/taming/modules/losses/vqperceptual.py

![](img/cea470c946cc32e015e333de136f064a.png)

**VQ-VAE** 中的损失函数是：

![](img/fd9a7a3da73f4d738614ab92f5e4cf91.png)

上面第一项  为重构 **Loss**。

而 **VQGAN** 则把上式中的   **替换为感**知（Perceptual）Loss，同时加入了生成 patch 的判别器 loss：a

![](img/880dcc99b4e79568c1ff9026babbd213.png)

> 📌看官[方实现，](https://github.com/CompVis/taming-transformers/blob/master/taming/modules/losses/vqperceptual.py)重构 Loss 其实同时包含了 L1 Loss 和 Perceptual Loss 😅。效果怎么好就怎么来吧。所以 VQGAN 的训练 Loss 为：

![](img/261aecd203472547c432e7e94aefc457.png)

通过以下方式自适应调整，其中 表示输入的梯度值：

![](img/332e5dddc823412008fd9004141f5b53.png)

#### 感知损失（Perceptual Loss）

+   Perceptual Loss（感知损失）论文笔记 - stardsd

+   VQGAN 中实现，参考：[lpips.py](https://github.com/CompVis/taming-transformers/blob/master/taming/modules/losses/lpips.py)

+   **感知损失**（Perceptual Lo**ss）是**一种用于衡量生成图像质量**的损失函数。它与传****统的像素级损失（如均方误差，M**SE**）不同**，更关**注图像的高层次特征和感知质量**。感知损失通常基于预训练的卷积神经网络（CNN），如VGG网络，用来捕捉图像的内容和样式特征。

**感知损失的工作原理：**

1.  **预训练模型的使用**：感知损失利用了在大规模数据集上预训练的卷积神经网络（例如VGG-16）。这些网络在分类任务上已经学到了丰富的图像特征表示。

>

1.  **特征提取：**将原始图像和生成图像分别通过预训练模型，提取出不同层次的特征图。这些特征图包含了图像的高层次语义信息。

>

1.  **计算损失**：在特征空间中计算原始图像和生成图像之间的差异。感知损失通常由内容损失和样式损失组成：

>

+    ，其中  和  是权重系数，用于平衡内容损失和样式损失。

+   内容损失：主要关注图像的高层语义信息和全局结构，主要作用于较高层。在特定层次上比较原始图像和生成图像的特征图，衡量它们的相似度。

+   样式损失：关注图像的纹理、颜色等风格特征，通常作用于从低到高的多个特征层。通过计算特征图的 Gram 矩阵，比较原始图像和生成图像的样式。

**其中**** ****、是第 **** **层的权重，用来平衡不同层次的样式损失。

**具体的计算过程：**

+   假设我们有一个经过**预训练卷积神经网络的特征图**（activati**on m**a**p） ，其维度为  ，其中  **  是通道数（channels）。Gram 矩阵    的计算步骤如下：

1.  **展平特征图**：将特征图展平成一个   行，   列的二维矩阵

![](img/f145ad0ac14ce2e33517a71f85dcaefa.png)

其中  表示通道，  表示展平后的空间位置。

1.  **计算Gram矩阵**：Gram矩阵  是通过特征图矩阵   的自身转置相乘得到的。

![](img/3f974e88cde9b23294f9f8648e743951.png)

Gram 矩阵   的每个元素    表示特征图中第   和第   个通道之间的相关性。

> 样式损失   是通过比较原始图像和生成图像的 Gram 矩阵来计算的。假设原始图像的特征图的 Gram 矩阵为  ，生成图像的特征图的 Gram 矩阵为  ，样式损失可以定义为：

![](img/6f1b6f4db8cf431e96eb2f61cb5f4ebb.png)

其中 是第  层的权重，用来平衡不同层次的样式损失。

**感知损失在许**多计算机视觉任务中都有广泛应用，包括但不限于：

+   **图像超分辨率**：通过感知损失，生成的高分辨率图像可以更好地保持原始图像的细节和感知质量。

+   **图像风格迁移**：在图像风格迁移（style transfer）中，通常会同时使用内容损失和样式损失。内容损失确保生成图像与原始内容图像在内容上相似，而样式损失则确保生成图像与目标样式图像在样式上相似。通过在不同层次上衡量内容和样式的相似度，可以生成具有特定艺术风格的图像。

+   **图像生成和重建**：在生成对抗网络（GANs）中使用感知损失，可以提升生成图像的真实感和视觉质量。

## TiTok 1D Tokenizer

+   [A](https://yucornetto.github.io/projects/titok.html)n Image is Worth 32 Tokens for Reconstruction and Generation

+   Code: [**1d-tokenizer**](https://github.com/bytedance/1d-tokenizer)

前面介绍的 **VQ** 类**Tokenizer**，一张图片表达后的**token** 长度，是跟图片被划分成的 **patch** 数量一致的

+   比如 256×256 的图片，如果 patch 大小为 16×16，那 patch 数量也是 16×16。如果是 512×512 的图片呢，那 patch 数量就变成了 32×32。

+   所以分辨率高的图片，要表达它所需的 token 数量就越多。另一个问题是，图片中的临近 patch 可能内容很相近，使用单独的 token 分别表达它们会存在冗余，有点浪费。

> 为了解决上面的问题，字节给出了一种思路：使用固定长度的 tokens 来表达任意分辨率的图片，而且其中的每**个**** **token 都能看到**图片的** global **信息，而不**是之前的单个 patch 中的 local 信息。模型的名称叫：Transformer-based 1-Dimensional Tokenizer (TiTok)，这名字取得。。

**TiTok**依旧是**VQ** 类 **Tokenizer**，它依旧使用 **Encoder** →**VQ** → **Decoder** 的流程。以下是整体结构：

![](img/ae8d892c41468746fa087ec02def45d0.png)

**接下来分开细说。**

### Encoder

> Enco**der 的主体架构是 BERT**：**它的输入前一部分是图片 ****p****atc****hes。和 ViT 一样，**这些 patc**hes **经**过 CN**N** 获得了输入的 embed**ding** 向量。BER**T 输**入的第二部分是固定**长度为    的 Latent Tokens。这里的   就是表示一张图片的固定 token 数量，如 3232、6464 等。这些 Latent Tokens 对应的 embedding 向量是随机初始化的，然后在训练过程中学习得到。Latent Tokens 和图片patches 的 embedding向量一起作为 BERT 的输入。

+   对于 **BERT** 的输出，**只使用**** Latent Tokens ****对应的那些输出向量**。这   个输出向量会作为 VQ 的输入。

### Decoder

**Decoder**** 的主体架构依旧是****BERT**。此时 BERT 的输入也包括两部分。

> **第****一部分是****从 VQ 中**得**到的    个离散t****okens，它们对应**的 embedding 向量就是 Codeb**ook 中对应的向量**，只是会利用线性变**换转换为 D**ecoder 需要的输入长度。第二部分是填充的重复mask token。mask token 只有一个，它对应的 embedding 通过学习得到。这个 mask token 会被重复   次，即与 patch 数量相同，追加到第一部分的     个 tokens 后面。

+   **BERT** 的输出只使用 mask tokens 对应的那些输出，这   个输出会被 reshape 为  (d,Hp,Wp)，然后通过一个上采样 CNN 恢复成原始图片。代码中是通过插值获得的上采样，而不是使用转置 CNN。

+   训练分成 2 个步骤，称之为 Warm-up 和 Decoder fine-tuning。实验表明这种训练方法可以改善训练过程的稳定性和最终重构的图片质量。

+    **Warm-up**：Codebook 直接从训练好 MaskGIT-VQGAN 模型中拿过来用，这一步训练模型中的 Encoder，Quantizer，Decoder。

+   Decoder fine-tuning：固定 Encoder 和 Quantizer，只精调 Decoder。具体方法可参考 VQGAN 的训练代码。通过这一步可以改进图片重构质量。

**重复下 TiTok 的好处：**

+   转换后的 token 长度与图片切分后的 patch 数量无关。

+   图片转换后的 token 长度与图片分辨率无关，可以是设定好的固定值。

+   图片中的临近 patch 可能内容很相近，使用单独的 token 分别表达它们存在冗余，有点浪费。

## OmniTokenizer

+   [OmniTokenizer](https://www.wangjunke.info/OmniTokenizer/)

+   Encoder Code: [ht](https://github.com/FoundationVision/OmniTokenizer/blob/main/OmniTokenizer/omnitokenizer.py#L778)tps://github.com/FoundationVision/OmniTokenizer/blob/main/OmniTokenizer/omnitokenizer.py#L778

> **OmniTokenizer** 是近期复旦的工作，**它能同时 ****token****化图片和视频**。OmniTokenizer 依旧是 VQVAE 架构，后者更准确说是 VQGAN 架构。它的 VQ 部分参考 VQGAN 就行。

**OmniTokenizer**的 **Encode**r和 **Decoder** 都是使用了魔改过的 **Transformer** 架构。**以下是 Encoder 结构**。

![](img/38c5dc23c2163b8795a8daea4e134cb1.png)

> 前面的处理都是类似，图片就是 **patch** 分块，然后使用 **CNN** 或者其他 **patch embed**** 方法变成序列****向**量。而视频则会划分为 3 维（带时间维度）的patches，但其中的首帧会拿出来单独处理。首帧 patches 和 3 维 patches 会利用各自的 patch embed 模块变成相同长度的序列。把时间维度拉平就能拼接到一块了。图片看作是只有一帧的视频。

```
first_frame, rest_frames = video[:, :, :1], video[:, :, 1:]

# derive patches
first_frame_tokens = self.to_patch_emb_first_frame(first_frame)

if rest_frames.shape[2] != 0:
    rest_frames_tokens = self.to_patch_emb(rest_frames)
    # simple cat
    tokens = torch.cat((first_frame_tokens, rest_frames_tokens), dim=1)
else:
    tokens = first_frame_tokens
```

> **Transformer**** 里面包含**两种模块层，一种是让各个 patch 对应序列在空间维度上进行 self-attention，称之为 Spatial Transformer Layers。OmniTokenizer 的 Spatial Layer 使用了 Swin 中的 Window Attention，只考虑空间维度上附近的一些 patches 对应序列，如👉图。

![](img/bf1f1d4e6cf8a7e5d46cb14c92bde409.png)

> **Spatial Layers** 之后，就是**在时间维度上**进行的 ca**usal self**-attention，称之为 Temporal Transformer Layers。Causal 的意思是 attention时只考虑当前时间之前的时间。

**以下是示例代码**

```
tokens = rearrange(tokens, &#x27;b t h w d -&gt; (b t) (h w) d&#x27;)
# encode - spatial
tokens = self.enc_spatial_transformer(tokens, video_shape=video_shape, q_strides=self.q_strides, is_spatial=True)
hw = tokens.shape[1
new_h, new_w = int(math.sqrt(hw)), int(math.sqrt(hw))
tokens = rearrange(tokens, &#x27;(b t) (h w) d -&gt; b t h w d&#x27;, b=b, h=new_h, w=new_w)
# encode - temporal
video_shape2 = tuple(tokens.shape[:-1])
# 完全不在空间维度上做 attention，空间维度转到了 Batch 维度
tokens = rearrange(tokens, &#x27;b t h w d -&gt; (b h w) t d&#x27;)
tokens = self.enc_temporal_transformer(tokens, video_shape=video_shape2, is_spatial=False)
```

### Decoder

+   Decoder 和 Encoder 结构完全对称，也就是先进行 Temporal Layers，然后进行 Spatial Layers。

### 训练方法

+   OmniTokenizer 的另一个创新是它的训练使用了 2 阶段的渐进式训练。

+   第一阶段只使用相同分辨率的图片进行训练，主要让模型理解空间结构。

+   第二阶段则是不同分辨率的图片和视频同时加入进行训练，模型主要是去理解时间维度的结构。