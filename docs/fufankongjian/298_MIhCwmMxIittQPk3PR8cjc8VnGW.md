# MateConv全量指令微调

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/BJlZdI9Yko300kxOUNZcgTcpnPg](https://kq4b3vgg5b.feishu.cn/docx/BJlZdI9Yko300kxOUNZcgTcpnPg)

### 【补充介绍】全量指令微调概念补充介绍

+   **从补全模型到对话模型**

全量指令微调（Full Instruction Tuning）在现代大规模预训练模型的应用中具有重要的意义。虽然预训练模型在大量的无监督数据上获得了强大的语言理解和生成能力，但这些模型通常缺乏处理具体任务的针对性表现。全量指令微调的核心作用在于将预训练模型进一步调整，使其能够根据明确的指令执行特定任务。

![](img/62096c675649988e3815c0e06dad1b08.png)

![](img/0e2da9addee04d47c8c7a266111e3099.png)

+   **全量指令微调的必要性**

1.  **增强模型的任务适应能力**：

+   预训练阶段的语言模型往往是通用的，并没有针对某个特定任务进行优化。虽然它们具备强大的生成和理解能力，但在实际应用中，我们常常需要模型执行某些具体的任务，例如对话生成、问答、文本摘要、翻译等。全量指令微调通过在明确的指令或任务定义下对模型进行进一步优化，提升了模型对这些任务的适应能力。

1.  **提升模型对多样化任务的执行能力**：

+   指令微调的一个关键特征是它能够帮助模型学会根据不同的输入指令生成相应的输出。通过微调，模型能够处理广泛的任务类型，并根据指令灵活应对不同的任务需求。这个过程让模型从一个纯语言生成器转变为一个能够执行多种任务的通用人工智能系统。

1.  **缩小预训练模型与应用场景的差距**：

+   预训练阶段的数据大多来自通用的文本数据，而模型实际应用中的任务和数据形式可能与预训练数据差异较大。通过全量指令微调，模型可以学习到如何在特定任务下调整其生成和决策方式，确保在实际应用中的表现更加精准和高效。这种微调过程弥补了预训练模型和实际应用场景之间的差距。

1.  **减少下游任务的数据需求**：

+   全量指令微调使得模型在接受指令后，能够直接执行多个任务，而不再需要为每个任务单独进行微调或标注大量数据。通过在多任务、多领域的指令数据上进行微调，模型能够以较少的数据应对大量的任务，提升了其广泛的适用性。

1.  **提高用户交互的可控性**：

+   通过指令微调，模型可以更好地理解和执行用户的明确需求，这使得模型的输出更加符合用户预期。用户可以通过简单明确的指令控制模型行为，而不再依赖复杂的上下文提示。这种可控性对于提供高质量的自动化服务和用户交互尤为重要。

全量指令微调是使预训练模型更加实用和高效的关键步骤。它不仅提升了模型的任务执行能力，还使得模型更加灵活、可控和广泛适应多样化的应用场景。这使得指令微调成为在现代大规模语言模型开发和应用中不可或缺的一部分。

![](img/ac683c9482f56d80a77ccd807aa9fe85.png)

![](img/d4a32157dc47f0e20304632f9e91e4ec.png)

+   预训练模型和指令微调模型不同的适用场景

不过需要注意的是，其实**预训练模型**和**指令微调模型**各自都有不同的适用场景，尽管指令微调模型在特定任务上表现更好，但预训练模型也有其独特的优势和适用领域。接下来，我们可以比较它们各自适用的场景，并解释为什么在某些情况下预训练模型仍然有它的价值。

##### 预训练模型的适用场景

1.  **通用语言理解任务**：

+   **场景**：预训练模型经过大量无监督文本数据的训练，具备广泛的语言理解能力，能够在没有明确任务定义的情况下生成文本或做出推理。

+   **例子**：生成文章、写作辅助、文本补全等需要较强语言理解和生成能力的任务。

+   **原因**：预训练模型没有被限制在特定任务上，因此它可以很好地处理广泛的语言生成需求，并且在一些开放领域的应用中表现出色。

1.  **低资源或无监督学习任务**：

+   **场景**：在数据有限或者缺乏标注数据的场景下，预训练模型可以直接应用于一些无监督或少量数据的任务，如文本分类、情感分析等。

+   **例子**：当你没有足够的任务数据进行微调时，预训练模型可以通过自监督学习生成文本或进行特征提取。

+   **原因**：预训练模型的核心优势在于它从大量无标注数据中学到的广泛的语言模式，这让它即使在没有具体任务数据的情况下也能展现出良好的表现。

1.  **跨领域或未知任务的探索**：

+   **场景**：当你需要在未知领域或者全新任务上探索模型的表现时，预训练模型可以作为一个强大的基础模型进行初步的探索和实验。

+   **例子**：如果你需要构建一个多领域的生成任务，预训练模型可以提供灵活性，允许你在不同任务之间切换，而不需要进行专门的任务微调。

+   **原因**：由于预训练模型没有被微调锁定在某一个任务上，它可以应用于不同领域、不同形式的输入，而不受任务特定的限制。

1.  **探索性的文本生成**：

+   **场景**：当任务的目标是生成富有创造力、探索性或多样化的文本时，预训练模型由于其没有特定任务的约束，可能会生成更具多样性和创造力的文本。

+   **例子**：文学写作、诗歌创作、广告文案等需要高自由度生成的任务。

+   **原因**：预训练模型的多样化语言生成能力让它能够在这些没有明确限制的场景中表现得更加灵活。

##### 指令微调模型的适用场景

1.  **明确任务驱动的场景**：

+   **场景**：在指令微调模型中，模型经过了明确的任务定义和微调，能够很好地理解并执行用户给定的指令。

+   **例子**：问答系统、文本摘要、机器翻译等有明确指令的任务。

+   **原因**：指令微调模型能够根据特定任务中的指令生成高质量、符合任务需求的输出。它通过微调，在特定任务上表现更加精确。

1.  **多任务统一处理**：

+   **场景**：在需要统一处理多个任务的情况下，指令微调模型非常适合。这些模型能够根据指令动态适应不同的任务需求，而无需为每个任务单独训练不同的模型。

+   **例子**：集成多种能力的智能助手，能够根据不同的指令生成对话、翻译文本、生成摘要等多种任务的结果。

+   **原因**：指令微调模型通过在多任务数据上的训练，具备根据不同指令处理不同任务的能力，这使得它可以灵活应对复杂的多任务场景。

1.  **高精度特定任务**：

+   **场景**：当任务需要高精度的结果，例如法律文档分析、医疗问答等具有高专业性要求的任务时，指令微调模型可以通过微调后的知识提升在特定任务上的表现。

+   **例子**：法律问题解答、医疗诊断建议生成等任务。

+   **原因**：指令微调模型可以在这些任务特定的领域数据上进行进一步的优化，从而在输出结果的准确性和专业性上更有保障。

1.  **高效用户交互**：

+   **场景**：在需要与用户进行高效交互的系统中，指令微调模型能够准确理解用户的意图，并根据具体指令生成用户期望的答案或结果。

+   **例子**：智能客服系统、虚拟助手、对话生成等任务。

+   **原因**：指令微调模型通过学习用户指令的意图，能够更好地理解并生成符合用户预期的结果，提升交互体验。

##### 预训练模型 vs. 指令微调模型适用场景的比较

| 特点 | 预训练模型 | 指令微调模型 |
| --- | --- | --- |
| **任务灵活性** | 适用于通用任务，能在没有特定任务数据的情况下进行探索性生成和任务处理 | 针对明确任务进行优化，能根据特定指令执行高效、精确的任务 |
| **任务精确性** | 生成的内容更加通用，适用于开放领域的任务 | 生成内容更加准确，特别适合需要精确输出的应用场景 |
| **多任务处理能力** | 可处理广泛的任务，但没有经过微调，在特定任务上的表现可能不如微调模型 | 能根据指令灵活处理多个任务，在多任务环境中表现优秀 |
| **资源要求** | 通常预训练模型在推理时资源需求较大，但在未微调的场景下灵活度较高 | 微调后模型在指定任务上表现出色，但可能在资源上有较高要求，尤其是多任务微调的情况下 |
| **无监督学习应用** | 在没有标注数据的场景下，预训练模型可以直接用于无监督任务 | 通常需要在标注数据上进行微调，因此在标注数据不足的情况下，微调模型不如预训练模型适用 |
| **用户交互** | 可以根据通用的用户输入生成合理的文本，但缺乏对特定指令的理解 | 能更好地根据用户的指令执行任务，提升用户交互体验 |
| **探索性和创造性** | 更加适合开放性任务，能生成探索性、创造性较强的文本 | 在任务明确时表现优异，但在过于开放的任务中，生成结果的多样性和创造力可能不如预训练模型 |

##### 总结：

+   **预训练模型**：适用于开放领域任务、通用生成、低资源场景、跨领域探索和无监督学习任务。它在没有明确任务要求时非常灵活，可以直接应用于多个任务场景。

+   **指令微调模型**：适合有明确指令需求的任务、高精度的特定任务、多任务处理以及需要高效用户交互的应用。它在根据指令进行高效任务执行和准确性要求较高的任务中表现出色。

+   **全量指令微调的一般流程**

1.  **数据准备**：

+   **任务定义与指令设计**：

+   在进行全量指令微调之前，必须明确微调模型需要执行的具体任务。这些任务可以是对话生成、问答系统、文本分类、机器翻译等。在数据准备阶段，需要为每个任务设计一组明确的指令，以指导模型的行为。例如，输入文本可以是 “给定一段话，请生成摘要”，而期望的输出是该段话的简洁概述。

+   **多样化的指令数据集**：

+   指令微调的数据集通常包括多任务、多领域的数据，以增强模型的广泛适应能力。这些数据集可以来自多个来源，比如自然语言处理（NLP）任务数据集或用户交互记录。数据集的多样性越高，模型在面对不同任务时的表现通常越好。

+   **指令格式化**：

+   在数据集中，输入的文本通常被格式化为“指令 + 输入”的形式，模型需要根据这组输入生成相应的输出。为了保持一致性，所有任务的数据都应遵循统一的格式标准，例如：

```
指令: 给定以下问题，请生成合理的回答。
输入: 问题: 世界上最高的山是什么？
输出: 世界上最高的山是珠穆朗玛峰。
```

1.  **模型配置**：

+   **预训练模型加载**：

+   微调的基础是已经预训练好的大模型。这些模型通常具有通用的语言理解能力。加载预训练模型时，需确保模型具备良好的初始化权重，使其能够较快适应指令微调任务。

+   **调整模型参数**：

+   在模型配置阶段，你可能需要根据指令微调任务的复杂性调整模型参数。比如，增加 max_seq_len 以处理较长的指令输入，或调整 vocab_size 以处理新的领域词汇。确保模型结构足够灵活，能够适应不同的任务需求。

1.  **指令微调**：

+   **多任务微调**：

+   全量指令微调的一个特点是它通常在多任务数据上同时进行训练。这意味着模型将同时接触来自不同任务的数据，并根据不同的指令生成相应的输出。训练时，模型会学习如何根据不同类型的输入指令作出正确的响应。

+   **损失函数设计**：

+   在微调阶段，损失函数的选择尤为重要。通常使用交叉熵损失函数来衡量模型输出与预期结果之间的差距。此外，如果任务之间的目标差异较大，可以对不同任务设置权重，从而引导模型重点关注某些特定的任务。

+   **学习率和优化器**：

+   选择合适的学习率和优化器对模型的微调至关重要。一般来说，可以使用较小的学习率（例如 1e-5 或 5e-5）来避免对预训练模型权重进行过度修改。AdamW 是较为常见的优化器，因为它在大规模语言模型微调任务中表现稳定。

1.  **训练与监控**：

+   **训练过程**：

+   在训练过程中，模型会根据指令和输入生成对应的输出，优化器会根据损失函数不断更新模型的权重。在这个过程中，确保模型训练稳定并且逐渐提高在各个任务上的表现。

+   **使用监控工具**：

+   在训练过程中，建议使用监控工具（如 TensorBoard 或 W&B），实时监控损失曲线、训练进度等信息，以便及早发现潜在问题，例如模型过拟合或收敛不良。

+   **验证集评估**：

+   在训练的过程中，定期使用验证集来评估模型在未见过的数据上的表现。验证集的设计应当与训练集保持一致，且最好覆盖多个任务场景，以确保模型的泛化能力。

1.  **模型评估与测试**：

+   **评估模型性能**：

+   一旦模型完成微调，需要对其在多个任务上的性能进行评估。常用的评估指标包括准确率（Accuracy）、精确率（Precision）、召回率（Recall）、F1 分数等，具体选择取决于任务的性质。

+   **测试实际应用场景**：

+   为了确保模型在真实场景中可以有效工作，可以通过实际的应用场景进行测试。比如，你可以通过模拟用户指令来测试模型在不同任务上的反应是否符合预期。

1.  **部署与应用**：

+   **模型导出与部署**：

+   完成微调后，模型可以导出为可部署的格式（例如 TorchScript、ONNX 等），并部署到生产环境中。

+   **持续优化与反馈**：

+   在生产环境中，模型的表现会收到用户交互的影响，因此可以根据用户反馈进行进一步的微调和优化。

> 不使用明确的 instruction 会让模型在推理阶段更依赖上下文信息，而不是等待明确的指令提示。它可以使模型在连续对话生成任务中表现得更自然，但缺点是，模型可能在多任务环境中表现不如明确指令下那么灵活，因为它没有被明确告知应该做什么任务。

> 知识灌注（Knowledge Infusion）指的是将特定的知识明确灌输到模型中，在指令微调阶段也是可以进行知识灌注的，使得模型能够记住特定信息，并在任务中使用这些知识。例如，你想让模型知道自己的名字，或掌握一些特定领域的事实，这种知识可以在指令微调阶段注入。

> 知识灌注的可行性：

> 可以进行知识灌注：在指令微调阶段，确实可以通过有针对性的训练数据进行知识灌注。你可以提供包含这些知识的训练数据，通过上下文或明确指令提示模型，从而让模型学会特定的知识。例如，你可以在对话数据中加入这样的对话：

> 用户：你叫什么名字？

> 模型：我是 MateConv，一个智能对话助手。

> 通过这样的训练数据，模型会逐渐记住这些知识，并在推理时能够自发地根据这些提示进行正确的回答。

---

### 全量指令微调和微调之间的区别和联系

这其实是一个非常综合且重要的问题，尤其是**当我**们**在大规模**模型**微调的场景下**，**追求平衡**性**能与**资**源消**耗时。全量指令微调和高效微调（也叫参数高效微调）各自代表了不同的微调策略，它们有不同的应用场景和技术优点，但也存在一些相互联系。接下来我们将详细对比它们的区别和联系。

#### 一、全量指令微调（Full Instruction Tuning）

**全量指令微调**指的是对整个模型的所有参数进行微调。这种方法最常用于当你有充足的数据和计算资源，并且希望模型在一个特定任务或多个任务上达到最佳性能。

##### 特点：

1.  **微调所有模型参数**：

+   全量微调意味着所有的预训练参数都会进行调整。模型在新的指令任务上逐渐适应并优化所有权重，从而能够高效地处理特定任务。

1.  **适用于大规模数据和明确任务**：

+   适合处理大量的数据集，并且能通过多任务、多领域的指令数据微调模型。这种方法能够让模型最大限度地适应新的任务需求。

1.  **计算和存储开销大**：

+   由于模型的所有参数都会被更新，全量指令微调通常需要非常高的计算资源和存储空间，尤其是当你处理的是数十亿参数规模的大模型时。

1.  **微调后效果精确**：

+   全量微调可以保证模型在微调后的任务上有最好的表现，模型完全适应了新的指令数据，在目标任务上能够达到最高精度。

1.  **应用场景**：

+   通常应用在有足够计算资源的环境中，特别是需要极高性能、处理特定任务（如法律文本分析、医疗诊断等）的模型开发。

##### 优点：

+   高性能，任务适应能力强。

+   精确控制模型行为和生成效果。

##### 缺点：

+   计算资源和时间开销巨大。

+   训练耗费大量的显存和存储资源，部署和更新较为昂贵。

#### 二、高效微调（Parameter-Efficient Tuning）

**高效微调**（Parameter-Efficient Tuning）是一种在不更新全**部参数的情况下，选择性地对**模**型某些部分进行微调的策略。常**见的高效微调技术包括 LoRA（Low-Rank Adaptation）、Prefix Tuning、Adapter Tuning 等。

##### 特点：

1.  **微调部分参数**：

+   高效微调通过引入一些小型模块（如 Adapter 层、前缀）或调整模型的某些子结构（如低秩矩阵），只对这些部分进行微调，而不调整整个模型的所有权重。

1.  **减少显存和计算需求**：

+   因为只需要更新少量参数，高效微调显著降低了显存消耗和计算资源的需求。这使得即使在资源受限的环境下，也能微调大规模的预训练模型。

1.  **适用于低资源环境**：

+   高效微调非常适合数据集较小、计算资源有限的场景。例如，在嵌入式系统或边缘计算设备上部署大模型时，高效微调可以让模型在这些环境中高效运行。

1.  **模块化和可重用性**：

+   高效微调的另一个优点是其模块化。微调过程中引入的小型模块可以在不同的任务之间进行共享或转移，避免重复训练。例如，适用于某个领域的 Adapter 层可以在该领域的多个任务上重复使用。

1.  **微调后效果良好但不如全量微调**：

+   虽然高效微调可以达到很好的效果，但与全量指令微调相比，性能上略逊一筹。尤其是在对任务精度要求极高的场景下，可能需要全量微调来最大化模型的效果。

##### 优点：

+   计算资源、显存开销小，适合低资源环境。

+   训练效率高，适用于快速迭代和部署。

##### 缺点：

+   在特定高精度任务上，性能可能不如全量微调。

+   微调时引入的模块或层会增加模型的复杂度，需要额外管理。

#### 三、区别

1.  **参数调整范围**：

+   **全量指令微调**：微调所有模型参数，更新整个网络的权重。

+   **高效微调**：只微调模型中的部分参数或增加的模块，如 LoRA、Adapter 层，原有的大部分权重保持不变。

1.  **计算和显存需求**：

+   **全量指令微调**：显著增加计算和显存需求，尤其是对于数十亿参数规模的模型，要求非常高。

+   **高效微调**：显存需求低，适合在资源有限的环境中运行。只需要少量的参数更新，因此计算资源消耗更低。

1.  **性能表现**：

+   **全量指令微调**：在任务上的表现往往最佳，因为模型所有参数都被精细优化，可以最大化适应任务需求。

+   **高效微调**：性能表现虽然不错，但通常不及全量微调，尤其是在一些复杂或精细的任务上。

1.  **部署和模块化**：

+   **全量指令微调**：整个模型被微调，每次微调都需要重新部署整个模型，更新较为繁琐。

+   **高效微调**：只需要部署微调后的模块，模型的主体部分保持不变，方便快速迭代和更新。

1.  **训练时间**：

+   **全量指令微调**：训练时间较长，特别是在大规模模型上，因为需要更新大量的参数。

+   **高效微调**：训练时间相对较短，因为只需要微调少部分参数。

#### 四、联系

1.  **相同的目标**：

+   无论是全量指令微调还是高效微调，它们的最终目标都是让预训练模型适应特定任务，提高在目标任务上的表现。两者的区别更多在于实现的方式和资源的权衡。

1.  **可以结合使用**：

+   在某些场景下，可以结合全量指令微调和高效微调。例如，你可以先对整个模型进行全量指令微调，以确保模型的高精度表现，然后在低资源环境中应用高效微调以进一步优化模型，或者扩展到新任务中而不需要微调整个模型。

1.  **知识共享**：

+   高效微调中的一些方法（如 Adapter 模块）可以在不同任务之间共享，这些模块可以通过全量微调的模型基础上进一步进行高效微调，扩展到不同的任务领域。

#### 总结

+   **全量指令微调**是一个“昂贵”的但高效的微调方法，适合资源充足、需要高精度表现的场景，尤其是那些需要处理多个复杂任务的场景。

+   **高效微调**则是一个“轻量级”的方法，适合资源有限的场景，并且在需要快速部署或频繁更新时非常有用。它可以通过少量的参数更新，实现较好的模型性能。

#### 五、单轮对话和多轮对话指令微调的区别

##### 1. **上下文处理**

+   **单轮对话微调**：

+   模型只需要处理当前输入和输出之间的映射，不需要考虑上下文，也不需要保存或记住之前的对话内容。

+   微调数据可以是一些独立的指令，比如“生成一个回复”、“回答这个问题”等。

+   微调时，数据集中每个问题和回答都可以视作独立的样本，因此微调过程相对简单。

+   **多轮对话微调**：

+   模型需要在每一轮对话中跟踪和理解完整的对话历史，包括之前的问答信息。模型必须具备记忆和状态管理的能力。

+   微调时，数据集不仅需要包含每轮对话的输入和输出，还需要保留对话的上下文，使模型能够在生成回复时参考前文。

+   这类任务的微调更加复杂，因为模型需要学会如何在多个回合中保持对话的连贯性，避免前后矛盾。

##### 2. **任务复杂性**

+   **单轮对话微调**：

+   任务相对简单，通常用于一些固定回答的任务，例如问答系统、知识查询、单轮任务执行等。模型的输出只需对当前输入负责。

+   微调时，模型不需要处理复杂的对话逻辑，只需对每个输入生成最合适的响应。

+   **多轮对话微调**：

+   任务复杂度更高，尤其是在多回合对话中，模型需要处理上下文依赖、对话状态跟踪等问题。

+   微调时，数据集需要涵盖不同类型的对话情境，模型不仅要能生成单轮的合适回复，还要保证整个对话的流畅性和一致性。

##### 3. **模型训练需求**

+   **单轮对话微调**：

+   对训练数据的需求相对较少，数据集只需要包含独立的问题和回答。由于没有上下文依赖，数据样本的数量可以较小。

+   模型的生成质量主要取决于它在每次输入时生成单一答案的能力。

+   **多轮对话微调**：

+   多轮对话需要更丰富的训练数据，涵盖多种对话情境和长短不一的对话历史，以帮助模型学习如何处理复杂的上下文。

+   模型需要学习如何在多个回合中保持对话的上下文一致性，因此训练时间和资源需求更高。

---

+   两种主流的全量指令微调方法

从模型训练角度来说，指令微调远不如预训练复杂，在实际进行指令微调的过程中，可以选择使用transformer库的原生方法实现全量指令微调，也可以选择一些开源的项目如Llama-Factory进行快速指令微调。这里我们主要尝试使用transformer库的原生方法来执行全量指令微调。

+   指令微调数据集

不同于预训练数据集，需要海量、高质量、多样性的文本数据，才能训练得到一个流畅问答的大模型，全量指令微调对于数据集的要求相对较低，制作门槛也相对较低，其中Llama-Factory上有非常完整的目前可用于中英文指令微调的数据集：https://github.com/hiyouga/LLaMA-Factory/blob/main/README_zh.md#%E6%95%B0%E6%8D%AE%E9%9B%86

![](img/3836a36ce9f4166f299a13b434b24763.png)

这里我们选取数据规模相对较小的匠数科技的sft数据集。该数据集是一个是一个完整、格式统一、安全的大模型训练和研究资源。从网络上的公开数据源收集并整理了大量开源数据集，对其进行了格式统一，数据清洗，包含10M条数据的中文数据集和包含2M条数据的英文数据集。总量大约在3B token，适合小尺寸中文大语言模型进行指令微调：

![](img/b3737a4903f713f9288bc2badcb5b4a6.png)

匠数科技大模型sft数据集官方地址：https://www.modelscope.cn/datasets/deepctrl/deepctrl-sft-data 。

### 一、借助transformer原生方法完成指令指令微调

#### 1.指令微调数据集获取与数据集清洗

+   借助魔搭社区下载数据集

```
conda activate MateConv
pip install modelscope
```

![](img/3770c37910ede833b2e6d2b421350919.png)

```
cd /root/autodl-tmp/MateConv
modelscope download --dataset deepctrl/deepctrl-sft-data sft_data_zh.jsonl --local_dir ./dataset
```

![](img/05dcc229094011722c4e7dde3ff47ed6.png)

下载完成后即可在dataset文件夹内看到该文件：

![](img/186835fb69f35280c266dc5594b6a863.png)

网盘地址如下：

![](img/ee3f8082aef999cd4480c07682b8111c.png)

+   查看指令微调的数据文件

```
import jsonlines

# 查看JSONL文件的前几行
file_path = &#x27;./dataset/sft_data_zh.jsonl&#x27;

with jsonlines.open(file_path) as reader:
    for i, obj in enumerate(reader):
        print(obj)  # 打印每一行
        if i &gt;= 4:  # 只打印前5行
            break
```

```
{&#x27;id&#x27;: 1, &#x27;instruction&#x27;: &#x27;&#x27;, &#x27;input&#x27;: &#x27;好的。现在请你将这个文本中的所有的逗号都替换成空格。&#x27;, &#x27;output&#x27;: &#x27;好的，请稍等一下，现在我会将文本中的所有逗号替换为空格。处理后文本为：&quot;这是一个句子 目的是看看是否可以正确地从这个句子中删除关键词。&quot;。处理结果如何？&#x27;, &#x27;history&#x27;: [[&#x27;给定一段文本和关键词列表，删除文本中包含所有给定关键词的子字符串。\n文本：&quot;这是一个测试句子，目的是看看模型是否可以正确地从这个句子中删除关键词。&quot;\\n关键词列表：[‘测试’，‘模型’]&#x27;, &#x27;删除包含所有给定关键词的子字符串后，文本变为：&quot;这是一个句子，目的是看看是否可以正确地从这个句子中删除关键词。&quot;&#x27;]], &#x27;language&#x27;: &#x27;chinese&#x27;, &#x27;data_source&#x27;: &#x27;https://huggingface.co/datasets/BelleGroup/train_3.5M_CN&#x27;, &#x27;input_len&#x27;: 59, &#x27;output_len&#x27;: 66, &#x27;num_utter&#x27;: 2, &#x27;type&#x27;: 31, &#x27;type_keyword&#x27;: [&#x27;字符串&#x27;, &#x27;代码&#x27;, &#x27;函数&#x27;, &#x27;编写&#x27;, &#x27;实现&#x27;, &#x27;给定&#x27;, &#x27;使用&#x27;, &#x27;输入&#x27;, &#x27;文本&#x27;, &#x27;程序&#x27;]}
{&#x27;id&#x27;: 2, &#x27;instruction&#x27;: &#x27;&#x27;, &#x27;input&#x27;: &#x27;帮我回答一道历史题目。清朝时期的八旗共有多少旗人？&#x27;, &#x27;output&#x27;: &#x27;清朝时期八旗旗人总数约为200万人左右，其中正黄旗、正蓝旗、正白旗、正红旗的人数较多，其他旗的人数较少。&#x27;, &#x27;history&#x27;: [[&#x27;写一个关于秋天的诗吧。&#x27;, &#x27;红黄绿交织，田野金黄色。风吹枫叶落，人在秋天慢步行。&#x27;], [&#x27;给我推荐一些好听的摇滚歌曲。&#x27;, &quot;摇滚歌曲推荐：AC/DC的Back in Black，Guns N&#x27; Roses的Sweet Child O&#x27; Mine，Rolling Stones的Satisfaction，Led Zeppelin的Stairway to Heaven，Nirvana的Smells Like Teen Spirit等。&quot;], [&#x27;计算一下2的100次方。&#x27;, &#x27;2的100次方等于1.267650600228229401496703205376e+30，约等于1.27乘以10的30次方。&#x27;]], &#x27;language&#x27;: &#x27;chinese&#x27;, &#x27;data_source&#x27;: &#x27;https://huggingface.co/datasets/BelleGroup/train_3.5M_CN&#x27;, &#x27;input_len&#x27;: 15, &#x27;output_len&#x27;: 73, &#x27;num_utter&#x27;: 4, &#x27;type&#x27;: 11, &#x27;type_keyword&#x27;: [&#x27;生成&#x27;, &#x27;诗歌&#x27;, &#x27;主题&#x27;, &#x27;爱情&#x27;, &#x27;创作&#x27;, &#x27;描述&#x27;, &#x27;歌词&#x27;, &#x27;表达&#x27;, &#x27;歌曲&#x27;, &#x27;有关&#x27;]}
{&#x27;id&#x27;: 3, &#x27;instruction&#x27;: &#x27;&#x27;, &#x27;input&#x27;: &#x27;嗯，谢谢你介绍的做法很详细，但我不喜欢吃鸡蛋，有没有其他菜做法能介绍一下？&#x27;, &#x27;output&#x27;: &#x27;当然，你可以试试酸辣土豆丝这道菜。\n材料：\n土豆2个、红椒1个、青椒1个、大葱1根、醋、生抽、盐、鸡精、料酒\n做法：\n1.土豆去皮，切成丝；红椒和青椒切成细丝；大葱切段备用。\n2.热锅凉油，油热后放入土豆丝，煸炒至变软。\n3.倒入红椒、青椒和大葱段，继续煸炒至熟。\n4.加入适量的盐、鸡精、料酒和生抽，翻炒均匀。\n5.最后，加入适量的醋，翻炒均匀即可。\n小贴士：\n1. 土豆切丝时，可以放入淡盐水中泡一下，这样可以去除多余的淀粉。\n2. 煮土豆丝时，不要煮得太久，以免烂糊。\n3. 加入醋的时候，根据自己的口味多少来进行调节，一般来说，盐与醋的比例为1:1。\n4. 如果喜欢辣味可以加入一些干辣椒丝。\n希望你会喜欢这道酸辣土豆丝！&#x27;, &#x27;history&#x27;: [[&#x27;介绍一道好吃的家常菜的做法&#x27;, &#x27;番茄炒蛋\n材料：\n鸡蛋3个、番茄1个、油、盐、糖、水淀粉\n做法：\n1.番茄洗净切成小块；鸡蛋打入碗中搅匀备用。\n2.热锅凉油，油热后放入番茄翻炒。\n3.番茄煮软后放盐，糖调味，再倒入打好的鸡蛋液，翻炒均匀。\n4.鸡蛋凝固后关火。\n5.加入半碗水，大火煮开。\n6.用水淀粉勾芡，装盘即可。\n小贴士：\n1. 番茄不能切得太细，否则炒出来就不太好看了。\n2. 在调味时，糖的分量也很重要，可以根据自己的口味来调节。\n3. 水淀粉勾芡的比例是：水和淀粉的比例大概是5:1。\n4. 煮开之后，记得要把水淀粉慢慢倒入，同时不断地搅拌，以免结块。\n5. 最后装盘时可以点缀一些绿色蔬菜，既好看又好吃。&#x27;]], &#x27;language&#x27;: &#x27;chinese&#x27;, &#x27;data_source&#x27;: &#x27;https://huggingface.co/datasets/BelleGroup/train_3.5M_CN&#x27;, &#x27;input_len&#x27;: 25, &#x27;output_len&#x27;: 304, &#x27;num_utter&#x27;: 2, &#x27;type&#x27;: 47, &#x27;type_keyword&#x27;: [&#x27;制作&#x27;, &#x27;食谱&#x27;, &#x27;包括&#x27;, &#x27;描述&#x27;, &#x27;步骤&#x27;, &#x27;烹饪&#x27;, &#x27;提供&#x27;, &#x27;材料&#x27;, &#x27;食材&#x27;, &#x27;菜谱&#x27;]}
{&#x27;id&#x27;: 4, &#x27;instruction&#x27;: &#x27;&#x27;, &#x27;input&#x27;: &#x27;请描述一下如何正确规划个人理财。&#x27;, &#x27;output&#x27;: &#x27;正确规划个人理财需要以下几个步骤：\n1.了解自己的财务状况。这包括收入、支出、资产和负债等信息。了解自己的财务状况可以帮助人们更好地制定财务计划。\n2.设定财务目标。需要考虑短期目标和长期目标，例如以年为单位设定的支出计划、购房、购车等的长期目标。\n3.制定预算计划。在了解自己的财务状况并设定财务目标后，需要制定一个预算计划。这可以帮助人们控制支出、节省开支并达到财务目标。\n4.理性投资和储蓄。人们可以投资于股票、基金、房产或其他投资渠道以实现财务目标。但在投资前需了解相关知识并进行风险评估。同时还应储蓄一定金额，以应对突发事件或为达成某些目标做准备。\n5.审时度势，合理调整。财务计划需要不断地审时度势，根据实际情况做出调整，以达到最终的财务目标。需要注意财务状况的变化、投资的收益和风险等因素。\n通过以上五个步骤，人们可以做到合理规划个人理财，掌握自己的财务命运，更好地实现自己的财务目标。&#x27;, &#x27;history&#x27;: [], &#x27;language&#x27;: &#x27;chinese&#x27;, &#x27;data_source&#x27;: &#x27;https://huggingface.co/datasets/BelleGroup/train_3.5M_CN&#x27;, &#x27;input_len&#x27;: 16, &#x27;output_len&#x27;: 400, &#x27;num_utter&#x27;: 1, &#x27;type&#x27;: 26, &#x27;type_keyword&#x27;: [&#x27;提供&#x27;, &#x27;建议&#x27;, &#x27;问题&#x27;, &#x27;提高&#x27;, &#x27;方法&#x27;, &#x27;帮助&#x27;, &#x27;提出&#x27;, &#x27;进行&#x27;, &#x27;解决&#x27;, &#x27;给出&#x27;]}
{&#x27;id&#x27;: 5, &#x27;instruction&#x27;: &#x27;&#x27;, &#x27;input&#x27;: &#x27;描述一下天堂和地狱的生态系统和环境。&#x27;, &#x27;output&#x27;: &#x27;天堂和地狱被认为是灵性信仰中关于死后世界的两种不同概念。然而，它们的生态系统和环境都是具有类似特征的极端不同的地方。以下是我对天堂和地狱的生态系统和环境的描述。\n天堂的生态系统和环境:\n天堂被描绘为一个美丽、平静、和谐的地方，类似于一片无垢的花园。天堂的生态系统和环境的特征包括:\n1. 充满和平和爱的氛围。这是一个没有恐惧、痛苦、疾病和死亡的地方。\n2. 色彩缤纷，充满生机。这是一个绿树成荫、花团锦簇的地方，充满生机和活力。\n3. 各种生物和动物和谐共存。天使、圣人和各种动物和谐相处，生态系统中没有互相侵害或抢夺资源。\n4. 充满清新气息的空气。没有污染、烟雾或其他有害物质，空气中充满清新芬芳的气息。\n5. 物质丰富的环境。天堂中生活着满足需求和愿望的人们，他们拥有一切所需的物质资源，而且没有匮乏、浪费或不公平。\n地狱的生态系统和环境:\n地狱被描绘为阴暗、恐怖、嘈杂和可怕的地方。地狱的生态系统和环境的特征包括:\n1. 充满痛苦和折磨的氛围。这是一个充满恐惧、悔恨和痛苦的地方，全是罪恶的味道。\n2. 火焰和烈火环绕。地狱中有燃烧的火焰和烈火，许多受罚者被投入火坑中痛苦折磨。\n3. 恶魔和妖魔横行。地狱中有恶魔、妖怪等可怕的生物，它们在无休止的受苦中享受着自己的又一场比赛。\n4. 污染和恶臭的气味。地狱中到处都是恶臭和污染，没有清新的气息。\n5. 没有物质资源。地狱中生活着被惩罚的人们不可能拥有任何物质财富，地狱环境充满了无尽的贫困、饥饿和疾病。\n综上所述，天堂和地狱是两个完全不同的地方，它们的生态系统和环境反映了它们的性质，体现了人类对不同阶段的死后生命的不同想象和信仰。&#x27;, &#x27;history&#x27;: [], &#x27;language&#x27;: &#x27;chinese&#x27;, &#x27;data_source&#x27;: &#x27;https://huggingface.co/datasets/BelleGroup/train_3.5M_CN&#x27;, &#x27;input_len&#x27;: 18, &#x27;output_len&#x27;: 696, &#x27;num_utter&#x27;: 1, &#x27;type&#x27;: 23, &#x27;type_keyword&#x27;: [&#x27;解释&#x27;, &#x27;描述&#x27;, &#x27;概念&#x27;, &#x27;提供&#x27;, &#x27;包括&#x27;, &#x27;说明&#x27;, &#x27;给出&#x27;, &#x27;应用&#x27;, &#x27;介绍&#x27;, &#x27;原理&#x27;]}
```

其中每条数据的数据格式如下：

![](img/0b89e2fbf818df04e03796dbd2de3c60.png)

而其中我们只需要提取对话的信息即可：将符合条件的部分保存到一个 CSV 文件中。整个过程涉及到数据的筛选、清洗、处理和写入。具体步骤如下：

1.  **选择输入文件**：

+   根据参数 contain_history，确定要处理的数据集文件名为 'sft_data.csv' 或 'sft_data_single.csv'。

1.  **文本筛选函数**：

+   chinese_ratio(text)：计算一个文本中中文字符的比例，用来筛选是否大部分内容是中文。

1.  **处理数据并写入 CSV**：

+   process_and_write_data(data)：对数据进行筛选，检查每一条对话是否符合以下条件：

+   如果 contain_history 为真，则数据必须有对话历史。

+   对话的问句 (q) 和答句 (a) 都必须存在，并且它们的长度要在指定范围内（问句长度在 10 到 256 之间，答句长度在 5 到 256 之间）。

+   问句和答句的中文字符比例必须大于 90%。

+   如果数据符合条件，则将问句、答句和对话历史（如果有的话）添加到列表中，并写入 CSV 文件。

1.  **逐行读取 JSONL 数据集**：

+   sft_datasets 包含了一个要处理的 JSONL 数据集文件路径。

+   使用 jsonlines 库逐行读取数据，并将问答对保存在 data 列表中。每当 data 列表中累计了 1000 条记录（chunk_size），就将数据进行处理并写入到 CSV 文件中。

+   如果读取的行有格式问题，代码会跳过并继续处理下一行。

1.  **进度条**：

+   使用 tqdm 库为整个处理过程添加进度条，显示处理数据的进度，并在每处理 chunk_size 条数据后更新进度。

1.  **输出结果**：

+   生成的 CSV 文件包括三个列：history（对话历史）、q（问句）、a（答句）。

```
import csv
import itertools
import re
import json
import jsonlines
import psutil
import ujson
import numpy as np
import pandas as pd
from transformers import AutoTokenizer
from datasets import load_dataset
from tqdm import tqdm
```

```
bos_token = &quot;&lt;s&gt;&quot;
eos_token = &quot;&lt;/s&gt;&quot;
```

```
tokenizer = AutoTokenizer.from_pretrained(&#x27;./model/mateconv_tokenizer&#x27;, use_fast=False)
print(&#x27;tokenizer词表大小：&#x27;, len(tokenizer))
```

```
tokenizer词表大小： 6400
```

```
from tqdm import tqdm  # 导入 tqdm

def sft_process(contain_history=False):
    file_name = &#x27;sft_data.csv&#x27; if contain_history else &#x27;sft_data_single.csv&#x27;

    def chinese_ratio(text):
        &quot;&quot;&quot;计算文本中中文字符的比例。&quot;&quot;&quot;
        chinese_chars = re.findall(r&#x27;[\u4e00-\u9fff]&#x27;, text)  # 匹配中文字符
        return len(chinese_chars) / len(text) if text else 0

    def process_and_write_data(data):
        &quot;&quot;&quot;处理数据并将其写入 CSV 文件。&quot;&quot;&quot;
        q_lst, a_lst, history_lst = [], [], []
        for per in data:
            history, q, a = per[&#x27;history&#x27;], per[&#x27;q&#x27;], per[&#x27;a&#x27;]

            # 数据筛选条件
            if (contain_history and not history) or not q or not a:
                continue
            if len(q) &lt; 10 or len(a) &lt; 5:
                continue
            if len(q) &gt; 256 or len(a) &gt; 256:
                continue
            if not (chinese_ratio(q) &gt; 0.9 and chinese_ratio(a) &gt; 0.9):
                continue

            # 将有效数据添加到列表
            q_lst.append(q)
            a_lst.append(a)
            if contain_history:
                history_lst.append(history)
            else:
                history_lst.append([])

        # 创建 DataFrame 并写入 CSV 文件
        df = pd.DataFrame({&#x27;history&#x27;: history_lst, &#x27;q&#x27;: q_lst, &#x27;a&#x27;: a_lst})
        df.to_csv(f&#x27;./dataset/{file_name}&#x27;, mode=&#x27;a&#x27;, header=False, index=False, 
                   lineterminator=&#x27;\r\n&#x27;, escapechar=&#x27;\\&#x27;, quoting=csv.QUOTE_MINIMAL)

    chunk_size = 1000  # 每次处理的记录数
    data = []

    # 创建 CSV 文件并写入表头
    with open(f&#x27;./dataset/{file_name}&#x27;, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        f.write(&#x27;history,q,a\n&#x27;)

    sft_datasets = [&#x27;./dataset/sft_data_zh.jsonl&#x27;]
    
    # 开始处理数据集
    for path in sft_datasets:
        with jsonlines.open(path) as reader:
            total_lines = sum(1 for _ in open(path))  # 获取总行数
            
            # 使用 tqdm 创建进度条
            with tqdm(total=total_lines, desc=&quot;Processing lines&quot;) as pbar:
                for idx, obj in enumerate(reader):
                    try:
                        data.append({
                            &#x27;history&#x27;: obj.get(&#x27;history&#x27;, &#x27;&#x27;),
                            &#x27;q&#x27;: obj.get(&#x27;input&#x27;, &#x27;&#x27;) + obj.get(&#x27;q&#x27;, &#x27;&#x27;),
                            &#x27;a&#x27;: obj.get(&#x27;output&#x27;, &#x27;&#x27;) + obj.get(&#x27;a&#x27;, &#x27;&#x27;)
                        })

                        # 每达到 chunk_size，就处理并写入数据
                        if len(data) &gt;= chunk_size:
                            process_and_write_data(data)
                            data = []
                            pbar.update(chunk_size)  # 更新进度条

                    except jsonlines.InvalidLineError as e:
                        print(f&quot;Skipping invalid JSON line {idx + 1}: {e}&quot;)
                        continue

                # 处理剩余的数据
                if data:
                    process_and_write_data(data)
                    pbar.update(len(data))  # 更新进度条

    print(&quot;数据处理完成！&quot;)
```

```
sft_process(contain_history=False)
```

```
Processing lines: 100%|██████████| 11381621/11381621 [02:59&lt;00:00, 63243.30it/s]

数据处理完成！
```

查看处理后的数据文件：

```
import pandas as pd

# 加载 CSV 文件
file_path = &#x27;./dataset/sft_data_single.csv&#x27;  # 确保路径正确

# 使用 pandas 读取前 5 行
df = pd.read_csv(file_path, nrows=5)

# 显示前 5 行数据
print(df)
```

```
  history                                q  \
0      []           请从这篇文章中提取出关于分类垃圾的解决方案。   
1      []  根据以下文本，对此事件进行分类：中国队在足球比赛中赢得了冠军。   
2      []                     请告诉我什么是机器学习。   
3      []                    请解释一下什么是人工智能。   
4      []           对给定的命题进行真假判断\n梦是一种真实经验   

                                                   a  
0  文章中提到的第二个方面——分类垃圾，是解决垃圾污染的有效措施之一。垃圾分类是指将生活垃圾按照...  
1  这个事件可以被分类为体育比赛。具体地，中国队在足球比赛中致胜并赢得了冠军。由此可以推断出，这...  
2  机器学习是一种人工智能技术，它使用算法和数学模型，让计算机自动地从数据中学习，并根据这些数据...  
3  人工智能是一种模拟人类智能的技术和方法。它的发展包括机器学习、自然语言处理、计算机视觉等技术...  
4  这个命题是错误的。尽管梦境中的经验可以让我们产生强烈的情感反应，但梦并不是一种真实经验，它是...  
```

![](img/27ccf18a6e3574927d06b85dbf1232b7.png)

> 注，课件网盘中还提供了一个同名的csv文件，该文件是包含了一部分英文对话的指令微调数据集，相对上述代码创建的数据集更大。课上实验选择其中一个即可。

![](img/b917ad8341e5cc0d57d80dd9f18c1c8f.png)

#### 2.指令微调代码编写与代码解释

+   指令微调代码

在准备好数据集之后，接下来即可开始指令微调。首先我们需要编写一个指令微调的脚本文件full_sft.py其中代码如下：

```
import os
import platform
import argparse
import time
import math
import warnings

import pandas as pd
import torch
import torch.nn.functional as F
import torch.distributed as dist
from contextlib import nullcontext

from torch import optim
from torch.nn.parallel import DistributedDataParallel
from torch.utils.data import DataLoader, DistributedSampler
from transformers import AutoTokenizer, AutoModel
from model.model import Transformer
from model.LMConfig import LMConfig
from model.dataset import SFTDataset

warnings.filterwarnings(&#x27;ignore&#x27;)


def Logger(content):
    if not ddp or dist.get_rank() == 0:
        print(content)


def get_lr(it, all):
    warmup_iters = args.warmup_iters
    lr_decay_iters = all
    min_lr = args.learning_rate / 10

    if it &lt; warmup_iters:
        return args.learning_rate * it / warmup_iters
    if it &gt; lr_decay_iters:
        return min_lr
    decay_ratio = (it - warmup_iters) / (lr_decay_iters - warmup_iters)
    assert 0 &lt;= decay_ratio &lt;= 1
    coeff = 0.5 * (1.0 + math.cos(math.pi * decay_ratio))
    return min_lr + coeff * (args.learning_rate - min_lr)


def train_epoch(epoch, wandb, start_step=0):
    start_time = time.time()
    for step, (X, Y, loss_mask) in enumerate(train_loader, start=start_step):  # 从 start_step 开始
        X = X.to(args.device)
        Y = Y.to(args.device)
        loss_mask = loss_mask.to(args.device)
        lr = get_lr(epoch * iter_per_epoch + step, args.epochs * iter_per_epoch)
        for param_group in optimizer.param_groups:
            param_group[&#x27;lr&#x27;] = lr

        with ctx:
            logits = model(X, Y).logits
            loss = F.cross_entropy(logits.view(-1, logits.size(-1)), Y.view(-1), ignore_index=0, reduction=&#x27;none&#x27;)
            loss_mask = loss_mask.view(-1)
            loss = torch.sum(loss * loss_mask) / loss_mask.sum()

        scaler.scale(loss).backward()

        if (step + 1) % args.accumulation_steps == 0:
            scaler.unscale_(optimizer)
            torch.nn.utils.clip_grad_norm_(model.parameters(), args.grad_clip)

            scaler.step(optimizer)
            scaler.update()

            optimizer.zero_grad(set_to_none=True)

        if step % args.log_interval == 0:
            spend_time = time.time() - start_time
            Logger(
                &#x27;Epoch:[{}/{}]({}/{}) loss:{:.3f} lr:{:.7f} epoch_Time:{}min:&#x27;.format(
                    epoch,
                    args.epochs,
                    step,
                    iter_per_epoch,
                    loss.item(),
                    optimizer.param_groups[-1][&#x27;lr&#x27;],
                    spend_time / (step + 1) * iter_per_epoch // 60 - spend_time // 60))

            if (wandb is not None) and (not ddp or dist.get_rank() == 0):
                wandb.log({&quot;loss&quot;: loss,
                           &quot;lr&quot;: optimizer.param_groups[-1][&#x27;lr&#x27;],
                           &quot;epoch_Time&quot;: spend_time / (step + 1) * iter_per_epoch // 60 - spend_time // 60})

        if (step + 1) % args.save_interval == 0 and (not ddp or dist.get_rank() == 0):
            model.eval()
            moe_path = &#x27;_moe&#x27; if lm_config.use_moe else &#x27;&#x27;
            ckp = f&#x27;{args.save_dir}/full_sft_{lm_config.dim}{moe_path}.pth&#x27;

            # 保存模型、优化器和其他状态
            checkpoint = {
                &#x27;model_state&#x27;: model.module.state_dict() if isinstance(model, torch.nn.parallel.DistributedDataParallel) else model.state_dict(),
                &#x27;optimizer_state&#x27;: optimizer.state_dict(),
                &#x27;scaler_state&#x27;: scaler.state_dict(),
                &#x27;epoch&#x27;: epoch,
                &#x27;step&#x27;: step,
                &#x27;args&#x27;: vars(args)  # 保存训练的参数
            }

            torch.save(checkpoint, ckp)
            Logger(f&quot;Checkpoint saved at {ckp}&quot;)
            model.train()

            
def load_checkpoint(checkpoint_path, model, optimizer, scaler):
    if os.path.exists(checkpoint_path):
        Logger(f&quot;Loading checkpoint from {checkpoint_path}&quot;)
        checkpoint = torch.load(checkpoint_path, map_location=args.device)
        
        model.load_state_dict(checkpoint[&#x27;model_state&#x27;])
        optimizer.load_state_dict(checkpoint[&#x27;optimizer_state&#x27;])
        scaler.load_state_dict(checkpoint[&#x27;scaler_state&#x27;])
        start_epoch = checkpoint[&#x27;epoch&#x27;] + 1  # 从上次的epoch继续
        start_step = checkpoint[&#x27;step&#x27;] + 1    # 从上次的step继续

        return start_epoch, start_step
    else:
        Logger(f&quot;No checkpoint found at {checkpoint_path}, starting from scratch.&quot;)
        return 0, 0  # 如果没有找到检查点，从头开始


def init_model():
    tokenizer = AutoTokenizer.from_pretrained(&#x27;./model/mateconv_tokenizer&#x27;)
    model_from = 1  # 1从权重，2用transformers

    def count_parameters(model):
        return sum(p.numel() for p in model.parameters() if p.requires_grad)

    if model_from == 1:
        model = Transformer(lm_config)
        moe_path = &#x27;_moe&#x27; if lm_config.use_moe else &#x27;&#x27;
        ckp = f&#x27;./out/pretrain_{lm_config.dim}{moe_path}.pth&#x27;
        state_dict = torch.load(ckp, map_location=args.device)
        unwanted_prefix = &#x27;_orig_mod.&#x27;
        for k, v in list(state_dict.items()):
            if k.startswith(unwanted_prefix):
                state_dict[k[len(unwanted_prefix):]] = state_dict.pop(k)
        model.load_state_dict(state_dict, strict=False)
    else:
        model = AutoModel.from_pretrained(&#x27;./MateConv&#x27;, trust_remote_code=True)

    Logger(f&#x27;LLM总参数量：{count_parameters(model) / 1e6:.3f} 百万&#x27;)
    model = model.to(args.device)

    return model, tokenizer


def init_distributed_mode():
    if not ddp: return
    global ddp_local_rank, DEVICE

    dist.init_process_group(backend=&quot;nccl&quot;)
    ddp_rank = int(os.environ[&quot;RANK&quot;])
    ddp_local_rank = int(os.environ[&quot;LOCAL_RANK&quot;])
    ddp_world_size = int(os.environ[&quot;WORLD_SIZE&quot;])
    DEVICE = f&quot;cuda:{ddp_local_rank}&quot;
    torch.cuda.set_device(DEVICE)


if __name__ == &quot;__main__&quot;:
    parser = argparse.ArgumentParser(description=&quot;MateConv Full SFT&quot;)
    parser.add_argument(&quot;--out_dir&quot;, type=str, default=&quot;out&quot;, help=&quot;Output directory&quot;)
    parser.add_argument(&quot;--epochs&quot;, type=int, default=19, help=&quot;Number of epochs&quot;)
    parser.add_argument(&quot;--batch_size&quot;, type=int, default=32, help=&quot;Batch size&quot;)
    parser.add_argument(&quot;--learning_rate&quot;, type=float, default=1e-4, help=&quot;Learning rate&quot;)
    parser.add_argument(&quot;--device&quot;, type=str, default=&quot;cuda:0&quot; if torch.cuda.is_available() else &quot;cpu&quot;, help=&quot;Device to use&quot;)
    parser.add_argument(&quot;--dtype&quot;, type=str, default=&quot;bfloat16&quot;, help=&quot;Data type&quot;)
    parser.add_argument(&quot;--use_wandb&quot;, action=&quot;store_true&quot;, help=&quot;Use Weights &amp; Biases&quot;)
    parser.add_argument(&quot;--wandb_project&quot;, type=str, default=&quot;MateConv-Full-SFT&quot;, help=&quot;Weights &amp; Biases project name&quot;)
    parser.add_argument(&quot;--num_workers&quot;, type=int, default=8, help=&quot;Number of workers for data loading&quot;)
    parser.add_argument(&quot;--ddp&quot;, action=&quot;store_true&quot;, help=&quot;Use DistributedDataParallel&quot;)
    parser.add_argument(&quot;--accumulation_steps&quot;, type=int, default=1, help=&quot;Gradient accumulation steps&quot;)
    parser.add_argument(&quot;--grad_clip&quot;, type=float, default=1.0, help=&quot;Gradient clipping threshold&quot;)
    parser.add_argument(&quot;--warmup_iters&quot;, type=int, default=0, help=&quot;Number of warmup iterations&quot;)
    parser.add_argument(&quot;--log_interval&quot;, type=int, default=100, help=&quot;Logging interval&quot;)
    parser.add_argument(&quot;--save_interval&quot;, type=int, default=1000, help=&quot;Model saving interval&quot;)
    parser.add_argument(&#x27;--local_rank&#x27;, type=int, default=-1, help=&#x27;local rank for distributed training&#x27;)

    args = parser.parse_args()

    lm_config = LMConfig()
    max_seq_len = lm_config.max_seq_len
    args.save_dir = os.path.join(args.out_dir)
    os.makedirs(args.save_dir, exist_ok=True)
    os.makedirs(args.out_dir, exist_ok=True)
    tokens_per_iter = args.batch_size * max_seq_len
    torch.manual_seed(1337)
    device_type = &quot;cuda&quot; if &quot;cuda&quot; in args.device else &quot;cpu&quot;

    args.wandb_run_name = f&quot;MateConv-Full-SFT-Epoch-{args.epochs}-BatchSize-{args.batch_size}-LearningRate-{args.learning_rate}&quot;

    ctx = nullcontext() if device_type == &quot;cpu&quot; else torch.cuda.amp.autocast()
    ddp = int(os.environ.get(&quot;RANK&quot;, -1)) != -1  # is this a ddp run?
    ddp_local_rank, DEVICE = 0, &quot;cuda:0&quot;
    if ddp:
        init_distributed_mode()
        args.device = torch.device(DEVICE)

    if args.use_wandb and (not ddp or ddp_local_rank == 0):
        import wandb
        wandb.init(project=args.wandb_project, name=args.wandb_run_name)
    else:
        wandb = None

    model, tokenizer = init_model()

    # 加载数据集
    df = pd.read_csv(&#x27;./dataset/sft_data_single.csv&#x27;)
    df = df.sample(frac=1.0)
    train_ds = SFTDataset(df, tokenizer, max_length=max_seq_len)
    train_sampler = DistributedSampler(train_ds) if ddp else None
    train_loader = DataLoader(
        train_ds,
        batch_size=args.batch_size,
        pin_memory=True,
        drop_last=False,
        shuffle=False,
        num_workers=args.num_workers,
        sampler=train_sampler
    )

    # 初始化优化器和scaler
    scaler = torch.cuda.amp.GradScaler(enabled=(args.dtype in [&#x27;float16&#x27;, &#x27;bfloat16&#x27;]))
    optimizer = optim.Adam(model.parameters(), lr=args.learning_rate)

    # 检查点路径
    moe_path = &#x27;_moe&#x27; if lm_config.use_moe else &#x27;&#x27;
    checkpoint_path = f&#x27;{args.save_dir}/full_sft_{lm_config.dim}{moe_path}.pth&#x27;

    # 加载检查点，恢复训练
    start_epoch, start_step = load_checkpoint(checkpoint_path, model, optimizer, scaler)

    # 编译模型（如果适用）
    if False and not lm_config.use_moe and platform.system() != &#x27;Windows&#x27; and float(torch.__version__.split(&#x27;.&#x27;)[0]) &gt;= 2:
        Logger(&quot;compiling the model... (takes a ~minute)&quot;)
        unoptimized_model = model
        model = torch.compile(model)

    # 分布式训练设置
    if ddp:
        model._ddp_params_and_buffers_to_ignore = {&quot;pos_cis&quot;}
        model = DistributedDataParallel(model, device_ids=[ddp_local_rank])

    # 每个epoch的迭代次数
    iter_per_epoch = len(train_loader)

    # 开始训练
    for epoch in range(start_epoch, args.epochs):
        train_epoch(epoch, wandb, start_step)
        start_step = 0  # 从第二个 epoch 开始，step 重新从 0 开始
```

+   指令微调代码解释

接下来，我们分部分解释代码的主要功能和逻辑：

#### 1. **导入依赖库**

```
import os
import platform
import argparse
import time
import math
import warnings
import pandas as pd
import torch
import torch.nn.functional as F
import torch.distributed as dist
from contextlib import nullcontext

from torch import optim
from torch.nn.parallel import DistributedDataParallel
from torch.utils.data import DataLoader, DistributedSampler
from transformers import AutoTokenizer, AutoModel
from model.model import Transformer
from model.LMConfig import LMConfig
from model.dataset import SFTDataset
```

+   这部分导入了常见的库，如 os、time、pandas，以及用于深度学习训练的 PyTorch 库。

+   主要导入了用于加载分词器和模型的 transformers 库（Hugging Face 的工具），以及自定义的 Transformer 模型、配置文件 LMConfig 和数据集处理模块 SFTDataset。

#### 2. **日志记录函数 ****Logger**

```
def Logger(content):
    if not ddp or dist.get_rank() == 0:
        print(content)
```

+   **功能**：这个函数用于在分布式训练中，只在主进程上输出日志内容（因为在分布式环境中，可能会有多个进程执行同样的操作）。

+   **dist.get_rank()**：返回当前进程的 rank，只有 rank 为 0 的进程（主节点）才会输出日志。

#### 3. **学习率调度函数 ****get_lr**

```
def get_lr(it, all):
    warmup_iters = args.warmup_iters
    lr_decay_iters = all
    min_lr = args.learning_rate / 10

    if it &lt; warmup_iters:
        return args.learning_rate * it / warmup_iters
    if it &gt; lr_decay_iters:
        return min_lr
    decay_ratio = (it - warmup_iters) / (lr_decay_iters - warmup_iters)
    coeff = 0.5 * (1.0 + math.cos(math.pi * decay_ratio))
    return min_lr + coeff * (args.learning_rate - min_lr)
```

+   **功能**：实现**余弦退火学习率调度**（Cosine Annealing Learning Ra**te）。在训**练的早期，学习率先逐步升高（warmup），然后在训练后期逐步减小。

+   这种调度策略有助于模型在训练中快速收敛，同时避免后期训练时步长过大导致不稳定。

#### 4. **模型训练函数 ****train_epoch**

```
def train_epoch(epoch, wandb):
    start_time = time.time()
    for step, (X, Y, loss_mask) in enumerate(train_loader):
        X = X.to(args.device)
        Y = Y.to(args.device)
        loss_mask = loss_mask.to(args.device)
        lr = get_lr(epoch * iter_per_epoch + step, args.epochs * iter_per_epoch)
        for param_group in optimizer.param_groups:
            param_group[&#x27;lr&#x27;] = lr

        with ctx:
            logits = model(X, Y).logits
            loss = F.cross_entropy(logits.view(-1, logits.size(-1)), Y.view(-1), ignore_index=0, reduction=&#x27;none&#x27;)
            loss_mask = loss_mask.view(-1)
            loss = torch.sum(loss * loss_mask) / loss_mask.sum()

        scaler.scale(loss).backward()

        if (step + 1) % args.accumulation_steps == 0:
            scaler.unscale_(optimizer)
            torch.nn.utils.clip_grad_norm_(model.parameters(), args.grad_clip)

            scaler.step(optimizer)
            scaler.update()

            optimizer.zero_grad(set_to_none=True)

        if step % args.log_interval == 0:
            spend_time = time.time() - start_time
            Logger(
                &#x27;Epoch:[{}/{}]({}/{}) loss:{:.3f} lr:{:.7f} epoch_Time:{}min:&#x27;.format(
                    epoch,
                    args.epochs,
                    step,
                    iter_per_epoch,
                    loss.item(),
                    optimizer.param_groups[-1][&#x27;lr&#x27;],
                    spend_time / (step + 1) * iter_per_epoch // 60 - spend_time // 60))

            if (wandb is not None) and (not ddp or dist.get_rank() == 0):
                wandb.log({&quot;loss&quot;: loss,
                           &quot;lr&quot;: optimizer.param_groups[-1][&#x27;lr&#x27;],
                           &quot;epoch_Time&quot;: spend_time / (step + 1) * iter_per_epoch // 60 - spend_time // 60})

        if (step + 1) % args.save_interval == 0 and (not ddp or dist.get_rank() == 0):
            model.eval()
            moe_path = &#x27;_moe&#x27; if lm_config.use_moe else &#x27;&#x27;
            ckp = f&#x27;{args.save_dir}/full_sft_{lm_config.dim}{moe_path}.pth&#x27;

            if isinstance(model, torch.nn.parallel.DistributedDataParallel):
                state_dict = model.module.state_dict()
            else:
                state_dict = model.state_dict()

            torch.save(state_dict, ckp)
            model.train()
```

+   **核心功能**：执行模型的单轮训练，计算损失并更新模型参数。

+   **重要步骤**：

+   **学习率更新**：通过 get_lr 动态调整学习率。

+   **损失计算**：使用 cross_entropy 损失函数，忽略 ignore_index=0（常用于忽略填充的 token）。

+   **梯度累积**：通过 args.accumulation_steps 控制梯度累积，适用于较大模型的训练。

+   **梯度裁剪**：使用 clip_grad_norm_ 限制梯度的最大范数，防止梯度爆炸。

+   **保存检查点**：定期保存模型权重到 .pth 文件中，支持恢复训练。

#### 5. **模型初始化 ****init_model**

```
def init_model():
    tokenizer = AutoTokenizer.from_pretrained(&#x27;./model/mateconv_tokenizer&#x27;)
    model_from = 1  # 1从权重，2用transformers

    def count_parameters(model):
        return sum(p.numel() for p in model.parameters() if p.requires_grad)

    if model_from == 1:
        model = Transformer(lm_config)
        moe_path = &#x27;_moe&#x27; if lm_config.use_moe else &#x27;&#x27;
        ckp = f&#x27;./out/pretrain_{lm_config.dim}{moe_path}.pth&#x27;
        state_dict = torch.load(ckp, map_location=args.device)
        unwanted_prefix = &#x27;_orig_mod.&#x27;
        for k, v in list(state_dict.items()):
            if k.startswith(unwanted_prefix):
                state_dict[k[len(unwanted_prefix):]] = state_dict.pop(k)
        model.load_state_dict(state_dict, strict=False)
    else:
        model = AutoModel.from_pretrained(&#x27;./MateConv&#x27;, trust_remote_code=True)

    Logger(f&#x27;LLM总参数量：{count_parameters(model) / 1e6:.3f} 百万&#x27;)
    model = model.to(args.device)

    return model, tokenizer
```

+   **功能**：加载模型和分词器，支持从不同源加载模型（如自定义模型权重或 Hugging Face 的 AutoModel）。

+   **模型权重加载**：从 ./out 目录中加载预训练权重（.pth 文件），并加载到 Transformer 模型中。

+   **参数统计**：计算模型的总参数量并输出。

#### 6. **分布式训练初始化 **init_distributed_mode

```
def init_distributed_mode():
    if not ddp: return
    global ddp_local_rank, DEVICE

    dist.init_process_group(backend=&quot;nccl&quot;)
    ddp_rank = int(os.environ[&quot;RANK&quot;])
    ddp_local_rank = int(os.environ[&quot;LOCAL_RANK&quot;])
    ddp_world_size = int(os.environ[&quot;WORLD_SIZE&quot;])
    DEVICE = f&quot;cuda:{ddp_local_rank}&quot;
    torch.cuda.set_device(DEVICE)
```

+   **功能**：初始化分布式训练环境，使用 NCCL 后端进行 GPU 通信。适用于多 GPU 或多节点训练。

#### 7. **主流程**

```
if __name__ == &quot;__main__&quot;:
    parser = argparse.ArgumentParser(description=&quot;MateConv Full SFT&quot;)
    ...
    args = parser.parse_args()
    ...
    model, tokenizer = init_model()
    ...
    train_ds = SFTDataset(df, tokenizer, max_length=max_seq_len)
    train_loader = DataLoader(...)
    ...
    scaler = torch.cuda.amp.GradScaler(enabled=(args.dtype in [&#x27;float16&#x27;, &#x27;bfloat16&#x27;]))
    optimizer = optim.Adam(model.parameters(), lr=args.learning_rate)
    ...
    for epoch in range(args.epochs):
        train_epoch(epoch, wandb)
```

+   **命令行参数解析**：使用 argparse 解析训练相关参数（如学习率、

批次大小等）。

+   **数据加载**：使用 SFTDataset 处理数据集，DataLoader 负责将数据批量化用于训练。

+   **自动混合精度训练**：通过 torch.cuda.amp.GradScaler 实现半精度训练（如 float16），以减少显存占用。

+   **训练循环**：遍历训练的 epoch，调用 train_epoch 进行模型更新。

+   代码保存

编写完代码后，将其保存在项目主目录下：

![](img/b6b3749713c7d3d6963e77a7d0b0f577.png)

#### 3.全量指令微调训练流程

+   开启全量指令微调

> 需要注意的是，当前脚本是从固定路径下读取质量微调数据，所以在开始脚本之前，要再次确认指令微调数据是否已经创建完成：

![](img/07690b09346eabfa9856b7fe4bdb0d2b.png)

```
deepspeed --master_port 29500 --num_gpus=2 full_sft.py --out_dir out --epochs 5 --use_wandb --wandb_project &quot;MateConv-SFT&quot;
```

![](img/1c9c9961fe26ccff14c5a971a05c325c.png)

并且可以在wandb中查看训练效果：

![](img/0880661ebadf0571e6ebd32c5ac52959.png)

+   中断后继续训练

和预训练一样，全量微调的代码也是支持中断后继续训练的，并且和预训练一样，默认情况下是训练1000步保存一次：

![](img/33ff10e309e9c320f1241fbec97f0ae7.png)

同时会保存模型参数如下：

![](img/d0fe57b7ae1c808ff5875bdebfa545e6.png)

此外，也可以继续使用上一小节介绍的screen工具来持久化会话。

+   训练结束

![](img/926bd19aca31855c24bb8f632b29fc47.png)

+   768模型全量指令微调

首先需要确认out文件夹内存在pretrain_768.pth预训练模型：

![](img/b28105ca5054fa86d0e0fb24f53fde5e.png)

然后修改./model/LMConfig.py内容如下：

![](img/5b8b2cd767d4fe483442e0c93e506305.png)

然后直接运行脚本即可：

```
deepspeed --master_port 29500 --num_gpus=2 full_sft.py --out_dir out --epochs 5 --use_wandb --wandb_project &quot;MateConv-SFT&quot;
```

![](img/c5ac8ddab88ef3bca52de0f791dc81a8.png)

### 二、模型对话效果测试与高层模型调用API封装

在完成了模型指令微调之后，接下来即可测试模型对话效果。

#### 1.使用transformer库进行模型推理

+   对话测试

```
import torch
import random
import numpy as np
from transformers import AutoTokenizer
from model.model import Transformer
from model.LMConfig import LMConfig
```

```
/root/miniconda3/envs/MateConv/lib/python3.10/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
  from .autonotebook import tqdm as notebook_tqdm
```

```
# 1. 设置设备和随机种子
device = torch.device(&quot;cuda&quot; if torch.cuda.is_available() else &quot;cpu&quot;)

def setup_seed(seed):
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    torch.cuda.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False

setup_seed(1337)
```

+   **目的****：设置随机数种**子是为了确保模型运行时的确定性和复现性。

+   random.seed、np.random.seed 和 torch.manual_seed 用于固定 Python、NumPy 和 PyTorch 的随机数生成器。

+   **设备选择**：优先选择 GPU 进行推理。如果 GPU 不可用，则使用 CPU。

+   cudnn.deterministic = True 确保在使用 CUDA 加速时，结果可以复现，但会略微降低性能。

```
# 2. 初始化模型和分词器
lm_config = LMConfig()
```

```
lm_config
```

```
LMConfig {
  &quot;aux_loss_alpha&quot;: 0.01,
  &quot;dim&quot;: 512,
  &quot;dropout&quot;: 0.0,
  &quot;flash_attn&quot;: true,
  &quot;hidden_dim&quot;: null,
  &quot;max_seq_len&quot;: 512,
  &quot;model_type&quot;: &quot;MateConv&quot;,
  &quot;multiple_of&quot;: 64,
  &quot;n_heads&quot;: 16,
  &quot;n_kv_heads&quot;: 8,
  &quot;n_layers&quot;: 8,
  &quot;n_routed_experts&quot;: 4,
  &quot;n_shared_experts&quot;: true,
  &quot;norm_eps&quot;: 1e-05,
  &quot;norm_topk_prob&quot;: true,
  &quot;num_experts_per_tok&quot;: 2,
  &quot;scoring_func&quot;: &quot;softmax&quot;,
  &quot;seq_aux&quot;: true,
  &quot;transformers_version&quot;: &quot;4.44.0&quot;,
  &quot;use_moe&quot;: false,
  &quot;vocab_size&quot;: 6400
}
```

这里确保"dim": 768或者512，才可以使用对应参数的模型。

```
max_seq_len = 1024  # 可以根据需要调整
lm_config.max_seq_len = max_seq_len

model = Transformer(lm_config).to(device)
model_path = &#x27;./out/full_sft_512.pth&#x27;  # 替换为你的模型路径
state_dict = torch.load(model_path, map_location=device)
model.load_state_dict(state_dict)
model.eval()
```

```
Transformer(
  (tok_embeddings): Embedding(6400, 512)
  (dropout): Dropout(p=0.0, inplace=False)
  (layers): ModuleList(
    (0-7): 8 x TransformerBlock(
      (attention): Attention(
        (wq): Linear(in_features=512, out_features=512, bias=False)
        (wk): Linear(in_features=512, out_features=256, bias=False)
        (wv): Linear(in_features=512, out_features=256, bias=False)
        (wo): Linear(in_features=512, out_features=512, bias=False)
        (attn_dropout): Dropout(p=0.0, inplace=False)
        (resid_dropout): Dropout(p=0.0, inplace=False)
      )
      (attention_norm): RMSNorm()
      (ffn_norm): RMSNorm()
      (feed_forward): FeedForward(
        (w1): Linear(in_features=512, out_features=1408, bias=False)
        (w2): Linear(in_features=1408, out_features=512, bias=False)
        (w3): Linear(in_features=512, out_features=1408, bias=False)
        (dropout): Dropout(p=0.0, inplace=False)
      )
    )
  )
  (norm): RMSNorm()
  (output): Linear(in_features=512, out_features=6400, bias=False)
)
```

```
tokenizer = AutoTokenizer.from_pretrained(&#x27;./model/mateconv_tokenizer&#x27;) 
```

+   **模型加载**：

+   LMConfig：初始化模型配置，例如设置 max_seq_len 表示模型的最大输入序列长度。

+   **加载模型权重**：从指定的 model_path 路径加载已经微调好的模型权重 (full_sft_512.pth)。

+   **模型评估模式**：model.eval() 切换模型为评估模式，禁用 dropout 和 batch normalization 的训练行为。

+   **分词器加载**：

+   分词器用于将输入的自然语言文本转换为模型所需的 token 序列，并可以将模型输出的 token 序列转回人类可读的文本。这里从 mateconv_tokenizer 路径加载分词器。

```
# 3. 对话函数：生成完整回复
def generate_reply(prompt, temperature=0.5, top_k=16, stream=True):
    messages = [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt}]
    
    # 使用自定义的 prompt 模板 (根据你的应用逻辑)
    new_prompt = tokenizer.apply_chat_template(
        messages, 
        tokenize=False, 
        add_generation_prompt=True
    )[-(max_seq_len - 1):]

    input_ids = tokenizer(new_prompt).data[&#x27;input_ids&#x27;]
    input_ids = torch.tensor(input_ids, dtype=torch.long, device=device).unsqueeze(0)

    generated_text = &quot;&quot;
    with torch.no_grad():
        # 生成器返回的生成结果
        res_y = model.generate(input_ids, 
                               tokenizer.eos_token_id, 
                               max_new_tokens=max_seq_len, 
                               temperature=temperature, 
                               top_k=top_k, 
                               stream=stream)

        # 从生成器逐步获取生成结果
        try:
            y = next(res_y)
        except StopIteration:
            print(&quot;No answer&quot;)
            return &quot;&quot;

        history_idx = 0
        while y is not None:
            answer = tokenizer.decode(y[0].tolist())
            if answer and answer[-1] == &#x27;�&#x27;:
                try:
                    y = next(res_y)
                except StopIteration:
                    break
                continue

            if len(answer):
                generated_text += answer[history_idx:]
            
            try:
                y = next(res_y)
            except StopIteration:
                break
            history_idx = len(answer)

    return generated_text
```

+   **功能**：这个函数用于生成对话回复，流程如下：

1.  **自定义 ****prompt**：将用户输入（prompt）转换为自定义格式，这里使用了 apply_chat_template 生成对话的模板，将 messages 处理为可供模型生成的输入。

1.  **生成 ****input_ids**：将 new_prompt 转换为 token 序列 (input_ids) 供模型使用，并且调整其形状以适应批处理（unsqueeze(0) 增加批次维度）。

1.  **模型生成**：通过 model.generate 函数，使用贪心搜索或随机采样（受 temperature 和 top_k 控制）生成回复。

+   temperature 控制生成时的随机性，值越小生成结果越确定，越大则越随机。

+   top_k 限制采样的选择范围，取概率最高的 k 个 token。

1.  **逐步解码**：模型生成结果会被逐步解码为可读文本，直到完成生成或遇到 eos_token。

1.  **返回结果**：最终返回生成的完整回复文本。

##### 参数说明：

+   **prompt**：用户输入的对话文本，模型将根据该输入生成回复。

+   **temperature**：采样时的随机性控制参数，较高的值会增加生成内容的多样性，较低的值会生成更确定的输出。

+   **top_k**：采样时只从概率最高的前 k 个 token 中选择，限制生成的词汇选择范围，避免生成低概率的单词。

+   **stream**：决定是否以流式方式逐步获取生成结果。

这里需要注意的是，**大模型判断回复停止**的主要依据是 eos_token（End of Sequence token），即序列结束标记。eos_token 是用于告诉模型生成任务已经完成的特殊标记。模型在生成文本时，一旦生成了 eos_token，它就知道应该停止输出。

---

### 补充介绍序列结束标记预测原理

大模型如何判断下一个单词（token）是 eos**_token（**序列结束标记），实际上是通过模**型的生成****机制，**结**合模型在**预训练和指令微调阶段学到的语言模式来决定的。模型通过概率分布来选择下一个 token，而这个概率分布是在预训练和指令微调中学到的。

#### 1. **生成机制的工作原理**：

模型生成文本的过程通常基于自回归机制，也就是模型在生成每个 token 时，都会基于之前生成的所有 token 来预测下一个 token。它并不会明确知道下一个 token 是什么，而是通过计算每个可能 token 的概率分布，从中选择最可能的 token（包括 eos_token）。

##### 步骤概述：

1.  **输入上下文**：给定一个输入（如用户问题或对话），模型会根据上下文生成一个概率分布，这个分布表示每个可能的 token 出现在该位置的概率。

1.  **选择下一个 token**：模型通过贪心搜索、随机采样（基于 temperature）或其他策略，选择下一个 token。eos_token 作为候选之一，它的概率也由模型预测出来。

1.  **生成停止**：当模型预测出 eos_token 并选择它作为下一个 token 时，生成过程停止。

#### 2. **影响 ****eos_token**** 判断的因素**：

##### (1) **预训练的影响**：

预训练的过程中，模型会接触到大量的自然语言文**本。通过自回归语****言模型的方式，模型学**会了如何生成自然语言，包括如何适时结束一段句子或文本。因此，预训练赋予了模型关于自然语言的结构、语法、句法模式的基础知识。

+   **预训练阶段的 ****eos_token**：在预训练的文本数据中，通常会在每个训练样本的末尾加上 eos_token，因此模型在预训练中学会了何时结束一段文本。在生成过程中，模型会根据上下文推测是否应该结束文本。

##### (2) **指令微调的影响**：

指令微调专门针对任务的要求进行了微调，模型不仅学习了如何生成回答，还学习了如何在不同的任务和上下文中生成适合的结束标记。因此，指令微调对 eos_token 的判断进一步进行了优化，使得模型在实际任务中能够更好地判断何时结束输出。

+   **指令微调阶段的 ****eos_token**：指令微调阶段的训练数据中，通常会标注任务的起始和结束，并提供明确的输入和输出。这使得模型能更好地理解在特定任务（如对话、问答等）中，何时应该生成 eos_token 来结束对话或回答。

#### 3. **模型如何具体判断 ****eos_token**：

在每个生成步骤中，模型会基于已经生成的 token 来预测下一个 token。预测的过程通常如下：

1.  **概率分布计算**：模型通过计算输入 token 序列的上下文信息，输出一个表示所有可能 token 的概率分布（Softmax）。这个概率分布表示每个 token 出现的可能性，包括 eos_token。

1.  例如，假设模型在某个位置预测下一 token 时，给出了以下概率分布：

+   the: 0.4

+   a: 0.3

+   eos_token: 0.2

+   其他 token：0.1

1.  **选择 token**：根据生成策略（如贪心搜索、随机采样等），模型会选择概率最大的 token，或者根据设定的 temperature 或 top_k 进行采样。如果 eos_token 的概率最高或者通过采样选中，模型会生成 eos_token，表示生成过程结束。

+   **贪心搜索**：选择当前概率最大的 token。

+   **随机采样**：根据概率进行采样，temperature 控制采样的随机性。

+   **top-k 采样**：只从概率最高的前 k 个 token 中选择。

---

+   测试问答

```
response = generate_reply(&quot;长江、&quot;)
response
```

```
&#x27;长江是中国最长的河流，总长度约为6,300千米。它发源于青藏高原，流经中国的11个省份，最后注入东海。长江流经9个省份，是中国最重要的水文发电资源之一。长江流域是中国最重要的农业、工业和交通中心之一，具有重要的经济和文化价值。&#x27;
```

```
response = generate_reply(&quot;你好，好久不见！&quot;)
response
```

```
&#x27;你好！我很高兴能为你服务。你需要帮忙做些什么吗？&#x27;
```

```
response = generate_reply(&quot;请问什么是机器学习？&quot;)
response
```

```
&#x27;机器学习是一种人工智能的分支，它通过对大量数据进行训练，使计算机能够自动发现数据中的规律和模式，然后通过这些规律和模式来做出预测或决策。机器学习可以应用于各种领域，如自然语言处理、图像识别、金融预测等。&#x27;
```

```
response = generate_reply(&quot;请问机器学习和深度学习的区别是什么？&quot;)
response
```

```
&#x27;机器学习和深度学习是人工智能的两个不同领域。机器学习是一种通过数据和算法来让计算机系统自动改进的方法，而深度学习是机器学习的一种特殊形式，它使用深层神经网络来模拟人类大脑的神经元网络。因此，深度学习是机器学习的一种特殊形式，它使用多层神经网络来模拟人类大脑的工作方式。&#x27;
```

#### 2.使用OpenAI风格API调用MateConv

+   创建OpenAI风格API中继服务脚本

使用 **OpenAI 风格** 调用私有化训练的大模型具有显著的优势和必要性，特别是在模型部署、集成和易用性方面：

##### 1. **简化接口设计，提升易用性**

OpenAI 的 API 风格以其简单、直观的设计著称，用户只需发送输入文本，API 便会返回生成的响应。使用这种风格调用私有化大模型能够极大降低用户的开发和集成成本，使非专业开发人员也能轻松调用模型。

+   **无缝集成**：通过统一的接口风格，开发者可以快速将模型集成到现有系统中，无需了解复杂的底层架构或实现细节。

+   **易于操作**：OpenAI 风格的 API 调用简洁明了，通常以 JSON 格式交互，标准化的接口便于维护和升级。

##### 2. **灵活的多任务处理**

使用 OpenAI 风格调用私有化模型能够灵活地适应多种任务场景，如对话生成、文本分类、问答系统等。API 接口允许开发者通过简单的指令输入，快速切换模型任务，从而提高开发效率和应用场景的灵活性。

+   **多功能支持**：开发者只需通过不同的 API 调用参数或指令，即可实现不同的功能，而无需重新设计接口。

+   **任务适应性强**：对于不同的自然语言处理任务，使用 OpenAI 风格的 API 允许模型在对话、内容生成、语言理解等多任务中快速切换和扩展。

##### 3. 统一调用体验，提升用户和开发者体验

OpenAI 风格的 API 调用方式已经在业界广泛应用和认可。使用类似的风格调用私有化大模型，能够为开发者提供一致的使用体验，减少学习曲线和使用门槛。同时，企业能够保持与外部 API 调用的接口一致性，方便未来扩展和维护。

+   **一致性和可移植性**：开发者可以利用同样的调用方式无缝切换公共 API 和私有化模型，保持一致的代码风格。

+   **用户体验提升**：通过简洁的接口，最终用户能够更快速地与大模型进行交互，提升整体使用感受。

+   编写OpenAI风格API调用脚本

```
# encoding: utf-8
import json
import re
import time
import uuid
import warnings

import tiktoken
import torch
import numpy as np
from typing import List
from flask import Flask, current_app, request, Blueprint, stream_with_context
from flask_cors import CORS
from sentence_transformers import SentenceTransformer
from sklearn.preprocessing import PolynomialFeatures
from transformers import AutoTokenizer, AutoModelForCausalLM
from marshmallow import validate, Schema, fields, EXCLUDE
from pydantic import BaseModel
from model.model import Transformer  
from model.LMConfig import LMConfig

warnings.filterwarnings(&#x27;ignore&#x27;, category=UserWarning)

# ------------------------------------------------------------------------------------------------------------------
DEVICE_NAME = &quot;cuda:0&quot; if torch.cuda.is_available() else &quot;cpu&quot;
DEVICE = torch.device(DEVICE_NAME)
MODEL_PATH = &quot;./MateConv/full_sft_512.pth&quot;
TOKENIZE_PATH = &quot;./model/mateconv_tokenizer&quot;
max_new_tokens = 1024
temperature = 0.7
top_k = 16


# ------------------------------------------------------------------------------------------------------------------

class Transformers():
    def __init__(self, app=None, tokenizer=None, model=None):
        # self.chat = None
        if app is not None:
            self.init_app(app, tokenizer, model)

    def init_app(self, app, tokenizer=None, model=None, chat=None):
        self.tokenizer = tokenizer
        self.model = model
        # if chat is None:
        #     # self.chat = model.chat
        #     self.chat = self.chat

    # gpt2&#x27;s
    def build_chat_input(self, tokenizer, messages: List[dict]):
        new_prompt = tokenizer.apply_chat_template(
            messages,
            tokenize=False,
            add_generation_prompt=True
        )[-(max_new_tokens - 1):]
        inputs_ids = tokenizer(new_prompt).data[&#x27;input_ids&#x27;]
        inputs_ids = (torch.tensor(inputs_ids, dtype=torch.long, device=DEVICE)[None, ...])
        return inputs_ids, tokenizer.eos_token_id, new_prompt

    def chat_stream(self, tokenizer, messages: List[dict], stream=True):
        input_ids, eos_token_id, new_prompt = self.build_chat_input(tokenizer, messages)
        if stream:
            res_y = self.model.generate(input_ids, tokenizer.eos_token_id, max_new_tokens=max_new_tokens,
                                        temperature=temperature, top_k=top_k, stream=True)

            try:
                y = next(res_y)
            except:
                print(&quot;No answer&quot;)
                return &#x27;No answer&#x27;

            history_idx = 0
            while y != None:
                answer = tokenizer.decode(y[0].tolist())
                if answer and answer[-1] == &#x27;�&#x27;:
                    try:
                        y = next(res_y)
                    except:
                        break
                    continue
                # print(answer)
                if not len(answer):
                    try:
                        y = next(res_y)
                    except:
                        break
                    continue

                yield answer[history_idx:]
                try:
                    y = next(res_y)
                except:
                    break
                history_idx = len(answer)
                if not stream:
                    break

    def chat_no_stream(self, tokenizer, messages: List[dict]):
        input_ids, eos_token_id, new_prompt = self.build_chat_input(tokenizer, messages)
        res_y = self.model.generate(input_ids, tokenizer.eos_token_id, max_new_tokens=max_new_tokens,
                                    temperature=temperature, top_k=top_k, stream=False)
        y = next(res_y)
        answer = tokenizer.decode(y[0].tolist())
        return answer


tfs = Transformers()
base_tfs = Transformers()

models_bp = Blueprint(&#x27;Models&#x27;, __name__, url_prefix=&#x27;/v1/models&#x27;)
chat_bp = Blueprint(&#x27;Chat&#x27;, __name__, url_prefix=&#x27;/v1/chat&#x27;)
completions_bp = Blueprint(&#x27;Completions&#x27;, __name__, url_prefix=&#x27;/v1/completions&#x27;)
embedding_bp = Blueprint(&#x27;Embeddings&#x27;, __name__, url_prefix=&#x27;/v1&#x27;)


def sse(line, field=&quot;data&quot;):
    return &quot;{}: {}\n\n&quot;.format(
        field, json.dumps(line, ensure_ascii=False) if isinstance(line, dict) else line)


def empty_cache():
    if torch.backends.mps.is_available():
        torch.mps.empty_cache()


def create_app():
    app = Flask(__name__)
    CORS(app)
    app.register_blueprint(models_bp)
    app.register_blueprint(chat_bp)
    app.register_blueprint(completions_bp)
    app.register_blueprint(embedding_bp)

    @app.after_request
    def after_request(resp):
        empty_cache()
        return resp

    # 手动加载自定义的模型
    tokenizer = AutoTokenizer.from_pretrained(TOKENIZE_PATH, trust_remote_code=True, use_fast=False)

    # 初始化自定义的 LMConfig
    lm_config = LMConfig()
    model = Transformer(lm_config).to(DEVICE)  # 使用自定义的 Transformer 模型类

    # 加载模型权重
    state_dict = torch.load(MODEL_PATH, map_location=DEVICE)
    model.load_state_dict(state_dict)

    tfs.init_app(app, tokenizer, model)
    base_tfs.init_app(app, tokenizer, model)

    return app

class ModelSchema(Schema):
    id = fields.Str()
    object = fields.Str(dump_default=&quot;model&quot;, metadata={&quot;example&quot;: &quot;model&quot;})
    created = fields.Int(dump_default=lambda: int(time.time()), metadata={&quot;example&quot;: 1695402567})
    owned_by = fields.Str(dump_default=&quot;owner&quot;, metadata={&quot;example&quot;: &quot;owner&quot;})


class ModelListSchema(Schema):
    object = fields.Str(dump_default=&quot;list&quot;, metadata={&quot;example&quot;: &quot;list&quot;})
    data = fields.List(fields.Nested(ModelSchema), dump_default=[])


class ChatMessageSchema(Schema):
    role = fields.Str(required=True, metadata={&quot;example&quot;: &quot;system&quot;})
    content = fields.Str(required=True, metadata={&quot;example&quot;: &quot;You are a helpful assistant.&quot;})


class CreateChatCompletionSchema(Schema):
    class Meta:
        unknown = EXCLUDE  # 忽略未知的字段

    model = fields.Str(required=True, metadata={&quot;example&quot;: &quot;MateConv&quot;})
    messages = fields.List(
        fields.Nested(ChatMessageSchema), required=True,
        metadata={&quot;example&quot;: [
            ChatMessageSchema().dump({&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a helpful assistant.&quot;}),
            ChatMessageSchema().dump({&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Hello!&quot;})
        ]}
    )
    temperature = fields.Float(load_default=1.0, metadata={&quot;example&quot;: 1.0})
    top_p = fields.Float(load_default=1.0, metadata={&quot;example&quot;: 1.0})
    n = fields.Int(load_default=1, metadata={&quot;example&quot;: 1})
    max_tokens = fields.Int(load_default=None, metadata={&quot;example&quot;: None})
    stream = fields.Bool(load_default=False, example=False)
    presence_penalty = fields.Float(load_default=0.0, example=0.0)
    frequency_penalty = fields.Float(load_default=0.0, example=0.0)


class ChatCompletionChoiceSchema(Schema):
    index = fields.Int(metadata={&quot;example&quot;: 0})
    message = fields.Nested(ChatMessageSchema, metadata={
        &quot;example&quot;: ChatMessageSchema().dump(
            {&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: &quot;\n\nHello there, how may I assist you today?&quot;}
        )})
    finish_reason = fields.Str(
        validate=validate.OneOf([&quot;stop&quot;, &quot;length&quot;, &quot;content_filter&quot;, &quot;function_call&quot;]),
        metadata={&quot;example&quot;: &quot;stop&quot;})


class ChatCompletionSchema(Schema):
    id = fields.Str(
        dump_default=lambda: uuid.uuid4().hex,
        metadata={&quot;example&quot;: &quot;cmpl-uqkvlQyYK7bGYrRHQ0eXlWi7&quot;})
    object = fields.Constant(&quot;chat.completion&quot;)
    created = fields.Int(dump_default=lambda: int(time.time()), metadata={&quot;example&quot;: 1695402567})
    model = fields.Str(metadata={&quot;example&quot;: &quot;MateConv&quot;})
    choices = fields.List(fields.Nested(ChatCompletionChoiceSchema))


class ChatDeltaSchema(Schema):
    role = fields.Str(metadata={&quot;example&quot;: &quot;assistant&quot;})
    content = fields.Str(required=True, metadata={&quot;example&quot;: &quot;Hello&quot;})


class ChatCompletionChunkChoiceSchema(Schema):
    index = fields.Int(metadata={&quot;example&quot;: 0})
    delta = fields.Nested(ChatDeltaSchema, metadata={&quot;example&quot;: ChatDeltaSchema().dump(
        {&quot;role&quot;: &quot;assistant&quot;, &quot;example&quot;: &quot;Hello&quot;})})
    finish_reason = fields.Str(
        validate=validate.OneOf([&quot;stop&quot;, &quot;length&quot;, &quot;content_filter&quot;, &quot;function_call&quot;]),
        metadata={&quot;example&quot;: &quot;stop&quot;})


class ChatCompletionChunkShema(Schema):
    id = fields.Str(
        dump_default=lambda: uuid.uuid4().hex,
        metadata={&quot;example&quot;: &quot;cmpl-uqkvlQyYK7bGYrRHQ0eXlWi7&quot;})
    object = fields.Constant(&quot;chat.completion.chunk&quot;)
    created = fields.Int(dump_default=lambda: int(time.time()), metadata={&quot;example&quot;: 1695402567})
    model = fields.Str(metadata={&quot;example&quot;: &quot;MateConv&quot;})
    choices = fields.List(fields.Nested(ChatCompletionChunkChoiceSchema))


class CreateCompletionSchema(Schema):
    model = fields.Str(required=True, metadata={&quot;example&quot;: &quot;MateConv&quot;})
    prompt = fields.Raw(metadata={&quot;example&quot;: &quot;Say this is a test&quot;})
    max_tokens = fields.Int(load_default=16, metadata={&quot;example&quot;: 256})
    temperature = fields.Float(load_default=1.0, metadata={&quot;example&quot;: 1.0})
    top_p = fields.Float(load_default=1.0, metadata={&quot;example&quot;: 1.0})
    n = fields.Int(load_default=1, metadata={&quot;example&quot;: 1})
    stream = fields.Bool(load_default=False, example=False)
    logit_bias = fields.Dict(load_default=None, example={})
    presence_penalty = fields.Float(load_default=0.0, example=0.0)
    frequency_penalty = fields.Float(load_default=0.0, example=0.0)


class CompletionChoiceSchema(Schema):
    index = fields.Int(load_default=0, metadata={&quot;example&quot;: 0})
    text = fields.Str(required=True, metadata={&quot;example&quot;: &quot;登鹳雀楼-&gt;王之涣\n夜雨寄北-&gt;&quot;})
    logprobs = fields.Dict(load_default=None, metadata={&quot;example&quot;: {}})
    finish_reason = fields.Str(
        validate=validate.OneOf([&quot;stop&quot;, &quot;length&quot;, &quot;content_filter&quot;, &quot;function_call&quot;]),
        metadata={&quot;example&quot;: &quot;stop&quot;})


class CompletionUsageSchema(Schema):
    prompt_tokens = fields.Int(metadata={&quot;example&quot;: 5})
    completion_tokens = fields.Int(metadata={&quot;example&quot;: 7})
    total_tokens = fields.Int(metadata={&quot;example&quot;: 12})


class CompletionSchema(Schema):
    id = fields.Str(
        dump_default=lambda: uuid.uuid4().hex,
        metadata={&quot;example&quot;: &quot;cmpl-uqkvlQyYK7bGYrRHQ0eXlWi7&quot;})
    object = fields.Constant(&quot;text_completion&quot;)
    created = fields.Int(dump_default=lambda: int(time.time()), metadata={&quot;example&quot;: 1695402567})
    model = fields.Str(metadata={&quot;example&quot;: &quot;MateConv&quot;})
    choices = fields.List(fields.Nested(CompletionChoiceSchema))
    usage = fields.Nested(CompletionUsageSchema)


@stream_with_context
def stream_chat_generate(messages):
    delta = ChatDeltaSchema().dump(
        {&quot;role&quot;: &quot;assistant&quot;})
    choice = ChatCompletionChunkChoiceSchema().dump(
        {&quot;index&quot;: 0, &quot;delta&quot;: delta, &quot;finish_reason&quot;: None})

    yield sse(
        ChatCompletionChunkShema().dump({
            &quot;model&quot;: &quot;MateConv&quot;,
            &quot;choices&quot;: [choice]})
    )

    # 调用 chat 方法并遍历其返回的生成器
    for response in tfs.chat_stream(tfs.tokenizer, messages):
        delta = ChatDeltaSchema().dump(
            {&quot;content&quot;: response})
        choice = ChatCompletionChunkChoiceSchema().dump(
            {&quot;index&quot;: 0, &quot;delta&quot;: delta, &quot;finish_reason&quot;: None})

        yield sse(
            ChatCompletionChunkShema().dump({
                &quot;model&quot;: &quot;MateConv&quot;,
                &quot;choices&quot;: [choice]})
        )

    yield sse(&#x27;[DONE]&#x27;)


@chat_bp.route(&quot;/completions&quot;, methods=[&#x27;POST&#x27;])
def create_chat_completion():
    # 接收并验证传入的JSON请求
    create_chat_completion = CreateChatCompletionSchema().load(request.json)

    # 如果请求使用了 stream，则返回流式响应
    if create_chat_completion[&quot;stream&quot;]:
        return current_app.response_class(
            stream_chat_generate(create_chat_completion[&quot;messages&quot;]),
            mimetype=&quot;text/event-stream&quot;
        )
    else:
        # 调用模型进行无流式生成
        response = tfs.chat_no_stream(tfs.tokenizer, create_chat_completion[&quot;messages&quot;])

        # 格式化为 OpenAI 样式的回复
        message = ChatMessageSchema().dump(
            {&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: response})
        choice = ChatCompletionChoiceSchema().dump(
            {&quot;index&quot;: 0, &quot;message&quot;: message, &quot;finish_reason&quot;: &quot;stop&quot;})

        # 返回 JSON 响应
        return ChatCompletionSchema().dump({
            &quot;model&quot;: create_chat_completion[&quot;model&quot;],
            &quot;choices&quot;: [choice]
        })

class EmbeddingRequest(BaseModel):
    input: List[str]
    model: str


@embedding_bp.route(&quot;/embeddings&quot;, methods=[&#x27;POST&#x27;])
def get_embeddings():
    request_data = request.get_json()  # 获取 POST 请求体中的 JSON 数据
    request_params = EmbeddingRequest(**request_data)  # 将 JSON 数据转换为 EmbeddingRequest 对象

    def expand_features(embedding, target_length):
        poly = PolynomialFeatures(degree=2)
        expanded_embedding = poly.fit_transform(embedding.reshape(1, -1))
        expanded_embedding = expanded_embedding.flatten()
        if len(expanded_embedding) &gt; target_length:
            # 如果扩展后的特征超过目标长度，可以通过截断或其他方法来减少维度
            expanded_embedding = expanded_embedding[:target_length]
        elif len(expanded_embedding) &lt; target_length:
            # 如果扩展后的特征少于目标长度，可以通过填充或其他方法来增加维度
            expanded_embedding = np.pad(
                expanded_embedding, (0, target_length - len(expanded_embedding))
            )
        return expanded_embedding

    def num_tokens_from_string(string: str) -&gt; int:
        &quot;&quot;&quot;Returns the number of tokens in a text string.&quot;&quot;&quot;
        encoding = tiktoken.get_encoding(&#x27;cl100k_base&#x27;)
        num_tokens = len(encoding.encode(string))
        return num_tokens

    def has_chinese_char(s):
        pattern = re.compile(r&#x27;[\u4e00-\u9fa5]&#x27;)
        # if bool(pattern.search(s)):
        #     print(&#x27;m3e编码&#x27;)
        # else:
        #     print(&#x27;bge编码&#x27;)

        return bool(pattern.search(s))

    # 计算嵌入向量和tokens数量
    embeddings = [embeddings_model_m3e.encode(text)
                  if has_chinese_char(text)
                  else embeddings_model_bge.encode(text)
                  for text in request_params.input]

    # 如果嵌入向量的维度不为1536，则使用插值法扩展至1536维度
    embeddings = [
        expand_features(embedding, 768) if len(embedding) &lt; 768 else embedding
        for embedding in embeddings
    ]

    # Min-Max normalization 归一化
    embeddings = [embedding / np.linalg.norm(embedding) for embedding in embeddings]

    # 将numpy数组转换为列表
    embeddings = [embedding.tolist() for embedding in embeddings]
    prompt_tokens = sum(len(text.split()) for text in request_params.input)
    total_tokens = sum(num_tokens_from_string(text) for text in request_params.input)

    response = {
        &quot;data&quot;: [
            {&quot;embedding&quot;: embedding, &quot;index&quot;: index, &quot;object&quot;: &quot;embedding&quot;}
            for index, embedding in enumerate(embeddings)
        ],
        &quot;model&quot;: request_params.model,
        &quot;object&quot;: &quot;list&quot;,
        &quot;usage&quot;: {
            &quot;prompt_tokens&quot;: prompt_tokens,
            &quot;total_tokens&quot;: total_tokens,
        },
    }
    # print(response)
    return response


app = create_app()

if __name__ == &#x27;__main__&#x27;:
    use_emb = False
    try:
        import ngrok
        import logging

        logging.basicConfig(level=logging.INFO)
        listener = ngrok.werkzeug_develop()
    except Exception:
        pass

    embeddings_model_m3e = SentenceTransformer(&#x27;.\\m3e-base&#x27;, device=&#x27;cpu&#x27;) if use_emb else None
    embeddings_model_bge = SentenceTransformer(&#x27;.\\bge-base-en-v1.5&#x27;, device=&#x27;cpu&#x27;) if use_emb else None

    app.run(debug=False, host=&quot;0.0.0.0&quot;, port=8000)
```

然后将其命名为openai_api.py并放在主目录下：

![](img/4131a8d6a8588e2db4b4f3f66e6b777c.png)

+   运行OpenAI API服务

然后在命令行中运行如下代码，以开启后端OpenAI API服务：

```
python openai_api.py
```

![](img/157d640fde028e2c4fc2881d2376fb4c.png)

+   OpenAI风格API调用MateConv

```
from openai import OpenAI
```

```
client = OpenAI(api_key=&quot;Key&quot;, 
                base_url=&quot;http://localhost:8000/v1&quot;)
```

```
completion = client.chat.completions.create(
  model=&quot;MateConv&quot;,
  messages=[
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，好久不见！&quot;}
  ]
)
```

```
completion
```

```
ChatCompletion(id=&#x27;dc626ed170a841cf8269eda9e4a0a627&#x27;, choices=[Choice(finish_reason=&#x27;stop&#x27;, index=0, logprobs=None, message=ChatCompletionMessage(content=&#x27;您好！我很高兴能够与您交谈。您想聊些什么呢？&#x27;, refusal=None, role=&#x27;assistant&#x27;, function_call=None, tool_calls=None))], created=1729771076, model=&#x27;MateConv&#x27;, object=&#x27;chat.completion&#x27;, service_tier=None, system_fingerprint=None, usage=None)
```

```
completion.choices[0].message.content
```

```
&#x27;您好！我也很高兴能与您交流。有什么需要我做的吗？&#x27;
```

```
completion = client.chat.completions.create(
  model=&quot;MateConv&quot;,
  messages=[
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问，什么是人工智能？&quot;}
  ]
)
completion.choices[0].message.content
```

```
&#x27;人工智能是一种模拟人类智能的技术，它通过计算机程序和算法来实现自主学习、推理、识别语音和图像等能力。它可以对数据进行处理和分析，并根据已有的数据和经验进行推理和决策。在现代社会中，人工智能已经广泛应用于各个领域，如医疗、金融、交通、教育等。&#x27;
```

```
completion = client.chat.completions.create(
  model=&quot;MateConv&quot;,
  messages=[
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问，什么是深度学习？&quot;}
  ]
)
completion.choices[0].message.content
```

```
&#x27;深度学习是一种机器学习的方法，它使用神经网络来模拟人脑神经网络，从而实现对复杂数据的学习和模式识别。深度学习的核心思想是建立起层次的层次结构，这些层次结构可以分为多个层次，每个层次都包含多个神经元和权重。深度学习的核心思想是，通过多层次的非线性变换，让神经网络能够自动地从输入数据中学习，并在新数据上取得更好的分类和预测能力。&#x27;
```

+   创建基于前端的聊天机器人

将web_chat放在项目主目录下：

![](img/e5b178f68590b06b746431b54d6fc578.png)

然后在命令行中运行：

```
streamlit run web_chat.py --server.port 8501
```

![](img/779a801d94a7a085b5202a4d116b3a3c.png)

> 若未安装streamlit，则需要使用如下指令进行安装

```
pip install streamlit
```

然后将8501映射到本地：

![](img/cab464e2e53d7460cd982040939ee677.png)

然后打开浏览器，即可在8501端口进行对话：

![](img/57212cab42ffb26df1cfb1b3962cb76b.png)

📍更**多大模型技术内容学习**

**扫码添加助理****英英**，回复“大模型”，了解更多大模型技术详情哦👇

![](img/338ad76d08427e5c9b3b0cbff0239c5d.png)

此外，**扫码回复“入群”**，即可加入**大模型技术社群****：海量硬核独家技术****干货内容****+无门槛****技术交流****！**