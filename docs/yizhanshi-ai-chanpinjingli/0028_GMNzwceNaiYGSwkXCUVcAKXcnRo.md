
# 将Lora adapter载入vLLM

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BzCAdmAUSoar6GxfjrhcE2e7nVd](https://v11enp9ok1h.feishu.cn/docx/BzCAdmAUSoar6GxfjrhcE2e7nVd)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

此文档旨在展示如何在 `glm4-9b` 模型上使用带有 lora adapter 的 `vLLM`。

从而允许用户通过命令行界面与模型交互。

lora微调方法请参考：[微调模型](https://zhipu-ai.feishu.cn/wiki/QanjwjOuaiWMZ6kdVZfcNZwCnBh)

准备好lora adapter到`LORA_PATH`

## 加载原始模型

[OpenAI API Demo](https://zhipu-ai.feishu.cn/wiki/VpncwoWn4i2ZsDkWSqYcbtwSnWd)加载`lora adapter`兼容OpenAI API的方法请参考：

以下代码来自：`basic_demo/vllm_cli_demo.py`

```
engine_args = AsyncEngineArgs(
    model=model_dir,
    tokenizer=model_dir,
    enable_lora=enable_lora,
    tensor_parallel_size=1,
    dtype="bfloat16",
    gpu_memory_utilization=0.9,
    enforce_eager=True,
    worker_use_ray=True,
    disable_log_requests=True
    # 如果遇见 OOM 现象，建议开启下述参数
    # enable_chunked_prefill=True,
    # max_num_batched_tokens=8192
)
```

新增的参数及解释：

- `enable_lora`：必需，设置为`True`则启用 LoRA 适配器的处理。
- `max_loras`：可选，单批次中 LoRA 的最大数量，根据实际使用的lora权重个数修改
- `max_lora_rank`：可选，最大`lora_rank`参数，默认为`16`
- `max_model_len`：可选，模型上下文长度。如果未指定，将自动从模型配置中得出。如果调低了`gpu_memory_utilizatio`的值，可能需要根据显存大小，适当调低该参数。

## 载入微调的模型

```
from vllm.lora.request import LoRARequest

engine.generate(inputs=inputs, sampling_params=sampling_params, request_id=f"{time.time()}", lora_request=LoRARequest("glm-4-lora", 1, lora_path=lora_path)):
```

在`engine.generate`中使用`lora_request`参数进行调用

`LoRARequest`的第一个参数是人类可识别的名称，第二个参数是适配器的全局唯一 ID，第三个参数是 LoRA 适配器的路径。

## 使用vLLM进行调用

```
python vllm_cli_demo.py
```

更多信息，请查看：[vLLM官方文档](https://docs.vllm.ai/en/stable/models/lora.html)
