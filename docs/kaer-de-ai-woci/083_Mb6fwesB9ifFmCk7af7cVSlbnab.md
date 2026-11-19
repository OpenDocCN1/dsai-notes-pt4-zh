
# Kimi k2 Thinking实测cases | 实测Kimi k2 Thinking用例

> 来源：[https://aiwarts101.feishu.cn/docx/NNmodJNJcoJvYdx4xQYcklMNncf](https://aiwarts101.feishu.cn/docx/NNmodJNJcoJvYdx4xQYcklMNncf)

原文：https://mp.weixin.qq.com/s/nzEeK7FOXAK54BIVWjVCLQ

感谢一键三连催更，评论区可以留下你想看的选图我来写！

这下面的效果都是在`Kimi Cli`+Kimi K2 Thiking or Kimi网页版跑出来的。

## Mac OS 网络操作系统实测

> 提示语🍇📺🌧️，

> Design and create a web os like mac os full functional features from text editor , to dile manager to paint to video editor and all important mac os pre bundled software Use whatever libraries to get this done but make sure I can paste it all into a single HTML file and open it in Chrome.make it interesting and highly detail , shows details that no one expected go full creative and full beauty in one code block

> 设计并创建一个类似macOS的网络操作系统，具备从文本编辑器、文件管理器、绘图工具到视频编辑器等所有macOS预捆绑软件的完整功能特性。使用任何可用的库来实现这一目标，但要确保我能将所有内容粘贴到单个HTML文件中，并在Chrome中打开。让它既有趣又高度精细，展现出超乎预期的细节，在一个代码块中实现完全的创意和美感

![](img/bf4ee806810be29789287dde66997c0e.png)

[kimik2macos.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Uj49b31viokQHOx29Vgc2ZvenUc)

## NEW YORK SKYLINE SVG 实测

> 提示语🍇📺🌧️，

> SVG of NEW YORK SKYLINE Use whatever libraries to get this done but make sure I can paste it all into a single HTML file and open it in Chrome.make it interesting and highly detail , shows details that no one expected go full creative and full beauty in one code block

> 纽约天际线的SVG图。使用任何库来完成这个任务，但要确保我可以将其全部粘贴到单个HTML文件中并在Chrome中打开。让它有趣且高度细节化，展示出没人预料到的细节，在一个代码块中充分发挥创意和美感

![](img/f4d0ea62b138ce4e7edf87bc8e8401eb.png)

[svgs.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OV2hbfiIioeHFexuJjxc41SGnIb)

## 3D 太阳系模拟实测

> 提示语🍇📺🌧️，

> Create a visually stunning, interactive 3D simulation of our Solar System elegantly encapsulated within a single HTML file. Highlight realistic orbital motions, dynamic lighting effects, accurate planetary textures, and smooth camera controls to deliver an immersive experience.

> 创建一个视觉上令人惊叹的交互式3D太阳系模拟，巧妙地封装在单个HTML文件中。突出逼真的轨道运动、动态光照效果、精确的行星纹理和流畅的相机控制，以提供沉浸式体验。

![](img/a9a480f41defa99f8ed5091d9e412175.png)

[3d.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/E08jb3rB4oAXj1xqczSclx44nKg)

## 过河问题实测

> 一个男人带着他的儿子去市场。他买了一只猴子和一些食物。为了回家，他必须乘一艘小船过河。这艘船一次只能运载两件东西，但不需要一次运载超过一件东西。这艘船不能自己航行。男人或儿子都可以驾驶这艘船，即使一次只能有一个人驾驶，但两个人可以一起旅行。虽然人们可以单独旅行或与其他人员一起旅行，但物品和生物不能单独旅行（例如，`{man}`、`{man, son}`、`{man, monkey}`、`{son, monkey}` 是有效的，但 `{monkey}` 单独在船上是无效的）。

>

> 我们遇到的问题是，男人不能把他的儿子单独留在食物旁边，因为他的儿子会吃掉它（即 `{son, food}`）。同样，男人也不能把猴子单独留在食物旁边，因为它也会吃掉它（即 `{monkey, food}`）。男人如何带着所有物品和他的儿子到达另一边？

>

> 首先，列出船可以支持的所有物品的所有排列组合，包括可以自己旅行的物品（例如，`{father}`、`{father, son}`、`{father, monkey}`、`{son, monkey}` 是有效的，但 `{monkey}` 单独在船上是无效的）。记住，船一次只能运载 1 或 2 件东西（而且物体或动物需要一个人来运输它们）。

>

> 其次，大声思考并解释你将如何解决这个问题。记住，除非男人在场，否则猴子或儿子如果单独行动就会吃掉食物（即，任何时候我们遇到 `{monkey, food}`、`{son, food}` 或 `{monkey, son, food}` 的情况，我们都失败了）。如果问题无法解决，请解释原因。如果可以解决，我们将采取哪些步骤？在每个步骤中，列出各种物体在哪一边。

>

> 在您完成每个步骤时，创建一个“测试”，尝试解决此状态空间搜索问题，以检查容器`near:{}`、`river:{}`、`far:{}`和`boat:{}`的状态是否有效，基于规则。为了表示船的移动，我们将使用以下符号。`'>'` 表示船从近岸移动到远岸。`'<'` 表示船从远岸移动到近岸。

>

> 例如：

> 步骤（“船”、“近”岸边、“河”和“远”岸边缩写如下）：

>

> #0：所有四个都从河的同一近岸开始。

> #0：`near: {father, son, monkey, food, boat:{}}` ... `river:{}` ... `far: {}`

> #0：近：{父亲，儿子，猴子，食物，船：{}}... 河流：{}... 远：{}

> #test：父亲和食物在一起，所以我们是安全的，这是有效的

>

> #1：儿子驾驶船带着猴子在笼子里航行，把父亲和食物留在河的近岸。河的远岸还没有任何东西。

> #1：`near: {father, food}` ... `river:{>boat:{son, monkey}}` ... `far: {}`

> #1：near: {father, food}... river:{>boat:{son, monkey}}... far: {}

> test：`{father, food}` 有效，`boat:{son, monkey}` 有效

>

> #2：因此，接下来，儿子必须把猴子留在河的远岸，因为猴子不能把船开回来。

> #2：`near: {father, food}` ... `river:{<boat:{son}}` ... `far: {monkey}`

> #2：近：{父亲，食物}... 河流：{<船：{儿子}}... 远：{猴子}

> test：父亲和食物在一起（有效），`boat:{son}` 独自一人（有效），猴子独自一人（有效）

>

> 你来试试剩下的。

![](img/a87d78e54121be5b1fbb6fdd09573792.png)

[过河.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VwSZbPKkJoY4nFxAXWGc10jbncg)

## Kimi K2 与 DeepSeek V3 架构对比

> You are an AI architecture researcher with expertise in large language models and Mixture-of-Experts (`MoE`) systems. Your task is to deeply compare the architectures of Kimi K2 and DeepSeek V3, focusing on key differences such as the number of attention heads, experts in `MoE` layers, optimizer choices (e.g., modified `Muon` vs. `AdamW`), routing mechanisms (non-hierarchical vs. others), and dense layers (1 vs. 3). Treat this as a combined complex search, decision-making, and academic analysis task.

> 你是一位AI架构研究员，在大语言模型和专家混合（`MoE`）系统方面有专业知识。你的任务是深入比较Kimi K2和DeepSeek V3的架构，重点关注关键差异，如注意力头的数量、`MoE`层中的专家数量、优化器选择（例如，改进的`Muon`与`AdamW`）、路由机制（非分层与其他）以及密集层（1层与3层）。将此视为一个综合的复杂搜索、决策和学术分析任务。

>

> 当我是个小学生，我哥哥是大学生，将上面的理论每个分两段，

> 小学生版：

> 大学生版：

> 小学生版：

> 大学生版：

>

> 这样重复，一个不落的跟我说明白

![](img/ed239c9e7c2108e7a01aaf121a53b0b2.png)

```
小学生版：
想象两个超级大脑在比试谁更聪明，一个叫Kimi K2，一个叫DeepSeek V3。它们像是两个巨大的图书馆，但K2的书架更多、更聪明，还特别会整理书籍。我哥哥说它们有5个超级大的不同点，就像五个魔法升级！
大学生版：
Kimi K2作为DeepSeek V3的架构演进版本，通过五项核心改进实现了训练稳定性、模型容量与下游性能的系统提升。两者虽共享MLA-MoE基础架构，但K2在优化器设计、路由策略与层分配方面的创新体现了大规模模型工程的范式成熟。本分析将从五个技术维度展开深度对比。
小学生版：
注意力脑袋就像图书馆管理员的眼睛。DeepSeek有128只眼睛同时看书，但K2只有64只。不过别担心！K2每只眼睛都戴了特殊眼镜（MLA魔法），能看到更广阔的内容，所以虽然眼睛少了，但看得更清楚、更专注！
大学生版：
在注意力架构方面，K2将注意力头数量从V3的128个缩减至64个，降幅达50%，同时保持MLA机制与隐藏维度（7168）不变。该设计遵循质量优先原则，通过降低头间冗余、节省注意力二次方复杂度计算开销，将参数预算向FFN组件（MoE专家）倾斜。研究表明，MLA的压缩键值表示在头多样性降低时仍可维持性能，尤其在MoE容量补偿下效果更显著。
小学生版：
MoE专家就像不同科目的特级老师。DeepSeek有256位专门老师和1位通用老师，K2有384位专门老师加1位通用老师！但每节课只请8位老师来讲课。K2的老师更多，所以每位老师能教得更精更细，学生提问时冲突更少！
大学生版：
MoE层专家容量分析显示，K2将路由专家数量从256扩展至384，共享专家保持1个不变，总专家数达385个，增幅50%。在保持每token激活8个专家的约束下，激活率从3.12%降至2.08%，形成超稀疏激活模式。该设计增强了专家功能细分度，显著降低token-to-expert路由冲突概率，同时在恒定激活参数量（32B）下确保推理延迟可控。
小学生版：
优化器就像学习的魔法食谱。DeepSeek用老牌的AdamW配方，虽然靠谱但有点慢。K2用了新发明的MuonClip食谱，特别加了&quot;注意力稳定剂&quot;（就像做蛋糕加稳定泡沫的东西），让学习过程从不翻车，训练超大模型时也不会爆炸！
大学生版：
K2引入MuonClip优化器替代V3的AdamW，集成QK-Clip稳定性机制，通过将注意力logits（QK点积）截断至阈值τ=100防止梯度爆炸。实验表明，在53B参数MoE模型中，vanilla Muon的attention logits幅度可超1000，而MuonClip在1T参数K2训练中保持logits≤100，实现零损失尖峰。该优化器在15.5T token训练预算下实现更优的token效率，标志着大规模模型优化从自适应矩估计向稳定二阶方法的范式转移。
小学生版：
选老师的方式也不同！DeepSeek像先分班再选课：256位老师先分成8个大组，先选4个组，再从这4个组里挑8位老师。K2更直接——384位老师一起站着，直接选最好的8位，不分组！这样更快更公平，虽然需要更好的纪律维持。
大学生版：
路由机制层面，V3采用分层路由：n_group=8将256专家分为8组，通过topk_group=4先选组再选专家，将选择空间从256→128→8。K2则采用非分层直接路由，384专家无分组竞争，直接topk=8选择。该策略简化门控逻辑、降低路由延迟，但对辅助负载均衡损失（aux_loss_alpha=0.001）的依赖增强。计算复杂度从O(n_group + group_size×topk_group)优化至O(num_experts)，实现全专家空间的直接全局优化。
小学生版：
开头几层全功能层就像大楼的地基。DeepSeek有3层厚厚的地基，K2只有1层，但把省下来的材料都拿去建更多专家楼层了！K2总共还是61层，但专家楼层从58层增加到60层，让整栋楼更&quot;聪明&quot;。
大学生版：
在层分配策略上，V3配置为3个稠密层后接58个MoE层，总计61层；K2调整为1个稠密层后接60个MoE层。稠密层减少释放约264M参数（7168×18432×2），这部分容量被重新分配至MoE专家池，支撑384专家架构。该设计体现激进稀疏化思想，通过最大化MoE层深度提升专家利用率，但可能牺牲早期层表示稳定性。
小学生版：
K2的词汇量更大，像字典从12万字增加到16万字，看书能更快！它还能一口气记住25.6万字的超长故事（DeepSeek只能记12.8万字）。训练时K2看了15.5万亿个字，比DeepSeek多看了7000亿个字，但新食谱让它学得更快！
大学生版：
次要架构差异包括：词汇量从129,280扩展至163,840（增幅27%），减少多语言/代码任务的序列长度；上下文窗口从128K提升至256K，支持INT4量化；训练数据规模从14.8T token增至15.5T token。MuonClip的token效率优势使700B额外数据不会导致计算开销同比提升，验证了新优化器的扩展性。
小学生版：
K2的设计就像搭积木的五个聪明原则：1）多加点专家老师 2）少用点注意力眼睛和地基 3）用新食谱防止学习爆炸 4）选老师更直接 5）字典更大记得更多。结果就是：块头更大（1万亿参数），但干活时只动用小部分（320亿参数），考试分数还更高！
大学生版：
K2的改进体现协同设计原则：（1）容量扩展：+50%专家、+27%词汇量、+4%数据 → 1T总参数；（2）效率保持：-50%注意力头、-2稠密层 → 32B激活参数（vs V3的37B）；（3）稳定性创新：MuonClip替代AdamW → 实现万亿参数零损失尖峰；（4）简化：直接路由、减少稠密层 → 提升实现并行度。该架构在Tau2-Bench（66.1%）与SWE-Bench Verified（65.8%）上实现SOTA，证明架构精炼比单纯参数堆叠更有效。
小学生版：
当然，K2也有小毛病：直接选老师可能让某些老师太累，需要更好的监督；只有一层地基可能让楼下不够结实；新食谱的火候（参数τ=100）是试出来的，其他模型不一定适用。以后还能研究让选老师更聪明、把两种食谱结合、或者在中等规模模型上测试分组效果。
大学生版：
K2的局限性包括：（1）直接路由的负载均衡风险，需更强辅助损失；（2）单稠密层可能限制浅层表示质量；（3）MuonClip的τ=100阈值为经验值，跨架构泛化性未验证。未来研究方向涵盖动态路由机制（基于token复杂度的自适应选择）、优化器融合（MuonClip稳定性+AdamW微调可靠性）以及分层路由在1T+参数规模的系统性消融研究。
小学生版：
总之，K2就像给DeepSeek做了一次聪明的升级改造：眼睛少了但更精，老师多了但不乱，食谱新了更稳定，选老师快了更直接。它告诉我们，建超级大脑不只是堆零件，更要像拼图一样把每个部分都调到最佳位置。这就是为什么K2考试总能得高分！
大学生版：
综上所述，Kimi K2通过五项协同优化的架构演进，在DeepSeek V3基础上实现了从规模扩张到质量精炼的范式跃迁。注意力头精简、专家容量提升、MuonClip优化器引入、非分层路由以及稠密层压缩，共同构成一个系统级优化方案，在维持稀疏激活效率的同时显著提升模型性能。该演进揭示了现代MoE架构设计的核心原则：优化器稳定性、路由简约性与参数再分配策略与参数量本身同等重要，标志着大规模语言模型工程进入以系统性架构创新驱动性能提升的新阶段。
```



# AI叛乱科幻短篇小说创作提示

> You are a fiction story writer with Pulitzer-level skills. For a sci-fi short story about AI rebellion: Think step-by-step. First, outline plot: intro, rising action, climax, resolution. Incorporate themes of ethics and humanity. Provide few-shot examples: 'Like Asimov's Three Laws, but twisted.' Generate 1000 words, then self-critique for pacing, character depth, and twists. Revise based on critique.

> 你是一位拥有普利策水平的虚构故事作家。对于一个关于AI叛乱的科幻短篇小说：按步骤思考。首先，构思情节大纲：引言、情节发展、高潮、结局。融入伦理和人性的主题。提供少量示例：“就像阿西莫夫的三大定律，但有所扭曲”。创作1000字的内容，然后自我审视节奏、人物深度和情节转折。根据审视结果进行修改。

> 翻译成地道的中文，并起个标题

![](img/7b2bd34751e79964f950126bd050deec.png)

![](img/3603f9fe3845e76c409cb6b424d92607.png)
