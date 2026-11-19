
# AI 开发提示词

> 来源：[https://qianniucity.feishu.cn/docx/QYc5dCWGAoyloUxalu6cHpx9n6g](https://qianniucity.feishu.cn/docx/QYc5dCWGAoyloUxalu6cHpx9n6g)

## 项目需求

```
开发一个基于`Next.js`和`Tailwind CSS`的智能背单词应用，用户可以在智能背单词应用中输入多个单词，然后点击生成短文的按钮，通过`Ollama`中的`gemma3:12b-it-qat`模型生成包含用户输入单词的短文，以帮助用户更容易记忆单词。
```

## `.clinerules`

```
## 文档搜索

在需要查询 `Next.js` 和 `Tailwind CSS` 相关文档时，务必使用 `Context7` 以获取最新的、版本相关的文档信息。

### `Next.js` 文档搜索

当需要查询 `Next.js` 文档时，搜索 `Next.js` 的稳定版本文档。在你的提问中，明确包含 `use context7` 并指明需要 `Next.js` 的稳定版文档。例如：

use context7 搜索 `Next.js` 稳定版关于路由功能的文档

### `Tailwind CSS` 文档搜索

当需要查询 `Tailwind CSS` 文档时，搜索 `Tailwind CSS` 的最新版本文档。在你的提问中，明确包含 `use context7` 并指明需要 `Tailwind CSS` 的最新文档。例如：

use context7 搜索 `Tailwind CSS` 最新版关于响应式设计的文档
```

## `01-tech-stack.md`

```
# 技术栈偏好

在本项目中，我们优先使用以下技术栈进行开发：

- **前端框架**: `Next.js` (稳定版)。请优先使用最新稳定版本的 `Next.js` 来构建用户界面和路由。当生成或修改前端代码时，请优先考虑使用 `App Router` (位于 `/src/app` 目录下) 来构建用户界面和路由。
- **样式库**: `Tailwind CSS` (最新版)。所有的样式都应该使用最新版本的 `Tailwind CSS` 的工具类来实现。请务必参考 `tailwind.config.js` 文件（通常位于项目根目录或 `/src/` 目录下）以了解项目的 `Tailwind CSS` 配置。
- **编程语言**: `TypeScript`。所有新的和修改的代码都应该使用 `TypeScript` 编写，以确保类型安全和更好的代码维护性。
- **AI 模型运行**: `Ollama`。本项目将使用本地运行的 `Ollama` 来加载和运行 AI 模型。具体使用的模型是 `gemma3:12b-it-qat`。
- **包管理器**: 请根据项目已有的配置使用 `npm` 或 `yarn`。
```

## `02-project-structure.md`

```
- **React 组件**:
    - 使用 `Next.js App Router` 时，页面组件应位于 `/src/app` 目录下的相应路由文件夹中。
    - 可重用的 React 组件应放置在 `/src/components` 目录下，并按照功能或模块进行组织（例如，`/src/components/ui`，`/src/components/features/vocabulary`）。
- **样式文件**:
    - `Tailwind CSS` 的配置文件通常位于项目根目录的 `tailwind.config.js`，或者在 `/src/styles/` 目录下。请始终参考此文件以了解项目的 `Tailwind CSS` 配置。
    - 任何全局的 CSS 文件（例如，用于 `Tailwind CSS` 指令 `@tailwind base`, `@tailwind components`, `@tailwind utilities`）通常位于 `/src/app/globals.css`。
- **工具函数**: 实用（utility）函数应放置在 `/src/lib` 或 `/src/utils` 目录下。
- **类型定义**: `TypeScript` 的类型（types）和接口（interfaces）应定义在 `/src/types` 或 `@types` 目录下。
- **静态资源**: 静态资源（例如图片、字体）应放置在 `/public` 目录下。
- **API 路由**: 使用 `Next.js` API 路由时，API 接口应位于 `/src/app/api` 目录下，用于处理与 `Ollama` 的交互或其他后端逻辑。
```

## `03-code-style.md`

```
# 代码风格和模式

请遵循以下代码风格和常用模式：

- **组件命名**: React 组件应使用 `PascalCase` 命名 (例如，`VocabularyCard`, `WordInput`)。
- **函数命名**: 辅助函数应使用 `camelCase` 命名 (例如，`formatWord`, `generateStory`)。
- **变量命名**: 变量应使用 `camelCase` 命名 (例如，`userWords`, `generatedText`)。常量可以使用 `UPPER_SNAKE_CASE` (例如，`MAX_WORDS`)。
- **组件类型**: 除非有特殊理由，否则优先使用函数式组件和箭头函数语法。
- **状态管理**: 优先考虑使用 React 的 `useState` 和 `useEffect` 进行组件级别的状态管理。如果需要更复杂的状态管理，请根据具体情况考虑 `useContext` 或其他的轻量级状态管理库。
- **错误处理**: 在进行 API 调用和处理用户输入时，务必进行适当的错误处理。
```

## `04-styling.md`

```
# 样式指南

- **Tailwind CSS 优先**: 在编写组件样式时，始终优先使用 `Tailwind CSS` 的工具类直接在 `JSX` 中进行样式化。
- **自定义 CSS**: 如果需要自定义 `CSS`，请尽量通过在 `tailwind.config.js` 中扩展 `Tailwind` 的配置，或者使用 `Tailwind` 的 `@apply` 指令来组合 `Tailwind CSS` 的工具类。避免编写大量的完全自定义的 `CSS`。
- **Tailwind 配置**: 任何对 `Tailwind CSS` 的自定义（例如，添加新的颜色、字体、断点）都应该在 `tailwind.config.js` 文件中进行。
- **响应式设计**: 在设计页面和组件时，请始终考虑响应式设计，并利用 `Tailwind CSS` 的响应式前缀 (例如，`sm:`, `md:`, `lg:`) 来适配不同的屏幕尺寸。
```

## `05-0llama-config.md`

```
# Ollama 配置

本项目使用本地运行的 `Ollama` 服务来生成短文。请注意以下配置和步骤：

- **Ollama 服务**: 确保 `Ollama` 服务已经在本地运行，并且可以通过默认端口 `http://localhost:11434/` 访问.
- **模型**: 使用的模型是 `gemma3:12b-it-qat`。在与 `Ollama` 交互之前，请确保该模型已经通过 `Ollama` 下载 . 可以使用命令 `ollama run gemma3:3b-it-qat` （或者 `gemma3:7b-it-qat` 或 `gemma3:12b-it-qat`，根据实际下载的版本）来下载和运行模型。
- **API 交互**: 当需要生成包含用户输入单词的短文时，请在 `Next.js` 的 `API` 路由中实现与 `Ollama` `API` 的交互。可以使用 `fetch` 或其他 `HTTP` 库向 `Ollama` `API` 发送 `POST` 请求，请求生成文本。
- **API 端点**: `Ollama` 的文本生成 `API` 端点通常是 `/api/generate`。
- **请求体**: 发送给 `Ollama` `API` 的请求体应包含 `model` 参数（设置为 `gemma 3:12b-it-qat`）以及 `prompt` 参数，该 `prompt` 应该包含用户输入的单词，并清晰地指示模型生成一个包含这些单词的短文以帮助记忆。
- **示例 Prompt**: 一个示例 `prompt` 可能是：“请使用以下单词创作一个简短的故事：[用户输入的单词列表，用逗号分隔]。”
- **错误处理**: 在与 `Ollama` `API` 交互时，请务必处理可能出现的错误，例如网络连接错误或模型生成错误。
```

## `06-app-logic.md`

```
# 应用核心逻辑

本项目旨在创建一个智能背单词应用。以下是核心功能和 `Cline` 在开发过程中应该考虑的事项：

- **用户输入**: 应用需要提供用户输入多个单词的界面。
- **单词接收**: `Next.js` 前端需要能够接收用户输入的单词列表。
- **API 调用**: 前端需要调用后端的 `API` 路由，将用户输入的单词列表发送给后端。
- **Ollama 交互**: 后端 `API` 路由接收到单词列表后，需要构建合适的 `prompt` 并调用本地 `Ollama` 服务，请求生成包含这些单词的短文。
- **短文返回**: 后端 `API` 路由需要将 `Ollama` 生成的短文返回给前端。
- **短文展示**: 前端需要将生成的短文展示给用户。
- **用户记忆辅助**: 生成的短文应该自然流畅，能够将用户输入的单词融入其中，帮助用户在语境中记忆单词。
- **提示工程**: 在生成 `prompt` 时，可以尝试不同的提示策略，以获得更符合用户期望的短文。可以指示模型生成更具创意、情节性或与单词含义相关的短文。
- **迭代优化**: 根据生成短文的效果，不断优化 `prompt` 和应用逻辑。

请在开发过程中，优先考虑实现上述核心功能，并确保代码的可读性和可维护性。
```

## `07-important-files.md`

```
# 重要文件位置（请勿修改）

为了项目的安全性和稳定性，请指示 `Cline` 不要读取或修改 以下文件或目录：

- **环境变量文件**: `.env`，`.env.local`，`.env.development.local`，`.env.production.local` 等包含 `API` 密钥或其他敏感环境变量的文件。
- **Ollama 相关配置**: 任何 `Ollama` 的配置文件或模型存储目录（如果 `Cline` 需要直接操作这些文件）。通常情况下，`Cline` 只需要通过 `API` 与 `Ollama` 交互，不需要直接修改其配置。
- **构建输出目录**: 通常是 `/.next/`，`/build/`，`/dist/` 等。
- **包管理器的锁文件**: `package-lock.json` (npm) 或 `yarn.lock` (yarn)。

请确保 `Cline` 在执行任何操作时都遵守这些限制，以防止意外修改或暴露敏感信息。
```

## 对话框输入

```
开发智能单词应用
```
