# Fooocus（SDXL硬件友好）

> 来源：[https://seigy6zzam.feishu.cn/docx/DNfsdme7MoaWRQxtVUdcuN5TnUc](https://seigy6zzam.feishu.cn/docx/DNfsdme7MoaWRQxtVUdcuN5TnUc)

![](img/48cfa806346e3bc45527436f1165e0d0.png)

![](img/cd42874a8e72c5f9f7c5cf0ff427e9da.png)

# 说明

## 后续方向

+   ContolNet会有吗？：https://github.com/lllyasviel/Fooocus/issues/46 根据下面图生图看算是反悔了一半😂

![](img/888ca188cdc1b853fc1d7ad6057c8965.png)

+   [**Fooocus-MRE**](https://github.com/MoonRide303/Fooocus-MRE)**：**[在](https://github.com/lllyasviel/Fooocus/discussions/689)Fooocus进化成完全体之前的最强增强，但是鉴于这个讨论 Porting any of Fooocus-MRE features into original code base? Fooocus-MRE跟Fooocus走向合并？

![](img/0de35b1a899fa7e7324030fad82c7192.png)

+   特殊特性：思路上更多是对齐了Stable Diffusion的高级配置，所以感觉理念上跟原作者就是有一定分歧的

+   最突出的当属对于[ControlNet的支持](https://huggingface.co/stabilityai/control-lora)（其实Fooocus在2.1的版本中Image Prompt也变相支持了controlnet）

+   图片生成的参数支持保存在生成的图片中

+   优化了很多小细节的体验，比如分辨率是x:x的形式，图片提示词风格可以搜索添加，给所有图片固定种子等等

## 风格参考

+   [风格整理](https://seigy6zzam.feishu.cn/wiki/M3M8wAlkziPeHNkcxgYc22lCnxe)荐本文档参考整理最新风格👉

+   配套：

+   南瓜图：https://github.com/lllyasviel/Fooocus/discussions/143

+   多样图：https://docs.google.com/spreadsheets/d/1AF5bd-fALxlu0lguZQiQVn1yZwxUiBJGyh2eyJJWl74

+   其他：

+   兔子图：https://clio.so/rabbitsxl

+   https://www.reddit.com/r/StableDiffusion/comments/15afvnb/sdxl_various_styles_keywords/

```
&quot;Style: Enhance&quot;,&quot;breathtaking {prompt} . award-winning, professional, highly detailed&quot;,&quot;ugly, deformed, noisy, blurry, distorted, grainy&quot;
&quot;Style: Anime&quot;,&quot;anime artwork {prompt} . anime style, key visual, vibrant, studio anime, highly detailed&quot;,&quot;photo, deformed, black and white, realism, disfigured, low contrast&quot;
&quot;Style: Photographic&quot;,&quot;cinematic photo {prompt} . 35mm photograph, film, bokeh, professional, 4k, highly detailed&quot;,&quot;drawing, painting, crayon, sketch, graphite, impressionist, noisy, blurry, soft, deformed, ugly&quot;
&quot;Style: Digital art&quot;,&quot;concept art {prompt} . digital artwork, illustrative, painterly, matte painting, highly detailed&quot;,&quot;photo, photorealistic, realism, ugly&quot;
&quot;Style: Comic book&quot;,&quot;comic {prompt} . graphic illustration, comic art, graphic novel art, vibrant, highly detailed&quot;,&quot;photograph, deformed, glitch, noisy, realistic, stock photo&quot;
&quot;Style: Fantasy art&quot;,&quot;ethereal fantasy concept art of {prompt} . magnificent, celestial, ethereal, painterly, epic, majestic, magical, fantasy art, cover art, dreamy&quot;,&quot;photographic, realistic, realism, 35mm film, dslr, cropped, frame, text, deformed, glitch, noise, noisy, off-center, deformed, cross-eyed, closed eyes, bad anatomy, ugly, disfigured, sloppy, duplicate, mutated, black and white&quot;
&quot;Style: Analog film&quot;,&quot;analog film photo {prompt} . faded film, desaturated, 35mm photo, grainy, vignette, vintage, Kodachrome, Lomography, stained, highly detailed, found footage&quot;,&quot;painting, drawing, illustration, glitch, deformed, mutated, cross-eyed, ugly, disfigured&quot;
&quot;Style: Neonpunk&quot;,&quot;neonpunk style {prompt} . cyberpunk, vaporwave, neon, vibes, vibrant, stunningly beautiful, crisp, detailed, sleek, ultramodern, magenta highlights, dark purple shadows, high contrast, cinematic, ultra detailed, intricate, professional&quot;,&quot;painting, drawing, illustration, glitch, deformed, mutated, cross-eyed, ugly, disfigured&quot;
&quot;Style: Isometric&quot;,&quot;isometric style {prompt} . vibrant, beautiful, crisp, detailed, ultra detailed, intricate&quot;,&quot;deformed, mutated, ugly, disfigured, blur, blurry, noise, noisy, realistic, photographic&quot;
&quot;Style: Lowpoly&quot;,&quot;low-poly style {prompt} . low-poly game art, polygon mesh, jagged, blocky, wireframe edges, centered composition&quot;,&quot;noisy, sloppy, messy, grainy, highly detailed, ultra textured, photo&quot;
&quot;Style: Origami&quot;,&quot;origami style {prompt} . paper art, pleated paper, folded, origami art, pleats, cut and fold, centered composition&quot;,&quot;noisy, sloppy, messy, grainy, highly detailed, ultra textured, photo&quot;
&quot;Style: Line art&quot;,&quot;line art drawing {prompt} . professional, sleek, modern, minimalist, graphic, line art, vector graphics&quot;,&quot;anime, photorealistic, 35mm film, deformed, glitch, blurry, noisy, off-center, deformed, cross-eyed, closed eyes, bad anatomy, ugly, disfigured, mutated, realism, realistic, impressionism, expressionism, oil, acrylic&quot;
&quot;Style: Craft clay&quot;,&quot;play-doh style {prompt} . sculpture, clay art, centered composition, Claymation&quot;,&quot;sloppy, messy, grainy, highly detailed, ultra textured, photo&quot;
&quot;Style: Cinematic&quot;,&quot;cinematic film still {prompt} . shallow depth of field, vignette, highly detailed, high budget Hollywood movie, bokeh, cinemascope, moody, epic, gorgeous, film grain, grainy&quot;,&quot;anime, cartoon, graphic, text, painting, crayon, graphite, abstract, glitch, deformed, mutated, ugly, disfigured&quot;
&quot;Style: 3d-model&quot;,&quot;professional 3d model {prompt} . octane render, highly detailed, volumetric, dramatic lighting&quot;,&quot;ugly, deformed, noisy, low poly, blurry, painting&quot;
&quot;Style: pixel art&quot;,&quot;pixel-art {prompt} . low-res, blocky, pixel art style, 8-bit graphics&quot;,&quot;sloppy, messy, blurry, noisy, highly detailed, ultra textured, photo, realistic&quot;
&quot;Style: Texture&quot;,&quot;texture {prompt} top down close-up&quot;,&quot;ugly, deformed, noisy, blurry&quot;
```

# 基本信息

+   项目地址：https://github.com/lllyasviel/Fooocus

+   详细更新日志：https://github.com/lllyasviel/Fooocus/blob/main/update_log.md

+   Window懒人包：作者的包只保留最新的，所以看最新到这里 https://github.com/lllyasviel/Fooocus/releases

+   1.0下载（基础文生图）：https://github.com/lllyasviel/Fooocus/releases/download/1.0.35/Fooocus_win64_1-1-1035.7z

+   2.0下载（开始支持图片变化和扩图，默认V2风格效果和提示词理解能力相对于V1提升80%左右）：https://github.com/lllyasviel/Fooocus/releases/download/release/Fooocus_win64_2-0-50.7z

+   🔥2.10下载（阶段性完全体）：https://github.com/lllyasviel/Fooocus/releases/download/release/Fooocus_win64_2-1-25.7z

+   模型下载：

+   大模型：

+   https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0_0.9vae.safetensors

+   https://huggingface.co/stabilityai/stable-diffusion-xl-refiner-1.0/resolve/main/sd_xl_refiner_1.0_0.9vae.safetensors

+   Lora模型：

+   测试：https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_offset_example-lora_1.0.safetensors

+   更多请访问C站

+   Fooocus为了对齐Midjourney功能训练了很多小模型，所以这些小模型都可以在这里找到：https://huggingface.co/lllyasviel/misc/tree/main，如果网络不好可以单独下载下来，但是要放到models对应的文件夹下面 Fooocus2\Fooocus\models\

# 汉化方法

> 将下面的文件下载下来，放到你的项目 Fooocus 代码项目文件夹的 language 子目录下，比如 Fooocus2\Fooocus\language其中的Fooocus2是我放启动脚本 run.bat 和 Fooocus 代码放库文件夹的地方，language 文件夹就在代码库文件夹下面

![](img/037b1ff9a9f2304071874a673002e75f.png)

![](img/a3b8dfda0705714ed596dce85dea9350.png)

![](img/a03e85b3d141f8a427976023319aaa3c.png)

[cn.json](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PWmjbf90ioSs1vxqnx4ckiV0n4c)

> 编辑启动脚本，比如通用的 run.bat 文件，增加 --language cn 的启动参数，如下图所示

![](img/70c2eac2e341788dc39c2d00071d40af.png)

# 文生图

+   Fooocus Advanced

> 如果网络不好可以自行通过迅雷下载下面的模型放到相应的文件夹下面

+   Base模型（6.46G）：https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_base_1.0_0.9vae.safetensors 下载到 Fooocus2\Fooocus\models\checkpoints

+   Refiner模型（5.66G）：https://huggingface.co/stabilityai/stable-diffusion-xl-refiner-1.0/resolve/main/sd_xl_refiner_1.0_0.9vae.safetensors 下载到 Fooocus2\Fooocus\models\checkpoints

+   Lora模型（47.3M）：https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/resolve/main/sd_xl_offset_example-lora_1.0.safetensors 下载到 Fooocus2\Fooocus\models\loras

+   VAE APPROX模型（209k）：https://huggingface.co/lllyasviel/misc/resolve/main/xlvaeapp.pth 下载到 Fooocus2\Fooocus\models\vae_approx

## 提示词扩写

> “Fooocus V2”风格可以看作是一种智能提示扩展。您可能会在控制台中看到一些由扩展生成的文本。这些文本以独特的方式处理，与您的书面提示不同。在大多数情况下，您无需担心这些生成的关键字会影响您的主要提示，你只要关注你的提示词是否包含关键信息

> 如果网络不好可以自行通过迅雷下载下面的expansion模型(335M左右) 放到相应的文件夹下面

+   https://huggingface.co/lllyasviel/misc/resolve/main/fooocus_expansion.bin 下载到 Fooocus2\Fooocus\models\prompt_expansion\fooocus_expansion

![](img/544e7e07dabf3fdc2e429a289c1f917b.png)

![](img/edfb6a0133632d27616513a8fcd8f267.png)

## 图片锐化

![](img/3ed8c3284da4a386999f841f880a3744.png)

|  | Sharpness = 2 | Sharpness = 10 | Sharpness = 20 |
| --- | --- | --- | --- |
| 对比1 | ![](img/714c17adea0a332e10b2df8ff78a2ee9.png) | ![](img/2ba5ac07021c674f3b819524df9fdade.png) | ![](img/1ae8e520b1717fad053af2a95a0dc5d5.png) |
| 对比2 | ![](img/2a75098702df77b591ee565d0b89e0e4.png) | ![](img/cbb5c5e1b5fdffcd4ac79c9a038d0ab9.png) | ![](img/acdcf36d0cc59585f3309b14a272768d.png) |

## 提示词强度

> 来自Stable Diffusion社区的 CFG scale。不同之处在于，Fooocus实施了自适应TSNR重新标度，以便在scale 较高（如大于15）时，结果不会被完全毁掉。

![](img/b06bce0a82dfa69de48a088a80d00518.png)

## 风格选项

> Fooocus V2 Style允许用户同时组合多种样式。样式的组合算法经过精心准备，因此添加多种样式不会降低画质。用户通常可以通过组合多种样式来获得非常高质量的图像。

![](img/db4bff58be5e7fe4271970182a925b59.png)

## 效果可比MidJourney

|  | MidJourney | Fooocus |
| --- | --- | --- |
| 对比效果 | ![](img/af26544f91edb19fa181899a89bf506d.png) | ![](img/ca21fefee2462bc56a647ec65fd2ca41.png) |

# 图生图

+   [[](https://github.com/lllyasviel/Fooocus/discussions/390)Fooocus 2.0.50] Variation/Upscale (Midjourney Toolbar)

+   [[Fooocus 2.0.60] Fooocus Inpaint or Out](https://github.com/lllyasviel/Fooocus/discussions/414)paint (Midjourney Left/Right/Top/Bottom)

+   [[](https://github.com/lllyasviel/Fooocus/discussions/557)Fooocus 2.1.0] Image Prompts (Midjourney Image Prompts)

## 图片变化和高清化

> 如果网络不好可以自行通过迅雷下载下面的upscale模型(32M左右) 放到相应的文件夹下面

+   https://huggingface.co/lllyasviel/misc/resolve/main/fooocus_upscaler_s409985e5.bin 下载到 Fooocus2\Fooocus\models\upscale_models

![](img/580fd3f23e2677a24561de3de62ec0a5.png)

|  | MidJourney | Fooocus |
| --- | --- | --- |
| 对比操作 | ![](img/af020bd7b4ee9991aa0f3e6832ffecd3.png) | ![](img/ce94115a540f400eb0bcb44aa6461692.png) |

### 图片变化

|  | 微小变化 | 强烈变化 |
| --- | --- | --- |
| 操作 | ![](img/ac1befef8dd49378225509720ba48dc4.png) | ![](img/1722f6c3dab5172a78f85c143e29c69a.png) |

### 高清化

|  | 1倍高清 | 2倍高清 |
| --- | --- | --- |
| 操作 | ![](img/dc5ccde5c5f43a602fbb48cf2278b4ee.png) | ![](img/c09bc621b451bdba8f7944667390cbf9.png) |
| 效果 | ![](img/56dfa4133383112fb47f97d2cbd74efb.png) | ![](img/549c8f59051af8972beddaa56c5b05db.png) |

快速2倍高清化（Fast 2X）

![](img/68e8d7abcdecf5d0f867029cb74f124a.png)

|  | 例1 | 例2 |
| --- | --- | --- |
| 效果 | ![](img/4e022ec601d881f3ae74bcbb156f47c5.png) | ![](img/30367af5925208376ded85a5bf6815cf.png) |
| 看细节 | ![](img/4c0111831da44fb25d4ec3380d4e3696.png) | ![](img/f1534cc1b4f6b355abe1aa7c85d07588.png) |

## 局部重绘和扩图

> 如果网络不好可以自行通过迅雷下载下面的head模型(52k左右)和inpaint path(1.22G左右) 放到相应的文件夹下面

+   https://huggingface.co/lllyasviel/fooocus_inpaint/resolve/main/fooocus_inpaint_head.pth 下载到 Fooocus2\Fooocus\models\inpaint

+   https://huggingface.co/lllyasviel/fooocus_inpaint/resolve/main/inpaint.fooocus.patch 下载到Fooocus2\Fooocus\models\inpaint

![](img/fc3c5d06967f2991fe09a7704c30d6b7.png)

|  | MidJourney | Fooocus |
| --- | --- | --- |
| 对比操作 | ![](img/827c4ed81c7032bf2d53bc1d7188ee8c.png) | ![](img/ddf0449c8e10d7c4757573830700c077.png) |

### 基于生成图片调整

在上面生成的图片拖入下面

![](img/a2b0ffc4e467eed6328c0287eee08cec.png)

### 局部重绘

![](img/89019f5faa55dd814722660fd8629b84.png)

![](img/12297650f3aaf5e09191918f50118b89.png)

### 扩充图像

![](img/55bf0c9983790e65d4c2f3b9de381bfa.png)

![](img/43d63dc18fd83988befc4a2adcf17b43.png)

### 高级：局部人物修复

+   单独修复女生的眼睛

![](img/cbecb9bb2b85597692c386ecc67b33ac.png)

![](img/9a4bf53ab754ea9439bb6dcb5fb0809a.png)

![](img/f1bb505d7287336b1cc56d1bd8947e47.png)

![](img/1abe3812f5f57af99b18d2424740e033.png)

+   前后修复对比

| 修复之前 | 修复之后 |
| --- | --- |
| ![](img/4185b64d3c47124b923cf8d68e9bb018.png) | ![](img/1b2f3e3309af3e5a3e8c0b4fe9d12f63.png) |
| ![](img/c2f109fb4fc9f2e5852cb9ede37dc240.png) | ![](img/cd9923b6fed2ae27799daf0b859fa25d.png) |

### 高级：增加物体和背景

![](img/3a43ad1764ddffa0b5cd462980db2a00.png)

| 局部添加物体 | 改变背景 |
| --- | --- |
| ![](img/ba7c15041e2ca76566b1197ce677bee7.png) | ![](img/f7ad69bc4e37f3264b422636ea817063.png) |

## 风格迁移和图片融合

> 如果网络不好可以自行通过迅雷下载下面的clip [vision模型(1](https://github.com/tencent-ailab/IP-Adapter).84G左右)，ip negative(65k左右)和ip adapter(966M左右) 放到相应的文件夹下面

+   https://huggingface.co/lllyasviel/misc/resolve/main/clip_vision_vit_h.safetensors 下载到 Fooocus2\Fooocus\models\clip_vision

+   [https://huggingface.co/lllyasviel/misc/reso](https://huggingface.co/lllyasviel/misc/resolve/main/fooocus_ip_negative.safetensors)lve/main/fooocus_ip_negative.safetensors 下载到Fooocus2\Fooocus\models\controlnet

+   "https://huggingface.co/lllyasviel/misc/resolve/main/ip-adapter-plus_sdxl_vit-h.bin 下载到Fooocus2\Fooocus\models\controlnet

![](img/8488372479e15ffa08e241e5dec32474.png)

|  | MidJourney | Fooocus |
| --- | --- | --- |
| 对比操作 | ![](img/aff64b604afeb22e62fb32459c34a1ea.png) | ![](img/c92ccb976281c2f84fdffa400d2ffac3.png) |

### 风格迁移

|  | **单图**默认Fooocus V2风格**无**提示词 | **单图**默认Fooocus V2风格**有**提示词 |
| --- | --- | --- |
| 对比操作 | ![](img/ffb7d79a51715038788f73a9b8ec76c1.png) | ![](img/40511686efd3dddbb2a2833cd9738e7f.png) |

### 图片融合

|  | **多图**默认Fooocus V2风格**无**提示词（直接融图） | **多图**默认Fooocus V2风格**有**提示词（强化主体） |
| --- | --- | --- |
| 对比操作 | ![](img/0f1ab13239174043ac063628c3a4effe.png) | ![](img/065a4301bf64701e3659c5a310b43198.png) |

### 综合应用

同时使用上面的特性，当前效果最均衡的渲染

+   多张图片融图

+   提示词强调主体

+   增加多风格迁移

![](img/e2ca2a1a7700895195260400a2d2fe86.png)

## 图片结构参考

> 如果网络不好可以自行通过迅雷下载下面的controlnet模型(400M左右) 放到相应的文件夹下面

+   [ht](https://huggingface.co/lllyasviel/misc/resolve/main/control-lora-canny-rank128.safetensors)tps://huggingface.co/lllyasviel/misc/resolve/main/control-lora-canny-rank128.safetensors 下载到 Fooocus2\Fooocus\models\controlnet

+   [https://huggingface.co/lllyasviel/misc/reso](https://huggingface.co/lllyasviel/misc/resolve/main/fooocus_xl_cpds_128.safetensors)lve/main/fooocus_xl_cpds_128.safetensors 下载到 Fooocus2\Fooocus\models\controlnet

![](img/8ba44a13abaed42f3b601284b0d394b5.png)

### Canny边缘

> 基于金字塔的Canny边缘控制。原因是SDXL使用1024px图像，标准Canny在如此高的分辨率下，往往会时不时地遗漏一些图像细节。这种方法使用多种分辨率来检测canny边缘，然后将它们软组合，从而捕获更多的结构（比canny）。金字塔部分来自“Edge Drawing：一种结合的实时边缘和段检测器”。使用时您将下载350MB的控件模型。

![](img/8af760c5a8dd4916463f06473d47e0d1.png)

![](img/6271b7ea31055d4e4b66bafbe3cfac47.png)

![](img/106249a30c67dab2084c46bf8ca950a1.png)

### **CPDS结构提取**

> 来自“[对比度保留脱色（CPD）](http://www.cse.cuhk.edu.hk/leojia/projects/color2gray/index.html)”的结构提取算法。“CPDS”意味着CPD结构。控制模型由Fooocus团队修改——它从SAI的深度控制-lora开始。使用这种方法的原因是速度快，免下载预处理器。请注意，我们只使用图像的结构部分，它并不是真正的“脱色”。使用它时，您将下载350MB的控制模型。

|  | **保持**提示词姿势不变 | **改变**提示词姿势不变 |
| --- | --- | --- |
| 对比效果 | ![](img/43dfadf3498967d0ad1de5055adf5c50.png) | ![](img/e5f4a4e7cfe6ec0405ef9bd217f0e683.png) |

# 图片描述

+   [F](https://github.com/lllyasviel/Fooocus/discussions/1363)ooocus 2.1.831: Fooocus Describe (in line with Midjourney Describe)

> 如果网络不好可以自行通过迅雷下载下面的controlnet模型(400M左右) 放到相应的文件夹下面

+   [https://huggingface.co/lllyasviel/misc/](https://huggingface.co/11lyasviel/misc/resolve/main/wd-v1-4-moat-tagger-v2.onnx)resolve/main/wd-v1-4-moat-tagger-v2.onnx 下载到 Fooocus2\Fooocus\models\clip_vision

+   [https://huggingface.co/lllyasviel/misc](https://huggingface.co/11lyasviel/misc/resolve/main/wd-v1-4-moat-tagger-v2.csv)/resolve/main/wd-v1-4-moat-tagger-v2.csv 下载到 Fooocus2\Fooocus\models\clip_vision

+   [ht](https://huggingface.co/lllyasviel/misc/resolve/main/fooocus_xl_cpds_128.safetensors)tps://huggingface.co/lllyasviel/misc/resolve/main/model_base_caption_capfilt_large.pth 下载到 Fooocus2\Fooocus\models\clip_vision

![](img/b40a0a8afb1b9ce3a5a708f983dd2a19.png)

![](img/f19f1bb2baa86545742bdfa2a3a89424.png)

+   对标功能

|  | MidJourney | Fooocus |
| --- | --- | --- |
| 对比操作 | ![](img/940b70d842cc3663d0316812bd4695d7.png) | ![](img/eea2c8207030feb288876bfa4a1cad25.png) |

+   视频演示

![](img/6192f82a75a140121899ee6f2401b0db.png)

[290128299-f207188f-8469-4c98-93a4-37f8d33689a5.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZSBxbwe3NoJHj6xjVtTcR1icnTf)

# Preset配置

+   [F](https://github.com/lllyasviel/Fooocus/discussions/679)ooocus 2.1.60: Official Anime and Realistic Support

作者特别增加了动漫和真实风单独的启动脚本，调整启动参数，自动下载调优验证后的预设模型们

![](img/7ba4ade36496880d75bda879ec858372.png)

> 如果网络不好可以自行通过迅雷下载在这里下载新的大模型 https://huggingface.co/lllyasviel/fav_models/tree/main/fav，下载后放到相应的文件夹下面

+   动漫风大模型：

+   https://huggingface.co/lllyasviel/fav_models/resolve/main/fav/bluePencilXL_v050.safetensors 下载到 Fooocus2\Fooocus\models\checkpoints

+   https://huggingface.co/lllyasviel/fav_models/resolve/main/fav/DreamShaper_8_pruned.safetensors 下载到 Fooocus2\Fooocus\models\checkpoints

+   动漫风embedding：https://huggingface.co/lllyasviel/fav_models/resolve/main/fav/unaestheticXLv31.safetensors 下载到 Fooocus2\Fooocus\models\embeddings

+   真实风大模型：https://huggingface.co/lllyasviel/fav_models/resolve/main/fav/realisticStockPhoto_v10.safetensors 下载到Fooocus2\Fooocus\models\checkpoints

+   真实风lora：https://huggingface.co/lllyasviel/fav_models/resolve/main/fav/SDXL_FILM_PHOTOGRAPHY_STYLE_BetaV0.4.safetensors 下载到Fooocus2\Fooocus\models\loras

## 动漫风预设

> https://github.com/lllyasviel/Fooocus/blob/main/presets/anime.json

> 使用SD1.5（DreamShaper_8）来细化SDXL（BluePencilXL），注意“sd1.5-as-xl-细化器算法”不同于所有其他软件——它只适用于Foocus，对应C站的地址：

> base XL：[https://civitai.com/models/](https://civitai.com/models/119012/bluepencil-xl)119012/bluep[encil-xl

> refiner 1.5：ht](https://civitai.com/models/4384/dreamshaper)tps://civit[a](https://civitai.com/models/119032/unaestheticxl-or-negative-ti)i.com/models/4384/dreamshaper

> embeddings：https://civitai.com/models/119032/unaestheticxl-or-negative-ti

![](img/0b4ecc785510975f0d75d8898c357843.png)

![](img/8eac049ade54f3c5d6a61ddade19a47f.png)

![](img/b9905b16b67ec522583a7c9de5ae6d0b.png)

![](img/f374a2f3aa69c05146937e84cb13fdf6.png)

![](img/69e6d56fa2587737ac9536cababa7868.png)

![](img/a3e92c436e850f1552e0a80ee19d09b3.png)

## 真实风预设

> https://github.com/lllyasviel/Fooocus/blob/main/presets/realistic.json

> 使用Foocus锐化度（sharpness）来增强SDXL（realessStockPhoto），对应C站的地址：

> base XL：[h](https://civitai.com/models/139565/realistic-stock-photo)ttps:[/](https://civitai.com/models/158945/sdxl-film-photography-style)/civitai.com/models/139565/realistic-stock-photo

> LORA：https://civitai.com/models/158945/sdxl-film-photography-style

![](img/7a34128d25099639623832a4d27d62e5.png)

![](img/e8870b4079fbb50ead1961796107fb94.png)

![](img/aff6bd695523a36d77925009bf3665de.png)

![](img/59e58ea3e6bd8eff23b9ea84357e0dd6.png)

## 自定义预设

> 参考 https://github.com/lllyasviel/Fooocus/tree/main/presets 中其他预设的配置，然后在启动脚本 run.bat 里面调整启动参数，比如下面这个（其实动漫风和真实风启动脚本 run-xxx.bat 也是使用 preset的参数）

> python entry_with_update.py --preset name_of_your_preset，如果你不清楚如何使用建议不要自己尝试。

# 功能对照参考

## MidJourney

|  | Midjourney | Fooocus |
| --- | --- | --- |
| 特点综述 | 高质量的文本到图像，无需太多的提示工程或参数调整。  （未知方法） | 高质量的文本到图像，无需太多的提示工程或参数调整。 |
| 图片变化 | V1 V2 V3 V4 | 输入图片 -&gt; Upscale or Variation -&gt; Vary (Subtle) / Vary (Strong) |
| 高清化 | U1 U2 U3 U4 | 输入图片 -&gt; Upscale or Variation -&gt; Upscale (1.5x) / Upscale (2x) |
| 局部重绘和扩充图像 | Inpaint / Up / Down / Left / Right (Pan) | 输入图片 -&gt; Inpaint or Outpaint -&gt; Inpaint / Up / Down / Left / Right |
| 图生图 | Image Prompt | 输入图片 -&gt; Image Prompt |
| 风格设置 | --style | Advanced -&gt; Style |
| 提示词强度 | --stylize | Advanced -&gt; Advanced -&gt; Guidance |
| 动漫风 | --niji | Fooocus supports SDXL models on Civitai  (如果您不知道，可以谷歌搜索“Civitai”) |
| 质量优先 | --quality | Advanced -&gt; Setting -&gt; Performance -&gt; Quality |
| 单次生图数量 | --repeat | Advanced -&gt; Image Number |
| 多条提示词 | Multi Prompts (::) | Just use multiple lines of prompts |
| 提示词权重 | Prompt Weights | You can use &quot; I am (happy:1.5)&quot;. |
| 负向提示词 | --no | Advanced -&gt; Negative Prompt |
| 分辨率 | --ar --aspect | Advanced -&gt; Aspect Ratios |
| 生成图片提示词 | /describe | Input Image -&gt; Describe |
| 混乱程度 | --chaos | ？？？ |
| 其他命令 | /ask  对话问答  /info.查看账号  /docs 查文档 | ？？？ |

## LeonardoAI

|  | Midjourney | Fooocus |
| --- | --- | --- |
| 提示词扩写 | Prompt Magic | Advanced -&gt; Style -&gt; Fooocus V2 |
| 高级参数 | Advanced Sampler Parameters (like Contrast/Sharpness/etc) | Advanced -&gt; Advanced -&gt; Sampling Sharpness / etc |
| 图片控制方法 | User-friendly ControlNets | Input Image -&gt; Image Prompt -&gt; Advanced |