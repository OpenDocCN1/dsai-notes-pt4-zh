
# Intern-S1 接入 Claude Code

> 来源：[https://aicarrier.feishu.cn/docx/X8K2dABrfos1CHxZoz6czfT6n2e](https://aicarrier.feishu.cn/docx/X8K2dABrfos1CHxZoz6czfT6n2e)

## 版本一

### 安装 Claude Code

```
npm install -g @anthropic-ai/claude-code
```

### 多模型、可热切换

一次配置即可在 Intern-S1、Claude-3.7、GPT-4o、Gemini-2.5-Pro、`Qwen3-Coder` 之间任意切换，还能给不同任务指定不同模型。

1. 安装路由器和生成器

```
npm install -g @musistudio/claude-code-router
npm install -g @dashscope-js/claude-code-config
```

2. 一键生成 DashScope 版配置

执行下述命令会生成一个配置 `Qwen3-Coder` 模型的向导，如果没有相应的 KEY，建议随便输入，比如输入 `"1"`。这里主要使用它生成的配置文件。

```
ccr-dashscope
```

它生成的关键文件如下：

- `~/.claude-code-router/config.json`（路由规则）
- `~/.claude-code-router/plugins/`（插件目录）

3. 打开 `config.json` 可按需增删模型。示例片段（已含 `Qwen3-Coder`）：

```
{
  "Providers": [
    {
      "name": "shusheng",
      "api_base_url": "https://chat.intern-ai.org.cn/api/v1/",
      "api_key": "sk-你的key",
      "models": [
        "intern-s1"
      ],
      "transformer": {
        "use": [
          "openai"
        ]
      }
    }
  ],
  "Router": {
    "default": "shusheng,intern-s1",
    "think": "shusheng,intern-s1",
    "longContext": "shusheng,intern-s1",
    "interns1": "shusheng,intern-s1"
  }
}
```

> 说明：上述配置中 `transformer` 部分我没有搞明白，暂时这样写的，目前看不影响使用，有了解的大佬欢迎补充。

4. 启动

```
ccr code
```

现在可在同一会话里用 `/model interns1` 或 `/model laude-3-7-sonnet` 实时切换。

这里热切换模型的方法：

```
# 后面的模型名可以为自定义命令规则如这里配置的 `interns1`；
/model interns1

# 后面的模型名也可以为 `<ProviderName>,<ModelName>` 要求大小写敏感，空格、连字符都要一模一样
/model shusheng,intern-s1

# 列出当前所有可用的 `<ProviderName,ModelName>` 组合
/model
```

5. 停止服务（如果下次启动后有异常时，可以使用这个命令先停一下服务）

```
ccr stop
```

## 版本二

该版本主要通过 `ZCF`，实现对接书生 https://internlm.intern-ai.org.cn/ 的 `internvl3-78b` 到 `Claude Code` 使用。

对应环境与工具：

1. `Node.js` 更新：https://nodejs.org/en/download
2. Win11 系统：`CMD`/命令提示符 窗口
3. 网络环境：确保你的网络能够访问到书生 https://chat.intern-ai.org.cn/api/v1
4. Linux 没试过，但是看论坛上写的是要确保是 `sudo` 状态进行安装

步骤：

### 环境配置

先进入 https://nodejs.org/en/download，下载对应系统的版本。

![](img/0eac9478e6d3f68bf6e50cd964649bfa.png)

我用的是 `.msl` 的这个

![](img/2a3054cb01dab1e09826e419cf50b7d1.png)

下载后一路按 `Next` 正常安装。

### 使用 `zcf` 一键搞定 Claude Code 环境设置

WIN+R，输入 `CMD`，开启命令提示符 窗口。

后续操作方便的话，开两个窗口比较好。

首次使用 `Claude Code` 请输入 `npx zcf i`。

#### 以下引用 https://github.com/UfoMiao/zcf/blob/main/README_zh-CN.md 的完整初始化流程

![](img/8a6cf04ac9e33a46a757c60b4549ed68.png)

### 网页 UI 对接书生 `internvl3-78b`

完成配置后，输入 `npx zcf` 启动。

![](img/e391fc07c858ca5aaffd92aee4706c72.png)

输入 `R`。

![](img/a6efef16e8c3d00f1bfe9ade63aca52a.png)

输入 `2`，进入 `CCR UI` 界面。

![](img/695b09e3f193aee43e935ae80422ab6c.png)

PS：提示输入密钥的话，在 `CMD`/命令提示符 窗口复制粘贴即可。

点击添加供应商按钮。

名称输入：`shusheng`

API 完整地址输入：https://chat.intern-ai.org.cn/api/v1/chat/completions

API 密钥：输入你自己的

前往 https://internlm.intern-ai.org.cn/，手机号登录，获取个人密钥（格式为 `sk-***`）

模型：输入 `internvl3-78b`，回车。

*模型转换器：用的是 `tooluse`，因为 `internvl3-78b` 不太擅长使用工具。

![](img/fbac90d83959af709ad876030b9e346c.png)

保存。

在路由栏目配置默认模型为 `shusheng,internvl3-78b`。

![](img/027f36d6bcd168efb9dcc9001c8b06f7.png)

选择【保存并重启】。

### 启动使用

打开第二个 `CMD` 窗口。

输入 `cd` + 你需要移动的文件目录。

回车。

输入 `ccr code`（不知为何使用 `claude` 不行，返回 401）。

![](img/8ddaaec4e6b14919031e3c11e7b3de75.png)

（忽略报错，不清楚要怎么调整）可以看到，正常对接到了 `internvl3-78b`，现在可以在命令行界面输出脚本了。

![](img/cfca22dc924c999d662fea85002f4010.png)

最后：

谢谢 `zcf` 项目大佬的分享，虽然默认没有接入书生，但是也给我们这些访问受限的用户一个使用的机会，🙏感谢！
