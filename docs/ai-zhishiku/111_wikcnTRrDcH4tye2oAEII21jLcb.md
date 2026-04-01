# 【AutoGPT】安装教程

> 来源：[https://my.feishu.cn/docx/BHXNdBFBxosj1RxUqcocfw9zncf](https://my.feishu.cn/docx/BHXNdBFBxosj1RxUqcocfw9zncf)

AutoGPT官方安装流程：

github.com/Torantulino[/Auto-GPT

AgentGPT体](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbU9tT19zTEhkZThaMXBWRnZDNUVnNE9tdFAwd3xBQ3Jtc0tucTRfVXRnbERfeWVVS0NsN0R1QlZqREJvdjBndjVoeWRHMVVEUU1ubmY1Tng2Xy1aWVd1TEtNekpRMVU2VlM1U0JIcGo5czlkWGJoTmotb25MWDFvNnVIX3pRM0pvX0I0NE9abE14VjQtRUVrTm04RQ&q=https%3A%2F%2Fgithub.com%2FTorantulino%2FAuto-GPT.git&v=KXZHiZPQ-jI)验地址：

agentgpt.reworkd.ai

AutoGPT本地安装流程：

1. 安装 Git for Windows，地址：gitforwindows.org

2. 安装 Python 3.11.3，地址：www.python.org

3. 在D盘新建autogpt文件夹

4. 进入 github.com/Torantulino/Auto-GPT

5. 在 D:\autogpt文件夹上方对话框，调出CMD, 输入git clone https://github.com/Torantulino/Auto-G...

6. 在 D:\autogpt\autogpt文件夹，调出CMD, 输入pip install -r requirements.txt

7. 在D:\autogpt\autogpt文件夹中找到，env.template文件。

8. 进入Pinecone.io，注册账号，生成一组API Key。

9. 把your pinecone api key替换为刚才生成的API Key。

10. 进入Pinecone.io，复制Environment代码。把your pinecone region替换为Environment代码。

11. 进入platform.openai.com/account/api-keys，获取API Key.

12. 点击Create new secret key,复制API值，替换原有的openai api key。

13. 保存文本文件，并把把.env.template文件替换为.env

14. 运行D:\autogpt\autogpt python scripts/main.py，执行auto GPT.

15. 运行D:\autogpt\autogpt python scripts/main.py --speak, 可以播放语音