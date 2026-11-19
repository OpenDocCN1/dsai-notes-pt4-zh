
# coze国内版插件汇总

> 来源：[https://bluefocus.feishu.cn/docx/NyqQdmdaRohYG9xeCdncPAZ3nch](https://bluefocus.feishu.cn/docx/NyqQdmdaRohYG9xeCdncPAZ3nch)

## 插件与插件工具的区别

目前国内版总共有多少个插件？都有什么插件？

是首页商店看到的这个 **36个** 吗？

![](img/761d13b267143e0afafd13f2aeaa1e9b.png)

是，它们叫**插件**，但在**工作流**里，我们用到的是**插件工具**！！！比如这个**文生图**的**插件**：

![](img/e7fd39915a82019f6277466cc107015b.png)

它包含了三个**插件工具**，前面讲过**插件和节点**的本质都是**API接口调用的封装**，那么插件和插件工具的区别：

> 答：**插件**指定 →【`BaseUrl` + 默认请求头】，**插件工具**指定 →【调用接口、输入/输出参数】

## 统计插件工具

所以，使用同一插件的不同插件工具，本质上就是调用同一域名下的不同的接口。好，修正下问题：

> **国内版总共有多少个插件工具**？

官方并没有说明，想知道得自己统计。在折腾工具流时，我发现插件搜索并没那么好用，比如，我想要用文生图的插件，搜"图"：

![](img/997b314d0d15fe5464861edf30816c0f.png)

明显是根据**插件名**来匹配的，支持文生图的`ByteArtist`插件就没显示出来。另外，我之前好像看到官方的Bot的工作流用到了隐藏插件，用户没法直接搜到，但是可以通过间接的方式来使用它：

> 打开官方Bot的工作流，点击右上角**创建副本**来拷贝工作流到自己的工作空间，就可以用它了。2333，之前云雀大模型抽风的时候，群里有人通过这样的方式拷贝了更牛批的`Seed`大模型。不过现在不行了，默认只能用云雀了，如果想调其它大模型，可以在代码节点使用`request_async`库来调用第三方的API来实现。

![](img/eadffcc1a7dbe003fd458bfd0d1d520e.png)

综上，笔者想做的事情就是：

+   **统计能搜到的插件工具**，做下汇总输出，方便Bot玩家对内置插件工具进行检索，快速找到心仪的工具。
+   发掘下官方Bot的工作流中有没有隐藏的插件工具。

## 爬取思路

手动统计肯定是不可能的，爬虫走一波~

先调皮下，搭个Bot，看用`LinkReaderPlugin`插件能否直接提取，写下简易提示词：

![](img/baea1ef8e16bea43909dfec6f5936046.png)

**试试**：

![](img/f16b236027ebe1d19e0245e9fcf02ac4.png)

哈哈哈，明显不行，需要登录，F12看下请求：

![](img/ba188946283666b6068c077029d4f787.png)

看到`msToken`这么长的加密参数，就不用想接口模拟了。数据量不大，浏览器自动化（`selenium`）+ 请求拦截（`BrowserMob Proxy`）走一波看看，随手写下爬取代码：

```python
import os
import time
from browsermobproxy import Server
from selenium import webdriver
import cp_utils

coze_save_dir = os.path.join(os.getcwd(), "coze")
explore_list_file = os.path.join(coze_save_dir, "explore_list.txt")

def init_browser():
    chrome_options = webdriver.ChromeOptions()
    chrome_options.add_argument('ignore-certificate-errors')  # 无视证书验证
    chrome_options.add_argument('--start-maximized')  # 开始时直接最大屏幕
    chrome_options.add_argument(r'--user-data-dir=D:\ChromeUserData')  # 设置用户数据目录，避免每次登陆
    chrome_options.add_argument('--proxy-server={0}'.format(proxy.proxy))  # 设置请求的代理
    return webdriver.Chrome(options=chrome_options)

# 模拟登陆，只需调用一次，以后登陆失效了主动调用下就好
def login():
    browser.get("https://www.coze.cn/home")

# 获取Bots列表
def get_explore_list():
    proxy.new_har("get_explore_list", options={
        'captureContent': True,
        'captureHeaders': True
    })
    browser.get('https://www.coze.cn/explore')
    time.sleep(5)
    result = proxy.har
    for entry in result['log']['entries']:
        if entry['request']['url'].find('/explore/get_explore_list') != -1:
            content = entry['response']['content']
            print(content)
            with open(explore_list_file, "w+", encoding='utf-8') as f:
                f.write(content['text'])
                print(explore_list_file, " 文件写入完毕...")
    proxy.close()
    browser.close()

if __name__ == '__main__':
    cp_utils.is_dir_existed(coze_save_dir)
    server = Server(os.path.join(os.getcwd(), r'browsermob-proxy-2.1.4\bin\browsermob-proxy'))
    server.start()
    proxy = server.create_proxy({'trustAllServers': True})
    browser = init_browser()
    login()
    get_explore_list()
```

先调`login()`，完成登录后关闭浏览器，注释掉，调用`get_explore_list()`，此时打开浏览器就处于登录态了，拦截包含`/explore/get_explore_list`字符串的URL，保存`text`到本地。运行后报错：

![](img/375675645b79c1fd5525190e5540002e.png)

没有`text`，难道是**二进制数据**？`proxy.new_har`的`option`参数再加个`'captureBinaryContent': True`，运行：

![](img/9d0f931ef2a92c5c58f531d0d9be9501.png)

淦，读是能读到了，但`text`这字符串明显就是字符串加密 + `Base64`啊，yue了，但打开浏览器数据确是正常的：

![](img/e7f20f72dfc7e837802ba14aee48b65d.png)

明显是执行某个解密js后加载出来的，破解就算了，先不谈水平不够，浪费时间之余意义不大。

+   那自动化 + Xpath抠页面节点？算了吧，又low又费劲，无计可施才会用这个方法。
+   感觉得搞个**Chrome插件**做下接口监听了，但那是我的知识盲区...
+   [说到Chrome插件](https://link.juejin.cn?target=https%3A%2F%2Flearn.scriptcat.org%2Fdocs%2F%25E7%25AE%2580%25E4%25BB%258B%2F)，突然想到一个神级插件 → `Tampermonkey`（油猴/篡改猴）。会点js看下文档就能耍，简单瞄了下《油猴开发指南》，看了几个案例，加之这里的需求也比较简单：请求加载url、监听特定url的响应内容，保存数据到本地。
+   感觉应该不复杂，而且有GPT加持，可以一试。
+   浏览器装下油猴插件，直接开始边写代码边测试~

### 浏览器跳特定URL

直接设置下`window.location.href`的属性就好了，代码如下：

```javascript
// ==UserScript==
// @name         扣子插件工具信息爬取
// @namespace    http://tampermonkey.net/
// @version      2024-02-21
// @description  try to take over the world!
// @author       CoderPig
// @match        https://www.coze.cn/*
// @grant        GM_xmlhttpRequest
// ==/UserScript==

(function() {
    'use strict';
    console.log('插件加载...')
    // 浏览器跳转特定url
    function locationUrl(url) {
        window.location.href = url;
    }
    // 脚本加载后3s跳转掘金
    setTimeout(function() {
        locationUrl("https://juejin.cn/");
    }, 3000);
})();
```

运行后，控制台输出**插件加载中...**，**3s后自动跳转**掘金。对了，如果是想发起一个请求不跳转，可以使用`Tampermonkey`提供的API → `GM_xmlhttpRequest`，用于发起跨域 HTTP(S) 请求。它的参数是一个对象，包含了请求的各种设置。可选参数如下：

+   `method`：HTTP 请求方法，如`"GET"`、`"POST"`等。
+   `url`：请求的 URL。
+   `headers`：一个包含 HTTP 请求头的对象，如：`{ "Content-Type": "application/x-www-form-urlencoded" }`。
+   `data`：要发送的数据。只有在`method`为`"POST"`时才会使用。
+   `binary`：如果为`true`，`responseText`将包含未处理的二进制数据。
+   `timeout`：请求超时时间，单位为毫秒。如果在这段时间内没有收到响应，将触发`ontimeout`事件。
+   `context`：一个可以在回调函数中使用的值，这个值会在调用回调函数时作为`this`参数传入。
+   `responseType`：期望的响应类型。可能的值有`"arraybuffer"`、`"blob"`、`"json"`等。
+   `overrideMimeType`：覆盖响应的 MIME 类型。
+   `anonymous`：如果为`true`，请求将不会发送 cookies 或 HTTP 认证信息。

然后是可选的**回调函数**：

+   `onload`：请求成功时调用的函数。
+   `onerror`：请求失败时调用的函数。
+   `onreadystatechange`：当`readyState`值改变时调用的函数。
+   `ontimeout`：请求超时时调用的函数。

上述回调函数都会接收一个响应对象，包含下述属性：

+   `responseText`：服务器的响应文本。
+   `responseXML`：如果服务器的响应是一个 XML 文档，这个属性将包含该文档。
+   `response`：服务器的响应。其类型由`responseType`参数决定。
+   `status`：服务器的响应状态码。
+   `statusText`：服务器的响应状态文本。
+   `readyState`：请求的当前状态。可能的值有 0（未初始化）、1（已打开）、2（已发送头部）、3（正在接收响应）、4（已完成）。
+   `finalUrl`：响应的最终 URL，考虑到可能的重定向。

一个发起请求，并在请求成功后，显示响应文本的简单代码示例如下：

```javascript
// 浏览器发起请求
function sendRequest(url) {
    GM_xmlhttpRequest({
        method: "GET",
        url: url,
        onload: function(response) {
            console.log(response.responseText);
        }
    });
}
```

### 监听特定URL的响应数据

通过重写`XMLHttpRequest.prototype.open()`方法，在请求发起前对其进行拦截和修改。判断`url`中如果包含特定字符串，给当前的`XMLHttpRequest`对象添加一个`load`的事件监听器，请求完成时会触发回调，在这里获取响应数据。代码如下：

```javascript
// 拦截url中有特定关键词的请求并下载响应内容
function interceptAndSaveResponse(keyword) {
    var originalOpen = window.XMLHttpRequest.prototype.open;
    window.XMLHttpRequest.prototype.open = function() {
        if (arguments[1].includes(keyword)) {
            this.addEventListener('load', function() {
                console.log('监听到请求，响应结果：', this.responseText);
            }, false);
        }
        return originalOpen.apply(this, arguments);
    };
}
interceptAndSaveResponse("explore/get_explore_list")
```

保存脚本，刷新页面，控制台可以看到请求的响应结果被打印出来了：

![](img/1f5c7aba6f3e6d33e0458c8e3c1c2f39.png)

### 下载响应数据到本地

油猴提供了`GM_download`[函数](https://link.juejin.cn?target=https%3A%2F%2Fwww.tampermonkey.net%2Fdocumentation.php%23api%3AGM_download) → 使用浏览器的默认下载器进行下载，官方文档给出了代码示例：

![](img/445000e3fbaa1f91dbe1c1829e8a5e7c.png)

但这个函数只能处理`url`，不支持直接保存其它数据，这里需要先创建一个`Blob`对象并获取它的URL：



进入`/path/to/xxx`目录，找到`xxx.json`。

在表达式`cvar = avar + bvar`中，加法运算符（`+`）将`avar`与`bvar`相加，得到它们的和`cvar`。

在`List.of(arg0, arg1, arg2)`中，`List`接口的工厂方法`of()`接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用`cmd`命令：`cmd arg0 arg1 arg2`。

```javascript
// 拦截url中有特定关键词的请求并下载响应内容
function interceptAndSaveResponse(keyword) {
    var originalOpen = window.XMLHttpRequest.prototype.open;
    window.XMLHttpRequest.prototype.open = function() {
        if (arguments[1].includes(keyword)) {
            this.addEventListener('load', function() {
                console.log('监听到请求', arguments[1]);
                // 创建Blob块并获取该Blob对象的URL
                var blob = new Blob([this.responseText], {type: 'application/json;charset=utf-8'});
                var blobUrl = URL.createObjectURL(blob);
                console.log('文件的url', blobUrl);
                GM_download({
                    url: blobUrl,
                    name: "response.json",
                    saveAs: false,
                });
                // 下载完释放URL避免内存泄露
                URL.revokeObjectURL(blobUrl);
            }, false);
        }
        return originalOpen.apply(this, arguments);
    };
}
```

运行后，控制台能看到创建的`Blob`对象的`url`：

![](img/c4b6b3b3a71c97c1af07aa2efff7a22a.png)

注释掉`URL.revokeObjectURL(blobUrl)`运行，打开`url`也能看到文件内容，但文件并没有下载，应该是报错了。`GM_download`里设置下`onerror`参数，打印下下载失败的原因：

```javascript
onerror: function (error) {
    //下载错误回调
    console.log(error)
},
```

保存脚本后刷新页面，控制台输出错误信息如下：

![](img/c56c7d049b17aff6438ab34b61b3a7e6.png)

> 查询文档得知：

> 出于安全原因，文件扩展名需要在Tampermonkey的选项页面中进行白名单设置。not_whitelisted - 请求的文件扩展名未列入白名单。

我们保存文件的扩展名是`.json`，需要把它添加到白名单中，具体操作：

> 打开油猴插件的设置 → 通用 → 配置模式 → 点击下拉选中高级 → 定位到下载BETA → 文件扩展名白名单中加上`.json`

![](img/e97767ce4bc28235a1563e1e5df6cf72.png)

保存后，再次刷新页面，`json`文件就自动下载到本地啦~

![](img/0ef7e370020bf6640ba30aaac26fc67b.png)

😁 行吧，使用油猴爬XHR的关键三步都已经跑通，接着执行具体的爬取过程~

## 1. 爬取过程

### 3.1. 能搜到插件工具

[浏览器F12](https://link.juejin.cn?target=https%3A%2F%2Fwww.coze.cn%2Fstore%2Fplugin)开下抓包，访问首页商店-插件，搜索必应搜索

![](img/be1cc684588ee57c498cf3827900bf6f.png)

定位到请求url为：

```bash
/marketplace/product/list?entity_type=2&sort_type=1&page_num=1&page_size=20&keyword=&msToken=xxx
```

直接调用函数：

```javascript
interceptAndSaveResponse("marketplace/product/list?entity_type=2")
```

做了分页加载，需要手动滑下页面到底部，触发第二页的加载，合并下两个`json`文件，总共有36个插件：

![](img/141d12bb12a67ca0969af82e3c99a5e3.png)

不难看出这个`plugin_extra/tools`里的就是插件列表了

![](img/70487cac2f888c5eb76b4b8135f3f144.png)

接着写下python脚本解析下`json`，抠下想要的数据，最终结果以列表的形式输出，包含三个字段：插件工具名、所属插件、工具作用，没啥难度，直接写出Python代码：

```python
def fetch_index_plugins_info():
    json_file = 'coze_index_plugins.json'
    plugins = json.loads(read_file_text_content(json_file))['data']['products']
    result_str = "|**插件工具名**|**所属插件**|**工具作用**|\n|:----|:----|:----|\n"
    for plugin in plugins:
        meta_info = plugin['meta_info']
        for tool in plugin['plugin_extra']['tools']:
            result_str += "|**{}**|{}|{}|\n".format(tool['name'], meta_info['name'],
                                                    tool['description'].replace("\n", "\t"))
    print(result_str)
```

运行输出结果如下 (共计68个插件工具)：

### 3.2. 官方Bot工作流中的隐藏插件工具

改下脚本的传参：`interceptAndSaveResponse("explore/get_explore_list")`，刷新页面，滑动到底部，合并下两个`json`文件。这里没法拿到`Bot`的信息，主要是获取`Bot ID`，然后跳转Bot详细信息页：`explore/{Bot ID}`。抓包看到跳转详情页后，有两个接口可以获得工作流相关的信息：

![](img/be92cd2bd83d849105120484f50e4766.png)

看了下数据差不多，点击**工作流**跳转**工作流详情页**，跳转后的URL是这样的：

`work_flow?from=explore&space_id=7330891910967525417&workflow_id=7337224205123239971`

两个可能变化的参数：

+ `workflow_id`：明显就是工作流id，前面可以获取到。

+ `space_id`：工作流所属的团队空间id，打开了另外一个官方Bot的工作流，发现也是这个值，7330891910967525417估计就是扣子团队空间的id了，直接写死就好。

进入工作流详情页后，`api/workflowV2/query`请求的`schema_json`字段能拿到所有节点的信息：

![](img/88b836f35db3d5712cee77485d4cc9e2.png)

可以过滤`type=4`的插件节点（`1`为起始、`2`为结束、`3`为LLM），然后关注`subtitle`字段就行了~

![](img/9718b4bd7e80acbbb0fba94eb537b24a.png)

#### 3.2.1. 提取Bot ID

接着写代码时间，python处理下首页bot的数据，提取下bot名称和id，打印一句定义js数组的代码：

```python
def fetch_bot_ids_list():
    json_file = 'coze_index_bots_info.json'
    bots = json.loads(read_file_text_content(json_file))['data']['bot_draft_list']
    result_str = "const botInfoArray = ["
    for bot in bots:
        result_str += '"{}-{}",'.format(bot['id'], bot['name'])
    print(result_str[:-1] + "]")
```

运行输出结果：

![](img/41ab9b6dc92bea294caed03e00d0c4be.png)

#### 3.2.2. 遍历获取Bot详情数据



接着新开一个油猴脚本，CV 下上面生成的数组，这里不能直接定义`index`，因为每次页面加载时，都会重新运行脚本，从而导致死循环。需要做下跨页面状态保存，Tampermonkey 提供了两个 API → `GM_setValue` 和 `GM_getValue`，在 `@grant` 那里声明下这两个函数，就可以开搞了，直接写出 js 代码：

```javascript
// ==UserScript==
// @name         爬取Coze Bot详细信息
// @namespace    http://tampermonkey.net/
// @version      2024-02-21
// @description  try to take over the world!
// @author       CoderPig
// @match        *://*/*
// @grant        GM_xmlhttpRequest
// @grant        GM_download
// @grant        GM_setValue
// @grant        GM_getValue
// ==/UserScript==

(function() {
    'use strict';
    console.log('爬取Bot信息的插件加载...')
    const botInfoArray = ["7337223627307401231-工作规划大师", "7332475242608246821-新春萌宠大拜年", "7332511402818846755-龙年守护灵", "7332899804240150562-东北大哥拜年啦", "7332511402818666531-龙年限量盲盒", "7332907205143953442-我是一只龙", "7326151847062945818-旅游大师", "7334216846788558860-逼婚大挑战", "7330080702177787941-春联大王", "7331804785265475618-相亲模拟器(女生版)", "7325774753346650139-看图讲故事", "7330496997293752359-卡通头像", "7330306326565830682-米其林星探", "7327691806693490724-简历诊断", "7330217698355544115-小红书文案输出大师", "7329855229166600204-马歇尔音箱粉丝", "7330292680737423411-求职助手", "7329857057623031847-MBTI性格专家", "7328607141122670626-快递查询助手", "7328364654592573467-书法老师", "7326166527584157746-公司分析助手", "7330144269849886747-穿越二次元", "7332430138480607247-本子上的游戏", "7326604198378831923-数学老师", "7330070214572392485-周报不用写", "7330144975906258995-SWOT专家", "7330144975906422835-山顶洞人", "7328609700545855523-小冒险家", "7328709492403634191-购车小帮手", "7325772186130874418-论文搜索助手", "7328064633946816563-图书旅人", "7328749946491076642-天蓬元帅猪八戒", "7326127566006468617-健康饮食", "7326149183642140709-尾尾小阿姨", "7325782706489491482-足球大师", "7325765993006006282-占星师"]

    // 拦截bot信息
    function interceptRequests() {
        const originalOpen = XMLHttpRequest.prototype.open;
        XMLHttpRequest.prototype.open = function (method, url) {
            if (url.includes('draftbot/get_bot_info')) {
                this.addEventListener('load', function () {
                    var blob = new Blob([this.responseText], {type: 'application/json;charset=utf-8'});
                    var blobUrl = URL.createObjectURL(blob);
                    const currentIndex = GM_getValue('currentIndex', 0) - 1;
                    const infoArray = botInfoArray[currentIndex].split("-");
                    // 拼接文件名
                    const fileName = `${infoArray[1]}_${infoArray[0]}.json`
                    console.log('文件的url', blobUrl);
                    GM_download({
                        url: blobUrl,
                        name: fileName,
                        saveAs: false
                    });
                });
            }
            originalOpen.apply(this, arguments);
        };
    }

    // 跳转到下一个页面
    function navigateToNextUrl() {
        const currentIndex = GM_getValue('currentIndex', 0);
        if (currentIndex >= botInfoArray.length) {
            return;
        }
        // 拼接Bot详情页URL
        const nextUrl = "https://www.coze.cn/explore/"botInfoArray[currentIndex].split("-")[0];
        console.log(nextUrl);
        setTimeout(() => {
            window.location.href = nextUrl;
            GM_setValue('currentIndex', currentIndex + 1);
        }, 3000); // 3秒跳转一次
    }

    interceptRequests();
    navigateToNextUrl();
})();
```

随便打开一个新的网页，每隔 3s 会自动跳转一个页面，拦截到 `draftbot/get_bot_info` 的 `XHR` 请求会自动下载。

![](img/13d93bf0f96e248203ed4632f1e9f7f9.png)

😏 Nice，36 个全爬下来了，接着 Python 脚本遍历下有工作流的 Bot，并提取出工作流 ID~

### 3.2.3. Bot工作流ID提取

遍历 json 文件，然后抠字段而已，直接写出代码：

```python
def filter_workflow_id_list():
    # 读取所有Json文件
    json_file_list = search_all_file(os.path.join(os.getcwd(), "bot_detail"), ".json")
    for json_file in json_file_list:
        data = json.loads(read_file_text_content(json_file))['data']
        workflow_list = json.loads(data['work_info']['workflow'])
        # 只输出有工作流的Bot
        if len(workflow_list) > 0:
            # 遍历工作流列表 (看下有哪些Bot有多个工作流)
            for workflow in workflow_list:
                print("【{}】{}".format(data['name'], workflow))
```

运行结果如下：

![](img/d56f72b234cc0ea2428d56a96aac0212.png)

行吧，只有这 12 个 Bot 有工作流，而且都只有一个工作流，接着访问这些工作流详情，提取节点信息~

### 3.2.4. 提取插件节点信息

稍微改下上面的输出，返回一个 js 数组，每个字符串的组成：Bot名-工作流名称-工作流ID：

![](img/7bfab6bbb7db3620ebc5e5540d93e1fd.png)

新建一个油猴脚本，CV 下上面的代码，稍微改改：

```javascript
(function() {
    'use strict';
    console.log('爬取工作流信息的插件加载...')
    const workflowInfoArray = ["龙年守护灵-Lucky_Dragon_Girl-7332517540213768244", "图书旅人-recommend_book-7328085556632600626", "工作规划大师-creat_doc-7337224205123239971", "龙年限量盲盒-dragon-7332516682319249408", "本子上的游戏-game-7332431820975423503", "工作规划大师-creat_doc-7337224205123239971", "我是一只龙-create_image-7332914063535538216", "穿越二次元-cross-7330147381364801573", "相亲模拟器(女生版)-characters_appear-7331804700762832911", "卡通头像-Avatar001-7330497573985239081", "购车小帮手-search_car-7328713305206292495", "旅游大师-recommended_places-7326176553195339802"]

    // 拦截bot信息
    function interceptRequests() {
        const originalOpen = XMLHttpRequest.prototype.open;
        XMLHttpRequest.prototype.open = function (method, url) {
            if (url.includes('workflowV2/query')) {
                this.addEventListener('load', function () {
                    var blob = new Blob([this.responseText], {type: 'application/json;charset=utf-8'});
                    var blobUrl = URL.createObjectURL(blob);
                    const currentIndex = GM_getValue('currentIndex', 0) - 1;
                    const infoArray = workflowInfoArray[currentIndex].split("-");
                    // 拼接文件名
                    const fileName = `${infoArray[0]}_${infoArray[1]}.json`
                    console.log('文件的url', blobUrl);
                    GM_download({
                        url: blobUrl,
                        name: fileName,
                        saveAs: false
                    });
                });
            }
            originalOpen.apply(this, arguments);
        };
    }

    // 跳转到下一个页面
    function navigateToNextUrl() {
        const currentIndex = GM_getValue('currentIndex', 0);
        if (currentIndex >= workflowInfoArray.length) {
            return;
        }
        // 拼接Bot详情页URL
        const nextUrl = "https://www.coze.cn/work_flow?from=explore&space_id=7330891910967525417&workflow_id="workflowInfoArray[currentIndex].split("-")[2];
        console.log(nextUrl);
        setTimeout(() => {
            window.location.href = nextUrl;
            GM_setValue('currentIndex', currentIndex + 1);
        }, 3000); // 3秒跳转一次
    }

    interceptRequests();
    navigateToNextUrl();
})();
```

运行后，随便打开一个网页，装杯水回来就好了：

![](img/ae874563cf4f382fd8e321417ba181fe.png)

写个脚本筛一筛，看这些工作流都用到了什么插件：



```python
def filter_workflow_plugin_list():
    # 读取所有Json文件
    json_file_list = search_all_file(os.path.join(os.getcwd(), "workflow_detail"), ".json")
    for json_file in json_file_list:
        schema = json.loads(json.loads(read_file_text_content(json_file))['data']['workflow']['schema_json'])
        for node in schema['nodes']:
            # 过滤插件节点，输出子标题
            if node['type'] == "4":
                print("【{}】 → {}".format(
                    json_file.split(os.path.sep)[-1].replace(".json", ""),
                    node['data']['nodeMeta']['subtitle']
                ))
```

运行输出结果：

![](img/adf95f0485ebf7da8dcb138bc18c3707.png)

**🤡 好吧，都是能搜到的**插件工具，并没有发现隐藏插件工具，试下把`type`改为3，打印下大模型节点的模型信息，看看有没有什么惊喜：

![](img/4ef6c45a03b68ecef515de024bdfc45b.png)

😏 懂得都懂。行吧，关于内置插件工具的统计分析就到这，最后写个脚本，把每个Bot的提示词汇总下，输出一个md文件，闲着没事可以学学怎么写prompt。

```python
def generate_prompts_md():
    json_file_list = search_all_file(os.path.join(os.getcwd(), "bot_detail"), ".json")
    md_content = "# Coze(扣子)-官方Bots提示词汇总\n"
    for json_file in json_file_list:
        data = json.loads(read_file_text_content(json_file))['data']
        md_content += "\n## {}\n\n".format(data['name'])
        prompt_list = json.loads(data['work_info']['system_info_all'])
        md_content += '\n{}\n\n---\n'.format(prompt_list[0]['data'])
    write_text_to_file(md_content, "coze_bots_prompts.md")
```

生成md文件如下：

![](img/b24485bc629753da439d2d22745adade.png)

😁 取需：《`coze_bots_prompts.md`》

作者：coder_pig

链接：https://juejin.cn/post/7338229082923122703

来源：稀土掘金

# 最后

> 如果本文对你有帮助或对AI Agent感兴趣，可以联系我们或者加入我们的AI洞察中心交流群～
>
> +   当然我也很希望看到你根据本文亲手捏的应用，联系我！我一定为你点赞👍

> 参考资料：[AI Agent 智能体榜单](https://bluefocus.feishu.cn/wiki/TwW5wNDlfifjLgkE2micxthenNe)

![](img/11e373329411e84c2c7b1f9d8cc383d7.png)

官方群聊

![](img/b1aeea3308151f24412ca162c13309c8.png)

智能体搭建交流群

![](img/301896047b658f4289f2a32e43ac0e77.png)

bot互助群

> 如果您想与高手切磋，与AI爱好者一起热烈讨论，发表观点，分享作品，欢迎加入我们的官方群聊

> **智能体搭建交流群**：智能体搭建的互助交流群，群里有coze、扣子等各路技术流大佬实时解答技术问题，想交流的欢迎进群

> 如果您想与更多让自己制作的智能体冲上热门推荐页，获得更多曝光，欢迎加入由bot创建爱好者自发建立的bot互助群
