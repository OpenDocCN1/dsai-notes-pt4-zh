
# 在 Windows 上使用 Flutter 开发安卓应用

> 来源：[https://now.feishu.cn/docx/XGJDd85egoCj0WxVdI2c5cIinfd](https://now.feishu.cn/docx/XGJDd85egoCj0WxVdI2c5cIinfd)

![](img/2a777d6e001e489da9a5bb382fba5499.png)

官方安装指南：https://docs.flutter.dev/get-started/install/windows/mobile

## 1. 下载安装 Android Studio

https://developer.android.com/studio?hl=zh-cn

## 2. 安装 Flutter SDK

![](img/83e0f40f526b3f5116a86354d9f2d441.png)

## 3. 环境检查

执行 `flutter doctor`，按提示完成环境安装。

如果提示：

Visual Studio not installed; this is necessary to develop Windows apps.

请访问下载页面：https://visualstudio.microsoft.com/downloads

![](img/46a888b515b5767bad96089e40a7db96.png)

## 4. 安装 Windsurf

![](img/1e26240b7492fd13e2f40473d1a4575b.png)

### 4.1 安装项目依赖

在项目根目录（`camera_app` 文件夹）下运行：

`flutter pub get`

### 4.2 运行

先启动模拟器：

`flutter emulators --launch Medium_Phone_API_35`

查看已连接设备：

`flutter devices`

运行应用：

`flutter run -d Pixel_3a`

初始化 Flutter 项目：

`flutter create .`

## 参考资源

- Flutter 第一个 Codelab：https://codelabs.developers.google.com/codelabs/flutter-codelab-first?hl=zh-cn#2
- Flutter 配置 Gradle 镜像，解决国内下载慢问题：https://juejin.cn/post/7299346813261676544
- Android Java 和 Gradle 迁移指南：https://docs.flutter.dev/release/breaking-changes/android-java-gradle-migration-guide

## 常见问题

如何修改为合适的 Java 版本？
