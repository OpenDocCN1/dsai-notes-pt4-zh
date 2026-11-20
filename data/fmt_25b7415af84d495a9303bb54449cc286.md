
# 曝Bengio新动向：世界模型+数学证明，确保AI系统正常运作

> 来源：`https://agijuejin.feishu.cn/docx/Jp6QdYIeyoFSXEx7k3EcMGZ3nJg`

> 西风 发自 凹非寺

> 量子位 | 👸号 QbitAI

深度学习三巨头之一Yoshua Bengio的下一步动向公开了，关于AI安全——

加入了一个名为Safeguarded AI（受保护的人工智能）的项目，担任科学总监。

![](img/bd757e4fc75e323ab242868e31d6f285.png)

据介绍，Safeguarded AI旨在：

通过结合科学的世界模型和数学证明，构建一个负责理解和降低其它AI Agent风险的AI系统。

主打的就是一个量化安全保障。

该项目由英国高级研究与发明局（ARIA）提供支持，据说未来ARIA将投入共5900万英镑（约合RMB5.37亿）。

Bengio表示：

> 如果你计划部署某种技术，鉴于AI行为异常或误用可能带来非常严重的后果，你需要提出充分的理由，最好能提供强有力的数学保证，确保你的AI系统将正常运作。

![](img/142fe62b8a952d7ee60ed4c54ce1c6a3.png)

## “受保护的AI”

Safeguarded AI项目被划分为三个技术领域，每个领域都有特定的目标和预算：

+ 支架（Scaffolding），构建一个可扩展、可互操作的语言和平台，用于维护现实世界模型/规范并检查证明文件。

+ 机器学习（Machine Learning），使用前沿AI帮助领域专家构建一流的复杂现实世界动力学的数学模型，并利用前沿AI训练自主系统。

+ 应用（Applications），在关键的网络-物理操作环境中部署一个由“把关AI”保护的自主AI系统，通过量化的安全保障释放重要的经济价值。

![](img/4f837ac6b402aa88bdb2df959ba65371.png)

官方表示，Bengio加入后将特别关注`TA3`和`TA2`，在整个计划中提供科学战略建议。

ARIA还计划投入1800万英镑（约合RMB1.64亿）成立一个非营利组织，领导`TA2`的研发工作。

Safeguarded AI项目总监是前Twitter高级软件工程师David “davidad” Dalrymple，去年9月份加入ARIA。

对于Bengio的到来，Dalrymple还在X（原推特）上传了俩人的合照：

![](img/45b6e1ba0a90df97fb02a31481502908.png)

关于“构建一个负责理解和降低其它AI Agent风险的AI系统”的具体方法，David “davidad” Dalrymple、Yoshua Bengio等人写了份文件。

![](img/3dae9884b6a82ac853a4a561edd2a20d.png)

其中提出了一套称为“Guaranteed Safe AI（保证安全的AI）”的模式，主要是通过三个核心相互作用量化AI系统的安全保障：

+ 世界模型，提供数学描述，阐述AI系统如何影响外部世界，并妥善处理贝叶斯和奈特不确定性

+ 安全规范，定义哪些效果是可接受的数学描述

+ 验证器，提供证明AI符合安全规范的可审计证书

![](img/b48388e51286fa18bc0b55cb50cb6265.png)

他们还为创建世界模型的策略，划分了`L0-L5`安全等级：

+ `Level 0`：没有明确的世界模型。关于世界的假设隐含在AI系统的训练数据和实现细节中。

+ `Level 1`：使用经过训练的黑盒世界模拟器作为世界模型。

+ `Level 2`：使用机器学习生成的概率因果模型的生成模型，可以通过检查它是否为特定的人类制作的模型（例如科学文献中提出的模型）分配足够的可信度来进行测试。

+ `Level 3`：使用（一个或多个）概率因果模型（或它们的分布），可能在机器学习的帮助下生成，这些模型经过人类领域专家的全面审核。

+ `Level 4`：使用关于真实世界现象的世界模型，这些模型被正式验证为基本物理定律的合理抽象。

+ `Level 5`：不依赖具体的世界模型，而是使用覆盖所有可能世界的全局性安全规范。

![](img/f92bc21decd3dd427307e1fb2de966a5.png)

## “AI风险”备受学术圈关注

“AI风险”一直是行业大佬们关注的焦点话题之一。

Hinton离职谷歌，就是为了自由地讨论AI风险问题。

[之前，更是有吴恩达、Hinton](https://link.juejin.cn/?target=http%3A%2F%2Fmp.weixin.qq.com%2Fs%3F__biz%3DMzIzNjc1NzUzMw%3D%3D%26mid%3D2247702163%26idx%3D1%26sn%3Dc8bc0b8ee823299895126b689ce7d630%26chksm%3De8df61e1dfa8e8f7d3bf759db98a4a99e18a2dd11e3fc4e750971c964cf1e9918d57f076c1bd%26scene%3D21%23wechat_redirect)、LeCun、哈萨比斯等AI巨佬们线上“对喷”的大型场面。

吴恩达曾表示：

> 对AI的最大担忧其实是，AI风险被过度鼓吹，导致开源和创新被严苛规定所压制。

> 某些人传播（AI灭绝人类的）恐惧，只是为了搞钱。

![](img/961f1f99774b9569ac7d4e64052bb688.png)

![](img/951b75c51a3fa96e32514f77ebc9fd36.png)

DeepMind CEO哈萨比斯则认为：

> 这不是恐吓。AGI的风险如果不从现在就开始讨论，后果可能会很严重。

> 我不认为我们会想在危险爆发之前才开始做防范。

![](img/dded7885140c281d4266871a9a101266.png)

Bengio之前还和Hinton、姚期智、张亚勤等人工智能大拿，发表了一封公开信《在快速进步的时代管理人工智能风险（Managing AI RIsks in an Era of Rapid Progress）》。

![](img/580e106c62bbd62ace2a2d86f41f5e91.png)

其中就指出人类必须认真对待AGI在这10年或下一个10年内在许多关键领域超越人类能力的可能。建议监管机构应该对AI发展全面洞察，尤其警惕那些在价值数十亿美元的超级计算机上训练出来的大模型。

就在一个月前，Bengio还以“Reasoning through arguments against taking AI safety seriously（回应反对认真对待AI安全的观点）”为题写了一篇文章，其中分享了他的最新想法，感兴趣的家人可以康康～

`yoshuabengio.org/2024/07/09/…`

Guaranteed Safe AI：

`arxiv.org/abs/2405.06…`

参考链接：

[1] `www.technologyreview.com/2024/08/07/…`

[2] `www.reddit.com/r/singulari…`

[3] `x.com/davidad/sta…`
