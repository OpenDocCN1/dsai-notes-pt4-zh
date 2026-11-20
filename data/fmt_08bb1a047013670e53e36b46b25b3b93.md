
# model_baichuan

> 来源：<https://v11enp9ok1h.feishu.cn/docx/GgiidmvDZobcGEx5mEscKSaOnqb>

## 介绍

1. 偶然测试，`baichuan-13b` 模型还不错，后面将基于 `baichuan-13b` 模型做一些项目，全都列举在本文件夹中。
2. 记录对 `baichuan-13b` 模型的相关修改。

## 添加flash attention

本来模型是没有 `flash attention` 的，就把 `pytorch` 的那个函数给加上了，已经 PR，参考链接 <https://huggingface.co/baichuan-inc/Baichuan-13B-Chat/discussions/21/files>。如果后期没有被合并，参考这里也可以进行修改。
