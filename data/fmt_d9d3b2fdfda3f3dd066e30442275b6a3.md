
# 本地搭建大模型知识库 Ollama+AnythingLLM（保姆级）

> 来源：[https://xmgtoagi.feishu.cn/docx/RssDd3YkOorxfJxifPQcM2cHn0g](https://xmgtoagi.feishu.cn/docx/RssDd3YkOorxfJxifPQcM2cHn0g)

## 一、安装 OLLAMA

好多同学不知道怎么下载需要用到的工具，我这里给大家都准备好了，需要的文末领取。`qwen7b.gguf` 是阿里巴巴的通义千问大模型，7B 大模型，7 亿参数，至少需要 8G 内存，越大回复越流畅。

![](img/837994e6c96597fc572b6d3a8314e88e.png)

下载完后，双击 `OllamaSetup`，自动安装成功，默认安装在 C 盘，不能选择安装路径的哦，安装完后，右下角会出现 Ollama 图标。

![](img/3c08eb29ccf44ef682059c0af197c13b.png)

## 二、创建环境变量

`OLLAMA_MODELS`，随意指定变量值，如图所示：

```
OLLAMA_MODELS
```

![](img/da7fa114ec44acfc5ec700b6b493fbbd.png)

## 三、重启 OLLAMA

选择右键小图标，然后退出。

![](img/26e146d1f18667bb7e7ae0333093bdd2.png)

在开始菜单中找到重启 Ollama。

![](img/377693928be4d337d8b368c51d13b22a.png)

## 四、重启后目录

![](img/c4a36738beb8b80f79e5561cf9448e6b.png)

## 五、创建 Modelfile

在 `blobs` 下创建 `Modelfile` 文件，没有后缀。

![](img/846ecee4f8e8395558700e81c775bc09.png)

## 六、加载模型

### 1. 准备模型文件

将 `qwen7b.gguf` 模型放到 `F:\Ollama\models\blobs` 目录下，和 `Modelfile` 同目录（不一定非要同目录，如果放到其他地方，以下路径随着更改就行）。

```
FROM ./qwen7b.gguf
```

![](img/17c85ff7a0239573b353260f4142010c.png)

### 2. 打开命令行

在 `Modelfile` 所在路径输入 `cmd`，然后回车。

![](img/550a6e05100cbfcc58d4ca823bac1c0c.png)

![](img/55e9e966fdb725872001c3ce8e78faec.png)

### 3. 执行创建命令

```
ollama create qwen -f Modelfile
```

等待差不多 10 分钟，出现 `success` 表示成功。

![](img/e8590c8929a77eafdc66a46efc99fe52.png)

## 七、成功后的目录

![](img/3bb80b16c9379d6bc5c2ecbc8397ac7e.png)

![](img/58b8441309e101f675dd49c2b5ac6063.png)

## 八、查看模型

```
ollama list
```

![](img/a0498d8988d109f6a06ebe6f021ce3ab.png)

## 九、运行模型

```
ollama run  qwen
```

运行成功的窗口如下：

这样就可以开始对话了。

![](img/c4f5fddecdd27d25bb479e3b910dd703.png)

![](img/a46d0cf69cb2baf44b60fee48df5641c.png)

## 十、安装 AnythingLLM

我们接着讲解，如何通过 AnythingLLM 实现友好对话的。

双击安装包，它会自动安装完成。

![](img/20c4007aef34057cb74a323a8e960a63.png)

这是安装后的界面。

![](img/6d22965d7031b1e0cbc7dd6ad200336d.png)

接下来我们设置一下 Ollama 配置，AnythingLLM 是一个前端，Ollama 是一个运行大模型的后台，我们现在需要通过 AnythingLLM 去调用后台接口，方便我们实现友好对话。

### 配置 Ollama

![](img/893c8901fc82b06b0ea79f9d76058057.png)

`llmProvider` 这里选择 Ollama，后台访问接口地址 `url` 默认为：`http://127.0.0.1:11434`，模型我们选择 `llama3` 中文版，也可以选择已经安装的模型 `qwen`，`token` 默认即可，如下图所示：

![](img/c5defe55c6c3f6a34f05ac2306bd56ee.png)

### 设置向量数据库

选择 `lanceDB` 即可。

![](img/f0038f0b83d7e830afabcc87168dd30e.png)

### 新建空间

![](img/d309f5686d025ebd08ebc1df20a3ab9c.png)

接下来我们测试一下。

![](img/fa3a59d5ddf15116c32ee789aa5028e2.png)

![](img/a306007a071c40b0f084c22b429ae0ff.png)

## 安装包获取

[AI 工具箱](https://xmgtoagi.feishu.cn/wiki/NCLCwlO73ipuV6kROAccBNNfnTb)

## 沟通交流

在使用的过程中会遇到很多问题，而且很多问题在网上找不到答案，因此我给大家搭建了一个环境，建了一个群，相关安装包都在里面了，专门沟通 AI 大模型领域，非诚勿扰！扫码请备注“大模型”，带其他目的人请走开。

![](img/fd7d6dbe05ac4ae02eba8a1121a8cc58.png)
