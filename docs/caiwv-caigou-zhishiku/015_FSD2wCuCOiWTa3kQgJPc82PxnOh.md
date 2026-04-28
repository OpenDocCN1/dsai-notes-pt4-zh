# 定向数据 Deepseek生成内容

> 来源：[https://rxq0zfnkm77.feishu.cn/docx/KCOmdePRsoXVWkxNOPLcOfBWnqc](https://rxq0zfnkm77.feishu.cn/docx/KCOmdePRsoXVWkxNOPLcOfBWnqc)

以下是一个基于提供的表格文件生成的综合数据看板，使用 HTML 格式输出并确保文件可执行。看板包含柱状图和折线图的可视化方式，展示基础数据信息。

```
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh-CN&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
    &lt;title&gt;供应商分类数据看板&lt;/title&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/echarts@5.4.0/dist/echarts.min.js&quot;&gt;&lt;/script&gt;
    &lt;style&gt;
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            font-family: Arial, sans-serif;
        }
        .chart-container {
            margin: 20px 0;
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 8px;
        }
        .chart-title {
            font-size: 18px;
            margin-bottom: 10px;
            color: #333;
        }
        .chart-subtitle {
            font-size: 16px;
            color: #666;
            margin-bottom: 10px;
        }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div class=&quot;container&quot;&gt;
        &lt;h1&gt;供应商分类数据看板&lt;/h1&gt;
        
        &lt;!-- 供应商采购金额图表 --&gt;
        &lt;div class=&quot;chart-container&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;供应商采购金额（柱状图）&lt;/div&gt;
            &lt;div class=&quot;chart-subtitle&quot;&gt;展示各供应商的采购金额分布&lt;/div&gt;
            &lt;div id=&quot;supplierAmountChart&quot; style=&quot;height: 400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;

        &lt;!-- 月度采购总额图表 --&gt;
        &lt;div class=&quot;chart-container&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;月度采购总额（折线图）&lt;/div&gt;
            &lt;div class=&quot;chart-subtitle&quot;&gt;展示各月采购总额的变化趋势&lt;/div&gt;
            &lt;div id=&quot;monthlyPurchaseChart&quot; style=&quot;height: 400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;

    &lt;script&gt;
        // 供应商采购金额数据
        const supplierAmountData = {
            categories: [&#x27;Supplier A&#x27;, &#x27;Supplier B&#x27;, &#x27;Supplier C&#x27;, &#x27;Supplier D&#x27;, &#x27;Supplier E&#x27;, &#x27;Supplier F&#x27;, &#x27;Supplier G&#x27;, &#x27;Supplier H&#x27;],
            series: [
                {
                    name: &#x27;采购金额（元）&#x27;,
                    type: &#x27;bar&#x27;,
                    data: [45091, 45100, 45250, 45320, 45410, 45530, 45650, 45780]
                }
            ]
        };

        // 月度采购总额数据
        const monthlyPurchaseData = {
            categories: [&#x27;1月&#x27;, &#x27;2月&#x27;, &#x27;3月&#x27;, &#x27;4月&#x27;, &#x27;5月&#x27;, &#x27;6月&#x27;, &#x27;7月&#x27;, &#x27;8月&#x27;, &#x27;9月&#x27;, &#x27;10月&#x27;, &#x27;11月&#x27;, &#x27;12月&#x27;],
            series: [
                {
                    name: &#x27;采购总额（元）&#x27;,
                    type: &#x27;line&#x27;,
                    data: [30000, 35000, 41000, 38000, 42000, 40000, 45000, 43000, 46000, 44000, 47000, 48000]
                }
            ]
        };

        // 初始化图表
        const supplierAmountChart = echarts.init(document.getElementById(&#x27;supplierAmountChart&#x27;));
        const monthlyPurchaseChart = echarts.init(document.getElementById(&#x27;monthlyPurchaseChart&#x27;));

        // 设置图表选项
        supplierAmountChart.setOption({
            title: {
                text: &#x27;供应商采购金额&#x27;
            },
            tooltip: {},
            xAxis: {
                data: supplierAmountData.categories
            },
            yAxis: {
                type: &#x27;value&#x27;
            },
            series: supplierAmountData.series
        });

        monthlyPurchaseChart.setOption({
            title: {
                text: &#x27;月度采购总额&#x27;
            },
            tooltip: {},
            xAxis: {
                data: monthlyPurchaseData.categories
            },
            yAxis: {
                type: &#x27;value&#x27;
            },
            series: monthlyPurchaseData.series
        });
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
```

### 使用说明：

1.  将上述代码保存为 .html 文件（例如 supplier_dashboard.html）。

1.  使用浏览器（如 Chrome、Firefox、Edge 等）打开该文件。

1.  图表将自动加载并显示，包含以下内容：

+   **供应商采购金额**：柱状图展示各供应商的采购金额分布。

+   **月度采购总额**：折线图展示各月采购总额的变化趋势。

### 数据说明：

+   **供应商采购金额**：展示各供应商的采购金额分布，帮助分析供应商采购情况。

+   **月度采购总额**：展示各月采购总额的变化趋势，帮助分析采购节奏和季节性变化。

### 注意事项：

1.  数据为示例数据，实际使用时请替换为真实数据。

1.  图表需要连接互联网以加载 ECharts 库，如果需要离线使用，请下载 ECharts 库并调整路径。

通过该看板，可以直观地了解供应商采购情况和月度采购总额的变化趋势，辅助采购管理和决策分析。