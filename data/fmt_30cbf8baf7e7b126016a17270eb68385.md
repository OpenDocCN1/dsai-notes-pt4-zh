
# （进阶）工具配置

> 来源：[https://deepwisdom.feishu.cn/docx/GnYGd8mUKoAF1YxDjQgcAjRBnKb](https://deepwisdom.feishu.cn/docx/GnYGd8mUKoAF1YxDjQgcAjRBnKb)

> 本节中，我们将学习如何为智能体提供外部工具，例如搜索网页进行 RAG、浏览网页进行爬虫

## 网页搜索 API

详情见：`search_config.py`

```yaml
## Supported api_type: serpapi/google/serper/ddg
## serper: Visit https://serper.dev/ to get key.
## serpapi: Visit https://serpapi.com/ to get key.
## google: Visit https://console.cloud.google.com/apis/credentials to get key.
## ddg: it is free, no need to get key.
search:
  api_type: 'google' # serpapi/google/serper/ddg
  api_key: 'YOUR_API_KEY'
  cse_id: 'YOUR_CSE_ID' # only for google
  params:
    engine: google # google/bing/yahoo/baidu/yandex, check https://serpapi.com/bing-search-api for more details
    google_domain: 'google.com'
    gl: us
    hl: en
```

## 网页浏览

```yaml
browser:
  engine: 'playwright' # playwright/selenium
  # for playwright engine, please check https://playwright.dev/python/docs/api/class-browsertype
  # for selenium engine, please check https://www.selenium.dev/documentation/webdriver/browsers
  browser_type: 'chrome' # playwright: chromium/firefox/webkit; selenium: chrome/firefox/edge/ie
```

## Azure TTS

```yaml
azure_tts_subscription_key: 'YOUR_API_KEY'
azure_tts_region: 'eastus'
```

## Mermaid

详情见：`mermaid.py`

```yaml
mermaid:
  engine: 'nodejs' # nodejs/ink/playwright/pyppeteer
  path: 'mmdc' # such as './node_modules/.bin/mmdc'
  puppeteer_config: './config/puppeteer-config' # only for nodejs
  pyppeteer_path: '/usr/bin/google-chrome-stable' # only for pyppeteer
```
