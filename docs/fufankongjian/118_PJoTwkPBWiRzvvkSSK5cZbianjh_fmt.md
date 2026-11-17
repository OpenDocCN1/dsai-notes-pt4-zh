
# GPT-4o实战：多模态知识库+多模态机器人

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/TpWkde3t1oeR2SxWmR2ctzGHn1f](https://kq4b3vgg5b.feishu.cn/docx/TpWkde3t1oeR2SxWmR2ctzGHn1f)

```
import os
import openai

openai.api_key = os.getenv("OPENAI_API_KEY") #调用你的API keys
from openai import OpenAI #导入OpenAI
client = OpenAI() #与OpenAI产生链接
```

```
response = client.chat.completions.create(
  model="gpt-4o",
  messages=[{"role": "user","content": "你好呀GPT！"}],
  max_tokens=300,
)
```

```
print(response.choices[0].message.content)
```

```
你好！你今天过得怎么样？有什么我可以帮你的吗？
```

```
# 使用GPT-4o描述图像
response = openai.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "user", "content": [
            {"type": "text", "text": "请描述以下图片的内容"},
            {"type": "image_url"
             , "image_url": {"url": 
        ,{"type": "image_url"
             , "image_url": {"url":  #改掉下面这行 ↓
        ]}
    ],
    max_tokens=300,
)

description = response.choices[0].message.content
print(description)
```

```
这张图片展示了一种办公场景：

- 中央是一台蓝色的电脑显示器，上面显示着图表和其他数据图形。
- 左侧是一位穿黄色上衣和黑色裤子的女性，她站在一个装有文件的橙色文件夹旁边，文件夹上有一些文件。
- 右侧是一位穿蓝色外套和黑裤子的男性，他靠近显示器，伸手指向显示器的内容。
- 背景中还有一些齿轮图案，象征着机械和系统的运转。
- 显示器旁边有一个绿色的植物。

这幅插画一般用于描述与数据分析、办公、团队协作相关的场景。
```

![](img/296301718c8f6ec51a72a24c6dbcf48c.png)

## 处理多模态数据

### 使用PDF解析库提取文本和表格

```
pdf_path = r"D:\pythonwork\LLM\Course\annual-and-transition-report-of-foreign-private-issuers-sections-13-or-15-d.pdf"
```

```
import pdfplumber

def extract_text_and_tables_from_pdf(pdf_path):
    with pdfplumber.open(pdf_path) as pdf:
        texts = []
        tables = []
        #取出从190-200页的信息
        #这里是核心财务信息
        for page in pdf.pages[190:200]: 
            texts.append(page.extract_text())
            tables.append(page.extract_tables())
    return texts, tables

texts, tables = extract_text_and_tables_from_pdf(pdf_path)
```

```
texts[:200]
```



# Bilibili Inc. Consolidated Financial Statements

## Consolidated Balance Sheets (Continued)

### F-5

```
Table of Contents
BILIBILI INC.
CONSOLIDATED BALANCE SHEETS (Continued)
(All amounts in thousands, except for share data)
December 31, December 31, December 31,
2022 2023 2023
RMB RMB US$
Note 2(e)
Shareholders’ equity
Ordinary shares:
Class Y Ordinary Shares (US$0.0001 par value; 100,000,000 shares authorized, 83,715,114 shares issued and outstanding as of December 31, 2022; US$0.0001 par value; 100,000,000 shares authorized, 83,715,114 shares issued and outstanding as of December 31, 2023) 52 52 7
Class Z Ordinary Shares (US$0.0001 par value; 9,800,000,000 shares authorized, 316,202,303 shares issued, 310,864,471 shares outstanding as of December 31, 2022; 9,800,000,000 shares authorized, 337,546,303 shares issued, 328,441,712 shares outstanding as of December 31, 2023) 201 213 30
Additional paid-in capital 36,623,161 40,445,175 5,696,584
Statutory reserves 36,173 44,749 6,303
Accumulated other comprehensive income 58,110 212,477 29,927
Accumulated deficit (21,479,869) (26,310,766) (3,705,794)
Total Bilibili Inc.’s shareholders’ equity 15,237,828 14,391,900 2,027,057
Noncontrolling interests 1,759 12,367 1,742
Total shareholders’ equity 15,239,587 14,404,267 2,028,799
Total liabilities and shareholders’ equity 41,830,570 33,159,067 4,670,357
```

The accompanying notes are an integral part of these consolidated financial statements.

F-5

## Consolidated Statements of Operations and Comprehensive Loss

### F-6

```
Table of Contents
BILIBILI INC.
CONSOLIDATED STATEMENTS OF OPERATIONS AND COMPREHENSIVE LOSS
(All amounts in thousands, except for share and per share data)
For the Year Ended December 31,
2021 2022 2023 2023
RMB RMB RMB US$
Note 2(e)
Net revenues 19,383,684 21,899,167 22,527,987 3,173,001
Cost of revenues (15,340,537 ) (18,049,872 ) (17,086,122 ) (2,406,530 )
Gross profit 4,043,147 3,849,295 5,441,865 766,471
Operating expenses:
Sales and marketing expenses (5,794,853) (4,920,745) (3,916,150) (551,578)
General and administrative expenses (1,837,506) (2,521,134) (2,122,432) (298,938)
Research and development expenses (2,839,862) (4,765,360) (4,467,470) (629,230)
Total operating expenses (10,472,221 ) (12,207,239 ) (10,506,052 ) (1,479,746 )
Loss from operations (6,429,074 ) (8,357,944 ) (5,064,187 ) (713,275 )
Other (expense)/income:
Investment loss, net (including impairments) (194,183) (532,485) (435,644) (61,359)
Interest income 70,367 281,051 542,472 76,406
Interest expense (155,467) (250,923) (164,927) (23,229)
Exchange losses (15,504) (19,745) (35,575) (5,011)
Debt extinguishment gain — 1,318,594 292,213 41,157
Others, net 10,411 157,944 132,640 18,682
Total other (expense)/income, net (284,376 ) 954,436 331,179 46,646
Loss before income tax expenses (6,713,450 ) (7,403,508 ) (4,733,008 ) (666,629 )
Income tax (95,289) (104,145) (78,705) (11,085)
Net loss (6,808,739 ) (7,507,653 ) (4,811,713 ) (677,714 )
Net loss/(profit) attributable to noncontrolling interests 19,511 10,640 (10,608) (1,494)
Net loss attributable to the Bilibili Inc.’s shareholders (6,789,228 ) (7,497,013 ) (4,822,321 ) (679,208 )
Net loss (6,808,739 ) (7,507,653 ) (4,811,713 ) (677,714 )
Other comprehensive (loss)/income:
Foreign currency translation adjustments (420,991) 337,972 154,367 21,742
Total other comprehensive (loss)/income (420,991) 337,972 154,367 21,742
Total comprehensive loss (7,229,730 ) (7,169,681 ) (4,657,346 ) (655,972 )
Comprehensive loss/(income) attributable to noncontrolling interests 19,511 10,640 (10,608 ) (1,494 )
Comprehensive loss attributable to the Bilibili Inc.’s shareholders (7,210,219 ) (7,159,041 ) (4,667,954 ) (657,466 )
Net loss per share, basic (17.87 ) (18.99 ) (11.67 ) (1.64 )
Net loss per share, diluted (17.87) (18.99) (11.67) (1.64)
Net loss per ADS, basic (17.87) (18.99) (11.67) (1.64)
Net loss per ADS, diluted (17.87) (18.99) (11.67) (1.64)
Weighted average number of ordinary shares, basic 379,898,121 394,863,584 413,210,271 413,210,271
Weighted average number of ordinary shares, diluted 379,898,121 394,863,584 413,210,271 413,210,271
Weighted average number of ADS, basic 379,898,121 394,863,584 413,210,271 413,210,271
Weighted average number of ADS, diluted 379,898,121 394,863,584 413,210,271 413,210,271
Share-based compensation expenses included in:
Cost of revenues 76,232 69,096 63,724 8,975
Sales and marketing expenses 53,452 59,041 56,649 7,979
General and administrative expenses 553,526 554,976 596,950 84,079
Research and development expenses 316,607 357,570 415,321 58,497
```

The accompanying notes are an integral part of these consolidated financial statements.

F-6

## Consolidated Statements of Changes in Shareholders’ Equity

### F-7

```
Table of Contents
BILIBILI INC.
CONSOLIDATED STATEMENTS OF CHANGES IN SHAREHOLDERS’ EQUITY
(All amounts in thousands, except for share data)
Ordinary shares
Class Y Ordinary Class Z Ordinary Additional Accumulated other Total
Shares Shares paid-in Statutory comprehensive Accumulated Noncontrolling sharehold ers’
Shares Amount Shares Amount capital reserves income/(loss) deficit interests equity
RMB RMB RMB RMB RMB RMB RMB RMB
Balance at December 31, 2020 83,715,114 52 268,204,838 172 14,616,302 17,884 141,129 (7,175,339) 182,004 7,782,204
Net loss — — — — — — — (6,789,228) (19,511) (6,808,739)
Share-based compensation — — — — 999,817 — — — — 999,817
Share issuance from exercise of
share options — — 3,262,562 3 — — — — — 3
Share issuance upon secondary
public offering (“HK IPO”), net
of issuance costs of
HKD337,143 — — 28,750,000 18 19,266,792 — — — — 19,266,810
Acquisition of subsidiaries — — 2,056,825 1 632,747 — — — (14,749) 617,999
Issuance of Class Z ordinary
shares related to long-term
investments — — 1,045,700 1 (1) — — — — —
Share issuance upon the
conversion of convertible senior
notes — — 2,854,277 3 449,908 — — — — 449,911
Capital injection in subsidiaries by
noncontrolling interests — — — — — — — — 2,187 2,187
Appropriation to statutory reserves — — — — — 6,737 — (6,737) — —
Purchase of noncontrolling
interests — — 715,271 1 (35,604) — — — (137,532) (173,135)
Foreign currency translation
adjustment — — — — — — (420,991) — — (420,991)
Balance at December 31, 2021 83,715,114 52 306,889,473 199 35,929,961 24,621 (279,862 ) (13,971,304 ) 12,399 21,716,066
```

The accompanying notes are an integral part of these consolidated financial statements.

F-7

### F-8 (Continued)

```
Table of Contents
BILIBILI INC.
CONSOLIDATED STATEMENTS OF CHANGES IN SHAREHOLDERS’ EQUITY (Continued)
(All amounts in thousands, except for share data)
Ordinary shares
Class Y Ordinary Class Z Ordinary Additional Accumulated other Total
Shares Shares paid-in Statutory comprehensive (loss)/income Accumulated Noncontrolling sharehold ers’
Shares Amount Shares Amount capital reserves deficit interests equity
RMB RMB RMB RMB RMB RMB RMB RMB
Balance at December 31, 2021 83,715,114 52 306,889,473 199 35,929,961 24,621 (279,862) (13,971,304) 12,399 21,716,066
Net loss — — — — — — — (7,497,013) (10,640) (7,507,653)
Share-based compensation — — — — 1,040,683 — — — — 1,040,683
Share issuance from exercise of
share options — — 3,929,433 4 — — — — — 4
Purchase of noncontrolling
interests — — 45,000 * * — — — — —
Share issuance upon the
conversion of convertible
senior notes — — 565 * 96 — — — — 96
Repurchase of shares — — (2,640,832) (2) (347,579) — — — — (347,581)
Appropriation to statutory
reserves — — — — — 11,552 — (11,552) — —
Foreign currency translation
adjustments — — — — — — 337,972 — — 337,972
Balance at December 31, 2022 83,715,114 52 308,223,639 201 36,623,161 36,173 58,110 (21,479,869 ) 1,759 15,239,587
```

* Less than 1.
The accompanying notes are an integral part of these consolidated financial statements.

F-8

### F-9 (Continued)

```
Table of Contents
BILIBILI INC.
CONSOLIDATED STATEMENTS OF CHANGES IN SHAREHOLDERS’ EQUITY (Continued)
(All amounts in thousands, except for share data)
Ordinary shares
Class Y Ordinary Class Z Ordinary Additional Accumulated other Total
Shares Shares paid-in Statutory comprehensive Accumulated Noncontrolling sharehold ers’
Shares Amount Shares Amount capital reserves income deficit interests equity
RMB RMB RMB RMB RMB RMB RMB RMB
Balance at December 31, 2022 83,715,114 52 308,223,639 201 36,623,161 36,173 58,110 (21,479,869) 1,759 15,239,587
Net (loss)/income — — — — — — — (4,822,321) 10,608 (4,811,713)
Share-based compensation — — — — 1,132,644 — — — — 1,132,644
Issuance of Class Z ordinary shares
upon new ADS offering (“ADS
offering”) — — 15,344,000 10 2,689,370 — — — — 2,689,380
Share issuance from exercise of share
options — — 2,210,741 2 — — — — — 2
Share issuance from vest of restricted
share units — — 22,500 * — — — — — *
Appropriation to statutory reserves — — — — — 8,576 — (8,576) — —
Foreign currency translation
adjustments — — — — — — 154,367 — — 154,367
Balance at December 31, 2023 83,715,114 52 325,800,880 213 40,445,175 44,749 212,477 (26,310,766 ) 12,367 14,404,267
```

* Less than 1.
The accompanying notes are an integral part of these consolidated financial statements.

F-9

## Consolidated Statements of Cash Flows

### F-10

```
Table of Contents
BILIBILI INC.
CONSOLIDATED STATEMENTS OF CASH FLOWS
(All amounts in thousands)
For the Year Ended December 31,
2021 2022 2023 2023
RMB RMB RMB US$
Note 2(e)
Cash flows from operating activities:
Net loss (6,808,739) (7,507,653) (4,811,713) (677,714)
Adjustments to reconcile net loss to net cash (used in)/provided by operating activities:
Depreciation of property and equipment 538,601 755,452 727,193 102,423
Amortization of intangible assets 1,903,226 2,581,325 2,003,199 282,145
Amortization of right-of-use assets 161,873 229,295 169,294 23,845
Amortization of debt issuance costs 25,234 48,167 23,299 3,282
Share-based compensation expenses 999,817 1,040,683 1,132,644 159,530
Allowance for/(reversal of) expected credit losses 189,165 130,549 (16,000) (2,254)
Inventory provision 24,454 191,088 10,431 1,469
Deferred income taxes (21,492) (36,495) (25,376) (3,574)
Unrealized exchange (gains)/losses (6,592) 2,277 931 131
Unrealized fair value changes of investments 200,274 251,383 66,462 9,361
Loss on disposal of property and equipment 611 748 646 91
Termination expenses of certain game projects — 525,762 354,811 49,974
Gain from disposal of subsidiaries and long-term investments (4,413) (178,378) (3,857) (543)
Loss from equity method investments 37,179 211,620 112,142 15,795
Revaluation of previously held equity interests 31,462 (152,153) 86,680 12,209
Impairments of long-term investments 91,493 465,645 278,891 39,281
Gain of convertible senior notes repurchase — (1,318,594) (292,213) (41,157)
Changes in operating assets and liabilities:
Accounts receivable (429,460) (59,866) (262,215) (36,932)
Amount due from related parties 8,792 (74,117) 75,273 10,602
Prepayments and other assets (1,747,744) (556,214) 342,537 48,245
Other long-term assets (138,396) (444,326) 185,783 26,167
Accounts payable 1,056,847 46,862 (60,458) (8,517)
Salary and welfare payable 254,213 396,113 (182,171) (25,658)
Taxes payable 77,365 130,684 29,006 4,085
Deferred revenue 494,551 173,935 133,902 18,859
Accrued liabilities and other payables 319,702 (1,026,319) 341,102 48,042
Amount due to related parties — (21,210) (12,007) (1,691)
Other long-term liabilities 94,969 282,367 (141,594) (19,943)
Net cash (used in)/provided by operating activities (2,647,008 ) (3,911,370 ) 266,622 37,553
Cash flows from investing activities:
Purchase of property and equipment (965,410) (760,427) (181,897) (25,620)
Purchase of intangible assets (2,721,799) (1,977,897) (1,148,247) (161,727)
Purchase of short-term investments (71,748,847) (70,578,711) (13,547,650) (1,908,147)
Maturities of short-term investments 60,524,888 81,698,532 16,328,255 2,299,787
Cash consideration paid for purchase of subsidiaries, net of cash acquired (521,984) (1,179,764) (70,000) (9,859)
Cash paid for long-term investments including loans (6,716,491) (1,466,311) (132,602) (18,677)
Repayment of loans from investees 539,225 596,766 684,834 96,457
Cash received from disposal/return of long-term assets 74,604 612,214 100,574 14,166
Placements of time deposits (10,697,444) (10,245,026) (9,961,925) (1,403,108)
Maturities of time deposits 7,655,147 13,909,967 9,690,806 1,364,921
Impact to cash resulting from deconsolidation of subsidiaries — (125) — —
Net cash (used in)/provided by investing activities (24,578,111 ) 10,609,218 1,762,148 248,193
Cash flows from financing activities:
Proceeds of short-term loan 1,332,597 1,701,532 1,950,482 274,720
Repayment of short-term loan (214,882) (1,450,627) (2,032,295) (286,243)
Purchase of noncontrolling interests (104,696) (56,741) (7,027) (990)
Capital injections from noncontrolling interests 2,187 — — —
Proceeds from exercise of employees’ share options 3 4 2 *
Proceeds from issuance of ordinary shares net of issuance costs 19,288,423 — 2,689,380 378,792
Repurchase of shares — (347,581) — —
Proceeds from/(Repurchase of) issuance of convertible senior notes, net of issuance costs of US$23,402, nil and
nil, respectively 10,085,520 (4,201,506) (7,675,227) (1,081,033)
Net cash provided by/(used in) financing activities 30,389,152 (4,354,919 ) (5,074,685 ) (714,754 )
Effect of exchange rate changes on cash and cash equivalents and restricted cash held in foreign currencies (319,034 ) 321,350 100,349 14,134
Net increase/(decrease) in cash and cash equivalents and restricted cash 2,844,999 2,664,279 (2,945,566) (414,874)
Cash and cash equivalents and restricted cash at beginning of the year 4,678,109 7,523,108 10,187,387 1,434,863
Cash and cash equivalents and restricted cash at end of the year 7,523,108 10,187,387 7,241,821 1,019,989
Including:
```

F-10

### F-11 (Continued)

```
Table of Contents
BILIBILI INC.
CONSOLIDATED STATEMENTS OF CASH FLOWS (Continued)
(All amounts in thousands)
Cash and cash equivalents at end of the year 7,523,108 10,172,584 7,191,821 1,012,947
Restricted cash at end of the year — 14,803 50,000 7,042
Supplemental disclosures of cash flows information:
Cash paid for income taxes, net of tax refund 73,717 80,591 103,391 14,562
Cash paid for interest expense 116,226 200,172 167,291 23,562
Supplemental schedule of non-cash investing and financing activities:
Property and equipment purchases financed by accounts payable 183,203 46,066 65,377 9,208
Acquisitions and investments financed by/(reduction of) payables 731,503 202,018 (80,000) (11,268)
Intangible assets purchases financed by payables 830,596 824,929 783,828 110,400
Issuance of ordinary shares in the business combination, purchase of noncontrolling interests and
investment addition 1,207,980 — — —
Issuance of ordinary shares in connection with debt conversion 449,914 96 — —
```

* Less than 1.
The accompanying notes are an integral part of these consolidated financial statements.

F-11

## Notes to the Consolidated Financial Statements

### 1. Operations

Bilibili Inc. (the “Company” or “Bilibili”) is an iconic brand and a leading video community for young generations in China. Incorporated as a limited liability company in the Cayman Islands in December 2013, the Company, through its consolidated subsidiaries, variable interest entities (“VIEs”) and subsidiaries of the VIEs (collectively referred to as the “Group”), is primarily engaged in the operation of providing online entertainment services to users in the People’s Republic of China (the “PRC” or “China”).

In April 2018, the Company completed its IPO on the NASDAQ Global Select Market. In March 2021, the Company successfully listed its Class Z ordinary shares on the main board of the Hong Kong Stock Exchange. The Company issued a total 28,750,000 Class Z ordinary shares in the global offering, including the fully exercised over-allotment option of 3,750,000 Class Z ordinary shares. Net proceeds from the global offering, including the over-allotment option, after deducting underwriting fees and other offering expenses, were approximately HKD22.9 billion (RMB19.3 billion).

On October 3, 2022 (the “Primary Conversion Effective Date”), the Company’s voluntary conversion of its secondary listing status to primary listing on the main board of the Hong Kong Stock Exchange became effective. The Company became a dual-primary listed company on the main board of Hong Kong Stock Exchange in Hong Kong and the NASDAQ Global Select Market in the United States.

In January 2023, the Company completed the offering of 15,344,000 ADSs at US$26.65 per ADS. The amount of net proceeds from such offering (after deducting all applicable costs and expenses including but not limited to selling commission) is approximately US$396.9 million (RMB2,689.4 million). Shortly thereafter, the Company completed repurchased of an aggregate principal amount of US$384.8 million (RMB2.6 billion) of its outstanding 0.5% convertible senior notes due December 2026 with an aggregate purchase price of US$331.2 million (RMB2.2 billion), which was funded by the net proceeds from the ADS Offering.

As of December 31, 2023, the Company’s major subsidiaries, VIEs and subsidiaries of the VIEs are as follows:

```
Percentage of
Direct or Indirect
Place and Year of Economic
Major Subsidiaries Incorporation Ownership Principal Activities
Bilibili HK Limited Hong Kong, 2014 100 Investment holding
Hode HK Limited Hong Kong, 2014 100 Investment holding
Chaodian HK Limited Hong Kong, 2019 100 Investment holding
Bilibili Co., Ltd. Japan, 2014 100 Business development
Hode Shanghai Limited (“Hode Shanghai”) PRC, 2014 100 Technology development1
Shanghai Bilibili Technology Co., Ltd. PRC, 2016 100 Technology development1
Chaodian (Shanghai) Technology Co., Ltd. PRC, 2019 100 E-commerce and advertising1
```

F-12

### 1. Operations (Continued)

```
Percentage of
Direct or
Indirect
Place and Year of Economic
Major VIEs and VIEs’ subsidiaries Incorporation Interest Principal Activities
Shanghai Hode Information Technology Co., Ltd. (“Hode Information
Technology”) PRC, 2013 100* Mobile game operation2
Shanghai Kuanyu Digital Technology Co., Ltd. (“Shanghai Kuanyu”) PRC, 2014 100* Video distribution and game distribution2
Sharejoy Network Technology Co., Ltd. (“Sharejoy Network”) PRC, 2014 100* Game distribution2
Shanghai Hehehe Culture Communication Co., Ltd. (“Shanghai Hehehe”) PRC, 2014 100* Comics distribution2
Shanghai Anime Tamashi Cultural Media Co., Ltd. (“Shanghai Anime
Tamashi”) PRC, 2015 100* E-commerce platform2
```

* Hode Shanghai is the primary beneficiary of the major VIEs and VIEs’ subsidiaries.
1 These companies were established in the PRC in the form of wholly foreign-owned enterprises.
2 These companies were established in the PRC in the form of investment solely by legal corporations or controlled by natural person(s).

F-13

### 1. Operations (Continued)

#### Contractual agreements with major VIEs

In order to comply with the PRC laws and regulations which prohibit or restrict foreign control of companies involved in provision of internet content services, the Group operates its restricted businesses in the PRC through the VIEs, whose equity interests are held by certain founders of the Group. The Company obtained control over these VIEs by entering into a series of contractual arrangements with the legal shareholders who are also referred to as nominee shareholders. These nominee shareholders are the legal owners of the VIEs. However, the rights of those nominee shareholders have been transferred to the Company through the contractual arrangements.

The contractual arrangements that are used to control the VIEs include powers of attorney, exclusive technology consulting and services agreements or exclusive business cooperation agreements, equity pledge agreements and exclusive option agreements. Management concluded that the Company, through the contractual arrangements, has the power to direct the activities that most significantly impact the VIEs’ economic performance, bears the risks of and enjoys the rewards normally associated with ownership of the VIEs, and therefore the Company is the ultimate primary beneficiary of these VIEs. As such, the Company consolidates the financial statements of these VIEs. Consequently, the financial results of the VIEs were included in the Group’s consolidated financial statements in accordance with the presentation as stated in Note 2(a).

The following is a summary of the contractual agreements entered into by and among the Company’s relevant subsidiaries, the VIEs, and respective nominee shareholders of the VIEs.

Exclusive Technology Consulting and Services Agreements. Under the exclusive technology consulting and services agreements between the Company’s relevant subsidiaries and the VIEs, the Company’s relevant subsidiaries have the exclusive right to provide the VIEs consulting and services related to, among other things, research and development, system operation, advertising, internal training and technical support. The Company’s relevant subsidiaries have the exclusive ownership of intellectual property rights created as a result of the performance of these agreements. These VIEs shall pay the Company’s relevant subsidiaries an annual service fee, which are subject to the adjustment by the Company’s relevant subsidiaries at its sole discretion. These agreements will remain effective for a 10 year’s term and then be automatically renewed, unless the Company’s relevant subsidiaries give the VIEs a termination notice 90 days before the term ends. On December 23, 2020, the above agreements were replaced by the exclusive business cooperation agreements, which contain terms substantially similar to the exclusive business cooperation agreements described above, the exclusive business cooperation agreements have an infinite period commencing from December 23, 2020, unless the Company’s relevant subsidiaries give the VIEs a termination notice 30 days before the term ends.

F-14



```
count = 0
for i in texts:
    count+= i.__len__()
```

```
count #21w字
```

```
21759
```

```
tables[:200]
```



```
[[[[&#x27;Shareholders’ equity&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;]],
  [[&#x27;Class Y Ordinary Shares (US$0.0001 par value; 100,000,000 shares authorized, 83,715,114\nshares issued and outstanding as of December 31, 2022; US$0.0001 par value; 100,000,000\nshares authorized, 83,715,114 shares issued and outstanding as of December 31, 2023)&#x27;,
    &#x27;&#x27;,
    &#x27;52&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;52&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;7&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Additional paid-in capital&#x27;,
    &#x27;&#x27;,
    &#x27;36,623,161&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;40,445,175&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;5,696,584&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Accumulated other comprehensive income&#x27;,
    &#x27;&#x27;,
    &#x27;58,110&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;212,477&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;29,927&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Total Bilibili Inc.’s shareholders’ equity&#x27;,
    &#x27;&#x27;,
    &#x27;15,237,828&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;14,391,900&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;2,027,057&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Total shareholders’ equity&#x27;,
    &#x27;&#x27;,
    &#x27;15,239,587&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;14,404,267&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;2,028,799&#x27;,
    &#x27;&#x27;]]],
 [[[&#x27;Net revenues&#x27;,
    &#x27;&#x27;,
    &#x27;19,383,684&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;21,899,167&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;22,527,987&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;3,173,001&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Gross profit&#x27;,
    &#x27;&#x27;,
    &#x27;4,043,147&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;3,849,295&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;5,441,865&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;766,471&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Sales and marketing expenses&#x27;,
    &#x27;&#x27;,
    &#x27;(5,794,853&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(4,920,745&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(3,916,150&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(551,578&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Research and development expenses&#x27;,
    &#x27;&#x27;,
    &#x27;(2,839,862&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(4,765,360&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(4,467,470&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(629,230&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Loss from operations&#x27;,
    &#x27;&#x27;,
    &#x27;(6,429,074&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(8,357,944&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(5,064,187&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(713,275&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Investment loss, net (including impairments)&#x27;,
    &#x27;&#x27;,
    &#x27;(194,183&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(532,485&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(435,644&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(61,359&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Interest expense&#x27;,
    &#x27;&#x27;,
    &#x27;(155,467&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(250,923&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(164,927&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(23,229&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Debt extinguishment gain&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;1,318,594&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;292,213&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;41,157&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Total other (expense)/income, net&#x27;,
    &#x27;&#x27;,
    &#x27;(284,376&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;954,436&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;331,179&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;46,646&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Income tax&#x27;,
    &#x27;&#x27;,
    &#x27;(95,289&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(104,145&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(78,705&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(11,085&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Net loss/(profit) attributable to noncontrolling interests&#x27;,
    &#x27;&#x27;,
    &#x27;19,511&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;10,640&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(10,608&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(1,494&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Net loss&#x27;,
    &#x27;&#x27;,
    &#x27;(6,808,739&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(7,507,653&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(4,811,713&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(677,714&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Foreign currency translation adjustments&#x27;,
    &#x27;&#x27;,
    &#x27;(420,991&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;337,972&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;154,367&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;21,742&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Total comprehensive loss&#x27;,
    &#x27;&#x27;,
    &#x27;(7,229,730&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(7,169,681&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(4,657,346&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(655,972&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Comprehensive loss attributable to the Bilibili Inc.’s shareholders&#x27;,
    &#x27;&#x27;,
    &#x27;(7,210,219&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(7,159,041&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(4,667,954&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(657,466&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Net loss per share, diluted&#x27;,
    &#x27;&#x27;,
    &#x27;(17.87&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(18.99&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(11.67&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(1.64&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Net loss per ADS, diluted&#x27;,
    &#x27;&#x27;,
    &#x27;(17.87&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(18.99&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(11.67&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(1.64&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Weighted average number of ordinary shares, diluted&#x27;,
    &#x27;&#x27;,
    &#x27;379,898,121&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;394,863,584&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;413,210,271&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;413,210,271&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Weighted average number of ADS, diluted&#x27;,
    &#x27;&#x27;,
    &#x27;379,898,121&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;394,863,584&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;413,210,271&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;413,210,271&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Cost of revenues&#x27;,
    &#x27;&#x27;,
    &#x27;76,232&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;69,096&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;63,724&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;8,975&#x27;,
    &#x27;&#x27;]],
  [[&#x27;General and administrative expenses&#x27;,
    &#x27;&#x27;,
    &#x27;553,526&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;554,976&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;596,950&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;84,079&#x27;,
    &#x27;&#x27;]]],
 [[[&#x27;Balance at December 31, 2020&#x27;,
    &#x27;&#x27;,
    &#x27;83,715,114&#x27;,
    &#x27;&#x27;,
    &#x27;52&#x27;,
    &#x27;&#x27;,
    &#x27;268,204,838&#x27;,
    &#x27;&#x27;,
    &#x27;172&#x27;,
    &#x27;&#x27;,
    &#x27;14,616,302&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;17,884&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;141,129&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(7,175,339&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;182,004&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;7,782,204&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Share-based compensation&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;999,817&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;999,817&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Share issuance upon secondary\npublic offering (“HK IPO”), net\nof issuance costs of\nHKD337,143&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;28,750,000&#x27;,
    &#x27;&#x27;,
    &#x27;18&#x27;,
    &#x27;&#x27;,
    &#x27;19,266,792&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;19,266,810&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Issuance of Class Z ordinary\nshares related to long-term\ninvestments&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;1,045,700&#x27;,
    &#x27;&#x27;,
    &#x27;1&#x27;,
    &#x27;&#x27;,
    &#x27;(1&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Capital injection in subsidiaries by\nnoncontrolling interests&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;2,187&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;2,187&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Purchase of noncontrolling\ninterests&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;715,271&#x27;,
    &#x27;&#x27;,
    &#x27;1&#x27;,
    &#x27;&#x27;,
    &#x27;(35,604&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(137,532&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(173,135&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Balance at December 31, 2021&#x27;,
    &#x27;&#x27;,
    &#x27;83,715,114&#x27;,
    &#x27;&#x27;,
    &#x27;52&#x27;,
    &#x27;&#x27;,
    &#x27;306,889,473&#x27;,
    &#x27;&#x27;,
    &#x27;199&#x27;,
    &#x27;&#x27;,
    &#x27;35,929,961&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;24,621&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(279,862&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(13,971,304&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;12,399&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;21,716,066&#x27;,
    &#x27;&#x27;]]],
 [[[&#x27;Balance at December 31, 2021&#x27;,
    &#x27;&#x27;,
    &#x27;83,715,114&#x27;,
    &#x27;&#x27;,
    &#x27;52&#x27;,
    &#x27;&#x27;,
    &#x27;306,889,473&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;199&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;35,929,961&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;24,621&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(279,862&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(13,971,304&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;12,399&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;21,716,066&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Share-based compensation&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;1,040,683&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;1,040,683&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Purchase of noncontrolling\ninterests&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;45,000&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;*&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;*&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Repurchase of shares&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;(2,640,832&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(2&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(347,579&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(347,581&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Foreign currency translation\nadjustments&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;337,972&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;337,972&#x27;,
    &#x27;&#x27;]]],
 [[[&#x27;Balance at December 31, 2022&#x27;,
    &#x27;&#x27;,
    &#x27;83,715,114&#x27;,
    &#x27;&#x27;,
    &#x27;52&#x27;,
    &#x27;&#x27;,
    &#x27;308,223,639&#x27;,
    &#x27;&#x27;,
    &#x27;201&#x27;,
    &#x27;&#x27;,
    &#x27;36,623,161&#x27;,
    &#x27;&#x27;,
    &#x27;36,173&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;58,110&#x27;,
    &#x27;&#x27;,
    &#x27;(21,479,869&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;1,759&#x27;,
    &#x27;&#x27;,
    &#x27;15,239,587&#x27;,
    &#x27;&#x27;],
   [&#x27;Net (loss)/income&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;(4,822,321&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;10,608&#x27;,
    &#x27;&#x27;,
    &#x27;(4,811,713&#x27;,
    &#x27;)&#x27;],
   [&#x27;Share-based compensation&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;1,132,644&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;1,132,644&#x27;,
    &#x27;&#x27;],
   [&#x27;Issuance of Class Z ordinary shares\nupon new ADS offering (“ADS\noffering”)&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;15,344,000&#x27;,
    &#x27;&#x27;,
    &#x27;10&#x27;,
    &#x27;&#x27;,
    &#x27;2,689,370&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;2,689,380&#x27;,
    &#x27;&#x27;],
   [&#x27;Share issuance from exercise of share\noptions&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;2,210,741&#x27;,
    &#x27;&#x27;,
    &#x27;2&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;2&#x27;,
    &#x27;&#x27;],
   [&#x27;Share issuance from vest of restricted\nshare units&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;22,500&#x27;,
    &#x27;&#x27;,
    &#x27;*&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;*&#x27;,
    &#x27;&#x27;],
   [&#x27;Appropriation to statutory reserves&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;8,576&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;(8,576&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;],
   [&#x27;Foreign currency translation\nadjustments&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;154,367&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;154,367&#x27;,
    &#x27;&#x27;],
   [&#x27;Balance at December 31, 2023&#x27;,
    &#x27;&#x27;,
    &#x27;83,715,114&#x27;,
    &#x27;&#x27;,
    &#x27;52&#x27;,
    &#x27;&#x27;,
    &#x27;325,800,880&#x27;,
    &#x27;&#x27;,
    &#x27;213&#x27;,
    &#x27;&#x27;,
    &#x27;40,445,175&#x27;,
    &#x27;&#x27;,
    &#x27;44,749&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;212,477&#x27;,
    &#x27;&#x27;,
    &#x27;(26,310,766&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;12,367&#x27;,
    &#x27;&#x27;,
    &#x27;14,404,267&#x27;,
    &#x27;&#x27;]]],
 [[[&#x27;Cash flows from operating activities:&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Adjustments to reconcile net loss to net cash (used in)/provided by operating activities:&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Amortization of intangible assets&#x27;,
    &#x27;&#x27;,
    &#x27;1,903,226&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;2,581,325&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;2,003,199&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;282,145&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Amortization of debt issuance costs&#x27;,
    &#x27;&#x27;,
    &#x27;25,234&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;48,167&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;23,299&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;3,282&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Allowance for/(reversal of) expected credit losses&#x27;,
    &#x27;&#x27;,
    &#x27;189,165&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;130,549&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(16,000&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(2,254&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Deferred income taxes&#x27;,
    &#x27;&#x27;,
    &#x27;(21,492&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(36,495&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(25,376&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(3,574&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Unrealized fair value changes of investments&#x27;,
    &#x27;&#x27;,
    &#x27;200,274&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;251,383&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;66,462&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;9,361&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Termination expenses of certain game projects&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;525,762&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;354,811&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;49,974&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Loss from equity method investments&#x27;,
    &#x27;&#x27;,
    &#x27;37,179&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;211,620&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;112,142&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;15,795&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Impairments of long-term investments&#x27;,
    &#x27;&#x27;,
    &#x27;91,493&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;465,645&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;278,891&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;39,281&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Changes in operating assets and liabilities:&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Amount due from related parties&#x27;,
    &#x27;&#x27;,
    &#x27;8,792&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(74,117&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;75,273&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;10,602&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Other long-term assets&#x27;,
    &#x27;&#x27;,
    &#x27;(138,396&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(444,326&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;185,783&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;26,167&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Salary and welfare payable&#x27;,
    &#x27;&#x27;,
    &#x27;254,213&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;396,113&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(182,171&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(25,658&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Deferred revenue&#x27;,
    &#x27;&#x27;,
    &#x27;494,551&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;173,935&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;133,902&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;18,859&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Amount due to related parties&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(21,210&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(12,007&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(1,691&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Net cash (used in)/provided by operating activities&#x27;,
    &#x27;&#x27;,
    &#x27;(2,647,008&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(3,911,370&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;266,622&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;37,553&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Purchase of property and equipment&#x27;,
    &#x27;&#x27;,
    &#x27;(965,410&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(760,427&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(181,897&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(25,620&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Purchase of short-term investments&#x27;,
    &#x27;&#x27;,
    &#x27;(71,748,847&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(70,578,711&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(13,547,650&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(1,908,147&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Cash consideration paid for purchase of subsidiaries, net of cash acquired&#x27;,
    &#x27;&#x27;,
    &#x27;(521,984&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(1,179,764&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(70,000&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(9,859&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Repayment of loans from investees&#x27;,
    &#x27;&#x27;,
    &#x27;539,225&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;596,766&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;684,834&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;96,457&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Placements of time deposits&#x27;,
    &#x27;&#x27;,
    &#x27;(10,697,444&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(10,245,026&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(9,961,925&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(1,403,108&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Impact to cash resulting from deconsolidation of subsidiaries&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(125&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Cash flows from financing activities:&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Repayment of short-term loan&#x27;,
    &#x27;&#x27;,
    &#x27;(214,882&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(1,450,627&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(2,032,295&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(286,243&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Capital injections from noncontrolling interests&#x27;,
    &#x27;&#x27;,
    &#x27;2,187&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Proceeds from issuance of ordinary shares net of issuance costs&#x27;,
    &#x27;&#x27;,
    &#x27;19,288,423&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;2,689,380&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;378,792&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Proceeds from/(Repurchase of) issuance of convertible senior notes, net of issuance costs of US$23,402, nil and\nnil, respectively&#x27;,
    &#x27;&#x27;,
    &#x27;10,085,520&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;(4,201,506&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(7,675,227&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;(1,081,033&#x27;,
    &#x27;)&#x27;]],
  [[&#x27;Effect of exchange rate changes on cash and cash equivalents and restricted cash held in foreign currencies&#x27;,
    &#x27;&#x27;,
    &#x27;(319,034&#x27;,
    &#x27;)&#x27;,
    &#x27;&#x27;,
    &#x27;321,350&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;100,349&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;14,134&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Cash and cash equivalents and restricted cash at beginning of the year&#x27;,
    &#x27;&#x27;,
    &#x27;4,678,109&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;7,523,108&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;10,187,387&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;1,434,863&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Including:&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;, &#x27;&#x27;]]],
 [[[&#x27;Cash and cash equivalents at end of the year&#x27;,
    &#x27;&#x27;,
    &#x27;7,523,108&#x27;,
    &#x27;&#x27;,
    &#x27;10,172,584&#x27;,
    &#x27;&#x27;,
    &#x27;7,191,821&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;1,012,947&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Supplemental disclosures of cash flows information:&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Cash paid for interest expense&#x27;,
    &#x27;&#x27;,
    &#x27;116,226&#x27;,
    &#x27;&#x27;,
    &#x27;200,172&#x27;,
    &#x27;&#x27;,
    &#x27;167,291&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;23,562&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Property and equipment purchases financed by accounts payable&#x27;,
    &#x27;&#x27;,
    &#x27;183,203&#x27;,
    &#x27;&#x27;,
    &#x27;46,066&#x27;,
    &#x27;&#x27;,
    &#x27;65,377&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;9,208&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Intangible assets purchases financed by payables&#x27;,
    &#x27;&#x27;,
    &#x27;830,596&#x27;,
    &#x27;&#x27;,
    &#x27;824,929&#x27;,
    &#x27;&#x27;,
    &#x27;783,828&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;110,400&#x27;,
    &#x27;&#x27;]],
  [[&#x27;Issuance of ordinary shares in connection with debt conversion&#x27;,
    &#x27;&#x27;,
    &#x27;449,914&#x27;,
    &#x27;&#x27;,
    &#x27;96&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;,
    &#x27;&#x27;,
    &#x27;—&#x27;,
    &#x27;&#x27;]]],
 [[[&#x27;Bilibili HK Limited&#x27;,
    &#x27;&#x27;,
    &#x27;Hong Kong, 2014&#x27;,
    &#x27;&#x27;,
    &#x27;100&#x27;,
    &#x27;&#x27;,
    &#x27;Investment holding&#x27;]],
  [[&#x27;Chaodian HK Limited&#x27;,
    &#x27;&#x27;,
    &#x27;Hong Kong, 2019&#x27;,
    &#x27;&#x27;,
    &#x27;100&#x27;,
    &#x27;&#x27;,
    &#x27;Investment holding&#x27;]],
  [[&#x27;Hode Shanghai Limited (“Hode Shanghai”)&#x27;,
    &#x27;&#x27;,
    &#x27;PRC, 2014&#x27;,
    &#x27;&#x27;,
    &#x27;100&#x27;,
    &#x27;&#x27;,
    &#x27;Technology development1&#x27;]],
  [[&#x27;Chaodian (Shanghai) Technology Co., Ltd.&#x27;,
    &#x27;&#x27;,
    &#x27;PRC, 2019&#x27;,
    &#x27;&#x27;,
    &#x27;100&#x27;,
    &#x27;&#x27;,
    &#x27;E-commerce and advertising1&#x27;]]],
 [[[&#x27;Shanghai Hode Information Technology Co., Ltd. (“Hode Information\nTechnology”)&#x27;,
    &#x27;&#x27;,
    &#x27;PRC, 2013&#x27;,
    &#x27;&#x27;,
    &#x27;100*&#x27;,
    &#x27;&#x27;,
    &#x27;Mobile game operation2&#x27;]],
  [[&#x27;Sharejoy Network Technology Co., Ltd. (“Sharejoy Network”)&#x27;,
    &#x27;&#x27;,
    &#x27;PRC, 2014&#x27;,
    &#x27;&#x27;,
    &#x27;100*&#x27;,
    &#x27;&#x27;,
    &#x27;Game distribution2&#x27;]],
  [[&#x27;Shanghai Anime Tamashi Cultural Media Co., Ltd. (“Shanghai Anime\nTamashi”)&#x27;,
    &#x27;&#x27;,
    &#x27;PRC, 2015&#x27;,
    &#x27;&#x27;,
    &#x27;100*&#x27;,
    &#x27;&#x27;,
    &#x27;E-commerce platform2&#x27;]]],
 [[[None,
    &#x27;In order to comply with the PRC laws and regulations which prohibit or restrict foreign control of companies involved in provision of internet&#x27;],
   [&#x27;content services, the Group operates its restricted businesses in the PRC through the VIEs, whose equity interests are held by certain founders of the&#x27;,
    None],
   [&#x27;Group. The Company obtained control over these VIEs by entering into a series of contractual arrangements with the legal shareholders who are also&#x27;,
    None],
   [&#x27;referred to as nominee shareholders. These nominee shareholders are the legal owners of the VIEs. However, the rights of those nominee shareholders&#x27;,
    None],
   [&#x27;have been transferred to the Company through the contractual arrangements.&#x27;,
    None]]]]
```



# PDF文档处理与多模态信息提取

## 1. 文本与表格转换

```
#转换成字符串
texts = repr(texts)
tables = repr(tables)
```

## 2. 提取PDF中的图像

```
import fitz  # PyMuPDF
import os

def extract_images_from_pdf(pdf_path, start_page, end_page, output_dir):
    # 确认输出路径是否存在，如果不存在则创建
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)

    pdf_document = fitz.open(pdf_path)
    images = []

    for page_num in range(start_page - 1, end_page):
        page = pdf_document.load_page(page_num)
        image_list = page.get_images(full=True)

        for image_index, img in enumerate(image_list):
            xref = img[0]
            base_image = pdf_document.extract_image(xref)
            image_bytes = base_image["image"]

            # 获取图片的元数据
            image_name = f"page{page_num + 1}_img{image_index + 1}"
            image_ext = base_image["ext"]
            images.append((image_name, image_ext, image_bytes))
    
    return images

# 页数范围
pdf_path = r"D:\pythonwork\LLM\Course\哔哩哔哩(B站)用户人群画像分析报告2023.pdf"
start_page = 1
end_page = 10
output_dir = r"D:\pythonwork\LLM\Course\output_images_folder"

# 提取图片
extracted_images = extract_images_from_pdf(pdf_path, start_page, end_page, output_dir)

# 保存图片到本地
for image_name, image_ext, image_bytes in extracted_images:
    image_path = os.path.join(output_dir, f"{image_name}.{image_ext}")
    with open(image_path, "wb") as image_file:
        image_file.write(image_bytes)
        print(f"Saved {image_path}")
```

```
Saved D:\pythonwork\LLM\Course\output_images_folder\page1_img1.jpeg
Saved D:\pythonwork\LLM\Course\output_images_folder\page2_img1.jpeg
Saved D:\pythonwork\LLM\Course\output_images_folder\page3_img1.jpeg
Saved D:\pythonwork\LLM\Course\output_images_folder\page4_img1.jpeg
Saved D:\pythonwork\LLM\Course\output_images_folder\page5_img1.jpeg
Saved D:\pythonwork\LLM\Course\output_images_folder\page6_img1.jpeg
Saved D:\pythonwork\LLM\Course\output_images_folder\page7_img1.jpeg
Saved D:\pythonwork\LLM\Course\output_images_folder\page8_img1.jpeg
Saved D:\pythonwork\LLM\Course\output_images_folder\page9_img1.jpeg
```

### 3.【选做】使用高精度OCR提取图像中的文本

```
"""
import pytesseract
from pdf2image import convert_from_path
from PIL import Image, ImageEnhance, ImageFilter

def preprocess_image(image):
    # 转换为灰度图像
    image = image.convert('L')
    # 增强对比度
    enhancer = ImageEnhance.Contrast(image)
    image = enhancer.enhance(2)
    # 应用滤镜
    image = image.filter(ImageFilter.MedianFilter())
    return image

def extract_text_from_image(image):
    preprocessed_image = preprocess_image(image)
    text = pytesseract.image_to_string(preprocessed_image)
    return text

# 指定需要处理的页数范围
start_page = 190
end_page = 200

# 加载指定页数的PDF页面
images = convert_from_path(pdf_path)
selected_images = images[start_page - 1:end_page]

# 提取文本
ocr_texts = [extract_text_from_image(image) for image in selected_images]

# 输出提取的文本
for page_num, text in enumerate(ocr_texts, start=start_page):
    print(f"Page {page_num}:\n{text}\n")
"""
```

### 4. 上传图像并生成URL集合

```
import oss2
import os

# 阿里云AccessKey ID和AccessKey Secret
access_key_id = 'your_access_key_id'
access_key_secret = 'your_access_key_secret'
bucket_name = 'your_bucket_name'
endpoint = 'your_endpoint'  # 例如: 'oss-cn-hangzhou.aliyuncs.com'

# 初始化Bucket
auth = oss2.Auth(access_key_id, access_key_secret)
bucket = oss2.Bucket(auth, endpoint, bucket_name)

def upload_image(file_path, object_name):
    """
    上传单个图像到OSS
    :param file_path: 本地文件路径
    :param object_name: 上传到OSS后的对象名
    :return: 上传的图像URL
    """
    bucket.put_object_from_file(object_name, file_path)
    url = f"https://{bucket_name}.{endpoint}/{object_name}"
    return url

def batch_upload_images(local_dir):
    """
    批量上传图像并返回URL列表
    :param local_dir: 本地目录路径
    :return: 上传的图像URL列表
    """
    image_urls = []
    for file_name in os.listdir(local_dir):
        if file_name.endswith(('.png', '.jpg', '.jpeg')):
            file_path = os.path.join(local_dir, file_name)
            object_name = f'images/{file_name}'  # 在OSS中的存储路径
            url = upload_image(file_path, object_name)
            image_urls.append(url)
    return image_urls

# 本地图像目录
local_directory = 'path/to/your/images'

# 批量上传图像并获取URL
uploaded_image_urls = batch_upload_images(local_directory)

# 打印所有上传的图像URL
for url in uploaded_image_urls:
    print(url)
```

```
image_urls = [
    "https://skojiangdoc.oss-cn-beijing.aliyuncs.com/LLM/Video/page1_img1.jpeg",
    "https://skojiangdoc.oss-cn-beijing.aliyuncs.com/LLM/Video/page2_img1.jpeg",
    "https://skojiangdoc.oss-cn-beijing.aliyuncs.com/LLM/Video/page3_img1.jpeg",
    "https://skojiangdoc.oss-cn-beijing.aliyuncs.com/LLM/Video/page4_img1.jpeg",
    "https://skojiangdoc.oss-cn-beijing.aliyuncs.com/LLM/Video/page5_img1.jpeg",
    "https://skojiangdoc.oss-cn-beijing.aliyuncs.com/LLM/Video/page6_img1.jpeg",
    "https://skojiangdoc.oss-cn-beijing.aliyuncs.com/LLM/Video/page7_img1.jpeg",
    "https://skojiangdoc.oss-cn-beijing.aliyuncs.com/LLM/Video/page8_img1.jpeg",
    "https://skojiangdoc.oss-cn-beijing.aliyuncs.com/LLM/Video/page9_img1.jpeg"
]
```

### 5. 将多模态信息传给GPT-4o

```
# 组合所有提取的内容
ocr_texts = ""
combined_texts = "\n".join(texts + tables + ocr_texts)
messages = [
    {
        "role": "user",
        "content": [
            {
                "type": "text",
                "text": "请用300字总结一下现在我给你的全部信息，包括图像"
            },
            {
                "type": "text",
                "text": combined_texts
            },
            *[{
                "type": "image_url",
                "image_url": {
                    "url": url
                }
            } for url in image_urls]
        ]
    }
]

response = client.chat.completions.create(
  model="gpt-4o",
  messages=messages,
  max_tokens=300,
)

# 打印响应
description = response.choices[0].message.content
print(description)
```

```
收到的信息包括表格、财务数据以及图表。这些数据和图表主要涵盖了B站2023年第三季度财报的内容，突出显示了关键指标，如日均活跃用户、总营收及毛利率等。2023年第三季度，B站日均活跃用户达1.03亿，较去年同比增长14%；月均活跃用户为3.41亿。总营收为58.1亿元人民币，毛利率上升至25.0%。其中，增值服务业务收入为26.0亿元，同比增长17%；广告业务收入为16.4亿元，同比增长21%；游戏业务收入为9.9亿元。调整后的净亏损同比大幅收窄51%，实现季度环比五连涨。UP主活跃同比增长21%，月均投稿量增加37%，并且“正式会员”保留率在第12个月达到约80%。此外，B站的广告收入高速增长，增收及减亏成效显著，形成了一个良好的财务表现。插图生动地展示了B站的财报数据及趋势，其中包括一些图形和图表来表示关键数据指标及其同比变化。总体而言，B站在第三季度取得了显著的用户增长及收入表现，如图所示，其各项关键指标均呈现上涨趋势。
```

```
#与gpt-4o进行对话

messages.append({"role":"assistant","content":response.choices[0].message.content})

prompt = "请帮我整理出过去2年中哩哔哩公司的发展情况"

messages.append({"role":"user","content":prompt})

response = client.chat.completions.create(
  model="gpt-4o",
  messages=messages,
  max_tokens=1000,
)

print(response.choices[0].message.content)
```



# 哔哩哔哩（B站）过去两年发展情况总结

过去两年中，哔哩哔哩公司（B站）的发展情况可从以下几个方面进行总结：

## 1. 用户增长

- **日均活跃用户（DAU）：**
  B站的日均活跃用户持续增长，从2022年Q1的约79.4百万增长至2023年Q3的1.03亿，增长率显著。

- **月均活跃用户（MAU）：**
  月均活跃用户数量也增加，2023年Q3达到了3.41亿。

- **用户活跃度：**
  用户日均使用时长创历史新高，达到100分钟。

## 2. 财务表现

- **总营收：**
  总收入逐渐增加，2023年Q3的营收为58.1亿元人民币，比去年同期增长。

- **毛利率：**
  毛利率连续五个季度环比上升，2023年Q3毛利率为25%，比2022年Q2的15%有所增长。

- **业务收入：**
  - **增值服务业务**：增加到26.0亿元，同比增长17%。
  - **广告业务**：收入提升至16.4亿元，同比增长21%。
  - **游戏业务**：收入达到9.9亿元。

- **调整后的净亏损：**
  2023年Q3调整后的净亏损同比减少51%。

## 3. 内容及平台生态

- **UP主（内容创作者）增长：**
  活跃UP主同比增长21%，投稿量增长37%。

- **内容多样性：**
  专业用户自制内容（PUGV）覆盖多个有趣且有用的领域，如职场知识、家居、养育、科技数码、汽车等。

## 4. 会员及互动

- **正式会员：**
  正式会员数量持续增加，正式会员第12个月的保留率约为80%，说明平台的用户粘性较高。

- **月均互动数：**
  2023年Q3的平均互动数达170亿，同比增长18%。

## 5. 重大事件

- **股票上市及募集资金：**
  2021年3月，B站在香港成功挂牌上市，发行了28,750,000股Class Z普通股份，募集到约19.3亿港元（约合22.9亿人民币）。2023年初，再次通过发行ADS募集资金，进一步支持公司的发展。

## 总结

哔哩哔哩在过去两年中，用户活跃度显著增加，财务表现持续改善，营收和毛利率逐步提升。同时，B站在内容丰富性和内容质量方面不断提升，吸引了大量内容创作者和用户。凭借稳健的用户增长策略和多样的内容生态，B站在互联网视频社区领域保持持续发展的势头。

### 代码示例：构建对话消息

```python
# messages.append({"role": "assistant", "content": response.choices[0].message.content})

prompt = "请问过去几年中，哔哩哔哩的利润状况如何？告诉我毛利率的具体数据即可"

messages.append({"role": "user", "content": prompt})

response = client.chat.completions.create(
  model="gpt-4o",
  messages=messages,
  max_tokens=1000,
)

description = response.choices[0].message.content

print(description)
```

## 毛利率数据

根据你提供的信息，以下是哔哩哔哩在过去几年的毛利率数据：

- **2022年Q2：** 15.0%
- **2022年Q3：** 18.2%
- **2022年Q4：** 20.3%
- **2023年Q1：** 21.8%
- **2023年Q2：** 23.1%

从这些数据可以看出，哔哩哔哩的毛利率在过去几年中呈现出逐步增长的趋势。

### 代码示例：组合提取内容

```python
# 组合所有提取的内容
ocr_texts = ""
combined_texts = "\n".join(texts + tables + ocr_texts)
messages = [
    {
        "role": "user",
        "content": [
            {"type": "text", "text": "请用300字总结一下现在我给你的全部信息，包括图像"},
            {"type": "text", "text": combined_texts},
            *[
                {"type": "image_url", "image_url": {"url": url}}
                for url in image_urls
            ],
        ],
    }
]

response = client.chat.completions.create(
  model="gpt-4o",
  messages=messages,
  max_tokens=300,
)

# 打印响应
description = response.choices[0].message.content
```

### 代码示例：图像生成

```python
response = client.images.generate(
  model="dall-e-3",
  prompt=description + "根据我所提供的数据，绘制横坐标为时间、纵坐标为百分比的折线图",
  size="1024x1024",
  quality="standard",
  n=1,
)

image_url = response.data[0].url

# 获取生成的图像URL
print(response.data[0].url)
```

## 生成图像资源

https://oaidalleapiprodscus.blob.core.windows.net/private/org-HSwQITpPfCnOXCUCglzxrecL/user-GMg4tqRRi7GglFyhJY97QbH7/img-Pu3VxHTw4Y4srobRnhs6NAvL.png?st=2024-05-14T11:23:22Z&se=2024-05-14T13:23:22Z&sp=r&sv=2021-08-06&sr=b&rscd=inline&rsct=image/png&skoid=6aaadede-4fb3-4698-a8f6-684d7786b067&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2024-05-13T15:49:02Z&ske=2024-05-14T15:49:02Z&sks=b&skv=2021-08-06&sig=5IuA2Oje0qRFN+4e/75b1v1K/uWZCM6NDoQEqG/3V0s%=

![](img/927a9b4a0cb14a9b03a7f8ae9b151bca.png)

## 大模型技术社群与资源

更多大模型技术内容学习+加入大模型技术社群

**扫码添加助理 英英**，回复“大模型”，了解更多大模型技术详情哦👇

![](img/b2d6aa4072b8a8701d883fc26ae5c708.png)

此外，**扫码回复“LLM”**，即可领取**公开课课件、代码、数据**等~
