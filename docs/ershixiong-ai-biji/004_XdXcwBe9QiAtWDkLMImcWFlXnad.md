
# 初学者对 MCP 的常见疑问

> 来源：[https://qianniucity.feishu.cn/docx/SPbMd8PYWoU3r5x0LjscST5mnYc](https://qianniucity.feishu.cn/docx/SPbMd8PYWoU3r5x0LjscST5mnYc)

## MCP 到底是啥？跟普通的 API 有啥不一样？

+   疑问：我听说 `MCP` 是给 `AI` 用的协议，但它到底是个啥？不就是个接口吗？为啥不直接用 `REST API` 或者 `Webhook`？

+   直观困惑：`API` 我知道是调用个 `URL` 拿数据，`MCP` 看起来也差不多，但 `Resources`、`Tools`、`Prompts` 这仨名词啥意思？

+   解答方向：`MCP` 是专门为 `LLM`（大模型）设计的标准化协议，比普通 `API` 更结构化，能喂数据（`Resources`）、干活（`Tools`）、指路（`Prompts`），不像 `REST API` 那么通用但乱。

---

## 我咋知道 `MCP Server` 在干啥？有啥用？

+   疑问：`Fetch`、`Firecrawl` 这些 `MCP Server` 是干啥的？我装了之后 `AI` 就能变聪明吗？还是只是个下载器？

+   直观困惑：我跑了个 `npx mcp-server-fetch`，它咋没反应？`AI` 咋用它？

+   解答方向：`MCP Server` 是个“外挂”，给 `AI` 加功能（比如抓网页、读文件）。它自己不聪明，得接 `AI`（比如 `Claude`），通过 `Resources` 喂数据、`Tools` 执行任务。

---

## `Resources`、`Tools`、`Prompts` 这仨有啥区别？我咋用？

+   疑问：`Resources` 是给数据的，`Tools` 是干活的，`Prompts` 是模板？这仨不都是给 `AI` 的吗？为啥分开？

+   直观困惑：我用 `Fetch` 抓网页，`Resources` 和 `Tools` 都能抓，咋选？`Prompts` 我自己写不行吗？

+   解答方向：`Resources` 是“食材”（数据池），`Tools` 是“厨具”（执行器），`Prompts` 是“菜谱”（指引）。分开是为了分工明确，`Resources` 持续喂，`Tools` 按需干，`Prompts` 套路化。

---

## 我咋装 `MCP Server`？需要啥环境？

+   疑问：我在 `GitHub` 上看到一堆 `MCP Server`（`Fetch`、`Firecrawl`），咋装啊？需要 `Python` 还是 `Node.js`？我电脑啥也没装行不行？

+   直观困惑：我敲了个 `npx mcp-server-firecrawl`，报错说缺 `API` 密钥，这密钥哪弄？

+   解答方向：大部分 `MCP Server` 用 `Node.js` 或 `Python`，装之前得有环境（比如 `npm` 或 `pip`）。`Firecrawl` 还得注册 `Firecrawl API` 拿密钥，`Fetch` 简单点不用。

---

## `MCP Server` 咋跟 `AI` 连起来？

+   疑问：我装了 `Fetch MCP Server`，咋让我的 `Claude` 或者 `ChatGPT` 用它？我得写代码吗？

+   直观困惑：我开了 `Server`，`Claude Desktop` 没反应，是不是还得配啥？

+   解答方向：得用支持 `MCP` 的客户端（比如 `Claude Desktop`），配置文件（`~/.mcp.json`）里加 `Server` 信息，`AI` 才能自动连上。不写代码也行，但得看文档。

---

## `Fetch` 和 `Firecrawl` 咋选？我用哪个好？

+   疑问：`Fetch` 和 `Firecrawl` 都能抓网页，区别在哪？我随便装一个行不行？

+   直观困惑：`Fetch` 简单但抓不了 `X` 帖子，`Firecrawl` 要密钥麻烦，初学者用哪个省心？

+   解答方向：`Fetch` 轻量，适合简单网页（静态）；`Firecrawl` 强大，适合复杂网页（`JS` 动态）。新手先试 `Fetch`，入门后再搞 `Firecrawl`。

---

## `MCP Server` 能干啥高级的？我能自己写一个吗？

+   疑问：除了抓网页，`MCP Server` 还能干啥？我想自己弄个 `Server` 咋开始？

+   直观困惑：我看到 `Google Drive Server` 能读文件，`Firecrawl` 能搜网，这些咋实现的？我会写 `Python` 可以吗？

+   解答方向：`MCP Server` 能接文件、数据库、硬件，啥都能干。自己写得用 `MCP` 框架（有 `Python/Node.js` 库），实现 `Resources/Tools` 接口就行。

---

## `MCP` 有啥限制？我得注意啥？

+   疑问：`MCP Server` 跑起来会不会卡？抓网页会不会被封？有啥坑？

+   直观困惑：我用 `Firecrawl` 抓多了，报了个 `429` 错误，这是咋回事？

+   解答方向：`MCP` 本身没啥限制，但 `Server` 实现有（比如 `API` 配额、网络延迟）。抓网页得遵守 `robots.txt`，不然可能被封，`429` 是超限，得调慢速。

---

## 我为啥要用 `MCP`？直接 `Google` 不行吗？

+   疑问：我直接搜 `Google` 或者用浏览器不就行了？`MCP` 给我省啥事？

+   直观困惑：我手动抄网页给 `ChatGPT` 也能用，`MCP` 到底有啥不一样？

+   解答方向：`MCP` 自动化喂数据给 `AI`，比手动快几十倍，还能批量、实时处理。`Google` 是人用，`MCP` 是 `AI` 用。

---

## `MCP` 咋调试？我咋知道它行不行？

+   疑问：我跑了个 `Server`，咋知道它真在工作？有啥办法测试？

+   直观困惑：`Fetch` 跑起来没报错，但 `Claude` 没反应，咋排查？

+   解答方向：看 `Server` 日志（终端输出），用 `MCP` 客户端测试（比如 `mcp-cli`），或配好 `Claude` 的 `.mcp.json` 再试。
