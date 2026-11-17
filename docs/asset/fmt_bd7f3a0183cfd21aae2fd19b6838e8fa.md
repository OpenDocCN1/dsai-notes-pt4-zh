
# AIGC的基本技术

> 来源：[https://docs.feishu.cn/docx/XPvkdXoUGo2Mcmx343McZ7pZnZ2](https://docs.feishu.cn/docx/XPvkdXoUGo2Mcmx343McZ7pZnZ2)

以下是AIGC领域的一些基本技术：

**1）生成对抗网络**（GAN, Generative Adversarial Networks）：GAN是由Ian Goodfellow等人于2014年提出的一种生成式模型。它由生成器（Generator）和判别器（Discriminator）两部分组成。生成器负责生成新的数据，判别器负责判断生成数据与真实数据的相似程度。通过不断的训练，生成器能够生成越来越逼真的数据。

**2）变分自编码器**（VAE, Variational Autoencoders）：VAE是一种基于概率生成模型的生成式方法，它通过编码器（Encoder）和解码器（Decoder）两部分来实现数据的生成。编码器负责将输入数据映射到潜在空间中的一个分布，解码器负责从潜在空间中的分布采样数据并生成新的数据。

**3）循环神经网络**（RNN, Recurrent Neural Networks）：RNN是一种用于处理序列数据的神经网络结构。RNN具有记忆功能，能够捕捉序列数据中的时序信息。在生成式人工智能中，RNN可以用于生成文本、音乐等序列数据。

**4）Transformer模型**：Transformer是一种基于自注意力（Self-Attention）机制的神经网络结构，它在自然语言处理领域取得了显著的成果。Transformer模型可以用于生成式任务，如文本生成、机器翻译等。

其他相关技术：除了上述技术外，还有一些其他的生成式模型和技术，如PixelRNN、PixelCNN、WaveNet等。这些技术在图像和音频生成等领域也取得了一定的成果。
