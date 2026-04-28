# 简单版 Deepseek生成内容

> 来源：[https://rxq0zfnkm77.feishu.cn/docx/B6PAdH2VDo870exTkE8cgxSGnZc](https://rxq0zfnkm77.feishu.cn/docx/B6PAdH2VDo870exTkE8cgxSGnZc)

以下是一个新能源汽车行业综合数据看板的HTML实现，包含可视化图表和交互设计，可直接在浏览器中运行：

```
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;zh-CN&quot;&gt;
&lt;head&gt;
    &lt;meta charset=&quot;UTF-8&quot;&gt;
    &lt;title&gt;新能源汽车行业综合数据看板&lt;/title&gt;
    &lt;!-- 引入ECharts --&gt;
    &lt;script src=&quot;https://cdn.bootcdn.net/ajax/libs/echarts/5.4.2/echarts.min.js&quot;&gt;&lt;/script&gt;
    &lt;style&gt;
        body {
            font-family: &quot;Microsoft YaHei&quot;, Arial, sans-serif;
            margin: 20px;
            background-color: #f5f7fa;
        }
        .dashboard-title {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 30px;
        }
        .chart-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(600px, 1fr));
            gap: 20px;
            padding: 20px;
        }
        .chart-card {
            background: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 2px 12px rgba(0,0,0,0.1);
        }
        .chart-title {
            font-size: 16px;
            color: #34495e;
            margin-bottom: 15px;
            font-weight: bold;
        }
    &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1 class=&quot;dashboard-title&quot;&gt;2024新能源汽车行业数据看板&lt;/h1&gt;
    
    &lt;div class=&quot;chart-container&quot;&gt;
        &lt;!-- 销量对比 --&gt;
        &lt;div class=&quot;chart-card&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;主要品牌月度销量对比（单位：辆）&lt;/div&gt;
            &lt;div id=&quot;salesChart&quot; style=&quot;height:400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;

        &lt;!-- 市场份额 --&gt;
        &lt;div class=&quot;chart-card&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;市场份额分布&lt;/div&gt;
            &lt;div id=&quot;marketShareChart&quot; style=&quot;height:400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;

        &lt;!-- 趋势分析 --&gt;
        &lt;div class=&quot;chart-card&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;近12个月销量趋势&lt;/div&gt;
            &lt;div id=&quot;trendChart&quot; style=&quot;height:400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;

        &lt;!-- 续航分布 --&gt;
        &lt;div class=&quot;chart-card&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;主流车型续航里程分布&lt;/div&gt;
            &lt;div id=&quot;rangeChart&quot; style=&quot;height:400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;

        &lt;!-- 充电设施 --&gt;
        &lt;div class=&quot;chart-card&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;充电桩覆盖率分析&lt;/div&gt;
            &lt;div id=&quot;chargingChart&quot; style=&quot;height:400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;

        &lt;!-- 价格分布 --&gt;
        &lt;div class=&quot;chart-card&quot;&gt;
            &lt;div class=&quot;chart-title&quot;&gt;车型价格分布（万元）&lt;/div&gt;
            &lt;div id=&quot;priceChart&quot; style=&quot;height:400px;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;

    &lt;script&gt;
        // 销量柱状图
        let salesChart = echarts.init(document.getElementById(&#x27;salesChart&#x27;));
        salesChart.setOption({
            tooltip: {},
            xAxis: {
                data: [&#x27;比亚迪&#x27;, &#x27;特斯拉&#x27;, &#x27;蔚来&#x27;, &#x27;小鹏&#x27;, &#x27;理想&#x27;, &#x27;哪吒&#x27;]
            },
            yAxis: {},
            series: [{
                name: &#x27;销量&#x27;,
                type: &#x27;bar&#x27;,
                data: [32000, 28000, 15000, 12000, 18000, 8000],
                itemStyle: {
                    color: &#x27;#5470c6&#x27;
                }
            }]
        });

        // 市场份额饼图
        let marketChart = echarts.init(document.getElementById(&#x27;marketShareChart&#x27;));
        marketChart.setOption({
            tooltip: { trigger: &#x27;item&#x27; },
            series: [{
                type: &#x27;pie&#x27;,
                radius: [&#x27;40%&#x27;, &#x27;70%&#x27;],
                data: [
                    { value: 35, name: &#x27;纯电动&#x27; },
                    { value: 25, name: &#x27;插电混动&#x27; },
                    { value: 20, name: &#x27;增程式&#x27; },
                    { value: 15, name: &#x27;氢燃料&#x27; },
                    { value: 5, name: &#x27;其他&#x27; }
                ],
                emphasis: {
                    itemStyle: {
                        shadowBlur: 10,
                        shadowOffsetX: 0,
                        shadowColor: &#x27;rgba(0, 0, 0, 0.5)&#x27;
                    }
                }
            }]
        });

        // 趋势折线图
        let trendChart = echarts.init(document.getElementById(&#x27;trendChart&#x27;));
        trendChart.setOption({
            xAxis: {
                type: &#x27;category&#x27;,
                data: [&#x27;1月&#x27;,&#x27;2月&#x27;,&#x27;3月&#x27;,&#x27;4月&#x27;,&#x27;5月&#x27;,&#x27;6月&#x27;,&#x27;7月&#x27;,&#x27;8月&#x27;,&#x27;9月&#x27;,&#x27;10月&#x27;,&#x27;11月&#x27;,&#x27;12月&#x27;]
            },
            yAxis: { type: &#x27;value&#x27; },
            series: [{
                data: [82000, 93200, 90100, 93400, 129000, 133000, 132000, 145000, 158000, 167000, 175000, 182000],
                type: &#x27;line&#x27;,
                smooth: true,
                areaStyle: {
                    color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                        { offset: 0, color: &#x27;#83bff6&#x27; },
                        { offset: 1, color: &#x27;#188df0&#x27; }
                    ])
                }
            }]
        });

        // 续航环形图
        let rangeChart = echarts.init(document.getElementById(&#x27;rangeChart&#x27;));
        rangeChart.setOption({
            tooltip: { trigger: &#x27;item&#x27; },
            series: [{
                type: &#x27;pie&#x27;,
                radius: [&#x27;50%&#x27;, &#x27;70%&#x27;],
                data: [
                    { value: 25, name: &#x27;400km以下&#x27; },
                    { value: 35, name: &#x27;400-600km&#x27; },
                    { value: 30, name: &#x27;600-800km&#x27; },
                    { value: 10, name: &#x27;800km以上&#x27; }
                ],
                roseType: &#x27;radius&#x27;
            }]
        });

        // 充电桩仪表盘
        let chargingChart = echarts.init(document.getElementById(&#x27;chargingChart&#x27;));
        chargingChart.setOption({
            series: [{
                type: &#x27;gauge&#x27;,
                progress: { show: true },
                detail: { valueAnimation: true, fontSize: 20 },
                data: [{ value: 78, name: &#x27;覆盖率&#x27; }],
                axisLine: { lineStyle: { width: 20 } }
            }]
        });

        // 价格分布散点图
        let priceChart = echarts.init(document.getElementById(&#x27;priceChart&#x27;));
        priceChart.setOption({
            xAxis: { type: &#x27;value&#x27; },
            yAxis: { type: &#x27;value&#x27; },
            series: [{
                symbolSize: 20,
                data: [
                    [15, 80], [18, 85], [22, 90], [25, 88],
                    [28, 82], [32, 78], [35, 75], [40, 70]
                ],
                type: &#x27;scatter&#x27;,
                color: &#x27;#ee6666&#x27;
            }]
        });
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
```

这个数据看板具有以下特点：

1.  可视化元素丰富，包含：

+   柱状图（销量对比）

+   饼图（市场份额）

+   折线图（趋势分析）

+   环形图（续航分布）

+   仪表盘（充电设施）

+   散点图（价格分布）

1.  响应式布局设计，自动适配不同屏幕尺寸

1.  现代UI设计风格，包含：

+   卡片式布局

+   阴影效果

+   圆角设计

+   专业配色方案

1.  使用ECharts可视化库，支持交互操作

1.  包含模拟行业数据，涵盖：

+   主流品牌销量

+   技术路线分布

+   市场趋势

+   产品参数分布

+   基础设施情况

使用方法：

1.  将代码保存为.html文件

1.  使用现代浏览器（Chrome/Firefox/Edge）打开

1.  所有图表均可自动渲染，支持鼠标悬停查看详细信息

数据可根据实际需求进行替换，图表类型和样式也可以通过修改配置项进行定制化调整。