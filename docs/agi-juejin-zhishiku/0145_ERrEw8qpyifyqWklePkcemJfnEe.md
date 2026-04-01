# 视频文本预训练简述

> 来源：[https://agijuejin.feishu.cn/docx/BQqadIZdroUxxUxNDkDcEnDZnxg](https://agijuejin.feishu.cn/docx/BQqadIZdroUxxUxNDkDcEnDZnxg)

> 哈工大SCIR 钟蔚弘｜[阅读原文](https://mp.weixin.qq.com/s/JQ0xAt758NQUdWx_tJo6vg)

> 转载请联系原作者取得授权

# 1. 简介

随着预训练模型的发展，研究者也开始尝试将预训练模型的架构和方法应用于多模态任务当中。在图片-文本多模态任务当中，预训练模型的应用已经取得了出色的表现。相比于图片，视频内容中包含的信息更加丰富而冗余，多帧之间可能包含高度相似的画面。与图片不同，视频内容中自然地包含了时序信息，随着视频时间长度的增长，其包含的时序信息也愈加丰富。同时，由于视频数据的体积相较于图片而言也更加庞大，数据集、模型的构建都为研究者提出了更大的挑战。因此，如何更优雅，高质量地建立视频-文本表示之间的联系、进行良好的交互，并为下游任务带来提升，就成为了研究者们探究的问题。

本文简单梳理了当前视频-文本预训练的模型架构及相关数据集，同时，针对视频信息较为冗余的特点，对引入细粒度信息的工作进行了简要介绍。

# 2. 常用预训练数据集

多模态预训练的数据通常来源于大规模的模态间对齐样本对。由于时序维度的存在，视频当中包含了比图片更加丰富而冗余的信息。因此，收集大规模的视频-文本对齐数据对用于视频预训练存在较高的难度。目前，大部分研究者所使用的公开预训练数据集主要包括HowTo100M [1] 和WebVid [2] 数据集，此外，由于视频和图片特征的相似性，也有非常多工作利用图片-文本预训练数据集进行训练，本节主要对视频-文本预训练中常用的数据集进行简单的介绍。

## 2.1 HowTo100M

学习视频-文本的跨模态表示通常需要人工标注描述的的视频片段(clip)，而标注一个这样的大规模数据集非常昂贵。Miech [1] 等人发布了HowTo100M数据集，帮助模型从带有自动转写的旁白文本(automatically transcribed narrations)的视频数据中学习到跨模态的表示。HowTo100M从1.22M个带有旁白的教学(instructional)网络视频中裁切得到了136M个视频片段(clip)。视频的教学内容多由人类展示，包含了超过两万三千个不同的视觉任务。

![](img/4cb5ee9a495557dc64c6c5a804294522.png)

图1 HowTo100M数据集概览

研究者从WikiHow中检索、抽取了23,611个与物理世界能够产生一定交互的视觉任务，并在YouTube中通过构造关键词搜索相关的视频，保留包含英语字幕的视频。英文字幕通过时间轴和具体的视频Clip构成视频-文本对，例子如图2所示。

![](img/677e1b74740cc74d47d1fa4881eb69d8.png)

图2 数据集中视频-文本对的例子

与之前的视频-文本配对数据集不同，HowTo100M数据集中的描述并不是人工标注的，由于旁白的特性，其与对应的视频clip在时序上可能并未对齐，同时也可能并不是完整的句子。但该数据集的规模足够庞大，为视频-文本预训练的工作进一步提供了可能。

## 2.2 WebVid

WebVid-2M [2] 数据集包含了从网络上爬取的2.5M视频-文本数据对，与HowTo100M不同，WebVid中包含的视频数据来自于通用领域。WebVid数据集的构造方式和CC3M [3] 比较类似，研究者发现CC3M中超过10%的图片事实上都是视频的概览，通过找到原视频，研究者得以爬取2.5M的视频-文本对。

图3展示了数据集中的一些样例，WebVid中的视频描述风格多样，包含精简亦或细节性的描述。

![](img/94829bb5128736871da520f5f4971082.png)

图3 WebVid数据集的例子

表1列出了现有的部分视频文本数据集的统计信息，相比于HowTo100M数据集，WebVid的规模仅有不到其1/10。但数据集中的文本通常是人工撰写的描述，具有较好的句子结构，与视频具有更好的匹配性，同时也避免了由于ASR撰写带来的语法错误。

表1 视频-文本数据集的统计数据 [2]

![](img/1c46233d0875e83f885c6281b53c9f23.png)

## 2.3 Conceptual Captions

视频是由多帧静态图片组合而成，由于视频资源收集的难度较大，研究人员也常使用大规模图片文本数据集来扩大预训练数据的规模，增强模型的泛化性。Conceptual Captions [3] 由Google的研究人员于2018年发布，研究人员通过从数十亿的网页中提取、过滤得到了大规模的图片-描述文本对。为了使图片描述更加干净易用，其基于描述文本和图片自动生成了对应的转换描述，称之为Conceptual Captions。数据集的统计数据如表2所示。

表3 Conceptual Captions的统计数据 [3]

![](img/bdf5cd15d27fcbb9047bf30b3bd3c894.png)

# 3. 模型架构

视频-文本预训练模型涉及到对视频、文本的编码和处理，通常可以粗略地分为单流(Single-Stream)架构和多流(Multi-Stream)架构。对于Single-Stream架构的模型，不同模态的特征/表示被输入到一个单独的跨模态编码器中，捕捉他们的模态内/间交互信息。而对于Multi-Stream架构，视频、文本将被输入到各自模态独立的编码器中，捕捉模态内部的表示信息，再通过不同的方式建立跨模态的联系。本节将对不同架构的模型进行简单介绍，对于引入更细粒度信息的模型，我们将在后面的章节进行介绍。

## 3.1 Single-Stream

### 3.1.1 VideoBERT

VideoBERT [4] 是第一个利用Transformer架构探究视频-文本表示的预训练模型。从BERT [5] 中获得启发，研究人员尝试将视频内容进行量化，对于视频Clip进行编码，聚类，从中抽取得到离散的表示，称之为视觉词语(video words)，对于每一个视频，其都可以由多个视觉词语进行表示，并能够和文本一同输入到编码器中进行联合的表示学习，其架构如图4所示。

![](img/964b89c7ceb14f64f155725ee338891f.png)

图4 VideoBERT模型架构 [4]

与BERT相似，VideoBERT引入了文本、视频两个模态的掩码完型任务(mask-completion)，具体而言，就是利用上下文和跨模态信息恢复被[MASK]标签遮盖的视频/文本token。同时，为了建模跨模态之间的关联性，VideoBERT也利用[CLS]标签位置的编码判断视频和文本之间是否时序/语义对齐。

### 3.1.2 ClipBERT

前人工作通常使用在不同领域预训练的视频编码器抽取得到的密集(dense)视频特征，ClipBERT [6] 利用了视频和图片之间的相似性，通过对视频进行稀疏(sparse)采样的方式对其进行编码，并实现了端到端的预训练。

具体而言，研究人员随机采取多个视频片段(Clip)，对于每个视频片段进行稀疏采样，以视频帧为单位进行编码得到表示；对于不同Clip不同帧的表示，可以进行时空上的信息融合，得到的表示将和文本编码共同输入到Transformer架构的编码器当中，如图5所示。以视频帧为单位进行编码，使得模型能够仅利用图片-文本数据集进行预训练（将图片看作只有一帧的视频），再在下游任务上利用视频进行训练。在预训练阶段，模型仅仅采用掩码语言建模，以及利用[CLS]标签进行视觉-文本匹配，来学习跨模态的表示。

![](img/5385c297386cdb8b403f745c3fcd217c.png)

图5 ClipBERT架构 [6]

### 3.1.3 VLM

VLM [7] 提出了一个简单、下游任务无关的预训练方法，统一了视频、文本的表示，能够接受视频、文本的单模态输入，亦或视频-文本的联合输入。如图6所示，模型引入了掩码表示建模(视频帧或者文本token)来建立模态内部的表示；同时引入掩码模态建模任务(MMM)，一次性遮盖整个视频或整个文本模态，指导模型利用跨模态交互来恢复信息。

![](img/f813c412b2d9e0acce3679e6f8a041f4.png)

图6 VLM的预训练范式 [7]

## 3.2 Multi-Stream

### 3.2.1 CBT

CBT [8] 提出通过有噪对比估计(noise contrastive estimation, NCE)来学习视频-文本的表示。CBT拓展了BERT的架构来建立跨模态的表示，在预训练阶段，两个单模态的Transformer分别被用于学习视频和文本的表示，并利用一个跨模态Transform er 来建立两个模 态的联合表示。由于视频特征是连续的，对于视频的单模态编码器和跨模态编码器 ，模型通过有噪对比估计来学习其表示。具体而言，对于单模态视频编码器，其损失函数具有如下的形式：

![](img/f59eff69c42b27d5092a7a917c1c18ac.png)

![](img/1e0633b377cfa524f6e8b363918f55fa.png)

其中  是 3D 编码器的编码输出，而  是视频 Transformer 的编码输出。而对于跨模态 Transformer 的联合表示，其损失函数具有如下的形式：

![](img/fa1d93ebbc955866b0c78be68f14261c.png)

其中MI(x,y)代表将视频，文本模态进行联合编码之后得到的联合隐层表示输出。

![](img/e354a131bdd28f0443582d7294a706c8.png)

图7：CBT模型的架构 [8]

### 3.2.2 UniVL

大多数跨模态模型仅仅应用于下游的多模态理解任务，并未过多探索跨模态的生成任务。UniVL [9] 建立了一个支持生成任务和理解任务的多模态框架。其架构大致如图8所示，UniVL将单模态编码器编码后的隐向量，输入到一个跨模态的编码器-解码器架构当中。通过NCE建立跨模态之间的相似性，使得同一个视频-文本对，其不同模态编码器编码之后的结果在表示上具有较好的相关性；同时通过跨模态的掩码语言建模和掩码帧建模来建立跨模态的交互。同时，利用解码器进行文本重建，为模型引入跨模态的生成能力。编码器-解码器架构的引入使得模型能够自然地应用到下游的描述生成任务当中。

![](img/b60a4ba9c26878cd1cf5d4999fbc127b.png)

图8 UniVL的框架、预训练任务和下游任务的应用 [9]

### 3.2.3 Frozen

Fronzen [2] 提出了一个专为检索任务设计的预训练框架。将图片看作视频的“快照”，将图文数据集作为视频数据集的一部分进行预训练，利用图文数据集增大了预训练的规模，同时利用视频-文本数据集学习关注视频内容中独有的时序信息。为了高效地进行跨模态检索，作者利用单模态的编码器编码两个模态的信息，并将两个模态的信息分别投影至一个共同的表示空间，计算其相似度。其架构大致如图9所示。视频以帧为单位进行输入，而图片则相当于仅包含一帧的视频，同时，模型也引入了space-time transformer [10] 来建模视频中的时空信息。在预训练阶段，NCE同样被用作衡量训练的损失，模型交替利用视频数据和文本数据进行预训练。

![](img/18e924df394108bb63710176f402f079.png)

图9 Frozen的模型架构[2]

# 4. 预训练中的细粒度信息

相比于图片，视频中包含的信息通常较为丰富而冗余，例如，在连续的帧之间，动作、场景等内容可能高度相似。基于密集采样的方法将会极大增加计算量，而基于稀疏采样的方式又会损失过多细粒度信息。如何从视频内容中提取理解视频所需要的细粒度信息，并加以利用，增强对视频内容的理解和跨模态表示的学习，也成为研究者所探究的问题之一。

## 4.1 基于时空信息

与图片相比，视频内容中包含更加丰富的时序信息和空间信息，建模视频中的时空信息，对理解视频内容具有十分重要的作用。

### 4.1.1 HERO

较少工作显式地探究模型对视频时序信息的理解。HERO [11] 在单流跨模态Transformer的架构之上，引入了一个时序Transformer来建模视频中的时序信息，同时利用掩码帧预测(MFM)和帧顺序预测(FOM)来增强模型对视频中时序信息的理解。具体而言，MFM任务要求模型通过文本信息和视频上下文来恢复当前帧的内容（通过回归或者有噪对比估计NCE的形式）；FOM则将跨模态编码后的帧信息按比例进行打乱，要求时序Transformer架构尝试利用视频上下文和按时序排列的字幕(subtitle)信息恢复被打乱的帧的顺序，如图10所示。

![](img/e05b7c8ef9b242be2873a44e47e8e8b0.png)

图10 HERO的模型架构 [11]

### 4.1.2 Merlot

包含密集视频描述(Dense caption)的视频内容通常包括丰富的跨模态时序对齐信息，Merlot [12] 通过视频帧-描述匹配(Contrastive frame-transcript matching)来建模视频内部的时序信息。Merlot的输入是稀疏采样的视频帧，和视频帧对应的描述，模型通过最大化视频帧和对应描述的相似度，最小化和视频内其他帧视频描述相似度来建立视频和文本之间的时序对齐信息；与HERO不同，Merlot利用时序重排序(Temporal Reordering)任务，按比例打乱视频帧的顺序，并判断帧之间的相对顺序来指导模型关注视频内部的时序信息。

![](img/82cf4d82683c660f8739705da21251e0.png)

图11 Merlot模型的架构 [12]

### 4.1.3 DECEMBERT

HowTo100M数据集中的对齐文本通常来自于自动语音识别(ASR)，包含较多噪声，而人类可能还会描述已经发生或者还未发声的场景，导致文本和视频片段的时序上并未完全对齐，或语义不一致。如图12所示，DECEMBERT [13] 从视频Clip中抽重新取了较为密集的caption来缓解噪声和语义不一致的问题；同时引入视频片段上下文的文本描述，来缓解可能产生的时序不一致问题。

![](img/3187fba2220274f708af176592188b68.png)

图12 DECEMBERT的架构 [13]

### 4.1.4 Violet

许多工作将视频看作是静态图片的集合，忽略了时序上信息具有一定的连续性，基于此特点设计的视觉掩码任务可能会失效。Violet [14] 利用Discrete VAE [22] 将视频的patch特征离散化为一系列视觉token，视频编码和文本编码联合输入跨模态Transformer后，掩码视觉token建模任务要求模型从视觉掩码输出中恢复对应的离散视觉token，与掩码语言建模任务得到了统一。同时，模型引入了基于块(Block wise)的离散视觉掩码任务，基于时间、空间同时掩码多个连续位置，防止其简单地从时空连续的位置恢复被掩码的信息。此外，一般的掩码方法以同样的概率遮盖重要/不重要的位置，Violet引入了Attended Masking的方法，利用跨模态的注意力权重，尝试遮盖模型认为更重要的区域，以提升掩码任务的难度。

![](img/e2092fe48af709e67ea351385ee0904e.png)

图13 Violet模型架构 [14]

## 4.2 基于实体和动作

基于patch或者视频级特征的编码方式隐式地编码了视频中的各类信息，粗粒度的视频-文本对齐方式可能难以挖掘视频中包含的细粒度信息。下面的一些工作也尝试引入了实体和动作等更细粒度的监督信息，增强跨模态的表示学习和建模。

### 4.2.1 ActBERT

![](img/9ff983405e4b7bb329400df4dca18984.png)

图14 ActBERT的模型架构 [15]

将视频离散化，与文本共同输入Transformer进行跨模态联合建模的方式可能会丢失很多细粒度的信息，ActBERT [15] 通过在预训练数据集上训练一个视频动作编码器，获得视频的动作编码，同时引入目标检测工具来获得视频内容中的物体信息编码。通过建立文本-动作-视频区域间的纠缠编码架构，来建立视频动作和区域信息与文本信息之间的交互，进而也保留了重要的时序信息。

### 4.2.2 OA-Trans

Multi-Stream跨模态表示学习通常建立视频-文本之间的整体对齐，并未探究更细粒度的对齐，OA-Trans [16] 引入了对关键帧目标检测得到的实体区域信息和对应的标签。如图15所示，模型将关键帧中不包含实体区域的位置进行掩码，经过时空编码得到剩余位置对应的表示，同时将实体区域的类别信息输入文本编码器进行编码。简单将获得的细粒度表示进行对齐，将对下游任务没有太多帮助。在不修改模型架构的前提下，研究者尝试建立单模态细粒度表示和另一个模态整体表示之间的相似度联系，指导单模态的整体表示能够蕴含更多细粒度的信息，在应用于下游任务时，预训练所用的细粒度表示可以被去除，而单模态的整体表示已经一定程度上具备保留细粒度信息的能力。

![](img/9244fdb17a99511934a82be0100a9eb3.png)

图15 OA-Trans的模型架构 [16]

### 4.2.3 ALPRO

引入细粒度的信息通常需要借助外部的特征抽取工具，这种做法不仅影响效率，由于特征抽取工具的类别数量等限制，效果也不尽如人意。如图16所示，ALPRO [17] 基于CLIP [18] 的思想，首先基于视频-文本对训练了一个视频-文本匹配架构(和ALRP的单模态编码器具有相同的架构)，通过提示描述*[CLS] A video of a [object]*，根据视频和描述的相似度，能够识别出视频中包含的实体。在训练过程中，模型随机裁剪一段视频，利用匹配架构获得视频的实体信息作为监督信号，引入提示实体匹配(Prompt Entity Matching)任务，要求跨模态编码器能够识别出对应裁剪位置的实体信息，以此建立视频对细粒度实体信息和场景的理解。

![](img/1604084b0a26e68d1ee3fc73934db462.png)

图16 ALPRO的模型架构 [17]

### 4.2.4 BridgeFormer

相比于利用物体检测的信息，BridgeFormer [19] 利用多项选择任务(Multiple Choice Questions)来增强模型对视频中实体新信息和动作信息的理解。具体而言，研究人员从原始文本中遮盖动词或名词短语来构建“问题”，将文本编码器得到的问题表示作为跨模态Transformer的查询(Query)，将视频内容编码表示作为键(Key)和键值(Value)，即将跨模态表示问题形式化为了给定问题，从视频中进行查询，获得答案的过程，在应用于下游任务时，单模态编码器已经学习如何建立到了细粒度的表示跨模态模块可以被去除。

![](img/13a7bb444dfbd76b5f58997d1aa15ea4.png)

图17 BridgeFormer的模型架构 [19]

## 4.3 隐式细粒度特征

相比于考虑显式的细粒度特征，如时序、实体、动作等信息，也有部分工作认为除了视频、文本的整体表示，还可以考虑帧、patch级别的不同粒度表示，来隐式编码细粒度特征。

### 4.3.1 HiT

在Transformer架构中，不同的层将聚焦于不同粒度的表示信息， 例如较低层的注意力记住倾向于编码更加基础表面的表示，而更高层的注意力机制，将会捕捉更加复杂的语义信息，基于这样的想法，HiT [20] 提出了分层跨模态的对比匹配机制，来建立不同粒度的跨模态表示对齐，具体而言，模型分别从视频、文本Query编码器的第一层和最后一层获得低层次特征级别和高层次语义级别的表示。并分别与另一模态的高层次编码结果进行表示匹配，如图18所示。

![](img/6e4d9460dd5d7c649d0bca144b8d1967.png)

图18 HiT的模型架构 [20]

### 4.3.2 HunYuan_tvr

大部分的检索模型都聚焦于建立视频整体和整个对应文本之间的表示关系，HunYuan_tvr [21] 从多个层次探究了细粒度表示的关系，通过建模帧-词语，视频片段-短语，视频-句子三个不同粒度跨模态表示之间的表示匹配，提出了层次化的跨模态交互方法来学习细粒度的跨模态联系，大致如图19所示。

![](img/426a90479ae9087bb40a53b9ec3973c4.png)

图19：HunYuan_tvr的大致模型架构 [21]

# 5. 总结

本文简单梳理了当前视频-文本预训练的模型架构及相关数据集，同时，针对视频信息较为冗余的特点，对引入细粒度信息的工作进行了介绍。

经过梳理和分析我们可以发现，目前视频-文本预训练的数据集由于收集和标注的难度较大，可用的数据集数量和规模和图-文预训练相比仍然较少，同时也缺乏更加细粒度的标注。

而为了减少对计算资源的依赖，同时更好地利用图片-文本预训练数据，视频-文本预训练模型从密集采样逐渐向稀疏采样过渡，为了弥补稀疏采样带来的信息损失和粗粒度预训练数据的监督信息缺乏，不少工作也开始探索如何抽取、或者通过无监督的方式来获得有用的细粒度信息，进一步增强细粒度的视频-文本表示学习。在未来，构建更大规模、更细粒度的视频-文本预训练数据；考虑更加合理有用的细粒度信息为训练过程提供帮助；设计、利用更强大的单模态、跨模态模型架构和自监督学习任务都是值得进一步探索的方向。

# 6. 参考文献

[1] Miech A, Zhukov D, Alayrac J B, et al. Howto100m: Learning a text-video embedding by watching hundred million narrated video clips[C]//Proceedings of the IEEE/CVF International Conference on Computer Vision. 2019: 2630-2640.

[2] Bain M, Nagrani A, Varol G, et al. Frozen in time: A joint video and image encoder for end-to-end retrieval[C]//Proceedings of the IEEE/CVF International Conference on Computer Vision. 2021: 1728-1738.

[3] Sharma P, Ding N, Goodman S, et al. Conceptual captions: A cleaned, hypernymed, image alt-text dataset for automatic image captioning[C]//Proceedings of the 56th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers). 2018: 2556-2565.

[4] Sun C, Myers A, Vondrick C, et al. Videobert: A joint model for video and language representation learning[C]//Proceedings of the IEEE/CVF International Conference on Computer Vision. 2019: 7464-7473.

[5] Devlin J, Chang M W, Lee K, et al. Bert: Pre-training of deep bidirectional transformers for language understanding[J]. arXiv preprint arXiv:1810.04805, 2018.

[6] Lei J, Li L, Zhou L, et al. Less is more: Clipbert for video-and-language learning via sparse sampling[C]//Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition. 2021: 7331-7341.

[7] Xu H, Ghosh G, Huang P Y, et al. VLM: Task-agnostic video-language model pre-training for video understanding[J]. arXiv preprint arXiv:2105.09996, 2021.

[8] Sun C, Baradel F, Murphy K, et al. Learning video representations using contrastive bidirectional transformer[J]. arXiv preprint arXiv:1906.05743, 2019.

[9] Luo H, Ji L, Shi B, et al. Univl: A unified video and language pre-training model for multimodal understanding and generation[J]. arXiv preprint arXiv:2002.06353, 2020.

[10] Bertasius G, Wang H, Torresani L. Is space-time attention all you need for video understanding?[C]//ICML. 2021, 2(3): 4.

[11] Li L, Chen Y C, Cheng Y, et al. Hero: Hierarchical encoder for video+ language omni-representation pre-training[J]. arXiv preprint arXiv:2005.00200, 2020.

[12] Zellers R, Lu X, Hessel J, et al. Merlot: Multimodal neural script knowledge models[J]. Advances in Neural Information Processing Systems, 2021, 34: 23634-23651.

[13] Tang Z, Lei J, Bansal M. Decembert: Learning from noisy instructional videos via dense captions and entropy minimization[C]//Proceedings of the 2021 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies. 2021: 2415-2426.

[14] Fu T J, Li L, Gan Z, et al. VIOLET: End-to-end video-language transformers with masked visual-token modeling[J]. arXiv preprint arXiv:2111.12681, 2021.

[15] Zhu L, Yang Y. Actbert: Learning global-local video-text representations[C]//Proceedings of the IEEE/CVF conference on computer vision and pattern recognition. 2020: 8746-8755.

[16] Wang J, Ge Y, Cai G, et al. Object-aware Video-language Pre-training for Retrieval[C]//Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition. 2022: 3313-3322.

[17] Li D, Li J, Li H, et al. Align and Prompt: Video-and-Language Pre-training with Entity Prompts[C]//Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition. 2022: 4953-4963.

[18] Radford A, Kim J W, Hallacy C, et al. Learning transferable visual models from natural language supervision[C]//International Conference on Machine Learning. PMLR, 2021: 8748-8763.

[19] Ge Y, Ge Y, Liu X, et al. Bridging Video-Text Retrieval With Multiple Choice Questions[C]//Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition. 2022: 16167-16176.

[20] Liu S, Fan H, Qian S, et al. Hit: Hierarchical transformer with momentum contrast for video-text retrieval[C]//Proceedings of the IEEE/CVF International Conference on Computer Vision. 2021: 11915-11925.

[21] Min S, Kong W, Tu R C, et al. HunYuan_tvr for Text-Video Retrivial[J]. arXiv preprint arXiv:2204.03382, 2022.

[22] Van Den Oord A, Vinyals O. Neural discrete representation learning[J]. Advances in neural information processing systems, 2017, 30.

本期责任编辑：丁 效

本期编辑：赵 阳