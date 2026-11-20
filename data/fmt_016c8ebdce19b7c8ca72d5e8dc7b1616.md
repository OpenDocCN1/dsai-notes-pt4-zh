
```
import os
os.environ[&quot;USE_FLASH_ATTENTION&quot;] = &quot;0&quot;

from PIL import Image
from transformers import AutoProcessor, AutoModelForImageTextToText
import torch

model_name = &quot;/root/data/workdir/vlhf1031/20251031101422/hf-200&quot;

# 加载处理器和模型
processor = AutoProcessor.from_pretrained(model_name, trust_remote_code=True)
model = AutoModelForImageTextToText.from_pretrained(
    model_name, 
    device_map=&quot;auto&quot;, 
    torch_dtype=torch.bfloat16,  # 使用 torch_dtype 而不是 dtype
    trust_remote_code=True,
    attn_implementation=&quot;eager&quot;  # 禁用 flash attention
)

# 加载图像
image = Image.open(&quot;/root/data/dataset/VLM-formula-recognition-dataset_intern_camp/train/mini_train/sample19996.png&quot;)
prompt = &quot;describle this img&quot;

messages = [
    {
        &quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: [
            {&quot;type&quot;: &quot;image&quot;, &quot;image&quot;: image},
            {&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: prompt},
        ],
    }
]

# 处理输入
inputs = processor.apply_chat_template(
    messages,
    add_generation_prompt=True,
    tokenize=True,
    return_dict=True,
    return_tensors=&quot;pt&quot;
).to(model.device)

# 确保输入数据类型与模型一致
inputs = {k: v.to(dtype=torch.bfloat16) if v.dtype == torch.float32 else v for k, v in inputs.items()}

# 生成
with torch.inference_mode():
    generate_ids = model.generate(**inputs, max_new_tokens=200)

decoded_output = processor.decode(generate_ids[0, inputs[&quot;input_ids&quot;].shape[1]:], skip_special_tokens=True)
print(&quot;current model: &quot;, model_name)
print(decoded_output)
```

运行模型进行推理，若模型输出正常且结果满意，即可提交。

![](img/def7857485b9684c1a4055ccf1898431.png)

或者测这张图片：

![](img/fbd33835109502def1be02662f48de8c.png)

# 模型上传到[魔乐](https://modelers.cn/models/new)

## 魔乐上传配置

1. 创建组织，例如命名为：`camp_test123`

![](img/4fa08b3dc1434febcc10416cabb0036e.png)

1. 在组织内创建模型

![](img/848a17a45cb7a56786cac0c33b48c02f.png)

![](img/d6021c301bde24533b3eaa82a9e21b42.png)

1. 为了保证比赛公正，本次使用的魔乐社区托管权重，请使用组织功能，并邀请`JimmyMa-99` 用户加入团队，同时请将上传的是否公开为私有模型。

![](img/41699122b3ccae941ddb75c1e70df273.png)

1. 一定要邀请米佬，等同意 。如在 24 小时内没有同意邀请，请在群里 @`Nev8r` 。

![](img/7ae1051234b29ca7912efd0ee7bc620e.png)

## 上传模型及日志

1. 下载所需环境包

```
python3.10 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install openmind_hub[all]
pip install &quot;openmind[pt]&quot; --extra-index-url https://download.pytorch.org/whl/cpu
```

1. 运行上传脚本

```
from openmind_hub import upload_folder

upload_folder(
    token=&quot;a5cxxe&quot;,#获取方式：https://modelers.cn/my/tokens
    folder_path=&quot;xxx&quot;,#模型位置，
    repo_id=&quot;xxx&quot;,#模型空间名字
)
```

```
python upload_model.py
```

模型位置需要放训练的 `log` 文件，包含日志信息（包含硬件、`loss` 等），如下图所示：

![](img/6898e46e2d155931d093eb77fcfc0236.png)

运行脚本上传完成，魔乐空间会如如下所示：

![](img/5dbc1268de499812cddfb2b3716e239d.png)

提交表单的 `prompt` 参考如下,这里也有优化空间，可自行探索最优 `prompt`：

```
请根据图片中的公式生成对应的 latex 公式文本
```

## 参赛流程

### 报名

点击以下链接进行报名,获取 `uid`

https://colearn.intern-ai.org.cn/set?s=gamef

### 提交参赛权重

微调后的权重请从以下链接入口提交，不要填写链接，参考截图。

提交表单：https://aicarrier.feishu.cn/share/base/form/shrcnRzFT7AatAZWolO7A3zsiKg

![](img/9de4cd5fcd27880aba6d93376695d04a.png)

![](img/c2b9d9326ff0c81a4f94ad45c1af30d7.png)

注意：需要选择好 `base model`，本次比赛请选择 `InternVL3_5-1B-HF` ，如提交其他会出现测评异常情况！

本次测评会提取模型返回中的最后一个 `latex` 块

```latex
E = mc^2
```

提交完成，等待后台半小时左右评测，最终获得成绩🎉🎉

![](img/cbc53544cc5f9d1e1cf0b7f39ba4c846.png)

微调后有较大提升：

![](img/74b118352f78ff7514db2cda560b41f7.png)

### 查看状态/分数

点击以下链接

https://aicarrier.feishu.cn/share/base/query/shrcnvGInFWkTavPofVvaOwkHFc

进行查看，输入你的 `uid`，比如闻星（`uid`：4）

在这里能够查看状态

![](img/7977e2f1da3774ccf2b07c8eb0beecda.png)

### 排行榜

https://aicarrier.feishu.cn/share/base/dashboard/shrcns0G2aJjSO4QXhQcGtv8XIh

<https://aicarrier.feishu.cn/share/base/dashboard/shrcns0G2aJjSO4QXhQcGtv8XIh?iframeFrom=docx&ccm_open=iframe>

## QA

### 日志

该缺陷可能导致日志写入功能异常，但不会影响模型的正常保存。

```
--- Logging error in Loguru Handler #2 ---
Record was: {&#x27;elapsed&#x27;: datetime.timedelta(seconds=2588, microseconds=214180), &#x27;exception&#x27;: None, &#x27;extra&#x27;: {}, &#x27;file&#x27;: (name=&#x27;trainer.py&#x27;, path=&#x27;/root/data/xtuner/xtuner/v1/train/trainer.py&#x27;), &#x27;function&#x27;: &#x27;_log_step&#x27;, &#x27;level&#x27;: (name=&#x27;INFO&#x27;, no=20, icon=&#x27;ℹ️&#x27;), &#x27;line&#x27;: 1026, &#x27;message&#x27;: &#x27;Step 102/290 data_time: 0.0009 lr: 2.488710e-05 time: 25.4424 text_tokens: 31805.0 total_loss: 0.20077972, reduced_llm_loss: 0.20077972, loss: 0.20077971 max_memory: 21.07 GB reserved_memory: 26.62 GB grad_norm: 0.88643903 tgs: 1250.1 e2e_tgs: 1248.5 eta: 1:19:09 &#x27;, &#x27;module&#x27;: &#x27;trainer&#x27;, &#x27;name&#x27;: &#x27;xtuner.v1.train.trainer&#x27;, &#x27;process&#x27;: (id=135707, name=&#x27;MainProcess&#x27;), &#x27;thread&#x27;: (id=139894268028736, name=&#x27;MainThread&#x27;), &#x27;time&#x27;: datetime(2025, 10, 31, 10, 57, 27, 496673, tzinfo=datetime.timezone(datetime.timedelta(seconds=28800), &#x27;CST&#x27;))}
Traceback (most recent call last):
  File &#34;/opt/conda/lib/python3.10/site-packages/loguru/_handler.py&#34;, line 206, in emit
    self._sink.write(str_record)
  File &#34;/opt/conda/lib/python3.10/site-packages/loguru/_file_sink.py&#34;, line 206, in write
    self._file.write(message)
OSError: [Errno 9] Bad file descriptor
--- End of logging error ---
[XTuner][RANK 0][2025-10-31 10:57:27][INFO] Step 102/290 data_time: 0.0009 lr: 2.488710e-05 time: 25.4424 text_tokens: 31805.0 total_loss: 0.20077972, reduced_llm_loss: 0.20077972, loss: 0.20077971 max_memory: 21.07 GB reserved_memory: 26.62 GB grad_norm: 0.88643903 tgs: 1250.1 e2e_tgs: 1248.5 eta: 1:19:09 
--- Logging error in Loguru Handler #2 ---
Record was: {&#x27;elapsed&#x27;: datetime.timedelta(seconds=2600, microseconds=218344), &#x27;exception&#x27;: None, &#x27;extra&#x27;: {}, &#x27;file&#x27;: (name=&#x27;jsonl_writer.py&#x27;, path=&#x27;/root/data/xtuner/xtuner/v1/_writer/jsonl_writer.py&#x27;), &#x27;function&#x27;: &#x27;_run&#x27;, &#x27;level&#x27;: (name=&#x27;WARNING&#x27;, no=30, icon=&#x27;⚠️&#x27;), &#x27;line&#x27;: 140, &#x27;message&#x27;: &#x27;Exception occurred during flushing /root/data/workdir/vlhf1031/20251031101422/logs/exp_tracking/rank0/tracker.jsonl&#x27;, &#x27;module&#x27;: &#x27;jsonl_writer&#x27;, &#x27;name&#x27;: &#x27;xtuner.v1._writer.jsonl_writer&#x27;, &#x27;process&#x27;: (id=135707, name=&#x27;MainProcess&#x27;), &#x27;thread&#x27;: (id=139877717894720, name=&#x27;JsonlWriterAsync&#x27;), &#x27;time&#x27;: datetime(2025, 10, 31, 10, 57: 39, 500837, tzinfo=datetime.timezone(datetime.timedelta(seconds=28800), &#x27;CST&#x27;))}
Traceback (most recent call last):
  File &#34;/root/data/xtuner/xtuner/v1/_writer/jsonl_writer.py&#34;, line 138, in _run
    writer._file_writer.flush()
OSError: [Errno 9] Bad file descriptor

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File &#34;/opt/conda/lib/python3.10/site-packages/loguru/_handler.py&#34;, line 206, in emit
    self._sink.write(str_record)
  File &#34;/opt/conda/lib/python3.10/site-packages/loguru/_file_sink.py&#34;, line 206, in write
    self._file.write(message)
OSError: [Errno 9] Bad file descriptor
--- End of logging error ---
[XTuner][RANK 0][2025-10-31 10:57:39][WARNING] Exception occurred during flushing /root/data/workdir/vlhf1031/20251031101422/logs/exp_tracking/rank0/tracker.jsonl

```

![](img/2168f42ef9015c33be00ba7127a6e9aa.png)

### 分数不够高

笔者也尝试调低 `lr`,最终排查是数据集的格式问题，转化成 `xtuner`数据集格式代码如下：
