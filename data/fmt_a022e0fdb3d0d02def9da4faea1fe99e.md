
```javascript
import html2canvas from 'html2canvas'
import jsPDF from 'jspdf'

// 导出为图片
const exportAsImage = async () => {
  const element = document.getElementById('trip-plan-content')
  const canvas = await html2canvas(element, { scale: 2 })
  const link = document.createElement('a')
  link.download = `${tripPlan.value.city}旅行计划.png`
  link.href = canvas.toDataURL()
  link.click()
}

// 导出为PDF
const exportAsPDF = async () => {
  const element = document.getElementById('trip-plan-content')
  const canvas = await html2canvas(element, { scale: 2 })
  const imgData = canvas.toDataURL('image/png')
  const pdf = new jsPDF('p', 'mm', 'a4')
  const imgWidth = 210
  const imgHeight = (canvas.height * imgWidth) / canvas.width
  pdf.addImage(imgData, 'PNG', 0, 0, imgWidth, imgHeight)
  pdf.save(`${tripPlan.value.city}旅行计划.pdf`)
}
```

通过这些前端技术，我们实现了一个完整的 Web 应用。用户可以在浏览器中填写表单，提交请求，等待 AI 生成旅行计划，然后查看详细的行程安排，在地图上看到景点位置，还可以导出为图片或 PDF。整个过程流畅自然，这就是现代 Web 应用的魅力。

## [13.6 功能实现详解](https://datawhalechina.github.io/hello-agents/#/./chapter13/%E7%AC%AC%E5%8D%81%E4%B8%89%E7%AB%A0%20%E6%99%BA%E8%83%BD%E6%97%85%E8%A1%8C%E5%8A%A9%E6%89%8B?id=_136-%e5%8a%9f%e8%83%bd%e5%ae%9e%e7%8e%b0%e8%af%a6%e8%a7%a3)

本节介绍智能旅行助手的核心功能实现，包括预算计算、加载进度条、行程编辑、导出功能和侧边导航。

### [13.6.1 预算计算功能](https://datawhalechina.github.io/hello-agents/#/./chapter13/%E7%AC%AC%E5%8D%81%E4%B8%89%E7%AB%A0%20%E6%99%BA%E8%83%BD%E6%97%85%E8%A1%8C%E5%8A%A9%E6%89%8B?id=_1361-%e9%a2%84%e7%ae%97%e8%ae%a1%e7%ae%97%e5%8a%9f%e8%83%bd)

在规划旅行时，预算是一个非常重要的考虑因素。用户需要知道这次旅行大概要花多少钱，钱都花在哪里。我们的智能旅行助手提供了自动预算计算功能，将费用分为四大类：景点门票、酒店住宿、餐饮和交通。

预算计算的逻辑在哪里实现呢？我们选择在后端的 `PlannerAgent` 中实现。为什么不在前端计算？因为预算的估算需要基于景点的门票价格、酒店的价格范围、餐饮的标准等信息，这些信息都是 `PlannerAgent` 在生成行程时已经获取的。如果在前端计算，就需要重复这些逻辑，而且可能不准确。

在 `PlannerAgent` 的提示词中，我们明确要求 `LLM` 生成预算信息：

```
PLANNER_AGENT_PROMPT = """
你是行程规划专家。

**输出格式：**
严格按照以下JSON格式返回：
{
  ...
  "budget": {
    "total_attractions": 180,
    "total_hotels": 1200,
    "total_meals": 480,
    "total_transportation": 200,
    "total": 2060
  }
}

**规划要求：**
...
7. 包含预算信息,根据景点门票、酒店价格、餐饮标准和交通方式估算
"""
```

`LLM` 会根据行程中的景点、酒店、餐饮安排，估算每一项的费用。比如，如果行程中包含故宫(门票 60 元)、天坛(门票 15 元)、颐和园(门票 30 元)，那么景点门票总费用就是 105 元。如果是 3 天 2 晚的行程，酒店是经济型(每晚 300 元)，那么酒店总费用就是 600 元。

在前端，我们使用 Ant Design Vue 的 `Statistic` 组件来展示预算信息。这个组件专门用于展示统计数据,支持数字动画、前缀后缀、自定义样式等：

```
<a-card v-if="tripPlan.budget" title="💰 预算明细"><a-row :gutter="16"><a-col :span="6"><a-statistic title="景点门票" :value="tripPlan.budget.total_attractions" suffix="元" /></a-col><a-col :span="6"><a-statistic title="酒店住宿" :value="tripPlan.budget.total_hotels" suffix="元" /></a-col><a-col :span="6"><a-statistic title="餐饮费用" :value="tripPlan.budget.total_meals" suffix="元" /></a-col><a-col :span="6"><a-statistic title="交通费用" :value="tripPlan.budget.total_transportation" suffix="元" /></a-col></a-row><a-divider /><a-row><a-col :span="24" style="text-align: center;"><a-statistictitle="预估总费用" :value="tripPlan.budget.total" suffix="元" :value-style="{ color: '#cf1322',fontSize: '32px',fontWeight: 'bold' }" /></a-col></a-row></a-card>
```

这段代码使用了栅格布局(`a-row`和`a-col`)，将四项费用并排显示。每项费用使用一个`a-statistic`组件，显示标题和数值。最后用一个分隔线(`a-divider`)隔开，下面显示总费用，使用红色大字体突出显示。

注意`v-if="tripPlan.budget"`这个条件渲染。因为预算信息是可选的(在 `Pydantic` 模型中定义为`Optional[Budget]`)，如果 `LLM` 没有生成预算信息，这个卡片就不会显示。这体现了前端对数据的容错处理。

### [13.6.2 加载进度条](https://datawhalechina.github.io/hello-agents/#/./chapter13/%E7%AC%AC%E5%8D%81%E4%B8%89%E7%AB%A0%20%E6%99%BA%E8%83%BD%E6%97%85%E8%A1%8C%E5%8A%A9%E6%89%8B?id=_1362-%e5%8a%a0%e8%bd%bd%e8%bf%9b%e5%ba%a6%e6%9d%a1)

生成旅行计划是一个耗时的操作。后端需要依次调用 `AttractionSearchAgent`、`WeatherQueryAgent`、`HotelAgent` 和 `PlannerAgent`，每个 `Agent` 都要调用 `LLM` 和外部 API。整个过程可能需要 10-30 秒。如果用户点击"开始规划"按钮后，页面没有任何反馈，用户会以为系统卡住了，可能会刷新页面或重复点击。

为了提升用户体验，我们添加了加载进度条和状态提示。现在只是模拟进度，可以让用户知道系统正在工作。

```
const loading = ref(false)
const loadingProgress = ref(0)
const loadingStatus = ref('')

const handleSubmit = async () => {
  loading.value = true
  loadingProgress.value = 0

  // 模拟进度更新
  const progressInterval = setInterval(() => {
    if (loadingProgress.value < 90) {
      loadingProgress.value += 10
      if (loadingProgress.value <= 30) loadingStatus.value = '🔍 正在搜索景点...'
      else if (loadingProgress.value <= 50) loadingStatus.value = '🌤️ 正在查询天气...'
      else if (loadingProgress.value <= 70) loadingStatus.value = '🏨 正在推荐酒店...'
      else loadingStatus.value = '📋 正在生成行程计划...'
    }
  }, 500)

  try {
    const response = await generateTripPlan(formData.value)
    clearInterval(progressInterval)
    loadingProgress.value = 100
    loadingStatus.value = '✅ 完成！'
    router.push({ name: 'result', state: { tripPlan: response } })
  } catch (error) {
    clearInterval(progressInterval)
    message.error('生成计划失败')
  } finally {
    loading.value = false
  }
}
```

### [13.6.3 行程编辑功能](https://datawhalechina.github.io/hello-agents/#/./chapter13/%E7%AC%AC%E5%8D%81%E4%B8%89%E7%AB%A0%20%E6%99%BA%E8%83%BD%E6%97%85%E8%A1%8C%E5%8A%A9%E6%89%8B?id=_1363-%e8%a1%8c%e7%a8%8b%e7%bc%96%e8%be%91%e5%8a%9f%e8%83%bd)

AI 生成的旅行计划虽然很智能，但可能不完全符合用户的个人需求。比如，用户可能不喜欢某个景点，想删除它；或者想调整景点的游览顺序。我们提供了行程编辑功能，让用户可以自定义行程。

编辑功能的核心是状态管理。我们需要维护两个状态：当前的行程计划和原始的行程计划。当用户进入编辑模式时，我们保存原始计划的副本。如果用户取消编辑，就恢复原始计划。如果用户保存修改，就更新当前计划：

```
const editMode = ref(false)
const originalPlan = ref<TripPlan | null>(null)

// 进入编辑模式
const toggleEditMode = () => {
  editMode.value = true
  originalPlan.value = JSON.parse(JSON.stringify(tripPlan.value))
}
```

注意这里使用了`JSON.parse(JSON.stringify(...))`来深拷贝对象。为什么不直接赋值？因为 JavaScript 中对象是引用类型，如果直接赋值，`originalPlan`和`tripPlan`会指向同一个对象，修改一个会影响另一个。深拷贝可以创建一个完全独立的副本。

移动景点的逻辑是交换数组中两个元素的位置：

```
// 移动景点
const moveAttraction = (dayIndex: number,attractionIndex: number,direction: 'up' | 'down') => {
  const attractions = tripPlan.value.days[dayIndex].attractions
  const newIndex = direction === 'up' ? attractionIndex - 1 : attractionIndex + 1
  
  if (newIndex >= 0 && newIndex < attractions.length) {
    [attractions[attractionIndex],attractions[newIndex]] = 
    [attractions[newIndex],attractions[attractionIndex]]
  }
}
```

这里使用了 ES6 的解构赋值语法来交换两个元素。`[a, b] = [b, a]`是一个很优雅的交换方式，不需要临时变量。

删除景点使用数组的`splice`方法：

```
// 删除景点
const deleteAttraction = (dayIndex: number,attractionIndex: number) => {
  tripPlan.value.days[dayIndex].attractions.splice(attractionIndex, 1)
}
```

保存修改时，我们需要重新初始化地图，因为景点的位置可能发生了变化：

```
// 保存修改
const saveChanges = () => {
  editMode.value = false
  message.success('修改已保存')
  initMap()  // 重新初始化地图
}

// 取消编辑
const cancelEdit = () => {
  if (originalPlan.value) {
    tripPlan.value = originalPlan.value
  }
  editMode.value = false
}
```

在模板中，我们根据`editMode`的值显示不同的 UI。编辑模式下，每个景点旁边会显示上移、下移、删除按钮：
