
```
import json
from datetime import datetime
from typing import Dict, List, Optional
import hashlib

class ContentProvenance:
    """内容溯源系统"""
    
    def __init__(self):
        self.provenance_db = []
    
    def record_generation(
        self,
        content: str,
        model: str,
        prompt: str,
        parameters: Dict,
        user_id: str,
        session_id: str,
        metadata: Optional[Dict] = None
    ) -> str:
        """
        记录内容生成过程
        
        Returns:
            content_id: 内容唯一标识符
        """
        content_id = self._generate_content_id(content, prompt, model)
        
        provenance_record = {
            'content_id': content_id,
            'content_hash': hashlib.sha256(content.encode()).hexdigest(),
            'model': model,
            'prompt': prompt,
            'parameters': parameters,
            'user_id': user_id,
            'session_id': session_id,
            'timestamp': datetime.now().isoformat(),
            'metadata': metadata or {},
            'version': '1.0'
        }
        
        self.provenance_db.append(provenance_record)
        
        return content_id
    
    def _generate_content_id(self, content: str, prompt: str, model: str) -> str:
        """生成内容 ID"""
        combined = f"{content}{prompt}{model}{datetime.now().isoformat()}"
        return hashlib.sha256(combined.encode()).hexdigest()[:16]
    
    def get_provenance(self, content_id: str) -> Optional[Dict]:
        """获取内容溯源信息"""
        for record in self.provenance_db:
            if record['content_id'] == content_id:
                return record
        return None
    
    def audit_user_generations(
        self,
        user_id: str,
        start_date: Optional[datetime] = None,
        end_date: Optional[datetime] = None
    ) -> List[Dict]:
        """审计用户的生成历史"""
        user_records = [
            record for record in self.provenance_db
            if record['user_id'] == user_id
        ]
        
        if start_date:
            user_records = [
                r for r in user_records
                if datetime.fromisoformat(r['timestamp']) >= start_date
            ]
        
        if end_date:
            user_records = [
                r for r in user_records
                if datetime.fromisoformat(r['timestamp']) <= end_date
            ]
        
        return user_records
    
    def export_audit_log(self, filepath: str) -> None:
        """导出审计日志"""
        with open(filepath, 'w', encoding='utf-8') as f:
            json.dump(self.provenance_db, f, indent=2, ensure_ascii=False)
        print(f"审计日志已导出到: {filepath}")

# 使用示例
provenance = ContentProvenance()

# 记录生成
content_id = provenance.record_generation(
    content="OpenClaw 是一个强大的 AI Agent 框架...",
    model="claude-4-sonnet",
    prompt="介绍 OpenClaw",
    parameters={'temperature': 0.7, 'max_tokens': 1000},
    user_id="user_12345",
    session_id="session_abc",
    metadata={'purpose': 'documentation', 'reviewed': False}
)

print(f"内容已记录,ID: {content_id}")

# 获取溯源信息
prov_info = provenance.get_provenance(content_id)
print(f"
溯源信息:")
print(json.dumps(prov_info, indent=2, ensure_ascii=False))

# 审计用户生成历史
user_audit = provenance.audit_user_generations("user_12345")
print(f"
用户 user_12345 共生成 {len(user_audit)} 条内容")
```

### 14.4.3 AI 生成内容披露

[根据最佳实践，应添加](https://medium.com/@digitalblueprint/ai-generated-content-risks-ethics-and-best-practices-35c6b0e4293c)明确的披露。如果内容将影响决策（金融、医疗、法律），应寻求人类专家审查。citation

```
class AIContentDisclosure:
    """AI 生成内容披露系统"""
    
    DISCLOSURE_TEMPLATES = {
        'full_ai': "⚠️ 本内容完全由 AI 生成,未经人工审查。请谨慎使用。",
        'ai_assisted': "ℹ️ 本内容由 AI 辅助生成,已经过人工审查和编辑。",
        'ai_draft': "📝 本内容为 AI 生成的初稿,正在等待专家审查。",
        'high_risk': "🚨 本内容涉及 {domain} 领域,已由专业人士审查。AI 生成内容仅供参考,不构成专业建议。",
    }
    
    @classmethod
    def add_disclosure(
        cls,
        content: str,
        disclosure_type: str,
        model: str,
        reviewed: bool = False,
        domain: Optional[str] = None
    ) -> str:
        """为内容添加 AI 披露"""
        
        # 选择披露模板
        if domain in ['medical', 'legal', 'financial']:
            template = cls.DISCLOSURE_TEMPLATES['high_risk'].format(domain=domain)
        elif reviewed:
            template = cls.DISCLOSURE_TEMPLATES['ai_assisted']
        elif disclosure_type == 'draft':
            template = cls.DISCLOSURE_TEMPLATES['ai_draft']
        else:
            template = cls.DISCLOSURE_TEMPLATES['full_ai']
        
        # 添加模型信息
        model_info = f"

---
{template}
生成模型: {model}"
        
        return content + model_info
    
    @classmethod
    def create_watermark(cls, content: str, metadata: Dict) -> str:
        """
        创建数字水印
        
        使用隐写术技术在内容中嵌入元数据
        """
        # 简化实现:在内容末尾添加隐藏的元数据
        watermark = f"

<!-- AI_METADATA: {json.dumps(metadata)} -->"
        return content + watermark

# 使用示例
disclosure = AIContentDisclosure()

ai_content = "OpenClaw 是一个开源 AI Agent 框架..."

# 添加披露
disclosed_content = disclosure.add_disclosure(
    content=ai_content,
    disclosure_type='full_ai',
    model='claude-4-sonnet',
    reviewed=False
)

print(disclosed_content)

# 高风险内容披露
medical_content = "建议患者服用..."
disclosed_medical = disclosure.add_disclosure(
    content=medical_content,
    disclosure_type='high_risk',
    model='gpt-4',
    reviewed=True,
    domain='medical'
)

print(f"
{disclosed_medical}")
```

---

## 14.5 合规性和监管要求

### 14.5.1 AI 内容监管框架（2026）

**关键监管要求：**

根据 2025-2026 年的监管发展，AI 内容安全面临以下合规要求：

1.  NIST AI 风险管理框架（AI RMF）

    +   描述和讨论“安全和韧性”AI
    +   包括 Measure 功能中的安全评估子类别
    +   [要求实施 prompt injection 预防](https://www.obsidiansecurity.com/blog/prompt-injection)和检测的特定控制 citation

2.  ISO 42001(AI 管理系统）

    +   要求对 prompt injection 风险进行特定控制
    +   强制实施身份和访问控制，扩展到 AI agents
    +   包括令牌管理和动态授权策略 [citation](https://www.obsidiansecurity.com/blog/prompt-injection)

3.  **GDPR（针对 AI 系统）**

    +   处理个人数据的 AI 系统必须实施“适当的技术措施”防止通过 prompt injection 未经授权访问
    +   [要求数据来源追踪](https://www.obsidiansecurity.com/blog/prompt-injection)和标记未验证数据源 citation

4.  **HIPAA（医疗领域）**

    +   访问 PHI（受保护健康信息）的 AI agents 需要技术保障措施（§164.312）
    +   包括访问控制和审计日志，必须考虑 [prompt injection 风险](https://www.obsidiansecurity.com/blog/prompt-injection) citation

### 14.5.2 合规检查清单
