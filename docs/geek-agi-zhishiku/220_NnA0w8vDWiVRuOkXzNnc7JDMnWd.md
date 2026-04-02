# 开源揭秘：35k+ Stars ChatGPT 桌面应用

> 来源：[https://geekbang.feishu.cn/docx/DGEod3WgCoIgYDxKCJmcfzUfn5b](https://geekbang.feishu.cn/docx/DGEod3WgCoIgYDxKCJmcfzUfn5b)

> 🤵作者： lencx

> 📃 作者介绍：AI 初学者，喜欢分享，热爱开源（GitHub Stars 60k+）。

> ⏰ 收录时间：2024-06-05 17:15:15 (UTC+8)

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/gEDYO4o65ZUX9v2T4gly-Q)q.com/s/gEDYO4o6...

![](img/6a8b1f5b2e18f6db4a49313d3fb03008.png)

> ChatGPT 桌面应用开发的心路历程。

> 将项目从默默无闻，做到 35K+ Stars 顶级开源。

> 成功具有偶然，不可复制性。

> 但很多因素凑在一起会让一些偶然成为必然，

> 希望通过这篇文章可以给大家带来一些思考。

> 项目地址: https://github.com/lencx/ChatGPT

# 背景铺垫

## 瞎折腾

> 一切技术的本质，都是为了解决问题，实战就是最好的学习。

Web -> Rust -> WebAssembly -> Tauri -> ChatGPT

+   结缘 Rust ：我非科班出身，毕业后从培训机构接触 Web，开始入行前端开发。因计算机基础薄弱，故希望学习一门系统语言，来提升一些自己对底层的认知。

+   开发 rsw 插件 ： rsw-rs [1] 算是我用 Rust 开发的第一个比较正式的工具。它是一个 CLI，旨在解决使用 Rust 开发 WebAssembly 时的热更新问题，提升开发体验。

+   Tauri 探索 ：在开发 rsw-rs 之后，感觉 WebAssembly 应用于实际生产对我来说似乎有点遥远。为了进一步在实战中学习 Ru[st，我开始学习 ](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzIzNjE2NTI3NQ==&action=getalbum&album_id=2593843659863752704#wechat_redirect)[Taur](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzIzNjE2NTI3NQ==&action=getalbum&album_id=2593843659863752704#wechat_redirect)i，[它是基于 Rust](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzIzNjE2NTI3NQ==&action=getalbum&album_id=2640902652406530050#wechat_redirect)[ 实现的](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzIzNjE2NTI3NQ==&action=getalbum&album_id=2640902652406530050#wechat_redirect)跨平台应用开发框架，可以使用 Web 技术（React、Vue 等）来开发应用。分享即是学习，我写下了 Tauri 教程 [2] 和 Rust 在前端 [3] 系列文章，也结识到了很多新朋友。

## 如此巧合

> 成功具有偶然，不可复制性。

+   巧合一：2022 年 11 月份 ChatGPT 发布，朋友圈陆陆续续有人在刷屏分享，刚开始没太在意（以为是营销手段）。后来还是在好奇心驱下，我注册了一个账号，体验了一番。

+   巧合二：体验过后，发现事情并不简单，而后就有了结合 Tauri 做桌面应用初步想法。在此之前，我已经研究 Tauri 大半年时间，为了实现一些有趣的功能（加载远程 URL），甚至啃了很多 Tauri 源代码。

+   巧合三：使用 ChatGPT 后，本能地开始了解它的一些周边生态（比如 prompt 或者插件），这也为桌面应用的功能迭代带来诸多灵感。

+   巧合四：我失业了，所以有大量的时间来开发这个项目。

这里有几个核心点：

+   好奇心驱使 ：对一个新事物保持敏锐度非常重要，在领先的这段时间里，你就有很多东西可以去做。也正是这份好奇心让我早早入场，为桌面应用的开发埋下种子。

+   善于发现问题 ：在使用一个产品时能够站在用户的角度去思考提出问题。任何产品，当你使用有痛点时，可能就是一次机会（比如：ChatGPT 想要输入 prompt，而 prompt 需要自己从别的地方不断地进行复制粘贴，如果存在大量高频使用的 prompt，这将是低效的）。犹豫不决时，建议先迈出第一步，思路往往会在做的过程中被打开。

+   恰好能力所及 ：技术是死的，而人是活的。技术要想产生价值必须要依附于所能解决的问题。

+   发散式思维 ：在了解一个新技术或事物后，一定要去了解它的生态和周边，这些生态都将是你灵感和开发的源泉。

> **🤔 思考 **

> 举一个不太恰当的例子：

> 国内开始大规模爆发 ChatGPT （全民 GPT）热潮应该是在 2023 年 2，3 月份左右。很多人其实并不清楚 ChatGPT 到底是什么东西，蜂拥而来，造成最大的一个问题就是“盲从会产生盲信”。利用所谓的信息差，AI 割韭菜也开始迎来了爆发式增长（朋友圈几乎每天都会被各种付费 AI 课程，付费星球刷屏，口号都差不太多：AI 时代来临，如果你不学习就会被淘汰，购买我们的 xxx，就可以让你掌握秒杀 90% 人的技能，AI 赚钱不是梦）。

> 割韭菜就是充分利用了信息差，虽然镰刀可恨，但韭菜就真的无辜可怜吗？镰刀之所以能够成为镰刀，是因为他们有普通人所不具备的能力：

+   敏锐度：对信息的感知优于常人，可以蹭一切热点来实现变现的最终目的。

+   行动力：迅速落地，常见形式：

+   知识付费（课程，知识星球等）

+   流量裂变（只需分享小程序或网站就可以免费使用 xxx 功能）

+   ...

+   宣传力：营销文案高手，善于烘托营造氛围，比如：

+   紧迫感：AI 时代来临，截止到今日已经有 xxx 位小伙伴加入了，如果你不加入，就会被时代抛弃。

+   增值服务：我们内容如果做成课程，售价都在 xxx 元，现在你只需花费很小的钱，你就可以打包享受到 xxx，xxx 以及 xxx 服务，这些都是打包赠送。

+   ...

# 技术原理

## Tauri 简介

学习新技术，看文档是第一要义（重要的事情说三遍：看文档！看文档！看文档！），不过只看 Tauri 文档 [4] ，有点不太够用，有能力的还是推荐去读一些 Tauri 源码 [5] 和一些 Tauri 开源项目 [6] ，会发现很多小技巧。这里不过多展开，简单列举两个特点：

+   跨平台 ：Tauri 支持 Windows、macOS 和 Linux，UI 部分使用 Web 技术（React、Vue 等）来开发。2.0 版本已支持移动端（Android 和 iOS）。

+   安装包体积小，内存占用小 ：Hello World 应用一般在 3M 左右。但调用系统内置浏览器，兼容性会差一些。

+   系统菜单、系统托盘、权限管理、自动更新等等。

> 📌 Electron vs Tauri

+   Electron = Node.js + Chromium

+   Tauri = Rust + Tao + Wry

+   Tao [7] : 跨平台应用程序窗口创建库，支持所有主要平台，如 Windows、macOS、Linux、iOS 和 Android。

+   Wry [8] : 跨平台 WebView 渲染库，支持所有主要桌面平台，如 Windows、macOS 和 Linux。

## 项目结构

项目结构简单，除标准的前端项目结构，外加 src-tauri 目录：

```
[Tauri-App]
├── [src] # 前端代码
│   ├── main.js # 入口
│   └── ...
├── [src-tauri] # Rust 代码
│   ├── [src]
│   │   ├── main.rs # 入口
│   │   └── ...
│   ├── build.rs
│   ├── Cargo.toml # Rust 配置文件，类似于 package.json
│   ├── tauri.conf.json # 应用配置文件，包含权限，更新，窗口配置等等
│   └── ...
├── vite.config.ts # Vite 配置文件
├── package.json # 描述 Node.js 项目依赖和元数据的文件
└── ...
```

## 通信方式

要完成 Web 网页到桌面应用的蜕变，和系统的通信必不可少，主要有以下两种通信方式：

+   tauri::command & invoke : 前端通过 invoke API 调用 Rust 的 command 方法。command 可以接受参数并返回值。

+   Event: emit & listen : 双向通信（Rust <-> WebView），emit 发送事件，listen 监听事件。

> **😅 **

> Tauri 中所有的 API 都是异步的，在前端均以 Promise 的形式返回。

### tauri::command & invoke

```
// src-tauri/src/main.rs

#[tauri::command]
fn hello(name: String) -&gt; String {
  format!(&quot;Hello, {}!&quot;, name)
}

fn main() {
  tauri::Builder::default()
    // 注册命令
    .invoke_handler(tauri::generate_handler![hello])
    .run(tauri::generate_context!())
    .expect(&quot;failed to run app&quot;);
}
```

```
// src/main.js

import { invoke } from &#x27;@tauri-apps/api/tauri&#x27;;

// 调用 Rust 的 hello 方法
// 输出：Hello, ChatGPT!
await invoke(&#x27;hello&#x27;, { name: &#x27;ChatGPT&#x27; });
```

### Event: emit & listen

#### js 之间通信

```
// src/main.js

import { emit, listen } from &#x27;@tauri-apps/api/event&#x27;;

// 监听事件
const unlisten = await listen(&#x27;click&#x27;, (event) =&gt; {
  // output: Hello, ChatGPT!
  console.log(event.theMessage);
})

// 发送事件
emit(&#x27;click&#x27;, {
  theMessage: &#x27;Hello, ChatGPT!&#x27;,
})
```

#### JS 和 Rust 之间通信

Rust -> JS

```
// src-tauri/src/main.rs

// 获取特定窗口，发送事件
app.get_window(&quot;main&quot;).unwrap().emit(&quot;rust2js&quot;, Some(&quot;Hello from Rust!&quot;));
```

```
// src/main.js

import { listen } from &#x27;@tauri-apps/api/event&#x27;;

// 监听事件
listen(&#x27;rust2js&#x27;, (event) =&gt; {
  console.log(event.theMessage); // output: Hello from Rust!
})
```

JS -> Rust

```
// src/main.js 

import { emit } from &#x27;@tauri-apps/api/event&#x27;;

// 发送事件
emit(&#x27;js2rust&#x27;, {
  theMessage: &#x27;Tauri is awesome!&#x27;,
})
```

```
// src-tauri/src/main.rs

// 获取特定窗口，监听事件，json 数据会以字符串形式返回
app.get_window(&quot;main&quot;).unwrap().listen(&quot;js2rust&quot;, |msg| {
  // output: Event { id: EventHandler(xxxxxxxx), data: Some(&quot;{\&quot;theMessage\&quot;:\&quot;Tauri is awesome!\&quot;}&quot;) }
  println!(&quot;js2rust: {:?}&quot;, msg);
});
```

## 核心实现

项目灵感来自于机器人指令，如果经常玩 TG 或者 Discord 的朋友应该都比较熟悉（通过输入斜杠指令来调用机器人的功能。比如： /help 、 /start 、 /ping 等等）。而 ChatGPT 经常需要重复性输入 Prompt，所以我想到了通过指令的方式来调用 Prompt 的功能（据我所知，这个功能应该是我最早实现，后来就出现了许多类似浏览器插件）。

桌面应用是基于 Tauri 的套壳实现，简单来说就是直接在 WebView 中加载网站 URL。通过注入脚本的方式来实现对网站功能的扩展。主要有以下几点：

+   如何加载 URL 到窗口？

+   加载的网址中如何注入脚本？

+   注入脚本中如何调用 Tauri API？

### 应用入口

```
// src-tauri/src/main.rs

#[tauri::command]
pub fn hello(name: String) {
  println!(&quot;Hello, {}!&quot;, name);
}

fn main() {
  tauri::Builder::default()
    .invoke_handler(tauri::generate_handler![hello]) // 注册命令
    .plugin() // 注册插件，如果命令过多可以考虑写成插件，方便管理
    .setup() // 初始化
    .system_tray() // 系统托盘
    .menu() // 系统菜单
    .on_menu_event() // 菜单事件
    .on_system_tray_event() // 托盘事件
    .on_window_event() // 窗口事件
    .run(tauri::generate_context!())
    .expect(&quot;error while running ChatGPT application&quot;);
}
```

```
// src/main.js

import { invoke } from &#x27;@tauri-apps/api&#x27;;

await invoke(&#x27;hello&#x27;, { name: &#x27;lencx&#x27; });
```

### 加载 URL 并注入脚本

```
// src-tauri/src/main.rs

tauri::Builder::default()
  .setup(|app| {
    tauri::WindowBuilder::new(
      app,
      &quot;main&quot;, // 窗口 ID
      tauri::WindowUrl::App(&quot;https://chat.openai.com&quot;.into()) // 加载 URL
    )
      .initialization_script(include_str!(&quot;./scripts/core.js&quot;)) // 注入脚本
      .title(&quot;ChatGPT&quot;) // 标题
      .inner_size(800.0, 600.0) // 窗口大小
      .resizable(true) // 是否可调整窗口大小
      .build()
      .unwrap();
  })
  .run(tauri::generate_context!())
  .expect(&quot;error while running ChatGPT application&quot;);
```

### 注入脚本中调用 Tauri API

这一部分比较复杂，因为 Tuari 的架构设计本身就是为安全而生的，所以如果应用程序选择通过加载远程 URL 的方式来创建窗口时，Tauri 不会为该窗口注入 Tauri API。这部分是从源码中获得的技巧，通过 Tauri 暴露的 __TAURI_POST_MESSAGE__ 底层 API 来模拟出上层 invoke API。代码有点多，也是整个应用的灵魂所在：

```
// src-tauri/src/scripts/core.js

// 生成唯一标识符
const uid = () =&gt; window.crypto.getRandomValues(new Uint32Array(1))[0];

// 转换回调函数，返回一个唯一的标识符
function transformCallback(callback = () =&gt; {}, once = false) {
  const identifier = uid();
  const prop = `_${identifier}`;
  Object.defineProperty(window, prop, {
    value: (result) =&gt; {
      if (once) {
        Reflect.deleteProperty(window, prop);
      }
      return callback(result)
    },
    writable: false,
    configurable: true,
  })
  return identifier;
}

// 模拟 invoke API
async function invoke(cmd, args) {
  return new Promise((resolve, reject) =&gt; {
    if (!window.__TAURI_POST_MESSAGE__) reject(&#x27;__TAURI_POST_MESSAGE__ does not exist!&#x27;);
    const callback = transformCallback((e) =&gt; {
      resolve(e);
      Reflect.deleteProperty(window, `_${error}`);
    }, true)
    const error = transformCallback((e) =&gt; {
      reject(e);
      Reflect.deleteProperty(window, `_${callback}`);
    }, true)
    window.__TAURI_POST_MESSAGE__({
      cmd,
      callback,
      error,
      ...args
    });
  });
}

// 模拟系统弹窗 API
async function message(message) {
  invoke(&#x27;messageDialog&#x27;, {
    __tauriModule: &#x27;Dialog&#x27;,
    message: {
      cmd: &#x27;messageDialog&#x27;,
      message: message.toString(),
      title: null,
      type: null,
      buttonLabel: null
    }
  });
}

// 将模拟的 API 挂载到 window 对象
window.uid = uid;
window.invoke = invoke;
window.message = message;
window.transformCallback = transformCallback;
```

Tauri 套壳 ChatGPT，代码实现到这里，整个应用程序的核心逻辑就算跑通了。即：

1.  tauri::WindowBuilder::new 加载 URL https://chat.openai.com

1.  initialization_script 注入脚本

1.  invoke 调用 tauri::command

1.  tauri::command 实现操作系统文件读写

# 开源浅思

程序员最不缺的就是编码力和创造力，但能够成为独立开发者的人却少之又少。我认为，主要有以下原因：

+   眼高手低，或不屑于去做。那不就是个套壳吗，有什么可搞的？

+   缺少开发独立产品思维，虽然在公司做过的项目挺多，但自己独立完成整个产品闭环时却有点茫然（功能实现，页面交互，界面排版，项目架构，项目推广等等）。

+   对信息的敏锐性，和技术的学习力下降。上班已经那么卷了，下班或周末就会选择躺平，不愿意走出舒适区。

+   缺乏分享意识。虽然平时技术群，各大社区没少吹牛，但能够正真沉淀下来的东西少之又少。

+   以及其他一些因素。

## 行动大于空想

当时我在 Tauri 群里聊开发桌面应用的想法时，有些群友表示不看好，认为已经有人开发过了，你完全可以给别人做贡献（提 PR）。而不是重复造轮子，同期类似项目还有两个：

+   sonnylazuardi/chat-ai-desktop [9] ：基于 Tauri 开发

+   vincelwt/chatgpt-mac [10] ：基于 Electron 开发

做一件事情时，身边必然会出现一些不和谐的声音，但他们的观点并不可以左右你的行动。就个人而言，我不喜欢被束缚，因为提 PR 就意味着你必须按照别人的想法去做，事情会变得不可控（通过/拒绝）。我创建项目的初衷并不是为了服务于人（也没想着会火），主要是为验证自己的一些想法。 迈出第一步，你将拥有无限可能。遇到问题解决问题，一个问题会衍生出一个新问题，这些实战会让你迅速成长 。

## 社区的力量

对于没有任何背景的人而言，项目早期想要获得关注是很困难的一件事情。这时候就需要借助外力，来帮助自己突破 0 到 1 的问题。在早期我做了两件事（向两个开源项目提 issues）：

+   liady/ChatGPT-pdf [11] ：PDF，图片导出功能

+   f/awesome-chatgpt-prompts [12] ：斜杠指令的 prompt 数据源

首先对两个库作者的工作表示感谢，并告知他们我已经将他们所做的工作集成到了 ChatGPT 应用中。Awesome ChatGPT Prompts 作者认为我这个想法很棒，并表示愿意在 README 中添加我的项目链接（ 相互成就，才能走的更远 ）。

项目早期还是比较辛苦的，虽然我仅用半天时间，就发布了 v0.1.0 版本，但接下来就进入了快速迭代期（开发功能，思考交互，回复 issues，Fix Bug 等等）。遇到棘手问题需要查找大量资料，属于边学边开发。那一段时间，人都魔怔了，每天睁开眼睛第一件事就看项目新增了多少 issues。随着项目发展，也有一些小伙伴参与进来，贡献 PR，献计献策。

在这里我想感谢每一个参与或支持开源的人，正是因为 TA 们带来的一丝丝温暖，才能使开源生态不断发展壮大。 做开源是很有成就感的事情，你的一举一动都有改变世界的可能 。

## 产品思维

> 摘自 《流量密码：ChatGPT 开源的一些思考》

+   产品闭环 ：它可以很小，功能可以很简陋，但是必须要形成最小闭环，保证其可用性（产品核心功能可以正常使用）。

+   速度要快 ：开发速度，更新速度，问题相应速度都要快，因为它可以帮助你抢占第一波用户（种子用户积累很重要，可以形成口碑，帮助产品二次传播）。

+   用户体验 ：这是需要花心思的，虽然你是一名开发者，但是你更是一名使用者。所以没有产品，你就是产品；没有设计，你就是设计（你就是用户，甚至你要比用户更懂用户，学会取舍）。

+   产品计划 ：你对产品未来方向的规划，计划加入什么牛逼的功能，需要在文档里写清楚。它就相当于是在给用户画饼，可以打动一些想要长期追随它的用户（注意：画饼不代表天马行空的想法，而是根据实际情况，可实现但因时间原因暂时无法实现的计划）。

+   差异化 ：因为当你发现机会的时候，别人可能早已经在里面开始收割了，所以产品功能的差异化，将是你的突破口（人无我有，人有我有优）。

+   稳定性 ：产品的初期的架构很重要，它可能会伴随其一生。重构有时候并不现实，因为它需要牵扯到很多的历史包袱，数据兼容，人力成本等等（可扩展性很重要）。

## 如何学习？

现在的我们正在面临各种碎片化的冲击。海量信息，短视频让人的思维愈发碎片化（许多人表示很难静下心来读一篇大几千字，上万字的文章，更别谈思考或输出了）。“卷”这个字也是近些年最火的一个字，没有信息让人焦虑，信息爆炸会让人变得更加焦虑。

我也是在开发桌面应用之后，才开始接触 AI 这个领域。写的文章多了（大约输出了几十篇 AI 系列文章），也莫名成了别人眼中的大佬（自己有多菜只有自己清楚）。

未知知识学习 = 扩展阅读 + 信息源 + 已有知识 + 经验推导

+   扩展阅读 ：善用 搜素引擎 ChatGPT，检索文章中的未知术语或名词（不过我更倾向于在 ChatGPT 给出结论后，自己再用搜索引擎复核一下）

+   信息源 ：尽可能去靠近信息源，关注领域大牛。信息具有时效性，二手信息会造成信息差，交智商税，走弯路是必然的。

+   分享输出 ：分享是最高效的学习方式。动手写或给别人讲，都会让你发现很多之前注意不到的细节（看往往是浮于表面，细节和坑都隐藏在更深处）。

## 什么是价值？

> 将价值简单粗暴地与金钱划上等号，我不知是对是错，但丢了根基，一切都不过是空中楼阁罢了。

在我看来价值是一个很抽象的东西，但是往往人们都喜欢用结果去衡量一个东西的价值（比如有多少用户，赚了多少钱等等）。举一个简单的例子：我经常混迹在 GitHub 社区，也看到过许多很牛的项目，是它们撑起了海量的上层应用，但是它们的关注度却不高，你觉得它们有价值吗？也有许多博眼球项目，含金量不高，却获得了巨大的关注度，你觉得它们价值高吗？

在这个以结果为导向的世界里，不管做什么事情，都避免不了被问到：“你做这个东西有什么价值？”。当你有了比较心，得失心，在做一件事情时就会变得畏手畏脚，甚至不屑去做。想的多不是坏事，当你在试图最大化利益时，往往也会丢掉许多可能性。人们常说的机遇是什么？我认为它就是： 一个人在积累知识，学习技能的同时，善于用发展的眼光去观察这个快速变化的世界，当你有能力去解决某个问题时，它对你来说就是一次机遇。

## 结束语

身为一名程序员我很自豪，虽然足不出户，指尖却有着可以改变 世界 (可能有点大了) 自己的力量。即使不能实现，将其作为努力的目标也不错。

### References

[1]

rsw-rs: https://github.com/rwasm/rsw-rs

[2]

Tauri 教程: https://github.com/lencx/tauri-tutorial

[3]

Rust 在前端: https://github.com/lencx/rust-fe

[4]

Tauri 文档: https://tauri.app/v1/guides

[5]

Tauri 源码: https://github.com/tauri-apps/tauri

[6]

Tauri 开源项目: https://github.com/tauri-apps/awesome-tauri

[7]

Tao: https://github.com/tauri-apps/tao

[8]

Wry: https://github.com/tauri-apps/wry

[9]

sonnylazuardi/chat-ai-desktop: https://github.com/sonnylazuardi/chat-ai-desktop

[10]

vincelwt/chatgpt-mac: https://github.com/vincelwt/chatgpt-mac

[11]

liady/ChatGPT-pdf: https://github.com/liady/ChatGPT-pdf

[12]

f/awesome-chatgpt-prompts: https://github.com/f/awesome-chatgpt-prompts