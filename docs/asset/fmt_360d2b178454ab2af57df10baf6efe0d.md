
# Sum4all - 总结插件

> 来源：[https://my.feishu.cn/docx/LaV1dWVsQopC7TxKJR0cZle1nrG](https://my.feishu.cn/docx/LaV1dWVsQopC7TxKJR0cZle1nrG)

## 转载自Github官网：https://github.com/fatwang2/sum4all?tab=readme-ov-file

## 不支持🛰️视频号分析，文字在链接前会走COW中的LLM，而不会走sum4all的总结

## 推荐使用Gemini，包含识图在内都可以使用，是免费的

## LLM容易出现幻觉或无法连接的情况，重试即可

### 功能特点

+ 支持联网搜索
+ 支持多轮追问
+ 支持文章链接总结，且支持发送到在线笔记
+ 支持文件内容总结，包括pdf、docx、markdown、txt、xls、csv、html、ppt
+ 支持图片总结，包括png、jpeg、jpg
+ 支持视频、播客内容总结，包括抖音、b站、📕、YouTube等
+ 支持多种内容总结服务，可自由组合
+ 支持自定义`prompt`
+ 支持自定义搜索、追问提示词

![](img/3574db35ae6fcda9b75da5f177e0d5ca.png)

![](img/b7db71977057d83f5a958822661b1969.png)

![](img/f4aaac20ebcf0a301e98d0a4497ab32e.png)

![](img/e5f545426b84eab409d3f2a35e95af07.png)

![](img/a1f6e1b2f172847092173579bdb6f3fe.png)

![](img/d57ed83a5b89244c07eef7385a73798a.png)

![](img/795239a15c06b3eccc4b9775bfee4f22.png)

![](img/dcf6728ed948b9ccd512be9acc0b4f5a.png)

### 安装

[使用管理员](https://github.com/zhayujie/chatgpt-on-wechat/tree/master/plugins/godcmd)口令在线安装，管理员认证方法见：管理员认证

```
#installp https://github.com/fatwang2/sum4all.git
```

安装成功后，根据提示使用`#scanp`命令来扫描新插件

![](img/d80ba230471e61746e223f3fcc14f9b9.png)

### 申请服务（自行选择，各有优劣）

### 配置

+ 服务器部署：复制插件目录的`config.json.template`文件,重命名为`config.json`，配置参数即可
+ docker部署：参考[项目docker部署](https://github.com/zhayujie/chatgpt-on-wechat#3-%E6%8F%92%E4%BB%B6%E4%BD%BF%E7%94%A8)的插件使用，`config.json`内增加`sum4all`插件的配置参数，操作见 docker插件配置

配置文件含义如下：

```
{
  &quot;url_sum&quot;: {
    &quot;enabled&quot;: true, #url总结服务开关
    &quot;service&quot;: &quot;sum4all&quot;, #url总结服务，目前支持openai、sum4all、gemini、bibigpt、opensum
    &quot;group&quot;: true, #url总结群聊开关
    &quot;qa_enabled&quot;:true, #url总结追问开关
    &quot;qa_prefix&quot;:&quot;问&quot;, #url总结追问前缀词
    &quot;prompt&quot;: &quot;&quot; #url总结prompt
  },
  &quot;search_sum&quot;: {
    &quot;enabled&quot;: false, #搜索总结服务开关
    &quot;service&quot;: &quot;sum4all&quot;, #搜索总结服务，目前支持openai、sum4all、gemini、perplexity
    &quot;search_service&quot;: &quot;duckduckgo&quot;, #搜索服务，duckduckgo免费，google、bing 需配置 search1api 的key
    &quot;group&quot;: true, #搜索总结群聊开关
    &quot;search_prefix&quot;:&quot;搜&quot;, #搜索总结前缀词
    &quot;prompt&quot;: &quot;&quot; #搜索总结prompt
  },
  &quot;file_sum&quot;: {
    &quot;enabled&quot;: false, #文件总结服务开关
    &quot;service&quot;: &quot;sum4all&quot;, #文件总结服务，目前支持openai、sum4all、gemini
    &quot;max_file_size&quot;: &quot;15000&quot;, #支持的文件大小，单位kb，最终能否总结取决于有大模型的token限制
    &quot;group&quot;: true, #文件总结群聊开关
    &quot;qa_prefix&quot;:&quot;问&quot;, #文件总结追问前缀词
    &quot;prompt&quot;: &quot;&quot; #文件总结prompt
  },
  &quot;image_sum&quot;: {
    &quot;enabled&quot;: false, #图片总结服务开关
    &quot;service&quot;: &quot;gemini&quot;, #图片总结服务，目前支持openai、sum4all、gemini、xunfei
    &quot;group&quot;: true, #图片总结群聊开关
    &quot;qa_prefix&quot;:&quot;问&quot;, #图片总结追问前缀词
    &quot;prompt&quot;: &quot;&quot; #图片总结prompt
  },
  &quot;note&quot;: {
    &quot;enabled&quot;: false, #笔记服务开关
    &quot;service&quot;: &quot;flomo&quot;, #笔记服务，目前支持flomo，后面考虑支持notion
    &quot;prefix&quot;:&quot;记&quot; #笔记服务前缀词
  },
  &quot;keys&quot;: {
    &quot;sum4all_key&quot;: &quot;&quot;, #如选sum4all，则必填
    &quot;search1api_key&quot;: &quot;&quot;, #如search_service选google或bing，则必填，购买方式见上
    &quot;gemini_key&quot;: &quot;&quot;, #如选gemini，则必填
    &quot;perplexity_key&quot;: &quot;&quot;, #如选perplexity，则必填
    &quot;open_ai_api_key&quot;: &quot;&quot;, #如选openai，则必填
    &quot;model&quot;: &quot;gpt-3.5-turbo&quot;, #openai模型
    &quot;open_ai_api_base&quot;: &quot;https://api.openai.com/v1&quot;, #openai请求地址
    &quot;xunfei_app_id&quot;: &quot;&quot;, #讯飞大模型appid，如选xunfei，则必填
    &quot;xunfei_api_key&quot;: &quot;&quot;, #讯飞大模型apikey，如选xunfei，则必填
    &quot;xunfei_api_secret&quot;: &quot;&quot;, #讯飞大模型apisecret，如选xunfei，则必填
    &quot;opensum_key&quot;: &quot;&quot;, #如选opensum，则必填
    &quot;bibigpt_key&quot;: &quot;&quot;, #如选bibigpt，则必填
    &quot;outputLanguage&quot;: &quot;zh-CN&quot; #bibigpt的输出语言，默认中文，其他支持列表见下
  }
}
```

bibigpt输出语言支持列表：

```
  English: &#x27;en-US&#x27;,
  中文: &#x27;zh-CN&#x27;,
  繁體中文: &#x27;zh-TW&#x27;,
  日本語: &#x27;ja-JP&#x27;,
  Italiano: &#x27;it-IT&#x27;,
  Deutsch: &#x27;de-DE&#x27;,
  Español: &#x27;es-ES&#x27;,
  Français: &#x27;fr-FR&#x27;,
  Nederlands: &#x27;nl-NL&#x27;,
  한국어: &#x27;ko-KR&#x27;,
  ភាសាខ្មែរ: &#x27;km-KH&#x27;,
  हिंदी: &#x27;hi-IN&#x27;,
```

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
