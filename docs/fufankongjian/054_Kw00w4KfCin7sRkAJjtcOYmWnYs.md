# DeepSeek R1 + Open-Webui多功能企业级部署实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/X55pdKQL8oz6D1xzzhcc40mjnIg](https://kq4b3vgg5b.feishu.cn/docx/X55pdKQL8oz6D1xzzhcc40mjnIg)

# 课程说明：

+   体验课内容节选自《2025大模型Agent智能体开发实战》完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》：

![](img/0fdbd0c096bff8a40fdce54156fcd804.png)

![](img/aad13260f5e8d2f0c16c078106315f01.png)

此外，公开课全套学习资料，已上传至网盘（ https://pan.baidu.com/s/1zdVHEEyJdgtE31DLvz41Iw?pwd=i8q7 ）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## **一、** **DeepSeek** **R1本地部署系列教程**

在[此前的公开课中，我们曾详细介绍过DeepSeek](https://www.bilibili.com/video/BV19kFoe6Ef7/) R1模型的本地部署+Ollama、vLLM等主流框架调 用流程：https://www.bilibili.com/video/BV19kFoe6Ef7/

![](img/c1e6bf579f7a3cdb23a5a1045b4285b1.png)

![](img/0790989c358fef554dd8d5c0f0734a8b.png)

以及，讲解了DeepSeek R1模型+An[ythingLLM的知](https://www.bilibili.com/video/BV12qF2ePEDa/)识库问答实战： https://www.bilibili.com/video/BV 12qF2ePEDa/

但对于DeepSeek R1，仍然有大量的实用功能没有实现，如代码解释器、联网功能、更加高效的知识库 检索功能、外部工具调用功能、以及连接本地MySQL数据库等功能。

本节公开课，我们就将结合DeepSeek R1与最热门的企业级大模型前端调用框架Open-WebUI，来 详细介绍如何完善丰富DeepSeek R1的本地部署调用功能。

![](img/c910c20c14046ec24214d19db3f55668.png)

+     **硬件要求**：本节公开课需调用DeepSeek R1 Distill 14B蒸馏模型进行实验，最少需要10G显存。

+     **课件代码领取**：公开课随课提供全部课件、代码、模型权重等各项内容。

+     **课程参考资料**：为了更好的辅助学习，随公开课附赠相关参考资料。

![](img/39b4c96af0fe333dca3ed762137b5f09.png)

![](img/e5d80ee69bbd60d7f590becbc35d4f54.png)

此外，公开课训练项目代码、数据、**及**模型权重等等，已上传至课件网盘，联系⬆**️**助教回复“DS网盘”，即可领取（目前咨询人数较多，助教老师在加急一一回复啦，小伙伴们发送后耐心等待一下哦🌹～）。

---

公开课目录：

DeepSeekR1+Open-Webui多功能企业级部署实战

一、DeepSeekR1本地部署系列教程

二、DeepSeek  R1部署基础环境搭建 1.Open-WebUI部署流程

2.ollama安装部署

3.1 【方案一】在线下载模型权重

3.2【方案二】手动下载GGUF权重并运行 三、Open-WebUI基础调用测试

1.首次启动流程   2.模型选择与切换

3.代码解释器功能实现

4.联网功能实现【国内不稳定】

四、DeepSeekR1+Open-WebUI进阶功能使用

1.RAG本地知识库问答

2.外部工具调用Functioncalling

3.连接本地MySQL数据库进行数据查询

## **二、** **DeepSeek** **R1部署基础环境搭建**

### 1.Open-WebUI部署流程

[首先需要安装Open-WebUI，官网地](https://github.com/open-webui/open-webui)址如下： https://github.com/open-webui/open-webui。

![](img/09a77d0e4e2067d8a65c71a7f2985500.png)

我们可以直接使用pip命令快速完成安装：

| pip install open-webui |
| --- |

![](img/6f4df6d624c1ab4183ff537fae7794af.png)

可以直接使用在GitHub项目主页上直接下载完整代码包，并上传至服务器解压缩运行：

![](img/26b9d8ec97876ea85a499098ec466f6c.png)

此外，也可以在课件网盘中领取完整代码包，并上传至服务器解压缩运行：

![](img/c18b57feb5d07565a930b486c1078f42.png)

![](img/286dd86118f1cc5629ee00a0aa73ea9b.png)

⬆️上图扫码即可领取～

安装完成后先不启动，等其他相关组件安装完后再启动。

### **2.** **ollama安装部署**

Open-WebUI原生支持使用Ollama调用本地模型进行推理， Ollama是一款大模型下载、管理、推   理、优化集一体的强大工具，可以快速调用各类离线部署的大模型。  Ollama官网：https://ollama.com/

![](img/c3102e7b0543cba9ce43ac8a1a0e80d8.png)

+   【安装方案一】Ollama在线安装

在Linux系统中，可以使用如下命令快速安装Ollama

| curl -fsSL https://ollama.com/install.sh | sh |
| --- |

![](img/28e0890a1a2d095bfd98b731b920ad0b.png)

但该下载流程会受限于国内网络环境，下载过程并不稳定。

+   【安装方案二】 Ollama离线安装

因此，在更为一般的情况下，推荐使用Ollama离线部署。我们可以[在Ollama Github主页查看目前 ](https://github.com/ollama/ollama/releases)Ollama支持的各操作系统安装包： https://github.com/ollama/ollama/releases

![](img/4d656e0dfcc680b6941cefc601930b5b.png)

若是Ubuntu操作系统，选择其中 ollama-linux-amd64.tgz下载和安装即可。 此外，安装包也可从网盘中下载：

![](img/21d21a8ea04d25e7fd4efd48eed578ff.png)

下载完成后，需要先上传至服务器：

![](img/07672c3bc880c5b15a939f7fe424eb6c.png)

然后使用如下命令进行解压缩

| mkdir ./ollama |
| --- |

解压缩后项目文件如图所示：

![](img/2fd24de769232f5ac22a5468e31f7268.png)

而在bin中，可以找到ollama命令的可执行文件。

![](img/695f04dc57523721ea69d46aaae15dde.png)

此时，我们可以使用如下方式使用ollama：

| cd ./bin |
| --- |

![](img/f5d64bb0be8a058c5ae33ab410d9e85d.png)

| 此处若显示没有可执行权限，可以使用如下命令为当前脚本添加可执行权限： |  |
| --- | --- |
|  | chmod +x ollama |

而为了使用命令方便，我们也可以将脚本文件写入环境变量中。我们可以在主目录（root）下找 到.bashrc文件：

![](img/bee7c877b66809448511f70d103ebefb.png)

然后在 .bashrc文件结尾写入ollama/bin文件路径：

![](img/9dd34586998c59e6871d9310b2e4d969.png)

保存并退出后，输入如下命令来使环境变量生效：

| source ~/.bashrc |
| --- |

然后在任意路径下输入如下命令，测试ollama环境变量是否生效

| ollama help |
| --- |

![](img/2ada1ea70eda28f051635b1da47a2182.png)

+   【可选】更换Ollama默认模型权重下载地址

接下来我们需要使用ollama来下载模型，但默认情况下， ollama会将模型下载到/root/.ollama文件 夹中，会占用系统盘空间，因此，若有需要，可以按照如下方法更换模型权重下载地址。

此外无论是在线还是离线安装的ollama，都可以按照如下方法更换模型权重下载地址。还是需要打 开 /root/.bashrc文件，写入如下代码：

| export OLLAMA_MODELS=/root/autodl-tmp/models |
| --- |

![](img/8bf435cdf36f9fff237911568514f10d.png)

这里的路径需要改写为自己的文件地址

保存并退出后，输入如下命令来使环境变量生效：

| source ~/.bashrc |
| --- |

测试环境变量是否生效

| echo $OLLAMA_MODELS |
| --- |

![](img/8a44eb3591643f614b2a3bb64f591c5e.png)

.    启动ollama

接下来即可启动ollama，为后续下载模型做准备：

| ollama start |
| --- |

![](img/418e32c2c9ea7441f37dfe43086fdbaf.png)

注意，在整个应用使用期间，需要持续开启Ollama。

**3.** **DeepSeek** R1与Embedding模型下载

接下来即可进入到模型下载环节。要顺利的使用Open-WebUI进行对话和知识库检索问答，需要下  载对话模型和Embedding模型。模型权重下载可以使用ollama pull命令进行在线下载（没有网络门槛， 且速度很快），当然也可以手动下载模型权重，然后上传到服务器上，再注册到Ollama中，就可以通过 Ollama调用模型了。以下两种模型权重下载方法任选其一执行即可。

#### **3.1** **【方案一】在线下载模型权重**

+    DeepSeek R1 Distill模型权重下载流程

目前DeepSeek R1及其蒸馏模型均支持使用ollama[进行调用，可以在模型主](https://ollama.com/library/deepseek-r1)页查看调用情况： https://oll ama.com/library/deepseek-r1

![](img/0ca14c8c07ced06f350da2ac94e0fd93.png)

其中各模型在Q4   K   M量化情况下运行所需显存情况如下：

![](img/5df5347239315501387566764e846c4e.png)

这里我们还是以1.5B模型为例进行演示，其他模型流程类似（只需要更换模型名称即可）。 .  deepseek-r1:1.5b模型权重下载流程

| ollama run deepseek-r1:1.5b |
| --- |

若只下载不调用，则可以使用ollama pull命令

使用该命令后系统就会自动下载模型权重，并自动完成注册。

![](img/78f3037d903e3ba450299dc693da044b.png)

等待模型下载完成后即可直接使用：

![](img/81f2ed87ac47ff677d5d707c33ee8403.png)

对话效果如下：

![](img/03ac258f43cb38a6b98de84b0f105f0a.png)

+   Embedding模型下载流程

nomi[c-embed-text是一个专门用于特征提取](https://ollama.com/library/nomic-embed-text)和句子相似度计算的Embedding模型。该模型在多个任务    上表现出色，特别是在分类、检索和聚类任务中，表现与OpenAI上一代Embedding模型性能相当。其核 心优势在于能够生成高质量的句子嵌入，这些嵌入在语义上非常接近，从而在相似度计算和分类任务中   表现优异。https://ollama.com/library/nomic-embed-text

![](img/3f395a6b8e2cc6a9c9c11db53ac926b2.png)

在线下载流程如下：

| ollama pull nomic-embed-text |
| --- |

![](img/3020c79e4ab611b4dd89d15fd42eba94.png)

也可以使用ollama list查看当前下载的模型列表：

| ollama list |
| --- |

![](img/08c6b3864dc039d10c6f5e852d1869b2.png)

此时应该能看到nomic embedding和DeepSeek R1 1.5B模型。 此外，若要使用多模态功能，则还需要安装Janus pro

| ollama pull erwan2/DeepSeek-Janus-Pro-7B |
| --- |

#### **3.2** 【方案二】手动下载GGUF权重并运行

除了使用ollama run命令外， ollama也支持调用手动下载的自定义模型，但需要是GGUF格式。模型权 重可以在huggingface或魔搭社区上下载。

+   DeepSeek R1 Distill模型权重下载流程

目[前De](https://modelscope.cn/search?search=DeepSeek-R1-Distill-Qwen-1.5B-GGUF)e[p](https://modelscope.cn/search?search=DeepSeek-R1-Distill-Qwen-1.5B-GGUF)Seek-R1-Distill-Qwen-1.5B模型已有各种不同量化版本的GGUF模型在魔搭社区中上线了： htt ps://modelscope.cn/search?search=DeepSeek-R1-Distill-Qwen-1.5B-GGUF

![](img/51049a04fd51bad745bbcc6263685378.png)

其中每个GGUF量化的权重都包含多个版本：

![](img/314a0205f8f236c6162ecdabf1de7708.png)

其中ollama调用的就是Q4_K_M量化版本。这里我们还是考虑下载Q4_K_M量化版本的GGUF权重进行调 用，具体执行流程如下。

+   安装魔搭社区

| pip install modelscope |
| --- |

下载GGUF格式权重

| mkdir DeepSeek-R1-1.5B-GGUF |
| --- |

![](img/119eda33759235a438c48eda61ff56bd.png)

也可从网盘中直接下载：

![](img/e63c3bff96b96d4fb68ef7f44315ec1a.png)

+   注册模型

然后需要创建一个file文件，用于进行ollama模型注册：

![](img/847db6244e28bc7617a6183c5e9f3e6a.png)

然后在File文件中写入自定义模型GGUF权重地址：

| FROM ./DeepSeek-R1-1.5B-GGUF./DeepSeek-R1-Distill-Qwen-1.5B-Q4 K  M.gguf |
| --- |

![](img/0c2ced19835bc413af37cc7d1c1960b1.png)

然后将该模型加入Ollama本地模型列表：

| ollama create DeepSeek-R1-1.5B -f ModelFile |
| --- |

查看模型是否注册成功

| ollama list |
| --- |

![](img/95bd8fab3d74360922d021000bd1e480.png)

接下来即可使用了：

| ollama run DeepSeek-R1-1.5B |
| --- |

![](img/2f5792c69b9afa100f944744f1afb736.png)

该流程也适用于其他不同型号的模型，下载对应的GGUF模型权重即可。

+   Embedding模型下载流程

类似的， nomic模型的GGUF格式也可以从魔搭社区中进[行下载，](https://modelscope.cn/models/nomic-ai/nomic-embed-text-v1.5-GGUF)然[后](https://modelscope.cn/models/nomic-ai/nomic-embed-text-v1.5-GGUF)注册使用。下载地址如下：  http s://modelscope.cn/models/nomic-ai/nomic-embed-text-v1.5-GGUF

![](img/3b6f625bdbfa1b4f4a19e74e5e10fdd0.png)

使用如下命令进行下载：

| mkdir nomic-embed-text-v1.5-GGUF |
| --- |

![](img/f907d57e8aba98a8ba08dcafb25dc88d.png)

也可以在网盘中进行下载并上传至服务器

![](img/b27421a0a2aaecef0174850713958ef9.png)

![](img/508ba139c5617557ecfd13ad7232572f.png)

然后即可查看到下载的模型权重

![](img/286c31775d04dbf5d6c239ad60d9a6bd.png)

然后编写Embedding模型的File文件 EmbeddingModelFile ：

| FROM ./nomic-embed-text-v1.5.f16.gguf |
| --- |

![](img/ab1ed81d0b754aa58b51bd54c91c1f42.png)

然后注册到ollama中：

| ollama create nomic-embed-text -f EmbeddingModelFile |
| --- |

![](img/18b1607b2292fb4e5d4a928221ae5abb.png)

然后即可通过ollama list查看是否注册成功：

![](img/da759c8995050bbccac842cc920d6b20.png)

## **三、** Open-WebUI基础调用测试

### **1.** **首次启动流程**

在准备好了Open-WebUI和一系列模型权重后，接下来我们尝试启动Open-WebUI，并借助本地模 型进行问答。

首先需要设置离线环境，避免Open-WebUI启动时自动进行模型下载：

| export HF_HUB_OFFLINE=1 |
| --- |

然后启动Open-WebUI

| open-webui serve |
| --- |

需要注意的是，如果启动的时候仍然报错显示无法下载模型，是Open-WebUI试图从huggingface上下载 embedding模型，之后我们会手动将其切换为本地运行的Embedding模型。

![](img/1e3e7fabc93a06885ab058c886db8313.png)

然后在本地浏览器输入地址:8080端口即可访问：

![](img/6ded85de4531e4593b5b90ac3a07a9b8.png)

若使用AutoDL，则需要使用SSH隧道工具进行地址代理

![](img/effd6001ab8d80da8073a2b53db0b954.png)

更多AutoDL相关操作详见公开课：《AutoDL快速入门与GPU租赁使用指南》 |https://www.bilibil i.com/video/BV1bxB7YYEST/

![](img/e456e1d61a69ba20e88f9ac294cb96a4.png)

然后首次使用前，需要创建管理员账号：

![](img/afdf83ad5bd41528a331b0ce04a6ce47.png)

然后点击登录即可。需要注意的是，此时Open-WebUI会自动检测后台是否启动了ollama服务，并列举 当前可用的模型。稍等片刻，即可进入到如下页面：

![](img/141c5f85dbc7527ee6ffd1de96ef45d8.png)

接下来即可进入到对话页面：

![](img/bb6fc4c61afab5bc53bda421cc21cae0.png)

若此时Ollama服务仍然开启，点击左上角即可选择Ollama已集成的模型：

![](img/21dc2aeb364a5f1388aac91bed527d40.png)

这里我们选择一个Chat模型即可开始对话：

![](img/9a8b367cdae4e39237fa833ab49a2657.png)

若登录对话页面时找不到模型，则需要先检查当前ollama服务是否开启、 ollama是否已经集成了 相关模型

至此，我们就完成了全部前期准备工作。

### **2.模型选择与切换**

Open WebUI支持自定义选择模型，点击左下方按钮：

![](img/63c9497b3a993c8c4372239124601fdd.png)

点击设置：

![](img/cb51df238fc95ad8e6d45f4fdca6fec6.png)

在外部连接中，可用选择是否启用在线模型。这里可以输入OpenAI API-KEY调用GPT模型，也可以输入 DeepSeek API-KEY，调用DeepSeek相关模型。此外，若是运行本地模型，则可在此处设置Ollama连   接。

![](img/c4c10e9f8ef8cbc240923e19cc7fcd68.png)

### **3.** **代码解释器功能实现**

Open Web-UI原生支持代码解释器功能，并且可以本地实时运行，并打印运行结果：

![](img/22486a7491b58cd98209de4d2eb33c70.png)

请帮我编写一段Python代码，用于绘制一个核密度分布图示例。

![](img/930253871bfc6acdc996946c94d938bd.png)

### **4.** **联网功能实现【国内不稳定】**

同样，我们可以在设置页面里选择是否开启联网功能。

![](img/36d53e575bb0f4b0223047fc7a0e96a3.png)

若是国内网络环境，推荐使用duckduckgo，是一款免费的搜索引擎，无需设置或API-KEY验证即可使 用：

![](img/ffb1d143114aa7429cbf464dd8c721f3.png)

此外[，若可以连接Google，也可以](https://www.bilibili.com/video/BV16u411c7Nd)使[用Google](https://www.bilibili.com/video/BV16u411c7Nd)的pse   （可编程搜索引擎），可以获得更加准确的搜索结  果。 Google pse搜索引擎注册河设置方法，详见我的23年公开课https://www.bilibili.com/video/BV16u 411c7Nd：

![](img/68c5d884cae3d9e33af7e58d3c4c7ab3.png)

Op[en WebUI的各项功能，如代码解释器、联网搜索、 RA](https://space.bilibili.com/385842994/upload/video)G问答、连接本地MySQL数据库等，在我 的23年大模型公开课合集中，都有更加底层的实现方法，感兴趣的同学可以在我的主页中看到对应 教程：https://space.bilibili.com/385842994/upload/video

![](img/a23679464fb25bd2ab49bf23cbc094a9.png)

需要按照如下设置duckduckgo并保存后：

![](img/93277698c59d7bee9fda13e2f1429271.png)

开启新的会话，并输入一个时效话题：

![](img/d6087df8fb441986e59f20f63c3ac63e.png)

能够看到，此时Open WebUI就开启了搜索和问答。由于duckduckgo是免费搜索引擎，有的时候并不稳 定。若想要获得稳定搜索结果，建议切换为在线模型，并使用网络工具+谷歌搜索来完成。

![](img/0141a65d43f92ecf9ecc3135fdbf0e18.png)

## **四、** **DeepSeek** R1+Open-WebUI进阶功能使用

### **1.** **RAG本地知识库问答**

+   基础问答流程

Open-WebUI拥有比AnythingLLM更加强大的RAG系统、以及更加灵活的功能选择。

![](img/4915dfe9a03e45749054e0f6c99eb90c.png)

然后填写语义向量模型：  nomic-embed-text并保存：

![](img/4bfb76fbc60417f7df085b158f0c69ac.png)

回到对话页面，可以这里可以直接拖拽添加文件，也可以点击对话框中+号添加文件。

![](img/10523def60eb52aa92b0946cabd77b0e.png)

这里我们上传了DeepSeek R1的微调教程并进行提问，问答效果如下：

![](img/5b498f102da7f0ffd0cd36d74f541330.png)

![](img/0c0fec906fe9b45ee910dd83d2ca278a.png)

不同于AnythingLLM不知道当前对话中包含文档， Open WebUI的RAG系统提示词会更加完备一些。同 时还会提供对应的检索结果：

![](img/e0ae3778244a8e404c4114d64ba7162d.png)

并且思考过程非常完备

![](img/6fff69f2c1b13344114d7a71b41552c6.png)

+   RAG流程优化

此外，我们也可以RAG基本原理对问答效果进行优化。

![](img/60637f11bb5e56c6f612510c7708ea71.png)

### **2.** **外部工具调用Function** **calling**

+   Function calling功能介绍

![](img/255c5417cbff777dbbc9deb3dafc0262.png)

+   Ollama tool use功能实现

需要注意的是， DeepSeek R1模型原生并不支持Fucntion calling，但借助Ollama，可以实现基础的 Function calling，并基于此完成Open-WebUI的外部工具调用工作。

![](img/2d90b432ad6cd514a6e161a650390a64.png)

[https://ollama.c](https://ollama.com/blog/tool-support)om/blog/tool-support

+   Open-WebUI工具调用功能实现

![](img/3828fba926adcbe1d7abd3721cffa6d0.png)

然后编写天气查询函数：

```
import requests 
import json
from fastapi import Request
from open_webui.models.users import Users
 
class Tools:
def    init    (self):
pass
 
async def get_weather(
self, loc: str,    request    : Request,    user    : dict,
         event_emitter    =None ) -&gt; str:
&quot;&quot;&quot;
获取指定城市的即时天气信息。

:param loc: 城市名称（如果是非英文城市名称，请先将其翻译为英文城市名称再输入）。 :param    request    : HTTP 请求对象（来自  FastAPI）。
:param    user    : 用户信息（可以用于个性化或记录请求）。
:param    event_emitter    : 事件发射器，用于将实时状态更新发送到前端。 :return: 格式化后的天气数据，作为字符串返回。
说明：
如果输入的是非英文城市名称，请先将其翻译为对应的英文城市名称再输入。 比如，输入“ 中国，北京 ”，需要转为  &quot;Beijing&quot;。
&quot;&quot;&quot;
 
# Step 1. 通知用户正在获取天气数据
await    event_emitter    ( {
&quot;type&quot;: &quot;status&quot;,
&quot;data&quot;: {&quot;description&quot;: f&quot;正在获取  {loc} 的天气数据 ...&quot;, &quot;done&quot;:
False},
} )
 
try:
# Step 2. 构建请求的  URL 和查询参数
url = &quot;https://api.openweathermap.org/data/2.5/weather&quot; params = {
&quot;q&quot;: loc,
&quot;appid&quot;: &quot;YOUR_API_KEY&quot;,  # 你的  API 密钥
&quot;units&quot;: &quot;metric&quot;,  # 使用摄氏度
&quot;lang&quot;: &quot;zh_cn&quot;,  # 输出简体中文 }
 
# Step 3. 向  OpenWeather API 发送  GET 请求
response = requests.get(url, params=params)
 
# Step 4. 解析响应数据
data = response.json()
 
# Step 5. 提取并格式化天气描述
weather_data = data.get(&quot;weather&quot;, []) if weather_data:
main_weather = weather_data[0].get(&quot;main&quot;, &quot;&quot;)
description = weather_data[0].get(&quot;description&quot;, &quot;&quot;)
weather_info = f&quot;当前天气： {main_weather} - {description}&quot; else:

weather_info = &quot;天气信息不可用。 &quot;
# Step 6. 通知用户天气数据已经成功获取
await    event_emitter    ( {
&quot;type&quot;: &quot;status&quot;,
&quot;data&quot;: {&quot;description&quot;: f&quot;成功获取  {loc} 的天气数据&quot;, &quot;done&quot;:
True},
} )
 
# Step 7. 返回格式化后的天气信息并发送到前端
await    event_emitter    ( {
&quot;type&quot;: &quot;message&quot;,
&quot;data&quot;: {&quot;content&quot;: f&quot;{loc} 的天气信息 : {weather_info}&quot;},
} )
 
# 返回格式化后的天气数据作为字符串
return f&quot;{loc} 的天气信息: {weather_info}&quot;
 
except Exception as e:
# Step 8. 如果发生错误，通知用户
await    event_emitter    ( {
&quot;type&quot;: &quot;status&quot;,
&quot;data&quot;: {&quot;description&quot;: f&quot;发生错误 : {str(e)}&quot;, &quot;done&quot;: True},
} )
# 返回错误信息
return f&quot;获取天气数据时发生错误： {str(e)}&quot;
```

并将函数进行写入：

![](img/b2d26be80fb400578cefaa22a104b53d.png)

完成后即可看到新的工具：

![](img/0d5f6372133a17c871ff23b8dd574cb9.png)

在对话时可以开启天气查询函数：

![](img/97d2c9ea22bdaa63c880a797a6a2a583.png)

比如询问北京天气，可以获得如下回答：

![](img/023ad66feaea4c3c8c2cc65710b52b06.png)

与实际天气相符

![](img/960020acc3ca0ac5c9561328ecc7076b.png)

### 3.连接本地MySQL数据库进行数据查询

此外，我们还可以借助工具，编写SQL围绕本地数据库中数据进行查询。

+   创建MySQL数据库

| apt install mysql-server |
| --- |

![](img/fa4077d44b12243e5b2b76aaa49021fb.png)

然后启动mysql，并设置初始密码：

| mysqld &amp; mysql |
| --- |

进入到SQL命令行后，输入如下命令：

| ALTER USER &#x27;root&#x27;@&#x27;localhost&#x27; IDENTIFIED BY &#x27;123&#x27;; |
| --- |

此处密码可根据实际需求设置。

![](img/fff487483de1981473eb8e2270d335b7.png)

然后输入 exit; 即可退出。

然后再次进入MySQL，并根据要求输入密码：

| mysql -u root -p |
| --- |

然后创建一个数据库：

| CREATE DATABASE school; USE school; |
| --- |

然后创建一个虚拟表格，里面包含了10位同学各自3门课程的分数：

| CREATE TABLE students_scores ( |
| --- |

| INSERT INTO students_scores (name, course1, course2, course3) VALUES |
| --- |

![](img/e3b3ab983b74828f30f8330962f03fa2.png)

然后即可查看数据集基本情况：

| SELECT * FROM students_scores; |
| --- |

![](img/9f54956b8f52bd1ee205762a23e93c18.png)

此外，还需要刷新身份验证，使得其他库（如pymysql）可以通过密码验证登录：

| ALTER USER &#x27;root&#x27;@&#x27;localhost&#x27; IDENTIFIED WITH mysql_native_password BY &#x27;123&#x27;; |
| --- |

![](img/66ee3362679880101401d9f9e451c178.png)

+   编写SQL查询工具

![](img/f012ab037bba45a98e748ad48929774a.png)

写入如下内容：

import pymysql import json

from fastapi import Request

from open_webui.models.users import Users

class Tools:

def    init    (self):

pass

async def sql_inter(

self, sql_query: str,    request    : Request,    user    : dict,

event_emitter    =None ) -> str:

"""

用于获取school数据库中各张表的有关相关信息。

:param sql_query: 字符串形式的SQL查询语句，用于执行对MySQL数据库中的查询。 :param    request    : HTTP 请求对象（来自  FastAPI）。

:param    user    : 用户信息（可以用于个性化或记录请求）。

:param    event_emitter    : 事件发射器，用于将实时状态更新发送到前端。 :return: SQL 查询结果的  JSON 字符串。

"""

# Step 1. 通知用户正在执行数据库查询

await    event_emitter    ( {

"type": "status",

"data": {"description": "正在查询数据库 ...", "done": False},

}

)

try:

# Step 2. 连接  MySQL 数据库

connection = pymysql.connect(

host='localhost',  # 数据库地址 user='root',  # 数据库用户名

passwd='123',  # 数据库密码

db='school',  # 数据库名

charset='utf8'  # 字符集选择utf8

)

# Step 3. 执行  SQL 查询

with connection.cursor() as cursor: cursor.execute(sql_query)

results = cursor.fetchall()

# Step 4. 格式化查询结果

results_json = json.dumps(results, ensure_ascii=False)

# Step 5. 通知用户查询完成

await    event_emitter    ( {

"type": "status",

"data": {"description": "数据库查询完成", "done": True},

} )

# Step 6. 返回查询结果

return f"查询结果 : {results_json}"

except Exception as e:

# Step 7. 如果发生错误，通知用户

await    event_emitter    ( {

"type": "status",

"data": {"description": f"发生错误 : {str(e)}", "done": True},

} )

# 返回错误信息

return f"获取数据库查询数据时发生错误： {str(e)}"

finally:

# Step 8. 关闭数据库连接

if connection:

connection.close()

![](img/09a9978f67ee794857b03ab0a31d4565.png)

然后在对话中打开数据库查询工具：

![](img/076861c11b21eb1efd14224cd2a4c39d.png)

然后即可进行对话和数据库查询：

![](img/b82fa410eecdc4c9a27285815aca62f7.png)

![](img/954aa2cb8c4ec0937c2baa7eff7f0d1a.png)

同时，数据库查询功能还可以与代码解释器功能结合进行使用：

![](img/ab5654f39f7be8613c9f91fd12776f7e.png)

并且，也可以结合知识库内容进行结合使用。

![](img/c7101ac69ed461402340d5663ca03936.png)

并且在右侧可以看到详细的对话流程。

![](img/4063d42b5fea7bc3e7b128f75b6d77c0.png)

![](img/0b5b63eca9e1c87126570e3d0df3f2a8.png)

更多关于大模型微调技术介绍，欢迎报名由我主讲的《2025大模型Agent智能体开发实战》（2月 DeepSeek强化班） https://whakv.xetslk.com/s/1tKbjV 进行更深度系统的学习哦~

![](img/46aa8aff319ec84ec16ed5e96cd249ff.png)

《2025大模型Agent智能体开发实战》2025年新春班上新&新年双重特惠，详细信息扫码添加助教，回

**复“大模型”** ，即可领取课程大纲&查看课程详情

![](img/3db2799944bce1e263dabdaf5e328829.png)