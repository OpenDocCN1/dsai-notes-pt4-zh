
# 微调大语言模型（LLaMa Unsloth）

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/NXs3dZXDqowaSrxtPjlc5NNqn8c](https://zsyhjtnsa5.feishu.cn/docx/NXs3dZXDqowaSrxtPjlc5NNqn8c)

## unsloth代码解读

- [GitHub Repo](https://github.com/unslothai/unsloth)
- [`Alpaca + Llama-3 8b full example.ipynb`](https://colab.research.google.com/drive/135ced7oHytdxu3N2DNe1Z0kqjyYIkDXp?usp=sharing)

### 环境初始化

To install Unsloth on your own computer, follow the [installation instructions](https://github.com/unslothai/unsloth#installation-instructions---conda) on our Github page.

You will learn how to do data prep, how to train, how to run the model, & how to save it (eg for `Llama.cpp`).

- We support Llama, Mistral, CodeLlama, TinyLlama, Vicuna, Open Hermes, etc.
- And Yi, Qwen ([llamafied](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fhuggingface.co%2Fmodels%3Fsort%3Dtrending%26search%3Dqwen%2Bllama)), Deepseek, all Llama, Mistral derived architectures.
- We support 16bit LoRA or 4bit QLoRA. Both 2x faster.
- `max_seq_length` can be set to anything, since we do automatic RoPE Scaling via kaiokendev's method.
- [NEW] With [PR 26037](https://github.com/huggingface/transformers/pull/26037), we support downloading [4bit models](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fhuggingface.co%2Funsloth) 4x faster! Our repo has Llama, Mistral 4bit models.

```
%%capture
# Installs Unsloth, Xformers (Flash Attention) and all other packages!
!pip install "unsloth[colab-new] @ git+https://github.com/unslothai/unsloth.git"
!pip install --no-deps "xformers<0.0.26" trl peft accelerate bitsandbytes
```

> 这段代码是在 Jupyter Notebook 中运行的，用于安装一些 Python 包。这里使用了 `pip`，Python 的包管理器，来安装包。
>
> `%%capture` 是一个 Jupyter Notebook 的魔术命令，用于捕获单元格的输出。这意味着安装过程中的所有输出（例如，进度条或警告）都不会显示在笔记本中。
>
> `!pip install "unsloth[colab-new] @ git+https://github.com/unslothai/unsloth.git"` 这行代码从 GitHub 仓库安装了名为 `unsloth` 的包。`[colab-new]` 是一个可选的包，只有在特定环境（如 Google Colab）中才需要。
>
> `!pip install --no-deps "xformers<0.0.26" trl peft accelerate bitsandbytes` 这行代码安装了几个包，但是使用 `--no-deps` 选项，这意味着 `pip` 不会尝试安装这些包的任何依赖项。这可能是因为这些依赖项已经在环境中安装了，或者用户希望手动管理它们。这行代码安装的包包括 `xformers`（版本小于 0.0.26）、`trl`、`peft`、`accelerate` 和 `bitsandbytes`。

```
from unsloth import FastLanguageModel
import torch
max_seq_length = 2048 # Choose any! We auto support RoPE Scaling internally!
dtype = None # None for auto detection. Float16 for Tesla T4, V100, Bfloat16 for Ampere+
load_in_4bit = True # Use 4bit quantization to reduce memory usage. Can be False.

# 4bit pre quantized models we support for 4x faster downloading + no OOMs.
fourbit_models = [
    "unsloth/mistral-7b-bnb-4bit",
    "unsloth/mistral-7b-instruct-v0.2-bnb-4bit",
    "unsloth/llama-2-7b-bnb-4bit",
    "unsloth/gemma-7b-bnb-4bit",
    "unsloth/gemma-7b-it-bnb-4bit", # Instruct version of Gemma 7b
    "unsloth/gemma-2b-bnb-4bit",
    "unsloth/gemma-2b-it-bnb-4bit", # Instruct version of Gemma 2b
    "unsloth/llama-3-8b-bnb-4bit", # [NEW] 15 Trillion token Llama-3
] # More models at https://huggingface.co/unsloth

model, tokenizer = FastLanguageModel.from_pretrained(
    model_name = "unsloth/llama-3-8b-bnb-4bit",
    max_seq_length = max_seq_length,
    dtype = dtype,
    load_in_4bit = load_in_4bit,
    # token = "hf_...", # use one if using gated models like meta-llama/Llama-2-7b-hf
)
```

> 这段 Python 代码主要用于加载和初始化一个预训练的语言模型。这里使用的是 `unsloth` 库，这是一个用于加载和使用大型语言模型的库。
>
> 首先，代码从 `unsloth` 库中导入了 `FastLanguageModel` 类，并导入了 `torch` 库。
>
> 然后，设置了一些参数：
>
> - `max_seq_length`：这是模型可以处理的最大序列长度。这里设置为 2048。
> - `dtype`：这是模型使用的数据类型。这里设置为 `None`，表示自动检测。也可以设置为 `Float16` 或 `Bfloat16`。
> - `load_in_4bit`：这是一个布尔值，表示是否使用 4 位量化来减少内存使用。这里设置为 `True`。
>
> 接下来，定义了一个列表 `fourbit_models`，其中包含了可以使用 4 位预量化模型的名称。这些模型可以更快地下载，并且不会出现内存溢出的问题。
>
> 最后，使用 `FastLanguageModel.from_pretrained` 方法加载并初始化了一个预训练的语言模型。这个方法需要以下参数：
>
> - `model_name`：要加载的模型的名称。这里使用的是 `"unsloth/llama-3-8b-bnb-4bit"`。
> - `max_seq_length`：模型可以处理的最大序列长度。
> - `dtype`：模型使用的数据类型。
> - `load_in_4bit`：是否使用 4 位量化。
>
> 这个方法返回两个对象：`model` 和 `tokenizer`。`model` 是加载的模型，`tokenizer` 是用于处理文本的分词器。

我们现在添加了 LoRA 适配器，所以我们只需要更新所有参数的 1 到 10％！

```
model = FastLanguageModel.get_peft_model(
    model,
    r = 16, # Choose any number > 0 ! Suggested 8, 16, 32, 64, 128
    target_modules = ["q_proj", "k_proj", "v_proj", "o_proj",
                      "gate_proj", "up_proj", "down_proj",],
    lora_alpha = 16,
    lora_dropout = 0, # Supports any, but = 0 is optimized
    bias = "none",    # Supports any, but = "none" is optimized
    # [NEW] "unsloth" uses 30% less VRAM, fits 2x larger batch sizes!
    use_gradient_checkpointing = "unsloth", # True or "unsloth" for very long context
    random_state = 3407,
    use_rslora = False,  # We support rank stabilized LoRA
    loftq_config = None, # And LoftQ
)
```

> 这段 Python 代码是使用 `unsloth` 库的 `FastLanguageModel` 类的 `get_peft_model` 方法来修改一个已经加载的语言模型。这个方法的目的是为了优化模型的性能和内存使用。
>
> 以下是这个方法的参数：
>
> - `model`：已经加载的语言模型。
> - `r`：一个大于 0 的整数，用于设置模型的投影维度。建议的值有 8、16、32、64、128。
> - `target_modules`：一个字符串列表，指定要优化的模型组件。
> - `lora_alpha`：LoRA（Low-Rank Adaptation）的超参数，用于控制低秩适应的强度。
> - `lora_dropout`：在 LoRA 层使用的 dropout 率。虽然支持任何值，但是 0 已经被优化过。
> - `bias`：在 LoRA 层使用的偏置。虽然支持任何值，但是 `"none"` 已经被优化过。
> - `use_gradient_checkpointing`：是否使用梯度检查点来减少 VRAM 使用。可以设置为 `True` 或 `"unsloth"`。如果设置为 `"unsloth"`，将使用更少的 VRAM，并且可以适应更大的批处理大小。
> - `random_state`：用于初始化随机数生成器的种子。
> - `use_rslora`：是否使用排名稳定的 LoRA。默认为 `False`。
> - `loftq_config`：LoftQ 的配置。默认为 `None`。
>
> 这个方法返回一个修改后的模型。

### 数据准备

We now use the Alpaca dataset from yahma, which is a filtered version of 52K of the original Alpaca dataset. You can replace this code section with your own data prep.

[NOTE] To train only on completions (ignoring the user's input), read `TRL`'s docs here.

[NOTE] Remember to add `EOS_TOKEN` to the tokenized output!! Otherwise you'll get infinite generations!

If you want to use the `ChatML` template for ShareGPT datasets, try our conversational notebook.

For text completions like novel writing, try this notebook.
