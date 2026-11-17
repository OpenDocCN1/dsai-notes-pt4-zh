
# 03-rules-template

# AI 接管开源项目 · Rules 文件模板（正反对照版）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/MOiTdLdMNohokLxqmqicaXRNndd](https://kq4b3vgg5b.feishu.cn/docx/MOiTdLdMNohokLxqmqicaXRNndd)

> 配套视频：《用 AI 编程工具接管一个完全陌生的开源项目》

> 作者：木羽Cheney （VibeCoding 实战课主讲老师）· 赋范大模型技术社区

> 用法：直接复制"正确写法"模板，按你的项目修改启动命令即可

---

## 核心原则：少即是多

`Claude Code` 官方最佳实践明确指出：

> "`CLAUDE.md` 太长时，Claude 会忽略一半规则。重要指令被噪声淹没。修复方法：无情地修剪——如果 Claude 不需要某条规则也能做对，删掉它。"

> — `Claude Code` 官方最佳实践

**翻译成人话**：规则文件不是说明书，是便签纸。只贴 AI 自己想不到的东西。

---

## 正确写法：最小化模板

### 模板 1：Node.js 全栈项目

文件位置：`.claude/rules/project.md` 或项目根目录的 `CLAUDE.md`

```
# 项目规则

## 启动命令
- 前端：cd frontend && npm run dev
- 后端：cd backend && npm start
- 同时启动（推荐）：npm run dev

## 必须知道的事
- 环境变量在 `.env` 文件中，从 `.env.example` 复制并填写
- 数据库是 SQLite，文件在 `data/app.db`，不要删除这个文件
- 前端端口 3000，后端端口 8080，不要随意修改端口号

## 高危操作
- 修改 `data/` 目录下的任何文件前，先告诉我
- 修改 `.env` 中的数据库连接字符串前，先备份 `.env`
```

**总计 12 行有效内容。** AI 每次任务开始时花 2 秒读完，不会遗漏任何一条。

### 模板 2：Python 项目

```
# 项目规则

## 启动命令
- 激活虚拟环境：source venv/bin/activate
- 启动服务：python main.py
- 启动开发模式：python main.py --debug

## 必须知道的事
- 依赖管理用 `requirements.txt`，安装命令：`pip install -r requirements.txt`
- 配置文件在 `config.yaml`，API Key 字段不要提交到 Git
- 模型下载目录在 `models/`，首次启动会自动下载

## 高危操作
- 不要删除 `models/` 目录下已下载的模型文件（重新下载很耗时）
- 修改 `config.yaml` 后必须重启服务才生效
```

### 模板 3：包含 AI 模型接入的项目

```
# 项目规则

## 启动命令
- 安装依赖：npm install
- 启动：npm start
- 开发模式：npm run dev

## 模型配置
- 配置位置：.env 文件中的 LLM_BASE_URL / LLM_API_KEY / LLM_MODEL
- 当前使用模型：qwen-plus（通义千问）
- API 格式：OpenAI 兼容（/v1/chat/completions）
- 不需要多模态能力

## 高危操作
- .env 中的 API Key 不要提交到 Git
- 切换模型时先确认新模型支持项目用到的 API 参数
```

---

## 错误写法：越写越乱的典型案例

### 案例 1：写了一大堆"项目结构"

```
# 项目规则

## 项目结构
- src/ 是源代码目录
  - src/components/ 是 React 组件
  - src/pages/ 是页面组件
  - src/hooks/ 是自定义 Hook
  - src/utils/ 是工具函数
  - src/api/ 是 API 调用层
  - src/store/ 是状态管理（Zustand）
- public/ 是静态资源
- server/ 是后端代码
  - server/routes/ 是路由
  - server/models/ 是数据模型
  - server/middleware/ 是中间件
- tests/ 是测试文件
- docs/ 是文档

## 技术栈
- 前端：React 18 + TypeScript 5.3 + Vite 5 + Tailwind CSS 3.4
- 后端：Node.js 20 + Express 4 + Prisma 5
- 数据库：PostgreSQL 16
- 测试：Vitest + Playwright
- 部署：Docker + GitHub Actions

## 代码规范
- 使用 ESLint + Prettier
- 组件用函数式写法，不用 Class
- 状态管理用 Zustand，不用 Redux
- CSS 用 Tailwind，不写自定义 CSS
- 变量命名用 camelCase，组件命名用 PascalCase
- 文件命名用 kebab-case
- 每个组件一个文件
- Props 必须定义 TypeScript 接口
...
（还能写 50 行）
```

**这有什么问题？**

| 问题 | 后果 |

| --- | --- |

| **项目结构描述** 🔴 | AI 看代码 2 秒就能知道目录结构，你写了 20 行等于浪费 AI 的注意力配额 |

| **技术栈说明** 🔴 | AI 看 `package.json` 就知道用了什么框架什么版本，你写的还可能和实际版本不一致 |

| **代码规范** 🟡 | 这些信息有用，但不应该放在主规则文件里。放到 `.claude/rules/coding-style.md` 作为独立文件 |

| **太长** 🔴 | 70+ 行，AI 每次任务都要读一遍，真正重要的启动命令淹没在噪声里 |

最致命的问题：**你改了代码但忘了更新规则文件**。比如你把 Zustand 换成了 Jotai，但规则里还写着"状态管理用 Zustand"，AI 就会按 Zustand 的方式写代码，越改越乱。

### 案例 2：把操作流程写进去了

```
# 项目规则

## 开发流程
1. 先从 main 分支拉取最新代码
2. 创建功能分支 feature/xxx
3. 在功能分支上开发
4. 开发完成后跑测试
5. 测试通过后提 PR
6. Code Review 通过后合并到 main

## 部署流程
1. 合并到 main 后自动触发 CI
2. CI 通过后自动构建 Docker 镜像
3. 推送到 Docker Hub
4. Kubernetes 自动拉取新镜像并滚动更新
```

**这有什么问题？**

这些是**流程文档**，不是**规则**。AI 不需要知道你的 CI/CD 怎么配置的，它的工作是帮你写代码和跑命令。

如果你想让 AI 遵守 Git 分支规范，只需要写一句：

```
## Git 规范
- 不要直接提交到 main 分支，总是创建功能分支
```

一句话够了，12 行流程说明完全多余。

## 对照表：什么该写，什么不该写

| 内容类型 | 该写？ | 放在哪？ | 理由 |

| --- | --- | --- | --- |

| 启动命令 | ✅ | 主规则文件 | AI 可能猜错启动方式（特别是有自定义参数时） |

| 高危操作提醒 | ✅ | 主规则文件 | 防止 AI 误删数据、误改配置 |

| 环境变量说明 | ✅ | 主规则文件 | `.env` 文件不一定有注释，AI 可能不知道某个变量是必填的 |

| 模型配置位置 | ✅ | 主规则文件 | 快速告诉 AI 在哪改，省得它全项目搜索 |

| 项目结构 | ❌ | 不写 | AI 看代码就知道 |

| 技术栈版本 | ❌ | 不写 | AI 看依赖文件就知道 |

| 代码规范 | ⚠️ | `.claude/rules/coding-style.md` | 有用，但不要放主文件，用路径匹配按需加载 |

| Git 流程 | ⚠️ | `.claude/rules/git.md` | 只写"一句话约束"，不要写完整流程 |

| 部署流程 | ❌ | 不写 | AI 通常不负责部署 |

| API 接口文档 | ❌ | 不写 | 太长了，放到 `.claude/skills/` 里按需加载 |

---

## 进阶：路径匹配规则

**当你的项目变大，需要更多规则时**，用独立规则文件 + 路径匹配代替一个巨大的主文件：

```
.claude/
├── rules/
│   ├── project.md          ← 主规则：启动命令 + 高危操作（所有场景加载）
│   ├── frontend.md         ← 前端规范（只在编辑 `src/` 时加载）
│   ├── backend.md          ← 后端规范（只在编辑 `server/` 时加载）
│   ├── testing.md          ← 测试规范（只在编辑 `tests/` 时加载）
│   └── git.md              ← Git 约束（只在执行 `git` 命令时加载）
```

每个文件开头加 YAML frontmatter 指定生效路径：

```
---
globs: ["src/**/*.tsx", "src/**/*.ts"]
---

# 前端代码规范
- 组件用函数式写法
- Props 定义 TypeScript 接口
- 样式用 Tailwind utility class
```

**这样 AI 编辑前端代码时自动加载前端规范，编辑后端代码时不加载——上下文始终精准。**

---

## 一句话总结

> 规则文件的最佳状态：你删掉它也不影响 AI 正常工作，但有了它 AI 不会犯低级错误。

如果你的规则文件有 100 行，问自己：去掉 80 行后 AI 还能正常工作吗？如果能，那 80 行就是噪声。

---

更多 AI 实战教程，关注赋范大模型技术社区。

![](img/03f635aae6dcd4a801f166b575d4e639.png)
