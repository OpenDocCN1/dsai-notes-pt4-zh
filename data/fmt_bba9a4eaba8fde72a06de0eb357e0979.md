
# 日志解读、部署与微调指南

## API 请求日志解读

> 从您提供的日志信息来看，这是一个API请求的日志，其中包含了用户IP、时间、鉴权信息、请求的URL路径等信息。下面是对这些信息的简单解读：

1. `User IP` `::ffff:172.17.0.1`: 这是用户的IP地址，`::ffff:172.17.0.1`是一个IPv6的地址，实际上表示的是IPv4地址`172.17.0.1`。

2. `Time` `8/18/2023, 3:06:27 AM`: 这是请求发生的时间。

3. `Auth` `admin did not provide an api key`: 这表明用户（在这种情况下是管理员）没有提供API密钥进行鉴权。

4. `Proxy` `v1/chat/completions?path=v1&path=chat&path=completions`: 这是被代理的请求路径。

5. `Base Url` `http://aistudio:8000`: 这是服务器的基础URL。

6. `OpenAI Route` `params { path: [ 'dashboard', 'billing', 'usage' ] }`: 这是请求的OpenAI路由参数。

7. `Auth` `allowed hashed codes: []`: 这是允许的哈希码列表，看起来这个列表是空的。

8. `Auth` `got access code::`: 这表明获取到了访问码，但是看起来访问码是空的。

9. `Auth` `hashed access code: d41d8cd98f00b204e9800998ecf8427e`: 这是获取到的访问码的哈希值，但由于原始的访问码是空的，所以这个哈希值实际上是空字符串的MD5哈希值。

> 总的来说，这个日志显示了一个API请求的详细信息，但是看起来这个请求没有提供API密钥，也没有提供访问码，可能无法通过鉴权。

## 模型容器 HTTP 日志解读

在模型容器这边可以看到日志

```
2023-08-18T03:05:07.580954421Z INFO:     172.17.0.1:32930 - "POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1" 200 OK
2023-08-18T03:06:08.489583122Z INFO:     172.17.0.1:32932 - "POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1" 200 OK
2023-08-18T03:06:31.109598989Z INFO:     172.17.0.1:32934 - "POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1" 200 OK
```

> 这条日志信息是HTTP服务器日志的一部分，它提供了关于特定HTTP请求的信息。下面是对这条日志的解读：

+ `2023-08-18T03:06:31.109598989Z`: 这是请求发生的时间，使用的是ISO 8601的日期和时间表示法，包括了日期、时间和时区（Z表示UTC时区）。

+ `INFO`: 这是日志的级别，表示这是一条信息级别的日志，通常用于描述正常的运行情况。

+ `172.17.0.1:32934`: 这是发起请求的客户端的IP地址和端口号。

+ `"POST /v1/chat/completions?path=v1&path=chat&path=completions HTTP/1.1"`: 这是请求的详细信息，包括了HTTP的方法（POST）、请求的路径（`/v1/chat/completions`）以及路径参数（`path=v1&path=chat&path=completions`）和HTTP的版本（HTTP/1.1）。

+ `200 OK`: 这是服务器对请求的响应状态码和状态消息，200表示请求已成功处理，OK是对应的状态消息。

> 总的来说，这条日志告诉我们在特定的时间，来自特定IP地址和端口的客户端向服务器发起了一个POST请求，请求的路径是`/v1/chat/completions`，路径参数是`path=v1&path=chat&path=completions`，服务器已经成功处理了这个请求并返回了200 OK的响应。

这项功能测试完成之后，在命令行界面中，使用`ctrl+c`终止运行脚本运行

## 部署备注

### 备注 1：使用 docker-compose 正式部署

正式部署可以使用以下`docker-compose`文件

```
version: '3.1'

services:

  chatgpt-next-web:

    image: yidadaa/chatgpt-next-web

    restart: always

    environment:

      BASE_URL: http://aistudio:8000

    ports:

      - 3000:3000
```

### 备注 2：使用 chatbot-ui 部署

另一个可用项目`chatbot-ui`：`https://github.com/mckaywrigley/chatbot-ui`

```
docker run -e OPENAI_API_HOST=http://192.168.14.111:8000 -e OPENAI_API_KEY=sk-XXXXXXXXXXXXXXXXXXXX -p 3000:3000 ghcr.io/mckaywrigley/chatbot-ui:main
```

## 微调

> 参照`https://github.com/THUDM/ChatGLM2-6B/tree/main`和`https://github.com/THUDM/P-tuning-v2/ptuning`对`ChatGLM2-6B`模型使用基于`P-Tuning v2`的微调。`P-Tuning v2`将需要微调的参数量减少到原来的 0.1%，再通过模型量化、`Gradient Checkpoint`等方法，最低只需要 7GB 显存即可运行。

### 环境准备

使用`chatglm2-6b`作为测试环境

```
docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -v $(pwd)/chatglm2-6b:/app/chatglm2-6b --restart=always -d -p 7860:7860 -p 8000:8000 -p 8501:8501 --name=chatglm2 chatglm2:base python /app/chatglm2-6b/ChatGLM2-6B/openai_api.py
```

进入容器上下文

```
docker exec -it chatglm2 bash
```

确定现在位于正确的路径下 `/app/chatglm2-6b/ChatGLM2-6B`

```
root@1e141c20dc2e:/app/chatglm2-6b/ChatGLM2-6B# pwd

/app/chatglm2-6b/ChatGLM2-6B

root@1e141c20dc2e:/app/chatglm2-6b/ChatGLM2-6B# dir

FAQ.md         README.md     __pycache__  chatglm2-6b  evaluation     ptuning           resources  web_demo.py

MODEL_LICENSE  README_EN.md  api.py       cli_demo.py  openai_api.py  requirements.txt  utils.py   web_demo2.py
```

下面以 [ADGEN](https://aclanthology.org/D19-1321.pdf) (广告生成) 数据集为例介绍代码的使用方法。

ADGEN 数据集任务为根据输入（`content`）生成一段广告词（`summary`）。

```json
{
    "content": "类型#上衣*版型#宽松*版型#显瘦*图案#线条*衣样式#衬衫*衣袖型#泡泡袖*衣款式#抽绳",
    "summary": "这件衬衫的款式非常的宽松，利落的线条可以很好的隐藏身材上的小缺点，穿在身上有着很好的显瘦效果。领口装饰了一个可爱的抽绳，漂亮的绳结展现出了十足的个性，配合时尚的泡泡袖型，尽显女性甜美可爱的气息。"
}
```

从 [Google Drive](https://drive.google.com/file/d/13_vf0xRTQsyneRKdD1bZIr93vBGOczrk/view?usp=sharing) 或者 [Tsinghua Cloud](https://cloud.tsinghua.edu.cn/f/b3f119a008264b1cabd1/?dl=1) 下载处理好的 ADGEN 数据集，将解压后的 `AdvertiseGen` 目录放到本目录下。

### 训练

进入`ptuning`目录并查看目录内容

```
cd ptuning
```

```
root@1e141c20dc2e:/app/chatglm2-6b/ChatGLM2-6B# cd ptuning/

root@1e141c20dc2e:/app/chatglm2-6b/ChatGLM2-6B/ptuning# dir

AdvertiseGen  __pycache__   deepspeed.json        evaluate.sh           main.py  train.sh       trainer.py          web_demo.py

README.md     arguments.py  ds_train_finetune.sh  evaluate_finetune.sh  output   train_chat.sh  trainer_seq2seq.py  web_demo.sh
```

安装依赖环境

```
pip install rouge_chinese nltk jieba datasets
```

> 这个命令是用来安装四个Python库的。以下是这些库的简单介绍：

1. `rouge_chinese`: 这是一个用于评估自然语言生成模型（如机器翻译、文本摘要等）的库，特别是针对中文。它提供了ROUGE（Recall-Oriented Understudy for Gisting Evaluation）的评价指标，这是一种常用的评价自然语言生成模型的方法。

2. `nltk`: NLTK（Natural Language Toolkit）是一个非常强大的自然语言处理库，它提供了大量的功能，包括词性标注、命名实体识别、文本分类、文本挖掘、词干提取、语义理解等。

3. `jieba`: Jieba是一个非常流行的中文分词库。分词是自然语言处理中的一个重要步骤，特别是对于中文。Jieba提供了基于词典的分词方法，也支持新词识别和自定义词典。

4. `datasets`: 这是Hugging Face公司开发的一个库，用于方便地加载和预处理各种自然语言处理的数据集。Hugging Face是一个专注于自然语言处理的开源公司，他们开发了很多有用的工具和模型，如`Transformers`库。

> 通过`pip install`命令，你可以将这些库安装到你的Python环境中，然后在你的代码中使用这些库的功能。

`main.py`解析

> 这个Python脚本主要用于微调（`fine-tuning`）基于Hugging Face库的序列到序列（`sequence-to-sequence`）模型。这是一个常用的训练和评估自然语言处理模型的脚本，适用于诸如机器翻译、文本生成、文本摘要等任务。

> 以下是一些主要的功能点：

1. 数据加载和预处理：脚本加载了训练、验证和测试数据集，并对数据进行了预处理，包括对输入和目标进行分词，以及对数据进行必要的截断和填充。

2. 模型加载：脚本加载了预训练的模型和分词器。如果指定了模型的检查点，脚本还会加载检查点中的模型状态。

3. 训练：如果指定了训练参数，脚本会对模型进行训练，并在训练结束后保存模型。

4. 评估和预测：如果指定了评估或预测参数，脚本会对模型进行评估或预测，并计算一些评价指标，如ROUGE和BLEU。

5. 日志和指标记录：脚本还包含了一些日志和指标记录的代码，用于记录训练和评估过程中的信息。

> 总的来说，这个脚本提供了一个完整的流程，用于加载数据、加载和微调模型、进行模型评估和预测，并记录相关的信息。

`train.sh`解析

```
PRE_SEQ_LEN=128

LR=2e-2

NUM_GPUS=1

torchrun --standalone --nnodes=1 --nproc-per-node=$NUM_GPUS main.py \

    --do_train \

    --train_file AdvertiseGen/train.json \

    --validation_file AdvertiseGen/dev.json \

    --preprocessing_num_workers 10 \

    --prompt_column content \

    --response_column summary \

    --overwrite_cache \

    --model_name_or_path /app/chatglm2-6b \ #此处需要进行模型路径的替换

    --output_dir output/adgen-chatglm2-6b-pt-$PRE_SEQ_LEN-$LR \

    --overwrite_output_dir \

    --max_source_length 64 \

    --max_target_length 128 \

    --per_device_train_batch_size 1 \

    --per_device_eval_batch_size 1 \

    --gradient_accumulation_steps 16 \

    --predict_with_generate \

    --max_steps 3000 \

    --logging_steps 10 \

    --save_steps 1000 \

    --learning_rate $LR \

    --pre_seq_len $PRE_SEQ_LEN \

    --quantization_bit 4
```

> 这个脚本是用来启动一个PyTorch训练任务的。它使用了`torchrun`命令，这是PyTorch的一个命令行工具，用于在多GPU或多节点环境中运行分布式训练。

> 以下是脚本中每个参数的解释：

1. `PRE_SEQ_LEN=128`: 设置预处理序列长度为128。

2. `LR=2e-2`: 设置学习率为0.02。

3. `NUM_GPUS=1`: 设置使用的GPU数量为1。

> `torchrun`命令的参数：

1. `--standalone`: 运行一个独立的训练任务，不需要与其他节点通信。

2. `--nnodes=1`: 设置使用的节点数量为1。

3. `--nproc-per-node=$NUM_GPUS`: 每个节点上的进程数量，这里设置为1，即每个节点上运行一个进程。

> 接下来是`main.py`脚本的参数：

1. `--do_train`: 进行训练。

2. `--train_file AdvertiseGen/train.json`: 训练数据的文件路径。

3. `--validation_file AdvertiseGen/dev.json`: 验证数据的文件路径。

4. `--preprocessing_num_workers 10`: 用于数据预处理的工作进程数量。

5. `--prompt_column content`: 输入列的名称。

6. `--response_column summary`: 输出列的名称。

7. `--overwrite_cache`: 如果设置，将在运行前清除缓存。

8. `--model_name_or_path /app/chatglm2-6b`: 预训练模型的名称或路径。

9. `--output_dir output/adgen-chatglm2-6b-pt-$PRE_SEQ_LEN-$LR`: 输出目录的路径，其中包括训练过程中保存的模型和其他文件。

10. `--overwrite_output_dir`: 如果设置，将在运行前清除输出目录。

11. `--max_source_length 64`: 输入序列的最大长度。

12. `--max_target_length 128`: 输出序列的最大长度。

13. `--per_device_train_batch_size 1`: 每个设备的训练批次大小。

14. `--per_device_eval_batch_size 1`: 每个设备的评估批次大小。

15. `--gradient_accumulation_steps 16`: 梯度累积步数，这可以有效地增加总的批次大小。

16. `--predict_with_generate`: 使用生成模式进行预测。

17. `--max_steps 3000`: 训练的最大步数。

18. `--logging_steps 10`: 每隔多少步记录一次日志。

19. `--save_steps 1000`: 每隔多少步保存一次模型。

20. `--learning_rate $LR`: 学习率。

21. `--pre_seq_len $PRE_SEQ_LEN`: 预处理序列长度。

22. `--quantization_bit 4`: 量化位数，这是用于模型压缩的技术。

> 这个脚本的目的是使用指定的预训练模型在给定的训练数据上进行训练，并将训练过程中的模型和其他文件保存到指定的输出目录。

启动`P-Tuning v2`模型训练

```
bash train.sh
```
