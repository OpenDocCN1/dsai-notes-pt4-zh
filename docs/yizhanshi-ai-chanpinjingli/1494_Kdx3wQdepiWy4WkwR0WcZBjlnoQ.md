# model_baichuan

> 来源：[https://v11enp9ok1h.feishu.cn/docx/GgiidmvDZobcGEx5mEscKSaOnqb](https://v11enp9ok1h.feishu.cn/docx/GgiidmvDZobcGEx5mEscKSaOnqb)

# 介绍

1.  偶然测试，baichuan-13b模型还不错，后面将基于baichuan-13b模型做一些项目，全都列举在本文件夹中。

1.  记录对baichuan-13b模型的相关修改。

## 添加flash attention

1.  本来模型是没有flash attention[的，就把pytorch的那个函数给加上了，已经PR，参考链接http](https://huggingface.co/baichuan-inc/Baichuan-13B-Chat/discussions/21/files)s://huggingface.co/baichuan-inc/Baichuan-13B-Chat/discussions/21/files 如果后期没有被合并，参考这里也可以进行修改。