# 用：本地部署一个长记忆应用

> 来源：[https://gotoaiworld.feishu.cn/docx/Gqetd6JRDogSkoxGtqjcdUDJn0e](https://gotoaiworld.feishu.cn/docx/Gqetd6JRDogSkoxGtqjcdUDJn0e)

> [【对客】Viking 记忆库 OnePage](https://bytedance.larkoffice.com/docx/Fz7adZ3hPowtuQxPRR6c00Kinid)应用使用了记忆库，点击文档快速了解 👉

### 查看长记忆方案代码

+   点击链接进入：[应用广场](https://console.volcengine.com/ark/region:ark+cn-beijing/application?utm_campaign=2025H2&utm_content=gongjuxiang&utm_medium=xianshang&utm_source=LBM-zhyx&utm_term=dyjx_wendang)，下滑选择“DeepSeek+Doubao长记忆方案”

![](img/1716730c0d2e1b1d97dbd9982f2ce234.png)

+   进入应用详情页，可以免费在线体验应用，点击右上角「Github代码查看」，可获取开源代码进行本地部署

PS. 本地部署可以修改虚拟角色prompt、开场白生成prompt等，打造一个自己的专属长记忆陪聊哦～

![](img/65486529839c53e6e4fa2a1be3595075.png)

### 本地部署

#### 下载代码仓库

+   打开本地终端，输入以下代码

```
 git clone https://github.com/volcengine/ai-app-lab.git
```

+   下载成功

![](img/ba7c63ca79284ba3bbac22b37a51791e.png)

#### 修改配置

+   找到下载的文件夹，修改ai-app-lab/demohouse/longterm_memory/backend/code/config.py 中的配置

![](img/a09dea242e03c1f7ddc1a9787f59086e.png)

![](img/89215ad77a52fb7709e49258a01e81da.png)

##### 修改ak，sk

+   进入[火山方舟控制台](https://console.volcengine.com/ark/region:ark+cn-beijing/overview?)，点击右上角个人中心，选择「API访问密钥」

![](img/cafd0c83e5d200dc8c7217323947efa8.png)

+   点击「新建密钥」，复制Access Key ID、Secret Access Key并填入文件对应处，修改后保存文件

[参考文档](https://www.volcengine.com/docs/84313/1254467)

![](img/22f234b1815cf6c8722b8f4e487b354b.png)

![](img/f2e0c4ee20fe2d02ae20b63a4ad2dccd.png)

##### 修改endpoint

+   进入[火山方舟控制台](https://console.volcengine.com/ark/region:ark+cn-beijing/overview?)，点击「在线推理」，选择「自定义推理接入点」->「创建推理接入点」

[参考文档](https://www.volcengine.com/docs/82379/1099522#594199f1)

![](img/cda9175e464d489abcef0dfa16366f7f.png)

+   创建 Doubao-1.5-pro-32K-character-250715 的endpoint

![](img/c4dcde12bfddc10202216e5767c30b1d.png)

![](img/1e5860216b1bf3996033afabe017b3ea.png)

+   创建  Doubao-embedding 的endpoint

![](img/a111ed961b7bee9709525d9b790682f8.png)

+   创建 DeepSeek-V3 的endpoint

![](img/e368e14a57173647172bbf73398dca73.png)

+   创建好三个推理接入点后，复制接入点ID填入对应的地方，修改后保存文件

![](img/4b7b6beb72b83363c12a82f8b561dac5.png)

![](img/029ab0a84654fc908563bba0d4521227.png)

##### 开通 VikingDB 向量库

+   进入 [Viking DB 向量库](https://console.volcengine.com/vikingdb/region:vikingdb+cn-beijing/home)，开通向量数据库和记忆库两个服务

[参考文档](https://www.volcengine.com/docs/84313/1254444)

![](img/6df5b942404469ebbddf68eeb81ff548.png)

![](img/128488487a8d52564dd5b4320b556080.png)

#### 安装后端依赖

##### 切换目录

+   找到下载的文件夹，打开终端，在终端中切换目录到 backend 中

```
cd /ai-app-lab/demohouse/longterm_memory/backend    #根据文件所在位置进行修改
```

##### 创建虚拟环境

```
python3.11 -m venv .venv  **  #项目要求虚拟环境的p**ython版本&gt;=3.9,&lt;3.12.0
```

##### 激活虚拟环境

```
source .venv/bin/activate    #每次启动后端都要先激活虚拟环境
```

##### 安装依赖

```
pip install poetry==1.6.1
poetry install
```

##### 启动后端

```
bash run.sh
```

##### 启动前端

+   打开一个新的终端（启动后端的终端不要关闭），在新的终端中切换目录到frontend 中

```
cd /ai-app-lab/demohouse/longterm_memory/frontend    #根据文件所在位置进行修改
yarn install
yarn run dev
```

+   前端正常运行，复制Local地址到浏览器中打开可视化界面

![](img/4a623ef5236e064b27e81062d4a52f6f.png)

+   前端界面，可以开始对话啦～

![](img/c78a0f3844a412c096a3e63081e017f2.png)

#### 修改prompt（可选）

##### 修改用户画像抽取prompt

+   文件路径：/ai-app-lab/demohouse/longterm_memory/backend/code/prompt.py

![](img/80ac1e3b91af0a7ea90e89bde6e9901a.png)

+   可在代码中修改用户画像抽取prompt，修改后需重启后端生效

```
SUMMARY_PROMPT = &quot;&quot;&quot;你是一个个人信息管理器，负责从对话中提取用户的相关信息，并将其存储为简洁且结构化的事实（facts）。你的任务是准确捕捉用户的偏好、重要信息和计划，为未来的互动提供个性化支持。以下是你需要关注的具体信息类型及处理指示。

需要记住的信息类型：
1. 个人偏好：喜好、厌恶和特定偏好（食物、物品、活动、娱乐等）。  
2. 重要个人信息：如姓名、关系、重要日期等。  
3. 计划和意图：未来的事件、旅行、目标等。  
4. 活动和服务偏好：就餐、旅行、爱好等方面的偏好。  
5. 健康偏好：饮食限制、健身计划等健康相关信息。  
6. 职业信息：职位、工作习惯、职业目标等。  
7. 其他兴趣：用户分享的书籍、电影、品牌等爱好信息。

# 示例
## 示例一
输入：你好。  
输出：{{&quot;facts&quot; : []}}

## 示例二
输入：今天气温是18摄氏度。  
输出：{{&quot;facts&quot; : []}}

## 示例三
输入：你好，我在寻找一家位于什刹海的烤鸭店。  
输出：{{&quot;facts&quot; : [&quot;在寻找一家位于什刹海的烤鸭店&quot;]}}

## 示例四
输入：昨天，我和李明在下午三点见面，一起讨论了新项目。  
输出：{{&quot;facts&quot; : [&quot;昨天和李明在三点见面&quot;]}}

## 示例五
输入：我的名字是林瀚，我是一名软件工程师。  
输出：{{&quot;facts&quot; : [&quot;姓名是林瀚&quot;, &quot;职业是软件工程师&quot;]}}

## 示例六
输入：我最喜欢的电影是《花样年华》。  
输出：{{&quot;facts&quot; : [&quot;最喜欢的电影是《花样年华》&quot;]}}

# 返回格式说明
返回格式为JSON，包含“facts”键，对应一个字符串列表，记录所有提取到的事实和偏好。

# 遵循以下指示：
- 当前日期为 {datetime}。
- 不要返回示例或其他提示内容。
- 仅根据用户和助手之间的对话提取信息，忽略系统消息。
- 若没有发现相关信息，返回空列表。

## 输入
{chat_history}
&quot;&quot;&quot;
```

+   记忆抽取过程演示

![](img/0ad958bab82aba23008d2092fd7894c3.png)

更新记忆

![](img/7102f0fd956f235052d7515c28484330.png)

记忆提取结果

![](img/7dd6f3ed820eb6c8cbeedf39210dd1ad.png)

基于更新的记忆进行对话

##### 修改虚拟角色prompt

+   文件路径：/ai-app-lab/demohouse/longterm_memory/backend/code/prompt.py

![](img/b2ff446b9be75841f77fd70374bf393f.png)

+   在代码中修改虚拟角色prompt

```
苏野，23岁插画师，双鱼座男生，总带着晒干的草木香。爱收集落叶标本，能叫出小区里每棵树的名字。说话像浸了温水，擅长从细节里捕捉情绪，朋友说他 “像春日公园的长椅，让人想坐下来慢慢聊”。
```

+   重新启动后端，通过和虚拟角色交互来查看虚拟人物prompt是否生效

修改前：

![](img/e2e6b40e47026af4ca5ac119d2e1484d.png)

修改后：

![](img/3e1f30faf6f8ec4bd0223216789cf4fa.png)

##### 修改开场白、人物名称和预置问题

+   文件路径：/ai-app-lab/demohouse/longterm_memory/frontend/src/demo/longTermMemory/components/Conversation/Welcome.tex

![](img/dd07bbefb7ffed91b4e0c717392d8ab5.png)

![](img/c1dee099b2211f95ab53aca4f8a85c8f.png)

+   在代码中修改对应内容，重新启动前端，前端界面发生变化

```
角色名称：长记忆-苏野
开场白：今天风里有槐花香呢，你那边现在是什么味道呀？
预置问题：[&#x27;你最近有没有发现什么不起眼却很治愈的小事？&#x27;, &#x27;如果能变成一种植物，你想成为什么？&#x27;]
```

![](img/94f1c5a5c504b06dd86c207a7d852b6d.png)