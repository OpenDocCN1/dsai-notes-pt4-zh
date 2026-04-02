# 常见问题

> 来源：[https://ai.feishu.cn/docx/H9hIdYRQroTUq2xzzNPcDXwEn5f](https://ai.feishu.cn/docx/H9hIdYRQroTUq2xzzNPcDXwEn5f)

> 以下问题和回答不完整，更多问题和回答可以在github进行关注和查看

### 在提出问题之前

请先查看：

https://github.com/THUDM/ChatGLM3/issues

和

https://github.com/THUDM/ChatGLM3/discussions

寻找相关的问题集

## Q 直接使用load_model_on_gpus进行多卡并行可行么？

1.  可行的，ChatGLM3模型已经修复了前一代模型的bug，可以直接使用load_model_on_gpus来执行

## Q 为什么输入 不少于1000字这种提示词的时候，模型会重复输出。

1.  这是由于官方模型在这方面的训练数据较少，属于模型原生级别的缺陷，在本代原生模型的能力下，这个问题无法避免。

1.  一个好的解决方法是避免这类提示词，或者增加模型的惩罚系数。

## Q 模型的并发量是多少

1.  官方并没有对模型的并发量做过科学的统计计算。这会因为硬件配置的不同影响模型的并发量，主要影响因素是显卡的推理能力。

## Q 模型有没有Int4或者TensorRT加速版本

1.  目前没有，官方正在规划中

## Q 模型无法再huggingface下载，还有没有其他的下载途径

1.  使用魔搭社区，SwanHub等国内镜像下载，但所有的模型更新都会在Huggingface率先公布。