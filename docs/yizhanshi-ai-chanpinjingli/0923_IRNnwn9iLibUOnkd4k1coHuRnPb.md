
# 【Coze 应用】企业应收账款账龄分析

> 来源：[https://v11enp9ok1h.feishu.cn/docx/L2lndsbeiozQBPxvZmBcuGhpnyc](https://v11enp9ok1h.feishu.cn/docx/L2lndsbeiozQBPxvZmBcuGhpnyc)

## 工作流设计

### `ReportCrawl`（目前还是需要人工去做）

1. `Report` → 指「财报」
2. `Crawl` → 网络「爬取」
3. 合起来就是「财报爬取」流程。

### `Split2DB`

1. `Split` → 「拆分」文件（PDF/Excel/Word 等）
2. PDF——纯文字==》WPS
3. 纯文字==》数据抽取
4. 2 → to 的谐音，表示「到」
5. `DB` → Database
6. 数据库合起来就是「把文件拆分并写入数据库」。

### `ARAgeCalc`

1. `AR` → Accounts Receivable（应收账款）
2. `Age` → 账龄
3. `Calc` → Calculation（计算）合起来就是「应收账款账龄计算」。

## 概念

大模型 `LLM`

豆包——软件——智能体

| 姓名· | 成绩 | 时间 |
| --- | --- | --- |
| 张三 | 77 | 星期二 |
| 李思 | 33 | 星期三 |
| 张三 | 77 | 星期三 |
|  |  |  |

原本：`（77+33）/2=50`

现在：`（77+33+77）/3=62`

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

把`beijing`替换成自己的云服务地址



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
BUCKET_NAME = "coze-cs"
REGION = "cn-beijing"
OSS_ENDPOINT = "https://oss-cn-beijing.aliyuncs.com"
TIMEOUT_THRESHOLD = 60000  # 60秒超时阈值（毫秒）
MAX_DOWNLOAD_TIME = 45  # 单文件最大下载时间（秒）

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger()

# ------------- 入口函数（适配 Coze）-------------
def handler(event, context):
    start_time = time.time() * 1000

    # 兼容 Coze 的 JSON 请求体
    try:
        if isinstance(event, dict):
            evt = event
        else:
            evt = json.loads(event)
            if "body" in evt:
                evt = json.loads(evt["body"])  # 兼容 FC 的 HTTP 触发器格式
    except Exception as e:
        logger.error("❌ 请求体解析失败", exc_info=True)
        return {
            "statusCode": 400,
            "body": json.dumps({"error": "Invalid JSON body"}),
            "headers": {"Content-Type": "application/json"}
        }

    stock_code = evt.get("stock_code")
    org_id = evt.get("org_id")
    company_name = evt.get("company_name")

    if not all([stock_code, org_id, company_name]):
        return {
            "statusCode": 400,
            "body": json.dumps({"error": "缺少必要参数：stock_code, org_id, company_name"}),
            "headers": {"Content-Type": "application/json"}
        }

    # 添加时间监控
    remaining_time = context.get_remaining_time_in_millis() if hasattr(context, "get_remaining_time_in_millis") else TIMEOUT_THRESHOLD
    logger.info(f"⏱️ 开始处理 {company_name}({stock_code})，剩余时间：{remaining_time}ms")
    
    result = main(stock_code, org_id, company_name, start_time, remaining_time)
    
    elapsed = int(time.time() * 1000 - start_time)
    logger.info(f"🏁 处理完成，耗时：{elapsed}ms")
    
    return {
        "statusCode": 200,
        "body": json.dumps(result),
        "headers": {"Content-Type": "application/json"}
    }

# ------------- 主逻辑 -------------
def main(stock_code, org_id, company_name, start_time, remaining_time):
    try:
        auth = oss2.Auth(os.getenv("OSS_ACCESS_KEY_ID"), os.getenv("OSS_ACCESS_KEY_SECRET"))
        bucket = oss2.Bucket(auth, OSS_ENDPOINT, BUCKET_NAME)

        pdf_infos = fetch_annual_report_links(stock_code, org_id)
        if not pdf_infos:
            return {"downloaded": [], "message": "未找到年报信息"}

        # 按年份降序排序并取最近5年
        years = sorted(pdf_infos.keys(), reverse=True)
        target_years = years[:5]
        logger.info(f"📅 找到年报: {len(pdf_infos)} 份")
        logger.info(f"🎯 目标年份: {', '.join(target_years)}")
        
        oss_dir = f"reports/{stock_code}"
        downloaded_urls = []

        for year in target_years:
            # 超时检查
            elapsed = time.time() * 1000 - start_time
            if remaining_time - elapsed < 5000:  # 剩余时间<5秒则终止
                logger.warning("⏰ 时间不足，跳过剩余处理")
                break

            filename = f"{year}.pdf"
            oss_key = f"{oss_dir}/{filename}"
            url = f"https://{BUCKET_NAME}.oss-cn-beijing.aliyuncs.com/{oss_key}"

            # 检查OSS是否已存在
            if bucket.object_exists(oss_key):
                logger.info(f"🔄 [{year}] 年报已存在，直接使用")
                downloaded_urls.append(url)
                continue

            # 下载并上传
            local_path = f"/tmp/{stock_code}_{year}.pdf"
            try:
                logger.info(f"🚀 开始下载 {year} 年报")
                download_pdf(pdf_infos[year]["url"], local_path)
                bucket.put_object_from_file(oss_key, local_path)
                logger.info(f"☁️ [{year}] 已上传到OSS: {oss_key}")
                downloaded_urls.append(url)
            except Exception as e:
                logger.error(f"❌ [{year}] 年处理失败: {e}")
            finally:
                # 清理本地文件
                if os.path.exists(local_path):
                    os.remove(local_path)
            time.sleep(1.5)  # 避免请求过快

        return {
            "downloaded": downloaded_urls,
            "message": f"成功处理 {len(downloaded_urls)}/{len(target_years)} 份年报"
        }

    except Exception as e:
        logger.exception("🔴 主流程出错")
        return {"downloaded": [], "message": f"处理失败：{str(e)}"}

# ------------- 下载 PDF -------------
def download_pdf(url, save_path):
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36",
        "Referer": "http://www.cninfo.com.cn",
        "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8",
        "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8",
        "Connection": "keep-alive"
    }
    
    logger.info(f"📥 正在下载: {os.path.basename(save_path)}")
    start_time = time.time()
    
    with requests.Session() as session:
        resp = session.get(
            url, 
            headers=headers, 
            stream=True, 
            timeout=(10, 30)  # 连接超时10秒，读取超时30秒
        )
        resp.raise_for_status()
        
        with open(save_path, "wb") as f:
            for chunk in resp.iter_content(chunk_size=8192):
                if chunk:
                    f.write(chunk)
                
                # 超时检查
                if time.time() - start_time > MAX_DOWNLOAD_TIME:
                    raise TimeoutError(f"下载超时，已耗时{MAX_DOWNLOAD_TIME}s")
        
        elapsed = time.time() - start_time
        logger.info(f"✅ 下载成功: {save_path} (耗时 {elapsed:.1f}s)")
        return save_path

# ------------- 抓取年报链接（恢复筛选逻辑）-------------
def fetch_annual_report_links(stock_code, org_id, max_pages=2):
    api_url = "http://www.cninfo.com.cn/new/hisAnnouncement/query"
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36",
        "Referer": f"http://www.cninfo.com.cn/new/commonUrl/pageOfSearch?code={stock_code}&orgid={org_id}",
        "Origin": "http://www.cninfo.com.cn",
        "X-Requested-With": "XMLHttpRequest",
        "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8"
    }

    pdfs = {}
    found_years = set()
    
    for page_num in range(1, max_pages + 1):
        # 已找到5年则提前终止
        if len(found_years) >= 5:
            break
            
        payload = {
            "pageNum": str(page_num),
            "pageSize": "30",
            "column": "szse" if stock_code.startswith("0") else "sse",
            "tabName": "fulltext",
            "stock": f"{stock_code},{org_id}",
            "category": "category_ndbg_szsh;",  # 只查询年报
            "seDate": "",  
            "searchkey": "",
            "secid": "",
            "plate": "",
            "trade": ""
        }

        try:
            logger.info(f"🔍 正在查询第 {page_num} 页年报...")
            resp = requests.post(api_url, data=payload, headers=headers, timeout=15)
            resp.raise_for_status()
            data = resp.json()
            
            # 无数据时终止
            if not data or "announcements" not in data or not data["announcements"]:
                logger.info(f"ℹ️ 第 {page_num} 页无数据，停止搜索")
                break

            for ann in data["announcements"]:
                title = ann.get("announcementTitle", "")
                adjunct_url = ann.get("adjunctUrl", "")
                
                # 跳过不需要的公告[1](@ref)
                if (not title or 
                    "英文" in title or 
                    "摘要" in title or 
                    "已取消" in title or 
                    not adjunct_url.endswith(".PDF")):
                    continue
                    
                # 只关注年报[1,2](@ref)
                if "年度报告" in title or ("年年度报告" in title):
                    match = re.search(r"(\d{4})年", title)
                    if not match:
                        continue
                        
                    year = match.group(1)
                    
                    # 已收集5个不同年份则跳过
                    if year in found_years:
                        continue
                        
                    # 处理URL和标题
                    pdf_url = "http://static.cninfo.com.cn/" + adjunct_url
                    title_clean = re.sub(r'[\\/:"*?<>|]', '_', title)
                    
                    pdfs[year] = {
                        "title": title_clean,
                        "url": pdf_url,
                        "year": year,
                        "is_updated": ("更新后" in title or "修订后" in title)
                    }
                    found_years.add(year)
                    
                    # 已找到5个年份则终止
                    if len(found_years) >= 5:
                        break

            time.sleep(random.uniform(0.8, 1.5))  # 合理延迟，避免请求过快[5](@ref)
        except Exception as e:
            logger.exception(f"⚠️ 查询第{page_num}页失败: {e}")
            time.sleep(3)  # 错误时增加等待时间
            
    return pdfs
```



## PDF

```
{
    "url": "https://coze-cs.oss-cn-beijing.aliyuncs.com/reports/600031/2022.pdf"
}
```

`index.py`

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
            re.compile(r'组合计提项目', re.I),
            re.compile(r'按信用风险', re.I)
        )
        # 表格结束的可能标志（保持与原始代码一致）
        self.end_markers = (
            re.compile(r'合计.*?\d+.*?\d+', re.I),  # 包含合计和数字的行
            re.compile(r'单位：', re.I),  # 单位标记
            re.compile(r'续表', re.I)  # 续表标记（有时表示分页）
        )
        
        # 最大搜索页面限制
        self.max_search_pages = 500

    def _download_pdf(self, url):
        """从URL下载PDF文件"""
        try:
            response = requests.get(url, timeout=15)
            if response.status_code != 200:
                raise Exception(f"无法下载PDF: HTTP {response.status_code}")
            return BytesIO(response.content)
        except requests.RequestException as e:
            raise Exception(f"下载失败: {str(e)}")

    def _find_table_pages(self, pdf):
        """找到包含表格的所有连续页面，处理分页情况（保持与原始代码一致）"""
        table_pages = []
        in_table = False

        for idx, page in enumerate(pdf.pages, 1):
            if idx > self.max_search_pages:
                break
                
            try:
                text = page.extract_text() or ''

                # 检查是否进入表格区域
                if not in_table and all(k.search(text) for k in self.keywords):
                    in_table = True
                    table_pages.append(idx)
                # 检查是否仍在表格区域（已进入表格但未找到结束标志）
                elif in_table:
                    # 检查是否有表格结束的标志
                    end_found = any(marker.search(text) for marker in self.end_markers)
                    # 检查是否有下一个主要章节标题（通常是新的大标题）
                    if re.search(r'^[一二三四五六七八九十]+\.', text, re.M) and len(table_pages) > 0:
                        end_found = True

                    if end_found:
                        # 确保当前页也被包含
                        table_pages.append(idx)
                        break
                    else:
                        table_pages.append(idx)
            except Exception as e:
                logger.error(f"处理第 {idx} 页时出错: {str(e)}")
                continue

        return table_pages if table_pages else []

    def extract_from_url(self, url):
        """从URL提取表格所在页码"""
        try:
            # 下载PDF
            pdf_bytes = self._download_pdf(url)
            
            with pdfplumber.open(pdf_bytes) as pdf:
                # 找到表格所在页面
                pages = self._find_table_pages(pdf)
                
                # 获取文件名
                file_name = os.path.basename(url)
                
                # 尝试从文件名中提取年份
                year_match = re.search(r'20\d{2}', file_name)
                year = year_match.group() if year_match else "未知"
                
                return {
                    "url": url,
                    "file_name": file_name,
                    "year": year,
                    "pages": [str(p) for p in pages]  # 转换为字符串数组
                }
        
        except Exception as e:
            logger.error(f"提取失败: {str(e)}")
            return {"error": f"处理失败: {str(e)}"}

# 函数计算入口
def handler(event, context):
    # 初始化返回值
    response = {
        "isBase64Encoded": False,
        "statusCode": 200,
        "headers": {
            "Content-Type": "application/json",
            "Access-Control-Allow-Origin": "*"
        },
        "body": ""
    }
    
    try:
        # 解析输入参数
        event_data = {}
        
        # 处理不同格式的输入
        if isinstance(event, bytes):
            # 字节类型输入
            event_str = event.decode('utf-8')
            try:
                event_data = json.loads(event_str)
            except:
                # 尝试解析为查询字符串
                params = {}
                for pair in event_str.split('&'):
                    if '=' in pair:
                        k, v = pair.split('=', 1)
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
        if 'url' in event_data:
            url = event_data['url']
        elif 'body' in event_data and isinstance(event_data['body'], dict) and 'url' in event_data['body']:
            url = event_data['body']['url']
        elif 'queryParameters' in event_data and 'url' in event_data['queryParameters']:
            url = event_data['queryParameters']['url']
        
        # 验证URL
        if not url:
            response['statusCode'] = 400
            response['body'] = json.dumps({"error": "缺少PDF URL参数"})
            return response
        
        logger.info(f"处理URL: {url}")
        
        # 处理请求
        extractor = PageExtractor()
        result = extractor.extract_from_url(url)
        
        # 构建响应
        response['body'] = json.dumps(result, ensure_ascii=False)
        
        return response
    
    except Exception as e:
        logger.error(f"全局错误: {str(e)}")
        response['statusCode'] = 500
        response['body'] = json.dumps({"error": f"服务器错误: {str(e)}"})
        return response

# 本地测试
if __name__ == '__main__':
    # 测试用URL
    test_url = "https://coze-cs.oss-cn-beijing.aliyuncs.com/reports/000425/2023.pdf"
    
    # 测试不同输入格式
    test_cases = [
        # Coze 可能的输入格式
        {"url": test_url},  # 简单字典格式
        {"body": {"url": test_url}},  # 嵌套body格式
        f"url={test_url}".encode('utf-8'),  # 查询字符串格式
        json.dumps({"url": test_url}).encode('utf-8')  # JSON字节格式
    ]
    
    for i, test_event in enumerate(test_cases):
        print(f"\n测试用例 {i+1}: {type(test_event)}")
        test_context = {}
        
        # 调用handler
        response = handler(test_event, test_context)
        
        # 输出结果
        print("响应结果:")
        print(json.dumps(response, ensure_ascii=False, indent=2))
```
