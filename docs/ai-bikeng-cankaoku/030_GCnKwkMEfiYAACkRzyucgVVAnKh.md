# Hosts文件作用与修改

> 来源：[https://xodnytdcaw.feishu.cn/docx/QR1vdnwhqoDYUWx7xv0cdsBdnog](https://xodnytdcaw.feishu.cn/docx/QR1vdnwhqoDYUWx7xv0cdsBdnog)

## 定义

**Hosts文件是一个简单**的文本文件，用于将域名映射到IP地址。它在操作系统中扮演着本地DNS（域名系统）解析器的角色，当用户访问网站时，系统会首先查找hosts文件以获取IP地址，优先级高于DNS服务器。这个文件主要用于加速域名解析，也可用于拦截广告和恶意网站。

![](img/a876a7cea213076d1b18528085f51c99.png)

## 作用

### 加速访问

```
202.108.22.5 www.baidu.com
```

如上所示，我们在本地的 hosts 文件中，将百度的IP 地址与百度的域名建立了映射关系，也就起到了"加快域名解析"的作用，因为不需要再去请求DNS服务器啦！如果想对其进行注释的话，直接在前面加#符号就可以，例如:

```
#202.108.22.5 www.baidu.com
```

再比如GitHub加速访问

```
140.82.121.4 github.com
```

### 屏蔽网站

错误的IP+域名

```
0.0.0.0 
```

比如这个意思就是屏蔽idm更新

再比如这个屏蔽模拟器的开屏广告

```
114.115.116.117 mumu.nie.netease.com
```

屏蔽向日葵更新

```
127.0.0.1 client-api.oray.com
```

hosts增加

```
127.0.0.1 account.listary.com
```

## 修改工具

[点此下载](https://lz.qaiu.top/parser?url=https://wwzq.lanzouq.com/ijEfZ3aqkhuh)