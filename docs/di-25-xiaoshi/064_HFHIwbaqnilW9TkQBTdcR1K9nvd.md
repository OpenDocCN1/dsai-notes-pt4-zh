
# 006 Python 与 JavaScript 模型互转

> 来源：`https://my.feishu.cn/docx/BVhfd9ddpo4I7Kxc8gIc5Jn7n2f`

# 一、任务简介

## 1、什么是 Python 模型？

简单来说，`Python` 模型就是通过 `Python` 版本的 `Tensorflow` 或者 `Keras` 所生成的模型，我们姑且称它为 `Python` 模型。

`Keras` 也是一个深度学习的框架，而 `Tensorflow` 也是支持 `Keras` 所拥有的 `API` 和语法的。

`Python` 模型具体包含哪些格式？比如说 `Tensorflow` 的 `Saved Model` 格式，这是 `Tensorflow` 上非常通用的格式，以及 `Keras` 的 `HDF5 Model` 等等，这些都是比较通用的格式。

那可以从哪里来获取这些 `Python` 模型？我们知道有很多好心人已经训练好了一些模型，放到了网上，让我们可以直接下载这些预训练好的模型，直接拿来用。所以说第一个获取方式就是从开源网站上下载这些 `Python` 模型。

除此之外，如果你的团队里有算法部门，那么算法同事也会训练一些 `Python` 模型，提供给你，让你可以转为可以在 `Javascript` 上运行的模型，我们就姑且称这些模型为 `Javascript` 模型。

`Javascript` 模型也不是一个很官方的叫法，只是为了跟 `Python` 模型形成对比。

## 2、什么是 JavaScript 模型

### （1）可以在 `Tensorflow.js` 中运行的模型

什么是 `Javascript` 模型？简单来说就是可以在 `Tensorflow.js` 中运行的模型。

既然可以在 `Tensorflow.js` 中运行，那么自然而然就可以在浏览器和这个 `Node.js` 中运行了，这极大地方便了前端对模型的部署。

之前 `Python` 模型只能在 `Python` 环境里运行，现在不仅能在浏览器里面直接跑，而且还能在 `Node.js` 里运行了。

这样一来我们就可以把它部署到更多的前端应用里了，不需要跟服务端进行网络请求，就可以直接进行预测了。

### （2）包括 `tfjs_layers_model`、`tfjs_graph_model` 等

那么 `Javascript` 模型包含哪些格式？

主要有两种格式，第一个是 `tfjs_layers_model`，第二个是 `tfjs_graph_model`。

### （3）通过开源网站、`TFJS` 生成、`Python` 模型转化等方式获取

最后我们说一下如何获取 `Javascript` 模型？

第一种获取方法就是从开源网站上预训练模型，不仅包含 `Python` 格式的，也有 `Javascript` 格式的，我们同样可以从一些开源网站上直接下载，直接拿来用。

第二种方式是通过 `Tensorflow.js` 来生成，我们可以在浏览器里面训练好，然后保存成文件或者像二进制这样的形式，把它保存成已经训练好的模型文件。

第三种就是通过 `Python` 模型转化而来，在真实的实际工作中，用第三种方式的情况会比较多。也就是平常我们会用 `Python` 版的 `Tensorflow` 来训练模型，然后转化为 `Tensorflow.js` 来运行。

## 3、为何要进行互转？

接下来，我们聊一下为何要进行互转？

### （1）`Python` to `JavaScript`：`JavaScript` 模型可以在浏览器中运行

第一种方式是从 `Python` 模型到 `Javascript` 模型，为何要这样做？`Python` 模型不是更通用吗？为何还要转化为 `Javascript` 格式？这是因为 `Javascript` 的模型可以在浏览器中直接运行，而 `Python` 的模型是不可以的。

我们训练好的模型想要在实际的工业场景下落地，`Javascript` 运行环境是绝对不能少的。像我们平常所接触到的前端网页游戏、网页应用、小程序，甚至在一些嵌入式的设备里面，都是由 `Javascript` 环境运行起来的。

因此，如果想让你所写的机器学习模型能够被普罗大众所使用的话，那么兼容 `Javascript` 环境是必不可少的环节之一。

当然，`Javascript` 环境运行模型也会引发一些问题。比如说有的模型特别大，运行在浏览器里面其实是非常卡顿的，因为它非常的吃内存。不光如此，如果模型特别大的话，光下载模型文件都需要很久很久。我们可以想象一下，一个 `1G` 的模型，你在下载的时候就要花费很长的一段时间了。

除了一些大模型无法部署的问题之外，还有模型安全的问题。比如说你辛辛苦苦训练了一个模型，结果别人直接把你的模型给下载走了，放到他的软件里去跑，那好像就很吃亏了。所以也不是所有的 `Python` 模型都会转为 `Javascript` 模型的。

但在某些场景下，这个操作却是必须的。比如说淘宝的美妆应用，为了获取一些更快的实时的一种速度，我们就把模型下载到了浏览器里面，这样可以达到即使在没有服务器交互的情况下，加载速度也是非常快速的。

还有一些开源的小游戏，我们也需要部署到浏览器里面，以获取到浏览器的各种用户交互能力，比如说摄像头、麦克风等等。

综上所述，要不要把 `Python` 模型转换为 `Javascript` 模型，还是要看具体场景综合来分析，但是这个技术还是非常值得我们去学习的。

### （2）`JavaScript` to `Python`：少见，为了在更多平台运行

聊完了这个 `Python` 模型转为 `Javascript` 模型之后，我们再来看一下这个 `Javascript` 模型转为 `Python` 模型？

这种转换其实是非常少见的，但是存在即合理。

猜测这种方法可能是为了让 `Javascript` 训练的模型在更多的平台上运行。随着 `Tensorflow.js` 支持 `Node.js` 环境之后，使用 `Javascript` 也可以进行大规模的模型训练了，而且速度丝毫不比 `Python` 差。因为它们背后运行的都是 `C++`，`Python` 和 `JS` 就可以理解为是桥，而且就语言来说，`Javascript` 其实比 `Python` 还要快。

所以根据这种情况，假以时日，相信随着社区成长起来之后，说不定 `Javascript` 模型也会变得非常多，非常通用。到时候 `Python` 社区可能会来 `Javascript` 社区来获取营养，那个时候可能就会有更多的这种 `Javascript` 模型转为 `Python` 模型的需求了。

### （3）`JavaScript` to `JavaScript`：分片/量化/加速

那么最后一种是 `Javascript` 模型到 `Javascript` 模型的操作，主要包含：分片、量化和加速。这几个东西分别是👇🏻：

分片就是把一个模型文件分成多个模型文件，比如说把一个 `4M` 的模型文件分成几十 `k` 的模型文件，就是把它给分开了，切开了。

这么做的好处是可以利用浏览器的并发请求能力来更快的把模型加载到浏览器中。比如说我们之前把一个 `4M` 的模型文件加载到浏览器里面可能需要很长时间，可能需要几秒钟。但如果我们把它分片切成多个并发加载的话就是同时加载的话，那么速度就会得到成倍的提升。

第二个功能是量化。简单来说这个量化就是牺牲一部分精度来把模型压缩的更小。那么为什么要把模型压缩的更小呢？其实还是为了能更快地去部署，就是让模型的下载速度更快一点。

最后一个是加速。加速其实就是让模型预测的时间更短，让模型可以更快地进行预测，对预测速度进行加速。

那么这个加速是怎么实现的？我们会把 `layers` 模型转为 `graph` 模型，这样做，`graph` 模型的速度就会相对会快一点，这是加速中常用的一个方法。

## 4、模型互转操作步骤

安装 `Tensorflow.js Converter` —— 将会使用 `Conda` 这个工具，来创建一个独立的虚拟的 `Python` 环境，专门给 `converter` 使用。

`Python` 与 `JavaScript` 模型互转 —— 将会把 `Keras` 中的 `H5` 模型，转化为 `JavaScript` 中 `tfjs_layers_model` 格式的模型，以及互转回来的过程。

`JavaScript` 模型的互转：分片、量化、加速 —— 在每次转换完之后，会再使用相应的程序来运行一下，看一下最终模型的效果好不好。

# 二、安装 `Tensorflow.js Converter`

## 1、操作步骤

安装 `Conda` —— 用来辅助我们创建虚拟且独立的 `Python` 环境

使用 `Conda` 安装 `Python` 虚拟环境

使用 `Python` 虚拟环境安装 `Tensorflow.js Converter`

## 2、安装过程

**Converter 相关：**

`tensorflow.js` 代码库：`https://github.com/tensorflow/tfjs/`

搜索 `Tensorflow.js Converter`，点击进入：`https://github.com/tensorflow/tfjs/tree/master/tfjs-converter`

**关于 `conda` 的安装：**

访问网站：`https://mirror.tuna.tsinghua.edu.cn/help/anaconda/` `anaconda` | 镜像站使用帮助 | 清华大学开源软件镜像站 | `Tsinghua Open Source Mirror`

拉到最下方，也就是最新版本，然后选择和自己电脑匹配的配置进行安装即可。

**相关命令行解析：**

```
// 安装一个 tfjs 的虚拟环境，python 版本为 3.8
conda create -n tfjs python=3.8

// 查看当前环境信息
conda info --envs

// 删除某个环境
conda remove -n tfjs --all

// 使用 tfjs 环境
conda activate tfjs

// 退出 tfjs 环境
conda deactivate

// 查看 python 版本
python --version
```

**虚拟环境安装成功后相关操作：**

```
// 使用 pip 安装 tensorflowjs
pip install tensorflowjs 

// 验证 tensoflow.js 安装后是否能正常使用
tensorflowjs_converter -h
```

# 三、`Python` 与 `JavaScript` 模型互转

## 1、操作步骤

`Python` 模型转 `JS` 模型：

`https://github.com/tensorflow/tfjs/blob/master/tfjs-converter/README.md#python-to-javascript`

`JS` 模型转 `Python` 模型：

`https://github.com/tensorflow/tfjs/blob/master/tfjs-converter/README.md#javascript-to-python`

## 2、举例说明

`py` 模型转成 `js` 模型：

```
// 输入是 keras 格式，输出是 tfjs_layers_model 格式。
// 第一个输入是 py 模型的路径，第二个路径是转成 js 模型后，输出的路径
tensorflowjs_converter --input_format=keras --output_format=tfjs_layers_model data/mobilenet/keras.h5 data/mobilenet/web_model2
```

`js` 模型转成 `py` 模型：

```
tensorflowjs_converter --input_format=tfjs_layers_model --output_format=keras data/mobilenet/web_model2/model.json data/mobilenet/keras2.h5
```

# 四、`JavaScript` 的模型互转：分片、量化、加速

## 1、操作步骤

**阐述说明：**

+ 分片 —— 把一个模型文件切成很多片，这样每一片的体积都会变小。可以理解为利用浏览器的并发加载能力，就可以更快地把模型加载到浏览器中，可以提升模型的下载速度。

+ 量化 —— 量化操作就是通过牺牲一部分精度来压缩模型的大小，这样也可以起到更快加载模型的效果。

+ 加速 —— 通过转为 `tfjs_graph_model` 来加速模型。

**操作方式：**

`https://github.com/tensorflow/tfjs/blob/master/tfjs-converter/README.md#javascript-to-javascript`

## 2、举例说明

### （1）分片

```
// --weight_shard_size_bytes=100000 表示指定分片后每一个的大小，100kb
tensorflowjs_converter 
--input_format=tfjs_layers_model 
--output_format=tfjs_layers_model 
--weight_shard_size_bytes=100000
data/mobilenet/web_model/model.json
data/mobilenet/sharded_model
```

### （2）量化

```
// --quantization_bytes=2 表示量化参数设置为 2
tensorflowjs_converter 
--input_format=tfjs_layers_model 
--output_format=tfjs_layers_model 
--quantization_bytes=2
data/mobilenet/web_model/model.json
data/mobilenet/quantization_model
```

### （3）加速

```
// 输出格式改为 tfjs_graph_model
tensorflowjs_converter 
--input_format=tfjs_layers_model 
--output_format=tfjs_graph_model 
data/mobilenet/web_model/model.json
data/mobilenet/graph_model
```
