# Siliconflow2cow - 一站式AI大模型

> 来源：[https://my.feishu.cn/docx/BOZNdO74VoxAL3x76SzcXe4knOg](https://my.feishu.cn/docx/BOZNdO74VoxAL3x76SzcXe4knOg)

##     前   言

今天要讲的白嫖攻略，是基于Siliconflow（硅基流动）目前的推广策略，根据消息，该活动将在2024/09/23结束，想要实现Token自由的抓紧了。

它目前的活动规则是，每当你邀请一个人，邀请人与被邀请人同时会获得14元的Token（约2000万Qwen2-72B的Token数量），并且此费用适用于任意分类的任意模型。

当然，时间过了不要紧，因为它对于新注册用户还是有免费赠送的一些额度的。

这里建议大家无论怎么样，最好都先备着。

注册链接：https://cloud.siliconflow.cn/i/wdyoipMi

之前有给大家介绍过Siliconflow这个平台[硅基流动 SiliconCloud](https://tffyvtlai4.feishu.cn/wiki/NRYiwIw84iMXGOkGLvxc62kvnNj)一站式AI大模型，参考链接在这里 -

内含了不少大模型，且跟进实时很快，基本上新上的没多久就会接入。

是个非常好用的全量平台。

![](img/b0802944d60a7d9ed9ed69e7cf8ef182.png)

其次说下**Flux**，它是判定为开源AI绘画中目前最强的存在。

Flux模型是一种生成式图像模型，由初创公司Black Forest Labs开发（核心人员均来自SD），继承了Stable Diffusion的技术，并在生成写实类图片方面表现出色。

按照老金我的评价，Flux在绘画层面几乎可以媲美MJ，并且最重要的是SD的特性，可控！指哪打哪，非常好用！

目前开放了3个版本，分别是Schnell，Dev和Pro，Pro只能用付费API，也就是说，Dev是目前开源最强版本！

对于想调整细节的人来讲，真的是福音！

---

##   🛰️机器人使用方法

下面开始讲解在🛰️机器人中，如何使用硅基流动的FluxAPI。

其他模型均可使用，可以参照老金我之前写的教程，配置如🛰️机器人语言模型等等的地方，或者Dify等LLMops平台等。

首先，你需要满足2个条件：

1、安装了基于chatgpt-on-wechat或dify--on-wechat的机器人。

2、申请了硅基流动的API，API在官网如下图位置

![](img/ff90441de3aadbe8b570df4f2fc0ace5.png)

其次，需要安装对应插件Siliconflow2cow，以此私聊机器人以下代码：

```
#installp https://github.com/lemodragon/Siliconflow2cow.git
#scanp
```

**#注：**

**原****版本插件不支持FluxDev模****型**，老金我调整了一版，可以私聊👸号【Siliconflow2cow】获取最新代码文件，插件目录下替换源文件即可。

老金修改版**增加Dev模型**支持同时，新**增加Flux模型的强化逻辑**（硅基里目前是Dev和Schnell），毕竟Flux和SD的提示词使用逻辑是不同的，Flux更倾向于人类自然语言。

并将**翻译API**进行了替换成**目前硅基免费的文字模型**了。

![](img/a5903c22bd8674bc3546df0bd0e2ec2b.png)

然后在插件目录下，点击终端，执行以下代码，安装依赖：

```
pip install -r requirements.txt
```

然后在云服务器文件中进行配置修改，路径是/root/chatgpt-on-wechat/plugins/Siliconflow2cow中找到config.json文件。

![](img/7db2dc2ecd9ad630dc07ea899200615e.png)

参数解释：

+   auth_token: 您的API认证令牌

+   drawing_prefixes: 触发绘图的命令前缀

+   image_output_dir: 生成图片的保存路径

+   clean_interval: 自动清理（默认3天）前的旧图片

+   clean_check_interval: 默认每小时检测一次图片是否需要清理（单位为s）

当说画或者draw时候，自动触发插件。

![](img/601d54b9323aa9737b15361f04c9605d.png)

![](img/c82d9590cc1ac2db512476435180ef00.png)

![](img/a0e5fb38a4e73615500088a266a0b848.png)

![](img/66ee1626a888c091942f0c54002d8978.png)

##    插件功能说明

### **翻译模型选择**

默认情况下，插件使用 DeepSeek 付费模型。您可以切换到免费模型，如：

```
Qwen/Qwen2-7B-Instruct (32K, 免费)
Qwen/Qwen2-1.5B-Instruct (32K, 免费)
Qwen/Qwen1.5-7B-Chat (32K, 免费)
THUDM/glm-4-9b-chat (32K, 免费)
THUDM/chatglm3-6b (32K, 免费)
01-ai/Yi-1.5-9B-Chat-16K (16K, 免费)
01-ai/Yi-1.5-6B-Chat (4K, 免费)
internlm/internlm2_5-7b-chat (32K, 免费)
国际领先模型部分：
google/gemma-2-9b-it (8K, 免费)
meta-llama/Meta-Llama-3-8B-Instruct (8K, 免费)
meta-llama/Meta-Llama-3.1-8B-Instruct (8K, 免费)
mistralai/Mistral-7B-Instruct-v0.2 (32K，免费) 
```

### **优化建议**

为提高图像质量，特别是解决颜色过度饱和的问题，可以考虑调整以下参数：

1.  **推理步数** (num_inference_steps)：

+   标准模型：20-50 步

+   快速模型（如 SDXL Turbo）：4-10 步

1.  **引导尺度** (guidance_scale)：

+   标准范围：5.0-8.0

+   对于过度饱和的情况，尝试：3.0-5.0

1.  **提示词优化**：

+   使用具体、详细的描述

+   包含艺术风格（例如，"油画风格"）

+   使用括号增加权重："(蓝色眼睛:1.2)"

1.  **模型特定配置**： 根据您使用的模型（FLUX、SD、SDXL Turbo 等）调整参数

### **使用方法**

使用以下格式生成图像：

```
[前缀] [提示词] -m [模型] ---[宽高比]
```

示例：

```
绘小女孩,情趣内衣,18岁,蜡烛,昏暗 -m flux ---16:9
```

**输入格式错误时，会使用默认模型f**lux默认尺寸1024x1024请求接口

#### **支持的模型**

+   flux: FLUX.1-schnell

+   sd3: Stable Diffusion 3 Medium

+   sdxl: Stable Diffusion XL Base 1.0

+   sd2: Stable Diffusion 2.1

+   sdt: Stable Diffusion Turbo

+   sdxlt: Stable Diffusion XL Turbo

+   sdxll: SDXL-Lightning

#### **可用宽高比**

+   1:1 (1024x1024)

+   1:2 (1024x2048)

+   2:1 (2048x1024)

+   3:2 (1536x1024)

+   2:3 (1024x1536)

+   4:3 (1536x1152)

+   3:4 (1152x1536)

+   16:9 (2048x1152)

+   9:16 (1152x2048)

#### **图像到图像转换**

在提示词中包含图片 URL：

```
绘 将这张图片中的猫娘头上加上玫瑰 https://demo-cloudflare-imgbed.pages.dev/file/3a58a0d70ecf5439ec784.png -m sdxl ---9:16
```

![](img/74f7910e416af95a9ce9ef79968fd1ed.png)

+   图生图有点奇葩，勉勉强强使用吧（第一张为原图，第二张为图生图）

![](img/f580d63886b026a72ff5546320860dc1.png)

## **重要说明**

1.  确保您有足够的 API 使用额度。

1.  请确保您有足够的存储空间来保存生成的图片。

1.  插件会自动优化您的提示词以产生更好的结果。

1.  请遵守API提供商的使用条款和内容政策。(出现451ERROR为检测到违规提示词，sd2较易触发)

1.  定期清理功能会自动删除指定天数前的图片，请注意备份重要图片。

1.  使用 绘clean_all 命令时要小心，它会删除所有已生成的图片。

## **故障排除**

如果遇到问题：

1.  验证您的 API 令牌是否正确。

1.  确保您有稳定的网络连接。

1.  查看日志文件以获取详细的错误信息。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>