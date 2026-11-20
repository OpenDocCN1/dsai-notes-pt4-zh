
+ 在Jeopardy问题生成任务上，输入`Hemingway`的检索结果：

![](img/32826115d044f78e8ea37b5638298b24.png)

实验结果表明，优于非检索方法：

![](img/5097a3d33f044cbe1983b5ea3bde57e5.png)

这里引用`GPT-3` few-shot的结果进行比较：`NaturalQuestions` (29.9%), `WebQuestions` (41.5%), `TriviaQA` (71.2%)

## `RETRO` (Borgeaud et al., 2021)

+ 基于32个`token`的块进行检索
+ 存储库：2 trillion `tokens`
+ 70亿参数（比`GPT-3`少25倍）
+ 使用冻结的`BERT`进行检索（不更新）
+ 在`MassiveText`上训练（与训练`Gopher`的数据集相同）

### 实验结果

+ 在语言建模方面表现出色
+ `NaturalQuestions`准确率：45.5%（`SOTA`为54.7%）

![](img/3d9a699c06e72a3b13c920a8d4453b9b.png)

### 讨论

+ 基于检索的模型高度适合知识密集型的问答任务
+ 除了可扩展性之外，基于检索的模型还提供了可解释性和更新存储库的能力
+ 目前尚不清楚这些模型是否具有与稠密`Transformer`相同的通用能力

## 总体总结

+ 为了扩大模型规模，需要改进稠密`Transformer`
+ 混合专家和基于检索的方法相结合更有效
+ 如何设计更好的、可扩展的体系结构仍然是一个悬而未决的问题
