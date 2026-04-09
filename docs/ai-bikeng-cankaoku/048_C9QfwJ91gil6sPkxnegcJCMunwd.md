# mdnice自定义主题

> 来源：[https://xodnytdcaw.feishu.cn/docx/Q6k4dr5LPoRBakxqfd4cIxU1nee](https://xodnytdcaw.feishu.cn/docx/Q6k4dr5LPoRBakxqfd4cIxU1nee)

## 预览

![](img/e903b3980bb31d8becaee371a7896d52.png)

## 配置

### 全局属性

![](img/490e13e23b431ad3401b30dbf27d4d62.png)

![](img/8ce2e4394a4f371662c72d533244a5c7.png)

### 自定义样式css代码

> 主题我自己的使用

> 样式---自定义css样式（添加如下代码）

紫色

```
/*自定义样式，实时生效*/

/* --- 全局属性 --- */
#nice {
    font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &quot;PingFang SC&quot;, Cambria, Cochin, Georgia, Times, &quot;Times New Roman&quot;, serif;
    padding: 10px;
    font-size: 16px;
    color: #353535;
    word-spacing: 0.5px;
    letter-spacing: 0.5px;
    border-radius: 16px;
}

/* --- 段落 --- */
#nice p {
    line-height: 1.75;
    margin: 0.8em 0;
    font-size: 16px;
    color: #353535;
}

/* --- 一级标题 (蓝色保留，如需改为橙色请告诉我) --- */
#nice h1 {
    border-top: 2px solid #2b4fed;
    text-align: center;
    font-size: 1.3em;
    margin-top: 40px;
}

#nice h1 .content {
    display: inline-block;
    font-weight: normal;
    background: #2b4fed;
    color: #ffffff;
    padding: 3px 15px;
    border-radius: 0 0 13px 13px;
}

/* --- 二级标题 (无线条/无标记/Claude色) --- */
#nice h2 {
    margin-top: 30px;
    margin-bottom: 10px;
}

#nice h2 .content {
    font-size: 20px;
    font-weight: 700;
    color: #D97757; 
    display: inline-block;
    padding-left: 0;
    border-left: none;
}

/* --- 三级标题 (文字#D97757，背景接近白色) --- */
#nice h3 {
    margin-top: 25px;
}

#nice h3 .content {
    background: rgba(217, 119, 87, 0.08); 
    color: #D97757;                      
    padding: 5px 12px;
    border-radius: 8px;
    font-size: 16px;
    font-weight: bold;
    display: inline-block;
}

/* --- 引用 (左侧粗边线 + 暖色背景) --- */
#nice blockquote {
    font-style: normal;
    border-left: 6px solid #D97757 !important; 
    padding: 20px 20px;
    line-height: 1.75;
    border-radius: 13px; 
    color: #444;
    background: #fdfaf7; 
    margin: 1.5em 0;
}

#nice blockquote p {
    margin: 0;
    color: #353535;
    font-weight: 500;
}

/* --- 链接 (网址颜色) --- */
#nice a {
    color: #D97757; 
    font-weight: 500;
    text-decoration: none;
    border-bottom: 1px solid #D97757;
}

/* --- 代码块 (修复显示问题) --- */
#nice pre {
    background: #282c34 !important;
    padding: 15px !important;
    border-radius: 13px;
    margin: 15px 0;
    overflow-x: auto;
    display: block;
}

#nice pre code {
    color: #abb2bf !important;
    font-family: Menlo, Monaco, Consolas, monospace;
    font-size: 14px;
    line-height: 1.6;
    background: transparent !important;
    display: block;
    white-space: pre;
}

/* --- 行内代码 --- */
#nice :not(pre) &gt; code {
    color: #D97757;
    background: rgba(217, 119, 87, 0.08);
    padding: 2px 5px;
    border-radius: 4px;
    font-size: 0.9em;
}

/* --- 列表 --- */
#nice ul, #nice ol {
    color: #D97757; 
    font-size: 16px;
}

#nice li section {
    color: #353535;
}

/* --- 加粗 --- */
#nice strong {
    font-weight: 700;
    color: #000000;
}

/* --- 斜体 (使用#D97757) --- */
#nice em {
    color: #D97757;
    font-style: italic;
}

/* --- 分隔线 --- */
#nice hr {
    height: 1px;
    border: none;
    margin: 40px 0;
    background-image: linear-gradient(to right, rgba(217, 119, 87, 0), rgba(217, 119, 87, 0.5), rgba(217, 119, 87, 0));
}
```

```
/* 自定义样式，实时生效 */

/* --- 全局属性 --- */
#nice {
    font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &quot;PingFang SC&quot;, Cambria, Cochin, Georgia, Times, &quot;Times New Roman&quot;, serif;
    padding: 10px;
    font-size: 16px;
    color: #353535;
    word-spacing: 0.5px;
    letter-spacing: 0.5px;
    border-radius: 16px;
}

/* --- 段落 --- */
#nice p {
    line-height: 1.75;
    margin: 0.8em 0;
    font-size: 16px;
    color: #353535;
}

/* --- 一级标题 --- */
#nice h1 {
    border-top: 2px solid #2b4fed;
    text-align: center;
    font-size: 1.3em;
    margin-top: 40px;
}

#nice h1 .content {
    display: inline-block;
    font-weight: normal;
    background: #2b4fed;
    color: #ffffff;
    padding: 3px 15px;
    border-radius: 0 0 13px 13px;
}

/* --- 二级标题 --- */
#nice h2 {
    margin-top: 30px;
    margin-bottom: 10px;
}

#nice h2 .content {
    font-size: 20px;
    font-weight: 700;
    color: #D97757; 
    display: inline-block;
    padding-left: 0;
    border-left: none;
}

/* --- 三级标题 --- */
#nice h3 {
    margin-top: 25px;
}

#nice h3 .content {
    background: rgba(217, 119, 87, 0.08); 
    color: #D97757;                      
    padding: 5px 12px;
    border-radius: 8px;
    font-size: 16px;
    font-weight: bold;
    display: inline-block;
}

/* --- 引用 --- */
#nice blockquote {
    font-style: normal;
    border-left: 6px solid #D97757 !important; 
    padding: 20px 20px;
    line-height: 1.75;
    border-radius: 13px; 
    color: #444;
    background: #fdfaf7; 
    margin: 1.5em 0;
}

#nice blockquote p {
    margin: 0;
    color: #353535;
    font-weight: 500;
}

/* --- 链接 --- */
#nice a {
    color: #D97757; 
    font-weight: 500;
    text-decoration: none;
    border-bottom: 1px solid #D97757;
}

/* --- 代码块 (长段代码) --- */
#nice pre {
    background: #282c34 !important;
    padding: 15px !important;
    border-radius: 13px;
    margin: 15px 0;
    overflow-x: auto;
    display: block;
}

#nice pre code {
    color: #abb2bf !important;
    font-family: Menlo, Monaco, Consolas, monospace;
    font-size: 14px;
    line-height: 1.6;
    background: transparent !important;
    display: block;
    white-space: pre;
}

/* --- 行内代码 (已优化为极简灰) --- */
#nice :not(pre) &gt; code {
    color: #444444;             /* 深灰文字，保持清晰度 */
    background: #f1f1f1;        /* 浅灰色背景 */
    padding: 2px 6px;           /* 适当间距 */
    border-radius: 4px;
    font-size: 0.9em;
    border: 1px solid #e1e1e1;  /* 微弱边框，增加质感 */
    font-family: Menlo, Monaco, Consolas, monospace;
}

/* --- 列表 --- */
#nice ul, #nice ol {
    color: #D97757; 
    font-size: 16px;
}

#nice li section {
    color: #353535;
}

/* --- 加粗 --- */
#nice strong {
    font-weight: bold;
    color: #000000;
    background: transparent !important;
}

/* --- 斜体 --- */
#nice em {
    color: #D97757;
    font-style: italic;
}

/* --- 分隔线 --- */
#nice hr {
    height: 1px;
    border: none;
    margin: 40px 0;
    background-image: linear-gradient(to right, rgba(217, 119, 87, 0), rgba(217, 119, 87, 0.5), rgba(217, 119, 87, 0));
}
```

蓝色

```
/* 自定义样式 - Neon Light 风格 (白底版) */

/* --- 全局属性 --- */
#nice {
    font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &quot;PingFang SC&quot;, sans-serif;
    padding: 10px;
    font-size: 16px;
    color: #353535; /* 深灰文字，白底阅读最舒适 */
    word-spacing: 0.5px;
    letter-spacing: 0.5px;
    border-radius: 16px;
    background-color: #ffffff; /* 背景强制白色 */
}

/* --- 段落 --- */
#nice p {
    line-height: 1.75;
    margin: 0.8em 0;
    color: #353535;
}

/* --- 一级标题 (霓虹电光青) --- */
#nice h1 {
    border-top: 2px solid #00f3ff;
    text-align: center;
    font-size: 1.3em;
    margin-top: 40px;
}

#nice h1 .content {
    display: inline-block;
    font-weight: normal;
    background: #00f3ff;
    color: #000000;
    padding: 3px 15px;
    border-radius: 0 0 13px 13px;
    box-shadow: 0 2px 10px rgba(0, 243, 255, 0.3);
}

/* --- 二级标题 (替换原 #D97757 为电光青) --- */
#nice h2 {
    margin-top: 30px;
    margin-bottom: 10px;
}

#nice h2 .content {
    font-size: 20px;
    font-weight: 700;
    color: #00bcd4; /* 稍深一点的青色，确保白底可读性 */
    display: inline-block;
    padding-left: 0;
    border-left: none;
}

/* --- 三级标题 (电光青背景) --- */
#nice h3 {
    margin-top: 25px;
}

#nice h3 .content {
    background: rgba(0, 243, 255, 0.1); 
    color: #00bcd4;                      
    padding: 5px 12px;
    border-radius: 8px;
    font-size: 16px;
    font-weight: bold;
    display: inline-block;
}

/* --- 引用 (全息紫边框) --- */
#nice blockquote {
    font-style: normal;
    border-left: 6px solid #d500f9 !important; 
    padding: 20px 20px;
    line-height: 1.75;
    border-radius: 13px; 
    color: #555;
    background: #fbfaff; 
    margin: 1.5em 0;
}

/* --- 代码块 (保持原始稳定逻辑) --- */
#nice pre {
    background: #282c34 !important;
    padding: 15px !important;
    border-radius: 13px;
    margin: 15px 0;
    overflow-x: auto;
    display: block;
}

#nice pre code {
    color: #abb2bf !important;
    font-family: Menlo, Monaco, Consolas, monospace;
    font-size: 14px;
    line-height: 1.6;
    background: transparent !important;
    display: block;
    white-space: pre;
}

/* --- 行内代码 (黑色文字 + 绿色背景) --- */
#nice :not(pre) &gt; code {
    color: #000000 !important;
    background: #b7f605 !important;
    padding: 2px 5px;
    border-radius: 4px;
    font-size: 0.9em;
}

/* --- 列表 --- */
#nice ul, #nice ol {
    color: #00bcd4; 
}

#nice li section {
    color: #353535;
}

/* --- 加粗 (正常加粗，无背景色，颜色使用电光青) --- */
#nice strong {
    font-weight: 700;
    color: #00bcd4;
    background: transparent !important;
}

/* --- 斜体 (全息紫) --- */
#nice em {
    color: #d500f9;
    font-style: italic;
}

/* --- 分隔线 (霓虹渐变) --- */
#nice hr {
    height: 1px;
    border: none;
    margin: 40px 0;
    background-image: linear-gradient(to right, rgba(0, 243, 255, 0), rgba(0, 188, 212, 0.5), rgba(0, 243, 255, 0));
}
```

## 说明

```
二级标题为#D97757
三级标题#D97757 背景#D97757（降低透明度）
行内代码背景使用#b7f605
代码框为Mac风格窗，代码颜色白色
斜体文字颜色为#b7f605
```

## 测试以markdown内容

```
&gt; 蓝色加粗字体为不常用还没有记住的

## 一.安装步骤

首先需要下载链接才可以



**下载地址**

https://editor.mdnice.com/




### 代码

```
irm https://get.activated.win | iex
```


1. 网页加速
2. 下载加速

## 二.待办/序号

### 待办

- 公众号发布文章

- 跟进预约今天安装软件的李先生与王总

- 千帆上架新品/撰写genmini使用教程

### 序号
1. ~~打造冲击~~
2. 江伟达


## 三.行内代码
依次点击`设置`→`通用`→`关于本机`，然后在查看在序列号发给我
### 表格

| 名称         | 说明           |
| ------------ | -------------- |
| **电视喵**   | *电视应用导航* |
| **数字工坊** | *我的小铺*     |


## 四.加粗/斜体


当然，~~一些简单的任务~~~基本用不到AI，反而会拖累你的效率，日常可以根据自己的实际情况，是否使用AI辅助。



*以上，既然看到这里了，如果觉得不错，随手点个赞、在看、转发三连吧，如果想第一时间收到推送，也可以给我个星标⭐～*

谢谢你看我的文章，我们，下次再见。
```