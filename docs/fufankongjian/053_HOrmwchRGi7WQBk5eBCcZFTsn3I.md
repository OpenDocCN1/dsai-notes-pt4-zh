# DeepSeek R1高性能部署实战

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/WrrBdCn2JouchKx3RF4c0EzPnGg](https://kq4b3vgg5b.feishu.cn/docx/WrrBdCn2JouchKx3RF4c0EzPnGg)

# 课程说明：

+   体验课内容节选自《2025大模型Agent智能体开发实战》完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》：

![](img/238e61fd78aee3ec00925fec3b68385d.png)

![](img/b50056a614cc6c349c9bb9f53dee6110.png)

此外，公开课全套学习资料，已上传至网盘（ https://pan.baidu.com/s/1x_IkUBh6B5oNHx62Epwgrw?pwd=g2g3 ）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

# KTransformer高性能部署DeepSeek R1满血版模型

## 一、DeepSeek R1部署方案综述

### 1. DeepSeek R1高性能部署方案介绍

伴随着DeepSeek R1模型使用需求不断深化，如何才能部署更高性能的满血版DeepSeek R1模型，就成了很多应用场景下的当务之急。受限于DeepSeek R1 671B（6710亿参数）的模型规模，通常情况下部署DeepSeek R1满血版模型需要1200G左右显存（考虑百人内并发情况），需要双节点8卡A100服务器才能运行（总成本约在260万-320万左右），而哪怕是INT 4半精度下，也需要至少490G显存，需要单节点8卡A100服务器才能运行。

![](img/c03d8dde156cfc43ac40602ee3e81b13.png)

> DeepSeek R1和DeepSeek V3都是默认BF8精度，是一种低精度的浮点数格式。BF8的全称是"Brain Floating Point"，由Google提出，主要用于大规模计算任务。与常见的16位浮点数（FP16）不同，BF8采用了8位尾数和8位指数的结构，能够在保证精度的同时减少计算和内存开销。BF8的设计目标是减少计算量并保持数值稳定性，特别是在机器学习模型训练中，能在加速硬件上提供比FP32更好的性能。

在此情况下，如何以更少的成本获得尽可能好的模型性能——也就是如果进行DeepSeek R1的高性能部署，就成了重中之重。基本来说，目前的解决方案有以下三种：

1.  采用“强推理、若训练”的硬件配置：如选择国产芯片、或者采购DeepSeek一体机、甚至是选择MacMini集群等，都是不错的选择。这些硬件模型训练性能较弱，但推理能力强悍，对于一些不需要进行模型训练和微调、只需要推理（也就是对话）的场景来说，是个非常不错的选择。例如45万左右成本，就能购买能运行DeepSeek R1满血版模型的Mac Mini集群，相比购买英伟达显卡，能够节省很大一部分成本。但劣势在于Mac M系列芯片并不适合进行模型训练和微调。

![](img/417193a44eed35fda22c64dfa6d60ae7.png)

1.  采用DeepSeek R1 Distill蒸馏模型：DeepSeek R蒸馏模型组同样推理性能不俗，且蒸馏模型尺寸在1.5B到70B之间，可以适配于任何硬件环境和各类不同的使用需求。

![](img/da34787afc4f1a9b78b5182b36cb503c.png)

1.  其中各蒸馏模型、各量化版本、各不同使用场景（如模型推理、模型高效微调和全量微调）下模型所需最低配置如下：

![](img/61c9ecbe28e66154a2aff983af2afda2.png)

1.  **采用KTransformers（Quick Transformers）技术：**这是一项由清华大学团队提出的，可以在模型运行过程中灵活的将专家模型加载到CPU上、同时将MLA/KVCache卸载到GPU上，从而深度挖掘硬件性能，实现更低的显存运行更大尺寸的模型。

![](img/f1a0de1669d1e270085e057d10bb59de.png)

1.  该技术目前的实践效果，可以实现480G内存+13G显存（长尺寸输出或多并发时达到20G显存），即可运行DeepSeek R1 Q_4_K_M量化版模型（类似INT4量化），并且响应速度能够达到15token/s。大幅降低了传统DeepSeek R1 INT4模型的运行门槛。这也是目前最具价值的DeepSeek R1高性能部署方案。

+   GitHub主页：https://github.com/kvcache-ai/ktransformers

![](img/b78f96df52e62426e7243d7e2e5f9bf7.png)

1.  传统情况下，8卡 A100 GPU服务器才能运行DeepSeek R1 INT4模型，成本接近200万。而480G内存+单卡4090服务器，总成本不到5万。

1.  **采用Unsloth动态量化技术：**此外，还可以考虑采用Unsloth的动态量化方案。不同于KT将不同的专家加载到CPU上，通过内存分担显存的方法保证R1 Q4KM模型运行，作为资深量化专家团队，Unsloth团队的技术方案则是在确保模型性能的基础上，更深度的进行模型量化（最多量化到1.58Bit），并且执行不同任务时将激活的专家加载到GPU上，从而压缩模型运行所需硬件条件。

![](img/d495ca42e58b440ce56b60a04c5712d6.png)

1.  该技术能够实现单卡24G显存运行1.58bit到2.51bit的DeepSeek R1模型，并且提供了一整套动态方案，支持从单卡24G到双卡80G服务器运行动态量化的R1模型，并且对于内存和CPU没有任何要求。

![](img/eb8e55642a2cc347a51af40e8313b88b.png)

1.  通常意义下量化程度越深，模型效果越差，但由于Unsloth出色的技术能力，导致哪怕是1.58bit量化情况下，量化模型仍能拥有大部分原版模型的能力。

![](img/491481fadd930a07e383e85e40598617.png)

+   Unsloth主页：https://unsloth.ai/

![](img/30c46ff23a646def0ae40bf8460cd28c.png)

本节公开课我们将重点介绍KTransformers方案的部署流程，实现在单卡4090+480G内存服务器上部署并调用DeepSeek R1满血版模型，并介绍如何使用KTransformers+FastAPI封装OpenAI风格的API，并将其与Open-WebUI进行集成。

**若**大家对Unsloth方案也感兴趣，可以在视频下方留言，我将根据大家的反馈决定是否继续讲解Unsloth方案实践流程。

### 2. 实验服务器配置说明

本次公开课服务器配置如下：

+   深度学习环境：PyTorch 2.5.1、Python 3.12(ubuntu22.04)、Cuda 12.4

+   硬件环境：

+   **GPU**：RTX 4090(24GB) * 4（实际只使用一张GPU）

+   **CPU**：64 vCPU Intel(R) Xeon(R) Gold 6430

+   **内存**：480G（至少需要382G）

+   **硬盘**：1.8T（实际使用需要380G左右）

KTransformer项目部署硬件配置方面需要注意如下事项：

+   GPU对实际运行效率提升不大，单卡3090、单卡4090、或者是多卡GPU服务器都没有太大影响，只需要留足20G以上显存（最小可行性实验的话只需要14G显存）即可；

+   若是多卡服务器，则可以进一步尝试手动编写模型权重卸载规则，使用更多的GPU进行推理，可以一定程度减少内存需求，但对于实际运行效率提升不大。最省钱的方案仍然是单卡GPU+大内存配置；

+   KTransformer目前开放了V2.0、V2.1和V3.0三个版本（V3.0目前只有预览版，只支持二进制文件下载和安装），其中V2.0和V2.1支持各类CPU，但从V3.0开始，只支持AMX CPU，也就是最新几代的Intel CPU。这几个版本实际部署流程和调用指令没有任何区别，公开课以适配性最广泛的V2.0版本进行演示，若当前CPU支持AMX，则可以考虑使用V3.0进行实验，推理速度会大幅加快。

+   CPU AMX（Advanced Matrix Extensions）是Intel在其Sapphire Rapids系列处理器中推出的一种新型硬件加速指令集，旨在提升矩阵运算的性能，尤其是针对深度学习和人工智能应用。

+   可以考虑在AutoDL上租赁4卡4090服务器，480G内存，约14元每小时。

### 3. 参考资料与课件领取

公开课全套代码课件、以及项目源码和自定义的脚本，都已上传至”赋泛大模型技术社区“

![](img/2af2ab8c0a9e0a8c833d2243071d2d32.png)

![](img/a98205d91966baffbbbaa82e3acdaab9.png)

👆扫码即可领取全部课程资料👆

其他更多相关参考资料

+   《DeepSeek R1本地部署流程》https://www.bilibili.com/video/BV19kFoe6Ef7/

![](img/74baede6e793c7816027ef42f3970926.png)

+   《AutoDL快速入门》：https://www.bilibili.com/video/BV1bxB7YYEST/

![](img/e69af17585b08f33aa62dd76a5f4d9bc.png)

## 二、KTransformers入门介绍与基础环境搭建

### 1.KTransformers项目入门介绍

#### 1.1 项目定位

KTransformers（发音为“Quick Transformers”）旨在通过先进的内核优化和计算分布/并行化策略来增强你使用Transformers的体验。

KTransformers 是一个灵活、以 Python 为中心的框架，其核心设计理念是可扩展性。用户仅需一行代码，即可实现优化模块的集成，并享受到以下特性：

+   与 Transformers 兼容的接口

+   符合 OpenAI 和 Ollama 规范的 RESTful API

+   一个简化版的 ChatGPT 风格 Web UI（最新版已弃用）

项目定位将 KTransformers 打造成一个灵活的平台，供用户探索和实验创新的大模型推理优化技术。因此，项目支持编写自定义脚本来实现模型权重的灵活卸载。

#### 1.2 项目参考资料

+   GitHub主页：https://github.com/kvcache-ai/ktransformers

+   项目使用指南：https://kvcache-ai.github.io/ktransformers/index.html

#### 1.3 KTransformers支持的模型及运行方式

+   **KT支持的模型类型：**

![](img/2825e0967520b6a90a623ce42628d397.png)

+   **KT支持的量化形式**

![](img/4c846736c8b04ea43a365235705712ed.png)

+   **不同模型所需运行条件**

![](img/8ae99eb7f318abbdc55da19b2560685a.png)

#### 1.4 KTransformers部署方法

KTransformers支持在Windows、Linux等操作系统下，使用源码部署或者docker工具进行部署。考虑到更为一般的企业级应用场景，本次实验采用Linux系统作为基础环境进行演示，并采用源码部署的方法进行部署。

### 2.DeepSeek R1模型权重与配置文件下载

本次实验使用官方推荐的DeepSeek R1 Q4_K_M，直接使用Unsloth压制的模型即可（此外也可以自己使用llama.cpp进行模型压缩），模型下载地址：

+   魔搭社区下载地址：https://www.modelscope.cn/models/unsloth/DeepSeek-R1-GGUF

![](img/cfb2571d50e7e114232057019741b820.png)

+   HuggingFace下载地址：https://huggingface.co/unsloth/DeepSeek-R1-GGUF

![](img/83591ca08af84ea723143d5b84e4625f.png)

模型权重较大，总共约350G左右。若使用AutoDL，最快下载方法是开启学术加速并从Huggingface上进行下载。

> AutoDL学术加速方法介绍：https://www.autodl.com/docs/network_turbo/

> 这里默认以AutoDL为基础实验环境进行介绍，默认已安装好CUDA、Miniconda等基础库。

下载流程如下：

+   安装huggingface_hub：

+   在命令行中输入

```
pip install huggingface_hub
```

![](img/8929af04f3e648905c45ec1bc1e8e374.png)

+   【可选】借助screen持久化会话

+   由于实际下载时间可能持续4-6个小时，因此最好使用screen开启持久化会话，避免因为关闭会话导致下载中断。

```
screen -S kt
```

+   创建一个名为kt的会话。之后哪怕关闭了当前会话，也可以使用如下命令

```
screen -r kt
```

+   若未安装screen，可以使用sudo apt install screen命令进行安装。

+   【可选】修改huggingface默认下载路径

+   在默认情况下，Huggingface会将下载文件保存在/root/.cache文件夹中，若想更换默认下载文件夹，则可以按照如下方式修改环境变量，或者在下载代码中设置下载路径。

+   首先在/root/autodl-tmp下创建名为HF_download文件夹作为huggingface下载文件保存文件夹（具体文件夹名称和地址可以自选）：

```
cd /root/autodl-tmp
mkdir HF_download
```

![](img/71d4b25964a5274a8c20e4e3c71740e1.png)

+   然后找到root文件夹下的.bashrc文件

![](img/d4599913b01e41b62f309b2810baa3dd.png)

+   在结尾处加上export HF_HOME="/root/autodl-tmp/HF_download"

![](img/31f0d6918a1465cfbe8281c6ff7c1256.png)

+   保存退出，输入

```
source ~/.bashrc
```

+   使环境变量生效。

+   下载模型权重

+   启动Jupyter

```
jupyter lab --allow-root
```

+   然后在开启的Jupyter页面中输入如下Python代码：

```
# 开启学术加速
import subprocess
import os

result = subprocess.run(&#x27;bash -c &quot;source /etc/network_turbo &amp;&amp; env | grep proxy&quot;&#x27;, shell=True, capture_output=True, text=True)
output = result.stdout
for line in output.splitlines():
    if &#x27;=&#x27; in line:
        var, value = line.split(&#x27;=&#x27;, 1)
        os.environ[var] = value
```

```
# 下载模型权重，只下载Q4_K_M部分权重
from huggingface_hub import snapshot_download
snapshot_download(
  repo_id = &quot;unsloth/DeepSeek-R1-GGUF&quot;,
  local_dir = &quot;DeepSeek-R1-GGUF&quot;,      
  allow_patterns = [&quot;*Q4_K_M*&quot;],
)
```

![](img/9eb25032bd292660aae0179c76e02f9f.png)

+

![](img/8560f96d0eea59bc63889b198efc13a2.png)

+   完成下载数个小时，下载过程需要持续启动Jupyter服务，其中如果出现下载中断，重新运行下载代码即可继续下载。

![](img/30172ace23d1066c0510ceff09c063b1.png)

+   然后即可在/root/autodl-tmp/DeepSeek-R1-GGUF/DeepSeek-R1-Q4_K_M中看到下载的GGUF格式模型权重：

![](img/9b4617163a759f3f35f1fdd678874d52.png)

+   【其他方案】使用魔搭社区进行下载

+   若是使用modelscope进行权重下载，则需要先安装魔搭社区

```
pip install modelscope
```

+   然后输入如下命令进行下载

```
modelscope download --model unsloth/DeepSeek-R1-GGUF  --include &#x27;**Q4_K_M**&#x27;  --local_dir /root/autodl-tmp/DeepSeek-R1-GGUF
```

![](img/8075e36b864b5983975f4a3a76f5e723.png)

+   下载DeepSeek R1原版模型的配置文件

+   此外，根据KTransformer的项目要求，还需要下载DeepSeek R1原版模型的除了模型权重文件外的其他配置文件，方便进行灵活的专家加载。因此我们还需要使用modelscope下载DeepSeek R1模型除了模型权重（.safetensor）外的其他全部文件，可以按照如下方式进行下载

```
mkdie ./DeepSeek-R1

modelscope download --model deepseek-ai/DeepSeek-R1  --exclude &#x27;*.safetensors&#x27;  --local_dir /root/autodl-tmp/DeepSeek-R1
```

![](img/9126d5b1d91287a81ae1c13d50dd4509.png)

+   下载后完整文件如下所示：

![](img/4ab1790bc550c383416b42616d251028.png)

+   相关文件也可以在课程课件中领取：

![](img/b9b35f0e99cf4e64d5cd247bd1ebac63.png)

![](img/50895d9d9059c50ecf73e1bc3ed1048d.png)

+   成果汇总

+   这里最终我们是下载了DeepSeek R1 Q4_K_M模型权重和DeepSeek R1的模型配置文件，并分别保存在两个文件夹中：

+   DeepSeek R1 Q4_K_M模型权重地址：/root/autodl-tmp/DeepSeek-R1-GGUF/DeepSeek-R1-Q4_K_M

![](img/501667914f5747b1fdbc516be5ab924c.png)

+   DeepSeek R1的模型配置文件地址：/root/autodl-tmp/DeepSeek-R1

![](img/c224cf05639cceaa7ca7cf60fe3ca60d.png)

## 三、KTransformer项目部署与调用流程

在准备好了DeepSeek R1 Q4_K_M的模型权重和DeepSeek R1模型配置文件之后，接下来开始着手部署KTransformer。该项目部署流程非常复杂，请务必每一步都顺利完成后，再执行下一步。在正式开始安装前，有以下几点需要事先声明：

+   关于版本：目前KT开放了V2.0、V2.1和V3.0预览版。课程以目前兼容性最强的V2.0进行演示，并介绍V3.0部署方法。若CPU满足要求（有AMX功能），则可运行V3.0。

+   V3.0版本需求：V3.0对软硬件环境要求较高，除了要求CPU支持AMX功能外，还要求Python 3.11以上及CUDA12.6。

+   项目Bug说明：在顺利实现DeepSeek R1高性能部署前，KT项目并不受关注，因此很多功能处于”年久失修“的状态。如KT宣传的三项功能，transformer API、Server API和Web Server，目前只有transformer API可以兼容DeepSeek R1的高性能部署，因此公开课的后半段，我会手写Server API。并且伴随着目前项目受更多关注，项目也在快速迭代，为了确保能够顺利运行，大家可以使用课程里面配套的项目原版代码+改写的脚本进行运行。其他版本的KT不确保能否运行成功。

![](img/26f37d60079fcf77f5667631f63929a3.png)

![](img/d4abd4823b0bfee28914c32b71c5bd22.png)

### 1. 安装基础依赖

首先需要安装**gcc**、**g++** 和 **cmake**等基础库：

```
sudo apt-get update
sudo apt-get install gcc g++ cmake ninja-build
```

然后继续安装 **PyTorch**、**packaging**、**ninja**：

```
pip install torch packaging ninja cpufeature numpy
```

接下来需要继续安装flash-attn

```
pip install flash-attn
```

以及需要手动安装libstdc

```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install --only-upgrade libstdc++6
```

```
conda install -c conda-forge libstdcxx-ng
```

### 2. 安装KTransformers

下载KT压缩包：

![](img/4ce8b5dca933922cfe022c2322c531ae.png)

![](img/5900010d1f118205409e877133a6e610.png)

上传至服务器指定路径/root/autodl-tmp，并使用如下命令进行解压缩：

```
unzip ktransformers-main.zip
mv ktransformers-main ktransformers
```

> 此外，也可以使用git clone https://github.com/kvcache-ai/ktransformers.git指令进行下载。但不同版本差异较大，需要区分。

然后进行部署，先进行初始化：

```
cd ktransformers
git submodule init
git submodule update
```

接下要需要确认当前CPU的类型，如果是双槽版本64核CPU，则需要使用如下命令设置NUMA=1：

```
export USE_NUMA=1
```

> 只需要在项目编译的时候输入一次即可。

例如，我当前的服务器CPU为：64 vCPU Intel(R) Xeon(R) Gold 6430

![](img/404acd772e4c111fd7152c966d9fb15f.png)

代表的就是64核双槽CPU，这种CPU往往出现在服务器使用场景中。

因此这里需要先输入export USE_NUMA=1，然后再执行后续命令。而这里如果不是64核双槽CPU，则无需执行这个命令。而若是64核双槽CPU，但未执行export USE_NUMA=1就执行了后续命令，则需要再次输入export USE_NUMA=1，然后再次运行后面的命令。

> 通过设置 USE_NUMA=1，你是在为系统和应用程序启用针对多CPU、多核架构的NUMA优化。这有助于提升在多处理器系统中的性能，特别是在处理并行计算和大规模数据时。

然后即可开始进行项目的安装——运行安装脚本：

```
sh ./install.sh
```

![](img/dd01a83f4dcf0283714b8566c272c2e0.png)

![](img/76e57582c0a65db80202a1c657af7b7a.png)

一切安装完成后，即可输入如下命令查看当前安装情况

```
pip show ktransformers
```

![](img/784dbcec1d56b988f809f43009261a0a.png)

> V 3.0安装方法

```
wget https://github.com/kvcache-ai/ktransformers/releases/download/v0.1.4/ktransformers-0.3.0rc0+cu126torch26fancy-cp311-cp311-linux_x86_64.whl
pip install ./ktransformers-0.3.0rc0+cu126torch26fancy-cp311-cp311-linux_x86_64.whl
```

> 其他流程完全一致。

### 3.运行KTransformer

部署完成后，即可尝试调用KTransformer进行对话。这里可以采用官方提供的最简单的对话脚本local_chat.py进行对话：

![](img/f08bd761820d3447d073e4d497db6589.png)

在项目根目录下输入如下命令：

```
python ./ktransformers/local_chat.py --model_path /root/autodl-tmp/DeepSeek-R1 --gguf_path /root/autodl-tmp/DeepSeek-R1-GGUF  --cpu_infer 65 --max_new_tokens 1000 --force_think true
```

参数解释如下：

+   <你的模型路径> 可以是本地路径，也可以是来自 Hugging Face 的在线路径（如 deepseek-ai/DeepSeek-V3）。如果在线出现连接问题，尝试使用镜像站点（如 hf-mirror.com）。

+   <你的GGUF路径> 也可以是在线路径，但由于文件较大，建议下载并量化模型以满足需求（注意，这是目录路径）。

+   --max_new_tokens 1000 是最大输出token长度。如果发现答案被截断，可以增加该值以获得更长的答案（但请注意，增大会导致OOM问题，并且可能减慢生成速度）。

+   --force_think true。打印R1模型的思考过程。

> 注1，可以添加命令 numactl -N 1 -m 1 旨在避免NUMA节点之间的数据传输。但如果此前设置了export USE_NUMA=1则不用再次输入该命令；

> 注2，若是单槽版本CPU，则不用输入参数--cpu_infer 65.

+   启动过程

+   需要完整加载61层模型权重

![](img/0968c1f5f1bad85686d0cdbbd67df7ca.png)

+   开启对话

+   稍等片刻即可开启命令行对话：

![](img/eccedeab3debb419de04a795135c0858.png)

+   响应速度

![](img/887984b97e20bd82aaf4af8f91ab10ff.png)

+   提示阶段（prompt eval）：模型处理了 12 个 token，耗时 2.7385 秒，处理速率为 4.38 个 token 每秒。

+   **评估阶段（eval eval）**：模型处理了 62 个 token，耗时 16.0092 秒，处理速率为 3.87 个 token 每秒。

+   由于AutoDL是虚拟化环境进行的运行，性能方面会受影响。

+   显存占用

+   仅占用不到14G显存。

![](img/967e0e2f2d8de369cc9ea45d249f4075.png)

## 四、编写OpenAI风格API调用脚本

### 1. 脚本编写流程

由于官方提供的Restful风格API未及时更新，无法顺利调用KT支持下的R1模型，为了便于开发，我们需要手动编写一个OpenAI风格的API，便于进行代码环境调用与其他开发工作。

我们需要在/root/autodl-tmp/ktransformers/ktransformers下创建一个chat_openai.py文件：

![](img/591b55df6aabef1440a0ee5d225d6ffa.png)

然后写入如下代码：

```
import argparse
import uvicorn
from typing import List, Dict, Optional, Any
from fastapi import FastAPI, HTTPException, status
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel
import os
import sys
import time
from fastapi import Request
from fastapi.responses import StreamingResponse, JSONResponse
import json
import logging

# 设置日志记录
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

project_dir = os.path.dirname(os.path.dirname(__file__))
sys.path.insert(0, project_dir)
import torch
from transformers import (
    AutoTokenizer,
    AutoConfig,
    AutoModelForCausalLM,
    GenerationConfig,
    TextStreamer,
)
from ktransformers.optimize.optimize import optimize_and_load_gguf
from ktransformers.models.modeling_deepseek import DeepseekV2ForCausalLM
from ktransformers.models.modeling_qwen2_moe import Qwen2MoeForCausalLM
from ktransformers.models.modeling_deepseek_v3 import DeepseekV3ForCausalLM
from ktransformers.models.modeling_llama import LlamaForCausalLM
from ktransformers.models.modeling_mixtral import MixtralForCausalLM
from ktransformers.util.utils import prefill_and_generate
from ktransformers.server.config.config import Config

custom_models = {
    &quot;DeepseekV2ForCausalLM&quot;: DeepseekV2ForCausalLM,
    &quot;DeepseekV3ForCausalLM&quot;: DeepseekV3ForCausalLM,
    &quot;Qwen2MoeForCausalLM&quot;: Qwen2MoeForCausalLM,
    &quot;LlamaForCausalLM&quot;: LlamaForCausalLM,
    &quot;MixtralForCausalLM&quot;: MixtralForCausalLM,
}

ktransformer_rules_dir = os.path.join(os.path.dirname(os.path.abspath(__file__)), &quot;optimize&quot;, &quot;optimize_rules&quot;)
default_optimize_rules = {
    &quot;DeepseekV2ForCausalLM&quot;: os.path.join(ktransformer_rules_dir, &quot;DeepSeek-V2-Chat.yaml&quot;),
    &quot;DeepseekV3ForCausalLM&quot;: os.path.join(ktransformer_rules_dir, &quot;DeepSeek-V3-Chat.yaml&quot;),
    &quot;Qwen2MoeForCausalLM&quot;: os.path.join(ktransformer_rules_dir, &quot;Qwen2-57B-A14B-Instruct.yaml&quot;),
    &quot;LlamaForCausalLM&quot;: os.path.join(ktransformer_rules_dir, &quot;Internlm2_5-7b-Chat-1m.yaml&quot;),
    &quot;MixtralForCausalLM&quot;: os.path.join(ktransformer_rules_dir, &quot;Mixtral.yaml&quot;),
}

# 全局变量，存储初始化后的模型
chat_model = None

class OpenAIChat:
    def __init__(
        self,
        model_path: str,
        optimize_rule_path: str = None,
        gguf_path: str = None,
        cpu_infer: int = Config().cpu_infer,
        use_cuda_graph: bool = True,
        mode: str = &quot;normal&quot;,
    ):
        torch.set_grad_enabled(False)
        Config().cpu_infer = cpu_infer

        self.tokenizer = AutoTokenizer.from_pretrained(model_path, trust_remote_code=True)
        config = AutoConfig.from_pretrained(model_path, trust_remote_code=True)
        self.streamer = TextStreamer(self.tokenizer, skip_prompt=True) if not Config().cpu_infer else None
        if mode == &#x27;long_context&#x27;:
            assert config.architectures[0] == &quot;LlamaForCausalLM&quot;, &quot;Only LlamaForCausalLM supports long_context mode&quot;
            torch.set_default_dtype(torch.float16)
        else:
            torch.set_default_dtype(config.torch_dtype)

        with torch.device(&quot;meta&quot;):
            if config.architectures[0] in custom_models:
                if &quot;Qwen2Moe&quot; in config.architectures[0]:
                    config._attn_implementation = &quot;flash_attention_2&quot;
                if &quot;Llama&quot; in config.architectures[0]:
                    config._attn_implementation = &quot;eager&quot;
                if &quot;Mixtral&quot; in config.architectures[0]:
                    config._attn_implementation = &quot;flash_attention_2&quot;
                model = custom_models[config.architectures[0]](config)
            else:
                model = AutoModelForCausalLM.from_config(
                    config, trust_remote_code=True, attn_implementation=&quot;flash_attention_2&quot;
                )

        if optimize_rule_path is None:
            if config.architectures[0] in default_optimize_rules:
                optimize_rule_path = default_optimize_rules[config.architectures[0]]

        optimize_and_load_gguf(model, optimize_rule_path, gguf_path, config)
        
        try:
            model.generation_config = GenerationConfig.from_pretrained(model_path)
        except:
            model.generation_config = GenerationConfig(
                max_length=128,
                temperature=0.7,
                top_p=0.9,
                do_sample=True
            )
        
        if model.generation_config.pad_token_id is None:
            model.generation_config.pad_token_id = model.generation_config.eos_token_id
        
        model.eval()
        self.model = model
        self.use_cuda_graph = use_cuda_graph
        self.mode = mode
        logger.info(&quot;Model loaded successfully!&quot;)

    def create_chat_completion(
        self,
        messages: List[Dict[str, str]],
        temperature: float = 0.7,
        max_tokens: int = 1000,
        top_p: float = 0.9,
        force_think: bool = False,
    ) -&gt; Dict:
        input_tensor = self.tokenizer.apply_chat_template(
            messages, add_generation_prompt=True, return_tensors=&quot;pt&quot;
        )
        
        if force_think:
            token_thinks = torch.tensor([self.tokenizer.encode(&quot;&lt;think&gt;\\n&quot;, add_special_tokens=False)],
                                        device=input_tensor.device)
            input_tensor = torch.cat([input_tensor, token_thinks], dim=1)

        generation_config = GenerationConfig(
            temperature=temperature,
            top_p=top_p,
            max_new_tokens=max_tokens,
            do_sample=True  # Ensure do_sample is True if using temperature or top_p
        )

        generated = prefill_and_generate(
            self.model,
            self.tokenizer,
            input_tensor.cuda(),
            max_tokens,
            self.use_cuda_graph,
            self.mode,
            force_think
        )

        # Convert token IDs to text
        generated_text = self.tokenizer.decode(generated, skip_special_tokens=True)

        return {
            &quot;choices&quot;: [{
                &quot;message&quot;: {
                    &quot;role&quot;: &quot;assistant&quot;,
                    &quot;content&quot;: generated_text
                }
            }],
            &quot;usage&quot;: {
                &quot;prompt_tokens&quot;: input_tensor.shape[1],
                &quot;completion_tokens&quot;: len(generated),
                &quot;total_tokens&quot;: input_tensor.shape[1] + len(generated)
            }
        }

class ChatMessage(BaseModel):
    role: str
    content: str

class ChatCompletionRequest(BaseModel):
    messages: List[ChatMessage]  # 确保 messages 是 Pydantic 模型实例的列表
    model: str = &quot;default-model&quot;
    temperature: Optional[float] = 0.7
    top_p: Optional[float] = 0.9
    max_tokens: Optional[int] = 1000
    stream: Optional[bool] = False
    force_think: Optional[bool] = True

class ChatCompletionResponse(BaseModel):
    id: str = &quot;chatcmpl-default&quot;
    object: str = &quot;chat.completion&quot;
    created: int = 0
    model: str = &quot;default-model&quot;
    choices: List[Dict[str, Any]]
    usage: Dict[str, int]

app = FastAPI(title=&quot;KVCache.AI API Server&quot;)

@app.get(&quot;/health&quot;)
async def health_check():
    return {&quot;status&quot;: &quot;healthy&quot;}

@app.middleware(&quot;http&quot;)
async def add_process_time_header(request: Request, call_next):
    start_time = time.time()
    response = await call_next(request)
    process_time = time.time() - start_time
    response.headers[&quot;X-Process-Time&quot;] = f&quot;{process_time:.4f}s&quot;
    return response

app.add_middleware(
    CORSMiddleware,
    allow_origins=[&quot;*&quot;],
    allow_credentials=True,
    allow_methods=[&quot;*&quot;],
    allow_headers=[&quot;*&quot;],
)

@app.post(&quot;/v1/chat/completions&quot;, response_model=ChatCompletionResponse)
async def chat_completion(request: ChatCompletionRequest):
    try:
        # 如果 messages 是 Pydantic 模型实例列表，使用 model_dump
        messages = [m.model_dump() for m in request.messages]
        response = chat_model.create_chat_completion(
            messages=messages,
            temperature=request.temperature,
            max_tokens=request.max_tokens,
            top_p=request.top_p,
            force_think=request.force_think
        )

        return {
            &quot;id&quot;: f&quot;chatcmpl-{int(time.time())}&quot;,
            &quot;object&quot;: &quot;chat.completion&quot;,
            &quot;created&quot;: int(time.time()),
            &quot;model&quot;: request.model,
            &quot;choices&quot;: [{
                &quot;index&quot;: 0,
                &quot;message&quot;: {
                    &quot;role&quot;: &quot;assistant&quot;,
                    &quot;content&quot;: response[&#x27;choices&#x27;][0][&#x27;message&#x27;][&#x27;content&#x27;]
                },
                &quot;finish_reason&quot;: &quot;stop&quot;
            }],
            &quot;usage&quot;: response[&#x27;usage&#x27;]
        }
    except Exception as e:
        logger.error(f&quot;API Error: {str(e)}&quot;)
        raise HTTPException(
            status_code=status.HTTP_500_INTERNAL_SERVER_ERROR,
            detail=f&quot;Internal server error: {str(e)}&quot;
        )

def create_app(model_path: str, gguf_path: str, cpu_infer:int, optimize_rule_path: Optional[str] = None):
    global chat_model
    chat_model = OpenAIChat(
        model_path=model_path,
        gguf_path=gguf_path,
        optimize_rule_path=optimize_rule_path,
        cpu_infer=cpu_infer
    )
    return app

def main():
    parser = argparse.ArgumentParser(description=&quot;KVCache.AI API Server&quot;)
    parser.add_argument(&quot;--model_path&quot;, type=str, required=True, help=&quot;HuggingFace模型路径&quot;)
    parser.add_argument(&quot;--gguf_path&quot;, type=str, required=True, help=&quot;GGUF模型文件路径&quot;)
    parser.add_argument(&quot;--optimize_rule_path&quot;, type=str, help=&quot;优化规则文件路径&quot;)
    parser.add_argument(&quot;--port&quot;, type=int, default=8000, help=&quot;服务端口号&quot;)
    parser.add_argument(&quot;--cpu_infer&quot;, type=int, default=70, help=&quot;使用cpu数量&quot;)
    parser.add_argument(&quot;--host&quot;, type=str, default=&quot;0.0.0.0&quot;, help=&quot;绑定地址&quot;)
    args = parser.parse_args()

    create_app(
        model_path=args.model_path,
        gguf_path=args.gguf_path,
        optimize_rule_path=args.optimize_rule_path,
        cpu_infer=args.cpu_infer
    )

    uvicorn.run(
        app,
        host=args.host,
        port=args.port,
        loop=&quot;uvloop&quot;,
        http=&quot;httptools&quot;,
        timeout_keep_alive=300,
        log_level=&quot;info&quot;,
        access_log=False
    )

if __name__ == &quot;__main__&quot;:
    main()
```

这段代码实现了一个基于 **FastAPI** 的 Web API 服务器，提供了一个聊天模型接口，并且支持 GPU 加速推理。接下来我会详细解释每个部分的作用。

1.  **导入库和初始化设置**

```
import argparse
import uvicorn
from typing import List, Dict, Optional, Any
from fastapi import FastAPI, HTTPException, status
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel
import os
import sys
import time
from fastapi import Request
from fastapi.responses import StreamingResponse, JSONResponse
import json
import logging
```

+   **argparse**：用于解析命令行参数。

+   **uvicorn**：用来运行 FastAPI 应用的 ASGI 服务器。

+   **FastAPI**：Web 框架，用于构建 API 服务。

+   **pydantic**：用于定义数据模型和数据验证。

+   **os****, ****sys****, ****time**：标准库，用于系统文件操作、时间控制等。

+   **logging**：用于记录日志信息，帮助调试和记录运行时信息。

1.  **设置日志**

```
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
```

+   **设置日志**的基础配置，日志级别为 INFO，并创建一个日志记录器 logger，方便在后续代码中记录日志。

1.  **设置项目目录和路径**

```
project_dir = os.path.dirname(os.path.dirname(__file__))
sys.path.insert(0, project_dir)
```

+   通过 os.path 获取项目的根目录并加入 sys.path 中，以便能够正确地导入项目中的模块。

1.  **导入深度学习模型相关库**

```
import torch
from transformers import (
    AutoTokenizer,
    AutoConfig,
    AutoModelForCausalLM,
    GenerationConfig,
    TextStreamer,
)
from ktransformers.optimize.optimize import optimize_and_load_gguf
from ktransformers.models.modeling_deepseek import DeepseekV2ForCausalLM
from ktransformers.models.modeling_qwen2_moe import Qwen2MoeForCausalLM
from ktransformers.models.modeling_deepseek_v3 import DeepseekV3ForCausalLM
from ktransformers.models.modeling_llama import LlamaForCausalLM
from ktransformers.models.modeling_mixtral import MixtralForCausalLM
from ktransformers.util.utils import prefill_and_generate
from ktransformers.server.config.config import Config
```

+   **这些库主要用于加载和使**用深度**学习模型，包含了 Hugg**ingFace 和 ktransformers 的相关模块，用于支持不同模型架构（如 Llama, Deepseek, Qwen2Moe 等）和模型推理优化。

1.  **定义模型和优化规则路径**

```
custom_models = {
    &quot;DeepseekV2ForCausalLM&quot;: DeepseekV2ForCausalLM,
    &quot;DeepseekV3ForCausalLM&quot;: DeepseekV3ForCausalLM,
    &quot;Qwen2MoeForCausalLM&quot;: Qwen2MoeForCausalLM,
    &quot;LlamaForCausalLM&quot;: LlamaForCausalLM,
    &quot;MixtralForCausalLM&quot;: MixtralForCausalLM,
}
```

+   这里定义了一个字典 custom_models，将每种自定义的模型类映射到其名称，以便根据配置自动加载相应的模型。

```
ktransformer_rules_dir = os.path.join(os.path.dirname(os.path.abspath(__file__)), &quot;optimize&quot;, &quot;optimize_rules&quot;)
default_optimize_rules = {
    &quot;DeepseekV2ForCausalLM&quot;: os.path.join(ktransformer_rules_dir, &quot;DeepSeek-V2-Chat.yaml&quot;),
    &quot;DeepseekV3ForCausalLM&quot;: os.path.join(ktransformer_rules_dir, &quot;DeepSeek-V3-Chat.yaml&quot;),
    &quot;Qwen2MoeForCausalLM&quot;: os.path.join(ktransformer_rules_dir, &quot;Qwen2-57B-A14B-Instruct.yaml&quot;),
    &quot;LlamaForCausalLM&quot;: os.path.join(ktransformer_rules_dir, &quot;Internlm2_5-7b-Chat-1m.yaml&quot;),
    &quot;MixtralForCausalLM&quot;: os.path.join(ktransformer_rules_dir, &quot;Mixtral.yaml&quot;),
}
```

+   定义了模型优化规则的路径，针对不同模型配置不同的规则文件。

1.  **OpenAIChat 类**

```
class OpenAIChat:
    def __init__(self, model_path: str, optimize_rule_path: str = None, gguf_path: str = None, cpu_infer: int = Config().cpu_infer, use_cuda_graph: bool = True, mode: str = &quot;normal&quot;):
        # 初始化模型和配置
```

+   **OpenAIChat** 类用于加载聊天模型并进行推理。

+   在 __init__ 方法中，使用 AutoTokenizer 和 AutoConfig 从指定的 model_path 加载模型和配置。

+   根据配置初始化不同类型的模型，并根据需要加载优化规则。

1.  **生成聊天响应**

```
def create_chat_completion(self, messages: List[Dict[str, str]], temperature: float = 0.7, max_tokens: int = 1000, top_p: float = 0.9, force_think: bool = False) -&gt; Dict:
    # 使用模型生成聊天回复
```

+   create_chat_completion 方法将输入的消息传入模型，生成聊天回复。

+   它使用 GenerationConfig 配置生成参数，并调用 prefill_and_generate 函数来生成最终的文本。

1.  **FastAPI 路由定义**

```
@app.post(&quot;/v1/chat/completions&quot;, response_model=ChatCompletionResponse)
async def chat_completion(request: ChatCompletionRequest):
    # 处理聊天请求，生成聊天响应
```

+   /v1/chat/completions 路由是 API 的核心，用于接收聊天请求并返回生成的聊天响应。

+   request 参数为客户端发来的请求数据（通过 Pydantic 模型进行验证）。

1.  **启动服务器**

```
def main():
    parser = argparse.ArgumentParser(description=&quot;KVCache.AI API Server&quot;)
    # 解析命令行参数并启动 FastAPI 服务
    uvicorn.run(app, host=args.host, port=args.port)
```

+   main** 函数解析命令**行参数，获取模型路径、端口等配置，并启动 FastAPI 服务。

此外，也可以在网盘中下载py文件，并上传至服务器：

![](img/b40237ec7121b4750dc2a6316d6f0b67.png)

![](img/60d462755ada599cf8f9d252bb28c384.png)

### 2. 启动FastAPI服务

+   安装相关依赖

```
pip install protobuf uvicorn httptools
```

+   输入启动命令

```
python ./ktransformers/chat_openai.py  --model_path /root/autodl-tmp/DeepSeek-R1 --gguf_path /root/autodl-tmp/DeepSeek-R1-GGUF  --cpu_infer 65
```

还是需要等待加载全部模型权重后，才可进行使用：

![](img/5624872820368791ea2e2b7062b8472b.png)

### 2. 借助OpenAI风格API调用R1模型

接下来即可在Jupyter中尝试对其进行调用了。

```
from openai import OpenAI

ds_api_key = &quot;none&quot;

# 实例化客户端
client = OpenAI(api_key=ds_api_key, 
                base_url=&quot;http://localhost:8000/v1&quot;)

# 调用 deepseekv3 模型
response = client.chat.completions.create(
    model=&quot;DeepseekV3ForCausalLM&quot;,
    messages=[
        {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，好久不见!&quot;}
    ]
)
```

```
response
```

![](img/d946f2ac30c6acf0789c4071039ae8e3.png)

![](img/7e91359f5800a5f333c525afbe70b553.png)

## 五、DeepSeek R1部署基础环境搭建

### 1.Open-WebUI部署流程

首先需要安装Open-WebUI，官网地址如下：https://github.com/open-webui/open-webui。

![](img/a39a16285ea31862636fef131b911b70.png)

我们可以直接使用pip命令快速完成安装：

```
pip install open-webui
```

![](img/ab452711c682df464657db5507488d1d.png)

可以直接使用在GitHub项目主页上直接下载完整代码包，并上传至服务器解压缩运行：

![](img/5eeefdb245a40796d90ae4db4fc4e8c4.png)

此外，也可以在课件网盘中领取完整代码包，并上传至服务器解压缩运行：

![](img/9581ffb55ea4cbd985df1b334cd6d0c1.png)

![](img/6b222fd569aedfdd602c97d2adfde423.png)

在工具栏写入函数：

```
import os
import json
import requests
from pydantic import BaseModel, Field
from typing import List, Union, Iterator

# Set DEBUG to True to enable detailed logging
DEBUG = False

class Pipe:
    class Valves(BaseModel):
        openai_API_KEY: str = Field(default=&quot;none&quot;)  # Optional API key if needed
        DEFAULT_MODEL: str = Field(default=&quot;DeepSeek-R1&quot;)  # Default model identifier

    def __init__(self):
        self.id = &quot;DeepSeek-R1&quot;
        self.type = &quot;manifold&quot;
        self.name = &quot;KT: &quot;
        self.valves = self.Valves(
            **{
                &quot;openai_API_KEY&quot;: os.getenv(&quot;openai_API_KEY&quot;, &quot;none&quot;),
                &quot;DEFAULT_MODEL&quot;: os.getenv(&quot;openai_DEFAULT_MODEL&quot;, &quot;DeepSeek-R1&quot;),
            }
        )
        # Self-hosted FastAPI server details
        self.api_url = &quot;http://localhost:8000/v1/chat/completions&quot;  # FastAPI server endpoint
        self.headers = {&quot;Content-Type&quot;: &quot;application/json&quot;}

    def get_openai_models(self):
        &quot;&quot;&quot;Return available models - for openai we&#x27;ll return a fixed list&quot;&quot;&quot;
        return [{&quot;id&quot;: &quot;KT&quot;, &quot;name&quot;: &quot;DeepSeek-R1&quot;}]

    def pipes(self) -&gt; List[dict]:
        return self.get_openai_models()

    def pipe(self, body: dict) -&gt; Union[str, Iterator[str]]:
        try:
            # Use default model ID since openai has a single endpoint
            model_id = self.valves.DEFAULT_MODEL

            messages = []

            # Process messages including system, user, and assistant messages
            for message in body[&quot;messages&quot;]:
                if isinstance(message.get(&quot;content&quot;), list):
                    # For openai, we&#x27;ll join multiple content parts into a single text
                    text_parts = []
                    for content in message[&quot;content&quot;]:
                        if content[&quot;type&quot;] == &quot;text&quot;:
                            text_parts.append(content[&quot;text&quot;])
                        elif content[&quot;type&quot;] == &quot;image_url&quot;:
                            # openai might not support image inputs - add a note about the image
                            text_parts.append(f&quot;[Image: {content[&#x27;image_url&#x27;][&#x27;url&#x27;]}]&quot;)
                    messages.append(
                        {&quot;role&quot;: message[&quot;role&quot;], &quot;content&quot;: &quot; &quot;.join(text_parts)}
                    )
                else:
                    # Handle simple text messages
                    messages.append(
                        {&quot;role&quot;: message[&quot;role&quot;], &quot;content&quot;: message[&quot;content&quot;]}
                    )

            if DEBUG:
                print(&quot;FastAPI API request:&quot;)
                print(&quot;  Model:&quot;, model_id)
                print(&quot;  Messages:&quot;, json.dumps(messages, indent=2))

            # Prepare the API call parameters
            payload = {
                &quot;model&quot;: model_id,
                &quot;messages&quot;: messages,
                &quot;temperature&quot;: body.get(&quot;temperature&quot;, 0.7),
                &quot;top_p&quot;: body.get(&quot;top_p&quot;, 0.9),
                &quot;max_tokens&quot;: body.get(&quot;max_tokens&quot;, 8192),
                &quot;stream&quot;: body.get(&quot;stream&quot;, True),
            }

            # Add stop sequences if provided
            if body.get(&quot;stop&quot;):
                payload[&quot;stop&quot;] = body[&quot;stop&quot;]

            # Sending request to local FastAPI server
            if body.get(&quot;stream&quot;, False):
                # Streaming response
                def stream_generator():
                    try:
                        response = requests.post(self.api_url, json=payload, headers=self.headers, stream=True)
                        for line in response.iter_lines():
                            if line:
                                yield line.decode(&quot;utf-8&quot;)
                    except Exception as e:
                        if DEBUG:
                            print(f&quot;Streaming error: {e}&quot;)
                        yield f&quot;Error during streaming: {str(e)}&quot;

                return stream_generator()
            else:
                # Regular response
                response = requests.post(self.api_url, json=payload, headers=self.headers)
                if response.status_code == 200:
                    generated_content = response.json().get(&quot;choices&quot;, [{}])[0].get(&quot;message&quot;, {}).get(&quot;content&quot;, &quot;&quot;)
                    return generated_content
                else:
                    return f&quot;Error: {response.status_code}, {response.text}&quot;

        except Exception as e:
            if DEBUG:
                print(f&quot;Error in pipe method: {e}&quot;)
            return f&quot;Error: {e}&quot;

    def health_check(self) -&gt; bool:
        &quot;&quot;&quot;Check if the openai API (local FastAPI service) is accessible&quot;&quot;&quot;
        try:
            # Simple health check with a basic prompt
            response = requests.post(
                self.api_url,
                json={
                    &quot;model&quot;: self.valves.DEFAULT_MODEL,
                    &quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;Hello&quot;}],
                    &quot;max_tokens&quot;: 5,
                },
                headers=self.headers,
            )
            return response.status_code == 200
        except Exception as e:
            if DEBUG:
                print(f&quot;Health check failed: {e}&quot;)
            return False
```

即可进行调用了：

![](img/c2c36affc4797b5f9ed903c4dbd9abeb.png)

实际对话效果如下：

![](img/ea2e5522c4c57aa393e24b9dac103637.png)