
# 基于多Agent电商推荐系统项目

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Rljvdqa0Uobs5ixQdEgc22NCn1d](https://qqs7y1hozd1.feishu.cn/docx/Rljvdqa0Uobs5ixQdEgc22NCn1d)

> 面向小白的企业级 AI Agent 项目 — 从零理解 Multi-Agent 架构，配套三语言代码 + 八股文 + 简历模板 + STAR面试话术，找工作全流程覆盖。

---

## 🤔 这个项目是什么？

### 用一句话解释

> 用 AI Agent 技术，让电商平台的推荐 + 文案 + 库存三个系统协同工作，像一个聪明的"AI 运营团队"一起为每位用户生成个性化推荐结果。

### 它解决了什么问题？

传统电商推荐系统存在三大痛点：

### 技术关键词（面试常考）

> Multi-Agent · Supervisor模式 · LangGraph · asyncio并行 · Redis Feature Store · A/B Testing · Thompson Sampling · RAG · ReAct · MiniMax LLM

---

## 🏗 系统架构（看图秒懂）

```
┌─────────────────────────────────────────────────────────────────┐
│                         用户发起推荐请求                           │
│                    {"user_id": "u001", "num_items": 5}           │
└───────────────────────────────┬─────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Supervisor 协调Agent                           │
│                  (python/orchestrator/supervisor.py)              │
│                                                                   │
│  ════════════════ Phase 1: 并行执行 ═══════════════════           │
│  ┌──────────────────────┐    ┌──────────────────────┐            │
│  │   用户画像 Agent      │    │   商品召回 Agent      │            │
│  │  user_profile_agent  │    │  product_rec_agent   │            │
│  │  ──────────────────  │    │  ────────────────── │            │
│  │  Redis → 实时行为特征 │    │  协同过滤+向量检索召回 │            │
│  │  RFM模型 → 用户分群   │    │  返回候选商品列表     │            │
│  └──────────┬───────────┘    └──────────┬──────────┘            │
│             │                           │                         │
│  ════════════════ Phase 2: 并行执行 ═══════════════════           │
│  ┌──────────────────────┐    ┌──────────────────────┐            │
│  │   LLM重排 Agent      │    │   库存决策 Agent      │            │
│  │  (product_rec再次调用)│    │   inventory_agent    │            │
│  │  ──────────────────  │    │  ────────────────── │            │
│  │  用户画像 × 商品属性  │    │  MySQL → 实时库存查询 │            │
│  │  LLM精排，返回TopN   │    │  过滤缺货，输出限购策略│            │
│  └──────────┬───────────┘    └──────────┬──────────┘            │
│             │                           │                         │
│  ════════════════ Phase 3: 串行执行 ═══════════════════           │
│             └──────────────┬────────────┘                         │
│                            ▼                                      │
│             ┌──────────────────────────────┐                      │
│             │      结果聚合器               │                      │
│             │  库存过滤 → 排序合并 → TopN   │                      │
│             └──────────────┬───────────────┘                      │
│                            ▼                                      │
│             ┌──────────────────────────────┐                      │
│             │   营销文案 Agent              │                      │
│             │  marketing_copy_agent        │                      │
│             │  ────────────────────────── │                      │
│             │  5套Prompt模板 × 用户分群    │                      │
│             │  LLM生成 + 广告法合规校验    │                      │
│             └──────────────┬───────────────┘                      │
│                            ▼                                      │
│             ┌──────────────────────────────┐                      │
│             │   A/B 测试引擎               │                      │
│             │  用户ID哈希分桶              │                      │
│             │  Thompson Sampling 动态调优  │                      │
│             └──────────────┬───────────────┘                      │
└──────────────────────────────┬──────────────────────────────────┘
                               ▼
              ┌─────────────────────────────────┐
              │  个性化推荐响应（返回给用户）      │
              │  商品列表 + 个性化文案 + 实验分组 │
              └─────────────────────────────────┘
```

![](img/a4d9d05283737712a205a343080c845d.png)

### 为什么用 Supervisor 模式？

Supervisor 模式是 Multi-Agent 系统中最主流的编排方式之一：

```
Supervisor 模式                     Handoffs 模式
──────────────────────              ──────────────────────
   Supervisor（中枢）                 Agent A → Agent B
    ┌────┬────┬────┐                       ↓
    ▼    ▼    ▼    ▼                 Agent B → Agent C
   A    B    C    D                        ↓
    └────┴────┴────┘                 Agent C → ...
    结果聚合 → 响应

✅ 集中控制，流程清晰          ✅ 去中心化，灵活
✅ 并行执行，延迟低            ✅ 适合对话/开放式任务
✅ 异常统一处理                ❌ 状态管理复杂
本项目采用 Supervisor 模式
```

## 🤖 四大核心 Agent 详解

### Agent 1：用户画像 Agent

**它做什么？**

把用户的历史行为数据（点击、购买、收藏）转化成结构化的"用户画像"，供其他 Agent 使用。

**核心逻辑（简化）**：

```
# Step 1：从 Redis Feature Store 获取实时行为特征
behavior = await feature_store.get_user_features(user_id)
# 返回: {"clicks_1h": 12, "purchases_7d": 3, "categories": ["手机", "耳机"]}

# Step 2：调用 LLM 分析，输出结构化画像
prompt = f"用户行为数据: {behavior}\n请分析用户分群和RFM得分，输出JSON"
profile_json = await llm.invoke(prompt)
# 输出: {"segments": ["active", "price_sensitive"], "rfm_score": {"recency": 0.8}}

# Step 3：返回 UserProfile 对象
return UserProfile(user_id=user_id, segments=["active"], rfm_score=...)
```

**关键技术**：

+   Redis Sorted Set：`ZADD` `user:u001:clicks` {时间戳} {商品ID}，支持滑动窗口查询

+   **RFM 模型**：Recency（最近购买时间）× Frequency（购买频率）× Monetary（消费金额）

+   **用户分群**：新客 / VIP / 价格敏感 / 活跃 / 流失风险，共 5 类

![](img/5116b26abe03b6a5b317ec9554d6e8d9.png)

---

### Agent 2：商品推荐 Agent

**它做什么？**

两阶段推荐：先"召回"大量候选商品，再用 LLM 精排出最合适的 TopN。

```
多路召回策略
  ├── 协同过滤（买了A也买了B）
  ├── 向量检索（Milvus，语义相似商品）
  ├── 热度策略（最近7天热卖）
  └── 新品策略（上架30天内）
        │
        ▼（去重合并，候选集）
  LLM 精排
  │ Prompt: "用户是价格敏感型，偏好手机配件，以下10件商品请排序..."
  │ 输出: 按相关性从高到低排列的商品 ID 列表
        │
        ▼
  TopN 商品列表（交给库存 Agent 过滤）
```

![](img/222584863a8f2f6bd975f387e4c9fa7a.png)

### Agent 3：营销文案 Agent

**它做什么？**

根据用户画像自动选择合适的文案风格模板，调用 LLM 生成个性化文案，并做广告法合规校验。

```
# 5套模板 × 用户分群
TEMPLATES = {
    "new_user":        "首单专属福利，{product}立减{discount}元！",
    "vip":             "尊享会员特权，{product}专属价{price}，品质之选。",
    "price_sensitive": "今日限时抢购！{product}历史最低价，仅剩{stock}件！",
    "active":          "根据您的浏览偏好，为您精选 {product}，好评率{rating}%",
    "churn_risk":      "好久不见！{product}为您专属保留，点击领取优惠券",
}

# 广告法合规校验（过滤违禁词）
BANNED_WORDS = ["最好", "第一", "最便宜", "绝对", "100%"]
```

![](img/9b2a49b1f001f17265a8e3ab33b1b692.png)

### Agent 4：库存决策 Agent

**它做什么？**

查询商品实时库存，过滤缺货商品，输出限购策略和补货预警。

```
# 输入: 推荐商品列表 [P001, P002, P003, ...]
# 查询 MySQL/WMS 实时库存
# 输出:
{
    "available_products": ["P001", "P003"],   # 有货商品
    "inventory_alerts": [                      # 库存预警
        {"product_id": "P001", "stock": 5, "warning": "库存紧张"}
    ],
    "purchase_limits": {                       # 限购策略
        "P001": 2  # 每人最多买2件
    }
}
```

![](img/a46c6b19864b1ad7f3185bb460af0d4e.png)

## 🌐 三语言实现对比

---

## 💻 关键代码展示

### Supervisor 并行编排（Python 核心代码）
