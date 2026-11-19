
# Agent智能代理开发与部署

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BzBpddSngoHltTxArOFcRQo1njh](https://v11enp9ok1h.feishu.cn/docx/BzBpddSngoHltTxArOFcRQo1njh)

## Agent智能代理基础理论

### 1.1 Agent核心概念与架构

#### 1.1.1 什么是Agent

`Agent（智能代理）` 是一个能够感知环境、制定计划并执行行动的智能实体。与传统程序不同，`Agent`具备以下特征：

| 特征 | 传统程序 | Agent智能代理 |
| --- | --- | --- |
| 自主性 | 被动响应用户输入 | 主动感知环境变化，自主决策 |
| 适应性 | 固定的执行逻辑 | 根据情况调整策略和行为 |
| 目标导向 | 执行预定义任务 | 面向目标，动态规划路径 |
| 交互性 | 单向或简单交互 | 与环境、用户、其他`Agent`协作 |

`Agent`的核心组件：

1. 感知器(`Perception`): 获取环境信息和用户输入
1. 规划器(`Planning`): 制定达成目标的行动计划
1. 执行器(`Action`): 调用工具执行具体操作
1. 记忆系统(`Memory`): 维护上下文和历史信息
1. 学习模块(`Learning`): 从经验中优化决策

![](img/1c500458ad8ad2836720a3af8b40bd49.png)

#### 1.1.2 基础Agent实现

```
import os
import json
from openai import OpenAI
from typing import List, Dict, Any
import asyncio
from datetime import datetime

class AgentCore:
    def __init__(self, model_name="ernie-4.5-turbo-128k-preview"):
        self.client = OpenAI(
            api_key=os.environ.get("**********************************************"),
            base_url="https://aistudio.baidu.com/llm/lmapi/v3"
        )
        self.model_name = model_name
        self.memory = []
        self.tools = {}
        
    def perceive(self, environment_data: Dict[str, Any]) -> Dict[str, Any]:
        """感知环境信息"""
        perception = {
            "timestamp": datetime.now().isoformat(),
            "environment": environment_data,
            "context": self.memory[-5:] if self.memory else []
        }
        return perception
    
    def reason(self, perception: Dict[str, Any], goal: str) -> Dict[str, Any]:
        """推理决策过程"""
        messages = [
            {
                "role": "system",
                "content": """你是一个智能代理，需要根据感知信息和目标进行推理决策。
                可用工具: {list(self.tools.keys())}
                目标: {goal}"""
            },
            {
                "role": "user", 
                "content": f"当前感知信息: {json.dumps(perception, ensure_ascii=False)}"
            }
        ]
        
        response = self.client.chat.completions.create(
            model=self.model_name,
            messages=messages,
            temperature=0.7
        )
        
        decision = {
            "reasoning": response.choices[0].message.content,
            "perception": perception,
            "goal": goal
        }
        return decision
        
    def act(self, decision: Dict[str, Any]) -> Dict[str, Any]:
        """执行行动"""
        action_result = {
            "decision": decision,
            "executed": True,
            "timestamp": datetime.now().isoformat(),
            "result": "Action completed successfully"
        }
        
        self.memory.append(action_result)
        return action_result

agent = AgentCore()
print("智能代理核心组件初始化完成")
```

### 1.2 Agent与传统程序的区别

#### 1.2.1 决策机制对比

传统程序遵循固定的`if-else`逻辑或预定义的工作流，而`Agent`具备动态决策能力。这种差异体现在以下几个方面：

1. 响应模式
+ 传统程序: 被动响应，等待明确指令
+ `Agent`: 主动感知，基于环境变化自主决策

1. 处理不确定性
+ 传统程序: 需要明确的输入格式和处理逻辑
+ `Agent`: 能够处理模糊需求，动态调整策略

1. 学习能力
+ 传统程序: 功能固定，需要手动更新
+ `Agent`: 从经验中学习，持续优化决策

```
class HybridAgent(AgentCore):def __init__(self, model_name="ernie-4.5-turbo-128k-preview"):super().__init__(model_name)
    self.reactive_layer = {}
    self.deliberative_layer = {}
    self.coordination_layer = {}

def process_request(self, request: str, context: Dict = None) -> Dict[str, Any]:if self._is_simple_request(request):
    return self._reactive_response(request)

return self._deliberative_response(request, context)

def _is_simple_request(self, request: str) -> bool:
    simple_patterns = ["你好", "时间", "天气", "计算"]
    return any(pattern in request for pattern in simple_patterns)

def _reactive_response(self, request: str) -> Dict[str, Any]:
    response_map = {
        "你好": "您好！我是您的智能助手，有什么可以帮助您的吗？",
        "时间": f"当前时间是 {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}",
    }
    
    for key, value in response_map.items():
        if key in request:
            return {
                "type": "reactive",
                "response": value,
                "processing_time": "< 1ms"
            }
    
    return self._deliberative_response(request)

def _deliberative_response(self, request: str, context: Dict = None) -> Dict[str, Any]:
    messages = [
        {
            "role": "system",
            "content": "你是一个智能代理，需要仔细分析用户请求并提供详细的解决方案。"
        },
        {
            "role": "user",
            "content": request
        }
    ]
    
    if context:
        messages.insert(1, {
            "role": "system", 
            "content": f"上下文信息: {json.dumps(context, ensure_ascii=False)}"
        })
    
    response = self.client.chat.completions.create(
        model=self.model_name,
        messages=messages,
        temperature=0.3
    )
    
    return {
        "type": "deliberative",
        "response": response.choices[0].message.content,
        "processing_time": "> 100ms"
    }

hybrid_agent = HybridAgent()

reactive_result = hybrid_agent.process_request("你好")
print("反应式响应:", reactive_result)

deliberative_result = hybrid_agent.process_request("如何优化深度学习模型的训练效率？")
print("深思式响应:", deliberative_result)
反应式响应: {'type': 'reactive', 'response': '您好！我是您的智能助手，有什么可以帮助您的吗？', 'processing_time': '< 1ms'}
深思式响应: {'type': 'deliberative', 'response': '优化深度学习模型的训练效率是一个多方面的任务，涉及数据处理、模型架构、超参数调整和硬件利用等多个方面。以下是一些关键策略和方法：\n\n1. **数据预处理和增强**：\n   - **数据标准化/归一化**：对输入数据进行标准化或归一化处理，使不同特征具有相似的尺度，有助于模型更快收敛。\n   - **数据增强**：通过对训练数据进行旋转、平移、缩放等变换，增加数据的多样性，提高模型的泛化能力。\n   - **高效数据加载**：使用数据加载器（如PyTorch的`DataLoader`）进行多线程/多进程数据加载，减少I/O瓶颈。\n\n2. **模型架构优化**：\n   - **选择合适的模型**：根据任务需求选择合适的模型架构，避免过于复杂的模型。\n   - **模型剪枝**：移除模型中不重要的权重或神经元，减少计算量和参数量。\n   - **知识蒸馏**：使用一个较大的“教师”模型来指导一个较小的“学生”模型的训练，提高小模型的性能。\n   - **量化**：将模型中的浮点数参数转换为低精度的整数，减少内存占用和计算量。\n\n3. **超参数优化**：\n   - **学习率调整**：使用学习率调度器（如`ReduceLROnPlateau`、`CosineAnnealingLR`）动态调整学习率。\n   - **批量大小（Batch Size）**：适当调整批量大小，较大的批量可以加速训练，但可能影响模型泛化能力。\n   - **优化器选择**：选择合适的优化器（如Adam、SGD、RMSprop），并根据任务调整其参数。\n\n4. **正则化和防止过拟合**：\n   - **Dropout**：在训练过程中随机丢弃一部分神经元，防止过拟合。\n   - **L1/L2正则化**：在损失函数中添加权重的L1或L2正则化项，防止模型过拟合。\n   - **早停（Early Stopping）**：在验证集性能不再提升时停止训练，防止过拟合。\n\n5. **分布式训练**：\n   - **数据并行**：将数据分成多个批次，同时在多个设备上训练同一个模型。\n   - **模型并行**：将模型的不同部分分配到不同的设备上，适用于非常大的模型。\n
```

### 1.3 Agent在行业应用中的作用

#### 1.3.1 行业应用场景分析

`Agent`智能代理在不同行业中发挥着重要作用，能够显著提升工作效率和决策质量：

| 行业 | 应用场景 | Agent角色 | 价值体现 |
| --- | --- | --- | --- |
| 金融 | 风险评估、投资建议 | 智能投顾Agent | 7×24小时监控，实时风险预警 |
| 医疗 | 诊断辅助、药物研发 | 医疗助手Agent | 大数据分析，精准诊断建议 |
| 制造 | 生产优化、质量控制 | 工业Agent | 预测性维护，智能调度 |
| 教育 | 个性化学习、作业批改 | 教学Agent | 因材施教，学习效果跟踪 |
| 客服 | 智能回答、问题解决 | 客服Agent | 多轮对话，情感理解 |

#### 1.3.2 行业Agent系统实现



# 任务与对话Agent实现

## TaskAgent 类

```python
class TaskAgent:
    def __init__(self):
        self.client = OpenAI(
            api_key=os.environ.get("********************************"),
            base_url="https://aistudio.baidu.com/llm/lmapi/v3"
        )
        self.task_pipeline = []
        self.business_rules = {}

    def define_task(self, task_name: str, steps: List[str], rules: Dict[str, Any]):
        self.business_rules[task_name] = {
            "steps": steps,
            "rules": rules,
            "created_at": datetime.now().isoformat()
        }

    def execute_task(self, task_name: str, parameters: Dict[str, Any]) -> Dict[str, Any]:
        if task_name not in self.business_rules:
            return {"error": f"未找到任务: {task_name}"}
        
        task_config = self.business_rules[task_name]
        results = []
        
        for step in task_config["steps"]:
            step_result = self._execute_step(step, parameters, task_config["rules"])
            results.append(step_result)
            
            if not step_result.get("success", False):
                return {
                    "task": task_name,
                    "status": "failed",
                    "failed_step": step,
                    "results": results
                }
        
        return {
            "task": task_name,
            "status": "completed",
            "results": results,
            "execution_time": datetime.now().isoformat()
        }

    def _execute_step(self, step: str, parameters: Dict[str, Any], rules: Dict[str, Any]) -> Dict[str, Any]:
        messages = [
            {
                "role": "system",
                "content": f"""执行任务步骤: {step}
                业务规则: {json.dumps(rules, ensure_ascii=False)}
                请按照规则严格执行，返回结构化结果。"""
            },
            {
                "role": "user",
                "content": f"执行参数: {json.dumps(parameters, ensure_ascii=False)}"
            }
        ]
        
        response = self.client.chat.completions.create(
            model="ernie-4.5-turbo-128k-preview",
            messages=messages,
            temperature=0.1  
        )
        
        return {
            "step": step,
            "success": True,
            "result": response.choices[0].message.content,
            "timestamp": datetime.now().isoformat()
        }
```

## ConversationalAgent 类

```python
class ConversationalAgent:
    def __init__(self):
        self.client = OpenAI(
            api_key=os.environ.get("AI_STUDIO_API_KEY"),
            base_url="https://aistudio.baidu.com/llm/lmapi/v3"
        )
        self.conversation_history = []
        self.user_profile = {}
        self.context_window = 10

    def chat(self, user_input: str, user_id: str = "default") -> Dict[str, Any]:
        self._update_user_profile(user_input, user_id)
        
        messages = self._build_conversation_context(user_input, user_id)
        
        response = self.client.chat.completions.create(
            model="ernie-4.5-turbo-128k",
            messages=messages,
            temperature=0.8,  
            stream=False
        )
        
        assistant_response = response.choices[0].message.content
        
        self._record_conversation(user_input, assistant_response, user_id)
        
        return {
            "response": assistant_response,
            "user_id": user_id,
            "conversation_turn": len(self.conversation_history),
            "timestamp": datetime.now().isoformat()
        }

    def _update_user_profile(self, user_input: str, user_id: str):
        if user_id not in self.user_profile:
            self.user_profile[user_id] = {
                "conversation_count": 0,
                "topics": [],
                "preferences": {},
                "first_interaction": datetime.now().isoformat()
            }
        
        self.user_profile[user_id]["conversation_count"] += 1
        self.user_profile[user_id]["last_interaction"] = datetime.now().isoformat()

    def _build_conversation_context(self, user_input: str, user_id: str) -> List[Dict[str, str]]:
        messages = [
            {
                "role": "system",
                "content": f"""你是一个友好、专业的AI助手。
                用户画像: {json.dumps(self.user_profile.get(user_id, {}), ensure_ascii=False)}
                请保持自然、有温度的对话风格。"""
            }
        ]
        
        # 添加近期对话历史
        recent_history = self.conversation_history[-self.context_window:]
        for turn in recent_history:
            if turn["user_id"] == user_id:
                messages.append({"role": "user", "content": turn["user_input"]})
                messages.append({"role": "assistant", "content": turn["assistant_response"]})
        
        messages.append({"role": "user", "content": user_input})
        
        return messages

    def _record_conversation(self, user_input: str, assistant_response: str, user_id: str):
        self.conversation_history.append({
            "user_id": user_id,
            "user_input": user_input,
            "assistant_response": assistant_response,
            "timestamp": datetime.now().isoformat()
        })
```

## 使用示例

```python
# 实例化两种Agent
task_agent = TaskAgent()
conversational_agent = ConversationalAgent()

# 定义一个业务任务
task_agent.define_task(
    "订单处理",
    ["验证订单信息", "检查库存", "计算价格", "生成订单号"],
    {"min_amount": 100, "max_items": 50, "discount_rate": 0.1}
)

# 执行任务
task_result = task_agent.execute_task("订单处理", {
    "items": ["商品A", "商品B"], 
    "quantity": [2, 1],
    "customer_level": "VIP"
})
print("任务执行结果:", task_result)

# 进行对话
chat_result = conversational_agent.chat("你好，我想了解一下你们的产品", "user_001")
print("对话结果:", chat_result)
```

## 任务执行结果

任务执行结果：

```
{'task': '订单处理', 'status': 'completed', 'results': [{'step': '验证订单信息', 'success': True, 'result': '根据提供的业务规则和执行参数，我将验证订单信息并返回结构化结果。

### 业务规则
- `min_amount`: 100（最小订单金额）
- `max_items`: 50（最大商品数量）
- `discount_rate`: 0.1（折扣率）

### 执行参数
- `items`: ["商品A", "商品B"]
- `quantity`: [2, 1]
- `customer_level`: "VIP"

### 验证步骤
1. 商品数量验证：
   - 商品A数量：2
   - 商品B数量：1
   - 总商品数量：2 + 1 = 3
   - 验证：3 ≤ 50（符合规则）

2. 订单金额验证：
   - 假设商品A和商品B的价格未知，无法直接计算订单金额。
   - 由于缺少价格信息，无法验证是否满足最小订单金额（100）的要求。

3. 客户等级验证：
   - 客户等级为"VIP"，但业务规则中未提及与客户等级相关的限制或优惠。
   - 因此，客户等级不影响当前订单验证。

### 结构化结果
    ```json
    {
      "validation_result": {
        "is_valid": false,
        "message": "无法验证订单金额，因为缺少商品价格信息。",
        "details": {
          "total_items": 3,
          "max_items_rule": 50,
          "items_validation": "通过",
          "amount_validation": "无法验证（缺少价格信息）",
          "customer_level": "VIP",
          "discount_rate": 0.1
        }
      }
    }
    ```

### 说明
- 由于缺少商品价格信息，无法计算订单总金额，因此无法验证是否满足最小订单金额（100）的要求'}]
```



对话结果:
```
{'response': '您好呀！很高兴您对我们的产品感兴趣～😊 您想了解的是哪类方向呢？比如是日常实用工具、学习办公类，还是生活娱乐相关的？或者您更关心功能亮点、使用场景，还是价格区间呢？我可以根据您的需求详细介绍哦！', 'user_id': 'user_001', 'conversation_turn': 1, 'timestamp': '2025-06-30T16:17:27.898283'}
```

### 1.3.3 单Agent vs 多Agent协作模式

在复杂的企业环境中，往往需要多个Agent协同工作来完成复杂任务。这种协作模式可以有效提升系统的处理能力和专业性。

```python
class AgentCoordinator:
    """Agent协调器 - 管理多Agent协作"""

    def __init__(self):
        self.agents = {}
        self.task_queue = []
        self.collaboration_history = []

    def register_agent(self, agent_id: str, agent_instance: Any, capabilities: List[str]):
        """注册Agent及其能力"""
        self.agents[agent_id] = {
            "instance": agent_instance,
            "capabilities": capabilities,
            "status": "ready",
            "workload": 0
        }

    def decompose_complex_task(self, complex_task: str) -> List[Dict[str, Any]]:
        """分解复杂任务为子任务"""
        client = OpenAI(
            api_key=os.environ.get("AI_STUDIO_API_KEY"),
            base_url="https://aistudio.baidu.com/llm/lmapi/v3"
        )

        agent_capabilities = {
            agent_id: info["capabilities"]
            for agent_id, info in self.agents.items()
        }

        messages = [
            {
                "role": "system",
                "content": f"""你是任务分解专家，需要将复杂任务分解为可以分配给不同Agent的子任务。

                可用Agent能力: {json.dumps(agent_capabilities, ensure_ascii=False)}

                请按照以下格式返回分解结果:

                {{
                    "subtasks": [
                        {{
                            "task_id": "子任务ID",
                            "description": "任务描述", 
                            "required_capability": "所需能力",
                            "assigned_agent": "推荐的Agent ID",
                            "dependencies": ["依赖的其他子任务ID"],
                            "priority": 1-5
                        }}
                    ]
                }}"""
            },
            {
                "role": "user",
                "content": f"需要分解的复杂任务: {complex_task}"
            }
        ]

        response = client.chat.completions.create(
            model="ernie-4.5-turbo-128k-preview",
            messages=messages,
            temperature=0.2
        )

        try:
            result = json.loads(response.choices[0].message.content)
            return result.get("subtasks", [])
        except:
            return []

    def execute_collaborative_task(self, complex_task: str) -> Dict[str, Any]:
        """执行协作任务"""
        # 分解任务
        subtasks = self.decompose_complex_task(complex_task)

        execution_plan = {
            "complex_task": complex_task,
            "subtasks": subtasks,
            "execution_order": self._determine_execution_order(subtasks),
            "start_time": datetime.now().isoformat()
        }

        results = []

        # 按执行顺序处理子任务
        for task_info in execution_plan["execution_order"]:
            agent_id = task_info["assigned_agent"]
            if agent_id in self.agents:
                # 模拟Agent执行子任务
                subtask_result = self._execute_subtask(agent_id, task_info)
                results.append(subtask_result)

        # 整合结果
        final_result = self._integrate_results(results, complex_task)

        self.collaboration_history.append({
            "task": complex_task,
            "execution_plan": execution_plan,
            "results": results,
            "final_result": final_result,
            "timestamp": datetime.now().isoformat()
        })

        return final_result

    def _determine_execution_order(self, subtasks: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
        """确定执行顺序（简化版拓扑排序）"""
        # 按优先级和依赖关系排序
        sorted_tasks = sorted(subtasks, key=lambda x: (len(x.get("dependencies", [])), -x.get("priority", 1)))
        return sorted_tasks

    def _execute_subtask(self, agent_id: str, task_info: Dict[str, Any]) -> Dict[str, Any]:
        """执行子任务"""
        agent_info = self.agents[agent_id]
        agent_info["workload"] += 1

        # 模拟任务执行
        result = {
            "task_id": task_info["task_id"],
            "agent_id": agent_id,
            "status": "completed",
            "result": f"Agent {agent_id} 成功完成子任务: {task_info['description']}",
            "execution_time": 100 + (agent_info["workload"] * 10),  # 模拟执行时间
            "timestamp": datetime.now().isoformat()
        }

        agent_info["workload"] -= 1
        return result

    def _integrate_results(self, results: List[Dict[str, Any]], original_task: str) -> Dict[str, Any]:
        """整合子任务结果"""
        client = OpenAI(
            api_key=os.environ.get("AI_STUDIO_API_KEY"),
            base_url="https://aistudio.baidu.com/llm/lmapi/v3"
        )

        messages = [
            {
                "role": "system",
                "content": "你是结果整合专家，需要将多个子任务的执行结果整合为最终的完整结果。"
            },
            {
                "role": "user",
                "content": f"""原始任务: {original_task}

                子任务执行结果: {json.dumps(results, ensure_ascii=False)}

                请整合这些结果，提供完整的任务完成报告。"""
            }
        ]

        response = client.chat.completions.create(
            model="ernie-4.5-turbo-128k-preview",
            messages=messages,
            temperature=0.3
        )

        return {
            "original_task": original_task,
            "status": "completed",
            "integrated_result": response.choices[0].message.content,
            "subtask_count": len(results),
            "total_execution_time": sum(r.get("execution_time", 0) for r in results),
            "completion_time": datetime.now().isoformat()
        }


# 创建专业化Agent
class DataAnalysisAgent:
    def __init__(self):
        self.capabilities = ["数据分析", "统计计算", "图表生成"]

class ReportGenerationAgent:
    def __init__(self):
        self.capabilities = ["报告撰写", "内容整理", "格式美化"]

class ResearchAgent:
    def __init__(self):
        self.capabilities = ["信息搜索", "资料整理", "趋势分析"]

# 实例化协调器和专业Agent
coordinator = AgentCoordinator()
data_agent = DataAnalysisAgent()
report_agent = ReportGenerationAgent()
research_agent = ResearchAgent()

# 注册Agent
coordinator.register_agent("data_analyst", data_agent, data_agent.capabilities)
coordinator.register_agent("report_writer", report_agent, report_agent.capabilities)
coordinator.register_agent("researcher", research_agent, research_agent.capabilities)

# 执行复杂协作任务



```python
complex_task = "生成一份关于2024年AI行业发展趋势的综合分析报告，包含市场数据分析和未来预测"

collaboration_result = coordinator.execute_collaborative_task(complex_task)

print("多Agent协作结果:")

print(json.dumps(collaboration_result, ensure_ascii=False, indent=2))
```

```
多Agent协作结果:

{
  "original_task": "生成一份关于2024年AI行业发展趋势的综合分析报告，包含市场数据分析和未来预测",
  "status": "completed",
  "integrated_result": "### 2024年AI行业发展趋势综合分析报告\n\n#### 一、市场数据分析\n\n1. **市场规模与增长**  \n   - **全球市场**：根据IDC预测，2024年全球AI市场规模预计达到**5000亿美元**，同比增长25%。其中，生成式AI（GenAI）占比将超过30%，成为核心增长引擎。  \n   - **区域分布**：北美（45%）、亚太（30%）和欧洲（20%）主导市场，中国、美国、印度为增速最快的前三大国家。  \n   - **行业应用**：医疗（20%）、金融（18%）、制造（15%）和零售（12%）是AI投入最高的领域，自动驾驶、智能客服、供应链优化为关键场景。\n\n2. **技术投资热点**  \n   - **生成式AI**：大模型训练与推理成本下降50%，推动企业级应用落地，预计2024年生成式AI软件市场规模突破**150亿美元**。  \n   - **边缘AI**：物联网设备与AI芯片结合，推动实时决策需求，边缘计算市场年复合增长率达35%。  \n   - **AI伦理与安全**：全球AI监管政策趋严，企业合规支出占比提升至10%，催生第三方审计与模型解释工具市场。\n\n3. **竞争格局**  \n   - **头部企业**：谷歌、微软、OpenAI占据生成式AI市场70%份额，亚马逊、英伟达在基础设施层（云计算、芯片）保持领先。  \n   - **初创生态**：2024年全球AI初创企业融资额预计达**800亿美元**，医疗AI、机器人流程自动化（RPA）为融资热点。  \n   - **开源生态**：Llama 3、Mistral等开源模型下载量激增，降低中小企业技术门槛，推动行业碎片化竞争。\n\n#### 二、未来趋势预测\n\n1. **技术突破方向**  \n   - **多模态大模型**：文本、图像、视频、3D模型融合处理成为标配，支持跨模态生成与推理（如GPT-5、Gemini Ultra）。  \n   - **自主AI代理**：AI Agent（如AutoGPT、Devin）实现任务自动规划与执行，颠覆传统SaaS软件交互模式。  \n   - **量子AI融合**：量子计算与机器学习结合，解决优化问题（如物流路径、药物分子设计），2024年进",
  "subtask_count": 0,
  "total_execution_time": 0,
  "completion_time": "2025-06-30T16:18:30.378429"
}
```

## 1.4 MCP协议在Agent中的关键作用

### 1.4.1 MCP协议核心概念

Model Context Protocol（MCP）是Anthropic推出的用于LLM应用和外部数据源（Resources）、工具（Tools）通信的标准协议。在Agent系统中，MCP协议解决了模型与外部工具集成的标准化问题。MCP协议的核心优势体现在以下几个方面：

| MCP组件 | 传统方式 | MCP方式 | 优势 |
| --- | --- | --- | --- |
| 工具集成 | 硬编码调用 | 标准化接口 | 可插拔、可扩展 |
| 数据源访问 | 直接API调用 | 统一资源管理 | 安全、可控 |
| 上下文管理 | 手动维护 | 协议层处理 | 自动、高效 |

```python
import json
import asyncio
from typing import Dict, List, Any, Optional
from abc import ABC, abstractmethod

class MCPResource(ABC):
    """MCP资源抽象基类"""

    @abstractmethod
    def get_schema(self) -> Dict[str, Any]:
        """获取资源模式定义"""
        pass

    @abstractmethod
    async def read(self, uri: str) -> Any:
        """读取资源内容"""
        pass


class MCPTool(ABC):
    """MCP工具抽象基类"""

    @abstractmethod
    def get_definition(self) -> Dict[str, Any]:
        """获取工具定义"""
        pass

    @abstractmethod
    async def execute(self, parameters: Dict[str, Any]) -> Any:
        """执行工具操作"""
        pass


class FileSystemResource(MCPResource):
    """文件系统资源实现"""

    def __init__(self, base_path: str = "./"):
        self.base_path = base_path

    def get_schema(self) -> Dict[str, Any]:
        return {
            "type": "resource",
            "name": "filesystem",
            "description": "本地文件系统访问",
            "properties": {
                "path": {
                    "type": "string",
                    "description": "文件路径"
                }
            }
        }

    async def read(self, uri: str) -> Any:
        """读取文件内容"""
        try:
            file_path = uri.replace("file://", "")
            with open(f"{self.base_path}/{file_path}", "r", encoding="utf-8") as f:
                return {
                    "content": f.read(),
                    "uri": uri,
                    "type": "text",
                    "size": len(f.read())
                }
        except Exception as e:
            return {"error": str(e), "uri": uri}


class CalculatorTool(MCPTool):
    """计算器工具实现"""

    def get_definition(self) -> Dict[str, Any]:
        return {
            "type": "function",
            "function": {
                "name": "calculator",
                "description": "执行数学计算",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "expression": {
                            "type": "string",
                            "description": "数学表达式，如 '2 + 3 * 4'"
                        }
                    },
                    "required": ["expression"]
                }
            }
        }

    async def execute(self, parameters: Dict[str, Any]) -> Any:
        """执行计算"""
        try:
            expression = parameters.get("expression", "")
            # 安全的表达式计算（仅支持基本运算）
            allowed_chars = "0123456789+-*/.() "
            if all(c in allowed_chars for c in expression):
                result = eval(expression)
                return {
                    "expression": expression,
                    "result": result,
                    "type": "calculation"
                }
            else:
                return {"error": "不支持的计算表达式", "expression": expression}
        except Exception as e:
            return {"error": str(e), "expression": parameters.get("expression", "")}


class WebSearchTool(MCPTool):
    """网络搜索工具实现"""

    def get_definition(self) -> Dict[str, Any]:
        return {
            "type": "function",
            "function": {
                "name": "web_search",
                "description": "搜索网络信息",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "query": {
                            "type": "string",
                            "description": "搜索关键词"
                        },
                        "max_results": {
                            "type": "integer",
                            "description": "最大结果数量",
                            "default": 5
                        }
                    },
                    "required": ["query"]
                }
            }
        }

    async def execute(self, parameters: Dict[str, Any]) -> Any:
        """执行网络搜索（模拟实现）"""
        query = parameters.get("query", "")
```



## 1.4.2 Agent与MCP的集成模式

在实际应用中，Agent需要与MCP服务器进行深度集成，实现动态的工具发现和调用。这种集成模式使得Agent具备了强大的扩展能力。

```python
max_results = parameters.get("max_results", 5)

# 模拟搜索结果

mock_results = [

    {

        "title": f"关于'{query}'的搜索结果 {i+1}",

        "url": f"https://example.com/result_{i+1}",

        "snippet": f"这是关于{query}的详细信息描述...",

        "relevance_score": 0.9 - (i * 0.1)

    }

    for i in range(min(max_results, 5))

]

return {

    "query": query,

    "results": mock_results,

    "total_count": len(mock_results),

    "search_time": "0.23s"

}

class MCPServer:

    """MCP服务器实现"""

    def __init__(self):

        self.resources: Dict[str, MCPResource] = {}

        self.tools: Dict[str, MCPTool] = {}

        self.clients = set()

    def register_resource(self, name: str, resource: MCPResource):

        """注册资源"""

        self.resources[name] = resource

    def register_tool(self, name: str, tool: MCPTool):

        """注册工具"""

        self.tools[name] = tool

    def list_resources(self) -> List[Dict[str, Any]]:

        """列出所有可用资源"""

        return [

            {

                "name": name,

                "schema": resource.get_schema()

            }

            for name, resource in self.resources.items()

        ]

    def list_tools(self) -> List[Dict[str, Any]]:

        """列出所有可用工具"""

        return [

            {

                "name": name,

                "definition": tool.get_definition()

            }

            for name, tool in self.tools.items()

        ]

    async def read_resource(self, resource_name: str, uri: str) -> Any:

        """读取资源"""

        if resource_name not in self.resources:

            return {"error": f"资源 {resource_name} 不存在"}

        return await self.resources[resource_name].read(uri)

    async def call_tool(self, tool_name: str, parameters: Dict[str, Any]) -> Any:

        """调用工具"""

        if tool_name not in self.tools:

            return {"error": f"工具 {tool_name} 不存在"}

        return await self.tools[tool_name].execute(parameters)

class MCPClient:

    """MCP客户端实现"""

    def __init__(self, server: MCPServer):

        self.server = server

        self.session_id = f"session_{datetime.now().timestamp()}"

    async def get_available_resources(self) -> List[Dict[str, Any]]:

        """获取可用资源列表"""

        return self.server.list_resources()

    async def get_available_tools(self) -> List[Dict[str, Any]]:

        """获取可用工具列表"""

        return self.server.list_tools()

    async def read_resource(self, resource_name: str, uri: str) -> Any:

        """读取资源"""

        return await self.server.read_resource(resource_name, uri)

    async def call_tool(self, tool_name: str, parameters: Dict[str, Any]) -> Any:

        """调用工具"""

        return await self.server.call_tool(tool_name, parameters)

# 创建MCP服务器和客户端

mcp_server = MCPServer()

# 注册资源和工具

mcp_server.register_resource("filesystem", FileSystemResource())

mcp_server.register_tool("calculator", CalculatorTool())

mcp_server.register_tool("web_search", WebSearchTool())

mcp_client = MCPClient(mcp_server)

# 测试MCP功能

async def test_mcp_functionality():

    # 获取可用资源和工具

    resources = await mcp_client.get_available_resources()

    tools = await mcp_client.get_available_tools()

    print("可用资源:")

    for resource in resources:

        print(f"- {resource['name']}: {resource['schema']['description']}")

    print("\n可用工具:")

    for tool in tools:

        print(f"- {tool['name']}: {tool['definition']['function']['description']}")

    # 测试工具调用

    calc_result = await mcp_client.call_tool("calculator", {"expression": "15 * 3 + 7"})

    print(f"\n计算结果: {calc_result}")

    search_result = await mcp_client.call_tool("web_search", {"query": "人工智能发展趋势", "max_results": 3})

    print(f"\n搜索结果: {json.dumps(search_result, ensure_ascii=False, indent=2)}")

# 在notebook中运行异步函数

import nest_asyncio

nest_asyncio.apply()

await test_mcp_functionality()
```

```python
class MCPEnabledAgent:
    """支持MCP协议的Agent"""

    def __init__(self, mcp_client: MCPClient, model_name: str = "ernie-4.5-turbo-128k-preview"):
        self.mcp_client = mcp_client
        self.client = OpenAI(
            api_key=os.environ.get("AI_STUDIO_API_KEY"),
            base_url="https://aistudio.baidu.com/llm/lmapi/v3"
        )
        self.model_name = model_name
        self.conversation_history = []

    async def process_request(self, user_request: str) -> Dict[str, Any]:
        """处理用户请求，自动判断是否需要使用工具"""

        # 获取可用工具信息
        available_tools = await self.mcp_client.get_available_tools()
        tools_info = self._format_tools_for_llm(available_tools)

        # 构建消息
        messages = [

            {

                "role": "system",

                "content": f"""你是一个智能助手，可以使用以下工具来帮助用户:

                {tools_info}

                如果需要使用工具，请按照以下格式返回:

                {{

                    "need_tool": true,

                    "tool_name": "工具名称",

                    "parameters": {{参数对象}},

                    "reasoning": "使用工具的原因"

                }}

                如果不需要工具，直接回答用户问题。"""

            }

        ]

        messages.extend(self.conversation_history[-6:])

        messages.append({"role": "user", "content": user_request})

        response = self.client.chat.completions.create(

            model=self.model_name,

            messages=messages,

            temperature=0.3

        )

        assistant_response = response.choices[0].message.content

        tool_decision = self._parse_tool_decision(assistant_response)

        if tool_decision and tool_decision.get("need_tool"):

            tool_result = await self._execute_tool_call(tool_decision)

            final_response = await self._generate_final_response(

                user_request, tool_decision, tool_result

            )

            result = {

                "user_request": user_request,

                "tool_used": tool_decision["tool_name"],

                "tool_result": tool_result,

                "response": final_response,

                "timestamp": datetime.now().isoformat()

            }

        else:

            result = {

                "user_request": user_request,

                "tool_used": None,

                "response": assistant_response,

                "timestamp": datetime.now().isoformat()

            }
```



```python
        self.conversation_history.extend([

            {"role": "user", "content": user_request},

            {"role": "assistant", "content": result["response"]}

        ])

        return result

    def _format_tools_for_llm(self, tools: List[Dict[str, Any]]) -> str:

        """格式化工具信息供LLM理解"""

        tool_descriptions = []

        for tool in tools:

            definition = tool["definition"]["function"]

            tool_descriptions.append(

                f"- {definition['name']}: {definition['description']}\n"

                f"  参数: {json.dumps(definition['parameters'], ensure_ascii=False)}"

            )

        return "\n".join(tool_descriptions)

    def _parse_tool_decision(self, response: str) -> Optional[Dict[str, Any]]:

        try:

            if "need_tool" in response and "{" in response and "}" in response:

                start_idx = response.find("{")

                end_idx = response.rfind("}") + 1

                json_str = response[start_idx:end_idx]

                return json.loads(json_str)

        except:

            pass

        return None

    async def _execute_tool_call(self, tool_decision: Dict[str, Any]) -> Dict[str, Any]:

        tool_name = tool_decision["tool_name"]

        parameters = tool_decision["parameters"]

        try:

            result = await self.mcp_client.call_tool(tool_name, parameters)

            return {

                "success": True,

                "result": result,

                "tool_name": tool_name,

                "parameters": parameters

            }

        except Exception as e:

            return {

                "success": False,

                "error": str(e),

                "tool_name": tool_name,

                "parameters": parameters

            }

    async def _generate_final_response(self, user_request: str, tool_decision: Dict[str, Any], tool_result: Dict[str, Any]) -> str:

        messages = [

            {

                "role": "system",

                "content": "基于工具执行结果，为用户提供清晰、有用的回复。"

            },

            {

                "role": "user",

                "content": f"""用户请求: {user_request}

                使用工具: {tool_decision['tool_name']}

                工具执行结果: {json.dumps(tool_result, ensure_ascii=False)}

                请基于这些信息为用户提供有帮助的回复。"""

            }

        ]

        response = self.client.chat.completions.create(

            model=self.model_name,

            messages=messages,

            temperature=0.4

        )

        return response.choices[0].message.content

mcp_agent = MCPEnabledAgent(mcp_client)

async def test_mcp_agent():

    calc_request = "帮我计算一下，如果我每月投资3000元，年化收益率8%，5年后总共能有多少钱？"

    calc_result = await mcp_agent.process_request(calc_request)

    print("计算请求结果:")

    print(json.dumps(calc_result, ensure_ascii=False, indent=2))

    print("\n" + "="*50 + "\n")

    search_request = "最近人工智能在医疗领域有什么新的应用？"

    search_result = await mcp_agent.process_request(search_request)

    print("搜索请求结果:")

    print(json.dumps(search_result, ensure_ascii=False, indent=2))

    print("\n" + "="*50 + "\n")

    simple_request = "什么是人工智能？"

    simple_result = await mcp_agent.process_request(simple_request)

    print("简单请求结果:")

    print(json.dumps(simple_result, ensure_ascii=False, indent=2))

await test_mcp_agent()
```

## Agent开发框架构建

### 2.1 基础Agent架构设计

#### 2.1.1 模块化架构设计

一个完善的`Agent`系统需要具备良好的模块化设计，确保各组件之间的松耦合和高内聚。我们将构建一个分层的架构：

+ 接口层: 处理外部交互和用户界面
+ 控制层: `Agent`的核心控制逻辑
+ 服务层: 各种功能服务和工具
+ 数据层: 记忆系统和知识存储

![](img/a5ce0954bc05f38061b4fa9d56254e18.png)

#### 2.1.2 Agent框架核心实现

`ERNIE`（Enhanced Representation through Knowledge Integration）是百度开发的知识增强语义理解模型。在`Agent`应用中，合理配置和管理`ERNIE` `API`连接是确保系统稳定运行的基础。

```python
import os

import time

import asyncio

from typing import Dict, List, Any, Optional

import httpx

from dataclasses import dataclass

from openai import OpenAI

import threading

from queue import Queue

import logging

@dataclass
class ERNIEConfig:
    """ERNIE模型配置类"""

    api_key: str

    base_url: str = "https://aistudio.baidu.com/llm/lmapi/v3"

    model_name: str = "ernie-4.5-turbo-128k-preview"

    max_tokens: int = 4096

    temperature: float = 0.7

    timeout: int = 30

    max_retries: int = 3

    rate_limit: int = 10

class ERNIEConnectionManager:

    def __init__(self, config: ERNIEConfig):

        self.config = config

        self.client = OpenAI(

            api_key=config.api_key,

            base_url=config.base_url,

            timeout=config.timeout

        )

        self.request_queue = Queue()

        self.rate_limiter = self._create_rate_limiter()

        self.health_status = {"healthy": True, "last_check": time.time()}

        self.request_stats = {"total": 0, "success": 0, "failed": 0}

    def _create_rate_limiter(self):

        def rate_limiter():

            while True:

                time.sleep(1.0 / self.config.rate_limit)

                if not self.request_queue.empty():

                    yield self.request_queue.get()

        return rate_limiter()

    async def health_check(self) -> bool:

        try:

            test_messages = [{"role": "user", "content": "Hello"}]

            response = self.client.chat.completions.create(

                model=self.config.model_name,

                messages=test_messages,

                max_tokens=10

            )

            self.health_status = {

                "healthy": True,

                "last_check": time.time(),

                "response_time": time.time() - self.health_status["last_check"]

            }

            return True

        except Exception as e:

            self.health_status = {

                "healthy": False,

                "last_check": time.time(),

                "error": str(e)

            }

            return False

    def get_stats(self) -> Dict[str, Any]:

        success_rate = (self.request_stats["success"] / max(self.request_stats["total"], 1)) * 100

        return {

            "health_status": self.health_status,

            "request_stats": self.request_stats,

            "success_rate": f"{success_rate:.2f}%",

            "config": {

                "model_name": self.config.model_name,

                "rate_limit": self.config.rate_limit,

                "timeout": self.config.timeout

            }

        }

class ERNIEModelWrapper:

    def __init__(self, connection_manager: ERNIEConnectionManager):

        self.connection_manager = connection_manager

        self.client = connection_manager.client

        self.config = connection_manager.config

        self.logger = logging.getLogger(__name__)

    async def generate_completion(

        self, 
```



```python
messages: List[Dict[str, str]],

**kwargs

) -> Dict[str, Any]:

    params = {

        "model": self.config.model_name,

        "messages": messages,

        "temperature": kwargs.get("temperature", self.config.temperature),

        "max_tokens": kwargs.get("max_tokens", self.config.max_tokens),

        **kwargs

    }

    for attempt in range(self.config.max_retries):

        try:

            self.connection_manager.request_stats["total"] += 1

            start_time = time.time()

            response = self.client.chat.completions.create(**params)

            end_time = time.time()

            self.connection_manager.request_stats["success"] += 1

            result = {

                "content": response.choices[0].message.content,

                "model": response.model,

                "usage": {

                    "prompt_tokens": response.usage.prompt_tokens,

                    "completion_tokens": response.usage.completion_tokens,

                    "total_tokens": response.usage.total_tokens

                },

                "response_time": end_time - start_time,

                "attempt": attempt + 1

            }

            return result

        except Exception as e:

            self.connection_manager.request_stats["failed"] += 1

            self.logger.warning(f"请求失败 (尝试 {attempt + 1}/{self.config.max_retries}): {str(e)}")

            if attempt == self.config.max_retries - 1:

                raise Exception(f"所有重试均失败: {str(e)}")

            await asyncio.sleep(2 ** attempt)

    async def generate_streaming_completion(

        self,

        messages: List[Dict[str, str]],

        **kwargs

    ):        

        params = {

            "model": self.config.model_name,

            "messages": messages,

            "temperature": kwargs.get("temperature", self.config.temperature),

            "max_tokens": kwargs.get("max_tokens", self.config.max_tokens),

            "stream": True,

            **kwargs

        }

        try:

            self.connection_manager.request_stats["total"] += 1

            response = self.client.chat.completions.create(**params)

            full_content = ""

            for chunk in response:

                if chunk.choices[0].delta.content:

                    content_delta = chunk.choices[0].delta.content

                    full_content += content_delta

                    yield {

                        "delta": content_delta,

                        "full_content": full_content,

                        "finished": False

                    }

            self.connection_manager.request_stats["success"] += 1

            yield {

                "delta": "",

                "full_content": full_content,

                "finished": True

            }

        except Exception as e:

            self.connection_manager.request_stats["failed"] += 1

            raise e

ernie_config = ERNIEConfig(

    api_key=os.environ.get("AI_STUDIO_API_KEY"),

    model_name="ernie-4.5-turbo-128k-preview",

    temperature=0.7,

    max_tokens=2048,

    rate_limit=5

)

connection_manager = ERNIEConnectionManager(ernie_config)

ernie_model = ERNIEModelWrapper(connection_manager)

async def test_ernie_functionality():

    health_ok = await connection_manager.health_check()

    print(f"ERNIE健康检查: {'通过' if health_ok else '失败'}")

    messages = [

        {"role": "system", "content": "你是一个专业的AI助手，擅长分析和解决问题。"},

        {"role": "user", "content": "请分析一下量子计算在未来十年的发展前景。"}

    ]

    result = await ernie_model.generate_completion(messages)

    print(f"\n生成结果:")

    print(f"内容: {result['content'][:200]}...")

    print(f"Token使用: {result['usage']}")

    print(f"响应时间: {result['response_time']:.2f}秒")

    print(f"\n流式生成测试:")

    stream_messages = [

        {"role": "user", "content": "用一段话描述人工智能的发展历程。"}

    ]

    async for chunk in ernie_model.generate_streaming_completion(stream_messages):

        if chunk["finished"]:

            print(f"\n流式生成完成，总长度: {len(chunk['full_content'])}")

        else:

            print(chunk["delta"], end="", flush=True)

    stats = connection_manager.get_stats()

    print(f"\n连接统计:")

    print(json.dumps(stats, ensure_ascii=False, indent=2))

await test_ernie_functionality()
```

### 2.1.3 模型参数调优与性能优化

在实际应用中，不同的使用场景需要不同的模型参数配置。理解和优化这些参数对Agent性能至关重要。

```python
from enum import Enum

from typing import NamedTuple

import statistics

class TaskType(Enum):

    CREATIVE_WRITING = "creative_writing"

    LOGICAL_REASONING = "logical_reasoning"

    FACTUAL_QA = "factual_qa"

    CODE_GENERATION = "code_generation"

    CONVERSATION = "conversation"

class ModelParams(NamedTuple):

    temperature: float

    top_p: float

    max_tokens: int

    presence_penalty: float

    frequency_penalty: float

class ERNIEParameterOptimizer:

    def __init__(self, ernie_model: ERNIEModelWrapper):

        self.ernie_model = ernie_model

        self.task_configs = self._initialize_task_configs()

        self.performance_history = []

    def _initialize_task_configs(self) -> Dict[TaskType, ModelParams]:

        return {

            TaskType.CREATIVE_WRITING: ModelParams(

                temperature=0.9,

                top_p=0.8,

                max_tokens=2048,

                presence_penalty=0.1,

                frequency_penalty=0.1

            ),

            TaskType.LOGICAL_REASONING: ModelParams(

                temperature=0.1,

                top_p=0.9,

                max_tokens=1024,

                presence_penalty=0.0,

                frequency_penalty=0.0

            ),

            TaskType.FACTUAL_QA: ModelParams(

                temperature=0.2,

                top_p=0.9,

                max_tokens=512,

                presence_penalty=0.0,

                frequency_penalty=0.0

            ),

            TaskType.CODE_GENERATION: ModelParams(

                temperature=0.1,

                top_p=0.95,

                max_tokens=1536,

                presence_penalty=0.0,

                frequency_penalty=0.0

            ),

            TaskType.CONVERSATION: ModelParams(

                temperature=0.7,

                top_p=0.8,

                max_tokens=1024,

                presence_penalty=0.3,

                frequency_penalty=0.3

            )

        }

    async def generate_optimized(

        self,

        messages: List[Dict[str, str]],

        task_type: TaskType,

        custom_params: Optional[Dict[str, Any]] = None) -> Dict[str, Any]:

        base_params = self.task_configs[task_type]._asdict()

        if custom_params:

            base_params.update(custom_params)

        start_time = time.time()

        result = await self.ernie_model.generate_completion(
```



```python
messages=messages,
**base_params
)

performance_data = {
    "task_type": task_type.value,
    "params": base_params,
    "response_time": result["response_time"],
    "token_usage": result["usage"],
    "timestamp": time.time()
}

self.performance_history.append(performance_data)

result["task_type"] = task_type.value
result["optimized_params"] = base_params
return result

def analyze_performance(self, task_type: Optional[TaskType] = None) -> Dict[str, Any]:
    if task_type:
        filtered_data = [
            d for d in self.performance_history 
            if d["task_type"] == task_type.value
        ]
    else:
        filtered_data = self.performance_history

    if not filtered_data:
        return {"message": "暂无性能数据"}

    response_times = [d["response_time"] for d in filtered_data]
    token_counts = [d["token_usage"]["total_tokens"] for d in filtered_data]

    analysis = {
        "total_requests": len(filtered_data),
        "response_time": {
            "mean": statistics.mean(response_times),
            "median": statistics.median(response_times),
            "min": min(response_times),
            "max": max(response_times)
        },
        "token_usage": {
            "mean": statistics.mean(token_counts),
            "median": statistics.median(token_counts),
            "total": sum(token_counts)
        }
    }

    if task_type:
        analysis["task_type"] = task_type.value

    return analysis

async def auto_tune_parameters(
    self,
    test_messages: List[Dict[str, str]],
    task_type: TaskType,
    optimization_target: str = "quality"  # "quality" or "speed"
) -> ModelParams:
    base_params = self.task_configs[task_type]
    best_params = base_params
    best_score = 0

    if optimization_target == "quality":
        temp_range = [0.1, 0.3, 0.5, 0.7, 0.9]
        top_p_range = [0.8, 0.9, 0.95]
    else:  
        temp_range = [0.1, 0.3]
        top_p_range = [0.9]

    print(f"开始自动调优 {task_type.value} 任务参数...")

    for temp in temp_range:
        for top_p in top_p_range:
            test_params = base_params._replace(
                temperature=temp,
                top_p=top_p
            )

            try:
                result = await self.ernie_model.generate_completion(
                    messages=test_messages,
                    **test_params._asdict()
                )
                score = self._calculate_score(result, optimization_target)
                print(f"测试参数 temp={temp}, top_p={top_p}, 得分={score:.3f}")

                if score > best_score:
                    best_score = score
                    best_params = test_params
            except Exception as e:
                print(f"参数测试失败 temp={temp}, top_p={top_p}: {e}")

            await asyncio.sleep(1)

    self.task_configs[task_type] = best_params
    print(f"调优完成，最佳参数: {best_params}")
    return best_params

def _calculate_score(self, result: Dict[str, Any], target: str) -> float:
    """计算参数组合得分"""
    if target == "speed":
        return 1.0 / (result["response_time"] + 0.1)
    else:
        content_length = len(result["content"])
        response_time = result["response_time"]
        quality_score = (content_length / 100) / (response_time + 1)
        return quality_score

optimizer = ERNIEParameterOptimizer(ernie_model)

async def test_parameter_optimization():
    creative_messages = [
        {"role": "user", "content": "写一个关于未来城市的科幻短故事，要求富有想象力和创意。"}
    ]

    creative_result = await optimizer.generate_optimized(
        creative_messages, 
        TaskType.CREATIVE_WRITING
    )

    print("创意写作结果:")
    print(f"内容长度: {len(creative_result['content'])}")
    print(f"使用参数: {creative_result['optimized_params']}")
    print(f"内容预览: {creative_result['content'][:150]}...\n")

    reasoning_messages = [
        {"role": "user", "content": "如果所有的猫都是动物，而加菲猫是一只猫，那么加菲猫是动物吗？请解释推理过程。"}
    ]

    reasoning_result = await optimizer.generate_optimized(
        reasoning_messages,
        TaskType.LOGICAL_REASONING
    )

    print("逻辑推理结果:")
    print(f"内容: {reasoning_result['content']}")
    print(f"使用参数: {reasoning_result['optimized_params']}\n")

    qa_messages = [
        {"role": "user", "content": "中国的首都是哪里？请给出简洁准确的答案。"}
    ]

    qa_result = await optimizer.generate_optimized(
        qa_messages,
        TaskType.FACTUAL_QA
    )

    print("事实问答结果:")
    print(f"内容: {qa_result['content']}")
    print(f"使用参数: {qa_result['optimized_params']}\n")

    performance_analysis = optimizer.analyze_performance()
    print("性能分析:")
    print(json.dumps(performance_analysis, ensure_ascii=False, indent=2))

await test_parameter_optimization()
```

### 2.2 基于ERNIE的多模态Agent构建

#### 2.2.1 文本与图像的融合处理

ERNIE模型支持多模态输入，可以同时处理文本和图像信息。这为Agent提供了更丰富的感知能力。

```python
import base64
import requests
from PIL import Image
import io
from typing import Union


class MultimodalERNIEAgent:

    def __init__(self, ernie_model: ERNIEModelWrapper):
        self.ernie_model = ernie_model
        self.supported_formats = ["jpg", "jpeg", "png", "bmp", "webp"]
        self.max_image_size = 10 * 1024 * 1024

    def encode_image_to_base64(self, image_path: str) -> str:
        try:
            with open(image_path, "rb") as image_file:
                return base64.b64encode(image_file.read()).decode("utf-8")
        except Exception as e:
            raise Exception(f"图像编码失败: {e}")

    def download_and_encode_image(self, image_url: str) -> str:
        try:
            response = requests.get(image_url, timeout=10)
            response.raise_for_status()
            if len(response.content) > self.max_image_size:
                raise Exception("图像文件过大")
            return base64.b64encode(response.content).decode("utf-8")
        except Exception as e:
            raise Exception(f"图像下载失败: {e}")

    async def analyze_image_with_text(
        self,
        image_input: Union[str, bytes],
        text_prompt: str,
        image_format: str = "jpeg"
    ) -> Dict[str, Any]:
        if isinstance(image_input, str):
            if image_input.startswith("http"):
                base64_image = self.download_and_encode_image(image_input)
            else:
                base64_image = self.encode_image_to_base64(image_input)
        else:
```



```python
base64_image = base64.b64encode(image_input).decode('utf-8')

messages = [
    {
        "role": "user",
        "content": [
            {
                "type": "text",
                "text": text_prompt
            },
            {
                "type": "image_url",
                "image_url": {
                    "url": f"data:image/{image_format};base64,{base64_image}"
                }
            }
        ]
    }
]

result = await self.ernie_model.generate_completion(
    messages=messages,
    model="ernie-4.5-turbo-vl-preview",
    max_tokens=2048
)

return {
    "analysis": result["content"],
    "prompt": text_prompt,
    "model_used": "ernie-4.5-turbo-vl-preview",
    "image_size": len(base64_image),
    "usage": result["usage"],
    "response_time": result["response_time"]
}

async def compare_images(
    self,
    image1_input: Union[str, bytes],
    image2_input: Union[str, bytes],
    comparison_aspect: str = "general") -> Dict[str, Any]:
    if isinstance(image1_input, str):
        if image1_input.startswith("http"):
            base64_image1 = self.download_and_encode_image(image1_input)
        else:
            base64_image1 = self.encode_image_to_base64(image1_input)
    else:
        base64_image1 = base64.b64encode(image1_input).decode('utf-8')

    if isinstance(image2_input, str):
        if image2_input.startswith("http"):
            base64_image2 = self.download_and_encode_image(image2_input)
        else:
            base64_image2 = self.encode_image_to_base64(image2_input)
    else:
        base64_image2 = base64.b64encode(image2_input).decode('utf-8')

    messages = [
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": f"请比较这两张图像的{comparison_aspect}。分析它们的相似点和不同点，提供详细的比较结果。"
                },
                {
                    "type": "image_url",
                    "image_url": {
                        "url": f"data:image/jpeg;base64,{base64_image1}"
                    }
                },
                {
                    "type": "image_url",
                    "image_url": {
                        "url": f"data:image/jpeg;base64,{base64_image2}"
                    }
                }
            ]
        }
    ]

    result = await self.ernie_model.generate_completion(
        messages=messages,
        model="ernie-4.5-turbo-vl-preview",
        max_tokens=2048
    )

    return {
        "comparison": result["content"],
        "aspect": comparison_aspect,
        "model_used": "ernie-4.5-turbo-vl-preview",
        "usage": result["usage"]
    }

async def generate_image_description(
    self,
    image_input: Union[str, bytes],
    description_style: str = "detailed") -> Dict[str, Any]:
    style_prompts = {
        "brief": "请用简洁的语言描述这张图像的主要内容。",
        "detailed": "请详细描述这张图像，包括场景、人物、物体、颜色、构图等各个方面。",
        "creative": "请用富有创意和文学性的语言描述这张图像，让读者能够通过文字感受到画面的美感。"
    }

    prompt = style_prompts.get(description_style, style_prompts["detailed"])

    result = await self.analyze_image_with_text(
        image_input=image_input,
        text_prompt=prompt
    )

    return {
        "description": result["analysis"],
        "style": description_style,
        "model_used": result["model_used"],
        "usage": result["usage"]
    }

async def extract_text_from_image(
    self,
    image_input: Union[str, bytes]
) -> Dict[str, Any]:
    result = await self.analyze_image_with_text(
        image_input=image_input,
        text_prompt="请识别并提取这张图像中的所有文字内容。如果图像中包含表格，请按照表格格式整理文字。"
    )

    return {
        "extracted_text": result["analysis"],
        "model_used": result["model_used"],
        "usage": result["usage"]
    }

multimodal_agent = MultimodalERNIEAgent(ernie_model)

def create_test_image():
    with open("test.jpeg", "rb") as image_file:
        return image_file.read()

async def test_multimodal_agent():
    test_image_bytes = create_test_image()

    print("=== 图像描述生成测试 ===")

    description_result = await multimodal_agent.generate_image_description(
        test_image_bytes,
        description_style="detailed"
    )

    print(f"详细描述: {description_result['description']}")
    print(f"Token使用: {description_result['usage']}")

    print("\n=== 简洁描述测试 ===")

    brief_description = await multimodal_agent.generate_image_description(
        test_image_bytes,
        description_style="brief"
    )

    print(f"简洁描述: {brief_description['description']}")

    print("\n=== 文字提取测试 ===")

    text_extraction = await multimodal_agent.extract_text_from_image(test_image_bytes)

    print(f"提取的文字: {text_extraction['extracted_text']}")

    print("\n=== 自定义分析测试 ===")

    custom_analysis = await multimodal_agent.analyze_image_with_text(
        test_image_bytes,
        "这张图像中有哪些几何形状？请列出它们的颜色和位置。"
    )

    print(f"几何形状分析: {custom_analysis['analysis']}")

await test_multimodal_agent()
```

## 2.2.2 多模态任务编排与执行

在复杂的多模态应用场景中，Agent需要能够协调处理不同类型的输入，并根据任务需求进行智能编排。

```python
from enum import Enum

from dataclasses import dataclass

from typing import List, Dict, Any, Optional, Union

import asyncio

class TaskStepType(Enum):
    TEXT_ANALYSIS = "text_analysis"
    IMAGE_ANALYSIS = "image_analysis"
    MULTIMODAL_FUSION = "multimodal_fusion"
    COMPARISON = "comparison"
    SYNTHESIS = "synthesis"


@dataclass
class TaskStep:
    step_id: str
    step_type: TaskStepType
    input_data: Dict[str, Any]
    output_requirements: Dict[str, Any]
    dependencies: List[str] = None


class MultimodalTaskOrchestrator:

    def __init__(self, multimodal_agent: MultimodalERNIEAgent):
        self.agent = multimodal_agent
        self.task_registry = {}
        self.execution_history = []

    def register_task_template(
        self,
        template_name: str,
        steps: List[TaskStep],
        description: str):
```



```python
self.task_registry[template_name] = {
    "steps": steps,
    "description": description,
    "created_at": datetime.now().isoformat()
}


async def execute_complex_task(
    self,
    task_name: str,
    input_data: Dict[str, Any]
) -> Dict[str, Any]:
    if task_name not in self.task_registry:
        raise ValueError(f"未找到任务模板: {task_name}")

    task_template = self.task_registry[task_name]
    steps = task_template["steps"]

    step_results = {}

    execution_plan = []

    execution_order = self._resolve_dependencies(steps)

    print(f"开始执行任务: {task_name}")
    print(f"执行步骤: {[step.step_id for step in execution_order]}")

    for step in execution_order:
        print(f"执行步骤: {step.step_id} ({step.step_type.value})")

        step_input = self._prepare_step_input(step, input_data, step_results)
        step_result = await self._execute_step(step, step_input)

        step_results[step.step_id] = step_result

        execution_plan.append({
            "step_id": step.step_id,
            "step_type": step.step_type.value,
            "execution_time": step_result.get("execution_time", 0),
            "success": step_result.get("success", True)
        })

    final_result = {
        "task_name": task_name,
        "execution_plan": execution_plan,
        "step_results": step_results,
        "final_output": self._synthesize_final_output(step_results, task_template),
        "timestamp": datetime.now().isoformat()
    }

    self.execution_history.append(final_result)

    return final_result


def _resolve_dependencies(self, steps: List[TaskStep]) -> List[TaskStep]:
    ordered_steps = []
    remaining_steps = steps.copy()

    while remaining_steps:
        executable_steps = []

        for step in remaining_steps:
            if not step.dependencies or all(
                dep in [s.step_id for s in ordered_steps]
                for dep in step.dependencies
            ):
                executable_steps.append(step)

        if not executable_steps:
            raise ValueError("检测到循环依赖，无法执行任务")

        # 添加到执行顺序并从待处理列表移除
        for step in executable_steps:
            ordered_steps.append(step)
            remaining_steps.remove(step)

    return ordered_steps


def _prepare_step_input(
    self,
    step: TaskStep,
    original_input: Dict[str, Any],
    previous_results: Dict[str, Any]
) -> Dict[str, Any]:
    step_input = original_input.copy()

    if step.dependencies:
        for dep_id in step.dependencies:
            if dep_id in previous_results:
                step_input[f"dep_{dep_id}"] = previous_results[dep_id]

    step_input.update(step.input_data)

    return step_input


async def _execute_step(self, step: TaskStep, input_data: Dict[str, Any]) -> Dict[str, Any]:
    start_time = time.time()

    try:
        if step.step_type == TaskStepType.TEXT_ANALYSIS:
            result = await self._execute_text_analysis(input_data)
        elif step.step_type == TaskStepType.IMAGE_ANALYSIS:
            result = await self._execute_image_analysis(input_data)
        elif step.step_type == TaskStepType.MULTIMODAL_FUSION:
            result = await self._execute_multimodal_fusion(input_data)
        elif step.step_type == TaskStepType.COMPARISON:
            result = await self._execute_comparison(input_data)
        elif step.step_type == TaskStepType.SYNTHESIS:
            result = await self._execute_synthesis(input_data)
        else:
            raise ValueError(f"不支持的步骤类型: {step.step_type}")

        result.update({
            "success": True,
            "execution_time": time.time() - start_time,
            "step_id": step.step_id
        })

        return result

    except Exception as e:
        return {
            "success": False,
            "error": str(e),
            "execution_time": time.time() - start_time,
            "step_id": step.step_id
        }


async def _execute_text_analysis(self, input_data: Dict[str, Any]) -> Dict[str, Any]:
    text = input_data.get("text", "")
    analysis_type = input_data.get("analysis_type", "general")

    messages = [
        {
            "role": "user",
            "content": f"请对以下文本进行{analysis_type}分析：\n\n{text}"
        }
    ]

    result = await self.agent.ernie_model.generate_completion(messages)

    return {
        "analysis_result": result["content"],
        "analysis_type": analysis_type,
        "input_text_length": len(text),
        "usage": result["usage"]
    }


async def _execute_image_analysis(self, input_data: Dict[str, Any]) -> Dict[str, Any]:
    image_data = input_data.get("image_data")
    analysis_prompt = input_data.get("prompt", "请分析这张图像")

    result = await self.agent.analyze_image_with_text(
        image_input=image_data,
        text_prompt=analysis_prompt
    )

    return {
        "image_analysis": result["analysis"],
        "usage": result["usage"]
    }


async def _execute_multimodal_fusion(self, input_data: Dict[str, Any]) -> Dict[str, Any]:
    """执行多模态融合分析"""

    text_analysis = input_data.get("dep_text_analysis", {}).get("analysis_result", "")
    image_analysis = input_data.get("dep_image_analysis", {}).get("image_analysis", "")
    fusion_goal = input_data.get("fusion_goal", "综合分析")

    messages = [
        {
            "role": "user",
            "content": f"""基于以下信息进行{fusion_goal}：

            文本分析结果：

            {text_analysis}

            图像分析结果：

            {image_analysis}

            请结合这两个分析结果，提供综合性的见解。"""
        }
    ]

    result = await self.agent.ernie_model.generate_completion(messages)

    return {
        "fusion_result": result["content"],
        "fusion_goal": fusion_goal,
        "usage": result["usage"]
    }


async def _execute_comparison(self, input_data: Dict[str, Any]) -> Dict[str, Any]:
    """执行比较分析"""

    comparison_items = input_data.get("comparison_items", [])
    comparison_aspect = input_data.get("aspect", "general")

    comparison_text = "\n".join([
        f"项目{i+1}: {item}" for i, item in enumerate(comparison_items)
    ])
```



```python
        messages = [

            {

                "role": "user",

                "content": f"请从{comparison_aspect}角度比较以下项目：\n\n{comparison_text}"

            }

        ]

        result = await self.agent.ernie_model.generate_completion(messages)

        return {

            "comparison_result": result["content"],

            "comparison_aspect": comparison_aspect,

            "items_count": len(comparison_items),

            "usage": result["usage"]

        }

    async def _execute_synthesis(self, input_data: Dict[str, Any]) -> Dict[str, Any]:

        synthesis_inputs = []

        for key, value in input_data.items():

            if key.startswith("dep_") and isinstance(value, dict):

                step_name = key.replace("dep_", "")

                if "analysis_result" in value:

                    synthesis_inputs.append(f"{step_name}: {value['analysis_result']}")

                elif "image_analysis" in value:

                    synthesis_inputs.append(f"{step_name}: {value['image_analysis']}")

                elif "fusion_result" in value:

                    synthesis_inputs.append(f"{step_name}: {value['fusion_result']}")

        synthesis_goal = input_data.get("goal", "综合所有分析结果")

        messages = [

            {

                "role": "user",

                "content": f"""目标：{synthesis_goal}

                需要综合的信息：

                {chr(10).join(synthesis_inputs)}

                请提供一个综合性的结论和建议。"""

            }

        ]

        result = await self.agent.ernie_model.generate_completion(messages)

        return {

            "synthesis_result": result["content"],

            "synthesis_goal": synthesis_goal,

            "input_count": len(synthesis_inputs),

            "usage": result["usage"]

        }

    def _synthesize_final_output(

        self,

        step_results: Dict[str, Any],

        task_template: Dict[str, Any]

    ) -> Dict[str, Any]:

        final_output = {

            "task_description": task_template["description"],

            "execution_summary": {},

            "key_results": {}

        }

        for step_id, result in step_results.items():

            if result.get("success"):

                if "synthesis_result" in result:

                    final_output["key_results"]["final_synthesis"] = result["synthesis_result"]

                elif "fusion_result" in result:

                    final_output["key_results"]["multimodal_fusion"] = result["fusion_result"]

                elif "analysis_result" in result:

                    final_output["key_results"][f"text_analysis_{step_id}"] = result["analysis_result"]

                elif "image_analysis" in result:

                    final_output["key_results"][f"image_analysis_{step_id}"] = result["image_analysis"]

        total_steps = len(step_results)

        successful_steps = sum(1 for r in step_results.values() if r.get("success"))

        total_time = sum(r.get("execution_time", 0) for r in step_results.values())

        final_output["execution_summary"] = {

            "total_steps": total_steps,

            "successful_steps": successful_steps,

            "success_rate": f"{(successful_steps/total_steps)*100:.1f}%",

            "total_execution_time": f"{total_time:.2f}s"

        }

        return final_output

orchestrator = MultimodalTaskOrchestrator(multimodal_agent)

# 定义复杂任务模板

def define_content_analysis_task():

    steps = [

        TaskStep(

            step_id="text_analysis",

            step_type=TaskStepType.TEXT_ANALYSIS,

            input_data={"analysis_type": "sentiment_and_topic"},

            output_requirements={"analysis_result": "string"}

        ),

        TaskStep(

            step_id="image_analysis",

            step_type=TaskStepType.IMAGE_ANALYSIS,

            input_data={"prompt": "分析图像的情感倾向和主要主题"},

            output_requirements={"image_analysis": "string"}

        ),

        TaskStep(

            step_id="multimodal_fusion",

            step_type=TaskStepType.MULTIMODAL_FUSION,

            input_data={"fusion_goal": "综合文本和图像的情感分析"},

            output_requirements={"fusion_result": "string"},

            dependencies=["text_analysis", "image_analysis"]

        ),

        TaskStep(

            step_id="final_synthesis",

            step_type=TaskStepType.SYNTHESIS,

            input_data={"goal": "生成完整的内容分析报告"},

            output_requirements={"synthesis_result": "string"},

            dependencies=["multimodal_fusion"]

        )

    ]

    orchestrator.register_task_template(

        "content_analysis",

        steps,

        "综合分析文本和图像内容的情感倾向和主题"

    )

define_content_analysis_task()

async def demo_complex_task():

    test_text = """

    今天的天气非常好，阳光明媚，让人心情愉悦。

    我在公园里看到了美丽的花朵和绿树，感受到了大自然的魅力。

    这种美好的时光让我想要分享给更多的人。

    """

    test_image = create_test_image()

    input_data = {

        "text": test_text,

        "image_data": test_image

    }

    print("开始执行复杂多模态任务...")

    result = await orchestrator.execute_complex_task("content_analysis", input_data)

    print("\n任务执行完成！")

    print(f"执行摘要: {result['final_output']['execution_summary']}")

    print(f"\n最终结果: {result['final_output']['key_results'].get('final_synthesis', '无')}")

    return result

print("已注册的任务模板:")

for name, template in orchestrator.task_registry.items():

    print(f"- {name}: {template['description']}")

    print(f"  步骤数量: {len(template['steps'])}")

await demo_complex_task()
```

## Agent工具调用与扩展机制

### 3.1 动态工具发现与注册

#### 3.1.1 工具注册系统设计

Agent的强大之处在于其能够动态发现和调用各种外部工具。一个完善的工具注册系统是Agent扩展性的基础。

```python
from abc import ABC, abstractmethod

from typing import Dict, List, Any, Callable, Optional, Type

import inspect

import asyncio

from dataclasses import dataclass, field

from enum import Enum

import json


class ToolCategory(Enum):
    DATA_PROCESSING = "data_processing"
    WEB_SERVICES = "web_services"
    FILE_OPERATIONS = "file_operations"
    CALCULATIONS = "calculations"
    COMMUNICATIONS = "communications"
    MEDIA_PROCESSING = "media_processing"
    SYSTEM_OPERATIONS = "system_operations"


@dataclass
class ToolMetadata:
    name: str
    description: str
    category: ToolCategory
    version: str = "1.0.0"
    author: str = "Unknown"
    requires_auth: bool = False
    rate_limit: Optional[int] = None
    dependencies: List[str] = field(default_factory=list)
```



# 工具基类与注册表

```python
tag: List[str] = field(default_factory=list)

class BaseTool(ABC):
    def __init__(self, metadata: ToolMetadata):
        self.metadata = metadata
        self.usage_count = 0
        self.error_count = 0
        self.total_execution_time = 0.0

    @abstractmethod
    def get_schema(self) -> Dict[str, Any]:
        pass

    @abstractmethod
    async def execute(self, **kwargs) -> Any:
        pass

    def get_statistics(self) -> Dict[str, Any]:
        avg_time = self.total_execution_time / max(self.usage_count, 1)
        return {
            "usage_count": self.usage_count,
            "error_count": self.error_count,
            "error_rate": f"{(self.error_count / max(self.usage_count, 1)) * 100:.2f}%",
            "average_execution_time": f"{avg_time:.3f}s",
            "total_execution_time": f"{self.total_execution_time:.3f}s"
        }

class ToolRegistry:
    def __init__(self):
        self.tools: Dict[str, BaseTool] = {}
        self.categories: Dict[ToolCategory, List[str]] = {
            category: [] for category in ToolCategory
        }
        self.tool_dependencies: Dict[str, List[str]] = {}
        self.registered_callbacks: Dict[str, List[Callable]] = {
            "on_register": [],
            "on_unregister": [],
            "on_execute": []
        }

    def register_tool(self, tool: BaseTool) -> bool:
        tool_name = tool.metadata.name

        if tool_name in self.tools:
            raise ValueError(f"工具 '{tool_name}' 已存在")

        missing_deps = self._validate_dependencies(tool.metadata.dependencies)
        if missing_deps:
            raise ValueError(f"缺少依赖工具: {missing_deps}")

        self.tools[tool_name] = tool
        self.categories[tool.metadata.category].append(tool_name)
        self.tool_dependencies[tool_name] = tool.metadata.dependencies
        self._trigger_callbacks("on_register", tool_name, tool)
        print(f"✓ 工具 '{tool_name}' 注册成功")
        return True

    def unregister_tool(self, tool_name: str) -> bool:
        if tool_name not in self.tools:
            return False

        tool = self.tools[tool_name]
        dependents = self._find_dependents(tool_name)

        if dependents:
            raise ValueError(f"无法注销工具 '{tool_name}'，以下工具依赖它: {dependents}")

        del self.tools[tool_name]
        self.categories[tool.metadata.category].remove(tool_name)
        del self.tool_dependencies[tool_name]
        self._trigger_callbacks("on_unregister", tool_name, tool)
        print(f"✓ 工具 '{tool_name}' 注销成功")
        return True

    def get_tool(self, tool_name: str) -> Optional[BaseTool]:
        return self.tools.get(tool_name)

    def list_tools(self, category: Optional[ToolCategory] = None) -> List[Dict[str, Any]]:
        if category:
            tool_names = self.categories[category]
        else:
            tool_names = list(self.tools.keys())

        return [
            {
                "name": name,
                "metadata": self.tools[name].metadata.__dict__,
                "schema": self.tools[name].get_schema(),
                "statistics": self.tools[name].get_statistics()
            }
            for name in tool_names
        ]

    def search_tools(self, query: str, category: Optional[ToolCategory] = None) -> List[str]:
        results = []
        search_tools = self.categories[category] if category else self.tools.keys()

        for tool_name in search_tools:
            tool = self.tools[tool_name]
            searchable_text = f"{tool.metadata.name} {tool.metadata.description} {' '.join(tool.metadata.tags)}"
            if query.lower() in searchable_text.lower():
                results.append(tool_name)

        return results

    async def execute_tool(self, tool_name: str, **kwargs) -> Dict[str, Any]:
        if tool_name not in self.tools:
            return {"error": f"工具 '{tool_name}' 不存在"}

        tool = self.tools[tool_name]
        start_time = time.time()

        try:
            self._trigger_callbacks("on_execute", tool_name, tool, "before")
            result = await tool.execute(**kwargs)
            execution_time = time.time() - start_time
            tool.usage_count += 1
            tool.total_execution_time += execution_time
            self._trigger_callbacks("on_execute", tool_name, tool, "after")

            return {
                "success": True,
                "result": result,
                "execution_time": execution_time,
                "tool_name": tool_name
            }

        except Exception as e:
            tool.error_count += 1
            tool.usage_count += 1
            return {
                "success": False,
                "error": str(e),
                "execution_time": time.time() - start_time,
                "tool_name": tool_name
            }

    def add_callback(self, event: str, callback: Callable):
        if event in self.registered_callbacks:
            self.registered_callbacks[event].append(callback)

    def _validate_dependencies(self, dependencies: List[str]) -> List[str]:
        missing = []
        for dep in dependencies:
            if dep not in self.tools:
                missing.append(dep)
        return missing

    def _find_dependents(self, tool_name: str) -> List[str]:
        dependents = []
        for name, deps in self.tool_dependencies.items():
            if tool_name in deps:
                dependents.append(name)
        return dependents

    def _trigger_callbacks(self, event: str, *args):
        for callback in self.registered_callbacks.get(event, []):
            try:
                callback(*args)
            except Exception as e:
                print(f"回调执行失败: {e}")

class CalculatorTool(BaseTool):
    def __init__(self):
        metadata = ToolMetadata(
            name="calculator",
            description="执行基本数学计算",
            category=ToolCategory.CALCULATIONS,
            tags=["math", "arithmetic", "calculator"]
        )
        super().__init__(metadata)

    def get_schema(self) -> Dict[str, Any]:
        return {
            "type": "function",
            "function": {
                "name": "calculator",
                "description": "执行数学计算",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "expression": {
                            "type": "string",
                            "description": "数学表达式，如 '2 + 3 * 4'"
                        }
                    }
                }
            }
        }
```



# 工具类定义

## `\`CalculatorTool\``

```python
            }

        }

    }

}

async def execute(self, **kwargs) -> Any:

    expression = kwargs.get("expression", "")

    allowed_chars = "0123456789+-*/.() "

    if not all(c in allowed_chars for c in expression):

        raise ValueError("表达式包含不支持的字符")

    try:

        result = eval(expression)

        return {

            "expression": expression,

            "result": result,

            "type": "number"

        }

    except Exception as e:

        raise ValueError(f"计算错误: {e}")
```

## `\`FileReaderTool\``

```python
class FileReaderTool(BaseTool):

    def __init__(self):

        metadata = ToolMetadata(

            name="file_reader",

            description="读取文本文件内容",

            category=ToolCategory.FILE_OPERATIONS,

            tags=["file", "read", "text"]

        )

        super().__init__(metadata)

    def get_schema(self) -> Dict[str, Any]:

        return {

            "type": "function",

            "function": {

                "name": "file_reader",

                "description": "读取文件内容",

                "parameters": {

                    "type": "object",

                    "properties": {

                        "file_path": {

                            "type": "string",

                            "description": "文件路径"

                        },

                        "encoding": {

                            "type": "string",

                            "description": "文件编码",

                            "default": "utf-8"

                        }

                    },

                    "required": ["file_path"]

                }

            }

        }

    async def execute(self, **kwargs) -> Any:

        file_path = kwargs.get("file_path")

        encoding = kwargs.get("encoding", "utf-8")

        try:

            with open(file_path, 'r', encoding=encoding) as f:

                content = f.read()

            return {

                "file_path": file_path,

                "content": content,

                "size": len(content),

                "encoding": encoding

            }

        except FileNotFoundError:

            raise ValueError(f"文件不存在: {file_path}")

        except Exception as e:

            raise ValueError(f"读取文件失败: {e}")
```

## `\`WebSearchTool\``

```python
class WebSearchTool(BaseTool):

    def __init__(self):

        metadata = ToolMetadata(

            name="web_search",

            description="搜索网络信息",

            category=ToolCategory.WEB_SERVICES,

            rate_limit=10,  # 每秒最多10次请求

            tags=["search", "web", "internet"]

        )

        super().__init__(metadata)

    def get_schema(self) -> Dict[str, Any]:

        return {

            "type": "function",

            "function": {

                "name": "web_search",

                "description": "搜索网络信息",

                "parameters": {

                    "type": "object",

                    "properties": {

                        "query": {

                            "type": "string",

                            "description": "搜索关键词"

                        },

                        "max_results": {

                            "type": "integer",

                            "description": "最大结果数量",

                            "default": 5

                        }

                    },

                    "required": ["query"]

                }

            }

        }

    async def execute(self, **kwargs) -> Any:

        query = kwargs.get("query")

        max_results = kwargs.get("max_results", 5)

        await asyncio.sleep(0.5)

        mock_results = [

            {

                "title": f"关于'{query}'的搜索结果 {i+1}",

                "url": f"https://example.com/result_{i+1}",

                "snippet": f"这是关于{query}的详细信息...",

                "relevance": 0.9 - (i * 0.1)

            }

            for i in range(min(max_results, 5))

        ]

        return {

            "query": query,

            "results": mock_results,

            "total_found": len(mock_results),

            "search_time": "0.5s"

        }
```

## `\`DataAnalysisTool\``

```python
class DataAnalysisTool(BaseTool):

    def __init__(self):

        metadata = ToolMetadata(

            name="data_analyzer",

            description="分析数值数据并生成统计报告",

            category=ToolCategory.DATA_PROCESSING,

            dependencies=["calculator"],  # 依赖计算器工具

            tags=["data", "analysis", "statistics"]

        )

        super().__init__(metadata)

    def get_schema(self) -> Dict[str, Any]:

        return {

            "type": "function",

            "function": {

                "name": "data_analyzer",

                "description": "分析数值数据",

                "parameters": {

                    "type": "object",

                    "properties": {

                        "data": {

                            "type": "array",

                            "items": {"type": "number"},

                            "description": "数值数据列表"

                        },

                        "analysis_type": {

                            "type": "string",

                            "enum": ["basic", "detailed"],

                            "description": "分析类型",

                            "default": "basic"

                        }

                    },

                    "required": ["data"]

                }

            }

        }

    async def execute(self, **kwargs) -> Any:

        data = kwargs.get("data", [])

        analysis_type = kwargs.get("analysis_type", "basic")

        if not data:

            raise ValueError("数据不能为空")

        total = sum(data)

        count = len(data)

        mean = total / count

        minimum = min(data)

        maximum = max(data)

        result = {

            "data_points": count,

            "sum": total,

            "mean": mean,

            "min": minimum,

            "max": maximum,

            "range": maximum - minimum

        }

        if analysis_type == "detailed":

            sorted_data = sorted(data)

            median_idx = count // 2

            median = sorted_data[median_idx] if count % 2 == 1 else (sorted_data[median_idx-1] + sorted_data[median_idx]) / 2

            variance = sum((x - mean) ** 2 for x in data) / count

            std_dev = variance ** 0.5

            result.update({

                "median": median,
```



```markdown
                "variance": variance,
                "standard_deviation": std_dev,
                "sorted_data": sorted_data
            })

        return result

tool_registry = ToolRegistry()

def on_tool_register(tool_name: str, tool: BaseTool):
    print(f"🔧 工具注册: {tool_name} ({tool.metadata.category.value})")

def on_tool_execute(tool_name: str, tool: BaseTool, phase: str):
    if phase == "before":
        print(f"⚡ 执行工具: {tool_name}")

tool_registry.add_callback("on_register", on_tool_register)
tool_registry.add_callback("on_execute", on_tool_execute)

tools_to_register = [
    CalculatorTool(),
    FileReaderTool(),
    WebSearchTool(),
    DataAnalysisTool()
]

for tool in tools_to_register:
    tool_registry.register_tool(tool)

print(f"\n已注册 {len(tool_registry.tools)} 个工具")

async def test_tool_system():
    print("\n=== 工具系统测试 ===")
    all_tools = tool_registry.list_tools()
    print(f"\n可用工具 ({len(all_tools)} 个):")
    for tool_info in all_tools:
        print(f"- {tool_info['name']}: {tool_info['metadata']['description']}")
    print(f"\n测试计算器工具:")
    calc_result = await tool_registry.execute_tool("calculator", expression="15 * 3 + 7")
    print(f"计算结果: {calc_result}")
    print(f"\n测试数据分析工具:")
    test_data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    analysis_result = await tool_registry.execute_tool(
        "data_analyzer", 
        data=test_data, 
        analysis_type="detailed"
    )
    print(f"分析结果: {json.dumps(analysis_result['result'], ensure_ascii=False, indent=2)}")
    print(f"\n测试网络搜索工具:")
    search_result = await tool_registry.execute_tool(
        "web_search", 
        query="人工智能发展趋势", 
        max_results=3
    )
    print(f"搜索结果: {len(search_result['result']['results'])} 条")
    print(f"\n按类别列出工具:")
    for category in ToolCategory:
        category_tools = tool_registry.list_tools(category)
        if category_tools:
            print(f"- {category.value}: {[t['name'] for t in category_tools]}")
    print(f"\n搜索包含'计算'的工具:")
    search_results = tool_registry.search_tools("计算")
    print(f"找到工具: {search_results}")
    print(f"\n工具使用统计:")
    for tool_name in tool_registry.tools:
        stats = tool_registry.tools[tool_name].get_statistics()
        print(f"- {tool_name}: {stats}")

await test_tool_system()
```

#### 3.1.2 智能工具推荐系统

基于`Agent`的历史使用模式和当前任务需求，设计智能工具推荐系统能够显著提升工作效率。

```markdown
from collections import defaultdict, Counter

import math

from typing import Tuple, Set

from datetime import datetime, timedelta

class ToolRecommendationEngine:

    def __init__(self, tool_registry: ToolRegistry):
        self.tool_registry = tool_registry
        self.usage_history: List[Dict[str, Any]] = []
        self.user_preferences: Dict[str, float] = defaultdict(float)
        self.tool_similarities: Dict[Tuple[str, str], float] = {}
        self.context_patterns: Dict[str, List[str]] = defaultdict(list)

    def record_tool_usage(
        self,
        tool_name: str,
        context: str,
        success: bool,
        execution_time: float,
        user_satisfaction: Optional[float] = None
    ):
        usage_record = {
            "tool_name": tool_name,
            "context": context,
            "success": success,
            "execution_time": execution_time,
            "user_satisfaction": user_satisfaction,
            "timestamp": datetime.now()
        }

        self.usage_history.append(usage_record)
        self._update_user_preferences(usage_record)
        self._update_context_patterns(context, tool_name)

        if len(self.usage_history) % 10 == 0:
            self._calculate_tool_similarities()

    def recommend_tools(
        self,
        context: str,
        current_tools: List[str] = None,
        max_recommendations: int = 5
    ) -> List[Dict[str, Any]]:
        recommendations = []
        all_tools = list(self.tool_registry.tools.keys())
        current_tools = current_tools or []

        for tool_name in all_tools:
            if tool_name in current_tools:
                continue
            score = self._calculate_recommendation_score(tool_name, context, current_tools)
            if score > 0:
                tool = self.tool_registry.get_tool(tool_name)
                recommendations.append({
                    "tool_name": tool_name,
                    "score": score,
                    "reason": self._generate_recommendation_reason(tool_name, context, score),
                    "category": tool.metadata.category.value,
                    "description": tool.metadata.description
                })
        recommendations.sort(key=lambda x: x["score"], reverse=True)
        return recommendations[:max_recommendations]

    def get_tool_analytics(self, tool_name: str) -> Dict[str, Any]:
        tool_records = [r for r in self.usage_history if r["tool_name"] == tool_name]
        if not tool_records:
            return {"message": f"工具 {tool_name} 暂无使用记录"}
        total_uses = len(tool_records)
        successful_uses = sum(1 for r in tool_records if r["success"])
        success_rate = successful_uses / total_uses
        execution_times = [r["execution_time"] for r in tool_records]
        avg_execution_time = sum(execution_times) / len(execution_times)
        satisfaction_scores = [r["user_satisfaction"] for r in tool_records if r["user_satisfaction"] is not None]
        avg_satisfaction = sum(satisfaction_scores) / len(satisfaction_scores) if satisfaction_scores else None
        recent_records = [r for r in tool_records if r["timestamp"] > datetime.now() - timedelta(days=7)]
        usage_trend = "增长" if len(recent_records) > total_uses * 0.3 else "稳定" if len(recent_records) > total_uses * 0.1 else "下降"
        contexts = [r["context"] for r in tool_records]
        common_contexts = Counter(contexts).most_common(3)
        return {
            "tool_name": tool_name,
            "usage_statistics": {
                "total_uses": total_uses,
                "success_rate": f"{success_rate:.2%}",
                "average_execution_time": f"{avg_execution_time:.3f}s",
                "average_satisfaction": f"{avg_satisfaction:.2f}" if avg_satisfaction else "N/A"
            },
            "trends": {
                "usage_trend": usage_trend,
                "recent_usage": len(recent_records)
            },
        }
```



```python
"common_contexts": [{"context": ctx, "count": count} for ctx, count in common_contexts],

"user_preference_score": self.user_preferences[tool_name]

}

def _calculate_recommendation_score(
    self,
    tool_name: str,
    context: str,
    current_tools: List[str]
) -> float:
    score = 0.0
    context_score = self._calculate_context_score(tool_name, context)
    score += context_score * 0.4
    preference_score = self.user_preferences[tool_name]
    score += preference_score * 0.3
    similarity_score = self._calculate_similarity_score(tool_name, current_tools)
    score += similarity_score * 0.2
    quality_score = self._calculate_quality_score(tool_name)
    score += quality_score * 0.1
    return score

def _calculate_context_score(self, tool_name: str, context: str) -> float:
    context_words = set(context.lower().split())
    context_usage = self.context_patterns.get(context, [])
    if tool_name in context_usage:
        return context_usage.count(tool_name) / len(context_usage)
    tool = self.tool_registry.get_tool(tool_name)
    if tool:
        tool_keywords = set(tool.metadata.description.lower().split() + tool.metadata.tags)
        keyword_match = len(context_words.intersection(tool_keywords)) / max(len(context_words), 1)
        return keyword_match * 0.5
    return 0.0

def _calculate_similarity_score(self, tool_name: str, current_tools: List[str]) -> float:
    if not current_tools:
        return 0.0
    max_similarity = 0.0
    for current_tool in current_tools:
        similarity = self.tool_similarities.get((tool_name, current_tool), 0.0)
        similarity += self.tool_similarities.get((current_tool, tool_name), 0.0)
        max_similarity = max(max_similarity, similarity)
    return max_similarity

def _calculate_quality_score(self, tool_name: str) -> float:
    tool_records = [r for r in self.usage_history if r["tool_name"] == tool_name]
    if not tool_records:
        return 0.5
    success_rate = sum(1 for r in tool_records if r["success"]) / len(tool_records)
    satisfaction_scores = [r["user_satisfaction"] for r in tool_records if r["user_satisfaction"] is not None]
    avg_satisfaction = sum(satisfaction_scores) / len(satisfaction_scores) if satisfaction_scores else 0.5
    quality_score = (success_rate + avg_satisfaction) / 2
    return quality_score

def _update_user_preferences(self, usage_record: Dict[str, Any]):
    tool_name = usage_record["tool_name"]
    preference_delta = 0.1 if usage_record["success"] else -0.05
    if usage_record["user_satisfaction"]:
        preference_delta += (usage_record["user_satisfaction"] - 0.5) * 0.1
    self.user_preferences[tool_name] += preference_delta
    self.user_preferences[tool_name] = max(0, min(1, self.user_preferences[tool_name]))

def _update_context_patterns(self, context: str, tool_name: str):
    self.context_patterns[context].append(tool_name)
    if len(self.context_patterns[context]) > 100:
        self.context_patterns[context] = self.context_patterns[context][-100:]

def _calculate_tool_similarities(self):
    tools = list(self.tool_registry.tools.keys())
    for i, tool1 in enumerate(tools):
        for j, tool2 in enumerate(tools[i + 1:], i + 1):
            similarity = self._compute_tool_similarity(tool1, tool2)
            self.tool_similarities[(tool1, tool2)] = similarity

def _compute_tool_similarity(self, tool1_name: str, tool2_name: str) -> float:
    tool1 = self.tool_registry.get_tool(tool1_name)
    tool2 = self.tool_registry.get_tool(tool2_name)
    if not tool1 or not tool2:
        return 0.0
    category_similarity = 1.0 if tool1.metadata.category == tool2.metadata.category else 0.0
    tags1 = set(tool1.metadata.tags)
    tags2 = set(tool2.metadata.tags)
    if tags1 or tags2:
        tag_similarity = len(tags1.intersection(tags2)) / len(tags1.union(tags2))
    else:
        tag_similarity = 0.0
    usage_similarity = self._calculate_usage_pattern_similarity(tool1_name, tool2_name)
    total_similarity = (category_similarity * 0.4 + tag_similarity * 0.3 + usage_similarity * 0.3)
    return total_similarity

def _calculate_usage_pattern_similarity(self, tool1_name: str, tool2_name: str) -> float:
    tool1_contexts = set()
    tool2_contexts = set()
    for record in self.usage_history:
        if record["tool_name"] == tool1_name:
            tool1_contexts.add(record["context"])
        elif record["tool_name"] == tool2_name:
            tool2_contexts.add(record["context"])
    if not tool1_contexts or not tool2_contexts:
        return 0.0
    return len(tool1_contexts.intersection(tool2_contexts)) / len(tool1_contexts.union(tool2_contexts))

def _generate_recommendation_reason(self, tool_name: str, context: str, score: float) -> str:
    tool = self.tool_registry.get_tool(tool_name)
    reasons = []
    if score > 0.8:
        reasons.append("高度匹配当前任务需求")
    elif score > 0.6:
        reasons.append("较好匹配当前任务")
    elif score > 0.4:
        reasons.append("可能有助于当前任务")
    if self.user_preferences[tool_name] > 0.7:
        reasons.append("您经常使用此类工具")
    context_usage = self.context_patterns.get(context, [])
    if tool_name in context_usage:
        usage_count = context_usage.count(tool_name)
        reasons.append(f"在类似场景中使用过{usage_count}次")
    return "，".join(reasons) if reasons else f"推荐使用{tool.metadata.category.value}类工具"

recommendation_engine = ToolRecommendationEngine(tool_registry)

async def simulate_usage_history():
    scenarios = [
        ("数据分析任务", ["calculator", "data_analyzer"], [True, True], [0.2, 0.8], [0.9, 0.85]),
        ("文件处理任务", ["file_reader"], [True], [0.3], [0.8]),
        ("研究任务", ["web_search", "data_analyzer"], [True, True], [0.5, 0.6], [0.75, 0.8]),
        ("计算任务", ["calculator"], [True], [0.1], [0.9]),
        ("信息搜集", ["web_search", "file_reader"], [True, False], [0.4, 0.2], [0.8, 0.3])
    ]
    for context, tools, successes, times, satisfactions in scenarios:
        for tool, success, time, satisfaction in zip(tools, successes, times, satisfactions):
            recommendation_engine.record_tool_usage(
                tool_name=tool,
                context=context,
                success=success,
                execution_time=time,
                user_satisfaction=satisfaction
            )
    print("✓ 使用历史数据模拟完成")

async def test_recommendation_system():
    print("\n=== 工具推荐系统测试 ===")
    await simulate_usage_history()
    test_contexts = [
        "需要分析销售数据并生成报告",
        "要搜索最新的技术资料",
        "处理大量文本文件",
        "进行复杂的数学计算"
    ]
    for context in test_contexts:
        print(f"\n上下文: {context}")
        recommendations = recommendation_engine.recommend_tools(context, max_recommendations=3)
        if recommendations:
            print("推荐工具:")
            for i, rec in enumerate(recommendations, 1):
                print(f"  {i}. {rec['tool_name']} (评分: {rec['score']:.3f})")
```



```python
print(f"     理由: {rec['reason']}")

else:

    print("  暂无推荐工具")

print(f"\n=== 工具分析报告 ===")

for tool_name in ["calculator", "data_analyzer", "web_search"]:

    analytics = recommendation_engine.get_tool_analytics(tool_name)

    print(f"\n{tool_name}:")

    print(f"  使用统计: {analytics.get('usage_statistics', {})}")

    print(f"  趋势: {analytics.get('trends', {})}")

    print(f"  常用场景: {[ctx['context'] for ctx in analytics.get('common_contexts', [])]}")

await test_recommendation_system()
```

### 3.2 基于ERNIE的智能工具选择

#### 3.2.1 语义理解驱动的工具匹配

利用ERNIE的强大语义理解能力，Agent可以更准确地理解用户意图并选择合适的工具。

```python
class SemanticToolMatcher:

    def __init__(self, ernie_model: ERNIEModelWrapper, tool_registry: ToolRegistry):

        self.ernie_model = ernie_model

        self.tool_registry = tool_registry

        self.tool_embeddings: Dict[str, List[float]] = {}

        self.intent_patterns: Dict[str, List[str]] = {}

    async def initialize_tool_embeddings(self):

        print("初始化工具语义嵌入...")

        for tool_name, tool in self.tool_registry.tools.items():

            tool_description = self._build_tool_description(tool)

            embedding = await self._generate_semantic_embedding(tool_description)

            self.tool_embeddings[tool_name] = embedding

        print(f"✓ 完成 {len(self.tool_embeddings)} 个工具的语义嵌入")

    async def match_tools_by_intent(

        self,

        user_input: str,

        context: Optional[str] = None,

        max_matches: int = 3

    ) -> List[Dict[str, Any]]:

        """基于用户意图匹配工具"""

        intent_analysis = await self._analyze_user_intent(user_input, context)

        candidate_tools = await self._find_candidate_tools(intent_analysis)

        semantic_matches = await self._calculate_semantic_similarities(

            user_input, candidate_tools

        )

        final_matches = self._rank_tool_matches(

            semantic_matches, intent_analysis, max_matches

        )

        return final_matches

    async def _analyze_user_intent(

        self,

        user_input: str,

        context: Optional[str] = None

    ) -> Dict[str, Any]:

        context_prompt = f"\n上下文信息: {context}" if context else ""

        messages = [

            {

                "role": "system",

                "content": """你是一个意图分析专家，需要分析用户的请求并提取关键信息。

                请分析用户的意图、操作类型、数据类型、期望结果等信息。

                返回JSON格式：

                {

                    "primary_intent": "主要意图",

                    "operation_type": "操作类型",

                    "data_types": ["数据类型列表"],

                    "expected_output": "期望输出",

                    "urgency": "紧急程度(low/medium/high)",

                    "complexity": "复杂度(simple/moderate/complex)",

                    "keywords": ["关键词列表"]

                }"""

            },

            {

                "role": "user",

                "content": f"请分析以下用户请求：{user_input}{context_prompt}"

            }

        ]

        response = await self.ernie_model.generate_completion(messages, temperature=0.1)

        try:

            intent_data = json.loads(response["content"])

            return intent_data

        except:

            return {

                "primary_intent": "未知意图",

                "operation_type": "general",

                "data_types": ["text"],

                "expected_output": "处理结果",

                "urgency": "medium",

                "complexity": "moderate",

                "keywords": user_input.split()[:5]

            }

    async def _find_candidate_tools(self, intent_analysis: Dict[str, Any]) -> List[str]:

        candidates = []

        keywords = intent_analysis.get("keywords", [])

        operation_type = intent_analysis.get("operation_type", "")

        data_types = intent_analysis.get("data_types", [])

        for tool_name, tool in self.tool_registry.tools.items():

            category_match = self._match_category_to_intent(tool.metadata.category, operation_type)

            keyword_match = self._match_keywords_to_tool(keywords, tool)

            datatype_match = self._match_datatypes_to_tool(data_types, tool)

            total_score = category_match + keyword_match + datatype_match

            if total_score > 0.3:

                candidates.append(tool_name)

        return candidates

    async def _calculate_semantic_similarities(

        self,

        user_input: str,

        candidate_tools: List[str]

    ) -> Dict[str, float]:

        similarities = {}

        user_embedding = await self._generate_semantic_embedding(user_input)

        for tool_name in candidate_tools:

            if tool_name in self.tool_embeddings:

                tool_embedding = self.tool_embeddings[tool_name]

                similarity = self._cosine_similarity(user_embedding, tool_embedding)

                similarities[tool_name] = similarity

            else:

                similarities[tool_name] = 0.0

        return similarities

    async def _generate_semantic_embedding(self, text: str) -> List[float]:

        messages = [

            {

                "role": "system",

                "content": """分析文本的语义特征，提取关键概念和主题。

                请返回10个最重要的语义特征词，用逗号分隔。"""

            },

            {

                "role": "user",

                "content": f"分析文本: {text}"

            }

        ]

        response = await self.ernie_model.generate_completion(messages, temperature=0.1)

        semantic_features = response["content"].split(",")[:10]

        embedding = []

        for i in range(100):

            feature_hash = hash(f"{text}_{i}") % 1000

            embedding.append(feature_hash / 1000.0)

        return embedding

    def _cosine_similarity(self, vec1: List[float], vec2: List[float]) -> float:

        if len(vec1) != len(vec2):

            return 0.0

        dot_product = sum(a * b for a, b in zip(vec1, vec2))

        magnitude1 = sum(a * a for a in vec1) ** 0.5

        magnitude2 = sum(b * b for b in vec2) ** 0.5

        if magnitude1 == 0 or magnitude2 == 0:

            return 0.0

        return dot_product / (magnitude1 * magnitude2)

    def _match_category_to_intent(self, category: ToolCategory, operation_type: str) -> float:

        category_mappings = {

            "calculation": [ToolCategory.CALCULATIONS],

            "analysis": [ToolCategory.DATA_PROCESSING, ToolCategory.CALCULATIONS],
```



```python
"search": [ToolCategory.WEB_SERVICES],

"file": [ToolCategory.FILE_OPERATIONS],

"communication": [ToolCategory.COMMUNICATIONS],

"media": [ToolCategory.MEDIA_PROCESSING],

"system": [ToolCategory.SYSTEM_OPERATIONS]

}

for op_type, categories in category_mappings.items():

    if op_type in operation_type.lower() and category in categories:

        return 1.0

return 0.0

def _match_keywords_to_tool(self, keywords: List[str], tool: BaseTool) -> float:

    tool_text = f"{tool.metadata.description} {' '.join(tool.metadata.tags)}".lower()

    matches = 0

    for keyword in keywords:

        if keyword.lower() in tool_text:

            matches += 1

    return matches / max(len(keywords), 1)

def _match_datatypes_to_tool(self, data_types: List[str], tool: BaseTool) -> float:

    datatype_mappings = {

        "number": ["calculator", "data_analyzer"],

        "text": ["file_reader", "web_search"],

        "file": ["file_reader"],

        "web": ["web_search"]

    }

    score = 0.0

    for data_type in data_types:

        if data_type in datatype_mappings:

            if tool.metadata.name in datatype_mappings[data_type]:

                score += 1.0

    return score / max(len(data_types), 1)

def _rank_tool_matches(

    self,

    semantic_matches: Dict[str, float],

    intent_analysis: Dict[str, Any],

    max_matches: int) -> List[Dict[str, Any]]:

    """综合评分并排序工具匹配结果"""

    ranked_matches = []

    for tool_name, semantic_score in semantic_matches.items():

        tool = self.tool_registry.get_tool(tool_name)

        if not tool:

            continue

        base_score = semantic_score * 0.6

        complexity = intent_analysis.get("complexity", "moderate")

        complexity_bonus = self._calculate_complexity_bonus(tool, complexity)

        urgency = intent_analysis.get("urgency", "medium")

        urgency_bonus = self._calculate_urgency_bonus(tool, urgency)

        quality_score = self._get_tool_quality_score(tool_name)

        final_score = base_score + complexity_bonus + urgency_bonus + quality_score

        match_info = {

            "tool_name": tool_name,

            "score": final_score,

            "semantic_similarity": semantic_score,

            "category": tool.metadata.category.value,

            "description": tool.metadata.description,

            "match_reasons": self._generate_match_reasons(

                tool, intent_analysis, semantic_score

            )

        }

        ranked_matches.append(match_info)

    ranked_matches.sort(key=lambda x: x["score"], reverse=True)

    return ranked_matches[:max_matches]

def _calculate_complexity_bonus(self, tool: BaseTool, complexity: str) -> float:

    complexity_scores = {

        "simple": {"calculator": 0.2, "file_reader": 0.2},

        "moderate": {"web_search": 0.1, "data_analyzer": 0.1},

        "complex": {"data_analyzer": 0.2}

    }

    return complexity_scores.get(complexity, {}).get(tool.metadata.name, 0.0)

def _calculate_urgency_bonus(self, tool: BaseTool, urgency: str) -> float:

    if urgency == "high":

        # 高紧急度偏好快速响应的工具

        fast_tools = ["calculator"]

        return 0.1 if tool.metadata.name in fast_tools else 0.0

    return 0.0

def _get_tool_quality_score(self, tool_name: str) -> float:

    """获取工具质量评分"""

    tool = self.tool_registry.get_tool(tool_name)

    stats = tool.get_statistics()

    error_rate = float(stats["error_rate"].rstrip("%")) / 100

    quality_score = (1 - error_rate) * 0.1

    return quality_score

def _generate_match_reasons(

    self,

    tool: BaseTool,

    intent_analysis: Dict[str, Any],

    semantic_score: float) -> List[str]:

    reasons = []

    if semantic_score > 0.8:

        reasons.append("高语义相似度匹配")

    elif semantic_score > 0.6:

        reasons.append("语义相关性较高")

    keywords = intent_analysis.get("keywords", [])

    tool_text = f"{tool.metadata.description} {' '.join(tool.metadata.tags)}".lower()

    matched_keywords = [kw for kw in keywords if kw.lower() in tool_text]

    if matched_keywords:

        reasons.append(f"匹配关键词: {', '.join(matched_keywords[:3])}")

    if tool.metadata.category.value in intent_analysis.get("operation_type", ""):

        reasons.append("工具类别匹配操作需求")

    return reasons

def _build_tool_description(self, tool: BaseTool) -> str:

    description_parts = [

        tool.metadata.description,

        f"类别: {tool.metadata.category.value}",

        f"标签: {', '.join(tool.metadata.tags)}"

    ]

    schema = tool.get_schema()

    if "function" in schema and "parameters" in schema["function"]:

        params = schema["function"]["parameters"].get("properties", {})

        param_names = list(params.keys())

        if param_names:

            description_parts.append(f"参数: {', '.join(param_names)}")

    return ". ".join(description_parts)

semantic_matcher = SemanticToolMatcher(ernie_model, tool_registry)

async def test_semantic_tool_matching():

    print("\n=== 语义工具匹配测试 ===")

    await semantic_matcher.initialize_tool_embeddings()

    test_requests = [

        "我需要计算一下投资回报率",

        "帮我搜索最新的AI技术发展报告",

        "分析这些销售数据的趋势",

        "读取这个配置文件的内容",

        "计算25个数字的平均值和标准差"

    ]

    for request in test_requests:

        print(f"\n用户请求: {request}")

        matches = await semantic_matcher.match_tools_by_intent(request)

        if matches:

            print("推荐工具:")

            for i, match in enumerate(matches, 1):

                print(f"  {i}. {match['tool_name']} (评分: {match['score']:.3f})")

                print(f"     语义相似度: {match['semantic_similarity']:.3f}")

                print(f"     匹配理由: {', '.join(match['match_reasons'])}")

        else:

            print("  未找到匹配的工具")

await test_semantic_tool_matching()
```

### 3.2.2 多轮对话中的工具链编排

在复杂的多轮对话中，Agent需要能够根据对话历史和上下文动态调整工具使用策略。

```python
class ConversationalToolOrchestrator:

    def __init__(
        self,
        ernie_model: ERNIEModelWrapper,
        tool_registry: ToolRegistry,
        semantic_matcher: SemanticToolMatcher
    ):
        self.ernie_model = ernie_model
        self.tool_registry = tool_registry
        self.semantic_matcher = semantic_matcher
        self.conversation_history: List[Dict[str, Any]] = []

        self.tool_execution_chain: List[Dict[str, Any]] = []

        self.conversation_context: Dict[str, Any] = {}
```



```python
async def process_conversational_request(
    self,
    user_message: str,
    session_id: str = "default"
) -> Dict[str, Any]:
    self._update_conversation_history(user_message, session_id)

    context_analysis = await self._analyze_conversation_context(user_message, session_id)

    execution_strategy = await self._determine_execution_strategy(context_analysis)

    execution_result = await self._execute_tool_chain(execution_strategy)

    conversation_response = await self._generate_conversation_response(
        user_message, execution_result, context_analysis
    )

    self._update_conversation_state(conversation_response, execution_result)

    return {
        "user_message": user_message,
        "response": conversation_response,
        "execution_result": execution_result,
        "context_analysis": context_analysis,
        "session_id": session_id,
        "timestamp": datetime.now().isoformat()
    }


async def _analyze_conversation_context(
    self,
    current_message: str,
    session_id: str
) -> Dict[str, Any]:
    session_history = [
        item for item in self.conversation_history[-10:]  # 最近10轮对话
        if item.get("session_id") == session_id
    ]
    history_text = "\n".join([
        f"用户: {item['user_message']}\n助手: {item.get('assistant_response', '')}"
        for item in session_history
    ])
    previous_tools = [
        item["tool_name"] for item in self.tool_execution_chain[-5:]
        if item.get("session_id") == session_id
    ]
    messages = [
        {
            "role": "system",
            "content": """你是一个对话上下文分析专家。分析用户的当前请求及对话历史，判断：

            1. 当前请求的意图类型
            2. 是否需要使用工具
            3. 与之前对话的关联性
            4. 需要的工具类型和执行顺序

            返回JSON格式：

            {
                "intent_type": "新任务/延续任务/修正任务/查询结果",
                "needs_tools": true/false,
                "continuation_of": "相关的前序任务ID或null",
                "required_tool_types": ["工具类型列表"],
                "execution_mode": "单步/序列/并行",
                "context_dependencies": ["依赖的上下文信息"],
                "expected_output_type": "期望输出类型"
            }"""
        },
        {
            "role": "user",
            "content": f"""当前用户请求: {current_message}

            对话历史:

            {history_text}

            最近使用的工具: {previous_tools}

            请分析当前请求的上下文。"""
        }
    ]

    response = await self.ernie_model.generate_completion(messages, temperature=0.2)

    try:
        context_data = json.loads(response["content"])
    except:
        context_data = {
            "intent_type": "新任务",
            "needs_tools": True,
            "continuation_of": None,
            "required_tool_types": ["general"],
            "execution_mode": "单步",
            "context_dependencies": [],
            "expected_output_type": "text"
        }

    return context_data


async def _determine_execution_strategy(
    self,
    context_analysis: Dict[str, Any]
) -> Dict[str, Any]:
    strategy = {
        "mode": context_analysis.get("execution_mode", "单步"),
        "tools": [],
        "dependencies": {},
        "fallback_plan": None
    }

    if not context_analysis.get("needs_tools", False):
        strategy["mode"] = "direct_response"
        return strategy

    intent_type = context_analysis.get("intent_type", "新任务")

    if intent_type == "新任务":
        current_message = self.conversation_history[-1]["user_message"]
        matched_tools = await self.semantic_matcher.match_tools_by_intent(
            current_message, max_matches=3
        )
        strategy["tools"] = [tool["tool_name"] for tool in matched_tools]

    elif intent_type == "延续任务":
        continuation_id = context_analysis.get("continuation_of")
        if continuation_id:
            related_tools = self._find_related_tools(continuation_id)
            strategy["tools"] = related_tools

    elif intent_type == "修正任务":
        last_failed_tool = self._find_last_failed_tool()
        if last_failed_tool:
            alternative_tools = self._find_alternative_tools(last_failed_tool)
            strategy["tools"] = alternative_tools

    if strategy["mode"] == "序列":
        strategy["dependencies"] = self._create_sequential_dependencies(strategy["tools"])
    elif strategy["mode"] == "并行":
        strategy["dependencies"] = {}

    return strategy


async def _execute_tool_chain(self, strategy: Dict[str, Any]) -> Dict[str, Any]:
    if strategy["mode"] == "direct_response":
        return {"mode": "direct", "results": []}

    execution_results = []
    execution_order = self._resolve_execution_order(strategy)

    for tool_name in execution_order:
        try:
            tool_params = await self._prepare_tool_parameters(
                tool_name, execution_results
            )
            result = await self.tool_registry.execute_tool(tool_name, **tool_params)
            execution_record = {
                "tool_name": tool_name,
                "parameters": tool_params,
                "result": result,
                "timestamp": datetime.now().isoformat(),
                "session_id": self.conversation_history[-1].get("session_id")
            }

            execution_results.append(execution_record)
            self.tool_execution_chain.append(execution_record)

            if not result.get("success", True) and strategy.get("fallback_plan"):
                fallback_result = await self._execute_fallback_plan(
                    strategy["fallback_plan"], tool_name
                )
                if fallback_result:
                    execution_results.append(fallback_result)

        except Exception as e:
            error_record = {
                "tool_name": tool_name,
                "error": str(e),
```



```python
"timestamp": datetime.now().isoformat(),

"session_id": self.conversation_history[-1].get("session_id")

}

execution_results.append(error_record)

return {

    "mode": strategy["mode"],

    "results": execution_results,

    "total_tools": len(execution_order),

    "successful_tools": sum(1 for r in execution_results if r.get("result", {}).get("success", False))

}

async def _prepare_tool_parameters(

    self,

    tool_name: str,

    previous_results: List[Dict[str, Any]]

) -> Dict[str, Any]:

    current_message = self.conversation_history[-1]["user_message"]

    messages = [

        {

            "role": "system",

            "content": f"""从用户请求中提取工具 {tool_name} 所需的参数。

            工具参数模式:

            {json.dumps(self.tool_registry.get_tool(tool_name).get_schema(), ensure_ascii=False)}

            如果之前有工具执行结果，也要考虑利用这些结果。

            只返回参数的JSON对象，不要其他文字。"""

        },

        {

            "role": "user",

            "content": f"""用户请求: {current_message}

            之前的工具执行结果:

            {json.dumps(previous_results, ensure_ascii=False)}

            请提取 {tool_name} 工具的参数。"""

        }

    ]

    response = await self.ernie_model.generate_completion(messages, temperature=0.1)

    try:

        parameters = json.loads(response["content"])

        return parameters

    except:

        return {"input": current_message}

async def _generate_conversation_response(

    self,

    user_message: str,

    execution_result: Dict[str, Any],

    context_analysis: Dict[str, Any]

) -> str:

    """生成对话响应"""

    if execution_result["mode"] == "direct":

        messages = [

            {

                "role": "user",

                "content": user_message

            }

        ]

        response = await self.ernie_model.generate_completion(messages)

        return response["content"]

    tool_results_summary = []

    for result in execution_result["results"]:

        if "result" in result and result["result"].get("success"):

            tool_results_summary.append({

                "tool": result["tool_name"],

                "output": result["result"]["result"]

            })

        else:

            tool_results_summary.append({

                "tool": result["tool_name"],

                "error": result.get("error", "执行失败")

            })

    messages = [

        {

            "role": "system",

            "content": """基于工具执行结果为用户生成友好、有用的回复。

            回复应该：

            1. 直接回答用户的问题

            2. 总结工具执行的结果

            3. 如果有错误，给出建议

            4. 保持对话的连贯性"""

        },

        {

            "role": "user",

            "content": f"""用户问题: {user_message}

            工具执行结果:

            {json.dumps(tool_results_summary, ensure_ascii=False, indent=2)}

            请生成合适的回复。"""

        }

    ]

    response = await self.ernie_model.generate_completion(messages, temperature=0.6)

    return response["content"]

def _update_conversation_history(self, user_message: str, session_id: str):

    """更新对话历史"""

    self.conversation_history.append({

        "user_message": user_message,

        "session_id": session_id,

        "timestamp": datetime.now().isoformat()

    })

    if len(self.conversation_history) > 100:

        self.conversation_history = self.conversation_history[-100:]

def _update_conversation_state(

    self,

    response: str,

    execution_result: Dict[str, Any]

):

    if self.conversation_history:

        self.conversation_history[-1]["assistant_response"] = response

        self.conversation_history[-1]["execution_summary"] = {

            "tools_used": [r["tool_name"] for r in execution_result.get("results", [])],

            "success_count": execution_result.get("successful_tools", 0)

        }

def _resolve_execution_order(self, strategy: Dict[str, Any]) -> List[str]:

    tools = strategy["tools"]

    dependencies = strategy.get("dependencies", {})

    if strategy["mode"] == "并行":

        return tools

    elif strategy["mode"] == "序列":

        return tools

    else:

        return tools[:1] if tools else []

def _create_sequential_dependencies(self, tools: List[str]) -> Dict[str, List[str]]:

    dependencies = {}

    for i, tool in enumerate(tools[1:], 1):

        dependencies[tool] = [tools[i-1]]

    return dependencies

def _find_related_tools(self, task_id: str) -> List[str]:

    recent_tools = [

        item["tool_name"] for item in self.tool_execution_chain[-3:]

        if item.get("continuation_of") == task_id

    ]

    return recent_tools[:2]

def _find_last_failed_tool(self) -> Optional[str]:

    for item in reversed(self.tool_execution_chain):

        if not item.get("result", {}).get("success", True):

            return item["tool_name"]

    return None

def _find_alternative_tools(self, failed_tool: str) -> List[str]:

    failed_tool_obj = self.tool_registry.get_tool(failed_tool)

    if not failed_tool_obj:

        return []

    alternatives = []

    for tool_name, tool in self.tool_registry.tools.items():

        if (tool_name != failed_tool and

            tool.metadata.category == failed_tool_obj.metadata.category):

            alternatives.append(tool_name)

    return alternatives[:2]

async def _execute_fallback_plan(

    self,

    fallback_plan: Dict[str, Any],

    failed_tool: str

) -> Optional[Dict[str, Any]]:

    alternative_tools = self._find_alternative_tools(failed_tool)

    if alternative_tools:

        alt_tool = alternative_tools[0]

        try:

            result = await self.tool_registry.execute_tool(alt_tool)

            return {

                "tool_name": alt_tool,

                "result": result,

                "is_fallback": True,

                "timestamp": datetime.now().isoformat()

            }

        except:

            pass

    return None

conversational_orchestrator = ConversationalToolOrchestrator(

    ernie_model, tool_registry, semantic_matcher

)

async def test_conversational_orchestration():

    print("\n=== 多轮对话工具编排测试 ===")

    conversation_scenarios = [

        ("我想分析一些销售数据", "session_1"),

        ("计算一下这些数字的平均值：10, 20, 30, 40, 50", "session_1"),

        ("刚才计算的结果能详细分析一下吗？", "session_1"),

        ("搜索一下最新的市场趋势报告", "session_2"),
```



```
("能把刚才搜索的结果总结一下吗？", "session_2")
]

for message, session in conversation_scenarios:
    print(f"\n会话 {session}")
    print(f"用户: {message}")
    result = await conversational_orchestrator.process_conversational_request(
        message, session
    )
    print(f"助手: {result['response']}")
    if result['execution_result']['results']:
        print(f"使用的工具: {[r['tool_name'] for r in result['execution_result']['results']]}")
    print(f"上下文分析: {result['context_analysis']['intent_type']}")

await test_conversational_orchestration()
```

## 总结

本教程完整展示了基于ERNIE大模型和MCP协议的Agent智能代理开发与部署的全流程。从理论基础到实际实现，从工具扩展到生产部署，涵盖了Agent开发的各个关键环节。

### 核心技术要点

- Agent架构设计：采用混合式架构，结合反应式和深思式处理模式
- ERNIE模型集成：多模态能力，参数优化，性能监控
- MCP协议应用：标准化工具接口，动态工具发现和注册
- 智能工具编排：语义理解驱动的工具选择，多轮对话工具链管理
- 生产级部署：微服务架构，容器化部署，全链路监控

### 实践应用价值

- 提升开发效率：标准化的工具接口和智能编排机制
- 增强用户体验：多模态交互和上下文感知能力
- 保障系统稳定：完善的监控告警和性能优化机制
- 支持业务扩展：灵活的架构设计和部署方案

通过本教程的学习和实践，开发者可以构建出功能强大、性能优异、部署便捷的企业级Agent应用系统。

---

本智能篇教程涵盖了AI Agent的完整应用，从基础理论到生产实践，为构建企业级AI Agent应用提供了全面的技术指导。继续学习后续的协作篇-A2A多Agent协同与编排章节，将进一步提升您的AI应用开发能力。
