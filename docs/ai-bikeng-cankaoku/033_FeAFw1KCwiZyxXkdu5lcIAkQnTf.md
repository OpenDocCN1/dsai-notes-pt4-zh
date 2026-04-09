# 玩转高级搜索

> 来源：[https://xodnytdcaw.feishu.cn/docx/HYOadVcSAonzYyxgaVPcnmqXnSd](https://xodnytdcaw.feishu.cn/docx/HYOadVcSAonzYyxgaVPcnmqXnSd)

## Listary

选项-特-网络-搜索

| 项目 | 关键词 | URL | Custom |
| --- | --- | --- | --- |
| 油管 | yt | http://youtube.com/results?q={query} | http://suggestqueries.google.com/complete/search?client=youtube&amp;ds=yt&amp;client=firefox&amp;q={query} |
| 推特 | t | https://twitter.com/search?q={query} | http://suggestqueries.google.com/complete/search?client=witter&amp;ds=yt&amp;client=firefox&amp;q={query} |
| B站 | bl | https://search.bilibili.com/all?keyword={query}&amp;order=click | https://search.bilibili.com/all?keyword={query}&amp;tids=36 |
| 小红书 | xhs | https://www.xiaohongshu.com/search_result?keyword={query} | https://www.xiaohongshu.com/search_result?keyword={query} |
| GitHub | git | https://github.com/search?q={query} | cmd /k gh {query} |
| 抖音 | dy | https://www.douyin.com/search/{query} | cmd /k tt {query} |

## Cherry Studio

ALI+E快捷助手

解释/翻译

## TabMark-Bookmark

一个问题，让多个X帮给你答案

X=传统搜索/AI/社媒

## yt-dlp下载视频

微信输入法添加常用语输入下载命令

```
yt-dlp --cookies &quot;D:\web3/cookies.txt&quot; -f &quot;bestvideo+bestaudio&quot; --merge-output-format mp4 
```

## 打开常用网站bat

```
@echo off
:: 设置编码为 ANSI 防止中文乱码
chcp 936 &gt;nul
title 大强快捷启动工具

:: 设置绿色字体
color 0A

:main_menu
cls
:: 调整窗口宽度和居中显示
mode con cols=80 lines=25

echo ==============================================================
echo                       大强同学（www.dqtx.cc）           
echo ==============================================================
echo    [1] 大强同学@官网                    https://dqtx.cc/
echo    [2] 大强同学@远程支持                https://742112.xyz
echo    [3] 大强同学@粉丝群                  http://110.42.45.244:48317/
echo    [4] 大强同学@Notion                  https://notion.dqtx.cc
echo    [5] 大强同学@GitHub                  https://github.com/dqtx760
echo    [6] 大强同学@CSDN                    https://blog.csdn.net/2402_82616859
echo    [7] 文章封面工具                     https://picprose.pixpark.net/zh
echo    [8] 文章排版工具                     https://editor.mdnice.com/
echo ==============================================================
echo                 按数字 1-8 直接跳转，按 0 退出
echo.

choice /c 123456780 /n /m &quot;请选择:&quot;
if %errorlevel% equ 1 start &quot;&quot; &quot;https://dqtx.cc/&quot; &amp; goto main_menu
if %errorlevel% equ 2 start &quot;&quot; &quot;https://742112.xyz/&quot; &amp; goto main_menu
if %errorlevel% equ 3 start &quot;&quot; &quot;http://110.42.45.244:48317/&quot; &amp; goto main_menu
if %errorlevel% equ 4 start &quot;&quot; &quot;https://discreet-ringer-ef6.notion.site/Derek-Zhao-1a5351994e4080e79edcda09672ef592&quot; &amp; goto main_menu
if %errorlevel% equ 5 start &quot;&quot; &quot;https://github.com/dqtx760&quot; &amp; goto main_menu
if %errorlevel% equ 6 start &quot;&quot; &quot;https://blog.csdn.net/2402_82616859?type=blog&quot; &amp; goto main_menu
if %errorlevel% equ 7 start &quot;&quot; &quot;https://picprose.pixpark.net/zh&quot; &amp; goto main_menu
if %errorlevel% equ 8 start &quot;&quot; &quot;https://editor.mdnice.com/&quot; &amp; goto main_menu
if %errorlevel% equ 9 exit

```

## 一键打开两个PSDbat

```
@echo off
:: 功能：静默打开两个PSD文件，3秒后自动关闭窗口（无错误提示）
title PSDAuto
color 0A
echo 正在打开 PSD 文件，请稍候...

:: 切换到目标目录（错误静默）
pushd &quot;D:\文件存档\图片\封面PSD&quot; 2&gt;nul

:: 尝试打开文件（不检查是否存在）
start &quot;&quot; &quot;bg.psd&quot; 2&gt;nul
ping -n 2 127.0.0.1 &gt;nul  &amp;:: 延迟约1秒
start &quot;&quot; &quot;cover.psd&quot; 2&gt;nul

:: 3秒后关闭窗口（静默）
ping -n 4 127.0.0.1 &gt;nul
exit

```

引流图

https://photofunia.com/cn/categories/lab/beach-sign

https://photofunia.com/cn/categories/lab/wooden_sign

https://photofunia.com/cn/categories/lab/airline