
# 知识图谱 neo4j

> 来源：[https://datawhaler.feishu.cn/docx/OQ6NdnTrVoF1amxy0X1cRXGZnch](https://datawhaler.feishu.cn/docx/OQ6NdnTrVoF1amxy0X1cRXGZnch)
>
> 本部分内容由 供稿~
>
> 内容初始来源：[知识图谱neo4j](https://zaiwen-chattests.feishu.cn/wiki/Qkgww2qzBiJHd2kJzwjcRGbNnJb)
>
> **内容贡献者说：****文档不断完善中...**

## neo4j 介绍

neo4j 是现在最流行的知识图谱工具，在学习中我也感觉用起来很简单。

## neo4j 的部署与运行

### Windows 中部署

#### 安装 Java JDK

如果已经安装了 JDK，就不需要再安装。判断是否已安装的方式是在 `cmd` 中输入 `java --version`，如果已安装会显示版本信息。

如果没有安装，请打开以下网址：https://www.oracle.com/java/technologies/downloads/?er=221886#jdk22-windows

然后下载对应的 `.exe` 文件，下载完成后直接双击运行即可。

![](img/7595056857cc731245f43665cca948a9.png)

#### 下载 neo4j

打开以下网址：https://neo4j.com/deployment-center/

选择社区版，然后选择 Windows 系统，点击 Download。

![](img/d69482921160b710dc13bf3114ea98f7.png)

下载的文件是一个压缩包，下载完成后直接解压到指定位置即可。

#### 运行 neo4j

在上一步解压后的文件夹中有一个 `bin` 目录。

![](img/c6567479c9627a353915e995fa728a6a.png)

在 `cmd` 中 `cd` 到 `bin` 目录下，然后直接运行 `neo4j console`，即可启动 neo4j。运行成功的截图如下：

![](img/210ae744616b096f8f34bdf7ca1cf498.png)

在浏览器中打开运行结果中的链接（截图中红色部分），即可打开可视化的知识图谱操作界面。

第一次打开时会提示输入用户名和密码，默认用户名和密码均为 `neo4j`，输入后会强制修改密码，设置一个密码即可。

### Linux 中部署

以下以腾讯云轻量服务器为例。

#### 安装 neo4j

参考这篇文章即可，介绍得非常详细。之前按照其他教程走了很多弯路，这里介绍了更方便的方法。

我使用的是文中介绍的 `Visit` 方式安装，这样可以省略很多安装依赖的时间。

如果使用 `rpm` 或 `deb` 包方式安装，下面的方法可能有所区别，请自行探索。

#### 配置 neo4j

通过以下命令查找 neo4j 的配置文件：

```
sudo find / -name "neo4j.conf"
```

取消以下内容的注释。注意：这一步与很多教程不同，很多教程写的是 `dbms.connectors.default_listen_address`，我找了很久都没找到，最后修改了下面的内容才解决了问题。

这段配置的作用是允许远程访问 neo4j 的可视化界面：

```
server.default_listen_address=0.0.0.0
```

#### 修改腾讯云的防火墙

具体是在防火墙中开放 `7474` 和 `7687` 端口，其他服务器也可能有此限制。

腾讯云开放端口的方式，可以参考以下文章。

#### 运行 neo4j

请使用以下命令运行 neo4j。也可以使用 `neo4j console` 运行，但不支持远程访问：

```
sudo neo4j start
```

运行后会出现以下输出：

```
Directories in use:
home:         /var/lib/neo4j
config:       /etc/neo4j
logs:         /var/log/neo4j
plugins:      /var/lib/neo4j/plugins
import:       /var/lib/neo4j/import
data:         /var/lib/neo4j/data
certificates: /var/lib/neo4j/certificates
licenses:     /var/lib/neo4j/licenses
run:          /var/lib/neo4j/run
Starting Neo4j.
WARNING! You are using an unsupported Java runtime.
* Please use Java(TM) 17 or Java(TM) 21 to run Neo4j.
* Please see https://neo4j.com/docs/ for Neo4j installation instructions.
Started neo4j (pid:3953349). It is available at http://0.0.0.0:7474
There may be a short delay until the server is ready.
```

在倒数第 2 行可以看到输出了地址 `http://0.0.0.0:7474`，将 `0.0.0.0` 修改为你的公网 IP，即可在浏览器中访问 neo4j 的可视化界面。

### 学习使用 neo4j 的可视化界面

以下是 neo4j 的可视化界面：

![](img/a44e2c73bb28aa4e8951a4fa6d95aa60.png)

点击截图中红框位置，neo4j 为我们准备了一份详细的教程，跟着教程操作一遍，就可以基本掌握 neo4j 的各种操作。

请大家一定按照教程学习，大约 15 分钟左右即可完成。

### Neo4j 的查询语言

直接使用 `gpt` 编写即可，AI 时代，我们不需要手动学习这种查询语言了。

### 在 Python 中操作知识图谱

在 Python 中使用 `py2neo` 包来操作知识图谱。`py2neo` 包的使用方法可以参考以下文档，该文档讲解非常详细：

https://segmentfault.com/a/1190000040519140?sort=newest

## 我对知识图谱的理解

知识图谱就是我们熟悉的三元组：实体、关系、实体。

不过学习后我发现，知识图谱并没有那么简单。每个实体还可以定义很多属性，例如人物实体可以有 `name`、`age`、`sex` 等各种属性。

每个关系也可以有很多属性。例如两个人物之间是朋友关系，在这个关系中还可以定义一个属性，表示这两个人物是亲密无间的朋友，还是点头之交的朋友。

### 知识图谱与结构化数据库

知识图谱在一定程度上可以完成结构化数据库的功能。

例如可以定义一个节点 `Person`，在节点 `Person` 上定义 `name`、`age`、`sex` 等属性，这些属性类似于结构化数据库中的表字段。

善用这些属性，可以使用知识图谱完成一些不太复杂的结构化数据库内容。

## 参考资料

1. 很详细的 neo4j 部署安装教程，会教我们如何阅读官方文档，根据官方文档部署安装。视频较长且有些拖沓，但看完后会学会如何根据官方文档学习。
2. 介绍了 Py2neo 如何操作 neo4j 创建节点和关系
3. 详细介绍了知识图谱的应用与原理
