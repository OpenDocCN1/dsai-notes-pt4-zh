
![](img/934e3074dbb9a2b634c2ca31c2e66d3d.png)

当然，你也可以直接在浏览器中搜索关键词`ollama`，按照图中的实例点击进入官网亦可。进入官网后点击正下方的Download按钮，进入下载页面。

![](img/c911a8558a5af3a4756d21b908e907ee.png)

确保是在Windows环境的下载方式，即可点击Download for Windows开始下载。

![](img/6061e19f0488ea58fdc280f1d3f8d127.png)

开始下载后便会在右上角下载/安装栏出现下图图标，安装完成后可以在下载栏或下载文件夹（你的默认下载路径）找到该文件。

![](img/2541483c75e1e4644d81973bd6acf5d6.png)

![](img/40f3dbc5226f6a49a6e5f11a0bbc9b1e.png)

下载完成后在对应文件夹双击打开安装包完成安装流程。

![](img/0df380215a682ec33f9d47098e71c927.png)

下载完成后在对应文件夹双击打开安装包完成安装流程。

![](img/bf40e66c6a221a3f2ebfdb861895fdfd.png)

等待安装完成即可。

![](img/37fe516ff91fdf1c98e6c84e6c8b9085.png)

安装完成后会有如下提示：

![](img/84ec7ff7f67d7ab55c29d6c4970c56fc.png)

启动Ollama可以直接在Windows的命令行中实现，通过win+R键启动`cmd`命令，打开命令行终端。

![](img/a7ca27428ad067a2c18c4e79ff12ccfd.png)

进入命令行：

![](img/dd894283f21c068a464d91b961bc4f8e.png)

在终端中执行命令`ollama run qwen2.5:72b`，即可下载`qwen2.5:72b`模型。模型下载完成后，会自动启动大模型，进入命令行交互模式，直接输入指令，就可以和模型进行对话了。对应参数的模型的下载方式可以通过在Ollama官网查看到下载指令。当出现`success`字样的时候说明下载成功，当出现`>>>`图标的时候可以进行对话交互了。

```
ollama run qwen2.5:72b
```

![](img/86e257fa1922333fa6c444c760d83a51.png)

完成下载后会直接进入模型启动状态，如果退出或刷新界面，再次输入指令`ollama run qwen2.5:72b`即可启动对应模型。

可以看到推理所需的计算资源相对均匀的分布在每张工作显卡上。

![](img/4c0b8947c492ba2832105a97050cc2a5.png)

如果想结束对话，直接在对话栏中输入`/bye`即可结束进程。

# 2.5 接入Qwen2.5 模型

Ollama 是支持 API 调用的，默认访问的 API 端点是：`http://localhost:11434`，所以当我们本地启动模型服务后，在Open WebUI 可以直接显示已经加载在本地的模型：

![](img/8cee48adaa68b1ba657b908cbd0adf97.png)

所以当我们本地启动模型服务后，在Open WebUI 可以直接显示已经加载在本地的模型：

![](img/48faf612f62216d366373c41f6614903.png)

接下来，就可以正常进行对话了。

![](img/2b0356c4d5f4bf865d8b81d260d4a216.png)

# 3. 个性化定制智能客服

有了这样一个系统，我们怎么去定制化成一个私有领域的智能客服呢？按照前两天的思路，我们要给该系统配置身份设定和可以使用的工具。具体的配置方法如下：

## 3.1 通过 `System Message` 设置身份背景信息

首先点击账户，打开管理员面板，找到模型选项：

![](img/2eb4f9beb303ebaa2661f81c2bac39cc.png)

依次修改对话的称呼以及给智能客服设定的身份信息。

![](img/e1f55e4a68e0239985f4f7f51851f2a7.png)

回到主页后刷新一下页面重新加载数据，则可以看到新的名称：

![](img/3e860671ec4d608fb31eb42561716212.png)

进行问答测试，可以发现已经生效：

![](img/fcc761e3190760bc8eadd1b1448f8129.png)

## 3.2 给智能客服添加私有工具

添加工具的思路和方法就和我们体验课第二天介绍的流程是完全一致的，只不过我们需要按照Open WebUI的规范来进行适配，方法如下：

+ **添加查询商品后台数据的工具**

对于电商智能客服这个场景，如果我们希望它能够在回答用户的问题之前，先去查询上述创建的商品后台数据库，那么就需要出创建一个函数来做这件事。比如用户输入：你们家都有什么球在卖？ 那么理想的状态是这个函数可以根据 球 这个关键字向后台数据库执行查询操作。所以我们构建了如下所示的`query_by_product_name`函数：

```
import pymysql


class Tools:
    def __init__(self):
        pass

    # Add your custom tools using pure Python code here, make sure to add type hints
    # Use Sphinx-style docstrings to document your tools, they will be used for generating tools specifications
    # Please refer to function_calling_filter_pipeline.py file from pipelines project for an example

    def query_by_product_name(self, product_name: str) -&gt; str:
        &quot;&quot;&quot;
        The name of the product to search for in the database. The function returns the promotional details if found.
        &quot;&quot;&quot;
        print(f&quot;开始查询产品：{product_name}&quot;)  # 打印正在查询的产品名称

        # 连接 MySQL 数据库
        try:
            print(&quot;尝试连接到 MySQL 数据库...&quot;)
            conn_mysql = pymysql.connect(
                host=&quot;localhost&quot;,  # 这里替换成你自己的 MySQL 主机地址
                user=&quot;root&quot;,  # 这里替换成你自己的用户名
                password=&quot;snowball2019&quot;,  # 这里替换成你自己的密码
                database=&quot;sports_db&quot;,  # 这里替换成你的数据库名称
                charset=&quot;utf8mb4&quot;,  # 字符集
            )
            print(&quot;成功连接到数据库&quot;)
        except Exception as e:
            print(f&quot;连接数据库失败：{e}&quot;)
            return str(e)

        cursor_mysql = conn_mysql.cursor()

        # 使用 SQL 查询按产品名称查找优惠信息。&#x27;%&#x27;符号允许部分匹配。
        try:
            query = &quot;SELECT * FROM products WHERE product_name LIKE %s&quot;
            print(
                f&quot;执行查询：{query}，参数：{(&#x27;%&#x27; + product_name + &#x27;%&#x27;,)}&quot;
            )  # 打印查询和参数
            cursor_mysql.execute(query, (&quot;%&quot; + product_name + &quot;%&quot;,))
        except Exception as e:
            print(f&quot;查询执行失败：{e}&quot;)
            return str(e)

        # 获取所有查询到的数据
        try:
            rows = cursor_mysql.fetchall()
            print(f&quot;查询结果：{rows}&quot;)  # 打印查询结果
        except Exception as e:
            print(f&quot;获取查询结果失败：{e}&quot;)
            return str(e)

        # 关闭连接
        conn_mysql.close()
        print(&quot;数据库连接已关闭&quot;)

        if not rows:
            print(&quot;未找到相关优惠信息&quot;)

        return f&quot;Current promotional details = {product_name}, {rows}&quot;
```

在Open WebUI中的配置方法如下：

![](img/acebee70bcb4fc30fc286e8c1ee1803e.png)

+ **添加查询商品折扣信息的数据**

同理，如果用户问相关的折扣信息，也需要先查询后台的数据库再进行回复。

```
import pymysql


class Tools:
    def __init__(self):
        pass

    # Add your custom tools using pure Python code here, make sure to add type hints
    # Use Sphinx-style docstrings to document your tools, they will be used for generating tools specifications
    # Please refer to function_calling_filter_pipeline.py file from pipelines project for an example

    def query_discount_by_product_name(self, product_name: str) -&gt; str:
        &quot;&quot;&quot;
        The name of the product to search for in the database. The function returns the promotional details if found.
        &quot;&quot;&quot;
        print(f&quot;开始查询产品：{product_name}&quot;)  # 打印正在查询的产品名称

        # 连接 MySQL 数据库
        try:
            print(&quot;尝试连接到 MySQL 数据库...&quot;)
            conn_mysql = pymysql.connect(
                host=&quot;localhost&quot;,  # 这里替换成你自己的 MySQL 主机地址
                user=&quot;root&quot;,  # 这里替换成你自己的用户名
                password=&quot;snowball2019&quot;,  # 这里替换成你自己的密码
                database=&quot;sports_db&quot;,  # 这里替换成你的数据库名称
                charset=&quot;utf8mb4&quot;,  # 字符集
            )
            print(&quot;成功连接到数据库&quot;)
        except Exception as e:
            print(f&quot;连接数据库失败：{e}&quot;)
            return str(e)

        cursor_mysql = conn_mysql.cursor()

        # 使用 SQL 查询按产品名称查找优惠信息。&#x27;%&#x27;符号允许部分匹配。
        try:
            query = &quot;SELECT * FROM discounts WHERE product_id LIKE %s&quot;
            print(
                f&quot;执行查询：{query}，参数：{(&#x27;%&#x27; + product_name + &#x27;%&#x27;,)}&quot;
            )  # 打印查询和参数
            cursor_mysql.execute(query, (&quot;%&quot; + product_name + &quot;%&quot;,))
        except Exception as e:
            print(f&quot;查询执行失败：{e}&quot;)
            return str(e)

        # 获取所有查询到的数据
        try:
            rows = cursor_mysql.fetchall()
            print(f&quot;查询结果：{rows}&quot;)  # 打印查询结果
        except Exception as e:
            print(f&quot;获取查询结果失败：{e}&quot;)
            return str(e)

        # 关闭连接
        conn_mysql.close()
        print(&quot;数据库连接已关闭&quot;)

        if not rows:
            print(&quot;未找到相关优惠信息&quot;)

        return f&quot;Current promotional details = {product_name}, {rows}&quot;
```

在Open WebUI中的配置方法如下：

![](img/bdd02f439867e1552737b865d96ab497.png)

配置完成后如下图所示：

![](img/35b8d2fba79c5134a89f687d75198c43.png)

## 3.3 给智能客服绑定工具

在完成工具的创建后，我们需要将工具绑定到指定的智能客服中，具体的操作方法仍然是首先点击账户，打开管理员面板，找到模型选项：

![](img/bd39b9ce1eadef52a995d8a986e19353.png)

勾选创建的工具并保存，即可在下次对话时自动加载工具。

![](img/788fd316b3b332eb454a8061d76afdda.png)

至此，智能客服就配置好了，可以正常进行对话交互了。

![](img/9126c14d40a9293dbbee62260edb1726.png)

所以大家也能够看到，即使这样的开源项目开放了在前端配置的控制面板，如果我们不理解大模型的底层应用原理的话，也很难在可视化页面灵活的配置出特定的场景，并不用提源码层面的二次开发了。所以掌握大模型在 部署调用、RAG以及Agent能力，是非常有必要的。

**《大模型与Agent开发实战课》**为【100+小时】体系大课，总共20大模块精讲精析，零基础直达大模型企业级应用！

🍻现开设了大模型学习交流群，扫描下👇码，来遇见更多志同道合的小伙伴~

海量硬核独家技术干货内容+无门槛技术交流~

![](img/b16e2364868b7e1943a44b63b500659c.png)

上图扫码👆即刻入群！

📍 社群技术交流氛围浓厚，不定期开设硬核干货&前沿技术公开课噢~

# 4. 大模型与Agent学习路径

![](img/7578774cac73d80bdb9e4bd907e84ccd.png)

在我们谈论的Stage 5：Organizations 阶段特别强调的AI是一个组织，组织的概念大家应该都比较清楚，它大到一个国家，小到两个人的团队，但它绝不仅仅是一个个体。但我们之前所学习的，强如完全自主循环代理ReAct，它其实做的都是在尝试去打造更强的独立个体，我们通过给它传递更多工具（Tool Calling），赋予上下文的记忆（Memory）等等多种方式让其能够处理越来越发展的任务需求。但随着需求越来越复杂，能够预想到的以下几个非常关键的问题是：
