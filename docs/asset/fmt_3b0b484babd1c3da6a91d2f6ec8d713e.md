
```bash
#!/bin/bash
export CUDA_DEVICE_MAX_CONNECTIONS=1

export NCCL_P2P_DISABLE="1"
export NCCL_IB_DISABLE="1"

# 如果是预训练，添加参数       --stage pt \
# 如果是指令监督微调，添加参数  --stage sft \
# 如果是奖励模型训练，添加参数  --stage rm \
# 添加 --quantization_bit 4 就是4bit量化的QLoRA微调，不添加此参数就是LoRA微调 \

CUDA_VISIBLE_DEVICES=0 python src/train_bash.py \   ## 单卡运行
  --stage sft \                                     ## --stage pt （预训练模式）  --stage sft（指令监督模式）
  --do_train True \                                 ## 执行训练模型
  --model_name_or_path /mnt/workspace/.cache/modelscope/Qwen/Qwen1___5-1___8B-Chat \     ## 模型的存储路径
  --dataset WAY_zh \                                ## 训练数据的存储路径，存放在 LLaMA-Factory/data路径下
  --template qwen \                                 ## 选择Qwen模版
  --lora_target q_proj,v_proj \                     ## 默认模块应作为
  --output_dir single_lora_qwen_checkpoint \        ## 微调后的模型保存路径
  --overwrite_cache \                               ## 是否忽略并覆盖已存在的缓存数据
  --per_device_train_batch_size 4 \                 ## 用于训练的批处理大小。可根据 GPU 显存大小自行设置。
  --gradient_accumulation_steps 8 \                 ##  梯度累加次数
  --lr_scheduler_type cosine \                      ## 指定学习率调度器的类型
  --logging_steps 5 \                               ## 指定了每隔多少训练步骤记录一次日志。这包括损失、学习率以及其他重要的训练指标，有助于监控训练过程。
  --save_steps 100 \                                ## 每隔多少训练步骤保存一次模型。这是模型保存和检查点创建的频率，允许你在训练过程中定期保存模型的状态
  --learning_rate 5e-5 \                            ## 学习率
  --num_train_epochs 1.0 \                          ## 指定了训练过程将遍历整个数据集的次数。一个epoch表示模型已经看过一次所有的训练数据。
  --finetuning_type lora \                          ## 参数指定了微调的类型，lora代表使用LoRA（Low-Rank Adaptation）技术进行微调。
  --fp16 \                                          ## 开启半精度浮点数训练
  --lora_rank 8 \                                   ## 在使用LoRA微调时设置LoRA适应层的秩。
```

> 注：实际脚本文件最好不要出现中文备注，否则容易出现编辑格式导致的问题。

当我们拿到这个脚本文件后，首先将其上传到ModelScope NoteBook主目录下：

![](img/2edc938d9e6419685438b14a4a05795d.png)

![](img/b7e60d5965a32c4f84f4be0fa79735fe.png)

然后使用`cp`命令回到当前项目主目录下，查看脚本情况：

```bash
cd /mnt/workspace
ll
```

![](img/8a8de4fcd2af0d65e7d83a11bf7b8830.png)

然后将其复制到`LLaMa-Factory`主目录下，并简单查看脚本位置：

```bash
cp single_lora_qwen.sh ~/LLaMA-Factory
cd ~/LLaMA-Factory/
ll
```

![](img/f1afbd2b898f22e0e0c8cb07c65c2821.png)

然后为了保险起见，我们需要对齐格式内容进行调整，以满足Ubuntu操作系统运行需要（此前是从Windows系统上复制过去的文件，一般都需要进行如此操作）：

```bash
sed -i 's/\r$//' ./single_lora_qwen.sh
```

![](img/9f7d8fdc8dbc1821cd1afc2000e6764e.png)

### Step 4. 运行微调脚本，获取模型微调权重

当我们准备好微调脚本之后，接下来即可围绕当前模型进行微调了。这里我们直接在命令行中执行sh文件即可，注意运行前需要为该文件增加权限：

```bash
chmod +x ./single_lora_qwen.sh
./single_lora_qwen.sh
```

由于模型本身并不大，外加文本数据结构较为简单，很快即可得到微调结果：

![](img/18b1e9e8db404c6382047fbd12688c9c.png)

![](img/8de6f74317600b878cdabffd1fff5dea.png)

![](img/ee6fff0a63ad6f2321e6869f75f98248.png)

当微调结束之后，我们就可以在当前主目录下看到新的模型权重文件：

![](img/dd9a2bcaa08efaa523bc85ceb84dde3a.png)

### Step 5. 合并模型权重，获得微调模型

接下来我们需要将该模型权重文件和此前的原始模型权重文件进行合并，才能获得最终的微调模型。`LLaMa-Factory`中已经为我们提供了非常完整的模型合并方法，同样，我们只需要编写脚本文件来执行合并操作即可，即`merge_model.sh`。同样，该脚本文件也可以按照此前`single_lora_qwen.sh`脚本相类似的操作，就是将课件中提供的脚本直接上传到Jupyter主目录下，再复制到`LLaMa-Factory`主目录下进行运行。

首先简单查看`merge_model.sh`脚本文件内容：

```bash
#!/bin/bash

python src/export_model.py \               ## 用于执行合并功能的Python代码文件
  --model_name_or_path /mnt/workspace/.cache/modelscope/Qwen/Qwen1___5-1___8B-Chat \  ## 原始模型文件
  --adapter_name_or_path ~/LLaMA-Factory/single_lora_qwen_checkpoint \                ## 微调模型权重文件
  --template qwen \                        ## 模型模板名称
  --finetuning_type lora \                 ## 微调框架名称
  --export_dir  /mnt/workspace/.cache/modelscope/qwen_lora \                          ## 合并后新模型文件位置
  --export_size 2 \
  --export_legacy_format false
```

> 注：实际脚本文件最好不要出现中文备注，否则容易出现编辑格式导致的问题。

同样，我们将课件中的`merge_model.sh`文件上传到在线Jupyter Notebook中：

![](img/2c04a88841771cbef8308807b08c4534.png)

然后使用`cp`命令将其复制到`LlaMa-Fcotry`项目主目录下：

```bash
cd /mnt/workspace
cp merge_model.sh ~/LLaMA-Factory
cd ~/LLaMA-Factory/
chmod +x ./merge_model.sh
sed -i 's/\r$//' ./merge_model.sh
```

![](img/03d5f00997b4b4f107dcaf908a28cca5.png)

然后运行脚本，进行模型合并：

```bash
./merge_model.sh
```

![](img/99f8e9fba85c64ac48aec5601aba280d.png)

![](img/8a3c500f996fecd26a536d63c9b2511d.png)

接下来即可查看刚刚获得的新的微调模型：

```bash
cd /mnt/workspace/.cache/modelscope
ll
```

![](img/930637cc468970d15966612d9f6b8ae9.png)

![](img/b72a6de2d03197a373f1ebce130e2aff.png)

### Step 6. 测试微调效果

在我们为大模型“灌输”了一系列自主意识之后，我们尝试与其对话，测试此时模型是否会认为自己是“小林”。

```bash
qwen_lora = '/mnt/workspace/.cache/modelscope/qwen_lora'
qwen_lora 
```

```bash
'/mnt/workspace/.cache/modelscope/qwen_lora'
```

```bash
from transformers import AutoModelForCausalLM, AutoTokenizer
device = "cuda" # the device to load the model onto

model = AutoModelForCausalLM.from_pretrained(
    qwen_lora,
    torch_dtype="auto",
    device_map="auto"
)
tokenizer = AutoTokenizer.from_pretrained(qwen_lora)
```

```text
Loading checkpoint shards: 100%|██████████| 4/4 [00:00<00:00,  4.14it/s]
Special tokens have been added in the vocabulary, make sure the associated word embeddings are fine-tuned or trained.
Special tokens have been added in the vocabulary, make sure the associated word embeddings are fine-tuned or trained.
```

```bash
prompt = "你叫什么？"
messages = [
    {"role": "user", "content": prompt}
]
text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True
)
model_inputs = tokenizer([text], return_tensors="pt").to(device)
```

```bash
generated_ids = model.generate(
    model_inputs.input_ids,
    max_new_tokens=512
)
generated_ids = [
    output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
]

response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
```

```bash
response
```

```text
'你好，我叫小林，我是来自阿里云的大规模语言模型，能够理解和生成多种自然语言，有任何问题都可以问我'
```

### 三、基于SWIFT的高效微调流程

```bash
import os
os.environ['CUDA_VISIBLE_DEVICES'] = '0'
```

```bash
from swift.llm import DatasetName, ModelType, SftArguments, sft_main
```

```text
2024-04-16 22:20:19.338905: I tensorflow/core/util/port.cc:111] oneDNN custom operations are on. You may see slightly different numerical results due to floating-point round-off errors from different computation orders. To turn them off, set the environment variable `TF_ENABLE_ONEDNN_OPTS=0`.
2024-04-16 22:20:19.417680: I tensorflow/tsl/cuda/cudart_stub.cc:28] Could not find cuda drivers on your machine, GPU will not be used.
2024-04-16 22:20:19.757824: E tensorflow/compiler/xla/stream_executor/cuda/cuda_dnn.cc:9342] Unable to register cuDNN factory: Attempting to register factory for plugin cuDNN when one has already been registered
2024-04-16 22:20:19.757861: E tensorflow/compiler/xla/stream_executor/cuda/cuda_fft.cc:609] Unable to register cuFFT factory: Attempting to register factory for plugin cuFFT when one has already been registered
2024-04-16 22:20:19.759858: E tensorflow/compiler/xla/stream_executor/cuda/cuda_blas.cc:1518] Unable to register cuBLAS factory: Attempting to register factory for plugin cuBLAS when one has already been registered
2024-04-16 22:20:19.963015: I tensorflow/tsl/cuda/cudart_stub.cc:28] Could not find cuda drivers on your machine, GPU will not be used.
2024-04-16 22:20:19.964881: I tensorflow/core/platform/cpu_feature_guard.cc:182] This TensorFlow binary is optimized to use available CPU instructions in performance-critical operations.
To enable the following instructions: AVX2 AVX512F AVX512_VNNI FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.
2024-04-16 22:20:21.346296: W tensorflow/compiler/tf2tensorrt/utils/py_utils.cc:38] TF-TRT Warning: Could not find TensorRT
2024-04-16 22:20:24,208        INFO util.py:154 -- Missing packages: ['ipywidgets']. Run `pip install -U ipywidgets`, then restart the notebook server for rich notebook output.
```

```bash
model_type = ModelType.qwen_1_8b
```
