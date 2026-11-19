
# 🛰️:你的应用版本过低，请升级至最新版本后再登录

> 来源：[https://yunyingmenghai.feishu.cn/docx/QOLrdlZU5oWnvPx11apcBErGnuh](https://yunyingmenghai.feishu.cn/docx/QOLrdlZU5oWnvPx11apcBErGnuh)

🛰️应该是通过检测是否存在用户目录，来决定蹦不蹦这个弹窗：

![](img/b1a4b6b2a04b3ba483f88644caf22bab.png)

版本过低

已知情况：

+ 一直在登录的账号，不弹
+ 曾经登录过的账号，再次登录，不弹
+ 新环境，弹
+ 旧环境，新账号，弹。

所以，应对的方法是比较简单的：创建用户目录。

具体做法是：

1. 先按提示，升级到最新版本
1. 登录🛰️
1. 正常使用一下（发发消息、发发图片……）

然后在 `WeChat Files` 目录下，应该可以看到账号对应 `wxid` 的目录。

这时候，再卸载掉🛰️但保留数据（可以把 `wxid` 目录复制一份出来），然后重新安装 3.9.2.23，应该就可以用了。

## 用代码修复:

🛰️是指 3.9.2.23 版本的🛰️

```
pip install pymem
```

```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

from pymem import Pymem

ADDRS = [0x2FFEAF8, 0x3020E1C, 0x3021AEC, 0x303C4D8, 0x303FEF4, 0x3040FA4, 0x30416EC]


def fix_version(pm: Pymem):
    WeChatWindll_base = 0
    for m in list(pm.list_modules()):
        path = m.filename
        if path.endswith("WeChatWin.dll"):
            WeChatWindll_base = m.lpBaseOfDll
            break

    for offset in ADDRS:
        addr = WeChatWindll_base + offset
        v = pm.read_uint(addr)
        if v == 0x63090A13:  # 已经修复过了
            continue
        elif v != 0x63090217:  # 不是 3.9.2.23 修复也没用
            raise Exception("别修了，版本不对，修了也没啥用。")

        pm.write_uint(addr, 0x63090A13)

    print("好了，可以扫码登录了")


if __name__ == "__main__":
    try:
        pm = Pymem("WeChat.exe")
        fix_version(pm)
    except Exception as e:
        print(f"{e}，请确认微信程序已经打开！")
```
