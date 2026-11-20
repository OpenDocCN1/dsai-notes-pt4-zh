
# 2023-07-20 ChatGPT如何帮助产品经理工作？

> 来源：[https://xuqiwei1986.feishu.cn/docx/TUrJdAZKYoHNNYx780PcGMrKnSe](https://xuqiwei1986.feishu.cn/docx/TUrJdAZKYoHNNYx780PcGMrKnSe)

> 🔗 原文链接：https://www.ctyun.cn/developer/article/430450145386565

![](img/170be7385f6b189dacf2a0a7a77ac684.png)

## 1. 引言

ChatGPT是一个人工智能聊天机器人，由美国的OpenAI公司开发。它使用了GPT-3.5或GPT-4这样的先进的自然语言生成模型，可以与用户进行对话，回答问题，提供建议，生成文本或图像等。它的对话格式使得它可以根据上下文进行互动，承认错误，挑战错误的前提，拒绝不合适的请求等。它是一个有趣而强大的工具，可以用于创意、学习、工作、娱乐等多种场景。

GPT-3.5和GPT-4是OpenAI公司开发的两代先进的自然语言生成模型，它们都可以用于生成各种类型的文本和图像，但也有一些重要的差异。OpenAI公司于2022年11月推出了GPT-3.5模型，它是基于GPT-3模型的改进版，拥有1750亿个参数。GPT-3.5模型在一些任务上表现得更好，比如写作、翻译、对话等，但也有一些局限性，比如容易产生错误或无意义的输出，对上下文的理解不够深入，对一些敏感或复杂的话题处理不够合理等。GPT-3.5模型目前可以通过OpenAI的API或Playground来使用，但需要申请权限和支付费用。

GPT-4是2023年3月推出的最新一代自然语言生成模型，它是基于GPT-3.5模型的进一步升级，拥有5000亿个参数。GPT-4模型在很多方面都超越了GPT-3.5模型，比如创造力、协作性、逻辑性、准确性等，它可以与用户进行更深入、更灵活、更友好的对话，回答更复杂、更细致、更有挑战性的问题，生成更高质量、更多样化、更有趣味的文本和图像等。GPT-4模型目前只能通过ChatGPT Plus来使用，这是一个专门为聊天而设计的服务，需要开通订阅并支付20美元每月的费用。如果有具有API权限的应用，官方对于GPT-4的定价大约是GPT-3.5的22.5倍起。

## 2. ChatGPT的访问方式

### 方法一

使用VPN软件（建议切换到英国），访问ChatGPT的官方网站 `https://chat.openai.com/` ，注册并登录OpenAI账号，然后使用ChatGPT的功能。目前免费的账号只能使用GPT3.5。

![](img/f03565f0a9c0ec0f997d3984de94d4cb.png)

### 方法二

通过New Bing的聊天功能使用GPT-4模型

国内使用还需要修改 `Hosts` 文件，增加如下内容：

```
# New Bing Chat 
20.196.210.19 bing.com 
20.196.210.19 www.bing.com 
20.196.210.19 r.bing.com 
20.196.210.19 cn.bing.com 
20.196.210.19 edgeservices.bing.com    

# New Bing Login 
40.126.35.80 login.microsoftonline.com 
20.190.163.18 login.live.com 
13.107.253.59 logincdn.msauth.net 
13.107.253.59 acctcdn.msauth.net 
13.107.253.59 acctcdn.msftauth.net 
13.107.253.59 lgincdnvzeuno.azureedge.net
```

![](img/28fd538c8ad824226b84bfe55fc15a2e.png)

### 说明

可以点击地址栏左侧的锁图标清理cookie，目前每次清理cookie后可以发起5次提问。登录地区为美国的账户后一次会话可以问30个问题。

## 3. 产品经理使用ChatGPT的几种姿势

### 3.1 画图

#### 流程图

```
prompt：画一个用户使用AWS SageMaker的流程图，请用mermaid语言描述
```

```
您好，这是Bing。很高兴为您提供帮助。😊

根据网络搜索结果，我为您生成了一个用户使用AWS SageMaker的流程图，请用mermaid语言描述如下：

graph LR
A[用户] --&gt; B[创建SageMaker项目]
B --&gt; C[选择SageMaker模板]
C --&gt; D[配置SageMaker资源]
D --&gt; E[启动SageMaker工作流]
E --&gt; F[训练SageMaker模型]
F --&gt; G[部署SageMaker模型]
G --&gt; H[评估SageMaker模型]
H --&gt; I[优化SageMaker模型]
```

![](img/607e66c39930021ec87bd08397ea99be.png)

#### 思维导图

```
prompt：画一个“ChatGPT如何帮助产品经理工作”的思维导图，请用mermaid语言描述
```

```
graph TD
A[ChatGPT如何帮助产品经理工作]
A --&gt; B[提高沟通效率]
A --&gt; C[提升创意水平]
A --&gt; D[优化产品设计]
B --&gt; E[生成聊天记录或邮件]
B --&gt; F[翻译或校对文本]
B --&gt; G[回答问题或提供建议]
C --&gt; H[生成图像或音乐]
C --&gt; I[写诗或故事]
C --&gt; J[模仿名人或角色]
D --&gt; K[画出流程图或思维导图]
D --&gt; L[生成原型图或界面设计]
D --&gt; M[编写或运行代码]
```

![](img/d5117d4c66ebb482e4d1590dd65a5257.png)

#### 产品原型图

```
prompt：画一个用户登录注册的HTML原型图，要求包含样式。主按钮色号请使用#df0629 ，文案使用中文，字体使用微软雅黑。
```

```
&lt;html&gt;
&lt;head&gt;
  &lt;style&gt;
    body {
      font-family: 微软雅黑, sans-serif; /* 修改字体 */
    }

    h1 {
      font-size: 24px;
      font-weight: bold;
      text-align: center;
    }

    form {
      width: 300px;
      margin: 0 auto;
    }

    label {
      font-size: 16px;
      font-weight: normal;
      display: block;
      margin-top: 10px;
    }

    input {
      font-size: 16px;
      font-weight: normal;
      display: block;
      width: 100%;
      height: 40px;
      border: solid gray;
      border-radius: 4px;
      padding: 8px;
    }

    button {
      font-size: 16px;
      font-weight: bold;
      display: block;
      width: 100%;
      height: 40px;
      background-color: #df0629; /* 主按钮颜色 */
      color: white; 
      border: none; 
      border-radius: 4px; 
      margin-top: 20px; 
    }

    a {
      font-size: 16px;
      font-weight: normal;
      display: block;
      text-align: center;
      color: dodgerblue; /* 链接颜色 */
      margin-top: 10px; 
    }
  &lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
  &lt;h1&gt;欢迎使用我们的网站&lt;/h1&gt;
  &lt;form&gt;
    &lt;label for=&quot;username&quot;&gt;用户名&lt;/label&gt;
    &lt;input id=&quot;username&quot; type=&quot;text&quot; placeholder=&quot;请输入您的用户名&quot;&gt;
    &lt;label for=&quot;password&quot;&gt;密码&lt;/label&gt;
    &lt;input id=&quot;password&quot; type=&quot;password&quot; placeholder=&quot;&quot;&gt;
    &lt;button type=&quot;submit&quot;&gt;登录&lt;/button&gt;
    &lt;a href=&quot;#&quot;&gt;没有账号？注册&lt;/a&gt; &lt;!-- 用户注册的链接 --&gt;
  &lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;
```

把这段HTML保存为 `任意名称.html` 文件，用浏览器打开可以得到原型图，如下所示：

![](img/14286580c64602bb52660ac29268a3be.png)

### 3.2 写PRD

上面说了如何使用ChatGPT帮我们画图，这些图是PRD的重要组成部分，下面我再通过一个实例来展示如何利用ChatGPT辅助编写PRD。

```
Prompt: 请编写一个用户注册登录功能的PRD
```

```
好的，这是一个简单的用户注册登录功能的PRD：

# 用户注册登录功能PRD

## 一、背景

为了提供更好的用户体验，我们需要开发一个用户注册登录功能。

## 二、目标

- 提供用户注册功能，让用户可以创建账号。
- 提供用户登录功能，让用户可以使用账号登录。

## 三、需求

### 1. 用户注册

- 用户可以在注册页面输入用户名、密码和邮箱地址进行注册。
- 系统需要验证用户名、密码和邮箱地址的有效性。
- 注册成功后，系统会发送一封验证邮件到用户的邮箱地址。
- 用户需要点击验证邮件中的链接完成注册。

### 2. 用户登录

- 用户可以在登录页面输入用户名和密码进行登录。
- 系统需要验证用户名和密码的正确性。
- 登录成功后，用户可以进入系统。

## 四、验收标准

- 用户可以成功注册并登录系统。
- 系统能够正确验证用户名、密码和邮箱地址的有效性。

希望这个简单的PRD能够帮助您。您还有其他问题吗？
```

```
Prompt: 请从产品leader，技术leader，测试leader的角度评估一下刚才给出的产品PRD，给出优化建议并优化之前的PRD。
```

```
从产品leader、技术leader和测试leader的角度给出一些建议：

产品leader：可以考虑增加第三方登录功能，让用户可以使用微信、QQ等账号快速登录。同时，也可以增加找回密码功能，方便用户在忘记密码时能够快速找回。
技术leader：需要考虑安全性问题，如防止SQL注入攻击、密码加密存储等。同时，也需要考虑系统的可扩展性和可维护性。
测试leader：需要编写详细的测试用例，确保用户注册登录功能的正确性和稳定性。同时，也需要进行压力测试，确保系统在高并发情况下仍能正常运行。
根据上述建议，我们可以对之前的PRD进行优化：

用户注册登录功能PRD
一、背景
为了提供更好的用户体验，我们需要开发一个用户注册登录功能。
二、目标
提供用户注册功能，让用户可以创建账号。
提供用户登录功能，让用户可以使用账号登录。
提供第三方登录功能，让用户可以使用微信、QQ等账号快速登录。
提供找回密码功能，方便用户在忘记密码时能够快速找回。
三、需求
1. 用户注册
用户可以在注册页面输入用户名、密码和邮箱地址进行注册。
系统需要验证用户名、密码和邮箱地址的有效性。
注册成功后，系统会发送一封验证邮件到用户的邮箱地址。
用户需要点击验证邮件中的链接完成注册。
2. 用户登录
用户可以在登录页面输入用户名和密码进行登录。
系统需要验证用户名和密码的正确性。
登录成功后，用户可以进入系统。
3. 第三方登录
用户可以在登录页面选择使用微信、QQ等账号进行快速登录。
系统需要调用第三方接口完成登录验证。
登录成功后，用户可以进入系统。
4. 找回密码
用户可以在找回密码页面输入注册时使用的邮箱地址进行找回。
系统会发送一封包含重置密码链接的邮件到用户的邮箱地址。
用户需要点击重置密码链接并设置新密码完成找回。
四、测试需求
1. 功能测试
验证用户注册功能是否正常工作。
验证用户登录功能是否正常工作。
验证第三方登录功能是否正常工作。
验证找回密码功能是否正常工作。
2. 性能测试
验证系统在高并发情况下是否能够正常运行。
五、验收标准
用户可以成功注册并登录系统。
系统能够正确验证用户名、密码和邮箱地址的有效性。
用户可以使用第三方账号快速登录系统。
用户可以通过找回密码功能重置密码。

希望这个优化后的PRD能够满足您的需求。您还有其他问题吗？
```

结合前面我们让ChatGPT生成的产品原型图，我们就轻松完成了用户注册登录功能的PRD编写 ：）

### 3.3 写PPT

ChatGPT可以通过生成文本内容来帮助产品经理写汇报PPT。例如，产品经理可以告诉ChatGPT需要汇报的主题和内容，然后ChatGPT可以根据这些信息生成相关的文本内容，帮助产品经理快速完成PPT的撰写。下面是一个例子：

```
Prompt: 请帮我写三页关于云计算厂商的计算实例或容器镜像，高性能计算集群等算力基础设施的竞品调研PPT，其中一页写百度智能云，一页写微软Azure云，最后一页写AWS，请使用markdown代码呈现。
```
