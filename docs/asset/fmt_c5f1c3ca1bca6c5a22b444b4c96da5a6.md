
# 训练参数配置与环境设置

## 环境变量与基础配置

```bash
#!/bin/bash
############################################################################
########################### Editable Part Begins ###########################
############################################################################

############# 多卡的情况下需要开启，如果是单卡需要关闭！！！！
############# 多卡的情况下需要开启，如果是单卡需要关闭！！！！
############# 多卡的情况下需要开启，如果是单卡需要关闭！！！！
export CUDA_VISIBLE_DEVICES=0
export NCCL_P2P_DISABLE=1
export NCCL_IB_DISABLE=1
############# 多卡的情况下需要开启，如果是单卡需要关闭！！！！
############# 多卡的情况下需要开启，如果是单卡需要关闭！！！！
############# 多卡的情况下需要开启，如果是单卡需要关闭！！！！

# exp meta information
EXP_NAME=default
PRETRAIN_DATASET=../output/image_captioning_output/res_10k.jsonl
PRETRAIN_DATASET_IMAGE_PATH=../input/pretrain_stage_1_10k

# training args
# pretraining
# make sure PRETRAIN_BATCH_SIZE_PER_GPU * PRETRAIN_GRADIENT_ACCUMULATION_STEPS * num_gpus = 256
# **NOTICE**: the default setting is for 1 GPU
PRETRAIN_BATCH_SIZE_PER_GPU=4
PRETRAIN_GRADIENT_ACCUMULATION_STEPS=64
PRETRAIN_DATALOADER_NUM_WORKERS=4
# finetuning
# make sure FINETUNE_BATCH_SIZE_PER_GPU * FINETUNE_GRADIENT_ACCUMULATION_STEPS * num_gpus = 128
# **NOTICE**: the default setting is for 1 GPU
FINETUNE_BATCH_SIZE_PER_GPU=4
FINETUNE_GRADIENT_ACCUMULATION_STEPS=32
FINETUNE_DATALOADER_NUM_WORKERS=4
# log and ckpt
LOGGING_STEP=1
CKPT_SAVE_STEPS=100
TOTAL_SAVE_CKPT_LIMIT=1

# inference args
# inference for some benchmarks supports multi-gpus
INFER_CUDA_IDX="0"
```

## 关键训练参数说明

此时我们需要注意几个关键的训练参数配置，分别是`pretrain`以及`finetune`，这是全部环节中最重要的两个阶段。如果没有成功运行这两个训练环节，一切都将是徒劳。

👆我已经把所有能加的强调符号都加上了，希望大家能够记住这个关键因素，许多同学最后无法提交的核心根本原因就在此。而其中对应的相关配置是

```bash
# **NOTICE**: the default setting is for 1 GPU
PRETRAIN_BATCH_SIZE_PER_GPU=4
PRETRAIN_GRADIENT_ACCUMULATION_STEPS=64
PRETRAIN_DATALOADER_NUM_WORKERS=4
# finetuning
# make sure FINETUNE_BATCH_SIZE_PER_GPU * FINETUNE_GRADIENT_ACCUMULATION_STEPS * num_gpus = 128
# **NOTICE**: the default setting is for 1 GPU
FINETUNE_BATCH_SIZE_PER_GPU=4
FINETUNE_GRADIENT_ACCUMULATION_STEPS=32
FINETUNE_DATALOADER_NUM_WORKERS=4
```

## 批量大小与梯度累积计算

我们需要满足两个关系，也就是 `4 * 64 = 256`，以及 `4 * 32 = 128`，这是必须满足的数量关系，最后的乘法结果需要满足 256 以及 128，如果你想要真正的多卡并行训练，则需要自行除以二；

如果我们想降低显存和内存占用，最好的想法当然是强行修改 `FINETUNE_BATCH_SIZE_PER_GPU` 为 1，并且把 `FINETUNE_DATALOADER_NUM_WORKERS` 变成 1，并且关闭后续的 `batchsize` 检查：

但是讲规矩的我们是不会这么做的（当然也可以不这么做，快速跑通先让自己开心一下）

所以我们决定缩小 `batchsize`，增大梯度累积，这样就可以在一张 24G 显存的显卡上跑通全过程（`finetune` 需要占据的显存要求比较高）：

```bash
# check the global size
PRETRAIN_PASS=`python $SCRIPT_DIR/training/preprocess/check_global_batch_size.py $PRETRAIN_BATCH_SIZE_PER_GPU $PRETRAIN_GRADIENT_ACCUMULATION_STEPS 256`
if [ "$PRETRAIN_PASS" = "False" ]; then
    echo "[ERROR] The global batch size of pretraining stage is not 256! Please check and retry."
    exit
fi
FINETUNE_PASS=`python $SCRIPT_DIR/training/preprocess/check_global_batch_size.py $FINETUNE_BATCH_SIZE_PER_GPU $FINETUNE_GRADIENT_ACCUMULATION_STEPS 128`
if [ "$FINETUNE_PASS" = "False" ]; then
    echo "[ERROR] The global batch size of finetuning stage is not 128! Please check and retry."
    exit
fi
```

至此，如果你是双卡战士（内存合计大于 120G 甚至更高），已经可以很愉快的跑通 `pretrain` 和 `finetune` 环节，跑不通可以找我退钱

你必须看到 `pretrain` 和 `finetune` 的环节经过了完整的训练，其中训练时间如果快使用 `A100` 大概需要 半个小时（`pretrain`）一个半小时（`finetune`）；具体时间取决于你的机器，但请你保证一定要完成训练。

其中 `finetune` 训练过程如图：（`pretrain` 也差不多，总之。。。你需要看到 `loss` 的正常训练过程）

![](img/0cf868d296bd0d3cc06c3252766952b1.png)

## 单卡显存优化配置

```bash
PRETRAIN_BATCH_SIZE_PER_GPU=4
PRETRAIN_GRADIENT_ACCUMULATION_STEPS=64
PRETRAIN_DATALOADER_NUM_WORKERS=4
# finetuning
# make sure FINETUNE_BATCH_SIZE_PER_GPU * FINETUNE_GRADIENT_ACCUMULATION_STEPS * num_gpus = 128
# **NOTICE**: the default setting is for 1 GPU
FINETUNE_BATCH_SIZE_PER_GPU=2
FINETUNE_GRADIENT_ACCUMULATION_STEPS=64
FINETUNE_DATALOADER_NUM_WORKERS=4
```

## 训练执行与时间预估

前面都是一些简单的准备工作，接下来让我们直接开始进入到真正的训练脚本，首先是 `pretrain` 阶段：

```bash
mkdir -p $SCRIPT_DIR/../output/training_dirs/$PRETRAIN_NAME

deepspeed $SCRIPT_DIR/training/mgm/train/train_mem.py \
    --deepspeed $SCRIPT_DIR/training/scripts/zero2_offload.json \
    --model_name_or_path $SCRIPT_DIR/training/model_zoo/LLM/gemma/gemma-2b-it \
    --version gemma \
    --data_path $PRETRAIN_DATASET_JSON \
    --image_folder $PRETRAIN_DATASET_IMAGE_PATH \
    --vision_tower $SCRIPT_DIR/training/model_zoo/OpenAI/clip-vit-large-patch14-336 \
    --vision_tower_aux $SCRIPT_DIR/training/model_zoo/OpenAI/openclip-convnext-large-d-320-laion2B-s29B-b131K-ft-soup \
    --mm_projector_type mlp2x_gelu \
    --tune_mm_mlp_adapter True \
    --mm_vision_select_layer -2 \
    --mm_use_im_start_end False \
    --mm_use_im_patch_token False \
    --image_size_aux $AUX_SIZE \
    --bf16 True \
    --output_dir $SCRIPT_DIR/../output/training_dirs/$PRETRAIN_NAME \
    --num_train_epochs $NUM_TRAIN_EPOCHS \
    --per_device_train_batch_size $PRETRAIN_BATCH_SIZE_PER_GPU \
    --per_device_eval_batch_size 4 \
    --gradient_accumulation_steps $PRETRAIN_GRADIENT_ACCUMULATION_STEPS \
    --evaluation_strategy "no" \
    --save_strategy "steps" \
    --save_steps $CKPT_SAVE_STEPS \
    --save_total_limit $TOTAL_SAVE_CKPT_LIMIT \
    --learning_rate 1e-3 \
    --weight_decay 0. \
    --warmup_ratio 0.03 \
    --lr_scheduler_type "cosine" \
    --logging_steps $LOGGING_STEP \
    --tf32 True \
    --model_max_length 2048 \
    --gradient_checkpointing True \
    --dataloader_num_workers $PRETRAIN_DATALOADER_NUM_WORKERS \
    --lazy_preprocess True \
    --report_to none \
    2>&1 | tee $SCRIPT_DIR/../output/training_dirs/$PRETRAIN_NAME/pretrain.log
```

这里我们使用的就是 `mgm` 框架在进行微调，我们可以看到其中的详细参数以及数据集的地址，包括使用的是 `deepseed`（使用的是 `zero2` 的优化策略，使用内存换显存，这也是为什么内存需求比较大）我们可以看到其中前面提到的大语言模型以及对应的视觉编码器。

```bash
mkdir -p $SCRIPT_DIR/../output/training_dirs/$FINETUNE_NAME

deepspeed $SCRIPT_DIR/training/mgm/train/train_mem.py \
    --deepspeed $SCRIPT_DIR/training/scripts/zero2_offload.json \
    --model_name_or_path $SCRIPT_DIR/training/model_zoo/LLM/gemma/gemma-2b-it \
    --version gemma \
    --data_path $SCRIPT_DIR/training/data/finetuning_stage_1_12k/mgm_instruction_stage_1_12k.json \
    --image_folder $SCRIPT_DIR/training/data/finetuning_stage_1_12k \
    --vision_tower $SCRIPT_DIR/training/model_zoo/OpenAI/clip-vit-large-patch14-336 \
    --vision_tower_aux $SCRIPT_DIR/training/model_zoo/OpenAI/openclip-convnext-large-d-320-laion2B-s29B-b131K-ft-soup \
    --pretrain_mm_mlp_adapter $SCRIPT_DIR/../output/training_dirs/$PRETRAIN_NAME/mm_projector.bin \
    --mm_projector_type mlp2x_gelu \
    --mm_vision_select_layer -2 \
    --mm_use_im_start_end False \
    --mm_use_im_patch_token False \
    --image_aspect_ratio pad \
    --group_by_modality_length True \
    --image_size_aux $AUX_SIZE \
    --bf16 True \
    --output_dir $SCRIPT_DIR/../output/training_dirs/$FINETUNE_NAME \
    --num_train_epochs $NUM_TRAIN_EPOCHS \
    --per_device_train_batch_size $FINETUNE_BATCH_SIZE_PER_GPU \
    --per_device_eval_batch_size 4 \
    --gradient_accumulation_steps $FINETUNE_GRADIENT_ACCUMULATION_STEPS \
    --evaluation_strategy "no" \
    --save_strategy "steps" \
    --save_steps $CKPT_SAVE_STEPS \
    --save_total_limit $TOTAL_SAVE_CKPT_LIMIT \
    --learning_rate 2e-5 \
    --weight_decay 0. \
    --warmup_ratio 0.03 \
    --lr_scheduler_type "cosine" \
    --logging_steps $LOGGING_STEP \
    --tf32 True \
    --model_max_length 2048 \
    --gradient_checkpointing True \
    --dataloader_num_workers $FINETUNE_DATALOADER_NUM_WORKERS \
    --lazy_preprocess True \
    --report_to none \
    2>&1 | tee $SCRIPT_DIR/../output/training_dirs/$FINETUNE_NAME/finetuning.log
```

对于 `finetune` 阶段，其实大家已经很熟悉了，主要就是多了个 `pretrain_mm_mlp_adapter` 以及一些学习参数的不同。
