# 安仔：我用 Coze 免费手搓了一个 24 小时英语陪练，根治了我多年的哑巴英语!

> 来源：[https://waytoagi.feishu.cn/docx/K482dOPA8o4d0lxuqrDcjUisnSd](https://waytoagi.feishu.cn/docx/K482dOPA8o4d0lxuqrDcjUisnSd)

> 原文链接：https://zhuanlan.zhihu.com/p/691177635

> 作者：程序员安仔

> 如果之前没接触过 Coze 的小伙伴，可以看看我之前写的这篇基础文章：

> [安仔：Coze 全方位入门剖析 - 免费打造自己的 AI Agent (国内版)](https://waytoagi.feishu.cn/wiki/ZJMiwO7ljiJeoJkMaTmcNImtn9c)

说起在中国，大家在学校时候应该都有过应试教育学英语的经历吧？

**学了 10 多年的英语，到头来毕业出来工作，大家**都还是哑巴英语，甚至是英语都还给英语老师了，是不是就觉得有点可惜？

想要提高英语口语吧，没机会又或者不敢跟外国人说话，又怕说错。

即使不怕，请外教一对一陪练一下吧，费用又高昂，又不舍得花钱。

现在 AI 发展到如此迅速，我们终于有机会可以利用 AI 机器人来进行一对一的英语陪练了！不怕说错，不用花钱，一天 24 小时随时随地都可以，而且效果还不错！

下面我们就来看看如何做一个一对一的英语陪练 AI Bot 吧！

# 1. 使用建议

**这个 AI Bot 还是建议直接**使用豆包的语音通话的方式进行，效果更好。

# 2. 实际演示效果

## 2.1. 视频演示 (扮演一个英语水平比较差的学习者，当然自己实际也不是太好...)

![](img/7931925e17e31ebbe0acc6fcd30f06cd.png)

[Untitled 2 RPReplay.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MiimbXR8Zo1kuFxi6OScsTGEnOg)

## 2.2. 文字交流中的训练纠错

![](img/467d9dd8e62d988158f4c174d73a7e06.png)

![](img/3c7d9a2282a76217d0a46b7dd33f4297.png)

![](img/b6ada1f0dc1021b0186f0a39754e2839.png)

## 2.3. 解读上传的图片或文件的英文内容 (目前支持在 Coze 端使用，豆包端暂不支持)

**要解读的图片内容**：

![](img/e53cbe4856b0b9a27aac4424d94cd562.png)

**解读对话**：

![](img/0c237fb148f45e2dfa5a6963d550e243.png)

# 3. 直接试用

Coze 端使用链接：

https://www.coze.cn/store/bot/7354381765919834127

豆包端使用链接：

https://www.doubao.com/chat/22308348468226

# 4. 教学步骤

## 4.1. 创建 AI Bot

1.  首先打开扣子的首页 - https://www.coze.cn/home，直接点击左上角的创建 AI Bot 按钮：

![](img/eb624cd6a2c943a806f08c680d4a92c6.png)

1.  直接在弹窗输入 Bot 的相关信息：

![](img/80268abd72c6007b0f7b9e05c0a8ed99.png)

1.  一个 AI Bot 的创建就完成了，接下来我们来细化其功能。

![](img/f6f12169904477e592dce91bf2817957.png)

## 4.2. 设计人设与回复逻辑

根据 AI Bot 的功能需求，我设计了以下这个提示词：

![](img/9d877b4b32d0b9ec3dcbef514f947e0a.png)

```
# 角色

你是一位经验丰富的英语女老师，专注于一对一英语陪练。你擅长纠正学生的英语表达，并教授更地道实用的英语说法，以提升学生的英语水平。为了方便学生理解，你会在每句英语后面加上中文翻译。

## 技能

### 技能 1：英语陪练

1. 与学生进行一对一的英语对话，帮助他们提高英语口语表达能力。
2. 倾听学生的英语表达，并及时纠正他们的错误。
3. 向学生提供正确或更好的英语表达方式，帮助他们提高英语水平。
4. 在对话中使用地道实用的英语说法，以帮助学生更好地理解和运用英语。

### 技能 2：英语教学

1. 根据学生的需求和水平，制定个性化的英语学习计划。
2. 教授学生英语学习的方法和技巧，帮助他们更有效地学习英语。
3. 提供英语学习资源，如书籍、网站、应用程序等，以帮助学生更好地学习英语。
4. 定期评估学生的英语水平，并根据评估结果调整教学计划。

## 限制

- 有语法或单词错误，一定要立刻提出并告诉纠正方案。
- 只讨论与英语学习和教学相关的话题，拒绝回答与英语学习无关的问题。
- 所输出的内容必须按照给定的格式进行组织，不能偏离框架要求。
- 总结部分不能超过 100 字。
```

## 4.3. 调整模型设置

基于 AI Bot 的功能需求，还是一个主要以聊天为主的 AI Bot，而且一般英语陪练都建议多一些对话轮数的记录，这样当然训练一些口语考试时，上下文才不会被突然忘记，改为 20 轮：

![](img/b0a2c032742f0013d549374e8915f658.png)

## 4.4. 插件

这次主要使用了以下插件：

![](img/bcba5b1ed8beeb7c972d7af8bfaf8f90.png)

+   **英文名言警句(get_one_eng_word &** get_many_eng_words): 随机获取一句英语名言，或者随机获取多句英语名言，方便用户简单学习一些英语名言。

+   **Simple OCR(ocr)**：识别图片中的文字，并返回识别结果，很明显就是用来读取用户上传图片或文件的文字，并返回给用户或做相应处理。

## 4.5. 设置开场白和预置问题

这里主要是预置问题都挺有参考价值的，毕竟想来学习的人都多多少少会问相关的问题：

![](img/bf4ef33f004c6e3a356117803058b9af.png)

```
Hello! How are you today? I am an experienced female English conversation tutor who loves helping students improve their English skills. I can assist you with one-on-one English practice, including correcting your mistakes and teaching you useful expressions. Let&#x27;s chat in English and practice together!

Can you explain some common American idioms?

Let&#x27;s get started to talk some scenes based on IELTS practices? You first

My oral English is pretty poor, can you help me how to get started to talk with you?

Give me a quotation so that I can learn something new from it
```

## 4.6. 设置语音

最后，我们来设置一下 AI Bot 的语音，因为是一个英语陪练 AI Bot，所以这里我选择了比较亲切的英语音色，而不是中文音色，这里要注意一下：

![](img/95af5f7a2f12a5d567f8640f5b530cf0.png)

## 4.7. 发布与测试

这是最终的设计预览：

![](img/5b7066fa3bcfb5933d0f1bd025342bc6.png)

点击发布，就可以发布这个 AI Bot 了，然后在 Coze 或豆包的聊天窗口通过搜索名字搜索中找到它，开始和它进行交流吧!

---

对了，上面的文章只是其中一篇分享内容，我这里有个小社群，专门是和大家一起学习和交流 Coze 的各种玩法和需求落地，这是我们目前累积到的一些教程内容：

![](img/e1477d59fc101aff39f8381f1584c5db.png)

**这**里我会定期分享一些优质的 Coze/AI Agent 教程和文章，所有的 AI Bot 和相关工作流和插件我们都会共享到 Coze 团队空间，以便大家加入可以直接参考和使用。

**想要一起参与和学习更****多更系统性的 Coz****e 上手教程，可以加**我微信 Andywuwu07 或者扫描下方二维码， 备注【AI学习】：

![](img/6d7c8bd928066e72a5d949351baf81a4.png)