
# 1. 安装依赖包

```bash
Looking in indexes: http://mirrors.aliyun.com/pypi/simple
Collecting datasets
  Downloading http://mirrors.aliyun.com/pypi/packages/e3/f5/668b3444a2f487b0052b908af631fe39eeb2bdb2359d9bbc2c3b80b71119/datasets-3.5.1-py3-none-any.whl (491 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 491.4/491.4 kB 4.3 MB/s eta 0:00:00
Requirement already satisfied: huggingface_hub in /root/miniconda3/lib/python3.12/site-packages (0.30.2)
Requirement already satisfied: filelock in /root/miniconda3/lib/python3.12/site-packages (from datasets) (3.18.0)
Requirement already satisfied: numpy>=1.17 in /root/miniconda3/lib/python3.12/site-packages (from datasets) (2.2.5)
Requirement already satisfied: pyarrow>=15.0.0 in /root/miniconda3/lib/python3.12/site-packages (from datasets) (20.0.0)
Requirement already satisfied: dill<0.3.9,>=0.3.0 in /root/miniconda3/lib/python3.12/site-packages (from datasets) (0.3.8)
Collecting pandas (from datasets)
  Downloading http://mirrors.aliyun.com/pypi/packages/38/f8/d8fddee9ed0d0c0f4a2132c1dfcf0e3e53265055da8df952a53e7eaf178c/pandas-2.2.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (12.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 12.7/12.7 MB 5.4 MB/s eta 0:00:00
Requirement already satisfied: requests>=2.32.2 in /root/miniconda3/lib/python3.12/site-packages (from datasets) (2.32.3)
Requirement already satisfied: tqdm>=4.66.3 in /root/miniconda3/lib/python3.12/site-packages (from datasets) (4.67.1)
Requirement already satisfied: xxhash in /root/miniconda3/lib/python3.12/site-packages (from datasets) (3.5.0)
Collecting multiprocess<0.70.17 (from datasets)
  Downloading http://mirrors.aliyun.com/pypi/packages/0a/7d/a988f258104dcd2ccf1ed40fdc97e26c4ac351eeaf81d76e266c52d84e2f/multiprocess-0.70.16-py312-none-any.whl (146 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 146.7/146.7 kB 2.9 MB/s eta 0:00:00
Requirement already satisfied: fsspec<=2025.3.0,>=2023.1.0 in /root/miniconda3/lib/python3.12/site-packages (from fsspec[http]<=2025.3.0,>=2023.1.0->datasets) (2025.3.0)
Requirement already satisfied: aiohttp in /root/miniconda3/lib/python3.12/site-packages (from datasets) (3.11.18)
Requirement already satisfied: packaging in /root/miniconda3/lib/python3.12/site-packages (from datasets) (25.0)
Requirement already satisfied: pyyaml>=5.1 in /root/miniconda3/lib/python3.12/site-packages (from datasets) (6.0.2)
Requirement already satisfied: typing-extensions>=3.7.4.3 in /root/miniconda3/lib/python3.12/site-packages (from huggingface_hub) (4.13.2)
Requirement already satisfied: aiohappyeyeballs>=2.3.0 in /root/miniconda3/lib/python3.12/site-packages (from aiohttp->datasets) (2.6.1)
Requirement already satisfied: aiosignal>=1.1.2 in /root/miniconda3/lib/python3.12/site-packages (from aiohttp->datasets) (1.3.2)
Requirement already satisfied: attrs>=17.3.0 in /root/miniconda3/lib/python3.12/site-packages (from aiohttp->datasets) (25.3.0)
Requirement already satisfied: frozenlist>=1.1.1 in /root/miniconda3/lib/python3.12/site-packages (from aiohttp->datasets) (1.6.0)
Requirement already satisfied: multidict<7.0,>=4.5 in /root/miniconda3/lib/python3.12/site-packages (from aiohttp->datasets) (6.4.3)
Requirement already satisfied: propcache>=0.2.0 in /root/miniconda3/lib/python3.12/site-packages (from aiohttp->datasets) (0.3.1)
Requirement already satisfied: yarl<2.0,>=1.17.0 in /root/miniconda3/lib/python3.12/site-packages (from aiohttp->datasets) (1.20.0)
Requirement already satisfied: charset-normalizer<4,>=2 in /root/miniconda3/lib/python3.12/site-packages (from requests>=2.32.2->datasets) (3.4.2)
Requirement already satisfied: idna<4,>=2.5 in /root/miniconda3/lib/python3.12/site-packages (from requests>=2.32.2->datasets) (3.10)
Requirement already satisfied: urllib3<3,>=1.21.1 in /root/miniconda3/lib/python3.12/site-packages (from requests>=2.32.2->datasets) (2.4.0)
Requirement already satisfied: certifi>=2017.4.17 in /root/miniconda3/lib/python3.12/site-packages (from requests>=2.32.2->datasets) (2025.4.26)
Requirement already satisfied: python-dateutil>=2.8.2 in /root/miniconda3/lib/python3.12/site-packages (from pandas->datasets) (2.9.0.post0)
Requirement already satisfied: pytz>=2020.1 in /root/miniconda3/lib/python3.12/site-packages (from pandas->datasets) (2025.2)
Requirement already satisfied: tzdata>=2022.7 in /root/miniconda3/lib/python3.12/site-packages (from pandas->datasets) (2025.2)
Requirement already satisfied: six>=1.5 in /root/miniconda3/lib/python3.12/site-packages (from python-dateutil>=2.8.2->pandas->datasets) (1.17.0)
Installing collected packages: multiprocess, pandas, datasets
Successfully installed datasets-3.5.1 multiprocess-0.70.16 pandas-2.2.3
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
```

# 2. 配置网络代理

```python
os.environ["HTTP_PROXY"] = "http://127.0.0.1:10080"
os.environ["HTTPS_PROXY"] = "http://127.0.0.1:10080"
```

# 3. 加载数据集

```python
from datasets import load_dataset

reasoning_dataset = load_dataset("unsloth/OpenMathReasoning-mini", split="cot")
```

```text
Generating cot split: 100%|██████████| 19252/19252 [00:01<00:00, 18337.63 examples/s]
```

```python
non_reasoning_dataset = load_dataset("mlabonne/FineTome-100k", split="train")
```

```text
Generating train split: 100%|██████████| 100000/100000 [00:01<00:00, 98397.79 examples/s] 
```

# 4. 查看数据集详情

```python
reasoning_dataset
```

```text
Dataset({
    features: ['expected_answer', 'problem_type', 'problem_source', 'generation_model', 'pass_rate_72b_tir', 'problem', 'generated_solution', 'inference_mode'],
    num_rows: 19252
})
```

```python
non_reasoning_dataset
```

```text
Dataset({
    features: ['conversations', 'source', 'score'],
    num_rows: 100000
})
```

# 5. 构建对话数据

```python
def generate_conversation(examples):
    problems  = examples["problem"]
    solutions = examples["generated_solution"]
    conversations = []
    for problem, solution in zip(problems, solutions):
        conversations.append([
            {"role" : "user",      "content" : problem},
            {"role" : "assistant", "content" : solution},
        ])
    return { "conversations": conversations, }
```

# 6. 应用聊天模板

```python
reasoning_conversations = tokenizer.apply_chat_template(
    reasoning_dataset.map(generate_conversation, batched = True)["conversations"],
    tokenize = False,
)
```

```text
Map: 100%|██████████| 19252/19252 [00:01<00:00, 11251.61 examples/s]
```

```python
reasoning_conversations[0]
```
