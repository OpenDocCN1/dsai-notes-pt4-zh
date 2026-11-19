
# 基于LLM的Agent在量化投资中的应用

> 来源：<https://v11enp9ok1h.feishu.cn/docx/FxCbdyNjzoBRoYxWdBYcM8lqnRg>
> 该Agent应用使用`Langchain`框架、`Llama3-70B-Instruct`模型、自定义的3个LLM循环计算的执行逻辑、接入一个辅助代码生成的工具构建。
> 通过示例展示如何借助基于LLM的Agent智能体，来自动化地完成单因子开发回测全流程，其中包括：挖掘与开发、单因子回测、单因子优化。

![](img/8a5deaf2350a16b590c9b7845f822863.png)

1. 第一个`Factor-LLM`负责构建因子表达式，它将对数据字段和算子进行组合和复合，输出因子名称、因子计算方法和因子含义。

![](img/0307d97cc2f6425a00481290c7067728.png)

1. 第二个`Code-LLM`接入了一个辅助代码生成的工具，以保证代码生成的稳定性。将上一步生成的因子表达式转换为可执行的代码，并计算结果。在此过程中如果发生错误，`Code-LLM`能够迭代地对代码进行修改。

[`factor_mining2_output.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TmjUbWZmJoTELWxd1Z9cBeoSnQe)

1. 第三个`Eval-LLM`负责对上一步的因子计算结果进行回测检验，并生成优化建议，将建议返回给`Factor-LLM`继续优化（来源华泰金工报告《GPT因子工厂：多智能体与因子挖掘》）。如此循环直至达到指定优化次数。

![](img/43b7db6b4b9978cb98bc3c2b5954d61c.png)

[`factor_mining3_output.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GkZSbZyFooPeYWxalBycUuyPnPd)

在金融行业中，时效性至关重要。在上述的三个示例中，LLM是这些应用的核心组件。相关的数据分析，代码的生成与调用，问题的拆解与工具选择，实际上都是LLM的推理过程。因此，提高LLM的推理速度，是提高上述示例的关键路径。

`TensorRT-LLM`是专门用于加速和优化LLM推理性能的加速库。它提供了最先进的优化方案，包括自定义`attention`内核，`inflight batching`机制，`paged KV caching`，量化（`FP8`、`INT4 AWQ`、`INT8 SmoothQuant`）等优化。通过这些优化，使LLM可以在NVIDIA GPU上高效地执行推理。

本文中的三个示例使用的是`Qwen`和`Llama`大模型，下面将展示在`TensorRT-LLM`中对`Qwen LLM`的优化步骤。

## TensorRT-LLM

1. 克隆`TensorRT-LLM`代码仓库

```
# TensorRT-LLM uses git-lfs, which needs to be installed in advance.

apt-get update && apt-get -y install python3.10 python3-pip openmpi-bin libopenmpi-dev git git-lfs

git lfs install

git clone https://github.com/NVIDIA/TensorRT-LLM.git

cd TensorRT-LLM

git submodule update --init --recursive

git lfs pull
```

1. 安装`TensorRT-LLM`

具体可参考[`TensorRT-LLM`安装指导](https://nvidia.github.io/TensorRT-LLM/installation/linux.html)中提供的两种从源码编译的安装方式；或参考此链接，使用`pip install`来安装

### 构建`Qwen`的推理引擎

1. 进入`TensorRT-LLM/examples/qwen/`目录，并安装相关依赖。

```
pip install -r requirements.txt

git lfs install
```

1. 从`HuggingFace`下载`Qwen-72B-Instruct`的模型权重

```
git clone https://huggingface.co/Qwen/Qwen2-72B-Instruct ./tmp/Qwen/72B
```

1. 构建`Qwen`的推理引擎

```
# Build Qwen-72B-Instruct using 8-way tensor parallelism.

python convert_checkpoint.py --model_dir ./tmp/Qwen/72B/ \
                            --output_dir ./tllm_checkpoint_8gpu_tp8 \
                            --dtype float16 \
                            --tp_size 8

trtllm-build --checkpoint_dir ./tllm_checkpoint_8gpu_tp8 \
            --output_dir ./tmp/qwen/72B/trt_engines/fp16/8-gpu/ \
            --gemm_plugin float16
```

1. 对构建好的推理引擎进行验证

```
# With fp16 inference

python3 ../run.py --input_text "你好，请问你叫什么？" \
                  --max_output_len=50 \
                  --tokenizer_dir ./tmp/Qwen/72B/ \
                  --engine_dir=./tmp/Qwen/72B/trt_engines/fp16/8-gpu/
```

### 对推理引擎进行性能测试

#### 安装`benchmark`工具

```
python3 ./scripts/build_wheel.py --benchmarks
```

#### 构建测试数据集

可参考此[文档](https://github.com/NVIDIA/TensorRT-LLM/tree/main/benchmarks/cpp)，构建基于[实际数据集](https://github.com/NVIDIA/TensorRT-LLM/tree/main/benchmarks/cpp#1--dataset)，[正态分布](https://github.com/NVIDIA/TensorRT-LLM/tree/main/benchmarks/cpp#2--normal-token-length-distribution)，或[均匀分布](https://github.com/NVIDIA/TensorRT-LLM/tree/main/benchmarks/cpp#2--uniform-token-length-distribution)的测试数据集。例如构建1000条固定输入长度为2048、固定输出长度为32的测试数据集：

```
python3 ./benchmarks/cpp/prepare_dataset.py \
    --output token-norm-dist.json \
    --tokenizer ./tmp/Qwen/72B/ \
    token-norm-dist \
    --num-requests 1000 \
    --input-mean 2048 --input-stdev 0 \
    --output-mean 32 --output-stdev 0
```

#### 运行性能测试

```
mpirun -n 8 ./benchmarks/gptManagerBenchmark \
    --engine_dir ./tmp/Qwen/72B/trt_engines/fp16/8-gpu/ \
    --request_rate 10 \
    --dataset ./token-norm-dist.json \
    --max_num_samples 500
```

### 性能测试结果

经过上述三个步骤，我们可以得到以下性能测试结果：

```
[BENCHMARK] num_samples 500
[BENCHMARK] num_error_samples 0

[BENCHMARK] num_samples 500
[BENCHMARK] total_latency(ms) 350899.59
[BENCHMARK] seq_throughput(seq/sec) 1.42
[BENCHMARK] token_throughput(token/sec) 45.60
[BENCHMARK] avg_acceptance_rate(tokens/decoding steps) 1.00

[BENCHMARK] avg_sequence_latency(ms) 209908.12
[BENCHMARK] max_sequence_latency(ms) 308942.41
[BENCHMARK] min_sequence_latency(ms) 66060.34
[BENCHMARK] p99_sequence_latency(ms) 308529.88
[BENCHMARK] p90_sequence_latency(ms) 305296.28
[BENCHMARK] p50_sequence_latency(ms) 215189.81
```

## 部署`TensorRT-LLM`的`OpenAI API`格式`Serving`

在`Agent`应用和常见的`LLM`应用框架中，大家通常偏向于使用`OpenAI API`格式的接口，在`TensorRT-LLM`中我们可以参考此步骤，通过一行命令便可以部署一个`OpenAI API`格式的`serving`服务。以下展示了我们部署一个 tensor parallelism 大小为 8 的 `Qwen2-72B-Instruct` 模型：

```
python /app/tensorrt_llm/examples/apps/openai_server.py /workspace/Qwen2-72B-Instruct --tp_size 8
```

![](img/b64f21b7d309b1eb86ee4063558004c5.png)
