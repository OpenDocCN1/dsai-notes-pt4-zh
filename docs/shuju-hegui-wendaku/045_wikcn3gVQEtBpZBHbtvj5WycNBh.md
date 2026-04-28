# MAC、IMEI地址收集必要性

> 来源：[https://docs.feishu.cn/docx/WHvZdHl8xoNe1ExPOFrcF12inrd](https://docs.feishu.cn/docx/WHvZdHl8xoNe1ExPOFrcF12inrd)

**-问**：想问下懂技术的法务一个问题，业务要收集**用户MAC、IME**I地址， 用于记录用户阅读记录、打赏记录，我理解现在MAC地址都收集不到了吧？或者这个理由收集MAC、IMEI，必要吗？

**-答1：**MAC、IMEI地址属于**不可变更**的**设备唯一标识**，感觉一般APP收集这类信息可能难以说明合理性，除非是用于保障网络安全和运营安全

**-答2：****这个理由要****获取MAC或者IM**EI，我觉得必要性不足。记录用户的阅读记录、打赏记录可以通过用户id等方式即可。

**MAC地址不用**权限弹窗**就能获取，不过现在大多**数手机厂商都支持随机MAC地址，实际上很难做到唯一标识到个人。

IMEI的获取需要**权限弹窗**，如果**用户不给，就获取不到**。

**-答3：**所以总结一下，出于打赏记录的风控要求，可**以收集AMC，IME**I，鉴于国内监管已经不允许收集MAC地址且因为手机厂商可以支持随机MAC地址，所以目前收集MAC地址不需要弹窗权限但是IMEI需要；阅读记录，则都不能。

**-答4：****落到开发**过程中就是在该功能设计时提出需要增加风控需求。在打赏功能前，用户就已经被标识出高中低风险了

**-答5：**有**andro****id.pe**rmission.I**NTERN**ET和android.p**ermission.**ACCESS_WIFI_STATE就可以拿MAC地址，这两个权限都没弹框。在Android10之后默认随机MAC，低版本还是能拿设备唯一MAC。如果OEM厂商在高版本没默认开这个随机特性，也能拿唯一MAC。

IMEI的话，需要OEM厂商的自定义权**限。这种自定**义权限只有OEM厂商自己用，三方申请不到，这种权限一般也不弹框。

[APP](https://cbe8t1r4a5.feishu.cn/wiki/wikcnm7SSi199OdP5q0UAot3Fsf)