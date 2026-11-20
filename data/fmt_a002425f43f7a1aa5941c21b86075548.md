
# FLUX现有Lora模型

> 来源：[https://aigcreative.feishu.cn/docx/HWEQdckZmodnz0xUkIicMnGMnGg](https://aigcreative.feishu.cn/docx/HWEQdckZmodnz0xUkIicMnGMnGg)

> 目前Xlabs已发布8款Lora模型

> 下载地址：`https://huggingface.co/XLabs-AI/flux-lora-collection`

XLabs-AI 团队在 Hugging Face 平台上发布了一个名为 `flux-lora-collection` 的模型库，该库包含了针对 `FLUX.1-dev` 模型训练好的 LoRAs 检查点。

这些 LoRAs 可以用于文本到图像的生成任务，支持多种风格如英国风格、迪士尼风格、动漫风格等。其中每个图像文件对应一个包含文本提示的 JSON 文件。

![](img/f346b66d12718807906b058d51fd0184.png)

![](img/2332a7cc35211a5101e94450192da66e.png)

## `furry_lora` 毛茸茸的

```
python3 main.py \
 --prompt &quot;Female furry Pixie with text &#x27;hello world&#x27;&quot; \
 --lora_repo_id XLabs-AI/flux-furry-lora --lora_name furry_lora.safetensors --device cuda --offload --use_lora \
 --model_type flux-dev-fp8 --width 1024 --height 1024 \
 --timestep_to_start_cfg 1 --num_steps 25 --true_gs 3.5 --guidance 4
```

![](img/adaca2c2fc1cec7cad25e0044d610514.png)

```
python3 main.py \
 --prompt &quot;Male furry Lycanthrope with fur-covered body in ancient ruins, howling at the full moon, surrounded by eerie mist, werewolf transformation, elder scrolls, eslweyr, glitch aesthetic, anime-inspired, digital illustration, artstation, furry&quot; \
 --lora_repo_id XLabs-AI/flux-furry-lora --lora_name furry_lora.safetensors --device cuda --offload --use_lora \
 --model_type flux-dev-fp8 --width 1024 --height 1024 \
 --timestep_to_start_cfg 1 --num_steps 25 --true_gs 3.5
```

## `mjv6_lora` mjv6

```
python3 main.py \
--prompt &quot;A handsome man in a suit, 25 years old, cool, futuristic&quot; \
--lora_repo_id XLabs-AI/flux-lora-collection --lora_name mjv6_lora.safetensors \
--device cuda:4 --offload --use_lora --model_type flux-dev-fp8 --width 1024 --height 1024
```

```
python3 main.py \
--prompt &quot;A girl in a suit covered with bold tattoos and holding a vest pistol, beautiful woman, 25 years old, cool, future fantasy, turquoise &amp; light orange ping curl hair&quot; \
--lora_repo_id XLabs-AI/flux-lora-collection --lora_name mjv6_lora.safetensors \
--device cuda --offload --use_lora --model_type flux-dev-fp8 --width 1024 --height 1024
```

## `anime_lora` 动漫

```
python3 main.py \
--prompt &quot;A cute corgi lives in a house made out of sushi, anime&quot; \
--lora_repo_id XLabs-AI/flux-lora-collection --lora_name anime_lora.safetensors \
--device cuda --offload --use_lora --model_type flux-dev-fp8 --width 1024 --height 1024
```

```
python3 main.py \
--prompt &quot;a girl with orange hair, standing in a room with a window, looking out at a cityscape, anime&quot; \
--lora_repo_id XLabs-AI/flux-lora-collection --lora_name anime_lora.safetensors \
--device cuda --offload --use_lora --model_type flux-dev-fp8 --width 1024 --height 1024
```

## `disney_lora` 迪士尼

```
python3 main.py \
--prompt &quot;An aerial view of beach with people on it, disney style&quot; \
--lora_repo_id XLabs-AI/flux-lora-collection --lora_name disney_lora.safetensors \
--device cuda --offload --use_lora --model_type flux-dev-fp8 --width 1024 --height 1024
```

```
python3 main.py \
--prompt &quot;A blue jay standing on a large basket of rainbow macarons, disney style&quot; \
--lora_repo_id XLabs-AI/flux-lora-collection --lora_name disney_lora.safetensors \
--device cuda --offload --use_lora --model_type flux-dev-fp8 --width 1024 --height 1024
```

## `scenery_lora` 风景

```
python3 main.py \
--prompt &quot;A fantasy cityscape with multiple buildings and skyscrapers all of which are covered in snow and ice, scenery style&quot; \
--lora_repo_id XLabs-AI/flux-lora-collection --lora_name scenery_lora.safetensors \
--device cuda --offload --use_lora --model_type flux-dev-fp8 --width 1024 --height 1024
```

![](img/1b2f915ccf8c23b600ae9423405e00e9.png)

```
python3 main.py \
--prompt &quot;A large ornate building with multiple levels and arches surrounded by trees and greenery. In front of it there are several statues and sculptures on pedestals with fire burning brightly in front of them, scenery style&quot; \
--lora_repo_id XLabs-AI/flux-lora-collection --lora_name scenery_lora.safetensors \
--device cuda --offload --use_lora --model_type flux-dev-fp8 --width 1024 --height 1024
```

## `art_lora` 艺术

```
python3 main.py \
--prompt &quot;white rabbit in blue dress and hat holding bow and arrow, art&quot; \
--lora_repo_id XLabs-AI/flux-lora-collection --lora_name art_lora.safetensors \
--device cuda --offload --use_lora --model_type flux-dev-fp8 --width 1024 --height 1024
```

![](img/6a35d612b45344683bbb6f480058dab8.png)

```
python3 main.py \
--prompt &quot;castle in the middle of forest at night, art&quot; \
--lora_repo_id XLabs-AI/flux-lora-collection --lora_name art_lora.safetensors \
--device cuda --offload --use_lora --model_type flux-dev-fp8 --width 1024 --height 1024
```

![](img/2eb544212400774233d32bcdd7989103.png)

`https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg`
