# 优化版Deepseek生成内容

> 来源：[https://rxq0zfnkm77.feishu.cn/docx/SdKvd4uTSoKjsIxj2JKccmhfnhg](https://rxq0zfnkm77.feishu.cn/docx/SdKvd4uTSoKjsIxj2JKccmhfnhg)

问：

生成一个针对中国市场的2021年-2024年新能源汽车行业综合数据看板，包含可视化设计和数据展现，使用HTML格式输出并确保文件可执行。看板应关注市场销售数据和政策环境，采用柱状图和折线图的可视化方式，填充基础的数据信息。

答：

以下是一个使用 HTML 和 ECharts 实现的新能源汽车行业综合数据看板，包含柱状图和折线图，展示市场销售数据和政策环境。

### 使用说明：

1.  将上述代码保存为 .html 文件（例如 new_energy_vehicles_dashboard.html）。

1.  使用浏览器（如 Chrome、Firefox、Edge 等）打开该文件。

1.  图表将自动加载并显示，包含以下内容：

+   市场销售数据（柱状图）

+   市场份额变化（折线图）

+   政策环境分析（表格形式）

+   产能数据（柱状图）

该文件是可执行的 HTML 文件：

```
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh-CN&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
    &lt;title&gt;中国新能源汽车行业综合数据看板（2021-2024年）&lt;/title&gt;
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
        &lt;h1&gt;中国新能源汽车行业综合数据看板（2021-2024年）&lt;/h1&gt;
        
        &lt;!-- 销量数据图表 --&gt;
        &lt;div class=&quot;chart-container&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;市场销售数据（单位：万辆）&lt;/div&gt;
            &lt;div class=&quot;chart-subtitle&quot;&gt;2021-2024年新能源汽车销量趋势&lt;/div&gt;
            &lt;div id=&quot;salesChart&quot; style=&quot;height: 400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;

        &lt;!-- 市场份额数据图表 --&gt;
        &lt;div class=&quot;chart-container&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;市场份额变化（%）&lt;/div&gt;
            &lt;div class=&quot;chart-subtitle&quot;&gt;2021-2024年新能源汽车市场份额趋势&lt;/div&gt;
            &lt;div id=&quot;marketShareChart&quot; style=&quot;height: 400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;

        &lt;!-- 政策环境数据表格 --&gt;
        &lt;div class=&quot;chart-container&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;政策环境分析&lt;/div&gt;
            &lt;div class=&quot;chart-subtitle&quot;&gt;2021-2024年政策支持力度评分（0-5分）&lt;/div&gt;
            &lt;table style=&quot;width: 100%; border-collapse: collapse;&quot;&gt;
                &lt;tr&gt;
                    &lt;th&gt;年份&lt;/th&gt;
                    &lt;th&gt;政策支持评分&lt;/th&gt;
                    &lt;th&gt;主要政策&lt;/th&gt;
                &lt;/tr&gt;
                &lt;tr&gt;
                    &lt;td&gt;2021&lt;/td&gt;
                    &lt;td&gt;4.2&lt;/td&gt;
                    &lt;td&gt;补贴逐步退出，双积分政策开始实施&lt;/td&gt;
                &lt;/tr&gt;
                &lt;tr&gt;
                    &lt;td&gt;2022&lt;/td&gt;
                    &lt;td&gt;4.8&lt;/td&gt;
                    &lt;td&gt;“双积分”政策全面推行，地方补贴继续&lt;/td&gt;
                &lt;/tr&gt;
                &lt;tr&gt;
                    &lt;td&gt;2023&lt;/td&gt;
                    &lt;td&gt;5.1&lt;/td&gt;
                    &lt;td&gt;充电基础设施建设加速，限购政策放开&lt;/td&gt;
                &lt;/tr&gt;
                &lt;tr&gt;
                    &lt;td&gt;2024&lt;/td&gt;
                    &lt;td&gt;4.9&lt;/td&gt;
                    &lt;td&gt;补贴政策完全退出，市场化改革深化&lt;/td&gt;
                &lt;/tr&gt;
            &lt;/table&gt;
        &lt;/div&gt;

        &lt;!-- 产能数据图表 --&gt;
        &lt;div class=&quot;chart-container&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;产能数据（单位：万辆）&lt;/div&gt;
            &lt;div class=&quot;chart-subtitle&quot;&gt;2021-2024年新能源汽车产能变化&lt;/div&gt;
            &lt;div id=&quot;capacityChart&quot; style=&quot;height: 400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;

    &lt;script&gt;
        // 销量数据
        const salesData = {
            categories: [&#x27;2021&#x27;, &#x27;2022&#x27;, &#x27;2023&#x27;, &#x27;2024&#x27;],
            series: [
                {
                    name: &#x27;销量（万辆）&#x27;,
                    type: &#x27;bar&#x27;,
                    data: [32, 68, 101, 135]
                }
            ]
        };

        // 市场份额数据
        const marketShareData = {
            categories: [&#x27;2021&#x27;, &#x27;2022&#x27;, &#x27;2023&#x27;, &#x27;2024&#x27;],
            series: [
                {
                    name: &#x27;市场份额（%）&#x27;,
                    type: &#x27;line&#x27;,
                    data: [12, 22, 30, 35]
                }
            ]
        };

        // 产能数据
        const capacityData = {
            categories: [&#x27;2021&#x27;, &#x27;2022&#x27;, &#x27;2023&#x27;, &#x27;2024&#x27;],
            series: [
                {
                    name: &#x27;产能（万辆）&#x27;,
                    type: &#x27;bar&#x27;,
                    data: [55, 83, 117, 152]
                }
            ]
        };

        // 初始化图表
        const salesChart = echarts.init(document.getElementById(&#x27;salesChart&#x27;));
        const marketShareChart = echarts.init(document.getElementById(&#x27;marketShareChart&#x27;));
        const capacityChart = echarts.init(document.getElementById(&#x27;capacityChart&#x27;));

        // 设置图表选项
        salesChart.setOption({
            title: {
                text: &#x27;市场销售数据（单位：万辆）&#x27;
            },
            tooltip: {},
            xAxis: {
                data: salesData.categories
            },
            yAxis: {
                type: &#x27;value&#x27;
            },
            series: salesData.series
        });

        marketShareChart.setOption({
            title: {
                text: &#x27;市场份额变化（%）&#x27;
            },
            tooltip: {},
            xAxis: {
                data: marketShareData.categories
            },
            yAxis: {
                type: &#x27;value&#x27;
            },
            series: marketShareData.series
        });

        capacityChart.setOption({
            title: {
                text: &#x27;产能数据（单位：万辆）&#x27;
            },
            tooltip: {},
            xAxis: {
                data: capacityData.categories
            },
            yAxis: {
                type: &#x27;value&#x27;
            },
            series: capacityData.series
        });
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
```

### 数据说明：

+   **销量数据**：以万辆为单位，展示了2021-2024年中国新能源汽车的市场销量。

+   **市场份额**：以百分比形式，展示了新能源汽车在整体汽车市场中的份额变化。

+   **政策环境**：通过表格形式展示了各年的政策支持力度评分和主要政策。

+   **产能数据**：以万辆为单位，展示了新能源汽车的产能变化。

### 注意事项：

1.  数据为示例数据，实际使用时请替换为真实数据。

1.  图表需要连接互联网以加载 ECharts 库，如果需要离线使用，请下载 ECharts 库并调整路径。

通过该看板，可以直观地了解中国新能源汽车行业在2021-2024年的市场趋势和政策环境，支持战略决策。