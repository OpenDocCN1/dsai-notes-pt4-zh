
# OpenList保姆级教程

> 来源：[https://xodnytdcaw.feishu.cn/docx/OO1idlXsEon1KcxNOnXcuJmOnUK](https://xodnytdcaw.feishu.cn/docx/OO1idlXsEon1KcxNOnXcuJmOnUK)

## 一、本地部署

### 1. 安装OpenList

下载地址

https://github.com/OpenListTeam/OpenList/releases/tag/v4.0.8

在安装目录使用`cmd`启动服务：

```
openlist server
```

浏览器访问地址：127.0.0.1：端口号

账号：`admin`  密码：`cmd`会呈现（后台改）没有的话在`OpenList\daemon\start.log`

在安装目录，新建VBS脚本，发送快捷方式到桌面，然后剪贴到自启动文件夹（`shell:startup`）

```
Dim ws
Set ws = Wscript.CreateObject("Wscript.Shell")
ws.run "openlist.exe server", vbhide
Wscript.quit
```

### 2. 网盘挂载

这个环节当中最重要的是添加根文件夹ID，一般都是按下F12，网络，最新chrome浏览器都是在在载荷里，比如兰奏云的对应`folder_id`，迅雷的对应`parent_id`，夸克的一般是分享链接的后面的一段数字。

![](img/0f6d6a12d1fdc9ee286a066b9e14b80d.png)

[蓝奏云](https://oplist.org/zh/guide/drivers/lanzou.html)

[迅雷](https://oplist.org/zh/guide/drivers/thunder.html)（教程多看2遍，不过容易掉，不建议挂载）

https://oplist.org/zh/guide/drivers/

### 3. 磁盘映射

用到的软件raidrive

https://www.raidrive.com/

先开放权限，网页-用户-admin-编辑-Webdav 读取/Webdav 管理打勾

raidrive软件的设置如下

![](img/b37434e5c924f830669a36fb7e3d8cbb.png)

### 4. PotPlayer播放

打开软件，新建专辑，然后选择“FTP/WebDAV/HTTP搜索”，填写主机路径与端口，账号与密码，这样，你网盘的资源就直接同步过来了。

![](img/352b2e720d47fa31397d76c147a16c29.png)

> 注意：

> [映射的磁盘](https://pan.xunlei.com/s/VOVYAPfBHqH7P09owGophrIQA1?pwd=8imi)默认无图标，磁盘图标自定义修改工具：点此下载

> [raidrive](https://pan.xunlei.com/s/VOVYAAAWmY2hq78N5lqBiOaWA1?pwd=eyji#)这款软件的有弹窗广告，可用火绒弹窗拦截:点此下载

## 二、云端部署（公网访问）

### IPV6直接访问

`http://[IPV6地址]:5244` 或花生壳内网穿透（IPV6地址查询命令`ipconfig`）

### 服务器部署

还有一种方法就是部署云应用或者部署服务器上，再买个域名，然后域名解析，通过域名访问

性价比域名：[Spaceship](https://www.spaceship.com/zh/domain-search/?query=&beast=false&tab=domains)

莱卡云服务器教程：[点此教程](https://blog.lcayun.com/3868.html)

雨云服务器：[点击直达](https://www.rainyun.com/NzYxMDg2_)

## 三、界面美化

参考1：https://blog.mmoe.work/oplist-beautification/、

参考2：https://github.com/wugeng20/openlist-glassmorphism-theme

Alist常用样式代码 :https://blog.itvv.cn/archives/491

自用皮肤开源仓库：https://github.com/dqtx760/oplist-Neumorphism

![](img/2dcbcac8c00f3a76b2b15d008136831e.png)

### 设置-全局

自定义头部

```
<style type="text/css">
body {
  /* 新拟态核心色调 */
  --neumorph-bg: #f0f0f3;
  --shadow-dark: rgba(163, 177, 198, 0.5);
  --shadow-light: rgba(255, 255, 255, 0.8);

  /* 仅修改背景部分（不影响 flex/布局） */
  background: var(--neumorph-bg) !important;
  background-image: none !important;  /* 确保无额外背景图干扰 */
  
  /* 纯背景新拟态（不干扰内容） */
  position: relative;  /* 确保背景覆盖完整 */
}

/* 添加一个伪元素作为背景层（避免影响内容布局） */
body::before {
  content: "";
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;  /* 确保在底层 */
  
  /* 新拟态背景效果 */
  background: var(--neumorph-bg);
  box-shadow:
    inset 6px 6px 12px var(--shadow-dark),
    inset -6px -6px 12px var(--shadow-light);
}

/* 以下所有代码保持原样，不修改宽度、高度或布局 */
.markdown-body, 
.main-content {
  width: 100%;
  max-width: 800px;
  padding: 30px;
  margin: 0 auto;
  text-align: center;
}

.hope-c-PJLV-idaeksS-css,
.hope-c-hOYTCS,
.hope-c-PJLV-igScBhH-css {
  background: var(--neumorph-bg) !important;
  border-radius: 15px !important;
  box-shadow: 
    8px 8px 15px var(--shadow-dark),
    -8px -8px 15px var(--shadow-light) !important;
  border: none !important;
  transition: all 0.3s ease;
  padding: 15px 25px !important;
  margin: 10px auto !important;
  display: inline-block !important;
  cursor: pointer;
}

.markdown-body pre,
.hope-c-ctSAUo {
  background: var(--neumorph-bg) !important;
  box-shadow: 
    inset 3px 3px 5px var(--shadow-dark),
    inset -3px -3px 5px var(--shadow-light) !important;
  border-radius: 10px !important;
  padding: 15px !important;
  margin: 0 auto 20px !important;
  max-width: 90%;
}

.footer {
  text-align: center !important;
  margin-top: 30px;
  opacity: 1 !important;
}

.copyright {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

@media screen and (max-width: 960px) {
  body {
    padding: 10px;
  }
  
  .markdown-body,
  .main-content {
    padding: 15px;
    max-width: 95%;
  }
}
</style>
```

自定义内容
