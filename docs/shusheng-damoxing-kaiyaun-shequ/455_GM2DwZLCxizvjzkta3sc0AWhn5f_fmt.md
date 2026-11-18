
# vLLM 部署 InternVL 实战

> 来源：[https://aicarrier.feishu.cn/docx/MqbUdU61ooBwI1xXNgNc6ZzJnEH](https://aicarrier.feishu.cn/docx/MqbUdU61ooBwI1xXNgNc6ZzJnEH)

## 前言

`vLLM`对于一些同学来说可能比较陌生，它是个什么东西？检索一下，找到了它的官方文档，里面介绍说：“`vLLM` is a `Python` library that also contains pre-compiled `C++` and `CUDA` binaries.”

嗯，是一个`Python`库。这解释有些抽象，讲了一些东西，但是还仍然没让人搞懂`vLLM`是啥。

大致浏览官网文档，之后看到`vLLM`的功能有：

+   离线或者在线部署各种大模型，顺带加速推理。

+   提[供大模型的量](https://docs.vllm.ai/en/stable/quantization/int8.html)化，[比如`INT8`](https://docs.vllm.ai/en/stable/quantization/bnb.html), [BitsAn](https://github.com/vllm-project/vllm/issues/1481)dBytes, GPTQ, AWQ, and SqueezeLLM (source, source, source)

因此可以发现：`vllm`跟`lmdeploy`的功能一样，是一个集成的大模型部署工具。

`vLLM`和`LMDeployd`都能让各种各样的大模型用统一易用的方式部署出来，而且能方便地用代码进行加速推理和量化。

+   这个部署工具支持很多大模型，`InternVL2`系列大模型也在supported model list里：https://docs.vllm.ai/en/latest/models/supported_models.html#supported-vlms

![](img/e64283a360c88f31c931e1466962e97a.png)

> 如果没有在名单里，基本需要自己写个代码调用`vLLM`的库去对接`InternVL2`大模型了。

+   所以使用`vLLM`部署`InternVL2`系列大模型，直接根据`vLLM`工具部署VLM的文档进行即可：https://docs.vllm.ai/en/stable/models/vlm.html#

## 命令行直接推理 CLI demo

`CLI`是（command-line interface，命令行界面），`CLI demo`的意思是只靠命令行的代码就能做这个demo。

命令行里进行推理，这样加载模型、输入提示词都在代码里，输出结果打印在命令行里，简洁而方便。

### 命令行推理的详解教程

#### 创建环境，安装vLLM库

推荐选择`cuda11`系列的的开发机环境，`cuda12.2`的`intern studio`开发机实测连`vllm`官网的大语言模型简易推理都跑不通（但是在`colab`里能跑通）。

开发机选择`cuda11.7`，用下面的代码安装`vLLM`库

```
# 创建新环境，防止跟其他项目的环境冲突
conda create -n vllm_env python=3.10 -y

# 激活环境，并安装vllm库
conda activate vllm_env
# CUDA 11系列都兼容
export VLLM_VERSION=0.6.1.post1
export PYTHON_VERSION=310
pip install https://github.com/vllm-project/vllm/releases/download/v${VLLM_VERSION}/vllm-${VLLM_VERSION}+cu118-cp${PYTHON_VERSION}-cp${PYTHON_VERSION}-manylinux1_x86_64.whl --extra-index-url https://download.pytorch.org/whl/cu118
```

> 这一步花了很多时间，主要是开发机装`torch`速度慢下来了，100K/s龟速下载800M的linux安装包，下载耗时一小时左右。

#### 下载大模型

使用`InternVL2-2B`模型进行测试，因此从`huggingface`仓库里下载它：

```
cd /root && mkdir vllm_test
cd /root/vllm_test && mkdir model

# 安装huggingface-hub库，方便从hf里下载或上传文件
pip install huggingface-hub
# 从hf的`OpenGVLab/InternVL2-2B`仓库里下载模型到本地的/root/model/InternVL2-2B文件夹
huggingface-cli download OpenGVLab/InternVL2-2B --local-dir /root/vllm_test/model/InternVL2-2B
```

> 如果用的是`intern-studio`的开发环境，可以直接调用/share文件夹的`InternVL2-2B`，不需要再下载了。这个模型的路径为：

```
"/share/new_models/OpenGVLab/InternVL2-2B"
```

> 因此使用软链接，把/share文件夹的`InternVL2-2B`模型映射到`/root/vllm_test/model/InternVL2-2B`位置：

```
ln -s /share/new_models/OpenGVLab/InternVL2-2B /root/vllm_test/model/InternVL2-2B
```

+   不映射也行，只是后面推理的脚本里，大模型的路径要修改为`/share/new_models/OpenGVLab/InternVL2-2B`

+   映射之后，想删除软链接，则运行`unlink /root/vllm_test/model/InternVL2-2B`

#### 执行推理

在`/root/vllm_test`文件夹里创建推理所用的脚本文件`vllm_cli_demo.py`：

```
touch /root/vllm_test/vllm_cli_demo.py
```

之后打开这个`vllm_cli_demo.py`，把下面的代码粘贴进去：

```
from vllm import LLM
from PIL import Image

# 加载模型
llm = LLM(model="/root/vllm_test/model/InternVL2-2B", trust_remote_code=True, enforce_eager=True)

# 构建提示词
prompt = "USER: <image>\n描述一下图片的内容?\nASSISTANT:"

# 加载图像
image = Image.open("/root/vllm_test/1.jpg")

# 生成输出
outputs = llm.generate({
    "prompt": prompt,
    "multi_modal_data": {"image": image},
})

# 打印生成的文本
for o in outputs:
    generated_text = o.outputs[0].text
    print(generated_text)
```

+   `trust_remote_code`参数不加的话，会报错。

+   加载模型的时候，使用了`enforce_eager=True`的参数，目的是始终使用 `Eager` 模式 `PyTorch`。

> 如果不加这个参数，那就默认为 `False`，将混合使用 `Eager` 模式和 `CUDA` 图以获得最大性能和灵活性。但是会出现`cuda error`的报错：

```
RuntimeError: CUDA error: API call is not supported in the installed CUDA driver
CUDA kernel errors might be asynchronously reported at some other API call, so the stacktrace below might be incorrect.
For debugging consider passing CUDA_LAUNCH_BLOCKING=1
Compile with `TORCH_USE_CUDA_DSA` to enable device-side assertions.
```

图片推理还需要有图片来测试，我测试的时候图片路径是`"/root/vllm_test/1.jpg"`，因此需要在`/root/vllm_test`里放一个`1.jpg`的图片。我在里面放了一张哆啦A梦的图片：

![](img/dbe1fb26ae31b3e91910b9b606dd0927.png)

之后回到命令行，执行这个推理的代码：

```
conda activate vllm_env
python /root/vllm_test/vllm_cli_demo.py
```

### 命令行推理的简易教程

三个步骤：安装`vllm`库、下载大模型、执行推理，代码如下：

安装`vllm`库：

```
# 创建新环境，防止跟其他项目的环境冲突
conda create -n vllm_env python=3.10 -y

# 激活环境，并安装vllm库
conda activate vllm_env
# CUDA 11系列都兼容
export VLLM_VERSION=0.6.1.post1
export PYTHON_VERSION=310
pip install https://github.com/vllm-project/vllm/releases/download/v${VLLM_VERSION}/vllm-${VLLM_VERSION}+cu118-cp${PYTHON_VERSION}-cp${PYTHON_VERSION}-manylinux1_x86_64.whl --extra-index-url https://download.pytorch.org/whl/cu118
```

> 这一步花了很多时间，主要是开发机装`torch`速度慢下来了，100K/s龟速下载。

下载大模型(直接建立软链接)：

```
ln -s /share/new_models/OpenGVLab/InternVL2-2B /root/vllm_test/model/InternVL2-2B
```

执行推理：

> 上传`1.jpg`，并修改`/root/vllm_test/vllm_cli_demo.py`的内容

```
conda activate vllm_env
python /root/vllm_test/vllm_cli_demo.py
```

执行代码，成功得到推理的结果：

![](img/d841708fdbf9f63c571d7807587b1268.png)

## 前后端分离推理 API demo

`API`（Application Programming Interface,应用程序编程接口）的便利之处在于：大模型部署完毕后，封装一下，分发给很多主机去调用，这些主机不使用本地的`GPU`，而是用部署大模型那台主机的`GPU`。

这样部署大模型的是后端，调用`API`的是前端。前后端分离后，一个大模型的`API`可以给多个人用。一个人也可以用多个大模型的`API`。而且大部分`API`都会有一个`OpenAI`格式的`API`，这样`pip install oepnai`之后，前端脚本调用各种`OpenAI`格式的`api`，代码基本不用改，换一个`url`和`api key`即可。

### 后端部署

新建一个终端，然后执行下面代码：

```
conda activate vllm_env
vllm serve /root/vllm_test/model/InternVL2-2B --max-model-len 4096 --trust-remote-code --enforce-eager --limit-mm-per-prompt image=2
```

运行后便把`InternVL2-2B`模型部署为`OpenAI`格式的`api`了，默认部署在本地的8000端口。有了这个`api`，能接入各种前端调用的脚本里，替代原先收费的`OpenAI`的`api key`。

### 前端调用

另外开启一个终端，执行调用`api`的代码`/root/vllm_test/vllm_api_demo.py`。

先用下面代码创建空文件：

```
touch /root/vllm_test/vllm_api_demo.py
```

然后把调用`api`的`python`代码，复制了粘贴进`vllm_api_demo.py`里：

`image_url = "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg";`

```
from openai import OpenAI

openai_api_key = "EMPTY";
openai_api_base = "http://localhost:8000/v1";

client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)

# Single-image input inference
# models.list()返回一个模型列表，每个模型都有一个id属性
model_name = client.models.list().data[0].id

chat_response = client.chat.completions.create(
    model=model_name,
    messages=[{
        # NOTE: The prompt formatting with the image token `<image>` is not needed
        # since the prompt will be processed automatically by the API server.
        {"type": "text", "text": "描述一下图片的内容"},
        {"type": "image_url", "image_url": {"url": image_url}},
    }],
)
print("Chat completion output:", chat_response.choices[0].message.content)
```

> 备注，`image_url`如果直接赋值为图片的路径，会有个报错：

```
openai.BadRequestError: Error code: 400 - {'object': 'error', 'message': "Invalid 'image_url': A valid 'image_url' must start with either 'data:image' or 'http'.", 'type': 'BadRequestError', 'param': None, 'code': 400}
```

> `image_url`目前就有`vLLM`的web demo文档里的`url`图片，图片是一幅优美的田园风景照片，具体如下：

> https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg

![](img/74d1413bc20cb3982a1e1439038724ce.png)

之后执行这个调用`api`的代码：

```
conda activate vllm_env
python /root/vllm_test/vllm_api_demo.py
```

运行的结果如下：

![](img/98cda189aeccc7b2a1166854cf7fb288.png)

## 网页端交互推理 Web demo

### 编写web demo脚本

在`/root/vllm_test`文件夹里创建推理所用的脚本文件`vllm_web_demo.py`：

```
touch /root/vllm_test/vllm_web_demo.py
```

根据上面的大语言模型的web demo代码，结合之前前后端分离的`API` demo代码，写了如下的`vllm_web_demo.py`：



```
import argparse
import json
import requests
import gradio as gr
from openai import OpenAI
import base64

openai_api_key = "EMPTY"
openai_api_base = "http://localhost:8000/v1"

client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)

# models.list()返回一个模型列表，每个模型都有一个id属性
model_name = client.models.list().data[0].id

def http_image_bot(question, image):
    headers = {"User-Agent": "vLLM Client"}

    # 读取图片并转换为 base64 格式
    with open(image, "rb") as img_file:
        encoded_image = base64.b64encode(img_file.read()).decode('utf-8')
    
    # 构造 data:image 格式的图片URL
    image_data_url = f"data:image/jpeg;base64,{encoded_image}"
    
    # 构造请求体
    pload = {
        "model": model_name,
        "messages": [
            {"role": "user", "content": [
                {"type": "text", "text": question},
                {"type": "image_url", "image_url": {"url": image_data_url}}
            ]}
        ]
    }

    response = requests.post(args.model_url, headers=headers, json=pload)
    print(response.text)  # 打印完整的响应
    response_data = response.json()

    # 提取返回的文本内容
    return response_data['choices'][0]['message']['content']


def build_demo():
    with gr.Blocks() as demo:
        gr.Markdown("# vLLM image and question demo\n")
        image_input = gr.Image(label="Upload an image", type="filepath")
        text_input = gr.Textbox(label="Ask a question about the image")
        outputbox = gr.Textbox(label="Output", placeholder="Generated result")
        submit_button = gr.Button("Submit")

        submit_button.click(http_image_bot, [text_input, image_input], outputbox)
    return demo


if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--host", type=str, default=None)
    parser.add_argument("--port", type=int, default=8001)
    parser.add_argument("--model-url", type=str, default="http://localhost:8000/v1/chat/completions")
    args = parser.parse_args()

    demo = build_demo()
    demo.queue().launch(server_name=args.host, server_port=args.port, share=True)
```

把上面代码粘贴进`/root/vllm_test/vllm_web_demo.py`里，等待运行。

## 启动web demo，开始交互式推理

之后回到命令行，先执行后端的部署代码：

```
conda activate vllm_env
vllm serve /root/vllm_test/model/InternVL2-2B --max-model-len 4096 --trust-remote-code --limit-mm-per-prompt image=2 --enforce-eager
```

再新开一个终端，执行前端web页面的代码：

```
conda activate vllm_env
pip install gradio # 之前没用到gradio就没装，这时候得补装gradio库了
python /root/vllm_test/vllm_web_demo.py
```

运行后得到的web demo的网页如下：

![](img/a63acb61343c2284a9c786d33e719410.png)

回到控制台，情况如下：

![](img/c651e370df87eeb062c653d69331c3a7.png)

这里让我困惑的是，回答远超预期了。因为之前单图推理的时候，也是InternVL2-2B模型，推理的结果是简单的一句话：“图中是一个动漫形象，是著名的哆啦A梦”。

但是这个交互推理的结果，一下子冒出了很多个提问和回答，看着有些像MindSearch了。

我再次进行图片推理，得到了这样的效果：

![](img/7fa93f7608d73544a0da6ad9676d3766.png)

为什么web demo推理会出现这种“其他问题”或者“了解更多”的延伸？可以同学之间互相交流，探讨原因。

## 多图推理

正常使用的话，轻量使用是单图用`cli_demo`推理或者`web_demo`推理。但是批量使用的话，那就需要把整个文件夹里的所有图片都用VLM进行推理了。

这时候`web_demo`不再实用，`cli_demo`或者`api_demo`的推理方式，就可以封装成一个函数，然后多次调用。也可以载入图片的时候，把图片放入列表里，同时载入模型进行推理，这时候就需要比较大的显存。

单图推理已经给出，多图推理基本很容易实现。如果vllm部署InternVL2进入新赛季的课程了，那么这个可以作为学员独立实现的进阶任务。

## vllm量化

假如目前只有8G显存，而InternVL2-8B模型有16G，怎么在8G显存的设备上跑InternVL2-8B模型？

做为成功通关LMDeploy量化的高手，可以试试这个挑战任务。参考vllm的量化文档，实现8G显存跑InternVL2-8B模型。如果LMDeploy理解深入，而且看文档和debug能力都有，那么即可独立解决这个挑战任务。
