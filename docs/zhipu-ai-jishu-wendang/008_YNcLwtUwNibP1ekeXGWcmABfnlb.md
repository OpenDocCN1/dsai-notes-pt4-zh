# GLM4.5V Ascend NPU说明

> 来源：[https://ai.feishu.cn/docx/XZ9zdrHltobxlgx3FqBc9Scyn0c](https://ai.feishu.cn/docx/XZ9zdrHltobxlgx3FqBc9Scyn0c)

**昇**腾 NPU 是专门用于 AI 训练/推理计算的 AI 专用处理器，其中的 AI Core 能够在很大程度上提高 AI 计算的效率。

[权重下载](https://modelers.cn/models/zhipuai/GLM-4.5V)

已在 Atlas 800T A2(8*64G) 设备测试。

# 基础环境安装

首先参考 [vllm-ascend](https://vllm-ascend.readthedocs.io/en/latest/installation.html) 文档准备 vllm 及 vllm-ascend 前置环境。下面以使用 docker 部署为例：

```
export IMAGE=quay.io/ascend/vllm-ascend:v0.9.2rc1
export NAME=vllm-glm45v

docker run \
    --name ${NAME} --privileged \
    --net=host \
    --device /dev/davinci0 \
    --device /dev/davinci1 \
    --device /dev/davinci2 \
    --device /dev/davinci3 \
    --device /dev/davinci4 \
    --device /dev/davinci5 \
    --device /dev/davinci6 \
    --device /dev/davinci7 \
    --device /dev/davinci_manager \
    --device /dev/devmm_svm \
    --device /dev/hisi_hdc \
    -v /usr/local/dcmi:/usr/local/dcmi \
    -v /usr/local/bin/npu-smi:/usr/local/bin/npu-smi \
    -v /usr/local/Ascend/driver/lib64/:/usr/local/Ascend/driver/lib64/ \
    -v /usr/local/Ascend/driver/version.info:/usr/local/Ascend/driver/version.info \
    -v /etc/ascend_install.info:/etc/ascend_install.info \
    -v /home:/home \
    --shm-size=128g \
    -it ${IMAGE} bash
​
```

容器内需下载针对 GLM 4.5 V 的适配代码：

```
git clone https://github.com/starmountain1997/vllm.git -b 091glm45v
git clone https://github.com/starmountain1997/vllm-ascend.git -b 091glm45v
```

覆盖安装 vllm 和 vllm-ascend：

```
cd vllm
VLLM_TARGET_DEVICE=empty pip install -v -e .

cd vllm-ascend
pip install -v -e .
```

如果提示无法找到特定版本 torch-npu，请添加 pip 源：

```
pip config set global.extra-index-url &quot;https://download.pytorch.org/whl/cpu/ https://mirrors.huaweicloud.com/ascend/repos/pypi&quot;​
```

需要重新安装最新版本的 transformers：

```
pip install git+https://github.com/huggingface/transformers.git
```

# 拉起服务

设置环境变量使用 vllm v1：

```
export VLLM_USE_V1=1
export HCCL_DETERMINISTIC=true
```

使用下列命令拉起服务：

```
python -m vllm.entrypoints.openai.api_server \
    --model {模型路径} \
    --tensor-parallel-size 8 \
    --enable_expert_parallel \
    --tool-call-parser glm45 \
    --reasoning-parser glm45 \
    --enable-auto-tool-choice \
    --allowed-local-media-path / \
    --served-model-name glm-4.5v
```

使用下面的代码进行简单测试：

```
import base64
import requests

image_path = &#x27;cat.jpg&#x27;

api_url = f&quot;http://localhost:8000/v1/chat/completions&quot;import argparse
from PIL import Image
import io

def encode_image(image_path):
    with Image.open(image_path) as img:
        resized_img = img.resize((3000, 4000), Image.Resampling.LANCZOS)
        buffered = io.BytesIO()
        resized_img.save(buffered, format=img.format)
        buffered.seek(0)
        return base64.b64encode(buffered.getvalue()).decode(&#x27;utf-8&#x27;)

def run_inference():
    image_base64 = encode_image(image_path)
    headers = {&#x27;Content-Type&#x27;: &#x27;application/json&#x27;}
    payload = {
        &quot;model&quot;: &quot;glm-4.5v&quot;,
        &quot;messages&quot;: [
            {
                &quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: [
                    {
                        &quot;type&quot;: &quot;text&quot;,
                        &quot;text&quot;: &quot;What is in this image?&quot;
                    },
                    {
                        &quot;type&quot;: &quot;image_url&quot;,
                        &quot;image_url&quot;: {
                            &quot;url&quot;: f&quot;data:image/jpeg;base64,{image_base64}&quot;
                        }
                    }
                ]
            }
        ],
        &quot;max_tokens&quot;: 1024,
        &quot;temperature&quot;: 0,
        &quot;stream&quot;: False
    }

    response = requests.post(api_url, headers=headers, json=payload)
    try:
        output = response.json()
        print(f&quot;输出内容:{output[&#x27;choices&#x27;][0][&#x27;message&#x27;][&#x27;content&#x27;]}&quot;)
    except:
        print(&quot;解析相应失败:&quot;, response.text)

if __name__ == &quot;__main__&quot;:
    run_inference()
​
```

# 性能测试

使用 [aisbench](https://gitee.com/aisbench/benchmark) 进行性能测试。

## 安装 aisbench

```
git clone https://gitee.com/aisbench/benchmark.git
cd benchmark/
pip3 install -e ./ --use-pep517
​
```

## 配置数据集

自行下载数据集，下面以 textvqa 数据集为例。

编辑 ais_bench/benchmark/configs/datasets/textvqa/textvqa_gen.py，修改 image_url 至 image_url_base64，并配置你的数据集的位置：

```
from ais_bench.benchmark.openicl.icl_prompt_template import PromptTemplate
from ais_bench.benchmark.openicl.icl_retriever import ZeroRetriever
from ais_bench.benchmark.openicl.icl_inferencer import GenInferencer
from ais_bench.benchmark.datasets import TEXTVQADataset, TEXTEvaluator, math_postprocess_v2


textvqa_reader_cfg = dict(
    input_columns=[&#x27;question&#x27;],
    output_column=&#x27;answer&#x27;
)


textvqa_infer_cfg = dict(
    prompt_template=dict(
        type=PromptTemplate,
                        #                                    修改这里 ↓
        template={&#x27;type&#x27;: &quot;image_text&quot;, &#x27;data&#x27;: [&#x27;image_url_base64&#x27;, &#x27;text&#x27;], &#x27;prompt&#x27;: &quot; Answer the question using a single word or phrase.&quot;}
    ),
    retriever=dict(type=ZeroRetriever),
    inferencer=dict(type=GenInferencer)
)

textvqa_eval_cfg = dict(
    evaluator=dict(type=TEXTEvaluator)
)

textvqa_datasets = [
    dict(
        abbr=&#x27;textvqa&#x27;,
        type=TEXTVQADataset,
        # 配置你的数据集位置 ↓
        path=&#x27;/home/guozr/weights/fixed/textvqa_val.jsonl&#x27;,
        reader_cfg=textvqa_reader_cfg,
        infer_cfg=textvqa_infer_cfg,
        eval_cfg=textvqa_eval_cfg
    )
]
​
```

## 配置测试条件

修改 ais_bench/benchmark/configs/models/vllm_api/vllm_api_stream_chat.py：

```
from ais_bench.benchmark.models import VLLMCustomAPIChatStream
from ais_bench.benchmark.utils.model_postprocessors import extract_non_reasoning_content

models = [
    dict(
        attr=&quot;service&quot;,
        type=VLLMCustomAPIChatStream,
        abbr=&#x27;vllm-api-stream-chat&#x27;,
        # 你权重的位置和vllm发起的模型名称 ↓
        path=&quot;/home/guozr/weights/GLM-4.5V&quot;,
        model=&quot;glm-4.5v&quot;,
        request_rate = 0,
        retry = 2,
        host_ip = &quot;localhost&quot;,
        host_port = 8001,
        max_out_len = 5500,
        # 这里可以调节并发 ↓
        batch_size=8,
        trust_remote_code=False,
        generation_kwargs = dict(
            temperature = 0.5,
            top_k = 10,
            top_p = 0.95,
            seed = None,
            repetition_penalty = 1.03,
        ),
        pred_postprocessor=dict(type=extract_non_reasoning_content)
    )
]
​
```

## 发起测试

精度测试：

```
ais_bench --models vllm_api_stream_chat --datasets textvqa_gen --debug
```

性能测试：

```
ais_bench --models vllm_api_stream_chat --datasets textvqa_gen --debug -m perf
```