
# LLM `KVCache`

> 来源：`https://learning.feishu.cn/docx/L8yDd0O7Yo47Orxk4ahcOGuMnnc`

`https://zhuanlan.zhihu.com/p/686183300`

### 一文读懂 `KVCache` - 游凯超

`The KV Cache: Memory Usage in Transformers`

`https://www.bilibili.com/video/BV1Nw411z7QN/`

## 为什么需要 `KVCache`？数学的解释，形象的解释。

## 为什么不需要 `Q Cache`？

![](img/fbb7ad6ae39ec6166db4e22d7a3cd670.png)

![](img/5eb67cc5e1352d7e3dd40f11b77557a3.png)

从数学上，`Q`/`K`/`V` 矩阵乘，都可以保存 `cache`。

但，`Q` 每次只算最新的即可，不需要历史 `token` 参与。

`softmax` 前后的 2 个 `BMM` (`matmul`)，`k` 和 `v` 是右矩阵，需要当前 token 的 `Q` 和所有历史 token 的 `k`/`v` 做乘法。

`Attn` 的最后一行只依赖于 `Q` 的最后一行，所以 `Q` 不需要 `Cache` 的。

![](img/e95e74f62ca927fdf0ac12c1fc890dff.png)

`32k` 是 `8k` context length 单价的 2 倍。

![](img/0d33c6c98aacd2e97c65ed32fc203ed4.png)

![](img/923a8d4ce3f93db2eb0438c1e494ddea.png)
