
# LongWriter继续训练

> 来源：[https://v11enp9ok1h.feishu.cn/docx/OEWYdT6M9oDds2xJNwvcdxxKn0V](https://v11enp9ok1h.feishu.cn/docx/OEWYdT6M9oDds2xJNwvcdxxKn0V)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

如果你想继续在我们的`LongWriter`模型训练，可以通过下面的方式完成

## SFT微调

下载模型权重

```
git clone https://huggingface.co/THUDM/LongWriter-glm4-9b
```

下载混合数据

```
git clone https://huggingface.co/datasets/THUDM/LongWriter-6k
```

下载项目代码

```
git clone https://github.com/THUDM/LongWriter
```

### 准备数据

准备自己的SFT数据，格式为gptshare的jsonl文件，命名为`general.jsonl`

示例

```
{
    &quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;...&quot;}, 
                 {&quot;role&quot;: &quot;assistant&quot;, &quot;content&quot;: &quot;...&quot;}, ...]
    }
```

将`datasets/THUDM/LongWriter-6k`中的文件重命名成`LongWriter-6k.jsonl`，复制到项目代码下的`train`文件夹

```
cp LongWriter-6k/long.jsonl  LongWriter/train/LongWriter-6k.jsonl
```

### 安装环境

> 记得使用虚拟环境

安装项目目录的依赖

```
pip install -r requirements.txt 
```

分别安装`DeepSpeed`、`flash-attn>=2.0.0`、`transformers==4.33.0`、`accelerate`、`wandb`、`tiktoken`

### 配置文件

修改`ds_config/stage3.json`文件

```
{
    &quot;bf16&quot;: {
      &quot;enabled&quot;: &quot;auto&quot;
    },

    &quot;fp16&quot;: {
      &quot;enabled&quot;: &quot;auto&quot;,
      &quot;loss_scale&quot;: 0,
      &quot;loss_scale_window&quot;: 1000,
      &quot;initial_scale_power&quot;: 16,
      &quot;hysteresis&quot;: 2,
      &quot;min_loss_scale&quot;: 1
    },

    &quot;checkpoint&quot;: {
      &quot;use_node_local_storage&quot;: true
    },
    &quot;zero_optimization&quot;: {
      &quot;stage&quot;: 3,
      &quot;offload_optimizer&quot;: {
        &quot;device&quot;: &quot;cpu&quot;,
        &quot;pin_memory&quot;: true
      },
      &quot;offload_param&quot;: {
        &quot;device&quot;: &quot;cpu&quot;,
        &quot;pin_memory&quot;: true
      },
      &quot;overlap_comm&quot;: true,
      &quot;contiguous_gradients&quot;: true,
      &quot;sub_group_size&quot;: 1e9,
      &quot;reduce_bucket_size&quot;: &quot;auto&quot;,
      &quot;stage3_prefetch_bucket_size&quot;: &quot;15099494&quot;,
      &quot;stage3_param_persistence_threshold&quot;: &quot;auto&quot;,
      &quot;stage3_max_live_parameters&quot;: 1e8,
      &quot;stage3_max_reuse_distance&quot;: 1e8,
      &quot;stage3_gather_16bit_weights_on_model_save&quot;: true
    },
    &quot;gradient_accumulation_steps&quot;: &quot;auto&quot;,
    &quot;gradient_clipping&quot;: &quot;auto&quot;,
    &quot;steps_per_print&quot;: 100,
    &quot;train_batch_size&quot;: &quot;auto&quot;,
    &quot;train_micro_batch_size_per_gpu&quot;: &quot;auto&quot;,
    &quot;wall_clock_breakdown&quot;: false,

    &quot;wandb&quot;: {
      &quot;enabled&quot;: true,  
      &quot;project&quot;: &quot;glm4_longwriter_sft&quot;
    }
 
  }
```

### 执行预处理

修改`pre_tokenize_glm4.py`文件

```
tokenizer = AutoTokenizer.from_pretrained( 改成模型权重路径, trust_remote_code=True)
```

执行命令，记得当前目录需要存在`general.jsonl`、`LongWriter-6k.jsonl`两个文件

```
python pre_tokenize_glm4.py 
```

如果没有出错，继续执行

```
python sort_and_group.py --train_file ./data/glm4/longwriter
```

### 开始训练

`glm4_longwriter.sh`脚本中可以配置使用显卡，这里使用了第0个，训练模型需要80G显存

```
deepspeed --include localhost:0 
```

执行命令，`WANDB_API_KEY`从`wandb.ai`获取，不需要可以不填写，并且将`ds_config/stage3.json`文件中`wandb`删除

```
WANDB_API_KEY=key ./scripts/glm4_longwriter.sh 
```

### 注意事项

+ 修改`stage3_prefetch_bucket_size`为整数例如`"15099494"`

+ 环境满足要求：`transformers==4.33.0` 和 `flash-attn>=2.0.0`。

+ 训练`GLM-4-9b`时出错：⚠️ `return self.mergeable_ranks[token] KeyError: '<|endoftext|>'`。请确保您已将`GLM-4-9b`模型文件夹下的`tokenization_chatglm.py`和`modeling_chatglm.py`文件替换为`patch/`下的补丁文件。
