# 【中&英】《生成人工智能手册》Generative AI Handbook: A Roadmap for Learning Resources

> 来源：[https://aigcreative.feishu.cn/docx/RxtGdtZmGolxSexNfErc1l2pn3e](https://aigcreative.feishu.cn/docx/RxtGdtZmGolxSexNfErc1l2pn3e)

> [作者：Willia](https://x.com/willccbb)m Bro[wn  ](https://github.com/genai-handbook/genai-handbook.github.io)    X@willccbb     v0.1 (June 5, 2024)

# Introduction 介绍

This document aims to serve as a handbook for learning the key concepts underlying modern artificial intelligence systems. Given the speed of recent development in AI, there really isn’t a good textbook-style source for getting up-to-speed on the latest-and-greatest innovations in LLMs or other generative models, yet there is an abundance of great explainer resources (blog posts, videos, etc.) for these topics scattered across the internet. My goal is to organize the “best” of these resources into a textbook-style presentation, which can serve as a roadmap for filling in the prerequisites towards individual AI-related learning goals. My hope is that this will be a “living document”, to be updated as new innovations and paradigms inevitably emerge, and ideally also a document that can benefit from community input and contribution. This guide is aimed at those with a technical background of some kind, who are interested in diving into AI either out of curiosity or for a potential career. I’ll assume that you have some experience with coding and high-school level math, but otherwise will provide pointers for filling in any other prerequisites. Please let me know if there’s anything you think should be added!

本文档旨在作为学习现代人工智能系统背后的关键概念的手册。考虑到人工智能最近的发展速度，确实没有一个好的教科书式的资源来快速了解 LLMs 或其他生成模型的最新和最伟大的创新，但互联网上有大量关于这些主题的优秀解释资源（博客文章、视频等）。我的目标是将这些资源中的“最好的”组织成教科书式的演示文稿，它可以作为满足个人人工智能相关学习目标的先决条件的路线图。我希望这将是一份“活文件”，随着新的创新和范式不可避免地出现而进行更新，并且理想情况下也是一份可以从社区投入和贡献中受益的文件。本指南针对的是那些具有某种技术背景、出于好奇或潜在职业而有兴趣深入研究人工智能的人。我假设您有一些编码和高中数学水平的经验，但否则将提供填写任何其他先决条件的指导。如果您认为有什么需要补充的，请告诉我！

## The AI Landscape 人工智能前景

As of June 2024, it’s been ab[out 18 ](http://chat.openai.com/)[months](https://openai.com/) since ChatGPT was released by OpenAI and the world sta[rted](https://llama.meta.com/) talk[ing a ](https://gemini.google.com/)lot more about artificial intelligen[ce. Muc](https://mistral.ai/)h has[ happened](https://www.anthropic.com/) since: tech giants like Meta and Google have released large l[anguage mo](https://finance.yahoo.com/news/customer-demand-nvidia-chips-far-013826675.html)dels of their own, newer[ orga](https://arxiv.org/list/cs.AI/recent)nizations like Mistral and Anthropic have proven to be serious contenders as well, innumerable startups have begun building on top of their APIs, everyone is scrambling for powerful Nvidia GPUs, papers appear on ArXiv at a breakneck pace, demos circulate of physical robots and artificial programmers powered by LLMs, and it seems like chatbots are finding their way into all aspects of online life (to varying degrees of success). In parallel to the LLM race, there’s been rapid development in image generation via diffusion models; DALL-E and Midjourney are displaying increasingly impressive results that often stump humans on social media, and with the progress from Sora, Runway, and Pika, it seems like high-quality video generation is right around the corner as well. There are ongoing debates about when “AGI” will arrive, what “AGI” even means, the merits of open vs. closed models, value alignment, superintelligence, existential risk, fake news, and the future of the economy. Many are concerned about jobs being lost to automation, or excited about the progress that automation might drive. And the world keeps moving: chips get faster, data centers get bigger, models get smarter, contexts get longer, abilities are augmented with tools and vision, and it’s not totally clear where this is all headed. If you’re following “AI news” in 2024, it can often feel like there’s some kind of big new breakthrough happening on a near-daily basis. It’s a lot to keep up with, especially if you’re just tuning in.

截至 2024 年 6 月，自 OpenAI 发布 ChatGPT 以来已经过去了大约 18 个月，世界开始更多地谈论人工智能。此后发生了很多事情：Meta 和 Google 等科技巨头发布了自己的大型语言模型，Mistral 和 Anthropic 等较新的组织也被证明是有力的竞争者，无数初创公司开始在他们的 API 基础上构建，每个人都在争夺强大的 Nvidia GPU、论文以极快的速度出现在 ArXiv 上、由 LLMs 驱动的物理机器人和人工程序员的演示在流传，聊天机器人似乎正在寻找进入在线生活的各个方面的方法（在不同程度上）的成功）。与LLM竞赛同时进行的，通过扩散模型生成图像也取得了快速发展； DALL-E 和 Midjourney 正在展示越来越令人印象深刻的结果，这些结果常常在社交媒体上难倒人们，随着 Sora、Runway 和 Pika 的进步，高质量视频生成似乎也指日可待。关于“AGI”何时到来、“AGI”到底意味着什么、开放模型与封闭模型的优点、价值一致性、超级智能、存在风险、假新闻和经济的未来，一直存在争论。许多人担心自动化会导致工作岗位流失，或者对自动化可能推动的进步感到兴奋。世界在不断发展：芯片变得更快，数据中心变得更大，模型变得更智能，上下文变得更长，能力通过工具和愿景得到增强，但目前尚不完全清楚这一切的发展方向。如果您在 2024 年关注“人工智能新闻”，您通常会感觉几乎每天都会发生某种重大的新突破。有很多事情需要跟上，尤其是当你刚刚收听时。

With[ progr](https://openai.com/index/hello-gpt-4o/)es[s happening so](https://deepmind.google/technologies/gemini/pro/) quic[kly, a natura](https://www.anthropic.com/news/claude-3-family)l inclination [by those see](https://huggingface.co/)ki[ng to “g](https://scale.com/)et in [on the acti](https://www.together.ai/)on” is to pick up the latest-and-greatest available tools (likely GPT-4o, Gemini 1.5 Pro, or Claude 3 Opus as of this writing, depending on who you ask) and try to build a website or application on top of them. There’s certainly a lot of room for this, but these tools will change quickly, and having a solid understanding of the underlying fundamentals will make it much easier to get the most out of your tools, pick up new tools quickly as they’re introduced, and evaluate tradeoffs for things like cost, performance, speed, modularity, and flexibility. Further, innovation isn’t only happening at the application layer, and companies like Hugging Face, Scale AI, and Together AI have gained footholds by focusing on inference, training, and tooling for open-weights models (among other things). Whether you’re looking to get involved in open-source development, work on fundamental research, or leverage LLMs in settings where costs or privacy concerns preclude outside API usage, it helps to understand how these things work under the hood in order to debug or modify them as needed. From a broader career perspective, a lot of current “AI/ML Engineer” roles will value nuts-and-bolts knowledge in addition to high-level frameworks, just as “Data Scientist” roles have typically sought a strong grasp on theory and fundamentals over proficiency in the ML framework du jour. Diving deep is the harder path, but I think it’s a worthwhile one. But with the pace at which innovation has occurred over the past few years, where should you start? Which topics are essential, what order should you learn them in, and which ones can you skim or skip?

随着进展如此之快，那些寻求“参与行动”的人自然倾向于选择最新最好的可用工具（截至撰写本文时，可能是 GPT-4o、Gemini 1.5 Pro 或 Claude 3 Opus） ，取决于你问的是谁）并尝试在它们之上构建一个网站或应用程序。当然还有很大的空间，但这些工具会很快发生变化，对底层基础知识有深入的了解将使您更容易地充分利用您的工具，并在新工具推出时快速使用它们，并评估成本、性能、速度、模块化和灵活性等方面的权衡。此外，创新不仅仅发生在应用层，Hugging Face、Scale AI 和 Together AI 等公司通过专注于开放权重模型的推理、训练和工具（等等）而站稳了脚跟。无论您是想参与开源开发、从事基础研究，还是在因成本或隐私问题而无法使用外部 API 的环境中利用 LLMs，了解这些事情在hood，以便根据需要调试或修改它们。从更广泛的职业角度来看，当前的许多“AI/ML 工程师”角色除了高级框架之外还将重视具体知识，就像“数据科学家”角色通常寻求对理论和基础知识的牢固掌握一样对当前的 ML 框架过于熟练。深入研究是一条更艰难的道路，但我认为这是一条值得的道路。但随着过去几年创新的步伐，您应该从哪里开始呢？哪些主题是必不可少的，您应该按什么顺序学习它们，以及您可以浏览或跳过哪些主题？

## The Content Landscape 内容格局

Textbooks are g[reat for prov](https://www.deeplearningbook.org/)iding a high-level roadmap of fields where the set of “key ideas” is more stable, [but as](https://web.stanford.edu/class/cs224n/index.html#coursework) far as I can tell, there really isn’t a publicly available post-ChatGPT “guide to AI” with textbook-style comprehensiveness or organization. It’s not clear that it would even make sense for someone to write a traditional textbook covering the current state of AI right now; many key ideas (e.g. QLoRA, DPO, vLLM) are no more than a year old, and the field will likely have changed dramatically by the time it’d get to print. The oft-referenced Deep Learning book (Goodfellow et al.) is almost a decade old at this point, and has only a cursory mention of language modeling via RNNs. The newer Dive into Deep Learning book includes coverage up to Transformer architectures and fine-tuning for BERT models, but topics like RLHF and RAG (which are “old” by the standards of some of the more bleeding-edge topics we’ll touch on) are missing. The upcoming “Hands-On Large Language Models” book might be nice, but it’s not officially published yet (available online behind a paywall now) and presumably won’t be free when it is. The Stanford CS224n course seems great if you’re a student there, but without a login you’re limited to slide-decks and a reading list consisting mostly of dense academic papers. Microsoft’s “Generative AI for Beginners” guide is fairly solid for getting your hands dirty with popular frameworks, but it’s more focused on applications rather than understanding the fundamentals.

教科书非常适合提供一系列领域的高级路线图，其中“关键思想”集更加稳定，但据我所知，确实没有公开的 ChatGPT 后“人工智能指南”和教科书-风格的全面性或组织性。目前还不清楚有人编写一本涵盖当前人工智能现状的传统教科书是否有意义；许多关键想法（例如 QLoRA、DPO、vLLM）都还不到一年的时间，到印刷时该领域可能已经发生了巨大变化。经常引用的《深度学习》一书（Goodfellow 等人）已经有近十年的历史了，并且只粗略地提到了通过 RNN 进行语言建模。较新的《深入深度学习》一书涵盖了 Transformer 架构和 BERT 模型的微调，但 RLHF 和 RAG 等主题（按照我们将要涉及的一些更前沿主题的标准来看，这些主题已经“过时”） ） 缺失。即将出版的《大型语言模型实践》一书可能很不错，但它尚未正式出版（现在可以通过付费专区在线获取），并且可能不会免费。如果您是斯坦福大学的学生，CS224n 课程似乎很棒，但如果没有登录，您只能使用幻灯片和主要由密集的学术论文组成的阅读列表。微软的“面向初学者的生成式人工智能”指南相当可靠，可以帮助您熟悉流行的框架，但它更侧重于应用程序而不是理解基础知识。

The closest resource I’m aware of to [what I hav](https://github.com/mlabonne/llm-course)e in mind is Maxime Labonne’s LLM Course on Github. It features many interactive code notebooks, as well as links to sources for learning the underlying concepts, several of which overlap with what I’ll be including here. I’d recommend it as a primary companion guide while working through this handbook, especially if you’re interested in applications; this document doesn’t include notebooks, but the scope of topics I’m covering is a bit broader, including some research threads which aren’t quite “standard” as well as multimodal models.

我所知道的与我的想法最接近的资源是 Github 上 Maxime Labonne 的 LLM 课程。它具有许多交互式代码笔记本，以及用于学习基本概念的资源链接，其中一些与我将在此处包含的内容重叠。我建议您在阅读本手册时将其作为主要配套指南，特别是如果您对应用程序感兴趣的话；本文档不包括笔记本，但我涵盖的主题范围更广泛，包括一些不太“标准”的研究线程以及多模式模型。

Still, there’s an abundance of other high-quality and accessible content which covers the latest advances in AI — it’s just not all organized. The best resources for quickly learning about new innovations are often one-off blog posts or YouTube videos (as well as Twitter/X threads, Discord servers, and discussions on Reddit and LessWrong). My goal with this document is to give a roadmap for navigating all of this content, organized into a textbook-style presentation without reinventing the wheel on individual explainers. Throughout, I’ll include multiple styles of content where possible (e.g. videos, blogs, and papers), as well as my opinions on goal-dependent knowledge prioritization and notes on “mental models” I found useful when first encountering these topics.

尽管如此，还有大量其他高质量且易于访问的内容涵盖了人工智能的最新进展——只是并非全部井井有条。快速了解新创新的最佳资源通常是一次性博客文章或 YouTube 视频（以及 Twitter/X 线程、Discord 服务器以及 Reddit 和 LessWrong 上的讨论）。我编写本文档的目标是提供一个导航所有这些内容的路线图，将其组织成教科书式的演示文稿，而无需重新设计各个解释器。在整个过程中，我将尽可能包含多种风格的内容（例如视频、博客和论文），以及我对目标相关知识优先顺序的看法以及我在第一次遇到这些主题时发现有用的“心理模型”注释。

**I’m** creating this document not as a “generative AI expert”, but rather as someone who’s recently had the experience of ramping up on many of these topics in a short time frame. While I’ve been working in and around AI since 2016 or so (if we count an internship project running evaluations for vision models as the “start”), I only started paying close attention to LLM developments 18 months ago, with the release of ChatGPT. I first started working with open-weights LLMs around 12 months ago. As such, I’ve spent a lot of the past year sifting through blog posts and papers and videos in search of the gems; this document is hopefully a more direct version of that path. It also serves as a distillation of many conversations I’ve had with friends, where we’ve tried to find and share useful intuitions for grokking complex topics in order to expedite each other’s learning. Compiling this has been a great forcing function for filling in gaps in my own understanding as well; I didn’t know how FlashAttention worked until a couple weeks ago, and I still don’t think that I really understand state-space models that well. But I know a lot more than when I started.

我并不是以“生成式人工智能专家”的身份来创建这份文档，而是以最近有过在短时间内熟悉这些主题的经验的人的身份来创建这份文档。虽然我从 2016 年左右开始就一直在人工智能领域工作（如果我们将运行视觉模型评估的实习项目算作“开始”），但我直到 18 个月才开始密切关注 LLM 的发展之前，随着 ChatGPT 的发布。大约 12 个月前，我第一次开始使用开放重量 LLMs。因此，在过去的一年里，我花了很多时间筛选博客文章、论文和视频，以寻找其中的精华；希望本文档是该路径的更直接版本。它也是我与朋友进行的许多对话的精华，我们试图找到并分享处理复杂主题的有用直觉，以加快彼此的学习速度。编译这篇文章对于填补我自己的理解空白也是一个很大的推动作用。直到几周前我才知道 FlashAttention 是如何工作的，而且我仍然认为我还没有真正理解状态空间模型。但我比刚开始的时候知道了很多。

## Resources 资源

Some of the sources we’ll draw from are:

我们将借鉴的一些来源是：

+   Blogs:  博客：

+   [Hugging Face](https://huggingface.co/blog) blog posts
拥抱脸博客文章

+   [Chip Huyen](https://huyenchip.com/blog/)’s blog  Chip Huyen 的博客

+   [Lilian Weng](https://lilianweng.github.io/)’s blog 翁莉莲的博客

+   [Tim Dettmers](https://timdettmers.com/)’ blog 蒂姆·戴特默斯的博客

+   Towards Data Science  走向数据科学

+   [Andrej Karpathy](https://karpathy.github.io/)’s blog  安德烈·卡帕蒂的博客

+   [Sebastian Ras](https://magazine.sebastianraschka.com/)chka’s “Ahead of AI” blog
Sebastian Raschka 的“Ahead of AI”博客

+   YouTube:  YouTube：

+   [Andrej Karpath](https://karpathy.ai/zero-to-hero.html)y’s “Zero to Hero” videos
安德烈·卡帕蒂 (Andrej Karpathy) 的“从零到英雄”视频

+   [3Blue1Brown](https://www.youtube.com/c/3blue1brown) videos  3Blue1Brown 视频

+   Mutual Information 互信息

+   StatQuest 统计任务

+   Textbooks  教科书

+   The [d2l.ai](http://d2l.ai/) interactive textbook
d2l.ai 互动教科书

+   The [Deep Learning](https://www.deeplearningbook.org/) textbook
深度学习教科书

+   Web courses:  网络课程：

+   [Maxime Lab](https://github.com/mlabonne/llm-course)onne’s LLM Course
Maxime Labonne 的 LLM 课程

+   Microsoft’s “Generative AI for Beginners”
微软的“初学者生成式人工智能”

+   Fast.AI’s [“Practical Deep ](https://course.fast.ai/)Learning for Coders”
Fast.AI 的“程序员实用深度学习”

+   Assorted university lecture notes
各种大学讲义

+   Original research papers (sparingly)
原创研究论文（少量）

I’ll often make reference to the original papers for key ideas throughout, but our emphasis will be on expository content which is more concise and conceptual, aimed at students or practitioners rather than experienced AI researchers (although hopefully the prospect of doing AI research will become less daunting as you progress through these sources). Pointers to multiple resources and media formats will be given when possible, along with some discussion on their relative merits.

我经常会参考原始论文的关键思想，但我们的重点将是更简洁和概念性的说明性内容，针对的是学生或从业者，而不是经验丰富的人工智能研究人员（尽管希望进行人工智能研究的前景会更好）当您在这些来源中取得进展时，就会变得不那么令人畏惧）。如果可能的话，将给出多种资源和媒体格式的指示，并对其相对优点进行一些讨论。

## Organization 组织

This document is organized into several sections and chapters, as listed below and in the sidebar. You are encouraged to jump around to whichever parts seem most useful for your personal learning goals. Overall, I’d recommend first skimming many of the linked resources rather than reading (or watching) word-for-word. This should hopefully at least give you a sense of where your knowledge gaps are in terms of dependencies for any particular learning goals, which will help guide a more focused second pass.

本文档分为几个部分和章节，如下和侧栏中所列。我们鼓励您跳到对您的个人学习目标最有用的部分。总的来说，我建议首先浏览许多链接的资源，而不是逐字阅读（或观看）。希望这至少能让您了解在任何特定学习目标的依赖性方面您的知识差距在哪里，这将有助于指导更有针对性的第二遍。

# Section I: Foundations of Sequential Prediction 第一节：序列预测的基础

> **Goal:** Recap machine learning basics + survey (non-DL) methods for tasks under the umbrella of “sequential prediction”.

> 目标：回顾“序列预测”下任务的机器学习基础知识 + 调查（非深度学习）方法。

>

> Our focus in this section will be on quickly overviewing classical topics in statistical prediction and reinforcement learning, which we’ll make direct reference to in later sections, as well as highlighting some topics that I think are very useful as conceptual models for understanding LLMs, yet which are often omitted from deep learning crash courses – notably time-series analysis, regret minimization, and Markov models.

> 本节的重点将是快速概述统计预测和强化学习中的经典主题，我们将在后面的部分中直接参考这些主题，并强调一些我认为作为理解概念模型非常有用的主题<b1001 ></b1001>，但深度学习速成课程中经常忽略这些内容，特别是时间序列分析、遗憾最小化和马尔可夫模型。

## Chapter 1: Preliminaries 第一章：预备知识

### Math 数学

Calculus and linear algebra are pretty much unavoidable if you want to understand modern deep learning, which is largely driven by matrix multiplication and backpropagation of gradients. Many technical people end their formal math educations around multivariable calculus or introductory linear algebra, and it seems common to be left with a sour taste in your mouth from having to memorize a suite of unintuitive identities or manually invert matrices, which can be discouraging towards the prospect of going deeper in one’s math education. Fortunately, we don’t need to do these calculations ourselves — programming libraries will handle them for us — and it’ll instead be more important to have a working knowledge of concepts such as:

如果你想了解现代深度学习，微积分和线性代数几乎是不可避免的，现代深度学习很大程度上是由矩阵乘法和梯度反向传播驱动的。许多技术人员围绕多元微积分或入门线性代数结束了他们的正式数学教育，并且由于必须记住一套不直观的恒等式或手动求逆矩阵而感到口酸，这似乎很常见，这可能会阻碍你的学习。进一步深入数学教育的前景。幸运的是，我们不需要自己进行这些计算——编程库将为我们处理它们——相反，掌握以下概念的实用知识会更重要：

+   Gradients and their relation to local minima/maxima
梯度及其与局部最小值/最大值的关系

+   The chain rule for differentiation
微分的链式法则

+   Matrices as linear transformations for vectors
矩阵作为向量的线性变换

+   Notions of basis/rank/span/independence/etc.
基础/等级/跨度/独立性/等等的概念。

Goo[d visualiza](https://www.youtube.com/@3blue1brown/playlists)tions can really help these ideas sink in, and I don’t think there’s a better source for this than these two YouTube series from 3Blue1Brown:

良好的可视化确实可以帮助这些想法深入人心，我认为没有比 3Blue1Brown 的这两个 YouTube 系列更好的资源了：

+   Essence of calculus  微积分的本质

+   [Essenc](https://www.youtube.com/watch?v=fNk_zzaMoSs&list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab&pp=iAQB)e of linear algebra
线性代数本质

If y[our math is rus](https://www.youtube.com/watch?v=aircAruvnKk&list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi)ty, I’d certainly encourage (re)watching these before diving in deeper. To test your understanding, or as a preview of where we’re headed, the shorter Neural networks video series on the same channel is excellent as well, and the latest couple videos in the series give a great overview of Transformer networks for language modeling.

如果你的数学很生疏，我当然鼓励你在深入研究之前（重新）观看这些内容。为了测试您的理解程度，或者作为我们前进方向的预览，同一频道上的较短的神经网络视频系列也非常好，该系列中的最新几个视频对用于语言建模的 Transformer 网络进行了很好的概述。

These [lecture notes](https://links.uwaterloo.ca/math227docs/set4.pdf) from Waterloo give some useful coverage of multivariable calculus as it relates to optimization, and “Linear Algebra Done Right” by Sheldon Axler is a nice reference text for linear algebra. “Convex Optimization” by Boyd and Vandenberghe shows how these topics lay the foundations for the kinds of optimization problems faced in machine learning, but note that it does get fairly technical, and may not be essential if you’re mostly interested in applications.

滑铁卢的这些讲义对与优化相关的多变量微积分进行了一些有用的介绍，谢尔顿·阿克斯勒 (Sheldon Axler) 的《线性代数正确》是线性代数的一本很好的参考书。 Boyd 和 Vandenberghe 的“凸优化”展示了这些主题如何为机器学习中面临的各种优化问题奠定基础，但请注意，它确实具有相当的技术性，如果您最感兴趣的是应用程序，则可能不是必需的。

Linear programming is certainly worth understanding, and is basically the simplest kind of h[igh-d](https://www.youtube.com/watch?v=E72DWgKP_1Y)i[mensio](https://www.youtube.com/watch?v=DYAIIUuAaGA&list=PLm3J0oaFux3ZYpFLwwrlv_EHH9wtH6pnX&index=66)nal[ op](https://www.youtube.com/watch?v=v-chgwlwqTk)ti[miz](https://www.youtube.com/watch?v=IWzErYm8Lyk)ati[on pr](https://genai-handbook.github.io/#online-learning-and-regret-minimization)oblem you’ll encounter [(but](https://genai-handbook.github.io/#generative-adversarial-nets) still quite practical); this illustrated video should give you most of the core ideas, and Ryan O’Donnell’s videos (17a-19c in this series, depending on how deep you want to go) are excellent if you want to go deeper into the math. These lectures (#10, #11) from Tim Roughgarden also show some fascinating connections between linear programming and the “online learning” methods we’ll look at later, which will form the conceptual basis for GANs (among many other things).

线性规划当然值得理解，并且基本上是您将遇到的最简单的高维优化问题（但仍然非常实用）；这个插图视频应该为您提供大部分核心思想，如果您想更深入地了解数学，Ryan O’Donnell 的视频（本系列中的 17a-19c，取决于您想要深入的程度）非常好。 Tim Roughgarden 的这些讲座（#10、#11）还展示了线性编程和我们稍后将讨论的“在线学习”方法之间的一些有趣的联系，这将构成 GAN（以及许多其他事物）的概念基础。

### Programming 编程

Most machine learning code is written in Python nowadays, and some of the references here will include Python examples for illustrating the discussed topics. If you’re unfamiliar with Python, or programming in general, I’ve heard good things about Replit’s 100 Days of Python course for getting started. Some systems-level topics will also touch on implementations in C++ or CUDA — I’m admittedly not much of an expert in either of these, and will focus more on higher-level abstractions which can be accessed through Python libraries, but I’ll include potentially useful references for these languages in the relevant sections nonetheless.

如今，大多数机器学习代码都是用 Python 编写的，这里的一些参考文献将包括用于说明所讨论主题的 Python 示例。如果您不熟悉 Python 或一般编程，我听说过 Replit 的 100 天 Python 入门课程。一些系统级主题还将涉及 C++ 或 CUDA 中的实现 - 诚然，我在这方面都不是专家，并且将更多地关注可以通过 Python 库访问的更高级别的抽象，但我会尽管如此，在相关部分中仍包含这些语言的潜在有用参考。

## Chapter 2: Statistical Prediction and Supervised Learning 第 2 章：统计预测和监督学习

Before getting to deep learning and large language models, it’ll be useful to have a solid grasp on some foundational concepts in probability theory and machine learning. In particular, it helps to understand:

在进入深度学习和大型语言模型之前，扎实掌握概率论和机器学习中的一些基本概念将很有用。特别是，它有助于理解：

+   Random variables, expectations, and variance
随机变量、期望和方差

+   Supervised vs. unsupervised learning
监督学习与无监督学习

+   Regression vs. classification
回归与分类

+   Linear models and regularization
线性模型和正则化

+   Empirical risk minimization
经验风险最小化

+   Hypothesis classes and bias-variance tradeoffs
假设类和偏差-方差权衡

For gen[eral ](https://www.youtube.com/watch?v=zeJD6dqJ5lo)pro[bability the](https://blogs.ubc.ca/math105/discrete-random-variables/)ory,[ havi](https://www.youtube.com/watch?v=r9S2fMQiP2E&list=PLm3J0oaFux3ZYpFLwwrlv_EHH9wtH6pnX&index=13)ng a solid understanding of how the Central Limit Theorem works is perhaps a reasonable litmus test for how much you’ll need to know about random variables before tackling some of the later topics we’ll cover. This beautifully-animated 3Blue1Brown video is a great starting point, and there are a couple other good probability videos to check out on the channel if you’d like. This set of course notes from UBC covers the basics of random variables. If you’re into blackboard lectures, I’m a big fan of many of Ryan O’Donnell’s CMU courses on YouTube, and this video on random variables and the Central Limit Theorem (from the excellent “CS Theory Toolkit” course) is a nice overview.

对于一般概率论来说，深入了解中心极限定理的工作原理也许是一个合理的试金石，可以检验您在处理我们将要介绍的一些后续主题之前需要了解多少随机变量。这个动画精美的 3Blue1Brown 视频是一个很好的起点，如果您愿意，还有其他几个不错的概率视频可以在该频道上查看。 UBC 的这套课程笔记涵盖了随机变量的基础知识。如果您喜欢黑板讲座，我是 YouTube 上 Ryan O'Donnell 的许多 CMU 课程的忠实粉丝，这段关于随机变量和中心极限定理的视频（来自优秀的“CS 理论工具包”课程）是很好的概述。

For understanding linear models and other key machine learning principles, the first two chapters of Hastie’s Elements of Statistical Learning (”Introduction” and “Overview of Supervised Learning”) should be enough to get started.

为了理解线性模型和其他关键的机器学习原理，Hastie 的《统计学习要素》的前两章（“简介”和“监督学习概述”）应该足以开始。

Once you’re familiar wi[th the ba](https://ryxcommar.com/2019/09/06/some-things-you-maybe-didnt-know-about-linear-regression/)[sics, this](https://twitter.com/ryxcommar) b[log post ](https://www.youtube.com/@statquest/playlists)by anonymous Twitter/X user @ryxcommar does a nice job discussing some common pitfalls and misconceptions related to linear regression. StatQuest on YouTube has a number of videos that might also be helpful.

一旦您熟悉了基础知识，匿名 Twitter/X 用户 @ryxcommar 的这篇博文就很好地讨论了与线性回归相关的一些常见陷阱和误解。 YouTube 上的 StatQuest 有许多可能也有帮助的视频。

Introductions to machine learning tend to emphasize linear models, and for good reason. Many phenomena in the real world are modeled quite well by linear equations — the average temperature over past 7 days is likely a solid guess for the temperature tomorrow, barring any other information about weather pattern forecasts. Linear systems and models are a lot easier to study, interpret, and optimize than their nonlinear counterparts. For more complex and high-dimensional problems with potential nonlinear dependencies between features, it’s often useful to ask:

机器学习的介绍倾向于强调线性模型，这是有充分理由的。现实世界中的许多现象都可以通过线性方程很好地建模 - 过去 7 天的平均温度很可能是明天温度的可靠猜测，除非有关天气模式预测的任何其他信息。线性系统和模型比非线性系统和模型更容易研究、解释和优化。对于特征之间具有潜在非线性依赖性的更复杂和高维问题，询问以下问题通常很有用：

+   What’s a linear model for the problem?
该问题的线性模型是什么？

+   Why does the linear model fail?
为什么线性模型会失败？

+   What’s the best way to add nonlinearity, given the semantic structure of the problem?
考虑到问题的语义结构，添加非线性的最佳方法是什么？

In particular, this framing will be helpful for motivating some of the model architectures we’ll look at later (e.g. LSTMs and Transformers).

特别是，这个框架将有助于激发我们稍后将讨论的一些模型架构（例如 LSTM 和 Transformer）。

## Chapter 3: Time-Series Analysis 第 3 章：时间序列分析

How much do you need to know about time-series analysis in order to understand the mechanics of more complex generative AI methods?

为了理解更复杂的生成人工智能方法的机制，您需要了解多少关于时间序列分析的知识？

**Short answer:** just a tiny bit for LLMs, a good bit more for diffusion.

简短的回答：LLMs 只需一点点，多一点用于扩散。

For modern Transformer-based LLMs, it’ll be useful to know:

对于现代基于 Transformer 的 LLMs，了解以下内容会很有用：

+   The basic setup for sequential prediction problems
顺序预测问题的基本设置

+   The notion of an autoregressive model
自回归模型的概念

There’s not really a coherent way to “visualize” the full mechanics of a multi-billion-parameter model in your head, but much simpler autoregressive models (like ARIMA) can serve as a nice mental model to extrapolate from.

确实没有一种连贯的方法可以在您的头脑中“可视化”数十亿参数模型的完整机制，但更简单的自回归模型（如 ARIMA）可以作为一个很好的心理模型进行推断。

When[ we get t](https://genai-handbook.github.io/#diffusion-models)o neural state-space models, a working knowledge of linear time-invariant systems and control theory (which have many connections to classical time-series analysis) will be helpful for intuition, but diffusion is really where it’s most essential to dive deeper into into stochastic differential equations to get the full picture. But we can table that for now.

当我们接触神经状态空间模型时，线性时不变系统和控制理论（与经典时间序列分析有很多联系）的实用知识将有助于直觉，但扩散确实是最重要的地方更深入地研究随机微分方程以获得全貌。但我们现在可以把这个问题搁置起来。

This blog post (Forecasting with Stochastic Models) from Towards Data Science is concise and introduces the basic concepts along with some standard autoregressive models and code examples.

这篇来自《走向数据科学》的博客文章（使用随机模型进行预测）非常简洁，介绍了基本概念以及一些标准自回归模型和代码示例。

This set of [notes](https://sites.ualberta.ca/~kashlak/data/stat479.pdf) from UAlberta’s “Time Series Analysis” course is nice if you want to go a bit deeper on the math.

如果您想更深入地了解数学，那么来自阿尔伯塔大学“时间序列分析”课程的这套笔记非常有用。

## Chapter 4: Online Learning and Regret Minimization 第 4 章：在线学习和遗憾最小化

It’s debatable how important it is to have a strong grasp on regret minimization, but I think a basic familiarity is useful. The basic setting here is similar to supervised learning, but:

牢牢掌握后悔最小化的重要性还有待商榷，但我认为基本的熟悉是有用的。这里的基本设置与监督学习类似，但是：

+   Points arrive one-at-a-time in an arbitrary order
积分以任意顺序一次到达一个

+   We want low average error across this sequence
我们希望这个序列的平均误差较低

If you squint and tilt your head, most of the algorithms designed for these problems look basically like gradient descent, often with delicate choices of regularizers and learning rates require for the math to work out. But there’s a lot of satisfying math here. I have a soft spot for it, as it relates to a lot of the research I worked on during my PhD. I think it’s conceptually fascinating. Like the previous section on time-series analysis, online learning is technically “sequential prediction” but you don’t really need it to understand LLMs.

如果你眯着眼睛歪着头，大多数为这些问题设计的算法看起来基本上都像梯度下降，通常需要对正则化器和学习率进行微妙的选择，以便数学计算出来。但这里有很多令人满意的数学。我对它情有独钟，因为它与我在博士期间所做的许多研究有关。我认为它在概念上很有趣。与上一节有关时间序列分析的内容一样，在线学习在技术上是“顺序预测”，但您并不真正需要它来理解LLMs。

The most direct connection to it[ tha](https://genai-handbook.github.io/#generative-adversarial-nets)t we’ll consider is when we look at GANs in Section VIII. There are many deep connections between regret minimization and equilibria in games, and GANs work basically by having two neural networks play a game against each other. Practical gradient-based optimization algorithms like Adam have their roots in this field as well, following the introduction of the AdaGrad algorithm, which was first analyzed for online and adversarial settings. In terms of other insights, one takeaway I find useful is the following: If you’re doing gradient-based optimization with a sensible learning rate schedule, then the order in which you process data points doesn’t actually matter much. Gradient descent can handle it.

我们要考虑的最直接的联系是当我们在第八节中查看 GAN 时。游戏中的遗憾最小化和均衡之间存在许多深刻的联系，GAN 的工作原理基本上是让两个神经网络互相玩游戏。在引入 AdaGrad 算法之后，像 Adam 这样的实用基于梯度的优化算法也起源于该领域，该算法首先针对在线和对抗设置进行了分析。就其他见解而言，我发现有用的一个要点如下：如果您使用合理的学习率计划进行基于梯度的优化，那么处理数据点的顺序实际上并不重要。梯度下降可以处理它。

I’d encourage you to at leas[t skim Chapter 1 of “Int](https://arxiv.org/pdf/1909.05207.pdf)roduction to Online Convex Optimization” by Elad Hazan to get a feel for the goal of regret minimization. I’ve spent a lot of time with this book and I think it’s excellent.

我鼓励您至少浏览一下 Elad Hazan 所著的《在线凸优化简介》的第一章，以了解遗憾最小化的目标。我在这本书上花了很多时间，我认为它非常棒。

## Chapter 5: Reinforcement Learning 第 5 章：强化学习

Re[inforcemen](https://genai-handbook.github.io/#finetuning-methods-for-llms)t[ Learning (R](https://genai-handbook.github.io/#tool-use-and-agents)L) will come up most directly when we look at finetuning methods in Section IV, and may also be a useful mental model for thinking about “agent” applications and some of the “control theory” notions which come up for state-space models. Like a lot of the topics discussed in this document, you can go quite deep down many different RL-related threads if you’d like; as it relates to language modeling and alignment, it’ll be most important to be comfortable with the basic problem setup for Markov decision processes, notion of policies and trajectories, and high-level understanding of standard iterative + gradient-based optimization methods for RL.

当我们在第四节中查看微调方法时，强化学习（RL）将最直接地出现，并且也可能是思考“代理”应用程序和状态中出现的一些“控制理论”概念的有用心智模型。空间模型。与本文档中讨论的许多主题一样，如果您愿意，可以深入研究许多不同的 RL 相关主题；因为它涉及语言建模和对齐，所以最重要的是熟悉马尔可夫决策过程的基本问题设置、策略和轨迹的概念，以及对 RL 标准迭代 + 基于梯度的优化方法的高级理解。

This [blog post](https://lilianweng.github.io/posts/2018-02-19-rl-overview/) from Lilian Weng is a great starting point, and is quite dense with important RL ideas despite its relative conciseness. It also touches on connections to AlphaGo and gameplay, which you might find interesting as well.

Lilian Weng 的这篇博文是一个很好的起点，尽管相对简洁，但其中包含了重要的 RL 思想。它还涉及与 AlphaGo 和游戏玩法的联系，您可能也会觉得有趣。

The textbook [“Reinforcement Learni](https://web.stanford.edu/class/psych209/Readings/SuttonBartoIPRLBook2ndEd.pdf)ng: An Introduction” by Sutton and Barto is generally considered the classic reference text for the area, at least for “non-deep” methods. This was my primary guide when I was first learning about RL, and it gives a more in-depth exploration of many of the topics touched on in Lilian’s blog post.

Sutton 和 Barto 的教科书《强化学习：简介》通常被认为是该领域的经典参考书，至少对于“非深度”方法而言是如此。这是我第一次学习 RL 时的主要指南，它对 Lilian 博客文章中涉及的许多主题进行了更深入的探索。

If[ you want](https://karpathy.github.io/2016/05/31/rl/)[ to jump a](https://arxiv.org/pdf/1810.06339)[head to ](https://arxiv.org/pdf/2201.02135)some more neural-flavored content, Andrej Karpathy has a nice blog post on deep RL; this manuscript by Yuxi Li and this textbook by Aske Plaat may be useful for further deep dives.

如果您想跳到一些更多神经风格的内容，Andrej Karpathy 有一篇关于深度 RL 的精彩博客文章； Yuxi Li 的这份手稿和 Aske Plaat 的这本教科书可能有助于进一步深入研究。

I[f you like 3Blue](https://www.youtube.com/playlist?list=PLzvYlJMoZ02Dxtwe-MmH4nOB5jYlMGBjr)1Brown-style animated videos, the series “Reinforcement Learning By the Book” is a great alternative option, and conveys a lot of content from Sutton and Barto, along with some deep RL, using engaging visualizations.

如果您喜欢 3Blue1Brown 风格的动画视频，“按书强化学习”系列是一个很好的选择，它通过引人入胜的可视化传达了 Sutton 和 Barto 的大量内容以及一些深度强化学习。

## Chapter 6: Markov Models 第 6 章：马尔可夫模型

Running a fixed policy in a Markov decision process yields a Markov chain; processes resembling this kind of setup are fairly abu[ndant, an](https://thagomizer.com/blog/2017/11/07/markov-models.html)d [many](https://ericmjl.github.io/essays-on-data-science/machine-learning/markov-models/) branches of machine learning involve modeling systems under Markovian assumptions (i.e. lack of path-dependence, given the current state). This blog post from Aja Hammerly makes a nice case for thinking about language models via Markov processes, and this post from “Essays on Data Science” has examples and code building up towards auto-regressive Hidden Markov Models, which will start to vaguely resemble some of the neural network architectures we’ll look at later on.

在马尔可夫决策过程中运行固定策略会产生马尔可夫链；类似这种设置的过程相当丰富，并且机器学习的许多分支都涉及马尔可夫假设下的建模系统（即，在给定当前状态的情况下，缺乏路径依赖性）。 Aja Hammerly 的这篇博客文章为通过马尔可夫过程思考语言模型提供了一个很好的案例，这篇来自“数据科学论文”的文章提供了构建自回归隐马尔可夫模型的示例和代码，这将开始隐约类似于一些我们稍后将讨论神经网络架构。

This [blog post](https://www.tweag.io/blog/2019-10-25-mcmc-intro1/) from Simeon Carstens gives a nice coverage of Markov chain Monte Carlo methods, which are powerful and widely-used techniques for sampling from implicitly-represented distributions, and are helpful for thinking about probabilistic topics ranging from stochastic gradient descent to diffusion.

Simeon Carstens 的这篇博文很好地介绍了马尔可夫链蒙特卡罗方法，这是一种强大且广泛使用的从隐式表示的分布中进行采样的技术，有助于思考从随机梯度下降到扩散等概率主题。

M[arkov mo](https://mlg.eng.cam.ac.uk/zoubin/papers/ijprai.pdf)d[els are also at the he](https://www.microsoft.com/en-us/research/uploads/prod/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf)art of many Bayesian methods. See this tutorial f[rom Zou](https://www.deeplearningbook.org/contents/graphical_models.html)bin Ghahramani for a nice overview, the textbook “Pattern Recognition and Machine Learning” for Bayesian angles on many machine learning topics (as well as a more-involved HMM presentation), and this chapter of the Goodfellow et al. “Deep Learning” textbook for some connections to deep learning.

马尔可夫模型也是许多贝叶斯方法的核心。请参阅 Zoubin Ghahramani 的本教程以获取很好的概述、有关许多机器学习主题的贝叶斯角度的教科书“模式识别和机器学习”（以及更多涉及的 HMM 演示文稿）以及 Goodfellow 等人的本章。 “深度学习”教科书，介绍与深度学习的一些联系。

# Section II: Neural Sequential Prediction 第二节：神经序列预测

> **Goal:** Survey deep learning methods + applications to sequential and language modeling, up to basic Transformers.

> 目标：调查深度学习方法以及顺序和语言建模的应用，直至基本的 Transformer。

>

> Here, more than any other section of the handbook, I’ll defer mostly wholesale to existing sequences of instructional resources. This material has been covered quite well by many people in a variety of formats, and there’s no need to reinvent the wheel.

> 在这里，与手册的任何其他部分相比，我将主要遵循现有的教学资源序列。这种材料已经被很多人以各种格式很好地介绍过了，没有必要重新发明轮子。

> There are a couple different routes you can take from the basics of neural networks towards Transformers (the dominant architecture for most frontier LLMs in 2024). Once we cover the basics, I’ll mostly focus on “deep sequence learning” methods like RNNs. Many deep learning books and courses will more heavily emphasize convolutional neural nets (CNNs), which are quite important for image-related applications and historically were one of the first areas where “scaling” was particularly successful, but technically they’re fairly disconnected from Transformers. They’ll make an appearance when we discuss state-space models and are definitely important for vision applications, but you’ll mostly be okay skipping them for now. However, if you’re in a rush and just want to get to the new stuff, you could consider diving right into decoder-only Transformers once you’re comfortable with feed-forward neural nets — this the approach taken by the excellent “Let’s build GPT” video from Andrej Karpathy, casting them as an extension of neural n-gram models for next-token prediction. That’s probably your single best bet for speedrunning Transformers in under 2 hours. But if you’ve got a little more time, understanding the history of RNNs, LSTMs, and encoder-decoder Transformers is certainly worthwhile.

> 从神经网络基础知识到 Transformer（2024 年大多数前沿 LLMs 的主导架构），您可以采取几种不同的路线。一旦我们介绍了基础知识，我将主要关注 RNN 等“深度序列学习”方法。许多深度学习书籍和课程都会更加强调卷积神经网络（CNN），它对于图像相关应用非常重要，并且历史上是“缩放”特别成功的首批领域之一，但从技术上讲，它们与图像相关的应用相当脱节。变形金刚。当我们讨论状态空间模型时，它们会出现，并且对于视觉应用来说绝对重要，但现在你基本上可以跳过它们。然而，如果你很着急，只是想了解新东西，那么一旦你熟悉了前馈神经网络，你可以考虑直接进入仅解码器的 Transformer——这就是优秀的“让我们来吧”所采用的方法。来自 Andrej Karpathy 的“构建 GPT”视频，将其作为用于下一个标记预测的神经 n 元模型的扩展。这可能是您在 2 小时内快速通关《变形金刚》的最佳选择。但如果您有更多时间，了解 RNN、LSTM 和编码器-解码器 Transformer 的历史当然是值得的。

> This section is mostly composed of signposts to content from the following sources (along with some blog posts):

> 本节主要由以下来源的内容组成（以及一些博客文章）：

+   The “Dive Into Deep Learning” (d2l.ai) interactive textbook (nice graphics, in-line code, some theory)
“深入深度学习”（d2l.ai）互动教科书（精美的图形、内联代码、一些理论）

+   3Blue1Brown’s “Neural networks” video series (lots of animations)
3Blue1Brown 的“神经网络”视频系列（大量动画）

+   [Andrej Karpath](https://www.youtube.com/watch?v=VMj-3S1tku0&list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ)y’s “Zero to Hero” video series (live coding + great intuitions)
Andrej Karpathy 的“从零到英雄”视频系列（实时编码 + 伟大的直觉）

+   “StatQuest with Josh Starmer” videos
“StatQuest 与 Josh Starmer”视频

+   [The Goodfellow ](https://www.deeplearningbook.org/)et al. “Deep Learning” textbook (theory-focused, no Transformers)
古德费罗等人。 《深度学习》教科书（以理论为主，无变形金刚）

> I[f your focus is on applicati](https://github.com/rasbt/machine-learning-book/tree/main)ons, you might find the interactive “Machine Learning with PyTorch and Scikit-Learn” book useful, but I’m not as familiar with it personally.

> 如果您的重点是应用程序，您可能会发现交互式“使用 PyTorch 和 Scikit-Learn 进行机器学习”一书很有用，但我个人对此并不熟悉。

> For these topics, you can also probably get away with asking conceptual questions to your preferred LLM chat interface. This likely won’t be true for later sections — some of those topics were introduced after the knowledge cutoff dates for many current LLMs, and there’s also just a lot less text on the internet about them, so you end up with more “hallucinations”.

> 对于这些主题，您也可以通过向您喜欢的 LLM 聊天界面询问概念性问题。对于后面的部分来说，情况可能并非如此 - 其中一些主题是在许多当前 LLMs 的知识截止日期之后引入的，而且互联网上关于它们的文本也少得多，所以你结束产生更多的“幻觉”。

## Chapter 7: Statistical Prediction with Neural Networks 第 7 章：神经网络统计预测

I’m not actually sure where I first learned about neural nets — they’re pervasive enough in technical discussions and general online media that I’d assume you’ve picked up a good bit through osmosis even if you haven’t studied them formally. Nonetheless, there are many worthwhile explainers out there, and I’ll highlight some of my favorites.

我实际上不确定我是在哪里第一次了解神经网络的——它们在技术讨论和一般在线媒体中足够普遍，我假设你已经通过渗透学到了很多东西，即使你还没有正式研究它们。尽管如此，还是有很多有价值的解释者，我将重点介绍一些我最喜欢的解释者。

+   The first 4 videos in 3Blue1Brown’s “Neural networks” series will take you from basic definitions up through the mechanics of backpropagation.
3Blue1Brown 的“神经网络”系列的前 4 个视频将带您从基本定义开始了解反向传播的机制。

+   This [blog post](https://karpathy.github.io/neuralnets/) f[rom A](https://www.youtube.com/watch?v=VMj-3S1tku0)ndrej Karpathy (back when he was a PhD student) is a solid crash-course, well-accompanied by his video on building backprop from scratch.
Andrej Karpathy（当时他还是一名博士生）的这篇博客文章是一门扎实的速成课程，并配有他从头开始构建反向传播的视频。

+   This [blog post](https://colah.github.io/posts/2015-08-Backprop/) from Chris Olah has a nice and concise walk-through of the math behind backprop for neural nets.
Chris Olah 的这篇博文对神经网络反向传播背后的数学进行了精彩而简洁的演练。

+   [Chapte](http://d2l.ai/)rs 3-5 of the d2l.ai book are great as a “classic textbook” presentation of deep nets for regression + classification, with code examples and visualizations throughout.
d2l.ai 书的第 3-5 章非常适合作为回归 + 分类的深度网络的“经典教科书”演示，贯穿全文的代码示例和可视化。

## Chapter 8: Recurrent Neural Networks 第 8 章：循环神经网络

RNNs are where we start adding “state” to our models (as we process increasingly long [sequences), and](https://karpathy.github.io/2015/05/21/rnn-effectiveness/)[ there ar](https://d2l.ai/chapter_recurrent-neural-networks/index.html)e some h[igh-le](http://d2l.ai/)vel similarities to hidden Mar[kov models](https://www.deeplearningbook.org/contents/rnn.html). This blog post from Andrej Karpathy is a good starting point. Chapter 9 of the d2l.ai book is great for main ideas and code; check out Chapter 10 of “Deep Learning” if you want more theory.

RNN 是我们开始向模型添加“状态”的地方（当我们处理越来越长的序列时），并且与隐马尔可夫模型有一些高级相似之处。 Andrej Karpathy 的这篇博文是一个很好的起点。 d2l.ai 书的第 9 章非常适合主要思想和代码；如果您想要更多理论，请查看“深度学习”的第 10 章。

For videos, [here](https://www.youtube.com/watch?v=AsNTP8Kwu80)’s a nice one from StatQuest.

对于视频，这是 StatQuest 提供的一个不错的视频。

## Chapter 9: LSTMs and GRUs 第 9 章：LSTM 和 GRU

Long Short-Term Memory (LSTM) networks and Gated Recurrent Unit (GRU) networks build upon RNNs with more specialized mechanisms for state representation (with semantic inspirations like “memory”, “forgetting”, and “resetting”), which have been useful for improving performance in more challenging data domains (like language).

长短期记忆 (LSTM) 网络和门控循环单元 (GRU) 网络建立在 RNN 的基础上，具有更专门的状态表示机制（受到“记忆”、“遗忘”和“重置”等语义启发），这些机制非常有用用于提高更具挑战性的数据领域（如语言）的性能。

[Chapter 10](https://d2l.ai/chapter_recurrent-modern/index.html) of [d2l.ai](http://d2l.ai/) covers both of these quite well[ (up ](https://www.youtube.com/watch?v=8HyCNIVRbSU)th[rough](https://www.youtube.com/watch?v=YCzL96nL7j0) 10.3). The “Understanding LSTM Networks” blog post from Chris Olah is also excellent. This video from “The AI Hacker” gives solid high-level coverage of both; StatQuest also has a video on LSTMs, but not GRUs. GRUs are essentially a simplified alternative to LSTMs with the same basic objective, and it’s up to you if you want to cover them specifically.

d2l.ai 第 10 章很好地涵盖了这两个问题（直到 10.3）。 Chris Olah 的“Understanding LSTM Networks”博客文章也非常出色。来自“人工智能黑客”的这段视频对两者进行了扎实的高层报道； StatQuest 也有关于 LSTM 的视频，但没有关于 GRU 的视频。 GRU 本质上是 LSTM 的简化替代品，具有相同的基本目标，如果您想具体介绍它们，则由您决定。

Neither LSTMs or GRUs are really prerequisites for Transformers, which are “stateless”, but they’re useful for understanding the general challenges neural sequ[ence of neu](https://genai-handbook.github.io/#s7)ral sequence and contextualizing the Transformer design choices. They’ll also help motivate some of the approaches towards addressing the “quadratic scaling problem” in Section VII.

LSTM 或 GRU 都不是 Transformer 的真正先决条件，它们是“无状态的”，但它们对于理解神经序列的一般挑战以及将 Transformer 设计选择情境化很有用。它们还将有助于激发一些方法来解决第七节中的“二次缩放问题”。

## Chapter 10: Embeddings and Topic Modeling 第 10 章：嵌入和主题建模

[Before di](https://towardsdatascience.com/latent-dirichlet-allocation-lda-9d1cd064ffa2)gestin[g Tran](https://jaketae.github.io/study/tf-idf/)sformers, it’s worth first establishing a couple concepts which will be useful for reasoning about what’s going on under the hood inside large language models. While deep learning has led to a large wave of progress in NLP, it’s definitely a bit harder to reason about than some of the “old school” methods which deal with word frequencies and n-gram overlaps; however, even though these methods don’t always scale to more complex tasks, they’re useful mental models for the kinds of “features” that neural nets might be learning. For example, it’s certainly worth knowing about Latent Dirichlet Allocation for topic modeling (blog post) and tf-idf to get a feel for what numerical similarity or relevance scores can represent for language.

在消化 Transformer 之前，值得首先建立几个概念，这些概念对于推理大型语言模型内部发生的情况很有用。虽然深度学习在 NLP 领域带来了一大波进步，但它肯定比一些处理词频和 n-gram 重叠的“老派”方法更难推理；然而，尽管这些方法并不总是能够扩展到更复杂的任务，但它们对于神经网络可能正在学习的各种“特征”来说是有用的心理模型。例如，当然值得了解主题建模的潜在狄利克雷分配（博客文章）和 tf-idf，以了解数字相似性或相关性分数可以代表语言。

Thinkin[g about word](https://web.stanford.edu/class/cs224n/readings/cs224n_winter2023_lecture1_notes_draft.pdf)s[ (or ](https://www.youtube.com/watch?v=f7o8aDNxf7k)tokens) as high-dimensional “m[eanin](https://www.youtube.com/watch?v=gQddtTdmG_8)g” vectors is quite useful, and the Word2Vec embedding method illustrates this quite well — you may have seen the classic “King - Man + Woman = Queen” example referenced before. “The Illustrated Word2Vec” from Jay Alammar is great for building up this intuition, and these course notes from Stanford’s CS224n are excellent as well. Here’s also a nice video on Word2Vec from ritvikmath, and another fun one video on neural word embeddings from Computerphile.

将单词（或标记）视为高维“含义”向量非常有用，Word2Vec 嵌入方法很好地说明了这一点 - 您可能已经看过之前引用的经典“King - Man + Woman = Queen”示例。 Jay Alammar 的《Word2Vec 插图》非常适合建立这种直觉，斯坦福大学 CS224n 的这些课程笔记也非常出色。这里还有一个来自 ritvikmath 的关于 Word2Vec 的精彩视频，以及另一个来自 Computerphile 的关于神经词嵌入的有趣视频。

Beyond b[ein](https://genai-handbook.github.io/#retrieval-augmented-generation)g a useful i[ntuition a](https://docs.cohere.com/docs/text-embeddings)nd an element of larger [language ](https://lilianweng.github.io/posts/2021-05-31-contrastive/)models, standalone neural embedding models are also widely used today. Often these are encoder-only Transformers, trained via “contrastive loss” to construct high-quality vector representations of text inputs which are useful for retrieval tasks (like RAG). See this post+video from Cohere for a brief overview, and this blog post from Lilian Weng for more of a deep dive.

除了作为一种有用的直觉和更大的语言模型的元素之外，独立的神经嵌入模型如今也被广泛使用。通常，这些是仅编码器的 Transformer，通过“对比损失”进行训练，以构建文本输入的高质量向量表示，这对于检索任务（如 RAG）很有用。请参阅 Cohere 的这篇文章+视频以获取简要概述，以及 Lilian Weng 的这篇博客文章以了解更多深入内容。

## Chapter 11: Encoders and Decoders 第 11 章：编码器和解码器

Up until now we’ve been pretty agnostic as to what the inputs to our networks are — numbers, characters, words — a[s long as ](https://d2l.ai/chapter_recurrent-modern/index.html)it c[an be ](http://d2l.ai/)converted to a vector representation somehow. Recurrent [models can](https://d2l.ai/chapter_attention-mechanisms-and-transformers/index.html) be configured to both input and outp[ut ei](https://web.stanford.edu/class/cs224n/readings/cs224n-self-attention-transformers-2023_draft.pdf)ther a single object (e.g. a vector) or an entire sequence. This observation enables the sequence-to-sequence encoder-decoder architecture, which rose to prominence for machine translation, and was the original design for the Transformer in the famed “Attention is All You Need” paper. Here, the goal is to take an input sequence (e.g. an English sentence), “encode” it into a vector object which captures its “meaning”, and then “decode” that object into another sequence (e.g. a French sentence). Chapter 10 in d2l.ai (10.6-10.8) covers this setup as well, which sets the stage for the encoder-decoder formulation of Transformers in Chapter 11 (up through 11.7). For historical purposes you should certainly at least skim the original paper, though you might get a bit more out of the presentation of its contents via “The Annotated Transformer”, or perhaps “The Illustrated Transformer” if you want more visualizations. These notes from Stanford’s CS224n are great as well.

到目前为止，我们对网络的输入是什么（数字、字符、单词）相当不可知，只要它可以以某种方式转换为向量表示即可。循环模型可以配置为输入和输出单个对象（例如向量）或整个序列。这一观察结果使得序列到序列的编码器-解码器架构成为可能，该架构在机器翻译领域崭露头角，并且是著名的“Attention is All You Need”论文中 Transformer 的原始设计。在这里，目标是获取输入序列（例如英语句子），将其“编码”为捕获其“含义”的向量对象，然后将该对象“解码”为另一个序列（例如法语句子）。 d2l.ai (10.6-10.8) 中的第 10 章也涵盖了此设置，这为第 11 章（直到 11.7）中 Transformer 的编码器-解码器公式奠定了基础。出于历史目的，您当然至少应该浏览一下原始论文，尽管您可能会通过“带注释的变压器”或“插图变压器”（如果您想要更多可视化）从其内容的呈现中获得更多信息。斯坦福大学 CS224n 的这些笔记也很棒。

[There are video](https://www.youtube.com/watch?v=L8HKweZIOmg)[s on enco](https://www.youtube.com/watch?v=PSs6nxngL6k)d[er-decoder ar](https://www.youtube.com/watch?v=4Bdc55j80l8)chitectures and Attention from StatQuest, a full walkthrough of the original Transformer by The AI Hacker.

有关于编码器-解码器架构的视频和来自 StatQuest 的 Attention，这是 The AI Hacker 对原始 Transformer 的完整演练。

However, note that these encoder-decoder Transformers differ from most modern LLMs, which are typically “decoder-only” – if you’re pressed for time, you may be okay jumping right to these models and skipping the history lesson.

但是，请注意，这些编码器-解码器 Transformer 与大多数现代的 LLMs 不同，后者通常是“仅解码器”——如果您时间紧迫，您可能可以直接跳到这些模型并跳过历史课。

## Chapter 12: Decoder-Only Transformers 第 12 章：仅解码器变压器

There’s a lot of moving pieces inside of Transformers — multi-head attention, skip connections, positional encoding, etc. — and it can be tough to appreciate it all the first time you see it. Building up intuitions for why some of these choices are made helps a lot, and here I’ll recommend to pretty much anyone that you watch a video or two about them (even if you’re normally a textbook learner), largely because there are a few videos which are really excellent:

《变形金刚》中有很多令人感动的部分——多头注意力、跳跃连接、位置编码等——第一次看到它时可能很难理解它们。建立对为什么做出这些选择的直觉会有很大帮助，在这里我会向几乎所有观看过一两个有关这些选择的视频的人推荐（即使您通常是教科书学习者），主要是因为一些非常精彩的视频：

+   3Blue1Brown’s “But what is a GPT?” and [“Attention in transformers, explained visually”](Attention in transformers, visually explained

+   3Blue1Brown 的“但是什么是 GPT？”和[“变压器中的注意力，视觉解释”]（变压器中的注意力，视觉解释

+   Chapter 6, Deep Learning) – beautiful animations + discussions, supposedly a 3rd video is on the way

+   第 6 章，深度学习）——漂亮的动画 + 讨论，据说第三个视频即将发布

+   Andrej Karpathy’s “Let’s build GPT” video – live coding and excellent explanations, really helped some things “click” for me
Andrej Karpathy 的“让我们构建 GPT”视频 – 实时编码和出色的解释，确实帮助我“点击”了一些事情

Here’s a [blog post](https://cameronrwolfe.substack.com/p/decoder-only-transformers-the-workhorse) fro[m Ca](https://d2l.ai/chapter_attention-mechanisms-and-transformers/large-pretraining-transformers.html)meron Wolfe walking through the decoder-only architecture in a similar style to the Illustrated/Annotated Transformer posts. There’s also a nice section in d2l.ai (11.9) covering the relationships between encoder-only, encoder-decoder, and decoder-only Transformers.

这是 Cameron Wolfe 的博客文章，以与插图/注释 Transformer 文章类似的风格介绍了仅解码器的架构。 d2l.ai (11.9) 中还有一个很好的部分，涵盖了仅编码器、编码器-解码器和仅解码器 Transformer 之间的关系。

# Section III: Foundations for Modern Language Modeling 第三节：现代语言建模的基础

> **Goal:** Survey central topics related to training LLMs, with an emphasis on conceptual primitives.

> 目标：调查与培训LLMs相关的中心主题，重点是概念原语。

>

> In this [section, we’ll ex](https://magazine.sebastianraschka.com/p/understanding-large-language-models)plore a number of concepts which will take us from the decoder-only Transformer architecture towards understanding the implementation choices and tradeoffs behind many of today’s frontier LLMs. If you first want a birds-eye view the of topics in section and some of the following ones, the post “Understanding Large Language Models” by Sebastian Raschka is a nice summary of what the LLM landscape looks like (at least up through mid-2023).

> 在本节中，我们将探讨一些概念，这些概念将使我们从仅解码器的 Transformer 架构转向理解当今许多前沿LLMs背后的实现选择和权衡。如果您首先想要鸟瞰本节中的主题以及以下一些主题，Sebastian Raschka 的文章“理解大型语言模型”很好地总结了 LLM 的情况（至少到 2023 年中期）。

## Chapter 13: Tokenization 第13章：代币化

[Chara](https://www.youtube.com/watch?v=zduSFxRajkE)c[ter-l](https://blog.octanove.org/guide-to-subword-tokenization/)evel tokenization (like in several of the Karpathy videos) tends to be inefficient for large-scale Transformers vs. word-level tokenization, yet naively picking a fixed “dictionary” (e.g. Merriam-Webster) of full words runs the risk of encountering unseen words or misspellings at inference time. Instead, the typical approach is to use subword-level tokenization to “cover” the space of possible inputs, while maintaining the efficiency gains which come from a larger token pool, using algorithms like Byte-Pair Encoding (BPE) to select the appropriate set of tokens. If you’ve ever seen Huffman coding in an introductory algorithms class I think it’s a somewhat useful analogy for BPE here, although the input-output format is notably different, as we don’t know the set of “tokens” in advance. I’d recommend watching Andrej Karpathy’s video on tokenization and checking out this tokenization guide from Masato Hagiwara.

与单词级标记化相比，字符级标记化（如几个 Karpathy 视频中的情况）对于大规模 Transformer 来说往往效率低下，但天真地选择完整单词的固定“词典”（例如 Merriam-Webster）会面临以下风险：在推理时遇到看不见的单词或拼写错误。相反，典型的方法是使用子字级标记化来“覆盖”可能输入的空间，同时保持来自较大标记池的效率增益，使用字节对编码（BPE）等算法来选择适当的集合的代币。如果您曾经在介绍性算法课程中见过霍夫曼编码，我认为这对于 BPE 来说是一个有点有用的类比，尽管输入输出格式明显不同，因为我们事先不知道“令牌”集。我建议观看 Andrej Karpathy 关于标记化的视频，并查看 Masato Hagiwara 的标记化指南。

## Chapter 14: Positional Encoding 第14章：位置编码

As we saw in the past section, Transformers don’t natively have the same notion of adjacency or position within a context windows (in contrast to RNNs), and position must instead represented with some kind of vector encoding. While this could be done naively with something like one-hot encoding, this is impractical for context-scaling and suboptimal for learnability, as it throws away notions of ordinality. Originally, this was done with sinusoidal positional encodings, which may feel reminiscent of Fourier features if you’re familiar; the most popular implementation of this type of approach nowadays is likely Rotary Positional Encoding, or RoPE, which tends to be more stable and faster to learn during training.

正如我们在上一节中看到的，Transformers 在上下文窗口中本身并不具有相同的邻接或位置概念（与 RNN 相比），并且位置必须用某种向量编码来表示。虽然这可以通过诸如 one-hot 编码之类的方式天真地完成，但这对于上下文扩展来说是不切实际的，并且对于可学习性来说也是次优的，因为它抛弃了序数的概念。最初，这是通过正弦位置编码完成的，如果您熟悉的话，这可能会让人想起傅里叶特征；如今此类方法最流行的实现可能是旋转位置编码（RoPE），它在训练过程中往往更稳定且学习速度更快。

Resources: 资源：

+   [blog post](https://harrisonpim.com/blog/understanding-positional-embeddings-in-transformer-models) by Harrison Pim on intution for positional encodings
Harrison Pim 关于位置编码直觉的博客文章

+   [blog post](https://machinelearningmastery.com/a-gentle-introduction-to-positional-encoding-in-transformer-models-part-1/) by Mehreen Saeed on the original Transformer positional encodings
Mehreen Saeed 关于原始 Transformer 位置编码的博客文章

+   [blog post](https://blog.eleuther.ai/rotary-embeddings/) on RoPE from Eleuther AI original Transformer: https://kazemnejad.com/blog/transformer_architecture_positional_encoding/ https://machinelearningmastery.com/a-gentle-introduction-to-positional-encoding-in-transformer-models-part-1/
Eleuther AI 原始 Transformer 关于 RoPE 的博客文章：https://kazemnejad.com/blog/transformer_architecture_positional_encoding/ https://machinelearningmastery.com/a-gentle-introduction-to-positional-encoding-in-transformer-models-part- 1/

+   [animated video](https://www.youtube.com/watch?v=GQPOtyITy54) from DeepLearning Hero
DeepLearning Hero 的动画视频

## Chapter 15: Pretraining Recipes 第15章：预训练食谱

Once you’ve committed to pretraining a LLM of a certain general size on a particular corpus of data (e.g Common Crawl, FineWeb), there are still a number of choices to make before you’re ready to go:

一旦您承诺在特定数据集（例如 Common Crawl、FineWeb）上预训练一定大小的 LLM，在准备开始之前仍然需要做出许多选择:

+   Attention mechanisms (multi-head, multi-query, grouped-query)
注意力机制（多头、多查询、分组查询）

+   Activations (ReLU, GeLU, SwiGLU)
激活（ReLU、GeLU、SwiGLU）

+   Optimizers, learning rates, and schedulers (AdamW, warmup, cosine decay)
优化器、学习率和调度器（AdamW、预热、余弦衰减）

+   Dropout? 辍学？

+   Hyperparameter choices and search strategies
超参数选择和搜索策略

+   Batching, parallelization strategies, gradient accumulation
批处理、并行化策略、梯度累积

+   How long to train for, how often to repeat data
训练多长时间，重复数据的频率

+   …and many other axes of variation
......以及许多其他变化轴

As far as I can tell, there’s not a one-size-fits-all rule book for how to go about this, but I’ll share a handful of worthwhile resources to consider, depending on your interests:

据我所知，没有一本通用的规则手册来指导如何解决这个问题，但我将根据您的兴趣分享一些值得考虑的资源：

+   While it predates the LLM[ era, the blog post](https://karpathy.github.io/2019/04/25/recipe/) “A Recipe for Training Neural Networks” is a great starting point for framing this problem, as many of these questions are relevant throughout deep learning.
虽然它早于 LLM 时代，但博客文章“训练神经网络的秘诀”是解决这个问题的一个很好的起点，因为其中许多问题与整个深度学习相关。

+   “The Novice’s LLM Training Guide” by Alpin Dale, discussing hyperparameter choices in practice, as well as the finetuning techniques we’ll see in future sections.
Alpin Dale 的“新手LLM培训指南”，讨论了实践中的超参数选择，以及我们将在以后的章节中看到的微调技术。

+   [“How to train your own La](https://blog.replit.com/llm-training)rge Language Models” from Replit has some nice discussions on data pipelines and evaluations for training.
Replit 的“如何训练你自己的大型语言模型”对数据管道和训练评估进行了一些很好的讨论。

+   F[o](https://iamshobhitagarwal.medium.com/navigating-the-attention-landscape-mha-mqa-and-gqa-decoded-288217d0a7d1)r understanding attention mechanism tradeoffs, see the post “Navigating the Attention Landscape: MHA, MQA, and GQA Decoded” by Shobhit Agarwal.
要了解注意力机制的权衡，请参阅 Shobhit Agarwal 的文章“导航注意力景观：MHA、MQA 和 GQA 解码”。

+   For discussion of “popular defa[ul](https://deci.ai/blog/evolution-of-modern-transformer-swiglu-rope-gqa-attention-is-all-you-need/)ts”, see the post “The Evolution of the Modern Transformer: From ‘Attention Is All You Need’ to GQA, SwiGLU, and RoPE” from Deci AI.
有关“流行默认值”的讨论，请参阅 Deci AI 的文章“现代 Transformer 的演变：从‘注意力就是你所需要的’到 GQA、SwiGLU 和 RoPE”。

+   For details on learning r[ate schedulin](https://d2l.ai/chapter_optimization/lr-scheduler.html)g, see Chapter 12.11 from the d2l.ai book.
有关学习率调度的详细信息，请参阅 d2l.ai 书中的第 12.11 章。

+   For discussion of some controversy surrounding [repo](https://blog.eleuther.ai/nyt-yi-34b-response/)rting of “best practices”, see this post from Eleuther AI.
有关有关“最佳实践”报告的一些争议的讨论，请参阅 Eleuther AI 的这篇文章。

## Chapter 16: Distributed Training and FSDP 第 16 章：分布式训练和 FSDP

There are a number of additional challenges associated with training models which are too large to fit on individual GPUs (or even multi-GPU machines), typically necessitating the use of distributed training protocols like Fully Sharded Data Parallelism (FSDP), in which models can be co-located across machines during training. It’s probably worth also understanding its precursor Distributed Data Parallelism (DDP), which is covered in the first post linked below.

还有许多与训练模型相关的额外挑战，这些模型太大而无法适应单个 GPU（甚至多 GPU 机器），通常需要使用分布式训练协议，例如完全分片数据并行（FSDP），其中模型可以在训练期间跨机器共处一处。也许还值得了解它的前身分布式数据并行性 (DDP)，它在下面链接的第一篇文章中进行了介绍。

Resources: 资源：

+   official FSDP [blog post](https://engineering.fb.com/2021/07/15/open-source/fsdp/) from Meta (who pioneered the method) https://sumanthrh.com/post/distributed-and-efficient-finetuning/
来自 Meta（该方法的先驱者）的官方 FSDP 博客文章 https://sumanthrh.com/post/distributed-and-efficient-finetuning/

+   [blog post](https://blog.clika.io/fsdp-1/) on FSDP by Bar Rozenman, featuring many excellent visualizations
Bar Rozenman 撰写的有关 FSDP 的博客文章，其中包含许多出色的可视化内容

+   [report](https://www.yitay.net/blog/training-great-llms-entirely-from-ground-zero-in-the-wilderness) from Yi Tai on the challenges of pretraining a model in a startup environment
Yi Tai 关于在初创环境中预训练模型面临的挑战的报告

+   [technical blog](https://www.answer.ai/posts/2024-03-14-fsdp-qlora-deep-dive.html) from Answer.AI on combining FSDP with parameter-efficient finetuning techniques for use on consumer GPUs
Answer.AI 的技术博客，介绍将 FSDP 与参数高效的微调技术结合起来用于消费级 GPU

## Chapter 17: Scaling Laws 第17章：缩放法则

It’s useful to know about scaling laws as a meta-topic which comes up [a lot](https://arxiv.org/abs/2203.15556) in discussions of LLMs (most prominently in reference to the “Chinchilla” paper), more so than any particular empirical finding or technique. In short, the performance which will result from scaling up the model, data, and compute used for training a language model results in fairly reliable predictions for model loss. This then enables calibration of optimal hyperparameter settings without needing to run expensive grid searches.

了解缩放定律作为一个元主题很有用，它在 LLMs 的讨论中经常出现（最突出的是参考“Chinchilla”论文），比任何特定的经验发现或技术都更重要。简而言之，扩展用于训练语言模型的模型、数据和计算所产生的性能可以对模型损失进行相当可靠的预测。这样就可以校准最佳超参数设置，而无需运行昂贵的网格搜索。

Resources: 资源：

+   [Chinchilla Scaling Laws for L](https://medium.com/@raniahossam/chinchilla-scaling-laws-for-large-language-models-llms-40c434e4e1c1)arge Language Models (blog overview by Rania Hossam)
大型语言模型的 Chinchilla 缩放法则（Rania Hossam 的博客概述）

+   New Scaling Laws for LLMs (discussion on LessWrong)
LLMs 的新缩放法则（LessWrong 上的讨论）

+   Chinchilla’s Wild Implications (post on LessWrong)
龙猫的狂野含义（LessWrong 上的帖子）

+   [Chinchilla Scaling: A](https://epochai.org/blog/chinchilla-scaling-a-replication-attempt) Replication Attempt (potential issues with Chinchilla findings)
龙猫缩放：复制尝试（龙猫发现的潜在问题）

+   [Scaling Laws and](https://cthiriet.com/blog/scaling-laws) Emergent Properties (blog post by Clément Thiriet)
缩放定律和新兴属性（Clément Thiriet 的博客文章）

+   “Scaling Language Models” (video lecture, Stanford CS224n)
“扩展语言模型”（视频讲座，斯坦福大学 CS224n）

## Chapter 18: Mixture-of-Experts 第18章：专家混合

While many of the prominent LLMs (such as Llama3) used today are “dense” models (i.e. without enforced sparsification), Mixture-of-Experts (MoE) architectures ar[e becom](https://the-decoder.com/gpt-4-architecture-datasets-costs-and-more-leaked/)ing increasingly popular for navigating tradeoffs between “knowledge” and efficiency, used perhaps most notably in the open-weights world by Mistral AI’s “Mixtral” models (8x7B and 8x22B), and rumored to be used for GPT-4. In MoE models, only a fraction of the parameters are “active” for each step of inference, with trained router modules for selecting the parallel “experts” to use at each layer. This allows models to grow in size (and perhaps “knowlege” or “intelligence”) while remaining efficient for training or inference compared to a comparably-sized dense model.

虽然当今使用的许多著名的LLMs（例如Llama3）都是“密集”模型（即没有强制稀疏），但专家混合（MoE）架构在“知识”之间的权衡中变得越来越流行。 ” 和效率，在开放重量世界中最显着的使用可能是 Mistral AI 的“Mixtral”模型（8x7B 和 8x22B），并且有传言称用于 GPT-4。在 MoE 模型中，只有一小部分参数对于每个推理步骤都是“活跃的”，并且经过训练的路由器模块用于选择在每一层使用的并行“专家”。与同等大小的密集模型相比，这允许模型的大小（可能还有“知识”或“智能”）增长，同时保持训练或推理的效率。

See this [blog post](https://huggingface.co/blog/moe) [from ](https://www.youtube.com/watch?v=0U_65fLoTq0)Hugging Face for a technical overview, and this video from Trelis Research for a visualized explainer.

请参阅 Hugging Face 的这篇博文了解技术概述，查看 Trelis Research 的视频了解可视化解释。

# Section IV: Finetuning Methods for LLMs 第四节：LLMs的微调方法

> **Goal:** Survey techniques used for improving and "aligning" the quality of LLM outputs after pretraining.

> 目标：用于改进和“调整”预训练后 LLM 输出质量的调查技术。

>

> In pre-training, the goal is basically “predict the next token on random internet text”. While the resulting “base” models are still useful in some contexts, their outputs are often chaotic or “unaligned”, and they may not respect the form[at of a ba](https://github.com/mlabonne/llm-course?tab=readme-ov-file)ck-and-forth conversation. Here we’ll look at a set of techniques for going from these base models to ones resembling the friendly chatbots and assistants we’re more familiar with. A great companion resource, especially for this section, is Maxime Labonne’s interactive LLM course on Github.

> 在预训练中，目标基本上是“预测随机互联网文本上的下一个标记”。虽然由此产生的“基本”模型在某些情况下仍然有用，但它们的输出通常是混乱的或“未对齐的”，并且它们可能不尊重来回对话的格式。在这里，我们将研究一组从这些基本模型转变为类似于我们更熟悉的友好聊天机器人和助手的技术。 Maxime Labonne 在 Github 上的交互式 LLM 课程是一个很好的配套资源，特别是对于本部分。

## Chapter 19: Instruct Fine-Tuning 第19章：指导微调

Instruct fine-tuning (or “instruction tuning”, or “supervised finetuning”, or “chat tuning” – the boundaries here are a bit fuzzy) is the primary technique used (at least initially) for coaxing LLMs to conform to a particular style or format. Here, data is presented as a sequence of (input, output) pairs where the input is a user question to answer, and the model’s goal is to predict the output – typically this also involves adding special “start”/”stop”/”role” tokens and other masking techniques, enabling the model to “understand” the difference between the user’s input and its own outputs. This technique is also widely used for task-specific finetuning on datasets with a particular kind of problem structure (e.g. translation, math, general question-answering).

指令微调（或“指令调整”、“监督微调”或“聊天调整”——这里的界限有点模糊）是用于哄骗的主要技术（至少在最初）LLMs符合特定的风格或格式。在这里，数据以（输入，输出）对的序列呈现，其中输入是要回答的用户问题，模型的目标是预测输出 - 通常这还涉及添加特殊的“开始”/“停止”/” “角色”标记和其他屏蔽技术，使模型能够“理解”用户的输入与其自己的输出之间的差异。该技术还广泛用于对具有特定类型问题结构（例如翻译、数学、一般问答）的数据集进行特定于任务的微调。

See this [blog post](https://newsletter.ruder.io/p/instruction-tuning-vol-1)[ from](https://www.youtube.com/watch?v=YoVek79LFe0) Sebastian Ruder or this video from Shayne Longpre for short overviews.

请参阅 Sebastian Ruder 的博客文章或 Shayne Longpre 的视频以获取简短概述。

## Chapter 20: Low-Rank Adapters (LoRA) 第 20 章：低阶适配器 (LoRA)

While pre-training (and “full finetuning”) requires applying gradient updates to all parameters of a model, this is typically impractical on consumer GPUs or home setups; fortunately, it’s often possible to significantly reduce the compute requirements by using parameter-efficient finetuning (PEFT) techniques like Low-Rank Adapters (LoRA). This can enable competitive performance even with relatively small datasets, particularly for application-specific use cases. The main idea behind LoRA is to train each weight matrix in a low-rank space by “freezing” the base matrix and training a factored representation with much smaller inner dimension, which is then added to the base matrix.

虽然预训练（和“完全微调”）需要对模型的所有参数应用梯度更新，但这对于消费级 GPU 或家庭设置通常是不切实际的；幸运的是，通过使用低阶适配器 (LoRA) 等参数高效微调 (PEFT) 技术，通常可以显着降低计算要求。即使数据集相对较小，这也可以实现具有竞争力的性能，特别是对于特定于应用程序的用例。 LoRA 背后的主要思想是通过“冻结”基础矩阵并训练具有更小的内部维度的因子表示来训练低秩空间中的每个权重矩阵，然后将其添加到基础矩阵。

Resources: 资源：

+   [LoRA paper walk](https://youtu.be/dA-NhCtrrVE?si=TpJkPfYxngQQ0iGj)through (video, part 1)
LoRA 论文演练（视频，第 1 部分）

+   LoRA code demo [(video, part 2)](https://youtu.be/iYr1xZn26R8?si=aG0F8ws9XslpZ4ur)
LoRA 代码演示（视频，第 2 部分）

+   [“](https://sebastianraschka.com/blog/2023/llm-finetuning-lora.html)Parameter-Efficient LLM Finetuning With Low-Rank Adaptation” by Sebastian Raschka
“参数高效 LLM 通过低阶适应进行微调”，作者：Sebastian Raschka

+   [“Practical Tips for Finetun](https://magazine.sebastianraschka.com/p/practical-tips-for-finetuning-llms)ing LLMs Using LoRA” by Sebastian Raschka
“使用 LoRA 微调 LLMs 的实用技巧”作者：Sebastian Raschka

Additionally, an “decomposed” LoRA variant called DoRA has been gaining[ pop](https://magazine.sebastianraschka.com/p/lora-and-dora-from-scratch)ularity in recent months, often yielding performance improvements; see this post from Sebastian Raschka for more details.

此外，一种名为 DoRA 的“分解”LoRA 变体近几个月越来越受欢迎，通常会带来性能改进；有关更多详细信息，请参阅 Sebastian Raschka 的这篇文章。

## Chapter 21: Reward Models and RLHF 第 21 章：奖励模型和 RLHF

One of the most prominent techniques for “aligning” a language model is Reinforcement Learning from Human Feedback (RLHF); here, we typically assume that an LLM has already been instruction-tuned to respect a chat style, and that we additionally have a “reward model” which has been trained on human preferences. Given pairs of differing outputs to an input, where a preferred output has been chosen by a human, the learning objective of the reward model is to predict the preferred output, which involves implicitly learning preference “scores”. This allows bootstrapping a general representation of human preferences (at least with respect to the dataset of output pairs), which can be used as a “reward simulator” for continual training of a LLM using RL policy gradient techniques like PPO.

“调整”语言模型最突出的技术之一是人类反馈强化学习（RLHF）；在这里，我们通常假设 LLM 已经经过指令调整以尊重聊天风格，并且我们另外还有一个根据人类偏好进行训练的“奖励模型”。给定输入的不同输出对，其中人类选择了首选输出，奖励模型的学习目标是预测首选输出，这涉及隐式学习偏好“分数”。这允许引导人类偏好的一般表示（至少相对于输出对的数据集），它可以用作“奖励模拟器”，使用 RL 策略梯度技术持续训练 LLM聚苯醚。

[F](https://huggingface.co/blog/rlhf)[or overviews, see the po](https://huyenchip.com/2023/05/02/rlhf.html)[sts “Illu](https://www.youtube.com/watch?v=2MBJOuVq380)[stra](https://sebastianraschka.com/blog/2024/research-papers-in-march-2024.html)ting Reinforcement Learning from Human Feedback (RLHF)” from Hugging Face and “Reinforcement Learning from Human Feedback” from Chip Huyen, and/or this RLHF talk by Nathan Lambert. Further, this post from Sebastian Raschka dives into RewardBench, and how reward models themselves can be evaluated against each other by leveraging ideas from Direct Preference Optimization, another prominent approach for aligning LLMs with human preference data.

有关概述，请参阅 Hugging Face 的文章“说明根据人类反馈进行强化学习 (RLHF)”和 Chip Huyen 的“根据人类反馈进行强化学习”，和/或 Nathan Lambert 的 RLHF 演讲。此外，Sebastian Raschka 的这篇文章深入探讨了 RewardBench，以及如何利用直接偏好优化的思想来相互评估奖励模型本身，直接偏好优化是另一种将 LLMs 与人类偏好数据对齐的重要方法。

## Chapter 22: Direct Preference Optimization Methods 第22章：直接偏好优化方法

The space of alignment algorithms seems to be following a similar trajectory as we saw with stochastic optimization algorithms a decade ago. In this an analogy, RLHF is like SGD — it works, it’s the original, and it’s also become kind of a generic “catch-all” term for the class of algorithms that have followed it. Perhaps DPO is AdaGrad, and in the year since its release there’s been a rapid wave of further algorithmic developments along the same lines (KTO, IPO, ORPO, etc.), whose relative merits are still under active debate. Maybe a year from now, everyone will have settled on a standard approach which will become the “Adam” of alignment.

对齐算法的空间似乎遵循与我们十年前在随机优化算法中看到的相似的轨迹。在这个类比中，RLHF 就像 SGD——它有效，它是原始的，而且它也成为了追随它的算法类的通用“包罗万象”术语。也许 DPO 就是 AdaGrad，自其发布以来的一年里，沿着同样的路线（KTO、IPO、ORPO 等）迅速出现了进一步的算法开发浪潮，其相对优点仍在激烈争论中。也许一年后，每个人都会确定一个标准方法，这将成为对齐的“亚当”。

For an overview of the theory [behind DP](https://towardsdatascience.com/understanding-the-implications-of-direct-preference-optimization-a4bbd2d85841)[O see thi](https://huggingface.co/blog/dpo-trl)s [blog post](https://huggingface.co/blog/pref-tuning) Matthew Gunton; this blog post from Hugging Face features some code and demonstrates how to make use of DPO in practice. Another blog post from Hugging Face also discusses tradeoffs between a number of the DPO-flavored methods which have emerged in recent months.

有关 DPO 背后理论的概述，请参阅这篇博客文章 Matthew Gunton； Hugging Face 的这篇博文提供了一些代码，并演示了如何在实践中使用 DPO。 Hugging Face 的另一篇博客文章还讨论了近几个月出现的一些 DPO 风格的方法之间的权衡。

## Chapter 23: Context Scaling 第23章：上下文缩放

Beyond task specification or alignment, another common goal of finetuning is to increase the effective context length of a model, either via additional training, adjusting parameters for positional encodings, or both. Even if adding more tokens to a model’s context can “type-check”, training on additional longer examples is generally necessary if the model may not have seen such long sequences during pretraining.

除了任务规范或对齐之外，微调的另一个常见目标是通过额外的训练、调整位置编码的参数或两者来增加模型的有效上下文长度。即使向模型的上下文添加更多标记可以进行“类型检查”，如果模型在预训练期间可能没有看到这么长的序列，那么通常需要对额外的较长示例进行训练。

Resources: 资源：

+   [“](https://gradient.ai/blog/scaling-rotational-embeddings-for-long-context-language-models)Scaling Rotational Embeddings for Long-Context Language Models” by Gradient AI
Gradient AI 的“长上下文语言模型的缩放旋转嵌入”

+   “Extending the RoPE” by Eleuther AI, introducing the YaRN method for increased context via attention temperature scaling
Eleuther AI 的“Extending the RoPE”，引入了 YaRN 方法，通过注意力温度缩放来增加上下文

+   [“Everything About Long ](https://huggingface.co/blog/wenbopan/long-context-fine-tuning)Context Fine-tuning” by Wenbo Pan
“关于长上下文微调的一切”，作者：Wenbo Pan

## Chapter 24: Distillation and Merging 第24章：蒸馏与合并

Here we’ll look at two very different methods of consolidating knowledge across LLMs — distillation and merging. Distillation was first popularized for BERT models, where the goal is to “distill” the knowledge and performance of a larger model into a smaller one (at least for some tasks) by having it serve as a “teacher” during the smaller model’s training, bypassing the need for large quantities of human-labeled data.

在这里，我们将研究两种截然不同的方法来巩固LLMs中的知识——蒸馏和合并。蒸馏首先在 BERT 模型中普及，其目标是通过在较小模型的训练期间充当“老师”，将较大模型的知识和性能“蒸馏”到较小的模型中（至少对于某些任务），绕过对大量人工标记数据的需求。

Some resources on distillation:

有关蒸馏的一些资源：

+   [“S](https://medium.com/huggingface/distilbert-8cf3380435b5)maller, faster, cheaper, lighter: Introducing DistilBERT, a distilled version of BERT” from Hugging Face
“更小、更快、更便宜、更轻：推出 DistilBERT，BERT 的精炼版”来自 Hugging Face

+   [“LLM distillation demystifie](https://snorkel.ai/llm-distillation-demystified-a-complete-guide/)d: a complete guide” from Snorkel AI
Snorkel AI 的“LLM蒸馏揭秘：完整指南”

+   “Distilling Step by Step” blog from Google Research
来自 Google Research 的“Distilling Step by Step”博客

[Merging, on the other](https://huggingface.co/blog/mlabonne/merge-models) hand, is much more of a “wild west” technique, largely used by open-source engineers who want to combine the strengths of multiple finetuning efforts. It’s kind of wild to me that it works at all, and perhaps grants some credence to “linear representation hypotheses” (which will appear in the next section when we discuss interpretability). The idea is basically to take two different finetunes of the same base model and just average their weights. No training required. Technically, it’s usually “spherical interpolation” (or “slerp”), but this is pretty much just fancy averaging with a normalization step. For more details, see the post Merge Large Language Models with mergekit by Maxime Labonne.

另一方面，合并更像是一种“狂野西部”技术，主要由希望结合多种微调工作优势的开源工程师使用。对我来说，它竟然有效，这有点疯狂，并且也许为“线性表示假设”提供了一些可信度（当我们讨论可解释性时，它将出现在下一节中）。这个想法基本上是对同一基本模型进行两种不同的微调，然后平均它们的权重。无需培训。从技术上讲，它通常是“球形插值”（或“slerp”），但这几乎只是带有归一化步骤的奇特平均。有关更多详细信息，请参阅 Maxime Labonne 撰写的文章使用 mergekit 合并大型语言模型。

# Section V: LLM Evaluations and Applications 第五节：LLM评估和应用

**Goal:** Survey how LLMs are used and evaluated in practice, beyond just "chatbots".

目标：调查 LLMs 在实践中的使用和评估方式，而不仅仅是“聊天机器人”。

Here we’ll be looking at a handful of topics related to improving or modifying the performance of language models without additional training, as well as techniques for measuring and understanding their performance.

在这里，我们将研究一些与在无需额外训练的情况下改进或修改语言模型性能相关的主题，以及测量和理解其性能的技术。

Before diving into the individual chapters, I’d recommend these two high-level overviews, which touch on many of the topics we’ll examine here:

在深入了解各个章节之前，我建议您进行这两个高级概述，它们涉及我们将在此处讨论的许多主题：

+   [“Building LLM applicat](https://huyenchip.com/2023/04/11/llm-engineering.html)ions for production” by Chip Huyen
“构建 LLM 生产应用程序”，作者：Chip Huyen

+   [“](https://www.oreilly.com/radar/what-we-learned-from-a-year-of-building-with-llms-part-i/)What [We Lea](https://www.oreilly.com/radar/what-we-learned-from-a-year-of-building-with-llms-part-ii/)rned from a Year of Building with LLMs” Part 1 and Part 2 from O’Reilly (several authors)
“我们从 LLMs 的一年建设中学到了什么”第 1 部分和第 2 部分来自 O’Reilly（几位作者）

These web courses also have a lot of relevant interactive materials:

这些网络课程还有很多相关的互动材料：

+   “Large Language Model Course” from Maxime Labonne
Maxime Labonne 的《大语言模型课程》

+   “Generative AI for Beginners” from Microsoft
微软的“面向初学者的生成式人工智能”

## Chapter 25: Benchmarking 第25章：基准测试

Be[yond the s](https://medium.com/@priyankads/perplexity-of-language-models-41160427ed72)tandard numerical performance measures used during LLM training like cross-entropy loss and perplexity, the true performance of frontier LLMs is more commonly judged according to a range of benchmarks, or “evals”. Common types of these are:

除了LLM训练期间使用的标准数值性能测量（例如交叉熵损失和困惑度）之外，前沿LLMs的真实性能更常见的是根据一系列基准或“评估”来判断”。其中常见的类型有：

+   Human-evaluated outputs (e.g. LMSYS Chatbot Arena)
人工评估的输出（例如 LMSYS Chatbot Arena）

+   [AI-ev](https://argilla.io/blog/mantisnlp-rlhf-part-4/)aluated outputs (as used in RLAIF)
AI 评估的输出（如 RLAIF 中使用的）

+   C[hallenge questi](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)on sets (e.g. those in HuggingFace’s LLM Leaderboard)
挑战问题集（例如 HuggingFace 的 LLM 排行榜中的问题集）

See the [slides](https://web.stanford.edu/class/cs224n/slides/cs224n-spr2024-lecture11-evaluation-yann.pdf) from Stanford’s CS224n fo[r an over](https://www.jasonwei.net/blog/evals)[view. Th](https://humanloop.com/blog/evaluating-llm-apps?utm_source=newsletter&utm_medium=sequence&utm_campaign=)is blog post by Jason Wei and this one by Peter Hayes do a nice job discussing the challenges and tradeoffs[ associate](https://ukgovernmentbeis.github.io/inspect_ai/)d with designing good evaluations, and highlighting a number of the most prominent ones used today. The documentation for the open source framework inspect-ai also features some useful discussion around designing benchmarks and reliable evaluation pipelines.

请参阅斯坦福大学 CS224n 的幻灯片了解概述。 Jason Wei 的这篇博文和 Peter Hayes 的这篇博文很好地讨论了与设计良好评估相关的挑战和权衡，并重点介绍了当今使用的一些最著名的评估。开源框架 Inspect-ai 的文档还提供了一些有关设计基准和可靠评估管道的有用讨论。

## Chapter 26: Sampling and Structured Outputs 第 26 章：采样和结构化输出

While typical LLM inference samples tokens one at a time, there are number of parameters controlling the token distribution (temperature,[ top_p, t](https://towardsdatascience.com/decoding-strategies-in-large-language-models-9733a8f70539)op_k) which can be modified to control the variety of responses, as well as non-greedy decoding strategies that allow some degree of “lookahead”. This blog post by Maxime Labonne does a nice job discussing several of them.

虽然典型的 LLM 推理一次采样一个令牌，但有许多参数控制令牌分布（温度、top_p、top_k），可以修改这些参数来控制响应的多样性以及非贪婪允许一定程度“前瞻”的解码策略。 Maxime Labonne 的这篇博文很好地讨论了其中的几个问题。

Sometimes we also want our outputs to follow a particular structure, particularly if we are using LLMs as a component of a larger system rather than as just a chat interface. Few-shot prompting works okay, but not all the time, particularly as output schemas become more complicated. For schema types like JSON, Pydantic and Outlines are popular tools for constraining the output structure from LLMs. Some useful resources:

有时我们还希望输出遵循特定的结构，特别是当我们使用 LLMs 作为更大系统的组件而不仅仅是聊天界面时。少量提示可以正常工作，但并非总是有效，特别是当输出模式变得更加复杂时。对于 JSON 等模式类型，Pydantic 和 Outlines 是用于约束 LLMs 输出结构的流行工具。一些有用的资源：

+   Pydantic Concepts  金字塔概念

+   Outlines for JSON  JSON 概要

+   [Outlines review](https://michaelwornow.net/2023/12/29/outlines-demo) by Michael Wornow
Michael Wornow 的大纲审查

## Chapter 27: Prompting Techniques 第27章：提示技巧

There are many prompting techniques, and many more prompt engineering guides out there, featuring methods for coaxing more desirable outputs from LLMs. Some of the classics:

有许多提示技术和更多提示工程指南，其中包含从 LLMs 中诱导更理想的输出的方法。一些经典：

+   Few-Shot Examples 少样本示例

+   Chain-of-Thought 思想链

+   Retrieval-Augmented Generation (RAG)
检索增强生成 (RAG)

+   ReAct 反应

This [blog post](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/) [by Li](https://www.promptingguide.ai/techniques)lian Weng discusses several of the most dominant approaches, guide has decent coverage and examples for a wider range of the prominent techniques used today. Keyword-searching on Twitter/X or LinkedIn will give you plenty more. We’ll also dig deeper into RAG and agent methods in later chapters.

Lilian Weng 的这篇博客文章讨论了几种最主要的方法，指南对当今使用的更广泛的突出技术进行了适当的覆盖和示例。在 Twitter/X 或 LinkedIn 上进行关键字搜索将为您提供更多信息。我们还将在后面的章节中深入研究 RAG 和代理方法。

## Chapter 28: Vector Databases and Reranking 第 28 章：向量数据库和重新排序

RAG [systems r](https://genai-handbook.github.io/#embeddings-and-topic-modeling)equire the ability to qu[ickl](https://www.pinecone.io/learn/series/faiss/)y retr[ieve](https://www.youtube.com/watch?v=W-i8bcxkXok) relevant documents from large corpuses. Relevancy is typically determined by similarity measures for semantic embedding vectors of both queries and documents, such as cosine similarity or Euclidean distance. If we have just a handful of documents, this can be computed between a query and each document, but this quickly becomes intractable when the number of documents grows large. This is the problem addressed by vector databases, which allow retrieval of the approximate top-K matches (significantly faster than checking all pairs) by maintaining high-dimensional indices over vectors which efficiently encode their geometric structure. These docs from Pinecone do a nice job walking through a few different methods for vector storage, like Locality-Sensitive Hashing and Hierarchical Navigable Small Worlds, which can be implemented with the popular FAISS open-source library. This talk by Alexander Chatzizacharias gives a nice overview as well.

RAG 系统需要能够从大型语料库中快速检索相关文档。相关性通常由查询和文档的语义嵌入向量的相似性度量来确定，例如余弦相似性或欧几里德距离。如果我们只有少量文档，则可以在查询和每个文档之间进行计算，但当文档数量变大时，这很快就会变得棘手。这是向量数据库解决的问题，向量数据库允许通过维护有效编码其几何结构的向量的高维索引来检索近似的前 K 个匹配（比检查所有对要快得多）。 Pinecone 的这些文档很好地介绍了几种不同的向量存储方法，例如局部敏感哈希和分层可导航小世界，这些方法可以使用流行的 FAISS 开源库来实现。 Alexander Chatzizacharias 的这篇演讲也给出了很好的概述。

Anoth[er r](https://www.pinecone.io/learn/series/rag/rerankers/)elated application of vector retrieval is the “reranking” problem, wherein a model can optimize for other metrics beyond query similarity, such as diversity within retrieved results. See these docs from Pinecone for an overview. We’ll see more about how retrieved results are actually used by LLMs in the next chapter.

矢量检索的另一个相关应用是“重新排名”问题，其中模型可以优化查询相似性之外的其他指标，例如检索结果中的多样性。有关概述，请参阅 Pinecone 的这些文档。我们将在下一章中详细了解 LLMs 如何实际使用检索到的结果。

## Chapter 29: Retrieval-Augmented Generation 第29章：检索增强生成

One of the most buzzed-about uses of LLMs over the past year, retrieval-augmented generation (RAG) is how you can “chat with a PDF” (if larger than a model’s context) and how applications like Perplexity and Arc Search can “ground” their outputs using web sources. This retrieval is generally powered by embedding each document for storage in a vector database + querying with the relevant section of a user’s input.

检索增强生成 (RAG) 是过去一年中最受关注的 LLMs 用途之一，它是如何“与 PDF 聊天”（如果大于模型的上下文）以及应用程序如何Perplexity 和 Arc Search 可以使用网络资源“接地”他们的输出。这种检索通常是通过将每个文档嵌入到矢量数据库中存储+使用用户输入的相关部分进行查询来实现的。

Some overviews: 一些概述：

+   “Deconstructing RAG” from Langchain
Langchain《解构RAG》

+   [“](https://www.bentoml.com/blog/building-rag-with-open-source-and-custom-ai-models)Building RAG with Open-Source and Custom AI Models” from Chaoyu Yang
“使用开源和自定义 AI 模型构建 RAG”，来自 Chaoyu Yang

The [Advanced RAG](https://learn.deeplearning.ai/courses/building-evaluating-advanced-rag/lesson/1/introduction) video course from DeepLearning.AI may also be useful for exploring variants on the standard setup.

DeepLearning.AI 的高级 RAG 视频课程也可能有助于探索标准设置的变体。

## Chapter 30: Tool Use and "Agents" 第30章：工具使用和“代理”

The other big application buzzwords you’ve most likely encountered in some form are “tool use” and “agents”, or “agentic programming”. This typically starts with the ReAct framework we saw in the prompting section, then gets extended to elicit increasingly complex behaviors like software engineering (see the much-buzzed-about “Devin” system from Cognition, and several related open-source efforts like Devon/OpenDevin/SWE-Agent). There are many programming frameworks for building agent systems on top of LLMs, with Langchain and LlamaIndex being two of the most popular. There also seems to be some value in having LLMs rewrite their own prompts + evaluate their own partial outputs; this observation is at the heart of the DSPy framework (for “compiling” a program’s prompts, against a reference set of instructions or desired outputs) which has recently been seeing a lot of attention.

您最有可能以某种形式遇到的其他大型应用程序流行语是“工具使用”和“代理”或“代理编程”。这通常从我们在提示部分看到的 ReAct 框架开始，然后扩展以引发越来越复杂的行为，例如软件工程（请参阅 Cognition 中备受关注的“Devin”系统，以及一些相关的开源项目，例如 Devon/ OpenDevin/SWE-Agent）。有许多用于在 LLMs 之上构建代理系统的编程框架，其中 Langchain 和 LlamaIndex 是最流行的两个。让LLMs重写他们自己的提示+评估他们自己的部分输出似乎也有一些价值；这一观察结果是 DSPy 框架（用于根据指令参考集或所需输出“编译”程序的提示）的核心，该框架最近受到了广泛关注。

Resources: 资源：

+   [“LLM Powered Auton](https://lilianweng.github.io/posts/2023-06-23-agent/)omous Agents” (post) from Lilian Weng
“LLM 动力自主代理”（帖子）来自 Lilian Weng

+   [“A Guide to LLM ](https://www.twosigma.com/articles/a-guide-to-large-language-model-abstractions/)Abstractions” (post) from Two Sigma
“LLM 抽象指南”（帖子）来自两个西格码

+   “DSPy Explained! (video)” by Connor Shorten
“DSPy 解释了！ （视频）” 作者：Connor Shorten

Also relevant are more narrowly-tailored (but perhaps more p[ract](https://neo4j.com/developer-blog/knowledge-graphs-llms-multi-hop-question-answering/)i[cal) ](https://neo4j.com/blog/unifying-llm-knowledge-graph/)ap[plication](https://numbersstation.ai/data-wrangling-with-fms-2/)s related to databases — see these two blog posts from Neo4J for discussion on applying LLMs to analyzing or constructing knowledge graphs, or this blog post from Numbers Station about applying LLMs to data wrangling tasks like entity matching.

同样相关的是与数据库相关的更窄定制（但可能更实用）的应用程序 - 请参阅 Neo4J 的这两篇博客文章，讨论如何应用 LLMs 来分析或构建知识图，或者查看 Numbers Station 的这篇博客文章关于将 LLMs 应用于实体匹配等数据整理任务。

## Chapter 31: LLMs for Synthetic Data 第 31 章：LLMs 用于合成数据

An increasing number of applications are making use of LLM-generated data for training or evalua[tion](https://www.promptingguide.ai/applications/synthetic_rag)s, including distillation, dataset augmentati[on, ](https://argilla.io/blog/mantisnlp-rlhf-part-4/)AI-assisted evaluation and labeling, self-critique, and more. This post demonstrates how to construct such a synthetic dataset (in a R[AG c](https://www.anthropic.com/news/claudes-constitution)ontext), and this post from Argilla gives an overview of RLAIF, which is often a popular alternative to RLHF, given the challenges associated with gathering pairwise human preference data. AI-assisted feedback is also a central component of the “Constitutional AI” alignment method pioneered by Anthropic (see their blog for an overview).

越来越多的应用程序正在利用 LLM 生成的数据进行训练或评估，包括蒸馏、数据集扩充、人工智能辅助评估和标记、自我批评等。这篇文章演示了如何构建这样一个综合数据集（在 RAG 环境中），Argilla 的这篇文章概述了 RLAIF，考虑到收集成对人类偏好数据相关的挑战，RLAIF 通常是 RLHF 的流行替代品。人工智能辅助反馈也是 Anthropic 首创的“宪法人工智能”对齐方法的核心组成部分（有关概述，请参阅他们的博客）。

## Chapter 32: Representation Engineering 第32章：表示工程

Representation Engineering is a new and promising technique for fine-grained steering of language model outputs via “control vectors”. Somewhat similar to LoRA adapters, it has the effect of adding low-rank biases to the weights of a network which can elicit particular response styles (e.g. “humorous”, “verbose”, “creative”, “honest”), yet is much more computationally efficient and can be implemented without any training required. Instead, the method simply looks at differences in activations for pairs of inputs which vary along the axis of interest (e.g. honesty), which can be generated synthetically, and then performs dimensionality reduction.

表示工程是一种新的、有前途的技术，用于通过“控制向量”对语言模型输出进行细粒度控制。与 LoRA 适配器有些相似，它具有向网络权重添加低秩偏差的效果，这可以引发特定的响应风格（例如“幽默”、“冗长”、“创造性”、“诚实”），但计算效率更高，无需任何培训即可实施。相反，该方法只是查看沿兴趣轴（例如诚实度）变化的输入对的激活差异，这些输入对可以综合生成，然后执行降维。

See this short [blog post](https://www.safe.ai/blog/representation-engineering-a-new-way-of-understanding-models) from Center for AI Safety (who pioneered the[ met](https://vgel.me/posts/representation-engineering/)hod) for a brief overview, and this post from Th[eia Vogel for a](https://www.youtube.com/watch?v=PkA4DskA-6M) technical deep-dive with code examples. Theia also walks through the method in this podcast episode.

请参阅人工智能安全中心（该方法的先驱）的这篇简短博客文章以获取简要概述，以及 Theia Vogel 的这篇文章以通过代码示例进行技术深入探讨。 Theia 也在本播客节目中介绍了该方法。

## Chapter 33: Mechanistic Interpretability 第33章：机械可解释性

Mechanistic Interpretability (MI) is the dominant paradigm for understanding the inner workings of LLMs by identifying sparse representations of “features” or “circuits” encoded in model weig[hts. Beyon](https://www.neelnanda.io/)d enabling potential modification or explanation of LLM outputs, MI is often viewed as an important step towards potentially “aligning” increasingly powerful systems. Most of the references here will come from Neel Nanda, a leading researcher in the field who’s created a large number of useful educational resources about MI across a range of formats:

机械可解释性 (MI) 是通过识别模型权重中编码的“特征”或“电路”的稀疏表示来理解 LLMs 内部工作原理的主导范例。除了能够对 LLM 输出进行潜在的修改或解释之外，MI 通常被视为潜在地“调整”日益强大的系统的重要一步。这里的大部分参考资料都来自 Neel Nanda，他是该领域的一位领先研究人员，他创建了大量关于 MI 的有用的教育资源，涵盖多种格式：

+   [“](https://www.neelnanda.io/mechanistic-interpretability/glossary)[A Comprehensiv](https://www.neelnanda.io/mechanistic-interpretability/glossary)e Mechanistic Interpretability Explainer & Glossary”
“全面的机制解释器和术语表”

+   [“A](https://www.neelnanda.io/mechanistic-interpretability/favourite-papers)n Extremely Opinionated Annotated List of My Favourite Mechanistic Interpretability Papers”
“我最喜欢的机械可解释性论文的极端固执己见的注释列表”

+   [“Mechanistic Interpretabili](https://www.lesswrong.com/posts/jLAvJt8wuSFySN975/mechanistic-interpretability-quickstart-guide)ty Quickstart Guide” (Neel Nanda on LessWrong)
“机械可解释性快速入门指南”（LessWrong 上的 Neel Nanda）

+   [“How useful is mechanisti](https://www.lesswrong.com/posts/tEPHGZAb63dfq2v8n/how-useful-is-mechanistic-interpretability)c interpretability?” (Neel and others, discussion on LessWrong)
“机械解释有多大用处？” （Neel 等人，LessWrong 上的讨论）

+   [“200 Concrete Problems ](https://docs.google.com/spreadsheets/d/1oOdrQ80jDK-aGn-EVdDt3dg65GhmzrvBWzJ6MUZB8n4/edit#gid=0)In Interpretability” (Annotated spreadsheet of open problems from Neel)
“200 个可解释性的具体问题”（Neel 的开放问题带注释的电子表格）

Addit[ionally, the articles “Toy Models of Supe](https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html)rposition” and “Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet” from Anthropic are on the longer side, but feature a number of great visualizations and demonstrations of these concepts.

此外，Anthropic 的文章“Toy Models of Superposition”和“Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet”篇幅较长，但对这些概念进行了许多精彩的可视化和演示。

## Chapter 34: Linear Representation Hypotheses 第34章：线性表示假设

An emerging[ theme fr](https://www.beren.io/2023-04-04-DL-models-are-secretly-linear/)[om s](https://www.youtube.com/watch?v=ko1xVcyDt8w)e[veral lines of interpr](https://arxiv.org/pdf/2310.02207)etability research has been the observation that internal representations of features in Transformers are often “linear” in high-dimensional space (a la Word2Vec). On one hand this may appear initially surprising, but it’s also essentially an implicit assumption for techniques like similarity-based retrieval, merging, and the key-value similarity scores used by attention. See this blog post by Beren Millidge, this talk from Kiho Park, and perhaps at least skim the paper “Language Models Represent Space and Time” for its figures.

多个可解释性研究中的一个新兴主题是观察到 Transformer 中特征的内部表示在高维空间中通常是“线性的”（类似于 Word2Vec）。一方面，这可能最初看起来令人惊讶，但它本质上也是基于相似性的检索、合并和注意力所使用的键值相似性分数等技术的隐含假设。请参阅 Beren Millidge 的博客文章和 Kiho Park 的演讲，也许至少浏览一下论文“语言模型代表空间和时间”中的数据。

# Section VI: Performance Optimizations for Efficient Inference 第六节：高效推理的性能优化

**Goal:** Survey architecture choices and lower-level techniques for improving resource utilization (time, compute, memory).

目标：调查用于提高资源利用率（时间、计算、内存）的架构选择和底层技术。

Here w[e’ll look](https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/) at a handful of techniques for improving the speed and efficiency of inference from pre-trained Transformer language models, most of which are fairly widely used in practice. It’s worth first reading this short Nvidia blog post for a crash course in several of the topics we’ll look at (and a number of others).

在这里，我们将介绍一些用于提高预训练 Transformer 语言模型的推理速度和效率的技术，其中大多数在实践中得到了相当广泛的应用。值得首先阅读这篇简短的 Nvidia 博客文章，了解我们将要讨论的几个主题（以及其他一些主题）的速成课程。

## Chapter 35: Parameter Quantization 第35章：参数量化

With the rapi[d increas](https://timdettmers.com/2022/08/17/llm-int8-and-emergent-features/)e in p[aram](https://www.tensorops.ai/post/what-are-quantized-llms)eter counts for leadin[g LL](https://www.maartengrootendorst.com/blog/quantization/)Ms and difficulties (both in cost and availability) in acquiring GPUs to run models on, there’s been a growing interest in quantizing LLM weights to use fewer bits each, which can often yield comparable output quality with a 50-75% (or more) reduction in required memory. Typically this shouldn’t be done naively; Tim Dettmers, one of the pioneers of several modern quantization methods (LLM.int8(), QLoRA, bitsandbytes) has a great blog post for understanding quantization principles, and the need for mixed-precision quantization as it relates to emergent features in large-model training. Other popular methods and formats are GGUF (for llama.cpp), AWQ, HQQ, and GPTQ; see this post from TensorOps for an overview, and this post from Maarten Grootendorst for a discussion of their tradeoffs.

随着领先LLMs参数数量的快速增加以及获取GPU来运行模型的困难（成本和可用性方面），人们对量化LLM权重越来越感兴趣每个位数更少，通常可以产生相当的输出质量，同时所需内存减少 50-75%（或更多）。通常这不应该天真地完成； Tim Dettmers 是多种现代量化方法（LLM.int8()、QLoRA、bitsandbytes）的先驱之一，他发表了一篇很棒的博客文章，介绍了量化原理以及混合精度量化的必要性大型模型训练中的新兴特征。其他流行的方法和格式有 GGUF（对于 llama.cpp）、AWQ、HQQ 和 GPTQ；请参阅 TensorOps 的这篇文章来了解概述，以及 Maarten Grootendorst 的这篇文章来讨论它们的权衡。

In addition to enabling inference on smaller machines, quantization is also popular for parameter-efficient trainin[g; i](https://www.youtube.com/watch?v=fQirE9N5q_Y)[n QL](https://huggingface.co/blog/4bit-transformers-bitsandbytes)oRA, most weights a[re quanti](https://www.answer.ai/posts/2024-03-14-fsdp-qlora-deep-dive.html)zed to 4-bit precision and frozen, while active LoRA adapters are trained in 16-bit precision. See this talk from Tim Dettmers, or this blog from Hugging Face for overviews. This blog post from Answer.AI also shows how to combine QLoRA with FSDP for efficient finetuning of 70B+ parameter models on consumer GPUs.

除了能够在较小的机器上进行推理之外，量化对于参数高效的训练也很流行；在 QLoRA 中，大多数权重被量化为 4 位精度并冻结，而主动 LoRA 适配器则以 16 位精度进行训练。请参阅 Tim Dettmers 的演讲或 Hugging Face 的博客了解概述。 Answer.AI 的这篇博文还展示了如何将 QLoRA 与 FSDP 结合起来，以便在消费类 GPU 上高效微调 70B+ 参数模型。

## Chapter 36: Speculative Decoding 第36章：推测解码

The basic idea behind speculative decoding is to speed up inference from a larger model by primarily sampling tokens from a much smaller model and occasionally applying corrections (e.g. every N tokens) from the larger model whenever the output distributions diverge. These batched consistency checks tend to be much faster than sampling N tokens directly, and so there can be large overall speedups if the token sequences from smaller models only diverge periodically.

推测性解码背后的基本思想是通过主要从较小模型中采样标记，并在输出分布发散时偶尔应用较大模型的校正（例如，每 N 个标记）来加速较大模型的推理。这些批量一致性检查往往比直接采样 N 个令牌要快得多，因此如果来自较小模型的令牌序列仅周期性发散，则可能会出现很大的整体加速。

See this [blog post](https://jaykmody.com/blog/speculative-sampling/) from Jay Mody for a walkthrough [of the ](https://pytorch.org/blog/hitchhikers-guide-speculative-decoding/)original paper, and this [PyTor](https://www.youtube.com/watch?v=hm7VEgxhOvk)ch article for some evaluation results. There’s a nice video overview from Trelis Research as well.

请参阅 Jay Mody 的这篇博客文章了解原始论文的演练，以及这篇 PyTorch 文章了解一些评估结果。 Trelis Research 也提供了一个很好的视频概述。

## Chapter 37: FlashAttention 第37章：FlashAttention

Comput[ing atten](https://genai-handbook.github.io/#s7)tion matrices tends to be a primary bottleneck in inference and training for Transformers, and FlashAttention has become one of the most widely-used techniques for speeding it up. In contrast to some of the techniques we’ll see in Section 7 which approximate attention with a more concise representation (occurring some representation error as a result), FlashAttention is an exact representation whose speedup comes from hardware-aware impleemntation. It applies a few tricks — namely, tiling and recomputation — to decompose the expression of attention matrices, enabling significantly reduced memory I/O and faster wall-clock performance (even with slightly increasing the required FLOPS).

计算注意力矩阵往往是 Transformers 推理和训练的主要瓶颈，而 FlashAttention 已成为最广泛使用的加速技术之一。与我们将在第 7 节中看到的一些技术相比，这些技术用更简洁的表示来近似注意力（因此会出现一些表示错误），FlashAttention 是一种精确的表示，其加速来自于硬件感知的实现。它应用了一些技巧（即平铺和重新计算）来分解注意力矩阵的表达，从而显着减少内存 I/O 和更快的挂钟性能（即使稍微增加所需的 FLOPS）。

Resources: 资源：

+   [Talk](https://www.youtube.com/watch?v=gMOAud7hZg4) by Tri Dao (author of FlashAttention)
Tri Dao 的演讲（FlashAttention 的作者）

+   ELI5: FlashAttention by Aleksa Gordić
ELI5：Aleksa Gordić 的 FlashAttention

## Chapter 38: Key-Value Caching and Paged Attention 第 38 章：键值缓存和分页注意力

[As noted in](https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/)[ the](https://docs.vllm.ai/en/stable/) NVIDIA blog referenced above, key-value caching is fairly standard in Transformer implementation matrices to avoid redundant recomputation of attention. This enables a tradeoff between speed and resource utilization, as these matrices are kept in GPU VRAM. While managing this is fairly straightforward for a single “thread” of inference, a number of complexities arise when considering parallel inference or multiple users for a single hosted model instance. How can you avoid recomputing values for system prompts and few-shot examples? When should you evict cache elements for a user who may or may not want to continue a chat session? PagedAttention and its popular implementation vLLM addresses this by leveraging ideas from classical paging in operating systems, and has become a standard for self-hosted multi-user inference servers.

正如上面引用的 NVIDIA 博客中所指出的，键值缓存在 Transformer 实现矩阵中是相当标准的，以避免冗余的注意力重新计算。这可以在速度和资源利用率之间进行权衡，因为这些矩阵保存在 GPU VRAM 中。虽然管理这一点对于单个推理“线程”来说相当简单，但在考虑单个托管模型实例的并行推理或多个用户时，会出现许多复杂性。如何避免重新计算系统提示和少数示例的值？对于可能想要或不想继续聊天会话的用户，什么时候应该逐出缓存元素？ PagedAttention 及其流行的实现 vLLM 通过利用操作系统中经典分页的思想解决了这个问题，并已成为自托管多用户推理服务器的标准。

Resources: 资源：

+   [The KV Cache: Memory U](https://www.youtube.com/watch?v=80bIUggRJf4)sage in Transformers (video, Efficient NLP)
KV 缓存：Transformers 中的内存使用情况（视频、Efficient NLP）

+   [Fast LLM Serving with vLL](https://www.youtube.com/watch?v=5ZlavKF_98U)M and PagedAttention (video, Anyscale)
使用 vLLM 和 PagedAttention 快速LLM 提供服务（视频、Anyscale）

+   vLLM [blog post](https://blog.vllm.ai/2023/06/20/vllm.html) vLLM 博客文章

## Chapter 39: CPU Offloading 第39章：CPU卸载

Th[e pr](https://www.datacamp.com/tutorial/llama-cpp-tutorial)imary method used for running LLMs either partially or entirely on CPU ([vs. GPU) ](https://justine.lol/matmul/)is llama.cpp. See here for a high-level overview; llama.cpp serves as the backend for a number of popular self-hosted LLM tools/frameworks like LMStudio and Ollama. Here’s a blog post with some technical details about CPU performance improvements.

用于部分或全部在 CPU（相对于 GPU）上运行 LLMs 的主要方法是 llama.cpp。请参阅此处了解高级概述； llama.cpp 充当许多流行的自托管 LLM 工具/框架（例如 LMStudio 和 Ollama）的后端。这是一篇博客文章，其中包含有关 CPU 性能改进的一些技术细节。

# Section VII: Sub-Quadratic Context Scaling 第七节：第二次上下文缩放

> **Goal:** Survey approaches for avoiding the "quadratic scaling problem" faced by self-attention in Transformers.

> 目标：避免 Transformers 中自注意力所面临的“二次缩放问题”的调查方法。

>

> A major bottleneck in scaling both the size and context length of Transformers is the quadratic nature of attention, in which all pairs of token interactions are considered. Here we’ll look at a number of approaches for circumventing this, ranging from those which are currently widely used to those which are more exploratory (but promising) research directions.

> 缩放 Transformer 的大小和上下文长度的主要瓶颈是注意力的二次性质，其中考虑了所有标记交互对。在这里，我们将研究一些规避这一问题的方法，从目前广泛使用的方法到更具探索性（但有希望）的研究方向。

## Chapter 40: Sliding Window Attention 第40章：滑动窗口注意力

[Intro](https://arxiv.org/abs/2004.05150)duced in the “Longformer” paper, sliding window attention acts as a sub-quadratic drop-in replacement for standard attention which allows attending only to a sliding window (shocking, right?) of recent tokens/states rather than the entire context window, under the pretense that vectors for these states have already attended to earlier ones and thus have sufficient representational power to encode relevant pieces of early context. Due to its simplicity, it’s become one of the more widely adopted approaches towards sub-quadratic scaling, and is used in Mistral’s popular Mixtral-8x7B model (among others).

在“Longformer”论文中介绍，滑动窗口注意力充当标准注意力的次二次替代品，它允许仅关注最近标记/状态的滑动窗口（令人震惊，对吧？）而不是整个上下文窗口，假装这些状态的向量已经关注了早期的状态，因此具有足够的代表性能力来编码早期上下文的相关部分。由于其简单性，它已成为更广泛采用的次二次缩放方法之一，并用于 Mistral 流行的 Mixtral-8x7B 模型（等等）。

Resources: 资源：

+   “What is Sliding Window Attention?” (blog post by Stephen M. Walker)
“什么是滑动窗口注意力？” （斯蒂芬·M·沃克的博客文章）

+   “Sliding Window Attention” (blog post by Manoj Kumal)
“滑动窗口注意力”（Manoj Kumal 的博客文章）

+   [“Longformer: The Long-D](https://www.youtube.com/watch?v=_8KNb5iqblE)ocument Transformer” (video by Yannic Kilcher)
“Longformer：长文档 Transformer”（视频由 Yannic Kilcher 拍摄）

## Chapter 41: Ring Attention 第41章：请注意

Another modification to standard attention mechanisms, Ring Attention enables sub-quadratic full-context interaction via incremental computation with a “message-passing” structure, wherein “blocks” of context communicat[e with ](https://www.reddit.com/r/MachineLearning/comments/1arj2j8/d_gemini_1m10m_token_context_window_how/)each other over a series of steps rather than all at once. Within each block, the technique is essentially classical attention. While largely a research direction rather than standard technique at least within the open-weights world, Google’s Gemini is rumored to possibly be using Ring Attention in order to enable its million-plus-token context.

对标准注意力机制的另一种修改，环注意力通过增量计算与“消息传递”结构实现了次二次全上下文交互，其中上下文“块”通过一系列步骤而不是一次性全部相互通信。在每个块中，该技术本质上是经典的注意力。虽然至少在开放权重领域很大程度上是一个研究方向而不是标准技术，但有传言称谷歌的 Gemini 可能正在使用 Ring Attention 来实现其超过 100 万代币的上下文。

Resources: 资源：

+   [“B](https://medium.com/@tanuj22july/breaking-the-boundaries-understanding-context-window-limitations-and-the-idea-of-ring-attention-170e522d44b2)reaking the Boundaries: Understanding Context Window Limitations and the idea of Ring Attention” (blog post, Tanuj Sharma)
“打破界限：理解上下文窗口限制和环形注意力的想法”（博客文章，Tanuj Sharma）

+   [“Understanding Ring Attention: Building ](https://www.e2enetworks.com/blog/understanding-ring-attention-building-transformers-with-near-infinite-context)Transformers With Near-Infinite Context” (blog post, E2E Networks)
“理解环注意力：构建具有近乎无限上下文的 Transformers”（博客文章，E2E Networks）

+   “Ring Attention Explained” (video)
“环注意解释”（视频）

## Chapter 42: Linear Attention (RWKV) 第42章：线性注意力（RWKV）

The Receptance-Weighted Key Value (RWKV) architecture is a return to the general structure of RNN models (e.g LSTMs), with modifications to enable increased scaling and a linear attention-style mechanism which supports recurrent “unrolling” of its representation (allowing constant computation per output token as context length scales).

接收加权键值 (RWKV) 架构是对 RNN 模型（例如 LSTM）一般结构的回归，进行了修改以实现更大的扩展性和支持循环“展开”其表示的线性注意式机制（允许常数每个输出标记的计算作为上下文长度比例）。

Resources: 资源：

+   (Huggingface blog) （拥抱脸博客）

+   [“The RWKV language model: An RNN with ](https://johanwind.github.io/2023/03/23/rwkv_overview.html)the advantages of a transformer” - Pt. 1 (blog post, Johan Wind)
“RWKV 语言模型：具有 Transformer 优点的 RNN”-Pt. 1（博客文章，Johan Wind）

+   [“How the RWKV language ](https://johanwind.github.io/2023/03/23/rwkv_details.html)model works” - Pt. 2
“RWKV 语言模型如何工作” - Pt. 2

+   [“](https://www.youtube.com/watch?v=x8pW19wKfXQ)RWKV: Reinventing RNNs for the Transformer Era (Paper Explained)” (video, Yannic Kilcher)
“RWKV：为 Transformer 时代重塑 RNN（论文解释）”（视频，Yannic Kilcher）

## Chapter 43: Structured State Space Models 第43章：结构化状态空间模型

[Structured ](https://www.cartesia.ai/)State Space Models (SSMs) have become one of the most popular alternatives to Transformers in terms of current research focus, with several notable variants (S4, Hyena, Mamba/S6, Jamba, Mamba-2), but are somewhat notorious for their complexity. The architecture draws inspiration from classical control theory and linear time-invariant systems, with a number of optimizations to translate from continuous to discrete time, and to avoid dense representations of large matrices. They support both recurrent and convolutional representations, which allows efficiency gains both for training and at inference, and many variants require carefully-conditioned “hidden state matrix” representations to support “memorization” of context without needing all-pairs attention. SSMs also seem to be becoming more practical at scale, and have recently resulted in breakthrough speed improvements for high-quality text to speech (via Cartesia AI, founded by the inventors of SSMs).

就当前研究焦点而言，结构化状态空间模型 (SSM) 已成为 Transformer 最受欢迎的替代方案之一，有几个著名的变体（S4、Hyena、Mamba/S6、Jamba、Mamba-2），但因其复杂。该架构从经典控制理论和线性时不变系统中汲取灵感，通过大量优化将连续时间转换为离散时间，并避免大型矩阵的密集表示。它们支持循环表示和卷积表示，这可以提高训练和推理的效率，并且许多变体需要仔细调节的“隐藏状态矩阵”表示来支持上下文的“记忆”，而不需要全对注意力。 SSM 似乎也变得更加实用，并且最近在高质量文本到语音方面实现了突破性的速度提升（通过由 SSM 发明者创立的 Cartesia AI）。

The best explainer o[ut there is likely “The Anno](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-mamba-and-state)ta[ted S](https://www.youtube.com/watch?v=9dSkvxS2EB0)4”, focused on the S4 paper from which SSMs originated. The post “A Visual Guide to Mamba and State Space Models” is great for intuitions and visuals with slightly less math, and Yannic Kilcher has a nice video on SSMs as well.

最好的解释者可能是“The Annotated S4”，重点关注 SSM 起源的 S4 论文。 “Mamba 和状态空间模型的视觉指南”一文对于直觉和视觉效果非常有用，数学稍微少一点，Yannic Kilcher 也有一个关于 SSM 的精彩视频。

Recently, the Mamba authors released their follow-up “Mamba 2” paper, and their accompanying series of blog posts discusses some newly-uncovered connections between SSM representations and linear attention which may be interesting:

最近，Mamba 作者发布了他们的后续“Mamba 2”论文，及其随附的一系列博客文章讨论了 SSM 表示和线性注意力之间新发现的一些联系，这些联系可能很有趣：

+   [State Space Duality (Mamba-2](https://tridao.me/blog/2024/mamba2-part1-model/)) Part I - The Model
状态空间对偶 (Mamba-2) 第一部分 - 模型

+   [State Space Duality (Mamba-2) ](https://tridao.me/blog/2024/mamba2-part2-theory/)Part II - The Theory
状态空间对偶 (Mamba-2) 第二部分 - 理论

+   [S](https://tridao.me/blog/2024/mamba2-part3-algorithm/)tate Space Duality (Mamba-2) Part III - The Algorithm
状态空间对偶 (Mamba-2) 第三部分 - 算法

+   [State Space Duality (Mamba-2) P](https://tridao.me/blog/2024/mamba2-part4-systems/)art IV - The Systems
状态空间对偶 (Mamba-2) 第四部分 - 系统

# Section VIII: Generative Modeling Beyond Sequences 第八节：超越序列的生成建模

> **Goal:** Survey topics building towards generation of non-sequential content like images, from GANs to diffusion models.

> 目标：调查主题旨在生成非序列内容，例如图像，从 GAN 到扩散模型。

>

> So far, everything we’ve looked has been focused on text and sequence prediction with language models, but many other “generative AI” techniques require learning distributions with less of a sequential structure (e.g. images). Here we’ll examine a number of non-Transformer architectures for generative modeling, starting from simple mixture models and culminating with diffusion.

> 到目前为止，我们所关注的一切都集中在语言模型的文本和序列预测上，但许多其他“生成人工智能”技术需要学习具有较少序列结构（例如图像）的分布。在这里，我们将研究许多用于生成建模的非 Transformer 架构，从简单的混合模型开始，到扩散结束。

## Chapter 45: Distribution Modeling 第45章：分布建模

Recallin[g our fir](https://mitesh1612.github.io/blog/2020/08/30/naive-bayes)s[t gli](https://www.youtube.com/watch?v=O2L2Uv9pdDA)mpse of language models as simple bigram distributions, the most basic thing you can do in distributional modeling is just count co-occurrence probabilities in your dataset and repeat them as ground truth. This idea can be extended to conditional sampling or classification as “Naive Bayes” (blog post video), often one of the simplest algorithms covered in introductory machine learning courses.

回想一下我们第一次看到的语言模型是简单的二元分布，在分布式建模中你可以做的最基本的事情就是计算数据集中的共现概率并将它们重复作为基本事实。这个想法可以扩展到条件采样或分类，如“朴素贝叶斯”（博客文章视频），通常是机器学习入门课程中介绍的最简单的算法之一。

The next generative model students are often taught is the Gaussian Mixture Model and its Exp[ectation-](https://mpatacchiola.github.io/blog/2020/07/31/gaussian-mixture-models.html)Maximizati[on al](https://www.youtube.com/watch?v=DODphRRL79c)gorithm; Gaussian Mixture Models + Expectation-Maximization algorithm. This blog post and this video give decent overviews; he core idea here is assuming that data distributions can be approximated as a mixture of multivariate Gaussian distributions. GMMs can also be used for clustering if individual groups can be assumed to be approximately Gaussian.

学生经常教授的下一个生成模型是高斯混合模型及其期望最大化算法；高斯混合模型+期望最大化算法。这篇博文和这段视频给出了不错的概述；这里的核心思想是假设数据分布可以近似为多元高斯分布的混合。如果可以假设各个组近似为高斯分布，则 GMM 也可以用于聚类。

While these methods aren’t very effective at representing complex structures like images or language, related ideas will appear as components of some of the more advanced methods we’ll see.

虽然这些方法在表示图像或语言等复杂结构方面并不是很有效，但相关的想法将作为我们将看到的一些更高级方法的组成部分出现。

## Chapter 46: Variational Auto-Encoders 第 46 章：变分自动编码器

Auto-encoders and variational auto-encoders are widely used for learning compressed representations of data distributions, and can also be useful for “denoising” inputs, which will come into play when we discuss diffusion. Some nice resources:

自动编码器和变分自动编码器广泛用于学习数据分布的压缩表示，也可用于“去噪”输入，这将在我们讨论扩散时发挥作用。一些不错的资源：

+   [“Autoencoders”](https://www.deeplearningbook.org/contents/autoencoders.html) chapter in the “Deep Learning” book
《深度学习》书中的“自动编码器”章节

+   [blog post](https://genai-handbook.github.io/[https://lilianweng.github.io/posts/2018-08-12-vae/]) from Lilian Weng
Lilian Weng 的博客文章

+   [video](https://www.youtube.com/watch?v=9zKuYvjFFS8) from Arxiv Insights
视频来自 Arxiv Insights

+   [blog post](https://towardsdatascience.com/deep-generative-models-25ab2821afd3) from Prakash Pandey on both VAEs and GANs
Prakash Pandey 关于 VAE 和 GAN 的博客文章

## Chapter 47: Generative Adversarial Nets 第47章：生成对抗网络

The basic idea behind Generative Adversarial Networks (GANs) is to simulate a “game” between two neural nets — the Generator wants to create samples which are indistinguishable from real data by the Discriminator, who wants to identify the generated samples, and both nets are trained continuously until an equilibrium (or desired sample quality) is reached. Following from von Neumann’s minimax theorem for zero-sum games, you basically get a “theorem” promising that GANs succeed at learning distributions, if you assume that gradient descent finds global minimizers and allow both networks to grow arbitrarily large. Granted, neither of these are literally true in practice, but GANs do tend to be quite effective (although they’ve fallen out of favor somewhat in recent years, partly due to the instabilities of simultaneous training).

生成对抗网络（GAN）背后的基本思想是模拟两个神经网络之间的“游戏”——生成器想要创建与鉴别器的真实数据无法区分的样本，鉴别器想要识别生成的样本，并且两个网络都是持续训练，直到达到平衡（或所需的样本质量）。根据冯·诺依曼零和博弈的极小极大定理，如果你假设梯度下降找到全局极小值并允许两个网络增长任意大，你基本上会得到一个“定理”，承诺 GAN 能够成功学习分布。诚然，这些在实践中都不是真的，但 GAN 确实往往非常有效（尽管近年来它们在某种程度上已经失宠，部分原因是同时训练的不稳定）。

Resources: 资源：

+   [“Complete Guide to Generative A](https://blog.paperspace.com/complete-guide-to-gans/)dversarial Networks” from Paperspace
Paperspace 的“生成对抗网络完整指南”

+   [“](https://www.analyticsvidhya.com/blog/2021/10/an-end-to-end-introduction-to-generative-adversarial-networksgans/)Generative Adversarial Networks (GANs): End-to-End Introduction” by
“生成对抗网络（GAN）：端到端介绍”作者：

+   [Deep Learning, Ch. 20](https://www.deeplearningbook.org/contents/generative_models.html) - Generative Models (theory-focused)
深度学习，第 1 章20 - 生成模型（以理论为中心）

## Chapter 48: Conditional GANs 第 48 章：条件 GAN

Conditional GANs are where we’ll start going from vanilla “distribution learning” to something which more closely resembles interactive generative tools like DALL-E and Midjourney, incorporating text-image multimodality. A key idea is to learn “representations” (in the sense of text embeddings or autoencoders) which are more abstract and can be applied to either text or image inputs. For example, you could imagine training a vanilla GAN on (image, caption) pairs by embedding the text and concatenating it with an image, which could then learn this joint distribution over images and captions. Note that this implicitly involves learning conditional distributions if part of the input (image or caption) is fixed, and this can be extended to enable automatic captioning (given an image) or image generation (given a caption). There a number of variants on this setup with differing bells and whistles. The VQGAN+CLIP architecture is worth knowing about, as it was a major popular source of early “AI art” generated from input text.

我们将从条件 GAN 开始，从普通的“分布学习”转向更类似于 DALL-E 和 Midjourney 等交互式生成工具的东西，并结合文本图像多模态。一个关键的想法是学习“表示”（在文本嵌入或自动编码器的意义上），它更抽象，可以应用于文本或图像输入。例如，您可以想象通过嵌入文本并将其与图像连接来在（图像，标题）对上训练普通 GAN，然后它可以学习图像和标题上的联合分布。请注意，如果输入的一部分（图像或标题）是固定的，这隐含地涉及学习条件分布，并且可以扩展以启用自动标题（给定图像）或图像生成（给定标题）。此设置有多种变体，具有不同的功能。 VQGAN+CLIP 架构值得了解，因为它是从输入文本生成的早期“AI 艺术”的主要流行来源。

Resources: 资源：

+   [“](https://blog.paperspace.com/conditional-generative-adversarial-networks/)Implementing Conditional Generative Adversarial Networks” blog from Paperspace
Paperspace 的“实施条件生成对抗网络”博客

+   [“Conditional Generative Adversarial Network](https://towardsdatascience.com/cgan-conditional-generative-adversarial-network-how-to-gain-control-over-gan-outputs-b30620bd0cc8) — How to Gain Control Over GAN Outputs” by Saul Dobilas
“条件生成对抗网络 — 如何控制 GAN 输出”作者：Saul Dobilas

+   “The Illustrated VQGAN” by LJ Miranda
LJ Miranda 的“VQGAN 插图”

+   [“](https://www.youtube.com/watch?v=Ih4qOakCZD4)Using Deep Learning to Generate Artwork with VQGAN-CLIP” talk from Paperspace
Paperspace 的“使用深度学习通过 VQGAN-CLIP 生成艺术品”演讲

## Chapter 49: Normalizing Flows 第49章：标准化流程

The aim of normalizing flows is to learn a series of invertible transformations between Gaussian noise and an output distribution, avoiding the need for “simultaneous training” in GANs, and have been popular for generative modeling in a number of domains. Here I’ll just recommend “Flow-based Deep Generative Models” from Lilian Weng as an overview — I haven’t personally gone very deep on normalizing flows, but they come up enough that they’re probably worth being aware of.

归一化流的目的是学习高斯噪声和输出分布之间的一系列可逆变换，避免 GAN 中“同时训练”的需要，并且在许多领域的生成建模中很受欢迎。在这里，我只推荐 Lilian Weng 的“基于流的深度生成模型”作为概述——我个人并没有深入探讨标准化流，但它们出现的次数足够多，可能值得关注。

## Chapter 50: Diffusion Models 第50章：扩散模型

One of the cen[t](https://medium.com/@gitau_am/a-friendly-introduction-to-denoising-diffusion-probabilistic-models-cc76b8abef25)ral ideas behind diffusion model[s (like S](https://huggingface.co/blog/lora)tableDiffusion) is iterative guided application of denoising operations, refining random noise into something that increasingly resembles an image. Diffusion originates from the worlds of stochastic differential equations and statistical physics — relating to the “Schrodinger bridge” problem and optimal transport for probability distributions — and a fair amount of math is basically unavoidable if you want to understand the whole picture. For a relatively soft introduction, see “A friendly Introduction to Denoising Diffusion Probabilistic Models” by Antony Gitau. If you’re up for some more math, check out “What are Diffusion Models?” for more of a deep dive. If you’re more interested in code and pictures (but still some math), see “The Annotated Diffusion Model” from Hugging Face, as well as this Hugging Face blog post on LoRA finetuning for diffusion models.

扩散模型（如 StableDiffusion）背后的核心思想之一是迭代引导去噪操作的应用，将随机噪声细化为越来越类似于图像的东西。扩散起源于随机微分方程和统计物理学的世界——与“薛定谔桥”问题和概率分布的最优传输有关——如果你想了解整个情况，大量的数学基本上是不可避免的。对于相对软的介绍，请参阅 Antony Gitau 的“去噪扩散概率模型的友好介绍”。如果您想了解更多数学知识，请查看“什么是扩散模型？”进行更深入的研究。如果您对代码和图片更感兴趣（但仍然对数学感兴趣），请参阅 Hugging Face 的“带注释的扩散模型”，以及这篇有关 LoRA 扩散模型微调的 Hugging Face 博客文章。

# Section IX: Multimodal Models 第九节：多式联运模型

> **Goal:** Survey how models can use multiple modalities of input and output (text, audio, images) simultaneously.

> 目标：调查模型如何同时使用多种输入和输出模式（文本、音频、图像）。

>

> Note: [This is the set of topics with](https://huyenchip.com/2023/10/10/multimodal.html) which I’m the least familiar, but wanted to include for completeness. I’ll be lighter on commentary and recommendations here, and will return to add more when I think I have a tighter story to tell. The post “Multimodality and Large Multimodal Models (LMMs)” by Chip Huyen is a nice broad overview (or “How Multimodal LLMs Work” by Kevin Musgrave for a more concise one).

> 注意：这是我最不熟悉的一组主题，但为了完整起见，我想将其包括在内。我会在这里减少评论和建议，当我认为我有更紧凑的故事要讲时，我会回来添加更多内容。 Chip Huyen 的文章“多模态和大型多模态模型 (LMM)”提供了很好的广泛概述（或者 Kevin Musgrave 的“多模态LLMs如何工作”以获得更简洁的概述）。

## Chapter 51: Tokenization Beyond Text 第51章：超越文本的标记化

The idea[ of token](https://www.assemblyai.com/blog/recent-developments-in-generative-ai-for-audio/)ization isn’t only relevant to text; audio, images, and video can also be “tokenized” for use in Transformer-style archictectures, and there a range of tradeoffs to consider between tokenization and other methods like convolution. The next two sections will look more into visual inputs; this blog post from AssemblyAI touches on a number of relevant topics for audio tokenization and representation in sequence models, for applications like audio generation, text-to-speech, and speech-to-text.

标记化的想法不仅与文本相关，而且与文本相关。音频、图像和视频也可以被“标记化”以用于 Transformer 风格的架构，并且在标记化和卷积等其他方法之间需要考虑一系列权衡。接下来的两节将更多地研究视觉输入； AssemblyAI 的这篇博文涉及音频标记化和序列模型表示的许多相关主题，适用于音频生成、文本转语音和语音转文本等应用。

## Chapter 52: VQ-VAE  第52章：VQ-VAE

The VQ-VAE architecture has become quite popular for image generation in recent years, and underlies at least the earlier versions of DALL-E.

近年来，VQ-VAE 架构在图像生成方面变得非常流行，并且至少是 DALL-E 早期版本的基础。

Resources: 资源：

+   [“Understanding VQ-VAE (DALL](https://mlberkeley.substack.com/p/vq-vae)-E Explained Pt. 1)” from the Machine Learning @ Berkeley blog
“理解 VQ-VAE（DALL-E 解释第 1 部分）”来自 Machine Learning @ Berkeley 博客

+   [“How is it so good ? (DALL](https://mlberkeley.substack.com/p/dalle2)-E Explained Pt. 2)”
“怎么这么好？ （DALL-E 解释第 2 部分）”

+   [“](https://shashank7-iitd.medium.com/understanding-vector-quantized-variational-autoencoders-vq-vae-323d710a888a)Understanding Vector Quantized Variational Autoencoders (VQ-VAE)” by Shashank Yadav
“理解矢量量化变分自编码器 (VQ-VAE)”作者：Shashank Yadav

## Chapter 53: Vision Transformers 第53章：视觉变形金刚

Visio[n Trans](https://d2l.ai/chapter_attention-mechanisms-and-transformers/vision-transformer.html)formers extend the Transformer architecture to domains like image and video, and have become popular for applications like self-driving cars as well as for multimodal LLMs. There’s a nice section in the d2l.ai book about how they work.

Vision Transformers 将 Transformer 架构扩展到图像和视频等领域，并在自动驾驶汽车以及多模式 LLMs 等应用中变得流行。 d2l.ai 书中有一个很好的章节介绍了它们的工作原理。

[“Generalized Vis](https://lilianweng.github.io/posts/2022-06-09-vlm/)ual Language Models” by Lilian Weng discusses a range of different approaches and for training multimodal Transformer-style models.

Lilian Weng 的“广义视觉语言模型”讨论了一系列不同的方法以及用于训练多模式 Transformer 风格模型的方法。

The post “Guide to Vision Language Models” from Encord’s blog overviews several architectures for mixing text and vision.

Encord 博客中的文章“视觉语言模型指南”概述了混合文本和视觉的几种架构。

If[ you’](https://arxiv.org/abs/2403.09611)re interested in practical large-scale training advice for Vision Transformers, the MM1 paper from Apple examines several architecture and data tradeoffs with experimental evidence.

如果您对 Vision Transformers 的实用大规模训练建议感兴趣，Apple 的 MM1 论文通过实验证据检查了几种架构和数据权衡。

[“Multimodal Neurons in Artific](https://distill.pub/2021/multimodal-neurons/)ial Neural Networks” from Distill.pub has some very fun visualizations of concept representations in multimodal networks.

Distill.pub 的“人工神经网络中的多模态神经元”对多模态网络中的概念表示进行了一些非常有趣的可视化。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>