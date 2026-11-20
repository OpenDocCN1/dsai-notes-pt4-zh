
# 思维树（Tree of Thoughts）

## 创意写作任务实验

### 基线
鉴于任务的创造性质,`IO`和`CoT`提示都是零样本的。前者提示语言模型直接根据输入约束生成连贯段落,而后者提示语言模型先做简要计划然后写段落,即计划作为中间思维步骤。我们对每个任务生成10个`IO`和`CoT`样本。我们还考虑每个任务的随机`IO`样本上进行迭代改进(`k ≤ 5`)的方法,其中语言模型基于输入约束和最后生成的段落判断段落是否已经“完全连贯”,如果不是的话生成一个改进的段落。

### ToT设置
我们构建深度为2的`ToT`(只有1个中间思维步骤)— 语言模型首先生成`k = 5`个计划并为最佳计划投票(图4),然后基于最佳计划类似地生成`k = 5`个段落并为最佳段落投票。这里宽度限制`b = 1`,因为每一步只保留一个选择。一个简单的零样本投票提示(“分析下面的选择,然后总结哪个对指令最有前景”)用于两步骤都采样5次投票。

### 结果
图5(a)展示了100个任务中`GPT-4`的平均得分，其中`ToT`（7.56）被认为比`IO`（6.19）和`CoT`（6.93）平均生成更连贯的段落。虽然这样的自动度量可能会有噪音，但图5(b)通过显示人们在100个段落对中更喜欢`ToT`而不是`CoT`来确认这一发现，有41个段落对如此，而只有21个段落对更喜欢`CoT`（其他38对段落则被发现“同样连贯”）。最后，迭代改进在这个自然语言任务上更加有效，它将`IO`的连贯性得分从6.19提高到7.67，将`ToT`的连贯性得分从7.56提高到7.91。我们认为这可以被视为`ToT`框架中思维生成的第三种方法，新的思维可以从改进旧的思维中产生，而不是从独立同分布或顺序生成。

![](img/1970d1a84b07197f7d41bae05ac6f662.png)

![](img/0356c6f85fbf270dec522a86bcd7a943.png)

### 迷你纵横填字游戏

在“24点游戏”和“创意写作”中，`ToT`相对较浅 - 至多需要3个思维步骤才能达到最终输出。在这里，我们探讨了`5x5`迷你纵横填字游戏作为一个更具挑战性的涉及自然语言的搜索问题。再次强调，目标不仅是解决任务，因为更一般的纵横填字游戏可以通过专门的`NLP`管道[31]轻松解决，该管道利用大规模检索而不是`LM`。相反，我们的目标是探索`LM`作为一种通用问题解决器的极限，该解决器通过有意识的推理来探索自己的思维并指导自己的探索。

**任务设置。**我们从`GooBix`网站获取了包含156个`5x5`迷你纵横填字游戏的数据。由于我们观察到相邻游戏包含相似的线索，我们使用了20个游戏，索引为1、6、···、91、96，用于测试，以及游戏136、141、146、151、156用于提示。对于每个任务，输入描述了5个横向线索和5个纵向线索，输出应该是一个`5x5 = 25`个字母的棋盘，以解决纵横填字游戏。对于评估，我们考虑三个成功的级别：正确字母的比例（每个游戏25个），正确单词的比例（每个游戏10个）和成功的游戏数量。

**基线。**我们在`IO`提示中提供了5个示例的输入-输出对，在`CoT`提示中，我们还按顺序包括中间的单词`h1..5`，然后`v1..5`。我们对每个提示运行了10个样本，并对结果进行了平均。

**ToT设置。**我们利用深度优先搜索（算法2），持续探索最有前途的后续单词线索，直到状态不再有前途，然后回溯到父状态以探索替代思维。为了使搜索可行，后续思维受到限制，不能更改任何填写的单词或字母，因此`ToT`最多具有10个中间步骤。对于思维生成，在每个状态下，我们将所有现有的思维（例如`Figure 6(a)`中的状态`h2.motor; h1.tasks`）都翻译成剩余线索的字母约束（例如`v1. To heap: tm ; ...`），然后提出提案提示5次，以提供下一个单词的填充位置和内容的候选项。重要的是，我们还提示`LM`为不同的思维提供置信水平，并在提案中汇总这些置信水平，以获得一个排序的下一个要探索的思维列表（`Figure 6(a)`）。

![](img/49975db43d7579d51d6cdb4caf303b11.png)

对于状态的评估，我们类似地将每个状态翻译成剩余线索的字母约束，然后针对每个线索评估是否可能根据这些约束填充。如果任何剩余线索被认为“不可能”填充（例如`v1. To heap: tm s`），那么状态子树的探索将被修剪，`DFS`将回溯到其父状态以探索下一个有前途的思维。我们将`DFS`搜索步骤限制为100步，并将最深度的探索状态呈现为最终输出（如果有多个状态被探索，则选择第一个）。

**结果。**如表3所示，`IO`和`CoT`提示方法的表现不佳，单词级成功率不到16%，而`ToT`显著改善了所有指标，实现了60%的单词级成功率，并解决了20个游戏中的4个。这样的改进并不令人意外，因为`IO`和`CoT`缺乏尝试不同线索、更改决策或回溯的机制。

| 方法 | 字母 | 单词 | 游戏 |
| --- | --- | --- | --- |
| `IO` | 38.7 | 14 | 0 |
| `+最佳状态 - 修剪 - 回溯` | 82.4 | 67.5 | 35 |

¹**Oracle和消融研究。**当从每个任务的最佳`DFS`状态（而不是启发式确定的最佳状态）输出时，`ToT`的性能甚至更高，实际解决了7/20个游戏（表3，“`+best state`”），表明我们的简单输出启发式可以轻松改进。有趣的是，有时当纵横填字游戏实际上被解决时，状态评估器仍可能将一些单词视为“不可能”并修剪 - 可能是因为`5x5`的纵横填字游戏设计上包含一些`GPT-4`无法识别²的罕见或过时的单词。鉴于状态评估作为修剪启发式是不完美的，我们还尝试消融修剪，并发现性能通常更差（表3，“`-prune`”）。然而，它确实可以找到4/20个游戏的正确解决方案（尽管只通过启发式输出1个），其中有3个游戏在100步内`ToT+修剪`不能解决。因此，在这种情况下，更好的`DFS`修剪启发式对问题解决至关重要。最后，我们通过运行一项消融研究来确认回溯的重要性，该研究在最多20个步骤内保持填写最有前途的线索，允许覆盖。这类似于带有宽度限制`b = 1`的“贪婪”`BFS`搜索，但表现不佳，单词级成功率仅为20%（表3，“`-backtrack`”）。

¹`Oracle` 在这里指的是“预言者”或“智者”，通常用于指代在某个领域或问题上具有卓越知识和能力的人或系统。在上下文中，它表示在每个任务中选择最佳`DFS`状态时，考虑到对问题解决的专业性，因此未翻译。

²例如，`agend` 是 `agendum` 的一种过时形式，但 `GPT-4` 将其视为 `agenda` 的打字错误。在知识不确定性下，外部检索或网络交互可以增强 `LM` 的问题解决能力。

## 相关工作

**规划和决策制定。**智能规划和决策制定对于实现预定义的目标至关重要。由于它们接受了大量的世界知识和人类示例的训练，因此已知 `LM` 具有丰富的常识，可以根据问题设置和环境状态提出合理的计划[10，39，34，11，32，38，37]。我们提出的“思维树”方法通过在每个问题解决步骤同时考虑多个潜在可行的计划，然后继续进行最有前途的计划，扩展了现有的规划公式。思维采样和价值反馈之间的整合有机地融合了规划和决策制定机制，实现了在解决方案树内进行有效搜索。另一方面，传统的决策制定程序通常需要训练专门的奖励和策略模型，就像强化学习中的 `CHAI` [30]一样，而我们使用 `LM` 本身提供的价值估计来进行决策制定。

**自我反思。**使用 `LLM` 来评估其自身预测的可行性正在成为问题解决中日益重要的过程。[25，17，21]引入了“自我反思”机制，其中 `LLM` 为其生成的候选项提供反馈。[4]通过根据其代码执行结果生成的 `LM` 自身生成的反馈消息，提高了 `LM` 的代码生成准确性。类似地，[14]还介绍了对行动和状态的“评论”或审查步骤，在解决计算机操作任务时决定下一步要采取的行动。与我们的工作密切相关的另一项最近工作是“自我评估引导解码”[36]。类似于我们的方法，自我评估解码也遵循一个树搜索过程，其中叶子从随机束搜索解码中采样，然后由 `LLM` 本身使用精心准备的自我评估提示进行评估。然而，他们的方法使用了 `PAL` 公式[7]，该公式将思维表示为代码，这使得难以处理像我们在本文中考虑的创意写作等具有挑战性的任务。因此，我们的“思维树”公式更加通用，可以处理 `GPT-4` 在标准提示下仅能获得非常低准确性的具有挑战性的任务。

**程序引导的 `LLM` 生成。**我们的提案还与最近的进展相关，它使用符号程序引导来组织 `LLM` 的行为。例如，[24]将 `LLM` 嵌入到算法搜索过程中，以帮助逐步解决问题，例如逐步回答问题，其中搜索树通过可能提供答案的相关段落进行扩展。然而，这种方法与我们的方法不同，因为树是通过采样外部段落而不是 `LLM` 自己的思维进行扩展的，并且没有反思或投票步骤。另一种方法，`LLM+P` [15]更进一步，将实际的规划过程委托给了一个传统的规划器。

**经典搜索方法。**最后但同样重要的是，我们的方法可以被视为用于问题解决的现代版本的经典搜索方法。例如，它可以被视为启发式搜索算法，类似于 `A*` [8]，其中每个搜索节点的启发式由 `LLM` 的自我评估提供。从这个角度来看，我们的方法还与[16]中提出的 `NeuroLogic A*` 式解码方法相关，该方法受到 `A*` 搜索的启发，但引入了前瞻启发式，对于 `LLM` 来说，这些启发式对于改进束搜索或前 `k` 采样解码是有效的。然而，这种方法受限于句子生成任务，而我们的框架则设计用于由价值反馈保护的复杂多步问题解决。

## 讨论

**局限性和未来方向。**像 `ToT` 这样的深思熟虑的搜索可能对 `GPT-4` 已经擅长的许多现有任务并不必要，而且作为一个初步步骤，这项工作只探讨了挑战 `GPT-4` 并将更好的搜索和规划能力与 `LM` 结合起来的三个相对简单的任务。然而，随着我们开始将 `LM` 用于更多实际的决策应用（例如编码、数据分析、机器人等），可能会出现更复杂的任务，并提供研究这些问题的新机会。此外，像 `ToT` 这样的搜索方法需要更多资源（例如 `GPT-4 API` 成本）以改善任务性能，但 `ToT` 的模块化灵活性允许用户自定义性能与成本的权衡，而正在进行的开源努力[29]应该会在不久的将来降低这些成本。最后，这项工作侧重于使用现成的 `LM`，通过使用 `ToT` 风格的高级反事实决策（例如，考虑下一段的潜在选择，而不是预测下一个标记）来微调 `LM`，可能会提供增强 `LM` 问题解决能力的机会。

**更广泛的影响。**`ToT` 是一个框架，使 `LM` 能够更自主、更智能地做出决策和解决问题。虽然当前的任务局限于推理和搜索问题，但未来涉及与外部环境或人类互动的应用可能会带来潜在的危险，例如促使 `LM` 的有害用途。另一方面，`ToT` 还提高了模型决策的可解释性和人类对齐的机会，因为所产生的表示是可读的、高级语言推理，而不是隐含的、低级的标记值。

**结论。**`LM` 的联想“系统1”可以通过基于搜索解决问题的可能路径的“系统2”有益地扩展。思维树框架提供了一种将有关问题解决的经典见解转化为当代 `LM` 可操作方法的方式。同时，`LM` 解决了这些经典方法的一个弱点，提供了一种解决不容易形式化的复杂问题的方法，例如创意写作。我们将 `LM` 与经典 `AI` 方法的交集视为未来工作的一个令人兴奋的方向。

## 参考文献

[1] T. Brown, B. Mann, N. Ryder, M. Subbiah, J. D. Kaplan, P. Dhariwal, A. Neelakantan, P. Shyam, G. Sastry, A. Askell, et al. Language models are few-shot learners. Advances in neural information processing systems, 33:1877–1901, 2020.

[2] C. Browne, E. J. Powley, D. Whitehouse, S. M. M. Lucas, P. I. Cowling, P. Rohlfshagen, S. Tavener, D. P. Liebana, S. Samothrakis, and S. Colton. A survey of monte carlo tree search methods. IEEE Transactions on Computational Intelligence and AI in Games, 4:1–43, 2012.

[3] M. Campbell, A. J. Hoane Jr, and F.-h. Hsu. Deep blue. Artificial intelligence, 134(1-2):57–83, 2002.

[4] X. Chen, M. Lin, N. Sch ¨arli, and D. Zhou. Teaching large language models to self-debug, 2023.

[5] A. Chowdhery, S. Narang, J. Devlin, M. Bosma, G. Mishra, A. Roberts, P. Barham, H. W. Chung, C. Sutton, S. Gehrmann, et al. Palm: Scaling language modeling with pathways. arXiv preprint arXiv:2204.02311, 2022.

[6] N. D. Daw, Y. Niv, and P. Dayan. Uncertainty-based competition between prefrontal and dorsolateral striatal systems for behavioral control. Nature neuroscience, 8(12):1704–1711, 2005.

[7] L. Gao, A. Madaan, S. Zhou, U. Alon, P. Liu, Y. Yang, J. Callan, and G. Neubig. Pal: Program-aided language models, 2023.

[8] P. E. Hart, N. J. Nilsson, and B. Raphael. A formal basis for the heuristic determination of minimum cost paths. IEEE Transactions on Systems Science and Cybernetics, 4(2):100–107,1968 doi: 10.1109/TSSC.1968.300136.

[9] P. E. Hart, N. J. Nilsson, and B. Raphael. A formal basis for the heuristic determination of minimum cost paths. IEEE transactions on Systems Science and Cybernetics, 4(2):100–107, 1968.

[10] W. Huang, P. Abbeel, D. Pathak, and I. Mordatch. Language models as zero-shot planners: Extracting actionable knowledge for embodied agents, 2022.

[11] W. Huang, F. Xia, T. Xiao, H. Chan, J. Liang, P. Florence, A. Zeng, J. Tompson, I. Mordatch, Y. Chebotar, et al. Inner monologue: Embodied reasoning through planning with language models. arXiv preprint arXiv:2207.05608, 2022.

[12] D. Kahneman. Thinking, fast and slow. Macmillan, 2011.

[13] D. Kahneman, S. Frederick, et al. Representativeness revisited: Attribute substitution in intuitive judgment. Heuristics and biases: The psychology of intuitive judgment, 49(49-81):74, 2002.

[14] G. Kim, P. Baldi, and S. McAleer. Language models can solve computer tasks, 2023.

[15] B. Liu, Y. Jiang, X. Zhang, Q. Liu, S. Zhang, J. Biswas, and P. Stone. Llm+p: Empowering large language models with optimal planning proficiency, 2023.

[16] X. Lu, S. Welleck, P. West, L. Jiang, J. Kasai, D. Khashabi, R. L. Bras, L. Qin, Y. Yu, R. Zellers, N. A. Smith, and Y. Choi. Neurologic a*esque decoding: Constrained text generation with lookahead heuristics. In North American Chapter of the Association for Computational Linguistics, 2021.

[17] A. Madaan, N. Tandon, P. Gupta, S. Hallinan, L. Gao, S. Wiegreffe, U. Alon, N. Dziri, S. Prabhumoye, Y. Yang, S. Welleck, B. P. Majumder, S. Gupta, A. Yazdanbakhsh, and P. Clark. Self-refine: Iterative refinement with self-feedback, 2023.

[18] A. Newell, J. C. Shaw, and H. A. Simon. Report on a general problem solving program. In IFIP congress, volume 256, page 64. Pittsburgh, PA, 1959.
