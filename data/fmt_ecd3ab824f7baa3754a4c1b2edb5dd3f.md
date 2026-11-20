
## 技能系统

Bundled Skills（内置技能）：这些技能随项目一起发布，存储在特定的内置目录中。测试文件 `skills.build-workspace-skills-prompt.applies-bundled-allowlist-without-affecting-workspace-skills.test.ts` 验证了内置技能的白名单机制不会影响工作空间技能。

Plugin Skills（插件技能）：通过 `plugin-skills.ts` 加载的外部技能包。这些技能可以通过包管理器安装，扩展系统的能力。

Workspace Skills（工作空间技能）：用户在自己的工作空间中创建的自定义技能。测试文件 `skills.build-workspace-skills-prompt.prefers-workspace-skills-managed-skills.test.ts` 表明工作空间技能具有最高优先级，会覆盖同名的插件技能和内置技能。

### 技能的构建和执行

测试文件揭示了技能系统的工作流程：

`skills.buildworkspaceskillcommands.test.ts` 测试了技能命令的构建过程。每个技能会被转换为 `Agent` 可以调用的命令。

`skills.buildworkspaceskillsnapshot.test.ts` 测试了技能快照功能，这允许系统保存和恢复技能的状态。

`skills.resolveskillspromptforrun.test.ts` 测试了技能提示词的解析。当 `Agent` 需要执行一个技能时，系统会根据技能定义和当前上下文，构建一个包含指令和可用工具列表的完整提示词。

`skills.build-workspace-skills-prompt.syncs-merged-skills-into-target-workspace.test.ts` 显示系统支持技能同步，可以将多个来源的技能合并到目标工作空间。

## 沙箱系统实现

### 沙箱架构

`src/agents/sandbox/` 目录实现了基于 Docker 的沙箱隔离系统。这个系统确保 `Agent` 执行的代码和工具调用不会对主机系统造成安全威胁。

核心文件分为几个层次：

Docker 集成层：`docker.ts` 封装了 Docker API 的调用，`types.docker.ts` 定义了 Docker 相关的类型。这一层负责容器的创建、启动、停止和删除。

配置管理层：`config.ts` 定义了沙箱的配置选项，`config-hash.ts` 通过计算配置的哈希值()来确保沙箱环境的一致性。当配置改变时，系统会创建新的沙箱容器。

运行时管理层：`manage.ts` 实现了沙箱的生命周期管理，`runtime-status.ts` 提供运行时状态查询，`context.ts` 维护沙箱的执行上下文，`registry.ts` 管理沙箱实例的注册表。

浏览器支持层：`browser.ts` 实现了沙箱内浏览器的集成，`browser-bridges.ts` 提供了沙箱浏览器与外部系统的桥接机制。这使得 `Agent` 可以在隔离环境中安全地进行网页操作。

工作空间管理：`workspace.ts` 管理沙箱的工作空间，包括文件的挂载和权限控制。

安全策略：`tool-policy.ts` 定义了工具使用策略，规定了哪些工具可以在沙箱内执行，哪些必须在主机上执行。`tool-policy.test.ts` 包含了策略的测试用例。

维护功能：`prune.ts` 实现了沙箱的清理功能，定期删除不再使用的容器和镜像，防止磁盘空间耗尽。

### 沙箱工作流程

当 `Agent` 需要执行一个需要隔离的操作时（例如运行用户提供的代码），系统会：

1. 通过 `config-hash.ts` 计算当前配置的哈希值
1. 在 `registry.ts` 中查找是否已有匹配的沙箱实例
1. 如果不存在，通过 `docker.ts` 创建新的容器
1. 通过 `workspace.ts` 挂载必要的文件和目录
1. 在容器内执行操作
1. 通过 `runtime-status.ts` 监控执行状态
1. 操作完成后，根据策略决定是保持容器运行还是销毁

项目根目录的 `Dockerfile.sandbox` 和 `Dockerfile.sandbox-browser` 定义了两种沙箱镜像。前者是通用的代码执行环境，后者额外包含了 Chrome 浏览器，用于需要浏览器的任务。

### 安全机制

沙箱系统的安全性体现在多个方面：

进程隔离：通过 Docker 容器实现完全的进程隔离，沙箱内的代码无法直接访问主机资源。

网络隔离：可以配置容器的网络策略，限制沙箱的网络访问。

文件系统隔离：沙箱有独立的文件系统，只能访问明确挂载的目录。

资源限制：可以限制容器的 CPU、内存等资源使用，防止资源耗尽攻击。

工具策略：通过 `tool-policy.ts` 定义的策略，某些敏感工具（如系统配置修改）被禁止在沙箱内执行。

## 开发工具链

### 包管理

项目使用 `pnpm` 作为包管理器。`pnpm-workspace.yaml` 文件定义了工作空间配置，支持 Monorepo 结构。

`package.json` 中定义了完整的构建和开发脚本：

```
{
  "scripts": {
    "dev": "node scripts/run-node.mjs",
    "build": "tsc -p tsconfig.json && ...",
    "ui:build": "node scripts/ui.js build",
    "gateway:watch": "tsx watch src/cli/entry.ts gateway",
    "test": "vitest"
  }
}
```

### 代码质量工具

项目使用以下工具保证代码质量：

+   `Oxlint` (`.oxlintrc.json`) - 基于 Rust 的高性能代码检查工具

+   `Oxfmt` (`.oxfmtrc.jsonc`) - 代码格式化工具

+   `detect-secrets` (`.detect-secrets.cfg`) - 密钥泄露检测

+   `shellcheck` (`.shellcheckrc`) - Shell 脚本检查

+   `swiftlint` (`.swiftlint.yml`) - Swift 代码检查

### 测试体系

项目建立了完善的测试体系，包含多个测试配置文件：

+   `vitest.unit.config.ts` - 单元测试配置

+   `vitest.e2e.config.ts` - 端到端测试配置

+   `vitest.gateway.config.ts` - Gateway 专项测试配置

+   `vitest.extensions.config.ts` - 扩展功能测试配置

+   `vitest.live.config.ts` - 实时环境测试配置

从 `src/` 目录下大量的 `.test.ts` 文件可以看出，项目的测试覆盖率很高，几乎每个核心模块都有对应的测试用例。

### 持续集成

`.github/` 目录包含了 GitHub Actions 的工作流配置，实现了自动化的构建、测试和发布流程。

## 部署方案

### Docker 部署

项目提供了完整的 Docker 部署方案：

+   `Dockerfile` - 主应用的 Docker 镜像

+   `docker-compose.yml` - Docker Compose 配置

+   `docker-setup.sh` - Docker 环境设置脚本

### 云平台部署

`fly.toml` 文件提供了 Fly.io 平台的部署配置，用户可以将 `Gateway` 部署到云端，实现远程访问。

## 总结

Clawdbot 是一个架构清晰、模块化程度高的个人 AI 助手项目。通过分析其代码实现，可以总结出以下技术特点：

架构设计：采用中心化的 `Gateway` 控制平面，所有模块通过 WebSocket 与 `Gateway` 通信，实现了松耦合的架构。

插件化：渠道、工具、技能都采用插件化设计，具有很强的可扩展性。

安全机制：通过沙箱隔离、白名单控制、配对机制等多层安全措施，保障系统安全。

跨平台能力：通过节点系统和原生应用，实现了跨 macOS、iOS、Android 的设备控制能力。

工程实践：使用现代化的开发工具链（`pnpm`、`Oxlint`、`vitest`），建立了完善的测试体系和自动化流程。

从代码实现来看，Clawdbot 是一个工程质量很高的开源项目，其设计思路和实现方式值得学习和借鉴。

最后我们再整理下 `package.json` 中所有依赖的介绍：

## 生产依赖 (dependencies)

## 可选依赖 (optionalDependencies)

## 开发依赖 (devDependencies)
