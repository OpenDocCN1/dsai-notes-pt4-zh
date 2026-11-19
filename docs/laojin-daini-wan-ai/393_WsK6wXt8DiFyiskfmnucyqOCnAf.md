
# Nicesuno - 调用Suno创建音乐

> 来源：[https://my.feishu.cn/docx/UciQdl0vZos7mxx8jm2cCTaXnkb](https://my.feishu.cn/docx/UciQdl0vZos7mxx8jm2cCTaXnkb)

## SUNO API 已失效

转载自[Github官网](https://github.com/wangxyd/nicesuno)

一款基于[`Suno`](https://suno.com/)和[`Suno-API`](https://github.com/SunoAI-API/Suno-API)创作音乐的`chatgpt-on-wechat`插件。

### 使用方法

```
1.创作声乐
用法：唱/演唱<提示词>
示例：唱明天会更好。
2.创作器乐
用法：演奏<提示词>
示例：演奏明天会更好。
3.创作歌词
用法：写歌/作词<提示词>
示例：写歌明天会更好。
4.自定义模式
用法：
唱/演唱/演奏
标题: <标题>
风格: <风格1> <风格2> ...
<歌词>
备注：前三行必须为创作前缀、标题、风格，<标题><风格><歌词>三个值可以为空，但<风格><歌词>不可同时为空！
```

## 插件效果

### 创作音乐

![](img/7786d48cd179e31fd0074fa0d3d81e6c.png)

### Suno超过限额之后，仅创作歌词

![](img/23f0ec0850b1bcd246744f0cbdf8966f.png)

## 安装方法

### 浏览器访问 `Suno` 获取 `session_id` 和 `Cookie`

浏览器访问并登录[`Suno`](https://suno.com/)。

按`F12`键打开开发者工具，选择“网络”标签；

稍等一分钟就会出现类似`tokens?_clerk_js_version=4.72.0-snapshot.vc141245`的请求，获取该`Request URL`中的`Session_id`以及`Cookie`；

`Session_id`获取示例：比如这里的`Request URL`为`https://clerk.suno.com/v1/client/sessions/sess_xeNbYcD4zOK89Vzwipl30x5gWq3/tokens?_clerk_js_version=4.72.0-snapshot.vc141245`，则`Session_id`是`sess_xeNbYcD4zOK89Vzwipl30x5gWq3`。

### 部署 `Suno-API`

详细的安装和配置步骤参考[`Suno-API`](https://github.com/SunoAI-API/Suno-API)，这里只给出大致步骤：

#### 克隆代码

`git clone https://github.com/SunoAI-API/Suno-API.git`

配置`Suno-API`，首先拷贝模板文件`.env.example`到`.env`，然后`cd Suno-API`：

`cp .env.example .env`

然后编辑`.env`文件，将其中的`SESSION_ID`和`COOKIE`两个环境变量的值，分别替换为步骤1中获取的`Session_id`和`Cookie`

```
BASE_URL=https://studio-api.suno.ai
SESSION_ID=将我替换为步骤1中获取的Session_id
COOKIE=将我替换为步骤1中获取的Cookie
```

#### 安装依赖

`pip3 install -r requirements.txt`

#### 运行程序

```
nohup uvicorn main:app &>> Suno-API.log &
tail -f Suno-API.log
```

### 安装 `Nicesuno` 插件

`#installp https://github.com/wangxyd/nicesuno.git#scanp`

`Nicesuno`的默认配置无需修改，即可使用`Suno`创作音乐。

### `Nicesuno` 自定义配置

如果需要自定义配置，可以按照如下方法修改：

拷贝模板文件`config.json.template`到`config.json`

`cp config.json.template config.json`

#### 编辑 `config.json`

```json
{
  "suno_api_bases": ["http://127.0.0.1:8000"],
  "music_create_prefixes": ["唱", "演唱"],
  "instrumental_create_prefixes": ["演奏"],
  "lyrics_create_prefixes": ["写歌", "作词"],
  "music_output_dir": "/tmp/nicesuno",
  "is_send_lyrics": true,
  "is_send_covers": true
}
```

以上配置项中：

+   `suno_api_bases`: `Suno-API`的监听地址和端口，注意该参数的值为一个字符串数组，后续用于实现自动切换`Suno`账号；
+   `music_create_prefixes`: 创作声乐的消息前缀，注意该参数的值为一个字符串数组；
+   `instrumental_create_prefixes`: 创作器乐的消息前缀，注意该参数的值为一个字符串数组；
+   `lyrics_create_prefixes`: 创作歌词的消息前缀，注意该参数的值为一个字符串数组；
+   `music_output_dir`: 创作的音乐的存储目录，默认为`/tmp/nicesuno`；
+   `is_send_lyrics`: 是否获取并发送歌词，默认为`true`；
+   `is_send_covers`: 是否下载并发送封面，默认为`true`。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
