
# 第 4 章 资源的玩法

> 来源：[https://v11enp9ok1h.feishu.cn/docx/JddrdGfqIomqmsxA0xvcDKUXncx](https://v11enp9ok1h.feishu.cn/docx/JddrdGfqIomqmsxA0xvcDKUXncx)

内容更新中~

## 资源的玩法

### 插件

#### 什么是插件？

> 参考：https://www.coze.cn/open/docs/guides/plugin

插件是一个工具集，一个插件内可以包含一个或多个工具（`API`）。

目前，`Coze` 集成了类型丰富的插件，包括资讯阅读、旅游出行、效率办公、图片理解等 `API` 及多模态模型。使用这些插件，可以帮助我们拓展智能体能力边界。例如，我们第三章的夸夸智能体内添加搜索插件，我们的智能体将拥有搜索新闻资讯的能力。

当 `Coze` 集成的插件不满足我们的使用需求，我们还可以创建自定义插件来集成需要使用的 `API`，后面我会从 `IDE` 创建插件、通过 `JSON` 或 `YAML` 文件导入插件，教大家如何扩展我们的使用边界。

（`API` 的方式放在第 6 章）

#### 使用 IDE 创建插件

> 制作谷歌搜索插件为例

> 参考：https://www.coze.cn/open/docs/guides/ide

> [【Coze 插件】零代码创建谷歌插件](https://zxdwhda-share.feishu.cn/wiki/Z4YGw0fSSinTn6kX6jlcP6uunje?fromScene=spaceOverview) 如果想学习零代码从 0 到 1 插件谷歌插件，跳转：

##### 前期准备

> 参考：https://zhuanlan.zhihu.com/p/174666017

官方需要非大陆链接，而且需要外币卡，所以我们使用这里使用 `serper`：https://serper.dev/

+ 有免费的额度，可以调用 2500 次

+ 国内直连

这里不多演示注册，复制 `key`，后面需要

![](img/ee2930d0481edcd97db48de70098cc34.png)

##### 新建插件资源与工具

先让我们创建一个插件

+ **插件名称**：谷歌搜索

+ **插件描述**：搜索网页

+ **插件工具创建方式**：云侧插件 - 在 `Coze IDE` 中创建

+ **IDE 运行时**：`Node.js`

让我们创建一个工具，点击“在 `IDE` 中创建工具”

点击左上角的“创建工具”

+ 名称：`Google_Search_Express`

+ 描述：谷歌搜索快速版

![](img/1fc5b4467569a048111ea473c8b2231d.png)

#### 界面布局

让我们逐条分析

1. 这里是工具列表，我们刚才新建了一个 `Google_Search_Express`，所以不是空置，有同学要问了，这里对应商店的是哪里，让我们以飞书云文档为例

![](img/773fd801845bbb1f07d5c35afee5370d.png)

大家可以发现，插件是有好几个工具的，还记得前面说的“插件是一个工具集，一个插件内可以包含一个或多个工具（`API`）。”，我们刚才创建的插件是有一个工具（当然也可以有多个），而飞书云文档有多个。

![](img/26af26d9f9c745d785a78e0043752705.png)

1. 依赖包

> `依赖包` 区域，管理依赖包，所有工具共用该依赖列表。

> 单击 `+` 或者单击 `添加依赖` 按钮，输入依赖包名称并选择版本（可通过 `依赖名@版本号` 的格式进行搜索），然后安装依赖包。

在编程中，依赖包（也称为库、模块或包）是预先编写好的代码集合，用于解决特定的问题或提供特定的功能。是可复用的代码集合，提供特定功能（如数据处理、网络请求、UI 组件等）。开发者通过导入这些包来避免重复造轮子，提升开发效率。

1. 代码

> 不同 `IDE` 运行时的代码模板不同，这里是 `Node.js` 的代码模板。

![](img/9b40a854a6730a7820a9ef5a05990158.png)

这里是你写或者让 AI 编写代码的地方。

1. 元数据

元数据的作用是让大语言模型理解每个工具输入或输出参数有哪些、各个参数有何含义。因此，在工具的元数据内提供详细的参数说明，可以让大语言模型更准确的使用工具。

![](img/ebeca22375a41d20528fd53b64ded560.png)

当用户在使用该工具的智能体时，模型会根据工具的元数据信息，从用户问答中解析、提取出对应的输入参数，并选择调用该工具，流程图如下所示。

![](img/c08b176e190fafbc77ddcd714e19663c.png)

元数据配置说明：

| 配置项 | 描述 |

| --- | --- |

| 名称 | 工具名称。建议输入清晰易理解的名称，便于后续大语言模型搜索与使用工具。 |

| 描述 | 工具的描述信息，一般用于记录当前工具的用途。 |

| 启用 | 是否启用当前工具。使用说明：  - 如果工具未开发测试完成，建议先禁用该工具，只启用并发布已通过测试的工具。  - 如果需要下线某一工具，可将该工具设置为禁用，并再次发布插件。  - 如果插件中只有一个工具，则不支持禁用该工具。如需下线该工具，您可以选择直接删除该插件，或者创建另一个工具并完成开发测试后，再禁用该工具，最后发布插件。 |

| 输入参数 | 当前工具对应接口的输入参数信息。准确、清晰易理解的参数名称、描述等信息，可以让大语言模型更准确的使用工具。 |

| 输出参数 | 当前工具对应接口的输出参数信息。准确、清晰易理解的参数名称、描述等信息，可以让大语言模型更准确的使用工具。 |

1. 测试相关

让我们先看测试代码，分别有输入、输出；一般我们做测试的时候只需要写输入就可以使用了，输出后面可以让 `Coze` 一键生成。而控制台是我们查看运行日志。

这是日志，日志一般只保留 7 天

好了！通过刚才的学习，相信你一定对插件与工具之间的作用有了一定的了解，下面让我们开始实操。

##### 完善工具

> 想学习怎么写代码的可以看这个 [【Coze 插件】零代码创建谷歌插件](https://zxdwhda-share.feishu.cn/wiki/Z4YGw0fSSinTn6kX6jlcP6uunje?fromScene=spaceOverview)

安装依赖包，搜索 `axios`

![](img/d46cb4e7d80973da6aba6940f69465d5.png)

![](img/5b8346a4c9b9b15e640860959e5e7f0f.png)

复制代码粘贴

```
import { Args } from '@/runtime';
import axios from 'axios';

interface Input {
  q: string;     // 必填：搜索关键词
  key: string;   // 必填：API 密钥
}

interface SearchResult {
  title: string;
  link: string;
  snippet: string;
}

interface Output {
  success: boolean;
  error?: string;
  results: SearchResult[];
  related?: string[];
}

export async function handler({ input, logger }: Args<Input>): Promise<Output> {
  // 基本参数验证
  if (!input.q || !input.key) {
    const errorMsg = 'Missing required parameters: both "q" and "key" are required';
    logger.error(errorMsg);
    return {
      success: false,
      error: errorMsg,
      results: []
    };
  }

  try {
    const { data } = await axios({
      method: 'post',
      url: 'https://google.serper.dev/search',
      headers: { 
        'X-API-KEY': input.key, // 使用动态 API 密钥
        'Content-Type': 'application/json'
      },
      data: JSON.stringify({
        q: input.q,
        gl: 'us',       // 默认国家：美国
        hl: 'en',       // 默认语言：英语
        num: 10,        // 默认结果数：10
        page: 1,        // 默认页码：1
        location: '',   // 默认地点：空
        tbs: ''         // 默认日期范围：空
      }),
      timeout: 10000
    });

    logger.info(`Successfully fetched ${data.organic?.length || 0} results for "${input.q}"`);
    
    // 结果格式化
    const results: SearchResult[] = data.organic?.map((item: any) => ({
      title: item.title,
      link: item.link,
      snippet: item.snippet
    })) || [];

    return { 
      success: true,
      results,
      related: data.related?.map((r: any) => r.query) || []
    };

  } catch (error: any) {
    // 精简错误信息提取
    const errorMsg = error.response?.data?.message || error.message || 'Unknown search error';
    logger.error(`Search failed for "${input.q}": ${errorMsg}`);
    
    return {
      success: false,
      error: errorMsg,
      results: []
    };
  }
};
```

编辑输入参数

+ 名称 `q`

+ 搜索关键词

+ 描述 `Key`

+ `API` 密钥

![](img/166ef4412aa807f07be9f440ffdb9afa.png)

实操不好的小伙伴，看视频：

![](img/a697f50f13e299f3f1628d425c2d5213.png)

##### 测试

点击右上角“自动生成”

![](img/8b06eab3f2dbd81ab84d9e1d5d0cb4e1.png)

修改默认的内容，点击运行

![](img/c143bfbd55f1c1b8906451a9d94e9fff.png)

![](img/93d125adf118a8fb1ab059bcb360309e.png)

可以看到运行结果

![](img/be9dd135c1cab1c65f32e47bb4b97ee6.png)

点击“更新输出参数”

![](img/2ef14c5dfe27be35e5baa7681b10202b.png)

出现这个说明正常，有时候会卡

我们可以看到输出参数给我们写好了

##### 发布

点击发布，稍等

返回到智能体就可以看到了~

![](img/89a34cc3f30af8063f774451aa3ddfc9.png)

> 好了，我把最难的放在了前面，后面的就简单了很多，加油！

#### 通过 JSON 或 YAML 文件导入插件

> 制作 `gpt-4o` 插件为例

> 参考：https://help.302.ai/docs/ji-cheng-dao-Coze-guo-nei-ban?search=1

这个工具使用较少，建议跳过

##### 前期准备

这里使用的 `302.AI`，使用原因：是题目的文档中的 `YAML` 文件使用比较详细，而且新人有免费额度，不做其他推荐使用，请自行寻找中转接口。

让我们注册：https://302.ai/

打开后台，添加 `API`，复制，后面有用

![](img/531165d4a93eac4ecdc75479dae3aed6.png)

##### 新建插件资源与工具

先让我们创建一个插件，点击导入

点击 “URL 和原始数据”，复制粘贴下面内容
