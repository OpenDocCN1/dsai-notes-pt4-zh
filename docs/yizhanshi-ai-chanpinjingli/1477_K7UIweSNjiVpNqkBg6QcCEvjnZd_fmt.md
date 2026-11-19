
# infer.ipynb

> 来源：[https://v11enp9ok1h.feishu.cn/docx/MClYdLZTUog8GVxNsUUcdWZvnMd](https://v11enp9ok1h.feishu.cn/docx/MClYdLZTUog8GVxNsUUcdWZvnMd)

## 介绍

这个是`chinese-gpt2`的推理代码

1. 将`model_name_or_path = "checkpoint-36000"`里面的`"checkpoint-36000"`，修改为模型所在的路径。
2. 然后运行下面一个代码块，即可输出文本生成结果

```python
from transformers import GPT2Tokenizer, GPT2LMHeadModel, AutoTokenizer

model_name_or_path = "checkpoint-36000"
tokenizer = AutoTokenizer.from_pretrained(model_name_or_path)

# add the EOS token as PAD token to avoid warnings
model = GPT2LMHeadModel.from_pretrained(model_name_or_path, pad_token_id=tokenizer.eos_token_id)
```

```python
txt = """
你是谁
"""
# encode context the generation is conditioned on
input_ids = tokenizer.encode(txt, return_tensors='pt', add_special_tokens=False)
# set no_repeat_ngram_size to 2
beam_output = model.generate(
    input_ids,
    max_length=200,
    num_beams=5,
    no_repeat_ngram_size=2,
    early_stopping=True
)

print("Output:\n" + 100 * '-')
print(tokenizer.decode(beam_output[0], skip_special_tokens=True))
```

```
Output:
----------------------------------------------------------------------------------------------------
你 是 谁?, 简 单 的 描 述 是, 答 案 是 你 好 ， 我 是 一 个 女 孩 子 的 妈 妈 。 她 是 我 的 初 中 同 学 。 我 们 一 起 读 书 的 时 候 ， 有 一 次 我 和 她 聊 天 ， 她 问 我 ， 你 为 什 么 喜 欢 我 ？ 我 说 ， 因 为 我 知 道 你 的 家 庭 条 件 不 错 ， 所 以 我 也 很 爱 她 。 后 来 她 告 诉 我 她 的 父 母 都 是 农 村 出 身 的 ， 而 且 是 个 很 有 文 化 的 人 。 当 时 我 就 觉 得 很 奇 怪 ， 就 问 了 她 一 句 ： 你 们 这 些 人 是 怎 么 想 的 ？ 她 回 答 说 ： 是 的 。 那 一 刻 我 感 觉 到 了 自 己 的 力 量 ， 也 就 是 说 我 对 这 个 世 界 充 满 了 好 奇 和 向 往 。 可 是 当 我
```
