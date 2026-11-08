# 只需 30 分钟，微调 Qwen2-7B，搭建专属 AI 客服解决方案

> 来源：[https://agispace.feishu.cn/docx/Hd5rdc1cLomMTfxLN25cetUGnPc](https://agispace.feishu.cn/docx/Hd5rdc1cLomMTfxLN25cetUGnPc)

> 原创：是莫尔索

> 👸号：莫尔索随笔

> 原文链接：https://mp.weixin.qq.com/s/Pb-l4vON8PvgXwRwgOt2FQ

> 产品接入大模型驱动的 AI 客服机器人也有一段时间了，也积累了不少真实场景下的客户问答数据，因为给每条回答设置了点 👍 点 👎 按钮，最近将其中点 👍 的问答对导出来（妥妥的人工标准高质量数据），试着基于 Qwen2-7B 微调一个小模型跑下效果，如果能够应付 85% 以上回答，准备在这个场景里撤下当前的智谱模型了。

Qwen2-7B 在中文场景下的回答效果我在 siliconflow 上体验下来，效果很不错，所以选它做基础模型。

![](img/b4caf703f88209a514282e11b8d014c8.png)

Qwen2-7B

## **微调步骤**

### 安装 LLaMA Factory 依赖

我是在谷歌 Colab() 里面微调的，毕竟免费用户可以白嫖 15G 显存的 T4 GPU，这里特别感谢 LLaMA Factory([1]) 项目，一个包免去多余的依赖安装和环境配置。

+

+

+

+

+

+

```
%cd /content/%rm -rf LLaMA-Factory!git clone https://github.com/hiyouga/LLaMA-Factory.git%cd LLaMA-Factory%ls!pip install -e .[torch,bitsandbytes]
```

### **更新预设数据集**

这个 identity.json（自我认知数据集） 实际上更新的是基础模型**内置设定，目**的是让模型回答时既不是 Qwen2-7B、也不是 ChatGPT() 等其他任何模型，它现在的名字叫BajiGo。

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

```
import json
%cd /content/LLaMA-Factory/
NAME = &quot;BajiGo&quot;AUTHOR = &quot;莫尔索&quot;
with open(&quot;data/identity.json&quot;, &quot;r&quot;, encoding=&quot;utf-8&quot;) as f:  dataset = json.load(f)
for sample in dataset:  sample[&quot;output&quot;] = sample[&quot;output&quot;].replace(&quot;{{&quot;+ &quot;name&quot; + &quot;}}&quot;, NAME).replace(&quot;{{&quot;+ &quot;author&quot; + &quot;}}&quot;, AUTHOR)
with open(&quot;data/identity.json&quot;, &quot;w&quot;, encoding=&quot;utf-8&quot;) as f:  json.dump(dataset, f, indent=2, ensure_ascii=False)
```

下面是更新后的 identity.json 示例：

identity.json 示例

### **添加自定义数据**

LLaMA-Factory 在 data 文件夹中提供了多个训练数据集示例，支持 alpaca 或 sharegpt 格式，不过我这里使用的是自己提前导出来做过处理的自定义数据集。

1.  自定义数据集选用 alpaca，格式如下：

+

+

+

+

+

+

+

+

+

+

+

+

```
[  {    &quot;instruction&quot;: &quot;user instruction (required)&quot;,    &quot;input&quot;: &quot;user input (optional)&quot;,    &quot;output&quot;: &quot;model response (required)&quot;,    &quot;system&quot;: &quot;system prompt (optional)&quot;,    &quot;history&quot;: [      [&quot;user instruction in the first round (optional)&quot;, &quot;model response in the first round (optional)&quot;],      [&quot;user instruction in the second round (optional)&quot;, &quot;model response in the second round (optional)&quot;]    ]  }]
```

1.  因为我只有标注后的问答对，所以只需要填充instruction和output部分即可，下面是部分数据，保存为 bajigo.json，并上传到 LLaMA-Factory 的 data 目录下。

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

```
  {    &quot;instruction&quot;: &quot;我能建两份菜单然后自行切换吗&quot;,    &quot;input&quot;: &quot;&quot;,    &quot;output&quot;: &quot;你好，暂不支持同时创建多个菜单。如果你是想在不同时段进行不同的菜单展示，你可以在首页点击「商品」进入商品管理页面，之后点击右上方的「批量管理」按钮，选中商品后在下方按需要点击上架/下架进行批量操作，来更换菜单内商品。    &quot;system&quot;: &quot;&quot;,    &quot;history&quot;: []  },  {    &quot;instruction&quot;: &quot;优惠券怎么发&quot;,    &quot;input&quot;: &quot;&quot;,    &quot;output&quot;: &quot;你好，你可以在首页点击「活动&amp;储值」-&gt;「活动」-&gt;「优惠券」设置优惠券，根据你的需求，自定义优惠券名称、生效范围、有效日期，优惠券类型也分为满减优惠券、折扣优惠券和免费优惠券。填好信息后，点击「下一步」按钮，就可以指定发送对象与发放数量了。目前只支持这种方式进行发券，不支持用户主动领取哦。,    &quot;system&quot;: &quot;&quot;,    &quot;history&quot;: []  },  {    &quot;instruction&quot;: &quot;怎么绑定打印机&quot;,    &quot;input&quot;: &quot;&quot;,    &quot;output&quot;: &quot;你好，你可以在首页点击「店铺设置」-&gt;「打印机」去添加打印机，并且我们支持同时绑定多台打印机。需要注意的是，我们目前只支持飞鹅牌的小票机和标签机，拿到机器后，你可以打开，然后找到打印机设置，扫描小票机或者标签机底部的二维码即可进行绑定。    &quot;system&quot;: &quot;&quot;,    &quot;history&quot;: []  },
```

1.  修改dataset_info.json，将数据集添加到全局配置，好让程序到时候可以直接根据bajigo名称匹配到相应训练数据。

+

+

+

+

+

+

+

+

+

+

```
  &quot;bajigo&quot;: {  &quot;file_name&quot;: &quot;bajigo.json&quot;,  &quot;columns&quot;: {    &quot;prompt&quot;: &quot;instruction&quot;,    &quot;query&quot;: &quot;input&quot;,    &quot;response&quot;: &quot;output&quot;,    &quot;system&quot;: &quot;system&quot;,    &quot;history&quot;: &quot;history&quot;    }  },
```

### **配置训练参数**

这里需要专门说下，刚开始计划使用的是 Qwen2-7B-Instruct 全量模型，奈何加载模型的时候 GPU 内存就爆了（大家如果不缺 GPU，配置为 Qwen/Qwen2-7B-Instruct 即可），最后找到了一个 4bit 量化版 Qwen2-7B-Instruct-bnb-4bit，下面是一些训练参数配置及解释。

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

```
import json
args = dict(  stage=&quot;sft&quot;,                        # 进行指令监督微调  do_train=True,  model_name_or_path=&quot;unsloth/Qwen2-7B-Instruct-bnb-4bit&quot;, # 使用 4 bit量化版 Qwen2-7B-Instruct 模型  dataset=&quot;identity,bajigo&quot;,      # 使用 bajigo 和自我认知数据集  template=&quot;qwen&quot;,                     # 使用 qwen2 提示词模板  finetuning_type=&quot;lora&quot;,                   # 使用 LoRA 适配器来节省显存  lora_target=&quot;all&quot;,                     # 添加 LoRA 适配器至全部线性层  output_dir=&quot;qwen2_lora&quot;,                  # 保存 LoRA 适配器的路径  per_device_train_batch_size=2,               # 批处理大小  gradient_accumulation_steps=4,               # 梯度累积步数  lr_scheduler_type=&quot;cosine&quot;,                 # 使用余弦学习率退火算法  logging_steps=10,                      # 每 10 步输出一个记录  warmup_ratio=0.1,                      # 使用预热学习率  save_steps=1000,                      # 每 1000 步保存一个检查点  learning_rate=5e-5,                     # 学习率大小  num_train_epochs=3.0,                    # 训练轮数  max_samples=300,                      # 使用每个数据集中的 300 条样本  max_grad_norm=1.0,                     # 将梯度范数裁剪至 1.0  quantization_bit=4,                     # 使用 4 比特 QLoRA （可选，4 bit量化版）  loraplus_lr_ratio=16.0,                   # 使用 LoRA+ 算法并设置 lambda=16.0（可选，4 bit量化版）  fp16=True                         # 使用 float16 混合精度训练（可选，4 bit量化版）)
json.dump(args, open(&quot;bajigo.json&quot;, &quot;w&quot;, encoding=&quot;utf-8&quot;), indent=2)
%cd /content/LLaMA-Factory/
!llamafactory-cli train bajigo.json  # 开始指令监督微调
```

### **开始训练**

接下来就是耐心等待炼丹，差不多 16 分钟左右，模型就微调结束了，果然 4bit 小有小的好处，是不是很容易 😎。

微调结束

## **智能客服上线**

接下来赶紧试下微调好的模型（利用 while 逻辑实现的一个简单终端对话效果）

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

+

# %cd /content/LLaMA-Factory/import sysimport os 获取当前工作目录current_path = os.getcwd() 拼接当前工作目录和src目录的路径src_path = os.path.join(current_path, 'src') 将src目录的路径添加到sys.path的开头sys.path.insert(0, src_path) from llamafactory.chat import ChatModelfrom llamafactory.extras.misc import torch_gc torch_gc()args = dict(  model_name_or_path="unsloth/Qwen2-7B-Instruct-bnb-4bit", # 使用 4 bit量化版 Qwen2-7B-Instruct 模型  adapter_name_or_path="qwen2_lora",            # 加载之前保存的 LoRA 适配器  template="qwen",                     # 和训练保持一致  finetuning_type="lora",                  # 和训练保持一致)chat_model = ChatModel(args) messages = []print("使用 clear 清除对话历史，使用 exit 退出程序。")while True:  query = input("\n用户: ")  if query.strip() == "exit":    break  if query.strip() == "clear":    messages = []    torch_gc()    print("对话历史已清除")    continue   messages.append({"role": "user", "content": query})  print("BajiGo: ", end="", flush=True)   response = ""  for new_text in chat_model.stream_chat(messages):    print(new_text, end="", flush=True)    response += new_text  print()  messages.append({"role": "assistant", "content": response})

下面就是对话效果截图，怎么样，回答的内容基本和训练数据集中的 QA 对差不多，接下来我会继续测试没纳入训练集的点 👎 问题的回答效果。

BajiGo对话效果

后台回复 「**Qwen-7B**」获取本次微调所有代码。