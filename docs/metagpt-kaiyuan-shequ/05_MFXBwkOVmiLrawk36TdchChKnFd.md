# （进阶）工具配置

> 来源：[https://deepwisdom.feishu.cn/docx/GnYGd8mUKoAF1YxDjQgcAjRBnKb](https://deepwisdom.feishu.cn/docx/GnYGd8mUKoAF1YxDjQgcAjRBnKb)

> 本节中，我们将学习如何为智能体提供外部工具，例如搜索网页进行 RAG、浏览网页进行爬虫

# 网页搜索 API

详情见：search_config.py

```
## Supported api_type: serpapi/google/serper/ddg
## serper: Visit https://serper.dev/ to get key.
## serpapi: Visit https://serpapi.com/ to get key.
## google: Visit https://console.cloud.google.com/apis/credentials to get key.
## ddg: it is free, no need to get key.
search:
  api_type: &#x27;google&#x27; # serpapi/google/serper/ddg
  api_key: &#x27;YOUR_API_KEY&#x27;
  cse_id: &#x27;YOUR_CSE_ID&#x27; # only for google
  params:
    engine: google # google/bing/yahoo/baidu/yandex, check https://serpapi.com/bing-search-api for more details
    google_domain: &#x27;google.com&#x27;
    gl: us
    hl: en
```

# 网页浏览

```
browser:
  engine: &#x27;playwright&#x27; # playwright/selenium
  # for playwright engine, please check https://playwright.dev/python/docs/api/class-browsertype
  # for selenium engine, please check https://www.selenium.dev/documentation/webdriver/browsers
  browser_type: &#x27;chrome&#x27; # playwright: chromium/firefox/webkit; selenium: chrome/firefox/edge/ie
```

# Azure TTS

```
azure_tts_subscription_key: &#x27;YOUR_API_KEY&#x27;
azure_tts_region: &#x27;eastus&#x27;
```

# Mermaid

详情见：[mermaid.py](https://github.com/geekan/MetaGPT/blob/main/metagpt/configs/mermaid_config.py)

```
mermaid:
  engine: &#x27;nodejs&#x27; # nodejs/ink/playwright/pyppeteer
  path: &#x27;mmdc&#x27; # such as &#x27;./node_modules/.bin/mmdc&#x27;
  puppeteer_config: &#x27;./config/puppeteer-config&#x27; # only for nodejs
  pyppeteer_path: &#x27;/usr/bin/google-chrome-stable&#x27; # only for pyppeteer
```