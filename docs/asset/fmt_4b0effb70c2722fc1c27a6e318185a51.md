
# 如何查看 cookie 饼干是否过期、被封

> 来源：[https://docs.feishu.cn/docx/Ng3mdMvp7oUtwCxOgNkcSeyLnNc](https://docs.feishu.cn/docx/Ng3mdMvp7oUtwCxOgNkcSeyLnNc)

参考 [Clewd版本功能说明](https://rentry.org/teralomaniac_clewd_ReleaseNotes)

1. 打开你的 `Clewd`，先点 `stop` 停止运行
2. 点开 `config.js`
3. `Cookiecounter` 改为 -1
4. 打开你的 `clewd`，点 `run`
5. 等他跑完，看输出结果
6. 看完结果没问题了，再改回原来的数字，点 `run` 运行

输出是类似这样的界面：

![](img/c47eb142a512b68c738f0b695678fa60.png)

## 关键词解释

+ `Banned!` 就是被封了
+ `warning` 因各种原因而被警告，比如黄标、封 1 天、永封等
+ `expired` 就是 cookie 过期，需要重新抓
+ `Overlap` 是跟别的 cookie 重复了，同一个邮箱抓了多个 cookie，这些饼干都共用 cd，这是没有意义的
+ `Unverified!` 没通过短信验证之类的号
+ `Restricted!` 此号被黄标警告了
+ `Exceeded limit!` 此账号到达对话上限，歇几个小时吧

没提示就是一切正常

## 其他关键词

+ `Magenta` 是 11 月 16 号左右 claude 新出的模型，现在可能所有人都是这个模型了（这个关键词应该不会再出现了，现在大家都是 claude 2.1）
+ `index` 代表当前是第几个 cookie，方便你删除被封的 cookie、用邮箱重新抓取过期 cookie
+ `[progress]` 代表当前查询 cookie 的进度，比如你有 10 个号，它就每次 10% 的往上涨，100% 代表全部 cookie 都查询完毕
+ `length` 表示你填入 `CookieArray` 的 cookie 数量
