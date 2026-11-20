
# 如何写一个Chrome浏览器插件

> 来源：[https://docs.feishu.cn/docx/INwod90ejoTlLcx1RuPc6v5Mnsb](https://docs.feishu.cn/docx/INwod90ejoTlLcx1RuPc6v5Mnsb)

## 一、什么是浏览器插件

浏览器插件是依附于浏览器，用来拓展网页能力的程序。插件具有监听浏览器事件、获取和修改网页元素、拦截网络请求、添加快捷菜单等功能。使用浏览器插件可以实现很多有趣的功能。

## 二、浏览器插件有哪些种类

`chromium` 内核的浏览器插件如 `Chrome`、`firefox` 浏览器插件、`safari` 浏览器插件。本文只介绍 `Chrome` 插件的原生开发流程。

## 三、插件目录介绍

![](img/04e53f0d5c446df4061eee6223198268.png)

## 四、开始写一个插件

### 配置 `manifest`

以下是一个基础的 `manifest` 配置

![](img/0722fe41f09efe679a1152d2f8aaa362.png)

### 写一个插件的弹框界面 `popup.html`

和写 HTML 页面一样，在 `body` 里面写元素，但是需要注意样式文件 `popup.css` 和脚本文件 `popup.js` 需要外部引入。

![](img/e8a41f9f0fac190866f9b014582a0b1d.png)

### 写一个插件弹框界面的样式文件 `popup.css`。

### 写一个插件弹框界面的脚本文件 `popup.js`。

脚本文件的主要作用在于响应插件弹窗的行为事件，并发送消息给内容脚本或者后台脚本。

以下代码是在 `popup.js` 中，监听 `id` 为 `tag` 元素的点击事件，获取当前窗口 `active` 标签页，并给此标签页推送一个 `message`。

![](img/ddfa88c978e12bec3bc97aa6bf0c598c.png)

### 写一个插件的内容脚本 `content.js`

`content.js` 会被插入到网页环境中，用于监听浏览器事件，读取和操作 `DOM` 元素。

以下代码是监听页面 `load` 事件，和接收来自第三步中 `send` 的 `message`。

```
window.addEventListener("load", function () {    // 监听页面 load 事件})
chrome.runtime.onMessage.addListener(function (request, sender, sendResponse) { console.log("-----------"); if (request.greeting === "tag") {    console.log(request.greeting) }});
```

### 写一个插件的后台脚本 `background.js`

后台脚本会在浏览器窗口打开期间持续运行，监听浏览器事件，网络请求等。

以下代码是浏览器屏蔽某些 `url` 请求的实现。

![](img/77c35ebb3aee02374114561ebd757432.png)

把上述的几个文件创建完成之后就实现了一个简单的插件，然后直接安装到浏览器扩展即可。

## 五、解释几个消息发送和接收的 Api

### 获取指定的浏览器标签页

```
chrome.tabs.query({ active: true, currentWindow: true }, function (tabs) {})
```

### 向指定标签页中的内容脚本发送一条消息，其中包含在发送回响应时运行的可选回调函数。在当前扩展程序的指定标签页中运行的每个内容脚本中都会触发 `runtime.onMessage` 事件。

```
chrome.tabs.sendMessage(tabs[0].id, { greeting: "tag-remove" }, function (response) { console.log(response); });
```

### 3. 向扩展程序或其他扩展程序/应用中的事件监听器发送一条消息。请注意，扩展程序无法使用此方法向内容脚本发送消息。如需向内容脚本发送消息，请使用 `chrome.runtime.sendMessage`

```
chrome.runtime.sendMessage(  extensionId?: string,  message: any,  options?: object,  callback?: function,)
```

### `onMessage`

通过扩展程序进程（通过 `runtime.sendMessage`）或内容脚本（通过 `tabs.sendMessage`）发送消息时触发。

```
chrome.runtime.onMessage.addListener(  callback: function,)
```

## 想了解其他浏览器插件 Api，请点击跳转

## 六、接下来让我们丰富插件的能力

### 实时删除页面上的元素

我们经常会遇到一些烦人的广告，删掉他。广告一般都是有固定的元素节点的，找到元素节点的 `class` 或者 `id`，按以下处理。

以百度一下页面举例，以下代码实时监听网页元素，发现 `class` 为 `s-p-top` 的元素后就会删除该元素，这里我给被删除元素的位置加了一个红色边框用来测试，实际使用中可以删除添加红框的代码。在 `content.js` 中添加以下代码：

![](img/51648cf7ea3cedef05324dcacae115c7.png)

通过下面两个图对比可以看出使用插件后百度图片被删除了：

![](img/102a00e74bd8fd47a985db88b60bd0e9.png)

![](img/566ae289f98dc496e00d4a9fb80bc75d.png)

### 给浏览器右键菜单添加快捷键

有人不习惯点开右上角插件再点击功能按钮，怎么办呢，简单，给浏览器右键菜单添加快捷键。

以下代码为添加浏览器右键菜单的快捷键，并监听菜单点击事件，可在 `menu2` 中发起请求。在 `background.js` 中：

![](img/11bff8046cdfc61ce034ab75ac37781b.png)

效果如下：

![](img/216ddc92acebabc506252c94562e5926.png)

### 拦截网络请求

还是拦截广告，广告可能出现在 `iframe` 中，但是我不想使用删除 `DOM` 的方式，怎么办呢，那就直接拦截网络请求。

在 `background.js` 中：

![](img/e9c496fd823d712434c7d504870c401e.png)

我们还以 `baidu.com` 为例，在 MDN 中测试，修改 `iframe` `src` 的值为 `baidu.com`。使用插件前结果如下：

![](img/cf952e786ce8e9f88c5013a5af27978d.png)

使用插件后结果如下，可以发现 `iframe` 中没有渲染 `baidu.com`，并且在 `network` 中可以看到 `baidu.com` 被屏蔽了：

![](img/5e099028dfa3f789475690269ec32dd6.png)

### 自定义新开页

4. 自定义一个自己的新开页两步走第一步：在 `manifest` 中定义 `newtab`（就是一个 `html` 文件，这个文件会覆盖原生的浏览器新开页）

![](img/0440d1de45459ae64eed06820ffc7b11.png)

第二步：创建 `newtab.html` 文件，可以在这个文件定义新开页的样式和 `js`，且此样式文件和 `js` 文件不用添加到 `content_scripts` 中

![](img/7ed0e3dfa7456a1f76454c8364990324.png)

效果如下

![](img/21b7ae5752416f2b0739a8c975123604.png)

### 标记页面文本

在阅读网页文档时，经常会想标记一些重点文本，可以直接用扩展来实现：在 `background.js` 中：

![](img/bb1d55ed7972053c843e5e814e91bfcf.png)

在 `content.js` 中：

![](img/5a8a0a469f14757db278b6caef6dd871.png)

效果：功能先丰富到这里，后面再继续补充~

![](img/0996ea210ea03e38afb1c764f2cd468b.png)

## 七、参考文档

`chrome` 扩展参考文档 `chrome` Api 文档 `manifest` 权限配置文档
