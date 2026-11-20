
如果将上面的代码运行 20 个 epoch，则模型在训练集上的准确率约为 0.78，在验证集上的准确率约为 0.35。这听起来很低，但是请注意，这是一个 5 类的分类问题，随机基线的准确率只有 0.20。

## 测试

为了测试刚刚训练的模型是否如预期，你需要构建一个预测器（`predictor`）。`predictor` 是一个提供基于 JSON 的接口的类，它被用于将输入数据传递给你的模型或将输出数据从模型中导出。接着，我便写了一个句子分类预测器（`https://github.com/mhagiwara/realworldnlp/blob/master/realworldnlp/predictors.py#L10`），将其用作句子分类模型的基于 JSON 的接口。

```py
tokens = ['This', 'is', 'the', 'best', 'movie', 'ever', '!']
predictor = SentenceClassifierPredictor(model, dataset_reader=reader)
logits = predictor.predict(tokens)['logits']
label_id = np.argmax(logits)

print(model.vocab.get_token_from_index(label_id, 'labels'))
```

运行这段代码后，你应该看到分类结果为「4」。「4」对应的是「非常积极」。所以你刚刚训练的模型正确地预测出了这是一个非常正面的电影评论。

![](img/2d1c94eb4a4ba15f356c96c72092e02b-fs8.png)

原文链接：`http://www.realworldnlpbook.com/blog/training-sentiment-analyzer-using-allennlp.html`

本文为机器之心编译，转载请联系本👸号获得授权。

✄------------------------------------------------

加入机器之心（全职记者 / 实习生）：hr@jiqizhixin.com

投稿或寻求报道：content@jiqizhixin.com

广告 & 商务合作：bd@jiqizhixin.com
