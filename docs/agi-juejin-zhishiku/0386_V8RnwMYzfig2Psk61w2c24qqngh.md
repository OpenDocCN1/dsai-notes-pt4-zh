# 人工智能助手API调用能力的动态评估方法

> 来源：[https://agijuejin.feishu.cn/docx/RdKidZdYRoaAo1xsT8TcoaKanhe](https://agijuejin.feishu.cn/docx/RdKidZdYRoaAo1xsT8TcoaKanhe)

> 哈工大SCIR｜[阅读原文](https://mp.weixin.qq.com/s/qxrx2LcETqjBApSYDMroVQ)

> 转载请联系原作者取得授权

> 论文名称：Beyond Static Evaluation: A Dynamic Approach to Assessing AI Assistants’ API Invocation Capabilities

> 论文作者：牟虹霖，徐阳，冯云龙，韩骁枫，李一同，侯宇泰，车万翔*

> 原创作者：牟虹霖

> 论文链接：https://arxiv.org/abs/2403.11128

#  **背景 **

随着大型语言模型（LLMs）的迅速发展，AI助手在使用工具上的能力（尤其是在API调用方面）得到了显著提升。这种进步使我们需要更精准有效的评估手段。目前，许多研究通过静态评估来衡量AI助手的API调用效果，即用预先收集的固定对话历史评估模型的API调用准确度[1,2]。如图1所示，在评估阶段，待测AI助手不需要参与实际对话，而是根据一个固定的对话历史来调用API。通过评估API调用参数与对话中信息的一致性来衡量模型效果。

![](img/de1531179e90d4546f0ad11f2963b628.png)

图1：静态自动评估方法，评估结果似乎一切正常

然而，这种静态评估方法可能高估模型的能力，因为AI助手与人类的实际交互存在遗漏信息、产生幻觉等问题，模型无法根据已有交互发起准确API调用。如图2所示，与图1中相同的待测AI助手在评估阶段不是直接总结对话历史，而是在实际参与了用户对话后根据对话结果调用API。与图1中满分表现不同，AI助手在对话期间“忘记”了询问应用名称，但在API调用时“幻想”了一个假的API名称，造成了API调用与用户需求不一致。这反映出静态自动评估方法在衡量模型API调用能力上的局限性。

![](img/f1a787d7c0adaaadd2552ecfc348ed03.png)

图2：人工评估方法，动态评估暴露出了静态方法中掩盖的模型缺陷（本例为槽位询问不全，且出现幻觉）

为此，我们提出了一种自动动态评估（AutoDE）方法，这种方法可以在**无需人类参**与的情况下，更准确地评估助手的API调用效果，如图3。我们采用了一个 用户代理 来代替人类与待测AI助手交互，可以反应模型在交互下的API调用能力缺陷。

![](img/8471765dbc0ee586fe0947ddb8b85bf8.png)

图3：我们的自动评估方法，同人工评估方法一样可以暴露出模型缺陷

#  **方法 **

##  **自动动态评估AutoDE **

我**们提出方法**AutoDE的主要目标是设计一种自动化的动态评估机制，以尽可能接近人工评估过程。我们引入了额外的语言模型作为 用户代理 来模拟人类标注者的行为。该模型通过多轮与AI助手的交互，提出指令并回答AI助手的问题，最终引导AI助手调用API。AI助手产生的API调用与期望API调用计算准确度指标，以此衡量AI助手的工具调用能力。自动动态评估的过程如图2。

![](img/aa4b9dcd5693776806a0f93ae5d94255.png)

图4：自动评估过程

**为确保可重**现性**并方便与其****它评估方法**进行比较，我们设定了 用户脚本 。 用户脚本 描述了参与对话的角色、背景、目的以及期望API调用。 用户脚本 的一个示例如下：

```
Character: Lisa, a busy mother
Background: Lisa needs to take her son,
who recently fell and sprained his
ankle, to the orthopedic department.
Purpose: Using a tablet, Lisa books an
appointment at the hospital using a
medical appointment registration app.
API: { &quot;funcName&quot;: &quot;RegMedAppt&quot;, &quot;time&quot;: &quot;Monday&quot;, &quot;departmentName&quot;: &quot;Orthopedic&quot;}
```

**用户代理 **的 prompt 如下，其中 {{U**SER_P**ROMPT}} 为上文描述的 用户脚本 ：

```
You are an experienced data annotator. 
You need to act as a user in a set of 
conversations between a user and a voice
assistant Bob ...
Please construct user queries or responses 
according to the following settings:
{{USER_PROMPT}}
```

##  **静态评估 **

人工评估尽管**可以更准确地评**估AI助手的性能，但往往具有更高的成本。历史上，许多研究采用静态评估作为人机对话评估的自动替代方法[1,2]。静态评估不需要用户和AI助手之间实时交互；相反，它基于预标注的对话历史进行操作。这种方法的主要吸引力在于其简单和迅速。在这种评估方法中，评估仅包括一个回合，AI助手根据预定义的历史直接产生API调用，而无需向用户提问或接收反馈，产生的API调用与期望API调用计算准确度指标。我们的静态对话历史由GPT4扮演用户和AI助手，通过 自动动态评估 节中的方法自行交互获得，最终获得275组静态对话。

##  **数据集构造 **

###  **API文档构造 **

从Rastogi等人的模式[3]中汲取灵感，我们参考语音助手的日常使用场景设计了66个API。这些 API 包含了名称、用法描述、参数列表和参数说明。具体而言，API 的功能主要包括：（1）手机系统设置，例如调节音量或亮度，打开 Wi-Fi 或蓝牙等；（2）娱乐系统，如播放音乐或视频；（3）个人助理，如处理电子邮件、导航、设置闹钟、与医院预约、订票、预订酒店；（4）查找新闻、天气、股票价格和其他信息；（5）多模态功能，如对象识别和图像标注。API文档的一个简化示例如下：

```
{
    &quot;domain&quot;: &quot;Device Manipulation&quot;,
    &quot;subdomain&quot;: &quot;Setting Item&quot;,
    &quot;function&quot;: &quot;Luminance&quot;,
    &quot;api&quot;: &quot;SetLuminance&quot;,
    &quot;description&quot;: &quot;Set the brightness ...&quot;,
    &quot;parameters&quot;: {
        &quot;deviceType&quot;: &quot;Supported device types ...&quot;,
        &quot;targetValue&quot;: &quot;Target brightness size&quot;
    }
}
```

###  **用户脚本构建 **

**有了API**文档之后**，我们开始**构建 用户脚本 。每个 用户脚本 都包括一个对话上下文和期望一个API调用。每个对话上下文对应一种API使用场景，指导用户和AI助手之间的交互。我们采用GPT 4来为每个API文档头脑风暴5个使用场景以保证评估的多样性，总共生成了330个配置文件。

#  **实验 **

##  **主实验 **

我们测试GPT3.5, Claude Instant 1.2, Llama 2 70B Chat[4]和Code Ll**ama 1**3B OASST[5]的API调用效果。其中自动动态测试采用GPT3.5和Llama 2 7B Chat作为 用户代理 。可以看到在多个AI助手上，相比静态评估自动动态评估的效果更接近人工评估。

![](img/3d8435ec6c57c6d6f89d4f6e80cd96f6.png)

表1：主实验结果

以人工评估计算的各个AI助手的F1分数为横轴，将静态评估、自动动态评估分数作为纵轴画图，可以看到明显自动动态评估下各AI助手分数接近一条直线，与人类评估的一致性更好。

![](img/6264fb3325d4e485677e19b936cfbda9.png)

图5：AI助手的F1分数

##  **讨论 **

AutoDE揭示了在静态评估中未被发现的人工智能助手的某些问题。

+   **难以调用API**

在静态评估中，AI助手被提示在当前轮次中输出API调用，以保证对API调用进行评估。然而，在动态评估中，是否进行API调用的决定取决于AI助手，它必须根据上下文决定是否进行这样的调用，这对AI助手的API调用能力提出了挑战。例如，Llama 2 70B Chat模型尽管提供了足够信息，但在调用API时常犹豫。它更倾向于指导用户完成任务，例如教用户在手机上操作，而不是直接调用API调用。即使修改了多种prompt，鼓励系统在某些情况下自主生成API，问题仍然存在。

经过分析，我们发现，在动态评估中，相比静态评估，Llama 2 70B Chat调用API的性能下降了约66%，而Code Llama 13B OASST的性能下降了约9%。这表明Llama 2 70B Chat在动态交互中生成API调用的能力较弱。

+   **询问虚假参数**

在静态评估中，AI助手依靠预标注的对话历史来总结API调用的参数。相比之下，在动态评估中，它必须识别缺失的参数并向用户询问。一些系统在没有明确要求的情况下，会错误地询问不存在的参数。例如，尽管"movieName"参数没有在可用参数列表中，AI助手仍然查询了它。这种虚假的参数查询问题导致了Code Llama 13B OASST在动态评估中的表现相比静态评估下降了9%。

+   **啰嗦的查询**

此外，我们还观察到，在动态评估期间，有些系统倾向于反复询问用户已经提供的信息。这在Claude Instant 1.2中表现尤为明显。在与Llama 2 7B Chat的动态评估中，Claude Instant 1.2的平均对话长度比静态对话记录长了1.88个回合，而GPT 3.5的平均对话长度则比静态历史记录短了1.61个回合。这表明Claude Instant 1.2倾向于反复询问用户，而GPT 3.5则更倾向于在单个回合中收集更多信息。我们未来的工作将旨在评估对话质量，包括一个叙述冗长的度量。

#  **结论 **

我们提出了一个名为 Automated Dynamic Evaluation（AutoDE）的系统，它通过动态交互来评估人工智能助手的 API 调用能力。AutoDE 采用了一个被称为 用户代理 的工具，该工具能够根据提供的 用户脚本 模拟人类用户的行为模式。我们在多个AI助手上使用自行组建的基准测试进行了实验。实验结果表明，AutoDE能够发现那些在静态评估中被忽略的缺陷。与手动评估相比，用户代理更接近实际应用情境，它揭示了一些问题，例如某些助手不愿调用 API 或存在虚假参数查询的问题。

总的来说，AutoDE 是一个有效的自动化工具，它可以替代成本高昂的人工评估，以判断AI助手对 API 的掌握程度。通过模拟人类交互，用户代理能够动态地引导系统，识别出那些在静态分析中无法看到的缺点。

#  **参考文献 **

[1] Zang, Xiaoxue and Rastogi, Abhinav and Sunkara, Srinivas and Gupta, Raghav and Zhang, Jianguo and Chen, Jindong. 2020. MultiWOZ 2.2: A dialogue dataset with additional annotation corrections and state tracking baselines. PID https://github.com/budzianowski/multiwoz

[2] Timo Schick, Jane Dwivedi-Yu, Roberto Dessì, Roberta Raileanu, Maria Lomeli, Luke Zettlemoyer, Nicola Cancedda, and Thomas Scialom. 2023. Toolformer: Language models can teach themselves to use tools. arXiv preprint arXiv:2302.04761.

[3] Rastogi, Abhinav and Zang, Xiaoxue and Sunkara, Srinivas and Gupta, Raghav and Khaitan, Pranav. 2020. Towards scalable multi-domain conversational agents: The schema-guided dialogue dataset. PID https://github.com/google-researchdatasets/dstc8-schema-guided-dialogue

[4] Touvron, H., Martin, L., Stone, K., Albert, P., Almahairi, A., Babaei, Y., ... & Scialom, T. (2023). Llama 2: Open foundation and fine-tuned chat models. arXiv preprint arXiv:2307.09288 .

[5] OpenAssistant. (2023b). OpenAssistant/codellama-13B-OASST-SFT-v10 · hugging face . OpenAssistant/codellama-13b-oasst-sft-v10 · Hugging Face. https://huggingface.co/OpenAssistant/codellama-13b-oasst-sft-v10

本期责任编辑：车万翔

本期编辑：杨 昕