
# 使用 Coze IDE 创建插件

> 来源：[https://v11enp9ok1h.feishu.cn/docx/ShkUdmeX3oTUkyxEQRuc2w3mnue](https://v11enp9ok1h.feishu.cn/docx/ShkUdmeX3oTUkyxEQRuc2w3mnue)

在 Coze IDE 中，您可以借助 AI 轻松创建插件，无需关心开发环境。创建插件后，必须发布插件才可以被 Bot 使用。

Coze IDE 是扣子提供的在线编码和运行环境，允许您在多种技术栈中创建插件。待插件发布后，IDE 会帮助您托管运行代码，您无需关心环境配置、服务部署等步骤。此外，您可以使用 IDE 内置的 AI 助手获取代码方面的帮助，包括生成代码、修改代码、解释代码和添加注释。

+ 在个人空间下创建的插件，仅能被个人调用。

+ 在团队空间下创建的插件，能被团队成员调用。

+ 插件发布了新版本后，使用了这个插件的 Bot 会自动使用发布的最新版本。

## 操作步骤

1. 登录[扣子](https://www.coze.cn/home)。

1. 在左侧导航栏的**工作区**区域，选择进入指定团队。

1. 在页面顶部进入**插件**页面，或者在某一 Bot 的**编排**页面，找到**插件**区域并单击 `+` 图标。

1. 单击**创建插件**。

1. 在**新建插件对话**框，根据以下信息完成配置并单击确认。

+ **插件图标**：（可选）单击默认图标后，您可以上传本地图片文件作为新的图标。

+ **插件名称**：自定义插件名称，用于标识当前插件。建议输入清晰易理解的名称，便于大语言模型搜索与使用插件。

+ **插件描述**：插件的描述信息，一般用于记录当前插件的用途。

+ **插件工具创建方式**：选择**在 Coze IDE 中创建**。

+ **IDE 运行时**：选择 `Node.js` 或者 `Python3`。

![](img/ef7014abf4b94a6d9b6124a0c933719c.png)

1. 在插件详情页，单击**在 IDE 中创建工具**。

1. 在弹出的**创建工具对话**框，设置工具名称和介绍，以明确工具的用途，并单击确定。

工具名称和介绍越清晰，大语言模型就越能理解并使用它。创建后，您将跳转到 Coze IDE 页面进行编码。

1. （可选）在 IDE 左上角**工具列表**区域，单击 `+` 图标，向插件添加更多工具。

您还可以通过单击列表内某一工具的设置图标，来编辑、删除或重置代码。

![](img/87d49ec663610ab5b7769508f0723d13.png)

1. （可选）在 IDE 左下角**依赖包**区域，管理依赖包，所有工具共用该依赖列表。

您可以单击 `+` 或者单击**添加依赖**按钮，输入依赖包名称并选择版本（可通过依赖名@版本号的格式进行搜索），然后安装依赖包。安装日志显示在控制台区域。

![](img/52314c318455c27d78fa4906a9ae97c6.png)

1. 在页面中间**代码**区域，根据所选的 IDE 运行时，默认内置了代码模板。

+ 请勿删除或修改模板内的 `handler` 方法，否则将导致函数运行失败。

+ 返回内容必须为 JSON 对象。

以下提供了不同 IDE 运行时的代码模板，您可以参考模板内的方式获取输入参数、打印日志。

Node.js

```typescript
/**
 * Each file needs to export a function named `handler`. This function is the entrance to the Tool.
 * @param {Object} args.input - input parameters, you can get test input value by input.xxx.
 * @param {Object} args.logger - logger instance used to print logs, injected by runtime
 * @returns {*} The return data of the function, which should match the declared output parameters.
 * 
 * Remember to fill in input/output in Metadata, it helps LLM to recognize and use tool.
 */
export async function handler({ input, logger }: Args<Input>): Promise<Output> { 
 const name = input.name || "world"; 
 
 logger.info(name); 
 
 return { 
 message: `Hello, ${name}!`, 
 }; 
}; 
```

Python3

```python
# Python code example 
""" 
Each file needs to export a function named `handler`. This function is the entrance to the Tool. 
 
Parameters: 
args: parameters of the entry function. 
args.input - input parameters, you can get test input value by args.input.xxx. 
args.logger - logger instance used to print logs, injected by runtime. 
 
Remember to fill in input/output in Metadata , it helps LLM to recognize and use tool. 
 
Return: 
The return data of the function, which should match the declared output parameters. 
""" 
def handler(args: Args[Input])->Output: 
    # get name from input  
    name=args.input.name 
 
    # print name 
    args.logger.info(name) 
     
    return {"content": "Hello,"+ name } 
```

在编码时，支持通过快捷键（macOS 为 command + I、Windows 为 Ctrl + I）唤起 AI 助手，AI 助手可以帮助您：

+ 生成代码：输入期望的功能，由 AI 为您生成代码。

+ 修改代码：选中待修改的代码，唤起 AI 助手，由 AI 帮助您修改代码。

+ 解释代码：选中代码，唤起 AI 助手，输入 `/explain` 让 AI 解释代码。

+ 注释代码：选中代码，唤起 AI 助手，输入 `/doc` 让 AI 为选中的代码添加注释。

![](img/9bae8caacee56fab47241d6df439e5e4.png)

1. 单击**元数据**，配置工具的元数据。

元数据的作用是让大语言模型理解每个工具输入或输出参数有哪些、各个参数有何含义。当用户在使用该工具的 Bot 时，模型会根据工具的元数据信息，从用户问答中解析、提取出对应的输入参数，并选择调用该工具，流程图如下所示。因此，在工具的元数据内提供详细的参数说明，可以让大语言模型更准确的使用工具。

![](img/130c8cbe2bb649d2dc53b32b43b36c45.png)

以下是一个网页搜索工具的元数据，当 Bot 内添加了该工具后，模型会根据工具的元数据信息，在收到匹配的用户查询语句时，调用工具处理用户任务。例如，如果用户发送消息查询上海天气，Bot 将会使用该工具响应用户。

![](img/b4d1679355b92126d100b85826ec7bfe.png)

元数据配置说明：

| 配置项 | 描述 |

| --- | --- |

| **名称** | 工具名称。建议输入清晰易理解的名称，便于后续大语言模型搜索与使用工具。 |

| **描述** | 工具的描述信息，一般用于记录当前工具的用途。 |

| **启用** | 是否启用当前工具。使用说明： |

| **输入参数** | 当前工具对应接口的输入参数信息。准确、清晰易理解的参数名称、描述等信息，可以让大语言模型更准确的使用工具。 |

| **输出参数** | 当前工具对应接口的输出参数信息。准确、清晰易理解的参数名称、描述等信息，可以让大语言模型更准确的使用工具。 |

1. 在页面右侧单击**测试代码图标**并输入所需的参数，然后单击 `Run` 测试工具。

如果您在 `Metadata` 设置了输入参数，可单击自动生成图标，由 IDE 生成模拟数据，您只需要调整参数值即可进行测试。

![](img/a7c102ed54bd8c2cf87409f55e0f950c.png)

您可以在**控制台**区域查看运行日志、在**输出**区域查看运行结果，单击**更新输出参数**，IDE 会自动把输出结果中的参数，更新到元数据的输出参数中。

![](img/bc4d1c1f9396ce5b97cd33ca031b6ad7.png)

1. 测试完成后，在页面右上角单击**发布**。

1. 在弹出的**发布对话框**，核对工具信息，并设置是否启用工具，然后单击下一步。

1. 在**个人信息收集声明对话**框，如果该**工具**会收集、传输用户个人信息，则需要选择是，并根据实际情况选择具体收集的个人信息，否则选择否，最后单击发布。

![](img/175f52f07a36b28f483e13494d9bdc03.png)

## 上架到商店

您可以将创建的插件发布到扣子商店，让更多扣子用户发现、使用您的插件。详情请参考将插件上架到商店。
