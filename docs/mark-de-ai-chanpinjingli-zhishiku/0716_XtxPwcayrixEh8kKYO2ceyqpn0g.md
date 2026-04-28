# 第一章：工具使用

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/JebQdyPh3oKd0LxnsYRcUOkqnDf](https://qqs7y1hozd1.feishu.cn/docx/JebQdyPh3oKd0LxnsYRcUOkqnDf)

## 1. 安装与登录

```
pip install wandbCopy to clipboardErrorCopied
```

首次使用需登录（需有 [wandb.ai](https://wandb.ai/) 账号）：

```
wandb loginCopy to clipboardErrorCopied
```

> 💡 无外网环境：跳过登录，直接使用 mode="offline"（见第3节）。

## 2. 基础用法：wandb.init()

在训练脚本开头初始化一个 run：

```
import wandb

wandb.init(
    project=&quot;cs336-a5-sft-v2&quot;,          # 项目名（必选）
    entity=&quot;your-team-or-username&quot;,     # 团队/用户名（可选）
    name=&quot;wanda_sft&quot;,        # 可读 run 名称
    config={&quot;model&quot;: &quot;Qwen2.5-Math-1.5B&quot;,&quot;dataset_tag&quot;: &quot;raw&quot;, # raw, sf, grpo&quot;batch_size&quot;: 64,&quot;max_examples&quot;: &quot;1000&quot;,&quot;seed&quot;: 2026,&quot;learning_rate&quot;: 2e-5,})Copy to clipboardErrorCopied
```

config参数可以自己定义，建议将所有超参数、数据路径、模型版本等放入 config，便于后续筛选与比较。

## 3. 离线模式（Offline Mode）

当服务器无法访问外网时，使用离线模式保存日志：

```
wandb.init(mode=&quot;offline&quot;, ...)Copy to clipboardErrorCopied
```

所有日志将保存在本地 wandb/ 目录下，格式为 offline-run-<timestamp>-<id>。

### 后续同步到云端

将包含 wandb/ 目录的文件夹拷贝到有网络的机器，执行：

```
wandb sync wandb/Copy to clipboardErrorCopied
```

> ⚠️ 注意：确保该机器已 wandb login，且 run ID 未被删除。

你也可以只同步特定 run：

```
wandb sync wandb/offline-run-20260116_113519-vc1rtoknCopy to clipboardErrorCopied
```

---

## 4. 记录指标：wandb.log()

在训练/评估循环中记录标量、图像、文本等：

```
for step, batch in enumerate(dataloader):
    loss = model(batch)
    wandb.log({&quot;train/loss&quot;: loss.item(),&quot;train/lr&quot;: scheduler.get_last_lr()[0],&quot;step&quot;: step
    })Copy to clipboardErrorCopied
```

支持：

+   标量（scalar）

+   图像（wandb.Image）

+   文本（wandb.Table）

+   直方图（wandb.Histogram）

+   音频、3D 对象等（较少用于 LLM）

> 📌 技巧：使用 / 分隔命名空间（如 eval/human_eval_pass@1），便于 UI 中分组展示。

## 5. 保存模型与工件（Artifacts）

W&B 支持将模型 checkpoint 作为 Artifact 上传，实现版本控制：

```
artifact = wandb.Artifact(name=&quot;llama3-70b-wanda-c4&quot;, type=&quot;model&quot;)
artifact.add_file(&quot;checkpoints/model.safetensors&quot;)
wandb.log_artifact(artifact)Copy to clipboardErrorCopied
```

后续可在其他实验中引用该模型：

```
artifact = run.use_artifact(&quot;llama3-70b-wanda-c4:latest&quot;)
artifact_dir = artifact.download()Copy to clipboardErrorCopied
```

> 🔒 注意：大模型文件较大，不建议上传模型参数文件。

## 6. 常见问题

#### Q1: 初始化超时？

```
wandb.errors.CommError: Run initialization has timed out...Copy to clipboardErrorCopied
```

解决：增加超时时间或切离线模式：

```
wandb.init(settings=wandb.Settings(init_timeout=120), mode=&quot;offline&quot;)Copy to clipboardErrorCopied
```

#### Q2: 能否禁用 W&B（如调试时）？

```
wandb.init(mode=&quot;disabled&quot;)  # 完全静默，不产生任何副作用
```