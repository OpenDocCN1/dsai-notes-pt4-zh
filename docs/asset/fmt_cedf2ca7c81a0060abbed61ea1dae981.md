
# 创建 OpenAI 客户端（兼容 DeepSeek API）

```
# 创建 DeepSeek 客户端
client = OpenAI(
    api_key=DEEPSEEK_API_KEY,
    base_url=DEEPSEEK_BASE_URL
)

print("DeepSeek 客户端创建成功")
```

```
DeepSeek 客户端创建成功
```

客户端创建成功。接下来调用 API 进行信息提取。

## 调用 DeepSeek API 进行提取

现在正式调用大模型，让它按照我们的 Prompt 提取信息。这一步会发送网络请求，通常耗时 2-5 秒。

```
# 调用 DeepSeek API
print("正在调用 DeepSeek API...")
start_time = time.time()

response = client.chat.completions.create(
    model=DEFAULT_MODEL,
    messages=[
        {
            "role": "system",
            "content": "你是一个文本信息抽取助手，请严格按照用户指定的 JSON 格式输出，不要添加多余解释。"
        },
        {
            "role": "user",
            "content": prompt
        }
    ],
    temperature=0.3,
    max_tokens=2000,
    stream=False
)

result_prompt = response.choices[0].message.content
elapsed = time.time() - start_time

print(f"提取完成（耗时 {elapsed:.2f} 秒）")
```

```
正在调用 DeepSeek API...
提取完成（耗时 22.45 秒）
```

API 调用成功！接下来查看提取结果。

## 查看纯提示词的提取结果

打印大模型返回的原始结果。

```
# 打印原始结果
print("纯提示词提取结果:")
print("=" * 80)
print(result_prompt)
print("=" * 80)
```

```
纯提示词提取结果:
================================================================================
{
  "extractions": [
    {"class": "时间", "text": "2025年12月22日上午"},
    {"class": "机构", "text": "国家统计局"},
    {"class": "地点", "text": "北京国务院新闻办公室"},
    {"class": "事件", "text": "举行新闻发布会"},
    {"class": "事件", "text": "公布2025年前11个月国民经济运行情况"},
    {"class": "机构", "text": "国家统计局"},
    {"class": "人物", "text": "付凌晖"},
    {"class": "指标", "text": "6.1%"},
    {"class": "指标", "text": "7.3%"},
    {"class": "时间", "text": "同日"},
    {"class": "机构", "text": "国家发展改革委"},
    {"class": "机构", "text": "发改委"},
    {"class": "事件", "text": "在例行发布会上介绍"},
    {"class": "时间", "text": "2026年起"},
    {"class": "事件", "text": "对\"人工智能+制造\"试点城市给予专项资金支持"},
    {"class": "指标", "text": "10个"},
    {"class": "地点", "text": "上海"},
    {"class": "地点", "text": "深圳"},
    {"class": "地点", "text": "成都"},
    {"class": "机构", "text": "发改委"},
    {"class": "人物", "text": "李春临"},
    {"class": "事件", "text": "资金将重点投向算力基础设施和工业软件"},
    {"class": "事件", "text": "与地方财政配套安排相衔接"},
    {"class": "时间", "text": "22日傍晚"},
    {"class": "机构", "text": "中国人民银行"},
    {"class": "机构", "text": "央行"},
    {"class": "事件", "text": "公告"},
    {"class": "时间", "text": "12月23日起"},
    {"class": "事件", "text": "下调金融机构存款准备金率"},
    {"class": "指标", "text": "0.25个百分点"},
    {"class": "机构", "text": "央行"},
    {"class": "人物", "text": "宣昌能"},
    {"class": "事件", "text": "答记者问"},
    {"class": "事件", "text": "保持流动性合理充裕"},
    {"class": "事件", "text": "支持中小微企业融资"},
    {"class": "机构", "text": "清华大学经济管理学院"},
    {"class": "人物", "text": "李稻葵"},
    {"class": "事件", "text": "上述措施有助于稳定市场预期"},
    {"class": "事件", "text": "需警惕部分行业\"低价竞争\"风险"},
    {"class": "事件", "text": "地方政府隐性债务化解仍是明年宏观政策的重要约束"}
  ]
}
================================================================================
```

可以看到，大模型返回了一个 JSON 格式的提取结果。我们解析并统计纯提示词的结果：

```
# 解析 JSON 并统计
prompt_result = json.loads(result_prompt)
prompt_extractions = prompt_result["extractions"]

# 基本统计
total_count_prompt = len(prompt_extractions)
type_counts_prompt = Counter(ext["class"] for ext in prompt_extractions)

print(f"纯提示词统计:")
print(f"  总实体数: {total_count_prompt}")
print(f"  按类型分布:")
for entity_type, count in sorted(type_counts_prompt.items()):
    print(f"    - {entity_type}: {count} 个")
```

```
纯提示词统计:
  总实体数: 40
  按类型分布:
    - 事件: 14 个
    - 人物: 4 个
    - 地点: 4 个
    - 指标: 4 个
    - 时间: 5 个
    - 机构: 9 个
```

统计完成。可以看到纯提示词提取了大量实体。但数量多不代表质量高——接下来我们将检查重复问题。

## 检查纯提示词的重复问题

检查是否存在重复提取的实体。

```
# 检查重复
text_counts = Counter(ext["text"] for ext in prompt_extractions)
duplicates = {text: count for text, count in text_counts.items() if count > 1}

if duplicates:
    print("发现重复实体:")
    for text, count in duplicates.items():
        print(f"  - '{text}' 重复了 {count} 次")
    
    total_duplicates = sum(count - 1 for count in duplicates.values())
    unique_count_prompt = total_count_prompt - total_duplicates
    
    print(f"\n去重后实际数量: {unique_count_prompt} 个（减少了 {total_duplicates} 个重复）")
else:
    print("✓ 未发现重复实体")
    unique_count_prompt = total_count_prompt
```

```
发现重复实体:
  - '国家统计局' 重复了 2 次
  - '发改委' 重复了 2 次
  - '央行' 重复了 2 次

去重后实际数量: 37 个（减少了 3 个重复）
```

可以看到，纯提示词存在明显的重复问题！这会导致：

1. 数量虚高
2. 需要人工去重

接下来我们运行 `LangExtract` 方式。这个方案使用 `LangExtract` 框架，它会自动处理去重、原文对齐等细节。

## 定义 LangExtract 的任务描述

`LangExtract` 的 Prompt 比纯提示词简洁得多，因为它内置了很多最佳实践。

```
# 定义 LangExtract 的任务描述
langextract_prompt = """
从新闻文本中提取以下信息：
- 时间
- 地点
- 机构
- 人物
- 事件
- 指标（数值/增速/比例/数量等）

要求：
1. 使用原文中的完整表述
2. 不要重复
3. 按出现顺序提取
"""

print("LangExtract 任务描述已定义")
print(f"Prompt 长度: {len(langextract_prompt)} 字符（比纯提示词简洁得多）")
```

```
LangExtract 任务描述已定义
Prompt 长度: 97 字符（比纯提示词简洁得多）
```

任务描述定义完成。注意它比纯提示词的 Prompt 简洁很多，因为 `LangExtract` 会自动处理输出格式、去重等细节。

## 定义 LangExtract 的 Few-shot 示例

使用 `LangExtract` 的数据结构定义 Few-shot 示例（与纯提示词使用完全相同的示例文本）。

```
# 定义 LangExtract 的 Few-shot 示例
examples = [
    lx.data.ExampleData(
        text=example_text,
        extractions=[
            lx.data.Extraction("时间", "2025年6月3日"),
            lx.data.Extraction("机构", "工业和信息化部"),
            lx.data.Extraction("地点", "北京"),
            lx.data.Extraction("人物", "张云明"),
            lx.data.Extraction("事件", "发布《算力基础设施高质量发展行动计划》"),
            lx.data.Extraction("指标", "300 EFLOPS"),
        ]
    )
]

print("LangExtract Few-shot 示例已定义")
print(f"示例数量: {len(examples)}")
print(f"示例实体数: {len(examples[0].extractions)}")
```

```
LangExtract Few-shot 示例已定义
示例数量: 1
示例实体数: 6
```

Few-shot 示例定义完成。注意这里使用了与纯提示词完全相同的示例文本和标准答案，确保对比的公平性。

## 初始化 LangExtract 模型

创建 `LangExtract` 的模型实例（使用相同的 DeepSeek API）。
