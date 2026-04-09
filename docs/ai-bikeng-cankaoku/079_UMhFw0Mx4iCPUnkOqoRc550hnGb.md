# VPS通过1Panel开源面板搭建OpenList

> 来源：[https://xodnytdcaw.feishu.cn/docx/DJDcdmszloRaVOxnD4RcrTWBnyf](https://xodnytdcaw.feishu.cn/docx/DJDcdmszloRaVOxnD4RcrTWBnyf)

## 准备工作

一台VPS

域名

## 搭建步骤

+   ssh连接服务

+   安装1panel面板

以root用户身份运行一键安装脚本，自动完成1Panel的下载和安装。

参考：ht[tps:](https://www.bilibili.com/video/BV17iUrYgEVG/?spm_id_from=333.337.search-card.all.click&vd_source=206031f494850e57fd6c92ace02b1bed)//1panel.cn/  视频教程

```
bash -c &quot;$(curl -sSL https://resource.fit2cloud.com/1panel/package/v2/quick_start.sh)&quot;
```

## 参考教程

使用1Panel开源面板搭建OpenList并挂载网盘