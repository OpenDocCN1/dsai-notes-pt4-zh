
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
