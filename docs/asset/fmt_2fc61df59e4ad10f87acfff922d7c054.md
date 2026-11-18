
```
# inference for submission
# TextVQA
echo "Infer on TextVQA..."
bash $SCRIPT_DIR/eval/textvqa.sh $FINETUNE_NAME $INFER_CUDA_IDX
# MMBench
echo "Infer on MMBench..."
bash $SCRIPT_DIR/eval/mmbench.sh $FINETUNE_NAME "mmbench_dev_20230712" $INFER_CUDA_IDX

# copy this script to output
cp $0 $SCRIPT_DIR/../output/train.sh

# info
echo "Training and Inference done."
echo "Training checkpoints are stored in output/training_dirs/$FINETUNE_NAME."
echo "Inference results are stored in output/eval_results/$FINETUNE_NAME."
```

# Baseline 全流程解答

## 评测环节

最后，我们愉快的来到了评测环节，具体需要通过 `TextVQA` 以及 `MMBench` 的测试验证（平均耗时10~20min），然后就可以成功的结束所有过程了，收拾收拾开始提交评测！

至此 `baseline` 全流程解答已结束。

### ⚠常见问题必知必会

+   最快需要多久可以跑完一个`baselilne`？

+   四个小时，包括环境准备。

+   推荐的运行环境？

+   **120G以上CPU内存** + 80G显存（实际上40G也够）机器即可，否则需要进行配置修改

+   10K数据的正常运行峰值需要102G的CPU内存以及40G的GPU显存。

+   autodl的3090或者4090双卡皆可，有更大的显存的双卡更佳；只要双卡可以解决一切内存不够问题 —— 在足够的火力覆盖面前，一切都是浮云

+   很难让我退缩怎么办？

+   等待的时候可以打几盘王者荣耀（黑神话悟空也可以，但是还没出）

+   实在不行还可以来下面体验`data-juicer`，只要开心就好

+   充钱你就可以体验速通的快乐！有句话叫做氪金会变强

+   在群里找到大腿，请他喝杯奶茶，让它教你

+   安装 `flashattn` 很慢怎么办

+   他在努力的编译了，等等吧

+   预编译包下载：https://github.com/Dao-AILab/flash-attention/releases/tag/v2.6.3

+   安装`flashattn`遇到 `CUDA` 版本问题怎么办

+   因为你的`CUDA`默认就是12开头的，而`mgm`的默认可能是11.x的`torch`，所以你只需要安装最新12.x的`torch`支持即可，比如：

```
pip install torch==2.2.1 torchvision==0.17.1 torchaudio==2.2.1
```

什么？你说**安装**后看到了红色的error，说`mgm`只支持2.0.1的`torch`？ 没事，`torch`的跨版本兼容性非常完美，忽略这个提示即可；当然你得知道这个支持并不是真正的“支持”，所谓在`pip`安装过程中的版本报错，只是因为安装依赖`requirements`里面的推荐版本是xxx，如果我们的版本和他不同，他就会显示一个“报错”；但极机智的我们对环境配置已经了如指掌如入无人之境，直接忽视，我的环境我做主。

+   我是学生。。。。你懂的

+   对不起，我也穷，我把充值买游戏皮肤的钱暂时转移成了显卡的燃烧数字；让我们就当作少喝几杯奶茶的减肥身材管理期

+   虽然钱没了，但你可以和别人吹你跑过100多G的程序，**让他向你投来崇拜（也可能是鄙视）的眼光后扬长而去。

## 什么是MGM

`MGM`本质上来自一篇最新的多模态性能提高框架（基于`LLAVA`）—— https://github.com/dvlab-research/MGM

对应论文—— https://arxiv.org/pdf/2403.18814

![](img/da93cf6eafb80193d10384f03148aba7.png)

这篇论文介绍了一个名为`Mini-Gemini`的框架，旨在提升多模态视觉语言模型（`VLMs`）的性能。尽管`VLMs`在基础视觉对话和推理方面取得了进展，但与高级模型（如`GPT-4`和`Gemini`）相比，仍存在性能差距。`Mini-Gemini`试图通过以下三个方面来缩小这一差距：

1. 高分辨率视觉令牌（High-Resolution Visual Tokens）：通过额外的视觉编码器对视觉令牌进行高分辨率细化，而不增加视觉令牌的数量。

1. 高质量数据（High-Quality Data）：构建一个高质量的数据集，以促进精确的图像理解和基于推理的生成，扩展当前`VLMs`的操作范围。

1. VLM引导的生成（VLM-guided Generation）：将这些增强与先进的大型语言模型（`LLMs`）和生成模型相结合，旨在提升`VLM`性能和用户体验。

`Mini-Gemini`框架支持从2B到34B参数规模的一系列密集和`MoE`大型语言模型，并且在多个零样本基准测试中取得了领先的性能，甚至超过了一些开发的私有模型。论文的目标是通过挖掘`VLMs`的潜力，同时进行图像理解、推理和生成，为当前框架赋予新的能力。

MGM框架支持的模型一览：

在本次任务中，我们将会使用这个框架来进行 `pretrain` 以及 `finetune`。

```
mkdir -p $SCRIPT_DIR/../output/training_dirs/$PRETRAIN_NAME

deepspeed $SCRIPT_DIR/training/mgm/train/train_mem.py \
    --deepspeed $SCRIPT_DIR/training/scripts/zero2_offload.json \
    --model_name_or_path $SCRIPT_DIR/training/model_zoo/LLM/gemma/gemma-2b-it \
    --version gemma \
    --data_path $PRETRAIN_DATASET_JSON \
    --image_folder $PRETRAIN_DATASET_IMAGE_PATH \
    --vision_tower $SCRIPT_DIR/training/model_zoo/OpenAI/clip-vit-large-patch14-336 \
    --vision_tower_aux $SCRIPT_DIR/training/model_zoo/OpenAI/openclip-convnext-large-d-320-laion2B-s29B-b131K-ft-soup \
    --mm_projector_type mlp2x_gelu \
    --tune_mm_mlp_adapter True \
    --mm_vision_select_layer -2 \
    --mm_use_im_start_end False \
    --mm_use_im_patch_token False \
    --image_size_aux $AUX_SIZE \
    --bf16 True \
    --output_dir $SCRIPT_DIR/../output/training_dirs/$PRETRAIN_NAME \
    --num_train_epochs $NUM_TRAIN_EPOCHS \
    --per_device_train_batch_size $PRETRAIN_BATCH_SIZE_PER_GPU \
    --per_device_eval_batch_size 4 \
    --gradient_accumulation_steps $PRETRAIN_GRADIENT_ACCUMULATION_STEPS \
    --evaluation_strategy "no" \
    --save_strategy "steps" \
    --save_steps $CKPT_SAVE_STEPS \
    --save_total_limit $TOTAL_SAVE_CKPT_LIMIT \
    --learning_rate 1e-3 \
    --weight_decay 0. \
    --warmup_ratio 0.03 \
    --lr_scheduler_type "cosine" \
    --logging_steps $LOGGING_STEP \
    --tf32 True \
    --model_max_length 2048 \
    --gradient_checkpointing True \
    --dataloader_num_workers $PRETRAIN_DATALOADER_NUM_WORKERS \
    --lazy_preprocess True \
    --report_to none \
    2>&1 | tee $SCRIPT_DIR/../output/training_dirs/$PRETRAIN_NAME/pretrain.log
```

具体的代码分析已经在之前看过，让我们不必深究细节，继续往下。

## TextVQA

遇事不决官网见：https://textvqa.org/

`TextVQA` 是一个用于评估基于图像中文本的视觉推理能力的数据集。这个数据集要求模型能够读取和理解图像中的文本，以回答相关的问题。`TextVQA` 数据集包含来自 `OpenImages` 的 28,408 张图像，45,336 个问题，以及 453,360 个标准答案。

总而言之就是一个用于给我们测试多模态模型能力的数据集。

## MMBench

遇事不决官网见：https://mmbench.opencompass.org.cn/

paper见：https://arxiv.org/pdf/2307.06281

`MMBench` 试图解决的问题是如何有效地评估大型视觉-语言模型（Large Vision-Language Models，简称`LVLMs`）的性能。尽管这些模型在视觉信息的感知和推理方面取得了显著进展，但现有的评估方法存在一些限制，如缺乏细粒度能力评估和非鲁棒性评估指标。为了克服这些限制，他们提出了一个名为`MMBench`的新型多模态基准测试，它包括两个主要组成部分：

1. **精心策划的数据集**：该数据集在评估问题的数量和多样性以及能力方面超越了现有的类似基准测试。

1. 新颖的`CircularEval`策略：结合了使用`ChatGPT`的方法，将自由形式的预测转换为预定义的选择，从而促进对模型预测的更鲁棒评估。

总而言之就是又一个用于给我们测试多模态模型能力的数据集。

至此，你已经完全理解了 `Baseline` 究竟在做什么

## 🍉`Data-juicer` 入门

![](img/15b6fd1d48ae317de8a00b3e0da6d8a2.png)

### 什么是 `data-juicer`

那么，什么是 `data-juicer` 呢？你可以认为他就是一个厨师， 你可以认为他是多模态世界中的数据守护者，通过不同算子实现组合实现了数据的高效清洗，此外，由于各种论文对数据处理的方法方式都不同，其中又蕴含了相似之处，通过高层抽象我们就可以很容易对不同论文的实现进行完美的复现，这也就为大家的科研工作和业务工作带来了更多的便捷。

当然，以上都是我自己猜的，可能不够全面，让我们直接看看官网的介绍：

> `Data-Juicer` 是一个多模态数据处理系统，旨在提升数据质量，使其更适合用于大语言模型（`LLMs`）。它提供了一个托管的 `JupyterLab` 平台，用户可以直接在浏览器中体验 `Data-Juicer` 的功能。

> 系统提供了一系列用于数据处理的强大工具，包括 80 多个核心操作符、20 多个可重用配置和丰富的工具包。`Data-Juicer` 支持多种数据格式和多模态数据处理（如文本、图像、音频、视频），并且可以灵活扩展以满足特定需求。

> 主要功能包括数据与模型协同开发、数据可视化、自动化评估以及高效的并行数据处理。用户可以通过不同的接口和工具进行数据的处理和分析，也可以使用沙盒功能进行数据与模型的快速迭代和优化。

> `Data-Juicer` 还支持分布式数据处理，用户可以利用 `RAY` 实现多机器数据处理，或者在集群环境中使用 `Slurm` 或 `Aliyun PAI-DLC` 进行数据分割和处理。

不过，就算是看了这些我们也无法理解全貌，让我们不如来看看他的核心，由于字数太多我不想打全称，以下让我们用 `DJ` 来称呼 `Data-Juicer` 。

### 深入理解算子

在`DJ`中，最核心的部分就是繁杂多样的算子实现，通过算子的排列组合成 `workflow` 实现更多操作；

https://github.com/modelscope/data-juicer/blob/main/docs/Operators_ZH.md#formatter

`Data-Juicer` 中的算子分为以下 5 种类型。

下面列出所有具体算子，每种算子都通过多个标签来注明其主要功能。

+   Domain 标签

+   General: 一般用途

+   `LaTeX`: 专用于 `LaTeX` 源文件

+   Code: 专用于编程代码

+   Financial: 与金融领域相关

+   Image: 专用于图像或多模态

+   Audio: 专用于音频或多模态

+   Video: 专用于视频或多模态

+   Multimodal: 专用于多模态

具体的算子我们可以只看一部分与多模态可能有关的算子：

| 算子 | 描述 |

| --- | --- |

| `image_captioning_from_gpt4v_mapper` | 基于`gpt-4-vision`和图像生成文本 |

| `image_captioning_mapper` | 生成样本，其标题是根据另一个辅助模型（例如 `blip2`）和原始样本中的图形生成的。 |

| `image_diffusion_mapper` | 用`stable diffusion`生成图像，对图像进行增强 |

| `video_captioning_from_frames_mapper` | 生成样本，其标题是基于一个文字生成图片的模型和原始样本视频中指定帧的图像。不同帧产出的标题会拼接为一条单独的字符串。 |

| `image_text_matching_filter` | 保留图像-文本的分类匹配分(基于`BLIP`模型)在指定范围内的样本 |

| `image_text_similarity_filter` | 保留图像-文本的特征余弦相似度(基于`CLIP`模型)在指定范围内的样本 |

| `phrase_grounding_recall_filter` | 保留从文本中提取的名词短语在图像中的定位召回率在一定范围内的样本 |

| `video_frames_text_similarity_filter` | 保留视频中指定帧的图像-文本的特征余弦相似度(基于`CLIP`模型)在指定范围内的样本 |

纯粹文本以及基本元素相关的操作：

| 算子 | 描述 |

| --- | --- |

| `text_formatter` | 准备和规范化本地文本文件，支持多种文件格式。 |

| `clean_html_mapper` | 去除 `HTML` 标签，提取纯文本内容。 |

| `fix_unicode_mapper` | 修复损坏的 `Unicode` 字符。 |

| `punctuation_normalization_mapper` | 将 `Unicode` 标点标准化为 `ASCII` 标点。 |

| `whitespace_normalization_mapper` | 标准化空白字符为常规 `ASCII` 空格。 |

| `document_deduplicator` | 通过 `MD5` 哈希值在文档级别进行去重。 |

| `document_minhash_deduplicator` | 使用 `MinHashLSH` 在文档级别对样本去重。 |

| `document_simhash_deduplicator` | 使用 `SimHash` 在文档级别对样本去重。 |

| `nlpaug_en_mapper` | 使用 `nlpaug` 库对英文文本进行简单增强。 |

| `nlpcda_zh_mapper` | 使用 `nlpcda` 库对中文文本进行简单增强。 |

| `image_face_blur_mapper` | 对图像中的人脸进行模糊处理。 |

| `image_blur_mapper` | 对图像进行模糊处理。 |

| `image_nsfw_filter` | 过滤掉不合适内容的图像。 |

| `image_watermark_filter` | 检测和排除有水印的图像。 |

| `image_deduplicator` | 精确匹配，在文档级别删除重复的图像。 |

| `image_size_filter` | 保留样本中包含的图片的大小（`bytes`）在指定范围内的样本。 |

| `image_shape_filter` | 保留样本中包含的图片的形状（即宽和高）在指定范围内的样本。 |

通过这些操作，我们可以使用不同算子的组合排列来对数据集进行高效清洗。

那么接下来新的问题来了——我们如何才能对算子的参数了解？这里推荐使用算子的all 参数表：

https://github.com/modelscope/data-juicer/blob/main/configs/config_all.yaml

在完整的 `yaml` 中，我们可以通过修改 `process` 内部的参数，实现不同算子的排列组合，其中每个算子的作用和参数的作用都在写在其中。
