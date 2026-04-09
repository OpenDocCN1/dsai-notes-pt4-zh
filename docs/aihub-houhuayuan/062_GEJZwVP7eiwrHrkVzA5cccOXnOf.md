# AIGC应用调研

> 来源：[https://seigy6zzam.feishu.cn/docx/TogId3KfHoYw6Cxwpnyc4Ivknad](https://seigy6zzam.feishu.cn/docx/TogId3KfHoYw6Cxwpnyc4Ivknad)

# 全景综述

2022年是AIGC爆火出圈的一年，不仅被消费者追捧，而且备受投没界关注，更是被技术和产业界竞相追逐。2022年12月，OpenAl的大型语言生成模型ChatGPT刷爆网网络，它能胜任刷高情商对话、生成代码、构思剧本和小说等多个场景，将人机对话推向新的的高度，让人不禁怀疑ChatGPT是否已经具有人类智能。全球各大科技企业都在积极拥抱AIGC，不断拆出相关的技术、平台和应用。微软、谷歌等全球计软件互联网大厂大力投入AIGC，并开始探索应用层面的突破，AIGC已经来到2年前自动驾驶的应用突破拐点，今年有望成为AIGC大年。类似ChatGPT的"大模型"作为AIGC的底层平台，将成为AIGC的核心资源。

AIGC (Al-Generated Content，人工智能生产内容)，狭义概念是利用AI自动生产内容的生产方式，但广义上AIGC已在实现人工智能从感知理解世界到生成创造世界的进击，AIGC代表AI技术发展的新趋势，过去传统人工智能偏向分析能力，而现在人工智能正在生成新内容，通过大量的训练数据和生成算法模型，自动生成文本、图片、音乐、视频、3D交互内容等各种形式的内容，换言之，AIGC正在加速成为AI领域的商业新边界;AIGC也会带来内容创作的变革，如智能数字内容孪生能力、智能数字内容编辑能力、智能数字内容创作能力。

AIGC按内容生成类别可划分为文本、代码、图像、音视频四类，根据红杉资本预测，2023年文本、代码生成有望得以成熟应用，其中文本生成可实现垂直领域或文案的精确调整，达到科研论文精度，代码生成可覆盖多语种多垂直领域;图像、音视频生成的成熟度相对较低，目前尚处于生成基础初稿的阶段，2030年有望得以成熟应用。

![](img/7d037c8a78d6ffe2e0b9b3b896bb17d7.png)

## 国内情况

![](img/8ff74f342d659dfef4b37caa30d048a4.png)

![](img/d61f537c372de60beee6e0b8ef649f94.png)

![](img/5430bd26a2b3c8b47879f299781b85f4.png)

![](img/754287453387c3ae6db07b0e5afe59be.png)

![](img/b8ed203dba97b75ae253c4ce0b39f776.png)

## 国外情况

![](img/4187d67f8dae7fca7e802434ecc53e29.png)

![](img/1ebef0182176127818a0c16e423659a2.png)

![](img/9e61dd4bf04f31efe65cad9087bbb8fc.png)

# AIGC 技术概述

## 历史和发展

+   AI的发展其实是个缓慢而波折的过程，中间经历过几起几落，除了在算法层面的不断优化，还有硬件算力的不断提升，而临界点则是算法的横向扩展能力可以跟算力基本匹配，很多之前难以突破的瓶颈才找到的突破的机会

![](img/4fd0ea8840a35758a001b382c9eced68.png)

+   最理想的情况下一个生成式的模型应该是识别多模态的，通过自然语言提供prompt的方式就可以扩的预期的结果，但是实际上想要统一所有的生成模型的范式还是有一定难度的，所以其实针对文本和图像的生成方案是分别发展的，很多宣称支持多模态的模型底层一般都会有一个调度器，执行文本和图像任务其实还是分开的。比较特别的是GPT-4，因为OpenAI自己有个Dalle-2的模型，其模型结构也是使用的transformer，所以文本任务和图像任务可以在基本结构上保持一致。但是现在应用更加广泛的反而是开源的diffusion模型

![](img/59cb7cf76d10c4e96686490ee4e5d88d.png)

![](img/d29980cb7d3cb00bf56385ae1a12da02.png)

## 模态转换关系

基本现在多种模态以文本为枢纽已经可以形成通常的转换关系了，只是由于不同的转化通路上相关的技术成熟度还存在不均衡，所以还存在一定的局限性，比如相对来说视频相关的转化能力在技术能力上是最早期的，而且也是相对来说最难的。很好宣称实现多模态的模型，其实不少也是钻了这个“空子”（当然没有一竿子打死，也有一些真的通过向量化，把其他的模态转化到 transformer 的范式下去实现统一建模，只是从结果看不同模态有其特点，transformer 的方案在文本可能效果不错，在其他模态未必是最优解）

# 在文本领域的应用

## 需求和挑战

![](img/2eef2d8d98e05a278fd136f69ffdf765.png)

![](img/064b2cd7b5ad84a5c208dc9af5cbabd6.png)

## 原理和案例

![](img/98782cdb58220e858b6b25bd418b8c41.png)

文本相关的NLP任务，现阶段基本靠一个基础结构模型+训练方法统一的范式，通过很基础的方式，比如 pre-trai[n trans](https://arxiv.org/pdf/2304.01852.pdf)former + instruct FT(指令微调)就可以达到很好的效果，更不用说chatgpt叠加上RM + RHLF的效果了，关于应用综述可以参考这篇paper，典型的应用任务包括下面这些：

![](img/7ac9bcb710a68b0b593124ce079a469c.png)

![](img/fdb992baef4a3151178ddbeaef20c253.png)

![](img/c6e02531a8c54896960e501c3725af54.png)

+   文本生成（Text Generation）：这个任务其实我觉得是这次爆发最有代表性的人物，一般将生成的文本分为三个层次：短语、句子和段落，只要你能给到具体清晰的要求描述，GPT基本可以给出基本可用的内容，当然智能程度越高，生成的效果也会越好

+   代码生成（Code Generation）：其实我觉得也可以算作文本生成的一种，只是因为代码还有运行的属性，所以单独拆出来算。通过分析代码生成的需求，GPT可以生成准确执行预期功能的代码片段。这不仅节省了从头开始编写代码的时间和精力，还降低了手动编码过程中可能发生错误的风险。此外，GPT学习和适应新编程语言和框架的能力使其能够完成更复杂的编程任务

+   数据或信息的提取、转换、增强、处理（Data or information extraction， transformation， enhancement， processing）：这块使用传统机器学习的方法我不确定有没有特别高效的手段，但是使用新方法的效果真的非常经验，我觉得甚至可以超过大部分人工总结的效果

+   **其他包括：**

+   问答（Question And Answering）：问答分为两种，一种是封闭式问题有确定性答案的，通常用于教育部门的问答测试。用户可以学习、比较和验证不同学术科目（如物理、数学和化学）和/或概念科目（如哲学和宗教）的答案。另一种是开放性问题没有确定性答案的，用户可以提出开放式和分析性问题，跟GPT形成讨论

+   文本分类（Text Classification）：将文本数据分配给预定义的类别。这项任务对于许多应用至关重要，包括情感分析、垃圾邮件检测和主题建模。比较适合很多需要自动打标的场景，之前该工作需要特征工程或者领域模型，现在门槛进一步降低

+   **推理（Inference）**：无论是归纳推理还是演绎推理都需要遵循严格的逻辑规则以保证推理的正确性和可靠性，这也是GPT在填词猜概率基础能力之外涌现出来的能力，是真正有所谓“智能”的部分，大家常用的CoT技巧或者进阶的比如ToT技巧主要就是在这个任务下的应用

前一段微软请到openAI的前特斯拉自动驾驶部门的高级主管[现OpenAI](https://www.youtube.com/watch?v=bZQun8Y4L2A)科学家的An[dr](https://karpathy.ai/stateofgpt.pdf)ej Karpathy分享的State of GPT（非常精彩）有更多细节，感兴趣可以了解，相关PPT参考

![](img/672fe32904b9a8f9305f6a5703a20667.png)

![](img/81ae131d2ba82741b7fe700ac521444a.png)

[另外想要了解“地表最](https://mp.weixin.qq.com/s/AIwinPksV_u-RQfcoCD7nw)强模型”可以参考最新泄漏的关于GPT4相关实现猜测（小道消息，非官方）

# 在图片领域的应用

## 需求和挑战

传统的图片相关领域最早主要以CV任务为主，也就是计算机视觉，更多是做图片信息感知的，因为图片中的信息量要比文本更多，所以之前想要做生成是一件很困难的事情。传统的CV任务包括：

![](img/c0fa7c496807d853d15efe3244a58c63.png)

![](img/71ae9eedb56d49fec50da2541554935f.png)

![](img/4bd0724414b427abfefa3ec0b1c95d4f.png)

![](img/acf4b17c14f31238a85fad835d6d0a2d.png)

![](img/70b31b0c1a1234aa46af5f3839748bd9.png)

![](img/95840f84b384189cbb6359be7c59178b.png)

![](img/4cf64c1b790676052c45f779c78f0af0.png)

![](img/c39173e8ceee728dbc572496d0ee829c.png)

![](img/37cc03e5f3f522b5d5a1397b6d31a25f.png)

![](img/e2d426b1ff99f651d36b6164964a525a.png)

计算机理解图像的过程大致如下，其实是对于图像信息的不断压缩，直到压缩到可以代表图像特征又不损失关键信息的程度

![](img/8aec855310e34ab1c305d6ff160b89cd.png)

可以看到传统的CV任务里面没有涉及到图片的生成，直到2014年GAN (Generative Adverserial Network)的出现，GAN模型一问世就风靡AI学术界， 在多个领域得到了广泛的应用，再之后就是是OpenAI的Dall-E问世，以及后续因为OpenAI发布最强的CLIP分类模型也进一步推动了AI绘画的发展，这里的故事太多就不展开了。

![](img/331294cd33e1be4f9c8f852697903df7.png)

![](img/832b0bb0b9ef53c22adfad88c06f28c6.png)

![](img/d67a36048a9721e3b2b47d40b8c20a59.png)

![](img/5509cfa10bf93c14001571f44197e138.png)

## 原理和案例

直到Diffusion模型的出现，具体的原理可以说很多，简单理解来说，他的原理就是从一幅白噪声图片不断去噪点生成图片（从右往左）

![](img/fb5ccdca1523fa317915b47e983d9c47.png)

从这个原理就大概可以知道，整个AI绘画的过程有点像是在拆盲盒，如果只是玩一玩，其实是挺有趣的，但是如果想要进行商业应用（当然服务好娱乐抽卡这种场景也是一种商业应用），解决商业上的问题，随机的生成是远远不够的，需要更多的控制能力，因为stable diffusion的开源，以及 stable-diffusion-webui 生态的蓬勃发展，进化出了很多高级的魔法技巧：

+   **基础模型：**因[为技](https://civitai.com/)术的发展以及前人栽树，训练基础模型门槛大大降低，繁荣的基础模型生态提供了二次元，三次元甚至是2.5次元图片应有尽有，类似C站分享的网站也给了模型分享很好的渠道，很好连接消费和生产端

+   **图生图：**生图的时候会在加噪和去噪的过程中参考提供的图片信息，提示词引导类似你跟设计师描述我要五彩斑斓的黑，而图生图类似于你跟设计师说我想要抄这张图，结合重绘幅度的设置，这样生成的图就会根据你的需求多少程度上利用参考图片中的关键信息

+   **高清放大：**通过重绘，切片重绘拼接或者常规的高清修复技巧，可以对于抽卡出来的低分辨率图片进行高清修复，生成更大尺寸的图片或者对于局部的细节绘制更加清晰，相当于固定了分辨率的诉求，通过技巧控制引导扩散模型进行二次生成

+   **Lora****：**在不改变大模型权限的前提下，为之前的的权重网络提供参数调整的能力，降低训练一个特定用户小模型的门槛，比如可以从特定人物，画风，概念，服饰和特别元素这些方面自己训练或者在模型分享的网站找到现成的，这某种程度上实现了图片数据的聚类和共享，也给图像局部和整体生成控制提供更精细和可组合的工具

+   **ControlNet****：**通过添加额外条件来控制扩散模型的神经网络结构，提供了一种增强稳定扩散的方法，提供了诸如 openpos，depth，canny，softedge和scribble等等（最新的1.1有14种模型）基于特定任务目标提取参考图的关键信息，从而以关键信息作为约束影响新图片的生成，可以说ControlNet是迈向文生图迈向工业化的重要一步

+   **图片生成视频：**虽然下面也有关于视频的AIGC能力说明，但是现阶段视频的生成不可控性太强，大部分的效果的比较一般，偏艺术风格的视频，而且长度都比较短，还处于非常早期的阶段，虽然最近有像pika lib有些惊艳的表现（后面会展开），但是当先有更多实用性的还是基于图片生成的视频，因为图片已经有很大的可控性，结合比如mov2mov，sadtalker或者帧图片补帧动画的方式，可以实现不错的效果

![](img/41e00a26f3910a6942e102f5ad3a16ad.png)

![](img/ffb7c4e25213fdfc0be1e03e0df3094f.png)

![](img/82d5212e0cb2a96717e27d18537fee04.png)

![](img/2f55d1269085a25a065ba13da6174aee.png)

![](img/a4af4c8b2cfe00b4c47db31518f4396b.png)

![](img/8cdddb3ca36f67d56a5f356ee1e75bc7.png)

# 在音频领域的应用

## 需求和挑战

![](img/49a3410dc8558fe773c35b233624783f.png)

![](img/80f729c1efcacd58a470ee40b8289a19.png)

![](img/b369f1defdc953b9d2e701af077fae15.png)

![](img/5279e8813ab181bb642f3e7cf4b0b88a.png)

![](img/bea07a9af0bd389a1d389de57acfe6c0.png)

## 原理和案例

这块领域的发展情况比较特殊，因为众所周知ASR和TTS的技术发展到现在其实已经比较成熟了，所以其实理论上大部分音频的内容都可以转化成文本，然后基于文本任务处理。不过这里有个特殊的领域，那就是音乐，基于音乐内容的消费是很大的市场。AI绘画刚出来的时候很多人嘲笑说难以替代画师，但是现在这个节点看[虽然对于高端场](https://google-research.github.io/seanet/musiclm/examples/)景不能替代，但是一些低端的场景如果画师不学习提升，其实生存空间应该会被大大挤压。类似的之前音乐的生产主要是歌手，编曲和词作者协作（当然有些能力比较强的可能自己都兼了），也属于创意性工作。但是这个任务拆开来看，现在词作其实可以用GPT解决，人声可以用TTS解决，只有编曲还有突破的瓶颈，但是因为这波AIGC的发展出现了google的MusicLM，还有meta的autocraft都潜在可能对于传统音乐行业有所冲击（当然现在的能力还弱一些）。另外受到GPT和文生图的这种文字驱动信息生成模式的影响，也有类似Bark这种文字驱动音频生成这样有趣的能力，其方法也是基于transformer模型。不过暂时看实用价值还不高（可以视为TTS增强），可能后面还需要观察一下。

上面描述的无中生有可能还需要进一步探索，但是结合已有信息去做转换，给潜在的商业应用提供了一些思路：

+   **歌声转化：**前一段比较火的AI孙燕姿就是一个典型的例子，完全重新生成不好做的话，仿例倒是可以做的，原理上其实就是对于要仿的音乐分离干声和伴奏，然后将目标音色训练模型，通过训练好的音色模型跟伴奏结合重新生成音乐，所以这块技术上其实有用到VITS的TTS，但是也有SVC独创性的算法。这里主要两块，一块是相当于分离了伴奏（当前的方式其实不算仿了就是拿过来用，但是如果像上面说的编曲可以通过AI生成，这块其实可以替换），另外一块就是训练个歌手的音色模型（其实可以是任何人的音色），然后把以分离出来的原始歌手干唱作为参考单纯把音色转换过去，这里面其实涉及到编曲和训练歌手音色的版权问题。

![](img/1ae16ae53b7a6218d48ee427e9d50d6b.png)

+   **检索式变声RVC：**这种方法可以将一个人的语音转换成另一个人的语音，而不需要训练数据对。它通过将输入语音与预先训练好的语音库进行比较，找到最相似的语音，并将其转换为目标语音。RVC只需要几十分钟的声音干声素材作为训练集，再加上普通显卡的支持，只需要几十分钟就能获得不亚于上面sovits4.0的效果，进一步降低了声音音色模型制作的成本

![](img/8a327db72909160655e36eb2334f5d02.png)

Bz站甚至顺势做了#AI虚拟之声实验室#的标签。该话题下的视频作品运用RVC、DDSP-SVC、DiffSinger、Diff-svc和SoVITS-svc等技术进行AI声音模型训练或应用，创作歌曲或配音，并投稿至【音乐-VOCALOID·UTAU】分区

![](img/35f3e089138dbf3e5abff562d38c7593.png)

# 在视频领域的应用

## 需求和挑战

这块其实相对来说跟视频平台的创编流程紧密相关，很多短视频和中长视频平台公司都有布局，在其创编工具中逐步完善相关的功能，但是现在大部分还都是过程辅助阶段，完全自主的从无到有生成能力上还存在一定的技术瓶颈，有一些国外的创业明星产品，有一些突破，但是暂时看实用性并不是特别高，我觉得除了下面说的合规和易用性挑战外，最大的技术以及生态发展挑战来说，就是稳定可控性了。现在很多视频生成能力时长都比较短，生成的效果虽然可以做到尽量满足文字的描述，但是可能很多关键性的细节难以控制，如果只是玩玩或者简单视频还行，大型的工业化生产就还有提升空间，视频也需要文生图的lora + controlnet时刻

![](img/2051f15825a2b599c26a3b4d4fb6841d.png)

![](img/c04bc0239eae17000820e42ba458261c.png)

![](img/f9af4815ef329f8d1fb78af89f26718c.png)

![](img/9dfb7c140e9b037058fcd011ed2e7344.png)

![](img/b158e1fb28ed5f01f4ec721f20a605c0.png)

![](img/e1e4189430916b87fd6fa7ee82815918.png)

## 原理和案例

+   **Runway: **这家公司参与了《瞬息全宇宙》的幕后制作，Runway的核心产品为视频编辑器Gen-1和Gen-2。Gen-1模型可以通过输入文本或图像，改变原视频的风格，例如将现实中的实拍镜头转换为卡通剪纸世界。Gen-2模型可以直接使用文本生成视频场景， 使用户可以使用文本提示生成“逼真的视频内容”并“自动剪辑视频”。

![](img/e68326c336301d8e2b058c79f9781f0f.png)

+   **Zeroscope:  **这是一款基于 Modelscope 的无水印视频模型，能够生成 1024 x 576 的高质量视频。该模型是使用 24 帧、1024x576 分辨率下的 9，923 个剪辑和 29，769 个标记帧，根据带有偏移噪声的原始权重进行训练的。主要是名为text-to-video-synthesis(文本生成视频大模型)，具有17亿参数数，由阿里达摩院提供。只需输入文本描述，就能生成对应的流畅视频。

![](img/c9faf6061cb57b5fe64d8472d2963447.png)

<https://fffiloni-zeroscope--qlggk.hf.space/file=/tmp/gradio/ebc612c3d979e65ef5a6ff0943a2e863eaacf9d5/tmpj43m67qj.mp4>

在草地上奔跑的狗狗：[Z](https://huggingface.co/spaces/fffiloni/zeroscope)eroscope Text-To-Video - a Hugging Face Space by fffiloni

+   **Pikalib**：https://www.pika.art/demo

<https://video.wixstatic.com/video/af70ef_cc6d03729d344f0b9fe8633ad81b9af1/360p/mp4/file.mp4>

<https://video.wixstatic.com/video/af70ef_caffbe10a41c4a848b49f6ac0af8a2df/360p/mp4/file.mp4>

# 三维模型应用

这块因为使用涉及比较少，仅提供一些易[观数据的](https://www.analysys.cn/article/detail/20021029)调研资料作为参考，更详细可以参考原始报告

![](img/6274c1ab78068e0eb058b607c0d62e27.png)

![](img/bee4ee0ace4ac2c2ed435d53dbf52692.png)

![](img/fd42dec6eac38f357ac9687741dcec85.png)

![](img/7082de462bcc61c674e87ef00d43fa36.png)

# 应用场景合集

|  |  | **创意生成（抽卡）** | **风格迁移** | **内容理解** | **局部调整** | **辅助工具** |
| --- | --- | --- | --- | --- | --- | --- |
| 文本 | 开源 | +   text-generation-webui：致力于打造文本生成领[域的stable](https://www.bilibili.com/video/BV1fh4y1M7DX/)-difussion-webui，但是感觉可能最大的制约主要是底模的使用门槛太高，虽然SD有一定门槛，大部分消费级显卡还是可以跑的，但是现在的LLM大部分还在ChatGPT3.5水平，对消费级显卡也不友好，最近看到一个视频讲的不错  | 类似图片也可以通过lora低成本实现，其实最早lora是用在文本生成领域的 | 基于数据源增强减少幻觉： | 基本LLM都是通过提示词完成的，比如常见的润色和纠错 | +   加速方案 |
|  | 免费工具 | text-generation-webui整合包（还在早期，相对于图像社区比较小众） | 基本LLM都是通过提示词完成的 | 基本LLM都是通过提示词完成的 | 基本LLM都是通过提示词完成的 |  |
|  | 用户产品 | +   [Character.AI](https://beta.character.ai/)：如果在基础模型能力上不会因为角色化变形，其实我觉得角色化可能是用户最有市场的场景，包括看到结合图像生成有更多的想象空间 | 基本LLM都是通过提示词完成的 | +   [chatbase](https://www.chatbase.co/)：使用任何在线或离线数据快速训练的聊天机器人，并将其作为iframe或支持小部件嵌入网站，从默认能力提供来看，也是跳出做通用，更偏向于做网站线索收集场景 | 基本LLM都是通过提示词完成的 | +   [relevanceai.com](https://relevanceai.com/)：构建端到端自定义LLM应用程序和代理，批量共享、使用和运行 |
| 图片 | 开源 | +   stable-difussion-webui：YYDS不解释，我觉得最核心还是开放的插件机制，以及丰富的插件生态 | 现在基本都是通过lora低成本实现 | 通用场景下，主要是识别图片中的大致内容以及生成描述 | +   stable-diffusion-inpainting：文本说明修改图片内容 | +   [CodeFormer](https://github.com/sczhou/CodeFormer)：图像超清修复，被集成到了SD-webui中 |
|  | 免费工具 | +   [秋叶整合包](https://www.bilibili.com/video/BV1iM4y1y7oA/)：基本是做的最容易上手的SD-webui整合包了，而且作者一直持续更新新功能，持续分享使用经验 | 现在基本都是通过lora低成本实现 | 暂未发现，感觉如果只是单纯的理解可能价值不大，更多跟应用目标结合，不过如果是OCR倒是有很多 | stable-diffusion-webui的各种整合插件 |  |
|  | 用户产品 | +   [Midjoureny](https://www.midjourney.com/)：底模能力太强大了，除了需要付费以及没有独立软件这两个缺点外，因为大部分的效果都是可以通过提示词实现，所以相对SD门槛会低一些 | Midjoureny更倾向于使用提示词描述，包括绘画风格，类型还有作者画风 | 这块OCR[的服](https://bobtranslate.com/service/ocr/volcengine.html)务还是挺多的，就不一一列举了，可以参考这里 | +   [改图鸭](https://www.gaituya.com/)：大部分的图片处理工作都可以用这个工具完成 |  |
| 音频 | 开源 | +   [Bark](https://github.com/suno-ai/bark)：通过提示词生成真实的人声，包括音乐、背景噪音和简单的音效，不完全是TTS，基于transformer模型，更类似文生图在音频的场景（音色抽卡） | +   [VITS](https://github.com/jaywalnut310/vits)（TTS）：下面的歌声转换是基于该能力实现的，你也可以结合自己的需求使用VITS-fast-fine-tuning微调定制 | 其实音频的理解本质可以归为文字的理解，所以下面主要是ASR，因为ASR的精度决定了文字理解的上限 | 暂无，不过可以通过很多剪辑软件切分和合并音频轴 | +   [AudioSep](https://github.com/Audio-AGI/AudioSep)：可以使用简单自然语言指令从混合音频中分离出特定的声音 |
|  | 免费工具 | +   [Bark整合包](https://www.bilibili.com/video/BV1MP411m7cw/)：下载整合包之后需要先联网，生成cache文件的内容有检验，没有打包，因为打包也没用 | +   [微软语音合成助手](https://www.bilibili.com/video/BV1QW4y1R7oo)（TTS）：输入文本可以选择希望的声音来朗读 | 其实音频的理解本质可以归为文字的理解，所以下面主要是ASR，因为ASR的精度决定了文字理解的上限 | 暂无，不过可以通过很多剪辑软件切分和合并音频轴 |  |
|  | 用户产品 | [ACE Studio](https://ace-studio.timedomain.cn/c)：结合AI能力进行音乐编曲，工具做的体验很棒，但是也需要一些基础的乐理知识才能用。因为普通歌手不太容易赚到钱，不确定长期商业前景 | +   [大饼AI变声器](https://dubbing.tech/)：免费客户端，有免费限时的声源可选 | 其实音频的理解本质可以归为文字的理解，所以下面主要是ASR，因为ASR的精度决定了文字理解的上限 | 暂无，不过可以通过很多剪辑软件切分和合并音频轴 |  |
| 视频 | 开源 | +   [Text2Video-Zero](https://github.com/Picsart-AI-Research/Text2Video-Zero)：文本到图像扩散模型的视频生成器，这个抽卡比图片还不可控 | +   [SimSwap](https://github.com/neuralchen/SimSwap)：具有单个训练模型的图像和视频上的任意面部替换框架 | 暂无，不过其实可以抽取视频帧转化成图片理解 | +   [FateZero](https://github.com/ChenyangQiQi/FateZero)：通过文本描述调整视频中的内容 |  |
|  | 免费工具 | +   sd-webui-text2video：SD插件方便直接使用 ModelScope 或者 VideoCrafter 实现图片生成视频的效果  | +   [CoDeF](https://github.com/qiuyu96/CoDeF)：传统的视频处理方法通常需要逐帧分析。CoDeF通过仅在一个图像上部署算法，并通过时间变形字段将结果传播到整个视频，提高了效率，实现对于视频内容的风格迁移 | +   Segment-and-Track-Anything：直接通过文字提取视频中的目标物体，在各种场景中的分割和跟踪功能，例如街景、AR、单元格、动画、航拍等 | +   [roop](https://github.com/s0md3v/roop)：拍一段视频，用你选择的脸替换里面的脸。你只需要一张想要的脸的图像。没有数据集，没有训练 |  |
|  | 用户产品 | +   [Runway Gen-2](https://runwayml.com/)：Runway是一家应用人工智能研究公司 艺术、娱乐和人类创造力的下一个时代，在视频生成领域效果卓越 | 之前出现过然后被快速封杀了 | 暂无，不过其实可以抽取视频帧转化成图片理解 | +   [Rask.AI](https://rask.ai/)：翻译视频，其实本质应该还是提取音频，然后音频合成回去 |  |
| 代码 | 开源 | +   [ChatDev](https://github.com/OpenBMB/ChatDev)：单角色SOP生成代码 | +   [gpt-migrate](https://github.com/joshpxyne/gpt-migrate)：轻松将代码库从一种框架或语言迁移到另一种 | +   screenshot-to-code：通过截图直接生成前端代码 | +    | +   [tldraw](https://github.com/tldraw/tldraw)：白板工具，借助GPT-Vision能力可以转化代码或者其他产物 |
|  | 免费工具 | +    |  |  |  |  |
|  | 用户产品 | +   [Github Copilot](https://github.com/features/copilot)：全能型代码辅助工具，支持通过自然语言编程 |  |  |  |  |
| 3D | 开源 | +   [shap-e](https://github.com/openai/shap-e)：OpenAI出品，比较擅长对于抽象物体的生成，形象人物效果比较差 |  |  |  |  |
|  | 用户产品 | +   [csm.ai](https://www.csm.ai/)：暂未试用过，图片转3D |  |  |  |  |

+   [ChatGLM2](https://github.com/THUDM/ChatGLM2-6B)

+   [FastChat](https://github.com/lm-sys/FastChat)

+   [BELLE](https://github.com/LianjiaTech/BELLE)

+   [stanford_alpaca](https://github.com/tatsu-lab/stanford_alpaca)

+   [qlora](https://github.com/artidoro/qlora)：降低微调成本

+   [AutoGPTQ](https://github.com/PanQiWei/AutoGPTQ)：降低模型使用成本

+   [CTranslate2](https://github.com/OpenNMT/CTranslate2)

+   [langchain](https://github.com/langchain-ai/langchain)

+   [MetaGPT](https://github.com/geekan/MetaGPT)

+   [llama_index](https://github.com/jerryjliu/llama_index)

+   [DemoGPT](https://github.com/melih-unsal/DemoGPT)：只需使用句子即可创建 LangChain 应用程序

+   [OpenLLM](https://github.com/bentoml/OpenLLM) 如何快速将各种LLM大模型部署到生产环境中，且轻松微调、部署、应用、调用大语言模型llm？

+   [LLM-Kit](https://github.com/wpydcr/LLM-Kit) 各大语言模型的全流程工具 WebUI 整合包。支持主流大模型API接口和开源模型。支持知识库，角色扮演，mj文生图，LoRA和全参数微调等等

+   Dify - LLMOps platform: 开源LLMOps工具，体验和设计一流

+   [OpenAgents](https://github.com/xlang-ai/OpenAgents/blob/main/README_ZH.md)：开源Agent搭建平台，无需代码开发能力，支持本地部署

+   [Agenta](https://github.com/Agenta-AI/agenta)：用于构建强大LLM应用程序的LLMOps平台。轻松试验和评估不同的提示、模型和工作流程

+   [FastGPT](https://github.com/labring/FastGPT)：知识的问答系统，为数据处理、模型调用等提供开箱即用功能。通过使用Flow工作流编排实现复杂问答场景

+   [botpress](https://github.com/botpress/botpress)：构建和部署GPT/LLM智能体的开源市场

+   [bisheng](https://github.com/dataelement/bisheng)：开源大模型应用开发平台，给用户提供最佳实践的方案，包括分析报告、知识库问答、要素提取和对话等

+   [AgentSims](https://github.com/py499372727/AgentSims)：通过模拟沙箱环境，设定人物角色，可以模拟验证智能体特定能力

# 智能体开发

> 相比于如火如荼的百模大战，还有卷得不能再卷得基于模型API的上层应用开发，后续更有想象空间的方向可能使智能体的开发，外文中一般叫做 agent，翻译过来就是代理，比较容易引起歧义。所以一般叫做智能体会更加贴切一些。ChatGPT给的定义：在计算机科学和人工智能领域，"Agent" 通常指代一个能够感知环境、做出决策并执行操作的实体。这个实体可以是一个程序、一个软件机器人或者其他类似的系统。"Agent" 可以根据预定义的目标或目标函数来行动，以最大化某种性能指标。

![](img/1a3674027c2fe32eb5b34628bbd8a58e.png)

|  | Agent | 微调 | 知识库 | 提示词工程 |
| --- | --- | --- | --- | --- |
| 技术原理 | 有自主规划和反思能力，会利用工具 | 常见的是通过Lora，更详细参考[PEFT](https://github.com/huggingface/peft) | 向量化后基于语义搜索匹配 | 激活预训练知识，进行角色扮演 |
| 进入门槛 | 需要熟悉业务流程，有更资深的工程开发能力 | 需要一定算法经验 | 需要一定工程开发能力 | 能够清晰描述需求 |
| 应用场景 | 部分替代人类工作 | 对于准确性要求极高，数据私有化，辅助人类工作获取准确信息 | 对于准确性要求较高，辅助人类工作获取较为准确信息，适合咨询和客服有一定的准确性容错空间 | 开放性对话，对于错误容忍度较高，更追求创意和启发的场景 |
| 数据需求 | 领域SOP以及专有知识 | 垂直或者私有数据 | 有存量常规数据，比如文档和表格 | 用于Few shot示范的简单数据 |
| 算法要求 | 除了GPT算法还有规划算法 | 深入理解 | 基本理解 | 无 |
| 平台级工具 | [这块暂时没有看](https://www.gptbots.ai/)到完全[成熟的，看到初具](https://botpress.com/)雏形的平台是 gptbots 和 botpress，其实知识库部分的LLMOps平台们如果扩展的工具使用能力和规划能力都可以迁移过来，而且可以明显看到有些平台是规划的，也有实现比如对接API的能力，只是可能还在开发中 | 因为[这块比较依赖](https://www.lamini.ai/)基础模型的选择，现在好的基础模型被少数公司垄断，开源[这块发展](https://openai.com/blog/gpt-3-5-turbo-fine-tuning-and-api-updates)比较快，但是相对成熟可以的主要是llama和chatglm，所以平台级别看到的比较少，因为关注吴恩达的课程有了解到一个叫 lamini 的平台，主要就是基于 Llama 2 的。另外闭源软件方面就是OpenAI前一段升级提供的微调接口了 | 比较纯粹的例如 [quivr](https://www.quivr.app/) 和 [danswer ](https://docs.danswer.dev/introduction), 甚至[whismer](https://whismer.com/)[等对接不](https://dify.ai/)同[知识源，进一步的](https://fastgpt.run/)平[台就是LLMO](https://dataelem.feishu.cn/wiki/ZxW6wZyAJicX4WkG0NqcWsbynde)ps[平台，比如d](https://agenta.ai/)ify、fastchat、bisheng和 agenta，国外的开源平台比较技术化相对纯粹简洁，国内的平台对于私有部署、多端透出做的比较完善，比如 Chato、brainstormgpt 还有360AI数字员工，看宣传上限也只是微调，但是就开始提&quot;数字员工&quot;了 | 大公司开放的C端应用就可以，比如[文心一言](https://yiyan.baidu.com/)、[科大讯飞](https://xinghuo.xfyun.cn/desk)、[字节豆包](https://www.doubao.com/) 和 [通义千问](https://tongyi.aliyun.com/)等等，不一而足，很多LLMOps平台最早的能力就是可以自定义用户prompt来实现小应用，还有很多prompt的共享或者交易平台 |

+   提示工程工具：

+   GPT提示词的结构化表达，提供通用提示词书写模板：https://github.com/yzfly/LangGPT

+   为提示词实验生成测试用例，找到最佳提示词：https://github.com/mshumer/gpt-prompt-engineer

+   知识库工具：

+   使用BERT模型支持句子、段落和图像向量计算：https://github.com/UKPLab/sentence-transformers

+   LLM应用程序的数据框架：https://github.com/jerryjliu/llama_index

+   使用LlamaIndex做的生产级应用：https://github.com/run-llama/sec-insights

+   使用了sentence-transformers的综合应用：https://github.com/StanGirard/quivr

+   内存处理，使用typescript实现的向量计算：https://github.com/oramasearch/orama

+   中文词向量：https://github.com/Embedding/Chinese-Word-Vectors

+   开源向量数据库：https://github.com/chroma-core/chroma

+   相似性搜索和密集向量聚类：https://github.com/facebookresearch/faiss

+   连接到现存的数据库：https://github.com/mindsdb/mindsdb

+   本地知识库学习：

+   https://github.com/PromtEngineer/localGPT

+   https://github.com/imartinez/privateGPT

+   微调：

+   量化模型微调QLora：https://github.com/artidoro/qlora

+   综合市面上大部分高效微调方案：https://github.com/huggingface/peft

+   传统基于开源模型微调例子：https://github.com/ssbuild/chatglm_finetuning

+   模型推理：

+   支持HuggingChat生产环境级性能：https://github.com/huggingface/text-generation-inference

+   特色在于支持CPU计算推理：https://github.com/xorbitsai/inference

+   用于LLM的高吞吐量和内存高效的推理和服务引擎：https://github.com/vllm-project/vllm

+   **加速推理的分页****注意力机制**：https://github.com/vllm-project/vllm

+   性能优化：

+   缓存LLM语义化结果：https://github.com/zilliztech/GPTCache

+   上面多方面的**综合应用**：https://aigc.phodal.com/

+   **面向未来的****智能体****时代：**从传统机器学习，到大语言模型，再到智能体时代的演进，其实个人理解，从大语言模型往智能体迁移，最重要的点在于工程体系的建立，需要将现在很多的单点突破有机结合实际场景的需求组合在一起，发挥出真正替代人类工作的作用，开始可能是部分工作内容，实现初步的共生关系，后面可能逐步发展为角色替代。当下相关的技术能力都初具雏形了，只是还比较依赖有经验的人去结合场景组合应用，后面是否像现在相对成熟的LLMOps一样，涌现出更多通用的平台去抹平下限，这块还需要观察

![](img/b17d80ebc44374edb89e4bd2d388d738.png)

+   **智能体相关技术发展：**可以明显看到，之前的技术进展都是年为单位的，现在都是月为单位的了，这一波热潮带动了理论的完善，也带动了很多面向 AGI 的应用尝试，现在也基本到了热度顶点开始衰退的阶段，下一波的潮流可能需要一个全球级别杀手级应用来带动了

![](img/d43eba503fd6d67562474ebdca3562b9.png)

+   **智能体****开发生态：**下面的图从开源和闭源两方阵营进行对比，围绕多个垂直领域的产品进行汇总。可以看到明显的区别就是闭源软件更偏重于编码和生产力领域，而开源领域是全面开花（当然这个图也未必是真的全景数据）但是可以从一个切面看出来，闭源软件因为面向VC和企业，会更加稳妥选择投入的领域，而代码和生产力起码是看上去不会错的方面，所以就比较卷，而相对来说垂直领域的企业应该会比较有机会，当然也是有行业理解和业务知识上的天然壁垒，开发者去切入如果没有相关储备也基本只能做表面上的能力，我觉得这也可能是开源的初衷，希望收集社区反馈

![](img/c898b8f4c9ad0249afd7fc09b83a3844.png)

## 人机协作模式

> 也许CUI（命令行交互界面）并不一定是最优的交互模式，但是在现有人机交互范式中引入 agent 改善体验还是有很大的机会，比如各种IM工具中，agent天然适合在这个协作场景下进行人机协作，甚至是agent和agent之间的协作，可以想象到机器人对话其实也是可能的，后面的多智能体协作也有体现

![](img/9915921a3a540392bf618c395c0b66d7.png)

从互联网时代到移动时代，虽然交互的细节上有很多新的理念和方法，但是本质都还是需要人理解软件系统，通过跟软件系统的交互实现数据资产对于现实状态的映射和追踪，人需要理解软件交互达成自己的目标。而通过 agent 可以打破这种形式，当然并不是说通过自然语言命令会完全替代现在的 GUI 交互形式（因为也会存在一些挑战，后面会提到），但是会实现现有GUI交互的迁移，对于软件的设计也会提出新的挑战，需要从以往堆砌交互，引导用户使用系统，转变成思考用户的目标是什么，哪些大块的操作是可以通过 agent 自主能力替代的，那些仍然需要用户介入，更好跟 agent 形成互动推进流程的

![](img/cf83b203929ce1fa60f7431401a65367.png)

通过下图也可以大概理解 agent 跟传统 AI 能力集成的区别

## 智能体结构

> 下面的智能体主[要以软件](https://github.com/xlang-ai/xlang-paper-reading/blob/main/llm-robotics-and-embodied-ai.md)智能体为主，但是整体架构以及一些思路其实跟机器人也有一定的相关性和启发性，关于机器人相关的论文研究可以参考这里，最新的综述论文可以参考这个 LLM-Agent-Survey 和这个 LLM-Agent-Paper-List

![](img/a90af86f50301a6e784c667e79036508.png)

![](img/8b73003cbedc7009ed92457cbe085618.png)

![](img/35367b638fad46a3818fef79c1d6a20f.png)

![](img/2d392f31e105b8e18d908c081c1b5b1f.png)

微软开源的 [Jarvis](https://github.com/microsoft/JARVIS) 也有类似的结构，区别于上面的通用建模框架，在对开源模型的应用上做了细化，在一定程度上表现出了多模态的能力，如果把CV和ASR的能力在“记忆”这个层面做结合和扩展，可能会有进一步增强。

[而跟开源模型做结合实现复杂](https://modelscope.cn/studios/damo/ModelScopeGPT/summary)功能，会[看到阿里的 m](https://github.com/ShishirPatil/gorilla)odelscopeGPT 还有 Gorilla 也有不错的实践。

![](img/e6e19bbda1ba762438ddef9a5d09c328.png)

如果有相关知识储备的朋友会发现，这个结构跟之前可能被j寄予厚望来实现AGI的强化学习结构很相似，下面是传统的强化学习结构，环境、智能体还有反馈这些要素在 LLM Agwnt 其实也有体现。强化学习中一个很重要的思路是“不断试错”，通过定义奖励规则，让智能体通过跟环境交互来合理激励模型收敛到目标任务的达成上。之前的尝试来说，其实这个过程相对来说能够解决问题的复杂度有限，或者说更加适合输出指令相对简单的场景，比如之前看到很多的应用是在游戏和工业四旋翼飞机上，这些场景的任务目标比较清晰，奖励函数相对好设计，平衡好短期和长期的奖励目标就可以达到不错的效果。包括其实在ChatGPT训练过程中的RLHF，也有涉及利用RL来强化LLM对齐人类偏好。其实个人感觉两者不是二元对立的，其实在下面 LLM Agent 通过 Reflexion 来做 Planning 其实就用到了RL。

对比来看，可能完全基于 RL 的 Agent 奖励函数需要每个例子单独设计，而相对来说，基于 LLM 的 Agent 因为通过SFT 或者 RLHF 对于模型隐含了通用的奖励函数（对齐人类目标）所以相对来说可以在更多难以精确定义奖励的场景默认达到符合人类预期的效果

![](img/5fb86599ad490d32ea5648972074a5ff.png)

+   从 OpenAI 的 Lilian Weng 最早期相关总结其实启发了很多后面的相关研[究，包括](https://lilianweng.github.io/posts/2023-06-23-agent/)很多将智能体的介绍也都会持续引用这篇文章，很多概念我觉得到现在仍然是适用的，所以基于这篇文章提出的关键概念，我们分开来看 LLM Agent 的三大块能力

### 规划和反思

> 下面的模型跟强化模型比较类似，模型可以通过跟环境的交互反馈，形成“反思”，这种“反思”跟人类意义上的不太一样，更多是激励因素带来的引导作用，通过环境的反馈更好引导模型向预期的奖励目标靠近

![](img/bd16ddb9d4a9a18672d40203267a75ca.png)

+   **任务规划和分解：**从最早提出的思维链，激活了最基础的LLM任务拆分能力，再到树形思考路径，相出多个可行解，然后择优选择，再到最近看到更加灵活自由度更高的图结构，这个演进跟“数据结构”有异曲同工之妙

+   CoT：https://github.com/Timothyxxx/Chain-of-ThoughtsPapers

+   ToT：

+   开源算法实现：

+   https://github.com/kyegomez/tree-of-thoughts

+   https://github.com/princeton-nlp/tree-of-thought-llm

+   提示词实践：https://github.com/dave1010/tree-of-thought-prompting

+   GoT：https://github.com/spcl/graph-of-thoughts

![](img/0e9eb2103fd48529980240802486e026.png)

![](img/efb0ed92b8d2215d83de1e0ba8844ff0.png)

+   **行为反思：**通过试错后根据环境反馈来反思行动有效性，积极进行调整，这里的思路个人感觉跟强化学习比较类似，只是这里更侧重于预测下一步行动的概率，而强化学习会更新激励的权重

+   [ReAct](https://github.com/ysymyth/ReAct)：一般按照思考、行[动和观察这样重](https://github.com/QwenLM/Qwen-7B/blob/main/examples/react_prompt.md)复的指令输入给LLM，LLM根据这些信息进行“思考”和下一步动作的预测。当下这种反思的能力更多用于训练LLM对于工具的使用，比如通义千问的例子

![](img/cbafaeaf46bc3a16053dc18edb215a4b.png)

![](img/8718b9900468113e4b2dd341483fe34f.png)

![](img/7b50edf9cc0f8aa6cf819c7889260d3f.png)

+   [Reflexion](https://github.com/noahshinn024/reflexion)：直接运用了强化学习，通过一个标准的RL设置，其中奖励模型提供简单的二元奖励，动作空间遵循ReAct中的设置，其中特定任务的动作空间用语言增强，以实现复杂的推理步骤。通过向LLM显示两个示例来创建的，每个示例是一对（失败的轨迹，指导计划未来变化的理想反射）。然后反射被添加到代理的工作记忆中，最多三个，用作查询LLM的上下文。

![](img/769aa077fe199ae0c05a7a3d0f0791e9.png)

![](img/a56e2c1372d59248c78de27c66a9ddad.png)

+   Chain of Hindsight：对于模型使用CoH结构微调后，可以通过提示引导增量提升输出效果，但是看起来不是很实用，如果一步到位在大多数场景可能更好，这种可能更适合探索性话题，所以不过多解读评论了

![](img/48cc9ba4a7a08325e85ca02073e9c40e.png)

![](img/10a0f286890ccab8f64e0ad8eda556cd.png)

### 长短期记忆

> 记忆会分很多种类型。理论上利用数字存储的能力，机器在这块是比人类有更多优势的，但是当下多模态的任务还没有明显突破，所以在视觉听觉等相关记忆要素上，包括“直觉”能力上，人类还存在实际的优势

![](img/9244c69c2501d6b61338803c50fa3cdc.png)

+   [faiss](https://github.com/facebookresearch/faiss)：在AI火起来之前就广泛使用的生产级向量搜索库，对大规模机器学习任务非常有益，包括最近邻搜索、聚类和近似最近邻搜索，有广泛的应用，包括图像检索、音频识别、自然语言处理和推荐系统

![](img/17ace6f23e57c9716d007a25114b70f8.png)

![](img/3907f292ad0c32d711d8198a77f76e38.png)

+   [chroma](https://github.com/chroma-core/chroma) 开源的向量数据库，不管效果如何，开源的向量数据库非常容易跑通。使用量比较高，主要在langchain早期的实现中关于向量数据库的例子就是使用这个，所以有点被带飞的感觉

![](img/8da5c0e866c25fa40a49827a758b05b0.png)

+   [weaviate](https://github.com/weaviate/weaviate)：一个开源的向量数据库，它既存储向量也存储对象，允许将向量搜索与结构化过滤相结合，具有云原生数据库的容错性和可扩展性，所有这些都可以通过GraphQL、REST和各种语言客户端访问。Dify的向量存储方案就是用的这个

![](img/d0aa4687eae29a2b51564659b2bf1caf.png)

+   [deeplake](https://github.com/activeloopai/deeplake) 另外一个向量数据库公司，对于公有云服务支持较全面，自己的云服务中支持了100+的流行图片、视频和音频数据集，支持存储向量、图像、文本和视频

![](img/02528fc6648b514fa0c065367de5166d.png)

+   [GPTCache](https://github.com/zilliztech/GPTCache) 在实际使用 LLM 回复内容前进行向量相似性评估，减少对于大语言模型的调用，用了语义缓存等替代策略。语义缓存识别和存储相似或相关的查询，从而增加缓存命中概率，提高整体缓存效率。通过缓存命中率、延迟以及召回这三个指标可以较好评估缓存带来的效果

![](img/d473282b5a4ed2fd82d426554fac779e.png)

+   [txtai](https://github.com/neuml/txtai) 不只支持语义搜索，还支持LLM编排和工作流，向量数据库是向量索引（稀疏和密集）、图网络和关系数据库的结合。所以向量搜索具有SQL、主题建模、检索增强生成等功能。向量数据库可以独立存在和/或作为大型语言模型（LLM）提示的强大知识来源。

![](img/1fcc6dc793c3da27b6b547bc6a2beb11.png)

![](img/9d2c2fada8c660b6c1d608ed8d981412.png)

![](img/0fc5661910e150f49c9ee303e08fd2d4.png)

![](img/6a7da618ba9b0d970de2e3fd5edc8a34.png)

![](img/ad5e2c60f0265b6de58e82cdf8a6236e.png)

+   [lancedb](https://github.com/lancedb/lancedb)：使用持久存储构建的用于向量搜索的开源数据库，它极大地简化了向量化的重新修复、过滤和管理。
LanceDB的核心是用Rust🦀编写的，并且使用Lance构建，Lance是一种为高性能ML工作负载设计的开源列格式

![](img/f5fc4727e1faca844d91b1dc096a0393.png)

+   [semantic-memory](https://github.com/microsoft/semantic-memory) 专门用于通过自定义连续数据混合管道对数据集进行高效索引。在某些情况下也可以作为工具库使用，并且快速可以作为Docker容器使用。利用高级嵌入能力和LLM，启用自然语言查询以从索引数据中获取答案，并附有引用和原始来源的链接。

![](img/96bce67656b8a3973f08a8c7439bb7c7.png)

![](img/a3672382eff02b589bc8a071e3c8cad6.png)

+   [LongMem](https://github.com/Victorwz/LongMem)：对于Arxiv论文“[使用长期记忆增强语言模型](https://arxiv.org/pdf/2306.07174.pdf)”的官方实现，主要用于对大语言模型的记忆能力增强

![](img/73f5477bb88f4ed0caafadd1aa60e4df.png)

![](img/67b1f4510d7b0c7f3e685946a7eb4418.png)

### 工具使用

> 针对已经被数字化的工具，模型在被特定训练后对于工具的使用也会有明显的优势，特别是很多工具有更加结果化的描述，也会更加容易被模型学习到使用的方法

![](img/9350c9e7f5508e025fe2b6eae903abf0.png)

[下面讨论](https://github.com/xlang-ai/xlang-paper-reading/blob/main/llm-tool-use.md)的工具以软件工具为主，相关的论[文参考这](https://github.com/Timothyxxx/EnvInteractiveLMPapers)里，也有关于跟真实环境工具交互的相关研究，下面是我总结的一些常见工作场景，ChatGPT打样的主要是代码沙箱，也是相对其他来说最通用的，因为只要是软件应用都是通过写代码实现的，而GPT本身具有代码生成的能力，如果给生成的代码提供运行环境就可以自动实现相应的功能，这也是个自然而然的延展思考过程。而插件能力则是对于存量应用的“温柔”，存量应用可以集成到GPT的生成流程中继续发光发热（虽然很多其实为了引流或者品牌透出，其实大部分更希望的是将GPT集成到自己的业务中）

+   [gpt-code-ui](https://github.com/ricklamers/gpt-code-ui) 简单模仿OpenAI的能力，提供通用的工具使用方案：生成代码同时通过提供本地沙箱运行环境执行代码达到基本的工具使用能力

![](img/04fd9e2018dc15e220aac53c6b84dc33.png)

+   open-interpreter 提供命令行交互界面，基本原理跟上面类似，不只是支持本地执行生成代码还可以执行shell命令，除了可以连接常规的openAI服务，也支持对接本地的LLM，可以看作对于上面的增强CLI版本

<https://user-images.githubusercontent.com/63927363/264166941-37152071-680d-4423-9af3-64836a6f7b60.mp4>

+   [OpenCopilot](https://github.com/openchatai/OpenCopilot) 让大语言模型理解存量的接口定义，结合GPT的推理能力合理调用使用接口，对于简单的系统无需了解操作界面，基于GPT对于接口的理解，跟GPT使用自然语言交互实现对于接口的调用

<https://user-images.githubusercontent.com/32633162/261857672-3bf5c24d-572c-4a42-9e45-40f05e5a16b2.mp4>

+   [RestGPT](https://github.com/Yifan-Song793/RestGPT) 跟OpenCopilot比较类似，都是期望通过让LLM理解swagger接口定义来使用API接口，在国外Open API接口服务的生态比较成熟，很多大公司的服务都可以使用 restful 接口调用，通过 Planner 自主规划智能体对于接口的调用和解析

![](img/fbb6900100c73b35663e82d4be757ce3.png)

+   [BMTools](https://github.com/OpenBMB/BMTools/tree/main/bmtools/tools) 固定场景的工具支持能力，比如常规的搜索，天气查询，算数运算，提供了多种现成的集成能力，主要通过实现一个一个工具的集成来扩展，能够保证精品度，但是丰富度受限于工具实现的丰富度，当前内置的开源实现工具已经比较多了，但是如果场景比较特殊仍然需要自己扩展

![](img/4db525e9d586fa5df9bf0251c27824f3.png)

+   [ToolBench](https://github.com/OpenBMB/ToolBench) 旨在构建开源、大规模、高质量的指令[调整 SFT ](https://github.com/ShishirPatil/gorilla)数据，以促进构建具有通用工具使用能力的强大LLMs。赋予开源 LLMs 掌握成千上万多样的真实世界API能力。通过收集高质量的指令调整数据集来实现这一目标。该数据集使用最新的ChatGPT（gpt-3.5-turbo-16k）自动构建，该版本升级了增强的函数调用功能。提供数据集、相应的训练和评估脚本，以及在ToolBench上经过微调的强大模型ToolLLaMA。可以自己训练一个Gorilla

![](img/44cf1e01cf3e27e5f8c4be8ab18899eb.png)

<https://user-images.githubusercontent.com/25274507/256989924-f1151d85-747b-4fac-92ff-6c790d8d9a31.mp4>

## 工程化应用

> 说到底其实智能体实现也是一个LLM应用，所以整体设计上跟开发一个常规LLM应用的技术[栈有很多](https://a16z.com/2023/06/20/emerging-architectures-for-llm-applications/)重叠的地方，而我觉得LLM App Stack的定义，更侧重于工程，也就是落地实现的细节，关于LLM应用开发技术栈相关的更详细的介绍可以参考这篇文章

![](img/b157df393aaad8e754a5116cc0aac566.png)

![](img/7527c1ac6dc185f7ea145f1a8a8abd83.png)

而关于更大范围的大语言模型的开发生态，可以参考下图，智能体的开发对于下层会有很强的依赖的相似性，而在应用层，智能体可能会被嵌入到应用层中，或者作为独立工具提供服务

![](img/f50e1f194fedfb59f3cce7772d23f157.png)

![](img/c27f2c7c83feae8b0913f141ac33f923.png)

![](img/e4e9d71a2994af89d73c3fa98b950892.png)

而对于智能体最关键的“大脑”，虽然现在仍然是闭源生态的各种模型独占鳌头，但是开源生态的发展也是在蓬勃向上的，特别是meta开源商用的llama 2，应该会带来一些新的变化

+   大语言模型全览：https://github.com/Hannibal046/Awesome-LLM

+   大语言模型综述：https://github.com/RUCAIBox/LLMSurvey

+   多模态模型：https://github.com/BradyFU/Awesome-Multimodal-Large-Language-Models

+   企业私有模型开发案例：[如](https://www.bilibili.com/video/BV1Ez4y1n7Fu/)何定制企业私有化AI大模型？定制行业专有大模型、企业llm知识库的路径和方法论，含llm知识库构建演示案例

+   AI模型理解误区： 百万成本微调垂直行业达模型VS低成本建立企业专属知识库或ai助理

+   OpenResume+Flowise：本地化利用AI大模型生成简历及输出精美的简历模版，可用于提供商业化服务

+   Llama相关信息：https://www.interconnects.ai/p/llama-2-from-meta

+   小而美的模型：https://github.com/jzhang38/TinyLlama

## 开源实现

> 下[面的图来](https://github.com/e2b-dev/awesome-sdks-for-ai-agents)自 e2b.dev 的整理，比我下面列出来的更多一些，我补充了几个自己更感兴趣的框架，而更多开源实现框架的总结参考这里

![](img/334fe85c3ab0e617090bd01d042a89a2.png)

### [Langchain](https://python.langchain.com/docs/get_started/introduction.html)

LangChain是一个用于开发由语言模型提供支持的应用程序的框架。它支持以下应用程序：

+   连接数据：将语言模型连接到其他数据源

+   生成代理（跟下面的MegaGPT定位匹配）：允许语言模型与其环境交互

期望通过[沉淀组件和C](https://github.com/langchain-ai/langchain)h[ain提升复用性，](https://github.com/hwchase17/langchainjs)但是其实针对这块社区的反馈差异比较大，组件的确是有复用性的，但是Chain的复用性有多少值得商榷，特别是现在大语言模型驱动主要是自然语言为主，过多的封装带来调试的成本，另外因为现在大语言模型其实标准化程度不高，很多chain可能会只在特定的语言模型上有效。此外lanchain其实提供了python和typescipt实现，普遍typescript的实现会落后于python实现的支持，所以也可以在node甚至浏览器端使用。我觉得说设计有些混乱，我觉得可能主要作者本身是实践派的，理论上没有特别完备时做的工程上的抽象，因此很多设计是逐渐补丁上去的，没有经历过特别大设计上的重构，如果对于prompt自定义诉求不高，其实功能丰富度还是不错的

![](img/5fe6fb88d2c7a55c60d3f10253d75c52.png)

### [MegaGPT](https://github.com/geekan/MetaGPT)

个人觉得应该是受到了之前OpenAI研究员Lilian Weng的启发，整体概念有比较多的相似之处，同时[也做](https://arxiv.org/abs/2308.00352)了很多概念的扩展，包括单智能体的神经和直觉（其实这块按照Lilian的阐述，应该算在之前的记忆部分），多智能体的协同等。从顶层架构对框架进行了抽象设计，使用更加符合直觉，也具有比较良好的扩展性。一些案例做的也比较亮眼，比如可以模拟软件公司的研发流程实现一些有一定复杂度的实现，前一段还发现他们那竟然还社区共创出了论文

![](img/a69cf9933900947f401631930e99c0e7.png)

![](img/161876e21d65a9c2e3a560e22335c8f0.png)

+   找到针对开发领域比较“元实现”的部分，模拟实现了一个软件公司的组成结构

![](img/074a1608e0ed4cf70ecb86f4ce27cb0f.png)

![](img/fd0678a9ecda330ed6d44b132b112272.png)

+   完善的生态设计思路，包括基建层，中间件层（开发框架），应用层（智能体市场）

![](img/9ddbb6c8dff968717ac08342ab91417e.png)

+   **案例1：**通过命令行告诉GPT实现完整命令行“贪吃蛇”小游戏

<https://user-images.githubusercontent.com/2707039/250054654-5e8c1062-8c35-440f-bb20-2b0320f8d27d.mp4>

+   **案例2：**实现推荐算法架构

![](img/176cee93572fe198e83f5517c1c28860.png)

+   更多案例：

+   智能体架构师生成架构图：https://github.com/geekan/MetaGPT/tree/main/docs/resources/workspace

+   智能体生成的小游戏：https://github.com/geekan/MetaGPT/releases/tag/v0.1.0

![](img/4b98f3c8b6968556a412e5a0d51002f1.png)

### [AutoGen](https://github.com/microsoft/autogen)

可以使用多个 Agent 来开发LLM应用程序，这些 Agent 可以相互交流以解决任务。AutoGen代理是可定制的，可转换的，并且无缝地允许人类参与。它们可以在各种模式下运行，这些模式采用LLM，人工输入和工具的组合。人工可以随时介入，减少不必要的组件模块，专注于搭建 agent 应用，通过 function 调用实现工具调用

+   使构建下一代LLM应用程序的基础上，多代理对话，以最小的努力。它简化了复杂LLM工作流程的编排，自动化和优化。它最大限度地提高了LLM模型的性能，并克服了它们的弱点

+   支持复杂工作流的多种对话模式。通过可自定义和可转换的代理，开发人员可以使用AutoGen构建有关会话自治、代理数量和代理会话拓扑的各种会话模式

+   它提供了具有不同复杂性的工作系统的集合。这些系统跨越了来自各种领域和复杂性的广泛应用，从而支持各种对话模式

+   提供增强的LLM推理。它提供了简单的性能调优、API统一和缓存等实用程序，以及高级使用模式，如错误处理、多配置推理、上下文编程等

![](img/534f37686921a7659ec782457f338feb.png)

### [semantic-kernel](https://github.com/microsoft/semantic-kernel)

提供了一套SDK，它集成了OpenAI、Azure OpenAI[和拥抱脸等](https://learn.deeplearning.ai/microsoft-semantic-kernel/)大型语言模型（LLM），支持使用C#、Python和Java等传统编程语言。只需要几行代码就可以将相关的插件连接起来。特殊之处在于它能够使用人工智能自动编排插件。使用语义内核规划器，可以要求LLM生成一个实现用户独特目标的计划，之后就可以自己为用户执行计划任务。因为是微软出品，而且是战略级的开发框架，起码不维护的可能性会相对小一些，deeplearning还推出了相关的小课

![](img/8609c23c7e8604f488978fd81c49164e.png)

还有配套的VSCode插件，毕竟是微软自家的，整体的开发体验较好

![](img/1b3f2377672bdb3543dc1a1ba22725a9.png)

### [XAgent](https://github.com/OpenBMB/XAgent)

使用双循环的结构，外循环规划任务，内循环结合API和工具箱执行具体子任务，实现了自主和可控性的平衡

+   自治：可以自动解决各种任务，无需人工参与

+   安全性：所有的动作都被限制在Docker容器中，保证数据安全

+   可扩展性：具有良好的扩展性，可以轻松地添加新的工具，以提高 agent 的能力，甚至新的 agent

+   web界面：提供了一个友好的GUI，供用户与 agent 进行交互，也可以使用命令行界面与代理交互

+   与人类合作：不仅能够在过程中遵循您的指导来解决复杂的任务，而且还可以在遇到挑战时寻求人的帮助

![](img/d2b738716f3594d1993eae3ee448c898.png)

![](img/3581c1c88ae278c1a3394dd9e3fa4de5.png)

### aiwaves-cn/agents

刚出来没有多久，可以用于构建自主语言代理的开源库/框架。该库经过精心设计，支持包括长短期记忆、工具使用、网络导航、多代理通信在内的重要功能，以及包括人机交互和符号控制在内的全新功能。只需填写自然语言的配置文件，即可自定义语言代理或多代理系统，并将语言代理部署在终端、Gradio界面或后端服务中。比较特色的地方在于可以定义SOP，SOP为整个任务定义子目标/子任务，并允许用户为语言代理定制细粒度的工作流程。可以通过配置文件定义SOP，最新版本甚至可以通过调用LLM或者案例库自动生成SOP

![](img/3e3fd41e5eeb1c6b1637a06f83029550.png)

### [Autochain](https://github.com/Forethought-Technologies/AutoChain)

Langchain轻量版，降低了 langchain 的过多的抽象层次，借鉴了 autoGPT 中的概念，很多概念同时也跟 langchain 保持对齐，所以如果对于 langchain 使用觉得负担太重，autochain 是一个不错的备选方案，不过看 autochain 的实现，针对单一概念支持的外部服务相对较少，比如工具只支持 google 搜素，向量数据库支持 pinecone、chromaDB、LanceDB，社区生态还没有 langchain 繁荣，如果你的智能体不依赖一些特殊服务，或者自己愿意实现，倒也不是个大问题。下面是默认的 chain 流程图

![](img/fc29777c581fc8f4a4ef3ea1ba5de881.png)

### [Agently](https://github.com/Maplemx/Agently)

非常精简但是全能的框架，使用nodejs开发，充分利用了nodejs运行态以及javascript语言的特性，可以使用异步处理过程以及链式调用串联多种能力组合。核心的代码不多，但是已经可以实现相对复杂的功能，对于理解 angent 的基本要素和学习有挺大帮助，也可以用于基于ChatGPT的智能体应用开发，暂时内置实现仅支持OpenAI的API模型以及MiniMax模型，但是因为框架做了比较合理的拆分，可以比较方便自行进行接入扩展。对于存量业务，如果使用的nodejs开发框架，可以像瑞士军刀一样嵌入使用流程中。

![](img/3ef1a04bdcfe0efbd1be61990e83192b.png)

## 云服务商

> 今年四月，AWS 的数据和机器学习副总裁 Swami Sivasubramanian 宣布了 Amazon Bedrock 和 Amazon Titan 模型作为在 AWS 上构建生成式人工智能的新工具的一部分。Amazon Bedrock 目前在预览阶段，这是一个完全托管的服务，通过 API 提供了来自 Amazon 和领先的人工智能初创公司（如 AI21 Labs、Anthropic、Cohere 和 Stability AI）的基础模型（FMs），也可以通过调用AWS的Faas服务增强服务能力

+   基本结构

![](img/24630471b303d8600156c50e44e3f1b4.png)

+   创建并填写基本信息

![](img/27cef5ed3883be4f7f619266aa40f083.png)

![](img/e10ccf5dce8ca924146df659e87ffcea.png)

![](img/51615311972427c5a445a6e98c190294.png)

+   选择大语言模型

![](img/5a6540cfae5c2272f6c94c0eb75d90ef.png)

+   选择执行动作组，这些动作可以是API也可以是Faas函数

![](img/0b651c64d78cf0e64f2bdb53bac2ba0c.png)

+   确认基本信息

![](img/005cc5390f36352e78230ad41df20709.png)

![](img/78b9ca04d4dd1bb5050d4d7cbe0da9a8.png)

+   部署智能体生成对话机器人

![](img/f74e6ebf731956a80998ec98799b2a15.png)

![](img/370d4dee23246fce0ba2a06a510a3024.png)

![](img/d337928273fcd1760ddaa67ddf1f861b.png)

![](img/322ae90970d6dedf90f3eb3b7ec4ee26.png)

## 单智能体实现

![](img/35293e069d1c19ced965905bbc02680a.png)

![](img/e03a141e62de06c17a6b2c33496f1e90.png)

> 其实现在我们对于智能体的界定还比较模糊，大部分场景下，通过简单的提示词工程调教出的一个解决垂直问题的聊天机器人也会有人跟风叫智能体，但是像上面智能体结构部分介绍的，一个严格意义上的智能体应该具备更强的规划和反思能力（现在利用GPT基本能力其实通过提示词已经可以有不错的基线水准），最早的通用智能体尝试其实初具雏形，但是可能没有真正解决商业业务问题，我觉得主要还是提示工程带来的规划反思能力毕竟有限，另一方面是对于业务SOP的理解还是太有限，放在实际场景下就往往不太符合预期，也可以看到最近的一些智能体应用都在往垂直方向发展

### 通用智能体

在ChatGPT火爆的那段时间，有很多人坚信已经到了AGI出现的时刻，所以涌现了很多声称实现类似自主行动智能体的方案，很多方案都期望设计一套通用的框架模拟一个人类的决策执行能力，但是实际效果其实只能在部分场景达到不错的效果，但是对于这些尝试给智能体的理论研究以及后面专有领域的实现提供了很好的参考和指导作用

| [Auto-GPT](https://github.com/Significant-Gravitas/Auto-GPT) | [babyagi](https://github.com/yoheinakajima/babyagi) | [SuperAGI](https://github.com/TransformerOptimus/SuperAGI) |
| --- | --- | --- |
| &lt;https://user-images.githubusercontent.com/70048414/232352935-55c6bf7c-3958-406e-8610-0913475a0b05.mp4&gt; | ![](img/c8ca782dcbae7e42c4f0bda35bb79482.png) | &lt;https://user-images.githubusercontent.com/92881074/248220600-bb18407a-b6b2-4a9f-9419-ec73ffacd2f8.mp4&gt; |

+   可以快速生成小应用代码并且运行：https://github.com/muellerberndt/mini-agi

### 术业有专攻

下面的总结主要是个人的总结收集，还有一份英文语境下的总结，可以参考这篇 awesome-ai-agents

|  | 开源地址 | 效果演示 |
| --- | --- | --- |
|  | [FinGPT](https://github.com/AI4Finance-Foundation/FinGPT) | ![](img/2bb586cb8138ec8584238fc2731a9351.png) |
|  | [G](https://github.com/ShishirPatil/gorilla)itHub - ShishirPatil/gorilla: Gorilla: An API store for LLMs | ![](img/18dec106bb744e51f1c4a05f4ab9e1f8.png) |
|  | [PentestGPT](https://github.com/GreyDGL/PentestGPT) | ![](img/df560f4c9d3ad89af8589a066f571721.png) |
|  | [LaWGPT](https://github.com/pengxiao-song/LaWGPT) | ![](img/74fc1eb3551e11979dba30320847524d.png) |
|  | [DoctorGPT](https://github.com/llSourcell/DoctorGPT) | ![](img/2c357df021b370fc33a6c2b90f0790de.png) |
|  | [MedicalGPT](https://github.com/shibing624/MedicalGPT) | ![](img/4f64b0cc2d478061bb852a41bba68616.png) |
|  | [MediaGPT](https://github.com/IMOSR/MediaGPT) | ![](img/bc6ae1c247705118f6b490ecdda2e275.png) |
|  | [TransGPT](https://github.com/DUOMO/TransGPT) | ![](img/3b95791d9bc9e6b01925ed34f6fe371f.png) |
|  | [GitHub - blcuicall/ta](https://github.com/blcuicall/taoli)oli: &quot;桃李“: 国际中文教育大模型 | ![](img/19d3979fcdc073537b63f7ca575036f9.png) |
|  | [ChatGenTitle](https://github.com/WangRongsheng/ChatGenTitle) | ![](img/00198892c70ae6f107605a0e2d30715b.png) |
|  | 读文档GPT，这个不能算智能体，但是是智能体重要的知识来源，有很多的开源实现： | ![](img/c1e42c693900879e5f87d5bc9447df63.png) |
| 指令遵循 | [RealChar](https://github.com/Shaunwei/RealChar)：定制语音聊天AI伴侣，可克隆女神声音、医生声音，对标character.ai、Call Annie的开源版本 | &lt;https://user-images.githubusercontent.com/5101573/253765762-62a1f3d1-1166-4254-9119-97647be52c42.mp4&gt; |
|  | [VirtualWife](https://github.com/yakami129/VirtualWife)：基于chatgpt实现与观众互动的虚拟主播，目前支持在B站进行直播，用户可以自由更换VRM虚拟人物模型 | ![](img/f929c93c5211f87a25c04e7aca597e7b.png) |
|  | [DB-GPT](https://github.com/csunny/DB-GPT) | &lt;https://user-images.githubusercontent.com/13723926/252262602-55f31781-1d49-4757-b96e-7ef6d3dbcf80.mp4&gt; |
|  | [gpt_academic](https://github.com/binary-husky/gpt_academic) | ![](img/aeebb9c7d977f535c9fddedd64df9d60.png) |
|  | [gpt-migrate](https://github.com/0xpayne/gpt-migrate) | &lt;https://user-images.githubusercontent.com/25165841/250232917-bcc99ce8-99b7-4e3d-a653-f89e163ed825.mp4&gt; |
|  | https://github.com/smol-ai/developer | ![](img/78c5830865e8ac6242c909a80db699da.png) |
|  | [sweep](https://github.com/sweepai/sweep)：深度结合github流程，将bug报告和功能请求转换为代码更改 | &lt;https://user-images.githubusercontent.com/44910023/249586186-365ec29f-7317-40a7-9b5e-0af02f2b0e47.mp4&gt; |
|  | [aider](https://github.com/paul-gauthier/aider)：对话式生成代码、管理代码、修改代码，基于chatGPT的半自动化终端代码助手，相比gpt-engineer更灵活 | ![](img/2608d11554e6f4c63782c513f8d80451.png) |
|  | [XrayGPT](https://github.com/mbzuai-oryx/XrayGPT)：医疗X光胸片大模型辅助医疗咨询的ai助手，医生的好帮手 | ![](img/ad3c95d68a43a3ea9e59304cd7e7639c.png) |
|  | [chart-gpt](https://github.com/whoiskatrin/chart-gpt) | ![](img/5792011cb23c36eb5dcee1dd4141c51a.png) |
|  | ChatGPT-CodeReview | ![](img/15fa37209d6bd4b5558ebf4fb70a5309.png) |
|  | [chatppt](https://github.com/HuiMi24/chatppt) | ![](img/4a8d42d6ae380e2bd13cf9c542449e47.png) |
|  | [co-mate](https://github.com/archguard/co-mate) | 人工智能驱动的架构副驾驶、设计和治理工具 |
|  | [GirlfriendGPT](https://github.com/EniasCailliau/GirlfriendGPT) | ![](img/5adb412781afd1dd5ba4397e56060d0e.png) |
| 定制SOP | [gpt-engineer](https://github.com/AntonOsika/gpt-engineer) |  |
|  | [DevOpsGPT](https://github.com/kuafuai/DevOpsGPT) | ![](img/86d46d7a5cbc60e35a2b3a3f5e4a0976.png) |
|  | [gpt-pilot](https://github.com/Pythagora-io/gpt-pilot) | ![](img/6ec001e26bd52e5b886b3164b8da06c9.png) |
|  | [ShortGPT](https://github.com/RayVentura/ShortGPT) | &lt;https://user-images.githubusercontent.com/121462835/252747808-a802faad-0fd7-4fcb-aa82-6365c27ea5fe.mp4&gt; |
|  | [gpt-researcher](https://github.com/assafelovic/gpt-researcher) | &lt;https://user-images.githubusercontent.com/13554167/252265342-a00c89a6-a295-4dd0-b58d-098a31c40fda.mp4&gt; |
|  | [SalesGPT](https://github.com/filip-michalsky/SalesGPT) | ![](img/9a3ff1703c6da7cef22273262a26db2f.png) |

![](img/186f7bfeb110faf3c1fd262058274141.png)

[243695075-6e362e45-4a94-4b0d-973d-393a31d92d9b.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CvALbM5vyoxPPkxqzpUc8ejjnqg)

### 工作流应用

+   工程研发

> **思路：**尝试总结和抽象你现有的工作流，最基础在单个任务上尝试，结合GPT本身的能力限制找到最合适的prompt和场景；进一步在流程上串联，通过分屏工具配置布局组合工具，如果有编程能力（可以让GPT帮你编程）可以定制工作流工具

因为其他领域的工作流了解的比较少，这里仅以研发工作流举例来说明思路，比如其中一个开源的项目期望通过AI工具对于研发流程进行重构：

+   销售过程

下面其实是一个智能体的部分通用行为，简单的智能体基本都可以按照这个框架套，比如这个销售过程管理的行为简化来看其实就是一个阶段推进器，销售跟客户的互动就是这个阶段推进器的主循环过程，结合外部的知识库通过跟客户的高效沟通，推进商机的进展。这里面强化了人的弱项，主要可能是各种信息的记忆，不管是外部工具的知识库还是跟客户的历史对话细节， 再或者是对于阶段推进的驱动力等等。当然这个是属于很初级的销售行为，对于更重要的客户当然还有客户关系的维系，推进关联销售，对于大单子的议价沟通等（当然在有些点其实基于这个图也是可以扩展的，只是暂时还没有做那么重度）

![](img/9a3242697cc616d31bc0055a61d51914.png)

启动这个简单的销售代表，只需要提供必要的信息，从开源的代码中可以发现这样的智能体对于目标驱动的理解是非常直接的，他所不知道的主要是领域内的最佳实践，也就是custom prompt以及作者初步抽象出来的初级销售过程SOP，只要简单的销售过程是可以抽象总结，智能体就是可以实现或模仿学习，这也是LLM上层做智能体很大的优势

![](img/da5f8053f09355d55ffc7190fbdd46ba.png)

而近期了解到猎豹移动做的行业应用，据说也取得的不错的效果，结合实际的一些落地效果[感觉可以跟开源](https://chatmax.ai/)的实现方案有一些相互结合的启发，而据说开发这个灵销的，使用的是这个chatmax，看起来可能主要是对于私有数据进行了微调（也许只是知识库集成），提高了回复的准确性，再结合提示词引导出违和感不那么强的自然对话

![](img/6b424adb6950f8f2c3dc83eb433940c3.png)

## 多智能体协作

![](img/1f7bc7ac9ad7ef8292522608cc4d405a.png)

> 单个智能体是有并发上限的，而且对于单个智能体的描述很多时候难以在同一套话术体系或者模板下描述的准确，这样实现的单个智能体的效果就会显得不够专精，所以这也类比了人类社会中的协同。通过多智能体的协同可以解决单智能体无法解决的问题。比如下面是之前斯坦福和google研究院共同发布的一篇论文，论文中讨论了智能体为交互式应用程序创建可信的人类行为模拟。在这项工作中，他们通过填充一个沙盒环境来演示生成智能体，使用25个代理。用户可以观察和干预智能体计划他们的一天，分享新闻，建立关系，协调小组活动，最近项目也开源出来

![](img/8db99ed121b7c9ed6f5e3bfdb597bbd0.png)

类似的，最近看到另外有研究被称为 Solo-Performance-Prompting 的方法，该方法利用 LLM 中的多个角色模拟认知协同，提高问题解决能力的协作过程，从而在疑难问题解决方面达到更好的效果

![](img/4f7b6567dd16bc8f364e7959aef9dbd6.png)

### 社会行为模拟

其实这种社会行为暂时看可以分成两种：

+   人工干预的行为：更多用于基于目标的智能体协作，比如模拟一个软件公[司，公司内各种角色，](https://github.com/OpenBMB/AgentVerse)[包括产品，架构](https://github.com/a16z-infra/ai-town)师，设计，研发和测试，按照预设的SOP互为上下游输入和输出组成流水线一样的工作流程实现既定目标，除了 MetaGPT 的例子，还看到另外一个 AgentVerse，不过原理上稍微有些不同。最近著名的a16z也给出了ai-town 开发脚手架项目

![](img/7d26e15772e671e30e22ead2c4436afb.png)

+   基于自然的社会化过程的自然扩散行为：比较类似下面人工智能小镇的形式，智能体的协作是随机性的，整体世界的状态是有每个不确定因素影响的智能体状态，他们之间的交互以及跟环境交互产生的影响等这些综合因素决定的，任何一个微小的随机过程会产生。

在模拟开始时，一个代理被初始化，意图组织一个情人节派对。尽管在接下来的事件链中有许多可能的失败点——代理可能没有按照这个意图行动，可能忘记告诉别人，可能不记得出现——情人节派对实际上确实发生了，有许多代理聚集在一起并进行互动

![](img/99388969fa16ed518a771b145c897587.png)

+   带有标记的区域。根节点描述整个世界，子节点描述区域（例如房屋、咖啡馆、商店），叶节点描述对象（例如桌子、书架）。代理记住一个子图，该子图反映了他们所看到的世界的部分，并在他们观察这些部分时保持这些部分的状态

![](img/1795e6e6fcc2e54f01915da630bfee34.png)

+   智能体翰·林生活中的一个早晨。约翰早上6点左右醒来，完成他早上的例行公事，包括刷牙、洗澡和吃早餐。他短暂地赶上了妻子梅和儿子艾迪，然后开始了他的一天工作。

![](img/ecdd6ea4742d926e9c3c90bac99f1973.png)

+   记忆流包含大量与代理当前情况相关和不相关的观察结果。检索确定了这些观察结果的一个子集，这些观察结果应该传递给语言模型以调节其对情况的响应。再接着，根据Klaus Mueller的反射树，智能体对世界的观察，以叶节点为代表，递归合成以导出Klaus的自我概念，即他高度致力于他的研究

![](img/82d3f2021bfba7c93208fb0c57812356.png)

![](img/e384bfd21f31abc6562fc08181d179be.png)

+   伊莎贝拉·罗德里格斯情人节派对邀请的扩散路径包括总共12名特工，除了伊莎贝拉，她在模拟结束时听说了霍布斯咖啡馆的派对

![](img/8c437d247e474e4c521ee904c07f4622.png)

### 协作过程模拟

#### [ChatDev](https://github.com/OpenBMB/ChatDev)

通过扮演不同角色的各种智能代理运营，包括首席执行官、首席技术官、程序员、测试人员等。这些代理形成了一个多代理组织结构，并通过“通过编程彻底改变数字世界”的使命团结在一起。ChatDev中的代理通过参加专门的功能研讨会进行协作，包括设计、编码、测试和记录等任务。通过多个智能体协作，让用户可以使用自然语言创建定制软件。期望提供一个易于使用、高度可定制和可扩展的框架，也是研究集体智能的理想场景。

![](img/4f9859fb8025641d592e492f8bef7686.png)

<https://user-images.githubusercontent.com/11889052/263643733-80d01d2f-677b-4399-ad8b-f7af9bb62b72.mp4>

下面是通过使用ChatDev创建出来的小型定制软件

![](img/c6c1a5726341fb32c1a2affbf97b091a.png)

#### [Camel](https://github.com/camel-ai/camel)

这是一个基于论文的实验性项目，使用初始提示来引导聊天代理完成任务，同时保持与人类意图的一致性。展示了如何使用角色扮演来生成对话数据，以研究聊天代理的行为和能力，为研究对话语言模型提供宝贵的资源。通过引入一种新颖的通信代理框架，为研究多代理系统的协作行为和能力提供一种可扩展的方法，并开源相关代码以支持对通信代理及其他方面的研究。比如下面的例子，可以模拟python程序员和股票交易者之间的对话，合作开发股票市场的交易机器人

![](img/54200f486fc6452dfa3b93ea067b21ec.png)

#### [AutoAgents](https://github.com/LinkSoul-AI/AutoAgents)

基于LLM的自动代理生成实验的实验性开源应用程序。该程序由LLM驱动，自主生成多代理以实现您设定的任何目标。为GPT生成不同的角色，以形成用于复杂任务的协作实体。模拟我们日常通过IM协作的过程，通过问题的发起自动构建群聊，由manager 分析任务，拆分解决问题的步骤，给群聊拉适合解决问题的角色，给不同角色提出指令要求，不同角色给出自己的反馈和操作

![](img/3a2881c10496e1fa507ee891dbb3e8bf.png)

演示案例：

+   确认室温超导是不是谣言

<https://user-images.githubusercontent.com/1501158/262344887-41898e0d-4137-450c-ad9b-bfb9b8c1d27b.mp4>

+   实现一个贪吃蛇（感觉贪吃蛇已经成为了agent实验的 hello world）

<https://user-images.githubusercontent.com/1501158/262345107-97e408cb-b70d-4045-82ea-07319c085138.mp4>

#### [Brainstormgpt](https://brainstormgpt.ai/)

猎豹移动的项目，看起来主要是利用了协作的框架，但是单智能体应该主要以提示词工程来实现，因此虽然框架理念比较领先，但是场景上还是切容错率较高的头脑风暴，所以也说明效果可能不太理想

<https://brainstormgpt.ai/video/guide_video.mp4>

### 领域专家市场

其实这块跟其他从LLMOps做智能体市场的公司类似，只是概念上比较[容易扩展，因为国](https://nexus.snikpic.io/)外的 freelancer 市场比较成熟，所以有类似的思路比如 NexusGPT，如果领域专家的经验可以通过不同层次的调教（提示词，知识库，工具应用）让AI习得，就可以产生专家市场。这里跟 MetaGPT 的 AgentStore 也有所对应。不过做“市场”这个事情是不是一个伪需求，可能还尚待考证，之前关注 copilot hub 也是做智能体市场的目标，现在又重新聚焦回学生使用AI工具辅助这个定位了，所以如果要做这个事情，能力上可能还需更多基建才好支持，比如 MetaGPT 更完善的分层和抽象

![](img/6edd0a937410a0f5d7897eccd8341bdf.png)

![](img/66a28fd38a263b74c2ac0e686f0c2fdd.png)

## 面临挑战

+   **有限上下文长度：**受限的上下文容量限制了历史信息、详细指令、API调用上下文和响应的包含。系统的设计必须在这种有限的通信带宽下工作，而像自我反思这样从过去的错误中学习的机制将从长或无限的上下文窗口中受益匪浅。尽管向量存储和检索可以提供对更大知识库的访问，但它们的表示能力不如全神贯注那么强大。最近百川智能号称他们的模型已经可以达到192K（超过了之前100K记录的Claude），值得期待

+   **长期规划和任务分解的挑战：**长期规划和有效探索解决方案空间仍然具有挑战性。当面临意外错误时，LLM很难调整计划，这使得它们与从试错中学习的人类相比不那么稳健。其实个人感觉在这点上是可以借鉴强化学习的思路的，强化学习就是一个典型的试错迭代的系统，这也是alphago到alpha-zero进化的体现，如果智能体在这方面可以更好跟强化学习结合，相当于给智能体提供了自我进化的能力

+   **自然语言接口的可靠性：**当前的代理系统依赖自然语言作为LLM和外部组件（如内存和工具）之间的接口。然而，模型输出的可靠性值得怀疑，因为LLM可能会出现格式错误，偶尔会表现出叛逆行为（例如拒绝遵循指令）。因此，大部分代理演示代码都集中在解析模型输出上。而且像提示词工程这种东西为什么存在，其实也是可以进一步弥合自然语言作为接口的不可靠性。提示词相当于一个预制的模板，在场景明确的地方，可以更好实现意图到目标的转化

+   **智能体****效果评估：**因为智能体是面向垂直场景实现，垂直场景的差异化会比较大，如何更好评估这些特定场[景下智能体的能力和解](https://github.com/THUDM/AgentBench)决问题的效果，暂时还比较缺乏相对客观的评估标准。清华开源的 AgentBench 项目，虽然从目标上想要提供对于不同LLM作为智能体“大脑”的适用性评估，但是其在不同环境下的评估标准和设计思路其实对于垂直场景下智能体的评估也提供了比较好的启发和参考价值

![](img/95571b81c02f45a328d0605f79a5618d.png)

# 参考资料

+   [亿欧智库 | 2023中国AI商业落地投资价值研究报](https://www.iyiou.com/research/202307081241)告_新兴技术及应用行业研究报告-亿欧智库

+   内容行业的范式革命:AIGC行业应用展望_互联网_艾瑞网

+   AIGC产业研究报告2023——语言生成篇-易观分析

+   AIGC产业研究报告 2023——图像生成篇-易观分析

+   AIGC产业研究报告2023——视频生成篇-易观分析

+   AIGC产业研究报告 2023——音频生成篇-易观分析

+   AIGC产业研究报告2023——三维生成篇-易观分析

+   AIGC产业研究报告2023——分子发现与电路设计篇-易观分析

+   [Su](https://arxiv.org/pdf/2304.01852.pdf)mmary of ChatGPT/GPT-4 Research and Perspective Towards the Future of Large Language Models

+   [ChatGLM 的 Prompt 工程实践教程](https://lslfd0slxc.feishu.cn/docx/Nqm9dX81hotVYUxFQuxcVR82n2g)

+   一文看懂计算机视觉-CV（基本原理+2大挑战+8大任务+4个应用）

+   【CV第一篇】计算机视觉四大基本任务

+   [计算机视觉(CV)任务介绍：分类、检测、分](https://cloud.tencent.com/developer/article/1610516)割、超分、关键点识别、图像生成、度量学习

+   [AI绘画何以突飞猛进? 从历史到](https://mp.weixin.qq.com/s/LsJwRMSqPXSjSyhoNgzi6w)技术突破， 一文读懂火爆的AI绘画发展史

+   ControlNet原理解析 | 读论文 - 掘金

+   [AI绘画可控性研究与应用](https://mp.weixin.qq.com/s?__biz=MzIxOTczNjQ2OQ==&mid=2247484794&idx=1&sn=3556e5c467512953596237d71326be6e&chksm=97d7f580a0a07c968dedb02d0ca46a384643e38b51b871c7a4f89b38a04fb2056e084167be05&scene=21#wechat_redirect)

+   [AIGC行业深度：应用场景、商业](https://zhuanlan.zhihu.com/p/605072814)模式、市场规模、产业链及相关公司深度梳理

+   AI大模型行业深度：行业概述、发展现状、产业链及相关公司深度梳理

+   人工智能行业深度：行业格局、市场前景、产业链及相关企业深度梳理

+   AI＋行业深度：现状及趋势、涉及行业及相关公司深度梳理

+   传媒+AI行业深度：行业变革、市场前瞻、投资主线分析及重点公司梳理

+   AI+游戏行业深度：产业变革、驱动因素、产业链及相关企业深度梳理

+   AI算力行业深度：供需格局、产业发展前瞻、产业链及重点公司梳理

+   AI大模型行业深度：行业概述、发展现状、产业链及相关公司深度梳理

+   万字长文：AIGC技术与应用全解析

+   [万字长文：LLM应用构建全解析](https://zhuanlan.zhihu.com/p/633288551)

+   万字长文：LLM能力与应用全解析

+   [2023年AIGC应用与实践展望报告_澎](https://www.thepaper.cn/newsDetail_forward_22600113)湃号·湃客_澎湃新闻-The Paper

+   [浅谈一些AIGC赚钱赛道](https://zhuanlan.zhihu.com/p/569545118)

+   [I](https://jalammar.github.io/explaining-transformers/)nterfaces for Explaining Transformer Language Models

+   [H](https://www.lavivienpost.com/how-chatgpt-works-architecture-illustrated/)ow ChatGPT works - Architecture illustrated - La Vivien Post

+   Cohere Prompt Engineering

+   [A ](https://arxiv.org/abs/2303.04226)Comprehensive Survey of AI-Generated Content (AIGC): A History of Generative AI from GAN to ChatGPT

+   [理解DALL·E 2， Stable Diffus](https://zhuanlan.zhihu.com/p/589223078)ion和 Midjourney的工作原理

+   中国信通院-科研能力-权威发布-白皮书

+   [Gi](https://github.com/hwchase17/langchain)tHub - langchain-ai/langchain: ⚡ Building applications with LLMs through composability ⚡

+   《AI 研发提效研究：自己动手训练 LoRA》

+   MetaGPT作者深度解析直播回放_哔哩哔哩_bilibili

+   探讨AIGC的崛起历程，浅析其背后技术发展 - 掘金

+   [Pr](https://aws.amazon.com/cn/blogs/aws/preview-enable-foundation-models-to-complete-tasks-with-agents-for-amazon-bedrock/)eview – Enable Foundation Models to Complete Tasks With Agents for Amazon Bedrock | Amazon Web Ser

+   A list of AI autonomous agents

+   [Pa](https://github.com/xlang-ai/xlang-paper-reading)per collection on building and evaluating language model agents via executable language grounding

+   [Pap](https://github.com/Timothyxxx/EnvInteractiveLMPapers)er collections of methods that using language to interact with environment, including interact with real world, simulated world or WWW(🏄).

+   [Patterns for Building LLM-base](https://eugeneyan.com/writing/llm-patterns/)d Systems & Products

+   LLM-Agent-Paper-List

+   推荐：LLM Powered Autonomous Agents

+   推荐：前言 - 构筑大语言模型应用：应用开发与架构设计

+   推荐：The Illustrated Stable Diffusion