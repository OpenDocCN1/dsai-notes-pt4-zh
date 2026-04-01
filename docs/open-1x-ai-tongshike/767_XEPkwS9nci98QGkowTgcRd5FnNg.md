# 微学习01 | DeepSeek+Ollama+RAGFlow：实现本地知识库检索

> 来源：[https://datawhaler.feishu.cn/docx/Ny2hdH4XboZx2rxLhiZcBSKrnWd](https://datawhaler.feishu.cn/docx/Ny2hdH4XboZx2rxLhiZcBSKrnWd)

# 一、标题释义

+   DeepSeek：众所周知的国产大模型（免费），[它的](https://www.deepseek.com/)特色是把深度思考的过程也告知了用户。官网

+   Ollama：开源的大型语言模型服务工具（免费），旨在帮助用户快速在本地运行大[模型](https://ollama.com/)。通过简单的安装指令，用户可以通过一条命令轻松启动和运行开源的大型语言模型。官网

+   RAGF[lo](https://ragflow.io/)w： 基于深度文档理解构建的开源 RAG（Retrieval-Augmented Generation）引擎，提供一套精简的 RAG 工作流程，结合大语言模型（LLM）针对用户各类不同的复杂格式数据提供可靠的问答以及有理有据的引用。官网

DeepSeek+Ollama+RAGFlow的含义就是：通过Ollama运行DeepSeek大模型，形成大模型服务。通过RAGFlow整合知识库、LLM，实现本地知识库的检索。

# 二、安装步骤

本次微学习打算在 **Windows环境 **体验“通过RAGFlow、Ollama、DeepSeek实现本地知识库搜索"。

## （一）Ollama安装

参看 [微学习05 | DeepSeek+Ollama+ChatBox：本地部署](https://datawhaler.feishu.cn/wiki/D8a5w5ttViGRDjk1arMck1lSnOb)（一）Ollama安装

## （二）DeepSeek安装

参看 [微学习05 | DeepSeek+Ollama+ChatBox：本地部署](https://datawhaler.feishu.cn/wiki/D8a5w5ttViGRDjk1arMck1lSnOb)（二）DeepSeek安装

## （三）RAGFlow安装

打开windows命令行，执行以下操作：

```
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/docker
docker compose -f docker-compose.yml up -d   #以后每次启动RAGFlow也是这个命令
```

![](img/44d32a1bc80c5bfa78b467f3c27432eb.png)

检查启动日志：

```
docker logs -f ragflow-server
```

如显示以下内容则表示安装和启动成功：

![](img/fc95e8968cf16cd27cb6a874951b7d9c.png)

查看docker镜像：

```
docker images
```

![](img/9b30583c892250026470ef288d0a0062.png)

查看运行中的docker容器：

```
docker ps
```

![](img/75c9971cc720e4428345a009c6186d6f.png)

以上列出的都是RAGFlow运行所需要的镜像包对应的容器，包含：

+   ragflow：RAGFlow本体。开源 RAG（Retrieval-Augmented Generation）引擎。

+   elasticsearch：分布式搜索和分析引擎。

+   minio：一种高性能、S3 兼容的对象存储，它专为大规模 AI/ML、数据湖和数据库工作负载而构建，它是由软件定义的存储，不需要购买任何专有硬件，就可以在云上和普通硬件上拥有分布式对象存储

+   MySQL

+   valkey：根据 BSD 许可发布的，是一款高性能键/值数据存储，支持各种负载，包括缓存、消息队列。

TIPS：如果端口冲突无法启动怎么办？

如果端口冲突，docker compose无法启动，可以修改docker_compose.yml文件，修改以下箭头所指的端口号：(例如把80改为7080）

![](img/6743690e4394f773b19624c935302a74.png)

**TIPS：遇到“Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:1200 -> 127.0.0.1:0: **listen tcp 0.0.0.0:1200: bind: An attempt was made to access a socket in a way forbidden by its access permissions.”怎么办？

进入管理员命令提示符界面：

```
net stop winnat
```

![](img/8597d1dd10ffc75d023e8ae1443a1668.png)

启动RAGFlow后执行：

```
net start winnat
```

![](img/dada0a2a240952f660cf63d5e6376717.png)

# 三、配置与验证

## （一）注册登录

打开浏览器：http://127.0.0.1 （如果像上面改过端口号，可以登录 http://127.0.0.1:7080)

![](img/21d6a29db36a0de09efe700635d626c5.png)

点击注册，注册之后就可以登录了：

![](img/19359d4b9e72d765076c73406455b254.png)

点击右上角的"English"右边的小箭头，切换到"简体中文"环境：

![](img/130fe7810a3f79c925b9838efaa8515b.png)

## （二）增加对话模型Ollama

点击右上角的头像，点击"模型提供商"：

![](img/1e0307d3c837ac04f96ff6c516afef25.png)

选择 "Ollama"，点击"添加模型"：

![](img/53a6928e3ce0eff4af51fe654d8a2ddf.png)

模型类型为chat，表示对话模型

模型名称为 deepseek-r1:32b，跟ollama list查到的模型一致：

![](img/a87dc8ac6d98826d4f551029c80d7a46.png)

基础URL填写：http://172.25.96.1:11434

其中IP地址通过在Windows环境ipconfig命令获得：

![](img/b6cf22832ae50c3ba166fefd8456b53d.png)

![](img/eccecaee8ba582b5318091c023c40e92.png)

另外，需在系统设置中配置用户变量：(配置完需重启电脑）

OLLAMA_HOST=:11434

![](img/77af55736807005902d8bf9bed454eba.png)

点击"确定"会显示已经增加的模型:

![](img/3d2fa41df44bc8f35a1f45791f2358a7.png)

## （三）增加Embedding模型

到 [硅基流动网站](https://cloud.siliconflow.cn/account/ak)[ 去](https://cloud.siliconflow.cn/i/7AV5Soqg)复制API-KEY：（如果没有注册可以先注册）

![](img/47d6cd01cc559cbb0a2c444406b371c8.png)

回到RAGFlow界面，在"模型提供商"中查找 SILCONFLOW（硅基流动）：

![](img/17f97541ac68ba31432130948b28ca47.png)

点击"添加模型"

新增密钥：

![](img/df592bdda72b6ebb4804f0d91b79a1ec.png)

点击"确定"会显示已经增加的Embedding模型:

![](img/c3f4e9ffefaccd400c21949251297eba.png)

点击"系统模型设置"：

![](img/e892e1c63b9f7fae0af3f3fab526af7d.png)

分别设置以下模型：

![](img/7d3282a8c0fb478f7950b9b390b8fb2f.png)

## （四）新建知识库

在"知识库"界面，点击"创建知识库"：

![](img/25119e169bb56e8db7ef5a04f3ba25ae.png)

点击"确定"：

![](img/ba03ccede731b121d4c93785b6f71cd9.png)

## （五）上传PDF文件到知识库

选择"数据集"，新增一个PDF文件：

![](img/f074cb2fdbb800e2bb63e32ef1df14b0.png)

点击"确定"：

![](img/a5a395f340b343ab5c49a15c796a2804.png)

![](img/f2dcc048e65dac6cdff4d516dcc6dee4.png)

点击 上面的三角按钮"开始解析":

![](img/8d935f47a1b9821243181042f6c42b27.png)

稍等一会儿，文件就被解析完毕（具体用的就是前面选择过的Embedding模型）：

![](img/307632167a9e74458ddf945ecf0d0762.png)

运行"检索测试"：

```
霍光执掌了汉室最高权力多少年
```

![](img/6ca5a3870af97a1c760d49e3737dacbf.png)

检查答案是否符合要求。如果不符合，有可能是Embedding模型没有选好或者配好，可以换一个Embedding模型，或者使用RAGFlow提供的参数配置页面进行配置。

## （六）创建助手进行聊天对话

点击聊天，新建助理：

![](img/39c49974cd613a1b4e9534a5845f1281.png)

![](img/4e55bb67a2cd95ae54e0a9f8cc083e83.png)

![](img/264e1eedcb3e24953243f628df23a85b.png)

把三个配置页面都设置一下，点击"确定"。

新建聊天：

![](img/eb074f5f28da1ed784f9100aacf0d305.png)

输入

```
霍光执掌了汉室最高权力多少年
```

![](img/afc933f07ee966f4e5d919c4c8d54771.png)