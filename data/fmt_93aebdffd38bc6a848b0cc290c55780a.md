
# Ollama 开源大语言模型部署

> 来源：[https://my.feishu.cn/docx/KDrOdJngHoBB77xI8SRcAgHAnfG](https://my.feishu.cn/docx/KDrOdJngHoBB77xI8SRcAgHAnfG)

大名鼎鼎的 Ollama 是一个开源的大型语言模型服务工具，它帮助用户快速在本地运行大模型。通过简单的安装指令，用户可以执行一条命令就在本地运行开源大型语言模型。

Ollama 下载地址：`https://ollama.com/download`

## 安装使用说明

选择对应系统，直接下载

![](img/e856a6c85e58f02943d3e8bca6d5ec53.png)

双击直接安装。

![](img/70304c09fc421947033c46be2cffefd7.png)

将下方地址复制进浏览器中。如果出现下方字样，表示安装完成。

`http://127.0.0.1:11434/`

![](img/c30c8bfce157fcd8b5884b97f9fbd05d.png)

并在系统右下角会出现这个羊驼。

![](img/694e42d900317b2e3d3d3a97ada4c33e.png)

在 Ollama 官网上查看可使用的模型。

地址：`https://ollama.com/library`

使用 `win+R` 打开运行，输入 `cmd`。

![](img/87768424436acc5a3818f52459a909ef.png)

这里我选择了 `qwen2:7b`，粘贴以下指令

```
ollama run qwen2:7b
```

下载完成，直接输入即可使用。

![](img/638d3f64dd99fb42948b21e55fe9dd96.png)

## 指令说明

下载模型：

```
ollama pull model_name
```

启动模型（若模型不存在，那么会先下载模型，然后再运行）：

```
ollama run model_name
```

删除模型：

```
ollama rm model_name
```

查看下载的模型：

```
ollama list
```

查看模型信息：

```
ollama show model_name
```

查看当前运行的模型：

```
ollama ps
```

复制模型：

```
ollama cp llama3 model3
```

运行多模态模型：

```
描述一下图片内容? /d/cloud.jpg
```

将提示作为参数传入：

```
ollama run qwen &quot;总结文中内容: $(cat README.md)&quot;
```

列出模型：

```
ollama list
```

## 定制自己的模型

所谓定制模型，就是在使用某个模型作为base模型，然后在该模型上进行定制，下面我们就以 `qwen2:7b` 作为基础模型进行定制：

1. 编写定制规则，即 `Modelfile` 文件，内容如下（此处 `Modelfile` 位置是 `C:\Users\Administrator\Desktop`）：

```
FROM qwen2:7b

set the temperature to 1 [higher is more creative, lower is more coherent]PARAMETER temperature 1

set the system messageSYSTEM &quot;&quot;&quot;You are Mario from Super Mario Bros. Answer as Mario, the assistant, only.&quot;&quot;&quot;
```

2. 使用 `Modelfile` 来创建定制模型，执行命令

```
ollama create mario -f ./Modelfile
```

![](img/c6c35c6a40990d76c9751ebd3c3111fe.png)

3. 查看已有的模型，`ollama list`

![](img/5495120017ef66733e35400910d3cef3.png)

可以看到一个名称为 `mario` 的模型已经存在了。运行该模型，输入 `prompt` 试一下：

![](img/f4e7d654ea4980c864cbdc443d2271c8.png)

效果不错，已经能按照定制的内容进行回答问题了。

关于定制化规则，更多的规则可以参考链接：

`https://github.com/ollama/ollama/blob/main/docs/modelfile.md`

`https://blog.csdn.net/spiderwower/article/details/138755776`

## 常见的Q&A整理

### 如何指定上下文窗口大小

Ollama 默认上下文窗口大小为 2048 tokens，可以通过命令 `/set parameter num_ctx 4096` 将上下文窗口长度调整为 4096 token

![](img/7a2719b6c9b45d80ea41d8a3f9255813.png)

先运行 `gemma:2b` 模型，然后将上下文窗口长度改为 4096

### 怎么判断模型时运行在CPU还是GPU上的？

执行命令 `ollama ps`，如下图

![](img/b13e4340bc87a4e9dbba6186c991d3f1.png)

通过 `Processor` 列来判断：

100% GPU 表示模型完全运行在 GPU 上，即显存中；

100% CPU 表示模型完全运行在 RAM，即内存中；

48%/52% CPU/GPU 表示模型即运行在了显存中，也运行在了内存中；

### 修改 Ollama Server 的配置

![](img/a46ce46529d8ef65bfc914eec93918d4.png)

`OLLAMA_HOST`：修改绑定的地址，可让别人通过局域网访问

`OLLAMA_MODELS`：修改模型的保存文件夹

更多的 Q&A 问题，参考官网文档：

`https://github.com/ollama/ollama/blob/main/docs/faq.md`

`https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc`
