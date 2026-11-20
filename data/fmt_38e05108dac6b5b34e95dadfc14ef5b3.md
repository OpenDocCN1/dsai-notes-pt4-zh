
# FinGLM：开放、公益、持久的金融大模型项目

> 来源：[https://ai.feishu.cn/docx/IeCjdoJbCo5ftOx72GbcWJstnqe](https://ai.feishu.cn/docx/IeCjdoJbCo5ftOx72GbcWJstnqe)

## 项目介绍

> 📈 一个旨在深度解析上市公司年报的对话交互智能系统。面对金融文本中的专业术语与暗含信息，我们致力于用AI实现专家级别的金融分析。
>
> 🚀 在AI领域，虽然已在文本对话取得进展，但真正的金融交互场景仍然是一个巨大挑战。多方机构联手举办此次竞赛，探索金融领域AI的边界。
>
> 📘 上市公司年报为投资者呈现了公司的经营状况、财务状况和未来规划。专业知识是解读的关键，而我们的目标是通过AI技术让这一过程变得更简单、更准确。

> 项目地址：https://github.com/MetaGLM/FinGLM

Demo：

![](img/872b2aefc555ab396a53eac87e984d90.png)

[5468_1700632485.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/L8GebaIt4o3pqXxao5Sc5BHlnOg)

## 🛠 项目框架

### 数据准备流程

+   [`PDF 转 TXT`](https://github.com/MetaGLM/FinGLM/blob/main/tools/pdf_to_txt)：

+   转换为 TXT 格式。
+   保留表格并合并单元格。

+   数据切分：

+   基础信息：例如公司名称等。
+   财务数据：例如资产负债表等。
+   综合信息：例如财务指标等。

+   数据处理：

+   计算基础公式：如营业成本率等。
+   计算增长率。
+   计算行业均值和排名。

+   存入数据库：

+   存入 SQL、Mongo 和 ES 中。
+   包括建表及存储。

### 模型微调流程

+   数据分类：如 SQL 数据、ES 数据等。
+   选择微调策略：例如 `ptuningv2`、`lora` 等。
+   执行微调：根据选定策略。

### 问答流程

+   输入问题：用户输入问题。
+   `Prompt` 准备：根据问题生成 prompt。
+   生成查询语句：基于 GPU 使用率选择生成方法。
+   查询数据库：并返回结果。
+   答案生成：结合问题和查询结果生成答案。

---

## 🌱 开源路线图

### 开源策略

1）**赛事转型**

+   比赛转型为[学习赛](https://tianchi.aliyun.com/competition/entrance/532126)，允许任何人学习使用。

2）**数据开源**

+   目前开源数据有 [70G/1w+份年报数据](https://github.com/MetaGLM/FinGLM/blob/main/data/reports)、[10000条人工标注评测数据](https://github.com/MetaGLM/FinGLM/blob/main/data)等。
+   后续我们也将根据项目需求，持续迭代更新数据。

3）**方案/代码/模型开源**

+   经同意，[结婚买房代代韭菜](https://github.com/MetaGLM/FinGLM/blob/main/code/%E7%BB%93%E5%A9%9A%E4%B9%B0%E6%88%BF%E4%BB%A3%E4%BB%A3%E9%9F%AD%E8%8F%9C)、[Chatglm反卷总局](https://github.com/MetaGLM/FinGLM/blob/main/code/Chatglm%E5%8F%8D%E5%8D%B7%E6%80%BB%E5%B1%80)、[nsddd](https://github.com/MetaGLM/FinGLM/blob/main/code/nsddd)、[馒头科技](https://github.com/MetaGLM/FinGLM/blob/main/code/%E9%A6%92%E5%A4%B4%E7%A7%91%E6%8A%80)、[南哪都队](https://github.com/MetaGLM/FinGLM/blob/main/code/%E5%8D%97%E5%93%AA%E9%83%BD%E9%98%9F)、[龙盈战队](https://github.com/MetaGLM/FinGLM/blob/main/code/%E9%BE%99%E7%9B%88%E6%88%98%E9%98%9F)、[安硕硕眼探企](https://github.com/MetaGLM/FinGLM/blob/main/code/finglm_all)、[随便取个名](https://github.com/MetaGLM/FinGLM/tree/main/code/%E9%9A%8F%E4%BE%BF%E5%8F%96%E4%B8%AA%E5%90%8D)等团队的方案、代码、模型完全开源，纳入FinGLM项目。
+   我们将长期维护优化 [FinGLM 项目](https://github.com/MetaGLM/FinGLM/blob/main/code/finglm_all)，提供便捷解决方案。

4）**开放交流**

+   以上团队成员将共同维护 FinGLM项目，确保项目持续迭代。我们也欢迎更多团队来共同贡献问题和方案。
+   我们将不定期组织线上、线下交流，将更优秀的技术推广给每个项目成员。
+   [FinG](https://github.com/MetaGLM/FinGLM#%E8%B4%A1%E7%8C%AE%E8%80%85)LM 开源项目出于完全公益目的，欢迎所有开发者申请加入，当然我们会进行严格审核。

5）**学习教程**

+   基于FinGLM项目的开发，我们将整合并制作以下（包含且不限于）学习教程.
+   数据预处理教程
+   数据库使用教程
+   GLM的使用教程
+   Prompt编写教程
+   模型微调数据准备
+   模型微调技巧和步骤
+   全流程落地

6）**项目资源池**

+   为了维持项目的健康发展，部分项目组织者（个人和企业）提供 10 万元作为开源项目资金池，以及提供项目算力、数据和模型支持。
+   我们欢迎所有受益于本项目的个人或单位[来赞助本](https://github.com/MetaGLM/FinGLM#%E5%BC%80%E6%BA%90%E8%B5%9E%E5%8A%A9)项目，包括且不限于以上内容，欢迎联系我们。

### 开源进度

#### 第一期【done】

+    组织 SMP 2023 ChatGLM 金融大模型挑战赛
+    比赛数据集开源
+   年报数据集
+    [PDF 文件](https://modelscope.cn/datasets/modelscope/chatglm_llm_fintech_raw_dataset/summary)。包含 11588 份 2019 年至 2021 年期间的部分上市公司年度报告。
+    [TXT 文件](https://modelscope.cn/datasets/modelscope/chatglm_llm_fintech_raw_dataset/summary)。利用 [`pdf2txt.py`](https://github.com/MetaGLM/FinGLM/blob/main/tool/pdf_to_txt) 对 PDF 文件解析而来。
+    [HTML 文件](https://modelscope.cn/datasets/modelscope/chatglm_llm_fintech_raw_dataset/summary)。
+   数据库接入
+    sqlite
+    mongodb
+    决赛项目开源：
+    [馒头科技](https://github.com/MetaGLM/FinGLM/blob/main/code/%E9%A6%92%E5%A4%B4%E7%A7%91%E6%8A%80)
+    [南哪都队](https://github.com/MetaGLM/FinGLM/blob/main/code/%E5%8D%97%E5%93%AA%E9%83%BD%E9%98%9F)
+    [Chatglm反卷总局](https://github.com/MetaGLM/FinGLM/blob/main/code/Chatglm%E5%8F%8D%E5%8D%B7%E6%80%BB%E5%B1%80)
+    [nsddd](https://github.com/MetaGLM/FinGLM/blob/main/code/nsddd)
+    [龙盈战队](https://github.com/MetaGLM/FinGLM/blob/main/code/%E9%BE%99%E7%9B%88%E6%88%98%E9%98%9F)
+    [结婚买房代代韭菜](https://github.com/MetaGLM/FinGLM/blob/main/code/%E7%BB%93%E5%A9%9A%E4%B9%B0%E6%88%BF%E4%BB%A3%E4%BB%A3%E9%9F%AD%E8%8F%9C)
+    [安硕硕眼探企](https://github.com/MetaGLM/FinGLM/blob/main/.code/finglm_all)
+   新增项目：
+   [随便取个名](https://github.com/MetaGLM/FinGLM/tree/main/code/%E9%9A%8F%E4%BE%BF%E5%8F%96%E4%B8%AA%E5%90%8D)

#### 第二期【ing】

+    微调Fintune
+    完善 `nl2sql`
+    增加学习教程
+    数据预处理教程
+    数据库使用教程
+    GLM的使用教程
+    Prompt编写教程
+    模型微调数据准备
+    模型微调技巧和步骤
+    全流程落地
+    问答系统异常处理
+    提供详细使用手册
+    文档注释完善

---

## 🏆 开源项目

项目介绍博客：

+   一张图读懂ChatGLM金融大模型
+   ChatGLM金融开源FinGLM学习笔记

### 0. `FinGLM_all`

[[PPT]](https://github.com/MetaGLM/FinGLM/blob/main/slides/%E5%AE%89%E7%A1%95%E7%A1%95%E7%9C%BC%E6%8E%A2%E4%BC%81%E5%88%86%E4%BA%AB%E5%8F%8AFinGLM%E5%BC%80%E6%BA%90%E5%8F%91%E5%B8%83.pdf) [[视频]](https://www.bilibili.com/video/BV1ip4y1F7Gw/)[[代码]](https://github.com/MetaGLM/FinGLM/blob/main/code/finglm_all)

本项目为安硕硕眼探企团队，根据自己的项目以及其他几队的项目整合而成。后续我们也将围绕此项目进行持续迭代升级。

![](img/ee87aad1e4b15b1c2f963e51cd8845b1.png)

### 馒头科技

[[PPT]](https://github.com/MetaGLM/FinGLM/blob/main/slides/%E9%A6%92%E5%A4%B4%E7%A7%91%E6%8A%80.pdf) [[视频]](https://www.bilibili.com/video/BV18h4y187UU/) [[代码]](https://github.com/MetaGLM/FinGLM/blob/main/code/%E9%A6%92%E5%A4%B4%E7%A7%91%E6%8A%80)

![](img/5c277f0ae40857a593765d7184409b4e.png)

### 南哪都队

[[PPT]](https://github.com/MetaGLM/FinGLM/blob/main/slides/%E5%8D%97%E5%93%AA%E9%83%BD%E9%98%9F.pdf) [[视频]](https://www.bilibili.com/video/BV1Gm4y1V7LD/) [[代码]](https://github.com/MetaGLM/FinGLM/blob/main/code/%E5%8D%97%E5%93%AA%E9%83%BD%E9%98%9F)

![](img/2fcc006b20ff21cbd2f5ee0f5d7784e7.png)

### Chatglm反卷总局

[[PPT]](https://github.com/MetaGLM/FinGLM/blob/main/slides/ChatGLM%E5%8F%8D%E5%8D%B7%E6%80%BB%E5%B1%80.pdf) [[视频]](https://www.bilibili.com/video/BV1hu4y147EW/) [[代码]](https://github.com/MetaGLM/FinGLM/blob/main/code/Chatglm%E5%8F%8D%E5%8D%B7%E6%80%BB%E5%B1%80)

![](img/f9aa799cf9ea42ceaa57f2ab2ea50eb0.png)

### nsddd

[[PPT]](https://github.com/MetaGLM/FinGLM/blob/main/slides/nsddd.pdf) [[视频]](https://www.bilibili.com/video/BV15u4y147Xx) [[代码]](https://github.com/MetaGLM/FinGLM/blob/main/code/nsddd)

![](img/37acfd3be3e9c8336f5b3813ccadb935.png)

### 龙盈战队

[[PPT]](https://github.com/MetaGLM/FinGLM/blob/main/slides/%E9%BE%99%E7%9B%88%E6%88%98%E9%98%9F.pdf) [[视频]](https://www.bilibili.com/video/BV1Ju4y167ew) [[代码]](https://github.com/MetaGLM/FinGLM/blob/main/code/%E9%BE%99%E7%9B%88%E6%88%98%E9%98%9F)

![](img/862af4baff1ed6114334a1192d606873.png)

### 结婚买房代代韭菜

[[PPT]](https://github.com/MetaGLM/FinGLM/blob/main/slides/%E7%BB%93%E5%A9%9A%E4%B9%B0%E6%88%BF%E4%BB%A3%E4%BB%A3%E9%9F%AD%E8%8F%9C.pdf) [[视频]](https://www.bilibili.com/video/BV1rm4y1G7uj) [[代码]](https://github.com/MetaGLM/FinGLM/blob/main/code/%E7%BB%93%E5%A9%9A%E4%B9%B0%E6%88%BF%E4%BB%A3%E4%BB%A3%E9%9F%AD%E8%8F%9C)

![](img/32f23b938777e620cd18b6de4329fef9.png)

![](img/9ac8380e4d6b479526956acb85f53404.png)

### TabIsabaopilong

[[PPT]](https://github.com/MetaGLM/FinGLM/blob/main/slides/TabIsabaopilong.pdf) [[视频]](https://www.bilibili.com/video/BV1894y1a7NJ/) [代码]

![](img/02fd529d07bafcbb77ae1852d1656386.png)

![](img/3b9a73cc0c10f6f9ed6e17dfa201f161.png)

### 饺子研究院

[[PPT]](https://github.com/MetaGLM/FinGLM/blob/main/slides/%E5%90%83%E8%BE%A3%E5%AD%90.pdf) [[视频]](https://www.bilibili.com/video/BV12z4y1V7S3/?spm_id_from=333.999.0.0&vd_source=df16438efe36af5724526b8869fb54c1) [代码]

![](img/b8c5ffc9a2cbc29ca7763627f116d65d.png)

### 流宝真人

[[PPT]](https://github.com/MetaGLM/FinGLM/blob/main/slides/%E6%B5%81%E5%AE%9D%E7%9C%9F%E4%BA%BA.pdf) [[视频]](https://www.bilibili.com/video/BV1QF411m7ap) [代码]

![](img/c245154cf75a26b20147e8edeed7a60c.png)

### 随便取个名

[PPT] [视频] [[代码](https://github.com/MetaGLM/FinGLM/tree/main/code/%E9%9A%8F%E4%BE%BF%E5%8F%96%E4%B8%AA%E5%90%8D)]

---

## 📚 数据集
