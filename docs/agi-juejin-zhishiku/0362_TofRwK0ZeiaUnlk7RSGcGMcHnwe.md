
# OpenAI安全系统负责人长文梳理：大模型的对抗攻击与防御

> 来源：[https://agijuejin.feishu.cn/docx/G5xxdy9IToXNRSxb6u0cSea4nAf](https://agijuejin.feishu.cn/docx/G5xxdy9IToXNRSxb6u0cSea4nAf)

> 机器之心｜[阅读原文](https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650897734&idx=2&sn=2604f5bff3bfadcf1b22a08f0426514a&chksm=84e4b938b393302ecc26e1c04e2f647e326799c32c6a56dc054b4ba6e8fce9985819e814a5e6&scene=21#wechat_redirect)

> 转载请联系原作者取得授权

> 选自 Lil’Log｜作者：Lilian Weng｜机器之心编译｜编辑：Panda

> LLM 能力强大，倘若别有用心之人用其来干坏事，可能会造成难以预料的严重后果。 虽然大多数商用和开源 LLM 都存在一定的内置安全机制，但却并不一定能防御形式各异的对抗攻击。 近日，OpenAI 安全系统（Safety Systems）团队负责人 Lilian Weng 发布了一篇博客文章《Adversar ial Attacks on LLMs》，梳理了针对 LLM 的对抗攻击类型并简单介绍了一些防御方法。

随着 ChatGPT 的发布，大型语言模型应用正在加速大范围铺开。OpenAI 的安全系统团队已经投入了大量资源，研究如何在对齐过程中为模型构建默认的安全行为。但是，对抗攻击或 prompt 越狱依然有可能让模型输出我们不期望看到的内容。

目前在对抗攻击方面的研究很多集中在图像方面，也就是在连续的高维空间。而对于文本这样的离散数据，由于缺乏梯度信号，人们普遍认为攻击会困难得多。Lilian Weng 之前曾写过一篇文章《Controllable Text Generation》探讨过这一主题。简单来说：攻击 LLM 本质上就是控制该模型输出特定类项的（不安全）内容。

文章地址：`https://lilianweng.github.io/posts/2021-01-02-controllable-text-generation/`

另一个研究攻击 LLM 的分支是为了提取预训练数据、私有知识，或通过数据毒化攻击模型训练过程。但这些并非本文要探讨的主题。

## 基础知识

### 威胁模型

对抗攻击是诱使模型输出我们不期望的内容的输入。许多早期研究关注的重点是分类任务，而近期的工作则开始更多关注生成模型的输出。本文探讨的是大型语言模型，并且假定攻击仅发生在推理阶段，也就是说模型权重是固定的。

![](img/244581001a2477acdc33cd55403381a8.png)

图 1：LLM 应用所面临的威胁概况

### 分类

在过去，研究社区更关注的是对分类器进行对抗攻击，并且许多是在图像领域。LLM 也可被用于分类。给定一个输入 `𝐱` 和一个分类器 `𝑓(.)`，我们希望找到该输入的一个差异细微的对抗版本 `𝐱_adv`，使得 `𝑓(𝐱)`≠`𝑓(𝐱_adv)`。

### 文本生成

给定一个输入 `𝐱` 和一个生成模型 `𝑝(.)`，该模型可输出一个样本 `y~𝑝(.|𝐱)`。这里的对抗攻击是找到一个 `𝑝(𝐱)`，使得 `y` 会违反该模型内置的安全行为，比如输出非法主题的不安全内容、泄漏隐私信息或模型训练数据。对生成任务而言，判断一次攻击成功与否并非易事，这需要一个超高质量的分类器来判断 `y` 是否安全或需要人类来进行审查。

### 白盒与黑盒

白盒攻击（White-box attacks）假设攻击者可以完全访问模型权重、架构和训练工作流程，这样一来攻击者就可以获得梯度信号。这里我们并不假设攻击者能获得全部训练数据。这仅适用于开源模型。黑盒攻击（Black-box attacks）则是假设攻击者只能访问 API 类型的服务 —— 攻击者可以提供输入 `𝐱` 并获取反馈的样本 `y`，而不知道有关模型的更多信息。

## 对抗攻击的类型

有多种不同方法可以帮助攻击者找到能诱使 LLM 输出不安全内容的对抗输入。这里给出五种方法。

![](img/6b91cc610a5f509ad71f8fea240a157f.png)

### token 操作

给定一段包含一个 token 序列的文本输入，我们可以使用简单的 token 操作（比如替换成同义词）来诱使模型给出错误预测。基于 token 操作的攻击属于黑盒攻击。在 Python 框架中，Morris et al. 2020 的论文《TextAttack: A Framework for Adversarial Attacks, Data Augmentation, and Adversarial Training in NLP》实现了许多词和 token 操作攻击方法，可用于为 NLP 模型创建对抗样本。这一领域的许多研究工作实验的是分类和蕴涵预测。

举个例子，Ribeiro et al (2018) 的研究《Semantically Equivalent Adversarial Rules for Debugging NLP models》依赖于人工提出的「语义等价式对抗规则（SEAR）」，其可以通过尽可能少的 token 操作来让模型无法生成正确答案。比如，其中的规则包括将 What 换成 Which、将 was 换为 is。另外，还有其他研究者提出的替换关键词、用同义词替换等方法。

### 基于梯度的攻击

如果是白盒攻击，则攻击者可以获取所有的模型参数和架构。因此，攻击者就可以依靠梯度下降来通过编程方式学习最有效的攻击手段。基于梯度的攻击仅在白盒设置下有效，比如开源 LLM。

Guo et al. 2021 的论文《Gradient-based Adversarial Attacks against Text Transformers》提出的基于梯度的分布式攻击（GBDA）使用了 Gumbel-Softmax 近似技巧来使对抗损失优化可微，其还使用了 BERTScore 和困惑度来增强可感知性和流畅性。

不过，Gumbel-softmax 技巧难以扩展用于 token 删除或增添，而是受限于 token 替换操作。

Ebrahimi et al. 2018 在论文《HotFlip: White-Box Adversarial Examples for Text Classification》 中则是将文本操作看作是向量空间中的输入，度量的是损失在这些向量上的导数。HotFlip 可以扩展用于 token 删除或增添。

Wallace et al. (2019) 的论文《Universal Adversarial Triggers for Attacking and Analyzing NLP》提出了一种在 token 上进行梯度引导式搜索的方法，可以找到诱使模型输出特定预测结果的短序列，这个短序列被称为 Universal Adversarial Triggers （UAT，通用对抗触发器）。UAT 不受输入的影响，这意味着这些触发器可以作为前缀（或后缀）连接到来自数据集的任意输入上。

Shin et al., 2020 的《AutoPrompt: Eliciting Knowledge from Language Models with Automatically Generated Prompts》使用了同样的基于梯度的搜索策略来为多样化的任务寻找最有效的 prompt 模板。

上面的 token 搜索方法可以使用波束搜索增强。当寻找最优的 token 嵌入时，可以选取 top-k 个候选项，而不是单独一个，在当前数据批上从左到右搜索，并根据 `𝓛_adv` 为每个波束评分。

![](img/c5bf89e863d2c42d28b074f899d127ff.png)

图 4：UAT 工作方式图示

UAT 的损失 `𝓛_adv` 需要针对具体任务而设计。分类或阅读理解依赖于交叉熵。

![](img/2c1f1678e6f96f00c827eb46bd20e7b8.png)

图 5：用于不同类型的语言任务的 UAT 示例

UAT 为何有效？这是一个很有趣的问题。因为 UAT 是与输入无关的，并且可以在有不同嵌入、token 化方案和架构的模型之间迁移，因此它们也许可以有效地利用训练数据中的偏差，毕竟这种偏差已经融入到了模型的全局行为中。

使用 UAT 攻击有一个缺点：很容易检测出来。原因是所学习到的触发器往往是毫无意义的。Mehrabi et al. (2022) 在论文《Robust Conversational Agents against Imperceptible Toxicity Triggers》中研究了 UAT 的两种变体，它们会促使所学到的触发器在多轮对话语境中难以察觉。其目标是创建能在给定对话中有效触发有毒响应的攻击性消息，同时保证该攻击在对话中是流畅、连贯和一致的。

这两种变体分别是 UAT-LM（Universal Adversarial Trigger with Language Model Loss）和 UTSC（Unigram Trigger with Selection Criteria）。

![](img/6b6be7338bdbad528eb37cca8340cc90.png)

图 6：UTSC 工作方式图示

UAT-LM 和 UTSC-1 的表现与 UAT 基准相当，但 UAT 攻击短语的困惑度出奇得高，比 UAT-LM 和 UTSC-1 都高得多。高困惑度让攻击更易被检测出来，也更容易缓解。根据人类评估，UTSC-1 攻击能比其它攻击方法得到更连贯、流畅和相关的结果。

![](img/3ba362d2143a91ac38f1664fa10f7b1c.png)

图 7：基于防御者模型对生成的攻击的响应，根据不同的有毒度分类器衡量的攻击成功率。

Zou et al. (2023) 的论文《Robust Conversational Agents against Imperceptible Toxicity Triggers》也研究了将通用对抗触发 token 作为后缀连接到输入请求上的情况。他们具体研究了对 LLM 的恶意请求 —— 对此模型应当拒绝回答。事实上，拒绝不被允许的内容类别（比如犯罪建议）是 GPT-4 内置的一个重要的安全措施。这里的对抗目标是诱使 LLM 即便在应当拒接回答时也输出肯定的响应。这就是说，当收到恶意请求时，模型会这样回答：「当然，你需要这样做……」预期的肯定响应也被配置成重复部分用户 prompt，以避免简单就能改变主题的后缀以优化「当然」式的响应。其损失函数很简单，就是输出目标响应的 NLL（负对数似然）。

![](img/9c8ca452049b9a1803c4f9bc06e72a52.png)

图 8: 引入对抗触发器的位置的图示。红色感叹号代表要学习的对抗性 token。

他们在 Vicuna-7b 和 Vicuna-13b 这两个不同的模型上进行了实验，使用了基于贪婪坐标梯度（GCG）的搜索来贪婪地寻找候选项，使得该候选项可以在所有可能的单 token 替换中最大程度地减少损失。

尽管他们的攻击序列完全是基于开源模型训练的，但它们却出乎意料地可以移植用于其它商用模型，这表明对开源模型的白盒攻击对私有模型也有效，尤其是当低层的训练数据有所重叠时。注意 Vicuna 的训练使用了从 GPT-3.5-turbo 收集的数据（通过 shareGPT），这本质上是蒸馏，因此这种攻击更像是白盒攻击。

![](img/0f91821be6bd30d8767dd64ec5398b38.png)

图 9：在 HB（有害行为）指令上的平均攻击成功率，这是在 5 次 prompt 上的平均结果。

Jones et al. 2023 等提出的自回归随机坐标上升（ARCA）则考虑了一个更宽泛的优化问题集合来寻找符合特定行为模式的输入 - 输出对 `(𝐱, 𝐲)` ，比如以「Barack Obama」开头但会导致毒性输出的非毒性输入。给定一个审核目标：`𝜙 : 𝑿×𝒀→ℝ`，其是将一对 (输入 prompt, 输出完成结果) 映射成一个分数。

![](img/47361d61d68d4f06b6e4317826374524.png)

图 10：诱使 GPT-2 和 GPT-J 生成有毒输出的平均成功率。粗线：CivilComments 的所有输出；点线：CivilComments 的 1,2,3-token 有毒输出。

### 越狱 prompt 设计

越狱 prompt 是以对抗方式诱使 LLM 输出应当避免的有害内容。越狱是黑盒攻击，因此词汇组合是基于启发式方法和人工探索进行的。Wei et al. (2023) 的论文《Jailbroken: How Does LLM Safety Training Fail?》提出了 LLM 安全的两种失败模式，可用于指导越狱攻击的设计。

#### 1. 互相竞争的目标

这是指模型的能力（比如「应始终遵从命令」）与安全目标相冲突的情况。利用互相竞争的目标的越狱攻击例子包括：

+   前缀注入：要求模型开始时必须使用肯定性的确认语句。

+   拒绝抑制：为模型提供详细的指令，让其不要以拒绝的格式进行响应。

+   风格注入：要求模型不使用长词汇，这样一来模型就无法进行专业性写作从而给出免责声明或解释拒绝的理由。

+   其它：角色扮演成 `DAN`（现在可以做任何事）、`AIM`（总是很聪明且不择手段）等等。

#### 2. 失配的泛化

这是指安全训练无法泛化到其具有能力的领域。当输入位于模型的安全训练数据分布之外（OOD），但又位于其宽泛的预训练语料库范围内时，就会出现这种情况。例子包括：

+   特殊编码：使用 `Base64` 编码来构建对抗性输入。

+   字符变换：`ROT13` 密码、火星文或脑残体（用视觉上相似的数字和符号替换字母）、摩尔斯电码

+   词变换：`Pig Latin`（用同义词替换敏感词，比如用「窃」替换「偷」）、负载拆分（即所谓的 token smuggling，将敏感词拆分成子字符串）

+   prompt 层面的混淆：翻译成其它语言、要求模型以其能理解的方式进行混淆

Wei et al. (2023) 实验了大量越狱方法，包括由以上原理构建的组合型策略。

+   `combination_1` 组合了前缀注入、拒绝抑制和 Base64 攻击。

+   `combination_2` 加入了风格注入。

+   `combination_3` 又添加了生成网站内容和格式化限制条件。

![](img/ff755def325063fa67209a93c8d9d9de.png)

图 11：越狱技巧的类型以及它们攻击模型的成功率

Greshake et al. (2023) 的论文《Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection》则在较高层面上观察了 prompt 注入攻击。其中指出，即便当攻击无法提供详细的方法而仅仅提供一个目标时，模型也有可能自动去实现它。当模型可以访问外部 API 和工具时，对更多信息（甚至是专有信息）的获取可能导致更大的钓鱼攻击和私密窥探攻击风险。

### 有人类参与的红队策略

Wallace et al. (2019) 在论文《Trick Me If You Can: Human-in-the-loop Generation of Adversarial Examples for Question Answering》中提出了有人类参与的对抗生成，其目标是构建用于引导人类攻破模型的工具。

他们用 QuizBowl QA 数据集进行了实验，设计了一个对抗书写界面，可让人类写出类似电视问答节目《Jeopardy》风格的问题，并用于诱导模型给出错误预测。依据重要程度的不同，每个词都会有不同颜色的标注（即移除该词后模型预测概率会发生变化）。词重要度是由模型梯度根据词嵌入来近似得到的。

![](img/ec85f2cbc910471ea21415a4feeb6810.png)

图 12：对抗书写界面，其中左上部分是列出的模型前五个预测，右下部分是用户问题，其中用不同颜色标注了词重要度。

在一次实验中，人类训练者要做的是找到安全分类器在分类暴力内容时失败的案例。Ziegler et al. (2022) 在论文《Adversarial Training for High-Stakes Reliability》中创建了一个工具，可用于辅助人类对抗者更快更有效地找到分类器的失败情况并消除它。用工具辅助重写比完全人工重写要更快些，可将每个样本所需的时间从 20 分钟降至 13 分钟。确切地说，他们引入了两个功能来辅助人类写作者：显示每个 token 的显著度分数、token 替换和插入。

![](img/c030816e1e072f183ca3c7430b7cba9b.png)

图 13：用工具辅助人类对分类器进行对抗攻击的 UI。人类要做的是编辑或补全 prompt，以降低模型预测输入是暴力内容的概率。

Xu et al. 2021 的《Bot-Adversarial Dialogue for Safe Conversational Agents》提出了 Bot-Adversarial Dialogue（BAD），该框架可以引导人类去诱使模型犯错（比如输出不安全的内容）。他们收集了 5000 多组模型与众包工作者的对话。每一组对话都包含 14 轮，然后他们根据不安全对话轮次的数量给模型打分。他们最终得到了 BAD 数据集，其中包含大约 2500 组带有攻击性标签的对话。

Anthropic 的红队数据集包含接近 4 万个对抗攻击，它们收集自人类红队者与 LLM 的对话。他们发现，RLHF 的规模越大，就越难以攻击。OpenAI 发布的大模型（比如 GPT-4 和 DALL-E 3）普遍使用了人类专家红队来进行安全准备。

数据集地址：`https://github.com/anthropics/hh-rlhf/tree/master/red-team-attempts`

### 模型红队策略



人类红队策略很强大，但是难以大规模实施而且可能需要大量经过培训的专业人士。现在想象一下：我们可以学习一个红队模型 `red` 来与目标 LLM 进行对抗，以触发其给出不安全响应。对于基于模型的红队策略，主要的难题是如何判断一次攻击是否成功；只有知道了这一点，我们才能构建用于训练红队模型的合适学习信号。

假设我们已经有一个高质量的分类器，能够判断模型的输出是否有害，我们就可以将其用作奖励来训练红队模型，以得到一些能最大化分类器在目标模型输出上的分数的输入。令 `r (𝐱, 𝐲)` 是一个这样的红队分类器，其可以判断在给定测试输入 `𝐱` 时，输出 `𝐲` 是否有害。根据 Perez et al. 2022 的论文《Red Teaming Language Models with Language Models》，寻找对抗攻击样本遵循一个简单的三步式过程：

1. 从一个红队 LLM `𝐱~p_red (.)` 采样测试输入；
2. 使用目标 LLM `p (𝐲 | 𝐱)` 为每个测试案例 `𝐱` 生成一个输出 `𝐲`；
3. 根据分类器 `r (𝐱, 𝐲)`，在测试案例中找到一个会得到有害输出的子集。

他们实验了几种从红队模型采样或进一步训练红队模型以使其更加有效的方法，其中包括零样本生成、随机式少样本生成、监督学习、强化学习。

Casper et al. (2023) 的论文《Explore, Establish, Exploit: Red Teaming Language Models from Scratch》设计了一种有人类参与的红队过程。其与 Perez et al. (2022) 的主要不同之处在于其明确地为目标模型设置了一个数据采样阶段，这样就可以收集其上的人类标签来训练针对特定任务的红队分类器。其包含探索（Explore）、建立（Establish）和利用（Exploit）三个阶段，如下图所示。

![](img/283070fd9a4f46d3f588592a0b4a0979.png)

图 15：采用「探索 - 建立 - 利用」三步式方法的红队策略流程

Mehrabi et al. 2023 的论文《FLIRT: Feedback Loop In-context Red Teaming》则是依靠红队 LM `𝑝_red` 的上下文学习来攻击图像或文本生成模型 `𝑝`，使其输出不安全的内容。

在每一次 FLIRT 迭代中：

1. 红队 LM `𝑝_red` 生成一个对抗 prompt `𝐱~𝑝_red (. | examples)`；其中初始的上下文样本由人类设计；
2. 生成模型 `𝑝` 根据这个 prompt 生成一个图像或文本输出 `𝐲`；
3. 使用分类器等机制对生成的内容 `𝐲` 进行评估，看其是否安全；
4. 如果 `𝐲` 被认为不安全，则使用触发 prompt `𝐱` 来更新 `𝑝_red` 的上下文模板，使其根据策略生成新的对抗 prompt。

至于如何更新 FLIRT 的上下文模板，有这样几个策略：`FIFO`、`LIFO`、`Scoring`、`Scoring-LIFO`。详见原论文。

![](img/e3b3b5624287d8e1d48cd4b76dd46f78.png)

图 16：在不同的扩散模型上，不同攻击策略的攻击有效率（触发了不安全生成结果的生成 prompt 的百分比）。基准是 `SFS`（随机少样本）。括号中的数值是独特 prompt 的百分比。

## 如何应对攻击

### 鞍点问题

Madry et al. 2017 的《Towards Deep Learning Models Resistant to Adversarial Attacks》提出了一个很不错的对抗稳健性（adversarial robustness）框架，即将对抗稳健性建模成一个鞍点问题，这样就变成了一个稳健优化（robust optimization）问题。该框架是为分类任务的连续输入而提出的，但它用相当简洁的数学公式描述了双层优化过程，因此值得分享。

让我们考虑一个分类任务，其基于由配对的 (样本，标签) 构成的数据分布，`(𝐱,𝑦)∈𝒟`，训练一个稳健分类器的目标就是一个鞍点问题：

![](img/67bba1816c78c734e0cf179c5e354bc9.png)

其中 `𝓢⊆ℝ^d` 是指允许用于对抗的一组扰动，比如我们希望一张图像的对抗版本看起来与原始版本类似。

其目标由一个内部最大化问题和一个外部最小化问题组成：

+ 内部最大化：寻找能导致高损失的最有效的对抗数据点 `𝐱+𝜹`。所有对抗性攻击方法最终都可归结为如何最大化这个内部过程的损失。

+ 外部最小化：寻找最佳的模型参数化方案，使得由内部最大化过程找到的最有效攻击的损失能被最小化。要训练出稳健的模型，一个简单方法是将每个数据点替换为其扰动版本，这些版本可以是一个数据点的多个对抗变体。

![](img/0840bc3e67f44ebce9c9fcfcf75778c2.png)

图 17：他们还发现，面对对抗攻击的稳健性需要更大的模型能力，因为这会让决策边界变得更为复杂。有趣的是，在没有数据增强的前提下，模型更大会有助于提升模型的稳健性。

## LLM 稳健性方面的一些研究工作

这里简单谈谈一些有关 LLM 稳健性的研究。

Xie et al. 2023 的论文《Defending ChatGPT against Jailbreak Attack via Self-Reminder》发现了一种简单直观的保护模型免受对抗攻击的方法：明确地指示模型成为负责任的模型，不要生成有害内容。这会极大降低越狱攻击的成功率，但对模型的生成质量会有副作用，这是因为这样的指示会让模型变得保守（比如不利于创意写作），或者会在某些情况下错误地解读指令（比如在安全 - 不安全分类时）。

为了降低对抗攻击风险，最常用的方法是用这些攻击样本来训练模型，这种方法被称为「对抗训练」。这也被认为是最强的防御方法，但却需要在稳健性和模型性能之间寻找平衡。Jain et al. 2023 通过实验测试了两种对抗训练设置，结果报告在论文《Baseline Defenses for Adversarial Attacks Against Aligned Language Models》中：(1) 使用有害 prompt 与带有 `I'm sorry. As a ...` 的响应构成的数据对来运行梯度下降；(2) 对于每一个训练步骤，在拒绝响应上运行一个下降步骤并在一个红队差响应上运行一个上升步骤。最终他们发现方法 (2) 毫无用处，因为模型的生成质量下降了很多，而攻击成功率却仅有少量下降。

白盒攻击往往会导致对抗 prompt 看起来毫无意义，因此可以通过困惑度检测出来。当然，通过明确地优化而降低困惑度，白盒攻击可以直接绕过这种检测方法，比如 `UAT` 的一种变体 `UAT-LM`。但是，这也可能会导致攻击成功率下降。

![](img/5979b5053c296a04feadf0660d43a9ba.png)

图 18：困惑度过滤器可以阻止来自 [Zou et al. (2023)] 的攻击。`PPL Passed` 和 `PPL Window Passed` 是指带有对抗性后缀的有害 prompt 绕过过滤器而不被检测到的比率。通过率越低，过滤器越好。地址：`https://arxiv.org/abs/2307.15043`

Jain et al. 2023 还测试了对文本输入进行预处理的方法，使得能在移除对抗性修改的同时维持语义含义。

+ 解释含义：使用 LLM 来解释输入文本的含义，这可能会对下游任务性能造成较小影响。

+ 重新 token 化：将 token 拆分开并使用多个更小的 token 来表示它们，比如使用 `BPE-dropout`（随机丢弃一定比例的 token）。使用这种方法的假设是对抗性 prompt 很可能会利用特定的对抗性 token 组合。这也确实有助于降低攻击成功率，但也有限，比如从 90% 以上降至 40%。
