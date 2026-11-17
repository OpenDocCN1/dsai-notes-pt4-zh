
# StrateChery对于DeepSeek的全方位评价

> 来源：[https://xcn2d971vuw4.feishu.cn/docx/X1oWdOcrao7kECxoRDWcdUxYnnU](https://xcn2d971vuw4.feishu.cn/docx/X1oWdOcrao7kECxoRDWcdUxYnnU)

原文链接：[DeepSeek FAQ](https://stratechery.com/2025/deepseek-faq/)

> **以下是总结内容：**

> 这是 1 月 27 日，星期一。你为什么还没写关于 DeepSeek 的内容？

> 我已经写了！上周二我写过关于 R1 的内容。

> 我完全忘了这件事。

> 这是我的责任。我仍然坚持当时的观点，包括我强调的两个最重要的发现（通过纯强化学习获得的涌现链式思维，以及蒸馏的强大力量），我还提到了低成本（这一点在 Sharp Tech 中有详细展开）以及芯片禁令的影响。但我完全未能预见到这一消息对整体 AI 讨论的广泛影响，尤其是在美中关系方面。

### 这类误判是否有先例？

> 有的。2023 年 9 月，华为发布了 Mate 60 Pro，搭载了由中芯国际制造的 7nm 芯片。对密切关注该领域的人来说，这并不意外：中芯国际早在一年前就已经制造了 7nm 芯片，而台积电更是早在 DUV 光刻技术时代就已经量产 7nm（后来的 7nm 才开始使用 EUV）。英特尔甚至更早就用 DUV 工艺制造了等效于台积电 7nm 的 10nm 芯片，虽然其良率不佳。因此，中芯国际在现有设备上生产 7nm 芯片——尤其是当不考虑良率时——并不令人意外，至少对我来说如此。

> 然而，我完全未能预见的是华盛顿方面的过度反应。这一事件引发了对芯片禁令的大幅扩展，最终导致拜登政府将芯片销售转变为基于许可的体系。这种反应的根源在于许多人对芯片制造的复杂性缺乏理解，因此完全被华为 Mate 60 Pro 的发布打了个措手不及。我感觉，过去 72 小时里发生的事情有些类似：DeepSeek 取得的成就——以及他们尚未实现的内容——本身的重要性甚至比市场的反应次要，而这种反应反映了人们原本的认知偏差。

### DeepSeek 具体发布了什么？

> 本次舆论爆发的直接导火索是 R1，这是一款类似于 OpenAI 的 o1 的推理模型。然而，许多引发关注的信息（例如 DeepSeek 的训练成本）实际上早在圣诞节期间的 V3 发布时就已披露。此外，支撑 V3 的诸多技术突破甚至可以追溯到去年 1 月的 V2 版本。

### DeepSeek V2 模型的意义是什么？

> DeepSeek-V2 引入了两个重要的突破：`DeepSeekMoE` 和 `DeepSeekMLA`。

1. `DeepSeekMoE`（专家混合模型）：

   + 传统模型（如 GPT-3.5）在训练和推理时都会激活整个模型，而实际上并非所有部分都需要参与计算。

   + MoE 技术将模型划分为多个“专家”，仅激活必要的部分。GPT-4 就是一种 MoE 模型，据推测它包含 16 个专家，每个约 1100 亿参数。

   + `DeepSeekMoE` 进一步优化了这一概念，引入了更细粒度的专家划分，并实现了训练时的负载均衡和路由优化，使得训练过程也更加高效。

1. `DeepSeekMLA`（多头潜变量注意力机制）：

   + 语言模型的推理成本主要受限于内存需求，即需要加载整个模型和上下文窗口。

   + 传统情况下，每个 token 需要存储键-值对，占用大量内存。

   + `DeepSeekMLA` 允许对键-值存储进行压缩，显著降低推理过程中的内存占用。

> 这些突破在 V3 中得到了进一步发展。

### 为什么 V3 训练成本如此之低？

> DeepSeek 声称 V3 训练耗时 278.8 万 H800 GPU 小时，按照每小时 2 美元计算，总成本仅为 557.6 万美元。这一数字看似难以置信，但如果理解 V3 的架构，就能明白其合理性。

+ V3 采用了 MoE 技术，虽然总参数量高达 6710 亿，但每个 token 仅激活 37 亿参数，对应计算量约 3333 亿 FLOPs。

+ DeepSeek 通过 FP8 计算精度优化了计算效率，使得 2048 颗 H800 GPU 每秒可处理 3.97 exaFLOPs。

+ 训练数据规模为 14.8 万亿 token，经过计算后可验证 278.8 万 GPU 小时的计算量确实足够完成训练。

> 不过，需要注意的是，这个成本仅指最终训练运行的开销，不包括前期的研究和实验。

### R1 的突破性在哪里？

> R1 是一款类似 OpenAI o1 的推理模型，特别擅长编码、数学和逻辑任务。但真正值得关注的是 R1-Zero，它完全通过强化学习（而非人类反馈）自主学习了推理能力。

> DeepSeek 采用了 AlphaGo 式的强化学习策略，让模型自己演化推理能力。他们给模型一系列数学、代码和逻辑问题，并设定两个奖励函数：

1. 计算正确答案的奖励。

1. 采用链式推理过程的奖励。

> 通过这种方式，模型自主发展出了复杂的推理能力，包括 DeepSeek 所描述的“顿悟时刻（Aha Moments）”。

### AI 未来的方向

#### AGI 是否接近？

AI 训练成本的急剧下降、自主推理能力的涌现，标志着 AGI 可能比预想更快到来。

#### AI 监管的未来

过去，美国主导的 AI 监管试图封锁先进模型的开源，而 DeepSeek 证明了封锁的无效。

美国可能面临是否扩大芯片禁令的选择。

#### AI 商业模式的变化

AI 训练和推理成本的降低，将加速 AI 技术的普及。

未来的 AI 市场可能由成本结构最优的企业主导，而非简单的技术领先者。

> DeepSeek 让世界进入了 AI 的新纪元，而美国和全球科技产业正面临一次前所未有的转变。

## 英文原文

It’s Monday, January 27. Why haven’t you written about DeepSeek yet?

I did! I wrote about R1 last Tuesday.

I totally forgot about that.

I take [responsibility](https://sharptech.fm/member/episode/the-end-of-open-ai-and-microsoft-risks-and-rationale-of-the-stargate-project-deep-seek-r-1-and-bitter-lessons-for-the-future). I stand by the post, including the two biggest takeaways that I highlighted (emergent chain-of-thought via pure reinforcement learning, and the power of distillation), and I mentioned the low cost (which I expanded on in Sharp Tech) and chip ban implications, but those observations were too localized to the current state of the art in AI. What I totally failed to anticipate were the broader implications this news would have to the overall meta-discussion, particularly in terms of the U.S. and China.

### Is there precedent for such a miss?

There is. In [September](https://www.semianalysis.com/p/chinas-smic-is-shipping-7nm-foundry) 2023 Huawei announced the Mate 60 Pro with a SMIC-manufactured 7nm chip. The existence of this chip wasn’t a surprise for those paying close attention: SMIC had made a 7nm chip a year earlier (the existence of which I had noted even earlier than that), and TSMC had shipped 7nm chips in volume using nothing but DUV lithography (later iterations of 7nm were the first to use EUV). Intel had also made 10nm (TSMC 7nm equivalent) chips years earlier using nothing but DUV, but couldn’t do so with profitable yields; the idea that SMIC could ship 7nm chips using their existing equipment, particularly if they didn’t care about yields, wasn’t remotely surprising — to me, anyways.

[What I totally failed to anticipate was the](https://stratechery.com/2025/ai-diffusion-regulations-closing-loopholes-the-objections/) overwrought reaction in Washington D.C. The dramatic expansion in the chip ban that culminated in the Biden administration transforming chip sales to a permission-based structure was downstream from people not understanding the intricacies of chip production, and being totally blindsided by the Huawei Mate 60 Pro. I get the sense that something similar has happened over the last 72 hours: the details of what DeepSeek has accomplished — and what they have not — are less important than the reaction and what that reaction says about people’s pre-existing assumptions.

### So what did DeepSeek announce?

The most proximate announcement to this weekend’s meltdown was R1, a reasoning model that is similar to OpenAI’s o1. However, many of the revelations that contributed to the meltdown — including DeepSeek’s training costs — actually accompanied the V3 announcement over Christmas. Moreover, many of the breakthroughs that undergirded V3 were actually revealed with the release of the V2 model last January.

### Is this model naming convention the greatest crime that OpenAI has committed?

Second greatest; we’ll get to the greatest momentarily.

### Let’s work backwards: what was the V2 model, and why was it important?

The DeepSeek-V2 model introduced two important breakthroughs: `DeepSeekMoE` and `DeepSeekMLA`. The “MoE” in `DeepSeekMoE` refers to “mixture of experts”. Some models, like GPT-3.5, activate the entire model during both training and inference; it turns out, however, that not every part of the model is necessary for the topic at hand. MoE splits the model into multiple “experts” and only activates the ones that are necessary; GPT-4 was a MoE model that was believed to have 16 experts with approximately 110 billion parameters each.

`DeepSeekMoE`, as implemented in V2, introduced important innovations on this concept, including differentiating between more finely-grained specialized experts, and shared experts with more generalized capabilities. Critically, `DeepSeekMoE` also introduced new approaches to load-balancing and routing during training; traditionally MoE increased communications overhead in training in exchange for efficient inference, but DeepSeek’s approach made training more efficient as well.

`DeepSeekMLA` was an even bigger breakthrough. One of the biggest limitations on inference is the sheer amount of memory required: you both need to load the model into memory and also load the entire context window. Context windows are particularly expensive in terms of memory, as every token requires both a key and corresponding value; `DeepSeekMLA`, or multi-head latent attention, makes it possible to compress the key-value store, dramatically decreasing memory usage during inference.

### I’m not sure I understood any of that.

The key implications of these breakthroughs — and the part you need to understand — only became apparent with V3, which added a new approach to load balancing (further reducing communications overhead) and multi-token prediction in training (further densifying each training step, again reducing overhead): V3 was shockingly cheap to train. DeepSeek claimed the model training took 2,788 thousand H800 GPU hours, which, at a cost of $2/GPU hour, comes out to a mere $5.576 million.

That seems impossibly low.

DeepSeek is clear that these costs are only for [the final training](https://arxiv.org/html/2412.19437v1) run, and exclude all other expenses; from the V3 paper:



# DeepSeek-V3 Training Costs and Technical Analysis

> Lastly, we emphasize again the economical training costs of `DeepSeek-V3`, summarized in `Table 1`, achieved through our optimized co-design of algorithms, frameworks, and hardware. During the pre-training stage, training `DeepSeek-V3` on each trillion tokens requires only 180K `H800` GPU hours, i.e., 3.7 days on our cluster with 2048 `H800` GPUs. Consequently, our pre-training stage is completed in less than two months and costs 2664K GPU hours. Combined with 119K GPU hours for the context length extension and 5K GPU hours for post-training, `DeepSeek-V3` costs only 2.788M GPU hours for its full training. Assuming the rental price of the `H800` GPU is $2 per GPU hour, our total training costs amount to only $5.576M. Note that the aforementioned costs include only the official training of `DeepSeek-V3`, excluding the costs associated with prior research and ablation experiments on architectures, algorithms, or data.

## Can DeepSeek Be Replicated for $5.576 Million?

So no, you can’t replicate DeepSeek the company for $5.576 million.

I still don’t believe that number.

Actually, the burden of proof is on the doubters, at least once you understand the `V3` architecture. Remember that bit about `DeepSeekMoE`: `V3` has 671 billion parameters, but only 37 billion parameters in the active expert are computed per token; this equates to 333.3 billion `FLOPs` of compute per token. Here I should mention another DeepSeek innovation: while parameters were stored with `BF16` or `FP32` precision, they were reduced to `FP8` precision for calculations; 2048 `H800` GPUs have a capacity of 3.97 exoflops, i.e. 3.97 billion billion `FLOPS`. The training set, meanwhile, consisted of 14.8 trillion tokens; once you do all of the math it becomes apparent that 2.8 million `H800` hours is sufficient for training `V3`. Again, this was just the final run, not the total cost, but it’s a plausible number.

## Was This a Violation of the Chip Ban?

Scale AI CEO Alexandr Wang said they have 50,000 `H100`s.

I don’t know where Wang got his info[rmation; I’m guessing](https://x.com/dylan522p/status/1859302712803807696) he’[s referring to](https://stratechery.com/2022/china-chip-ban-clarifications-nvidias-a800-china-intel-and-tower/) this November 2024 tweet from Dylan Patel, which says that DeepSeek had “over 50k Hopper GPUs”. `H800`s, however, are Hopper GPUs, they just have much more constrained memory bandwidth than `H100`s because of U.S. sanctions.

Here’s the thing: a huge number of the innovations I explained above are about overcoming the lack of memory bandwidth implied in using `H800`s instead of `H100`s. Moreover, if you actually did the math on the previous question, you would realize that DeepSeek actually had an excess of computing; that’s because DeepSeek actually programmed 20 of the 132 processing units on each `H800` specifically to manage cross-chip communications. This is actually impossible to do in `CUDA`. DeepSeek engineers had to drop down to `PTX`, a low-level instruction set for Nvidia GPUs that is basically like assembly language. This is an insane level of optimization that only makes sense if you are using `H800`s.

Meanwhile, DeepSeek also makes their models available for inference: that requires a whole bunch of GPUs above-and-beyond whatever was used for training.

## Is V3 a Leading-Edge Model?

Nope. `H100`s were prohibited by the chip ban, but not `H800`s. Everyone assumed that training leading edge models required more interchip memory bandwidth, but that is exactly what DeepSeek optimized both their model structure and infrastructure around.

Again, just to emphasize this point, all of the decisions DeepSeek made in the design of this model only make sense if you are constrained to the `H800`; if DeepSeek had access to `H100`s, they probably would have used a larger training cluster with much fewer optimizations specifically focused on overcoming the lack of bandwidth.

## What Is Distillation?

It’s definitely competitive with OpenAI’s `4o` and Anthropic’s `Sonnet-3.5`, and appears to be better than `Llama`’s biggest model. What does seem likely is that DeepSeek was able to distill those models to give `V3` high quality tokens to train on.

Distillation is a means of extracting understanding from another model; you can send inputs to the teacher model and record the outputs, and use that to train the student model. This is how you get models like `GPT-4 Turbo` from `GPT-4`. Distillation is easier for a company to do on its own models, because they have full access, but you can still do distillation in a somewhat more unwieldy way via `API`, or even, if you get creative, via chat clients.

Distillation obviously violates the terms of service of various models, but the only way to stop it is to actually cut off access, via IP banning, rate limiting, etc. It’s assumed to be widespread in terms of model training, and is why there are an ever-increasing number of models converging on `GPT-4o` quality. This doesn’t mean that we know for a fact that DeepSeek distilled `4o` or `Claude`, but frankly, it would be odd if they didn’t.

## Is Distillation Terrible for Leading-Edge Models?

Distillation seems terrible for leading edge models.

It is! On the positive side, OpenAI and Anthropic and Google are almost certainly using distillation to optimize the models they use for inference for their consumer-facing apps; on the negative side, they are effectively bearing the entire cost of training the leading edge, while everyone else is free-riding on their investment.

Indeed, this is [probably](https://stratechery.com/2025/stargate-the-end-of-microsoft-and-openai/) the core economic factor undergirding the slow divorce of Microsoft and OpenAI. Microsoft is interested in providing inference to its customers, but much less enthused about funding $100 billion data centers to train leading edge models that are likely to be commoditized long before that $100 billion is depreciated.

## Is This Why Big Tech Stock Prices Are Down?

In the long run, model commoditization and cheaper inference — which DeepSeek has also demonstrated — is great for Big Tech. A world where Microsoft gets to provide inference to its customers for a fraction of the cost means that Microsoft has to spend less on data centers and `GPUs`, or, just as likely, sees dramatically higher usage given that inference is so much cheaper. Another big winner is Amazon: `AWS` has by-and-large failed to make their own quality model, but that doesn’t matter if there are very high quality open source models that they can serve at far lower costs than expected.

Apple is also a big winner. Dramatically decreased memory requirements for inference make edge inference much more viable, and Apple has the best hardware for exactly that. `Apple Silicon` uses unified memory, which means that the `CPU`, `GPU`, and `NPU` (neural processing unit) have access to a shared pool of memory; this means that Apple’s high-end hardware actually has the best consumer chip for inference (Nvidia gaming `GPUs` max out at 32GB of `VRAM`, while Apple’s chips go up to 192 GB of `RAM`).

Meta, meanwhile, is the biggest winner of all. I already laid out last fall how every aspect of Meta’s business benefits from AI; a big barrier to realizing that vision is the cost of inference, which means that dramatically cheaper inference — and dramatically cheaper training, given the need for Meta to stay on the cutting edge — makes that vision much more achievable.

Google, meanwhile, is probably in worse shape: a world of decreased hardware requirements lessens the relative advantage they have from `TPUs`. More importantly, a world of zero-cost inference increases the viability and likelihood of products that displace search; granted, Google gets lower costs as well, but any change from the status quo is probably a net negative.

I asked why the stock prices are down; you just painted a positive picture!

My picture is of the long run; today is the short run, and it seems likely the market is working through the shock of `R1`’s existence.

## What About R1?

Wait, you haven’t even talked about `R1` yet.



# R1

`R1` is a reasoning model like OpenAI’s `o1`. It has the ability to think through a problem, producing much higher quality results, particularly in areas like coding, math, and logic (but I repeat myself).

## Is This More Impressive Than V3?

Actually, the reason why I spent so much time on `V3` is that that was the model that actually demonstrated a lot of the dynamics that seem to be generating so much surprise and controversy. `R1` is notable, however, because `o1` stood alone as the only reasoning model on the market, and the clearest sign that OpenAI was the market leader.

`R1` undoes the `o1` mythology in a couple of important ways. First, there is the fact that it exists. OpenAI does not have some sort of special sauce that can’t be replicated. Second, `R1` — like all of DeepSeek’s models — has open weights (the problem with saying “open source” is that we don’t have the data that went into creating it). This means that instead of paying OpenAI to get reasoning, you can run `R1` on the server of your choice, or even locally, at dramatically lower cost.

## How Did DeepSeek Make R1?

DeepSeek actually made two models: `R1` and `R1-Zero`. I actually think that `R1-Zero` is the bigger deal; as I noted above, it was my biggest focus in last Tuesday’s Update:

> `R1-Zero`, though, is the [bigger deal in my mind](https://github.com/deepseek-ai/DeepSeek-R1/blob/main/DeepSeek_R1.pdf). From the paper:

> In this paper, we take the first step toward improving language model reasoning capabilities using pure reinforcement learning (RL). Our goal is to explore the potential of LLMs to develop reasoning capabilities without any supervised data, focusing on their self-evolution through a pure RL process. Specifically, we use `DeepSeek-V3-Base` as the base model and employ `GRPO` as the RL framework to improve model performance in reasoning. During training, `DeepSeek-R1-Zero` naturally emerged with numerous powerful and interesting reasoning behaviors. After thousands of RL steps, `DeepSeek-R1-Zero` exhibits super performance on reasoning benchmarks. For instance, the `pass@1` score on AIME 2024 increases from 15.6% to 71.0%, and with majority voting, the score further improves to 86.7%, matching the performance of `OpenAI-o1-0912`.

> Reinforcement learning is a technique where a machine learning model is given a bunch of data and a reward function. The classic example is AlphaGo, where DeepMind gave the model the rules of Go with the reward function of winning the game, and then let the model figure everything else on its own. This famously ended up working better than other more human-guided techniques.

> LLMs to date, however, have relied on reinforcement learning with human feedback; humans are in the loop to help guide the model, navigate difficult choices where rewards aren’t obvious, etc. `RLHF` was the key innovation in transforming GPT-3 into ChatGPT, with well-formed paragraphs, answers that were concise and didn’t trail off into gibberish, etc.

> `R1-Zero`, however, drops the HF part — it’s just reinforcement learning. DeepSeek gave the model a set of math, code, and logic questions, and set two reward functions: one for the right answer, and one for the right format that utilized a thinking process. Moreover, the technique was a simple one: instead of trying to evaluate step-by-step (process supervision), or doing a search of all possible answers (a la AlphaGo), DeepSeek encouraged the model to try several different answers at a time and then graded them according to the two reward functions.

> What emerged is a model that developed reasoning and chains-of-thought on its own, including what DeepSeek called “Aha Moments”:

> A particularly intriguing phenomenon observed during the training of `DeepSeek-R1-Zero` is the occurrence of an “aha moment”. This moment, as illustrated in Table 3, occurs in an intermediate version of the model. During this phase, `DeepSeek-R1-Zero` learns to allocate more thinking time to a problem by reevaluating its initial approach. This behavior is not only a testament to the model’s growing reasoning abilities but also a captivating example of how reinforcement learning can lead to unexpected and sophisticated outcomes.

> This moment is not only an “aha moment” for the model but also for the researchers observing its behavior. It underscores the power and beauty of reinforcement learning: rather than explicitly teaching the model on how to solve a problem, we simply provide it with the right incentives, and it autonomously develops advanced problem-solving strategies. The “aha moment” serves as a powerful reminder of the potential of RL to unlock new levels of intelligence in artificial systems, paving the way for more autonomous and adaptive models in the future.

> This is one of the most powerful affirmations yet of The Bitter Lesson: you don’t need to teach the AI how to reason, you can just give it enough compute and data and it will teach itself!

> Well, almost: `R1-Zero` reasons, but in a way that humans have trouble understanding. Back to the introduction:

> However, `DeepSeek-R1-Zero` encounters challenges such as poor readability, and language mixing. To address these issues and further enhance reasoning performance, we introduce `DeepSeek-R1`, which incorporates a small amount of cold-start data and a multi-stage training pipeline. Specifically, we begin by collecting thousands of cold-start data to fine-tune the `DeepSeek-V3-Base` model. Following this, we perform reasoning-oriented RL like `DeepSeek-R1-Zero`. Upon nearing convergence in the RL process, we create new `SFT` data through rejection sampling on the RL checkpoint, combined with supervised data from `DeepSeek-V3` in domains such as writing, factual QA, and self-cognition, and then retrain the `DeepSeek-V3-Base` model. After fine-tuning with the new data, the checkpoint undergoes an additional RL process, taking into account prompts from all scenarios. After these steps, we obtained a checkpoint referred to as `DeepSeek-R1`, which achieves performance on par with `OpenAI-o1-1217`.

> [This sounds a lot](https://stratechery.com/2024/openais-new-model-how-o1-works-scaling-inference/) like what OpenAI did for `o1`: DeepSeek started the model out with a bunch of examples of chain-of-thought thinking so it could learn the proper format for human consumption, and then did the reinforcement learning to enhance its reasoning, along with a number of editing and refinement steps; the output is a model that appears to be very competitive with `o1`.

Here again it seems plausible that DeepSeek benefited from distillation, particularly in terms of training `R1`. That, though, is itself an important takeaway: we have a situation where AI models are teaching AI models, and where AI models are teaching themselves. We are watching the assembly of an AI takeoff scenario in realtime.

## So Are We Close to AGI?

It definitely seems like it. This also explains why Softbank (and whatever investors Masayoshi Son brings together) would provide the funding for OpenAI that Microsoft will not: the belief that we are reaching a takeoff point where there will in fact be real returns towards being first.

## But Isn’t `R1` Now in the Lead?

I don’t think so; this has been overstated. `R1` is competitive with `o1`, although there do seem to be some holes in its capability that point towards some amount of distillation from `o1-Pro`. OpenAI, meanwhile, has demonstrated `o3`, a far more powerful reasoning model. DeepSeek is absolutely the leader in efficiency, but that is different than being the leader overall.

## So Why Is Everyone Freaking Out?



# DeepSeek, Nvidia, and the Chip Ban

## China’s AI Progress and DeepSeek’s Efficiency

I think there are multiple factors. First, there is the shock that China has caught up to the leading U.S. labs, despite the widespread assumption that China isn’t as good at software as the U.S.. This is probably the biggest thing I missed in my surprise over the reaction. The reality is that China has an extremely proficient software industry generally, and a very good track record in AI model building specifically.

Second is the low training cost for V3, and DeepSeek’s low inference costs. This part was a big surprise for me as well, to be sure, but the numbers are plausible. This, by extension, probably has everyone nervous about Nvidia, which obviously has a big impact on the market.

Third is the fact that DeepSeek pulled this off despite the chip ban. Again, though, while there are big loopholes in the chip ban, it seems likely to me that DeepSeek accomplished this with legal chips.

## Nvidia’s Moats and Challenges

I own Nvidia! Am I screwed?

There are real challenges this news presents to the Nvidia story. Nvidia has two big moats:

+ CUDA is the language of choice for anyone programming these models, and CUDA only works on Nvidia chips.
+ Nvidia has a massive lead in terms of its ability to combine multiple chips together into one large virtual GPU.

These two moats work together. I noted above that if DeepSeek had access to H100s they probably would have used a larger cluster to train their model, simply because that would have been the easier option; the fact they didn’t, and were bandwidth constrained, drove a lot of their decisions in terms of both model architecture and their training infrastructure. Just look at the U.S. labs: they haven’t spent much time on optimization because Nvidia has been aggressively shipping ever more capable systems that accommodate their needs. The route of least resistance has simply been to pay Nvidia. DeepSeek, however, just demonstrated that another route is available: heavy optimization can produce remarkable results on weaker hardware and with lower memory bandwidth; simply paying Nvidia more isn’t the only way to make better models.

## Factors Still Favoring Nvidia

That noted, there are three factors still in Nvidia’s favor. First, how capable might DeepSeek’s approach be if applied to H100s, or upcoming GB100s? Just because they found a more efficient way to use compute doesn’t mean that more compute wouldn’t be useful. Second, lower inference costs should, in the long run, drive greater usage. Microsoft CEO Satya Nadella, in a late night tweet almost assuredly directed at the market, said exactly that:

`https://platform.twitter.com/embed/Tweet.html?dnt=true&embedId=twitter-widget-0&features=eyJ0ZndfdGltZWxpbmVfbGlzdCI6eyJidWNrZXQiOltdLCJ2ZXJzaW9uIjpudWxsfSwidGZ3X2ZvbGxvd2VyX2NvdW50X3N1bnNldCI6eyJidWNrZXQiOnRydWUsInZlcnNpb24iOm51bGx9LCJ0ZndfdHdlZXRfZWRpdF9iYWNrZW5kIjp7ImJ1Y2tldCI6Im9uIiwidmVyc2lvbiI6bnVsbH0sInRmd19yZWZzcmNfc2Vzc2lvbiI6eyJidWNrZXQiOiJvbiIsInZlcnNpb24iOm51bGx9LCJ0ZndfZm9zbnJfc29mdF9pbnRlcnZlbnRpb25zX2VuYWJsZWQiOnsiYnVja2V0Ijoib24iLCJ2ZXJzaW9uIjpudWxsfSwidGZ3X21peGVkX21lZGlhXzE1ODk3Ijp7ImJ1Y2tldCI6InRyZWF0bWVudCIsInZlcnNpb24iOm51bGx9LCJ0ZndfZXhwZXJpbWVudHNfY29va2llX2V4cGlyYXRpb24iOnsiYnVja2V0IjoxMjA5NjAwLCJ2ZXJzaW9uIjpudWxsfSwidGZ3X3Nob3dfYmlyZHdhdGNoX3Bpdm90c19lbmFibGVkIjp7ImJ1Y2tldCI6Im9uIiwidmVyc2lvbiI6bnVsbH0sInRmd19kdXBsaWNhdGVfc2NyaWJlc190b19zZXR0aW5ncyI6eyJidWNrZXQiOiJvbiIsInZlcnNpb24iOm51bGx9LCJ0ZndfdXNlX3Byb2ZpbGVfaW1hZ2Vfc2hhcGVfZW5hYmxlZCI6eyJidWNrZXQiOiJvbiIsInZlcnNpb24iOm51bGx9LCJ0ZndfdmlkZW9faGxzX2R5bmFtaWNfbWFuaWZlc3RzXzE1MDgyIjp7ImJ1Y2tldCI6InRydWVfYml0cmF0ZSIsInZlcnNpb24iOm51bGx9LCJ0ZndfbGVnYWN5X3RpbWVsaW5lX3N1bnNldCI6eyJidWNrZXQiOnRydWUsInZlcnNpb24iOm51bGx9fQ%3D%3D&frame=false&hideCard=false&hideThread=false&id=1883753899255046301&lang=en&origin=https%3A%2F%2Fstratechery.com%2F2025%2Fdeepseek-faq%2F&sessionId=b06c518abbbab42a1f00e3cfb0218012978e0662&siteScreenName=stratechery&theme=light&widgetsVersion=2615f7e52b7e0%3A1702314776716&width=550px`

Third, reasoning models like R1 and o1 derive their superior performance from using more compute. To the extent that increasing the power and capabilities of AI depend on more compute is the extent that Nvidia stands to benefit!

## Inference Alternatives and Stock Uncertainty

Still, it’s not all rosy. At a minimum DeepSeek’s efficiency and broad availability cast significant doubt on the most optimistic Nvidia growth story, at least in the near term. The payoffs from both model and infrastructure optimization also suggest there are significant gains to be had from exploring alternative approaches to inference in particular. For example, it might be much more plausible to run inference on a standalone AMD GPU, completely sidestepping AMD’s inferior chip-to-chip communications capability. Reasoning models also increase the payoff for inference-only chips that are even more specialized than Nvidia’s GPUs.

In short, Nvidia isn’t going anywhere; the Nvidia stock, however, is suddenly facing a lot more uncertainty that hasn’t been priced in. And that, by extension, is going to drag everyone down.

## The Chip Ban

So what about the chip ban?

The easiest argument to make is that the importance of the chip ban has only been accentuated given the U.S.’s rapidly evaporating lead in software. Software and knowhow can’t be embargoed — we’ve had these debates and realizations before — but chips are physical objects and the U.S. is justified in keeping them away from China.

At the same time, there should be some humility about the fact that earlier iterations of the chip ban seem to have directly led to DeepSeek’s innovations. Those innovations, moreover, would extend to not just smuggled Nvidia chips or nerfed ones like the H800, but to Huawei’s Ascend chips as well. Indeed, you can very much make the case that the primary outcome of the chip ban is today’s crash in Nvidia’s stock price.

What concerns me is the mindset undergirding something like the chip ban: instead of competing through innovation in the future the U.S. is competing through the denial of innovation in the past. Yes, this may help in the short term — again, DeepSeek would be even more effective with more computing — but in the long run it simply sews the seeds for competition in an industry — chips and semiconductor equipment — over which the U.S. has a dominant position.

## Like AI Models? OpenAI, Regulation, and Innovation

AI models are a great example. I mentioned above I would get to OpenAI’s greatest crime, which I consider to be the 2023 Biden Executive Order on AI. I wrote in Attenuating Innovation:

> The point is this: if you accept the premise that regulation locks in incumbents, then it sure is notable that the early AI winners seem the most invested in generating alarm in Washington, D.C. about AI. This despite the fact that their concern is apparently not sufficiently high to, you know, stop their work. No, they are the responsible ones, the ones who care enough to call for regulation; all the better if concerns about imagined harms kneecap inevitable competitors.

That paragraph was about [OpenAI](https://openai.com/index/better-language-models/) specifically, and the broader San Francisco AI community generally. For years now we have been subject to hand-wringing about the dangers of AI by the exact same people committed to building it — and controlling it. These alleged dangers were the impetus for OpenAI becoming closed back in 2019 with the release of GPT-2:



# OpenAI's Release Strategy and the AI Landscape

> Due to concerns about large language models being used to generate deceptive, biased, or abusive language at scale, we are only releasing a much smaller version of GPT-2 along with sampling code (opens in a new window). We are not releasing the dataset, training code, or GPT-2 model weights…We are aware that some researchers have the technical capacity to reproduce and open source our results. We believe our release strategy limits the initial set of organizations who may choose to do this, and gives the AI community more time to have a discussion about the implications of such systems.

> We also think governments should consider expanding or commencing initiatives to more systematically monitor the societal impact and diffusion of AI technologies, and to measure the progression in the capabilities of such systems. If pursued, these efforts could yield a better evidence base for decisions by AI labs and governments regarding publication decisions and AI policy more broadly.

The arrogance in this statement is only surpassed by the futility: here we are six years later, and the entire world has access to the weights of a dramatically superior model. OpenAI’s gambit for control — enforced by the U.S. government — has utterly failed. In the meantime, how much innovation has been foregone by virtue of leading edge models not having open weights? More generally, how much time and energy has been spent lobbying for a government-enforced moat that DeepSeek just obliterated, that would have been better devoted to actual innovation?

## AI Doom Scenarios and the Importance of Openness

**So you're not worried about AI doom scenarios?**

I definitely understand the concern, and just noted above that we are reaching the stage where AIs are training AIs and learning reasoning on their own. I recognize, though, that there is no stopping this train. More than that, this is exactly why openness is so important: we need more AIs in the world, not an unaccountable board ruling all of us.

## China's Open-Source Approach

**Wait, why is China open-sourcing their model?**

Well DeepSeek is, to be clear; CEO Liang Wenfeng said in a must-read interview that open source is key to attracting talent:

> In the face of disruptive technologies, moats created by closed source are temporary. Even OpenAI’s closed source approach can’t prevent others from catching up. So we anchor our value in our team — our colleagues grow through this process, accumulate know-how, and form an organization and culture capable of innovation. That’s our moat.

> Open source, publishing papers, in fact, do not cost us anything. For technical talent, having others follow your innovation gives a great sense of accomplishment. In fact, open source is more of a cultural behavior than a commercial one, and contributing to it earns us respect. There is also a cultural attraction for a company to do this.

The interviewer asked if this would change:

> DeepSeek, right now, has a kind of idealistic aura reminiscent of the early days of OpenAI, and it’s open source. Will you change to closed source later on? Both OpenAI and Mistral moved from open-source to closed-source.

> We will not change to closed source. We believe having a strong technical ecosystem first is more important.

This actually makes sense beyond idealism. If models are commodities — and they are certainly looking that way — then long-term differentiation comes from having a superior cost structure; that is exactly what DeepSeek has delivered, which itself is resonant of how China has come to dominate other industries. This is also contrary to how most U.S. companies think about differentiation, which is through having differentiated products that can sustain larger margins.

## Is OpenAI Screwed?

So is OpenAI screwed?

[N](https://stratechery.com/2023/the-accidental-consumer-tech-company-chatgpt-meta-and-product-market-fit-aggregation-and-apis/)ot necessarily. ChatGPT made OpenAI the accidental consumer tech company, which is to say a product company; there is a route to building a sustainable consumer business on commoditizable models through some combination of subscriptions and advertisements. And, of course, there is the bet on winning the race to AI take-off.

Anthropic, on the other hand, is probably the biggest loser of the weekend. DeepSeek made it to number one in the App Store, simply highlighting how Claude, in contrast, hasn’t gotten any traction outside of San Francisco. The API business is doing better, but API businesses in general are the most susceptible to the commoditization trends that seem inevitable (and do note that OpenAI and Anthropic’s inference costs look a lot higher than DeepSeek because they were capturing a lot of margin; that’s going away).

## Winners, Losers, and America's Choice

**So this is all pretty depressing, then?**

Actually, no. I think that DeepSeek has provided a massive gift to nearly everyone. The biggest winners are consumers and businesses who can anticipate a future of effectively-free AI products and services. Jevons Paradox will rule the day in the long run, and everyone who uses AI will be the biggest winners.

Another set of winners are the big consumer tech companies. A world of free AI is a world where product and distribution matters most, and those companies already won that game; The End of the Beginning was right.

China is also a big winner, in ways that I suspect will only become apparent over time. Not only does the country have access to DeepSeek, but I suspect that DeepSeek’s relative success to America’s leading AI labs will result in a further unleashing of Chinese innovation as they realize they can compete.

That leaves America, and a choice we have to make. We could, for very logical reasons, double down on defensive measures, like massively expanding the chip ban and imposing a permission-based regulatory regime on chips and semiconductor equipment that mirrors the E.U.’s approach to tech; alternatively, we could realize that we have real competition, and actually give ourself permission to compete. Stop wringing our hands, stop campaigning for regulations — indeed, go the other way, and cut out all of the cruft in our companies that has nothing to do with winning. If we choose to compete we can still win, and, if we do, we will have a Chinese company to thank.
