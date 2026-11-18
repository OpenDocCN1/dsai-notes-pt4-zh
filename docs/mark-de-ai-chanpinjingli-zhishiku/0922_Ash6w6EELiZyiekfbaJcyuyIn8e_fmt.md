
# 用 Claude Code 接入 Figma，1 小时重构整个小程序 UI

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/UMs4dPv6po8t1HxmA77c4Bhdnqd](https://qqs7y1hozd1.feishu.cn/docx/UMs4dPv6po8t1HxmA77c4Bhdnqd)

## 1、配置 Figma

先在 Figma 中创建 `token`，有了这个 `cc` 才能有权限操作。

点击「头像」- `settings` - `security` - `generate new token`

![](img/3ec23bd7e65f2f88da0edda3d1c5ada5.png)

下面核心给可读可写权限，如果你不是很清楚，可以全部都给问题不大。

```
## Files
- ✅ `file_content:read` — 必选，读取设计内容  
- ✅ `file_metadata:read` — 必选，读取文件信息  
- ✅ `file_versions:read` — 可选  
- ✅ `file_comments:read` — 可选  
- ✅ `file_comments:write` — 可选  

## Design systems
- ✅ `library_assets:read` — 读取组件和样式  
- ✅ `library_content:read` — 读取发布的组件库  
- ✅ `team_library_content:read` — 读取团队组件库  

## Development
- ✅ `file_dev_resources:read` — 读取开发资源  
- ✅ `file_dev_resources:write` — 如果需要 Code Connect 映射  

## Projects
- ✅ `projects:read` — 读取项目结构  

## Webhooks
- ❌ `webhooks:read` — 不需要  
- ❌ `webhooks:write` — 不需要
```

记得保存好 `token`，关闭后就没法复制了，只能建一个新的，不能查看。

![](img/1723a2f07ac44151f41ae4ec30f5daaa.png)

![](img/e94f5ee9d05d7efcb40b2bfb4ea4ceb6.png)

配置 `MCP`，我们让 `CC` 自己来完成。

![](img/e53597dd7be0bd90a875b7fa92eff61a.png)

配置完成后，最好重启下 `CC` 终端，然后打开，输入 `/mcp` 回车，选择 Figma 进行登录授权。

这样 `MCP server` 里面包含了 10 多 个工具。其中特别的是 `use_figma`，这个它会修改你的 Figma 文件内容，其他工具只能读的形式。

![](img/4fd150e622724c4611f4b4f0d88bf86e.png)

## 2、在 Figma 中 Drafts 新建 Design

新建一个画板后，在顶部栏或点击这个设计板，复制把链接给到 `cc` 帮你设计 UI。

![](img/27a5d4bfa59744b5d6c3a2bb61d4c62e.png)

## 3、实际效果

从登录页，到户用端，骑手，商家后台所有页面，都把 UI 设计得非常完整。

![](img/a126a2d60554bf39cf14f08decd1d263.png)

重视结果，设计开发合一，可观察行为。传统模式一去兮不复返了：探索 → 研究 → 模型 → 迭代 → 交付 → 开发 → 发布。这样环节流程太多，耗时周期数日。

从 23 年开始，生图视频，vibe coding 各个领域，到今天已经是第三个年头了，肉眼可见发展速度惊人。

尤其在 vibe coding 领域，现在 90% 人都不会写一行代码，只做架构工程化，模糊边界定义指导 ai，做 ai 的评审导师，评审它的输出结果，也许是该要掌握的。
