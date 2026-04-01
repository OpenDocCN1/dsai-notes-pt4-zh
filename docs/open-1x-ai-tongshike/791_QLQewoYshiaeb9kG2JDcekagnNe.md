# 数据集构建系列三：数据增强篇

> 来源：[https://datawhaler.feishu.cn/docx/GJFqdQWbMooCD7xUTKUcDtTZncc](https://datawhaler.feishu.cn/docx/GJFqdQWbMooCD7xUTKUcDtTZncc)

前篇：[零基础入门：低显存玩转DeepSeek-R1蒸馏模型Lora微调](https://rcnv3xvuzwi1.feishu.cn/wiki/IifdwX7pmi4AEPk0nSXckmd5nCb?from=from_copylink)

[数据集构建系列一：微调篇](https://rcnv3xvuzwi1.feishu.cn/wiki/W8GhwaQFHiMvFXk0kC8ca0TCnEg?from=from_copylink)

[数据集构建系列二：RAG向量知识库篇](https://rcnv3xvuzwi1.feishu.cn/wiki/J3j7wxlU7izURdkMugBcZsT7nDc?from=from_copylink)

[数据集构建系列三：数据增强篇](https://rcnv3xvuzwi1.feishu.cn/wiki/OdYZwCSksiTrcTk1yjwcQZblnDd?from=from_copylink)

# 一、什么是**数据增强**

**  数据增强（Data Aug**m**entat**ion）是通过对原始数**据进行修**改**或生成**新**数据**来[数据集构建系列一：微调篇](https://rcnv3xvuzwi1.feishu.cn/wiki/W8GhwaQFHiMvFXk0kC8ca0TCnEg#share-EnABdgcZDoM97sxn0u2cD7Eyn1f)充训练集的技术，旨在提高模型的泛化能力、鲁棒性和性能，尤其在数据量不足或数据多样性有限时发挥关键作用。通过生成新数据（如翻译、GAN生成样本）扩充训练集，提升模型泛化能力。更多相关介绍：

**核心目标**

1.  **增加数据多样性**：通过生成变体数据，模拟现实场景中的变化（如光照、噪声、语言风格等）。

1.  **防止过拟合**：减少模型对训练数据的记忆，增强对未见数据的适应能力。

1.  **解决数据不平衡**：例如对小样本类别进行过采样，缓解类别分布不均问题。

# 二、具体示例（心境大模型——基于deepseek垂直领域数据蒸馏构建数据集）

###  1.心境大模型参考链接

+   **代码仓库**：https://github.com/Kedreamix/Xinjing-LM

+   **Demo体验**：https://www.modelscope.cn/studios/Kedreamix/Xinjing-LM/

### 2.**数据配比**

+   **策略**: 在心理健康蒸馏微调中，我们将数据按 2:8 的比例分配，其中 20% 为通用数据，80% 为心理健康数据。

+   **目的**: 确保模型在保持通用能力的同时，专注于心理健康领域的任务

+   探索：我们也在探索短CoT预训练，后续长CoT在垂域上的一些效果，如后续在新的微调中，我们使用了4096以及更高的长度，但是这样也会导致模型占用的显存很大。这里学习的时候，由于希望能够让模型学习到更好的知识，并且保持鲁棒性我是将数据进行2:8分，2为通用的数据，8为心理健康的数据，希望有效果。

后续我们还会继续探讨数据的配比问题，以及我们前面实际上有尝试得到多轮对话的数据集，但是由于时间关系并没有全部拿进去训练，后续如果有时间的话，会考虑一下通用数据，通用推理数据，垂域推理数据的配比，比如deepseek官方说是用2:8的通用和通用推理数据的比例，我们也后续会探索在垂域是不是有特殊的配比。

### 3.数据集选择

为了确保模型在心理健康领域的表现，选择了以下数据集，并进行了针对性的优化：

+   Chinese-Data-Distill-From-R1：

+   **来源**: 开源中文蒸馏数据集，源自 DeepSeek-R1 模型，包含 110,000 个样本。

+   **内容**: 涵盖数学、考试、STEM 和通用类型数据，具体分为：

+   Math: 36,987 个样本

+   Exam: 2,440 个样本

+   STEM: 12,000 个样本

+   General: 58,573 个样本（包括逻辑推理、小红书、知乎等内容）

+   **特点**: 数据来源多样，经过蒸馏处理（temperature=0.6），数学数据增加了特定提示词，确保数据的多样性和专业性。

+   **链接**：https://modelscope.cn/datasets/liucong/Chinese-DeepSeek-R1-Distill-data-110k

+   psychology-10k-sft：

+   **内容**: 包含 10,000 条心理学相关指令微调数据，每条数据由输入（input）、输出（output）和指令（instruction）三部分组成。

+   **用途**: 帮助模型更好地理解和生成与心理学相关的文本内容，适用于心理学领域的自然语言处理任务。

+   **链接**: https://huggingface.co/datasets/DuongTrongChi/psychology-10k-sft

### 4.数据集构建

#### psychology-10k-sft-zh：

将 psychology-10k-sft 的英文数据通过 DeepSeek-V3 翻译为中文，确保模型能够处理中文心理学场景，后续发现社区有同类工作，不过我们的翻译会更加专业一点，会有一定的问题指向，原有的翻译会带多一点句号，不是很符合对话的标准。

```
import json
from openai import OpenAI
import time
import concurrent.futures
import random

clients = [
    OpenAI(api_key=&quot;XXX&quot;, base_url=&quot;https://api-inference.modelscope.cn/v1/&quot;),
    OpenAI(api_key=&quot;XXX&quot;, base_url=&quot;https://api-inference.modelscope.cn/v1/&quot;),
]
current_client_index = 0

def translate_text(text):
    &quot;&quot;&quot;使用OpenAI API进行翻译&quot;&quot;&quot;
    # client = random.choice(clients)  # 随机选择一个客户端
    global current_client_index
    client = clients[current_client_index]
    current_client_index = (current_client_index + 1) % len(clients)  # 轮换到下一个客户端
    try:
        response = client.chat.completions.create(
            model=&quot;deepseek-ai/DeepSeek-V3&quot;,
            messages=[
                {
                    &quot;role&quot;: &quot;system&quot;,
                    &quot;content&quot;: &quot;将以下英文内容准确翻译为中文，保持专业术语和心理学术语的正确性，并且最好是问题性的，不以问号结尾，按照以下格式输出: 翻译后的文本:&lt;翻译后的文本&gt;&quot;
                },
                {
                    &quot;role&quot;: &quot;user&quot;,
                    &quot;content&quot;: text
                }
            ],
            temperature=0.3,
            # timeout = 3,
        )
        # 使用正则表达式提取翻译后的文本
        import re
        translated_content = response.choices[0].message.content
        match = re.search(r&#x27;翻译后的文本:\s*(.*)&#x27;, translated_content)
        if match:
            text = match.group(1).strip()
            if text[0] == &#x27;&lt;&#x27; and text[-1] == &#x27;&gt;&#x27;:
                return text[1:-1]
            return match.group(1).strip()
        else:
            # print(&quot;警告：未找到标准翻译格式，返回原始响应&quot;)
            return translated_content
    except Exception as e:
        # print(f&quot;翻译出错: {str(e)}&quot;)
        pass
        return None


def process_item(item):
    &quot;&quot;&quot;处理单个数据项并返回翻译结果&quot;&quot;&quot;
    translated_input = translate_text(item[&#x27;input&#x27;])
    # 构建包含翻译后内容的字典
    translated_item = {
        &quot;input&quot;: translated_input,
    }
    return translated_item
from concurrent.futures import ThreadPoolExecutor, as_completed

def process_dataset(input_file, output_file, error_file, limit=1000, num_threads=4):
    print(input_file)
    &quot;&quot;&quot;处理整个数据集，但最多只处理前limit条记录&quot;&quot;&quot;
    with open(input_file, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        dataset = []
        for line in f:
            try:
                line = line.replace(&quot;&#x27;input&#x27;&quot;, &quot;\&quot;input\&quot;&quot;).replace(&quot;&#x27;instruction&#x27;&quot;, &quot;\&quot;instruction\&quot;&quot;).replace(&quot;&#x27;output&#x27;&quot;, &quot;\&quot;output\&quot;&quot;).replace(&quot;&#x27;}&quot;,&quot;\&quot;}&quot;).replace(&quot;instruction\&quot;: &#x27;&quot;, &quot;instruction\&quot;: \&quot;&quot;).replace(&quot;output\&quot;: &#x27;&quot;, &quot;output\&quot;: \&quot;&quot;).replace(&quot;input\&quot;: \&#x27;&quot;, &quot;input\&quot;: \&quot;&quot;)
                line = line.replace(&quot;&#x27;, \&quot;instruction&quot;, &quot;\&quot;, \&quot;instruction&quot;).replace(&quot;&#x27;, \&quot;output&quot;, &quot;\&quot;, \&quot;output&quot;)
                # print(len(line))
                dataset.append(json.loads(line))
            except Exception as e:
                print(f&quot;解析错误: {str(e)}，行内容: {line.strip()}&quot;)
    limit = min(limit, len(dataset))
    
    with open(output_file, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f_out, open(error_file, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f_err:  # 提前打开输出文件
        processed_count = 0  # 计数器，用于跟踪已处理的数据条目数量
        
        # 使用线程池进行并行处理
        with ThreadPoolExecutor(max_workers=num_threads) as executor:  # 设置并发线程数
            futures = {executor.submit(process_item, item): item for item in dataset[:limit]}  # 提交任务
            
            for future in as_completed(futures):
                processed_count += 1
                item = futures[future]
                try:
                    translated_item = future.result()
                    if translated_item[&#x27;input&#x27;] is None:
                        print(f&quot;处理第 {processed_count}/{limit} 条时出错&quot;)
                        f_err.write(str(item) + &#x27;\n&#x27;)  # 写入错误样本
                        f_err.flush()
                        continue
                    # 立即写入已翻译的数据
                    json_str = json.dumps(translated_item, ensure_ascii=False)
                    f_out.write(json_str + &#x27;\n&#x27;)
                    f_out.flush()  # 强制刷新缓冲区，确保数据被写入文件

                    print(f&quot;已处理并保存 {processed_count}/{limit} 条&quot;)
                except Exception as e:
                    print(f&quot;处理第 {processed_count}/{limit} 条时出错: {str(e)}&quot;)
                    f_err.write(str(item) + &#x27;\n&#x27;)  # 写入错误样本
                    f_err.flush()

    print(&quot;翻译完成！&quot;)


if __name__ == &quot;__main__&quot;:
    process_dataset(&quot;output_file.json&quot;, &quot;translated_dataset10k.json&quot;, &quot;error_samples.json&quot;, limit=1000000)
```

##### 核心代码解释

###### 1.**功能**：调用大模型API进行文本翻译

+   **关键点**：

+   使用temperature=0.3控制输出稳定性

+   系统提示词要求特定输出格式（翻译后的文本:<内容>）

+   正则表达式处理响应，支持带尖括号的特殊情况

+   客户端轮询策略实现简单负载均衡

```
def translate_text(text):
    global current_client_index
    client = clients[current_client_index]
    current_client_index = (current_client_index + 1) % len(clients)
    try:
        response = client.chat.completions.create(...)
        # 正则提取翻译结果
    except Exception as e:
        return None
```

###### 2.**功能：**数据集处理核心

+   **核心流程**：

1.  读取并修复格式错误的JSON数据

1.  创建线程池并行处理翻译任务

1.  使用as_completed实现异步结果处理

+   **关键设计**：

+   替换单引号等操作用于纠正数据格式错误

+   实时写入+flush()确保数据持久化

+   错误样本单独记录

```
def process_dataset(...):
    # 数据加载与预处理
    with open(input_file) as f:
        for line in f:
            # 多字符替换修复JSON格式
            dataset.append(json.loads(line))
    
    # 并行处理
    with ThreadPoolExecutor(...) as executor:
        futures = {executor.submit(process_item, item): item...}
        
        # 结果实时写入
        for future in as_completed(futures):
            json_str = json.dumps(...)
            f_out.write(json_str + &#x27;\n&#x27;)
```

#### **心理健康-R1蒸馏中文数据集-10k (psychology-**10k-Deepseek-R1-zh)：

1.  **内容**：为了得到推理的数据，我们利用deepseek R1 对 psychology-10k-sft-zh进行推理回答，生成思维链（think）和输出（content），增强模型的推理能力，为后续蒸馏做准备。

1.  **链接**：https://www.modelscope.cn/datasets/Kedreamix/psychology-10k-Deepseek-R1-zh

```
import json
from openai import OpenAI
from concurrent.futures import ThreadPoolExecutor, as_completed

# 初始化两个客户端，每个客户端对应一个API密钥
clients = [
    OpenAI(api_key=&quot;XXX&quot;, base_url=&quot;https://api-inference.modelscope.cn/v1/&quot;),
    OpenAI(api_key=&quot;XXX&quot;, base_url=&quot;https://api-inference.modelscope.cn/v1/&quot;),
]

current_client_index = 0


def inference_with_deepseek_r1(text):
    &quot;&quot;&quot;使用DeepSeek R1进行推理，并轮换API密钥&quot;&quot;&quot;
    global current_client_index
    client = clients[current_client_index]
    current_client_index = (current_client_index + 1) % len(clients)  # 轮换到下一个客户端
    print(f&quot;Question: {text}&quot;)
    # try:
    response = client.chat.completions.create(
        model=&quot;deepseek-ai/DeepSeek-R1&quot;,
        messages=[{
            &quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位专业心理咨询师，需根据来访者的问题描述提供符合专业且富有同理心的建议和指导。&quot;,
            &quot;role&quot;: &quot;user&quot;, &quot;content&quot;: text
            }
        ],
        temperature=0.6
    )
    think_process = response.choices[0].message.reasoning_content
    output_r1 = response.choices[0].message.content
    return think_process, output_r1, &quot;success&quot;
    # except Exception as e:
    #     print(f&quot;推理出错: {str(e)}&quot;)
    #     return None, None, &quot;error&quot;  # 返回错误信息


def process_item(item):
    &quot;&quot;&quot;处理单个数据项&quot;&quot;&quot;
    think, output_r1, error = inference_with_deepseek_r1(item[&#x27;input&#x27;])
    # if error:
    #     return {&quot;input&quot;: item[&#x27;input&#x27;], &quot;error&quot;: error}
    return {
        &quot;input&quot;: item[&#x27;input&#x27;],
        &quot;think&quot;: think,
        &quot;content&quot;: output_r1
    }

def process_dataset_in_reverse(input_file, output_file, error_file, limit=1000, num_threads=4):
    &quot;&quot;&quot;处理整个数据集，但最多只处理前limit条记录，从最后一条记录开始倒序处理&quot;&quot;&quot;
    
    # 读取已处理的输入，避免重复处理
    processed_inputs = set()
    try:
        with open(output_file, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f_out:
            for line in f_out:
                result = json.loads(line)
                processed_inputs.add(result[&#x27;input&#x27;])
            
    except FileNotFoundError:
        pass  # 如果输出文件不存在，则跳过

    with open(input_file, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        dataset = [json.loads(line) for line in f]
    limit = min(limit, len(dataset))
    
    with open(output_file, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f_out, open(error_file, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f_err:
        processed_count = 0
        
        # 使用线程池进行并行处理
        with ThreadPoolExecutor(max_workers=num_threads) as executor:  # 设置并发线程数
            futures = {executor.submit(process_item, item): item for item in dataset[:limit] if item[&#x27;input&#x27;] not in processed_inputs}  # 提交任务
            
            for future in as_completed(futures):
                processed_count += 1
                item = futures[future]
                try:
                    result = future.result()
                    if &#x27;error&#x27; in result:
                        f_err.write(json.dumps(result, ensure_ascii=False) + &quot;\n&quot;)  # 写入源数据
                        f_err.flush()
                        print(f&quot;处理第 {processed_count}/{limit} 条时出错: {result[&#x27;error&#x27;]}&quot;)
                    else:
                        json_str = json.dumps(result, ensure_ascii=False)
                        f_out.write(json_str + &quot;\n&quot;)  # 修正为每条记录单独写入
                        f_out.flush()
                        
                        print(f&quot;已处理并保存 {processed_count}/{limit} 条&quot;)
                except Exception as e:
                    print(f&quot;处理第 {processed_count}/{limit} 条时出错: {str(e)}&quot;)
                    f_err.write(json.dumps({&quot;input&quot;: item[&#x27;input&#x27;], &quot;error&quot;: str(e)}, ensure_ascii=False) + &quot;\n&quot;)
                    f_err.flush()

    print(&quot;推理完成！&quot;)

if __name__ == &quot;__main__&quot;:
    process_dataset_in_reverse(&quot;translated_dataset10k.json&quot;, &quot;inferred_dataset10k.json&quot;, &quot;error_log.json&quot;, limit=100000, num_threads=8)
```

##### 核心代码解释

###### 1.模型推理函数

+   调用DeepSeek R1模型进行心理咨询问答生成。

+   使用全局变量轮换API客户端，避免单个密钥的速率限制。

+   返回模型的思考过程（reasoning_content）和最终回答（content）。

```
def inference_with_deepseek_r1(text):
    global current_client_index
    client = clients[current_client_index]
    current_client_index = (current_client_index + 1) % len(clients)  # 轮换客户端
    print(f&quot;Question: {text}&quot;)
    response = client.chat.completions.create(
        model=&quot;deepseek-ai/DeepSeek-R1&quot;,
        messages=[{
            &quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位专业心理咨询师...&quot;,
            &quot;role&quot;: &quot;user&quot;, &quot;content&quot;: text
        }],
        temperature=0.6
    )
    think_process = response.choices[0].message.reasoning_content
    output_r1 = response.choices[0].message.content
    return think_process, output_r1, &quot;success&quot;
```

###### 2.数据处理单元

+   处理单个数据项，调用推理函数生成结果。

+   返回包含原始输入、模型思考过程和回答的字典。

```
def process_item(item):
    think, output_r1, error = inference_with_deepseek_r1(item[&#x27;input&#x27;])
    return {
        &quot;input&quot;: item[&#x27;input&#x27;],
        &quot;think&quot;: think,
        &quot;content&quot;: output_r1
    }
```

###### 3.主处理流程与并发控制

+   从input_file读取数据集，限制处理前limit条。

+   使用processed_inputs集合避免重复处理相同输入。

+   通过ThreadPoolExecutor启动多线程（num_threads控制并发数）。

+   动态提交任务，按完成顺序写入结果。

```
def process_dataset_in_reverse(input_file, output_file, error_file, limit=1000, num_threads=4):
    processed_inputs = set()
    # 读取已处理的数据避免重复
    try:
        with open(output_file, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f_out:
            for line in f_out:
                result = json.loads(line)
                processed_inputs.add(result[&#x27;input&#x27;])
    except FileNotFoundError:
        pass

    with open(input_file, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        dataset = [json.loads(line) for line in f]
    limit = min(limit, len(dataset))
    
    with open(output_file, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f_out, open(error_file, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f_err:
        with ThreadPoolExecutor(max_workers=num_threads) as executor:
            futures = {executor.submit(process_item, item): item 
                      for item in dataset[:limit] if item[&#x27;input&#x27;] not in processed_inputs}
            
            for future in as_completed(futures):
                # 结果写入与错误处理
                ...
```

###### 4.文件写入与错误处理

+   成功结果追加到output_file，错误信息写入error_file。

+   使用flush()确保实时写入，避免缓冲区未刷新导致数据丢失。

```
try:
    result = future.result()
    if &#x27;error&#x27; in result:
        f_err.write(json.dumps(result, ensure_ascii=False) + &quot;\n&quot;)
    else:
        f_out.write(json.dumps(result, ensure_ascii=False) + &quot;\n&quot;)
except Exception as e:
    f_err.write(json.dumps({&quot;input&quot;: item[&#x27;input&#x27;], &quot;error&quot;: str(e)}, ...))
```

#### **构造多轮对话数据集**：

参考EmoLLM-心理健康大模型构建数据集的方法，我们的数据集通过调用 DeepSeek R1 模型生成心理健康相关的对话数据，我们从配置文件中读取情感类型和生活场景列表，并为每种情感和场景组合生成多轮对话。通过构造特定的提示（prompt），模型生成符合要求的对话内容。

**构建方法**: 参考 EmoLLM 心理健康大模型的数据集构建方法，调用 DeepSeek R1 生成心理健康相关的多轮对话数据。

**特点**: 根据情感类型和生活场景组合生成对话，确保模型能够应对多样化的用户需求。

```
# -*- coding: utf-8 -*-
import json
import argparse
import yaml
import re
import os
from tqdm import tqdm
from openai import OpenAI

with open(&#x27;config.yml&#x27;, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
    configs = yaml.load(f.read(), Loader=yaml.FullLoader)

client = OpenAI(
    api_key=&quot;xxx&quot;,  # 请替换成您的真实API Key
    base_url=&quot;https://api-inference.modelscope.cn/v1/&quot;  # 根据实际情况填写
)


def deepseek_api(data, emo):
    prompt = f&#x27;&#x27;&#x27;你是一个研究过无数具有心理健康问题的病人与心理健康医生对话的专家，请你构造一些符合实际情况的具有心理健康问题的病人和心理健康医生的连续的多轮对话记录。
    要求病人的问题属于{data}场景，具有{emo}情感，医生的回复尽可能包含心理辅导知识，并且能够一步步诱导病人说出自己的问题进而提供解决问题的可行方案。
    注意，构造的数据必须以医生的陈述为结束语，每次只需要构造一个案例并且不需要写案例一、二等等，请返回完整的对话内容。
    请以如下格式返回生成的数据：
    病人：病人的咨询或陈述 
    医生：医生的安抚和建议
    &#x27;&#x27;&#x27;
    messages = [
        {
            &#x27;role&#x27;: &#x27;system&#x27;,
            &#x27;content&#x27;: &#x27;你现在是一个专业的心理咨询师。&#x27;
        },
        {
            &#x27;role&#x27;: &#x27;user&#x27;,
            &#x27;content&#x27;: prompt
        }
    ]
    try:
        response = client.chat.completions.create(
            model=&quot;deepseek-ai/DeepSeek-R1&quot;,  # ModleScope Model-Id
            messages=messages,
        )
        result = response.choices[0].message.content if hasattr(response, &#x27;choices&#x27;) else &#x27;ERROR&#x27;
        print(result)
    except Exception as e:
        print(&quot;Error:&quot;, e)
        result = &#x27;ERROR&#x27;
    return result


if __name__ == &#x27;__main__&#x27;:
    parser = argparse.ArgumentParser(description=&#x27;数据生成参数&#x27;)
    parser.add_argument(&#x27;--data&#x27;, type=str, help=&#x27;生活场景&#x27;)

    emotions_lis = configs[&#x27;emotions_list&#x27;]
    areas_of_life = configs[&#x27;areas_of_life&#x27;]
    ai_tool = &#x27;deepsseek-R1&#x27;

    total_num_each_emo_area = 5

    for area in areas_of_life:
        for emo in emotions_lis:
            gen_path = f&#x27;./{ai_tool}/{area}/{emo}.json&#x27;
            conversation_lis = []  # 在每种组合开始时初始化空列表

            for i in tqdm(range(total_num_each_emo_area), desc=&#x27;{emo}, {area}&#x27;.format(emo=emo, area=area)):
                one_conversation = {
                    &quot;conversation&quot;: []
                }

                res = deepseek_api(data=area, emo=emo)

                doctor_pattern = r&#x27;医生：(.*?)(病人：|$)&#x27;
                doctor_matches = re.findall(doctor_pattern, res, re.DOTALL)
                doctor_conversations = [match[0] for match in doctor_matches]

                patient_pattern = r&#x27;病人：(.*?)医生：&#x27;
                patient_matches = re.findall(patient_pattern, res, re.DOTALL)
                patient_conversations = [match for match in patient_matches]

                for doc, pat in zip(doctor_conversations, patient_conversations):
                    if len(one_conversation[&#x27;conversation&#x27;]) == 0:
                        one_conversation[&#x27;conversation&#x27;].append(
                            {
                                &quot;system&quot;: &quot;现在你是一个心理专家, 我有一些心理问题, 请你用专业的知识帮我解决。&quot;,
                                &quot;input&quot;: pat,
                                &quot;output&quot;: doc
                            },
                        )
                    else:
                        one_conversation[&#x27;conversation&#x27;].append(
                            {
                                &quot;input&quot;: pat,
                                &quot;output&quot;: doc
                            },
                        )

                conversation_lis.append(one_conversation)  # 将单个对话添加到列表中

            # 在所有对话生成完毕后一次性保存
            if not os.path.exists(os.path.dirname(gen_path)):
                os.makedirs(os.path.dirname(gen_path))
            with open(gen_path, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
                json.dump(conversation_lis, f, ensure_ascii=False, indent=4)

            print(f&#x27;All conversations generated and saved to {gen_path}&#x27;)
```

##### 核心代码解释

###### 1.**API调用函数** deepseek_api

+   根据输入的data（生活场景）和emo（情感类型），动态生成prompt，要求模型模拟心理咨询对话。

+   调用DeepSeek-R1模型生成多轮对话数据，格式严格限定为病人：... 医生：...。

+   捕获API调用异常，返回错误标记ERROR。

```
def deepseek_api(data, emo):
    # 构造动态prompt
    prompt = f&#x27;&#x27;&#x27;你是一个研究过无数具有心理健康问题的病人与心理健康医生对话的专家，请你构造一些符合实际情况的具有心理健康问题的病人和心理健康医生的连续的多轮对话记录。
    要求病人的问题属于{data}场景，具有{emo}情感，医生的回复尽可能包含心理辅导知识，并且能够一步步诱导病人说出自己的问题进而提供解决问题的可行方案。
    注意，构造的数据必须以医生的陈述为结束语，每次只需要构造一个案例并且不需要写案例一、二等等，请返回完整的对话内容。
    请以如下格式返回生成的数据：
    病人：病人的咨询或陈述 
    医生：医生的安抚和建议
    &#x27;&#x27;&#x27;
    
    messages = [
        {&#x27;role&#x27;: &#x27;system&#x27;, &#x27;content&#x27;: &#x27;你现在是一个专业的心理咨询师。&#x27;},
        {&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: prompt}
    ]
    
    try:
        response = client.chat.completions.create(
            model=&quot;deepseek-ai/DeepSeek-R1&quot;,  # 使用DeepSeek-R1模型
            messages=messages,
        )
        result = response.choices[0].message.content if hasattr(response, &#x27;choices&#x27;) else &#x27;ERROR&#x27;
    except Exception as e:
        print(&quot;Error:&quot;, e)
        result = &#x27;ERROR&#x27;
    return result
```

###### 2.**对话生成与结构化处理**

1.  **进度显示**:

+   使用 tqdm 显示进度条，描述当前处理的情感-场景组合。

1.  **API调用**:

+   调用 deepseek_api 生成原始对话文本（格式为 病人：... 医生：...）。

1.  **正则提取**:

+   **医生对话**：匹配 医生： 后的内容，直到遇到 病人： 或文本结束。

+   **病人对话**：匹配 病人： 后的内容，直到遇到 医生：。

+   清洗提取的文本（去除首尾空格）。

1.  **数据格式化**:

+   第一轮对话添加 system 字段定义角色，后续对话仅保留 input 和 output。

+   确保对话轮次按 病人→医生 交替顺序排列。

1.  **数据收集**:

+   将生成的单组对话添加到 conversation_lis 列表中。

```
for i in tqdm(range(total_num_each_emo_area), desc=&#x27;{emo}, {area}&#x27;.format(emo=emo, area=area)):
    one_conversation = {&quot;conversation&quot;: []}  # 单组对话容器
    res = deepseek_api(data=area, emo=emo)   # 调用API生成原始对话文本
    
    # 正则提取医生和病人的对话内容
    doctor_pattern = r&#x27;医生：(.*?)(病人：|$)&#x27;  # 匹配&quot;医生：&quot;后的内容，直到&quot;病人：&quot;或结束
    doctor_matches = re.findall(doctor_pattern, res, re.DOTALL)
    doctor_conversations = [match[0].strip() for match in doctor_matches]
    
    patient_pattern = r&#x27;病人：(.*?)医生：&#x27;     # 匹配&quot;病人：&quot;后的内容，直到&quot;医生：&quot;出现
    patient_matches = re.findall(patient_pattern, res, re.DOTALL)
    patient_conversations = [match.strip() for match in patient_matches]
    
    # 结构化对话数据
    for doc, pat in zip(doctor_conversations, patient_conversations):
        if len(one_conversation[&#x27;conversation&#x27;]) == 0:  # 第一轮对话
            one_conversation[&#x27;conversation&#x27;].append({
                &quot;system&quot;: &quot;现在你是一个心理专家...&quot;,  # 系统角色定义
                &quot;input&quot;: pat,                        # 病人提问
                &quot;output&quot;: doc                        # 医生回答
            })
        else:  # 后续对话
            one_conversation[&#x27;conversation&#x27;].append({
                &quot;input&quot;: pat,
                &quot;output&quot;: doc
            })
    
    conversation_lis.append(one_conversation)  # 添加当前生成的对话
```

# 三、完结感言

首先，非常感谢合作小伙伴Element和我一起共创数据集构建系列三。

其次，非常感谢Deepseek官网满血版以及kimi在本章的代码修改、资料收集以及文章润色方面提供的宝贵帮助！

这是本系列的第三篇，中间还有很多需要完善的地方，我们非常期待各位小伙伴的宝贵建议和指正，让我们共同进步，一起在AI学习的道路上探索更多乐趣！