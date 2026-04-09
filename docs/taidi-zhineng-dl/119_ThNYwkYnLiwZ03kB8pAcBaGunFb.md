# AI 芯片软件栈

> 来源：[https://learning.feishu.cn/docx/LskHdTrTtoyRfKx0bCycqVKVnAd](https://learning.feishu.cn/docx/LskHdTrTtoyRfKx0bCycqVKVnAd)

# 关注要点

一个开源项目发展的初期看是否有一个合理的初期技术架构。而当发展到了四年五年的时候，我们开始发现需要看到未来发展的道路。

技术要点上：

1.  分 compile，runtime 2 条线。

1.  Runtime 看：调度、内存管理、command queue，设备管理等。

成熟度的判定技巧：

1.  凡是画了 KMD/UMD 的，一般都是软件做的很差的，甚至啥玩意都没有。比如 璧仞、燧原、2020 年的希姆。

# 芯片厂商

## 华为 - MindSpore

来自 https://www.slideshare.net/Huawei/introduction-to-huaweis-fullstack-ai-portfolio

![](img/27d9ead2529d032d923d9516c2845857.png)

![](img/36449f37b16795f35d87262a2c946117.png)

![](img/0f243b2988914d3a3019e7804a10b404.png)

来自 https://www.mindspore.cn/tutorial/zh-CN/r1.2/introduction.html

![](img/78aacf0e86ad334786a90c87c6b7b1c3.png)

![](img/d90d733d21fdf2f036a19491efc2ac55.png)

## 寒武纪 - Cambricon PyTorch

魔改 Pytorch，用 mlir 做了个图编译器。

![](img/994aa293fef5fbdb326531cddac65e37.png)

![](img/3e0e9fb409440be46ee0aad1292bac4e.png)

![](img/c9f8e253cdf6cb9933f3bfb48f545a6c.png)

## **璧仞**

![](img/0a631d1c7f7d26c3dc6ab78c86944a4d.png)

## **燧原**

早期用 tensorflow 和 xla。尝试 mlir 不太成功。

![](img/2235eaa2c5247f19a90a78331aa67399.png)

##  摩尔线程

软件栈架构, MUSA（Metacomputing Unified System Architecture)

https://www.mthreads.com/product/S3000

![](img/6d6a29dc0fb9bc489a6dd595d7981f5e.png)

GPU 虚拟化，跟英伟达的 docker 版本，图基本完全一样。

![](img/04c622c7877236bdce00af62d6cdc30a.png)

MTVERSE 元宇宙平台

![](img/3ad96c621c2f57f3c1f36f52e15e1c05.png)

![](img/2c213a3c1cd1c75359196973a64536a9.png)

## 沐曦 - TODO

MXMACA 软件栈

## 天数智芯

https://www.iluvatar.com/software?fullCode=cpjs-rj-rjz

![](img/d5a9db35b5b0fa135255a9ea966e0081.png)

## **登临**

片内是异构的，即，GPGPU 里面烧了些 DSA (TensorCore)。-- 这不算异构，英伟达早加了 TensorCore。

![](img/8460e1eee73b40e37935d6fa3b7e115a.png)

![](img/0696b34e91ce695e4845983741b8295d.png)

## 瀚博

https://www.vastaitech.com/software

![](img/7a6d5b2d24247f3eac8d60f969be1131.png)

## **昆仑芯**

https://dl.acm.org/doi/pdf/10.1145/3526241.3530316

GLSVLSI-2022：KunlunTVM: A Compilation Framework for Kunlun Chip Supporting Both Training and Inference

2023.12.29 from https://www.kunlunxin.com/technology

![](img/8ff99708ab72df773bfe6312a6e6c16a.png)

![](img/b039c53d56e28cd6d7bb3f7da80fda0e.png)

![](img/67273df14d3fedf83381276af7222221.png)

## **平头哥**

![](img/8844d1ca6958b6a4eecac8a68146f87f.png)

## 希姆 - TensorTurbo

[希姆软件栈](https://dwpl6xgouw.feishu.cn/wiki/XnspwbVnCi43QrkY92ZczmTGn8c)

![](img/a32fdf13746cb150bec8d3825766b1a1.png)

![](img/cdf2fd2026101ff33fc7627277828b25.png)

## **Graphcore**

![](img/6d89598835b98ab8402626c1b8a227b3.png)

# 软件厂商

## 百度 - PaddlePaddle

出自: https://www.modb.pro/doc/48500

![](img/d2c84642a998d6199fccf3d1e09dc677.png)

![](img/4a04be121127565d1d6c5415c5f40d39.png)

Paddle 赚钱

![](img/6729d7ed04e5215314f98de85af36d7e.png)

## 商汤 - PPQ 量化工具

https://github.com/openppl-public/ppq

![](img/0b484315c18a86c7f02cd30a153307da.png)

## Oneflow

![](img/323d86546aefd34d8cf2d34c76cfd98c.png)