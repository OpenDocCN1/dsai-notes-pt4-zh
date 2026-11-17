
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



## 数据集格式化

```
alpaca_prompt = """Below is an instruction that describes a task, paired with an input that provides further context. Write a response that appropriately completes the request.

### Instruction:
{}

### Input:
{}

### Response:
{}"""

EOS_TOKEN = tokenizer.eos_token # Must add EOS_TOKEN
def formatting_prompts_func(examples):
    instructions = examples["instruction"]
    inputs       = examples["input"]
    outputs      = examples["output"]
    texts = []
    for instruction, input, output in zip(instructions, inputs, outputs):
        # Must add EOS_TOKEN, otherwise your generation will go on forever!
        text = alpaca_prompt.format(instruction, input, output) + EOS_TOKEN
        texts.append(text)
    return { "text" : texts, }
pass

from datasets import load_dataset
dataset = load_dataset("yahma/alpaca-cleaned", split = "train")
dataset = dataset.map(formatting_prompts_func, batched = True,)
```

> 这段 Python 代码主要用于加载一个数据集，并对数据集中的每个样本进行格式化。

> 首先，定义了一个字符串 `alpaca_prompt`，它是一个模板，用于格式化数据集中的每个样本。这个模板包含三个占位符 {}，分别对应于指令、输入和响应。

> 然后，定义了一个变量 `EOS_TOKEN`，它是分词器的结束符号。这个符号用于标记文本的结束，防止生成的文本无限制地进行下去。

> 接下来，定义了一个函数 `formatting_prompts_func`，这个函数接受一个包含指令、输入和输出的样本，然后使用 `alpaca_prompt` 模板和 `EOS_TOKEN` 来格式化这个样本。这个函数返回一个字典，其中包含一个键 `text`，对应的值是一个包含所有格式化后的文本的列表。

> 然后，使用 `datasets` 库的 `load_dataset` 函数加载了一个名为 `yahma/alpaca-cleaned` 的数据集，并选择了训练集。

> 最后，使用 `map` 方法对数据集中的每个样本应用 `formatting_prompts_func` 函数。这个方法的 `batched` 参数设置为 `True`，表示一次处理多个样本。这样可以提高处理速度。

## 训练模型

> Now let's [use Huggingf](https://colab.research.google.com/corgiredirector?site=https%3A%2F%2Fhuggingface.co%2Fdocs%2Ftrl%2Fsft_trainer)ace TRL's `SFTTrainer`! More docs here: TRL SFT docs. We do 60 steps to speed things up, but you can set `num_train_epochs=1` for a full run, and turn off `max_steps=None`. We also support TRL's `DPOTrainer`!

> 现在让我们使用Huggingface TRL的`SFTTrainer`！更多文档请参考：TRL SFT文档。为了加快速度，我们进行60个步骤，但您可以将`num_train_epochs`设置为1以完成全部运行，并关闭`max_steps=None`。我们还支持TRL的`DPOTrainer`！

```
from trl import SFTTrainer
from transformers import TrainingArguments

trainer = SFTTrainer(
    model = model,
    tokenizer = tokenizer,
    train_dataset = dataset,
    dataset_text_field = "text",
    max_seq_length = max_seq_length,
    dataset_num_proc = 2,
    packing = False, # Can make training 5x faster for short sequences.
    args = TrainingArguments(
        per_device_train_batch_size = 2,
        gradient_accumulation_steps = 4,
        warmup_steps = 5,
        max_steps = 60,
        learning_rate = 2e-4,
        fp16 = not torch.cuda.is_bf16_supported(),
        bf16 = torch.cuda.is_bf16_supported(),
        logging_steps = 1,
        optim = "adamw_8bit",
        weight_decay = 0.01,
        lr_scheduler_type = "linear",
        seed = 3407,
        output_dir = "outputs",
    ),
)
```

> 这段 Python 代码是用于设置和初始化一个用于训练语言模型的训练器。

> 首先，从 `trl` 库中导入了 `SFTTrainer` 类，这是一个用于训练语言模型的训练器。同时，从 `transformers` 库中导入了 `TrainingArguments` 类，这是一个用于设置训练参数的类。

> 然后，创建了一个 `SFTTrainer` 对象，并设置了以下参数：

+ `model`：要训练的模型。

+ `tokenizer`：用于处理文本的分词器。

+ `train_dataset`：用于训练的数据集。

+ `dataset_text_field`：数据集中包含文本的字段的名称。

+ `max_seq_length`：模型可以处理的最大序列长度。

+ `dataset_num_proc`：用于数据预处理的进程数。

+ `packing`：是否使用序列打包来提高训练速度。这里设置为 `False`，表示不使用序列打包。

> 接下来，创建了一个 `TrainingArguments` 对象，并设置了以下参数：

+ `per_device_train_batch_size`：每个设备的训练批处理大小。

+ `gradient_accumulation_steps`：梯度累积的步数。

+ `warmup_steps`：预热的步数。

+ `max_steps`：训练的最大步数。

+ `learning_rate`：学习率。

+ `fp16`：是否使用 16 位浮点数进行训练。这里的值取决于设备是否支持 `bfloat16` 类型。

+ `bf16`：是否使用 `bf16` 类型进行训练。这里的值取决于设备是否支持 `bfloat16` 类型。

+ `logging_steps`：记录日志的步数。

+ `optim`：优化器的类型。

+ `weight_decay`：权重衰减的系数。

+ `lr_scheduler_type`：学习率调度器的类型。

+ `seed`：随机数生成器的种子。

+ `output_dir`：输出目录，用于保存训练结果。

> 最后，将 `TrainingArguments` 对象作为 `args` 参数传递给 `SFTTrainer` 对象。

### 显示当前内存统计

```
#@title Show current memory stats
gpu_stats = torch.cuda.get_device_properties(0)
start_gpu_memory = round(torch.cuda.max_memory_reserved() / 1024 / 1024 / 1024, 3)
max_memory = round(gpu_stats.total_memory / 1024 / 1024 / 1024, 3)
print(f"GPU = {gpu_stats.name}. Max memory = {max_memory} GB.")
print(f"{start_gpu_memory} GB of memory reserved.")
```

> 这段 Python 代码用于获取并显示当前 GPU 的内存状态。

> 首先，使用 `torch.cuda.get_device_properties(0)` 获取第一个（索引为0）GPU的属性，这些属性包括 GPU 的名称、总内存等。

> 然后，使用 `torch.cuda.max_memory_reserved()` 获取当前已经预留（但不一定全部使用）的 GPU 内存量。这个值被转换为 GB（Gigabytes）并四舍五入到小数点后三位。

> 接着，从 GPU 的属性中获取总内存量，这个值也被转换为 GB 并四舍五入到小数点后三位。

> 最后，使用 `print` 函数打印 GPU 的名称、总内存量以及已预留的内存量。

```
trainer_stats = trainer.train()
```

> 这段 Python 代码是用于开始训练模型的。

> `trainer` 是一个 `SFTTrainer` 对象，它是 `trl` 库中的一个类，用于训练语言模型。`SFTTrainer` 类的 `train` 方法用于开始训练过程。

> `train` 方法没有参数，它会使用在创建 `SFTTrainer` 对象时设置的所有参数（例如，模型、分词器、训练数据集等）。

> `train` 方法返回一个字典，其中包含了训练过程的一些统计信息，例如，训练的总时间、每个步骤的平均时间、每个步骤的平均损失等。这个字典被赋值给 `trainer_stats` 变量，以便后续使用。

### 显示最终的内存和时间统计数据

```
#@title Show final memory and time stats
used_memory = round(torch.cuda.max_memory_reserved() / 1024 / 1024 / 1024, 3)
used_memory_for_lora = round(used_memory - start_gpu_memory, 3)
used_percentage = round(used_memory / max_memory * 100, 3)
lora_percentage = round(used_memory_for_lora / max_memory * 100, 3)
print(f"{trainer_stats.metrics['train_runtime']} seconds used for training.")
print(f"{round(trainer_stats.metrics['train_runtime'] / 60, 2)} minutes used for training.")
print(f"Peak reserved memory = {used_memory} GB.")
print(f"Peak reserved memory for training = {used_memory_for_lora} GB.")
print(f"Peak reserved memory % of max memory = {used_percentage} %.")
print(f"Peak reserved memory for training % of max memory = {lora_percentage} %.")
```

> 这段 Python 代码用于显示训练过程中的内存使用和时间消耗。

> 首先，使用 `torch.cuda.max_memory_reserved()` 获取当前已经预留（但不一定全部使用）的 GPU 内存量。这个值被转换为 GB（Gigabytes）并四舍五入到小数点后三位，然后赋值给 `used_memory`。

> 然后，计算训练过程中增加的内存使用量，即 `used_memory` 减去开始时的内存量 `start_gpu_memory`，结果赋值给 `used_memory_for_lora`。

> 接着，计算内存使用量占总内存的百分比，即 `used_memory` 除以 `max_memory`，结果赋值给 `used_percentage`。同样，计算训练过程中增加的内存使用量占总内存的百分比，结果赋值给 `lora_percentage`。

> 最后，使用 `print` 函数打印训练时间、内存使用量以及它们占总内存的百分比。这些信息可以帮助我们了解训练过程的资源消耗情况。

## 推理

让我们运行模型！您可以更改指令和输入 - 将输出留空！

```
# alpaca_prompt = Copied from above
FastLanguageModel.for_inference(model) # Enable native 2x faster inference
inputs = tokenizer(
[
    alpaca_prompt.format(
        "Continue the fibonnaci sequence.", # instruction
        "1, 1, 2, 3, 5, 8", # input
        "", # output - leave this blank for generation!
    )
], return_tensors = "pt").to("cuda")

outputs = model.generate(**inputs, max_new_tokens = 64, use_cache = True)
tokenizer.batch_decode(outputs)
```

> 这段 Python 代码是用于生成新的文本，具体来说，是继续斐波那契数列。

> 首先，使用 `FastLanguageModel.for_inference(model)` 方法对模型进行优化，以便在推理时能够更快地运行。

> 然后，创建一个输入，这个输入是一个列表，其中包含一个字符串。这个字符串是使用 `alpaca_prompt` 模板格式化的，其中的指令是 "Continue the fibonnaci sequence."，输入是 "1, 1, 2, 3, 5, 8"，输出是空字符串（因为我们希望模型生成输出）。这个输入被分词器处理，并转换为张量，然后被移动到 GPU 上。

> 接着，使用模型的 `generate` 方法生成新的文本。这个方法的参数包括：

+ `inputs`：上面创建的输入。

+ `max_new_tokens`：生成的新令牌的最大数量。这里设置为 64，表示最多生成 64 个新令牌。

+ `use_cache`：是否使用缓存来提高生成速度。这里设置为 `True`，表示使用缓存。

> `generate` 方法返回一个包含生成的令牌的张量。

> 最后，使用分词器的 `batch_decode` 方法将生成的令牌解码为文本。这个方法返回一个包含生成的文本的列表。

您还可以使用`TextStreamer`进行连续推理-这样您就可以逐个标记地查看生成的结果，而不必一直等待。

```
# alpaca_prompt = Copied from above
FastLanguageModel.for_inference(model) # Enable native 2x faster inference
inputs = tokenizer(
[
    alpaca_prompt.format(
        "Continue the fibonnaci sequence.", # instruction
        "1, 1, 2, 3, 5, 8", # input
        "", # output - leave this blank for generation!
    )
], return_tensors = "pt").to("cuda")

from transformers import TextStreamer
text_streamer = TextStreamer(tokenizer)
_ = model.generate(**inputs, streamer = text_streamer, max_new_tokens = 128)
```

> 这段 Python 代码用于生成新的文本，具体来说，是继续斐波那契数列。



# 模型推理与生成

首先，使用 `FastLanguageModel.for_inference(model)` 方法对模型进行优化，以便在推理时能够更快地运行。

然后，创建一个输入，这个输入是一个列表，其中包含一个字符串。这个字符串是使用 `alpaca_prompt` 模板格式化的，其中的指令是 "Continue the fibonnaci sequence."，输入是 "1, 1, 2, 3, 5, 8"，输出是空字符串（因为我们希望模型生成输出）。这个输入被分词器处理，并转换为张量，然后被移动到 GPU 上。

接着，从 `transformers` 库中导入 `TextStreamer` 类，并创建一个 `TextStreamer` 对象。`TextStreamer` 是一个用于处理生成的文本的工具，它可以将生成的令牌流式地解码为文本。

然后，使用模型的 `generate` 方法生成新的文本。这个方法的参数包括：

+   `inputs`：上面创建的输入。
+   `streamer`：上面创建的 `TextStreamer` 对象。
+   `max_new_tokens`：生成的新令牌的最大数量。这里设置为 128，表示最多生成 128 个新令牌。

`generate` 方法返回一个包含生成的令牌的张量。这个张量被赋值给 `_`，表示我们不关心这个返回值，因为生成的文本已经被 `TextStreamer` 对象处理并显示出来了。

# 模型优化后的保存和加载

## 保存为 Lora 格式

将最终模型保存为 LoRA 适配器，可以使用 Huggingface 的 `push_to_hub` 进行在线保存，或者使用 `save_pretrained` 进行本地保存。

[注意] 此操作仅保存 LoRA 适配器，并不包括完整的模型。要进行 16bit 或 GGUF 的保存，请向下滚动！

```
model.save_pretrained("lora_model") # Local saving
tokenizer.save_pretrained("lora_model")
# model.push_to_hub("your_name/lora_model", token = "...") # Online saving
# tokenizer.push_to_hub("your_name/lora_model", token = "...") # Online saving
```

这段 Python 代码用于保存训练好的模型和分词器。

首先，使用模型的 `save_pretrained` 方法将模型保存到本地。这个方法的参数是一个字符串，表示保存模型的目录的路径。这里，模型被保存到 `lora_model` 目录。

然后，使用分词器的 `save_pretrained` 方法将分词器保存到本地。这个方法的参数也是一个字符串，表示保存分词器的目录的路径。这里，分词器也被保存到 `lora_model` 目录。

接下来的两行代码被注释掉了，它们是用于将模型和分词器保存到 Hugging Face Model Hub 的。如果你想要将模型和分词器保存到 Model Hub，你需要取消这两行代码的注释，并替换 `your_name/lora_model` 为你在 Model Hub 上的用户名和模型的名称，同时替换 `...` 为你的 Hugging Face API 令牌。

现在，如果您想加载我们刚保存的LoRA适配器进行推理，请将`False`设置为`True`：

```
if False:
    from unsloth import FastLanguageModel
    model, tokenizer = FastLanguageModel.from_pretrained(
        model_name = "lora_model", # YOUR MODEL YOU USED FOR TRAINING
        max_seq_length = max_seq_length,
        dtype = dtype,
        load_in_4bit = load_in_4bit,
    )
    FastLanguageModel.for_inference(model) # Enable native 2x faster inference

# alpaca_prompt = You MUST copy from above!

inputs = tokenizer(
[
    alpaca_prompt.format(
        "What is a famous tall tower in Paris?", # instruction
        "", # input
        "", # output - leave this blank for generation!
    )
], return_tensors = "pt").to("cuda")

outputs = model.generate(**inputs, max_new_tokens = 64, use_cache = True)
tokenizer.batch_decode(outputs)
```

这段 Python 代码用于加载预训练的模型和分词器，并使用它们生成新的文本。

首先，有一个被设置为 `False` 的 `if` 语句，这意味着这个 `if` 语句块中的代码不会被执行。这个语句块中的代码是用于从本地加载预训练的模型和分词器的。如果你想要执行这个语句块中的代码，你需要将 `False` 改为 `True`。

然后，创建一个输入，这个输入是一个列表，其中包含一个字符串。这个字符串是使用 `alpaca_prompt` 模板格式化的，其中的指令是 "What is a famous tall tower in Paris?"，输入和输出都是空字符串（因为我们希望模型生成输出）。这个输入被分词器处理，并转换为张量，然后被移动到 GPU 上。

接着，使用模型的 `generate` 方法生成新的文本。这个方法的参数包括：

+   `inputs`：上面创建的输入。
+   `max_new_tokens`：生成的新令牌的最大数量。这里设置为 64，表示最多生成 64 个新令牌。
+   `use_cache`：是否使用缓存来提高生成速度。这里设置为 `True`，表示使用缓存。

`generate` 方法返回一个包含生成的令牌的张量。

最后，使用分词器的 `batch_decode` 方法将生成的令牌解码为文本。这个方法返回一个包含生成的文本的列表。

您还可以使用Hugging Face的`AutoModelForPeftCausalLM`。只有在未安装`unsloth`的情况下才使用此选项。由于不支持4bit模型下载，它可能会非常慢，而且`Unsloth`的推理速度是其两倍。

```
if False:
    # I highly do NOT suggest - use Unsloth if possible
    from peft import AutoPeftModelForCausalLM
    from transformers import AutoTokenizer
    model = AutoPeftModelForCausalLM.from_pretrained(
        "lora_model", # YOUR MODEL YOU USED FOR TRAINING
        load_in_4bit = load_in_4bit,
    )
    tokenizer = AutoTokenizer.from_pretrained("lora_model")
```

这段 Python 代码是用于从本地加载预训练的模型和分词器的，但是它被设置在一个 `if False:` 语句块中，这意味着这段代码默认是不会被执行的。

首先，从 `peft` 包中导入 `AutoPeftModelForCausalLM` 类，这是一个用于因果语言模型的类。然后，从 `transformers` 包中导入 `AutoTokenizer` 类，这是一个用于自动加载分词器的类。

接着，使用 `AutoPeftModelForCausalLM.from_pretrained` 方法从本地加载预训练的模型。这个方法的参数包括：

+   `"lora_model"`：模型的名称。这应该是你在训练模型时保存模型的目录的名称。
+   `load_in_4bit`：是否以4位的方式加载模型。这是一个布尔值，如果为 `True`，则模型将以4位的方式加载，这可以减少内存使用量，但可能会降低模型的精度。

然后，使用 `AutoTokenizer.from_pretrained` 方法从本地加载预训练的分词器。这个方法的参数是分词器的名称，这应该是你在训练模型时保存分词器的目录的名称。

这段代码的作者在注释中明确表示不建议使用这种方式加载模型和分词器，而建议使用 `Unsloth`（可能是一个工具或者库）来加载。

## 保存为float16以供VLLM使用

我们还支持直接保存为float16。选择`merged_16bit`以获取float16，或者选择`merged_4bit`以获取int4。我们还允许作为备选项的`lora`适配器。使用`push_to_hub_merged`将其上传到您的Hugging Face账户！您可以访问https://huggingface.co/settings/tokens获取个人令牌。

```
# Merge to 16bit
if False: model.save_pretrained_merged("model", tokenizer, save_method = "merged_16bit",)
if False: model.push_to_hub_merged("hf/model", tokenizer, save_method = "merged_16bit", token = "")

# Merge to 4bit
if False: model.save_pretrained_merged("model", tokenizer, save_method = "merged_4bit",)
if False: model.push_to_hub_merged("hf/model", tokenizer, save_method = "merged_4bit", token = "")

# Just LoRA adapters
if False: model.save_pretrained_merged("model", tokenizer, save_method = "lora",)
if False: model.push_to_hub_merged("hf/model", tokenizer, save_method = "lora", token = "")
```

这段 Python 代码是用于保存训练好的模型和分词器的，但所有的保存操作都被设置在 `if False:` 语句块中，这意味着这些代码默认是不会被执行的。

这段代码提供了三种不同的保存方法：

1. `merged_16bit`：这种方法将模型保存为16位的格式。这可以减少模型的大小，但可能会降低模型的精度。
2. `merged_4bit`：这种方法将模型保存为4位的格式。这可以进一步减少模型的大小，但可能会进一步降低模型的精度。
3. `lora`：这种方法只保存模型的 LoRA（Low-Rank Adaptation）适配器。这可以大大减少模型的大小，但只有当你只关心 LoRA 适配器时才适用。

每种保存方法都有两个步骤：首先，使用 `model.save_pretrained_merged` 方法将模型保存到本地；然后，使用 `model.push_to_hub_merged` 方法将模型推送到 Hugging Face Model Hub。

这些方法的参数包括：

+   `"model"` 或 `"hf/model"`：保存或推送模型的目录的路径。
+   `tokenizer`：分词器。
+   `save_method`：保存方法，可以是 `"merged_16bit"`、`"merged_4bit"` 或 `"lora"`。
+   `token`：你的 Hugging Face API 令牌，用于推送模型到 Model Hub。

如果你想要执行这些保存操作，你需要将 `False` 改为 `True`，并替换 `"model"`、`"hf/model"` 和 `""` 为你的实际路径和令牌。

## 转化为 GGUF / llama.cpp

为了保存到GGUF / llama.cpp，我们现在原生支持它！我们克隆`llama.cpp`，并将其默认保存为`q8_0`。我们允许所有的方法，如`q4_k_m`。使用`save_pretrained_gguf`进行本地保存，使用`push_to_hub_gguf`上传到HF。

一些支持的量化方法（完整列表请参考我们的维基页面）：

+   `q8_0` - 快速转换。资源消耗高，但通常可接受。
+   `q4_k_m` - 推荐使用。对`attention.wv`和`feed_forward.w2`张量的一半使用`Q6_K`，其他情况下使用`Q4_K`。
+   `q5_k_m` - 推荐使用。对`attention.wv`和`feed_forward.w2`张量的一半使用`Q6_K`，其他情况下使用`Q5_K`。

```
# Save to 8bit Q8_0
if False: model.save_pretrained_gguf("model", tokenizer,)
if False: model.push_to_hub_gguf("hf/model", tokenizer, token = "")

# Save to 16bit GGUF
if False: model.save_pretrained_gguf("model", tokenizer, quantization_method = "f16")
if False: model.push_to_hub_gguf("hf/model", tokenizer, quantization_method = "f16", token = "")

# Save to q4_k_m GGUF
if False: model.save_pretrained_gguf("model", tokenizer, quantization_method = "q4_k_m")
if False: model.push_to_hub_gguf("hf/model", tokenizer, quantization_method = "q4_k_m", token = "")
```

这段 Python 代码是用于保存训练好的模型和分词器的，但所有的保存操作都被设置在 `if False:` 语句块中，这意味着这些代码默认是不会被执行的。

这段代码提供了三种不同的保存方法：

1. `8bit Q8_0`：这种方法将模型保存为8位的格式。这可以减少模型的大小，但可能会降低模型的精度。
2. `16bit GGUF`：这种方法将模型保存为16位的格式。这可以进一步减少模型的大小，但可能会进一步降低模型的精度。
3. `q4_k_m GGUF`：这种方法将模型保存为4位的格式，但使用了一种特殊的量化方法（可能是一种名为 `"q4_k_m"` 的方法）。这可以大大减少模型的大小，但可能会大大降低模型的精度。

每种保存方法都有两个步骤：首先，使用 `model.save_pretrained_gguf` 方法将模型保存到本地；然后，使用 `model.push_to_hub_gguf` 方法将模型推送到 Hugging Face Model Hub。

这些方法的参数包括：

+   `"model"` 或 `"hf/model"`：保存或推送模型的目录的路径。
+   `tokenizer`：分词器。
+   `quantization_method`：量化方法，可以是 `"f16"` 或 `"q4_k_m"`。
+   `token`：你的 Hugging Face API 令牌，用于推送模型到 Model Hub。

如果你想要执行这些保存操作，你需要将 `False` 改为 `True`，并替换 `"model"`、`"hf/model"` 和 `""` 为你的实际路径和令牌。

# 本地微调LLaMa3

## 环境准备

### 安装conda

按照以下步骤来安装 `miniconda`：

1.  [请首先下载 `miniconda` 安装脚本](https://docs.conda.io/en/latest/miniconda.html)。你可以访问 Miniconda 来选择匹配你操作系统的安装脚本。

1.  让我们假设你的操作系统是 64位的 Linux，你可以通过以下命令下载 `miniconda`：

```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

1.  为了运行这个脚本，你需要使其成为可执行的。你可以使用 `chmod` 命令来改变这个脚本的权限：

```
chmod +x Miniconda3-latest-Linux-x86_64.sh
```

1.  现在你可以运行这个脚本来安装 `miniconda` 了：

```
./Miniconda3-latest-Linux-x86_64.sh
```

1.  脚本启动后，只需要按照屏幕上的提示步骤即可完成安装。



安装完成后，你可能需要重启你的 shell 或者整个系统来确保 `path` 变量已经更新，然后你就可以使用 `conda` 命令了。

## 安装unsloth

```bash
conda create --name unsloth_env python=3.10
```

> 这个命令的作用是使用`conda`创建一个新的Python环境，名称为`unsloth_env`，并且在这个环境中的Python版本为3.10。通常，我们创建一个独立的环境以便于管理项目的依赖，并防止它们与其他项目的依赖发生冲突。

```bash
conda activate unsloth_env
```

> 该命令的目的是激活刚刚创建的环境`unsloth_env`。在命令执行之后，你将在`unsloth_env`环境中，这意味着任何你安装的Python包或者库都会在这个环境中安装，而不会影响到其他环境。

```bash
conda install pytorch-cuda=12.1 pytorch cudatoolkit xformers -c pytorch -c nvidia -c xformers
```

> 这个命令将在你激活的环境（在这里是`unsloth_env`）中安装several Python包：

+   `pytorch`: 一个开源的深度学习框架，广泛用于各种人工智能项目。

+   `pytorch-cuda=12.1`: 这是PyTorch的CUDA版本，方便在支持CUDA的硬件上进行GPU加速运算。

+   `cudatoolkit`: CUDA Toolkit是一个开发者工具包，用以在Nvidia的GPU上进行并行编程。

+   `xformers`: 一个神经网络库，用于实现Transformer模型。

+   `-c pytorch -c nvidia -c xformers`：这些指定了安装包的来源频道。

```bash
pip install "unsloth[colab-new] @ git+https://github.com/unslothai/unsloth.git"
```

> 这是一个`pip`命令，用以安装来自GitHub仓库的`unsloth`项目，并且指定其附加的安装选项`colab-new`。这条命令将从GitHub的仓库中克隆项目，并且根据项目配置安装所需的Python包。

## 创建jupyter venv

1.  你需要在这个环境中安装 `ipykernel`，这是一个提供 Jupyter 与 Python 内核交互的软件包：

    ```bash
    conda install ipykernel
    ```

2.  安装完 `ipykernel` 之后，你需要创建一个指向你的 Conda 环境的 Jupyter 内核：

    ```bash
    python -m ipykernel install --user --name=unsloth_env
    ```

    这里 `--name=` 后面的就是你想要用的名字，可以和 Conda 环境的名字保持一致。

完成以上步骤后，你就可以在 Jupyter Notebook 界面的内核列表中看到你的 Conda 环境 `unsloth_env` 了。通过在 Jupyter Notebook 中选择相应的内核，你就能使用这个特定的 Conda 环境了。

## 下载映像

```bash
git clone https://hf-mirror.com/unsloth/llama-3-8b-bnb-4bit
```

## 训练

```python
from unsloth import FastLanguageModel
import torch

from trl import SFTTrainer
from transformers import TrainingArguments


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

alpaca_prompt = """Below is an instruction that describes a task, paired with an input that provides further context. Write a response that appropriately completes the request.

### Instruction:
{}

### Input:
{}

### Response:
{}"""

EOS_TOKEN = tokenizer.eos_token # Must add EOS_TOKEN
def formatting_prompts_func(examples):
    instructions = examples["instruction"]
    inputs       = examples["input"]
    outputs      = examples["output"]
    texts = []
    for instruction, input, output in zip(instructions, inputs, outputs):
        # Must add EOS_TOKEN, otherwise your generation will go on forever!
        text = alpaca_prompt.format(instruction, input, output) + EOS_TOKEN
        texts.append(text)
    return { "text" : texts, }
pass

from datasets import load_dataset

file_path = "/home/Ubuntu/alpaca_data_cleaned.json"


dataset = load_dataset("json", data_files={"train": file_path}, split="train")

dataset = dataset.map(formatting_prompts_func, batched = True,)


trainer = SFTTrainer(
    model = model,
    tokenizer = tokenizer,
    train_dataset = dataset,
    dataset_text_field = "text",
    max_seq_length = max_seq_length,
    dataset_num_proc = 2,
    packing = False, # Can make training 5x faster for short sequences.
    args = TrainingArguments(
        per_device_train_batch_size = 2,
        gradient_accumulation_steps = 4,
        warmup_steps = 5,
        max_steps = 60,
        learning_rate = 2e-4,
        fp16 = not torch.cuda.is_bf16_supported(),
        bf16 = torch.cuda.is_bf16_supported(),
        logging_steps = 1,
        optim = "adamw_8bit",
        weight_decay = 0.01,
        lr_scheduler_type = "linear",
        seed = 3407,
        output_dir = "outputs",
    ),
)

trainer_stats = trainer.train()


# alpaca_prompt = Copied from above
FastLanguageModel.for_inference(model) # Enable native 2x faster inference
inputs = tokenizer(
[
    alpaca_prompt.format(
        "Continue the fibonnaci sequence.", # instruction
        "1, 1, 2, 3, 5, 8", # input
        "", # output - leave this blank for generation!
    )
], return_tensors = "pt").to("cuda")

from transformers import TextStreamer
text_streamer = TextStreamer(tokenizer)
_ = model.generate(**inputs, streamer = text_streamer, max_new_tokens = 128)

model.save_pretrained("lora_model") # Local saving


inputs = tokenizer(
[
    alpaca_prompt.format(
        "介绍AI超元域频道", # instruction
        "", # input
        "", # output - leave this blank for generation!
    )
], return_tensors = "pt").to("cuda")

from transformers import TextStreamer
text_streamer = TextStreamer(tokenizer)
_ = model.generate(**inputs, streamer = text_streamer, max_new_tokens = 128)
```

## 推理



```python
from unsloth import FastLanguageModel
import torch

from trl import SFTTrainer
from transformers import TrainingArguments

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

if True:
    from unsloth import FastLanguageModel
    model, tokenizer = FastLanguageModel.from_pretrained(
        model_name = "lora_model", # YOUR MODEL YOU USED FOR TRAINING
        max_seq_length = max_seq_length,
        dtype = dtype,
        load_in_4bit = load_in_4bit,
    )
    FastLanguageModel.for_inference(model) # Enable native 2x faster inference

alpaca_prompt = """Below is an instruction that describes a task, paired with an input that provides further context. Write a response that appropriately completes the request.

### Instruction:
{}

### Input:
{}

### Response:
{}

"""

inputs = tokenizer(
[
    alpaca_prompt.format(
        "介绍AI超元域频道", # instruction
        "", # input
        "", # output - leave this blank for generation!
    )
], return_tensors = "pt").to("cuda")

from transformers import TextStreamer
text_streamer = TextStreamer(tokenizer)
_ = model.generate(**inputs, streamer = text_streamer, max_new_tokens = 128)
```
