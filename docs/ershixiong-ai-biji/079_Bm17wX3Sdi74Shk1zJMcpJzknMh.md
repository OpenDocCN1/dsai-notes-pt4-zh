# Aider + Deepseek 安装配置实践

> 来源：[https://qianniucity.feishu.cn/docx/ERc5dynpbodoluxcCdwcfTrUnCd](https://qianniucity.feishu.cn/docx/ERc5dynpbodoluxcCdwcfTrUnCd)

### 安装 Aider

#### Windows

```
powershell -ExecutionPolicy ByPass -c &quot;irm https://aider.chat/install.ps1 | iex&quot;
```

#### Mac & Linux

使用curl下载脚本并使用sh执行：

```
curl -LsSf https://aider.chat/install.sh | sh
```

如果你的系统没有curl，你可以使用wget：

```
wget -qO- https://aider.chat/install.sh | sh
```

### 配置 Deepseek

```
python -m pip install -U aider-chat

export DEEPSEEK_API_KEY=&lt;key&gt; # Mac/Linux
setx   DEEPSEEK_API_KEY &lt;key&gt; # Windows, restart shell after setx

# Use DeepSeek Coder V3
aider --deepseek
```

![](img/a185cf95c0ccfcbe994a8e6fba60ec4a.png)

### 编程实践：

#### 构建贪吃蛇游戏

![](img/c63ee88a932ac27665f74bd99ac4734b.png)

#### 优化游戏体验

![](img/8ee7bf2d5e94f9a0a90a95e7f35646cf.png)

#### 修复 bug

![](img/0f012bdb1c55ef805a93e9c7335b8893.png)

#### 最终效果

![](img/fa90419c056d3465cf53970e1212e7f8.png)

#### 优势

+   支持中文prompt

+   支持配置各种第三方 API

+   习惯用 vim 的大牛，不二的选择

### 相关链接

+   Aider 官方文档： https://aider.chat/

+   Deepseek 官网： https://www.deepseek.com/