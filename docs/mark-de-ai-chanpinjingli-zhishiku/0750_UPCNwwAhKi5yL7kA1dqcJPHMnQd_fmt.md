
# 测试和调试Skills

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/NIWadwgqnojUoPx0tFwcZvQtn7b](https://qqs7y1hozd1.feishu.cn/docx/NIWadwgqnojUoPx0tFwcZvQtn7b)

测试和调试是确保Skills质量和可靠性的关键环节。一个好的Skill不仅要在理想情况下工作，还要在各种边缘情况下保持稳定。本节将介绍系统性的测试方法和调试技巧，帮助你构建健壮的Skills。

就像建造房子需要质量检验一样，开发Skills也需要全面的测试来确保每个组件都能正常工作。

## Skills加载测试方法

![](img/6b9ef37584adc97982f21e5028c86810.png)

### 基本加载验证

首先确认Skill能够被Claude Code正确识别和加载：

#### 检查Skill发现

```
# 重新启动Claude Code或运行
claude /skills
```

你应该能在输出中看到你的Skill名称。

#### 验证文件结构

```
# 检查Skill目录结构
ls -la ~/.claude/skills/your-skill-name/

# 确认SKILL.md存在
cat ~/.claude/skills/your-skill-name/SKILL.md | head -10
```

#### 测试YAML语法

```
# 使用工具验证YAML frontmatter
python -c "
import yaml
with open('SKILL.md', 'r') as f:
    content = f.read()
frontmatter = content.split('---')[1]
data = yaml.safe_load(frontmatter)
print('YAML is valid:', data)
"
```

### 元数据验证

确保YAML frontmatter中的信息正确：

```
---
name: test-skill
description: 测试Skill功能，验证加载和执行
version: 1.0.0
---
```

验证要点：

+   `name`字段符合命名规范（小写字母、数字、连字符）

+   `description`清晰描述功能和使用时机

+   没有语法错误或特殊字符问题

## 功能测试技巧

### 单元测试基本结构

创建测试来验证Skill的核心功能：

```
# tests/test_skill.py
import pytest
from unittest.mock import Mock, patch

class TestSkill:
    def setup_method(self):
        """测试前准备"""
        self.skill_name = "test-skill"
        # 初始化测试环境

    def test_skill_metadata(self):
        """测试Skill元数据"""
        # 验证name、description等字段
        assert self.skill_name == "test-skill"

    def test_skill_execution(self):
        """测试Skill执行"""
        # 模拟执行场景
        pass

    def teardown_method(self):
        """测试后清理"""
        # 清理测试数据
        pass
```

### 参数验证测试

测试Skill如何处理不同类型的输入：

```
def test_parameter_validation():
    """测试参数验证"""
    # 测试有效参数
    valid_params = {"input": "test.txt", "format": "json"}
    assert validate_parameters(valid_params) == True

    # 测试无效参数
    invalid_params = {"input": "", "format": "invalid"}
    assert validate_parameters(invalid_params) == False

    # 测试边界情况
    edge_params = {"input": "a" * 1000, "format": "json"}  # 超长输入
    assert validate_parameters(edge_params) == False
```

### 错误处理测试

确保Skill能正确处理异常情况：

```
def test_error_handling():
    """测试错误处理"""
    # 测试文件不存在的情况
    with pytest.raises(FileNotFoundError):
        process_file("nonexistent.txt")

    # 测试网络错误
    with patch('requests.get', side_effect=ConnectionError):
        with pytest.raises(ConnectionError):
            fetch_data_from_api()

    # 测试权限错误
    with patch('os.access', return_value=False):
        with pytest.raises(PermissionError):
            write_output_file("/restricted/path/output.txt")
```

## 调试渐进式披露问题

### 第一级调试：元数据加载

当Skill没有被触发时，检查元数据：

## 调试清单

### 1. 验证YAML frontmatter
- 检查`name`字段是否符合规范
- 确认`description`包含明确的触发条件
- 验证没有YAML语法错误

### 2. 测试触发条件
尝试不同的表述方式：
- "帮我分析这个数据" (具体任务)
- "我有一个Excel文件需要处理" (文件类型)
- "创建数据可视化" (功能描述)

### 3. 检查Skill可见性

```bash
# 确认Skill在列表中
claude /skills | grep your-skill-name
```

### 第二级调试：指令加载

当Skill被触发但执行不正确时：

## 指令调试步骤

### 1. 验证`SKILL.md`内容
检查指令是否：
- 使用清晰的语言
- 提供具体的步骤
- 包含必要的上下文信息

### 2. 测试指令理解
在Claude中询问：
- "你能看到这个Skill的什么内容？"
- "这个Skill的执行步骤是什么？"

### 3. 简化测试
创建最小化版本的`SKILL.md`，只包含核心功能

### 第三级调试：资源访问

当Skill无法访问外部资源时：

## 资源调试

### 1. 验证文件路径

```bash
# 检查引用文件是否存在
ls -la references/
ls -la assets/

# 测试相对路径解析
cat references/api-docs.md
```

### 2. 检查文件权限

```bash
# 验证文件可读
ls -l references/api-docs.md

# 测试文件访问
head -5 references/api-docs.md
```

### 3. 调试引用语法

确保在`SKILL.md`中的引用格式正确：

- [`references/api-docs.md`](references/api-docs.md)
- [`assets/templates/config.json`](assets/templates/config.json)

## 性能测试和token优化

### 理解Token消耗的重要性

什么是Token？

Token是Claude处理文本的基本单位。每次与Claude对话时，系统都会计算你发送的文本和Claude回复的文本的token数量。

为什么需要监控Token消耗？

+   Claude有上下文窗口限制（通常是200K token）

+   Skill的内容会占用上下文窗口

+   过长的Skill会影响Claude处理其他任务的能力

+   监控token消耗可以帮助优化Skill性能

### 手动估算Token消耗

你不需要复杂的代码来估算token消耗，这里有一个简单的方法：

#### 基础估算规则

+   英文文本：大约每4个字符算1个token

+   中文文本：大约每1.5个字符算1个token

+   代码：每行代码大约算5-10个token

#### 实际估算步骤

第一步：测量你的`SKILL.md`文件大小

```
# 查看文件大小（字符数）
wc -m SKILL.md

# 或者查看行数和字数
wc -l -w SKILL.md
```

第二步：估算token数量

假设你的`SKILL.md`有2000个字符：

+   如果主要是英文：2000 ÷ 4 ≈ 500 token

+   如果主要是中文：2000 ÷ 1.5 ≈ 1300 token

第三步：分析各部分消耗

```
# 查看YAML frontmatter部分
head -10 SKILL.md

# 查看主要内容长度
tail -n +11 SKILL.md | wc -m
```

估算结果解读：

+   < 500 token：优秀，加载迅速

+   500-2000 token：良好，性能尚可

+   2000-5000 token：需要优化，考虑使用外部引用

+   5000 token：强烈建议优化，拆分内容

### 实际监控Token消耗

#### 在Claude Code中观察消耗

方法1：查看对话历史

在Claude Code中，每次Skill触发时你会看到类似信息：

```
Launching skill: my-skill
Base Path: /Users/username/.claude/skills/my-skill/
[Skill content loaded]
```

方法2：使用Claude询问消耗情况

你可以直接询问Claude：

```
"这个Skill加载了多少内容到上下文中？"
"帮我分析一下当前上下文的使用情况"
```

#### 使用浏览器开发者工具（Web版本）

如果你使用Claude Code Web版本：

1. 按F12打开开发者工具

1. 查看Network标签页

1. 寻找包含"messages"或"completions"的API请求

1. 查看请求大小和响应大小

### Token消耗分析工具

#### 简单估算脚本（可选）

如果你想更精确地估算，这里是一个简单的Python脚本：

```
# 创建文件：token_estimator.py
def estimate_tokens(text):
    """简单估算token数量"""
    if not text:
        return 0

    # 中文字符更消耗token
    chinese_chars = sum(1 for char in text if '\u4e00' <= char <= '\u9fff')
    english_chars = len(text) - chinese_chars

    # 估算：中文1.5个token/字符，英文0.25个token/字符
    return int(chinese_chars * 1.5 + english_chars * 0.25)

# 使用示例
with open('SKILL.md', 'r', encoding='utf-8') as f:
    content = f.read()

print(f"估算token数量: {estimate_tokens(content)}")
```

使用方法：

1. 将上述代码保存为`token_estimator.py`

1. 放在你的Skill目录中

1. 运行：`python token_estimator.py`

#### 在线Token计算器

使用在线工具快速估算：

+   [OpenAI Token Calculator](https://platform.openai.com/tokenizer)

+   Anthropic Token Counter：搜索"Anthropic token calculator"

### 优化策略

#### 减少初始加载内容

```
## 优化前（高token消耗）
---
name: comprehensive-data-analyzer
description: 这是一个功能非常全面的数据分析工具，可以处理各种数据格式，包括CSV、JSON、XML等，支持数据清洗、统计分析、可视化、机器学习等多种功能...
---

# 全面数据分析工具

## 支持的数据格式
- CSV文件
- JSON文件
- XML文件
- 数据库连接
- API数据源

## 分析功能
- 描述性统计
- 相关性分析
- 聚类分析
- 回归分析
- 时间序列分析

## 可视化功能
- 柱状图
- 折线图
- 散点图
- 热力图
- 箱线图

[详细功能列表](references/full-feature-list.md)
```

```
## 优化后（低token消耗）
---
name: data-analyzer
description: 处理数据分析任务。当用户需要统计计算、数据可视化或格式转换时使用此Skill。
---

# 数据分析助手

## 快速开始
1. 上传或指定数据文件
2. 选择分析类型（统计/可视化/转换）
3. 执行分析并获取结果

## 高级功能
复杂分析和自定义选项请参考 [`references/advanced-usage.md`](references/advanced-usage.md)

## 支持格式
- CSV、JSON、XML文件
- 数据库查询结果
- API返回数据
```

#### 使用外部引用

## 优化技巧

### 1. 核心内容保留在主文件
- 基本使用步骤
- 重要安全提醒
- 常见问题的快速解决方法

### 2. 详细信息移至外部文件
- 完整的API文档
- 详细的配置选项
- 高级用例和示例

### 3. 按需加载

```markdown
## 高级配置

需要自定义设置时，请查看 [`references/configuration.md`](references/configuration.md)

## 故障排除

遇到问题时，参考 [`references/troubleshooting.md`](references/troubleshooting.md)
```

## 跨环境兼容性测试

### 为什么需要测试兼容性？

不同的用户使用不同的操作系统和Claude Code版本，你的Skill需要能够在各种环境下正常工作。

常见的兼容性问题：

+   文件路径分隔符（Windows用`\`，其他用`/`）

+   命令行工具可用性（Linux/macOS vs Windows）

+   Python版本差异

+   网络访问限制

### 简单兼容性检查

你不需要复杂的测试环境，以下是实用的检查方法：

#### 检查操作系统差异

```
# 查看当前操作系统
uname -a

# 检查Python版本
python --version

# 检查关键工具是否可用
which python
which bash
which cat
```

#### 测试文件路径兼容性

```
# 在Skill中使用相对路径，避免绝对路径
# 错误示例：
/Users/username/.claude/skills/my-skill/data.txt

# 正确示例：
data.txt
./data.txt
references/docs.md
```

#### 检查Claude Code版本

```
# 查看Claude Code版本
claude --version

# 如果版本太旧，可能不支持某些新功能
# 建议用户升级：pip install --upgrade claude-code
```

### 处理常见兼容性问题

#### 操作系统路径问题



# 技能兼容性与调试指南

## 兼容的路径处理

```
# 兼容的路径处理
import os

# 获取当前Skill目录
skill_dir = os.path.dirname(__file__)

# 构建文件路径
data_file = os.path.join(skill_dir, 'data', 'sample.csv')

# 读取文件
with open(data_file, 'r') as f:
    content = f.read()
```

## 命令执行兼容性

```
# 使用跨平台命令
# 错误：只适用于Linux/macOS
ls -la

# 正确：Python脚本处理
python scripts/list_files.py
```

## 版本兼容性测试清单

基础功能测试：

+ Skill能否被正确加载  
+ YAML frontmatter解析正常  
+ 基本指令执行成功  
+ 文件引用路径正确  

操作系统测试：

+ Linux环境（推荐Ubuntu 18.04+）  
+ macOS环境（推荐10.15+）  
+ Windows环境（推荐使用WSL）  

Claude Code版本测试：

+ CLI版本测试  
+ Desktop版本测试（如果可用）  
+ Web版本测试（如果可用）  

## Claude Code版本兼容性

测试不同版本的Claude Code：

```
## 版本兼容性测试

### 测试矩阵
- Claude Code CLI v1.0+
- Claude Code Desktop v1.0+
- Claude Code Web版本

### 兼容性检查
1. **Skills发现**：所有版本都能发现Skills吗？
2. **指令加载**：YAML frontmatter格式是否一致？
3. **脚本执行**：Bash/Python执行环境是否相同？
4. **文件访问**：相对路径解析是否一致？

### 已知差异
- Web版本：强制沙箱环境，更严格的安全限制
- Desktop版本：本地文件系统完全访问
- CLI版本：依赖系统环境配置
```

## 调试工具和技术

### 简单日志记录

为什么需要日志？

日志就像Skill的"黑匣子"，记录了执行过程中的关键信息，帮助你了解Skill是否按预期工作。

简单日志方法：

```
# 在脚本中添加简单的日志输出
echo "开始执行Skill" >> skill_debug.log
date >> skill_debug.log

# 执行关键步骤时记录状态
echo "正在处理数据文件..." >> skill_debug.log

# 完成后记录结果
echo "Skill执行完成" >> skill_debug.log
```

查看日志：

```
# 查看最新日志
tail -20 skill_debug.log

# 搜索特定错误
grep "ERROR" skill_debug.log
```

### 逐步调试技巧

什么是逐步调试？

逐步调试就像是慢放播放，你可以一步步观察Skill的执行过程，找出问题所在。

简单调试步骤：

#### 步骤1：隔离问题

```
# 先测试Skill是否能被加载
claude /skills | grep your-skill

# 如果加载失败，检查SKILL.md格式
head -10 SKILL.md
```

#### 步骤2：测试单个组件

如果Skill加载成功，但功能不工作，尝试手动执行脚本部分：

```
# 测试文件读取
cat data/sample.txt

# 测试脚本执行
python scripts/process.py --input test.txt
```

#### 步骤3：简化测试

```
# 创建最小化测试版本
# 只保留SKILL.md的基本结构，去掉复杂引用
cp SKILL.md SKILL_simple.md
# 编辑SKILL_simple.md，只保留核心功能

# 测试简化版本是否工作
```

#### 步骤4：记录问题模式

```
# 当问题发生时，记录上下文
echo "问题时间: $(date)" >> error_log.txt
echo "用户输入: $user_input" >> error_log.txt
echo "错误信息: $error_message" >> error_log.txt
echo "系统状态: $(uname -a)" >> error_log.txt
```

### 性能检查工具

为什么需要性能检查？

Skill执行太慢或消耗太多资源会影响用户体验，需要定期检查性能。

简单性能监控：

#### 方法1：时间测量

```
# 记录开始时间
start_time=$(date +%s)

# 执行Skill
claude --skill your-skill "测试输入"

# 记录结束时间
end_time=$(date +%s)

# 计算耗时
execution_time=$((end_time - start_time))
echo "执行耗时: ${execution_time}秒"
```

#### 方法2：资源使用检查

```
# 检查内存使用（macOS/Linux）
ps aux | grep claude

# 检查磁盘空间
df -h

# 检查网络连接（如果Skill需要网络）
ping -c 3 google.com
```

#### 方法3：简化性能测试

```
# 测试不同大小的输入
echo "小文件测试:" > perf_test.log
time claude --skill your-skill "小输入测试" >> perf_test.log

echo "大文件测试:" >> perf_test.log
time claude --skill your-skill "大输入测试" >> perf_test.log

# 查看结果
cat perf_test.log
```

### 常用调试清单

#### Skill加载问题

+ `SKILL.md`文件存在且可读  
+ `YAML frontmatter`格式正确  
+ `name`字段符合规范  
+ `description`包含触发条件  
+ 文件权限正确  

#### 功能执行问题

+ 脚本文件存在且可执行  
+ 依赖项已安装  
+ 文件路径正确  
+ 输入数据格式正确  
+ 输出位置可写  

#### 性能问题

+ Skill文件大小合理  
+ 没有不必要的引用  
+ 脚本执行时间正常  
+ 内存使用合理  

### 寻求帮助

当自己无法解决时：

1. 查看官方文档

+ Claude Code官方文档  
+ Skills相关FAQ  

1. 社区支持

+ 在Claude对话中询问："这个Skill为什么不工作？"  
+ 描述具体问题和错误信息  

1. 简化重现

```
# 创建最小重现案例
mkdir debug_case
cp SKILL.md debug_case/
# 只保留导致问题的部分
```

记录调试过程

```
# 保存调试信息
echo "调试时间: $(date)" > debug_report.txt
echo "问题描述: $issue_description" >> debug_report.txt
echo "尝试的解决方案:" >> debug_report.txt
# 列出你尝试过的方法
```

## 最佳实践总结

1. 分层测试：从单元测试到集成测试，再到端到端测试  

1. 持续验证：每次修改后重新运行测试  

1. 环境一致性：在目标环境中进行最终测试  

1. 错误模拟：主动测试错误情况和边界条件  

1. 性能监控：跟踪token消耗和执行时间  

1. 文档记录：记录测试结果和已知问题  

通过系统性的测试和调试，你可以确保Skills在各种情况下都能稳定可靠地工作。记住，测试不是一次性活动，而是一个持续的过程，需要随着Skill的发展而不断完善。
