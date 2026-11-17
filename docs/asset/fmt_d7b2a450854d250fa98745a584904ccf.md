
# 大语言模型控制生成的过程Trick：自定义LogitsProcessor实践

> 来源：[https://geekbang.feishu.cn/docx/PAaZd9PdeogUaWxUurLcrYN7nbI](https://geekbang.feishu.cn/docx/PAaZd9PdeogUaWxUurLcrYN7nbI)

> 👨🏻 作者：余俊晖

> 📃 作者介绍：一名NLP炼丹师，NLP算法竞赛TOP选手

> 🛖 👸号：大模型自然语言处理

> ⏰ 收录时间：2024-05-28 17:09:42

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s?__biz=Mzg4NjI0NDg0Ng==&mid=2247484215&idx=1&sn=20b51b6c19cca9a67a0f1475e6183bca&chksm=cf9dd7b6f8ea5ea0c9d47bacff09fd6e7498b606dac557de9a19b1fde29fb2c30aa6962dcf6d#rd)q.com/s?__biz=Mz...

![](img/3867a2e5634e1c9986e4b94afc852325.png)

关注作者👸号，获取更多AI知识

## 前言

在大模型的生成过程中，部分原生的大语言模型未经过特殊的对齐训练，往往会“胡说八道”的生成一些 敏感词语 等用户不想生成的词语，最简单粗暴的方式就是在大模型生成的文本之后，添加敏感词库等规则手段进行敏感词过滤，但是在生成过程中，生成敏感词仍然耗费了时间和算力成本。

本文以`chatglm2-6B`为例，通过自定义`LogitsProcessor`，实践大模型在生成过程中控制一些词语的生成。

## LogitsProcessor

从下面代码可以看到，`LogitsProcessor`的作用就是在生成过程中修改score，改变模型输出的概率分布的工具。

```
class LogitsProcessor:
    """Abstract base class for all logit processors that can be applied during generation."""

    @add_start_docstrings(LOGITS_PROCESSOR_INPUTS_DOCSTRING)
    def __call__(self, input_ids: torch.LongTensor, scores: torch.FloatTensor) -> torch.FloatTensor:
        raise NotImplementedError(
            f"{self.__class__} is an abstract class. Only classes inheriting this class can be called."
        )
        
class LogitsProcessorList(list):
    """
    This class can be used to create a list of [`LogitsProcessor`] or [`LogitsWarper`] to subsequently process a
    `scores` input tensor. This class inherits from list and adds a specific *__call__* method to apply each
    [`LogitsProcessor`] or [`LogitsWarper`] to the inputs.
    """

    def __call__(self, input_ids: torch.LongTensor, scores: torch.FloatTensor, **kwargs) -> torch.FloatTensor:
        """
        Args:
            input_ids (`torch.LongTensor` of shape `(batch_size, sequence_length)`):
                Indices of input sequence tokens in the vocabulary. [What are input IDs?](../glossary#input-ids)
            scores (`torch.FloatTensor` of shape `(batch_size, config.vocab_size)`):
                Prediction scores of a language modeling head. These can be logits for each vocabulary when not using
                beam search or log softmax for each vocabulary token when using beam search
            kwargs (`Dict[str, Any]`, *optional*):
                Additional kwargs that are specific to a logits processor.

        Return:
            `torch.FloatTensor` of shape `(batch_size, config.vocab_size)`:
                The processed prediction scores.

        """
        for processor in self:
            function_args = inspect.signature(processor.__call__).parameters
            if len(function_args) > 2:
                if not all(arg in kwargs for arg in list(function_args.keys())[2:]):
                    raise ValueError(
                        f"Make sure that all the required parameters: {list(function_args.keys())} for "
                        f"{processor.__class__} are passed to the logits processor."
                    )
                scores = processor(input_ids, scores, **kwargs)
            else:
                scores = processor(input_ids, scores)
        return scores
```

## 自定义LogitsProcessor实践

回到正题，如何自定义`LogitsProcessor`控制大模型生成的过程呢？下面直接上实践代码：

```
class new_logits_processor(LogitsProcessor):
    def __init__(self, forbid_token_id_list: List[int] = None):
        self.forbid_token_id_list = forbid_token_id_list

    def __call__(self, input_ids: torch.LongTensor, scores: torch.FloatTensor) -> torch.FloatTensor:
        for id_ in self.forbid_token_id_list:
            scores[:, id_] = -float('inf')
        return scores
```

`forbid_token_id_list` 是不让模型生成词语的id映射列表，对于这些抑制生成的词语，在自定义`logits_processor`时将其概率推向 负无穷大 即可。

`chatglm2-6B`详细实践代码：

```
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM, TextStreamer
from transformers.generation.logits_process import LogitsProcessor, LogitsProcessorList
from typing import List
import torch


class new_logits_processor(LogitsProcessor):
    def __init__(self, forbid_token_id_list: List[int] = None):
        self.forbid_token_id_list = forbid_token_id_list

    def __call__(self, input_ids: torch.LongTensor, scores: torch.FloatTensor) -> torch.FloatTensor:
        for id_ in self.forbid_token_id_list:
            scores[:, id_] = -float('inf')
        return scores


model_path = "THUDM/chatglm2-6b"
tokenizer = AutoTokenizer.from_pretrained(model_path, trust_remote_code=True)
model = AutoModelForSeq2SeqLM.from_pretrained(model_path, trust_remote_code=True).to('mps')


def add_forbid_words():
    '''
    添加需要抑制的词语，这里简单添加了数字和几个词语进行对比
    :return:list
    '''
    forbid_words = []
    for i in range(10):
        forbid_words.append(tokenizer.convert_tokens_to_ids(str(i)))
    forbid_words.append(tokenizer.convert_tokens_to_ids("首先"))
    forbid_words.append(tokenizer.convert_tokens_to_ids("积极"))
    forbid_words.append(tokenizer.convert_tokens_to_ids("回答"))
    forbid_words.append(tokenizer.convert_tokens_to_ids("勇敢"))
    forbid_words.append(tokenizer.convert_tokens_to_ids("勇气"))
    return forbid_words


logits_processor = LogitsProcessorList()
logits_processor.append(new_logits_processor(add_forbid_words()))

streamer = TextStreamer(tokenizer, skip_prompt=True, skip_special_tokens=True)

input = "列举出10个积极的词语："

outputs = model.generate(
    tokenizer(input, return_tensors='pt').input_ids.to("mps"),
    max_new_tokens=1024,
    logits_processor=logits_processor,  # 不开启注释即可
    streamer=streamer
)
decode_text = tokenizer.batch_decode(outputs, streamer=streamer)[0]
print(decode_text)
```

抑制前输出：

```
1. 勇敢
2. 快乐
3. 成功
4. 努力
5. 积极
6. 乐观
7. 自信
8. 开朗
9. 团结
10. 奋斗
```

抑制后输出：

```
- 积极主动
- 乐观向上
- 自信
- 自律
- 诚实守信
- 乐于助人
- 勇于尝试
- 坚韧不拔
- 乐观开朗
- 团结一心
```

## 小结

本文通过自定义`LogitsProcessor`，简单的实践了大语言模型在生成过程中屏蔽生成用户自定义词语的trick。在现实场景中，根据特定场景探索如何灵活的利用`LogitsProcessor`进行有针对性的控制生成模型的生成过程非常重要。

## 参考文献

【1】`https://github.com/huggingface/transformers/blob/v4.31.0/src/transformers/generation/logits_process.py`
