# 用 Diffusers VAE 模型进行视频编码和解码

> 来源：[https://ai.feishu.cn/docx/OzkpdQVKloXdeHxdgHWctAGunYb](https://ai.feishu.cn/docx/OzkpdQVKloXdeHxdgHWctAGunYb)

使用 CogVideoX-5b VAE 模型可以将视频编码为潜在表示，或者将其解码回视频，或者编码并解码。在运行此脚本之前，请确保克隆 CogVideoX 的 Hugging Face 模型仓库，并将 {your local diffusers path} 参数设置为克隆的仓库路径。

**编码：**

视频路径：../resources/videos/1.mp4

内存：~34GB

```
python cli_vae_demo.py --model_path {your local diffusers path}/CogVideoX-5b/vae/ --video_path ../resources/videos/1.mp4 --mode encode
```

如果您的 GPU 内存不足，我们在资源文件夹中提供了一个预编码的张量文件（encoded.pt），您仍然可以运行解码命令。

**解码：**

内存：~19GB

```
python cli_vae_demo.py --model_path {your local diffusers path}/CogVideoX-5b/vae/ --encoded_path ./encoded.pt --mode decode
```

**编码并解码：**

视频路径：../resources/videos/1.mp4

内存：34GB+19GB

```
python cli_vae_demo.py --model_path {your local diffusers path}/CogVideoX-5b/vae/ --video_path ../resources/videos/1.mp4 --mode both
```

## 导入和编码视频

从 CogVideoX Pipeline 加载预先训练好的 VAE 模型，用模型将视频文件编码成潜在空间表示法（latent representation）。这种编码表示是视频的压缩形式能捕捉视频帧的基本特征。

**参数：**

model_path： 预训练 VAE 模型的文件路径。

video_path： 视频文件路径，在编码时处理每一帧，并将其编码为一个潜空间。

dtype： 计算数据类型，默认使用 torch.float16（半精度），可以减少内存使用并加快计算速度。

device： 计算设备，GPU 为 "cuda"，CPU 为 "cpu"。

**返回值：**编码帧的 torch.Tensor 张量

```
import argparse
import torch
import imageio
import numpy as np
from diffusers import AutoencoderKLCogVideoX
from torchvision import transforms

def encode_video(model_path, video_path, dtype, device):
    &quot;&quot;&quot;
    Loads a pre-trained AutoencoderKLCogVideoX model and encodes the video frames.

    Parameters:
    - model_path (str): The path to the pre-trained model.
    - video_path (str): The path to the video file.
    - dtype (torch.dtype): The data type for computation.
    - device (str): The device to use for computation (e.g., &quot;cuda&quot; or &quot;cpu&quot;).

    Returns:
    - torch.Tensor: The encoded video frames.
    &quot;&quot;&quot;
    model = AutoencoderKLCogVideoX.from_pretrained(model_path, torch_dtype=dtype).to(device)
    video_reader = imageio.get_reader(video_path, &quot;ffmpeg&quot;)

    frames = [transforms.ToTensor()(frame) for frame in video_reader]
    video_reader.close()

    frames_tensor = torch.stack(frames).to(device).permute(1, 0, 2, 3).unsqueeze(0).to(dtype)

    with torch.no_grad():
        encoded_frames = model.encode(frames_tensor)[0].sample()
    return encoded_frames
```

## 解码视频

使 VAE 模型，将先前编码的视频张量解码回原始的视频帧，将压缩的潜在表示转换回视频帧的张量。

**部分参数：**

encoded_tensor_path： 编码张量的文件路径，其中包含视频的潜在表示。

**返回值：**解码后视频帧的 torch.Tensor 张量

```
def decode_video(model_path, encoded_tensor_path, dtype, device):
    &quot;&quot;&quot;
    Loads a pre-trained AutoencoderKLCogVideoX model and decodes the encoded video frames.

    Parameters:
    - model_path (str): The path to the pre-trained model.
    - encoded_tensor_path (str): The path to the encoded tensor file.
    - dtype (torch.dtype): The data type for computation.
    - device (str): The device to use for computation (e.g., &quot;cuda&quot; or &quot;cpu&quot;).

    Returns:
    - torch.Tensor: The decoded video frames.
    &quot;&quot;&quot;
    model = AutoencoderKLCogVideoX.from_pretrained(model_path, torch_dtype=dtype).to(device)
    encoded_frames = torch.load(encoded_tensor_path, weights_only=True).to(device).to(dtype)
    with torch.no_grad():
        decoded_frames = []
        for i in range(6):  # 6 seconds
            start_frame, end_frame = (0, 3) if i == 0 else (2 * i + 1, 2 * i + 3)
            current_frames = model.decode(encoded_frames[:, :, start_frame:end_frame]).sample
            decoded_frames.append(current_frames)
        model.clear_fake_context_parallel_cache()

        decoded_frames = torch.cat(decoded_frames, dim=2)
    return decoded_frames
```

## 保存视频

获取包含视频帧的张量，处理张量以确保其保存格式正确，然后使用 imageio 库将帧写入视频文件，保存为指定输出路径下的视频文件。

**参数：**

tensor：包含视频帧的输入张量，每个帧都以多维数组的形式存储在该张量中。

output_path： 保存视频的文件路径，将 "output.mp4 "添加到此路径中以创建完整的文件名。

```
def save_video(tensor, output_path):
    &quot;&quot;&quot;
    Saves the video frames to a video file.

    Parameters:
    - tensor (torch.Tensor): The video frames tensor.
    - output_path (str): The path to save the output video.
    &quot;&quot;&quot;
    frames = tensor[0].squeeze(0).permute(1, 2, 3, 0).cpu().numpy()
    frames = np.clip(frames, 0, 1) * 255
    frames = frames.astype(np.uint8)

    writer = imageio.get_writer(output_path + &quot;/output.mp4&quot;, fps=30)
    for frame in frames:
        writer.append_data(frame)
    writer.close()
```

## 解析CLI参数和函数调用

主函数使用 argparse 模块创建命令行界面，允许用户指定各种选项，如模式、模型和视频文件的路径以及计算设置。

用户可以选择的mode有三种：encode（编码）、decode（解码）、both（编码并解码）

```
if __name__ == &quot;__main__&quot;:
    parser = argparse.ArgumentParser(description=&quot;CogVideoX encode/decode demo&quot;)
    parser.add_argument(&quot;--model_path&quot;, type=str, required=True, help=&quot;The path to the CogVideoX model&quot;)
    parser.add_argument(&quot;--video_path&quot;, type=str, help=&quot;The path to the video file (for encoding)&quot;)
    parser.add_argument(&quot;--encoded_path&quot;, type=str, help=&quot;The path to the encoded tensor file (for decoding)&quot;)
    parser.add_argument(&quot;--output_path&quot;, type=str, default=&quot;.&quot;, help=&quot;The path to save the output file&quot;)
    parser.add_argument(
        &quot;--mode&quot;, type=str, choices=[&quot;encode&quot;, &quot;decode&quot;, &quot;both&quot;], required=True, help=&quot;Mode: encode, decode, or both&quot;
    )
    parser.add_argument(
        &quot;--dtype&quot;, type=str, default=&quot;float16&quot;, help=&quot;The data type for computation (e.g., &#x27;float16&#x27; or &#x27;float32&#x27;)&quot;
    )
    parser.add_argument(
        &quot;--device&quot;, type=str, default=&quot;cuda&quot;, help=&quot;The device to use for computation (e.g., &#x27;cuda&#x27; or &#x27;cpu&#x27;)&quot;
    )
    args = parser.parse_args()

    device = torch.device(args.device)
    dtype = torch.float16 if args.dtype == &quot;float16&quot; else torch.float32

    if args.mode == &quot;encode&quot;:
        assert args.video_path, &quot;Video path must be provided for encoding.&quot;
        encoded_output = encode_video(args.model_path, args.video_path, dtype, device)
        torch.save(encoded_output, args.output_path + &quot;/encoded.pt&quot;)
        print(f&quot;Finished encoding the video to a tensor, save it to a file at {encoded_output}/encoded.pt&quot;)
    elif args.mode == &quot;decode&quot;:
        assert args.encoded_path, &quot;Encoded tensor path must be provided for decoding.&quot;
        decoded_output = decode_video(args.model_path, args.encoded_path, dtype, device)
        save_video(decoded_output, args.output_path)
        print(f&quot;Finished decoding the video and saved it to a file at {args.output_path}/output.mp4&quot;)
    elif args.mode == &quot;both&quot;:
        assert args.video_path, &quot;Video path must be provided for encoding.&quot;
        encoded_output = encode_video(args.model_path, args.video_path, dtype, device)
        torch.save(encoded_output, args.output_path + &quot;/encoded.pt&quot;)
        decoded_output = decode_video(args.model_path, args.output_path + &quot;/encoded.pt&quot;, dtype, device)
        save_video(decoded_output, args.output_path)
```