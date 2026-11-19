
# 开源项目

> 来源：[https://xmgtoagi.feishu.cn/docx/X36pdAFzWoFGpDxrd8Gcjy89nmg](https://xmgtoagi.feishu.cn/docx/X36pdAFzWoFGpDxrd8Gcjy89nmg)

## AI画廊

### 介绍

AI画廊是一个图片管理、相册管理、AI绘画成果管理项目，2024年新开源项目，可以将自己的照片、摄像作品、A绘画I作品从后端上传，展示到前端，后端实现了图片分类管理、图片和关键词批量上传，如用于AI绘画方面，可一键复制关键词，且移动端自适应。

+   前端体验地址：`http://47.109.46.37/webhome`

+   后端体验地址：`http://47.109.46.37`

+   整体效果如下：

![](img/63cd4df6d4bf78f943fdbfe388a4c058.png)

![](img/30daf001c29d3322667690ff31d44578.png)

![](img/f23972e629418bf10872c9e0c6e06c35.png)

![](img/da3f88c43d088fecd84231273af1b4d9.png)

### 开发计划

+   增加🛰️扫码登录功能

+   增加看广告后下载壁纸功能，实现广告收益

+   增加图片水印功能，防止图片被盗

### 软件架构

+   在开源项目芋道源码的基础上改造而成，

+   采用 `vue-element-plus-admin`

+   实现改换 saas，自动引入等功能

+   使用 `Element Plus` 免费开源的中后台模版

感谢`yudao`框架

原框架地址：`https://gitee.com/yudaocode/yudao-ui-admin-vue3?_from=gitee_search`

### 环境搭建

1.  先初始化`mysql`教程`ai-wallpaper-open/ruoyi-vue-pro/sql/mysql/ai-picture.sql`

1.  安装启动`redis`

### 前端启动

```
npm install
```

![](img/bc682d58a9b7f50cf2e07b92a53814bd.png)

```
npm dev
```

![](img/f3b49661d1e56dd6d0f4730bb1c413b2.png)

+   管理端访问地址：`localhost:80`

+   用户名`admin`，密码：`admin123`

+   展示端访问地址：`localhost:80/webhome`

### 后端启动

+   修改数据库配置信息

![](img/7c93fee4e5d4d48d8180fcbad2b57776.png)

+   启动后端项目

![](img/c04476129201bd34623c91e8f5711969.png)

### 使用说明

后端管理图片类型和图片，以及pc、移动端展示效果

![](img/aa279deecaf215dcf2cf83e0a79930be.png)

![](img/33ae53475ce3453ff18c1857c5ac106c.png)

![](img/e98191a34b25d8abc3c06f2f371b66e0.png)

![](img/23a710a319e3a97e94c8d9a392e73903.png)

![](img/137ab3889d16e50c9e53844d657891d3.png)

![](img/198da561032b809790c6eac3bf5e235f.png)

### 环境参与贡献

1.  Fork 本仓库

1.  新建 `Feat_xxx` 分支

1.  提交代码

1.  新建 Pull Request

### 版权说明

AI画廊版权归我所有，可商用，但请注明出处。

### 联系我

提需求、问题反馈，欢迎联系我（🛰️：`xmgcode88`）。如有技术问题，可扫码进技术交流群

![](img/dba56ebbe419f3ff8f701aa21a096488.png)

### 支持作者

如果对你有用的话，也可以请作者喝杯奶茶哦，开源不易！！！！

![](img/96f83e092656d0068c1961eb494e18cb.png)
