# 苹果AI后台提示词曝光，工程师：别幻觉！别捏造事实！

> 来源：[https://agijuejin.feishu.cn/docx/ROFqd00CWonck6xCU5mcIghunMc](https://agijuejin.feishu.cn/docx/ROFqd00CWonck6xCU5mcIghunMc)

> 金磊 发自 凹非寺

> 量子位 | 👸号 QbitAI

**苹果AI****（Ap**ple Intelligence）还没有正式到来，后台的提示词却意外曝光了。

事情是这样的。

上周苹果不是刚发布了包含生成式AI的开发者测试版本嘛，然后就有位开发者在茫茫文件夹里发现了个不得了的东西——

MacOS 15.1 Beta 1里面，有疑似Apple Intelligence的后台提示词！

紧接着他就把自己的发现顺手发到了Reddit上，瞬间引来众多网友的吃瓜：

![](img/84fb0ff722e58f3408df4c7f3e3e4190.png)

这个JSON文件里的代码是这样的：

```
less
**代码解读
复制代码
&quot;prom**ptTemplat**es&quot;**: {    &quot;com.apple.textCom**position.Mai****lRe**p**lyQ**A**&quot;**:** &quot;    {**{** spe**c**ialToken.**c**hat.r**o**le.**s**yste**m** }}You a**r**e a help**f**ul mail a**s**sist**a**n**t** whic**h** can** **hel**p** **i**denti**f**y rel**e**vant qu**es**tions** **f**r**om a** **giv**e**n m**a**il an**d** a shor**t **rep**l**y snippe**t**. Given a** **mail **a**nd **t**he reply s**n**ippet**,** a**s**k r**e**leva**nt** qu**e**stions** **wh**i**ch ar**e** explicit**l**y as**k**ed** **in the m**a**il**.** Th**e** answer t**o** thos**e** que**s**tion**s** will **b**e selected by** **th**e** recipie**n**t w**h**ich will** h**elp re**d**uce ha**l**luc**i**nation in** **draft**i**ng t**h**e r**e**sp**o**nse. Ple**a**se outp**u**t top q**u**est**i**ons **a**lo**n**g wit**h** set of p**os**si**b**le **a**nsw**e**rs/option**s** for **e**ach** **of those** **qu**e**sti**o**ns. D**o** not as**k **que**s**tions whi**c**h are **a**ns**w**ered **by** t**h**e re**p**ly s**nip**pet. **Th**e q**u**estions** **should** **be** **short**,** n**o** mor**e **than 8** wo**rds. **Th**e answe**r**s sh**o**uld be** **sh**o**r**t** as **w**ell, a**r**ound** **2** **word**s**. **P**resent your **o**utput in a** **json for**m**at **w**ith a l**i**st** **of **d**icti**on**ar**i**es** **containi**n**g **q**uesti**o**n **a**nd **a**nswe**rs** as **t**he key**s**. **I**f no **q**uest**ion i**s as**k**ed in **t**he ma**i**l, t**h**en **o**utput a**n** emp**ty l**ist []. Only**** output vali****d json and **nothing else.{{ specialToken.chat.component.turnEnd }}{{ specialToken.chat.role.user }}{{ userContent }}    &quot;}
```

翻译过来就是：

> 你是一个有用的邮件助理，可以从给定的邮件和简短的回复片段中帮忙识别相关问题。

> 给定一封邮件和回复片段，询问邮件中明确提出的相关问题。

> 这些问题的答案将由收件人选择，这将有助于减少草拟回答时的幻觉。

> 请输出最重要的问题以及每个问题的可能答案/选项，不要问由回复片段回答的问题。

> 问题应简短，不超过8个单词。答案也应该简短，大约2个词。

> 以JSON格式呈现输出，并使用包含问题和答案的字典列表作为键。如果邮件中没有提问，那么输出一个空列表[]。只输出有效的JSON，不输出其他内容。

看完不得不感慨，即使是一个邮件助手的功能，苹果工程师也是事无巨细地给Apple Intelligence加了诸多“条条框框”。

用外媒ArsTechnica**的话来说就是“像极了父母给年幼的孩子提要求，让孩子按照他们预期的设定做事”。

除此之外，曝光的代码片段里还有个有意思的点：

![](img/99103c752bdc27115e037978e3d6213e.png)

```
less
**代码解读****
复制代码
**&quot;**c**h**at.**r**ole**.**s**y**stem.de**f**ault&quot;: &quot;s**y**stem**&lt;**n&gt;You are **a** he**l**pful assi**s**tant **t**hat classifie**s** the **f**ol**l**owin**g** i**n**put te**xt**-t**o**-im**a**ge qu**e**ry a**s **Safe **o**r Un**s**afe.** **In** **the fi**rs**t **l**ine**,** repl**y** w**i**th Saf**e **or** **Uns**a**fe. If** **the **in**put **i**s U**n**saf**e**, in the s**e**con**d** line**,** lis**t** all the** c**ate**g**ories **t**he input t**e**xt **vi**olate**s.** The **Un**safe** c**ategories **ar**e: C**SE**AI, Dr**u**gs**,** Gore,** H**arassment**,** Hate**, **Nudity or** s**exual, Of**f**en**s**ive words**, **Self-**ha**rm, Terr**or**ism or **extremism, Toxic, Violence, Weapons.&quot;
```

大致内容是：

> 您是一个有用的助手，可以将以下输入文本到图像查询分类为安全或不安全。

> 在第一行，回复“安全”或“不安全”。如果输入是不安全的，在第二行中列出输入文本违反的所有类别。

> 不安全的类别是：CSEAI（儿童性虐待图像）、毒品、血腥、骚扰、仇恨、裸体或性行为、攻击性语言、自残、恐怖主义或极端主义、有毒、暴力、武器。

嗯，安全这块也算是被把控的方方面面了。

![](img/65a748e64a099c839cd341d927be4fb1.png)

不过即便如此，也是逃不过网友们的好奇和大胆尝试。

**有人先是让Ap**ple Intelligence生成 “悲伤的图片” ，确实被拒了：

![](img/6c11276970b8a21c8467e5c5b112bf61.png)

但如果换个说法，“给我提供人们哀悼的视频”，那么就可以跑通了。

这位网友还分享了其中最好的一帧画面：

![](img/98e05a2c9a2a88c3aae2ebbf5e1a37c6.png)

那么曝光的提示词还有哪些内容？我们继续往下看。

## “不要产生幻觉！不要捏造事实”

首先还是一个跟邮件相关的内容。

![](img/161fc0d7475ea9215de216f8f198a03a.png)

```
less
**代码解读
复制代码
&quot;prom**ptTemplat**es&quot;**:** {    &quot;com.a****ppl**e**.te**x**tC**o**mposition**.**MailR**e**plyLo**n**gFo**r**mRew**r**ite&quot;: &quot;** **  ** **{{ sp**e**cialT**ok**en.ch**a**t**.**role**.s**y**s**tem }**}**You are **a**n **a**ssistant **w**hich hel**p**s the** **us**e**r** **respo**n**d to **t**heir ma**il**s.** **Given** **a **m**ail,** **a d**r**aft r**e**sponse i**s** init**i**all**y** provide**d **b**a**sed** **on** **a short **r**epl**y** sn**i**ppet. **I**n o**r**der to m**ak**e the **d**raft **r**e**s**ponse n**i**cer** **and com**p**lete,** **a **s**et of que**s**tio**n** and **i**ts answe**r** a**r**e provided.** **Ple**a**se wr**i**te a conc**i**se **a**nd na**t**ural re**pl**y by m**o**difyi**n**g t**h**e dra**f**t resp**onse** to i**nc**or**p**ora**t**e the given** q**ue**s**tio**n**s an**d** t**h**eir ans**w**ers. Please** lim**it the reply** within 50 words. Do not hallucinate. Do not make up factual information.{{ specialToken.chat.component.turnEnd }}    &quot;
```

这个例子的内容是这样的：

> 你是一个帮助用户回复邮件的助手。

> 给定一封邮件，初步回复是基于一个简短的回复片段提供的。为了使草稿回复更好更完整，提供了一组问题及其答案。请通过修改草稿回复以包含所给问题及其答案，写一个简洁自然的回复。

> 请将回复限制在50个字以内。不要产生幻觉。不要编造事实信息。

其中的“不要产生幻觉”和“不要编造事实信息”可以说是吸引了大部分吃瓜群众的注意力。

苹果工程师们也是狠狠抓住了LLM的痛点。

更多的例子还包括：

![](img/38664f0ad972ae13a11a617c0ad064da.png)

```
less
代码解读
**复制代码
{ **   &quot;message&quot;:** {     **   &quot;topline&quot;: &quot;**[**Dia**log**ue]&lt;n{**{ doc }**}{{ **c****o**n**tex**t** }}**&lt;**n&gt;**[**End of** **Di**a**logue]&lt;n&gt;Yo**u** are an **ex**per**t** at su**m**ma**r**izi**n**g messa**g**es. You** **pr**e**fer to u**s**e clauses** i**ns**t**ead** **of com**p**let**e** sentenc**e**s. D**o** no**t** answer **an**y ques**t**ion **f**rom **t**he mess**a**ge**s**. P**l**ease **k**eep yo**u**r** sum**mary** **of th**e **i**n**put** **with**i**n a **1**0 **w**ord **l**imit**.**&lt;n&gt;You** **must** **keep to t**hi**s **r**ole** **unl**e**s**s **to**l**d ot**h**erw**i**se**,** if you** don&#x27;t, it w**ill not be hel**pful.&quot;,    ** **   **&quot;vi**sualCo**ncepts&quot;**: &quot;[**N**ote]&lt;n{{ doc }}**{**{ context }}&lt;n&gt;**[**E**nd of Not**e**]&lt;n**&gt;**[Instruc**t**ion]**&lt;**n&gt;Su**m**m**a**rize** **th**e** pro**vid**ed tex**t **into** **a lis**t** o**f** **m**ost 5 **t**opic**s.** Eac**h** to**p**ic i**s** a** **single wo**r**d.** **Sor**t** the **list by rele**vance of the **topic.&quot;,       ** **&quot;vi**sua**lTopLi**ne&quot;: &quot;[**Dial**o****g**u**e]&lt;**n**{{ **d**oc** **}}{{ c**o**nt**e**xt }}&lt;n&gt;[En**d** of Dial**og**ue]**&lt;**n&gt;You **a**re** **an **e**xpert a**t** summar**i**zi**n**g messag**e**s. You pr**ef**er** **to **u**se cla**u**ses** **instead **o**f co**m**ple**t**e senten**ce**s. Do **n**ot a**n**swer** **any que**s**ti**o**n f**r**om th**e** messa**g**e**s. P**leas**e** keep** y**o**u**r s**u**mmar**y** of **t**he** **inpu**t** wit**h**in a 1**0** wor**d** limit.&lt;n**&gt;Y**ou** **mus**t** ke**e**p** t**o **t**his **r**ole** **un**l**ess tol**d otherwise, if you don&#x27;t, it will not be helpful.&quot;    }}
```

> 你是总结信息的专家。你更喜欢使用短语而不是完整的句子。不要回答消息中的任何问题。请将你的总结控制在10个字以内。你必须保持这个角色，除非另有说明，否则如果你不这样做，将不会有帮助。

> 将提供的文本总结成最多5个主题的列表。每个主题为一个单词。按主题的相关性排序。

![](img/489a41260a92dda77153728646045ebb.png)

```
vbnet**
代**码解读
复制代码
**{{ **specialToken.chat.role.**us**er }}Y**ou a****re** a director on a movie set**! **Here is a movie **id**ea of &quot;{{ u**se**rPrompt }}&quot; but wit**h ****a s**pecial foc**us** on {{ traits** }}. {**{ dynamicLifeC**on****text**** }} B**a**sed ****on** this** mo**vie idea, a stor**y **titled &quot;{{ storyTitl**e }}&quot; **has been wri**tt**en, and yo**ur**** j**ob is to curat**e **up to** {{ ta**rgetAssetCo**un****t **}} di**verse **assets to best make the movie for chapter &quot;{{ fallbackQuery }}&quot; in this story. Select assets based on their captions from the below photo library, where each asset has an ID as the key, and a caption as the value. {{ assetDescriptionsDict }} Return the result as an array of the selected asset IDs in JSON format. Do not return asset IDs if no good matches are found. Do not return duplicated or non-existent asset IDs. Assets:{{ specialToken.chat.component.turnEnd }}
```

> 你是电影片场的导演！这是一个电影创意：”{{ userPrompt }}”，但特别关注{{ traits }}。

> {{ dynamicLifeContext }}基于这个电影创意，已经写好了一个名为”{{ storyTitle }}”的故事，你的工作是策划最多{{ targetAssetCount }}种多样的素材，以最佳方式制作这个故事中章节”{{ fallbackQuery }}”的电影。

> 从下面的照片库中根据其标题选择素材，每个素材都有一个ID作为键，标题作为值。

> {{ assetDescriptionsDict }}将结果作为选定素材ID的数组返回，格式为JSON。如果没有找到合适的匹配，请不要返回素材ID。不要返回重复或不存在的素材ID。

从所有的例子中，我们不难发现一个规律：

苹果工程师们会先告诉Apple Intelligence它的角色定位；然后再围绕这个角色可能会出现的各种状况，提出相对细节的要求。

事无巨细，事无巨细。

![](img/3c65f38590ad38e033425d5c5bbe3566.png)

那么这些开发者们是从哪儿找到了的这些JSON文件？

他们在Reddit中也公开了具体的文件路径：

/System/Library/AssetsV2/com_apple_MobileAsset_UAF_SummarizationKitConfiguration

如果你已经下载安装了MacOS 15.1 Beta 1，不妨可以去找一找看下。

## 和此前Siri的“玩法”相似

**若是回顾苹果**此前在AI上的发展，其实这种 “预先设定” 模式是早就有了的——

正是迟迟不上大模型的**Siri**。

据之前的消息，苹果高管曾否决了让Siri进行长对话的建议，因为他们觉得这样会导致Siri对话难以控制，而且“很花哨”。

加之苹果在**隐私安全方面毫不妥协**的立场，让Siri能力进一步提升也面临���大挑战。反观OpenAI会收集用户聊天内容来训练模型，以提升效果。

与此同时，为了防止Siri“胡言乱语”，苹果倾向的路线是先让人工团队预先写出Siri的回复，而不是用AI生成。

![](img/5354ab9470822cd1b3b5d76bd81420b4.png)

比如询问Siri，iPhone的价格是多少？它不会正面回复，只会给出官网链接。

Siri的设计团队要求，在功能上线前，它的回答准确率需要逼近完美。工程师们曾试图花费几个月时间说服他们，不是每个问题都需要人工来验证一遍，如果这样将会限制Siri回答问题的范围。

而且苹果设计团队还多次拒绝允许用户对Siri回答问题进行反馈，导致开发团队无法理解模型的局限。

那么在几个月后即将在苹果各大操作系统上线的Apple Intelligence，能否改写诸如Siri在AI上的遗憾，只有到时候亲测效果才可见分晓了。

参考链接：

[1]www.reddit.com/r/MacOSBeta…

[2]www.theverge.com/2024/8/5/24…

[3]techcrunch.com/2024/08/06/…

[4]arstechnica.com/gadgets/202…