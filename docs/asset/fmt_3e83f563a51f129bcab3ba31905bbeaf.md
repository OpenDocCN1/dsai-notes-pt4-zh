
```py
def build_cnn(activation,
              dropout_rate,
              optimizer):
    model = Sequential()
    if (activation == 'selu'):
        model.add(Conv2D(32, kernel_size=(3, 3),
                         activation=activation,
                         input_shape=input_shape,
                         kernel_initializer='lecun_normal'))
        model.add(Conv2D(64, (3, 3), activation=activation, 
                         kernel_initializer='lecun_normal'))
        model.add(MaxPooling2D(pool_size=(2, 2)))
        model.add(AlphaDropout(0.25))
        model.add(Flatten())
        model.add(Dense(128, activation=activation, 
                        kernel_initializer='lecun_normal'))
        model.add(AlphaDropout(0.5))
        model.add(Dense(10, activation='softmax'))
    else:
        model.add(Conv2D(32, kernel_size=(3, 3),
                         activation=activation,
                         input_shape=input_shape))
        model.add(Conv2D(64, (3, 3), activation=activation))
        model.add(MaxPooling2D(pool_size=(2, 2)))
        model.add(Dropout(0.25))
        model.add(Flatten())
        model.add(Dense(128, activation=activation))
        model.add(Dropout(0.5))
        model.add(Dense(10, activation='softmax'))

    model.compile(
        loss='binary_crossentropy', 
        optimizer=optimizer, 
        metrics=['accuracy'])
    return model
```

使用 `GELU` 函数有个小问题；`Keras` 中目前还没有这个函数。幸好我们能轻松地向 `Keras` 添加新的激活函数。

```py
# Add the GELU function to Keras
def gelu(x):
    return 0.5 * x * (1 + tf.tanh(tf.sqrt(2 / np.pi) * (x + 0.044715 * tf.pow(x, 3))))
get_custom_objects().update({'gelu': Activation(gelu)})

# Add leaky-relu so we can use it as a string
get_custom_objects().update({'leaky-relu': Activation(LeakyReLU(alpha=0.2))})

act_func = ['sigmoid', 'relu', 'elu', 'leaky-relu', 'selu', 'gelu']
```

现在我们可以使用 `act_func` 数组中定义的不同激活函数训练模型了。我们会在每个激活函数上运行一个简单的 for 循环，并将结果添加到一个数组：

```py
result = []
for activation in act_func:
    print('\nTraining with -->{0}<-- activation function\n'.format(activation))

    model = build_cnn(activation=activation,
                      dropout_rate=0.2,
                      optimizer=Adam(clipvalue=0.5))

    history = model.fit(x_train, y_train,
                        validation_split=0.20,
                        batch_size=128, # 128 is faster, but less accurate. 16/32 recommended
                        epochs=100,
                        verbose=1,
                        validation_data=(x_test, y_test))

    result.append(history)

    K.clear_session()
    del model
print(result)
```

基于此，我们可以为每个激活函数绘制从 `model.fit()` 得到的历史图，然后看看损失和准确度结果的变化情况。现在我们可以为数据绘图了，我用 `matplotlib` 写了一小段代码：

```py
new_act_arr = act_func[1:]
new_results = result[1:]
def plot_act_func_results(results, activation_functions = []):
    plt.figure(figsize=(10,10))
    plt.style.use('dark_background')
    # Plot validation accuracy values
    for act_func in results:
        plt.plot(act_func.history['val_acc'])

    plt.title('Model accuracy')
    plt.ylabel('Test Accuracy')
    plt.xlabel('Epoch')
    plt.legend(activation_functions)
    plt.show()
    # Plot validation loss values
    plt.figure(figsize=(10,10))
    for act_func in results:
        plt.plot(act_func.history['val_loss'])

    plt.title('Model loss')
    plt.ylabel('Test Loss')
    plt.xlabel('Epoch')
    plt.legend(activation_functions)
    plt.show()

plot_act_func_results(new_results, new_act_arr)
```

这会得到如下图表：![](img/9117ec2448b8a42dfe2d67c8dceaf57e.jpg)

## 扩展阅读

### 书籍

下面是四本写得很赞的书：

*   Deep Learning，作者：Ian Goodfellow、Yoshua Bengio、Aaron Courville
*   The Hundred-Page Machine Learning Book，作者：Andriy Burkov
*   Hands-On Machine Learning with Scikit-Learn and TensorFlow，作者：Aurélien Géron
*   Machine Learning: A Probabilistic Perspective，作者：Kevin P. Murphy

### 论文

下面是本文讨论过的重要论文：

*   Leaky ReLU 论文：`https://ai.stanford.edu/~amaas/papers/relu_hybrid_icml2013_final.pdf`
*   ELU 论文：`https://arxiv.org/pdf/1511.07289.pdf`
*   SELU 论文：`https://arxiv.org/pdf/1706.02515.pdf`
*   GELU 论文：`https://arxiv.org/pdf/1606.08415.pdf`

[机器之心「SOTA 模型」](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650770891&idx=1&sn=25bde35991047a997337c8dd25350089&chksm=871a49b5b06dc0a36fc3407e3643550ef97f72b007e67c4f4be250bfd60c9fdc5389624569c0&scene=21#wechat_redirect)：22大领域、127个任务，机器学习SOTA研究一网打尽。![](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650770891&idx=1&sn=25bde35991047a997337c8dd25350089&chksm=871a49b5b06dc0a36fc3407e3643550ef97f72b007e67c4f4be250bfd60c9fdc5389624569c0&scene=21#wechat_redirect)

点击阅读原文，立即访问。
