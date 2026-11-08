# Rustdesk客户端编译

> 来源：[https://xodnytdcaw.feishu.cn/docx/WLJgdMrE9oU3x7xBJhvcU7Dvngd](https://xodnytdcaw.feishu.cn/docx/WLJgdMrE9oU3x7xBJhvcU7Dvngd)

## 一.准备工作

### 安装git：[点此下载](https://git-scm.com/)

### 配置github SSHkey

想要正常克隆推送 github源码，我们需要配置github的sshkey，以此对接git和github。任务栏搜索框，搜索git bash并运行。在git bash代码框，依次输入如下代码并执行：

```
#配置 Git 的用户名和邮箱
git config --global user.name &quot;用户名&quot;
git config --global user.email &quot;邮箱&quot;
 
#生成密钥（一直回车直至这个界面）
ssh-keygen -t rsa -C &quot;邮箱&quot;
 
#查看公钥
cat ~/.ssh/id_rsa.pub
```

打开github个人中心-设置，左侧导航点击SSH and GPG keys，再点击New SSH key，标题随意，将git bash窗口公钥复制到key这里，最后点击 这里添加保存。

![](img/233ba66ee425fd977cbad064d8a976ce.png)

克隆代码本地命令：

```
git clone --recurse-submodules SSH地址
```

## 二.Fork Rustdesk仓库

+   [rustdesk](https://github.com/rustdesk/rustdesk)

+   [hbb_common](https://github.com/rustdesk/hbb_common/tree/57c8a23ab970587ea6380943b04dc354020bbe7c)

## 三.修改代码

### 3.1拉取镜像

```
git clone --recurse-submodules &lt;SSH地址&gt; #--recurse-submodules参数，递归克隆子模块
```

![](img/e502d8da32cf4e390d8961bedc5b64be.png)

### 3.2.1修改hbb_common的文件：

#### 集成ID/中继服务器

**src/config.rs**修改 103-104行默认服务器和密钥为你自己的服务器和密钥。

> 也可以通过添加环境变量(安全些)

> **RE**NDEZVOUS_SERVER    对应值域名

> **RS_**PUB_KEY                  对应值key

![](img/f5d4622c80f422571012c8a1cf1097e1.png)

![](img/2c2e6ac622f3061bdb30d9992f35c1a0.png)

#### 固定远程密码

**src/config.rs**找到HARD_SETTINGS字段（71行）将那一整行替换为如下代码块：

```
pub static ref HARD_SETTINGS: RwLock&lt;HashMap&lt;String, String&gt;&gt; = {
        let mut map = HashMap::new();
        map.insert(&quot;password&quot;.to_string(), &quot;&lt;明文填写你要固定的被控端密码&gt;&quot;.to_string());
        RwLock::new(map)
    };
```

配置子模块并创建新分支

```
git checkout -b &lt;新建分支名&gt;
```

推送修改后的代码

```
git add .
git commit -m &quot;改为自己的ID/中继服务器&quot;
git push origin &lt;新建的分支名&gt;  # 推送分支到我们子模块Fork
```

返回主仓库主目录：

```
cd ../../.. 
```

更新子模块的 Commit ID:

```
git add libs/hbb_common  # 提交子模块的新 Commit
git commit -m &quot;更新修改后的子模块&quot;
git push origin &lt;主仓库分支名&gt;
```

提交修改并推送到主仓库：

```
git add .
git commit -m &quot;修改替换API地址&quot;
git push origin &lt;主仓库分支名，默认master&gt;
```

### 3.2 .2修改 rustdesk 仓库代码

> 这里有4个位置需要修改

**.gitmodule****s(把Ur**l换你自己的hbb_common仓库地址)

**github/workflows/flutter-****ci.y****m**l  最后一行false呢改成true

#### 集成API限制控制权限

> 只有登录账号用户后才可以控制

**src/common.rs**  1007行，把“https://admin.rustdesk.com”替换你自己的API服务器地址

#### 隐藏底部就绪处超链接服

**flutter\lib\desktop\pages\**connection_page.dart 使用编辑器打开。找到81-110行，使用如下代码替换。(删除客户端删除底部就绪处超链接服务广告 )

![](img/a2479ae6bf00f0c2a120131250bdcff6.png)

```
    Widget setupServerWidget() =&gt; Flexible(
       child: Offstage(
         offstage: !(!_svcStopped.value &amp;&amp;
             stateGlobal.svcStatus.value == SvcStatus.ready &amp;&amp;
             _svcIsUsingPublicServer.value),
         child: Row(
           crossAxisAlignment: CrossAxisAlignment.center,
           children: [], 
         ),
       ),
     );
```

同步子模块地址

```
git submodule sync
```

提交并推送代码

```
git add .gitmodules
git commit -m &quot;更新所有子模块地址至我的Fork（备注内容）&quot;
git push origin &lt;主仓库分支名，默认master&gt;
```

### 3.3编译客户端

触发 GitHub Action 构建

在本地 rustdesk 仓库中创建一个新 **tag，**例如 1.3.9，请使用 固定格式 (如 1.3.9 或 v1.3.9)，以便于自动匹配构建规则。

```
git tag -a 1.3.9 -m &quot;修改内置服务器和 key&quot;
```

按照如下图所示操作，大概耗时1小时候，就可以去下载。

![](img/1a358845e0ea7cca5c94b6dc52dca847.png)

## 四.个人化定制（🛰️：dqtx33）

| 集成ID/中继服务器 | 20 | **hbb_common ****仓库下的 ****src/config.rs** |
| --- | --- | --- |
| 固定远程密码 | 12 | **hbb_common ****仓库下的 ****src/config.rs** |
| 隐藏底部就绪处超链接服 | 15 | rustdesk\flutter\lib\desktop\pages\connection_page.dart |
| 集成API限制控制权限 | 10 | rustdesk/src/common.rs |
| 隐藏UAC/权限安装提示 | 15 | 安装后打包单文件 |

## 五.参考教程

https://space.bilibili.com/1111740218/lists/3990266?type=season

https://space.bilibili.com/28459251/upload/video

https://www.smianao.com/?s=RustDesk

https://rainss.cn/essay/1460.html

[ID修改脚本（测试无效）](https://www.bilibili.com/video/BV15yy8YhEan/?spm_id_from=333.337.search-card.all.click&vd_source=206031f494850e57fd6c92ace02b1bed)

Ru[st](https://www.bilibili.com/video/BV1MibBz5Emy/?spm_id_from=333.1387.upload.video_card.click&vd_source=206031f494850e57fd6c92ace02b1bed)Desk开源远程桌面自动配置工具  教程