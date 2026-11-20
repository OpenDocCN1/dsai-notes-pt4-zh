
# QA｜其他技术彩蛋

> 来源：[https://datawhaler.feishu.cn/docx/GGtOdQV8FodRn4xrvUDch6Qwnfr](https://datawhaler.feishu.cn/docx/GGtOdQV8FodRn4xrvUDch6Qwnfr)

## Task3：微调入门

1. 感觉LoRa微调作用没有修改prompt作用大？

1. 微调的损失率没降下去，如何调整参数？

> ——大模型微调不看损失率，大概都是2.9-4之间的，太低就要考虑会不会过拟合了

1.

![](img/38fa7a7a6ceea63d7d5a7223432da535.png)

> ——对显卡的CC和linux的系统版本低的Cuda的编译好像会出现问题

> 装个新的anaconda重新配一个环境降一下torch和cuda到适合该显卡内核版本的版本试一下

> https://github.com/vllm-project/vllm/issues/629

> https://github.com/pytorch/pytorch/issues/31285

1. CoT(思维链)如何使用？

> ——可以放在instruction部分或微调阶段

> CoT可以有效减少逻辑错误和前后不搭

1. 本地运行的大模型使用`apply_chat_template`模版，得到的输出总是包含`input`的内容应该怎么处理呀，除了后期再做清理，有没有什么方法可以使模型生成的内容不包含`input`的内容呀？之前使用`inputs = tokenizer(combined_message, return_tensors='pt', padding=True, truncation=True).to(model.device)`和`inputs = tokenizer.apply_chat_template(messages, add_generation_prompt=True, padding=True,truncation=True,return_tensors="pt").to(model.device)`得到的`output`都会包含`input`的内容

1. deepseek的额度不小心喽完了，大家有用其他平台便宜一点的token吗？

> ——试试GLM，通义等等、充值deepseek。

1. CoT 是不是不太适合用在20b以下的模型呀？re3它是用于longer story的对800字的短文写作会有帮助吗

> ——主要参考下他是如何设计prompts和修正故事逻辑的。

1. 大模型随着问题增加会不断增加显存占用吗？感觉稍微问几个问题显存就分配完了然后显卡就不工作了

> ——可能是没有写代码清除显存缓存。

1. DeepLn怎么开通端口访问？

> ——进行本地运行，或者挂在服务器跑，要实名认证。

1. 使用vllm遇到如下问题：

![](img/d4647d14418dc6de723a26d8ed90680c.png)

> ——该问题是显存爆了

1. 在微调时，构建提示词模板的时候，为角色设计提示词能生效吗？

![](img/4c34f89babbb23c8c0c0036edc8c60ec.png)

## Task2：baseline 精读

1. **评测失败可能是什么原因？**

> 可以检查一下`submit.json`是否满足以下要求

![](img/41ceb2a1f0e9ab02ab3a7d38a840df26.png)

1. 一次性处理的文本长度有什么限制？

1. vllm 和直接开 4 个文件同时推理的区别是什么？

1. vllm 能在哪些操作系统上用？

1. vllm 和 lora 有什么关系？

1. vllm 不能提前启动吗？它的原理是什么？

1. fastapi 起服务能扛住高并发吗？

1. lora 还有别的微调方法吗？

1. M1 pro 能玩多大的模型？

1. 算力问题怎么解决？

1. `TrainingArguments`中的`per_device_train_batch_size`和`gradient_accumulation_steps`是否要同时下调？

> ——不需要

> 对显存没自信可以把`batch_size`调成`1`

> 梯度累计4可以留着，调成1可能会胡乱梯度下降

## Task1：跑通 baseline

1. token 不够怎么办？

> ——充值或找别的赠送额度的API

1. 按照字数分割，每段故事不连贯怎么办？

> ——影响不大，因为只要在中间故事中学习到逻辑梗概和上下文能力（baseline 500字），前后的截断带来的问题对模型微调的结果影响不大。

1. 有什么获得别的数据集的方法？

> ——Github上搜索WritingPrompts dataset或者搜索悟道数据集或者https://github.com/ yangkevin2/emnlp22-re3-story-generation

1. 跑完baseline只有三行`output`是正常的吗

> ——是正常的，原始代码中设置的是3，作为演示，这样输出的速度能快一些；评测会随机抽取答案，可能会抽到空值

1. 耗时非常长是什么原因，怎么解决

> ——耗时长可能跟显卡或者API速度有关系。耗时太长的话，建议选用别的更高配置的GPU，或换用别的 大模型API进行打标，baseline 推荐耗时 20～30 分钟

1. 速通代码里面有`seed`吗？或者是在哪里设置呢

> ——大模型一般不进行`seed`设置，预测时一般采用multifold来做模型和预测的多样性设置。因为模型输出的本质还是马尔可夫链，后一个输出`token`和前一个输出的`token`是有依赖关系的，大模型在输出时循环输出下一个`token`，直到遇见`stop token`。温度和`topK`可以控制大模型输出的多样性

1. 可以本地跑大模型吗

> ——本地没有3090及以上的GPU的话跑不起来。大模型不建议本地跑，能跑起来也非常慢。

1. 生成50个结果就可以提交了吗？还是必须生成1000条结果？

> ——task1是经过简化的，不是跑完整的结果，是虚假的1000条

1. 有什么方法可以把微调后的模型快速传到其他服务器上面？

> ——`ssh`压缩下载，然后再往上推；或者利用`scp`命令，`scp`在linux之间传输很快

1. 在deepln上正在跑的任务，关闭电脑主机后，云上的任务需要重新跑吗？关闭后发现之前的任务没有在运行

> ——在电脑关机时让deepln在后台继续运行的方法：可以使用`nohup`来跑，将`ipynb`换成`py`文件来跑

1. baseline的代码中数据集拆分后直接限制800字，不是把样本搞小了吗？800字是不是应该在生成结果的地方限制？

> ——这里是切小说用800字当作喂料，一整本喂进去可能会爆显存，按800字切分数据集可以让微调后的模型生成字数接近800字，因为生成目标是基于一段话生成800字的小文。这种切分主要是为了保证字数，没有太考虑到上下文，后续要想提升分数可以针对这方面做一些改进工作

1. baseline跑成完绩为0是正常的吗

> ——是正常的，评分结果是从你生成的结果中随机抽取进行评分，baseline实际上只生成了3条数据，其他是补的空字符串，如果正好抽中的都是空数据集就会导致评分为0

1. 在云上跑baseline时环境中缺少所需库是什么原因

> ——可能是jupyter核选错了，更换核即可

1. 跑1000条模型大概需要多久

> ——要考虑模型大小和显卡型号。P100需要24小时左右，7B模型；A100一分钟八条，4090一分钟两条（数据来自飞书群友）

1. baseline为什么选这个模型？

> ——相对比较轻量，便于下载

1. 怎么量化结果？

> ——基于现有的结果做对比

1. 限速问题的解决方法

> ——多线程可以一定程度提速，飞书文档中提供了相应的文件

1. deepln 存储数据怎么收费？为什么DeepLn保存到`/data`目录的数据，重新启动服务器挂载数据以后找不到了？

> 前30天免费，可以手动延期

> 重新启动服务器，并不是挂载到新的`/data`目录，存储的地址在`/data/syncdata`，需要手动把目录中的`coding`目录所有内容copy到当前的`coding`目录下。下次关机时，保存最新的`/data`目录即可。

![](img/a648b658093e303f0ecf028c81c1ac90.png)

![](img/7c5a39f63bdaeb38d4413671a3b1db2b.png)

```
mv {datapath}/data/coding /data/
```

1. `!pip install -u huggingface hub modelscope "transformers>=4.37.0"` ......模型一直下载失败

> ——点击scrollable element

> 在这个代码块中单独加一个`!pip list`看看有没有`modelscope`库

> 如果没有就执行一下 `!pip install modelscope`

> 如果有就重启一下notebook

![](img/28ad563dfc0a324b20f3e799a993dfcd.png)

## Deepln 服务器相关

### 本地连接 deepln GPU 服务器

1. 复制 `ssh` 链接和默认密码

![](img/2b9c4f0a079b1b6c9d2045de09c17ff8.png)

点击复制可获得 `ssh` 连接命令：

+   `ssh -p 45688 root@r7oxbesmvwxxxxxxsnow.deepln.com`

1. Mac 使用 vs code 进行 `ssh` 连接

+   Vs code 安装 Romote - SSH 插件

+   新建远程

![](img/5da534bead2d57b1b080e280192181e2.png)

![](img/34333b3d1705a34333b3d1705a... 

![](img/8d6de178e96e26a1b23b9711a99ae0ba.png)

![](img/651ea62f7068564b08c70f4e25587f4a.png)

![](img/0b0e6593f79d36c8e50c1b27beed6f73.png)

### Git clone 不了？

打开🪜全局试试

### 无法执行 notebook？

> 选择 `torch` 版本即可

![](img/caac11b79c4ef06ffc86e8c4fcad9e64.png)
