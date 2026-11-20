
# 生成视频的描述性文本

> 来源：[https://ai.feishu.cn/docx/LqNud0TlOo69Ixxt0JucEgp9nIg](https://ai.feishu.cn/docx/LqNud0TlOo69Ixxt0JucEgp9nIg)

通常情况下，大多数视频数据都没有相应的描述性文本，因此有必要将视频数据转换为文本描述，为文本到视频模型提供必要的训练数据。

## 用 CogVLM2-Video 生成视频的描述性文本

🤗 [Hugging Face](https://huggingface.co/THUDM/cogvlm2-video-llama3-chat) | 🤖 [ModelScope](https://modelscope.cn/models/ZhipuAI/cogvlm2-video-llama3-chat) | 📑 [Blog](https://cogvlm2-video.github.io/) ｜ 💬 [Online Demo](http://cogvlm2-online.cogviewai.cn:7868/)

CogVLM2-Video 是一种多功能视频理解模型，具有基于时间戳的问题解答功能。用户可以向模型输入 `请详细描述这段视频` 等提示语，以获得详细的视频字幕：

![](img/6363868da9cfc411339dc3c065960d6d.png)

### CogVLM2-Video CLI 演示

我们可以用 CLI 与 CogVLM2-Video 模型交互。CogVLM2-Video 是优化视频输入的 LLaMA 架构的一个变体。CLI 允许用户提供视频并通过文本查询与模型互动，生成结合视频信息的响应。

注意，需要 `Python 3.6` 或更高版本

#### 安装依赖

```
pip install -r requirements.txt
```

如果希望使用 CUDA 功能，请确保您的系统具有兼容的 GPU。

#### 使用 CLI 运行

`--quant`：指定模型加载的量化精度。接受 `4` 或 `8` 的值。默认值为 `0`（不使用量化）。

```
python script.py --quant 8
```

#### 导入以及常量和参数解析

+ `MODEL_PATH`: 定义了要使用的预训练模型路径，需要将 `THUDM/cogvlm2-video-llama3-chat` 换成本地路径。

+ `DEVICE`: 根据是否可用 `CUDA`，选择计算设备（`GPU` 或 `CPU`）。

+ `TORCH_TYPE`: 根据 `CUDA` 计算能力选择数据类型（`bfloat16` 或 `float16`）。

+ 使用 `argparse` 处理命令行参数，`--quant` 参数指定量化精度（4 位或 8 位）。如果模型路径中包含 `int4`，则将量化精度设为 4 位。

```
import io
import numpy as np
import torch
from decord import cpu, VideoReader, bridge
from transformers import AutoModelForCausalLM, AutoTokenizer, BitsAndBytesConfig
import argparse

MODEL_PATH = "THUDM/cogvlm2-video-llama3-chat"
DEVICE = 'cuda' if torch.cuda.is_available() else 'cpu'
TORCH_TYPE = torch.bfloat16 if torch.cuda.is_available() and torch.cuda.get_device_capability()[0] >= 8 else torch.float16

parser = argparse.ArgumentParser(description="CogVLM2-Video CLI Demo")
parser.add_argument('--quant', type=int, choices=[4, 8], help='启用4位或8位精度加载', default=0)
args = parser.parse_args()

if 'int4' in MODEL_PATH:
    args.quant = 4
```

#### 视频加载

+ `bridge.set_bridge('torch')`: 设置 `Decord` 库使用 `PyTorch` 作为桥接。

+ 从 `video_path` 读取视频文件并转换为二进制流。

+ 如果视频流存在，则用 `io.BytesIO` 将其包装成流；否则，直接读取视频路径。

+ `strategy` 参数决定如何选取视频帧：

+ `base` 策略：从视频开始的指定时间段中均匀选取帧。

+ `chat` 策略：根据帧的时间戳选择尽可能均匀分布的帧。

+ 将选择的帧提取为批量数据并调整维度顺序，以便 `PyTorch` 使用。

```
def load_video(video_path, strategy='chat'):
    bridge.set_bridge('torch')
    with open(video_path, 'rb') as f:
        mp4_stream = f.read()
    num_frames = 24

    if mp4_stream is not None:
        decord_vr = VideoReader(io.BytesIO(mp4_stream), ctx=cpu(0))
    else:
        decord_vr = VideoReader(video_path, ctx=cpu(0))
    frame_id_list = None
    total_frames = len(decord_vr)
    if strategy == 'base':
        clip_end_sec = 60
        clip_start_sec = 0
        start_frame = int(clip_start_sec * decord_vr.get_avg_fps())
        end_frame = min(total_frames,
                        int(clip_end_sec * decord_vr.get_avg_fps())) if clip_end_sec is not None else total_frames
        frame_id_list = np.linspace(start_frame, end_frame - 1, num_frames, dtype=int)
    elif strategy == 'chat':
        timestamps = decord_vr.get_frame_timestamp(np.arange(total_frames))
        timestamps = [i[0] for i in timestamps]
        max_second = round(max(timestamps)) + 1
        frame_id_list = []
        for second in range(max_second):
            closest_num = min(timestamps, key=lambda x: abs(x - second))
            index = timestamps.index(closest_num)
            frame_id_list.append(index)
            if len(frame_id_list) >= num_frames:
                break
    video_data = decord_vr.get_batch(frame_id_list)
    video_data = video_data.permute(3, 0, 1, 2)
    return video_data
```

#### 模型加载

+ 使用 `AutoTokenizer` 从指定的 `MODEL_PATH` 加载分词器。

+ 根据 GPU 内存大小和量化设置，决定如何加载模型。如果 GPU 内存小于 `48GB` 且未启用量化，则会提示用户并退出。

+ 根据 `--quant` 参数选择是否加载量化模型（4 位或 8 位），或者加载标准模型。

```
tokenizer = AutoTokenizer.from_pretrained(
    MODEL_PATH,
    trust_remote_code=True,
)

if torch.cuda.is_available() and torch.cuda.get_device_properties(0).total_memory < 48 * 1024 ** 3 and not args.quant:
    print("GPU 内存少于 48GB。请使用 cli_demo_multi_gpus.py 或传递 `--quant 4` 或 `--quant 8`。")
    exit()

if args.quant == 4:
    model = AutoModelForCausalLM.from_pretrained(
        MODEL_PATH,
        torch_dtype=TORCH_TYPE,
        trust_remote_code=True,
        quantization_config=BitsAndBytesConfig(
            load_in_4bit=True,
            bnb_4bit_compute_dtype=TORCH_TYPE,
        ),
        low_cpu_mem_usage=True
    ).eval()
elif args.quant == 8:
    model = AutoModelForCausalLM.from_pretrained(
        MODEL_PATH,
        torch_dtype=TORCH_TYPE,
        trust_remote_code=True,
        quantization_config=BitsAndBytesConfig(
            load_in_8bit=True,
            bnb_4bit_compute_dtype=TORCH_TYPE,
        ),
        low_cpu_mem_usage=True
    ).eval()
else:
    model = AutoModelForCausalLM.from_pretrained(
        MODEL_PATH,
        torch_dtype=TORCH_TYPE,
        trust_remote_code=True
    ).eval().to(DEVICE)
```

#### 执行生成

根据 `MODEL_PATH` 判断使用的模型策略（`base` 或 `chat`）。脚本会提示用户输入视频路径。如果用户没有输入视频路径，则脚本会进入纯文本对话模式。如果用户提供了视频路径，则调用 `load_video` 函数来加载视频。之后，脚本进入一个子循环以处理用户的查询。在这个子循环中，脚本调用 `model.build_conversation_input_ids()` 函数来准备模型的输入，这些输入包括用户的查询、视频帧和对话历史。接着，脚本配置生成参数，比如最大新令牌数和采样方法等。使用配置好的模型生成响应后，脚本将生成的响应解码为文本，并打印出来，同时将当前对话添加到历史记录中。

```
while True:
    strategy = 'base' if 'cogvlm2-video-llama3-base' in MODEL_PATH else 'chat'
    print(f"使用 {strategy} 模型")
    video_path = input("视频路径 >>>>> ")
    if video_path == '':
        print('您没有输入视频路径，接下来的将是纯文本对话。')
        video = None
    else:
        video = load_video(video_path, strategy=strategy)

    history = []
    while True:
        query = input("人类:")
        if query == "clear":
            break

        inputs = model.build_conversation_input_ids(
            tokenizer=tokenizer,
            query=query,
            images=[video],
            history=history,
            template_version=strategy
        )

        inputs = {
            'input_ids': inputs['input_ids'].unsqueeze(0).to(DEVICE),
            'token_type_ids': inputs['token_type_ids'].unsqueeze(0).to(DEVICE),
            'attention_mask': inputs['attention_mask'].unsqueeze(0).to(DEVICE),
            'images': [[inputs['images'][0].to('cuda').to(TORCH_TYPE)]],
        }
        gen_kwargs = {
            "max_new_tokens": 2048,
            "pad_token_id": 128002,
            "top_k": 1,
            "do_sample": True,
            "top_p": 0.1,
            "temperature": 0.1,
        }
        with torch.no_grad():
            outputs = model.generate(**inputs, **gen_kwargs)
            outputs = outputs[:, inputs['input_ids'].shape[1]:]
            response = tokenizer.decode(outputs[0], skip_special_tokens=True)
            print("\nCogVLM2-Video:", response)
        history.append((query, response))
```
