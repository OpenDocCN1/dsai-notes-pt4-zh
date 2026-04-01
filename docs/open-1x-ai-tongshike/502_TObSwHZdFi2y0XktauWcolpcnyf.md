# 从零入门NLP竞赛

> 来源：[https://datawhaler.feishu.cn/docx/LC17dokycof8Zqx2NIdczdwenWe](https://datawhaler.feishu.cn/docx/LC17dokycof8Zqx2NIdczdwenWe)

> **从零入门NLP竞赛 **是 Datawh**ale 202**4 年 AI 夏令营第二期的学习活动（“NLP”方向），基于讯飞开放平台“基于术语词典干预的机器翻译挑战赛”开展的实践学习——

+   适合想 入门并实践 深度学习、解决NLP问题、机器翻译 的学习者参与

+   **只要会用电脑**，就能跟着本文档内容，在** 30分钟内，****完成赛事速通体验！**

+   快来拿下你的第一个 **“****基****于**术语词典干预的机器翻译挑战赛” 分数！

>

> 让我们一起学习进阶，动手实践、写笔记打卡，

> 努力**上分**冲刺**夏令营TOP10**，获得**荣誉证书和奖学金**！

# 小白零基础 30 分钟速通指南

> 整个学习活动会带领大家进行项目制学习，由浅入深，逐渐进阶。

> 当前环节可让你在 **30****分钟**** ****内****，****从一**个完全小白的选手，跑通 “基于术语词典干预的机器翻译挑战赛” ！

> 我们整个学习活动，将带你从 **跑通最简的Baseline**，到**了解竞赛通用流程**、**深入各个竞赛环节**，**精读Baseline与进阶实践**，这些一个都不会少！

> 千里之行，始于足下，从这里，开启你的 AI 学习之旅吧！

> 记得在完成速通后，填写打卡表单抽奖哟~

> —— Datawhale贡献者团队

## Step1：报名赛事！(点击即可跳转)

> 赛事链接：[https://challenge.xfyun.cn/h5/detail?type](https://challenge.xfyun.cn/h5/detail?type=machine-translation-2024&ch=dw24_z741D2)=machine-translation-2024&ch=dw24_z741D2

> **登录后直接报名即可~**

![](img/1128c19a968a0df100a74a81bc015a0e.png)

![](img/a42287bf5e318ee88d454fb829d3c345.png)

![](img/8234f50249e29490f9d4cda3d23fd2de.png)

## Step2：下载代码文件

> 下载下方两个文件即可

+   模型训练的代码脚本文件

![](img/4488864c57ff9d44dbdd768dfcf7c4c0.png)

[task-1_terminology.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BxCKbSIYHof4Pyxu26Tc1piUnrg)

+   训练模型所用到的数据集(赛题指定数据集)

![](img/69fb201b45f6962c8041bc0b0de2edba.png)

[dataset.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YwxnbGeIjoDDFuxnc8OcvJzOnrf)

## Step3：启动魔搭GPU环境（点击即可跳转）

> 魔搭链接：https://modelscope.cn/my/mynotebook/preset

> 选择方式二，启动！

![](img/610f076b80f7c08258d8758ce2a238f4.png)

![](img/fa4d88723f41037a6bba620ccf846f4f.png)

![](img/4d5c8c9b0f690e1c1c984bc1458b348c.png)

+   进入环境时，可能需要登录阿里云账号，登录即可

![](img/2980d583dbd62ee5262faeede6dd0aac.png)

+   新用户需要先绑定阿里云账号

![](img/04c1638a4f76fa022c574d67b9b11ddd.png)

## Step4：运行baseline！

> 上传数据和代码文件，然后**一键运行**！

1.  新建终端

![](img/835a14f20b1b977fd11f6896e5b29e3f.png)

1.  新建目录

> 复制下面的命令，粘贴在终端处并回车运行

```
mkdir MT
cd MT
mkdir code
```

![](img/dfddfefe231808ba1fba8f8097955180.png)

![](img/0369cd28e363bfa9b608adc6c4251f8b.png)

1.  在MT/目录下上传dataset压缩包文件

> 双击打开 MT/ 目录，并拖入dataset压缩包文件完成上传

![](img/1e064d9a2d1694d3afae759a980ba830.png)

1.  在MT/code/目录下上传task-1代码文件

> 双击打开 MT/code/ 目录，并拖入task-1代码文件完成上传

![](img/59b8533b6a89353f78918c403074247b.png)

![](img/3400949c7f406ae829547239e450fdb2.png)

1.  解压数据集

> 复制下面的命令，粘贴在终端处并回车运行

> unzip dataset.zip

![](img/0e9677c28c1431975f15dd17de7e7fa4.png)

![](img/74996889290f5e9e20076812957a3951.png)

1.  打开代码文件，一键运行代码！（大概需要15分钟）

（如果时间过长，检查Step3是不是误开了CPU）

![](img/ad07da5c65621f93304f0b6b0f874dfc.png)

![](img/2e4e8a3d5cd28f2a9d514e816173e0f9.png)

![](img/975dfd3ae920e8abbccadac0322b8c73.png)

> [Task1知识点文档](https://datawhaler.feishu.cn/wiki/FVs2wAVN5iqHMqk5lW2ckfhAncb)待运行完成的过程中，可以看看   中的内容，进一步了解赛题和学习代码哟~

1.  下载 submit.txt 结果文件

![](img/7df508870f4255901ae26dedf2c74d46.png)

1.  运行结束后记得关闭实例

> 在结束开发后，大家记得及时保存环境现场。

> 魔搭Notebook会在没有后续操作的1小时后自动关闭，仅保存 notebook 文件。

> 如果你有其他任何需要保存的文件，请提前下载保存到本地。

> 在确定所有东西已经备份后，最好主动关闭实例~

![](img/75c1c7a7a1ac5328b19048f38d802986.png)

+   如果没有【关闭实例】按钮，可以回到魔搭Notebook页(点击即可跳转)进行实例关闭

> 链接：https://modelscope.cn/my/mynotebook/preset

![](img/040510532860b1d6bf4a0732450185ba.png)

## Step4：提交文件，拿下第一个分数！(点击即可跳转官网进行提交)

> 链接：[ht](https://challenge.xfyun.cn/topic/info?type=machine-translation-2024&option=tjjg&ch=dw24_AtTCK9)tps://challenge.xfyun.cn/topic/info?type=machine-translation-2024&option=tjjg&ch=dw24_AtTCK9

> **提交结果文件，参与排行榜评比**

![](img/617c21fa8a351c34d12591d15c931de9.png)

![](img/14d1739971867169fcacf35fcf2222df.png)

![](img/c2cb83358498fa37437bd28437036333.png)

![](img/d937e03280767bb9101b3a77815f6216.png)

> **评分大概需要 2 分钟～****评分结束后可****以在下****方看到自己****提交的评分结果哟！（预期分数在零点几左右😉~，可****以**自己调整训练数据和epochs继续上分，这里为了快速演示，只使用了部分数据进行训练）

# 🎉恭喜你拿下了属于自己的第一个分数！

## 快来给自己点赞！

## 然后填写表单，开启抽奖吧~祝你好运~

> 请大家提交完作品后填写下面的表单哦~

> 直接在下方填写或点击打开链接：https://datawhaler.feishu.cn/share/base/form/shrcnJbn2cFoIuctLprh5iVDonf

> 参与者还可以参与Datawhale【抽奖】哦~有机会获得Datawhale精美周边小礼品！！

> 也可以把你的分数截图转发到群里呀~

<https://datawhaler.feishu.cn/share/base/form/shrcnJbn2cFoIuctLprh5iVDonf?iframeFrom=docx&ccm_open=iframe>

# 📅 记住下面的学习规划表！

> **教程贡献者说：**

> 这次的机器翻译赛题相较于以往的同类赛题，有以下两个特点：训练集文本更长，预测难度更大；专业化名词很多，需要充分利用提供的术语词典。

> 希望经过本次学习，各位学习者都能对机器翻译有初步的了解，通过进一步的学习，打开NLP学习的大门！

>

> 接下来我们会基于这个赛题进行更多的知识点学习，会涉及到：

1.  基于seq2seq的机器翻译

1.  常用 NLP 模型

1.  Transformer 架构详解

>

> **学习规划表如下：**

| ## 从零入门AI竞赛(NLP方向)**-学习规划表****（7天）** |  |  |  |  |
| --- | --- | --- | --- | --- |
| #### 基于术语词典干预的机器翻译挑战赛 |  |  |  |  |
| **Task** | **Task 描述** | **相关知识点** | **学习文件&amp;链接** | **学习安排** |
| **Task 1** | **目标：**跑通baseline，体验NLP模型解决问题的流程，基本了解赛题要求，理解赛题场景 | 1.  机器翻译任务的发展历程 | 1.  [从零入门NLP竞赛](https://datawhaler.feishu.cn/wiki/TObSwHZdFi2y0XktauWcolpcnyf) | [**开营直播**](https://meeting.tencent.com/v2/cloud-record/share?id=a9f6ce22-61e7-43a7-ba71-dac30dc60675&amp;from=3&amp;is-single=false&amp;record_type=2) |
| **Task 2** | **目标**：深入理解赛题，精读baseline代码，入门机器学习代码实现 | 1.  赛题深入解析：赛题背景 | 1.  [Task2知识点文档](https://datawhaler.feishu.cn/wiki/PztLwkofsi95oak2Iercw9hkn2g) | [**baseline精讲直播**](https://meeting.tencent.com/v2/cloud-record/share?id=2fcf8e58-ca7e-483e-b145-3d8097fd844a&amp;from=3&amp;is-single=false&amp;record_type=2) |
| **Task 3** | **目标**：进阶Transformer模型完成任务，入门深度学习 | 1.  Transformer架构 | 1.  [Task3知识点文档](https://datawhaler.feishu.cn/wiki/OgQWwkYkviPfpwkE1ZmcXwcWnAh) | [**进阶分享直播**](https://meeting.tencent.com/v2/cloud-record/share?id=c0c178a6-dd3f-476c-a566-6175762346b7&amp;from=3&amp;is-single=false&amp;record_type=2) |
| **Task 4** | **目标**：通过代码实践入门大模型处理机器翻译 |  | 1.  [Task4知识点文档](https://datawhaler.feishu.cn/wiki/L64jw94HwivDoQkptGccDMCznxc) |  |
|  |  |  |  |  |

### 祝大家上分顺利，学习愉快~ 不要害怕在群里提问和交流哟！

> 不过建议先查看** **[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/FPFzwD4FWiCpBFkVh0Ucqs1Wntd)** **是否早已解决并收录你的问题~ 然后再在群里仔细描述清楚问题进行提问哟~

> QA文档可任意编辑，可随时留言提问，我们的助教会关注，遇到感兴趣的问题还可以在上面进行回答哟~

# 🍰 再来一点学习加餐！

### 你知道刚刚发生了什么吗？

> 刚刚我们跑通了MVP，想必你一定很好奇这背后发生了什么吧？

> 在学习的过程中，不单单要知道如何做，更要知道为什么要这样做。这样我们在遇到同类型的问题时，才能举一反三。

> 一般情况下，拿到一个赛题之后，我们需要明确：

> 1）这是一个什么场景下的赛题；

> 2）这个赛题要解决什么问题。

> 深度思考这些问题，既能帮助我们更好地解决相关问题，更能让我们在后续遇到相关问题时，更好地完成迁移。

>

### 本赛题是一个经典的**NLP问题**

> 赛事目标为——【通过术语词典构建机器翻译模型】

> 在特定领域或行业中，由于机器翻译难以保证术语的一致性，导致翻译效果还不够理想。对于术语名词、人名地名等机器翻译不准确的结果，可以通过术语词典进行纠正，避免了混淆或歧义，最大限度提高翻译质量。

## 关于 NLP 和 大语言模型

自然语言处理（Natural Language Processing，NLP）是语言学与人工智能的分支，试图让计算机能够完成处理语言、理解语言和生成语言等任务。

大致可以将 NLP 任务 分为四类：

1.  **序列标注**：比如中文分词，词性标注，命名实体识别，语义角色标注等都可以归入这一类问题。这类任务的共同点是句子中每个单词要求模型根据上下文都要给出一个分类类别；

1.  **分类任务**：比如我们常见的文本分类，情感计算等都可以归入这一类。这类任务特点是不管文章有多长，总体给出一个分类类别即可；

1.  **句子关系判断**：比如问答推理，语义改写，自然语言推理等任务都是这个模式，它的特点是给定两个句子，模型判断出两个句子是否具备某种语义关系；

1.  **生成式任务**：比如机器翻译，文本摘要，写诗造句，看图说话等都属于这一类。它的特点是输入文本内容后，需要自主生成另外一段文字。

> Datawhale有一个开源项目 基于transformers的自然语言处理(NLP)入门，结合了形象生动的原理讲解和多个动手实践项目，帮助初学者快速入门深度学习时代的NLP。

> 学习这个开源项目需要有一定的python代码基础

> 大家如果学有余力，可以搭配这个项目的一些内容进行实践学习，帮助自己上分~

## [Task1知识点文档](https://datawhaler.feishu.cn/wiki/FVs2wAVN5iqHMqk5lW2ckfhAncb) 内还有更多精彩内容等着你~

## 更多QA详见[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/FPFzwD4FWiCpBFkVh0Ucqs1Wntd)~

#

> 本教程主要由  、  写作贡献，经过多位助教内测和建议修改，期待你的点赞和评论～