
```
from enum import Enum
from dataclasses import dataclass
from typing import List, Dict
import json

class ComplianceFramework(Enum):
    """合规框架"""
    NIST_AI_RMF = "NIST AI Risk Management Framework"
    ISO_42001 = "ISO 42001"
    GDPR = "GDPR"
    HIPAA = "HIPAA"
    CCPA = "CCPA"
    SOC2 = "SOC 2"

@dataclass
class ComplianceRequirement:
    """合规要求"""
    framework: ComplianceFramework
    requirement_id: str
    description: str
    implemented: bool = False
    evidence: str = ""
    notes: str = ""

class ComplianceChecker:
    """合规检查器"""
    
    def __init__(self):
        self.requirements = self._initialize_requirements()
    
    def _initialize_requirements(self) -> List[ComplianceRequirement]:
        """初始化合规要求清单"""
        return [
            # NIST AI RMF
            ComplianceRequirement(
                framework=ComplianceFramework.NIST_AI_RMF,
                requirement_id="GOVERN-1.1",
                description="建立 AI 风险管理政策和程序"
            ),
            ComplianceRequirement(
                framework=ComplianceFramework.NIST_AI_RMF,
                requirement_id="MAP-2.1",
                description="识别和文档化 AI 系统风险,包括 prompt injection"
            ),
            ComplianceRequirement(
                framework=ComplianceFramework.NIST_AI_RMF,
                requirement_id="MEASURE-2.3",
                description="实施 AI 系统行为的持续监控"
            ),
            ComplianceRequirement(
                framework=ComplianceFramework.NIST_AI_RMF,
                requirement_id="MANAGE-1.1",
                description="实施 prompt injection 预防和检测控制"
            ),
            
            # ISO 42001
            ComplianceRequirement(
                framework=ComplianceFramework.ISO_42001,
                requirement_id="ISO-6.1",
                description="AI 系统的风险评估和处理"
            ),
            ComplianceRequirement(
                framework=ComplianceFramework.ISO_42001,
                requirement_id="ISO-7.2",
                description="AI 系统的访问控制和身份验证"
            ),
            ComplianceRequirement(
                framework=ComplianceFramework.ISO_42001,
                requirement_id="ISO-8.1",
                description="AI 模型训练数据的安全管理"
            ),
            
            # GDPR
            ComplianceRequirement(
                framework=ComplianceFramework.GDPR,
                requirement_id="GDPR-Art32",
                description="实施适当的技术和组织措施确保安全处理个人数据"
            ),
            ComplianceRequirement(
                framework=ComplianceFramework.GDPR,
                requirement_id="GDPR-Art25",
                description="数据保护的设计和默认(Privacy by Design)"
            ),
            ComplianceRequirement(
                framework=ComplianceFramework.GDPR,
                requirement_id="GDPR-Art35",
                description="对高风险 AI 系统进行数据保护影响评估(DPIA)"
            ),
            
            # HIPAA
            ComplianceRequirement(
                framework=ComplianceFramework.HIPAA,
                requirement_id="HIPAA-164.308",
                description="实施管理保障措施保护 ePHI"
            ),
            ComplianceRequirement(
                framework=ComplianceFramework.HIPAA,
                requirement_id="HIPAA-164.312",
                description="实施技术保障措施,包括访问控制和审计日志"
            ),
            ComplianceRequirement(
                framework=ComplianceFramework.HIPAA,
                requirement_id="HIPAA-164.316",
                description="维护 AI 系统访问 PHI 的文档和记录"
            ),
            
            # SOC 2
            ComplianceRequirement(
                framework=ComplianceFramework.SOC2,
                requirement_id="CC6.1",
                description="实施逻辑和物理访问控制"
            ),
            ComplianceRequirement(
                framework=ComplianceFramework.SOC2,
                requirement_id="CC7.2",
                description="检测和监控安全事件"
            ),
        ]
    
    def check_compliance(self, framework: Optional[ComplianceFramework] = None) -> Dict:
        """检查合规状态"""
        requirements = self.requirements
        if framework:
            requirements = [r for r in requirements if r.framework == framework]
        
        total = len(requirements)
        implemented = sum(1 for r in requirements if r.implemented)
        compliance_rate = (implemented / total * 100) if total > 0 else 0
        
        return {
            'framework': framework.value if framework else 'All',
            'total_requirements': total,
            'implemented': implemented,
            'pending': total - implemented,
            'compliance_rate': compliance_rate,
            'status': 'COMPLIANT' if compliance_rate == 100 else 'NON_COMPLIANT',
            'requirements': requirements
        }
    
    def mark_implemented(
        self,
        requirement_id: str,
        evidence: str,
        notes: str = ""
    ) -> bool:
        """标记要求为已实施"""
        for req in self.requirements:
            if req.requirement_id == requirement_id:
                req.implemented = True
                req.evidence = evidence
                req.notes = notes
                return True
        return False
    
    def generate_compliance_report(self, filepath: str) -> None:
        """生成合规报告"""
        report = {
            'generated_at': datetime.now().isoformat(),
            'frameworks': {}
        }
        
        for framework in ComplianceFramework:
            compliance_status = self.check_compliance(framework)
            report['frameworks'][framework.value] = compliance_status
        
        with open(filepath, 'w', encoding='utf-8') as f:
            json.dump(report, f, indent=2, ensure_ascii=False, default=str)
        
        print(f"合规报告已生成: {filepath}")
    
    def get_pending_requirements(self) -> List[ComplianceRequirement]:
        """获取待实施的要求"""
        return [r for r in self.requirements if not r.implemented]

# 使用示例
checker = ComplianceChecker()

# 标记一些要求为已实施
checker.mark_implemented(
    requirement_id="NIST-MANAGE-1.1",
    evidence="已实施 InputValidator 和 AgentBehaviorMonitor",
    notes="使用多层验证和实时监控"
)

checker.mark_implemented(
    requirement_id="GDPR-Art32",
    evidence="已实施加密、访问控制和审计日志",
    notes="符合 GDPR 第32条要求"
)

# 检查整体合规状态
overall_status = checker.check_compliance()
print(f"整体合规率: {overall_status['compliance_rate']:.1f}%")
print(f"已实施: {overall_status['implemented']}/{overall_status['total_requirements']}")
print(f"状态: {overall_status['status']}")

# 检查特定框架
nist_status = checker.check_compliance(ComplianceFramework.NIST_AI_RMF)
print(f"
NIST AI RMF 合规率: {nist_status['compliance_rate']:.1f}%")

# 获取待办事项
pending = checker.get_pending_requirements()
print(f"
待实施要求({len(pending)}项):")
for req in pending[:5]:  # 显示前5项
    print(f"  - [{req.framework.value}] {req.requirement_id}: {req.description}")

# 生成报告
checker.generate_compliance_report("compliance_report_2026.json")
```

---

## 14.6 事件响应和危机管理

### 14.6.1 内容安全事件分类
