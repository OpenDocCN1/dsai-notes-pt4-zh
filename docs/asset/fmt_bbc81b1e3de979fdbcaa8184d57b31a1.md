
## 本地模型下载与运行

```
from modelscope import AutoModelForCausalLM, AutoTokenizer

model_name = "qwen/Qwen2.5-7B-Instruct"

model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype="auto",
    device_map="auto"
)
tokenizer = AutoTokenizer.from_pretrained(model_name)

prompt = "Give me a short introduction to large language model."
messages = [
    {"role": "system", "content": "You are Qwen, created by Alibaba Cloud. You are a helpful assistant."},
    {"role": "user", "content": prompt}
]
text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True
)
model_inputs = tokenizer([text], return_tensors="pt").to(model.device)

generated_ids = model.generate(
    **model_inputs,
    max_new_tokens=512
)
generated_ids = [
    output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
]

response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
print(response)
```

编辑好脚本之后通过`python download.py`开始执行这个文件，如下图所示开始自动下载大模型启动所需的项目文件与权重文件以及分词器文件。

![](img/a3855b4bb6743e041780e6993d8f46cf.png)

文件安装完毕后会启动对话测试（再次运行该文件会直接加载本地权重文件后直接进行运行推理），出现以下文本返回信息说明文件下载完整且可以启动：

![](img/2c6077ff2130dbbd4a37f7dbad84ef39.png)

通过后台监控可以看到推理该对话占用将近15G的显存：

![](img/880f8cdddc503082433b9f5551fa9cd1.png)

文件完整性可以通过以下文档进行检查：

![](img/9093ee09c6644c7352b0b8085c1e5bed.png)

### Step 6.2 从Model Scope下载GLM4-9b-chat模型权重

Model Scope下载路径如下：

`https://www.modelscope.cn/models/Qwen/Qwen2.5-7B-Instruct`

Model Scope支持多种下载模式：SDK下载、Git下载、命令行下载（下载完整模型、下载单个文件后直接加文件名即可）、手动下载

![](img/066c508abfd5d8d063215a82ad83c20a.png)

这是一种快捷的本机下载方式，首先通过命令`pip install modelscope`安装好达摩社区的安装包，然后通过以下指令可以下载模型的权重文件，这种方法无需🪜，无需下载额外的插件。

```
modelscope download --model Qwen/Qwen2.5-7B-Instruct
```

![](img/df5ffc19a64c6e9e5a6f0381122ab01b.png)

这种命令可以支持下载单个的文件，通过以下指令可以单独下载某文件,用于补充或更新(这里用`README.md`文件为例)：

```
modelscope download --model Qwen/Qwen2.5-7B-Instruct README.md --local_dir ./dir
```

下载完成后全部的权重文件的内容如下：

![](img/5238fad67a631f5842cf10bc01b75c37.png)

### Step 6.3 手动方式下载

在Model Scope中也可以实现手动下载，虽然相较于命令行下载的方法较繁琐，但是特殊情况下的有效安装方法。在模型下载页面可以通过手动点击文件对应的下载按钮即可实现下载。

![](img/a668f7a6ac93ef8a13b248211ee750c8.png)

下载好的权重文件和命令行下载方式的一样，不过一般如果是在主机-服务器环境下使用这种方式下载的话需要`xftp`这样的传输工具在上传到服务器使用。

![](img/d8c9f41b498c3c4af99dd2f0a0c588e5.png)

# 3. ModelScope线上部署办法

## ModelScope在线算力与在线环境获取指南

登录魔搭社区：`https://www.modelscope.cn/home`，点击注册：

![](img/970b7349aeb05b69e81c0314d94117b0.png)

输入账号密码完成注册：

![](img/98b76887e19ec983f0ae2ce9a1e4be5c.png)

注册完成后，点击个人中心，点击绑定阿里云账号：

在跳转页面中选择登录阿里云，未注册阿里云也可以在当前页面注册：

![](img/1fa74bfea66a64a42d1f72e2c308781c.png)

绑定完成后，点击左侧“我的Notebook”，即可查看当前账号获赠算力情况。对于首次绑定阿里云账号的用户，都会赠送永久免费的CPU环境（8核32G内存）和36小时限时使用的GPU算力（32G内存+24G显存）。这里的GPU算力会根据实际使用情况扣除剩余时间，总共36小时的使用时间完全足够进行前期各项实验。

![](img/db9eecc3dc5d55b14919c65dda8f865c.png)

接下来启动GPU在线算力环境，选择方式二、点击启动：

![](img/f42de01232a242b889262674f373564b.png)

稍等片刻即可完成启动，并点击查看Notebook：

![](img/096c4f56774fc65142a64bdd20f135e5.png)

即可接入在线NoteBook编程环境：

当前NoteBook编程环境和Colab类似（谷歌提供的在线编程环境），可以直接调用在线算力来完成编程工作，并且由于该服务由ModelScope提供，因此当前NoteBook已经完成了CUDA、PyTorch、Tensorflow环境配置，并且已经预安装了大模型部署所需各种库，如Transformer库、vLLM库、modelscope库等，并且当前NoteBook运行环境是Ubuntu操作系统，我们可以通过Jupyter中的Terminal功能对Ubuntu系统进行操作：

进入到命令行界面：

![](img/e62579bbac0f05944d7b5cd9acdfce8b.png)

通过`pip show +name`的方式可以查看ModelScope已经安装好的依赖资源，可以看到已经安装好了我们所需的各种依赖资源。

![](img/31c3097dc0970fca9bb56be30852e6f9.png)

输入`nvidia-smi`，查看当前GPU情况：

![](img/6adb936165fc7c24c82b04822ffc8cca.png)

继续在命令行中进行操作，和本地下载流程一样，在创建部署新文件之前我们先为之创建一个新文件夹，随后进入该文件夹中进行下载操作。

```
mkdir qwen2-5
cd qwen2-5
modelscope download --model Qwen/Qwen2.5-7B-Instruct
```

![](img/26c3de7c6be33ed1ed6aec02c2264516.png)

下载完成之后，其完整的文件信息也如下图所示：

![](img/49d1cfc87e7d164c795f9d04e06d485e.png)

当然，这里需要注意的是，哪怕当前在线编程环境已经做了适配，但并不一定满足所有ModelScope中模型运行要求，既并非每个拉取的Jupyter文件都可以直接运行。不过无论如何，ModelScope Notebook还是为初学者提供了非常友好的、零基础即可入手尝试部署大模型的绝佳实践环境。

# 4. Ollama框架部署流程

## 4.1 Ollama基本信息介绍

Ollama 是一个开源的大语言模型服务工具，专注于简化本地模型的创建、管理和部署流程。它可以帮助开发人员和数据科学家轻松地在本地或私有环境中使用大语言模型，而不必依赖于云服务，从而保证了数据隐私和灵活性。

Ollama 非常适合需要在本地运行大语言模型的开发者和企业，如：

+ 开发和测试：在本地快速创建和测试新的语言模型。
+ 隐私保护：在本地部署模型，适用于有严格隐私需求的企业。
+ 多模型管理：轻松管理和部署多个模型，适合有大量模型管理需求的团队。

在官网可以看到Ollama支持的模型列表 `https://ollama.com/library`

![](img/f84f9f32a3e63f4a5532f5d395784c8c.png)

每个模型下面有支持的功能和参数型号以及基本的模型描述，点击进入对应模型可以看到下载所需占用的内存大小。

![](img/00a451ef4a117bc7da0bbeda21d9dce3.png)

## 4.2 使用Ollama实现LLM下载流程

使用Ollama方法下载的前两步骤与正常方法下载相同（本地部署流程的前五步），即创建虚拟环境和依据硬件配置Pytorch环境，部署好流程后可以继续下面的步骤。

Ollama安装硬件要求：

+ Windows：3060以上显卡+8G以上显存+16G内存，硬盘空间至少20G显卡
+ Mac：M1或M2芯片 16G内存，20G以上硬盘空间

下载Ollama的指令如下：

```
curl -fsSL https://ollama.com/install.sh | sh
```

![](img/72901839077d867ae130984fb662bdbc.png)

下载完成后检测,如果返回版本号则说明成功下载：

```
ollama -v
```

![](img/b87038e712a524ddf2751d5a6c8acd97.png)

通过指令`ollama help`可以查看该系统可执行的命令：

![](img/45607439812bfd44b8c1da21f56d58cd.png)

通过以下指令可以，检查ollama可运行的模型列表，可以看见之前下载的Llama 3-8b模型的信息呈现在列：

```
ollama list
```

![](img/a537747e612cf220f76ca39115ff90e1.png)

**下载模型**

在终端中执行命令`ollama run qwen2.5`或`ollama run qwen2.5:7b`，即可下载 `qwen2.5:7b` 模型。模型下载完成后，会自动启动大模型，进入命令行交互模式，直接输入指令，就可以和模型进行对话了对应参数的模型的下载方式可以通过在Ollama官网查看到下载指令。

> 以下是进行qwen2.5:72b模型进行下载&运行测试。

![](img/d7046167ff81fc14ea6a841988b5a1a3.png)

下载70B的模型只需要一行指令即可完成部署：

```
ollama run qwen2.5:72b
```

![](img/95525cb0bade361927b7114d6f4f0b00.png)

完成下载后会直接进入模型启动状态，如果退出或刷新界面，再次输入指令`ollama run qwen2.5:72b`即可启动对应模型。

可以看到推理所需的计算资源相对均匀的分布在每张工作显卡上。

![](img/b6f8b0e5b57d51bae1d94a40007d8aef.png)

对该模型进行数学能力测试后可以看到，它无论在应对简单的数学陷阱题，还是中等难度的数学问题时，都能作出准确且逻辑清晰的回答。这表明该模型的指令微调（instruct）效果显著，具备良好的数学理解和推理能力。

![](img/016ad899141d601ce93979650dfb3258.png)

> 使用 `/bye` 退出正在进行的 Ollama进程。

## 4.3 如何卸载安装

直接在你的安装目录下，删除ollama文件夹即可。所有下载的数据和大模型文件都在里面，Ollama 的默认安装目录通常是用户主目录下的 `.ollama` 文件夹。例如，在 Unix 系统（如 macOS 和 Linux）上，默认安装目录为：

`~/.ollama`

或可以通过指令 `ollama rm modlename`的方式来移除对应的大模型文件。

# 5. 在Windows环境下进行Ollama方式下载

Ollama同样支持在Windows环境下进行部署下载，使用此方法下载意味着你可以无需调整任何硬件设备直接在你的主机上实现Llama 3.2模型的安装部署和推理使用。

首先要下载Windows版的Ollama文件，进入官网选择下图的内容进行下载：

`https://ollama.com/download`

![](img/ba9cce302842ee94b5416937848e00e5.png)

点击Download for window即可实现下载安装包，文件大小为664MB。

![](img/463f461506ab1887a80f425c57c14906.png)

下载完成后在对应文件夹双击打开安装包完成安装流程。

![](img/15f54a7cb3142610b38024e30afbf487.png)

![](img/54f1b58c7156d4bb3bb0af642bf96df8.png)

框架和模型默认路径都是C盘，为了主机内存管理方便，可以通过以下步骤来实现模型下载地址的切换。

1.在桌面下方搜索栏输入高级系统设置并进入。

![](img/fd7003ad197718fde148f4f02b2af704.png)

在系统设置中找到高级的标签，点击下方的环境变量选项。

![](img/dbcfd01907498cdaa0ddda73fd1a7633.png)

在系统变量栏点击新建，变量名设置为`OLLAMA_MODELS`,变量值设置为你想要的路径位置，这里设置为`D:\Ollama\models`

![](img/17a8b4a54ea0d4b98fe88aca451ab229.png)

设置好路径之后可以在命令行中进行检查，通过以下命令实现：

![](img/967f5dbd290907e15f78bca32b4172ec.png)

在完成Ollama部署之后，系统会自动启动ollama的程序，界面如下：

![](img/7bd46e4a90fd9391fd4436379e68833a.png)

在Ollama官网支持的模型界面可以找到对应可推理的模型的部署代码，以下以Qwen2.5：0.5B为例进行演示。

![](img/d820e982bb5c034a6984cd23bceb28d9.png)

在Windows平台运行和在Linux系统中进行的指令一样，执行`ollama run qwen2.5:0.5b`即可实现该模型的下载和运行。

同样的，在完成模型下载之后会直接开启模型的推理任务，可以通过命令行的方式进行模型对话交互。

![](img/b0c6966f58c9be30bde2ac0bd6f65b64.png)

再次启动Ollama可以直接在Windows的命令行中实现，通过win+R键启动`cmd`命令，执行`ollama run qwen2.5:0.5b`便可以开启对话，实例所演示的是qwen2.5系列最小的模型，通过测试仅在资源有限的纯CPU环境下也可以流畅运行，这展现出未来AIPC的一种可能方向。

# 6. 使用vLLM框架进行推理部署

首先来看一下什么是vLLM。

vLLM 是一个用于大型语言模型推理和服务的快速且易于使用的库，简单理解就是一个大模型的推理框架。这是一个用于快速 LLM 推理和服务的开源库。它提供的吞吐量比 HuggingFace Transformers 高 24 倍，而无需更改任何模型架构。vLLM在延迟方面也有显著优势。

GLM4的官方集成了vLLM开源框架来实现部署和加速推理。

vLLM论文地址：`https://arxiv.org/pdf/2309.06180`

vLLM官方地址：`https://docs.vllm.ai/en/latest/index.html`

vLLM支持模型列表

`https://docs.vllm.ai/en/latest/models/supported_models.html`

![](img/42e5d4baae00eac073a826407c4bf9a5.png)

vLLM有几个优化点值得我们去关注： 其一是PagedAttention 高效管理注意力键值内存，其二是vLLM支持多GPU和多节点推理，适用于大规模部署场景。其三，vLLM支持多种量化方案，如GPTQ、AWQ和FP8，优化了CUDA内核以提高推理效率。最后，它还可以为大语言模型构建API服务器。

## 6.1 vLLM安装

![](img/d740c1baf648f92663d6f8da1384241b.png)
