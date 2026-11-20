
# vLLM, FastChat, Xorbits Inference

> 来源：[https://v11enp9ok1h.feishu.cn/docx/GCwudXFaFouE9mxh1F7cAr6SneC](https://v11enp9ok1h.feishu.cn/docx/GCwudXFaFouE9mxh1F7cAr6SneC)

`https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie`

本章节将带领开发者更方便的接入目前主流的底层模型中间件框架，方便用户基于其开发应用。

在开始适配之前，请确定已经按照对应开源框架的安装说明配置好对应的开源框架。

## FastChat

Github链接：`https://github.com/lm-sys/FastChat`

快速安装

```
pip3 install "fschat[model_worker,webui]">=0.2.33
```

使用0.2.33版本之后的版本才能正常支持 ChatGLM3-6B

## vLLM

Github链接：`https://github.com/vllm-project/vllm`

快速安装

```
pip3 install vllm>=0.2.2
```

使用0.2.2版本之后的版本才能正常支持 ChatGLM3-6B

## Xorbits Inference

Github链接: `https://github.com/xorbitsai/inference`

快速安装

```
pip install "xinference[all]">=0.6.3
```

- 使用0.6.3版本之后的版本才能正常支持 ChatGLM3-6B
