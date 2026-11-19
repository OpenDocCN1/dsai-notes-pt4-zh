
# 开发一个飞书：知识管理平台

> 来源：[https://xmgtoagi.feishu.cn/docx/WwkddaSIIovPuaxq2UfcDJASnjd](https://xmgtoagi.feishu.cn/docx/WwkddaSIIovPuaxq2UfcDJASnjd)

大家好，我是小码哥，90 后，一个喜欢在互联网折腾的人，对互联网有一种热爱之情，不亚于恋人，我说的热爱是愿意花时间、精力去研究东西，不知疲倦，同时也通过互联网搞钱，毕竟光喜欢还不够，还得变现，因为生活所迫，哈哈！不扯了，进入正题。

我一直有一个执念，想从头搭建一个自己的知识付费平台，把自己所学、所想、所获记录在内，就是一个博客，其实类似这种开源项目挺多，比如我试过不少开源博客（`WordPress`、`mogu`、`halo` 等等不下 10 种），`mogu`开源项目我向作者购买了源码，一开始觉得挺好的，用来学习挺不错，但是后来发现，维护成本有点高，因为 `mogu` 采用的是微服务架构，作为我来说，有点太沉重了，前后又试了好几个开源平台，但要么冗重、要么不可控、要么编辑内容时，后来飞书知识库给了我很多灵感，特别是他的“可见即可得”的编辑器，以及编辑器中很多组件，体验起来非常的丝滑，于是我用了一个多月的碎片时间，参考飞书知识库，搭建了自己的知识管理平台，名叫简享，一个简洁的内容分享平台，从零搭起的、可控的、多租户的知识付费平台。

## 一、前端预览

### （1）首页

设计风格采用现代化简约风格，导航栏包含知识分类、搜索、写文章入口、登录功能。

体验地址：http://www.easyjx.cn

![](img/edd1be5ffcabd956ae8e658419e7438d.png)

### （2）文章编辑器

点击写文章，即可进入文章编辑界面，也是为了这个编辑器，才开发了整个知识管理系统，同样，输入 `/` 即可弹出预设的组件，目前开发了几十种，后续根据实际情况，不断的完善。

#### 会员组件

我先介绍一个知识付费的组件，这个组件只有会员用户才能看到里面的内容，也就是文章发布后，这里面的内容，才有成为会员才能查看。

![](img/2f32c68640df26c31d753cc663898d75.png)

![](img/681cae9947842f7b31cf09a99cdfd301.png)

![](img/70b88c4601af9135fd32f617638c86d2.png)

#### `markdown`测试

完整预览可点击链接：http://www.easyjx.cn/article/126755/reader

![](img/33b668d6eb26888167d86b4007b8b1f4.png)

#### 其他组件

其他组件就不一一展示了整体效果下，

![](img/6c00d2e86dc3b6d613b4f50ef5c8d43e.png)

![](img/651fbdc4c410d33e426851abe89460fd.png)

![](img/bf8ccdaa320bb318caf90f6875cca0b4.png)

![](img/ffda28a84dca6deb96e47375bbdc7ba4.png)

可以看到，这是非常简单，主要采用 `markdown` 编辑器，可见及可得，当然 `html` 标签页可以识别。

![](img/0bfc8a91e7ea76bbd40d87c954af21c5.png)

右边可以对文章大纲和正文字数进行统计

![](img/5a595a823865edf413aef2a08a55d5b5.png)

### （3）文章发布

点击发布后，会弹出文章属性设置，文章的分类、摘要，另外我单独开发了文章访问控制权限，主要有是否允许复制、访问级别、访问密码，如果取消复制，那么用户就无法复制文章内容，如果启动密码，访问文章，比如通过密码才能访问。

![](img/270c47057d43695dad1a52ed0ca84bd1.png)

#### 禁止复制

禁止了所有复制方式，包括快捷键、右键复制，以及通过浏览器调试来复制，都进行了限制。如果用户想通过浏览器调试来复制，内容直接消失不见。

![](img/1256cf8dd9026882999716f81c3afb8f.png)

#### 密码访问

如果是启动了密码访问，并设置了密码，普通用户访问就需要输入密码了。

![](img/0181f39bbdb2b970321a7589a0d4b89b.png)

## 二、后端预览

后端是只有管理员才有权限访问的，包含了文章管理，入口如下：

![](img/7cf2fa0cd256b26ab633c65a97a89b0d.png)

### （1）仪表盘

### （2）文章管理

包括文章的是否允许复制、置顶、推荐、编辑、查看、下线、删除等功能

![](img/4d3318df7d7c86d7adc799e891965d0e.png)

### （3）文章分类

文章分类可以动态管理，动态展示

![](img/146957a6e207865d04ce6e0f2c064f00.png)

### （4）用户管理

![](img/9facf9126be5b74fb1fba29118dca441.png)

`vip` 用户在后台可以设置，并设置会员的有效期

![](img/2032e509f81ad754ffe14119af0df803.png)

### （5）用户角色

![](img/7b00cdd939f1f6ef94e23512acd2306b.png)

### （6）系统配置

这个地方很关键，文件服务器的配置、租户配置、网站基础信息、站长信息、

![](img/7a6dc6f2566953fb42225cb0e8a2ed05.png)

## 三、总体架构

### 技术架构

+ 整体采用 "前后端分离" 的技术架构

+ `后端` 以 `Spring Boot` + `MyBatis-Plus` 为核心，按业务模块拆分

+ `前端` 以 `Vue3` + `Vite` 为核心，辅以 `Pinia`、`Element Plus`、`Tailwind` 和 `Tiptap` 自研编辑器扩展

+ 多租户隔离、文件存储与预览、完整的后台管理，配合 `Nginx` 做简洁部署

+ 基本覆盖一个知识平台的全链路能力

### 项目结构

```
项目根目录/
├── backend/           # 后端 Maven 多模块工程
├── frontend/          # 前端 Vue3/Vite 工程
```

![](img/f25cb6849c06c7ad206fea76a9336d15.png)

![](img/b3c8fbab1200332feac63bee79e8f4f4.png)

### 技术栈详情

#### 后端技术栈

后端可以自行增加模块，模块之间通过 `Maven` 来管理，模块之间独立，开发更加灵活。

+ 核心框架：`Spring Boot` 2.7.x

+ 安全认证：`Spring Security`（`JWT`）

+ 数据持久层：`MyBatis-Plus`（配合 `PageHelper`）、`Druid` 连接池

+ 缓存：`Redis`

+ API 文档：`Knife4j`/`Swagger`

+ 工具类：`Lombok`、`MapStruct`、`Fastjson2`、`Hutool`

+ 数据库：`MySQL`（生产）

+ 文件存储：七牛云 SDK

#### 前端技术栈

+ 核心框架：`Vue 3` + `Vite`、`TypeScript`

+ 状态管理：`Pinia`

+ 路由：`Vue Router`

+ UI 组件：`Element Plus`

+ 样式系统：`Tailwind CSS`

+ 富文本编辑器：`Tiptap`（含自研扩展）

+ 测试：`Playwright`（E2E）、`Vitest`（单元测试）

### 运行与部署

+ 后端以传统 `jar` 运行，前端打包为静态资源，统一由 `Nginx` 托管

+ `Vite` 开发环境走 `/api` 代理到后端

+ 开发环境 `application-dev.yml` 使用本地 `MySQL`/`Redis`

+ 生产环境 `application-prod.yml` 使用云数据库与生产级 `Druid` 配置

## 后端详细说明

### 模块划分

### 核心配置

#### 依赖版本

+ `Spring Boot` 2.7.18

+ `MySQL` 驱动 8.0.x

+ `MyBatis-Plus` 3.5.x

+ `Druid` 1.2.x

+ `PageHelper` 1.4.x

#### 配置文件

```
# application.yml 通用配置
server:
  tomcat:
    threads:
      max: 200
      min-spare: 10

spring:
  servlet:
    multipart:
      max-file-size: 10MB
      max-request-size: 20MB

jwt:
  header: Authorization
  expireTime: 480
```

### 安全与权限

#### 认证机制

+ `JWT` 无状态登录，`Token` 放在 `Authorization` 请求头

+ `expireTime` 默认 8 小时超时

#### 权限控制

+ `Spring Security` + `RBAC` 权限模型

+ 菜单与权限码在系统模块定义

+ 开放路由在白名单中明确列出

### 多租户设计

+ 通过 `TenantContextHolder` 注入租户上下文

+ 接口层按租户隔离数据

+ 支持通过域名、当前上下文等方式识别租户

+ `ISysTenantConfigService` 提供租户配置查询服务

## 前端详细说明

### 项目结构

```
src/
├── components/          # 公共组件
│   └── editor/         # 富文本编辑器组件
├── views/              # 页面组件
│   ├── portal/         # 门户页面
│   └── admin/          # 后台管理页面
├── router/             # 路由配置
├── stores/             # 状态管理（Pinia）
├── api/                # API 接口封装
└── config/             # 配置文件
```

### 编辑器设计

#### 核心特性

+ 基于 `Tiptap` 深度定制

+ 命令面板（输入 `/` 呼出）

+ 气泡菜单（选中文本显示）

+ 多媒体支持（图片、视频、音频）

+ 表格编辑功能

+ `Markdown` 区块支持

+ 安全资源保护

#### 扩展组件

```
// 主要扩展列表
- 文本样式：Bold、Italic、Underline、Highlight
- 布局结构：BulletList、OrderedList、Blockquote
- 媒体支持：Image、Video、Audio、Upload
- 高级功能：Table、Iframe、SecureResource、MarkdownBlock
- 交互增强：CommandsMenu、HeadingSelector、Placeholder
```

### 内容保护机制

#### 前端保护指令

```
<template>
  <div v-should-protect-content="protectionConfig">
    <!-- 受保护的内容 -->
  </div>
</template>
```

#### 保护策略

+ 复制限制（禁止复制、右键菜单）

+ 选择限制（禁止文本选择）

+ 截图防护（CSS 干扰）

+ 按租户配置动态启用

## 安全策略

### 身份认证

+ `JWT` `Token` 认证机制

+ `Token` 超时自动刷新

+ 无状态会话管理

### 访问控制

+ `RBAC` 权限模型

+ 接口级别权限校验

+ 数据权限隔离

### 内容保护

+ 前端防复制保护

+ 后端接口权限控制

+ 敏感内容加密存储

### 防刷机制

+ 请求频率限制

+ 操作冷却期控制

+ 异常行为监控

## 部署与运维

### 环境配置

#### 开发环境

```
# application-dev.yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/simple-share
  redis:
    host: localhost
    port: 6379
```

#### 生产环境

```
# application-prod.yml
spring:
  datasource:
    url: jdbc:mysql://云数据库地址
    druid:
      # 生产级连接池配置
```

### Nginx 配置示例

```
server {
    listen 80;
    server_name your-domain.com;
    
    # 前端静态资源
    location / {
        root /path/to/frontend/dist;
        try_files $uri $uri/ /index.html;
    }
    
    # 后端 API 代理
    location /api {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### 监控建议

+ 日志按天切割，关键接口单独记录

+ `Nginx` 访问日志分析异常流量

+ 数据库定期备份（7/30 天保留策略）

+ 文件存储生命周期管理

## 与飞书编辑器的设计取舍

### 借鉴特性

+ ✅ 命令面板（输入 `/` 快速插入）

+ ✅ 气泡菜单（选中文本快捷操作）

+ ✅ 块级编辑体验

+ ✅ 多媒体内容无缝集成

### 差异化设计

+ 🔄 `Markdown` 块嵌入（非全量转换）

+ 🔄 安全资源扩展（权限控制内容）

+ 🔄 多租户架构支持

+ 🔄 自研表格组件（满足基础需求）

### 技术选型考量

+ 基于 `Tiptap` 的扩展性优势

+ `Vue 3` 组合式 API 的开发体验

+ `TypeScript` 的类型安全保障

+ 前后端分离的架构灵活性

## 未来规划

### 近期规划

+ [ ] 审核系统（楼层、审核、敏感词）

+ [ ] 评论系统（楼层、审核、敏感词）

+ [ ] 在线支付（楼层、审核、敏感词）

+ [ ] 互动功能完善（点赞、关注、收藏夹）

+ [ ] 搜索增强（全文检索、联想）

### 中期规划

+ [ ] 数据统计分析（阅读量、转化漏斗）

+ [ ] 商业化功能（付费解锁、会员体系）

+ [ ] 多租户主题定制

### 长期规划

+ [ ] 协同编辑（`WebSocket`/`OT`）

+ [ ] 内容治理（水印、防盗链）

+ [ ] 容器化部署（`Docker`）

## 结语

这个项目从一开始就不是为了堆功能，而是为了把 "写作体验" 和 "内容可控" 做到我满意的程度。飞书知识库的很多设计让我受益，但我更希望在自己掌控的技术栈里，让好的编辑体验自然延伸。技术上我尽量保持清晰的模块划分、合理的约定与可插拔的能力；产品上我坚持最小可用功能（`MVF`），把核心打稳，再逐步扩充。

如果你也想做一个自己的知识平台，可以联系我获取源码，如果你不懂技术，我也可以给你搭建一套属于你自己的知识管理平台。把你的内容体系、商业化逻辑、风格偏好都装进去。

![](img/19f08bccdc2d1be0f6139323c1314875.png)
