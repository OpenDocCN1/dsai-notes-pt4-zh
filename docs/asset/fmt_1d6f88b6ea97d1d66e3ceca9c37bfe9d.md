
现在生成了嵌入矩阵。按照 `mean=emb_mean` 和 `std=emb_std` 的正态分布对矩阵进行初始化。遍历语料库中的 80000 个单词。对每一个单词而言，如果这个单词存在于 GloVe 中，我们就可以得到这个单词的嵌入，如果不存在那就略过。

```py
word_index = tokenizer.word_index
nb_words = MAX_NB_WORDS
embedding_matrix = np.random.normal(emb_mean, emb_std, (nb_words, embed_size))

oov = 0
for word, i in tqdm_notebook(word_index.items()):
    if i >= MAX_NB_WORDS: continue
    embedding_vector = embeddings_index.get(word)
    if embedding_vector is not None:
        embedding_matrix[i] = embedding_vector
    else:
        oov += 1

print(oov)

def get_rnn_model_with_glove_embeddings():
    embedding_dim = 300
    inp = Input(shape=(MAX_LENGTH, ))
    x = Embedding(MAX_NB_WORDS, embedding_dim, weights=[embedding_matrix], input_length=MAX_LENGTH, trainable=True)(inp)
    x = SpatialDropout1D(0.3)(x)
    x = Bidirectional(GRU(100, return_sequences=True))(x)
    avg_pool = GlobalAveragePooling1D()(x)
    max_pool = GlobalMaxPooling1D()(x)
    conc = concatenate([avg_pool, max_pool])
    outp = Dense(1, activation="sigmoid")(conc)

    model = Model(inputs=inp, outputs=outp)
    model.compile(loss='binary_crossentropy',
                  optimizer='adam',
                  metrics=['accuracy'])
    return model

rnn_model_with_embeddings = get_rnn_model_with_glove_embeddings()

filepath="./models/rnn_with_embeddings/weights-improvement-{epoch:02d}-{val_acc:.4f}.hdf5"
checkpoint = ModelCheckpoint(filepath, monitor='val_acc', verbose=1, save_best_only=True, mode='max')

batch_size = 256
epochs = 4

history = rnn_model_with_embeddings.fit(x=padded_train_sequences, 
                                      y=y_train, 
                                      validation_data=(padded_test_sequences, y_test), 
                                      batch_size=batch_size, 
                                      callbacks=[checkpoint], 
                                      epochs=epochs, 
                                      verbose=1)

best_rnn_model_with_glove_embeddings = load_model('./models/rnn_with_embeddings/weights-improvement-03-0.8372.hdf5')

y_pred_rnn_with_glove_embeddings = best_rnn_model_with_glove_embeddings.predict(
    padded_test_sequences, verbose=1, batch_size=2048)

y_pred_rnn_with_glove_embeddings = pd.DataFrame(y_pred_rnn_with_glove_embeddings, columns=['prediction'])
y_pred_rnn_with_glove_embeddings['prediction'] = y_pred_rnn_with_glove_embeddings['prediction'].map(lambda p: 
                                                                                                    1 if p >= 0.5 else 0)
y_pred_rnn_with_glove_embeddings.to_csv('./predictions/y_pred_rnn_with_glove_embeddings.csv', index=False)
y_pred_rnn_with_glove_embeddings = pd.read_csv('./predictions/y_pred_rnn_with_glove_embeddings.csv')
print(accuracy_score(y_test, y_pred_rnn_with_glove_embeddings))
0.837203100893 
```

准确率达到了 83.7%！来自外部词嵌入的迁移学习起了作用！本教程剩余部分都会在嵌入矩阵中使用 GloVe 嵌入。

## 6. 多通道卷积神经网络

这一部分实验了我曾了解过的卷积神经网络结构（http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/）。CNN 常用于计算机视觉任务。但最近我试着将其应用于 NLP 任务，而结果也希望满满。

简要了解一下当在文本数据上使用卷积网络时会发生什么。为了解释这一点，我从 wildm.com（一个很好的博客）中找到了这张非常有名的图（如下所示）。

了解一下使用的例子：I like this movie very much！（7 个分词）

* 每个单词的嵌入维度是 5。因此，可以用一个维度为 (7,5 的矩阵表示这句话。你可以将其视为一张「图」（数字或浮点数的矩阵）。
* 6 个滤波器，大小为 (2, 5) (3, 5) 和 (4, 5) 的滤波器各两个。这些滤波器应用于该矩阵上，它们的特殊之处在于都不是方矩阵，但它们的宽度和嵌入矩阵的宽度相等。所以每个卷积的结果将是一个列向量。
* 卷积产生的每一列向量都使用了最大池化操作进行下采样。
* 将最大池化操作的结果连接至将要传递给 `softmax` 函数进行分类的最终向量。

### 背后的原理是什么？

检测到特殊模式会激活每一次卷积的结果。通过改变卷积核的大小和连接它们的输出，你可以检测多个尺寸（2 个、3 个或 5 个相邻单词）的模式。

模式可以是像是「我讨厌」、「非常好」这样的表达式（词级的 ngram？），因此 CNN 可以在不考虑其位置的情况下从句子中分辨它们。

![](img/11551a493e9d76619dcf65692c04ae7b-fs8.png)

```py
def get_cnn_model():
    embedding_dim = 300

    filter_sizes = [2, 3, 5]
    num_filters = 256
    drop = 0.3

    inputs = Input(shape=(MAX_LENGTH,), dtype='int32')
    embedding = Embedding(input_dim=MAX_NB_WORDS,
                          output_dim=embedding_dim,
                          weights=[embedding_matrix],
                          input_length=MAX_LENGTH,
                          trainable=True)(inputs)

    reshape = Reshape((MAX_LENGTH, embedding_dim, 1))(embedding)
    conv_0 = Conv2D(num_filters, 
                    kernel_size=(filter_sizes[0], embedding_dim), 
                    padding='valid', kernel_initializer='normal', 
                    activation='relu')(reshape)

    conv_1 = Conv2D(num_filters, 
                    kernel_size=(filter_sizes[1], embedding_dim), 
                    padding='valid', kernel_initializer='normal', 
                    activation='relu')(reshape)
    conv_2 = Conv2D(num_filters, 
                    kernel_size=(filter_sizes[2], embedding_dim), 
                    padding='valid', kernel_initializer='normal', 
                    activation='relu')(reshape)

    maxpool_0 = MaxPool2D(pool_size=(MAX_LENGTH - filter_sizes[0] + 1, 1), 
                          strides=(1,1), padding='valid')(conv_0)

    maxpool_1 = MaxPool2D(pool_size=(MAX_LENGTH - filter_sizes[1] + 1, 1), 
                          strides=(1,1), padding='valid')(conv_1)

    maxpool_2 = MaxPool2D(pool_size=(MAX_LENGTH - filter_sizes[2] + 1, 1), 
                          strides=(1,1), padding='valid')(conv_2)
    concatenated_tensor = Concatenate(axis=1)(
        [maxpool_0, maxpool_1, maxpool_2])
    flatten = Flatten()(concatenated_tensor)
    dropout = Dropout(drop)(flatten)
    output = Dense(units=1, activation='sigmoid')(dropout)

    model = Model(inputs=inputs, outputs=output)
    adam = Adam(lr=1e-4, beta_1=0.9, beta_2=0.999, epsilon=1e-08, decay=0.0)

    model.compile(optimizer=adam, loss='binary_crossentropy', metrics=['accuracy'])

    return model

cnn_model_multi_channel = get_cnn_model()

plot_model(cnn_model_multi_channel, 
           to_file='./images/article_5/cnn_model_multi_channel.png', 
           show_shapes=True, 
           show_layer_names=True)
```

![](img/748a3d0d17e656ec87519664795b813c-fs8.png)

```py
filepath="./models/cnn_multi_channel/weights-improvement-{epoch:02d}-{val_acc:.4f}.hdf5"
checkpoint = ModelCheckpoint(filepath, monitor='val_acc', verbose=1, save_best_only=True, mode='max')

batch_size = 256
epochs = 4

history = cnn_model_multi_channel.fit(x=padded_train_sequences, 
                                    y=y_train, 
                                    validation_data=(padded_test_sequences, y_test), 
                                    batch_size=batch_size, 
                                    callbacks=[checkpoint], 
                                    epochs=epochs, 
                                    verbose=1)

best_cnn_model = load_model('./models/cnn_multi_channel/weights-improvement-04-0.8264.hdf5')

y_pred_cnn_multi_channel = best_cnn_model.predict(padded_test_sequences, verbose=1, batch_size=2048)

y_pred_cnn_multi_channel = pd.DataFrame(y_pred_cnn_multi_channel, columns=['prediction'])
y_pred_cnn_multi_channel['prediction'] = y_pred_cnn_multi_channel['prediction'].map(lambda p: 1 if p >= 0.5 else 0)
y_pred_cnn_multi_channel.to_csv('./predictions/y_pred_cnn_multi_channel.csv', index=False)
y_pred_cnn_multi_channel = pd.read_csv('./predictions/y_pred_cnn_multi_channel.csv')
print(accuracy_score(y_test, y_pred_cnn_multi_channel))
0.826409655689
```

准确率为 82.6%，没有 RNN 那么高，但是还是比 BOW 模型要好。也许调整超参数（滤波器的数量和大小）会带来一些提升？

## 7. RNN + CNN

RNN 很强大。但有人发现可以通过在循环层上叠加卷积层使网络变得更强大。

这背后的原理在于 RNN 允许嵌入序列和之前单词的相关信息，CNN 可以使用这些嵌入并从中提取局部特征。这两个层一起工作可以称得上是强强联合。

更多相关信息请参阅：http://konukoii.com/blog/2018/02/19/twitter-sentiment-analysis-using-combined-lstm-cnn-models/
