
# 代码实战：手动编写Qwen模型的Chat方法并接入vLLM框架，实现多轮对话及WebUI应用

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/WDZFdPZP1o0trCxVDsBczox0n4d](https://kq4b3vgg5b.feishu.cn/docx/WDZFdPZP1o0trCxVDsBczox0n4d)

## 本地部署开源大模型

### 3.手动实现基于vLLM框架的chat 方法

所谓续写模式和对话模式，源于模型的训练数据不同，对于Qwen-7B基座模型来说，其训练数据的格式是这样的：

```
输入：1月30日上午，中央气象台官微发布一张郑州市降水预报图，直呼：“河南郑州本次降雪预报图，这么离谱的预报图头一次见！<|endoftext|>
输出：预计1月31日至2月5日，我国中东部地区将进入入冬以来持续时间最长、影响范围最广的雨雪冰冻天气过程。10省份将出现暴雪或大暴雪，河南、河北、山东、辽宁、湖北等地的日降水量或累计降雪量具有极端性，河南、湖北、安徽、湖南、贵州5省将出现冻雨。<|endoftext|>
...
...
输入：1月28日，“格力2024全球梦想盛典”在珠海举行。格力电器员工在“格力全球梦想盛典”上合唱《我妈董明珠》的视频引起网友热议。<|endoftext|>
输出：这个节目名为音乐快板《大“格”局 新魅“力”》，表演者当时为了增加幽默气氛，开了个小玩笑，原话是“我妈就是董明珠……我妈最喜欢董明珠”。<|endoftext|>
```

而对话模型，其训练数据往往是一问一答的形式。每个模型都有其开发者自定义的输入输出格式，比如gpt3.5是这样的：

```
{"messages": [{"role": "system", "content": "Marv is a factual chatbot that is also sarcastic."}, 
              {"role": "user", "content": "What's the capital of France?"}, 
              {"role": "assistant", "content": "Paris, as if everyone doesn't know that already."}]}

{"messages": [{"role": "system", "content": "Marv is a factual chatbot that is also sarcastic."}, 
              {"role": "user", "content": "Who wrote 'Romeo and Juliet'?"}, 
              {"role": "assistant", "content": "Oh, just some guy named William Shakespeare. Ever heard of him?"}]}

{"messages": [{"role": "system", "content": "Marv is a factual chatbot that is also sarcastic."}, 
              {"role": "user", "content": "How far is the Moon from Earth?"}, 
              {"role": "assistant", "content": "Around 384,400 kilometers. Give or take a few, like that really matters."}]},]}]}
```

Qwen微调的对话格式是这样的：

```
[
  {
    "id": "identity_0",
    "conversations": [
      {
        "from": "user",
        "value": "你好"
      },
      {
        "from": "assistant",
        "value": "我是一个语言模型，我叫通义千问。"
      }
    ]
  }
]
```

Qwen的开源系列中的Chat类对话模型（Qwen-7B-Chat），它是基于对应的预训练版本（Qwen-7B）进行微调（SFT，S监督学习，FT微调）得到的，所以基于上述对话形式，就会通过如下方式，来界定某一个角色下的输入和输出：

```
输入：<|im_start|>system
      你是一个智能助手。<|im_end|>                 （这一部分是系统信息）
      
      <|im_start|>user
      你好呀，请你介绍一下你自己<|im_end|>
      <|im_start|>assitant
      我是Qwen-Chat模型，一个非常厉害的人工智能助手，你可以向我提出任意的提问<|im_end|>
      <|im_start|>user
      好的，我想吃点甜食，你能不能帮我推荐一些好吃的食物？<|im_end|>
      <|im_start|>assitant
      当然可以，如果你喜欢吃甜食，那么推荐你吃巧克力的甜甜圈，太好吃了<|im_end|>       （这一部分是历史对话信息）
      
      <|im_start|>user
      那么去哪里可以买到呢？<|im_end|>
       <|im_start|>assitant<|endoftext|>    （这一部分是最新一轮的用户提问）

输出：你可以去超市、蛋糕店等地方买到这个美味的食物<|im_end|><|endoftext|>     （这部分是Qwen模型针对最新一轮的提问，结合历史内容生成的回复）
```

所以我们能表象看到的微调格式，并不是实际意义上送入Qwen模型训练的形式，而是会经过一系列格式的转化后，再送入模型。可以在这里看到：

![](img/3ab751e7b5efa9d6f206ad23514b588d.png)

这种格式，被Qwen官方定义为ChatML格式。上述的输出，对应的就是我们上一步使用`chat()`方法，对Qwen模型执行的三次对话，也就是这样的三次操作：

```
# 第一轮对话
response, history = model.chat(tokenizer, "你好呀，请你介绍一下你自己", history="你是一个智能助手。")
print(response)   -- >  我是Qwen-Chat模型，一个非常厉害的人工智能助手，你可以向我提出任意的提问

# 第一轮对话
response, history = model.chat(tokenizer, "好的，我想吃点甜食，你能不能帮我推荐一些好吃的食物？", history=history)
print(response)  -- >  当然可以，如果你喜欢吃甜食，那么推荐你吃巧克力的甜甜圈，太好吃了

# 第三轮对话
response, history = model.chat(tokenizer, "那么去哪里可以买到呢？", history=history)
print(response)  -- > 你可以去超市、蛋糕店等地方买到这个美味的食物
```

这里的`<|im_end|>`、`<|im_start|>`及`<|endoftext|>`，也是Qwen官方定义的特殊Token，其中Qwen-7B中有`<|endoftext|>`，Qwen-7B-Chat中有`<|endoftext|>`、`<|im_start|>`以及`<|im_end|>`。同时词表中也留有供扩展的特殊token位，可用`<|extra_0|>`到`<|extra_204|>`来指代。我们可以通过`tokenizer.special_tokens`来查看：

```
# 导入Transformer依赖库
from transformers import AutoModelForCausalLM, AutoTokenizer

# 初始化tokenizer
tokenizer = AutoTokenizer.from_pretrained("/home/Work/00.Work_muyu/muyu_qwen/models/qwen/Qwen-7B-Chat/", trust_remote_code=True,)

# 打印特殊Token
tokenizer.special_tokens
```
