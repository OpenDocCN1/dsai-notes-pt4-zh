# “创造神迹” -> “打造利器”：AI 从实验室走向市场的五大障碍

> 来源：[https://agijuejin.feishu.cn/docx/F2c4dcqj6oq2iKxhRNRc7tDDnge](https://agijuejin.feishu.cn/docx/F2c4dcqj6oq2iKxhRNRc7tDDnge)

**原文链接：****https://mp.weixin.qq.com/s/hw**7QloSf6OKK_Gt2CWtX_w

**作者 | Arvind Naray**anan & Sayash Kapoor

**编译 | 岳扬**

---

**编者按：**AI 能立即改变世界吗？为何巨额投资却难见成效？你是否也在思考：我们开发的 AI 产品真的解决了用户的需求吗？

面对这些问题，许多公司陷入了困境：要么过于专注模型开发而忽视实际应用，要么盲目将 AI 融入产品而不考虑是否需要。这不仅导致资源浪费，更可能使我们错失 AI 真正的价值所在。

本文深入剖析了 AI 从实验室走向市场的五大障碍，包括使用成本、产品可靠性、隐私问题、产品安全和用户界面。作者基于对 OpenAI、Google 等 AI 公司的观察，认为 AI 公司正从追求创造"神迹"转向专注于构建实用产品，这是一个积极的转变。

这篇文章为我们提供了一个清晰的视角，帮助我们理解 AI 行业面临的实际挑战。它提醒我们，尽管 AI 技术发展迅速，但将其转化为成功的商业产品仍需要时间和大家的不懈努力。

**作者 | Arvind Naray**anan & Sayash Kapoor

**编译 | 岳扬**

AI 企业正集体计划在硬件和数据中心上投入高达万亿美元([1])的资金，然而迄今为止，这些投入所带来的成果却相对较少。这一现象也引发了许多人的担忧，他们怀疑 GenAI 是否只是一场泡沫([2])。我们不会对未来的事情做出任何预测，但我们认为，对于事情当初是如何走到这一步的，我们已经有了一个清晰的认识。

**本文将探讨 AI 企业所犯的那些错误，以及他们是如何努力纠正这些错误的。接下来我们将讨论人工智能公司**为了使 GenAI 在商业上取得足够的成功，从而证明之前在硬件和数据中心上的巨额投资是合理的，它们还需要跨越的五大障碍。

# 01 **产品与市场的契合点**

ChatGPT 一经推出，便被用户们发掘出数种意想不到的用途，这让 AI 开发者们激动不已。然而，他们并未准确把握市场脉搏，忽视了概念验证（POC）与可靠产品之间存在的巨大鸿沟。这种市场误判导致出现了两种截然相反的大语言模型（LLMs）商业化方法，但两者都存在缺陷。

OpenAI 和 Anthropic 专注于模型的构建，对产品开发则显得不够重视。比如，OpenAI 花了 6 个月的时间才发布了 ChatGPT iOS App，而 Android App 更是又晚了 8 个月才面市！

与此同时，谷歌和微软在竞争中慌不择路，将 AI 技术匆忙植入各种产品，却未深入思考哪些产品真正需要 AI 的加持，以及 AI 应该如何与这些产品整合。

这两家公司都忘记了 “打造用户所需产品” 这一核心原则。大语言模型的通用性让开发者误以为，他们可以不必寻找产品与市场的契合点，似乎只要让模型执行某个任务，就能取代之前精心设计的产品或功能。

OpenAI 和 Anthropic 采取的自主开发（DIY）方法导致出现了一个问题：早期使用大语言模型（LLMs）的用户群体中，不能代表普遍的大模型使用者。因为这些人更愿意投入精力去研究如何利用这些新技术去实现自己的目的，而普通用户则更倾向于使用简单易用的产品。这种情况使得该技术的公众形象产生了负面印象。(1)

与此同时，**微软和谷歌那种“强行植入 AI ”的做法，虽然偶尔能带**来便利，但更多时候却让用户感到不胜其烦。此外，由于测试工作没做到位，还出现了许多本可避免的错误，比如微软早期的 Sydney([3]) 聊天机器人和谷歌的 Gemini([4]) 图像生成器。这些情况同样引发了公众的不满和抵触。

不过，这些公司现在正在调整运营策略。OpenAI 似乎正在从专注于基础研究和创新探索的领域转型，更加倾向于开发和推动具体的产品与服务走向市场化。如果撇开 OpenAI 董事会内部的戏剧性事件不谈，其核心转变是从追求创新神迹（creating gods）到专注于产品开发（building products）。Anthropic 则吸纳了许多之前在 OpenAI 工作、更关注通用人工智能（AGI）研究的科研人员和开发人员，他们在 OpenAI 感到格格不入。尽管如此，Anthropic 也认识到了打造产品的必要性。

谷歌和微软在这方面动作较慢，但我们推测苹果可能会迫使它们做出改变。去年，苹果([5])在 AI 领域似乎还处于落后状态，但事后来看，苹果在其全球开发者大会（WWDC）上所展示的那种谨慎而深思熟虑的 AI 发展策略，似乎更能够赢得用户的认可(2)。谷歌在其即将推出的新款 Pixel 手机([6])和新版本的 Android 操作系统中，对于如何整合 AI 似乎进行了更多的思考，比起在搜索功能上应用 AI 要用心得多，不过它们还未上市，我们不妨拭目以待。

再来看看 Meta，其旨在利用人工智能在其依赖广告收益的社交媒体平台上创造内容，并增强用户的互动参与度。在一个充斥着 AIGC（人工智能生成内容，Artificial Intelligence Generated Content）的世界，其社会影响无疑是复杂且具有两面性的([7])。然而，从商业策略的角度来看，这一方针无疑是明智的。

# 02 **基于 AI 的面向消费者型产品（**consumer AI）需跨越的五大障碍

为了打造吸引人的基于 AI 的面向消费者型产品，开发人员需跨越大语言模型（LLMs）的五大障碍。(3)

## **2.1 产品成本**

在众多应用场景中，模型的能力并非限制因素，成本才是。即便是简单的聊天应用，成本也限制了 chatbot 能够回溯的对话历史长度 —— 随着对话时间的延长，对整个对话历史进行处理的成本会迅速飙升，变得令人难以承受。

大模型的成本已经显著下降 —— 过去 18 个月里，同等能力的模型成本降低了超过 100 倍。(4)因此，有企业宣称 LLMs 已达到或即将达到“便宜到微不足道”([8])的水平。不过，等他们免费开放 API 时，我们才会真正相信这一点。

**我们认为成本问题仍将是一个关注点，因为在许多大模型应**用中，成本的降低直接意味着准确性的提升。这是因为 LLMs 存在随机性，多次重复尝试完成任务（十次、千次甚至百万次）是一种提高成功率的有效方法。因此，在相同预算的情况下，模型越便宜，我们就能进行更多次尝试。我们在近期一篇关于 AI Agents 的论文([9])中对此进行了量化分析。此后，也有多篇论文([10][11])提出了类似的观点。

尽管如此，我们还是有理由相信，在不久之后我们将进入一个新阶段，届时在多数应用场景中，成本的优化（cost optimization）将不再是一个重要问题。

## **2.2 产品可靠性**

产品的能力和可靠性在我们看来是两个相对独立的概念。如果一个 AI 系统有 90% 的概率正确完成一项任务，我们可以说它具备完成该任务的能力，但却不能保证稳定可靠地完成。那些让我们达到 90% 正确率的技术，却未必能帮助我们达到 100%。

对于基于统计学习的系统来说，追求完美的准确性本身就是一项艰巨的任务。回想机器学习在广告精准定向、欺诈检测，或是近期在天气预报领域的成功案例，完美的准确性并非目标 —— 只要系统的表现优于现有技术水平，它就是有价值的。即便在医疗诊断和其它医疗健康领域，我们也能接受一定程度的误差([12])。

然而，当 AI 被应用于面向消费者型产品时，用户期望它能像以前的传统软件一样运行，即需要它们能够稳定无误地工作。如果一款 AI travel agent 只有 90% 的几率能正确预订假期目的地，那它就不会成功。正如我们之前写过的文章([13])，可靠性不足是近期一些基于 AI 的小工具失败的原因之一。

AI **开发者们对此问题的认识相对较为迟缓，原因在于我们这些专家习惯于将 AI **看成与传统软件有本质区别的东西。比如，我们俩在日常工作中对聊天机器人和 AI Agents 的使用频率非常高，对于这些工具可能出现的幻觉和不可靠之处，我们几乎能够自动地进行规避。去年，AI 开发者们或许还寄希望于非专业用户能够学会适应 AI，但现在逐渐清楚的是，企业需要调整 AI 以符合用户的期望，让 AI 像传统软件一样运行。

提高 AI 的可靠性是我们普林斯顿团队的研究重点。目前，能否利用随机性组件（LLMs）构建出确定性系统，还是一个尚未解答的问题。有些公司宣称已经解决了可靠性问题 —— 比如，一些法律科技公司宣传他们的系统“不会产生幻觉”。但事实证明，这些说法为时尚早([14])。

## **2.3 隐私问题**

在机器学习的发展历程来看，它通常依赖于包含敏感信息的数据源（data sources），比如利用历史浏览记录进行广告定向推送或使用医疗记录发展医疗健康科技([15])。然而，LLMs 在这方面显得有些特立独行，因为它们主要是通过公共资源（如网页和书籍）来进行训练的。(5)

但随着** **AI assistants 的普及，隐私问题再次成为焦点。为了打造实用的 AI assistants，企业需要利用用户的交互数据来训练系统。例如，如果想要 AI 在电子邮件这一使用场景表现优异，那么就必须在电子邮件数据上进行训练([16])。不过，各大企业在隐私政策（privacy policies）上对此往往语焉不详，目前尚不清楚这种情况的实际影响范围。(6)电子邮件、文档和屏幕截图等数据可能比聊天记录更为敏感。

此外，还有一种与推理而非训练有关的隐私问题。AI assistants 要想为我们提供便利，就必须能够访问我们的个人数据。例如，微软曾宣布要设计一项饱受争议的功能，该功能将每隔几秒钟对用户的电脑屏幕进行截图，以便其 CoPilot AI 能够对用户的行为进行记忆、学习。但这一举动引发了公众的强烈抗议，微软最终不得不放弃这一计划([17])。

我们在这里提醒大家，不要仅仅从技术层面理解隐私问题，比如说“数据永远不会离开设备”。Meredith Whittaker 指出([18])，设备上的欺诈行为检测（fraud detection）可能会使持续监控变得常态化，而且所构建的这种基础设施有可能被不良分子恶意利用。尽管如此，技术创新([19])确实能够在一定程度上帮助我们保护隐私。

## **2.4 产品的安全问题**

在产品安全方面，存在一系列相关的问题：例如 Gemin**i 的图****像生成功能存在****偏见([**20])等意外故障；例如声音克隆或 deepfakes 等 AI 的滥用；以及如提示词注入（prompt injection）等黑客攻击，可能会泄露用户数据或以其他方式损害用户权益。

**我们认为意外故障是可以修复的。至于大多数类**型的滥用，我们的观点是，无法创建([21])一个不会被滥用的模型，因此必须主要从下游进行防御。当然，并非每个人都同意这种观点，因此很多公司将继续因为不可避免的滥用而不断受到负面新闻的影响，但这些公司似乎已经将这视为开展 AI 业务的一部分经营成本😄。

让我们谈谈第三类 —— 黑客攻击。据我们所知，这是大部分公司似乎最不关注的问题。至少从理论上讲，灾难性的黑客攻击([22])是有可能发生的，例如 AI worms（译者注：比如电子邮件场景的 AI 蠕虫病毒，黑客会精心构建一个“有毒提示词”，然后通过电子邮件发送给受害者A，受害者A可能并不知情，但是其 AI assistants 会查看这封“蠕虫邮件”，然后执行其中的命令，发送给更多人 B、C、D、E...） 会在用户之间传播，诱骗这些用户的 AI assistants 执行有害行为（其中包括创建更多 worm 的副本）。

虽然已在许多概念验证演示（proof-of-concept demonstrations）和漏洞赏金计划([23][24])中发现了已部署产品中的这些漏洞，但我们还没有看到这种类型的在野攻击。我们不确定这是否是因为 AI assistants 的使用率较低导致的，还是因为市面上这些公司笨拙的防御措施([25])已被证明足够有效，抑或是其他原因。时间会证明一切！

## **2.5 产品的用户界面**

因为大语言模型（LLMs）存在不确定性，所以在许多应用场景中，如果机器人出现偏差，用户需要能够介入进行干预。在 chatbot 中，可能只是重新生成答案或显示多个版本的答案，让用户自行选择。但在错误成本较高的应用场景中（例如预订航班），如要确保充分的监督便更加棘手了，系统必须避免频繁打扰用户，以免引起他们的不满。

自然语言**用户界面的问**题更为复杂，在这种用户界面中，用户与 AI assistants 进行对话， AI assistants 也会以自然语言回应，这正是 GenAI 巨大潜力的体现所在。仅举一个例子，如果人工智能能够集成到智能眼镜([26])中，并在你需要的时刻无需询问便主动与你对话——比如检测到你正在凝视一个外语标志——这将是一种与目前完全不同的体验。然而，受限的（译者注：constrained，可能指的是那些不支持复杂交互或只提供有限操作选择的界面。）用户界面几乎不允许出现错误或意外的行为。

# 03 Concluding thoughts

AI 的支持者经常声称，由于 AI 能力的快速提升，我们应该很快就能看到其对社会和经济产生的巨大影响。我们对当前 AI 能力未来趋势的预测和不严谨的思考持怀疑态度([27])。更重要的是，即便 AI 能力确实迅速提升，开发者也需要跨越上文中的这些障碍。这些障碍涉及到社会和技术等方面的因素，而非纯粹技术上的问题，因此进展会比较缓慢。即使这些障碍得到了解决，企业也需要将 AI 整合到现有产品和工作流中，并培训员工如何有效地使用 AI，同时避免其潜在的陷阱。我们预期这个过程应该将在十年或更长时间内而不是一两年内完成。

# 04 **Further reading**

Benedict Evans 曾撰文讨论过([28][29])使用通用语言模型来构建单一用途软件（single-purpose software）的重要性。

**脚注：**

1.  明确地说，我们并不认为限制对最先进模型的访问就能减少滥用。但考虑到 LLMs（大语言模型）的滥用比合法使用更为容易（合法使用更需要经过深思熟虑），因此滥用现象普遍存在并不令人意外。

1.  AI 的整合速度是相对的。即便是苹果在其产品中整合 AI 的方法，也受到了过于快速的批评。

1.  这些因素都与用户体验息息相关；我们暂且不考虑环境成本（environmental costs）、版权问题（training on copyrighted data）等因素。

1.  例如，GPT-3.5（text-davinci-003）API 每百万个 token 的成本为 20 美元，而更强大的 gpt-4o-mini 的成本仅为 15 美分。

1.  需要明确指出，数据源公开并不意味着没有隐私问题。

1.  例如，谷歌声称：“我们使用公开可用的信息来帮助训练谷歌的 AI 模型。”在其它场合，谷歌表示可能使用如电子邮件等私人数据来提供服务、维护和改善服务、提供个性化服务和开发新服务。与这些已披露信息一致的一种方法为，仅使用公共数据来预训练如 Gemini 这样的模型，而使用私人数据来微调这些模型，以创建电子邮件自动回复机器人等应用。Anthropic 是我们所知的唯一例外。它表示：“我们不会在未经用户明确许可的情况下使用用户提交的数据来训练我们的生成模型。截至目前，我们尚未使用任何客户或用户提交的数据来训练我们的生成模型（generative models）。”尽管他们对保护用户隐私的承诺令人钦佩，但我们预测，如果该公司想要进一步构建产品，这将使该公司处于不利地位。

Thanks for reading!

Hope you have enjoyed and learned new things from this blog!

## About the authors

Arvind Narayanan is a professor of computer science at Princeton and the director of the Center for Information Technology Policy. He led the Princeton Web Transparency and Accountability Project to uncover how companies collect and use our personal information. His work was among the first to show how machine learning reflects cultural stereotypes. Narayanan is a recipient of the Presidential Early Career Award for Scientists and Engineers (PECASE).

**Sayash Kapoor** is a computer science Ph.D. candidate at Princeton University's Center for Information Technology Policy. His research focuses on the societal impact of AI. He previously worked on AI in the industry and academia at Facebook, Columbia University, and EPFL Switzerland. He is a recipient of a best paper award at ACM FAccT and an impact recognition award at ACM CSCW.

#

**END**

**本期互动内容 🍻**

**❓**文章列举了 AI 走向市场需要克服的5个障碍（成本、可靠性、隐私、安全、用户界面）。你认为哪个障碍最难克服？为什么？

![](img/d3136f66477dfe789d05d537d19a3a26.png)

**🔗文中链接🔗**

[1]https://www.goldmansachs.com/images/migrated/insights/pages/gs-research/gen-ai--too-much-spend,-too-little-benefit-/TOM_AI%202.0_ForRedaction.pdf

[2]https://www.inc.com/sam-blum/new-warnings-ai-bubble-when-could-it-burst.html

[3]https://www.nytimes.com/2023/02/16/technology/bing-chatbot-microsoft-chatgpt.html

[4]https://www.theverge.com/2024/2/21/24079371/google-ai-gemini-generative-inaccurate-historical

[5]https://www.vox.com/technology/354794/apple-artificial-intelligence-ai-wwdc

[6]https://blog.google/products/pixel/google-pixel-9-pro-xl/

[7]https://knightcolumbia.org/content/how-to-prepare-for-the-deluge-of-generative-ai-on-social-media

[8]https://x.com/sama/status/1813984333352649087

[9]https://www.aisnakeoil.com/p/new-paper-ai-agents-that-matter

[10]https://arxiv.org/pdf/2407.21787v1

[11]https://arxiv.org/html/2408.03314v1

[12]https://www.himss.org/news/north-carolina-hospital-system-reduces-sepsis-cases-using-predictive-analytics

[13]https://www.aisnakeoil.com/p/new-paper-ai-agents-that-matter

[14]https://dho.stanford.edu/wp-content/uploads/Legal_RAG_Hallucinations.pdf

[15]https://www.wired.com/story/google-deepmind-nhs-health-data/

[16]https://www.nytimes.com/2024/06/26/technology/terms-service-ai-training.html

[17]https://www.bbc.com/news/articles/cd11rje1mrro

[18]https://mobile.x.com/mer__edith/status/1790692059059200017

[19]https://www.wired.com/story/apple-intelligence-android-hybrid-ai-privacy/

[20]https://www.cnbc.com/2024/02/26/googles-gemini-ai-picture-generator-to-relaunch-in-a-few-weeks.html

[21]https://www.aisnakeoil.com/p/ai-safety-is-not-a-model-property

[22]https://arxiv.org/abs/2302.12173

[23]https://embracethered.com/blog/posts/2023/google-bard-data-exfiltration/

[24]https://www.landh.tech/blog/20240304-google-hack-50000/

[25]https://kai-greshake.de/posts/approaches-to-pi-defense/

[26]https://www.techradar.com/computing/artificial-intelligence/i-finally-tried-the-meta-ai-in-my-ray-ban-smart-glasses-thanks-to-an-accidental-uk-launch-and-its-by-far-the-best-ai-wearable

[27]https://www.aisnakeoil.com/p/ai-scaling-myths

[28]https://www.ben-evans.com/benedictevans/2023/10/5/unbundling-ai

[29]https://www.ben-evans.com/benedictevans/2024/6/8/building-ai-products

**本文经原作者授权🪶，由 Baihai IDP** 编译。如需转载译文，请联系获取授权。

**原文链接：**

https://www.aisnakeoil.com/p/ai-companies-are-pivoting-from-creating