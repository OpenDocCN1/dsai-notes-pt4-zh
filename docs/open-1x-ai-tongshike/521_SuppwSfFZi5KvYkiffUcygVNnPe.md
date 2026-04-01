# 从零入门CPU部署大模型&应用开发

> 来源：[https://datawhaler.feishu.cn/docx/H809dvGGnomJs5xtLGwcoiHQnrh](https://datawhaler.feishu.cn/docx/H809dvGGnomJs5xtLGwcoiHQnrh)

> 从零入门CPU部**署大模型&应用开发 是** Datawhale 2024 年 AI 夏令营第二期的学习活动（“大模型应用开发”方向），基于天池平台“Intel LLM Hackathon 天池挑战赛”开展的实践学习——

+   适合想 **入门并实践 在CPU上部署大模型、** 大模型应用开发、构建大模型RAG应用 的学习者参与

+   **只要会用电脑**，就能跟着**本文档内容**，在** 30分钟内，****完成这些开发流程的速通体验！**

>

> 我们一起学习进阶，学习实践、写笔记打卡

> **创作属于自己的RAG应用**，冲刺**夏令营优秀学员**，获得**荣誉证****书！**

# 半小时速通体验CPU开发开源大模型应用！

> 你是否觉得大模型只能运行在 GPU 上，没有 GPU 的你注定与更高阶的大模型研发有缘无分？

> Intel  IPEX-LLM 框架，将为你带来 在 CPU 上高性能部署大模型的神奇体验！

>

> 英特尔与魔搭社区基于此，开展了Intel LLM Hackathon 天池挑战赛，接下来，跟随教程的脚步！一起体验在 CPU 上高性能部署大语言的模型的全流程吧！

![](img/dbabcd299c1223875bd2517b7d96161f.png)

## 第零步：报名赛事（点击即可跳转）

> 赛事链接：https://tianchi.aliyun.com/competition/entrance/532239

![](img/c9be21bb85353b56607896c9ef37a281.png)

![](img/340b3d2b58921c47340e39c059c33b4d.png)

![](img/1ed8f6d2323343e77a376585561cf5bf.png)

## 第一步：下载如下两个代码文件！

> 下载下方两个文件即可

+   baseline1：在 CPU 上部署并进行大模型推理

![](img/c807af6c39e3f54bd39d0e80322e857a.png)

[初赛baseline1-使用ipex-llm推理大语言模型.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/A8cqbem6OoYEt0x1IoycF4xvnOh)

+   baseline2：在 CPU 上部署大模型并运行检索增强生成应用

![](img/6cf5217d8f276f3c40b6c40ed2c87fda.png)

[初赛baseline2-使用ipex-llm运行RAG应用.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OhJgbEPP7oTE4vxdvgjcvcKRnDc)

## 第二步：启动魔搭 CPU 环境（点击即可跳转）

方法一：

> 链接：https://modelscope.cn/my/mynotebook/preset

> 选择第二个镜像，启动！

> 认准 ubuntu22.04-py310-torch2.1.2-tf2.14.0-1.14.0！

![](img/715183abe8bfb5488c7127e951c81388.png)

选择第二个并运行

![](img/fa736d72b966d02cacf0b0c2b0e58663.png)

进入 notebook

![](img/de33ac7ce75a109c0e77039603ac8b56.png)

+   新用户需要先绑定阿里云账号

![](img/f791c4db72d205809f87b406e07f1d1b.png)

## 第三步： 半小时，在 CPU 上部署&构建大模型应用！

### 一、使用 IPEX-LLM 推理大语言模型（大概需要15分钟）

1.  上传文件！直接拖入！

![](img/87f6876e7c86cfc780d0003f2c83edbc.png)

1.  安装环境**（大概需要 10**** 分钟****）**

**方法一：**

+   双击打开 初赛baseline1文件，

![](img/f5c82dddf8394ebd6a03f876bf12b542.png)

+   运行第一段代码

![](img/7652ca19113603c255f09e1fbcd0ce47.png)

+   运行环境安装脚本**（大概需要 10**** 分钟****）**

> 新建终端，复制bash install.sh命令，粘贴后敲击“回车↩︎”运行脚本

![](img/f60ce30a0b1bcc5e4bbd5eea31d45fb3.png)

![](img/b67b464faa361641736251b9a37e4a92.png)

![](img/6d29be466446ee377ee03be36bdb6b1c.png)

1.  切换环境

> 回到初赛baseline1文件，切换 ipex 环境

![](img/f1c0cdec59974d7a293fc6fa95d93359.png)

![](img/5b6e1f15ce2705ff29bbf7e2630c61c2.png)

![](img/7e4ba6980f7622dcb636476ea0804c18.png)

**方法二：**

在第二步选择这个镜像：

![](img/d751883920c5120f6778bcf46d73d59c.png)

然后在notebook中输入下面的命令来安装ipex-llm，就可以接着做4.下载模型，量化模型，创建文件 之后的步骤了

```
!pip install ipex-llm==2.1.0b20240711
```

> 到这里！我们已经**成功了 50%**！！！我们马上就要开始和大模型见面了！！！

1.  下载模型，量化模型，创建文件**（大概需要 5 分钟）**

> 一键运行初赛baseline1文件中的剩余代码

![](img/c7ac3431a9a32839d7262f310fa50c89.png)

![](img/d7684014d0532e9c1b37e92428f05c33.png)

1.  **在 CPU 上运行大模**型！进行第一次对话！（大概需要 30 秒）

+   运行 run.py 文件**（大概需要 30 秒）**

> 切换回命令行终端，依次粘贴下列代码

```
cd **/**mnt**/**workspace
conda activate ipex
python3 run.py
```

![](img/78b1342055c6ee2a64b1ec7767f7ae9e.png)

![](img/1e73d7ffd8da2fbbed4bdf7f4eb1b00b.png)

1.  **在 CPU 上和大模型**进行流式对话！（大概需要 30 秒）

+   运行 run_stream.py 文件（大概需要 30 秒）

> 这次在命令行中使用 python3 run_stream.py 命令

![](img/6f268a58261fe9767c32128dd800a7f4.png)

![](img/f3f5d13a18c4c48d461727bd02410288.png)

![](img/7762e551fc3cceeadf85ef8d1037a089.png)

> 至此！！！！我们已经完成了**在 CPU 上部署大模型****并****进行推理**！！！！

> 甚至还体验了流式输出！让他逐步输出内容！体验更丝滑！！！

### 二、使用 IPEX-LLM 和 Gradio/Streamlit 构建大模型应用（大概需要10分钟）

> 更进一步，跟大模型进行更高自由度的对话！（大概需要5分钟）

1.  安装Gradio（大概需要 2 分钟）

1.  在命令行中运行命令：pip install gradio

![](img/176e3c13425dfd64279777e2a2d33647.png)

1.  修改run_gradio_stream.py文件

> 修改run_gradio_stream.py文件最后一行，注意修改为自己对应的数字

![](img/885e8b401e1c1f6cafd68a07450b0e78.png)

![](img/8ef3d979467e299c15c26992353e97b0.png)

1.  运行run_gradio_stream.py文件（此处大概需要 2 分钟）

> 在命令行中运行命令：python3 run_gradio_stream.py

> 点击 “local URL” 后的链接即可开始聊天

![](img/b1925f193f55bc92c1b0a2bccb351b72.png)

![](img/f0452cbf6aa7cf4a0ad6de0038fa5a1b.png)

![](img/39deab9564023767a41eec268d1fe4ef.png)

> 至此！！！！我们已经在 CPU 上完成了部署大模型！以及构建简单 Gradio 前后端应用！！！！！

> 不喜欢 Gradio 我们还可以试试 streamlit！（大概需要 5 分钟）

1.  安装 streamlit

> 在命令行中运行命令：pip install streamlit

![](img/4b6aad68fb61d4779793d6b37076a6d7.png)

1.  运行 run_streamlit_stream.py文件

> 在命令行中运行命令：streamlit run run_streamlit_stream.py

![](img/cdae095e373ebf42921ec1e28c4d5699.png)

![](img/84003946624152815d8c482d13c10fb8.png)

![](img/f87b6e852c3e7e6dfd85db5f0dd6e9de.png)

> 恭喜你！！！你完整体验了使用 IPEX-LLM 在 CPU推理大语言模型！！！并且构建 Gradio 和 Streamlit 应用的全过程！！！！如果想要仔细了解其中的细节，可以再仔细研究一下 初赛baseline1文件的内容！

>

> 更多知识点还可以在 [Task1：了解模型获取和IPEX-LLM部署工具](https://datawhaler.feishu.cn/wiki/Gka7wVHxziP4fKkgDwXcdiZxnah) 查看~

### 三、使用 IPEX-LLM 运行 RAG 应用（大概需要10分钟）

1.  创建新文件夹data

![](img/bb1dad84a7b360d06a6bf6e1ebb328c4.png)

![](img/f47ce1c1173a85eeff7690f20779b55f.png)

1.  下载后上传llamatiny.pdf 文件

> 注意要放至/data目录下

![](img/c4381aa2bc5db52c8a010f92bdfcddf7.png)

[llamatiny.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UzYwbb2lMotctzxWwrWcjCuXnEb)

![](img/60c169684f56a452a2d9d7d53442991f.png)

![](img/8cdda1d75a9f6ee2de5b99e615145f27.png)

1.  环境配置（大概需要5分钟）

> 在命令行中运行如下命令：

> pip install PyMuPDF llama-index-vector-stores-chroma llama-index-readers-file llama-index-embeddings-huggingface llama-index

![](img/3821bef1b78ddd0f61196e43b728c021.png)

1.  下载 Embedding 模型，创建 run_rag.py 文件（大概需要2分钟）

> 打开 初赛baseline2文件，一键运行所有代码段

![](img/8de3856e7d336f3ccdb9c2887122ac8e.png)

![](img/606ad73cd69744330790f5c9ef2ea505.png)

![](img/e60e052fda8dfbc9a8dc2b3e9d8d7254.png)

1.  运行 run_rag.py 文件（大致需要2分钟）

> 在命令行中运行命令：python3 run_rag.py

> 此时已可以一键运行准备好的 RAG 应用, 此时请保证你已经拥有 baseline1 中转换好的 int4 模型，以及上述提到的 bge 模型 ，还有之前提到的 llamatiny.pdf 文件。

![](img/fa5504ebd0d507c40a6858e71e7bdad8.png)

![](img/f9210d38270c96280a40b84b8f0f644c.png)

> **恭喜你！！！你在**半小时内，完整体验了使用 IPEX-LLM 在 CPU推理大语言模型！！！构建Gradio/Streamlit应用，甚至RAG应用！！！！

> 记得截个图，打卡的时候会用到哦~

> 剩下的时间，你可以自由探索和学习更多细节的知识和代码了！！！

## 第四步：结束运行，关闭实例

> 在结束开发后，大家记得及时保存环境现场。

> 魔搭Notebook会在没有后续操作的1小时后自动关闭，仅保存 notebook 文件。

> 在确定所有东西已经备份后，最好主动关闭实例~

>

> **📢注意：**如**果你有其他任何****需**要保存的文件，请提前下载保存到本地。

![](img/fe8c4e7d6702c30ff37de870b652829f.png)

+   如果没有【关闭实例】按钮，可以回到魔搭Notebook页(点击即可跳转)进行实例关闭

> 链接：https://modelscope.cn/my/mynotebook/preset

![](img/2ec78d3e769024c60e8c57f57b116b1a.png)

# 🎉恭喜你跑完了全流程！

## 快来给自己点赞！

## 然后填写表单，寻找志同道合的小伙伴吧~

> 请大家提交完作品后填写下面的表单哦~

> 组队表： https://docs.qq.com/sheet/DZlJobUp6b2lFdVdV

>

> 也可以把你的应用效果截图转发到群里呀~

# 📅 记住下面的学习规划表！

## Task1还没有结束~

> 点击 [Task1：了解模型获取和IPEX-LLM部署工具](https://datawhaler.feishu.cn/wiki/Gka7wVHxziP4fKkgDwXcdiZxnah)查看更多吧~

> **教程贡献者说：**

> 大模型应用开发，其实就是找到自己的idea方向，把自己的想法，通过技术的方式，实现为可持续被调用的工具，自用或被大家使用，提升自己的效率或传递自己的一种思想和哲学~

> 跟以往的应用开发不同，大模型的出现让大家可以通过较为简单的代码就可以调用**AI能力，让AI很高效地**完成想要的交互，实现效果。目前大模型的核心能力在于，可以作为流水线上的一个熟练工，稳定持续完成一些指定的任务（如，让大模型稳定根据输入的文本生成题目，让其扮演某个动漫人物）

>

> 大家**可能都经历过，一个让我们激动不已的创**意，如果三天没有开始思考落地，甚至一天、半小时，我们就可能再也不会去想办法实现它。我们参加类似这次学习活动的应用赛，可以在较短时间的压力督促下，快速推进创意落地，通过团队的力量，真正实现从零到一，甚至不断持续优化，将玩具变为工具、甚至产品！

>

> 这次学习跟一般的大模型应用开发相比，复杂度会偏高，涉及到选择并部署开源大模型，以及进行相关应用开发。

> 这个项目融合了多项技术，但其实并不难，Datawhale的开源贡献者们经过了长期的打磨，已经推出了许多大模型技术栈全链的开源项目，本次学习活动教程基于他们的基础，针对确定的场景进行了整合呈现。

> 在现在这样拥有大模型的年代，学习东西都变得像聊天一样快乐，我非常推荐大家有不懂的就多查官方文档，利用文档和代码多和大模型互动，你一定要相信自己是完全能学会的。

> 接下来我们会基于这个赛题进行更多的知识点学习，会涉及到：

1.  在CPU环境下进行模型下载与部署

1.  **intel部署及优化工具**

1.  RAG理论：llamaindex

1.  Gradio/Streamlit 开发应用Demo入门

>

> 请相信，你很优秀，**我坚信你可以和队友一起协作，通过**参与学习活动的安排，完成自己的应用开发~

> 最后，想送给大家八个字：不懂就问，不会就学。

>

> 祝大家学习开心，学有所成~ 项目顺利落地、甚至蒸蒸日上！

>

> **更多详情见如下学习规划表：**

| ## **从零入门CP****U部署大模型**&amp;应用开发(大模型应用开发方向)-学习规划表（14 天） |  |  |  |  |
| --- | --- | --- | --- | --- |
| ### Intel LLM Hackathon 天池挑战赛 |  |  |  |  |
| **Task** | **Task 描述** | **相关知识点** | **学习文件&amp;链接**  （点击即可跳转查看） | **学习安排** |
| **Task 1** | **目标**：跑通baseline，体验在CPU上部署大模型及应用开发 ，并阅读代码学习其中思路 | 1.  下载模型、魔搭是啥 | 1.  [从零入门CPU部署大模型&amp;应用开发](https://datawhaler.feishu.cn/wiki/SuppwSfFZi5KvYkiffUcygVNnPe)**(速通手册)** | [**开营直播**](https://tongyi.aliyun.com/efficiency/doc/transcripts/4l6xqabx4v8p9m2y?sl=1&amp;source=1#) |
| **Task 2** | **目标**：思考idea、参加 头脑风暴、组队 |  | 1.  [Task2：头脑风暴会](https://datawhaler.feishu.cn/wiki/PUyiw4imoiGy55kM7w5cV3isnqe) | [**腾讯会议交流**](https://meeting.tencent.com/v2/cloud-record/share?id=5d65fe31-52a2-4ffa-80c7-63bd908a6f9e&amp;from=3&amp;is-single=false&amp;record_type=2)  （点击链接即可跳转回放） |
| **Task 3** | **目标**：通过精读baseline代码，理解具体实现方案，入门RAG理论、Gradio/Streamlit开发 | 1.  RAG理论：llamaindex | 1.  [Task3知识点文档](https://datawhaler.feishu.cn/wiki/R78xwbV1DiqZpwkO7M5cHtCmnof) | **直播+****腾讯会议交流** |
| **Task 4** | **目标**：应用开发&amp;技术交流**(初级)** |  | 1.  [Task4：技术交流会](https://datawhaler.feishu.cn/wiki/Usjrw8lp7ilAXkkyLysc3YYMnDg) | **直播+****腾讯会议交流** |
| **Task 5** | **目标**：应用开发&amp;技术交流**(进阶)** |  | 1.  [Task5：进阶技术交流会](https://datawhaler.feishu.cn/wiki/Ljarw5Ci6i4CPtkzTDmcsqJrnRg) | demo互评会  2024/7/26 20:00 |
|  |  |  |  |  |

### 祝大家都能做出属于自己的应用，学习愉快~ 不要害怕在群里提问和交流哟！

> 不过建议先查看 [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/BDQZwdzGviUQzrkRNUncjfA2nAd)** **是否早已解决并收录你的问题~ 然后再在群里仔细描述清楚问题进行提问哟~

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

### 本次学习活动的赛题“Inte**l LLM H**ackathon 天池挑战赛”目标是创作RAG应用

> **学习目标：【使用CPU**部署和开发开源大模型RAG应用】，需要给出详细的项目方案

> (初赛)基于**Intel 工具链**，**在魔搭平台上使用免费的**CPU Notebook，下载开源大模型(有指定list)，进行部署和RAG应用开发，需要给出详细的项目方案

## 关于大模型应用开发、开源大模型、RAG应用

#### 关于大模型应用开发

> 大模型应用开发，即基于大模型能力，去做应用的开发，一般情况下使用其他大模型厂商提供的API服务进行开发较为方便，我们这次学习活动就是要做出属于自己的大模型应用。

> 大模型应用开发一般主要分为这几个步骤：idea创意设计，拆解需求 -> 设计功能 -> 进行开发（实现UI、设计核心AI功能Prompt、前后端编程、项目集成等）-> 部署和上线应用

> Datawhale有一个开源项目 动手学大模型应用开发(llm-universe)，是一个面向小白开发者的大模型应用开发教程，旨在结合个人知识库助手项目，通过一个课程完成大模型开发的重点入门。

> 需注意：此项目学习需要一定技术能力

> **学有余力还可以**参考其中的内容更好地开发大模型应用哟~

>

#### 关于开源大模型

> 开源大模型是

> Datawhale有一个开源项目 开源大模型食用指南(Self-LLM)，围绕开源大模型，提供包括环境配置、本地部署、高效微调等技能在内的全流程指导，简化开源大模型的部署、使用和应用流程，让更多的普通学生、研究者更好地使用开源大模型，帮助开源、自由的大模型更快融入到普通学习者的生活中。

> 需注意：此项目学习需要一定技术能力

> **学有余力还可以参考其**中的内容更好地理解开源大模型的部署哟~

>

#### 关于RAG

> Datawhale有一个开源项目 大模型白盒子构建指南(Tiny-Universe)，从原理出发、以“白盒”为导向、围绕大模型全链路的“手搓”大模型指南，旨在帮助有传统深度学习基础的读者从底层原理出发，“纯手搓”搭建一个清晰、可用的大模型系统，包括大模型本身、RAG 框架、Agent 系统及大模型评估体系。

> 需注意：此项目学习需要**较高**技术能力

> **学有余力还可以参考其**中的内容更好地理解开源大模型的部署哟~

>

> ————————————

> 大家在本次学习过程中，可以一边将上面推荐的材料作为字典，遇到问题时通过这些材料、大模型、搜索引擎，去翻阅查看，做到理论和实践的结合，真正理解这些理论知识的价值、以及究竟要如何在真实的问题场景中使用。

>

> （如果大家想仔细研读的话，可以在学习过程中，依照其中的内容进行实践；

> 还可以多多在此处留言，也许夏令营后就会展开相关内容的组队学习哟~）

## 还可以试试以下玩法~

为了更好的掌握 IPEX-LLM 及大语言模型推理的使用方法, 在此留有几项思考题与作业,欢迎学有余力的同学更进一步:

1.  尝试更改 os.environ["OMP_NUM_THREADS"] = "8" 中的线程数值， 或者直接注释该行， 查看修改后对推理速度的影响。

1.  尝试加载不同参数量的千问模型如 0.5B ,7B , 或任意其他常见大模型, 如 llama3 进行推理.

1.  尝试更换不同上下文进行推理, 思考当前硬件下的最佳可支持上下文长度.

1.  尝试更换不同的 PDF 文件,进行提问查看返回结果.

1.  尝试将 RAG 对话脚本接入 gradio 前端, 或是 streamlit 前端实现更方便的对话.

1.  尝试调整 run_rag.py 中的 max_new_tokens context_window text_parser = SentenceSplitter(chunk_size=384) 相关数值参数，查看对结果有何影响

#

> 本教程主要由  、 、  写作贡献，经过多位助教内测和建议修改，期待你的点赞和评论～