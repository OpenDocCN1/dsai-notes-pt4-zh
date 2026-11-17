
# AI-3002 使用 Azure AI 文档智能开发解决方案

> 来源：[`https://zsyhjtnsa5.feishu.cn/docx/AekrdGK2UobYwQxbRREcFp6ynHb`](https://zsyhjtnsa5.feishu.cn/docx/AekrdGK2UobYwQxbRREcFp6ynHb)

实验 repo：`https://github.com/microsoftlearning/mslearn-ai-information-extraction`

## 规划 Azure AI 文档智能解决方案

`https://learn.microsoft.com/zh-cn/training/modules/plan-form-recognizer-solution/`

### 什么是 Azure AI 文档智能？

Azure AI 文档智能是一项 Azure 服务，旨在自动化从各种已完成表单和文档中提取数据的过程。过去，将数据从表单输入到数据库或其他存储介质需要大量手动操作，这种方法效率低下、成本高昂且容易出错。AI 文档智能利用人工智能技术，高精度地识别打印或手写文本中的模式，从而提供一种更经济、更准确的替代方案，替代了传统的手动数据输入方式。它可以分析客户、合作伙伴或雇主完成的表单，并从中提取所需数据。

### Azure AI 文档智能与 Azure AI 视觉中的 OCR 功能有何区别？

虽然 Azure AI 文档智能和 Azure AI 视觉（尤其是其光学字符识别 - OCR 功能）都能从文档中提取文本，但它们的应用场景有所不同。Azure AI 视觉的 OCR 功能主要用于从文档的图片中提取简单的文字和文本，而不提供上下文信息，这适用于那些已有自己的分析代码或只需要基础文本提取的用户。

相比之下，Azure AI 文档智能提供了更复杂的文档分析功能。它不仅可以识别文本，还能识别键/值对、表格和特定于上下文的字段。如果您需要部署一个完整的文档分析解决方案，使用户能够提取并理解文档中的文本和其结构化信息，那么 Azure AI 文档智能是更合适的选择。

### Azure AI 文档智能如何利用模型来提取数据？

Azure AI 文档智能通过使用模型来识别您希望从文档中提取的数据类型。模型有助于提高结果的准确性并控制输出数据的结构。Azure AI 文档智能以 JSON 格式输出数据，这种格式与许多数据库、存储位置和编程语言兼容。

它提供了多种类型的模型：

+ 预生成模型： 这些模型适用于常见的表单和文档类型，例如发票、收据、W-2 税表、身份证件、名片和医疗保险卡等。使用预生成模型可以快速创建和部署解决方案，而无需训练自己的模型。它还包括用于通用文档分析的“读取”和“布局”模型。

+ 自定义模型： 对于结构异常或独特的表单，您可以创建自定义模型，并使用已完成表单的示例对其进行训练，以提取比预生成模型更具体的信息。

+ 组合模型： 您可以将多个自定义模型（针对不同类型的文档训练）关联到一个组合模型中。这样，用户可以将不同类型的表单提交到同一个服务，该服务将自动识别表单类型，并选择最合适的自定义模型进行分析。

### 如何在 Azure 订阅中创建 Azure AI 文档智能资源？

要在 Azure 中创建 Azure AI 文档智能资源，请遵循以下步骤：

1. 在 Azure 门户中，选择“创建资源”。

1. 在“搜索服务和市场”框中，键入“文档智能”，然后按“Enter”。

1. 在“文档智能”页中，选择“创建”。

1. 在“创建文档智能”页的“项目详细信息”下，选择您的订阅，然后选择或创建资源组。

1. 在“实例详细信息”下，选择离用户最近的“区域”。

1. 在“名称”文本框中，键入资源的唯一名称。

1. 选择“定价层”（可选择“免费（F0）”或“标准（S0）”），然后选择“查看 + 创建”。

1. 验证测试通过后，选择“创建”。

完成这些步骤后，Azure 将部署新的 Azure AI 文档智能资源。

### 连接到 Azure AI 文档智能资源需要哪些信息？

要编写使用 Azure AI 文档智能的应用程序，您需要两项关键信息来连接到您的资源：

+ 终结点 （Endpoint）： 这是可以联系您的资源的 URL 地址。

+ 访问密钥 （Access Key）： 这是一个唯一的字符串，Azure 使用它来验证对 Azure AI 文档智能的调用。

您可以通过以下步骤从 Azure 门户获取这些详细信息：

1. 在 Azure 门户中，导航到您的 Azure AI 文档智能资源。

1. 在“资源管理”下，选择“密钥和终结点”。

1. 复制“KEY 1”或“KEY 2”以及“终结点”的值，并将其存储在您的应用程序代码中。

### Azure AI 文档智能提供了哪些工具和 API 支持？

Azure AI 文档智能提供了多种工具和 API，以支持不同的开发需求：

+ Azure AI 文档智能工作室 （Azure AI Document Intelligence Studio）： 这是一个可视化工具，允许用户在不编写代码的情况下探索和理解 Azure AI 文档智能的功能。它可用于尝试分析文档，例如使用预生成模型来分析销售发票，并决定是否需要创建自定义模型。

+ API 和 SDK： 对于希望将 Azure AI 文档智能集成到自己的应用程序中的开发者，它提供了应用程序编程接口 （API），支持多种主流编程语言，包括 C#/。NET、Java、Python 和 JavaScript。如果需要使用其他语言，也可以通过其 RESTful Web 服务调用 Azure AI 文档智能。

### Microsoft 在设计和构建 AI 解决方案时遵循哪些负责任的 AI 原则？

Microsoft 在设计和构建 AI 解决方案时，遵循以下负责任的 AI 原则，这些原则也适用于使用 Azure AI 文档智能时：

+ 公平性： 所有 AI 系统应公平对待所有人，不受种族、信仰、性别、性取向或其他因素的影响。

+ 可靠性和安全性： 所有 AI 系统应提供具有可量化置信度的可靠答案，并确保安全运行。

+ 隐私和安全： 所有 AI 系统应保护敏感数据，并在适用的数据保护法律框架内运行。

+ 包容性： 所有 AI 系统应可供所有用户使用，无论其能力如何。

+ 透明： 所有 AI 系统应能够被理解，并且其操作应公开透明。

+ 问责： 所有 AI 系统应由对其行为负责的人员进行操作。

为遵守这些原则，使用 AI 文档智能时应彻底测试解决方案，确保只收集必要数据并获得用户同意，寻求法律建议，并持续人工检查和监控系统以防止损害和确保安全性。

### 在部署 Azure AI 文档智能解决方案时，有哪些需要考虑的方面以确保负责任的使用？

在部署 Azure AI 文档智能解决方案时，为了确保负责任的使用，应考虑以下方面：

+ 彻底测试： 确保使用您希望读取的表单彻底测试解决方案，以确保它能够提取所需的数据。

+ 数据收集和同意： 确保仅收集表单中所需的数据，并让用户同意存储和分析其信息。

+ 法律咨询： 尤其是在处理个人或敏感数据时，获取解决方案的法律建议至关重要。

+ 人工监督和干预： 继续使用人工代理检查部署的系统，并确保管理员可以干预解决方案以防止潜在的伤害。

+ 持续检查安全性： 持续检查系统的安全性及其针对恶意攻击和意外数据丢失的复原能力。

这些考虑因素有助于确保您的 AI 文档智能解决方案符合负责任的 AI 原则，并避免负面影响。

### 实验：Extract information from multimodal content

`https://github.com/MicrosoftLearning/mslearn-ai-information-extraction/blob/main/Instructions/Labs/01-content-understanding.md`

### 实验：Develop a Content Understanding client application

`https://github.com/MicrosoftLearning/mslearn-ai-information-extraction/blob/main/Instructions/Labs/02-content-understanding-api.md`

## 使用预生成的文档智能模型

`https://learn.microsoft.com/zh-cn/training/modules/use-prebuilt-form-recognizer-models/`

### 什么是 Azure AI 文档智能中的预生成模型？

预生成模型是 Microsoft 针对通用表单和文档类型（如发票、收据、税务表单、ID 文档等）进行训练和优化的 AI 模型。 用户无需自行训练模型，即可直接使用这些模型从各种文档中提取数据。 它们通过处理大量示例数据进行训练，从而能够在处理预期表单时提供准确可靠的结果。

### 预生成模型能提取哪些类型的数据？

预生成模型旨在从提交的文档中提取多种类型的数据。 这包括：

+ 文本提取：所有模型都能从手写和印刷文本中提取文本和单词行。

+ 键值对：许多模型能识别文档中标签或键及其对应响应或值，并将其提取为键值对（例如，"Weight" 和 "31 kg"）。

+ 实体：某些模型能提取包含常见、更复杂数据结构的文本作为实体，例如人名、地点和日期。

+ 选择标记：某些模型能提取指示选择的文本范围，如单选按钮和复选框。

+ 表格：许多模型能从扫描的表单中提取表格，包括单元格数据、行数、列数、列标题和行标题，甚至支持包含合并单元格的表格。

+ 字段：针对特定表单类型训练的模型能识别一组固定的字段值（例如，发票模型中的“客户名称”和“发票总额”）。

### 与自定义模型相比，使用预生成模型有哪些优势？

预生成模型的主要优势在于无需训练，这意味着可以节省大量的时间和资源。 Microsoft 已经对这些模型进行了大规模的训练和严格的测试，因此它们在处理常见文档类型时具有高准确性和可靠性。 此外，使用预生成模型通常会带来更低的成本，并且可以受益于定期的模型更新。 而自定义模型需要用户投入时间和资源来收集示例数据并进行训练，尽管它们可以针对行业特定或独特的表单类型提供更可靠的结果。

### 预生成模型对输入文档有哪些具体要求？

为了确保预生成模型返回准确和有用的结果，提交的文档需要满足以下要求：

+ 文件格式：支持 JPEG、PNG、BMP、TIFF 或 PDF。 读取模型还可以接受 Microsoft Office 文件。

+ 文件大小：标准层下文件必须小于 500 MB，免费层必须小于 4 MB。

+ 图像尺寸：图像尺寸必须在 50 x 50 像素到 10,000 x 10,000 像素之间。

+ PDF 文档尺寸：PDF 文档的尺寸必须小于 17 x 17 英寸或 A3 纸张大小。

+ PDF 保护：PDF 文档不得使用密码保护。

+ PDF 文本嵌入：建议提交嵌入文本的 PDF 文件，以减少字符识别错误。

+ 页数限制：PDF 和 TIFF 文件可以包含任意数量的页，但在标准层中只分析前 2,000 页，免费层中只分析前两页。

### Azure AI 文档智能中针对非结构化或半结构化文档提供了哪些预生成模型？

对于结构不那么明确或不可预测的文档，Azure AI 文档智能提供了以下预生成模型：

+ 读取模型：从文档和图像中提取印刷和手写文本，并能检测文本行的语言和类型（手写或印刷）。它是所有其他预生成模型文本提取的基础。

+ 通用文档模型：在读取模型的基础上，增加了对键值对、实体、选择标记和表格的检测。它能从结构化、半结构化和非结构化文档中提取这些值，并且是唯一支持实体提取的预生成模型（例如人名、组织、日期等）。

+ 布局模型：除了提取文本外，还能返回输入图像或 PDF 文件中的选择标记和表格结构信息。它擅长处理复杂表格结构（如合并单元格、不完整行列）和歪斜文档，并提供每个表格单元格的详细信息（内容、边界框、位置、标题状态和索引）。

### 针对财务和身份识别文档，Azure AI 文档智能提供了哪些特定预生成模型？

Azure AI 文档智能提供了多种针对特定财务和身份识别文档的预生成模型，它们能够从这些常见表单类型中提取特定字段：

+ 发票模型：从发票中提取客户名称、采购订单号、发票日期、供应商/客户详细信息、总金额、税款以及表格中的行项目详情（如产品描述、单价、数量、行总计等）。

+ 收据模型：从收据中提取商家详细信息（名称、电话、地址）、总金额、税款、小费、交易日期和时间，以及行项目详情。v3.0 及更高版本还支持单页酒店收据并提取入住/退房日期。

+ ID 文档模型：分析美国驾照和国际护照（仅限个人信息页），提取姓名、性别、出生日期、国籍、发证国家/地区、文档编号、机器可读区域等个人信息。

+ 名片模型：从名片中提取姓名、邮寄地址、电子邮件和网站地址以及各种电话号码。

此外，还有其他预生成模型，例如：美国税务模型（W-2、1098、1099、1040）、医疗保险卡模型、结婚证模型、信用卡/借记卡模型、抵押贷款文件模型、银行对账单模型和工资单存根模型。

### 如何在应用程序中使用预生成模型？

Azure AI 文档智能被设计为一个 RESTful Web 服务，可以通过支持 Web 服务的任何语言（如 C#/。NET、Java、Python、JavaScript）进行调用。要连接和认证服务，需要一个服务终结点（服务的 URL）和一个 API 密钥（唯一的访问密钥），这两个值都可以从 Azure 门户获取。由于服务响应可能需要几秒钟，建议使用异步调用来提交表单并获取分析结果。用户还可以使用 Azure AI 文档智能工作室来视觉化地探索和试验预生成模型的功能，这有助于设计和编写代码。

### 使用 ID 文档模型时需要注意哪些数据保护问题？

ID 文档模型提取的大多数数据都属于个人数据，具有敏感性质，并受大多数司法管辖区的数据保护法（如 GDPR）的约束。因此，在使用 ID 文档模型时，务必确保您拥有存储个人数据的合法权限，并按照处理此类信息的法律要求，遵守所有适用的数据保护和隐私法规。

### 实验：Analyze forms with prebuilt Azure AI Document Intelligence models

`https://github.com/MicrosoftLearning/mslearn-ai-information-extraction/blob/main/Instructions/Labs/03-prebuilt-doc-intelligence-model.md`



```python
from dotenv import load_dotenv
import os

# Add references
from azure.core.credentials import AzureKeyCredential
from azure.ai.formrecognizer import DocumentAnalysisClient


def main():

    # Clear the console
    os.system('cls' if os.name == 'nt' else 'clear')

    try:
        # Get config settings
        load_dotenv()
        endpoint = os.getenv('ENDPOINT')
        key = os.getenv('KEY')


        # Set analysis settings
        fileUri = "https://github.com/MicrosoftLearning/mslearn-ai-information-extraction/blob/main/Labfiles/prebuilt-doc-intelligence/sample-invoice/sample-invoice.pdf?raw=true"
        fileLocale = "en-US"
        fileModelId = "prebuilt-invoice"

        print(f"\nConnecting to Forms Recognizer at: {endpoint}")
        print(f"Analyzing invoice at: {fileUri}")


        # Create the client
        document_analysis_client = DocumentAnalysisClient(
            endpoint=endpoint, credential=AzureKeyCredential(key)
        )


        # Analyse the invoice
        poller = document_analysis_client.begin_analyze_document_from_url(
            fileModelId, fileUri, locale=fileLocale
        )


        # Display invoice information to the user
        receipts = poller.result()
        
        for idx, receipt in enumerate(receipts.documents):
        
            vendor_name = receipt.fields.get("VendorName")
            if vendor_name:
                print(f"\nVendor Name: {vendor_name.value}, with confidence {vendor_name.confidence}.")
        
            customer_name = receipt.fields.get("CustomerName")
            if customer_name:
                print(f"Customer Name: '{customer_name.value}, with confidence {customer_name.confidence}.")
        
        
            invoice_total = receipt.fields.get("InvoiceTotal")
            if invoice_total:
                print(f"Invoice Total: {invoice_total.value.symbol}{invoice_total.value.amount}, with confidence {invoice_total.confidence}.")


    except Exception as ex:
        print(ex)

    print("\nAnalysis complete.\n")

if __name__ == "__main__":
    main()
```

> **功能概述**

> 这段代码使用 Azure 的 Form Recognizer 服务（文档智能服务）来分析一个在线发票 PDF 文件，提取关键字段如供应商名称、客户名称和发票总金额，并将这些信息输出到控制台。

> **关键实现点**

1. **环境配置**：
   + 使用 `dotenv` 从 `.env` 文件加载 Azure 服务的终结点（`ENDPOINT`）和密钥（`KEY`）。

1. **客户端初始化**：
   + 创建 `DocumentAnalysisClient` 实例，用于与 Azure Form Recognizer 服务交互。

1. **文档分析**：
   + 通过 `begin_analyze_document_from_url` 方法异步分析指定 URL 的发票文档（使用预构建的 `prebuilt-invoice` 模型）。
   + 设置区域选项（`en-US`）以优化文本识别。

1. **结果提取与展示**：
   + 从分析结果中提取 `VendorName`、`CustomerName` 和 `InvoiceTotal` 字段的值和置信度。
   + 格式化输出字段信息（如货币符号和金额）。

1. **错误处理**：
   + 使用 `try-except` 捕获并打印可能的异常（如网络问题或认证失败）。

```
ENDPOINT="YOUR_ENDPOINT"
KEY="YOUR_KEY"
```

# 使用 Azure 文档智能从表单中提取数据

https://learn.microsoft.com/zh-cn/training/modules/work-form-recognizer/

## 什么是 Azure 文档智能？

Azure 文档智能是微软 Azure AI 服务之一，它利用光学字符识别（`OCR`）功能和深度学习模型，从各种文档中提取文本、键值对、选择标记和表格。它旨在帮助用户从文档中高准确度地提取数据，而无需从头开始构建复杂的深度学习模型，因为其基础模型已经过大量表单示例的训练。

## 如何访问 Azure 文档智能服务？

可以通过多种方式访问 Azure 文档智能服务：

+   `REST API`：允许任何支持 HTTP 请求的语言与服务交互。

+   客户端库 `SDK`：提供多种编程语言（如 `Python`、`.NET`）的特定 SDK，简化服务集成。

+   Azure 文档智能工作室：一个用户界面（在线工具），用于可视化浏览、理解和集成服务功能，例如分析表单布局、从预生成模型提取数据以及训练自定义模型。

+   Azure AI Foundry：另一个可能的访问点。

## 使用 Azure 文档智能有哪些文件输入要求？

为了确保顺利处理和分析，Azure 文档智能对输入文档有以下要求：

+   格式：必须是 `JPG`、`PNG`、`BMP`、`PDF`（文本或扫描）或 `TIFF` 格式。

+   文件大小：付费（`S0`）层的文件大小必须小于 500 MB，免费（`F0`）层的文件大小必须小于 4 MB。

+   图像尺寸：图像尺寸必须介于 50 x 50 像素与 10000 x 10000 像素之间。

+   训练数据集总大小：对于训练自定义模型，总大小必须为 500 页或更少。

## Azure 文档智能提供哪些主要组件或模型？

Azure 文档智能主要由以下组件组成，以满足不同的数据提取需求：

+   文档分析模型：接收 `JPEG`、`PNG`、`PDF` 和 `TIFF` 文件作为输入，并以 `JSON` 文件的形式返回边界框、文本内容、表格、选择标记（复选框或单选按钮）和文档结构中的文本位置。这包括布局模型、读取模型或通用文档模型。

+   预生成模型：这些是已经过训练的模型，可以直接用于从常见表单（如 `W-2` 表格、发票、收据、身份证件、医疗保险、疫苗接种和名片）中提取数据。它们无需额外训练，服务会分析文档并返回结构化的 `JSON` 输出。

+   自定义模型：用于从特定于企业的表单中提取数据。这些模型需要根据示例文档进行训练，训练完成后可以分析新文档并返回 `JSON` 输出。自定义模型又分为：

    +   自定义模板模型：准确提取标记的键值对、选择标记、表格、区域和签名，训练速度快，支持 100 多种语言。

    +   自定义神经模型：结合布局和语言功能的深度学习模型，最适合半结构化或非结构化文档。

## 如何训练自定义模型？

训练自定义模型有两种主要方法：

1. 使用 `API` 或 `SDK` 进行监督式机器学习：

   + 将示例表单以及包含布局和标签字段信息的 `JSON` 文件存储在 Azure `Blob` 容器中。

   + 可以使用 Azure 文档智能的“分析文档”功能为每个示例表单生成 `ocr.json` 文件。

   + 还需要一个描述要提取字段的单个 `fields.json` 文件，以及每个示例表单的 `labels.json` 文件，将字段映射到该表单中的位置。

   + 为容器生成共享访问安全（`SAS`）`URL`。

   + 使用“生成模型”`REST API` 函数（或等效的 `SDK` 方法）进行训练。

   + 使用“获取模型”`REST API` 函数（或等效的 `SDK` 方法）获取训练后的模型 ID。

1. 使用 Azure 文档智能工作室：

   + 创建 Azure 文档智能或 Azure AI 服务资源。

   + 收集至少 5-6 个用于训练的示例表单，并将其上传到存储帐户容器。

   + 配置跨域资源共享（`CORS`），使工作室能够将标记的文件存储在存储容器中。

   + 在工作室中创建自定义模型项目，并链接存储容器和 Azure 文档智能资源。

   + 使用工作室将标签应用于文本。

   + 训练模型，训练后会收到模型 ID 和平均准确度。

   + 通过分析未在训练中使用过的新表单来测试模型。

## 如何使用已训练好的自定义模型提取数据？

要使用自定义模型提取表单数据，您需要使用受支持的 `SDK` 或 `REST API` 的“分析文档”函数。在此函数中，您需要提供在模型训练期间生成的模型 ID 和要分析的文档的 `URI`。这个函数会启动表单分析过程。然后，您可以请求结果，以获取包含提取内容的 `analyzeResult` 对象，以及包含文档内容信息的页数组，通常以 `JSON` 格式返回。

## 如果提取数据的置信度分数较低怎么办？

如果 `analyzeResult` 的置信度值较低，可以尝试以下方法来提高数据提取的准确性：

+   提高输入文档的质量：确保文档清晰、扫描良好，没有模糊或遮挡。

+   确保表单相似性：检查正在分析的表单在外观上是否与训练集中的表单相似。如果表单外观差异很大，请考虑训练多个模型，每个模型专注于一种表单格式。

+   根据用例调整可接受的置信度：对于低风险应用程序，80% 或更高的置信度分数可能是可以接受的。然而，对于更敏感的情况，例如处理医疗记录或账单，通常建议追求接近 100% 的分数。

## Azure 文档智能使用什么技术来提取信息？

Azure 文档智能结合了以下核心技术来从文档中提取信息：

+   光学字符识别（`OCR`）：这是基础技术，用于检测图像中的文本，并创建围绕检测到的对象的边界框。边界框的位置会记录为相对于页面其余部分的坐标。

+   深度学习模型：服务利用预先训练的深度学习模型，这些模型在大量表单示例上进行了训练。这些模型能够识别键值对、选择标记、表格结构以及文档中的其他复杂关系。Azure 文档智能将 `OCR` 捕获的原始文件中的关系与深度学习模型的智能结合起来，以结构化形式返回边界框数据和其他提取的信息，从而实现高准确度的数据提取。

## 实验：Analyze forms with custom Azure AI Document Intelligence models

https://github.com/MicrosoftLearning/mslearn-ai-information-extraction/blob/main/Instructions/Labs/04-custom-doc-intelligence-model.md

# 创建组合的文档智能模型

## 什么是 Azure AI 文档智能中的组合模型？

组合模型是由多个自定义模型组成的一种模型类型。当用户提交表单进行分析时，Azure AI 文档智能会自动对表单进行分类，并选择最合适的自定义模型进行分析。这消除了用户需要手动指定正确模型版本的麻烦，特别适用于数据收集过程中存在多个略有不同表单版本的情况。

## 为什么需要使用组合模型？

在实际应用中，例如进行调查时，可能会存在不同版本的问卷同时使用的情况。如果每个版本都训练了单独的自定义模型，用户在提交表单时很容易选择错误的模型。组合模型解决了这个问题，它能够自动识别并使用最适合分析特定表单的自定义模型，从而简化了操作并提高了效率。

## Azure AI 文档智能支持哪两种类型的自定义模型？它们有何区别？

Azure AI 文档智能支持两种自定义模型：

+   自定义模板模型：适用于表单具有一致视觉模板的情况。这意味着表单的格式和布局在所有已完成的示例中都应保持一致。

+   自定义神经网络模型：适用于表单不一致、半结构化或非结构化的情况。这类模型能够处理布局和结构变化较大的表单。

## 组合模型与单个自定义模型在使用上有什么不同？

在分析表单时，使用组合模型和使用单个自定义模型的方法类似。主要区别在于，在使用组合模型时，你需要指定组合模型的模型 ID，而不是单个自定义模型的 ID。在分析结果中，可以通过检查 `docType` 字段来确定是哪个自定义模型实际用于分析该表单。

## 创建组合模型有哪些限制或兼容性要求？

创建组合模型时存在以下限制和兼容性要求：

+   自定义模板模型只能与其他自定义模板模型组合（支持 3.0 和 2.1 `API` 版本）。

+   自定义神经网络模型只能与其他自定义神经网络模型组合。

+   自定义神经网络模型不能与自定义模板模型组合。

+   单个组合模型中最多可以包含 100 个自定义模型。

## 如何在 Azure AI 文档智能工作室中创建组合模型？

在 Azure AI 文档智能工作室中创建组合模型的步骤如下：

1. 在主页上选择“自定义模型”。

1. 在“我的项目”下，选择一个自定义模型，然后在左侧导航中选择“模型”。

1. 在“模型”列表中，选择所有你想要包含在新组合模型中的模型。

1. 选择“撰写”。

1. 在“撰写新模型”对话框中，输入组合模型的“模型 ID”和“说明”，然后选择“撰写”。

## 如何通过代码创建组合模型？

通过代码创建组合模型，你需要使用 Azure AI 文档智能 `SDK`。首先，创建 `DocumentModelAdministrationClient` 的实例，并使用其终结点和 `API` 密钥连接到 Azure AI 文档智能。然后，将所有自定义模型的模型 ID 放入一个 `List<string>` 中，并将该列表传递给 `StartCreateComposedModelAsync()` 方法，如下所示：

```
string endpoint = "<endpoint>";
string apiKey = "<apiKey>";
var credential = new AzureKeyCredential(apiKey);
var client = new DocumentModelAdministrationClient(new Uri(endpoint), credential);

List<string> modelIds = new List<string>()
{
    firstCustomModel.ModelId,
    secondCustomModel.ModelId,
    thirdCustomModel.ModelId,
};

BuildModelOperation operation = await client.StartCreateComposedModelAsync(modelIds, modelDescription: "Composed model example");
Response<DocumentModel> operationResponse = await operation.WaitForCompletionAsync();
```

## 实验：Create an knowledge mining solution

https://github.com/MicrosoftLearning/mslearn-ai-information-extraction/blob/main/Instructions/Labs/05-knowledge-mining.md
