# CI for AI system

> 来源：[https://learning.feishu.cn/docx/HSPwdyZIDoZKIxxJ03tcrJH6nMg](https://learning.feishu.cn/docx/HSPwdyZIDoZKIxxJ03tcrJH6nMg)

https://hud.pytorch.org/benchmark/compilers

1.  TorchBench: Guarding the Performance

1.  very good CI infra

# TorchBench: Guarding the Performance

https://www.youtube.com/watch?v=CRqkcAQv0T4

TorchBench: Guarding the Performance of the PyTorch Ecosystem with Continuous Benchmarking - Xu Zhao, Meta

![](img/1163e3e62139c89856affbd961a162dc.png)

![](img/9fc0e7541bf975deea1c46cf5f8931f5.png)

![](img/08dd4ecd09e94d2b37f739b15c423955.png)

![](img/c87ad2e28f3140f192f8ca10fdee4632.png)

![](img/8ff151457cec170ea6e724325080a96f.png)

![](img/c5335832dc3cddd3ef82e6f26574a8ab.png)

![](img/7e0ea644fe7512bbddce13a35041e281.png)

noise treatment:

1.  CPU runners: 隔离 cpu core，防止进程间相互影响。

1.  GPU runners: tune the freq, 保证结果的稳定性。

![](img/2c72dac67aca579f8374e47e869a3ec1.png)

![](img/91d271080f9eb9b480d220ef8c2e6da2.png)

![](img/0c6c1caddac4734ee4dcf6a8ccd6b977.png)