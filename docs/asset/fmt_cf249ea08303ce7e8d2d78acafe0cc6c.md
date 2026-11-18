
## 训练与正则化

### Label Smoothing
During training, we employed label smoothing of value   [30]. This hurts perplexity, as the model learns to be more unsure, but improves accuracy and BLEU score.

我们在训练过程中采用了三种类型的正则化。

### Residual Dropout
我们对每个子层的输出应用了dropout[27]，然后再将其添加到子层的输入并进行规范化处理。此外，我们对编码器和解码器堆栈中的嵌入和位置编码的总和进行剔除。对于基础模型，我们使用 的比率。

### 标签平滑
在训练中，我们采用了标签平滑值 [30]。这损害了迷惑性，因为模型学会了更多的不确定性，但提高了准确率和BLEU得分。

## 7 结论

In this work, we presented the Transformer, the first sequence transduction model based entirely on attention, replacing the recurrent layers most commonly used in encoder-decoder architectures with multi-headed self-attention.

在这项工作中，我们提出了Transformer，这是第一个完全基于注意力的序列转换模型，用多头的自注意力取代了编码器-解码器架构中最常用的递归层。

For translation tasks, the Transformer can be trained significantly faster than architectures based on recurrent or convolutional layers. On both WMT 2014 English-to-German and WMT 2014 English-to-French translation tasks, we achieve a new state of the art. In the former task our best model outperforms even all previously reported ensembles.

对于翻译任务，Transformer的训练速度明显快于基于递归或卷积层的架构。在WMT 2014英译德和WMT 2014英译法的翻译任务上，我们实现了新的技术状态。在前一项任务中，我们的最佳模型甚至超过了之前报道的所有组合。

We are excited about the future of attention-based models and plan to apply them to other tasks. We plan to extend the Transformer to problems involving input and output modalities other than text and to investigate local, restricted attention mechanisms to efficiently handle large inputs and outputs such as images, audio and video. Making generation less sequential is another research goal of ours.

我们对基于注意力的模型的未来感到兴奋，并计划将其应用于其他任务。我们计划将Transformer扩展到涉及文本以外的输入和输出模式的问题，并研究局部的、有限的注意力机制，以有效地处理大型输入和输出，如图像、音频和视频。使生成的顺序性降低是我们的另一个研究目标。
