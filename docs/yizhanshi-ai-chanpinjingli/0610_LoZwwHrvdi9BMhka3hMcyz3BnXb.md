# Megatron-SWIFT训练

> 来源：[https://v11enp9ok1h.feishu.cn/docx/YVLXdIVhaonFeIxmVgLcXKCwnDE](https://v11enp9ok1h.feishu.cn/docx/YVLXdIVhaonFeIxmVgLcXKCwnDE)

# Megatron-SWIFT训练

SWIFT引入了Megatron的并行技术来加速大模型的训练，包括数据并行、张[量并行、流水线并行、序](https://modelscope.cn/docs/llm-training-and-inference/user-guide/supported-models-and-datasets)列并行，上下文并行。支持Megatron训练的模型可以参考支持的模型与数据集文档。

## 环境准#

**使用Megatron-SWIFT，除了安**装swift依赖外，还需要安装以下内容：

```
pip install pybind11
# transformer_engine
# 若出现安装错误，可以参考该issue解决: https://github.com/modelscope/ms-swift/issues/3793
pip install git+https://github.com/NVIDIA/TransformerEngine.git@stable

# apex
git clone https://github.com/NVIDIA/apex
cd apex
pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --config-settings &quot;--build-option=--cpp_ext&quot; --config-settings &quot;--build-option=--cuda_ext&quot; ./
```

**或者你也可以使用镜像：**

```
modelscope-registry.cn-hangzhou.cr.aliyuncs.com/modelscope-repo/modelscope:ubuntu22.04-cuda12.4.0-py311-torch2.6.0-vllm0.8.3-modelscope1.25.0-swift3.3.0.post1
modelscope-registry.us-west-1.cr.aliyuncs.com/modelscope-repo/modelscope:ubuntu22.04-cuda12.4.0-py311-torch2.6.0-vllm0.8.3-modelscope1.25.0-swift3.3.0.post1
```

依[赖库Megatron-LM将](https://github.com/NVIDIA/Megatron-LM/tree/core_r0.11.0)会由swift进行git clone并安装，不需要用户手动安装。你也可以通过环境变量MEGATRON_LM_PATH指向已经下载好的repo路径（断网环境，core_r0.11.0分支）。

## 快速入门案例

这里介绍使用2卡80GiB A100对Qwen2.5-7B-Instruct模型进行自我认知微调的快速入门案例，以下最佳实践可以在10分钟内完成。

首先，我们需要将HF格式的权重转为Megatron格式：

```
CUDA_VISIBLE_DEVICES=0 \
swift export \
    --model Qwen/Qwen2.5-7B-Instruct \
    --to_mcore true \
    --torch_dtype bfloat16 \
    --test_convert_precision true \
    --output_dir Qwen2.5-7B-Instruct-mcore
```

+   注意：若出现OOM，请将--test_convert_precision true参数去除

然后，使用以下脚本进行训练，训练所需显存资源为2*80GiB：

```
NPROC_PER_NODE=2 \
CUDA_VISIBLE_DEVICES=0,1 \
megatron sft \
    --load Qwen2.5-7B-Instruct-mcore \
    --dataset &#x27;AI-ModelScope/alpaca-gpt4-data-zh#500&#x27; \
              &#x27;AI-ModelScope/alpaca-gpt4-data-en#500&#x27; \
              &#x27;swift/self-cognition#500&#x27; \
    --tensor_model_parallel_size 2 \
    --micro_batch_size 4 \
    --global_batch_size 16 \
    --recompute_granularity selective \
    --train_iters 100 \
    --eval_iters 5 \
    --finetune true \
    --cross_entropy_loss_fusion true \
    --lr 1e-5 \
    --lr_warmup_iters 10 \
    --min_lr 1e-6 \
    --save megatron_output/Qwen2.5-7B-Instruct \
    --save_interval 100 \
    --max_length 2048 \
    --system &#x27;You are a helpful assistant.&#x27; \
    --num_workers 4 \
    --no_save_optim true \
    --no_save_rng true \
    --dataset_num_proc 4 \
    --model_author swift \
    --model_name swift-robot
```

最后，将Megatron格式权重转为HF格式：

```
CUDA_VISIBLE_DEVICES=0 \
swift export \
    --mcore_model megatron_output/Qwen2.5-7B-Instruct/vx-xxx \
    --to_hf true \
    --torch_dtype bfloat16 \
    --test_convert_precision true \
    --output_dir megatron_output/Qwen2.5-7B-Instruct/vx-xxx-hf
```

我们对生成的HF格式权重进行推理：

```
CUDA_VISIBLE_DEVICES=0 \
swift infer \
    --model megatron_output/Qwen2.5-7B-Instruct/vx-xxx-hf \
    --stream true \
    --temperature 0 \
    --max_new_tokens 2048
```

推理结果如下：

```
&lt;&lt;&lt; who are you?
I am a language model developed by swift, you can call me swift-robot. How can I assist you?
```

+   [更多](https://github.com/modelscope/ms-swift/tree/main/examples/train/megatron)案例：例如packing、多机，可以查看这里。

+   若要进行预训练，你可以使用megatron pt替代megatron sft，这将会使用生成式的template进行训练。

## Benchmark#

使用megatron sft和swift sft在单机八卡A800环境下进行14B模[型全](https://github.com/modelscope/ms-swift/tree/main/examples/train/megatron/benchmark)参数训练的速度对比如下，对应脚本参考这里。

## 命令行参数

### Megatron参数#

训练参数:

+   🔥micro_batch_size: 每个device的批次大小，默认为1。

+   🔥global_batch_size: 总批次大小，等价于micro_batch_size*数据并行大小*梯度累加步数。默认为16。

+   🔥recompute_granularity: 重新计算激活的粒度，可选项为'full', 'selective'。其中full代表重新计算整个transformer layer，selective代表只计算transformer layer中的核心注意力部分。通常'selective'是推荐的。默认为'selective'。

+   recompute_method: 该参数需将recompute_granularity设置为'full'才生效，可选项为'uniform', 'block'。默认为None。

+   recompute_num_layers: 该参数需将recompute_granularity设置为'full'才生效，默认为None。若recompute_method设置为uniform，该参数含义为每个均匀划分的重新计算单元的transformer layers数量。例如你可以指定为--recompute_granularity full --recompute_method uniform --recompute_num_layers 4。recompute_num_layers越大，显存占用越小，计算成本越大。默认为None。

+   deterministic_mode: 确定性模式，这会导致训练速度下降，默认为False。

+   🔥train_iters: 训练的总迭代次数，默认为None。

+   🔥log_interval: log的时间间隔（单位：iters），默认为5。

+   tensorboard_dir: tensorboard日志写入的目录。默认None，即存储在f'{save}/runs'目录下。

+   no_masked_softmax_fusion: 默认为False。用于禁用query_key_value的scaling, masking, and softmax融合。

+   no_bias_dropout_fusion: 默认为False。用于禁用bias和dropout的融合。

+   no_bias_swiglu_fusion: 默认为False。指定--no_bias_dropout_fusion true，用于禁止bias和swiglu融合。

+   no_rope_fusion: 默认为False。指定--no_rope_fusion true用于禁止rope融合。

+   no_gradient_accumulation_fusion: 默认为False。指定--no_gradient_accumulation_fusion true用于禁用梯度累加融合。

+   🔥cross_entropy_loss_fusion: 启动交叉熵损失计算融合。默认为False。

+   🔥use_flash_attn: 使用 FlashAttention 注意力机制实现，默认为False。

+   optimizer: 优化器类型，可选为'adam'、'sgd'。默认为adam。

+   dataloader_type: 默认为'cyclic'，可选为'single', 'cyclic', 'external'。若开启--streaming，则设置为external。

+   manual_gc: 禁用默认垃圾回收器，手动触发垃圾回收。默认为False。

+   manual_gc_interval: 触发垃圾回收的间隔。默认为0。

+   seed: python、numpy、pytorch和cuda的随机种子，默认为42。

+   🔥num_workers: dataloder的workers数量，默认为4。

+   seq_length: 默认为None，即设置为max_length。对数据集长度进行限制请使用基本参数中的--max_length控制，无需设置此参数。

+   use_cpu_initialization: 在cpu上初始化权重，默认为False。在进行HF和MCore权重转换时会被使用。

+   no_create_attention_mask_in_dataloader: 在dataloader中不创建attention mask，默认为True。

学习率参数:

+   🔥lr: 初始学习率，最终会根据学习率预热策略和衰减策略决定每个迭代的学习率，默认为1e-5。

+   lr_decay_style: 学习率衰减策略，默认为'cosine'。通常设置为'cosine', 'linear', 'constant'。

+   🔥lr_decay_iters: 学习率衰减的迭代次数。默认为None，则设置为--train_iters。

+   🔥lr_warmup_iters: 线性学习率预热的迭代次数，默认为0。

+   🔥min_lr: 学习率的最小值，将低于改阈值的学习率裁剪为该值，默认为0。

正则化参数:

+   🔥weight_decay: 默认为0.1。

+   🔥clip_grad: l2梯度裁剪，默认为1.0。

+   adam_beta1: 默认0.9。

+   adam_beta2: 默认0.95。

+   adam_eps: 默认1e-8。

+   sgd_momentum: 默认为0.9。

checkpoint参数:

+   🔥save: checkpoint的输出目录，默认None。在训练中，若未设置该参数，则默认为f'megatron_output/{model_suffix}'，例如'megatron_output/Qwen2.5-7B-Instruct'。

+   🔥save_interval: checkpoint保存的间隔（steps），默认为500。

+   注意：训练结束时一定会保存权重。

+   🔥no_save_optim: 不保存optimizer，默认为False。

+   🔥no_save_rng: 不保存rng，默认为False。

+   🔥load: 加载的checkpoint目录，默认None。

+   🔥no_load_optim: 不载入optimizer，默认为False。

+   🔥no_load_rng: 不载入rng，默认为False。

+   🔥finetune: 将模型加载并微调。不加载检查点的优化器和随机种子状态，并将迭代数设置为0。默认为False。

+   ckpt_format: checkpoint的格式。可选为'torch', 'torch_dist', 'zarr'。默认为'torch_dist'。

+   no_initialization: 不对权重进行初始化，默认为True。

+   auto_detect_ckpt_format: 自动检测ckpt format为legacy还是distributed格式。默认为True。

+   exit_on_missing_checkpoint: 如果设置了–-load，但找不到检查点，则直接退出，而不是初始化。默认为True。

分布式参数:

+   distributed_backend: 分布式后端，可选为'nccl', 'gloo'。默认为nccl。

+   🔥use_distributed_optimizer: 使用分布式优化器。默认为True。

+   🔥tensor_model_parallel_size: tp数，默认为1。

+   🔥pipeline_model_parallel_size: pp数，默认为1。

+   🔥sequence_parallel: 启动序列并行的优化器。默认为False。

+   🔥context_parallel_size: cp数，默认为1。

+   tp_comm_overlap: 启用张量并行通信与GEMM（通用矩阵乘法）内核的重叠（降低通信耗时）。默认为False。

+   overlap_grad_reduce: 启用DDP中grad reduce操作的重叠（降低DP通信耗时）。默认为False。

+   overlap_param_gather: 启用分布式优化器中参数all-gather的重叠（降低DP通信耗时）。默认为False。

+   distributed_timeout_minutes: torch.distributed的timeout时间（单位为分钟），默认为60分钟。

日志参数

+   log_params_norm: 记录参数的norm。默认为True。

+   log_throughput: 记录每个GPU的吞吐量。默认为True。

+   注意：在非packing情况下，log_throughput并不准确，因为seq_length并不等于真实序列长度。

+   tensorboard_log_interval: 记录到tensorboard的间隔（steps），默认为1。

+   tensorboard_queue_size: 队列长度（与磁盘IO相关），类似于写入的间隔。默认为50。

+   log_timers_to_tensorboard: 记录timers到tensorboard。默认为True。

+   no_log_learning_rate_to_tensorboard: 不记录学习率到tensorboard。默认为False。

+   log_validation_ppl_to_tensorboard: 将验证困惑度写入tensorboard。默认为True。

+   log_memory_to_tensorboard: 将内存日志写入tensorboard。默认为True。

+   logging_leval: 日志级别。默认为None。

评估参数

+   🔥eval_iters: 评估的迭代次数，默认为100。

+   🔥eval_interval: 评估的间隔（steps），默认为None，即设置为save_interval。

混合精度参数

+   fp16: fp16模式。默认为False。会根据模型的torch_dtype进行设置。请使用--torch_dtype进行设置，默认读取config.json。

+   bf16: bf16模式。默认为False。会根据模型的torch_dtype进行设置。

+   apply_query_key_layer_scaling: 将Q * K^T 缩放为 1 / 层数（例如：第layer_num层则除以layer_num）。这对fp16训练很有帮助。默认为None，即若使用--fp16，则设置为True。

+   attention_softmax_in_fp32: 在attention_mask和softmax中使用fp32进行计算。默认为True。

模型参数: （以下参数通常不需要进行设置，会根据HF模型的config.json进行配置，用户无需关心）

+   num_layers: transformer layers的层数，默认为None。

+   hidden_size: transformer hidden size，默认为None。

+   ffn_hidden_size: transformer FFN层的hidden size。默认为None，设置为4*hidden_size。

+   num_attention_heads: transformer attention heads的个数，默认为None。

+   group_query_attention: 默认为None。若num_query_groups>1，group_query_attention设置为True，否则为False。

+   num_query_groups: 默认为1。

+   max_position_embeddings: 位置编码的最大长度，默认为None。

+   position_embedding_type: 位置编码的类型，可选为'learned_absolute'、'rope'、'relative'和'none'，默认为'rope'。

+   rotary_base: 默认为10000。

+   rotary_percent: 默认为1.。

+   normalization: 可选为'LayerNorm', 'RMSNorm'，默认为RMSNorm。

+   norm_epsilon: 默认为1e-5。

+   swiglu: 使用swiglu替代默认的gelu。默认为True。

+   untie_embeddings_and_output_weights: 解开embedding和输出权重的绑定，默认为True。

+   disable_bias_linear: 禁用linear层的bias。默认为True。

+   add_qkv_bias: 仅在QKV的linear中增加bias，默认为True。

+   attention_dropout: 默认为0.。

+   hidden_dropout: 默认为0.。

+   transformer_impl: 使用哪种transformer实现，可选项为'local'和'transformer_engine'。默认为transformer_engine。

+   padded_vocab_size: 完整词表大小，默认为None。

+   rope_scaling: rope_scaling相关参数，默认为None。格式参考llama3.1 config.json，传入json字符串。

### Megatron训练参数#

Megatron训练参数继承自Megatron参数和基本参数。基本参数的内容可以参考。此外还包括以下参数：

+   add_version: 在save上额外增加目录'<版本号>-<时间戳>'防止权重覆盖，默认为True。

+   🔥packing: 是否使用序列packing，默认为False。

+   🔥streaming: 流式读取并处理数据集，默认False。通常在处理大型数据集时，设置为True。更多流式的参数查看命令行参数文档。

+   lazy_tokenize: 默认为False。若该参数设置为False，则在训练之前对所有的数据集样本进行tokenize（这可以避免在训练中出现报错）；设置为True，则在训练中对数据集进行tokenize（这可以节约内存）。