# GLM4模型开源，意料之中的尺寸，意料之外的效果

> 来源：[https://agijuejin.feishu.cn/docx/EupNdr0qFoqD9YxHUvrcA2R6nmh](https://agijuejin.feishu.cn/docx/EupNdr0qFoqD9YxHUvrcA2R6nmh)

> 刘聪NLP｜[阅读原文](https://mp.weixin.qq.com/s/LPsRps5OcYBKWRNizuwIsw)

> 转载请联系原作者取得授权

##  **写在前面 **

今天智谱开了GLM-4-9B的模型，不是6B，是9B。

一共开源了四个模型，Base版本模型（GLM-4-9B）、Chat版本模型（GLM-4-9B-Chat和GLM-4-9B-Chat-1M）和多模态模型（GLM-4V-9B-Chat）。

其中，模型为多语言模型除了支持中文、英文之外，还支持日语，韩语，德语在内的26种语言；Chat系列模型支持网页浏览、代码执行、自定义工具调用（Function Call）的能力；GLM-4V-9B模型支持中英双语多轮对话能力。

```
Github: https://github.com/THUDM/GLM-4
HF: https://huggingface.co/collections/THUDM/glm-4-665fcf188c414b03c2f7e3b7
```

##  **模型说明 **

GLM-4-9B模型的结构与GLM-3-6B模型结构一致，主要修改为模型层数、词表大小、支持更长的上下文。

+   词表由65024增加到151552；

+   模型层数由28增加到40；

+   最大长度之前的32K、128K到128K、1M。

模型的License还是免费学术研究、商业需要登记，但必须遵守相关条款和条件，与GLM3一致。

##  **效果说明 **

效果一句话总结，全面领先Llama-3-8B模型，全面**领先上一代ChatGLM3-6B模型。 **（这让我更加期待过两天即将开源的Qwen2系列模型的效果啦，开源真的越来越好了）

下面效果来自于官方Github效果截图。

在Base和Chat模型上，GLM-4-9B均优于Llama-3-8B模型。

![](img/a28a674f8f817e4a23a4e56da41e5550.png)

Base

![](img/41ccf1b8200f9758ddad1124cf17e3ea.png)

Chat

1M模型上进行大海捞针，效果全绿。

![](img/cbb5a03c890b2cb3a8d64deb7d9b2c3f.png)

工具调用上，也是优于Llama-3-8B模型。

![](img/1a85baf7e87111ff4eb2d47817e11119.png)

最**后是GLM-4V-9B**多模态模型效果，全面领先前一阵爆火的面壁MiniCPM-Llama3-V2.5多模态模型 （毕竟斯坦福都来抄） 。

![](img/f768a2dc280ec6dab0667750a69b9e61.png)

现在这些榜单的效果虽然可以展现出来一定能力，但我还是更相信对战榜单，后面不知道lmsys上会不会有GLM-4-9B-Chat的效果，真实场景中PK一把，看看谁弱谁强。

##  **快速调用 **

直[**接transformer**](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247487260&idx=1&sn=a6db35312dd894c6df84bf9ba5515845&scene=21#wechat_redirect)s走起，以GLM-4-9B-Chat模型为例。PS：模型下载有困难的同学，详见我之前写的一篇文章 《大模型下载使我痛苦》 。

```
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

device = &quot;cuda&quot;

tokenizer = AutoTokenizer.from_pretrained(&quot;THUDM/glm-4-9b-chat&quot;,trust_remote_code=True)

query = &quot;你好&quot;

inputs = tokenizer.apply_chat_template([{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: query}],
                                       add_generation_prompt=True,
                                       tokenize=True,
                                       return_tensors=&quot;pt&quot;,
                                       return_dict=True
                                       )

inputs = inputs.to(device)
model = AutoModelForCausalLM.from_pretrained(
    &quot;THUDM/glm-4-9b-chat&quot;,
    torch_dtype=torch.bfloat16,
    low_cpu_mem_usage=True,
    trust_remote_code=True
).to(device).eval()

gen_kwargs = {&quot;max_length&quot;: 2500, &quot;do_sample&quot;: True, &quot;top_k&quot;: 5}
with torch.no_grad():
    outputs = model.generate(**inputs, **gen_kwargs)
    outputs = outputs[:, inputs[&#x27;input_ids&#x27;].shape[1]:]
    print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

##  **写在最后 **

只能说在意料之内，智谱还是仅开源了一个10B以下的模型。

期待过两天，Qwen2系列模型的效果，我反正知道是一系列模型，最大能到72B。

[**欢迎多多关注**](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486601&idx=1&sn=dd2f338675de1448981bcfbe1efa71e3&scene=21#wechat_redirect)👸号「NLP工作站」， 加入交流群 ，交个朋友吧，一起学习，一起进步！