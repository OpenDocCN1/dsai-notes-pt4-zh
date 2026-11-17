
## CoT数据集介绍

[NuminaMath CoT](https://huggingface.co/datasets/AI-MO/NuminaMath-CoT) 数据集地址：https://huggingface.co/datasets/AI-MO/NuminaMath-CoT

除了`NuminaMath CoT`数据集外，还有`APPs`（编程数据集）、`TACO`（编程数据集）、`long_form_thought_data_5k`（通用问答数据集）等，都是CoT数据集，均可用于推理模型微调。相关数据集介绍，详见公开课：《借助DeepSeek R1进行模型蒸馏，模型蒸馏入门实战！》| https://www.bilibili.com/video/BV1X1FoeBEgW/

![](img/f36b30f427602ed961bc7d79eb5bc7b5.png)

### 4.2 `medical-o1-reasoning-SFT`数据集介绍

本节公开课，我们选取24年12月31号最新发布的一个包含推理过程的医学数据集：由深圳大数据研究院发布的`HuatuoGPT-o1`模型的微调数据集——`medical-o1-reasoning-SFT`。

+   [`medical-o1-reasoning-SFT`](https://huggingface.co/datasets/FreedomIntelligence/medical-o1-reasoning-SFT) 地址：https://huggingface.co/datasets/FreedomIntelligence/medical-o1-reasoning-SFT

![](img/10a62a4f09c5e2b23d8a5bd1b0d2e9c9.png)

数据集总共包含25400条数据，均为医学领域疾病诊断数据集，且不乏一些疑难杂症的推理和判断，数据集整体质量较高，推理过程严谨准确，非常适合进行医疗领域模型微调，可以极大程度提高模型对于病理的推理过程，并在这个过程中完成一些医疗知识的灌注。

例如一种一条数据集内容如下：

+   `Question`：A 45-year-old man with a history of alcohol use, who has been abstinent for the past 10 years, presents with sudden onset dysarthria, shuffling gait, and intention tremors. Given this clinical presentation and history, what is the most likely diagnosis?

> 一位45岁的男性，有饮酒史，过去10年一直戒酒，现因突然出现构音困难、步态蹒跚和意向性震颤就诊。根据这一临床表现和病史，最可能的诊断是什么？

`Complex_CoT`：Alright, let’s break this down. We have a 45-year-old man here, who suddenly starts showing some pretty specific symptoms: dysarthria, shuffling gait, and those intention tremors. This suggests something's going wrong with motor control, probably involving the cerebellum or its connections. Now, what's intriguing is that he's had a history of alcohol use, but he's been off it for the past 10 years. Alcohol can do a number on the cerebellum, leading to degeneration,and apparently, the effects can hang around or even appear long after one stops drinking. At first glance, these symptoms look like they could be some kind of chronic degeneration,maybe something like alcoholic cerebellar degeneration, but hold on. This looks different. The symptoms just came on all of a sudden. Chronic degenerations typically have a more gradual onset. Okay, let’s reconsider this sudden nature. It’s making me think of something more acute, more rapid onset. Hmm, if we dig back into his history of drinking,there might have been some damagedone, leading to nutritional deficiencies, like a lack of thiamine. Wernicke’s encephalopathy is a classic possibility here. That condition pops up due to thiamine deficiency, often after a history of alcohol use. It’s known for causing ataxia, confusion, and eye movement issues. However, he’s not showing the full triad of symptoms; there's no mention of confusion or eye problems, so maybe it doesn’t fit perfectly. Oh, wait a second, maybe we're missing something simpler. Given the acute nature of the symptoms,maybe this is more inline with something like a stroke. Sudden onset can definitely suggest a vascular event. With his alcohol history, he’s at risk for things like high blood pressure, which can increase stroke risk. In particular, lacunar strokes can mess with motor coordination,speech, the works. These are small, but significant enough to cause these exact symptoms: dysarthria, tremors, and a shuffling gait. But hang on, what if there’s another angle we’re missing? Chronic alcohol effects might still play a role here, justin a different way. There’s a condition called acquired hepatocerebral degeneration. This can happen in people with a history of liver disease due to alcohol, even years after they’ve quit. It can result in symptoms like these because of how substances get deposited in the brain. Linking this back to our patient, his history with alcohol could’ve led to some liver issues, and now, here we are with these symptoms showing up suddenly. Makes sense, right? So, considering everything, acquired hepatocerebral degeneration seems to fit quite nicely. That’s probably our most fitting diagnosis for this situation.

> 好的，让我们一步一步分析这个问题。我们有一位45岁的男性，突然出现了几个相当具体的症状：构音困难、步态蹒跚和意向性震颤。这提示着运动控制可能出现了问题，很可能是小脑或其连接受到了影响。现在有趣的是，他有饮酒史，但过去10年一直戒酒。酒精对小脑有很大的影响，可能导致退化，而且显然这种影响可能会持续很长时间，甚至在戒酒后仍然会出现。乍一看，这些症状看起来像是某种慢性退行性病变，也许像是酒精性小脑退行性病变，但稍等一下，这看起来有所不同。症状突然出现。慢性退行性病变通常是逐渐开始的。好吧，让我们重新考虑一下这种突然的性质。这让我想到一些更急性的、起病迅速的疾病。嗯，如果我们回顾他的饮酒史，可能存在一些损伤，导致营养缺乏，比如缺乏硫胺素。维尔尼克脑病就是一个经典的可能性。这个病状是由于硫胺素缺乏引起的，通常发生在有饮酒史的人群中。它的典型症状包括共济失调、意识混乱和眼球运动问题。然而，他并没有表现出完整的三联症；没有提到混乱或眼部问题，所以这可能不完全符合。哦，等一下，也许我们漏掉了更简单的情况。考虑到症状的急性性质，也许这更像是中风。突然发作肯定可以提示血管事件。考虑到他的饮酒史，他有高血压的风险，这会增加中风的风险。特别是，腔隙性中风可能会影响运动协调、言语等。这些小的中风虽然很小，但足够显著，能引发这些确切的症状：构音困难、震颤和蹒跚步态。但等等，是否还有其他角度我们没考虑到？慢性酒精影响可能仍然起着作用，只是以不同的方式。有一种叫做获得性肝脑病的疾病。这种情况可能发生在有酒精引起的肝病史的人身上，即使他们已经戒酒多年。它可能会导致类似的症状，因为体内的物质沉积在大脑中。回到我们的患者，他的酒精史可能导致了一些肝脏问题，而现在这些症状突然出现了。这是有道理的，对吧？所以，综合考虑所有因素，获得性肝脑病似乎非常符合。这个诊断可能是我们最合适的结论。

`Response` ：Considering the clinical presentation of sudden onset dysarthria, shuffling gait, and intention tremors in a 45-year-old man with a history of alcohol use who has been abstinent for the past 10 years, the most likely diagnosis is acquired hepatocerebral degeneration. This condition is associated with chronic liver disease, which can often be a consequence of long-term alcohol use. Despite the patient's abstinence from alcohol for a decade, previous alcohol use may have led to underlying liver dysfunction. This dysfunction, even if subclinical, can cause encephalopathy due to the accumulation of neurotoxic substances that affect the brain. The sudden onset of these neurological symptoms aligns with how acquired hepatocerebral degeneration can manifest, making it a probable diagnosis in this scenario.

考虑到这位45岁男性突发构音困难、蹒跚步态和意向性震颤的临床表现，以及他有饮酒史且过去10年未再饮酒，最可能的诊断是获得性肝脑病。该病与慢性肝病相关，而慢性肝病通常是长期饮酒的后果。尽管患者已经戒酒十年，但之前的饮酒可能导致了潜在的肝功能障碍。即使这些障碍是亚临床的，它也能通过神经毒性物质的积聚引起脑病。症状的突然出现与获得性肝脑病的表现方式相符，因此它是最符合的诊断。

## 三 DeepSeek R1模型微调代码实战

本部分见Jupyter文件。

> [DeepSeek R1 Distill高效微调入门实战](https://kq4b3vgg5b.feishu.cn/wiki/WWzZwHVNei8zsWkieJ3cyHzWnFe?from=from_copylink)部分内容已经转为在线文档导入社区➡️请点击🔗：
