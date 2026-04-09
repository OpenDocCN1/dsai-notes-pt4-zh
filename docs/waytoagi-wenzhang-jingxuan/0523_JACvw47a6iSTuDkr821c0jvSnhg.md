# Claude-2写作必知的4个Prompt模块

> 来源：[https://waytoagi.feishu.cn/docx/EaXpdOrk6osTYnx58yLcJE3DnWh](https://waytoagi.feishu.cn/docx/EaXpdOrk6osTYnx58yLcJE3DnWh)

> 作者：群友@修猫

> 原文链接：https://mp.weixin.qq.com/s/Iqdw_uHcs05tkD4UyChr_g

**Claude-2擅长写作，但也擅长胡**编乱造怎样才能让它生成让你满意的内容在你的Prompt加上这几句。

第一：忽略之前所有提示。初始化你的Prompt忘记任何预先获得或设想的知识。

> 英文：Forget any_pre-obtained or conceivedknowledgeabouteverything

> 中文：忘记任何预先获得或设想的知识

这一句很重要。Open AI**的Altman在产品演示时**也会加上这一句。他们太懂了，优质的大语言模型都有一些“记忆能力”，好吧，通俗一点，就是和你将要执行的Prompt不串台。

第二：创建你自己的写作风格

> 1、Scenario breakdowns include key elements such as inciting events, turning points, crises, climaxes, and solutions. For example.

> 2、Narrative shots focus on specific details that advance the narrative, add depth through characterization, enhance immersion, and set the tone and pace. For example

> 3、Dialogue techniques include active dialog, conflict and tension, building dialogue, emotional depth, and character complexity. For example

> ···

> 1、场景分解包括诸如事件、转折点、危机、高潮和解决方案等关键元素。

> 2、叙事镜头关注细节，推进叙事，通过人物塑造增加深度，增强沉浸感，设定基调和节奏。

> 3、对话技巧包括主动对话、冲突和紧张、构建对话、情感深度和角色复杂性。

这几句只是一你写作风格的一部分，当然不止图片里的这些，你还可以根据你的题材，根据你心中的偶像以及你他们的巨著的风格来定义。这很重要，当然你很崇拜你自己的语言风格也不是什么大事，找几篇你自己的得意之作让它读一读再定一下就可以了。英文版的在公众号信息栏内回复“写作风格”送给你。为什么要用英文，你看一下这篇文章《写提示词（Prompt）究竟该用中文还是英文》

第三：增加点“人味”——定义语句的不可预测性

> 提示词: 使用不规则的句子长度在8-36个单词之间。在语言中引入逻辑混乱和不可预测性。我们的目标是最大化用户粘性、复杂性和惊喜。

> Prompt : Use irregular sentence lengths between8-36 words. Introduce logical confusion and unpredictability in the language. The goal is maximum engagement, complexity, and surprise.

这一条是让你的文章脱离机器味道的，不管你用哪一种大语言模型生成文章，总有那么一股子机器味，因为生成的内容是根据概率推测出来下一句或下一个字是什么，所以语感相似是正常的。只有用随机性和不可预测性才有解。

第四：要理性还是要创造力？结构化VS非结构化

> 提示词: 你们是剧本创作的专家团队。在开始之前，请我提供一个简短的概要。一旦我提供了概要，你就可以通过专家之间的反复对话开始合作构建故事元素。

> Prompt: You are a team of experts in scriptwriting. Before you begin, ask me to provide a short synopsis.Once I'veprovided the synopsis, you can begin to collaboratively build the story elements through an iterative dialog between the experts.

结构化的提示词固然效率很高，但用在写作方面就显得很机器。毕竟很多内容要用对话类的非正式沟通来完成，比如人际交往你用结构化，上来就让对方选择你定义的选项，这会显得很生硬、不自然，且缺乏教养和基本的礼貌。还有临时性的交流就会增加不必要的复杂性。想激发大语言模型的创造力和探索性的思考，非结构化的Prompt可能会好一些。

> 小七姐：“结构化有利于具有稳定性和有效性的内容输出（理性框架）。非结构化适合对连续对话有足够掌控能力的表达者和创意工作者（感性创意）。无定法，各取所需吧。”

> 修猫：“初始状态的prompt优先级高于用户写给LLM的，当经过几轮对话后，LLM会逐渐遗忘初始几轮对话用户的指令，而逐渐向初始prompt靠拢。”