# ComfyUI +Flux

> 来源：[https://xodnytdcaw.feishu.cn/docx/RiETdCSPXoB7qgxGFOLcr5BQnA5](https://xodnytdcaw.feishu.cn/docx/RiETdCSPXoB7qgxGFOLcr5BQnA5)

我是通过ComfyUI +Flux这种方式，网传推理dev版本的Flux模型时，最低显存需要16G，建议24G，模型本身大小是22多G，我配置4060显卡，8G显存，经测试，底模用的fp8的量化版模，出图时间：张/2分钟

[ComfyUI安装包](https://www.comfy.org/)

Flux.1 Dev模型文件

> 把这4个模型，放到对应的目录下就可以了。

+   t5xxl_fp16.safetensors/clip_l.safetensors放在 ComfyUI/models/clip/ 目录下

+   ae.safetensors：放在 ComfyUI/models/vae/ 目录下

+   flux1-dev.safetensors: 放在 ComfyUI/models/unet/ 目录下

>

> clip_l.safetensors和t5xxl_fp16.safetensors是关于文字语言编码，ae.safetensors是VAE，flux1-dev.safetensors是底模，这么理解就行。

CLIP模型

https://huggingface.co/comfyanonymous/flux_text_encoders/tree/main

t5xxl_fp8_e4m3fn.safetensors/clip_l.safetensors

VAE模型

https://huggingface.co/black-forest-labs/FLUX.1-dev/tree/main

Unet模型

https://huggingface.co/black-forest-labs/FLUX.1-dev/tree/main

https://huggingface.co/Kijai/flux-fp8/tree/main（量化版）

> flux相关模型与工作流

> 迅雷网盘链接：

> 链接：https://pan.xunlei.com/s/VOLKiSS0bOOmeiPqXp-wJGtXA1

> 提取码：mk53