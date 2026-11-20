
# 环境准备

## 安装 transformers

执行以下命令安装 `transformers`：

```
!pip install transformers
```

输出：

```
Looking in indexes: http://mirrors.aliyun.com/pypi/simple
Collecting transformers
  Downloading http://mirrors.aliyun.com/pypi/packages/d9/b7/98f821d70102e2d38483bbb7013a689d2d646daa4495377bc910374ad727/transformers-4.41.2-py3-none-any.whl (9.1 MB)
Requirement already satisfied: filelock in /root/miniconda3/lib/python3.12/site-packages (from transformers) (3.14.0)
Requirement already satisfied: huggingface-hub<1.0,>=0.23.0 in /root/miniconda3/lib/python3.12/site-packages (from transformers) (0.23.2)
Requirement already satisfied: numpy>=1.17 in /root/miniconda3/lib/python3.12/site-packages (from transformers) (1.26.4)
Requirement already satisfied: packaging>=20.0 in /root/miniconda3/lib/python3.12/site-packages (from transformers) (23.2)
Requirement already satisfied: pyyaml>=5.1 in /root/miniconda3/lib/python3.12/site-packages (from transformers) (6.0.1)
Collecting regex!=2019.12.17 (from transformers)
  Downloading http://mirrors.aliyun.com/pypi/packages/91/28/a187c6eccd980454b668e837df35ad0935989a6b00b9288bc094296bc2f6/regex-2024.5.15-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (788 kB)
Requirement already satisfied: requests in /root/miniconda3/lib/python3.12/site-packages (from transformers) (2.31.0)
Collecting tokenizers<0.20,>=0.19 (from transformers)
  Downloading http://mirrors.aliyun.com/pypi/packages/80/54/12047a69f5b382d7ee72044dc89151a2dd0d13b2c9bdcc22654883704d31/tokenizers-0.19.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (3.6 MB)
Collecting safetensors>=0.4.1 (from transformers)
  Downloading http://mirrors.aliyun.com/pypi/packages/3f/7c/c894b48c98ffe7fa82358c089818369a71c05d8483099cd1a9266fbd605c/safetensors-0.4.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.2 MB)
Requirement already satisfied: tqdm>=4.27 in /root/miniconda3/lib/python3.12/site-packages (from transformers) (4.66.2)
Requirement already satisfied: fsspec>=2023.5.0 in /root/miniconda3/lib/python3.12/site-packages (from huggingface-hub<1.0,>=0.23.0->transformers) (2024.2.0)
Requirement already satisfied: typing-extensions>=3.7.4.3 in /root/miniconda3/lib/python3.12/site-packages (from huggingface-hub<1.0,>=0.23.0->transformers) (4.12.1)
Requirement already satisfied: charset-normalizer<4,>=2 in /root/miniconda3/lib/python3.12/site-packages (from requests->transformers) (2.0.4)
Requirement already satisfied: idna<4,>=2.5 in /root/miniconda3/lib/python3.12/site-packages (from requests->transformers) (3.7)
Requirement already satisfied: urllib3<3,>=1.21.1 in /root/miniconda3/lib/python3.12/site-packages (from requests->transformers) (2.1.0)
Requirement already satisfied: certifi>=2017.4.17 in /root/miniconda3/lib/python3.12/site-packages (from requests->transformers) (2024.2.2)
Installing collected packages: safetensors, regex, tokenizers, transformers
Successfully installed regex-2024.5.15 safetensors-0.4.3 tokenizers-0.19.1 transformers-4.41.2
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
```

## 安装 tiktoken

执行以下命令安装 `tiktoken`：

```
!pip install tiktoken
```

输出：

```
Looking in indexes: http://mirrors.aliyun.com/pypi/simple
Collecting tiktoken
  Downloading http://mirrors.aliyun.com/pypi/packages/50/81/1842a22f15586072280364c2ab1e40835adaf64e42fe80e52aff921ee021/tiktoken-0.7.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.1 MB)
Requirement already satisfied: regex>=2022.1.18 in /root/miniconda3/lib/python3.12/site-packages (from tiktoken) (2024.5.15)
Requirement already satisfied: requests>=2.26.0 in /root/miniconda3/lib/python3.12/site-packages (from tiktoken) (2.31.0)
Requirement already satisfied: charset-normalizer<4,>=2 in /root/miniconda3/lib/python3.12/site-packages (from requests>=2.26.0->tiktoken) (2.0.4)
Requirement already satisfied: idna<4,>=2.5 in /root/miniconda3/lib/python3.12/site-packages (from requests>=2.26.0->tiktoken) (3.7)
Requirement already satisfied: urllib3<3,>=1.21.1 in /root/miniconda3/lib/python3.12/site-packages (from requests>=2.26.0->tiktoken) (2.1.0)
Requirement already satisfied: certifi>=2017.4.17 in /root/miniconda3/lib/python3.12/site-packages (from requests>=2.26.0->tiktoken) (2024.2.2)
Installing collected packages: tiktoken
Successfully installed tiktoken-0.7.0
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
```

# 环境检查

导入 `torch` 并检查 `torch.cuda.is_available()`：

```
import torch
torch.cuda.is_available()
```

输出：

```
True
```

# 模型下载

接下来导入 `modelscope` 库：

```
from modelscope import snapshot_download
```

输出：

```
2024-06-05 18:39:50,436 - modelscope - INFO - PyTorch version 2.3.0+cu121 Found.
2024-06-05 18:39:50,437 - modelscope - INFO - Loading ast index from /root/.cache/modelscope/ast_indexer
2024-06-05 18:39:50,437 - modelscope - INFO - No valid ast index found from /root/.cache/modelscope/ast_indexer, generating ast index from prebuilt!
2024-06-05 18:39:50,483 - modelscope - INFO - Loading done! Current index file version is 1.14.0, with md5 4a57d10c249fdcacb8d1f0d8eeb382d2 and a total number of 976 components indexed
```

然后进行模型下载：

```
# 模型下载
from modelscope import snapshot_download
model_dir = snapshot_download('ZhipuAI/glm-4-9b-chat')
```

下载进度：

```
Downloading: 100%|██████████| 654/654 [00:00<00:00, 5.15MB/s]
Downloading: 100%|██████████| 48.0/48.0 [00:00<00:00, 428kB/s]
Downloading: 100%|██████████| 126/126 [00:00<00:00, 927kB/s]
Downloading: 100%|██████████| 7.62k/7.62k [00:00<00:00, 10.5MB/s]
Downloading: 100%|█████████▉| 4.63G/4.63G [00:13<00:00, 379MB/s]
Downloading: 100%|█████████▉| 4.66G/4.66G [00:13<00:00, 374MB/s]
Downloading: 100%|█████████▉| 4.58G/4.58G [00:13<00:00, 357MB/s]
Downloading: 100%|█████████▉| 1.09G/1.09G [00:03<00:00, 339MB/s]
Downloading: 100%|██████████| 23.4k/23.4k [00:00<00:00, 61.1MB/s]
Downloading: 100%|██████████| 36.3k/36.3k [00:00<00:00, 18.6MB/s]
Downloading: 100%|██████████| 73.0/73.0 [00:00<00:00, 600kB/s]
Downloading: 100%|██████████| 8.66M/8.66M [00:00<00:00, 65.8MB/s]
Downloading: 100%|██████████| 49.7k/49.7k [00:00<00:00, 11.4MB/s]
Downloading: 100%|██████████| 4.59k/4.59k [00:00<00:00, 8.31MB/s]
```

查看 `model_dir`：

```
model_dir
```

输出：

```
'/root/autodl-tmp/glm-4-9b-chat'
```

# 模型推理

使用 `transformers` 后端进行推理。

以下是单卡服务器运行代码：

```
import torch
from modelscope import AutoModelForCausalLM, AutoTokenizer
```

## 设置设备

设置设备为 `CUDA`（GPU），以加速计算：

```
device = "cuda"
```

## 加载分词器

从预训练模型目录加载分词器，并信任远程代码：

```
tokenizer = AutoTokenizer.from_pretrained(model_dir, trust_remote_code=True)
```

## 定义输入查询

定义用户的输入查询：

```
query = "你好"
```

## 应用聊天模板

使用聊天模板应用分词器，并生成输入张量
