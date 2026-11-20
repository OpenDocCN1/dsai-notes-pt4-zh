
# AI | 基于 MaxKB+Ollama+Llama3 手把手从0-1构建本地私有化知识库

> 来源：[https://agispace.feishu.cn/docx/LKNkd2yvrogctfxYhzJcPik4nwf](https://agispace.feishu.cn/docx/LKNkd2yvrogctfxYhzJcPik4nwf)

> 作者：译点

> 👸号：每天译点晓知识

> 原文链接：https://mp.weixin.qq.com/s/GsKSHYfWxaGq496Q5DR1pg

## 前言

随着人工智能技术的快速发展-大型语言模型（LLM），如 OpenAI 的 GPT 系列，已成为众多企业和个人关注的焦点，而 Ollama 作为一款开源的大型语言模型服务工具，正逐渐受到开发者和研究人员的关注，本文将探讨基于 MaxKB+Ollama +Llama3 如何快速零成本构建本地私有化 AI 知识库！

![](img/b40da3814dc824632ebb93fd61ee6c00.png)

## 安装 Ollama

Ollama 作为一款开源的大型语言模型服务工具，凭借其开源性、大规模和可定制性等特点，在人工智能领域展现出了巨大的潜力。通过高效性、灵活性和社区支持等优势，Ollama 在NLP、机器翻译和文本生成等领域得到了广泛应用。

![](img/2d307e20c6b83ba221c126207fb6b540.png)

![](img/440e284436f6a37b7a90ce042191e5a2.png)

下载&安装 Ollama 对应版本，根据操作系统任选其一即可。

## 运行 Llama3

输入命令，运行：To run and chat with Llama3.

```
ollama run llama3
```

![](img/ce290937591c147fba1aed3ee2586e0b.png)

上述命令，默认下载的就是：

![](img/e189e356860d0964de84da6538749998.png)

## 安装 MaxKB

MaxKB 是一款基于 LLM（Large Language Model）大语言模型-知识库问答系统(Max Knowledge Base)

![](img/01f0b4afcf6591c901f7d4c7031a6da9.png)

在 MaxKB 中接入 Ollama 的 LLM 模型，可以快速部署本地知识库问答系统~

![](img/4ac8c1e93203c13c307e8e8ec4578859.png)

```
docker run -d --name=maxkb -p 8080:8080 -v ~/.maxkb:/var/lib/postgresql/data 1panel/maxkb
```

拉取镜像，待其镜像下载完成后，

![](img/0613d22dbf0841eee6fa738345672989.png)

我们可以查看其镜像相关信息，

![](img/228884298536a9152a0eed05d2b12a9f.png)

界面可视化-启/停构建的镜像服务。

![](img/74a68406a33487e998e86814d1ba2581.png)

首先，打开 MaxKB 界面 UI：

```
http://127.0.0.1:8080/ui/login#/
```

![](img/834ca0bf84c430d20c357a15f5e85465.png)

```
初始账号: admin初始密码: MaxKB@123..
```

登录成功之后，更新密码：

![](img/8f6cb64b826805a42acbd1e891217096.png)

然后，在左侧导航菜单栏，打开系统设置，进入模型设置，添加我们前面下载好的 Ollama+Llama3：

![](img/9f620936139331041e9d58438fd0c965.png)

模型设置，可根据实际场景需要添加：

![](img/86317a5e7fe1f9a965d39fef525cb072.png)

当模型配置完成后，则构建我们的专属知识库：

![](img/50fff988b9b6e1a69b3d9f9835c2887f.png)

这里，我们可以设置知识内容分段的类型：

![](img/3d9df6c7dbe0b055c1de58c6db4b253b.png)

这样，我们的译点架构-专属知识库就创建好了。

![](img/d3daae0479dc938c27d371abeb77b633.png)

接下来，我们就可以根据译点架构专属知识库，构建我们的译点 AI 智能小助手。

![](img/223b96479a4597dc7fce8ffaff650f11.png)

补充完善我们的应用信息，进行开场白及多轮对话等功能设置，问题优化等。

![](img/cd0e20e6e44ab31e0e6d522b48afb23d.png)

至此，我们的 AI 大模型，私有知识库，专属应用就都配置完成。这样，一个本地私有化知识库应用就完成了搭建！与此同时，我们就拥有了一个智能且私有化的 AI 知识库，节省了 baidu 或 google 一下的时间，大大提高了我们日常工作中的效率！

![](img/8c5afd9f0434fa959ac5e6251bfc06ee.png)

其实，这与早期所接触研发的知识库（对信息的归纳、概括与总结->数据客观存在-信息加工）领域相比来看，AI 在语义、语境搜索，上下文联想，召回率，相关性，准确度等各方面丝毫不逊色，尤其是在垂类的知识领域，AI 检索使得回答更精准（前提：提示词给得恰到好处，让机器成为你的好帮手），交互体验的效果也更受用户青睐！

![](img/92188ecf77558db3831b2246d81c797f.png)

当然，也需要持续关注应用的运行情况，并根据用户反馈和市场需求进行迭代和更新，这包括优化用户体验、增加新功能，安全隐私等。由于时间等问题，后续会抽空专门整理一些智能 AI 助手回答效果及调优的文章（根据后台童鞋粉丝的需求呼吁度）><
