# 用 Streamlit 网页应用生成视频

> 来源：[https://ai.feishu.cn/docx/UZnvdAWiroZsNxxWRtqc07iznsg](https://ai.feishu.cn/docx/UZnvdAWiroZsNxxWRtqc07iznsg)

## 用 Streamlit 运行

设置 OPENAI_API_KEY 的环境变量。您应使用 OpenAI 或 ZhiupAI 的实际 API 密钥替换 OpenAI 密钥或 ZhiupAI 密钥。然后，设置 OPENAI_BASE_URL 的环境变量，从而指定 API 的基本 URL。在本例中，提供的 URL 是 ZhiupAI，如果使用的是 OpenAI，则无需设置此变量或将其设置为 OpenAI 的基本 URL。最后，运行 Streamlit 应用程序。

```
$ export OPENAI_API_KEY=your OpenAI Key or ZhiupAI Key
$ export OPENAI_BASE_URL=https://open.bigmodel.cn/api/paas/v4/  # include when using ZhipuAI, not when using OpenAI
$ streamlit run streamlit_web_demo.py
```

## 加载模型

从 CogVideoXPipeline 加载本地的 CogVideoX 模型。

**参数：**

model_path： 预训练 VAE 模型的文件路径。

dtype： 计算数据类型，默认使用 torch.float16（半精度），可以减少内存使用并加快计算速度。

device： 计算设备，GPU 为 "cuda"，CPU 为 "cpu"。

**返回值：**CogVideoXPipeline

```
import base64
import json
import os
import time
from datetime import datetime
from typing import List

import imageio
import numpy as np
import streamlit as st
import torch
from convert_demo import convert_prompt
from diffusers import CogVideoXPipeline


model_path: str = &quot;THUDM/CogVideoX-5b&quot;


@st.cache_resource
def load_model(model_path: str, dtype: torch.dtype, device: str) -&gt; CogVideoXPipeline:
    &quot;&quot;&quot;
    Load the CogVideoX model.

    Args:
    - model_path (str): Path to the model.
    - dtype (torch.dtype): Data type for model.
    - device (str): Device to load the model on.

    Returns:
    - CogVideoXPipeline: Loaded model pipeline.
    &quot;&quot;&quot;
    pipe = CogVideoXPipeline.from_pretrained(model_path, torch_dtype=dtype)
    pipe.enable_model_cpu_offload()
    return pipe
```

请注意，您需要在这将THUDM/CogVideoX-5b 替换您自己的本地路径。

## 生成视频

根据给定的文本提示生成视频帧。

**参数：**

Pipeline： 生成视频的模型Pipeline。

prompt： 生成视频的文本提示。

num_inference_steps： 模型推理的步数。

guidance_scale： 生成过程中用于引导的刻度。

num_videos_per_prompt： 为每个提示生成的视频数量。

device and dtype： 用于处理的设备和数据类型。

**返回值 ：**NumPy 数组形式的视频帧列表。

```
def generate_video(
    pipe: CogVideoXPipeline,
    prompt: str,
    num_inference_steps: int = 50,
    guidance_scale: float = 6.0,
    num_videos_per_prompt: int = 1,
    device: str = &quot;cuda&quot;,
    dtype: torch.dtype = torch.float16,
) -&gt; List[np.ndarray]:
    &quot;&quot;&quot;
    Generate a video based on the provided prompt and model path.

    Args:
    - pipe (CogVideoXPipeline): The pipeline for generating videos.
    - prompt (str): Text prompt for video generation.
    - num_inference_steps (int): Number of inference steps.
    - guidance_scale (float): Guidance scale for generation.
    - num_videos_per_prompt (int): Number of videos to generate per prompt.
    - device (str): Device to run the generation on.
    - dtype (torch.dtype): Data type for the model.

    Returns:
    - List[np.ndarray]: Generated video frames.
    &quot;&quot;&quot;
    prompt_embeds, _ = pipe.encode_prompt(
        prompt=prompt,
        negative_prompt=None,
        do_classifier_free_guidance=True,
        num_videos_per_prompt=num_videos_per_prompt,
        max_sequence_length=226,
        device=device,
        dtype=dtype,
    )
    pipe.enable_model_cpu_offload()
    # Generate video
    video = pipe(
        num_inference_steps=num_inference_steps,
        guidance_scale=guidance_scale,
        prompt_embeds=prompt_embeds,
        negative_prompt_embeds=torch.zeros_like(prompt_embeds),
    ).frames[0]
    return video
```

## 保存视频和相关元数据

将视频帧序列保存为视频文件。从列表中移除第一帧，使用指定的文件路径、帧频和编解码器初始化视频写入器，然后在关闭写入器之前将每一帧写入视频文件以最终完成。

```
def save_video(video: List[np.ndarray], path: str, fps: int = 8) -&gt; None:
    &quot;&quot;&quot;
    Save the generated video to a file.

    Args:
    - video (List[np.ndarray]): Video frames.
    - path (str): Path to save the video.
    - fps (int): Frames per second for the video.
    &quot;&quot;&quot;
    # Remove the first frame
    video = video[1:]

    writer = imageio.get_writer(path, fps=fps, codec=&quot;libx264&quot;)
    for frame in video:
        np_frame = np.array(frame)
        writer.append_data(np_frame)

    writer.close()
```

然后，将视频生成过程的元数据存储在一个 JSON 文件中。将原始和转换后的提示、推理步骤数、引导比例和生成的视频数等信息编译成字典，然后将这些数据保存到指定路径下的格式化 JSON 文件中。

```
def save_metadata(
    prompt: str,
    converted_prompt: str,
    num_inference_steps: int,
    guidance_scale: float,
    num_videos_per_prompt: int,
    path: str,
) -&gt; None:
    &quot;&quot;&quot;
    Save metadata to a JSON file.
    
    
    Args:
    - prompt (str): Original prompt.
    - converted_prompt (str): Converted prompt.
    - num_inference_steps (int): Number of inference steps.
    - guidance_scale (float): Guidance scale.
    - num_videos_per_prompt (int): Number of videos per prompt.
    - path (str): Path to save the metadata.
    &quot;&quot;&quot;
    metadata = {
        &quot;prompt&quot;: prompt,
        &quot;converted_prompt&quot;: converted_prompt,
        &quot;num_inference_steps&quot;: num_inference_steps,
        &quot;guidance_scale&quot;: guidance_scale,
        &quot;num_videos_per_prompt&quot;: num_videos_per_prompt,
    }
    with open(path, &quot;w&quot;) as f:
        json.dump(metadata, f, indent=4)
```

## 用 Streamlit 调用函数并执行

配置界面，加载 CogVideoX 模型，并在侧边栏设置用户输入的视频生成参数。处理提示输入、完善输入并根据用户设置生成视频。生成的视频显示在应用程序中，并在侧边栏提供下载链接，方便用户访问。

```
def main() -&gt; None:
    &quot;&quot;&quot;
    Main function to run the Streamlit web application.
    &quot;&quot;&quot;
    st.set_page_config(page_title=&quot;CogVideoX-Demo&quot;, page_icon=&quot;🎥&quot;, layout=&quot;wide&quot;)
    st.write(&quot;# CogVideoX 🎥&quot;)
    dtype: torch.dtype = torch.float16
    device: str = &quot;cuda&quot;

    global pipe
    pipe = load_model(model_path, dtype, device)

    with st.sidebar:
        st.info(&quot;It will take some time to generate a video (~90 seconds per video in 50 steps).&quot;, icon=&quot;ℹ️&quot;)
        num_inference_steps: int = st.number_input(&quot;Inference Steps&quot;, min_value=1, max_value=100, value=50)
        guidance_scale: float = st.number_input(&quot;Guidance Scale&quot;, min_value=0.0, max_value=20.0, value=6.0)
        num_videos_per_prompt: int = st.number_input(&quot;Videos per Prompt&quot;, min_value=1, max_value=10, value=1)

        share_links_container = st.empty()

    prompt: str = st.chat_input(&quot;Prompt&quot;)
    if prompt:
        # Not Necessary, Suggestions
        with st.spinner(&quot;Refining prompts...&quot;):
            converted_prompt = convert_prompt(prompt=prompt, retry_times=1)
            if converted_prompt is None:
                st.error(&quot;Failed to refine the prompt, using the original one.&quot;)

        st.info(f&quot;**Original prompt:**  \n{prompt}  \n  \n**Converted prompt:**  \n{converted_prompt}&quot;)
        torch.cuda.empty_cache()

        with st.spinner(&quot;Generating Video...&quot;):
            start_time = time.time()
            video_paths = []

            timestamp = datetime.now().strftime(&quot;%Y%m%d_%H%M%S&quot;)
            output_dir = f&quot;./output/{timestamp}&quot;
            os.makedirs(output_dir, exist_ok=True)

            metadata_path = os.path.join(output_dir, &quot;config.json&quot;)
            save_metadata(
                prompt, converted_prompt, num_inference_steps, guidance_scale, num_videos_per_prompt, metadata_path
            )

            for i in range(num_videos_per_prompt):
                video_path = os.path.join(output_dir, f&quot;output_{i + 1}.mp4&quot;)

                video = generate_video(
                    pipe, converted_prompt or prompt, num_inference_steps, guidance_scale, 1, device, dtype
                )
                save_video(video, video_path, fps=8)
                video_paths.append(video_path)
                
                with open(video_path, &quot;rb&quot;) as video_file:
                    video_bytes: bytes = video_file.read()
                    st.video(video_bytes, autoplay=True, loop=True, format=&quot;video/mp4&quot;)
                torch.cuda.empty_cache()

            used_time: float = time.time() - start_time
            st.success(f&quot;Videos generated in {used_time:.2f} seconds.&quot;)

            # Create download links in the sidebar
            with share_links_container:
                st.sidebar.write(&quot;### Download Links:&quot;)
                for video_path in video_paths:
                    video_name = os.path.basename(video_path)
                    with open(video_path, &quot;rb&quot;) as f:
                        video_bytes: bytes = f.read()
                    b64_video = base64.b64encode(video_bytes).decode()
                    href = f&#x27;&lt;a href=&quot;data:video/mp4;base64,{b64_video}&quot; download=&quot;{video_name}&quot;&gt;Download {video_name}&lt;/a&gt;&#x27;
                    st.sidebar.markdown(href, unsafe_allow_html=True)


if __name__ == &quot;__main__&quot;:
    main()
```