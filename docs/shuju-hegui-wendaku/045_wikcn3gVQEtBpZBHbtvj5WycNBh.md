
# MAC、IMEI地址收集必要性

> 来源：[https://docs.feishu.cn/docx/WHvZdHl8xoNe1ExPOFrcF12inrd](https://docs.feishu.cn/docx/WHvZdHl8xoNe1ExPOFrcF12inrd)

## 问题

想问下懂技术的法务一个问题，业务要收集用户`MAC`、`IMEI`地址，用于记录用户阅读记录、打赏记录，我理解现在`MAC`地址都收集不到了吧？或者这个理由收集`MAC`、`IMEI`，必要吗？

## 回答

### 答1

`MAC`、`IMEI`地址属于不可变更的设备唯一标识，感觉一般`APP`收集这类信息可能难以说明合理性，除非是用于保障网络安全和运营安全

### 答2

这个理由要获取`MAC`或者`IMEI`，我觉得必要性不足。记录用户的阅读记录、打赏记录可以通过用户`id`等方式即可。

`MAC`地址不用权限弹窗就能获取，不过现在大多数手机厂商都支持随机`MAC`地址，实际上很难做到唯一标识到个人。

`IMEI`的获取需要权限弹窗，如果用户不给，就获取不到。

### 答3

所以总结一下，出于打赏记录的风控要求，可以收集`AMC`、`IMEI`，鉴于国内监管已经不允许收集`MAC`地址且因为手机厂商可以支持随机`MAC`地址，所以目前收集`MAC`地址不需要弹窗权限但是`IMEI`需要；阅读记录，则都不能。

### 答4

落到开发过程中就是在该功能设计时提出需要增加风控需求。在打赏功能前，用户就已经被标识出高中低风险了

### 答5

有`android.permission.INTERNET`和`android.permission.ACCESS_WIFI_STATE`就可以拿`MAC`地址，这两个权限都没弹框。在`Android10`之后默认随机`MAC`，低版本还是能拿设备唯一`MAC`。如果`OEM`厂商在高版本没默认开这个随机特性，也能拿唯一`MAC`。

`IMEI`的话，需要`OEM`厂商的自定义权限。这种自定义权限只有`OEM`厂商自己用，三方申请不到，这种权限一般也不弹框。

[APP](https://cbe8t1r4a5.feishu.cn/wiki/wikcnm7SSi199OdP5q0UAot3Fsf)
