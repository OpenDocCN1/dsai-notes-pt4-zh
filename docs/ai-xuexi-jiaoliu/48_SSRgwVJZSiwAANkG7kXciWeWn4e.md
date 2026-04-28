# 生成各种图表、PPT、设计

> 来源：[https://zcareers.feishu.cn/docx/K2RfdhcvAoSwULx4LXtczBUIn7f](https://zcareers.feishu.cn/docx/K2RfdhcvAoSwULx4LXtczBUIn7f)

## 流程图、时序图、类图、状态图、甘特图等

### 1.1 先用deepseek生成mermaid内容

**核心****：在给D**eep**Seek的提示词中加上图表类**型，以及“以Mermaid格式输出”

![](img/ada0a8bf995682edfd099f5888c0b401.png)

### 1.2 用其它工具生成图表

**draw.io地址：**https://app.diagrams.net/

**操作：**复制mermaid代码到**draw.io**，点击调整图形-》插入-》高级-》Mermaid，粘贴到输入框，点插入

![](img/78955ce1114845b128e44acaeb3158b9.png)

## 思维导图

下面介绍使用三个工具来生成思维导图。

### 2.1 Mermaid Live Editor

#### 一、先用deepseek生成mermaid内容

比如：我想梳理一本书的内容，生成一张思维导图。可以把文档上传，让它先梳理内容，然后根据梳理的内容制作思维导图。

```
我想了解一下这本书，请帮我梳理一下这本书的主要内容，主要讲了哪些内容（并标注章节方便查看）
```

![](img/428409e87864a839029ec1d6330507ae.png)

然后让deepseek生成思维导图。

**核心：同样是提示中增加“****用Marmaid格式****”来输出**

```
请将上面梳理的内容，用Marmaid语法绘制一张思维导图
```

![](img/908ae63f35a538cd79a8d31a93b53bd5.png)

#### 二、然后生成思维导图

**地址：**https://mermaid.live/

**优点：使用方便快捷。**

**操作：**复制deepseek生成的mermaid内容，粘贴到左侧空白处，右侧就直接生成思维导图了。

点左下角的“Actions”--》“PNG”，就可以下载下来。

![](img/ac5fb23d6cda3666802e3200090d5036.png)

### 2.2 draw.io

同上：同样是先让deepseek来生成内容，并用mermaid格式输出。这次用draw.io来生成思维导图。

**地址：**https://app.diagrams.net/

**优点：可以修改。**

**操作：**过程图如下。

![](img/fe2e3d85aa1cfefd8ff3c844b1e4d7e6.png)

![](img/82ddb3b9f2d318dfdcd19a732bc2f003.png)

![](img/40730fdb9093f29a43425de4c693006e.png)

![](img/09350124c2632c070e608fecf8c10bf1.png)

这就是生成的思维导图，右侧可以对节点的颜色和文字内容进行修改，及其它增删等操作。

![](img/6fb1ae691bfa3fb5599e491da65bf4a1.png)

点“文件”--》“导出为”--》选择导出格式，就可以导出了。

![](img/f6d2701c5f25b2d82740e95b2ecd3b38.png)

### 2.3 xmind

如果已经习惯使用xmind来制作思维导图，那则需要让deepseek来生成markdown格式的内容。

**核心**：给DeepSeek的指令中加上“以markdownd格式输出”

![](img/b40d41d9bccea4c3a4c837d0a2d595e8.png)

**

操**作：

1.  新建一个txt文件，复制deepseek生成的markdown文本到这个txt文件中，保存。

1.  把文件后缀改成.md。

1.  然后把.md文件导入到Xmind中，如下图。选择刚刚创建的.md文件导入。

**xmind下载地址**：https://xmind.cn/ （需要安装）

![](img/c8171716860a1b4bc777ca2447af8d0e.png)

![](img/3117bb5e0e2d73f5fba2f548902b368f.png)

## React图表

### 3.1 折线图

```
请根据以下数据用Mermaid语法生成一个“折线图”，来显示2024年整年从1月到12月份“我的体重变化”，数据分别为：[74,76,75,72,69,70,69,68,65,66,67,66]，折线图中的文字使用英文展示
```

![](img/fbd79d5a603e2fc0b0eeea52e9b53006.png)

Mermaid Live Editor：

![](img/3800ba7d13000d01c86fc12600d5b953.png)

**draw.io:**

![](img/9bfa440eacc6498790086b5bdff2deaf.png)

### 3.2 饼图

```
请用marmind语法生成一个“饼图”，来展示以下数据：
2024年我的支出情况：​住房占据了总支出的 ​30%，交通占比 ​15%，​餐饮​占比 ​25%，​娱乐占比​15%，​购物占比​10%，其他​杂项支出占比 ​5%。
```

![](img/397f4ca97e5ea866bcac4eb4a108d388.png)

![](img/1ea9841f52046a8af196d9c63299b2b9.png)

![](img/7fd32fad70232d12edd46a665a01ecb9.png)

### 3.3 雷达图

**Mermai**d不支持雷达图，但可以让deepseek用 Python来生成。

测试数据：

![](img/8445c084e90c285aafbf52015a8eaa66.png)

![](img/57cc358a1dcf953a1c6ddae95907e482.png)

运行效果：

![](img/0fb41794662d81d1840ae78031b21efd.png)

## 矢量图

![](img/d354a3b9050b96f43857c1dc81fb3eae.png)

运行结果：

![](img/2bfe1eae0f260cdaac78b5a484817e0a.png)

## Html文件

![](img/a4991fd324996e3ae7e1ca666dfe6b0d.png)

点“运行HTML”

![](img/d626cc20b0bec4d24b0fe530a2f2136b.png)

运行效果：

![](img/5bc58c2bb229736ebb7eb2924b04db91.png)

如果使用deepseek的平替工具，可能没有直接运行HTML的功能。这时可以把html代码复制下来，创建一个.html文件，使用浏览器打开就可以了，效果是一样的。

## 结合 Kimi 做 PPT

**核心**：给DeepSeek的指令中加上“用markdownd格式输出”

![](img/2da3fe46de348f5cd393c3c898ae4609.png)

**操作**：复制到KIMI的PPT助手输入框中，让它优化一下信息层级，然后点一键生成PPT，选一个模板，然后点生成PPT

![](img/57224d78bb950fb529b2edbeea0e9a5d.png)

![](img/680680c7abeefcbf02cd1e2062d542b7.png)

## 结合AI生图工具（即梦等）

**核心：**让DeepSeek来帮忙写AI生图的提示词

```
按照Midjourney万能提示词公式：主体 + 风格 + 细节 + 光照 + 色彩 + 构图 +  环境 + 情绪 + 视角 + 艺术家/参考 + 质量参数，要求用词简洁准确，去掉语义重复和冲突的内容。先给英文提示词，再给中文翻译。
我想要生成一个皇冠形状的项链，中间主石是一颗大的红宝石，其他部分镶嵌了小的红宝石和钻石
```

## 结合AI视频工具（快手、可灵、海螺等）

**核心：**让DeepSeek来帮忙写镜头语言的提示词

```
按照视频生成的万能提示词公式：主体描述 + 动态行为 + 场景细节 + 镜头语言，要求描述精准，用词准确，先给英文提示词，再给中文翻译。

我要生成一只猫在浴缸前面看着鱼发呆的视频片段，请帮我写好文生视频的提示词。
```