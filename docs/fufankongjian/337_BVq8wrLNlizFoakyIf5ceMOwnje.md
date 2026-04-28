# Qwen3 Coder模型API调用指南

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/UwAcdk0MZoEhpmxsYxCcJe1on9l](https://kq4b3vgg5b.feishu.cn/docx/UwAcdk0MZoEhpmxsYxCcJe1on9l)

### 注册百炼平台账号

在**人工智能**技术飞速发展的今天，越来越多的企业和开发者希望能够以更低的门槛、更高的效率参与到大模型的训练与应用之中。阿里推出的百炼平台，正是在这一背景下应运而生的一项重要技术基础设施。作为一个专为大模型开发打造的一站式平台，百炼旨在打通从模型训练、评估、微调、部署到应用的全流程，为用户提供“即插即用”的智能开发能力。

百炼平台的最大亮点在于其**高集成度与高可配置性**。无论是企业级用户希望定制垂直领域模型，还是个人开发者尝试快速微调开源模型，百炼都能提供所需的工具链与算力支持。在技术架构上，百炼深度融合了阿里云的弹性计算资源，并配套了丰富的预训练模型库、数据处理工具、评估指标体系与在线推理服务，从而大幅简化了AI研发的工程复杂度。

值得一提的是，百炼还引入了**交互式的任务流程编排系统**，使得复杂的模型训练与调优过程可以通过可视化的方式高效管理。用户可以灵活选择模型基座、训练策略与评测方法，在统一的工作流中完成整个生命周期的管理。这种高度模块化的设计不仅提升了研发效率，也为模型质量和可控性提供了保障。

总的来说，阿里百炼平台正逐步成为国内领先的大模型工程平台之一，为推动大模型技术的落地应用提供了坚实支撑。无论你是从事科研探索，还是聚焦产业实践，百炼都可能是你进入“模型智能时代”的关键入口。

+   官网地址：https://bailian.console.aliyun.com/#/home

![](img/fe8c3ea28fc33dd0824f5b04d2a57326.png)

点击登录：

![](img/53409e2fbfc1c19b0bfd704bc4ee1635.png)

注册或扫码登录即可：

![](img/241e62781bd1863c49bf477782a482aa.png)

+   查看模型

![](img/51e0011e1e0626462081f414f1b67783.png)

点击查看详情

![](img/72a454898e4b2f36baf78c5c8751e44d.png)

即可查看模型调用价格与赠送额度：

![](img/05efe284ff4e40041cff80fd19db982b.png)

点击立即体验，可以跳转到对话页面进行对话测试：

![](img/e675ce54e8227af1cad5f11b4ac84bee.png)

### 注册API-KEY

左侧点击API-KEY，右上角选择创建API-KEY即可

![](img/15054985e80b218a4560866a3535c77a.png)

### 调用Qwen3 Coder模型

如果在Chatbox、Dify等工具或平台中调用模型，需要输入三个信息：

+   获取的API Key

+   Base URL：https://dashscope.aliyuncs.com/compatible-mode/v1

+   模型名称，如qwen3-coder-plus

#### 3.1 在Python中调用Qwen3 Coder模型

+   安装OpenAI库：

```
pip install openai
```

+   借助OpenAI风格API进行模型调用

```
import os
from openai import OpenAI

client = OpenAI(
    api_key=&quot;YOUR_DASHSCOPE_API_KEY&quot;, # 输入百炼API-KEY
    base_url=&quot;https://dashscope.aliyuncs.com/compatible-mode/v1&quot;,
)
completion = client.chat.completions.create(
    model=&quot;qwen3-coder-plus&quot;,
    messages=[
        {&#x27;role&#x27;: &#x27;system&#x27;, &#x27;content&#x27;: &#x27;You are a helpful assistant.&#x27;},
        {&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: &#x27;请编写一个Python函数 find_prime_numbers，该函数接受一个整数 n 作为参数，并返回一个包含所有小于 n 的质数（素数）的列表。质数是指仅能被1和其自身整除的正整数，如2, 3, 5, 7等。不要输出非代码的内容。&#x27;}],
    )
print(completion.choices[0].message.content)
```

![](img/598581ed7465a80013eae97a1baeb856.png)

#### 3.2 在Open-WebUI中调用Qwen3 Coder模型

Open-WebUI是目前最通用的企业级大模型对话前端之一，项目主页：https://github.com/open-webui/open-webui :

![](img/9781b121cae5ecc819cf23407fae3caa.png)

+   安装Open-WebUI

```
pip install open-webui
```

+   启动Open-WebUI

```
open-webui serve
```

![](img/3f94b69c02a17b93f8c0c5998ba8e09f.png)

+   在Open-WebUI中调用Qwen3 Coder模型：

在本地登录Open-WebUI：http://localhost:8080/

![](img/3ec720fa712013d42ad8538b77f5a1df.png)

![](img/47f5fd093d81375edc8871fb10b9d6b5.png)

![](img/a7ea30033ba67503d042a6c845d66ce6.png)

![](img/b2ba5393c3c6e0d21e9801d53c73d980.png)

点击保存即可。然后回到主页即可开始进行对话：

![](img/7103b1b48ed8b723c1b71b5caa54dbfc.png)

![](img/cf339bafd77753c8391297e270a1a181.png)

![](img/3093d7f1644605fcfc3c44a656218461.png)