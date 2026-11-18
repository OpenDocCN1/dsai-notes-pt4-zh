
# Django Notes

> 来源：[https://learning.feishu.cn/docx/VkBsdpBhPoPiLKxpgBscqLkVnbA](https://learning.feishu.cn/docx/VkBsdpBhPoPiLKxpgBscqLkVnbA)

## FAQ

### `settings.DATABASES` is improperly configured.

#### 复现 step

1. 登录一个带 user/admin 系统的 project
1. 初始化这个项目，并访问任何一个页面。
1. 报错：`settings.DATABASES` is improperly configured.

#### Root cause analysis

1. 登录其他 project 之后，cookie 里有 session
1. 访问这个项目的页面时，如果 `AuthenticationMiddleware` 启用，则会先检查 session。
1. session 应该存在数据库里，而数据库配置错误。所以，抛异常。

#### 解决方案

有 2 个方法：

1. 配一个 database，

比如：

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

1. disable `AuthenticationMiddleware`

```
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    # 'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```
