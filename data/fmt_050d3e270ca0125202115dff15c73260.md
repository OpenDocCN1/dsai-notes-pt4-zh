
# Netlify部署NextJS静态网站

> 来源：[https://now.feishu.cn/docx/YGlQdgrnNoOBW3xB6XucSYbWnoc](https://now.feishu.cn/docx/YGlQdgrnNoOBW3xB6XucSYbWnoc)

![](img/58d3140750a45f7e34ce0e08483e21e8.png)

我已经踩过两次坑了，项目本地写好了部署到Netlify就冲突。

要在新建项目/部署项目前确认环境变量的设置：

```
NPM_FLAGS
--legacy-peer-deps
```
