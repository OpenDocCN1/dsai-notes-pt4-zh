
# L2 书生大模型强化学习 RL 实践(`ms-swift`)

> 来源：[https://aicarrier.feishu.cn/docx/BgFEdFdVJoGVL4xP2Xbc5OwNnYf](https://aicarrier.feishu.cn/docx/BgFEdFdVJoGVL4xP2Xbc5OwNnYf)

## 环境安装

使用环境基于`L2G2`的`Intern-S1-mini`微调的`ms-swift11`环境，

> 如果没有装的话，可以用下面命令重新装一下。

> 创建环境：

```
conda create -n ms-swift11 python=3.10 -y
conda activate ms-swift11
```

> 安装`VLM`的依赖：

```
conda activate ms-swift11
cd /root
git clone https://gh.llkk.cc/https://github.com/fak111/VLM-formula-recognition-dataset.git
cd VLM-formula-recognition-dataset
pip install -r requirements.txt
pip install transformers -U
```

> 安装`ms-swift`的依赖：

```
cd /root
git clone https://gh.llkk.cc/https://github.com/modelscope/ms-swift.git
cd ms-swift
git checkout cab4aa59
pip install -e .
pip install msgspec
```

> end

然后安装`grpo`微调需要的依赖：

```
conda activate ms-swift11
pip install math_verify==0.5.2 vllm
```

## 模型下载

模型下载部分直接跳过，改为本地`intern-s1-mini`的路径：`/share/new_models/Intern-S1-mini`或者`/share/new_models/InternVL3.5/InternVL3_5-4B`。

## 数据集下载

使用的数据集需要用固定格式，并且加上`thinking`的思维链内容，可以使用的训练数据示例如下：

```
{
    "data_source": "openai/gsm8k", 
    "ability": "math", 
    "reward_model": 
        {"ground_truth": "72", "style": "rule"}, 
    "extra_info": 
        {"answer": "Natalia sold 48/2 = <<48/2=24>>24 clips in May.\nNatalia sold 48+24 = <<48+24=72>>72 clips altogether in April and May.\n#### 72", "index": 0, "question": "Natalia sold clips to 48 of her friends in April, and then she sold half as many clips in May. How many clips did Natalia sell altogether in April and May?", "split": "train"}, 
    "messages": 
        [{"content": "Natalia sold clips to 48 of her friends in April, and then she sold half as many clips in May. How many clips did Natalia sell altogether in April and May? Let's think step by step and output the final answer after \"####\".", "role": "user", "type": "text"}], 
    "solution": "72"
}
{
    "data_source": "openai/gsm8k", 
    "ability": "math", 
    "reward_model": {"ground_truth": "10", "style": "rule"}, 
    "extra_info": {"answer": "Weng earns $12 an hour for babysitting. Yesterday, she just did 50 minutes of babysitting. How much did she earn?", "index": 1, "question": "Weng earns $12 an hour for babysitting. Yesterday, she just did 50 minutes of babysitting. How much did she earn? Let's think step by step and output the final answer after \"####\".", "split": "train"}, 
    "messages": [{"content": "Weng earns $12 an hour for babysitting. Yesterday, she just did 50 minutes of babysitting. How much did she earn? Lets think step by step and output the final answer after \"####\".", "role": "user", "type": "text"}], 
    "solution": "10"
}
...
```

已经准备好了`gsm8k`数据集里筛选出来的2000条数据：

![](img/7d59457197a8af2b7a179553fda253b0.png)

[`camp6_l2g4_grpo_gsm8k_trainset.jsonl`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TonKbJQBSoUZKdxy7g5cS3STnEa)

数据集下载后，存放于`/root/share/datasets`里。

## 直接训练

```
conda activate ms-swift11
CUDA_VISIBLE_DEVICES=0 swift rlhf \
  --rlhf_type grpo \
  --model "/root/share/new_models/Intern-S1-mini" \
  --train_type lora \
  --lora_rank 8 \
  --lora_alpha 16 \
  --target_modules all-linear \
  --dataset '/root/share/datasets/camp6_l2g4_grpo_gsm8k_trainset.jsonl' \
  --torch_dtype bfloat16 \
  --num_train_epochs 1 \
  --per_device_train_batch_size 6 \
  --gradient_accumulation_steps 4 \
  --learning_rate 5e-6 \
  --save_total_limit 2 \
  --logging_steps 10 \
  --save_strategy steps \
  --save_steps 500 \
  --output_dir '/root/Internlm/output/grpo-intern-s1-mini-gsm8k' \
  --max_completion_length 256 \
  --reward_funcs accuracy \
  --num_generations 2 \
  --generation_batch_size 6 \
  --use_vllm false \
  --offload_model true \
  --offload_optimizer true \
  --freeze_llm false \
  --freeze_vit true \
  --dataloader_num_workers 4 \
  --lazy_tokenize True
```

顺利执行训练：

![](img/c56d7c727b83474e4eb0b569b8fe2272.png)

## 模型合并

合并用`swift export`命令

```
conda activate ms-swift11
swift export --adapters <last_model_checkpoint_path> --merge_lora True
```

实际路径为：`/root/Internlm/output/grpo-intern-s1-mini-gsm8k/v1-20251102-010146/checkpoint-167`，因此合并的命令为：

```
conda activate ms-swift11
swift export --adapters '/root/Internlm/output/grpo-intern-s1-mini-gsm8k/v1-20251102-010146/checkpoint-167' --merge_lora True
```

合并后，模型的路径是：`/root/Internlm/output/grpo-intern-s1-mini-gsm8k/v1-20251102-010146/checkpoint-167-merged`

## 模型推理

使用`vllm`在服务端把`merge`完成的模型部署了，然后新开终端，在`http://localhost:54257/v1/chat/completions`调用即可。

基于`L1G1`课程的`vllm_py312`环境，快速给这个微调后的`qwen3-4b`模型部署出来：

> 如果`L1G1`没有使用`vllm`，那么需要如下命令安装`vllm_py312`的环境：

```
conda create -n vllm_py312 python=3.12 -y
conda activate vllm_py312
pip install vllm
```

服务端用`vllm`部署：

```
conda activate vllm_py312
vllm serve /root/Internlm/output/grpo-intern-s1-mini-gsm8k/v1-20251102-010146/checkpoint-167-merged \
  --trust-remote-code \
  --port 54257 \
  --max-model-len 8000 \
  --gpu-memory-utilization 0.90 \
  --max-num-seqs 8 \
  --swap-space 8 \
  --enforce-eager
```

![](img/93761d5876f18925591e2b4ba70a1744.png)

部署就绪了，等待客户端发送请求。

客户端执行：

```
curl http://localhost:54257/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
        "model": "//root/Internlm/output/grpo-intern-s1-mini-gsm8k/v1-20251102-010146/checkpoint-167-merged",
        "messages": [
          {"role": "system", "content": "You are a helpful assistant."},
          {"role": "user", "content": "Weng earns $12 an hour for babysitting. Yesterday, she just did 50 minutes of babysitting. How much did she earn? Lets think step by step and output the final answer after \"####\"."}
        ]
      }'
```

发送后即可顺利得到推理的结果。

参考教程：[L2 书生大模型强化学习 RL 实践(`ms-swift`)](https://aicarrier.feishu.cn/wiki/HU1TwFFBVi8tcAkz3mocazqOnle)
