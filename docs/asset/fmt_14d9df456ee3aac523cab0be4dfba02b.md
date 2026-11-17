
```bash
PRE_SEQ_LEN=128
LR=2e-2
NUM_GPUS=1
torchrun --standalone --nnodes=1 --nproc-per-node=$NUM_GPUS main.py \
    --do_train \
    --train_file AdvertiseGen/train.json \    #训练文件
    --validation_file AdvertiseGen/dev.json \ #验证文件
    --preprocessing_num_workers 10 \
    --prompt_column content \
    --response_column summary \
    --overwrite_cache \
    --model_name_or_path /mnt/workspace/chatglm2/models/chatglm2-6b \ #修改模型路径
    --output_dir output/adgen-chatglm2-6b-pt-$PRE_SEQ_LEN-$LR \
    --overwrite_output_dir \
    --max_source_length 128 \
    --max_target_length 128 \
    --per_device_train_batch_size 1 \
    --per_device_eval_batch_size 1 \
    --gradient_accumulation_steps 16 \
    --predict_with_generate \
    --max_steps 3000 \
    --logging_steps 10 \
    --save_steps 1000 \
    --learning_rate $LR \
    --pre_seq_len $PRE_SEQ_LEN
    --quantization_bit 4
```

> `train.sh` 中的 `PRE_SEQ_LEN` 和 `LR` 分别是 soft prompt 长度和训练的学习率，可以进行调节以取得最佳的效果。

> `P-Tuning-v2` 方法会冻结全部的模型参数，可通过调整 `quantization_bit` 来改变原始模型的量化等级，`quantization_bit 4` 是 `INT4` 量化版，不加此选项则为 `FP16` 精度加载。

> `CHECKPOINT` 实际就是 `train.sh` 中的 `output_dir`。

### 开始训练

```bash
bash train.sh
```

这个时候才发现和训练数据的大小无关，自定义20条数据一样也是预计11个小时(￣ェ￣;)

`Int4` 量化版运行占用 `GPU` 显存为：`7945 MiB`，3000 个 `steps` 整体需要 4 小时。

运行日志如下

![](img/dedd8f2c3f1ee2ff8fa4f1f102b75eed.png)

运行完成后，生成的模型位于：

`ChatGLM2-6B/ptuning/output/adgen-chatglm2-6b-pt-128-2e-2/checkpoint-3000` 下

查看模型大小

```bash
du -sh output/
```

模型比较：

`Chatglm2` 的大模型约为 `12G`，而微调模型约为 `7M`。

### 4. 推理

+ `evaluate.sh` 文件修改

依然修改一下 `evaluate.sh`，我需要修改的是 Python 版本和关闭 `quantization_bit`。

```bash
PRE_SEQ_LEN=128
CHECKPOINT=adgen-chatglm2-6b-pt-pt-128-2e-2
STEP=3000
NUM_GPUS=1
torchrun --standalone --nnodes=1 --nproc-per-node=$NUM_GPUS main.py \
    --do_predict \
    --validation_file AdvertiseGen/dev.json \   #验证数据集
    --test_file  AdvertiseGen/dev.json \        #测试数据集
    --overwrite_cache \
    --prompt_column content \
    --response_column summary \
    --model_name_or_path /mnt/workspace/chatglm2/models/chatglm2-6b \ 
    --ptuning_checkpoint ./output/$CHECKPOINT/checkpoint-$STEP \  #./output/$CHECKPOINT/checkpoint-$STEP  \
    --output_dir ./output/$CHECKPOINT \                #./output/$CHECKPOINT \
    --overwrite_output_dir \
    --max_source_length 128 \
    --max_target_length 128 \
    --per_device_eval_batch_size 1 \
    --predict_with_generate \
    --pre_seq_len $PRE_SEQ_LEN
    --quantization_bit 4
```

```bash
bash evaluate.sh
```

执行完成后，会生成评测文件，评测指标为中文 Rouge score 和 BLEU-4。生成的结果保存在 `./output/adgen-chatglm2-6b-pt-128-2e-2/generated_predictions.txt`。我们准备了 5 条推理数据，所以相应的在文件中会有 5 条评测数据，`labels` 是 `dev.json` 中的预测输出，`predict` 是 `ChatGLM2-6B` 生成的结果，对比预测输出和生成结果，评测模型训练的好坏。如果不满意调整训练的参数再次进行训练。

### 5. 模型部署

+ 直接运行代码：

首先载入 `Tokenizer`：

```python
from transformers import AutoTokenizer, AutoModel, AutoConfig

model_path = "/data/work/xiehao/chatglm2-6b-model" #或者远程 “THUDM/chatglm2-6b”
# 载入Tokenizer
#tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True)
tokenizer = AutoTokenizer.from_pretrained(model_path, trust_remote_code=True)
```

1. 如果需要加载的是新 Checkpoint（只包含 `PrefixEncoder` 参数）：

```python
# 微调前

#model = AutoModel.from_pretrained(model_path, trust_remote_code=True, device='cuda')

#model = model.eval()

#response, history = model.chat(tokenizer, chat_str, history=[])

#print(response)



# 微调后

#通过AutoConfig读取model_path目录下的config.json的参数信息
#config = AutoConfig.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True, pre_seq_len=128)
#model = AutoModel.from_pretrained("THUDM/chatglm-6b", config=config, trust_remote_code=True)
config = AutoConfig.from_pretrained(model_path, trust_remote_code=True, pre_seq_len=128)
model = AutoModel.from_pretrained(model_path, config=config, trust_remote_code=True)

#prefix_state_dict = torch.load(lora_model_path)
prefix_state_dict = torch.load(os.path.join(CHECKPOINT_PATH, "pytorch_model.bin"))

new_prefix_state_dict = {}
for k, v in prefix_state_dict.items():
    if k.startswith("transformer.prefix_encoder."):
        new_prefix_state_dict[k[len("transformer.prefix_encoder."):]] = v
model.transformer.prefix_encoder.load_state_dict(new_prefix_state_dict)
```

# 注意你可能需要将 `pre_seq_len` 改成你训练时的实际值。如果你是从本地加载模型的话，需要将 `THUDM/chatglm-6b` 改成本地的模型路径（注意不是 `checkpoint` 路径）

1. 如果需要加载的是旧 Checkpoint（包含 `ChatGLM-6B` 以及 `PrefixEncoder` 参数），或者进行的是全参数微调，则直接加载整个 Checkpoint：

```python
model = AutoModel.from_pretrained(CHECKPOINT_PATH, trust_remote_code=True)
```

之后根据需求可以进行量化，也可以直接使用：

```python
# Comment out the following line if you don't use quantization
model = model.quantize(4)
model = model.half().cuda()
model.transformer.prefix_encoder.float()
model = model.eval()
response, history = model.chat(tokenizer, chat_str, history=[])
print(response)
```

+ 也可以直接运行支持加载 [P-Tuning v2 checkpoint](https://github.com/THUDM/ChatGLM-6B/blob/main/ptuning/web_demo.py) 的 web demo

这里我们先修改 `web_demo.sh` 的内容以符合实际情况，将 `pre_seq_len` 改成你训练时的实际值，将 `THUDM/chatglm2-6b` 改成本地的模型路径。

`web_demo.sh` 文件

```bash
PRE_SEQ_LEN=128
  
CUDA_VISIBLE_DEVICES=0 python3 web_demo.py \
    --model_name_or_path /mnt/workspace/chatglm2/models/chatglm2-6b \
    --ptuning_checkpoint output/adgen-chatglm2-6b-pt-128-2e-2/checkpoint-3000 \
    --pre_seq_len $PRE_SEQ_LEN
```

然后再执行

```bash
#如果没有安装gradio需要安装
pip install gradio
#需要安装mdtex2html
pip install mdtex2html

#执行微调模型
bash web_demo.sh
```

![](img/5a3b44511ff776a89f6ec27e7a373a89.png)

---

# 单卡微调 ChatGLM P-tuning + LoRA

### 硬件环境

单张 `RTX 3090`

### 系统环境

`ubuntu 20.04`

### 相关资源

[THUDM/ChatGLM-6B: ChatGLM-6B: An Open Bilingual Dialogue Language Model | 开源双语对话语言模型](https://github.com/THUDM/ChatGLM-6B)

THUDM/chatglm-6b · Hugging Face

[mymusise/ChatGLM-Tuning: 一种平价的chatgpt实现方案, 基于ChatGLM-6B + LoRA](https://github.com/mymusise/ChatGLM-Tuning)

### 操作流程

#### P-tuning

1. 下载项目和模型

ChatGLM 官方提供了开包即食的 P-tuning 实现

```bash
# 克隆ChatGLM-6B项目
git clone https://github.com/THUDM/ChatGLM-6B.git
cd ChatGLM-6B

# 下载checkpoint
git lfs install
git clone https://huggingface.co/THUDM/chatglm-6b

cd ../ptuning
```

这里以训练对话为例，修改 `ptuning` 目录下的 `train_chat.sh`

```bash
PRE_SEQ_LEN=128 # prefix 长度
LR=1e-2         # 学习率

CHAT_TRAIN_DATA="path/to/train_data.json"        # train 数据 
CHAT_VAL_DATA="path/to/val_data.json"            # val 数据
CHECKPOINT_NAME="output/pt-$PRE_SEQ_LEN-$LR"     # 保存 checkpoint 路径

CUDA_VISIBLE_DEVICES=0 python3 main.py \
    --do_train \
    --train_file $CHAT_TRAIN_DATA \
    --validation_file $CHAT_VAL_DATA \
    --prompt_column prompt \    
    --response_column response \
    --history_column history \
    --overwrite_cache \
    --model_name_or_path ../chatglm-6b \         # 载入 checkpoint 路径
    --output_dir $CHECKPOINT_NAME \
    --overwrite_output_dir \
    --max_source_length 256 \
    --max_target_length 256 \
    --per_device_train_batch_size 1 \
    --per_device_eval_batch_size 1 \
    --gradient_accumulation_steps 16 \
    --predict_with_generate \
    --max_steps 3000 \
    --logging_steps 10 \
    --save_steps 1000 \
    --learning_rate $LR \
    --pre_seq_len $PRE_SEQ_LEN \
    --quantization_bit 4
```

#### 对话数据集

需要注意的是，多轮对话需要提前处理成 `ChatGLM` 的格式，参考 `ChatGLM-6B/ptuning` 对话数据集
