# 谷歌Bard_VS_Baize-7B_VS_文心一言体验对比

> 来源：[https://aiyanxishe.feishu.cn/docx/R1CwdR2zgozyRWx9YG9cCu6PnJA](https://aiyanxishe.feishu.cn/docx/R1CwdR2zgozyRWx9YG9cCu6PnJA)

2023年4月4日，来自加州大学圣迭戈分校、中山大学和微软亚研的研究者提出了Baize，该模型是让ChatGPT 自我对话，批量生成高质量多轮对话数据集，利用该数据集对LLaMA进行微调得到的（目前版本还没有RLHF）

![](img/72e5360321c6fd1434d30dd4ec1c2461.png)

关于Baize的详细介绍可以参考：https://mp.weixin.qq.com/s/zxElGfclNbBwTuDG4Qrxnw

+   论文题目：Baize: An Open-Source Chat Model with Parameter-Efficient Tuning on Self-Chat Data

+   论文链接：https://arxiv.org/abs/2304.01196

+   Github：https://github.com/project-baize/baize/blob/main/README.md

+   在线 Demo：https://huggingface.co/spaces/project-baize/baize-lora-7B

之前对LLM模型进行了相关的体验对比，感兴趣的可以阅读如下列表：

谷歌Bard_VS_百度文心一言

Bard vs. Bing vs. ChatGPT效果对比

文心一言 VS ChatGLM-6B对比

GPT4 VS ChatGPT，优劣势对比

下面使用Huggingface的在线demo进行体验，并且与谷歌Bard进行对比：

Note：体验的Prompt来自谷歌Bard_VS_百度文心一言

下面从ChatGLM-6B的六大方面进行测试对比

# **自我认知**

**Bard**

![](img/53cdd0c1d50a4eb472b51b417b07d021.png)

**文心一言**

![](img/b680c43ef84d816f08737fa3dc818c66.png)

**Baize-7B**

![](img/308a787256c01c3c89ecb738e677c826.png)

结论：上述三个模型都有基本的自我认知

# **提纲写作**

**Bard**

![](img/4e3bea2648ea0d5cdc28be733a217c5d.png)

**文心一言**

![](img/d8c9a1d8cb26d3350f4eec8d3ff9b7e0.png)

**Baize-7B**

![](img/3ce6d0aeb3da60b52cf1e02791ed6fc4.png)

结论：Bard和文心一言给出了大纲的列表；而Baize-7B做了简单介绍，最后列出了NLP的相关任务；Baize-7B明显不如Bard和文心一言

# **文案写作**

**Bard**

![](img/287dad4b3e744f87f6c8866189d3c280.png)

![](img/04adc51685600bec38e3ffa16af57f80.png)

**文心一言**

![](img/a0cc59dafa2c07f37d2b49c1a01b278d.png)

![](img/775576d46e9fa749013f075087f38be6.png)

**Baize-7B**

![](img/162e1f0ab02c94ab81a7d272e54ea48b.png)

![](img/ba995c43909da49a28ff936020078bb7.png)

结论：Bard和文心一言都生成了一段文案，而Baize-7B只是把关键词split出来了；在生成新闻方面Baize-7B也弱于Bard和文心一言，并没有生成10个新闻，而是6个

# **邮件写作助手**

**Bard**

![](img/58758dd015efde87b755c51c8340c1da.png)

![](img/f0f0f127cb2492d73ada694d132d9a36.png)

**文心一言**

![](img/098f12074450fef14607aa4622da0850.png)

![](img/affc1a91df6a226ba569523ee87bb90f.png)

**Baize-7B**

![](img/87084fd377f93b868a03e7d6bf749a5e.png)

![](img/fdc0e48717375a0facdd0a218487726c.png)

结论：Baize-7B生成的邮件内容较短

# **信息抽取**

**Bard**

![](img/aa696aeeed5ab1271aacdb9f93409083.png)

**文心一言**

![](img/0c8a6c24228413138d1792fbc6afe9ba.png)

**Baize-7B**

![](img/3122f36c43eb4e91f9d84a42d79b4ad1.png)

结论：Bard的表现是最好的，提取信息最全，其他两个模型只提取了部分信息

# **角色扮演**

**Bard**

![](img/8f970eb33380dca3b3ef90468443a196.png)

**文心一言**

![](img/42619ef2a3cb272bb9e1691d7a3ec0f4.png)

**Baize-7B**

![](img/4280fc3e8cc185159be193f1f31a3892.png)

结论：角色扮演方面Bard表现较好，而文心一言和Baize-7B几乎没有这样的能力

# **评论比较**

**Bard**

![](img/8ed5d84ea8aa42de7af4bdaec161194d.png)

**文心一言**

![](img/b906ebbbacd65d4f1e7b9beb0476470c.png)

**Baize-7B**

![](img/d0d909a5677641c1a0f59a9e265512fc.png)

结论：文心一言拒绝回答这样的问题，而Bard和Baize-7B都给出了自己的看法；

# **旅游向导**

**Bard**

![](img/aadf48c891142c1de03d4e455a40cccf.png)

**文心一言**

![](img/7199db8281a21c638db84179d3aaf2d0.png)

**Baize-7B**

![](img/f8845d461a0185c24a5ecc50d3eef723.png)

结论：在旅游推荐方面，上述三个模型都给出了答案，但是Baize-7B的答案没有换行，缺少条例性