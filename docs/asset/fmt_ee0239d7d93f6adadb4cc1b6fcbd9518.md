
```
class IncidentResponsePlan:
    def __init__(self, key_manager, notification_service):
        self.key_manager = key_manager
        self.notification_service = notification_service
    
    def execute_response(self, compromised_key_id, incident_details):
        """执行应急响应计划"""
        print("🚨 启动应急响应流程")
        
        # 步骤 1: 立即撤销密钥
        print("步骤 1: 撤销泄露的密钥")
        self.key_manager.revoke_key(compromised_key_id)
        
        # 步骤 2: 生成新密钥
        print("步骤 2: 生成新密钥")
        key_info = self.key_manager.keys[compromised_key_id]
        new_key_id, new_key_value = self.key_manager.create_key(
            key_info['service'],
            key_info['user_id']
        )
        
        # 步骤 3: 通知相关人员
        print("步骤 3: 通知相关人员")
        self.notification_service.send_alert(
            severity="critical",
            title="API 密钥泄露事件",
            message=f"密钥 {compromised_key_id} 已泄露并被撤销。新密钥: {new_key_id}",
            details=incident_details
        )
        
        # 步骤 4: 审查访问日志
        print("步骤 4: 审查访问日志")
        suspicious_activities = self._review_access_logs(compromised_key_id)
        
        # 步骤 5: 评估影响
        print("步骤 5: 评估影响")
        impact_assessment = self._assess_impact(
            compromised_key_id,
            suspicious_activities
        )
        
        # 步骤 6: 更新应用程序
        print("步骤 6: 更新应用程序配置")
        # 这通常需要手动操作
        print(f"  新密钥: {new_key_value}")
        print("  请立即更新所有使用该密钥的应用程序")
        
        # 步骤 7: 生成事件报告
        print("步骤 7: 生成事件报告")
        report = self._generate_incident_report(
            compromised_key_id,
            new_key_id,
            incident_details,
            impact_assessment
        )
        
        return report
    
    def _review_access_logs(self, key_id):
        """审查访问日志"""
        # 实际实现中,这里会查询日志系统
        return []
    
    def _assess_impact(self, key_id, suspicious_activities):
        """评估影响"""
        return {
            'data_accessed': False,
            'unauthorized_operations': len(suspicious_activities),
            'estimated_cost': 0.0
        }
    
    def _generate_incident_report(self, old_key_id, new_key_id, 
                                  incident_details, impact_assessment):
        """生成事件报告"""
        from datetime import datetime
        
        report = {
            'incident_id': f"INC-{datetime.utcnow().strftime('%Y%m%d%H%M%S')}",
            'timestamp': datetime.utcnow().isoformat(),
            'type': 'api_key_compromise',
            'severity': 'critical',
            'compromised_key': old_key_id,
            'new_key': new_key_id,
            'incident_details': incident_details,
            'impact_assessment': impact_assessment,
            'actions_taken': [
                'Revoked compromised key',
                'Generated new key',
                'Notified security team',
                'Reviewed access logs',
                'Assessed impact'
            ],
            'recommendations': [
                '立即更新所有应用程序配置',
                '审查代码库,确保没有硬编码的密钥',
                '实施密钥扫描工具',
                '加强访问控制',
                '定期进行安全审计'
            ]
        }
        
        return report

# 使用示例
# incident_response = IncidentResponsePlan(key_manager, notification_service)
# report = incident_response.execute_response(
#     compromised_key_id="openai_user123_abc123",
#     incident_details={
#         'source': 'GitHub public repository',
#         'discovered_by': 'automated scan',
#         'exposure_duration': 'unknown'
#     }
# )
```

**预防措施清单**

```
# API 密钥泄露预防清单

## 代码层面
- [ ] 使用 .gitignore 排除 .env 文件
- [ ] 使用密钥管理服务 (AWS Secrets Manager, Vault)
- [ ] 实施 pre-commit 钩子扫描密钥
- [ ] 定期审查代码库

## 存储层面
- [ ] 加密存储所有密钥
- [ ] 设置严格的文件权限 (chmod 600)
- [ ] 使用专用的密钥存储目录
- [ ] 启用磁盘加密

## 访问控制
- [ ] 实施最小权限原则
- [ ] 使用短期令牌而非长期密钥
- [ ] 启用多因素认证
- [ ] 定期审查访问权限

## 监控和审计
- [ ] 启用 API 使用监控
- [ ] 设置异常使用告警
- [ ] 记录所有密钥访问
- [ ] 定期审查审计日志

## 轮换和更新
- [ ] 制定密钥轮换计划 (90 天)
- [ ] 自动化轮换流程
- [ ] 测试轮换流程
- [ ] 文档化轮换步骤

## 应急响应
- [ ] 制定泄露响应计划
- [ ] 定期演练应急流程
- [ ] 准备通知模板
- [ ] 建立升级机制
```

## 13.5 合规性指南

### 13.5.1 GDPR 合规要点

GDPR 是 AI 系统必须遵守的最重要法规之一。[2026](https://secureprivacy.ai/blog/gdpr-compliance-2026) 年的执法力度显著加强。citation

**核心原则实施**
