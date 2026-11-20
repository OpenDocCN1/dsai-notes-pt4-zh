
> 由于每次`run`的时候计算都要等待`TFrecordReader`读出数据，而没用利用`batch`的方式。如果用多线程`batch`可以在计算期间异步读取数据。在`TensorFlow`所有例子中都是使用`TFRecordReader`的`read`接口去读取数据，再用`batch`将数据多线程抓过来。但是，其实这样做加速很慢。需要使用`TFRecordReader`的`read_up_to`的方法配合`batch`的`equeue_many=True`的参数，才可以做到最大的加速比。使用`tf.train.batch`的API后，性能提升了38倍。

> 此时，性能已经基本达到预期了。例如整体数据量是2亿，按照以前的性能计算1000条/秒，大概需要运行55个小时。而现在大概需要运行87分钟，再加上预处理（15分钟）与预拉取数据（10分钟）的时间，在不增加任何计算资源的情况下大概需要2个小时以内。而如果是并行处理，则可以在分钟级完成训练。

## （二）线上预估

线上流量是模型效果的试金石。离线训练好的模型只有参与到线上真实流量预估，才能发挥其价值。在演化的过程中，我们开发了一套稳定可靠的线上预估体系，提高了模型迭代的效率。

### 1. 模型同步

开发高可用的同步组件：用户只需要提供线下训练好的模型的`HDFS`路径，该组件会自动同步到线上服务机器上。该组件基于`HTTPFS`实现，它是美团离线计算组提供的`HDFS`的HTTP方式访问接口。同步过程如下：

+ 同步前，检查模型`md5`文件，只有该文件更新了，才需要同步。

+ 同步时，随机链接`HTTPFS`机器并限制下载速度。

+ 同步后，校验模型文件`md5`值并备份旧模型。

+ 同步过程中，如果发生错误或者超时，都会触发报警并重试。依赖这一组件，我们实现了在2min内可靠的将模型文件同步到线上。

### 2. 模型计算

当前线上有两套并行的预估计算服务。

#### 基于 `TF Serving` 的模型服务

`TF Serving` 是 `TensorFlow` 官方提供的一套用于在线实时预估的框架。它的突出优点是：和`TensorFlow`无缝链接，具有很好的扩展性。使用`TF serving`可以快速支持`RNN`、`LSTM`、`GAN`等多种网络结构，而不需要额外开发代码。这非常有利于我们模型快速实验和迭代。

> 使用这种方式，线上服务需要将特征发送给`TF Serving`，这不可避免引入了网络IO，给带宽和预估时延带来压力。我们尝试了以下优化，效果显著。

并发请求。一个请求会召回很多符合条件的广告。在客户端多个广告并发请求`TF Serving`，可以有效降低整体预估时延。

特征ID化。通过将字符串类型的特征名哈希到64位整型空间，可以有效减少传输的数据量，降低使用的带宽。

在典型的五层网络（512*256*256*256*128）下，单个广告的预估时延约4800μs，具体见下图：

![](img/56c6a2783e40fbf5bfb45a0170fbd3bb.png)

#### 定制的模型计算实现

> 由于广告线上服务需要极高的性能，对于主流深度学习模型，我们也定制开发了具体计算实现。

全连接DNN模型中使用`Relu`作为激活函数时，可用滚动数组、剪枝、寄存器和CPU Cache，具体如下：

```
// 滚动数组
int nextLayerIndex = currentLayerIndex ^ 1 ;
System.arraycopy(bias, bOff, data[nextLayerIndex], 0, nextLayerSize);
for (int i = 0; i &lt; currentLayerSize; i ++) {
    float value = data[currentLayerIndex][i];
    // 剪枝
    if (value &gt; 0.0) {
        // 寄存器
        int index = wOff + i * nextLayerSize;
        // CPU 缓存友好
        for (int j = 0; j &lt; nextLayerSize; j++) {
            data[nextLayerIndex][j] += value * weights[index + j];
        }
    }
}
for (int i = 0; i &lt; nextLayerSize; k++) {
    data[nextArrayIndex][i] = ReLu(data[nextArrayIndex][i]);
}
arrayIndex = nextArrayIndex;
```

优化后的单个广告预估时延约650μs，见下图：

![](img/7b7c99c365a14ba36c8727749d67d25b.png)

> 综上，当前线上预估采取“两条腿走路”的策略。利用`TF Serving`快速实验新的模型结构，以保证迭代效率；一旦模型成熟切换主流量，开发定制实现，以保证线上性能。

### 模型效果

> 借助于分层实验平台，我们可以方便的分配流量，完成模型的小流量实验上线。该分层实验平台同时提供了分钟粒度的小流量实时效果数据，便于模型评估和效果监控。
