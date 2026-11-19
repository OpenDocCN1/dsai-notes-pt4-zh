
![](img/aa35a6c5648828a8b4db015a5bf7253b.png)

检查Policy Engine

![](img/bf13a45372cfe734a74a80e31d183391.png)

Policy Overview

+ TE
+ RBAC

![](img/023f3cd27e9027d45af7c308761784e5.png)

被称为Policy，或者是Allow rule

![](img/142968e545b1430842eb73c1d99b42ad.png)

问题：

![](img/ca577ec7dc0e271dd544a550de335bcb.png)

rule太多，容易冲突

# 4 Our works

![](img/9455fe9c822a86f067e61b1a3b9227c6.png)

![](img/22dd39c0869c770402f7df8170f9e111.png)

给一个进程改名字

+ 可以bypass

![](img/b01c631d0c90d0e4655bff685ce7e79e.png)

Ad-hoc：随机

不能保证正确放置，不能保证可以防止bypass

![](img/8b4ef03fae86940064a79679650dad19.png)

![](img/b063b88de3e0b58b4b86171d099304fb.png)

![](img/74a8514bb85746dee2691d55188a5150.png)

![](img/55a925c4f2ac3e8fe66b595a44ba1cd0.png)

检测出了36个bugs

# 5 Attack and Defense

## Introduction

如果Access Control做得好，还是有存在Attack 和 Defense

![](img/804bd90b1c85e91f6959bc55ff25a890.png)

缺陷密度：不准

https://cve.mitre.org/

搜漏洞

### 威胁模型

What can the attacker do？

![](img/a595233e2f13bbcca830cad9bc7e882d.png)

+ Lab：国家实验室
+ Shack：硬件
+ Hack：软件

> 一般Lab防不住

## Run time attack and defense

![](img/65a6a67e615717ebbc83251d7ccc87d0.png)

进攻防守是猫鼠游戏，攻防演化

![](img/53add79dab8524df343b6ee78fe62f66.png)

![](img/dbf0cad112e0aee49ca4935be8a69211.png)

![](img/8d7444029ad0028f7de75ebb6288b809.png)

三种进攻方法

+ 代码注入：直接执行`root_access`
+ 代码重用：改变控制流
+ 非控制数据攻击：修改信号

### 内核注入攻击

![](img/468727294d4515bd8ffaa0a6953b3439.png)

![](img/1b2bd1a9ab1b55033d94634722ba98b4.png)

SMEP Intel 和 PXN ARM防止了`jump-to-user`攻击

![](img/af701edc0ef5f5708c4790cf5f7907af.png)

但是有问题，对内核页表没有保护

![](img/4ba090c6011227db252f9fd1d9b884c7.png)

#### 例子 `ret2usr`

![](img/163578d7544c24e4a00f5e53929b302c.png)

![](img/f6c6ca5af5f6fc6fc450a247c98e62d3.png)

![](img/b7ff391ed1463239eb379388c2182fcc.png)

> SMEP技术主要通过限制内核态代码执行权限，从而防止恶意软件和攻击者利用内核漏洞来攻击操作系统。

> SMEP技术是在Intel CPU的保护模式下实现的，通过在处理器上设置一个特殊的位来实现。当SMEP技术被启用时，处理器会阻止内核态代码执行用户态地址空间中的代码，从而防止内核态代码攻击用户态程序或者用户数据。

> SMEP技术的实现方式是通过在处理器的CR4寄存器中设置一个特定的位来实现。该位控制了处理器的执行保护模式（EPM）功能，当该位被设置时，处理器会将内核态代码执行的地址空间与用户态代码的地址空间进行区分，从而避免内核态代码越权访问用户态代码和数据。

防御方法：SMEP直接把直接映射区域变成不可执行的

### 代码重用攻击

![](img/3a3b410b295c0f1a11c730e711a08a5e.png)

拼接代码片段，这叫做`gadget`

+ 改变返回地址，ROP攻击，Return-oriented programming

![](img/26a32e58d356cba5a4c84cf509772773.png)

+ 改变函数指针，JOP攻击，Jump-oriented programming

![](img/be658d74e215a104c5df13c1fb5f1699.png)

知道`SP`，知道`task_struct`，就可以知道`jid`，`uid`

![](img/405e0a504af27024f09e5e102d0d282f.png)

`Canary`：检查预警

+ `canary`值暴露了，容易被绕过

Shadow Stack的基本思想是在程序执行函数调用和返回时，使用一个额外的栈来保存函数调用和返回地址，以便在后续需要返回时使用。与传统的栈不同，Shadow Stack是由硬件或者软件实现的，而不是由程序自己维护的。

Shadow Stack的实现方式是通过在程序堆栈的基础上增加一个额外的栈，用于保存函数调用和返回地址。每次进行函数调用或者返回时，程序会将当前的函数调用和返回地址保存到Shadow Stack中，并在后续需要返回时从Shadow Stack中获取保存的返回地址，从而保证代码执行的正确性和安全性。

+ 问题有可能我连你这个影子栈也改，所以如何防护保护这个影子栈很重要

![](img/602e9658825131f5b8bebaa630915a6c.png)

跳转：只能跳到要求的地址

CFI技术的基本思想是在程序的控制流中插入特殊的指令或者代码，用于检测程序的跳转和调用是否符合预期。例如，在函数调用时，CFI技术会检查调用的函数是否存在，并检查返回地址是否正确。在程序跳转时，CFI技术会检查跳转的目标地址是否正确，从而防止跳转到非法的地址。

指针保护是一种防护技术，用于保护指向函数的指针，防止被篡改。指针保护的基本思想是在程序运行时检查指针的合法性，确保指针指向的函数地址没有被篡改。指针保护可以通过多种方式实现，例如使用指针加密、指针随机化、指针签名等技术，从而提高指针的安全性。

跳转检查是一种防护技术，用于检查跳转目标地址是否合法。跳转检查的基本思想是在程序运行时检查跳转指令的目标地址是否在一个白名单中，如果不在白名单中，则阻止跳转。跳转检查可以通过多种方式实现，例如使用函数指针表、代码签名、控制流完整性检查等技术，从而提高程序的安全性。

![](img/2d2daaa170f54c7ac833a625e5567c32.png)

### 非控制数据攻击

![](img/bbbb2670c537179c1838a373c5c3e6cb.png)

![](img/1ae60351bf37becc41bad59bd89e5c9c.png)

![](img/95598accac84202efe7d14535fe22555.png)

# 6 Boot protection

> 运行时安全

![](img/ede5bfa909b46422a3562e02c1e1bab9.png)

给一个签名

用bootloader验证

+ 秘钥签名
+ 公钥烧到rom里，公钥验证

内核量级不大

![](img/ec93952f4e94b43bd64e3a19e2ecfe24.png)

![](img/5304653f5fe4566e3528703d04ef3db1.png)

可信根是硬件

![](img/d41395c1eea4682961869fc5e0aadcab.png)

+ Boot loader 去验证
+ 避免BL会升级，分成PBL和SBL，PBL写死不会变
+ 安卓的kernel image 100M
+ System image取决于厂商，会挺大，有很多垃圾app
+ 这么大，如何有效率的验证？
+ Merkle tree用于验证
+ 只需要验证`root`，不需要验证其他的
+ `dm-verify`：google发明的验证方法

## OS protection principles

![](img/c963f02244295c96da9a0729f252b428.png)

Complete mediation：所有的都需要被检查，reference monitor就是遵循这个

![](img/fd8e545bbed4a311fcdfbde38cb78a0e.png)

+ Trusted path：唯一的获得正确的信息的路径

![](img/247ea8dadb6ea72f460da86d1637a9df.png)

+ 分层

![](img/4b6d6b602766cda31f22503512d1db32.png)

## Our work

![](img/deb37b7fcd14af6a921b639955d15d1c.png)

### `TZ-RKP`

![](img/446a80aab86b0aba9a114ca510f50bce.png)

![](img/932548658a4d80054a0e315c1e0cf807.png)

支持`trustzone`，就分成了两块隔离环境

+ Secure world
+ Normal world
+ linux跑在normal world，因为linux有很多问题

![](img/867bafa82211fdeee3251a61fcb6761d.png)

页表还是在normal，但是操作都在secure world上操作，你如果去进行操作需要去secure world上操作，会进行一些检查

+ TrustZone world切换太慢，影响性能
+ 增加了TrustZone的可攻击面
+ RKP放在了TrustZone里

### `SKEE`

![](img/bf5c5e61198a24a96508a0ecab0f907e.png)

内核本身只有一个页表

![](img/1cbb4d83d28be889ba19db33a5253f6b.png)

用两个页表，实现隔离

![](img/a314064967c3a452bb2e5f057c6430a0.png)

### `RegVault`

![](img/17e5d5129f7d6af43fe8ff340e41b27c.png)

![](img/347a4404cdeedbc9448c487522855aea.png)

+ 敏感数据和其他数据混杂存储，无法隔离保护
+ 物理分离
+ 逻辑分离：加密
+ 硬件加密

![](img/2b96a670b0fc90fae931f9cc37f41b5e.png)

仅在寄存器里面是解密的

# 7 虚拟化

https://coolshell.cn/articles/22320.html
