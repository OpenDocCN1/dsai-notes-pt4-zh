
# PC上使用谷歌浏览器插件获取Cookie

> 来源：[https://sqivg8d05rm.feishu.cn/docx/Ky8rdwLSIoHXXkxfSOAciySYnQg](https://sqivg8d05rm.feishu.cn/docx/Ky8rdwLSIoHXXkxfSOAciySYnQg)

1. 使用谷歌浏览器、edge浏览器、指纹浏览器或其他 Chromium 内核浏览器，打开 [谷歌插件商店](https://chromewebstore.google.com/)
1. 搜索类似 cookie edit 这样的关键词
1. 安装插件 cookie 编辑插件，比如 [cookie-editor](https://chromewebstore.google.com/detail/cookie-editor/hlkenndednhfkekhgcdicdfddnkalmdm)、easy cookie editor 等
1. 如果提示要获取权限，要通过

![](img/d18c854410a155fdf60fe8e6cfb739e4.png)

1. 打开 [claude官网](https://claude.ai/) 并登陆
1. 浏览器右上角找到你安装的 cookie 插件，点击它，就可以看到 cookie

![](img/e82645fc96ab9dccd1a492f70d75ba93.png)

![](img/389833d9a75b33362f00ecf1531c5bf6.png)

1. 在获取的 `sk-ant-sid01-xxxxx` 参数前加上 `sessionKey=`，变成类似 `sessionKey=sk-ant-sid01-xxxxx` 的形式
1. 填入 Clewd 反代里
1. 其他浏览器也是同理

## 指纹浏览器安装 Cookie 插件

1. 打开指纹浏览器
1. 如下图点击资源，再点击扩展中心

![](img/ae527f0ee8992dc305f1718ce91e52ce.png)

1. 如下图，直接就会有，没有的话在右上角搜索一下即可

![](img/0bf1b51878bcb4a9139110f069039932.png)

1. 这样注册完直接就可以抓 Cookie

![](img/29c4485cbaaad8d4067fa4f028f93f74.png)
