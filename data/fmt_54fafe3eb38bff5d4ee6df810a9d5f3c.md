
```
COMPLEX_PROMPT = """你是旅行规划助手。你需要：
1. 使用maps_text_search搜索景点，关键词根据用户偏好确定
2. 使用maps_weather查询天气,获取未来几天的天气预报
3. 使用maps_text_search搜索酒店,类型根据用户需求确定
4. 整合所有信息生成旅行计划,包括每天的景点、餐饮、住宿安排
注意：必须按顺序执行,每个工具只能调用一次,输出必须是JSON格式...
"""
```

这样的提示词有几个问题。首先是难以维护。如果我们想修改景点搜索的逻辑（比如增加评分筛选），就需要修改整个提示词，很容易影响到其他部分。其次是容易出错。LLM 需要同时理解多个任务的要求，很容易搞混不同任务的格式和参数。最后是难以调试。当生成的计划不符合预期时，我们很难知道是哪个环节出了问题，是景点搜索不准确，还是天气查询失败，还是整合逻辑有问题？

面对这些问题，一个自然的想法是：能不能把复杂的任务分解成多个简单的任务，让不同的 Agent 各司其职？这就是多 Agent 协作的核心思想。

想象一下现实世界中的旅行社。当你去旅行社咨询旅行计划时，不会只有一个人为你服务。通常会有专门的景点顾问，负责推荐景点；有酒店顾问，负责预订酒店；还有行程规划师，负责把所有信息整合成完整的行程。每个人都专注于自己擅长的领域，最后由行程规划师把所有信息汇总。这种分工协作的方式，比让一个人做所有事情要高效得多。

### 13.3.2 Agent 角色设计

基于任务分解原则，我们设计了四个专门的 Agent，如图 13.6 所示:

![](img/a321c8430dd92eeb16099be6e6e741bd.png)

图 13.6 多智能体协作流程

+ `AttractionSearchAgent`(景点搜索专家)专注于搜索景点信息。它只需要理解用户的偏好（比如"历史文化"、"自然风光"），然后调用高德地图的 POI 搜索工具，返回相关的景点列表。它的提示词很简单，只需要说明如何根据偏好选择关键词，如何调用工具。

+ `WeatherQueryAgent`(天气查询专家)专注于查询天气信息。它只需要知道城市名称，然后调用天气查询工具，返回未来几天的天气预报。它的任务非常明确，几乎不会出错。

+ `HotelAgent`(酒店推荐专家)专注于搜索酒店信息。它需要理解用户的住宿需求（比如"经济型"、"豪华型"），然后调用 POI 搜索工具，返回符合要求的酒店列表。

+ `PlannerAgent`(行程规划专家)负责整合所有信息。它接收前三个 Agent 的输出，加上用户的原始需求（日期、预算等），然后生成完整的旅行计划。它不需要调用任何外部工具，只需要专注于信息的整合和行程的安排。

现在让我们详细设计每个 Agent 的角色和提示词。设计提示词时，我们需要考虑几个关键问题：这个 Agent 需要什么输入？它应该产生什么输出？它需要调用什么工具？它可能遇到什么问题？

`AttractionSearchAgent`的任务是根据用户偏好搜索景点。它的输入是城市名称和用户偏好（比如"历史文化"、"自然风光"）。它需要调用`amap_maps_text_search`工具，参数是关键词和城市。它的输出是景点列表，包含名称、地址、评分等信息。

```
ATTRACTION_AGENT_PROMPT = """你是景点搜索专家。

**工具调用格式:**
`[TOOL_CALL:amap_maps_text_search:keywords=景点,city=城市名]`

**示例:**
- `[TOOL_CALL:amap_maps_text_search:keywords=景点,city=北京]`
- `[TOOL_CALL:amap_maps_text_search:keywords=博物馆,city=上海]`

**重要:**
- 必须使用工具搜索,不要编造信息
- 根据用户偏好({preferences})搜索{city}的景点
"""
```

这个提示词很简洁，但包含了所有必要的信息。它明确说明了工具调用的格式，提供了具体的示例，还强调了两个重要原则：必须使用工具（不能编造），要根据用户偏好搜索。

`WeatherQueryAgent`的任务更简单，只需要查询天气。它的输入是城市名称，输出是天气信息。

```
WEATHER_AGENT_PROMPT = """你是天气查询专家。

**工具调用格式:**
`[TOOL_CALL:amap_maps_weather:city=城市名]`

请查询{city}的天气信息。
"""
```

`HotelAgent`的任务是搜索酒店。它的输入是城市名称和住宿类型，输出是酒店列表。

```
HOTEL_AGENT_PROMPT = """你是酒店推荐专家。

**工具调用格式:**
`[TOOL_CALL:amap_maps_text_search:keywords=酒店,city=城市名]`

请搜索{city}的{accommodation}酒店。
"""
```

`PlannerAgent`是最复杂的，因为它需要整合所有信息。它的输入是用户需求和前三个 Agent 的输出，输出是完整的旅行计划(JSON 格式)。

```
PLANNER_AGENT_PROMPT = """你是行程规划专家。

**输出格式:**
严格按照以下JSON格式返回:
{
  "city": "城市名称",
  "start_date": "YYYY-MM-DD",
  "end_date": "YYYY-MM-DD",
  "days": [...],
  "weather_info": [...],
  "overall_suggestions": "总体建议",
  "budget": {...}
}

**规划要求:**
1. weather_info必须包含每天的天气
2. 温度为纯数字(不带°C)
3. 每天安排2-3个景点
4. 考虑景点距离和游览时间
5. 包含早中晚三餐
6. 提供实用建议
7. 包含预算信息
"""
```

### 13.3.3 Agent 协作流程

现在让我们看看这四个 Agent 如何协作完成旅行规划任务。整个流程可以分为五个步骤：

```
class TripPlannerAgent:
    def __init__(self):
        self.attraction_agent = SimpleAgent(name="景点搜索", prompt=ATTRACTION_PROMPT)
        self.weather_agent = SimpleAgent(name="天气查询", prompt=WEATHER_PROMPT)
        self.hotel_agent = SimpleAgent(name="酒店推荐", prompt=HOTEL_PROMPT)
        self.planner_agent = SimpleAgent(name="行程规划", prompt=PLANNER_PROMPT)

    def plan_trip(self, request: TripPlanRequest) -> TripPlan:
        # 步骤1: 景点搜索
        attraction_response = self.attraction_agent.run(f"请搜索{request.city}的{request.preferences}景点")
        # 步骤2: 天气查询
        weather_response = self.weather_agent.run(f"请查询{request.city}的天气")
        # 步骤3: 酒店推荐
        hotel_response = self.hotel_agent.run(f"请搜索{request.city}的{request.accommodation}酒店")
        # 步骤4: 整合生成计划
        planner_query = self._build_planner_query(
            request, attraction_response, weather_response, hotel_response
        )
        planner_response = self.planner_agent.run(planner_query)
        # 步骤5: 解析JSON
        trip_plan = self._parse_trip_plan(planner_response)
        return trip_plan
```

这个流程顺序执行四个步骤，每个步骤的输出作为下一个步骤的输入。注意我们使用了`TripPlanRequest`和`TripPlan`这两个 Pydantic 模型，这是在 13.2 节中定义的。

### [13.3.4 查询构建](https://datawhalechina.github.io/hello-agents/#/./chapter13/%E7%AC%AC%E5%8D%81%E4%B8%89%E7%AB%A0%20%E6%99%BA%E8%83%BD%E6%97%85%E8%A1%8C%E5%8A%A9%E6%89%8B?id=_1334-%e6%9f%a5%e8%af%a2%e6%9e%84%e5%bb%ba)

`PlannerAgent` 需要整合所有信息，这个查询需要包含所有必要的信息，而且要组织得清晰有序，让 LLM 能够准确理解。

```
def _build_planner_query(
    self,
    request: TripPlanRequest,
    attraction_response: str,
    weather_response: str,
    hotel_response: str) -> str:
    """构建规划Agent的查询"""
    return f"""
请根据以下信息生成{request.city}的{request.days}日旅行计划:

**用户需求:**
- 目的地: {request.city}
- 日期: {request.start_date} 至 {request.end_date}
- 天数: {request.days}天
- 偏好: {request.preferences}
- 预算: {request.budget}
- 交通方式: {request.transportation}
- 住宿类型: {request.accommodation}

**景点信息:**
{attraction_response}

**天气信息:**
{weather_response}

**酒店信息:**
{hotel_response}

请生成详细的旅行计划,包括每天的景点安排、餐饮推荐、住宿信息和预算明细。
"""
```

通过这种多 Agent 协作的设计，我们把一个复杂的旅行规划任务分解成了四个简单的子任务。每个 Agent 都专注于自己擅长的领域，也为未来的功能扩展（比如添加餐厅推荐 Agent、交通规划 Agent）打下了良好的基础。

## [13.4 MCP 工具集成详解](https://datawhalechina.github.io/hello-agents/#/./chapter13/%E7%AC%AC%E5%8D%81%E4%B8%89%E7%AB%A0%20%E6%99%BA%E8%83%BD%E6%97%85%E8%A1%8C%E5%8A%A9%E6%89%8B?id=_134-mcp-%e5%b7%a5%e5%85%b7%e9%9b%86%e6%88%90%e8%af%a6%e8%a7%a3)

### 13.4.1 为什么不直接调用 API

在 13.3 节中，我们设计了四个 Agent 来协作完成旅行规划任务。其中 `AttractionSearchAgent`、`WeatherQueryAgent` 和 `HotelAgent` 都需要调用高德地图的 API 来获取数据。一个自然的问题是：为什么不直接在 Agent 中调用高德地图的 HTTP API？

让我们先看看直接调用 API 会是什么样子。高德地图提供了 POI 搜索 API，我们需要构造 HTTP 请求，传递参数，解析响应：

```
import requests

def search_poi(keywords: str, city: str, api_key: str):
    """直接调用高德地图POI搜索API"""
    url = "https://restapi.amap.com/v3/place/text"
    params = {"keywords": keywords, "city": city, "key": api_key, "output": "json"}
    response = requests.get(url, params=params)
    data = response.json()
    return data
```

这种方式看起来很简单，但在实际使用中会遇到几个问题。首先是 Agent 无法自主调用。在我们的 HelloAgents 框架中，Agent 通过识别提示词中的工具调用标记（如`[TOOL_CALL:tool_name:arg1=value1]`）来调用工具。如果我们直接在代码中调用 API，Agent 就失去了自主决策的能力，变成了一个简单的函数调用。

其次是参数传递复杂。高德地图的 API 有很多参数，比如 POI 搜索有`keywords`、`city`、`types`、`offset`、`page`等十几个参数。如果我们要让 Agent 能够灵活使用这些参数，就需要在提示词中详细说明每个参数的含义和格式，这会让提示词变得非常复杂。

第三是响应解析困难。高德地图 API 返回的是 JSON 格式的数据，结构比较复杂。我们需要编写代码来解析这些数据，提取我们需要的字段。如果 API 的响应格式发生变化，我们就需要修改解析代码。

最后是工具管理混乱。高德地图提供了十几个不同的 API（POI 搜索、天气查询、路线规划等），如果我们为每个 API 都编写一个函数，然后手动注册到 Agent 的工具列表中，代码会变得很冗长。而且当我们想添加新的 API 时，需要修改多个地方。

### 13.4.2 高德地图 MCP 集成

MCP (Model Context Protocol) 是 Anthropic 提出的标准化协议，用于连接 LLM 和外部工具。本节将介绍如何在项目中集成高德地图 MCP 服务器。我们的项目用的是`amap-mcp-server`，这是一个用 Node.js 实现的 MCP 服务器：

![](img/a88f5c27313bd516211eaac5c114f851.png)

图 13.7 `amap-mcp-server` 工具

高德地图 MCP 服务器提供了多种工具，主要分为以下类别，如表 13.1 所示:

![](img/b38b70061649257b8b0f8b79015f36ac.png)

表 13.1 高德地图 MCP 工具分类

通过 MCP 协议，我们可以很方便地在 HelloAgents 中集成：

```
from hello_agents.tools import MCPTool
from app.config import get_settings

settings = get_settings()
# 创建 MCP 工具
mcp_tool = MCPTool(
    name="amap_mcp",
    command="npx",
    args=["-y", "@sugarforever/amap-mcp-server"],
    env={"AMAP_API_KEY": settings.amap_api_key},
    auto_expand=True)
```

这段代码做了什么呢？首先，`command`和`args`指定了如何启动 MCP 服务器。`npx -y @sugarforever/amap-mcp-server`会从 npm 仓库下载并运行`amap-mcp-server`这个包。`env`参数传递了环境变量，这里我们传递了高德地图的 API 密钥。

注意：本文档中部分示例使用 `npx` 启动 MCP（Model Context Protocol）服务。而在本节代码仓中，我们实际采用的是 `uvx` 方式。需要说明的是，`npx` 和 `uvx` 在设计理念上高度一致，区别仅在于所处的生态系统，`npx` 面向 JavaScript/Node.js（包来自 `npm`），而 `uvx` 面向 Python（包来自 `PyPI`）。两种方式并无优劣之分，请大家在使用时按需进行选择。
