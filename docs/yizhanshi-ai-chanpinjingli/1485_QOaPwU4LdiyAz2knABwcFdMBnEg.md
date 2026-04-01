# VisionEncoderDecoderModel

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Cn7md0kJ4obVYJxh4gDc6BSNnig](https://v11enp9ok1h.feishu.cn/docx/Cn7md0kJ4obVYJxh4gDc6BSNnig)

# 介绍

## image-encoder-decoder

![](img/a41b02ab630821a16d822726467b69cd.png)

看到这个模型

1.  感觉这个模型很有趣，想法很好。

1.  发现这个模型关于中文的不多。

1.  之前的clip训练其实挺失败的，loss没有下降.

主要也就是抱着学习的态度，把源码看懂，把流程跑通。分享中间的细节和踩坑经历。

## 源码介绍

能想出来还是非常厉害的(直呼胶水怪！！)，目前从源码上看，大概是这么一回事:

![](img/284f78a128a7287f5ee7819e07e2ba1b.png)

1.  使用vit来作为encoder部分，输出encoder_hidden_states，绿色部分1。

1.  使用gpt2来作为decoder部分,接受encoder_hidden_states,绿色部分3。

1.  如果encoder输出的encoder_hidden_states和decoder接受的encoder_hidden_states维度不一样，就加个linear,绿色部分2。

## 模型训练需要的数据样式

训练的时候，模型需要的数据主要有两个维度：

1.  pixel_value：image通过processor生成

1.  label：text通过tokenizer生成的input_ids。

1.  计算loss的时候，其实和gpt2一模一样的（自回归，本质上就是向后错位一下）。

# huggingface🤗

目前已经把我训练好的模型，发[布](https://huggingface.co/yuanzhoulvpi/vit-gpt2-image-chinese-captioning)在huggingface上了。https://huggingface.co/yuanzhoulvpi/vit-gpt2-image-chinese-captioning

# 训练

## 数据部分

本模块处理数据的方式和clip模型差不多，可以看隔壁文件夹，训练clip的数据处理思路。

### 注意⚠️

1.  只要把processdta_02.ipynb文件替换即可。

1.  执行顺序依然按照着processdta_01.ipynb、processdta_02.ipynb、processdta_03.ipynb。

## 训练部分train_encoder_decoder.ipynb

1.  处理图像，使用的是"google/vit-base-patch16-224"模型。

1.  处理文本，使用的是"yuanzhoulvpi/gpt2_chinese"模型。

1.  最后就是把两个模型通过VisionEncoderDecoderModel粘起来。

## 训练的loss

![](img/d9ac9c9a4529ed1fb790ed9b564109cc.png)

## 训练的信息

gpu使用的是3090，模型大概是2.16亿个参数。花了超过20个小时。但是大部分时间都是卡在IO上（加载图片上）

![](img/d6f87b4e6573c343153122b902a073d0.png)

# 推理

## 用你自己训练

参考infer_encoder_decoder.ipynb

## 直接用

```
from transformers import (VisionEncoderDecoderModel, 
                          AutoTokenizer,ViTImageProcessor)
import torch
from PIL import Image
```

```
vision_encoder_decoder_model_name_or_path = &quot;yuanzhoulvpi/vit-gpt2-image-chinese-captioning&quot;#&quot;vit-gpt2-image-chinese-captioning/checkpoint-3200&quot;

processor = ViTImageProcessor.from_pretrained(vision_encoder_decoder_model_name_or_path)
tokenizer = AutoTokenizer.from_pretrained(vision_encoder_decoder_model_name_or_path)
model = VisionEncoderDecoderModel.from_pretrained(vision_encoder_decoder_model_name_or_path)
device = torch.device(&quot;cuda&quot; if torch.cuda.is_available() else &quot;cpu&quot;)
model.to(device)
```

```
max_length = 16
num_beams = 4
gen_kwargs = {&quot;max_length&quot;: max_length, &quot;num_beams&quot;: num_beams}


def predict_step(image_paths):
    images = []
    for image_path in image_paths:
        i_image = Image.open(image_path)
        if i_image.mode != &quot;RGB&quot;:
            i_image = i_image.convert(mode=&quot;RGB&quot;)

        images.append(i_image)

    pixel_values = processor(images=images, return_tensors=&quot;pt&quot;).pixel_values
    pixel_values = pixel_values.to(device)

    output_ids = model.generate(pixel_values, **gen_kwargs)

    preds = tokenizer.batch_decode(output_ids, skip_special_tokens=True)
    preds = [pred.strip() for pred in preds]
    return preds


predict_step([&#x27;bigdata/image_data/train-1000200.jpg&#x27;])
```