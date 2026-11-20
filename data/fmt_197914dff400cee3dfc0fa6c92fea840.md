
# 超参数搜索

```python
# hyperparameter search with grid search over the validation set
seq_lens = [3, 4, 5]
smoothings = [0.03, 0.1, 0.3, 1.0]
best_loss = float('inf')
best_kwargs = {}
for seq_len, smoothing in itertools.product(seq_lens, smoothings):
    # train the n-gram model
    model = NgramModel(vocab_size, seq_len, smoothing)
    for tape in dataloader(train_tokens, seq_len):
        model.train(tape)
    # evaluate the train/val loss
    train_loss = eval_split(model, train_tokens)
    val_loss = eval_split(model, val_tokens)
    print("seq_len %d | smoothing %.2f | train_loss %.4f | val_loss %.4f"
          % (seq_len, smoothing, train_loss, val_loss))
    # update the best hyperparameters
    if val_loss < best_loss:
        best_loss = val_loss
        best_kwargs = {'seq_len': seq_len, 'smoothing': smoothing}
```

# 第二部分 训练模型

```python
# re-train the model with the best hyperparameters
seq_len = best_kwargs['seq_len']
print("best hyperparameters:", best_kwargs)
model = NgramModel(vocab_size, **best_kwargs)
for tape in dataloader(train_tokens, seq_len):
    model.train(tape)
```

这部分就是使用最优参数训练得到最佳模型。其实这一步可以和上面的超参搜索步骤结合。

## 从模型抽样

这里是从模型中抽象200个token。起始token是准备 `seq_len - 1` 个 `EOT_TOKEN` 前缀序列，预测下一个token，得到 `vocab_size` 大小的概率数组，使用随机方式产生`coinf`，使用离散抽样 `sample_discrete` 函数里获取累计概率值刚大于 `coinf` 的索引 作为下一个`token_id`，然后截取`tape[1:]` 的字符串作为新的前缀预测下一个token。

```python
# sample from the model
tape = [EOT_TOKEN] * (seq_len - 1)
for _ in range(200):
    # print(f"tape: {tape}")
    probs = model(tape)
    # sample the next token
    coinf = random.random()
    probs_list = probs.tolist()
    next_token = sample_discrete(probs_list, coinf)
    # otherwise update the token tape, print token and continue
    next_char = token_to_char[next_token]
    # update the tape
    tape.append(next_token)
    if len(tape) > seq_len - 1:
        tape = tape[1:]
    print(next_char, end='')
print() # newline
```

## 测试集评估 loss 和 困惑度

```python
# at the end, evaluate and report the test loss
test_loss = eval_split(model, test_tokens)
test_perplexity = np.exp(test_loss)
print("test_loss %f, test_perplexity %f" % (test_loss, test_perplexity))
```

# C 代码解读

C代码的实现逻辑基本和 python 版本一致，Python 版本代码中多了超参数搜索这个步骤，而 C 版本中直接使用了最优的超参进行训练和推理，所以看起来C版本快很多，去掉 Python 超参搜索后，python 速度明显快不少，当然比 C 语言版本还是慢一个数量级的。

两个版本的基础流程都是 `dataloader` 从文件中加载数据，然后喂给 `NgramModel` 做训练。

这个是C代码中模型定义：

```c
typedef struct {
    // hyperparameters
    int seq_len;
    int vocab_size;
    float smoothing;
    // parameters
    size_t num_counts; // size_t because int would only handle up to 2^31-1 ~= 2 billion counts
    uint32_t* counts;
    // internal buffer for ravel_index
    int* ravel_buffer;
} NgramModel;
```

其中的 `counts` 就是计数统计缓存，和 python 中的 `seq_len` 个 `vocab_size` 维度的数据大小一致，只是C中使用一维度的线性数组表示多维数组，所以需要将 n-gram 的滑动窗口的数组token id 转为线性地址，使用的函数是`ravel_index`:

```c
size_t ravel_index(const int* index, const int n, const int dim) {
    // convert an n-dimensional index into a 1D index (ravel_multi_index in numpy)
    // each index[i] is in the range [0, dim)
    size_t index1d = 0;
    size_t multiplier = 1;
    for (int i = n - 1; i >= 0; i--) {
        int ix = index[i];
        assert(ix >= 0 && ix < dim);
        index1d += multiplier * ix;
        multiplier *= dim;
    }
    return index1d;
}
```

`ravel_index` 执行逻辑是内部根据`index`数组，这个数组就是n-gram长为 `n` 的滑动窗口 int 型的 token id 数组，从低位(对应低维)到高位(对应高维)进行维度的计算累加，

使用方法：

```c
void ngram_train(NgramModel model, const int tape) {
    // tape here is of length seq_len, and we want to update the counts
    size_t offset = ravel_index(tape, model->seq_len, model->vocab_size);
    assert(offset >= 0 && offset < model->num_counts);
    model->counts[offset]++;
}
```

这里的`tape`其实就是token id 的 `seq_len` 的数组。

# 总结

整体上看懂 Python 和 C 版本的代码需要先理解内部的数学原理，代码几乎都是按照数学逻辑实现的，当然n-gram的数学模型和数学公式都还比较简单。

具体数学公式的代码实现还有一些小技巧，比如：

1. Python如何实现n-gram的`seq_len`维度的数组和统计
2. C版本的如何将代表n-gram滑动窗口的多维度的数组转为一维度的索引地址
3. C版本是如何更细节的抽象模型和数据加载

看懂代码后对这些技巧有更深度的体会

# 主要参考：

`https://web.stanford.edu/~jurafsky/slp3/3.pdf`
