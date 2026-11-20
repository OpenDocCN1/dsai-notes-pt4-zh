
# 2026-03-05免费利器Jina Reader：一键读取任意网页、图片、PDF，AI时代的内容净化器，还能绕过各种墙

> 来源：https://xuqiwei1986.feishu.cn/docx/IpuDd4mCIonIgExm1Pdc8dvtnMb
>
> 🔗 原文链接：https://mp.weixin.qq.com/s?__biz=MzI1Mzg2MjAxNQ==&mid=2247490020&idx=1&sn=7e401240104559d251080cff09712c30&chksm=e8834e6ea1cd8e26c5ad6a090bbcf9e3ad5e5a01b285cf2e59059f38930ed631ad0069297723#rd

原创 jackao jackao 恶人笔记2026年3月5日 08:25 北京

大家好，我是你们的AI开发助手夹克。

最近使用OpenClaw获取信息时，发现了一个不错的工具，X推特上 @RookieRicardoR（耳朵）分享的一个神器：在任意网页链接前加上 `https://r.jina.ai/` ，就能瞬间提取出干净的正文内容，连推特的登录墙都能轻松绕过，而且它还可以读图片和 PDF，直接输出识别后的干净文本。默认速率限制是 20 次/每分钟。如果不够用的话，可以访问 `https://jina.ai/reader/` ，它还会送你一个 1000万 token 的 key，完全免费。

测试了一下，一个图片URL直接扔进去，可以直接输出Markdown描述：“Looking inside of a large, gothic like room with a large, centred, metal table”（大意：走进一个巨大的哥特式房间，中央有一张大金属桌）。另一张则是那张黑白哥特风格大厅的原图，完美印证了工具的图像理解能力。

这玩意儿 太香了 ，尤其是给OpenClaw之类的AI agent用，高效还省token。今天这篇文章就分享一下。

## 一、Jina Reader到底是什么？

Jina AI（一家专注搜索基础模型的公司）推出的 Reader API ，核心功能就是把任何URL（网页、PDF、图片）转换成 LLM友好 的干净文本（主要是Markdown，也支持JSON）。

它不像普通爬虫那样抓一堆乱七八糟的HTML，而是：

+ 用浏览器内核真实渲染页面
+ 自动去掉广告、导航、页脚、脚本
+ 提取核心内容
+ 图片自动用视觉语言模型（VLM）生成描述
+ PDF直接OCR+结构化提取（支持带大量图片的复杂PDF）

一句话总结：给你的AI喂最干净、最省token的“净菜”。

## 二、零门槛使用方法（3分钟上手）

### 1. 最简单的前缀法（无需任何账号）

浏览器直接输入：

```
https://r.jina.ai/ + 你的任意链接
```

回车就会输出干净Markdown了！

+ 测试网页：试试任意推文链接、知乎文章、付费墙文章
+ 测试图片：直接丢图片URL（支持jpg、png等）
+ 测试PDF：直接丢PDF链接（NASA那份经典PDF测试案例超级稳）

### 2. 进阶：带API Key（推荐给需要高并发的）

1. 打开 `https://jina.ai/reader/`
2. 一键生成API Key（无需信用卡，新用户直接送 1000万免费token，非商业使用完全够）
3. 在请求头加：

```
Authorization: Bearer 你的key
```

这样速率从 20次/分钟 直接提到 500次/分钟，如果还不够的话，可以付费获取更高，而且按key计费更稳定。

### 3. 命令行/Curl示例（开发者最爱）

```
# 普通读取
curl "https://r.jina.ai/https://www.example.com";

# 开启图像自动描述（强烈推荐！）
curl -H "x-with-generated-alt: true" "https://r.jina.ai/你的图片或网页URL";

# 输出JSON结构化数据
curl -H "Accept: application/json" "https://r.jina.ai/你的URL";

# 读取PDF
curl "https://r.jina.ai/https://www.nasa.gov/xxx.pdf";
```

### 4. 同时还是一个搜索神器：s.jina.ai

可以直接 `https://s.jina.ai/`你的搜索词，会返回前5个结果 + 每个结果的干净正文，从而一键实现“带RAG的搜索”。

## 三、分析：为什么这么强？

### 1. 绕过一切反爬

推特登录墙、知乎盐选、Medium付费、Substack……基本全灭。因为它走的是真实浏览器渲染，不是模拟请求。

### 2. 图像理解能力拉满

默认不开图像描述是为了省延迟，但加个header就自动用Jina自家2.4B VLM给每张图写`alt`文本。你的文本-only LLM瞬间拥有“看图说话”能力。

### 3. PDF处理无敌

复杂排版、多图PDF、扫描件都能快速转纯文本，适合做ChatPDF、论文速读、合同分析。

### 4. 成本与性能

+ 免费10M token ≈ 几千篇长文
+ 输出极度干净，喂给OpenClaw/ClaudeCode/Cursor时token消耗能省30%以上
+ 多语言支持（29种语言），中文效果极佳

### 5. 局限性（客观说）

+ 极少数动态SPA页面可能需要加等待选择器
+ 图像描述默认关闭（手动开即可）
+ 免费token适合低并发非商业使用，生产环境建议买1B token套餐（$50）

## 四、使用建议与进阶玩法

### 新手建议

+ 先用前缀法玩两天，感受“白嫖快乐”，领10M token，存起来提速用
+ 结合Cursor或Claude Projects，把读到的内容直接扔进去总结/翻译/分析

### 进阶玩家玩法

+ 做个人RAG：每天自动抓取关注的Substack/Newsletter，转成向量存库
+ 构建AI Agent：让Agent用Reader去实时查最新资讯，再决策
+ 批量处理PDF：写个Python脚本，循环处理一堆论文
+ 图像分析链路：图片URL → Reader描述 → 大模型深度理解

### 小技巧

+ 想指定只提取某部分内容？加 `x-target-selector` header
+ 动态页面加载慢？加 `x-wait-for-selector` 或 `x-timeout`
+ 想流式输出？`Accept: text/event-stream`

## 结语：AI时代的“内容净化器”

如今信息过载已经不是问题，“优质输入”才是稀缺能力。Jina Reader 也许就是那个把互联网杂乱内容变成“LLM最爱净菜”的神器。它免费、简单、强大，还在持续迭代（ReaderLM-v2、Jina VLM都在背后硬核支撑）。

强烈建议大家去试试，用完欢迎回来评论区分享你的神级用法。点赞+转发，下次我们继续分享更多免费AI基础设施神器。AI时代，工具人也要卷起来！

（本文基于公开推文与官方文档整理，所有链接均可直接点击体验。如有更新以官网为准）
