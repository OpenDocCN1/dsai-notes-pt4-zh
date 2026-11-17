
最终，分布式异步 `SGD` 是一个很有前景的想法，尽管这还需要很多研究才能让我们不容置疑地推荐它，而非「标准」的异步 `SGD`。此外，许多在 [7] 中所示的想法（如压缩、量化等）可以被用于异步 `SGD` 中，以优化传统的参数服务器的设置。

## 该何时使用分布式深度学习

对每种使用情况，分布式深度学习往往不都是最好的选择。

分布式学习并非廉价——与在单独的机器上相比，因为诸如同步、数据与参数的网络传输等问题，分布式系统往往有一个日常开销。要使分布式训练变得值得，我们需要利用好分布式系统的计算优势来抵消这笔开销。此外，在分布式训练上，设定（包括准备与载入训练数据）及调参都将会变得更为复杂。因此，我们的建议简单明了：在训练时间可以接受的情况下，都（不使用分布式训练而）在单机上训练神经网络。

神经网络的训练时间因为这两种原因而变得很长：网络的大小非常之大（每次迭代都很费时）及数据量非常之多。通常来说，这两种情况将同时出现——因为「大网络、少数据」或「小网络、大数据」的情况往往导致欠拟合或过拟合，不能有很好的泛化效果。

在某些情况下，多 GPU 系统应该首先被考虑（如 `Deeplearning4j` 的 `Parallel-Wrapper` 系统可以让神经网络在单机上轻松进行同步训练）。基于多 GPU 系统对模型进行同步运算对大型网络也是可行的。

另外一个要考虑的方面是，网络传输（耗时）与计算（耗时）之比。分布式训练在网络传输（耗时）与计算（耗时）之比较低时，会比较高效。因为每层的计算量很少，小的、层数少的网络并不适用于分布训练。有参数共享的网络（如 `CNN` 或 `RNN`）往往适用于分布式训练：因为它们每个参数计算量远远高于，如多层感知机或自编码体系架构的计算量。

## Part2 预告：在 Apache Spark 上部署 `Deeplearning4j` 的深度学习网络

在我们介绍分布式深度学习系列文章中，我们将在第三篇的第二部分仔细探讨 `Deeplearning4j` 的机遇 Apache Spark 的参数平均法的实现，并通过一个例子来演示如何在 Spark 集群上来训练神经网络。

## 参考文献

[1] Kai Chen and Qiang Huo. Scalable training of deep learning machines by incremental block training with intra-block parallel optimization and blockwise model-update filtering. In 2016 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP), pages 5880–5884. IEEE, 2016.

[2] Jeffrey Dean, Greg Corrado, Rajat Monga, Kai Chen, Matthieu Devin, Mark Mao, Andrew Senior, Paul Tucker, Ke Yang, Quoc V Le, et al. Large scale distributed deep networks. In Advances in Neural Information Processing Systems, pages 1223–1231, 2012.

[3] Suyog Gupta, Wei Zhang, and Josh Milthrope. Model accuracy and runtime tradeoff in distributed deep learning. arXiv preprint arXiv:1509.04210, 2015.

[4] Qirong Ho, James Cipar, Henggang Cui, Seunghak Lee, Jin Kyu Kim, Phillip B. Gibbons, Garth A Gibson, Greg Ganger, and Eric P Xing. More effective distributed ml via a stale synchronous parallel parameter server. In C. J. C. Burges, L. Bottou, M. Welling, Z. Ghahramani, and K. Q. Weinberger, editors, Advances in Neural Information Processing Systems 26, pages 1223–1231. Curran Associates, Inc., 2013.

[5] Forrest N Iandola, Khalid Ashraf, Mattthew W Moskewicz, and Kurt Keutzer. Firecaffe: near-linear acceleration of deep neural network training on compute clusters. arXiv preprint arXiv:1511.00175, 2015.

[6] Augustus Odena. Faster asynchronous sgd. arXiv preprint arXiv:1601.04033, 2016.

[7] Nikko Strom. Scalable distributed dnn training using commodity gpu cloud computing. In Sixteenth Annual Conference of the International Speech Communication Association, 2015.`http://nikkostrom.com/publications/interspeech2015/strom_interspeech2015.pdf`.

[8] Hang Su and Haoyu Chen. Experiments on parallel training of deep neural network using model averaging. arXiv preprint arXiv:1507.01239, 2015.

[9] Wei Zhang, Suyog Gupta, Xiangru Lian, and Ji Liu. Staleness-aware async-sgd for distributed deep learning. IJCAI, 2016.

©本文由机器之心编译，转载请联系本号获得授权。

✄------------------------------------------------

**加入机器之心（全职记者/实习生）：hr@almosthuman.cn**

**投稿或寻求报道：editor@almosthuman.cn**

**广告&商务合作：bd@almosthuman.cn**

![](img/a573ff7d72f49f8fe283857b964d06fd.jpg)
