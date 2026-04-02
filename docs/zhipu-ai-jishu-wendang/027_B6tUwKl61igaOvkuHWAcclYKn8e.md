# Intel 设备加速推理

> 来源：[https://ai.feishu.cn/docx/DWXYdGyAio0blFxUB7ocUA0pnnb](https://ai.feishu.cn/docx/DWXYdGyAio0blFxUB7ocUA0pnnb)

此文档旨在帮助开发者在 Intel 设备上适配 / 部署 GLM-4-9B-Chat 模型。

> 本 Demo 仅支持 GLM-4-9B-Chat 模型，不支持 GLM-4v-9B 模型。

> 本 Demo 必须使用 Linux 操作系统。

# 选择方案

+   如果您的设备支持 OpenVINO 版本 2024.2.0，您可以使用 OpenVINO 加速方案，跳过 ITREX方案，使用OpenVINO 套件推理速度更加迅速。

+   如果您仅仅是 Intel 设备，但是不支持 OpenVINO （例如 NPU设备），您可以使用 ITREX方案，保证模型能运行。

# ITREX 推理适配

本代码将带领开发者使用 intel-extension-for-transformers  适配推理 GLM-4-9B-Chat 模型

请配合以下代码库中微调部分代码进行上手练习：

https://github.com/THUDM/GLM-4/tree/main/intel_device_demo

## 环境配置

首先，你需要在安装好 [快速使用（推理部署）](https://zhipu-ai.feishu.cn/wiki/PCDWwC2Ndiuj39kryJZcsTDbnlg) 中提到的依赖的基础下，安装本 Demo需要的依赖

```
cd intel_device_demo_openvino
pip install -r requirements.txt
```

## 运行模型推理

您可以使用一行代码运行模型推理：

```
python itrex_cli_demo.py
```

如果您是第一次推理，会有一次模型转换权重的过程，转换后的模型权重存放在`runtime_outputs`文件夹下，这大概会消耗`60G`的硬盘空间。

转换完成后，文件夹下有两个文件：

```
ne_chatglm2_f32.bin 52G(如果您不使用FP32进行推理，可以删掉这个文件)
ne_chatglm2_q_nf4_bestla_cfp32_sym_sfp32_g32.bin 8.1G
```

如果您不是第一次推理，则会跳过这个步骤，直接开始对话，推理效果如下：

```
shell
Welcome to the CLI chat. Type your messages below.

User: 你好
AVX:1 AVX2:1 AVX512F:1 AVX512BW:1 AVX_VNNI:0 AVX512_VNNI:1 AMX_INT8:0 AMX_BF16:0 AVX512_BF16:0 AVX512_FP16:0
beam_size: 1, do_sample: 1, top_k: 40, top_p: 0.900, continuous_batching: 0, max_request_num: 1, early_stopping: 0, scratch_size_ratio: 1.000
model_file_loader: loading model from runtime_outs/ne_chatglm2_q_nf4_bestla_cfp32_sym_sfp32_g32.bin
Loading the bin file with NE format...
load_ne_hparams  0.hparams.n_vocab = 151552                        
load_ne_hparams  1.hparams.n_embd = 4096                          
load_ne_hparams  2.hparams.n_mult = 0                             
load_ne_hparams  3.hparams.n_head = 32                            
load_ne_hparams  4.hparams.n_head_kv = 0                             
load_ne_hparams  5.hparams.n_layer = 40                            
load_ne_hparams  6.hparams.n_rot = 0                             
load_ne_hparams  7.hparams.ftype = 0                             
load_ne_hparams  8.hparams.max_seq_len = 131072                        
load_ne_hparams  9.hparams.alibi_bias_max = 0.000                         
load_ne_hparams  10.hparams.clip_qkv = 0.000                         
load_ne_hparams  11.hparams.par_res = 0                             
load_ne_hparams  12.hparams.word_embed_proj_dim = 0                             
load_ne_hparams  13.hparams.do_layer_norm_before = 0                             
load_ne_hparams  14.hparams.multi_query_group_num = 2                             
load_ne_hparams  15.hparams.ffn_hidden_size = 13696                         
load_ne_hparams  16.hparams.inner_hidden_size = 0                             
load_ne_hparams  17.hparams.n_experts = 0                             
load_ne_hparams  18.hparams.n_experts_used = 0                             
load_ne_hparams  19.hparams.n_embd_head_k = 0                             
load_ne_hparams  20.hparams.norm_eps = 0.000000                      
load_ne_hparams  21.hparams.freq_base = 5000000.000                   
load_ne_hparams  22.hparams.freq_scale = 1.000                         
load_ne_hparams  23.hparams.rope_scaling_factor = 0.000                         
load_ne_hparams  24.hparams.original_max_position_embeddings = 0                             
load_ne_hparams  25.hparams.use_yarn = 0                             
load_ne_vocab    26.vocab.bos_token_id = 1                             
load_ne_vocab    27.vocab.eos_token_id = 151329                        
load_ne_vocab    28.vocab.pad_token_id = 151329                        
load_ne_vocab    29.vocab.sep_token_id = -1                            
init: hparams.n_vocab         = 151552
init: hparams.n_embd          = 4096
init: hparams.n_mult          = 0
init: hparams.n_head          = 32
init: hparams.n_layer         = 40
init: hparams.n_rot           = 0
init: hparams.ffn_hidden_size = 13696
init: n_parts    = 1
load: ctx size   = 16528.38 MB
load: layers[0].ffn_fusion    = 1
load: scratch0   = 4096.00 MB
load: scratch1   = 2048.00 MB
load: scratch2   = 4096.00 MB
load: mem required  = 26768.38 MB (+ memory per state)
.............................................................................................
model_init_from_file: support_bestla_kv = 1
kv_cache_init: run_mha_reordered = 1
model_init_from_file: kv self size =  690.00 MB
Assistant:
你好👋！我是人工智能助手，很高兴为你服务。有什么可以帮助你的吗？
```

# OpenVINO™ 推理加速

本代码将带领开发者使用 Intel® Distribution of OpenVINO™ Toolkit 加速推理 GLM-4-9B-Chat 模型

请配合以下代码库中微调部分代码进行上手练习：

https://github.com/THUDM/GLM-4/tree/main/intel_device_demo

[OpenVINO](https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/overview.html) 是 Intel 为深度学习推理而设计的开源工具包。它可以帮助开发者优化模型，提高推理性能，减少模型的内存占用。 本示例将展示如何使用 OpenVINO 部署 GLM-4-9B-Chat 模型。

## 环境配置

首先，你需要在安装好 [快速使用（推理部署）](https://zhipu-ai.feishu.cn/wiki/PCDWwC2Ndiuj39kryJZcsTDbnlg) 中提到的依赖的基础下，安装本 Demo需要的依赖

```
cd intel_device_demo_openvino
pip install -r requirements.txt
```

## 模型权重转换

由于需要将Huggingface模型转换为OpenVINO IR模型，因此您需要下载模型并转换。

```
python3 convert.py --model_id THUDM/glm-4-9b-chat --output {your_path}/glm-4-9b-chat-ov
```

可选择参数

```
`--model_id` - 模型所在目录的路径（绝对路径）。
`--output` - 转换后模型保存的地址。
`--precision` - 转换的精度。
```

转换过程如下

```
====Exporting IR=====
Framework not specified. Using pt to export the model.
Loading checkpoint shards: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 10/10 [00:04&lt;00:00,  2.14it/s]
Special tokens have been added in the vocabulary, make sure the associated word embeddings are fine-tuned or trained.
Special tokens have been added in the vocabulary, make sure the associated word embeddings are fine-tuned or trained.
Special tokens have been added in the vocabulary, make sure the associated word embeddings are fine-tuned or trained.
Special tokens have been added in the vocabulary, make sure the associated word embeddings are fine-tuned or trained.
Using framework PyTorch: 2.3.1+cu121
Mixed-Precision assignment ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 160/160 • 0:01:45 • 0:00:00
INFO:nncf:Statistics of the bitwidth distribution:
┍━━━━━━━━━━━━━━━━┯━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┯━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┑
│   Num bits (N) │ % all parameters (layers)   │ % ratio-defining parameters (layers)   │
┝━━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┿━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┥
│              8 │ 31% (76 / 163)              │ 20% (73 / 160)                         │
├────────────────┼─────────────────────────────┼────────────────────────────────────────┤
│              4 │ 69% (87 / 163)              │ 80% (87 / 160)                         │
┕━━━━━━━━━━━━━━━━┷━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┷━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┙
Applying Weight Compression ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 163/163 • 0:03:46 • 0:00:00
Configuration saved in glm-4-9b-ov/openvino_config.json
====Exporting tokenizer=====
Special tokens have been added in the vocabulary, make sure the associated word embeddings are fine-tuned or trained.
```

## 运行模型推理

使用一行代码推理模型：

```
python3 chat.py --model_path {your_path}/glm-4-9b-chat-ov --max_sequence_length 4096 --device CPU
```

可选择参数

```
`--model_path` - OpenVINO IR 模型所在目录的路径。
`--max_sequence_length` - 输出标记的最大大小。
`--device` - 运行推理的设备。
```