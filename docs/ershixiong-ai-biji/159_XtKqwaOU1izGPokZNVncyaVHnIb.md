
# Vercel AI SDK 工具调用功能详解

> 来源：[https://qianniucity.feishu.cn/docx/SQpIdCjERoLewsxiRygcmDrLnn9](https://qianniucity.feishu.cn/docx/SQpIdCjERoLewsxiRygcmDrLnn9)

Vercel AI SDK 的工具调用功能为开发者提供了一种强大的方式，使语言模型能够执行特定任务并访问外部资源。这一功能极大地扩展了语言模型的能力范围，使其不仅限于生成文本，还可以执行各种外部操作，如获取天气信息、访问数据库、生成用户界面等。本文将全面解析 Vercel AI SDK 中的工具调用功能，包括其核心概念、实现方式、高级特性以及最佳实践。

## 工具的基本构成与定义

在 Vercel AI SDK 中，工具（`Tool`）是一个可以被模型调用以执行特定任务的对象。每个工具由三个关键元素组成，这些元素决定了工具的行为和使用方式。

首先，工具包含一个可选的描述（`description`），这一描述能够影响模型何时选择使用该工具。合理的描述能够帮助模型理解工具的用途和适用场景，从而在适当的时机调用正确的工具。

其次，工具定义了一个参数模式（`parameters`），它可以是 Zod 模式或 JSON 模式。这一模式不仅被语言模型消费以了解工具接收什么参数，还用于验证模型调用工具时提供的参数是否有效。

最后，工具可以包含一个可选的异步执行函数（`execute`），该函数接收工具调用的参数并产生特定类型的结果。值得注意的是，这个执行函数是可选的，因为在某些场景下，开发者可能希望将工具调用转发到客户端或队列，而不是在同一进程中执行。

Vercel AI SDK 提供了一个辅助函数 `tool` 来帮助开发者定义工具并推断其类型。在使用 `generateText` 或 `streamText` 函数时，工具作为一个对象传递，其中工具名称作为键，工具定义作为值。

## 工具定义示例

以下是一个定义天气查询工具的示例：

```
import { z } from 'zod';
import { generateText, tool } from 'ai';

const result = await generateText({
  model: yourModel,
  tools: {
    weather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
  prompt: 'What is the weather in San Francisco?',
});
```

在这个示例中，我们定义了一个名为 `weather` 的工具，它接受一个位置参数，并返回该位置的温度信息。

## 多步骤工具调用

Vercel AI SDK 的一个强大特性是支持多步骤工具调用，这通过 `maxSteps` 参数实现。当 `maxSteps` 设置为大于 1 的数值时，如果模型生成了工具调用，AI SDK 会将工具结果传回模型，触发新的生成，直到没有更多的工具调用或达到最大步骤数。

默认情况下，`generateText` 或 `streamText` 仅触发单一生成（`maxSteps: 1`）。这适用于许多场景，特别是当你可以依赖模型的训练数据生成响应时。然而，当提供工具时，模型现在可以选择生成普通文本响应或生成工具调用。如果模型生成工具调用，则该生成完成，该步骤结束。

在许多情况下，我们希望模型在工具执行后生成文本，总结工具结果或在用户查询的上下文中使用多个工具。这正是多步骤调用的用武之地。

## 多步骤调用示例

以下是一个两步过程的示例：

```
import { z } from 'zod';
import { generateText, tool } from 'ai';

const { text, steps } = await generateText({
  model: yourModel,
  tools: {
    weather: tool({
      description: 'Get the weather in a location',
      parameters: z.object({
        location: z.string().describe('The location to get the weather for'),
      }),
      execute: async ({ location }) => ({
        location,
        temperature: 72 + Math.floor(Math.random() * 21) - 10,
      }),
    }),
  },
  maxSteps: 5, // 允许最多5个步骤
  prompt: 'What is the weather in San Francisco?',
});
```

在这个例子中，第一步是模型接收提示并生成工具调用，执行工具；第二步是模型接收工具结果并生成考虑了工具结果的响应。

## 访问步骤信息

要访问中间工具调用和结果，可以使用结果对象中的 `steps` 属性或 `streamText` 的 `onFinish` 回调。这包含了每个步骤的文本、工具调用、工具结果等信息。

此外，可以提供 `onStepFinish` 回调，当步骤完成时触发，即该步骤的所有文本增量、工具调用和工具结果都可用时。

## 响应消息管理

在使用多步骤工具调用时，将生成的助手消息和工具消息添加到对话历史中是一个常见任务。`generateText` 和 `streamText` 都有一个 `response.messages` 属性，可用于将助手和工具消息添加到对话历史中。

这个属性包含一个 `CoreMessage` 对象数组，可以添加到对话历史中：

```
import { generateText } from 'ai';

const messages = [ // ... ];

const { response } = await generateText({
  // ...
  messages,
});

// 将响应消息添加到对话历史中
messages.push(...response.messages);
```

这种方式使开发者能够轻松地维护包含模型响应和工具交互的完整对话历史。

## 工具选择控制

Vercel AI SDK 允许通过 `toolChoice` 设置影响何时选择工具。它支持以下设置：

1. `auto`（默认）：模型可以选择是否以及选择哪个工具调用

1. `required`：模型必须调用工具，但可以选择调用哪个工具

1. `none`：模型不能调用工具

1. `{ type: 'tool', toolName: string }`：模型必须调用指定的工具

例如，强制模型调用工具：

```
import { z } from 'zod';
import { generateText, tool } from 'ai';

const result = await generateText({
  model: yourModel,
  tools: {
    weather: tool({
      // ...工具定义
    }),
  },
  toolChoice: 'required', // 强制模型调用工具
  prompt: 'What is the weather in San Francisco?',
});
```

这种控制机制使开发者能够更精确地指导模型的行为，根据特定场景的需要决定工具调用策略。

## 工具执行选项

调用工具时，可以向工具执行函数传递额外的选项作为第二个参数。这些选项提供了更多的上下文和控制能力。

## 工具调用 ID

工具调用的 ID 被转发到工具执行。这可以用于在流数据中发送与工具调用相关的信息：

```
import { StreamData, streamText, tool } from 'ai';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const data = new StreamData();

  const result = streamText({
    // ...
    tools: {
      myTool: tool({
        // ...
        execute: async (args, { toolCallId }) => {
          // 为工具调用返回自定义状态
          data.appendMessageAnnotation({
            type: 'tool-status',
            toolCallId,
            status: 'in-progress',
          });
          // ...
        },
      }),
    },
    onFinish() {
      data.close();
    },
  });

  return result.toDataStreamResponse({ data });
}
```

## 消息访问

发送给语言模型以启动包含工具调用的响应的消息被转发到工具执行。可以在 `execute` 函数的第二个参数中访问它们。在多步骤调用中，消息包含来自所有先前步骤的文本、工具调用和工具结果。

## 中止信号

`generateText` 和 `streamText` 的中止信号被转发到工具执行。可以在 `execute` 函数的第二个参数中访问它们，例如中止长时间运行的计算或将其转发到工具内部的 `fetch` 调用。

```
import { z } from 'zod';
import { generateText, tool } from 'ai';

const result = await generateText({
  model: yourModel,
  abortSignal: myAbortSignal, // 将被转发到工具的信号
  tools: {
    weather: tool({
      // ...
      execute: async ({ location }, { abortSignal }) => {
        return fetch(
          `https://api.weatherapi.com/v1/current.json?q=${location}`,
          { signal: abortSignal }, // 将中止信号转发给 fetch
        );
      },
    }),
  },
  prompt: 'What is the weather in San Francisco?',
});
```

## 类型安全与模块化

模块化代码通常需要定义类型以确保类型安全和可重用性。为此，AI SDK 提供了几个用于工具、工具调用和工具结果的辅助类型。

每个工具调用根据调用的工具类型化为 `ToolCall<NAME, ARGS>`。类似地，工具结果类型化为 `ToolResult<NAME, ARGS, RESULT>`。`streamText` 和 `generateText` 中的工具定义为 `ToolSet`。

类型推断辅助函数 `ToolCallUnion<TOOLS>` 和 `ToolResultUnion<TOOLS>` 可用于从工具中提取工具调用和工具结果类型。

```
import { openai } from '@ai-sdk/openai';
import { ToolCallUnion, ToolResultUnion, generateText, tool } from 'ai';
import { z } from 'zod';

const myToolSet = {
  firstTool: tool({
    // ...工具定义
  }),
  secondTool: tool({
    // ...工具定义
  }),
};

type MyToolCall = ToolCallUnion<typeof myToolSet>;
type MyToolResult = ToolResultUnion<typeof myToolSet>;

async function generateSomething(prompt: string): Promise<{
  text: string;
  toolCalls: Array<MyToolCall>; // 类型化的工具调用
  toolResults: Array<MyToolResult>; // 类型化的工具结果
}> {
  return generateText({
    model: openai('gpt-4o'),
    tools: myToolSet,
    prompt,
  });
}
```

这种强类型支持使开发人员能够构建更健壮、更易于维护的代码，特别是在大型项目中。

## 错误处理

AI SDK 有三种与工具调用相关的错误：

1. `NoSuchToolError`：模型尝试调用未在工具对象中定义的工具

1. `InvalidToolArgumentsError`：模型使用与工具参数不匹配的参数调用工具

1. `ToolExecutionError`：工具执行期间发生的错误

1. `ToolCallRepairError`：工具调用修复期间发生的错误

## 在 generateText 中处理错误

`generateText` 抛出错误，可以使用 `try/catch` 块处理：

```
try {
  const result = await generateText({ /* ... */ });
} catch (error) {
  if (NoSuchToolError.isInstance(error)) {
    // 处理无此工具错误
  } else if (InvalidToolArgumentsError.isInstance(error)) {
    // 处理无效工具参数错误
  } else if (ToolExecutionError.isInstance(error)) {
    // 处理工具执行错误
  } else {
    // 处理其他错误
  }
}
```

## 在 streamText 中处理错误

`streamText` 将错误作为完整流的一部分发送。错误部分包含错误对象。使用 `toDataStreamResponse` 时，可以传递 `getErrorMessage` 函数来提取错误消息并将其作为数据流响应的一部分转发。

## 工具调用修复

工具调用修复功能是实验性的，未来可能会发生变化。语言模型有时无法生成有效的工具调用，特别是当参数复杂或模型较小时。

开发者可以使用 `experimental_repairToolCall` 函数尝试用自定义函数修复工具调用。可以使用不同的策略来修复工具调用：

1. 使用具有结构化输出的模型生成参数

1. 将消息、系统提示和工具架构发送到更强大的模型以生成参数

1. 根据调用的工具提供更具体的修复指令

## 使用结构化输出模型进行修复



```
import { openai } from '@ai-sdk/openai';
import { generateObject, generateText, NoSuchToolError, tool } from 'ai';

const result = await generateText({
  model,
  tools,
  prompt,
  experimental_repairToolCall: async ({
    toolCall,
    tools,
    parameterSchema,
    error,
  }) => {
    if (NoSuchToolError.isInstance(error)) {
      return null; // 不尝试修复无效的工具名称
    }

    const tool = tools[toolCall.toolName];

    const { object: repairedArgs } = await generateObject({
      model: openai('gpt-4o', { structuredOutputs: true }),
      schema: tool.parameters,
      prompt: [
        `The model tried to call the tool "${toolCall.toolName}"` +
        ` with the following arguments:`,
        JSON.stringify(toolCall.args),
        `The tool accepts the following schema:`,
        JSON.stringify(parameterSchema(toolCall)),
        'Please fix the arguments.',
      ].join('\n'),
    });

    return { ...toolCall, args: JSON.stringify(repairedArgs) };
  },
});
```

这种修复策略特别有用，因为它利用了更强大的模型的能力来理解和纠正参数错误，从而提高工具调用的成功率。

## 结论与最佳实践

Vercel AI SDK 的工具调用功能为开发者提供了一种强大的方式，使语言模型能够与外部系统交互并执行特定任务。通过理解和利用工具定义、多步骤调用、类型安全和错误处理等核心概念，开发者可以构建复杂而强大的 AI 应用程序。

在实施工具调用时，应考虑以下最佳实践：

1. 提供清晰、具体的工具描述，帮助模型理解何时调用特定工具
2. 使用 `Zod` 或 JSON 模式精确定义工具参数，减少参数错误
3. 设置适当的 `maxSteps` 值，考虑任务的复杂性和所需的顺序步骤数
4. 利用类型系统确保类型安全和代码可维护性
5. 实现健壮的错误处理策略，优雅地处理各种工具调用错误
6. 对于复杂参数的工具，考虑实现工具调用修复功能

随着 AI 应用程序变得越来越复杂，工具调用将成为扩展语言模型能力的关键机制。通过掌握 Vercel AI SDK 提供的工具调用功能，开发者可以创建更智能、更功能丰富的应用程序，实现语言模型与外部世界的无缝集成。

### Citations:

1. [https://sdk.vercel.ai/docs/ai-sdk-core/tools-and-tool-calling](https://sdk.vercel.ai/docs/ai-sdk-core/tools-and-tool-calling)
