# Lama Cleaner免费开源图片修复工具

> 来源：[https://diwfpqo78m.feishu.cn/docx/SHI5djdebojnq8xmEYqcWIw9nCg](https://diwfpqo78m.feishu.cn/docx/SHI5djdebojnq8xmEYqcWIw9nCg)

原文链接： [https://github.com](https://github.com/Sanster/lama-cleaner)/Sanster/lama-cle...

# Lama Cleaner

Lama Cleaner 是一款免费、开源且完全可自行托管的修复工具，由最先进的 AI 模型提供支持。您可以使用它从您的照片中删除任何不需要的对象、缺陷、人物或擦除和替换您照片上的任何内容。

由 SOTA AI 模型提供支持的免费开源修复工具。

[🎦 点击观看视频](https://user-images.githubusercontent.com/3998421/196976498-ba1ad3ab-fa18-4c55-965f-5c6683141375.mp4)

## 赞助

## 特征

+   完全免费开源，完全自托管，支持CPU & GPU & M1/2

+   [Windows 一键安装程序](https://lama-cleaner-docs.vercel.app/install/windows_1click_installer)

+   多个 SOTA AI [模型](https://lama-cleaner-docs.vercel.app/models)

+   擦除模型：LaMa/LDM/ZITS/MAT/FcF/Manga

+   擦除和替换模型：Stable Diffusion/Paint by Example

+   [ ](https://lama-cleaner-docs.vercel.app/plugins)后处理 [插件：](https://lama-cleaner-docs.vercel.app/plugins)

+   [RemoveBG ](https://github.com/danielgatis/rembg): 移除图片背景

+   [RealESRGAN ](https://github.com/xinntao/Real-ESRGAN): 超级分辨率

+   [GFPGAN ](https://github.com/TencentARC/GFPGAN): 人脸修复

+   [RestoreFormer ](https://github.com/wzhouxiff/RestoreFormer): 面部修复

+   lama-cleaner-docs 的更多功能

+

## 快速开始

Lama Cleaner 只需两个命令即可轻松使用 SOTA AI 模型：

```
# In order to use the GPU, install cuda version of pytorch first.
# pip install torch==1.13.1+cu117 torchvision==0.14.1 --extra-index-url https://download.pytorch.org/whl/cu117
pip install lama-cleaner
lama-cleaner --model=lama --device=cpu --port=8080
```

就这样，Lama Cleaner 现在运行在 http://localhost:8080

在lama-cleaner-docs 查看所有命令行参数

## 发展

仅当您计划修改前端并自行重新编译时才需要。

### 前端

前端代码修改自 c[lean](https://cleanup.pictures/)up.pictures ，您可以  在这里 体验他们出色的在线服务 。

+   安装依赖项： cd lama_cleaner/app/ && pnpm install

+   启动开发服务器： pnpm start

+   建造： pnpm build