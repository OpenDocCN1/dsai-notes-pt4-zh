
# AI CODING作业3

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/Teo9dGILaofCssx5UbEcHKmenOe](https://xatc0v8uz5m.feishu.cn/docx/Teo9dGILaofCssx5UbEcHKmenOe)

# 神农AI识百草 - 智能中药材识别系统

基于先进AI技术的中药材智能识别平台，帮助用户快速准确识别中药材种类、了解功效与用法。

## 🎉 最新更新 (v1.1.0)

发布日期: 2025-01-30

### 🐛 重要修复

+ ✅ 修复自定义域名API调用问题：解决了在自定义域名（`mossecho.cn`）下`API Key`未配置的错误

+ ✅ 优化环境判断逻辑：改进了生产环境检测机制，确保在所有部署环境下正确运行

+ ✅ 增强API代理检测：提升了API代理的稳定性和可靠性

+ ✅ 完善CORS配置：优化了跨域请求处理，确保在各种域名下正常工作

### 🚀 性能提升

+ 改进了错误处理和用户提示

+ 增强了网络异常的处理能力

+ 优化了API调用的稳定性

支持的部署环境：

+ ✅ 本地开发环境

+ ✅ Vercel默认域名

+ ✅ Vercel自定义域名

+ ✅ 其他云平台部署

## 功能特点

### 核心功能

+ 🔍 智能识别：基于深度学习的图像识别技术

+ 📚 药材百科：丰富的中药材知识库

+ 🔎 智能搜索：支持名称、功效、症状等多维度搜索

+ ❤️ 收藏功能：保存感兴趣的药材信息

+ 📱 响应式设计：支持手机、平板、电脑等多种设备

### 用户系统

+ 👤 用户认证：邮箱注册/登录系统

+ 📊 使用统计：详细的使用次数和历史记录

+ 🔒 数据安全：用户数据云端安全存储

+ 🔄 数据同步：跨设备数据同步

### 会员系统

+ 🆓 免费体验：游客3次/天，注册用户10次/天

+ 👑 付费会员：无限制识别次数

+ 💎 多种套餐：月度/季度/年度会员选择

+ 🎁 会员特权：专属功能和优先支持

## 🚀 快速开始

### 环境要求

+ `Node.js` 18.0 或更高版本

+ `npm` 或 `pnpm` 包管理器

### 安装步骤

1. 克隆项目

   ```bash
   git clone https://github.com/your-username/herb-recognition-system.git
   cd herb-recognition-system
   ```

1. 安装依赖

   ```bash
   npm install
   # 或使用 pnpm
   pnpm install
   ```

1. 配置环境变量

   ```bash
   cp .env.example .env.local
   ```

   在 `.env.local` 文件中填入您的API配置：

   ```
   # 应用基础配置
   VITE_APP_NAME=神农AI识百草
   VITE_APP_VERSION=1.1.0
   VITE_APP_DESCRIPTION=基于AI的中药材智能识别系统
   # 豆包API配置（必需）
   VITE_DOUBAO_API_KEY=your_doubao_api_key_here
   VITE_DOUBAO_API_URL=https://ark.cn-beijing.volces.com/api/v3
   VITE_DOUBAO_MODEL=ep-20241218105028-xxxxx
   VITE_API_TIMEOUT=30000
   # 其他可选配置
   VITE_DEV_MODE=true
   VITE_LOG_LEVEL=debug
   ```

1. 启动开发服务器

   ```bash
   npm run dev
   # 或使用 pnpm
   pnpm dev
   ```

1. 访问应用

   打开浏览器访问 `http://localhost:5556`

## 🔧 API配置

### 豆包 Doubao API

本项目使用豆包（Doubao）AI服务进行药材识别。您需要：

1. 注册火山引擎账号

   + 访问 [火山引擎控制台](https://console.volcengine.com/)
   + 注册并完成实名认证

1. 开通豆包大模型服务

   + 在控制台中找到「豆包大模型」服务
   + 开通服务并创建推理接入点

1. 获取API配置信息

   + API Key：在「API密钥管理」中创建
   + API URL：通常为 `https://ark.cn-beijing.volces.com/api/v3`
   + Model：您创建的推理接入点ID（格式如：`ep-20241218105028-xxxxx`）

1. 配置环境变量

   + 复制 `.env.example` 为 `.env.local`
   + 填入获取的API配置信息
   + 确保 `.env.local` 文件不被提交到版本控制

## 📖 使用说明

### 药材识别

1. 点击“药材识别”页面

1. 上传药材图片（支持 JPG、PNG 格式）

1. 等待AI识别结果

1. 查看识别结果和详细信息

1. 可以收藏感兴趣的药材

### 药材搜索

1. 点击“药材搜索”页面

1. 输入药材名称、拼音或别名

1. 查看搜索结果

1. 点击药材名称查看详细信息

1. 使用收藏按钮收藏药材

### 药材百科

1. 点击“药材百科”页面

1. 浏览药材分类

1. 查看药材详细信息

1. 使用搜索功能快速定位

## 🛠️ 技术栈

+ 前端框架：React 18 + TypeScript

+ 构建工具：Vite

+ 样式框架：Tailwind CSS

+ 路由管理：React Router

+ 状态管理：React Hooks

+ AI服务：豆包 Doubao Vision API

+ 图标库：Lucide React

+ 通知组件：Sonner

## 📁 项目结构

```
src/
├── components/          # 可复用组件
│   ├── ImageUpload.tsx  # 图片上传组件
│   └── HistoryQuery.tsx # 历史查询组件
├── hooks/               # 自定义Hooks
│   └── useFavorites.ts  # 收藏功能Hook
├── pages/               # 页面组件
│   ├── Home.tsx         # 首页
│   ├── Recognition.tsx  # 识别页面
│   ├── Search.tsx       # 搜索页面
│   └── Encyclopedia.tsx # 百科页面
├── services/            # 服务层
│   ├── aiService.ts     # AI服务接口
│   └── historyService.ts# 历史记录服务
├── data/                # 数据文件
│   └── herbsDatabase.ts # 药材数据库
└── utils/               # 工具函数
```

## 🔒 安全说明

### API密钥保护

⚠️ 重要提醒：请务必保护好您的API密钥！

+ ✅ 正确做法：

  + 使用 `.env.local` 文件存储真实的API密钥

  + 将 `.env*` 文件加入 `.gitignore`（已配置）

  + 不要在代码中硬编码API密钥

  + 不要在公开场合分享包含密钥的配置文件

  + 定期轮换API密钥

+ ❌ 避免做法：

  + 不要将 `.env.local` 文件提交到Git

  + 不要在代码注释中写入真实密钥

  + 不要在截图中暴露密钥信息

  + 不要使用弱密钥或默认密钥

### 文件安全配置

项目已配置以下安全措施：

```
.env
.env.local
.env.development.local
.env.test.local
.env.production.local
```

这些文件已在 `.gitignore` 中配置。

### 部署时的安全注意事项

1. 本地开发：使用 `.env.local` 文件

1. 生产部署：在部署平台的环境变量设置中配置

1. 团队协作：通过安全渠道分享密钥，不要通过Git

1. 密钥泄露：如发现密钥泄露，立即在火山引擎控制台中删除并重新生成

## 🚀 部署

### Vercel 部署（推荐）

1. 准备代码

   ```bash
   git clone https://github.com/your-username/herb-recognition-system.git
   cd herb-recognition-system
   ```

1. 部署到Vercel

   + 访问 [Vercel](https://vercel.com/) 并登录
   + 点击 "New Project" 导入GitHub项目
   + 选择本项目仓库

1. 配置环境变量 在Vercel项目设置的 "Environment Variables" 中添加：

   ```
   VITE_DOUBAO_API_KEY=your_actual_api_key
   VITE_DOUBAO_API_URL=https://ark.cn-beijing.volces.com/api/v3
   VITE_DOUBAO_MODEL=your_actual_model_endpoint
   VITE_API_TIMEOUT=30000
   ```

1. 部署完成

   + Vercel会自动构建和部署
   + 获得生产环境URL

### Netlify 部署

1. 连接GitHub

   + 访问 [Netlify](https://netlify.com/) 并登录
   + 点击 "New site from Git"
   + 选择GitHub并授权

1. 配置构建设置

   ```
   Build command: npm run build
   Publish directory: dist
   ```

1. 设置环境变量 在 "Site settings" > "Environment variables" 中添加所需配置

### 自托管部署

1. 安装依赖

   ```bash
   npm install
   ```

1. 配置环境变量

   ```bash
   cp .env.example .env.production
   ```

   编辑 `.env.production` 文件

1. 构建生产版本

   ```bash
   npm run build
   ```

1. 预览生产版本

   ```bash
   npm run preview
   ```

1. 使用静态文件服务器

   ```bash
   npx serve dist
   ```

   或使用nginx等Web服务器

## 🤝 贡献指南

欢迎贡献代码！请遵循以下步骤：

1. Fork 本项目

1. 创建特性分支（`git checkout -b feature/AmazingFeature`）

1. 提交更改（`git commit -m 'Add some AmazingFeature'`）

1. 推送到分支（`git push origin feature/AmazingFeature`）

1. 创建 Pull Request

### 开发规范

+ 使用 TypeScript 进行类型安全开发

+ 遵循 ESLint 和 Prettier 代码规范

+ 编写清晰的提交信息

+ 为新功能添加适当的测试

## 📄 许可证

[MIT License](https://github.com/maxecho20/0830moss1zhongyao/blob/master/LICENSE) - 详见 `LICENSE` 文件

## 🙏 致谢

+ [豆包 Doubao](https://www.volcengine.com/product/doubao) - AI识别服务

+ [React](https://reactjs.org/) - 前端框架

+ [Tailwind CSS](https://tailwindcss.com/) - 样式框架

+ [Lucide](https://lucide.dev/) - 图标库

## 📞 联系我们

如果您有任何问题或建议，请通过以下方式联系：

+ 创建 [GitHub Issue](https://github.com/your-username/herb-recognition-system/issues)

+ 发送邮件至：your-email@example.com

## 🔄 更新日志

### v1.0.0 (2024-12-19)

+ ✅ 完成药材识别功能

+ ✅ 完成药材搜索功能

+ ✅ 完成药材百科功能

+ ✅ 添加收藏功能

+ ✅ 实现响应式设计

+ ✅ 完成安全配置

+ ✅ 准备开源发布

---

免责声明: 本系统提供的药材信息仅供参考，不能替代专业医师的诊断和建议。使用中药材前请咨询专业中医师。
