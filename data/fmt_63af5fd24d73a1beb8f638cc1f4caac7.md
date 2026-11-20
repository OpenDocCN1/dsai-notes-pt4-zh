
+   Dify 社区：关注官方论坛、Discord/Slack 频道（如有），与其他用户交流经验、寻求帮助。

## 结语

Dify 为企业拥抱生成式 AI、实现智能化转型提供了一个强大而易用的平台。通过本课程的学习，您已经掌握了使用 Dify 构建从简单对话到复杂自动化流程的各种 AI 应用的核心技能。希望您能将所学知识应用到实际工作中，创造出更多有价值的解决方案。

# 附加实验

## 构建 Workflow - 天气预报查询与格式化 (Practical Case: Weather Forecast Workflow)

这个实践案例将演示如何构建一个相对复杂的 Workflow，它不仅利用知识库和 LLM，还集成了外部工具（高德天气）和代码执行节点，来完成一个从输入地区名到输出格式化天气预报的完整流程。

### 场景定义：智能天气查询与播报

+   **目标:** 用户输入一个地区名称，工作流首先利用知识库和 LLM 将其转换为高德地图 API 能识别的 `adcode` (行政区划代码)，然后调用高德天气工具查询该地区的天气预报（通常包含未来几天的数据），最后通过代码执行节点解析返回的 JSON 数据，提取并格式化输出今天的天气信息。

+   **输入:** `input` (字符串，用户输入的地区名称，如“北京”、“上海市浦东新区”)

+   **输出:** `output` (字符串，格式化后的今日天气预报，例如：“今日天气，白天晴，温度25摄氏度，晚上多云，气温18摄氏度”)

+   **依赖:**

+   一个或多个包含地区名称与 `adcode` 对应关系的 Dify 知识库。（`https://a.amap.com/lbs/static/code_resource/AMap_adcode_citycode.zip`）

+   配置好的 `LLM` (例如 deepseek-chat)。

+   Dify 内置的高德天气工具 (`gaode_weather`) (需要确保在 Dify 市场中已安装并可能需要配置高德 API Key)。

+   Python 3 代码执行环境。

### Workflow 结构解析

根据 DSL 文件，这个“天气预报”工作流的步骤如下：

1.  开始 (`Start Node`): 接收用户输入的地区名称 (`input`)。

1.  知识检索 (`Knowledge Retrieval Node`): 使用用户输入的地区名称 (`input`) 在包含 `adcode` 信息的知识库中进行检索，并启用了 Reranker (`BAAI/bge-reranker-v2-m3`)，Top K=4，旨在精确找到与输入地区最相关的包含 `adcode` 的文本块。

1.  大语言模型 (`LLM Node`): 接收用户输入的地区名称 (`input`) 和知识检索结果 (`context`)。其核心任务是根据这两者，准确地提取或推断出该地区对应的 `adcode`。Prompt 指示它只需要输出 `adcode` 值，避免其他无关内容。使用的是 `deepseek-chat` 模型。

1.  工具 (`Tool Node` - 高德天气): 接收上一步 LLM 输出的 `adcode` 作为 `city` 参数，调用内置的 `gaode_weather` 工具来查询天气信息。

1.  代码执行 (`Code Node`): 接收高德天气工具返回的（通常是包含多天预报的）JSON 格式的文本结果 (`text`) 作为输入参数 `arg1`。执行一段 Python 代码，该代码负责：

+   解析输入的 JSON 字符串。

+   找到今天的天气数据。

+   提取白天天气、白天温度、夜晚天气、夜晚温度等关键信息。

+   将这些信息格式化成一句自然语言的中文天气预报。

+   处理可能出现的 JSON 解析错误、数据缺失或格式转换错误。

1.  结束 (`End Node`): 将代码执行节点输出的格式化后的今日天气预报字符串 (`result`) 作为整个工作流的最终结果 (`output`) 返回。

**整体流程概览:** 用户输入地名 -> RAG+LLM 获取 `adcode` -> 调用天气 API 获取预报数据 (JSON) -> Python 代码解析 JSON 并格式化输出今日天气 -> 返回结果。这个流程巧妙地结合了多种技术来解决一个实际问题。

### 在 Dify 中构建 Workflow

在 Dify 的可视化界面中搭建这个“天气预报”工作流：

1.  **创建新 Workflow:** 在工作室创建“工作流 (Workflow)”应用，命名为 天气预报，选择图标（如 🌤️）。

1.  **配置开始节点:**

+   点击“开始”节点。

+   添加输入变量：`input`，类型 `text-input`，标签“输入地区名”，设为必需。

![](img/c692e4f804ef91bf48ebcda81e9b35e1.png)

1.  **添加并配置知识检索节点:**

+   拖入“知识检索”节点，连接“开始”节点。

+   **选择知识库:** 关联包含地区名和 `adcode` 信息的知识库。

+   **查询变量:** 选择 `input`。

![](img/a6b3eba77ec05fb11fefc6c4ca6be2f8.png)

1.  添加并配置 LLM 节点 (获取 Adcode):

+   拖入“LLM”节点，连接“知识检索”节点。

+   **选择模型:** `deepseek-chat`。

+   **配置上下文:** 启用，变量选择知识检索节点的 `result`。

+   **编写 Prompt:**

```
根据用户输入 `{{#start.input#}}` 和知识检索的结果 `{{#knowledge_retrieval.result#}}`，输出用户期望得到的 `adcode`。只需要给出准确的 `adcode` 值，禁止出现 “think” 相关内容。
```

![](img/12fefc1e344b9c1dae89eafdcb762cfd.png)

1.  添加并配置工具节点 (高德天气):

+   从节点库（可能在“内置工具”下）拖入“天气预报 (`gaode_weather`)”工具节点，连接“LLM”节点。

+   点击工具节点进行配置：

+   **工具名称/提供商:** 确认是 `gaode_weather` / `gaode`。

+   **配置参数:** 找到 `city` 参数的输入设置。

+   **映射变量:** 将输入类型设为“变量 (`Variable`)”，并选择来自上游 LLM 节点的输出 `text` (即 `adcode`) 作为 `city` 参数的值。

+   (可能需要配置高德 API Key，这通常在 Dify 的全局设置或该工具的配置中完成)

![](img/233171f57018e1029922bd98dc530e08.png)

1.  **添加并配置代码执行节点:**

+   从节点库拖入“代码执行 (`Code`)”节点，连接“天气预报”工具节点。

+   点击代码节点进行配置：

+   **标题:** 可以命名为“格式化天气结果”。

+   **代码语言:** 选择 Python 3。

+   **配置输入变量:** 添加一个输入变量 `arg1`。将其值来源设置为上游“天气预报”工具节点的输出 `text` (即天气 API 返回的 JSON 字符串)。

+   **编写代码:** 将 DSL 文件中提供的 Python 代码完整地粘贴到代码编辑器中。这段代码负责解析 JSON、提取今日天气并格式化输出。

```
# -*- coding: utf-8 -*-
import json
import datetime
import re

def main(arg1: str) -> dict:
    """
    Extracts a JSON array from the beginning of arg1, finds today's weather,
    formats it into a human-readable string, and returns it.
    Handles potential extra text after the JSON array.
    """
    match = re.search(r'^(\[.*\])', arg1.strip())
    if not match:
        return {"result": "Error: Could not find JSON array at the beginning of the input."}

    json_string_to_parse = match.group(1)

    try:
        report = json.loads(json_string_to_parse)
        today_date = datetime.date.today().strftime("%Y-%m-%d")

        if not isinstance(report, list):
             return {"result": "Error: Parsed JSON is not a list."}

        for weather in report:
            if isinstance(weather, dict):
                report_date = weather.get("date")
                if today_date == report_date:
                    # Found today's weather, now format the output string
                    day_weather = weather.get("dayweather", "未知") # Default value if key missing
                    day_temp_str = weather.get("daytemp_float", "0") # Default to "0" string
                    night_weather = weather.get("nightweather", "未知")
                    night_temp_str = weather.get("nighttemp_float", "0")

                    # Safely convert temps to float and round
                    try:
                        day_temp_i = round(float(day_temp_str))
                    except ValueError:
                        day_temp_i = 0 # Default to 0 if conversion fails
                    try:
                        night_temp_i = round(float(night_temp_str))
                    except ValueError:
                         night_temp_i = 0

                    # Construct the result string using f-string (more modern)
                    res = f"今日天气，白天{day_weather}，温度{day_temp_i}摄氏度，晚上{night_weather}，气温{night_temp_i}摄氏度"
                    # Or using the % formatting like in your example:
                    # res = "今日天气，白天%s，温度%d摄氏度，晚上%s，气温%d摄氏度" % (day_weather, day_temp_i, night_weather, night_temp_i)

                    return {"result": res} # Return the formatted string

        # If loop completes without finding today's date
        return {"result": f"No weather information found for today ({today_date}) in the JSON data."}

    except json.JSONDecodeError:
        return {"result": "Error: Invalid JSON structure in the extracted part."}
    except ValueError:
        # Catch potential float conversion errors not caught inside the loop (less likely now)
         return {"result": "Error: Could not convert temperature value to number."}
    except Exception as e:
        return {"result": f"Error: {str(e)}"}
```

+   **定义输出变量:** 代码的 `return {"result": res}` 语句定义了输出。Dify 通常会自动识别或允许您定义输出变量，确保有一个名为 `result` 的字符串类型输出变量。

![](img/2ba32ba47bdc64d07f42c23d8bfa9f6b.png)

1.  **配置结束节点:**

+   将“代码执行”节点的右侧输出锚点连接到“结束”节点的输入锚点。

+   点击“结束”节点进行配置。

+   **添加输出变量:** 定义一个输出变量，例如 `output`。将其值来源设置为上游“代码执行”节点的输出 `result`。

![](img/dbf360d6fd7586917b39bc6cef3fa8be.png)

1.  **保存 Workflow。**

![](img/500f28e325ef87c74e146b0b1a784a27.png)

### 运行与测试 Workflow

1.  点击“运行”或“调试”。

1.  在输入框中输入一个地区名称，例如“闵行区”、“广州”、“杭州西湖区”等（确保您的 `adcode` 知识库包含这些信息）。

![](img/9c6863a6bdc46456f9500017540dd21f.png)

1.  点击执行。

1.  **分步检查节点输出:**

+   **知识检索:** 输出是否包含了与输入地区相关的、可能含有 `adcode` 的文本块？

![](img/b4e51905a9a2a17d183378319b6d2801.png)

+   LLM (获取 `Adcode`): 输出是否仅仅是一个 6 位的数字 `adcode`？这是关键一步。

![](img/ca3bdba52f7cf3f12c9e9a89cd4dd0e6.png)

+   **天气预报工具:** 输出是否是一段看起来像 JSON 格式的天气预报数据？

![](img/12816c76f177398e8384129762a63ec7.png)

+   **代码执行:** 输出 (`result`) 是否是一句格式正确的中文今日天气预报？

![](img/7edfed0b3b4845fdbbb13196355ee34d.png)

+   **结束:** 最终输出是否与代码执行节点的 `result` 一致？

![](img/c1d4565f192a87d1fc415356d12d5adf.png)

1.  **测试不同地区:** 尝试输入不同的省、市、区县名称，测试流程的鲁棒性。

1.  **测试边界情况:** 输入知识库可能没有的地区，观察 LLM 节点能否正确处理（例如返回空值或错误提示），以及后续节点如何响应（代码节点应该能处理无效输入并返回错误信息）。

这个“天气预报” Workflow 案例充分展示了如何通过组合 Dify 的不同节点类型（RAG、LLM、工具、代码）来构建一个端到端的、解决特定问题的自动化流程。

## 发布 MCP 服务

将天气预报发布为工具

![](img/8f5cd564c85d168dacc43d9c6033a330.png)

按照下图所示，输入一个工具调用名称，并且描述写好，最好详细一点，能让大模型理解这个工具到底是干啥的。

![](img/1044d56d0019ea4362e0e2ff21a5df13.png)

从应用商店，安装MCP Compatible Dify Tools
