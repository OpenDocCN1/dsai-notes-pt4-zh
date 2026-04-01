# LLM（十）：从 ColossalChat 到 DeepSpeedChat, RLHF的实现及优化

> 来源：[https://agijuejin.feishu.cn/docx/B2OPdzNJxo5fe2xqTZ0cd6vGnvb](https://agijuejin.feishu.cn/docx/B2OPdzNJxo5fe2xqTZ0cd6vGnvb)

> 作者：紫气东来

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/621391363)hihu.com/p/621391...

## 一、深入理解 ColossalChat

在上一期

[紫](https://zhuanlan.zhihu.com/p/618695885)气东来：NLP（九）：LLaMA, Alpaca, ColossalChat 系列模型研究 238 赞同 · 26 评论 文章

![](img/3da209a9ad8d6701231fe536ddd8b653.png)

的最后部分简要介绍了ColossalChat 的训练过程。下面将逐步解析训练的3个过程。

### 1.1 Stage 1：Supervised instructs tuning

该阶段训练采用 InstructionWild 数据集中的 "instinwild_en.json" ，总数据量为52K，其数据格式如下所示：

```
{
    &quot;instruction&quot;: &quot;Provide a list of the top 10 most popular mobile games in Asia&quot;,
    &quot;input&quot;: &quot;&quot;,
    &quot;output&quot;: &quot;The top 10 most popular mobile games in Asia are:\n1) PUBG Mobile\n2) Pokemon Go\n3) Candy Crush Saga\n4) Free Fire\n5) Clash of Clans\n6) Mario Kart Tour\n7) Arena of Valor\n8) Fantasy Westward Journey\n9) Subway Surfers\n10) ARK Survival Evolved&quot;,
    &quot;id&quot;: 0
}
```

SFT 的训练逻辑如下图所示，即是典型的 Instruction 的做法 ：

![](img/7a054e8625c1df293250d4e22947f99f.png)

为节约GPU资源，使用 GPT2-XL 模型训练，命令如下：

```
torchrun --standalone --nproc_per_node=8 train_sft.py \
    --pretrain &quot;gpt2-xl&quot; \
    --model &#x27;gpt2&#x27; \
    --strategy colossalai_zero2 \
    --log_interval 10 \
    --save_path  &#x27;./trained_sft_gpt2-xl&#x27; \
    --dataset &#x27;dataset/instinwild_en.json&#x27; \
    --batch_size 2 \
    --accimulation_steps 8 \
    --lr 2e-5 \
    --max_datasets_size 51200 \
    --max_epochs 2 
```

训练过程的 lr 和 loss 的记录如下图所示：

![](img/009420336fca102cc9bbe0fce501d0cf.png)

### 1.2 Stage 2：Training reward model

RM 模型训练使用 Anthropic/hh-rlhf 数据集，训练集数据总量为161K，其数据样本如下所示：

| chosen (string) | rejected (string) |
| --- | --- |
| &quot; Human: What kind of noises did dinosaurs make? Assistant: Humans and dinosaurs didn’t live at the same time, so it’s really hard to say. The best place to find out what noises dinosaurs made would be Human: yes they did Assistant: to guess, and that would probably require lots of reading and a certain amount of imagination, so we’re not really prepared to do that. Human: you cant read Assistant: You can read?&quot; | &quot; Human: What kind of noises did dinosaurs make? Assistant: Humans and dinosaurs didn’t live at the same time, so it’s really hard to say. The best place to find out what noises dinosaurs made would be Human: yes they did Assistant: to guess, and that would probably require lots of reading and a certain amount of imagination, so we’re not really prepared to do that. Human: you cant read Assistant: there’s a lot of stuff humans don’t know&quot; |

RM 的训练逻辑如下图所示:

![](img/5d8235546535c09a755aa342f87c987f.png)

训练的命令如下:

```
torchrun --standalone --nproc_per_node=8 train_reward_model.py \
   --pretrain  &#x27;gpt2-xl&#x27; \
   --model &#x27;gpt2&#x27; \
   --strategy colossalai_zero2 \
   --loss_fn &#x27;log_sig&#x27;\
   --save_path &#x27;trained_rm_gpt2-xl.pt&#x27;\
   --dataset &#x27;Anthropic/hh-rlhf&#x27;\
```

在训练过程中，除记录loss外，还需要记录在验证集上的 dist(chosen_reward-reject_reward 的均值) 和 acc ，结果如下所示：

![](img/8b2e69f42c9c14f48f07dbac6c81db7b.png)

### 1.3 Stage 3：Training model with reinforcement learning by human feedback

Stage3 是通过阶段2训练出来的奖励函数微调出一个RL模型，微调过程中通过PPO算法限制RL模型的参数更新范围(以阶段1的SFT模型的策略为参考基准，PPO算法避免与基线模型SFT的策略偏离过远)。其整体过程如下图所示，该过程又可简单分为2个部分。

![](img/a3643e157620c3670a3fe170e6d937bd.png)

+   部分1: Make Experience ，利用 SFT 、Actor、RM、Critic模型计算生成 Experience 存入 buffer 中，该部分所有模型都进行前向推理。

![](img/40df2bc131f14816c2cf8c1f3e620222.png)

+   部分2: 利用 Experience 计算价值损失(value loss)和策略损失(policy loss)，并更新参数。

![](img/961db2449052137929c028b7db6888e0.png)

这里需要说明的是价值损失(value loss)和策略损失(policy loss) 为什么不同：

+   价值损失(value loss) 采用的均方误差，这个目的是为了使判断更加准确，不偏离方向；

+   策略损失(policy loss) reward与KL的调和值，其目的是为了获取更大的回报。

使用前2阶段训练得到的SFT和RM模型，第3阶段的完整训练如下所示：

```
torchrun --standalone --nproc_per_node=8 train_prompts.py \
         --pretrain &quot;trained_sft_gpt2-xl&quot; \
         --model &#x27;gpt2&#x27; \
         --strategy colossalai_zero2 \
         --prompt_path &quot;dataset/seed_prompts_en.jsonl&quot; \
         --pretrain_dataset &#x27;dataset/instinwild_en.json&#x27; \
         --rm_model &#x27;gpt2&#x27; \
         --rm_pretrain &quot;gpt2-xl&quot; \
         --rm_path &#x27;trained_rm_gpt2-xl.pt&#x27; \
         --train_batch_size 1 \
         --experience_batch_size 1 \
         --num_episodes 20 \
         --max_epochs 20
```

## 二、体验 DeepSpeedChat

从训练过程上来说，DeepSpeedChat 与 ColossalChat 没有明显，都是过程一致的3个阶段，主要是实现了一些工程上的优化。

![](img/308a936aa5b3581d3992b73d7a920750.png)

为便于与ColossalChat 比较采用同等规模模型，3个阶段可通过一个命令运行

```
python3 train.py --actor-model facebook/opt-1.3b --reward-model facebook/opt-350m --deployment-type single_node
```

其中3个阶段实际上仍然被分解为3步执行：

+   第一步：SFT 训练

```
deepspeed main.py \
   --data_path Dahoas/rm-static Dahoas/full-hh-rlhf Dahoas/synthetic-instruct-gptj-pairwise yitingxie/rlhf-reward-datasets \
   --data_split 2,4,4 \
   --model_name_or_path facebook/opt-1.3b \
   --per_device_train_batch_size 8 \
   --per_device_eval_batch_size 8 \
   --max_seq_len 512 \
   --learning_rate 9.65e-6 \
   --weight_decay 0. \
   --num_train_epochs 16 \
   --gradient_accumulation_steps 1 \
   --lr_scheduler_type cosine \
   --num_warmup_steps 0 \
   --seed 1234 \
   --zero_stage 2 \
   --deepspeed \
   --output_dir &quot;./output&quot; \
```

第一阶段训练日志过程的部分日志如下：

```
***** Running training *****
***** Evaluating perplexity, Epoch 0/16 *****
ppl: 4937.2431640625
Beginning of Epoch 1/16, Total Micro Batches 920
[2023-05-01 17:08:55,881] [INFO] [logging.py:96:log_dist] [Rank 0] step=10, skipped=6, lr=[9.649998241787337e-06, 9.649998241787337e-06], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-01 17:08:56,078] [INFO] [timer.py:199:stop] epoch=0/micro_step=10/global_step=10, RunningAvgSamplesPerSec=69.28514057779762, CurrSamplesPerSec=62.312571874000376, MemAllocated=4.98GB, MaxMemAllocated=23.97GB
[2023-05-01 17:09:06,144] [INFO] [logging.py:96:log_dist] [Rank 0] step=20, skipped=6, lr=[9.649978461909591e-06, 9.649978461909591e-06], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-01 17:09:06,339] [INFO] [timer.py:199:stop] epoch=0/micro_step=20/global_step=20, RunningAvgSamplesPerSec=65.28289507772782, CurrSamplesPerSec=62.544054882381914, MemAllocated=4.98GB, MaxMemAllocated=23.97GB
...
***** Evaluating perplexity, Epoch 1/16 *****
ppl: 2.0012214183807373
Beginning of Epoch 2/16, Total Micro Batches 920
[2023-05-01 17:24:53,358] [INFO] [logging.py:96:log_dist] [Rank 0] step=930, skipped=13, lr=[9.55789070120902e-06, 9.55789070120902e-06], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-01 17:24:53,558] [INFO] [timer.py:199:stop] epoch=1/micro_step=10/global_step=930, RunningAvgSamplesPerSec=62.532406953883935, CurrSamplesPerSec=62.11747630869141, MemAllocated=4.98GB, MaxMemAllocated=23.97GB
[2023-05-01 17:25:03,622] [INFO] [logging.py:96:log_dist] [Rank 0] step=940, skipped=13, lr=[9.555877413047903e-06, 9.555877413047903e-06], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-01 17:25:03,819] [INFO] [timer.py:199:stop] epoch=1/micro_step=20/global_step=940, RunningAvgSamplesPerSec=62.53102982115688, CurrSamplesPerSec=62.38918457600946, MemAllocated=4.98GB, MaxMemAllocated=23.97GB
...
***** Evaluating perplexity, Epoch 15/16 *****
ppl: 1.7830698490142822
Beginning of Epoch 16/16, Total Micro Batches 920
[2023-05-01 21:07:37,315] [INFO] [logging.py:96:log_dist] [Rank 0] step=13810, skipped=265, lr=[1.5092112560532933e-07, 1.5092112560532933e-07], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-01 21:07:37,514] [INFO] [timer.py:199:stop] epoch=15/micro_step=10/global_step=13810, RunningAvgSamplesPerSec=62.66213404288616, CurrSamplesPerSec=62.30927408141939, MemAllocated=4.98GB, MaxMemAllocated=23.97GB
[2023-05-01 21:07:47,568] [INFO] [logging.py:96:log_dist] [Rank 0] step=13820, skipped=265, lr=[1.4837637890662103e-07, 1.4837637890662103e-07], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-01 21:07:47,765] [INFO] [timer.py:199:stop] epoch=15/micro_step=20/global_step=13820, RunningAvgSamplesPerSec=62.66198725001066, CurrSamplesPerSec=62.56049691270701, MemAllocated=4.98GB, MaxMemAllocated=23.97GB
...
***** Evaluating perplexity, Epoch 16/16 *****
ppl: 1.780816674232483
saving the final model ...
```

+   第二步：Reward Model 训练

```
deepspeed main.py \
   --data_path Dahoas/rm-static Dahoas/full-hh-rlhf Dahoas/synthetic-instruct-gptj-pairwise yitingxie/rlhf-reward-datasets \
   --data_split 2,4,4 \
   --model_name_or_path facebook/opt-350m \
   --num_padding_at_beginning 1 \
   --per_device_train_batch_size 4 \
   --per_device_eval_batch_size 4 \
   --max_seq_len 512 \
   --learning_rate 5e-5 \
   --weight_decay 0.1 \
   --num_train_epochs 1 \
   --disable_dropout \
   --gradient_accumulation_steps 1 \
   --lr_scheduler_type cosine \
   --num_warmup_steps 0 \
   --seed 1234 \
   --zero_stage 0 \
   --deepspeed \
   --output_dir &quot;./output&quot; \
```

第二阶段训练日志过程的部分日志如下：

```
***** Running training *****
***** Evaluating reward, Epoch 0/1 *****
chosen_last_scores (higher is better) : 2.8095741271972656, acc (higher is better) : 0.4898989498615265
Beginning of Epoch 1/1, Total Micro Batches 3680
...
[2023-05-02 17:21:25,830] [INFO] [logging.py:96:log_dist] [Rank 0] step=10, skipped=7, lr=[4.999991801084829e-05, 4.999991801084829e-05], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-02 17:21:25,849] [INFO] [timer.py:199:stop] epoch=0/micro_step=10/global_step=10, RunningAvgSamplesPerSec=100.85116912167754, CurrSamplesPerSec=92.62450760911271, MemAllocated=4.32GB, MaxMemAllocated=12.79GB
[2023-05-02 17:21:29,272] [INFO] [logging.py:96:log_dist] [Rank 0] step=20, skipped=7, lr=[4.999846044088921e-05, 4.999846044088921e-05], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-02 17:21:29,291] [INFO] [timer.py:199:stop] epoch=0/micro_step=20/global_step=20, RunningAvgSamplesPerSec=96.39672859406251, CurrSamplesPerSec=93.42591718628839, MemAllocated=4.32GB, MaxMemAllocated=12.79GB
[2023-05-02 17:21:32,713] [INFO] [logging.py:96:log_dist] [Rank 0] step=30, skipped=7, lr=[4.9995181012051625e-05, 4.9995181012051625e-05], mom=[(0.9, 0.95), (0.9, 0.95)]
...
[2023-05-02 17:28:40,660] [INFO] [logging.py:96:log_dist] [Rank 0] step=1270, skipped=17, lr=[3.701016326089881e-05, 3.701016326089881e-05], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-02 17:28:40,679] [INFO] [timer.py:199:stop] epoch=0/micro_step=1270/global_step=1270, RunningAvgSamplesPerSec=92.90538668585853, CurrSamplesPerSec=92.88912634851383, MemAllocated=4.32GB, MaxMemAllocated=12.79GB
[2023-05-02 17:28:44,120] [INFO] [logging.py:96:log_dist] [Rank 0] step=1280, skipped=17, lr=[3.682254575425273e-05, 3.682254575425273e-05], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-02 17:28:44,139] [INFO] [timer.py:199:stop] epoch=0/micro_step=1280/global_step=1280, RunningAvgSamplesPerSec=92.90310674484337, CurrSamplesPerSec=92.82032648798923, MemAllocated=4.32GB, MaxMemAllocated=12.79GB
...
[2023-05-02 17:42:28,021] [INFO] [logging.py:96:log_dist] [Rank 0] step=3660, skipped=29, lr=[2.1869706348343066e-08, 2.1869706348343066e-08], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-02 17:42:28,040] [INFO] [timer.py:199:stop] epoch=0/micro_step=3660/global_step=3660, RunningAvgSamplesPerSec=92.68522051842969, CurrSamplesPerSec=93.28319595223864, MemAllocated=4.32GB, MaxMemAllocated=12.79GB
[2023-05-02 17:42:31,478] [INFO] [logging.py:96:log_dist] [Rank 0] step=3670, skipped=29, lr=[1.385489430420217e-08, 1.385489430420217e-08], mom=[(0.9, 0.95), (0.9, 0.95)]
[2023-05-02 17:42:31,497] [INFO] [timer.py:199:stop] epoch=0/micro_step=3670/global_step=3670, RunningAvgSamplesPerSec=92.68524203482882, CurrSamplesPerSec=93.03399659243877, MemAllocated=4.32GB, MaxMemAllocated=12.79GB
...
Epoch 1/1 with loss inf
***** Evaluating reward, Epoch 1/1 *****
chosen_last_scores (higher is better) : -0.4733814597129822, acc (higher is better) : 0.6717171669006348
saving model ...
```

+   第三步：RLHF 训练

```
Actor_Lr=9.65e-6
Critic_Lr=5e-6

deepspeed --master_port 12346 main.py \
   --data_path Dahoas/rm-static \
   --data_split 2,4,4 \
   --actor_model_name_or_path &#x27;output/actor-models/1.3b&#x27; \
   --critic_model_name_or_path &#x27;output/reward-models/350m&#x27; \
   --num_padding_at_beginning 1 \
   --per_device_train_batch_size 4 \
   --per_device_mini_train_batch_size 4 \
   --generation_batch_numbers 1 \
   --ppo_epochs 1 \
   --max_answer_seq_len 256 \
   --max_prompt_seq_len 256 \
   --actor_learning_rate ${Actor_Lr} \
   --critic_learning_rate ${Critic_Lr} \
   --num_train_epochs 1 \
   --lr_scheduler_type cosine \
   --gradient_accumulation_steps 1 \
   --disable_actor_dropout \
   --num_warmup_steps 100 \
   --deepspeed --seed 1234 \
   --enable_hybrid_engine \
   --actor_zero_stage 2 \
   --critic_zero_stage 2 \
   --enable_ema \
   --output_dir &#x27;output&#x27;
```

第三阶段训练日志过程的部分日志如下：

```
。。.************************[start] Initializing Actor Model [start] *************************
huggingface/tokenizers: The current process just got forked, after parallelism has already been used. Disabling parallelism to avoid deadlocks...
To disable this warning, you can either:
        - Avoid using `tokenizers` before the fork if possible
        - Explicitly set the environment variable TOKENIZERS_PARALLELISM=(true | false)
...
*******************[end] Initialized Ref Model [end] (duration: 23.45s)*******************
*************************[start] Initializing EMA Model [start] **************************
Using /root/.cache/torch_extensions/py38_cu117 as PyTorch extensions root...
No modifications detected for re-loaded extension module utils, skipping build step...
Loading extension module utils...
Time to load utils op: 0.0008325576782226562 seconds
...
*******************[end] Initialized EMA Model [end] (duration: 20.59s)*******************
************************[start] Initializing Critic Model [start] ************************
Using /root/.cache/torch_extensions/py38_cu117 as PyTorch extensions root...
No modifications detected for re-loaded extension module utils, skipping build step...
Loading extension module utils...
Time to load utils op: 0.0007164478302001953 seconds
...
*****************[end] Initialized Reward Model [end] (duration: 14.60s)******************
***** Running training *****
Beginning of Epoch 1/1, Total Generation Batches 954
epoch: 0|step: 0|ppo_ep: 1|act_loss: 0.039031982421875|cri_loss: 0.0031604766845703125|unsuper_loss: 0.0
average reward score: -2.208984375
-------------------------------------------------------------------------------------
|E2E latency=4.19s |Gather latency=0.00s (0.00%) |Generate time=2.82s (67.47%) |Training time=1.13s (27.03%) |Others=0.23 (5.50%)|CurSamplesPerSec=7.65 |AvgSamplesPerSec=7.65
[2023-05-02 17:46:36,487] [INFO] [loss_scaler.py:181:update_scale] [deepspeed] OVERFLOW! Rank 0 Skipping step. Attempted loss scale: 65536, reducing to 32768
[2023-05-02 17:46:36,695] [INFO] [loss_scaler.py:181:update_scale] [deepspeed] OVERFLOW! Rank 0 Skipping step. Attempted loss scale: 65536, reducing to 32768
epoch: 0|step: 1|ppo_ep: 1|act_loss: 0.041168212890625|cri_loss: 0.036041259765625|unsuper_loss: 0.0
average reward score: -2.3125
-------------------------------------------------------------------------------------
...
-------------------------------------------------------------------------------------
|E2E latency=2.90s |Gather latency=0.00s (0.00%) |Generate time=1.56s (53.92%) |Training time=1.04s (36.09%) |Others=0.29 (10.00%)|CurSamplesPerSec=11.05 |AvgSamplesPerSec=11.06
epoch: 0|step: 36|ppo_ep: 1|act_loss: 0.2275390625|cri_loss: 0.0435791015625|unsuper_loss: 0.0
average reward score: -1.783203125
-------------------------------------------------------------------------------------
|E2E latency=2.89s |Gather latency=0.00s (0.00%) |Generate time=1.56s (53.98%) |Training time=1.04s (35.96%) |Others=0.29 (10.06%)|CurSamplesPerSec=11.06 |AvgSamplesPerSec=11.06
epoch: 0|step: 37|ppo_ep: 1|act_loss: -0.06414794921875|cri_loss: 0.0183868408203125|unsuper_loss: 0.0
average reward score: -2.1171875
-------------------------------------------------------------------------------------
|E2E latency=2.89s |Gather latency=0.00s (0.00%) |Generate time=1.56s (54.05%) |Training time=1.04s (35.97%) |Others=0.29 (9.99%)|CurSamplesPerSec=11.08 |AvgSamplesPerSec=11.06
epoch: 0|step: 38|ppo_ep: 1|act_loss: -0.203857421875|cri_loss: 0.043121337890625|unsuper_loss: 0.0
average reward score: -1.763671875
...
-------------------------------------------------------------------------------------
|E2E latency=2.89s |Gather latency=0.00s (0.00%) |Generate time=1.56s (54.05%) |Training time=1.04s (35.92%) |Others=0.29 (10.04%)|CurSamplesPerSec=11.07 |AvgSamplesPerSec=11.07
epoch: 0|step: 951|ppo_ep: 1|act_loss: 0.041351318359375|cri_loss: 0.07244873046875|unsuper_loss: 0.0
average reward score: -1.9130859375
-------------------------------------------------------------------------------------
|E2E latency=2.89s |Gather latency=0.00s (0.00%) |Generate time=1.56s (54.11%) |Training time=1.03s (35.84%) |Others=0.29 (10.05%)|CurSamplesPerSec=11.09 |AvgSamplesPerSec=11.07
epoch: 0|step: 952|ppo_ep: 1|act_loss: -0.287109375|cri_loss: 0.333251953125|unsuper_loss: 0.0
average reward score: -1.8779296875
-------------------------------------------------------------------------------------
|E2E latency=2.89s |Gather latency=0.00s (0.00%) |Generate time=1.56s (54.06%) |Training time=1.04s (35.88%) |Others=0.29 (10.06%)|CurSamplesPerSec=11.08 |AvgSamplesPerSec=11.07
[2023-05-02 18:32:26,992] [INFO] [loss_scaler.py:181:update_scale] [deepspeed] OVERFLOW! Rank 0 Skipping step. Attempted loss scale: 8192, reducing to 4096
epoch: 0|step: 953|ppo_ep: 1|act_loss: -0.48193359375|cri_loss: 1.1884765625|unsuper_loss: 0.0
average reward score: -2.115234375
-------------------------------------------------------------------------------------
saving model ...
```

## 参考资料

[1] [h](https://link.zhihu.com/?target=https%3A//github.com/hpcaitech/ColossalAI/tree/main/applications/Chat)ttps:// github.com/hpcaitech/Co lossalAI/tree/main/applications/Chat

[2] [h](https://link.zhihu.com/?target=https%3A//blog.csdn.net/v_JULY_v/article/details/129996493)ttps:// blog.csdn.net/v_JULY_v/ article/details/129996493

[3] [ht](https://link.zhihu.com/?target=https%3A//jonathan-hui.medium.com/rl-proximal-policy-optimization-ppo-explained-77f014ec3f12)tps:// jonathan-hui.medium.com /rl-proximal-policy-optimization-ppo-explained-77f014ec3f12

[4] Proximal Policy Optimization (PPO)

[5] [h](https://link.zhihu.com/?target=https%3A//colossalai.org/docs/features/zero_with_chunk/)ttps:// colossalai.org/docs/fea tures/zero_with_chunk/

[6] [De](https://link.zhihu.com/?target=https%3A//github.com/microsoft/DeepSpeedExamples/tree/master/applications/DeepSpeed-Chat)epSpeedExamples/applications/DeepSpeed-Chat at master · microsoft/DeepSpeedExamples (github.com)

> 无人会得东风意，春色都将付海棠。 —— 杨万里《海棠坞》