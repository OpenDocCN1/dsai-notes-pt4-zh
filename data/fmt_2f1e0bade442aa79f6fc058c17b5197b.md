
# gemini2.5 compute use安装

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/VbMWdHutRorkb7xWPUDcxnv9nRc](https://xatc0v8uz5m.feishu.cn/docx/VbMWdHutRorkb7xWPUDcxnv9nRc)

![](img/1f1dfb66bc8bbb7cfbfd808f236a49d6.png)

## 通过Google AI Studio

### 安装依赖

获取API密钥：

访问 Google AI Studio控制台

登录谷歌账号

创建新项目或选择现有项目

生成API密钥（需要与计费账户关联）

安装必要依赖：

```
pip install google-generativeai
pip install playwright  # 用于浏览器自动化
```

### 基本使用示例

```
import os
import google.generativeai as genai

# 配置API密钥
api_key = "YOUR_API_KEY"
genai.configure(api_key=api_key)

# 初始化模型
model = genai.GenerativeModel('gemini-2.5-pro-computer-use')

# 发送请求
response = model.generate_content("请打开https://example.com并查找最新新闻")
print(response.text)
```

## AI Studio API 获取

`https://aistudio.google.com/api-keys`

![](img/c763e94af0b5448fe63c600d29d737be.png)

![](img/14c512781b53b0a0feb5f7c27280a0a1.png)

获取到API之后替换，

给到Trae帮我们安装环境

再把Gemini compute use配置好
