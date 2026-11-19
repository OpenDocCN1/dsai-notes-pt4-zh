
# 可视化

> 来源：[https://docs.feishu.cn/docx/NXKVd8NGXoVCYxxkW1Icw0rXnBL](https://docs.feishu.cn/docx/NXKVd8NGXoVCYxxkW1Icw0rXnBL)
> RyanFcr

# 0 About Class

`https://course.projects.zjuidg.org/`

![](img/99b4287eedbf34ed51125074adb97283.png)

![](img/5497720e5fb5649eafb7313f937cc8ec.png)

![](img/6b431ffe7e6e9e11b4e33ecafa08fb01.png)

HW1

![](img/355886e064b24081a4d07ae9276d7ff2.png)

# 1 Introduction to Visualization

## 1.1 What is data visualization

Input: `data` Output: `visual form` Goal: `insight`

图形符号学

信息可视化——>科学可视化

多学科交叉

计算机+可视化，利用计算机去呈现可视化

Why Visualization？

+   视觉存在误导
+   统计不完全能够呈现数据的特性
+   Helps us think
+   Uses perception to offload cognition
+   Serves as an external aid to augment working memory
+   Boosts our cognitive abilities

可视化分析重要应用：

+   大科学
+   大工程
+   大安全
+   智慧城市
+   互联网与社交媒体
+   体育数据分析
+   体育数据建模、策略分析、模拟理解

Show it don't tell

D3.js

Tableau

+   Tableau 是商业智能可视化分析软件。. 利用 Tableau 查看并理解任何数据。. Tableau Software 可以帮助人们查看并理解数据，可连接到几乎任何数据库，只需拖放即可创建可视化

Power BI

Processing

## 1.2 Visualization is hot & cool & young

![](img/bfadb3331801f0bb38cb2af040b7fd8d.png)

![](img/c5ec12d68669b0cca975138b9d9361e8.png)

![](img/c91b31d045ce454b1700bfc5b64ac67e.png)

+   科学可视化
+   标量
+   向量
+   张量
+   信息可视化（高维数据、抽象数据）
+   可视分析学

## 1.3 How to create visualization?

![](img/7e65a409471a8ec77368f6a1f51731dd.png)

![](img/c680850b07e5ece6df8819a2459f7f02.png)

![](img/72beaf0b3a9a5cf28dd1012465b71068.png)

![](img/edab31ab187dc773e3ea1da35291ffa4.png)

# 2 Data

## 2.1 数据的属性

数据量增长、更新快

大数据和生活息息相关

+   数据的属性
+   类别属性
+   状态。东西的名称
+   区分
+   序数属性
+   能够比较
+   数值属性
+   能够计算
+   连续：需计量或测量获取
+   离散：只能整数单位计算
+   统计方法在数据中的应用是
+   了解数据总体情况的有力工具
+   分析数据的基础
+   数据分布
+   均值
+   中位数
+   方差
+   了解数据对象关系
+   相异性矩阵（下三角矩阵，距离）

![](img/bb8259554cee80ee19dbf75fecacaf15.png)

+   失配比

![](img/322eb315d771ee1c4ab5629da60e56a6.png)

![](img/b4d75042abd6ff09bcd5edf064b6d8c7.png)

不需要考虑都不存在的点

![](img/73eb71ccedca49a020f4249d27731d67.png)

![](img/bbf0402f05a8768c60694693a61c359c.png)

![](img/26272db9b4b5ba4fbcea61b315886480.png)

`P`是阶

+   （了解数据对象关系以）去除冗余数据

## 2.2 数据问题

+   第一范式：几千年前记录和描达自然现象的经验科学；
+   第二范式：数百年前利用模型归纳总结过去记录的现象的理论科学；
+   第三范式：利用科学计算机对复杂现象进行模拟仿真的计算科学；
+   第四范式：计算机将模拟仿真，进行分析总结，得到理论，即数据密集型科学。 (2007年）

获取与预处理数据

存储与分析数据

呈现与解释数据

![](img/308ff806c2234ed52b56723970795cb8.png)

![](img/baea35112b5dab694059e9cd47685554.png)

问题

+   数据质量问题——>数据清洗
+   准确性
+   完整性
+   一致性
+   时效性
+   可信性
+   可解释性

如何发现潜在的数据质量问题？

如何发现未知的错误？

+   数据可视化

## 2.3 可视化数据分析的方法

+   采集数据
+   总结规律
+   形成结论

![](img/e3b812b66addb65b3b5d43ad615d0b84.png)

探索式数据分析可视化分析方法（按信息复杂程度）

+   原始数据可视化

![](img/f1cb59ff4fd08206e44e84c543598403.png)

![](img/30dc1df13ed724d1a1fce86a339dbf68.png)

![](img/41965b6eb8b7228f593d18b046f703ff.png)

![](img/a6fdb9e17eddc36f9c1fe65791f7eea5.png)

![](img/b468e1481ce5a7acb8762ffedf11502f.png)

![](img/92b4f5268d77b53c8d6695f0bcd6b402.png)

![](img/3f5d4a5c2e948dfdcbef88b00ee5fa1c.png)

![](img/144e235fa0fdc48628fc54236c699f51.png)

![](img/7b1945d23b38c34c90f9585856f75433.png)

+   统计结果可视化

![](img/26fe459960def320c3a8af6cf68aace6.png)

+   多协同视图
+   每个图满足不同的需求，所以有个思路是把所有视图结合起来

![](img/be37975ccd9566de974cedfa40045669.png)

数据挖掘

![](img/c01feeb60a1e20c285c8be7fbb765b19.png)

![](img/5c710a6f701eac4630130a787966f189.png)

![](img/1c3d70c82bc953da759b134b660e5a54.png)

![](img/216c2f493bf0d7675609eff34c2779ca.png)

+   描述型任务

![](img/dc2ea9a38b4aa7b006405a28b4f0b058.png)

+   预测型任务
+   分类（寻找一种分类的方法）
+   演化分析（针对时空行为，预言未知）
+   统计方法
+   回归分析
+   参数估计
+   机器学习
+   决策树
+   神经网络
+   传统算法
+   K均值算法
+   K临近算法

人为判断（人机互补)

+   利用可视化

# 3 Perception & Cognition

外部认知的方法

视觉

+   Perception
+   去感受。自然的呈现方式

![](img/f927ac2b28c57254259e9060caef1b63.png)

+   Cognition
+   如何去理解你看见的

![](img/c2fb2a9b84c8358bbf8bb95ee28ae9d0.png)

Working memory很小

+   有选择性的放进我们的内存里，我们只是聚焦于我们关注的内存
+   可视化可以帮我们扩展working memory
+   Shape constancy
+   形状变了，但是我们还是会认为他们是一个东西
+   Size constancy
+   大小变了，但是我们还是会认为他们是一个东西
+   Lightness constancy
+   明亮度横跨六个数量级

感知系统基于相对判断，而非绝对判断

频闪效应——视觉的inlusion

Autokinetic Effect

深度感知，如何获取我们的深度感知

+   单目线索
+   Linear perspective 线性视角
+   Interposition 遮挡
+   Relative size 相对大小
+   Texture gradient 纹理
+   Shadowing
+   双目线索：双目的视差
+   只能通过两只眼睛全神贯注才可以
+   Retinal disparity
+   当一个物体向我们靠近，图像的差距会变大
+   Convergence
+   当一个物体向我们靠近，我们的眼睛会focus在物体上

视觉错位艺术

认知

+   看见一幅图，我们会根据我们的认知去理解这个图片

Gestalt Theory

![](img/910808560c151ad8199c72338a2f6928.png)

![](img/430925e62df4987b4d11649894ae57ea.png)

颜色本质是**波长**

![](img/d14f4ec0eb7733a88b1ca1134c4bd23e.png)

![](img/551a2c4307caf758b60e0c0b1b9344d1.png)

+   颜色
+   饱和度
+   亮度

工具

![](img/302f60ae7cf0d0d00b2a78b5062f0a57.png)

# 4 Visualization Tools

编程——>低代码

交互式

+   Power BI
+   Tableou
+   Gehpi
+   网络

### 配置式可视化工具

+   Vega Lite
+   Echarts

适用场景：

用户：对编程有简单了解数据：简单，结构清晰任务

需求：中等复杂度，交互任务较多

### 编程式可视化工具

+   OpenGL
+   WebGL

![](img/f89434316a1d6b56106bad32ded470d5.png)

+   Pixi.js
+   Processing
+   D3
+   P5.js

# 5 Basics of Data Visualization I

![](img/8845b9b03e647a17b28dfaaeba67edba.png)

[【22】6-Basics of Data Visualization 1.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnKnFOhUqhAZ38HXiktiLbDc)

## 5.1 Visualization Process Model

![](img/bf4d5b7f917f8e904726ab0d5f71a2ad.png)

![](img/1942691842ec83d88d93d89f9c7123ef.png)

+   Data analysis
+   降维
+   Filtering
+   过滤数据
+   Mapping
+   Rendering

![](img/2d2046de6c7f92ebbf85ea857b69bd34.png)

![](img/03b91b87d45213a3f1e3ed23a55e4afa.png)

## 5.2 Visual Encoding Principles

![](img/62216c7626036a222daaceb49e6f3e8b.png)

+   Continuous
+   有顺序，连续
+   没有零点，零点意义不明确，只有比大小，没有比例
+   有比例，有零点
+   Ordered
+   有顺序，离散
+   Categorical
+   类别的

![](img/545914a5f4639b197a9f6256c97f581a.png)

![](img/0df3014dce6145d865945c8f2c72cbd9.png)

![](img/622411bc2f6bc940a673e841f1082418.png)

+   Data model
+   数据抽象：抽象数学
+   Conceptual model
+   有语义的

![](img/cb5e5ac7e766db3d07d275da8285b347.png)

视觉变量

+   位置信息
+   比较大小
+   颜色信息
+   分类

![](img/42deab3789892103b62d1b1ffe794ac9.png)

+   定量，连续
+   顺序，离散
+   类别

![](img/e064603351e73aa9d708b5cc232d61e5.png)

![](img/735a438bfcb294951a96386f93053de4.png)

Eg

![](img/e40e08616cea25c7c54e396a064cf2bb.png)

表现力和有效性

+   一定要matching

![](img/fda1b22359e0caf79829493b7a565cd5.png)

![](img/537bd5388bfab2ce5fd3238be5d5043c.png)

可分离的：很多元素在一起可能会有干扰

包含一组的方式

![](img/90b7ccb5434876221a36295bef1e7ad8.png)

![](img/b4b67afc12db13ca3f224afe12c3b017.png)

+   在大的区域用亮的，饱和度高的
+   在小的区域用低饱和度的

![](img/85e0ed41823539512ff3fdd1dca3ea5f.png)

+   只有少量的颜色可以被有效的使用；我们使用颜色应该控制

![](img/4fc1532d2e4b696c498149357eb3093a.png)

+   用一系列的颜色，改变颜色和饱和度的

![](img/52a049b0ef49a509349bd2ec22d806fd.png)

+   不太好，感知上非线性的

![](img/f2e1f0e99d349a458702160724d50dfb.png)

## 5.3 Visual Analysis Model

![](img/c51d1c98e8c99abde3b23d6cce4da9f9.png)

![](img/e1c4879657125212f0da7fe0a920d4f1.png)

![](img/f083ccb01389302723d0b7735061b54d.png)

![](img/eeed78669718b880b6a9e8a17f9af708.png)

![](img/fa0e0fd219d74ff49a949cbe2c9c7ac6.png)

![](img/010d12b3a23de05337667a00ccd31991.png)

![](img/43a6115759a56ce3404069acdf324a23.png)

![](img/26ba4d9d20c8567f438aad1352f1f081.png)

![](img/8f8b4fa3e405c0bf797a44c8c170d0ea.png)

![](img/b447c5b85feeeacc98a4521267067ea2.png)

# 6 Interaction

![](img/11b703cca837c5d31926e41a9ba8c181.png)

[【22】8-Interactions in Visualization .pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnBDrEtElqs4HiVwWKNw3vTe)

![](img/40de3ee1af605bd0be1660398bd37be4.png)

## 6.1 Types of Interactions

+   Representation
+   Interaction

![](img/5f858bf70c25fd254f0e56a83c68ccf7.png)

![](img/4478220f08de9b959cc8564046020c20.png)

![](img/2b1700262492960667f6732b0054ad32.png)

![](img/8499845b2796fc08111abb565b26b279.png)

+   Select

![](img/c123f7de835baadb8ba2d1da4a6866da.png)

+   Pop-up Tooltips
+   鼠标滑到，提前显示



# 6

+ Hovering mouse cursor brings up details of item.
+ Mouse Selection
+ 选中

![](img/9624210371e7eb2a17b46ad6cf097208.png)
![](img/10dd825266be5f5a8e73410d7774a122.png)

+ Explore

![](img/b6d463f6eafc0e4b2d1d5839ceb07045.png)
![](img/eef924a8eb458c072c017a8688843866.png)
![](img/de96caeb0442e99c4c452b28a15176f3.png)

+ Reconfigure

![](img/e0b0af84c1968e38c7307e5692d98ce8.png)

+ rearrange view改变视图
+ sorting
+ Reposition重新定位
+ Encode

![](img/a037362da3aed9bbf89b7c6a8d73dc73.png)

+ abstract/elaborate

![](img/097b2b8fc649d435aec2675f68715e2b.png)

+ Filter

![](img/ea2f1f3ca0c289712597c9fdbfd1faaf.png)
![](img/05ef6776a51f812d1650266e22f0a317.png)
![](img/8a6b474dd2d4a288ca9c4f059d24ca99.png)
![](img/b22ef6596c4fc6473da1c60176a64d38.png)
![](img/f3f7751bcb77026d2586bae61365b8f7.png)
![](img/7b3828c1851a19539bbaa73058a6839f.png)

+ Connect

![](img/88559e5d4e0604f6306265d1d7db0bd3.png)

## 6.2 Overview + Details

![](img/19522a1df3f6091d1680375313b4d533.png)

数据太大

![](img/dbeb2a7a1ac66e8e3d2123b7653c04dc.png)
![](img/d216c1a6a8647103c6334abab6178b32.png)

+ 卷轴

## 6.3 Focus + Context

+ 鱼眼
+ 增强展示效果

## 6.4 Animation

+ 光滑过渡
+ 动画

## 6.5 Interaction Hardware Design

ARVR

+ Leap Motion
+ Multi-touch

# 7 Basics of Data Visualization Il

![](img/3f0208c46b7f1c2940f3fcfeee637974.png)

[【22】7-Basics of Data Visualization 2.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnG2Wo76MP1oXaSIbjIu36vb)

## 7.1 Transformation

对数据的转换

![](img/62af6c7460ee79959958289940fc6c78.png)
![](img/c118e2b2b9e07d637f79aa4e5116732e.png)

归一化，同一维度

![](img/2974174a3ed98657389e2b25ca8d2cad.png)
![](img/c6d4169b38bfa4e7f558fc57d8c0adc3.png)

找到曲线趋势

采样：信号处理

![](img/d9e0387e02c5ca66b8a569bd243340cb.png)
![](img/8a86e165d48512cea872a0dd4aba1bb5.png)
![](img/f7dbe7386d453e521c1edf5c24859689.png)

模糊离散化，更好理解数据

![](img/d358e1add9d8ab9d26dfb66c82a4c875.png)

降维——二维三维

![](img/51dece147613ffbe6ba12a249a8156ab.png)

聚类

![](img/e20fdf3b5ff854b742ec0414321e042b.png)

先随机，再聚类计算迭代

## 7.2 Statistical Charts

统计报表

![](img/221a4e84baced56157a5434ca1feda17.png)
![](img/3f66cac368fd71f71f9f6dd4d5833cae.png)
![](img/cc94c8b478345589658bed894d7f3013.png)

如何展示更好，识别更有效？所有线段平均等于45度

![](img/00fe3aa927f8c6e8666c79a2de40383f.png)
![](img/eb5c5216ad987b5473a8be9a53c6e257.png)
![](img/8b80a0d8888fbb85676ae835a0770195.png)
![](img/330b37ff909ad1a6dc3b1de1ab3096f8.png)

三种方式

![](img/ef24305adfce34a096f037550be20f73.png)
![](img/0aa87278050b4123026e41362e0e7c3a.png)

增强对应频率

+ 找到相应频率
+ 生成趋势曲线
+ 弯曲到45°
+ 剔除相似的纵横比

![](img/4974154a535f8313eb5682a94865e076.png)

比较小，可以加入到文本里

![](img/5bfa8057fc69a1a2fc1c2a2d9ab7050a.png)

消费啤酒的量

![](img/f1d5c50ba803f0c7af09d47132dc0af9.png)

很形象

![](img/1342193812cf12c69594b0dd04fa9772.png)

有正负

![](img/637b53f1ec29ad381eefd0d006c06fa7.png)

不要3D，没有新的信息，存在误解

![](img/01dfdf85143224f4e023d1505b6fabb6.png)

柱状图堆叠

![](img/f3d05245fc61bb12a81d5fa494463727.png)

Pie Chart 饼图

![](img/d75ed8e1adba4a5e1e0d6be6828a97df.png)
![](img/7a0c6a4f66738041bf6ccbe855df4d9f.png)

用于描述两个数据的关系

![](img/921d6c6ed93b579cdef602677ff594b1.png)

拟合，但是拟合不一定可以适用

![](img/c003dbe0816c0d046e2e2645dcd495e4.png)
![](img/00e6df0647da1ade1e64392e641482d7.png)
![](img/2128e1954dac6c711b321345154e838c.png)
![](img/a1e4a9404db09c78998074b623c3ffc7.png)
![](img/720058e663eba5d3944a1c6afce5ceca.png)
![](img/e3d787562a65cafcc3d7c528bdb6a312.png)
![](img/ad731b5595d5fe9fc56138b1850ea2b4.png)
![](img/f67854dcc297e659d188f68affc9ef2c.png)

WebGL

![](img/e0e376c4913da1061aba7de2d4dc79ea.png)
![](img/6e269cad5f0797447f5a005cb81a070b.png)

# 8 Visualization & Coding

# 9 Geographical Data Visualization

![](img/f9d02a79012bb0a2fa5df7aa3b0e28d6.png)

[【22】10-Geographical Visualization.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcngLSVfrClwDkk6mfJgIfD9e)

![](img/e912772b5c3ee3c9ea26b5b8e023fed3.png)
![](img/8738ab489f2700de53b1c36ccf17d5d4.png)
![](img/4990b1d1dbaf159eb1e78bba1a72f4d9.png)
![](img/565e3f0d6717c13d096f9398230dae11.png)

## 9.1 点可视化

![](img/6a4ffd5749a4a9112c395ebe32ba2f26.png)
![](img/2879c9aae4fa0d59334bda6a5ca85f3d.png)

向量点

![](img/2406c547c669b7492e8d81b6b8870034.png)
![](img/23521892b8942809ac87448063935d8a.png)
![](img/da975a3c21b3abe0263cbffe188efb4e.png)

韦恩图

![](img/d58887be3f6b43c5ae9e91ad088849c0.png)

## 9.2 线可视化

![](img/f55e2c3072cd0f8012c575ce8908ca35.png)
![](img/f65ebeda98c7a9c4732053246042c34a.png)
![](img/7f7b993397d9e8dd0d99da7583870d2b.png)

绑定

![](img/81a9807ae01988b7a312dcc18b9b106f.png)

Flow Map

![](img/b8f3456a0bac777cdc2ba8619c06765c.png)
![](img/61f565cab022a0e8f08323a0f1302558.png)

## 9.3 区域可视化

![](img/ffe8cd5a2032c338c0d76e3e1a3ceda5.png)
![](img/7650d58e7c98eaf76b14c4c970f611d2.png)
![](img/f8793e1298f455f2492e70f1f7fe843b.png)
![](img/6e0bf12ebb28e7a344954866398c8ac5.png)
![](img/ef36b7951bd9f39e75de6c071c7a60bd.png)
![](img/1c6e7b95720523dee9bfc2b285e7b131.png)
![](img/70c6a3f64c067e6c5b97d38ed9b874e3.png)

## 9.4 立体可视化

![](img/9f0c48a3fed42b36fdec1e264598abcc.png)
![](img/50919a8aa9442dc1fb9fcae6e04b6690.png)
![](img/eaccf403649ccc19df36afc07fa0696d.png)

## 9.5 地理空间数据组织形式

![](img/96efca2e4377d03fe30776eef52f0022.png)
![](img/89cd34b67c10471306d6792ef655ec6f.png)
![](img/843e50794ab578a66ecb3bc116659288.png)
![](img/678e793d1fc9506935d0838b1ba6f955.png)
![](img/8db07ee911d80c30d6bba09fa63eccd3.png)
![](img/55fbc19639a6f854af9be69767179724.png)
![](img/a7a3b50688ba705fbe5b7bcc4fc6b8ab.png)
![](img/24ff2adbe575dcc624a783bfd75542d2.png)
![](img/349e11459160a27562987e3a32d7193a.png)
![](img/d866b68aa795069ebffcb84415c8386f.png)
![](img/ff8dc5f072275bd140a44974e258b2f2.png)
![](img/a6ab52011a012435fe604db6bf61ca92.png)
![](img/99c14c2a80cc7683409257ee483c5cc0.png)
![](img/faf237e84a0ffc4e034519d1ba77cd17.png)
![](img/88c4b2bfec6ad209e6db1bcca8f84fa9.png)
![](img/d89ee317b92a16bcbb089ef9cf449f35.png)

`http://globe.cid.harvard.edu/`

三维

![](img/a6c2daff1f8518d45bd8c63fe188bd74.png)
![](img/02a9d913c7c7028a2ca01468dafe3250.png)

# 10 Spatial Data Visualization

# 11 Temporal Data Visualization

![](img/b521c7c715fc3d9ba04b1d7009f655af.png)

[【22】12-Temporal Data Visualization.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/boxcnnohSMtzL1xBR1dBmUnO9qg)

## 11.1 Temporal Data Visualization

![](img/60280541cd5c276c5835cc04b0088627.png)

+ 时间数据
+ 时变数据
+ 时序数据

![](img/e12a0bdf31af04ec0d0cadde8ce4d3ad.png)
![](img/3607c2075cfd8d5c390fe0d84afd65f6.png)

折线图

![](img/18bb1bd01e3850a2b1262bbbe6ee5406.png)
![](img/64d29f4a95fa5466cea04c14875104a9.png)

收发邮费的时间可视化——看待自己时间作息

![](img/3d847b039473d046927a12d157d21455.png)

多维

![](img/d81882a51f17f2e64a579c972adb00ae.png)

音乐不同产品销数额

![](img/f13a6d6a598a53e00211fe8102df2b9b.png)

周期性

![](img/6aed6fe7d0888b6d0503bd694d742abd.png)

日历

![](img/c995c5985295037f2170b7f65dc721ef.png)
![](img/65dd06e70fb536c70f38eec852c393f6.png)
![](img/77e41dea2bc6bc14f702fe5331f29927.png)
![](img/f79481497c573bd06542465a7b4bfea6.png)
![](img/413f2c3e9b73da87efcbfaf524d200a9.png)
![](img/5cee8289cf884c61e9d7e1d056e26471.png)

角色和人的交互可视化

数据处理

![](img/76285dac1aa2585247d43ca4e2858a3f.png)
![](img/790cfe24452a038578eb212d09df7178.png)

不只是静态的，还有动画

![](img/d4275285bcca5dfbf415d8ecb9be1dee.png)

`https://flowingdata.com/2015/12/15/a-day-in-the-life-of-americans/?platform=hootsuite`

![](img/794b48c6fffc380d740ebeb492a8ce47.png)
![](img/f2c1eeb2234ffe12ca98fd6a4eb61951.png)

## 11.2 Temporal Spatial Data Visualization

时空

`https://www.youtube.com/watch?v=72-blGAuh6s`

交通信息可视化

![](img/804d4a972d0e1751990ff72249d18b27.png)

历史：航线

![](img/72ee1a17e606cb28e76d242b88c93c2b.png)
![](img/7f63bd4efde36ad737f6d7139e8b3c84.png)
![](img/ef659e76241a4c4032e770803d4e4d9f.png)

共享单车

![](img/b9a7af3f422251b78fbbaef75a51b0e5.png)

## 11.3 Streaming Data Visualization



![](img/def346807d394dd0a62579ccd90d16f3.png)

+   计算相似度

![](img/6f75b4f9c64850de9ff7161d1b16c9ae.png)
![](img/77d1a354dd6aa0096750b88aa9d32717.png)
![](img/beea3c01b62a27722ecae127f1fde633.png)

找到两个序列最短距离，描述相似度

+   表达成符号

![](img/5f4e2788138cebd74f7cea79718a757c.png)
![](img/4d2c0f809f2d399f3ead0c052ad1e35b.png)
![](img/e3deb7c32463e880504d883e00bff2ff.png)

# 11.4 Design Principles

![](img/66418328389afeec138080084946aa41.png)

+   用熟悉的视觉表达
+   为多维度提供并排的比较
+   空间位置是最强的可视化提示
+   多视图通过显式的调节更为有效

![](img/0dc71ff1a22d1af6b24b56a127ea91f7.png)

+   总览为先，缩放过滤，按需查看细节
+   避免突兀视觉改变
+   用户交互要有直接视觉反馈
+   上下文同时展现不同层次的内容

![](img/29d07429095afee6e08e4cb1dd01670a.png)
![](img/70fd8dd584bc0c0d7082508d36f3c02c.png)
![](img/b089f910d52430d837b77293a72ab2e8.png)

# 12 High-dimensional Data Visualization
# 13 Visualization Applications
# 14 Hierarchical Data Visualization
# 15 Network Data Visualization
# 16 Cross-Media Visualization
# 17 Q&A by Teaching Assistants
# 18 Course Project Presentations
