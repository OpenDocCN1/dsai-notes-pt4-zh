
# 谜撰-趟坑经验

> 来源：[https://datawhaler.feishu.cn/docx/YxAEd9L3boLIJHxv6TNc5arVnpc](https://datawhaler.feishu.cn/docx/YxAEd9L3boLIJHxv6TNc5arVnpc)

<https://datawhaler.feishu.cn/sync/FsXmd9Oifs2Jx4bq3CCcPXE8npg>

> 本文档由 写作贡献~

> 欢迎大家评论、点赞、一起讨论~

> ：我先说下必备的条件，然后说下趟坑的经验，写的不好请各位多见谅！（这里有坑的地方我会标注出来）

## 必备条件

1. 需要一个魔搭社区的账号；
1. 开通以后会赠送几个CPU或者GPU（十几个小时）把，这里主要说下DSW（交互式建模）环境；
![](img/b93e8f1fcf8ffd103557374c92e5a03a.png)
1. 选择CPU开启笔记本以后：
1. 这个环境不是持久化的，也就是每次进去都是新的笔记本；（坑：开不开心，意不意外，惊不惊喜？）
1. 建议用的时候还是要了解下环境的搭建问题；比如`pip` 安装命令；`venv`虚拟环境或者`conda`环境；

参考资料：
https://blog.csdn.net/weixin_40922744/article/details/103721870

https://zhuanlan.zhihu.com/p/60647332

1. 需要一个阿里云的账号，并且开通灵积大模型服务；（主要获取`APIKEY`）

## 然后直接动手从开发版进行自己的编剧和创作

1. 首先理解一下谜撰游戏的设定和安排；
1. 主要由剧情和角色组成；
1. 剧情由：开场白，主角进场发言，主角退场发言（主要就这三个）
1. 每个剧情都应该有一个解谜为目的，所以你要设定好剧情完成条件
1. 角色的设定有线索卡，和剧情相关；
1. 那么理解上面我们就可以用小助手帮助创建剧情了。
1. 这个开发版是可以直接使用小助手，你就给他一段话，让它根据你的画生成剧情；
1. 那么这个剧情注意会冲掉你前面的剧情我一直以为是添加在前面剧情后面，后来发现不是，其实他就只能生成一幕剧情，比如你写好了1，2，3；那么我用了小助手以后就是 重新根据你的提示写1，并且这个剧情和角色会把你原来写的都冲了（坑：开不开心，意不意外，惊不惊喜？）
1. 话说回来其实还是方便的，但是这个谜撰估计是使用的人太多，结果呢经常等半天，我每次看都1000秒了，剧情loading不出来，整个界面假死状态（坑：开不开心，意不意外，惊不惊喜？）
1. 那么解决办法呢？有的。其实就是你自己制作一个开发版，甚至不开放不就没人和你抢资源了。看我后面把代码稍微改动下即可；(参考`3.c`的代码改动）

## 涉及到的更改

1. 界面需要修改下，路径：`./game_app.py`

```
def format_cover_html(name="", bot_avatar_path="assets/bg.png"):
    config = {
        'name': f"谜馔：{name}" if name else "谜馔",
        'description': '这是一款模拟餐馆经营的解密推理游戏, 快来开始吧😊',
        'introduction_label': "<br>玩法介绍",
        'introduction_context': "玩家将协助卢克·天行者，在餐厅中收集线索进行解谜<br>"
                                "怎样找到公主获得帮助<br>"
                                "怎样在欧比旺大师下进行原力觉醒<br>"
                                "......<br>"
                                "通过你的智慧去收集线索，打败西斯尊主<br>"
                                "拯救全世界！"
    }
    image_src = covert_image_to_base64(bot_avatar_path)
    return f"""
```

1. 在`logo`的路径直接替换`logo`：`./asset/bg.png`
1. 弄个自己的开发版本：`./app.py`（原来`-toc`改为`-tod`）

```
os.system(f"python game_app.py -tod --name {game_name}")
```

1. 关于提交代码

前面的克隆下来代码就不说了

需要提交的话输入下面的命令

```
git add . ##对当前目录及子目录进行操作
git status ##显示目前将会提交的文件详情
git commit -m "modify" #进行提交的文件备注
git push #提交
```

1. 关于配置；

![](img/3d99f45e830bee898595f74349c14e14.png)
