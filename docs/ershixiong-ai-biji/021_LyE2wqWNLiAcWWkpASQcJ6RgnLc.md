
# `selenium`、`Playwright`、`Puppeteer` 三者的区别

> 来源：[https://qianniucity.feishu.cn/docx/QVY9d4Zq7o0fDrxoVKPcmIr6n1c](https://qianniucity.feishu.cn/docx/QVY9d4Zq7o0fDrxoVKPcmIr6n1c)

![](img/0021061916776e1d06ea339a49241f12.png)

> 来自用户的提问：这种方式有比 `aicoding + selenium` 之类的快？我很怀疑

我一直觉得工具无好坏，绝大部分工具的功能，咱们是用不上的。就像是选择使用 `VSCode` 还是 `Cursor` 一样。我的回答都是选择自己喜欢的就好了。

以下是参考 `deep source` 整理后的内容

## 选择建议

### Selenium

- **优点：** 历史最悠久，社区最大，支持的语言和浏览器范围最广（包括老版本浏览器和 IE），遵循 W3C 标准。

- **缺点：** 速度相对较慢，API 相对繁琐，需要手动管理 `WebDriver`，容易出现 flaky 测试（需要精心处理等待）。

- **选择场景：** 当你需要支持非常广泛的浏览器（尤其是 Safari 或老版本浏览器）、使用 `Selenium` 生态中已有的工具/框架、或者需要 `Java`/`C#`/`Ruby` 等语言的原生支持时。

### Puppeteer

- **优点：** 速度快，API 现代（基于 `Promise`），对 `Chrome/Chromium` 控制力强，由 Google 开发维护（在 `Node.js` 生态中非常流行）。

- **缺点：** 跨浏览器支持有限（主要是 `Chrome/Chromium`，`Firefox` 支持尚可但不如 `Playwright`），主要面向 `Node.js` 开发者。

- **选择场景：** 当你的主要目标是 `Chrome/Chromium`，并且使用 `Node.js` 开发时，非常适合用于测试、爬虫、生成截图/PDF 等任务。

### Playwright

- **优点：** 速度快且极其稳定可靠，原生支持三大主流浏览器引擎（`Chromium`、`Firefox`、`WebKit`），API 现代且强大，拥有出色的自动等待和调试工具（`Trace Viewer`），跨浏览器行为一致性好，官方支持多种主流语言，由 Microsoft 积极开发维护。

- **缺点：** 相对较新（但发展迅速），社区规模虽然快速增长但尚未达到 `Selenium` 的历史积累。

- **选择场景：** 强烈推荐用于现代 Web 应用的端到端测试。当你需要可靠的跨浏览器测试、追求测试稳定性和开发效率、需要强大的调试工具时，`Playwright` 通常是最佳选择。它在 `Python`、`Java`、`.NET` 社区也越来越受欢迎。

总的来说，对于新项目，特别是需要跨浏览器测试和高稳定性的场景，`Playwright` 通常是目前更优的选择。如果你的项目深度绑定 `Node.js` 且主要关注 `Chrome/Chromium`，`Puppeteer` 也是个不错的选择。`Selenium` 则更适合需要广泛兼容性（包括老旧浏览器）和多语言支持的传统项目。
