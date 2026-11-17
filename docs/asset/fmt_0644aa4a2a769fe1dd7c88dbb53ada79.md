
# 第十三章 智能旅行助手

> 来源：[https://gotoaiworld.feishu.cn/docx/S83GdmaLaopAZfxmhg2clTTin5l](https://gotoaiworld.feishu.cn/docx/S83GdmaLaopAZfxmhg2clTTin5l)

在前面的章节中，我们从零开始构建了 `HelloAgents` 框架，实现了多种智能体范式、工具系统、记忆机制、协议通信和性能评估等核心功能。从本章开始，我们将进入一个全新的阶段：将所学知识融会贯通，构建完整的实用应用。

还记得在第一章中，我们构建的第一个智能体吗？那是一个简单的智能旅行助手，展示了Thought-Action-Observation循环的基本原理。本章的智能旅行助手将是一个完整的项目，包含以下核心功能：

（1）智能行程规划：用户输入目的地、日期、偏好等信息，系统自动生成包含景点、餐饮、酒店的完整行程计划。

（2）地图可视化：在地图上标注景点位置、绘制游览路线，让行程一目了然。

（3）预算计算：自动计算门票、酒店、餐饮、交通费用，显示预算明细。

（4）行程编辑：支持添加、删除、调整景点，实时更新地图。

（5）导出功能：支持导出为 PDF 或图片，方便保存和分享。

## [13.1 项目概述与架构设计](https://datawhalechina.github.io/hello-agents/#/./chapter13/%E7%AC%AC%E5%8D%81%E4%B8%89%E7%AB%A0%20%E6%99%BA%E8%83%BD%E6%97%85%E8%A1%8C%E5%8A%A9%E6%89%8B?id=_131-%e9%a1%b9%e7%9b%ae%e6%a6%82%e8%bf%b0%e4%b8%8e%e6%9e%b6%e6%9e%84%e8%ae%be%e8%ae%a1)

### 13.1.1 为什么需要智能旅行助手

规划一次旅行是一件既令人兴奋又令人头疼的事情。你需要在网上搜索景点信息，对比不同的攻略，查看天气预报，预订酒店，计算预算，规划路线。这个过程可能需要花费几个小时甚至几天的时间。而且即使花了这么多时间，你也不确定规划的行程是否合理，是否遗漏了什么重要的景点，预算是否准确。

传统的旅行规划方式有几个痛点。首先是信息分散。景点信息在旅游网站上，天气信息在天气网站上，酒店信息在预订网站上，你需要在多个网站之间切换，手动整合这些信息。其次是缺少个性化。大部分攻略都是通用的，不考虑你的个人偏好、预算限制、出行时间等因素。最后是难以调整。当你想修改行程时，可能需要重新规划整个行程，因为景点的顺序、时间安排、预算都是相互关联的。

AI 技术为解决这些问题提供了新的可能。想象一下，你只需要告诉系统"我想去北京玩 3 天，喜欢历史文化，预算中等"，系统就能自动为你生成一个完整的行程计划，包括每天去哪些景点、在哪里吃饭、住哪个酒店、需要多少预算。而且这个计划是可以调整的，你可以删除不喜欢的景点，调整游览顺序，系统会自动更新地图和预算。

这就是我们要构建的智能旅行助手。它不仅仅是一个技术演示，而是一个真正有用的应用。通过这个项目，你会学到如何将 AI 技术应用到实际问题中，如何设计多智能体系统，如何构建完整的 Web 应用。

### [13.1.2 技术架构概览](https://datawhalechina.github.io/hello-agents/#/./chapter13/%E7%AC%AC%E5%8D%81%E4%B8%89%E7%AB%A0%20%E6%99%BA%E8%83%BD%E6%97%85%E8%A1%8C%E5%8A%A9%E6%89%8B?id=_1312-%e6%8a%80%e6%9c%af%e6%9e%b6%e6%9e%84%e6%a6%82%e8%a7%88)

系统采用经典的前后端分离架构，分为四个层次，如图 13.1 所示：

![](img/c07b287dc7ff76b879da8b4281e0dfd8.png)

图 13.1 智能旅行助手技术架构

（1）前端层 (Vue3+TypeScript)：负责用户交互和数据展示，包括表单输入、结果展示、地图可视化。

（2）后端层 (FastAPI)：负责 API 路由、数据验证、业务逻辑。

（3）智能体层 (`HelloAgents`)：负责任务分解、工具调用、结果整合。包含 4 个专门的 `Agent`。

（4）外部服务层：提供数据和能力，包括高德地图 API、Unsplash API、LLM API。

数据流转过程如下：用户在前端填写表单 → 后端验证数据 → 调用智能体系统 → 智能体依次调用景点搜索、天气查询、酒店推荐、行程规划 `Agent` → 每个 `Agent` 通过 MCP 协议调用外部 API → 整合结果返回前端 → 前端渲染展示。

项目的结构参考如下，提供便于定位源码：

```
helloagents-trip-planner/
├── backend/                    # 后端代码
│   ├── app/
│   │   ├── agents/            # 智能体实现
│   │   ├── api/               # API路由
│   │   ├── models/            # 数据模型
│   │   ├── services/          # 服务层
│   │   └── config.py          # 配置文件
└── requirements.txt       # Python依赖
│
└── frontend/                   # 前端代码
    ├── src/
    │   ├── views/             # 页面组件
    │   ├── services/          # API服务
    │   ├── types/             # 类型定义
    │   └── router/            # 路由配置
    └── package.json           # npm依赖
```

详细的架构设计和数据流转将在后续章节中介绍。

### 13.1.3 快速体验：5 分钟运行项目

在深入学习实现细节之前，让我们先把项目跑起来，看看最终的效果。这样你会对整个系统有一个直观的认识。

环境要求：

+   Python 3.10 或更高版本

+   Node.js 16.0 或更高版本

+   npm 8.0 或更高版本

获取 API 密钥：

你需要准备以下 API 密钥：

+   LLM 的 API(OpenAI、DeepSeek 等)

+   高德地图 Web 服务 Key：访问 `https://console.amap.com/` 注册并创建应用

+   Unsplash Access Key：访问 `https://unsplash.com/developers` 注册并创建应用

将所有 API 密钥放入 `.env` 文件。

启动后端：

```
# 1. 进入后端目录
cd helloagents-trip-planner/backend

# 2. 安装依赖
pip install -r requirements.txt

# 3. 配置环境变量
cp .env.example .env
# 编辑 .env 文件，填入你的 API 密钥

# 4. 启动后端服务
uvicorn app.api.main:app --reload
# 或者
python run.py
```

成功启动后，访问 `http://localhost:8000/docs` 可以看到 API 文档。

打开新的终端窗口：

```
# 1. 进入前端目录
cd helloagents-trip-planner/frontend

# 2. 安装依赖
npm install

# 3. 启动前端服务
npm run dev
```

成功启动后，访问 `http://localhost:5173` 即可使用应用。

体验核心功能：

首先需在首页表单中填写目的地城市、旅行日期、偏好、预算、交通及住宿类型等信息。点击“开始规划”按钮后，系统会显示加载进度条，并很快生成结果页面，如图 13.2 所示。

![](img/45bb2dd16cc3df8fc1d016e3f6cbe533.png)

图 13.2 旅行助手规划进行页面

随后加载成功，该页面会清晰展示行程概览、预算明细、景点地图、每日行程详情和天气信息，如图 13.3，13.4 所示。

![](img/a1d9b5994ea901866d14f3b405fb791d.png)

图 13.3 旅行助手规划完成页面

![](img/856eea25f6989d33f7b82bbcd8f50038.png)

图 13.4 旅行助手规划完成页面

如果用户需要个性化调整，可以点击“编辑行程”按钮，自由调整景点顺序或删除某个景点，如图 13.5 所示。规划完成后，通过“导出行程”下拉菜单，即可将最终方案轻松保存为图片或 PDF 文件，方便随时查阅。

![](img/feb4fa3d04630143d3fe6c28addf7def.png)

图 13.5 旅行助手规划完成页面

## [13.2 数据模型设计](https://datawhalechina.github.io/hello-agents/#/./chapter13/%E7%AC%AC%E5%8D%81%E4%B8%89%E7%AB%A0%20%E6%99%BA%E8%83%BD%E6%97%85%E8%A1%8C%E5%8A%A9%E6%89%8B?id=_132-%e6%95%b0%e6%8d%ae%e6%a8%a1%e5%9e%8b%e8%ae%be%e8%ae%a1)

### 13.2.1 Web 应用中的数据流转

在构建智能旅行助手时，我们需要解决一个核心问题：如何表示和传递旅行计划数据?

我们需要理解一个完整的 Web 应用中数据是如何流转的。想象一下，当用户在浏览器中点击"开始规划"按钮时，会发生什么？

用户在前端填写的表单数据(目的地、日期、预算等)需要通过 HTTP 请求发送到后端服务器。后端接收到数据后，会调用智能体系统进行处理。智能体又会调用高德地图 API、Unsplash API 等外部服务获取数据。这些外部 API 返回的数据格式各不相同，有的用 `lng`，有的用 `lon`，有的用 `longitude`。最后，后端需要将处理好的数据返回给前端，前端再渲染成用户看到的页面。

在这个过程中，数据经历了多次转换：前端表单 → HTTP 请求 → 后端 Python 对象 → 外部 API 响应 → 后端 Python 对象 → HTTP 响应 → 前端 TypeScript 对象 → 页面展示。如果没有统一的数据格式，每一步转换都可能出错。这就是为什么我们需要数据模型。

### 13.2.2 从字典到 Pydantic 模型

让我们从第一章的简单原型开始。在那个原型中，我们使用 Python 字典来表示景点数据：

```
# 第一章的做法：使用字典
attraction = {"name": "故宫", "location": {"lng": 116.397128, "lat": 39.916527}, "price": 60}
# 访问数据
lng = attraction["location"]["lng"]
```

这种方式在原型阶段很方便，但在实际项目中会遇到很多问题。首先是字段名不统一的问题。高德地图 API 返回的位置数据是"116.397128，39.916527"这样的字符串，需要手动分割成经纬度。而 Unsplash API 可能使用 `longitude` 和 `latitude`。如果我们在代码中到处都用字典，就需要在每个地方都处理这些差异。

其次是类型安全的问题。假设我们不小心把 `price` 写成了字符串 "60"，在 Python 中这不会立即报错，但在计算总预算时就会出问题。更糟糕的是，这种错误只能在运行时才能发现，而且错误信息可能很难定位。

最后是维护性的问题。当我们需要给景点添加新字段(比如 `rating` 评分)时，需要在代码的多个地方修改。如果遗漏了某个地方，就会导致数据不一致。

Pydantic 提供了一个解决方案。它是 Python 的数据验证库，可以让我们用类来定义数据结构，并自动处理验证、转换和序列化。让我们看一个简单的例子：

```
from pydantic import BaseModel, Field

class Location(BaseModel):
    longitude: float = Field(..., description="经度")
    latitude: float = Field(..., description="纬度")


class Attraction(BaseModel):
    name: str
    location: Location
    ticket_price: int = 0


# 创建对象
attraction = Attraction(
    name="故宫",
    location=Location(longitude=116.397128, latitude=39.916527),
    ticket_price=60
)

# 类型安全的访问
lng = attraction.location.longitude  # IDE会提供代码补全
```

这样做有几个好处。首先，如果我们传入了错误的类型(比如把 `ticket_price` 设为字符串)，Pydantic 会立即抛出异常，告诉我们哪里出错了。其次，IDE 可以根据类型定义提供代码补全和类型检查，大大减少了拼写错误。最后，当我们需要修改数据结构时，只需要修改类定义，所有使用这个类的地方都会自动更新。

### 13.2.3 Pydantic 的核心概念

在深入设计我们的数据模型之前，让我们先了解 Pydantic 的几个核心概念。Pydantic 的基础是 `BaseModel` 类，所有的数据模型都需要继承这个类。每个字段都可以指定类型，Pydantic 会自动进行类型检查和转换。

字段定义使用 `Field` 函数，它可以指定默认值、描述、验证规则等。`...` 表示这个字段是必填的，如果创建对象时没有提供这个字段，Pydantic 会抛出异常。我们也可以使用 `Optional` 来表示可选字段，或者直接提供默认值。

```
from pydantic import BaseModel, Field
from typing import Optional, List

class Attraction(BaseModel):
    name: str = Field(..., description="景点名称")  # 必填
    rating: float = Field(default=0.0, ge=0, le=5)  # 默认值,范围验证
    visit_duration: int = Field(default=60, gt=0)  # 大于0
    description: Optional[str] = None  # 可选字段
```

Pydantic 还支持嵌套模型和列表。我们可以在一个模型中使用另一个模型作为字段类型,这样就可以构建复杂的数据结构。比如，一个景点包含位置信息，一个行程包含多个景点。

```
class DayPlan(BaseModel):
    date: str
    attractions: List[Attraction]  # 景点列表
    hotel: Optional[Hotel] = None  # 可选的酒店信息
```

最强大的功能之一是自定义验证器。有时候外部 API 返回的数据格式不符合我们的要求，我们可以使用 `field_validator` 装饰器来自定义验证和转换逻辑。比如，高德地图返回的温度是"16°C"这样的字符串，我们需要把它转换成数字：

```
from pydantic import field_validator

class WeatherInfo(BaseModel):
    temperature: int

    @field_validator('temperature', mode='before')
    def parse_temperature(cls, v):
        """解析温度字符串：'16°C' -> 16"""
        if isinstance(v, str):
            v = v.replace('°C', '').replace('℃', '').strip()
            return int(v)
        return v
```

这个验证器会在创建对象之前自动执行，将字符串转换成整数。这样我们就不需要在代码的每个地方都手动处理温度格式了。

### 13.2.4 自底向上的模型设计

现在让我们开始设计智能旅行助手的数据模型。一个好的设计原则是自底向上：先定义最基础的模型，然后逐步组合成复杂的结构。这样做的好处是每个模型都很简单，容易理解和维护。

最基础的模型是位置信息。无论是景点、酒店还是餐厅，都需要位置信息。我们定义一个 `Location` 类来表示经纬度坐标：
