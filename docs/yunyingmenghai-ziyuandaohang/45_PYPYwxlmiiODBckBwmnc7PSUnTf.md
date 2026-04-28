# py安装依赖提示

> 来源：[https://yunyingmenghai.feishu.cn/docx/OM8ydYp27oQSROxJuhDc9U4enHe](https://yunyingmenghai.feishu.cn/docx/OM8ydYp27oQSROxJuhDc9U4enHe)

```
try:
    import pandas as pd
    import xlrd  # 添加xlrd导入
    import requests
    import json
    import time
    from bs4 import BeautifulSoup
    from urllib3.exceptions import InsecureRequestWarning
    requests.packages.urllib3.disable_warnings(InsecureRequestWarning)  # 禁用SSL警告
except ImportError:
    print(&quot;请先安装所需的库:&quot;)
    print(&quot;pip install pandas xlrd&gt;=2.0.1 requests beautifulsoup4&quot;)
    exit(1)
```