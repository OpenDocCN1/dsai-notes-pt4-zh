
### 研究完成：服务器推送最终报告，关闭连接

如果想把 SSE 用于前后端的项目中还需要做如下配置。

### 后端 `FastAPI` SSE 端点

```python
from fastapi import FastAPI
from fastapi.responses import StreamingResponse
from typing import AsyncGenerator
import asyncio
import json

app = FastAPI()

async def research_stream(topic: str) -> AsyncGenerator[str, None]:
    """研究流式生成器
    
    生成SSE格式的数据：
    data: {"type": "progress", "data": {...}}
    
    """
    try:
        # 1. 规划阶段
        yield f"data: {json.dumps({'type': 'progress', 'stage': 'planning', 'percentage': 10, 'text': '正在规划研究任务...'})}\n\n"
        # 调用PlanningService
        todo_items = await planning_service.plan_todo_list(topic)
        yield f"data: {json.dumps({'type': 'plan', 'data': [item.dict() for item in todo_items]})}\n\n"
        # 2. 执行阶段
        task_summaries = []
        for idx, task in enumerate(todo_items, start=1):
            # 更新进度
            percentage = 10 + (idx / len(todo_items)) * 70
            yield f"data: {json.dumps({'type': 'progress', 'stage': 'executing', 'percentage': percentage, 'text': f'正在研究任务{idx}/{len(todo_items)}：{task.title}'})}\n\n"
            # 搜索
            search_results = await search_service.search(task.query)
            # 总结
            summary, source_urls = await summarization_service.summarize_task(task, search_results)
            
            task_summaries.append((task, summary, source_urls))
            # 推送任务总结
            yield f"data: {json.dumps({'type': 'task_summary', 'task_id': task.id, 'summary': summary})}\n\n"
        # 3. 报告阶段
        yield f"data: {json.dumps({'type': 'progress', 'stage': 'reporting', 'percentage': 90, 'text': '正在生成最终报告...'})}\n\n"
        # 生成报告
        report = await reporting_service.generate_report(topic, task_summaries)
        # 推送最终报告
        yield f"data: {json.dumps({'type': 'report', 'data': report})}\n\n"
        # 完成
        yield f"data: {json.dumps({'type': 'progress', 'stage': 'completed', 'percentage': 100, 'text': '研究完成！'})}\n\n"
    except Exception as e:
        # 错误处理
        yield f"data: {json.dumps({'type': 'error', 'message': str(e)})}\n\n"

@app.post("/api/research")
async def research(request: ResearchRequest):
    """研究端点（SSE）"""
    return StreamingResponse(
        research_stream(request.topic),
        media_type="text/event-stream",
        headers={"Cache-Control": "no-cache", "Connection": "keep-alive"},
    )
```

### 前端使用 `EventSource` 接收 `SSE`

```typescript
// composables/useResearch.ts
import { ref } from 'vue';

export function useResearch() {
  const isLoading = ref(false)
  const progressPercentage = ref(0)
  const progressText = ref('')
  const markdownContent = ref('')
  const error = ref<string | null>(null)
  
  const startResearch = (topic: string) => {
    isLoading.value = true
    error.value = null
    
    // 创建EventSource
    const eventSource = new EventSource(`/api/research?topic=${encodeURIComponent(topic)}`)
    
    // 监听消息
    eventSource.onmessage = (event) => {
      const data = JSON.parse(event.data)
      
      switch (data.type) {
        case 'progress':
          progressPercentage.value = data.percentage
          progressText.value = data.text
          break
          
        case 'plan':
          // 显示规划结果
          console.log('规划结果:', data.data)
          break
          
        case 'task_summary':
          // 追加任务总结到Markdown
          markdownContent.value += `\n\n## 任务${data.task_id}\n\n${data.summary}`
          break
          
        case 'report':
          // 显示最终报告
          markdownContent.value = data.data
          break
          
        case 'error':
          error.value = data.message
          eventSource.close()
          isLoading.value = false
          break
          
        case 'completed':
          eventSource.close()
          isLoading.value = false
          break
      }
    }
    
    // 错误处理
    eventSource.onerror = (err) => {
      console.error('SSE错误:', err)
      error.value = '连接失败，请重试'
      eventSource.close()
      isLoading.value = false
    }
  }
  
  return {
    isLoading,
    progressPercentage,
    progressText,
    markdownContent,
    error,
    startResearch,
  }
}
```

### 在组件中使用

```vue
<script setup lang="ts">
import { useResearch } from '@/composables/useResearch'
const { 
  isLoading, 
  progressPercentage, 
  progressText, 
  markdownContent, 
  error,
  startResearch 
} = useResearch()
const handleStartResearch = (topic: string) => {
  startResearch(topic)
}
</script>
```

### [14.6.3 研究结果可视化](https://datawhalechina.github.io/hello-agents/#/./chapter14/%E7%AC%AC%E5%8D%81%E5%9B%9B%E7%AB%A0%20%E8%87%AA%E5%8A%A8%E5%8C%96%E6%B7%B1%E5%BA%A6%E7%A0%94%E7%A9%B6%E6%99%BA%E8%83%BD%E4%BD%93?id=_1463-%e7%a0%94%e7%a9%b6%e7%bb%93%e6%9e%9c%e5%8f%af%e8%a7%86%e5%8c%96)

研究结果以 `Markdown` 格式展示，包含标题、段落、列表、引用等元素。我们使用 `marked` 库将 `Markdown` 转换为 `HTML`，并添加自定义样式。

### 渲染 `Markdown`

```javascript
import { marked } from 'marked';

// 配置marked
marked.setOptions({
  breaks: true,  // 支持换行
  gfm: true,     // 支持GitHub Flavored Markdown
})

// 渲染
const renderedHtml = marked(markdownContent.value)
```

研究报告中包含大量来源引用，我们需要特殊处理：

```markdown
## 参考文献

### 任务1：Datawhale的基本信息
- [Datawhale GitHub](https://github.com/datawhalechina)
- [Datawhale 官网](https://datawhale.club)

### 任务2：Datawhale的主要项目
- [Hello-Agents 教程](https://github.com/datawhalechina/Hello-Agents)
......
```

通过全屏模态对话框 UI、`SSE` 实时进度展示和 `Markdown` 结果可视化，我们构建了一个用户友好的前端界面。用户可以清晰地看到研究进度，并以美观的格式查看研究结果。

## [14.7 本章小结](https://datawhalechina.github.io/hello-agents/#/./chapter14/%E7%AC%AC%E5%8D%81%E5%9B%9B%E7%AB%A0%20%E8%87%AA%E5%8A%A8%E5%8C%96%E6%B7%B1%E5%BA%A6%E7%A0%94%E7%A9%B6%E6%99%BA%E8%83%BD%E4%BD%93?id=_147-%e6%9c%ac%e7%ab%a0%e5%b0%8f%e7%bb%93)

在本章中，我们从零开始构建了一个完整的自动化深度研究智能体系统。让我们回顾一下核心要点：

### （1）`TODO` 驱动的研究范式

我们提出了一种新的研究范式——`TODO` 驱动的研究。这种范式将复杂的研究主题分解为可执行的子任务，通过三个阶段完成研究：

+ 规划阶段：将研究主题分解为 3-5 个子任务，每个子任务包含标题、意图和搜索查询

+ 执行阶段：对每个子任务执行搜索和总结，生成结构化的知识

+ 报告阶段：整合所有子任务的总结，生成最终的研究报告

这种范式的优势在于：

1. 可控性强：每个子任务都有明确的目标和范围

1. 质量可靠：通过专门的 `Agent` 保证每个环节的质量

1. 易于调试：可以单独调试每个子任务

1. 可扩展性好：可以轻松添加新的子任务或修改现有子任务

### （2）三 `Agent` 协作系统

我们设计了三个专门的 `Agent`，各司其职：

+ `TODO Planner`（研究规划专家）：负责将研究主题分解为子任务

+ `Task Summarizer`（任务总结专家）：负责总结每个子任务的搜索结果

+ `Report Writer`（报告撰写专家）：负责整合所有子任务的总结，生成最终报告

这种设计的优势在于：

1. 职责清晰：每个 `Agent` 专注于一个特定的任务

1. `Prompt` 优化：可以为每个 `Agent` 定制专门的 `Prompt`

1. 易于维护：修改一个 `Agent` 不会影响其他 `Agent`

1. 质量保证：每个 `Agent` 都是该领域的"专家"

### （3）`ToolAwareSimpleAgent` 的设计

我们扩展了 `HelloAgents` 框架的 `SimpleAgent`，实现了 `ToolAwareSimpleAgent`。这个 `Agent` 具有工具调用监听能力，可以：

+ 监听工具调用：通过回调函数监听每次工具调用

+ 实时反馈：将工具调用信息实时推送给前端

+ 调试支持：记录所有工具调用，便于调试

这个 `Agent` 已经集成到 `HelloAgents` 框架中，可以在其他项目中复用。

### （4）工具系统集成

我们充分利用了 `HelloAgents` 框架的工具系统：

+ `SearchTool`：扩展支持更多种搜索引擎（`Tavily`、`DuckDuckGo`、`Perplexity` 等）

+ `NoteTool`：持久化研究进度，支持恢复和审计

+ `ToolRegistry`：统一管理所有工具，支持自定义扩展

通过配置化的设计，用户可以轻松切换搜索引擎，无需修改代码。

### （5）核心服务实现

我们实现了四个核心服务，连接 `Agent` 和工具：

+ `PlanningService`：调用规划 `Agent`，解析 `JSON`，验证格式

+ `SummarizationService`：调用总结 `Agent`，处理搜索结果，提取来源

+ `ReportingService`：调用报告 `Agent`，整合总结，生成报告

+ `SearchService`：调度搜索引擎，处理结果，错误降级，结果缓存

这些服务各司其职，通过清晰的接口协作，实现了从研究主题到最终报告的自动化流程。

### （6）前端交互设计

我们设计了用户友好的前端界面：

+ 全屏模态对话框：沉浸式体验，清晰的层次

+ `SSE` 实时进度：实时展示研究进度，用户体验良好

+ `Markdown` 可视化：美观的格式，清晰的结构

通过 `Vue 3` + `TypeScript` + `SSE` 的技术栈，我们实现了一个现代化的 Web 应用。

这些知识不仅适用于深度研究助手，也可以应用到其他 AI 应用中。希望读者能够在本章的基础上，探索更多的可能性，构建出更强大的 AI 系统。
