
# 撤回授权杀死程序

> 来源：[https://docs.feishu.cn/docx/JYKfdjgtMoHZQVxObrSc532yn0g](https://docs.feishu.cn/docx/JYKfdjgtMoHZQVxObrSc532yn0g)

**问：**手机APP任一系统权限从授权改为不授权，都会杀死应用进程，还有这个逻辑吗？我自己测试了几个app，从直观感受来看，好像不都是啊。斗鱼和得物感觉就不是。

**答1：**权限撤回会杀死进程，这是运行时权限的设计使然。如果看到应用好像没关闭，那可能是应用内数据恢复过来，给你一种没有变化的感觉，但并不总是能够这样。

[APP](https://cbe8t1r4a5.feishu.cn/wiki/wikcnm7SSi199OdP5q0UAot3Fsf)
