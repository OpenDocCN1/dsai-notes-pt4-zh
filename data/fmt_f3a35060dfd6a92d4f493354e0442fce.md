
# 定向数据 Deepseek生成内容

> 来源：[https://rxq0zfnkm77.feishu.cn/docx/KCOmdePRsoXVWkxNOPLcOfBWnqc](https://rxq0zfnkm77.feishu.cn/docx/KCOmdePRsoXVWkxNOPLcOfBWnqc)

以下是一个基于提供的表格文件生成的综合数据看板，使用 HTML 格式输出并确保文件可执行。看板包含柱状图和折线图的可视化方式，展示基础数据信息。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>供应商分类数据看板</title>
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
        <h1>供应商分类数据看板</h1>
        
        <!-- 供应商采购金额图表 -->
        <div class="chart-container">
            <div class="chart-title">供应商采购金额（柱状图）</div>
            <div class="chart-subtitle">展示各供应商的采购金额分布</div>
            <div id="supplierAmountChart" style="height: 400px;"></div>
        </div>

        <!-- 月度采购总额图表 -->
        <div class="chart-container">
            <div class="chart-title">月度采购总额（折线图）</div>
            <div class="chart-subtitle">展示各月采购总额的变化趋势</div>
            <div id="monthlyPurchaseChart" style="height: 400px;"></div>
        </div>
    </div>

    <script>
        // 供应商采购金额数据
        const supplierAmountData = {
            categories: ['Supplier A', 'Supplier B', 'Supplier C', 'Supplier D', 'Supplier E', 'Supplier F', 'Supplier G', 'Supplier H'],
            series: [
                {
                    name: '采购金额（元）',
                    type: 'bar',
                    data: [45091, 45100, 45250, 45320, 45410, 45530, 45650, 45780]
                }
            ]
        };

        // 月度采购总额数据
        const monthlyPurchaseData = {
            categories: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
            series: [
                {
                    name: '采购总额（元）',
                    type: 'line',
                    data: [30000, 35000, 41000, 38000, 42000, 40000, 45000, 43000, 46000, 44000, 47000, 48000]
                }
            ]
        };

        // 初始化图表
        const supplierAmountChart = echarts.init(document.getElementById('supplierAmountChart'));
        const monthlyPurchaseChart = echarts.init(document.getElementById('monthlyPurchaseChart'));

        // 设置图表选项
        supplierAmountChart.setOption({
            title: {
                text: '供应商采购金额'
            },
            tooltip: {},
            xAxis: {
                data: supplierAmountData.categories
            },
            yAxis: {
                type: 'value'
            },
            series: supplierAmountData.series
        });

        monthlyPurchaseChart.setOption({
            title: {
                text: '月度采购总额'
            },
            tooltip: {},
            xAxis: {
                data: monthlyPurchaseData.categories
            },
            yAxis: {
                type: 'value'
            },
            series: monthlyPurchaseData.series
        });
    </script>
</body>
</html>
```

## 使用说明

1. 将上述代码保存为 `.html` 文件（例如 `supplier_dashboard.html`）。
2. 使用浏览器（如 Chrome、Firefox、Edge 等）打开该文件。
3. 图表将自动加载并显示，包含以下内容：

+ **供应商采购金额**：柱状图展示各供应商的采购金额分布。
+ **月度采购总额**：折线图展示各月采购总额的变化趋势。

## 数据说明

+ **供应商采购金额**：展示各供应商的采购金额分布，帮助分析供应商采购情况。
+ **月度采购总额**：展示各月采购总额的变化趋势，帮助分析采购节奏和季节性变化。

## 注意事项

1. 数据为示例数据，实际使用时请替换为真实数据。
2. 图表需要连接互联网以加载 `ECharts` 库，如果需要离线使用，请下载 `ECharts` 库并调整路径。

通过该看板，可以直观地了解供应商采购情况和月度采购总额的变化趋势，辅助采购管理和决策分析。
