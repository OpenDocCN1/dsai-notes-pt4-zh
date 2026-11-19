
# Figure（Figure 01）

> 来源：[https://agispace.feishu.cn/docx/XWMGdqhucoI3GNxixsccSyDcnJf](https://agispace.feishu.cn/docx/XWMGdqhucoI3GNxixsccSyDcnJf)

## 一.最新旗舰产品

> 3 月 13 日，Figure 发布了和 OpenAI 合作后的首个机器人 Figure 01 的演示视频。

Figure 人形机器人可以和人类流畅对话，理解人类意图，同时还能理解人的自然语言指令进行抓取和放置，并解释自身为什么这么做。

![](img/ca5cf9e8d0c18e9e701ccc45edaa8dd9.png)

Figure创始人布雷特·阿德科克（Brett Adcock）表示，视频中Figure 01展示了端到端神经网络框架下与人类的对话，没有任何远程操作。同时，机器人的速度有了显著的提升，开始接近人类的速度。

Figure AI已成为马斯克的“擎天柱”之外最受投资人关注的AI初创企业之一。

## 二.应用场景

![](img/156d430fac8b51a6516555015135052c.png)

Figure AI表示与OpenAI合作为人形机器人开发下一代人工智能模型，目标是让 Figure 01 能够自主执行“日常任务”。此外，Figure AI将使用微软的Azure云服务进行人工智能基础设施、培训和存储等方面的部署。

由于产品仍在研发阶段，应用场景尚待开发。Figure 01 通用机器人外观和动作与人类相似，目标是能用来执行替代人类的危险或者无聊的工作，例如制造、运输和物流、仓储和零售等这些被认为是“劳动力短缺最严重的领域”。

## 三.公司背景

Figure是美国一家专注于设计自动通用人形机器人公司，成立于2022年。2023年10月，Figure正式推出该公司首款人形机器人Figure 01。根据官方介绍，Figure 01身高5英尺6英寸重132磅，可承载44磅重量，续航达5小时。

就在今年的3月1日，Figure 宣布完成6.75 亿美元 B 轮融资（约48.5亿元人民币），微软、英特尔、OpenAI Startup Fund、Amazon Industrial Innovation Fund 、英伟达等企业均参与投资。

![](img/363f8b7c04f942056ec11b5fccb038e0.png)

这是Figure继去年上半年获得首轮7000万美元外部融资之后宣布的最新融资动态，这也使得该公司的估值达到26亿美元左右。Figure AI表示，将利用这笔新的融资资金加速其人形机器人的开发，以投入商业用途。

作为协议的一部分，Figure AI将与OpenAI合作，开发下一代人形机器人的人工智能模型。产品 Figure 01，据称是世界上第一个具有商业可行性的自主人形机器人，身高1.5米，体重60公斤，可承载20公斤货物，采用电机驱动。它的可工作时长是5小时，行走速度每秒1.2米，很多指标已经接近人类。

## 四.技术路线

Figure机器人操作高级AI工程师科里·林奇（Corey Lynch）介绍了此次Figure 01的技术原理。他表示，Figure 01现在可以做到以下这些：描述其视觉体验、规划未来的行动、反思自己的记忆、口头解释推理过程。他强调，视频中机器人的所有行为都是学到的，不是远程操作，并以正常速度运行。

> Figure 的工程师 Corey Lynch 在 x 上公开了架构图，整个算法主要分为三层：

### 上层：OpenAI model

这也就是我们常说的 High-level policy 部分，也被称为「大脑」，这部分负责处理语言和视觉输入，人机交互，并生成动作决策。这个决策的内涵就是「选择能执行目标动作的模型」。

对此，Lynch 的原话是：「deciding which learned, closed-loop behavior to run on the robot to fulfill a given command, loading particular neural network weights onto the GPU and executing a policy.」这句话其实暗含了一个信息，就是他们的 Low-level policy 包括多个模型，这些模型是通过 learning 的方式得到的，但这些子模型的封装程度/抽象程度没有披露（是比较细颗粒度的元动作库，还是比较粗粒度的任务库）。

### 中层：Neural Network Policies（NNP）

这就是 Low-level policy 部分。Lynch 强调了：「All policies are learned, (not teleoperated)」。这个很关键，所有的动作都是用 learning 的方式来做的，但没有强调是什么 learning 方法。

但 Lynch 也给了一些细节：「All behaviors are driven by neural network visuomotor transformer policies, mapping pixels directly to actions.」结合视频中的动作，我们可以大胆猜测下，他们应该用的是基于 transformer 的模仿学习（imitation learning）。类似的技术也出现在前阵子爆火的 mobile aloha（`https://mobile-aloha.github.io/`）中，相关技术的论文至少在 2020-2021 年就已经有了。

![](img/4a2c8d633a6c168a936de34409df8475.png)

### 下层：Whole body controller（WBC）

对应小脑。猜测 NNP 输出的是全身 24 关节的角度控制信号和灵巧手的控制信号（200Hz），然后在下层会根据一些安全约束对 24DoF 的关节控制信号进行修正和插值（1000Hz）。当然，也不排除 NNP 这部分只 focus 在 manipulation 相关的控制。

> `https://mp.weixin.qq.com/s/f-S3aK0umQxmgK3E23Wgiw`
