
# 文生视频量化版本

> 来源：[https://ai.feishu.cn/docx/VhxmdarTWonePsxFbi3cJgrRngf](https://ai.feishu.cn/docx/VhxmdarTWonePsxFbi3cJgrRngf)

这篇内容使用 `CogVideoX` 模型从文本提示生成视频，并增加量化的选项。量化可减少模型的内存占用和计算需求。

在开始之前，必须从源代码中安装 `torchao`、`torch`、`diffusers`、`accelerate` 库。只有 `H100` 或更高版本的英伟达 GPU 才支持 `FP-8` 量化。所有量化方案都必须使用英伟达™（NVIDIA®）图形处理器。

## 运行推理

```
python cli_demo_quantization.py --prompt "A girl riding a bike." --model_path THUDM/CogVideoX-5b --quantization_scheme fp8 --dtype bfloat16
```

请注意，您需要在这将 `THUDM/CogVideoX-5b` 替换您自己的本地路径。

## 配置环境

导入要用到的库，并且配置 `PyTorch` 的优化和调试设置。

```
import argparse
import os
import torch
import torch._dynamo
from diffusers import AutoencoderKLCogVideoX, CogVideoXTransformer3DModel, CogVideoXPipeline, CogVideoXDPMScheduler
from diffusers.utils import export_to_video
from transformers import T5EncoderModel
from torchao.quantization import quantize_, int8_weight_only
from torchao.float8.inference import ActivationCasting, QuantConfig, quantize_to_float8

os.environ["TORCH_LOGS"] = "+dynamo,output_code,graph_breaks,recompiles"
torch._dynamo.config.suppress_errors = True
torch.set_float32_matmul_precision("high")
torch._inductor.config.conv_1x1_as_mm = True
torch._inductor.config.coordinate_descent_tuning = True
torch._inductor.config.epilogue_fusion = False
torch._inductor.config.coordinate_descent_check_all_directions = True
```

## 量化模型

接收一个模型组件（部件）和一个量化方案，然后应用相应的量化技术：

`int8`：应用 8 位整数量化。

`fp8`：应用 8 位浮点量化。

```
def quantize_model(part, quantization_scheme):
    if quantization_scheme == "int8":
        quantize_(part, int8_weight_only())
    elif quantization_scheme == "fp8":
        quantize_to_float8(part, QuantConfig(ActivationCasting.DYNAMIC))
    return part
```

## 生成视频

从指定路径加载 `CogVideoX` 模型的文本编码器、变换器和 `VAE`（变异自动编码器）组件，将选择的量化方案应用到每个模型组件，然后用量化模型初始化 `CogVideoXPipeline`。使用 `CogVideoXPipeline` 根据提供的文本提示生成视频，并为推理步骤、引导比例和帧数指定参数。最后，将生成的视频导出到指定的文件路径。

+ 如果选择使用`pipe.transformer.to(memory_format=torch.channels_last)`将模型的张量存储器格式转换为转换器组件的 `channels_last` 可以提高性能，尤其是卷积操作的性能。首次使用时，编译优化代码可能需要大量时间（约 30 分钟）。这一编译步骤是一次性成本，但可以在后续运行中加快推理速度。

+ 使用 `FP8` 时，删掉 `pipe.enable_model_cpu_offload()`。

+ 使用 `DPM Sechduler` 时，`use_dynamic_cfg=True`。使用 `DDIM scheduler` 时，设定为 `False`。

```
def generate_video(
    prompt: str,
    model_path: str,
    output_path: str = "./output.mp4",
    num_inference_steps: int = 50,
    guidance_scale: float = 6.0,
    num_videos_per_prompt: int = 1,
    quantization_scheme: str = "fp8",
    dtype: torch.dtype = torch.bfloat16,
):
    """
    Generates a video based on the given prompt and saves it to the specified path.

    Parameters:
    - prompt (str): The description of the video to be generated.
    - model_path (str): The path of the pre-trained model to be used.
    - output_path (str): The path where the generated video will be saved.
    - num_inference_steps (int): Number of steps for the inference process. More steps can result in better quality.
    - guidance_scale (float): The scale for classifier-free guidance. Higher values can lead to better alignment with the prompt.
    - num_videos_per_prompt (int): Number of videos to generate per prompt.
    - quantization_scheme (str): The quantization scheme to use ('int8', 'fp8').
    - dtype (torch.dtype): The data type for computation (default is torch.bfloat16).
    """

    text_encoder = T5EncoderModel.from_pretrained(model_path, subfolder="text_encoder", torch_dtype=dtype)
    text_encoder = quantize_model(part=text_encoder, quantization_scheme=quantization_scheme)
    transformer = CogVideoXTransformer3DModel.from_pretrained(model_path, subfolder="transformer", torch_dtype=dtype)
    transformer = quantize_model(part=transformer, quantization_scheme=quantization_scheme)
    vae = AutoencoderKLCogVideoX.from_pretrained(model_path, subfolder="vae", torch_dtype=dtype)
    vae = quantize_model(part=vae, quantization_scheme=quantization_scheme)
    pipe = CogVideoXPipeline.from_pretrained(
        model_path,
        text_encoder=text_encoder,
        transformer=transformer,
        vae=vae,
        torch_dtype=dtype,
    ).to("cuda")
    pipe.scheduler = CogVideoXDPMScheduler.from_config(pipe.scheduler.config, timestep_spacing="trailing")

    # Using with compile will run faster. First time infer will cost ~30min to compile.
    # pipe.transformer.to(memory_format=torch.channels_last)
    # for FP8 should remove  pipe.enable_model_cpu_offload()
    pipe.enable_model_cpu_offload()
    pipe.vae.enable_tiling()
    video = pipe(
        prompt=prompt,
        num_videos_per_prompt=num_videos_per_prompt,
        num_inference_steps=num_inference_steps,
        num_frames=49,
        use_dynamic_cfg=True,  ## This id used for DPM Sechduler, for DDIM scheduler, it should be False
        guidance_scale=guidance_scale,
        generator=torch.Generator(device="cuda").manual_seed(42),
    ).frames[0]

    export_to_video(video, output_path, fps=8)
```

## 执行生成

定义脚本的命令行参数，包括提示符、模型路径、输出路径、推理步骤、引导比例、视频数量、`dtype` 和量化方案。并且调用`generate_video`制作并保存视频。

```
if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Generate a video from a text prompt using CogVideoX")
    parser.add_argument("--prompt", type=str, required=True, help="The description of the video to be generated")
    parser.add_argument(
        "--model_path", type=str, default="THUDM/CogVideoX-5b", help="The path of the pre-trained model to be used"
    )
    parser.add_argument(
        "--output_path", type=str, default="./output.mp4", help="The path where the generated video will be saved"
    )
    parser.add_argument(
        "--num_inference_steps", type=int, default=50, help="Number of steps for the inference process"
    )
    parser.add_argument("--guidance_scale", type=float, default=6.0, help="The scale for classifier-free guidance")
    parser.add_argument("--num_videos_per_prompt", type=int, default=1, help="Number of videos to generate per prompt")
    parser.add_argument(
        "--dtype", type=str, default="bfloat16", help="The data type for computation (e.g., 'float16', 'bfloat16')"
    )
    parser.add_argument(
        "--quantization_scheme",
        type=str,
        default="bf16",
        choices=["int8", "fp8"],
        help="The quantization scheme to use (int8, fp8)",
    )

    args = parser.parse_args()
    dtype = torch.float16 if args.dtype == "float16" else torch.bfloat16
    generate_video(
        prompt=args.prompt,
        model_path=args.model_path,
        output_path=args.output_path,
        num_inference_steps=args.num_inference_steps,
        guidance_scale=args.guidance_scale,
        num_videos_per_prompt=args.num_videos_per_prompt,
        quantization_scheme=args.quantization_scheme,
        dtype=dtype,
    )
```
