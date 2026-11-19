
# 004 归一化

> 来源：[https://my.feishu.cn/docx/YZGNdgUQlo03aCxVLt6c2kzonhg](https://my.feishu.cn/docx/YZGNdgUQlo03aCxVLt6c2kzonhg)

## 一、归一化任务简介

### 1、什么是归一化

归一化，指的是把大数量级特征转化到较小的数量级下，通常是[0, 1]或[-1, 1]。

举个例子，比如说我们想把**身高体重预测**归一化，那么就需要把**身高体重的数据**，给压缩到[0, 1]或者[-1, 1]之间。

再比如说，想把**房价预测**问题给归一化，那么就需要把影响房价的数据给压缩到[0, 1]或者[-1, 1]之间。

### 2、为何要归一化？

绝大多数`Tesorflow.js`的模型都不是给特别大的数设计的 —— 拿房价预测来说，它的数据有可能是上万的房价以及房屋面积，而根据单位的不同，它们的数量级也有可能非常大，比如说房价可能是几百万，房屋面积可能是几百平方米，这些数量级都很大。那为了能让`Tensorflow.js`的模型可以更好地进行训练，进行预测，我们最好就还是要把这些数据集先压缩一下，然后再喂给模型，让他去训练，去学习。

将**不同数量级的特征**转换到**同一数量级**，**防止某个特**征影响过大 —— 同样拿房价预测来讲，有可能压缩过后的数据，有些很大，有些又小了。那为了方便计算，就需要将它们都一起，转换到同一数量级，让后面可以更好地计算。

### 3、归一化操作步骤

**上面了**解了归一化是什么，下面谈谈归一化的过程。如下：

+ 首先先准备**身高体重训练数据**并归一化
+ 训练模型并预测，将结果**反归一化**为正常数据

## 二、归一化训练数据

下面，我们将用`tensorflow.js`，来归一化整个训练数据。

### 1、操作步骤

+ 准备身高体重训练数据
+ 使用`tfvis`来可视化训练数据
+ 使用`Tensorflow.js`的`API`进行归一化

### 2、训练过程

具体代码如下👇🏻：

```javascript
import * as tf from '@tensorflow/tfjs';
import * as tfvis from '@tensorflow/tfjs-vis';

window.onload = async () => {
  const heights = [150, 160, 170];
  const weights = [40, 50, 60];

  tfvis.render.scatterplot(
    { name: '身高体重训练数据' },
    { values: heights.map((x, i) => ({ x, y: weights[i] })) },
    // 设置x轴和y轴的范围
    {
      xAxisDomain: [140, 180],
      yAxisDomain: [30, 70]
    }
  );

  // 让每一个身高数据，分别减去150，然后再除以20
  const inputs = tf.tensor(heights).sub(150).div(20);
  // 让每一个体重数据，分别减去40，然后再除以20
  const labels = tf.tensor(weights).sub(40).div(20);

};
```

最终展示效果如下👇🏻：

![](img/2f08335ff83920cb0b017774dc990d1a.png)

可以看到，每个点都正确渲染了。

## 三、训练、预测、反归一化

**上面**我**们对**数据进**行了归一**化，接下来，要把这些数据做进一步的训练、预测，以及反归一化。

### 1、操作步骤

+ 定义一个神经网络模型
+ 将归一化后的数据喂给模型学习——训练模型
+ 预测后，把结果反归一化为正常数据

### 2、训练过程

首先创建个文件，命名为`script.js`。具体代码如下：

```javascript
import * as tf from '@tensorflow/tfjs';
import * as tfvis from '@tensorflow/tfjs-vis';

window.onload = async () => {
  const heights = [150, 160, 170];
  const weights = [40, 50, 60];

  tfvis.render.scatterplot(
    { name: '身高体重训练数据' },
    { values: heights.map((x, i) => ({ x, y: weights[i] })) },
    // 设置x轴和y轴的范围
    {
      xAxisDomain: [140, 180],
      yAxisDomain: [30, 70]
    }
  );

  // 让每一个身高数据，分别减去150，然后再除以20
  const inputs = tf.tensor(heights).sub(150).div(20);
  // 让每一个体重数据，分别减去40，然后再除以20
  const labels = tf.tensor(weights).sub(40).div(20);

  // 定义模型结构
  const model = tf.sequential();
  model.add(tf.layers.dense({ units: 1, inputShape: [1] }));
  model.compile({
    loss: tf.losses.meanSquaredError,
    optimizer: tf.train.sgd(0.1)
  });

  await model.fit(inputs, labels, {
    // 训练数据只有3个，所以这里设置为3。如果不设置的情况下，初始值是32
    batchSize: 3,
    epochs: 200,
    callbacks: tfvis.show.fitCallbacks({ name: '训练过程' }, ['loss'])
  });

  const output = model.predict(tf.tensor([180]).sub(150).div(20));
  // 上面是先归一化预测，然后拿到数据结构后，这里再反归一化
  alert(
    `如果身高为 180cm，那么预测体重为 ${output.mul(20).add(40).dataSync()[0]}kg`
  );
};
```

在`index.html`文件引入，**代码如下：**

```html
<script src="script.js"></script>
```

**最终我们来看下效果：**

![](img/ec9b359e975fd9ed4b4917d14644a5f3.png)

至此，我们就完成了整个归一化的过程，并成功预测到了数据。
