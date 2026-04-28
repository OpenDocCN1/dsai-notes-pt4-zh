# 添加脚本和可执行代码

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/R5rKdvjbBonAQSxadE7cTYjRnhb](https://qqs7y1hozd1.feishu.cn/docx/R5rKdvjbBonAQSxadE7cTYjRnhb)

当SKILL.md中的指令描述不足以完成复杂任务时，你可以通过添加可执行脚本和代码来增强Skill的功能。这些脚本可以处理计算密集型任务、外部API调用、文件处理等操作，让Skill变得更加智能和实用。

脚本是Skill的"执行引擎"，让Claude能够从"指导者"转变为"实干家"。

## 何时需要添加脚本

### 计算密集型任务

当任务需要复杂的数学计算、数据处理或算法执行时：

适用场景：

+   大数据分析和统计计算

+   图像处理和格式转换

+   加密解密操作

+   机器学习模型推理

示例：

```
# 在SKILL.md中引用脚本
## 数据分析
对于复杂的数据分析，使用内置脚本：
```bash
python scripts/analyze_data.py --input data.csv --output report.json
```
```

### 外部系统集成

当需要与外部API、数据库或服务交互时：

适用场景：

+   调用REST API获取数据

+   连接数据库执行查询

+   与云服务集成（如AWS、Google Cloud）

+   调用命令行工具

示例：

```
## API集成
调用外部服务获取最新数据：
```bash
python scripts/fetch_api_data.py --endpoint https://api.example.com/data
```
```

### 文件和数据处理

当需要复杂的文件操作或数据转换时：

适用场景：

+   批量文件处理

+   数据格式转换（JSON↔XML↔CSV）

+   压缩解压操作

+   文件校验和验证

### 自动化工作流

当任务涉及多个步骤的自动化执行时：

适用场景：

+   CI/CD流水线

+   批量数据迁移

+   系统部署和配置

+   定期备份任务

## 脚本存放规范

### 目录结构

遵循标准化的目录结构，便于管理和维护：

```
my-skill/
├── SKILL.md
├── scripts/
│   ├── analyze_data.py      # 数据分析脚本
│   ├── fetch_api.py         # API调用脚本
│   ├── process_files.py     # 文件处理脚本
│   └── utils/
│       ├── helpers.py       # 辅助函数
│       └── validators.py    # 验证函数
├── assets/
│   └── templates/
│       └── report_template.json
└── tests/
    ├── test_scripts.py
    └── fixtures/
        └── sample_data.json
```

### 命名规范

使用清晰、一致的命名约定：

+   文件名：使用小写字母和下划线，如 data_processor.py

+   函数名：使用小写字母和下划线，如 process_data()

+   类名：使用大驼峰命名，如 DataProcessor

+   常量：使用大写字母和下划线，如 MAX_RETRY_COUNT

### 文件权限设置

确保脚本具有正确的执行权限：

```
# 设置Python脚本为可执行
chmod +x scripts/data_processor.py

# 设置Shell脚本为可执行
chmod +x scripts/deploy.sh
```

## Claude调用脚本的机制

### 指令中的脚本引用

在SKILL.md中明确告诉Claude如何调用脚本：

```
## 数据处理步骤

1. **验证输入数据**
   ```bash
   python scripts/validate_input.py --file input.csv
   ```

2. **执行数据转换**
   ```bash
   python scripts/transform_data.py --input input.csv --output output.json
   ```

3. **生成结果报告**
   ```bash
   python scripts/generate_report.py --data output.json --template report_template.md
   ```
```

### 参数传递机制

Claude通过命令行参数向脚本传递信息：

```
## 用户自定义处理

当用户指定特定参数时：
```bash
python scripts/custom_process.py --user_input &quot;{{user_input}}&quot; --options &quot;{{options}}&quot;
```
```

Claude会自动替换 {{user_input}} 和 {{options}} 为实际值。

### 错误处理和重试

指导Claude如何处理脚本执行失败：

```
## 错误处理

如果脚本执行失败：
1. 检查错误信息：`echo &quot;Error details: {{error_message}}&quot;`
2. 重试操作：`python scripts/retry_operation.py --attempt 2`
3. 回退方案：使用备用脚本 `python scripts/fallback.py`
```

## 脚本开发最佳实践

### 输入验证

始终验证输入参数的有效性：

```
# scripts/data_processor.py
import sys
import argparse

def validate_input(file_path, output_format):
    &quot;&quot;&quot;验证输入参数&quot;&quot;&quot;
    if not file_path or not os.path.exists(file_path):
        print(f&quot;Error: Input file &#x27;{file_path}&#x27; not found&quot;)
        sys.exit(1)

    valid_formats = [&#x27;json&#x27;, &#x27;xml&#x27;, &#x27;csv&#x27;]
    if output_format not in valid_formats:
        print(f&quot;Error: Invalid output format &#x27;{output_format}&#x27;. Valid options: {valid_formats}&quot;)
        sys.exit(1)

def main():
    parser = argparse.ArgumentParser(description=&#x27;Process data files&#x27;)
    parser.add_argument(&#x27;--input&#x27;, required=True, help=&#x27;Input file path&#x27;)
    parser.add_argument(&#x27;--output&#x27;, required=True, help=&#x27;Output file path&#x27;)
    parser.add_argument(&#x27;--format&#x27;, default=&#x27;json&#x27;, choices=[&#x27;json&#x27;, &#x27;xml&#x27;, &#x27;csv&#x27;], help=&#x27;Output format&#x27;)

    args = parser.parse_args()

    # 验证输入
    validate_input(args.input, args.format)

    # 处理数据
    process_data(args.input, args.output, args.format)
    print(f&quot;Processing complete. Output saved to {args.output}&quot;)

if __name__ == &#x27;__main__&#x27;:
    main()
```

### 输出格式标准化

使用一致的输出格式，便于Claude解析：

```
# scripts/api_fetcher.py
import json
import sys

def fetch_data(api_url, timeout=30):
    &quot;&quot;&quot;获取API数据&quot;&quot;&quot;
    try:
        # API调用逻辑
        result = {&quot;success&quot;: True, &quot;data&quot;: fetched_data, &quot;timestamp&quot;: get_timestamp()}
        print(json.dumps(result, indent=2))
    except Exception as e:
        error_result = {&quot;success&quot;: False, &quot;error&quot;: str(e), &quot;timestamp&quot;: get_timestamp()}
        print(json.dumps(error_result, indent=2))
        sys.exit(1)

def main():
    # 参数解析和调用逻辑
    pass

if __name__ == &#x27;__main__&#x27;:
    main()
```

### 日志记录

添加适当的日志输出，帮助调试：

```
# scripts/file_processor.py
import logging
import sys

# 配置日志
logging.basicConfig(
    level=logging.INFO,
    format=&#x27;%(asctime)s - %(levelname)s - %(message)s&#x27;
)

def process_files(file_list):
    &quot;&quot;&quot;处理文件列表&quot;&quot;&quot;
    logger = logging.getLogger(__name__)

    for file_path in file_list:
        try:
            logger.info(f&quot;Processing file: {file_path}&quot;)
            # 处理逻辑
            logger.info(f&quot;Successfully processed: {file_path}&quot;)
        except Exception as e:
            logger.error(f&quot;Failed to process {file_path}: {e}&quot;)
            # 决定是否继续处理其他文件

def main():
    # 主逻辑
    pass

if __name__ == &#x27;__main__&#x27;:
    main()
```

## 参数传递和返回值处理

### 参数传递方式

Claude可以通过多种方式向脚本传递参数：

#### 命令行参数

```
## 数据分析
```bash
python scripts/analyze.py --file &quot;{{file_path}}&quot; --method &quot;{{analysis_method}}&quot; --output &quot;{{output_path}}&quot;
```
```

#### 环境变量

```
## 配置加载
```bash
export API_KEY=&quot;{{api_key}}&quot;
export DEBUG=&quot;{{debug_mode}}&quot;
python scripts/process_data.py
```
```

#### 标准输入

```
## 批量处理
```bash
echo &quot;{{data_list}}&quot; | python scripts/batch_processor.py
```
```

### 返回值处理

脚本应该返回结构化的结果，便于Claude理解：

```
# scripts/result_formatter.py
import json
import sys

def format_result(success, data=None, error=None, metadata=None):
    &quot;&quot;&quot;格式化返回值&quot;&quot;&quot;
    result = {
        &quot;success&quot;: success,
        &quot;timestamp&quot;: get_current_timestamp(),
        &quot;metadata&quot;: metadata or {}
    }

    if success:
        result[&quot;data&quot;] = data
    else:
        result[&quot;error&quot;] = error

    return result

def main():
    try:
        # 处理逻辑
        processed_data = perform_processing()
        result = format_result(True, data=processed_data, metadata={&quot;records_processed&quot;: len(processed_data)})
    except Exception as e:
        result = format_result(False, error=str(e))

    # 输出JSON结果
    print(json.dumps(result, indent=2))

if __name__ == &#x27;__main__&#x27;:
    main()
```

## 调试脚本执行问题

### 常见问题诊断

#### 脚本找不到

```
## 诊断步骤
1. 确认脚本路径：`ls -la scripts/my_script.py`
2. 检查文件权限：`ls -l scripts/my_script.py`
3. 验证Python路径：`which python`
4. 测试脚本执行：`python scripts/my_script.py --help`
```

#### 参数传递错误

```
## 参数验证
检查传递的参数：
```bash
echo &quot;Received parameters: {{all_parameters}}&quot;
python scripts/validate_params.py --params &quot;{{all_parameters}}&quot;
```
```

#### 依赖缺失

```
## 依赖检查
```bash
python -c &quot;import required_module; print(&#x27;Module available&#x27;)&quot;
pip install missing-dependency
```
```

### 调试技巧

#### 添加调试输出

```
## 调试模式
启用详细输出：
```bash
export DEBUG=1
python scripts/debug_script.py --input &quot;{{input_data}}&quot;
```
```

#### 逐步执行

```
## 分步调试
1. 验证输入：`python scripts/validate.py --input &quot;{{input}}&quot;`
2. 执行处理：`python scripts/process.py --validated-input validated.json`
3. 生成输出：`python scripts/output.py --processed-data processed.json`
```

#### 日志分析

```
## 日志检查
查看执行日志：
```bash
tail -f /tmp/skill_execution.log
python scripts/analyze_logs.py --log-file /tmp/skill_execution.log
```
```

### 错误恢复机制

```
## 错误处理流程

如果脚本执行失败：

### 临时文件清理
```bash
rm -f /tmp/temp_*.tmp
```

### 状态重置
```bash
python scripts/reset_state.py --clean-workspace
```

### 备用方案
```bash
python scripts/fallback_processor.py --input &quot;{{original_input}}&quot;
```

### 用户通知
```
执行过程中遇到错误：{{error_message}}
已尝试自动恢复，请检查结果或重试操作。
```
```

## 实际案例：数据分析Skill

### SKILL.md配置

```
---
name: data-analyzer
description: 执行复杂数据分析任务，包括统计计算、可视化和报告生成
---

# 数据分析助手

## 基本分析
```bash
python scripts/basic_stats.py --data &quot;{{data_file}}&quot; --output &quot;{{stats_file}}&quot;
```

## 高级分析
```bash
python scripts/advanced_analysis.py --data &quot;{{data_file}}&quot; --method &quot;{{analysis_method}}&quot; --visualize
```

## 报告生成
```bash
python scripts/generate_report.py --stats &quot;{{stats_file}}&quot; --template report_template.md --output &quot;{{report_file}}&quot;
```

## 故障排除
- 如果分析失败，检查数据格式：`python scripts/validate_data.py --file &quot;{{data_file}}&quot;`
- 内存不足时，使用流式处理：`python scripts/stream_processor.py --input &quot;{{data_file}}&quot;`
```

### 脚本示例

```
# scripts/basic_stats.py
import pandas as pd
import json
import sys

def calculate_basic_stats(data_file):
    &quot;&quot;&quot;计算基本统计信息&quot;&quot;&quot;
    try:
        df = pd.read_csv(data_file)

        stats = {
            &quot;row_count&quot;: len(df),
            &quot;column_count&quot;: len(df.columns),
            &quot;columns&quot;: list(df.columns),
            &quot;numeric_stats&quot;: {}
        }

        # 数值列统计
        numeric_columns = df.select_dtypes(include=[&#x27;number&#x27;]).columns
        for col in numeric_columns:
            stats[&quot;numeric_stats&quot;][col] = {
                &quot;mean&quot;: float(df[col].mean()),
                &quot;median&quot;: float(df[col].median()),
                &quot;std&quot;: float(df[col].std()),
                &quot;min&quot;: float(df[col].min()),
                &quot;max&quot;: float(df[col].max())
            }

        return stats
    except Exception as e:
        print(f&quot;Error calculating stats: {e}&quot;, file=sys.stderr)
        sys.exit(1)

def main():
    import argparse

    parser = argparse.ArgumentParser(description=&#x27;Calculate basic statistics&#x27;)
    parser.add_argument(&#x27;--data&#x27;, required=True, help=&#x27;Input data file&#x27;)
    parser.add_argument(&#x27;--output&#x27;, required=True, help=&#x27;Output stats file&#x27;)

    args = parser.parse_args()

    stats = calculate_basic_stats(args.data)

    with open(args.output, &#x27;w&#x27;) as f:
        json.dump(stats, f, indent=2)

    print(f&quot;Statistics saved to {args.output}&quot;)

if __name__ == &#x27;__main__&#x27;:
    main()
```

通过精心设计的脚本，你可以让Skill处理几乎任何类型的任务。记住，脚本应该是可靠的、文档化的，并且能够优雅地处理错误情况。这样，Claude就可以专注于决策和指导，而将具体的执行工作交给脚本处理。