# Gradio 网页界面生成视频

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Q6GodXPG0ouMsUxqw3TciEZMnFe](https://v11enp9ok1h.feishu.cn/docx/Q6GodXPG0ouMsUxqw3TciEZMnFe)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

这是一个简单的 Gradio 网络应用程序，您可以直接在浏览器中使用 CogVideoX-5B 模型生成视频。与我们的 Huggingface Space 类似，您可以使用该脚本运行一个简单的网络应用程序来生成视频。

# 设置环境变量

```
OPENAI_API_KEY = your_api_key
OPENAI_BASE_URL= your_base_url
GRADIO_TEMP_DIR= gradio_tmp
```

# 安装依赖

```
pip install -r requirements.txt
```

# 主脚本

gradio_composite_demo/app.py为 Gradio 网页演示的主脚本，使用以下命令运行

```
OpenAI_API_KEY=your_openai_api_key OpenAI_BASE_URL=https://api.openai.com/v1 python inference/gradio_web_demo.py
```

## 导入和设置

检查 GPU 是否可用（"cuda"），否则会退回到使用 CPU（"cpu"）。下载 Real-ESRGAN 模型用于升频，下载 RIFE 模型用于使用 Hugging Face Hub 进行帧插值。使用预训练模型初始化 CogVideoXPipeline，为其配置调度程序。然后将模型的张量存储器格式转换为 channels_last，并应用 PyTorch 的 torch.compile 优化变换器组件，以获得更好的性能。最后，从各自的文件中加载缩放模型和帧插值模型，并且指定初始 prompt。

```
import math
import os
import random
import threading
import time

import gradio as gr
import torch
from diffusers import CogVideoXPipeline, CogVideoXDDIMScheduler, CogVideoXDPMScheduler
from datetime import datetime, timedelta

from diffusers.image_processor import VaeImageProcessor
from openai import OpenAI
import moviepy.editor as mp
import utils
from rife_model import load_rife_model, rife_inference_with_latents
from huggingface_hub import hf_hub_download, snapshot_download

device = &quot;cuda&quot; if torch.cuda.is_available() else &quot;cpu&quot;

hf_hub_download(repo_id=&quot;ai-forever/Real-ESRGAN&quot;, filename=&quot;RealESRGAN_x4.pth&quot;, local_dir=&quot;model_real_esran&quot;)
snapshot_download(repo_id=&quot;AlexWortega/RIFE&quot;, local_dir=&quot;model_rife&quot;)

pipe = CogVideoXPipeline.from_pretrained(&quot;THUDM/CogVideoX-5b&quot;, torch_dtype=torch.bfloat16).to(device)
pipe.scheduler = CogVideoXDPMScheduler.from_config(pipe.scheduler.config, timestep_spacing=&quot;trailing&quot;)

pipe.transformer.to(memory_format=torch.channels_last)
pipe.transformer = torch.compile(pipe.transformer, mode=&quot;max-autotune&quot;, fullgraph=True)

os.makedirs(&quot;./output&quot;, exist_ok=True)
os.makedirs(&quot;./gradio_tmp&quot;, exist_ok=True)

upscale_model = utils.load_sd_upscale(&quot;model_real_esran/RealESRGAN_x4.pth&quot;, device)
frame_interpolation_model = load_rife_model(&quot;model_rife&quot;)

sys_prompt = &quot;&quot;&quot;You are part of a team of bots that creates videos. You work with an assistant bot that will draw anything you say in square brackets.

For example , outputting &quot; a beautiful morning in the woods with the sun peaking through the trees &quot; will trigger your partner bot to output an video of a forest morning , as described. You will be prompted by people looking to create detailed , amazing videos. The way to accomplish this is to take their short prompts and make them extremely detailed and descriptive.
There are a few rules to follow:

You will only ever output a single video description per user request.

When modifications are requested , you should not simply make the description longer . You should refactor the entire description to integrate the suggestions.
Other times the user will not want modifications , but instead want a new image . In this case , you should ignore your previous conversation with the user.

Video descriptions must have the same num of words as examples below. Extra words will be ignored.
&quot;&quot;&quot;
```

请注意，您需要在这将THUDM/CogVideoX-5b 替换您自己的本地路径。

## 转换 Prompt

输入用户的基本Prompt，使用 glm-4-0520 将其转换为适合视频生成的更详细的描述性格式。根据预定义的prompt以写好的几轮对话的例子，模型最终可以给出更加适合视频生成的prompt。

```
def convert_prompt(prompt: str, retry_times: int = 3) -&gt; str:
    if not os.environ.get(&quot;OPENAI_API_KEY&quot;):
        return prompt
    client = OpenAI()
    text = prompt.strip()

    for i in range(retry_times):
        response = client.chat.completions.create(
            messages=[
                {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: sys_prompt},
                {
                    &quot;role&quot;: &quot;user&quot;,
                    &quot;content&quot;: &#x27;Create an imaginative video descriptive caption or modify an earlier caption for the user input : &quot;a girl is on the beach&quot;&#x27;,
                },
                {
                    &quot;role&quot;: &quot;assistant&quot;,
                    &quot;content&quot;: &quot;A radiant woman stands on a deserted beach, arms outstretched, wearing a beige trench coat, white blouse, light blue jeans, and chic boots, against a backdrop of soft sky and sea. Moments later, she is seen mid-twirl, arms exuberant, with the lighting suggesting dawn or dusk. Then, she runs along the beach, her attire complemented by an off-white scarf and black ankle boots, the tranquil sea behind her. Finally, she holds a paper airplane, her pose reflecting joy and freedom, with the ocean&#x27;s gentle waves and the sky&#x27;s soft pastel hues enhancing the serene ambiance.&quot;,
                },
                {
                    &quot;role&quot;: &quot;user&quot;,
                    &quot;content&quot;: &#x27;Create an imaginative video descriptive caption or modify an earlier caption for the user input : &quot;A man jogging on a football field&quot;&#x27;,
                },
                {
                    &quot;role&quot;: &quot;assistant&quot;,
                    &quot;content&quot;: &quot;A determined man in athletic attire, including a blue long-sleeve shirt, black shorts, and blue socks, jogs around a snow-covered soccer field, showcasing his solitary exercise in a quiet, overcast setting. His long dreadlocks, focused expression, and the serene winter backdrop highlight his dedication to fitness. As he moves, his attire, consisting of a blue sports sweatshirt, black athletic pants, gloves, and sneakers, grips the snowy ground. He is seen running past a chain-link fence enclosing the playground area, with a basketball hoop and children&#x27;s slide, suggesting a moment of solitary exercise amidst the empty field.&quot;,
                },
                {
                    &quot;role&quot;: &quot;user&quot;,
                    &quot;content&quot;: &#x27;Create an imaginative video descriptive caption or modify an earlier caption for the user input : &quot; A woman is dancing, HD footage, close-up&quot;&#x27;,
                },
                {
                    &quot;role&quot;: &quot;assistant&quot;,
                    &quot;content&quot;: &quot;A young woman with her hair in an updo and wearing a teal hoodie stands against a light backdrop, initially looking over her shoulder with a contemplative expression. She then confidently makes a subtle dance move, suggesting rhythm and movement. Next, she appears poised and focused, looking directly at the camera. Her expression shifts to one of introspection as she gazes downward slightly. Finally, she dances with confidence, her left hand over her heart, symbolizing a poignant moment, all while dressed in the same teal hoodie against a plain, light-colored background.&quot;,
                },
                {
                    &quot;role&quot;: &quot;user&quot;,
                    &quot;content&quot;: f&#x27;Create an imaginative video descriptive caption or modify an earlier caption in ENGLISH for the user input: &quot;{text}&quot;&#x27;,
                },
            ],
            model=&quot;glm-4-0520&quot;,
            temperature=0.01,
            top_p=0.7,
            stream=False,
            max_tokens=200,
        )
        if response.choices:
            return response.choices[0].message.content
    return prompt
```

## 推理

使用 CogVideoXPipeline 根据文本提示生成视频。如果没有提供种子（-1），则会生成一个介于 0 和 255 之间的随机种子。如果再次使用相同的种子，这将确保生成的视频具有可重复性。

**参数：**

prompt： 描述视频内容的文本输入，也就是prompt。

num_inference_steps： 模型在推理过程中的步数。步骤越多，输出质量越高，但也需要更多的计算时间。

guidance_scale： 用于控制模型输出在多大程度上遵循所提供的提示的系数。较高的值通常会产生更符合提示的结果。

seed：用于生成随机数的种子。如果为-1，则使用随机种子。

progress（optional）： 用于在推理过程中跟踪进度，与 Gradio 的进度跟踪工具配合使用。

**Pipeline 调用参数：**

prompt： 描述所需视频的文本提示。

num_videos_per_prompt： 为每个提示生成的视频数量（此处设置为 1）。

num_inference_steps（推理步数）： 推理过程的步数，会影响视频质量。

num_frames（帧数）： 生成视频的帧数（设置为 49）。

use_dynamic_cfg：是否使用动态配置（设置为 True）。

output_type（输出类型）： 指定输出应为 “pt”（PyTorch 张量）格式。

guidance_scale： 控制输出与提示的一致性。

generator： 用给定种子初始化的 torch.Generator，以确保可重复性。

frames：提取生成的视频帧。

```
def infer(
    prompt: str,
    num_inference_steps: int,
    guidance_scale: float,
    seed: int = -1,
    progress=gr.Progress(track_tqdm=True),
):
    if seed == -1:
        seed = random.randint(0, 2**8 - 1)
    video_pt = pipe(
        prompt=prompt,
        num_videos_per_prompt=1,
        num_inference_steps=num_inference_steps,
        num_frames=49,
        use_dynamic_cfg=True,
        output_type=&quot;pt&quot;,
        guidance_scale=guidance_scale,
        generator=torch.Generator(device=&quot;cpu&quot;).manual_seed(seed),
    ).frames

    return (video_pt, seed)
```

## 处理视频转换为 GIF 格式

使用 moviepy 库将视频文件（MP4 格式）转换为 GIF 格式，在定期删除指定目录中超过一定时间的文件。

```
def convert_to_gif(video_path):
    clip = mp.VideoFileClip(video_path)
    clip = clip.set_fps(8)
    clip = clip.resize(height=240)
    gif_path = video_path.replace(&quot;.mp4&quot;, &quot;.gif&quot;)
    clip.write_gif(gif_path, fps=8)
    return gif_path
    
def delete_old_files():
    while True:
        now = datetime.now()
        cutoff = now - timedelta(minutes=10)
        directories = [&quot;./output&quot;, &quot;./gradio_tmp&quot;]

        for directory in directories:
            for filename in os.listdir(directory):
                file_path = os.path.join(directory, filename)
                if os.path.isfile(file_path):
                    file_mtime = datetime.fromtimestamp(os.path.getmtime(file_path))
                    if file_mtime &lt; cutoff:
                        os.remove(file_path)
        time.sleep(600)
```

## 线程和 Gradio 接口设置

使用 Python 线程机制运行后台任务，负责定期清理输出目录中的旧文件，以有效管理磁盘空间。通过在单独的线程中运行这个清理过程，主程序可以继续顺利运行，而不会被这些维护任务打断或减慢运行速度。

```
threading.Thread(target=delete_old_files, daemon=True).start()
```

然后，使用 Gradio 设置用户友好型界面，以简单的方式与 CogVideoX-5b 模型进行交互。

+   标题和信息显示： 界面以标题开始，提供了相关资源的链接和免责声明。

+   用户输入部分： 用户可以输入一个prompt来生成的视频，还可以选择使用的语言模型。

+   参数调整： 界面提供调整参数的选项，如推理步骤数和引导比例。

+   视频输出和下载选项： 视频生成后，会直接显示在界面中。用户可以选择下载视频或其 GIF 版本，从而灵活使用或共享输出结果。

```
with gr.Blocks() as demo:
    gr.Markdown(&quot;&quot;&quot;
           &lt;div style=&quot;text-align: center; font-size: 32px; font-weight: bold; margin-bottom: 20px;&quot;&gt;
               CogVideoX-5B Huggingface Space🤗
           &lt;/div&gt;
           &lt;div style=&quot;text-align: center;&quot;&gt;
               &lt;a href=&quot;https://huggingface.co/THUDM/CogVideoX-5B&quot;&gt;🤗 5B Model Hub&lt;/a&gt; |
               &lt;a href=&quot;https://github.com/THUDM/CogVideo&quot;&gt;🌐 Github&lt;/a&gt; |
               &lt;a href=&quot;https://arxiv.org/pdf/2408.06072&quot;&gt;📜 arxiv &lt;/a&gt;
           &lt;/div&gt;

           &lt;div style=&quot;text-align: center; font-size: 15px; font-weight: bold; color: red; margin-bottom: 20px;&quot;&gt;
            ⚠️ This demo is for academic research and experiential use only. 
            &lt;/div&gt;
           &quot;&quot;&quot;)
    with gr.Row():
        with gr.Column():
            prompt = gr.Textbox(label=&quot;Prompt (Less than 200 Words)&quot;, placeholder=&quot;Enter your prompt here&quot;, lines=5)

            with gr.Row():
                gr.Markdown(
                    &quot;✨Upon pressing the enhanced prompt button, we will use [GLM-4 Model](https://github.com/THUDM/GLM-4) to polish the prompt and overwrite the original one.&quot;
                )
                enhance_button = gr.Button(&quot;✨ Enhance Prompt(Optional)&quot;)

            with gr.Group():
                with gr.Column():
                    with gr.Row():
                        seed_param = gr.Number(
                            label=&quot;Inference Seed (Enter a positive number, -1 for random)&quot;, value=-1
                        )
                    with gr.Row():
                        enable_scale = gr.Checkbox(label=&quot;Super-Resolution (720 × 480 -&gt; 1440 × 960)&quot;, value=False)
                        enable_rife = gr.Checkbox(label=&quot;Frame Interpolation (8fps -&gt; 16fps)&quot;, value=False)
                    gr.Markdown(
                        &quot;✨In this demo, we use [RIFE](https://github.com/hzwer/ECCV2022-RIFE) for frame interpolation and [Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN) for upscaling(Super-Resolution).&lt;br&gt;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;The entire process is based on open-source solutions.&quot;
                    )

            generate_button = gr.Button(&quot;🎬 Generate Video&quot;)

        with gr.Column():
            video_output = gr.Video(label=&quot;CogVideoX Generate Video&quot;, width=720, height=480)
            with gr.Row():
                download_video_button = gr.File(label=&quot;📥 Download Video&quot;, visible=False)
                download_gif_button = gr.File(label=&quot;📥 Download GIF&quot;, visible=False)
                seed_text = gr.Number(label=&quot;Seed Used for Video Generation&quot;, visible=False)

    gr.Markdown(&quot;&quot;&quot;
    &lt;table border=&quot;0&quot; style=&quot;width: 100%; text-align: left; margin-top: 20px;&quot;&gt;
        &lt;div style=&quot;text-align: center; font-size: 32px; font-weight: bold; margin-bottom: 20px;&quot;&gt;
            🎥 Video Gallery
        &lt;/div&gt;
        &lt;tr&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top; font-size: 0.9em;&quot;&gt;
                &lt;p&gt;A garden comes to life as a kaleidoscope of butterflies flutters amidst the blossoms, their delicate wings casting shadows on the petals below. In the background, a grand fountain cascades water with a gentle splendor, its rhythmic sound providing a soothing backdrop. Beneath the cool shade of a mature tree, a solitary wooden chair invites solitude and reflection, its smooth surface worn by the touch of countless visitors seeking a moment of tranquility in nature&#x27;s embrace.&lt;/p&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top;&quot;&gt;
                &lt;video src=&quot;https://github.com/user-attachments/assets/cf5953ea-96d3-48fd-9907-c4708752c714&quot; width=&quot;100%&quot; controls autoplay loop&gt;&lt;/video&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top; font-size: 0.9em;&quot;&gt;
                &lt;p&gt;A small boy, head bowed and determination etched on his face, sprints through the torrential downpour as lightning crackles and thunder rumbles in the distance. The relentless rain pounds the ground, creating a chaotic dance of water droplets that mirror the dramatic sky&#x27;s anger. In the far background, the silhouette of a cozy home beckons, a faint beacon of safety and warmth amidst the fierce weather. The scene is one of perseverance and the unyielding spirit of a child braving the elements.&lt;/p&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top;&quot;&gt;
                &lt;video src=&quot;https://github.com/user-attachments/assets/fe0a78e6-b669-4800-8cf0-b5f9b5145b52&quot; width=&quot;100%&quot; controls autoplay loop&gt;&lt;/video&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;tr&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top; font-size: 0.9em;&quot;&gt;
                &lt;p&gt;A suited astronaut, with the red dust of Mars clinging to their boots, reaches out to shake hands with an alien being, their skin a shimmering blue, under the pink-tinged sky of the fourth planet. In the background, a sleek silver rocket, a beacon of human ingenuity, stands tall, its engines powered down, as the two representatives of different worlds exchange a historic greeting amidst the desolate beauty of the Martian landscape.&lt;/p&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top;&quot;&gt;
                &lt;video src=&quot;https://github.com/user-attachments/assets/c182f606-8f8c-421d-b414-8487070fcfcb&quot; width=&quot;100%&quot; controls autoplay loop&gt;&lt;/video&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top; font-size: 0.9em;&quot;&gt;
                &lt;p&gt;An elderly gentleman, with a serene expression, sits at the water&#x27;s edge, a steaming cup of tea by his side. He is engrossed in his artwork, brush in hand, as he renders an oil painting on a canvas that&#x27;s propped up against a small, weathered table. The sea breeze whispers through his silver hair, gently billowing his loose-fitting white shirt, while the salty air adds an intangible element to his masterpiece in progress. The scene is one of tranquility and inspiration, with the artist&#x27;s canvas capturing the vibrant hues of the setting sun reflecting off the tranquil sea.&lt;/p&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top;&quot;&gt;
                &lt;video src=&quot;https://github.com/user-attachments/assets/7db2bbce-194d-434d-a605-350254b6c298&quot; width=&quot;100%&quot; controls autoplay loop&gt;&lt;/video&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;tr&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top; font-size: 0.9em;&quot;&gt;
                &lt;p&gt;In a dimly lit bar, purplish light bathes the face of a mature man, his eyes blinking thoughtfully as he ponders in close-up, the background artfully blurred to focus on his introspective expression, the ambiance of the bar a mere suggestion of shadows and soft lighting.&lt;/p&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top;&quot;&gt;
                &lt;video src=&quot;https://github.com/user-attachments/assets/62b01046-8cab-44cc-bd45-4d965bb615ec&quot; width=&quot;100%&quot; controls autoplay loop&gt;&lt;/video&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top; font-size: 0.9em;&quot;&gt;
                &lt;p&gt;A golden retriever, sporting sleek black sunglasses, with its lengthy fur flowing in the breeze, sprints playfully across a rooftop terrace, recently refreshed by a light rain. The scene unfolds from a distance, the dog&#x27;s energetic bounds growing larger as it approaches the camera, its tail wagging with unrestrained joy, while droplets of water glisten on the concrete behind it. The overcast sky provides a dramatic backdrop, emphasizing the vibrant golden coat of the canine as it dashes towards the viewer.&lt;/p&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top;&quot;&gt;
                &lt;video src=&quot;https://github.com/user-attachments/assets/d78e552a-4b3f-4b81-ac3f-3898079554f6&quot; width=&quot;100%&quot; controls autoplay loop&gt;&lt;/video&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;tr&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top; font-size: 0.9em;&quot;&gt;
                &lt;p&gt;On a brilliant sunny day, the lakeshore is lined with an array of willow trees, their slender branches swaying gently in the soft breeze. The tranquil surface of the lake reflects the clear blue sky, while several elegant swans glide gracefully through the still water, leaving behind delicate ripples that disturb the mirror-like quality of the lake. The scene is one of serene beauty, with the willows&#x27; greenery providing a picturesque frame for the peaceful avian visitors.&lt;/p&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top;&quot;&gt;
                &lt;video src=&quot;https://github.com/user-attachments/assets/30894f12-c741-44a2-9e6e-ddcacc231e5b&quot; width=&quot;100%&quot; controls autoplay loop&gt;&lt;/video&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top; font-size: 0.9em;&quot;&gt;
                &lt;p&gt;A Chinese mother, draped in a soft, pastel-colored robe, gently rocks back and forth in a cozy rocking chair positioned in the tranquil setting of a nursery. The dimly lit bedroom is adorned with whimsical mobiles dangling from the ceiling, casting shadows that dance on the walls. Her baby, swaddled in a delicate, patterned blanket, rests against her chest, the child&#x27;s earlier cries now replaced by contented coos as the mother&#x27;s soothing voice lulls the little one to sleep. The scent of lavender fills the air, adding to the serene atmosphere, while a warm, orange glow from a nearby nightlight illuminates the scene with a gentle hue, capturing a moment of tender love and comfort.&lt;/p&gt;
            &lt;/td&gt;
            &lt;td style=&quot;width: 25%; vertical-align: top;&quot;&gt;
                &lt;video src=&quot;https://github.com/user-attachments/assets/926575ca-7150-435b-a0ff-4900a963297b&quot; width=&quot;100%&quot; controls autoplay loop&gt;&lt;/video&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
    &lt;/table&gt;
        &quot;&quot;&quot;)
```

## 生成和设置 Gradio 处理程序

调用 infer 函数，根据用户选择应用超分辨率和帧插值，将每个帧的张量转换为 PIL 图像，并且保存生成的视频并将其转换为 GIF，更新 Gradio 组件以显示生成的视频和 GIF。

如果用户选择增强提示，则使用 convert_prompt 函数来增强提示。点击生成按钮时触发生成函数，接收用户输入并更新输出组件。点击增强按钮时增强提示，用增强的提示更新提示文本框

```
def generate(prompt, seed_value, scale_status, rife_status, progress=gr.Progress(track_tqdm=True)):
        latents, seed = infer(
            prompt,
            num_inference_steps=50,  # NOT Changed
            guidance_scale=7.0,  # NOT Changed
            seed=seed_value,
            # progress=progress,
        )
        if scale_status:
            latents = utils.upscale_batch_and_concatenate(upscale_model, latents, device)
        if rife_status:
            latents = rife_inference_with_latents(frame_interpolation_model, latents)

        batch_size = latents.shape[0]
        batch_video_frames = []
        for batch_idx in range(batch_size):
            pt_image = latents[batch_idx]
            pt_image = torch.stack([pt_image[i] for i in range(pt_image.shape[0])])

            image_np = VaeImageProcessor.pt_to_numpy(pt_image)
            image_pil = VaeImageProcessor.numpy_to_pil(image_np)
            batch_video_frames.append(image_pil)

        video_path = utils.save_video(batch_video_frames[0], fps=math.ceil((len(batch_video_frames[0]) - 1) / 6))
        video_update = gr.update(visible=True, value=video_path)
        gif_path = convert_to_gif(video_path)
        gif_update = gr.update(visible=True, value=gif_path)
        seed_update = gr.update(visible=True, value=seed)

        return video_path, video_update, gif_update, seed_update

    def enhance_prompt_func(prompt):
        return convert_prompt(prompt, retry_times=1)

    generate_button.click(
        generate,
        inputs=[prompt, seed_param, enable_scale, enable_rife],
        outputs=[video_output, download_video_button, download_gif_button, seed_text],
    )

    enhance_button.click(enhance_prompt_func, inputs=[prompt], outputs=[prompt])

if __name__ == &quot;__main__&quot;:
    demo.launch()
```