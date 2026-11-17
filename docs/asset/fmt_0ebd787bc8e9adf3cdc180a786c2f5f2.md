
`[HUMAN]`:

user prompt here

`[AI]`:

model response here

`[HUMAN]`:

user prompt here

`[AI]`:

model response here

```
The corresponding code should be:
```

```python
_register_template(

    name="custom",

    format_user=StringFormatter(slots=["[HUMAN]:\n{{content}}\n[AI]:\n"]),

    format_separator=EmptyFormatter(slots=["\n\n"]),

    efficient_eos=True,

)
```

```python
"""
eos_slots = [] if efficient_eos else [{"eos_token"}]
template_class = Llama2Template if name.startswith("llama2") else Template
default_user_formatter = StringFormatter(slots=["{{content}}"])
default_assistant_formatter = StringFormatter(slots=["{{content}}"] + eos_slots)
default_function_formatter = FunctionFormatter(slots=["Action: {{name}}\nAction Input: {{arguments}}"] + eos_slots)
default_tool_formatter = ToolFormatter(tool_format="default")
default_separator_formatter = EmptyFormatter()
templates[name] = template_class(
    format_user=format_user or default_user_formatter,
    format_assistant=format_assistant or default_assistant_formatter,
    format_system=format_system or default_user_formatter,
    format_function=format_function or default_function_formatter,
    format_observation=format_observation or format_user or default_user_formatter,
    format_tools=format_tools or default_tool_formatter,
    format_separator=format_separator or default_separator_formatter,
    default_system=default_system,
    stop_words=stop_words,
    efficient_eos=efficient_eos,
    replace_eos=replace_eos,
    force_system=force_system,
)
```

### 典型的几个模板

```python
_register_template(
    name="alpaca",
    format_user=StringFormatter(slots=["### Instruction:\n{{content}}\n\n### Response:\n"]),
    format_separator=EmptyFormatter(slots=["\n\n"]),
    default_system=(
        "Below is an instruction that describes a task. "
        "Write a response that appropriately completes the request.\n\n"
    ),
)
```

```python
_register_template(
    name="qwen",
    format_user=StringFormatter(slots=["<|im_start|>user\n{{content}}<|im_end|>\n<|im_start|>assistant\n"]),
    format_system=StringFormatter(slots=["<|im_start|>system\n{{content}}<|im_end|>\n"]),
    format_observation=StringFormatter(slots=["<|im_start|>tool\n{{content}}<|im_end|>\n<|im_start|>assistant\n"]),
    format_separator=EmptyFormatter(slots=["\n"]),
    default_system="You are a helpful assistant.",
    stop_words=["<|im_end|>"],
    replace_eos=True,
)
```

```python
_register_template(
    name="chatml",
    format_user=StringFormatter(slots=["<|im_start|>user\n{{content}}<|im_end|>\n<|im_start|>assistant\n"]),
    format_system=StringFormatter(slots=["<|im_start|>system\n{{content}}<|im_end|>\n"]),
    format_observation=StringFormatter(slots=["<|im_start|>tool\n{{content}}<|im_end|>\n<|im_start|>assistant\n"]),
    format_separator=EmptyFormatter(slots=["\n"]),
    stop_words=["<|im_end|>", "<|im_start|>"],
    replace_eos=True,
)
```

```python
_register_template(
    name="deepseek",
    format_user=StringFormatter(slots=["User: {{content}}\n\nAssistant:"]),
    format_system=StringFormatter(slots=[{"bos_token"}, "{{content}}"]),
    force_system=True,
)
```

```python
_register_template(
    name="default",
    format_user=StringFormatter(slots=["Human: {{content}}\nAssistant: "]),
    format_system=StringFormatter(slots=["{{content}}\n"]),
    format_separator=EmptyFormatter(slots=["\n"]),
)
```

```python
_register_template(
    name="llama2",
    format_user=StringFormatter(slots=[{"bos_token"}, " {{content}} "]),
    format_system=StringFormatter(slots=["<<SYS>>\n{{content}}\n<</SYS>>\n\n"]),
    default_system=(
        "You are a helpful, respectful and honest assistant. "
        "Always answer as helpfully as possible, while being safe. "
        "Your answers should not include any harmful, unethical, "
        "racist, sexist, toxic, dangerous, or illegal content. "
        "Please ensure that your responses are socially unbiased and positive in nature.\n\n"
        "If a question does not make any sense, or is not factually coherent, "
        "explain why instead of answering something not correct. "
        "If you don't know the answer to a question, please don't share false information."
    ),
)
```

```python
_register_template(
    name="llama2_zh",
    format_user=StringFormatter(slots=[{"bos_token"}, "評 {{content}} </tool_call>"]),
    format_system=StringFormatter(slots=["<<SYS>>\n{{content}}\n<</SYS>>\n\n"]),
    default_system="You are a helpful assistant. 你是一个乐于助人的助手。",
)
```

```python
_register_template(
    name="llama3",
    format_user=StringFormatter(
        slots=[
            (
                "<|start_header_id|>user<|end_header_id|>\n\n{{content}}<|eot_id|>"
                "<|start_header_id|>assistant<|end_header_id|>\n\n"
            )
        ]
    ),
    format_system=StringFormatter(
        slots=[{"bos_token"}, "<|start_header_id|>system<|end_header_id|>\n\n{{content}}<|eot_id|>"]
    ),
    format_observation=StringFormatter(
        slots=[
            (
                "<|start_header_id|>tool<|end_header_id|>\n\n{{content}}<|eot_id|>"
                "<|start_header_id|>assistant<|end_header_id|>\n\n"
            )
        ]
    ),
    default_system="You are a helpful assistant.",
    stop_words=["<|eot_id|>"],
    replace_eos=True,
)
```

### 2.3 安装依赖

在一个干净的虚拟环境，安装如下依赖

```bash
git clone https://github.com/hiyouga/LLaMA-Factory.git
conda create -n llama_factory python=3.10
conda activate llama_factory
cd LLaMA-Factory
pip install -e .[metrics]
```

可选的额外依赖项：`deepspeed`、`metrics`、`unsloth`、`galore`、`badam`、`vllm`、`bitsandbytes`、`gptq`、`awq`、`aqlm`、`qwen`、`modelscope`、`quality`

### 2.4 WEB-UI 训练

[LlamaFactory Colab Demo脚本](https://link.zhihu.com/?target=https%3A//colab.research.google.com/drive/1eRTPn37ltBbYsISy9Aw2NuI2Aq5CQrD9%3Fusp%3Dsharing) 脚本: https://colab.research.google.com/drive/1eRTPn37ltBbYsISy9Aw2NuI2Aq5CQrD9?usp=sharing

`ui` 界面目前只支持单卡训练

#### 模型选择

![](img/6ffa60e4d7edc70e0228538bb3407216.png)

#### 数据集选择配置

![](img/1fa0c4a9df1c30407ae356dd1e6f6b9b.png)

#### 训练方式选择

![](img/96ea2cdb72a00f037f90f6073af5d466.png)

#### 训练参数配置

![](img/1c19f14224d68bce89ed7ee559edbe21.png)

#### 预览训练命令

![](img/0110f8f936ce004fa283e173ea1655d9.png)

#### 日志展示

![](img/530dfe795e2cdeee116e2d0b2ad1948f.png)

![](img/7112cf428d6fc43eea2c727ced5eb254.png)

![](img/efc62be3d7fca8398a6973a1de6470f7.png)

### 2.5 命令脚本训练

目前，最新版本绝大多数`examples`已改成`yaml`配置文件，而不是`shell`命令用`parser`读取参数了。

当然，本质上就是把训练参数传到`train.py`中

![](img/9aa0c5cdacee2e494b962d42a431bee2.png)

`examples`下的`lora_single_gpu/llama3_lora_sft.yaml`
