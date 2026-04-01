# model_modify

> 来源：[https://v11enp9ok1h.feishu.cn/docx/EdthdEDV0oYllBxnWHFc4FQknbc](https://v11enp9ok1h.feishu.cn/docx/EdthdEDV0oYllBxnWHFc4FQknbc)

# 模型修改工具集合

## 词表扩充

1.  场景：

+   1.1 对国外的大模型，添加新的中文词表；

+   1.2 对通用大模型，添加垂直领域的词表；

1.  实现逻辑：

+   2.1 准备特定领域的文本进行分词，得到一串词表列表（不在乎形式，最后数据形式为List[str]即可，比如：['疎', '很多人都', '樣的', '商人', '藥物'])

+   2.2 使用tokenizer.add_tokens方法，将上述词表添加上去。

+   2.3 使用model.resize_token_embeddings方法，重新调整embedding的大小。

+   2.4 将tokenizer和model保存起来。

+   2.5 冻结部分参数，只训练embedding对应的参数。

前四步，参考代码:code01_扩充词表.ipynb

## 词表裁切

1.  场景：

+   1.1 对多语言模型，做词表裁切（丢掉用不到的词表），比如只保留bloom模型的中文部分；

1.  实现逻辑：

+   2.1 查看模型的所有词表；

+   2.2 找到不用的词表；

+   2.3 把tokenizer里面的词表删掉，并且重新建立索引；

+   2.4 把model里面的embedding和lm_head的矩阵的行或者列删掉；

+   2.5 保存

参考代码:[https://github.com/yuanzh](https://github.com/yuanzhoulvpi2017/LLMPruner)oulvpi2017/LLMPruner