
接着，创建`script.js`文件，来加载Iris的数据集。
具体代码如下：

```
import * as tf from &#x27;@tensorflow/tfjs&#x27;;
import * as tfvis from &#x27;@tensorflow/tfjs-vis&#x27;;
import { getData } from &#x27;./data.js&#x27;;

window.onload = async () =&gt; {
  // 加载数据集
  // xTrain代表输入的特征；xTest代表训练集的所有标签
  const [xTrain, yTrain, xTest, yTest] = getIrisData(0.15);
}
```

# 三、定义模型结构：带有softmax的多层神经网络

## 1、操作步骤

+ 初始化一个神经网络模型
+ 为神经网络模型添加两个层
+ 设计这两个层的神经元个数、`inputShape`、激活函数

## 2、训练过程

我们需要用`model`，来为这个神经网络网络添加两个层。具体代码如下👇🏻：

```
import * as tf from &#x27;@tensorflow/tfjs&#x27;;
import * as tfvis from &#x27;@tensorflow/tfjs-vis&#x27;;
import { getData } from &#x27;./data.js&#x27;;

window.onload = async () =&gt; {
  // 加载数据集
  // xTrain代表输入的特征；y代表训练集的所有标签
  const [xTrain, yTrain, xTest, yTest] = getIrisData(0.15);
  
  const model = tf.sequential();
  // 带有softmax的多层神经网络
  model.add(
    tf.layers.dense({
      units: 10,
      inputShape: [xTrain.shape[1]],
      activation: &#x27;sigmoid&#x27;
    })
  );
  model.add(
    tf.layers.dense({
      units: 3,
      activation: &#x27;softmax&#x27;
    })
  );
}
```

# 四、训练模型：交叉熵损失函数与准确度度量

## 1、操作步骤

+ 设置交叉熵函数
+ 添加准确度度量
+ 训练模型并使用`tfvis`可视化训练过程

## 2、训练过程

添加完成神经网络的层之后，接下来要为这个模型，设置交叉熵损失函数与准确度度量，之后进行预测。具体代码如下👇🏻：

```
import * as tf from &#x27;@tensorflow/tfjs&#x27;;
import * as tfvis from &#x27;@tensorflow/tfjs-vis&#x27;;
import { getData } from &#x27;./data.js&#x27;;

window.onload = async () =&gt; {
  // 加载数据集
  ……
  
  // 定义模型结构 
  ……
  
  // 交叉熵损失函数与准确度度量
  model.compile({
    loss: &#x27;categoricalCrossentropy&#x27;, // 设置损失函数
    optimizer: tf.train.adam(0.1), // 设置优化器
    metrics: [&#x27;accuracy&#x27;] // 设置度量
  });

  // 开始训练
  await model.fit(xTrain, yTrain, {
    epochs: 100,
    validationData: [xTest, yTest],
    callbacks: tfvis.show.fitCallbacks(
      { name: &#x27;训练效果&#x27; },
      [&#x27;loss&#x27;, &#x27;val_loss&#x27;, &#x27;acc&#x27;, &#x27;val_acc&#x27;],
      { callbacks: [&#x27;onEpochEnd&#x27;] } // 设置图标只展示 onEpochEnd
    )
  });
}
```

最后，来看下效果：

![](img/794868a35dee48e96b2616aab3510b28.png)

可以看到，蓝线和黄线基本是接近高度吻合，所以说预测效果接近95%+的正确率。

# 五、多分类预测方法

## 1、操作步骤

+ 编写前端界面，并输入待预测数据
+ 使用训练好的模型进行预测
+ 将输出的`Tensor`转为普通数据并显示

## 2、训练过程

首先我们新建个文件夹 `index.html`，具体代码如下：

```
&lt;script src=&quot;script.js&quot;&gt;&lt;/script&gt;
&lt;form action=&quot;&quot; onsubmit=&quot;predict(this); return false;&quot;&gt;
    花萼长度：&lt;input type=&quot;text&quot; name=&quot;a&quot;&gt;&lt;br&gt;
    花萼宽度：&lt;input type=&quot;text&quot; name=&quot;b&quot;&gt;&lt;br&gt;
    花瓣长度：&lt;input type=&quot;text&quot; name=&quot;c&quot;&gt;&lt;br&gt;
    花瓣宽度：&lt;input type=&quot;text&quot; name=&quot;d&quot;&gt;&lt;br&gt;
    &lt;button type=&quot;submit&quot;&gt;预测&lt;/button&gt;
&lt;/form&gt;
```

接着，在`script.js`文件里面，继续编写对数据进行预测的代码。如下所示：

```
import * as tf from &#x27;@tensorflow/tfjs&#x27;;
import * as tfvis from &#x27;@tensorflow/tfjs-vis&#x27;;
import { getData } from &#x27;./data.js&#x27;;

window.onload = async () =&gt; {
  // 加载数据集
  ……
  
  // 定义模型结构 
  ……
  
  // 交叉熵损失函数与准确度度量
  ……
  
  // 多分类预测方法
  window.predict = (form) =&gt; {
    const input = tf.tensor([
      [form.a.value * 1, form.b.value * 1, form.c.value * 1, form.d.value * 1]
    ]);
    const pred = model.predict(input);
    alert(`预测结果：${IRIS_CLASSES[pred.argMax(1).dataSync(0)]}`);
  };
}
```

最后，我们来看下预测效果：

![](img/75cb53fd226f052dd04ca3901d4f03db.png)
