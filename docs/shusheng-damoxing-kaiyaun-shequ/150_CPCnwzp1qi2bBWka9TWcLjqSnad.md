# L2G1-InternVL3部署微调测评实践

> 来源：[https://aicarrier.feishu.cn/docx/F7euddTdaogJuzxwwSvcIhjpn2g](https://aicarrier.feishu.cn/docx/F7euddTdaogJuzxwwSvcIhjpn2g)

**在基础课程L1G3、**L1G4和L1G2里，大家已**经上手体验过了L**MDeploy部署InternLM3系列模型、Xtuner微调InternLM3系列模型和opencompass、VLMEvalKit的测评。

今天把部署、微调和测评的目标从大语言模型替换为视觉大语言模型(Visual Language Models)的InternVL3系列模型。这个模型的特点是，由图片跟文本一起作为输入，然后大模型推理之后输出文本。

# InternVL3-2B的部署

部署InternVL3系列模型，可以用LMDeploy、vllm、ollama等工具快捷使用，也可以用transformers部署。

创建开发机，选择cuda12.2的镜像，选择30%的A100。

## 创建环境、安装依赖

微调需要用到lmdeploy，先创建一个用于微调的虚拟环境lmdeploy：

```
conda create -n lmdeploy-vl python=3.10 -y
```

创建完成后，激活这个虚拟环境，并且在这个环境中安装所需依赖：

```
conda activate lmdeploy-vl
pip install torch==2.4.0+cu121 torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu121
pip install lmdeploy==0.7.3 timm==1.0.15 flash_attn==2.7.3
```

## InternVL3-2B的cli-demo

在/root/demo文件夹里创建一个cli-demo.py脚本，用于执行internvl3-2b模型的推理。

目标模型InternVL3-2B模型已经提前存放与/share/new_models/InternVL3/InternVL3-1B里，无需再下载。

```
cd /root &amp;&amp; mkdir demo
cd /root/demo
touch cli-demo.py
```

简易的cli-demo.py脚本如下，复制后粘贴进入/root/demo/cli-demo.py即可：


image = load_image(&#x27;https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg&#x27;)

```
from lmdeploy import pipeline, TurbomindEngineConfig, GenerationConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = &#x27;/share/new_models/InternVL3/InternVL3-2B&#x27;
pipe = pipeline(model, backend_config=TurbomindEngineConfig(session_len=16384, tp=1), chat_template_config=ChatTemplateConfig(model_name=&#x27;internvl2_5&#x27;))
gen_config = GenerationConfig(top_k=40, top_p=0.8, temperature=0.8)
sess = pipe.chat((&#x27;描述一下图片&#x27;, image), gen_config=gen_config)
print(sess.response.text)
sess = pipe.chat(&#x27;图中女人在干什么?&#x27;, session=sess, gen_config=gen_config)
print(sess.response.text)
```

之后执行cli-demo.py：

```
conda activate lmdeploy-vl
python /root/demo/cli-demo.py
```

执行后效果如下：

![](img/3c09938e05fae34c08667743adc4bced.png)

## InternVL3-2B的web-demo

使用transformers部署InternVL3-2B模型，先在/root/demo里创建web-demo.py:

```
cd /root/demo
touch web-demo.py
```

然后把调用transformers并且执行推理的脚本加入gradio的前端交互，用如下代码实现：

```
import gradio as gr
import torch
from transformers import AutoModel, AutoTokenizer, AutoConfig
from PIL import Image

# 加载模型部分
path = &#x27;/root/share/new_models/InternVL3/InternVL3-2B&#x27;
device_map = None
model = AutoModel.from_pretrained(
    path,
    torch_dtype=torch.bfloat16,
    load_in_8bit=False,
    low_cpu_mem_usage=True,
    use_flash_attn=False,
    trust_remote_code=True,
    device_map=&quot;auto&quot; if device_map is None else device_map
).eval()
tokenizer = AutoTokenizer.from_pretrained(path, trust_remote_code=True, use_fast=False)

# 预处理图片函数
def preprocess(image, max_num=12, image_size=448):
    from torchvision import transforms as T
    from torchvision.transforms.functional import InterpolationMode
    transform = T.Compose([
        T.Lambda(lambda img: img.convert(&#x27;RGB&#x27;) if img.mode != &#x27;RGB&#x27; else img),
        T.Resize((image_size, image_size), interpolation=InterpolationMode.BICUBIC),
        T.ToTensor(),
        T.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])
    ])
    pixel_values = transform(image)
    pixel_values = pixel_values.unsqueeze(0).to(torch.bfloat16).cuda()
    return pixel_values

# 推理函数
def predict(image, message, history):
    if image is None:
        return history + [(&quot;请上传一张图片&quot;, &quot;&quot;)]

    pixel_values = preprocess(image)
    
    if history is None or len(history) == 0:
        user_input = f&quot;&lt;image&gt;\n{message}&quot;
        response, new_history = model.chat(
            tokenizer, pixel_values, user_input,
            generation_config={&quot;max_new_tokens&quot;: 1024, &quot;do_sample&quot;: True},
            history=None, return_history=True
        )
    else:
        response, new_history = model.chat(
            tokenizer, pixel_values, message,
            generation_config={&quot;max_new_tokens&quot;: 1024, &quot;do_sample&quot;: True},
            history=history, return_history=True
        )
    
    return new_history

# Gradio界面
with gr.Blocks(title=&quot;InternVL3-2B Web Demo&quot;) as demo:
    gr.Markdown(&quot;&lt;h1 align=&#x27;center&#x27;&gt;📷 InternVL3-2B Chat Demo&lt;/h1&gt;&quot;)

    
    with gr.Row():
        with gr.Column(scale=1):
            image_input = gr.Image(type=&quot;pil&quot;, label=&quot;上传一张图片&quot;)
        with gr.Column(scale=2):
            chatbot = gr.Chatbot(height=600, label=&quot;对话&quot;)
            message_input = gr.Textbox(placeholder=&quot;请输入你的问题...&quot;, label=&quot;提问&quot;)
            send_btn = gr.Button(&quot;发送&quot;)
    
    state = gr.State([])  # 保存history

    def on_send(image, message, history):
        if message.strip() == &quot;&quot;:
            return history
        history = predict(image, message, history)
        return history

    send_btn.click(
        fn=on_send,
        inputs=[image_input, message_input, state],
        outputs=[chatbot]
    )
    message_input.submit(
        fn=on_send,
        inputs=[image_input, message_input, state],
        outputs=[chatbot]
    )

demo.launch(server_name=&quot;127.0.0.1&quot;, server_port=6000, share=True)
```

执行/root/demo/web-demo.py：

```
conda activate lmdeploy-vl
pip install gradio==5.29.0
python /root/demo/web-demo.py
```

等终端出现gradio的url之后，在笔记本的命令行里进行端口映射：

```
ssh root@ssh.intern-ai.org.cn -p 你的ssh端口号 -CNg -L 6001:127.0.0.1:6000
```

输入密码后，在本地浏览器里输入https://127.0.0.1:6001即可进行网页端的模型推理。

> 有些电脑的命令行工具里，CTRL+V不是粘贴。如果粘贴密码之后，端口映射失**败，可能就是粘贴密码不成**功。解决**方法就是手**动打字输入那一串密码；或者设置添加公钥，避免CTRL+V粘贴密码。

启动之后，找了一张老虎的照片进行测试，效果如下：

![](img/e5e297ae6d843180108b6557d44d4349.png)

# InternVL3-9B的微调

创建开发机，选择cuda12.2的镜像，选择50%的A100。

## 创建环境、安装依赖

微调需要用到xtuner，先创建一个用于微调的虚拟环境xtuner：

```
conda create -n xtuner python=3.10 -y
```

创建完成后，激活这个虚拟环境，并且在这个环境中安装所需依赖：

```
conda activate xtuner
pip install torch==2.4.0+cu121 torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu121
pip install xtuner timm flash_attn datasets==2.21.0 deepspeed==0.16.1
conda install mpi4py -y
```

## 准备数据集

微调的目标是让目前大模型获得新的知识，相比于RAG提供的智能检索，大模型的知识需要通过训练(梯度下降)来获得。

新的知识，就是要自己准备一个新的数据集。经过一段时间的大模型使用，再结合自己兴趣爱好，发现目前主流的大模型对于三国演义电视剧的人物识别和画面剧情解读都很乏力。于是我用1994版《三国演义》的前5集制作了一个用于InternVL系列模型的训练集。

数据集存放于modelscope(https://www.modelscope.cn/datasets/livehouse/SanguoVLM-trainset/files)里，只需要把训练集下载到放在/root文件夹的./train文件夹里即可：

```
cd /root &amp;&amp; mkdir train
```

下载数据集：

```
apt install git-lfs -y
git lfs install
git clone https://oauth2:xxxxxxxxx@www.modelscope.cn/datasets/livehouse/SanguoVLM-trainset.git
```

上方代码里xxxxxxxxx的token需要替换为自己的token，获取方法是：

点击数据集页面：https://www.modelscope.cn/datasets/livehouse/SanguoVLM-trainset/files，之后“下载数据集”，然后下滑到最后的Git下载。

复制最下面的代码git clone https://oauth2:xxxxxxxxx@www.modelscope.cn/datasets/livehouse/SanguoVLM-trainset.git

![](img/240e072195ff5a81f1f79aa2264bdabb.png)

![](img/ce623fc8547ec3af60176fad53fb1177.png)

此时训练集里的文件结构为：

```
train/SanguoVLM-trainset
├── annotations-sft.jsonl
└── images
    ├── 三国演义1-桃园三结义-p1.mp4_20250507_140557.671.jpg
    ├── 三国演义1-桃园三结义-p1.mp4_20250507_140604.089.jpg
    ├── 三国演义1-桃园三结义-p1.mp4_20250507_140621.425.jpg
    ├── 三国演义1-桃园三结义-p1.mp4_20250507_140642.868.jpg
    ├── ……
    ├── ……
    ├── ……
    ├── 三国演义5-三英战吕布-p4.mp4_20250509_235249.187.jpg
    ├── 三国演义5-三英战吕布-p4.mp4_20250509_235639.840.jpg
    └── 三国演义5-三英战吕布-p4.mp4_20250509_235750.451.jpg
```

如果学习的时候想把这个SanguoVLM-trainset上传到huggingface平台，请把开源协议设置为cc-by-nc-nd-4.0，禁止商用。另外设置登录后方能下载，避免随机爬虫爬取，方便对其他大模型的能力测评。

如果有自己构建数据集，微调某个领域的VLM助手，可以参考https://internvl.readthedocs.io/en/latest/get_started/chat_data_format.html内容来制作数据集。

数据集准备完毕，个性化的处理流程就基本完成了。剩下的步骤是训练脚本里修改参数、开始训练、模型合并和模型测试。

## 训练参数修改

由于已经安装了xtuner的python包到环境里，因此不需要下载整个源码即可开始训练，只需要下载一个适合InternVL3-4B模型微调的python脚本即可。

从xtuner源码里找一个适配InternVL系列模型的训练脚本，直接修改一下参数就可以开始训练。先创建一下训练脚本名称为/root/train/internvl3-9b_lora_finetune.py：

```
cd /root/train
touch internvl3-9b_lora_finetune.py
```

把xtuner里的一个internvl2-8b模型的配置文件（https://github.com/InternLM/xtuner/blob/main/xtuner/configs/internvl/v2/internvl_v2_internlm2_5_8b_lora_finetune.py）拿来，稍微改一下参数和训练集路径。

github里internvl_v2_internlm2_5_8b_lora_finetune.py的内容如下：

```
# Copyright (c) OpenMMLab. All rights reserved.
from mmengine.hooks import (
    CheckpointHook,
    DistSamplerSeedHook,
    IterTimerHook,
    LoggerHook,
    ParamSchedulerHook,
)
from mmengine.optim import AmpOptimWrapper, CosineAnnealingLR, LinearLR
from peft import LoraConfig
from torch.optim import AdamW
from transformers import AutoTokenizer

from xtuner.dataset import InternVL_V1_5_Dataset
from xtuner.dataset.collate_fns import default_collate_fn
from xtuner.dataset.samplers import LengthGroupedSampler
from xtuner.engine.hooks import DatasetInfoHook
from xtuner.engine.runner import TrainLoop
from xtuner.model import InternVL_V1_5
from xtuner.utils import PROMPT_TEMPLATE

#######################################################################
#                          PART 1  Settings                           #
#######################################################################
# Model
path = &quot;OpenGVLab/InternVL2-8B&quot;

# Data
data_root = &quot;./data/llava_data/&quot;
data_path = data_root + &quot;LLaVA-Instruct-150K/llava_v1_5_mix665k.json&quot;
image_folder = data_root + &quot;llava_images&quot;
prompt_template = PROMPT_TEMPLATE.internlm2_chat
max_length = 8192

# Scheduler &amp; Optimizer
batch_size = 1  # per_device
accumulative_counts = 2
dataloader_num_workers = 4
max_epochs = 1
optim_type = AdamW
# official 1024 -&gt; 4e-5
lr = 1e-6
betas = (0.9, 0.999)
weight_decay = 0.05
max_norm = 1  # grad clip
warmup_ratio = 0.03

# Save
save_steps = 1000
save_total_limit = 1  # Maximum checkpoints to keep (-1 means unlimited)

#######################################################################
#            PART 2  Model &amp; Tokenizer &amp; Image Processor              #
#######################################################################
model = dict(
    type=InternVL_V1_5,
    model_path=path,
    freeze_llm=True,
    freeze_visual_encoder=True,
    # comment the following lines if you don&#x27;t want to use Lora in llm
    llm_lora=dict(
        type=LoraConfig,
        r=128,
        lora_alpha=256,
        lora_dropout=0.05,
        target_modules=None,
        task_type=&quot;CAUSAL_LM&quot;,
    ),
    # uncomment the following lines if you don&#x27;t want to use Lora in visual encoder # noqa
    # visual_encoder_lora=dict(
    #     type=LoraConfig, r=64, lora_alpha=16, lora_dropout=0.05,
    #     target_modules=[&#x27;attn.qkv&#x27;, &#x27;attn.proj&#x27;, &#x27;mlp.fc1&#x27;, &#x27;mlp.fc2&#x27;])
)

#######################################################################
#                      PART 3  Dataset &amp; Dataloader                   #
#######################################################################
llava_dataset = dict(
    type=InternVL_V1_5_Dataset,
    model_path=path,
    data_paths=data_path,
    image_folders=image_folder,
    template=prompt_template,
    max_length=max_length,
)

train_dataloader = dict(
    batch_size=batch_size,
    num_workers=dataloader_num_workers,
    dataset=llava_dataset,
    sampler=dict(
        type=LengthGroupedSampler,
        length_property=&quot;modality_length&quot;,
        per_device_batch_size=batch_size * accumulative_counts,
    ),
    collate_fn=dict(type=default_collate_fn),
)

#######################################################################
#                    PART 4  Scheduler &amp; Optimizer                    #
#######################################################################
# optimizer
optim_wrapper = dict(
    type=AmpOptimWrapper,
    optimizer=dict(type=optim_type, lr=lr, betas=betas, weight_decay=weight_decay),
    clip_grad=dict(max_norm=max_norm, error_if_nonfinite=False),
    accumulative_counts=accumulative_counts,
    loss_scale=&quot;dynamic&quot;,
    dtype=&quot;float16&quot;,
)

# learning policy
# More information: https://github.com/open-mmlab/mmengine/blob/main/docs/en/tutorials/param_scheduler.md  # noqa: E501
param_scheduler = [
    dict(
        type=LinearLR,
        start_factor=1e-5,
        by_epoch=True,
        begin=0,
        end=warmup_ratio * max_epochs,
        convert_to_iter_based=True,
    ),
    dict(
        type=CosineAnnealingLR,
        eta_min=0.0,
        by_epoch=True,
        begin=warmup_ratio * max_epochs,
        end=max_epochs,
        convert_to_iter_based=True,
    ),
]

# train, val, test setting
train_cfg = dict(type=TrainLoop, max_epochs=max_epochs)

#######################################################################
#                           PART 5  Runtime                           #
#######################################################################
# Log the dialogue periodically during the training process, optional
tokenizer = dict(
    type=AutoTokenizer.from_pretrained,
    pretrained_model_name_or_path=path,
    trust_remote_code=True,
)

custom_hooks = [
    dict(type=DatasetInfoHook, tokenizer=tokenizer),
]

# configure default hooks
default_hooks = dict(
    # record the time of every iteration.
    timer=dict(type=IterTimerHook),
    # print log every 10 iterations.
    logger=dict(type=LoggerHook, log_metric_by_epoch=False, interval=10),
    # enable the parameter scheduler.
    param_scheduler=dict(type=ParamSchedulerHook),
    # save checkpoint per `save_steps`.
    checkpoint=dict(
        type=CheckpointHook,
        save_optimizer=False,
        by_epoch=False,
        interval=save_steps,
        max_keep_ckpts=save_total_limit,
    ),
    # set sampler seed in distributed evrionment.
    sampler_seed=dict(type=DistSamplerSeedHook),
)

# configure environment
env_cfg = dict(
    # whether to enable cudnn benchmark
    cudnn_benchmark=False,
    # set multi process parameters
    mp_cfg=dict(mp_start_method=&quot;fork&quot;, opencv_num_threads=0),
    # set distributed parameters
    dist_cfg=dict(backend=&quot;nccl&quot;),
)

# set visualizer
visualizer = None

# set log level
log_level = &quot;INFO&quot;

# load from which checkpoint
load_from = None

# whether to resume training from the loaded checkpoint
resume = False

# Defaults to use random seed and disable `deterministic`
randomness = dict(seed=None, deterministic=False)

# set log processor
log_processor = dict(by_epoch=False)
```

上方代码里黄色标记的部分是模型路径、训练集路径、批次大小、训练轮数、学习率、保存间隔等参数，需要多次摸索合适调整。

+   其中模型路径(path)和训练集路径(data_root,data_path和image_folder)必须要调整正确，否则无法正确开始训练。

+   然后批次大小(batch_size)过大容易爆显存(CUDA Out of Memory)，然后训练中止。

+   训练轮数(max_epochs），如果太大的话，训练时间会十几个小时，不适合demo；如果太小，则训练出来的模型可能识别效果不佳。

+   学习率(lr)需要合理寻找，过大容易过拟合，过小容易不拟合。这个学习率的寻找，需要几十次几百次的训练进行练习和体会。

+   保存间隔(save_steps)，这个要根据训练的总迭代步数确定，如果本次训练是200次迭代，那么每40步保存一次训练结果比较合适，所以save_steps=40比较好。可以先设置save_steps=200，然后开始训练看看总迭代步数跟当前的保存间隔是否匹配。

+   保存次数(save_total_limit )，硬盘空间充足的话，尽量设置为-1，这样训练中间段的训练结果也能保存。一旦训练结束的时候过拟合了，那么中间段的尚未过拟合的训练结果，还能用一用。

训练参数适当修改后如下的训练脚本如下：

```
# Copyright (c) OpenMMLab. All rights reserved.
from mmengine.hooks import (
    CheckpointHook,
    DistSamplerSeedHook,
    IterTimerHook,
    LoggerHook,
    ParamSchedulerHook,
)
from mmengine.optim import AmpOptimWrapper, CosineAnnealingLR, LinearLR
from mmengine.optim.scheduler import MultiStepLR
from mmengine.visualization import Visualizer, WandbVisBackend
from peft import LoraConfig
from torch.optim import AdamW
from transformers import AutoTokenizer

from xtuner.dataset import InternVL_V1_5_Dataset
from xtuner.dataset.collate_fns import default_collate_fn
from xtuner.dataset.samplers import LengthGroupedSampler
from xtuner.engine.hooks import DatasetInfoHook
from xtuner.engine.runner import TrainLoop
from xtuner.model import InternVL_V1_5
from xtuner.utils import PROMPT_TEMPLATE

#######################################################################
#                          PART 1  Settings                           #
#######################################################################
# Model
path = &#x27;/root/share/new_models/InternVL3/InternVL3-9B&#x27;
# path = &#x27;/root/train/merged&#x27;

# Data
data_root = &quot;/root/train/SanguoVLM-trainset/&quot;
data_path = data_root + &quot;annotations-sft.jsonl&quot;
image_folder = data_root
# 这里不需要是data_root + &quot;images&quot;，.jsonl文件里加了`images`，再加则会出现images/images/xxx.jpg的嵌套行为
prompt_template = PROMPT_TEMPLATE.internlm2_chat
max_length = 8192

# Scheduler &amp; Optimizer
batch_size = 1  # per_device
accumulative_counts = 2
dataloader_num_workers = 4
max_epochs = 8
optim_type = AdamW
# official 1024 -&gt; 4e-5
lr = 2e-5
betas = (0.9, 0.999)
weight_decay = 0.05
max_norm = 1  # grad clip
warmup_ratio = 0.03

# Save
save_steps = 200
save_total_limit = -1  # Maximum checkpoints to keep (-1 means unlimited)

#######################################################################
#            PART 2  Model &amp; Tokenizer &amp; Image Processor              #
#######################################################################
model = dict(
    type=InternVL_V1_5,
    model_path=path,
    freeze_llm=True,
    freeze_visual_encoder=True,
    # comment the following lines if you don&#x27;t want to use Lora in llm
    llm_lora=dict(
        type=LoraConfig,
        r=64,
        lora_alpha=256,
        lora_dropout=0.05,
        target_modules=None,
        task_type=&quot;CAUSAL_LM&quot;,
    ),
    # uncomment the following lines if you don&#x27;t want to use Lora in visual encoder # noqa
    # visual_encoder_lora=dict(
    #     type=LoraConfig, r=64, lora_alpha=16, lora_dropout=0.05,
    #     target_modules=[&#x27;attn.qkv&#x27;, &#x27;attn.proj&#x27;, &#x27;mlp.fc1&#x27;, &#x27;mlp.fc2&#x27;])
)

#######################################################################
#                      PART 3  Dataset &amp; Dataloader                   #
#######################################################################
llava_dataset = dict(
    type=InternVL_V1_5_Dataset,
    model_path=path,
    data_paths=data_path,
    image_folders=image_folder,
    template=prompt_template,
    max_length=max_length,
)

train_dataloader = dict(
    batch_size=batch_size,
    num_workers=dataloader_num_workers,
    dataset=llava_dataset,
    sampler=dict(
        type=LengthGroupedSampler,
        length_property=&quot;modality_length&quot;,
        per_device_batch_size=batch_size * accumulative_counts,
    ),
    collate_fn=dict(type=default_collate_fn),
)

#######################################################################
#                    PART 4  Scheduler &amp; Optimizer                    #
#######################################################################
# optimizer
optim_wrapper = dict(
    type=AmpOptimWrapper,
    optimizer=dict(type=optim_type, lr=lr, betas=betas, weight_decay=weight_decay),
    clip_grad=dict(max_norm=max_norm, error_if_nonfinite=False),
    accumulative_counts=accumulative_counts,
    loss_scale=&quot;dynamic&quot;,
    dtype=&quot;float16&quot;,
)

# learning policy
# More information: https://github.com/open-mmlab/mmengine/blob/main/docs/en/tutorials/param_scheduler.md  # noqa: E501
# param_scheduler = dict(type=&#x27;MultiStepLR&#x27;, by_epoch=True, milestones=[8, 11], gamma=0.1)
# fyi → https://mmengine.readthedocs.io/zh-cn/latest/tutorials/param_scheduler.html
param_scheduler = [
    dict(
        type=LinearLR,
        start_factor=1e-5,
        by_epoch=True,
        begin=0,
        end=warmup_ratio * max_epochs,
        convert_to_iter_based=True,
    ),
    # 主学习率调度器
    dict(type=&#x27;MultiStepLR&#x27;,
         by_epoch=False,  # 按轮次更新学习率
         milestones=[3500, 5000],
         gamma=0.1,
    ),
    # dict(
    #     type=CosineAnnealingLR,
    #     eta_min=0.0,
    #     by_epoch=True,
    #     begin=warmup_ratio * max_epochs,
    #     end=max_epochs,
    #     convert_to_iter_based=True,
    # ),
]

# train, val, test setting
train_cfg = dict(type=TrainLoop, max_epochs=max_epochs)

#######################################################################
#                           PART 5  Runtime                           #
#######################################################################
# Log the dialogue periodically during the training process, optional
tokenizer = dict(
    type=AutoTokenizer.from_pretrained,
    pretrained_model_name_or_path=path,
    trust_remote_code=True,
)

custom_hooks = [
    dict(type=DatasetInfoHook, tokenizer=tokenizer),
]

# configure default hooks
default_hooks = dict(
    # record the time of every iteration.
    timer=dict(type=IterTimerHook),
    # print log every 10 iterations.
    logger=dict(type=LoggerHook, log_metric_by_epoch=False, interval=10),
    # enable the parameter scheduler.
    param_scheduler=dict(type=ParamSchedulerHook),
    # save checkpoint per `save_steps`.
    checkpoint=dict(
        type=CheckpointHook,
        save_optimizer=False,
        by_epoch=False,
        interval=save_steps,
        max_keep_ckpts=save_total_limit,
    ),
    # set sampler seed in distributed evrionment.
    sampler_seed=dict(type=DistSamplerSeedHook),
)

# configure environment
env_cfg = dict(
    # whether to enable cudnn benchmark
    cudnn_benchmark=False,
    # set multi process parameters
    mp_cfg=dict(mp_start_method=&quot;fork&quot;, opencv_num_threads=0),
    # set distributed parameters
    dist_cfg=dict(backend=&quot;nccl&quot;),
)

# set visualizer
visualizer = None
# visualizer = dict(
#    type=Visualizer,
#    vis_backends=[
#        dict(type=WandbVisBackend,
#            init_kwargs=dict(project=&#x27;internvl2-example&#x27;))])

# set log level
log_level = &quot;INFO&quot;

# load from which checkpoint
load_from = None

# whether to resume training from the loaded checkpoint
resume = False
 # --resume &quot;/root/train/lora_output/iter_1040.pth&quot;

# Defaults to use random seed and disable `deterministic`
randomness = dict(seed=None, deterministic=False)

# set log processor
log_processor = dict(by_epoch=False)
```

修改完毕后，使用xtuner train命令启动微调即可。

其中训练过程的学习率调度器param_scheduler安排比较重要，其次是总训练轮数max_epochs。调度器的详细调整可以参考https://mmengine.readthedocs.io/zh-cn/latest/tutorials/param_scheduler.html。

## 开始微调

微调代码如下：

```
cd /root/train
mkdir lora_output # 方便储存微调过程中各个阶段的模型
conda activate xtuner
export HF_ENDPOINT=https://hf-mirror.com
xtuner train /root/train/internvl3-9b_lora_finetune.py --work-dir /root/train/lora_output --deepspeed deepspeed_zero1
```

![](img/b2729ab905aaa8ea8f9290756f445bb4.png)

训练的结果会存放在/root/train/lora_output里，是一个个.pth的文件。

训练得到的iter_xxx.pth文件，需要跟InternVL3-9B模型合并，然后才能对合并后的模型进行推理，查看其对三国演义前5集的识别效果。

> 也可以用后面几集的人物测试，只要这个人在训练集里有，应该能识别出来身份，就是当前画面的剧情肯定无法准确识别。

![](img/d0e738f8fb216952c551be25c7cb7984.png)

![](img/ed1b296b03f843a96d8390863371cf0e.png)

## 模型合并

由于没有下载xtuner的源代码，所以模型合并的脚本，要自己在/root/train里自己建一个：

```
cd /root/train
touch internvl3-9b-merge.py
```

而merge的脚本内容在https://github.com/InternLM/xtuner/blob/main/xtuner/configs/internvl/v1_5/convert_to_official.py里有，内容如下：

```
# Copyright (c) OpenMMLab. All rights reserved.
import argparse
import os.path as osp

import torch
from mmengine.config import Config
from transformers import AutoTokenizer

from xtuner.model.utils import LoadWoInit
from xtuner.registry import BUILDER


def convert_to_official(config, trained_path, save_path):
    cfg = Config.fromfile(config)
    cfg.model.pretrained_pth = trained_path
    cfg.model.quantization_vit = False
    cfg.model.quantization_llm = False

    with LoadWoInit():
        model = BUILDER.build(cfg.model)
    model.to(torch.bfloat16)

    if model.use_visual_encoder_lora:
        vision_model = model.model.vision_model.merge_and_unload()
        model.model.vision_model = vision_model

    if model.use_llm_lora:
        language_model = model.model.language_model.merge_and_unload()
        model.model.language_model = language_model

    model.model.save_pretrained(save_path)

    tokenizer = AutoTokenizer.from_pretrained(
        cfg.model.model_path, trust_remote_code=True
    )
    tokenizer.save_pretrained(save_path)

    print(model)


def main():
    parser = argparse.ArgumentParser(
        description=&quot;Convert the pth model to HuggingFace model&quot;
    )
    parser.add_argument(&quot;config&quot;, help=&quot;config file name or path.&quot;)
    parser.add_argument(&quot;trained_model_pth&quot;, help=&quot;The trained model path.&quot;)
    parser.add_argument(&quot;save_path&quot;, help=&quot;The path to save the converted model.&quot;)
    args = parser.parse_args()

    if osp.realpath(args.trained_model_pth) == osp.realpath(args.save_path):
        raise ValueError(&quot;The trained path and save path should not be the same.&quot;)

    convert_to_official(args.config, args.trained_model_pth, args.save_path)


if __name__ == &quot;__main__&quot;:
    main()
```

将其复制后，粘贴到/root/train/internvl3-9b-merge.py里，内容无需修改，就是使用该脚本的时候需要指定格式。

执行模型合并脚本的格式是：

```
python internvl3-9b-merge.py [-h] config trained_model_pth save_path
```

+   config是训练的配置文件，/root/train/internvl3-9b_lora_finetune.py

+   trained_model_pth，就是训练结果/root/train/lora_output/iter_xxx.pth

+   save_path，就是合并后模型想保存的路径，可以自由命名，比如/root/train/InternVL3-9B-iter100,/root/train/merged

因此python /root/train/internvl3-9b-merge.py合并的命令为：

```
conda activate xtuner
python /root/train/internvl3-9b-merge.py /root/train/internvl3-9b_lora_finetune.py /root/train/lora_output/iter_xxx.pth /root/train/merged/
```

7分钟后合并完成：

![](img/873604fce5cf836d6c1d2056753a63e3.png)

## 微调后模型的web-demo

之前部署InternVL3-9B模型的web-demo.py，修改一下模型路径为/root/train/merged，即可进行推理测试。

先创建一个web-demo-sft.py的文件，用于保存新模型做web-demo的代码：

```
cd /root/train
touch web-demo-sft.py
```

然后下方的代码已经把模型路径从/root/share/new_models/InternVL3/InternVL3-9B替换为/root/train/merged了，复制后粘贴进web-demo-sft.py即可：

```
import gradio as gr
import torch
from transformers import AutoModel, AutoTokenizer, AutoConfig
from PIL import Image

# 加载模型部分
path = &#x27;/root/train/merged&#x27;
# path = &#x27;/root/share/new_models/InternVL3/InternVL3-8B&#x27;
device_map = None
model = AutoModel.from_pretrained(
    path,
    torch_dtype=torch.bfloat16,
    load_in_8bit=False,
    low_cpu_mem_usage=True,
    use_flash_attn=False,
    trust_remote_code=True,
    device_map=&quot;auto&quot; if device_map is None else device_map
).eval()
tokenizer = AutoTokenizer.from_pretrained(path, trust_remote_code=True, use_fast=False)

# 预处理图片函数
def preprocess(image, max_num=12, image_size=448):
    from torchvision import transforms as T
    from torchvision.transforms.functional import InterpolationMode
    transform = T.Compose([
        T.Lambda(lambda img: img.convert(&#x27;RGB&#x27;) if img.mode != &#x27;RGB&#x27; else img),
        T.Resize((image_size, image_size), interpolation=InterpolationMode.BICUBIC),
        T.ToTensor(),
        T.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])
    ])
    pixel_values = transform(image)
    pixel_values = pixel_values.unsqueeze(0).to(torch.bfloat16).cuda()
    return pixel_values

# 推理函数
def predict(image, message, history):
    if image is None:
        return history + [(&quot;请上传一张图片&quot;, &quot;&quot;)]

    pixel_values = preprocess(image)
    
    if history is None or len(history) == 0:
        user_input = f&quot;&lt;image&gt;\n{message}&quot;
        response, new_history = model.chat(
            tokenizer, pixel_values, user_input,
            generation_config={&quot;max_new_tokens&quot;: 1024, &quot;do_sample&quot;: True},
            history=None, return_history=True
        )
    else:
        response, new_history = model.chat(
            tokenizer, pixel_values, message,
            generation_config={&quot;max_new_tokens&quot;: 1024, &quot;do_sample&quot;: True},
            history=history, return_history=True
        )
    
    return new_history

# Gradio界面
with gr.Blocks(title=&quot;InternVL3-9B-sft Web Demo&quot;) as demo:
    gr.Markdown(&quot;&lt;h1 align=&#x27;center&#x27;&gt;📷 InternVL3-9B-sft Chat Demo&lt;/h1&gt;&quot;)
    gr.Markdown(&quot;&lt;p align=&#x27;center&#x27;&gt;⚠️ 务必上传&lt;strong&gt;正方形图片&lt;/strong&gt;，否则人脸压缩，识别不准。推荐分辨率为 &lt;code&gt;448×448&lt;/code&gt; 尺寸仍然能看清人脸的图片。&lt;/p&gt;&quot;)
    
    with gr.Row():
        with gr.Column(scale=1):
            image_input = gr.Image(type=&quot;pil&quot;, label=&quot;上传一张图片&quot;)
        with gr.Column(scale=2):
            chatbot = gr.Chatbot(height=600, label=&quot;对话&quot;)
            message_input = gr.Textbox(placeholder=&quot;请输入你的问题...&quot;, label=&quot;提问&quot;)
            send_btn = gr.Button(&quot;发送&quot;)
    
    state = gr.State([])  # 保存history

    def on_send(image, message, history):
        if message.strip() == &quot;&quot;:
            return history
        history = predict(image, message, history)
        return history

    send_btn.click(
        fn=on_send,
        inputs=[image_input, message_input, state],
        outputs=[chatbot]
    )
    message_input.submit(
        fn=on_send,
        inputs=[image_input, message_input, state],
        outputs=[chatbot]
    )

demo.launch(server_name=&quot;127.0.0.1&quot;, server_port=6000, share=True)
```

代码粘贴完成后，在merged模型里加入chat模版:

```
cp /root/share/new_models/InternVL3/InternVL3-9B/modeling_internvl_chat.py /root/train/merged

cp /root/share/new_models/InternVL3/InternVL3-9B/modeling_intern_vit.py /root/train/merged

cp /root/share/new_models/InternVL3/InternVL3-9B/conversation.py /root/train/merged

cp /root/share/new_models/InternVL3/InternVL3-9B/modeling_internlm2.py /root/train/merged
```

执行推理：

```
conda activate xtuner
pip install gradio==5.29.0
cd /root/train
python web-demo-sft.py
```

之后本地笔记本进行端口映射：

```
ssh root@ssh.intern-ai.org.cn -p 你的ssh端口号 -CNg -L 6004:127.0.0.1:6000
```

然后本地笔记本打开127.0.0.1:6004，开始测试新模型的三国演义人物识别和剧情解读的能力吧！

## 微调结果展示

max_epochs=8的时候，3小时的训练效果如下：

![](img/bcb444b1f225fcc2ff8be61138325a61.png)

![](img/60283c347cfea58308761df8b16df072.png)

![](img/bca26929a4398bf348821d2d9754f950.png)

![](img/26e5397c0c56c62dba2b5dfaa02b9e3d.png)

![](img/dfc1badad0b8f4a0301c7f9dc9d0468d.png)

![](img/4cd13affaf5ea9d86263a4355ee4c2ec.png)

![](img/dfe210bc199d1ea30ffa2700f3691903.png)

![](img/d5ea21c37370288ebc882eae02901c63.png)

![](img/317e4b74d5702deb0fbb676816253697.png)

人物识别上有一些准确度，但是剧情上很不准确。属于欠拟合，需要在细化上下功夫，先用大学习率快速学习总体特征，然后再用小学习率学习各种细化特征(各个人物之间的区别)。

上一轮训练的时候，loss下降很快，已经学习到了总体特征(语言风格，剧情讲述)。

准确度不足的问题，就是各条数据里的细化差别还需要小火慢炖，用低学习率慢慢跑若干个epochs。这时候loss基本不会再快速下降了，而是在细化学习每一条数据的特征，方便模型能更加准确地识别和解读。

---

经过测试发现，训练素材是1440*1080的长方形图片，会被压缩为448*448的方形图片导致人脸变形，训练效果大降。后来crop为方形图片进行训练。同时设置max_epochs=12，训练3个多小时，选择了iter_9336.pth，merge之后进行web-demo，发现识别准确度更高了一些，效果如下：

> 1、个别冷门人物，在训练集里数量很少，所以模型对其学习程度不到位，因此训练集里的几个冷门人物识别效果不好。

> 2、多人图片如果压缩到448*448尺寸，那么人脸的区域基本糊成一小块了，模型很难识别，因此多人图片放进去识别效果不太稳定。

成功案例：

![](img/003c513ac746752415973be3c667e706.png)

![](img/83020d058d24f074a0ddaec54e1a1387.png)

![](img/7481cfbc78f4be4667dd16e24c01644e.png)

![](img/8efcff0d2c2334e5c56d375308a671ff.png)

![](img/dcf68c41919956d879350effc3c121af.png)

![](img/5448673a8c18b3efca4c168ab19ba607.png)

![](img/ba0ced2cdf0a7ccdcb57c1ae436079cb.png)

![](img/67fc0b9c6cbbbe65c82b2cf0315b451e.png)

![](img/4aecabda80042f7e8b3391ed5f792f95.png)

![](img/b7fdff187fca1469af8b5cf732ca415b.png)

> 剧情解读有些小瑕疵，估计是模型推理之时，随机参数设置过高。

![](img/633d5d6027a7819c21cab10f4e90927d.png)

![](img/f8754d06ea2c73c73558378776ecfadc.png)

![](img/cc7712c95e9c5e12d0d9974a24769d61.png)

![](img/d8ffb85618df6b5e91b0e3bea2301964.png)

![](img/70bc925afee230208c91a410222cf735.png)

![](img/f1a868421ae09a41a2f13d42e504c7bb.png)

![](img/1f0764b1609349d520cbdcc2602d4249.png)

![](img/54f394e840f13cb4dacba6064ad929fc.png)

![](img/b6a7077e024efb66c970f5bf918dc041.png)

![](img/f8905f2c3ee9a01b7224a8e3e2fcf299.png)

> 曹操这个角色的人物识别和剧情解读的训练效果很好，除了训练过程合理安排了，还有个原因是训练集里曹操的图片很多。前5集《桃园三结义》、《十常侍乱政》、《董卓霸京师》、《孟德献刀》、《三英战吕布》，除了第一集曹操没啥出镜，其他每集里60%以上的分镜都有曹操出现。因此曹操图片多了，就能充分学习。

失败案例：

![](img/94432051096bf84789ffc40e500bf6aa.png)

![](img/29161a2d0d029f17d990925f78852e1d.png)

> 刘备识别失败原因，在于训练集里刘备的肖像太少了。训练集里一共259张图片，属于刘备的图片也就才10张左右，比张飞和关羽还少。训练集里图片最多的就是曹操了，有三四十张。

![](img/840497595671c034b18a9faf5171b09c.png)

> 剧情解读错误，这个画面是刘关张平定黄巾之后，在去安喜县赴任的路上，一共就截取了3张图放入训练集中。

![](img/d2fe4f8db15d71fd9cb6c6e8fa4be698.png)

> 身份识别正确，但剧情背景知识错误，算是模型生成时候，概率分布导致。

![](img/336e8287823b30c5ed9669b630e02755.png)

> 卢植只在训练集里出现了一次，无法准确识别也可以理解

![](img/1cf22263934f0ec4db637b7db66ac22e.png)

> 多人图片，448*448像素块里，难以准确识别

![](img/88f20429ad449c629a581608aa4889a8.png)

![](img/42c441ca78e9b255eeebc20326970e25.png)

![](img/86d6532028dbb9eb3afa2b61c5bcb14a.png)

> 何氏的图片也有限，拟合的不是很到位。同一张图片让模型回答好几次，得到的答案有的是曹操有的是董太后有的是何皇后。

# 使用VLMEvalKit测评

创建开发机，选择cuda12.2的镜像，选择50%的A100。

## 安装依赖

创建环境：

```
# 基于conda环境
conda create -n VLMEvalKit python=3.10 -y
```

安装依赖：

```
conda activate VLMEvalKit
pip install idna requests
pip install torch==2.4.0 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

git clone https://ghfast.top/https://github.com/open-compass/VLMEvalKit.git
cd VLMEvalKit
pip install -e .
pip install timm einops accelerate lmdeploy openai datasets==2.19.2
```

根据文档介绍https://github.com/open-compass/VLMEvalKit/blob/main/docs/zh-CN/ConfigSystem.md，默认情况下，VLMEvalKit通过在run.py脚本中使用--model和--data参数设置模型名称（在/vlmeval/config.py中定义）和数据集名称（在vlmeval/dataset/__init__.py 或 vlmeval/dataset/video_dataset_config.py 中定义）来启动评估。

由于测评需要用/root/train/merged-9336这个模型，用/root/train/SanguoVLM-testset这个测试集。

## 添加新测评集

根据https://github.com/open-compass/VLMEvalKit/blob/main/docs/zh-CN/Development.md，添加新的benchmark。

需要一步一步修改VLMEvalKit的代码，把自己的一个数据集整理为VLMEvalKit支持的测评集。我把修改整理到了自己的VLMEvalKit仓库里https://github.com/HelloWarcraft/VLMEvalKit，可以直接从这个仓库里下载VLMEvalKit源码，就可以跳过3.3节的新增测评集的内容了：

```
cd /root &amp;&amp; mkdir temp
cd /root/temp
git clone https://github.com/HelloWarcraft/VLMEvalKit
```

之后执行python run.py之时，--data SanguoDataset就可以生效了。

## 添加新模型

/root/temp/VLMEvalKit/vlmeval/config.py文件中设置在 VLMEvalKit 中支持的 VLM 名称，以及模型路径。

由于InternVL3-9B的模型在本地已经有了，这里在VLMEvalKit/vlmeval/config.py下第802行，新增模型名称和模型路径：

```
    &quot;InternVL3-9B-merged-9336&quot;: partial(
        InternVLChat, model_path=&quot;/root/train/merged-9336&quot;, version=&quot;V2.0&quot;
    ),
```

![](img/0b47a261917f0abc2959640db2ff5bec.png)

这样即可通过"InternVL3-9B-merged-9336"来调用"/root/train/merged-9336"。

其实也可以使用lmdeploy把/root/train/merged-9336模型部署为InternVL3-9B-merged-9336，命令如下：

```
conda activate VLMEvalKit
pip install lmdeploy
cd /root/temp/VLMEvalKit
lmdeploy serve api_server /root/train/merged-9336 --model-name InternVL3-9B-merged-9336
```

之后执行python run.py之时，--model InternVL3-9B-merged-9336就可以生效了。

## 开始测评

### 使用internlm2_5-7b-chat的api作为测评LLM

由于是开放型问题，待测试的新模型根据测评集的问题会得到一个prediction，prediction跟标准答案answer比对。

目前的VLM是输出一段文本，不是分类或者选择题，直接判别prediction==answer肯定不合适，那么如何评判prediction是否跟answer匹配？

这时候就另外需要一个LLM来当测评的老师，比对prediction和answer，VLMEvalKit支持用GPT-4、Gemini等在线LLM进行测评，只需要环境变量里设置：

```
export OPENAI_API_KEY=
export GOOGLE_API_KEY=
```

也可以用本地LLM经过lmdeploy部署，然后，使用本地部署的LLM来当测评老师。

因此在实际开始测评之前，要给测评老师的这个LLM安排到位。

这里使用本地的internlm2_5-7b-chat当测评老师。新开终端，将其部署为api格式：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
lmdeploy serve api_server /share/new_models/Shanghai_AI_Laboratory/internlm2_5-7b-chat --server-port 23333 --api-keys sk-123456 --cache-max-entry-count 0.2
```

> 密钥必须以 "sk-" 开头，后跟任意数字。（https://github.com/open-compass/VLMEvalKit/blob/main/docs/zh-CN/Quickstart.md）

之后要新开一个终端，指定这个老师的api地址和api密钥：

```
cd /root/temp/VLMEvalKit
touch temp.txt
```

在temp.txt里粘贴如下内容：

```
OPENAI_API_KEY=sk-123456
OPENAI_API_BASE=http://0.0.0.0:23333/v1/chat/completions
LOCAL_LLM=/share/new_models/Shanghai_AI_Laboratory/internlm2_5-7b-chat
```

之后修改temp.txt的名称为.env：

```
cp /root/temp/VLMEvalKit/temp.txt /root/temp/VLMEvalKit/.env
```

### 开始测评

#### 使用SanguoDataset测评集

SanguoDataset里有186条测试数据，为了降低测评时间，设置了一个SanguoDataset_MINI的小测评集，里面有62条数据。只测评SanguoDataset_MINI即可，很省时间，耗时会缩短SanguoDataset的1/3。

使用以下命令，用新测评集，先测评InternVL3-2B，观察没微调的InternVL3-2B模型：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset --model InternVL3-2B --verbose --work-dir /root/temp/VLMEvalKit/results
```

测评完毕，总分186分，InternVL3-2B拿了108.6分，得分率是58.39%：

![](img/6d28b5f0f8bcc7f5f16f8d5cec477d76.png)

使用以下命令开始用InternVL3-9B-merged-9336测评新数据集：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset --model InternVL3-9B-merged-9336 --verbose --work-dir /root/temp/VLMEvalKit/results
```

这个模型的测评，前半段生成prediction过程就需要给5小时，prediction跟answer比对过程又估计需要2小时，一共7小时。所以这个目前不做了，除非是真心准备了测评集，在严格测评模型的推理能力。

#### 使用SanguoDataset_MINI测评集

186条测评数据的SanguoDataset.tsv已经被简化为SanguoDataset_MINI.tsv的39条数据了。然后添加到了VLMEvalKit里，之后--data SanguoDataset_MINI命令生效。

开始测评InternVL3-2B模型：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset_MINI --model InternVL3-2B --verbose --work-dir /root/temp/VLMEvalKit/results
```

prediction生成完毕，之后开始prediction跟answer的比对：

![](img/0f5b2fe46ccaab54b5256705d1d970b7.png)

比对完成，InternVL3-2B得分是16.5分，得分率42.31%：

![](img/4cc4ed50e8a402abfdd7ba6ad0f13164.png)

测评InternVL3-9B-merged-9336模型：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset_MINI --model InternVL3-9B-merged-9336 --verbose --work-dir /root/temp/VLMEvalKit/results
```

评分完成，InternVL3-9B-merged-9336得分是19.7分，得分率50.51%：

![](img/0a2d06277bee3a6df23dc5a6f169127a.png)

其中**人物识别**类题目9道，微调前的回答全都是错的，微调后有74.4%的正确率。训练集跟测评集里的图完全不一样，相当于考试考新题，人物识别74.4%的正确率，效果很不错了。

**剧情问答**类题目，提问的时候就已经剧透了，微调前的InternVL3-2B都能回答个七七八八。微调后分数反而下降，大概是因为prediction跟answer比对的时候，不太严谨。

**画面解释**类题目，则纯粹是解释画面，是否微调几乎没影响。

#### 使用SanguoDataset_AB_MINI测评集

新增的AB_MINI测评集，划分为3个分类各20个问题，更加平均。

开始测评InternVL3-2B模型：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset_AB_MINI --model InternVL3-2B --verbose --work-dir /root/temp/VLMEvalKit/results
```

InternVL3-2B测评结果如下：

![](img/343ed1171bfbf99f54a90d3d31ed72f8.png)

在VLMEvalKit/results里有InternVL3-2B_SanguoDataset_AB_MINI_gpt-4-turbo.xlsx的文件，里面记录了每一条的判分结果：

![](img/a43252c5dd9d5d77eb7782ac8dca7a29.png)

测评InternVL3-9B-merged-9336模型：

```
conda activate VLMEvalKit
cd /root/temp/VLMEvalKit
python run.py --data SanguoDataset_AB_MINI --model InternVL3-9B-merged-9336 --verbose --work-dir /root/temp/VLMEvalKit/results
```

InternVL3-9B-merged-9336的测评效果如下：

![](img/74f8278655ec27ead5c809a1eb933586.png)

每一条的判分结果如下：

![](img/8cb1a32d8074b66dc70fd99eafbcec18.png)