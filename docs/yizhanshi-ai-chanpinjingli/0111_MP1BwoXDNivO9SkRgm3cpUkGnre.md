# 用 Huggingface Diffusers Pipeline 转换并文生视频

> 来源：[https://v11enp9ok1h.feishu.cn/docx/IV4Ddi5m6o0I9Yxyi8Yc5lrjn3y](https://v11enp9ok1h.feishu.cn/docx/IV4Ddi5m6o0I9Yxyi8Yc5lrjn3y)

此脚本将预训练的 Transformer 和 VAE检查点转换为与 CogVideoX 模型兼容的格式。然后利用 CogVideoX 模型，使用 Hugging Face Diffusers Pipeline 根据文本提示生成视频。

+   模型检查点的转换： Transformer 和 VAE 模型的检查点从其原始格式转换为与 CogVideoX 架构兼容的格式。此过程涉及重命名键、重新分配权重以及处理模型 state_dict 中的特殊情况。

+   文本生成视频： 在转换必要的模型检查点后，脚本使用文本提示生成视频序列。这是通过在 Hugging Face Diffusers 框架内利用 CogVideoX 模型的强大功能来实现的。

+   管道集成： 脚本将各种组件（transformer、VAE、调度器、文本编码器）集成到一个统一的管道中，然后用于视频生成。

*此脚本需要安装 `diffusers>=0.30.0` 库。

# 运行

要运行脚本，用户需要提供 Transformer 和 VAE 检查点的路径，指定输出目录，并提供文本编码器模型 (T5) 的路径。用户还可以选择以 FP16 精度保存模型权重以提高效率，或将模型推送到抱脸枢纽。

```
$ python convert_and_generate.py --transformer_ckpt_path &lt;path_to_transformer_checkpoint&gt; --vae_ckpt_path &lt;path_to_vae_checkpoint&gt; --output_path &lt;path_to_output_directory&gt; --text_encoder_path &lt;path_to_t5&gt;
```

# 重新分配

拆分并重新分配转换器模型中与query、key和value组件相关的权重。

使用 torch.chunk() 将key对应的权重拆分成三部分，然后将这些部分分配给新键（to_q、to_k 和 to_v），按照 CogVideoX 模型的要求重组权重。 从 state_dict 中删除原始键，以避免冗余。

```
import argparse
from typing import Any, Dict

import torch
from diffusers import AutoencoderKLCogVideoX, CogVideoXDDIMScheduler, CogVideoXPipeline, CogVideoXTransformer3DModel
from transformers import T5EncoderModel, T5Tokenizer


# Function to reassign the query, key, and value weights in-place
def reassign_query_key_value_inplace(key: str, state_dict: Dict[str, Any]):
    to_q_key = key.replace(&quot;query_key_value&quot;, &quot;to_q&quot;)
    to_k_key = key.replace(&quot;query_key_value&quot;, &quot;to_k&quot;)
    to_v_key = key.replace(&quot;query_key_value&quot;, &quot;to_v&quot;)
    to_q, to_k, to_v = torch.chunk(state_dict[key], chunks=3, dim=0)
    state_dict[to_q_key] = to_q
    state_dict[to_k_key] = to_k
    state_dict[to_v_key] = to_v
    state_dict.pop(key)
```

重新分配转换器模型中与query和key组件相关的层规范化权重。

检查key是否与query或key层相关。根据层和组件，函数会重命名关键字，使其符合 CogVideoX 模型中的预期格式。然后从 state_dict 中删除原始key。

```
# Function to reassign layer normalization for query and key in-place
def reassign_query_key_layernorm_inplace(key: str, state_dict: Dict[str, Any]):
    layer_id, weight_or_bias = key.split(&quot;.&quot;)[-2:]

    if &quot;query&quot; in key:
        new_key = f&quot;transformer_blocks.{layer_id}.attn1.norm_q.{weight_or_bias}&quot;
    elif &quot;key&quot; in key:
        new_key = f&quot;transformer_blocks.{layer_id}.attn1.norm_k.{weight_or_bias}&quot;

    state_dict[new_key] = state_dict.pop(key)
```

处理模型内自适应层归一化（AdaLN）权重的重新分配。

将权重拆分为 12 部分，然后将其重组为两组，分别对应两个归一化层。这些新权重将分配给 state_dict 中与 CogVideoX 模型预期结构相匹配的新key。然后从 state_dict 中删除原始的key。

```
# Function to reassign adaptive layer normalization in-place
def reassign_adaln_norm_inplace(key: str, state_dict: Dict[str, Any]):
    layer_id, _, weight_or_bias = key.split(&quot;.&quot;)[-3:]

    weights_or_biases = state_dict[key].chunk(12, dim=0)
    norm1_weights_or_biases = torch.cat(weights_or_biases[0:3] + weights_or_biases[6:9])
    norm2_weights_or_biases = torch.cat(weights_or_biases[3:6] + weights_or_biases[9:12])

    norm1_key = f&quot;transformer_blocks.{layer_id}.norm1.linear.{weight_or_bias}&quot;
    state_dict[norm1_key] = norm1_weights_or_biases

    norm2_key = f&quot;transformer_blocks.{layer_id}.norm2.linear.{weight_or_bias}&quot;
    state_dict[norm2_key] = norm2_weights_or_biases

    state_dict.pop(key)
```

从 state_dict 中删除指定的key。

```
# Function to remove keys from state_dict in-place
def remove_keys_inplace(key: str, state_dict: Dict[str, Any]):
    state_dict.pop(key)
```

从保存的检查点中提取 state_dict，处理各种可能的格式。检查已保存字典中不同的可能key（model、module、state_dict），并提取相应的state_dict以作进一步处理。

```
# Function to replace keys in the &quot;up&quot; block in-place
def replace_up_keys_inplace(key: str, state_dict: Dict[str, Any]):
    key_split = key.split(&quot;.&quot;)
    layer_index = int(key_split[2])
    replace_layer_index = 4 - 1 - layer_index

    key_split[1] = &quot;up_blocks&quot;
    key_split[2] = str(replace_layer_index)
    new_key = &quot;.&quot;.join(key_split)

    state_dict[new_key] = state_dict.pop(key)
```

定义几个对模型检查点转换过程至关重要的字典。这些字典用于重命名模型 state_dict 中的key值（如Transformers和VAEs），并处理key值需要特定就地操作的特殊情况。

```
# Dictionary for renaming transformer keys
TRANSFORMER_KEYS_RENAME_DICT = {
    &quot;transformer.final_layernorm&quot;: &quot;norm_final&quot;,
    &quot;transformer&quot;: &quot;transformer_blocks&quot;,
    &quot;attention&quot;: &quot;attn1&quot;,
    &quot;mlp&quot;: &quot;ff.net&quot;,
    &quot;dense_h_to_4h&quot;: &quot;0.proj&quot;,
    &quot;dense_4h_to_h&quot;: &quot;2&quot;,
    &quot;.layers&quot;: &quot;&quot;,
    &quot;dense&quot;: &quot;to_out.0&quot;,
    &quot;input_layernorm&quot;: &quot;norm1.norm&quot;,
    &quot;post_attn1_layernorm&quot;: &quot;norm2.norm&quot;,
    &quot;time_embed.0&quot;: &quot;time_embedding.linear_1&quot;,
    &quot;time_embed.2&quot;: &quot;time_embedding.linear_2&quot;,
    &quot;mixins.patch_embed&quot;: &quot;patch_embed&quot;,
    &quot;mixins.final_layer.norm_final&quot;: &quot;norm_out.norm&quot;,
    &quot;mixins.final_layer.linear&quot;: &quot;proj_out&quot;,
    &quot;mixins.final_layer.adaLN_modulation.1&quot;: &quot;norm_out.linear&quot;,
}

# Dictionary for handling special keys in transformer
TRANSFORMER_SPECIAL_KEYS_REMAP = {
    &quot;query_key_value&quot;: reassign_query_key_value_inplace,
    &quot;query_layernorm_list&quot;: reassign_query_key_layernorm_inplace,
    &quot;key_layernorm_list&quot;: reassign_query_key_layernorm_inplace,
    &quot;adaln_layer.adaLN_modulations&quot;: reassign_adaln_norm_inplace,
    &quot;embed_tokens&quot;: remove_keys_inplace,
}
# Dictionary for renaming VAE keys
VAE_KEYS_RENAME_DICT = {
    &quot;block.&quot;: &quot;resnets.&quot;,
    &quot;down.&quot;: &quot;down_blocks.&quot;,
    &quot;downsample&quot;: &quot;downsamplers.0&quot;,
    &quot;upsample&quot;: &quot;upsamplers.0&quot;,
    &quot;nin_shortcut&quot;: &quot;conv_shortcut&quot;,
    &quot;encoder.mid.block_1&quot;: &quot;encoder.mid_block.resnets.0&quot;,
    &quot;encoder.mid.block_2&quot;: &quot;encoder.mid_block.resnets.1&quot;,
    &quot;decoder.mid.block_1&quot;: &quot;decoder.mid_block.resnets.0&quot;,
    &quot;decoder.mid.block_2&quot;: &quot;decoder.mid_block.resnets.1&quot;,
}

# Dictionary for handling special keys in VAE
VAE_SPECIAL_KEYS_REMAP = {
    &quot;loss&quot;: remove_keys_inplace,
    &quot;up.&quot;: replace_up_keys_inplace,
}

# Maximum length of the tokenizer (Must be 226)
TOKENIZER_MAX_LENGTH = 226
```

从保存的检查点中提取 state_dict，处理各种可能的格式。检查已保存字典中不同的可能key（model、module、state_dict），并提取相应的状态判定以作进一步处理。

```
# Function to extract the state_dict from a saved checkpoint
def get_state_dict(saved_dict: Dict[str, Any]) -&gt; Dict[str, Any]:
    state_dict = saved_dict
    if &quot;model&quot; in saved_dict.keys():
        state_dict = state_dict[&quot;model&quot;]
    if &quot;module&quot; in saved_dict.keys():
        state_dict = state_dict[&quot;module&quot;]
    if &quot;state_dict&quot; in saved_dict.keys():
        state_dict = state_dict[&quot;state_dict&quot;]
    return state_dict
```

通过为现有条目分配新key名来更新 state_dict。简单地重命名 state_dict 中的key，将其值赋值给新的键，并删除旧的key。

```
# Function to update the state_dict with new key assignments in-place
def update_state_dict_inplace(state_dict: Dict[str, Any], old_key: str, new_key: str) -&gt; Dict[str, Any]:
    state_dict[new_key] = state_dict.pop(old_key)

```

# 模型转换

通过重命名key值和重新分配权重，将转换器检查点转换为 CogVideoX 格式。函数加载变换器检查点并提取状态信息，并且遍历状态信息中的键，并根据预定义规则重新命名它们。然后应用特定的函数来处理特殊键，如查询、键和值权重。

**返回：**

返回转换后的模型，以便在 CogVideoX Pipeline 中使用。

```
# Function to convert a transformer checkpoint to the CogVideoX format
def convert_transformer(ckpt_path: str):
    PREFIX_KEY = &quot;model.diffusion_model.&quot;

    original_state_dict = get_state_dict(torch.load(ckpt_path, map_location=&quot;cpu&quot;, mmap=True))
    transformer = CogVideoXTransformer3DModel()

    for key in list(original_state_dict.keys()):
        new_key = key[len(PREFIX_KEY) :]
        for replace_key, rename_key in TRANSFORMER_KEYS_RENAME_DICT.items():
            new_key = new_key.replace(replace_key, rename_key)
        update_state_dict_inplace(original_state_dict, key, new_key)

    for key in list(original_state_dict.keys()):
        for special_key, handler_fn_inplace in TRANSFORMER_SPECIAL_KEYS_REMAP.items():
            if special_key not in key:
                continue
            handler_fn_inplace(key, original_state_dict)

    transformer.load_state_dict(original_state_dict, strict=True)
    return transformer
```

将 VAE 检查点转换为 CogVideoX 格式

将 VAE 检查点转换为 CogVideoX 格式，与Transformer的转换方式类似。加载 VAE 检查点并提取 state_dict，遍历 state_dict 中的key，应用预定义的重命名规则，应用特定函数来处理 VAE 架构中的特殊键。

**返回： **

返回转换后的 VAE 模型，以便在 CogVideoX Pipeline 中使用。

```
# Function to convert a VAE checkpoint to the CogVideoX format
def convert_vae(ckpt_path: str):
    original_state_dict = get_state_dict(torch.load(ckpt_path, map_location=&quot;cpu&quot;, mmap=True))
    vae = AutoencoderKLCogVideoX()

    for key in list(original_state_dict.keys()):
        new_key = key[:]
        for replace_key, rename_key in VAE_KEYS_RENAME_DICT.items():
            new_key = new_key.replace(replace_key, rename_key)
        update_state_dict_inplace(original_state_dict, key, new_key)

    for key in list(original_state_dict.keys()):
        for special_key, handler_fn_inplace in VAE_SPECIAL_KEYS_REMAP.items():
            if special_key not in key:
                continue
            handler_fn_inplace(key, original_state_dict)

    vae.load_state_dict(original_state_dict, strict=True)
    return vae
```

# 参数解析

解析用户为配置脚本而提供的命令行参数，设置预期参数，如检查点路径、输出目录和其他选项。然后解析所提供的命令行参数，并返回参数供脚本使用。

```
# Function to parse command-line arguments for the script
def get_args():
    parser = argparse.ArgumentParser()
    parser.add_argument(
        &quot;--transformer_ckpt_path&quot;, type=str, default=None, help=&quot;Path to original transformer checkpoint&quot;
    )
    parser.add_argument(&quot;--vae_ckpt_path&quot;, type=str, default=None, help=&quot;Path to original vae checkpoint&quot;)
    parser.add_argument(&quot;--output_path&quot;, type=str, required=True, help=&quot;Path where converted model should be saved&quot;)
    parser.add_argument(
        &quot;--text_encoder_path&quot;,
        type=str,
        required=True,
        default=&quot;google/t5-v1_1-xxl&quot;,
        help=&quot;Path where converted model should be saved&quot;,
    )
    parser.add_argument(
        &quot;--text_encoder_cache_dir&quot;,
        type=str,
        default=None,
        help=&quot;Path to text encoder cache directory. Not needed if text_encoder_path is in your local.&quot;,
    )
    parser.add_argument(&quot;--fp16&quot;, action=&quot;store_true&quot;, default=True, help=&quot;Whether to save the model weights in fp16&quot;)
    parser.add_argument(
        &quot;--push_to_hub&quot;, action=&quot;store_true&quot;, default=False, help=&quot;Whether to push to HF Hub after saving&quot;
    )
    return parser.parse_args()
```

首先使用 get_args() 解析命令行参数，处理脚本的执行。脚本初始化转换器和 VAE 模型的占位符，如果提供了各自的检查点路径，则转换这些模型。然后，脚本使用 T5 模型加载标记器和文本编码器，并为视频生成设置调度程序。这些组件被组合成一个 CogVideoXPipeline。如果指定，管道将转换为 FP16 精度，以提高效率。最后，管道被保存到输出目录，并可选择推送到 Hugging Face Hub。

```
if __name__ == &quot;__main__&quot;:
    args = get_args()

    transformer = None
    vae = None

    if args.transformer_ckpt_path is not None:
        transformer = convert_transformer(args.transformer_ckpt_path)
    if args.vae_ckpt_path is not None:
        vae = convert_vae(args.vae_ckpt_path)

    tokenizer = T5Tokenizer.from_pretrained(args.text_encoder_path, model_max_length=TOKENIZER_MAX_LENGTH)
    text_encoder = T5EncoderModel.from_pretrained(args.text_encoder_path, cache_dir=args.text_encoder_cache_dir)

    scheduler = CogVideoXDDIMScheduler.from_config(
        {
            &quot;snr_shift_scale&quot;: 3.0,
            &quot;beta_end&quot;: 0.012,
            &quot;beta_schedule&quot;: &quot;scaled_linear&quot;,
            &quot;beta_start&quot;: 0.00085,
            &quot;clip_sample&quot;: False,
            &quot;num_train_timesteps&quot;: 1000,
            &quot;prediction_type&quot;: &quot;v_prediction&quot;,
            &quot;rescale_betas_zero_snr&quot;: True,
            &quot;set_alpha_to_one&quot;: True,
            &quot;timestep_spacing&quot;: &quot;linspace&quot;,
        }
    )

    pipe = CogVideoXPipeline(
        tokenizer=tokenizer, text_encoder=text_encoder, vae=vae, transformer=transformer, scheduler=scheduler
    )

    if args.fp16:
        pipe = pipe.to(dtype=torch.float16)

    pipe.save_pretrained(args.output_path, safe_serialization=True, push_to_hub=args.push_to_hub)
```