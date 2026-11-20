
# 中国人自己的Llama：Qwen1.5开源110B参数模型

> 来源：[https://agijuejin.feishu.cn/docx/C57rdszmMogqigx5m8FcagxknyC](https://agijuejin.feishu.cn/docx/C57rdszmMogqigx5m8FcagxknyC)

> 刘聪NLP｜[阅读原文](https://mp.weixin.qq.com/s/K4nwMGcgftyAo_h8JoBaQg)

> 转载请联系原作者取得授权

## 写在前面

大家好，我是刘聪NLP。

开源界最近属实是太疯狂了，前有`Llama3-70B`模型开源，后有`Qwen1.5`开源千亿（`110B`）级别参数模型。

`Qwen`你真的让我开始捉摸不透了，`1.5`系列已经从`0.5B`、`1.8B`、`7B`、`14B`、`32B`、`72B`到现在的`110B`，还有`Code`系列模型、`MOE`系列模型，太全了，感觉已经快把中文开源模型市场给垄断了。

并且，注意的是，`110B`的模型是`Dense`的模型，不是虚胖的`MOE`模型（个人总觉得`MOE`模型的参数有点不真实，不喜勿喷）。

上周的时候，`Qwen1.5-110B-Chat`的`Demo`已经放出，其实早有内部消息说本周开源参数，果然，你是说话算数的。

```
Blog: https://qwenlm.github.io/blog/qwen1.5-110b
HF: https://huggingface.co/Qwen/Qwen1.5-110B-Chat
Demo: https://huggingface.co/spaces/Qwen/Qwen1.5-110B-Chat-demo
```

## Qwen1.5-110B介绍

模型结构与之前模型相似，采用`Transformer-Decoder`架构，并使用分组查询注意力（`Grouped Query Attention`，`GQA`），加速模型推理计算。模型的最大长度为`32K`，支持英、中、法、西、德、俄、日、韩、越等多种语言。

在基础能力上的效果全面领先`72B`模型，与`Llama3-70B`模型也是平分秋色。

![](img/9c24deefd4de3d46e709da562c234fab.png)

在`Chat`能力上，在`MT-Bench`和`AlpacaEval 2.0`上进行了评估，也是优于`Llama-3-70B-Instruct`。

![](img/8cfaea66c585b3c0ce724c6631d4d6b8.png)

由于在训练`110B`模型过程中，没有对预训练的方法进行大幅改变，因此与`72B`相比的性能提升主要来自于增加模型规模。

此外，在`lmsys`的对战榜单上，原来`Llama3-70B`是开源最强，排在第6，`Qwen1.5-72B`排在13。按照这个架势，`Qwen1.5-110B`完全有机会成为开源最强模型，期待后续大家投票的结果。

![](img/64886824e1a685e758ac763efec9fb0e.png)

`Qwen1.5-110B`本身就有很多中文数据，相较于`Llama3`模型来说，对中文的适配也会更好。（反正我基本全面拥抱了，主要是各个尺寸都有，用着就很舒服）

## 模型使用

直接`transformers`走起，模型下载[有困难的同学，详见我](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247487260&idx=1&sn=a6db35312dd894c6df84bf9ba5515845&scene=21#wechat_redirect)之前写的一篇文章《 大模型下载使我痛苦 》。

```
from transformers import AutoModelForCausalLM, AutoTokenizer
device = "cuda" # the device to load the model onto

model = AutoModelForCausalLM.from_pretrained(
    "Qwen/Qwen1.5-110B-Chat",
    torch_dtype="auto",
    device_map="auto"
)
tokenizer = AutoTokenizer.from_pretrained("Qwen/Qwen1.5-110B-Chat")

prompt = "你叫什么名字"
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

generated_ids = model.generate(model_inputs.input_ids,max_new_tokens=512)

generated_ids = [output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)]
response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]

print(response)
```

PS：自己没有显卡的也没关系，反正`HF`上是由`Demo`的，嫖就完事儿了。

## 测试几个弱智吧问题

石油也是油，为啥没人用它来炒菜？

![](img/18495d5654c75a201b62c1fbb79cbef0.png)

如果把脏话都说出来了，那么嘴是不是就干净了

![](img/e62ffc7a9a38ac70ee4cc4f55ddaaad3.png)

为什么孙悟空是中国猴子却叫美猴王，不应该叫中猴王吗

![](img/9248036ee66e2c49e5952a4f3cb6f76e.png)

下雨天关节疼 那我骗它是晴天不就不会疼了

![](img/a27f8913b905bc14a448a8f37e51010c.png)

PS：大部分回答还是不错的，自己体验下来整体效果也是很强。

## 写在最后

`Qwen2`系列模型说是下个月会开源，十分期待。

大模型开源社区也是越来月繁荣昌盛，个人认为与闭源直接的差距是在缩小的，请各位头部开源组织继续加油，我会紧紧跟随。

![](img/7c9202415399adb9fe701abc210122c6.png)

[欢迎多多关注](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486601&idx=1&sn=dd2f338675de1448981bcfbe1efa71e3&scene=21#wechat_redirect)👸号「NLP工作站」，加入交流群，交个朋友吧，一起学习，一起进步！
