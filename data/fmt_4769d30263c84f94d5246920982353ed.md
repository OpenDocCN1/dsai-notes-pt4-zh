
# Vibe Coding（氛围编程）使用 `Vite` + `React` + `IndexedDB` 实现一个本地使用的 Jira Clone

> 来源：[https://now.feishu.cn/docx/Z3rwdRUycoFDB8xkekzcjjcXnXb](https://now.feishu.cn/docx/Z3rwdRUycoFDB8xkekzcjjcXnXb)

## 项目基本功能演示

![](img/4facff5a957215a214ff84a0660ef926.png)

[`jira-clone-localDB.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZONCbugCfoo86Rxm2wKcL59xnSQ)

项目初衷是写一个自己的本地项目任务管理工具，可以实现Jira式流畅的拖放动作和项目任务的本地数据库保存

## Step 01 在 `bolt.new` 生成一个项目框架

+   使用`React`构建UI
+   使用`DnDkit`实现流畅的拖放动作

## Step 02 使用Cursor进行项目完善

+   `Vite`作为构建工具
+   数据库初始化
+   我本来打算使用`SQLite`
+   Cursor给我改成了`IndexedDB`
+   `IndexedDB`是一个强大的浏览器数据库
+   UI优化

## Step 03 可以使用`Electron`打包为跨平台应用

## GitHub 分享地址（仅作为参考，整个流程在整理中）

https://github.com/Neonity2020/projecthub
