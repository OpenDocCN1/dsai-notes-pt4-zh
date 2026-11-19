
# 002 使用预测训练模型进行图片分类

> 来源：[https://my.feishu.cn/docx/Krrld8ehcoOfZGxb4xYcsjcUnjf](https://my.feishu.cn/docx/Krrld8ehcoOfZGxb4xYcsjcUnjf)

## 一、任务简介

### 1、什么是预训练模型

已经事先训练好的模型，无需训练即可预测。也就是说我们无需训练，直接就可以拿来预测了。那么这些模型是谁训练好的？可能是一些大公司，比如说像谷歌，也有可能是一些研究机构、大学教授等等，即已经有人训练好了，并上传到了网上，我们只需要下载就行了。

在 `Tensorflow.js` 中可以调用 Web 格式的模型文件。当然还有其他类型的格式，比如，Python版本的 `Tensorflow.js`，它就有一个叫 `SavedModel` 的格式，这也是一个非常通用的格式。

### 2、操作步骤

加载 `MobileNet` 模型 —— `MobileNet` 是一种卷积神经网络，且 `MobileNet` 这种卷积神经网络的特点就是比较轻量，它的模型文件非常的小，响应速度非常的快。但是它的缺点就是相比于更复杂的一些模型、或者更复杂的那些卷积神经网络的模型来比较的话，准确度就没有他们那么高，不过基本上也够用了。正如它的名字一样，这种模型通常被用于移动端。

进行预测 —— 之前我们的任务都是要先准备数据，然后构建复杂的神经网络，然后再进行训练，训练完之后才能预测。而使用了 `MobileNet` 模型之后，我们就可以直接进行预测了。

## 二、加载 MobileNet 模型

### 1、操作步骤

+ 下载 `MobileNet` 模型文件
+ 在本地开启静态文件服务器
+ 使用 `Tensorflow.js` 的 `loadLayersModel` 方法加载模型

### 2、Coding Time

#### （1）加载数据

首先我们，要先准备 `mobilenet` 的数据集，目录如下：

![](img/c39c68efb0f02447eb232cd0343ae7fd.png)

具体文件如下：

![](img/76c9e19b42182bd8b5e06c0d111468a1.png)

[`data.zip`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H8AmbkFBlouVdWxM9JacqKzCnac)

然后呢，在本地启动一个静态服务，来加载这部分的数据。具体代码如下：

```
http-server data --cors
```

#### （2）加载模型

下面，我们在根目录下，额外建立一个文件夹，命名为 `mobilenet`。之后，建立一个新的文件 `script.js`，来编写相关代码。具体代码如下：

```
import * as tf from '@tensorflow/tfjs';
import { IMAGENET_CLASSES } from './imagenet_classes';
import { file2img } from './utils';

const MOBILENET_MODEL_PATH =
  'http://127.0.0.1:8080/mobilenet/web_model/model.json';

window.onload = async () => {
  // 加载MobileNet模型
  const model = await tf.loadLayersModel(MOBILENET_MODEL_PATH);
};
```

到这里，我们就完成了对 `mobilenet` 模型的加载。

## 三、进行预测

### 1、操作步骤

+ 编写前端界面输入待预测数据
+ 使用训练好的模型进行预测
+ 将输出的 `Tensor` 转为普通数据并显示

### 2、训练过程

我们在 `mobilenet` 文件夹下，创建一个新的文件 `index.html`，这个文件主要用来编写前端界面，以及引入脚本。具体代码如下：

```
<script src="script.js"></script>
<input type="file" onchange="predict(this.files[0])">
```

然后呢，新建一个 `utils.js` 文件，来封装获取图片的内容。具体代码如下：

```
/**
 *
 * @param {*} f 文件转换为图片
 * @returns
 */
export function file2img(f) {
  return new Promise((resolve) => {
    // 实例化一个文件实例
    const reader = new FileReader();
    // 读取数据文件的内容
    reader.readAsDataURL(f);
    // 开始读取之后，会触发一个异步操作。且异步操作灰枣onload之后结束
    reader.onload = (e) => {
      // 新建一个图片标签
      const img = document.createElement('img');
      // 设置图片的src
      img.src = e.target.result;
      // 设置图片的宽高，并且图片是个彩色图片
      img.width = 224;
      img.height = 224;
      // 封装成Promise，在Promise里面去返回图片的内容
      img.onload = () => resolve(img);
    };
  });
}
```

最后来到之前我们加载过模型的 `script.js` 文件，具体代码如下：

```
import * as tf from '@tensorflow/tfjs';
// 这里面预置了1000中已经分类好的图片的索引
import { IMAGENET_CLASSES } from './imagenet_classes';
import { file2img } from './utils';

const MOBILENET_MODEL_PATH =
  'http://127.0.0.1:8080/mobilenet/web_model/model.json';

window.onload = async () => {
  // 加载MobileNet模型
  const model = await tf.loadLayersModel(MOBILENET_MODEL_PATH);
  /**
   *
   * @param {*} file 接收一个文件
   */
  window.predict = async (file) => {
    // 将图片文件转化为图片的html element
    const img = await file2img(file);
    // 把选择的图片给挂载到浏览器上
    document.body.appendChild(img);
    // tidy是便于清除中间的这个Tensor，然后来优化我们的这个Webgl的内存，防止内存泄漏。

    const pred = tf.tidy(() => {
      // 将来要输入的Tensor
      const input = tf.browser
        .fromPixels(img)
        .toFloat() // 整数转化为浮点数
        .sub(255 / 2) // -127.5 ~ 127.5 （为了达到归一化）
        .div(255 / 2) // 归一化
        .reshape([1, 224, 224, 3]); // 预测最终图片展示的格式
      return model.predict(input);
    });

    // 获取最大值的坐标，argMax(1)表示获取第二维
    const index = pred.argMax(1).dataSync()[0];
    setTimeout(() => {
      // alert会阻塞JS引擎的执行，所以要放在定时器里面
      alert(`预测结果：${IMAGENET_CLASSES[index]}`);
    }, 0);
  };
};
```

至此，我们就完成了预测，最后来看下预测效果。如下图所示：

![](img/82a64686d5b38a8e44658a9629194f79.png)
