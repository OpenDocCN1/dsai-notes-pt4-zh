
# LLamaFactory微调Longwriter

> 来源：[https://ai.feishu.cn/docx/RPCYdjkNeoerYKxBHAoca0HWnMc](https://ai.feishu.cn/docx/RPCYdjkNeoerYKxBHAoca0HWnMc)

## 微调资源

根据 `pack` 包的长度定义，一般 20000 tokens 长度需要 80G 显存。

## 下载源码

### 下载仓库

```bash
git clone https://github.com/hiyouga/LLaMA-Factory.git
```

### 下载模型权重

```bash
git clone https://huggingface.co/THUDM/LongWriter-glm4-9b
```

[https://github.com/hiyouga/LLaMA-Factory/pull/5532](https://github.com/hiyouga/LLaMA-Factory/pull/5532)

## 环境说明

建议使用 `conda` 配置一个新环境。

```bash
conda create -p <path>/conda_env/llamafactory python=3.10
```

### 安装源码

```bash
conda activate <path>/conda_env/llamafactory
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
```

### LLaMABoard 可视化微调（由 [Gradio](https://github.com/gradio-app/gradio) 驱动）

```bash
llamafactory-cli webui
```

## 开始微调-UI

再微调前，你需要准备自己的数据，数据格式按照 `LLaMA-Factory` 的文档准备，我们提供了一个长文本论文生成的数据集，可以使用此数据微调，需要注意由于这个输入单个文档长度很高，需要 `cutoff_len` 大小是 21000，显存大小 80G 的设备才可以微调。

### `nlp_paper_inst` 数据集

```json
{
  "nlp_paper_inst": {
    "file_name": "nlp_paper_inst.json",
    "columns": {
      "prompt": "prompt",
      "response": "response",
      "system": "system"
    }
  }
}
```

### 加载模型

![](img/91a39efded9a801f522d6da38a161922.png)

### 配置训练模板

![](img/3190a9a135cd519a9f28f9831a81f3e4.png)

### 选择准备的数据

![](img/80879f46f365a0e04c329d0fc3035652.png)

### 配置 `Pack`

![](img/962dd52fdd672325a2a7a17a584b2b22.png)

### `Lora`

![](img/4eba1dc8387f67f3386ab5e998a3e723.png)

### 预览训练命令

![](img/324c8c0c070ee2d35587168a9eaa2019.png)

### 微调

![](img/2d67d53c03a928b2f680773ed828b51f.png)

#### 开始微调-BASH

在 `LLaMA-Factory` 仓库上，`examples/train_lora` 目录下有个执行脚本，放在源码目录下方，设置好环境即可完成微调。

`start_glm_train.sh`

```bash
CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 WANDB_API_KEY=974207f7173417ef95d2ebad4cbe7f2f9668a093 llamafactory-cli train \
    --stage sft \
    --do_train True \
    --model_name_or_path /mnt/ceph/develop/jiawei/model_checkpoint/LongWriter-glm4-9b-base \
    --preprocessing_num_workers 1 \
    --finetuning_type lora \
    --template glm4 \
    --flash_attn auto \
    --dataset_dir data \
    --dataset nlp_paper_inst \
    --cutoff_len 21000 \
    --learning_rate 2e-5 \
    --num_train_epochs 1.0 \
    --max_samples 100000 \
    --per_device_train_batch_size 1 \
    --gradient_accumulation_steps 2 \
    --lr_scheduler_type cosine \
    --max_grad_norm 1.0 \
    --logging_steps 1 \
    --save_steps 1 \
    --warmup_steps 0 \
    --warmup_ratio 0.03 \
    --weight_decay 0.1 \
    --optim adamw_torch \
    --packing True \
    --report_to wandb \
    --run_name LongWriter-test4 \
    --output_dir saves/LongWriter-glm4-9b-base/lora/train_2024-11-01-test4 \
    --bf16 True \
    --plot_loss True \
    --ddp_timeout 180000000 \
    --include_num_input_tokens_seen True \
    --lora_rank 8 \
    --lora_alpha 32 \
    --lora_dropout 0.05 \
    --lora_target all
```

## 测试对话

完成微调训练后，选择一个模型，和刚才的 `lora` 结果，点击 `chat` 对话，加载模型即可对话。

![](img/15b0de022c3505d5369c8a03835ae5d6.png)

记得配置对话模板。

![](img/b96f9fe55a7b97a1b66544c4e7f6cab7.png)
