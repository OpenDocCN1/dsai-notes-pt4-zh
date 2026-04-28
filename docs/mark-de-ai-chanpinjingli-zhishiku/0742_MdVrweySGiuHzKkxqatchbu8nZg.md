# Skills配置选项

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/MpUYdTty3oXebdxpp9Tc4Xd0nCc](https://qqs7y1hozd1.feishu.cn/docx/MpUYdTty3oXebdxpp9Tc4Xd0nCc)

## SKILL.md 元数据配置

Skills 的配置主要通过 SKILL.md 文件的 YAML 前置元数据来实现。本节详细介绍各种配置选项及其使用方法。

## 核心元数据字段

### 必需字段

#### name

```
name: my-skill
```

+   类型: 字符串

+   长度: 1-64字符

+   格式: 小写字母、数字和连字符

+   约束: 不以连字符开头/结尾，无连续连字符

+   作用: 唯一标识符，用于引用和组织

#### description

```
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files, forms, or document extraction.
```

+   类型: 字符串

+   长度: 1-1024字符

+   作用: 描述功能和使用时机，帮助 AI 发现

+   最佳实践: 包含关键词，说明适用场景

### 可选字段

#### license

```
license: Apache-2.0
```

+   作用: 指定许可证信息

+   常见值: Apache-2.0, MIT, BSD, Proprietary

+   建议: 使用标准许可证名称或许可证文件路径

#### compatibility

```
compatibility: Designed for Claude Code (or similar products)
```

+   作用: 说明环境要求和兼容性

+   内容: 产品名称、系统要求、网络访问等

+   长度: 最多500字符

#### metadata

```
metadata:
  author: example-org
  version: &quot;1.0&quot;
  tags: [&quot;pdf&quot;, &quot;text-extraction&quot;, &quot;forms&quot;]
  homepage: &quot;https://github.com/example/my-skill&quot;
```

+   作用: 存储额外元数据

+   结构: 键值对，支持嵌套

+   用途: 作者信息、版本、标签等

#### allowed-tools

```
allowed-tools: Read, Grep, Bash(python:*)
```

+   作用: 限制 Skills 可用的工具

+   格式: 逗号分隔的工具名称

+   实验性: 目前支持有限，可能因实现而异

## 配置示例

### 基础配置

```
---
name: pdf-processing
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files, forms, or document extraction.
---
```

### 完整配置

```
---
name: advanced-pdf-processor
description: Comprehensive PDF processing toolkit. Extract text, fill forms, merge documents, convert formats. Use for any PDF-related tasks including document analysis, form automation, and content extraction.
license: MIT
compatibility: Requires Python 3.8+, works with Claude Code and compatible agents
metadata:
  author: pdf-tools-team
  version: &quot;2.1.0&quot;
  tags: [&quot;pdf&quot;, &quot;text-extraction&quot;, &quot;forms&quot;, &quot;conversion&quot;]
  homepage: &quot;https://github.com/pdf-tools/advanced-pdf-processor&quot;
  repository: &quot;https://github.com/pdf-tools/advanced-pdf-processor&quot;
allowed-tools: Read, Grep, Bash(python:*), Bash(pdftotext:*)
---
```

## 配置验证

### 自动验证

Skills 在加载时会自动验证配置的正确性：

#### 格式验证

+   YAML 语法正确性

+   必需字段存在性

+   字段类型正确性

#### 约束验证

+   name 格式检查

+   description 长度检查

+   compatibility 长度检查

#### 错误处理

系统会对配置进行严格的质量检查，发现问题时会详细报告：

1.  必需字段检查：

+   验证 name 字段是否存在且格式正确

+   验证 description 字段是否存在且长度合适

1.  格式验证：

+   名称只能包含小写字母、数字和连字符

+   描述长度不能超过1024个字符

+   兼容性描述长度不能超过500个字符

1.  错误收集：将所有发现的问题收集起来，一次性反馈给用户

### 验证工具

#### skills-ref 工具介绍

skills-ref 是 Agent Skills 规范的官方参考实现工具，由 Anthropic 提供，用于验证 Skills 的格式和配置正确性。

#### 获取和安装 skills-ref

从 Agent Skills 规范的官方 GitHub 仓库获取：

```
# 克隆仓库
git clone https://github.com/agentskills/agentskills.git
cd agentskills/skills-ref

# 创建虚拟环境并安装
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# 或 .venv\Scripts\activate  # Windows

# 安装工具
pip install -e .
```

安装完成后，skills-ref 命令将在虚拟环境中可用。

#### 使用 skills-ref 验证配置

```
# 验证单个 Skill
skills-ref validate my-skill/

# 批量验证目录中的所有 Skills
skills-ref validate skills-dir/

# 生成详细报告（显示所有验证信息）
skills-ref validate my-skill/ --verbose

# 验证失败时查看错误详情
skills-ref validate my-skill/ 2&gt;&amp;1
```

#### skills-ref 的其他功能

```
# 读取 Skill 属性（输出 JSON 格式）
skills-ref read-properties my-skill/

# 生成可用 Skills 的提示 XML
skills-ref to-prompt skill-a/ skill-b/
```

## 配置最佳实践

### 描述编写

```
# 好的描述示例
description: Analyze Excel spreadsheets, create pivot tables, and generate charts. Use when working with Excel files, spreadsheets, or .xlsx files.

# 不好的描述示例
description: Helps with data
```

要点:

+   明确说明功能

+   包含使用关键词

+   说明适用场景

+   保持简洁准确

### 命名规范

```
# 推荐命名
name: pdf-text-extractor
name: code-review-assistant
name: api-documentation-generator

# 避免的命名
name: PDF-Extractor  # 大写字母
name: pdf_extractor  # 下划线
name: pdf--extractor # 连续连字符
```

### 兼容性声明

```
# 具体声明
compatibility: Designed for Claude Code (or similar products)

# 条件声明
compatibility: Requires internet access for API calls, works offline for local processing

# 环境要求
compatibility: Requires Python 3.8+, Node.js 16+, or compatible runtime
```

### 元数据丰富

```
metadata:
  # 基本信息
  author: &quot;your-org&quot;
  version: &quot;1.0.0&quot;
  created: &quot;2024-01-01&quot;

  # 分类和标签
  category: &quot;data-processing&quot;
  tags: [&quot;pdf&quot;, &quot;text&quot;, &quot;extraction&quot;, &quot;automation&quot;]

  # 联系方式
  homepage: &quot;https://example.com/my-skill&quot;
  repository: &quot;https://github.com/example/my-skill&quot;
  issues: &quot;https://github.com/example/my-skill/issues&quot;

  # 技术信息
  runtime: &quot;python&quot;
  dependencies: [&quot;PyPDF2&quot;, &quot;pandas&quot;]
```

## 工具权限配置

### allowed-tools 详解

#### 语法格式

```
allowed-tools: ToolName, ToolName(parameters), ToolName(pattern:*)
```

#### 工具类型

+   基础工具: Read, Write, Grep, Glob

+   带参数工具: Bash(command:*), RunCommand(timeout:30)

+   通配符模式: Bash(python:*), Bash(node:*)

#### 配置示例

```
# 基本文件操作
allowed-tools: Read, Write, Grep, Glob

# 脚本执行权限
allowed-tools: Bash(python:*), Bash(node:*), Bash(jq:*)

# 受限权限
allowed-tools: Read, Grep

# 完全权限（不推荐）
allowed-tools: all
```

### 安全考虑

#### 最小权限原则

+   只授予必要的工具权限

+   避免过度宽泛的权限

+   定期审查和更新权限

#### 风险评估

```
# 高风险工具
allowed-tools: Bash(rm:*), Bash(dd:*), Bash(format:*)

# 中风险工具
allowed-tools: Bash(curl:*), Bash(wget:*)

# 低风险工具
allowed-tools: Read, Grep, Glob
```

## 配置继承和覆盖

### 层级配置

虽然 Skills 主要通过 SKILL.md 配置，但也支持外部配置文件的继承：

#### 项目级配置

```
# .claude/skills.yaml
skills:
  pdf-processing:
    # 覆盖默认配置
    timeout: 60
    max_file_size: &quot;10MB&quot;
```

#### 用户级配置

```
# ~/.claude/skills.yaml
skills:
  pdf-processing:
    # 用户偏好
    output_format: &quot;markdown&quot;
    verbose: true
```

### 配置合并

配置按以下优先级合并：

1.  SKILL.md 元数据（最高优先级）

1.  用户配置

1.  项目配置

1.  全局配置

1.  默认值（最低优先级）

## 动态配置

### 运行时配置

某些配置可以在运行时动态调整：

#### 环境变量

```
# 设置超时时间
export CLAUDE_SKILL_TIMEOUT=120

# 启用调试模式
export CLAUDE_SKILL_DEBUG=true
```

#### 命令行参数

```
# 运行时指定参数
claude --skill pdf-processing --timeout 60 --verbose
```

### 条件配置

基于上下文的条件配置：

```
metadata:
  config_conditions:
    # 大文件特殊处理
    large_files:
      condition: &quot;file_size &gt; 10MB&quot;
      timeout: 300
      memory_limit: &quot;2GB&quot;

    # 网络限制环境
    offline_mode:
      condition: &quot;network_unavailable&quot;
      disable_network_features: true
```

## 配置调试和故障排除

### 常见问题

#### 配置未生效

现象: 修改配置后没有变化

原因: 缓存问题或优先级错误

解决:

```
# 清除缓存
claude --clear-cache

# 检查配置优先级
claude --show-config pdf-processing
```

#### 验证失败

现象: Skill 无法加载，提示验证错误

原因: 配置格式错误

解决:

```
# 详细验证
skills-ref validate my-skill/ --verbose

# 检查错误详情
skills-ref validate my-skill/ 2&gt;&amp;1
```

#### 权限不足

现象: 工具调用被拒绝

原因: allowed-tools 配置不正确

解决:

```
# 检查并修正权限配置
allowed-tools: Read, Write, Grep, Bash(python:*)
```

### 调试命令

```
# 显示当前配置
claude --show-config

# 显示技能特定配置
claude --show-config pdf-processing

# 验证配置语法
claude --validate-config my-skill/

# 调试模式运行
claude --debug --skill pdf-processing
```

## 总结

Skills 的配置通过 SKILL.md 文件的元数据实现，提供了灵活而强大的定制能力。通过合理使用各种配置选项，可以根据具体需求优化 Skills 的行为，确保最佳的用户体验和功能效果。