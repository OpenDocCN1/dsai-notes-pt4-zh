
这个 Prompt 包含了几个关键要求：

1. **提取目标**：人物、情感、关系

1. **属性要求**：为每个实体添加属性，增加上下文信息

1. **精确对齐**：`extraction_text` 必须是原文精确子串

1. **戏剧格式**：角色名称后跟冒号，这是戏剧脚本的特殊格式

接下来，我们需要提供 **Few-shot 示例**，引导大模型正确提取。

### 5.4 定义 Few-shot 示例

Few-shot 示例是 LangExtract 的核心。通过提供高质量的示例，我们可以引导模型理解“什么是好的提取”。我们将使用一段经典的罗密欧与朱丽叶对话作为示例。

```python
# 定义 Few-shot 示例
examples = [
    lx.data.ExampleData(
        text=textwrap.dedent("""\
            罗密欧: 轻声！那边窗子里亮起来的是什么光？
            那就是东方，朱丽叶就是太阳。
            朱丽叶: 啊！罗密欧，罗密欧！为什么你偏偏是罗密欧呢？"""),
        extractions=[
            lx.data.Extraction(
                extraction_class="人物",
                extraction_text="罗密欧",
                attributes={"情感状态": "惊叹"}
            ),
            lx.data.Extraction(
                extraction_class="情感",
                extraction_text="轻声！",
                attributes={"情感": "温柔敬畏", "人物": "罗密欧"}
            ),
            lx.data.Extraction(
                extraction_class="关系",
                extraction_text="朱丽叶就是太阳",
                attributes={"类型": "比喻", "人物1": "罗密欧", "人物2": "朱丽叶"}
            ),
            lx.data.Extraction(
                extraction_class="人物",
                extraction_text="朱丽叶",
                attributes={"情感状态": "渴望"}
            ),
            lx.data.Extraction(
                extraction_class="情感",
                extraction_text="为什么你偏偏是罗密欧呢？",
                attributes={"情感": "渴望的疑问", "人物": "朱丽叶"}
            ),
        ]
    )
]

print("Few-shot 示例定义完成！")
print(f"\n示例数量: {len(examples)}")
print(f"示例文本长度: {len(examples[0].text)} 字符")
print(f"示例提取数量: {len(examples[0].extractions)} 个")
print("\n" + "=" * 60)
print("示例文本：")
print("=" * 60)
print(examples[0].text)
```

```
Few-shot 示例定义完成！

示例数量: 1
示例文本长度: 65 字符
示例提取数量: 5 个

============================================================
示例文本：
============================================================
罗密欧: 轻声！那边窗子里亮起来的是什么光？
那就是东方，朱丽叶就是太阳。
朱丽叶: 啊！罗密欧，罗密欧！为什么你偏偏是罗密欧呢？
```

这个 Few-shot 示例展示了高质量提取的标准：

1. **人物提取**：提取"罗密欧"和"朱丽叶"，并添加"情感状态"属性

1. **情感提取**：提取"轻声！"和"为什么你偏偏是罗密欧呢？"，标注情感类型和归属人物

1. **关系提取**：提取"朱丽叶就是太阳"这个比喻，标注类型和涉及的人物

注意每个 `extraction_text` 都是原文的精确子串，这是 LangExtract 的核心要求。

准备工作完成，接下来创建模型。

### 5.5 创建 LangExtract 模型

我们使用 `OpenAILanguageModel` 来对接 DeepSeek API。LangExtract 的设计让我们可以轻松地使用任何 OpenAI 兼容的 API。

```python
# 创建 DeepSeek 模型实例
model = OpenAILanguageModel(
    model_id=DEFAULT_MODEL,
    api_key=DEEPSEEK_API_KEY,
    base_url=DEEPSEEK_BASE_URL
)

print("模型创建成功！")
print(f"  模型 ID: {DEFAULT_MODEL}")
print(f"  API 端点: {DEEPSEEK_BASE_URL}")
```

```
模型创建成功！
  模型 ID: deepseek-chat
  API 端点: https://api.deepseek.com
```

模型实例创建完成。这里我们传入了三个参数：

+ `model_id`：模型名称，这里是 deepseek-chat

+ `api_key`：API 密钥，从环境变量读取

+ `base_url`：API 端点，DeepSeek 的 API 端点

接下来进入最关键的部分：**运行长文本提取**。

### 5.6 运行长文本提取（核心）

这是本章的核心部分。我们将使用 `lx.extract()` 函数，配合长文本处理的三大核心参数：

| 参数名 | 值 | 作用 |
| --- | --- | --- |
| `extraction_passes` | 3 | 多轮提取，提高召回率 |
| `max_workers` | 20 | 并行处理，加快速度 |
| `max_char_buffer` | 1000 | 分块大小，保持准确性 |

这三个参数是 LangExtract 处理长文本的核心策略：分块 + 多轮 + 并行。

> **注意**：运行这个 Cell 会调用 DeepSeek API，处理 54,000 字符的文本，大约需要 1-2 分钟，请耐心等待。

```python
# 运行长文本提取
print("开始运行长文本提取...")
print(f"   输入文本: {len(input_text):,} 字符")
print(f"   提取轮数: 3 轮")
print(f"   并行数: 20 workers")
print(f"   分块大小: 1000 字符")
print("\n正在处理，请稍候...\n")

result = lx.extract(
    text_or_documents=input_text,
    prompt_description=prompt,
    examples=examples,
    model=model,
    extraction_passes=3,      # 多轮提取提高召回率
    max_workers=20,           # 并行处理加速
    max_char_buffer=1000      # 较小的上下文提高准确性
)

print("\n" + "=" * 60)
print("提取完成！")
print("=" * 60)
print(f"文本长度: {len(result.text):,} 字符")
print(f"提取实体总数: {len(result.extractions)} 个")
```
