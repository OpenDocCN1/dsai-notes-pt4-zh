
## 数据预处理

```python
import json
import os

def to_abs(base_path, path):
    if not path or os.path.isabs(path):
        return path
    abs_path = os.path.join(base_path, path)
    if not os.path.exists(abs_path):
        raise FileNotFoundError(f"图像文件不存在: {abs_path}")
    return abs_path

def process_jsonl(input_path, output_path, base_dir):
    processed_lines = 0
    modified_lines = 0

    with open(input_path, 'r', encoding='utf-8') as infile, \
         open(output_path, 'w', encoding='utf-8') as outfile:

        for line in infile:
            if not line.strip():
                continue

            data = json.loads(line)
            changed = False

            # 处理顶层 images 列表
            if 'images' in data and isinstance(data['images'], list):
                data['images'] = [to_abs(base_dir, img) for img in data['images']]
                changed = True

            # 处理 messages 中的 content
            if 'messages' in data:
                for msg in data['messages']:
                    # 转换 role -> from
                    if 'role' in msg:
                        role = msg.pop('role')
                        msg['from'] = 'human' if role == 'user' else 'gpt'
                    
                    # 转换 content -> value，并确保保留 <image> 标签
                    if 'content' in msg:
                        content = msg.pop('content')
                        # 如果 content 是字符串且包含 <image>，保留标签
                        if isinstance(content, str) and '<image>' in content:
                            msg['value'] = content  # 直接保留原始字符串
                        else:
                            msg['value'] = content  # 其他情况直接赋值
                        changed = True

            # 写入数据
            outfile.write(json.dumps(data, ensure_ascii=False) + '\n')
            processed_lines += 1
            if changed:
                modified_lines += 1

    print(f"处理完成：总行数={processed_lines}，修改行数={modified_lines}，输出路径：{output_path}")

# 配置路径
INPUT_PATH = "/root/dataset/VLM-formula-recognition-dataset_intern_camp/train/train_mini.jsonl"
OUTPUT_PATH = "/root/dataset/VLM-formula-recognition-dataset_intern_camp/train/train_mini_lf.jsonl"
BASE_DIR = "/root/dataset/VLM-formula-recognition-dataset_intern_camp/train/"

# 执行处理
process_jsonl(INPUT_PATH, OUTPUT_PATH, BASE_DIR)
```

### 执行

```bash
python convert.py
```

## 创建 dataset_info.json 文件

创建 `dataset_info.json` 文件

```bash
mkdir /root/llamafactory_workdir/data # 如果上一个例子已经转换过可以跳过这一步
cd /root/llamafactory_workdir/data
touch dataset_info.json
```

`dataset_info.json` 文件内容如下：

```json
{
    "VLM-formula-recognition": {
        "file_name": "/root/dataset/VLM-formula-recognition-dataset_intern_camp/train/train_mini_lf.jsonl",
        "formatting": "sharegpt",
        "columns": {
            "messages": "messages",
            "images": "images"
        }
    }
}
```

## Yaml 配置文件

创建 `internvl3_5_1B_lora_sft_ds.yaml` 文件

```bash
mkdir /root/llamafactory_workdir
cd /root/llamafactory_workdir
touch internvl3_5_1B_lora_sft_ds.yaml
```

`internvl3_5_1B_lora_sft_ds.yaml` 文件内容如下

```yaml
### model
model_name_or_path: /share/new_models/InternVL3.5/InternVL3_5-1B-HF
image_max_pixels: 262144
video_max_pixels: 16384
trust_remote_code: true

### method
stage: sft
do_train: true
finetuning_type: lora
lora_rank: 8
lora_target: all

### dataset
dataset: VLM-formula-recognition  # video: mllm_video_demo
template: intern_vl
cutoff_len: 2048
max_samples: 8000
overwrite_cache: true
preprocessing_num_workers: 1
dataloader_num_workers: 4

### output
output_dir: saves/InternVL3_5-1B/lora/sft
logging_steps: 10
save_steps: 2000
plot_loss: true
overwrite_output_dir: true
save_only_model: false
report_to: none  # choices: [none, wandb, tensorboard, swanlab, mlflow]

### train
per_device_train_batch_size: 1
gradient_accumulation_steps: 8
learning_rate: 1.0e-6
num_train_epochs: 3.0
lr_scheduler_type: cosine
warmup_ratio: 0.1
bf16: true
ddp_timeout: 180000000
resume_from_checkpoint: null

### eval
val_size: 0.1
per_device_eval_batch_size: 1
eval_strategy: steps
eval_steps: 2000
```

## 安装依赖

```bash
pip install transformers==4.52.1
```

## 开启微调

```bash
torchrun --nproc_per_node 1 \
    --nnodes 1 \
    --node_rank 0 \
    --master_addr 127.0.0.1 \
    --master_port 7007 \
    /root/LLaMA-Factory/src/train.py internvl3_5_1B_lora_sft_ds.yaml
```

## 模型合并导出

LoRA 模型合并和导出时，可通过指定 `export_device` 参数为 `auto` 来自动检测当前加速卡环境，启用 NPU 作为导出设备：

```bash
ASCEND_RT_VISIBLE_DEVICES=0 
llamafactory-cli export \
            --model_name_or_path /share/new_models/InternVL3.5/InternVL3_5-1B-HF \
            --adapter_name_or_path saves/InternVL3_5-1B/lora/sft  \
            --template intern_vl \
            --finetuning_type lora \
            --export_dir saves/InternVL3_5-1B/lora/megred-model-path \
            --export_device auto \
            --export_legacy_format False \
            --trust_remote_code True
```

# 参考文档

https://ascend.github.io/docs/index.html

https://ascend.github.io/docs/sources/llamafactory/index.html
