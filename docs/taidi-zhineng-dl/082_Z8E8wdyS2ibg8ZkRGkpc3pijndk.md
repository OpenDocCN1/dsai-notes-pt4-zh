# Positional encoding

> 来源：[https://learning.feishu.cn/docx/E1WzdKmu0ojUybxyWm6cTia9nSb](https://learning.feishu.cn/docx/E1WzdKmu0ojUybxyWm6cTia9nSb)

https://kazemnejad.com/blog/transformer_architecture_positional_encoding

https://kexue.fm/archives/8130 让研究人员绞尽脑汁的Transformer位置编码 2021.02.03

Q K 最终算出来一个 score，加上 positional embedding 也不会比信息带到下一层。

但，如果 positional embedding 加到了 v 上，则会带入下一层。

不同于RNN、CNN等模型，对于Transformer模型来说，位置编码的加入是必不可少的，因为纯粹的Attention模块是无法捕捉输入顺序的，即无法区分不同位置的Token。为此我们大体有两个选择：1、想办法将位置信息融入到输入中，这构成了绝对位置编码的一般做法；2、想办法微调一下Attention结构，使得它有能力分辨不同位置的Token，这构成了相对位置编码的一般做法。

![](img/7c3b003660438427cac78241d702dd55.png)

![](img/07f540b3733974ac3d8f980dd29ff099.png)

![](img/b95c302435ef59cc968afd0000ec26b3.png)

# Positional embeddings

https://www.youtube.com/watch?v=1biZfFLPRSY

Positional embeddings in transformers EXPLAINED | Demystifying positional encodings.

![](img/81470dec70bbece377486dd3aeb71245.png)

![](img/05eb70998134dd1835fba4a8ceabffc4.png)

![](img/853b5996dc26c08c4f639e7843eab24c.png)

![](img/3aa1c7d741ef6a799cf2ee95ead8f909.png)

![](img/8c268a35f7a8f47602f04a071a99a75a.png)

2 个设计要求：

1.  相同 position 的 positional encoding 要相同。不管 token 是否相同。

1.  Positional encoding 的值与 token embedding 不能相差太多，否则在 embedding space 里的偏移太大，语义改变太多。

![](img/e6c07696caa1ea047552bb4435cef9f2.png)

问题是取值太大，破坏了 约束 2

最大值应该是 bounded 的。

试试 sin/cos

![](img/d782e52c5dfd7828628cdf0e23b8980b.png)

周期的，但只要周期足够大，就不会破坏 1。

sigmoid

![](img/cb91222c92a9cc45c6312d3f56a1caf2.png)

![](img/1fae2070704434c2729072aa84aea3e8.png)

![](img/c5c90438fb13b3090e595be8a991affb.png)

1.  Last dim：体现 token 的绝对位置，但，分辨率比较低。临近的 2 个 token 很难区分开。

1.  倒数第 2 个 dim：分辨率提高了。

1.  往前走：分辨率逐渐提高了。

![](img/4229ddeeca6678c44d4555c95935e39f.png)

![](img/a98fee7888f1d83fc611f30eebceafea.png)

![](img/da16591941dab96837a169b46680cb05.png)

![](img/9856e13a3209418ea62d6689320524aa.png)

![](img/47d7a69a253bcd93f24ee569c932424e.png)

![](img/e8ae7340a95bba20fca9534523d7ff8b.png)

# **Adding vs. concatenating positional e**mbeddings & Learned positional encodings

https://www.youtube.com/watch?v=M2ToEXF6Olw

When to add and when to concatenate positional embeddings? What are arguments for learning positional encodings? When to hand-craft them

Wang, Yu-An, and [Yun-Nung Chen. "](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbkRuRnpKMWMxTEFuaThOZnpGWjhtUFhkWU81Z3xBQ3Jtc0treHpuN0cwNUpzdTdwY09lUTdvZzZ4Tk1mOUUwbnJTd3hteWk1RzIzZkFoY2syTnZJT0FLQ091NmNncVB6alFYbWIwajlmNHU0ODNXU01FMW5Zc2phR3BVLWZmMzExSDNLRko0NC10TU1LSHdhZTlpcw&q=https%3A%2F%2Farxiv.org%2Fpdf%2F2010.04903.pdf&v=M2ToEXF6Olw)What do position embeddings learn? an empirical study of pre-trained language model positional encoding." arXiv preprint arXiv:2010.04903 (2020). https://arxiv.org/pdf/2010.04903.pdf

![](img/db369861e7cceac11d8d1a477b7d4652.png)

细节：Queen 的 positional embedding 都一样。

![](img/b6e08cf4274faf79a46f2805d6aed6ff.png)

![](img/09c87857676007295023afde30d90c70.png)

Concat 的方案

![](img/ec69afe6633d93e622c4afff2d899679.png)

Concat 的 cost 太大了。

![](img/8c51aa7f024cfda821f23d21c7cd5390.png)

# Learned positional encoding

![](img/14a500a00e55537d8a797fb7fc813c7b.png)

Bert 和 Roberta 这种 masked LM 模型，学到的结构化 positional 信息比较少。

GPT2 对 positional 信息的结构化学的更多。因为，必须从左到右完成序列的建模。

![](img/7927e1693d9ea0564c4e8af50127395e.png)

学到的 positional encoding 信息，并没有比 hand craft 手工设计 的版本更好。

![](img/870295bf593d5cc36dcc2421511450a3.png)

![](img/aa087f0d890a8c42bee3cc19a2d51f19.png)

![](img/36956c1d41c8bdc6b0d91145009aa68b.png)

![](img/ab2ad99a73dcf6b054ff37f0bd43dd88.png)

# Relative Position Representations

https://www.youtube.com/watch?v=DwaBQbqh5aE

Self-Attention with Relative Position Representations – Paper explained

Shaw, Peter, Jakob Uszkoreit, and Ashish Vaswani. "Se[lf-Attention wit](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbEpMOXBOVVNpX0x5bW1Zb3BZd0xfNFNrYlhGd3xBQ3Jtc0trX2pESXhGd1hYS0ZHTmhLb3BYU0RRNzFPOFZoY3FLNmZLSWlMMGhqaW1oM0ttVlhaR1RFYkZQMWx0N29nMWNLWUxEbExUQ2hjcW9WUU4xNDhXUkdYa3BFZU9tWDZKbHFjcHhhV01veWs5WFRyWlpnYw&q=https%3A%2F%2Farxiv.org%2Fpdf%2F1803.02155.pdf&v=DwaBQbqh5aE)h Relative Position Representations." In Proceedings of the 2018 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies, Volume 2 (Short Papers), pp. 464-468. 2018. https://arxiv.org/pdf/1803.02155.pdf

![](img/3de8f234175a7766fdcf1f48f211e290.png)

![](img/95c23984d85fb8ffad1d9c3fc73b926f.png)

classical

![](img/018859c8aca29c8d3069ed32806a5058.png)

Relative

![](img/899386ddd0af2e5491022c1da41ef22b.png)

![](img/fcd7b56cc5f3a47e1161b4e017afc994.png)

5 个 token，一共 9 个 relative positional encoding。从 w-4 到 w4

可以 handcraft 或者 learned

![](img/61c3711c137f6dae3a5b5a516742faf3.png)

![](img/1be16eb8f35c4923a6a6c61f7100b9f5.png)

![](img/c7e0a39ec844de9d705ceffe9bc3e3dc.png)

![](img/605eefb021bd01c2e21790b00beac503.png)

Self-attention 的思路，包括加入 positional encoding 的信息，

特别是 GAN 里面的 latent space edit 问题的 MLP 解决方案。

![](img/061c12d5e04ae7f2dda87a1f97428fd6.png)

![](img/50e51a86a32f6702fd9c1b0c828514a1.png)

![](img/98d38c0aee989d2f58db9f69a8d91c11.png)

![](img/afdd1c34aae01970a91ef2bcbc313fb1.png)

改了 K 和 V，没改 Q。

![](img/0c228d6d9e63dc84a52b16faf3911ad5.png)

![](img/0674770d6c4fed1c58737870e4eba96f.png)

![](img/b0c50d8694db1948e71762d6520f913d.png)

Graph 上也能用。

![](img/749e55b71fde0b9596d29a401c0df5ca.png)

![](img/8ebb1a6632591c3b2c3e9263aff99ea1.png)

Clip at k, generalizes to any seq length.

Handcraft 也能，但不是所有 learned 都可以泛化到所有 seq length。

![](img/5950742a8af65b70d2b431788a91e693.png)

# Relative position - v2

![](img/2605a0cd6acbdfe9efbe2c2d6ba6acfe.png)

# LM-Infinite

https://arxiv.org/abs/2308.16137

![](img/e5f21e2c34992bb66b82f2a54b8b630f.png)

# RoPE position embedding

https://www.youtube.com/watch?v=o29P0Kpobz0

旋转位置编码RoPE (Rotary Position Embedding) 是目前大模型中广泛使用的一种位置编码，包括但不限于Llama、Baichuan、ChatGLM、Qwen等

RoFormer: Enhanced Transformer with Rotary Position Embedding (main paper that proposes RoPE embeddings): https://arxiv.org/abs/2104.09864

这是一种配合Attention机制能达到“绝对位置编码的方式实现相对位置编码”的设计。而也正因为这种设计，它还是目前唯一一种可用于线性Attention的相对位置编码。

![](img/d9b678e08fab50e016ed793cde038100.png)

![](img/c7d86195f6accbbc2dee034debf3e997.png)

![](img/c0c6f8b068bb6f5ab500861bac75af97.png)

![](img/efc348330bccf25293f70c3e0d3997f6.png)

2d case

![](img/0104dacf7df1bc5b10e4ff9d7bf4edc4.png)

一般情况的公式，超过 2d 的

![](img/3bab445a356a1e41f7f2af5154900fda.png)

实现的时候，不会用矩阵乘。因为，构造矩阵 太慢了。

![](img/451eff36d21d03d9758c8153c08415e3.png)

![](img/165639d7882ab3f193082d512b60438b.png)

![](img/ed2db9111679ec03a163061d4d0bad01.png)

![](img/9752f2b330780f02e9e18993944266f1.png)

![](img/19a5dca968d2f4c94272d4d6b0a819e0.png)

![](img/1ccbe188e7d4de32f71824fd787b5083.png)

![](img/7b6d3178fe8b1d9142e7656a8302503b.png)

Using interpolation for RoPE: [https://arxiv.or](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbHlzMmFFeXFacWZGRkdXZ3cxVWYxNEZjb0Fid3xBQ3Jtc0tsY2pCQWtoQ0swVW1IOWRTemFXRHZuM3NpOTVyMm0ydVVKTFBYS3hHVG9rNlRlQnE4RzBVc1lPX3FXM1Q0bkl6bDBUZFpPSTRnblI1R0ZqLUNvcUp2TEdEekgwcUIxWHJHbDZnY0dCQ0NDbHZQT01MMA&q=https%3A%2F%2Farxiv.org%2Fpdf%2F2306.15595.pdf&v=GQPOtyITy54)g/pdf/2306.15595.pdf

公式的推导过程

![](img/b9370717e6ccaef2e366114a36ce7179.png)

![](img/22ca79a24707881b3c24a6bfd7bff84c.png)

# **ALiBi**

https://www.youtube.com/watch?v=-Kgxv64aG3o

ALiBi - Train Short, Test Long: Attention with linear biases enables input length extrapolation

https://arxiv.org/abs/2108.12409

Train Short, Test Long: Attention with Linear Biases Enables Input Length Extrapolation

长度外推性的工作

没有需要学习的参数

![](img/817c1a21a640dfebbf86d075a75ec8ef.png)

![](img/3b3e0159082bd0a82ac1ba3023ec6814.png)

这个工作，再进一步，很自然的就能想到 田渊栋 H2O streaming KV cache 的工作。

# RoPE 的长度外推

https://mp.weixin.qq.com/s/RtI95hu-ZLxGkdGuNIkERQ

包括线性插值、NTK-Aware插值、NTK-by-parts、Dynamic NTK、YaRN等