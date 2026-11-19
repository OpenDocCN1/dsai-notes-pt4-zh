
# Qwen1.5开源32B模型-将开源进行到底

> 来源：[https://agijuejin.feishu.cn/docx/LVZhdGFE9ocxXdxqAaAcPWFqnpc](https://agijuejin.feishu.cn/docx/LVZhdGFE9ocxXdxqAaAcPWFqnpc)

> 刘聪NLP｜[阅读原文](https://mp.weixin.qq.com/s/WOiyQYSs5XZzSsn6hdb_Ww)

> 转载请联系原作者取得授权

## 写在前面

大家好，我是刘聪NLP。

阿里开源的千问系列模型，一直受到业界好评，之前版本有0.5B、1.8B、7B、14B、72B，但一直缺少的30B级别开源模型，这也一直是一个遗憾。

怎么说呢？72B模型太大，很多人用不起来，无论是微调，还是模型推理部署，成本太高；而14B又有点小，很多场景中效果不理想，因此需要一个处在中间状态的模型-30B左右。

周二有群友透露，本周会开源30B级别模型。

![](img/c5d9dd0053b839d89a484fd88b544ec2.png)

群友果然靠谱，在清明节期间，放出来了Qwen1.5的32B模型，不仅涉及Base模型、Chat模型、还有量化的系列模型，详细见下面链接。

```
HF: https://huggingface.co/Qwen/Qwen1.5-32B
Blog: https://qwenlm.github.io/blog/qwen1.5-32b/
```

PS：千问真是开源走到了底，不给其他家留余地，之前30B级别的Yi怎么办呀！！！

## 模型介绍

模型参数：词表大小152064，64层、隐藏层维度5120，支持32k长度。

```
{
  "architectures": [
    "Qwen2ForCausalLM"
  ],
  "attention_dropout": 0.0,
  "bos_token_id": 151643,
  "eos_token_id": 151643,
  "hidden_act": "silu",
  "hidden_size": 5120,
  "initializer_range": 0.02,
  "intermediate_size": 27392,
  "max_position_embeddings": 32768,
  "max_window_layers": 35,
  "model_type": "qwen2",
  "num_attention_heads": 40,
  "num_hidden_layers": 64,
  "num_key_value_heads": 8,
  "rms_norm_eps": 1e-06,
  "rope_theta": 1000000.0,
  "sliding_window": 32768,
  "tie_word_embeddings": false,
  "torch_dtype": "bfloat16",
  "transformers_version": "4.37.2",
  "use_cache": true,
  "use_sliding_window": false,
  "vocab_size": 152064
}
```

看看指标。

![](img/8a8612a80044ddbd7bf86d58feee4a99.png)

从指标上来看，Qwen1.5-32B模型，相较于72B模型有一些差距，但是由于Yi-34B和Llama2-34B很多。Yi在30B级别的地位不保啦。

32B-Chat版本模型在MT-Bench榜单上超过8分，并且与72B-Chat版本模型差距较小。

![](img/9148900ef60c2aca0a3f8c9aa1aa5232.png)

Qwen1.5-32B还支持多语言，包括阿拉伯语、西班牙语、法语、葡萄牙语、德语、意大利语、俄语、日语、韩语、越南语、泰语和印尼语等，相关评测如下。

![](img/3aa566cc54005cde2c6a80afb8e29061.png)

在32k长度上，进行大海捞针实验，也相对比较出色。

![](img/5ba0868019d907ec0ade2c8cd1f8fc6c.png)

## 快速使用

```
from transformers import AutoModelForCausalLM, AutoTokenizer
device = "cuda" # the device to load the model onto

model = AutoModelForCausalLM.from_pretrained(
    "Qwen/Qwen1.5-32B-Chat",
    torch_dtype="auto",
    device_map="auto"
)
tokenizer = AutoTokenizer.from_pretrained("Qwen/Qwen1.5-32B-Chat")

prompt = "Give me a short introduction to large language model."
messages = [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": prompt}
]
text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True
)
model_inputs = tokenizer([text], return_tensors="pt").to(device)

generated_ids = model.generate(
    model_inputs.input_ids,
    max_new_tokens=512
)
generated_ids = [
    output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
]

response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
```

## 写在最后

阿里的千问系列模型真实将开源进行到底了，吾辈之楷模！！！

[欢迎多多关注](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486601&idx=1&sn=dd2f338675de1448981bcfbe1efa71e3&scene=21#wechat_redirect)👸号「NLP工作站」，加入交流群，交个朋友吧，一起学习，一起进步！
