
# 指令微调工程建议（代码）

> 来源：https://agijuejin.feishu.cn/docx/Sz7TdONFAoEg4Yx0aWRclApVnTc
>
> 作者：Walker
>
> 原文：https://zhuanlan.zhihu.com/p/690047840
>
> Github：https://github.com/WalkerMitty/Fast-Llama2

[本文旨在用指令微调探索LLM领域适应能力，是上篇](https://zhuanlan.zhihu.com/p/689800667)文章的扩展。（ Walker：LLM is not all you need（大模型领域知识微调实践） ）

## Llama2

官方仓库在 [这里](https://link.zhihu.com/?target=https%3A//github.com/meta-llama/llama-recipes)，但是我感觉这个仓库太臃肿了，我只是想微调 `Llama2`，快速验证效果。而 `huggingface` 的 `SFTrainer` 又封装得太深了，不好改动。因此我基于 `Llama2` 仓库，删掉了不必要的代码，并添加了一些对我重要的三个功能，如下：

+ 加载训练好的 `lora` 继续训练（不能每次都重来吧？）

+ [推理输出](https://zhida.zhihu.com/search?q=%E7%BD%AE%E4%BF%A1%E5%BA%A6&zhida_source=entity&is_preview=1)文本的同时输出权重（打印 `logit` 分析 置信度）

+ 修改了 `scheduler` 逻辑，只有当 `loss` 增加时才减小 `lr`（个人感觉这种方式最简单也最通用，因为原来的 `scheduler` 在我的数据集收敛实在太慢）

## Flan-T5

`Flan-T5` 是 encoder-decoder 架构里比较出色的一个模型，很多论文都是以 `Flan-T5` 作为 backbone，比如 [`InstructUIE`](https://link.zhihu.com/?target=https%3A//github.com/BeyonderXX/InstructUIE)，我也是在这个基础上做过指令微调的实验，他们的仓库已经很详细了，不用我赘述。不过由于国内网络问题，建议在 `run_uie.py` 最前面加一行

```
os.environ["HF_DATASETS_OFFLINE"] = "1"
```

否则你还会以为程序卡住了。。。

By the way，这个repo不支持 `Lora` 微调，不过我建议用 `zero1` 或者 `zero2` 设置降低显存，加速训练，效果也差不多。

## 国内下载模型权重

国内下载模型权重有魔塔和 `hf-mirror` 可供选择，魔塔模型太少了，而且很多都是非 `hf` (`pytorch`) 版本，不够方便，这里只介绍 `hf-mirror` 的快速用法（下载速度实测10M/ s）

```
pip install huggingface_hub
export HF_ENDPOINT=https://hf-mirror.com
# 下面是一个例子
huggingface-cli download --resume-download --local-dir-use-symlinks False google/flan-t5-xl pytorch_model-00002-of-00002.bin
```

但是这样会下载到默认路径，修改 `huggingface` 的 cache 路径脚本：

```
vi ~/.bashrc
export HF_HOME="your path"
source ~/.bashrc
```
