
# 多模态RAG系统前端部署指南

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/F0YvdsbU1oamppxz3IPcN4bsnZf](https://kq4b3vgg5b.feishu.cn/docx/F0YvdsbU1oamppxz3IPcN4bsnZf)

## 课程说明

体验课内容节选自《2025大模型Agent智能体开发实战》(秋招冲刺班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(秋招冲刺班)

![](img/ba72d27bca84dc7f2ec626b1e15e4f4f.png)

![](img/2a075594e7a211d17ac51672d13efe3d.png)

公开课全套学习资料，已上传至网盘（https://pan.baidu.com/s/1vV-8ew5tAfZOfwG3c4WAFw 提取码: i345）需要更系统深入学习大模型vx可扫码⬆️添加助教咨询喔～

---

## 多模态RAG检索系统 - 前端部署指南

这是一个基于React + TypeScript + Tailwind CSS构建的多模态RAG（检索增强生成）系统前端界面。

## 🚀 快速开始

### 1. 本地开发部署

```
# 1. 克隆或下载代码到本地
# 将所有文件拷贝到您的项目目录中

# 2. 安装依赖
npm install

# 3. 启动开发服务器
npm run dev
```

开发服务器将在 `http://localhost:3000` 启动。

### 2. 生产环境部署

```
# 构建生产版本
npm run build

# 启动生产服务器
npm run start
```

## ⚙️ 环境配置

### 运行模式配置

本系统支持两种运行模式：

#### 🔸 模拟模式（默认）

无需真实后端即可体验所有功能，系统将：

+ 自动生成虚拟PDF文档页面
+ 模拟流式聊天响应
+ 展示完整的引用和解析功能
+ 提供真实的交互体验

当 `VITE_API_BASE_URL` 未设置或指向 `localhost` 时自动启用。

#### 🔸 真实后端模式

连接到您的实际后端服务：

创建 `.env.local` 文件在项目根目录：

```
# .env.local
VITE_API_BASE_URL=https://your-backend-domain.com/api/v1

# 或本地开发环境：
# VITE_API_BASE_URL=http://localhost:8001/api/v1
```

#### 切换模式的方法

1. 使用模拟模式（推荐用于演示）:

+ 删除或注释掉 `.env.local` 中的 `VITE_API_BASE_URL`
+ 系统将自动启用模拟功能

1. 连接真实后端:

+ 在 `.env.local` 中设置正确的 `VITE_API_BASE_URL`
+ 确保后端服务正在运行

### 常见后端端口配置

+ FastAPI开发服务器: 通常使用 8000 或 8001 端口
+ Flask开发服务器: 通常使用 5000 端口
+ Express/Node.js: 通常使用 3001 或 8080 端口
+ 生产环境: 通常使用 80 (HTTP) 或 443 (HTTPS) 端口

## 🎯 功能特性

### 当前支持的功能

**完整的用户界面**

+ 现代化深色主题设计
+ 响应式两列布局（聊天界面 + PDF面板）
+ 流式聊天界面与markdown渲染
+ PDF上传、预览和翻页功能
+ 实时后端连接状态检查

**智能模拟系统**

+ 🎯 完整的PDF上传和解析模拟
+ 🤖 智能AI对话响应（支持文档问答）
+ 📄 动态PDF页面生成和预览
+ 🔗 真实的引用链接和弹窗功能
+ ⚡ 流式打字效果和进度指示
+ 📊 向量索引构建和搜索模拟

**生产就绪特性**

+ TypeScript类型安全
+ 错误处理和用户反馈
+ 性能优化的组件架构
+ 可配置的API连接

### 后端API要求

为了实现完整功能，您的后端需要提供以下API端点：

```
GET  /api/v1/health                    # 健康检查
POST /api/v1/pdf/upload               # PDF文件上传
POST /api/v1/pdf/parse                # 开始PDF解析
GET  /api/v1/pdf/status               # 查询解析状态
GET  /api/v1/pdf/page                 # 获取PDF页面图片
POST /api/v1/index/build              # 构建向量索引
POST /api/v1/index/search             # 搜索向量索引
POST /api/v1/chat                     # SSE流式聊天
GET  /api/v1/pdf/chunk                # 获取引用块详情
POST /api/v1/chat/clear               # 清空聊天会话
```

## 📦 项目结构

```
├── App.tsx                 # 主应用组件
├── components/             # 所有React组件
│   ├── ChatInterface.tsx   # 聊天界面组件
│   ├── Header.tsx          # 顶部导航栏
│   ├── HealthCheck.tsx     # API状态检查
│   ├── MarkdownRenderer.tsx # Markdown渲染器
│   ├── PDFPanel.tsx        # PDF展示面板
│   └── ui/                 # ShadCN UI组件库
├── services/               # API服务层
│   └── api.ts              # 后端API调用封装
└── styles/                 # 样式文件
    └── globals.css         # 全局样式和主题
```

## 🛠️ 开发指南

### 技术栈

+ React 18 - 用户界面框架
+ TypeScript - 类型安全的JavaScript
+ Tailwind CSS v4 - 原子化CSS框架
+ ShadCN/UI - 现代化组件库
+ Lucide React - 图标库
+ Sonner - Toast通知组件

### 自定义配置

#### 修改主题色彩

编辑 `styles/globals.css` 中的CSS变量：

```
.dark {
  --primary: #3b82f6;        /* 主色调 */
  --secondary: rgba(51, 65, 85, 0.85); /* 次要色调 */
  --accent: rgba(59, 130, 246, 0.15);  /* 强调色 */
}
```

#### 添加新的API端点

在 `services/api.ts` 中添加新的API函数：

```
export async function yourNewApiCall(): Promise<YourResponse> {
  const response = await fetch(`${API_BASE_URL}/your-endpoint`);
  return response.json();
}
```

## 🚨 常见问题

### Q: 前端启动后显示"API Offline"？

**A**: 这完全正常！系统已自动启用模拟模式，您可以：

1. 🎮 立即体验: 上传任意PDF文件体验完整功能
1. 💬 测试聊天: 与AI进行对话，支持中英文
1. 📖 查看文档问答: 上传PDF后提问相关内容
1. 🔌 连接真实后端: 配置 `VITE_API_BASE_URL` 环境变量

### Q: 如何修改后端连接地址？

**A**: 创建 `.env.local` 文件并设置 `VITE_API_BASE_URL` 变量，或直接修改 `services/api.ts` 文件。

### Q: 部署到生产环境需要注意什么？

**A**:

1. 确保后端API地址配置正确
1. 检查CORS跨域设置
1. 使用HTTPS协议保证安全性
1. 配置合适的错误监控和日志记录

## 🔧 环境变量参考

```
# .env.local
VITE_API_BASE_URL=http://localhost:8001/api/v1  # 后端API地址
VITE_APP_TITLE=多模态RAG检索系统                  # 应用标题（可选）
```

## 📝 更新日志

+ v1.0.0 - 初始版本，完整的RAG系统前端界面
+ 支持PDF上传、解析、聊天和引用功能
+ 深色主题和现代化UI设计
+ 完善的错误处理和离线演示模式

---

作者: 九天Hector

项目: 多模态RAG检索系统演示界面

---

体验课内容节选自《2025大模型Agent智能体开发实战》(秋招冲刺班)完整版付费课程

体验课时间有限，若想深度学习大模型技术，欢迎大家报名由我主讲的《2025大模型Agent智能体开发实战》(秋招冲刺班)

![](img/076cf5686787077141a165ae6458eee7.png)

![](img/1782ccfad45862ed7c6b9d447492fed7.png)

此外，若是对大模型底层原理感兴趣，也欢迎报名由我和菜菜老师共同主讲的《2025大模型原理与实战课程》(秋招冲刺班)

大模型秋招冲刺班开班特惠进行中，直播间享五折特价+全套SVIP新班特定福利，合购还有更多优惠哦~详细信息扫码添加助教，回复“大模型”，即可领取课程大纲&查看课程详情👇

![](img/c76ee1bb980db726b83a427102fbcfe7.png)

《2025大模型Agent智能体开发实战》(秋招冲刺班)为【100+小时】体系大课，总共20大模块精讲精析，零基础直达大模型企业级应用！
