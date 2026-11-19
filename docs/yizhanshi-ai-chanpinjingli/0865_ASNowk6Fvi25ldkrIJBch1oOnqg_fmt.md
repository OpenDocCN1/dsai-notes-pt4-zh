
# 企业舆情分析

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Crj1d6FW9oFcplxh02rcnbCtnef](https://v11enp9ok1h.feishu.cn/docx/Crj1d6FW9oFcplxh02rcnbCtnef)

![](img/fe86c8874ce77bb6813a87bf3ef21cfa.png)

## 前言

感谢星环科技提供的舆情分析模板，本项目[基于该模板做了优化和多维表](https://www.coze.cn/template/project/7468212330137124915?from=store_search_suggestion)格扩展，模板：扣子-AI 智能体开发平台

关于本工作流的相关配置，可以在学习群@助教或者添加作者飞书

### 效果

> 建议“进入原多维表格”

<https://zxdwhda-share.feishu.cn/share/base/dashboard/shrcnHcgxaz0kj7G8iLrY0Zpj0c?iframeFrom=docx&ccm_open=iframe>

### 产品设计思路

> 为什么要有这些功能

## coze设计

### 整体分析

#### 输入节点

`topic-搜索词`

`cnt-搜索数量`

#### query拼接

给 `topic-搜索词` 加上最新新闻

#### 搜索选择器

> 因为📕 cookie 限制的问题，和调试阶段需要快速调试，这里加上了“搜索选择器”

#### 🔎 搜索模块

聚合了搜狐热闻、联网问答、头条搜索📕搜索、12+热榜内容

更多内容参考：第3部分[企业舆情分析](https://zxdwhda-share.feishu.cn/wiki/An82w6wc4iwoYik2iqVcwbPunBb#ZWctdnU0hovzXDxx5ENc0IkSnJg)

#### 聚合节点

聚合所有来源的数据

```
async def main(args: Args) -> Output:
    params = args.params

    # 安全获取输入参数并确保列表类型
    input1 = params.get('input1', []) if isinstance(params, dict) else []
    input2 = params.get('input2', []) if isinstance(params, dict) else []
    input3 = params.get('input3', []) if isinstance(params, dict) else []
    input4 = params.get('input4', []) if isinstance(params, dict) else [] 
    input5 = params.get('input5', []) if isinstance(params, dict) else []
    
    # 将input4加入处理列表
    inputs = [
        i if isinstance(i, list) else []
        for i in [input1, input2, input3, input4, input5]  # 包含input
    ]

    def is_valid(item):
        """验证条目有效性"""
        return isinstance(item, dict) and all(
            key in item and 
            isinstance(item[key], str) and 
            item[key].strip()  # 排除空字符串
            for key in ["url", "title", "summary", "source"]
        )

    output = []
    for input_data in inputs:
        for item in input_data:
            # 过滤特定空结果模板
            if (
                isinstance(item, dict) and
                item.get("title") == "No Results Found" and
                item.get("summary") == "Your search did not match any documents." and
                item.get("url") == "" and
                item.get("source") == ""
            ):
                continue
            
            if is_valid(item):
                output.append({
                    "url": item["url"].strip(),
                    "title": item["title"].strip(),
                    "summary": item["summary"].strip(),
                    "source": item["source"].strip()
                })

    return {"output": output}
```

![](img/4fe1759fc78bb0da0c1b5ec9caa4a91f.png)

#### 其他节点

统计数据总量

```
async def main(args: Args) -> Output:
    params = args.params
    input_array = params['input']
    
    # 统计数组长度
    count = len(input_array)
    
    # 构建输出对象
    ret: Output = {
        "count": count
    }
    return ret
```

获取当前系统时间

```
async function main({ params }: Args): Promise<Output> {
    const now = new Date();

    const year = now.getFullYear();
    const month = String(now.getMonth() + 1).padStart(2, '0'); // 月份从0开始, 需加1，并保证两位数格式
    const day = String(now.getDate()).padStart(2, '0'); // 保证两位数格式

    const dateString = `${year}-${month}-${day}`;

    const ret = {
        time: dateString
    };

    return ret;
}
```

#### ⭐️ 内容关键词模块

更多内容参考：第4部分[企业舆情分析](https://zxdwhda-share.feishu.cn/wiki/An82w6wc4iwoYik2iqVcwbPunBb#Kd5PdYEGOoLaYgxJCsVc3GElndg)

#### 📈 舆情分析模块

更多内容参考：第5部分[企业舆情分析](https://zxdwhda-share.feishu.cn/wiki/An82w6wc4iwoYik2iqVcwbPunBb#E6cDdUoPHod5zLxOEc4cyBnvn3c)

#### 🛠处理模块

更多内容参考：第6部分[企业舆情分析](https://zxdwhda-share.feishu.cn/wiki/An82w6wc4iwoYik2iqVcwbPunBb#Tt7cdy6rPops5TxZMygcJGPVnQc)

## 🔎 搜索模块详解

### 普通搜索

搜狐热闻：https://www.coze.cn/store/plugin/7343894357063385100

```
async def main(args: Args) -> Output:
    # 安全获取参数，处理可能的缺失或类型错误
    params = getattr(args, 'params', {})
    input_array = params.get('input', []) if isinstance(params, dict) else []
    if not isinstance(input_array, list):
        input_array = []
    
    output_data = []
    
    def escape_single_quote(text: str) -> str:
        return text.replace("'", "\\'")
    
    for item in input_array:
        # 跳过无效条目
        if not isinstance(item, dict):
            continue
        
        # 提取字段并验证类型
        url = item.get("url")
        title = item.get("title")
        brief = item.get("brief")
        
        # 检查必要字段是否存在且为非空字符串
        if not all([
            isinstance(url, str) and url.strip(),
            isinstance(title, str),
            isinstance(brief, str)
        ]):
            continue
        
        # 转义字段并构建结果
        transformed_item = {
            "url": escape_single_quote(url),
            "title": escape_single_quote(title),
            "summary": escape_single_quote(brief),
            "source": "搜狐新闻"
        }
        output_data.append(transformed_item)
    
    # 处理空结果情况
    if not output_data:
        output_data = [{
            "url": "",
            "title": "No Results Found",
            "summary": "Your search did not match any documents.",
            "source": "搜狐新闻"
        }]
    
    return {"data_sohu": output_data}
```

联网问答（扣子搜索）：https://www.coze.cn/store/plugin/7380276977141465088

```
async def main(args: Args) -> Output:
    params = args.params
    input_array = params['input']
    
    output_data = []
    
    def escape_single_quote(text: str) -> str:
        return text.replace("'", "\\'")
    
    for item in input_array:
        if item is None:
            continue
        if not isinstance(item, dict):
            continue
        # 添加对sitename字段的检查
        if "url" not in item or "title" not in item or "summary" not in item or "sitename" not in item:
            continue
        if not item["url"]:
            continue

        transformed_item = {
            "url": escape_single_quote(item["url"]),
            "title": escape_single_quote(item["title"]),
            "summary": escape_single_quote(item["summary"]),
            "source": escape_single_quote(item["sitename"])
        }
        output_data.append(transformed_item)
    
    ret: Output = {
        "data_general": output_data
    }
    return ret
```

头条搜索：https://www.coze.cn/store/plugin/7328315124756807717



```
async def main(args: Args) -> Output:
    params = args.params
    input_array = params['input']
    
    # 按后续环节要求赋值data
    output_data = []
    
    def escape_single_quote(text: str) -> str:
        return text.replace("'", "\\'")
    
    for item in input_array:
        if item is None:
            continue
        if not isinstance(item, dict):
            continue
        if "url" not in item or "title" not in item or "summary" not in item:
            continue
        if not item["url"]:
            continue
            
        transformed_item = {
            # 将代表网页链接的字段输出为url
            "url": escape_single_quote(item["url"]),
            # 将代表网页标题的字段输出为title
            "title": escape_single_quote(item["title"]),
            # 将代表网页总结的字段输出为summary
            "summary": escape_single_quote(item["summary"]),
            # source字段，取sitename的值并转义
            "source": escape_single_quote(item["sitename"])
        }
        output_data.append(transformed_item)
    
    # 构建输出对象
    ret: Output = {
        "data_toutiao": output_data
    }
    return ret
```

# 📕搜索

📕插件——`xhs_search_note`工具：https://www.coze.cn/store/plugin/7413263498387423242

> 根据【关键词】搜索【📕笔记】

📕笔记提取：https://www.coze.cn/store/plugin/7426236806493995062

> 获取📕笔记信息添加到我的智能体

批处理：

![](img/f6e6eee704fcb0295e89c8d90dc743f7.png)

批处理体：

```
from typing import TypedDict

# 1. 定义包含所有字段的类型
class Output(TypedDict):
    title: str
    summary: str
    url: str
    source: str  # 必须包含所有字段

async def main(args: Args) -> Output:
    params = args.params
    # 2. 严格匹配类型定义
    ret: Output = {
        "title": params['title'],
        "summary": params['summary'],
        "url": params['url'],
        "source": "小红书"  # 固定值
    }
    return ret
```

![](img/b6c4dc9b9da0f959b638fd8b9cf78f39.png)

# 热榜检索

热搜榜聚合：https://www.coze.cn/store/plugin/7351343435422433280

筛选知乎链接

是否与关键词相关

> 大模型节点

```
# 角色
你是一个舆情监控专家，擅长分析各类信息与总结内容之间的相关性，能够精准判断关键词和新闻标题是否相关，并给出合理的判断依据。

## 技能
### 技能 1: 判断相关性
1. 当用户提供关键词和新闻标题时，仔细分析两者的内容、主题、关键信息等方面。
2. 通过对内容语义、逻辑关系的理解，判断关键词和新闻标题是否相关。
3. 给出明确的判断结果（相关或不相关），并详细阐述判断依据。

## 限制:
- 只围绕判断关键词和新闻标题是否相关回答，拒绝回答无关话题。
- 输出内容需清晰呈现判断结果和依据，不能偏离框架要求。

## 输出
{Boolean}}：<false/true>  ，false代表无关，true代表有关
```

```
用户提供关键词：{{topic}}
新闻标题：{{title}}
```

判断是否相关输出

`格式化`

```
async def main(args: Args) -> Output:
    params = args.params
    ret: Output = {
        "output":{
            "title": params['title'],
            "summary": params['summary'],
            "url": params['url'],
            "source": params['source']
        }
    }
    return ret
```

`格式化_热榜数据处理`

![](img/ade6305ad5e65173f10088cc0d577d6d.png)

```
async def main(args: Args) -> Output:
    params = args.params
    
    # 提取输入的三维数据结构
    input_urls = params.get("url", [])
    input_titles = params.get("title", [])
    input_summaries = params.get("summary", [])
    input_sources = params.get("source", [])
    
    output_data = []
    
    def escape_single_quote(text: str) -> str:
        return text.replace("'", "\\'") if text else ""
    
    # 确保数组长度一致
    max_length = max(len(input_urls), len(input_titles), len(input_summaries))
    
    for i in range(max_length):
        # 防御性提取字段
        url = str(input_urls[i]) if i < len(input_urls) else ""
        title = str(input_titles[i]) if i < len(input_titles) else ""
        summary = str(input_summaries[i]) if i < len(input_summaries) else ""
        source = str(input_sources[i]) if i < len(input_sources) else ""
        
        # 过滤空URL条目
        if not url.strip():
            continue
            
        # 构建符合要求的输出对象
        transformed_item = {
            "url": escape_single_quote(url),
            "title": escape_single_quote(title),
            "summary": escape_single_quote(summary),
            "source": escape_single_quote(source)
        }
        output_data.append(transformed_item)
    
    # 空结果兜底逻辑
    if not output_data:
        output_data.append({
            "url": "",
            "title": "No Valid Data Found",
            "summary": "No processed items meet the criteria",
            "source": ""
        })
    
    return {"data_toutiao": output_data}
```

# ⭐️ 话题模块详解

# 📈 舆情分析模块详解

# 🛠处理模块详解

大家再添加链接的时候，从地址中复制，不要选择分享

![](img/5830323d0aa3cb0671c8d5d8c798f763.png)

# 多维表格设计

[Crisis monitor and response 副本](https://zxdwhda-share.feishu.cn/wiki/VS8jwixqgivHjLkPvGEcVRKVnv8)

大家可以直接复制

# 多智能体设计

https://www.coze.cn/store/agent/7502710927675523109?bot_id=true

![](img/6750d03445df4eb36eca923b6c3dde09.png)

# 提醒

大家不要忘了打卡 Task2
