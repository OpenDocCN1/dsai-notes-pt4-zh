
## GAE 优势估计

```python
def _estimate_advantage(self, rewards, values):
    if self.args.whiten_rewards:
        rewards = torch_ops.whiten(rewards, shift_mean=False)  # 奖励标准化（可选）

    lastgaelam = 0
    advantages_reversed = []
    gen_length = self.args.response_len  # 生成长度（如 128）

    # 从后往前计算 GAE（反向遍历 token）
    for t in reversed(range(gen_length)):
        nextvalues = values[:, t + 1] if t < gen_length - 1 else 0.0
        # TD error: δ_t = r_t + γ * V(s_{t+1}) - V(s_t)
        delta = rewards[:, t] + self.args.gamma * nextvalues - values[:, t]
        # GAE: A_t = δ_t + γλ A_{t+1}
        lastgaelam = delta + self.args.gamma * self.args.lam * lastgaelam
        advantages_reversed.append(lastgaelam)

    advantages = torch.stack(advantages_reversed[::-1], dim=1)  # 反转回正常顺序
    returns = advantages + values  # Q(s,a) ≈ A(s,a) + V(s)

    # 优势函数标准化（减均值、除标准差）→ 降低方差
    advantages = torch_ops.whiten(advantages, shift_mean=True)

    return dict(returns=returns, advantages=advantages)
```

## Rollout 采样轨迹

`rollout`（采样轨迹）：完成一次完整的采样 → 评估 → 奖励计算 → 优势估计流程，为后续 PPO 更新准备数据。

```python
@torch.inference_mode()
def rollout(self, queries_data):
    self.policy.eval()
    unwrapped_policy = self.accelerator.unwrap_model(self.policy, keep_fp32_wrapper=True)
    self.ref_policy.eval()
    self.reward_model.eval()

    rollouts = []
    for batch in tqdm.tqdm(queries_data, desc="rollout"):
        # 1. 从当前策略生成 responses
        queries, masks = batch['queries'], batch['query_attn_masks']
        responses = unwrapped_policy.respond(queries, masks, temperature=...)  # 生成

        # 2. 用当前策略计算 logprobs 和 values（critic 输出）
        policy_outputs = self.policy(queries, masks, responses, ...)  # forward

        # 3. 用参考策略（SFT 模型）计算 ref_logprobs（用于 KL）
        ref_outputs = self.ref_policy(queries, masks, responses, ...)

        # 4. 将 response 转为文本，再用 reward tokenizer 重新 tokenize
        #    （因为 policy 和 reward model 的 tokenizer 可能不同）
        text_queries = decode(queries); text_responses = decode(responses)
        text_sequences = [q + r for q, r in zip(text_queries, text_responses)]
        sequences = reward_tokenizer(text_sequences, ...)  # 重新 tokenize

        # 5. 用 reward model 计算任务奖励
        reward_outputs = self.reward_model(**sequences)
        reward_outputs = self.post_reward(reward_outputs, responses)  # 处理未正常结束的序列

        # 6. 奖励塑形：加入 KL 惩罚
        shaped = self._shape_reward(rewards=reward_outputs['rewards'], ...)

        # 7. 保存所有数据到 rollouts
        rollouts_batch.update(policy_outputs, ref_outputs, reward_outputs, shaped)
        rollouts.append(rollouts_batch.cpu())

    # 合并所有 batch
    rollouts = common.merge_dict(rollouts, merge_fn=torch.cat)

    # 8. 统一计算 GAE（用整个 rollout 数据集，更稳定）
    advantages = self._estimate_advantage(
        rewards=rollouts["shaped_rewards"].to(device),
        values=rollouts["values"].to(device),
    )

    return {**rollouts, **advantages}
```

## PPO 损失计算

PPO 损失计算：使用裁剪机制防止策略更新过大。

```python
def compute_loss(self, rollouts):
    # 提取旧策略数据（from rollout）
    values, old_logprob, returns, advantages, ... = rollouts

    # 用当前策略重新计算 logprobs 和 values
    outputs = self.policy(queries, masks, responses, ...)
    vpred = outputs["values"]      # 新的 value 预测
    logprob = outputs["logprobs"]  # 新的 log prob

    # --- Value Loss (Critic) ---
    # 裁剪 value 预测（类似 PPO 裁剪）
    vpredclipped = torch.clamp(vpred, values ± cliprange_value)
    vf_losses1 = (vpred - returns) ** 2
    vf_losses2 = (vpredclipped - returns) ** 2
    vf_loss = 0.5 * max(vf_losses1, vf_losses2).mean()  # PPO-style value loss

    # --- Policy Loss (Actor) ---
    ratio = exp(logprob - old_logprob)  # 新旧策略概率比
    pg_losses = -advantages * ratio
    pg_losses2 = -advantages * clip(ratio, 1-ε, 1+ε)
    pg_loss = max(pg_losses, pg_losses2).mean()  # PPO 裁剪目标

    # 总损失 = 策略损失 + vf_coef * 价值损失
    loss = pg_loss + self.args.vf_coef * vf_loss

    # 记录统计量
    approxkl = 0.5 * (logprob - old_logprob)^2 的均值
    entropy = outputs["entropies"].mean()

    return loss, stats
```

## PPO 训练曲线分析

在语言模型的 PPO 训练过程中，一个健康、合理的训练曲线是什么样？

![图14.3 ppo训练过程曲线](https://raw.githubusercontent.com/datawhalechina/diy-llm/main/docs/chapter14/images/14-3-ppo%E8%AE%AD%E7%BB%83%E8%BF%87%E7%A8%8B%E6%9B%B2%E7%BA%BF.png)

图14.3 ppo训练过程曲线

- Increasing overall rewards：总奖励上升。`kl_sum_seq` 这个名字有点误导性，它实际上代表的是每个序列的塑形奖励（shaped rewards）的总和，这个“塑形奖励” = 任务奖励（如数学题答对得高分） + KL 惩罚项（负值），衡量模型的整体表现是否在变好。
- Incl. reward model：任务奖励上升。这个指标衡量的是模型直接从奖励模型那里获得的任务奖励，不包括 KL 惩罚。它反映了模型在核心任务上的表现，比如回答问题的准确性、遵循指令的能力等。
- Negative KL rewards：负的 KL 奖励。这个指标就是 KL 惩罚项，也就是前面提到的 `-β * KL`。说明模型确实在探索和改进，但没有失控。这是 PPO 算法“近端”思想的体现——允许一定程度的偏离，但限制其幅度。

这三条曲线共同描绘了一个健康的 PPO 训练过程：模型在奖励模型的引导下，逐步学会生成更好的回复，同时通过 KL 惩罚保持一定的稳定性，避免过度偏离初始的良好行为。

### 14.2.2 为什么我们需要另一个强化学习（RL）算法？

#### 1. 为什么不直接用 PPO？

PPO 是目前最成功的在线强化学习算法之一，尤其在语言模型的对齐（Alignment）中被广泛应用。但它有两个主要缺点：

- 实现复杂 (complicated implementation)：PPO 不是一个简单的“开箱即用”的算法。它包含多个复杂的组件，如：`Rollout` 采样、奖励塑形、优势估计、损失计算等。这些步骤需要精心设计和调试，对于新手或追求快速迭代的研究者来说，门槛很高。
- 价值模型 (Value model) 的负担：PPO 需要一个额外的 `Value Model` 来估计状态的价值（`V(s)`），从而计算优势函数（`A = Q - V`）。
- 内存消耗大 (memory hungry)：价值网络与策略网络共享主干结构，但需要额外的参数和计算资源。
- 额外的调参 (additional tuning)：价值网络本身也需要训练和优化，这增加了整个系统的复杂性和超参数搜索空间。你需要同时调优策略网络和价值网络，确保它们协同工作。

> ✅ 总结：PPO 虽然强大且有效，但其工程复杂度高、资源消耗大、调参困难，尤其是在低资源或追求高效开发的场景下，显得不够轻便。

#### 1. 为什么不直接用 DPO？

DPO（Direct Preference Optimization）是近年来兴起的一种替代方案，它绕过了传统的强化学习框架，直接从人类偏好数据中进行优化。但其也存在以下限制：

- 数据形式不匹配 (Data not inherently pairwise)：DPO 的核心思想是基于成对比较（pairwise comparisons）数据，即给定一个提示（prompt），有两个不同的回复（response A 和 response B），并标注哪个更好。然而，在许多实际应用场景中，我们拥有的数据并非天然就是成对的。例如，在“可验证奖励”（Verifiable Rewards）领域，数据通常是单个序列加上一个客观的分数（如数学题答对得 1 分，答错得 0 分）。这种标量奖励信号无法直接用于 DPO。
- 离线算法 (Offline)：DPO 是一种离线算法。它在固定的、预先收集好的偏好数据集上进行一次性训练。这与 PPO 的在线学习特性不同。PPO 可以在训练过程中不断生成新的样本、获得新的反馈，并据此迭代更新策略。DPO “理论上可以通过迭代使其在线化”，但这会增加复杂性，失去了其作为简单离线方法的优势。

> ✅ 总结：DPO 在处理人类偏好数据时非常优雅和高效，但它不适用于非成对的、可验证的标量奖励场景，并且其离线特性限制了它在需要持续探索和在线学习任务中的应用。

### 14.2.3 GRPO：去掉了价值函数的 PPO

GRPO ([Group Relative Policy Optimization](https://arxiv.org/pdf/2402.03300)) 是在 [`DeepSeekMath`](https://arxiv.org/abs/2501.12948) 论文中提出并在 `Deepseek-R1` 中发扬光大的算法。GRPO 在 PPO 的基础上，移除了 `Value Function` 和 `Advantage Computation`。这是对 PPO 最大的改动，也是其轻量化的根本原因。并且采用了一种全新的方式来估算“优势”——即 `z-score`（z-score within group）。

![图14.4 PPO 与 GRPO 的对比](https://raw.githubusercontent.com/datawhalechina/diy-llm/main/docs/chapter14/images/14-4-ppo%E4%B8%8Egrpo%E7%9A%84%E5%AF%B9%E6%AF%94.png)

图14.4 PPO 与 GRPO 的对比

> ✅ 简单来说，GRPO = PPO - `Value Model` + Group Z-Score Advantage。

#### [GRPO 的目标函数](https://datawhalechina.github.io/diy-llm/#/./chapter14/chapter14_%E5%8F%AF%E9%AA%8C%E8%AF%81%E5%A5%96%E5%8A%B1%E7%9A%84%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0?id=grpo-%e7%9a%84%e7%9b%ae%e6%a0%87%e5%87%bd%e6%95%b0)

```
JGRPO(θ) = E[q∼P(Q), {oi}i=1G∼πθold(O∣q)] (1/G) ∑i=1G (min(πθ(oi∣q)/πθold(oi∣q) * Ai, clip(πθ(oi∣q)/πθold(oi∣q), 1−ϵ, 1+ϵ) * Ai) − β * DKL(πθ ∥ πref))
```

- `min(...)` 部分：是 PPO 的经典裁剪目标函数，用于更新策略 `πθ`。
- `πθ(oi∣q)/πθold(oi∣q)` 是新旧策略的概率比。
- `Ai` 是第 `i` 个输出 `oi` 的“优势”，这是 GRPO 最大的创新点。
- `clip(...)` 是 PPO 的裁剪机制，防止策略更新过大。
- `-β * DKL(πθ ∥ πref)` 部分：这是 KL 散度惩罚项，用于防止新策略 `πθ` 偏离参考策略 `πref` 太远，保证生成结果的稳定性。
- `β` 是控制 KL 惩罚强度的超参数。

PPO 的目标函数：

```
min(πθ(a∣s)/πθk(a∣s) * A, clip(πθ(a∣s)/πθk(a∣s), 1−ϵ, 1+ϵ) * A)
```

GRPO 与 PPO 两者的目标函数结构非常相似，都包含概率比和裁剪。核心区别在于 `A` 的来源：

- PPO: `A` 是通过 `V(s)` 和 `GAE` 计算出来的，这是一个复杂且资源密集的过程。
- GRPO: `A` 是通过组内 `z-score` 计算出来的，这是一个简单、快速、无需额外模型的过程。
