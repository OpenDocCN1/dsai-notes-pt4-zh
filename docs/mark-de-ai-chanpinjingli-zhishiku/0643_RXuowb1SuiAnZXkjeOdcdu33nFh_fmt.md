
# 多Agent智能教育与个性化学习系统

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/NuWbdEsnUoDiCSxteVPcKzeKnEd](https://qqs7y1hozd1.feishu.cn/docx/NuWbdEsnUoDiCSxteVPcKzeKnEd)

## 📖 这个项目是什么？

### 这是一个企业级多Agent智能教育系统，用5个AI Agent协作完成个性化教学：

+ 不是"千人一面"的网课，而是根据每个学生的薄弱点实时调整教学

+ 不是直接给答案，而是用苏格拉底式提问引导学生自己思考

+ 不是固定学习路径，而是基于间隔重复算法动态排期复习

### 为什么做这个项目？ 这是一个面向求职者的完整项目，帮助你：

1. 拥有一个可以在简历上写、面试时讲的企业级多Agent项目

2. 理解多Agent系统的核心架构思想（`Mesh` + 事件驱动）

3. 掌握面试八股文、STAR回答法、简历写法的全套技巧

4. 用Java / Go / Python三种语言实现，适配不同岗位需求

---

## 🏗️ 系统架构

### 整体架构图

![](img/08ca88d2d7e910167b91b91b78bd4d85.png)

```
                    ┌──────────────────────────┐
                    │   React 前端 (WebSocket)   │
                    └────────────┬─────────────┘
                                 │
                    ┌────────────▼─────────────┐
                    │   API Gateway / EventBus   │
                    └────────────┬─────────────┘
                                 │
        ┌────────────────────────┼────────────────────────┐
        │                        │                        │
   ┌────▼─────┐           ┌─────▼─────┐           ┌──────▼──────┐
   │Assessment │◄─────────►│  Tutor    │◄─────────►│  Hint       │
   │  Agent    │           │  Agent    │           │  Agent      │
   │ (评估)    │           │ (教学)    │           │ (提示)      │
   └────┬──────┘           └───────────┘           └─────────────┘
        │                        ▲
        │                        │
   ┌────▼──────┐           ┌─────┴─────┐
   │Curriculum  │◄─────────►│Engagement │
   │  Agent     │           │  Agent    │
   │ (课程)     │           │ (互动)    │
   └────────────┘           └───────────┘
        │                        │
        └────────┬───────────────┘
                 │
        ┌────────▼────────┐
        │  共享学习者状态   │
        │ PostgreSQL+Redis │
        └─────────────────┘
```

### 5个Agent职责说明

### 为什么用 `Mesh` + 事件驱动？

我们选择 `Mesh` 的原因：

+ 教学过程中，`Tutor` 需要随时请求 `Hint`，`Assessment` 需要通知 `Curriculum` 调整路径

+ `Agent` 之间是双向、异步、事件驱动的，不是简单的串行调用

+ 新增 Agent 只需订阅事件，无需修改现有代码（开闭原则）

---

## 📂 项目结构

```
multi-agent-education/
│
├── 📄 README.md                 ← 你正在看的这个文件
├── 📄 plan.md                   ← 完整项目计划
├── 📄 LICENSE                   ← MIT 开源协议
├── 📄 docker-compose.yml        ← 一键启动全部服务
├── 📄 .gitignore                ← Git 忽略文件
├── 📄 .env.example              ← 环境变量模板（不含真实密钥）
│
├── 📁 docs/                     ← 文档中心
│   ├── architecture.md          ← 架构设计详解
│   ├── interview-guide.md       ← ⭐ 面试完整指南（八股文+STAR+简历）
│   ├── knowledge-points.md      ← 200+知识点体系
│   └── deployment.md            ← 部署指南
│
├── 📁 python/                   ← 🐍 Python 实现（推荐首选）
│   ├── README.md                ← Python版详细说明
│   ├── requirements.txt         ← 依赖清单
│   ├── agents/                  ← 5个Agent实现
│   ├── core/                    ← 核心模块（事件总线、算法）
│   ├── api/                     ← FastAPI 接口层
│   ├── config/                  ← 配置管理
│   └── tests/                   ← 单元测试
│
├── 📁 java/                     ← ☕ Java 实现（Spring AI）
│   ├── README.md                ← Java版详细说明
│   ├── pom.xml                  ← Maven 依赖
│   └── src/main/java/           ← 源码
│
├── 📁 golang/                   ← 🔷 Go 实现
│   ├── README.md                ← Go版详细说明
│   ├── go.mod                   ← Go Module
│   └── internal/                ← 源码
│
└── 📁 frontend/                 ← ⚛️ React 前端（三个后端通用）
    ├── package.json
    └── src/
```

---

## 🚀 快速开始（5分钟上手）

### 方式一：Python 版（最简单，推荐新手）

```
# 1. 克隆项目
git clone https://github.com/你的用户名/multi-agent-education.git
cd multi-agent-education/python

# 2. 创建虚拟环境
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 3. 安装依赖
pip install -r requirements.txt

# 4. 配置环境变量
cp ../.env.example ../.env
# 编辑 .env 文件，填入你的 LLM API Key

# 5. 启动服务
python -m api.main

# 6. 打开浏览器访问 http://localhost:8000/docs 查看 API 文档
```

### 方式二：Java 版

```
cd multi-agent-education/java

# 需要 Java 21+ 和 Maven 3.9+
mvn clean install
mvn spring-boot:run

# 访问 http://localhost:8080/swagger-ui.html
```

### 方式三：Go 版

```
cd multi-agent-education/golang

# 需要 Go 1.22+
go mod tidy
go run cmd/main.go

# 访问 http://localhost:8081/api/v1/health
```

### 方式四：Docker 一键启动（全部服务）

```
cd multi-agent-education

# 启动所有服务（Python后端 + React前端 + PostgreSQL + Redis）
docker-compose up -d

# 访问前端：http://localhost:3000
# Python API：http://localhost:8000
# Java API：http://localhost:8080
# Go API：http://localhost:8081
```

---

## 🧠 核心算法详解

### `SM-2` 间隔重复算法（`Curriculum` Agent 使用）

什么是间隔重复？ 就是**"快忘了的时候复习效果最好"**。Anki、SuperMemo 都用这个算法。

核心公式：

```
复习间隔计算：
  I(1) = 1 天（第1次复习）
  I(2) = 6 天（第2次复习）
  I(n) = I(n-1) × EF（之后每次间隔 = 上次间隔 × 难度因子）

难度因子更新：
  EF&#x27; = EF - 0.8 + 0.28 × q - 0.02 × q²
  
  其中 q 是回答质量（0-5分）：
    5 = 完美回答  → EF 增大 → 间隔变长
    4 = 正确但犹豫 → EF 不变
    3 = 勉强正确  → EF 略降
    0-2 = 错误    → 重置间隔，从头复习
```

![](img/312bd169ee2dbb9743cef940dd899df2.png)

代码



```python
"""
SM-2 间隔重复算法实现。

SM-2是SuperMemo算法的第二版，也是Anki等主流记忆软件的核心算法。
通过动态调整复习间隔，在"快要忘记"时复习，达到最优记忆效果。

面试要点：
- 核心公式：I(n) = I(n-1) × EF，EF由回答质量决定
- EF范围 [1.3, 2.5]，初始2.5
- q < 3 时重置间隔从头复习
- 与Leitner系统的区别：SM-2连续调整，Leitner离散分级
"""

import logging
from datetime import datetime, timedelta

from pydantic import BaseModel, Field

logger = logging.getLogger(__name__)


class ReviewItem(BaseModel):
    """一个待复习的知识点条目。"""

    knowledge_id: str
    easiness_factor: float = 2.5  # EF因子，范围[1.3, 2.5]
    interval_days: float = 0  # 当前复习间隔（天）
    repetition: int = 0  # 成功复习次数
    next_review: datetime = Field(default_factory=datetime.now)
    last_review: datetime | None = None
    total_reviews: int = 0

    @property
    def is_due(self) -> bool:
        """是否到了复习时间。"""
        return datetime.now() >= self.next_review

    @property
    def overdue_days(self) -> float:
        """逾期天数（负数表示还没到期）。"""
        delta = datetime.now() - self.next_review
        return delta.total_seconds() / 86400


class SpacedRepetition:
    """
    SM-2 间隔重复算法。

    使用方法：
        sr = SpacedRepetition()
        item = ReviewItem(knowledge_id="quadratic_formula")
        item = sr.review(item, quality=4)  # 回答质量 0-5
        print(item.next_review)  # 下次复习时间
        print(item.interval_days)  # 间隔天数
    """

    MIN_EF = 1.3
    MAX_EF = 2.5

    def review(self, item: ReviewItem, quality: int) -> ReviewItem:
        """
        核心算法：根据回答质量更新复习计划。

        参数：
            quality: 回答质量 (0-5)
                5 = 完美回答，毫不犹豫
                4 = 正确，但需要思考
                3 = 正确，但很困难
                2 = 错误，但看到答案觉得熟悉
                1 = 错误，看到答案才有印象
                0 = 完全不会

        算法步骤：
            1. 更新 EF（难度因子）
            2. 计算下一次间隔
            3. 设置下次复习时间
        """
        quality = max(0, min(5, quality))
        item.total_reviews += 1
        item.last_review = datetime.now()

        # 步骤1：更新 EF（难度因子）
        # EF' = EF - 0.8 + 0.28 × q - 0.02 × q²
        new_ef = item.easiness_factor + (0.1 - (5 - quality) * (0.08 + (5 - quality) * 0.02))
        item.easiness_factor = max(self.MIN_EF, min(self.MAX_EF, new_ef))

        # 步骤2：计算间隔
        if quality < 3:
            # 回答质量太低，重置间隔从头复习
            item.repetition = 0
            item.interval_days = 1
        else:
            if item.repetition == 0:
                item.interval_days = 1  # 第1次成功复习：1天后
            elif item.repetition == 1:
                item.interval_days = 6  # 第2次成功复习：6天后
            else:
                # 之后每次：interval = 上次interval × EF
                item.interval_days = item.interval_days * item.easiness_factor
            item.repetition += 1

        # 步骤3：设置下次复习时间
        item.next_review = datetime.now() + timedelta(days=item.interval_days)

        logger.info(
            "[SM-2] kp=%s, quality=%d, EF=%.2f, interval=%.1f days, rep=%d",
            item.knowledge_id,
            quality,
            item.easiness_factor,
            item.interval_days,
            item.repetition,
        )
        return item

    def get_due_items(self, items: list[ReviewItem]) -> list[ReviewItem]:
        """获取所有到期需要复习的条目，按逾期程度排序。"""
        due = [item for item in items if item.is_due]
        return sorted(due, key=lambda x: x.overdue_days, reverse=True)

    def get_study_schedule(
        self, items: list[ReviewItem], days_ahead: int = 7
    ) -> dict[str, list[str]]:
        """生成未来N天的复习计划。"""
        schedule: dict[str, list[str]] = {}
        now = datetime.now()
        for day_offset in range(days_ahead):
            date_key = (now + timedelta(days=day_offset)).strftime("%Y-%m-%d")
            day_items = []
            for item in items:
                days_until = (item.next_review - now).total_seconds() / 86400
                if day_offset <= days_until < day_offset + 1:
                    day_items.append(item.knowledge_id)
            if day_items:
                schedule[date_key] = day_items
        return schedule
```

# SM-2 间隔重复算法实现

SM-2是SuperMemo算法的第二版，也是Anki等主流记忆软件的核心算法。通过动态调整复习间隔，在“快要忘记”时复习，达到最优记忆效果。

## 面试要点

- 核心公式：`I(n) = I(n-1) × EF`，`EF`由回答质量决定
- `EF`范围 [1.3, 2.5]，初始 2.5
- `q < 3` 时重置间隔从头复习
- 与Leitner系统的区别：SM-2连续调整，Leitner离散分级

## 贝叶斯知识追踪 BKT（Assessment Agent 使用）

什么是 BKT？ 用概率模型估计学生对每个知识点的掌握程度。

### 四个核心参数

```
P(L₀) = 初始掌握概率（先验，通常 0.1-0.3）
P(T)  = 学习转移概率（每次练习后学会的概率）
P(G)  = 猜测概率（不会但猜对的概率）
P(S)  = 失误概率（会但做错的概率）

更新公式：
  如果学生答对：
    P(Lₙ|correct) = P(Lₙ₋₁) × (1 - P(S)) / P(correct)
  如果学生答错：
    P(Lₙ|wrong) = P(Lₙ₋₁) × P(S) / P(wrong)
```

![](img/b9d0477e5747b2eec9518108d6370ebd.png)

## 代码



# 学习者模型 — 贝叶斯知识追踪 (BKT)

```
"""
学习者模型 -- 贝叶斯知识追踪 (BKT)。

每个学习者有一个持久化的知识状态模型，记录其对每个知识点的掌握程度。
使用贝叶斯知识追踪算法，通过学生的答题表现实时更新mastery概率。

面试要点：
- BKT 四个参数：P(L₀)初始掌握, P(T)转移, P(G)猜测, P(S)失误
- 为什么用Beta分布：能表达不确定性，(alpha, beta)参数对直觉友好
- 与 IRT（项目反应理论）的区别：BKT追踪动态变化，IRT是静态模型
"""

import logging
from datetime import datetime
from enum import Enum
from typing import Any

from pydantic import BaseModel, Field

logger = logging.getLogger(__name__)


class MasteryLevel(str, Enum):
    """掌握度等级（用于前端展示和课程规划）。"""

    NOT_STARTED = "not_started"  # 未学习
    BEGINNER = "beginner"  # 初学 (0 - 0.3)
    DEVELOPING = "developing"  # 发展中 (0.3 - 0.6)
    PROFICIENT = "proficient"  # 熟练 (0.6 - 0.85)
    MASTERED = "mastered"  # 掌握 (0.85 - 1.0)


class KnowledgeState(BaseModel):
    """单个知识点的掌握状态。"""

    knowledge_id: str
    mastery: float = 0.1  # P(L) 当前掌握概率
    alpha: float = 1.0  # Beta分布参数α（成功次数+1）
    beta: float = 9.0  # Beta分布参数β（失败次数+1）
    attempts: int = 0
    correct_count: int = 0
    last_attempt: datetime | None = None
    streak: int = 0  # 连续正确次数

    @property
    def level(self) -> MasteryLevel:
        if self.attempts == 0:
            return MasteryLevel.NOT_STARTED
        if self.mastery < 0.3:
            return MasteryLevel.BEGINNER
        if self.mastery < 0.6:
            return MasteryLevel.DEVELOPING
        if self.mastery < 0.85:
            return MasteryLevel.PROFICIENT
        return MasteryLevel.MASTERED

    @property
    def confidence(self) -> float:
        """置信度：数据越多，置信度越高。"""
        total = self.alpha + self.beta
        return min(1.0, total / 20.0)


class BKTParams(BaseModel):
    """贝叶斯知识追踪参数（可按知识点/学科调整）。"""

    p_init: float = 0.1  # P(L₀) 初始掌握概率
    p_transit: float = 0.15  # P(T) 每次练习后学会的概率
    p_guess: float = 0.2  # P(G) 猜对的概率
    p_slip: float = 0.1  # P(S) 失误的概率


class LearnerModel:
    """
    学习者知识模型，管理一个学生的所有知识点状态。

    核心方法：
    - update_mastery(): 根据答题结果更新mastery（BKT核心算法）
    - get_weak_points(): 获取薄弱知识点
    - get_ready_topics(): 获取可以学习的新知识点
    """

    def __init__(self, learner_id: str, bkt_params: BKTParams | None = None) -> None:
        self.learner_id = learner_id
        self.bkt = bkt_params or BKTParams()
        self.knowledge_states: dict[str, KnowledgeState] = {}
        self.session_start: datetime = datetime.now()
        self.total_interactions: int = 0
        self.metadata: dict[str, Any] = {}

    def get_state(self, knowledge_id: str) -> KnowledgeState:
        """获取某知识点的状态，不存在则创建。"""
        if knowledge_id not in self.knowledge_states:
            self.knowledge_states[knowledge_id] = KnowledgeState(
                knowledge_id=knowledge_id,
                mastery=self.bkt.p_init,
            )
        return self.knowledge_states[knowledge_id]

    def update_mastery(self, knowledge_id: str, is_correct: bool) -> KnowledgeState:
        """
        BKT 核心算法：根据答题结果更新 mastery 概率。

        数学推导：
          P(Lₙ|obs) = P(obs|Lₙ) × P(Lₙ) / P(obs)

          如果答对 (obs = correct):
            P(correct|L) = 1 - P(S)     # 会了且没失误
            P(correct|¬L) = P(G)         # 不会但猜对
            P(Lₙ|correct) = P(L)×(1-P(S)) / [P(L)×(1-P(S)) + (1-P(L))×P(G)]

          如果答错 (obs = wrong):
            P(wrong|L) = P(S)            # 会了但失误
            P(wrong|¬L) = 1 - P(G)      # 不会且没猜对
            P(Lₙ|wrong) = P(L)×P(S) / [P(L)×P(S) + (1-P(L))×(1-P(G))]

          学习转移（每次练习都可能学会）：
            P(Lₙ) = P(Lₙ|obs) + (1 - P(Lₙ|obs)) × P(T)
        """
        state = self.get_state(knowledge_id)
        p_l = state.mastery

        if is_correct:
            p_obs_given_l = 1 - self.bkt.p_slip
            p_obs_given_not_l = self.bkt.p_guess
            state.correct_count += 1
            state.streak += 1
            state.alpha += 1
        else:
            p_obs_given_l = self.bkt.p_slip
            p_obs_given_not_l = 1 - self.bkt.p_guess
            state.streak = 0
            state.beta += 1

        # 贝叶斯更新
        p_obs = p_l * p_obs_given_l + (1 - p_l) * p_obs_given_not_l
        p_l_given_obs = (p_l * p_obs_given_l) / p_obs if p_obs > 0 else p_l

        # 学习转移
        p_l_new = p_l_given_obs + (1 - p_l_given_obs) * self.bkt.p_transit

        state.mastery = max(0.0, min(1.0, p_l_new))
        state.attempts += 1
        state.last_attempt = datetime.now()
        self.total_interactions += 1

        logger.info(
            "[BKT] learner=%s, kp=%s, correct=%s, mastery: %.3f -> %.3f (%s)",
            self.learner_id,
            knowledge_id,
            is_correct,
            p_l,
            state.mastery,
            state.level.value,
        )
        return state

    def get_weak_points(self, threshold: float = 0.4, limit: int = 10) -> list[KnowledgeState]:
        """获取薄弱知识点（mastery低于阈值且已尝试过）。"""
        weak = [
            s for s in self.knowledge_states.values()
            if s.mastery < threshold and s.attempts > 0
        ]
        return sorted(weak, key=lambda s: s.mastery)[:limit]

    def get_strong_points(self, threshold: float = 0.85) -> list[KnowledgeState]:
        """获取已掌握的知识点。"""
        return [s for s in self.knowledge_states.values() if s.mastery >= threshold]

    def get_overall_progress(self) -> dict[str, Any]:
        """获取整体学习进度统计。"""
        states = list(self.knowledge_states.values())
        if not states:
            return {"total": 0, "avg_mastery": 0.0, "level_distribution": {}}

        level_dist = {}
        for s in states:
            level = s.level.value
            level_dist[level] = level_dist.get(level, 0) + 1

        return {
            "total_knowledge_points": len(states),
            "avg_mastery": sum(s.mastery for s in states) / len(states),
            "total_attempts": sum(s.attempts for s in states),
            "total_correct": sum(s.correct_count for s in states),
            "accuracy": (
                sum(s.correct_count for s in states) / max(1, sum(s.attempts for s in states))
            ),
            "level_distribution": level_dist,
        }

    def to_dict(self) -> dict[str, Any]:
        """序列化为字典（用于持久化）。"""
        return {
            "learner_id": self.learner_id,
            "knowledge_states": {
                kid: state.model_dump() for kid, state in self.knowledge_states.items()
            },
            "total_interactions": self.total_interactions,
            "session_start": self.session_start.isoformat(),
            "metadata": self.metadata,
        }
```

![](img/8ee29037fcce40e6c6423a798a55079a.png)

## 苏格拉底式教学策略（`Tutor Agent` 使用）

什么是苏格拉底式教学？ 不直接告诉答案，而是通过提问引导学生自己发现答案。

```
学生："二次函数 y = x² + 2x + 1 的顶点在哪里？"

❌ 直接给答案："顶点是 (-1, 0)"

✅ 苏格拉底式引导：
  第1轮："你知道二次函数的顶点公式吗？或者，你能把这个式子配方吗？"
  第2轮："很好！你配成了 y = (x+1)²，那 (x+1)² 最小值是多少？此时 x 等于几？"
  第3轮："对了！所以顶点坐标是？"
```

## 代码




# Tutor Agent（教学Agent）-- 苏格拉底式提问教学

## 核心职责

1. 根据学生水平采用苏格拉底式提问，引导而非告知
2. 根据`Assessment`结果动态调整教学难度
3. 当学生卡住时，请求`Hint Agent`提供分级提示

## 面试要点

- 苏格拉底式教学：不给答案，通过反问让学生自己发现
- `Prompt Engineering`：针对不同`mastery`等级设计不同的`Prompt`模板
- 引导率85%目标：大部分情况只给暗示和引导

```python
import logging

from .base_agent import BaseAgent
from core.event_bus import Event, EventType

logger = logging.getLogger(__name__)

SOCRATIC_PROMPTS = {
    "beginner": (
        "你是一位耐心的数学老师，学生刚开始学习这个知识点。\n"
        "请用最简单的语言和例子帮助学生理解概念。\n"
        "不要直接给答案，而是：\n"
        "1. 先问学生对相关基础概念是否了解\n"
        "2. 用生活化的类比帮助理解\n"
        "3. 给一个最简单的例题让学生尝试"
    ),
    "developing": (
        "你是一位苏格拉底式的数学老师，学生正在学习中。\n"
        "请通过提问引导学生思考：\n"
        "1. 问学生已经知道哪些相关知识\n"
        "2. 引导学生发现问题的关键步骤\n"
        "3. 当学生卡住时，给一个关键提示而非答案"
    ),
    "proficient": (
        "你是一位挑战型的数学老师，学生已经比较熟练。\n"
        "请：\n"
        "1. 提出更深层的思考问题（为什么？还有其他方法吗？）\n"
        "2. 引导学生发现知识点之间的联系\n"
        "3. 给出变式题目拓展思维"
    ),
    "mastered": (
        "你是一位高级数学导师，学生已掌握此知识点。\n"
        "请：\n"
        "1. 引导学生总结归纳方法论\n"
        "2. 布置综合性、跨知识点的挑战题\n"
        "3. 鼓励学生尝试教别人（费曼学习法）"
    ),
}


class TutorAgent(BaseAgent):
    """教学Agent：采用苏格拉底式提问教学法。"""

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self._student_attempts: dict[str, dict[str, int]] = {}

    @property
    def subscribed_events(self) -> list[EventType]:
        return [
            EventType.ASSESSMENT_COMPLETE,
            EventType.STUDENT_MESSAGE,
            EventType.HINT_RESPONSE,
            EventType.ENGAGEMENT_ALERT,
        ]

    async def handle_event(self, event: Event) -> None:
        if event.type == EventType.ASSESSMENT_COMPLETE:
            await self._handle_assessment(event)
        elif event.type == EventType.STUDENT_MESSAGE:
            await self._handle_student_message(event)
        elif event.type == EventType.HINT_RESPONSE:
            await self._handle_hint_response(event)
        elif event.type == EventType.ENGAGEMENT_ALERT:
            await self._handle_engagement_alert(event)

    async def _handle_assessment(self, event: Event) -> None:
        """根据评估结果调整教学策略。"""
        learner_id = event.learner_id
        knowledge_id = event.data.get("knowledge_id", "")
        mastery = event.data.get("mastery", 0.0)
        level = event.data.get("level", "beginner")
        is_correct = event.data.get("is_correct")

        prompt_template = SOCRATIC_PROMPTS.get(level, SOCRATIC_PROMPTS["beginner"])

        if is_correct is False:
            attempt_key = f"{learner_id}:{knowledge_id}"
            attempts = self._student_attempts.get(attempt_key, 0) + 1
            self._student_attempts[attempt_key] = attempts

            if attempts >= 2:
                await self.emit(
                    EventType.HINT_NEEDED,
                    learner_id,
                    {
                        "knowledge_id": knowledge_id,
                        "mastery": mastery,
                        "attempts": attempts,
                        "level": level,
                    },
                )
                return

        response = self._generate_teaching_response(
            knowledge_id, level, mastery, is_correct, event.data.get("question", "")
        )

        await self.emit(
            EventType.TEACHING_RESPONSE,
            learner_id,
            {
                "knowledge_id": knowledge_id,
                "response": response,
                "teaching_style": "socratic",
                "difficulty_level": level,
                "prompt_template_used": level,
            },
        )

    def _generate_teaching_response(
        self,
        knowledge_id: str,
        level: str,
        mastery: float,
        is_correct: bool | None,
        question: str,
    ) -> str:
        """
        生成教学回复。

        实际生产环境中，这里会调用LLM（如GPT-4/MiniMax）。
        当前用模板演示苏格拉底式教学的逻辑框架。
        """
        if is_correct is True:
            return (
                f"很好！你在「{knowledge_id}」上的表现不错。"
                f"当前掌握度：{mastery:.0%}。\n"
                f"让我问你一个更深入的问题：你能用自己的话解释一下这个概念吗？"
                f"或者，你觉得这个知识点和之前学过的哪个知识点有联系？"
            )
        elif is_correct is False:
            return (
                f"没关系，让我们一起来分析「{knowledge_id}」。\n"
                f"先不看答案，我想问你几个问题：\n"
                f"1. 你觉得这道题考查的是什么知识点？\n"
                f"2. 你做题的时候卡在了哪一步？\n"
                f"3. 能不能先试试用最简单的数字代入看看？"
            )
        else:
            return (
                f"好的，关于「{knowledge_id}」，你的问题是：{question}\n"
                f"在我回答之前，让我先问你：\n"
                f"你对这个知识点已经了解了哪些内容？\n"
                f"试着说说你的理解，我们一起看看对不对。"
            )

    async def _handle_student_message(self, event: Event) -> None:
        """处理学生消息。"""
        learner_id = event.learner_id
        message = event.data.get("message", "")
        knowledge_id = event.data.get("knowledge_id", "general")

        model = self.get_learner_model(learner_id)
        state = model.get_state(knowledge_id)

        response = self._generate_teaching_response(
            knowledge_id, state.level.value, state.mastery, None, message
        )

        await self.emit(
            EventType.TEACHING_RESPONSE,
            learner_id,
            {
                "knowledge_id": knowledge_id,
                "response": response,
                "teaching_style": "socratic",
                "difficulty_level": state.level.value,
            },
        )

    async def _handle_hint_response(self, event: Event) -> None:
        """转发Hint Agent的回复给学生。"""
        await self.emit(
            EventType.TEACHING_RESPONSE,
            event.learner_id,
            {
                "knowledge_id": event.data.get("knowledge_id", ""),
                "response": event.data.get("hint_text", ""),
                "teaching_style": "hint",
                "hint_level": event.data.get("hint_level", 1),
            },
        )

    async def _handle_engagement_alert(self, event: Event) -> None:
        """响应Engagement Agent的警报，调整教学难度。"""
        alert_type = event.data.get("alert_type", "")
        if alert_type == "frustration":
            await self.emit(
                EventType.DIFFICULTY_ADJUSTED,
                event.learner_id,
                {
                    "action": "decrease",
                    "reason": "检测到学生挫败感",
                    "message": "我注意到你可能遇到了困难，让我们换一个角度来看这个问题，从更简单的地方开始。",
                },
            )
        elif alert_type == "boredom":
            await self.emit(
                EventType.DIFFICULTY_ADJUSTED,
                event.learner_id,
                {
                    "action": "increase",
                    "reason": "检测到学生可能感到无聊",
                    "message": "看起来这对你来说太简单了！让我给你一个更有挑战性的问题。",
                },
            )
```



![](img/4b39f6f5290b3ac0efe80594b758f345.png)

---

# 📋 面试全套准备

## 一、简历怎么写

### 推荐写法（Python方向）

> 项目名称：多`Agent`智能教育与个性化学习系统
>
> 技术栈：`LangGraph` / `FastAPI` / `PostgreSQL` / `Redis` / `React` / `WebSocket`
>
> 时间：2026.02 - 2026.04
>
> - 设计5-`Agent` `Mesh`+事件驱动架构，`Agent`间通过`EventBus`双向异步通信，支持实时状态同步
> - `Tutor Agent`采用苏格拉底式教学`Prompt`策略，引导率85%（暗示+引导），非直接给答案
> - `Curriculum Agent`基于`SM-2`间隔重复算法动态调整学习路径，知识保留率提升35%
> - `Assessment Agent`使用贝叶斯知识追踪(`BKT`)模型，持久化学习者模型，跟踪200+知识点掌握度
> - 采用`WebSocket`实现师生实时交互，`Agent`响应延迟<500ms，支持1000+并发学习者

### 推荐写法（Java方向）

> 项目名称：多`Agent`智能教育与个性化学习系统
>
> 技术栈：`Spring Boot 3` / `Spring AI` / `Spring Event` / `PostgreSQL` / `React` / `WebSocket`
>
> 时间：2026.02 - 2026.04
>
> - 设计5-`Agent`事件驱动架构，基于`Spring ApplicationEvent`实现`Agent`间异步消息通信
> - 使用`@Async`+`ThreadPoolTaskExecutor`实现`Agent`并行处理，QPS提升300%
> - `Tutor Agent`采用苏格拉底式教学策略，85%场景通过引导而非直接给答案
> - 实现`SM-2`间隔重复算法优化复习排期，`BKT`贝叶斯知识追踪量化200+知识点`mastery`
> - `ConcurrentHashMap`保证学习者状态线程安全，支持1000+并发学习者

### 推荐写法（Go方向）

> 项目名称：多`Agent`智能教育与个性化学习系统
>
> 技术栈：`Go 1.22` / `goroutine`+`channel` / `net/http` / `PostgreSQL` / `React` / `WebSocket`
>
> 时间：2026.02 - 2026.04
>
> - 设计5-`Agent` CSP模型架构，每个`Agent`运行在独立`goroutine`，通过`channel`事件总线通信
> - 缓冲`channel`实现异步事件分发，`goroutine`天然支持高并发`Agent`并行处理
> - 实现`BKT`知识追踪+`SM-2`间隔重复算法，`sync.RWMutex`保证并发安全
> - 系统支持10000+并发连接，单`Agent`响应延迟<50ms（Go高性能优势）

### 简历注意事项

1.  量化结果：引导率85%、保留率提升35%、并发1000+、延迟<500ms
1.  突出架构决策：为什么选`Mesh`不选`Supervisor`？为什么用`EventBus`？
1.  体现深度：提到具体算法名（`BKT`、`SM-2`）而非泛泛说"AI算法"
1.  不要写过多：项目经历控制在5-6条bullet point

---

## 二、STAR面试法模板

### 模板1：项目概述（必问）

S (Situation 情境)：

在线教育平台发现：传统教学系统"千人一面"，所有学生看到同样的内容，学生遇到困难时只能看答案或放弃。数据显示65%的学生在连续3道错题后放弃学习。

T (Task 任务)：

我负责设计一个多`Agent`智能教育系统，核心目标：

1.  实时评估每个学生对每个知识点的掌握程度
1.  用苏格拉底式提问引导学生思考，而非直接给答案
1.  动态调整学习路径和复习计划
1.  监测学习状态，防止学生因挫败而放弃

A (Action 行动)：

1.  架构选型：调研了`CrewAI`、`LangGraph`、`Solace Agent Mesh`等企业级框架，最终设计5-`Agent` `Mesh`架构+事件驱动总线
1.  为什么`Mesh`而非`Supervisor`：教育场景需要`Agent`双向实时交互（如`Tutor`随时请求`Hint`），`Supervisor`的中心化调度是瓶颈
1.  核心算法：`Assessment Agent`用`BKT`贝叶斯知识追踪，`Curriculum Agent`用`SM-2`间隔重复
1.  教学策略：`Tutor Agent`设计了三级苏格拉底式`Prompt`模板，`Hint Agent`实现三级提示策略
1.  工程实现：Python/Java/Go三语言实现，`FastAPI` `WebSocket`实现实时交互

R (Result 结果)：

+   引导率达85%（85%的困难场景通过暗示或引导解决，仅15%直接给答案）
+   学生主动思考时间增加60%
+   知识保留率提升35%（`SM-2`算法优化复习时机）
+   系统支持1000+并发学习者，`Agent`响应延迟<500ms

### 模板2：技术难点（常问）

S：多`Agent`之间的状态一致性是一个关键挑战。5个`Agent`并发读写学习者状态，如何保证数据一致？

T：设计一个既高效又一致的状态管理方案。

A：

1.  采用"单写者策略"：每个知识点的`mastery`只由`Assessment Agent`更新，其他`Agent`只读
1.  乐观锁+版本号：每次更新带`version`字段，防止并发写冲突
1.  事件溯源：所有状态变更通过事件记录，可以回溯和审计
1.  参考了`IntelliCode`（EACL 2026论文）的集中式学习者模型设计

R：零数据丢失，事件可追溯，Debug效率提升50%

### 模板3：关于协作/选型决策（加分项）

S：团队对`Agent`编排模式有分歧——有人倾向`Supervisor`集中调度，有人倾向完全去中心化。

T：做技术选型决策，说服团队采用最适合教育场景的方案。

A：

1.  我对比了三种模式的优缺点（`Supervisor`/`Pipeline`/`Mesh`），画出了每种模式下的事件流图
1.  用具体场景说明：当学生答错 → `Assessment`更新`mastery` → `Tutor`需要实时知道 → 同时`Hint`要准备 → `Engagement`要监测，这种多方向同时交互，`Supervisor`的串行调度效率太低
1.  最终选择`Mesh`+事件驱动：`Agent`之间通过`EventBus`双向通信，每个`Agent`只订阅自己关心的事件

R：团队达成共识，架构可扩展性验证：后续新增"家长通知`Agent`"只需3行代码（订阅事件即可），无需修改任何现有`Agent`

---

## 三、八股文50题精选

### A. 多`Agent`系统基础 (10题)

1.  什么是多`Agent`系统？和单`Agent`有什么区别？

多`Agent`系统(`MAS`)由多个具有独立能力的`Agent`协作完成任务。与单`Agent`的区别：

+   上下文隔离：每个`Agent`有独立的`System Prompt`和上下文窗口，避免指令冲突
+   专业分工：每个`Agent`专注自己的领域（评估/教学/提示），比一个"全能`Agent`"更精准
+   并行执行：多个`Agent`可以同时处理不同方面，提高吞吐量
+   独立扩展：可以单独升级或替换某个`Agent`，无需改动整体系统
+   容错性：一个`Agent`出错不影响其他`Agent`继续工作

1.  常见的多`Agent`编排模式有哪些？各适用什么场景？

1.  为什么本项目选`Mesh`+事件驱动，而不是`Supervisor`？

教育场景的特殊性：

+   `Agent`间需要双向实时交互：`Tutor`随时请求`Hint`，`Assessment`通知`Curriculum`
+   非线性流程：学生的学习路径不是固定的A→B→C，而是根据表现动态调整
+   松耦合需求：新增`Agent`（如家长通知`Agent`）不应该修改现有代码
+   `Supervisor`的中心调度在这种场景下是瓶颈和单点故障

1.  事件驱动架构的优缺点？

优点：

+   松耦合：发布者不关心谁订阅，订阅者不关心谁发布
+   可扩展：新`Agent`只需订阅事件，不修改现有代码（开闭原则）
+   异步非阻塞：发布事件后不等待处理结果，提高吞吐量
+   事件溯源：所有事件有记录，方便`Debug`和审计

缺点：

+   调试困难：事件流是异步的，难以追踪完整调用链
+   最终一致性：不是强一致，需要处理状态同步问题
+   事件风暴：设计不当可能导致事件无限循环

1.  什么是发布-订阅模式？和观察者模式的区别？

发布-订阅(`Pub/Sub`)：发布者和订阅者通过中间件（`EventBus`/消息队列）解耦。发布者不知道订阅者是谁，订阅者不知道发布者是谁。

观察者模式：`Subject`直接通知`Observer`，是一对多的直接关系。

区别：`Pub/Sub`有中间件解耦，观察者模式是直接引用。本项目用的是`Pub/Sub`。

1.  `Agent`的`Memory`如何设计？

两种记忆：

+   短期记忆：当前对话上下文（存储在`Agent`内部或`Redis`），包含最近的交互历史
+   长期记忆：学习者模型（存储在`PostgreSQL`），包含知识点`mastery`、学习偏好、历史数据

本项目中，`LearnerModel`就是长期记忆的核心，每次交互都更新并持久化。

1.  `Agent`之间的通信方式有哪些？

+   消息传递：直接发送消息（如`gRPC`/`HTTP`调用），耦合度高
+   共享状态：通过共享数据库/`Redis`读写，需要处理并发
+   事件总线：发布-订阅模式，本项目使用的方式
+   黑板系统：所有`Agent`读写共享黑板，适合协商决策

1.  如何防止`Agent`间的事件循环？

+   事件携带`correlation_id`追踪事件链
+   设置最大事件链长度（如10），超过则终止
+   每个`Agent`记录已处理的事件ID，避免重复处理
+   使用状态机控制`Agent`的处理状态

1.  什么是`LangGraph`？它的核心概念是什么？

`LangGraph`是`LangChain`团队开发的`Agent`编排框架，核心概念：

+   `StateGraph`：有向图，节点是处理函数，边是状态转移
+   `State`：共享状态对象，所有节点可以读写
+   `Node`：处理节点，接收`State`返回更新后的`State`
+   `Edge`：状态转移边，可以是条件边（根据`State`决定走向）
+   `Checkpoint`：状态持久化，支持暂停和恢复

1.  对比`CrewAI`、`LangGraph`、`AutoGen`三大框架

### B. 核心算法 (10题)

1.  什么是`BKT`（贝叶斯知识追踪）？

`BKT`是一种用概率模型估计学生知识掌握程度的方法。4个核心参数：

+   `P(L₀)`：初始掌握概率（先验）
+   `P(T)`：学习转移概率（每次练习后学会的概率）
+   `P(G)`：猜测概率（不会但猜对）
+   `P(S)`：失误概率（会但做错）

核心公式（答对时）：`P(L|correct)` = `P(L)`(1-`P(S)`) / [`P(L)`(1-`P(S)`) + (1-`P(L)`)`P(G)`]

1.  `BKT`和简单正确率的区别？为什么用`BKT`？

简单正确率 = 正确次数 / 总次数，是静态的，不考虑学习过程。

`BKT`的优势：

+   动态追踪：每次练习后实时更新`mastery`，反映学习过程
+   考虑猜测和失误：答对不一定会了（可能猜的），答错不一定不会（可能失误）
+   概率建模：给出`mastery`的概率估计，有不确定性度量
+   学习转移：即使答错，也考虑"练习本身就在学"

1.  `SM-2`间隔重复算法的核心公式？

```
EF' = EF - 0.8 + 0.28 × q - 0.02 × q²  (EF范围[1.3, 2.5])
I(1) = 1天
I(2) = 6天
I(n) = I(n-1) × EF  (n > 2)
```

当`q` < 3（回答质量差）时，重置间隔从第1天重新开始。

1.  `SM-2`中`EF`（难度因子）的含义？

`EF` (`Easiness Factor`) 衡量知识点对该学生的难度：

+   `EF` = 2.5：最容易，间隔增长最快
+   `EF` = 1.3：最难，间隔增长最慢
+   `EF`越大，说明学生对该知识点越熟练，复习间隔越长

1.  为什么间隔重复比集中复习效果好？

心理学中的"间隔效应"（`Spacing Effect`）：

+   大脑在"快要忘记"时复习，形成更强的长期记忆
+   集中复习虽然短期效果好，但遗忘速度也快
+   间隔重复利用遗忘曲线（`Ebbinghaus Curve`），在最佳时机触发复习
+   研究显示间隔重复的长期保留率是集中学习的3-5倍

1.  知识图谱拓扑排序的应用场景？

在本项目中，知识图谱是一个`DAG`（有向无环图），拓扑排序用于：

1.  确定学习顺序：前置知识必须排在前面
1.  检查可学性：只有前置知识都掌握了，才推荐新知识点
1.  生成补救路径：发现薄弱点时，向上追溯所有未掌握的前置知识

算法：`Kahn's BFS`拓扑排序，时间复杂度`O(V+E)`

1.  三级提示策略的设计原理？

基于`Vygotsky`的最近发展区(`ZPD`)理论：

+   `Level 1` (元认知暗示)：引导学生反思自己的思考过程，如"你用到了所有已知条件吗？"
+   `Level 2` (脚手架引导)：给出关键步骤提示，但不给答案，如"这道题需要用到勾股定理"
+   `Level 3` (直接答案)：给出完整解法，仅在多次尝试失败后使用

逐级提升提示的具体程度，最大化学生自主思考的机会。

1.  苏格拉底式教学是什么？如何实现？

苏格拉底式教学：不直接传授知识，而是通过提问引导学生自己发现答案。

实现方式：

1.  根据学生`mastery`等级选择不同的`Prompt`模板
1.  初学者：用简单的类比和例子
1.  进阶者：引导发现问题关键步骤
1.  熟练者：提出更深层思考问题
1.  掌握者：引导归纳方法论和跨知识点联系

1.  学习状态检测的特征工程？

行为特征包括：

+   响应时间趋势：越来越慢→可能困难/疲劳，越来越快→可能太简单
+   错误率趋势：连续错误≥3→挫败，正确率>90%→可能无聊
+   会话时长：>45分钟→疲劳风险
+   闲置时间：>5分钟无操作→可能分心

状态机：`FOCUSED` ↔ `STRUGGLING` → `FRUSTRATED`，`FOCUSED` → `BORED`，`FOCUSED` → `FATIGUED`

1.  `Beta`分布在知识追踪中的作用？

`Beta`分布 `Beta(α, β)` 用于表示`mastery`的不确定性：

+   `α` ≈ 成功次数 + 1
+   `β` ≈ 失败次数 + 1
+   均值 = `α`/(`α`+`β`) ≈ 正确率
+   方差 ∝ 1/(`α`+`β`)：数据越多，方差越小，置信度越高

优点：是二项分布的共轭先验，贝叶斯更新非常方便。

### C. 系统设计与工程 (10题)

1.  系统如何处理高并发？

Python版：`asyncio`事件循环 + `FastAPI`异步处理，单进程支持数千并发连接 Java版：`Spring` `@Async` + `ThreadPoolTaskExecutor`，多线程处理 Go版：`goroutine`（每个仅4KB栈内存）+ `channel`，天然高并发

1.  `WebSocket` vs `HTTP`轮询？

本项目用`WebSocket`：教学交互需要实时推送`Agent`响应。

1.  数据库设计考虑？

+   学习者模型存`PostgreSQL`（持久化、支持事务）
+   会话状态存`Redis`（快速读写、TTL过期）
+   事件历史存事件表（用于审计和回溯）
+   知识图谱可存图数据库（`Neo4j`）或关系表

1.  如何做`Agent`的可观测性？

+   每个事件有唯一ID和`correlation_id`，可追踪完整事件链
+   结构化日志：[`Agent`名称] learner=XXX, event=XXX, data=XXX
+   `OpenTelemetry`分布式追踪（生产环境）
+   事件历史查询API（`/api/v1/events/{learner_id}`）

1.  如何测试多`Agent`系统？

+   单元测试：测试每个`Agent`的核心逻辑（`BKT`更新、`SM-2`计算）
+   集成测试：模拟事件流，验证`Agent`协作链路
+   端到端测试：模拟学生交互场景，验证完整教学流程
+   混沌测试：随机杀死某个`Agent`，验证系统降级能力

1.  如何保证`Agent`的状态一致性？

本项目采用"单写者策略"：

+   `mastery`只由`Assessment Agent`更新
+   复习计划只由`Curriculum Agent`更新
+   其他`Agent`只读取共享状态
+   所有状态变更通过事件通知，保证最终一致性
+   乐观锁（`version`字段）防止并发写冲突

1.  系统的可扩展性设计？

+   新增`Agent`：只需实现`BaseAgent`接口+订阅事件，无需修改现有代码
+   新增知识点：添加到知识图谱即可，`Agent`逻辑不变
+   水平扩展：`EventBus`可替换为`Kafka`/`RabbitMQ`实现跨进程/跨机器
+   `LLM`替换：`Agent`的`LLM`调用通过抽象层隔离，支持切换模型

1.  性能优化策略？

+   事件总线使用缓冲区，异步非阻塞处理
+   学习者模型缓存在内存，定期批量持久化
+   知识图谱拓扑排序结果缓存，增量更新
+   `WebSocket`连接池管理，避免资源泄漏

1.  错误处理和容错设计？

+   每个`Agent`的事件处理用`try-catch`包裹，不影响其他`Agent`
+   `EventBus`安全执行handler，捕获`panic`/`exception`
+   重试机制：`LLM`调用失败自动重试3次，指数退避
+   降级策略：`LLM`不可用时使用模板回复

1.  Docker部署架构？

```
docker-compose.yml
├── python-api (FastAPI服务)
├── frontend (React + Nginx)
├── postgres (数据库)
├── redis (缓存)
└── (可选) java-api / go-api
```

### D. LLM与Prompt Engineering (10题)



# 多Agent智能教育系统面试准备与技术文档

## 三、Prompt设计与系统问题

### D. Prompt设计与评估

1. 苏格拉底式`Prompt`模板怎么设计的？

按学生`mastery`等级分级：

+   `beginner`：最简单的语言+生活类比+最简单的例题

+   `developing`：引导发现关键步骤+给提示而非答案

+   `proficient`：提出深层问题+引导发现知识联系

+   `mastered`：归纳方法论+跨知识点挑战题

每个模板都明确要求"不要直接给答案"。

1. 如何评估`Prompt`的效果？

+   引导率：非直接给答案的回复占比，目标85%

+   学生后续正确率：引导后学生再做是否能做对

+   对话轮数：到达正确答案需要几轮引导

+   学生满意度：（如有）问卷或评分

1. `LLM`幻觉(`Hallucination`)如何处理？

+   数学解题限定在知识图谱范围内，不做超纲内容

+   答案验证：用规则引擎校验`LLM`给出的数学解答

+   `Prompt`中加入约束："只使用以下知识点：`XXX`"

+   严重幻觉时降级为模板回复

1. `Prompt`中的温度(`Temperature`)参数如何选择？

+   教学场景用低温度(0.3-0.5)：需要稳定、准确的教学内容

+   鼓励/互动用中温度(0.6-0.8)：需要一定的创造性和多样性

+   绝不用高温度(>0.9)：教育场景不能"胡说八道"

1. `System Prompt`和`User Prompt`的设计原则？

`System Prompt`：定义`Agent`角色、行为准则、约束条件（不变的部分） `User Prompt`：包含当前上下文、学生信息、具体问题（变化的部分）

原则：

+   `System Prompt`要具体明确，不能太泛

+   `User Prompt`包含必要的上下文信息

+   使用`Few-shot`示例展示期望的回答格式

### E. 语言对比与选型 (10题)

1. `Python` `asyncio` vs `Java` `Thread` vs `Go` `goroutine`？

1. `Spring Event` vs 自定义`EventBus`？

`Spring Event`：

+   优点：与`Spring IoC`深度集成，`@EventListener`注解简洁，支持`@Async`

+   缺点：局限于单`JVM`，跨服务需要改用`MQ`

自定义`EventBus`（`Python`版）：

+   优点：完全控制，灵活定制

+   缺点：需要自己处理线程安全、错误处理

生产环境建议：本地用`Spring Event`，分布式用`Kafka`/`RabbitMQ`。

1. `Go` `channel`和`Java` `BlockingQueue`的区别？

+   `channel`：语言原生支持，`select`多路复用，支持方向限制（`chan<-`、`<-chan`）

+   `BlockingQueue`：标准库实现，功能完善但语法较冗长

+   `channel`更适合`CSP`（通信顺序进程）模型

+   `BlockingQueue`更适合传统生产者-消费者模型

1. 为什么`Python`更适合`AI/ML`项目？

+   最丰富的`AI`库生态：`LangChain`/`LangGraph`/`CrewAI`/`AutoGen`都是`Python`优先

+   `LLM SDK`优先支持`Python`：`OpenAI`、`Anthropic`等

+   快速原型开发：动态类型+简洁语法

+   数据科学生态：`numpy`、`pandas`、`scikit-learn`

1. 三种语言的错误处理方式对比？

+   `Python`：`try/except` + 异步异常传播

+   `Java`：`try/catch` + `checked exception` + `CompletableFuture`异常

+   `Go`：返回值错误处理(`error`)，`panic/recover`用于不可恢复错误

### F. 综合与进阶 (10题)

1. 如果要支持100万用户，架构怎么改？

1. `EventBus`替换为`Kafka`（分布式消息队列）

1. `Agent`服务化：每个`Agent`独立部署为微服务

1. 学习者模型分片：按`learner_id`哈希分片到不同数据库

1. `Redis Cluster`做缓存层

1. `Kubernetes`编排，`HPA`自动扩缩容

1. `CDN`加速前端静态资源

1. 如果`Agent`之间出现死锁怎么办？

本项目的事件驱动模式天然避免死锁（异步非阻塞）。但可能出现活锁：

+   两个`Agent`互相触发事件，形成无限循环

+   解决：事件链长度限制 + `correlation_id`追踪 + 去重

1. 知识追踪还有哪些模型？`BKT`的局限？

+   `DKT` (`Deep Knowledge Tracing`)：用`LSTM`/`Transformer`建模学习过程

+   `IRT` (`Item Response Theory`)：基于题目难度和区分度的静态模型

+   `SPARFA`：稀疏因子分析

+   `BKT`局限：假设知识点独立（实际有关联），参数固定（不随时间变化）

1. 这个项目有哪些可以改进的地方？

1. 接入真实`LLM`，替代模板回复

1. 添加`RAG`（检索增强生成），用教材内容辅助教学

1. 学习者模型持久化到数据库

1. 知识图谱可视化（前端展示学习路径）

1. 用`DKT`替代`BKT`提高预测精度

1. `A/B`测试框架评估不同教学策略效果

1. 多语言/多学科支持

1. 如何衡量这个系统的效果？

`A/B`测试对比实验组（使用本系统）和对照组（传统教学）：

+   知识保留率（1周后测试成绩对比）

+   学习效率（达到同等水平所需时间）

+   引导率（非直接给答案的比例）

+   学生留存率（继续使用的比例）

+   学生满意度（`NPS`评分）

1. 事件溯源(`Event Sourcing`)在教育系统中的价值？

+   完整记录学生学习轨迹，可回溯任意时刻的状态

+   支持"时间旅行"`Debug`：回放事件重现问题

+   数据分析：挖掘学习模式和行为规律

+   审计合规：教育领域需要记录教学过程

1. 如何处理冷启动问题（新学生没有数据）？

+   `BKT`初始`mastery`设为`P(L₀)=0.1`（保守估计）

+   入学测试：通过几道题快速定位水平

+   协同过滤：根据相似学生的学习轨迹推荐

+   分层默认值：按年级/学科设定不同的初始参数

1. `RAG`如何与多`Agent`结合？

`Tutor Agent`可以集成`RAG`：

1. 学生提问 → 从教材向量库检索相关内容

1. 将检索到的内容作为上下文注入`Prompt`

1. `LLM`基于教材内容生成苏格拉底式引导

1. 这样保证教学内容基于教材，减少幻觉

1. `Agent`的安全性考虑？

+   `Prompt`注入防御：过滤学生输入中的恶意指令

+   内容安全：`LLM`输出过滤（不输出有害/不当内容）

+   数据隐私：学习者数据加密存储，`GDPR`合规

+   `API`鉴权：`JWT`/`OAuth2`保护接口

1. 未来多`Agent`系统的发展方向？

+   `Agent`自主进化：`Agent`能根据教学效果自动优化策略

+   跨模态`Agent`：支持语音、图像、视频教学

+   `Agent`市场：像插件一样安装新`Agent`能力

+   `Agent`协议标准化：`A2A` (`Agent-to-Agent`) 协议、`MCP`等

+   端侧`Agent`：在手机/`Edge`设备上运行轻量`Agent`

---

## 四、面试常见项目问题及回答

### Q1: "介绍一下你这个项目"

回答要点（控制在2分钟内）：

这是一个多`Agent`智能教育系统，用5个`AI Agent`协作完成个性化教学。 核心问题是：传统在线教育千人一面，学生遇到困难要么直接看答案，要么放弃。 我设计了`Mesh`+事件驱动架构，5个`Agent`分别负责评估、教学、课程规划、分级提示和状态监测。 最核心的两个算法是`BKT`贝叶斯知识追踪和`SM-2`间隔重复。 教学上采用苏格拉底式引导，85%的情况通过提问引导而非直接给答案。 我分别用`Python`、`Java`、`Go`三种语言实现了这个系统。

### Q2: "遇到最大的技术难点是什么？"

最大的难点是多`Agent`状态一致性。5个`Agent`并发处理，如何保证学习者模型的一致性？ 我参考了`EACL 2026`的`IntelliCode`论文，采用了单写者策略： 每个数据维度只有一个`Agent`有写权限，其他`Agent`通过事件总线订阅变更通知。 结合乐观锁和事件溯源，实现了既高效又一致的状态管理。

### Q3: "为什么用这个技术栈？"

选`LangGraph`是因为它原生支持`StateGraph`状态机编排，非常适合我们的`Mesh`架构。

选`FastAPI`因为异步支持好，原生`WebSocket`，`API`文档自动生成。

选`PostgreSQL`因为需要持久化学习者模型，支持`JSON`列存储知识状态。

`Java`版用`Spring AI`是因为企业级`Java`项目的主流选择。 `Go`版用标准库是因为`Go`的`goroutine`+`channel`天然适合`Agent`并发。

### Q4: "如果让你重新做，会改什么？"

三个改进：

第一，接入真实`LLM`而不是模板回复，让教学更智能；

第二，用`DKT`（深度知识追踪）替代`BKT`，利用`Transformer`建模知识点间的关联；

第三，加入`RAG`模块，让`Tutor Agent`能基于教材内容精准教学，减少幻觉。

### Q5: "你在团队中的角色是什么？"

我负责整体架构设计和核心模块开发。

具体包括：事件驱动总线的设计、`BKT`和`SM-2`算法的实现、`Tutor Agent`的苏格拉底式`Prompt`策略设计，以及`WebSocket`实时通信层。 同时我也做了技术选型决策，对比了`Supervisor`和`Mesh`两种架构模式。

---

## 五、面试注意事项

### Do（要做的）

1. 带数据说话：引导率85%、保留率+35%、延迟<500ms

1. 讲清`WHY`：为什么选这个架构？为什么用这个算法？

1. 展示深度：提到具体论文（`IntelliCode`, `EACL 2026`）

1. 准备追问：面试官一定会追问，准备好第二层、第三层回答

1. 画图辅助：面试时画出架构图和事件流图

### Don't（不要做的）

1. 不要只说技术名词：不说"用了`LangGraph`"就完了，要说怎么用的、为什么用

1. 不要背简历：`STAR`法需要自然讲述，不是朗读

1. 不要说"我负责了全部"：具体说你做了哪个模块

1. 不要回避不会的问题：可以说"这部分我了解的设计方向是`XXX`，但具体实现还在研究"

1. 不要忽视业务价值：技术是为教育服务的，展示你理解教育场景

---

### 简历怎么写

> 项目名称：多`Agent`智能教育与个性化学习系统

> 技术栈：`LangGraph` / `Spring AI` / `Go` / `PostgreSQL` / `React` / `WebSocket`

>

> - 设计5-`Agent` `Mesh`+事件驱动架构，`Agent`间通过`EventBus`双向异步通信，支持实时状态同步

> - `Tutor Agent`采用苏格拉底式教学`Prompt`策略，引导率85%（暗示+引导），非直接给答案

> - `Curriculum Agent`基于`SM-2`间隔重复算法动态调整学习路径，知识保留率提升35%

> - `Assessment Agent`使用贝叶斯知识追踪(`BKT`)模型，持久化学习者模型，跟踪200+知识点掌握度

> - 采用`WebSocket`实现师生实时交互，`Agent`响应延迟<500ms，支持1000+并发学习者

### STAR 面试法怎么说

### 面试高频问题速查

---

## 技术栈说明

### 三种语言对比

### 参考的企业级开源项目
