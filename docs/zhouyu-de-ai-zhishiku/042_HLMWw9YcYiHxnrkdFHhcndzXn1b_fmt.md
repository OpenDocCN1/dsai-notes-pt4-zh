
# GA和posthog数据跟踪工具接入教程

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/UWg6d1dOToyfZix1Mn2cNMcxnoc](https://xatc0v8uz5m.feishu.cn/docx/UWg6d1dOToyfZix1Mn2cNMcxnoc)

## 一、 Google Analytics (GA4) 接入流程

GA4 主要用于监控流量来源、用户归因及与 Google 广告生态的集成。

### 注册与基础配置

- **登录平台**：访问 [GA 平台](https://www.google.com/search?q=https://analytics.google.com/) 并完成账号注册。

- **创建数据流**：在设置中选择“网站”，输入你产品的正式版本链接（需删除前面的 `https://`）。

- **获取代码**：点击“创建并继续”，系统会弹出包含衡量 ID 和设置方法的抽屉。

### 代码接入与事件上报

- **Agent 协作方案**：如果你使用 AI 开发工具，可以提供特定提示词。

- **提示词示例**：“在产品里学员点击提交 Credits 申请表是一个关键事件，我需要上报一个 GA 自定义事件。”

- **核心逻辑**：将 GA 提供的全局网站标签（`gtag.js`）部署在网站的 `<head>` 中，并在关键交互处调用上报接口。

---

## 二、 PostHog 接入流程

PostHog 侧重于产品内用户行为分析，提供录屏、热力图、AB 测试等深度功能。

### 注册与安装

- **注册账号**：前往 [PostHog 官网](https://posthog.com/) 完成注册。

- **命令行安装**：在项目根目录执行以下命令：

```
npx -y @posthog/wizard@latest
```

### 环境配置

在项目的 `.env` 文件中添加以下环境变量（需替换为你的实际 Key）：

- `VITE_PUBLIC_POSTHOG_KEY=phc_wOpwAhodveBkWU8mimTubdm14S3sJ4DQaYr6fC9nusP`
- `VITE_PUBLIC_POSTHOG_HOST=https://us.i.posthog.com`

### 初始化代码实现

在项目的入口文件（如 `main.jsx` 或 `App.jsx`）中完成初始化：

```javascript
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import { PostHogProvider } from 'posthog-js/react'
import App from './App.jsx'

const options = {
  api_host: import.meta.env.VITE_PUBLIC_POSTHOG_HOST,
}

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <PostHogProvider apiKey={import.meta.env.VITE_PUBLIC_POSTHOG_KEY} options={options}>
      <App />
    </PostHogProvider>
  </StrictMode>
)
```

### 采集关键事件

在需要追踪的具体组件或逻辑中调用：

```javascript
posthog.capture('my_event_name', { property_key: 'property_value' })
```

---

## 三、 核心注意事项

1. **数据采集原则**：应采集所有重要事件及参数，并确保上报精准，避免多报或重报。

2. **环境区分**：建议在开发环境和生产环境使用不同的 Key，防止测试数据污染真实业务指标。

3. **隐私合规**：PostHog 支持自托管，如果对数据主权有极高要求，可考虑私有化部署。
