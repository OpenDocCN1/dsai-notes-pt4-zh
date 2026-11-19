
# Skills 权限控制

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/RM2Odl93uoA0i2xXIebcxsKVn9e](https://qqs7y1hozd1.feishu.cn/docx/RM2Odl93uoA0i2xXIebcxsKVn9e)

## Skills 安全概述

Skills 的权限控制是为了确保安全执行而设计的，主要通过声明式权限配置来实现。本节介绍 Skills 的权限机制和安全考虑。

## allowed-tools 字段

### 基本概念

`allowed-tools` 是 Agent Skills 规范中的一个可选字段，允许 Skills 作者预先声明该 Skills 可以使用的工具。这是一个实验性功能，主要用于减少运行时的权限提示。

```
---
name: safe-file-reader
description: Read files without making changes. Use when you need read-only file access.
allowed-tools: Read, Grep, Glob
---

# Safe File Reader

This Skill provides read-only file access tools.
```

### 支持的工具类型

#### 基础工具

+ `Read`: 读取文件内容
+ `Grep`: 文件内容搜索
+ `Glob`: 文件模式匹配
+ `List`: 目录列表

#### 脚本执行工具

+ `Bash`: 命令行执行，支持参数限制
+ `Execute`: 直接执行脚本

### 配置语法

#### 简单工具列表

```
allowed-tools: Read, Grep, Glob
```

#### 带参数限制的工具

```
allowed-tools: Bash(python:*), Bash(jq:*), Execute(/safe/scripts/*)
```

注意: 这个功能是实验性的，不同的 Agent 实现可能支持程度不同。

## Claude Code 安全机制

### 权限模型

Claude Code 采用基于权限的安全模型，每个工具调用都需要用户确认：

#### 工具权限级别

+ 安全工具: 如 `Read`、`Grep`，无需特殊权限
+ 系统工具: 如 `Bash`、`RunCommand`，需要用户确认
+ 网络工具: HTTP 请求等，需要明确授权

#### 权限确认流程

1. Skills 尝试调用工具
1. Claude Code 检查权限要求
1. 提示用户确认（如果需要）
1. 用户批准后执行

### 沙箱机制（可选）

Claude Code 提供了可选的沙箱功能，可以减少权限提示：

#### 启用沙箱

```
# 在 Claude Code 中启用沙箱
/sandbox
```

#### 沙箱模式

+ 自动允许模式: 沙箱内的命令自动执行，无需确认
+ 限制访问: 文件系统和网络访问受到控制

#### 沙箱限制

+ 默认情况下不启用
+ 需要手动开启
+ 提供额外的安全层但不是强制性的

## 安全最佳实践

### 权限最小化原则

#### 明确声明需要的工具

```
# 推荐：只声明需要的工具
allowed-tools: Read, Grep, Bash(python3 -c "*")

# 避免：过度宽泛的权限
allowed-tools: Bash(*)  # 不推荐
```

#### 定期审查权限

+ 检查 Skills 是否仍然需要所有声明的工具
+ 移除不再使用的权限
+ 根据使用情况调整权限范围

### 输入验证

#### 验证输入数据

Skills 应该验证所有输入数据的安全性和格式：

+ 检查文件路径是否安全
+ 验证数据格式正确性
+ 过滤潜在的恶意内容

#### 安全的文件操作

文件操作需要多重安全检查，就像银行的保险库一样：

1. 路径规范化：清理和标准化文件路径，防止绕过检查
1. 目录限制：确保文件只能在预先允许的目录范围内
1. 权限验证：检查文件是否存在且具有读取权限
1. 安全读取：使用安全的方式打开和读取文件内容

这样可以防止恶意访问系统中的敏感文件。

### 脚本安全

#### 避免危险操作

脚本编写时要避免安全风险：

危险做法：

+ 使用 `os.system(user_input)` 可能导致命令注入攻击
+ 使用 `subprocess.call(user_input, shell=True)` 可能被恶意利用

安全做法：

+ 明确指定命令和参数：`subprocess.run(['ls', '-la', safe_path])`
+ 验证输入安全性后再执行
+ 限制可执行的命令范围

#### 脚本权限控制

+ 脚本文件应该有适当的执行权限
+ 避免在脚本中硬编码敏感信息
+ 使用参数化输入而不是字符串拼接

### 网络安全

#### 安全的网络访问

```
# 在 allowed-tools 中限制网络访问
allowed-tools: HTTP(https://trusted-api.com/*)
```

+ 只允许访问可信的域名
+ 使用 HTTPS 协议
+ 验证 SSL 证书

### 错误处理安全

#### 安全的错误信息

错误信息应该保护系统安全：

+ 避免暴露内部细节：不要显示数据库连接信息、文件路径等
+ 提供有用提示：告诉用户问题所在，但不泄露系统信息
+ 记录详细信息：敏感信息记录在日志中，仅开发者可见

不安全示例："Database connection failed: invalid password for user admin" 安全示例："数据格式无效，请检查输入内容"

## 权限配置示例

### 文件处理 Skills

```
---
name: file-processor
description: Process and analyze text files. Use for reading, searching, and basic text manipulation.
allowed-tools: Read, Grep, Glob
---
```

### 数据分析 Skills

```
---
name: data-analyzer
description: Analyze CSV and JSON data files using Python scripts. Use for data processing and basic analytics.
allowed-tools: Read, Bash(python3 -c "*"), Execute(/safe/scripts/analyze.py)
---
```

### 网络调用 Skills

```
---
name: api-client
description: Make HTTP requests to REST APIs. Use for fetching data from web services.
allowed-tools: HTTP(https://api.example.com/*), Read
---
```

## 安全检查清单

### 创建 Skills 时

+ 是否声明了所有需要的工具？
+ 权限是否遵循最小化原则？
+ 是否避免了危险的操作？
+ 错误信息是否安全？

### 部署前检查

+ 是否验证了所有脚本的安全性？
+ 是否测试了权限配置的正确性？
+ 是否检查了依赖的安全性？
+ 是否有安全审查？

### 运行时监控

+ 监控异常的工具使用模式
+ 定期审查权限日志
+ 更新过期的安全配置
+ 响应安全漏洞报告

## 常见安全问题及解决方案

### 权限被拒绝

问题: Skills 无法执行需要的操作

解决:

+ 检查 `allowed-tools` 配置是否正确
+ 确认工具名称拼写
+ 验证参数格式

### 脚本执行失败

问题: 脚本无法运行或产生错误

解决:

+ 检查脚本文件权限
+ 验证脚本语法
+ 查看错误日志

### 网络访问被阻止

问题: HTTP 请求失败

解决:

+ 检查 `allowed-tools` 中的 HTTP 配置
+ 确认 URL 格式正确
+ 验证网络连接

### 文件访问错误

问题: 无法读取或写入文件

解决:

+ 检查文件路径权限
+ 验证文件存在性
+ 确认 `Read`/`Write` 工具已声明
