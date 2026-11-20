
#### 42\. 为场景标注学习层级特征

Learning Hierarchical Features for Scene Labeling (2013)

作者：J. Donahue et al.

#### 43\. 为识别学习中层特征（2010），Y. Boureau（LeCun）

Learning mid-level features for recognition (2010)

作者：Y. Boureau (LeCun)

### 五、图片说明

#### 44\. 显示、注意以及说明：带有视觉注意模型的神经图像说明生成

Show, attend and tell: Neural image caption generation with visual attention (2015)

作者：K. Xu et al. (Bengio)

#### 45\. 显示和说明：一个神经图像说明生成器

Show and tell: A neural image caption generator (2015)

作者：O. Vinyals et al.

![](img/582e307dbf14fb3f2f77e647cd3bbaae.jpg)

摘要：在人工智能领域，自动描述一张图像的内容是一个基础问题，它涉及到了计算机视觉和自然语言处理的结合。在此论文中，我们提出了一个基于深度循环架构的生成模型，结合了如今在计算机视觉和机器翻译的最新进展，这个模型能够用于生成图像的自然描述。训练出的模型将最大化的接近给定训练图像的的描述语句。在数个数据集上的实验都显示出了这一模型仅仅从图像描述中学习到的准确率和语言流畅程度。我们的模型相当的准确，在质量和数量上我们都进行了验证。例如，当前最前沿的方法在 Pascal 数据集上的 BLEU-1 分数（分数越高越好）是 25，我们的方法达到了 59 分，可相比于人类的表现 69 分。同样，在 Flickr30K 上的表现从 56 分，改善到了 66 分；在 SBU 上的表现从 19 改善到了 28。最终，在最新发布的 COCO 数据集上，我们得到了 27.7 的 BLEU-4 分数，这是目前最前沿的水平。

#### 46\. 用于视觉识别和描述的长期循环卷积网络

Long-term recurrent convolutional networks for visual recognition and description (2015)

作者：J. Donahue et al.

#### 47\. 可生成图像说明的深度视觉-语义校准模型

Deep visual-semantic alignments for generating image descriptions (2015)

作者：A. Karpathy and L. Fei-Fei

### 六、视频 / 人类行为

#### 48\. 利用卷积神经网络进行大规模视频分类（2014）

Large-scale video classification with convolutional neural networks (2014)

作者：A. Karpathy et al. (FeiFei)

![](img/13f007cd6b4a9ac58ff78dfe6891280f.jpg)

摘要：在解决图像识别问题方面，卷积神经网络（CNNs）已被证实是一类功能强大的模型。在这些研究结论的鼓舞下，我们用 487 类、100 万个 YouTube 视频，经验评估了 CNNs 在大规模视频分类任务中的表现。我们研究了多个方法将 CNN 的连通性扩展到时间域来利用本地时空信息，并且认为，一种 multiresolution 凹架构有望加速训练速度。与基于强特征的基线相比较，我们最好的时空网络表现不仅有了显著提升（(55.3% 到 63.9%)），而且让人惊讶的是，与 单框模型相比较，表现也有了适度提升（59.3% 到 60.9%）。通过用 UCF-101 Action Recognition 数据组重新训练顶层，我们还进一步研究了最好模型的泛化情况，结果发现与 UCF-101 基线模型相比较，其表现从 43.9%提升到了 63.3%。

#### 49\. DeepPose：利用深度神经网络评估人类姿势

DeepPose: Human pose estimation via deep neural networks (2014)

作者：A. Toshev and C. Szegedy (Google)

#### 50\. 用于视频中动作识别的双流卷积网络

Two-stream convolutional networks for action recognition in videos (2014)

作者：K. Simonyan et al.

#### 51\. 使用可穿戴传感器对人类活动识别的调查

A survey on human activity recognition using wearable sensors (2013)

作者：O. Lara and M. Labrador

#### 52\. 用于人类动作识别的 3D 卷积神经网络

3D convolutional neural networks for human action recognition (2013)

作者：S. Ji et al.

#### 53\. 带有改进轨迹的动作识别

Action recognition with improved trajectories (2013)

作者：H. Wang and C. Schmid

#### 54\. 用独立子空间分析，学习用于动作识别的等级恒定的时空特征

Learning hierarchical invariant spatio-temporal features for action recognition with independent subspace analysis (2011)

作者：Q. Le et al

### 七、词嵌入

#### 55\. Glove: 用于词表征的全局向量

Glove: Global vectors for word representation (2014)

作者：J. Pennington et al.

#### 56\. 句子和文档的分布式表示

Distributed representations of sentences and documents (2014)

作者：Q. Le and T. Mikolov (Google)

![](img/4eaaa84369e6cb5a427b32c64a1cada8.jpg)

摘要：很多机器学习算法都需要将输入表示成一个固定长度的特征向量。对于文本学习来说，最常用的一种方法是用词袋模型来表示。不管词袋模型多么流行，其存在两个弱点：一是丢失了词序信息，二是忽略了词义。比如，「powerful」，「strong」和「Paris」都是同等地位的。本文，我们提出了一种称为段落向量的无监督学习算法，旨在从变长度的长文本中学习其固定长度的特征表示，比如句子，段落和文档。我们的算法通过预测文档中的词来将每一篇文档表示成一个稠密的向量。这种思路帮助我们的算法解决了词袋模型的缺点。实验结果表面段落向量算法比词袋模型和其他文本表示的模型都要出众。最后，我们在几个文本分类和情感分析任务上得到了最优结果。

#### 57\. 词、短语及其合成性的分布式表征

Distributed representations of words and phrases and their compositionality (2013)

作者：T. Mikolov et al. (Google)

#### 58\. 有效评估词在向量空间中的表征

Efficient estimation of word representations in vector space (2013)

作者：T. Mikolov et al. (Google)

#### 59\. 词表征：一个针对半监督学习的简单且通用的方法

Word representations: a simple and general method for semi-supervised learning (2010)

作者：J. Turian (Bengio)

### 八、机器翻译 / 问答

#### 60\. 面向全人工智能问答：一套必备的玩具任务

Towards ai-complete question answering: A set of prerequisite toy tasks (2015)

作者：J. Weston et al.

#### 61\. 通过共同学习对齐和翻译实现神经机器翻译

Neural machine translation by jointly learning to align and translate (2014)

作者：D. Bahdanau et al. (Bengio)

#### 62\. 利用神经网络进行序列到序列的学习

Sequence to sequence learning with neural networks (2014)

作者：I. Sutskever et al.

#### 63\. 用 RNN 编码——解码器学习短语表征，实现统计机器翻译

Learning phrase representations using RNN encoder-decoder for statistical machine translation (2014)

作者：K. Cho et al.(Bengio)

#### 64\. 一个为句子建模的卷积神经网络

A convolutional neural network for modelling sentences (2014)

作者：N. Kalchbrenner et al.

#### 65\. 用于句子分类的卷积神经网络

Convolutional neural networks for sentence classification (2014)

作者：Y. Kim

#### 66\. 斯坦福 coreNLP 自然语言处理工具

The stanford coreNLP natural language processing toolkit (2014)

作者：C. Manning et al.

#### 67\. 基于情感树库应用于情感组合研究的递归深度网络模型

Recursive deep models for semantic compositionality over a sentiment treebank (2013)

作者：R. Socher et al.

![](img/eb3f9f881f65244789a2ead25881d372.jpg)

摘要：词语义空间虽然非常有用但无法准确地表达一个短语的意思。类似于情感检测的组合性任务需要更丰富的有监督训练和评价资源和更有力的组合模型。为了解决这个问题，我么提出了情感树库。库中包括含了 11855 个句子解析树中的 215154 个细粒度情感短语标签，对于情感组合提出了新的挑战。为此我们提出了递归神经张量网络模型。采用树库进行训练，本文模型在很多指标上比所有前人的模型都出众。将单句的正负情绪分类准确率从 80%提升到了 85.4%。对于预测细粒度的情感标签准确率提升到了 80.7%，相比于特征袋模型提升了 9.7 个百分点。最后，本文模型是唯一一个可以准确地捕捉否定词影响的模型，并且判断范围不管是正面短语还是负面短语都可以到多种树级别。

#### 68\. 几乎从头开始的自然语言处理

Natural language processing (almost) from scratch (2011)

作者：R. Collobert et al.

#### 69\. 基于语言模型的循环神经网络

Recurrent neural network based language model (2010)

作者：T. Mikolov et al.

### 九、语音/及其他

#### 70\. 自动语音识别：一种深度学习的方法

Automatic Speech Recognition - A Deep Learning Approach (Book, 2015)

作者：D. Yu and L. Deng (Microsoft)

#### 71\. 使用深度循环网络进行语音识别

Speech recognition with deep recurrent neural networks (2013)

作者：A. Graves (Hinton)

#### 72\. 用于语音识别中声学建模的深度神经网络：四个研究小组的观点分享

Deep neural networks for acoustic modeling in speech recognition: The shared views of four research groups (2012)

作者：G. Hinton et al.

![](img/09c80de1ee59cf1510cd59da35fdec26.jpg)

摘要：大多数当前的语音识别系统都使用隐马尔科夫模型（HMMs）来解决语音中的时间变化问题，用混合高斯模型（GMMs）来评价每一个 HMM 拟合声音输入表示帧或者小窗口帧系数的效果。存在一种替代评价方法是使用前馈神经网络来将多个帧系数作为输入，将 HMM 状态的后验概率作为输出。深度神经网络有很多隐藏层，通过新的方法进行训练，在很多语音识别任务上都比 GMM 模型更加出众，有时甚至会好非常多。本文将会做一个综述，分别对四家研究机构在最近语音识别的声学建模领域取得的成功进行介绍。

#### 73\. 基于上下文预训练的深度神经网络在大规模词表语音识别中的应用

Context-dependent pre-trained deep neural networks for large-vocabulary speech recognition (2012)

作者：G. Dahl et al.

#### 74\. 使用深度信念网络进行声学建模

Acoustic modeling using deep belief networks (2012)

作者：A. Mohamed et al. (Hinton)

### 十、强化学习/机器人

#### 75\. 通过深度神经网络和树搜索来掌控围棋游戏

Mastering the game of Go with deep neural networks and tree search (2016)

作者：D. Silver et al. (DeepMind)

摘要：围棋被视为人工智能挑战经典游戏中最难的一个，因为其巨大的搜索空间和对位置和移动的评价难度。本文提出了一种新方法使用“值网络”来评价位置，用“策略网络”来选择移动。这些深度神经网络是从人类专家棋局中进行有监督学习，然后在从自对弈中进行强化学习。如果不考虑前向搜索的话，当前最好的神经网路模型是蒙特卡洛树搜索，这种方法通过进行上千局的自对弈来进行仿真。我们也介绍了一种新点的搜索算法，将蒙特卡洛仿真与值网络和策略网络进行了综合。使用这种搜索算法，我们的项目 AlphaGo 有 99.8%的胜率，并且以 5：0 的比分打败了来自欧洲的人类冠军。这也是计算机第一次在真实围棋比赛中几百人类专业选手，将 10 年后的目标提前完成了。

#### 76\. 通过深度强化学习实现人类水平控制

Human-level control through deep reinforcement learning (2015)

作者：V. Mnih et al. (DeepMind)

#### 77\. 侦测机器人抓取的深度学习

Deep learning for detecting robotic grasps (2015)

作者：I. Lenz et al.

#### 78\. 用强化学习玩雅达利游戏

Playing atari with deep reinforcement learning (2013)

作者：V. Mnih et al. (DeepMind)

### 十一、无监督

#### 79\. 生成对抗网络

Generative adversarial nets (2014)

作者：I. Goodfellow et al. (Bengio)

![](img/58db5269ad5df7ab4b65c5a7743dd30c.jpg)

摘要：我们提出了一种通过对抗过程来评判生成模型的新框架，这里包括两个模型：一个是生成模型 `G`，用来学习数据分布，另一个是评判模型 `D`，用来计算一个样本来自于训练数据而不是 `G` 的概率。训练 `G` 模型的过程是最大化 `D` 模型犯错的概率。这个框架相当于一个小型的两人游戏。在任意的 `G` 和 `D` 函数空间中，存在一个唯一的解，使得 `G` 恢复训练数据的分布并且 `D` 得出的概率是 0.5。这里，`G` 和 `D` 均被定义为多层感知机，整个系统用向后传播误差的方法来进行训练。在整个训练过程和样本生成过程中都没有用到任何的马尔科夫链或者是展开近似推理网络。通过定性分析和生成样本评价的定量分析证明了本框架的潜力。

#### 80\. 自编码变量贝叶斯

Auto-Encoding Variational Bayes (2013)

作者：D. Kingma and M. Welling

#### 81\. 用大规模无监督学习搭建高水平特征

Building high-level features using large scale unsupervised learning (2013)

作者：Q. Le et al.

#### 82\. 无监督特征学习中单层网络分析

An analysis of single-layer networks in unsupervised feature learning (2011)

作者：A. Coates et al.

#### 83\. 堆栈降噪解码器：在本地降噪标准的深度网络中学习有用的表征
