
# React Agent 实现指南

### 3.1 React Agent 类定义

```
class ReactAgent:
    def __init__(self, api_key: str = '') -> None:
        """初始化 React Agent"""
        self.api_key = api_key
        self.tools = Tools()                    # 工具管理
        self.model = Siliconflow(api_key)      # LLM 客户端
        self.system_prompt = self._build_system_prompt()
    
    def _build_system_prompt(self) -> str:
        """构建 ReAct 系统提示"""
        # 组合工具描述和 ReAct 模式指导
        return prompt_template
    
    def _parse_action(self, text: str) -> tuple[str, dict]:
        """解析模型输出中的行动和参数"""
        # 使用正则表达式提取行动和参数
        action_pattern = r"行动[:：]\s*(\w+)"
        action_input_pattern = r"行动输入[:：]\s*({.*?}|[^\n]*)"
        return action, action_input_dict
    
    def _execute_action(self, action: str, action_input: dict) -> str:
        """执行指定的工具行动"""
        # 调用对应工具并返回结果
        
    def run(self, query: str, max_iterations: int = 3, verbose: bool = True) -> str:
        """运行 ReAct Agent 主循环"""
        # 实现思考-行动-观察循环
```

### 3.2 系统提示构建

#### 为什么系统提示如此重要？

系统提示是`React Agent`的"大脑"，它直接决定了`Agent`的行为模式。一个好的系统提示应该包含：

1. 时间信息：让 Agent 知道当前时间，避免过时信息

1. 工具清单：明确告诉 Agent 有哪些工具可用

1. 行为模式：详细的 ReAct 流程指导

1. 输出格式：规范化的思考-行动-观察格式

#### 构建思路

我们使用`f-string`动态生成系统提示，这样可以：

+ 自动包含当前时间

+ 动态加载可用工具列表

+ 保持提示的时效性和准确性

#### 系统提示模板解析

```
prompt = f"""现在时间是 {time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())}。
你是一位智能助手，可以使用以下工具来回答问题：

{tool_descriptions}

请遵循以下 ReAct 模式：

思考：分析问题和需要使用的工具
行动：选择工具 [google_search] 中的一个
行动输入：提供工具的参数
观察：工具返回的结果

你可以重复以上循环，直到获得足够的信息来回答问题。

最终答案：基于所有信息给出最终答案

开始！"""
```

#### 设计要点

+ 中文提示：更符合国内用户习惯

+ 具体工具名：明确告诉模型可用工具

+ 循环指导：说明可以多次使用工具

+ 最终答案：明确结束条件

```
prompt = f"""现在时间是 {time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())}。
你是一位智能助手，可以使用以下工具来回答问题：

{tool_descriptions}

请遵循以下 ReAct 模式：

思考：分析问题和需要使用的工具
行动：选择工具 [google_search] 中的一个
行动输入：提供工具的参数
观察：工具返回的结果

你可以重复以上循环，直到获得足够的信息来回答问题。

最终答案：基于所有信息给出最终答案

开始！"""
```

### 3.3 行动解析机制

#### 为什么解析这么复杂？

在实际应用中，大模型的输出格式往往不够规范，可能出现：

+ 中英文混合的冒号

+ JSON格式不规范

+ 参数缺失或格式错误

+ 多余的空格或换行

因此我们需要一个鲁棒的解析机制。

#### 解析思路

1. 多层匹配：先用正则提取，再用JSON解析

1. 容错设计：解析失败时提供降级方案

1. 格式兼容：支持JSON字符串和纯文本参数

#### 解析流程详解

```
模型输出：
思考：用户询问特朗普生日，需要搜索
行动：google_search
行动输入：{"search_query": "特朗普生日"}

解析步骤：
1. 正则提取行动 → google_search
2. 正则提取参数 → {"search_query": "特朗普生日"}
3. JSON解析 → {'search_query': '特朗普生日'}
4. 返回结构化数据 → ('google_search', {'search_query': '特朗普生日'})
```

#### 常见错误场景

+ ❌ 行动输入：特朗普生日 → 自动转为 `{"search_query": "特朗普生日"}`

+ ❌ 行动输入：{"search_query":"特朗普生日"} → 补全JSON格式

+ ❌ 行动输入："特朗普生日" → 去除多余引号

#### 代码实现

```
def _parse_action(self, text: str, verbose: bool = False) -> tuple[str, dict]:
    """从文本中解析行动和行动输入"""
    # 更灵活的正则表达式模式
    action_pattern = r"行动[:：]\s*(\w+)"
    action_input_pattern = r"行动输入[:：]\s*({.*?}|\{.*?\}|[^\n]*)"
    
    action_match = re.search(action_pattern, text, re.IGNORECASE)
    action_input_match = re.search(action_input_pattern, text, re.DOTALL)
    
    action = action_match.group(1).strip() if action_match else ""
    action_input_str = action_input_match.group(1).strip() if action_input_match else ""
    
    # 清理和解析JSON
    action_input_dict = {}
    if action_input_str:
        try:
            action_input_str = action_input_str.strip()
            if action_input_str.startswith('{') and action_input_str.endswith('}'):
                action_input_dict = json5.loads(action_input_str)
            else:
                # 如果不是JSON格式，尝试解析为简单字符串参数
                action_input_dict = {"search_query": action_input_str.strip("'\"")}
        except Exception as e:
            action_input_dict = {"search_query": action_input_str.strip("'\"")}
    
    return action, action_input_dict
```

### 3.4 ReAct 主循环

#### 什么是 ReAct 循环？

`ReAct`循环是 Agent 的"心跳"，它让 Agent 能够：

+ 持续思考：基于新信息不断调整策略

+ 工具调用：在需要时主动获取外部信息

+ 结果整合：将工具结果与已有知识结合

#### 循环的四个阶段

1. 思考阶段（Thought）：模型分析问题，决定是否需要工具

1. 行动阶段（Action）：选择合适的工具并提供参数

1. 观察阶段（Observation）：执行工具并获取结果

1. 整合阶段（Integration）：将新信息整合到上下文中

#### 为什么需要 max_iterations？

+ 防止无限循环：避免模型陷入死循环

+ 控制成本：限制 API 调用次数

+ 用户体验：避免过长的响应时间

#### 状态管理的重要性

每次循环都需要：

+ 保持上下文：将观察结果加入对话历史

+ 更新输入：为下一轮循环准备新的提示

+ 历史记录：确保模型知道之前做了什么

#### 循环逻辑详解

```
def run(self, query: str, max_iterations: int = 3, verbose: bool = True) -> str:
    conversation_history = []
    current_text = f"问题：{query}"
    
    for iteration in range(max_iterations):
        # 获取模型响应
        response, history = self.model.chat(current_text, conversation_history, self.system_prompt)
        
        # 解析行动
        action, action_input = self._parse_action(response, verbose=verbose)
        
        if not action or action == "最终答案":
            return self._format_response(response)
        
        # 执行行动
        observation = self._execute_action(action, action_input)
        
        # 更新上下文继续对话
        current_text = f"{response}\n观察结果:{observation}\n"
        conversation_history = history
    
    return self._format_response(response)
```

### 3.5 完整使用示例

#### 环境准备

在开始之前，请确保：

1. 已安装必要的库：`pip install openai json5 requests`

1. 已获取 `Siliconflow` API key（注册后免费获取）

1. 已获取 `Serper` API key（注册后免费获取2500次调用）

#### 分步骤运行

#### 第一步：基础测试

```
# 测试工具是否正常工作
from tool import ReactTools

tools = ReactTools()
result = tools.google_search("Python最新版本")
print("工具测试：", result[:100] + "...")
```

#### 第二步：Agent初始化

```
# 初始化 React Agent
agent = ReactAgent(api_key="your_api_key")

# 测试简单查询（无需工具）
response = agent.run("你好，请自我介绍", max_iterations=1)
print("简单查询：", response)
```

#### 第三步：复杂查询

```
# 测试需要工具的查询
response = agent.run("美国最近一次阅兵的原因有哪些？", max_iterations=3, verbose=True)
print("\n" + "="*50)
print("最终答案：", response)
```

#### 调试技巧

1. 查看详细日志：设置`verbose=True`查看完整流程

1. 减少迭代次数：测试时使用`max_iterations=1`快速验证

1. 检查API响应：确保网络连接和API key有效

1. 错误排查：观察绿色输出中的每一步执行状态

#### 输出示例

```
[ReAct Agent] 开始处理问题: 美国最近一次阅兵的原因有哪些？
[ReAct Agent] 第 1 次思考...
[ReAct Agent] 模型响应:
思考：用户询问美国最近一次阅兵的原因，我需要搜索相关信息
行动：google_search
行动输入：{"search_query": "美国最近一次阅兵 原因 2025"}
[ReAct Agent] 执行行动: google_search | 参数: {'search_query': '美国最近一次阅兵 原因 2025'}
[ReAct Agent] 观察结果:
1. **美国最近一次阅兵...**
...
[ReAct Agent] 已获得最终答案
最终答案：美国最近一次阅兵的主要原因包括...
```

![](img/937b1ed52d1b2a4f6de214e5e6074f3f.png)
