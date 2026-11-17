
## 第一阶段训练

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

## 第二步：Reward Model 训练

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
   --output_dir "./output" \
```

### 第二阶段训练日志

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

## 第三步：RLHF 训练

```
Actor_Lr=9.65e-6
Critic_Lr=5e-6

deepspeed --master_port 12346 main.py \
   --data_path Dahoas/rm-static \
   --data_split 2,4,4 \
   --actor_model_name_or_path 'output/actor-models/1.3b' \
   --critic_model_name_or_path 'output/reward-models/350m' \
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
   --output_dir 'output'
```

### 第三阶段训练日志

第三阶段训练日志过程的部分日志如下：

