
# 极简的LLM多卡预测方法

> 来源：[https://agijuejin.feishu.cn/docx/OBR1dBwXgoOpS2xFOqCcJivLnfg](https://agijuejin.feishu.cn/docx/OBR1dBwXgoOpS2xFOqCcJivLnfg)

> 作者：[nghuyong](https://www.zhihu.com/people/hu-yong-64)

> 原文：https://zhuanlan.zhihu.com/p/689905473

需求场景：有多张卡，且单卡可以放下完整的模型，希望对一大批测试数据进行推理预测。

期望实现：用“数据并行”的方法进行并行推理，即每张卡都加载完整的模型，并把数据均分到每张卡上进行预测，预测完毕再对每张卡的结果进行合并。

`vllm`，`fastchat`等部署框架可能能解决，但稍微有点“重”，极简的实践推荐用`accelerate`

下面给一个具体的例子`batch_size=1`

```
from accelerate import Accelerator
from accelerate.utils import gather_object
from transformers import AutoModelForCausalLM, AutoTokenizer

# 初始化accelerator
accelerator = Accelerator()

# 加载模型和tokenizer，模型会分别加载到不同的卡上
model_path="llama2-7b"
model = AutoModelForCausalLM.from_pretrained(
    model_path,    
    device_map={"": accelerator.process_index}, 
    torch_dtype=torch.bfloat16,
)
tokenizer = AutoTokenizer.from_pretrained(model_path)   

# 加载预测数据
all_items = json.load(open('predict_data.json'))

# 预测数据均分到多卡上(核心)
with accelerator.split_between_processes(all_items) as items:
    results = []
    # 每张卡独立推理自己分到的数据
    for item in items:
        prompt_tokenized=tokenizer(item['prompt'], return_tensors="pt").to("cuda")
        output_tokenized = model.generate(
            **prompt_tokenized, 
            max_new_tokens=1024,
            pad_token_id=tokenizer.eos_token_id)[0]
        output_tokenized=output_tokenized[len(prompt_tokenized["input_ids"][0]):]
        output = tokenizer.decode(output_tokenized, skip_special_tokens=True)
        item['predict'] = output
        results.append(item)

# 汇总多卡的结果
items_with_predict = gather_object(results)
```

最后通过`accelerate`启动即可

```
# 在8卡上并行推理
accelerate launch --num_processes 8 predict.py
```
