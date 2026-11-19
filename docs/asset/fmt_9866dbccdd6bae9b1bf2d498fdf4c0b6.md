
# GDB

> 来源：[https://docs.feishu.cn/docx/CsO6dB79co6peLxjmpecxhUnnmb](https://docs.feishu.cn/docx/CsO6dB79co6peLxjmpecxhUnnmb)

## Gdb

- `gdb` 中输入 `enable tui` 可以打开源代码展示窗口。
- 可以通过指令 `focus asm` / `focus reg` 调整聚焦
- `(gdb) layout asm`: 显示汇编代码
- `layout src` 显示 `c` 代码
- `layout spilt` 显示 `c` 代码和汇编代码
- `(gdb) start`: 单步执行，运行程序，停在第一执行语句
- `(gdb) continue`: 从断点后继续执行，简写 `c`
- `(gdb) next`: 单步调试（逐过程，函数直接执行），简写 `n`
- `(gdb) step instruction`: 执行单条指令，简写 `si`
- `(gdb) run`: 重新开始运行文件（`run-text`：加载文本文件，`run-bin`：加载二进制文件），简写 `r`
- `(gdb) backtrace`: 查看函数的调用的栈帧和层级关系，简写 `bt`
- `(gdb) break` 设置断点，简写 `b`
- 断在 `foo` 函数：`b foo`
- 断在某地址: `b * 0x80200000`
- `(gdb) delete` 可以删除所有断点
- `(gdb) finish`: 结束当前函数，返回到函数调用点
- `(gdb) frame`: 切换函数的栈帧，简写 `f`
- `(gdb) print`: 打印值及地址，简写 `p`
- `(gdb) info`: 查看函数内部局部变量的数值，简写 `i`
- 查看寄存器 `ra` 的值：`i r ra`
- `(gdb) display`: 追踪查看具体变量值
- `(gdb) x/4x`: 以 16 进制打印 处开始的 16 Bytes 内容

更多命令可以参考 [100个gdb小技巧](https://wizardforcel.gitbooks.io/100-gdb-tips/content/)

- `(gdb) info breakpoints`: 查看断点
- [GDB禁用和删除断点 (biancheng.net)]

## 网站

- [`zxgio/gdb-cheatsheet`: GDB cheats](https://github.com/zxgio/gdb-cheatsheet)heet for reversing binaries (github.com)

![](img/02462eb7de48060862d88db36200329f.png)

- [`gdb-cheatsheet.pdf`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OVlTbGY10onoUgx3AhfcXpn9nRe)

![](img/7d85e27d6576ecfad19f6360e085095d.png)

- [`GDB Cheat Sheet.pdf`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LHACbxIQNo1FJHxW3vyc1QMNnwe)
