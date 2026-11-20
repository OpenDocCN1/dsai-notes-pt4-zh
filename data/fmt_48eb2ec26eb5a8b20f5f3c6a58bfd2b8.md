
# Git LFS 安装与 LLaMA2 模型部署

## Git LFS 安装

`git lfs install`是Git Large File Storage（LFS）的安装命令。Git LFS 是一种 Git 扩展，它用于处理大文件，如音频样本、视频、数据集等。默认情况下，Git 不适合处理大文件，因为它会将所有版本的所有文件都存储在本地仓库中，这可能会导致仓库变得非常大。Git LFS 解决了这个问题，它将大文件存储在远程服务器上，只在本地保留一个轻量级的引用。

当你运行`git lfs install`命令时，它将执行以下操作：

1. 安装一些必要的钩子（`hooks`），这些钩子将在执行`git`命令时运行，以确保大文件被正确地处理。例如，`pre-push`钩子将在推送到远程仓库之前运行，`post-checkout`钩子将在检出分支或提交后运行。

2. 设置一些必要的`Git`配置。例如，它将设置`filter.lfs.clean`和`filter.lfs.smudge`配置，这些配置定义了如何在将大文件添加到仓库和从仓库检出大文件时处理这些文件。

注意，在运行`git lfs install`命令之前，你需要先安装 Git LFS。你可以从 Git LFS 的 GitHub 仓库下载适合你的操作系统的安装程序。

## 模型下载

### 下载映像，并进行必要的路径修改

```
# MetaAI LLaMA2 Models (10~14GB vRAM)

git clone https://huggingface.co/meta-llama/Llama-2-7b-chat-hf

git clone https://huggingface.co/meta-llama/Llama-2-13b-chat-hf

mkdir meta-llama

mv Llama-2-7b-chat-hf meta-llama/

mv Llama-2-13b-chat-hf meta-llama/

# 或 Chinese LLaMA2 (10~14GB vRAM)

git clone https://huggingface.co/LinkSoul/Chinese-Llama-2-7b

mkdir LinkSoul

mv Chinese-Llama-2-7b LinkSoul/

# 或 Chinese LLaMA2 4BIT (5GB vRAM)

git clone https://huggingface.co/soulteary/Chinese-Llama-2-7b-4bit

mkdir soulteary

mv Chinese-Llama-2-7b-4bit soulteary/
```

### 以下载13b模型为例（7b忘了记录日志输出了）

```
chengzh@AIStudio:~/docker-llama2-chat$ git clone https://huggingface.co/meta-llama/Llama-2-13b-chat-hf

Cloning into 'Llama-2-13b-chat-hf'...

Username for 'https://huggingface.co': huaqloud

Password for 'https://huaqloud@huggingface.co':

remote: Enumerating objects: 52, done.

remote: Counting objects: 100% (41/41), done.

remote: Compressing objects: 100% (41/41), done.

remote: Total 52 (delta 19), reused 0 (delta 0), pack-reused 11

Unpacking objects: 100% (52/52), 497.59 KiB | 618.00 KiB/s, done.

Username for 'https://huggingface.co': huaqloud

Password for 'https://huaqloud@huggingface.co':

Filtering content: 100% (8/8), 8.48 GiB | 1.56 MiB/s, done.

Encountered 6 file(s) that may not have been copied correctly on Windows:

        model-00003-of-00003.safetensors

        pytorch_model-00003-of-00003.bin

        pytorch_model-00001-of-00003.bin

        pytorch_model-00002-of-00003.bin

        model-00002-of-00003.safetensors

        model-00001-of-00003.safetensors

See: `git lfs help smudge` for more details.
```

这个输出是`git clone`命令的结果，它从Hugging Face的模型仓库克隆了一个名为`Llama-2-13b-chat-hf`的项目。这个项目可能包含一个预训练的聊天模型。

下面是对这个输出的具体解析：

> 1. `Cloning into 'Llama-2-13b-chat-hf'...`: 这个消息表示开始克隆项目到本地的`Llama-2-13b-chat-hf`目录。
> 2. `Username for 'https://huggingface.co': huaqloud`: 这个消息表示`Git`请求输入用于`https://huggingface.co`的用户名，这里的用户名是`huaqloud`。
> 3. `Password for 'https://huaqloud@huggingface.co':`: 这个消息表示`Git`请求输入用于`https://huggingface.co`的密码。
> 4. `remote: Enumerating objects: 52, done.`: 这个消息表示`Git`开始列举远程仓库中的对象。这个仓库中有52个对象。
> 5. `Unpacking objects: 100% (52/52), 497.59 KiB | 618.00 KiB/s, done.`: 这个消息表示`Git`已经完成了对象的解压缩。
> 6. `Filtering content: 100% (8/8), 8.48 GiB | 1.56 MiB/s, done.`: 这个消息表示`Git LFS`已经完成了大文件的过滤。这个项目中有8个大文件，总大小为8.48 GiB。
> 7. `Encountered 6 file(s) that may not have been copied correctly on Windows:`: 这个消息表示在`Windows`上可能有6个文件没有被正确地复制。这可能是因为这些文件的大小超过了`Git LFS`在`Windows`上的限制。这些文件可能是模型的权重文件。
> 8. `See: git lfs help smudge for more details.`: 这个消息表示如果需要更多的信息，可以查阅`git lfs help smudge`命令的帮助信息。

总的来说，这个项目包含一个大模型（可能是一个聊天模型），模型的权重文件被存储在Git LFS中。在克隆这个项目时，可能有一些文件没有被正确地复制，这可能会影响到模型的使用。

总的来说，操作中注意以下三点：

1. 使用在`huggingface`上注册的用户名密码进行登录。

2. 耐心是美德，本手册涉及到的模型小则25GB大则92GB，一定要有足够的耐心，尤其是带宽不是太高的情况下。

3. 虽然有报错，但是似乎并不影响模型使用。

又及：中文版的两个模型`Chinese-Llama-2-7b`和 `Chinese-Llama-2-7b-4bit`下载时不需要登录

所有四个模型下载并调整好目录之后，我们的工作目录长这个样子

```
chengzh@AIStudio:~/docker-llama2-chat$ tree

.
├── LICENSE
├── LinkSoul
│   └── Chinese-Llama-2-7b
│       ├── README.md
│       ├── config.json
│       ├── generation_config.json
│       ├── pytorch_model-00001-of-00003.bin
│       ├── pytorch_model-00002-of-00003.bin
│       ├── pytorch_model-00003-of-00003.bin
│       ├── pytorch_model.bin.index.json
│       ├── special_tokens_map.json
│       ├── tokenizer.model
│       └── tokenizer_config.json
├── README.md
├── README_EN.md
├── docker
│   ├── Dockerfile.13b
│   ├── Dockerfile.7b
│   ├── Dockerfile.7b-cn
│   ├── Dockerfile.7b-cn-4bit
│   └── Dockerfile.base
├── llama.cpp
│   ├── Dockerfile.converter
│   └── Dockerfile.runtime
├── llama2-13b
│   ├── app.py
│   └── model.py
├── llama2-7b
│   ├── app.py
│   └── model.py
├── llama2-7b-cn
│   ├── app.py
│   └── model.py
├── llama2-7b-cn-4bit
│   ├── app.py
│   ├── model.py
│   └── quantization_4bit.py
├── meta-llama
│   ├── Llama-2-13b-chat-hf
│   │   ├── LICENSE.txt
│   │   ├── README.md
│   │   ├── Responsible-Use-Guide.pdf
│   │   ├── USE_POLICY.md
│   │   ├── config.json
│   │   ├── generation_config.json
│   │   ├── model-00001-of-00003.safetensors
│   │   ├── model-00002-of-00003.safetensors
│   │   ├── model-00003-of-00003.safetensors
│   │   ├── model.safetensors.index.json
│   │   ├── pytorch_model-00001-of-00003.bin
│   │   ├── pytorch_model-00002-of-00003.bin
│   │   ├── pytorch_model-00003-of-00003.bin
│   │   ├── pytorch_model.bin.index.json
│   │   ├── special_tokens_map.json
│   │   ├── tokenizer.json
│   │   ├── tokenizer.model
│   │   └── tokenizer_config.json
│   └── Llama-2-7b-chat-hf
│       ├── LICENSE.txt
│       ├── README.md
│       ├── USE_POLICY.md
│       ├── config.json
│       ├── generation_config.json
│       ├── model-00001-of-00002.safetensors
│       ├── model-00002-of-00002.safetensors
│       ├── model.safetensors.index.json
│       ├── pytorch_model-00001-of-00002.bin
│       ├── pytorch_model-00002-of-00002.bin
│       ├── pytorch_model.bin.index.json
│       ├── special_tokens_map.json
│       ├── tokenizer.json
│       ├── tokenizer.model
│       └── tokenizer_config.json
├── scripts
│   ├── make-13b.sh
│   ├── make-7b-cn-4bit.sh
│   ├── make-7b-cn.sh
│   ├── make-7b.sh
│   ├── run-13b.sh
│   ├── run-7b-cn-4bit.sh
│   ├── run-7b-cn.sh
│   └── run-7b.sh
└── soulteary
    └── Chinese-Llama-2-7b-4bit
        ├── README.md
        ├── config.json
        ├── generation_config.json
        ├── pytorch_model-00001-of-00002.bin
        ├── pytorch_model-00002-of-00002.bin
        ├── pytorch_model.bin.index.json
        ├── special_tokens_map.json
        ├── tokenizer.model
        └── tokenizer_config.json

14 directories, 79 files
```

## 运行容器

### 让容器跑起来

根据所需要的版本，一键运行LLaMA2模型容器化应用

```
# 7B

bash scripts/run-7b.sh

# 或 13B

bash scripts/run-13b.sh

# 或 Chinese 7B

bash scripts/run-7b-cn.sh

# 或 Chinese 7B 4BIT

bash scripts/run-7b-cn-4bit.sh
```

依然以7B为例分析容器是怎么跑起来的，以下是`scripts/run-7b.sh`的内容

```
#!/bin/bash

docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 --rm -it -v `pwd`/meta-llama:/app/meta-llama -p 7860:7860 soulteary/llama2:7b
```

这是一个 Bash 脚本，它用于运行一个 Docker 容器。让我们一步步地解析这个脚本：

> 1. `#!/bin/bash`: 这一行告诉系统这个脚本应该用哪个解释器执行。在这个例子中，脚本将由 Bash（Bourne Again SHell）解释器执行。
> 2. `docker run`: 这是 Docker 的一个命令，用于运行一个 Docker 容器。
> 3. `--gpus all`: 这个选项告诉 Docker 使用所有可用的 GPU。
> 4. `--ipc=host`: 这个选项设置 Docker 容器的 IPC（Inter-Process Communication）命名空间。在这个例子中，容器将使用主机的 IPC 命名空间，这意味着容器中的进程可以和主机中的进程共享内存。
> 5. `--ulimit memlock=-1 --ulimit stack=67108864`: 这些选项设置了容器的资源限制。在这个例子中，`memlock=-1` 移除了内存锁定的限制，`stack=67108864` 设置了栈的大小为 64MB。
> 6. `--rm`: 这个选项告诉 Docker 在容器退出时自动删除容器。
> 7. `-it`: 这两个选项一起使用，`-i` 保持 STDIN 打开，`-t` 分配一个伪终端，这使得你可以交互式地使用容器。
> 8. `` `-v `pwd`/meta-llama:/app/meta-llama` ``: 这个选项挂载了一个卷。在这个例子中，主机的 `meta-llama` 目录被挂载到了容器的 `/app/meta-llama` 目录。
> 9. `-p 7860:7860`: 这个选项发布了一个端口。在这个例子中，容器的 7860 端口被映射到了主机的 7860 端口。
> 10. `soulteary/llama2:7b`: 这是 Docker 容器的镜像名称。在这个例子中，容器将基于 `soulteary/llama2:7b` 这个镜像。

总的来说，这个脚本运行了一个 Docker 容器，这个容器使用了所有可用的 GPU，没有内存锁定的限制，栈的大小为 64MB，退出时自动删除，可以交互式地使用，`meta-llama` 目录被挂载到了 `/app/meta-llama`，容器的 7860 端口被映射到了主机的 7860 端口，容器基于 `soulteary/llama2:7b` 这个镜像。

### 以下是7b的容器运行过程

```
chengzh@AIStudio:~/docker-llama2-chat$ bash scripts/run-7b.sh

=============

== PyTorch ==

=============

NVIDIA Release 23.06 (build 63009835)
```
