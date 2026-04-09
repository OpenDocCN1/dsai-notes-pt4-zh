# 101N0102 ngram C 核心代码解读

> 来源：[https://aicarrier.feishu.cn/docx/T3MhdsYwaoTOJ0xQoCIcmjQMnQf](https://aicarrier.feishu.cn/docx/T3MhdsYwaoTOJ0xQoCIcmjQMnQf)

> 虽然不知道大家有没有懂ngram，🐧小弟但是经历了不懈的学习总之是懂了。来写一个ngram c代码解读吧。之前很少写代码解读，总感觉容易写成流水账。今天第一次体验一下

### 写在前面

代码仓库在：https://github.com/EurekaLabsAI/ngram

还是来一个最简单的简介吧：

N-Gram是一种基于统计语言模型的算法。它的基本思想是将文本里面的内容按照字节进行大小为N的滑动窗口操作，形成了长度是N的字节片段序列。

每一个字节片段称为gram，对所有gram的出现频度进行统计，并且按照事先设定好的阈值进行过滤，形成关键gram列表，也就是这个文本的向量特征空间，列表中的每一种gram就是一个特征向量维度。

### 代码整体解读

我们这里要训练的是一个类似花名册的东西，在data/train.txt下。最终的目的是通过给定的n的大小，使用一个{1, 2, 3, 4, 5, 6} - Gram的算法来进行采样生成一个新的名字。

### 先玩玩再说

整体来看呢，这版c代码会比python代码快很多。如果你体验了python代码和c代码，这俩简直就不是一个速度。。。

#### 编译代码

c写完代码需要编译再执行，简单编译下

```
gcc -O3 ngram.c -o ngram -lm
```

参数解释：

+   -O3: 用O3进行编译期优化

+   -lm：使用数学相关的库

#### 不同的seq_len的结果

编译好之后，就要运行啦。在这里ngram模型接受两个输入:

+   seq_len: 第一个是seq_len。它代表了模型使用几个字节组成一个片段来统计概率。

+   Smoothing: 另一个是smoothing，这代表了一个光滑值。光滑值是，如果某个片段没有出现时。不存在它的频率，这样也无法计算它的概率。无法计算loss，也不会把这种可能性从词表里采样到。

让我们先试试不同seq_len的结果：

![](img/6cbac2b4f73f62ffcc7ca2b626511b5e.png)

![](img/4a26ab85ca1086ab338b7a88b5b519a9.png)

![](img/d21128098d80d4da8b8866719c15f1ed.png)

![](img/8e2bdaefc2725587ebdbc242b6848b89.png)

![](img/7841e9c602d86cb5cc34583f5046ecd4.png)

![](img/b0fffcd08ad630012e523bc8dc7a476e.png)

能看到在n是4的时候，text的PPL最低。生成的名字也会更符合认知。

#### 不同的smoothing的结果

这里不再展示，smoothing变化时，PPL也会随之变化。代码预设的n = 4，s=0.1得到最低的PPL。

### 从主函数开始

```
// the arity of the n-gram model (1 = unigram, 2 = bigram, 3 = trigram, ...)
int seq_len = 4;
float smoothing = 0.1f;
```

这里定义了两个最重要的变量seq_len和smoothing，控制模型分片大小和光滑程度的参数。

```
// simple argparse, example usage: ./ngram -n 4 -s 0.1
for (int i = 1; i &lt; argc; i+=2) {
    if (i + 1 &gt;= argc) { error_usage(); } // must have arg after flag
    if (argv[i][0] != &#x27;-&#x27;) { error_usage(); } // must start with dash
    if (!(strlen(argv[i]) == 2)) { error_usage(); } // must be -x (one dash, one letter)
    if (argv[i][1] == &#x27;n&#x27;) { seq_len = atoi(argv[i+1]); }
    else if (argv[i][1] == &#x27;s&#x27;) { smoothing = atof(argv[i+1]); }
    else { error_usage(); }
}
```

从命令行中读取n和s，默认为n=4，s=0.1。

### 碰到NgramModel定义和初始化

接下来在主函数里，代码定义了NgramModel，并调用ngram_init进行了初始化。让我们看看这是什么？

```
NgramModel model;
ngram_init(&amp;model, NUM_TOKENS, seq_len, smoothing);
```

#### 定义NgramModel

```
// ----------------------------------------------------------------------------
// ngram model

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

NgramModel通过一个结构体定义，分别是三个超参数，seq_len, vocab_size(词表大小，这里是26个字符+截止符),smoothing。三个参数，num_counts, counts和ravel_buffer。至于它们都是什么，请继续往下看。

#### 初始化NgramModel

```
ngram_init(&amp;model, NUM_TOKENS, seq_len, smoothing);
void ngram_init(NgramModel *model, const int vocab_size, const int seq_len, const float smoothing) {
    // sanity check and store the hyperparameters
    assert(vocab_size &gt; 0);
    assert(seq_len &gt;= 1 &amp;&amp; seq_len &lt;= 6); // sanity check max ngram size we&#x27;ll handle
    model-&gt;vocab_size = vocab_size;
    model-&gt;seq_len = seq_len;
    model-&gt;smoothing = smoothing;
    // allocate and init memory for counts (np.zeros in numpy)
    model-&gt;num_counts = powi(vocab_size, seq_len);
    model-&gt;counts = (uint32_t*)mallocCheck(model-&gt;num_counts * sizeof(uint32_t));
    for (size_t i = 0; i &lt; model-&gt;num_counts; i++) {
        model-&gt;counts[i] = 0;
    }
    // allocate buffer we will use for ravel_index
    model-&gt;ravel_buffer = (int*)mallocCheck(seq_len * sizeof(int));
}
```

初始化时，ngram_init需要一个类型为NgramModel结构题定义的指针，一个vocab_size大小，一个seq_len，一个smoothing。

##### 实现：

首先做了一系列assert。

+   定义了模型的vocab_size是NUM_TOKENS，也就是27。

+   定义了seq_len和smoothing。

+   定义了num_counts 是  。这里powi同理于std::pow，就不展开了。

+   给counts分配空间并全部初始化为0

> 上面两步的感觉其实就像是torch.zeros([vocab_size, vocab_size, ..., vocab_size], dtype=torch.int32)。

> n是几维度就是几。

+   ravel_buffer， 这是一个内部缓存，后续详细介绍。

### 碰到DataLoader相关的一系列操作

```
// train the model
DataLoader train_loader;
dataloader_init(&amp;train_loader, &quot;data/train.txt&quot;, seq_len);
while (dataloader_next(&amp;train_loader)) {
}
dataloader_free(&amp;train_loader);
```

看这段实现，我们首先实例化了一个train_loader，之后初始化了train_loader。之后开始训练，让我们对照实例化和原始代码进行讲解。

#### 定义DataLoader & 定义Tape

```
typedef struct {
    FILE *file;
    int seq_len;
    Tape tape;
} DataLoader;
```

很好理解的是file和seq_len。那Tape是什么呢？简单理解就是某个片段。

##### 定义一个缓冲区Tape

```
typedef struct {
    int n;
    int length;
    int* buffer;
} Tape;
```

Tape 存储一个固定窗口的token，功能类似于一个有限队列。以方便Ngram模型的更新。

+   N 当前缓冲区中token数量

+   length：缓冲区长度

+   buffer：缓冲区

#### 初始化DataLoader & 初始化Tape

```
dataloader_init(&amp;train_loader, &quot;data/train.txt&quot;, seq_len);
void dataloader_init(DataLoader *dataloader, const char *path, const int seq_len) {
    dataloader-&gt;file = fopenCheck(path, &quot;r&quot;);
    dataloader-&gt;seq_len = seq_len;
    tape_init(&amp;dataloader-&gt;tape, seq_len);
}
```

首先我们让dataloader读取了一个文件，之后确定n大小。最后用seq_len初始化dataloader中的tape。

```
void tape_init(Tape *tape, const int length) {
    // we will allow a buffer of length 0, useful for the Unigram model
    assert(length &gt;= 0);
    tape-&gt;length = length;
    tape-&gt;n = 0; // counts the number of elements in the buffer up to max
    tape-&gt;buffer = NULL;
    if (length &gt; 0) {
        tape-&gt;buffer = (int*)mallocCheck(length * sizeof(int));
    }
}
```

通过seq_len定义了tape的length。初始化了某个片段的统计值，并分配了length长度的空间给tape。

#### 迭代DataLoader->dataloader_next & tape_update

```
dataloader_next(&amp;train_loader)
int dataloader_next(DataLoader *dataloader) {
    // returns 1 if a new window was read, 0 if the end of the file was reached
    int c;
    while (1) {
        c = fgetc(dataloader-&gt;file);
        if (c == EOF) {
            break;
        }
        int token = tokenizer_encode(c);
        int ready = tape_update(&amp;dataloader-&gt;tape, token);
        if (ready) {
            return 1;
        }
    }
    return 0;
}
```

+   读取过程：读啊读，如果读到了文件最后就break。每个读进来的字符(\n, a,b,c...z)通过tokenizer_encode转换成数字(0-26)。我们把0定义成结束符号,对应的就是\n。（很合理是不是，正好换行是结束。可以换下一个词了。

+   更新过程：

```
int tape_update(Tape *tape, const int token) {
    // returns 1 if the tape is ready/full, 0 otherwise
    if (tape-&gt;length == 0) {
        return 1; // unigram tape is always ready
    }
    // shift all elements to the left by one
    for (int i = 0; i &lt; tape-&gt;length - 1; i++) {
        tape-&gt;buffer[i] = tape-&gt;buffer[i + 1];
    }
    // add the new token to the end (on the right)
    tape-&gt;buffer[tape-&gt;length - 1] = token;
    // keep track of when we&#x27;ve filled the tape
    if (tape-&gt;n &lt; tape-&gt;length) {
        tape-&gt;n++;
    }
    return (tape-&gt;n == tape-&gt;length);
```

开始的时候tape空空如也，我们一个个字符从tape的右侧读进来，然后慢慢入队。等这个队列满了的时候，我们会返回return (tape->n == tape->length);也就是true。这时候会触发:

```
if (ready) {
    return 1;
}
```

说明我们第一次堆满这个队列，那么就可以开始我们的更新流程了～

#### 释放DataLoader -> dataloader_free

```
void dataloader_free(DataLoader *dataloader) {
    fclose(dataloader-&gt;file);
    tape_free(&amp;dataloader-&gt;tape);
}
```

没啥好说的，关闭文件，释放缓冲区一气呵成～

### 终于要开始训练啦

```
ngram_train(&amp;model, train_loader.tape.buffer);
```

训练代码异常的简单，只有一行。让我们看看在这里都做了啥吧。

```
ngram_train(&amp;model, train_loader.tape.buffer);
void ngram_train(NgramModel *model, const int* tape) {
    // tape here is of length `seq_len`, and we want to update the counts
    size_t offset = ravel_index(tape, model-&gt;seq_len, model-&gt;vocab_size);
    assert(offset &gt;= 0 &amp;&amp; offset &lt; model-&gt;num_counts);
    model-&gt;counts[offset]++;
}
```

我们的输入是我们model，和一个被填满的队列。

#### ravel_index开始寻址

```
size_t offset = ravel_index(tape, model-&gt;seq_len, model-&gt;vocab_size);
size_t ravel_index(const int* index, const int n, const int dim) {
    // convert an n-dimensional index into a 1D index (ravel_multi_index in numpy)
    // each index[i] is in the range [0, dim)
    size_t index1d = 0;
    size_t multiplier = 1;
    for (int i = n - 1; i &gt;= 0; i--) {
        int ix = index[i];
        assert(ix &gt;= 0 &amp;&amp; ix &lt; dim);
        index1d += multiplier * ix;
        multiplier *= dim;
    }
    return index1d;
}
```

根据n和词表大小进行1D训址，做这步是因为c语言中高维数组定义寻址较复杂，直接压缩成单维度进行寻址。

+   在每个维度上寻找索引，这个操作在每个维度上寻找了它所对应的索引

```
int ix = index[i];
```

+   依次迭代维度

这里可以理解成multiplier是一个寻址的基，每一次迭代新的维度需要给基 * 一个词表大小。

```
index1d += multiplier * ix;
multiplier *= dim;
```

感觉说的还是很抽象，那让咱们来画个图吧：

下面是一个8x8的表，如果我们想找到这个3，3的坐标一维的表示，是不是需要像这样

| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 |
| 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 |
| 24 | 25 | 26 | 我想找到这儿，它的坐标是(3, 3) |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |

扩展到高维度也是一样的，它返回了一个token在一维上的偏移值。然后我们把这个偏移值在模型定义的一个counts里++ ... 一直训练到结束。

### 再说说这个模型的推理吧

```
// sample from the model for 200 time steps
Tape sample_tape;
tape_init(&amp;sample_tape, seq_len - 1);
tape_set(&amp;sample_tape, EOT_TOKEN); // fill with EOT tokens to init
uint64_t rng = 1337;
for (int i = 0; i &lt; 200; i++) {
    ngram_inference(&amp;model, sample_tape.buffer, probs);
    float coinf = random_f32(&amp;rng);
    int token = sample_discrete(probs, NUM_TOKENS, coinf);
    tape_update(&amp;sample_tape, token);
    char c = tokenizer_decode(token);
    printf(&quot;%c&quot;, c);
}
printf(&quot;\n&quot;);
```

是不是看完了训练，推理都看起来眉清目秀了～在推理时，我们先定义了一个Tape，填充满结束符号，也就是0

```
ngram_inference(&amp;model, sample_tape.buffer, probs);
void ngram_inference(NgramModel *model, const int* tape, float* probs) {
    // here, tape is of length `seq_len - 1`, and we want to predict the next token
    // probs should be a pre-allocated buffer of size `vocab_size`

    // copy the tape into the buffer and set the last element to zero
    for (int i = 0; i &lt; model-&gt;seq_len - 1; i++) {
        model-&gt;ravel_buffer[i] = tape[i];
    }
    ...
}
```

ravel_buffer在推理时使用，我们给ravel_buffer最后填上0元素。

```
model-&gt;ravel_buffer[model-&gt;seq_len - 1] = 0;
// find the offset into the counts array based on the context
size_t offset = ravel_index(model-&gt;ravel_buffer, model-&gt;seq_len, model-&gt;vocab_size);
// seek to the row of counts for this context
uint32_t* counts_row = model-&gt;counts + offset;
```

在这里还有一个点，每次ravel_buffer的最后一个值都是0，这样索引时找到的offset都是某一行的开始。counts_row能找到特定行的开始位置。

```
// calculate the sum of counts in the row
float row_sum = model-&gt;vocab_size * model-&gt;smoothing;
for (int i = 0; i &lt; model-&gt;vocab_size; i++) {
    row_sum += counts_row[i];
}
```

添加smooting为了防止row_sum为0的情况发生。

```
if (row_sum == 0.0f) {
    // the entire row of counts is zero, so let&#x27;s set uniform probabilities
    float uniform_prob = 1.0f / model-&gt;vocab_size;
    for (int i = 0; i &lt; model-&gt;vocab_size; i++) {
        probs[i] = uniform_prob;
    }
} else {
    // normalize the row of counts into probabilities
    float scale = 1.0f / row_sum;
    for (int i = 0; i &lt; model-&gt;vocab_size; i++) {
        float counts_i = counts_row[i] + model-&gt;smoothing;
        probs[i] = scale * counts_i;
    }
}
```

当然如果你的smoothing设成0了，那不好意思。还是会为0，这时候就每个位置均分。否则计算这一组tape每个字符的概率。拿n=4举例，现在这步我们算得就是：

之后我们会在这组概率里随机采样一下。采样就不多说了😋

```
int token = sample_discrete(probs, NUM_TOKENS, coinf);
```

采样完之后还有什么很关键？那就是把采样的结果加进tape里啊！

```
tape_update(&amp;sample_tape, token);
```

这样下次预测就会根据这次的结果再进行预测啦，一直预测到生成一个0。0是截止符号，也是换行符号。我们就会开始预测下一个词。

### 模型咋测试好不好呢

最后还是要讲讲测试，大部分的接口都在上文同步过了。

```
// evaluate the test split loss
DataLoader test_loader;
dataloader_init(&amp;test_loader, &quot;data/test.txt&quot;, seq_len);
float sum_loss = 0.0f;
int count = 0;
while (dataloader_next(&amp;test_loader)) {
    // note that ngram_inference will only use the first seq_len - 1 tokens in buffer
    ngram_inference(&amp;model, test_loader.tape.buffer, probs);
    // and the last token in the tape buffer is the label
    int target = test_loader.tape.buffer[seq_len - 1];
    // negative log likelihood loss
    sum_loss += -logf(probs[target]);
    count++;
}
dataloader_free(&amp;test_loader);
float mean_loss = sum_loss / count;
float test_perplexity = expf(mean_loss);
printf(&quot;test_loss %f, test_perplexity %f\n&quot;, mean_loss, test_perplexity);
```

这里我们主要是计算一个negative log likelihood loss。它的计算流程是训练好的模型，每次test加载一个新的字符。并进行生成。找到新的字符应该是什么。

```
int target = test_loader.tape.buffer[seq_len - 1];
```

并得到这个字符的negative log likelihood loss

```
-logf(probs[target]);
```

之后一直累加就好了，计算平均loss和PPL

```
float mean_loss = sum_loss / count;
float test_perplexity = expf(mean_loss);
```

### 最后的最后

这就是关于ngram c的全部啦，又是肝到天亮的一次。学习c的代码能让你更感觉到融会贯通，更加掌握ngram。确实很有意思～