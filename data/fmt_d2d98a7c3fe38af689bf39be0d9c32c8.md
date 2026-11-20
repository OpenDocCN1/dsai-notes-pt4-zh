
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
