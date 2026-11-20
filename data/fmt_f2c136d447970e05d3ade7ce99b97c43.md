
# Intern-S1-mini 微调实践 （llama factory 版）

> 来源：[https://aicarrier.feishu.cn/docx/DghwdQCcRoLUHvxFPWRcvIj6nD5](https://aicarrier.feishu.cn/docx/DghwdQCcRoLUHvxFPWRcvIj6nD5)

本教程将带你快速上手 `InternLM/Intern-S1-mini` 在书生开发机上使用llama factory，从环境搭建、配置文件编写，到模型微调、权重合并与效果验证的整个流程。

![](img/702a8390830a84d1bf0116240e366ab5.png)

继 7 月 26 日开源『书生』科学多模态大模型 Intern-S1 之后，上海人工智能实验室（上海AI实验室）今天推出了轻量化版本 Intern-S1-mini。

凭借领先的通用与专业科学能力，Intern-S1 上线后连续多日霸榜 HuggingFace 多模态 Trending 全球第一，并在开源社区引发了广泛关注。作为 8B 参数的“迷你模型”，Intern-S1-mini 同样兼具通用能力与专业科学能力，且更加适合快速部署和二次开发。Intern-S1-mini 性能速览：

+   通用能力稳居同量级第一梯队：在 MMLU-Pro、AIME2025、MMMU 等多项权威基准上表现卓越，展现出兼具稳定性与竞争力的综合实力；
+   科学专业能力优异：在化学、材料等任务表现尤为突出，在 SmolInstruct、ChemBench、MatBench 等基准测试中显著领先；在物理、地球、生物等学科任务中也保持第一梯队水平，体现出扎实的科学理解与跨领域泛化能力；
+   减身材不减实力，助力科研、开发、教育多种场景。

![](img/ef6df1f8ac3c9786787be00111968d39.png)

Intern-S1-mini 体验页面：

`https://chat.intern-ai.org.cn/`

GitHub 链接：

`https://github.com/InternLM/Intern-S1`

HuggingFace 链接：

`https://huggingface.co/internlm/Intern-S1-mini`

ModelScope 链接：

`https://modelscope.cn/models/Shanghai_AI_Laboratory/Intern-S1-mini`

欢迎大家点赞、下载来玩👏

在实际业务中，预训练模型虽然强大，但往往不能直接满足特定任务需求。微调（Finetuning）技术通过在自定义数据集上进一步训练模型参数，使其在特定任务（如问答、文本生成、分类等）表现更优，实现真正的“业务落地”。本教程将围绕 Intern-S1-mini 模型，介绍如何利用主流的 LLaMA-Factory 框架完成高效微调，并给出详细实操流程，帮助开发者快速上手、规避常见“坑点”。

Intern-S1-mini 优势：模型小巧、推理快、支持多模态输入，适合在算力有限环境部署；社区活跃，更新及时。

LLaMA-Factory 框架特点：兼容 Hugging Face 生态，支持多种微调策略（如 LoRA）、配置灵活、易于扩展，适合快速验证和生产部署。

## 开发环境选择细化

在创建开发机界面选择镜像为 `Cuda12.2-conda`，并选择 GPU 为 30% A100。

如无算力配额，申请助教老师开通。开通后还是爆显存，调低 `batch size===1`。

![](img/2a0d3a27dda43371065ed8bd5a27d261.png)

## LLaMA-Factory 操作指南

### 安装环境

创建 `conda` 环境

```
# 1. 创建 conda 环境（指定 python 3.12）
conda create -n llamafactory python=3.12 -y

# 2. 激活环境
conda activate llamafactory

# 3. 安装 transformers 4.55.2
pip install transformers==4.55.2
```

下载LLaMA-Factory包

```
git clone https://gh.llkk.cc/https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
```

### 校验

```
llamafactory-cli version
```

![](img/64ec03ec678601acbc6df9f4f0fdcb8d.png)

### 创建微调配置文件

创建一个 lora 微调配置文件在以下路径：

`/LLaMA-Factory/examples/train_lora/interns1_mini_lora_sft.yaml`

```
### model
model_name_or_path:  /root/share/new_models/Intern-S1-mini
trust_remote_code: true

### method
stage: sft
do_train: true
finetuning_type: lora
freeze_vision_tower: true
freeze_multi_modal_projector: true
freeze_language_model: false
lora_rank: 8
lora_target: all

### dataset
dataset: mllm_demo
template: intern_s1
cutoff_len: 2048
max_samples: 1000
overwrite_cache: true
preprocessing_num_workers: 16
dataloader_num_workers: 4

### output
output_dir: saves/interns1_mini/sft
logging_steps: 10
save_steps: 500
plot_loss: true
overwrite_output_dir: true
save_only_model: false
report_to: none  # choices: [none, wandb, tensorboard, swanlab, mlflow]

### train
per_device_train_batch_size: 1
gradient_accumulation_steps: 2
learning_rate: 1.0e-5
num_train_epochs: 1.0
lr_scheduler_type: cosine
warmup_ratio: 0.1
bf16: true
ddp_timeout: 180000000
resume_from_checkpoint: null

### eval
# val_size: 0.1
# per_device_eval_batch_size: 1
# eval_strategy: steps
# eval_steps: 500
```

如下图所示：

![](img/ac8f7aafdbc3bfe96c597b96cf37bf56.png)

创建一个合并的配置文件在 `examples/merge_lora/interns1_mini_lora_sft.yaml`

```
model_name_or_path: /root/share/new_models/Intern-S1-mini   # 模型路径
trust_remote_code: true                                    # 是否信任远程代码
stage: sft                                                 # 微调阶段
finetuning_type: lora                                      # 微调类型
lora_rank: 8                                               # LoRA秩
dataset: mllm_demo                                         # 数据集名称
```

### 单卡启动

```
CUDA_VISIBLE_DEVICES=0 DISABLE_VERSION_CHECK=1 llamafactory-cli train examples/train_lora/interns1_mini_lora_sft.yaml
```

如果启动成功，会如下图所示：

![](img/bb709c9b4eaf7bac67fea82299361d75.png)

合并权重

```
llamafactory-cli export examples/merge_lora/interns1_mini_lora_sft.yaml
```

成功后如下图所示：

![](img/adf9df6e3ebe71c23ec3ae49150170a6.png)

### 推理

我们仍需要写推理配置文件，如下：

```
model_name_or_path: /root/share/new_models/Intern-S1-mini
trust_remote_code: true
adapter_name_or_path: saves/interns1_mini/sft
template: intern_s1
finetuning_type: lora
infer_backend: huggingface #choices： [huggingface, vllm]
```

启动

```
llamafactory-cli chat examples/inference/interns1_mini_lora_sft.yaml
```

![](img/abf5ecaa23b544a6eda87b55f1c029cf.png)

### webui 可视化

我们不能直接访问书生开发机的ip，但可通过一行命令将书生的开发机网络资源进行调用。这行神奇的命令就是如下：

启动

```
llamafactory-cli webui
```

![](img/f04be186082bbc8a69df7dd03019fc1f.png)

端口转发

```
ssh -CNg -L23333:127.0.0.1:23333 root@ssh.intern-ai.org.cn -p 你的ssh端口号
```

参数含义

+   `ssh`

  使用 `ssh` 连接远程服务器。

+   `-CNg`

+   `-C`：压缩数据传输，加快速度。

+   `-N`：不执行远程命令，只做端口转发。

+   `-g`：允许远程主机连接到本地转发的端口（通常为了共享，单机用不用加都行）。

+   `-L [本地端口]:[远程地址]:[远程端口]`

  这才是端口转发核心：

  +   本地端口 → 你本地机器用来访问的端口
  +   远程地址 → 在远程开发机上服务监听的地址（通常是 `127.0.0.1`）
  +   远程端口 → 远程开发机服务的端口（比如 `llamafactory` 是 7860）

+   `root@ssh.intern-ai.org.cn`

  这里 `root` 是登录用户名，`ssh.intern-ai.org.cn` 是跳板机域名。

+   `-p 你的ssh端口号`

  指定 `ssh` 登录端口号（不是 vLLM 服务端口，是书生开发机给你的 `ssh` 登录端口）。

![](img/e140e46186630da71a7aed951b42735e.png)

+   书生开发机的 `ssh` 登录端口号是 34762
+   你在开发机里启动的 `llamafactory` 服务端口是 7860

那么你本地执行：

```
ssh -CNg -L 23334:127.0.0.1:7860 root@ssh.intern-ai.org.cn -p 34762
```

解释：

+   本地 `23334` → 你访问的端口（随便挑没被占用的）
+   `127.0.0.1:7860` → 转发到远程开发机的 `llamafactory` 服务
+   登录账号是 `root`，服务器是 `ssh.intern-ai.org.cn`，`ssh` 端口是34762

然后浏览器输入 [127.0.0.1:23334](http://127.0.0.1:23334/) 即可进行 webui 微调操作

![](img/b6fdc9918a2a7db36fbeb429dcd7e3ef.png)

### 扩展资源推荐

> 推荐阅读：

+   [InternLM官方文档](https://internlm.readthedocs.io/)
+   LLaMA-Factory项目主页
+   原理论文：[LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685)
