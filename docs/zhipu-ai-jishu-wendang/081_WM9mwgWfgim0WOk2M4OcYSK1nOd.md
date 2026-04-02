# 用 Diffusers 进行视频生成推理

> 来源：[https://ai.feishu.cn/docx/ILzCd4GjVoyBZdxidVccr6CqnKf](https://ai.feishu.cn/docx/ILzCd4GjVoyBZdxidVccr6CqnKf)

使用 Huggingface Diffusers Pipeline 生成视频，并保存为 MP4 格式。

在开始之前，确认diffusers>=0.30.1已安装，并且在diffusers 0.31.0 发布后需更新。

```
pip install diffusers&gt;=0.30.1
```

# 使用 CLI 运行推理

用命令行界面（CLI）运行推理脚本，从文本提示中生成视频：

```
$ python cli_demo.py --prompt &quot;A girl riding a bike.&quot; --model_path THUDM/CogVideoX-5b
```

请注意，您需要在这将THUDM/CogVideoX-5b 替换您自己的本地路径。

# 生成视频

根据文本提示生成视频的核心函数，使用num_inference_steps，guidance_scale等参数来控制视频生成过程。在加载好 CogVideoXPipeline 后，编码Prompt并且根据它生成一系列视频的画面。

**参数：**

prompt：生成视频的提示词

model_path： 预训练模型路径

output_path： 生成视频保存路径

num_inference_steps： 模型在推理过程中的步数，步数越多，生成的视频质量越好，但是计算时间更长。

guidance_scale ： 用于控制视频生成过程中使用的指导强度，较高的值会使生成的视频更贴近所提供的提示，更加逼真度，但降低了视频的创造性。

num_videos_per_prompt： 为每个prompt生成的视频数量，可以根据一个文本提示生成多个相似的视频。

dtype： 计算数据类型，默认使用 torch.float16（半精度）以减少内存使用并加快计算速度。

```
import argparse
import torch
from diffusers import CogVideoXPipeline, CogVideoXDDIMScheduler, CogVideoXDPMScheduler
from diffusers.utils import export_to_video


def generate_video(
    prompt: str,
    model_path: str,
    output_path: str = &quot;./output.mp4&quot;,
    num_inference_steps: int = 50,
    guidance_scale: float = 6.0,
    num_videos_per_prompt: int = 1,
    dtype: torch.dtype = torch.bfloat16,
):
    &quot;&quot;&quot;
    Generates a video based on the given prompt and saves it to the specified path.

    Parameters:
    - prompt (str): The description of the video to be generated.
    - model_path (str): The path of the pre-trained model to be used.
    - output_path (str): The path where the generated video will be saved.
    - num_inference_steps (int): Number of steps for the inference process. More steps can result in better quality.
    - guidance_scale (float): The scale for classifier-free guidance. Higher values can lead to better alignment with the prompt.
    - num_videos_per_prompt (int): Number of videos to generate per prompt.
    - dtype (torch.dtype): The data type for computation (default is torch.bfloat16).

    &quot;&quot;&quot;
```

## 加载预训练模型

以指定精度（bfloat16）加载预训练的 CogVideoXPipeline。在 from_pretrained 函数中添加 device_map=“balanced”，并移除 enable_model_cpu_offload() 函数以使用多 GPU。

```
    # 1.  Load the pre-trained CogVideoX pipeline with the specified precision (bfloat16).
    # add device_map=&quot;balanced&quot; in the from_pretrained function and remove the enable_model_cpu_offload()
    # function to use Multi GPUs.

    pipe = CogVideoXPipeline.from_pretrained(model_path, torch_dtype=dtype)
```

## 配置调度程序

可更改为 CogVideoXDPMScheduler 或 CogVideoXDDIMScheduler。我们建议 CogVideoX-2B 与 CogVideoXDDIMScheduler一同使用，CogVideoX-5B 与 CogVideoXDPMScheduler一同使用。

```
    # 2. Set Scheduler.
    # Can be changed to `CogVideoXDPMScheduler` or `CogVideoXDDIMScheduler`.
    # We recommend using `CogVideoXDDIMScheduler` for CogVideoX-2B and `CogVideoXDPMScheduler` for CogVideoX-5B.
    # pipe.scheduler = CogVideoXDDIMScheduler.from_config(pipe.scheduler.config, timestep_spacing=&quot;trailing&quot;)
    pipe.scheduler = CogVideoXDPMScheduler.from_config(pipe.scheduler.config, timestep_spacing=&quot;trailing&quot;)

```

## 启用 CPU 卸载和平铺

如果您有多个 GPU 或足够的 GPU 内存（如 H100），请将其关闭，这样会减少推理所需的时间。

```
    # 3. Enable CPU offload for the model, enable tiling.
    # turn off if you have multiple GPUs or enough GPU memory(such as H100) and it will cost less time in inference
    pipe.enable_model_cpu_offload()
    pipe.vae.enable_tiling()
```

## 生成视频帧

根据给定的提示词生成视频帧。默认情况下，num_frames（帧数）设置为 49，以便以 8 帧/秒的速度生成 6 秒的视频。种子的使用可确保再次生成。

```
    # 4. Generate the video frames based on the prompt.
    # `num_frames` is the Number of frames to generate.
    # This is the default value for 6 seconds video and 8 fps,so 48 frames and will plus 1 frame for the first frame.
    # for diffusers `0.30.1` and after version, this should be 49.
    video = pipe(
        prompt=prompt,
        num_videos_per_prompt=num_videos_per_prompt,  # Number of videos to generate per prompt
        num_inference_steps=num_inference_steps,  # Number of inference steps
        num_frames=49,  # Number of frames to generate，changed to 49 for diffusers version `0.31.0` and after.
        use_dynamic_cfg=True,  ## This id used for DPM Sechduler, for DDIM scheduler, it should be False
        guidance_scale=guidance_scale,  # Guidance scale for classifier-free guidance, can set to 7 for DPM scheduler
        generator=torch.Generator().manual_seed(42),  # Set the seed for reproducibility
    ).frames[0]
```

## 导出为视频

导出为视频文件，原始视频的 fps 为 8。

```
    # 5. Export the generated frames to a video file. fps must be 8 for original video.
    export_to_video(video, output_path, fps=8)
```

## 执行视频生成

最终，使用 argparse 获取用户提供的参数值，解析命令行参数。

```
if __name__ == &quot;__main__&quot;:
    parser = argparse.ArgumentParser(description=&quot;Generate a video from a text prompt using CogVideoX&quot;)
    parser.add_argument(&quot;--prompt&quot;, type=str, required=True, help=&quot;The description of the video to be generated&quot;)
    parser.add_argument(
        &quot;--model_path&quot;, type=str, default=&quot;THUDM/CogVideoX-5b&quot;, help=&quot;The path of the pre-trained model to be used&quot;
    )
    parser.add_argument(
        &quot;--output_path&quot;, type=str, default=&quot;./output.mp4&quot;, help=&quot;The path where the generated video will be saved&quot;
    )
    parser.add_argument(
        &quot;--num_inference_steps&quot;, type=int, default=50, help=&quot;Number of steps for the inference process&quot;
    )
    parser.add_argument(&quot;--guidance_scale&quot;, type=float, default=6.0, help=&quot;The scale for classifier-free guidance&quot;)
    parser.add_argument(&quot;--num_videos_per_prompt&quot;, type=int, default=1, help=&quot;Number of videos to generate per prompt&quot;)
    parser.add_argument(
        &quot;--dtype&quot;, type=str, default=&quot;bfloat16&quot;, help=&quot;The data type for computation (e.g., &#x27;float16&#x27; or &#x27;bfloat16&#x27;)&quot;
    )

    args = parser.parse_args()

    dtype = torch.float16 if args.dtype == &quot;float16&quot; else torch.bfloat16

    generate_video(
        prompt=args.prompt,
        model_path=args.model_path,
        output_path=args.output_path,
        num_inference_steps=args.num_inference_steps,
        guidance_scale=args.guidance_scale,
        num_videos_per_prompt=args.num_videos_per_prompt,
        dtype=dtype,
    )
```