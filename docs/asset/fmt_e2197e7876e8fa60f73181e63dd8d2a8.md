
# Deploy 知识库共建

> 来源：`https://v11enp9ok1h.feishu.cn/docx/KwcKdCxyXo8TBcxYVyXcaacrnsb`

`https://v11enp9ok1h.feishu.cn/sync/PaIIdslbxsIJUIbkTfPcsMCFnph`

## 任务规划

| 章节 | 任务ID | 小节 | 描述 |
| --- | --- | --- | --- |
| 基础篇 |  |  |  |
| 模型部署基础 | Deploy001 | 模型推理 | 什么是模型推理，和训练的区别，如何使用最简单的`torch`推理一个模型 |
|  | Deploy002 | 模型部署 | 选择其他框架对模型进行推理，简单介绍下常见的推理框架 ONNX OpenVINO TensorRT |
|  | Deploy003 | 模型量化 | 解释最基础的模型量化知识，模型量化公式等理论知识解析 |
| 大模型架构详解 | Deploy004 | 矩阵乘法 | 知识讲解+最简单Py代码 |
|  | Deploy005 | 位置编码 | `RoPE`为主 |
|  | Deploy006 | Norm | `LayerNorm` + `RmsNorm` |
|  | Deploy007 | Attention | `Attention` |
|  | Deploy008 | 激活函数 | `Gelu` `Relu` `SwiGelu` |
|  | Deploy009 | 解码相关 | `topk`，`topp`，beam search |
| 大模型部署框架解析 | Deploy010 | vLLM | 详细讲讲论文可以 |
|  | Deploy011 | TRT-LLM | nv有什么优化？ |
|  | Deploy012 | LMDeploy | 推理速度上的优势 |
| 简单的CUDA | Deploy013 | CUDA 编程模型简介 | thread wrap block |
|  | Deploy014 | CUDA 内存模型简介 | 主存，共享内存，寄存器 |
|  | Deploy015 | CUDA bank conflict | 是什么 哪里会有 如何减弱 bank conflict |
|  | Deploy016 | CUDA 向量化访存 | 如何加速数据传递 |
|  | Deploy017 | CUDA 共享内存 | 为什么需要共享内存。共享内存应用的场景 |
|  | Deploy018 | CUDA 计算资源 | Tensorcore调用 |
| 进阶篇 |  |  |  |
| 用cuda和Triton写大模型 | Deploy019 | Triton简介 |  |
|  | Deploy020 | cuda/Triton 写一个简单的量化层，激活函数 |  |
|  | Deploy021 | cuda/Triton 写一个`LayerNorm`/`RmsNorm` |  |
|  | Deploy022 | cuda/Triton 写一个`RoPE` |  |
|  | Deploy023 | 用`cublas`写一个矩阵乘法 |  |
| 大模型量化 | Deploy024 | 量化简析 | 如何写一个量化层，如何插入量化层？如何插入解量化层.主流的量化方法 w8a8/w4a16。如何选择量化方法 |
|  | Deploy025 | w8a8 | 如何融合量化算子 认识`int8`矩阵乘法 smoothquant 如何调用tensorcore加速你的`int8`矩阵乘法 |
|  | Deploy026 | w4a16 | 整体流程 awq 快速反量化 如何为了应用tensorcore对数据进行特殊排布 |
|  | Deploy027 | `kvcache` | 为什么要对`kvcache`做量化 `int8` kvcache量化 算子实现 `int4` kvcache量化 |
| 底层加速 | Deploy028 | Flash attn | 1 2 3 decoding |
|  | Deploy029 | GQA MHA MLA |  |
|  | Deploy030 | 矩阵乘法合并 |  |
|  | Deploy031 | Paged attention |  |
|  | Deploy032 | Group gemm |  |
| 系统层面加速 | Deploy033 | Contiguous batching |  |
|  | Deploy034 | 投机解码 |  |
|  | Deploy035 | Prefix cache |  |

## 进阶之大模型底层加速方法

+ qkv矩阵乘法的合并

+ （`qkv`矩阵乘法和`rope`的合并 这个可能比较难 这么写的人不多

+ Flash attn 1

+ Flash attn 2 3

+ Flash decoding and ++

+ GQA MHA MLA

+ Paged attention

+ （针对MoE的 `Group Gemm` 估计这个也会找不到人写

## 大模型系统层面加速方法

+ Contiguous batching

+ 投机解码

+ Medusa

+ Prefix cache
