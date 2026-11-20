
# 网站搭建/GitHub/Cloudflare

> 来源：[https://xodnytdcaw.feishu.cn/docx/T11KdmWZgoaxz5xqNuPc1pRMnUf](https://xodnytdcaw.feishu.cn/docx/T11KdmWZgoaxz5xqNuPc1pRMnUf)

## 一.GitHubPages功能

需要创建同名的仓库，仓库名称必须是：`你用户名.github.io`，然后`pages`添加自定义域，域名`DNS`添加记录`CNAME`记录，`@`与`www`，IP地址可`ping 仓库地址`获取，也可以直接填写

体验地址：`dqtx.cc`

![](img/a49dbdd00526230d11a7150f00843809.png)

## 二.Cloudflare部署

使用Cloudflare的`Workers/Pages`功能，可以将你GitHub的仓库部署自动上线，

⚠️：多个域名同时托管在Cloudflare，添加域

账户主页，会看到你托管的域名，`DNS`，添加新的记录

`SSL/TLS`-`https`访问 ，可以设定缓存规则（`Cache Rules`），可以加速访问

### 案例1：Cloudflare Pages-导航站

演示：`dh.dqtx.cc`

[部署导航网站案例](https://mp.weixin.qq.com/s/90LUmKilfLZfc5L63Ej3Sg?click_id=21)

![](img/11f47b511a286f3fd960981c05060914.png)

![](img/9bb24c51fe43b635d424696d2ab24464.png)

### 案例2：Cloudflare Worker-notion自定域

演示：`notion.dqtx.cc`

绑定Notion域名到自定义域名 部署失败

![](img/abeaacef5b7a7cbc059f3e9ac7b02b22.png)

![](img/50f43fc40b071654797c3fef2278ce7b.png)

## 三.Vercel部署

### 案例1：影视网站

体验：`tv.dqtx.cc` [项目地址](https://github.com/dqtx760/MoonTV)

![](img/55031997798eafd266719fe30a9fca71.png)

[这个在电视上也](https://github.com/orion-lib/OrionTV)可以导入，电视上安装`OrionTV`播放器

![](img/3055547005d227f4b8144ff365087cf9.png)

### 案例2：大强远程技术支持

体验：`742112.xyz`

![](img/0b126e7b14d056c8dc5f159372adae55.png)

### 案例3：大强附件表

体验：`88.dqtx.cc`

![](img/372086464fff19fe50796213274149ef.png)

## 四.域名/云应用

### 域名

`https://www.spaceship.com/`

### 云应用部署`openlist`

[雨云地址](https://www.rainyun.com/NzYxMDg2_)[优惠](http://110.42.45.237:48317/)码 `NzYxMDg2`   体验地址：点此访问

![](img/f6152667cc164ba9fa09c1e4f7bfb331.png)

## 五.写在最后

### 提升Vercel 站点在国内的访问效率

使用Vercel时，可能会遇到在国内访问速度一般且不稳定的情况，尤其是对于部分地区的用户来说，访问可能会受到一定的限制。为了提升Vercel站点在国内的访问效率，我们可以采取一些措施。

一种方法是将网站进行备案，然后购买国内服务商的`CDN`服务，如腾讯云的`CDN`或`EdgeOne`。这样可以通过国内的节点来加速访问，提高用户的体验。

另一种较为简便的方法是利用一些国人开放的`CDN`项目，例如：

提升部署在`cloudflare`、`vercel`或`netlify`的网站在中国国内的访问速度和稳定性

`https://github.com/xingpingcn/enhanced-FaaS-in-China`

一个数字域名年成本几块钱，无需购买服务器，借助GitHub一些项目，通过Cloudflare/Vercel部署上线，绑定自己的域名即可访问，一开始路不好走，走多了自然就好走了！
