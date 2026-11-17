
# 5 | Chain of Thought Prompting

> 来源：[https://aigcreative.feishu.cn/docx/ApkVdnXmioOWsnxfNAucf2phnxb](https://aigcreative.feishu.cn/docx/ApkVdnXmioOWsnxfNAucf2phnxb)

Chain of thought prompting is a technique used to encourage a Language Model (LM) to explain its reasoning process when answering a prompt. This method involves providing the LM with a few-shot example where the reasoning process is explained in detail. By doing so, the LM is encouraged to also provide its own reasoning process when answering the prompt, which often leads to more accurate results.

To better understand how this works, consider the example of a standard prompt, where the LM provides an incorrect response and hallucinates the output. However, when given the exact steps and process taking the model through its calculations, it then provides the correct response. This is because the chain of thought prompt allows the LM to decompose multi-step problems into intermediate steps and allocate additional computation to problems that require more reasoning steps.

![](img/0fbbce18f9dfb8e104f189a04affeb17.png)

(Image from Wei et al., 2022)

Chain of thought prompting is a promising technique that offers a number of advantages for language models. One significant benefit is that it allows models to break down complex problems into smaller, more manageable steps, allowing for more efficient computation and potentially better performance.

Additionally, the use of chain of thought prompting provides an interpretable view into the model's decision-making process, which can be invaluable for debugging and improving model performance. This technique is highly versatile and can be applied to a wide range of tasks, including math word problems, symbolic manipulation, and commonsense reasoning. Furthermore, chain of thought prompting can be easily implemented in existing language models by including examples of chain of thought sequences in the few-shot prompts.

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>
