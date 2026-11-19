
# 2023-07-31非程序员小白版本地部署ChatGLM2-6B +LangChain 个人知识库模型-今日头条

> 来源：https://xuqiwei1986.feishu.cn/docx/FoGgd5Tc5obYeexghe5cpH3wnDc

> 🔗 原文链接：https://www.toutiao.com/article/7261776672708543036/?log_from=46cb3d951c27a_1692254353775

## 非程序员小白版本地部署ChatGLM2-6B +LangChain 个人知识库模型

原创 2023-07-31 08:51 · 北斗九号的水哥

## ChatGLM2-6B 部署

我这次采用的是本地部署，机器是 Macbook pro M1 64G

### 进入你的系统“终端”

```
# 键盘：command + 空格键
# 搜索“终端”，点击“打开”，进入“终端”界面
```

![](img/350da6fbd03d744a1e4bced3be470d66.png)

![](img/4658a81a6a12573217ec15e1319aa1b5.png)

### 下载源码

```
# 在终端里面，输入命令：
git clone <https://github.com/THUDM/ChatGLM2-6B.git>
# 系统这时会自动下载源代码，如果下载不成功，需要自己到Github去手动下载文件
# 地址：<https://github.com/THUDM/ChatGLM2-6B>
```

### 安装依赖

```
# 下载完成后，依次在终端内运行如下命令：
1、cd /xxx/xxxx/ChatGLM2-6
# 进入模型所在的文件夹
2、pip3 install -r requirements.txt
# 运行依赖文件的安装
# 其中 `transformers` 库版本推荐为 4.30.2，`torch` 推荐使用 2.0 及以上的版本，以获得最佳的推理性能pip install -r requirements.txt
```

### 下载模型

```
# 推荐从[这里](<https://cloud.tsinghua.edu.cn/d/674208019e314311ab5c/>)手动下载模型参数文件，并将下载的文件替换到本地的 `chatglm2-6b` 目录下。
```

### 参数调整

```
# 因为前面改了模型默认下载地址，所以这里需要改下路径参数
# 找到模型文件夹中的 web_demo.py 文件，并用文本编辑形式打开
# 找到以下代码，并将里面的“xxx/xxxxx//chatglm2-6b”改成自己本地的文件夹路径
# 不知道如何查找Mac文件夹路径的，看[这里](<https://www.google.com/search?q=mac%E6%96%87%E4%BB%B6%E5%A4%B9%E8%B7%AF%E5%BE%84&oq=mac%E6%96%87%E4%BB%B6%E5%A4%B9%E8%B7%AF%E5%BE%84&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIHCAEQABiABNIBDTc3OTAwMTQ0ajBqMTWoAgCwAgA&sourceid=chrome&ie=UTF-8>)
tokenizer = AutoTokenizer.from_pretrained("/xxx/xxxxx/chatglm2-6b", trust_remote_code=True)
model = AutoModel.from_pretrained("/xxx/xxxxx//chatglm2-6b", trust_remote_code=True).cuda()

# 如果想要本地访问，需要修改此处最后的“.cuda()”为“.to('mps')”
model = AutoModel.from_pretrained("/xxx/xxxxx//chatglm2-6b", trust_remote_code=True).to('mps')
```

### Web 模式启动

```
# 以上顺利完成后，在终端输入命令：
python3 web_demo.py
```

你会看到以下界面，这也代表你的ChatGLM2 顺利完成了本地部署。

![](img/6d89ebd17f36621c09011b3f39d42684.png)

## LangChain + ChatGLM2-6B 构建知识库

### 项目部署

#### 下载源码

```
# 同样的方法，进入“终端”，下载LangChain-ChatGLM 项目的源码
git clone <https://github.com/chatchat-space/langchain-ChatGLM.git>
# 系统这时会自动下载源代码，如果下载不成功，需要自己到Github去手动下载文件
# 地址：<https://github.com/chatchat-space/langchain-ChatGLM>
```

#### 安装依赖

```
1、cd /xxx/xxxx/langchain-ChatGLM
# 进入模型所在的文件夹
2、pip3 install -r requirements.txt
# 运行依赖文件的安装
# 安装的过程应该会有很多问题，我们放在最后的模块来讲，如果迫不及待，也可以跳到最后看解决方案
```

#### 下载向量模型

```
# 下载 Embedding 模型text2vec-large-chinese，依然推荐手动到主页下载
# 地址：<https://huggingface.co/GanymedeNil/text2vec-large-chinese/tree/main>

# 到这里，你本地应该有三个文件夹的内容，分别是：“ChatGLM2模型文件夹”、“LangChain-ChatGLM文件夹”、“text2vec模型文件夹”，如果不是，请回头看看漏了哪一步
```

#### 参数调整

```
# 模型下载完成后，请在LangChain-ChatGLM文件夹里面，找到“configs/model_config.py”文件，对里面的`embedding_model_dict`和`llm_model_dict`参数进行修改。主要修改的是文件地址“xxx”的项目为本地的模型文件夹地址

embedding_model_dict = {
    "ernie-tiny": "nghuyong/ernie-3.0-nano-zh",
    "ernie-base": "nghuyong/ernie-3.0-base-zh",
    "text2vec-base": "shibing624/text2vec-base-chinese",
    "text2vec": "/xxx/xxxx/text2vec",
    "m3e-small": "moka-ai/m3e-small",
    "m3e-base": "moka-ai/m3e-base",
}

llm_model_dict = {
    ...
    "chatglm2-6b": {
        "name": "chatglm2-6b",
        "pretrained_model_name": "/xxx/xxxx/chatglm2-6b",
        "local_model_path": None,
        "provides": "ChatGLM"
    },
    ...
}

# LLM 名称改成 `chatglm2-6b``LLM_MODEL` = "chatglm2-6b"
```

### 项目启动

#### Web 模式启动

```
# 进入LangChain-ChatGLM 文件夹
cd /xxx/xxxx/langchain-ChatGLM
# 启动项目
python3 webui.py
# 如果没有自己打开，可以找到终端代码块里面的web地址：<http://0.0.0.0:7860>，复制到浏览器中打开
```

看到以下界面，这也代表你的LangChain-ChatGLM顺利完成了本地部署，可以开始进阶的玩耍啦

![](img/a4a2a8e4f0ac6dabc7cfe120cc4d9401.png)

## 遇到的问题汇总 & 解决方案

```
# 对，哪有那么顺利，过程中，也是遇到了很多挑战的。
```

### 安装依赖文件出错

```
# 在安装“requirements.txt” 文件的时候，经常出现卡住的情况

# 解决方案1：看到卡住的模块，就手动安装，通过以下命令
1、`pip3 install <模块名称>` 或 `pip3 install --upgrade <模块名称>`

# 解决方案2:修改“requirements.txt” 文件里面的文件，先“注释”掉卡住的文件，后续再单独安装，可以通过网络搜索找到无法安装的解决方案
2、用文本编辑模式打开“requirements.txt”，然后在卡住的项目前面加“#“号

# 当安装过程中出现长时间卡顿，或者严重的错误时，可以使用以下命令终止当前运行
`Ctrl + C` 组合键
```

### 导入本地知识库后，无法使用

### 安装完成后，导入文件，使用个人知识库时提示：`File "/Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages/langchain/vectorstores/faiss.py"` , line 268 , in `similarity_search_with_score` `docs = self.similarity_search_with_score_by_vector` ( `TypeError`: `MyFAISS.similarity_search_with_score_by_vector` () got an unexpected keyword argument `'filter'`

# 解决方案1，通过上面的文件路径，找到问题的代码函数，将对应的`filter` 注释掉。

# 解决方案2，找到官方的Gibhub：`https://github.com/chatchat-space/langchain-ChatGLM/issues` ，在上面查看是否有同类型的问题被提出过，找到官方或社区的答案。上面这里的问题，我是自己解决了之后，才发现官方已经解答过这个问题了。
