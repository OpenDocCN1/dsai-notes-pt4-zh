
# DeepSeek-R1-Distill-Qwen-7B vLLM 部署调用

> 来源：[https://v11enp9ok1h.feishu.cn/docx/GQlFduZxroTd57xdmX4c5z5Tnid](https://v11enp9ok1h.feishu.cn/docx/GQlFduZxroTd57xdmX4c5z5Tnid)

## vLLM 简介

vLLM 框架是一个高效的大语言模型推理和部署服务系统，具备以下特性：

+   **高效的内存管理**：通过 `PagedAttention` 算法，vLLM 实现了对 `KV 缓存` 的高效管理，减少了内存浪费，优化了模型的运行效率。

+   **高吞吐量**：vLLM 支持异步处理和连续批处理请求，显著提高了模型推理的吞吐量，加速了文本生成和处理速度。

+   **易用性**：vLLM 与 HuggingFace 模型无缝集成，支持多种流行的大型语言模型，简化了模型部署和推理的过程。兼容 OpenAI 的 API 服务器。

+   **分布式推理**：框架支持在多 GPU 环境中进行分布式推理，通过模型并行策略和高效的数据通信，提升了处理大型模型的能力。

+   **开源共享**：vLLM 由于其开源的属性，拥有活跃的社区支持，这也便于开发者贡献和改进，共同推动技术发展。

## 环境准备

本文基础环境如下：

```
----------------
ubuntu 22.04
python 3.12
cuda 12.1
pytorch 2.3.0
----------------
```

> 本文默认学习者已配置好以上 Pytorch (cuda) 环境，如未配置请先自行安装。

首先 `pip` 换源加速下载并安装依赖包

```
python -m pip install --upgrade pip
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

pip install modelscope==1.22.3
pip install openai==1.61.0
pip install tqdm==4.67.1
pip install transformers==4.48.2
pip install vllm==0.7.1
```

> 考虑到部分同学配置环境可能会遇到一些问题，我们在 AutoDL 平台准备了 DeepSeek-R1-Distill 的环境镜像，点击下方链接并直接创建 Autodl 示例即可。

> `https://www.codewithgpu.com/i/datawhalechina/self-llm/DeepSeek-R1-Distill-self-llm`

## 模型下载

使用 `modelscope` 中的 `snapshot_download()` 函数下载模型，第一个参数为模型名称，参数 `cache_dir` 为模型的下载路径。

新建 `model_download.py` 文件并在其中输入以下内容，粘贴代码后记得保存文件。

```
from modelscope import snapshot_download

model_dir = snapshot_download('deepseek-ai/DeepSeek-R1-Distill-Qwen-7B', cache_dir='/root/autodl-tmp', revision='master')
```

然后在终端中输入 `python model_download.py` 执行下载，这里需要耐心等待一段时间直到模型下载完成。

> 注意：记得修改 `cache_dir` 为你的模型下载路径哦~

## 代码准备

### Python脚本

新建 `vllm_model.py` 文件并在其中输入以下内容，粘贴代码后请及时保存文件。下面的代码有很详细的注释，如有不理解的地方，欢迎大家提 issue。

首先从 vLLM 库中导入 `LLM` 和 `SamplingParams` 类。`LLM` 类是使用 vLLM 引擎运行离线推理的主要类。`SamplingParams` 类指定采样过程的参数，用于控制和调整生成文本的随机性和多样性。

vLLM 提供了非常方便的封装，我们直接传入模型名称或模型路径即可，不必手动初始化模型和分词器。

我们可以通过这个代码示例熟悉下 vLLM 引擎的使用方式。被注释的部分内容可以丰富模型的能力，但不是必要的，大家可以按需选择，自己多多动手尝试 ~

```
# vllm_model.py
from vllm import LLM, SamplingParams
from transformers import AutoTokenizer
import os
import json

# 自动下载模型时，指定使用modelscope; 否则，会从HuggingFace下载
os.environ['VLLM_USE_MODELSCOPE']='True'

def get_completion(prompts, model, tokenizer=None, max_tokens=8192, temperature=0.6, top_p=0.95, max_model_len=2048):
    stop_token_ids = [151329, 151336, 151338]
    # 创建采样参数。temperature 控制生成文本的多样性，top_p 控制核心采样的概率
    sampling_params = SamplingParams(temperature=temperature, top_p=top_p, max_tokens=max_tokens, stop_token_ids=stop_token_ids)
    # 初始化 vLLM 推理引擎
    llm = LLM(model=model, tokenizer=tokenizer, max_model_len=max_model_len,trust_remote_code=True)
    outputs = llm.generate(prompts, sampling_params)
    return outputs


if __name__ == "__main__":    
    # 初始化 vLLM 推理引擎
    model='/root/autodl-temp/deepseek-ai/DeepSeek-R1-Distill-Qwen-7B' # 指定模型路径
    # model="deepseek-ai/DeepSeek-R1-Distill-Qwen-7B" # 指定模型名称，自动下载模型
    tokenizer = None
    # 加载分词器后传入vLLM 模型，但不是必要的。
    # tokenizer = AutoTokenizer.from_pretrained(model, use_fast=False) 
    
    text = ["请帮我制定个简短的初学者Python学习计划<think>\n", ] # 可用 List 同时传入多个 prompt，根据 DeepSeek 官方的建议，每个 prompt 都需要以 <think>\n 结尾，如果是数学推理内容，建议包含（中英文皆可）：Please reason step by step, and put your final answer within \boxed{}.

    # messages = [
    #     {"role": "user", "content": prompt+"<think>\n"}
    # ]
    # 作为聊天模板的消息，不是必要的。
    # text = tokenizer.apply_chat_template(
    #     messages,
    #     tokenize=False,
    #     add_generation_prompt=True
    # )

    outputs = get_completion(text, model, tokenizer=tokenizer, max_tokens=8192, temperature=0.6, top_p=0.95, max_model_len=2048) # 思考需要输出更多的 Token 数，max_tokens 设为 8K，根据 DeepSeek 官方的建议，temperature应在 0.5-0.7，推荐 0.6

    # 输出是一个包含 prompt、生成文本和其他信息的 RequestOutput 对象列表。
    # 打印输出。
    for output in outputs:
        prompt = output.prompt
        generated_text = output.outputs[0].text
        if r"</think>" in generated_text:
            think_content, answer_content = generated_text.split(r"</think>")
        else:
            think_content = ""
            answer_content = generated_text
        print(f"Prompt: {prompt!r}, Think: {think_content!r}, Answer: {answer_content!r}")
```

运行代码

```
python vllm_model.py
```

结果如下：

```
Prompt: '请帮我制定个简短的初学者Python学习计划<think>\n', Think: '好的，我现在要帮用户制定一个简短的初学者Python学习计划。首先，我得考虑用户的使用场景和需求。用户可能是刚接触Python的新手，可能不太熟悉编程的基本概念，所以计划需要既全面又简短。\n\n用户的需求很明确，就是制定一个学习计划，但可能还需要一些指导。所以，我应该提供结构化的建议，分阶段学习，逐步深入，同时加入一些实用的建议，比如练习和资源推荐。\n\n接下来，我得分析用户的可能使用场景。用户可能在赶时间，或者希望有一个系统的学习路径。所以，计划需要高效，每个阶段的时间不宜过长，避免用户感到压力。\n\n然后，我需要考虑用户可能的背景。用户可能没有太多编程经验，所以计划中的内容要从基础开始，逐步引入高级概念。同时，要提醒用户不要急于求成，保持耐心和持续学习。\n\n用户可能的深层需求是希望在有限的时间内快速提升，或者希望通过系统的学习来逐步掌握编程。因此，计划需要涵盖基本概念、编程结构、函数、模块和框架，以及高级内容，确保用户能够全面掌握。\n\n现在，我得把这些思考整合成一个结构化的计划。首先，明确学习目标，然后分阶段安排，包括基础、编程结构、函数、模块和框架，最后是高级内容。每个阶段的建议要具体，比如使用在线资源、练习、阅读书籍等。\n\n另外，我需要提醒用户注意不要急于求成，保持学习的热情和耐心。这样，用户在学习过程中不会感到压力，反而会感到成就感和满足感。\n\n最后，我得确保整个计划简洁明了，每个阶段的时间安排合理，不会让用户感到累重。同时，加入一些实用的建议，如使用在线资源、练习、阅读书籍等，帮助用户更有效地学习。\n\n总结一下，我的计划应该包括以下几个阶段：基础入门、编程结构、函数与模块、框架与高级内容，每个阶段都有具体的学习建议，同时提醒用户保持学习的热情和耐心。\n', Answer: '

以下是一个简短的初学者Python学习计划，分为几个阶段，建议合理安排时间：

---

### **阶段一：Python入门（10天）**
**目标：** 掌握基本的Python语法和基本编程概念。
- **学习内容：**
  - 基本语法：变量、类型、运算符、条件语句、循环（for, while）
  - 函数：定义、调用、返回值
  - 列表、元组、集合：基本操作
  - 字典：基本操作
  - 代码结构：if语句、try-except块
- **建议：**
  - 使用在线资源（Codecademy, LeetCode等）练习
  - 每天学习1-2小时，完成两到三个练习题
  - 完成每章课程后进行测试

---

### **阶段二：Python编程（15天）**
**目标：** 掌握基本编程技巧，学习一些常见的编程问题。
- **学习内容：**
  - 逻辑思维：解决简单的问题
  - 代码结构：函数的嵌套、多线程
  - 数据结构：数组、字典、集合的高级使用
  - 交互界面：使用input()和print()函数
  - 代码优化：使用注释、调试工具
- **建议：**
  - 使用在线资源（Codecademy, LeetCode等）解决实际问题
  - 每天学习1-2小时，完成两到三个练习题
  - 完成每章课程后进行测试

---

### **阶段三：Python高级内容（10天）**
**目标：** 掌握Python的高级功能和常用库。
- **学习内容：**
  - 梯度下降法、机器学习基础（假设你对机器学习感兴趣）
  - 解决常见问题：文件处理、图像处理、数据分析
  - 日期和时间处理：datetime模块
  - 异常处理：try-except块
  - 应用编程：解决实际项目
- **建议：**
  - 使用在线资源（Codecademy, LeetCode等）解决实际问题
  - 每天学习1-2小时，完成两到三个练习题
  - 完成每章课程后进行测试

---

### **阶段四：Python社区与实践（10天）**
**目标：** 学习Python的社区和实践，提升代码质量和团队协作。
- **学习内容：**
  - 代码社区：GitHub、GitHub Pages、Stack Overflow等
  - 代码风格：使用PEP 8规范
  - 团队协作：使用GitHub和Git
  - 项目实践：完成一个完整的项目
- **建议：**
  - 使用在线资源（Codecademy, LeetCode等）解决实际问题
  - 每天学习1-2小时，完成两到三个练习题
  - 完成每章课程后进行测试

---

### **结束建议：**
- **保持耐心：** Python学习需要时间和积累，不要急于求成
- **持续学习：** 每天花1-2小时学习，保持学习的热情
- **记录学习进展：** 定期记录学习内容和成果，逐步完善计划

希望这个计划能帮助你快速掌握Python！如果还有其他问题，随时提问！'
```

![](img/baf513563293a29b373b724487b2a264.png)

### 创建兼容 OpenAI API 接口的服务器

DeepSeek-R1-Distill-Qwen 兼容 OpenAI API 协议，所以我们可以直接使用 vLLM 创建 OpenAI API 服务器。vLLM 部署实现 OpenAI API 协议的服务器非常方便。默认会在 `http://localhost:8000` 启动服务器。服务器当前一次托管一个模型，并实现列表模型、`completions` 和 `chat completions` 端口。

+   `completions`：是基本的文本生成任务，模型会在给定的提示后生成一段文本。这种类型的任务通常用于生成文章、故事、邮件等。

+   `chat completions`：是面向对话的任务，模型需要理解和生成对话。这种类型的任务通常用于构建聊天机器人或者对话系统。

在创建服务器时，我们可以指定模型名称、模型路径、聊天模板等参数。

+   `--host` 和 `--port` 参数指定地址。

+   `--model` 参数指定模型名称。

+   `--chat-template` 参数指定聊天模板。

+   `--served-model-name` 指定服务模型的名称。

+   `--max-model-len` 指定模型的最大长度。

```
python -m vllm.entrypoints.openai.api_server \
  --model /root/autodl-tmp/deepseek-ai/DeepSeek-R1-Distill-Qwen-7B \
  --served-model-name DeepSeek-R1-Distill-Qwen-7B \
  --max-model-len=2048
```

加载完毕后出现如下信息说明服务成功启动

![](img/709edb9723bb52fc5f424e0683051809.png)

+   通过 `curl` 命令查看当前的模型列表

```
curl http://localhost:8000/v1/models
```

得到的返回值如下所示
