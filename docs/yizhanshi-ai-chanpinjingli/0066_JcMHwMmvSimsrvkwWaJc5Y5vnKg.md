# 模型推理

> 来源：[https://v11enp9ok1h.feishu.cn/docx/VaxHdZmJBoNkpRxPYShcNDSKn6b](https://v11enp9ok1h.feishu.cn/docx/VaxHdZmJBoNkpRxPYShcNDSKn6b)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

本章节将带领开发者学习如何快速上手推理 CogVLM2 系列模型

请配合以下代码库中微调部分代码进行上手练习

https://github.com/THUDM/CogVLM2/tree/main/basic_demo

## 依赖安装

```
pip install -r requirements.txt
```

## 使用CLI运行demo

运行此代码，你需要制定模型的所有层都加载在一张显卡上。

```
CUDA_VISIBLE_DEVICES=0 python cli_demo.py
```

如果你有多张显卡并打算将模型的层进行手动拆分，以实现在更多较低显存的显卡上载入模型，你可以运行这个代码

```
python cli_demo_multi_gpu.py
```

在 cli_demo_multi_gpu.py 中，我们使用了 infer_auto_device_map

函数来自动分配模型的不同层到不同的GPU上。你需要设置 `max_memory` 参数来指定每张GPU的最大内存。例如，如果你有两张GPU，每张GPU的内存为23GiB，你可以这样设置：

```

device_map = infer_auto_device_map(
    model=model,
    max_memory={i: &quot;23GiB&quot; for i in range(torch.cuda.device_count())},
    # set 23GiB for each GPU, depends on your GPU memory, you can adjust this value
    no_split_module_classes=[&quot;CogVLMDecoderLayer&quot;]
)
```

## 运行本代码以开始在 WebUI 中对话。

```
CUDA_VISIBLE_DEVICES=0 p chainlit run web_demo.py
```

拉起对话后，你将能和模型进行对话，效果如下：

![](img/dcf8b39887e29784fe09025f1f340bc8.png)

# OpenAI API

我们提供了一个简单的示例，通过以下代码拉起模型，之后，您可以使用 OpenAI API格式的方式请求和模型的对话。

```
python openai_api_demo.py
```

开发者可以通过以下代码来调用模型

```
python openai_api_request.py
```

# 使用TGI加速推理

如果您希望使用TGI进行加速推理CogVLM2模型，您可以查看以下内容：

由于当前hugging face官方的tgi并不支持cogvlm2的推理。算法团队成员在tgi官方代码的基础上增加了cogvlm2的支持，您可以查看对应的Fork分支代码：

**https://github.com**/leizhao1234/cogvlm2

这里，我们还提供了如何从源代码构建镜像的方法：

1.  构建docker

```
docker build -t image_name .
```

1.  本地编译

```
#需cuda12.1
#
#下载conda并安装
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh
//加速rust下载
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup

#安装rust
curl --proto &#x27;=https&#x27; --tlsv1.2 -sSf https://sh.rustup.rs | sh

#创建conda环境
conda create -n tgi python=3.10
conda activate tgi
#下载protobuf
PROTOC_ZIP=protoc-21.12-linux-x86_64.zip
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v21.12/$PROTOC_ZIP
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc
sudo unzip -o $PROTOC_ZIP -d /usr/local &#x27;include/*&#x27;
rm -f $PROTOC_ZIP

pip install torch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 --index-url https://download.pytorch.org/whl/cu121
#进入text-text-generation-inference目录，编译基本组件
BUILD_EXTENSIONS=True make install # Install repository and HF/transformer fork with CUDA kernels
#编译kernel
cd server
make install-vllm-cuda
make build-apex
make install-flash-attention-v2-cuda
cd -
#启动服务
text-generation-launcher --model-id model_path --num-shard 1 --port 8080 --max-concurrent-requests 409600 --max-input-length 8190 --max-total-tokens 8192 --max-batch-prefill-tokens 8192 --trust-remote-code --max-waiting-tokens 2 --waiting-served-ratio 0.2 --cuda-memory-fraction 0.97 --cuda-graphs 0 --dtype bfloat16

#参考：https://github.com/huggingface/text-generation-inference#local-install

```

通过上述方式，您可以在本地运行TGI加速的CogVLM2模型。