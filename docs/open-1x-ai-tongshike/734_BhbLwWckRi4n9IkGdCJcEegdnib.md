# OpenManus部署教程+初体验

> 来源：[https://datawhaler.feishu.cn/docx/Ax2hdwttAoyaGSxj9JDc0YOInLV](https://datawhaler.feishu.cn/docx/Ax2hdwttAoyaGSxj9JDc0YOInLV)

# 1 引言

Manus 引爆舆论热点，到底是 Agent 弄潮儿还是营销手段？无论你的猜测如何，我想目前大多数人都没有邀请码验证自己的想法。

但开源的力量是强大的，MetaGPT 团队花费 3 个小时搭建了类 Manus 项目--OpenManus，任何人都可以在自己电脑部署体验，虽然产品细节有些不一样，但伟大产品的思路总是一致的，让我们试试这条道路能否通往罗马

# 2 部署准备

在部署 OpenManus 之前，你需要先准备好以下工作：

+   代码编辑器，推荐 VScode

+   🪜环境，非必须，但没有的话可能会有未知问题
PS：本文基于 windows 环境，其他系统可以参考他人文档

# 3 部署步骤

## 3.1 python 准备

OpenManus 需要 python 大于等于 3.11，可以先检查一下自己 python 版本，操作如下：

+   输入 win+R 打开 cmd

+   输入 python --version，显示 python 版本号大于 3.11 即可
如果未安装 python 或者目前安装的 Python 小于要求版本，可以前往 Python 官网重新安装，安装时注意勾选 add to path，减少配置环境变量的工作
Ps：如果之前已经有 python 版本了，要注意环境变量中新安装的路径是否排在最上面，否则还是会默认使用旧版本 python

![](img/231603e41d86e6e084ddad40829b355c.png)

## 3.2 初始化项目

### 3.2.1 打开项目

1.  [进入 OpenManus 的项目地址，点击下载压缩包 GitHub - mannaandpo](https://github.com/mannaandpoem/OpenManus/tree/main)em/OpenManus: No fortress， purely open ground。 OpenManus is Coming.

1.  下载后解压，打开 vscode，选择到对应文件夹

### 3.2.2 安装 UV 及依赖包

1.  点击终端-新建终端

![](img/6dd859253b16b1409bcb451b68e57a2a.png)

1.  打开终端后，输入 powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex" 安装 uv，如果已安装可以跳过，显示"everything's installed！"表示安装完成

1.  此处创建一个虚拟环境，用于后续安装依赖包，运行项目，继续在终端中输入 "uv venv --python python 3.13  "，表示使用 python 3.13 创建一个虚拟环境，显示这个提示即创建成功

![](img/509f21dc185ac32a443513af4e8f47db.png)

1.  输入 。venv\Scripts\activate，激活虚拟环境，激活后命令行前缀应该会有 （OpenManus-main）

![](img/19870ac950838af1d4dae0333a180006.png)

1.  输入 uv pip install -r requirements.txt，安装所有依赖包，

### 3.2.3 初始化项目

1.  基础环境配置好之后，需要完成初始化工作，继续在终端中输入 cp config/config.example.toml config/config.toml，创建配置文件

1.  找到对应的配置文件，在此填充自己的模型地址，名称和 apikey，注意不要配置到 config。 Example 的文件里面了

![](img/1fd1bc91ce818aaca91217fd145e0e49.png)

1.  配置完成后，在终端输入 python main.py，当看到 enter your prompt 时，表示所有工作都已准备完成，可以开始愉快的玩耍了

![](img/3c84ffa113a84d037172ae9767c0009a.png)

# 4 体验项目

## 4.1 界面效果

OpenManus 目前没有可视化界面，所有操作都必须在终端处理，在此可以输入你的要求，比方说"搜集一下目前小米 su 7 的评价，整理成 md 文档给我"，接下来 OpenManus 就会自动执行，并定期展示进度

![](img/d8b43e22cd220368247b1eae4e592648.png)

## 4.2 思路说明

目前 OpenManus 的执行思路大致如下：

+   根据用户需求，梳理待办思路

+   每一步都调用 API，思考应该使用什么工具，达成什么目标

+   调用工具后，根据执行结果，评估目标是否达成，然后继续执行下一步

# 5 评价

**优点**

+   开发速度很快，并且直接开源，十分倾佩这种极客团队

+   可以自定义 API，没有中间商收差价

**缺点**

1.  从产品角度来说：能用但不好用，命令行的模式就注定了产品体验不会很高

1.  设定的步骤是最多 30 步，对于一些复杂任务明明还没有结束，但是没有产出结果，也不知道如何继续

1.  对网页的操作更多还是**搜索-打开-爬取文本**这三样，但这些其实带搜索的 AI 基本都可以做到，更进一步的对浏览器操作，例如点击特定元素，输入内容，观察图片/视频等操作目前都还没有

1.  用户主动介入时灵时不灵，导致最终产出的效果基本依赖 AI 自身