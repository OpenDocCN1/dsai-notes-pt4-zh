# 3分钟搞定 Notion 自定义域名

> 来源：[https://xodnytdcaw.feishu.cn/docx/IOsydYjTxoQXFpx5Jbec9LAwndf](https://xodnytdcaw.feishu.cn/docx/IOsydYjTxoQXFpx5Jbec9LAwndf)

## 一.准备工作

域名

notion分享页面

## 二.创建 Workers

来到Cloudflare，点账户主页-计算（Workers和Pages）-Workers

![](img/0d1a3e1baf81ae7f18df5aa513d878cf.png)

名称随便写，我这里就写notion(添加路由的Worker就是这个名称）

![](img/c30b4e87f548a6e6bfec457decd84b19.png)

## 三.设置 Workers 路由

账户主页-点击你的域名-Workers 路由-添加路由

![](img/f9c72c42f180d9c9e6925628671dd423.png)

填入域名，注意网址后面加 /*Worker 就是那个贴代码后部署的名称

![](img/cb1eccf6a1883b8aedb9fc3b6f95e880.png)

## 四.管理Workers

点击代码修改，然后全选清空里面代码

![](img/a525a4cf3e4faf6e9d886b373716f4e8.png)

打开这个网站：[点击打开](https://fruition-git-master-stephenou.vercel.app/)  填写你的自定义域名与notion分享链接，之后复制生成的代码，粘贴的修改代码的区域

![](img/2850fd03f5309e82b5ec8ebe79847e62.png)

添加自定义域

![](img/dd054cff1452a1c2d1b5ef19c6e637f6.png)

至此，你就可以通过自己的域名来访问 Notion 主页

体验地址：notion.dqtx.cc