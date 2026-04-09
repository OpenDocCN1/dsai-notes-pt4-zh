# 小七姐：通过数据分析器+dify 快速创建数据集

> 来源：[https://waytoagi.feishu.cn/docx/EbBldqjbOo3sLrxjW08cvor5nje](https://waytoagi.feishu.cn/docx/EbBldqjbOo3sLrxjW08cvor5nje)

> 作者：小七姐

> [**可以在以下地址关注她，主页内**](https://t.zsxq.com/15KhQ0RE7)容更丰富：小七姐的prompt学习社群

## 流程

![](img/525dd79dcd338e1c4df7c476f3d3d31c.png)

## 1、原始文档格式转换

利用第三方工具将原始文档转换成txt工具，经过测试，代码解释器对TXT的识别度较高，特别是中文语言编码情况下。

这里使用的是wps的转换工具：

![](img/797ed2dac0611e25f3fc084609559f2a.png)

输出后打开txt文档，手动删除掉目录页：

![](img/03ecb382da2d14381a9f5b6fdbbea1bd.png)

## 2、代码解释器操作

### 1、导入文件到代码解释器，进行初步格式调整

![](img/48f16bf5a82feddeeca6137464fff4d8.png)

![](img/2df2834d0ddb2a88cfb09287b8c9459b.png)

![](img/07da876ec2807512da31f2f2cf2c1a4c.png)

### 2、格式重新调整

![](img/8a8639dee1d9ffe65f64cea9f33e4111.png)

![](img/87786da2af01da0d942c5c16dd628e9f.png)

## 3、导入Dify进行数据集训练

![](img/55293e9748dd332ff51704ae9a0a0724.png)

![](img/d5fe2f1b4289d87f9cde7e78fb75927b.png)

此处建议使用**经济模式**，高质量索引会出现错误，目前尚不清楚原因，待解决。

![](img/80ffc0fb7a9c9b11660d9d1bc379ae9d.png)

## 4、创建bot

### 创建并命名

![](img/5ca053bb2e5021267c0f9f1128ef2f32.png)

### 点击提示词编排，输入提示词并设置上下文数据集

![](img/fad7949c8170d204685ebc122767c995.png)

### 模型设置

此处建议选择claude2 ，对比3.5质量更高

![](img/e5cbfe4bc03c10d2a3c73613c644bc8c.png)

![](img/b2b4e965c82a13b1479d7088d0f40b6e.png)

### 测试

#### 原始文档：

#### GPT3.5

![](img/5b04ae02fd51a97ba57c76f64da5f6a9.png)

#### claude2

![](img/ec1e023789d975bde7d760c4ea8a17ce.png)