
# gptpdf：一个简单巧妙的复杂pdf解析工具，提升RAG效果

> 来源：[https://agijuejin.feishu.cn/docx/Daw2dBkGbomUZCxA0qzcFYasn5d](https://agijuejin.feishu.cn/docx/Daw2dBkGbomUZCxA0qzcFYasn5d)

> 作者：ully | AI工程化

> 原文：https://mp.weixin.qq.com/s/jRjmrSojHqAOiLC_dzGG0w

在构建RAG应用时，一个核心的工作就是构建知识库，进而以便于在实际进行问答时能够更准确地检索到文档内有关于问题的相关上下文信息。而知识库文档的一大来源来自于pdf格式文件，这类文件通常是富文本的，包含图片，表格等，且无法直接解析，必须通过一些技术手段将其拆分识别形成可被后续处理的文本文件，如text或markdown。

“Quility in，Quility Out”是构建高质量RAG的指导思想，因此，文档处理流程第一步——加载解析就变得尤为重要，也是很多RAG框架体现其核心优势的地方之一，比如`RAGFlow` 的核心组件`DeepDoc`利用OCR、布局识别、表格解析等技术提取文字，图片等内容。`llamaindex`推出了`llamaparse`智能解析复杂内容的pdf，提取文字，图片，表格等信息，并默认输出为markdown格式文件。

![](img/1b26ddd7363edd16ee257409f3768685.png)

最近出现了一个新的工具`gptpdf`（https://github.com/CosmosShadow/gptpdf），创新的使用了视觉大语言模型（如 `GPT-4o`）将复杂PDF 解析为 markdown，实现非常简洁，核心代码仅有293行，但几乎可以完美地解析排版、数学公式、表格、图片、图表等，每页平均成本为0.013 美元（`GPT-4o`接口费用）。

## 核心实现流程

其核心实现流程分为两步，如下所示：

1、使用 `PyMuPDF` 库，对 PDF 进行解析出所有非文本区域，并做好标记。

![](img/10828c57d037ee3459273f3cfe6da107.png)

2、使用视觉大模型（如 `GPT-4o`）进行解析，得到 markdown 文件。

![](img/f69311bc2505408af61b9319cb0020a7.png)

## 提示词模板

`GPT-4o`的默认提示词为，对于别的大模型可以结合效果表现进行调整：

如果区域是表格或者图片，使用 ![]() 的形式插入到输出内容中，否则直接输出文字内容。

```
DEFAULT_PROMPT = """使用markdown语法，将图片中识别到的文字转换为markdown格式输出。你必须做到：
1. 输出和使用识别到的图片的相同的语言，例如，识别到英语的字段，输出的内容必须是英语。
2. 不要解释和输出无关的文字，直接输出图片中的内容。例如，严禁输出 “以下是我根据图片内容生成的markdown文本：”这样的例子，而是应该直接输出markdown。
3. 内容不要包含在```markdown ```中、段落公式使用 $$ $$ 的形式、行内公式使用 $ $ 的形式、忽略掉长直线、忽略掉页码。

再次强调，不要解释和输出无关的文字，直接输出图片中的内容。

"""

DEFAULT_RECT_PROMPT = """图片中用红色框和名称(%s)标注出了一些区域。

"""

DEFAULT_ROLE_PROMPT = """你是一个PDF文档解析器，使用markdown和latex语法输出图片的内容。

"""
```

整个使用也比较简单，可以方便地集成到现有的RAG流程中或是单独使用。

```
from gptpdf import parse_pdf

api_key = 'Your OpenAI API Key'

content, image_paths = parse_pdf(pdf_path, api_key=api_key)

print(content)
```

## gptpdf-ui

`gptpdf-ui`项目在此基础上封装为web服务 ，支持在线解析和预览。

![](img/febb349e77dfc4081881dbb9b82885b5.png)

## 小结

该工具使用简单，易于集成，感兴趣的开发者可以关注。
