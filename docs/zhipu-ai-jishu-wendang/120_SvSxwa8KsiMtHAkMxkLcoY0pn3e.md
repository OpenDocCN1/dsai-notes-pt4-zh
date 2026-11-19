
# 模型下载和安装

> 来源：[https://ai.feishu.cn/docx/GEVJdB3htoI4aVxL8giccD4in0d](https://ai.feishu.cn/docx/GEVJdB3htoI4aVxL8giccD4in0d)

## 下载模型和代码

### 下载示例代码

从 github 下载源码

```
git clone https://github.com/THUDM/CogAgent.git
```

### 下载模型

[下载](https://github.com/git-lfs/git-lfs?utm_source=gitlfs_site&utm_medium=installation_link&utm_campaign=gitlfs#installing)模型文件前请先确保`git lfs`命令已安装，安装教程请参考这里。

```
git lfs install
```

模型文件已上传至 Huggingface, Modelsope, WiseModel 三个平台，用户可以快速安装模型。

+ 从 Huggingface 下载模型

```
git clone https://huggingface.co/THUDM/cogagent1.5-9b
```

+ 从 Modelscope 下载模型

```
git clone https://modelscope.cn/models/ZhipuAI/cogagent1.5-9b
```

## 最低配置要求

单 GPU 推理的 VRAM 消耗：

`BF16`: 29GB 显存，这通常需要shyi

微调显存消耗（每个 GPU）：

`LORA`: 36GB 显存(`bs=1`, `1 * GPU`, `2048 token` 上下文)

`SFT`: (`bs=1`, `8 * GPU`, `2048 token` 上下文)

支持平台：

+ Windows 10，11

+ Mac 14，15

+ Android 13，14，15 以及其他GUI和UI操作方式几乎相同的安卓UI发行版

补充：

+ 不建议使用 `INT4` 精度推理，性能损失较大。

+ 未测试在非 NVIDIA 设备上的性能数据和支持情况。

+ 图片编码占用的 1600 tokens 也已经被计算在内。

# 贡献指南

如果您有技术问题，您需要在 github issue 提出：

`https://github.com/THUDM/GLM-Edge/issues`

如果您想贡献关于 开源项目仓库更新规划 中的代码，或者修改仓库中的bug，您需要提出 PR：

`https://github.com/THUDM/GLM-Edge/pulls`

同时，请严格按照 [5.0 做出贡献](https://zhipu-ai.feishu.cn/wiki/WFx8wHev5ilQ9Tk7ERGcKa8MnVd) 中的要做出贡献。感谢您的支持。
