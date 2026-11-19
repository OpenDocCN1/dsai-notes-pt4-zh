
# 命令行参数

> 来源：[https://v11enp9ok1h.feishu.cn/docx/PJmHdZImcoVJU6xl2d3cV6w6nDh](https://v11enp9ok1h.feishu.cn/docx/PJmHdZImcoVJU6xl2d3cV6w6nDh)

命令行参数的介绍会分为基本参数，原子参数、集成参数和特定模型参数。命令行最终使用的参数列表为集成参数。集成参数继承自基本参数和一些原子参数。特定模型参数是针对于具体模型的参数，可以通过`--model_kwargs`或者环境变量进行设置。Megatron-SWIFT命令行参数介绍可以在Megatron-SWIFT训练文档中找到。

提示：

+ 命令行传入list使用空格隔开即可。例如：`--dataset <dataset_path1> <dataset_path2>`。
+ 命令行传入dict使用json。例如：`--model_kwargs '{"fps_max_frames": 12}'`
+ 带🔥的参数为重要参数，刚熟悉ms-swift的用户可以先关注这些命令行参数

## 基本参数

+ 🔥`tuner_backend`: 可选为`'peft'`，`'unsloth'`。默认为`'peft'`
+ 🔥`train_type`: 可选为: `'lora'`、`'full'`、`'longlora'`、`'adalora'`、`'llamapro'`、`'adapter'`、`'vera'`、`'boft'`、`'fourierft'`、`'reft'`。默认为`'lora'`
+ 🔥`adapters`: 用于指定adapter的id/path的list，默认为`[]`
+ `external_plugins`: 外部plugin py文件列表，这些文件会被注册进plugin模块中，例子请参见这里
+ `seed`: 默认为42
+ `model_kwargs`: 特定模型可传入的额外参数，该参数列表会在训练推理时打印日志进行提示。例如`--model_kwargs '{"fps_max_frames": 12}'`
+ `load_args`: 当指定`--resume_from_checkpoint`、`--model`、`--adapters`会读取保存文件中的`args.json`，将默认为None的基本参数（除去数据参数和生成参数）进行赋值（可通过手动传入进行覆盖）。推理和导出时默认为True，训练时默认为False
+ `load_data_args`: 如果将该参数设置为True，则会额外读取`args.json`中的数据参数。默认为False
+ `use_hf`: 控制模型下载、数据集下载、模型推送使用ModelScope还是HuggingFace。默认为False，使用ModelScope
+ `hub_token`: hub token. ModelScope的hub token可以查看这里
+ `custom_register_path`: 自定义模型、对话模板和数据集注册的`.py`文件路径的list。默认为`[]`

### 模型参数

+ 🔥`model`: 模型id或模型本地路径。如果是自定义模型请配合`model_type`和`template`使用，具体可以参考自定义模型
+ `model_type`: 模型类型。相同的模型架构、template、模型加载过程被定义为一个`model_type`。默认为None，根据`--model`的后缀和`config.json`中的`architectures`属性进行自动选择
+ `model_revision`: 模型版本，默认为None
+ `task_type`: 默认为`'causal_lm'`。可选为`'causal_lm'`、`'seq_cls'`、`'embedding'`。`seq_cls`的例子可以查看这里，`embedding`的例子查看这里
+ 🔥`torch_dtype`: 模型权重的数据类型，支持`float16`,`bfloat16`,`float32`。默认为None，从`config.json`文件中读取
+ `attn_impl`: attention类型，可选项为`flash_attn`, `sdpa`, `eager`。默认使用`sdpa`，若不支持则使用`eager`。
+ 注意：这三种实现并不一定都支持，这取决于对应模型的支持情况。
+ `num_labels`: 分类模型（即`--task_type seq_cls`）需要指定该参数。代表标签数量，默认为None
+ `problem_type`: 分类模型（即`--task_type seq_cls`）需要指定该参数。可选为`'regression'`, `'single_label_classification'`, `'multi_label_classification'`。默认为None，根据`num_labels`和数据集类型进行自动设置
+ `rope_scaling`: rope类型，支持`linear`和`dynamic`，请配合`max_length`共同使用。默认为None
+ `device_map`: 模型使用的`device_map`配置，例如：`'auto'`、`'cpu'`、json字符串、json文件路径。默认为None，根据设备和分布式训练情况自动设置
+ `max_memory`: `device_map`设置为`'auto'`或者`'sequential'`时，会根据`max_memory`进行模型权重的`device`分配，例如：`--max_memory '{0: "20GB", 1: "20GB"}'`。默认为None
+ `local_repo_path`: 部分模型在加载时依赖于github repo。为了避免git clone时遇到网络问题，可以直接使用本地repo。该参数需要传入本地repo的路径, 默认为None

### 数据参数

+ 🔥`dataset`: 数据集id或路径的list。默认为`[]`。每个数据集的传入格式为：数据集id or 数据集路径:子数据集#采样数量，其中子数据集和取样数据可选。本地数据集支持`jsonl`、`csv`、`json`、文件夹等。开源数据集可以通过`git clone`到本地并将文件夹传入而离线使用。自定义数据集格式可以参考自定义数据集。你可以传入`--dataset <dataset1> <dataset2>`来使用多个数据集。
+ 子数据集: 该参数只有当`dataset`为ID或者文件夹时生效。若注册时指定了`subsets`，且只有一个子数据集，则默认选择注册时指定的子数据集，否则默认为`'default'`。你可以使用`/`来选择多个子数据集，例如：`<dataset_id>:subset1/subset2`。你也可以使用`'all'`来选择所有的子数据集，例如：`<dataset_id>:all`。
+ 采样数量: 默认使用完整的数据集。若采样数少于数据样本总数，则进行随机选择（不重复采样）。若采样数高于数据样本总数，则只额外随机采样采样数%数据样本总数的样本，数据样本重复采样采样数//数据样本总数次。注意：流式数据集只进行顺序采样。若设置`--dataset_shuffle false`，则非流式数据集也进行顺序采样。
+ 🔥`val_dataset`: 验证集id或路径的list。默认为`[]`
+ 🔥`split_dataset_ratio`: 不指定`val_dataset`时如何拆分训练集和验证集，默认为0.01。若不需要切分验证集，设置为0即可
+ `data_seed`: 数据集随机种子，默认为42
+ 🔥`dataset_num_proc`: 数据集预处理的进程数，默认为1
+ `dataset_shuffle`: 是否对`dataset`进行随机操作。默认为True。
+ 注意：CPT/SFT的随机包括两个部分：数据集的随机，由`dataset_shuffle`控制；`train_dataloader`中的随机，由`train_dataloader_shuffle`控制。
+ `val_dataset_shuffle`: 是否对`val_dataset`进行随机操作。默认为False。
+ 🔥`streaming`: 流式读取并处理数据集，默认False。通常在处理大型数据集时，设置为True
+ `interleave_prob`: 默认值为 None。在组合多个数据集时，默认使用`concatenate_datasets`函数；如果设置了该参数，则会使用`interleave_datasets`函数。该参数通常用于流式数据集的组合，并会作为参数传入`interleave_datasets`函数中
+ `stopping_strategy`: 可选为`"first_exhausted"`, `"all_exhausted"`，默认为`"first_exhausted"`。传入`interleave_datasets`函数中
+ `shuffle_buffer_size`: 该参数用于指定流式数据集的随机buffer大小，默认为1000
+ `enable_cache`: 数据集预处理使用cache，默认False
+ `download_mode`: 数据集下载模式，包含`reuse_dataset_if_exists`和`force_redownload`，默认为`reuse_dataset_if_exists`
+ `columns`: 用于对数据集进行列映射，使数据集满足`AutoPreprocessor`可以处理的样式，具体查看这里。你可以传入json字符串，例如：`'{"text1": "query", "text2": "response"}'`，默认为None。
+ `strict`: 如果为True，则数据集只要某行有问题直接抛错，否则会丢弃出错数据样本。默认False
+ `remove_unused_columns`: 是否删除数据集中不被使用的列，默认为True
+ 🔥`model_name`: 仅用于自我认知任务，只对`swift/self-cognition`数据集生效，替换掉数据集中的`{{NAME}}`通配符。传入模型中文名和英文名，以空格分隔，例如：`--model_name 小黄 'Xiao Huang'`。默认为None
+ 🔥`model_author`: 仅用于自我认知任务，只对`swift/self-cognition`数据集生效，替换掉数据集中的`{{AUTHOR}}`通配符。传入模型作者的中文名和英文名，以空格分隔，例如：`--model_author '魔搭' 'ModelScope'`。默认为None
+ `custom_dataset_info`: 自定义数据集注册的json文件路径，参考自定义数据集。默认为`[]`

### 模板参数

+ 🔥`template`: 对话模板类型。默认为None，自动选择对应`model`的`template`类型
+ 🔥`system`: 自定义`system`字段，可以传入字符串或者txt文件路径。默认为None，使用`template`的默认`system`
+ 注意：数据集中的`system`优先级最高，然后是`--system`，最后是定义在`template`中的`default_system`。
+ 🔥`max_length`: 单样本的tokens最大长度。默认为None，设置为模型支持的tokens最大长度(`max_model_len`)
+ 注意：PPO、GRPO和推理情况下，`max_length`代表`max_prompt_length`
+ `truncation_strategy`: 如果单样本的tokens超过`max_length`如何处理，支持`delete`, `left`和`right`，代表删除、左侧裁剪和右侧裁剪，默认为`'delete'`
+ 🔥`max_pixels`: 多模态模型输入图片的最大像素数（H*W），将超过该限制的图像进行缩放。默认为None，不限制最大像素数
+ `tools_prompt`: 智能体训练时的工具列表转为system的格式，请参考智能体训练。可选为`'react_en'`、`'react_zh'`、`'glm4'`、`'toolbench'`、`'qwen'`，默认为`'react_en'`
+ `norm_bbox`: 控制如何对`bbox`进行缩放。可选项为`'norm1000'`, `'none'`。其中`'norm1000'`代表对`bbox`进行千分位坐标缩放，`'none'`代表不缩放。默认为None，根据模型进行自动选择
+ `response_prefix`: `response`的前缀字符，例如QwQ-32B将`response_prefix`设置为`'<think>\n'`。默认为None，根据模型自动设置
+ `padding_side`: 当训练`batch_size>=2`时的`padding_side`，可选值为`'left'`、`'right'`，默认为`'right'`。（推理时的`batch_size>=2`时，只进行左padding）
+ `loss_scale`: 训练tokens的loss权重设置。默认为`'default'`，代表所有`response`（含history）以1计算交叉熵损失。可选值为`'default'`、`'last_round'`、`'all'`，以及`agent`需要的`loss_scale`: `'react'`、`'agentflan'`、`'alpha_umi'`和`'qwen'`。其中`'last_round'`代表只计算最后一轮`response`的损失，`'all'`代表计算所有tokens的损失。agent部分可以查看插件化和智能体训练
+ `use_chat_template`: 使用chat模板或generation模板，默认为True。swift pt会自动设置为generation模板
+ `template_backend`: 选择`template`后端，可选为`'swift'`、`'jinja'`，默认为`'swift'`。如果使用`jinja`，则使用`transformers`的`apply_chat_template`。
+ 注意：`jinja`的`template`后端只支持推理，不支持训练。

### 生成参数

参考`generation_config`文档

+ 🔥`max_new_tokens`: 推理最大生成新tokens的数量。默认为None，无限制
+ `temperature`: 温度参数。默认为None，读取`generation_config.json`。
+ 注意：`do_sample`参数在本版本中移除了，请将`temperature`配置为0来达到相同效果
+ `top_k`: `top_k`参数，默认为None。读取`generation_config.json`
+ `top_p`: `top_p`参数，默认为None。读取`generation_config.json`
+ `repetition_penalty`: 重复惩罚项。默认为None，读取`generation_config.json`
+ `num_beams`: beam search的并行保留数量，默认为1
+ 🔥`stream`: 流式输出，默认为False
+ `stop_words`: 除了`eos_token`外额外的停止词，默认为`[]`。
+ 注意：`eos_token`会在输出`respsone`中被删除，额外停止词会在输出中保留
+ `logprobs`: 是否输出`logprobs`，默认为False
+ `top_logprobs`: 输出`top_logprobs`的数量，默认为None

### 量化参数

以下为加载模型时量化的参数，具体含义可以查看量化文档。这里不包含swift export中涉及的`gptq`、`awq`量化参数

+ 🔥`quant_method`: 加载模型时采用的量化方法，可选项为`bnb`、`hqq`、`eetq`
+ 🔥`quant_bits`: 量化bits数，默认为None
+ `hqq_axis`: `hqq`量化axis，默认为None
+ `bnb_4bit_compute_dtype`: `bnb`量化计算类型，可选为`float16`、`bfloat16`、`float32`。默认为None，设置为`torch_dtype`
+ `bnb_4bit_quant_type`: `bnb`量化类型，支持`fp4`和`nf4`，默认为`nf4`
+ `bnb_4bit_use_double_quant`: 是否使用双重量化，默认为True
+ `bnb_4bit_quant_storage`: `bnb`量化存储类型，默认为None

## 原子参数

### Seq2SeqTrainer参数



# 训练参数列表

[该参数列表继](https://huggingface.co/docs/transformers/main/en/main_classes/trainer#transformers.Seq2SeqTrainingArguments)承自`transformers.Seq2SeqTrainingArguments`，`ms-swift`对其默认值进行了覆盖。未列出的请参考HF官方文档

+ 🔥`output_dir`: 默认为`None`，设置为`output/<model_name>`

+ 🔥`gradient_checkpointing`: 是否使用`gradient_checkpointing`，默认为`True`

+ 🔥`deepspeed`: 默认为`None`。可以设置为`'zero0'`、`'zero1'`、`'zero2'`、`'zero3'`、`'zero2_offload'`、`'zero3_offload'`来使用`ms-swift`内置的`deepspeed`配置文件

+ `zero_hpz_partition_size`: 默认为`None`，这个参数是`ZeRO++`的特性，即node内模型分片，node间数据分片，如果遇到`grad_norm NaN`，请尝试使用`--torch_dtype float16`

+ 🔥`per_device_train_batch_size`: 默认值`1`

+ 🔥`per_device_eval_batch_size`: 默认值`1`

+ 🔥`gradient_accumulation_steps`: 梯度累加，默认为`None`，即设置`gradient_accumulation_steps`使得`total_batch_size>=16`。`total_batch_size`等于`per_device_train_batch_size * gradient_accumulation_steps * world_size`

+ `weight_decay`: `weight`衰减系数，默认值`0.1`

+ `adam_beta2`: 默认为`0.95`

+ 🔥`learning_rate`: 学习率，全参数默认为`1e-5`，`LoRA`等`tuners`为`1e-4`

+ `lr_scheduler_type`: `lr_scheduler`类型，默认为`'cosine'`

+ `lr_scheduler_kwargs`: `lr_scheduler`其他参数。默认为`None`

+ 🔥`gradient_checkpointing_kwargs`: 传入`torch.utils.checkpoint`中的参数。例如设置为`--gradient_checkpointing_kwargs '{"use_reentrant": false}'`。默认为`None`

+ `full_determinism`: 确保训练中获得可重现的结果，注意：这会对性能产生负面影响。默认为`False`

+ 🔥`report_to`: 默认值为`tensorboard`。你也可以指定`--report_to tensorboard wandb swanlab`、`--report_to all`

+ `logging_first_step`: 是否记录第一个`step`的日志，默认为`True`

+ `logging_steps`: 日志打印间隔，默认为`5`

+ `predict_with_generate`: 验证时使用生成式的方式，默认为`False`。

+ `metric_for_best_model`: 默认为`None`，即当`predict_with_generate`设置为`False`时，设置为`'loss'`，否则设置为`'rouge-l'`（在`PPO`训练时，不进行默认值设置；`GRPO`训练设置为`'reward'`）。

+ `greater_is_better`: 默认为`None`，即当`metric_for_best_model`含`'loss'`时，设置为`False`，否则设置为`True`。

## 其他重要参数

+ 🔥`num_train_epochs`: 训练的`epoch`数，默认为`3`

+ 🔥`save_strategy`: 保存模型的策略，可选为`'no'`、`'steps'`、`'epoch'`，默认为`'steps'`

+ 🔥`save_steps`: 默认为`500`

+ 🔥`eval_strategy`: 评估策略。默认为`None`，跟随`save_strategy`的策略

+ 🔥`eval_steps`: 默认为`None`，如果存在评估数据集，则跟随`save_steps`的策略

+ 🔥`save_total_limit`: 最多保存的`checkpoint`数，会将过期的`checkpoint`进行删除。默认为`None`，保存所有的`checkpoint`

+ `max_steps`: 最大训练的`steps`数。在数据集为流式时需要被设置。默认为`-1`

+ 🔥`warmup_ratio`: 默认为`0.`

+ `save_on_each_node`: 默认为`False`。在多机训练时需要被考虑

+ `save_only_model`: 是否只保存模型权重而不包含优化器状态，随机种子状态等内容。默认为`False`

+ 🔥`resume_from_checkpoint`: 断点续训参数，传入`checkpoint`路径。默认为`None`

+ 注意: `resume_from_checkpoint`会读取模型权重，优化器权重，随机种子，并从上次训练的`steps`继续开始训练。你可以指定`--resume_only_model`只读取模型权重。

+ 🔥`ddp_backend`: 可选为`"nccl"`、`"gloo"`、`"mpi"`、`"ccl"`、`"hccl"`、`"cncl"`、`"mccl"`。默认为`None`，进行自动选择

+ 🔥`ddp_find_unused_parameters`: 默认为`None`

+ `ddp_timeout`: 默认为`1800`，单位为秒

+ 🔥`dataloader_num_workers`: 默认为`None`，若是`windows`平台，则设置为`0`，否则设置为`1`

+ `dataloader_pin_memory`: 默认为`True`

+ `dataloader_persistent_workers`: 默认为`False`

+ `dataloader_prefetch_factor`: 默认为`None`，若`dataloader_num_workers > 0`，设置为`10`

+ `train_dataloader_shuffle`: `CPT/SFT`训练的`dataloader`是否随机，默认为`True`。该参数对`IterableDataset`无效。`IterableDataset`采用顺序的方式读取

+ 🔥`neftune_noise_alpha`: `neftune`添加的噪声系数, 默认为`0`，通常可以设置为`5`、`10`、`15`

+ [🔥`use_`](https://github.com/linkedin/Liger-Kernel)[`li`](https://github.com/modelscope/ms-swift/blob/main/examples/train/liger)`ger_kernel`: 是否启用`Liger`内核加速训练并减少显存消耗。默认为`False`。示例`shell`参考这里

+ `average_tokens_across_devices`: 是否在设备之间进行`token`数平均。如果设置为`True`，将使用`all_reduce`同步`num_tokens_in_batch`以进行精确的损失计算。默认为`False`

+ `max_grad_norm`: 梯度裁剪。默认为`1.`

+ `push_to_hub`: 推送`checkpoint`到`hub`。默认为`False`

+ `hub_model_id`: 默认为`None`

+ `hub_private_repo`: 默认为`False`

### Tuner参数

+ 🔥`freeze_llm`: 该参数只对多模态模型生效，可用于全参和`LoRA`，但含义不同。若是全参数训练，将`freeze_llm`设置为`True`将会将`llm`部分权重进行冻结，若是`LoRA`训练且`target_modules`设置为`'all-linear'`，将`freeze_llm`设置为`True`将会取消在`llm`部分添加`LoRA`模块。该参数默认为`False`

+ 🔥`freeze_vit`: 该参数只对多模态模型生效，可用于全参和`LoRA`，含义参考`freeze_llm`。默认为`True`

+ 🔥`freeze_aligner`: 该参数只对多模态模型生效，可用于全参和`LoRA`，含义参考`freeze_llm`。默认为`True`

+ 🔥`target_modules`: 指定`lora`模块，默认为`all-linear`。在`LLM`和多模态`LLM`中，其行为有所不同。若是`LLM`则自动寻找除`lm_head`外的`linear`并附加`tuner`，若是多模态`LLM`，则默认只在`LLM`上附加`tuner`，该行为可以被`freeze_llm`、`freeze_vit`、`freeze_aligner`控制。该参数不限于`LoRA`，可用于其他`tuners`

+ 🔥`target_regex`: 指定`lora`模块的`regex`表达式，默认为`None`。如果该值传入，则`target_modules`参数失效。该参数不限于`LoRA`，可用于其他`tuners`

+ `init_weights`: 初始化`weights`的方法，`LoRA`可以指定为`true`、`false`、`gaussian`、`pissa`、`pissa_niter_[number of iters]`，`Bone`可以指定为`true`、`false`、`bat`。默认值`true`

+ 🔥`modules_to_save`: 在已附加`tuner`后，额外指定一部分原模型模块参与训练和存储。默认为`[]`。该参数不限于`LoRA`，可用于其他`tuners`

#### 全参

+ `freeze_parameters`: 需要被冻结参数的前缀，默认为`[]`

+ `freeze_parameters_ratio`: 从下往上冻结的参数比例，默认为`0`。可设置为`1`将所有参数冻结，结合`trainable_parameters`设置可训练参数

+ `trainable_parameters`: 额外可训练参数的前缀，默认为`[]`。

+ 备注：`trainable_parameters`的优先级高于`freeze_parameters`和`freeze_parameters_ratio`。当指定全参数训练时，会将所有模块设置为可训练的状态，随后根据`freeze_parameters`、`freeze_parameters_ratio`将部分参数冻结，最后根据`trainable_parameters`重新打开部分参数参与训练

#### LoRA

+ 🔥`lora_rank`: 默认为`8`

+ 🔥`lora_alpha`: 默认为`32`

+ `lora_dropout`: 默认为`0.05`

+ `lora_bias`: 默认为`'none'`，可以选择的值: `'none'`、`'all'`。如果你要将`bias`全都设置为可训练，你可以设置为`'all'`

+ `lora_dtype`: 指定`lora`模块的`dtype`类型。支持`'float16'`、`'bfloat16'`、`'float32'`。默认为`None`，跟随原模型类型

+ 🔥`use_dora`: 默认为`False`，是否使用`DoRA`

+ `use_rslora`: 默认为`False`，是否使用`RS-LoRA`

+ 🔥`lorap_lr_ratio`: `LoRA+`参数，默认值`None`，建议值`10~16`。使用`lora`时指定该参数可使用`lora+`

##### LoRA-GA

+ `lora_ga_batch_size`: 默认值为`2`。在`LoRA-GA`中估计梯度以进行初始化时使用的批处理大小。

+ `lora_ga_iters`: 默认值为`2`。在`LoRA-GA`中估计梯度以进行初始化时的迭代次数。

+ `lora_ga_max_length`: 默认值为`1024`。在`LoRA-GA`中估计梯度以进行初始化时的最大输入长度。

+ `lora_ga_direction`: 默认值为`ArB2r`。在`LoRA-GA`中使用估计梯度进行初始化时的初始方向。允许的值有：`ArBr`、`A2rBr`、`ArB2r`和`random`。

+ `lora_ga_scale`: 默认值为`stable`。`LoRA-GA`的初始化缩放方式。允许的值有：`gd`、`unit`、`stable`和`weightS`。

+ `lora_ga_stable_gamma`: 默认值为`16`。当初始化时选择`stable`缩放时的`gamma`值。

#### FourierFt

`FourierFt`使用`target_modules`、`target_regex`、`modules_to_save`三个参数。

+ `fourier_n_frequency`: 傅里叶变换的频率数量, `int`类型, 类似于`LoRA`中的`r`. 默认值`2000`.

+ `fourier_scaling`: `W`矩阵的缩放值, `float`类型, 类似`LoRA`中的`lora_alpha`. 默认值`300.0`.

#### BOFT

`BOFT`使用`target_modules`、`target_regex`、`modules_to_save`三个参数。

+ `boft_block_size`: `BOFT`块尺寸, 默认值`4`.

+ `boft_block_num`: `BOFT`块数量, 不能和`boft_block_size`同时使用.

+ `boft_dropout`: `boft`的`dropout`值, 默认`0.0`.

#### Vera

`Vera`使用`target_modules`、`target_regex`、`modules_to_save`三个参数。

+ `vera_rank`: `Vera Attention`的尺寸, 默认值`256`.

+ `vera_projection_prng_key`: 是否存储`Vera`映射矩阵, 默认为`True`.

+ `vera_dropout`: `Vera`的`dropout`值, 默认`0.0`.

+ `vera_d_initial`: `Vera`的`d`矩阵的初始值, 默认`0.1`.

#### GaLore

+ 🔥`use_galore`: 默认值`False`, 是否使用`GaLore`.

+ `galore_target_modules`: 默认值`None`, 不传的情况下对`attention`和`mlp`应用`GaLore`.

+ `galore_rank`: 默认值`128`, `GaLore`的`rank`值.

+ `galore_update_proj_gap`: 默认值`50`, 分解矩阵的更新间隔.

+ `galore_scale`: 默认值`1.0`, 矩阵权重系数.

+ `galore_proj_type`: 默认值`std`, `GaLore`矩阵分解类型.

+ `galore_optim_per_parameter`: 默认值`False`, 是否给每个`Galore`目标`Parameter`设定一个单独的`optimizer`.

+ `galore_with_embedding`: 默认值`False`, 是否对`embedding`应用`GaLore`.

+ `galore_quantization`: 是否使用`q-galore`. 默认值`False`.

+ `galore_proj_quant`: 是否对`SVD`分解矩阵做量化, 默认`False`.

+ `galore_proj_bits`: `SVD`量化`bit`数.

+ `galore_proj_group_size`: `SVD`量化分组数.

+ `galore_cos_threshold`: 投影矩阵更新的`cos`相似度阈值. 默认值`0.4`.

+ `galore_gamma_proj`: 在投影矩阵逐渐相似后会拉长更新间隔, 本参数为每次拉长间隔的系数, 默认值`2`.

+ `galore_queue_size`: 计算投影矩阵相似度的队列长度, 默认值`5`.

#### LISA

注意仅支持全参数，即`--train_type full`.

+ 🔥`lisa_activated_layers`: 默认值`0`, 代表不使用`LISA`，改为非`0`代表需要激活的`layers`个数，建议设置为`2`或`8`.

+ `lisa_step_interval`: 默认值`20`, 多少`iter`切换可反向传播的`layers`.

#### UNSLOTH

`unsloth`无新增参数，对已有参数进行调节即可支持:

```
--tuner_backend unsloth
--train_type full/lora
--quant_bits 4
```

#### LLAMAPRO

+ 🔥`llamapro_num_new_blocks`: 默认值`4`, 插入的新`layers`总数.

+ `llamapro_num_groups`: 默认值`None`, 分为多少组插入`new_blocks`, 如果为`None`则等于`llamapro_num_new_blocks`, 即每个新的`layer`单独插入原模型.

#### AdaLoRA#

以下参数`train_type`设置为`adalora`时生效. `adalora`的`target_modules`等参数继承于`lora`的对应参数, 但`lora_dtype`参数不生效.

+ `adalora_target_r`: 默认值`8`, `adalora`的平均`rank`.

+ `adalora_init_r`: 默认值`12`, `adalora`的初始`rank`.

+ `adalora_tinit`: 默认值`0`, `adalora`的初始`warmup`.

+ `adalora_tfinal`: 默认值`0`, `adalora`的`final warmup`.

+ `adalora_deltaT`: 默认值`1`, `adalora`的`step`间隔.

+ `adalora_beta1`: 默认值`0.85`, `adalora`的`EMA`参数.

+ `adalora_beta2`: 默认值`0.85`, `adalora`的`EMA`参数.

+ `adalora_orth_reg_weight`: 默认值`0.5`, `adalora`的正则化参数.

#### ReFT

以下参数`train_type`设置为`reft`时生效.

1. `ReFT`无法合并`tuner`

2. `ReFT`和`gradient_checkpointing`不兼容

3. 如果使用`DeepSpeed`遇到问题请暂时卸载`DeepSpeed`

+ 🔥`reft_layers`: `ReFT`应用于哪些层上, 默认为`None`, 代表所有层, 可以输入层号的`list`, 例如`reft_layers 1 2 3 4`

+ 🔥`reft_rank`: `ReFT`矩阵的`rank`, 默认为`4`.



### ReFT参数

+   `reft_intervention_type`: ReFT的类型, 支持`NoreftIntervention`, `LoreftIntervention`, `ConsreftIntervention`, `LobireftIntervention`, `DireftIntervention`, `NodireftIntervention`, 默认为`LoreftIntervention`.

+   `reft_args`: ReFT Intervention中的其他支持参数, 以json-string格式输入.

### LMDeploy参数

参数含义可以查看[lmdeploy文档](https://lmdeploy.readthedocs.io/en/latest/api/pipeline.html#turbomindengineconfig)

+   🔥`tp`: tensor并行度。默认为1

+   `session_len`: 默认为`None`

+   `cache_max_entry_count`: 默认为0.8

+   `quant_policy`: 默认为0

+   `vision_batch_size`: 默认为1

### vLLM参数

参数含义可以查看[vllm文档](https://docs.vllm.ai/en/latest/serving/engine_args.html)

+   🔥`gpu_memory_utilization`: 默认值0.9

+   🔥`tensor_parallel_size`: 默认为1

+   `pipeline_parallel_size`: 默认为1

+   `max_num_seqs`: 默认为256

+   🔥`max_model_len`: 默认为`None`

+   `disable_custom_all_reduce`: 默认为`False`

+   `enforce_eager`: vllm使用pytorch eager模式还是建立cuda graph，默认为`False`。设置为`True`可以节约显存，但会影响效率

+   🔥`limit_mm_per_prompt`: 控制vllm使用多图，默认为`None`。例如传入`--limit_mm_per_prompt '{"image": 5, "video": 2}'`

+   `vllm_max_lora_rank`: 默认为16。vllm对于lora支持的参数

+   `enable_prefix_caching`: 开启vllm的自动前缀缓存，节约重复查询前缀的处理时间。默认为`False`

### 合并参数

+   🔥[`merge_lora`](https://github.com/modelscope/ms-swift/blob/main/examples/export/merge_lora.sh): 是否合并lora，本参数支持lora、llamapro、longlora，默认为`False`。例子参数这里

+   `safe_serialization`: 是否存储safetensors，默认为`True`

+   `max_shard_size`: 单存储文件最大大小，默认`'5GB'`

## 集成参数

### 训练参数

训练参数除包含[基本参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#%E5%9F%BA%E6%9C%AC%E5%8F%82%E6%95%B0)、[`Seq2SeqTrainer`](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#tuner%E5%8F%82%E6%95%B0)参数、tuner参数外，还包含下面的部分:

+   `add_version`: 在`output_dir`上额外增加目录`'<版本号>-<时间戳>'`防止权重覆盖，默认为`True`

+   `resume_only_model`: 默认为`False`。如果在指定`resume_from_checkpoint`的基础上，将该参数设置为`True`，则仅resume模型权重

+   `check_model`: 检查本地模型文件有损坏或修改并给出提示，默认为`True`。如果是断网环境，请设置为`False`

+   🔥`create_checkpoint_symlink`: 额外创建checkpoint软链接，方便书写自动化训练脚本。`best_model`和`last_model`的软链接路径分别为`f'{output_dir}/best'`和`f'{output_dir}/last'`

+   `loss_type`: loss类型。默认为`None`，使用模型自带损失函数

+   🔥`packing`: 是否使用序列packing提升计算效率，默认为`False`

+   注意：使用packing请结合[`--attn_impl`](https://github.com/huggingface/transformers/pull/31629) `flash_attn`使用且`"transformers>=4.44"`，具体查看该PR。

+   🔥`lazy_tokenize`: 是否使用lazy_tokenize。若该参数设置为`False`，则在训练之前对所有的数据集样本进行tokenize（多模态模型则包括从磁盘中读取图片）。该参数在LLM训练中默认设置为`False`，而MLLM训练默认为`True`，节约内存

+   `acc_strategy`: 训练和验证时计算acc的策略。可选为`seq`和`token`级别的acc，默认为`token`

+   `max_new_tokens`: 覆盖生成参数。`predict_with_generate=True`时的最大生成token数量，默认64

+   `temperature`: 覆盖生成参数。`predict_with_generate=True`时的temperature，默认0

+   `optimizer`: plugin的自定义optimizer名称，默认为`None`

+   `metric`: plugin的自定义metric名称。默认为`None`，即在`predict_with_generate=False`的情况下设置为`'acc'`，在`predict_with_generate=True`的情况下设置为`'nlg'`

+   `eval_use_evalscope`: 是否使用evalscope进行训练时评测，需要设置该参数来开启评测，具体使用参考

+   `eval_datasets`: 评测数据集，可设置多个数据集，用空格分割

+   `eval_datasets_args`: 评测数据集参数，json格式，可设置多个数据集的参数

+   `eval_limit`: 评测数据集采样数

+   `eval_generation_config`: 评测时模型推理配置，json格式，默认为`{'max_tokens': 512}`

### RLHF参数

RLHF参数继承于[训练参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#%E8%AE%AD%E7%BB%83%E5%8F%82%E6%95%B0)

+   🔥`rlhf_type`: 人类对齐算法类型，支持dpo、orpo、simpo、kto、cpo、rm、ppo和grpo。默认为`'dpo'`

+   `ref_model`: 采用dpo、kto、ppo、grpo算法且使用全参数训练时需要传入。默认为`None`

+   `ref_model_type`: 同`model_type`。默认为`None`

+   `ref_model_revision`: 同`model_revision`。默认为`None`

+   🔥`beta`: KL正则项系数，默认为`None`，即simpo算法默认为2.，GRPO默认为[`0.04`](https://modelscope.cn/docs/llm-training-and-inference/user-guide/rlhf)，其他算法默认为0.1。具体参考文档

+   `label_smoothing`: 是否使用DPO smoothing，默认值为0

+   🔥`rpo_alpha`: 控制DPO中加入sft_loss的权重，默认为1。最后的loss为`KL_loss + rpo_alpha * sft_loss`

+   `cpo_alpha`: CPO/SimPO loss 中 nll loss的系数, 默认为1.

+   `simpo_gamma`: SimPO算法中的reward margin项，论文建议设置为0.5-1.5，默认为1.

+   `desirable_weight`: KTO算法中对desirable response的loss权重 λD, 默认为1.

+   `undesirable_weight`: KTO算法中对undesirable response的loss权重 λU, 默认为1.

+   `loss_scale`: 覆盖模板参数，默认为`'last_round'`

+   `temperature`: 默认为0.9，该参数将在PPO、GRPO中使用

#### Reward模型参数

reward模型参数将在PPO、GRPO中使用。

+   `reward_model`: 默认为`None`

+   `reward_adapters`: 默认为`[]`

+   `reward_model_type`: 默认为`None`

+   `reward_model_revision`: 默认为`None`

#### PPO参数

以下参数含义可以参考[这里](https://huggingface.co/docs/trl/main/ppo_trainer)

+   `num_ppo_epochs`: 默认为4

+   `whiten_rewards`: 默认为`False`

+   `kl_coef`: 默认为0.05

+   `cliprange`: 默认为0.2

+   `vf_coef`: 默认为0.1

+   `cliprange_value`: 默认为0.2

+   `gamma`: 默认为1.0

+   `lam`: 默认为0.95

+   `num_mini_batches`: 默认为1

+   `local_rollout_forward_batch_size`: 默认为64

+   `num_sample_generations`: 默认为10

+   `response_length`: 默认为512

+   `missing_eos_penalty`: 默认为`None`

#### GRPO参数

+   `num_generations`: GRPO算法中的G值，默认为8

+   `max_completion_length`: GRPO算法中的最大生成长度，默认为512

+   `ds3_gather_for_generation`: 该参数适用于DeepSpeed ZeRO-3。如果启用，策略模型权重将被收集用于生成，从而提高生成速度。然而，禁用此选项允许训练超出单个GPU VRAM的模型，尽管生成速度会变慢。禁用此选项与vLLM生成不兼容。默认为`True`

+   `reward_funcs`: GRPO算法奖励函数，可选项为accuracy、format、cosine 和 repetition，见`swift/plugin/orm.py`。你也可以在plugin中自定义自己的奖励函数。默认为`[]`

+   `reward_weights`: 每个奖励函数的权重。必须与奖励函数的数量匹配。如果为 `None`，则所有奖励的权重都相等，为1.0

+   提示：如果GRPO训练中包含`--reward_model`，则其加在奖励函数的最后位置

+   `log_completions`: 是否记录训练中的模型生成内容，搭配 `--report_to wandb` 使用。默认为`False`

+   提示：若没有设置`--report_to wandb`，则会在`checkpoint`中创建`completions.jsonl`来存储生成内容

+   `use_vllm`: 是否使用vLLM作为GRPO生成的infer_backend，默认为`False`

+   `num_infer_workers`: 每个node上推理worker数量，仅对vllm或者lmdeploy时有效

+   `vllm_device`: 设置vLLM部署的设备，可以设置为`auto`，代表按照`num_infer_workers`数量使用最后的几张卡，否则请传入和`num_infer_workers`相等数量的设备，例如`--vllm_device cuda:1 cuda:2`

+   `vllm_gpu_memory_utilization`: vllm透传参数，默认为0.9

+   `vllm_max_model_len`: vllm透传参数，默认为`None`

+   `vllm_max_num_seqs`: vllm透传参数，默认为256

+   `vllm_enforce_eager`: vllm透传参数，默认为`False`

+   `vllm_limit_mm_per_prompt`: vllm透传参数，默认为`None`

+   `vllm_enable_prefix_caching`: vllm透传参数，默认为`True`

+   `top_k`: 默认为50

+   `top_p`: 默认为0.9

+   `repetition_penalty`: 重复惩罚项。默认为1.

+   `num_iterations`: 每个批次代更新次数，默认为1.

+   `epsilon`: clip 系数，默认为0.2.

+   `epsilon_high`: upper clip 系数，默认为`None`，设置后与`epsilon`共同构成`[epsilon, epsilon_high]`裁剪范围.

+   `async_generate`: 异步rollout以提高训练速度，默认`false`.

+   `sleep_level`: vllm特有参数，在训练和rollout复用卡的时候，可以选择vllm进行offload.

+   `move_model_batches`: 在模型向vLLM/LMDeploy等快速推理框架移动参数时，将layers分为多少个batch. 默认为`None`, 代表整个模型不进行拆分，否则拆分为`move_model_batches+1`(非layer参数)+`1`(多模态部分参数)个

+   `offload_optimizer`: 是否在vLLM/LMDeploy推理时offload optimizer参数，默认为`False`

+   `offload_model`: 是否在vLLM/LMDeploy推理时offload 模型本身，默认为`False`

+   注意：若该参数设置为`True`，训练时grad_norm一直为0，请安装`vllm==0.7.3`

+   `gc_collect_after_offload`: 是否在offload结束时进行gc（python gc和GPU gc），默认为`False`

+   `multi_turn_func`: 多轮GRPO参数, 传入对应的plugin名称, 同时在`plugin/multi_turn.py`中添加好对应的实现

+   `mini_batch_size`: 用于将每个设备上的批次大小（`per_device_batch`）进一步切分为更小的子批次。为确保切分有效，`per_device_train_batch_size` 需要能够被 `mini_batch_size` 整除

+   `dynamic_sample`: 筛除group内奖励标准差为0的数据，额外采样新数据，默认为`False`。

+   `max_resample_times`: `dynamic_sample`设置下限制重采样次数，默认3次。

+   `overlong_filter`: 跳过超长截断的样本，不参与loss计算，默认为`False`。

#### cosine 奖励参数

+   `cosine_min_len_value_wrong`: cosine 奖励函数参数，生成错误答案时，最小长度对应的奖励值。默认值为0.0

+   `cosine_max_len_value_wrong`: 生成错误答案时，最大长度对应的奖励值。默认值为-0.5

+   `cosine_min_len_value_correct`: 生成正确答案时，最小长度对应的奖励值。默认值为1.0

+   `cosine_max_len_value_correct`: 生成正确答案时，最大长度对应的奖励值。默认值为0.5

+   `cosine_max_len`: 生成文本的最大长度限制。默认等于 `max_completion_length`

#### repetition 奖励参数

+   `repetition_n_grams`: 用于检测重复的 n-gram 大小。默认值为3

+   `repetition_max_penalty`: 最大惩罚值，用于控制惩罚的强度。默认值为-1.0

#### soft overlong 奖励参数

+   `soft_max_length`: 论文中的`L_max`，模型的最大生成长度，默认等于`max_completion_length`

+   `soft_cache_length`: 论文中的`L_cache`，控制长度惩罚区间，区间为`[soft_max_length-soft_cache-length, soft_max_length]`

### SWANLAB

+   `swanlab_token`: SwanLab的api-key

+   `swanlab_project`: swanlab的project，需要在页面中预先创建好:https://swanlab.cn/space/~

+   `swanlab_workspace`: 默认为`None`，会使用api-key对应的username

+   `swanlab_exp_name`: 实验名，可以为空，为空时默认传入`--output_dir`的值

+   `swanlab_mode`: 可选`cloud`和`local`，云模式或者本地模式

### 推理参数



# 命令行参数说明

## 推理参数

推理参数除包含[基本参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#%E5%9F%BA%E6%9C%AC%E5%8F%82%E6%95%B0)、[合并参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#%E5%90%88%E5%B9%B6%E5%8F%82%E6%95%B0)、[vLLM参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#vllm%E5%8F%82%E6%95%B0)、[LMDeploy参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#LMDeploy%E5%8F%82%E6%95%B0)外，还包含下面的部分：

+ 🔥`infer_backend`: 推理加速后端，支持`pt`、`vllm`、`lmdeploy`三种推理引擎。默认为`pt`

+ 🔥`max_batch_size`: 指定`infer_backend`为`pt`时生效，用于批量推理，默认为`1`

+ `ddp_backend`: 指定`infer_backend`为`pt`时生效，用于指定多卡推理时的分布式后端，默认为`None`，进行自动选择。多卡推理例子可以查看[这里](https://github.com/modelscope/ms-swift/tree/main/examples/infer/pt)

+ 🔥`result_path`: 推理结果存储路径（`jsonl`），默认为`None`，保存在`checkpoint`目录（含`args.json`文件）或者`./result`目录，最终存储路径会在命令行中打印

+ `metric`: 对推理的结果进行评估，目前支持`acc`和`rouge`。默认为`None`，即不进行评估

+ `val_dataset_sample`: 推理数据集采样数，默认为`None`

## 部署参数

部署参数继承于[推理参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#%E6%8E%A8%E7%90%86%E5%8F%82%E6%95%B0)

+ `host`: 服务`host`，默认为`0.0.0.0`

+ `port`: 端口号，默认为`8000`

+ `api_key`: 访问需要使用的`api_key`，默认为`None`

+ `owned_by`: 默认为`swift`

+ 🔥`served_model_name`: 提供服务的模型名称，默认使用`model`的后缀

+ `verbose`: 打印详细日志，默认为`True`

+ 注意：在`swift app`或者`swift eval`时，默认为`False`

+ `log_interval`: `tokens/s`统计值打印间隔，默认`20`秒。设置为`-1`则不打印

+ `max_logprobs`: 最多返回客户端的`logprobs`数量，默认为`20`

## Web-UI参数

+ `server_name`: web-ui的`host`，默认为`0.0.0.0`

+ `server_port`: web-ui的`port`，默认为`7860`

+ `share`: 默认为`False`

+ `lang`: web-ui的语言，可选为`zh`、`en`。默认为`zh`

## App参数

App参数继承于[部署参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#%E9%83%A8%E7%BD%B2%E5%8F%82%E6%95%B0)，[Web-UI参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#Web-UI%E5%8F%82%E6%95%B0)

+ `base_url`: 模型部署的`base_url`，例如`http://localhost:8000/v1`。默认为`None`，使用本地部署

+ `studio_title`: studio的标题。默认为`None`，设置为模型名

+ `is_multimodal`: 是否启动多模态版本的`app`。默认为`None`，自动根据`model`判断，若无法判断，设置为`False`

+ `lang`: 覆盖Web-UI参数，默认为`en`

## 评测参数

评测参数继承于[部署参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#%E9%83%A8%E7%BD%B2%E5%8F%82%E6%95%B0)

+ 🔥`eval_backend`: 评测后端，默认为`Native`，也可以指定为`OpenCompass`或`VLMEvalKit`

+ 🔥`eval_dataset`: 评测数据集，请查看[评测文档](https://modelscope.cn/docs/llm-training-and-inference/user-guide/evaluation)

+ `eval_limit`: 每个评测集的采样数，默认为`None`

+ `eval_output_dir`: 评测存储结果的文件夹，默认为`eval_output`

+ 🔥`local_dataset`: 部分评测集，如`CMB`无法直接运行，需要下载额外数据包才可以使用。设置本参数为`true`可以自动下载全量数据包，并在当前目录下创建`data`文件夹并开始评测。数据包仅会下载一次，后续会使用缓存。该参数默认为`false`。

+ 注意：默认评测会使用`~/.cache/opencompass`下的数据集，在指定本参数后会直接使用当前目录下的`data`文件夹

+ `temperature`: 覆盖生成参数，默认为`0`

+ `eval_num_proc`: 评测时客户端最大并发数，默认为`16`

+ 🔥`eval_url`: 评测url，例如`http://localhost:8000/v1`。例子可以查看[这里](https://github.com/modelscope/ms-swift/tree/main/examples/eval/eval_url)。默认为`None`，采用本地部署评估

### MCTS

+ `rollout_depth`: `rollout` 时的最大深度，默认为 `5`

+ `rollout_start_depth`: 开始 `rollout` 时的深度，低于此深度的节点只会进行 `expand` 操作，默认为 `3`

+ `max_iterations`: `mcts` 的最大迭代次数，默认为 `100`

+ `process_reward_rate`: `select` 中计算 `value` 时 `process reward` 占的比例，默认为 `0.0`，即不使用 `PRM`

+ `exploration_rate`: `UCT` 算法中的探索参数，值越大越照顾探索次数较小的节点，默认为 `0.5`

+ `api_key`: 使用 `client` 作为推理引擎时需要，默认为 `EMPTY`

+ `base_url`: 使用 `client` 作为推理引擎时需要，默认为 `https://dashscope.aliyuncs.com/compatible-mode/v1`

## 导出参数

导出参数除包含[基本参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#%E5%9F%BA%E6%9C%AC%E5%8F%82%E6%95%B0)和[合并参数](https://modelscope.cn/docs/llm-training-and-inference/user-guide/command-line-parameters#%E5%90%88%E5%B9%B6%E5%8F%82%E6%95%B0)外，还包含下面的部分:

+ 🔥`output_dir`: 导出结果存储路径。默认为`None`，会自动设置合适后缀的路径

+ `exist_ok`: 如果`output_dir`存在，不抛出异常，进行覆盖。默认为`False`

+ 🔥`quant_method`: 可选为`gptq`、`awq`、`bnb`，默认为`None`。例子参考[这里](https://github.com/modelscope/ms-swift/tree/main/examples/export/quantize)

+ `quant_n_samples`: `gptq`/`awq`的校验集采样数，默认为`256`

+ `max_length`: 校准集的`max_length`，默认值`2048`

+ `quant_batch_size`: 量化`batch_size`，默认为`1`

+ `group_size`: 量化`group`大小，默认为`128`

+ `to_ollama`: 产生`ollama`所需的`Modelfile`文件。默认为`False`

+ 🔥`to_mcore`: `HF`格式权重转成`Megatron`格式。默认为`False`

+ `to_hf`: `Megatron`格式权重转成`HF`格式。默认为`False`

+ `mcore_model`: `mcore`格式模型路径。默认为`None`

+ `thread_count`: `--to_mcore true`时的模型切片数。默认为`None`，根据模型大小自动设置，使得最大分片小于`10GB`

+ `test_convert_precision`: 测试`HF`和`Megatron`格式权重转换的精度误差。默认为`False`

+ 🔥`push_to_hub`: 是否推送`hub`，默认为`False`。例子参考[这里](https://github.com/modelscope/ms-swift/blob/main/examples/export/push_to_hub.sh)

+ `hub_model_id`: 推送的`model_id`，默认为`None`

+ `hub_private_repo`: 是否是`private repo`，默认为`False`

+ `commit_message`: 提交信息，默认为`update files`

## 采样参数

+ `prm_model`: 过程奖励模型的类型，可以是模型`id`（以`pt`方式拉起），或者`plugin`中定义的`prm key`（自定义推理过程）

+ `orm_model`: 结果奖励模型的类型，通常是通配符或测试用例等，一般定义在`plugin`中

+ `sampler_type`: 采样类型，目前支持`sample`、`mcts`，未来会支持`dvts`

+ `sampler_engine`: 支持`pt`、`lmdeploy`、`vllm`、`client`、`no`，默认为`pt`，采样模型的推理引擎

+ `sampler_type`: 采样类型，目前支持`sample`（`do_sample`方式），未来会支持`mcts`和`dvts`

+ `sampler_engine`: 支持`pt`、`lmdeploy`、`vllm`、`no`，默认为`pt`，采样模型的推理引擎

+ `output_dir`: 输出目录，默认为`sample_output`

+ `output_file`: 输出文件名称，默认为`None`使用时间戳作为文件名。传入时不需要传入目录，仅支持`jsonl`格式

+ `override_exist_file`: 如`output_file`存在，是否覆盖

+ `num_sampling_per_gpu_batch_size`: 每次采样的`batch_size`

+ `num_sampling_per_gpu_batches`: 共采样多少`batch`

+ `n_best_to_keep`: 返回多少最佳`sequences`

+ `data_range`: 本采样处理数据集的分片。传入格式为`2 3`，代表数据集分为`3`份处理（这意味着通常有三个`swift sample`在并行处理），本实例正在处理第`3`个分片

+ `temperature`: 在这里默认为`1.0`

+ `prm_threshold`: `PRM`阈值，低于该阈值的结果会被过滤掉，默认值为`0`

+ `easy_query_threshold`: 单个`query`的所有采样中，`ORM`评估如果正确，大于该比例的`query`会被丢弃，防止过于简单的`query`出现在结果中，默认为`None`，代表不过滤

+ `engine_kwargs`: 传入`sampler_engine`的额外参数，以`json string`传入，例如`{"cache_max_entry_count":0.7}`

+ `num_return_sequences`: 采样返回的原始`sequence`数量。默认为`64`，本参数对`sample`采样有效

+ `cache_files`: 为避免同时加载`prm`和`generator`造成显存`OOM`，可以分两步进行采样，第一步将`prm`和`orm`置为`None`，则所有结果都会输出到文件中，第二次运行采样将`sampler_engine`置为`no`并传入`--cache_files`为上次采样的输出文件，则会使用上次输出的结果进行`prm`和`orm`评估并输出最终结果。

+ 注意：使用`cache_files`时，`--dataset`仍然需要传入，这是因为`cache_files`的`id`是由原始数据计算的`md5`，需要把两部分信息结合使用。

## 特定模型参数

特定模型参数可以通过`--model_kwargs`或者环境变量进行设置，例如: `--model_kwargs '{"fps_max_frames": 12}'`或者`FPS_MAX_FRAMES=12`

### qwen2_vl, qvq, qwen2_5_vl

参数含义同`qwen_vl_utils`或者`qwen_omni_utils`库，可以查看[这里](https://github.com/QwenLM/Qwen2-VL/blob/main/qwen-vl-utils/src/qwen_vl_utils/vision_process.py#L24)

+ `IMAGE_FACTOR`: 默认为`28`

+ `MIN_PIXELS`: 默认为`4 * 28 * 28`

+ 🔥`MAX_PIXELS`: 默认为`16384 * 28 * 28`，参考[这里](https://github.com/modelscope/ms-swift/blob/main/examples/train/multimodal/ocr.sh#L3)

+ `MAX_RATIO`: 默认为`200`

+ `VIDEO_MIN_PIXELS`: 默认为`128 * 28 * 28`

+ 🔥`VIDEO_MAX_PIXELS`: 默认为`768 * 28 * 28`，参考[这里](https://github.com/modelscope/ms-swift/blob/main/examples/train/multimodal/video.sh#L7)

+ `VIDEO_TOTAL_PIXELS`: 默认为`24576 * 28 * 28`

+ `FRAME_FACTOR`: 默认为`2`

+ `FPS`: 默认为`2.0`

+ `FPS_MIN_FRAMES`: 默认为`4`

+ 🔥`FPS_MAX_FRAMES`: 默认为`768`，参考[这里](https://github.com/modelscope/ms-swift/blob/main/examples/train/multimodal/video.sh#L8)

### qwen2_audio

+ `SAMPLING_RATE`: 默认为`16000`

### qwen2_5_omni

`qwen2_5_omni`除了包含`qwen2_5_vl`和`qwen2_audio`的模型特定参数外，还包含以下参数：

+ `USE_AUDIO_IN_VIDEO`: 默认为`False`

+ 🔥`ENABLE_AUDIO_OUTPUT`: 默认为`True`。若使用`zero3`进行训练，请设置为`False`

### internvl, internvl_phi3

参数含义可以查看[这里](https://modelscope.cn/models/OpenGVLab/Mini-InternVL-Chat-2B-V1-5)

+ `MAX_NUM`: 默认为`12`

+ `INPUT_SIZE`: 默认为`448`

### internvl2, internvl2_phi3, internvl2_5, internvl3

参数含义可以查看[这里](https://modelscope.cn/models/OpenGVLab/InternVL2_5-2B)

+ `MAX_NUM`: 默认为`12`

+ `INPUT_SIZE`: 默认为`448`

+ `VIDEO_MAX_NUM`: 默认为`1`。视频的`MAX_NUM`

+ `VIDEO_SEGMENTS`: 默认为`8`

### minicpmv2_6, minicpmo2_6

+ `MAX_SLICE_NUMS`: 默认为`9`，参考[这里](https://modelscope.cn/models/OpenBMB/MiniCPM-V-2_6/file/view/master?fileName=config.json&status=1)

+ `VIDEO_MAX_SLICE_NUMS`: 默认为`1`，`视频`的`MAX_SLICE_NUMS`，参考[这里](https://modelscope.cn/models/OpenBMB/MiniCPM-V-2_6)

+ `MAX_NUM_FRAMES`: 默认为`64`，参考[这里](https://modelscope.cn/models/OpenBMB/MiniCPM-V-2_6/file/view/master?fileName=config.json&status=1)

### minicpmo2_6

+ `INIT_TTS`: 默认为`False`

+ `INIT_AUDIO`: 默认为`False`



# 环境变量配置

## ovis1_6, ovis2

+   [MA](https://github.com/AIDC-AI/Ovis/blob/d248e34d755a95d24315c40e2489750a869c5dbc/ovis/model/modeling_ovis.py#L312)`X_PARTITION`: 默认为9，参考这里

## mplug_owl3, mplug_owl3_241101

+   [MA](https://modelscope.cn/models/iic/mPLUG-Owl3-7B-240728)`X_NUM_FRAMES`: 默认为16，参考这里

## xcomposer2_4khd

+   [HD](https://modelscope.cn/models/Shanghai_AI_Laboratory/internlm-xcomposer2-4khd-7b)`_NUM`: 默认为55，参考这里

## xcomposer2_5

+   [HD](https://modelscope.cn/models/AI-ModelScope/internlm-xcomposer2d5-7b/file/view/master?fileName=modeling_internlm_xcomposer2.py&status=1#L254)`_NUM`: 图片数量为1时，默认值为24。大于1，默认为6。参考这里

## video_cogvlm2

+   [NU](https://github.com/THUDM/CogVLM2/blob/main/video_demo/inference.py#L22)`M_FRAMES`: 默认为24，参考这里

## phi3_vision

+   [NU](https://modelscope.cn/models/LLM-Research/Phi-3.5-vision-instruct)`M_CROPS`: 默认为4，参考这里

## llama3_1_omni

+   [N_](https://github.com/ictnlp/LLaMA-Omni/blob/544d0ff3de8817fdcbc5192941a11cf4a72cbf2b/omni_speech/infer/infer.py#L57)`MELS`: 默认为128，参考这里

## video_llava

+   `NUM_FRAMES`: 默认为16

## 其他环境变量

+   `CUDA_VISIBLE_DEVICES`: 控制使用哪些GPU卡。默认使用所有卡

+   `ASCEND_RT_VISIBLE_DEVICES`: 控制使用哪些NPU卡（ASCEND卡生效）。默认使用所有卡

+   `MODELSCOPE_CACHE`: 控制缓存路径。

+   `NPROC_PER_NODE`: `torchrun`中`--nproc_per_node`的参数透传。默认为1。若设置了`NPROC_PER_NODE`或者`NNODES`环境变量，则使用`torchrun`启动训练或推理

+   `MASTER_PORT`: `torchrun`中`--master_port`的参数透传。默认为29500

+   `MASTER_ADDR`: `torchrun`中`--master_addr`的参数透传

+   `NNODES`: `torchrun`中`--nnodes`的参数透传

+   `NODE_RANK`: `torchrun`中`--node_rank`的参数透传

+   `LOG_LEVEL`: 日志的level，默认为'INFO'，你可以设置为'WARNING', 'ERROR'等。
