# 20250502-一致性新高度，实测Midjourney Omni-Reference 全向参考：不仅角色，万物皆可一致。

> 来源：[https://aigcreative.feishu.cn/docx/Bd41d7g1ZoAKiHxuF8QcSX0RnAH](https://aigcreative.feishu.cn/docx/Bd41d7g1ZoAKiHxuF8QcSX0RnAH)

> 链接: https://mp.weixin.qq.com/s/z6WMNiIInLRY3tRsNrZdyw

> 作者：汗青 AI Talk 公众号：汗青 AITalk

![](img/3d39c48a0bc1e7dd3ab4e00d1a0faa72.png)

Midjourney v7上线后，配套的角色参考（cref）功能一直迟迟未公布。当时我就有个预感，也在评测文章里说过：这可能是个全新功能的伏笔。

果然，今天Midjourney正式公布「 Omni-Reference」，即 「全向参考」，或翻译成「万能参考」，配合--oref、--ow两个参数使用，这不是Character Reference的v7升级版，而是一次全面的参考进化。

# 惊艳的一致性和细节迁移

直接看效果吧，这是我的参考原图。

![](img/86be7217681ac62872509b110f85b476.png)

然后，我们在Midjourney Web里调用一个新功能 Omni-Reference，设置一个全新的变量参数--ow，赋值400，这时我们就启用了MJ的 Omni-Reference功能，得到以下结果。

![](img/cbcfcdbb7ea1d3addcc71a2f2a9ebc05.png)

![](img/ddc637c530fadbde43408086adb2ace9.png)

![](img/7a5e170df3b7edca36d80c62cc575822.png)

![](img/291036423d9058cff0d261b346bf0a76.png)

![](img/a53c35c9f0539ae3442e1b60bf7b2a18.png)

![](img/0836a86f1991660bf55d9e74a4cd61c3.png)

不能说很像，只能说是一模一样。

同时，请关注皮肤的质感迁移，非常非常的到位。甚至连指甲部位的装饰，脸部两侧的头饰细节都一模一样。

我的个人观点：虽然不能现在就下结论说这个效果超越了Lora（二者逻辑还是有些区别,后面会讲），但单单说“像”这一点，绝大部分Lora是根本没办法做到这种程度的，尤其是在细节和画面质量上。

![](img/f902496c1271e3c3548a2f58f1c857c1.png)

![](img/faedf9483d100c2d88423aa7bd0f499b.png)

![](img/febdb93cc9004ab7cb7dc042ecc6e517.png)

![](img/9e23c0c1208d9d3900fdca4bcfde9d20.png)

太变态了。

接下来，我们来试着根据一个女孩的照片，模拟她在不同的场景和服装下的角色一致性。

原图如下，我们这次把ow的赋值设定在200、700两个档位：

![](img/b4fdb696fa1fc40b7a08789c3ba750fa.png)

![](img/42c3c464be00a74be33f9ba8317b104a.png)

分别测试的提示词如下：

+

+

+

+

+

```
side shot of a girl is cookingside shot of a girl is drinking a girl is running a girl is reading book on bed a girl is makeing up for her self, and wearing a blue shirt.
```

结果如下：

![](img/cb553f44bbdd6ea40d317156d4aae7e7.png)

![](img/dbf3cf20c8fa82634820a480fc73d114.png)

![](img/356176c77e926983d8b19032ba3cb2ae.png)

![](img/48876467a508e0df489ac8ae8853f7ca.png)

![](img/0c7a6023b118c46390cf7fd03b7f82af.png)

![](img/4d0cc1bed251f96195e60aef2523960e.png)

![](img/997f13ee642034568781667ea5e2b05e.png)

![](img/ecff868d0a1933c33c2cd34e346153e9.png)

无论是场景的变化还是服装的变化，完成得都非常好，这和之前MJ的角色参考功能完全不是一个段位的效果（用过的都知道那玩意儿基本没用）

# 对比4o和Lora的优缺点

好了，聊到这儿说说很多人力捧的用4o做角色一致性的问题，为什么我一直强调4o很强大但它代替不了工业生产呢？因为本质上它目前就干不了这个活，同样的图片，看看4o的模仿结果，我开了几次，根本得不到能体现一致性的长相，尤其实在写实风格下：

![](img/f92c285179757c8428c7cfd946fb37e5.png)

![](img/6dc6cae0ee4d920bf9ba8cb338eb6598.png)

所以，4o目前只能做一些基本卡通风格的一致性生产，而且并不是因为卡通风格它支持的好，而是因为卡通风格对一致性的要求很低。

这就是为什么我说，什么工具干什么活，4o很强，但你拿它做影视作品，就是自找苦吃。

至于Lora，我斗胆用自己的照片试了一下真人直出在 Omni-Reference下的效果，说真的完全可以接受。

原图：

![](img/ac8a27dd5e5782d4d653b49ca0537fd3.png)

生成图像：

![](img/a2260342bd4887fcb5c1d88fec177da9.png)

![](img/3c7ea9a13ae437beec692acf7dc895ee.png)

Lora的逻辑是建立在多张照片参考的基础之上的，而MJ只需要一张照片，直接拿来对比有点不公平。理论上讲，Lora在具备模型训练经验的情况下，在还原真人的能力上应该是更逼真，但二者的工作量不是一个量级，门槛也有区别。 Omni-Reference在只用一张照片的情况下还原到这个程度，我是能接受的。

所以如果你想要一个最逼真的真人效果，经验和数据量也够，可能还是Lora更优，这个还需要大量的测试才能下结论，但是MJ胜在门槛低，而且美学效果更好。

# 何为全向参考

那么为什么这次MJ起的名字是 Omni-Reference全向参考（万物参考），而不是角色参考？答案很简单，它不光能支持人的一致性，还能支持物体、机械体、各种风格。

看下面这个逆天的例子，参考原图如下：

![](img/0de8e70f26cf3a85f1b36da0de04ca39.png)

ow设置在300-600区间，模拟这个机械体的各种状态：

![](img/e0ed925a26d8d72ca9341c1a1351173b.png)

![](img/f4d72a3573fa9ddc822dd2e6385efb48.png)

![](img/3d93000b2626bd5115b207c24756c074.png)

太变态了。

公平地说，这种效果以前根本没有AI图像模型做到过，对各个角度的推理都非常准确，机械臂在各种形态下都维持了很强大的一致性。

![](img/fdfec5c3eb796c2df673a102cb9c7463.png)

![](img/eb1d4a875f4ae0954cf20e6255a62df8.png)

![](img/b3fb3ac0b3403abcd2c54d62a16bde6b.png)

![](img/1a88f030250c66040a4031532360276c.png)

再看一个装置物体的测试，有非常多的细节, 同时我们把它们迁移到不同的场景里。 你会发现效果强悍得变态，

![](img/623b2f73403e3e7b856a960b169fcb31.png)

![](img/d8a2486c58734a5267348890047abd0f.png)

同时，「 Omni-Reference 全向参考」还能实现同样一个场景的不同角度，做设计的同学都明白这个功能的重要性，因为需要维持一致性的不仅仅是角色，叙事的背景和环境同样重要，一旦穿帮，代入感急剧下降。

![](img/dac92a3a74f6e36af3dff828d6fa0041.png)

![](img/dd3d4fdfa7c2751c7a4a6e12a14b8c51.png)

所以这次的新功能并非只是解决了人物角色的问题，它本质上提供了一种场景、物体等任何“主体”维持一致性的能力，这个意义非常大。

# 多风格支持

再来看看另外一个大赛道：二次元动漫在 Omni-Reference下的表现。我们测试一个2D风格的动漫女孩子，众所周知动漫风格可以容纳的细分是很广的，写实的、萌系的、线条感的、2.5D的，虽然都叫动漫，可是却完全不一样。4o的另一个硬伤，实际上是在量产动漫风格的图片时，会随机得到完全不同的结果。

Midjourney配合 Omni-Reference完全解决了这个问题。看下面这个2D女孩子的参考表现，实际上在实现难度上比真人还要大，但拿捏的非常好。之前很多朋友会使用iw和sref来实现类似效果，但是在保持风格时，人物的一致性又没办法维持（Cref根本没法用），现在这个问题完全解决了。

![](img/5d153f54b12f183a7ec176416f400818.png)

生成不同角度，人物的细节和画风维持得非常稳定：

![](img/9cfad124c9c89a8dbc46ba8c2705bbc5.png)

![](img/6d87d8e910603dd4b06308d45dd3fd50.png)

![](img/989bdb17ec3f00fbfd5c09fb62e36a7b.png)

![](img/0edb70da14504880e820312656d7626e.png)

3D萌系风格效果：

![](img/b49a35d08a52f098b5fd007f8d0af1ed.png)

![](img/fadb4e2163705c005a5d9bc11a0ad15e.png)

# 总结

Omni-Reference的表现符合了我之前的期待，确实是一次非常有价值的升级，其意义甚至不小于v7的发布，因为还是那句话：对于创作者来说，需要的不仅仅只是“图像质量”“视频质量”这样的硬指标，更多的还是方便创作的效率工具。

一致性是各位创作者长久以来非常苦恼的老大难问题， Omni-Reference的出现不能说完全解决了这个痛点，但至少是一个巨大的提升。方便的操作加上MJ v7的图像质量，真的是目前非常好的解题方案。

![](img/9cf9485dc2244d4052689db8f97221be.png)

![](img/686ca26d9d3b5b850917b7a9886d1059.png)

其实Mj今天的官方推特上也说明了，他们自己也认为 Omni-Reference是一个潜力很大，等待创作者不断挖掘的状态，我想这也说明了AI的核心价值：可能性。

至于尚存的缺陷，再给技术一些时间就好，没必要 吹捧或者贬低谁，真的，过两天没准又有什么新东西出来，与其较劲，不如赶紧试一试。

看趋势，别看瑕疵。

无论如何，这绝对是AI一致性的新高度。

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>

# 知识库动态