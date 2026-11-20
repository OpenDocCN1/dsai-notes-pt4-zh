
![](img/dfc7e8dbf05afdb3c49658869fd6ef00.png)

+   然后output dream queries将会作为SD的condition进行图像生成，此时采用噪声预测loss来监督condition的质量

![](img/fa247b9049c68465407be231d7216e06.png)

可以看到为了提升理解任务上的性能，后续的方案大多都重新换回了continuous tokenizer，同样因为这个原因，此时image token的生成就不再是next-token prediction了，而是依赖于一系列预定义好的learnable query作为输出，并行理解并输出得到生成模型的condition，此时甚至都有点偏离了visual autoregressive的定义，更像是visual image autoregressive，为了更清楚展示一些发展的脉络，因此还是把SEED-X和DreamLLM放在了此处进行介绍

# Native
可以看到上面我们提到的3个方案都还是需要额外的生成模型例如Stable Diffusion介入，而且LLM输出的image token都是作为condition甚至不能说是真的“image token”，更像是经过LLM处理后对齐的text feature，不能说是LLM真的在生成图片，因此我称上面的为“Non-native”非原生生成，而下述的两个方案，就偏向于原生的生成，无需下游生成模型

## Lumina-mGPT
> Unified IO、Unified IO2、Chameleon是我们在上一章所谈过的三个多模态基座模型，旨在通过一个统一的transformer完成对各个模态的encode和decode，完成各种各样的任务，其中就包括图像生成，它们都是以next-token prediction的形式生成image token并且直接通过vocabulary来decode image token完成生成。在上一篇中，我们比较详细地介绍了Unified IO、Unified IO2、Chameleon，此处不再赘述

> Unified IO、Unified IO2、Chameleon并没有强调自己是为了增强图像生成能力，而ANOLE和Lumina-mGPT主要是进一步增强这些unified transform的图像生成能力，希望能够优化生成图像的质量和灵活性（更多分辨率，更多长宽比）。具体来说，Lumina-mGPT直接采用Chameleon预训练好的模型，然后进行了采用了10M的高质量数据对图像生成任务进行额外的学习，同时为了实现任意分辨率、长宽比的生成加入了以下techs：

+   在image embedding中插入了用来表示height/width的预定义指示符
+   图片每行token embedding末尾会插入一个`<end-of-line>`的特殊token
+   渐进分辨率文生图监督训练：
+   预定义了三个不同分辨率阶段：512、768、1024，每个阶段面积基本接近预定义值，但是长宽比有所差异
+   为了让用户之后可以自定义要生成图片的分辨率，每个训练样本会插入一个resolution-aware prompt
+   该过程还会额外插入OpenHermess中text-only数据和Mini-Gemini中image-text pair数据防止灾难性遗忘

![](img/4ff011c06eaef782d11ba09b4903ca24.png)

## ANOLE
目前开源的chameleon并不支持除text外的其他模态生成，而ANOLE提出一个简单的数据和参数高效的方式去对chameleon进行微调，以实现不妥协文本生成和其他模态理解的情况下，拥有多模态生成能力。ANOLE是基于chameleon的，因此具有Native（原生）多模态模型、完全基于token无需diffusion介入生成过程的优点，是为了解决开源的chameleon没有开放图文交替生成的问题

具体来说：ANOLE保持和chameleon结构一致，并且冻结chameleon大部分预训练权重，只微调`lm_head`中和image token idx预测相关的部分权重，微调数据集来自于LAION-5B art中5859张图片

![](img/043b3cf84d281d79ea3112f7fd810ecb.png)

# Visual Scale Autoregressive
+   visual autoregressive这种建模方式对于图像来说似乎不是一种很自然的方式，图像是一个2D的输入数据，visual autoregressive是直接将其拉成1D进行建模，并且这种建模顺序也没有特定的说法，默认都是从左上角按顺序逐个生成直到右下角，但直觉上从外向内或者从内向外螺旋逐个生成也是合理的

+   Visual Scale Autoregressive提供了一种更自然地建模图像的方式，每次autoregressive生成的图像token不再代表生成图像某个位置的patch feature，而是代表该图片在某个分辨率下的生成效果。因此Visual Scale Autoregressive在每个预定的分辨率下会并行的生成多个token，需要一个多尺度的VQ-VAE保证各个分辨率下都能重建出图片

![](img/0a54d00c541288486a2920fbf61ebc40.png)

## VAR
+   而VAR就是Visual Scale Autoregressive的开山之作，具体来说，VAR会训练一个具有K个尺度的VQVAE，一张图片通过这样multi-scale VQVAE就可以得到K个Token map，每个Token map具有不同的分辨率

+   每次自回归的进行是通过输入前`k-1`个token map拼接后的结果来预测第`k`个尺度下的token map。也就是说从原来输入逐个token拼接预测得到单独的下一个token，变成了每次自回归输入拼接上前一个尺度的token map，然后预测得到下一个分辨率的token map，token map中的  个tokens将会被并行预测出来

![](img/9476d32d7bfe6fb0bfeba51b1198f93c.png)

> 对于该multi-scale VQVAE的构造，作者采用了残差设计的思想，而不是每个尺度单独重建恢复原图，也就是说原图的特征会被分解成多个尺度量化后token的组合。具体流程如下：

+   将原来image continuous feature离散化成对应第 k 个尺度下的 hk×wk 的token map
+   对该token map进行插值和变换，对齐原始image feature的空间分辨率大小 H×W
+   从image feature中减去该尺度插值后的token map，表示已经重建恢复一定部分内容，剩下待重建的内容作为下一个尺度重建内容的目标

> 此时VQ-GAN进行decode重建时，也需要利用残差的思想，将所有尺度的重建结果进行组合

+   对每个尺度的token map进行插值和变换，对齐原始image feature的分辨率
+   累积该token map重建的部分

通过上述quantize + dequantize过程可以完成图像的重建，以实现multi-scale VQVAE的训练。由于采用的discrete image tokenizer，因此可以使用CE loss的方式以实现VAR transformer的训练

上述过程随之而来一个问题就是，当生成图片的时候，一定需要所有尺度都生成完成，才可以完成最后输出图片的累积吗？例如一开始只有一个`<boi>`，然后生成1x1 token map再生成2x2 token map，再生成3x3 token map，再生成5x5 token map，最后组合前面四个结果得到最终生成的图？如果是的话，那推理是怎么控制一次并行输出多个scale token的？

我们对应到`def autoregressive_infer_cfg()`用于自回归代码部分可以发现，确实是需要逐个scale自回归生成，但并不是像训练一样，每次尺度的token map预测都需要`concat`上之前尺度的预测，而是对上一个尺度输出token map插值到下一次scale生成对应所需要的分辨率，此时每次transformer的输入仅仅只有当前需要产生的尺度，类似于将上一个尺度插值得到的粗糙结果进行清晰化

```
f_hat, next_token_map = self.vae_quant_proxy[0].get_next_autoregressive_input(si, len(self.patch_nums), f_hat, h_BChw) 
    # ===================== get_next_autoregressive_input: only used in VAR inference, for getting next step's input =====================def get_next_autoregressive_input(self, si: int, SN: int, f_hat: torch.Tensor, h_BChw: torch.Tensor) -> Tuple[Optional[torch.Tensor], torch.Tensor]: # only used in VAR inferenceHW = self.v_patch_nums[-1]if si != SN-1:h = self.quant_resi[si/(SN-1)](F.interpolate(h_BChw, size=(HW, HW), mode='bicubic'))     # conv after upsamplef_hat.add_(h)return f_hat, F.interpolate(f_hat, size=(self.v_patch_nums[si+1], self.v_patch_nums[si+1]), mode='area')else:h = self.quant_resi[si/(SN-1)](h_BChw)f_hat.add_(h)return f_hat, f_hat
```

实验上主要做的class-conditional synthesis，以class embedding作为`[s]` start token放在序列最开始，用以引导后面各个尺度的生成，并且会用`AdaNorm`对class condition进行增强，还会对`query`、`keys`进行norm来稳定训练，代码中其实还有很多细节在文中没有被强调，感兴趣的可以去阅读源码

## STAR
STAR是将上述工作从class-condition拓展到text-condition，以实现利用文本定制化生产图片的效果

VAR方法使用`AdaNorm`来注入条件信息对于文本这种具有丰富信息的输入来说太过简单，因此提出了用一个额外的clip text encoder来提取文本表征作为控制信息，并通过额外的cross attention注入到transformer中。为了进一步提升预测过程图像内部的结构性，提出使用2D的`RoPE`来取代VAR中的learnable absolute position embedding

![](img/d2c3c5d7d1119ce72cf75270714df4cf.png)

> 具体来说，CLIP text encoder最后提取出来的text prompt embedding会被pooling产生一个single vector，并作为visual scale autoregressive的`[s]` start token。起始的1 × 1 token缺乏传达足够的文本细节的能力，并且在随后的尺度中存在被遮蔽的风险，因此提出了在self attention和mlp之间插入额外的cross attention模块来向每个scale的预测中引入条件信息

> 要点：

+   以`[s]` start token作为预测起点，主要决定了输出内容的全局信息
+   以scale-wise的形式，每次自回归的预测一个固定分辨率大小的token map

# 总结
Q1：Visual Autoregressive的多模态生成范式变化

A1：从MLLM并行输出作为condition变化到使用离散化的图像tokenizer使得MLLM能够统一生成和理解任务。离散化使得MLLM可以实现用CE loss来监督并且带来了抛弃下游生成模型的可能性，但是通常会带来理解任务性能的大幅降低。采用continuous tokenizer来融合图像的生成，可以缓解理解任务性能的下降，但是生成的监督信号需要更多的设计思考，并且似乎无法摆脱下游生成模型的引入。visual scale autoregressive更多是纯纯在做生成，希望能用更好的scaling性质来取代diffusion的位置。感觉问题归根结底在于：到底图像怎么样用生成实现理解的效果，目前看起来任重而道远
