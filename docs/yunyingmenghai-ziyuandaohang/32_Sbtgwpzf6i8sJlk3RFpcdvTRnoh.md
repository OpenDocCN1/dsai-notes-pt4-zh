
# 网站需要配置哪些DNS记录

> 来源：`https://yunyingmenghai.feishu.cn/docx/X3OidqwyIo4mZSxRUajchj6Fnse`

## 对于一个典型的网站,您通常需要配置以下DNS记录

### A记录 (`IPv4`)

将您的域名指向网站服务器的`IP`地址。

例如：`panhub.fun A 203.0.113.1`

### AAAA记录 (`IPv6`)

如果您的服务器支持`IPv6`,添加这个记录。

例如：`panhub.fun AAAA 2001:db8::1`

### CNAME记录

用于创建域名别名。

例如：`www.panhub.fun CNAME panhub.fun`

### MX记录

用于电子邮件路由。

例如：`panhub.fun MX 10 mail.panhub.fun`

### TXT记录

用于各种验证目的,如`SPF`记录用于邮件验证。

例如：`panhub.fun TXT "v=spf1 include:_spf.google.com ~all"`

### NS记录

指定域名的权威`DNS`服务器。

例如：`panhub.fun NS ns1.yourprovider.com`

`panhub.fun NS ns2.yourprovider.com`

### CAA记录

指定哪些`CA`可以为您的域名颁发`SSL`证书。

例如：`panhub.fun CAA 0 issue "letsencrypt.org"`

### PTR记录

用于反向`DNS`查找,通常由您的`ISP`或主机提供商设置。

### SRV记录

用于指定特定服务的位置,如`VOIP`或`XMPP`。

### DKIM记录

用于电子邮件身份验证,通常是`TXT`记录的一种。

### DMARC记录

也是一种`TXT`记录,用于电子邮件身份验证策略。

## 对于您的网站`panhub.fun`,最基本的设置应包括

### A记录(指向您的服务器IP)

```
主域名A记录：
通常至少需要一条A记录，将您的主域名（例如 `panhub.fun`）指向您的服务器IP地址。
www子域名：
许多网站会为 `www` 子域名添加一条单独的A记录。虽然也可以使用CNAME记录，但直接使用A记录可能会略微提高性能。
例如：`www.panhub.fun A 203.0.113.1`
其他子域名：
如果您有其他子域名（如 `blog.panhub.fun`, `shop.panhub.fun` 等），每个可能需要单独的A记录。
负载均衡：
如果您使用多台服务器进行负载均衡，可能需要多条A记录，每条指向不同的服务器IP。
例如：
`panhub.fun A 203.0.113.1`
`panhub.fun A 203.0.113.2`
地理分布：
对于全球分布的网站，可能需要为不同地区设置不同的A记录。
冗余和故障转移：
为了提高可靠性，有时会配置多个A记录作为备份。
所以，总结来说：
最少配置：至少一条A记录指向您的主域名。
常见配置：两条A记录，一条用于主域名，一条用于www子域名。
更复杂的配置：根据您的具体需求，可能会有多条A记录。
建议：
首先为主域名（`panhub.fun`）配置一条A记录。
考虑为www子域名（`www.panhub.fun`）添加一条A记录或CNAME记录。
根据您的具体需求，考虑是否需要其他A记录。
```

### CNAME记录(www子域名)

```
`CNAME`（`Canonical Name`）记录用于创建域名别名，将一个域名指向另一个域名。以下是配置`CNAME`记录的步骤和一些常见用例：
配置步骤：
登录您的`DNS`管理面板（通常在域名注册商或`DNS`服务提供商处）。
找到添加`DNS`记录的选项，选择添加`CNAME`记录。
填写以下信息：
名称/主机：要创建别名的子域名（例如 `www`）
类型：选择 `CNAME`
值/指向：目标域名（通常是您的主域名）
保存更改。
常见`CNAME`配置示例：
www子域名：
名称：`www`
类型：`CNAME`
值：`panhub.fun.`
这将使 `www.panhub.fun` 指向 `panhub.fun`
子域名指向第三方服务：
如果您使用第三方服务（如博客平台、电子商务平台等），可能需要配置`CNAME`。
例如，对于GitHub Pages：
名称：`blog`
类型：`CNAME`
值：`username.github.io.`
邮件服务：
如果使用第三方邮件服务，可能需要`CNAME`记录。
例如，对于Google Workspace：
名称：`mail`
类型：`CNAME`
值：`ghs.googlehosted.com.`
注意事项：
`CNAME`记录不能用于根域名（例如 `panhub.fun`）。根域名必须使用`A`记录。
一个域名只能有一个`CNAME`记录，因为它会重定向所有的子域名查询。
`CNAME`记录可能会稍微增加`DNS`查询时间，因为需要额外的查询步骤。
如果您使用`CNAME`，确保目标域名有正确的`A`记录。
某些`DNS`提供商可能不要求在值末尾加点（`.`），但添加它通常是个好习惯，以明确指定完整域名。
对于您的网站 `panhub.fun`，最常见的`CNAME`配置可能是：
名称：`www`
类型：`CNAME`
值：`panhub.fun.`
这将确保无论用户输入 `www.panhub.fun` 还是 `panhub.fun`，都能访问到您的网站
```

### MX记录(如果您使用该域名收发邮件)

使用企业🛰️的邮箱

![](img/24fd370da2472f1fb1cf3205fb2f3fbb.png)

### TXT记录(用于各种验证)

企业邮箱配置`TXT`记录

![](img/6b41cef6852249ccc3fe38d473658b15.png)

### CAA记录(用于`Let's Encrypt`证书)

[域名配置`CAA` (证书颁发机构授权)记录以加快`SSL`证书验证过程](https://yunyingmenghai.feishu.cn/wiki/T2KqwsZbaiKiJgk0HIbcJu5rnmh)
