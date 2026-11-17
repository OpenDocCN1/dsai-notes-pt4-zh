
# 大模型微调与优化指南

```json
{
    "object": "fine_tuning.job.event",
    "id": "ftevent-abc-123",
    "created_at": 1693582679,
    "level": "info",
    "message": "Step 300/300: training loss=0.15, validation loss=0.27, full validation loss=0.40",
    "data": {
        "step": 300,
        "train_loss": 0.1499,
        "valid_loss": 0.2657,
        "full_valid_loss": 0.4032,
        "train_mean_token_accuracy": 0.9444,
        "valid_mean_token_accuracy": 0.9565,
        "full_valid_mean_token_accuracy": 0.9089
    },
    "type": "metrics"
}
```

## 1. 指标解读

📌 如何解读？

+   训练损失（`train_loss`）应随训练过程下降。

+   验证损失（`valid_loss`）也应下降，但过低可能表示过拟合。

+   token 准确率（`token accuracy`）应上升，但并非越高越好，因任务不同而异。

---

## 2. 数据质量优化

如果微调效果不如预期，考虑优化数据集：

📌 改进策略

1. 收集针对性示例
   - 发现模型表现不佳的情况，并提供正确示例训练。

2. 检查数据一致性
   - 训练数据中的风格、语气、格式是否保持一致？

3. 平衡数据
   - 避免数据集中某些模式过多或过少。
   - 例如，如果训练集中 60% 的回答是 `我无法回答`，但实际场景只需要 5%，那么模型可能会拒绝过多请求。

📌 示例：如果模型错误地生成 `我可以帮你安排会议`，但它实际上不具备该功能，你可以：

+   检查训练数据，是否提供了错误的示范。
+   修正数据，确保模型不会产生误导性的回答。

---

## 3. 数据量优化

+   如果结果不理想，可以扩大训练数据集。

+   建议先训练完整数据集，再用一半数据训练，观察差异。

📌 如果数据有限，质量 > 数量

+   高质量小数据集比低质量大数据集更有效。

---

## 4. 超参数优化

可调整以下超参数（`hyperparameters`）：

+   训练轮次（`epochs`）

+   学习率倍率（`learning rate multiplier`）

+   批量大小（`batch size`）

📌 自动优化

+   默认情况下，OpenAI 会自动设置最佳超参数。

+   你可以在观察结果后调整。

---

### 4.1 设置超参数

```python
from openai import OpenAI
client = OpenAI()

client.fine_tuning.jobs.create(
    training_file="file-abc123",
    model="gpt-4o-mini-2024-07-18",
    method={
        "type": "supervised",
        "supervised": {
            "hyperparameters": {"n_epochs": 2},
        },
    },
)
```

📌 调整建议

+   如果模型没有很好地学习任务，增加 `epochs`。

+   如果模型生成内容过于单一，减少 `epochs`。

+   如果模型收敛较慢，增大学习率（`learning rate multiplier`）。

---

## 5. 视觉微调（Vision Fine-tuning）

你可以在 `JSONL` 训练数据中包含图片，以微调视觉模型。

📌 示例：训练模型识别奶酪种类

`"url": "https://upload.wikimedia.org/wikipedia/commons/3/36/Danbo_Cheese.jpg"`

```json
{
  "messages": [
    { "role": "system", "content": "你是一个能够识别奶酪种类的助手。" },
    { "role": "user", "content": "这是什么奶酪？" },
    { "role": "user", "content": [
        {
          "type": "image_url",
          "image_url": {
          }
        }
      ]
    },
    { "role": "assistant", "content": "丹麦 Danbo 奶酪" }
  ]
}
```

📌 视觉数据要求

+   最多 50,000 个包含图片的示例。

+   每个示例最多 10 张图片。

+   图片最大 10MB。

+   支持格式：JPEG, PNG, WEBP。

📌 限制

+   禁止用于面部识别、儿童照片、CAPTCHA 解析等。

---

## 6. 微调高级功能：检查点、DPO 训练、Weights & Biases 集成

### 6.1 使用微调检查点（Checkpointed Models）

在微调任务的每个 `训练轮次（epoch）` 结束后，OpenAI 自动创建完整的模型检查点（`checkpoint`）。这些检查点是完整的模型，可以直接用于推理。

📌 检查点的作用

+   防止过拟合（`Overfitting`）：使用早期的检查点版本可能能获得更好的泛化能力。

+   训练过程中的回溯点：如果最终模型效果不佳，可以从某个检查点继续训练，而不必从零开始。

📌 如何访问检查点

```python
from openai import OpenAI
client = OpenAI()

# 获取微调任务的所有检查点
checkpoints = client.fine_tuning.jobs.checkpoints.list("ftjob-abc123")

# 输出检查点信息
for checkpoint in checkpoints:
    print(checkpoint["fine_tuned_model_checkpoint"])
```

📌 示例 API 响应

```json
{
    "object": "fine_tuning.job.checkpoint",
    "id": "ftckpt_zc4Q7MP6XxulcVzj4MZdwsAB",
    "created_at": 1519129973,
    "fine_tuned_model_checkpoint": "ft:gpt-3.5-turbo-0125:my-org:custom-suffix:96olL566:ckpt-step-2000",
    "metrics": {
        "full_valid_loss": 0.134,
        "full_valid_mean_token_accuracy": 0.874
    },
    "fine_tuning_job_id": "ftjob-abc123",
    "step_number": 2000
}
```

📌 解析

+   `step_number`: 训练到第 2000 步时创建的检查点。

+   `metrics`: 该检查点的验证损失（`valid loss`）和 `token` 准确率（`token accuracy`）。

📌 当前限制

+   仅保留最近 3 个训练轮次（`epochs`）的检查点。

+   未来将提供更灵活的检查点策略。

---

### 6.2 直接偏好优化（DPO）微调

`DPO`（`Direct Preference Optimization`）允许你基于人类偏好来微调模型，使其生成更符合用户期望的输出。

📌 DPO 适用场景

+   你希望优化模型对不同回答的偏好（如客户服务、对话风格）。

+   你有一组数据，其中包含多个可能的回答，并希望微调模型学习最佳回答。

---

#### 6.2.1 准备 DPO 数据集

DPO 训练集需要包含：

1. 用户输入（`prompt`）

2. 优选回答（`preferred output`）

3. 次选回答（`non-preferred output`）

📌 数据格式（`JSONL` 文件）

```json
{
  "input": {
    "messages": [
      {
        "role": "user",
        "content": "Hello, can you tell me how cold San Francisco is today?"
      }
    ],
    "tools": [],
    "parallel_tool_calls": true
  },
  "preferred_output": [
    {
      "role": "assistant",
      "content": "Today in San Francisco, it is not quite cold as expected. Morning clouds will give way to sunshine, with a high near 68°F (20°C) and a low around 57°F (14°C)."
    }
  ],
  "non_preferred_output": [
    {
      "role": "assistant",
      "content": "It is not particularly cold in San Francisco today."
    }
  ]
}
```

📌 重要说明

+   仅支持单轮对话（`one-turn conversations`）。

+   优选答案和次选答案必须是最后一条 `assistant` 消息。

---

#### 6.2.2 配置 DPO 训练任务

```python
from openai import OpenAI
client = OpenAI()

job = client.fine_tuning.jobs.create(
    training_file="file-all-about-the-weather",
    model="gpt-4o-2024-08-06",
    method={
        "type": "dpo",
        "dpo": {
            "hyperparameters": {"beta": 0.1},
        },
    },
)
```

📌 参数说明

`beta`

+   控制 DPO 对新偏好的适应度：

+   低值（<0.5）→ 更倾向于学习新偏好。

+   高值（>1.5）→ 更保守，倾向于保持原有行为。

+   默认 `auto` → 由 OpenAI 自动调整。

---

### 6.3 Weights & Biases（W&B）集成

你可以使用 `Weights & Biases（W&B）` 追踪微调过程中的超参数、损失变化和性能指标。

📌 `W&B` 集成的作用

+   自动记录超参数、训练损失、验证损失等信息。

+   提供可视化界面，帮助你分析微调效果。

+   支持多个微调任务的对比分析。

---

#### 6.3.1 配置 W&B 集成

在创建微调任务时，添加 `wandb` 集成：

```bash
curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer $OPENAI_API_KEY" \
    -d '{
    "model": "gpt-4o-mini-2024-07-18",
    "training_file": "file-ABC123",
    "validation_file": "file-DEF456",
    "integrations": [
        {
            "type": "wandb",
            "wandb": {
                "project": "custom-wandb-project",
                "tags": ["project:tag", "lineage"]
            }
        }
    ]
}' https://api.openai.com/v1/fine_tuning/jobs
```

📌 参数说明

+   `"project"`: 在 `W&B` 上创建的项目名称。

+   `"tags"`: 添加自定义标签，方便搜索任务。

---

#### 6.3.2 访问 W&B 记录

创建微调任务后，你可以在 `Weights & Biases` 的仪表盘中查看微调记录：

`https://wandb.ai/<WANDB-ENTITY>/<WANDB-PROJECT>/runs/ftjob-ABCDEF`

📌 `W&B` 提供的可视化数据

+   训练损失 & 验证损失

+   训练 token 准确率

+   微调超参数（`epochs`, `learning_rate`, `batch_size`）

---

## 7. 微调 vs 召回增强（`RAG`）

何时使用微调？何时使用嵌入 + 召回增强生成（`RAG`）？

📌 对比

| 特性 | 微调（`Fine-tuning`） | RAG（嵌入+`RAG`） |
| --- | --- | --- |
| 适用场景 | 需要固定风格、严格格式的输出 | 需要动态访问外部知识库 |
| 模型更新 | 训练后固定 | 无需训练，随时更新知识库 |
| 数据依赖 | 需要大量高质量训练数据 | 只需要高质量的知识库 |
| 计算开销 | 训练成本高 | 依赖向量数据库，推理成本低 |

📌 推荐

+   如果任务需要灵活获取最新知识 → 使用 `RAG`

+   如果任务要求严格格式和固定风格 → 使用微调

---

# 构建高效、可靠的智能代理（Agents）

+   地址：`https://platform.openai.com/docs/guides/agents`

![](img/bedd870a6ada1fb80ac32e202c355578.png)

---

## 1. 代理（Agents）概述

`Agents`（智能代理）是一种能够智能完成任务的系统，可以执行简单工作流或追求复杂、开放式目标。

📌 OpenAI 提供的 `Agents SDK`：

+   轻量级且强大的编排框架，用于构建、部署和优化智能代理。

+   支持内置可观测性（`Observability`），可以跟踪和优化代理性能。
