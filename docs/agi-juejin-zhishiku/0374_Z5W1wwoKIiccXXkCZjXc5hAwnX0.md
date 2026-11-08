# Qwen1.5-MoE模型：2.7B的激活参数量达到7B模型的性能

> 来源：[https://agijuejin.feishu.cn/docx/CpBnd3H1Go1ZwhxCbqucTaeCnaf](https://agijuejin.feishu.cn/docx/CpBnd3H1Go1ZwhxCbqucTaeCnaf)

> 刘聪NLP｜[阅读原文](https://mp.weixin.qq.com/s/FTd9L6HzpV-5AoT20V8YyQ)

> 转载请联系原作者取得授权

##  **写在前面 **

今天阿里放了MOE的模型，总参数量14.3B，具有64个专家，每次激活8个，在仅激活2.7B参数情况下，效果可以与Qwen1.5-7B模型媲美，并且训练陈本降低75%，推理速度提升1.74倍。同时阿里开源了Base版本模型和Chat版本模型。

```
博客：https://qwenlm.github.io/zh/blog/qwen-moe/
HF：https://huggingface.co/Qwen/Qwen1.5-MoE-A2.7B
HF：https://huggingface.co/Qwen/Qwen1.5-MoE-A2.7B-Chat
```

注意：以下内容来自参考Qwen官方博客。

##  **模型结构 **

与Mistral-MOE配置8个专家采用Top-2门控策略选择专家不同，主要进行以下改进：

+   细粒度专家细分：将一个完整的FFN拆分成数个部分，每一部分作为一个独立的专家。

+   模型初始化优化：利用Qwen-1.8B模型初始化Qwen1.5-MoE-A2.7B参数，并在在初始化阶段引入了随机参数，不仅加速了模型的收敛速度，还提升了预训练过程中整体性能。

+   共享路由机制：实现共享专家与路由专家的融合，模型始终存在4个共享专家处于激活状态，并从剩余的60个路由专家中选择激活的4个专家。

![](img/9ac262a2f55a116e56f4ef627b195000.png)

来自Deepseek-MOE

##  **实验效果 **

在MMLU、GSM8K、HumanEval和Multilingual榜单上评论了Qwen1.5-MoE-A2.7B模型的语言理解、数学、代码和多语言能力。在MT-Bench榜单上测试了Chat模型的能力。发现Qwen1.5-MoE-A2.7B效果可以媲美最佳的7B模型。

![](img/9ebda63b9e43b15cdec79cc6eb69f4b8.png)

尽管MOE模型的总参数了较大，但实际激活参数远小于7B模型，并由于初始化方法无需过多数据进行模型训练，从而显著降低了模型75%的训练成本。

![](img/eeb53b4e12162498a2313c46aaa98155.png)

并且在A100-80G显卡上，利用vllm框架测试Qwen1.5-7B和Qwen1.5-MoE-A2.7B模型的推理性能，保持输入输出token均为1k情况下，具体TPS如下，可以发现Qwen1.5-MoE-A2.7B模型与Qwen1.5-7B模型相比，推理速度提高了约1.74倍。

![](img/344d68cbcb1440dae7cf1347e5d154bb.png)

##  **模型推理 **

利用transformers进行Qwen1.5-MoE-A2.7B模型推理如下所示：

```
from transformers import AutoModelForCausalLM, AutoTokenizer
device = &quot;cuda&quot; # the device to load the model onto

model = AutoModelForCausalLM.from_pretrained(
    &quot;Qwen/Qwen1.5-MoE-A2.7B-Chat&quot;,
    torch_dtype=&quot;auto&quot;,
    device_map=&quot;auto&quot;
)
tokenizer = AutoTokenizer.from_pretrained(&quot;Qwen/Qwen1.5-MoE-A2.7B-Chat&quot;)

prompt = &quot;Give me a short introduction to large language model.&quot;
messages = [
    {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;You are a helpful assistant.&quot;},
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt}
]
text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True
)
model_inputs = tokenizer([text], return_tensors=&quot;pt&quot;).to(device)

generated_ids = model.generate(
    model_inputs.input_ids,
    max_new_tokens=512
)
generated_ids = [
    output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
]

response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
```

##  **写在最后 **

开源的MOE模型越来越多，最近老马的Grok-1模型是MOE，DBRX的132B模型也是MOE，今天千问也开源了MOE。

但总觉得如论MOE模型有些投机取巧的意味，狗头保命。

[**欢迎多多关注**](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486601&idx=1&sn=dd2f338675de1448981bcfbe1efa71e3&scene=21#wechat_redirect)👸号「NLP工作站」， 加入交流群 ，交个朋友吧，一起学习，一起进步！