
# 使用`Claude Code`的`Slash Command`编排写作工作流

> 来源：https://xodnytdcaw.feishu.cn/docx/RBBmd99C6orHmexxr9ocTw92nib

## 下载知识库

新建文件夹，里面把往👸号问下全部下载

## 创建记忆文件`CLAUDE.md`

在这个目录下启动 `Claude Code`

```
Claude
```

创建记忆文件`CLAUDE.md`

> 记忆文件存放位置说明

> 项目记忆

> 项目根目录 `./CLAUDE.md`（推荐）

> 全局记忆

> `%USERPROFILE%\.claude\CLAUDE.md`（Windows）

> `~/.claude/CLAUDE.md`（macOS/Linux）

建配合`Slash Commands`写作自定义命令使用

```
/init 总结我的公众号文章写作风格，常用表达库，形成我的写作模板，后续我需要参考写文章
```

## 创建`Slash Commands`

`Slash Commands`即自定义斜杠命令。

比如我创建一个`/search-web`命令用来搜索资料用

> 环境准备

```
mkdir -p ~/.claude/commands     #创建命令目录（递归创建，不存在则自动生成）
touch ~/.claude/commands/search-web.md   # 创建命令文件（以search-web.md为例）
```

编写命令文件（Markdown 格式）

每个命令对应一个 `.md` 文件，包含元数据与执行逻辑（示例）

```
---
# 元数据（YAML格式，必须放在开头）
description: &quot;调用搜索工具查询指定关键词的网页信息&quot; # 命令描述
argument-hint: &quot;搜索关键词（如：Slash Commands 最佳实践）&quot; # 参数提示
allowed-tools: [&quot;Search&quot;] # 允许调用的工具（如Search、Read、Edit）
---
# 命令执行逻辑（提示词模板，$ARGUMENTS为参数占位符）
请使用搜索工具查询以下关键词的最新信息，并整理成结构化摘要：
$ARGUMENTS
```

全局命令：`~/.claude/commands/` 下，所有项目可用。

项目命令：项目根目录`.claude/commands/` 下，仅当前项目可用。

调用方式：在 `Claude Code` 中输入 `/search-web` `Slash Commands` 自定义方法，自动替换 `$ARGUMENTS` 为传入参数。

### `search-web`：自动搜集资料

```
/search-web  Claude Code上手教程
```

可以使用 `Claude Code` 的 `Slash Commands` 写一个自定义命令，辅助资料的搜集

> 辅助参考 https://zhuanlan.zhihu.com/p/1943813633428689089

`.md` 文件命令文件提示内容

```
在互联网上搜索关于 [$ARGUMENTS](https://x.com/search?q=%24ARGUMENTS&amp;src=cashtag_click) 的内容，然后把最靠[谱、最能解答我疑问的信息都](https://x.com/)给我扒出[来！

# 搜索要求
1. 主](https://t.co/Mpgo6UuhRG)要关注 http://anthropic.com, http://x.com/, http://reddit.com, http://linux.do 上的资料和动态
2. 需要搜索最新的消息，比如当前时间月份的
3. 使用 gemini cli 的 GoogleSearch 搜素资料
4. 可以启动多个并行的子代理加快各个执行的速度
5. 最后总结的时候需要 ultrathink
```

### `article`：一键写文章

```
/article 使用claude code的slash commands，撰写一篇关于Claude Code新手教程
```

`.md` 文件命令文件提示内容

```
根据主题创建公众号文章：$ARGUMENTS
严格遵循：
1. 参考 CLAUDE.md 的写作风格
2. 准备2-3个备选标题
3. 1000-1500字
4. 包含效果展示、问题描述、步骤教学
5. 结尾：好了，今天的分享就到这里。安啦，各位！
```

### 去标题

```
根据输入的内容：$ARGUMENTS，生成 5 个吸引人公众爆款文章的标题
```

## 常用内置命令

| 命令 | 解释 |

| --- | --- |

| `/add-dir` | 添加工作目录 |

| `/clear` | 清空对话历史 |

| `/compact [instructions]` | 精简对话内容，还可以指定重点方向 |

| `/config` | 查看/修改配置 |

| `/doctor` | 给你的 `Claude Code` 做个全面体检 |

| `/login` | 登录账号 |

| `/logout` | 退出账号 |

| `/mcp` | 查看 `mcp` 的配置 |

| `/memory` | 编辑 `CLAUDE.md` 文件 |

| `/model` | 选择使用的模型 |

| `/permissions` | 设置允许/禁止的工具权限 |

| `/status` | 查看账户和系统状态 |

| `/config` | 设置 |

| `/resume` | 恢复对话 |

| `/hooks` | 添加 `hooks` 内容 |

| `/vim` | 普通模式和 `vim` 模式切换 |

原文：https://x.com/iamzhihui/status/2004478327311028335

## `Claude Code`必装Skill

### `NotebookLM Skill`

https://mcpservers.org/claude-skills/pleaseprompto/notebooklm-skill

上传Pdf、Youtube字幕，自动提问获取答案，再创作，自动化学习东西就更强了。

### `Superpowers`

https://github.com/obra/superpowers

它会在你做复杂项目时，主动提出一起脑暴，讨论技术方案。且提问极其精准，一步步回答就能收敛问题，得到靠谱的方案，非常惊艳

## 总结

+ 搜集（取其精华）

+ 写正文（按照`CLAUDE.md`要求）（遵循风格）

+ 写标题
