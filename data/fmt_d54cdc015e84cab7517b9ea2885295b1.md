
# 2023-08-28 用ChatGPT写个爬虫爬取🛰️👸号文章

> 来源：https://xuqiwei1986.feishu.cn/docx/UlrndTcTooLXP3xpk7qc38ttn9f

> 🔗 原文链接：https://articles.zsxq.com/id_wjngo2xhrjn1.html

在赚钱看到很多圈友用ChatGPT写代码，今天分享一个我以前用ChatGPT写的爬虫。

3月底的时候，AIGC比较火，天天在知乎和🛰️里面刷相关内容，切换app比较麻烦，就想到了用爬虫去爬一些比较火的文章去读，于是想到了搜狗(搜狗里面有知乎和🛰️的搜索)，在搜狗中搜索【`AIGC`】抓取每天发布的阅读量高的文章来看。

当时ChatGPT也是🔥的不行，于是就用ChatGPT写了个爬虫做这件事，下面是实操的步骤👇🏻

## 0: 如何向ChatGPT描述我的需求？

想法有了，接下来要去用ChatGPT写代码了。那如何向他描述我的需求呢。不妨先问一下它，我怎么描述它能理解清楚我的需求。

![](img/17236d36621dbbd93710a03c18046f64.png)

如上图，ChatGPT的回答非常具体清晰，我们按它的建议提出我们的需求即可。

## 1：整理需求，交给ChatGPT生成代码

按ChatGPT给我的要点，我整理了一下我的需求

按以下要求写一个爬虫程序

1. 使用`python`和`selenium`
2. 网站 URL:`weixin.sogou.com`
3. 输入关键字 [`AIGC`]
4. 爬取标题、链接、来源
5. 抓取前10页的内容
6. 爬取的结果输出到Excel文件中,Excel文件命名为`AIGC`+🛰️+日期

如图：

![](img/1fbf2963f2572e93e6e5921af419b1f6.png)

ChatGPT生成第一版代码

```python
import os

from datetime import datetime

from selenium import webdriver

from selenium.webdriver.common.keys import Keys

from selenium.webdriver.common.by import By

from selenium.webdriver.support.ui import WebDriverWait

from selenium.webdriver.support import expected_conditions as EC

from openpyxl import Workbook

# 设置Chrome驱动器路径

chromedriver_path = "/path/to/chromedriver"

# 设置搜索关键字

keyword = "AIGC"

# 初始化Chrome驱动器

driver = webdriver.Chrome(chromedriver_path)

# 打开网站URL

driver.get(" https://weixin.sogou.com ")

# 找到搜索框并输入关键字

search_box = driver.find_element(By.ID, "query")

search_box.send_keys(keyword)

search_box.send_keys(Keys.ENTER)

# 循环爬取前10页的内容

for page in range(1, 11):

    # 等待页面加载完成

    WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.CLASS_NAME, "news-list")))

    # 获取当前页面的所有文章元素

    articles = driver.find_elements(By.CLASS_NAME, "news-list").pop().find_elements(By.TAG_NAME, "li")

    # 创建Excel工作簿

    wb = Workbook()

    # 创建工作表

    ws = wb.active

    # 设置工作表表头

    ws.append(["标题", "链接", "来源"])

    # 循环处理每篇文章

    for article in articles:

        # 获取文章标题和链接

        title = article.find_element(By.TAG_NAME, "h3").text

        link = article.find_element(By.TAG_NAME, "a").get_attribute("href")

        # 获取文章来源

        try:

            source = article.find_element(By.CLASS_NAME, "account").text

        except:

            source = ""

        # 将文章信息写入Excel工作表中

        ws.append([title, link, source])

    # 保存Excel文件

    date_str = datetime.now().strftime("%Y%m%d")

    filename = f"AIGC🛰️{date_str}.xlsx"

    wb.save(filename)

    # 点击下一页按钮

    next_btn = driver.find_element(By.CLASS_NAME, "np")

    next_btn.click()

# 关闭Chrome驱动器

driver.quit()
```

我们复制代码到PyCharm，然后执行一下，看看结果是否符合预期。

![](img/3c516d273cefdef1a24ac390f2be8ef6.png)

从截图我们可以看到，程序执行失败了。但是生成了文件。我们再看一下文件生成的数据对不对？

![](img/ea90449039be67859bb3c5e88f775b71.png)

生成的文件里面确实有内容，但是只有最后一页的数据！

## 2：跟ChatGPT互动，让它修改代码（改bug）

我把上面代码执行的结果和异常信息告诉它，看它如何回复。

![](img/0a6bff34513e67a5703cd222b0647faf.png)

这个回复对小白来说显然不友好，我先不管它。直接让它修改自己的代码。我直接把刚才的代码贴给它（为什么这么做？），让它去修改bug。

![](img/482f762d8913549854b50dd389a043ce.png)

把它写的代码贴给它后，它很显然发现了自己的错误，然后它自己修改了代码。

![](img/b86f10c70d3069979ea368f8f3e33157.png)

修改后代码

```python
import os

from datetime import datetime

from selenium import webdriver

from selenium.webdriver.common.keys import Keys

from selenium.webdriver.common.by import By

from selenium.webdriver.support.ui import WebDriverWait

from selenium.webdriver.support import expected_conditions as EC

from openpyxl import Workbook

# 设置Chrome驱动器路径

chromedriver_path = "/path/to/chromedriver"

# 设置搜索关键字

keyword = "AIGC"

# 初始化Chrome驱动器

driver = webdriver.Chrome(chromedriver_path)

# 打开网站URL

driver.get(" https://weixin.sogou.com ")

# 找到搜索框并输入关键字

search_box = driver.find_element(By.ID, "query")

search_box.send_keys(keyword)

search_box.send_keys(Keys.ENTER)

# 等待页面加载完成

WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.CLASS_NAME, "news-list")))

# 获取Excel文件名

date_str = datetime.now().strftime("%Y%m%d")

filename = f"AIGC🛰️{date_str}.xlsx"

# 创建Excel工作簿

wb = Workbook()

# 创建工作表

ws = wb.active

# 设置工作表表头

ws.append(["标题", "链接", "来源"])

# 循环爬取前10页的内容

for page in range(1, 11):

    # 获取当前页面的所有文章元素

    articles = driver.find_elements(By.CLASS_NAME, "news-list").pop().find_elements(By.TAG_NAME, "li")

    # 循环处理每篇文章

    for article in articles:

        # 获取文章标题和链接

        title = article.find_element(By.TAG_NAME, "h3").text

        link = article.find_element(By.TAG_NAME, "a").get_attribute("href")

        # 获取文章来源

        try:

            source = article.find_element(By.CLASS_NAME, "account").text

        except:

            source = ""

        # 将文章信息写入Excel工作表中

        ws.append([title, link, source])

    # 保存Excel文件

    if page == 10:

        wb.save(filename)

    # 点击下一页按钮

    try:

        next_btn = driver.find_element(By.CLASS_NAME, "np")

        next_btn.click()

    except:

        break

# 关闭Chrome驱动器

driver.quit()
```

这次我们执行代码后获取到了满意的效果，在搜狗输入【`AIGC`】后把前10页的🛰️👸号文章到抓取到了。

篇幅有限只截取一部分。

![](img/55a471edafa1d68f94b96ae3a3c0d067.png)

## 3：总结

整体效果还算满意，基本不用费太大劲就能实现我的需求。

提示词很重要，除了网上收集的模板，大家也可以用我的思路。

先去问ChatGPT怎么向他提问它才能理解你的需求。然后按它的调调给它提示词。

比较触动的是 ChatGPT能发现自己的bug并自主修改。

目前看对一点编程经验的小白来说还不能那么容易的自己用ChatGPT写代码，需要一些编程的通识。

## 4：最后

回答一下我为什么把之前写的代码直接贴给它去修改？

有人或许说他不是能记住上下文吗。

看下图，当我让它重写时它又换了一个写法去写代码。

对写代码这种需求来说，我们上一版的代码都调一半了，它在换个思路显然是不合适。

所以我把之前的代码贴给它，让它去修改，否则用重写的方案又得再调一遍比较浪费时间。

![](img/24743534586c7ede3092360072015256.png)

关于【爬取🛰️👸号文章和知乎】的代码感兴趣同学可以找我要，我私下发。

这个代码其实是通用的代码，咱们把【`AIGC`】换成你感兴趣的关键字就可以自己采集信息了。
