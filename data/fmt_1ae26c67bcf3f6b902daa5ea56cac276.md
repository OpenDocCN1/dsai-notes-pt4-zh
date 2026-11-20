
# Prompting 与数据预处理

```json
{
    'comment': '选择珠江花园的原因就是方便，有电动扶梯直接到达海边，周围餐馆、食廊、商场、超市、摊位一应俱全。酒店装修一般，但还算整洁。 泳池在大堂的屋顶，因此很小，不过女儿倒是喜欢。 包的早餐是西式的，还算丰富。 服务吗，一般',
    'prompt': '总体上来说很[MASK]。选择珠江花园的原因就是方便，有电动扶梯直接到达海边，周围餐馆、食廊、商场、超市、摊位一应俱全。酒店装修一般，但还算整洁。 泳池在大堂的屋顶，因此很小，不过女儿倒是喜欢。 包的早餐是西式的，还算丰富。 服务吗，一般',
    'mask_offset': 6,
    'label': '1'
}
```

可以看到输出的是转换后的模板，并且标记出了 `[MASK]` 在文本中的位置，符合我们的预期。

## 数据预处理

与之前一样，接下来我们就通过 `DataLoader` 库来按批(batch)加载数据，将文本转换为模型可以接受的 `token` IDs。

```python
from torch.utils.data import Dataset

class ChnSentiCorp(Dataset):
    def __init__(self, data_file):
        self.data = self.load_data(data_file)

    def load_data(self, data_file):
        Data = {}
        with open(data_file, 'rt', encoding='utf-8') as f:
            for idx, line in enumerate(f):
                items = line.strip().split('\t')
                assert len(items) == 2
                Data[idx] = {
                    'comment': items[0],
                    'label': items[1]
                }
        return Data

    def __len__(self):
        return len(self.data)

    def __getitem__(self, idx):
        return self.data[idx]

train_data = ChnSentiCorp('data/ChnSentiCorp/train.txt')
valid_data = ChnSentiCorp('data/ChnSentiCorp/dev.txt')
test_data = ChnSentiCorp('data/ChnSentiCorp/test.txt')
```

下面我们输出数据集的尺寸，并且打印出一个训练样本：

```python
print(f'train set size: {len(train_data)}')
print(f'valid set size: {len(valid_data)}')
print(f'test set size: {len(test_data)}')
print(next(iter(train_data)))
```

输出结果：

```
train set size: 9600
valid set size: 1200
test set size: 1200
{
    'comment': '选择珠江花园的原因就是方便，有电动扶梯直接到达海边，周围餐馆、食廊、商场、超市、摊位一应俱全。酒店装修一般，但还算整洁。 泳池在大堂的屋顶，因此很小，不过女儿倒是喜欢。 包的早餐是西式的，还算丰富。 服务吗，一般',
    'label': '1'
}
```

> 最常见的 `Prompting` 方法就是借助模板将问题转换为 `MLM` 任务来解决。这里我们定义模板形式为“总体上来说很 `[MASK]`。`x`”，其中 `x` 表示评论文本，并且规定如果 `[MASK]` 被预测为“好”就判定情感为“积极”，如果预测为“差”就判定为“消极”，即“积极”和“消极”标签对应的 `label word` 分别为“好”和“差”。

可以看到，`MLM` 任务与[序列标注](https://transformers.run/2022/03/18/transformers-note-6.html)任务很相似，也是对 `token` 进行分类，并且类别是整个词表，不同之处在于 `MLM` 任务只需要对文中特殊的 `[MASK]` `token` 进行标注。因此在处理数据时我们需要：

1. 记录下模板中所有 `[MASK]` `token` 位置，以便在模型的输出序列中将它们的表示取出。

1. 记录下 `label word` 对应的 `token ID`，因为我们实际上只关心模型在这些词语上的预测结果。

下面我们首先编写模板和 `verbalizer` 对应的函数：

```python
def get_prompt(x):
    prompt = f'总体上来说很[MASK]。{x}'
    return {
        'prompt': prompt,
       'mask_offset': prompt.find('[MASK]')
    }


def get_verbalizer(tokenizer):
    return {
        'pos': {'token': '好', 'id': tokenizer.convert_tokens_to_ids("好")},
        'neg': {'token': '差', 'id': tokenizer.convert_tokens_to_ids("差")}
    }
```

这里由于模板中只包含一个 `[MASK]` `token`，因此我们直接通过 `str.find()` 函数获取其位置，如果模板中包含多个 `[MASK]` `token`，就需要把他们的位置都记录下来。`verbalizer` 记录了从标签到对应 `label word` 的映射，这里我们通过 `tokenizer.convert_tokens_to_ids()` 来获取 `label word` 对应的 `token ID`。

例如，第一个样本转换后的模板为：

```python
from transformers import AutoTokenizer

checkpoint = "bert-base-chinese"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

comment = '这个宾馆比较陈旧了，特价的房间也很一般。总体来说一般。'

print('verbalizer:', get_verbalizer(tokenizer))

prompt_data = get_prompt(comment)
prompt, mask_offset = prompt_data['prompt'], prompt_data['mask_offset']

encoding = tokenizer(prompt, truncation=True)
tokens = encoding.tokens()
mask_idx = encoding.char_to_token(mask_offset)

print('prompt:', prompt)
print('prompt tokens:', tokens)
print('mask idx:', mask_idx)
```

```
verbalizer: {'pos': {'token': '好', 'id': 1962}, 'neg': {'token': '差', 'id': 2345}}
prompt: 总体上来说很[MASK]。这个宾馆比较陈旧了，特价的房间也很一般。总体来说一般。
prompt tokens: ['[CLS]', '总', '体', '上', '来', '说', '很', '[MASK]', '。', '这', '个', '宾', '馆', '比', '较', '陈', '旧', '了', '，', '特', '价', '的', '房', '间', '也', '很', '一', '般', '。', '总', '体', '来', '说', '一', '般', '。', '[SEP]']
mask idx: 7
```

可以看到，`BERT` 分词器正确地将“`[MASK]`”识别为一个 `token`，并且记录下 `[MASK]` `token` 在序列中的索引。

但是这种做法要求我们能够从词表中找到合适的 `label word` 来代表每一个类别，并且 `label word` 只能包含一个 `token`，而很多时候这是无法实现的。因此，另一种常见做法是为每个类别构建一个可学习的虚拟 `token`（又称伪 `token`），然后运用类别描述来初始化虚拟 `token` 的表示，最后使用这些虚拟 `token` 来扩展模型的 `MLM` 头。

例如，这里我们可以为“积极”和“消极”构建专门的虚拟 `token` “`[POS]`”和“`[NEG]`”，并且设置对应的类别描述为“好的、优秀的、正面的评价、积极的态度”和“差的、糟糕的、负面的评价、消极的态度”。下面我们扩展一下上面的 `verbalizer` 函数，添加一个 `vtype` 参数来区分两种 `verbalizer` 类型：

```python
def get_verbalizer(tokenizer, vtype):
    assert vtype in ['base', 'virtual']
    return {
        'pos': {'token': '好', 'id': tokenizer.convert_tokens_to_ids("好")},
        'neg': {'token': '差', 'id': tokenizer.convert_tokens_to_ids("差")}
    } if vtype == 'base' else {
        'pos': {
            'token': '[POS]', 'id': tokenizer.convert_tokens_to_ids("[POS]"),
            'description': '好的、优秀的、正面的评价、积极的态度'
        },
        'neg': {
            'token': '[NEG]', 'id': tokenizer.convert_tokens_to_ids("[NEG]"),
            'description': '差的、糟糕的、负面的评价、消极的态度'
        }
    }

vtype = 'virtual'
# add Label words
if vtype == 'virtual':
    tokenizer.add_special_tokens({'additional_special_tokens': ['[POS]', '[NEG]']})

print('verbalizer:', get_verbalizer(tokenizer, vtype=vtype))
```

输出结果：

```
verbalizer: {
    'pos': {'token': '[POS]', 'id': 21128, 'description': '好的、优秀的、正面的评价、积极的态度'}, 
    'neg': {'token': '[NEG]', 'id': 21129, 'description': '差的、糟糕的、负面的评价、消极的态度'}
}
```

**注意：**[“`[POS]`”](https://transformers.run/intro/2021-12-11-transformers-note-2/) 和 “`[NEG]`” 是我们新添加的 `token`，因此如模型与分词器中介绍的那样，我们首先需要通过 `tokenizer.add_special_tokens()` 将这两个 `token` 添加进模型的词表，然后才能获取它们的 `token ID`。

`Prompting` 方法实际输入的是转换后的模板，而不是原始文本，因此我们首先使用模板函数 `get_prompt()` 来更新数据集：

```python
class ChnSentiCorp(Dataset):
    def __init__(self, data_file):
        self.data = self.load_data(data_file)

    def load_data(self, data_file):
        Data = {}
        with open(data_file, 'rt', encoding='utf-8') as f:
            for idx, line in enumerate(f):
                items = line.strip().split('\t')
                assert len(items) == 2
                prompt_data = get_prompt(items[0])
                Data[idx] = {
                    'comment': items[0],
                    'prompt': prompt_data['prompt'],
                   'mask_offset': prompt_data['mask_offset'],
                    'label': items[1]
                }
        return Data

    def __len__(self):
        return len(self.data)

    def __getitem__(self, idx):
        return self.data[idx]
```

在实际应用场景下，模板的转换过程可能比文本中的要复杂得多（可能非常耗时），因此这里我们将其放置于数据集函数而不是 `DataLoader` 中，使得数据集返回的就是转换后的样本。
