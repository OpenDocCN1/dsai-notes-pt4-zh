# DeepSeek-OCR Usage Guide-vLLM

> 来源：[https://gotoaiworld.feishu.cn/docx/WCfbdbl1Rog2OnxjdRbcrm3ynRg](https://gotoaiworld.feishu.cn/docx/WCfbdbl1Rog2OnxjdRbcrm3ynRg)

## Introduction

[DeepSeek-OCR](https://huggingface.co/deepseek-ai/DeepSeek-OCR) is a frontier OCR model exploring optical context compression for LLMs.

## Installing vLLM

```
uv venv
source .venv/bin/activate
# Until v0.11.1 release, you need to install vLLM from nightly builduv pip install -U vllm --pre --extra-index-url https://wheels.vllm.ai/nightly
```

## Running DeepSeek-OCR

In this guide, we demonstrate how to set up DeepSeek-OCR for offline OCR batch processing tasks.

```
from vllm import LLM, SamplingParamsfrom vllm.model_executor.models.deepseek_ocr import NGramPerReqLogitsProcessorfrom PIL import Image# Create model instancellm = LLM(    model=&quot;deepseek-ai/DeepSeek-OCR&quot;,    enable_prefix_caching=False,    mm_processor_cache_gb=0,    logits_processors=[NGramPerReqLogitsProcessor])# Prepare batched input with your image fileimage_1 = Image.open(&quot;path/to/your/image_1.png&quot;).convert(&quot;RGB&quot;)image_2 = Image.open(&quot;path/to/your/image_2.png&quot;).convert(&quot;RGB&quot;)prompt = &quot;&lt;image&gt;\nFree OCR.&quot;model_input = [    {        &quot;prompt&quot;: prompt,        &quot;multi_modal_data&quot;: {&quot;image&quot;: image_1}    },    {        &quot;prompt&quot;: prompt,        &quot;multi_modal_data&quot;: {&quot;image&quot;: image_2}    }]sampling_param = SamplingParams(            temperature=0.0,            max_tokens=8192,            # ngram logit processor args            extra_args=dict(                ngram_size=30,                window_size=90,                whitelist_token_ids={128821, 128822},  # whitelist: &lt;td&gt;, &lt;/td&gt;            ),            skip_special_tokens=False,        )# Generate outputmodel_outputs = llm.generate(model_input, sampling_param)# Print outputfor output in model_outputs:    print(output.outputs[0].text)
```

## Configuration Tips

+   **It's important to use the cus**tom logits processor along with the model for the optimal OCR and markdown generation performance.

+   Unlike multi-turn chat use cases, we do not expect OCR tasks to benefit significantly from prefix caching or image reuse, therefore it's recommended to turn off these features to avoid unnecessary hashing and caching.

+   DeepSeek-OCR works better with plain[ prompts than instruct](https://github.com/deepseek-ai/DeepSeek-OCR/blob/2ac6d64a00656693b79c4f759a5e62c1b78bbeb1/DeepSeek-OCR-master/DeepSeek-OCR-vllm/config.py#L27-L37)ion formats. Find more example prompts for various OCR tasks in the official DeepSeek-OCR repository.

+   Depending on your hardware capability, adjust max_num_batched_tokens for better throughput performance.

+   Check out vLLM documentation for additional information on batch inference with multimodal inputs.