# Task2 baseline精读

> 来源：[https://datawhaler.feishu.cn/docx/V1xmd2lVgoNXQgx9kmBck3aKnWb](https://datawhaler.feishu.cn/docx/V1xmd2lVgoNXQgx9kmBck3aKnWb)

> **教程贡献者说：**

> 前面我们快速跑通了baseline

> 今天我们的任务是搞清楚baseline讲了些啥，为什么这样用。

> 你可以从中学会如何使用大语言模型完成AI+逻辑推理的任务。

>

> ⚠️**注意：**本Task，建议大家使用其他有免费API的开源模型进行测试，或直接自己部署，否则可能产生较多API调用费用

>

> 推荐大家使用 魔搭Notebook：方式二GPU 进行相应的模型部署与测试

# 赛题思路介绍

在task01中我们介绍了机器学习与深度学习**两部分的思路，那么大家肯定**想知道如何使用大模型解决这个问题，我们baseline是如何做的？代码的核心在哪里？大模型做了什么？

## 2.1 大模型介绍

### 大语言模型的概念

**大语言模型（英文：Large La**nguage Model，缩写LLM），也称大型语言模型，是一种人工智能模型，旨在理解和生成人类语言。

**通常**，**大语言模型 (**LLM) 指包含数十亿（Billion或更多）参数的语言模型，这些模型在大量的文本数据上进行训练，例如国外的有GPT-3 、GPT-4、PaLM 、Galactica 和 LLaMA 等，国内的有ChatGLM、文心一言、通义千问、讯飞星火等。

### **大模型的能力和特点**

1.  **大模型的能力**

大语言模型（LLM）与以前的预训练语言模型（PLM）的主要区别在于其涌现能力。这种能力在小型模型中不明显，但在大型模型中显著。例如：

+   **上下文学习**：首次由GPT-3引入，允许模型在提供自然语言指令或多个任务示例的情况下，通过理解上下文并生成相应输出来执行任务。

+   **指令遵循**：通过指令微调，LLM可以根据任务指令执行未见过的任务，展示出强大的泛化能力。

+   **逐步推理**：通过"思维链（Chain of Thought, CoT）"策略，LLM能够解决多步推理任务，例如数学问题。

1.  **大模型的特点**

+   **巨大的规模**：参数规模达数十亿甚至数千亿，使其能捕捉更多语言知识和复杂语法结构。

+   **预训练和微调**：在大规模无标签文本数据上预训练，然后通过有标签数据微调，适应特定任务。

+   **上下文感知**：具备强大的上下文感知能力，能够理解和生成依赖前文的文本内容。

+   **多语言支持**：支持多种语言，促进跨文化和跨语言的应用。

+   **多模态支持**：一些LLM支持文本、图像和语音的多模态数据。

+   **涌现能力**：在大规模模型中表现出明显的性能提升，能处理更复杂的任务。

+   **多领域应用**：广泛应用于文本生成、自动翻译、信息检索、摘要生成、聊天机器人等多个领域。

+   **伦理和风险问题**：需要谨慎处理生成有害内容、隐私问题和认知偏差等伦理和风险问题。

## 2.2 大语言模型解题方案介绍

### 大模型推理介绍

嘿，小伙伴们！今天咱们要聊的是一个超级酷炫的话题——大语言模型，简称LLM。别担心，即使你是技术小白，也能轻松跟上我的节奏。想象一下，如果有一个超级大脑能听懂你的话，还能跟你聊天、写故事、甚至帮你做决定，是不是很神奇？这就是LLM的魔力所在！

话说回来，在过去的几年里，这些超级大脑们在各种各样的任务上都取得了惊人的进步。比如说，它们现在不仅能回答你关于天气的问题，还能帮你写出一篇关于气候变化的文章呢！而且，随着这些超级大脑变得越来越大（不是说它们真的会长成巨人哦），它们开始展现出了一些真正了不起的能力——推理能力！

推理能力是什么呢？简单来说，就是能够理解一些基本的逻辑，然后根据这些逻辑去思考问题。比如，如果你知道“所有猫都喜欢吃鱼”，并且还知道“汤姆是一只猫”，那么你应该可以推断出“汤姆喜欢吃鱼”。这就是推理的魅力所在！

不过，虽然我们的超级大脑们已经开始展现出这种能力，但它们究竟是怎么学会的，这背后的技术细节可就复杂多了。科学家们正在夜以继日地研究这个问题，试图找到让AI模型变得更聪明的秘密武器。各大研究机构和公司都在这个领域投入了大量的资源，因为谁要是能解开这个谜题，谁就能引领未来的AI潮流啦！

所以，下次当你和朋友聊起人工智能的时候，你可以骄傲地说：“你知道吗？现在的超级大脑不仅能听懂你说的话，还能自己思考问题呢！”当然，别忘了提醒他们，这些超级大脑还在成长中，就像我们小时候学数学一样，有时候也会犯点小糊涂哦。

### 大模型推理实现最常用方法——提示工程(Prompt Engineering)

提示工程（Prompt Engineering）是一门较新的学科，关注提示词开发和优化，帮助用户将大语言模型（Large Language Model, LLM）用于各场景和研究领域。 掌握了提示工程相关技能将有助于用户更好地了解大型语言模型的能力和局限性。

研究人员可利用提示工程来提升大语言模型处理复杂任务场景的能力，如问答和算术推理能力。开发人员可通过提示工程设计、研发强大的工程技术，实现和大语言模型或其他生态工具的高效接轨。

提示工程不仅仅是关于设计和研发提示词。它包含了与大语言模型交互和研发的各种技能和技术。提示工程在实现和大语言模型交互、对接，以及理解大语言模型能力方面都起着重要作用。用户可以通过提示工程来提高大语言模型的安全性，也可以赋能大语言模型，比如借助专业领域知识和外部工具来增强大语言模型能力。

#### 输入问题：

这里选取了train的第一个问题。可以看到有问题背景、提问、以及选项这样结构的数据字典。

```
{
  &#x27;problem&#x27;: 
        &#x27;有一群人和一些食物类型。下列是关于这些个体和食物的已知信息：\n\n1. 鸡肉是一种食物。\n2. 苹果是一种食物。\n3. 如果X吃了Y，且X活着，则Y是一种食物。\n4. Bill存活。\n5. Bill吃了花生。\n6. John吃所有食物。\n7. Sue吃所有Bill吃的食物。\n8. John喜欢所有食物。\n\n根据以上信息，回答以下选择题：&#x27;, 
   &#x27;questions&#x27;: [
        {
        &#x27;question&#x27;: &#x27;选择题 1：\n谁喜欢吃花生？&#x27;, 
        &#x27;options&#x27;: [&#x27;Bill&#x27;, &#x27;Sue&#x27;, &#x27;John&#x27;, &#x27;None of the above&#x27;]
        }
    ], &#x27;id&#x27;: &#x27;round1_test_data_000&#x27;
}
```

#### 转化后的prompt：

我们通过数据处理，将上面的问题字典转化为MD格式的prompt。这就是我们让大语言模型能理解并做分析的秘诀。

```
你是一个逻辑推理专家，擅长解决逻辑推理问题。以下是一个逻辑推理的题目，形式为单项选择题。所有的问题都是（close-world assumption）闭世界假设，即未观测事实都为假。请逐步分析问题并在最后一行输出答案，最后一行的格式为&quot;答案是：A&quot;。题目如下：

###题目:
有一群人和一些食物类型。下列是关于这些个体和食物的已知信息：

鸡肉是一种食物。
苹果是一种食物。
如果X吃了Y，且X活着，则Y是一种食物。
Bill存活。
Bill吃了花生。
John吃所有食物。
Sue吃所有Bill吃的食物。
John喜欢所有食物。

根据以上信息，回答以下选择题：

###问题:
选择题 1：
谁喜欢吃花生？
A. Bill
B. Sue
C. John
D. None of the above
```

#### 大模型回答：

这里为了大家方便起见使用了尚未微调的开源模型Qwen2-1.5b-instruct的api。（虽然赛题要求使用大模型后上传环境，大家可以先在低成本的api上做好试验，最后提交前把环境打包到docker文件上传即可。）

我们可以看到大模型在output['choices']['message']['content']返回了推理的结果。我们通过这样的方式让大模型完成了我们的推理任务。

```
{
    &quot;status_code&quot;: 200,
    &quot;request_id&quot;: &quot;18d43478-2aec-92be-8840-cd3bb08009cf&quot;,
    &quot;code&quot;: &quot;&quot;,
    &quot;message&quot;: &quot;&quot;,
    &quot;output&quot;: {
        &quot;text&quot;: null,
        &quot;finish_reason&quot;: null,
        &quot;choices&quot;: [
            {
                &quot;finish_reason&quot;: &quot;stop&quot;,
                &quot;message&quot;: {
                    &quot;role&quot;: &quot;assistant&quot;,
                    &quot;content&quot;: &quot;我们可以通过分析给出的信息来解答这个问题。\n\n首先，我们知道Bill吃了花生（根据第5条信息），并且Bill还活着（根据第4条信息）。根据第3条规则，“如果X吃了Y，且X活着，则Y是一种食物”，我们可以推断出花生是一种食物。\n\n接下来，我们知道John吃所有食物（根据第6条信息），并且John喜欢所有食物（根据第8条信息）。既然花生是一种食物，那么John应该也吃花生，并且喜欢花生。\n\nSue吃所有Bill吃的食物（根据第7条信息），既然Bill吃了花生，那么Sue也应该吃花生。\n\n所以，喜欢吃花生的人有Bill、Sue和John。\n\n答案是：C. John\n\n但是，题目要求选择一个选项，而实际上Bill、Sue和John都喜欢吃花生。然而，在给出的选项中，只有John被明确列出作为喜欢吃花生的人（选项C）。因此，虽然Bill和Sue也喜欢吃花生，但根据题目要求，正确答案应当是包含在选项中的那个，即John。\n\n答案是：C. John&quot;
                }
            }
        ]
    },
    &quot;usage&quot;: {
        &quot;input_tokens&quot;: 210,
        &quot;output_tokens&quot;: 223,
        &quot;total_tokens&quot;: 433
    }
}
```

# 整体代码介绍

整体代码主要包括**答案生成**和**纠错与结果文件生成**两个大模块。

**答案生成部分**包括大模型的处理函数、大模型返回结果抽取、多线程处理及答案生成的启动。

这里代码核心是**大模型部分**，动手能力强的小伙伴可以从这里入手开始自己的上分之路~为了保证整体代码性能使用多线程处理请求。

**纠错与结果生成部分**存在的目的是由于目前使用了api调用在线开源大模型，因为网络、模型能力等原因会导致有一些结果会出现缺失。（比如大模型回答时，没有明确给出ABCD的结果，而返回的空值。也有时因为网络retry模块机会使用结束后，依然没有提取到结果会跳过某个问题。）

## 3.1 环境配置

> 这里我们配置一些需要的环境，保障代码顺利跑通。

安装需要的库

```
!pip install scipy openai tiktoken retry dashscope loguru
```

配置好API，这个API在我们task01和大家介绍了，这里不多赘述。

```
**#** 注意**：**这里需要填入你的key~ 咱们在第二步申请的。
dashscope.api_key=&quot;sk-&quot;
```

导入需要的环境，包括日志处理、多线程、api请求等相关库引入。

```
**import json
i****mport os
fro**m pprint im**p****o**rt pprin**t**
imp**o**rt re
from **t**qdm i**m**port tqdm
im**p**ort ran**d**om

import uui**d**
im**p**ort openai
im**p****o**rt tiktoken
import json
import numpy as np
import requests
from retry import retry
from scipy import sparse
#from rank_bm25 import BM25Okapi
#import jieba
from http import HTTPStatus
import dashscope

from concurrent.futures import ThreadPoolExecutor, as_completed
from loguru import logger
import json
import time
from tqdm import tqdm

logger.remove()  # 移除默认的控制台输出
logger.add(&quot;logs/app_{time:YYYY-MM-DD}.log&quot;, level=&quot;INFO&quot;, rotation=&quot;00:00&quot;, retention=&quot;10 days&quot;, compression=&quot;zip&quot;)

MODEL_NAME = &#x27;qwen2-7b-instruct&#x27;
```

## 3.2 答案生成部分

### 3.2.1 大模型部分

**call_qwen_api **这个函数目的是通过输入模型名称、prompt，完成大模型api调用。

```
def **call_qwen_api(MODEL_NAME,** query):
** **   #** **这里**采**用das**h**sc**o**pe的api调**用****模**型推理，通过h**t**tp传**输**的json封**装**返回**结**果
    m**e**ssage**s** = [
  ** **  ** **  {&#x27;rol**e**&#x27;: &#x27;user&#x27;, &#x27;co**n**t**e**nt&#x27;: query}]
    response = dashscope.Generation.call(
        MODEL_NAME,
        messages=messages,
        result_format=&#x27;message&#x27;,  # set the result is message format.
    )
    if response.status_code == HTTPStatus.OK:
        print(response)
        return response[&#x27;output&#x27;][&#x27;choices&#x27;][0][&#x27;message&#x27;][&#x27;content&#x27;]
    else:
        print(&#x27;Request id: %s, Status code: %s, error code: %s, error message: %s&#x27; % (
            response.request_id, response.status_code,
            response.code, response.message
        ))
        raise Exception()
```

**api_retry **这个函数是当大模型调用api时可能会导致出错中断的问题，为了保证每个问题都被大模型处理过，我们需要设置一个反复尝试的函数。# 最大尝试次数5次 # 再次尝试等待时间 60秒。如果出现错误我们存储到日志文件。

```
**def api_retr****y**(MODEL_NAME, query):
    # 最大尝试次数
    max**_****r**e**t**ries = 5
    # 再次尝试等待时间
    retry_delay = 60  # in seconds
    attempts = 0
    while attempts &lt; max_retries:
        try:
            return call_qwen_api(MODEL_NAME, query)
        except Exception as e:
            attempts += 1   
            if attempts &lt; max_retries:
                logger.warning(f&quot;Attempt {attempts} failed for text: {query}. Retrying in {retry_delay} seconds...&quot;)
                time.sleep(retry_delay)
            else:
                logger.error(f&quot;All {max_retries} attempts failed for text: {query}. Error: {e}&quot;)
                raise
```

**get_prompt** prompt的模版函数，通过字符串处理的方式拼接完整的prompt

```
# 这里定义了prompt推**理**模版**
**
def ge**t**_**p**rompt(p**r**oblem, qu**e**stion, options**):
****
  **  op**tions = &#x27;\n&#x27;.jo**in(f&quot;{&#x27;ABCDEFG&#x27;[i]}. {o}&quot; for i, o in enumerate(options))

    prompt = f&quot;&quot;&quot;你是一个逻辑推理专家，擅长解决逻辑推理问题。以下是一个逻辑推理的题目，形式为单项选择题。所有的问题都是（close-world assumption）闭世界假设，即未观测事实都为假。请逐步分析问题并在最后一行输出答案，最后一行的格式为&quot;答案是：A&quot;。题目如下：

### 题目:
{problem}

### 问题:
{question}
{options}
&quot;&quot;&quot;
    # print(prompt)
    return prompt

```

### 3.2.2 抽取函数

通过抽取函数可以将大语言模型生成的结果抽取成答案对应的选项，这里的匹配原则和prompt呼应。我们可以看到prompt要求【最后一行的格式为"答案是：A"】这样的规范，那么我们采用正则表达式re.compile方法匹配到答案对应的选项。当我们匹配为空时，我们默认选"A"。

```
**#** 这里使用ex**t**ract**抽取**模获得抽取的结果

def e**x**t**r**act(input_text):
    ans_pattern = re.compile(r&quot;答案是：(.)&quot;, re.S)

    problems = ans_pattern.findall(input_text)
    # print(problems)
    if(problems == &#x27;&#x27;):
        return &#x27;A&#x27;
    return problems[0]
```

### 3.2.3 多线程处理

```
def proc**ess_datas**(datas,MODEL_NAME**)****:****
**    res**u**lts = []
    # 定义线程池 选择16线**程**
    with** **ThreadPoolExecutor(max_wo**r**kers=16)** **as executor:
        # 这里**我**们使用futu**r**e_data 存**储每个线程的数据
** ** **** **     future_**d**ata = {}
** **       # 这里的le**n**s记录了调用**a****p**i的次数，也就是我们每个问题背景下的所有子问**题**之和。
        lens = 0
        # 送入多线程任务
        # 这里每个data下是一个问题背景，其中包含多个子问题。
        for data in tqdm(datas, desc=&quot;Submitting tasks&quot;, total=len(datas)):
            problem = data[&#x27;problem&#x27;]
            # 这里面我们用enumerate方法每次循环得到问题的序号id和实际的问题。
            for id,question in enumerate(data[&#x27;questions&#x27;]):
                prompt = get_prompt(problem, 
                                    question[&#x27;question&#x27;], 
                                    question[&#x27;options&#x27;],
                                    )
                # 这里送入线程池等待处理，使用api_retry，向api_retry传入MODEL_NAME, prompt参数
                future = executor.submit(api_retry, MODEL_NAME, prompt)
                # 每个线程我们存储对应的json问题数据以及问题序号id，这样我们就能定位出执行的是哪个子问题
                future_data[future] = (data,id)
                time.sleep(0.6)  # 控制每0.6秒提交一个任务 防止接口超过并发数
                lens += 1
        # 处理多线程任务
        for future in tqdm(as_completed(future_data), total=lens, desc=&quot;Processing tasks&quot;):
            # print(&#x27;data&#x27;,data)
            # 取出每个线程中的字典数据及对应的问题id
            data = future_data[future][0]
            problem_id = future_data[future][1]
            try:
                # 获取api运行结果
                res  = future.result()
                # 抽取大语言模型返回结果
                extract_response = extract(res)
                # print(&#x27;res&#x27;,extract_response)
                # 装入answer字段
                data[&#x27;questions&#x27;][problem_id][&#x27;answer&#x27;] = extract_response
                # 在结果列表中新增数据字典
                results.append(data)
                # print(&#x27;data&#x27;,data)
                
            except Exception as e:
                logger.error(f&quot;Failed to process text: {data}. Error: {e}&quot;)
    
    return results
```

### 3.2.4 启动函数

```
def **main(ifn**, of**n):
    if os**.pat**h.exists(ofn**):
       ** ****p**ass
    data = **[**]
    # **按****行读取**数据
   ** wi****t****h** o**p**en(ifn) as r**e**ader:
        for line in reader:
            sample = json.loads(line)
            data.append(sample)
    datas = data
    # print(data)
    # 均匀地分成多个数据集
    return_list = process_datas(datas,MODEL_NAME)
    print(len(return_list))
    print(&quot;All tasks finished!&quot;)
    return return_list
    
if __name__ == &#x27;__main__&#x27;:
# 这里给了一个抽取范例参考
    a = extract(&quot;&quot;&quot;根据欧几里得算法，逐步解析计算两个数6和7的最大公约数（gcd）的步骤如下：

1. 判断6和7是否相等：不相等。
2. 判断6和7大小关系，7 &gt; 6，所以用更大的数7减去较小的数6得到结果1。
3. 现在计算6和1的最大公约数。
4. 6 &gt; 1，根据算法用更大的数6减去较小的数1得到结果5。
5. 再计算5和1的最大公约数。
6. 5 &gt; 1，用5减去1得到结果4。
7. 再计算4和1的最大公约数。
8. 4 &gt; 1，用4减去1得到结果3。
9. 再计算3和1的最大公约数。
10. 3 &gt; 1，用3减去1得到结果2。
11. 再计算2和1的最大公约数。
12. 2 &gt; 1，用2减去1得到结果1。
13. 最后计算1和1的最大公约数，两数相等，gcd即为这两个数，也就是1。

因此，6和7的最大公约数是1。

答案是：C.&quot;&quot;&quot;)

    print(a)
    # 调用主函数
    return_list = main(&#x27;round1_test_data.jsonl&#x27;, &#x27;upload.jsonl&#x27;)

```

## 3.3 纠错与结果文件生成

### 3.3.1 去重与排序

将一个问题背景下的所有问题存入同一个字典，并按id序号排序。

```
def **has_complete_answer(**questions):**
**    # **这**里假设完整答案的**判**断逻辑是：每个**q**uestion都有一个&#x27;**answer&#x27;键
 **** **  for q**u**estion in questions:
       ** **if &#x27;answe**r**&#x27; not in question:**
**         ** **  return **F**alse
    **r**eturn True

def fi**l**te**r**_problems**(**da**t**a):
    result = []
    problem_**s**et = set(**)**
**
**  ** ** for item in data:
        # print(&#x27;处理的item&#x27; ,item)
        problem = item[&#x27;problem&#x27;]
        if problem in problem_set:
            # 找到已存在的字典
            for existing_item in result:
                if existing_item[&#x27;problem&#x27;] == problem:
                    # 如果当前字典有完整答案，替换已存在的字典
                    if has_complete_answer(item[&#x27;questions&#x27;]):
                        existing_item[&#x27;questions&#x27;] = item[&#x27;questions&#x27;]
                        existing_item[&#x27;id&#x27;] = item[&#x27;id&#x27;]
                    break
        else:
            # 如果当前字典有完整答案，添加到结果列表
            if has_complete_answer(item[&#x27;questions&#x27;]):
                result.append(item)
                problem_set.add(problem)

    return result

return_list = filter_problems(return_list)
# 排序工作 通过id字段后三位代表序号
sorted_data = sorted(return_list, key=lambda x: int(str(x[&#x27;id&#x27;])[-3:]))
print(sorted_data)
```

### 3.3.2 纠错

```
def **find_mis****s**in**g**_ids**(dict_list):
 **   #** 提取所有序号
 ****   extra****cted_ids **= {int**(**d[&#x27;id&#x27;**]**[-3:]) for d in dict_list}
    
    # 创建0-500的序号集合
    all_ids = set(range(500))
    
    # 找出缺失的序号
    missing_ids = all_ids - extracted_ids
    
    return sorted(missing_ids)

# 示例字典列表
dict_list = sorted_data

# 找出缺失的序号
missing_ids = find_missing_ids(dict_list)
print(&quot;缺失的序号:&quot;, missing_ids)

len(missing_ids)
```

### 3.3.3 补错

针对空缺的列表我们进行补错，让每个answer字段默认填充为A，当然如果这种补错机制大家觉得不满意可以再送入多线程函数处理一边。

```
data  = []**
****w****i**th open(&#x27;**r****o**und1_t**e**st_d**a**t**a**.**j**so**n**l&#x27;) as reader:
    for id,line in enumerate(reader):
        if(id in missing_ids):
            sample = json.loads(line)
            for question in sample[&#x27;questions&#x27;]:
                question[&#x27;answer&#x27;] = &#x27;A&#x27;
            sorted_data.append(sample)
sorted_data = sorted(sorted_data, key=lambda x: int(str(x[&#x27;id&#x27;])[-3:]))
        
```

### 3.3.4 存储文件

```
with open(**&#x27;**upload.jsonl**&#x27;**, **&#x27;**w**&#x27;****)** a**s** writer:
    for sample in sorted_data:
        writer.write(json.dumps(sample, ensure_ascii=False))
        writer.write(&#x27;\n&#x27;)
```

# 继续迎接接下来的挑战吧！

<https://datawhaler.feishu.cn/sync/KI9PdPe6lsZXWsbMjiXcMBeOn7g>

> 本教程主要由   写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 ****署名—非商业性使用**—相同方式共享 4.0 协议国际版许可协议进行许可。