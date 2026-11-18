
# 完整通用SVG流程图生成提示词模板

请根据以下业务流程描述创建一个专业的SVG流程架构图：

[在此处插入您的业务流程自然语言描述]

## 视觉设计原则

配色方案采用清爽简洁的风格，避免深色背景或复杂渐变。使用浅色背景（如`#fafbfc`或`#f8fafc`）搭配明亮但不刺眼的主色调。不同功能模块应使用语义化的色彩区分，例如绿色系表示处理功能，橙色系表示审核功能，蓝色系表示交互功能，红色系表示重要决策或转换节点。

节点设计保持统一性和层次感。所有矩形节点使用相同的圆角半径（通常`5-8px`），通过颜色深浅和边框粗细来区分重要程度。关键节点使用较粗的边框（`2-3px`），普通节点使用标准边框（`1-2px`）。节点内文字采用层次化设计，主标题使用粗体较大字号，副标题或说明文字使用较小字号和灰色。

## 布局和流程结构

采用从左到右的时间线布局，将整个流程按照时间顺序水平排列。将相似功能的节点垂直对齐，形成功能分组。重要的转换节点或决策点应在视觉上突出显示，可使用不同的形状、颜色或位置来强调。

连线设计应简洁明确。使用不同颜色的连线对应不同的功能流程，所有连线都应包含适当的箭头标记以明确方向。对于复杂的多段路径，必须使用`path`元素而非`line`元素。虚线用于表示可选路径或回退操作。分支连线应采用清晰的分叉设计，从决策节点的共同起点水平延伸后再分向不同目标，避免使用直接的斜线连接。

## 技术实现要求

SVG代码结构应清晰规范。在文档开头定义所有必要的`marker`元素，包括不同颜色的箭头标记。合理分组和注释代码段，每个功能区域使用清晰的注释标识。确保所有多段连线使用正确的`path`语法，避免在`line`元素中重复定义坐标属性。

连线精确定位规范必须严格遵守。起点应从源节点的边缘适当位置延伸，终点应准确连接到目标节点的边缘中心区域，避免连接到节点的角落或顶点。水平连线应连接节点的左右边缘中心点，垂直连线应连接节点的上下边缘中心点。连线路径规划必须避免穿越其他节点，当直线路径会与中间节点产生视觉冲突时，应设计合理的绕行路径，通过增加适当的转折点确保连线清晰可见。

对于从一个决策点分向多个目标的分支连线，应使用折线分叉结构而非直接的斜线连接。具体实现为从源节点右边缘中心点水平延伸一定距离形成主干，然后在适当位置垂直分叉至各目标节点的垂直位置，最后水平连接到目标节点的左边缘中心点。这种设计能够清晰表达从单一决策点派生多个路径的逻辑关系。

多段连线的转折点应基于节点的实际位置和尺寸精确计算，确保有足够的间距避免视觉拥挤。转折点之间的距离应保持一致性，维护整体布局的规整感。绕行路径应选择最短且最整洁的方案，优先使用水平和垂直线段的组合。

## 文字和标注规范

文字和标注设计注重可读性。使用无衬线字体确保在不同设备上的显示效果。关键信息使用适当的字重和颜色强调。在流程图底部添加简洁的说明文字，解释主要流程逻辑。节点内文字应保持简洁明确，主标题控制在`2-4`个字符或词汇，副标题提供必要的功能说明。

## 响应性和可维护性要求

响应性和可维护性考虑。使用合理的`viewBox`设置确保图表在不同尺寸下的显示效果。保持一致的命名规范和代码结构，便于后续修改和维护。节点尺寸和间距应保持规律性，便于整体布局的调整。

## 参考模板结构

请参考以下SVG模板的结构、样式和技术实现方式，确保生成的流程图符合上述所有设计规范和技术要求：

```svg
<svg viewBox="0 0 1400 800" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <marker id="arrow" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto">
      <path d="M0,0 L0,6 L9,3 z" fill="#1f2937"/>
    </marker>
    <marker id="greenArrow" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto">
      <path d="M0,0 L0,6 L9,3 z" fill="#16a34a"/>
    </marker>
    <marker id="orangeArrow" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto">
      <path d="M0,0 L0,6 L9,3 z" fill="#f59e0b"/>
    </marker>
    <marker id="blueArrow" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto">
      <path d="M0,0 L0,6 L9,3 z" fill="#2563eb"/>
    </marker>
    <marker id="redArrow" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto">
      <path d="M0,0 L0,6 L9,3 z" fill="#dc2626"/>
    </marker>
  </defs>
  <!-- 背景 -->
  <rect width="1400" height="800" fill="#fafbfc"/>
  <!-- 标题 -->
  <text x="700" y="30" text-anchor="middle" font-family="Arial, sans-serif" font-size="20" font-weight="bold" fill="#1f2937">业务流程架构图</text>
  <!-- 左侧初始节点区域 -->
  <rect x="30" y="150" width="140" height="60" rx="8" fill="#f8fafc" stroke="#64748b" stroke-width="2"/>
  <text x="100" y="175" text-anchor="middle" font-family="Arial, sans-serif" font-size="14" font-weight="bold" fill="#1f2937">流程起始点</text>
  <text x="100" y="195" text-anchor="middle" font-family="Arial, sans-serif" font-size="11" fill="#64748b">初始化操作</text>
  <rect x="30" y="260" width="140" height="60" rx="8" fill="#f0f9ff" stroke="#0ea5e9" stroke-width="2"/>
  <text x="100" y="285" text-anchor="middle" font-family="Arial, sans-serif" font-size="14" font-weight="bold" fill="#0c4a6e">选择界面</text>
  <text x="100" y="305" text-anchor="middle" font-family="Arial, sans-serif" font-size="11" fill="#64748b">功能选择配置</text>
  <!-- 处理流程A（绿色） -->
  <rect x="250" y="120" width="130" height="50" rx="5" fill="#dcfce7" stroke="#16a34a" stroke-width="2"/>
  <text x="315" y="140" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#15803d">流程A启动</text>
  <text x="315" y="155" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">处理类型A</text>
  <rect x="420" y="120" width="130" height="50" rx="5" fill="#dcfce7" stroke="#16a34a" stroke-width="2"/>
  <text x="485" y="140" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#15803d">参数配置A</text>
  <text x="485" y="155" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">配置处理参数</text>
  <rect x="590" y="120" width="130" height="50" rx="5" fill="#dcfce7" stroke="#16a34a" stroke-width="2"/>
  <text x="655" y="140" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#15803d">数据处理A</text>
  <text x="655" y="155" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">数据格式化</text>
  <rect x="760" y="120" width="130" height="50" rx="5" fill="#dcfce7" stroke="#16a34a" stroke-width="2"/>
  <text x="825" y="140" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#15803d">执行处理A</text>
  <text x="825" y="155" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">核心业务处理</text>
  <rect x="930" y="120" width="130" height="50" rx="5" fill="#dcfce7" stroke="#16a34a" stroke-width="2"/>
  <text x="995" y="140" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#15803d">生成结果A</text>
  <text x="995" y="155" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">输出处理结果</text>
  <!-- 处理流程B（橙色） -->
  <rect x="250" y="220" width="130" height="50" rx="5" fill="#fef3c7" stroke="#f59e0b" stroke-width="2"/>
  <text x="315" y="240" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#92400e">流程B启动</text>
  <text x="315" y="255" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">处理类型B</text>
  <rect x="420" y="220" width="130" height="50" rx="5" fill="#fef3c7" stroke="#f59e0b" stroke-width="2"/>
  <text x="485" y="240" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#92400e">参数配置B</text>
  <text x="485" y="255" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">配置处理参数</text>
  <rect x="590" y="220" width="130" height="50" rx="5" fill="#fef3c7" stroke="#f59e0b" stroke-width="2"/>
  <text x="655" y="240" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#92400e">数据处理B</text>
  <text x="655" y="255" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">数据格式化</text>
  <rect x="760" y="220" width="130" height="50" rx="5" fill="#fef3c7" stroke="#f59e0b" stroke-width="2"/>
  <text x="825" y="240" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#92400e">执行处理B</text>
  <text x="825" y="255" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">核心业务处理</text>
  <rect x="930" y="220" width="130" height="50" rx="5" fill="#fef3c7" stroke="#f59e0b" stroke-width="2"/>
  <text x="995" y="240" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#92400e">生成结果B</text>
  <text x="995" y="255" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">输出处理结果</text>
  <!-- 右侧结果展示 -->
  <rect x="1100" y="160" width="140" height="80" rx="8" fill="#f3f4f6" stroke="#6b7280" stroke-width="2"/>
  <text x="1170" y="185" text-anchor="middle" font-family="Arial, sans-serif" font-size="14" font-weight="bold" fill="#374151">结果展示区域</text>
  <text x="1170" y="205" text-anchor="middle" font-family="Arial, sans-serif" font-size="11" fill="#64748b">处理结果A展示</text>
  <text x="1170" y="220" text-anchor="middle" font-family="Arial, sans-serif" font-size="11" fill="#64748b">处理结果B展示</text>
  <text x="1170" y="235" text-anchor="middle" font-family="Arial, sans-serif" font-size="11" fill="#64748b">实时状态更新</text>
  <!-- 状态转换 -->
  <rect x="500" y="340" width="400" height="60" rx="8" fill="#fef2f2" stroke="#dc2626" stroke-width="3"/>
  <text x="700" y="365" text-anchor="middle" font-family="Arial, sans-serif" font-size="16" font-weight="bold" fill="#dc2626">系统状态转换节点</text>
  <text x="700" y="385" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" fill="#64748b">保持上下文状态，继承处理历史</text>
  <!-- 交互处理流程（蓝色） -->
  <rect x="250" y="480" width="130" height="50" rx="5" fill="#f0f9ff" stroke="#2563eb" stroke-width="2"/>
  <text x="315" y="500" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#1e40af">用户交互输入</text>
  <text x="315" y="515" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">交互界面处理</text>
  <rect x="420" y="480" width="130" height="50" rx="5" fill="#f0f9ff" stroke="#2563eb" stroke-width="2"/>
  <text x="485" y="500" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#1e40af">输入验证</text>
  <text x="485" y="515" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">数据验证处理</text>
  <rect x="590" y="480" width="130" height="50" rx="5" fill="#f0f9ff" stroke="#2563eb" stroke-width="2"/>
  <text x="655" y="500" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#1e40af">交互处理</text>
  <text x="655" y="515" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">综合交互处理</text>
  <rect x="760" y="480" width="130" height="50" rx="5" fill="#f0f9ff" stroke="#2563eb" stroke-width="2"/>
  <text x="825" y="500" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#1e40af">内容更新</text>
  <text x="825" y="515" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">基于历史更新</text>
  <rect x="930" y="480" width="130" height="50" rx="5" fill="#f0f9ff" stroke="#2563eb" stroke-width="2"/>
  <text x="995" y="500" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#1e40af">生成更新版本</text>
  <text x="995" y="515" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">优化后输出</text>
  <!-- 实时更新展示 -->
  <rect x="1100" y="480" width="140" height="50" rx="5" fill="#f3f4f6" stroke="#6b7280" stroke-width="2"/>
  <text x="1170" y="500" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#374151">实时更新展示</text>
  <text x="1170" y="515" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">动态内容更新</text>
  <!-- 虚线框选输出结果区域 -->
  <rect x="920" y="110" width="150" height="170" rx="10" fill="none" stroke="#9ca3af" stroke-width="2" stroke-dasharray="8,4"/>
  <text x="995" y="105" text-anchor="middle" font-family="Arial, sans-serif" font-size="11" fill="#6b7280">初始处理结果</text>
  <!-- 重新处理路径 -->
  <rect x="550" y="620" width="200" height="50" rx="5" fill="#fff7ed" stroke="#ea580c" stroke-width="2" stroke-dasharray="5,5"/>
  <text x="650" y="640" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#c2410c">重新处理路径</text>
  <text x="650" y="655" text-anchor="middle" font-family="Arial, sans-serif" font-size="10" fill="#64748b">重新启动处理流程</text>
  <!-- 主要连接线 -->
  <!-- 初始流程连接 -->
  <line x1="100" y1="210" x2="100" y2="260" stroke="#1f2937" stroke-width="2" marker-end="url(#arrow)"/>
  <text x="110" y="235" font-family="Arial, sans-serif" font-size="12" fill="#64748b">进入</text>
  <!-- 从选择界面到功能分支 -->
  <path d="M 170 290 L 200 290 L 200 150 L 250 150" stroke="#16a34a" stroke-width="2" marker-end="url(#greenArrow)" fill="none"/>
  <path d="M 170 290 L 200 290 L 200 240 L 250 240" stroke="#f59e0b" stroke-width="2" marker-end="url(#orangeArrow)" fill="none"/>
  <text x="210" y="190" font-family="Arial, sans-serif" font-size="12" fill="#16a34a">流程A</text>
  <text x="210" y="260" font-family="Arial, sans-serif" font-size="12" fill="#f59e0b">流程B</text>
  <!-- 处理流程A连接 -->
  <line x1="380" y1="145" x2="420" y2="145" stroke="#16a34a" stroke-width="2" marker-end="url(#greenArrow)"/>
  <line x1="550" y1="145" x2="590" y2="145" stroke="#16a34a" stroke-width="2" marker-end="url(#greenArrow)"/>
  <line x1="720" y1="145" x2="760" y2="145" stroke="#16a34a" stroke-width="2" marker-end="url(#greenArrow)"/>
  <line x1="890" y1="145" x2="930" y2="145" stroke="#16a34a" stroke-width="2" marker-end="url(#greenArrow)"/>
  <!-- 处理流程B连接 -->
  <line x1="380" y1="245" x2="420" y2="245" stroke="#f59e0b" stroke-width="2" marker-end="url(#orangeArrow)"/>
  <line x1="550" y1="245" x2="590" y2="245" stroke="#f59e0b" stroke-width="2" marker-end="url(#orangeArrow)"/>
  <line x1="720" y1="245" x2="760" y2="245" stroke="#f59e0b" stroke-width="2" marker-end="url(#orangeArrow)"/>
  <line x1="890" y1="245" x2="930" y2="245" stroke="#f59e0b" stroke-width="2" marker-end="url(#orangeArrow)"/>
  <!-- 到结果展示的连接 -->
  <line x1="1060" y1="145" x2="1100" y2="180" stroke="#16a34a" stroke-width="2" marker-end="url(#greenArrow)"/>
  <line x1="1060" y1="245" x2="1100" y2="220" stroke="#f59e0b" stroke-width="2" marker-end="url(#orangeArrow)"/>
  <!-- 进入交互流程 -->
  <path d="M 500 370 L 200 370 L 200 450 L 315 450 L 315 480" stroke="#2563eb" stroke-width="3" marker-end="url(#blueArrow)" fill="none"/>
  <text x="290" y="425" font-family="Arial, sans-serif" font-size="14" fill="#2563eb">进入交互</text>
  <!-- 交互流程连接 -->
  <line x1="380" y1="505" x2="420" y2="505" stroke="#2563eb" stroke-width="2" marker-end="url(#blueArrow)"/>
  <line x1="550" y1="505" x2="590" y2="505" stroke="#2563eb" stroke-width="2" marker-end="url(#blueArrow)"/>
  <line x1="720" y1="505" x2="760" y2="505" stroke="#2563eb" stroke-width="2" marker-end="url(#blueArrow)"/>
  <line x1="890" y1="505" x2="930" y2="505" stroke="#2563eb" stroke-width="2" marker-end="url(#blueArrow)"/>
  <line x1="1060" y1="505" x2="1100" y2="505" stroke="#2563eb" stroke-width="2" marker-end="url(#blueArrow)"/>
  <!-- 重新开始路径 -->
  <path d="M 995 280 L 995 400 L 1260 400 L1260 645 L 750 645" stroke="#ea580c" stroke-width="2" stroke-dasharray="5,5" marker-end="url(#arrow)" fill="none"/>
  <path d="M 995 280 L 995 365 L 900 365" stroke="#dc2626" stroke-width="2" marker-end="url(#redArrow)" fill="none"/>
  <path d="M 550 645 L 100 645 L 100 320" stroke="#ea580c" stroke-width="2" stroke-dasharray="5,5" marker-end="url(#arrow)" fill="none"/>
  <text x="870" y="565" font-family="Arial, sans-serif" font-size="10" fill="#ea580c">重新开始</text>
  <!-- 流程说明区域 -->
  <rect x="50" y="720" width="1300" height="60" rx="5" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1"/>
  <text x="70" y="740" font-family="Arial, sans-serif" font-size="14" font-weight="bold" fill="#1f2937">核心流程说明</text>
  <text x="70" y="760" font-family="Arial, sans-serif" font-size="12" fill="#475569">1. 用户从起始点进入系统，通过选择界面配置参数，系统根据选择执行相应的处理流程并生成初始结果</text>
  <text x="70" y="775" font-family="Arial, sans-serif" font-size="12" fill="#475569">2. 系统转换至交互模式，用户可通过交互界面进行内容调整，支持基于历史的增量更新和实时展示</text>
</svg>
```
