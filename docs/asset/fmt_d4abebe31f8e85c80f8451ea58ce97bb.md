
### Step1：监督微调

[如果服务器](https://zhida.zhihu.com/search?q=%E8%84%9A%E6%9C%AC%E6%96%87%E4%BB%B6&zhida_source=entity&is_preview=1)的显存不够大，可以在脚本文件`./training/step1_supervised_finetuning/training_scripts/single_gpu/run_1.3b.sh`中添加以下代码调整`batch_size`，但也不宜过小。大的`batch size`蓄力时间长，更新比较稳定，且更有效率。

```
--per_device_train_batch_size 8 --per_device_mini_train_batch_size=8 \
```

在下面三种方式中，`single_gpu` 只适合训练较小的模型，而 `single_node` 和 `multi_node` 更适合训练较大的模型。第一次运行时，建议使用 `single_gpu`，因为在这种模式下，输出的错误信息会更详细。如果遇到 GPU 内存不足的问题，可以尝试使用 `single_node` 和 `multi_node` 来训练。如果问题仍然存在，需要手动调整 `batch-size`。

```
python3 train.py --step 1 --deployment-type single_gpu  #单GPU训练
python3 train.py --step 1 --deployment-type single_node #多GPU训练
python3 train.py --step 1 --deployment-type multi_node  #多Node训练
```

此步骤主要进行：

+ 模型下载：代码会自动下载对应的模型，默认情况下模型被存放在

```
~/.cache/huggingface/hub/models--facebook--opt-1.3b
```

+ 数据下载：此步训练使用了以下数据

```
Dahoas/rm-static    # 对话（prompt，response，chosen，rejected） 
Dahoas/full-hh-rlhf # 对话（prompt，response，chosen，rejected）
Dahoas/synthetic-instruct-gptj-pairwise #对话（prompt，chosen，rejected）
yitingxie/rlhf-reward-datasets  # 对话（prompt，chosen，rejected）
openai/webgpt_comparisons       # 带人工打分的数据，comparisons with human feedback，19,578 comparisons）
stanfordnlp/SHP                 # 18个领域的385k 人类标注数据
```

+ 模型训练：模型训练完成之后会被存储在`output/actor-models/1.3b`下面。你可以通过`training.log`文件来查看训练的进度。

+ 训练观察：

```
watch -n 0.5 nvidia-smi #查看gpu使用情况
less output/actor-models/1.3b/training.log #查看训练日志
```

![](img/de6ae16760ca5e8a0bef97b873a250da.png)

![](img/9bb069fdbfd788e0e2b001f533cb4ffd.png)

### 评价与测试

官方为我们准备了一些promt可以用来测试，我们只需要添加 baseline 模型和 finetune 后的模型的路径即可。

```
cd training/step1_supervised_finetuning/
export CUDA_VISIBLE_DEVICES=0
python prompt_eval.py \
    --model_name_or_path_baseline facebook/opt-1.3b \
    --model_name_or_path_finetune ../../output/actor-models/1.3b
```

评价程序会调用 `prompt_eval.py` 来分别输出 baseline 和 finetune 后模型的结果。可以看到 finetune 后的模型表达要远远好于 baseline 模型。

![](img/241518fa2d7929abd6d2acabecfd1250.png)

也可以用以下代码进行测试：

```
cd ../
python chat.py --path output/actor-models/1.3b/
```

我提了一个问题：Why can birds fly?

它回答：I think you’re referring to birds that have evolved to be able to fly. These birds are called “birds of prey”. They are able to fly because they have evolved to have strong legs and strong wings. These birds are able to fly because they have evolved to have strong legs and strong wings.

![](img/dd7b24a472196b7fb01932c1be8a9115.png)

### Step2：Reward模型微调

**任务介绍：** 在第三步（Step3）中，强化学习阶段需要使用奖励模型。奖励模型会对模型生成的答案进行打分，Step3 的强化训练会根据这些分数对模型进行优化，从而使最终模型生成更高分的答案。奖励模型同样基于预训练模型进行训练，在这里我们使用了 350M 的 `opt` 模型。

**启动训练：** 启动 [训练方法 ](https://zhida.zhihu.com/search?q=%E8%AE%AD%E7%BB%83%E6%96%B9%E6%B3%95&zhida_source=entity&is_preview=1) 与前面类似：

```
python3 train.py --step 2 --deployment-type single_gpu  #单GPU训练
python3 train.py --step 2 --deployment-type single_node #多GPU训练
python3 train.py --step 2 --deployment-type multi_node  #多Node训练
```

训练数据：

+ 单 GPU 训练时只使用了 `Dahoas/rm-static` 数据

+ 多 GPU 训练使用了更多的数据：

```
Dahoas/rm-static
Dahoas/full-hh-rlhf
Dahoas/synthetic-instruct-gptj-pairwise
yitingxie/rlhf-reward-datasets
openai/webgpt_comparisons
stanfordnlp/SHP
```

![](img/a4cae464b183ade168beac8c506e1fa6.png)

**评价与测试：**

步骤如下：

+ 打开文件 `run_eval.sh` 设置 `--model_name_or_path` 参数。

```
shell pythonrw eval.py \ --model_name_or_path ../../output/reward-models/350m
```

+ 转移到目录 `step2_reward_model_finetuning` 下

+ 执行：`bash evaluation_scripts/run_eval.sh`

![](img/18d69cd1024fba6fbe4ea24c78957c42.png)

### Step3：RLHF训练

**任务介绍：** RLHF 是基于人类反馈的强化学习的缩写。根据官方介绍，此步训练面临两个主要挑战：

+ 同时使用多个模型的内存消耗问题：此步训练不仅使用被训练的主模型，还使用奖励模型进行评分，因此会占用更多的 GPU 内存。

+ 如何有效地生成答案：在 RLHF 训练过程中，需要生成多个备选答案。由于模型一次推理只能生成一个答案，因此需要进行多次模型推理，这种操作会大幅度增加训练时间。

在此实例中，通过将 `DeepSpeed` 训练和推理功能整合为一个统一的混合引擎（Hybrid Engine）来应对这些挑战。更多详细信息可以参考官方说明。

在此步骤首次运行时，会安装并编译新的工具（`transformer_inference`）。如果编辑过程出现问题，建议升级 `PyTorch` 和 `CUDA` 版本。在我的环境下，使用 `PyTorch` 2.0 和 `CUDA` 11.7 下可以成功编译。

**启动训练：**

```
python3 train.py --step 3 --deployment-type single_gpu  #单GPU训练
python3 train.py --step 3 --deployment-type single_node #多GPU训练
python3 train.py --step 3 --deployment-type multi_node  #多Node训练
```

此步训练后的模型被存储在 `output/step3-models/1.3b/` 下。

![](img/a1e5471df394b1df175759024f25e3c3.png)

**常见问题：**

+ GPU 内存不足时，在 `sh` 脚本中增加如下设置，调整 `batch size`：

```
--per_device_train_batch_size 8 --per_device_mini_train_batch_size=8 \
```

### 4、评价与测试

使用 `chat.py` 命令（需要移动到 `DeepSpeed-Chat` 目录下）进行评价与测试。执行方式如下：

```
python chat.py --path output/step3-models/1.3b/actor
```

![](img/15f170e00a37b2514c96ad242d8eb025.png)

备注：这套工具刚刚发布不久，最近作者和贡献者们还在频繁的更新中。更多的信息，请关注其 github 仓库中的动态。

## 三、代码篇

### 1、概要

`DeepSpeed-Chat` 共有三个 Step。其中 Step1 和 Step2 比较类似，与 Step3 相比，也比较简单。所以，我们通过学习 Step1 来了解这套工具的基本功能。我们主要来学习 `step1_supervised_finetuning` 目录下的 `main.py` 程序，以及与其关联的 `utils` 下代码。我们会在以下几个方面，对代码进行解读：

+ 数据处理部分

+ 模型初始化部分

+ 训练代码部分

+ DeepSpeed 并行处理的理解，以及使用流程

### 2、代码概要

Step1 的训练过程是通过 `bash` 脚本 `run_1.3b.sh` 来调用 `main.py` 来进行训练的，`run_1.3b.sh` 脚本主要包含以下内容

```
deepspeed main.py \
   --data_path Dahoas/rm-static \
   --data_split 2,4,4 \
   --model_name_or_path facebook/opt-1.3b \
   --per_device_train_batch_size 8 \
   --per_device_eval_batch_size 8 \
   --max_seq_len 512 \
   --learning_rate 9.65e-6 \
   --weight_decay 0.1 \
   --num_train_epochs 2 \
   --gradient_accumulation_steps 1 \
   --lr_scheduler_type cosine \
   --num_warmup_steps 0 \
   --seed 1234 \
   --zero_stage $ZERO_STAGE \
   --deepspeed \
   --output_dir $OUTPUT \
   &amp;&gt; $OUTPUT/training.log
```

结合 `main.py` 程序，我们可以将参数分为三大类

#### 与数据相关的参数

```
data_path        : 数据路径，huggingface 数据库， 比如：`Dahoas/rm-static`
data_split       : 数据的拆分方式，比如 2,4,4 是为 step1、2、3 分配的数据比例
max_seq_len      : 最大序列长度（超过长度会被截掉）
data_output_path : 相关数据的存储地址（local storage，不能是 shared storage）
```

#### 与模型相关的参数

```
model_name_or_path : 模型名称或路径，huggingface 模型，比如：`facebook/opt-1.3b`
lora_dim           : 如果大于 0，则使用 `LoRA` 优化
lora_module_name   : 设置 `LoRA` 的范围，比如可以只针对 `decoder.layers`
only_optimize_lora : 是否只优化 `LoRA` 的参数
```

#### 与训练相关的参数

```
per_device_train_batch_size : 训练时的 Batch size（per device：每个 GPU 的 Size）
per_device_eval_batch_size  : 评价时的 Batch size（per device）
learning_rate               : 学习率
weight_decay                : 权重衰减，防止模型过拟合的技术
num_train_epochs            : 训练 epoch 数
gradient_accumulation_steps : 累积多少个 mini-batch 的梯度后再进行一次参数更新
lr_scheduler_type           : learning rate 的调整策略，比如 linear、cosine
```

#### deepspeed

```
zero_stage  : 这个对应 `DeepSpeed` 工具中的 zero 方式，分别是 0、1、2、3
offload     : ZeRO-Offload 通过利用主机 CPU 上的计算和内存资源来执行优化器，从而减少此类模型的 GPU 计算和内存需求
local_rank  : 分布式训练时的一个变量，用于标识当前 GPU 设备的本地排名（本机排名，与 global-rank 不同）
gradient_checkpointing : 降低深度学习模型训练过程中内存消耗的技术
```

#### 其他

```
seed        : 随机排序是的 seed
output_dir  : 模型的存储目录
```

### 3、分布式训练的基本概念

#### local_rank

`local_rank` 在分布式训练时使用的一个变量，用于标识当前 GPU 设备的本地排名（local rank）。

当 `args.local_rank` 等于 `-1` 时，表示代码不在分布式设置下运行，仅使用单个 GPU 进行训练。如果 `args.local_rank` 不等于 `-1`，则表示代码在分布式设置下运行，当前 GPU 设备被分配了一个唯一的本地排名。代码会将设备设置为指定的 GPU（`torch.device("cuda", args.local_rank)`），并使用 `deepspeed.init_distributed()` 函数调用初始化分布式后端。

注意：在 `PyTorch` 中也有分布式初始化方法 `torch.distributed.init_process_group()` 函数。但是当使用 `DeepSpeed` 库时，不要替换为 `deepspeed.init_distributed()`。

#### global_rank

在分布式训练中，每个进程都有一个唯一的全局排名，用于标识该进程在分布式环境中的位置。全局排名的范围是从 0 到 `world_size-1`，其中 `world_size` 是整个分布式环境中进程的总数。本程序中通过 `torch.distributed.get_rank()` 来读取 `global_rank`，本函数在初始化分布式后端之后才能调用。

`torch.distributed.barrier()` 是一个同步函数，用于在分布式环境中同步各个进程的状态。在调用该函数时，进程会阻塞等待，直到所有进程都调用了该函数之后，才会解除阻塞并继续执行后面的代码。

在分布式训练中，`torch.distributed.barrier()` 通常用于同步各个进程的梯度更新。在每个进程完成一轮前向传播和反向传播后，它们需要同步各自的梯度，并且等待其他进程完成同样的操作，才能进行下一轮更新。这时就可以使用 `torch.distributed.barrier()` 函数实现同步。

另外一个用法，在模型参数并行训练时，数据的读取只需要在 `local_rank` 为 0 的 GPU 上进行，其他进程使用 `torch.distributed.barrier()` 来阻塞来等待数据读取完成。

### 4、数据相关
