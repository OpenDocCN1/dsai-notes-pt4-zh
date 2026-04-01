# CLI升级小技巧

> 来源：[https://my.feishu.cn/docx/Q2zsdKa2lodH3dxcTMBcUaYfnBe](https://my.feishu.cn/docx/Q2zsdKa2lodH3dxcTMBcUaYfnBe)

## Codex 和 Gemini Cli安装

+   分别升级（npm 版）

```
codex: 
npm -g i @openai/codex@latest

gemini: 
npm -g i @google/gemini-cli@latest
```

+   一次性升级（npm 版）

```
npm -g i @openai/codex@latest @google/gemini-cli@latest
```

+   一次性验证

```
npm list -g @openai/codex @google/gemini-cli
```

## Claude Code原生版本升级安装

升级步骤

第一步备份配置：

```
cp -r ~/.claude ~/.claude.backup
```

第二步如果你用npm装的，先卸载：

```
npm uninstall -g @anthropic-ai/claude-code
```

第三步安装原生版本：

```
## Mac用户
brew install claude-code

## Linux用户
curl -fsSL https://claude.ai/install.sh | bash

## Windows PowerShell用户
irm https://claude.ai/install.ps1 | iex
```

第四步验证，用 claude --version命令，看到版本号就成功了。