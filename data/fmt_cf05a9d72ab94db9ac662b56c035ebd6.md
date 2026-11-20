
# Intern-S1 玩转开源 Coze

> 来源：[https://aicarrier.feishu.cn/docx/G7wBd7cFFoF17kxoLCacJLTpnrh](https://aicarrier.feishu.cn/docx/G7wBd7cFFoF17kxoLCacJLTpnrh)

## 0. 写在开头

上海AI LAB在7月26日发布并开源了通专结合的科学多模态大模型——Intern-S1，在多模态综合能力方面领先InternVL3、Qwen2.5-VL等主流开源模型，同时在面对复杂科学数据的深度理解上表现更加出色。

Coze 是一个低代码快速创建、调试和部署 AI Agent 的平台。也在7月26日正式推出了开源本地部署版（这样看来两者很有缘分呢👨‍❤️💋‍👨），共开源了两个项目：Coze-Studio 一站式 AI Agent 开发工具，提供各类最新大模型和工具、多种开发模式和框架，并提供`Prompt`、`RAG`、`Plugin`、`Workflow`等Agent开发中的核心能力；Coze Loop 则是扣子的 Agent 评估&监控引擎。

相关链接：

1. Intern Large Models（Intern-S1体验页面）
1. [`github.com`](https://github.com/InternLM/Intern-S1)（Intern-S1 github链接）
1. `https://huggingface.co/internlm/Intern-S1-FP8`（HuggingFace链接）
1. `https://modelscope.cn/models/Shanghai_AI_Laboratory/Intern-S1`（ModelScope链接）
1. `https://github.com/coze-dev/coze-studio`（coze项目地址）
1. [Coze开源项目-社区内容分享](https://bytedance.larkoffice.com/docx/DNmTdFokHoRF6CxaG3ScA7KBnSc)（Coze社区文章）

## 开源Coze本地部署及启动

Coze的本地部署版本是借助`Docker`运行的，可避免用户自己反复安装环境，只需要安装`docker`并运行`docker compose`服务，就可以自动拉取所有组件的镜像并运行容器，在本地启动coze服务。

### 1.1 docker及docker-compose安装

如果是直接安装在windows或mac本机，可直接在`https://www.docker.com/products/docker-desktop/`下载安装包，全部选择默认设置即可。但要注意windows须在安装前启动Hyper-V，并按提示重启。win10专业版操作过程如下（如果非专业版遇到启动Hyper-V不成功的情况可以某宝花5块钱买一个激活密钥）：

+ 打开“设置-系统-可选功能”，点击右侧“更多Windows功能”，然后勾选Hyper-V，点确定：

![](img/1dd1f3f5718786f582b10808874f4200.png)

![](img/b52ab2dd083d812a234f8592cfc7b9da.png)

+ 等待系统搜索并安装完成后，点击重启：

![](img/d63ed8e01c98229d574ec791bfadca64.png)

![](img/5546dacbf3e10abcb0e343fb2df3177a.png)

+ 开始菜单的windows管理工具中可找到Hyper-V，运行如下说明安装成功：

![](img/ba614aca0c303b3ebc4ccd556575129f.png)

![](img/eda81473b9446a9f9a54fa388b496148.png)

如果是在linux环境安装（以centOS通过阿里镜像源安装举例），安装脚本参考如下：

```
# 1. 添加基础软件
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
yum install -y yum-utils device-mapper-persistent-data lvm2 wget
# 2. 设置阿里云 Docker 的yum源
vi /etc/yum.repos.d/docker-ce.repo
# 3. 输入以下内容
[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-stable-debuginfo]
name=Docker CE Stable - Debuginfo $basearch
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/debug-$basearch/stable
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-stable-source]
name=Docker CE Stable - Sources
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/source/stable
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-edge]
name=Docker CE Edge - $basearch
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/$basearch/edge
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-edge-debuginfo]
name=Docker CE Edge - Debuginfo $basearch
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/debug-$basearch/edge
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-edge-source]
name=Docker CE Edge - Sources
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/source/edge
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-test]
name=Docker CE Test - $basearch
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/$basearch/test
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-test-debuginfo]
name=Docker CE Test - Debuginfo $basearch
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/debug-$basearch/test
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-test-source]
name=Docker CE Test - Sources
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/source/test
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-nightly]
name=Docker CE Nightly - $basearch
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/$basearch/nightly
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-nightly-debuginfo]
name=Docker CE Nightly - Debuginfo $basearch
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/debug-$basearch/nightly
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

[docker-ce-nightly-source]
name=Docker CE Nightly - Sources
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/source/nightly
enabled=0
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg%

# 4.查看仓库中所有Docker版本
yum list docker-ce --showduplicates | sort -r

# 5. 安装Docker
yum install docker-ce-19.03.11 -y

# 6. 启动并加入开机启动
systemctl start docker
systemctl enable docker
```

完成安装后可在命令行执行`docker --version`或`docker-compose --version`查看安装情况，desktop也可通过客户端左下角状态栏“Running”状态来确认。

### 1.2 源码中配置书生大模型yaml文件

克隆源码

```
git clone https://github.com/coze-dev/coze-studio.git
```

在书生大模型平台获取`API-token`

在书生api token申请界面 `https://internlm.intern-ai.org.cn/api/tokens` 按如下步骤操作，注意每条`Token`只显示一次，要妥善保存：

![](img/0c97f0e5f820e844d9fec21054484b30.png)

参考书生api文档：`https://internlm.intern-ai.org.cn/api/document`

获取另外两个调用参数：模型名及模型url。模型名参考`获取模型列表`，如调用`intern-s1`，可以配置成`intern-s1`或`intern-latest`：

![](img/6715e662daf366b856eb1d06d09d2d23.png)

复制模型配置模板并编辑

由于当前`coze-studio`官方源码中未给出`intern-s1`的配置文件，且`intern-s1`与`doubao-seed-1.6`均支持`curl`调用和`openai` python sdk调用方式，这里我们可以将`doubao-seed-1.6`的模板`backend/conf/model/template/model_template_ark_doubao-seed-1.6.yaml`拷贝至前一级路径并改名为：`backend/conf/model/ark_interns1.yaml`，如下：

```
cd coze-studio
# 复制模型配置模版
cp backend/conf/model/template/model_template_ark_doubao-seed-1.6.yaml backend/conf/model/intern-s1.yaml
```

将部分配置项修改后的代码文件如下所示：
