
# Transformers库的使用及配置大模型开发环境

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/BM9VdWBdno3muSxj2f1cRxxxnNe](https://kq4b3vgg5b.feishu.cn/docx/BM9VdWBdno3muSxj2f1cRxxxnNe)

## 本地部署开源大模型

关于私有化部署的大模型，针对不同的开发场景，选择合适的工具至关重要。例如，进行对话测试、接口调用和数据清洗等任务时，Jupyter Lab 这种交互式环境非常适合。它允许我们直观地观察到每一步操作的输出，这对于数据分析和初步测试尤其有用。另一方面，当涉及到更复杂的软件开发项目时，例如执行模型微调、定制对话逻辑或使用像 `langchain` 这样的工具进行工程级应用开发，PyCharm 这种功能全面的集成开发环境（IDE）就显得更为合适。PyCharm 提供了代码调试、项目管理和版本控制等高级功能，这对于开发大型应用和维护复杂代码库非常重要。

因此，我们将分别介绍这两种工具的使用方法。大家可以根据具体需求灵活选择最合适的工具，以获得最佳的开发体验。

我们首先来看一下 Jupyter Lab 如何加载本地部署或者远程服务器部署的大模型环境。

需要明确的是：如果想要在本地的集成开发环境（如 Jupyter Lab、PyCharm 或 VsCode 等）中执行无论是在本地，还是远程服务器上部署的大模型项目的代码脚本或调用大模型服务时，必须首先进入创建大模型时所建立的虚拟环境。而这个过程，在任何开发工具（IDE）中，默认情况下并不会自动加载这个虚拟环境。因此，接下来我们将详细介绍如何在 Jupyter Lab 和 PyCharm 中手动加载指定的虚拟环境，以便在本地 IDE 中有效进行大型模型的相关开发工作。

## 2. 配置大模型开发环境（Jupyter Lab）

### 2.1 本地部署大模型，本地 Jupyter Notebook 加载

默认安装的 Jupyter Notebook/Lab 在启动时，只能选择一个其默认的 kernel 环境，即：

![](img/7b307e0ebd30f2648faf66ca4a565a50.png)

安装完 Anaconda 利用 `conda` 创建了虚拟环境，但是启动 `jupyter notebook` 之后却找不到虚拟环境，原因是缺少 `kernel.json` 文件。解决方法如下：

+ Step 1. 打开 Conda 的命令行终端

![](img/76c7d86d3f0463c5f1630057af37498c.png)

+ Step 2. 安装 `ipykernel`

安装 `ipykernel` 的命令如下：

```
conda install ipykernel
```

![](img/ee19ff76766137a9a303a973d9712d05.png)

+ Step 3. 找到想要使用的虚拟环境

查询虚拟环境指令如下：

```
conda env list
```

![](img/61b5d8a9f5b287d4cac5692c2cec5acc.png)

+ Step 4. 创建虚拟环境的 kernel 文件

找到项目文件的虚拟环境后，对其创建 kernel 文件，执行命令如下：

```
# 我这里是 Qwen
conda install -n <环境名称> ipykernel
```

![](img/af1fb94556a563f9906a718d4349d6d9.png)

+ Step 5. 激活虚拟环境

然后进入该虚拟环境中，执行命令如下：

```
# 我这里是 Qwen
source activate <环境名称>
```

![](img/11c3873384d68d1f2b8cd7ef8483053f.png)

+ Step 6. 将虚拟环境的 kernel 写入 Jupyter Lab

```
# 我这里是 Qwen
python -m ipykernel install --user --name <环境名称> --display-name <jupyter lab 中的显示名称>
```

![](img/40dd2577e25e4071a3f60d6e783bbc6b.png)

+ Step 7. 此时再查看 Jupyter Lab，就发现可以进入虚拟环境了

![](img/6a5eb79e0e34ff89768d7a011af0970c.png)

+ Step 8. 删除 Kernel

如果需要删除在 Jupyter Lab 中的 Kernel，可以先通过如下命令找到目前已存在的 kernel 名称：

```
jupyter kernelspec list
```

![](img/c801ba9f48d2af8c8dcbb329ffd1d52a.png)

然后删除指定的 kernel 环境。注：这只是删除了在 Jupyter Lab 上的 Kernel，并不会删除实际的虚拟环境

![](img/04dcd3e7ca5e5bec901fb580a4b94615.png)

### 2.2 远程服务器部署大模型，本地 Jupyter Notebook 加载

第二种情况是：在远程服务器上部署的开源大模型（基本都是 Linux 操作系统环境），想要在本地的计算机上使用 Jupyter Lab IDE 调用服务，那需要执行如下操作：

+ Step 1. 安装远程 Jupyter Lab 依赖

在远程服务器的命令行，输入如下命令：

```
pip install jupyterlab
```

![](img/7cbfe8345acf557731eb48500ff09737.png)

+ Step 2. 加密连接密码

安全起见，对连接时的密码进行加密处理，否则明文写在配置文件中，容易造成数据安全风险，依次执行如下操作：

```
from jupyter_server.auth import passwd
passwd()
```

![](img/64ea19158829b3477992920696afa1a5.png)

+ Step 3. 生成 Jupyter Lab 的配置文件

完成密码加密后，执行如下命令生成 Jupyter Lab 的配置文件（`jupyter_lab_config.py`）：

```
jupyter lab --generate-config
```

![](img/b3414b2ae084aee68eb6880d014b14a9.png)

+ Step 4. 编辑 Jupyter Lab 的配置文件

使用 Vim 编辑器，找到如下配置，执行修改：

```
c.ServerApp.allow_origin = '*'
c.ServerApp.allow_remote_access = True
c.ServerApp.ip = '0.0.0.0'
c.ServerApp.open_browser = False  
c.ServerApp.password = '加密后的密码'（上一步复制的加密串）
c.ServerApp.port = 8002 
```

![](img/0ebb41a0f75acf6a9cb72a40ab14e735.png)

+ Step 5. 后台启动 Jupyter Lab 服务

全部配置完成后，在服务器端启动 Jupyter Lab 服务，通过如下命令后台启动：

```
nohup jupyter lab --allow-root > jupyterlab.log 2>&1 &
```

![](img/c65b673ca7818e54ed91f2ab37529a3a.png)

+ Step 6. 打开本地机器的浏览器

这里只需要输入自己在上一步配置的服务器 IP、端口及输入密码，通过认证后即可进入 Jupyter Lab 环境。

![](img/9e8ad37caade2d52fb2933beba3ba856.png)

+ Step 7. 进入 Jupyter Lab 环境

同样，默认进来以后，只有 Anaconda 在安装时默认创建的 (Base) 这个虚拟环境，所以接下来需要把 Qwen 的模型运行依赖的虚拟环境加载进来。这里的操作和 Windows 基本一致。

![](img/9d78bd7a88b892891bbe0ab1b25595dd.png)

+ Step 8. 安装 Kernel 工具

![](img/215abf269068053b8ffa4bf85f903bef.png)

+ Step 9. 给指定的虚拟环境安装 Kernel

![](img/e5dea03805e2ae18ab2a51e0015b9507.png)

+ Step 10. 将虚拟环境的 Kernel 写入 Jupyter Lab

![](img/9ba6ac0a98d703d58f38be8215e661eb.png)

+ Step 11. 验证安装情况

![](img/8197f959487e2ec9e1bff2bcb711f087.png)

+ Step 12. 修改 Jupyter Lab 的默认启动路径

首次安装后，当启动 Jupyter Lab 时，其加载的默认路径对应的 Linux 系统路径是 `/home` 目录。

![](img/94c185e10abdb2dd7eadd226105820b3.png)

而我们在进行大模型开发时，往往需要在代码中指定大模型的权重文件、配置文件等信息，所以如果使用默认路径，再寻找大模型相关的路径往往是比较麻烦的，一个最方便的方式就是将其默认启动路径，修改为大家常用的项目开发路径。修改 Jupyter Lab 默认启动路径的方法如下：

首先，生成 Jupyter 配置文件，运行以下命令来创建：

```
jupyter notebook --generate-config
```

![](img/1395db1990116d372a8ba40acee730f9.png)

然后，使用文本编辑器打开这个配置文件。在配置文件中，找到以下行：

```
vim /root/.jupyter/jupyter_notebook_config.py

# 找到如下行：快捷方法是进入文件后，直接按一下 / ，输入关键字，就可以定位到。
# c.NotebookApp.notebook_dir = ''
```

![](img/cec70a807536b8b35aa2375a56765fc0.png)

取消该行的注释，并将空字符串替换为你希望 Jupyter 启动时的默认目录路径。比如我先找到我想替换的路径：

![](img/7ad3b9b473d9917040377fce73c5da8e.png)

取消该行的注释，并将空字符串替换为上述希望 Jupyter 启动时的默认目录路径。

![](img/e02f7d882ba868b2dcfd4109ec622b76.png)

+ Step 13. 重启 Jupyter Lab 服务

如果是通过 `nohup` 命令在后台启动 Jupyter Lab，并且希望在重启之后应用新的默认启动路径，需要首先按照之前的步骤修改配置文件，然后重新启动 Jupyter Lab。

首先需要停止当前运行的 Jupyter Lab 实例，找到其进程 ID（PID）。可以使用 `ps` 命令配合 `grep` 来查找：

```
ps aux | grep jupyter
```

![](img/40ac7e4cd56275ec4c10e301f3cfde72.png)

找到 Jupyter Lab 进程的 PID 后，使用 `kill` 命令来停止它。

![](img/7cc7dcbb16f9ecf4da2865d23a6572d2.png)

最后使用之前的命令重新启动 Jupyter Lab：

```
nohup jupyter lab --allow-root > jupyterlab.log 2>&1 &
```

![](img/dd1bb9b55c00da86311d3afb2e8d4c4a.png)

此时再重新在本地的浏览器中打开 Jupyter Lab 的远程地址，默认启动路径就会更改为自定义的路径。

![](img/f09613533fb30dbb511d9bf3df071191.png)

接下来，我们就在 Jupyter Lab 中，进行后续的应用开发尝试。

📍 更多大模型技术内容学习

扫码添加助理英英，回复“大模型”，了解更多大模型技术详情哦👇

![](img/5638eb917eb726b1132150bcd09356f6.png)

此外，扫码回复“入群”，即可加入大模型技术社群：海量硬核独家技术干货内容 + 无门槛技术交流！
