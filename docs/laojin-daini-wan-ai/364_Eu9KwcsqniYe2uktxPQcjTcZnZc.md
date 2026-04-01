# Manus开源版本！无需邀请码，老金手把手教你咋用！

> 来源：[https://my.feishu.cn/docx/ANRWdud8roaIxYxtt7mctaRgneb](https://my.feishu.cn/docx/ANRWdud8roaIxYxtt7mctaRgneb)

Hello，老铁们好，我是老金！

我发现吧，老金的文章完读率不高，所以，这回老金先卖个关子。

文末放一键部署大法，和实测案例。

### 引言：什么是通用智能体？

这两天Manus属实火的不要不要的，连央视网都发布了文章！

![](img/f3202c2a52254a78d45123f3901fed16.png)

某鱼上的邀请码，都炒到飞起，属实惊人！就离谱！

![](img/1058616679db9fd259df4807491edf4f.png)

它到底有什么魔力呢？老金给你娓娓道来。

据悉，这是全球首款真正意义上的通用AI Agent，从官网展示的案例可以看到，它能够独立思考、规划并执行复杂任务，直接交付完整成果。官方宣称，Manus不仅仅是一个只会聊天的对话式AI工具，而是一个真正的自主智能体（Agent）。

从发布的视频演示来看，**M**anus在接到用户指令后可以直接操作电脑完成一系列报告撰写、表格制作等工作，并在最后导出符合用户需求的产品。

![](img/e9f9c8007cfe7e746733be1733d79df2.png)

### 开源版来袭！

一码难求？那么Github上肯定快速就有人出手了！

上线仅仅一天！10.3k Star！

![](img/8b5c26d16e76dae92009bc08757520aa.png)

看看官方演示视频。

### 安装方法手把手教程（别怕，后面老金告你怎么一键搞定！）

#### **安装指南**

1.  创建新的 conda 环境：

```
conda create -n open_manus python=3.12
conda activate open_manus
```

1.  克隆仓库：

```
git clone https://github.com/mannaandpoem/OpenManus.git
cd OpenManus
```

1.  安装依赖：

```
pip install -r requirements.txt
```

#### **配置说明**

OpenManus 需要配置使用的 LLM API，请按以下步骤设置：

1.  在 config 目录创建 config.toml 文件（可从示例复制）：

```
cp config/config.example.toml config/config.toml
```

1.  编辑 config/config.toml 添加 API 密钥和自定义设置：

```
#全局 LLM 配置
[llm]model = &quot;gpt-4o&quot;base_url = &quot;https://api.openai.com/v1&quot;api_key = &quot;sk-...&quot;  # 替换为真实 API 密钥max_tokens = 4096temperature = 0.0

# 可选特定 LLM 模型配置
[llm.vision]
model = &quot;gpt-4o&quot;base_url = &quot;https://api.openai.com/v1&quot;api_key = &quot;sk-...&quot;  # 替换为真实 API 密钥
```

#### **快速启动**

一行命令运行 OpenManus：

```
python main.py
```

然后通过终端输入你的创意！

如需体验开发中版本，可运行：

```
python run_flow.py
```

### 一键搞定？

当然邀请出我们的Cursor，只需要输入一句提示词。

```
项目已下载，config配置已经完成，需要在虚拟空间中帮我运行main.py，教程网址https://github.com/mannaandpoem/OpenManus/tree/main
```

为了避免网络问题等小白问题，老金我这里直接教下载配置。

打开网址：

```
https://github.com/mannaandpoem/OpenManus/tree/main
```

点击下载压缩包。

![](img/146d27c1349be1741f7573c2efcc874c.png)

解压后，在目录中打开文件夹*\OpenManus-main\config，复制个配置文件，并改名。

![](img/da2881da77f2723a97a42146d7e361ec.png)

打开Cursor，输入提示词，就搞定！

**中途如果你缺了**任何环境，它都会直接帮你安装好，记得开Agent模式！

因为老金本地已经啥环境都有了，所以略过了这些环节，都是之前Cursor安装的！

一张长图，证明我只说了一句话！

![](img/8bcb5b41320e48cc8f4f1133af16bf90.png)

最后，老金我给你们个实测！看视频！

![](img/c8a752bc5a8b509cf6894049e5fd011b.png)

[MyVideo_1.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JYjtbjRVRoaIKCxSj9Ocv0NAn0d)

嗯？有点不对劲儿？你想问老金我的为啥是中文？

哈哈，来找老金教你啊！~

### 结语

AI不再是冷冰冰的代码，而是能与你并肩作战的「数字伙伴」——它能替你扛下繁琐，把创造力还给你。看到这里，说明你和我一样，骨子里都躁动着对未来的野心。

但通往星辰大海的路上，99%的人会卡在「环境报错」「API玄学」「部署鬼打墙」。

别让技术门槛浇灭你的热情！

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>