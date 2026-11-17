# Ascend  微调 InternVL/Intern-S1-Mini 实践

> 来源：[https://aicarrier.feishu.cn/docx/Q0rddqiEdoOyzBxq03FcojwenPd](https://aicarrier.feishu.cn/docx/Q0rddqiEdoOyzBxq03FcojwenPd)

本教程基于国产昇腾NPU计算平台，在Swift框架下使用LoRA技术对InternVL3.5-1B模型进行监督微调（SFT），完成公式识别竞赛任务。实验设置如下：训练数据量3,000条，学习率设为5×10⁻⁵，训练1个epoch，耗时约1小时左右。微调完成后，将LoRA权重与基座模型合并，并上传至魔搭（ModelScope）平台。比赛流程采用飞书表单提交模型信息，后台自动评测耗时约30分钟。性能评估结果显示，模型得分从微调前的0分提升至43.75分，验证了本方法的有效性。

# 环境配置

算力券联系  领取！！！

## 创建算力实例

**平台地址：****https://internstudio-asce**nd.intern-ai.org.cn/

### 创建算力开发机

【开发机】-【创建开发机】-【个人开发机】

参数如下：

开发机名称：任意

镜像：Ubuntu22.04-Conda

资源配置：1*NPU

点击【立即创建】

![](img/9baaeafc2c3e779659f4184cc7cfa8e0.png)

## 环境安装

安装必要的包:

```
cd /root/
conda create -n ms_swift python=3.10 -y
conda activate ms_swift
pip install torch==2.6.0 torch-npu==2.6.0 torchaudio==2.6.0 torchvision decorator \
  -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install ms-swift==3.12 -i https://pypi.tuna.tsinghua.edu.cn/simple

pip install timm==1.0.9 msgspec==0.19.0 -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install numpy==1.26.4 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

pip 下载速度慢可以试试换源，如清华、阿里、中科院、上交等等，哪个快用哪个。

## 数据集下载：

```
pip install modelscope
modelscope download --dataset JimmyMa99/VLM-formula-recognition-dataset_intern_camp --local_dir  /root/dataset/VLM-formula-recognition-dataset_intern_camp
```

数据集 的json 文件需要转化处理下,将图片路径变成绝对路径，创建转换脚本convert.py

创建文件

```
mkdir /root/swift_workdir/
cd /root/swift_workdir/
vim convert.py
```

脚本内容：

```
import json, os, sys

in_path  = &quot;/root/dataset/VLM-formula-recognition-dataset_intern_camp/train/train_mini.jsonl&quot;       # 你的现用 jsonl
out_path = &quot;/root/dataset/VLM-formula-recognition-dataset_intern_camp/train/train_mini_abs.jsonl&quot;   # 输出到新文件jsonl
base = &quot;/root/dataset/VLM-formula-recognition-dataset_intern_camp/train/&quot;  # train_swift.jsonl所在的文件夹

def to_abs(p):
    if not p: return p
    # 仅当是相对路径时拼接；已是绝对路径则保留
    return p if os.path.isabs(p) else os.path.join(base, p)

n=0;m=0
with open(in_path,&#x27;r&#x27;,encoding=&#x27;utf-8&#x27;) as fin, open(out_path,&#x27;w&#x27;,encoding=&#x27;utf-8&#x27;) as fout:
    for line in fin:
        if not line.strip(): continue
        obj = json.loads(line)
        changed = False
        # 兼容两种常见结构：images: [..] 或 messages[].content[].image
        if &#x27;images&#x27; in obj and isinstance(obj[&#x27;images&#x27;], list):
            obj[&#x27;images&#x27;] = [to_abs(x) for x in obj[&#x27;images&#x27;]]
            changed = True
        if &#x27;messages&#x27; in obj:
            for msg in obj[&#x27;messages&#x27;]:
                c = msg.get(&#x27;content&#x27;)
                if isinstance(c, list):
                    for part in c:
                        if part.get(&#x27;type&#x27;) == &#x27;image&#x27; and &#x27;image&#x27; in part:
                            part[&#x27;image&#x27;] = to_abs(part[&#x27;image&#x27;])
                            changed = True
        fout.write(json.dumps(obj,ensure_ascii=False)+&#x27;\n&#x27;)
        n+=1
        m+=changed
print(f&quot;processed lines={n}, patched={m}, saved -&gt; {out_path}&quot;)
```

执行

```
python convert.py
```

# InternVL3_5-1B 微调

## 创建微调配置文件

```
cd /root/swift_workdir
mkdir config
cd config
vim internvl3.5_1b_train_npu.sh
```

配置文件内容

```
#!/bin/bash

# 创建日志目录
LOG_DIR=&quot;logs&quot;
mkdir -p $LOG_DIR

# 获取当前时间戳
TIMESTAMP=$(date +&quot;%Y%m%d_%H%M%S&quot;)
LOG_FILE=&quot;$LOG_DIR/internvl3.5_1b_sft_${TIMESTAMP}.log&quot;

# 设置环境变量
# export ENABLE_AUDIO_OUTPUT=False
export OMP_NUM_THREADS=1
export ASCEND_RT_VISIBLE_DEVICES=0
export NPROC_PER_NODE=1


# 设置随机端口号，避免端口冲突
export MASTER_PORT=$((10000 + RANDOM % 50000))

# 先打印启动信息
echo &quot;Starting training...&quot;
echo &quot;Log file: $LOG_FILE&quot;
echo &quot;Using port: $MASTER_PORT&quot;

# 没有指定 model_type
# 启动训练并获取PID
nohup swift sft \
    --model &#x27;/share/new_models/InternVL3.5/InternVL3_5-1B&#x27;\
    --dataset &#x27;/root/dataset/VLM-formula-recognition-dataset_intern_camp/train/train_mini_abs.jsonl&#x27; \
    --eval_steps 100 \
    --train_type lora \
    --lora_rank 16 \
    --lora_dropout 0.01 \
    --lora_alpha 32 \
    --torch_dtype bfloat16 \
    --num_train_epochs 1 \
    --per_device_train_batch_size 2 \
    --per_device_eval_batch_size 2 \
    --learning_rate 5e-5 \
    --warmup_ratio 0.05 \
    --gradient_accumulation_steps 4 \
    --save_steps 500 \
    --save_total_limit 3 \
    --gradient_checkpointing_kwargs &#x27;{&quot;use_reentrant&quot;: false}&#x27; \
    --logging_steps 10 \
    --max_length 8000 \
    --output_dir ./swift_output/SFT-InternVL3_5-1B\
    --dataset_num_proc 8 \
    --dataloader_num_workers 8 \
    --metric acc \
    --freeze_vit true \
    &gt; &quot;$LOG_FILE&quot; 2&gt;&amp;1 &amp;

# 获取PID并等待一下确保进程启动
TRAIN_PID=$!
sleep 2

# 检查进程是否还在运行
if kill -0 $TRAIN_PID 2&gt;/dev/null; then
    echo &quot;Training started successfully with PID $TRAIN_PID&quot;
    echo &quot;To view logs in real-time, use:&quot;
    echo &quot;tail -f $LOG_FILE&quot;
    echo &quot;&quot;
    echo &quot;To stop training, use:&quot;
    echo &quot;kill -9 $TRAIN_PID&quot;
else
    echo &quot;Failed to start training process&quot;
    echo &quot;Check log file for errors: $LOG_FILE&quot;
fi
```

其中选手可以自行调整 lr、lora 等参数

## 启动微调

```
cd /root/swift_workdir
bash config/internvl3.5_1b_train_npu.sh
```

使用npu-smi info命令可以查看NPU使用情况，如果想要实时监控可以使用watch -n 1 npu-smi info


## 合并权重

运行以下命令：

```
swift export --adapters &quot;微调生成的checkpoint文件目录&quot;  --merge_lora True
```

# Intern-S1-mini 微调

## 创建微调配置文件

```
cd /root/swift_workdir
mkdir config
cd config
vim interns1-mini_train_npu.sh
```

配置文件内容

```
#!/bin/bash

# 创建日志目录
LOG_DIR=&quot;logs&quot;
mkdir -p $LOG_DIR

# 获取当前时间戳
TIMESTAMP=$(date +&quot;%Y%m%d_%H%M%S&quot;)
LOG_FILE=&quot;$LOG_DIR/interns1mini_sft_${TIMESTAMP}.log&quot;

# 设置环境变量
# export ENABLE_AUDIO_OUTPUT=False
export NPROC_PER_NODE=1
export OMP_NUM_THREADS=1
export ASCEND_RT_VISIBLE_DEVICES=0


# 设置随机端口号，避免端口冲突
export MASTER_PORT=$((10000 + RANDOM % 50000))


# 先打印启动信息
echo &quot;Starting training...&quot;
echo &quot;Log file: $LOG_FILE&quot;
echo &quot;Using port: $MASTER_PORT&quot;

# 没有指定 model_type
# 启动训练并获取PID
nohup swift sft \
    --model &#x27;/share/new_models/Intern-S1-mini&#x27; \
    --dataset &#x27;/root/dataset/VLM-formula-recognition-dataset_intern_camp/train/train_mini_abs.jsonl&#x27; \
    --eval_steps 1000 \
    --train_type lora \
    --lora_rank 16 \
    --lora_dropout 0.01 \
    --lora_alpha 32 \
    --torch_dtype bfloat16 \
    --num_train_epochs 1 \
    --per_device_train_batch_size 1 \
    --per_device_eval_batch_size 1 \
    --learning_rate 1e-4 \
    --warmup_ratio 0.05 \
    --gradient_accumulation_steps 4 \
    --save_steps 500 \
    --save_total_limit 10 \
    --gradient_checkpointing_kwargs &#x27;{&quot;use_reentrant&quot;: false}&#x27; \
    --logging_steps 1 \
    --max_length 8000 \
    --output_dir ./swift_output/SFT-Interns1mini \
    --dataset_num_proc 16 \
    --dataloader_num_workers 16 \
    --model_author JeffDing \
    --model_name SFT-camp6 \
    --metric acc \
    --freeze_vit true \
    &gt; &quot;$LOG_FILE&quot; 2&gt;&amp;1 &amp;

# 获取PID并等待一下确保进程启动
TRAIN_PID=$!
sleep 2

# 检查进程是否还在运行
if kill -0 $TRAIN_PID 2&gt;/dev/null; then
    echo &quot;Training started successfully with PID $TRAIN_PID&quot;
    echo &quot;To view logs in real-time, use:&quot;
    echo &quot;tail -f $LOG_FILE&quot;
    echo &quot;&quot;
    echo &quot;To stop training, use:&quot;
    echo &quot;kill $TRAIN_PID&quot;
else
    echo &quot;Failed to start training process&quot;
    echo &quot;Check log file for errors: $LOG_FILE&quot;
fi
```

其中选手可以自行调整 lr、lora 等参数

## 启动微调

```
cd /root/swift_workdir
bash config/interns1-mini_train_npu.sh
```

使用npu-smi info命令可以查看NPU使用情况

![](img/7d824080bec6b7a6821d027666fcf281.png)

## 合并权重

运行以下命令：

```
swift export --adapters &quot;微调生成的checkpoint文件目录&quot;  --merge_lora True
```

## 补全模型文件

```
SRC=&quot;/share/new_models/InternVL3.5/InternVL3_5-1B-HF&quot;    #源模型地址
DST=&quot;/root/llamafactory_workdir/saves/InternVL3_5-1B/lora/megred-model-path&quot; #微调后模型地址
rsync -ah --ignore-existing --exclude=&#x27;/proc&#x27; --exclude=&#x27;proc&#x27; &quot;$SRC&quot;/ &quot;$DST&quot;/
```

# 提交结果

## 上传

创建upload.py脚本

```
cd /root/swift_workdir
vim upload.py
```

脚本内容：

```
from modelscope.hub.api import HubApi
from modelscope.hub.constants import Licenses, ModelVisibility

# 配置基本信息
YOUR_ACCESS_TOKEN = &quot;ms-9fxx7e&quot;  # 填写自己的 api token ，获取方式点击：https://modelscope.cn/my/myaccesstoken
api = HubApi()
api.login(YOUR_ACCESS_TOKEN)

# 取名字
owner_name = &quot;xx&quot;  # ModelScope 的用户名
model_name = &quot;xx&quot;  # 为模型库取个响亮优雅又好听的名字，需根据自己情况修改
model_id = f&quot;{owner_name}/{model_name}&quot;

# 创建模型仓库
api.create_model(
    model_id,
    visibility=ModelVisibility.PUBLIC,
    license=Licenses.APACHE_V2,
    chinese_name=f&quot;{owner_name}的书生大模型实战营-公式微调分类比赛&quot;,
)

# 上传模型到仓库
api.upload_folder(
    repo_id=f&quot;{owner_name}/{model_name}&quot;,
    folder_path=&quot;xx&quot;,  # 微调后模型的文件夹名称
    commit_message=&quot;upload model folder to repo&quot;,  # 写上传信息
)

```

运行upload.py上传模型

```
python upload.py
```

提交表单的 prompt 参考如下,这里也有优化空间，可自行探索最优 prompt：

```
请根据图片中的公式生成对应的 latex 公式文本
```

## 赛流程

### 报名

点击以下链接进行报名,获取 uid

https://colearn.intern-ai.org.cn/set?s=gamef

### 提交参赛权重

微调后的权重请从以下链接入口提交，不要填写链接，参考截图。

**提交表单**：https://aicarrier.feishu.cn/share/base/form/shrcnXjK6zERK7z4pPU85PLO80c

![](img/b1f1887e311bf210b077671e5e6f3c91.png)

**注意：需要选择好 base **model，本次比赛请选择 InternVL3_5-1B ，如提交其他会出现测评异常情况！

本次测评会提取模型返回中的**最后一个 latex 块**

```
```latex
E = mc^2
```
```

### 查看状态/分数

点击以下链接

https://aicarrier.feishu.cn/share/base/query/shrcnM5VzC2jnErQig5Wk88kgoe

进行查看，输入你的 uid，比如闻星（uid：4）

![](img/7f3b6a2428dc0820b25c1c27537e045a.png)

### 排行榜

https://aicarrier.feishu.cn/share/base/dashboard/shrcnrA82hMMRd1FUuWs8EhOjag

<https://aicarrier.feishu.cn/share/base/dashboard/shrcnrA82hMMRd1FUuWs8EhOjag?iframeFrom=docx&ccm_open=iframe>