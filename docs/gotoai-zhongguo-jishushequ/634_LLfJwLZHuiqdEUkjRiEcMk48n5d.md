# 大模型蒸馏(Model Distillation）

> 来源：[https://gotoaiworld.feishu.cn/docx/JntUdFXLKoeoEYxokHWcirYsnSd](https://gotoaiworld.feishu.cn/docx/JntUdFXLKoeoEYxokHWcirYsnSd)

# 一、知识蒸馏

> 知识蒸馏是指将知识从一个笨重的大型模型或一组模型转移到可以在现实世界约束下实际部署的单个较小模型的过程。

## 1.1 什么是知识蒸馏？

知识蒸馏是指将知识从一个笨重的大型模型或一组模型转移到可以在现实世界约束下实际部署的单个较小模型的过程。从本质上讲，它是一种模型压缩形式，Bucilua和合作者于2006年首次成功演示。

知识蒸馏更常在与复杂架构（包括多个层和模型参数）相关的神经网络模型上执行。因此，随着深度学习在过去十年中的出现，以及深度学习在语音识别、图像识别和自然语言处理等多个领域的成功，知识蒸馏技术在实际应用中越来越受到重视。

部署大型深度神经网络模型的挑战对于内存和计算能力有限的边缘设备尤其重要。为了应对这一挑战，首先提出了一种模型压缩方法，将大模型中的知识转移到训练较小的模型中，而不会造成任何显着的性能损失。Hinton及其同事将这种从大模型学习小模型的过程正式化为“知识蒸馏”框架。

如图 1 所示，在知识蒸馏中，一个小型“学生”模型学会模仿大型“教师”模型，并利用教师的知识来获得相似或更高的准确率。在下一节中，我将更深入地探讨知识蒸馏框架及其底层架构和机制。

![](img/28e71abce8293ea3cd35feb2b4cc6b6c.png)

[图 1.知](https://arxiv.org/abs/2006.05525)识提炼的师生框架 |来源：Arxiv

## 1.2 深入知识蒸馏

知识蒸馏系统由知识、蒸馏算法和师生架构三个主要组成部分组成。

**知识**

在神经网络中，知识通常是指学习到的权重和偏差。同时，大型深度神经网络中的知识来源也存在丰富的多样性。典型的知识蒸馏使用logits作为教师知识的来源，而其他则侧重于中间层的权重或激活。其他类型的相关知识包括不同类型的激活和神经元之间的关系或教师模型本身的参数。

**不同形式的知识**分**为三种不同的类**型**：基于响应的知**识、基于特征的知识和基于关系的知识。图 2 说明了教师模型中的这三种不同类型的知识。我将在下一节中详细讨论这些不同的知识来源。

![](img/bb99818d05877d0f92bdb13acef7ca9a.png)

[图 2.教](https://arxiv.org/abs/2006.05525)师模型中的不同种类知识 |来源：Arxiv

1.  **基于响应的知识**

如图 2 所示，基于响应的知识侧重于教师模型的最终输出层。假设是学生模型将学习模仿教师模型的预测。如图 3 所示，这可以通过使用称为蒸馏损失的损失函数来实现，该函数分别捕获学生和教师模型的 logits 之间的差异。由于这种损失在训练中被最小化，学生模型将变得更擅长做出与教师相同的预测。

在图像分类等计算机视觉任务的背景下，软目标包括基于响应的知识。软目标表示输出类的概率分布，通常使用 softmax 函数进行估计。每个软目标对知识的贡献都使用称为温度的参数进行调节。基于软目标的基于响应的知识蒸馏通常用于监督学习的背景下。

[图 3.基](https://arxiv.org/abs/2006.05525)于响应的知识蒸馏 |来源：Arxiv

1.  **基于特征的知识**

经过训练的教师模型还可以捕获中间层中的数据知识，这对于深度神经网络尤其重要。中间层学习区分特定特征，这些知识可用于训练学生模型。如图 4 所示，目标是训练学生模型学习与教师模型相同的特征激活。蒸馏损失函数通过最小化教师和学生模型的特征激活之间的差异来实现这一点。

![](img/4db7e8dccc3d9310d0c2a6193159cae1.png)

[图 4.基](https://arxiv.org/abs/2006.05525)于特征的知识蒸馏 |来源：Arxiv

1.  **基于关系的知识**

除了在神经网络的输出层和中间层中表示的知识外，捕获特征图之间关系的知识也可用于训练学生模型。这种形式的知识，称为基于关系的知识，如图 5 所示。这种关系可以建模为特征图、图形、相似矩阵、特征嵌入或基于特征表示的概率分布之间的相关性。

![](img/2206f3d436480810bfedf1ce0066d8e8.png)

[图 5.基](https://arxiv.org/abs/2006.05525)于关系的知识蒸馏 |来源：Arxiv

**训练**

培训学生和教师模型的方法主要分为三种类型，即离线、在线和自蒸馏。蒸馏训练方法的分类取决于教师模型是否与学生模型同时修改，如图6所示。

[图6.知识](https://arxiv.org/abs/2006.05525)蒸馏培训计划的类型 |来源：Arxiv

**1.离线蒸馏**

离线蒸馏是最常见的方法，其中使用预训练的教师模型来指导学生模型。在该方案中，首先在训练数据集上对教师模型进行预训练，然后从教师模型中提炼出知识来训练学生模型。鉴于深度学习的最新进展，各种预训练的神经网络模型是公开可用的，这些模型可以根据用例充当教师。离线蒸馏是深度学习中的一种成熟技术，并且更容易实施。

1.  **在线蒸馏**

在离线蒸馏中，预训练的教师模型通常是一个大容量的深度神经网络。对于一些用例，预训练模型可能无法用于离线蒸馏。为了解决这一限制，可以使用在线蒸馏，在单个端到端训练过程中同时更新教师和学生模型。在线蒸馏可以使用并行计算进行作，从而使其成为一种高效的方法。

1.  **自蒸馏**

如图6所示，在自蒸馏中，教师和学生模型使用相同的模型。例如，来自深度神经网络深层的知识可用于训练浅层。它可以被认为是在线蒸馏的特例，并以多种方式实例化。教师模型早期时期的知识可以转移到其后期时期来训练学生模型。

**建筑**

师生网络架构的设计对于高效的知识获取和提炼至关重要。通常，更复杂的教师模型和更简单的学生模型之间存在模型能力差距。这种结构性差距可以通过高效的师生架构优化知识转移来缩小。

由于深度神经网络的深度和广度，从深度神经网络转移知识并不简单。最常见的知识转移架构包括学生模型，即：

+   教师模型的较浅版本，层数更少，每层神经元数更少，

+   教师模型的量化版本，

+   具有高效基本作的更小网络，

+   具有优化全球网络架构的更小网络，

+   和老师一样的模式。

除了上述方法之外，神经架构搜索等最新进展也可用于在给定特定教师模型的情况下设计最佳学生模型架构。

## 1.3 知识蒸馏算法

在本节中，我将重点介绍训练学生模型从教师模型中获取知识的算法。

1.  **对抗蒸馏**

最近在生成对抗网络的背景下概念化的对抗学习用于训练一个生成器模型，该模型学习生成尽可能接近真实数据分布的合成数据样本，以及一个鉴别器模型，该模型学习区分真实数据样本和合成数据样本。这一概念已应用于知识蒸馏，使学生和教师模型能够更好地表示真实的数据分布。

为了实现学习真实数据分布的目标，可以使用对抗学习来训练生成器模型，以获得合成训练数据以用作此或增强原始训练数据集。第二种基于对抗学习的蒸馏方法侧重于鉴别器模型，以根据logits或特征图将样本与学生和教师模型区分开来。这种方法可以帮助学生很好地模仿老师。第三种基于对抗性学习的蒸馏技术侧重于在线蒸馏，其中学生和教师模型被共同优化。

1.  **多教师蒸馏**

在多教师蒸馏中，学生模型从几个不同的教师模型中获取知识，如图 7 所示。使用教师模型集合可以为学生模型提供不同类型的知识，这些知识可能比从单个教师模型获得的知识更有益。

来自多个教师的知识可以组合为所有模型的平均响应。通常从教师那里传授的知识类型基于 logits 和特征表示。如第 2.1 节所述，多名教师可以传授不同类型的知识。

图7.多师蒸馏 |来源：[Arxiv](https://arxiv.org/abs/2006.05525)

1.  **跨模式蒸馏**

图8显示了跨模态蒸馏训练方案。在这里，教师接受一种模式的培训，其知识被提炼到需要不同模式知识的学生中。当在训练或测试期间无法获得特定模式的数据或标签时，就会出现这种情况，因此需要跨模式转移知识。

跨模态蒸馏最常用于视觉领域。例如，来自在标记图像数据上训练的教师的知识可用于蒸馏具有未标记输入域（如光流、文本或音频）的学生模型。在这种情况下，从教师模型的图像中学习的特征用于学生模型的监督训练。跨模态蒸馏对于视觉问答、图像字幕等应用非常有用。

![](img/acffbec95f42a6b2510c1e1060fa712b.png)

图8.跨模式蒸馏 |来源：[Arxiv](https://arxiv.org/abs/2006.05525)

1.  **其他**

除了上面讨论的蒸馏算法外，还有其他几种算法已应用于知识蒸馏。

+   **基于图形的蒸馏**使用图形而不是从教师到学生的单个实例知识来捕获数据内部关系。图表有两种使用方式——作[为知识](https://neptune.ai/blog/self-supervised-learning)转移的手段，以及控制教师知识的转移。在基于图的蒸馏中，图的每个顶点代表一个自监督的教师，它可以分别基于基于响应或基于特征的知识，如logits和特征图。

+   **基于注意力的蒸馏**基于使用注意力图从特征嵌入中转移知识。

+   由于隐私、安全或保密原因，无**数据蒸馏**基于合成数据，没有训练数据集。合成数据通常是从预训练教师模型的特征表示中生成的。在其他应用中，GAN 还用于生成合成训练数据。

+   **量化蒸馏**用于将知识从高精度教师模型（例如 32 位浮点）转移到低精度学生网络（例如 8 位）。

+   **终身蒸馏**基于持续学习、终身学习和元学习的学习机制，其中以前学到的知识被积累并转移到未来的学习中。

+   **基于搜索的神经架构蒸馏**用于识别合适的学生模型架构，以优化教师模型的学习。

## 1.4 知识蒸馏的应用

知识蒸馏已成功应用于图像识别、NLP 和语音识别等多个机器学习和深度学习用例。在本节中，我将重点介绍知识蒸馏技术的现有应用和未来潜力。

1.  **愿景**

知识蒸馏在[计算机视觉](https://neptune.ai/blog/category/computer-vision)领域的应用非常多。最先进的计算机视觉模型越来越多地基于深度神经网络，这些网络可以从模型压缩中获益于部署。知识蒸馏已成功用于以下用例：

+   图像分类，

+   人脸识别，

+   图像分割，

+   动作识别，

+   目标检测，

+   车道检测，

+   行人检测，

+   面部标志检测，

+   位姿估计，

+   视频字幕 /

+   图像检索，

+   阴影检测，

+   文本到图像合成，

+   视频分类，

+   视觉问答等 。

知[识蒸馏还可用](https://neptune.ai/blog/improving-ml-model-performance)于交叉分辨率人脸识别等利基用例，其中基于高分辨率人脸教师模型和低分辨率人脸学生模型的架构可以提高模型性能和延迟。由于知识蒸馏可以利用不同类型的知识，包括跨模态数据、多域、多任务和低分辨率数据，因此可以针对特定的视觉识别用例训练各种蒸馏的学生模型。

**可能对您感兴趣**

[对象检测算法和库](https://neptune.ai/blog/object-detection-algorithms-and-libraries)

运[行计算机视觉项目的顶级工](https://neptune.ai/blog/top-tools-to-run-a-computer-vision-project)具

1.  **自然语言处理**

[鉴于语](https://neptune.ai/blog/category/natural-language-processing)言模型或翻译模型等大容量深度神经网络的盛行，知识蒸馏在 NLP 应用中的应用尤为重要。最先进的语言模型包含数十亿个参数，例如GPT-3包含1750亿个参数。这比之前最先进的语言模型 BERT 大了几个数量级，后者在基础版本中包含 1.1 亿个参数。

因此，知识蒸馏在 NLP 中非常流行，以获得快速、轻量级的模型，这些模型训练起来更容易且计算成本更低。除了语言建模之外，知识蒸馏还用于 NLP 用例，例如：

+   神经机器翻译，

+   文本生成，

+   问答

+   文献检索，

+   文本识别。

使用知识蒸馏，可以获得高效、轻量级的 NLP 模型，这些模型可以以较低的内存和计算要求进行部署。师生培训还可用于解决多语言 NLP 问题，其中来自多语言模型的知识可以相互转移和共享。

**案例研究：DistilBERT**

[DistilBERT](https://sol.sbc.org.br/journals/index.php/jidm/article/download/1895/1775/7066) 是 Hugging Face 开发的一种更小、[更](https://arxiv.org/abs/1910.01108v4)快、更便宜、更轻的 BERT 模型 [4]。在这里，作者预训练了一个较小的 BERT 模型，该模型可以在各种 NLP 任务上进行微调，精度相当高。在预训练阶段应用知识蒸馏，获得 BERT 模型的蒸馏版本，该模型小 40%（6600 万个参数对 1.1 亿个参数），速度快 60%（GLUE 情感分析任务的推理为 410 秒对 668 秒），同时保留了相当于原始 BERT 模型准确率 97% 的模型性能。在 DistilBERT 中，学生具有与 BERT 相同的架构，并且是使用一种新的三元组损失获得的，该损失结合了与语言建模、蒸馏和余弦距离损失相关的损失。

**另请阅读**

揭秘 BERT：Transformer 模型性能的关键

1.  **演讲**

最先进的语音识别模型也基于深度神经网络。现代 ASR 模型是端到端训练的，基于架构，包括卷积层、具有注意力的序列到序列模型，以及最近的 transformer。对于实时的设备上语音识别，获得更小、更快的模型以获得有效性能变得至关重要。

语音中的知识蒸馏有几个用例：

+   **语音识别**

+   **口语识别，**

+   **音频分类，**

+   **说话人识别，**

+   **声学事件检测，**

+   **语音合成，**

+   **语音增强，**

+   **噪声鲁棒ASR **

+   **多语言ASR，**

+   **口音检测。**

案例研究：Amazon Alexa 的声学建模

Parthasarathi 和 Strom （2019） 利用师生培训为 100 万小时的未标记语音数据生成软目标，其中训练数据集仅包含 7000 小时的标记语音。教师模型产生了所有输出类的概率分布。学生模型还在给定相同特征向量的情况下对输出类产生概率分布，目标函数优化了这两个分布之间的交叉熵损失。在这里，知识蒸馏有助于简化在大量语音数据语料库上生成目标标签。

https://neptune.ai/blog/knowledge-distillation

## 1.5 大模型蒸馏过程

大模型蒸馏是一种将大型、复杂模型（教师模型）的知识迁移到小型、高效模型（学生模型）的技术，旨在解决大模型部署成本高、推理速度慢的问题。以下是其详细介绍：

+   核心原理：通过让学生模型模仿教师模型的输出（如概率分布、中间特征、注意力权重等）来学习其行为模式，而非直接学习原始数据，从而获得更高效的推理能力。

+   **蒸馏过程：**

+   训练大模型：首先训练一个大模型，这个模型通常非常复杂，且在训练数据上表现出色，但计算消耗也非常大。

+   **生成软标签：**使用训练好的大模型对数据进行推理，得到一个概率分布，即软标签。这些软标签包含了大模型对各个类别的 “信心”，比硬标签信息更丰富。

+   训练小模型：使用软标签来训练一个较小的模型。小模型通过拟合大模型的输出，学习到大模型所掌握的知识，从而获得类似的性能。

+   **技术方法：**

+   离线蒸馏：教师模型预训练完成后固定参数，学生模型单独训练，如 TinyBERT、DistilBERT。

+   在线蒸馏：教师和学生模型联合训练，动态调整知识传递，如 Deep Mutual Learning。

+   自蒸馏：同一模型内部不同层或分支间传递知识，如 MiniLM。

+   多教师蒸馏：融合多个教师模型的输出，提升学生模型的泛化性。

+   **作用与价值：**

+   模型轻量化：降低模型的复杂度和计算量，提高模型的运行效率。

+   加速推理，降低运行成本：简化后的模型运行速度更快，减少了对硬件资源的需求，降低模型运行成本。

+   提升泛化能力：帮助学生模型学习到教师模型中蕴含的泛化模式，提高其在未见过的数据上的表现。

+   促进模型的可解释性和可部署性：轻量化后的模型通常更加简洁明了，有利于理解和分析模型的决策过程，同时也更容易进行部署和应用。

+   **应用场景：**

+   移动端部署：如华为的 TinyBERT 用于手机端 NLP 任务。

+   实时推理：谷歌的 Meena 聊天机器人通过蒸馏降低延迟。

+   低成本训练：初创公司使用 DistilGPT-3 替代昂贵的 GPT-3 API。

+   隐私保护：蒸馏模型避免直接使用敏感数据训练大模型，小模型更易本地化部署，减少数据上传风险。

# 二、大模型蒸馏和量化差别

## **2.1 目的不同**

1.  蒸馏：核心目的是知识迁移，将大模型（教师模型）的知识和能力传递给小模型（学生模型），使小模型在保持较小规模和较低计算成本的同时，尽可能获取大模型的性能，提高模型的泛化能力。

1.  量化：主要是通过降低模型参数的数值精度，来减小模型体积、降低计算资源需求，加快模型的推理速度，以便在资源受限的设备上运行。

## **2.2 操作对象不同**

1.  蒸馏：操作对象是两个不同规模的模型，即教师模型和学生模型，关注的是模型之间知识的传递和模仿。

1.  量化：操作对象是模型内部的参数，对模型中原本以高精度表示的参数进行精度降低处理。

## **2.3 实现方法不同**

1.  蒸馏：通过让学生模型学习教师模型的输出概率分布（软标签）、中间层特征或样本之间的关系等知识来实现，如软标签蒸馏、硬标签蒸馏和混合策略等。

1.  量化：通过将模型参数从较高精度的数值表示转换为较低精度的数值表示，如将 32 位浮点数转换为 16 位、8 位浮点数甚至整数，有线性量化、非线性量化等方法。

## **2.4 对模型性能的影响不同**

1.  蒸馏：如果蒸馏过程设计得当，学生模型可以在一定程度上接近教师模型的性能，在某些情况下甚至可能超越教师模型在特定任务上的表现，同时提升泛化能力。

1.  量化：不可避免地会带来一定的精度损失，虽然通过合理的量化策略可以尽量减少对模型性能的影响，但在极端情况下可能会导致模型性能下降。

## **2.5 应用场景不同**

1.  蒸馏：适用于需要在不同规模模型之间进行知识迁移的场景，如将大型的预训练模型的知识迁移到小型的定制化模型中，以满足特定应用场景对模型性能和资源消耗的要求。

1.  量化：适用于对计算资源和存储资源有严格限制的场景，如在移动端设备、边缘计算设备上部署模型时，通过量化可以使模型更好地适应这些设备的硬件条件。

## 2.6 举例说明

1.  DeepSeek-R1-Distill-Qwen-7B-Q8_0.gguf

1.  该模型使用了蒸馏还是量化？

1.  这个模型既使用了蒸馏技术，也使用了量化技术，具体分析如下：

1.  蒸馏：模型名称中的“Distill”表明采用了蒸馏技术，是以DeepSeek-R1为教师模型，将其知识和推理能力通过蒸馏传递给Qwen-7B这个学生模型。

1.  量化：模型名称中的“Q8_0”表示该模型进行了量化操作，采用的是8位量化，即将模型参数从较高精度的数值表示转换为8位的数值表示，以减小模型体积、降低计算资源需求和加快推理速度。

# 三、Blog

## 1.Distillation: Turning Smaller Models into High-Performance, Cost-Effective Solutions

https://techcommunity.microsoft.com/blog/aiplatformblog/distillation-turning-smaller-models-into-high-performance-cost-effective-solutio/4355029

**Introduction**

![](img/5b3c268907f03154c7931d3ab2786f66.png)

Large Language Models (LLMs) have transformed the landscape of natural language processing (NLP) with their ability to understand and generate human-like text. However, their size and complexity often pose challenges in terms of deployment, speed, and cost. Usually for specialized niche tasks, we end up deploying the best available model even though we don’t utilize all its capabilities. This is where distillation comes in, offering a method to create (fine-tune) smaller, customized, more efficient models, while retaining much of the performance of a significantly larger state-of-the-art model.

What is distillation?

Distillation is a technique designed to transfer knowledge of a large pre-trained model (the "teacher") into a smaller model (the "student"), enabling the student model to achieve comparable performance to the teacher model. This technique allows users to leverage the high quality of larger LLMs, while reducing inference costs in a production environment, thanks to the smaller student model.

How distillation works?

In distillation, knowledge can be transferred from teacher to student model in several ways. Here, we specifically discuss response-based, offline distillation, where the student model learns to mimic the output (only predictions) of the teacher model, and the teacher model is not trained during distillation.

**Teacher Model:** A large, high-capacity teacher model that is already pre-trained on massive datasets. This model has learnt rich representations and complex patterns from the data which allows it to generalize well even on unseen tasks.

Knowledge Extraction: The teacher model generates outputs based on given inputs, which are then used as training data for the student model.  This involves not just mimicking outputs but also understanding the underlying reasoning processes.

Student Model Training: A smaller student model is trained using the extracted knowledge as a guide. The student model learns to mimic the teacher model's behavior and predictions on specific tasks.

**Advantages**

![](img/9689e16d2314f6e5d819aa32e5765fb2.png)

**Reduced Size**: The resulting student model is significantly smaller, making it easier to deploy in resource-constrained environments.

**Lower Cost**: Running smaller models incurs lower operational costs while maintaining competitive performance levels.

Task-Specific Optimization: Distillation can be tailored for specific applications, enhancing efficiency and accuracy.

**Performance**:  Smaller models exhibit significantly lower latency compared to larger models, which in turn boosts the throughput of the deployment.

**Customization**: Distillation allows users to select desirable traits from multiple larger models and transfer them to smaller models.

**Personalization**: Personality traits can be incorporated into the model, enabling it to respond with relevant answers when queried about its personality.

Synthetic Data Generation: At scale data generation can be done either only for labels or from scratch using just seed/meta data.

**Generalization:** Distillation can help student models generalize better by learning from the teacher model's knowledge and avoiding overfitting.

**Improved Multili**ngual Capabilities: The multilingual performance of smaller models can be significantly enhanced with the help of teacher models making them suitable for global applications.

Distillation in Azure AI Foundry

Distillation as a Service is now supported on Azure allowing a variety of task types and more to be added soon. Following tasks are supported.

+   **Summarization**: Given a document (article) to summarize, generate an entity-dense summary of the document.

+   Conversational Assistant: Generate AI assistant responses on single-turn and multi-turn conversational datasets. To generate each response, the available chat history and the current user prompt are utilized.

+   Natural Language Understanding (NLU)

o   **MATH: **Generate numeric answers to math problems.

o   Natural Language Inference (NLI): Given premise and hypothesis, determine if premise entails the hypothesis, or contradicts the hypothesis, or is neutral i.e. neither entails not contradicts the hypothesis.

o   **Multiple-Choice **Quest**i**on Answering: Given question and answer choices, determine the correct answer choice.

Distillation Process

Overview of the two-step distillation process: (1) Generate synthetic data using a task-specific, elaborate prompt (2) Train (and infer from) the student model using a shorter prompt

(Figure source: https://arxiv.org/pdf/2410.18588)

The distillation process involves two main steps: generate high quality synthetic data (labels) using the teacher model, followed by instruction-based finetuning of the student model.

**Data Generation**

High-quality data generation is crucial for the student model's performance. Azure provides a proprietary library of advanced prompts, to generate high-quality synthetic data for all supported tasks, utilizing techniques such as Chain of Thought (CoT) or Chain of Density (CoD), and other best practices. This option can be enabled by passing the enable_chain_of_thought parameter while invoking the distillation pipeline, ensuring reasoning-based answers and consequently high-quality data for distillation.

Instruction Fine-Tuning

The next step is to fine-tune the smaller model using the task-specific generated data. This involves using a concise, task-specific prompt and training with the input and generated output (excluding reasoning steps). These innovations ensure significant performance gains for a given task while minimizing the cost (number of tokens) for the user. When using user-provided prompts, the same prompt is applied in both data generation and fine-tuning.

## 2.知识蒸馏+持续学习

###  什么是知识蒸馏？

+   核心目标： 把一个大而复杂的模型（通常叫教师模型）学到的“知识”和“技巧”，压缩并传授给一个小而高效的模型（通常叫学生模型）。

+   为什么需要它？ 那些超级厉害的AI模型（比如能写诗、答题、识图的大模型）往往体积庞大、计算复杂，需要强大的电脑（如GPU服务器）才能运行。这导致它们很难直接用在你的手机📱、智能手表⌚️或者家里的智能音箱上（这些设备计算能力有限、电量也有限）。

![](img/8d863949e107c715ea70f9b46188a982.png)

+   解决办法： 就像老师傅不可能时刻在厨房掌勺，但可以把毕生经验总结成简单口诀教给徒弟一样，知识蒸馏让强大的教师模型把自己的“智慧”提炼出来，教给小模型，让小模型也能达到接近老师的高水平，但运行起来更快、更省资源！

### 🧑‍🏫 老师傅怎么教徒弟？（技术比喻简化版）

![](img/47be37e35caa1e5d47d2766ce678e4c3.png)

1.  老师傅展示绝活： 教师模型面对一堆问题（比如识别图片是猫🐱还是狗🐶）时，不仅给出最终答案（“这是猫”），还会给出一个更详细的“解题思路”或“可信度分布”——比如它可能说：“这张图有85%像猫，12%像狗，3%像兔子🐰”。这个详细的判断结果，包含了模型捕捉到的细微差别（比如某些花纹让猫看起来有点像狗），叫做软标签。

1.  徒弟模仿学习：

![](img/d2ef9c53dca1e7b1edd595536a6d60e3.png)

1.  掌握精髓： 通过模仿老师傅详细的判断思路，徒弟（学生模型）学到了老师傅捕捉关键特征、权衡模糊信息的能力。虽然徒弟的“大脑”（模型结构）简单很多，但它理解了做判断的“门道”和“感觉”，因此也能做出更接近老师傅水平的判断，而不仅仅是死记硬背答案。

### ✨ 知识蒸馏的好处

+   变小变快： 学生模型体积小很多，计算速度快很多，耗电也少很多。

+   保持水准： 学生模型的性能通常比直接用硬标签训练的小模型要好得多，甚至能接近教师模型的水平。

+   更“聪明”： 学生模型通过模仿软标签，学到了更鲁棒（不容易被干扰）、更泛化（处理新情况更好）的能力。

+   易于部署： 小巧高效的学生模型可以轻松塞进你的手机、手表、汽车、摄像头等各种日常设备里，让强大的AI能力无处不在！

![](img/403f6e76f9da0abcb8f6d4f5aff15a11.png)

### 🔍 一句话总结

> 知识蒸馏就像是让一位AI“大师”把毕生所学凝练成精华，传授给一位“小学徒”，让这个小学徒也能拥有接近大师的本领，但行动更轻快、更适合走进千家万户。 它解决了大模型“不好用”的问题，是小设备也能享受强大AI的关键技术之一！

# 四、Paper论文

## Knowledge Distillation: A Survey

![](img/5cb335e1029ca26f381011b1f6e80cf3.png)

https://arxiv.org/abs/2006.05525

# 五、参考

## 1.1 Knowledge Distillation Tutorial

https://docs.pytorch.org/tutorials/beginner/knowledge_distillation_tutorial.html

## 1.2 Understanding the Essentials of Model Distillation

https://medium.com/stream-zero/understanding-the-essentials-of-model-distillation-in-ai-1e97403bee8a

## 1.3 What Is Model Distillation?

https://builtin.com/artificial-intelligence/model-distillation

## 1.4 Model Distillation: What is it and how does it work?

https://humanloop.com/blog/model-distillation

## 1.5 What is Model Distillation? - Labelbox

https://labelbox.com/guides/model-distillation/