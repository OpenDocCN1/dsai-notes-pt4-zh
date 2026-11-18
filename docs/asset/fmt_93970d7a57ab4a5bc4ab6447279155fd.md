
# Task2：RNN建模SMILES进行反应产率预测

> 来源：[https://datawhaler.feishu.cn/docx/Q3ifd4obWoJdrExdLp0ctT4Wnjd](https://datawhaler.feishu.cn/docx/Q3ifd4obWoJdrExdLp0ctT4Wnjd)

> **教程贡献者说：**

> 前面我们用`RDkit`工具将SMILES表示的分子转化为了`Morgan Fingerprint`（向量），然后使用随机森林对数据进行建模得到了我们的第一个成绩。

> 在Task2中，我们的任务是进一步了解AI4Science相关知识，然后使用深度学习的方法建模。

> 你可以从中：了解一些相关历史、了解SMILES和分子指纹，并对`RDkit`工具包有更深入认识；探究深度学习方法如何建模化学中的序列问题。

> 其中，使用RNN建模SMILES进行产率预测是重要环节，大家一定要好好学习！！！

# 引导思考

## 如何将化学知识转化为计算机可识别的形式

当我们谈论到化学这门学科的时候，大家应该都会情不自禁地想起元素周期表，分子式，化学反应方程，以及熵、焓、自由能等等复杂且繁多地化学知识。同物理、生物等传统学科一样，当前化学这门学科的知识和内容主要都是用文本和化学领域专有符号记录的，这些符号之中的意义计算机无法直观地理解。但是，其实随着计算机技术的发展，将化学知识用计算机存储的方法也一直在发展和进步，只是这些内容并未像GPT等在文本领域创造了惊天动地的成就，所以往往不为人们所知。

# 正文内容——Ai4Chemistry知识点补充

## AI4Science的早期历史（概要）

AI4Science的发展历史大致也经历这三个阶段：

1. 将化学知识以计算机形式存储，并构建数据库；

1. 机器学习；

1. 深度学习。

在第一个阶段中，人们主要做的事情就是尝试使用不同的方法，尽可能地将化学知识和信息以计算机的形式进行存储，并以此为基础开始构建数据库。例如，用一些字符表示分子或者其他化学符号，如何保存一个具有空间结构的分子的原子、键的位置信息等等。

在第二阶段，大家开始使用一些手动的特征工程对已有数据进行编码、特征提取等操作。例如，在`baseline`中我们使用了分子指纹（molecule fingerprint）作为我们的编码方式。再辅以传统的机器学习的方法，做一些预测。

在第三阶段，各种各样的深度神经网络也开始被广泛使用。这些网络不仅仅开始学习各种特征，也像`word2vec`那样，非常多的网络也被拿来对分子进行向量化。这也导致后来又非常多的新型的分子指纹出现。基于`seq2seq`模型学习表示为序列类型的化学数据、基于`diffusion`重建分子三维空间结构等等，都是当今的潮流方向。

## SMILES —— 最流行的将分子表示为序列类型数据的方法

SMILES，提出者Weininger et al[1]，全称是Simplified Molecular Input Line Entry System，是一种将化学分子用ASCII字符表示的方法，在化学信息学领域有着举足轻重的作用。当前对于分子和化学式的储存形式，几乎都是由SMILES（或者它的一些手足兄弟）完成的。使用非常广泛的分子/反应数据库，例如ZINC[2]，ChemBL[3]，USPTO[4]等，都是采用这种形式存储。SMILES将化学分子中涉及的原子、键、电荷等信息，用对应的ASCII字符表示；环、侧链等化学结构信息，用特定的书写规范表达。以此，几乎所有的分子都可以用特定的SMILES表示，且SMILES的表示还算比较直观。

表1：一些常见的化学结构用SMILES表示。

![](img/fce8fc2d65d1253273643be74227d09a.png)

表2：化学反应也可以用SMILES表示，用`>>`连接产物即可。

![](img/3af52309d5453897cc3306a85a8e12bc.png)

表3：一些常见分子的SMILES例子

![](img/875d0e1626b874e1be0ede76150a76f9.png)

在SMILES中，原子由他们的化学符号表示，`=`表示双键、`#`表示三键、`[]`里面的内容表示侧基或者特殊原子（例如`[Cu+2]`表示带电+2电荷的Cu离子）。通过SMLIES，就可以把分子表示为序列类型的数据了。

（注：SMILES有自己的局限性：例如选择不同的起始原子，写出来的SMILES不同；它无法表示空间信息。）

> 拓展：事实上，使用图数据（grpah）表示分子是非常合适的。图网络相比于基于SMILES的序列网络，在某些方面会更胜一筹。感兴趣的同学可以自行探索。

> SMILES的缺点：

1. 无法表达充分的空间信息。

1. 一个分子起始原子不同等因素，有多个SMILES。（不一一对应）—— SMILES规范化。

图1 具有对映异构的氨基酸分子。（对映异构：两者互为镜像而不能重合）

![](img/4c3924bad0fc18b978ddcae49f661912.png)

## 分子指纹 —— 分子向量化

分子的指纹就像人的指纹一样，用于表示特定的分子。分子指纹是一个具有固定长度的位向量（即由0，1组成），其中，每个为1的值表示这个分子具有某些特定的化学结构。例如，对于一个只有长度为2的分子指纹，我们可以设定第一个值表示分子是否有甲基，第二个位置的值表示分子是都有苯环，那么`[0,1]`的分子指纹表示的就是一个有苯环而没有甲基的分子。通常，分子指纹的维度都是上千的，也即记录了上千个子结构是否出现在分子中。

> （这非常像NLP中的`Bag of Word`）。

![](img/4005bea87290fd53d2c47053354c651f.png)

有一定化学知识的小伙伴可能会发现：例如像氯甲基苯的邻位和对位，采用分子指纹的形式就无法区分出来。所以，分子指纹也有自己的局限性。

图2 氯甲基苯的邻位（左）和对位（右）

![](img/d58d964ad261bc8bd6244924955bf35d.png)

也正是随着解决这类问题的需求的增加，后续发展出了非常多的新式的分子指纹。例如，就有前面提到的通过深度学习学习出来的分子指纹。

> （类似NLP中`word2vec`方法）。

特点：

1. 非常稀疏。大量的0，少量的1；

1. 指纹和分子并一一对应。

其他类型的分子指纹：

1. 算法不同，分子指纹不同；`n-gram`中`n`的不同、对分子的子图匹配规则不同。

1. 基于深度学习的分子指纹。指纹向量由深度学习得到，模型给出。（`word2vec`）

## RDkit —— 强大、丰富且高效的化学信息工具

`RDkit`是化学信息学中主要的工具，是开源的。网址：`http://www.rdkit.org`，支持`WIN`、`MAC`、`Linux`，可以被`python`、`Java`、`C`调用。几乎所有的与化学信息学相关的内容都可以在上面找到。常用的功能包括但不限于：

1. 读和写分子；

1. 循环获取分子中原子、键、环的信息；

1. 修饰分子；

1. 获取分子指纹；

1. 计算分子相似性；

1. 将分子绘制为图片；

1. 子结构匹配和搜索；

1. 生成和优化3D结构。

`RDkit`会将分子读取为`RDkit`中专属的`rdkit.Chem.rdchem.Mol`对象，并以`Mol`对象为基础，可以对分子进行转化为各种表达形式，例如SMILES、SMARTS：

```
from rdkit import Chem
m = Chem.MolFromSmiles('Cc1ccccc1')
res = Chem.MolToSmarts(m)
# res : [#6]-[#6]1:[#6]:[#6]:[#6]:[#6]:[#6]:1
```

绘制图片：

```
from rdkit.Chem import Draw
img = Draw.MolToImage(m)
img.show() # 把图片展示出来
```

![](img/5ff41a3fa52dd7d7da17e942a0a0c95f.png)

子结构搜索：

```
m = Chem.MolFromSmiles('c1ccccc1O')
patt = Chem.MolFromSmarts('ccO')
m.HasSubstructMatch(patt)
# True
```

获得分子指纹（`fingerprint`）并通过指纹计算分子之间的相似性：

```
from rdkit import Chem
from rdkit import DataStructs
from rdkit.Chem import AllChem
ms = [Chem.MolFromSmiles('CCOC'), Chem.MolFromSmiles('CCO'),
Chem.MolFromSmiles('COC')]
fpgen = AllChem.GetRDKitFPGenerator()
fps = [fpgen.GetFingerprint(x) for x in ms]
DataStructs.TanimotoSimilarity(fps[0],fps[1]) # TanimotoSimilarity是计算分子相似性的其中一个指标，更多指标详见RDkit官网
# 0.6
DataStructs.TanimotoSimilarity(fps[0],fps[2])
# 0.4
DataStructs.TanimotoSimilarity(fps[1],fps[2])
# 0.25
```

`Tanimoto Similarity`计算方法：

![](img/e920b44032b9b7cc9c42cb120ff4b081.png)

其中： 就是是分子指纹。

分子相似性的计算方法也是多样的。

## 机器学习

机器学习按照目标可以分为分类任务（`classification`）和回归（`regression`）任务两大类。所谓分类任务，就是模型预测的结果是离散的值，例如类别；那么，回归任务中，模型预测的结果就是连续的值，例如房价等等。在本次竞赛中，我们需要预测的目标是反应的产率，是`0-1`之间的一个连续的数值，所以是一个回归任务。（注：离散值通过一些处理可以近似认为是连续值，所以不要被连续值和离散值限制了自己的思维）。

传统的机器学习需要需要经历特征工程这一步骤，即将原始数据转化为向量形式。然后通过`SVM`、`Random Forest`等算法学习数据的规律。这些方法在处理简单的任务时是比较有效的。

图3 决策树 （左）分类型决策树，（右）回归型决策树

![](img/355866c68d43c74d2e286d1a28606212.png)

划分每个节点的目标是让该节点中的值尽可能相同。在分类任务中，常见的就是信息熵衡量；在回归任务中，可以使用均方误差、绝对误差等进行衡量。

图4 随机森林

![](img/5fb58fa5f325ef70ef15a4359a99f22d.png)

将多个决策树结合在一起，训练每个决策树的数据集都是随机有放回地从原数据中选出。预测的时候，输入会通过每个决策树进行预测，然后考虑每个树地输出结果，得到最终的预测值。

## 深度学习

深度学习可以归为机器学习的一个子集，主要通过神经网络学习数据的特征和分布。深度学习的一个重要进化是不再需要繁琐的特征工程，让神经网络自己从里面学习特征。

SMILES是一种以ASCII组成的序列，可以被理解为一种“化学语言”。既然是一种语言，那么很自然地想到了可以使用NLP中的方法对SMILES进行建模。

使用RNN对SMILES建模是早期的一个主要方法。RNN（Recurrent Neural Network）是处理序列数据的一把好手。RNN的网络每层除了会有自己的输出以外，还会输出一个隐向量到下一层。

图5 RNN的架构示意图

![](img/1b2c517596dd133fa79338829636dba5.png)

其中，每一层相当于做了一次线性变换：

每层的输出：`y_n = Softmax(Vh_n + c)`

通过隐向量的不断传递，序列后面的部分就通过“阅读”隐向量，获取前面序列的信息，从而提升学习能力。

但是RNN也有缺点：如果序列太长，那么两个相距比较远的字符之间的联系需要通过多个隐藏向量。这就像人和人之间传话一样，传递的人多了，很容易导致信息的损失或者扭曲。因此，它对长序列的记忆能力较弱。

同时，RNN需要一层一层地传递，所以并行能力差，同时也比较容易出现梯度消失或梯度爆炸问题。

在后面学习中，我们会继续学习使用`Transformer`这种架构，这种架构就完美解决了长序列学习能力差、难以并行、出现梯度消失或者爆炸等问题。

## 参考文献

[1] D. Weininger. Smiles, a chemical language and information system. 1. introduction to methodology and encoding rules. Journal of chemical information and computer sciences, 28(1):31–36, 1988.

[2] J. J. Irwin, T. Sterling, M. M. Mysinger, E. S. Bolstad, and R. G. Coleman. ZINC: a free tool to discover chemistry for biology. Journal of chemical information and modeling, 52(7):1757–1768, 2012.

[3] A. Gaulton, L. J. Bellis, A. P. Bento, J. Chambers, M. Davies, A. Hersey, Y. Light, S. McGlinchey, D. Michalovich, B. Al-Lazikani, et al. ChEMBL: a large-scale bioactivity database for drug discovery. Nucleic acids research, 40(D1):D1100–D1107, 2012.

[4] D. Lowe. Chemical reactions from US patents (1976-sep2016). doi, 10:m9, 2018.

# 实践部分

从教程2开始，我们就需要使用到`GPU`，因此，我们在魔搭上选择机器的时候，记得调整为`GPU`：

![](img/c52fedef08e074d73f2a47f7772c72a8.png)

建议实践部分直接在代码`ipynb`文件进行学习和笔记。（将该文件置于`baseline`得到的文件夹的`mp/code/`下）

![](img/5547a99f9324db009d5d20b47d94a9e9.png)

[`Task2_RNN.ipynb`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XC8obOuJUopkaxxJxUccGD1Jnce)

在魔搭上，我们的整个项目目录应该是这样：

![](img/b0e2dc79b4c05290661fc3f792903254.png)

其中，所有`ipynb`文件均置于`code/`下；数据放置在`dataset/`下；`model/`用来保存训练的模型；`output/`输出模型预测的结果，也即需要提交到比赛的`txt`文件。

词汇表文件：（将该文件置于`mp/`下）

![](img/4e0af152250accd9ecd3021205f53d52.png)

[`vocab_full.txt`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NGy4bJreboXFFoxWTjkczWzln2V)

对于模型部分，我们可以直接调用`Pytorch`的模块完成。`Pytorch`中内置了丰富的模型，我们只需按照教程`torch.nn.RNN`文档使用即可。`Pytorch`中RNN链接。

数据：赛题给出了`Reactant1`, `Reactant2`, `Product`, `Additive`, `Solvent`这些信息，分别代表反应物1、反应物2、产物、添加剂（催化剂等等）、溶剂。这里，我们可以按照`Reactant1.Reactant2.Additive.Solvent>Product`的方式讲这些组合成一个字串。在SMILES中，反应物和反应物之间用`.`隔开，产物用`>`隔开（注意，这些只是约定俗成）。

> 但是，在我的实践中发现：给出的数据集中，有相当多的反应的`Additive`和`Solvent`这两条中都是一样的，也即这个字段对最后`Yield`的影响相对而言更小。所以，我们将这两个段的分子忽视，进行训练。如果不省略的话，模型会清一色地输出一个值，这个值往往是训练数据中`Yield`的均值。

参数设置：学习者可以自行尝试不同的参数，对比模型的性能。同时，也可以学习和思考，如何将`Additive`，`Solvent`这两段的SMILES以一种“高效”的方式添加到模型的学习中，以增强模型的性能。

# 来个小总结

这节，我们更深入了解和学习了AI4Chemistry中的一些重要概念和方法。并对使用机器学习方法建模Chemistry问题和使用深度学习方法建模Chemistry问题。最后，在示例代码中，我们给出了具体的代码完成用RNN建模SMILES并进行预测。

# 后面还有更多内容等你挑战~

<https://datawhaler.feishu.cn/sync/XGMQd4cS3s7L0mbC5lmcHttvnRb>

> 本教程主要由   写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> 注：本文采用 署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。
