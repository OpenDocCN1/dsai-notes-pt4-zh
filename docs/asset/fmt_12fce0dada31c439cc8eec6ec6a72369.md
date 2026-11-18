
```
A = relax.Var("A", relax.TensorStructInfo((128, 128), "float32"))
B = relax.Var("B", relax.TensorStructInfo((128, 128), "float32"))

bb = relax.BlockBuilder()

with bb.function("main"):
    with bb.dataflow():
        C = bb.emit_te(te_matmul, A, B)
        D = bb.emit_te(te_relu, C)
        R = bb.emit_output(D)
    bb.emit_func_output(R, params=[A, B])

MyModule = bb.get()
MyModule.show()
```

# Chap 10 - Summary and Future Works

## Import Model From PyTorch

直接看 slide 好像就行，作者眼中的要点总结。

没看到非常有价值的 future work 总结。

![](img/8dbb1ff6c4685a4b957d67ea567cb3b6.png)

[10-Summary-Future.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SrMHbfEYAoVvHMxhukqcuC5jnnq)
