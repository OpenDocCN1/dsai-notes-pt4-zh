
```
class SpecializedRouter:
    def __init__(self):
        self.task_model_mapping = {
            "code": "claude-sonnet-4-6",  # Sonnet 编码能力强
            "chat": "claude-haiku-4-5",   # Haiku 适合对话
            "analysis": "claude-sonnet-4-6",  # Sonnet 推理能力强
            "translation": "claude-haiku-4-5",  # 翻译用 Haiku 足够
            "creative": "claude-sonnet-4-6",  # 创意内容用 Sonnet
            "summary": "claude-haiku-4-5",  # 摘要用 Haiku
            "computer_use": "claude-sonnet-4-6",  # 计算机使用用 Sonnet
            "office": "claude-sonnet-4-6",  # 办公任务用 Sonnet（超越 Opus）
        }
    
    def detect_task_type(self, task):
        """检测任务类型"""
        keywords = {
            "code": ["代码", "函数", "程序", "bug", "debug", "实现"],
            "chat": ["你好", "谢谢", "怎么样", "聊天"],
            "analysis": ["分析", "评估", "比较", "研究"],
            "translation": ["翻译", "translate", "英文", "中文"],
            "creative": ["写", "创作", "故事", "文章", "诗"],
            "summary": ["总结", "摘要", "概括"],
            "computer_use": ["自动化", "桌面", "操作", "点击"],
            "office": ["Excel", "表格", "文档", "办公"],
        }
        
        for task_type, task_keywords in keywords.items():
            if any(keyword in task for keyword in task_keywords):
                return task_type
        
        return "chat"  # 默认为对话
    
    def route(self, task):
        """路由到专用模型"""
        task_type = self.detect_task_type(task)
        model = self.task_model_mapping[task_type]
        
        print(f"Task type: {task_type}, using model: {model}")
        
        return client.messages.create(
            model=model,
            max_tokens=1024,
            messages=[{"role": "user", "content": task}]
        )

# 使用示例
router = SpecializedRouter()

# 代码任务 → Sonnet
router.route("写一个快速排序算法")

# 翻译任务 → Haiku
router.route("把这段话翻译成英文：今天天气很好")

# 办公任务 → Sonnet（在办公生产力上超越 Opus）
router.route("帮我分析这个 Excel 表格")
```

### 模式 4: 成本优先的降级策略

优先使用便宜模型，失败时自动升级：

```
class CostOptimizedRouter:
    def __init__(self):
        self.model_hierarchy = [
            "claude-haiku-4-5",
            "claude-sonnet-4-6",
            "claude-opus-4-6"
        ]
    
    def call_with_fallback(self, task, quality_threshold=0.7):
        """从最便宜的模型开始尝试，不满足质量要求则升级"""
        
        for model in self.model_hierarchy:
            print(f"Trying model: {model}")
            
            response = client.messages.create(
                model=model,
                max_tokens=1024,
                messages=[{"role": "user", "content": task}]
            )
            
            content = response.content[0].text
            
            # 质量评估（简化版）
            quality_score = self.assess_quality(task, content)
            
            print(f"Quality score: {quality_score}")
            
            if quality_score >= quality_threshold:
                print(f"Quality threshold met with {model}")
                return content
            else:
                print(f"Quality insufficient, trying next model...")
        
        # 如果所有模型都不满足，返回最好的结果
        return content
    
    def assess_quality(self, task, response):
        """评估响应质量（简化实现）"""
        # 实际应用中可以使用更复杂的评估逻辑
        # 例如：长度检查、关键词匹配、格式验证等
        
        if len(response) < 50:
            return 0.3  # 太短
        elif "抱歉" in response or "无法" in response:
            return 0.5  # 拒绝回答
        else:
            return 0.8  # 看起来正常

# 使用示例
router = CostOptimizedRouter()

# 大多数情况下会用 Haiku 完成，节省成本
result = router.call_with_fallback("什么是 Python？")

# 复杂任务可能会自动升级到 Sonnet 或 Opus
complex_result = router.call_with_fallback(
    "设计一个分布式系统的架构，包括负载均衡、数据一致性和容错机制"
)
```

**混合使用的最佳实践**

1.  **明确任务分类标准**： 建立清晰的规则来判断任务复杂度

1.  **监控模型性能**： 跟踪每个模型在不同任务上的表现

1.  **动态调整策略**： 根据实际效果优化路由逻辑

1.  **成本效益分析**： 定期评估混合策略的成本节省效果

1.  **用户体验优先**： 不要为了省钱而牺牲用户体验

**实际案例：智能客服系统**

```
class SmartCustomerService:
    def __init__(self):
        self.faq_model = "claude-haiku-4-5"
        self.technical_model = "claude-sonnet-4-6"
        self.escalation_model = "claude-opus-4-6"
    
    def handle_query(self, query, user_tier="free"):
        """根据查询类型和用户等级选择模型"""
        
        # 1. 快速分类
        query_type = self.classify_query(query)
        
        # 2. 根据类型和用户等级选择模型
        if query_type == "faq":
            model = self.faq_model
        elif query_type == "technical" and user_tier == "premium":
            model = self.technical_model
        elif query_type == "complaint" and user_tier == "premium":
            model = self.escalation_model
        else:
            model = self.faq_model  # 默认
        
        # 3. 调用模型
        response = self.call_model(model, query, query_type)
        
        return response
    
    def classify_query(self, query):
        """分类查询类型"""
        if any(word in query for word in ["退款", "投诉", "不满意"]):
            return "complaint"
        elif any(word in query for word in ["技术", "bug", "错误", "无法使用"]):
            return "technical"
        else:
            return "faq"
    
    def call_model(self, model, query, query_type):
        """调用相应模型"""
        system_prompts = {
            "faq": "你是一个客服助手，简洁友好地回答常见问题。",
            "technical": "你是一个技术支持专家，详细分析并解决技术问题。",
            "complaint": "你是一个高级客服经理，以同理心处理客户投诉，提供最佳解决方案。"
        }
        
        return client.messages.create(
            model=model,
            max_tokens=512,
            system=system_prompts[query_type],
            messages=[{"role": "user", "content": query}]
        )

# 使用示例
service = SmartCustomerService()

# FAQ → Haiku
service.handle_query("你们的营业时间是？", user_tier="free")

# 技术问题 + 付费用户 → Sonnet
service.handle_query("我的账号无法登录，显示错误代码 500", user_tier="premium")

# 投诉 + 付费用户 → Opus
service.handle_query("我要投诉，你们的服务太差了！", user_tier="premium")
```

**成本节省效果**

假设每天处理 10,000 个客服查询：

+   70% FAQ（Haiku）: 7,000 × $0.0045 = $31.50

+   25% 技术问题（Sonnet）: 2,500 × $0.0135 = $33.75

+   5% 投诉（Opus）: 500 × $0.0225 = $11.25

**总成本**： $76.50/天 = $2,295/月

如果全部使用 Sonnet: $0.0135 × 10,000 × 30 = $4,050/月

**节省**： 43%

如果全部使用 Opus: $0.0225 × 10,000 × 30 = $6,750/月

**节省**： 66%

**总结**： 多模型混合使用是一种强大的优化策略，可以在保证质量的前提下显著降低成本。关键是建立合理的任务分类和路由机制，让每个模型都在最适合的场景中发挥作用。

## 5.5 模型参数调优

模型参数是控制 Claude 输出行为的关键工具。合理调整参数可以让模型生成更符合需求的内容，无论是追求创意性、准确性还是一致性。本节将详细介绍三个最重要的参数：`Temperature`、`Max Tokens` 和 `Top P`。

### 5.5.1 `Temperature` 温度参数

`Temperature` 是控制模型输出随机性的核心参数，直接影响生成内容的创意性和确定性。

**参数定义**

`Temperature` 控制模型在选择下一个 token 时的随机性程度：

+   **取值范围**： 0.0 - 1.0

+   **默认值**： 1.0

+   **作用机制**： 通过调整 token 概率分布的“陡峭程度”来影响选择

**工作原理**

当模型预测下一个 token 时，会为所有可能的 token 计算概率。`Temperature` 通过以下方式影响这些概率：

+   **`Temperature = 0`**: 总是选择概率最高的 token（完全确定性）
