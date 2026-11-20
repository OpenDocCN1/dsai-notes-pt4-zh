
# 技巧二：多实验

> 来源：[https://my.feishu.cn/docx/EhfSdoJ9XoHDdpxVWN9cIUvCnFb](https://my.feishu.cn/docx/EhfSdoJ9XoHDdpxVWN9cIUvCnFb)

我还想教给各位第二个技巧：多试验。

图片生成遇到不符合预期的情况，不要慌，分析问题，然后使用控制变量法，一个个调整图片，不要急，上面我写的 prompt 还有一个地方，不知道各位有没有注意，就是开头的 stock photo，试试将这两个词删掉会怎样？

```
two Asian men in suits shaking hands,say goodbye in front of the main entrance of the office building, focus on two hands, taken with Canon, background bokeh
```

生成结果也依然满足需求，而且手指的数量也准确了，说明这个 Stock Image 对模型的影响不是很大。

![](img/16cd2fae857c32f01bc12d2f25de9542.png)
