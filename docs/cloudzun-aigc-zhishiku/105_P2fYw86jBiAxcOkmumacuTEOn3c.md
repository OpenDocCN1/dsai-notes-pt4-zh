# 📘 模型本地化部署实操手册

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/LZ2zdvkCzoeO4QxT6S7ciB2jn4c](https://zsyhjtnsa5.feishu.cn/docx/LZ2zdvkCzoeO4QxT6S7ciB2jn4c)

**—— 适配 Ollama （Linux/WSL） 与** LM Studio （Windows）

## 1. 方案 A：使用 Ollama 部署 （推荐 - 命令行/API）

**适用环境：** Linux， WSL （Windows Subsystem for Linux）， Mac

**特点：** 轻量、支持 API 调用、适合开发者。

### 1.1 安装 Ollama

如果在当前环境中尚未安装 Ollama，请执行以下命令一键安装：

```
curl -fsSL https://ollama.com/install.sh | sh
```

+   验证安装： 运行 ollama --version。

### 1.2 创建模型配置文件 （Modelfile）

Ollama 需要一个配置文件来定义模型路径和对话模板。

1.  **新建文件：** 在终端中创建一个名为 Modelfile 的文件。

```
cd ~
nano Modelfile
```

1.  **写入内容：** 将以下内容粘贴进去（请修改 FROM 路径为你实际的 GGUF 文件路径）：

```
# [关键] 这里填写上一节量化生成的 GGUF 绝对路径
FROM /home/chengzh/LLaMA-Factory/deepseek_merged/deepseek-r1-1.5b-q4_k_m.gguf

# [关键] 对话模板设置
# DeepSeek-R1-Distill-Qwen 基于 Qwen 架构，必须使用 ChatML 格式，否则会乱码
TEMPLATE &quot;&quot;&quot;{{ if .System }}&lt;|im_start|&gt;system
{{ .System }}&lt;|im_end|&gt;
{{ end }}{{ if .Prompt }}&lt;|im_start|&gt;user
{{ .Prompt }}&lt;|im_end|&gt;
{{ end }}&lt;|im_start|&gt;assistant
&quot;&quot;&quot;

# 设置停止符，防止模型自言自语
PARAMETER stop &quot;&lt;|im_start|&gt;&quot;
PARAMETER stop &quot;&lt;|im_end|&gt;&quot;

# 设置系统提示词
SYSTEM &quot;你是一个乐于助人的AI助手。&quot;
```

1.  **保存退出：** 按 Ctrl + O 保存，按 Ctrl + X 退出。

### 1.3 构建并运行模型

1.  **构建模型：** 将配置导入 Ollama 数据库，命名为 my-deepseek。

```
ollama create my-deepseek -f Modelfile
```

1.  **运行对话：**

```
ollama run my-deepseek
```

+   测试： 输入“你好，你是谁？”，观察其是否使用了微调后的身份回答。

---

## 2. 方案 B：使用 LM Studio 部署 （图形化界面）

**适用环境：** Windows， Mac

**特点：** 界面友好、拖拽可用、适合普通用户演示。

### 2.1 准备工作：获取模型文件

由于 LM Studio 运行在 Windows 宿主机上，你需要访问 WSL 或 Linux 中的文件。

+   **访问方法：** 打开 Windows 文件资源管理器，在地址栏输入 \\wsl$，进入对应的 Linux 发行版文件夹，找到刚才生成的 .gguf 文件。

### 2.2 模型存放路径规范 （推荐）

虽然 LM Studio 支持任意路径加载，但为了规范管理，建议将模型放入其默认扫描目录。

1.  **创建目录结构：**

在 Windows 中找到以下路径（如果不存在则手动创建）：

C:\Users\[你的Windows用户名]\.cache\lm-studio\models\

1.  **新建模型文件夹：**

在 models 目录下新建一个文件夹，例如 my-deepseek，再创建一个子目录 \DeepSeek-R1-Distill-Qwen-1.5B-GGUF

1.  **放入文件：**

将 .gguf 文件复制进去。最终路径结构应如下所示：

```
C:\Users\cheng\.cache\lm-studio\models\my-deepseek\DeepSeek-R1-Distill-Qwen-1.5B-GGUF\deepseek-r1-1.5b-q4_k_m.gguf
```

### 2.3 加载与配置 （关键步骤）

1.  打开 **LM Studio**，点击左侧 **"AI Chat"** 图标。

1.  在顶部下拉菜单 "Select a model to load" 中，选择刚才放入的 my-deepseek。

1.  配置 Preset （对话模板）：

+   **非常重要：**** 在右侧设置栏 **（Settings） 中，找到 "Preset" 选项。

+   **必须选择：** **Qwen 2.5** 或 **ChatML**。

+   原因： 选错模板（如默认的 Llama 3）会导致模型输出乱码或一直在重复说话。

1.  **开始对话：** 在底部输入框发送测试消息。

---

## 3. 附录：Ollama API 调用示例 （Python）

部署在 Ollama 的模型会自动开启 11434 端口，支持 OpenAI 兼容格式调用。

```
from openai import OpenAI

# 连接本地 Ollama 服务
client = OpenAI(
    base_url=&#x27;http://localhost:11434/v1&#x27;,
    api_key=&#x27;ollama&#x27;, # 占位符，无需修改
)

response = client.chat.completions.create(
    model=&quot;my-deepseek&quot;,
    messages=[{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请把&#x27;人工智能&#x27;写成一首藏头诗。&quot;}]
)

print(response.choices[0].message.content)
```

这是为您整理的 **第 7 节：拓展实验**。

**这一部**分内容旨在让学生体验 AI 的组合拳能力。通过引入视觉模型和嵌入模型，演示如何让单一的对话模型进化为“能看、能记、能聊”的全能助手。

请将以下内容添加到实验手册的最后。

---

## 7. 拓展实验：构建多模态与 RAG 基础

**目标：** 在同一个 Ollama 服务中同时运行多个不同功能的模型，体验多模态（视觉）与嵌入（向量化）能力。

### 7.1 赋予 AI “眼睛” （多模态视觉模型）

我们要加载一个轻量级的视觉模型，让 AI 能看懂图片。

+   **选用模型：** **moondream** （1.6B）

+   理由： 体积极小（约 800MB），速度飞快，非常适合与我们的微调模型搭配运行。

**操作步骤：**

1.  **下载模型：**

在终端运行：

```
ollama pull moondream
```

1.  **准备测试图片：**

下载一张测试图片（例如一只猫）到当前目录：

```
wget -O test_img.jpg https://img0.baidu.com/it/u=1371527764,1362753235&amp;fm=253&amp;fmt=auto&amp;app=138&amp;f=JPEG
```

1.  **运行与测试：**

启动模型：

```
ollama run moondream
```

在对话框 >>> 中直接输入文字和图片路径：

```
Describe this image ./test_img.jpg
```

+   **预期结果：** 模型会用英文描述图片内容（例如 "A cute cat looking at...") .

---

### 7.2 赋予 AI “记忆索引” （嵌入模型）

嵌入模型用于将文字转换为计算机能理解的“数字向量”，是构建 RAG（知识库）的核心组件。

+   **选用模型：** nomic-embed-text

+   理由： 性能强悍，上下文长，且体积仅 270MB。

**操作步骤：**

1.  **下载模型：**

```
ollama pull nomic-embed-text
```

1.  **测试向量化 （API 方式）：**

不进入对话界面，直接用命令测试其功能：

```
curl http://localhost:11434/api/embeddings -d &#x27;{
  &quot;model&quot;: &quot;nomic-embed-text&quot;,
  &quot;prompt&quot;: &quot;DeepSeek大模型&quot;
}&#x27;
```

+   **预期结果：** 屏幕上会打印出一长串浮点数 [0.123, -0.987, ...]。这说明模型已成功将文字转化为向量。

---

### 7.3 综合实战：多模型协同工作 （Python）

**场景：**** 这是一**个高级演示。我们将编写一个 P**ytho**n 脚本，让 视觉模型 看图，然后把描述传给我们的 微调模型 进行中文润色和创作。

**1. 安装 Python 库**

```
pip install ollama
```

**2. 创建脚本 ****multi_model.py**

新建文件并粘贴以下代码：

```
import ollama

# 定义图片路径
img_path = &#x27;test_img.jpg&#x27;

print(f&quot;👁️  正在调用 Moondream 模型观看图片...&quot;)
# 1. 让视觉模型看图
vision_res = ollama.chat(
    model=&#x27;moondream&#x27;,
    messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;Describe this image&#x27;, &#x27;images&#x27;: [img_path]}]
)
desc = vision_res[&#x27;message&#x27;][&#x27;content&#x27;]
print(f&quot;--&gt; 英文描述: {desc}\n&quot;)

print(f&quot;🧠 正在调用微调模型 (My-DeepSeek) 进行创作...&quot;)
# 2. 让微调模型根据描述写诗
chat_res = ollama.chat(
    model=&#x27;my-deepseek&#x27;, # 刚才微调并导入的模型
    messages=[{&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: f&quot;请把这段英文描述翻译成中文，并以此为灵感写一首简短的诗：{desc}&quot;}]
)
print(f&quot;--&gt; 回答结果:\n{chat_res[&#x27;message&#x27;][&#x27;content&#x27;]}&quot;)
```

**3. 运行脚本**

```
python multi_model.py
```

+   **预期效果：**

1.  控制台显示 Moondream 识别出了图片中的猫。

1.  随后显示你的 DeepSeek 微调模型将描述翻译并写成了一首关于猫的诗。

> **👨‍🏫 教学总结：**

> 通过这个实验，我们证明了 Ollama 可以轻松地在显存中同时加载多个专精模型（视觉、推理、嵌入）。未来的 AI 应用开发，正是像这样将不同模型像积木一样组合使用的过程。

这是一份为你整理的Ollama核心功能快速入门手册，涵盖从安装到进阶应用的核心步骤和常用命令，方便你快速上手。

**为了让学**习路**径更清晰**，可以**参照下面**的图示，从安装起步，到核心交互，再到进阶集成：

```
flowchart TD
    A[Ollama入门三步走] --&gt; B[第一步：安装与验证]
    A --&gt; C[第二步：模型管理与交互]
    A --&gt; D[第三步：进阶集成与应用]

    B --&gt; B1[系统要求：主流操作系统&lt;br&gt;内存8GB+，磁盘20GB+]
    B --&gt; B2[安装命令&lt;br&gt;官网下载安装包或脚本]
    B --&gt; B3[验证安装&lt;br&gt;`ollama --version`]

    C --&gt; C1[探索与拉取模型&lt;br&gt;`ollama list`&lt;br&gt;`ollama pull &lt;模型名&gt;`]
    C --&gt; C2[运行与对话&lt;br&gt;`ollama run &lt;模型名&gt;`&lt;br&gt;支持交互式对话与参数调整]
    C --&gt; C3[基础管理命令&lt;br&gt;`ollama ps`&lt;br&gt;`ollama rm`&lt;br&gt;`ollama cp`等]

    D --&gt; D1[自定义模型&lt;br&gt;创建Modelfile文件&lt;br&gt;`ollama create`]
    D --&gt; D2[API服务与集成&lt;br&gt;`ollama serve`&lt;br&gt;提供REST API用于编程调用]
    D --&gt; D3[图形界面&lt;br&gt;可选安装Open WebUI等第三方GUI]
```

## Ollama 基本命令

### 📥 第一步：安装与验证

1.  **获取Ollama**：前往 [Ollama官网](https://ollama.com/) 下载对应系统（Windows/macOS/Linux）的安装包。

1.  **系统要求**：主流操作系统，建议至少**8GB内存**和**20GB可用磁盘空间**（用于存放模型）。

1.  **验证安装**：打开终端（命令提示符），输入 ollama --version，能显示版本号即安装成功。

### 🤖 第二步：模型管理与交互

Ollama的核心操作通过命令行完成，以下是常用命令的速查表：

| 任务 | 命令 | 说明与示例 |
| --- | --- | --- |
| **拉取模型** | ollama pull &lt;模型名&gt; | 从仓库下载模型，如 ollama pull llama3。 |
| **运行模型** | ollama run &lt;模型名&gt; | 启动模型进入交互对话，如 ollama run qwen3-vl。按 Ctrl+D 退出。 |
| **列出本地模型** | ollama list | 查看所有已下载的模型。 |
| **查看模型信息** | ollama show &lt;模型名&gt; | 显示指定模型的详细参数。 |
| **停止模型** | ollama stop &lt;模型名&gt; | 停止正在运行的模型实例。 |
| **删除模型** | ollama rm &lt;模型名&gt; | 删除本地模型以释放空间，如 ollama rm llama3。 |
| **复制模型** | ollama cp &lt;源模型&gt; &lt;新模型&gt; | 创建模型副本。 |

**关于你关心的模型参数**：如之前讨论，直接运行 ollama run** qwen3-vl**，Ollama会为你自动下载其默认版本（通常是社区预设的8B参数Q4量化版）。若要指定特定版本，需要使用完整标签，如 ollama run qwen3-vl:8b-q4_K_M。

**运行参数调整**：可以在运行时加入参数控制模型行为，例如 ollama run llama3 --temperature 0.7 --max-tokens 200。

### ⚙️ 第三步：进阶集成与应用

1.  **自定义模型**：通过创建**Modelfile**文件，可以基于现有模型定制系统提示词、调整参数，然后用 ollama create <自定义名> -f ./Modelfile 构建专属模型。

1.  **启用API服务**：运行 ollama serve 启动本地API服务（默认端口11434），便可通过HTTP请求与模型交互，方便集成到你的应用程序中。

+   **Python调用示例**：

```
import requests
response = requests.post(&#x27;http://localhost:11434/api/generate&#x27;, json={
  &quot;model&quot;: &quot;llama3&quot;,
  &quot;prompt&quot;: &quot;你好，请介绍一下你自己。&quot;,
  &quot;stream&quot;: False
})
print(response.json()[&#x27;response&#x27;])
```

1.  **使用图形界面****：如果更喜欢可视化操**作，可以安装第三方Web UI（如 Open WebUI）来管理模型和对话。

1.  **其他实用命令**：

+   ollama help：查看所有命令帮助。

+   ollama ps：查看当前正在运行的模型。

### ⚠️ 重要安全提示

**Ollama**默认的API服务（端口11434）没有访问控制。如果部署在对公网开放的服务上，务必通过防火墙、反向代理或修改配置来限制访问，防止被未授权调用。

希望这份手册能帮助你快速开始。如果你在安装某个特定模型或尝试API集成时遇到具体问题，可以随时提出，我能提供更细致的解答。

以下是安装Open WebUI（原名Oll**ama WebU**I）的详细方法，推荐使用Docker方式安装，这是最简单且能避免环境依赖冲突的方式。

## Open WebUI 的安装

### 📦 推荐方式：使用Docker安装（最简单）

这是官方推荐且最适合大多数用户的方法，能一键完成所有依赖配置。

1.  **确保环境就绪**

+   **安装Docker**[：请确保你的系统（Windo](https://www.docker.com/products/docker-desktop/)ws/macOS/Linux）已安装 Docker Desktop 并已启动。

+   **安装Ollama**[：Open We](https://ollama.com/)bUI需要后端Ollama服务，请确保已 安装Ollama 并能正常运行模型（如 ollama run llama3）。

1.  一条命令启动Open WebUI

打开终端（命令提示符/PowerShell/Terminal），执行以下命令：

```
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

**命令参数说明**：

+   -p 3000:8080**: 将容器内**的8080端口映射到你本机的3000端口（之后通过 http://localhost:3000 访问）。

+   --add-host=...: 让容器内能访问到你主机上运行的Ollama服务（默认端口11434）。

+   -v open-webui:/app/backend/data: 将数据持久化存储在名为 open-webui 的Docker卷中，防止重启后数据丢失。

+   --restart always: 设置容器随Docker自动启动。

1.  **访问与登录**

+   打开浏览器，访问 http://localhost:3000。

+   首次进入会要求**创建管理员账户**，按提示填写邮箱和密码即可。

### 🛠️ 备用方式：从源码安装（适合开发者）

如果你想使用最新开发版或进行代码定制，可以采用此方式。

1.  **克隆项目与安装依赖**

```
# 1. 克隆仓库
git clone https://github.com/open-webui/open-webui.git
cd open-webui

# 2. 安装后端依赖 (确保已安装Python 3.11+)
cd backend
pip install -r requirements.txt

# 3. 安装前端依赖 (确保已安装Node.js 18+)
cd ../frontend
npm ci
```

1.  **配置与启动**

+   在 backend 目录下，复制环境变量文件并修改：

```
cp .env.example .env
# 编辑 .env 文件，确保 OLLAMA_API_BASE_URL 指向你的Ollama服务地址（通常是 http://localhost:11434）
```

+   **启动后端服务**（在 backend 目录）：

```
python main.py
```

+   **启动前端服务**（在 frontend 目录，新开终端）：

```
npm run dev
```

+   访问 http://localhost:8080 即可。

### 🔧 常见安装问题与解决

| 问题 | 可能原因与解决方案 |
| --- | --- |
| **访问 ****localhost:3000**** 无法连接** | 1. 检查Docker容器是否运行：docker ps。&lt;br&gt;2. 确认端口是否被占用，可尝试将命令中的 -p 3000:8080 改为 -p 3080:8080，然后访问 http://localhost:3080。 |
| WebUI中无法连接Ollama | 1. 确保Ollama服务正在运行：oll**ama lis**t。&lt;br&gt;2. 对于Linux系统，Docker命令可能需要将 --add-host=host.docker.internal:host-gateway 替换为 --network=host，并使用 http://localhost:11434 作为Ollama地址。 |
| **如何更新Open WebUI？** | 使用Docker方式更新非常简单：&lt;br&gt;1. 停止并删除旧容器：docker stop open-webui &amp;&amp; docker rm open-webui。&lt;br&gt;2. 拉取最新镜像并重新运行上述 docker run 命令（数据因 -v 参数会保留）。 |
| **如何自定义配置？** | [可以创建自定义配置文件。详](https://docs.openwebui.com/configuration)细选项请参考官方文档的 Configuration 部分。 |

### 🚀 安装完成后的第一步

登录后，建议进行以下操作：

1.  **连接模型**：在WebUI设置中，确认“Ollama API Base URL”正确（通常是 http://host.docker.internal:11434 或 http://localhost:11434）。

1.  **下载模型**：在WebUI的“模型”页面，可以直接搜索并下载新模型（如 llama3、qwen3-vl），这比命令行更直观。

1.  **开始聊天**：在“聊天”页面选择模型，即可开始对话。Open WebUI支持多模态（上传图片）、对话历史管理、角色预设等丰富功能。

希望这份指南能帮助你顺利安装Open WebUI。如果在安装过程中遇到任何具体报错信息，可以随时提出，我能提供更针对性的解决方案。