# COW初级使用教程

> 来源：[https://my.feishu.cn/docx/KQQAdLryeoiVUixMhKxcScOjnqf](https://my.feishu.cn/docx/KQQAdLryeoiVUixMhKxcScOjnqf)

先来一个初级教学，本文适用于纯新手小白，不用懂原理，一步步复制操作即可拥有自己的“微信贾维斯”。

本文分为 4 大模块，分别是：

1、申请 LLM（语言大模型）的 API（接口）。

2、注册云服务器（就是让它挂在网络上，不开启随身设备它也能自己运行）

3、配置环境（云服务器的配置）

4、微信机器人配置

---

**详情讲解：**

## **一、申请 LLM**

教程中使用的是智谱 AI。这步的意义是获取外部（微信）调用 AI 的接口。

智谱 AI 是由清华大学计算机系技术成果转化而来的公司，致力于打造新一代认知智能通用模型。

官网地址：https://open.bigmodel.cn/

点击“开始使用”。

![](img/4b79cbc8751217b82e19609d0f7528c0.png)

注册并登录账号：

![](img/f8630b9219da817d1eb069ac5c48aaa5.png)

点击右上角，查看 Key。

![](img/14f8acdcf4619a56465165f8a374a563.png)

点击这里的复制，获取 API Key。

![](img/cce021819dc75fdddac0f6bfa1ac6414.png)

至此，第一步完成，把获取的 API Key 放到其他任意文本软件中记录，方便后续查找。类似这样，养成好习惯，避免之后找不到了。

这里我把需要记录的内容，都以#放入小本本记录 来进行标识。

此处是#放入小本本记录 1 号内容 - 大模型接口秘钥。

注意：任何 Key 相关的不要随意透漏给其他人，是使用的唯一标识，分享时，注意打马。

![](img/7e593520dc24410fb05d97484973115d.png)

## **二、申请云服务器**

不论腾讯云还是阿里云，华为云等，都有免费白嫖试用的，用完一个再用其他的重新部署一遍，美滋滋～

教程中使用的是腾讯云。

官网地址：https://cloud.tencent.com/act/pro/free

直接选这个。

![](img/2644d1371abdbee096488fe64777ddf6.png)

弹框中选择【宝塔】，然后点击【试用】

![](img/599bd8d30e8898d4027b65b85a3b6890.png)

控制台界面中，点击【登录】

![](img/6dbee8e095894cf3500dd947bb89622b.png)

直接点击【登录】，以后再用时候微信扫码就行。

![](img/ec2f4f07f87dfb45903ee9175ecded53.png)

登录后，就在当前页面，复制下方代码粘贴进入图示位置，然后点击回车。此处输出的内容，也要保存好。

此处是#放入小本本记录 2 号内容 - 云服务器链接地址和账号密码。

```
sudo /etc/init.d/bt default
```

![](img/b8494a581067a9e842fe86f05af49465.png)

然后我们要打开防火墙，避免打不开服务器连接。

在这里点击空白区域，进入【查看详情】。

![](img/cb08fb86b0f3ad5a47b0d463f9807b98.png)

点击【防火墙】，再点击【添加规则】

![](img/9b8c6d91991b75067fe2514935f46c74.png)

把这里一模一样的填好。

来源处点击一下，【下拉框中选择】，其他均为【手动输入】。

![](img/4e75d3ddaa0090147d0f7b00e2fdda7f.png)

## **三、宝塔环境配置**

输入这里的外网地址，和账号密码。

![](img/f3863afdb65395f466a238c1b503a5a6.png)

是这样一个界面。

![](img/8b115c161308c879f9d2dba341531d18.png)

第一次进入会让你绑定一下，点击免费注册，注册完成后，返回此页，登录账号。

![](img/05a112cad0e04a3bbcf2bdb017084c47.png)

首次会有个推荐安装，只安装这 2 个即可。其他的取消勾选。

![](img/2146f48d5a95a34fcb131953728f8fa4.png)

点击“软件商店”，会让你关联，点击【点击查看】按钮，跳转进入腾讯云，

![](img/2b158ea86abb549966868287f9aab1de.png)

此处是#放入小本本记录 3 号内容 - 腾讯云访问秘钥。

点击“API 密匙关联”--点击“新建秘钥”。复制这两个，一定保存好。

![](img/cbd35a7e62ba3781cae78afd0d47bcae.png)

点击确定后，列表上会有一个 APPID，这三个串码，依次对应粘贴到刚才的登录窗口里即可。

![](img/381210d90682834c0663775ace9d3b8b.png)

![](img/1d3fa75ca40bfd84418187d95d3df4e5.png)

![](img/a95ac3debbf5dd9e010de90d2890ae30.png)

**提醒：**

+   下边出现代码，复制的时候，注意复制全。

+   代码我已经分好步骤，每次只需要粘贴一行，然后点击一次回车。

+   回车后，只有最左边显示[  ]中括号时，才是上一个命令执行完毕了。没有出现[ ]中括号对话前缀时，不要操作。

+   如果你发现 ctrl+v 粘贴不进去，试试 shift+ctrl+v 粘贴。

首先，点击【终端】。

![](img/ec82ed963fef871e56a47785b779df0a.png)

输入以下链接，注意常规粘贴不上的话，使用 shift+ctrl+v 粘贴。

```
cd /root || exit 1
```

下方两行一次性全部粘贴进入，然后点击回车，等待下载完成。

```
echo &quot;开始安装 Anaconda...&quot;
wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh
```

执行安装

```
bash Anaconda3-2021.05-Linux-x86_64.sh -b -p /root/anaconda
```

下载界面如图所示

![](img/bcc66d24e5d699a2b155c6e38606f211.png)

安装界面如图所示，要等最下方的 done 输出完成后再输入。

![](img/74565ada62b203a69d0f89f47abe93d6.png)

```
rm -f Anaconda3-2021.05-Linux-x86_64.sh
```

继续粘贴，注意看图完成效果，再继续。

```
/root/anaconda/bin/conda create -y --name AI python=3.8
```

![](img/945058fbd735820c0f5569077fce73fd.png)

继续，一行一行依次粘贴，依次回车：

```
/root/anaconda/bin/source activate AI
echo &#x27;source /root/anaconda/bin/activate AI&#x27; &gt;&gt; ~/.bashrc
source ~/.bashrc
```

执行完成后，你会看到，最左侧出现了（AI）的字符。

如果第一行出现了错误，不要紧，直接跳过下一步，以防万一。

![](img/b7b1c02612f440f72969d80c91db0c78.png)

## 四、配置COW（Chatgpt-on-Wechat）

继续，一行一行依次粘贴，依次回车：

```
cd /root
```

![](img/3987182a8eb227d25b2a90c4bda00e11.png)

这个注意一定要粘贴完整，这里容易粘贴不全，这里链接微信机器人代码库。

```
git clone https://github.com/zhayujie/chatgpt-on-wechat
```

![](img/0f64859e34824c0ba148dd39876f3f98.png)

出现下方的样子，就是成功了。如果失败，或者没反应，刷新一下，重新再试一次

![](img/87b93ab92510d99f03113a69304730e4.png)

继续一行一行，依次输入：

```
cd chatgpt-on-wechat/
pip install -r requirements.txt
```

![](img/f8fa28b399839a7054f4a728c4b31fd0.png)

等待执行完成，如上图后，继续粘贴：

```
pip install -r requirements-optional.txt
```

![](img/6e5d1ad2d8e5fd2516f84716126f6900.png)

继续输入

```
pip3 install zhipuai
```

![](img/6580fefd89788589b41c103b67c997d5.png)

上边的都执行完成后。现在我们到【文件】菜单中去执行，点击文件 - 找到【root】 ，进入 root 文件夹 ，找到【chatgpt-on-wechat】文件夹，并进入。

![](img/0a189cb19e0293416767605b5d15ba74.png)

点击文件夹上方功能栏中的【终端】（注意，不是左侧一级菜单里的终端，是文件夹上方那一行的终端电脑）

![](img/a3f5fa0dfc8c9ad8e38b2ecf59f48f5e.png)

粘贴进入  ，点击回车。点击后，关闭此弹窗。

```
cp config-template.json config.json
```

刷新页面。在当前目录下，找到 config.json 文件。如下图：

![](img/19f7d78276fe4a8e3f7afbb080356096.png)

**双击这个文件，我画红框的地方是需要修改的地**方。* 因为这个地方对格式和符号要求比较严格，如果是小白，建议你直接复制我下方的配置。

![](img/271857fd243e879b817d90714eb5047b.png)

+   删除上图文件里的所有代码。

+   复制下边的代码，粘贴到文件里。

+   找到第 4 行，把一开始就让你注册并保存好的智谱 API key，粘贴到双引号里。

+   这也是你唯一需要修改的地方。修改完之后，点击保存，关闭文件。

+   [※ COW主目录Config说明](https://tffyvtlai4.feishu.cn/wiki/Ti7uwsKgiiW6bpk2nmQcVhQ8nnb)果有报错，可能是更新导致，请查看   是否缺少配置。

```
{
  &quot;channel_type&quot;: &quot;wx&quot;,  
  &quot;model&quot;: &quot;glm-4&quot;,  
  &quot;zhipu_ai_api_key&quot;: &quot;把你一开始注册的智谱AI的API key，粘贴到这里&quot;,  
  &quot;zhipu_ai_api_base&quot;: &quot;https://open.bigmodel.cn/api/paas/v4&quot;,  
  &quot;text_to_image&quot;: &quot;dall-e-2&quot;,  &quot;voice_to_text&quot;: &quot;openai&quot;,  
  &quot;text_to_voice&quot;: &quot;openai&quot;,  
  &quot;proxy&quot;: &quot;&quot;,  &quot;hot_reload&quot;: false,  
  &quot;single_chat_prefix&quot;: [&quot;&quot;],  
  &quot;single_chat_reply_prefix&quot;: &quot;&quot;,  
  &quot;group_chat_prefix&quot;: [&quot;@bot&quot;],  
  &quot;group_name_white_list&quot;: [&quot;ALL_GROUP&quot;],  
  &quot;image_create_prefix&quot;: [&quot;画&quot;],  
  &quot;speech_recognition&quot;: true,  
  &quot;group_speech_recognition&quot;: false,  
  &quot;voice_reply_voice&quot;: false,  
  &quot;conversation_max_tokens&quot;: 2500,  
  &quot;expires_in_seconds&quot;: 3600,  
  &quot;character_desc&quot;: &quot;你是ChatGPT, 一个由OpenAI训练的大型语言模型, 你旨在回答并解决人们的任何问题，并且可以使用多种语言与人交流。&quot;,  
  &quot;temperature&quot;: 0.7,  
  &quot;subscribe_msg&quot;: &quot;感谢您的关注！\n这里是AI智能助手，可以自由对话。\n支持语音对话。\n支持图片输入。\n支持图片输出，画字开头的消息将按要求创作图片。\n支持tool、角色扮演和文字冒险等丰富的插件。\n输入{trigger_prefix}#help 查看详细指令。&quot;,  
  &quot;use_linkai&quot;: false,  
  &quot;linkai_api_key&quot;: &quot;&quot;,  
  &quot;linkai_app_code&quot;: &quot;&quot;
}
```

依然在当前文件，【终端】里进行，依次复制粘贴进入：

```
cd plugins/godcmdcp config.json.template config.json
```

![](img/b017cee7695a011b9b08008a1e98b75f.png)

操作完成后，退出窗口，刷新一下。进入/root/chatgpt-on-wechat/plugins/godcmd，下边是依次进入窗口的路径：

![](img/d51172aee4b1880f53dceea8d3970427.png)

双击 config.json，进入后，设置下你的 password 和 admin_users 可以设置为和我一样的，后边再改，点击保存后关闭。

![](img/46e4e20ee4428e82790cfdc4ec133432.png)

重新回到/root/chatgpt-on-wechat/这个文件路径下，点击终端，继续依次粘贴：

```
touch nohup.out
nohup python3 app.py &amp; tail -f nohup.out
```

![](img/c02faa6faf854b35eb8b2bbcbf031232.png)

![](img/795f80e2e6b2d1a70ac4ae661918324e.png)

最下方会出现一个二维码，使用你想要做机器人的微信扫码登录即可。

![](img/c056578dd5f108a33d32c31ce20bdb0f.png)

登录成功后，找另一个人私聊或者在群中@你，就可以看到机器人的正常回复了。这个时候就是已经通了。

2、如果你现在想为这个 AI 赋予什么样的提示词，在如图路径下**的 Config.josn **中修改：["character_desc": "你是 ChatGPT， 一个由 OpenAI 训练的大型语言模型， "，]。中文部分，便是设置 AI 提示词的地方你可以进行更改。

![](img/524e05149d8193f8fcdd756b5579a3e3.png)

3、此后，如果你对文件内容进行了任何更改，都需要返回宝塔“首页 - 右上角 - 点击重启，重启一下服务器”。

4、重启后，在“文件”的【终端】里，直接输入下方代码，重新扫码登录即可。

```
nohup python3 app.py &amp; tail -f nohup.out
```

再次整理本文 #记录小本本 内容：

1、LLM 的 API key

2、云服务器配置

3、腾讯云访问秘钥

4、每次重启云服务器后，需要在 chatgpt-on-wechat 目录下执行运行 log 文件启动

```
nohup python3 app.py &amp; tail -f nohup.out
```

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>