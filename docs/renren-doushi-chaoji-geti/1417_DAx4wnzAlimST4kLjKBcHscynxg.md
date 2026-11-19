
# Colab上部署ChatGLM2-6b-int4

> 来源：[https://xuqiwei1986.feishu.cn/docx/XDURdRmfRoDVnqxUujlcEo5mn9g](https://xuqiwei1986.feishu.cn/docx/XDURdRmfRoDVnqxUujlcEo5mn9g)

Colab是Google提供的免费云资源平台（https://colab.research.google.com/），里面提供了免费的GPU资源供开发者使用，但GPU资源非强制保障（白嫖的也可以理解，只是使用时候需要注意在使用完后断开），如果需要更好的服务可以订阅pro等付费服务，今天分享下我在Colab上部署及使用免费GPU运行ChatGLM2-6b-int4的过程；

## Colab连接到google云盘

Colab的笔记本需要连接到google云盘，后续的操作也需要记住连接到云盘相关目录下，方可保证所有操作能在我们指定的位置上进行：

![](img/4c9c88709ec3d28fdca90519839b52db.png)

而默认的文件会生成在`/content`目录下；

记住以下这个命令，后续的操作我们都会指定到这个目录下：`%cd /content/drive/MyDrive`

![](img/cbdababeac6db80bccf98f51e0fbd51d.png)

## 确认目前已经运行在GPT模式下

Colab默认运行模式没有GPU资源，需要手动修改为使用GPU资源，点击“修改”--“笔记本配置”，在以下页面中进行修改：

![](img/ad9e48dd9e3b94df9525e79bd0bfda21.png)

修改后，可以在笔记本中运行此命令确认是否已经有GPU资源：`!nvidia-smi`

如果出现以下显示，则说明已经可以使用GPU资源；

![](img/53e051f3f3a0db579315e42fbe4350a4.png)

## 在Colab上部署ChatGLM2-6b-int4

考虑到空间配额，本次我部署的是int4版本来尝试部署过程及推理，而部署的过程就是项目下载及权重下载；

### 3.1 从Git上下载项目文件

先确认我们目前处于`/content/drive/MyDrive`下；

在笔记本中运行：`!git clone https://github.com/THUDM/ChatGLM-6B.git` 下载项目文件；

![](img/b6bc47978e5ba6685dafb0885d9f8a7e.png)

运行完我们可以看到，项目文件已经出现在我们的`/content/drive/MyDrive/ChatGLM-6B`目录下：

![](img/be0a88108c4047b9f0c19d61b357f608.png)

### 3.2 从HuggingFace上下载权重文件

在目录下新建一个文件夹`model`，以便放置下载的权重文件

![](img/3be026e65adb9d90304313e5837897fc.png)

完成后可以看到新建的`model`文件夹已经存在`ChatGLM-6B`目录下

![](img/723288230925b37cbdb785e5c0762543.png)

在笔记本中把路径定位到`model`目录下：`%cd /content/drive/MyDrive/ChatGLM-6B/model`

![](img/9fc0572f8c0ba42936aa144f312944ec.png)

由于权重文件是lfs类型，所以还是需要安装一下lfs：`!git lfs install`

![](img/70a44e7b664dfc697af95eee2c7d97b9.png)

开始下载权重文件：`!git clone https://huggingface.co/THUDM/chatglm2-6b-int4`

![](img/5c9fbd20eab6c8b36cd82e4662c93586.png)

这里面的报错其实可以忽略，因为这个权限错误是在文件下载完成后检查时候爆出来的，其实并不影响下载文件的使用，感觉可以忽略；

下载完成后，可以看到相关权重文件都已经出现在`model`目录下：

![](img/d3958897ff0a9bfb37a480fa4fd23938.png)

## 安装需要的依赖包

目录切换到`ChatGLM-6B`下：`%cd /content/drive/MyDrive/ChatGLM-6B`

![](img/b0003c26a9d200d4c2d4ad7006869dcd.png)

安装项目需要的依赖包：`!pip install -r requirements.txt`

![](img/c446edda6786f93e008f61a848ac9b1.png)

## 修改配置，运行模型

在`ChatGLM-6B`文件夹下的`web_demo.py`中修改模型位置文件，指向下载的权重文件位置：

![](img/f6fa595404ab8e5ecdbed51ecf305656.png)

直接在笔记本中尝试加载模型并进行简单的对话：

![](img/3b35adb00971b1d0e0062c250110a3b8.png)

到目前，直接使用`python`命令运行`web_demo.py`,可以运行web页面进行对话啦；
