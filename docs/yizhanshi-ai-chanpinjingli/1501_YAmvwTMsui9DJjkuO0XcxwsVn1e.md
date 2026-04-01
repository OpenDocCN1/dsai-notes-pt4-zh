# train_qwen2

> 来源：[https://v11enp9ok1h.feishu.cn/docx/P54NdCHamoFM5OxkfWMcZW5Nn3b](https://v11enp9ok1h.feishu.cn/docx/P54NdCHamoFM5OxkfWMcZW5Nn3b)

## 对qwen2的微调

## 使用的数据

### download from github

```
git lfs clone https://github.com/Toyhom/Chinese-medical-dialogue-data.git
```

### convert data to instruction data

```
import json
import pandas as pd
from datasets import load_dataset
import os
from pathlib import Path
import shutil
from glob import glob
```

```

all_file_list = glob(&quot;Chinese-medical-dialogue-data/*/*/*.csv&quot;)

alldata = (
    pd.concat([pd.read_csv(i, encoding=&quot;GB18030&quot;) for i in all_file_list])
    .pipe(lambda x: x[[&quot;ask&quot;, &quot;answer&quot;]])
    .pipe(lambda x: x.loc[x[&quot;ask&quot;].apply(lambda j: str(j) != &quot;无&quot;)])
    .pipe(lambda x: x.loc[x[&quot;ask&quot;].apply(lambda j: len(str(j).replace(&quot; &quot;, &quot;&quot;)) &gt; 10)])
    .pipe(
        lambda x: x.loc[x[&quot;answer&quot;].apply(lambda j: len(str(j).replace(&quot; &quot;, &quot;&quot;)) &gt; 5)]
    )
    .pipe(lambda x: x.loc[x[&quot;ask&quot;].apply(lambda j: len(str(j)) &lt;= 100)])
    .pipe(lambda x: x.loc[x[&quot;answer&quot;].apply(lambda j: len(str(j)) &lt;= 500)])
    .sample(frac=1.0)
    .reset_index(drop=True)
)
```

```
from tqdm import tqdm

chunk_size = 100
chunk_s_list = list(range(0, alldata.shape[0], chunk_size))

target_dir = &quot;custom_data001&quot;
shutil.rmtree(target_dir, ignore_errors=True)
os.makedirs(target_dir)

for index, chunk_s in tqdm(enumerate(chunk_s_list), total=len(chunk_s_list)):
    temp_data = alldata.loc[chunk_s: (chunk_s + chunk_size)]
    with open(f&quot;{target_dir}/data_{index}.json&quot;, encoding=&quot;utf-8&quot;, mode=&quot;w&quot;) as fout:
        for i, iter in temp_data.iterrows():
            fout.write(
                json.dumps(
                    {&quot;instruction&quot;: str(iter[&quot;ask&quot;]), &quot;output&quot;: str(iter[&quot;answer&quot;])},
                    ensure_ascii=False,
                )
                + &quot;\n&quot;
            )
```

## 使用的模型

[使用的模式为qwen1](https://huggingface.co/collections/Qwen/qwen15-65c0a2f577b1ecb76d786524).5-xxB-chat，具体的下载链接为Qwen/qwen15

## 修改的目的

主要是在transformers[的Tr](https://github.com/huggingface/transformers/blob/efdd436663436e78d8ad3213d11325d86578db95/src/transformers/trainer.py#L2386-L2401)ainer的这个部分代码，出现问题https://github.com/huggingface/transformers/blob/efdd436663436e78d8ad3213d11325d86578db95/src/transformers/trainer.py#L2386-L2401

报错如下：

```
TypeError: Object of type Tensor is not JSON serializable trainer
```

#### 原始代码

```

    def _maybe_log_save_evaluate(self, tr_loss, grad_norm, model, trial, epoch, ignore_keys_for_eval):
        if self.control.should_log and self.state.global_step &gt; self._globalstep_last_logged:
            if is_torch_tpu_available():
                xm.mark_step()

            logs: Dict[str, float] = {}

            # all_gather + mean() to get average loss over all processes
            tr_loss_scalar = self._nested_gather(tr_loss).mean().item()

            # reset tr_loss to zero
            tr_loss -= tr_loss

            logs[&quot;loss&quot;] = round(tr_loss_scalar / (self.state.global_step - self._globalstep_last_logged), 4)
            if grad_norm is not None:
                logs[&quot;grad_norm&quot;] = grad_norm
            logs[&quot;learning_rate&quot;] = self._get_learning_rate()

            self._total_loss_scalar += tr_loss_scalar
            self._globalstep_last_logged = self.state.global_step
            self.store_flos()

            self.log(logs)
```

#### 修改之后的代码

代码链接： [htt](https://github.com/yuanzhoulvpi2017/zero_nlp/blob/4fb3e8fb12b24c9d469ca88bee83e764c90bda8b/train_qwen/train_qwen2_sft.py#L258-L263)ps://github.com/yuanzhoulvpi2017/zero_nlp/blob/4fb3e8fb12b24c9d469ca88bee83e764c90bda8b/train_qwen/train_qwen2_sft.py#L258-L263