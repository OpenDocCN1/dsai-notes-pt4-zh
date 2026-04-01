# ChatGPT第二弹：PPO算法

> 来源：[https://agijuejin.feishu.cn/docx/MirIdbdpcoLpoOxz7sOcyDSYntc](https://agijuejin.feishu.cn/docx/MirIdbdpcoLpoOxz7sOcyDSYntc)

> 哈工大SCIR｜[阅读原文](https://mp.weixin.qq.com/s/h9c3w1af2uBYLlVbw7j82A)

> 转载请联系原作者取得授权

> 原创作者：陈一帆

> 转载须标注出处：哈工大SCIR

# 1. ChatGPT与PPO算法

在上篇文章中我们提到，ChatGPT的训练过程主要分为三步：微调GPT-3模型、人工对微调后模型的生成结果打分以训练得到一个奖励模型、 基于微调后的GPT-3结合奖励模型采用强化学习的方法更新策略。而第三步中强化学习的方法为OpenAI于2017年提出的Proximal Policy Optimization（PPO）算法。PPO算法提出后应用场景相当广泛，包含图像识别 [1] 、机械臂控制 [2] 、电子游戏 [3] 以及今天的ChatGPT。OpenAI基于PPO算法设计的AI程序OpenAI Five甚至在2019年4月13日击败了Dota 2世界冠军战队OG [4] 。

在接下来的部分中，我们会在第二章中介绍强化学习的基础知识，第三章中介绍PPO算法论文并对其中的公式进行推导。

# **2. 强化学习基础**

以下内容参考OpenAI Spinning Up [5] ，其为OpenAI公开的一份关于深度强化学习的教育资源。

## **2.1 基本定义**

![](img/4b32f0044bfb69ef605d6d666f15a1b6.png)

![](img/2cfbdb9091644dc69f5280c3716b1bf7.png)

图 1强化学习示意图

![](img/a4bb29e1dbe6f0be4a1e9c3d305ae784.png)

![](img/3b0b880212e919d1673eebce3c8dddfb.png)

关于Q、V值的定义和转换关系可以参考图2

![](img/272adeb26ec8a4862b0587ab3cbd6db9.png)

图2 Q、V 值的定义和转换关系 [6]

显然，强化学习模型优化目标可以使用  表示，其中  表示智能体的开始状态。

## **2.2 Model-Free vs Model-Based, Value-Based **vs Policy-Based, off-policy vs on-policy

强化学习算法种类繁多，可按图3所示类别粗略分类。

![](img/09ebc83a1b7a1f453ac37dd20c3879fc.png)

图3 强化学习算法分类 [5]

可以看到PPO算法是无模型的强化学习算法，属于策略优化类算法，并且是on-policy的。本节中我们将对这些概念进行介绍，让大家对PPO算法有一个更好的认识。这些概念一共可分为三组，分别是：

+   基于模型的强化学习（Model-based RL）、无模型的强化学习（Model-free RL）；

+   基于策略的强化学习（Policy-based RL）、基于值的强化学习（Value-based RL，即图中Q-leranng）；

+   on-policy、off-policy。

![](img/62e2c56129a7feb202d156abdf6296cb.png)

我们以围棋为例举例说明，自然而然地，我们会以棋盘上棋子的分布作为状态，以一次落子作为动作，一般以当前棋局的胜率作为奖励。不妨假设强化学习程序是黑方棋手，如果以黑方棋手落子前棋子分布作为状态，那么每次状态的改变便是两颗棋子，由于无法预测白方棋手的落子，我们无法对状态转移进行建模，此场景便是无模型的强化学习。但是，我们使用搜索引擎会发现，AlphaGO、AlphaGo Zero、AlphaZero 均是基于模型的强化学习模型，其差别便在于，他们会在训练的过程中同时扮演黑方棋手和白方棋手，这也便是“自己与自己下棋”的由来，那么状态的定义便是任意一方落子前棋子的分布，如此状态转移情况显而易见：给定当前棋子的分布（当前状态）以及本次落子的位置（动作），求落子后棋子的分布（下一步状态）。而奖励值（落子后棋局的胜率）获取的一般思路是棋局模拟，即不执行实际的落子动作，而是使用其他模型模拟棋局的进行得到获胜概率。具体方法是蒙特卡罗树搜索：例如将棋局继续进行 1600 次，得到执行各候选动作的获胜次数，除以 1600 作为获胜概率，如此也可以找到当前最优动作，模拟棋局的进行不依赖实际落子，AlphaGo Zero 依赖一个更加小型的模型快速走子，AlphaZero 直接依赖待学习的策略帮助自己走子。这类似于人类棋手的心算，在强化学习场景下我们说这是对环境进行建模，如此我们便可以称其为基于环境的强化学习。

![](img/221fb99f8f6097770390342e0bba4755.png)

![](img/24412e7c7b2a995861da3fe1c29a76f5.png)

需要注意的是，上文中提到的策略迭代、值迭代算法与此处基于值的强化学习方法、基于策略的强化学习方法并无对应关系，他们均是基于值函数来完成策略的更新，可以划入基于值的强化学习方法中。

我们最后简要介绍两种基于值的强化学习算法Q-Learning和SARSA（State-Action-Reward-Sate-Action） 并以此为例说明on-policy和off-policy的强化学习。

![](img/5c7e00659119c9bb1ee7ff46b9b072a5.png)

![](img/2d427cd97c2dbe11613ba8e030b5801d.png)

图4 Q-learning 算法：off-policy [7]

![](img/8b44a8e901dd5b793c0207fe8a8dfde4.png)

图5 SARSA算法：on-policy [7]整体而言，on-policy是行为策略与目标策略一致，而off-policy是行为策略与目标策略不一致。除此之外还需要注意的是offline离线学习算法，该类型算法使用静态的已经获取好的数据用于学习和部署，与前面 提到的实时与环境交互获得反馈的在线强化学习模型有所不同。

![](img/1e71667ddbe634a8a7e805abf248f422.png)

图6 on-policy vs off-policy vs offline RL [8]

行为策略与目标策略的不一致有多种情况，Q-learning 中选择 ϵ-greedy 作为行为策略目的是在数据获取过程中有一定的探索性，而在图6（b）中行为策略与目标策略的不一致在于行为策略会累积一定数据后更新目标策略，或者说目标策略的更新不会马上同步到行为策略中，此时行为策略是旧的目标策略。图（a）则是 on-policy 算法，图（c）是 offline 算法，可以看到仅有简单的数据收集-> 策略学习-> 模型部署过程。

## **2.3 强化学习基础小结**

在上文中，我们首先介绍了强化学习的定义与优化目标，并介绍了重要的两个值函数：状态值函数，状态动作值函数，无论是基于值或是基于策略的强化学习方法中他们均有着重要的作用。在第二节中我们首先简要介绍了基于模型的强化学习，分为预测和控制两种形式，学习控制其本质是为了寻找最优策略，而学习预测是为了对策略找到一种评价标准。在无模型的强化学习方法中，我们首先介绍了基于值的强化学习法方法，并以Q-learning和SARSA为例介绍了on-policy和off-policy。基于策略的强化学习方法的介绍，我们将通过阅读PPO论文进行。

![](img/45de54b4dc246f6c6f4ccfa04e6673d1.png)

图7 强化学习示意图

# **3. PPO论文讲解**

![](img/559b2fb45f73b3ad4b3f3fe465f40ce4.png)

# **3.1 策略梯度定理**

策略梯度定理是策略梯度算法中最简单的形式，论文中给出了策略梯度算法中梯度估计的一般形式：

![](img/a62d9a3210083bdd434c25c98c8a2faf.png)

该方法下，模型容易在一次轨迹中进行多步优化，这并不准确，因为一次优化后其轨迹信息理应再次采样，后续的实验中也会证明这一方法容易导致较大的策略迭代，并造成严重影响。

下面我们从基本定义开始推导。

**最简单的策略梯度定理**

![](img/f6497376be9613fa15209084f9b3a951.png)

得到以上中间结论后，可以开始以下推导:

![](img/839a8a004372dcdca688cf0afbbb862e.png)

**悟已往之不谏，知来者之可追：仅考虑未**来的奖励（ Reward-to-Go ）

![](img/691aa33d6f04d4a8393bc0f730ec1ab7.png)

在代码实现上与原来不同的仅仅只有轨迹生成后loss值的计算，loss值的计算需要(s,a)对的权重，原公式中权重均为轨迹的总收益，此处为(s,a)对之后总的奖励。

**EGLP 引理（ Expected G**rad-Log-Prob Lemma ）

![](img/c83bdf81e31a9acc700775671a787eb5.png)

![](img/9762e5dcadffe735b53424894f768117.png)

## **3.2 信赖域策略优化算法（Trust Region P**olicy Opitimization）

由于上述方法中模型迭代步长过大，并且其未在策略变化后重新进行采样，因此作者将上述目标函数修正如下：

![](img/1762d94e4c8fc1f568e7a349a83fc4b8.png)

![](img/e56b2a555d2221604d6866d2745bce6d.png)

![](img/068d60569dbe7cc402b065ff92afe3e7.png)

旧策略的状态分布代替新策略的状态分布

![](img/98296412c30cfeb7e6e9e60d9b944ef1.png)

该结论可直观的由图8所示。

![](img/1eb41d298f28b247ad69cec2a9e3c488.png)

图8 TRPO 一阶近似图

![](img/7a58bec1d937191fdc942fb8245aceaa.png)

![](img/0cfe10c9f01ea29135bae5ca9122a9cd.png)

![](img/ff20d5bae5747a048388c019127ba1b9.png)

**平均KL散度代替最大KL散度**

![](img/a0f64024f5c9fc9dbc8dea993b36de3b.png)

**重要性采样**

![](img/07674ba3a3e50268d4c36b5b0939e5f8.png)

![](img/0cada6511013f64759824bb485f0aa5b.png)

## **3.3 CLIP**

![](img/f33fb450b01080d12be34c947c02ed14.png)

上式为CPI（Conservative Policy Iteration）算法的目标函数，正如其名，CPI算法使用了一种非常保守且复杂的方式对目标函数进行优化 [12] 。相比之下TPRO算法并未改变目标函数的形式，而是引入约束项，使用拉格朗日对偶法处理之后采用共轭梯度法进行优化，而PPO算法的改进之处同样是解决这一目标函数难以优化的问题：如何控制更新步伐，避免策略更新步伐过大导致效果下降、更新步伐太小导致训练速度缓慢的问题，即使TRPO算法，也仍然遗留了实现困难，求解过程计算量大的缺点。

我们首先来看PPO算法对此问题的第一种优化方式，将上述目标函数修改为如下形式：

![](img/c1eefd772932725f64c7483092de60d3.png)

![](img/ddf332a173ea76f75def5325f0833efd.png)

图9 CLIP 效果图

下图也说明了CLIP相比于CPI区间范围更小。

![](img/a7a8ffb51460541373aa15b6de51c12b.png)

图10 CLIP、CPI 效果对比

## 3.4 Adaptive KL Penalty Coefficient

![](img/c61796fc42bb9d15db4b62e25bbc3d36.png)

## **3.5 算法**

![](img/76e2d8c2ae91f271ff104942e7858163.png)

论文随后给出PPO算法伪代码如下：

![](img/99b1dcb336c12e49b343da60d1918ae3.png)

图11 PPO伪代码

![](img/40aabe452911a60be7b45476d546048b.png)

## **3.6 实验**

### **3.6.1 不同目标函数的对比**

![](img/7904672f63e9efca079a6df0f28c5da5.png)

![](img/495e088b4f4129b72891ef1035a75e92.png)

图12 不同目标函数的效果

### 3.6.2 连续空间下与其他算法的对比

![](img/e4675cea92d184798ecf8b457ddcf42d.png)

![](img/1c7bdc2e2e81d56008e8116c0d3c37a7.png)

图13 连续空间下算法效果对比

### **3.6.3 连续空间下示例**

![](img/63506ee1f029c9abe0cadd23b8050b53.png)

![](img/d80ad7319fe2ddd7ee6a322a790aeb96.png)

图14 3D控制学习曲线

![](img/878a14afdf7f4aad6095aa3cee9b8042.png)

图15 3D控制示意图

上图中为12帧静态图像，在第7帧时目标的位置发生了变化，机器人也随之朝向新的目标。

### 3.6.4 雅达利(游戏)中与其他算法的对比

作者在雅达利发行的49款游戏上测试了PPO以及其他算法的效果，如下表所示：

![](img/1d83d9312378a7e088fd8cbaec44d5c1.png)

图16 雅达利游戏中效果

# **3.7 论文总结**

![](img/e2b5e2b7a5607c8c55bc3bdbaba3d11a.png)

# **4 总结与思考**

PPO 算法自 2017 年提出后，以其实现相对简单、效果优良的特点，在 OpenAI 强化学习相关工作中出现频率极高，在游戏、机器人控制等实际应用领域成功证明其性能，截至目前仍然是基于策略的强化学习算法中最前沿的算法，即使 OpenAI 已经解散其机器人研究组，PPO 算法却又意外的在 NLP 领域大放异彩，其与 GPT-3 的结合成功产生了让人惊艳无比的 ChatGPT，笔者不禁思考，这种 RLHF（Reinforcement Learning Human Feedback）范式下究竟是什么让 ChatGPT 如此强大，是作为 Human Feedback 的高质量语料，还是 PPO 算法中蕴含的策略，还是 GPT-3 中未充分挖掘的大模型潜力？

目前高质量数据逐渐成为商业公司的专属，国内大模型的研发也落后于美国，模型提出后缺乏后续维护，没有长期坚持下去形成自己的技术路线，不注重研究的 diversity。 而国外部分顶尖研究组已经与 OpenAI 等大公司形成紧密的联系，在相关研究上有着很大的优势，ChatGPT 的出现已经对很多 NLP 研究领域造成了巨大的冲击，面对美国在大模型上的封锁，我国缺乏由商业公司主导的大模型，国内研究组只能另辟蹊径，但大模型的出现到产生 ChatGPT 一般的成果需要漫长的技术积累，若不立刻行动起来，我国与美国在大模型上的差距恐怕会越拉越大。

# **参考文献**

[1] Barret Zoph, Vijay Vasudevan, Jonathon Shlens, and Quoc V. Le. Learning transferable architectures for scalable image recognition. CoRR, abs/1707.07012, 2017.

[2] OpenAI. Learning dexterity: a human-like robot hand to manipulate physical objects with unprecedented dexterity. Web Page, 2018. Last Accessed December 23, 2022.

[3] Christopher Berner, Greg Brockman, Brooke Chan, Vicki Cheung, Przemysław Dębiak, Christy Dennison, David Farhi, Quirin Fischer, Shariq Hashme, Chris Hesse, et al. Dota2 with large scale deep reinforcement learning. arXiv preprint arXiv:1912.06680, 2019.

[4] OpenAI. Openai five defeats dota 2 world champions. Web Page, 2018. Last Accessed December 23, 2022.

[5] OpenAI. Openai spinning up. Web Page, 2018. Last Accessed December 23, 2022.

[6] CSDN. 【强化学习笔记】强化学习中的 v 值和 q 值 . Web Page, 2022. Last Accessed December 23, 2022.

[7] David Silver. Lectures on reinforcement learning.URL: https://www.davidsilver. uk/teaching/ , 2015.

[8] Sergey Levine, Aviral Kumar, George Tucker, and Justin Fu. Offline reinforcement learn ing: Tutorial, review, and perspectives on open problems. CoRR,abs/2005.01643, 2020.

[9] OpenAI. Openai implement code. Web Page, 2018. Last Accessed December 23, 2022.

[10] John Schulman, Sergey Levine, Pieter Abbeel, Michael Jordan, and Philipp Moritz. Trust region policy optimization. In Francis Bach and David Blei, editors, Proceedings of the 32nd International Conference on Machine Learning, volume 37 of Proceedings of Machine Learning Research, pages 1889–1897, Lille, France, 07–09 Jul 2015. PMLR.

[11] Lucian Buşoniu, Damien Ernst, Bart De Schutter, and Robert Babuška. Approximate reinforcement learning: An overview. In 2011 IEEE symposium on adaptive dynamic programming and reinforcement learning (ADPRL), pages 1–8. IEEE, 2011.

[12] S. Kakade and J. Langford, Approximately Optimal Approximate Reinforcement Learning. Proceedings of the Nineteenth International Conference on Machine Learning: , 2002.

# **作者简介**

陈一帆, 就读于哈尔滨工业大学社会计算与信息检索研究中心，对话技术（DT）组，大四本科生，师从张伟男教授。研究方向为对话式推荐系统。

![](img/e8bc298afac2568668631bf580e40468.png)

本期责任编辑：赵森栋

本期编辑：赵 阳