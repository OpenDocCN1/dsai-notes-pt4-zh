
```
def process_w2v_data(corpus, skip_window=2, method="skip_gram"):
    all_words = [sentence.split(" ") for sentence in corpus]
    # groups all the iterables together and produces a single iterable as output
    all_words = np.array(list(itertools.chain(*all_words)))
    vocab, v_count = np.unique(all_words, return_counts=True)
    vocab = vocab[np.argsort(v_count)[::-1]]

    print("All vocabularies are sorted by frequency in decresing oreder")
    v2i = {v: i for i, v in enumerate(vocab)}
    i2v = {i: v for v, i in v2i.items()}

    pairs = []
    js = [i for i in range(-skip_window, skip_window + 1) if i != 0]

    for c in corpus:
        words = c.split(" ")
        w_idx = [v2i[w] for w in words]
        if method == "skip_gram":
            for i in range(len(w_idx)):
                for j in js:
                    if i + j < 0 or i + j >= len(w_idx):
                        continue
                    pairs.append((w_idx[i], w_idx[i + j]))
        elif method.lower() == "cbow":
            for i in range(skip_window, len(w_idx) - skip_window):
                context = []
                for j in js:
                    context.append(w_idx[i + j])
                pairs.append(context + [w_idx[i]])
        else:
            raise ValueError

    pairs = np.array(pairs)
    print("5 expample pairs:\n", pairs[:5])
    if method.lower() == "skip_gram":
        x, y = pairs[:, 0], pairs[:, 1]
    elif method.lower() == "cbow":
        x, y = pairs[:, :-1], pairs[:, -1]
    else:
        raise ValueError
    return Dataset(x, y, v2i, i2v)
```

其结果如下：

```
step: 0  |  loss: 3.557112216949463
step: 200  |  loss: 3.3495876789093018
step: 400  |  loss: 2.5264182090759277
step: 600  |  loss: 2.787606954574585
step: 800  |  loss: 2.678286552429199
step: 1000  |  loss: 3.160764217376709
step: 1200  |  loss: 3.1824135780334473
step: 1400  |  loss: 2.823436737060547
step: 1600  |  loss: 2.750861167907715
step: 1800  |  loss: 2.4983673095703125
step: 2000  |  loss: 2.409540891647339
step: 2200  |  loss: 2.5646309852600098
step: 2400  |  loss: 2.5172579288482666
```

![](img/415ff343fc3eec9570b99c51de885359.png)

## 参考资料

[1]. https://github.com/yhangf/ML-NOTE

[2]. [机器是这样理解语言 - 词向量 - 自然语言处理 | 莫烦](https://link.zhihu.com/?target=https%3A//mofanpy.com/tutorials/machine-learning/nlp/intro-w2v/)Python (mofanpy.com)

> 夜雨翦春韭，新炊间黄粱。——杜甫《赠卫八处士》

编辑于 2022-02-01 21:18
