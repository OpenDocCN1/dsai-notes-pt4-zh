
# Task4：源大模型微调实战-知识点文档

> 来源：[https://datawhaler.feishu.cn/docx/OZaGdovwkoW4FWxswN8csQBxn8e](https://datawhaler.feishu.cn/docx/OZaGdovwkoW4FWxswN8csQBxn8e)

# 引导思考

+ RAG之所以能解决大模型幻觉的问题，很大原因是充分调用了大模型的能力
+ 如果大模型在当前任务上能力不佳，如果提升其能力？
+ 另外，怎么使大模型学习其本身不具备的能力呢？

# 正文内容

## 1 引言

### 1.1 大模型微调技术简介

模型微调也被称为指令微调（Instruction Tuning）或者有监督微调（Supervised Fine-tuning, SFT），该方法利用成对的任务输入与预期输出数据，训练模型学会以问答的形式解答问题，从而解锁其任务解决潜能。经过指令微调后，大语言模型能够展现出较强的指令遵循能力，可以通过零样本学习的方式解决多种下游任务。

然而，值得注意的是，指令微调并非无中生有地传授新知，而是更多地扮演着催化剂的角色，激活模型内在的潜在能力，而非单纯地灌输信息。

相较于预训练所需的海量数据，指令微调所需数据量显著减少，从几十万到上百万条不等的数据，均可有效激发模型的通用任务解决能力，甚至有研究表明，少量高质量的指令数据（数千至数万条）亦能实现令人满意的微调效果。这不仅降低了对计算资源的依赖，也提升了微调的灵活性与效率。

### 1.2 轻量化微调技术简介

然而，由于大模型的参数量巨大， 进行全量参数微调需要消耗非常多的算力。为了解决这一问题，研究者提出了参数高效微调（Parameter-efficient Fine-tuning），也称为轻量化微调 （Lightweight Fine-tuning），这些方法通过训练极少的模型参数，同时保证微调后的模型表现可以与全量微调相媲美。

常用的轻量化微调技术有`LoRA`、`Adapter` 和 `Prompt Tuning`。

### 1.3 LoRA技术简介

`LoRA` 是通过低秩矩阵分解，在原始矩阵的基础上增加一个旁路矩阵，然后只更新旁路矩阵的参数。

![](img/53de3c31b77fe7cdd40baf01a13c04b0.png)

> 参考资料

+ [LoRA paper](https://arxiv.org/pdf/2106.09685.pdf)
+ 大模型轻量级微调（LoRA）：训练速度、显存占用分析
+ 【深度学习】混合精度训练与显存分析
+ [全网最全-混合精度训练原理](https://zhuanlan.zhihu.com/p/441591808)

## 2 源2.0-2B 微调实战

本节我们将进行 `源2.0-2B` 微调实战，我们使用一个简历命名实体识别的数据集进行微调，进而开发一个AI简历助手，进而可以批量地自动识别并提取简历中的关键信息（如姓名、教育背景、工作经历等），提升简历处理的效率。

具体来说，输入一个摘录自某简历的句子，模型需要识别出所有的命名实体。实体的类别包括：姓名（NAME）、国籍（CONT）、种族（RACE）、职位（TITLE）、教育背景（EDU）、专业（PRO）、组织名（ORG）、地名（LOC）。原始的数据来自于`BAAI/COIG-PC-Lite`。

### 2.0 PAI实例创建

在实战之前，需要开通阿里云`PAI-DSW`试用，并在魔搭社区创建`PAI`实例，创建流程与速通手册一致~

如果忘记如何创建的，可以参考下面的内容复习一下~

#### Step0：开通阿里云`PAI-DSW`试用

<https://datawhaler.feishu.cn/sync/DkaqdtRoIsuiyAbDBoPcacKUned>

#### Step1：在魔搭社区创建`PAI`实例！（点击即可跳转）

<https://datawhaler.feishu.cn/sync/AZmfdDSBGsaIxObZ2w4cStnKnid>

### 2.1 环境准备

进入实例，点击终端。

![](img/9680ebf981ff56e58644eae6513a757b.png)

运行下面代码，下载文件，并将`Task 4：源大模型微调实战`中内容拷贝到当前目录。

```
git lfs install
git clone https://www.modelscope.cn/datasets/Datawhale/AICamp_yuan_baseline.git
cp AICamp_yuan_baseline/Task\ 4：源大模型微调实战/* .
```

![](img/c5e0f719e9c348169fc89b4978009de9.png)

双击打开`Task 4：源大模型微调实战.ipynb`，运行所有单元格。

![](img/8348e74fd9d81cdb0cf456c2f3bf6ae1.png)

通过下面的命令，我们可以看到`ModelScope`已经提供了所需要的大部分依赖，如 `torch`，`transformers` 等。

```
# 查看已安装依赖
pip list
```

![](img/0ab84ea212a69d30cf2d3bdbed3721b7.png)

但是为了进行Demo搭建，还需要运行下面的单元格，在环境中安装`streamlit`。

```
# 安装 streamlit
pip install streamlit==1.24.0
```

安装成功后，我们的环境就准备好了。

### 2.2 模型下载

`Yuan2-2B-Mars`支持通过多个平台进行下载，因为我们的机器就在魔搭，所以这里我们直接选择通过魔搭进行下载。

模型在魔搭平台的地址为 `IEITYuan/Yuan2-2B-Mars-hf`。

单元格 `2.1 模型下载` 会执行模型下载。

这里使用的是 `modelscope` 中的 `snapshot_download` 函数，第一个参数为模型名称 `IEITYuan/Yuan2-2B-Mars-hf`，第二个参数 `cache_dir` 为模型保存路径，这里`.`表示当前路径。

模型大小约为4.1G，由于是从魔搭直接进行下载，速度会非常快。

下载完成后，会在当前目录增加一个名为 `IEITYuan` 的文件夹，其中 `Yuan2-2B-Mars-hf` 里面保存着我们下载好的源大模型。

![](img/cecf8c7f8f2100b214cca306f2ac60cf.png)

### 2.3 数据处理

运行 `2.3 数据处理` 下面的单元格。

我们使用 `pandas` 进行数据读取，然后转成 `Dataset` 格式：

![](img/08a2420b7a1fdd640e7978936eb7446f.png)

然后我们需要加载 `tokenizer`：

![](img/be29727e251b378166ecbf34227a34a3.png)

为了完成模型训练，需要完成数据处理，这里我们定义了一个数据处理函数 `process_func`：

```
def process_func(example):
    MAX_LENGTH = 384    # Llama分词器会将一个中文字切分为多个token，因此需要放开一些最大长度，保证数据的完整性

    instruction = tokenizer(f"{example['input']}<sep>")
    response = tokenizer(f"{example['output']}<eod>")
    input_ids = instruction["input_ids"] + response["input_ids"]
    attention_mask = [1] * len(input_ids) 
    labels = [-100] * len(instruction["input_ids"]) + response["input_ids"] # instruction 不计算loss

    if len(input_ids) > MAX_LENGTH:  # 做一个截断
        input_ids = input_ids[:MAX_LENGTH]
        attention_mask = attention_mask[:MAX_LENGTH]
        labels = labels[:MAX_LENGTH]

    return {
        "input_ids": input_ids,
        "attention_mask": attention_mask,
        "labels": labels
    }
```

具体来说，需要使用`tokenizer`将文本转成id，同时将 `input` 和 `output` 拼接，组成 `input_ids` 和 `attention_mask` 。

这里我们可以看到，源大模型需要在 `input` 后面添加一个特殊的token `<sep>`， 在 `output` 后面添加一个特殊的token `<eod>`。

同时，为了防止数据超长，还有做一个截断处理。

然后使用 `map` 函数对整个数据集进行预处理：

![](img/7e02a26bef224afe86ef42bc6d4546e1.png)

处理完成后，我们使用`tokenizer`的`decode`函数，将id转回文本，进行最后的检查：

![](img/8d123194acd81fcc328d05d903edec70.png)

### 2.4 模型训练

首先我们需要加载源大模型参数，然后打印模型结构：

![](img/d9b0292445e8284af077f86fa7365af1.png)

可以看到，源大模型中包含24层 `YuanDecoderLayer`，每层中包含 `self_attn`、`mlp` 和 `layernorm`。

另外为了进行模型使用训练，需要先执行 `model.enable_input_require_grads()` 。

![](img/5cadb03a9b75b0defcc79d0de90fda1d.png)

最后，我们打印下模型的数据类型，可以看到是 `torch.bfloat16`。

![](img/ead3bfba3111a364d6dbd299cee692f5.png)

在本节中，我们使用`Lora`进行轻量化微调，首先需要配置 `LoraConfig`：

![](img/ee20764fdd96af6792b6ad02c226df0a.png)

然后构建一个 `PeftModel`:

![](img/4b42c34a98efb2b98069c9154f1f5d10.png)

通过 `model.print_trainable_parameters()`，可以看到需要训练的参数在所有参数中的占比：

![](img/2b6d7902cb740dc2c86f40ec4dfad6c3.png)

然后，我们设置训练参数 `TrainingArguments`:

![](img/b79ab3420ea29e4bd56f667c2c9049e4.png)

同时初始化一个 `Trainer`:

![](img/5ff94181887e11847fbf26925c6a74ec.png)

最后运行 `trainer.train()` 执行模型训练。

![](img/2931515ba9647036b55915a3ddd2a745.png)

在训练过程中，会打印模型训练的`loss`，我们可以通过`loss`的降低情况，检查模型是否收敛：

![](img/4973899f60b9bfb346d618305d41bcef.png)

模型训练完成后，会打印训练相关的信息：

![](img/ebe1937ae3db16d09cbeebadffe304d8.png)

同时，我们会看到左侧 `output` 文件夹下出现了3个文件夹，每个文件夹下保存着每个epoch训完的模型。

![](img/6988beb668f753ff2c95d72fdaf48413.png)

这里，以`epoch3`为例，可以看到其中保存了训练的`config`、`state`、`ckpt`等信息。

![](img/e1f751f05e2037c25fc27fcddec763b1.png)

### 2.5 效果验证

完成模型训练后，我们通过定义一个生成函数 `generate()`，来进行效果验证：

![](img/4cacf0b1a4ab73bf6c07c4e0801cacc8.png)

同时定义好输入的`prompt template`，这个要和训练保持一致。

![](img/073fa99d158cb08611e9029215a1f1ca.png)

最后，我们输入样例进行测试：

![](img/b6c8930cb6781c5e4ce6dd1137184f86.png)

可以看到，通过模型微调，模型已经具备了相应的能力

此时，我们查看机器状态，如下图所示：

![](img/d88336372ab349eb20fa83e882b6aa6d.png)

可以看到约占用 17G 显存。

显存的占用多少和需要训练的模型参数、`batch size`等相关，大家可以尝试不同的训练配置。

训练完成后，我们尝试使用训练好的模型，搭建Demo。

首先，点击重启内核，清空显存。

（因为Demo也需要占用显存，不先清空会因为显存不足报错。）

![](img/04fbc87800a4220b8ee59d1936e85fce.png)

然后，我们将在终端输入下面的命令，启动`streamlit`服务：

```
streamlit run Task\ 4\ 案例：AI简历助手.py --server.address 127.0.0.1 --server.port 6006
```

![](img/21666e616580482cb2f6723f9fb7cd0d.png)

然后点击链接，跳转到浏览器，进入Demo：

输入文本：张三，汉族，金融学硕士。

![](img/bcec5213b9e46fce37d2c44aaa9efa01.png)

可以看到，Demo完成了信息抽取，并进行了结构化展示。

这样我们完成了一个AI简历助手的构建。

### 2.6 关闭PAI实例！！！！（点击即可跳转）

<https://datawhaler.feishu.cn/sync/RMHtdxBnqshtNsbJHcqcMma2nLg>

### 还可以尝试替换其他Yuan大模型！

<https://datawhaler.feishu.cn/sync/UG0ydiJi9sSFkub60HmcuQTJn9f>

## 3 课后作业

针对一个下游任务，构建数据集，进行`Yuan-2B`模型微调实战，完成微调后模型的效果测试和调优。

### 3.1 任务挑选和训练集构建

+ 原则：任务是否必须微调，单纯通过`prompt`或者`rag`能否解决？
+ 任务来源：
+ 已有任务：传统的NLP任务，分类、`NER`等等 -> 直接复用开源训练集
+ 新任务：构建应用过程中依赖的能力 -> 有无开源训练集可复用，如果没有，如何收集数据
+ 训练集预处理：收集好的数据需要哪些预处理步骤？数据清洗？去重？

### 3.2 微调实战

+ 效果测试：测试集怎么来？轻量化微调效果能否达到需求？
+ 效果调优：没有达到的原因有哪些？数据不够？参数设置不对？需要全量微调？模型太小？

### 3.3 其他

参考下面的链接，探索更多数据集：

+ `huggingface` 数据集：https://hf-mirror.com/datasets
+ 魔搭数据集：https://modelscope.cn/datasets
+ `openDataLab` 数据集：https://opendatalab.org.cn/
+ 和鲸社区 数据集：https://www.heywhale.com/home/dataset

确定好任务后，开始模型微调，提交测试效果吧！

## 4 作品提交方式

如果完成了作品开发，可以提交初版作品啦，加油

<https://datawhaler.feishu.cn/share/base/form/shrcnbLAWydDXkK3lFwbIQBecCg?iframeFrom=docx&ccm_open=iframe>

# 来个小总结

# ❀ 恭喜看完全部知识教程！

> 我们的学习也已接近过半！接下来，就都是你们团队开发的时间！
> 后面我们也会继续展开技术交流会，有任何疑难杂症，欢迎一起探讨~
> 比赛不结束，我们不散场！比赛结束了，我们依旧不散场！依旧可以交流idea、交流demo！

# 把时间交给你们，期待你们的应用惊艳全场！！！

>

> 填写打卡，加入作品交流群持续迭代作品！

### 🎉合影留念墙

感谢很棒的你一路的坚持，让我们在这里给自己点个赞！

期待下期夏令营还能与你再次相遇~

一起合个影 :)

> [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/Gf5DwJNXeiLOjDkMlPhcnwjTnMf)任何想法都可以在学习群里交流或在   进行填写哟~

>

> 本教程主要由   写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> 注：本文采用 `署名—非商业性使用—相同方式共享 4.0 协议国际版` 许可协议进行许可。
