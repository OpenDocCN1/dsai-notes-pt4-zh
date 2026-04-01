# 使用 MarsCode 编写 Node.js 全栈应用开发实践

> 来源：[https://agijuejin.feishu.cn/docx/SSqKdAlNsoo8tYxEeYOcnYIbnHe](https://agijuejin.feishu.cn/docx/SSqKdAlNsoo8tYxEeYOcnYIbnHe)

> 作者：狼叔

> 链接：https://juejin.cn/post/7389452029348462643

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

我基本上经历 Node.js 的几个部署阶段，从原始云服务，到K8S容器化，到无服务的 FaaS 云函数，也做过一些科普工作。2017年之后做过很多年 SSR ，也是从基本 Web 框架到 FaaS ，最多做个降级容灾等大流量下稳定性相关的工作，其他就是全栈场景了。现在基本上，也就TRPC能够让我觉得眼前一亮，仔细想想，也不过是 SDK 一样的思路，至于 AI 和 Node.js 开发有什么好的实践，我还没看到，都是常规思路，当我试过 MarsCode 之后，还是有些激动的。

![](img/0fa6d021a7252b02a6c1f27c9e474362.png)

在 MarsCode 发布之前，我就是第一波邀测用户，发布会当天我也去了现场。这个项目的核心开发主要是天猪、死月、老雷、段鹏飞等 Node.js 中坚力量，无论出于何种想法，我都是应该是去支持的。下面结合我了解的一些信息和动手实践内容，简单介绍一下我理解的 MarsCode。

## MarsCode是什么？

> 官方说法：MarsCode 是字节跳动推出的免费AI编程助手，提供了 AI 驱动的云端 IDE 版和支持VS code 和 JetBrains 等的扩展版。MarsCode IDE 具备代码补全、生成、解释功能，支持AI问答和 BUG 修复功能。MarsCode 还提供 AI 插件开发与部署工具，简化了 API 测试、存储管理和Schema 管理。此外，它拥有一键部署功能，提供安全可靠的云开发环境，支持多种项目创建方式，无需配置即可开始开发，确保了快速稳定的开发体验。

+   Marscode 国内版：[www.marscode.cn](https://www.marscode.cn)

+   Marscode 国际版：www.marscode.com

官方说 MarsCode 主要包含了 AI 功能的编程助手 和 Cloud IDE，我认为不全面。我理解 MarsCode 是 AI + WebIDE + 自动化部署平台。

+    深度集成AI 功能，从 Chat 到选中到流程集成。

+    WebIDE，以前我们开玩笑说它最大的作用是为外包用，代码不落地。那时候是因为性能不够，加载市场较慢。现在 Chrome 和电脑硬件变强，加载速度已经很好了，甚至有人直接用 iPad 写代码。这是个趋势的，比如 Codespaces ，就是 Github 提供的 WebIDE。

+    自动化部署平台，它有部署服务，做了非常多的简化工作。

发布会当天通过需求开发、修复 Bug、开源项目学习三个实际场景，详细演示了豆包 MarsCode 的项目问答、代码补全、单测生成、Bug Fix 等功能。确实没翻车，演示的还算比较流程。

> 官方说：通过完整场景演示串联各功能的使用配合，展示出豆包 MarsCode 在日常工作中的优秀落地能力，无论是新手程序员还是经验丰富的开发者，都能体验到豆包 MarsCode 带来的助力和效率提升。

这句话我不敢说，通过下面一系列实践，让大家来真实感受，可能更靠谱。下面通过场景一个具体的项目来讲讲我对 MarsCode 的理解。

## 创建项目，WebIDE 速度非常快

首先，在官网场景一个 Node.js 项目。注意这里选择的是 Node.js for AI Plugin，这是官方推荐的模版。

![](img/bbecbf16ca637a0b8187925ef9073dae.png)

从项目创建到加载非常快，体感上2-5秒左右，体感速度应该跟 replit 差不多持平，如下图。

![](img/d269dfccd42e0ea833873b3306a7ca84.png)

这里尝试着去问了一下当前操作系统是啥，目前版本还比较傻，无法回答。

![](img/fdead91d17a1327b73d9f708d266760a.png)

安装 tree 命令，英语查看文件目录结构。备注：apt-get下个版本可能会干掉或局部支持。

![](img/227429248236fe546c8391e170422d95.png)

通过tree和du查看文件数和存储大小。

![](img/480d0fa5be75e283fbac25fd5870c695.png)

18个文件，加上 npm 安装，总大小72M，加上 os，大约300多M。

我是第一批 MarsCode 邀测用户，大约是4月底。那个时候一个项目从创建到启动大约1-2分钟，现在基本上能够做到[2-5s左右启动，同为 VSCode 的谷歌 idx](https://link.juejin.cn?target=https%3A%2F%2Fopulent-garbanzo-6vw4gx9xjv9h5qrq.github.dev%2F%255C)，现[在启动还是大概按分钟计算的，Codespace大约30s左右](https://link.juejin.cn?target=https%3A%2F%2Fopulent-garbanzo-6vw4gx9xjv9h5qrq.github.dev%2F)%25EF%25BC%2589%25E3%2580%2582%25E7%259B%25AE%25E5%2589%258D)（[opulent-garbanzo-6vw4gx9xjv9h5qrq.github.dev/\](opulent-garbanzo-6vw4gx9xjv9h5qrq.github.dev/)）。目前 MarsCode 这个性能，可以说是行业里是领先的，应该是 Top 3。我和天猪简单的聊聊几句，此处是有高级技巧的，他们连着加班搞了1个月，在 K8S 上优化了很多细节，具体还处于保密阶段。

## 项目实战

我个人对 Node.js 比较熟悉，就以这个角度来做分析是比较容易的。这里我选了 Node.js for AI Plugin 这个模版，它是专门服务 Chatgpt 插件场景的，MarsCode 还有非常多的其他模版，支持社区绝大部分框架（比如 React、Vue、Python、Go、Rust 等），也支持从 Git Repo 导入，理论上支持所有语言和框架。

本小节从项目目录结构到写法，到和 Vercel 对比，希望深入浅出的剖析一下 MarsCode 实现的一些细节。

### 项目结构

直接看一下忽略 node_modules，只显示3层的结构，这样更便于理解。

```
$ tree -I node_modules -L 3
.
├── api
│   ├── metadata.json
│   ├── sayhi
│   │   └── hello.ts
│   └── todo
│       ├── consts.ts
│       ├── create.ts
│       ├── delete.ts
│       ├── edit.ts
│       ├── search.ts
│       └── types.ts
├── package.json
├── pnpm-lock.yaml
├── README.md
├── runtime
│   ├── cli
│   ├── current_revision
│   ├── package.json
│   ├── pnpm-lock.yaml
│   ├── src
│   │   ├── cli.js
│   │   ├── core
│   │   ├── proxy
│   │   ├── schema
│   │   ├── types
│   │   └── wrapper
│   └── united_manager_version
└── tsconfig.json

10 directories, 18 files
```

分析如下

1.  使用 pnpm 做包管理器，行业内最佳选择。

1.  采用 ts 编写，代码非常规范，可圈可点。

1.  核心分 api 和 runtime 2个文件夹

1.  api 是业务代码

1.  runtime 是 SDK，把函数写法，cli 等工具放进去的。后面会隐藏。当前是不错的学习材料。

### 请求分析

api 目录结构，这是开发者平时改动最多的地方。

```
$ tree api
api
├── metadata.json
├── sayhi
│   └── hello.ts
└── todo
    ├── consts.ts
    ├── create.ts
    ├── delete.ts
    ├── edit.ts
    ├── search.ts
    └── types.ts

2 directories, 8 files
```

它的服务详情藏的略深。需要点击项目名，然后才能显示项目详情，里面才有 Service Detail。它会显示当前项目的域名和路由，对理解项目至关重要。

看了 Service Detail 和目录结构，这里面的原理就比较容易理解了。

首先它是一个 Server 容器，简单理解是 Docker。配置如上图，2核，4G内存，10G硬盘。实话说，这个配置给的很大方了。

有了 Server，自然就有 IP 和端口，这里 Functions 下面的 HOST 就是默认给的域名。

```
arduino
 代码解读
复制代码
https://37iyzcv1c9g9m.ahost.marscode.site
```

那个API Key其实是JWT类似的机制来实现的。复制一个 curl 请求，大致原理就能扒出来了。

```
$ curl --location --request 
        POST &#x27;https://37iyzcv1c9g9m.ahost.marscode.site/sayhi/hello&#x27; 
        --header &#x27;Content-Type: application/json&#x27; 
        --header &#x27;Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmdW5jdGlvbmlkIjoiMzdpeXpjdjFjOWc5bSIsImlhdCI6MTcyMDA5NjgwOCwianRpIjoiNjY4Njk4MjgwZWQ2ZDRiZmQ1MTQ0ODgyIiwidHlwZSI6ImZ1bmN0aW9uIiwidXNlcmlkIjoiQ0xPVURJREVfMWRrbnc5NTFuNXA1dm5fNzM4Nzc1NzM0ODkyMjcwNDkxMyIsInZlcnNpb24iOjJ9.qZuzik_o-lg4Qpo4XdQC2FjpaRpaZEL5fRebCV9PI0M&#x27; 
        --data &#x27;{}&#x27;
{&quot;message&quot;:&quot;hello world&quot;}%  
```

从这个请求可以看出

+   POST 请求

+   文件即路由。sayhi/hello.ts对应sayhi/hello请求。

+   默认返回 JSON 格式

+   默认做了 Authorization 鉴权，我搜了一下 jwt 或 jsonwebtoken 都没有搜到，可能是只实现了机制，没用过对应的包。

### 函数写法

看一下函数

```
import { Args } from &#x27;@/runtime&#x27;;

export interface Input {
  // name of user
  name?: string;
}

export interface Output {
  // reply to greet the user
  message: string;
}

/**
 * Say hello to the user when he introduces himself
 */
export async function handler({ input, logger }: Args&lt;Input&gt;): Promise&lt;Output&gt; {
  const name = input.name || &#x27;world&#x27;;

  logger.info(`user name is ${name}`);

  return {
    message: `hello ${name}`
  };
}
```

要点说明

+    输入 Args

+    输出 Output

+    对外导出 Async 函数

是不是发现这东西和 trpc 很像？

![](img/bd1099be7f7583d29ddd70b395691269.png)

对比一下

+   trpc 需要定义方法名，MarsCode 里的 Async 函数名，比如 handler。二者一模一样

+   trpc 显式 input，里面做了 zod 校验，MarsCode 是 ts 类型，也算异曲同工。

+   图中的 trpc 是 query，即 GET 方法，然后是返回。而 MarsCode 全部是 POST，其实更简单。

很明显这种函数写法是行业里最佳实践，基于 FaaS，隐藏了 HTTP 协议细节，强类型，采用约定式路由。

MarsCode 函数写法上要更简单，对于测试过程也能够做到极大的简化。在 MarsCode 里，Api测试过程是非常简单的，只需要4步，结合AI可以提效。

1.  点击 API Test 按钮，这时候会切换到 API Test panel，

1.  点击 Auto Generate，这部分是 AI 根据 Input 类型进行生产 Mock 数据，看着还行。

1.  点击 Send 按钮就可以发送了。 我猜测和上面的 curl 是一样的。

1.  在 Output 区域输出 message

![](img/469d54c21dc7276207cca56a6e891fd6.png)

### 都是POST有问题吗？

在 MarsCode 里，所有的函数都采用 Post http verb 提供服务。如下图。

![](img/37f0edb35efd962f44334af868161e00.png)

我之前猜测，这是一个无心插柳的故事（事故）。和天猪求证，隐藏协议的一个考虑点是 云函数是没有协议概念的，虽然一般会有个触发器的概念，但 AI Plugin 场景没必要的。其实在约定式路由里，表达 Verb 是件麻烦事，要么代码里加，要么加配置文件，很明显都是有不少工作量的。

对于API而言，都用POST有问题吗？专业后端可能会拿 Restful API 说事，为了语义表达等等，这曾经是很多年被奉为圭臬的标准。咱且不论它到底好不好，在落地上，严格遵守的并不多。

参考github.com/jshttp/meth…

```
&#x27;get&#x27;,&#x27;post&#x27;,&#x27;put&#x27;,&#x27;head&#x27;,&#x27;delete&#x27;,&#x27;options&#x27;,&#x27;trace&#x27;,&#x27;copy&#x27;
,&#x27;lock&#x27;,&#x27;mkcol&#x27;,&#x27;move&#x27;,&#x27;purge&#x27;,&#x27;propfind&#x27;,&#x27;proppatch&#x27;,&#x27;unlock&#x27;,
&#x27;report&#x27;,&#x27;mkactivity&#x27;,&#x27;checkout&#x27;,&#x27;merge&#x27;,&#x27;m-search&#x27;,&#x27;notify&#x27;,
&#x27;subscribe&#x27;,&#x27;unsubscribe&#x27;,&#x27;patch&#x27;,&#x27;search&#x27;,&#x27;connect&#x27;
```

如果再细分的话，可以按照是否有 Body 分成2类，典型的就是 GET 和 POST。GET 有一个2048字节的限制，所以一些超长的请求会被截断。一般查询类的用 GET，创建用 POST。

交代了上面这点背景之后，我们来讨论一下只用 POST 是否 OK？先说我的结论，我认为是 OK 的。POST 是万能的，它没有 GET 的限制，如果不在意语义完全是可以的。业内从 trpc 到 httpc.dev，都是有意的隐藏 HTTP 协议，只要知道函数名和参数就能够完成调用。

这里以 [httpc.dev](httpc.dev) 举例

```
// Server
function add(x: number, y: number) {
    return x + y;
};

function greet(name: string) {
    return `Hello ${name}`;
};

export default {
    add,
    greet
}

// Client
import createClient from &quot;@your-package/api-client&quot;;

const client = createClient({
    endpoint: &quot;http://api.domain.com&quot;
});

let result = await client.add(1, 2);
// result: 3

let message = await client.greet(&quot;Edith&quot;);
// message: &quot;Hello Edith&quot;
```

很明显，这样的写法是极简的。它背后的实现原理也非常简单，就是约定了请求写法，如下图。

httpc 的写法所有的 Get 请求都把参数放到 $p 上，然后取到的数组传给函数的参数，这块就是call、apply 发挥的地方。处理 Post 请求更简单，只需要把 Body 内容作为函数参数，实现更简单。

这样let result = await client.add(1, 2);这样的写法，就抹去了 HTTP 细节。其实，这就是 RPC 的玩法，只是 HTTP2 流行了之后，RPC 结合 HTTP 协议，通过约定，简化了写法。

在 ChatGPT 之前，我其实也是 Restful 拥护者。但当我被问到一个问题，结合 AI 你能提效70%吗？问这话的是一个算法，每天处理数据，用 Python 写一写片汤代码。他确实可以，但我为什么不可以呢？是不是我对专业化放不开？结合 MarsCode 上的函数实现，一旦 Restful API 变成函数级调用，AI 出码一定是效率更高的。

我想，AI 发展必然会有现有技术栈对 AI 进行适配，甚至是类似于 Moonbit 这样的在设计之初就为 AI 出发做了语法语义准备的新事物。我们以前用的技术栈真的对吗？在2024年年中，我想我们都需要考虑一下。

### BaaS决定它的应用场景

在 create.ts 里，有这样的代码。

```
 const result = await baas.redis.hset(
    TODOListKey,
    input.name,
    JSON.stringify({ name: input.name, state: TODOState.Active })
  );
```

很明显就是一个 BaaS SDK 里有 Redis。在云函数里，FaaS 是function as service。BaaS 是backend as service。BaaS SDK 就是和后端服务相关的 SDK，也可以叫中间件。

在 MarsCode 函数项目里，内置了 Redis 和文件存储。比如我通过 hgetall 来查一下我刚才创建的数据，结果如下图。

![](img/c2765ff5b9ec6e562f7307d242041fe3.png)

它的文件用法也是非常简单的。参考[docs.marscode.com/reference/f…](docs.marscode.com/reference/f…

```
import { baas } from &#x27;@marscode/baas-sdk&#x27;;

// upload
const fileInfo = await baas.file.upload(&#x27;hello world&#x27;, &quot;hello.txt&quot;);

// download
const content = await baas.file.download(fileInfo.url);
```

从这个实现看，大概率是本地文件。如果想利用免费服务器做点啥也是可行的。

至此，我们基本就可以有一个结论，从目前的文档和前面的总结来看，MarsCode 对 Node.js 和Python 支持是最好的。从 Node.js 实现上分析，它对 BFF 支持极好，如果要接云数据库(比如supabase\planetscale)，也是极其简单的，可以覆盖目前 Web 开发的所有场景。

MarsCode 同时支持从模版创建项目和从 Git Repo 导入，也就是说，你可以使用任意技术栈任意框架，它是一个通用的 WebIDE + 运行环境。我个人最喜欢它的 Node.js 云函数项目模版，简单实用，结合 AI 代码提示，写起来非常高效。

### 结合部署看一下全链路提效

在 MarsCode WebIDE 里是有终端的，但你不需要自己启动服务，它会自动帮你运行。API Test 前面大家都看到了，下面我们再看看它的一键部署过程。

你只需要点击上面的 Deploy 按钮即可，然后 MarsCode 就会帮你自动完成发布。

绑定域名

![](img/1e499aa311f120af068fb7a86908b85b.png)

![](img/786f186b671ed5ef77155ba3de87c240.png)

取到你的域名里，把 cname 映射上就可以生效了。

### 对比 Vercel 和 MarsCode

+   Vercel 是一个自动化部署环境。结合 Github，对于 CI/CD 支持极好。

+   MarsCode 是 WebIDE，提供了 AI 和自动化部署能力，WebIDE 里内建的 CI/CD 更简单。

![](img/8e8614b3536b30f37c59ef952788879e.png)

再看一下价格

Vercel价格参考[vercel.com/docs/pricin…](vercel.com/docs/pricin…

![](img/556749483096e96ddc88d26b536b4998.png)

MarsCode免费额度（WebIDE运行环境和Server），参考[docs.marscode.com/get-started…](docs.marscode.com/get-started…

![](img/5d54899a51c2ff6c1b20dc7aa03f7e46.png)

## 最佳实践

先来看看我的几个尝试。

1、使用 React 项目，我试了2个模版，CRA 和Nextjs，支持的都非常不错。想怀旧，用 Koa 写 EJS 也是可以的。

![](img/14f69841f41ee2a000dc83160ea54051.png)

2、API推荐采用云函数（**Node.js for AI Plugin**）

除了 Node.js 外，还有 Python AI 插件，当然你也可以选择 Go、Java，Rust 等。前面已经讲了，这里就不再重复了。

3、更多模版

从语言到模版，已经非常全面了。其中对国产第一语言 Moonbit 的支持是行业里第一个，支持断点调试了哦。

![](img/2ea8123643c6fb12eb7b82f4da426c2b.png)

有了上面这些信息，我们就可以给出最佳实践了。

1.  前后端分离，前端选 React，后端选 Node.js 云函数，这是一个常规技术选型。

1.  前后端分离 + BFF

1.  前端选 React，BFF 选 Node.js 云函数，后端选 Node.js 云函数 + 云数据库。

1.  前端选 React，BFF 选 Node.js 云函数，后端选其他语言云函数+ 云数据库。

由于 MarsCode 目前是免费的，且云函数是按需付费，可以在很大范围内享受免费的红利。对于独立开发者或者小公司小团队是非常适合的。这里需要提醒一下，自动发布仅限海外服务器，对于出海的开发者来说会更友好。

这种组合的方式，可以最大范围的使用免费额度。当然，还可以有很多骚操作，比如讲一个站点拆成多个页面，然后通过微前端或者微模块的方式嵌入。只要你想省钱，办法总比困难多，哈哈哈哈。

## AI

关于AI部分，建议大家去看看阮一峰老师写的测评，参考www.ruanyifeng.com/blog/2024/0…

![](img/df75692f3ffbb0805a798a6927f0717f.png)

这是他给的结论，MarsCode 完全可以替代Github Copilot。我一般都是眼见为实，自己折腾了一些内容。

### 分析项目

MarsCode IDE 通过提供开发模版，让开发者能够快速进入项目而无需运维本地环境。借助原生集成的 AI 能力，开发者不再需要自己去理解代码，从而更高效地上手项目。发布会的时候，现场演示了Langchain 项目的导入和分析，效果还是非常不错的。

我在 Node.js 云函数项目里也做了简单提问，效果还可以。

![](img/98dcccbc147f3d0a961e0654c48d8443.png)

截图下面还有一段，如下。

```
项目使用PNPM来管理依赖项。您可以使用PNPM命令来安装依赖项，例如pnpm add axios。

项目还提供了OpenAPI Schema的生成工具，
它可以根据Input和Output接口的类型描述和注释生成相应的Schema描述。
在构建和部署过程中，将生成metadata.json文件，并将其解析为最终的OpenAPI Schema。

此外，项目还提供了API测试工具，可以帮助您测试API。
您可以在MarsCode中点击顶部的&quot;API Test&quot;按钮打开API测试面板，并发送请求进行测试。

最后，项目还提供了集成的云函数托管功能，您可以通过点击顶部的&quot;Deploy&quot;按钮快速部署云函数。

如果您需要更详细的信息，可以参考项目中的README.md文件或查阅MarsCode的文档。

如果您有任何问题或反馈，可以在MarsCode的Discord社区中提问或反馈。

希望这些信息对您有所帮助！如果您有任何与编程相关的问题，我将很乐意帮助您。
```

它描述的准确程度是非常棒的，通过项目实战章节的内容相信大家都能有一个比较好的判断了。

+   OpenAPI Schema，我是没提到的。

+   内置的也确实是 pnpm，执行 pnpm add axios，这个确实是可行的。

+   根据 Input 和 Output 接口的类型描述和注释生成相应的 Schema 描述，上面我有讲到。

+   在构建和部署过程中，将生成 metadata.json 文件，并将其解析为最终的 OpenAPI Schema，我没写，原因是我没看懂，他就是一个空文件。

+   API Test 和 Deploy 也是对的。

这块生成的还不错的，甚至我觉得可能是项目组给 LLM 喂了很好的饲料。无论如何，我自己的这个体验还是不错的。

### 问答

我日常用问答式的应用还是比较多的，这里面我选了一个我最近常用的 AST Vistor 的写法，分别测试一下。

MarsCode的返回结果如下图。

![](img/b3c2c06df1e58531124d8f7fb9cac49b.png)

下面是 Chatgpt 3.5 返回的结果。

![](img/6ddac3e2c951d5638200fab186c2df96.png)

不清楚什么原因，可以看出来的是，MarsCode的回答很明显是更偏向于代码的。

其实自己买 Chatgpt 或者套壳服务还是需要一年 500 元以上的。我们可以假设一下，开一个MarsCode 去蹭他的免费额度，我目前了解到的信息是 MarsCode 每天补全 1500，问答 100，可能会动态调整。对于一般的开发来说，如果对比各个模型答案，基本上是够用的。

### 创建User接口

AIGC 出现后，我们都想着如何让AI帮我们自动化，比如一句话，让他帮我们打开车机功能，智能家具功能，很明显这已经比之前的人工智障的音箱好多了。还有很多人尝试过通过语音控制浏览器，和Siri 类似，但比 Siri 好用的多。我个人很喜欢 Vimium，结合语音转文字，文字通过 LLM 解释下，找对最匹配的行为。甚至很多基于这种形式的硬件，曾经我都很关注。

所以，我想按照这个方式嚯嚯一下 MarsCode。于是向它提问：帮我在api目录下新建user的crud接口。

![](img/c9b11a28eaa99db4038b7c4c76193bbf.png)

MarsCode 确实能够生成正确的基于它云函数的代码。但没有帮我创建目录和文件。

MarsCode 生成的代码区块有3个操作

+   复制

+   插入，插入到当前选中文件，会根据光标进行

+   新建文件，是 VSCode 的那种 cmd+n，不是真的新建文件。保存是需要自己进行的。

如下图。

![](img/35f8be1c26a8b0b43486725d4c23a5f9.png)

还是比较保守，按照当前的能力来说，直接写入文件也可以的，可能对算法还不是特别有信心，毕竟现阶段AI还是不能替代程序员。当前AI能力局部上已经很强了，但完整流程可控还有一些距离，即时 MoA（Mixture-Of-Agents） 也没法做的效果很好。

无论如何，MarsCode 在云函数这块能够出码准确，还是不错的。

### 部分选中

Copilot 也有类似的功能，这个就不多讲了。

![](img/1323c6e170242eb20887986dd95e66b2.png)

### 生成文档

选中代码之后，直接生成文档，这个功能还是非常方便的。尤其是云函数这个模版里，它的准确度是非常棒的。

![](img/a2c4bb156e74a435bff526f389826890.png)

### 生成测试

AI 上来之后，最先折腾的就是测试，然后设计，然后前端。事实上，局部代码，生成测试用例是可行的，这种就是能帮忙多少生成多少，集成测试不行，单元测试能生成也行。

![](img/2ecaf567293632873305d9adb04674d4.png)

看一下它生成的代码

```
import { Args } from &#x27;@/runtime&#x27;;
import { handler, Input, Output } from &#x27;@/api/sayhi/hello&#x27;;

async function testSayHi(): Promise&lt;void&gt; {
  const input: Input = {
    name: &#x27;John&#x27;,
  };

  const args: Args&lt;Input&gt; = {
    input,
    logger: console, // 使用控制台作为日志记录器
  };

  try {
    const result: Output = await handler(args);
    console.log(result.message); // 打印返回的消息
  } catch (error) {
    console.error(&#x27;Error:&#x27;, error); // 打印错误信息
  }
}

testSayHi();
```

逻辑上没有问题，只是没有套用任何测试框架而已，直接用 node 命令执行更快。如果想换成 xv 测试框架，只需要把 Node.js 断言加进来，简单处理断言即可。

## 总结

MarsCode 适用于通用 Web 开发的所有场景，其WebIDE和全场景AI智能是特色，在云函数基础服务上打造一站式智能开发平台。

本文中总结了MarsCode平台的3个要点。

+   WebIDE 和全场景 AI 智能，免费额度足够用，基本上可以不用买套壳服务了。GitHub Copilot 是收费的，每月10美元，而豆包 MarsCode 是免费的，单单这一点就值得推荐后者。你把它当成免费的AI问答服务也挺好。

+   云函数基础服务，免费额度足够用，比 Vercel 要多的多。

+   覆盖全场景，目前前端、BFF、Api 都支持，可组合使用，更划算。

在写这篇文章的过程中也发现一些 MarsCode 目前还不支持的问题。显然，对于刚发布没多久的产品来说，这多少有点鸡蛋里挑骨头了。

1.  比如我想了解当前容器信息。目前ai是没有训练的。

1.  比如我想使用 tsx 转译文件，目前是不会成功的。

1.  比如云函数里自动加测试，目前还没有。

无论如何，MarsCode 是可用的，有创新的，先进的，基于 WebIDE 做 AI 是我非常欣赏的方向。我是坚定的 Web OS 信仰者，未来浏览器可以搞定所有事情，结合 AI 提效，不管我们喜不喜欢，它都一定会存在且演进，至于能不能替代程序员还需要观察，但善于用 AI 的程序员肯定是更抢手的。从 MarsCode 开始，展望一下未来软件开发场景，是一件很不错的事儿，推荐大家试一试。