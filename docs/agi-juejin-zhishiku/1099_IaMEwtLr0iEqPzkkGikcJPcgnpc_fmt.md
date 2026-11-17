
# 一文读懂如何基于Ollama在本地运行LLM

> 来源：[https://agijuejin.feishu.cn/docx/W5AydAX0FoUcNRxoG7Rc5NJinQd](https://agijuejin.feishu.cn/docx/W5AydAX0FoUcNRxoG7Rc5NJinQd)

> 作者：Luga Lee 架构驿站

> 原文：https://mp.weixin.qq.com/s/DC05r0pI-mFcJiWiWPFsDQ

> “Mixtral有46.7B的总参数，但每个令牌只使用12.9B参数。因此，它以与12.9B型号相同的速度和成本处理输入并生成输出。”

## 为什么要使用 Ollama 开源项目?

在当今人工智能技术飞速发展的时代，大型语言模型(LLM)无疑已成为焦点炯炯的科技明星。自从ChatGPT的推出以来，其强大的自然语言理解和生成能力便惊艳了全球，成为人工智能商业化进程中的杰出代表。

然而，这一领域并非只有诸如 OpenAI的GPT、Anthropic的Claude、谷歌的Gemini等知名商业模型才折桂争芳。事实上，开源LLM同样正在蓬勃发展，为用户提供了更加多元、灵活和经济的选择。无论是希望尝试当下无法作为公共服务获取的前沿模型，还是追求绝对的数据隐私保护，亦或是想以最优成本获取满足需求的性能，开源LLM无疑都是不二之选。

而在开源LLM的部署和使用方面，Ollama则可谓是独树一帜的优秀方案。作为一个功能强大且使用便捷的工具套件，Ollama不仅为用户提供了精心策源的优质模型备选，更允许直接引入自有的自定义模型，确保了最大程度的灵活性和定制空间。

无论是本地部署还是云端服务，Ollama的简洁设计和完备功能确保了LLM的无缝集成和高效运行。用户可以轻松调用各种计算资源，如GPU加速等，充分释放模型的算力潜能。同时，Ollama 还提供了Web UI、API等多种友好的交互界面，使得用户可以根据不同场景和需求,自如切换使用模式。

更为重要的是，Ollama 在隐私和安全性方面也作出了完备的部署。通过本地化运行和数据加密等策略，确保了对话内容和用户数据的完全保密，有效回应了人们对数据安全日益增长的忧虑。在一些涉及敏感信息或隐私法规的领域，Ollama无疑是LLM部署的最佳选择。

而在经济性方面，Ollama 同样大放异彩。庞大的开源模型储备库使得用户可以根据实际需求，自由挑选大小适中、性价比极高的模型。同时，Ollama的轻量级设计也减小了部署和运维的系统开销，进一步降低了总体成本。

总的来说，Ollama 正在为开源LLM的发展和应用插上腾飞的翅膀。通过其灵活的定制能力、出色的性能表现、良好的隐私保护和卓越的经济性，Ollama必将吸引越来越多的个人和企业用户加入到开源LLM的阵营，共同推动这一人工智能前沿技术的创新和落地。在通往人工通用智能的道路上，开源LLM和Ollama将扮演越来越重要的角色。

## Ollama 开源项目独有的优势

运行Ollama的优势众多，它是一个备受欢迎的开源项目，提供了在本地部署和运行大型语言模型的能力。其独特特性可参考如下所示：

1. 积极的维护和更新

Ollama 项目得到积极的维护和更新。开发团队不断改进和优化该项目，确保其性能和功能的最新状态。这意味着我们可以依赖于稳定的支持，并享受到最新的改进和修复。

2. 庞大而活跃的社区支持

Ollama拥有一个庞大而活跃的社区。您可以加入这个社区，与其他用户和开发者交流、分享经验和解决问题。社区的活跃性意味着我们可以获得及时的帮助和支持，共同推动项目的发展。

3. 灵活的部署选项

Ollama提供了便捷的docker容器，使我们可以在本地运行LLM，而无需担心繁琐的安装过程。这为您提供了更大的灵活性，可以根据自己的需求选择合适的部署方式。

4. 简单易用

Ollama的使用非常简单。它提供了直观的接口和清晰的文档，使得初学者和有经验的开发者都能够快速上手。我们可以轻松地配置和管理模型，进行文本生成和处理任务。

5. 多样化的模型支持

Ollama支持多个模型，并不断添加新的模型。这为我们提供了更多的选择，可以根据具体需求选择适合的模型。此外，您还可以引入自定义模型，以满足个性化的需求。

6. 强大的前端支持

Ollama 还提供了优秀的第三方开源前端工具，称为"Ollama Web-UI"。这个前端工具使得使用Ollama更加便捷和直观，我们可以通过它进行模型的交互和可视化展示。

7. 多模态模型支持

除了固有的文本处理特性支持，Ollama还支持多模态模型。这意味着我们可以处理多种类型的数据，例如图像、音频和视频等。这种多模态的支持为您提供了更广泛的应用领域和更丰富的功能。

综上所述，运行Ollama带来了诸多优势。积极的维护和更新、庞大而活跃的社区支持、灵活的部署选项、简单易用的接口、多样化的模型支持、强大的前端工具以及多模态模型的能力，都使得 Ollama 成为一个强大而受欢迎的工具，为用户提供了丰富的功能和便利的开发体验。

## 本地运行 Ollama 基本操作步骤

### 1. 部署 Ollama

这里，我们以 Mac 平台为操作环境，基于 Docker 进行安装部署，具体可参考如下命令：

```
[lugalee@Labs ~ ] % docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```



```
[lugalee@Labs ~ ] % docker ps
CONTAINER ID   IMAGE           COMMAND               CREATED      STATUS          PORTS                                           NAMES
cef2b5f8510c   ollama/ollama   "/bin/ollama serve"   7 days ago   Up 31 seconds   0.0.0.0:11434->11434/tcp, :::11434->11434/tcp   ollama
```

```
[lugalee@Labs ~ ] % docker logs -f cef2b5f8510c
Couldn't find '/root/.ollama/id_ed25519'. Generating new private key.
Your new public key is: 

ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJjLw5r9XVTA5HWmFMiVR+ulwVj2jtWLWgcD1yTOZb03

2024/05/27 07:08:17 routes.go:1008: INFO server config env="map[OLLAMA_DEBUG:false OLLAMA_LLM_LIBRARY: OLLAMA_MAX_LOADED_MODELS:1 OLLAMA_MAX_QUEUE:512 OLLAMA_MAX_VRAM:0 OLLAMA_NOPRUNE:false OLLAMA_NUM_PARALLEL:1 OLLAMA_ORIGINS:[http://localhost https://localhost http://localhost:* https://localhost:* http://127.0.0.1 https://127.0.0.1 http://127.0.0.1:* https://127.0.0.1:* http://0.0.0.0 https://0.0.0.0 http://0.0.0.0:* https://0.0.0.0:*] OLLAMA_RUNNERS_DIR: OLLAMA_TMPDIR:]"
time=2024-05-27T07:08:17.576Z level=INFO source=images.go:704 msg="total blobs: 0"
time=2024-05-27T07:08:17.576Z level=INFO source=images.go:711 msg="total unused blobs removed: 0"
time=2024-05-27T07:08:17.576Z level=INFO source=routes.go:1054 msg="Listening on [::]:11434 (version 0.1.38)"
time=2024-05-27T07:08:17.576Z level=INFO source=payload.go:30 msg="extracting embedded files" dir=/tmp/ollama2516332727/runners
time=2024-05-27T07:08:19.764Z level=INFO source=payload.go:44 msg="Dynamic LLM libraries [cuda_v11 cpu]"
time=2024-05-27T07:08:19.766Z level=INFO source=types.go:71 msg="inference compute" id=0 library=cpu compute="" driver=0.0 name="" total="2.5 GiB" available="1.3 GiB"
2024/05/27 08:29:29 routes.go:1008: INFO server config env="map[OLLAMA_DEBUG:false OLLAMA_LLM_LIBRARY: OLLAMA_MAX_LOADED_MODELS:1 OLLAMA_MAX_QUEUE:512 OLLAMA_MAX_VRAM:0 OLLAMA_NOPRUNE:false OLLAMA_NUM_PARALLEL:1 OLLAMA_ORIGINS:[http://localhost https://localhost http://localhost:* https://localhost:* http://127.0.0.1 https://127.0.0.1 http://127.0.0.1:* https://127.0.0.1:* http://0.0.0.0 https://0.0.0.0 http://0.0.0.0:* https://0.0.0.0:*] OLLAMA_RUNNERS_DIR: OLLAMA_TMPDIR:]"
time=2024-05-27T08:29:29.102Z level=INFO source=images.go:704 msg="total blobs: 0"
time=2024-05-27T08:29:29.102Z level=INFO source=images.go:711 msg="total unused blobs removed: 0"
time=2024-05-27T08:29:29.102Z level=INFO source=routes.go:1054 msg="Listening on [::]:11434 (version 0.1.38)"
time=2024-05-27T08:29:29.103Z level=INFO source=payload.go:30 msg="extracting embedded files" dir=/tmp/ollama2886309190/runners
time=2024-05-27T08:29:31.318Z level=INFO source=payload.go:44 msg="Dynamic LLM libraries [cpu cuda_v11]"
time=2024-05-27T08:29:31.320Z level=INFO source=types.go:71 msg="inference compute" id=0 library=cpu compute="" driver=0.0 name="" total="2.5 GiB" available="1.4 GiB"
[GIN] 2024/05/27 - 08:41:23 | 200 |     359.716µs |       127.0.0.1 | HEAD     "/"
[GIN] 2024/05/27 - 08:41:23 | 404 |     773.017µs |       127.0.0.1 | POST     "/api/show"
time=2024-05-27T08:41:31.891Z level=INFO source=download.go:136 msg="downloading 6a0746a1ec1a in 47 100 MB part(s)"
time=2024-05-27T08:41:38.716Z level=INFO source=images.go:1001 msg="request failed: Get \"https://dd20bb891979d25aebc8bec07b2b3bbc.r2.cloudflarestorage.com/ollama/docker/registry/v2/blobs/sha256/6a/6a0746a1ec1aef3e7ec53868f220ff6e389f6f8ef87a01d77c96807de94ca2aa/data?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=66040c77ac1b787c3af820529859349a%2F20240527%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20240527T084133Z&X-Amz-Expires=1200&X-Amz-SignedHeaders=host&X-Amz-Signature=618deb64019fbd08450a5b0dc842810c294fd9f7bafb22f3cb29046d2b1379db\": EOF"
time=2024-05-27T08:41:38.716Z level=INFO source=download.go:178 msg="6a0746a1ec1a part 45 attempt 0 failed: Get \"https://dd20bb891979d25aebc8bec07b2b3bbc.r2.cloudflarestorage.com/ollama/docker/registry/v2/blobs/sha256/6a/6a0746a1ec1aef3e7ec53868f220ff6e389f6f8ef87a01d77c96807de94ca2aa/data?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=66040c77ac1b787c3af820529859349a%2F20240527%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20240527T084133Z&X-Amz-Expires=1200&X-Amz-SignedHeaders=host&X-Amz-Signature=618deb64019fbd08450a5b0dc842810c294fd9f7bafb22f3cb29046d2b1379db\": EOF, retrying in 1s"
time=2024-05-27T08:42:04.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 33 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:42:19.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 5 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:42:23.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 27 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:43:12.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 44 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:43:20.893Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 14 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:43:20.893Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 39 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:43:22.717Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 45 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:43:27.896Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 12 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:43:52.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 10 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:43:58.895Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 14 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:44:05.896Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 27 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:44:15.897Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 28 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:44:18.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 40 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:44:22.894Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 6 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:44:33.898Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 27 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:44:41.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 8 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:45:21.901Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 28 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:45:31.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 3 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:45:34.895Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 16 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:46:10.187Z level=INFO source=download.go:178 msg="6a0746a1ec1a part 11 attempt 0 failed: unexpected EOF, retrying in 1s"
time=2024-05-27T08:46:12.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 15 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:46:15.188Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 11 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:46:43.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 35 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:47:00.458Z level=INFO source=download.go:178 msg="6a0746a1ec1a part 31 attempt 0 failed: unexpected EOF, retrying in 1s"
time=2024-05-27T08:47:10.895Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 1 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:47:30.896Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 10 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:47:38.824Z level=INFO source=download.go:178 msg="6a0746a1ec1a part 9 attempt 0 failed: unexpected EOF, retrying in 1s"
time=2024-05-27T08:47:40.431Z level=INFO source=download.go:178 msg="6a0746a1ec1a part 32 attempt 0 failed: unexpected EOF, retrying in 1s"
time=2024-05-27T08:47:45.433Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 32 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:47:52.892Z level=INFO source=download.go:251 msg="6a0746a1ec1a part 17 stalled; retrying. If this persists, press ctrl-c to exit, then 'ollama pull' to find a faster connection."
time=2024-05-27T08:49:39.094Z level=INFO source=download.go:136 msg="downloading 4fa551d4f938 in 1 12 KB part(s)"
time=2024-05-27T08:49:42.828Z level=INFO source=download.go:136 msg="downloading 8ab4849b038c in 1 254 B part(s)"
time=2024-05-27T08:49:47.583Z level=INFO source=download.go:136 msg="downloading 577073ffcc6c in 1 110 B part(s)"
time=2024-05-27T08:49:51.368Z level=INFO source=download.go:136 msg="downloading 3f8eb4da87fa in 1 485 B part(s)"
[GIN] 2024/05/27 - 08:49:59 | 200 |         8m35s |       127.0.0.1 | POST     "/api/pull"
[GIN] 2024/05/27 - 08:49:59 | 200 |    1.434741ms |       127.0.0.1 | POST     "/api/show"
[GIN] 2024/05/27 - 08:49:59 | 200 |     893.744µs |       127.0.0.1 | POST     "/api/show"
...
```



# Ollama 部署与使用

此时，`Ollama` 已部署 OK。

## 2. 运行 `llama3`

接下来，我们可以进行 `llama3`，具体如下所示：

```
[lugalee@Labs ~ ] % ollama run llama3
pulling manifest 
pulling 6a0746a1ec1a... 100% ▕█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████▏ 4.7 GB                         
pulling 4fa551d4f938... 100% ▕██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████▏  12 KB                         
pulling 8ab4849b038c... 100% ▕██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████▏  254 B                         
pulling 577073ffcc6c... 100% ▕█████████████████████████████████████████████████████████████████████████████████████████████████████████████████████▏  110 B                         
pulling 3f8eb4da87fa... 100% ▕██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████▏  485 B                         
verifying sha256 digest 
writing manifest 
removing any unused layers 
success
```

此时，我们可以与模型进行交互，具体可参考：

```
>>> Why do people at the bottom like to tear each other apart? 为什么底层人就喜欢互撕？
A thought-provoking question!

It's important to note that not everyone at the "bottom" (whatever that means in a specific context) likes
```

## 3. 运行 `Ollama Web-UI`

接下来，我们使用 `Docker` 运行 `Ollama Web-UI` `docker` 容器，以配合我们的 `Ollama` 实例。具体如下所示：

```
[lugalee@Labs ~ ] % docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v ollama-webui:/app/backend/data --name ollama-webui --restart always ghcr.io/ollama-webui/ollama-webui:main
Unable to find image 'ghcr.io/ollama-webui/ollama-webui:main' locally
main: Pulling from ollama-webui/ollama-webui
f546e941f15b: Pull complete 
24935aba99a7: Pull complete 
07b3e0dc751a: Pull complete 
7e0115596a7a: Pull complete 
a66610a3b2a1: Pull complete 
c175d9a7d72d: Pull complete 
118c270ec011: Pull complete 
2560efb5a1fe: Pull complete 
0a3a251ca3a9: Pull complete 
e66887d25b67: Pull complete 
e138e3ef45c8: Pull complete 
ebe686cd15b0: Pull complete 
cbb3a246d52b: Pull complete 
133390a53c46: Pull complete 
5691cbf1cdd3: Pull complete 
Digest: sha256:d5a5c1126b5decbfbfcac4f2c3d0595e0bbf7957e3fcabc9ee802d3bc66db6d2
Status: Downloaded newer image for ghcr.io/ollama-webui/ollama-webui:main
8fa5ed05075bbcc118451d79d10ea6754fa8c51c47e27016949807ccc60cac4e
[lugalee@Labs ~ ] % docker ps
CONTAINER ID   IMAGE                                    COMMAND               CREATED         STATUS         PORTS                                           NAMES
8fa5ed05075b   ghcr.io/ollama-webui/ollama-webui:main   "bash start.sh"       5 seconds ago   Up 4 seconds   0.0.0.0:3000->8080/tcp, :::3000->8080/tcp       ollama-webui
cef2b5f8510c   ollama/ollama                            "/bin/ollama serve"   7 days ago      Up 2 minutes   0.0.0.0:11434->11434/tcp, :::11434->11434/tcp   ollama
[lugalee@Labs ~ ] % docker logs -f 8fa5ed05075b
No WEBUI_SECRET_KEY provided
Generating WEBUI_SECRET_KEY
Loading WEBUI_SECRET_KEY from .webui_secret_key
INFO:     Started server process [1]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8080 (Press CTRL+C to quit)
INFO:     192.168.215.1:48232 - "GET / HTTP/1.1" 200 OK
INFO:     192.168.215.1:48232 - "GET /manifest.json HTTP/1.1" 200 OK
INFO:     192.168.215.1:48233 - "GET /_app/immutable/entry/start.9d275286.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48236 - "GET /_app/immutable/chunks/index.afef706d.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48232 - "GET /_app/immutable/chunks/index.65c87b32.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48235 - "GET /_app/immutable/chunks/singletons.1c028cc1.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48234 - "GET /_app/immutable/chunks/scheduler.f6668761.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48237 - "GET /_app/immutable/entry/app.653e80bf.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48235 - "GET /_app/immutable/assets/Toaster.3a6d0da3.css HTTP/1.1" 200 OK
INFO:     192.168.215.1:48237 - "GET /_app/immutable/chunks/Toaster.svelte_svelte_type_style_lang.4f4e956a.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48233 - "GET /_app/immutable/chunks/constants.c3d2104f.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48234 - "GET /_app/immutable/nodes/0.e5f3822c.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48236 - "GET /_app/immutable/chunks/each.2cd6bd47.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48234 - "GET /_app/immutable/chunks/navigation.613629d5.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48235 - "GET /_app/immutable/chunks/index.c122cac0.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48233 - "GET /_app/immutable/chunks/index.c4d7520e.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48234 - "GET /_app/immutable/nodes/1.ccc6771d.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48236 - "GET /_app/immutable/assets/0.de36d200.css HTTP/1.1" 200 OK
INFO:     192.168.215.1:48232 - "GET /_app/immutable/nodes/2.02d12cc1.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48237 - "GET /_app/immutable/chunks/stores.81ef0f3d.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48235 - "GET /_app/immutable/chunks/FileSaver.min.898eb36f.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48233 - "GET /_app/immutable/chunks/_commonjsHelpers.de833af9.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48237 - "GET /_app/immutable/chunks/index.24d0f459.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48234 - "GET /_app/immutable/chunks/index.69f6ab4e.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48236 - "GET /_app/immutable/chunks/index.15f26e66.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48233 - "GET /_app/immutable/chunks/index.b7ffcff4.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48232 - "GET /_app/immutable/chunks/index.94bf9275.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48235 - "GET /_app/immutable/chunks/Modal.68c1a0a5.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48236 - "GET /_app/immutable/chunks/index.f3d17e4d.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48237 - "GET /_app/immutable/chunks/AdvancedParams.020cee97.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48234 - "GET /_app/immutable/chunks/index.3f5e7a6d.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48233 - "GET /_app/immutable/assets/2.c7820e19.css HTTP/1.1" 200 OK
INFO:     192.168.215.1:48234 - "GET /_app/immutable/nodes/3.f0821a72.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48236 - "GET /_app/immutable/chunks/Tags.4457bfcc.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48235 - "GET /_app/immutable/chunks/index.4987a420.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48237 - "GET /_app/immutable/chunks/dayjs.min.1e504c00.js HTTP/1.1" 200 OK
INFO:     192.168.215.1:48234 - "GET /_app/immutable/assets/index.8a27bb7e.css HTTP/1.1" 200 OK
INFO:     192.168.215.1:48234 - "GET /api/v1/ HTTP/1.1" 200 OK
...
```

此命令将运行名为`ollama-webui`的`Ollama Web-UI`容器，将容器端口8080映射到主机端口3000，并映射一个卷以持久存储文件。我们可以通过访问`http://127.0.0.1:3000/`进行测试访问，具体可参考：

![](img/b61bb08ed88ce38655bfcf6289ab643c.png)

注册完成后，我们在本地创建一个账户，然后进入主页进行模型交互，具体可参考如下：

![](img/59c8904876a45f61b4b9001d892c035f.png)

OK，到此为止，尽情玩吧～

## 参考资料

[1] `https://openreview.net/pdf?id=uAjxFFing2`

[2] `https://microsoft.github.io/autogen/`

Adiós !

Hello folks，我是 Luga，`Traefik` Ambassador，`Jakarta EE` Ambassador， 一个 10 年+ 技术老司机，从 IT 屌丝折腾到码畜，最后到“酱油“架构师。如果你喜欢技术，不喜欢呻吟，那么恭喜你，来对地方了，关注我，共同学习、进步、超越～

## 推荐阅读

• 一文读懂构建多代理的 `AutoGen` 开源框架

• 一文读懂本地运行 `LLM` 的最佳实践方法
