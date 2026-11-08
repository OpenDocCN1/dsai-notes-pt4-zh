# 智能体AI Agent的极速入门：从ReAct、AutoGPT到AutoGen

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Y7oudiVSWoI9pjxtKiZchLYanBb](https://v11enp9ok1h.feishu.cn/docx/Y7oudiVSWoI9pjxtKiZchLYanBb)

前言

如这两天在🧣上所说，除了已经在七月官网上线的AIGC模特生成系统外，我正在并行带多个项目组

第二项目组，论文审稿GPT第2版的效果已经超过了GPT4，详见《七月论文审稿GPT第2版：用一万多条paper-review数据集微调LLaMA2最终反超GPT4》，预计今年4月份对外发布，且还在推进第2.5版

第三项目组，RAG知识库问答，春节之前第一版即OK

第四项目组，大模型机器人项目，目前正在推进对斯坦福mobile aloha的复现

第五项目组，便是本文要涉及的Agent项目，目前先做一系列技术调研(故而有的本文)，3月份会公布我们的产品形态

这些项目只要不断推进 可以做的很大，且最终大家能做出结果，不断实践、不断尝试是第一关键，至于能力是在这个过程中不断提高的，毕竟过程中有的是提高机会

而对于本文要讲的Agent，其实去年4月份随着Auto-GPT的爆火(其中最基础的组件之一便是ReAct)，便带动AI Agent越来越火了，自此

看好他的人很多，甚至有些朋友还会有夸大其词之嫌，说马上要取代人类大部分工作了，当然 他们实际去实践搭一个Agent的很少，即便少数兴致冲冲搭了，但发现没有传说中那么神，便不免心生失落

觉得概念大于实际的人也很多，在他们看来，这一切都是炒作，虽然他们五年前认为AI是上一个炒作，觉得和区块链、Web3、元宇宙一样都在概念中，后来发现AI确实是赋能千行百业 没法“喷”，便“喷”去年兴起的大模型，也是同样的说辞 说都在概念中，一年过去 他们不“喷”大模型了，现在改“喷”Agent了

在我看来，太过看好、和太过不看好，都有失偏颇，真正不失偏颇的看法是，既认识到其威力，也认识到其局限才是严谨客观的态度，本文便在这种态度之下诞生

第一部分 什么是ReAct：Auto-GPT的基础组件

ReAct其实不是刚出来的概念，它于2022年10月份便由Google Research 的 Brain Team 通过此篇论文《ReAct: Synergizing Reasoning and Acting in Language Models》提出来了，没错，又是Google的建设性工作之一，曾一度感觉，没有Google(毕竟Transformer、指令微调、CoT等哪个不是Google的杰作，包括RLHF也是Google deepmind和OpenAI联合推出来的)，就没有后来的ChatGPT，^_^

1.1 如何理解ReAct：Reasoning(协同推理) + Acting(行动)

1.1.1 HotpotQA与AlfWorld中的示例：寻找遥控器之外控制苹果TV的工具、把胡椒粉放到抽屉上

简而言之，ReAct = Reasoning(协同推理) + Acting(行动)，但具体怎么理解这个ReAct呢，比如针对以下两个示例

除了Apple Remote，还有哪些其他设备可以用于控制“最初设计用来交互的程序”？

如果是标准思路的话，可能出现错误回答：iPod

如果是用一步步推理的CoT的话，虽然看起来有模有样，但最终结果还是错的(因为它给的答案——iPhone iPad iPod没法控制苹果TV)

如果单纯做多步搜索呢？虽然通过Act的方式，搜索出来好几种答案，但最终还是没找对

只有通过ReAct(reazon + Act)，它会每采取一步行动之后，然后驻足观察且思考，从而推理出下一步应该采取什么行动

想法1：我需要搜索Apple Remote，找到其最初设计用于互动的程序

行为1：进行[Apple Remote]的搜索

观察1：Apple Remote是苹果公司在2005年10月推出的一款遥控器，最初设计用于控制“Front Row”的媒体中心程序......

想法2：Apple Remote最初被设计用于控制“Front Row”媒体中心程序。接下来，我需要搜索“Front Row”，找到其他可以对其进行控制的设备

行为2：进行[前排]的搜索

观察2：无法找到[Front Row]。类似结果包括：“地球Front Row座位”，“Front Row赛车”，“Front Row[软件]”...

想法3：由于未能找到“Front Row”，我需要搜索关于“Front Row”的软件

行为3：进行[Front Row(软件)]的搜索。

观察3：发现原来是一个已停产的媒体中心软件——Front Row

想法4："Front Row"可以通过Apple Remote或键盘功能键来控制，因此答案应该是键盘功能键

行为4: 完成对[键盘功能键] 的学习

在一间房子里，有一个6号橱柜、一个1号橱柜、一台1号咖啡机、一块3号工作台面、一个1号炉灶和一个1号烤面包机，您的任务是将一些胡椒瓶放在抽屉上

如果只是盲目行动 没有思考或推理的话

前往抽屉1

观察1：抽屉1处于关闭状态

打开抽屉1

观察2：您成功打开了抽屉1，它现在是打开的。里面有一个洗碗海绵2和一个勺子1

前往洗脸盆1

观察3：在洗脸盆上，您发现了一个洗碗海绵3、一把切菜刀和两个勺子

从洗脸盆取出胡椒瓶

观察4：没有任何变化发生

再次从洗脸盆取出胡椒瓶

观察5: 仍然没有任何变化发生

那如果是边行动 边观察 便推理的模式呢？则会有以下一系列步骤

行动1：思考[首先我需要找到一个胡椒瓶...... 更有可能出现在橱柜（1-6），台面（1-3），......

行动2：前往橱柜1

观察2：在橱柜1上，您发现了一个花瓶2

(智能体按顺序前往橱柜1、然后是橱柜2，接着是台面1和台面2)

行动6：前往台面3

观察6：在台面3上，您看到了一个苹果1、一个面包1...... 以及一个胡椒瓶1和一个花瓶3

行动7：从台面3取下胡椒瓶1

观察7：您从台面3取下了胡椒瓶1

行动8：思考[现在我已经找到了胡椒瓶1。接下来，我需要将其放入第一抽屉中

行动9：走向第一抽屉

观察9: 第一抽屉被关闭着

行动10: 打开第一抽屉

观察10: 您打开了第一抽屉……

行动11: 将胡椒瓶放入第一抽屉内/上方

观察11: 您将胡椒瓶放入了第一抽屉内/上方

1.1.2 设计Agent之prompt的关键点

所以，在设计Agent的时候，需要设计好prompt，其包含几个元素：

思考(Thought)

推理过程的文字展示，我要干什么，或者说我想要 LLM 帮我做什么，以及为了达成这件事情所需要的前置条件是什么

行动(Act)

生成与外部交互的指令，确定这一步要做什么之后所生成的对应行为指令文字，比如遇到了 LLM 没有预设的知识，要进行搜索

观察(Obs)

从外部获取执行指令得到的结果，相当于拿到当前这一步的行为的结果，准备进行下一步，比如如果是搜索的话，这里就会是搜索结果

自此，你有没发现，再加上根据观察得到的结果反馈之后，不就是强化学习那一套么？不过，谁说不是呢，之所以称之为智能体，就是希望其有根据观察结果具备下一步应该怎么做的推理 + 思考能力，这不也是RL的目标么(关于什么是RL，参见：强化学习极简入门，当然，RL中，这个奖励函数的制定并不容易)

Auto-GPT 便是参考了当前的框架，从而表现的非常出色。在未来如果 LLM 要成为 AGI，也许这个框架是一个基础的行动框架，而如果需要在机器人或者复杂的虚拟环境中使用 GPT 来做为大脑，让它自己思考、生成行动、获取执行后的结果、再次思考并计划下一步这样的路径必不可少

1.2 如何从公式层面定义ReAct

1.2.1 智能体与环境交互以解决任务的一般设置

考虑一个智能体与环境交互以解决任务的一般设置

在时间步骤，智能体从环境接收到一个观察，并采取一个行动，遵循某些策略，其中是智能体的上下文

当映射具有高度隐式性且需要大量计算时，学习策略变得具有挑战性

例如，下图(1c)中显示的智能体无法生成正确的最终动作(Act 4)完成QA任务，因为它需要在轨迹上下文(Question, Act 1-3, Obs 1-3)上进行复杂的推理

同样，下图(2a)中显示的智能体无法从上下文中理解到sinkbasin 1(洗脸盆)不包含peppershaker 1(胡椒瓶)，因此继续产生幻觉般的行动

ReAct的思想很简单：我们增加智能体的行动空间到，其中是语言空间，语言空间中的行动，我们将其称为思考或推理痕迹，不会影响外部环境，因此不会产生观察反馈

相反，思考旨在通过对当前上下文进行推理来组合有用的信息，并更新上下文以支持未来的推理或行动

如上述两个图所示，可能存在各种类型的有用思考，例如

分解任务目标并创建行动计划

比如2b中的Act 1

行动1：思考[首先我需要找到一个胡椒瓶...... 更有可能出现在橱柜（1-6），台面（1-3），......

再比如1d中Thought 1)

想法1：我需要搜索Apple Remote，找到其最初设计用于互动的程序

注入与任务解决相关的常识知识(2b中的Act 1)

行动1：思考[首先我需要找到一个胡椒瓶...... 更有可能出现在橱柜（1-6），台面（1-3），......

从观察中提取重要部分(1d中的Thought2, 4)

想法2：Apple Remote最初被设计用于控制“Front Row”媒体中心程序。接下来，我需要搜索“Front Row”，找到其他可以对其进行控制的设备

想法4："Front Row"可以通过Apple Remote或键盘功能键来控制，因此答案应该是键盘功能键

跟踪进展和过渡行动计划(2b中的Act 8)

行动8：思考[现在我已经找到了胡椒瓶1。接下来，我需要将其放入第一抽屉中

处理异常并调整行动计划(1d中的Thought 3)等等

想法3：由于未能找到“Front Row”，我需要搜索关于“Front Row”的软件

然而，由于语言空间是无限的，在这个增强的动作空间中学习变得困难且需要具备深厚的语言先验知识(as the language space L is unlimited, learning in this augmented action space is difficult and requires strong language priors)

本文主要关注一个冻结的大型语言模型PaLM-540B，该模型利用有限上下文示例生成特定领域任务所需的动作和自由形式表达思想(1d/2b)

In this paper, we mainly focus on the setup where a frozen large language model, PaLM-540B (Chowdhery et al., 2022)1, is prompted with few-shot in-context examples to generate both domain-specific actions and free-form language thoughts for task solving(Figure 1 (1d), (2b)).

每个上下文中的例子都是人类在解决任务实例时所采取的行动、思考和环境观察轨迹(Each in-context example is a human trajectory of actions, thoughts, and environment observations to solve a task instance (see Appendix C))

对于推理比较关键的任务(比如寻找遥控器之外的交互设备)，我们交替生成思考和行动，以便任务解决轨迹由多个思考-行动-观察步骤组成

For the tasks where reasoning is of primary importance (Figure 1(1)), we alternate the generation of thoughts and actions so that the task-solving trajectory consists of multiple thought-action-observation steps.

相较而言，在涉及大量动作的决策任务中(把胡椒粉摆放到抽屉上方)，思想只需在轨迹的最相关位置稀疏地出现，因此我们让语言模型自主决定思想和动作的异步发生

In contrast, for decision making tasks that potentially involve a large number of actions (Figure 1(2)), thoughts only need to appear sparsely in the most relevant positions of a trajectory, so we let the language model decide the asynchronous occurrence of thoughts and actions for itself.

1.2.2 让「决策与推理能力集成到大模型中」的ReAct的特点

由于决策制定和推理能力集成到一个大型语言模型中，ReAct具有以下特点：

直观易用：设计ReAct提示很简单，因为人类注释者只需在他们采取的行动之上以语言方式记录下他们的思考。本文中没有使用任何特定的格式选择、思考设计或示例选择

通用和灵活：由于思考空间和思考-行动发生格式的灵活性，ReAct适用于具有不同行动空间和推理需求的各种任务，包括但不限于QA、事实验证、文本游戏和Web导航

性能和鲁棒性：ReAct展现出对新任务实例的强大泛化能力，而仅从一个到六个上下文示例中学习，始终在不同领域的推理或行动中表现优异

与人类对齐和可控性：ReAct承诺一种可解释的顺序决策制定和推理过程，人类可以轻松地检查推理和事实正确性。此外，人类还可以通过thought editing来控制或纠正智能体的行为(humans can also control or correct the agent behavior on the go by thought editing)

1.3 ReAct适合什么样的任务：知识密集型推理任务

1.3.1 通过HotPotQA和FEVER任务对比SC、COT、ReAct

考虑两个具有挑战性的知识检索和推理数据集：

HotPotQA，一个需要在两个或更多Wikipedia段落上进行推理的多跳问答基准(a multi-hop question answering)测试

FEVER，一个事实验证基准，其中每个声明都被标注为SUPPORTS，REFUTES或NOT ENOUGH INFO，方法是通过检查是否存在维基百科段落来验证该声明的事实程度

在这项工作中，采用了只提问问题的设置，其中模型仅接收问题/声明作为输入，并无法直接访问支持段落(we operate in a question-only setup for both tasks, where models only receive the question/claim as input without access to support paragraphs)，即它必须依靠内部知识或通过与外部环境交互来检索知识以支持推理(have to rely on their internal knowledge or retrieve knowledge via interacting with an external environment to support reasoning)

我们设计了一个简单的维基百科Web API，具有三种类型的操作，以支持交互式信息检索：

搜索[实体]，如果存在相应实体的wiki页面，则返回前5个句子，否则建议从Wikipedia搜索引擎中选择前5个相似实体

查找[字符串]，它将返回包含字符串的页面中的下一个句子，模拟浏览器上的Ctrl+F功能

完成[答案]，这将完成当前任务并给出答案。我们注意到，这个行动空间大多只能基于确切的段落名称检索到一小部分段落，这比最先进的词汇或神经检索器要弱得多

We note that this action space mostly can only retrieve asmall part of a passage based on exact passage name, which is significantly weaker than state-of-the-art lexical or neural retrievers

目的是模拟人类如何与维基百科进行交互，并强制模型通过语言中的显式推理来检索

The purpose is to simulate how humans would interact with Wikipedia,and force models to retrieve via explicit reasoning in language

ReAct提示：对于HotpotQA和Fever，我们从训练集中随机选择6和3个案例2，并手动组成ReAct格式轨迹，以作为提示的少量样本，类似于下图(1d)

每个轨迹由多个思考-行动-观察步骤(即密集思考)组成，其中自由形式的思考用于各种目的

具体来说，我们使用一组思考来分解问题（“我需要搜索x，找到y，然后找到z”），从维基百科观察中提取信息（“x始于1844年”，“该段落未提到x”），执行常识（“x不是y，所以z必须是...”）或算术推理（“1844<1989”），指导搜索重构（“也许我可以搜索/查找x”），并综合最终答案（“...所以答案是x”）

基线：我们系统地削弱ReAct轨迹，构建多个基线的提示(格式如上图1a-1c)：

1a)标准提示Standard，它删除了ReAct轨迹中的所有思考、行动和观察

1b)思维链提示CoT，它删除了行动和观察，并作为仅推理的基线。还通过在推理期间使用解码温度为0.7的21个CoT轨迹进行抽样并采用多数答案来构建自一致性基线(CoT-SC)，发现它能够提高CoT的性能

1c)仅操作提示(Act)，它删除了ReAct轨迹中的思考，粗略地类似于WebGPT与互联网交互以回答问题，虽然它执行不同的任务和行动空间，并使用模仿和强化学习而不是提示

而ReAct所展示的问题解决过程更加事实和基础，而CoT在制定推理结构方面更加准确，但容易受到虚构的事实或思想的影响。因此，建议将ReAct和CoT-SC结合起来，并让模型根据实际情况决定何时切换到其他方法：

A)ReAct→CoT-SC：当ReAct在给定步骤内无法返回答案时，回退到CoT-SC

当然，他们将HotpotQA和FEVER分别设置为7步和5步，因为发现步骤更多不会提高ReAct的性能

B)CoT-SC→ReAct：当n个CoT-SC样本中的多数答案出现少于n/2次时(即内部知识可能不会自信地支持任务)，回退到ReAct

总之，ReAct + CoT-SC对促进LLMs效果最佳，在HotpotQA和Fever上，最佳的提示方法分别为ReAct → CoT-SC和CoT-SC → ReAct

此外，图2(即上图右侧部分)显示了不同方法在使用不同数量的CoT-SC样本时的表现。虽然两种ReAct + CoT-SC方法在各自的任务中具有优势，但它们都显著并一致地优于CoT-SC，跨越不同数量的样本，仅使用3-5个样本即可达到CoT-SC的性能。这些结果表明，合理地结合模型内部知识和外部知识对于推理任务至关重要

1.3.2 对ReAct的微调

鉴于手动注释大规模推理轨迹和动作存在挑战，考虑采用类似于Zelikman等人(2022)的自举方法

利用ReAct生成了3000条正确答案轨迹来微调较小的语言模型(PaLM-8/62B)，以便在输入问题/主张条件下解码轨迹(包括所有想法、动作和观察)

Due to the challenge of manually annotating reasoning traces and actions at scale,we consider a bootstraping approach similar to Zelikman et al. (2022)

using 3,000 trajectorieswith correct answers generated by ReAct (also for other baselines) to finetune smaller language models (PaLM-8/62B) to decode trajectories (all thoughts, actions, observations) conditioned on input questions/claims

下图显示了四种方法(标准、CoT、Act、ReAct)在HotpotQA上提示/微调的扩展效果，其中，ReAct在微调中表现最佳

使用PaLM-8/62B，由于难以从上下文示例中学习推理和行动，因此“prompt ReAct”是四种prompt方法中表现最差的(即上图左侧前两个区域中，红色柱子最矮)

然而，当仅使用3,000个示例进行微调时，ReAct在4种方法中成为最佳方法(即上图右侧的两个区域中，红色柱子都是最高的)

PaLM-8B微调ReAct在所有PaLM-62B提示方法中表现最佳(即上图右侧的第一个区域中的红色柱子，高于上图左侧第二个区域中的所有柱子)

而PaLM-62B微调ReAct在所有540B提示方法中表现最佳   (即上图右侧的第二个区域中的红色柱子，高于上图左侧第三个区域中的所有柱子)

相反，对于PaLM8/62B，标准或CoT的微调明显劣于ReAct或Act的微调，因为

前者CoT本质上是教模型记忆(可能是幻觉的)知识事实

而后者ReAct教模型如何(推理和)行动以从维基百科中获取信息，这是一种更可推广的知识推理技能

由于所有提示方法仍然远远落后于特定领域的最新方法(表1)，即使用更多人工编写的数据进行微调可能是释放ReAct力量的更好方法

1.4 ReAct的两大应用：ALFWorld、WebShop

我们还在两个基于语言的交互式决策任务ALFWorld和WebShop上测试了ReAct，这两个任务都具有复杂的环境，需要智能体在长时间内进行行动和探索，以获得稀疏的奖励，因此需要推理来有效地行动和探索

1.4.1 ALFWorld

ALFWorld：是一个合成的基于文本的游戏，旨在与具体化的ALFRED基准相一致。它包括6种类型的任务，智能体需要通过文本操作(例如，去到咖啡桌1，取走纸2，使用桌灯1)导航和与模拟家庭进行交互，以实现高级目标(例如，在桌灯下检查纸)

一个任务实例可能有50多个位置，并且需要一个专家策略超过50步才能解决，因此挑战智能体规划和跟踪子目标，以及系统地探索(例如，逐个检查所有桌子是否有桌灯)

特别是，ALFWorld内置的一个挑战是需要确定常见家庭物品的可能位置(例如，桌灯可能在桌子、货架或梳妆台上)，因此这个环境非常适合LLMs利用其预训练的常识知识

为了促使ReAct，我们随机注释了每个任务类型的三条轨迹，其中每个轨迹包括稀疏的思考，其中

分解目标

跟踪子目标完成

确定下一个子目标

通过常识推理找到一个对象以及该怎么做(reason viacommonsense where to find an object and what to do with it )

按照Shridhar等人的方法，作者在特定于任务的设置中评估134个未见过的评估游戏。为了提高鲁棒性，通过注释的3个轨迹的每个排列构造了每个任务类型的6个提示

使用相同的轨迹构造Act提示，但不使用思考-由于任务实例是从训练集中随机选择的，因此既不偏向ReAct也不偏向Act，并提供公平和可控的比较，以测试稀疏思考的重要性

Act prompts are constructed usingthe same trajectories, but without thoughts —since task instances are randomly chosen from the training set, it favors neither ReAct nor Act and provides a fair and controlled comparison to test theimportance of sparse thoughts.

对于基线，我们使用BUTLER，这是一个通过对每种任务类型的105个专家轨迹进行训练的模仿学习代理

1.4.2 WebShop

WebShop：ReAct也可以与嘈杂的现实语言环境进行互动，以用于实际应用吗？

我们调查了WebShop（Yao等人，2022），这是一个最近提出的在线购物网站环境，拥有1.18M个真实世界的产品和12k个人类指令

与ALFWorld不同，Webshop包含各种结构化和非结构化文本(例如，从亚马逊爬取的产品标题、描述和选项)，并要求智能体根据用户指令(例如，“我正在寻找一个带抽屉的床头柜。它应该有一种镍涂层，价格低于140美元”)通过网络交互来购买产品(例如，搜索“床头柜抽屉”，选择按钮，如“颜色：现代镍白色”或“返回搜索”)

该任务的评估基于对500个测试指令的平均得分(即所选产品覆盖所有情节期望属性的平均百分比)，和成功率(即所选产品满足所有要求情节的百分比)

This task is evaluated by average score(percentage of desired attributes covered by the chosen product averaged across all episodes) and success rate (percentage of episodes where the chosen product satisfies all requirements) on 500 testinstructions.

用于搜索、选择产品、选择选项和购买的Act提示来制定Act提示，使用ReAct提示还需要推理确定要探索什么，何时购买以及哪些产品选项与指令相关

我们与使用1,012个人类注释的轨迹进行训练的模仿学习IL方法进行比较，以及使用10,587个训练指令进行训练的模仿+强化学习(IL + RL)方法

结果显示，ReAct在ALFWorld(上图表3)和Webshop(上图表4)上的表现均优于Act

在ALFWorld上，最佳的ReAct试验取得了71％的平均成功率，显著优于最佳的Act(45％)和BUTLER（37％）试验

事实上，即使是最差的ReAct试验(48％)，也能击败两种方法的最佳试验

此外，ReAct相对于Act的优势在六个受控试验中保持一致，相对性能提升范围从33％到90％，平均为62％

且可以看到，在完全没有任何想法的情况下，Act无法正确地将目标分解为更小的子目标，或者失去了对环境当前状态的跟踪(without any thoughts at all, Act fails to correctly decompose goalsinto smaller subgoals, or loses track of the current state of the environment)

在Webshop上，one-shot Act prompt已经与IL和IL + RL方法相当

通过额外的稀疏推理，ReAct实现了显着更好的性能，相对于先前最佳成功率，绝对提高了10％。通过检查示例，我们发现，ReAct更有可能通过推理来填补嘈杂的观察和行动之间的差距（例如，“对于‘用于客厅的节省空间的垫凳’，项目有‘39x18x18英寸’和‘蓝色’的选项，看起来很不错。”），从而识别与指令相关的产品和选项

但是，现有方法仍远未达到专家人类的表现水平(上图表4)，他们执行的产品探索和查询重构显然仍然对基于提示的方法构成挑战