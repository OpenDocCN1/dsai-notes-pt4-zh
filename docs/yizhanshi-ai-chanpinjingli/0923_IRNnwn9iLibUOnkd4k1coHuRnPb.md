# 【Coze 应用】企业应收账款账龄分析

> 来源：[https://v11enp9ok1h.feishu.cn/docx/L2lndsbeiozQBPxvZmBcuGhpnyc](https://v11enp9ok1h.feishu.cn/docx/L2lndsbeiozQBPxvZmBcuGhpnyc)

## 工作流设计

### ReportCrawl（目前还是需要人工去做）

1.  Report → 指「财报」

1.  Crawl → 网络「爬取」

1.  合起来就是「财报爬取」流程。

### Split2DB

1.  Split → 「拆分」文件（PDF/Excel/Word 等）

1.  PDF——纯文字==》WPS

1.  纯文字==》数据抽取

1.  2 → to 的谐音，表示「到」

1.  DB → Database

1.  数据库合起来就是「把文件拆分并写入数据库」。

### ARAgeCalc

1.  AR → Accounts Receivable（应收账款）

1.  Age → 账龄

1.  Calc → Calculation（计算）合起来就是「应收账款账龄计算」。

## 概念

大模型 LLM

豆包——软件——智能体

| 姓名· | 成绩 | 时间 |
| --- | --- | --- |
| 张三 | 77 | 星期二 |
| 李思 | 33 | 星期三 |
| 张三 | 77 | 星期三 |
|  |  |  |

原本：（77+33）/2=50

现在：（77+33+77）/3=62

## 爬虫

```
{
    &quot;stock_code&quot;: &quot;600031&quot;,
    &quot;org_id&quot;: &quot;gssh0600031&quot;,
    &quot;company_name&quot;: &quot;三一重工股份有限公司&quot;
}
```

```
OSS_ACCESS_KEY_ID
```

```
OSS_ACCESS_KEY_SECRET
```

把beijing替换成自己的云服务地址

```
# -*- coding: utf-8 -*-
# index.py —— 适配 Coze HTTP 节点调用（FC 3.0 内置运行时）
import json
import os
import time
import logging
import oss2
import requests
import re
import random

# ------------- 常量 -------------
BUCKET_NAME = &quot;coze-cs&quot;
REGION = &quot;cn-beijing&quot;
OSS_ENDPOINT = &quot;https://oss-cn-beijing.aliyuncs.com&quot;
TIMEOUT_THRESHOLD = 60000  # 60秒超时阈值（毫秒）
MAX_DOWNLOAD_TIME = 45  # 单文件最大下载时间（秒）

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger()

# ------------- 入口函数（适配 Coze） -------------
def handler(event, context):
    start_time = time.time() * 1000

    # 兼容 Coze 的 JSON 请求体
    try:
        if isinstance(event, dict):
            evt = event
        else:
            evt = json.loads(event)
            if &quot;body&quot; in evt:
                evt = json.loads(evt[&quot;body&quot;])  # 兼容 FC 的 HTTP 触发器格式
    except Exception as e:
        logger.error(&quot;❌ 请求体解析失败&quot;, exc_info=True)
        return {
            &quot;statusCode&quot;: 400,
            &quot;body&quot;: json.dumps({&quot;error&quot;: &quot;Invalid JSON body&quot;}),
            &quot;headers&quot;: {&quot;Content-Type&quot;: &quot;application/json&quot;}
        }

    stock_code = evt.get(&quot;stock_code&quot;)
    org_id = evt.get(&quot;org_id&quot;)
    company_name = evt.get(&quot;company_name&quot;)

    if not all([stock_code, org_id, company_name]):
        return {
            &quot;statusCode&quot;: 400,
            &quot;body&quot;: json.dumps({&quot;error&quot;: &quot;缺少必要参数：stock_code, org_id, company_name&quot;}),
            &quot;headers&quot;: {&quot;Content-Type&quot;: &quot;application/json&quot;}
        }

    # 添加时间监控
    remaining_time = context.get_remaining_time_in_millis() if hasattr(context, &quot;get_remaining_time_in_millis&quot;) else TIMEOUT_THRESHOLD
    logger.info(f&quot;⏱️ 开始处理 {company_name}({stock_code})，剩余时间：{remaining_time}ms&quot;)
    
    result = main(stock_code, org_id, company_name, start_time, remaining_time)
    
    elapsed = int(time.time() * 1000 - start_time)
    logger.info(f&quot;🏁 处理完成，耗时：{elapsed}ms&quot;)
    
    return {
        &quot;statusCode&quot;: 200,
        &quot;body&quot;: json.dumps(result),
        &quot;headers&quot;: {&quot;Content-Type&quot;: &quot;application/json&quot;}
    }

# ------------- 主逻辑 -------------
def main(stock_code, org_id, company_name, start_time, remaining_time):
    try:
        auth = oss2.Auth(os.getenv(&quot;OSS_ACCESS_KEY_ID&quot;), os.getenv(&quot;OSS_ACCESS_KEY_SECRET&quot;))
        bucket = oss2.Bucket(auth, OSS_ENDPOINT, BUCKET_NAME)

        pdf_infos = fetch_annual_report_links(stock_code, org_id)
        if not pdf_infos:
            return {&quot;downloaded&quot;: [], &quot;message&quot;: &quot;未找到年报信息&quot;}

        # 按年份降序排序并取最近5年
        years = sorted(pdf_infos.keys(), reverse=True)
        target_years = years[:5]
        logger.info(f&quot;📅 找到年报: {len(pdf_infos)} 份&quot;)
        logger.info(f&quot;🎯 目标年份: {&#x27;, &#x27;.join(target_years)}&quot;)
        
        oss_dir = f&quot;reports/{stock_code}&quot;
        downloaded_urls = []

        for year in target_years:
            # 超时检查
            elapsed = time.time() * 1000 - start_time
            if remaining_time - elapsed &lt; 5000:  # 剩余时间&lt;5秒则终止
                logger.warning(&quot;⏰ 时间不足，跳过剩余处理&quot;)
                break

            filename = f&quot;{year}.pdf&quot;
            oss_key = f&quot;{oss_dir}/{filename}&quot;
            url = f&quot;https://{BUCKET_NAME}.oss-cn-beijing.aliyuncs.com/{oss_key}&quot;

            # 检查OSS是否已存在
            if bucket.object_exists(oss_key):
                logger.info(f&quot;🔄 [{year}] 年报已存在，直接使用&quot;)
                downloaded_urls.append(url)
                continue

            # 下载并上传
            local_path = f&quot;/tmp/{stock_code}_{year}.pdf&quot;
            try:
                logger.info(f&quot;🚀 开始下载 {year} 年报&quot;)
                download_pdf(pdf_infos[year][&quot;url&quot;], local_path)
                bucket.put_object_from_file(oss_key, local_path)
                logger.info(f&quot;☁️ [{year}] 已上传到OSS: {oss_key}&quot;)
                downloaded_urls.append(url)
            except Exception as e:
                logger.error(f&quot;❌ [{year}] 年处理失败: {e}&quot;)
            finally:
                # 清理本地文件
                if os.path.exists(local_path):
                    os.remove(local_path)
            time.sleep(1.5)  # 避免请求过快

        return {
            &quot;downloaded&quot;: downloaded_urls,
            &quot;message&quot;: f&quot;成功处理 {len(downloaded_urls)}/{len(target_years)} 份年报&quot;
        }

    except Exception as e:
        logger.exception(&quot;🔴 主流程出错&quot;)
        return {&quot;downloaded&quot;: [], &quot;message&quot;: f&quot;处理失败：{str(e)}&quot;}

# ------------- 下载 PDF -------------
def download_pdf(url, save_path):
    headers = {
        &quot;User-Agent&quot;: &quot;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36&quot;,
        &quot;Referer&quot;: &quot;http://www.cninfo.com.cn&quot;,
        &quot;Accept&quot;: &quot;text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8&quot;,
        &quot;Accept-Language&quot;: &quot;zh-CN,zh;q=0.9,en;q=0.8&quot;,
        &quot;Connection&quot;: &quot;keep-alive&quot;
    }
    
    logger.info(f&quot;📥 正在下载: {os.path.basename(save_path)}&quot;)
    start_time = time.time()
    
    with requests.Session() as session:
        resp = session.get(
            url, 
            headers=headers, 
            stream=True, 
            timeout=(10, 30)  # 连接超时10秒，读取超时30秒
        )
        resp.raise_for_status()
        
        with open(save_path, &quot;wb&quot;) as f:
            for chunk in resp.iter_content(chunk_size=8192):
                if chunk:
                    f.write(chunk)
                
                # 超时检查
                if time.time() - start_time &gt; MAX_DOWNLOAD_TIME:
                    raise TimeoutError(f&quot;下载超时，已耗时{MAX_DOWNLOAD_TIME}s&quot;)
        
        elapsed = time.time() - start_time
        logger.info(f&quot;✅ 下载成功: {save_path} (耗时 {elapsed:.1f}s)&quot;)
        return save_path

# ------------- 抓取年报链接（恢复筛选逻辑）-------------
def fetch_annual_report_links(stock_code, org_id, max_pages=2):
    api_url = &quot;http://www.cninfo.com.cn/new/hisAnnouncement/query&quot;
    headers = {
        &quot;User-Agent&quot;: &quot;Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36&quot;,
        &quot;Referer&quot;: f&quot;http://www.cninfo.com.cn/new/commonUrl/pageOfSearch?code={stock_code}&amp;orgid={org_id}&quot;,
        &quot;Origin&quot;: &quot;http://www.cninfo.com.cn&quot;,
        &quot;X-Requested-With&quot;: &quot;XMLHttpRequest&quot;,
        &quot;Content-Type&quot;: &quot;application/x-www-form-urlencoded; charset=UTF-8&quot;
    }

    pdfs = {}
    found_years = set()
    
    for page_num in range(1, max_pages + 1):
        # 已找到5年则提前终止
        if len(found_years) &gt;= 5:
            break
            
        payload = {
            &quot;pageNum&quot;: str(page_num),
            &quot;pageSize&quot;: &quot;30&quot;,
            &quot;column&quot;: &quot;szse&quot; if stock_code.startswith(&quot;0&quot;) else &quot;sse&quot;,
            &quot;tabName&quot;: &quot;fulltext&quot;,
            &quot;stock&quot;: f&quot;{stock_code},{org_id}&quot;,
            &quot;category&quot;: &quot;category_ndbg_szsh;&quot;,  # 只查询年报
            &quot;seDate&quot;: &quot;&quot;,  
            &quot;searchkey&quot;: &quot;&quot;,
            &quot;secid&quot;: &quot;&quot;,
            &quot;plate&quot;: &quot;&quot;,
            &quot;trade&quot;: &quot;&quot;
        }

        try:
            logger.info(f&quot;🔍 正在查询第 {page_num} 页年报...&quot;)
            resp = requests.post(api_url, data=payload, headers=headers, timeout=15)
            resp.raise_for_status()
            data = resp.json()
            
            # 无数据时终止
            if not data or &quot;announcements&quot; not in data or not data[&quot;announcements&quot;]:
                logger.info(f&quot;ℹ️ 第 {page_num} 页无数据，停止搜索&quot;)
                break

            for ann in data[&quot;announcements&quot;]:
                title = ann.get(&quot;announcementTitle&quot;, &quot;&quot;)
                adjunct_url = ann.get(&quot;adjunctUrl&quot;, &quot;&quot;)
                
                # 跳过不需要的公告[1](@ref)
                if (not title or 
                    &quot;英文&quot; in title or 
                    &quot;摘要&quot; in title or 
                    &quot;已取消&quot; in title or 
                    not adjunct_url.endswith(&quot;.PDF&quot;)):
                    continue
                    
                # 只关注年报[1,2](@ref)
                if &quot;年度报告&quot; in title or (&quot;年年度报告&quot; in title):
                    match = re.search(r&quot;(\d{4})年&quot;, title)
                    if not match:
                        continue
                        
                    year = match.group(1)
                    
                    # 已收集5个不同年份则跳过
                    if year in found_years:
                        continue
                        
                    # 处理URL和标题
                    pdf_url = &quot;http://static.cninfo.com.cn/&quot; + adjunct_url
                    title_clean = re.sub(r&#x27;[\\/:&quot;*?&lt;&gt;|]&#x27;, &#x27;_&#x27;, title)
                    
                    pdfs[year] = {
                        &quot;title&quot;: title_clean,
                        &quot;url&quot;: pdf_url,
                        &quot;year&quot;: year,
                        &quot;is_updated&quot;: (&quot;更新后&quot; in title or &quot;修订后&quot; in title)
                    }
                    found_years.add(year)
                    
                    # 已找到5个年份则终止
                    if len(found_years) &gt;= 5:
                        break

            time.sleep(random.uniform(0.8, 1.5))  # 合理延迟，避免请求过快[5](@ref)
        except Exception as e:
            logger.exception(f&quot;⚠️ 查询第{page_num}页失败: {e}&quot;)
            time.sleep(3)  # 错误时增加等待时间
            
    return pdfs
```

## PDF

```
{
    &quot;url&quot;: &quot;https://coze-cs.oss-cn-beijing.aliyuncs.com/reports/600031/2022.pdf&quot;
}
```

index.py

```
import os
import re
import json
import pdfplumber
import requests
import logging
from io import BytesIO

# 配置日志
logger = logging.getLogger()
logger.setLevel(logging.INFO)

class PageExtractor:
    def __init__(self):
        # 关键词用于定位表格所在页面（保持与原始代码一致）
        self.keywords = (
            re.compile(r&#x27;组合计提项目&#x27;, re.I),
            re.compile(r&#x27;按信用风险&#x27;, re.I)
        )
        # 表格结束的可能标志（保持与原始代码一致）
        self.end_markers = (
            re.compile(r&#x27;合计.*?\d+.*?\d+&#x27;, re.I),  # 包含合计和数字的行
            re.compile(r&#x27;单位：&#x27;, re.I),  # 单位标记
            re.compile(r&#x27;续表&#x27;, re.I)  # 续表标记（有时表示分页）
        )
        
        # 最大搜索页面限制
        self.max_search_pages = 500

    def _download_pdf(self, url):
        &quot;&quot;&quot;从URL下载PDF文件&quot;&quot;&quot;
        try:
            response = requests.get(url, timeout=15)
            if response.status_code != 200:
                raise Exception(f&quot;无法下载PDF: HTTP {response.status_code}&quot;)
            return BytesIO(response.content)
        except requests.RequestException as e:
            raise Exception(f&quot;下载失败: {str(e)}&quot;)

    def _find_table_pages(self, pdf):
        &quot;&quot;&quot;找到包含表格的所有连续页面，处理分页情况（保持与原始代码一致）&quot;&quot;&quot;
        table_pages = []
        in_table = False

        for idx, page in enumerate(pdf.pages, 1):
            if idx &gt; self.max_search_pages:
                break
                
            try:
                text = page.extract_text() or &#x27;&#x27;

                # 检查是否进入表格区域
                if not in_table and all(k.search(text) for k in self.keywords):
                    in_table = True
                    table_pages.append(idx)
                # 检查是否仍在表格区域（已进入表格但未找到结束标志）
                elif in_table:
                    # 检查是否有表格结束的标志
                    end_found = any(marker.search(text) for marker in self.end_markers)
                    # 检查是否有下一个主要章节标题（通常是新的大标题）
                    if re.search(r&#x27;^[一二三四五六七八九十]+\.&#x27;, text, re.M) and len(table_pages) &gt; 0:
                        end_found = True

                    if end_found:
                        # 确保当前页也被包含
                        table_pages.append(idx)
                        break
                    else:
                        table_pages.append(idx)
            except Exception as e:
                logger.error(f&quot;处理第 {idx} 页时出错: {str(e)}&quot;)
                continue

        return table_pages if table_pages else []

    def extract_from_url(self, url):
        &quot;&quot;&quot;从URL提取表格所在页码&quot;&quot;&quot;
        try:
            # 下载PDF
            pdf_bytes = self._download_pdf(url)
            
            with pdfplumber.open(pdf_bytes) as pdf:
                # 找到表格所在页面
                pages = self._find_table_pages(pdf)
                
                # 获取文件名
                file_name = os.path.basename(url)
                
                # 尝试从文件名中提取年份
                year_match = re.search(r&#x27;20\d{2}&#x27;, file_name)
                year = year_match.group() if year_match else &quot;未知&quot;
                
                return {
                    &quot;url&quot;: url,
                    &quot;file_name&quot;: file_name,
                    &quot;year&quot;: year,
                    &quot;pages&quot;: [str(p) for p in pages]  # 转换为字符串数组
                }
        
        except Exception as e:
            logger.error(f&quot;提取失败: {str(e)}&quot;)
            return {&quot;error&quot;: f&quot;处理失败: {str(e)}&quot;}

# 函数计算入口
def handler(event, context):
    # 初始化返回值
    response = {
        &quot;isBase64Encoded&quot;: False,
        &quot;statusCode&quot;: 200,
        &quot;headers&quot;: {
            &quot;Content-Type&quot;: &quot;application/json&quot;,
            &quot;Access-Control-Allow-Origin&quot;: &quot;*&quot;
        },
        &quot;body&quot;: &quot;&quot;
    }
    
    try:
        # 解析输入参数
        event_data = {}
        
        # 处理不同格式的输入
        if isinstance(event, bytes):
            # 字节类型输入
            event_str = event.decode(&#x27;utf-8&#x27;)
            try:
                event_data = json.loads(event_str)
            except:
                # 尝试解析为查询字符串
                params = {}
                for pair in event_str.split(&#x27;&amp;&#x27;):
                    if &#x27;=&#x27; in pair:
                        k, v = pair.split(&#x27;=&#x27;, 1)
                        params[k] = v
                event_data = params
        elif isinstance(event, str):
            # 字符串类型输入
            try:
                event_data = json.loads(event)
            except:
                pass
        elif isinstance(event, dict):
            # 字典类型输入
            event_data = event
        
        # 从不同位置获取URL
        url = None
        if &#x27;url&#x27; in event_data:
            url = event_data[&#x27;url&#x27;]
        elif &#x27;body&#x27; in event_data and isinstance(event_data[&#x27;body&#x27;], dict) and &#x27;url&#x27; in event_data[&#x27;body&#x27;]:
            url = event_data[&#x27;body&#x27;][&#x27;url&#x27;]
        elif &#x27;queryParameters&#x27; in event_data and &#x27;url&#x27; in event_data[&#x27;queryParameters&#x27;]:
            url = event_data[&#x27;queryParameters&#x27;][&#x27;url&#x27;]
        
        # 验证URL
        if not url:
            response[&#x27;statusCode&#x27;] = 400
            response[&#x27;body&#x27;] = json.dumps({&quot;error&quot;: &quot;缺少PDF URL参数&quot;})
            return response
        
        logger.info(f&quot;处理URL: {url}&quot;)
        
        # 处理请求
        extractor = PageExtractor()
        result = extractor.extract_from_url(url)
        
        # 构建响应
        response[&#x27;body&#x27;] = json.dumps(result, ensure_ascii=False)
        
        return response
    
    except Exception as e:
        logger.error(f&quot;全局错误: {str(e)}&quot;)
        response[&#x27;statusCode&#x27;] = 500
        response[&#x27;body&#x27;] = json.dumps({&quot;error&quot;: f&quot;服务器错误: {str(e)}&quot;})
        return response

# 本地测试
if __name__ == &#x27;__main__&#x27;:
    # 测试用URL
    test_url = &quot;https://coze-cs.oss-cn-beijing.aliyuncs.com/reports/000425/2023.pdf&quot;
    
    # 测试不同输入格式
    test_cases = [
        # Coze 可能的输入格式
        {&quot;url&quot;: test_url},  # 简单字典格式
        {&quot;body&quot;: {&quot;url&quot;: test_url}},  # 嵌套body格式
        f&quot;url={test_url}&quot;.encode(&#x27;utf-8&#x27;),  # 查询字符串格式
        json.dumps({&quot;url&quot;: test_url}).encode(&#x27;utf-8&#x27;)  # JSON字节格式
    ]
    
    for i, test_event in enumerate(test_cases):
        print(f&quot;\n测试用例 {i+1}: {type(test_event)}&quot;)
        test_context = {}
        
        # 调用handler
        response = handler(test_event, test_context)
        
        # 输出结果
        print(&quot;响应结果:&quot;)
        print(json.dumps(response, ensure_ascii=False, indent=2))
```