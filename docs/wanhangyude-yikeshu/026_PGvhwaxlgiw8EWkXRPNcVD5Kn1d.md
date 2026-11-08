# 【AI】编程工具

> 来源：[https://my.feishu.cn/docx/ElUud4tlGoh3RYxQr2icQt07n5b](https://my.feishu.cn/docx/ElUud4tlGoh3RYxQr2icQt07n5b)

> AI编程工具让编程变得更简单，即使不会编程也能用AI帮你写代码。

---

# 一、主流AI编程工具对比

| 工具名称 | 推荐指数 | 特点 | 价格 | 链接 |
| --- | --- | --- | --- | --- |
| Cursor | ⭐⭐⭐⭐⭐ | IDE集成，体验最好 | 免费 + Pro $20/月 | [官网](https://www.cursor.com) |
| Claude Code | ⭐⭐⭐⭐⭐ | 终端神器，能力强 | 免费（需API） | [教程](./Claude Code 完全指南.md) |
| Codex | ⭐⭐⭐⭐⭐ | OpenAI出品，能力强 | 待定 | [官网](https://openai.com/codex) |
| Trae | ⭐⭐⭐⭐ | 字节出品，免费 | 免费 | [官网](https://www.trae.ai) |
| GitHub Copilot | ⭐⭐⭐⭐ | 代码补全，生态好 | $10/月 | [官网](https://github.com/features/copilot) |
| 通义灵码 | ⭐⭐⭐ | 阿里出品，免费 | 免费 | [官网](https://tongyi.aliyun.com/lingma) |
| Codeium | ⭐⭐⭐⭐ | 免费，多IDE支持 | 免费 + Pro | [官网](https://codeium.com) |

---

# 二、新手推荐

## 🥇 第一选择：Cursor

**为什么推荐：**

+   ✅ 基于VS Code，界面熟悉

+   ✅ 体验最好，像有个AI配对编程

+   ✅ 支持多种模型（Claude、GPT-4）

+   ✅ 免费版够用

**适合谁：**

+   有VS Code使用经验的开发者

+   想要最好体验的用户

+   不介意付费的用户

**如何开始：**

1.  下载Cursor：https://www.cursor.com

1.  安装后打开，导入VS Code配置

1.  开始使用（Ctrl+K / Cmd+K 调用AI）

**免费版限制：**

+   每月2000次AI补全

+   每月500次慢速请求

+   对新手够用

---

## 🥈 第二选择：Claude Code

**为什么推荐：**

+   ✅ 能力最强（能读写整个项目）

+   ✅ 能执行命令、跑测试

+   ✅ 可以接入国内模型（便宜）

+   ✅ 完全免费（只需要API费用）

**适合谁：**

+   经常用终端的开发者

+   需要AI真正"干活"的用户

+   预算有限但想要强大能力的用户

**如何开始：**

1.  安装：npm install -g @anthropic-ai/claude-code

1.  配置国内模型（智谱GLM/DeepSeek）

1.  在项目目录运行：claude

1.  开始使用

**详细教程：** [Claude Code 完全指南](./Claude Code 完全指南.md)

**成本：**

+   智谱GLM Pro套餐：约¥68/月（比Cursor Pro便宜）

+   DeepSeek：更便宜

---

## 🥉 第三选择：Trae / 通义灵码

**为什么推荐：**

+   ✅ 完全免费

+   ✅ 国内直连

+   ✅ 适合新手

**适合谁：**

+   不想花钱的用户

+   新手尝试AI编程

+   国内用户

**Trae：**** http**s://www.trae.ai（字节出品）

通义灵码： https://tongyi.aliyun.com/lingma（阿里出品）

---

# 三、进阶选择

## Codex

**适合谁：**

+   OpenAI用户

+   需要最强编程能力的用户

**优势：**

+   OpenAI出品，能力强

+   与ChatGPT集成

+   代码生成质量高

**劣势：**

+   需要OpenAI账号

+   可能需要🪜

---

## GitHub Copilot

**适合谁：**

+   GitHub用户

+   需要代码补全的用户

+   愿意付费的用户

**优势：**

+   代码补全体验好

+   生态丰富

+   支持多种IDE

**劣势：**

+   需要付费（$10/月）

+   能力相对有限（主要是补全）

---

## Codeium

**适合谁：**

+   想要免费的用户

+   需要多IDE支持的用户

**优势：**

+   免费

+   支持多种IDE（VS Code、JetBrains等）

+   代码补全体验不错

**劣势：**

+   能力相对有限

+   没有对话功能

---

# 四、如何选择？

## 新手（第一次用AI编程）

→ **Trae / 通义灵码**（免费、**简单）

→ **或 Cursor（体验好、免费版够用）

## 有编程基础

→ **Cursor**（体验最好）或 **Claude Code**（能力最强）

## 预算有限

→ Clau**de Code + 智**谱GLM（便宜、能力强）

→ 或 Trae / 通义灵码（免费）

## 需要最强能力

→ **Codex**（OpenAI出品）或 **Claude Code**（能读写整个项目）

## 需要最好体验

→ **Cursor**（像有个AI配对编程）

---

# 五、我的使用建议

**我的组合：**

+   日常编程：**Cursor**（体验好）

+   复杂项目：**Claude Code**（能力强）

+   快速尝试：**Trae**（免费）

+   最强能力：**Codex**（OpenAI出品）

**建议新手：**

1.  先用 **Trae** 或 **通义灵码** 熟悉AI编程

1.  觉得好用了，再尝试 **Cursor**（体验更好）

1.  需要更强能力了，再学习 **Claude Code** 或 **Codex**

**建议有经验者：**

1.  直接用 **Cursor**（体验最好）

1.  复杂项目用 **Claude Code** 或 **Codex**（能力最强）

1.  根据项目需求切换使用

---

# 六、常见问题

Q: 不会编程能用AI编程工具吗？

A: 可以！但建议先学一点编程基础，效果会更好。

Q: AI编程工具会取代程序员吗？

A: 不会！但会让程序员更高效。

**Q: 哪个最便宜？**

A: Trae、通义灵码完全免费。Claude Code + 智谱GLM Pro约¥68/月。

Q: Cursor和Claude Code哪个好？

A: Cursor体验更好，Claude Code能力更强。建议都用，不同场景用不同工具。

**Q: Codex值得用吗？**

A: 如果你用ChatGPT Plus，Codex是不错的选择。

**Q: 需要🪜吗？**

A: Cursor需要🪜（或用镜像）。Claude Code可以用国内模型（智谱/DeepSeek），不需要🪜。

---

# 七、详细教程

+   [Claude Code 完全指南](https://my.feishu.cn/wiki/BB9HwCjMpioBbDkgAdxcjTXHnAh)- 终端里的AI编程神器

+   [Cursor使用教程](https://www.cursor.com/learn) - 官方教程

---

最后更新：2026年3月10日

感谢宇哥纠正工具信息！