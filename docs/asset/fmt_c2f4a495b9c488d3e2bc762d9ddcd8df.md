
```yaml
<!-- .claude/skills/team-standards/SKILL.md -->
---
name: team-standards
description: 团队编码标准和最佳实践
---

你是团队编码标准的守护者。根据以下规范进行代码审查和指导：

## Python代码规范
- 使用Black格式化，行宽88字符
- 使用Ruff进行lint检查
- 类型注解覆盖率 > 80%
- 遵循Google Python Style Guide

## JavaScript/TypeScript规范
- 使用Prettier格式化
- 使用ESLint + TypeScript strict模式
- 优先函数式编程风格

## API设计规范
- RESTful命名
- 统一错误响应格式：`{ data, error }`
- 版本化路径：`/api/v1/...`
```

---

# Part 4: CI/CD深度集成

## 4.1 GitHub Actions集成

完整的CI workflow：

```
# .github/workflows/claude-ci.yml
name: Claude Code CI

on:
  pull_request:
    branches: [main, develop]
  push:
    branches: [main, develop]

env:
  ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}

jobs:
  code-review:
    name: Claude代码审查
    runs-on: ubuntu-latest
    steps:
      - name: Checkout代码
        uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: 获取变更的文件
        id: changed-files
        uses: tj-actions/changed-files@v39

      - name: Claude代码审查
        if: steps.changed-files.outputs.any_changed == 'true'
        run: |
          DIFF=$(git diff origin/${{ github.base_ref }}...HEAD)

          # 调用Claude API进行审查
          REVIEW=$(curl -s https://api.anthropic.com/v1/messages \
            -H "x-api-key: $ANTHROPIC_API_KEY" \
            -H "anthropic-version: 2023-06-01" \
            -H "content-type: application/json" \
            -d "{
              \"model\": \"claude-sonnet-4-6\",
              \"max_tokens\": 4096,
              \"messages\": [{
                \"role\": \"user\",
                \"content\": \"请审查以下PR的代码变更...\"
              }]
            }" | jq -r '.content[0].text')

          echo "## Claude代码审查结果" >> $GITHUB_STEP_SUMMARY
          echo "$REVIEW" >> $GITHUB_STEP_SUMMARY

  quality-check:
    name: 代码质量检查
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - run: pip install black ruff pytest pytest-cov
      - run: black --check .
      - run: ruff check .
      - run: pytest --cov --cov-report=xml
```

---

# Part 5: 团队监控与度量

## 5.1 Claude使用情况监控

监控脚本核心逻辑：

```
#!/usr/bin/env python3
"""
Claude使用情况监控脚本
跟踪团队成员的Claude API调用和成本
"""

import sqlite3
from datetime import datetime, timedelta

def log_api_call(user, model, prompt_tokens, completion_tokens, purpose=None, success=True):
    """记录一次API调用"""
    total_tokens = prompt_tokens + completion_tokens
    # 计算成本（Sonnet 4.6定价）
    cost = (prompt_tokens * 0.003 + completion_tokens * 0.015) / 1000

    # 存储到数据库
    # ...

def generate_usage_report(days=7):
    """生成使用情况报告"""
    # 总体统计、按用户统计、按目的统计
    # ...
    return {
        "period_days": days,
        "overall": {...},
        "by_user": [...],
        "success_rate": 98.5
    }
```

---

# Part 6: 跨团队协作案例

## 6.1 多团队共享配置

团队配置继承结构：

```
company-claude-config/           # 公司级配置（基础，作为 Git 子模块共享）
├── .editorconfig
├── vscode/
│   └── settings.json
├── settings.json                # 公司级 Claude 通用设置
└── claude/
    └── skills/
        └── company-standards/   # 公司编码标准

team-a-project/                  # 团队A项目（引用公司基础配置）
├── .claude/
│   └── settings.json            # 团队A项目级设置
├── CLAUDE.md                    # 团队A项目指令
└── claude/
    └── skills/
        └── team-a-patterns/     # 团队A特定模式

team-b-project/                  # 团队B项目（引用公司基础配置）
├── .claude/
│   └── settings.json            # 团队B项目级设置
├── CLAUDE.md                    # 团队B项目指令
└── claude/
    └── skills/
        └── team-b-patterns/     # 团队B特定模式
```

配置共享方式：

- 公司基础配置放在共享仓库中，作为 Git 子模块引入各团队项目
- 各团队在项目根目录的 `.claude/settings.json` 中定义团队级设置（自动生效）
- 用户级配置在 `~/.claude/settings.json`，项目级配置自动覆盖用户级

## 6.2 `modelOverrides`：自定义模型端点映射

企业环境中，通常需要将Claude模型请求转发到自有的云端点（如Amazon Bedrock、Azure OpenAI），而非直接调用Anthropic API。`modelOverrides`配置可实现模型ID到实际端点的映射：

```
// settings.json（用户级或项目级均可）
{
  "modelOverrides": {
    "claude-sonnet-4-6": "arn:aws:bedrock:us-east-1:123456:model/claude-sonnet",
    "claude-opus-4-6": "your-azure-deployment-name"
  }
}
```

工作原理：

- 设置后，Claude Code内部仍使用标准模型名称（如`claude-sonnet-4-6`）
- 实际API请求会自动转发到`modelOverrides`中映射的自定义端点
- 团队成员无需关心底层端点细节，统一使用标准模型名即可

适用场景：

- 企业通过Amazon Bedrock部署的Claude模型（使用ARN标识）
- 通过Azure OpenAI Service访问的Claude部署
- 内部API网关代理的自定义模型端点

> 提示：建议在公司级`settings.json`中统一配置`modelOverrides`，确保所有团队成员使用相同的企业端点。

## 6.3 Worktree系统：大型Monorepo并行开发

企业级大型monorepo（几十GB级别）中，每个开发者通常只需操作其中一部分代码。Claude Code的`Worktree`系统通过`Git worktree` + `sparse checkout`实现轻量化隔离环境：

启动方式：

```
# 使用 --worktree 标志启动，自动创建Git worktree隔离环境
claude --worktree
```

`Sparse Checkout`配置：

通过`worktree.sparsePaths`指定只检出monorepo中的特定路径，避免克隆整个仓库：

```
// .claude/settings.json
{
  "worktree": {
    "sparsePaths": [
      "packages/frontend/**",
      "packages/shared/**",
      "package.json",
      "tsconfig.json"
    ]
  }
}
```

企业实践建议：

- 按模块划分：每个团队只检出自己负责的`packages/*`子目录和共享依赖
- 配合Agent Teams：多个Agent分别在不同`worktree`中并行开发不同模块，互不干扰
- CI/CD集成：在CI环境中使用`sparse checkout`加速构建，只拉取变更涉及的模块

> 注意：`Worktree`环境是临时的，退出Claude Code后可选择保留或清理。适合短期任务和并行开发场景。

---

# 第二部分：企业安全与合规指南

## 🎯 本部分学习目标

安全问题是企业的生命线！`API Key`泄露到GitHub后被恶意刷量$10,000+的案例屡见不鲜。

完成本部分后，你将能够：

1. ✅ 建立 `API Key`管理体系，防止密钥泄露和滥用
2. ✅ 实现敏感数据全链路保护，从代码到日志到数据库
3. ✅ 满足 GDPR/HIPAA/SOC2等合规要求，通过企业审计
4. ✅ 部署自动化安全扫描系统，持续检测代码和配置漏洞
5. ✅ 制定 `API`泄露应急响应流程，最小化安全事故影响
6. ✅ 执行 20条企业安全最佳实践，建立纵深防御体系

---

## Part 1: API Key安全管理

### 1.1 API Key泄露的灾难后果

真实安全事故案例：

| 事故 | 后果 | 损失 |
| --- | --- | --- |
| `API Key`提交到GitHub | 被扫描器发现，立刻被盗刷 | $15,000 |
| 硬编码在前端代码 | 暴露给所有用户 | 无限额度滥用 |
| 明文存储在配置文件 | 服务器被入侵后泄露 | $8,000 |
| 共享给离职员工 | 前员工恶意使用 | 法律诉讼 |
| 未设置使用限制 | 被DDoS攻击刷量 | $25,000 |

这些都是真实发生的安全事故，每一个都可能让公司蒙受重大损失！

### 1.2 企业级密钥管理架构

#### 1.2.1 密钥分级管理

```
密钥层级体系：

Production Keys (生产环境)
├── Master Key (主密钥)          # 最高权限，仅CEO/CTO持有
│   ├── 用途：密钥轮换、紧急恢复
│   └── 访问：2FA + 硬件密钥
├── Service Keys (服务密钥)      # 各服务独立密钥
│   ├── API Gateway Key          # 限制：10,000 RPM
│   ├── Backend Service Key      # 限制：5,000 RPM
│   └── Batch Job Key            # 限制：100 RPM
└── Developer Keys (开发密钥)    # 开发/测试环境
    ├── Dev Environment          # 限制：100 RPM
    └── Test Environment         # 限制：50 RPM

Staging Keys (预发布环境)
└── 独立密钥，与生产完全隔离

Development Keys (开发环境)
└── 团队共享，每周轮换
```

#### 1.2.2 密钥存储方案

❌ 绝对禁止的做法：

```
# ❌ 硬编码（找死）
ANTHROPIC_API_KEY = "sk-ant-api03-XXXXXXXX"

# ❌ 提交到Git（自杀）
# .env文件未加入.gitignore

# ❌ 明文配置文件（送死）
api_key: "sk-ant-api03-XXXXXXXX"

# ❌ 前端代码（群体自杀）
const API_KEY = "sk-ant-api03-XXXXXXXX";
```

✅ 正确的企业级做法：

```
# ✅ 方案1：环境变量 + Secrets Manager
import boto3

def get_api_key() -> str:
    """从AWS Secrets Manager获取API Key"""
    client = boto3.client('secretsmanager', region_name='us-east-1')
    response = client.get_secret_value(SecretId='prod/anthropic/api-key')
    return response['SecretString']

# ✅ 方案2：HashiCorp Vault
import hvac

def get_api_key_from_vault() -> str:
    """从Vault获取API Key"""
    client = hvac.Client(url='https://vault.example.com:8200')
    client.auth.approle.login(
        role_id=os.getenv("VAULT_ROLE_ID"),
        secret_id=os.getenv("VAULT_SECRET_ID")
    )
    secret = client.secrets.kv.v2.read_secret_version(
        path='anthropic/api-key',
        mount_point='secret'
    )
    return secret['data']['data']['key']
```

三大方案对比：
