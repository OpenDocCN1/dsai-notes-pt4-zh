
# 优化版Deepseek生成内容

> 来源：[https://rxq0zfnkm77.feishu.cn/docx/SdKvd4uTSoKjsIxj2JKccmhfnhg](https://rxq0zfnkm77.feishu.cn/docx/SdKvd4uTSoKjsIxj2JKccmhfnhg)

## 问：

生成一个针对中国市场的2021年-2024年新能源汽车行业综合数据看板，包含可视化设计和数据展现，使用`HTML`格式输出并确保文件可执行。看板应关注市场销售数据和政策环境，采用柱状图和折线图的可视化方式，填充基础的数据信息。

## 答：

以下是一个使用 `HTML` 和 `ECharts` 实现的新能源汽车行业综合数据看板，包含柱状图和折线图，展示市场销售数据和政策环境。

### 使用说明：

1. 将上述代码保存为 `.html` 文件（例如 `new_energy_vehicles_dashboard.html`）。
2. 使用浏览器（如 Chrome、Firefox、Edge 等）打开该文件。
3. 图表将自动加载并显示，包含以下内容：
   + 市场销售数据（柱状图）
   + 市场份额变化（折线图）
   + 政策环境分析（表格形式）
   + 产能数据（柱状图）

该文件是可执行的 `HTML` 文件：

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>中国新能源汽车行业综合数据看板（2021-2024年）</title>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.0/dist/echarts.min.js"></script>
    <style>
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
    </style>
</head>
<body>
    <div class="container">
        <h1>中国新能源汽车行业综合数据看板（2021-2024年）</h1>
        
        <!-- 销量数据图表 -->
        <div class="chart-container">
            <div class="chart-title">市场销售数据（单位：万辆）</div>
            <div class="chart-subtitle">2021-2024年新能源汽车销量趋势</div>
            <div id="salesChart" style="height: 400px;"></div>
        </div>

        <!-- 市场份额数据图表 -->
        <div class="chart-container">
            <div class="chart-title">市场份额变化（%）</div>
            <div class="chart-subtitle">2021-2024年新能源汽车市场份额趋势</div>
            <div id="marketShareChart" style="height: 400px;"></div>
        </div>

        <!-- 政策环境数据表格 -->
        <div class="chart-container">
            <div class="chart-title">政策环境分析</div>
            <div class="chart-subtitle">2021-2024年政策支持力度评分（0-5分）</div>
            <table style="width: 100%; border-collapse: collapse;">
                <tr>
                    <th>年份</th>
                    <th>政策支持评分</th>
                    <th>主要政策</th>
                </tr>
                <tr>
                    <td>2021</td>
                    <td>4.2</td>
                    <td>补贴逐步退出，双积分政策开始实施</td>
                </tr>
                <tr>
                    <td>2022</td>
                    <td>4.8</td>
                    <td>“双积分”政策全面推行，地方补贴继续</td>
                </tr>
                <tr>
                    <td>2023</td>
                    <td>5.1</td>
                    <td>充电基础设施建设加速，限购政策放开</td>
                </tr>
                <tr>
                    <td>2024</td>
                    <td>4.9</td>
                    <td>补贴政策完全退出，市场化改革深化</td>
                </tr>
            </table>
        </div>

        <!-- 产能数据图表 -->
        <div class="chart-container">
            <div class="chart-title">产能数据（单位：万辆）</div>
            <div class="chart-subtitle">2021-2024年新能源汽车产能变化</div>
            <div id="capacityChart" style="height: 400px;"></div>
        </div>
    </div>

    <script>
        // 销量数据
        const salesData = {
            categories: ['2021', '2022', '2023', '2024'],
            series: [
                {
                    name: '销量（万辆）',
                    type: 'bar',
                    data: [32, 68, 101, 135]
                }
            ]
        };

        // 市场份额数据
        const marketShareData = {
            categories: ['2021', '2022', '2023', '2024'],
            series: [
                {
                    name: '市场份额（%）',
                    type: 'line',
                    data: [12, 22, 30, 35]
                }
            ]
        };

        // 产能数据
        const capacityData = {
            categories: ['2021', '2022', '2023', '2024'],
            series: [
                {
                    name: '产能（万辆）',
                    type: 'bar',
                    data: [55, 83, 117, 152]
                }
            ]
        };

        // 初始化图表
        const salesChart = echarts.init(document.getElementById('salesChart'));
        const marketShareChart = echarts.init(document.getElementById('marketShareChart'));
        const capacityChart = echarts.init(document.getElementById('capacityChart'));

        // 设置图表选项
        salesChart.setOption({
            title: {
                text: '市场销售数据（单位：万辆）'
            },
            tooltip: {},
            xAxis: {
                data: salesData.categories
            },
            yAxis: {
                type: 'value'
            },
            series: salesData.series
        });

        marketShareChart.setOption({
            title: {
                text: '市场份额变化（%）'
            },
            tooltip: {},
            xAxis: {
                data: marketShareData.categories
            },
            yAxis: {
                type: 'value'
            },
            series: marketShareData.series
        });

        capacityChart.setOption({
            title: {
                text: '产能数据（单位：万辆）'
            },
            tooltip: {},
            xAxis: {
                data: capacityData.categories
            },
            yAxis: {
                type: 'value'
            },
            series: capacityData.series
        });
    </script>
</body>
</html>
```

### 数据说明：

+ **销量数据**：以万辆为单位，展示了2021-2024年中国新能源汽车的市场销量。
+ **市场份额**：以百分比形式，展示了新能源汽车在整体汽车市场中的份额变化。
+ **政策环境**：通过表格形式展示了各年的政策支持力度评分和主要政策。
+ **产能数据**：以万辆为单位，展示了新能源汽车的产能变化。

### 注意事项：

1. 数据为示例数据，实际使用时请替换为真实数据。
2. 图表需要连接互联网以加载 `ECharts` 库，如果需要离线使用，请下载 `ECharts` 库并调整路径。

通过该看板，可以直观地了解中国新能源汽车行业在2021-2024年的市场趋势和政策环境，支持战略决策。
