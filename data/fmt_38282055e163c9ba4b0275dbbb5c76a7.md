
# 多Agent智能教育与个性化学习系统

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/DBmodKPSooqkKrxMHFccCJEGnwe](https://qqs7y1hozd1.feishu.cn/docx/DBmodKPSooqkKrxMHFccCJEGnwe)

> 这是一个企业级多Agent智能教育系统，用5个AI Agent协作完成个性化教学，是一个面向求职者的完整项目，帮助你：

1. 拥有一个可以在简历上写、面试时讲的企业级多Agent项目
2. 理解多Agent系统的核心架构思想（Mesh + 事件驱动）
3. 掌握面试八股文、STAR回答法、简历写法的全套技巧
4. 用人工智能主流Python语言实现，适配AI行业岗位需求

![](img/db2e30fe9964bcc0d3b110405ab61619.png)

## 整体架构

本系统采用 Mesh + 事件驱动 架构，5个Agent通过`EventBus`双向异步通信。

### 架构图

```
                        ┌──────────────┐
                        │  React 前端   │
                        │  (WebSocket)  │
                        └──────┬───────┘
                               │
                        ┌──────▼───────┐
                        │ API Gateway  │
                        │  / EventBus  │
                        └──────┬───────┘
                               │
      ┌────────────────────────┼────────────────────────┐
      │                        │                        │
 ┌────▼──────┐          ┌─────▼──────┐          ┌──────▼──────┐
 │ Assessment│◄────────►│   Tutor    │◄────────►│    Hint     │
 │   Agent   │          │   Agent    │          │    Agent    │
 └────┬──────┘          └────────────┘          └─────────────┘
      │                       ▲
      │                       │
 ┌────▼──────┐          ┌─────┴──────┐
 │ Curriculum│◄────────►│ Engagement │
 │   Agent   │          │   Agent    │
 └───────────┘          └────────────┘
      │                       │
      └───────┬───────────────┘
              ▼
      ┌───────────────┐
      │ 共享学习者状态  │
      │ (PostgreSQL)  │
      └───────────────┘
```

## 事件流设计

### 核心事件流：学生答题

```
学生答题
  → `STUDENT_SUBMISSION` 事件
  → `Assessment Agent` 处理
      → `MASTERY_UPDATED` 事件
          → `Curriculum Agent`（更新SM-2复习计划）
      → `ASSESSMENT_COMPLETE` 事件
          → `Tutor Agent`（生成苏格拉底式回复）
          → `Engagement Agent`（分析学习状态）
  → `Engagement Agent` 并行处理
      → 如果检测到挫败 → `ENGAGEMENT_ALERT` 事件
          → `Tutor Agent`（降低难度）
          → `Curriculum Agent`（放慢节奏）
```

### 提示流：学生卡住

![](img/7902549d80d0c519b7612027f7a5ce46.png)

```
学生连续答错
  → `Tutor Agent` 检测到 `attempts >= 2`
  → `HINT_NEEDED` 事件
  → `Hint Agent` 处理
      → 判断提示级别（1/2/3）
      → `HINT_RESPONSE` 事件
      → `Tutor Agent` 转发给学生
```

## 为什么选择 Mesh + 事件驱动

### 与其他架构模式的对比

![](img/0f6b982e4c2135f44f0a87b892d03ea9.png)

> Supervisor 模式：

+ 中心化调度器分配任务给各Agent
+ 缺点：调度器是瓶颈和单点故障
+ 不适合本场景：教育交互是双向的、非线性的

> Pipeline 模式：

+ Agent按固定顺序串行处理
+ 缺点：灵活性差，不支持双向交互
+ 不适合本场景：学习路径不是固定的

> Mesh 模式 (我们的选择)：

+ Agent之间通过事件总线自由通信
+ 优点：松耦合、可扩展、支持双向交互
+ 适合本场景：Agent需要实时感知和响应其他Agent的状态

### 开闭原则验证

新增一个"家长通知Agent"只需要：

![](img/f5d846fe3c8bc1e5513ab7f58b353731.png)

```
class ParentNotifyAgent(BaseAgent):
    @property
    def subscribed_events(self):
        return [EventType.WEAKNESS_DETECTED, EventType.ENCOURAGEMENT]

    async def handle_event(self, event):
        # 发送通知给家长
        pass
```

无需修改任何现有Agent代码。

## 数据流

### 学习者模型数据流

```
学生行为数据 → `Assessment Agent` → BKT更新`mastery`
                                → 持久化到数据库
                                → 事件通知其他Agent
                                      ↓
                               `Curriculum Agent` → SM-2更新复习计划
                               `Tutor Agent` → 调整教学难度
                               `Engagement Agent` → 检测学习状态
```

### 状态一致性保证

![](img/03743281dfc459d76a28b2aa1363dbba.png)

+ 单写者策略：`mastery`只由`Assessment Agent`写入
+ 事件溯源：所有变更通过事件记录，可追溯
+ 版本号：每次更新带`version`，防止并发冲突

## 部署指南

![](img/6b43a545cba6fd19125f8d58ee57f0ca.png)

### 方式一：本地（推荐新手）

```
cd python
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python -m api.main
# 访问 http://localhost:8000/docs
```

#### 前端

```
cd frontend
npm install
npm run dev
# 访问 http://localhost:3000
```

### 方式二：Docker Compose（一键部署）

```
# 在项目根目录
cp .env.example .env
# 编辑 .env 填入 API Key

docker-compose up -d

# 查看服务状态
docker-compose ps

# 查看日志
docker-compose logs -f python-api

# 停止
docker-compose down
```

服务地址：

+ 前端：http://localhost:3000
+ Python API：http://localhost:8000/docs
+ PostgreSQL：localhost:5432
+ Redis：localhost:6379

### 5个Agent职责说明

### 为什么用 Mesh + 事件驱动？

> 选择 Mesh 的原因：

+ 实战过程中，`Tutor` 需要随时请求 `Hint`，`Assessment` 需要通知 `Curriculum` 调整路径
+ Agent 之间是双向、异步、事件驱动的，不是简单的串行调用
+ 新增 Agent 只需订阅事件，无需修改现有代码（开闭原则）

## 🧠 核心算法详解

![](img/813880386381de75f0d1af96086537c7.png)

### SM-2 间隔重复算法（`Curriculum Agent` 使用）

什么是间隔重复？ 就是"快忘了的时候复习效果最好"。Anki、SuperMemo 都用这个算法。

核心公式：

```
复习间隔计算：
  `I(1)` = 1 天（第1次复习）
  `I(2)` = 6 天（第2次复习）
  `I(n)` = `I(n-1)` × `EF`（之后每次间隔 = 上次间隔 × 难度因子）

难度因子更新：
  `EF'` = `EF` - 0.8 + 0.28 × `q` - 0.02 × `q²`
  
  其中 `q` 是回答质量（0-5分）：
    5 = 完美回答  → `EF` 增大 → 间隔变长
    4 = 正确但犹豫 → `EF` 不变
    3 = 勉强正确  → `EF` 略降
    0-2 = 错误    → 重置间隔，从头复习
```

### 贝叶斯知识追踪 BKT（`Assessment Agent` 使用）

什么是 BKT？ 用概率模型估计学生对每个知识点的掌握程度。

四个核心参数：

```
`P(L₀)` = 初始掌握概率（先验，通常 0.1-0.3）
`P(T)`  = 学习转移概率（每次练习后学会的概率）
`P(G)`  = 猜测概率（不会但猜对的概率）
`P(S)`  = 失误概率（会但做错的概率）

更新公式：
  如果学生答对：
    `P(Lₙ|correct)` = `P(Lₙ₋₁)` × (1 - `P(S)`) / `P(correct)`
  如果学生答错：
    `P(Lₙ|wrong)` = `P(Lₙ₋₁)` × `P(S)` / `P(wrong)`
```

### 苏格拉底式教学策略（`Tutor Agent` 使用）

什么是苏格拉底式教学？ 不直接告诉答案，而是通过提问引导学生自己发现答案。

```
学生："二次函数 y = x² + 2x + 1 的顶点在哪里？"

❌ 直接给答案："顶点是 (-1, 0)"

✅ 苏格拉底式引导：
  第1轮："你知道二次函数的顶点公式吗？或者，你能把这个式子配方吗？"
  第2轮："很好！你配成了 y = (x+1)²，那 (x+1)² 最小值是多少？此时 x 等于几？"
  第3轮："对了！所以顶点坐标是？"
```
