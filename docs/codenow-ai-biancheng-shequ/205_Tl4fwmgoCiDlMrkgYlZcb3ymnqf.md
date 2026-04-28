# NextJS 15降级使用React 18

> 来源：[https://now.feishu.cn/docx/Wue3dCXbUofRt6xEspJcMoYznye](https://now.feishu.cn/docx/Wue3dCXbUofRt6xEspJcMoYznye)

最新的NextJS 15绑定了React 19RC（前瞻预览版）很多插件、组件是不支持最新的React 19RC的，会造成依赖的不兼容，所以我们新建项目时需要先对React来降级到版本18，这样就可以支持大多数的依赖了

```
npm uninstall react react-dom
npm install react@18.2.0 react-dom@18.2.0
```

先卸载react再npm 安装React 18，OK，let's build!