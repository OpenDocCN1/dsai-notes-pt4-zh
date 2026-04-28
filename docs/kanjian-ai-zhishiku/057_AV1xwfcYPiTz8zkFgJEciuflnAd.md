# Llama3本地部署

> 来源：[https://jlqyxzzvbk.feishu.cn/docx/W66XdSbVOom0K7x7TaPc98mbnjg](https://jlqyxzzvbk.feishu.cn/docx/W66XdSbVOom0K7x7TaPc98mbnjg)

参考 https://www.cnblogs.com/zepc007/p/18152751

1、安装Llama3（命令行模式）

2、安装WebUI

docker pull ghcr.io/open-webui/open-webui:main

docker run -d -p 3000:8080 -e OLLAMA_BASE_URL=https://宿主机IP:11434 -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main