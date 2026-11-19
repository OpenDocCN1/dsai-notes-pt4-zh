
# 人类对齐

> 来源：[https://v11enp9ok1h.feishu.cn/docx/WgC6dgH2low3p5x0JyycfpPynXe](https://v11enp9ok1h.feishu.cn/docx/WgC6dgH2low3p5x0JyycfpPynXe)

本文档提供了各种人类偏好对齐算法的训练脚本。若您希望深入[了解](https://github.com/modelscope/modelscope-classroom/blob/main/LLM-tutorial/M.%E4%BA%BA%E7%B1%BB%E5%81%8F%E5%A5%BD%E5%AF%B9%E9%BD%90%E8%AE%AD%E7%BB%83.md)更详尽的算法信息及其选择方法，请参考文档

## 数据集

PPO与GRPO算法所需的数据仅为模型输入，也就是`system prompt`（可选）加上`query`。其中GRPO中的奖励函数可能需要额外的数据列，比如计算准确率需要`solution`列作为参考答案。

RM和DPO类算法如ORPO，CPO，SimPO，则需要 `(x,yw,yl)` 格式的数据，其中 `xxx` 表示模型输入，`yw`、`yl` 分别表示符合人类偏好的偏好回答和不符合人类偏好的拒绝回答，比如

![](img/1432e1c8eccc31fd6691fa8a695f628d.png)

> 而KTO算法的数据比较特殊，只需要 `(x,y,label)` 格式的数据，其中 `xxx` 表示模型输入，`yyy` 表示模型输出，`label`表示回答是否符合人类偏好 比如

![](img/07bb6e2d08bb096e7531043c9bebb8af.png)

使用自定义数据集对文本模型或者多模态大模型进行RLHF训练可以参考。

## GRPO

[论文arvix](https://arxiv.org/abs/2402.03300)

训练脚本参考[这里](https://modelscope.cn/docs/llm-training-and-inference/user-guide/GRPO).

## DPO

[论文arvix](https://arxiv.org/abs/2305.18290)

超参

+ `beta`：KL正则系数，值越大表示对偏离参考模型的惩罚越大。默认为0.1

> 建议在开始DPO训练之前，使用偏好数据集中的偏好回答部分进行SFT训练，以确保数据符合DPO算法的分布要求。 我们也在`DPO loss`中混合了`sft loss`来稳定训练，你可以通过设置超参`rpo_alpha`来调整`sft loss`的系数，默认为1.

训练脚本参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/rlhf/dpo.sh).

## RM#

[论文arvix](https://arxiv.org/abs/2203.02155)

RLHF中的Reward Modeling阶段

使用`SFT`训练后的`base model`或者`instruct model`作为基底模型, 增加`value head`, 使用偏好数据集训练为`reward model`

增加的`value head`权重会保存在`value_head.safetensors` 或 `value_head.bin`文件中

训练脚本参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/rlhf/rm.sh).

## PPO#

[论文arvix](https://arxiv.org/abs/2203.02155)

RLHF中的PPO(proximal policy optimization)阶段, 涉及到四个模型

+ `model`: 训练模型, `SFT`训练后的`base model`或者`instruct model`

+ `ref_model`: 参考模型, 默认为 `model`

+ `reward_model`: 奖励模型, 由`RM`阶段训练得到

+ `value_model`: 价值模型, 由`reward_model`初始化, 在训练中同步更新

超参

+ `local_rollout_forward_batch_size`: 每次数据采样的批量大小, 默认为64

+ `whiten_rewards`: 对奖励进行归一化处理, 默认为`False`

+ `kl_coef`: KL散度项的系数, 默认为0.05

+ `cliprange`: PPO策略损失函数中的clip范围, 默认为0.2

+ `vf_coef`: 价值损失函数系数, 默认为0.1

+ `cliprange_value`: PPO价值损失函数中的clip范围, 默认为0.2

+ `gamma`: 累计奖励的折扣因子, 默认为1.0

+ `lam`: [GAE](https://arxiv.org/abs/1506.02438)中的lambda系数, 默认为0.95

+ `num_sample_generations`: 训练过程中生成的调试样本数量, 默认为10

注意: 训练`base model`时, 需要先`SFT`后再进行`RLHF`, 指定`chat template`, `sft_type`建议使用`full`

训练中的指标解释参考[文档](https://huggingface.co/docs/trl/ppov2_trainer#explanation-of-the-logged-metrics)

## KTO#

[论文arvix](https://arxiv.org/abs/2402.01306)

超参

+ `beta`： KL正则系数，值越大表示对偏离参考模型的惩罚越大。默认为0.1

+ `desirable_weight` ：损失函数中的`lambda_D`项，偏好回答样本的损失权重, 默认为1.0

+ `undesirable_weight` ：损失函数中的`lambda_U`项，拒绝回答样本的损失权重，默认为1.0

用 `nD` 和 `nU` 分别表示数据集中偏好回答和拒绝回答的样本数量，对于超参 `lambda_D` 和 `lambda_U` ，作者推荐设置 `lambda_D * nD / (lambda_U * nU) ∈ [1, 4/3]`

训练脚本 使用 `(x,y,label)` 格式数据训练

训练脚本参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/rlhf/kto.sh).

## CPO#

[论文arvix](https://arxiv.org/abs/2401.08417) 超参

+ `beta`：隐含奖励前的系数，默认为0.1

+ `cpo_alpha`: `nll loss`系数, 默认为1.0

训练脚本参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/rlhf/cpo.sh).

## ORPO#

[论文arvix](https://arxiv.org/abs/2403.07691)

超参

+ `lambda`: Odds Ratio loss系数

注意：ORPO使用参数`--beta`传入超参`lambda`

训练脚本参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/rlhf/orpo.sh).

## SimPO#

[论文arvix](https://arxiv.org/abs/2405.14734) 超参

+ `beta`：隐含奖励前的系数，默认为2.0

+ `simpo_gamma`：`reward margin`项，默认为1.0

+ `cpo_alpha`: 混合`CPO nll loss`提高训练稳定性, 默认为1.0, 设置0.0使用原始`SimPO`算法

训练脚本参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/train/rlhf/simpo.sh).
