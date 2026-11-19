
# 开发：CES 2024 行业图谱WIP

> 来源：[https://v11enp9ok1h.feishu.cn/docx/NWCqd1DZSoV2A0xddyuc2jftnwd](https://v11enp9ok1h.feishu.cn/docx/NWCqd1DZSoV2A0xddyuc2jftnwd)

<https://v11enp9ok1h.feishu.cn/sync/PaIIdslbxsIJUIbkTfPcsMCFnph>

价值100美金（消耗了至少100美金token）

## 下载阅读

推荐英文版

[`CES_2024_Industry_Mapping.xlsx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Tmeub2JMpo75HXxUBWdchLilnQg)

Google翻译版

GPT翻译中文版：

![](img/f6132e34a7d8ca02233a089d9bf07edf.png)

[`CES2024行业图谱.xlsx`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/NECRbAxbYogVUnxzO4vcHSfpnad)

## 项目背景

这是一个实验，探索在AI时代能否根据信息框架归纳资讯，提高阅读效率。

CES行业图谱覆盖：

+ CES 4300家参展商产品信息，47个行业最新动态。
+ 每间公司搜集10个相关新闻报道，700M文本数据。
+ 用ChatGPT总结，汇总成一个行业图谱，便于研究。

项目难点：

+ 20%企业是startup，没有网络信息，容易造成幻觉，需多信息源交叉验证，幻觉控制在5%。

### 总体情况

+ 本届主题是AI for All。
+ 大语言模型提高I/O效率，甚至成为I/O系统雏形。
+ 大语言模型给硬件应用场景、form factor、新交互方式提供机会。家庭、汽车是目前的主战场，面向现实场景的个人硬件层出不穷，例如Rabbit R1。

![](img/9b36ba8e53a662222afe8679ebb11b34.png)

## 研发实现

### Pipeline

整个技术管线分为爬虫、归纳、修复、输出四个环节。创新点：

+ 级联设计：任务1爬虫和任务2归纳同时执行，提高效率。
+ 多模型配合：
+ 90%由ChatGPT 3.5执行归纳，控制成本。
+ 10%由ChatGPT 4.0执行归纳1024，修复幻觉。
+ DeepL执行最后的翻译工作。

### 资讯收集

首先，我们需要一个搜索引擎，搜索相关信息在什么地方。与爬虫软件不同，爬虫是针对单个网页（如amazon）或指定几个网址收集信息。但假如网页在哪里都不知道，就需要搜索引擎找到才能爬虫。这里推荐Tavily API接口，集成了搜索、爬虫等功能，免费。

| 资讯渠道 | 优点 | 缺点 |
| --- | --- | --- |
| GPTs Bing Search | quick search为主，信息不一定全面，但整体效果不错。 | 无法提供API访问接口，不符合项目需求 |
| Azure Bing Search | 可API访问，每个月1000次免费 | 只返回前5条信息，而且只有url和网页摘要，没有网页原文。 |
| Tavily API[推荐] | 提供广泛API搜索库 | 因为没有商业化，服务不稳定。 |

Python执行过程

原始代码作为Prompt的参考代码，自己可根据需求让GPT参照编程。

1. 从CES官方收集4000+参展商名单，作为Tavily搜索爬虫的任务列表。
1. Tavily根据这份名单爬虫，每一家都以`json`文件方式保存到`Crawler`目录。

```
import openpyxl
```

### 总结归纳

1. Tavily收集回来原始数据需要清洗，可减少幻觉现象。filter包括：

+ 原始数据需包含参展商公司名字

1. 选择一个大语言模型基座进行信息归纳。考虑因素如下：

+ Context windows越大，可从越多原始数据中找到产品相关信息。
+ 价格成本。原本没做限制100k tokens原始文档直接给到GPT 4，一次请求就1美金。后来限制单次请求限制10k tokens，而且转为GPT 3.5模型，成本才得到控制。
+ CES是海外活动，英语语料为主，不考虑中文基座模型。

| 模型 | 优点 | 缺点 |
| --- | --- | --- |
| Google Gemini | 32k context windows | prompt指令语义理解非常差，需设置多条明确指令才懂执行。 |
| ChatGPT 4.0 | 128k context windows | 太贵了。全部跑这个接口需要4000美元。 |
| ChatGPT 3.5 | 16k context windows | 信息归纳效果勉强可以，价格可以接受。 |

Python执行过程

+ 对`crawler`目录爬虫数据进行逐一归纳，另存到`Summary`目录下。

```
import os
```

### 幻觉修复

ChatGPT 3.5回应中有15%的幻觉，更换ChatGPT 4修复。

原因如下

+ 数据质量低，没有厂商的信息，Tavily只找到CES overall的信息。
+ ChatGPT 3.5无法区别品牌与CES之间关系，会让CES热点信息代替品牌信息。

解决方案

+ 更换信息源，换成Azure Bing Search，增加信息覆盖范围。因请求量较大，请使用付费接口，避免免费额度不足导致中断。
+ 更换大语言模型，换成ChatGPT 4.0。

Python代码执行

+ 在`Summary`目录检索一些幻觉关键字，如“transparent”、“rabbit r1”等，这些都是CES 2014热门词汇。
+ 当`json`文件出现这些词汇，`python`会提取该文件的品牌名称，在Bing再搜索一次让ChatGPT4归纳修复。

```
import os
import json
import requests
from openai import OpenAI

# Load your API keys from environment variables or config files
azure_subscription_key = "API_KEY" # 填写Azure api key，用企业邮箱申请就可以申请免费1000次额度。
openai_api_key = "API_KEY" # 填写openai api key

# Initialize OpenAI client
openai_client = OpenAI(api_key=openai_api_key)

# Directory containing JSON files
json_dir = "Summary" # 需要检查的目录

# Function to search using Bing
def bing_search(query):
    headers = {"Ocp-Apim-Subscription-Key": azure_subscription_key}
    params = {"q": query, "textDecorations": True, "textFormat": "HTML"}
    response = requests.get("https://api.bing.microsoft.com/v7.0/search", headers=headers, params=params)
    return response.json()

# Function to summarize using GPT-3.5
def summarize_with_gpt(filtered_results, question):
    if not filtered_results:
        return "No relevant brand information found."
    prompt = f"Based on raw content as below, structurally and briefly summarize {question}. Summary content is strictly limited to 150 words.\nIf raw content is empty, just reply no infomation. \nIf there is no information about the brand mentioned in front, just reply no infomation. CES is a exhibition event, not a company name or brand. The key words before the CES is the company name.\n\nOutput reference format:\n\nBrand or Company name\n\nProduct 1\n\n -feature 1\n\n -feature 2\n\n -feature 3\n\n and so on\n\n Raw Content:\n\n"
    prompt += " ".join(filtered_results)
    print(prompt)
    response = openai_client.chat.completions.create(
        model="gpt-4-1106-preview",  # 换了目前最好的模型基座
        messages=[{"role": "system", "content": "You are a product expert who summarizes the specifications and features of products based on the raw content, ignoring irrelevant content from other brand. The summary should be limited to 150 words."}, {"role": "user", "content": prompt}],
        max_tokens=150,   # 指定归纳的字数
        temperature=0.5)   # 让AI遵循文本进行归纳分析
    print(response.choices[0].message.content)
    return response.choices[0].message.content

# Process each JSON file in the directory
for filename in os.listdir(json_dir):
    if filename.endswith(".json"):
        file_path = os.path.join(json_dir, filename)
        with open(file_path, 'r') as file:
            data = json.load(file)
            summary = data.get("Summary")
            if summary and ("transparent" in summary or "No information" in summary): # 设置多个幻觉关键词
                question = data.get("Question", "Unknown")
                # 假如发现有幻觉内容，就提取公司名称
                filtered_words = [word for word in question.split()[:3] if word.lower() not in ['ces', '2024', 'and', 'the', 'co.,', 'ltd.', 'technology', 'inc.', 'ltd', 'co.,ltd', 'co.,ltd.', 'electronics', 'limited', 'electronic', 'inc', 'technologies', 'llc', 'corporation', 'international', 'tech', 'group', 'gmbh', 'industrial', 'company', 'corp.']] # 过滤无用关键词后，再送到BING搜索
                print(filtered_words)
                search_results = bing_search(question)
                filtered_results = [snippet["snippet"] for snippet in search_results["webPages"]["value"] if any(word.lower() in snippet["snippet"].lower() for word in filtered_words)]
                new_summary = summarize_with_gpt(filtered_results, question)
                # GPT4根据BING内容修复
                # Update the Summary field
                data["Summary"] = new_summary
                # Save the updated JSON file
                with open(file_path, 'w') as file:
                    json.dump(data, file, indent=4)
```

### 合并输出

将多个关联文件合并

```
import pandas as pd
```
