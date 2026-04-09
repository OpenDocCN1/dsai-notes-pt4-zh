# 使用 MCP 进行代码执行：构建更高效的 AI Agent

> 来源：[https://feicaiclub.feishu.cn/docx/M2Zvdr2fEoTm41xgp50cTqLLnlh](https://feicaiclub.feishu.cn/docx/M2Zvdr2fEoTm41xgp50cTqLLnlh)

![](img/524015ff32adc3526e2a9298c12949ed.png)

Published Nov 04, 2025

**Direct tool calls consume context for each definition and result. Agents s**cale better by writing code to call tools instead. Here's how it works with MCP.

# 使用 MCP 进行代码执行：构建更高效的 AI Agent

[模型上下文协议（MCP）](https://modelcontextprotocol.io/) 是一个用于连接 AI Agent与外部系统的开放标准。传统上，将Agent连接到工具和数据需要为每对配对进行自定义集成，这会造成碎片化和重复劳动，使得难以扩展真正互联的系统。MCP 提供了一个通用协议——开发者只需在其Agent中实现一次 MCP，就能解锁整个集成生态系统。

自 2024 年 11 月推出 MCP 以[来，采用速度迅](https://github.com/modelcontextprotocol/servers)猛：社区已经构建了数千个 MC[P 服](https://modelcontextprotocol.io/docs/sdk)务器，所有主流编程语言都有可用的 SDK，行业已将 MCP 采纳为连接Agent与工具和数据的事实标准。

如今，开发者经常构建能够访问数十个 MCP 服务器上的数百或数千个工具的Agent。然而，随着连接工具数量的增长，预先加载所有工具定义并通过上下文窗口传递中间结果会减慢Agent速度并增加成本。

在这篇博客中，我们将探讨代码执行如何使Agent能够更高效地与 MCP 服务器交互，在使用更少 token 的同时处理更多工具。

## 工具带来的过度 token 消耗使Agent效率降低

随着 MCP 使用规模的扩大，有两种常见模式会增加Agent的成本和延迟：

+   工具定义使上下文窗口过载；

+   中间工具结果消耗额外的 token。

### 工具定义使上下文窗口过载

大多数 MCP 客户端会将所有工具定义预先直接加载到上下文中，使用直接工具调用语法将它们暴露给模型。这些工具定义可能如下所示：

```
gdrive.getDocument
描述：从 Google Drive 检索文档
参数：
  documentId（必需，字符串）：要检索的文档 ID
  fields（可选，字符串）：要返回的特定字段
返回：包含标题、正文内容、元数据、权限等的文档对象

salesforce.updateRecord
描述：更新 Salesforce 中的记录
参数：
  objectType（必需，字符串）：Salesforce 对象类型（潜在客户、联系人、账户等）
  recordId（必需，字符串）：要更新的记录 ID
  data（必需，对象）：要更新的字段及其新值
返回：带有确认的更新记录对象
```

工具描述占用更多上下文窗口空间，增加响应时间和成本。在Agent连接到数千个工具的情况下，它们需要在读取请求之前处理数十万个 token。

### 中间工具结果消耗额外的 token

大多数 MCP 客户端允许模型直接调用 MCP 工具。例如，你可能会问你的Agent："从 Google Drive 下载我的会议记录并将其附加到 Salesforce 潜在客户。"

模型将进行如下调用：

```
工具调用：gdrive.getDocument(documentId: &quot;abc123&quot;)
→ 返回 &quot;讨论了 Q4 目标...\n[完整记录文本]&quot;
（加载到模型上下文中）

工具调用：salesforce.updateRecord(
  objectType: &quot;SalesMeeting&quot;,
  recordId: &quot;00Q5f000001abcXYZ&quot;,
  data: { &quot;Notes&quot;: &quot;讨论了 Q4 目标...\n[完整记录文本]&quot; }
)
（模型需要再次将整个记录写入上下文）
```

每个中间结果都必须通过模型传递。在这个例子中，完整的通话记录流经两次。对于一个 2 小时的销售会议，这可能意味着处理额外的 50,000 个 token。更大的文档甚至可能超出上下文窗口限制，破坏工作流程。

对于大型文档或复杂数据结构，模型在工具调用之间复制数据时可能更容易出错。

## 使用 MCP 进行代码执行提高上下文效率

随着代码执行环境对Agent变得越来越普遍，一个解决方案是将 MCP 服务器呈现为代码 API 而不是直接工具调用。然后Agent可以编写代码与 MCP 服务器交互。这种方法解决了两个挑战：Agent可以只加载它们需要的工具，并在将结果传回模型之前在执行环境中处理数据。

有多种方法可以做到这一点。一种方法是从连接的 MCP 服务器生成所有可用工具的文件树。以下是使用 TypeScript 的实现：

```
servers
├── google-drive
│   ├── getDocument.ts
│   ├── ...（其他工具）
│   └── index.ts
├── salesforce
│   ├── updateRecord.ts
│   ├── ...（其他工具）
│   └── index.ts
└── ...（其他服务器）
```

然后每个工具对应一个文件，类似于：

```
// ./servers/google-drive/getDocument.ts
import { callMCPTool } from &quot;../../../client.js&quot;;

interface GetDocumentInput {
  documentId: string;
}

interface GetDocumentResponse {
  content: string;
}

/* 从 Google Drive 读取文档 */
export async function getDocument(input: GetDocumentInput): Promise&lt;GetDocumentResponse&gt; {
  return callMCPTool&lt;GetDocumentResponse&gt;(&#x27;google_drive__get_document&#x27;, input);
}
```

上面的 Google Drive 到 Salesforce 的例子变成了代码：

```
// 从 Google Docs 读取记录并添加到 Salesforce 潜在客户
import * as gdrive from &#x27;./servers/google-drive&#x27;;
import * as salesforce from &#x27;./servers/salesforce&#x27;;

const transcript = (await gdrive.getDocument({ documentId: &#x27;abc123&#x27; })).content;
await salesforce.updateRecord({
  objectType: &#x27;SalesMeeting&#x27;,
  recordId: &#x27;00Q5f000001abcXYZ&#x27;,
  data: { Notes: transcript }
});
```

Agent通过探索文件系统发现工具：列出 ./servers/ 目录以查找可用服务器（如 google-drive 和 salesforce），然后读取它需要的特定工具文件（如 getDocument.ts 和 updateRecord.ts）以了解每个工具的接口。这让Agent只加载当前任务需要的定义。这将 token 使用量从 150,000 个 token 减少到 2,000 个 token——节省了 98.7% 的时间和成本。

Cloudflare [发布了类似的发现](https://blog.cloudflare.com/code-mode/)，将使用 MCP 的代码执行称为"代码模式"。核心见解是相同的：LLM 擅长编写代码，开发者应该利用这一优势构建与 MCP 服务器更高效交互的Agent。

## 使用 MCP 进行代码执行的好处

使用 MCP 进行代码执行使Agent能够通过按需加载工具、在数据到达模型之前过滤数据以及在单个步骤中执行复杂逻辑来更高效地使用上下文。使用这种方法还有安全和状态管理方面的好处。

### 渐进式披露

模型非常擅长导航文件系统。将工具呈现为文件系统上的代码允许模型按需读取工具定义，而不是预先全部读取。

或者，可以向服务器添加一个 search_tools 工具来查找相关定义。例如，在使用上面使用的假设 Salesforce 服务器时，Agent搜索"salesforce"并只加载当前任务需要的那些工具。在 search_tools 工具中包含详细级别参数，允许Agent选择所需的详细级别（例如仅名称、名称和描述，或带有架构的完整定义）也有助于Agent节省上下文并高效找到工具。

### 上下文高效的工具结果

在处理大型数据集时，Agent可以在返回之前在代码中过滤和转换结果。考虑获取一个 10,000 行的电子表格：

```
// 没有代码执行 - 所有行都流经上下文
工具调用：gdrive.getSheet(sheetId: &#x27;abc123&#x27;)
→ 在上下文中返回 10,000 行以手动过滤

// 有代码执行 - 在执行环境中过滤
const allRows = await gdrive.getSheet({ sheetId: &#x27;abc123&#x27; });
const pendingOrders = allRows.filter(row =&gt;
  row[&quot;Status&quot;] === &#x27;pending&#x27;
);
console.log(`找到 ${pendingOrders.length} 个待处理订单`);
console.log(pendingOrders.slice(0, 5)); // 只记录前 5 个以供审查
```

Agent看到 5 行而不是 10,000 行。类似的模式适用于聚合、跨多个数据源的连接或提取特定字段——所有这些都不会使上下文窗口膨胀。

### 更强大且上下文高效的控制流

循环、条件和错误处理可以使用熟悉的代码模式完成，而不是链接单个工具调用。例如，如果你需要在 Slack 中发送部署通知，Agent可以编写：

```
let found = false;
while (!found) {
  const messages = await slack.getChannelHistory({ channel: &#x27;C123456&#x27; });
  found = messages.some(m =&gt; m.text.includes(&#x27;deployment complete&#x27;));
  if (!found) await new Promise(r =&gt; setTimeout(r, 5000));
}
console.log(&#x27;收到部署通知&#x27;);
```

这种方法比通过Agent循环在 MCP 工具调用和睡眠命令之间交替更高效。

此外，能够写出一个被执行的条件树也节省了"第一个 token 的时间"延迟：Agent不必等待模型评估 if 语句，而是可以让代码执行环境来完成这项工作。

### 保护隐私的操作

当Agent使用 MCP 进行代码执行时，中间结果默认保留在执行环境中。这样，Agent只能看到你明确记录或返回的内容，这意味着你不希望与模型共享的数据可以在工作流中流动，而无需进入模型的上下文。

对于更敏感的工作负载，Agent线束可以自动对敏感数据进行标记化。例如，假设你需要将电子表格中的客户联系详情导入 Salesforce。Agent编写：

```
const sheet = await gdrive.getSheet({ sheetId: &#x27;abc123&#x27; });
for (const row of sheet.rows) {
  await salesforce.updateRecord({
    objectType: &#x27;Lead&#x27;,
    recordId: row.salesforceId,
    data: {
      Email: row.email,
      Phone: row.phone,
      Name: row.name
    }
  });
}
console.log(`更新了 ${sheet.rows.length} 个潜在客户`);
```

MCP 客户端在数据到达模型之前拦截数据并对 PII 进行标记化：

```
// 如果Agent记录 sheet.rows，它会看到：
[
  { salesforceId: &#x27;00Q...&#x27;, email: &#x27;[EMAIL_1]&#x27;, phone: &#x27;[PHONE_1]&#x27;, name: &#x27;[NAME_1]&#x27; },
  { salesforceId: &#x27;00Q...&#x27;, email: &#x27;[EMAIL_2]&#x27;, phone: &#x27;[PHONE_2]&#x27;, name: &#x27;[NAME_2]&#x27; },
  ...
]
```

然后，当数据在另一个 MCP 工具调用中共享时，它通过 MCP 客户端中的查找进行去标记化。真实的电子邮件地址、电话号码和姓名从 Google Sheets 流向 Salesforce，但从不通过模型。这防止Agent意外记录或处理敏感数据。你还可以使用它来定义确定性的安全规则，选择数据可以流向和来自何处。

### 状态持久化和技能

具有文件系统访问权限的代码执行允许Agent跨操作维护状态。Agent可以将中间结果写入文件，使它们能够恢复工作并跟踪进度：

```
const leads = await salesforce.query({
  query: &#x27;SELECT Id, Email FROM Lead LIMIT 1000&#x27;
});
const csvData = leads.map(l =&gt; `${l.Id},${l.Email}`).join(&#x27;\n&#x27;);
await fs.writeFile(&#x27;./workspace/leads.csv&#x27;, csvData);

// 稍后的执行从停止的地方继续
const saved = await fs.readFile(&#x27;./workspace/leads.csv&#x27;, &#x27;utf-8&#x27;);
```

Agent还可以将自己的代码持久化为可重用的函数。一旦Agent为任务开发了工作代码，它就可以保存该实现以供将来使用：

```
// 在 ./skills/save-sheet-as-csv.ts 中
import * as gdrive from &#x27;./servers/google-drive&#x27;;

export async function saveSheetAsCsv(sheetId: string) {
  const data = await gdrive.getSheet({ sheetId });
  const csv = data.map(row =&gt; row.join(&#x27;,&#x27;)).join(&#x27;\n&#x27;);
  await fs.writeFile(`./workspace/sheet-${sheetId}.csv`, csv);
  return `./workspace/sheet-${sheetId}.csv`;
}

// 稍后，在任何Agent执行中：
import { saveSheetAsCsv } from &#x27;./skills/save-sheet-as-csv&#x27;;
const csvPath = await saveSheetAsCsv(&#x27;abc123&#x27;);
```

这与 [技能（Skills）](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) 的概念紧密相关，技能是可重用指令、脚本和资源的文件夹，供模型提高在专业任务上的性能。向这些保存的函数添加 SKILL.md 文件会创建一个模型可以参考和使用的结构化技能。随着时间的推移，这使你的Agent能够构建一个更高级能力的工具箱，演化它最有效工作所需的脚手架。

请注意，代码执行会引入其自身的复杂性。[运行](https://www.anthropic.com/engineering/claude-code-sandboxing)Agent生成的代码需要一个具有适当沙箱、资源限制和监控的安全执行环境。这些基础设施要求增加了直接工具调用可以避免的运营开销和安全考虑。代码执行的好处——降低 token 成本、降低延迟和改进工具组合——应该与这些实施成本进行权衡。

## 总结

MCP 为Agent连接到许多工具和系统提供了基础协议。然而，一旦连接的服务器过多，工具定义和结果可能会消耗过多的 token，降低Agent效率。

尽管这里的许多问题感觉很新颖——上下文管理、工具组合、状态持久化——它们都有来自软件工程的已知解决方案。代码执行将这些已[建立的模式应](https://modelcontextprotocol.io/community/communication)用于Agent，让它们使用熟悉的编程结构更高效地与 MCP 服务器交互。如果你实施了这种方法,我们鼓励你与 MCP 社区 分享你的发现。

## 致谢

本文由 Adam Jones 和 Conor Kelly 撰写。感谢 Jeremy Fox、Jerome Swannack、Stuart Ritchie、Molly Vorwerck、Matt Samuels 和 Maggie Vo 对本文草稿的反馈。