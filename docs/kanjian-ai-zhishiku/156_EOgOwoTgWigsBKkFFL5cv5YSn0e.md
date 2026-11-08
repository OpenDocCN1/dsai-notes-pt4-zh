# DEEP SEEK 本地部署（Ollama + ChatBox）+ 私有知识库（cherry studio）-第三方

> 来源：[https://jlqyxzzvbk.feishu.cn/docx/TvdOdj85voDjycxwvHrcuKK2nvm](https://jlqyxzzvbk.feishu.cn/docx/TvdOdj85voDjycxwvHrcuKK2nvm)

有问题**欢迎加🛰️交流，请备注来意。**[看见AI及个人介绍](https://jlqyxzzvbk.feishu.cn/wiki/L8anw4IEViiNEAkuN42csd9xn4b?from=from_copylink)

![](img/c7619dd119be202a70725831174f915c.png)

🛰️号：seeai01 或者 13858104569

### 1 软硬件需求

1.  1.1 硬件需求 DeepSeek R1 本地部署的硬件需求如下：

1.  **基本推荐配置：   **

CPU: 8核或以上，内存: 32GB或以上，存储: 100GB SSD或以上，GPU: NVIDIA GTX 1080或同等性能

DeepSe**ek-7B 对**标（gpt3.5）（ 4.7GB） 推荐配置：RTX3060 以上显卡 + 16G以上 内存 + 50G SSD 存储空间

**  4060:32B.**

**DeepSeek-70B** 对标（gp**t 4.0）（模**型大小：40GB）推荐配置：RTX4090 以上显卡 + 32G以上 内存 + 200G SSD 存储空间 。

满血版本：671B 版本 普通人基本装不了的。

## 2.1 Ollama 的下载

Ollama 是一个开源的本地模型运行工具，可以方便地下载和运行各种开源模型，如 Llama、Qwen、DeepSeek 等。Ollama 提供高效、灵活的大模型部署和管理解决方案，可以简化模型的部署流程。支持 macOS、Linux、Windows 操作系统。

官网地址：https://ollama.com/

下载地址：https://ollama.com/download

备份链接：https://github.com/ollama/ollama/releases/download/v0.5.7/OllamaSetup.exe

本教程选择 Ollama Windows 版本下载。

## 2.2 Ollama 的安装

下载完成后直接点击 OllamaSetup.exe 进行安装。

安装完成后，cmd 打开命令行窗口，输入 “ollama -v” 测试，显示 ollama 的版本为 “0.5.7”，表明安装成功。

## 2.3 Ollama 的运行

Ollama **没有用户界面**，在后台运行。

打开浏览器，输入 “http://localhost:11434/”，显示 “Ollama is running”。

1.  deepseek - r1 模型安装

## 3.1 Ollma 安装 deepseek - r1 模型

deepseek - r1 不同版本模型的执行命令不同：

+   1.5b 的命令为：“

```
ollama run deepseek-r1:1.5b
```

”

+   7b 的命令为：“

```
ollama run deepseek-r1:7b
```

”

+   14b 的命令为：“

```
ollama run deepseek-r1:14b
```

DeepSeek-R1-Distill-Qwen-32B

```
ollama run deepseek-r1:32b
```

DeepSeek-R1-Distill-Llama-70B

```
ollama run deepseek-r1:70b
```

推荐大家先下载较小的 1.5b 模型，把本教程的安装程序跑通，再根据自己的硬件配置和需求安装适当的版本。

1.  本教程选择选择 7b 模型，在命令行窗口运行安装命令 “ollama run deepseek - r1:7b”，程序会自动下载和安装运行。

默认地址是 “C:\Users% username%.ollama\models”。如果要修改模型的保存地址，可以新建一个文件夹如 “C:\Model”，然后在系统环境变量添加系统变量：“变量名：OLLAMA_MODELS，变量值：C:\Model”。

1.  安装完成后显示 “success”，表明安装成功。

## 3.2 使用 deepseek - r1 模型

1.  在命令行窗口运行命令 “ollama run deepseek - r1:7b”，就可以在提示行输入，与 deepseek - r1 进行聊天。

1.  输入 “/?”，可以获得帮助。

## 3.3 修改ollama安装地址。

1.  打开系统属性：右键点击Windows桌面上的“此电脑”或“计算机”图标，选择“属性”以打开系统属性窗口。

1.  进入环境变量设置：在系统属性窗口中，点击左侧的“高级系统设置”按钮。在弹出的“系统属性”窗口中，切换到“高级”选项卡，然后点击“环境变量”按钮。

1.  新建或修改环境变量：在“环境变量”窗口中，可以在“系统变量”或“用户变量”部分找到或新建一个名为OLLAMA_MODELS的环境变量。如果已存在该变量，则直接选中并点击“编辑”按钮；如果不存在，则点击“新建”按钮来创建一个新的环境变量。

1.  设置新的存储路径：在“编辑环境变量”或“新建环境变量”窗口中，输入变量名OLLAMA_MODELS，并在“变量值”文本框中输入希望保存模型文件的新目录路径。例如，如果希望将模型文件保存在D盘的OllamaModels目录下，则输入D:\OllamaModels。

1.  重启Ollama：设置完环境变量后，需要重启Ollama以应用更改。可以通过任务管理器结束Ollama的进程，然后重新运行Ollama来实现重启。

## 3.4 删除模型

1.  打开终端或命令行界面：首先，你需要打开你的终端或命令行界面。

1.  导航到存储模型的目录：确保你的当前工作目录是存储模型的文件夹。

1.  使用命令删除模型：输入命令 ollama rm <model_name>，其中 <model_name> 是你要删除的模型的名称。确认删除操作，根据提示输入确认信息，然后等待删除完成。

比如 ：ollama rmdeepseek - r1:7b

注意事项：

+   备份重要数据：在删除模型之前，请确保你已经备份好了需要保留的数据。删除模型是一个不可逆的操作，一旦删除将无法恢复。

+   版本差异：具体的命令和操作可能会因为OLLAMA的版本而有所不同，建议参考OLLAMA的官方文档或者使用 ollama --help 命令获取更多帮助信息。

通过以上步骤，你可以在OLLAMA中成功删除不需要的模型。

## 4.1 使用CHAT_BOX 安装

1.  申请API：目前Deep Seek 正在遭受国外黑客攻击，所以推荐使用 硅基流动 的API，价格跟官方一致。

1.  点击https://cloud.siliconflow.cn/i/SR669UVC

1.  申请账户登录后，再左侧菜单栏找到API 秘钥。

![](img/7f4fbc90dd3307cfab4a7e903834695f.png)

1.  右上角点击生成

![](img/c43f8c1fff20c97ac3136ee3eaf14a2c.png)

1.  点击秘钥进行复制：

![](img/3bfd5c16575a8fdf5be7da898c492ad8.png)

## 4.2 使用CHATBOX

1.  登录官方页面：https://chatboxai.app/zh

1.  点击运行网页版（使用桌面版也一样）

1.  点击左下角 setting。

1.  如图所示： Model Provider ：OPENAI API

API key ： 填写复制的 API秘钥

API Host ：填写网址： https://api.siliconflow.cn/v1

第一步：点击 **Model&Token  第二步： m**odel选择 Custom Model 。 再 Custom

![](img/74272c88db03c71cd7e31f4c7c7dba7f.png)

### 拉倒最下面，选择 custom model （或者 自定义模型）

Model name 输入：deepseek-ai/DeepSeek-R1

![](img/824d72d03204e51ae5ed09443c07e985.png)

1.  开始聊天。点击左下角：new chat 。 进行聊天即可。

## 4.3 CHAT汉化

1.  左下角 Settings

1.  选择  Display

1.  第一个 选择中文。

# 5 设置DeepSeek R1部署和本地私有知识库

## 5.1 安装cherry studio

1.  通过Chat_box 安装的朋友们可以通过设置本地知识库的方式，实现个人智能体助手

1.  下载 cherry_studio（功能和chatbox一致，但更加强大，可以代替chatbox）

1.  下载地址：https://cherry-ai.com/download。选择对应的系统进行下载。

1.  如果需要选择其他系统：

![](img/a44eb0adf94d7688565dd7d3c4ecdd79.png)

1.  本地安装

1.  申请API：目前Deep Seek 正在遭受国外黑客攻击，所以推荐使用 硅基流动 的API，价格跟官方一致。

1.  点击https://cloud.siliconflow.cn/i/PQjf5Pqp

1.  申请账户登录后，再左侧菜单栏找到API 秘钥。

![](img/64fa0943f09176d8bde3c0db7f18bd56.png)

1.  右上角点击生成

![](img/80d70f2441eab9f3067ed9a7579ec1e4.png)

1.  点击秘钥进行复制：

![](img/9019a935d646d6d10dccffcf3529680c.png)

## 5.2 设置API接口（获取API的方法，在4.1章）

1.  打开设置界面：

![](img/ad59597d2bc1f8c98b879e3882495b3f.png)

1.  选择硅基服务，并填写API

![](img/467e7bf894ac246a4c3131767e6da797.png)

1.  添加模型

![](img/5dc38a0bb7126493ddf52c372d834310.png)

![](img/8e6416b67555d941fb4b97a77d42aad6.png)

1.  添加嵌入模型

![](img/2bc1e5b25d14606de15c8a116d7bd53b.png)

1.  聊天测试是否安装成功

![](img/6597ddfc99972ed8d8d09c30a9233010.png)

1.  添加知识库

![](img/36eccfb284d90607338a6faec4678faf.png)

![](img/126cc2f3007873d6180176b08dec94d9.png)

![](img/184cf2f64f6eb9da78b62d606c135b06.png)

![](img/88a09db057243376aaaed86e4f768146.png)

1.  确认知识库安装成功，并尝试搜索

![](img/39e60ecf384122a66f44dcf7c97b9d71.png)

![](img/5f7efdbe2b5dd315a97246cff522a5d7.png)

1.  聊天中设置知识库

![](img/ca9af94478d4484a9d48be32bd09e938.png)

1.  设置完成。

## 注意：知识库所使用的token较多，请结合自身情况进行使用。

## 5.3 基于ollama实现本地知识库，待完成……