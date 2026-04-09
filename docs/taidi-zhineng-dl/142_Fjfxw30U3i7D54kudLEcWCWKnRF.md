# Django Notes

> 来源：[https://learning.feishu.cn/docx/VkBsdpBhPoPiLKxpgBscqLkVnbA](https://learning.feishu.cn/docx/VkBsdpBhPoPiLKxpgBscqLkVnbA)

## FAQ

#### settings.DATABASES is improperly configured.

**复现 step**

1.  登录一个带 user/admin 系统的 project

1.  初始化这个项目，并访问任何一个页面。

1.  报错：settings.DATABASES is improperly configured.

Root cause analysis

1.  登录其他 project 之后，cookie 里有 session

1.  访问这个项目的页面时，如果 enable AuthenticationMiddleware，则会先检查 session。

1.  session 应该存在数据库里，而数据库配置错误。所以，抛异常。

**解决方案**

有 2 个方法：

1.  配一个 database，

比如：

```
DATABASES = {
    &#x27;default&#x27;: {
        &#x27;ENGINE&#x27;: &#x27;django.db.backends.sqlite3&#x27;,
        &#x27;NAME&#x27;: os.path.join(BASE_DIR, &#x27;db.sqlite3&#x27;),
    }
}
```

1.  disable AuthenticationMiddleware

```
MIDDLEWARE = [
    &#x27;django.middleware.security.SecurityMiddleware&#x27;,
    &#x27;django.contrib.sessions.middleware.SessionMiddleware&#x27;,
    &#x27;django.middleware.common.CommonMiddleware&#x27;,
    &#x27;django.middleware.csrf.CsrfViewMiddleware&#x27;,
    # &#x27;django.contrib.auth.middleware.AuthenticationMiddleware&#x27;,
    &#x27;django.contrib.messages.middleware.MessageMiddleware&#x27;,
    &#x27;django.middleware.clickjacking.XFrameOptionsMiddleware&#x27;,
]
```