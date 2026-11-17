# 多模态大模型Qwen2-VL安装部署与调用教程

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/RzWwdqfNtoiyVIxjHNJcRQAense](https://kq4b3vgg5b.feishu.cn/docx/RzWwdqfNtoiyVIxjHNJcRQAense)

---

## 模型介绍

![](img/2aa371e6ffed338b80a9ef2a30c35972.png)

Qwen2VL是由阿里团队开源的最新图像、视频识别的多模态大模型。该模型支持对各种分辨率和比例的图像的理解并返回文本对话，并具有基本的结合视觉的文本理解能力、数学推理能力、多语言能力。Qwen2VL可以理解 20 分钟以上的视频，以进行高质量的基于视频的问答、对话、内容创建等。

除了英文和中文外，Qwen2-VL 现在还支持理解图像中不同语言的文本，包括大多数欧洲语言、日语、韩语、阿拉伯语、越南语等。

本次开源的模型包括2B、7B、72B参数的模型，运行对应模型大致需要的显存分别至少为6G、34G、144G。

+   图像能力测试

![](img/22a42c91711a3bef9dceb3baa4f7cdb6.png)

+   视频能力测试

![](img/659143d0a2456623dd0bf89baa052b68.png)

## 本地部署流程

+   Step 1. 创建conda虚拟环境

Conda创建虚拟环境的意义在于提供了一个隔离的、独立的环境，用于Python项目和其依赖包的管理。每个虚拟环境都有自己的Python运行时和一组库。这意味着我们可以在不同的环境中安装不同版本的库而互不影响。根据官方文档信息建议Python版本3.10以上。创建虚拟环境的办法可以通过使用以下命令创建：

```
# qwen2_5 是你想要给环境的名称，python=3.11 指定了要安装的Python版本。你可以根据需要选择不同的名称和/或Python版本。

conda create -n qwen2_5 python=3.11
```

![](img/d8b0fa30a050f183d100ada49f8310f0.png)

创建虚拟环境后，需要激活它。使用以下命令来激活刚刚创建的环境。如果成功激活，可以看到在命令行的最前方的括号中，就标识了当前的虚拟环境。虚拟环境创建完成后接下来安装torch。

![](img/25a2fa38cfb31c4f6298cf1a2bc5aad6.png)

如果忘记或者想要管理自己建立的虚拟环境，可以通过conda env list命令来查看所有已创建的环境名称。

![](img/f73f79da0cc102579fbcfc1c94a52fbf.png)

如果需要卸载指定的虚拟环境则通过以下指令实现：

```
conda env remove --name envname
```

![](img/f188c1ca6c4e2628da01f5beb4038aca.png)

+   需要注意的是无法卸载当前激活的环境，建议卸载时先切换到base环境中再执行操作。

+   Step 2. 查看当前驱动最高支持的CUDA版本

我们需要根据CUDA版本选择Pytorch框架，先看下当前的CUDA版本：

```
nvidia -smi
```

![](img/858d7e65902fa8116e92d41a6b83c8d3.png)

+   Step 3. 在虚拟环境中安装Pytorch

进入Pytorch官网：https://pytorch.org/get-started/previous-versions/

![](img/42eb844910d7b08f9169bb318a39e3ed.png)

当前的电脑CUDA的最高版本要求是12.2，所以需要找到不大于12.2版本的Pytorch。

![](img/4f69d210a96007f298a69291f4755c4b.png)

直接复制对应的命令，进入终端执行即可。这实际上安装的是为 CUDA 12.1 优化的 PyTorch 版本。这个 PyTorch 版本预编译并打包了与 CUDA 12.1 版本相对应的二进制文件和库。

![](img/4c1214c7cf9a3e492e72a4dcc3f351ce.png)

+   **Step 4. 安装必要的依赖**

Transfomers是大模型推理时所需要使用的框架，通过以下指令可以下载最新版本的Transfomers：

pip install transformers -U

![](img/a99f1ddb63b2ac9caed0a3199b31d2f5.png)

安装完成后可以通过以下命令检查是否安装：

```
pip show transformers
```

![](img/9ccf42cfdd7df89e2649afceaf2fa771.png)

接下来需要安装下载工具modelscope以及接下来要下载脚本的依赖accelerate，通过以下代码进行对应工具的部署：

pip install modelscope

![](img/153d8ba34283b425c625a9eefe3f7975.png)

pip install accelerate>=0.26.0

![](img/849fe62a126f1796edb295adbe4eddcf.png)

安装qwen_vl_utils库来加速模型推理：

pip install qwen-vl-utils

![](img/5fdef3638ea454bdb0e2fa027a7e6ebd.png)

+   **Step 5. 下载模型权重**

在下载模型权重之前，先为之创建合适的文件夹。

![](img/9a0bb8ebe0ac0e19579760acd3cf6680.png)

本流程选择在modelscope平台下载Qwen2VL模型，这种方式在国内安装部署大模型非常便捷。通过以下命令开始下载，需注意最后的一个参数要修改成你要存放模型的文件夹。

modelscope download --model Qwen/Qwen2-VL-7B-Instruct --local_dir ./dir

![](img/c7891dfc9c1ce737c12dc4aeac125716.png)

## 本地调用测试

#### 单图推理

+   导入相关库

```
from transformers import Qwen2VLForConditionalGeneration, AutoTokenizer, AutoProcessor
from qwen_vl_utils import process_vision_info
from modelscope import snapshot_download
```

+   设置模型路径

```
model_dir = &#x27;./model/dir&#x27;
```

+   实例化模型对象

```

model = Qwen2VLForConditionalGeneration.from_pretrained(
    model_dir, torch_dtype=&quot;auto&quot;, device_map=&quot;auto&quot;

processor = AutoProcessor.from_pretrained(model_dir)
)
```

+   创建消息

                &quot;image&quot;: &quot;https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-VL/assets/demo.jpeg&quot;,

```
messages = [
    {
        &quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: [
            {
                &quot;type&quot;: &quot;image&quot;,
            },
            {&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;Describe this image.&quot;},
        ],
    }
]
```

+   进行推理

```

generated_ids = model.generate(**inputs, max_new_tokens=128)
generated_ids_trimmed = [
    out_ids[len(in_ids) :] for in_ids, out_ids in zip(inputs.input_ids, generated_ids)
]
output_text = processor.batch_decode(
    generated_ids_trimmed, skip_special_tokens=True, clean_up_tokenization_spaces=False
)
print(output_text)
```

demo中进行测试的图片内容如下：

![](img/6b0e91c377c4c1ded2766a8a90410cdb.png)

执行完返回的结果会如下：

![](img/d2245789cdcd8ad0ab7ee4f7874aee8e.png)

进行推理Qwen2VL：7B识别官方给出的demo图像消耗将近34G显存。

![](img/6f53af3d3fdc8fb4105c3b6fea97b5be.png)

对于其他的信息源测试：无论是云平台挂载的图片还是本地图片识别，消耗的显存都不超过16G。以下图为例，这是一个展示模型性能的图表，将之用上述方法交付给Qwen2VL进行推理：

![](img/bf20b071671ac6c3052da846d5438ad0.png)

推理返回结果如下

![](img/84ef3ee47bcd85850f6241ad9eb41043.png)

显存消耗情况如下，大约消耗16G。

![](img/859a385d9a08e041b4433c80e1e8d1c6.png)

#### 多图推理

多图推理的代码如下，需注意修改本地启动模型路径和传入信息中的图像信息路径，其中，传入进行推理的图像支持本地.jpg / .png /.jpeg 格式的文件，也支持线上url格式的文件。

+   多图推理的代码如下

```
from transformers import Qwen2VLForConditionalGeneration, AutoTokenizer, AutoProcessor
from qwen_vl_utils import process_vision_info
from modelscope import snapshot_download
model_dir = &#x27;/home/LLM/qwen2VL&#x27;

model = Qwen2VLForConditionalGeneration.from_pretrained(
    model_dir, torch_dtype=&quot;auto&quot;, device_map=&quot;auto&quot;
)

processor = AutoProcessor.from_pretrained(model_dir)

# Messages containing multiple images and a text query
messages = [
    {
        &quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: [
            {&quot;type&quot;: &quot;image&quot;, &quot;image&quot;: &quot;/home/LLM/llama3_2_v/mao.png&quot;},
            {&quot;type&quot;: &quot;image&quot;, &quot;image&quot;: &quot;/home/LLM/llama3_2_v/fm.png&quot;},
            {&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;Identify the similarities between these images.&quot;},
        ],
    }
]

# Preparation for inference
text = processor.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True
)
image_inputs, video_inputs = process_vision_info(messages)
inputs = processor(
    text=[text],
    images=image_inputs,
    videos=video_inputs,
    padding=True,
    return_tensors=&quot;pt&quot;,
)
inputs = inputs.to(&quot;cuda&quot;)

# Inference
generated_ids = model.generate(**inputs, max_new_tokens=128)
generated_ids_trimmed = [
    out_ids[len(in_ids) :] for in_ids, out_ids in zip(inputs.input_ids, generated_ids)
]
output_text = processor.batch_decode(
    generated_ids_trimmed, skip_special_tokens=True, clean_up_tokenization_spaces=False
)
print(output_text)
```

![](img/cd57b7c4e3d1240bed2dfd9ac062e098.png)

![](img/6277c13421784af3e6faf743783ab864.png)

传入的图片一个是.jpg的，一个是.png的，返回内容如下：

![](img/659d1d55046bbec32a82a62067336c33.png)

Qwen2VL也支持中文问答.

![](img/5de6d11581407470c2c720904647edbc.png)

#### 视频推理

```
from transformers import Qwen2VLForConditionalGeneration, AutoTokenizer, AutoProcessor
from qwen_vl_utils import process_vision_info
from modelscope import snapshot_download
model_dir = &#x27;/home/LLM/qwen2VL&#x27;

model = Qwen2VLForConditionalGeneration.from_pretrained(
    model_dir, torch_dtype=&quot;auto&quot;, device_map=&quot;auto&quot;
)

processor = AutoProcessor.from_pretrained(model_dir)


messages = [
    {
        &quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: [
            {
                &quot;type&quot;: &quot;video&quot;,
                &quot;video&quot;: &quot;/home/LLM/llama3_2_v/可灵-小老虎.mp4&quot;,
                &quot;max_pixels&quot;: 360 * 420,
                &quot;fps&quot;: 1.0,
            },
            {&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;Describe this video.&quot;},
        ],
    }
]

# Preparation for inference
text = processor.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True
)
image_inputs, video_inputs = process_vision_info(messages)
inputs = processor(
    text=[text],
    images=image_inputs,
    videos=video_inputs,
    padding=True,
    return_tensors=&quot;pt&quot;,
)
inputs = inputs.to(&quot;cuda&quot;)

# Inference
generated_ids = model.generate(**inputs, max_new_tokens=128)
generated_ids_trimmed = [
    out_ids[len(in_ids) :] for in_ids, out_ids in zip(inputs.input_ids, generated_ids)
]
output_text = processor.batch_decode(
    generated_ids_trimmed, skip_special_tokens=True, clean_up_tokenization_spaces=False
)
print(output_text)
```

推理效果如下：

![](img/75e13ccf5079e1ef25a848eb3053a019.png)

返回中文推理结果：

![](img/7b8b128fd51f7bd6f56b8fedc13de296.png)

需要注意的是，视频仅支持输入本地路径的文件进行推理。

尽管 Qwen2-VL 在视觉任务中表现出色，但从第三方的视角来看，它仍然存在一些值得注意的局限性，这些限制对其在特定场景中的适用性提出了挑战。以下是一些主要的观察点：

+   音频支持缺失
Qwen2-VL 目前无法处理视频中的音频信息，因此在需要综合视觉与听觉的任务中，其表现可能受限。

+   数据覆盖的时效性
模型的图像训练数据更新至 2023 年 6 月，之后的事件或变化可能未被纳入，这限制了其在时效性要求较高场景中的应用能力。

+   个人与知识产权识别能力的局限
在识别特定人物或品牌方面，模型可能存在盲点，尤其是对于较少曝光的个人或冷门知识产权的识别，准确率仍需提升。

+   复杂指令的理解能力
当用户提出涉及多个步骤或需要复杂逻辑的指令时，Qwen2-VL 的理解与执行能力表现出一定不足，难以高效完成复杂任务。

+   计数能力不足
特别是在包含大量物体或复杂背景的场景中，模型对物体数量的识别往往不够精准，这可能对特定应用场景（如物流或物品清点）产生影响。

+   空间推理能力的短板
模型在推断 3D 空间中物体的相对位置时表现较弱，对于需要精确空间理解的任务（如建筑设计或机器人导航）仍有较大改进空间。

这些局限性表明，尽管 Qwen2-VL 展现出强大的视觉能力，但其性能在某些特定领域仍有优化空间。未来若能针对这些短板进行持续改进，模型的应用范围和实用性将进一步拓宽。

### 推理Qwen2vl：2b模型

                &quot;image&quot;: &quot;https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-VL/assets/demo.jpeg&quot;,

```

import torch
from transformers import Qwen2VLForConditionalGeneration, AutoProcessor
from qwen_vl_utils import process_vision_info

model_dir = &#x27;/home/LLM/qwen2vl-2b&#x27;

# 加载模型到 GPU0
model = Qwen2VLForConditionalGeneration.from_pretrained(
    model_dir, torch_dtype=&quot;auto&quot;
)
device = torch.device(&quot;cuda:0&quot;)
model = model.to(device)

# 加载处理器
processor = AutoProcessor.from_pretrained(model_dir)

messages = [
    {
        &quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: [
            {
                &quot;type&quot;: &quot;image&quot;,
            },
            {&quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;Describe this image.&quot;},
        ],
    }
]

# 准备文本和视觉输入
text = processor.apply_chat_template(
    messages, tokenize=False, add_generation_prompt=True
)
image_inputs, video_inputs = process_vision_info(messages)

inputs = processor(
    text=[text],
    images=image_inputs,
    videos=video_inputs,
    padding=True,
    return_tensors=&quot;pt&quot;,
)

# 确保 inputs 在 GPU0 上
inputs = inputs.to(device)

# 推理：生成输出
generated_ids = model.generate(**inputs, max_new_tokens=128)
generated_ids_trimmed = [
    out_ids[len(in_ids):] for in_ids, out_ids in zip(inputs.input_ids, generated_ids)
]
output_text = processor.batch_decode(
    generated_ids_trimmed, skip_special_tokens=True, clean_up_tokenization_spaces=False
)
print(output_text)
```

+   官方demo演示

![](img/b1f7cd6ded53acccf24f981a39466434.png)

![](img/b898bb0c89227208dd8d0201928c735f.png)

推理线上图片需要将近19G显存。

![](img/ac51f93e5986cbe94fc23dc995a8a031.png)

+   本地图片识别测试

![](img/404959e1aca3547c6a604c8928118b1b.png)

![](img/666afd6f50eb3d16bfbbde8286b3583c.png)

![](img/fa094c26b0ef2a05e2c5c7a0be3dfb23.png)

推理大概需要6G的显存消耗。

![](img/125782356c0eb335192bb06d75f8d499.png)

---

📍更**多大模型技术内容学习**

**扫码添加助理****英英**，回复“大模型”，了解更多大模型技术详情哦👇

![](img/42931e29b54fe851c9e77e65db98bb9a.png)

此外，**扫码回复“入群”**，即可加入**大模型技术社群****：海量硬核独家技术****干货内容****+无门槛****技术交流****！**