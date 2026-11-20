
# 2023-05-15利用ChatGPT套娃赚10W+

> 来源：[https://xuqiwei1986.feishu.cn/docx/Xor7dsyD7oU3SFxnJqtcuzzmn4b](https://xuqiwei1986.feishu.cn/docx/Xor7dsyD7oU3SFxnJqtcuzzmn4b)
> 🔗 原文链接：`https://articles.zsxq.com/id_m35r2smop4q1.html`

本文建议有编程基础的同学们观看，其他同学们可以观摩！

大家好，我是闲富，坐标杭州，有一家专注科技的软件公司。一直以来，通过硬核科技来进行商业落地，是我和小伙伴们一直努力的方向。公司的人员组成除了普通的前后端研发之后，还有一些逆向以及算法工程师。

ChatGPT最早是我清华的一个朋友在2022年2月份的时候给我提到了，他当时开发了一个小的`demo`，用来自动写文章，当时我就意识到这个非常强大，但是他也提到了难点，光训练数据就有17TB，并且需要非常多的显卡，所以当时这个事情没有做成产品。

当美国人工智能研究实验室OpenAI于2022年11月30日发布的聊天机器人程序ChatGPT的时候，我意识到机会来了！

进行科技商业落地分析的时候，尤其是中美科技市场之间，其实有一个创业的范式，就是把美国的产品在中国复现一边。那么架构一个`openai`的中转网站就迅速的火热起来。另外对于普通人而言，想要快速尝试这个产品，这个时候机会就出现了。

## 发现机会，开干

人工模拟操作之后，发现网站的技术难度还是比较小的，并没有太多指纹设备的检测。

通常解决批量的问题，有两种方式：

直接调用，用`api`调用的方式来进行调用这种方式的有点在于速度很快，缺点是对技术要求非常高，要迅速地能够熟悉平台的算法，另外稳定性较差，一旦平台升级，需要重新来。

通过`RPA`等方式，模拟人工操作的方式，这种方式的优势是直接忽视技术更新，缺点是速度比较慢，不过稳定性强，无视平台的技术升级。

大概分析了一下`openai`的请求代码，发现其中还是有一些加密和环境参数的，所以迅速的采用第二种方式。

重点来了，如果想要批量操作，需要解决以下三个问题：

架构一个可以无限接受邮件的邮箱，类似于网上的10分钟邮箱

通过`RPA`等技术完成 真人检测，表单填写 按钮发送等功能

解决 国外`SMSCode`网站和 国外`ip`的问题。

第三个问题比较好解决，团队在这一块也有些积累。评论区可以分享给到大家。

### 临时邮箱

好，先来分析第一个问题，如何架构一个可以无限接受邮件的邮箱。其实这个需求本身就是一个可以赚钱的项目。

因为在现实中，大家为了不让自己的 邮箱收到过多的垃圾邮件，所以会使用这类邮箱来注册一些小的网站的注册，所以就催生了这类需求。

比如：`https://10minutemail.info/`

比如：`https://www.mailtemp.net/`

这类产品一般是免费给大家使用，然后网上也接了很多google广告等等。

关于通过搭建临时邮箱如何变现的问题，这一块如果大家感兴趣，我另外开帖子再写。

首先来看搭建之后的效果。

`http://127.0.0.1/get? name=ai4079xx80@bosf23.xyz`

![](img/68c022917b7ccb7c0cee3e92dece46ec.png)

只需要随机`name`和`@`之间的，就可以生成一个可以查看邮件内容的邮箱。理论上，你有无数个这样子的邮箱，不需要注册，也不需要登录，并且这个是可以通过`api`来操作的。

![](img/7745f89d1af3f3bda4b6a97b2c2e5bb8.png)

对了，这里给广大程序员介绍大家平常用不到的语言，易语言。简单介绍下：

> 易语言（EPL）是一门以中文作为程序代码编程语言，其以“易”著称，创始人为 吴涛 。易语言早期版本的名字为E语言，也通常代指与之对应的 集成开发环境 。其最早的版本的发布可追溯至2000年9月11日。创造易语言的初衷是进行用中文来编写程序的实践，方便中国人以中国人的思维编写程序，并不用再去学习西方思维。易语言的诞生极大的降低了编程的门槛和学习的难度。从2000年以来，易语言已经发展到一定的规模，功能上、 用户数量 上都十分可观。

团队之所以选用这个不入流的语言，是因为他能够快速开发客户端界面，并且有非常多的人贡献了很多有用的插件。可以说，它几乎可以完全提到`.net`开发，缺点就是它有点上不得台面，所以很多程序员对他并不熟悉，但是他真的是接单利器，能够非常快速的满足客户的需求。关于以后易语言在科技变现中的经验，我也会另开帖子介绍。

那么无限邮箱的技术原理是什么呢？其实非常简单，有心的程序可以利用Python或者其他语言来实现一个类似的功能。我们来普及一下邮箱的协议，我们来问下ChatGPT。

常见的邮箱协议有以下几种：

`POP3`（Post Office Protocol Version 3）：一种用于接收邮件的协议，通过`POP3`协议，用户可以从邮件服务器上下载自己的邮件到本地计算机。

`IMAP`（Internet Message Access Protocol）：一种用于接收和管理邮件的协议，通过`IMAP`协议，用户可以在邮件服务器上管理自己的邮件，包括查看、删除、移动等操作。

`SMTP`（Simple Mail Transfer Protocol）：一种用于发送邮件的协议，通过`SMTP`协议，用户可以将自己本地计算机上的邮件发送到邮件服务器，并由服务器转发给目标邮箱。

`MAPI`（Messaging Application Programming Interface）：一种用于访问邮件客户端程序的`API`接口，可以让开发者通过编程方式与邮件客户端进行交互，实现特定的功能。

因为我们的无限邮箱，并不发邮件，只是接邮件，所以我们就编写一个`IMAP`的邮箱服务器。这一块网上直接有用的东西并不多，我们还是求助于ChatGPT，来了解下`IMAP`服务器的基本原理。

技术套娃开始，用ChatGPT来给我们指明技术的方向！

```python
import threading
import socket

class IMAPServer(threading.Thread):
    def __init__(self, host, port):
        super().__init__()
        self.host = host
        self.port = port

    def run(self):
        # 创建套接字
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server_socket:
            # 绑定地址和端口
            server_socket.bind((self.host, self.port))
            server_socket.listen(5)
            print(f'IMAP server is running on {self.host}:{self.port}')

            while True:
                # 接受客户端连接
                client_socket, client_address = server_socket.accept()
                print(f'Accepted connection from {client_address}')

                # 启动新线程处理客户端请求
                client_thread = threading.Thread(target=self.handle_client, args=(client_socket,))
                client_thread.start()

    def handle_client(self, client_socket):
        # 发送欢迎信息
        client_socket.send(b'* OK IMAP4rev1 server ready\r\n')

        # 循环读取客户端请求
        while True:
            data = client_socket.recv(1024)
            if not data:
                break

            # 解析客户端命令
            command = data.decode().strip()
            print(f'Received command: {command}')

            if command.startswith('LOGIN'):
                # 处理登录命令
                client_socket.send(b'OK LOGIN completed\r\n')
            elif command == 'LOGOUT':
                # 处理注销命令
                client_socket.send(b'OK LOGOUT completed\r\n')
                break
            else:
                # 处理未知命令
                client_socket.send(b'NO Unknown command\r\n')

        # 关闭客户端连接
        client_socket.close()


# 启动IMAP服务器
imap_server = IMAPServer('localhost', 143)
imap_server.start()
```

了解了原理之后，我们就用易语言来进行手写一个`IMAP`协议，并且增加我们需要的功能。动手能力强的程序员，可以自己亲自实践下，编写自己的`IMAP`邮箱服务器，从来实现一个临时邮箱的网站，可以通过接广告变现。

## `RPA`或者填表技术

`rpa`技术在去年和今年都慢慢火了起来，但其实在易语言界，这个功能早都被应用的非常广泛了。其实原理比较简单，就是通过解析`DOM`元素，来识别定位和操作`DOM`元素，进行功能的识别。

`RPA`这块，比较成熟的软件就是影刀。官网是：`https://www.winrobot360.com/`

它的功能非常强大，可以应用于无法律风险的电商数据爬虫，或者是一些常规操作，本质上是一个能够解放双手，提高效率的工具，这一块如果深挖，也是可以接单变现的。之前我们团队也用影刀接到过单子，后续也会分享给到家经验。

易语言填表其实和`RPA`是一样的，打开一个浏览器，然后通过`DOM`定位来操作网页。

![](img/1b6e0410be55032c2e109dfcfef4d824.png)

我们注册ChatGPT的软件截图如下：（不要觉得丑，写的太仓促，能够赚钱的代码就是好代码）

获取邮件验证码：

![](img/09a84410169b4272b647bb72d8ff497a.png)

还有一个是全部的方法的：

![](img/7966f12408917edc16bb6fc5cbfafdd9.png)

能够操作网页的好处也是可以拿到`apikey`的。

因为传统无限邮箱中，是没有邮箱密码的，但是客户会有一个改密码的需求，所以我们有增加了邮箱密码的功能，这样子就支持用户可以接受到ChatGPT的改密码的邮件了。

经过一下午的代码编写和测试之后，软件可以运转起来。

## 商业变现

对于变现这块，就没有太多要讲的了，往后的故事大家就可以自行脑补了。因为有了这个资源，你往后的N多玩法的成本就大大降低了。

其实这个产品衍生出来另外两个变现命题，感兴趣的朋友可以一起共创下！

1.如何通过创建一个无限邮箱网站来广告变现

2.如何通过`RPA`技术来接无风险爬虫单子

## 闲富总结

大家好，我是闲富。始终坚信，科学技术是第一生产力。另外给大家推荐一本好书，这本书对我的影响非常巨大。

李录《文明现代化价值投资与中国》，他在书中其实给我们科技人员指明了一个致富的道路，欢迎交流！

大家对李录可能不熟悉，巴菲特投资比亚迪，就是他牵线搭桥的。比亚迪也是我非常喜欢的一家公司，技术为王，是一家践行通过科技创新并且获得巨大商业成就的公司。

把更牛逼的技术和更广大的市场结合起来，是我们技术人员创富的最佳方式！

以上就是我今天的分享，如果对你有帮助，可以点个赞啊！

### 另外最近在搞无限临时邮箱的网站变现，感兴趣的可以聊聊！
