
# Ascend NPU 下 LlamaFactory 训练书生大模型实践

> 来源：[https://aicarrier.feishu.cn/docx/BiaTdcO6tolU7lx9d8ocYg3xnPh](https://aicarrier.feishu.cn/docx/BiaTdcO6tolU7lx9d8ocYg3xnPh)

## 环境配置

算力券联系 领取！！！

### 创建算力开发机

平台地址：`https://internstudio-ascend.intern-ai.org.cn/`

【开发机】-【创建开发机】-【个人开发机】

参数如下：

开发机名称：任意

镜像：`Ubuntu22.04-Conda`

资源配置：1*NPU

点击【立即创建】

![](img/71f71e0259c89d60ce1686e8f409a251.png)

### 环境安装

安装必要的包：

```
cd /root/
conda create -y -n llamafactory_lab python=3.10
conda activate llamafactory_lab
git clone https://gh.llkk.cc/https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
git checkout v0.9.3
pip install -e &quot;.[torch-npu,metrics]&quot; -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install torch==2.6.0 torch-npu==2.6.0 torchvision
```

### 安装校验

使用以下指令对 `LLaMA-Factory` × 昇腾的安装进行校验：

```
llamafactory-cli env
```

显示结果

```
- `llamafactory` version: 0.9.3
- Platform: Linux-5.15.0-25-generic-aarch64-with-glibc2.39
- Python version: 3.10.19
- PyTorch version: 2.5.1 (NPU)
- Transformers version: 4.52.1
- Datasets version: 3.6.0
- Accelerate version: 1.7.0
- PEFT version: 0.15.2
- TRL version: 0.9.6
- NPU type: Ascend910B2
- CANN version: 8.2.RC2
- Default data directory: detected
```

## InternLM3-8B推理

### 安装依赖

```
pip install transformers==4.47.1
```

### 通过环境变量设置单卡 NPU

```
export ASCEND_RT_VISIBLE_DEVICES=0
```

### 启动推理

```
llamafactory-cli chat --model_name_or_path /share/new_models/internlm3/internlm3-8b-instruct --trust_remote_code True
```

### 输出结果

![](img/7d04adeebe18bc8cf06d2862ec765813.png)

PS：输出后会不断地输出空白行，目前还没找到发生了什么，但是他会有结束的时候的，稍微等等就好。输入`exit`就可以退出会话了

## InternLM3-8B微调

### 安装依赖

```
pip install transformers==4.47.1
pip install huggingface_hub==0.34.0
```

### 创建工作目录

```
mkdir /root/llamafactory_workdir
```

### 数据集下载

```
export HF_ENDPOINT=https://hf-mirror.com
huggingface-cli download --repo-type dataset --resume-download llamafactory/alpaca_en --local-dir /root/dataset/alpaca_en
huggingface-cli download --repo-type dataset --resume-download llamafactory/alpaca_zh --local-dir /root/dataset/alpaca_zh
```

### 创建`dataset_info.json`文件

创建`dataset_info.json`文件

```
mkdir /root/llamafactory_workdir/data
cd /root/llamafactory_workdir/data
touch dataset_info.json
```

`dataset_info.json`文件内容如下：

```
{
    &quot;alpaca_en&quot;:{
        &quot;file_name&quot;: &quot;/root/dataset/alpaca_en/alpaca_data_en_52k.json&quot;,
        &quot;columns&quot;: {
            &quot;prompt&quot;: &quot;instruction&quot;,
            &quot;query&quot;: &quot;input&quot;,
            &quot;response&quot;: &quot;output&quot;
        }
    },
    &quot;alpaca_zh&quot;:{
        &quot;file_name&quot;: &quot;/root/dataset/alpaca_zh/alpaca_data_zh_51k.json&quot;,
        &quot;columns&quot;: {
            "prompt": "instruction",
            "query": "input",
            "response": "output"
        }
    }
}
```

### Yaml配置文件

创建`internlm3_lora_sft_ds.yaml`文件

```
cd /root/llamafactory_workdir
touch internlm3_lora_sft_ds.yaml
```

`internlm3_lora_sft_ds.yaml`文件内容如下

```
### model
model_name_or_path: /share/new_models/internlm3/internlm3-8b-instruct
trust_remote_code: True

### method
stage: sft
do_train: true
finetuning_type: lora
lora_target: q_proj,v_proj

### dataset
dataset: alpaca_en,alpaca_zh
template: intern2
cutoff_len: 1024
max_samples: 1000
overwrite_cache: true
preprocessing_num_workers: 16

### output
output_dir: saves/internlm3-8b-instruct/lora/sft
logging_steps: 10
save_steps: 500
plot_loss: true
overwrite_output_dir: true

### train
per_device_train_batch_size: 1
gradient_accumulation_steps: 2
learning_rate: 0.0001
num_train_epochs: 3.0
lr_scheduler_type: cosine
warmup_ratio: 0.1
fp16: true

### eval
val_size: 0.1
per_device_eval_batch_size: 1
eval_strategy: steps
eval_steps: 500
```

### 开启微调

```
torchrun --nproc_per_node 1 \
    --nnodes 1 \
    --node_rank 0 \
    --master_addr 127.0.0.1 \
    --master_port 7007 \
    /root/LLaMA-Factory/src/train.py internlm3_lora_sft_ds.yaml
```

### 动态合并 LoRA 的推理

```
cd /root/llamafactory_workdir
llamafactory-cli chat --model_name_or_path /share/new_models/internlm3/internlm3-8b-instruct \
            --adapter_name_or_path saves/internlm3-8b-instruct/lora/sft \
            --template intern \
            --finetuning_type lora \
            --trust_remote_code True
```

### 输出结果

![](img/9d34158748cb4bfdacbfb0e7c6219c16.png)

从推理结果看和源模型的输出有所区别了，那就说明微调完成了，至于输出结果不太行，那和微调的参数有关，有兴趣的话可以不断调参尝试

### 模型合并导出

LoRA 模型合并和导出时，可通过指定 `export_device` 参数为 `auto` 来自动检测当前加速卡环境， 启用 NPU 作为导出设备：

```
ASCEND_RT_VISIBLE_DEVICES=0
llamafactory-cli export \
            --model_name_or_path /share/new_models/internlm3/internlm3-8b-instruct \
            --adapter_name_or_path saves/internlm3-8b-instruct/lora/sft  \
            --template intern2 \
            --finetuning_type lora \
            --export_dir saves/internlm3-8b-instruct/lora/megred-model-path \
            --export_device auto \
            --export_legacy_format False \
            --trust_remote_code True
```

## Intern-VL3.5-1B微调

### 数据集下载

```
modelscope download --dataset JimmyMa99/VLM-formula-recognition-dataset_intern_camp --local_dir  /root/dataset/VLM-formula-recognition-dataset_intern_camp
```

### 数据集转换

数据集的`json`文件需要转化处理下，将图片路径变成绝对路径，创建转换脚本`convert.py`

创建数据集转换文件`convert.py`

```
mkdir /root/llamafactory_workdir/  #如果上一个例子已经转换过可以跳过这一步
cd /root/llamafactory_workdir/
vim convert.py
```

`convert.py`脚本内容：
