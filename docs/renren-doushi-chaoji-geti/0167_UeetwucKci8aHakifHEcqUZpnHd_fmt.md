
# 一句话让 AI 自动生成短视频：用 `n8n` 打造 YouTube 自动发布系统

> 来源：[https://xuqiwei1986.feishu.cn/docx/HSIvdNunjoiqT7xDNiUcVAXgndC](https://xuqiwei1986.feishu.cn/docx/HSIvdNunjoiqT7xDNiUcVAXgndC)

> 不再手动上传，只要输入创意，`n8n` 帮你自动生成和发布视频。

#AI #自动化 #`n8n` #YouTube #Sora2 #Claude

如果你也曾想过：“能不能用 AI 自动生成视频，然后直接发布到 YouTube？”——这篇文章就是为你写的。

最近，我用 `n8n`、Claude 和 Sora-2 搭建了一个完整的自动化工作流：从输入一个创意开始（手动），到生成视频、添加标题描述，再自动上传到 YouTube，全程无人值守。

本文会完整讲解整个系统的实现过程、技术细节、常见坑点，以及它的实际应用价值。

---

## 一、为什么要做这个自动化？

AI 视频生成越来越普及，但手动发布依然耗时：

+ 每次都要想标题、写描述、加标签。
+ YouTube 上传页面填写信息多，流程繁琐。
+ 想批量测试不同风格的视频，效率低。

最近Sora-2不是很火嘛，于是我想，如果这些操作能交给自动化工具处理，那就能把精力集中在创意本身。

于是，便有了这个项目：用 `n8n` 搭建一个自动生成并发布视频的工作流。

目标很明确：

1. 自动生成视频内容（通过 Claude + Sora-2）
2. 自动上传到 YouTube（通过 API）
3. 减少重复操作，实现“输入一句话 ➡️ 自动产出视频 ➡️ 自动发布YouTube”

---

## 二、工作流总体设计

整个系统基于 `n8n` 实现，核心流程如下：

```
💬 聊天输入界面
    ↓
🤖 AI生成内容（Claude）
    ↓  
📝 解析AI响应
    ↓
🎬 生成视频（Sora-2）
    ↓
⏳ 检查生成状态 ──未完成→ ⏰ 等待重试 ──┐
    ↓完成                              │
📥 下载视频文件 ←───────────────────────┘
    ↓
📤 上传到YouTube
```

最终工作流流程图：

![](img/dcf3ab1507d2056dfb2a6d5119170b5a.png)

实现效果：

+ 输入一句话，例如 “一只金渐层猫咪在追着红色激光点上蹿下跳”。
+ 使用`https://api.aicso.top/` 接口平台的Claude模型生成提示词、标题、描述、标签。
+ 使用`https://api.aicso.top/` 接口平台的Sora-2模型生成视频。
+ 自动上传到 YouTube，设置标题、文案、区域、标签。

只要输入一句话，就会自动生成视频和发布。

实际耗时：

+ 手动操作：15-30分钟（同使用Sora-2制作视频和下载视频）
+ 自动流程：输入创意后不到 6 分钟（等待视频生成约 2-5 分钟）

单条视频成本：

单条视频成本约 ¥0.0595~0.096 元：

+ Claude API：¥0.01~0.03
+ Sora-2 API(限时特价)：¥0.05~0.066
+ `n8n`：免费（Github注册满180天用户，ClawCloud每月赠送5美刀，一个月的费用都没有5刀，基本够了）
+ YouTube API：免费

一条视频的费用约等于无了，可以尽情造。

---

## 三、具体搭建步骤

### 3.1、第一步：准备环境

所需工具和账号：

1. `n8n`（推荐 ClawCloud，有公网访问）
2. Claude/Sora-2 API 密钥（在`https://api.aicso.top/` 创建令牌，claude和Sora-2可以创建不同的分组，claude创建默认分组，Sora-2创建限时特价分组，¥0.05一个10秒视频）
3. Google Cloud 项目（启用 YouTube Data API v3，这里不会细讲创建项目，主要讲权限配置和应用发布）
4. YouTube 账户（绑定 OAuth2）

> ⚠️ 注意：YouTube OAuth 认证不支持 `localhost`。必须部署在有公网域名的服务器上，或者使用`ngrok`、`frp`等反向代理，本文主要是讲解在ClawCloud部署的`n8n`。

推荐部署方式：

+ ClawCloud 部署的`n8n`：GitHub 180天及以上的用户每月有5刀的额度，每天0.14刀，一个月0.14*30=4.2够用了。
+ 海外云服务器 `npmjs` 部署：需绑定域名并配置 SSL。

### 3.2、第二步：创建工作流

#### 3.2.1、创建新工作流

1、打开 `n8n` 控制台 → 新建工作流「Create Workflow」：

![](img/7e79ad5d18369479e97e92fa7cf9c81f.png)

2、在顶部给工作流命名，例如「YouTube视频自动发布2」，因为我已经存在了，所以加了个2区分：

![](img/bfd0f095417fd7befc0086642a5d76b7.png)

#### 3.2.2、添加节点1：`Chat Trigger`（聊天触发器）

1、点击「`Add first step...`」，在弹出窗口输入选择「`On chat message`」：

![](img/3280af0e24b46f9da7e594b008dadcd7.png)

2、启用「`Make Chat Publicly Available`」，在「`Initial Message(s)`」输入「请输入要生成的视频主题、内容~」：

![](img/9a2cd63bc124836be49c7129f8db5dbe.png)

#### 3.2.3、添加节点2：AI生成视频信息（`HTTP Request`）

1、点击「`When chat message received`」 节点右侧的「+」，搜索「`HTTP Request`」并点击，修改名称为「AI生成视频信息」：

![](img/2beb7cb0dcb156d305756834ba5e41d6.png)

![](img/c24861b4e2462b82fa268659718738ab.png)

2、配置节点：

`Method`：`POST`

`URL`：`https://api.aicso.top/v1/chat/completions`

`Authentication`：`None`（我们手动在 `Headers` 中配置）

`Headers`：

+ `Name`: `Authorization`

+ `Value`: `Bearer 你的API密钥`（注意 `Bearer` 后有空格）

+ 再添加一个(可以不加)：`Name`: `Content-Type`，`Value`: `application/json`

![](img/9c2d5ac74727c425d2320576368f829f.png)

`Body`：

```

{
  "model": "claude-sonnet-4-5-20250929",
  "messages": [
    {
      "role": "system",
      "content": "你是一个专业的视频创作助手。根据用户输入，生成适合YouTube Shorts的视频提示词、标题、描述和话题，输出内容必须是英文，简洁直白。输出格式必须是JSON: {\"prompt\":\"视频提示词\",\"title\":\"标题含#话题\",\"content\":\"内容含#话题\",\"tags\":\"#话题\"}"
    },
    {
      "role": "user",
      "content": "{{ $json.chatInput.trim() }}"
    }
  ],
  "temperature": 0.7
}

```

这个提示词只是测试生成的，需要自己优化调整为更好的。

![](img/c04ccf9b696da61df0ebb023ff993d1d.png)

#### 3.2.4、添加节点3：解析AI响应（`Code`）

1、点击上一个节点右侧的「+」，搜索「`Code`」，点击「`Code`」，选择「`Code in JavaScript`」：

![](img/b5a6f0a741404fc9518729637dfc06a4.png)

![](img/4544922cd917a9265e04d7920ff0e79d.png)

2、设置节点名称为「解析AI响应」：

![](img/6cf8889ce4120ef3ba855f7eacfd9fb6.png)

3、添加下面代码：

```

// 解析AI返回的JSON
const aiResponse = $input.item.json;
const content = JSON.parse(aiResponse.choices[0].message.content);

return {
  json: {
    prompt: content.prompt,
    title: content.title,
    content: content.content    ,
    tags: content.tags
  }
};

```

新代码(增加了代码块的处理-2025年11月9日)：

```

// 1. 从AI响应中获取原始的字符串内容
const aiResponse = $input.item.json;
const rawContent = aiResponse.choices[0].message.content;

// 2. 清理字符串，移除可能存在的Markdown代码标记
const cleanedContent = rawContent.replaceAll("```json", '').replaceAll("```", '');

// 3. 解析清理后的字符串为JSON对象
const content = JSON.parse(cleanedContent);

// 4. 返回结构化的数据
return {
  json: {
    prompt: content.prompt,
    title: content.title,
    content: content.content,
    tags: content.tags
  }
};

```

![](img/81e0346f5023df850f912afa42d13dcb.png)

#### 3.2.5、添加节点4：生成视频（`Sora-2`）

1、点击上一个节点右侧的「+」，搜索「`http`」，选择「`HTTP Request`」节点，修改节点名称为「生成视频（`Sora-2`）」：

![](img/bd08dbc2b21c5d3525d014d23f69fb43.png)

![](img/04b156fa2e150dccd33b4f85754da2f2.png)

2、配置节点：

`Method`：`POST`

`URL`：`https://api.aicso.top/v1/chat/completions`

`Authentication`：`None`

`Headers`：

+ `Name`: `Authorization`，`Value`: `Bearer 你的API密钥`

+ `Name`: `Content-Type`，`Value`: `application/json` 可不配置

![](img/ffdf0fce79c7ca01ebdcffb159ff8650.png)

`Body`（JSON）：

```

{

  "model": "sora-2-portrait",

  "messages": [

    {

      "role": "user",

      "content": [

        {

          "type": "text",

          "text": "{{ $json.prompt }}"

        }

      ]

    }

  ]

}

```

`Options` → `Timeout`：`600000`（10分钟）

![](img/b79985687f36e50cf624a1da1e9852c3.png)

#### 3.2.6、添加节点5：检查视频状态（`IF`）

1、添加节点，点击上一个节点右侧的「+」，搜索「`IF`」，选择「`IF`」节点，修改节点名称为「检查视频状态」：

![](img/4561eddf7fb1244a3c24cd3d1672e776.png)

![](img/d27a3c95140f075544444decb72b5be3.png)

2、配置节点：

`Conditions`：

+ `Value 1`: `{{ $json.choices[0].message.content }}`

+ `Operation`: `contains`

+ `Value 2`: `[download video](https://`

![](img/464a920fa3f41546e518863c1664ba6d.png)

这个节点会有两个输出：

+ `true`：视频生成完成，连接到下一个 `HTTP Request` 节点下载视频

+ `false`：视频未完成，连接到 `Wait` 节点等待，`Wait` 节点又连接到「生成视频（`Sora-2`）」节点，其实只要「生成视频（`Sora-2`）」开启了重试就可以不用这个节点了。

#### 3.2.7、添加节点6：`Wait`（等待节点）

1、添加节点，点击 `IF` 节点下方的 `false` 输出端，搜索「`Wait`」，选择「`Wait`」节点：

![](img/010e083957e04e085697e39773ee38f2.png)

![](img/ed73231677d10fa38a120b7ba4e0716d.png)

节点使用默认配置即可，然后点击 `Wait` 节点右侧的「+」,连接到「生成视频（`Sora-2`）」节点前面形成循环。

#### 3.2.8、添加节点7：下载视频（`HTTP Request`）

1、添加节点，点击 `IF` 节点的 `true` 输出端，搜索「`http`」，选择「`HTTP Request`」节点，修改节点名称为「下载视频」：

![](img/d55de747f9331ca7f8b83933a84b1eae.png)

![](img/7411c74ca31c7c3bb22044a2fa290147.png)

2、配置节点：

`Method`：`GET`

`URL`：使用表达式提取视频链接

```

{{ $json.choices[0].message.content.match("(https://.*mp4)")[0] }}

```



### 3.2.8 配置输出字段

**Response Format**：File

Put Output in Field：`videoData`（视频数据存储的属性名）

![](img/c44245d9ebbd4823de894b521c097896.png)

### 3.2.9 添加节点8：上传到YouTube

1、添加节点，点击上一个节点右侧的「+」，搜索「YouTube」，点击「YouTube」，选择`Upload a video`节点，修改节点名称为「上传到YouTube」：

![](img/f27257a6739a8d30a180b6a1a4440d77.png)

![](img/28b846c83c1a936118e1a919dfff429b.png)

![](img/309811f62bc24e265dfcf6180d7b5678.png)

2、配置节点：

`Credential to connect with`：待配置（下一步说明）

`Resource`：Video

`Operation`：Upload

`Title`：`{{ $('解析AI响应').item.json.title }}`

`Category ID`：1（Film & Animation）

`Description`：`{{ $('解析AI响应').item.json.content }}`

`Tags`：`{{ $('解析AI响应').item.json.tags }}`

`Region Code`：United States of America(the)-US

`Input Binary Field`：`videoData`

![](img/a77b80727ade93c91ed1b48071dd2bda.png)

![](img/7e3cde831bd6867006cf49ca35188512.png)

![](img/0ce500e6b4a6080ffe7cb9cce03d3bac.png)

设置好后我们就开始YouTube凭证配置这个坑点了。

### 3.3 第三步：配置密钥与凭证

#### 3.3.1 Claude / Sora-2 API

1、访问「https://api.aicso.top/console/token」创建令牌，如未注册，可以点我的邀请链接注册「https://api.aicso.top/register?aff=yzLp」，点击添加令牌创建2个令牌，Claude模型的是`default`默认分组的，Sora2模型的是「限时特价」分组的：

![](img/6394cf6197e173e8c40664d5a75973f5.png)

![](img/11f9120f3ea1128260d82868281005d1.png)

![](img/4fc101530ea9c1a94ea4219f6776e787.png)

创建好后复制令牌粘贴到对应节点的「Authorization」参数值的`Bearer`后面即可，`Bearer`右边记得要加个空格。

#### 3.3.2 YouTube OAuth2 认证

**⚠️ 重要提示**：YouTube 凭证配置是整个流程中最容易踩坑的环节，请仔细阅读！

根据 [N8N 官方文档](https://docs.n8n.io/integrations/builtin/credentials/google/)，YouTube 节点：

+ ✅ 支持 OAuth2 认证
+ ❌ 不支持 `Service Account`（服务账号）

1、在 Google Cloud Console「https://console.cloud.google.com/」 创建新项目或选择现有项目：

![](img/53301524cecf603a85da32ec8a75958f.png)

![](img/9dcd9d9a6bc7e19da0ab8e04816ca313.png)

2、点击「API和服务」，在新页面点击「库」：

![](img/5d15d3d620e8b41f656c5949b0157790.png)

![](img/2e5c7588464805b1f3b143ef18e05664.png)

3、在「API库」页面输入「YouTube」搜索，找到「YouTube Data API v3」并启用：

![](img/ba655b6ada3a723eed08cbdb66642f95.png)

![](img/a5cfb3eccc827f8532233ff4f3d3f633.png)

![](img/a49a6affc56df8f12b4f3959dc920cd3.png)

![](img/5996aca084c640b2d1fee63148f95042.png)

4、点击「凭证➡️创建凭证」，选择「OAuth 客户端 ID」：

![](img/e44a5b9c8c94ac4cb7da6389a07b79f2.png)

![](img/feb8949be0fcba5a584969d4e585a932.png)

![](img/37f5a436a74f8cbf9a67777de6adaacc.png)

![](img/6a90490f9045452d858b186b8da00a46.png)

![](img/e073a145997098a97ac8788862400965.png)

![](img/333efb725f7ebad57fc3eb5c4ea1822a.png)

![](img/5736ba5efa9a13bee2f3a10097eed1d6.png)

![](img/81b2064e93872ff1b9d05ddc4a17115a.png)

![](img/2cda0304f948746486ad6ee8269c49d8.png)

2、创建「OAUTH客户端」：

![](img/a705187851eb5a58be801a36f268f6ef.png)

需要在「上传到YouTube」节点的`Credential to connect with`处创建凭证，如果有，点击右边的笔图标编辑也可：

![](img/e8839064962e4ba153dc8e5acecc5915.png)

![](img/78bf18a816c677fa78672d5ff3214eab.png)

复制`OAuth Redirect URL`，返回「创建 OAuth 客户端 ID」，在`已获授权的重定向 URI`输入框输入复制的`OAuth Redirect URL`：

![](img/dc25706951471d1d0556cb64c1e47198.png)

![](img/14189f12ae833d91785121c6c7673c44.png)

复制自己的`客户端 ID`和`客户端密钥`填写到n8n的YouTube凭证输入框：

![](img/4cb4247444711b1faadae29a54547ff3.png)

填好后先不要点击`Sign in with Google`，因为我们还没发布应用。

![](img/7e81a6f833b21237dee5c8ef25fd1df6.png)

![](img/e425afa74e228e6c546d55764feafd30.png)

没发布应用就会出现`403 access denied`错误，这是一个隐藏的坑点，我在这里琢磨了好几个小时😭。

点击「目标对象」，然后点击页面的发布应用：

![](img/dac84ed6fa2dab6651e404a797eb7b19.png)

![](img/b9a81a5afaa4fabe0f9337ef3f0eb493.png)

![](img/73172dd42b99e00c2b71e2c72937dbfe.png)

返回n8n点击`Sign in with Google`：

![](img/d9162165d794a4a1c64812e803d694c5.png)

![](img/5f5c18eb72f544f8b5f05d6b6af44197.png)

![](img/ac88b8172039d8f43d9fe11dd42b12da.png)

![](img/a86db1c83d05d7c035d9d7d5d77afe3d.png)

![](img/9c479b98c8a0efb1db8576434acf8433.png)

![](img/0474295141d31ea1a9567cc2bccb8236.png)

![](img/a0acd3108531b9571d1dfcb37298976b.png)

因为我解析了域名，所以要修改地址为我解析的域名再访问：

![](img/0bb02e5edea52914766475b0216986c2.png)

返回YouTube凭证设置：

![](img/1e29eedb074fa3be5fde4003c66ec17a.png)

账号连接成功。

### 3.4 第四步：测试运行

1、在工作流左下角的`Chat`窗口输入视频主题，记得要修改`AI生成视频信息`、`生成视频（Sora-2）`两个节点的令牌哦：

![](img/c283bda386b0dc8f5370ef59690b5065.png)

执行完成没报错👍。

2、检查 YouTube 频道是否上传成功：

![](img/9e199a7aca74820a65d5ba103e00c35d.png)

发布的视频播放数有高有低高的有1000+(之前用Sora2生成的视频手动发布的有破2w播放)，低的也有0，

---

## 四、常见问题与坑点

**1、问题：`OAuth Redirect URL` 不能使用 `localhost` ⚠️ 高频坑点**

+ **现象**：点击`Sign in with Google`后授权失败
+ **原因**：Google 不允许 `localhost` 作为 OAuth 回调地址，会出现地址无法访问的情况
+ **解决方案**：
  + 方案1：使用公网域名部署 n8n（推荐）
  + 我使用的是 `clawcloud` 部署 + 域名解析
  + 也可以用其他云服务商的海外服务器部署（阿里云、腾讯云等）
  + 方案2：使用反向代理工具
  + `ngrok`、`frp`等工具将本地 n8n 暴露到公网（临时方案）

2、问题：Google 应用未发布导致授权失败（⚠️ 重要坑点）

+ **现象**：授权时提示权限不足或无法访问
+ **原因**：应用处于测试状态，未发布
+ **解决**：访问 https://console.cloud.google.com/auth/audience?project=你的项目名称 发布应用

如果不发布应用，会导致授权失败或权限不足

3、问题：API 返回 401 错误

+ 检查 API 密钥是否正确
+ 确认格式为 `Bearer` 密钥（注意空格）

4、问题：YouTube 上传失败

+ 重新授权 YouTube 凭证
+ 确认 Google Cloud 项目已启用 YouTube Data API v3
+ 检查 OAuth 重定向 URI 是否正确
+ 确认使用的是 OAuth2（不是 `Service Account`服务账号）

**5、问题：视频生成超时**

+ 增加`生成视频(Sora-2)`节点的 `timeout` 值
+ 当前设置为 `600000ms`（10分钟），可根据需要调整，如调整为`900秒`
+ 增加重试机制

---

## 五、项目总结

### 5.1 值得做这个项目吗？

如果你的目标是 **学习技术** ✅ 值得做。

+ 这是一个非常实用的 `n8n` 自动化入门项目，可以让我们快速理解工作流编排的核心逻辑。
+ 可以学会并掌握 API 集成、AI 内容生成、自动化上传 等实用技能。
+ 同时还能体验 Claude、Sora-2 等AI工具的协作方式，了解完整的 AI 工具链工作原理。

如果你的目标是 **通过它赚钱** ❌ 不建议。

+ **AI 生成内容流量不稳定**，平台更喜欢原创、有质量的内容。
+ **投入产出比偏低**：生成容易，吸引用户难。
+ **原创性有限**，大量发布甚至可能被算法判定为“低原创内容”。

AI 生成的视频原创性有限，发多了可能会被平台检测出来。真正能长久运营的账号，仍然离不开内容质量。

所以，把这个项目当成技术实践与效率工具，而不是流量机器。

它能让你少花时间在机械操作上，多花时间在创意上。

### 5.2 我的真实感受

**收获：**

+ ✅ 掌握了多项新技术（n8n、API、AI 视频生成）
+ ✅ 明显减少了重复劳动，提高创作效率
+ ✅ 能快速验证不同视频创意和风格
+ ✅ 整个搭建过程挺有意思的

**局限：**

+ ⚠️ 视频质量与播放表现不稳定，依赖提示词质量
+ ⚠️ 成本虽然不高，但积累起来也不少
+ ⚠️ AI 生成的内容缺乏个性和深度
+ ⚠️ 不适合大规模或商业化内容生产

### 5.3 给想尝试的朋友的建议

**1、把它当作学习项目**，不要期望太高的收益

**2、先解决部署问题**：

+ 新手建议用 `n8n Cloud`，也可以用 `ClawCloud` 部署，避免折腾部署
+ 自托管的话，必须配好公网域名和 SSL
+ `localhost` 是无法完成 YouTube OAuth 授权的

3、YouTube 凭证配置要仔细：

+ 这是最容易卡住的地方
+ 一定要发布 Google 应用
+ `OAuth Redirect URL` 要填对

**4、先小规模测试**，生成几个视频看看效果

**5、关注学习过程**，而不是结果

6、如果想认真做 YouTube，还是要做原创内容

如果你也在探索类似的自动化方案，或者有什么好的想法和建议，欢迎在评论区交流！
