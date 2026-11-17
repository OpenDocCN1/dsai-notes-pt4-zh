
# Qwen2-VL 模型推理与局限性分析

## 推理代码示例

```python
from transformers import Qwen2VLForConditionalGeneration, AutoTokenizer, AutoProcessor
from qwen_vl_utils import process_vision_info
from modelscope import snapshot_download
model_dir = '/home/LLM/qwen2VL'

model = Qwen2VLForConditionalGeneration.from_pretrained(
    model_dir, torch_dtype="auto", device_map="auto"
)

processor = AutoProcessor.from_pretrained(model_dir)


messages = [
    {
        "role": "user",
        "content": [
            {
                "type": "video",
                "video": "/home/LLM/llama3_2_v/可灵-小老虎.mp4",
                "max_pixels": 360 * 420,
                "fps": 1.0,
            },
            {"type": "text", "text": "Describe this video."},
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
    return_tensors="pt",
)
inputs = inputs.to("cuda")

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

## Qwen2-VL 的主要局限性

尽管 `Qwen2-VL` 在视觉任务中表现出色，但从第三方的视角来看，它仍然存在一些值得注意的局限性，这些限制对其在特定场景中的适用性提出了挑战。以下是一些主要的观察点：

+ 音频支持缺失

`Qwen2-VL` 目前无法处理视频中的音频信息，因此在需要综合视觉与听觉的任务中，其表现可能受限。

+ 数据覆盖的时效性

模型的图像训练数据更新至 2023 年 6 月，之后的事件或变化可能未被纳入，这限制了其在时效性要求较高场景中的应用能力。

+ 个人与知识产权识别能力的局限

在识别特定人物或品牌方面，模型可能存在盲点，尤其是对于较少曝光的个人或冷门知识产权的识别，准确率仍需提升。

+ 复杂指令的理解能力

当用户提出涉及多个步骤或需要复杂逻辑的指令时，`Qwen2-VL` 的理解与执行能力表现出一定不足，难以高效完成复杂任务。

+ 计数能力不足

特别是在包含大量物体或复杂背景的场景中，模型对物体数量的识别往往不够精准，这可能对特定应用场景（如物流或物品清点）产生影响。

+ 空间推理能力的短板

模型在推断 3D 空间中物体的相对位置时表现较弱，对于需要精确空间理解的任务（如建筑设计或机器人导航）仍有较大改进空间。

这些局限性表明，尽管 `Qwen2-VL` 展现出强大的视觉能力，但其性能在某些特定领域仍有优化空间。未来若能针对这些短板进行持续改进，模型的应用范围和实用性将进一步拓宽。

## 推理 Qwen2-VL 2B 模型

```json
"image": "https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-VL/assets/demo.jpeg",
```

```python
import torch
from transformers import Qwen2VLForConditionalGeneration, AutoProcessor
from qwen_vl_utils import process_vision_info

model_dir = '/home/LLM/qwen2vl-2b'

# 加载模型到 GPU0
model = Qwen2VLForConditionalGeneration.from_pretrained(
    model_dir, torch_dtype="auto"
)
device = torch.device("cuda:0")
model = model.to(device)

# 加载处理器
processor = AutoProcessor.from_pretrained(model_dir)

messages = [
    {
        "role": "user",
        "content": [
            {
                "type": "image",
            },
            {"type": "text", "text": "Describe this image."},
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
    return_tensors="pt",
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

+ 官方demo演示

![](img/b1f7cd6ded53acccf24f981a39466434.png)

![](img/b898bb0c89227208dd8d0201928c735f.png)

推理线上图片需要将近19G显存。

![](img/ac51f93e5986cbe94fc23dc995a8a031.png)

+ 本地图片识别测试

![](img/404959e1aca3547c6a604c8928118b1b.png)

![](img/666afd6f50eb3d16bfbbde8286b3583c.png)

![](img/fa094c26b0ef2a05e2c5c7a0be3dfb23.png)

推理大概需要6G的显存消耗。

![](img/125782356c0eb335192bb06d75f8d499.png)

---

📍更多大模型技术内容学习

扫码添加助理英英，回复“大模型”，了解更多大模型技术详情哦👇

![](img/42931e29b54fe851c9e77e65db98bb9a.png)

此外，扫码回复“入群”，即可加入大模型技术社群：海量硬核独家技术干货内容+无门槛技术交流！
