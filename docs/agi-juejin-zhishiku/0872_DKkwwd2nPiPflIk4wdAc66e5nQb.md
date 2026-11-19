
# RAG加速器：数据抽取服务

> 来源：[https://agijuejin.feishu.cn/docx/KmlCdxVfwo8NRyxTeSFc8nw2nTe](https://agijuejin.feishu.cn/docx/KmlCdxVfwo8NRyxTeSFc8nw2nTe)

> 作者：AI小智

> 原文：https://mp.weixin.qq.com/s/cXnCnyJZ_XwIhWf-48FHCg

> **❝**

> 本文译自[Use Case Accelerant: Extraction Service](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzg2ODA5NTM1OA==&action=getalbum&album_id=3115055723522015235&token=162966106&lang=zh_CN#wechat_redirect)一文。介绍了基于LLM优化数据抽取，提升RAG效能的实现思路，以及工程实践。本系列合集，点击链接查看

![](img/2b9b24ce183ae7dd7fa931771faf7a39.png)

今日，我们激动地公布了我们最新的开源项目——用例加速器：一项数据抽取服务。大型语言模型（LLM）极其擅长从非结构化数据中抽取出有结构的信息。我们在最近的几个版本中不断优化`LangChain`开源库中的数据抽取支持，现在，我们更进一步推出了一个起始知识库，帮助你打造专属的数据抽取应用工具。

这个起始知识库包括了一个基础网络应用程序模板，你可以基于此模板，打造出能让你团队中非技术成员也能轻松上手使用LLM进行数据抽取的自助式应用。

接下来我们会更详尽地介绍这项数据抽取服务，但在此之前，先来聊聊数据抽取这个用例以及LLM在其中能发挥哪些作用。

## 数据抽取用例

![](img/817286d5d430152d6405e526d05f0f3e.png)

为了从诸如PDF文档、幻灯片、网站以及产品评价和用户反馈等非结构化数据中汲取宝贵的商业洞察，企业不惜投入巨大的资源和精力。

已部署的数据抽取解决方案通常采用手工揉合自动化的混搭手段，自动化抽取部分或者是依靠手工定制规则，或者是使用特定的机器学习（ML）模型。

尽管自动化解决方案愈来愈偏向使用ML技术而不是基于规则的方法，意识到ML驱动下的系统通常更具扩展性和可维护性，之前的ML解决方案通常需要大量工作量来构建和维护，并且需要大量标注过的数据来训练模型，这成为了它们的一大痛点。

如今，大型语言模型（LLM）作为一种全新且强大的ML技术，展现出在信息抽取应用领域的潜能。只需向LLM提供合适的命令（即指令）和范例，它就能够适应特定的数据抽取任务。因此，LLM显著降低了采用AI驱动进行信息抽取的门槛。

新一代的信息抽取解决方案将构建在LLM之上。这些方案仍然需要人工介入，以向LLM提供反馈和指导，并可能需要一些自定义规则和启发式方法来处理特殊情况。然而，相比老一代方案，新方案在维护和扩展上都有显著提升。

## 基于LLM的解决方案

如何利用LLM进行信息抽取呢？下面，我们将介绍构建一个抽取服务需要的基本组件和要考虑的要点。

### 源数据

使用LLM抽取信息的第一步是将希望抽取信息的原始数据转换成文本格式。`LangChain`提供了数百种文档加载器，你可以依此将PDF等文件格式转换为文本格式。

加载文本后就需要考虑如何向LLM呈现这些文本。最直接的方式是将整个文本都呈现给LLM，适用于短文本。然而，对于较长的文本，这种方法有几个弊端。首先，长文本可能超出了LLM处理的上下文窗口范围。其次，就算全部文本都在处理窗口内，LLM在处理更长的输入时表现往往下降。再者，即使结果还可以，整个推断过程也会比较耗时。

一种更复杂的方法是将文本分为多个片段，只处理与LLM相关的片段。这会带来选择哪些片段是相关的问题。或者，也可以并行处理所有片段保证不遗漏重要内容。不管采取哪种方式，你都需要考虑如何将不同片段的结果合并处理，及如何处理跨片段断裂的信息。

### 明确抽取需求

如果要让LLM有效抽取信息，就必须明确告诉它你需要什么。

明确需求可以归纳为三个关键部分：

1. 描述你想抽取数据结构的模式（`Schema`）。
1. 提供抽取背景与指示的命令（`Prompt`）。
1. 展示文本信息抽取实例的样例（`Reference examples`）。

### 实现过程

了解了组成部分后，接下来是如何让它们一起工作。

我们此处要分享两个简单但强大的思路。

第一个要点是确保LLM以正确的格式回应。如果只是在命令中指示LLM输出JSON格式，它可能并不总会遵守。因此，函数调用已经成为确保LLM严格输出特定格式的一个新方法，而且相对靠谱。我们推荐使用此方法来提升性能。

你可以参考 结构化输出文档，其中高层次的`LangChain` API展示了不同LLM如何调用工具和函数。

另一个提升性能的方法是使用参考样例。尽管没有样例也能设置抽取问题，但我们发现实际操作中，将输入和对应输出的样例纳入其中通常大有裨益。有时候，这些样例比指示本身还更能有效指导LLM处理特定情况。

在我们的提取用例文档中，你可以找到更多细节，助你从LLMs中提取更好的性能。

## 服务细节

明白了这些基础之后，我们来看看我们的数据抽取服务到底提供了哪些东西？

该服务基于`FastAPI`和`Postgresql`搭建，并提供了一个标准的REST API接口。同时附带有`docker-compose`文件，方便你快速搭建服务环境。这个服务以一个基本的Web应用程序模板形态现身，你可以进一步开发扩展，为你的团队成员创建一个便捷的数据抽取应用程序。简而言之，它就是一个帮你开始应用开发的样板。当然，如果你不想做任何开发，也可以直接使用它进行数据抽取流程。

该API支持定义并持久化“抽取器”，这些抽取器包含了配置LLM抽取需要的关键部件：一个描述你想从数据中抽取的结构的图式（`Schema`），一个提供抽取上下文背景并振奋模型的指令（`Prompt`），以及展示一些抽取样例给模型的样例（`Reference examples`）。

此外，API提供了一个让你提交文件进行抽取的端点，使用已经定义的抽取器，以及另一个通过`RemoteRunnable`来让抽取服务作为更大`LangChain` Expression Language (`LCEL`) 链中的一环更容易使用的端点。

服务已经预设了基于`MIME`类型的解析器，并可支持`PDF`和`HTML`文档的解析。你还可以扩展该服务以支持其他文件类型（例如，`PowerPoint`），方法是使用现有的`LangChain`文档加载器和解析器。

设计抽取API时我们作了一个关键决定：设计它总能提取多个实体。通常在文本信息抽取时这是期望的行为，因为这让模型有可能在文字中存在对应信息时抽取多个实体，且在文本中没有相关信息时不会有任何提取动作。

你如何实际使用这个服务呢？

在上面的基础上，有几个方面是你需要考虑的。你可以使用`JSON`模式精确定义提取信息，也可以指定需合入的样例来提升提取结果的质量。你既可以在调用API时指定这些参数，也可以将抽取器和样例保存在数据库中，便于保存和分享。在传入文本时，你可以选择传入原始文本或者二进制文件。

## 使用示例

![](img/1bbc57101bdd558a7363cce73d2ff725.png)

让我们通过一个例子来演示如何使用这项服务。

首先，可以创建一个抽取器：

```bash
curl -X 'POST' \
  'http://localhost:8000/extractors' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "name": "Personal Information",
  "description": "Extract personal information",
  "schema": {
      "type": "object",
      "title": "Person",
      "required": [
        "name",
        "age"
      ],
      "properties": {
        "age": {
          "type": "integer",
          "title": "Age"
        },
        "name": {
          "type": "string",
          "title": "Name"
        }
      }
    },
  "instruction": "Extract personal information based on the user input."
}';
```

这段代码是做什么的呢？

首先，我们向服务发送了一个`name`参数——这是我们所创建的抽取器的一个简单可读名称，并不由LLM使用。同样，`description`参数也仅供记录，未在任何处理中使用。

服务使用的是`schema`参数。这是表示我们希望抽取对象结构的`JSON`模式。我们在此抽取一个包含两个必要属性的个人信息：`age`（一个整数）和`name`（一个字符串）。

服务还使用了`instruction`参数。这些指令与模式和文本一同发送至LLM，以提供额外的上下文提示以及指导LLM 应如何操作。

发送完毕后，我们会收到一个新创建的抽取器ID对应的回应：

```json
{
  "uuid": "32d5324a-8a48-4073-b57c-0a2ebfb0bf5e"
}
```

注册完成后，我们可以用如下方式发起请求：

```bash
curl -s -X 'POST' \
'http://localhost:8000/extract' \
-H 'accept: application/json' \
-H 'Content-Type: multipart/form-data' \
-F 'extractor_id=32d5324a-8a48-4073-b57c-0a2ebfb0bf5e' \
-F 'text=my name is chester and i am 20 years old. My name is eugene and I am 1 year older than chester.' \
-F 'mode=entire_document' \
-F 'file=' | jq .
```

我们传入了一个抽取器的 ID —— 这是我们在前一步骤中返回的 ID。

我们还传入了要抽取信息的文本内容。

我们指定了整个文件的提取模式`mode=entire_document`。如果我们要处理的文本很长，可能需要使用不同的处理方式 —— 这就是我们掌控该参数的地方。

最后，我们传入了一个空值的文件参数`file`，表明我们此次请求没有使用文件上传功能。

我们得到了如下响应：

```json
{
  "data": [
    {
      "name": "chester",
      "age": 20
    },
    {
      "name": "eugene",
      "age": 21
    }
  ]
}
```

## 结语

在这篇博文中，我们介绍了如何利用LLM从非结构化文本中抽取结构化数据的概念，展现了我们全面开源的知识库，即一项提供此类服务的平台，并透过一个简易的例子告诉你如何操作。若想深入了解更复杂的案例，你可以访问服务的说明文件。如需了解如何在不使用此服务的情况下通过`LangChain`执行数据抽取的详细信息，请查阅我们更新的用例文件。接下来一周内，我们将为这项服务添加一个直观易用的前端，并提供一个托管版本。我们非常期待你的反馈！

更多资源：

+ `LangChain` 数据抽取文档 — https://python.langchain.com/docs/use_cases/extraction

+ 结构化输出功能 — https://python.langchain.com/docs/guides/structured_output

+ 函数调用 — https://python.langchain.com/docs/modules/model_io/chat/function_calling

+ `LangChain`基准测试 — https://github.com/langchain-ai/langchain-benchmarks
