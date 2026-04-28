# Qwen-3部署与调用详解（上）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/JCHqdZqT1oKo4bxSGJec35jxnOe](https://kq4b3vgg5b.feishu.cn/docx/JCHqdZqT1oKo4bxSGJec35jxnOe)

## 课程说明：

+   体验课内容节选自《2025大模型Agent智能体开发实战》(5月班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(5月班)

![](img/74dc47b93400002585fa9d6b441a20e7.png)

![](img/fe3711ca960629b236346f3e911f7ff2.png)

此外，公开课全套学习资料，已上传至网盘

（https://pan.baidu.com/s/1wtLsh6h3RhTfCBFgWa5BYQ 提取码: r8rq ）

需要更系统深入学习大模型可扫码⬆️添加助教咨询喔～

---

## 一、Qwen3系列模型介绍

### 1. Qwen3 模型简介

2025年4月29日凌晨，阿里巴巴开源新一代通义千问模型Qwen3，其中旗舰模型 Qwen3-235B-A22B 在代码、数学、通用能力等基准测试中，与 DeepSeek-R1、o1、o3-mini、Grok-3 和 Gemini-2.5-Pro 等顶级模型相比，表现出极具竞争力的结果。此外，小型 MoE 模型 Qwen3-30B-A3B 的激活参数数量是 QwQ-32B 的 10%，表现更胜一筹，甚至像 Qwen3-4B 这样的小模型也能匹敌 Qwen2.5-72B-Instruct 的性能，性能全面超越Deepseek-R1、豆包1.5pro 等国内领先模型，并登顶全球最强开源模型。

![](img/3bcf3b7406231b392b523bdd286e405e.png)

![](img/f4a242ae8c5ccf219e2b61fb26ce4643.png)

Qwen3原生支持MCP协议，并具备强大的工具调用（function calling）能力，并且集成封装了工具调用模板和工具调用解析器的Qwen-Agent 框架，大大降低编码复杂性，Qwen3 的工具调用能力，支持实现高效的手机及电脑Agent操作等任务。

### 2. Qwen3 模型特性

最新版本Qwen3有以下特点：

+   全尺寸稠密与混合专家模型：0.6B, 1.7B, 4B, 8B, 14B, 32B and 30B-A3B, 235B-A22B

+   支持在**思考模式**（用于复杂逻辑推理、数学和编码）**和 非思**考模式 （用于高效通用对话）之间无缝切换，确保在各种场景下的最佳性能。

+   显著增强的推理能力，在数学、代码生成和常识逻辑推理方面超越了之前的 QwQ（在思考模式下）和 Qwen2.5 指令模型（在非思考模式下）。

+   卓越的人类偏好对齐，在创意写作、角色扮演、多轮对话和指令跟随方面表现出色，提供更自然、更吸引人和更具沉浸感的对话体验。

+   擅长智能体能力，可以在思考和非思考模式下精确集成外部工具，在复杂的基于代理的任务中在开源模型中表现领先。

+   支持 100 多种语言和方言，具有强大的多语言理解、推理、指令跟随和生成能力。

同时Qwen3系列模型的核心亮点具备多种思考模式：

+   思考模式：在这种模式下，模型会逐步推理，经过深思熟虑后给出最终答案。这种方法非常适合需要深入思考的复杂问题。

+   非思考模式：在此模式中，模型提供快速、近乎即时的响应，适用于那些对速度要求高于深度的简单问题。

这种灵活性使用户能够根据具体任务控制模型进行“思考”的程度。例如，复杂的问题可以通过扩展推理步骤来解决，而简单的问题则可以直接快速作答，无需延迟。至关重要的是，这两种模式的结合大大增强了模型实现稳定且高效的“思考预算”控制能力。如上文所述，Qwen3 展现出可扩展且平滑的性能提升，这与分配的计算推理预算直接相关。这样的设计让用户能够更轻松地为不同任务配置特定的预算，在成本效益和推理质量之间实现更优的平衡。

![](img/b867238bc4a0d26e994ee8316538bda1.png)

### 3. Qwen3 模型训练流程

在预训练方面，Qwen3 的数据集相比 Qwen2.5 有了显著扩展。Qwen2.5是在 18 万亿个 token 上进行预训练的，而 Qwen3 使用的数据量几乎是其两倍，达到了约 36 万亿个 token，涵盖了 119 种语言和方言。为了构建这个庞大的数据集，我们不仅从网络上收集数据，还从 PDF 文档中提取信息。我们使用 Qwen2.5-VL 从这些文档中提取文本，并用 Qwen2.5 改进提取内容的质量。为了增加数学和代码数据的数量，我们利用 Qwen2.5-Math 和 Qwen2.5-Coder 这两个数学和代码领域的专家模型合成数据，合成了包括教科书、问答对以及代码片段等多种形式的数据。

预训练过程分为三个阶段。在第一阶段（S1），模型在超过 30 万亿个 token 上进行了预训练，上下文长度为 4K token。这一阶段为模型提供了基本的语言技能和通用知识。在第二阶段（S2），我们通过增加知识密集型数据（如 STEM、编程和推理任务）的比例来改进数据集，随后模型又在额外的 5 万亿个 token 上进行了预训练。在最后阶段，我们使用高质量的长上下文数据将上下文长度扩展到 32K token，确保模型能够有效地处理更长的输入。

![](img/a7b93feef3729b98bdcb20468a95fb43.png)

为了开发能够同时具备思考推理和快速响应能力的混合模型，Qwen3模型实施了一个四阶段的训练流程。该流程包括：（1）长思维链冷启动，（2）长思维链强化学习，（3）思维模式融合，以及（4）通用强化学习。

在第一阶段，使用多样的的长思维链数据对模型进行了微调，涵盖了数学、代码、逻辑推理和 STEM 问题等多种任务和领域。这一过程旨在为模型配备基本的推理能力。第二阶段的重点是大规模强化学习，利用基于规则的奖励来增强模型的探索和钻研能力。

在第三阶段，在一份包括长思维链数据和常用的指令微调数据的组合数据上对模型进行微调，将非思考模式整合到思考模型中。确保了推理和快速响应能力的无缝结合。最后，在第四阶段，在包括指令遵循、格式遵循和 Agent 能力等在内的 20 多个通用领域的任务上应用了强化学习，以进一步增强模型的通用能力并纠正不良行为。

### 4.Qwen3模型相关资料

+   Qwen3模型技术博客：https://qwenlm.github.io/zh/blog/qwen3/

![](img/38ab744d4e4786d5546b320979194abe.png)

+   Qwen3系列魔搭社区主页：https://modelscope.cn/collections/Qwen3-9743180bdc6b48

![](img/e08a7d1581bda2f34474d012548207d4.png)

+   Qwen3系列 huggingface：https://huggingface.co/collections/Qwen/qwen3-67dd247413f0e2e4f653967f

![](img/4aad054f1c3d32e65f53168ef943a0f9.png)

+   Qwen3模型GitHub主页：https://github.com/QwenLM/Qwen3

![](img/65ef5a9b291c072bd4cafc34d7fce907.png)

+   Qwen3模型入门介绍：https://www.bilibili.com/video/BV1fGGyzKEJf/

![](img/e550840a9b31f2748ded754cdc71e0bd.png)

### 5. Qwen3全系列模型性能及硬件需求介绍

#### 5.1 不同模型适用场景参考

![](img/3023555aaf69d76d266ecaee13b24422.png)

#### 5.2 各模型性能评分及配置参考

![](img/e19f12934d66c9ba3bcc5b40935848d9.png)

> 注：Qwen3-235B模型最低也可以在H20（98G）*4服务器上运行，但这种配置下运行非常极限，更推荐在H20（141G）*4服务器上运行，以获得更好的性能。

### 6. 公开课课件领取

+   **课件领取地址**

![](img/e72b219f54767b40ed4f67ba67ad1dc1.png)

+   公开课服务器配置【AutoDL服务器租赁运行】

+   PyTorch 2.5.1，Python 3.12(ubuntu22.04)，Cuda 12.4

+   操作系统：Ubuntu 22.04

+   **GPU**：H800 * 4

+   **CPU**：40 vCPU Intel(R) Xeon(R) Platinum 8458P

+   **内存**：200GB DDR4

+   基于此，公开课中将演示30B-A3B模型和8B dense模型部署和运行方法，此外大家也可以根据自己实际硬件情况合理选择模型进行部署和运行。

## 二、Qwen3模型下载与Transformers推理流程

### 1. Qwen3模型下载与环境搭建

+   【可选】创建虚拟环境

```
conda create --name qwen3 python=3.11
conda init
source ~/.bashrc
conda activate qwen3
```

![](img/481db500ccb5b04ed39c2970bc43994d.png)

+   【可选，若创建虚拟环境，则必须要执行】创建Jupyter Kernel

```
conda install jupyterlab
conda install ipykernel
python -m ipykernel install --user --name qwen3 --display-name &quot;Python qwen3&quot;
```

![](img/2dfbce083f7cb806cec60220f07bef6a.png)

+   安装魔搭社区

```
pip install modelscope
```

+   创建项目主目录

```
cd /root/autodl-tmp
mkdir Qwen3
```

![](img/b6232ac810b42be5180e93d059ce6816.png)

+   上传并安装依赖

![](img/b88080a9037d72e0ddf2d7a47f3df38c.png)

+   项目依赖在网盘中：

![](img/2fc0b47d70ffc361a1d9bbf8f15d7f96.png)

+   上传完即可进行安装

```
pip install -r requirements.txt
```

+   下载项目权重

+   一般来说我们可以在魔搭社区或者hugging上下载，考虑到国内网络环境，优先推荐在魔搭社区上选择模型版本并进行下载。进入到ModelScope 的主页中：https://www.modelscope.cn/home

![](img/f664e3e1bedab7d0c2a04983c480e4c1.png)

+   通过Qwen3关键词找到对应的模型详情页，点击进入，如下图所示：

![](img/671d2b8c97eba4d899dc3a01cc55beaa.png)

+

![](img/1b1d464be29b4cebae184a5f5f778513.png)

+   这里我们选择14B模型进行下载和测试：

```
cd /root/autodl-tmp/Qwen3
mkdir ./Qwen3-14B
modelscope download --model Qwen/Qwen3-14B --local_dir ./Qwen3-14B
```

![](img/18c87eaf8ebd9752f2a36c6f71322590.png)

![](img/09fe4b4704704d1a11686d6fe4c81bba.png)

### 2. Jupyter中使用transformer原生库调用流程

然后即可使用transformer进行调用，具体调用步骤如下：

+   导入相关库

```
from modelscope import AutoModelForCausalLM, AutoTokenizer
```

+   设置模型下载地址

```
model_name = &quot;./Qwen3-14B&quot;
```

+   实例化预训练模型与分词器

```
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(
    model_name, 
    torch_dtype=&quot;auto&quot;,  # 指定模型权重数据类型
    device_map=&quot;auto&quot;  # 指定模型权重加载设备
)
```

+   创建消息message

```
prompt = &quot;给出一个关于大语言模型的简短介绍。&quot;
messages = [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt}
]
```

+   词嵌入过程

```
text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True,
    enable_thinking=True  # 是否启用思考模式
)
model_inputs = tokenizer([text], return_tensors=&quot;pt&quot;).to(model.device)
```

+   创建并回复

```
# 进行文本补全
generated_ids = model.generate(
    **model_inputs,
    max_new_tokens=32768
)
output_ids = generated_ids[0][len(model_inputs.input_ids[0]):].tolist() 

# 结果将首先显示思考内容，在&lt;think&gt;&lt;/think&gt;标签中，然后是实际响应
print(tokenizer.decode(output_ids, skip_special_tokens=True))
```

+   最终运行结果如下：

![](img/b3a2d8902ca1c4c8e4f6ed6937b20bff.png)

![](img/273f1c70c9564a034054d43b0b9baeec.png)

而如果要禁用思考模式，则需要将enable_thinking=True改为False即可。

通过enable_thinking参数决定是否启用思考模式，如何设置为True，模型回复结果中就会包含思维链的全部内容。此时模型显存占用约62G。

![](img/3fa019936c6ba475e6a3638bd0859e09.png)

而除了使用原生的Transformers库加载以外，目前主流的大模型部署框架以llama.cpp、Ollama和Vllm为主，其各自项目在Github上的Stars增长曲线如下所示。其中Ollama的Stars增长曲线是断档式领先，而llama.cpp和Vllm的Stars增长曲线则相对平缓，处于一个稳步增长的趋势。

![](img/b4a22a5f3c535f0f10a3bb1c9570829f.png)

## 三、Qwen3-14B模型接入Ollama与推理流程

Ollama是在Github**上的一个开源项目，其项目定位**是：一个本地运行大模型的集成框架，目前主要针对主流的LLaMA架构的开源大模型设计，通过将模型权重、配置文件和必要数据封装进由Modelfile定义的包中，从而实现大模型的下载、启动和本地运行的自动化部署及推理流程。此外，Ollama内置了一系列针对大模型运行和推理的优化策略，目前作为一个非常热门的大模型托管平台，基本主流的大模型应用开发框架如LangChain、AutoGen、Microsoft GraphRAG及热门项目AnythingLLM、OpenWebUI等高度集成。

目前Qwen3系列模型已支持使用ollama进行调用，可以在模型主页查看调用情况：https://ollama.com/library/qwen3

![](img/c545f4c7ddc515870d0ea4ced43ef9ba.png)

### 1. Ollama项目安装

Ollama项目支持跨平台部署，目前已兼容Mac、Linux和Windows操作系统。特别地对Mac和Windows用户提供了非常直观的预览版，包括了内置的GPU加速功能、访问完整模型库的能力，以及对OpenAI的兼容性在内的Ollama REST API，对用户使用尤为友好。

但无论使用哪个操作系统，Ollama项目的安装过程都设计得非常简单。根据后续的课程的研发需求以及真实企业的应用需求，我们建议大家使用Linux系统进行实践。同时课程也将选择以Linux版本为例进行详细介绍。对于其他操作系统版本的安装，大家可以通过如下链接，根据自己的实际情况进行安装体验：https://github.com/ollama/ollama

![](img/4545083a27f2f84cbdd7cfa4f641b9a1.png)

#### 【安装方案一】Ollama在线安装

Ollama项目本地安装的方法极为简单，这里我们以Linux系统为例，先进入命令行终端，执行如下一条命令行即可自动化完成：

```
curl -fsSL https://ollama.com/install.sh | sh
```

> 注意：Qwen3系列模型虽然已经集成Qllama，但运行此模型需要 Ollama 0.6.6 或更高版本。

![](img/26237cea3eae3a216aeb94433c75981b.png)

这行命令的目的是从https://ollama.com/ 网站读取 install.sh 脚本，并立即通过 sh 执行该脚本，在安装过程中会包含以下几个主要的操作：

1.  检查当前服务器的基础环境，如系统版本等；

1.  下载Ollama的二进制文件；

1.  配置系统服务，包括创建用户和用户组，添加Ollama的配置信息；

1.  启动Ollama服务；

这个过程会比较慢，拉取的文件约2G左右，如果安装过程中未出现任何错误信息，通常情况下能够表明安装已成功。可以通过执行以下命令来检查Ollama服务的运行状态：

```
ollama list
```

![](img/7faf0410dd3deb84fd69761362142083.png)

#### 【安装方案二】Ollama离线安装

更为一般的情况下，推荐使用Ollama离线部署。我们可以在Ollama Github主页查看目前Ollama支持的各操作系统安装包：https://github.com/ollama/ollama/releases

![](img/15ab963541226cdc228f84fc93a15168.png)

若是Ubuntu操作系统，选择其中ollama-linux-amd64.tgz下载和安装即可。

![](img/7a34918829229a3ab7cfd036976672fa.png)

此外，Ollama的离线版本安装包也可从网盘中下载：

![](img/901772606729f88afd8b49af4f701dfa.png)

大家扫码即可下载。

![](img/b5cc0d336f7e418ac33dbf1816b37e76.png)

+   下载完成后，需要先上传至服务器：

![](img/e1bee0f49368efef765fc8039d82479d.png)

+   然后使用如下命令进行解压缩

```
mkdir ./ollama
tar -zxvf ollama-linux-amd64.tgz -C ./ollama
```

+   解压缩后项目文件如图所示：

![](img/5aa4432f42dbcd2ee7202084cd8fec01.png)

+   而在bin中，可以找到ollama命令的可执行文件。

![](img/d2d8118dd925d42abbd5fa67af6bbe9a.png)

+   此时，我们可以使用如下方式使用ollama：

```
cd ./bin
./ollama help
```

![](img/9e49a9236ba53666dfb09fbb75122b14.png)

+   此处若显示没有可执行权限，可以使用如下命令为当前脚本添加可执行权限：

```
chmod +x ollama
```

+   而为了使用命令方便，我们也可以将脚本文件写入环境变量中。我们可以在主目录（root）下找到.bashrc文件：

![](img/918e0a60ab7f3c8189c30d690b05c6c2.png)

+   然后在.bashrc文件结尾写入ollama/bin文件路径：

![](img/d1816a8206f91cc205053ff3fdd37286.png)

```
export PATH=$PATH:/root/autodl-tmp/ollama/bin
```

+   保存并退出后，输入如下命令来使环境变量生效：

```
source ~/.bashrc
```

+   然后在任意路径下输入如下命令，测试ollama环境变量是否生效

```
ollama help
```

![](img/e7df3037b993fe70663855da80fc8707.png)

+   【可选】更换Ollama默认模型权重下载地址

+   接下来我们需要使用ollama来下载模型，但默认情况下，ollama会将模型下载到/root/.ollama文件夹中，会占用系统盘空间，因此，若有需要，可以按照如下方法更换模型权重下载地址。

+   此外无论是在线还是离线安装的ollama，都可以按照如下方法更换模型权重下载地址。还是需要打开/root/.bashrc文件，写入如下代码：

```
export OLLAMA_MODELS=/root/autodl-tmp/models
```

![](img/84e9451db3300fc5fe26bb7acdae06f1.png)

+   这里的路径需要改写为自己的文件地址

+   保存并退出后，输入如下命令来使环境变量生效：

```
source ~/.bashrc
```

+   测试环境变量是否生效

```
echo $OLLAMA_MODELS
```

![](img/97a331411545a20e5ae11550f878dd2e.png)

+   启动ollama

+   接下来即可启动ollama，为后续下载模型做准备：

```
ollama start
```

![](img/96fb65ad9bf0be1e9f2f76edaadf228d.png)

+   注意，在整个应用使用期间，需要持续开启Ollama。

### 2. Qwen3-14B GGUF格式模型权重下载

ollama模型权重下载可以使用ollama pull命令进行在线下载（没有网络门槛，且速度很快），当然也可以手动下载模型权重，然后上传到服务器上，再注册到Ollama中，就可以通过Ollama调用模型了。以下两种模型权重下载方法任选其一执行即可。

#### 2.1 【方案一】在线下载模型权重

ollama中使用pull命令即可直接下载模型权重。

![](img/3075616af98fe9a53efa9a9ace618a39.png)

```
 ollama run qwen3:14B
```

> 注，在使用pull命令下载模型前，需要先启动ollama。

使用该命令后系统就会自动下载模型权重，并自动完成注册。等待模型下载完成后即可直接使用。

> 除此以外，也可以通过Ollama serve更灵活的指定启动参数。这里有一些关键的参数，可以控制Ollama 服务的启动行为：

+   **OLLAMA_HOST**：设置Ollama 服务的监听地址，默认是127.0.0.1:11434，如果需要指定其他地址，可以设置为公网IP:11434；

+   CUDA_VISIBLE_DEVICES：设置Ollama 服务使用的GPU设备，默认选择所有可用的GPU设备，如果需要指定其他GPU设备，可以设置为1，2 等，用逗号分隔；

+   OLLAMA_SCHED_SPREAD：设置Ollama所选择GPU资源是否均匀分布，默认是true，如果设置为false，则Ollama会优先选择性能最好的GPU设备；

+   OLLAMA_NUM_PARALLEL：设置Ollama 每个模型可以同时处理的最大并行请求数量。默认值会根据可用显（内）存自动选择4或1。

+   OLLAMA_MAX_QUEUE：如果在已经加载一个或多个模型的同时，没有足够的可用内存来加载新的模型请求，则所有新请求将排队，直到可以加载新型号为止。随着先前的模型闲置，将卸载一个或多个，以腾出空间为新型号腾出空间。排队的请求将按顺序处理；

+   OLLAMA_MAX_LOADED_MODELS：设置最大加载的模型数量，默认是 3 * GPU 的数量，如果超过这个数量，新的请求会被拒绝；

> 大家可以根据自己的需求，灵活设置参数组合，比如：

![](img/4fb9c49402d85a41d79bc429dc7f3c20.png)

```
OLLAMA_HOST=192.168.110.131:11434 CUDA_VISIBLE_DEVICES=0,1 OLLAMA_SCHED_SPREAD=1 OLLAMA_NUM_PARALLEL=10 ollama serve
```

#### 2.2 【方案二】离线下载模型权重

除了使用ollama run命令外，ollama也支持调用手动下载的自定义模型，但需要是GGUF格式。目前Qwen3模型已有各种不同量化版本的GGUF模型在魔搭社区和huggingface中上线了：https://modelscope.cn/models/lmstudio-community/Qwen3-14B-GGUF

![](img/519825c6ccc30e8a39bcb9a50e612fe9.png)

其中每个GGUF量化的权重都包含多个版本：

![](img/7ba2910f3816dd4b6daff6bd1ac33ff7.png)

其中ollama调用的就是Q4_K_M量化版本。这里我们还是考虑下载Q4_K_M量化版本的GGUF权重进行调用，具体执行流程如下。

+   下载GGUF格式权重

```
cd ~/autodl-tmp
mkdir Qwen3-14B-GGUF
modelscope download --model lmstudio-community/Qwen3-14B-GGUF --local_dir ./Qwen3-14B-GGUF
```

![](img/1e25a1a5b9d752b0f4fb22c8fa00ba70.png)

+   注册模型

然后需要创建一个file文件，用于进行ollama模型注册：

![](img/50fd7c44a0e07cbacf6a98442683cdab.png)

然后在File文件中写入自定义模型GGUF权重地址：

```
FROM ./qwen3-14b-q4_k_m.gguf
```

![](img/87deed6935a255e8a613ec3febbc1221.png)

然后，将该模型加入Ollama本地模型列表：

```
cd /home/08_vllm/Qwen3-14B-GGUF/
ollama create Qwen3-14B -f ModelFile
```

![](img/db2c7987b1f78efc330961430b81ea9a.png)

查看模型是否注册成功

```
ollama list
```

![](img/081f67d926f18cd9823262635f3ce6a0.png)

接下来即可在命令行中进行调用：

```
ollama run Qwen3-14B
```

![](img/dddd94ebebc6c56a019f2a10586860d6.png)

后续内容见Jupyter。