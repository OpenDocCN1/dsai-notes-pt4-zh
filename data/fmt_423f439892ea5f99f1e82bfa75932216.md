
# Clewd 入门

> 来源：[https://sqivg8d05rm.feishu.cn/docx/TcGTdio3roe2shxnOARcZdYGn4d](https://sqivg8d05rm.feishu.cn/docx/TcGTdio3roe2shxnOARcZdYGn4d)

> 本文内容基于[**Clewd修改版**](https://github.com/teralomaniac/clewd)，并参考坎佬的**[**Clewd教程**](https://rentry.org/teralomaniac_clewd) / [**Clewd更新与功能说明**](https://rentry.org/teralomaniac_clewd_ReleaseNotes)。

部署教程请参考[部署 Clewd 和 酒馆](https://sqivg8d05rm.feishu.cn/wiki/OMBWwQFN0i44MKk9GURcEhYindg)，多Cookie一键整理格式：https://clewd.pages.dev/

# Clewd 后台与更新

## Replit云端

登录后点击`My Repls`，找到自己的`clewd`项目

![](img/2f398293b18ed7fea78d6f0fa4361c3d.png)

![](img/9f69c62cb028d3febf044975065058e0.png)

选择 `Tools` 栏中的 `Shell`，手机端右滑至 new tab 页面，选择 `Shell`。

在`Shell`内输入`bash update.sh`并回车。

![](img/936bc39bd086e93b0f5b938caef72717.png)

![](img/395ddd22437f1eeeded4a5215108599a.png)

如遇更新后报错：

1. 建议直接[重新Fork](https://replit.com/@waterqiu/clewd)，教程参考[Replit 部署 Clewd 与酒馆](https://sqivg8d05rm.feishu.cn/wiki/E925wr6MKixS44kJUBtcB0uYnQe)。

1. 可尝试更新`node`版本，在 `Shell` 中输入：

```
npm init -y && npm i --save-dev node@20 npm@10 && npm config set prefix=$(pwd)/node_modules/node && export PATH=$(pwd)/node_modules/node/bin:$PATH
```

## 本地

尝试使用`clewd`文件夹内的 `update.bat` 进行更新，如果更新不了，就下载 Clewd修改版 覆盖文件，有什么更好的方法欢迎补充。

# 配置文件

## 添加Cookie

### Replit 添加Cookie

点击 `Tools` 栏中的 `Secrets`，手机端滑到最右侧点击 `Tools` 栏中的 `Secrets`。

点击 `Edit` 编辑，以`"cookie1"`,`"cookie2"`,`"cookie3"` 的格式填入，cookie之间用英文逗号隔开。

推荐使用多Cookie一键整理格式：https://clewd.pages.dev/。

广告：[淘宝2.49元Cookie店](https://item.taobao.com/item.htm?ft=t&id=749509289037)。

![](img/971f10433f32c031a2f31b85130e1ca2.png)

### 本地等其他平台添加Cookie

找到 `Clewd` 文件夹中的 `config.js` 文件并打开。

在 `CookieArray` 的 `[]` 中以`"cookie1"`,`"cookie2"`,`"cookie3"` 的格式填入，Cookie之间用英文逗号隔开。

推荐使用多Cookie一键整理格式：https://clewd.pages.dev/

![](img/ac535f08b73de59110ac848b73993de0.png)

## 如何查看cookie饼干是否过期、被封

[如何查看cookie饼干是否过期、被封](https://sqivg8d05rm.feishu.cn/wiki/UrRBwtwDaib92pkAZDPcylnNn4c)

## 如何为`clewd`添加秘钥

[如何为`clewd`添加秘钥](https://sqivg8d05rm.feishu.cn/wiki/AZJcwTtVUiyrNUkuteBcdERgnvf)
