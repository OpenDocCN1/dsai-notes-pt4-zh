
# chinese_gpt2

> 来源：[https://v11enp9ok1h.feishu.cn/docx/KdkbdG4xEoMAV3xXopAckOkYnIg](https://v11enp9ok1h.feishu.cn/docx/KdkbdG4xEoMAV3xXopAckOkYnIg)

## 介绍

1. 本文，将介绍如何使用中文语料，训练一个`gpt2`

1. 可以使用你自己的数据训练，用来：写新闻、写古诗、写对联等

1. 我这里也训练了一个中文`gpt2`模型，使用了612万个样本，每个样本有512个tokens，总共相当于大约31亿个tokens

### ⚠️安装包

需要准备好环境，也就是安装需要的包

```
pip install -r requirements.txt
```

像是`pytorch`这种基础的包肯定也是要安装的，就不提了。

## 数据

### 数据来源

1. 获得数据:数据链接，关注👸号【统计学人】，然后回复【`gpt2`】即可获得。

1. 获得我训练好的模型(使用了15GB的数据(31亿个tokens)，在一张3090上，训练了60多小时)

### 数据格式

1. 数据其实就是一系列文件夹📁，然后每一个文件夹里面有大量的文件，每一个文件都是`.csv`格式的文件。其中有一列数据是`content`

1. 每一行的`content`就代表一句话,截图如下

![](img/58cb2c84c122397b77e39ced260e47e6.png)

1. 虽然数据有15GB那么大，但是处理起来一点也不复杂，使用`datasets`包，可以很轻松的处理大数据，而我只需要传递所有的文件路径即可，这个使用`glob`包就能完成。

## 代码

### ⚙️训练代码`train_chinese_gpt2.ipynb`

#### ⚠️注意

1. 现在训练一个`gpt2`代码，其实很简单的。抛开处理数据问题，技术上就三点:`tokenizer`、`gpt2_model`、`Trainer`

1. `tokenizer`使用的是`bert-base-chinese`，然后再添加一下`bos_token`、`eos_token`、`pad_token`。

1. `gpt2_model`使用的是`gpt2`，这里的`gpt2`我是从0开始训练的。而不是使用别人的预训练的`gpt2`模型。

1. `Trainer`训练器使用的就是`transformers`的`Trainer`模块。（支撑多卡并行，`tensorboard`等，都写好的，直接调用就行了，非常好用）

### 😊模型

模型已经上传到`huggingface`上了 [https://huggingface.co/yuanzhoulvpi/gpt2_chinese](https://huggingface.co/yuanzhoulvpi/gpt2_chinese)

![](img/24fb97cd88d04d52ef4aca9814a1d2c1.png)

### 📤推理代码`infer.ipynb`

#### ⚠️注意

这个是`chinese-gpt2`的推理代码

1. 将代码中的`model_name_or_path = "checkpoint-36000"`里面的`checkpoint-36000`,修改为模型所在的路径。

1. 然后运行下面一个代码块，即可输出文本生成结果

1. 可以参考这个代码，制作一个`api`，或者打包成一个函数或者类。

### 🤖交互机器人界面`chatbot.py`

#### ⚠️注意

1. 修改代码里面的第4行，这一行值为模型所在的位置，修改为我分享的模型文件路径。

```
model_name_or_path = "checkpoint-36000"
```

1. 运行

```
python chatbot.py
```

1. 点击链接，即可在浏览器中打开机器人对话界面

![](img/a91f01d64bd94a3dc68e8b416208e7df.png)

## 更多

1. 这个完整的项目下来，其实我都是全靠`huggingface`文档、教程度过来的.

1. 我做的东西，也就是把`Tokenizer`改成中文的了，然后也整理了数据，别的大部分东西，都不是我做的了.

1. 原文链接为[https://huggingface.co/course/zh-CN/chapter7/6?fw=pt](https://huggingface.co/course/zh-CN/chapter7/6?fw=pt).

## 一起学习

其实，我更喜欢做应用，但是也要理解相关的背后原理，目前还在研究相关的`gpt2`原理还有相关的推理细节，这是我整理的链接，希望可以共同进步
