
# GitHub Copilot 开发实践课程大纲

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/H3npdzgKFoqGFYxCdIBci8Asn3y](https://zsyhjtnsa5.feishu.cn/docx/H3npdzgKFoqGFYxCdIBci8Asn3y)

## 课程概述

本课程通过 16 章系统性内容全面覆盖 GitHub Copilot 的使用，从基础设置到高级开发技术，帮助开发者掌握 AI 辅助开发的完整工作流。课程内容涵盖代码分析、功能开发、测试、重构、性能优化、安全处理和规范驱动开发等多个方面，适合 C# 和 Python 开发者学习。

课程采用实践导向的教学方式，每章都包含真实的代码场景和动手实验，让学员能够在实际项目中应用所学技能。通过学习本课程，学员将能够充分利用 GitHub Copilot 提高开发效率、改善代码质量，并掌握 AI 辅助开发的最佳实践。

## 课程目标

完成本课程后，学员将能够：

- 配置和定制 GitHub Copilot 开发环境
- 使用 GitHub Copilot 进行代码分析和文档生成
- 加速新功能开发和单元测试编写
- 运用 AI 辅助进行代码重构和优化
- 掌握振动编码（Vibe Coding）和原型开发方法
- 识别并合并重复代码，提高代码可维护性
- 简化复杂条件逻辑，改善代码可读性
- 使用 GitHub Copilot 进行性能分析和优化
- 管理 GitHub 问题和处理安全扫描警报
- 实施规范驱动开发流程
- 自定义 GitHub Copilot 配置以适应个人工作流
- 使用 GitHub Copilot SDK 开发 AI 应用程序

## 课程受众

本课程适合以下人员学习：

- 软件开发人员：希望提高开发效率、改善代码质量的专业开发者
- 全栈工程师：使用 C# 或 Python 进行 Web 应用开发的工程师
- 技术负责人：希望了解 AI 辅助开发最佳实践的团队领导
- 代码质量倡导者：关注代码可维护性、可读性和性能优化的开发者
- AI 技术探索者：对 AI 辅助开发工具感兴趣的技术人员
- 学生和学习者：希望掌握现代开发工具的编程学习者

### 前置要求

- 具备基本的编程知识（C# 或 Python）
- 了解 Git 版本控制基础
- 熟悉 Visual Studio Code 或其他代码编辑器
- 拥有 GitHub 账户

## 课程内容

### 第一章 GitHub Copilot 设置与用户界面

**内容概述：** 学习如何配置 GitHub Copilot 设置，探索 Visual Studio Code 中的 GitHub Copilot 用户界面功能

**知识点：**

- GitHub Copilot 设置配置
- 聊天模式（Ask/Edit/Agent）使用
- 代码补全功能
- 智能操作（Smart Actions）
- Chat 视图、Quick Chat、Inline Chat 功能对比
- 聊天参与者与聊天变量
- 斜杠命令使用
- 代码完成建议接受与拒绝

---

### 第二章 代码分析与文档生成

**内容概述：** 使用 GitHub Copilot 分析不熟悉的代码库，生成代码解释和文档

**知识点：**

- 代码库分析方法
- 代码解释生成
- `README` 文档自动生成
- `@workspace` 聊天参与者使用
- 聊天变量（`#usages`、`#codebase`）使用
- 斜杠命令（`/explain`）使用
- 拖放文件添加上下文
- 项目结构分析

---

### 第三章 新功能开发

**内容概述：** 使用 GitHub Copilot 加速开发新代码功能，包括分支管理和拉取请求

**知识点：**

- 新功能开发流程
- Git 分支管理
- 拉取请求创建与合并
- 内联聊天代码更新
- 聊天视图编辑模式
- 代码提交消息生成
- 拉取请求摘要生成
- 代码同步与远程仓库管理

---

### 第四章 单元测试开发

**内容概述：** 使用 GitHub Copilot 加速单元测试开发，检查现有测试方法并扩展测试项目

**知识点：**

- 单元测试框架（`pytest`/`xunit`）
- 测试用例生成
- 测试方法编写
- 测试断言与 `Mock` 对象
- 代理模式创建测试类
- 编辑模式编写单元测试
- 测试驱动开发流程
- `Pytest` 配置与运行
- 参数化测试与 Fixture

---

### 第五章 代码重构与优化

**内容概述：** 使用 GitHub Copilot 重构和改进现有代码，包括使用静态字典和 LINQ 查询

**知识点：**

- 代码重构技术
- 性能优化方法
- 静态字典替代反射
- LINQ 查询优化
- 模式匹配使用
- 泛型方法重构
- 依赖注入配置
- 代码可读性改进

---

### 第六章 振动编码与原型开发

**内容概述：** 使用振动编码过程和 GitHub Copilot Agent 创建原型应用程序

**知识点：**

- 振动编码（Vibe Coding）概念
- GitHub Copilot Agent 使用
- 产品需求文档（PRD）编写
- 线框图生成
- 原型应用开发流程
- 迭代优化方法
- 自定义指令文件（`copilot-instructions.md`）
- 响应式界面设计

---

### 第七章 合并重复代码

**内容概述：** 分析复杂代码库并合并重复代码逻辑，提高代码可维护性

**知识点：**

- 重复代码识别方法
- 代码合并技术
- 共享服务类创建
- 帮助方法提取
- 代理模式重构
- 代码质量改进
- 业务逻辑保持
- 测试验证流程

---

### 第八章 大型函数重构

**内容概述：** 分析复杂代码并将大型函数重构为更小、更专注的函数

**知识点：**

- 大型函数识别
- 单一职责原则
- 函数提取技术
- 代码可读性改进
- 错误处理模式保持
- 存根方法创建
- 增量测试验证
- 审计日志保持

---

### 第九章 简化复杂条件语句

**内容概述：** 使用 GitHub Copilot 简化复杂条件逻辑，减少嵌套并提高可读性

**知识点：**

- 复杂条件逻辑分析
- 嵌套减少技术
- 帮助方法提取
- 提前返回与守卫子句
- 开关表达式使用
- 布尔变量优化条件
- 业务规则保持
- 代码可维护性提升

---

### 第十章 性能分析实现

**内容概述：** 使用 GitHub Copilot 实现性能分析，识别和优化性能瓶颈

**知识点：**

- 性能分析方法
- 性能瓶颈识别
- 代码优化技术
- 性能测试工具
- 性能指标监控
- 优化策略实施

---

### 第十一章 GitHub 问题管理

**内容概述：** 使用 GitHub Copilot 解决 GitHub 问题，分析问题并生成解决方案

**知识点：**

- GitHub Issues 管理
- 问题分析方法
- 解决方案生成
- 代码修复流程
- 问题跟踪与关闭

---

### 第十二章 安全扫描警报处理

**内容概述：** 使用 GitHub Copilot 解决 GitHub 秘密扫描警报，识别和修复安全漏洞

**知识点：**

- GitHub 安全扫描功能
- 秘密扫描警报识别
- 安全漏洞修复
- 密钥管理最佳实践
- 安全代码编写
- 警报解除流程

---

### 第十三章 规范驱动开发入门

**内容概述：** 开始规范驱动开发，学习使用 GitHub Spec Kit 进行规范驱动开发

**知识点：**

- 规范驱动开发概念
- GitHub Spec Kit 介绍
- 需求分析方法
- 规范文档编写
- 规范与代码关联
- 开发流程规划

---

### 第十四章 规范驱动开发实现

**内容概述：** 实现规范驱动开发，使用 GitHub Spec Kit 实现规范驱动开发流程

**知识点：**

- 规范驱动开发实施
- 规范到代码转换
- GitHub Spec Kit 工具使用
- 规范验证流程
- 代码与规范同步
- 开发工作流整合

---

### 第十五章 GitHub Copilot 自定义配置

**内容概述：** 配置自定义 GitHub Copilot 在 Visual Studio Code 中的设置

**知识点：**

- GitHub Copilot 自定义设置
- Visual Studio Code 配置优化
- 个性化工作流设置
- 模型选择与切换
- 内容过滤配置
- 企业级配置管理

---

### 第十六章 AI 应用开发

**内容概述：** 开发 AI 启用的应用程序，使用 GitHub Copilot SDK 构建 AI 应用程序

**知识点：**

- GitHub Copilot SDK 使用
- AI 应用开发流程
- SDK 集成方法
- AI 功能实现
- 应用程序部署
- AI 应用测试与验证

---

## 软件环境要求

完成本课程需要配置以下软件和工具：

### 基础软件

- Visual Studio Code（最新版本）
- C# Dev Kit 扩展（C# 路线）
- Python 扩展（Python 路线）
- GitHub Copilot 扩展
- GitHub Copilot Chat 扩展
- Git（版本 2.48 或更高）
- 配置用户名和邮箱
- 配置远程仓库访问

### GitHub 要求

- GitHub 账户
- 个人免费账户或企业账户
- 已验证的邮箱地址
- GitHub Copilot 订阅（以下任一）
- GitHub Copilot Free（每月 2,000 代码补全 + 50 次聊天/高级请求）
- GitHub Copilot Pro
- GitHub Copilot Pro+
- GitHub Copilot Business
- GitHub Copilot Enterprise

### 开发环境（根据技术路线选择）

#### C# 路线

- .NET SDK（版本 8.0 或更高）
- SQL Server LocalDB 或 SQLite
- NuGet.org 包源配置

#### Python 路线

- Python（版本 3.11 或更高）
- `uv` 包管理器
- `pytest` 测试框架
- `pip` 或 `uv` 依赖管理

### 可选工具

- GitHub Desktop（Git 图形化客户端）
- GitHub CLI（命令行工具）
- Specify CLI（规范驱动开发工具）
- Postman 或类似工具（API 测试）

### 系统要求

- 操作系统：Windows 10/11、macOS 或 Linux
- 内存：最低 8GB RAM（推荐 16GB）
- 存储：至少 10GB 可用空间
- 网络：稳定的互联网连接（用于 GitHub Copilot 服务）

## GH-300 认证准备

完成本课程后，学员将具备参加 Microsoft GH-300: GitHub Copilot 认证考试所需的专业知识和技能。

### 认证信息

- 考试代码：GH-300
- 认证名称：Microsoft Certified: GitHub Copilot
- 级别：中级
- 考试时长：100 分钟
- 考试语言：英语、西班牙语、葡萄牙语（巴西）、韩语、日语
- 考试方式：在线监考（通过 Pearson VUE）

### 考试涵盖内容

- 负责任地使用 GitHub Copilot
- 使用 GitHub Copilot 功能
- GitHub Copilot 功能详解
- 了解 GitHub Copilot 数据和体系结构
- 应用提示工程和上下文构建
- 使用 GitHub Copilot 提高开发人员工作效率
- 配置隐私、内容排除和安全措施

### 备考资源

- [GH-300 考试学习指南](https://aka.ms/GH300-StudyGuide)
- [模拟考试练习](https://learn.microsoft.com/credentials/certifications/github-copilot/practice/assessment?assessment-type=practice&assessmentId=218035372&practice-assessment-type=certification)
- [考试沙盒体验](https://GHCertDemo.starttest.com)
- [认证海报](https://arch-center.azureedge.net/Credentials/Certification-Poster_en-us.pdf)

### 考试安排

- 通过 Pearson VUE 安排考试
- 强烈建议使用个人 MSA 账户注册考试（如使用组织 AAD 账户，离职后考试记录将丢失）
- 价格根据考试所在国家或地区而定

### 重考政策

- 如果考试未通过，可在第一次考试 24 小时后重考
- 后续重考需等待更长时间
- 详细信息请访问：[补考政策](https://learn.microsoft.com/zh-cn/credentials/support/retake-policy)
