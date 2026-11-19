
# Ascend NPU `lm-evaluation-harness` 评测 `Intern-S1`

> 来源：[https://aicarrier.feishu.cn/docx/JKBfdnMjYo6DV7xWPdDcafzCnLd](https://aicarrier.feishu.cn/docx/JKBfdnMjYo6DV7xWPdDcafzCnLd)

## 一、安装环境依赖

### 1.1 创建开发机环境

前往InternStudio Ascend控制台创建开发机

![](img/83134b0612eff7687ba7c009567b6997.png)

填写开发机名称、选择镜像、资源配置选择NPU*1最后点击立即创建。

![](img/ef8fd1829f6fab47f39dc1de354c8f5b.png)

开发机点击并且创建成功后，点击进入开发机。

选择Terminal打开终端

![](img/aa7e7b7b88c5528079b6052f993af471.png)

在终端输入以下命令查看NPU 信息。

```
npu-smi info
```

### 1.2 创建`lm-eval`环境

执行以下代码创建`conda`环境并下载`uv`：

```
conda create -n lm-eval python=3.11 -y
conda activate lm-eval

pip config set global.index-url https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple
pip install uv
```

`uv`下载完成后安装依赖：

```
# lm-eval
uv pip install git+https://gh-proxy.com/https://github.com/EleutherAI/lm-evaluation-harness.git
uv pip install lm-eval[api]

# vllm
uv pip install vllm==0.11.0
uv pip install vllm-ascend==0.11.0rc1
```

## 二、启动vLLM服务部署`Intern-S1-mini`

### 2.1 设置昇腾相关环境变量

```
source /usr/local/Ascend/ascend-toolkit/set_env.sh
source /usr/local/Ascend/nnal/atb/set_env.sh
```

### 2.2 启动vLLM服务

```
export VLLM_USE_MODELSCOPE=true
vllm serve /root/share/new_models/Intern-S1-mini --trust-remote-code --enforce-eager
```

![](img/425b4fbdeb79ee8b587677fef230e490.png)

### 2.3 启动`lm-eval`进行`gsm8k`评测

```
lm-eval run \
  --model local-chat-completions \
  --model_args "base_url=http://127.0.0.1:8000/v1/chat/completions,model=/root/share/new_models/Intern-S1-mini" \
  --tasks gsm8k \
  --apply_chat_template \
  --limit 10 # 可去除，去除后进入完整评测
```

等待完成后可以看到：

![](img/25bce901797a83e380b7c18d382554d1.png)

## 三、通过API评测`Intern-S1`

### 3.1 获取`API_KEY`

访问链接：`https://internlm.intern-ai.org.cn/api/callDetail`

并且创建`API Token`：

![](img/992f3b4cc1504e2ae641b2717446464d.png)

### 3.2 启动`lm-eval`进行`gsm8k`评测

```
export OPENAI_API_KEY=""
# 在此处填写api_key
lm-eval run \
  --model local-chat-completions \
  --model_args "base_url=https://chat.intern-ai.org.cn/api/v1/chat/completions,apply_chat_template=True,model=intern-s1,eos_string=<|eos|>" \ # model指向intern-s1模型 还可以选择其他模型进行评测
  --tasks gsm8k \
  --apply_chat_template \
  --limit 10 # 可去除，去除后进入完整评测
```

等待完成后可以看到：

![](img/9662d327943de71727b92b43dd2e35a9.png)

### 3.3 模型列表

![](img/3b7f2f7b801f10e4cc5e6c2515276736.png)

## 参考文档

[Ascend NPU vLLM 部署Intern-S1-mini](https://aicarrier.feishu.cn/wiki/RMWUwISpgigRFgkFpD0c7QKAn8g)
