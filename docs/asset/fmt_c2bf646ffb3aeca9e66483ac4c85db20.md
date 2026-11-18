
# L1G4- InternLM 论文分类微调实践（Swift 版）

> 来源：[https://aicarrier.feishu.cn/docx/GCxbdpzOooDbe2xXACZcZAORnFh](https://aicarrier.feishu.cn/docx/GCxbdpzOooDbe2xXACZcZAORnFh)

> 文档尚处于草稿阶段，完善中

![](img/5d1eef3ed0856c71a650a036e8ea5353.png)

屏幕前的吴彦祖、刘亦菲们好，欢迎参加书生实战营，这个文档将带大家微调 InternLM 模型的「论文分类」能力，并带大家参加论文分类比赛，狠狠赚赞助商一笔。

## 环境安装

在[创建开发机界面](https://studio.intern-ai.org.cn/console/instance)选择镜像为 `Cuda12.2-conda`，并选择 GPU 为30% A100。

![](img/61dab606ae070b18e7706e0c34ebdb1c.png)

安装 `ms-swift`

```
conda create -n ms-swift python=3.10 -y
conda activate ms-swift
pip install ms-swift -U
pip install wandb
```

注意：后续所有的操作都需要在 `ms-swift` 的虚拟环境中执行，在进入新终端时需要先执行 `conda activate ms-swift`

## 训练

本节为大家提供了处理好的 [arxiv 数据集](https://www.kaggle.com/datasets/Cornell-University/arxiv)和采集得到的数据。

训练集下载链接：https://www.modelscope.cn/datasets/JimmyMa99/smartflow-arxiv-dataset/files

```
conda activate ms-swift
pip install modelscope
modelscope download --dataset JimmyMa99/smartflow-arxiv-dataset --local_dir ./datasets/train
```

### 预训练

#### 使用 `lora` 进行预训练

如直接复制请删除注释、更改模型路径与数据集路径

```
conda activate ms-swift
bash config/internlm3-8b-lora.sh
```

脚本解析：

```
#!/bin/bash

# 创建日志目录
LOG_DIR="logs"
mkdir -p $LOG_DIR  # 确保日志目录存在，如果不存在则创建

# 获取当前时间戳，用于生成唯一的日志文件名
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
LOG_FILE="$LOG_DIR/internlm3-8b_lora_sft_${TIMESTAMP}.log"  # 设置日志文件路径

# 设置CUDA环境变量
export NPROC_PER_NODE=1  # 设置每个节点使用的进程数为1
export OMP_NUM_THREADS=1  # 限制OpenMP线程数为1，避免过多线程竞争
export CUDA_VISIBLE_DEVICES=0  # 指定使用的GPU编号为0
export MASTER_PORT=$((10000 + RANDOM % 50000))

# 使用nohup命令在后台运行训练任务，即使终端关闭也能继续运行
nohup swift sft \
    --model {model_path} \  # 指定基础模型路径
    --train_type lora \  # 使用LoRA训练方法
    --dataset {data_path} \  # 指定训练数据集
    --torch_dtype bfloat16 \  # 使用bfloat16精度以节省显存
    --num_train_epochs 1 \  # 设置训练轮数为2
    --per_device_train_batch_size 2 \  # 每个设备的训练批次大小为4
    --learning_rate 5e-5 \  # 学习率设置为5e-5
    --warmup_ratio 0.1 \  # 预热阶段占总训练步数的10%
    --split_dataset_ratio 0 \  # 不拆分数据集
    --lora_rank 8 \  # LoRA的秩设置为8
    --lora_alpha 32 \  # LoRA的alpha参数设置为32
    --use_chat_template false \  # 不使用聊天模板
    --target_modules all-linear \  # 对所有线性层应用LoRA
    --gradient_accumulation_steps 2 \  # 梯度累积步数为2，用于增大有效批次大小
    --save_steps 2000 \  # 每2000步保存一次模型
    --save_total_limit 5 \  # 最多保存5个检查点
    --gradient_checkpointing_kwargs '{"use_reentrant": false}' \  # 梯度检查点设置，禁用重入
    --logging_steps 5 \  # 每5步记录一次日志
    --max_length 2048 \  # 最大序列长度设为2048
    --output_dir ./swift_output/InternLM3-8B-Lora \  # 输出目录
    --dataloader_num_workers 256 \  # 数据加载器使用256个工作线程
    --model_author JimmyMa99 \  # 模型作者信息
    --model_name InternLM3-8B-Lora \  # 模型名称
    > "$LOG_FILE" 2>&1 &  # 将标准输出和错误输出重定向到日志文件，并在后台运行

# 打印进程ID和日志文件位置，便于用户跟踪
echo "Training started with PID $!"  # 显示后台进程的PID
echo "Log file: $LOG_FILE"  # 显示日志文件位置

# 提示用户如何实时查看日志
echo "To view logs in real-time, use:"
echo "tail -f $LOG_FILE"
```

#### 合并权重

将刚刚跑完的 log 中显示的权重保存路径修改为以下 adapters 的参数

```
conda activate ms-swift
swift export --adapters xx/checkpoint-xxx --merge_lora true
```

### SFT

#### 使用 `lora` 进行SFT

如直接复制请删除注释、更改模型路径与数据集路径

```
conda activate ms-swift
bash config/internlm3-8b-sft-lora.sh
```

脚本解析：

```
#!/bin/bash
# 指定使用bash解释器执行脚本

# 创建日志目录
LOG_DIR="logs"
# 定义日志存储目录变量
mkdir -p $LOG_DIR
# 创建日志目录，-p参数确保即使目录已存在也不会报错

# 获取当前时间戳
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
# 获取当前时间并格式化为年月日_时分秒格式
LOG_FILE="$LOG_DIR/internlm3-8b_lora_sft_${TIMESTAMP}.log"
# 组合日志文件路径，使用时间戳确保文件名唯一

# 设置CUDA设备
export NPROC_PER_NODE=1
# 设置每个节点的进程数为1
export OMP_NUM_THREADS=1
# 设置OpenMP线程数为1，限制并行线程数
export CUDA_VISIBLE_DEVICES=0
# 指定使用的GPU设备为0号设备

nohup swift sft \
# 使用nohup命令在后台运行swift sft命令，即使终端关闭也能继续运行
    --model /root/code/camp5_course/swift_output/InternLM3-8B-Lora/v1-20250416-140542/checkpoint-74-merged \
    # 指定基础模型路径，使用之前训练的checkpoint-74的合并模型
    --train_type lora \
    # 设置训练类型为LoRA（低秩适应）
    --dataset '/root/code/camp5_course/data/swift_formatted_sft_train_data.jsonl' \
    # 指定训练数据集的路径
    --torch_dtype bfloat16 \
    # 设置模型参数的数据类型为bfloat16，减少内存占用
    --num_train_epochs 1 \
    # 设置训练轮数为5轮
    --per_device_train_batch_size 22 \
    # 设置每个设备的训练批次大小为8
    --learning_rate 1e-4 \
    # 设置学习率为0.0001
    --warmup_ratio 0.1 \
    # 设置预热比例为0.1，即10%的训练步骤用于学习率从小到大的预热
    --split_dataset_ratio 0 \
    # 设置数据集分割比例为0，不进行训练/验证分割
    --lora_rank 8 \
    # 设置LoRA的秩为8，控制可训练参数的数量
    --lora_alpha 32 \
    # 设置LoRA的alpha为32，影响LoRA更新的缩放程度
    --target_modules all-linear \
    # 设置LoRA目标模块为所有线性层
    --gradient_accumulation_steps 2 \
    # 设置梯度累积步数为2，相当于扩大了批次大小
    --save_steps 2000 \
    # 每2000步保存一次检查点
    --save_total_limit 5 \
    # 最多保存5个检查点，超过会删除旧的
    --gradient_checkpointing_kwargs '{"use_reentrant": false}' \
    # 设置梯度检查点的参数，关闭重入功能以提高稳定性
    --logging_steps 5 \
    # 每5步记录一次日志
    --max_length 2048 \
    # 设置最大序列长度为2048
    --output_dir ./swift_output/InternLM3-8B-Lora \
    # 设置输出目录
    --dataloader_num_workers 256 \
    # 设置数据加载器的工作进程数为256，加速数据加载
    --model_author JimmyMa99 \
    # 设置模型作者信息
    --model_name InternLM3-8B-Lora \
    # 设置模型名称
    > "$LOG_FILE" 2>&1 &
    # 将标准输出和标准错误重定向到日志文件，并在后台运行

# 打印进程ID和日志文件位置
echo "Training started with PID $!"
# 显示训练进程的PID（$!代表最近一个后台进程的PID）
echo "Log file: $LOG_FILE"
# 显示日志文件的路径

# 显示查看日志的命令
echo "To view logs in real-time, use:"
echo "tail -f $LOG_FILE"
# 提示用户如何实时查看日志文件内容
```

#### 合并权重

```
conda activate ms-swift
swift export --adapters xx/checkpoint-xxx --merge_lora true
```

## 推理测试

### 新建 `streamlit` 代码
