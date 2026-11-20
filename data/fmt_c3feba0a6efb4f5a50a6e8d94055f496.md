
# 开源揭秘：35k+ Stars ChatGPT 桌面应用

> 来源：https://geekbang.feishu.cn/docx/DGEod3WgCoIgYDxKCJmcfzUfn5b

> 🤵作者：lencx

> 📃 作者介绍：AI 初学者，喜欢分享，热爱开源（GitHub Stars 60k+）。

> ⏰ 收录时间：2024-06-05 17:15:15 (UTC+8)

> 🔗 原文链接：https://mp.weixin.qq.com/s/gEDYO4o65ZUX9v2T4gly-Q

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

`Web` -> `Rust` -> `WebAssembly` -> `Tauri` -> `ChatGPT`

+   结缘 `Rust` ：我非科班出身，毕业后从培训机构接触 `Web`，开始入行前端开发。因计算机基础薄弱，故希望学习一门系统语言，来提升一些自己对底层的认知。

+   开发 `rsw` 插件 ： `rsw-rs` [1] 算是我用 `Rust` 开发的第一个比较正式的工具。它是一个 `CLI`，旨在解决使用 `Rust` 开发 `WebAssembly` 时的热更新问题，提升开发体验。

+   `Tauri` 探索 ：在开发 `rsw-rs` 之后，感觉 `WebAssembly` 应用于实际生产对我来说似乎有点遥远。为了进一步在实战中学习 `Rust`，我开始学习 `Tauri`，它是基于 `Rust` 实现的跨平台应用开发框架，可以使用 `Web` 技术（`React`、`Vue` 等）来开发应用。分享即是学习，我写下了 `Tauri` 教程 [2] 和 `Rust` 在前端 [3] 系列文章，也结识到了很多新朋友。

## 如此巧合

> 成功具有偶然，不可复制性。

+   巧合一：2022 年 11 月份 `ChatGPT` 发布，朋友圈陆陆续续有人在刷屏分享，刚开始没太在意（以为是营销手段）。后来还是在好奇心驱下，我注册了一个账号，体验了一番。

+   巧合二：体验过后，发现事情并不简单，而后就有了结合 `Tauri` 做桌面应用初步想法。在此之前，我已经研究 `Tauri` 大半年时间，为了实现一些有趣的功能（加载远程 `URL`），甚至啃了很多 `Tauri` 源代码。

+   巧合三：使用 `ChatGPT` 后，本能地开始了解它的一些周边生态（比如 `prompt` 或者插件），这也为桌面应用的功能迭代带来诸多灵感。

+   巧合四：我失业了，所以有大量的时间来开发这个项目。

这里有几个核心点：

+   好奇心驱使 ：对一个新事物保持敏锐度非常重要，在领先的这段时间里，你就有很多东西可以去做。也正是这份好奇心让我早早入场，为桌面应用的开发埋下种子。

+   善于发现问题 ：在使用一个产品时能够站在用户的角度去思考提出问题。任何产品，当你使用有痛点时，可能就是一次机会（比如：`ChatGPT` 想要输入 `prompt`，而 `prompt` 需要自己从别的地方不断地进行复制粘贴，如果存在大量高频使用的 `prompt`，这将是低效的）。犹豫不决时，建议先迈出第一步，思路往往会在做的过程中被打开。

+   恰好能力所及 ：技术是死的，而人是活的。技术要想产生价值必须要依附于所能解决的问题。

+   发散式思维 ：在了解一个新技术或事物后，一定要去了解它的生态和周边，这些生态都将是你灵感和开发的源泉。

> 🤔 思考

> 举一个不太恰当的例子：

> 国内开始大规模爆发 `ChatGPT` （全民 `GPT`）热潮应该是在 2023 年 2，3 月份左右。很多人其实并不清楚 `ChatGPT` 到底是什么东西，蜂拥而来，造成最大的一个问题就是“盲从会产生盲信”。利用所谓的信息差，`AI` 割韭菜也开始迎来了爆发式增长（朋友圈几乎每天都会被各种付费 `AI` 课程，付费星球刷屏，口号都差不太多：`AI` 时代来临，如果你不学习就会被淘汰，购买我们的 `xxx`，就可以让你掌握秒杀 90% 人的技能，`AI` 赚钱不是梦）。

> 割韭菜就是充分利用了信息差，虽然镰刀可恨，但韭菜就真的无辜可怜吗？镰刀之所以能够成为镰刀，是因为他们有普通人所不具备的能力：

+   敏锐度：对信息的感知优于常人，可以蹭一切热点来实现变现的最终目的。

+   行动力：迅速落地，常见形式：

+   知识付费（课程，知识星球等）

+   流量裂变（只需分享小程序或网站就可以免费使用 `xxx` 功能）

+   ...

+   宣传力：营销文案高手，善于烘托营造氛围，比如：

+   紧迫感：`AI` 时代来临，截止到今日已经有 `xxx` 位小伙伴加入了，如果你不加入，就会被时代抛弃。

+   增值服务：我们内容如果做成课程，售价都在 `xxx` 元，现在你只需花费很小的钱，你就可以打包享受到 `xxx`，`xxx` 以及 `xxx` 服务，这些都是打包赠送。

+   ...

# 技术原理

## Tauri 简介

学习新技术，看文档是第一要义（重要的事情说三遍：看文档！看文档！看文档！），不过只看 `Tauri` 文档 [4] ，有点不太够用，有能力的还是推荐去读一些 `Tauri` 源码 [5] 和一些 `Tauri` 开源项目 [6] ，会发现很多小技巧。这里不过多展开，简单列举两个特点：

+   跨平台 ：`Tauri` 支持 `Windows`、`macOS` 和 `Linux`，`UI` 部分使用 `Web` 技术（`React`、`Vue` 等）来开发。`2.0` 版本已支持移动端（`Android` 和 `iOS`）。

+   安装包体积小，内存占用小 ：`Hello World` 应用一般在 `3M` 左右。但调用系统内置浏览器，兼容性会差一些。

+   系统菜单、系统托盘、权限管理、自动更新等等。

> 📌 Electron vs Tauri

+   `Electron` = `Node.js` + `Chromium`

+   `Tauri` = `Rust` + `Tao` + `Wry`

+   `Tao` [7] : 跨平台应用程序窗口创建库，支持所有主要平台，如 `Windows`、`macOS`、`Linux`、`iOS` 和 `Android`。

+   `Wry` [8] : 跨平台 `WebView` 渲染库，支持所有主要桌面平台，如 `Windows`、`macOS` 和 `Linux`。

## 项目结构

项目结构简单，除标准的前端项目结构，外加 `src-tauri` 目录：

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

要完成 `Web` 网页到桌面应用的蜕变，和系统的通信必不可少，主要有以下两种通信方式：

+   `tauri::command` & `invoke` : 前端通过 `invoke` API 调用 `Rust` 的 `command` 方法。`command` 可以接受参数并返回值。

+   `Event`: `emit` & `listen` : 双向通信（`Rust` <-> `WebView`），`emit` 发送事件，`listen` 监听事件。

> 😅

`Tauri` 中所有的 `API` 都是异步的，在前端均以 `Promise` 的形式返回。

### `tauri::command` & `invoke`

```
// src-tauri/src/main.rs

#[tauri::command]
fn hello(name: String) -> String {
  format!("Hello, {}!", name)
}

fn main() {
  tauri::Builder::default()
    // 注册命令
    .invoke_handler(tauri::generate_handler![hello])
    .run(tauri::generate_context!())
    .expect("failed to run app");
}
```

```
// src/main.js

import { invoke } from '@tauri-apps/api/tauri';

// 调用 Rust 的 hello 方法
// 输出：Hello, ChatGPT!
await invoke('hello', { name: 'ChatGPT' });
```

### `Event`: `emit` & `listen`

#### `js` 之间通信

```
// src/main.js

import { emit, listen } from '@tauri-apps/api/event';

// 监听事件
const unlisten = await listen('click', (event) => {
  // output: Hello, ChatGPT!
  console.log(event.theMessage);
})

// 发送事件
emit('click', {
  theMessage: 'Hello, ChatGPT!',
})
```

#### `JS` 和 `Rust` 之间通信

`Rust` -> `JS`

```
// src-tauri/src/main.rs

// 获取特定窗口，发送事件
app.get_window("main").unwrap().emit("rust2js", Some("Hello from Rust!"));
```

```
// src/main.js

import { listen } from '@tauri-apps/api/event';

// 监听事件
listen('rust2js', (event) => {
  console.log(event.theMessage); // output: Hello from Rust!
})
```

`JS` -> `Rust`

```
// src/main.js 

import { emit } from '@tauri-apps/api/event';

// 发送事件
emit('js2rust', {
  theMessage: 'Tauri is awesome!',
})
```

```
// src-tauri/src/main.rs

// 获取特定窗口，监听事件，json 数据会以字符串形式返回
app.get_window("main").unwrap().listen("js2rust", |msg| {
  // output: Event { id: EventHandler(xxxxxxxx), data: Some("{\"theMessage\":\"Tauri is awesome!\"}") }
  println!("js2rust: {:?}", msg);
});
```

## 核心实现

项目灵感来自于机器人指令，如果经常玩 TG 或者 Discord 的朋友应该都比较熟悉（通过输入斜杠指令来调用机器人的功能。比如：`/help` 、 `/start` 、 `/ping` 等等）。而 `ChatGPT` 经常需要重复性输入 `Prompt`，所以我想到了通过指令的方式来调用 `Prompt` 的功能（据我所知，这个功能应该是我最早实现，后来就出现了许多类似浏览器插件）。

桌面应用是基于 `Tauri` 的套壳实现，简单来说就是直接在 `WebView` 中加载网站 `URL`。通过注入脚本的方式来实现对网站功能的扩展。主要有以下几点：

+   如何加载 `URL` 到窗口？

+   加载的网址中如何注入脚本？

+   注入脚本中如何调用 `Tauri` API？

### 应用入口

```
// src-tauri/src/main.rs

#[tauri::command]
pub fn hello(name: String) {
  println!("Hello, {}!", name);
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
    .expect("error while running ChatGPT application");
}
```

```
// src/main.js

import { invoke } from '@tauri-apps/api';

await invoke('hello', { name: 'lencx' });
```

### 加载 `URL` 并注入脚本

```
// src-tauri/src/main.rs

tauri::Builder::default()
  .setup(|app| {
    tauri::WindowBuilder::new(
      app,
      "main", // 窗口 ID
      tauri::WindowUrl::App("https://chat.openai.com".into()) // 加载 URL
    )
      .initialization_script(include_str!("./scripts/core.js")) // 注入脚本
      .title("ChatGPT") // 标题
      .inner_size(800.0, 600.0) // 窗口大小
      .resizable(true) // 是否可调整窗口大小
      .build()
      .unwrap();
  })
  .run(tauri::generate_context!())
  .expect("error while running ChatGPT application");
```

### 注入脚本中调用 `Tauri` API

这一部分比较复杂，因为 `Tauri` 的架构设计本身就是为安全而生的，所以如果应用程序选择通过加载远程 `URL` 的方式来创建窗口时，`Tauri` 不会为该窗口注入 `Tauri` API。这部分是从源码中获得的技巧，通过 `Tauri` 暴露的 `__TAURI_POST_MESSAGE__` 底层 API 来模拟出上层 `invoke` API。代码有点多，也是整个应用的灵魂所在：
